---
title: Hämta v3 förutsägelse slut punkt
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 05/05/2020
ms.author: diberry
ms.openlocfilehash: d73508e8734883f5ffef205b6c1f03905f349f91
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316500"
---
1. I LUIS-portalen i avsnittet **Hantera** (översta högra menyn) på sidan **Azure-resurser** (menyn till vänster) på fliken **förutsägelse resurser** , kopierar du **exempel frågan** längst ned på sidan.

    Klistra in webb adressen i en ny flik i webbläsaren.

    URL: en har ditt app-ID, nyckel och plats namn. URL: en för v3-förutsägelsen ser ut så här:

    `https://YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-PREDICTION-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

