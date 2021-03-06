---
title: Använda Azure import/export för att exportera data från Azure-blobbar | Microsoft Docs
description: Lär dig hur du skapar export jobb i Azure Portal för att överföra data från Azure-blobar.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 10/29/2020
ms.author: alkohli
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 39f9a5802d7f10753c8ea81bf414da195e137cc6
ms.sourcegitcommit: bbd66b477d0c8cb9adf967606a2df97176f6460b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93234145"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>Använda Azure Import/Export-tjänsten till att exportera data från Azure Blob Storage

Den här artikeln innehåller stegvisa instruktioner för hur du använder tjänsten Azure import/export för att på ett säkert sätt exportera stora mängder data från Azure Blob Storage. Tjänsten kräver att du levererar tomma enheter till Azure-datacentret. Tjänsten exporterar data från ditt lagrings konto till enheterna och levererar sedan enheterna tillbaka.

## <a name="prerequisites"></a>Förutsättningar

Innan du skapar ett export jobb för att överföra data från Azure Blob Storage bör du noggrant granska och slutföra följande lista över nödvändiga komponenter för tjänsten.
Du måste:

- Ha en aktiv Azure-prenumeration som kan användas för import/export-tjänsten.
- Ha minst ett Azure Storage konto. Se listan över [lagrings konton och lagrings typer som stöds för import/export-tjänsten](storage-import-export-requirements.md). Information om hur du skapar ett nytt lagrings konto finns i [så här skapar du ett lagrings konto](storage-account-create.md).
- Har tillräckligt många diskar av [typer som stöds](storage-import-export-requirements.md#supported-disks).
- Ha ett FedEx-/DHL-konto. Om du vill använda en annan operatör än FedEx/DHL kontaktar du Azure Data Box drifts team på `adbops@microsoft.com` .
  - Kontot måste vara giltigt, måste ha ett saldo och måste ha funktioner för retur leverans.
  - Generera ett spårnings nummer för export jobbet.
  - Varje jobb ska ha ett separat spårningsnummer. Det finns inte stöd för flera jobb med samma spårningsnummer.
  - Om du inte har ett transport företags konto går du till:
    - [Skapa ett FedEx-konto](https://www.fedex.com/en-us/create-account.html)eller
    - [Skapa ett DHL-konto](http://www.dhl-usa.com/en/express/shipping/open_account.html).

## <a name="step-1-create-an-export-job"></a>Steg 1: skapa ett export jobb

### <a name="portal"></a>[Portal](#tab/azure-portal)

Utför följande steg för att skapa ett export jobb i Azure Portal.

1. Logga in på <https://portal.azure.com/> .
2. Gå till **alla tjänster > lagring > import/export-jobb**.

    ![Gå till import/export-jobb](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Klicka på **skapa import/export-jobb**.

    ![Klicka på import/export-jobb](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. I **grunderna** :

    - Välj **exportera från Azure**.
    - Ange ett beskrivande namn på export jobbet. Använd det namn du väljer för att följa förloppet för dina jobb.
        - Namnet får bara innehålla gemena bokstäver, siffror, bindestreck och under streck.
        - Namnet måste börja med en bokstav och får inte innehålla blank steg.
    - Välj en prenumeration.
    - Ange eller Välj en resurs grupp.

        ![Grunder](./media/storage-import-export-data-from-blobs/export-from-blob3.png)

5. I **jobb information** :

    - Välj det lagrings konto där de data som ska exporteras finns. Använd ett lagrings konto nära den plats där du befinner dig.
    - DropOff-platsen fylls i automatiskt baserat på den region där det valda lagrings kontot finns.
    - Ange de BLOB-data som du vill exportera från ditt lagrings konto till din tomma enhet eller dina enheter.
    - Välj att **Exportera alla** BLOB-data i lagrings kontot.

         ![Exportera alla](./media/storage-import-export-data-from-blobs/export-from-blob4.png)

    - Du kan ange vilka behållare och blobbar som ska exporteras.
        - **Ange en blob som ska exporteras** : Använd **lika** med-väljaren. Ange den relativa sökvägen till blobben som börjar med behållar namnet. Använd *$root* för att ange rot containern.
        - **För att ange alla blobbar som börjar med ett prefix** : Använd Start **med** -väljaren. Ange prefixet som börjar med ett snedstreck (/). Prefixet kan vara prefixet för behållar namnet, det fullständiga behållar namnet eller det fullständiga behållar namnet följt av prefixet för BLOB-namnet. Du måste ange BLOB-sökvägar i giltigt format för att undvika fel under bearbetningen, som du ser i den här skärm bilden. Mer information finns i [exempel på giltiga BLOB-sökvägar](#examples-of-valid-blob-paths).

           ![Exportera valda behållare och blobbar](./media/storage-import-export-data-from-blobs/export-from-blob5.png)

    - Du kan exportera från BLOB list filen.

        ![Exportera från BLOB list filen](./media/storage-import-export-data-from-blobs/export-from-blob6.png)

   > [!NOTE]
   > Om blobben som ska exporteras används under data kopieringen tar Azure import/export-tjänsten en ögonblicks bild av blobben och kopierar ögonblicks bilden.

6. I **information om retur leverans** :

    - Välj operatören i list rutan. Om du vill använda en annan operatör än FedEx/DHL väljer du ett befintligt alternativ i list rutan. Kontakta Azure Data Box drifts teamet på `adbops@microsoft.com`  med information om den operatör som du planerar att använda.
    - Ange ett giltigt transportföretags konto nummer som du har skapat med transport företaget. Microsoft använder det här kontot för att skicka tillbaka enheterna till dig när ditt export jobb är klart.
    - Ange ett fullständigt och giltigt kontakt namn, telefon, e-postadress, gatuadress, ort, post, region och land/region.

        > [!TIP]
        > Ange en grupp-e-postadress i stället för att ange en e-postadress för en enskild användare. Detta säkerställer att du får meddelanden även om en administratör lämnar.

7. **Sammanfattning** :

    - Granska informationen om jobbet.
    - Anteckna jobb namnet och tillhandahåll leverans adressen för Azure Data Center för att leverera diskar till Azure.

        > [!NOTE]
        > Skicka alltid diskarna till data centret som anges i Azure Portal. Om diskarna levereras till fel Data Center kommer jobbet inte att bearbetas.

    - Klicka på **OK** för att slutföra genereringen av export jobb.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Använd följande steg för att skapa ett export jobb i Azure Portal.

[!INCLUDE [azure-cli-prepare-your-environment-h3.md](../../../includes/azure-cli-prepare-your-environment-h3.md)]

### <a name="create-a-job"></a>Skapa ett jobb

1. Använd kommandot [AZ Extension Add](/cli/azure/extension#az_extension_add) för att lägga till [AZ import-export-](/cli/azure/ext/import-export/import-export) tillägget:

    ```azurecli
    az extension add --name import-export
    ```

1. Om du vill hämta en lista över de platser som du kan ta emot diskar från, använder du kommandot [AZ import-export location List](/cli/azure/ext/import-export/import-export/location#ext_import_export_az_import_export_location_list) :

    ```azurecli
    az import-export location list
    ```

1. Kör följande [AZ import-export Create-](/cli/azure/ext/import-export/import-export#ext_import_export_az_import_export_create) kommando för att skapa ett export jobb som använder ditt befintliga lagrings konto:

    ```azurecli
    az import-export create \
        --resource-group myierg \
        --name Myexportjob1 \
        --location "West US" \
        --backup-drive-manifest true \
        --diagnostics-path waimportexport \
        --export blob-path=/ \
        --type Export \
        --log-level Verbose \
        --shipping-information recipient-name="Microsoft Azure Import/Export Service" \
            street-address1="3020 Coronado" city="Santa Clara" state-or-province=CA postal-code=98054 \
            country-or-region=USA phone=4083527600 \
        --return-address recipient-name="Gus Poland" street-address1="1020 Enterprise way" \
            city=Sunnyvale country-or-region=USA state-or-province=CA postal-code=94089 \
            email=gus@contoso.com phone=4085555555" \
        --storage-account myssdocsstorage
    ```

    > [!TIP]
    > Ange en grupp-e-postadress i stället för att ange en e-postadress för en enskild användare. Detta säkerställer att du får meddelanden även om en administratör lämnar.

   Det här jobbet exporterar alla blobbar i ditt lagrings konto. Du kan ange en BLOB för export genom att ersätta det här värdet för **--export** :

    ```azurecli
    --export blob-path=$root/logo.bmp
    ```

   Detta parameter värde exporterar blobben som heter *logo.bmp* i rot behållaren.

   Du kan också välja att välja alla blobbar i en behållare med hjälp av ett prefix. Ersätt detta värde för **--export** :

    ```azurecli
    blob-path-prefix=/myiecontainer
    ```

   Mer information finns i [exempel på giltiga BLOB-sökvägar](#examples-of-valid-blob-paths).

   > [!NOTE]
   > Om blobben som ska exporteras används under data kopieringen tar Azure import/export-tjänsten en ögonblicks bild av blobben och kopierar ögonblicks bilden.

1. Använd kommandot [AZ import-export List](/cli/azure/ext/import-export/import-export#ext_import_export_az_import_export_list) för att se alla jobb för resurs gruppen myierg:

    ```azurecli
    az import-export list --resource-group myierg
    ```

1. Om du vill uppdatera jobbet eller avbryta jobbet kör du kommandot [AZ import-export Update](/cli/azure/ext/import-export/import-export#ext_import_export_az_import_export_update) :

    ```azurecli
    az import-export update --resource-group myierg --name MyIEjob1 --cancel-requested true
    ```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Använd följande steg för att skapa ett export jobb i Azure PowerShell.

[!INCLUDE [azure-powershell-requirements-h3.md](../../../includes/azure-powershell-requirements-h3.md)]

> [!IMPORTANT]
> Även om **AZ. ImportExport** PowerShell-modulen är i för hands version måste du installera den separat med hjälp av `Install-Module` cmdleten. När den här PowerShell-modulen blir allmänt tillgänglig kommer den att ingå i framtida versioner av AZ PowerShell-modulen och är tillgängliga som standard i Azure Cloud Shell.

```azurepowershell-interactive
Install-Module -Name Az.ImportExport
```

### <a name="create-a-job"></a>Skapa ett jobb

1. Om du vill hämta en lista över platser som du kan ta emot diskar från använder du cmdleten [Get-AzImportExportLocation](/powershell/module/az.importexport/get-azimportexportlocation) :

   ```azurepowershell-interactive
   Get-AzImportExportLocation
   ```

1. Kör följande [New-AzImportExport-](/powershell/module/az.importexport/new-azimportexport) exempel för att skapa ett export jobb som använder ditt befintliga lagrings konto:

   ```azurepowershell-interactive
   $Params = @{
      ResourceGroupName = 'myierg'
      Name = 'Myexportjob1'
      Location = 'westus'
      BackupDriveManifest = $true
      DiagnosticsPath = 'waimportexport'
      ExportBlobListblobPath = '\'
      JobType = 'Export'
      LogLevel = 'Verbose'
      ShippingInformationRecipientName = 'Microsoft Azure Import/Export Service'
      ShippingInformationStreetAddress1 = '3020 Coronado'
      ShippingInformationCity = 'Santa Clara'
      ShippingInformationStateOrProvince = 'CA'
      ShippingInformationPostalCode = '98054'
      ShippingInformationCountryOrRegion = 'USA'
      ShippingInformationPhone = '4083527600'
      ReturnAddressRecipientName = 'Gus Poland'
      ReturnAddressStreetAddress1 = '1020 Enterprise way'
      ReturnAddressCity = 'Sunnyvale'
      ReturnAddressStateOrProvince = 'CA'
      ReturnAddressPostalCode = '94089'
      ReturnAddressCountryOrRegion = 'USA'
      ReturnAddressPhone = '4085555555'
      ReturnAddressEmail = 'gus@contoso.com'
      StorageAccountId = '/subscriptions/<SubscriptionId>/resourceGroups/myierg/providers/Microsoft.Storage/storageAccounts/myssdocsstorage'
   }
   New-AzImportExport @Params
   ```

    > [!TIP]
    > Ange en grupp-e-postadress i stället för att ange en e-postadress för en enskild användare. Detta säkerställer att du får meddelanden även om en administratör lämnar.

   Det här jobbet exporterar alla blobbar i ditt lagrings konto. Du kan ange en BLOB för export genom att ersätta värdet för **-ExportBlobListblobPath** :

   ```azurepowershell-interactive
   -ExportBlobListblobPath $root\logo.bmp
   ```

   Detta parameter värde exporterar blobben som heter *logo.bmp* i rot behållaren.

   Du kan också välja att välja alla blobbar i en behållare med hjälp av ett prefix. Ersätt det här värdet för **-ExportBlobListblobPath** :

   ```azurepowershell-interactive
   -ExportBlobListblobPath '/myiecontainer'
   ```

   Mer information finns i [exempel på giltiga BLOB-sökvägar](#examples-of-valid-blob-paths).

   > [!NOTE]
   > Om blobben som ska exporteras används under data kopieringen tar Azure import/export-tjänsten en ögonblicks bild av blobben och kopierar ögonblicks bilden.

1. Använd cmdleten [Get-AzImportExport](/powershell/module/az.importexport/get-azimportexport) för att se alla jobb för resurs gruppen myierg:

   ```azurepowershell-interactive
   Get-AzImportExport -ResourceGroupName myierg
   ```

1. Om du vill uppdatera jobbet eller avbryta jobbet kör du cmdleten [Update-AzImportExport](/powershell/module/az.importexport/update-azimportexport) :

   ```azurepowershell-interactive
   Update-AzImportExport -Name MyIEjob1 -ResourceGroupName myierg -CancelRequested
   ```

---

<!--## (Optional) Step 2: -->

## <a name="step-2-ship-the-drives"></a>Steg 2: leverera enheterna

Om du inte vet hur många enheter du behöver går du till [kontrol lera antalet enheter](#check-the-number-of-drives). Om du känner till antalet enheter kan du fortsätta med att leverera enheterna.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>Steg 3: uppdatera jobbet med spårnings information

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-4-receive-the-disks"></a>Steg 4: ta emot diskarna

När instrument panelen rapporterar att jobbet är klart skickas diskarna till dig och spårnings numret för leveransen är tillgängligt på portalen.

1. När du har fått enheterna med exporterade data måste du hämta BitLocker-nycklarna för att låsa upp enheterna. Gå till export jobbet i Azure Portal. Klicka på fliken **Importera/exportera** .
2. Markera och klicka på export jobbet i listan. Gå till **kryptering** och kopiera nycklarna.

   ![Visa BitLocker-nycklar för export jobb](./media/storage-import-export-data-from-blobs/export-from-blob-7.png)

3. Använd BitLocker-nycklar för att låsa upp diskarna.

Exporten är klar.

## <a name="step-5-unlock-the-disks"></a>Steg 5: Lås upp diskarna

Använd följande kommando för att låsa upp enheten:

   `WAImportExport Unlock /bk:<BitLocker key (base 64 string) copied from Encryption blade in Azure portal> /driveLetter:<Drive letter>`

Här är ett exempel på inmatade exempel.

   `WAImportExport.exe Unlock /bk:CAAcwBoAG8AdQBsAGQAIABiAGUAIABoAGkAZABkAGUAbgA= /driveLetter:e`

För tillfället kan du ta bort jobbet eller lämna det. Jobb tas automatiskt bort efter 90 dagar.

## <a name="check-the-number-of-drives"></a>Kontrol lera antalet enheter

Det här *valfria* steget hjälper dig att avgöra hur många enheter som krävs för export jobbet. Utför det här steget på ett Windows-system som kör en [operativ system version som stöds](storage-import-export-requirements.md#supported-operating-systems).

1. [Ladda ned WAImportExport version 1](https://www.microsoft.com/download/details.aspx?id=42659) på Windows-systemet.
2. Zippa upp till standardmappen `waimportexportv1` . Till exempel `C:\WaImportExportV1`.
3. Öppna ett PowerShell-eller kommando rads fönster med administratörs behörighet. Kör följande kommando för att ändra katalogen till den zippade mappen:

   `cd C:\WaImportExportV1`

4. Kör följande kommando för att kontrol lera antalet diskar som krävs för de valda blobarna:

   `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    Parametrarna beskrivs i följande tabell:

    |Kommando rads parameter|Description|
    |--------------------------|-----------------|
    |**/logdir:**|Valfritt. Logg katalogen. Utförliga loggfiler skrivs till den här katalogen. Om detta inte anges används den aktuella katalogen som logg katalog.|
    |**SN**|Krävs. Namnet på lagrings kontot för export jobbet.|
    |**sk**|Krävs endast om en behållar-SÄKERHETSASSOCIATIONER inte har angetts. Konto nyckeln för lagrings kontot för export jobbet.|
    |**/csas:**|Krävs endast om en lagrings konto nyckel inte har angetts. Behållar SAS för att lista de blobbar som ska exporteras i export jobbet.|
    |**/ExportBlobListFile:**|Krävs. Sökväg till XML-filen som innehåller en lista över BLOB-sökvägar eller prefix för BLOB-sökvägar för de blobbar som ska exporteras. Fil formatet som används i `BlobListBlobPath` -elementet i åtgärden [Put Job](/rest/api/storageimportexport/jobs) för att importera/exportera REST API.|
    |**/DriveSize:**|Krävs. Storleken på de enheter som ska användas för ett export jobb, *t. ex.* 500 GB, 1,5 TB.|

    Se ett [exempel på PreviewExport-kommandot](#example-of-previewexport-command).

5. Kontrol lera att du kan läsa och skriva till de enheter som ska levereras för export jobbet.

### <a name="example-of-previewexport-command"></a>Exempel på PreviewExport-kommandon

Följande exempel visar `PreviewExport` kommandot:

```powershell
    WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB
```

Filen med export-BLOB-listan får innehålla BLOB-namn och blob-prefix, som du ser här:

```xml
<?xml version="1.0" encoding="utf-8"?>
<BlobList>
<BlobPath>pictures/animals/koala.jpg</BlobPath>
<BlobPathPrefix>/vhds/</BlobPathPrefix>
<BlobPathPrefix>/movies/</BlobPathPrefix>
</BlobList>
```

Azure import/export-verktyget visar en lista över alla blobbar som ska exporteras och beräknar hur de ska packas upp i enheter med den angivna storleken, med hänsyn till eventuella kostnader som behövs, och sedan beräknar antalet enheter som behövs för att lagra blobbar och disk användnings information.

Här är ett exempel på utdata, med informations loggar utelämnade:

```powershell
Number of unique blob paths/prefixes:   3
Number of duplicate blob paths/prefixes:        0
Number of nonexistent blob paths/prefixes:      1

Drive size:     500.00 GB
Number of blobs that can be exported:   6
Number of blobs that cannot be exported:        2
Number of drives needed:        3
        Drive #1:       blobs = 1, occupied space = 454.74 GB
        Drive #2:       blobs = 3, occupied space = 441.37 GB
        Drive #3:       blobs = 2, occupied space = 131.28 GB
```

## <a name="examples-of-valid-blob-paths"></a>Exempel på giltiga BLOB-sökvägar

I följande tabell visas exempel på giltiga BLOB-sökvägar:

   | Väljare | BLOB-sökväg | Description |
   | --- | --- | --- |
   | Börjar med |/ |Exporterar alla blobar i lagrings kontot |
   | Börjar med |/$root/ |Exporterar alla blobbar i rot behållaren |
   | Börjar med |/book |Exporterar alla blobar i alla behållare som börjar med prefix **boken** |
   | Börjar med |MP3 |Exporterar alla blobbar i container **musiken** |
   | Börjar med |/music/love |Exporterar alla blobbar i container **musiken** som börjar med prefixet **kärlek** |
   | Lika med |$root/logo.bmp |Exporterar BLOB- **logo.bmp** i rot behållaren |
   | Lika med |Videor/story.mp4 |Exporterar BLOB- **story.mp4** i behållar **videor** |

## <a name="next-steps"></a>Nästa steg

- [Visa jobb-och enhets status](storage-import-export-view-drive-status.md)
- [Granska import/export-krav](storage-import-export-requirements.md)
