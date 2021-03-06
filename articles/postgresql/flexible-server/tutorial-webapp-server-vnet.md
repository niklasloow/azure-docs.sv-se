---
title: 'Självstudie: Skapa Azure Database for PostgreSQL-flexibel Server och Azure App Service webbapp i samma virtuella nätverk'
description: Snabb starts guide för att skapa en Azure Database for PostgreSQL-flexibel server med webbappar i ett virtuellt nätverk
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 09/22/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 20401a3c96a9a20399c07d1a30370d27f2858e29
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946661"
---
# <a name="tutorial-create-an-azure-database-for-postgresql---flexible-server-with-app-services-web-app-in-virtual-network"></a>Självstudie: skapa en Azure Database for PostgreSQL flexibel server med App Services webbapp i virtuellt nätverk

> [!IMPORTANT]
> Azure Database for PostgreSQL-flexibel Server är i för hands version

Den här självstudien visar hur du skapar en Azure App Service webbapp med Azure Database for PostgreSQL-flexibel Server (för hands version) i ett [virtuellt nätverk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

I den här självstudien kommer du att
>[!div class="checklist"]
> * Skapa en PostgreSQL flexibel server i ett virtuellt nätverk
> * Skapa en webbapp
> * Lägg till webbappen i det virtuella nätverket
> * Ansluta till postgres från webbappen 

## <a name="prerequisites"></a>Förutsättningar

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

Den här artikeln kräver att du kör Azure CLI version 2,0 eller senare lokalt. Kör kommandot `az --version` om du vill se vilken version som är installerad. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI](/cli/azure/install-azure-cli).

Du måste logga in på ditt konto med kommandot [AZ login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) . Observera egenskapen **ID** från kommandoutdata för det motsvarande prenumerationsnamnet.

```azurecli
az login
```

Om du har flera prenumerationer ska du välja lämplig prenumeration där resursen ska debiteras. Välj det specifika prenumerations-ID:t under ditt konto med hjälp av kommandot [az account set](/cli/azure/account). Ersätt egenskapen **prenumerations-ID** från **AZ-inloggnings** resultatet för din prenumeration till plats hållaren för prenumerations-ID.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-a-postgresql-flexible-server-in-a-new-virtual-network"></a>Skapa en PostgreSQL-flexibel server i ett nytt virtuellt nätverk

Skapa en privat flexibel server i ett virtuellt nätverk (VNET) med följande kommando:
```azurecli
az postgres flexible-server create --resource-group myresourcegroup --location westus2
```
Detta kommando utför följande åtgärder, vilket kan ta några minuter:

- Skapa resurs gruppen om den inte redan finns.
- Genererar ett server namn om det inte har angetts.
- Skapa ett nytt virtuellt nätverk för den nya postgreSQL-servern. Anteckna namnet på det virtuella nätverket och under nätet som skapats för servern eftersom du måste lägga till webbappen i samma virtuella nätverk.
- Skapar administratörs användar namn, lösen ord för servern om inget anges.
- Skapar en tom databas med namnet **postgres**

> [!NOTE]
> - Anteckna ditt lösen ord som ska genereras för dig om inget anges. Om du glömmer bort lösen ordet måste du återställa lösen ordet med hjälp av ``` az postgres flexible-server update``` kommandot
> - Om du inte använder App Service-miljön måste du aktivera Tillåt åtkomst från alla Azure IP-adresser med hjälp av det här kommandot. 
>  ```azurecli
>  az postgres flexible-server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
>  ```


## <a name="create-a-web-app"></a>Skapa en webbapp
I det här avsnittet skapar du app Host i App Service app, ansluter den här appen till postgres-databasen och distribuerar sedan koden till den värden. Se till att du är i lagrings platsen för din program kod i terminalen.

Skapa en App Service app (värd processen) med kommandot AZ webapp up

```azurecli
az webapp up --resource-group myresourcegroup --location westus2 --plan testappserviceplan --sku B1 --name mywebapp
```

> [!NOTE]
> - För argumentet--location använder du samma plats som du gjorde för databasen i föregående avsnitt.
> - Ersätt <App-Name> med ett unikt namn i alla Azure (Server slut punkten är https:// \<app-name> . azurewebsites.net). Tillåtna tecken för <App-Name> är A-Z, 0-9 och-. Ett utmärkt mönster är att använda en kombination av företagets namn och en app-ID.

Detta kommando utför följande åtgärder, vilket kan ta några minuter:

- Skapa resurs gruppen om den inte redan finns. (I det här kommandot använder du samma resurs grupp där du skapade databasen tidigare.)
- Skapa App Service plan ```testappserviceplan``` i den grundläggande pris nivån (B1) om den inte finns. --plan och--SKU är valfria.
- Skapa App Service-appen om den inte finns.
- Aktivera standard loggning för appen om den inte redan är aktive rad.
- Ladda upp lagrings platsen med hjälp av ZIP-distribution med build Automation aktiverat.

## <a name="add-the-web-app-to-the-virtual-network"></a>Lägg till webbappen i det virtuella nätverket
Använd **AZ webapp VNet-integration-** kommandot för att lägga till en regional virtuell nätverks integrering i en webapp. Ersätt <VNet-Name> och <namn på undernät> med det virtuella nätverket och under nätets namn som den flexibla servern använder.

```azurecli
az webapp vnet-integration add -g myresourcegroup -n  mywebapp --vnet <vnet-name> --subnet <subnet-name>
```

## <a name="configure-environment-variables-to-connect-the-database"></a>Konfigurera miljövariabler för att ansluta databasen
Med koden nu distribuerad till App Service är nästa steg att ansluta appen till den flexibla servern i Azure. App-koden förväntar sig att hitta databas information i ett antal miljövariabler. Om du vill ange miljövariabler i App Service skapar du "appinställningar" med ```az webapp config appsettings``` kommandot Set.

```azurecli
az webapp config appsettings set --settings DBHOST="<postgres-server-name>.postgres.database.azure.com" DBNAME="postgres" DBUSER="<username>" DBPASS="<password>"
```


- Replace ```postgres-server-name``` , ```username``` , ```password``` för det nyligen skapade flexibla Server kommandot.
- Ersätt <username> och <password> med de autentiseringsuppgifter som kommandot också genererat för dig.
- Resurs gruppen och App-namnet hämtas från de cachelagrade värdena i Azure/config-filen.
- Kommandot skapar inställningar med namnet ```DBHOST``` , ```DBNAME``` , ```DBUSER``` och ```DBPASS``` . Om program koden använder ett annat namn för databas informationen använder du dessa namn för de appinställningar som anges i koden.

## <a name="clean-up-resources"></a>Rensa resurser

Rensa alla resurser som du skapade i självstudien med hjälp av följande kommando. Det här kommandot tar bort alla resurser i resurs gruppen.

```azurecli
az group delete -n myresourcegroup
```


## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Mappa ett befintligt anpassat DNS-namn till Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain)