---
title: Enkelsidig rendering
description: Beskriver inställningar och användnings fall för Enkels idiga åter givning
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 37a665c776a64558a13910875f221462fb7d0ef8
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205072"
---
# <a name="no-loc-textsingle-sided-rendering"></a>:::no-loc text="Single-sided"::: Render

De flesta åter givnings programmen använder [backend-culling](https://en.wikipedia.org/wiki/Back-face_culling) för att förbättra prestandan. När maskorna klipps av öppen med [Klipp ut plan](cut-planes.md)kommer användarna ofta att titta på bak sidan av trianglar. Om dessa trianglar är slaktade, ser inte resultatet ut som övertygande.

Sättet att förhindra det här problemet på ett tillförlitligt sätt är att rendera trianglar *dubbels idiga*. Eftersom det inte går att använda backend-culling har prestanda effekter, som standard växlar Azure-fjärrrendering endast till dubbels idig åter givning för maskor som skärs med ett klipp plan.

Med * :::no-loc text="single-sided"::: åter givnings* inställningen kan du anpassa det här beteendet.

> [!CAUTION]
> :::no-loc text="single-sided":::Åter givnings inställningen är en experimentell funktion. Den kan tas bort igen i framtiden. Ändra inte standardinställningen, om det inte löser ett kritiskt problem i ditt program.

## <a name="prerequisites"></a>Förutsättningar

:::no-loc text="single-sided":::Åter givnings inställningen har bara en påverkan för maskor som har [konverterats](../../how-tos/conversion/configure-model-conversion.md) med `opaqueMaterialDefaultSidedness` alternativet inställt på `SingleSided` . Som standard är det här alternativet inställt på `DoubleSided` .

## <a name="no-loc-textsingle-sided-rendering-setting"></a>:::no-loc text="Single-sided"::: åter givnings inställning

Det finns tre olika lägen:

**Normal:** I det här läget återges alltid maskor när de konverteras. Det innebär att nät som konverteras med `opaqueMaterialDefaultSidedness` set till `SingleSided` alltid återges med en bakgrunds Cull aktive rad, även om de överlappar ett klipp plan.

**DynamicDoubleSiding:** I det här läget, när ett klipp plan överlappar ett nät, växlas det automatiskt till dubbels idig åter givning. Det här läget är standard läget.

**AlwaysDoubleSided:** Tvingar all Enkels idig geometri att renderas dubbels idiga hela tiden. Det här läget exponeras främst så att du enkelt kan jämföra prestanda påverkan mellan :::no-loc text="single-sided"::: och :::no-loc text="double-sided"::: rendera.

Ändra :::no-loc text="single-sided"::: åter givnings inställningarna kan göras på följande sätt:

```cs
void ChangeSingleSidedRendering(AzureSession session)
{
    SingleSidedSettings settings = session.Actions.SingleSidedSettings;

    // Single-sided geometry is rendered as is
    settings.Mode = SingleSidedMode.Normal;

    // Single-sided geometry is always rendered double-sided
    settings.Mode = SingleSidedMode.AlwaysDoubleSided;
}
```

```cpp
void ChangeSingleSidedRendering(ApiHandle<AzureSession> session)
{
    ApiHandle<SingleSidedSettings> settings = session->Actions()->GetSingleSidedSettings();

    // Single-sided geometry is rendered as is
    settings->SetMode(SingleSidedMode::Normal);

    // Single-sided geometry is always rendered double-sided
    settings->SetMode(SingleSidedMode::AlwaysDoubleSided);
}
```

## <a name="api-documentation"></a>API-dokumentation

* [C# RemoteManager. SingleSidedSettings-egenskap](/dotnet/api/microsoft.azure.remoterendering.remotemanager.singlesidedsettings)
* [C++ RemoteManager:: SingleSidedSettings ()](/cpp/api/remote-rendering/remotemanager#singlesidedsettings)

## <a name="next-steps"></a>Nästa steg

* [Klippa ut planer](cut-planes.md)
* [Konfigurera modell konverteringen](../../how-tos/conversion/configure-model-conversion.md)