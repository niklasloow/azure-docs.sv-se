---
title: 'VPN Gateway: Azure AD-klient för P2S VPN-anslutningar: Azure AD-autentisering'
description: Lär dig hur du konfigurerar en Azure AD-klient för P2S med öppen VPN-autentisering.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: alzam
ms.openlocfilehash: 74df878baa6ed4e2ba902a73a7830c0b075c58ce
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90981533"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>Skapa en Azure Active Directory-klientorganisation för P2S-anslutningar med OpenVPN-protokoll

När du ansluter till ditt VNet kan du använda certifikatbaserad autentisering eller RADIUS-autentisering. Men när du använder det öppna VPN-protokollet kan du också använda Azure Active Directory autentisering. Den här artikeln hjälper dig att skapa en Azure AD-klient för P2S Open VPN-autentisering.

> [!NOTE]
> Azure AD-autentisering stöds bara för OpenVPN-® protokoll anslutningar och kräver Azure VPN-klienten, som bara är tillgänglig för Windows 10.
>


## <a name="1-verify-azure-ad-tenant"></a><a name="tenant"></a>1. kontrol lera Azure AD-klienten

Kontrol lera att du har en Azure AD-klient. Om du inte har en Azure AD-klient kan du skapa en med hjälp av stegen i artikeln [skapa en ny klient](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) :

* Organisations namn
* Ursprungligt domännamn

Exempel:

   ![Ny Azure AD-klient](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Skapa Azure AD-klient användare

Din Azure AD-klient behöver följande konton: ett globalt administratörs konto och ett huvud användar konto. Huvud användar kontot används som ditt huvud inbäddnings konto (tjänst konto). När du skapar ett användar konto för Azure AD-klienten justerar du katalog rollen för den typ av användare som du vill skapa.

Följ stegen i [den här artikeln](../active-directory/fundamentals/add-users-azure-active-directory.md) för att skapa minst två användare för din Azure AD-klient. Se till att ändra **katalog rollen** för att skapa konto typerna:

* Global administratör
* Användare

## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>3. Aktivera Azure AD-autentisering på VPN-gatewayen

1. Leta upp katalog-ID: t för den katalog som du vill använda för autentisering. Den visas i avsnittet Egenskaper på sidan Active Directory.

    ![Katalog-ID](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. Kopiera katalog-ID:t.

3. Logga in på Azure Portal som en användare som har tilldelats rollen som **Global administratör** .

4. Ge sedan administrativt medgivande. Kopiera och klistra in webb adressen som hör till distributions platsen i webbläsarens Adress fält:

    Offentliga

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Government

    ```
   https://login.microsoftonline.us/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Tyskland

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure Kina 21Vianet

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. Välj det **globala administratörs** kontot om du uppmanas att göra det.

    ![Katalog-ID](./media/openvpn-create-azure-ad-tenant/pick.png)

6. Välj **acceptera** när du uppmanas till detta.

    ![Skärm bild som visar de meddelande behörigheter som har begärts acceptera för din organisation med information och alternativet att acceptera.](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. I **företags program**i Azure AD visas **Azure VPN** i listan.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)
    
8. Om du inte redan har en fungerande punkt-till-plats-miljö, följer du anvisningarna för att skapa en. Se [skapa en punkt-till-plats-VPN](vpn-gateway-howto-point-to-site-resource-manager-portal.md) för att skapa och konfigurera en punkt-till-plats-VPN-gateway. 

    > [!IMPORTANT]
    > Bas-SKU: n stöds inte för OpenVPN.

9. Aktivera Azure AD-autentisering på VPN-gatewayen genom att gå till **punkt-till-plats-konfiguration** och plocknings- **OpenVPN (SSL)** som **tunnel typ**. Välj **Azure Active Directory** som **Autentiseringstyp** och fyll i informationen i avsnittet **Azure Active Directory** .

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azure-ad-auth-portal.png)


   > [!NOTE]
   > Se till att du inkluderar ett avslutande snedstreck i slutet av `AadIssuerUri` värdet. Annars kan anslutningen Miss lyckas.

10. Skapa och ladda ned profilen genom att klicka på länken **Ladda ned VPN-klient** .

11. Extrahera den hämtade ZIP-filen.

12. Bläddra till mappen unzippad "AzureVPN".

13. Anteckna platsen för filen "azurevpnconfig.xml". azurevpnconfig.xml innehåller inställningen för VPN-anslutningen och kan importeras direkt till Azure VPN-klientprogrammet. Du kan också distribuera filen till alla användare som behöver ansluta via e-post eller på annat sätt. Användaren måste ha giltiga autentiseringsuppgifter för Azure AD för att kunna ansluta.

## <a name="next-steps"></a>Nästa steg

För att kunna ansluta till ditt virtuella nätverk måste du skapa och konfigurera en profil för VPN-klienter. Se [Konfigurera en VPN-klient för P2s VPN-anslutningar](openvpn-azure-ad-client.md).
