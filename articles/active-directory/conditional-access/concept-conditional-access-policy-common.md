---
title: Vanliga principer för villkorlig åtkomst – Azure Active Directory
description: Ofta använda principer för villkorlig åtkomst för organisationer
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 07/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbf4d7b9f622ff590eee401544bf62655fd8d096
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86024391"
---
# <a name="common-conditional-access-policies"></a>Vanliga principer för villkorlig åtkomst

[Säkerhets inställningarna](../fundamentals/concept-fundamentals-security-defaults.md) är bra för vissa men många organisationer behöver större flexibilitet än de erbjuder. Många organisationer behöver till exempel möjlighet att utesluta vissa konton, t. ex. deras nödfalls åtkomst eller administrations konton för brytar-glas från principer för villkorlig åtkomst som kräver Multi-Factor Authentication. För dessa organisationer kan de gemensamma principer som hänvisas till i den här artikeln användas.

![Principer för villkorlig åtkomst i Azure Portal](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Konton för nöd åtkomst

Mer information om konton för nöd åtkomst och varför de är viktiga finns i följande artiklar: 

* [Hantera konton för nöd åtkomst i Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Skapa en elastisk strategi för hantering av åtkomst kontroll med Azure Active Directory](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Vanliga principer som distribueras av organisationer

* [Blockera äldre autentisering](howto-conditional-access-policy-block-legacy.md)\*
* [Kräv MFA för administratörer](howto-conditional-access-policy-admin-mfa.md)\*
* [Kräv MFA för Azure-hantering](howto-conditional-access-policy-azure-management.md)\*
* [Kräv MFA för alla användare](howto-conditional-access-policy-all-users-mfa.md)\*

\*De här fyra principerna när de konfigureras tillsammans är härma-funktioner aktiverade av [säkerhets inställningar](../fundamentals/concept-fundamentals-security-defaults.md).

## <a name="additional-policies"></a>Ytterligare principer

* [Inloggnings risk baserad villkorlig åtkomst (kräver Azure AD Premium P2)](howto-conditional-access-policy-risk.md)
* [Användar risk-baserad villkorlig åtkomst (kräver Azure AD Premium P2)](howto-conditional-access-policy-risk-user.md)
* [Kräv betrodd plats för MFA-registrering](howto-conditional-access-policy-registration.md)
* [Blockera åtkomst efter plats](howto-conditional-access-policy-location.md)
* [Kräv kompatibel enhet](howto-conditional-access-policy-compliant-device.md)
* [Blockera åtkomst förutom vissa appar](howto-conditional-access-policy-block-access.md)

## <a name="next-steps"></a>Nästa steg

- [Simulera inloggnings beteende med hjälp av What If verktyget för villkorlig åtkomst.](troubleshoot-conditional-access-what-if.md)

- [Använd endast rapport läge för villkorlig åtkomst för att fastställa effekten av nya princip beslut.](concept-conditional-access-report-only.md)
