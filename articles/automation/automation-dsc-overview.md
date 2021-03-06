---
title: Översikt över Azure Automation tillstånds konfiguration
description: Den här artikeln innehåller en översikt över Azure Automation tillstånds konfiguration.
keywords: PowerShell DSC, önskad tillstånds konfiguration, PowerShell DSC Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 06/22/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 437a917e0f9b6e7a7370e828c8e3ee95218cea3f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87079748"
---
# <a name="azure-automation-state-configuration-overview"></a>Översikt över Azure Automation tillstånds konfiguration

Azure Automation tillstånds konfiguration är en Azure Configuration Management-tjänst som gör att du kan skriva, hantera och kompilera DSC- [konfigurationer](/powershell/scripting/dsc/configurations/configurations) (Desired State Configuration) för noder i ett moln eller lokalt Data Center. Tjänsten importerar också [DSC-resurser](/powershell/scripting/dsc/resources/resources)och tilldelar konfigurationer till mål noder, allt i molnet. Du kan komma åt Azure Automation tillstånds konfiguration i Azure Portal genom att välja **tillstånds konfiguration (DSC)** under **konfigurations hantering**.

Du kan använda Azure Automation tillstånds konfiguration för att hantera flera olika datorer:

- Virtuella Azure-datorer
- Virtuella Azure-datorer (klassisk)
- Fysiska/virtuella Windows-datorer lokalt eller i ett annat moln än Azure (inklusive AWS EC2-instanser)
- Fysiska/virtuella Linux-datorer lokalt, i Azure eller i ett annat moln än Azure

Om du inte är redo att hantera dator konfigurationen från molnet kan du använda Azure Automation tillstånds konfiguration som en slut punkt för endast rapporter. Med den här funktionen kan du ange (push) konfigurationer via DSC och Visa rapporterings information i Azure Automation.

