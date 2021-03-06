---
title: Lägg till Azure Storage (behållare)
description: Lär dig hur du kopplar en anpassad nätverks resurs i en container app i Azure App Service. Dela filer mellan appar, hantera statiskt innehåll via fjärr anslutning och komma åt lokalt, osv.
author: msangapu-msft
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu
zone_pivot_groups: app-service-containers-windows-linux
ms.openlocfilehash: c240399f1368862a969561409371e075a010e8f2
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435617"
---
# <a name="access-azure-storage-as-a-network-share-from-a-container-in-app-service"></a>Åtkomst Azure Storage som en nätverks resurs från en behållare i App Service

::: zone pivot="container-windows"

Den här guiden visar hur du kopplar Azure Storage-filer som en nätverks resurs till en Windows-behållare i App Service. Endast [Azure Files resurser](../storage/files/storage-how-to-use-files-cli.md) och [Premium fil resurser](../storage/files/storage-how-to-create-premium-fileshare.md) stöds. Fördelarna är skyddat innehåll, innehålls portabilitet, åtkomst till flera appar och flera överförings metoder.

::: zone-end

::: zone pivot="container-linux"

Den här guiden visar hur du kopplar Azure Storage till en Linux-behållare App Service. Fördelarna är skyddat innehåll, innehålls portabilitet, beständig lagring, åtkomst till flera appar och flera överförings metoder.

::: zone-end

## <a name="prerequisites"></a>Krav

::: zone pivot="container-windows"

- [En befintlig Windows-container i Azure App Service](quickstart-custom-container.md)
- [Skapa Azure-filresurs](../storage/files/storage-how-to-use-files-cli.md)
- [Ladda upp filer till Azure-filresursen](../storage/files/storage-files-deployment-guide.md)

::: zone-end

::: zone pivot="container-linux"

- En befintlig [App Service i Linux-appen](index.yml).
- Ett [Azure Storage konto](../storage/common/storage-account-create.md?tabs=azure-cli)
- En [Azure-filresurs och katalog](../storage/files/storage-how-to-use-files-cli.md).

::: zone-end

> [!NOTE]
> Azure Files är icke standard lagring och debiteras separat, inte tillsammans med webbappen. Den stöder inte användning av brand Väggs konfiguration på grund av begränsningar i infrastrukturen.
>

## <a name="limitations"></a>Begränsningar

::: zone pivot="container-windows"

- Azure Storage i App Service är **i för hands version** och **stöds inte** för **produktions scenarier**.
- Azure Storage i App Service stöds för närvarande **inte** för att ta med egna kod scenarier (Windows-appar som inte har container).
- Azure Storage i App Service **stöder inte** användning av konfigurationen av **lagrings brand väggen** på grund av infrastruktur begränsningar.
- Azure Storage med App Service kan du ange **upp till fem** monterings punkter per app.
- Azure Storage som är monterade till en app kan inte nås via App Service FTP/FTPs-slutpunkter. Använd [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

::: zone-end

::: zone pivot="container-linux"

- Azure Storage i App Service är **i för hands version** för App Service på Linux och Web App for containers. Det finns **inte stöd** för **produktions scenarier**.
- Azure Storage i App Service stöder montering av **Azure Files behållare** (Läs/skriv) och **Azure Blob-behållare** (skrivskyddat)
- Azure Storage i App Service **stöder inte** användning av konfigurationen av **lagrings brand väggen** på grund av infrastruktur begränsningar.
- Azure Storage i App Service kan du ange **upp till fem** monterings punkter per app.
- Azure Storage som är monterade till en app kan inte nås via App Service FTP/FTPs-slutpunkter. Använd [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

::: zone-end

## <a name="link-storage-to-your-app"></a>Länka lagring till din app

::: zone pivot="container-windows"

När du har skapat ditt [Azure Storage-konto, fil resurs och katalog](#prerequisites)kan du nu konfigurera din app med Azure Storage.

Om du vill montera en Azure Files resurs till en katalog i App Service-appen använder du [`az webapp config storage-account add`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) kommandot. Lagrings typen måste vara migreringsåtgärden.

```azurecli
az webapp config storage-account add --resource-group <group-name> --name <app-name> --custom-id <custom-id> --storage-type AzureFiles --share-name <share-name> --account-name <storage-account-name> --access-key "<access-key>" --mount-path <mount-path-directory of form c:<directory name> >
```

Du bör göra detta för alla andra kataloger som ska länkas till en Azure Files-resurs.

::: zone-end

::: zone pivot="container-linux"

När du har skapat ditt [Azure Storage-konto, fil resurs och katalog](#prerequisites)kan du nu konfigurera din app med Azure Storage.

Om du vill montera ett lagrings konto till en katalog i App Service-appen använder du [`az webapp config storage-account add`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) kommandot. Lagrings typen kan vara AzureBlob eller migreringsåtgärden. Migreringsåtgärden används i det här exemplet. Inställningen monterings Sök väg motsvarar den mapp som du vill montera från Azure Storage. Om den ställs in på "/" monteras hela Azure Storage.


> [!CAUTION]
> Katalogen som anges som monterings Sök väg i din webbapp måste vara tom. Innehåll som lagras i den här katalogen tas bort när en extern montering läggs till. Om du migrerar filer för en befintlig app, gör en säkerhets kopia av din app och dess innehåll innan du börjar.
>

```azurecli
az webapp config storage-account add --resource-group <group-name> --name <app-name> --custom-id <custom-id> --storage-type AzureFiles --share-name <share-name> --account-name <storage-account-name> --access-key "<access-key>" --mount-path <mount-path-directory>
```

Du bör göra detta för alla andra kataloger som ska länkas till ett lagrings konto.

::: zone-end

## <a name="verify-linked-storage"></a>Verifiera länkad lagring

När resursen är länkad till appen kan du kontrol lera detta genom att köra följande kommando:

```azurecli
az webapp config storage-account list --resource-group <resource-group> --name <app-name>
```

## <a name="next-steps"></a>Nästa steg

::: zone pivot="container-windows"

- [Migrera anpassad program vara till Azure App Service med hjälp av en anpassad behållare](tutorial-custom-container.md?pivots=container-windows).

::: zone-end

::: zone pivot="container-linux"

- [Konfigurera en anpassad behållare](configure-custom-container.md?pivots=platform-linux).

::: zone-end
