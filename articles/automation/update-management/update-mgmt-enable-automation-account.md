---
title: Aktivera Azure Automation Uppdateringshantering från Automation-konto
description: Den här artikeln beskriver hur du aktiverar Uppdateringshantering från ett Automation-konto.
services: automation
ms.date: 09/09/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 787338be06c2e30aabb6421a42e7cb3aaabf8a2a
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89669506"
---
# <a name="enable-update-management-from-an-automation-account"></a>Aktivera Uppdateringshantering från ett Automation-konto

Den här artikeln beskriver hur du kan använda ditt Automation-konto för att aktivera [uppdateringshantering](update-mgmt-overview.md) funktionen för virtuella datorer i din miljö, inklusive datorer eller servrar som är registrerade med [Azure Arc-aktiverade servrar](../../azure-arc/servers/overview.md) (för hands version). Om du vill aktivera virtuella Azure-datorer i stor skala måste du aktivera en befintlig virtuell Azure-dator med hjälp av Uppdateringshantering.

> [!NOTE]
> När du aktiverar Uppdateringshantering, stöds bara vissa regioner för att länka en Log Analytics arbets yta och ett Automation-konto. En lista över mappnings par som stöds finns i [region mappning för Automation-konto och Log Analytics-arbetsyta](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration. Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-konto](../index.yml) för att hantera datorer.
* En [virtuell Azure-dator](../../virtual-machines/windows/quick-create-portal.md)eller virtuell dator eller Server registrerad med ARC-aktiverade servrar (för hands version). Icke-virtuella datorer eller servrar i Azure måste ha [Log Analytics agent](../../azure-monitor/platform/log-analytics-agent.md) för Windows eller Linux installerat och rapportering till arbets ytan som är länkad till Automation-kontot uppdateringshantering är aktive rad i. Agenten kan installeras på Arc-aktiverade servrar genom att distribuera [azure Log Analytics VM-tillägget](../../azure-arc/servers/manage-vm-extensions.md) med Azure Arc.

## <a name="sign-in-to-azure"></a>Logga in på Azure

Logga in på [Azure-portalen](https://portal.azure.com).

## <a name="enable-update-management"></a>Aktivera uppdateringshantering

1. I ditt Automation-konto väljer du **uppdaterings hantering** under **uppdaterings hantering**.

2. Välj Log Analytics arbets yta och Automation-konto och välj **Aktivera** för att aktivera uppdateringshantering. Det tar upp till 15 minuter att slutföra installationen.

    ![Aktivera uppdateringshantering](media/update-mgmt-enable-automation-account/onboardsolutions2.png)

## <a name="enable-azure-vms"></a>Aktivera virtuella Azure-datorer

1. Från ditt Automation-konto väljer du **uppdaterings hantering** under **uppdaterings hantering**.

2. Välj **+ Lägg till virtuella Azure-datorer** och välj en eller flera virtuella datorer i listan. Virtuella datorer som inte kan aktive ras är nedtonade och kan inte väljas. Virtuella Azure-datorer kan finnas i vilken region som helst, oavsett platsen för ditt Automation-konto.

3. Välj **Aktivera** för att lägga till de valda virtuella datorerna i dator gruppens sparade sökning efter funktionen.

    ![Aktivera virtuella Azure-datorer](media/update-mgmt-enable-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Aktivera virtuella datorer som inte är från Azure

Datorer som inte i Azure måste läggas till manuellt.

1. Från ditt Automation-konto väljer du **uppdaterings hantering** under **uppdaterings hantering**.

2. Välj **Lägg till icke-Azure-dator**. Den här åtgärden öppnar ett nytt webbläsarfönster med [instruktioner för att installera och konfigurera Log Analytics agent för Windows](../../azure-monitor/platform/log-analytics-agent.md) så att datorn kan börja rapportera till uppdateringshantering. Om du aktiverar en dator som för närvarande hanteras av Operations Manager krävs ingen ny agent. Informationen om arbets ytan läggs till i agent konfigurationen.

## <a name="enable-machines-in-the-workspace"></a>Aktivera datorer i arbets ytan

Manuellt installerade datorer eller datorer som redan rapporterar till din arbets yta måste läggas till Azure Automation för att Uppdateringshantering ska kunna aktive ras.

1. Från ditt Automation-konto väljer du **uppdaterings hantering** under **uppdaterings hantering**.

2. Välj **hantera datorer**. Knappen **hantera datorer** kan vara nedtonad om du tidigare har valt alternativet **Aktivera på alla tillgängliga och framtida datorer**

    ![Sparade sökningar](media/update-mgmt-enable-automation-account/managemachines.png)

3. Om du vill aktivera Uppdateringshantering för alla tillgängliga datorer som rapporterar till arbets ytan väljer du **Aktivera på alla tillgängliga datorer** på sidan hantera datorer. Den här åtgärden inaktiverar kontrollen för att lägga till datorer individuellt. Den här uppgiften lägger till alla namn på de datorer som rapporterar till arbets ytan till den dator grupp som sparade Sök frågan `MicrosoftDefaultComputerGroup` . När det här alternativet är markerat inaktive ras knappen **hantera datorer** .

4. Om du vill aktivera funktionen för alla tillgängliga datorer och framtida datorer väljer du **Aktivera på alla tillgängliga och framtida datorer**. Det här alternativet tar bort den sparade Sök-och omfattnings konfigurationen från arbets ytan och tillåter att funktionen inkluderar alla Azure-och icke-Azure-datorer som för närvarande eller i framtiden rapporterar till arbets ytan. När det här alternativet är markerat inaktive ras knappen **hantera datorer** permanent, eftersom det inte finns någon tillgänglig omfattnings konfiguration.

    > [!NOTE]
    > Eftersom det här alternativet tar bort de sparade sökningarna och omfattnings konfigurationerna i Log Analytics, är det viktigt att ta bort eventuella borttagnings lås på arbets ytan Log Analytics innan du väljer det här alternativet. Om du inte gör det kan inte alternativet ta bort konfigurationerna och du måste ta bort dem manuellt.

5. Om det behövs kan du lägga till omfångs konfigurationerna genom att lägga till den ursprungliga sparade Sök frågan igen. Mer information finns i [begränsa uppdateringshantering distributions omfång](update-mgmt-scope-configuration.md).

6. Om du vill aktivera funktionen för en eller flera datorer väljer du **Aktivera på valda datorer** och väljer **Lägg till** bredvid varje dator. Den här uppgiften lägger till de valda dator namnen i den dator grupp sparade Sök frågan för funktionen.

## <a name="next-steps"></a>Nästa steg

* Om du vill använda Uppdateringshantering för virtuella datorer läser du [Hantera uppdateringar och korrigeringar för dina virtuella datorer](update-mgmt-manage-updates-for-vm.md).

* När du inte längre behöver hantera virtuella datorer eller servrar med Uppdateringshantering kan du läsa [ta bort virtuella datorer från uppdateringshantering](update-mgmt-remove-vms.md).

* Information om hur du felsöker allmänna Uppdateringshantering fel finns i [felsöka uppdateringshantering problem](../troubleshoot/update-management.md).
