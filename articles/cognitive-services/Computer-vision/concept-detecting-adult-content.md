---
title: Vuxen, vågat, fullständig-innehåll – Visuellt innehåll
titleSuffix: Azure Cognitive Services
description: Begrepp som rör identifiering av olämpligt innehåll i avbildningar med hjälp av Visuellt innehåll APi.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5cc8a4508ceeda245fbc10a81e16f3ecf05284c7
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/21/2020
ms.locfileid: "95013624"
---
# <a name="detect-adult-content"></a>Identifiera vuxen innehåll

Visuellt innehåll kan identifiera vuxna material i bilder så att utvecklare kan begränsa visningen av dessa bilder i program varan. Innehålls flaggor används med ett resultat mellan noll och en så att utvecklarna kan tolka resultaten enligt sina egna inställningar.

> [!NOTE]
> Mycket av den här funktionen erbjuds av [Azure Content moderator](../content-moderator/overview.md) -tjänsten. Se det här alternativet för lösningar till fler rigorösa innehålls redigerings scenarier, till exempel text moderator och mänsklig gransknings arbets flöden.

## <a name="content-flag-definitions"></a>Definition av innehålls flagga

Inom klassificeringen "vuxen" är flera olika kategorier:

- **Sexuella** bilder definieras som sådana som är explicit sexuella och som ofta visar nakenhet och sexuella handlingar.
- **Vågat** -avbildningar definieras som bilder som är sexuellt och ofta innehåller mindre sexuellt innehåll än bilder som taggats som **vuxen**.
- **Fullständig** -avbildningar definieras som de som visar Gore.

## <a name="use-the-api"></a>Använda API:et

Du kan identifiera innehåll för vuxna med API för att [analysera avbildning](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) . När du lägger till värdet för `Adult` i **visualFeatures** -Frågeparametern returnerar API tre booleska egenskaper &mdash; `isAdultContent` , `isRacyContent` och `isGoryContent` &mdash; i dess JSON-svar. Metoden returnerar också motsvarande egenskaper &mdash; `adultScore` , `racyScore` och `goreScore` &mdash; som representerar förtroende poängen mellan noll och en för varje respektive kategori.

- [Snabb start: analysera en avbildning (.NET SDK)](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Snabb start: analysera en avbildning (REST API)](./quickstarts/csharp-analyze.md)