---
title: Logga ML-experiment och mått
titleSuffix: Azure Machine Learning
description: Övervaka dina Azure ML-experiment och körningen av mått för att förbättra modellskapandet. Lägg till loggning i träningsskriptet med run.log, Run.start_logging eller ScriptRunConfig.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 44fe71f575a32ccc1a687bc87793cb6a8b6508a9
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89650628"
---
# <a name="enable-logging-in-azure-ml-training-runs"></a>Aktivera loggning i Azure ML:s träningskörningar
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Med Python-SDK:n i Azure Machine Learning kan du logga information i realtid med hjälp av både standardpaketet för Python-loggning och SDK-specifika funktioner. Du kan logga lokalt och skicka loggar till din arbetsyta i portalen.

Med loggarna kan du diagnostisera fel och varningar, eller spåra prestandamått som parametrar och modellprestanda. I den här artikeln får du lära dig hur du aktiverar loggning i följande scenarier:

> [!div class="checklist"]
> * Interaktiva träningssessioner
> * Skicka träningsjobb med ScriptRunConfig
> * Inbyggda `logging`-inställningar i Python
> * Loggning från fler källor


> [!TIP]
> Den här artikeln visar hur du övervakar modellträningsprocessen. Om du vill veta mer om att övervaka resursanvändning och händelser från Azure Machine Learning, till exempel kvoter, slutförda träningskörningar eller slutförda modelldistributioner, kan du läsa [Övervakning i Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="data-types"></a>Datatyper

Du kan logga flera datatyper, inklusive skalära värden, listor, tabeller, bilder, kataloger med mera. Mer information och Python-kodexempel för olika datatyper finns på [referenssidan Körningsklass](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py&preserve-view=true).

## <a name="interactive-logging-session"></a>Interaktiv loggningssession

Interaktiva loggningssessioner används vanligtvis i miljöer med notebook-filer. Metoden [Experiment.start_logging()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#&preserve-view=truestart-logging--args----kwargs-) startar en interaktiv loggningssession. Alla mått som loggas under sessionen läggs till i körningsposten i experimentet. Metoden [run.complete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=truecomplete--set-status-true-) avslutar sessionerna och markerar körningen som slutförd.

## <a name="scriptrunconfig-logs"></a>ScriptRunConfig-loggar

I det här avsnittet får du lära dig att lägga till loggningskod i ScriptConfig-körningar. Du kan använda [**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true)-klassen till att kapsla in skript och miljöer för upprepningsbara körningar. Du kan också använda det här alternativet till att visa widgeten för Jupyter Notebooks vid övervakning.

I det här exemplet utförs en parameterrensning av alfavärden och resultaten samlas in med hjälp av metoden [run.log()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=truelog-name--value--description----).

1. Skapa ett träningsskript som innehåller loggningslogiken `train.py`.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]


1. Skicka ```train.py```-skriptet för körning i en användarhanterad miljö. Hela skriptmappen skickas för träningen.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src)] [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)]

    Parametern `show_output` aktiverar utförlig loggning så att du kan se information från träningsprocessen, samt information om eventuella fjärresurser eller beräkningsmål. Använd följande kod för att aktivera utförlig loggning när du skickar experimentet.

```python
run = exp.submit(src, show_output=True)
```

Du kan också använda samma parameter i funktionen `wait_for_completion` för den resulterande körningen.

```python
run.wait_for_completion(show_output=True)
```

## <a name="native-python-logging"></a>Intern Python-loggning

Vissa loggar i SDK:n kan innehålla ett fel som uppmanar dig att ange loggningsnivån DEBUG. Om du ska ange loggningsnivån lägger du till nedanstående kod i skriptet.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="additional-logging-sources"></a>Fler loggningskällor

Azure Machine Learning kan också logga information från andra källor under träningen, till exempel automatiserade maskininlärningskörningar eller Docker-containrar som kör jobben. Dessa loggar dokumenteras inte, men om du stöter på problem och kontaktar Microsofts support kan de använda loggarna vid felsökning.

Information om loggning av mått i Azure Machine Learning-designern (förhandsversion) finns i [Loggning av mått i designern (förhandsversion)](how-to-track-designer-experiments.md)

## <a name="example-notebooks"></a>Exempelnotebook-filer

Följande notebook-filer demonstrerar begreppen i den här artikeln:
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nästa steg

Läs de här artiklarna om du vill lära dig mer om att använda Azure Machine Learning:

* Lär dig att [logga mått i Azure Machine Learning-designern (förhandsversion)](how-to-track-designer-experiments.md).

* Se ett exempel på hur du registrerar den bästa modellen och distribuerar den i självstudien [Träna en bildklassificeringsmodell med Azure Machine Learning](tutorial-train-models-with-aml.md).
