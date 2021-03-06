---
title: Förbered dina tekniska till gångar för Azure Container
description: Teknisk resurs och rikt linjer som hjälper dig att konfigurera ett behållar erbjudande på Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 11/30/2020
ms.openlocfilehash: 30f9eac0a0e88092a243b4d9421d2a4f33b07841
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759036"
---
# <a name="prepare-your-azure-container-technical-assets"></a>Förbered dina tekniska till gångar för Azure Container

Den här artikeln innehåller tekniska resurser och rekommendationer som hjälper dig att skapa ett behållar erbjudande på Azure Marketplace.

## <a name="before-you-begin"></a>Innan du börjar

Snabb starter, självstudier och exempel finns i [Azure Container instances-dokumentationen](/azure/container-instances/).

## <a name="fundamental-technical-knowledge"></a>Grundläggande teknisk kunskap

Att utforma, skapa och testa dessa till gångar tar tid och kräver teknisk kunskap om både Azure-plattformen och teknikerna som används för att skapa erbjudandet.

Förutom din lösnings domän bör ditt tekniska team ha kunskap om följande Microsoft-tekniker:

- Grundläggande förståelse för [Azure-tjänster](https://azure.microsoft.com/services/)
- Så här [utformar och skapar du Azure-program](https://azure.microsoft.com/solutions/architecture/)
- Arbeta med kunskaper om [azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage)och [Azure-nätverk](https://azure.microsoft.com/services/?filter=networking)
- Arbeta med [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- Arbeta med [JSON](https://www.json.org/).

## <a name="suggested-tools"></a>Rekommenderade verktyg

Välj en eller båda av följande skript miljöer för att hantera din behållar avbildning:

- [Azure PowerShell](/powershell/azure/)
- [Azure CLI](/cli/azure/).

Vi rekommenderar att du lägger till dessa verktyg i utvecklings miljön:

- [Azure Storage Explorer](/azure/vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Tillägg: [Azure Resource Manager verktyg](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Tillägg: [beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Tillägg: [PRETTIFY JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

Granska de tillgängliga verktygen på sidan [Azure utvecklarverktyg](https://azure.microsoft.com/) . Om du använder Visual Studio granskar du de verktyg som är tillgängliga i [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-the-container-image"></a>Skapa behållar avbildningen

Du kan inte distribuera en avbildning till Azure Container Instances från ett lokalt register.

- Om du redan har en arbets behållare i det lokala registret skapar du ett Azure-register och laddar upp behållar avbildningen till Azure Container Registry. Mer information finns i [Självstudier: bygga och distribuera behållar avbildningar i molnet med Azure Container Registry uppgifter](/azure/container-registry/container-registry-tutorial-quick-task.md).

- Om du inte har en behållar avbildning ännu och behöver Använd ditt befintliga program eller skapa ett nytt behållar program, klona program käll koden från GitHub, skapa en behållar avbildning från program källan och testa avbildningen i en lokal Docker-miljö. Mer information finns i [Självstudier: skapa en behållar avbildning för distribution till Azure Container instances](/azure/container-instances/container-instances-tutorial-prepare-app.md).

## <a name="next-steps"></a>Nästa steg

- [Skapa ditt container erbjudande](create-azure-container-offer.md)
