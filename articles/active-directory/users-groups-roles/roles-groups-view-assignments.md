---
title: Visa roller som har tilldelats till en grupp i Azure Active Directory | Microsoft Docs
description: Lär dig hur rollerna som har tilldelats en grupp kan visas med hjälp av Azure AD administrations Center. Att Visa grupper och tilldelade roller är standard användar behörigheter.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c0a34b3861c82b3d2ef54a36108f9ea522d716d
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90983101"
---
# <a name="view-roles-assigned-to-a-group-in-azure-active-directory"></a>Visa roller som har tilldelats till en grupp i Azure Active Directory

I det här avsnittet beskrivs hur de roller som har tilldelats en grupp kan visas med hjälp av Azure AD administrations Center. Att Visa grupper och tilldelade roller är standard användar behörigheter.

1. Logga in på [administrations centret för Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) med eventuella autentiseringsuppgifter som inte är administratör eller administratör.

1. Välj den grupp som du är intresse rad av.

1. Välj **tilldelade roller**. Nu kan du se alla Azure AD-roller som har tilldelats den här gruppen.

   ![Visa alla roller som har tilldelats en vald grupp](./media/roles-groups-view-assignments/view-assignments.png)

## <a name="using-powershell"></a>Använda PowerShell

### <a name="get-object-id-of-the-group"></a>Hämta objekt-ID för gruppen

```powershell
Get-AzureADMSGroup -SearchString “Contoso_Helpdesk_Administrators”
```

### <a name="view-role-assignment-to-a-group"></a>Visa roll tilldelning till en grupp

```powershell
Get-AzureADMSRoleAssignment -Filter "principalId eq '<object id of group>" 
```

## <a name="using-microsoft-graph-api"></a>Använda Microsoft Graph API

### <a name="get-object-id-of-the-group"></a>Hämta objekt-ID för gruppen

```powershell
GET https://graph.microsoft.com/beta/groups?$filter displayName eq ‘Contoso_Helpdesk_Administrator’ 
```

### <a name="get-role-assignments-to-a-group"></a>Hämta roll tilldelningar till en grupp

```powershell
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments?$filter=principalId eq
```

## <a name="next-steps"></a>Nästa steg

- [Använd moln grupper för att hantera roll tilldelningar](roles-groups-concept.md)
- [Felsökningsroller tilldelade till molngrupper](roles-groups-faq-troubleshooting.md)
