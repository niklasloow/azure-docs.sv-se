---
title: Översikt över Azure Arc-aktiverade servrar
description: Lär dig hur du använder Azure Arc-aktiverade servrar för att hantera servrar som ligger utanför Azure som en Azure-resurs.
keywords: Azure Automation, DSC, PowerShell, önskad tillstånds konfiguration, uppdaterings hantering, ändrings spårning, inventering, Runbooks, python, grafisk, hybrid
ms.date: 09/16/2020
ms.topic: overview
ms.openlocfilehash: 9fcac35f943eefb15a200e9b148b22ce67cc3941
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90887545"
---
# <a name="what-is-azure-arc-enabled-servers"></a>Vad är Azure Arc-aktiverade servrar?

Med Azure Arc-aktiverade servrar kan du hantera dina Windows-och Linux-datorer utanför Azure, i företagets nätverk eller annan moln leverantör, på samma sätt som du hanterar virtuella datorer i Azure. När en hybrid dator är ansluten till Azure blir den en ansluten dator och behandlas som en resurs i Azure. Varje ansluten dator har ett resurs-ID, hanteras som en del av en resurs grupp i en prenumeration och fördelar med Azures standard konstruktioner, till exempel Azure Policy och att använda taggar. Tjänste leverantörer som hanterar en kunds lokala infrastruktur kan hantera sina hybrid datorer, precis som de gör i dag med interna Azure-resurser, i flera kund miljöer med [Azure Lighthouse](../../lighthouse/how-to/manage-hybrid-infrastructure-arc.md) med Azure Arc.

För att kunna leverera den här upplevelsen med dina hybrid datorer utanför Azure måste den Azure-anslutna dator agenten installeras på varje dator som du planerar att ansluta till Azure. Den här agenten levererar inga andra funktioner och ersätter inte Azure [Log Analytics-agenten](../../azure-monitor/platform/log-analytics-agent.md). Log Analytics agent för Windows och Linux krävs om du vill övervaka operativ system och arbets belastningar som körs på datorn proaktivt, hantera den med hjälp av Automation-runbooks eller lösningar som Uppdateringshantering eller använda andra Azure-tjänster som [Azure Security Center](../../security-center/security-center-intro.md).

## <a name="supported-scenarios"></a>Scenarier som stöds

När du ansluter datorn till Azure Arc-aktiverade servrar kan du utföra följande konfigurations hanterings-och övervaknings uppgifter:

- Tilldela [Azure policy gäst konfigurationer](../../governance/policy/concepts/guest-configuration.md) med samma erfarenhet som princip tilldelning för virtuella Azure-datorer.

- Rapport om konfigurations ändringar av installerad program vara, Microsoft-tjänster, Windows-register och filer och Linux-daemon på övervakade servrar med hjälp av Azure Automation [ändringsspårning och inventering](../../automation/change-tracking.md).

- Övervaka din anslutna dators prestanda för gäst operativ system och identifiera program komponenter för att övervaka deras processer och beroenden med andra resurser som programmet kommunicerar med hjälp av [Azure Monitor for VMS](../../azure-monitor/insights/vminsights-overview.md).

- Förenkla distributionen med andra Azure-tjänster som Azure Automation [tillstånds konfiguration](../../automation/automation-dsc-overview.md) och Azure Monitor Log Analytics arbets yta med de [Azure VM-tillägg](manage-vm-extensions.md) som stöds för din icke-Azure Windows-eller Linux-dator. Detta inkluderar konfiguration av konfiguration eller program vara efter distribution med hjälp av tillägget för anpassat skript.

- Använd [uppdateringshantering](../../automation/update-management/update-mgmt-overview.md) i Azure Automation för att hantera operativ system uppdateringar för dina Windows-och Linux-servrar. Distribuera först [hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) -rollen och följ sedan stegen för att [Aktivera uppdateringshantering](../../automation/update-management/update-mgmt-enable-portal.md) på din icke-Azure Windows-eller Linux-dator.

- Ta med dina icke-Azure-servrar för hot identifiering och proaktiv övervakning för potentiella säkerhetshot med hjälp av [Azure Security Center](../../security-center/security-center-intro.md).

Loggdata som samlas in och lagras i en Log Analytics-arbetsyta från hybrid datorn innehåller nu egenskaper som är speciella för datorn, till exempel ett resurs-ID. Detta kan användas för att ge stöd åt [resurs Sammanhangs](../../azure-monitor/platform/design-logs-deployment.md#access-mode) logg åtkomst.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Regioner som stöds

En slutgiltig lista över regioner som stöds med Azure Arc-aktiverade servrar finns på sidan [Azure-produkter efter region](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc) .

I de flesta fall ska den plats som du väljer när du skapar installations skriptet vara den Azure-region som är geografiskt närmast din dators plats. Data i vila lagras i den region i Azure som innehåller den region som du anger, vilket även kan påverka ditt val av region om du har data placering krav. Om den Azure-region som datorn är ansluten till påverkas av ett avbrott påverkas inte den anslutna datorn, men hanterings åtgärder som använder Azure kan inte slutföras. I händelse av ett regionalt avbrott, om du har flera platser som stöder en geografiskt redundant tjänst, är det bäst att ansluta datorerna på varje plats till en annan Azure-region.

### <a name="agent-status"></a>Agent status

Den anslutna dator agenten skickar ett vanligt pulsslags meddelande till tjänsten var 5: e minut. Om tjänsten slutar att ta emot dessa pulsslags meddelanden från en dator anses datorn vara offline och statusen ändras automatiskt till **frånkopplad** i portalen inom 15 till 30 minuter. När du tar emot ett efterföljande pulsslags meddelande från den anslutna dator agenten kommer dess status automatiskt att ändras till **ansluten**.

## <a name="next-steps"></a>Nästa steg

Innan du utvärderar eller aktiverar Arc-aktiverade servrar över flera hybrid datorer kan du läsa [Översikt över anslutna dator agenter](agent-overview.md) för att förstå krav, teknisk information om agenten och distributions metoder.
