---
title: Support mat ris för VMware/fysisk haveri beredskap i Azure Site Recovery
description: Sammanfattar stöd för haveri beredskap för virtuella VMware-datorer och fysiska servrar till Azure med hjälp av Azure Site Recovery.
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: e3130242e29b8d3886b585d56d33d0a9a2379ee3
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/24/2020
ms.locfileid: "95800280"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>Stöd mat ris för haveri beredskap för virtuella VMware-datorer och fysiska servrar till Azure

I den här artikeln sammanfattas komponenter och inställningar som stöds för haveri beredskap för virtuella VMware-datorer och fysiska servrar till Azure med hjälp av [Azure Site Recovery](site-recovery-overview.md).

- [Lär dig mer](vmware-azure-architecture.md) om haveri beredskap för virtuella VMware-datorer/fysiska servrar.
- Följ våra [självstudier](tutorial-prepare-azure.md) för att testa haveri beredskap.

> [!NOTE]
> Site Recovery flyttar eller lagrar inte kund information från mål regionen, där haveri beredskap har kon figurer ATS för käll datorerna. Kunder kan välja ett Recovery Services valv från en annan region om de så vill. Recovery Services valvet innehåller metadata men inga faktiska kund uppgifter.

## <a name="deployment-scenarios"></a>Distributionsscenarier

