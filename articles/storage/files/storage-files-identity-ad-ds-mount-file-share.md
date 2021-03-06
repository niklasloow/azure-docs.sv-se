---
title: Montera Azure-filresurs till en AD DS-ansluten virtuell dator
description: Lär dig hur du monterar en fil resurs på dina lokala Active Directory Domain Services anslutna datorer.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: 9807563c768b82c823ff754aaa679ddc917bf62d
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87535067"
---
# <a name="part-four-mount-a-file-share-from-a-domain-joined-vm"></a>Del fyra: montera en fil resurs från en domänansluten virtuell dator

Innan du börjar den här artikeln bör du se till att slutföra den föregående artikeln, [Konfigurera katalog-och filnivå behörigheter över SMB](storage-files-identity-ad-ds-configure-permissions.md).

Processen som beskrivs i den här artikeln kontrollerar att fil resursen och åtkomst behörigheterna är korrekt konfigurerade och att du kan komma åt en Azure-filresurs från en domänansluten virtuell dator. Roll tilldelning på resurs nivå kan ta lite tid att börja gälla. 

Logga in på klienten genom att använda de autentiseringsuppgifter som du har beviljat behörigheter till, som du ser i följande bild.

![Skärm bild som visar inloggnings skärmen för Azure AD för användarautentisering](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

## <a name="mounting-prerequisites"></a>Monterings krav

Innan du kan montera fil resursen måste du kontrol lera att du har gått igenom följande krav:

- Om du monterar fil resursen från en klient som tidigare har monterat fil resursen med hjälp av lagrings kontots nyckel kontrollerar du att du har kopplat från resursen, tagit bort beständiga autentiseringsuppgifter för lagrings konto nyckeln och använder AD DS-autentiseringsuppgifter för autentisering.
- Din klient måste ha detaljerad information om din AD DS. Om datorn eller den virtuella datorn ligger utanför nätverket som hanteras av AD DS måste du aktivera VPN för att komma åt AD DS för autentisering.

Ersätt plats hållarna med dina egna värden och Använd sedan följande kommando för att montera Azure-fil resursen:

```PSH
# Always mount your share using.file.core.windows.net, even if you setup a private endpoint for your share.
$connectTestResult = Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 445
if ($connectTestResult.TcpTestSucceeded)
{
  net use <desired-drive letter>: \\<storage-account-name>.file.core.windows.net\<fileshare-name>
} 
else 
{
  Write-Error -Message "Unable to reach the Azure storage account via port 445. Check to make sure your organization or ISP is not blocking port 445, or use Azure P2S VPN, Azure S2S VPN, or Express Route to tunnel SMB traffic over a different port."
}

```

Om du stöter på problem med att montera med AD DS-autentiseringsuppgifter kan du se att [det inte går att montera Azure Files med AD-autentiseringsuppgifter](storage-troubleshoot-windows-file-connection-problems.md#unable-to-mount-azure-files-with-ad-credentials) för vägledning.

Om du monterar fil resursen har du aktiverat och konfigurerat AD DS-autentisering lokalt för dina Azure-filresurser.

## <a name="next-steps"></a>Nästa steg

Om identiteten som du skapade i AD DS för att representera lagrings kontot finns i en domän eller ORGANISATIONSENHET som tillämpar lösen ords rotationen fortsätter du till nästa artikel för instruktioner om hur du uppdaterar lösen ordet:

[Uppdatera lösen ordet för din lagrings konto identitet i AD DS](storage-files-identity-ad-ds-update-password.md)
