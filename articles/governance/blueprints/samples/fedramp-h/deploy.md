---
title: Distribuera FedRAMP High skiss-exempel
description: Distribuera steg för FedRAMP-exemplet med hög skiss, inklusive information om skiss artefakts parametrar.
ms.date: 10/26/2020
ms.topic: sample
ms.openlocfilehash: 5d08eb932748ca01fd5780266248bde4025a3d2b
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92669312"
---
# <a name="deploy-the-fedramp-high-blueprint-sample"></a>Distribuera FedRAMP-exemplet för hög skiss

Följande steg måste vidtas för att distribuera Azure-skisser FedRAMP med hög skiss:

> [!div class="checklist"]
> - Skapa en ny skiss från exemplet
> - Märk din kopia av exemplet som **Publicerat**
> - Tilldela din kopia av skissen till en befintlig prenumeration

Om du inte har någon Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free) innan du börjar.

## <a name="create-blueprint-from-sample"></a>Skapa skiss från exempel

Börja först med att implementera skissexemplet genom att skapa en ny skiss i din miljö, med exemplet som utgångspunkt.

1. Välj **Alla tjänster** i den vänstra rutan. Sök efter och välj **Skisser** .

1. På sidan **Komma igång** till vänster väljer du knappen **Skapa** under _Skapa en skiss_ .

1. Leta upp exemplet på **FedRAMP med hög** skiss under _andra exempel_ och välj **Använd det här exemplet** .

1. Ange _Grundinställningar_ för skissexemplet:

   - **Skiss namn** : Ange ett namn för din kopia av exemplet på FedRAMP hög skiss.
   - **Definitions plats** : Använd ellipsen och välj den hanterings grupp där du vill spara din kopia av exemplet.

1. Välj fliken _Artefakter_ överst på sidan eller **Nästa: Artefakter** längst ned på sidan.

1. Granska listan med artefakter som utgör skissexemplet. Många av artefakterna innehåller parametrar som vi definierar senare. Välj **Spara utkast** när du har granskat skissexemplet.

## <a name="publish-the-sample-copy"></a>Publicera exempelkopian

Din kopia av skissexemplet har nu skapats i din miljö. Den skapas i läget **Utkast** och måste vara **Publicerad** innan den kan tilldelas och distribueras. Kopian av skiss exemplet kan anpassas efter din miljö och dina behov, men den ändringen kan flytta den bort från justeringen med FedRAMP hög kontroller.

1. Välj **Alla tjänster** i den vänstra rutan. Sök efter och välj **Skisser** .

1. Välj sidan **Skissdefinitioner** till vänster. Använd filtren för att hitta din kopia av skissexemplet och markera det sedan.

1. Välj **Publicera skiss** överst på sidan. På den nya sidan till höger anger du en **Version** för din kopia av skissexemplet. Den här egenskapen är användbar om du gör en ändring senare. Ange **ändrings anteckningar** , till exempel "första versionen som publicerats från FedRAMP-exemplet med hög skiss". Välj därefter **Publicera** längst ned på sidan.

## <a name="assign-the-sample-copy"></a>Tilldela exempelkopian

När kopieringen av skiss exemplet har **publicerats** kan den tilldelas en prenumeration i hanterings gruppen som den sparades till. I det här steget anges parametrar för att göra varje distribution av skissexemplets kopia unik.

1. Välj **Alla tjänster** i den vänstra rutan. Sök efter och välj **Skisser** .

1. Välj sidan **Skissdefinitioner** till vänster. Använd filtren för att hitta din kopia av skissexemplet och markera det sedan.

1. Välj **Tilldela skiss** överst på skissdefinitionssidan.

