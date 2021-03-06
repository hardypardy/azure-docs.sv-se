---
title: Machine Learning i Azure Synapse Analytics
description: En översikt över Machine Learning-funktioner i Azure Synapse Analytics.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: overview
ms.reviewer: jrasnick, garye
ms.date: 09/25/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 382ba871f95b3b36c3f819de8d582ba2c5dc358a
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93316083"
---
# <a name="machine-learning-capabilities-in-azure-synapse-analytics-workspaces-preview"></a>Machine Learning funktioner i Azure Synapse Analytics (för hands versioner av arbets ytor)

[!INCLUDE [preview](../includes/note-preview.md)]

Azure Synapse Analytics erbjuder olika Machine Learning-funktioner. Den här artikeln innehåller en översikt över hur du kan använda Machine Learning i Azure-Synapse.

Den här översikten täcker de olika funktionerna i Synapse som är relaterade till Machine Learning, från ett process perspektiv för data vetenskap.

Du kanske är bekant med hur en vanlig data vetenskaps process ser ut. Det är en välkänd process som de flesta Machine Learning-projekt följer.

På hög nivå innehåller processen följande steg:
* (Affärs förståelse)
* Förvärv och förståelse av data
* Modellering
* Modell distribution och poängsättning

Den här artikeln beskriver Azure Synapse Machine Learning-funktionerna i olika analys motorer, från ett process perspektiv för data vetenskap. För varje steg i data vetenskaps processen sammanfattas de Azure Synapse-funktioner som kan hjälpa dig.

## <a name="azure-synapse-machine-learning-capabilities"></a>Funktioner för Azure Synapse Machine Learning

### <a name="data-acquisition-and-understanding"></a>Förvärv och förståelse av data

De flesta Machine Learning-projekt inkluderar väletablerade steg och ett av de här stegen är att komma åt och förstå data.

#### <a name="data-source-and-pipelines"></a>Data källa och pipeliner

Tack vare [Azure Data Factory](/azure/data-factory/introduction), en internt integrerad del av Azure Synapse, finns det en kraftfull uppsättning verktyg som är tillgängliga för data inmatning och pipeliner för data dirigering. På så sätt kan du enkelt bygga datapipelines för att komma åt och omvandla data till ett format som kan användas för maskin inlärning. [Lär dig mer om datapipelines](/azure/data-factory/concepts-pipelines-activities?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) i Synapse. 

#### <a name="data-preparation-and-explorationvisualization"></a>Förberedelse av data och utforskning/visualisering

En viktig del av Machine Learning-processen är att förstå data efter utforskning och visualiseringar.

Beroende på var data lagras, erbjuder Synapse en uppsättning olika verktyg för att utforska och förbereda den för analys och maskin inlärning. Ett av de snabbaste sätten att komma igång med data utforskning använder Apache Spark eller Synapse SQL Server-pooler direkt över data i data Lake.

* [Apache Spark för Azure Synapse](../spark/apache-spark-overview.md) erbjuder funktioner för att transformera, förbereda och utforska dina data i stor skala. Dessa Spark-pooler erbjuder verktyg som PySpark/python, Scala och .NET för data bearbetning i stor skala. Med hjälp av kraftfulla visualiserings bibliotek kan data utforsknings upplevelsen förbättras för att hjälpa till att förstå data bättre. [Lär dig mer om hur du utforskar och visualiserar data i Synapse med Spark](../get-started-analyze-spark.md).

* [SYNAPSE SQL Server-pooler](../sql/on-demand-workspace-overview.md) ger ett sätt att utforska data med tsql direkt över data Lake. Synapse SQL Server-pooler erbjuder också vissa inbyggda visualiseringar i Synapse Studio. [Lär dig mer om hur du utforskar data med SYNAPSE SQL Server-pooler](../get-started-analyze-sql-on-demand.md).

### <a name="modeling"></a>Modellering

I Azure Synapse kan utbildnings maskin inlärnings modeller utföras på Apache Spark pooler med verktyg som PySpark/python, Scala eller .NET.

#### <a name="train-models-on-spark-pools-with-mllib"></a>Träna modeller på Spark-pooler med MLlib

Maskin inlärnings modeller kan tränas med hjälp av olika algoritmer och bibliotek. [Spark MLlib](http://spark.apache.org/docs/latest/ml-guide.html) erbjuder skalbara algoritmer för maskin inlärning som kan hjälpa till att lösa de flesta klassiska Machine Learning-problem. [Den här självstudien](../spark/apache-spark-machine-learning-mllib-notebook.md) beskriver hur du tränar en modell med MLlib i Synapse.

Förutom MLlib kan populära bibliotek som [Scikit lära](https://scikit-learn.org/stable/) också användas för att utveckla modeller. Mer information om hur du installerar bibliotek på Synapse Spark-pooler finns i [Hantera bibliotek för Apache Spark i Azure Synapse Analytics](../spark/apache-spark-azure-portal-add-libraries.md) .

#### <a name="train-models-with-azure-machine-learning-automl"></a>Träna modeller med Azure Machine Learning AutoML

Ett annat sätt att träna Machine Learning-modeller, som inte kräver mycket tidigare erfarenhet med Machine Learning, är att använda AutoML. [AutoML](/azure/machine-learning/concept-automated-ml) är en funktion som automatiskt tågen en uppsättning maskin inlärnings modeller och låter användaren välja den bästa modellen baserat på vissa mått. Tack vare en sömlös integrering med Azure Machine Learning från Azure Synapse-anteckningsböcker kan användare enkelt utnyttja AutoML i Synapse med genom strömnings Azure Active Directory autentisering.  Det innebär att du bara behöver peka på din Azure Machine Learning-arbetsyta och behöver inte ange några autentiseringsuppgifter. Här är en [AutoML själv studie kurs](../spark/apache-spark-azure-machine-learning-tutorial.md) som beskriver hur du tränar modeller med hjälp av Azure Machine Learning AutoML på Synapse Spark-pooler.

### <a name="model-deployment-and-scoring"></a>Modell distribution och poängsättning

Modeller som har tränats antingen i Azure Synapse eller utanför Azure Synapse kan enkelt användas för batch-poäng. För närvarande i Synapse finns det två sätt att köra batch-poäng.

* Du kan använda [funktionen tsql predict](../sql-data-warehouse/sql-data-warehouse-predict.md) i Synapse SQL-pooler för att köra dina förutsägelser direkt där dina data är i livet. Med den här kraftfulla och skalbara funktionen kan du utöka dina data utan att flytta några data från ditt informations lager. En ny [miljö för den guidade maskin inlärnings modellen i Synapse Studio](https://aka.ms/synapse-ml-ui) introducerades där du kan distribuera en ONNX-modell från Azure Machine Learning Model-registret i Synapse SQL-pooler för batch-poäng med hjälp av predict.

* Ett annat alternativ för maskin inlärnings modeller för batch-bedömning i Azure Synapse är att utnyttja Apache Spark pooler för Azure Synapse. Beroende på vilka bibliotek som används för att träna modellerna kan du använda en kod upplevelse för att köra batch-poängen.

## <a name="next-steps"></a>Nästa steg

* [Kom igång med Azure Synapse Analytics](../get-started.md)
* [Skapa en arbetsyta](../get-started-create-workspace.md)
* [Snabb start: skapa en ny Azure Machine Learning länkad tjänst i Synapse](quickstart-integrate-azure-machine-learning.md)
* [Självstudie: bedömnings guide för Machine Learning-modell – dedikerad SQL-pool](tutorial-sql-pool-model-scoring-wizard.md)
