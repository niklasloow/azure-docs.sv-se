---
title: Developer-kommentarer för anpassade principer
titleSuffix: Azure AD B2C
description: Anteckningar för utvecklare om hur man konfigurerar och underhåller Azure AD B2C med anpassade principer.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/19/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 23e443a542ed22906abb237e402834b707e623a9
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88852040"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Developer-kommentarer för anpassade principer i Azure Active Directory B2C

Anpassad princip konfiguration i Azure Active Directory B2C är nu allmänt tillgänglig. Den här konfigurations metoden riktar sig till avancerade identitets utvecklare som skapar komplexa identitets lösningar. Anpassade principer gör kraften i det tillgängliga ramverket för identitets miljö i Azure AD B2C klienter.
Avancerade identitets utvecklare som använder anpassade principer bör planera att investera lite tid genom att gå igenom och läsa referens dokument.

De flesta anpassade princip alternativ är nu allmänt tillgängliga, men det finns underliggande funktioner, till exempel tekniska profil typer och API: er för innehålls definition som finns i olika faser i program varu livs cykeln. Många fler kommer. Tabellen nedan anger nivån på tillgänglighet på en mer detaljerad nivå.

## <a name="features-that-are-generally-available"></a>Funktioner som är allmänt tillgängliga

- Skapa och ladda upp användar transporter för anpassad autentisering med hjälp av anpassade principer.
    - Beskriv användar resan steg för steg som utbyten mellan anspråks leverantörer.
    - Definiera villkorlig grenning i användar resor.
- Samverka med REST API-aktiverade tjänster i dina anpassade användar resor för autentisering.
- Federera med identitets leverantörer som är kompatibla med OpenIDConnect-protokollet.
- Federera med identitets leverantörer som följer SAML 2,0-protokollet.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Ansvars områden för anpassad princip funktion – Ställ in utvecklare

Den manuella princip konfigurationen ger lägre åtkomst till den underliggande plattformen av Azure AD B2C och resulterar i att ett unikt förtroende ramverk skapas. Många möjliga permutationer av anpassade identitets leverantörer, förtroende relationer, integreringar med externa tjänster och steg för steg-arbets flöden kräver en metodisk metod för design och konfiguration.

Utvecklare som använder den anpassade princip inställningen bör följa följande rikt linjer:

- Bekanta dig med konfigurations språket för anpassade principer och hantering av nyckel/hemligheter. Mer information finns i [TrustFrameworkPolicy](trustframeworkpolicy.md).
- Bli ägare till scenarier och anpassade integreringar. Dokumentera ditt arbete och informera din Live site-organisation.
- Utför metodiska scenario testning.
- Följ program utvecklings-och mellanlagringsplatsen med bästa praxis med minst en utvecklings-och test miljö och en produktions miljö.
- Håll dig informerad om ny utveckling från identitets leverantörer och tjänster som du integrerar med. Du kan till exempel hålla reda på ändringar i hemligheter och schemalagda och oplanerade ändringar i tjänsten.
- Konfigurera aktiv övervakning och övervaka svars tiderna för produktions miljöer. Mer information om hur du integrerar med Application Insights finns i [Azure Active Directory B2C: samla in loggar](analytics-with-application-insights.md).
- Behåll kontaktens e-postadresser som är aktuella i Azure-prenumerationen och håll dig uppdaterad med e-postmeddelandena från Microsoft Live-site.
- Ta åtgärds tid när du uppmanas att göra det av Microsoft Live-site-teamet.

## <a name="terms-for-features-in-public-preview"></a>Villkor för funktioner i offentlig för hands version

- Vi rekommenderar att du bara använder de offentliga för hands funktionerna i utvärderings syfte.
- Service avtal (service avtal) gäller inte för de offentliga för hands versions funktionerna.
- Support förfrågningar för offentliga för hands versions funktioner kan arkiveras via vanliga Support kanaler.

## <a name="features-by-stage-and-known-issues"></a>Funktioner efter steg och kända problem

Funktioner för anpassad princip/identitets upplevelse Framework är under konstant och snabb utveckling. Följande tabell är ett index över funktioner och tillgänglighet för komponenter.


### <a name="protocols-and-authorization-flows"></a>Protokoll och auktoriseringsarkiv

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
|-------- | :-----------: | :-------: | :--: | ----- |
| [OAuth2 auktoriseringskod](authorization-code-flow.md) |  |  | X |  |
| OAuth2-auktoriseringskod med PKCE |  |  | X | Endast mobil program  |
| [OAuth2 implicit flöde](implicit-flow-single-page-application.md) |  |  | X |  |
| [Autentiseringsuppgifter för OAuth2 resurs ägare](ropc-custom.md) |  | X |  |  |
| [OIDC ansluta](openid-connect.md) |  |  | X |  |
| [SAML2](connect-with-saml-service-providers.md)  |  |  |X  | POST-och omdirigerings bindningar. |
| OAuth1 |  |  |  | Stöds inte. |
| WSFED | X |  |  |  |

