---
title: Visa trafik på en karta | Microsoft Azure Maps
description: Ta reda på hur du lägger till trafik data i Maps. Lär dig mer om flödes data och se hur du använder Azure Maps Web SDK för att lägga till incident data och Flow-data till Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-js
ms.openlocfilehash: 584db064bf6f6a6b99e6e2d09f6046912cfcd1bf
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91335253"
---
# <a name="show-traffic-on-the-map"></a>Visa trafik på kartan

Det finns två typer av trafik data tillgängliga i Azure Maps:

- Incident data – består av punkt-och linjebaserade data för sådant som konstruktion, väg stängningar och olyckor.
- Flow-data – innehåller mått för trafik flödet på vägarna. Ofta används trafikflödes data för att färga vägarna. Färgerna baseras på hur mycket trafik som saktar ned flödet, i förhållande till hastighets gränsen eller något annat mått. Trafik flödes data i Azure Maps har tre olika mått mått:
    - `relative` – är i förhållande till vägens fria flödes hastighet.
    - `absolute` – är den absoluta hastigheten hos alla fordon på vägen.
    - `relative-delay` – visar områden som är långsammare än den genomsnittliga fördröjningen.

Följande kod visar hur du visar trafik data på kartan.

```javascript
//Show traffic on the map using the traffic options.
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

Nedan visas det fullständiga kod exemplet för ovanstående funktioner.

<br/>

<iframe height='500' scrolling='no' title='Visa trafik på en karta' src='//codepen.io/azuremaps/embed/WMLRPw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Se pennan <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Visa trafik på en karta</a> efter Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) på <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="traffic-overlay-options"></a>Alternativ för trafik överlägg

Med följande verktyg kan du växla mellan olika inställningar för trafik överlägg för att se hur åter givningen förändras. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Alternativ för trafik överlägg" src="//codepen.io/azuremaps/embed/RwbPqRY/?height=700&theme-id=0&default-tab=result" frameborder='no' loading="lazy" loading="lazy" allowtransparency="true" allowfullscreen="true">
Se alternativen för rit <a href='https://codepen.io/azuremaps/pen/RwbPqRY/'>Överlagrings trafik</a> genom Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) på <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="add-traffic-controls"></a>Lägg till trafik kontroller

Det finns två olika trafik kontroller som kan läggas till i kartan. Den första kontrollen `TrafficControl` lägger till en växlings knapp som kan användas för att aktivera och inaktivera trafik. Med alternativen för den här kontrollen kan du ange när trafik inställningarna ska användas när trafik visas. Som standard visar kontrollen relativa trafikflödes-och incident data, men du kan ändra detta om du vill visa absolut trafikflöde och inga incidenter om det behövs. Den andra kontrollen `TrafficLegendControl` lägger till en trafik flödes förklaring till kartan som hjälper användaren att förstå vad färg koderna väg markeringar betyder. Den här kontrollen visas bara på kartan när trafikflödes data visas på kartan och kommer att döljas vid alla andra tillfällen.

Följande kod visar hur du lägger till trafik kontroller på kartan.

```JavaScript
//Att the traffic control toogle button to the top right corner of the map.
map.controls.add(new atlas.control.TrafficControl(), { position: 'top-right' });

//Att the traffic legend control to the bottom left corner of the map.
map.controls.add(new atlas.control.TrafficLegendControl(), { position: 'bottom-left' });
```

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Trafik kontroller" src="https://codepen.io/azuremaps/embed/ZEWaeLJ?height500&theme-id=0&default-tab=js,result&embed-version=2&editable=true" frameborder='no' loading="lazy" loading="lazy" allowtransparency="true" allowfullscreen="true">
Se Penn <a href='https://codepen.io/azuremaps/pen/ZEWaeLJ'>trafik kontroller</a> efter Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) på <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nästa steg

Läs mer om de klasser och metoder som används i den här artikeln:

> [!div class="nextstepaction"]
> [Karta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [TrafficOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions)

Förbättra användar upplevelsen:

> [!div class="nextstepaction"]
> [Mappa interaktion med mus händelser](map-events.md)

> [!div class="nextstepaction"]
> [Skapa en tillgänglig karta](map-accessibility.md)

> [!div class="nextstepaction"]
> [Kod exempel sida](https://aka.ms/AzureMapsSamples)
