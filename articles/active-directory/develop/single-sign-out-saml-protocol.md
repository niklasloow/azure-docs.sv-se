---
title: SAML-protokoll för enkel inloggning i Azure
description: I den här artikeln beskrivs SAML-protokollet enkel utloggning i Azure Active Directory
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 07/19/2017
ms.author: kenwith
ms.custom: aaddev
ms.reviewer: paulgarn
ms.openlocfilehash: 1d09355993af96e9e0cd334c57174cdaa771b388
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88118271"
---
# <a name="single-sign-out-saml-protocol"></a>SAML-protokoll för enkel utloggning

Azure Active Directory (Azure AD) stöder en enkel inloggnings profil för SAML 2,0-webbläsare. För att enkel utloggning ska fungera korrekt måste **LogoutURL** för programmet uttryckligen registreras med Azure AD under program registreringen. Azure AD använder LogoutURL för att omdirigera användare när de har loggat ut.

Följande diagram visar arbets flödet för processen för enkel inloggning i Azure AD.

![Arbets flöde för enkel inloggning i Azure AD](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest
Moln tjänsten skickar ett `LogoutRequest` meddelande till Azure AD för att indikera att en session har avslut ATS. Följande utdrag visar ett exempel `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest
Det `LogoutRequest` element som skickas till Azure AD kräver följande attribut:

* `ID`-Detta identifierar utloggnings förfrågan. Värdet för `ID` får inte börja med en siffra. Den typiska metoden är att lägga till **ID** i sträng representationen av ett GUID.
* `Version`– Ange värdet för det här elementet som **2,0**. Det här värdet är obligatoriskt.
* `IssueInstant`– Det här är en `DateTime` sträng med ett UTC-värde (Coordination Universal Time) och tur och [RETUR-format ("o")](/dotnet/standard/base-types/standard-date-and-time-format-strings). Azure AD förväntar sig ett värde av den här typen, men tvingar inte det.

### <a name="issuer"></a>Utfärdare
`Issuer`Elementet i en `LogoutRequest` måste exakt matcha ett av **ServicePrincipalNames** i moln tjänsten i Azure AD. Detta är vanligt vis inställt på **app-ID-URI: n** som anges vid program registrering.

### <a name="nameid"></a>NameID
Värdet för `NameID` elementet måste exakt matcha `NameID` användarens som loggas ut.

## <a name="logoutresponse"></a>LogoutResponse
Azure AD skickar ett `LogoutResponse` som svar på ett- `LogoutRequest` element. Följande utdrag visar ett exempel `LogoutResponse` .

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse
Azure AD anger `ID` - `Version` och- `IssueInstant` värden i- `LogoutResponse` elementet. Den anger också `InResponseTo` elementet till värdet för `ID` attributet för det som har bevarat `LogoutRequest` svaret.

### <a name="issuer"></a>Utfärdare
Azure AD anger det här värdet till `https://login.microsoftonline.com/<TenantIdGUID>/` där \<TenantIdGUID> är klient-ID: t för Azure AD-klienten.

Om du vill utvärdera värdet för `Issuer` elementet använder du värdet för **app-ID-URI: n** som angavs under program registreringen.

### <a name="status"></a>Status
Azure AD använder `StatusCode` elementet i- `Status` elementet för att ange att utloggningen lyckades eller misslyckades. När utloggnings försöket Miss lyckas `StatusCode` kan elementet också innehålla anpassade fel meddelanden.