### <a name="identify-providers-federation"></a>Identifiera Provider Federation 

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
|-------- | :-----------: | :-------: | :--: | ----- |
| [OpenID Connect](openid-connect-technical-profile.md) |  |  | X | Till exempel Google +.  |
| [OAuth2](oauth2-technical-profile.md) |  |  | X | Till exempel Facebook.  |
| [OAuth1](oauth1-technical-profile.md) |  | X |  | Till exempel Twitter. |
| [SAML2](saml-identity-provider-technical-profile.md) |  |   | X | Till exempel Salesforce, ADFS. |
| WSFED| X |  |  |  |


### <a name="rest-api-integration"></a>REST API-integrering

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
|-------- | :-----------: | :-------: | :--: | ----- |
| [REST API med Basic auth](secure-rest-api.md#http-basic-authentication) |  |  | X |  |
| [REST API med klient certifikats-auth](secure-rest-api.md#https-client-certificate-authentication) |  |  | X |  |
| [REST API med OAuth2 Bearer-auth](secure-rest-api.md#oauth2-bearer-authentication) |  | X |  |  |

### <a name="component-support"></a>Komponent stöd

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
| ------- | :-----------: | :-------: | :--: | ----- |
| [Autentisering av mobil faktor](phone-factor-technical-profile.md) |  |  | X |  |
| [Azure MFA-autentisering](multi-factor-auth-technical-profile.md) |  | X |  |  |
| [Engångslösenord](one-time-password-technical-profile.md) |  | X |  |  |
| [Azure Active Directory](active-directory-technical-profile.md) som lokal katalog |  |  | X |  |
| Azure e-mail Subsystem för e-postverifiering |  |  | X |  |
| [E-postleverantörer från tredje part](custom-email-mailjet.md) |  |X  |  |  |
| [Stöd för flera språk](localization.md)|  |  | X |  |
| [Predikat-validering](predicates.md) |  |  | X | Till exempel komplexitet av lösen ord. |
| [Visa kontroller](display-controls.md) |  |X  |  |  |


### <a name="page-layout-versions"></a>Sidlayoutversioner

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
| ------- | :-----------: | :-------: | :--: | ----- |
| [2.0.0](page-layout.md) |  | X |  |  |
| [1.2.0](page-layout.md) |  | X |  |  |
| [1.1.0](page-layout.md) |  |  | X |  |
| [1.0.0](page-layout.md) |  |  | X |  |
| [Stöd för Java Script](javascript-samples.md) |  | X |  |  |

### <a name="app-ief-integration"></a>App-IEF-integrering

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Frågesträngparametern `domain_hint` |  |  | X | Tillgängligt som anspråk kan skickas till IDP. |
| Frågesträngparametern `login_hint` |  |  | X | Tillgängligt som anspråk kan skickas till IDP. |
| Infoga JSON i användar resa via `client_assertion` | X |  |  | Är föråldrad. |
| Infoga JSON i användar resa som `id_token_hint` |  | X |  | Go-Forward-metoden för att skicka JSON. |
| [Skicka Identity Provider-token till programmet](idp-pass-through-custom.md) |  | X |  | Till exempel från Facebook till app. |

### <a name="session-management"></a>Sessionshantering

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
| ------- | :-----------: | :-------: | :--: | ----- |
| [Standardprovider för SSO-session](custom-policy-reference-sso.md#defaultssosessionprovider) |  |  | X |  |
| [Provider för extern inloggningssession](custom-policy-reference-sso.md#externalloginssosessionprovider) |  |  | X |  |
| [SAML SSO-sessionsbiljett](custom-policy-reference-sso.md#samlssosessionprovider) |  |  | X |  |
| [OAuthSSOSessionProvider](custom-policy-reference-sso.md#oauthssosessionprovider)  |  | X |  |  |
| [Enkel utloggning](session-overview.md#sign-out)  |  | X |  |  |

### <a name="security"></a>Säkerhet

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
|-------- | :-----------: | :-------: | :--: | ----- |
| Princip nycklar – generera, manuell, uppladdning |  |  | X |  |
| Princip nycklar – RSA/cert, hemligheter |  |  | X |  |


### <a name="developer-interface"></a>Gränssnittet för utvecklare

| Funktion | Utveckling | Förhandsgranskning | Allmän tillgänglighet (GA) | Anteckningar |
| ------- | :-----------: | :-------: | :--: | ----- |
| Azure Portal – IEF UX |  |  | X |  |
| Princip uppladdning |  |  | X |  |
| [Application Insights användar resa loggar](troubleshoot-with-application-insights.md) |  | X |  | Används för fel sökning under utveckling.  |
| [Application Insights händelse loggar](application-insights-technical-profile.md) |  | X |  | Används för att övervaka användar flöden i produktionen. |


## <a name="next-steps"></a>Nästa steg

- Kontrol lera [Microsoft Graph åtgärder som är tillgängliga för Azure AD B2C](microsoft-graph-operations.md)
- Lär dig mer om [anpassade principer och skillnaderna med användar flöden](custom-policy-overview.md).
