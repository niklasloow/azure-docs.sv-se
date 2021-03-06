---
title: DoD effekt nivå 5 skiss exempel
description: Distribuera steg för DoD-effekten nivå 5-exempel, inklusive information om skiss artefakt parameter.
ms.date: 09/17/2020
ms.topic: sample
ms.openlocfilehash: deb6ffa0f886cc6a4a9bea10a38d7cd82e7df8f1
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90978222"
---
# <a name="deploy-the-dod-impact-level-5-blueprint-sample"></a>Distribuera skiss exemplet DoD-effekt nivå 5

Om du vill distribuera ett skiss exempel på skydds avdelningen i Azure skisser på nivå 5 (DoD IL5), måste du vidta följande steg:

> [!div class="checklist"]
> - Skapa en ny skiss från exemplet
> - Markera din kopia av exemplet som **publicerad**
> - Tilldela din kopia av skissen till en befintlig prenumeration

Om du inte har någon Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free) innan du börjar.

## <a name="create-blueprint-from-sample"></a>Skapa skiss från exempel

Börja med att implementera skiss exemplet genom att skapa en ny skiss i din miljö med hjälp av exemplet som ett start verktyg.

1. Välj **Alla tjänster** i den vänstra rutan. Sök efter och välj **Skisser**.

1. Från sidan **komma igång** till vänster väljer du knappen **skapa** under _skapa en skiss_.

1. Leta upp exempel på **DoD effekt nivå 5** under _andra exempel_ och välj **Använd det här exemplet**.

1. Ange _grunderna_ för skiss exemplet:

   - **Skiss namn**: Ange ett namn för din kopia av skiss exemplet för DoD-effekten nivå 5.
   - **Definitions plats**: Använd ellipsen och välj den hanterings grupp där du vill spara din kopia av exemplet.

1. Välj fliken _artefakter_ överst på sidan eller **Nästa: artefakter** längst ned på sidan.

1. Granska listan över artefakter som utgör skiss exemplet. Många av artefakterna har parametrar som vi definierar senare. Välj **Spara utkast** när du är klar med att granska skiss exemplet.

## <a name="publish-the-sample-copy"></a>Publicera exempel kopian

Din kopia av skiss exemplet har nu skapats i din miljö. Den skapas i **utkast** läge och måste **publiceras** innan den kan tilldelas och distribueras. Kopian av skiss exemplet kan anpassas efter din miljö och dina behov, men ändringen kan flyttas bort från justeringen med DoD-effekter på nivå 5.

1. Välj **Alla tjänster** i den vänstra rutan. Sök efter och välj **Skisser**.

1. Välj sidan **skiss definitioner** till vänster. Använd filtren för att hitta din kopia av skiss exemplet och markera det.

1. Välj **publicera skiss** överst på sidan. På den nya sidan till höger anger du en **version** för din kopia av skiss exemplet. Den här egenskapen är användbar för om du gör en ändring senare. Ange **ändrings anteckningar** som "första versionen som publicerats från DoD IL5 skiss-exemplet". Välj sedan **publicera** längst ned på sidan.

## <a name="assign-the-sample-copy"></a>Tilldela exempel kopian

När kopieringen av skiss exemplet har **publicerats**kan den tilldelas en prenumeration i hanterings gruppen som den sparades till. Det här steget är den plats där parametrar tillhandahålls för att göra varje distribution av kopian av skiss exemplet unik.

1. Välj **Alla tjänster** i den vänstra rutan. Sök efter och välj **Skisser**.

1. Välj sidan **skiss definitioner** till vänster. Använd filtren för att hitta din kopia av skiss exemplet och markera det.

1. Välj **tilldela skiss** överst på skiss definitions sidan.

