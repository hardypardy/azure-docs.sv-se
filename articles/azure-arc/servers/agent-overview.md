---
title: Översikt över den anslutna datorns Windows-agent
description: Den här artikeln innehåller en detaljerad översikt över Azure Arc-aktiverade Server Agent som har stöd för övervakning av virtuella datorer i hybrid miljöer.
ms.date: 09/30/2020
ms.topic: conceptual
ms.openlocfilehash: 8a66f99f535013b8aac52fdee43b91a8c734b10a
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/13/2020
ms.locfileid: "94577591"
---
# <a name="overview-of-azure-arc-enabled-servers-agent"></a>Översikt över Azure Arc-aktiverade Server Agent

Azure Arc-aktiverade servrar som är anslutna till dator agenten gör att du kan hantera dina Windows-och Linux-datorer utanför Azure i företags nätverket eller någon annan moln leverantör. Den här artikeln innehåller en detaljerad översikt över agent-, system-och nätverks krav och olika distributions metoder.

>[!NOTE]
>Från och med den allmänna versionen av Azure Arc-aktiverade servrar i september 2020 inaktive ras alla för hands versioner av Azures anslutna dator agenter (agenter med lägre versioner än **deprecated** 1,0) den **2 februari 2021**.  Med den här tids ramen kan du uppgradera till version 1,0 eller högre innan de förinstallerade agenterna inte längre kan kommunicera med tjänsten Azure Arc-aktiverade servrar.

## <a name="agent-component-details"></a>Information om agent komponent

Azure Connected Machine agent-paketet innehåller flera logiska komponenter, som sammanställs tillsammans.

* HIMDS (hybrid instance metadata service) hanterar anslutningen till Azure och den anslutna datorns Azure-identitet.

* Konfigurations agenten för gäster tillhandahåller In-Guest princip-och gäst konfigurations funktioner, till exempel bedöma om datorn uppfyller de nödvändiga principerna.

    Observera följande beteende med Azure Policy [gäst konfiguration](../../governance/policy/concepts/guest-configuration.md) för en frånkopplad dator:

    * En princip tilldelning för gäst konfiguration som riktar sig mot frånkopplade datorer påverkas inte.
    * Gäst tilldelning lagras lokalt i 14 dagar. Om den anslutna dator agenten återansluter till tjänsten inom 14-dagars perioden tillämpas princip tilldelningarna igen.
    * Tilldelningar tas bort efter 14 dagar och omtilldelas inte till datorn efter 14-dagars perioden.

* Tilläggs agenten hanterar VM-tillägg, inklusive installation, avinstallation och uppgradering. Tillägg laddas ned från Azure och kopieras till `%SystemDrive%\AzureConnectedMachineAgent\ExtensionService\downloads` mappen i Windows och för Linux till `/opt/GC_Ext/downloads` . I Windows installeras tillägget på följande sökväg `%SystemDrive%\Packages\Plugins\<extension>` och i Linux installeras tillägget på `/var/lib/waagent/<extension>` .

## <a name="download-agents"></a>Ladda ned agenter

Du kan ladda ned Azure Connected Machine agent-paketet för Windows och Linux från de platser som anges nedan.

