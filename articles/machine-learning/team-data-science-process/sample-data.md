---
title: Exempel data på olika Azure Storage platser – team data science process
description: Exempel data i Azure Blob-behållare, SQL Server och Hive-tabeller för att minska den till en mindre men representativ och mer hanterbar storlek.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 878a2a47dfaddf1d3399a1dbd98865332838e148
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321751"
---
# <a name="sample-data-in-azure-blob-containers-sql-server-and-hive-tables"></a><a name="heading"></a>Exempeldata i Azure-blobcontainrar, SQL Server och Hive-tabeller

I följande artiklar beskrivs hur du kan sampla data som lagras på en av tre olika Azure-platser:

* [**Azure Blob container-data**](sample-data-blob.md) samplas genom att ladda ned det program mässigt och sedan sampla det med hjälp av python-kod.
* [**SQL Server data**](sample-data-sql-server.md) samplas med både SQL och python-programmeringsspråket. 
* [**Hive-tabell data**](sample-data-hive.md) samplas med hjälp av Hive-frågor.

Den här samplings aktiviteten är ett steg i [TDSP (Team data science process)](./index.yml).

**Varför exempel data?**

Om data uppsättningen som du planerar att analysera är stor är det vanligt vis en bra idé att stänga av data för att minska den till en mindre men representativ och mer hanterbar storlek. Downsizing kan under lätta data förståelse, utforskning och funktions teknik. Den här samplings rollen i Cortana Analytics-processen är att möjliggöra snabb prototyper av data bearbetnings funktioner och maskin inlärnings modeller.