---
title: Azure Security Centerens huvud instrument panel eller översikts sida
description: Lär dig mer om funktionerna på sidan för Security Center översikt
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 899fddf08a817126ce775733da38ffd10ab3ab23
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91952122"
---
# <a name="azure-security-centers-overview-page"></a>Översikts sida för Azure Security Center

När du öppnar Azure Security Center visas sidan Översikt på den första sidan. 

:::image type="content" source="media/overview-page/overview.png" alt-text="Security Centers översiktssida":::

Upptäck och utvärdera säkerheten för dina arbets belastningar och identifiera och minska riskerna med sidan Security Center översikt.

Översikten ger en enhetlig vy över säkerhets position för dina arbets belastningar i hybrid molnet. Dessutom visas säkerhets aviseringar, täcknings information med mera.


## <a name="features-of-the-overview-page"></a>Funktioner på sidan Översikt

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Security Centers översiktssida":::

Den **översta meny raden** innehåller:
- **Prenumerationer** – du kan visa och filtrera listan över prenumerationer genom att välja den här knappen. Security Center kommer att justera visningen så att den återspeglar säkerhets position för de valda prenumerationerna.
- **Nyheter** – öppnar [viktig information](release-notes.md) så att du kan hålla dig uppdaterad med nya funktioner, fel korrigeringar och föråldrade funktioner.
- **Höga nummer** för de anslutna moln kontona, för att Visa kontexten för informationen i huvud panelerna nedan. Samt antalet aktiva rekommendationer och aviseringar.
    Lär dig mer om att ansluta dina [AWS-konton](quickstart-onboard-aws.md) och dina [GCP-projekt](quickstart-onboard-gcp.md).


I mitten av sidan finns **fyra centrala paneler**, vart och ett länkar till en dedikerad instrument panel för mer information:
- **Säker poäng** Security Center kontinuerligt utvärderar dina resurser, prenumerationer och din organisation efter säkerhets problem. Den sammanställer sedan alla resultat i en enda poäng så att du snabbt kan tala om din aktuella säkerhets situation: ju högre poäng, desto lägre är den identifierade risk nivån. [Läs mer](secure-score-security-controls.md).
- **Efterlevnad** – Security Center ger insikter om position utifrån kontinuerlig utvärdering av Azure-miljön. Security Center analyserar riskfaktorer i din hybrid moln miljö enligt rekommenderade säkerhets metoder. Dessa utvärderingar mappas till kompatibilitetskontroller från en uppsättning standarder som stöds. [Läs mer](security-center-compliance-dashboard.md).
- **Azure Defender** – det här är CWPP (Cloud Worker Protection Platform) som är integrerad i Security Center för avancerade, intelligenta, skydd av dina Azure-och hybrid arbets belastningar. Panelen visar täckningen för dina anslutna resurser (för de valda prenumerationerna) och de senaste aviseringarna, färgkodade efter allvarlighets grad. [Läs mer](azure-defender.md).
- **Inventering** – panelen visar antalet oövervakade virtuella datorer och en enkel barometer av dina resurser som övervakas av Security Center. [Läs mer](asset-inventory.md).


Fönstret **insikter** innehåller anpassade objekt för din miljö, inklusive:
- Dina mest begripna resurser
- Dina säkerhets kontroller har högsta möjliga möjlighet att öka dina säkra Poäng
- De aktiva rekommendationerna med flest resurser som påverkas
- De senaste blogg inläggen av Azure Security Center experter

## <a name="next-steps"></a>Nästa steg

Den här sidan introducerade sidan Security Center översikt. Relaterad information finns i:

- [Utforska och hantera dina resurser med till gångs inventering och hanterings verktyg](asset-inventory.md)
- [Säkra poäng i Azure Security Center](secure-score-security-controls.md)