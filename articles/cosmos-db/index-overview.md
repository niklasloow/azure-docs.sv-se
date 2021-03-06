---
title: Indexering i Azure Cosmos DB
description: Förstå hur indexering fungerar i Azure Cosmos DB, olika typer av index, till exempel intervall, spatialdata, sammansatta index som stöds.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: tisande
ms.openlocfilehash: 3d07657fc3345ddd8dfadd163dc3c9f957d77af3
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068395"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indexering i Azure Cosmos DB – Översikt

Azure Cosmos DB är en schema oberoende-databas som gör att du kan iterera i ditt program utan att behöva hantera schema-eller index hantering. Som standard indexerar Azure Cosmos DB automatiskt varje egenskap för alla objekt i din [behållare](databases-containers-items.md#azure-cosmos-containers) utan att behöva definiera något schema eller konfigurera sekundära index.

Målet med den här artikeln är att förklara hur Azure Cosmos DB indexerar data samt hur det använder index för att förbättra frågeprestanda. Vi rekommenderar att du går igenom det här avsnittet innan du utforskar hur du anpassar [indexerings principer](index-policy.md).

## <a name="from-items-to-trees"></a>Från objekt till träd

Varje gång ett objekt lagras i en behållare projiceras innehållet som ett JSON-dokument och konverteras sedan till en träd representation. Det innebär att varje egenskap hos objektet visas som en nod i ett träd. En rotnod för pseudo skapas som överordnad till alla de första nivå egenskaperna för objektet. Löv noderna innehåller faktiska skalära värden som utförs av ett objekt.

Anta till exempel följande objekt:

```json
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

Den representeras av följande träd:

:::image type="content" source="./media/index-overview/item-as-tree.png" alt-text="Det föregående objektet representeras som ett träd" border="false":::

Observera hur matriser kodas i trädet: varje post i en matris får en mellanliggande nod som är märkt med indexet för posten i matrisen (0, 1 osv.).

## <a name="from-trees-to-property-paths"></a>Från träd till egenskaps Sök vägar

Anledningen till varför Azure Cosmos DB transformerar objekt i träd beror på att det tillåter att egenskaper refereras till av sina sökvägar i dessa träd. För att hämta sökvägen till en egenskap kan vi gå över trädet från rotnoden till den egenskapen och sammanfoga etiketterna för varje nod.

Här är Sök vägarna för varje egenskap från det exempel objekt som beskrivs ovan:

- /Locations/0/Country: "Tyskland"
- /Locations/0/City: "Berlin"
- /Locations/1/Country: "Frankrike"
- /Locations/1/City: "Paris"
- /Headquarters/Country: "Belgien"
- /Headquarters/Employees: 250
- /exports/0/City: "Moskva"
- /exports/1/City: "Aten"

När ett objekt skrivs, indexerar Azure Cosmos DB effektivt varje egenskaps bana och dess motsvarande värde.

## <a name="index-kinds"></a>Index typer

Azure Cosmos DB stöder för närvarande tre typer av index.

### <a name="range-index"></a>Intervall index

**Intervall** indexet baseras på en ordnad träd struktur. Intervall index typen används för:

- Likhets frågor:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
   ```

   ```sql
   SELECT * FROM c WHERE c.property IN ("value1", "value2", "value3")
   ```

   Likhets matchning för ett mat ris element
   ```sql
    SELECT * FROM c WHERE ARRAY_CONTAINS(c.tags, "tag1")
    ```

- Intervall frågor:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ```
  (fungerar för `>` , `<` , `>=` , `<=` , `!=` )

- Söker efter en egenskap:

   ```sql
   SELECT * FROM c WHERE IS_DEFINED(c.property)
   ```

- Sträng system funktioner:

   ```sql
   SELECT * FROM c WHERE CONTAINS(c.property, "value")
   ```

   ```sql
   SELECT * FROM c WHERE STRINGEQUALS(c.property, "value")
   ```

- `ORDER BY` skickar

   ```sql
   SELECT * FROM container c ORDER BY c.property
   ```

- `JOIN` skickar

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Intervall index kan användas för skalära värden (sträng eller tal).

### <a name="spatial-index"></a>Rums index

Med **rums** index kan du skapa effektiva frågor om geospatiala objekt som-punkter, linjer, polygoner och multipolygoner. Dessa frågor använder ST_DISTANCE ST_WITHIN ST_INTERSECTS nyckelord. Följande är några exempel som använder spatial index typ:

- Geospatiala avstånds frågor:

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Geospatial inom frågor:

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] })
   ```

- Geospatiala skärnings frågor:

   ```sql
   SELECT * FROM c WHERE ST_INTERSECTS(c.property, { 'type':'Polygon', 'coordinates': [[ [31.8, -5], [32, -5], [31.8, -5] ]]  })  
   ```

Rums index kan användas på korrekt formaterade geospatiala [JSON](geospatial.md) -objekt. Punkter, lin Est rings, polygoner och multipolygoner stöds för närvarande.

### <a name="composite-indexes"></a>Sammansatta index

**Sammansatta** index ökar effektiviteten när du utför åtgärder på flera fält. Den sammansatta index typen används för:

- `ORDER BY` frågor om flera egenskaper:

```sql
 SELECT * FROM container c ORDER BY c.property1, c.property2
```

- Frågor med ett filter och `ORDER BY` . Dessa frågor kan använda ett sammansatt index om filter egenskapen har lagts till i- `ORDER BY` satsen.

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' ORDER BY c.property1, c.property2
```

- Frågor med ett filter på två eller flera egenskaper där minst en egenskap är ett likhets filter

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' AND c.property2 > 'value'
```

Så länge ett filter predikat använder en av index typerna, kommer frågesyntaxen att utvärderas först innan resten görs. Om du till exempel har en SQL-fråga som `SELECT * FROM c WHERE c.firstName = "Andrew" and CONTAINS(c.lastName, "Liu")`

* Ovanstående fråga filtreras först efter poster där firstName = "Anders" med hjälp av indexet. Sedan skickar du alla poster för firstName = "Anders" genom en efterföljande pipeline för att utvärdera innehåller filtervärdet.

* Du kan påskynda frågor och undvika fullständig genomsökning av behållare när du använder funktioner som inte använder indexet (t. ex. innehåller) genom att lägga till ytterligare filter-predikat som använder indexet. Ordningen på filter satserna är inte viktig. Frågemotor är ett sätt att ta reda på vilka predikat som är selektivt selektivt och köra frågan på motsvarande sätt.

## <a name="querying-with-indexes"></a>Fråga med index

De sökvägar som extraherades vid indexering av data gör det enkelt att söka efter indexet när en fråga bearbetas. Genom att matcha `WHERE` -satsen i en fråga med listan över indexerade sökvägar är det möjligt att identifiera de objekt som matchar frågespråket mycket snabbt.

Överväg till exempel följande fråga: `SELECT location FROM location IN company.locations WHERE location.country = 'France'` . Frågans predikat (filtrering av objekt, där vilken plats som helst har "Frankrike" som land/region) matchar sökvägen som marker ATS i rött nedan:

:::image type="content" source="./media/index-overview/matching-path.png" alt-text="Matcha en angiven sökväg inom ett träd" border="false":::

> [!NOTE]
> En `ORDER BY` sats som sorteras efter en enskild egenskap behöver *alltid* ett intervall index och kommer att Miss betes om sökvägen den refererar till inte har en. På samma sätt `ORDER BY` behöver en fråga som order by flera egenskaper *alltid* ett sammansatt index.

## <a name="next-steps"></a>Nästa steg

Läs mer om indexering i följande artiklar:

- [Indexeringsprincip](index-policy.md)
- [Hantera indexerings principen](how-to-manage-indexing-policy.md)
