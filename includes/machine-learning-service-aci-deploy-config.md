---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 190da8fc98f3a03499188ab173f058d15cd2dafe
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027559"
---
Posterna i `deploymentconfig.json` dokument översikten till parametrarna för [AciWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py). I följande tabell beskrivs mappningen mellan entiteterna i JSON-dokumentet och parametrarna för-metoden:

| JSON-entitet | Metod parameter | Description |
| ----- | ----- | ----- |
| `computeType` | NA | Beräkningsmålet. För ACI måste värdet vara `ACI` . |
| `containerResourceRequirements` | NA | Behållare för processor-och minnes enheterna. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Antalet processor kärnor som ska allokeras. Appar `0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Mängden minne (i GB) som ska allokeras för den här webb tjänsten. Objekt `0.5` |
| `location` | `location` | Azure-regionen som den här webservicen ska distribueras till. Om den inte anges kommer arbets ytans plats användas. Mer information om tillgängliga regioner hittar du här: [ACI-regioner](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
| `authEnabled` | `auth_enabled` | Om autentisering ska aktive ras för den här webbtjänsten. Standardvärdet är false |
| `sslEnabled` | `ssl_enabled` | Om SSL ska aktive ras för den här webbtjänsten. Standardvärdet är false. |
| `appInsightsEnabled` | `enable_app_insights` | Huruvida AppInsights ska aktive ras för den här webbtjänsten. Standardvärdet är false |
| `sslCertificate` | `ssl_cert_pem_file` | Certifikat filen som behövs om SSL är aktiverat |
| `sslKey` | `ssl_key_pem_file` | Nyckel filen som behövs om SSL är aktiverat |
| `cname` | `ssl_cname` | CNAME för om SSL är aktiverat |
| `dnsNameLabel` | `dns_name_label` | DNS-namnets etikett för bedömnings slut punkten. Om du inte anger en unik DNS-benämning kommer att genereras för bedömnings slut punkten. |

Följande JSON är ett exempel på en distributions konfiguration för användning med CLI:

```json
{
    "computeType": "aci",
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    },
    "authEnabled": true,
    "sslEnabled": false,
    "appInsightsEnabled": false
}
```