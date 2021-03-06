---
title: Använda aktiv utbildning med kunskaps bas – QnA Maker
description: Lär dig att förbättra kvaliteten på din kunskaps bas med aktiv inlärning. Granska, acceptera eller avvisa, Lägg till utan att ta bort eller ändra befintliga frågor.
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 32dfe6a274889181410ab5f800ac595ee81c148c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91298153"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Använda aktiv inlärning för att förbättra kunskapsbasen

Med [aktiv inlärning](../Concepts/active-learning-suggestions.md) kan du förbättra din kunskaps bas kvalitet genom att föreslå alternativa frågor. Användar-och insändningar beaktas och visas som förslag i listan över alternativa frågor. Du har möjlighet att antingen lägga till dessa förslag som alternativa frågor eller avvisa dem.

Kunskaps basen ändras inte automatiskt. För att alla ändringar ska börja gälla måste du godkänna förslagen. Dessa förslag lägger till frågor, men ändrar inte eller tar inte bort befintliga frågor.


## <a name="upgrade-runtime-version-to-use-active-learning"></a>Uppgradera runtime-versionen för att använda aktiv inlärning

Active Learning stöds i runtime-version 4.4.0 och senare. Om din kunskaps bas har skapats på en tidigare version kan du [Uppgradera körningen](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) för att använda den här funktionen.

## <a name="turn-on-active-learning-for-alternate-questions"></a>Aktivera aktiv inlärning för alternativa frågor

Aktiv inlärning är inaktiverat som standard. Aktivera det om du vill se föreslagna frågor. När du har aktiverat aktiv inlärning måste du skicka information från klient appen till QnA Maker. Mer information finns i [arkitektoniskt flöde för att använda GenerateAnswer och träna API: er från en bot](improve-knowledge-base.md#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Välj **publicera** för att publicera kunskaps basen. Aktiva inlärnings frågor samlas endast in från GenerateAnswer API förutsägelse-slut punkten. Frågorna i test fönstret i QnA Makers portalen påverkar inte aktiv inlärning.

1. Om du vill aktivera aktiv inlärning i QnA Maker portal går du till det övre högra hörnet och väljer ditt **namn**, gå till [**tjänst inställningar**](https://www.qnamaker.ai/UserSettings).

    ![Aktivera alternativ för den föreslagna frågan i Active Learning på sidan tjänst inställningar. Välj ditt användar namn på menyn längst upp till höger och välj sedan tjänst inställningar.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Hitta QnA Maker tjänsten och växla sedan **aktiv inlärning**.

    > [!div class="mx-imgBorder"]
    > [![På sidan tjänst inställningar växlar du till funktionen aktiv inlärning. Om du inte kan växla funktionen kan du behöva uppgradera tjänsten.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > Den exakta versionen på föregående bild visas som ett exempel. Din version kan vara annorlunda.

    När **aktiv inlärning** har Aktiver ATS föreslår kunskaps basen nya frågor med jämna mellanrum baserat på frågor som skickats av användaren. Du kan inaktivera **aktiv inlärning** genom att växla inställningen igen.

## <a name="review-suggested-alternate-questions"></a>Granska föreslagna alternativa frågor

[Granska alternativa föreslagna frågor](improve-knowledge-base.md) på sidan **Redigera** i varje kunskaps bas.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Skapa en kunskaps bas](./manage-knowledge-bases.md)