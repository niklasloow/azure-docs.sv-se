---
title: TAN i Azure Cosmos DB frågespråk
description: Lär dig mer om SQL system Function-TAN i Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9d7187ba116067445e835769fc33aa70677ef80b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "78301989"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 Returnerar tangens för den angivna vinkeln, i radianer, i det angivna uttrycket.  
  
## <a name="syntax"></a>Syntax
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argument
  
*numeric_expr*  
   Är ett numeriskt uttryck.  
  
## <a name="return-types"></a>Retur typer
  
  Returnerar ett numeriskt uttryck.  
  
## <a name="examples"></a>Exempel
  
  I följande exempel beräknas tangens för PI ()/2.  
  
```sql
SELECT TAN(PI()/2) AS tan 
```  
  
 Här är resultatuppsättningen.  
  
```json
[{"tan": 16331239353195370 }]  
```  

## <a name="remarks"></a>Kommentarer

Den här system funktionen kommer inte att använda indexet.

## <a name="next-steps"></a>Nästa steg

- [Matematiska funktioner Azure Cosmos DB](sql-query-mathematical-functions.md)
- [System funktioner Azure Cosmos DB](sql-query-system-functions.md)
- [Introduktion till Azure Cosmos DB](introduction.md)
