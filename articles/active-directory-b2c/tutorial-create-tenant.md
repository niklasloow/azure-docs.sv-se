---
title: Självstudie – Skapa en Azure Active Directory B2C-klient
description: Följ den här självstudien för att lära dig hur du förbereder registreringen av dina program genom att skapa en Azure Active Directory B2C-klient med hjälp av Azure Portal.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 07/01/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: fbccbcf1ac85b63c5610b9904a84e5e6e3fb6c63
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922202"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Självstudie: Skapa en klientorganisation i Azure Active Directory B2C

Innan dina program kan interagera med Azure Active Directory B2C (Azure AD B2C) måste de registreras i en klient som du hanterar.

I den här artikeln kan du se hur du:

> [!div class="checklist"]
> * Skapa en Azure AD B2C-klient
> * Länka din klient till din prenumeration
> * Växla till den katalog som innehåller din Azure AD B2C-klient
> * Lägg till Azure AD B2C resurs som en **favorit** i Azure Portal

Du får lära dig hur du registrerar ett program i nästa självstudie.

## <a name="prerequisites"></a>Krav

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="create-an-azure-ad-b2c-tenant"></a>Skapa en Azure AD B2C-klient

1. Logga in på [Azure-portalen](https://portal.azure.com/). Logga in med ett Azure-konto som har tilldelats minst [deltagar](../role-based-access-control/built-in-roles.md) rollen i prenumerationen eller en resurs grupp i prenumerationen.

1. Välj den katalog som innehåller din prenumeration.

    I verktygsfältet Azure Portal väljer du ikonen **katalog + prenumeration** och väljer sedan den katalog som innehåller din prenumeration. Den här katalogen skiljer sig från den som kommer att innehålla Azure AD B2C klient.

    ![Prenumerations klient, katalog + prenumerations filter med vald prenumerations klient](media/tutorial-create-tenant/portal-01-pick-directory.png)

1. I menyn i Azure-portalen eller på sidan **Start** väljer du **Skapa en resurs**.
1. Sök efter **Azure Active Directory B2C**och välj sedan **skapa**.
1. Välj **Skapa en ny Azure AD B2C-klient**.

    ![Skapa en ny Azure AD B2C klient som valts i Azure Portal](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. På sidan **skapa en katalog** anger du följande:

   - **Organisations namn** – ange ett namn för din Azure AD B2C-klient.
   - **Första domän namn** – ange ett domän namn för Azure AD B2C klient organisationen.
   - **Land eller region** – Välj land eller region i listan. Det går inte att ändra det här valet senare.
   - **Prenumeration** – välj din prenumeration i listan.
   - **Resurs grupp** – Välj en resurs grupp som ska innehålla klient organisationen. Eller Välj **Skapa ny**, ange ett **namn** för resurs gruppen, Välj **resurs gruppens plats**och välj sedan **OK**.

    ![Skapa klient organisations formulär i med exempel värden i Azure Portal](media/tutorial-create-tenant/review-and-create-tenant.png)

1. Välj **Granska + skapa**.
1. Granska dina katalog inställningar. Välj sedan **Skapa**.

Du kan länka flera Azure AD B2C klienter till en enda Azure-prenumeration för fakturerings syfte. Om du vill länka en klient måste du vara administratör i Azure AD B2C-klienten och tilldelas minst en deltagar roll i Azure-prenumerationen. Se [Länka en Azure AD B2C-klient till en prenumeration](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription).

## <a name="select-your-b2c-tenant-directory"></a>Välj din B2C-klient katalog

Om du vill börja använda din nya Azure AD B2C-klient måste du växla till den katalog som innehåller klienten.

Välj filtret **katalog + prenumeration** på den översta menyn i Azure Portal och välj sedan den katalog som innehåller Azure AD B2C-klienten.

Om du först inte ser din nya Azure B2C-klient i listan uppdaterar du webbläsarfönstret och väljer sedan **katalog + prenumerations** filter igen på den översta menyn.

![B2C som innehåller en katalog som är vald i Azure Portal](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Lägg till Azure AD B2C som favorit (valfritt)

Det här valfria steget gör det enklare att välja Azure AD B2C klient i följande och alla efterföljande självstudier.

I stället för att söka efter *Azure AD B2C* i **alla tjänster** varje gång du vill arbeta med din klient organisation kan du istället fylla i resursen. Sedan kan du välja den från Portal-menyns **Favoriter** -avsnitt för att snabbt bläddra till din Azure AD B2C-klient.

Du behöver bara utföra den här åtgärden en gång. Innan du utför de här stegen bör du kontrol lera att du har växlat till den katalog som innehåller Azure AD B2C klient organisationen enligt beskrivningen i föregående avsnitt, [välja din B2C-innehavaradministratör](#select-your-b2c-tenant-directory).

1. Logga in på [Azure-portalen](https://portal.azure.com).
1. I menyn Azure Portal väljer du **alla tjänster**.
1. I Sök rutan **alla tjänster** söker du efter **Azure AD B2C**, hovrar över Sök resultatet och väljer sedan stjärn ikonen i knapp beskrivningen. **Azure AD B2C** visas nu i Azure Portal under **Favoriter**.
1. Om du vill ändra placeringen för den nya favoriten går du till Azure Portal-menyn, väljer **Azure AD B2C**och drar den sedan uppåt eller nedåt till önskad position.

    ![Azure AD B2C, Favoriter-menyn Microsoft Azure-portalen](media/tutorial-create-tenant/portal-08-b2c-favorite.png)

## <a name="next-steps"></a>Nästa steg

I den här artikeln lärde du dig att:

> [!div class="checklist"]
> * Skapa en Azure AD B2C-klient
> * Länka din klient till din prenumeration
> * Växla till den katalog som innehåller din Azure AD B2C-klient
> * Lägg till Azure AD B2C resurs som en **favorit** i Azure Portal

Nu ska du lära dig hur du registrerar ett webb program i din nya klient.

> [!div class="nextstepaction"]
> [Registrera dina program >](tutorial-register-applications.md)
