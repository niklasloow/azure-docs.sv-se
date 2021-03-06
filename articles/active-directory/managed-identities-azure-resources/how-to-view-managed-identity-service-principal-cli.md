---
title: Visa tjänstens huvud namn för en hanterad identitet – Azure CLI – Azure AD
description: Stegvisa instruktioner för att Visa tjänstens huvud namn för en hanterad identitet med Azure CLI.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: b8eec72666eadf90a401dc8f0adb77df77dbf782
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90969308"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Visa tjänstens huvud namn för en hanterad identitet med Azure CLI

Hanterade identiteter för Azure-resurser tillhandahåller Azure-tjänster med en automatiskt hanterad identitet i Azure Active Directory. Du kan använda den här identiteten för att autentisera till en tjänst som stöder Azure AD-autentisering utan att ha autentiseringsuppgifter i din kod. 

I den här artikeln får du lära dig hur du visar tjänstens huvud namn för en hanterad identitet med hjälp av Azure CLI.

## <a name="prerequisites"></a>Förutsättningar

- Om du inte känner till hanterade identiteter för Azure-resurser kan du läsa [avsnittet Översikt](overview.md).
- Om du inte redan har ett Azure-konto kan du [Registrera dig för ett kostnads fritt konto](https://azure.microsoft.com/free/).
- Aktivera [systemtilldelad identitet på en virtuell dator eller ett](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) [program](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).
- Om du vill köra exempel skripten har du två alternativ:
    - Använd [Azure Cloud Shell](../../cloud-shell/overview.md)som du kan öppna med knappen **prova** på det övre högra hörnet av kodblock.
    - Kör skript lokalt genom att installera den senaste versionen av [Azure CLI](/cli/azure/install-azure-cli)och logga sedan in på Azure med [AZ-inloggning](/cli/azure/reference-index#az-login). Använd ett konto som är kopplat till den Azure-prenumeration som du vill skapa resurser i.   

## <a name="view-the-service-principal"></a>Visa tjänstens huvud namn

Följande kommando visar hur du visar tjänstens huvud namn för en virtuell dator eller ett program med hanterad identitet aktive rad. Ersätt `<VM or application name>` med dina egna värden. 

```azurecli-interactive
az ad sp list --display-name <VM or application name>
```

## <a name="next-steps"></a>Nästa steg

Mer information om hur du hanterar Azure AD-tjänstens huvud namn med Azure CLI finns i [AZ AD SP](/cli/azure/ad/sp).
