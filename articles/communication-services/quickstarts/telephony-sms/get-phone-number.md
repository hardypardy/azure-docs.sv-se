---
title: Snabb start – hämta ett telefonnummer från Azure Communication Services
description: Lär dig hur du köper ett telefonnummer till kommunikations tjänster med hjälp av Azure Portal.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
ms.openlocfilehash: cff39f93f9caddfdbe48788f14b62642a373e2bf
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148195"
---
# <a name="quickstart-get-a-phone-number-using-the-azure-portal"></a>Snabb start: Hämta ett telefonnummer med hjälp av Azure Portal

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Kom igång med Azure Communication Services genom att använda Azure Portal för att köpa ett telefonnummer.

## <a name="prerequisites"></a>Förutsättningar

- Ett Azure-konto med en aktiv prenumeration. [Skapa ett konto kostnads fritt](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [En Active Communication Services-resurs.](../create-communication-resource.md)

## <a name="get-a-phone-number"></a>Hämta ett telefonnummer

Om du vill börja etablerings numren går du till kommunikations tjänst resursen på [Azure Portal](https://portal.azure.com).

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

### <a name="getting-new-phone-numbers"></a>Hämtar nya telefonnummer

Navigera till bladet med **telefonnummer** på resurs-menyn.

:::image type="content" source="../media/manage-phone-azure-portal-phone-page.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

Tryck på **Get** -knappen för att starta guiden. I guiden på bladet **telefonnummer** får du hjälp med en serie frågor som hjälper dig att välja det telefonnummer som passar bäst för ditt scenario. 

Du måste först välja det **land/den region** där du vill etablera telefonnumret. När du har valt land/region måste du välja det **användnings fall** som bäst motsvarar dina behov. 

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

### <a name="select-your-phone-number-features"></a>Välj dina telefonnummers funktioner

Konfigurationen av ditt telefonnummer är uppdelad i två steg: 

1. Valet av [siffer typ](../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services)
2. Valet av [nummer funktioner](../../concepts/telephony-sms/plan-solution.md#phone-number-features-in-azure-communication-services)

Du kan välja mellan två telefonnummer typer: **geografiska**och **kostnads fria**. När du har valt en siffer typ kan du sedan välja funktionen.

I vårt exempel har vi valt en **kostnads fri** nummer typ med **utgående samtal** och **inkommande och utgående SMS** -funktioner.

:::image type="content" source="../media/manage-phone-azure-portal-select-plans.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

Härifrån klickar du på knappen **Nästa: tal** längst ned på sidan för att anpassa de telefonnummer som du vill etablera.

### <a name="customizing-phone-numbers"></a>Anpassa telefonnummer

På sidan **siffror** kommer du att anpassa de telefonnummer som du vill etablera.

:::image type="content" source="../media/manage-phone-azure-portal-select-numbers-start.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

> [!NOTE]
> Den här snabb starten visar anpassnings flödet för **avgiftsfri** nummer typ. Upplevelsen kan skilja sig något från om du har valt **geografisk** nummer typ, men slut resultatet blir detsamma.

Välj **rikt nummer** i listan över tillgängliga rikt nummer och ange den kvantitet som du vill etablera. Klicka sedan på **Sök** för att hitta siffror som uppfyller de valda kraven. De telefonnummer som motsvarar dina behov visas tillsammans med deras månads kostnad.

:::image type="content" source="../media/manage-phone-azure-portal-found-numbers.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

> [!NOTE]
> Tillgängligheten beror på den siffer typ, plats och de funktioner som du har valt.
> Siffror är reserverade under en kort tid innan transaktionen upphör att gälla. Om transaktionen upphör att gälla måste du markera talen igen.

Om du vill visa köp sammanfattningen och placera beställningen klickar du på **Nästa: knappen Sammanfattning** längst ned på sidan.

### <a name="place-order"></a>Placera order

På sidan Sammanfattning visas siffer typ, funktioner, telefonnummer och total månads kostnad för att etablera telefonnumret.

> [!NOTE]
> Priserna som visas är de **månatliga återkommande avgifterna** som omfattar kostnaden för att leasa det valda telefonnumret till dig. Som inte ingår i den här vyn är **kostnaderna för betala per** användning som uppstår när du gör eller tar emot samtal. Pris listorna finns [här](../../concepts/pricing.md). Dessa kostnader är beroende av siffer typ och destinationer som kallas. Till exempel kan pris per minut för ett samtal från ett regionalt Seattle-nummer till ett regionalt nummer i New York och ett samtal från samma nummer till ett BRITTISKt mobil nummer vara olika.

Klicka slutligen på **Placera order** längst ned på sidan för att bekräfta.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers-summary.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

## <a name="find-your-phone-numbers-on-the-azure-portal"></a>Hitta dina telefonnummer på Azure Portal

Gå till Azure Communication-resursen på [Azure Portal](https://portal.azure.com):

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

Välj bladet telefonnummer på menyn för att hantera dina telefonnummer.

:::image type="content" source="../media/manage-phone-azure-portal-phones.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

> [!NOTE]
> Det kan ta några minuter innan de etablerade talen visas på den här sidan.


### <a name="customizing-phone-numbers"></a>Anpassa telefonnummer

På sidan **siffror** kan du välja ett telefonnummer för att konfigurera det.

:::image type="content" source="../media/manage-phone-azure-portal-capability-update.png" alt-text="Skärm bild som visar huvud sidan för en kommunikations tjänst resurs.":::

Välj funktionerna i de tillgängliga alternativen och klicka sedan på **Bekräfta** för att tillämpa ditt val.

## <a name="troubleshooting"></a>Felsökning

Vanliga frågor och problem:

- Det går bara att köpa telefonnummer för oss just nu. Detta baseras på fakturerings adressen för den prenumeration som resursen är associerad med. För tillfället går det inte att flytta en resurs till en annan prenumeration.

- När ett telefonnummer släpps, frigörs inte telefonnumret eller kan inte köpas förrän i slutet av fakturerings perioden.

- När en kommunikations tjänst resurs tas bort, släpps de telefonnummer som är associerade med resursen automatiskt samtidigt.

## <a name="next-steps"></a>Nästa steg

I den här snabb starten har du lärt dig att:

> [!div class="checklist"]
> * Köp ett telefonnummer
> * Hantera ditt telefonnummer
> * Frisläpp ett telefonnummer

> [!div class="nextstepaction"]
> [Skicka ett SMS](../telephony-sms/send.md) 
>  [Kom igång med att anropa](../voice-video-calling/getting-started-with-calling.md)