> [!NOTE]
> Hantering av virtuella Azure-datorer med Azure Automation tillstånds konfiguration ingår utan extra kostnad om det installerade tillägget för Azure VM Desired State Configuration är större än 2,70. Mer information finns på [**sidan med automatiserings priser**](https://azure.microsoft.com/pricing/details/automation/).

## <a name="why-use-azure-automation-state-configuration"></a>Varför ska du använda konfiguration av Azure Automation tillstånd

Azure Automation tillstånds konfiguration ger flera fördelar jämfört med användningen av DSC utanför Azure. Den här tjänsten möjliggör skalbarhet på tusentals datorer snabbt och enkelt från en central, säker plats. Du kan enkelt aktivera datorer, tilldela dem deklarativ konfigurationer och Visa rapporter som visar varje dators kompatibilitet med önskat tillstånd som du anger.

Azure Automation tillstånds konfigurations tjänsten är till DSC vad Azure Automation runbooks är till PowerShell-skript. Med andra ord, på samma sätt som Azure Automation hjälper dig att hantera PowerShell-skript, kan du även hantera DSC-konfigurationer. 

### <a name="built-in-pull-server"></a>Inbyggd hämtnings Server

Azure Automation tillstånds konfiguration är en DSC-pull-server som liknar [Windows-funktionen DSC-service](/powershell/scripting/dsc/pull-server/pullserver). Mål noder kan automatiskt ta emot konfigurationer, följa det önskade läget och rapportera om deras efterlevnad. Den inbyggda hämtnings servern i Azure Automation eliminerar behovet av att konfigurera och underhålla en egen hämtnings Server. Azure Automation kan rikta in virtuella eller fysiska Windows-eller Linux-datorer, i molnet eller lokalt.

### <a name="management-of-all-your-dsc-artifacts"></a>Hantering av alla dina DSC-artefakter

Azure Automation tillstånds konfiguration får samma hanterings skikt till [PowerShell-önskad tillstånds konfiguration](/powershell/scripting/dsc/overview/overview) när det gäller PowerShell-skript. Från Azure Portal eller från PowerShell kan du hantera alla DSC-konfigurationer, resurser och mål-noder.

![Skärm bild av Azure Automation Sidan](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Import av rapport data till Azure Monitor loggar

Noder som hanteras med Azure Automation tillstånds konfiguration Skicka detaljerade rapporterings status data till den inbyggda hämtnings servern. Du kan konfigurera Azure Automation tillstånds konfiguration för att skicka dessa data till Log Analytics-arbetsytan. Se [vidarebefordra rapporterings data för Azure Automation tillstånds konfiguration till Azure Monitor loggar](automation-dsc-diagnostics.md).

## <a name="prerequisites"></a>Förutsättningar

Beakta kraven i det här avsnittet när du använder Azure Automation tillstånds konfiguration.

### <a name="operating-system-requirements"></a>Operativsystemskrav

För noder som kör Windows stöds följande versioner:

- Windows Server 2019
- Windows Server 2016
- Windows Server-2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8,1
- Windows 7

>[!NOTE]
>Den fristående produkt-SKU: n för [Microsoft Hyper-V server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) innehåller ingen implementering av DSC. Därför kan den inte hanteras av PowerShell DSC eller Azure Automation tillstånds konfiguration.

För noder som kör Linux stöder DSC Linux-tillägget alla Linux-distributioner som anges i [POWERSHELL DSC-dokumentationen](/powershell/scripting/dsc/getting-started/lnxgettingstarted).

### <a name="dsc-requirements"></a>DSC-krav

För alla Windows-noder som körs i Azure installeras [WMF 5,1](/powershell/scripting/wmf/setup/install-configure) när datorer är aktiverade. För noder som kör Windows Server 2012 och Windows 7 är [WinRM](/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) aktiverat.

För alla Linux-noder som körs i Azure installeras [POWERSHELL DSC för Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) när datorer är aktiverade.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Konfiguration av privata nätverk

Om noderna finns i ett privat nätverk krävs följande port och URL: er. De här resurserna ger nätverks anslutningen till den hanterade noden och tillåter DSC att kommunicera med Azure Automation.

* Port: endast TCP 443 krävs för utgående Internet åtkomst
* Global URL: ***. Azure-Automation.net**
* Global URL för US Gov, Virginia: ***. Azure-Automation.us**
* Agent tjänst: **https:// \<workspaceId\> . agentsvc.Azure-Automation.net**

Om du använder DSC-resurser som kommunicerar mellan noder, till exempel [waitfor *-resurserna](/powershell/scripting/dsc/reference/resources/windows/waitForAllResource), måste du också tillåta trafik mellan noderna. Se dokumentationen för varje DSC-resurs för att förstå dessa nätverks krav.

Information om klient kraven för TLS 1,2 finns i [TLS 1,2 Enforcement för Azure Automation](automation-managing-data.md#tls-12-enforcement-for-azure-automation).

#### <a name="proxy-support"></a>Stöd för proxy

Proxy-stöd för DSC-agenten finns i Windows version 1809 och senare. Det här alternativet är aktiverat genom att ange värdena för `ProxyURL` och `ProxyCredential` egenskaperna i [metaconfiguration-skriptet](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) som används för att registrera noder. 

>[!NOTE]
>Azure Automation tillstånds konfiguration ger inte stöd för DSC-proxy för tidigare versioner av Windows.

För Linux-noder stöder DSC-agenten proxy och använder `http_proxy` variabeln för att fastställa URL: en. Mer information om proxy-Support finns i [generera DSC-metaconfigurations](automation-dsc-onboarding.md#generate-dsc-metaconfigurations).

#### <a name="dns-records-per-region"></a>DNS-poster per region

Vi rekommenderar att du använder de adresser som anges i tabellen [DNS-poster per region](how-to/automation-region-dns-records.md) när du definierar undantag.

## <a name="next-steps"></a>Nästa steg

- Information om hur du kommer igång finns i [Kom igång med Azure Automation tillstånds konfiguration](automation-dsc-getting-started.md).
- Information om hur du aktiverar noder finns i [aktivera Azure Automation tillstånds konfiguration](automation-dsc-onboarding.md).
- Mer information om hur du kompilerar DSC-konfigurationer så att du kan tilldela dem till mål noder finns i [kompilera DSC-konfigurationer i Azure Automation tillstånds konfiguration](automation-dsc-compile.md).
- Om du vill se ett exempel på hur du använder Azure Automation tillstånds konfiguration i en pipeline för kontinuerlig distribution, se [Konfigurera kontinuerlig distribution med choklad](automation-dsc-cd-chocolatey.md).
- Pris information finns i pris information för [Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/).
- En PowerShell-cmdlet-referens finns i [AZ. Automation](/powershell/module/az.automation/?view=azps-3.7.0#automation).
