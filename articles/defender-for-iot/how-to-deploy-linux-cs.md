---
title: Installera & distribuera Linux C#-agenten
description: Lär dig hur du installerar och distribuerar säkerhets agenten Defender for IoT C#-baserad i Linux
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.custom: devx-track-csharp
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 48737831440a1402b6974955b4da61a4216b011f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941705"
---
# <a name="deploy-defender-for-iot-c-based-security-agent-for-linux"></a>Distribuera Defender för IoT C#-baserad säkerhets agent för Linux

I den här guiden beskrivs hur du installerar och distribuerar tjänsten Defender för IoT C#-baserad säkerhets agent på Linux.

I den här guiden får du lära du dig att:

> [!div class="checklist"]
> * Installera
> * Verifiera distributionen
> * Avinstallera agenten
> * Felsöka

## <a name="prerequisites"></a>Förutsättningar

För andra plattformar och agent-varianter, se [Välj rätt säkerhets agent](how-to-deploy-agent.md).

1. Om du vill distribuera säkerhets agenten krävs lokala administratörs rättigheter på den dator som du vill installera på.

1. [Skapa en säkerhetsmodul](quickstart-create-security-twin.md) för enheten.

## <a name="installation"></a>Installation

Använd följande steg för att distribuera säkerhets agenten:

1. Hämta den senaste versionen till datorn från [GitHub](https://aka.ms/iot-security-github-cs).

1. Extrahera innehållet i paketet och navigera till mappen _/install_ .

1. Lägg till behörigheter som körs i **InstallSecurityAgent-skriptet** genom att köra `chmod +x InstallSecurityAgent.sh`

1. Kör sedan följande kommando med **rot privilegier**:

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```

   Mer information om autentiseringsmetoder finns i [så här konfigurerar du autentisering](concept-security-agent-authentication-methods.md).

Det här skriptet utför följande åtgärder:

- Installerar nödvändiga komponenter.

- Lägger till en tjänst användare (med interaktiv inloggning inaktive rad).

- Installerar agenten som en **daemon** – förutsätter att enheten använder **system** för klassisk distributions modell.

- Konfigurerar **sudoers** så att agenten kan utföra vissa uppgifter som rot.

- Konfigurerar agenten med de angivna autentiseringsinställningarna.

Om du behöver ytterligare hjälp kör du skriptet med parametern – Help: `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Avinstallera agenten

Avinstallera agenten genom att köra skriptet med parametern – u: `./InstallSecurityAgent.sh -u` .

> [!NOTE]
> Avinstallationen tar inte bort saknade nödvändiga komponenter som installerades under installationen.

## <a name="troubleshooting"></a>Felsökning

1. Kontrol lera distributions statusen genom att köra:

    `systemctl status ASCIoTAgent.service`

1. Aktivera loggning.
   Om agenten inte kan starta aktiverar du loggning för att få mer information.

   Aktivera loggningen genom att:

   1. Öppna konfigurations filen för redigering i valfri Linux-redigerare:

        `vi /var/ASCIoTAgent/General.config`

   1. Redigera följande värden:

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" />
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```

       **LogFilePath** -värdet kan konfigureras.

       > [!NOTE]
       > Vi rekommenderar att du **inaktiverar utloggning när** fel sökningen har slutförts. **Om** du lämnar loggning ökar logg filens storlek och data användningen.

   1. Starta om agenten genom att köra:

       `systemctl restart ASCIoTAgent.service`

   1. Visa logg filen om du vill ha mer information om det här problemet.

       Logg filens plats är: `/var/ASCIoTAgent/IotAgentLog.log`

       Ändra sökvägen till fil platsen enligt det namn som du valde för **logFilePath** i steg 2.

## <a name="next-steps"></a>Nästa steg

- Läs [översikten över](overview.md) Defender for IoT-tjänsten
- Läs mer om Defender för IoT- [arkitektur](architecture.md)
- Aktivera [tjänsten](quickstart-onboard-iot-hub.md)
- Läs [vanliga frågor och svar](resources-frequently-asked-questions.md)
- Förstå [aviseringar](concept-security-alerts.md)
