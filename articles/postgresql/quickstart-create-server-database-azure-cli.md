---
title: 'Snabb start: skapa server – Azure CLI – Azure Database for PostgreSQL-enskild server'
description: I den här snabb starts guiden skapar du en Azure Database for PostgreSQL-server med hjälp av Azure CLI.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 06/25/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 182226a0bb97c9239f1c0d0dc6e7d4d2b8bdbb1c
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798881"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-server-by-using-the-azure-cli"></a>Snabb start: skapa en Azure Database for PostgreSQL-server med hjälp av Azure CLI

Den här snabb starten visar hur du använder [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) -kommandon i [Azure Cloud Shell](https://shell.azure.com) för att skapa en enda Azure Database for postgresql server på fem minuter. Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

> [!TIP]
> Överväg att använda det enklare [AZ postgres](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) Azure CLI-kommandot som för närvarande finns som för hands version. Prova [snabb](./quickstart-create-server-up-azure-cli.md)starten.

## <a name="prerequisites"></a>Förutsättningar
Den här artikeln kräver att du kör Azure CLI version 2,0 eller senare lokalt. Kör kommandot `az --version` om du vill se vilken version som är installerad. Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI](/cli/azure/install-azure-cli).

Du måste logga in på ditt konto med hjälp av kommandot [AZ login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) . Observera egenskapen **ID** som refererar till **prenumerations-ID** för ditt Azure-konto. 

```azurecli-interactive
az login
```

Välj det aktuella prenumerations-ID: t under ditt konto med hjälp av kommandot  [AZ Account set](/cli/azure/account) . Anteckna **ID-** värdet från **AZ-inloggnings** resultatet som ska användas som värde för **prenumerations** argumentet i kommandot. 

```azurecli
az account set --subscription <subscription id>
```

Om du har flera prenumerationer ska du välja lämplig prenumeration där resursen ska debiteras. Använd [AZ Account List](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list)för att hämta alla dina prenumerationer.

## <a name="create-an-azure-database-for-postgresql-server"></a>Skapa en Azure Database for PostgreSQL-server

Skapa en [Azure-resurs grupp](../azure-resource-manager/management/overview.md) med kommandot [AZ Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) och skapa sedan din postgresql-server i den här resurs gruppen. Du bör ange ett unikt namn. I följande exempel skapas en resursgrupp med namnet `myresourcegroup` på platsen `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

Skapa en [Azure Database for postgresql server](overview.md) genom att använda kommandot [AZ postgres Server Create](/cli/azure/postgres/server) . En server kan innehålla flera databaser.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 
```
Här följer information om föregående argument: 

**Inställning** | **Exempelvärde** | **Beskrivning**
---|---|---
name | mydemoserver | Unikt namn som identifierar din Azure Database for PostgreSQL-Server. Ditt servernamn får bara innehålla gemener, siffror och bindestreck. Det måste innehålla mellan 3 och 63 tecken.
resource-group | myresourcegroup | Namnet på Azure-resurs gruppen.
location | westus | Azure-plats för servern.
admin-user | myadmin | Användar namn för Administratörs inloggning. Det kan inte vara **azure_superuser**, **administratör**, **administratör**, **rot**, **gäst**eller **offentlig**.
admin-password | *säkert lösenord* | Lösen ordet för administratörs användaren. Det måste innehålla 8 till 128 tecken från tre av följande kategorier: engelska versala bokstäver, engelska gemena bokstäver, siffror och icke-alfanumeriska tecken.
sku-name|GP_Gen5_2| Namnet på pris nivån och beräknings konfigurationen. Följ konventionen {pris nivå}_{Compute generation}_{virtuella kärnor} i korthet. Mer information finns i [Azure Database for PostgreSQL prissättning](https://azure.microsoft.com/pricing/details/postgresql/server/).

>[!IMPORTANT] 
>- Standard versionen av PostgreSQL på servern är 9,6. Om du vill se alla versioner som stöds, se [postgresql-versioner som stöds](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions).
>- Se [det här referens dokumentet](https://docs.microsoft.com/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create)om du vill visa alla argument för kommandot **AZ postgres Server Create** .
>- SSL är aktiverat som standard på servern. Mer information om SSL finns i [Konfigurera SSL-anslutning](./concepts-ssl-connection-security.md).

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurera en brandväggsregel på servernivå 
Som standard är den server som du har skapat inte offentligt tillgänglig och skyddas med brand Väggs regler. Du kan konfigurera brand Väggs reglerna på servern med hjälp av kommandot [AZ postgres Server Firewall-Rule Create](/cli/azure/postgres/server/firewall-rule) för att ge din lokala miljö åtkomst att ansluta till servern. 

I följande exempel skapas en brandväggsregel som kallas `AllowMyIP` som tillåter anslutningar från den specifika IP-adressen 192.168.0.1. Ersätt IP-adressen eller intervallet med IP-adresser som motsvarar var du ska ansluta. Om du inte känner till din IP-adress går du till [whatismyipaddress.com](https://whatismyipaddress.com/) för att hämta den.


```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Se till att nätverkets brand vägg tillåter port 5432 för att undvika anslutnings problem. Azure Database for PostgreSQL servrar använder den porten. 

## <a name="get-the-connection-information"></a>Hämta anslutningsinformationen

Ange värd information och autentiseringsuppgifter för att ansluta till servern.

```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mydemoserver
```

Resultatet är i JSON-format. Anteckna värdena för **administratorLogin** och **fullyQualifiedDomainName** .

```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-the-azure-database-for-postgresql-server-by-using-psql"></a>Ansluta till den Azure Database for PostgreSQL servern med hjälp av psql
[Psql](https://www.postgresql.org/docs/current/static/app-psql.html) -klienten är ett populärt alternativ för att ansluta till postgresql-servrar. Du kan ansluta till servern med hjälp av psql med [Azure Cloud Shell](../cloud-shell/overview.md). Du kan också använda psql i din lokala miljö om du har den tillgänglig. En tom databas, **postgres**, skapas automatiskt med en ny postgresql-server. Du kan använda databasen för att ansluta till psql, som du ser i följande kod. 

   ```bash
 psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

> [!TIP]
> Om du föredrar att använda en URL-sökväg för att ansluta till postgres, kodar URL @-tecknet i användar namnet med `%40` . Anslutnings strängen för psql skulle till exempel vara:
>
> ```
> psql postgresql://myadmin%40mydemoserver@mydemoserver.postgres.database.azure.com:5432/postgres
> ```


## <a name="clean-up-resources"></a>Rensa resurser
Om du inte behöver de här resurserna för en annan snabb start eller självstudier kan du ta bort dem genom att köra följande kommando. 

```azurecli-interactive
az group delete --name myresourcegroup
```

Om du bara vill ta bort den nyligen skapade servern kan du köra kommandot [AZ postgres Server Delete](/cli/azure/postgres/server) .

```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med export och import](./howto-migrate-using-export-and-import.md)
> 
> [Distribuera en django-webbapp med PostgreSQL](../app-service/containers/tutorial-python-postgresql-app.md)
>
> [Ansluta till en Node.JS-app](./connect-nodejs.md)