**Scenario** | **Detaljer**
--- | ---
Haveri beredskap för virtuella VMware-datorer | Replikering av lokala virtuella VMware-datorer till Azure. Du kan distribuera det här scenariot i Azure Portal eller med hjälp av [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Haveri beredskap för fysiska servrar | Replikering av lokala Windows/Linux fysiska servrar till Azure. Du kan distribuera det här scenariot i Azure Portal.

## <a name="on-premises-virtualization-servers"></a>Lokala virtualiseringsservrar

**Server** | **Krav** | **Detaljer**
--- | --- | ---
vCenter Server | Version 7,0 & efterföljande uppdateringar i den här versionen, 6,7, 6,5, 6,0 eller 5,5 | Vi rekommenderar att du använder en vCenter-Server i återställnings distributionen för haveri beredskap.
vSphere-värdar | Version 7,0 & efterföljande uppdateringar i den här versionen, 6,7, 6,5, 6,0 eller 5,5 | Vi rekommenderar att vSphere-värdar och vCenter-servrar finns i samma nätverk som processervern. Som standard körs processervern på konfigurations servern. [Läs mer](vmware-physical-azure-config-process-server-overview.md).

## <a name="site-recovery-configuration-server"></a>Site Recovery konfigurations Server

Konfigurations servern är en lokal dator som kör Site Recovery-komponenter, inklusive konfigurations servern, processervern och huvud mål servern.

- För virtuella VMware-datorer ställer du in konfigurations servern genom att hämta en OVF-mall för att skapa en virtuell VMware-dator.
- För fysiska servrar konfigurerar du Configuration Server-datorn manuellt.

**Komponent** | **Krav**
--- |---
Processorkärnor | 8
RAM | 16 GB
Antal diskar | 3 diskar<br/><br/> Diskarna innehåller OS-disken, cache-disken för processervern och lagrings enheten för återställning efter fel.
Ledigt disk utrymme | 600 GB utrymme för processervern.
Ledigt disk utrymme | 600 GB utrymme för lagrings enheten.
Operativsystem  | Windows Server 2012 R2 eller Windows Server 2016 med Skriv bords miljö <br/><br> Om du planerar att använda det inbyggda huvud målet för den här enheten för återställning efter fel kontrollerar du att operativ system versionen är samma eller högre än de replikerade objekten.|
Nationella inställningar för operativsystem | Engelska (en-us)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | Krävs inte för konfigurations Server version [9,14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) eller senare.
Windows Server-roller | Aktivera inte Active Directory Domain Services; Internet Information Services (IIS) eller Hyper-V.
Grup principer| -Förhindra åtkomst till kommando tolken. <br/> -Förhindra åtkomst till verktyg för redigering av registret. <br/> – Förtroende logik för bifogade filer. <br/> – Aktivera skript körning. <br/> - [Läs mer](/previous-versions/windows/it-pro/windows-7/gg176671(v=ws.10))|
IIS | Kontrol lera att du:<br/><br/> -Har ingen befintlig standard webbplats <br/> -Aktivera  [Anonym autentisering](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731244(v=ws.10)) <br/> -Aktivera [FastCGI](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753077(v=ws.10)) -inställning  <br/> -Har inte redan befintlig webbplats/app-lyssning på port 443<br/>
Typ av nätverkskort | VMXNET3 (när den distribueras som en virtuell VMware-dator)
IP-adresstyp | Statisk
Portar | 443 som används för kontroll av kanal dirigering<br/>9443 för data transport

## <a name="replicated-machines"></a>Replikerade datorer

Site Recovery stöder replikering av alla arbets belastningar som körs på en dator som stöds.

**Komponent** | **Detaljer**
--- | ---
Dator inställningar | Datorer som replikeras till Azure måste uppfylla [Azure-kraven](#azure-vm-requirements).
Dator arbets belastning | Site Recovery stöder replikering av alla arbets belastningar som körs på en dator som stöds. [Läs mer](./site-recovery-workload.md).
Dator namn | Kontrol lera att visnings namnet på datorn inte hamnar i [reserverade resurs namn för Azure](../azure-resource-manager/templates/error-reserved-resource-name.md)<br/><br/> Logiska volym namn är inte Skift läges känsliga. Se till att det inte finns två volymer på en enhet med samma namn. Ex: volymer med namnen "voLUME1", "voLUME1" kan inte skyddas via Azure Site Recovery.

### <a name="for-windows"></a>För Windows

**Operativsystem** | **Detaljer**
--- | ---
Windows Server 2019 | Stöds från samlad [uppdatering 34](https://support.microsoft.com/help/4490016) (version 9,22 av mobilitets tjänsten) och senare.
Windows Server 2016 64-bitars | Stöds för Server Core, server med Skriv bords miljö.
Windows Server 2012 R2/Windows Server 2012 | Stöds.
Windows Server 2008 R2 med SP1 och senare. | Stöds.<br/><br/> Från version [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) av mobilitets tjänst agenten behöver du [underhålla stack uppdatering (självbetjänings)](https://support.microsoft.com/help/4490628) och [SHA-2-uppdateringen](https://support.microsoft.com/help/4474419) som är installerad på datorer som kör Windows 2008 R2 med SP1 eller senare. SHA-1 stöds inte från september 2019 och om SHA-2-kod signering inte är aktiverat installeras/uppgraderas inte som förväntat i agent tillägget. Läs mer om [SHA-2-uppgradering och krav](https://aka.ms/SHA-2KB).
Windows Server 2008 med SP2 eller senare (64-bitars/32-bitars) |  Stöds endast för migrering. [Läs mer](migrate-tutorial-windows-server-2008.md).<br/><br/> Från version [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) av mobilitets tjänst agenten behöver du [underhålla stack uppdatering (självbetjänings)](https://support.microsoft.com/help/4493730) och [SHA-2-uppdateringen](https://support.microsoft.com/help/4474419) som är installerad på Windows 2008 SP2-datorer. ISHA-1 stöds inte från september 2019 och om SHA-2-kod signering inte är aktiverat installeras/uppgraderas inte som förväntat i agent tillägget. Läs mer om [SHA-2-uppgradering och krav](https://support.microsoft.com/en-us/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus).
Windows 10, Windows 8.1, Windows 8 | Endast 64-bitars system stöds. 32-bitars system stöds inte.
Windows 7 med SP1 64-bitars | Stöds från samlad [uppdatering 36](https://support.microsoft.com/help/4503156) (version 9,22 av mobilitets tjänsten) och senare. </br></br> Från [9,30](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery) av mobilitets tjänst agenten behöver du [underhålla stack uppdatering (självbetjänings)](https://support.microsoft.com/help/4490628) och [SHA-2-uppdateringen](https://support.microsoft.com/help/4474419) som är installerad på datorer med Windows 7 SP1.  SHA-1 stöds inte från september 2019 och om SHA-2-kod signering inte är aktiverat installeras/uppgraderas inte som förväntat i agent tillägget. Läs mer om [SHA-2-uppgradering och krav](https://support.microsoft.com/en-us/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus).

### <a name="for-linux"></a>För Linux

**Operativsystem** | **Detaljer**
--- | ---
Linux | Endast 64-bitars system stöds. 32-bitars system stöds inte.<br/><br/>Alla Linux-servrar måste ha en installation av [Linux Integration Services (LIS)-komponenter](https://www.microsoft.com/download/details.aspx?id=55106) . Det krävs för att starta-servern i Azure efter redundanstestning/redundans. Om inbyggda LIS-komponenter saknas, se till att installera [komponenterna](https://www.microsoft.com/download/details.aspx?id=55106) innan du aktiverar replikering för de datorer som ska starta i Azure. <br/><br/> Site Recovery dirigerar redundans till att köra Linux-servrar i Azure. Linux-leverantörer kan dock begränsa stödet till endast distributions versioner som inte har uppnått livs längd.<br/><br/> I Linux-distributioner stöds bara de lager kärnor som ingår i distributionens lägre versions version/uppdatering.<br/><br/> Det finns inte stöd för att uppgradera skyddade datorer över stora versioner av Linux-distribution. För att uppgradera, inaktivera replikering, uppgradera operativ systemet och aktivera sedan replikering igen.<br/><br/> [Läs mer](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) om stöd för Linux och teknik med öppen källkod i Azure.
Linux Red Hat Enterprise | 5,2 till 5,11</b><br/> 6,1 till 6,10</b> </br> 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6, [7,7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [7,8](https://support.microsoft.com/help/4564347/), [7,9 Beta version](https://support.microsoft.com/help/4578241/), [7,9](https://support.microsoft.com/help/4590304/) </br> [8,0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8,1, [8,2](https://support.microsoft.com/help/4570609) <br/> Några äldre kärnor på servrar som kör Red Hat Enterprise Linux 5.2-5.11 & 6.1 – 6.10 har inte välinstallerade [komponenter för Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) . Om inbyggda LIS-komponenter saknas, se till att installera [komponenterna](https://www.microsoft.com/download/details.aspx?id=55106) innan du aktiverar replikering för de datorer som ska starta i Azure.
Linux: CentOS | 5,2 till 5,11</b><br/> 6,1 till 6,10</b><br/> </br> 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, 7,6, [7,7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [7,8](https://support.microsoft.com/help/4564347/), [7,9](https://support.microsoft.com/help/4578241/) </br> [8,0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8,1, [8,2](https://support.microsoft.com/help/4570609) <br/><br/> Några äldre kärnor på servrar som kör CentOS 5.2 – 5.11 & 6.1-6.10 har inte välinstallerade  [komponenter för Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) . Om inbyggda LIS-komponenter saknas, se till att installera [komponenterna](https://www.microsoft.com/download/details.aspx?id=55106) innan du aktiverar replikering för de datorer som ska starta i Azure.
Ubuntu | Ubuntu 14,04 * LTS [-Server (granska vilka kernel-versioner som stöds)](#ubuntu-kernel-versions)<br/>Ubuntu 16,04 * LTS [-Server (granska vilka kernel-versioner som stöds)](#ubuntu-kernel-versions) </br> Ubuntu 18,04 * LTS [-Server (granska vilka kernel-versioner som stöds)](#ubuntu-kernel-versions) </br> Ubuntu 20,04 * LTS [-Server (granska vilka kernel-versioner som stöds)](#ubuntu-kernel-versions) </br> (*har stöd för alla 14,04.* x *, 16,04.* x *, 18,04.* x *, 20,04.* x *-versioner)
Debian | Debian 7/Debian 8 (omfattar stöd för alla 7. *x*, 8. *x* -versioner); Debian 9 (innehåller stöd för 9,1 till 9,13. Debian 9,0 stöds inte.) [(granska kernel-versioner som stöds)](#debian-kernel-versions)
SUSE Linux | SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4, [SP5](https://support.microsoft.com/help/4570609) [(granska de kernel-versioner som stöds)](#suse-linux-enterprise-server-12-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 15, 15 SP1 [(granska stödda kernel-versioner)](#suse-linux-enterprise-server-15-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 11 SP3. [Se till att hämta de senaste installations program för mobilitets agenten på konfigurations servern](vmware-physical-mobility-service-overview.md#download-latest-mobility-agent-installer-for-suse-11-sp3-rhel-5-debian-7-server). </br> SUSE Linux Enterprise Server 11 SP4 </br> **Obs!** det finns inte stöd för att uppgradera replikerade datorer från SUSE Linux Enterprise Server 11 SP3 till SP4. Om du vill uppgradera inaktiverar du replikeringen och aktiverar den igen efter uppgraderingen. <br/>|
Oracle Linux | 6,4, 6,5, 6,6, 6,7, 6,8, 6,9, 6,10, 7,0, 7,1, 7,2, 7,3, 7,4, 7,5, [7,6, 7,7](https://support.microsoft.com/en-us/help/4531426/update-rollup-42-for-azure-site-recovery), [7,8](https://support.microsoft.com/help/4573888/), [8,0](https://support.microsoft.com/help/4573888/), [8,1](https://support.microsoft.com/help/4573888/), [8,2](https://support.microsoft.com/help/4573888/)  <br/> Köra Red Hat-kompatibel kernel eller Enterprise kernel release 3, 4 & 5 (UEK3, UEK4, UEK5)<br/><br/>8.1<br/>Att köra på alla UEK-kerneler och RedHat kernel <= 3.10.0-1062. * stöds i [9,35](https://support.microsoft.com/help/4573888/) -stöd för rest av RedHat-kärnan finns i [9,36](https://support.microsoft.com/help/4578241/)

> [!Note]
>- För var och en av Windows-versionerna stöder Azure Site Recovery endast [LTSC-versioner (långsiktig Servicing Channel)](/windows-server/get-started-19/servicing-channels-19#long-term-servicing-channel-ltsc) .  Det finns för närvarande inte stöd för de [halvårs tillgängliga Channel-](/windows-server/get-started-19/servicing-channels-19#semi-annual-channel) versionerna för tillfället.
>- Se till att för Linux-versioner Azure Site Recovery inte stöder anpassade OS-avbildningar. Endast de lager kerneler som ingår i distributionens lägre versions version/uppdatering stöds.

### <a name="ubuntu-kernel-versions"></a>Ubuntu kernel-versioner

**Version som stöds** | **Mobilitets tjänst version** | **Kernelversion** |
--- | --- | --- |
14,04 LTS | [9,34](https://support.microsoft.com/help/4570609), [9,35](https://support.microsoft.com/help/4573888/), [9,36](https://support.microsoft.com/help/4578241/), [9,37](https://support.microsoft.com/help/4582666/), [9,38](https://support.microsoft.com/help/4590304/) | 3.13.0-24-genered to 3.13.0-170-Generic,<br/>3.16.0-25-genered to 3.16.0-77-Generic,<br/>3.19.0-18-genered till 3.19.0-80-genered,<br/>4.2.0-18-genered to 4.2.0-42-Generic,<br/>4.4.0-21-genered to 4.4.0-148-genered,<br/>4.15.0 – 1023 – Azure till 4.15.0 – 1045 – Azure |
|||
16,04 LTS | [9,38](https://support.microsoft.com/help/4590304/) | 4.4.0-21-genered to 4.4.0-190-Generic,<br/>4.8.0-34-Generic to 4.8.0-58-Generic,<br/>4.10.0-14-generic to 4.10.0-42-Generic,<br/>4.11.0-13-genered to 4.11.0-14-generic,<br/>4.13.0-16-genered to 4.13.0-45-genered,<br/>4.15.0-13-genered to 4.15.0-118-generic<br/>4.11.0-1009 – Azure till 4.11.0-1016-Azure,<br/>4.13.0-1005 – Azure till 4.13.0 – 1018 – Azure <br/>4.15.0-1012 – Azure till 4.15.0-1096 – Azure|
16,04 LTS | [9,37](https://support.microsoft.com/help/4582666/) | 4.4.0-21-genered to 4.4.0-189-Generic,<br/>4.8.0-34-Generic to 4.8.0-58-Generic,<br/>4.10.0-14-generic to 4.10.0-42-Generic,<br/>4.11.0-13-genered to 4.11.0-14-generic,<br/>4.13.0-16-genered to 4.13.0-45-genered,<br/>4.15.0-13-genered to 4.15.0-115-generic<br/>4.11.0-1009 – Azure till 4.11.0-1016-Azure,<br/>4.13.0-1005 – Azure till 4.13.0 – 1018 – Azure <br/>4.15.0-1012 – Azure till 4.15.0-1093 – Azure |
16,04 LTS | [9,36](https://support.microsoft.com/help/4578241/)| 4.4.0 – 21-genered to 4.4.0-186-Generic,<br/>4.8.0-34-Generic to 4.8.0-58-Generic,<br/>4.10.0-14-generic to 4.10.0-42-Generic,<br/>4.11.0-13-genered to 4.11.0-14-generic,<br/>4.13.0-16-genered to 4.13.0-45-genered,<br/>4.15.0-13-genered to 4.15.0-112-generic<br/>4.11.0-1009 – Azure till 4.11.0-1016-Azure,<br/>4.13.0-1005 – Azure till 4.13.0 – 1018 – Azure <br/>4.15.0-1012 – Azure till 4.15.0-1092 – Azure |
16,04 LTS | [9,35](https://support.microsoft.com/help/4573888/) | 4.4.0-21-genered to 4.4.0-184-Generic,<br/>4.8.0-34-Generic to 4.8.0-58-Generic,<br/>4.10.0-14-generic to 4.10.0-42-Generic,<br/>4.11.0-13-genered to 4.11.0-14-generic,<br/>4.13.0-16-genered to 4.13.0-45-genered,<br/>4.15.0-13-genered to 4.15.0-106-generic<br/>4.11.0-1009 – Azure till 4.11.0-1016-Azure,<br/>4.13.0-1005 – Azure till 4.13.0 – 1018 – Azure <br/>4.15.0-1012 – Azure till 4.15.0-1089 – Azure |
16,04 LTS | [9,34](https://support.microsoft.com/help/4570609) | 4.4.0 – 21-genered to 4.4.0-178-Generic,<br/>4.8.0-34-Generic to 4.8.0-58-Generic,<br/>4.10.0-14-generic to 4.10.0-42-Generic,<br/>4.11.0-13-genered to 4.11.0-14-generic,<br/>4.13.0-16-genered to 4.13.0-45-genered,<br/>4.15.0-13-genered to 4.15.0-101-generic<br/>4.11.0-1009 – Azure till 4.11.0-1016-Azure,<br/>4.13.0-1005 – Azure till 4.13.0 – 1018 – Azure <br/>4.15.0-1012 – Azure till 4.15.0-1083 – Azure |
|||
18,04 LTS | [9,38](https://support.microsoft.com/help/4590304/) | 4.15.0-20-Generic to 4.15.0-118-generic </br> 4.18.0-13-Generic to 4.18.0-25-genered </br> 5.0.0-15-genered to 5.0.0-61-generic </br> 5.3.0-19-genered to 5.3.0-67-generic </br> 5.4.0 – 37-generisk till 5.4.0-48-genered</br> 4.15.0-1009 – Azure till 4.15.0-1096 – Azure </br> 4.18.0-1006 – Azure till 4.18.0 – 1025 – Azure </br> 5.0.0-1012 – Azure till 5.0.0-1036 – Azure </br> 5.3.0-1007 – Azure till 5.3.0-1035 – Azure </br> 5.4.0-1020-Azure till 5.4.0-tjänste – Azure|
18,04 LTS | [9,37](https://support.microsoft.com/help/4582666/) | 4.15.0-20-Generic to 4.15.0-115-generic </br> 4.18.0-13-Generic to 4.18.0-25-genered </br> 5.0.0-15-Generic to 5.0.0-60-generic </br> 5.3.0-19-genered to 5.3.0-66-generic </br> 5.4.0 – 37-generisk till 5.4.0-45-genered</br> 4.15.0-1009 – Azure till 4.15.0-1093 – Azure </br> 4.18.0-1006 – Azure till 4.18.0 – 1025 – Azure </br> 5.0.0-1012 – Azure till 5.0.0-1036 – Azure </br> 5.3.0-1007 – Azure till 5.3.0-1035 – Azure </br> 5.4.0-1020 – Azure till 5.4.0-1023 – Azure|
18,04 LTS | [9,36](https://support.microsoft.com/help/4578241/) | 4.15.0-20-Generic to 4.15.0-112-generic </br> 4.18.0-13-Generic to 4.18.0-25-genered </br> 5.0.0-15-genered to 5.0.0-58-generic </br> 5.3.0-19-genered to 5.3.0-64-genered </br> 5.4.0 – 37-generisk till 5.4.0-42-generic</br> 4.15.0-1009 – Azure till 4.15.0-1092 – Azure </br> 4.18.0-1006 – Azure till 4.18.0 – 1025 – Azure </br> 5.0.0-1012 – Azure till 5.0.0-1036 – Azure </br> 5.3.0-1007-Azure till 5.3.0-1032 – Azure </br> 5.4.0-1020 – Azure till 5.4.0 – 1022 – Azure|
18,04 LTS | [9,35](https://support.microsoft.com/help/4573888/) | 4.15.0-20-Generic to 4.15.0-108-generic </br> 4.18.0-13-Generic to 4.18.0-25-genered </br> 5.0.0-15-Generic to 5.0.0-52-generic </br> 5.3.0-19-genered to 5.3.0-61-generic </br> 4.15.0-1009 – Azure till 4.15.0-1089 – Azure </br> 4.18.0-1006 – Azure till 4.18.0 – 1025 – Azure </br> 5.0.0-1012 – Azure till 5.0.0-1036 – Azure </br> 5.3.0-1007-Azure till 5.3.0-1031-Azure|
18,04 LTS | [9,34](https://support.microsoft.com/help/4570609) | 4.15.0-20-Generic to 4.15.0-101-generic </br> 4.18.0-13-Generic to 4.18.0-25-genered </br> 5.0.0-15-genered to 5.0.0-48-genered </br> 5.3.0-19-genered to 5.3.0-53-generic </br> 4.15.0-1009 – Azure till 4.15.0-1083 – Azure </br> 4.18.0-1006 – Azure till 4.18.0 – 1025 – Azure </br> 5.0.0-1012 – Azure till 5.0.0-1036 – Azure </br> 5.3.0-1007-Azure till 5.3.0-1022 – Azure|
|||
20,04 LTS |[9,38](https://support.microsoft.com/help/4590304/) | 5.4.0 – 26-Generic to 5.4.0-48 </br> -Generic 5.4.0-1010-Azure till 5.4.0-tjänsteoch Azure
20,04 LTS |[9,37](https://support.microsoft.com/help/4582666/) | 5.4.0 – 26-Generic to 5.4.0-45 </br> -Generic 5.4.0-1010 – Azure till 5.4.0-1023 – Azure
20,04 LTS |[9,36](https://support.microsoft.com/help/4578241/) | 5.4.0-26-genered to 5.4.0-42 </br> -Generic 5.4.0-1010 – Azure till 5.4.0-1022 – Azure

### <a name="debian-kernel-versions"></a>Debian Kernel-versioner


**Version som stöds** | **Mobilitets tjänst version** | **Kernelversion** |
--- | --- | --- |
Debian 7 | [9,34](https://support.microsoft.com/help/4570609), [9,35](https://support.microsoft.com/help/4573888/), [9,36](https://support.microsoft.com/help/4578241/), [9,37](https://support.microsoft.com/help/4582666/), [9,38](https://support.microsoft.com/help/4590304/)| 3.2.0 – 4-amd64 till 3.2.0-6-amd64, 3.16.0 -0. bpo. 4-amd64 |
|||
Debian 8 | [9,35](https://support.microsoft.com/help/4573888/), [9,36](https://support.microsoft.com/help/4578241/), [9,37](https://support.microsoft.com/help/4582666/), [9,38](https://support.microsoft.com/help/4590304/) | 3.16.0 – 4-amd64 till 3.16.0-11-amd64, 4.9.0 -0. bpo. 4-amd64 till 4.9.0 -0. bpo. 11-amd64 |
Debian 8 | [9,34](https://support.microsoft.com/help/4570609) | 3.16.0 – 4-amd64 till 3.16.0-10-amd64, 4.9.0 -0. bpo. 4-amd64 till 4.9.0 -0. bpo. 11-amd64 |
|||
Debian 9,1 | [9,38](https://support.microsoft.com/help/4590304/) | 4.9.0-1-amd64 till 4.9.0-13-amd64 </br> 4.19.0 -0. bpo. 1-amd64 till 4.19.0 -0. bpo. 11-amd64 </br> 4.19.0 -0. bpo. 1 – Cloud-amd64 till 4.19.0 -0. bpo. 11-Cloud-amd64 </br> 
Debian 9,1 | [9,37](https://support.microsoft.com/help/4582666/) | 4.9.0-3-amd64 till 4.9.0-13-amd64, 4.19.0 -0. bpo. 6-amd64 till 4.19.0 -0. bpo. 10-amd64, 4.19.0 -0. bpo. 6 – Cloud-amd64 till 4.19.0 -0. bpo. 10 – Cloud-amd64

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 stödda kernel-versioner

**Frisläpp** | **Mobilitets tjänst version** | **Kernelversion** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9,38](https://support.microsoft.com/help/4590304/) | Alla [aktie-och SUSE 12 SP1-, SP2-, SP3-och SP4-kärnor](https://www.suse.com/support/kb/doc/?id=000019587) stöds.</br></br> 4.4.138-4,7-Azure till 4.4.180-4.31 – Azure</br>4.12.14-6.3 – Azure till 4.12.14-6.43 – Azure </br> 4.12.14-16,7 – Azure till 4.12.14-16.28 – Azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9,36](https://support.microsoft.com/help/4578241/), [9,37](https://support.microsoft.com/help/4582666/),  | Alla [aktie-och SUSE 12 SP1-, SP2-, SP3-och SP4-kärnor](https://www.suse.com/support/kb/doc/?id=000019587) stöds.</br></br> 4.4.138-4,7-Azure till 4.4.180-4.31 – Azure</br>4.12.14-6.3 – Azure till 4.12.14-6.43 – Azure </br> 4.12.14-16,7 – Azure till 4.12.14-16.22 – Azure  |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9,35](https://support.microsoft.com/help/4573888/) | Alla [aktie-och SUSE 12 SP1-, SP2-, SP3-och SP4-kärnor](https://www.suse.com/support/kb/doc/?id=000019587) stöds.</br></br> 4.4.138-4,7-Azure till 4.4.180-4.31 – Azure</br>4.12.14-6.3 – Azure till 4.12.14-6.43 – Azure </br> 4.12.14-16,7 – Azure till 4.12.14-16.19 – Azure  |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9,34](https://support.microsoft.com/help/4570609) | Alla [aktie-och SUSE 12 SP1-, SP2-, SP3-och SP4-kärnor](https://www.suse.com/support/kb/doc/?id=000019587) stöds.</br></br> 4.4.138-4,7-Azure till 4.4.180-4.31 – Azure</br>4.12.14-6.3 – Azure till 4.12.14-6.43 – Azure </br> 4.12.14-16,7 – Azure till 4.12.14-16.13 – Azure  |

### <a name="suse-linux-enterprise-server-15-supported-kernel-versions"></a>SUSE Linux Enterprise Server 15 kernel-versioner som stöds

**Frisläpp** | **Mobilitets tjänst version** | **Kernelversion** |
--- | --- | --- |
SUSE Linux Enterprise Server 15, SP1, SP2 | [9,38](https://support.microsoft.com/help/4590304/)  | Som standard stöds alla [börs-och 15-kernels](https://www.suse.com/support/kb/doc/?id=000019587) .</br></br> 4.12.14 – 5.5 – Azure till 4.12.14 – 5.47 – Azure </br></br> 4.12.14 – 8.5 – Azure till 4.12.14 – 8.44 – Azure </br> 5.3.18-16-Azure </br> 5.3.18-18.5 – Azure till 5.3.18-18.18 – Azure
SUSE Linux Enterprise Server 15 och 15 SP1 | [9,36](https://support.microsoft.com/help/4578241/), [9,37](https://support.microsoft.com/help/4582666/)  | Som standard stöds alla [börs-och 15-kernels](https://www.suse.com/support/kb/doc/?id=000019587) .</br></br> 4.12.14 – 5.5 – Azure till 4.12.14 – 5.47 – Azure </br></br> 4.12.14 – 8.5 – Azure till 4.12.14 – 8.38 – Azure
SUSE Linux Enterprise Server 15 och 15 SP1 | [9,35](https://support.microsoft.com/help/4573888/)  | Som standard stöds alla [börs-och 15-kernels](https://www.suse.com/support/kb/doc/?id=000019587) .</br></br> 4.12.14 – 5.5 – Azure till 4.12.14 – 5.47 – Azure </br></br> 4.12.14 – 8.5 – Azure till 4.12.14 – 8.33 – Azure 
SUSE Linux Enterprise Server 15 och 15 SP1 | [9,34](https://support.microsoft.com/help/4570609)  | Som standard stöds alla [börs-och 15-kernels](https://www.suse.com/support/kb/doc/?id=000019587) .</br></br> 4.12.14 – 5.5 – Azure till 4.12.14 – 5.47 – Azure </br></br> 4.12.14 – 8.5 – Azure till 4.12.14 – 8.30 – Azure 

## <a name="linux-file-systemsguest-storage"></a>Linux-filsystem/gäst lagring

**Komponent** | **Tillåtna**
--- | ---
Filsystem | ext3, ext4, XFS, BTRFS (villkor som gäller enligt den här tabellen)
Etablering av hantering av logiska volymer (LVM)| Tjock etablering – Ja <br></br> Tunn etablering – nej
Volym hanterare | -LVM stöds.<br/> -/Boot på LVM stöds från [Samlad uppdatering 31](https://support.microsoft.com/help/4478871/) (version 9,20 av mobilitets tjänsten) och senare. Den stöds inte i tidigare mobilitets tjänst versioner.<br/> -Flera operativ system diskar stöds inte.
Paravirtualiserade lagrings enheter | Enheter som exporteras av paravirtualiserade drivrutiner stöds inte.
Block-i/o-enheter med flera köer | Stöds inte.
Fysiska servrar med HP CCISS Storage Controller | Stöds inte.
Namngivnings konvention för enhet/monterings punkt | Enhets namnet eller monterings punktens namn måste vara unikt.<br/> Se till att inga enheter/monterings punkter har Skift läges känsliga namn. Exempelvis stöds inte namngivning av enheter för samma virtuella dator som *device1* och *device1* .
Kataloger | Om du kör en tidigare version av mobilitets tjänsten än version 9,20 (Publicerad i [Samlad uppdatering 31](https://support.microsoft.com/help/4478871/)) gäller följande begränsningar:<br/><br/> – Dessa kataloger (om de har ställts in som separata partitioner/fil system) måste finnas på samma OS-disk på käll servern:/(rot),/boot,/usr,/usr/local,/var,/etc.</br> -/Boot-katalogen bör finnas på en diskpartition och inte vara en LVM-volym.<br/><br/> Dessa begränsningar gäller inte från version 9,20 och senare. 
Start Katalog | – Start diskarna får inte vara i GPT-format. Detta är en begränsning för Azure-arkitekturen. GPT-diskar stöds som data diskar.<br/><br/> Det finns inte stöd för flera start diskar på en virtuell dator<br/><br/> -/Boot på en LVM-volym över mer än en disk stöds inte.<br/> -En dator utan en start disk kan inte replikeras.
Krav på ledigt utrymme| 2 GB på/root-partitionen <br/><br/> 250 MB på installationsmappen
XFSv5 | XFSv5-funktioner på XFS-filsystem, till exempel kontroll summa för metadata, stöds (mobilitets tjänst version 9,10 och senare).<br/> Använd xfs_info-verktyget för att kontrol lera XFS-superblocket för partitionen. Om `ftype` är inställt på 1 används XFSv5-funktionerna.
BTRFS | BTRFS stöds från samlad [uppdatering 34](https://support.microsoft.com/help/4490016) (version 9,22 av mobilitets tjänsten) och senare. BTRFS stöds inte om:<br/><br/> -BTRFS fil systemets del volym ändras efter att skyddet Aktiver ATS.</br> -BTRFS fil system sprids över flera diskar.</br> -Fil systemet BTRFS har stöd för RAID.

## <a name="vmdisk-management"></a>Hantering av virtuell dator/disk

**Åtgärd** | **Detaljer**
--- | ---
Ändra storlek på disk på replikerad virtuell dator | Stöds på den virtuella käll datorn före redundans, direkt i egenskaperna för den virtuella datorn. Du behöver inte inaktivera/återaktivera replikering.<br/><br/> Om du ändrar den virtuella käll datorn efter redundansväxlingen, fångas inte ändringarna.<br/><br/> Om du ändrar disk storleken på den virtuella Azure-datorn efter redundansväxlingen, skapar Site Recovery en ny virtuell dator med uppdateringarna när du växlar tillbaka.
Lägg till disk på replikerad virtuell dator | Stöds inte.<br/> Inaktivera replikering för den virtuella datorn, Lägg till disken och aktivera sedan replikering igen.

> [!NOTE]
> Alla ändringar av disk identiteten stöds inte. Om disk partitionering exempelvis har ändrats från GPT till MBR eller vice versa, ändrar detta disk identiteten. I ett sådant scenario avbryts replikeringen och en ny installation krävs. 

## <a name="network"></a>Nätverk

**Komponent** | **Tillåtna**
--- | ---
Värd nätverk NIC Teaming | Stöds för virtuella VMware-datorer. <br/><br/>Stöds inte för replikering av fysisk dator.
Värd nätverkets VLAN | Ja.
Värd nätverk IPv4 | Ja.
Värd nätverk IPv6 | Nej.
NIC-Teamning för gäst/Server-nätverk | Nej.
Gäst-/Server nätverk IPv4 | Ja.
Gäst-/Server nätverk IPv6 | Nej.
Statisk IP för gäst-/Server nätverk (Windows) | Ja.
Gäst-/Server nätverk statisk IP (Linux) | Ja. <br/><br/>Virtuella datorer konfigureras för att använda DHCP vid återställning efter fel.
Gäst-/Server nätverk flera nätverkskort | Ja.
Åtkomst till Site Recovery-tjänst via privat länk | Ja. [Läs mer](hybrid-how-to-enable-replication-private-endpoints.md).


## <a name="azure-vm-network-after-failover"></a>Azure VM-nätverk (efter redundans)

**Komponent** | **Tillåtna**
--- | ---
Azure ExpressRoute | Ja
ILB | Ja
ELB | Ja
Azure Traffic Manager | Ja
Multi-NIC | Ja
Reserverad IP adress | Ja
IPv4 | Ja
Behåll Källans IP-adress | Ja
Tjänstslutpunkter för virtuellt nätverk i Azure<br/> | Ja
Snabbare nätverk | Nej

## <a name="storage"></a>Storage
**Komponent** | **Tillåtna**
--- | ---
Dynamisk disk | OS-disken måste vara en standard disk. <br/><br/>Data diskar kan vara dynamiska diskar
Konfiguration av Docker-disk | Nej
Värd-NFS | Ja för VMware<br/><br/> Nej för fysiska servrar
Värd-SAN (iSCSI/FC) | Ja
Värd virtuellt San | Ja för VMware<br/><br/> Ej tillämpligt för fysiska servrar
Värd-multisökväg (MPIO) | Ja, testad med Microsoft DSM, EMC PowerPath 5,7 SP4, EMC PowerPath DSM för CLARiiON
Virtuella värd volymer (VVols) | Ja för VMware<br/><br/> Ej tillämpligt för fysiska servrar
Gäst/Server VMDK | Ja
Gäst-/Server delad kluster disk | Nej
Gäst/Server-krypterad disk | Nej
Gäst-/Server-NFS | Nej
Gäst-/Server-iSCSI | För migrering – Ja<br/>Vid haveri beredskap – nej kommer iSCSI att återställas efter fel som en ansluten disk till den virtuella datorn
Gäst/Server SMB 3,0 | Nej
Gäst/Server-RDM | Ja<br/><br/> Ej tillämpligt för fysiska servrar
Gäst-/Server disk > 1 TB | Ja, disken måste vara större än 1024 MB<br/><br/>Upp till 8 192 GB vid replikering till hanterade diskar (9,26-version och senare)<br></br> Upp till 4 095 GB vid replikering till lagrings konton
Gäst/Server-disk med fysisk och fysisk sektor storlek för 4K | Nej
Gäst-/Server disk med en fysisk sektor storlek på 4K och 512 byte | Nej
Gäst-/Server volym med Striped disk >4 TB | Ja
Hantering av logiska volymer (LVM)| Tjocka etablering – Ja <br></br> Tunn allokering – nej
Gäst/Server – lagrings utrymmen | Nej
Gäst/Server Hot Lägg till/ta bort disk | Nej
Gäst/Server – exkludera disk | Ja
Multisökväg för gäst/Server (MPIO) | Nej
GPT-partitioner för gäst/Server | Det finns stöd för fem partitioner från samlad [uppdatering 37](https://support.microsoft.com/help/4508614/) (version 9,25 av mobilitets tjänsten) och senare. Tidigare fyra stöddes.
ReFS | Elastiskt fil system stöds med mobilitets tjänst version 9,23 eller senare
Gäst/Server-EFI/UEFI-start | – Stöds för alla [UEFI-operativ system för Azure Marketplace](../virtual-machines/generation-2.md#generation-2-vm-images-in-azure-marketplace) med Site Recovery Mobility agent version 9,30 och senare. <br/> -Start typen för säker UEFI stöds inte. [Läs mer.](../virtual-machines/generation-2.md#on-premises-vs-azure-generation-2-vms)

## <a name="replication-channels"></a>Kanaler för replikering

|**Typ av replikering**   |**Tillåtna**  |
|---------|---------|
|Avlästa data överföringar (ODX)    |       Nej  |
|Seeding offline        |   Nej      |
| Azure Data Box | Nej

## <a name="azure-storage"></a>Azure-lagring

**Komponent** | **Tillåtna**
--- | ---
Lokalt redundant lagring | Ja
Geo-redundant lagring | Ja
Geo-redundant lagring med läsbehörighet (RA-GRS) | Ja
Cool Storage | Nej
Frekvent lagring| Nej
Blockblobar | Nej
Kryptering vid vila (SSE)| Ja
Kryptering vid vila (CMK)| Ja (via PowerShell AZ 3.3.0-modul och senare)
Dubbel kryptering i vila | Ja (via PowerShell AZ 3.3.0-modul och senare). Läs mer om regioner som stöds för [Windows](../virtual-machines/windows/disk-encryption.md) och [Linux](../virtual-machines/linux/disk-encryption.md).
Premium Storage | Ja
Alternativ för säker överföring | Ja
Import/export-tjänst | Nej
Azure Storage brand väggar för virtuella nätverk | Ja.<br/> Konfigurerat på mål lagring/cache lagrings konto (används för att lagra replikeringsdata).
Allmänna-syfte v2-lagrings konton (frekventa och låg frekventa nivåer) | Ja (transaktions kostnader är betydligt högre för v2 jämfört med v1)

## <a name="azure-compute"></a>Azure Compute

**Funktion** | **Tillåtna**
--- | ---
Tillgänglighetsuppsättningar | Ja
Tillgänglighetszoner | Nej
) | Ja
Hanterade diskar | Ja

## <a name="azure-vm-requirements"></a>Virtuella Azure VMware-datorer

Lokala virtuella datorer som replikeras till Azure måste uppfylla de krav på virtuella Azure-datorer som sammanfattas i den här tabellen. När Site Recovery kör en krav kontroll för replikering kommer kontrollen att Miss klar om några av kraven inte uppfylls.

**Komponent** | **Krav** | **Detaljer**
--- | --- | ---
Gästoperativsystem | Kontrol lera att [operativ system som stöds](#replicated-machines) för replikerade datorer. | Kontrollen Miss lyckas om den inte stöds.
Gäst operativ systemets arkitektur | 64-bitars. | Kontrollen Miss lyckas om den inte stöds.
Storlek på operativsystemdisk | Upp till 2 048 GB. | Kontrollen Miss lyckas om den inte stöds.
Antal operativsystemdiskar | 1 </br> Start-och systempartitionering på olika diskar stöds inte | Kontrollen Miss lyckas om den inte stöds.
Antal datadiskar | 64 eller mindre. | Kontrollen Miss lyckas om den inte stöds.
Data disk storlek | Upp till 8 192 GB vid replikering till hanterad disk (9,26-version och senare)<br></br>Upp till 4 095 GB vid replikering till lagrings konto| Kontrollen Miss lyckas om den inte stöds.
Nätverkskort | Flera nätverkskort stöds. |
Delad VHD | Stöds inte. | Kontrollen Miss lyckas om den inte stöds.
FC-disk | Stöds inte. | Kontrollen Miss lyckas om den inte stöds.
BitLocker | Stöds inte. | BitLocker måste inaktive ras innan du aktiverar replikering för en dator. |
VM-namn | Mellan 1 och 63 tecken.<br/><br/> Begränsat till bokstäver, siffror och bindestreck.<br/><br/> Dator namnet måste börja och sluta med en bokstav eller en siffra. |  Uppdatera värdet i dator egenskaperna i Site Recovery.

## <a name="resource-group-limits"></a>Resurs grupps gränser

Information om hur många virtuella datorer som kan skyddas under en enda resurs grupp finns i artikeln om [prenumerations gränser och kvoter](../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits).

## <a name="churn-limits"></a>Omsättnings gränser

Följande tabell innehåller gränserna för Azure Site Recovery.
- Dessa gränser är baserade på våra tester, men de gäller inte för alla möjliga kombinationer av app-I/O.
- De faktiska resultaten kan variera beroende på blandningen av I/O i ditt program.
- För bästa resultat rekommenderar vi starkt att du kör [verktyget Deployment Planner](site-recovery-deployment-planner.md)och utför omfattande program testning med hjälp av redundanstest för att få den sanna prestanda bilden för din app.

**Mål för replikering** | **Average Source Disk I/O Size** (Genomsnittlig I/O-storlek för källdisk) |**Average Source Disk Data Churn** (Genomsnittlig dataomsättning för källdisk) | **Total käll disk data omsättning per dag**
---|---|---|---
Standard Storage | 8 kB    | 2 MB/s | 168 GB per disk
Premium P10- eller P15-disk | 8 kB    | 2 MB/s | 168 GB per disk
Premium P10- eller P15-disk | 16 kB | 4 MB/s |    336 GB per disk
Premium P10- eller P15-disk | 32 kB eller mer | 8 MB/s | 672 GB per disk
Premium P20-, P30-, P40- eller P50-disk | 8 kB    | 5 MB/s | 421 GB per disk
Premium P20-, P30-, P40- eller P50-disk | minst 16 kB |20 MB/s | 1684 GB per disk


**Källans dataomsättning** | **Övre gräns**
---|---
Högsta dataomsättning av alla diskar på en virtuella dator | 54 MB/s
Maximal dataomsättning per dag som stöds av en processerver | 2 TB

- Det här är genomsnittliga värden baserade på en I/O-överlappning på 30 procent.
- Site Recovery kan hantera högre dataflöden med annan överlappning, större skrivningsstorlek och verkligt I/O-beteende under arbetsbelastningen.
- Dessa tal förutsätter en typisk efter släpning på ungefär fem minuter. Det vill säga, när data har överförts bearbetas de och en återställningspunkt skapas inom fem minuter.

## <a name="vault-tasks"></a>Valv aktiviteter

**Åtgärd** | **Tillåtna**
--- | ---
Flytta valv över resurs grupper | Nej
Flytta valvet inom och över prenumerationer | Nej
Flytta lagring, nätverk, virtuella Azure-datorer över resurs grupper | Nej
Flytta lagring, nätverk, virtuella Azure-datorer inom och över prenumerationer. | Nej


## <a name="obtain-latest-components"></a>Hämta de senaste komponenterna

**Namn** | **Beskrivning** | **Detaljer**
--- | --- | ---
Konfigurationsserver | Installerat lokalt.<br/> Koordinerar kommunikationen mellan lokala VMware-servrar eller fysiska datorer och Azure. | - [Läs mer om](vmware-physical-azure-config-process-server-overview.md) konfigurations servern.<br/> - [Lär dig mer om](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) att uppgradera till den senaste versionen.<br/> - [Lär dig hur](vmware-azure-deploy-configuration-server.md) du konfigurerar konfigurations servern.
Processerver | Installeras som standard på konfigurationsservern.<br/> Tar emot replikeringsdata, optimerar dem med cachelagring, komprimering och kryptering och skickar dem till Azure.<br/> När distributionen växer kan du lägga till ytterligare process servrar för att hantera större volymer av replikeringstrafiken. | - [Lär dig mer om](vmware-physical-azure-config-process-server-overview.md) processervern.<br/> - [Lär dig mer om](vmware-azure-manage-process-server.md#upgrade-a-process-server) att uppgradera till den senaste versionen.<br/> - [Lär dig mer om](vmware-physical-large-deployment.md#set-up-a-process-server) att konfigurera skalbara process servrar.
Mobilitetstjänst | Installerat på virtuella VMware-datorer eller fysiska servrar som du vill replikera.<br/> Samordnar replikering mellan lokala VMware-servrar/fysiska servrar och Azure.| - [Lär dig mer om](vmware-physical-mobility-service-overview.md) mobilitets tjänsten.<br/> - [Lär dig mer om](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) att uppgradera till den senaste versionen.<br/>



## <a name="next-steps"></a>Nästa steg
[Lär dig hur](tutorial-prepare-azure.md) du förbereder Azure för haveri beredskap för virtuella VMware-datorer.

[9.32 UR]: https://support.microsoft.com/en-in/help/4538187/update-rollup-44-for-azure-site-recovery
[9.31 UR]: https://support.microsoft.com/en-in/help/4537047/update-rollup-43-for-azure-site-recovery
[9.30 UR]: https://support.microsoft.com/en-in/help/4531426/update-rollup-42-for-azure-site-recovery
[9.29 UR]: https://support.microsoft.com/en-in/help/4528026/update-rollup-41-for-azure-site-recovery
[9.28 UR]: https://support.microsoft.com/en-in/help/4521530/update-rollup-40-for-azure-site-recovery
[9.27 UR]: https://support.microsoft.com/en-in/help/4517283/update-rollup-39-for-azure-site-recovery
[9.26 UR]: https://support.microsoft.com/en-in/help/4513507/update-rollup-38-for-azure-site-recovery
[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery