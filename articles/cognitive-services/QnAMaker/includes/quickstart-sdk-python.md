---
title: 'Snabb start: QnA Maker klient bibliotek för python'
description: Den här snabb starten visar hur du kommer igång med QnA Maker-klient biblioteket för python.
ms.topic: include
ms.date: 06/18/2020
ms.openlocfilehash: d99ed657098a8046a64c10ae48433aa87c2f06f6
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246489"
---
Använd QnA Maker klient bibliotek för python för att:

* Skapa en kunskaps databas
* Uppdatera en kunskaps databas
* Publicera en kunskaps databas
* Hämta slut punkts nyckel för förutsägelse körning
* Vänta på aktivitet som körs
* Hämta en kunskaps databas
* Få svar
* Ta bort kunskaps bas

[Referens dokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker?view=azure-python)  |  [Biblioteks käll kod](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-knowledge-qnamaker)  |  [Paket (PyPi)](https://pypi.org/project/azure-cognitiveservices-knowledge-qnamaker/)  |  [Python-exempel](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Krav

* Azure-prenumeration – [skapa en kostnads fritt](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
* När du har en Azure-prenumeration kan du skapa en [QNA Maker resurs](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) i Azure Portal för att hämta din redigerings nyckel och slut punkt. När den har distribuerats väljer **du gå till resurs**.
    * Du behöver nyckeln och slut punkten från den resurs som du skapar för att ansluta ditt program till API för QnA Maker. Du klistrar in nyckeln och slut punkten i koden nedan i snabb starten.
    * Du kan använda den kostnads fria pris nivån ( `F0` ) för att testa tjänsten och senare uppgradera till en betald nivå för produktion.

## <a name="setting-up"></a>Konfigurera

### <a name="install-the-client-library"></a>Installera klient biblioteket

När du har installerat python kan du installera klient biblioteket med:

```console
pip install azure-cognitiveservices-knowledge-qnamaker
```

### <a name="create-a-new-python-application"></a>Skapa ett nytt python-program

Skapa en ny python-fil med namnet `quickstart-file.py` och importera följande bibliotek.

[!code-python[Dependencies](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=Dependencies&highlight=4,5)]

Skapa variabler för resursens Azure-slutpunkt och nyckel.

> [!IMPORTANT]
> Gå till Azure Portal och hitta nyckeln och slut punkten för den QnA Maker resurs som du skapade i kraven. De kommer att finnas på resursens **nyckel-och slut punkts** sida under **resurs hantering**.
> Du behöver hela nyckeln för att skapa din kunskaps databas. Du behöver bara resurs namnet från slut punkten. Formatet är `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`.
> Kom ihåg att ta bort nyckeln från koden när du är klar och publicera den aldrig offentligt. För produktion bör du överväga att använda ett säkert sätt att lagra och komma åt dina autentiseringsuppgifter. Till exempel ger [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) säker nyckel lagring.

[!code-python[Resource variables](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=Resourcevariables)]

## <a name="object-models"></a>Objekt modeller

[QNA Maker](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker?view=azure-python) Maker använder två olika objekt modeller:
* **[QnAMakerClient](#qnamakerclient-object-model)** är objektet för att skapa, hantera, publicera och ladda ned kunskaps listan.
* **[QnAMakerRuntime](#qnamakerruntimeclient-object-model)** är objektet för att fråga kunskaps basen med GenerateAnswer-API: et och skicka nya föreslagna frågor med hjälp av träna API (som en del av den [aktiva inlärningen](../concepts/active-learning-suggestions.md)).

[!INCLUDE [Get KBinformation](./quickstart-sdk-cognitive-model.md)]

### <a name="qnamakerclient-object-model"></a>QnAMakerClient-objektmodellen

Redigerings QnA Makers klienten är ett [QnAMakerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker?view=azure-python) -objekt som autentiserar till Azure med hjälp av Microsoft. rest. ServiceClientCredentials, som innehåller din nyckel.

När klienten har skapats använder du [kunskaps bas](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebaseoperations?view=azure-python) egenskapen för att skapa, hantera och publicera din kunskaps bas.

Hantera din kunskaps bas genom att skicka ett JSON-objekt. För omedelbara åtgärder returnerar en metod vanligt vis ett JSON-objekt som indikerar status. För långvariga åtgärder är svaret åtgärds-ID. Anropa [åtgärderna. get_details](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.operations(class)?view=azure-python#get-details-operation-id--custom-headers-none--raw-false----operation-config-) -metoden med åtgärds-ID för att fastställa [status för begäran](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.operation(class)?view=azure-python).

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient-objektmodellen

Förutsägelse QnA Maker-klienten är ett [QnAMakerRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.runtime.qnamakerruntimeclient?view=azure-python) -objekt som autentiserar till Azure med hjälp av Microsoft. rest. ServiceClientCredentials, som innehåller din förutsägelse körnings nyckel som returneras från klient anropet för [redigering. EndpointKeysOperations. get_keys](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.endpointkeysoperations?view=azure-python#get-keys-custom-headers-none--raw-false----operation-config-) när kunskaps listan har publicerats.

Använd metoden [generate_answer](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.runtime.operations.runtimeoperations?view=azure-python#generate-answer-kb-id--generate-answer-payload--custom-headers-none--raw-false----operation-config-) för att få ett svar från frågans körnings tid.


## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Autentisera klienten för att redigera kunskaps basen

Instansiera en klient med din slut punkt och nyckel. Skapa ett CognitiveServicesCredentials-objekt med din nyckel och Använd den med slut punkten för att skapa ett [QnAMakerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.qnamakerclient?view=azure-python) -objekt.

[!code-python[Authorization to resource key](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=AuthorizationAuthor)]

## <a name="create-a-knowledge-base"></a>Skapa en kunskapsbas

 Använd klient objekt för att hämta ett objekt för [kunskaps bas åtgärder](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations?view=azure-python) .

En kunskaps bas lagrar fråge-och svars par för [CreateKbDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.create_kb_dto?view=azure-python) -objektet från tre källor:

* För **redaktionellt innehåll**använder du [QnADTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.qn_adto?view=azure-python) -objektet.
    * Om du vill använda metadata och Uppföljnings anvisningar använder du redigerings kontexten eftersom dessa data läggs till på den enskilda QnA-ihopparningen.
* För **filer**använder du [FileDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.file_dto?view=azure-python) -objektet. FileDTO innehåller fil namnet och den offentliga URL: en för att komma åt filen.
* För **URL: er**använder du en lista med strängar som representerar offentliga tillgängliga URL: er.

Anropa metoden [create](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#create-create-kb-payload--custom-headers-none--raw-false----operation-config-) och skicka sedan det returnerade åtgärds-ID: t till metoden [Operations. getDetails](#get-status-of-an-operation) för att avsöka efter status.

Den sista raden i följande kod returnerar kunskaps bas-ID: t från svaret från MonitorOperation.

[!code-python[Create knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=CreateKBMethod&highlight=36,38)]

Se till att inkludera [`_monitor_operation`](#get-status-of-an-operation) funktionen, som refereras till i ovanstående kod, för att kunna skapa en kunskaps bas.

## <a name="update-a-knowledge-base"></a>Uppdatera en kunskapsbas

Du kan uppdatera en kunskaps bas genom att skicka i kunskaps bas-ID: t och en [UpdateKbOperationDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto?view=azure-python) som innehåller objekt för att [lägga till](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_add?view=azure-python), [Uppdatera](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_update?view=azure-python)och [ta bort](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_delete?view=azure-python) DTO till [uppdaterings](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#update-kb-id--update-kb--custom-headers-none--raw-false----operation-config-) metoden. Använd metoden [operation. getDetail](#get-status-of-an-operation) för att avgöra om uppdateringen har slutförts.

[!code-python[Update a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=UpdateKBMethod&highlight=68,69)]

Se till att inkludera [`_monitor_operation`](#get-status-of-an-operation) funktionen, som refereras till i ovanstående kod, för att kunna uppdatera en kunskaps bas.

## <a name="download-a-knowledge-base"></a>Hämta en kunskaps bas

Använd [nedladdnings](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebaseoperations?view=azure-python#download-kb-id--environment--custom-headers-none--raw-false----operation-config-) metoden för att ladda ned databasen som en lista över [QnADocumentsDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.qnadocumentsdto?view=azure-python). Detta motsvarar _inte_ QNA Maker portalens export från **inställnings** sidan eftersom resultatet av den här metoden inte är en TSV-fil.

[!code-python[Download a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=DownloadKB&highlight=2)]

## <a name="publish-a-knowledge-base"></a>Publicera en kunskapsbas

Publicera kunskaps basen med [publicerings](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#publish-kb-id--custom-headers-none--raw-false----operation-config-) metoden. Detta tar den aktuella sparade och utbildade modellen som refereras till av kunskaps bas-ID: t och publicerar den på en slut punkt.

[!code-python[Publish a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=PublishKB&highlight=2)]

## <a name="get-query-runtime-key"></a>Hämta körnings nyckel för fråga

När en kunskaps databas har publicerats behöver du frågans körnings nyckel för att fråga körnings miljön. Detta är inte samma nyckel som används för att skapa det ursprungliga klient objektet.

Använd metoden [EndpointKeysOperations. get_keys](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.endpointkeysoperations?view=azure-python#get-keys-custom-headers-none--raw-false----operation-config-) för att hämta [EndpointKeysDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.endpointkeysdto?view=azure-python) -klassen.

Använd någon av de nyckel egenskaper som returnerades i objektet för att fråga kunskaps listan.

[!code-python[Get query runtime key](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=GetQueryEndpointKey&highlight=2)]


## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Autentisera körningen för att skapa ett svar

Skapa en [QnAMakerRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.runtime.qnamakerruntimeclient?view=azure-python) för att söka i kunskaps basen för att skapa ett svar eller träna från aktiv inlärning.

[!code-python[Authenticate the runtime](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=AuthorizationQuery)]

Använd QnAMakerRuntimeClient för att få svar från kunskapen eller för att skicka nya föreslagna frågor till kunskaps basen för [aktiv inlärning](../concepts/active-learning-suggestions.md).

## <a name="generate-an-answer-from-the-knowledge-base"></a>Generera ett svar från kunskaps basen

Generera ett svar från en publicerad kunskaps bas med hjälp av metoden RuntimeClient. Runtime. generateAnswer. Den här metoden godkänner kunskaps bas-ID: t och QueryDTO. Få åtkomst till ytterligare egenskaper för QueryDTO, till exempel Top och context, som ska användas i din Chat-robot.

[!code-python[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=GenerateAnswer&highlight=5)]

Detta är ett enkelt exempel som frågar kunskaps basen. Om du vill förstå avancerade fråge scenarier kan du läsa mer i [exempel på andra frågor](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer).

## <a name="delete-a-knowledge-base"></a>Ta bort en kunskapsbas

Ta bort kunskaps basen med metoden [Delete](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#delete-kb-id--custom-headers-none--raw-false----operation-config-) med en parameter i kunskaps bas-ID: t.

[!code-python[Delete a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=DeleteKB&highlight=2)]

## <a name="get-status-of-an-operation"></a>Hämta status för en åtgärd

Vissa metoder, till exempel skapa och uppdatera, kan ta tillräckligt lång tid i stället för att vänta på att processen ska slutföras, returneras en [åtgärd](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.operation.operation?view=azure-python) . Använd åtgärds-ID: t från åtgärden att söka (med logiken för omprövning) för att fastställa statusen för den ursprungliga metoden.

_SetTimeout_ -anropet i följande kod block används för att simulera asynkron kod. Ersätt detta med logiken för omprövning.

[!code-python[Monitor an operation](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=MonitorOperation&highlight=7)]

## <a name="run-the-application"></a>Kör programmet

Kör programmet med python-kommandot på snabb starts filen.

```console
python quickstart-file.py
```

Källkoden för det här exemplet finns på [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/QnAMaker/sdk/quickstart.py).