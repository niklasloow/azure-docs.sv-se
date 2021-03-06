---
title: Template Functions-numeric
description: Beskriver de funktioner som används i en Azure Resource Manager mall för att arbeta med siffror.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 00b44d971a487a0bbec27f3fc2d0746cedd6f874
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84677924"
---
# <a name="numeric-functions-for-arm-templates"></a>Numeriska funktioner för ARM-mallar

Resource Manager innehåller följande funktioner för att arbeta med heltal i din Azure Resource Manager-mall (ARM):

* [skapa](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [flyta](#float)
* [int](#int)
* [bekräftat](#max)
* [minimum](#min)
* [rest](#mod)
* [mul](#mul)
* [Build](#sub)

## <a name="add"></a>add

`add(operand1, operand2)`

Returnerar summan av de två angivna heltalen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
|operand1 |Ja |int |Första talet som ska läggas till. |
|operand2 |Ja |int |Det andra numret som ska läggas till. |

### <a name="return-value"></a>Returvärde

Ett heltal som innehåller summan av parametrarna.

### <a name="example"></a>Exempel

I följande [exempel mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) läggs två parametrar till.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| addResult | Int | 8 |

## <a name="copyindex"></a>copyIndex

`copyIndex(loopName, offset)`

Returnerar indexet för en upprepnings slinga.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| loopName | No | sträng | Namnet på slingan för att hämta iterationen. |
| offset |No |int |Talet som ska läggas till i det nollbaserade upprepning svärdet. |

### <a name="remarks"></a>Kommentarer

Den här funktionen används alltid med ett **copy** -objekt. Om inget värde anges för **förskjutning**returneras det aktuella upprepning svärdet. Upprepning svärdet börjar med noll.

Med egenskapen **loopName** kan du ange om copyIndex refererar till en resurs upprepning eller egenskaps upprepning. Om inget värde anges för **loopName**används den aktuella resurs typen iteration. Ange ett värde för **loopName** när du går igenom en egenskap.

Mer information om hur du använder Copy finns i:

* [Resurs upprepning i ARM-mallar](copy-resources.md)
* [Egenskaps upprepning i ARM-mallar](copy-properties.md)
* [Variabel iteration i ARM-mallar](copy-variables.md)
* [Utdata iteration i ARM-mallar](copy-outputs.md)

### <a name="example"></a>Exempel

I följande exempel visas en kopierings slinga och det index värde som ingår i namnet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {}
}
```

### <a name="return-value"></a>Returvärde

Ett heltal som representerar det aktuella indexet för upprepningen.

## <a name="div"></a>div

`div(operand1, operand2)`

Returnerar heltals divisionen av de två angivna heltalen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Talet som delas. |
| operand2 |Ja |int |Det tal som används för att dividera. Kan inte vara 0. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar divisionen.

### <a name="example"></a>Exempel

Följande [exempel mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) delar upp en parameter med en annan parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| divResult | Int | 2 |

## <a name="float"></a>float

`float(arg1)`

Konverterar värdet till ett flytt ALS nummer. Du använder bara den här funktionen när du skickar anpassade parametrar till ett program, till exempel en Logic app.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |sträng eller heltal |Värdet som ska konverteras till ett flytt ALS nummer. |

### <a name="return-value"></a>Returvärde

Ett flytt ALS nummer.

### <a name="example"></a>Exempel

I följande exempel visas hur du använder float för att skicka parametrar till en Logic app:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

## <a name="int"></a>int

`int(valueToConvert)`

Konverterar det angivna värdet till ett heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja |sträng eller heltal |Värdet som ska konverteras till ett heltal. |

### <a name="return-value"></a>Returvärde

Ett heltal av det konverterade värdet.

### <a name="example"></a>Exempel

I följande [exempel mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) konverteras det användardefinierade parametervärdet till Integer.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": {
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| intResult | Int | 4 |

## <a name="max"></a>bekräftat

`max (arg1)`

Returnerar det maximala värdet från en matris med heltal eller en kommaavgränsad lista med heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris med heltal eller kommaavgränsad lista med heltal |Samlingen för att hämta det högsta värdet. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar det maximala värdet från mängden.

### <a name="example"></a>Exempel

I följande [exempel mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) visas hur du använder Max med en matris och en lista med heltal:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

## <a name="min"></a>min

`min (arg1)`

Returnerar det minsta värdet från en matris med heltal eller en kommaavgränsad lista med heltal.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matris med heltal eller kommaavgränsad lista med heltal |Samlingen för att hämta det lägsta värdet. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar det lägsta värdet från mängden.

### <a name="example"></a>Exempel

I följande [exempel mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) visas hur du använder min med en matris och en lista med heltal:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

## <a name="mod"></a>rest

`mod(operand1, operand2)`

Returnerar resten av heltals divisionen med de två angivna heltalen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Talet som delas. |
| operand2 |Ja |int |Talet som används för att dividera, kan inte vara 0. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar resten.

### <a name="example"></a>Exempel

I följande [exempel mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) returneras resten av divisionen av en parameter med en annan parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| modResult | Int | 1 |

## <a name="mul"></a>mul

`mul(operand1, operand2)`

Returnerar multiplikationen av de två angivna heltalen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Första talet som ska multipliceras. |
| operand2 |Ja |int |Det andra talet som ska multipliceras. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar multiplikationen.

### <a name="example"></a>Exempel

Följande [exempel-mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) multiplicerar en parameter med en annan parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

## <a name="sub"></a>Build

`sub(operand1, operand2)`

Returnerar subtraktion av de två angivna heltalen.

### <a name="parameters"></a>Parametrar

| Parameter | Krävs | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| operand1 |Ja |int |Det tal som subtraheras från. |
| operand2 |Ja |int |Talet som subtraheras. |

### <a name="return-value"></a>Returvärde

Ett heltal som representerar subtraktionen.

### <a name="example"></a>Exempel

I följande [exempel-mall](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) subtraheras en parameter från en annan parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

Utdata från föregående exempel med standardvärdena är:

| Name | Typ | Värde |
| ---- | ---- | ----- |
| under resultat | Int | 4 |

## <a name="next-steps"></a>Nästa steg

* En beskrivning av avsnitten i en Azure Resource Manager mall finns i [förstå strukturen och syntaxen för ARM-mallar](template-syntax.md).
* Om du vill iterera ett visst antal gånger när du skapar en typ av resurs, se [skapa flera instanser av resurser i Azure Resource Manager](copy-resources.md).
