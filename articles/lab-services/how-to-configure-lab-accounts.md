---
title: Konfigurera automatisk avstängning av virtuella datorer i Azure Lab Services
description: Den här artikeln beskriver hur du konfigurerar automatisk avstängning av virtuella datorer i labb kontot.
ms.topic: article
ms.date: 08/17/2020
ms.openlocfilehash: 8647aed0e66993b8a7b8e5c0a42c8ceabbb1fb9e
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798456"
---
# <a name="configure-automatic-shutdown-of-vms-for-a-lab-account"></a>Konfigurera automatisk avstängning av virtuella datorer för ett labb konto

Du kan aktivera flera kostnads styrnings funktioner för automatisk avstängning för att proaktivt förhindra ytterligare kostnader när de virtuella datorerna inte används aktivt. Kombinationen av följande tre funktioner för automatisk avstängning och från koppling fångar upp de flesta fall där användare av misstag lämnar sina virtuella datorer som kör:
 
- Koppla automatiskt bort användare från virtuella datorer som operativ systemet bedömer vara inaktivt (endast Windows).
- Stäng virtuella datorer automatiskt när användare kopplar från (Windows & Linux).
- Stäng av virtuella datorer som har startats automatiskt, men som inte ansluter till dem.

Granska mer information om funktionen för automatisk avstängning i avsnittet [maximera kostnads kontroll med inställningar för automatisk avstängning](cost-management-guide.md#automatic-shutdown-settings-for-cost-control) .

## <a name="enable-automatic-shutdown"></a>Aktivera automatisk avstängning

1. I [Azure Portal](https://portal.azure.com/) går du till **labb konto** sidan.
1. Välj **labb inställningar** på den vänstra menyn.
1. Välj de inställningar för automatisk avstängning som passar ditt scenario.  

    > [!div class="mx-imgBorder"]
    > ![Inställning för automatisk avstängning i labb konto](./media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)
    
    Inställningarna gäller alla labb som skapats i labb kontot. En labbs kapare (lärare) kan åsidosätta den här inställningen på labb nivån. Ändringen av den här inställningen på labb kontot påverkar bara labb som skapas när ändringen har gjorts.

    Avmarkera kryss rutorna på den här sidan om du vill inaktivera inställningen (s). 

## <a name="next-steps"></a>Nästa steg

Information om hur en labb ägare kan konfigurera eller åsidosätta den här inställningen på labb nivån finns i [Konfigurera automatisk avstängning av virtuella datorer för ett labb](how-to-enable-shutdown-disconnect.md)
