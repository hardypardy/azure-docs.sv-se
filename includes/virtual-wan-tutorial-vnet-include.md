---
title: ta med fil
description: ta med fil
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40ae634897361219c39e60d2161d3576cc44a400
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077523"
---
För att snabbt skapa ett virtuellt nätverk, kan du klicka på ”prova” i den här artikeln för att öppna en PowerShell-konsol i Azure Cloud Shell. Justera värdena och kopiera och klistra sedan in kommandona i konsolfönstret. 

Se till att adressutrymmet för det virtuella nätverket du skapar inte överlappar adressintervallen för andra virtuella nätverk du vill ansluta till, eller dina lokala nätverksadresser.

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Om du inte redan har en resursgrupp som du vill använda, skapa en ny. Justera PowerShell-kommandon för att återspegla resursgruppens namn du vill använda, och sedan kör du följande cmdlet:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Skapa ett virtuellt nätverk

Justera PowerShell-kommandon för att skapa ett virtuellt nätverk som är kompatibel för din miljö.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```