1. Ange parameter värden för skiss tilldelningen:

   - Grundläggande inställningar

     - **Prenumerationer**: Välj en eller flera av de prenumerationer som finns i hanterings gruppen som du sparade din kopia av skiss exemplet till. Om du väljer fler än en prenumeration skapas en tilldelning för varje användning av de angivna parametrarna.
     - **Tilldelnings namn**: namnet fylls i automatiskt för dig baserat på namnet på skissen.
       Ändra vid behov eller lämna det som är.
     - **Plats**: Välj en region som den hanterade identiteten ska skapas i. Azure Blueprint använder den här hanterade identiteten för att distribuera alla artefakter i den tilldelade skissen. Mer information finns i [hanterade identiteter för Azure-resurser](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Ritnings definitions version**: Välj en **publicerad** version av din kopia av skiss exemplet.

   - Lås tilldelning

     Välj skiss lås inställningen för din miljö. Mer information finns i [Låsa skissresurser](../../concepts/resource-locking.md).

   - Hanterad identitet

     Lämna alternativet standard _tilldelade_ hanterade identiteter.

   - Artefakt parametrar

     De parametrar som definieras i det här avsnittet gäller för den artefakt som den definieras under. Dessa parametrar är [dynamiska parametrar](../../concepts/parameters.md#dynamic-parameters) eftersom de definieras när skissen tilldelas. En fullständig lista eller artefakt parametrar och deras beskrivningar finns i [tabellen artefakt parametrar](#artifact-parameters-table).

1. När alla parametrar har angetts väljer du **tilldela** längst ned på sidan. Skiss tilldelningen skapas och artefakt distributionen påbörjas. Distributionen tar ungefär en timme. Du kan kontrol lera statusen för distributionen genom att öppna skiss tilldelningen.

> [!WARNING]
> Tjänsten Azure-ritningar och de inbyggda skiss exemplen är **kostnads fria**. Azure-resurser [priss ätts per produkt](https://azure.microsoft.com/pricing/). Använd [pris kalkylatorn](https://azure.microsoft.com/pricing/calculator/) för att beräkna kostnaden för att köra resurser som distribuerats i skiss exemplet.

## <a name="artifact-parameters-table"></a>Tabellen artefakt parametrar

I följande tabell visas en lista över Skissernas artefakt parametrar:

|Artefakt namn|Artefakt typ|Parameternamn|Beskrivning|
|-|-|-|-|
|DoD:s effektnivå 5|Principtilldelning|Lista över användare som måste ingå i administratörs gruppen för en virtuell Windows-dator|En semikolonavgränsad lista med användare som ska ingå i den lokala gruppen Administratörer; T. ex. administratör; Mina Användare1; myUser2|
|DoD:s effektnivå 5|Principtilldelning|Lista över användare som uteslutits från administratörs gruppen för Windows-datorer|En semikolonavgränsad lista med användare som ska undantas i den lokala gruppen Administratörer; T. ex. administratör; Mina Användare1; myUser2|
|DoD:s effektnivå 5|Principtilldelning|Lista över resurs typer som ska ha diagnostiska loggar aktiverade||
|DoD:s effektnivå 5|Principtilldelning|Log Analytics arbetsyte-ID för VM-agentens rapportering|ID (GUID) för Log Analytics arbets ytan där VM-agenter ska rapportera|
|DoD:s effektnivå 5|Principtilldelning|Lista över regioner där Network Watcher ska aktive ras|Om du vill se en fullständig lista över regioner använder du get-AzLocation,|
|DoD:s effektnivå 5|Principtilldelning|Lägsta TLS-version för Windows-webbservrar|Den lägsta TLS-protokollversion som ska aktive ras på Windows-webbservrar|
|DoD:s effektnivå 5|Principtilldelning|Senaste PHP-version|Den senaste PHP-versionen som stöds för App Services|
|DoD:s effektnivå 5|Principtilldelning|Senaste Java-version|Senaste Java-version som stöds för App Services|
|DoD:s effektnivå 5|Principtilldelning|Senaste Windows python-versionen|Senaste python-version som stöds för App Services|
|DoD:s effektnivå 5|Principtilldelning|Senaste versionen av Linux python|Senaste python-version som stöds för App Services|
|DoD:s effektnivå 5|Principtilldelning|Valfritt: lista över virtuella Windows-avbildningar som stöder Log Analytics agent som ska läggas till i gransknings området|En semikolonavgränsad lista med bilder; T:/Subscriptions/ <subscriptionId> /resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage|
|DoD:s effektnivå 5|Principtilldelning|Valfritt: lista över virtuella Linux-avbildningar som stöder Log Analytics agent som ska läggas till i gransknings området|En semikolonavgränsad lista med bilder; T:/Subscriptions/ <subscriptionId> /resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage|
|DoD:s effektnivå 5|Principtilldelning|Princip påverkan: det måste finnas fler än en ägare som har tilldelats din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Effekt för principen: disk kryptering bör tillämpas på virtuella datorer|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: e-postmeddelande till Prenumerationens ägare för aviseringar med hög allvarlighets grad måste vara aktiverat|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: fjärrfelsökning bör inaktive ras för Function-appar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att ".NET Framework"-versionen är den senaste, om den används som en del av Funktionsapp|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: transparent datakryptering på SQL-databaser ska aktive ras|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: sårbarhets bedömning ska vara aktiverat på dina SQL-hanterade instanser|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att PHP-versionen är den senaste, om den används som en del av API-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: en Azure Active Directory administratör bör tillhandahållas för SQL-servrar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: endast säkra anslutningar till din Redis Cache ska vara aktiverade|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: Endpoint Protection-lösningen bör installeras på virtuella datorers skalnings uppsättningar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: granska obegränsad nätverks åtkomst till lagrings konton|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: avancerade data säkerhets inställningar för SQL-hanterad instans ska innehålla en e-postadress för att ta emot säkerhets aviseringar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: sårbarheter i säkerhets konfiguration på dina virtuella datorers skalnings uppsättningar bör åtgärdas|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: säker överföring till lagrings konton ska vara aktiverat|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: adaptiva program kontroller ska vara aktiverade på virtuella datorer|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Geo-redundant säkerhets kopiering ska aktive ras för Azure Database for PostgreSQL|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att Java-version är den senaste, om den används som en del av webbappen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: högst 3 ägare bör anges för din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: en e-postadress till en säkerhets kontakt måste anges för din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: CORS bör inte tillåta alla resurser att komma åt dina webb program|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: externa konton med Skriv behörighet bör tas bort från din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: externa konton med Läs behörighet bör tas bort från din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: föråldrade konton bör tas bort från din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Funktionsapp bör endast vara tillgängligt via HTTPS|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: se till att "python-version" är den senaste, om den används som en del av webbappen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: se till att "python-version" är den senaste, om den används som en del av Function-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att PHP-versionen är den senaste, om den används som en del av webbappen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: se till att "python-version" är den senaste, om den används som en del av API-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Princip påverkan: sårbarheter bör åtgärdas av en lösning för sårbarhets bedömning|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Geo-redundant säkerhets kopiering ska aktive ras för Azure Database for MySQL|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att ".NET Framework"-versionen är den senaste, om den används som en del av webbappen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: system uppdateringar bör installeras på datorerna|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att Java-version är den senaste, om den används som en del av API-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att "HTTP-version" är den senaste, om den används för att köra webbappen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: den senaste TLS-versionen ska användas i din API-app|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: MFA ska vara aktiverade konton med Skriv behörighet för din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: avancerade data säkerhets inställningar för SQL Server ska innehålla en e-postadress för att ta emot säkerhets aviseringar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att "HTTP-version" är den senaste, om den används för att köra API-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Microsoft IaaSAntimalware-tillägget bör distribueras på Windows-servrar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att Java-version är den senaste, om den används som en del av Function-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: åtkomst via slut punkt mot Internet ska begränsas|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Security Center standard pris nivå ska väljas|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: granska användningen av anpassade RBAC-regler|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: webb programmet bör endast vara tillgängligt via HTTPS|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: granskning på SQL Server måste vara aktive rad|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Log Analytics agenten ska installeras på virtuella datorer|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: DDoS Protection standard ska vara aktive rad|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: MFA ska vara aktiverat på konton med ägar behörigheter för din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att PHP-versionen är den senaste, om den används som en del av Function-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: avancerad data säkerhet ska vara aktiverat på dina SQL-servrar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: avancerad data säkerhet ska aktive ras på SQL-hanterade instanser|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: e-postaviseringar till administratörer och prenumerations ägare ska aktive ras i avancerade data säkerhets inställningar för SQL-hanterad instans|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: övervaka saknade Endpoint Protection i Azure Security Center|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Effekt för principen: just-in-Time-Network Access Control ska tillämpas på virtuella datorer|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: ett telefonnummer till en säkerhets kontakt måste anges för din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Service Fabric kluster bör endast använda Azure Active Directory för klientautentisering|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: API-appen bör endast vara tillgänglig via HTTPS|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: avancerade skydds typer ska anges till alla i avancerade data säkerhets inställningar för SQL-hanterad instans|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Geo-redundant lagring ska vara aktiverat för lagrings konton|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att ".NET Framework"-versionen är den senaste, om den används som en del av API-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: system uppdateringar på Virtual Machine Scale set bör installeras|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: e-postaviseringar till administratörer och prenumerations ägare måste vara aktiverade i avancerade data säkerhets inställningar för SQL Server|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: fjärrfelsökning bör inaktive ras för webb program|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: långsiktig Geo-redundant säkerhets kopiering ska aktive ras för Azure SQL-databaser|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: sårbarheter i säkerhets konfigurationen på dina datorer bör åtgärdas|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: kontrol lera att "HTTP-version" är den senaste, om den används för att köra Function-appen|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: MFA ska vara aktiverat på konton med Läs behörighet för din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: avancerade skydds typer bör anges till alla i avancerade data säkerhets inställningar för SQL Server|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: sårbarheter i säkerhets konfigurationerna för behållaren bör åtgärdas|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Påverkan för principen: fjärrfelsökning bör inaktive ras för API Apps|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: föråldrade konton med ägar behörigheter bör tas bort från din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: sårbarhets bedömning ska vara aktiverat på dina SQL-servrar|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: Log Analytics agenten ska installeras på Virtual Machine Scale Sets|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: den senaste TLS-versionen ska användas i din webbapp|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: externa konton med ägar behörigheter bör tas bort från din prenumeration|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: den senaste TLS-versionen ska användas i Funktionsapp|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: [för hands version]: Kubernetes-tjänster bör uppgraderas till en icke-sårbar Kubernetes-version|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|
|DoD:s effektnivå 5|Principtilldelning|Gällande princip: sårbarheter på SQL-databaser bör åtgärdas|Azure Policy påverkan för den här principen. Mer information om effekter finns i https://aka.ms/policyeffects|

## <a name="next-steps"></a>Nästa steg

Nu när du har granskat stegen för att distribuera DoD-underläggnings nivå 5, kan du gå till följande artiklar om du vill veta mer om skiss-och kontroll mappning:

> [!div class="nextstepaction"]
> [DoD effekt nivå 5 skiss – översikt](./index.md) 
>  [DoD-effekt nivå 5 skiss-kontroll mappning](./control-mapping.md)

Ytterligare artiklar om skisser och hur de används:

- Mer information om [livscykeln för en skiss](../../concepts/lifecycle.md).
- Förstå hur du använder [statiska och dynamiska parametrar](../../concepts/parameters.md).
- Lär dig hur du anpassar [sekvensordningen för en skiss](../../concepts/sequencing-order.md).
- Lär dig hur du använder [resurslåsning för en skiss](../../concepts/resource-locking.md).
- Lär dig hur du [uppdaterar befintliga tilldelningar](../../how-to/update-existing-assignments.md).