1. Ange parametervärden för skisstilldelningen:

   - Grundläggande inställningar

     - **Prenumerationer** : Välj en eller flera av de prenumerationer som finns i hanterings gruppen som du sparade din kopia av skiss exemplet till. Om du väljer mer än en prenumeration, skapas en tilldelning för var och en med de angivna parametrarna.
     - **Tilldelnings namn** : namnet fylls i automatiskt för dig baserat på namnet på skissen.
       Ändra vid behov eller lämna det som det är.
     - **Plats** : Välj en region som den hanterade identiteten ska skapas i. Azure Blueprint använder den här hanterade identiteten för att distribuera alla artefakter i den tilldelade skissen. Mer information finns i [hanterade identiteter för Azure-resurser](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Ritnings definitions version** : Välj en **publicerad** version av din kopia av skiss exemplet.

   - Lås tilldelning

     Välj skissens låsinställning för din miljö. Mer information finns i [Låsa skissresurser](../../concepts/resource-locking.md).

   - Hanterad identitet

     Lämna alternativet standard _tilldelade_ hanterade identiteter.

   - Artefaktparametrar

     De parametrar som definieras i avsnittet gäller för den artefakt som de definieras under. Dessa parametrar är [dynamiska parametrar](../../concepts/parameters.md#dynamic-parameters) eftersom de definieras när skissen tilldelas. En fullständig lista eller artefakt parametrar och deras beskrivningar finns i [tabellen artefakt parametrar](#artifact-parameters-table).

1. När alla parametrar har angetts väljer du **Tilldela** längst ned på sidan. Skisstilldelningen skapas och artefaktdistributionen påbörjas. Distributionen tar ungefär en timme. Du kan kontrollera statusen för distributionen genom att öppna skisstilldelningen.

> [!WARNING]
> Tjänsten Azure Blueprints och de inbyggda skissexemplen är **kostnadsfria** . Azures resurser [prissätts per produkt](https://azure.microsoft.com/pricing/). Använd [priskalkylatorn](https://azure.microsoft.com/pricing/calculator/) till att beräkna kostnaden för att köra de resurser som distribueras i skissexemplet.

## <a name="artifact-parameters-table"></a>Tabell med artefaktparametrar

I följande tabell visas en lista med skissartefaktens parametrar:

|Artefaktnamn|Artefakttyp|Parameternamn|Beskrivning|
|-|-|-|-|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Log Analytics arbetsyte-ID som virtuella datorer ska konfigureras för|Detta är ID (GUID) för Log Analytics arbets ytan som de virtuella datorerna ska konfigureras för.|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Lista över resurs typer som ska ha diagnostiska loggar aktiverade|Lista över resurs typer som ska granskas om diagnostisk logg inställning inte är aktive rad. Du hittar giltiga värden i [Azure Monitor-diagnostiska loggar scheman](../../../../azure-monitor/platform/resource-logs-schema.md#service-specific-schemas).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Lista över användare som ska undantas från gruppen Administratörer för virtuella Windows-datorer|En semikolonavgränsad lista med medlemmar som ska uteslutas i den lokala gruppen Administratörer. T. ex. administratör; Mina Användare1; myUser2|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Lista över användare som ska ingå i administratörs gruppen för en virtuell Windows-dator|En semikolonavgränsad lista med medlemmar som ska ingå i den lokala gruppen Administratörer. T. ex. administratör; Mina Användare1; myUser2|
|\[För hands version \] : distribuera Log Analytics agent för Linux VM Scale Sets (VMSS)|Principtilldelning|Log Analytics arbets yta för Linux VM Scale Sets (VMSS)|Om den här arbets ytan ligger utanför tilldelnings omfånget måste du manuellt bevilja behörigheten Log Analytics deltagare (eller liknande) till princip tilldelningens huvud-ID.|
|\[För hands version \] : distribuera Log Analytics agent för Linux VM Scale Sets (VMSS)|Principtilldelning|Valfritt: lista över virtuella dator avbildningar som har stöd för Linux-operativsystem som ska läggas till i omfånget|En tom matris kan användas för att ange inga valfria parametrar: \[\]|
|\[För hands version \] : distribuera Log Analytics agent för virtuella Linux-datorer|Principtilldelning|Log Analytics arbets yta för virtuella Linux-datorer|Om den här arbets ytan ligger utanför tilldelnings omfånget måste du manuellt bevilja behörigheten Log Analytics deltagare (eller liknande) till princip tilldelningens huvud-ID.|
|\[För hands version \] : distribuera Log Analytics agent för virtuella Linux-datorer|Principtilldelning|Valfritt: lista över virtuella dator avbildningar som har stöd för Linux-operativsystem som ska läggas till i omfånget|En tom matris kan användas för att ange inga valfria parametrar: \[\]|
|\[För hands version \] : distribuera Log Analytics agent för Windows VM Scale Sets (VMSS)|Principtilldelning|Log Analytics arbets yta för Windows VM Scale Sets (VMSS)|Om den här arbets ytan ligger utanför tilldelnings omfånget måste du manuellt bevilja behörigheten Log Analytics deltagare (eller liknande) till princip tilldelningens huvud-ID.|
|\[För hands version \] : distribuera Log Analytics agent för Windows VM Scale Sets (VMSS)|Principtilldelning|Valfritt: lista över virtuella dator avbildningar som har stöd för Windows OS som ska läggas till i omfånget|En tom matris kan användas för att ange inga valfria parametrar: \[\]|
|\[För hands version \] : distribuera Log Analytics agent för virtuella Windows-datorer|Principtilldelning|Log Analytics arbets yta för virtuella Windows-datorer|Om den här arbets ytan ligger utanför tilldelnings omfånget måste du manuellt bevilja behörigheten Log Analytics deltagare (eller liknande) till princip tilldelningens huvud-ID.|
|\[För hands version \] : distribuera Log Analytics agent för virtuella Windows-datorer|Principtilldelning|Valfritt: lista över virtuella dator avbildningar som har stöd för Windows OS som ska läggas till i omfånget|En tom matris kan användas för att ange inga valfria parametrar: \[\]|
|Distribuera Avancerat skydd på lagrings konton|Principtilldelning|Effekt|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|Distribuera granskning på SQL-servrar|Principtilldelning|Värdet i dagar för kvarhållningsperioden (0 anger obegränsad kvarhållning)|Retentions dagar (valfritt, 180 dagar om inget anges)|
|Distribuera granskning på SQL-servrar|Principtilldelning|Resurs grupp namn för lagrings konto för SQL Server-granskning|Granskning skriver databas händelser till en Gransknings logg i ditt Azure Storage konto (ett lagrings konto skapas i varje region där ett SQL Server skapas som kommer att delas av alla servrar i den regionen). Viktigt – för att kunna utföra granskningen tar du inte bort eller byter namn på resurs gruppen eller lagrings kontona.|
|Distribuera diagnostikinställningar för nätverks säkerhets grupper|Principtilldelning|Prefix för lagrings konto för diagnostik för nätverks säkerhets grupp|Det här prefixet kombineras med nätverks säkerhets gruppens plats för att bilda det skapade lagrings konto namnet.|
|Distribuera diagnostikinställningar för nätverks säkerhets grupper|Principtilldelning|Resurs grupp namn för lagrings konto för nätverksdiagnostik för nätverks säkerhets grupp (måste finnas)|Resurs gruppen som lagrings kontot kommer att skapas i. Resurs gruppen måste redan finnas.|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Tillåtna platser för resurser och resurs grupper|Lista över Azure-platser som din organisation kan ange när resurser distribueras. Det angivna värdet används också av principen för tillåtna platser i princip initiativet.|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Sårbarhets bedömning ska vara aktiverat på SQL-hanterade instanser|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Sårbarhets bedömning bör vara aktiverat på dina SQL-servrar|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Sårbarhets bedömning ska vara aktiverat på Virtual Machines|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Geo-redundant lagring ska vara aktiverat för lagrings konton|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Geo-redundant säkerhets kopiering måste aktive ras för Azure Database for MariaDB|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Geo-redundant säkerhets kopiering måste aktive ras för Azure Database for MySQL|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Geo-redundant säkerhets kopiering måste aktive ras för Azure Database for PostgreSQL|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Regler för nätverks säkerhets grupper för virtuella datorer som riktas mot Internet bör vara skärpta|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Webb program bör endast vara tillgängliga via HTTPS|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Funktionsapp bör endast vara tillgängligt via HTTPS|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Externa konton med Skriv behörighet bör tas bort från din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Externa konton med Läs behörighet bör tas bort från din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Externa konton med ägar behörigheter bör tas bort från din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Föråldrade konton med ägar behörigheter bör tas bort från din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Föråldrade konton bör tas bort från din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|CORS bör inte tillåta alla resurser åtkomst till ditt webb program|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|System uppdateringar på virtuella datorers skalnings uppsättningar bör installeras|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|MFA ska vara aktiverat på konton med Läs behörighet för din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|MFA ska vara aktiverat på konton med ägar behörigheter för din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|MFA ska vara aktiverat på konton med skrivbehörigheter för din prenumeration|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|
|\[För hands version \] : granska FedRAMP hög kontroller och distribuera särskilda VM-tillägg för att stödja gransknings krav|Principtilldelning|Långsiktig Geo-redundant säkerhets kopiering ska aktive ras för Azure SQL-databaser|Information om princip effekter finns i [förstå Azure policys effekter](../../../policy/concepts/effects.md).|


## <a name="next-steps"></a>Nästa steg

Nu när du har granskat stegen för att distribuera FedRAMP High skiss-exemplet kan du gå till följande artiklar om du vill veta mer om skiss-och kontroll mappning:

> [!div class="nextstepaction"]
> [FedRAMP hög skiss – översikt](./index.md) 
>  [FedRAMP av hög skiss-kontroll mappning](./control-mapping.md)

Ytterligare artiklar om skisser och hur de används:

- Mer information om [livscykeln för en skiss](../../concepts/lifecycle.md).
- Förstå hur du använder [statiska och dynamiska parametrar](../../concepts/parameters.md).
- Lär dig hur du anpassar [sekvensordningen för en skiss](../../concepts/sequencing-order.md).
- Lär dig hur du använder [resurslåsning för en skiss](../../concepts/resource-locking.md).
- Lär dig hur du [uppdaterar befintliga tilldelningar](../../how-to/update-existing-assignments.md).