* [Windows agent Windows Installer-paketet](https://aka.ms/AzureConnectedMachineAgent) från Microsoft Download Center.

* Linux Agent-paketet distribueras från Microsofts [paket lagrings plats](https://packages.microsoft.com/) med det önskade paket formatet för distributionen (. RPM eller. DEB).

Azure Connected Machine agent för Windows och Linux kan uppgraderas till den senaste versionen manuellt eller automatiskt beroende på dina behov. Mer information finns [här](manage-agent.md).

## <a name="prerequisites"></a>Förutsättningar

### <a name="supported-operating-systems"></a>Operativsystem som stöds

Följande versioner av operativ systemet Windows och Linux stöds officiellt för den Azure-anslutna dator agenten:

- Windows Server 2012 R2 och högre (inklusive Windows Server Core)
- Ubuntu 16,04 och 18,04 LTS (x64)
- CentOS Linux 7 (x64)
- SUSE Linux Enterprise Server (SLES) 15 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64)
- Amazon Linux 2 (x64)

### <a name="required-permissions"></a>Behörigheter som krävs

* För att publicera datorer är du medlem i rollen **Azure Connected Machine onboarding** .

* Om du vill läsa, ändra och ta bort en dator är du medlem i rollen **resurs administratör för Azure Connected Machine** . 

### <a name="azure-subscription-and-service-limits"></a>Azure-prenumeration och tjänstbegränsningar

Innan du konfigurerar dina datorer med Azure Arc-aktiverade servrar granskar du Azure Resource Manager [prenumerations gränser](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) och [resurs grupps gränser](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) för att planera för antalet datorer som ska anslutas.

### <a name="transport-layer-security-12-protocol"></a>Transport Layer Security 1,2-protokoll

För att säkerställa säkerheten för data som överförs till Azure rekommenderar vi starkt att du konfigurerar datorn att använda Transport Layer Security (TLS) 1,2. Äldre versioner av TLS/Secure Sockets Layer (SSL) har befunnits vara sårbara och även om de fortfarande arbetar för att tillåta bakåtkompatibilitet, rekommenderas de **inte**.

|Plattform/språk | Support | Mer information |
| --- | --- | --- |
|Linux | Linux-distributioner tenderar att förlita sig på [openssl](https://www.openssl.org) för TLS 1,2-stöd. | Kontrol lera [openssl-ändringsloggen](https://www.openssl.org/news/changelog.html) för att bekräfta att din version av OpenSSL stöds.|
| Windows Server 2012 R2 och senare | Stöds och är aktiverat som standard. | För att bekräfta att du fortfarande använder [standardinställningarna](/windows-server/security/tls/tls-registry-settings).|

### <a name="networking-configuration"></a>Nätverkskonfiguration

Den anslutna dator agenten för Linux och Windows kommunicerar på ett säkert sätt till Azure-bågen via TCP-port 443. Om datorn ansluter via en brand vägg eller proxyserver för att kommunicera via Internet kan du läsa igenom kraven nedan för att förstå kraven på nätverks konfigurationen.

Om den utgående anslutningen begränsas av din brand vägg eller proxyserver kontrollerar du att URL: erna i listan nedan inte är blockerade. Om du bara tillåter de IP-adressintervall eller domän namn som krävs för att agenten ska kunna kommunicera med tjänsten, måste du också tillåta åtkomst till följande service märken och URL: er.

Service märken:

* AzureActiveDirectory
* AzureTrafficManager
* AzureResourceManager
* AzureArcInfrastructure

Er

| Agentresurs | Beskrivning |
|---------|---------|
|`management.azure.com`|Azure Resource Manager|
|`login.windows.net`|Azure Active Directory|
|`dc.services.visualstudio.com`|Application Insights|
|`*.guestconfiguration.azure.com` |Gästkonfiguration|
|`*.his.arc.azure.com`|Hybrid identitets tjänst|

För hands versioner (version 0,11 och lägre) kräver också åtkomst till följande URL: er:

| Agentresurs | Beskrivning |
|---------|---------|
|`agentserviceapi.azure-automation.net`|Gästkonfiguration|
|`*-agentservice-prod-1.azure-automation.net`|Gästkonfiguration|

En lista över IP-adresser för varje service tag/region finns i JSON-filen – [Azure IP-intervall och service märken – offentligt moln](https://www.microsoft.com/download/details.aspx?id=56519). Microsoft publicerar veckovis uppdateringar som innehåller varje Azure-tjänst och de IP-intervall som används. Mer information finns i [service tag](../../virtual-network/network-security-groups-overview.md#service-tags).

URL: erna i föregående tabell krävs utöver informationen om tjänst Tagns IP-adressintervall eftersom majoriteten av tjänsterna för närvarande inte har en registrering av service tag. Därför kan IP-adresserna ändras. Om det krävs IP-adressintervall för brand Väggs konfigurationen ska **AzureCloud** -tjänst tag gen användas för att ge åtkomst till alla Azure-tjänster. Inaktivera inte säkerhetsövervakning eller granskning av dessa URL: er, Tillåt dem som andra Internet trafik.

### <a name="register-azure-resource-providers"></a>Registrera Azure-resurs leverantörer

Azure Arc-aktiverade servrar är beroende av följande Azure-resurs leverantörer i din prenumeration för att kunna använda den här tjänsten:

* **Microsoft. HybridCompute**
* **Microsoft. GuestConfiguration**

Om de inte är registrerade kan du registrera dem med hjälp av följande kommandon:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

Azure CLI:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Du kan också registrera resurs leverantörerna i Azure Portal genom att följa stegen under [Azure Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

## <a name="installation-and-configuration"></a>Installation och konfiguration

Att ansluta datorer i din hybrid miljö direkt med Azure kan utföras med hjälp av olika metoder beroende på dina behov. I följande tabell beskrivs varje metod för att avgöra vilken som fungerar bäst för din organisation.

> [!IMPORTANT]
> Den anslutna dator agenten kan inte installeras på en virtuell Azure Windows-dator. Om du försöker igen identifierar installationen detta och återställer.

| Metod | Beskrivning |
|--------|-------------|
| Interaktivt | Installera agenten manuellt på ett enda eller litet antal datorer enligt stegen i [ansluta datorer från Azure Portal](onboard-portal.md).<br> Från Azure Portal kan du generera ett skript och köra det på datorn för att automatisera installations-och konfigurations stegen för agenten.|
| I skala | Installera och konfigurera agenten för flera datorer efter att [ansluta datorerna med ett huvud namn för tjänsten](onboard-service-principal.md).<br> Den här metoden skapar ett huvud namn för tjänsten för att ansluta datorer icke-interaktivt.|
| I skala | Installera och konfigurera agenten för flera datorer enligt metoden [med hjälp av Windows POWERSHELL DSC](onboard-dsc.md).<br> Den här metoden använder ett huvud namn för tjänsten för att ansluta datorer icke-interaktivt med PowerShell DSC. |

## <a name="connected-machine-agent-technical-overview"></a>Teknisk översikt över anslutna dator agenter

### <a name="windows-agent-installation-details"></a>Installations information för Windows-agent

Den anslutna dator agenten för Windows kan installeras med hjälp av någon av följande tre metoder:

* Dubbelklicka på filen `AzureConnectedMachineAgent.msi` .
* Manuellt genom att köra Windows Installer-paketet `AzureConnectedMachineAgent.msi` från kommando gränssnittet.
* Från en PowerShell-session med en skriptad metod.

När du har installerat den anslutna dator agenten för Windows tillämpas följande ytterligare konfigurations ändringar i hela systemet.

* Följande installationsfiler skapas under installationen.

    |Mapp |Beskrivning |
    |-------|------------|
    |%ProgramFiles%\AzureConnectedMachineAgent |Standard installations Sök väg som innehåller agentens stödfiler.|
    |%ProgramData%\AzureConnectedMachineAgent |Innehåller konfigurationsfilerna för agenten.|
    |%ProgramData%\AzureConnectedMachineAgent\Tokens |Innehåller de hämtade token.|
    |%ProgramData%\AzureConnectedMachineAgent\Config |Innehåller agent konfigurations filen `agentconfig.json` som registrerar registrerings informationen med tjänsten.|
    |%SystemDrive%\Program Files\ArcConnectedMachineAgent\ExtensionService\GC | Installations Sök väg som innehåller gäst konfigurations agentens filer. |
    |%ProgramData%\GuestConfig |Innehåller principer (tillämpade) från Azure.|
    |%SystemDrive%\AzureConnectedMachineAgent\ExtensionService\downloads | Tillägg laddas ned från Azure och kopieras här.|

* Följande Windows-tjänster skapas på mål datorn under installationen av agenten.

    |Tjänstnamn |Visningsnamn |Processnamn |Beskrivning |
    |-------------|-------------|-------------|------------|
    |himds |Azure Hybrid-Instance Metadata Service |himds.exe |Den här tjänsten implementerar IMDS (Azure instance metadata service) för att hantera anslutningen till Azure och den anslutna datorns Azure-identitet.|
    |DscService |Gäst konfigurations tjänst |dsc_service.exe |Kodbasen för önskad tillstånds konfiguration (DSC v2) som används i Azure för att implementera In-Guest principen.|

* Följande miljövariabler skapas under Agent installationen.

    |Namn |Standardvärde |Beskrivning |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Det finns flera loggfiler tillgängliga för fel sökning. De beskrivs i följande tabell.

    |Loggas |Beskrivning |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Innehåller information om agenternas (HIMDS) tjänst och interaktion med Azure.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Innehåller utdata från azcmagent-verktygets kommandon när argumentet verbose (-v) används.|
    |%ProgramData%\GuestConfig\ gc_agent_logs \ gc_agent. log |Registrerar information om DSC-tjänstens aktivitet,<br> särskilt anslutningen mellan HIMDS-tjänsten och Azure Policy.|
    |% Program data% \GuestConfig\gc_agent_logs\gc_agent_telemetry.txt |Registrerar Detaljer om telemetri för DSC-tjänster och utförlig loggning.|
    |%SystemDrive%\ProgramData\GuestConfig\ ext_mgr_logs|Registrerar information om tilläggs Agent komponenten.|
    |%SystemDrive%\ProgramData\GuestConfig\ extension_logs\<Extension>|Registrerar information från det installerade tillägget.|

* Den lokala säkerhets gruppen **hybrid agent tilläggs program** skapas.

* Följande artefakter tas inte bort under avinstallationen av agenten.

    * *%ProgramData%\AzureConnectedMachineAgent\Log
    * %ProgramData%\AzureConnectedMachineAgent och under kataloger
    * %ProgramData%\GuestConfig

### <a name="linux-agent-installation-details"></a>Installations information för Linux-agenten

Den anslutna dator agenten för Linux finns i det önskade paket formatet för distributionen (. RPM eller. DEB) som finns i Microsoft- [paketets lagrings plats](https://packages.microsoft.com/). Agenten installeras och konfigureras med Shell-skript paketet [Install_linux_azcmagent. sh](https://aka.ms/azcmagent).

När du har installerat den anslutna dator agenten för Linux tillämpas följande ytterligare konfigurations ändringar i hela systemet.

* Följande installationsfiler skapas under installationen.

    |Mapp |Beskrivning |
    |-------|------------|
    |/var/opt/azcmagent/ |Standard installations Sök väg som innehåller agentens stödfiler.|
    |/opt/azcmagent/ |
    |/opt/GC_Ext | Installations Sök väg som innehåller gäst konfigurations agentens filer.|
    |/opt/DSC/ |
    |/var/opt/azcmagent/tokens |Innehåller de hämtade token.|
    |/var/lib/GuestConfig |Innehåller principer (tillämpade) från Azure.|
    |/opt/GC_Ext/downloads|Tillägg laddas ned från Azure och kopieras här.|

* Följande daemonar skapas på mål datorn under installationen av agenten.

    |Tjänstnamn |Visningsnamn |Processnamn |Beskrivning |
    |-------------|-------------|-------------|------------|
    |himdsd. service |Azure Hybrid-Instance Metadata Service |/opt/azcmagent/bin/himds |Den här tjänsten implementerar IMDS (Azure instance metadata service) för att hantera anslutningen till Azure och den anslutna datorns Azure-identitet.|
    |dscd. service |Gäst konfigurations tjänst |/opt/DSC/dsc_linux_service |Det här är den Desired State Configuration (DSC v2) kodbasen som används i Azure för att implementera In-Guest principen.|

* Det finns flera loggfiler tillgängliga för fel sökning. De beskrivs i följande tabell.

    |Loggas |Beskrivning |
    |----|------------|
    |/var/opt/azcmagent/log/himds.log |Innehåller information om agenternas (HIMDS) tjänst och interaktion med Azure.|
    |/var/opt/azcmagent/log/azcmagent.log |Innehåller utdata från azcmagent-verktygets kommandon när argumentet verbose (-v) används.|
    |/opt/logs/dsc.log |Registrerar information om DSC-tjänstens aktivitet,<br> särskilt anslutningen mellan himds-tjänsten och Azure Policy.|
    |/opt/logs/dsc.telemetry.txt |Registrerar Detaljer om telemetri för DSC-tjänster och utförlig loggning.|
    |/var/lib/GuestConfig/ext_mgr_logs |Registrerar information om tilläggs Agent komponenten.|
    |/var/lib/GuestConfig/extension_logs|Registrerar information från det installerade tillägget.|

* Följande miljövariabler skapas under Agent installationen. Dessa variabler anges i `/lib/systemd/system.conf.d/azcmagent.conf` .

    |Namn |Standardvärde |Beskrivning |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Följande artefakter tas inte bort under avinstallationen av agenten.

    * /var/opt/azcmagent
    * /opt/logs

## <a name="next-steps"></a>Nästa steg

* Börja utvärdera Azure Arc-aktiverade servrar genom att följa artikeln [Connect hybrid Machines to Azure från Azure Portal](onboard-portal.md).

* Felsöknings information finns i [fel söknings guide för anslutna datorer](troubleshoot-agent-onboard.md).