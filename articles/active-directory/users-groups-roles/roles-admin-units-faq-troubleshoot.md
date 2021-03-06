---
title: Fel sökning och vanliga frågor och svar om administrativa enheter – Azure Active Directory | Microsoft Docs
description: Undersök administrativa enheter för att bevilja behörigheter med begränsad omfattning i Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22992f66d45b9bdb76383301cccff645c98a74d9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91264787"
---
# <a name="azure-ad-administrative-units-troubleshooting-and-faq"></a>Azure AD-administrativa enheter: fel sökning och vanliga frågor och svar

För mer detaljerad administrativ kontroll i Azure Active Directory (Azure AD) kan du tilldela användare till en Azure AD-roll med en omfattning som är begränsad till en eller flera administrativa enheter (Australien). Exempel på PowerShell-skript för vanliga aktiviteter finns i [arbeta med administrativa enheter](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true).

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**F: Varför kan jag inte skapa en administrativ enhet?**

**A:** Endast en *Global administratör* eller en *privilegie rad roll administratör* kan skapa en administrativ enhet i Azure AD. Kontrol lera att den användare som försöker skapa den administrativa enheten tilldelas antingen rollen *Global administratör* eller *administratör för privilegie rad roll* .

**F: Jag har lagt till en grupp i den administrativa enheten. Varför visas inte grupp medlemmarna?**

**A:** När du lägger till en grupp till den administrativa enheten innebär det inte att alla grupp medlemmar läggs till i den. Användarna måste tilldelas direkt till den administrativa enheten.

**F: Jag har precis lagt till (eller tagit bort) en medlem i den administrativa enheten. Varför visas inte medlemmen (eller fortfarande visas) i användar gränssnittet?**

**A:** Ibland kan bearbetningen av tillägg eller borttagning av en eller flera medlemmar av den administrativa enheten ta några minuter innan de visas på sidan **administrativa enheter** . Alternativt kan du gå direkt till den associerade resursens egenskaper och se om åtgärden har slutförts. Mer information om användare och grupper i Australien finns i [lista administrativa enheter för en användares](roles-admin-units-add-manage-users.md) och [lista över administrativa enheter för en grupp](roles-admin-units-add-manage-groups.md).

**F: Jag är en delegerad lösen ords administratör för en administrativ enhet. Varför kan jag inte återställa en speciell användares lösen ord?**

**A:** Som administratör för en administrativ enhet kan du bara återställa lösen ord för användare som har tilldelats din administrativa enhet. Se till att användaren vars lösen ords återställning inte fungerar tillhör den administrativa enhet som du har tilldelats. Om användaren tillhör samma administrativa enhet men du fortfarande inte kan återställa lösen ordet, kontrollerar du de roller som är tilldelade till användaren. 

En administrativ enhets begränsad administratör kan inte återställa lösen ordet för en användare som har tilldelats en roll med en företagsomfattande omfattning för att förhindra en behörighets höjning.

**F: Varför krävs administrativa enheter? Gick det inte att använda säkerhets grupper som ett sätt att definiera ett omfång?**

**A:** Säkerhets grupper har ett befintligt syfte och en befintlig auktoriserings modell. En *användar administratör*kan till exempel hantera medlemskap i alla säkerhets grupper i Azure AD-organisationen. Rollen kan använda grupper för att hantera åtkomst till program som Salesforce. En *användar administratör* bör inte kunna hantera Delegerings modellen själva, vilket skulle innebära att säkerhets grupper har utökats till stöd för "resurs gruppering"-scenarier. Administrativa enheter, t. ex. organisationsenheter i Windows Server Active Directory, är avsedda att ge ett sätt att administrera en mängd olika katalog objekt. Själva säkerhets grupperna kan vara medlemmar i resurs omfång. Att använda säkerhets grupper för att definiera en uppsättning säkerhets grupper som en administratör kan hantera kan bli förvirrande.

**F: Vad betyder det att lägga till en grupp i en administrativ enhet?**

**A:** Om du lägger till en grupp i en administrativ enhet får du själva gruppen i hanterings omfånget för alla *användar administratörer* som också har begränsad till den administrativa enheten. Användar administratörer för den administrativa enheten kan hantera namnet och medlemskapet i själva gruppen. Den ger inte *användarens administratör* behörighet att hantera användare av gruppen (till exempel för att återställa sina lösen ord). För att ge *användar administratören* möjlighet att hantera användare måste användarna vara direkt medlemmar i den administrativa enheten.

**F: kan en resurs (användare eller grupp) vara medlem i fler än en administrativ enhet?**

**A:** Ja, en resurs kan vara medlem i mer än en administrativ enhet. Resursen kan hanteras av alla organisations omfattande och administrativa enhets administratörer som har behörigheter över resursen.

**F: är administrativa enheter tillgängliga i B2C-organisationer?**

**A:** Nej, administrativa enheter är inte tillgängliga för B2C-organisationer.

**F: är kapslade administrativa enheter som stöds?**

**A:** Nej, kapslade administrativa enheter stöds inte.

**F: är administrativa enheter som stöds i PowerShell och Graph API?**

**S:** Ja. Du hittar stöd för administrativa enheter i [PowerShell-cmdlet-dokumentation](/powershell/module/Azuread/?view=azureadps-2.0-preview&preserve-view=true) och [exempel skript](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0-preview&preserve-view=true).

Hitta stöd för [resurs typen administrativeUnit](/graph/api/resources/administrativeunit?view=graph-rest-beta&preserve-view=true) i Microsoft Graph.

## <a name="next-steps"></a>Nästa steg

- [Begränsa omfång för roller med hjälp av administrativa enheter](directory-administrative-units.md)
- [Hantera administrativa enheter](roles-admin-units-manage.md)