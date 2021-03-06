---
title: Rollbaserad åtkomst kontroll-Custom Vision
titleSuffix: Azure Cognitive Services
description: I den här artikeln visas hur du konfigurerar rollbaserad åtkomst kontroll för dina Custom Vision-projekt.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 09/11/2020
ms.author: pafarley
ms.openlocfilehash: fc536b3b0c7af113919f274fc4bdd5fad63cbd06
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90057221"
---
# <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Custom Vision stöder rollbaserad åtkomst kontroll i Azure (Azure RBAC), ett auktoriserings system för att hantera individuell åtkomst till Azure-resurser. Med RBAC tilldelar du olika team medlemmar olika nivåer av behörigheter för dina Custom Vision-projekt. Mer information om RBAC finns i [Azure RBAC-dokumentationen](https://docs.microsoft.com/azure/role-based-access-control/).

## <a name="add-role-assignment-to-custom-vision-resource"></a>Lägg till roll tilldelning i Custom Vision resurs

Azure RBAC kan tilldelas till en Custom Vision-resurs. Om du vill bevilja åtkomst till en Azure-resurs lägger du till en roll tilldelning.
1. I [Azure Portal](https://ms.portal.azure.com/)väljer du **alla tjänster**. 
1. Välj sedan **Cognitive Services**och navigera till din speciella Custom vision utbildnings resurs.
   > [!NOTE]
   > Du kan också konfigurera RBAC för hela resurs grupper, prenumerationer eller hanterings grupper. Det gör du genom att välja önskad omfattnings nivå och sedan navigera till önskat objekt (till exempel välja **resurs grupper** och sedan klicka igenom till önskad resurs grupp).
1. Välj **åtkomst kontroll (IAM)** i det vänstra navigerings fönstret.
1. Välj fliken **roll tilldelningar** för att Visa roll tilldelningarna för det här omfånget.
1. Välj **Lägg till**  ->  **Lägg till roll tilldelning**.
1. I list rutan **roll** väljer du den roll som du vill lägga till.
1. Välj en användare, grupp, tjänstens huvud namn eller hanterad identitet i listan **Välj** . Om du inte ser säkerhets objekt i listan kan du skriva in rutan Välj för att söka i katalogen efter visnings namn, e-postadresser och objekt identifierare.
1. Välj **Spara** för att tilldela rollen.

Inom några minuter tilldelas målet den valda rollen i det valda omfånget.

## <a name="custom-vision-role-types"></a>Custom Vision roll typer

Använd följande tabell för att fastställa åtkomst behoven för dina Custom Vision-resurser.

|Roll  |Behörigheter  |
|---------|---------|
|`Cognitive Service Custom Vision Contributor`     | Fullständig åtkomst till projekten, inklusive möjligheten att skapa, redigera eller ta bort ett projekt.        |
|`Cognitive Service Custom Vision Trainer`     | Fullständig åtkomst, förutom möjligheten att skapa eller ta bort ett projekt. Lärare kan visa och redigera projekt och träna, publicera, avpublicera eller exportera modeller.        |
|`Cognitive Service Custom Vision Labeler`     | Möjlighet att ladda upp, redigera eller ta bort utbildnings avbildningar och skapa, lägga till, ta bort eller ta bort taggar. Etiketter kan visa projekt, men kan inte uppdatera något annat än träna bilder och taggar.         |
|`Cognitive Service Custom Vision Deployment`     | Möjlighet att publicera, avpublicera eller exportera modeller. Distribuerare kan visa projekt, men kan inte uppdatera ett projekt, träna bilder eller taggar.        |
|`Cognitive Service Custom Vision Reader`     | Möjlighet att visa projekt. Läsarna kan inte göra några ändringar.        |
