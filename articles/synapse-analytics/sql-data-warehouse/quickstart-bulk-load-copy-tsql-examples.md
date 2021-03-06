---
title: Autentiseringsmekanismer med KOPIERINGs instruktionen
description: Beskriver autentiseringsmekanismer för Mass inläsning av data
services: synapse-analytics
author: kevinvngo
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 07/10/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 9ed3a4b0827e81b3f779d95a6eab1dc341e69bb1
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96019386"
---
# <a name="securely-load-data-using-synapse-sql"></a>Läs in data på ett säkert sätt med Synapse SQL

Den här artikeln beskriver och innehåller exempel på mekanismer för säker autentisering för [kopierings instruktionen](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest). KOPIERINGs instruktionen är det mest flexibla och säkra sättet för Mass inläsning av data i Synapse SQL.
## <a name="supported-authentication-mechanisms"></a>Autentiseringsmekanismer som stöds

I följande matris beskrivs de autentiseringsmetoder som stöds för varje filtyp och lagrings konto. Detta gäller för käll lagrings platsen och fel filens plats.

|                          |                CSV                |              Parquet               |                ORC                 |
| :----------------------: | :-------------------------------: | :-------------------------------:  | :-------------------------------:  |
|  **Azure Blob Storage**  | SAS/MSI/TJÄNSTENS HUVUD NAMN/NYCKEL/AAD |              SAS/NYCKEL               |              SAS/NYCKEL               |
| **Azure Data Lake Gen2** | SAS/MSI/TJÄNSTENS HUVUD NAMN/NYCKEL/AAD | SAS (BLOB-slutpunkt)/MSI (DFS-slutpunkt)/SERVICE huvud namn/nyckel/AAD | SAS (BLOB-slutpunkt)/MSI (DFS-slutpunkt)/SERVICE huvud namn/nyckel/AAD |


## <a name="a-storage-account-key-with-lf-as-the-row-terminator-unix-style-new-line"></a>A. Lagrings konto nyckel med LF som rad avgränsare (UNIX-typ ny rad)


```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO target_table (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://adlsgen2account.dfs.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV'
    ,CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>')
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0QW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    ,ROWTERMINATOR='0x0A' --0x0A specifies to use the Line Feed character (Unix based systems)
)
```
> [!IMPORTANT]
>
> - Använd det hexadecimala värdet (0x0A) för att ange rad matnings tecknet. Obs! COPY-instruktionen tolkar strängen "\n" som "\r\n" (rad matnings retur).

## <a name="b-shared-access-signatures-sas-with-crlf-as-the-row-terminator-windows-style-new-line"></a>B. Signaturer för delad åtkomst (SAS) med CRLF som rad avslutning (Windows-formatmall, ny rad)
```sql
COPY INTO target_table
FROM 'https://adlsgen2account.dfs.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV'
    ,CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    ,ROWTERMINATOR='\n'-- COPY command automatically prefixes the \r character when \n (newline) is specified. This results in carriage return newline (\r\n) for Windows based systems.
)
```

> [!IMPORTANT]
>
> - Ange inte ROWTERMINATOR som ' \r\n ' som ska tolkas som ' \r\r\n ' och kan resultera i tolknings problem

## <a name="c-managed-identity"></a>C. Hanterad identitet

Hanterad identitets autentisering krävs när ditt lagrings konto är kopplat till ett VNet. 

### <a name="prerequisites"></a>Förutsättningar

1. Installera Azure PowerShell med hjälp av den här [guiden](/powershell/azure/install-az-ps?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
2. Om du har ett konto av typen generell användning v1 eller bloblagring måste du först uppgradera till generell användning v2 med hjälp av den här [guiden](../../storage/common/storage-account-upgrade.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
3. Du måste ha **Tillåt att betrodda Microsoft-tjänster har åtkomst till det här lagrings kontot** under Azure Storage konto **brand väggar och inställningar för virtuella nätverk** . Mer information finns i den här [guiden](../../storage/common/storage-network-security.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#exceptions).
#### <a name="steps"></a>Steg

1. I PowerShell **registrerar du SQL Server** med Azure Active Directory:

   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId your-subscriptionId
   Set-AzSqlServer -ResourceGroupName your-database-server-resourceGroup -ServerName your-database-servername -AssignIdentity
   ```

2. Skapa ett **Allmänt-syfte v2-lagrings konto** med hjälp av den här [guiden](../../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

   > [!NOTE]
   > Om du har ett allmänt v1-eller Blob Storage-konto måste du **först uppgradera till v2** med hjälp av den här [guiden](../../storage/common/storage-account-upgrade.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

3. Under ditt lagrings konto navigerar du till **Access Control (IAM)** och väljer **Lägg till roll tilldelning**. Tilldela din SQL Server rollen som **ägare av BLOB-dataägare, deltagare eller läsare** i Azure.

   > [!NOTE]
   > Endast medlemmar med ägar behörighet kan utföra det här steget. Information om olika inbyggda Azure-roller finns i den här [guiden](../../role-based-access-control/built-in-roles.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
   
    > [!IMPORTANT]
    > Ange **lagrings** **data** ägare, deltagare eller Reader Azure-roll. De här rollerna skiljer sig från de inbyggda Azure-rollerna för ägare, deltagare och läsare. 

    ![Bevilja Azure RBAC-behörighet att läsa in](./media/quickstart-bulk-load-copy-tsql-examples/rbac-load-permissions.png)

4. Nu kan du köra COPY-instruktionen som anger "hanterad identitet":

    ```sql
    COPY INTO dbo.target_table
    FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.txt'
    WITH (
        FILE_TYPE = 'CSV',
        CREDENTIAL = (IDENTITY = 'Managed Identity'),
    )
    ```

## <a name="d-azure-active-directory-authentication"></a>D. Azure Active Directory-autentisering
#### <a name="steps"></a>Steg

1. Under ditt lagrings konto navigerar du till **Access Control (IAM)** och väljer **Lägg till roll tilldelning**. Tilldela Azure AD-användaren rollen som **ägare av BLOB-dataägare, deltagare eller läsare** . 

    > [!IMPORTANT]
    > Ange **lagrings** **data** ägare, deltagare eller Reader Azure-roll. De här rollerna skiljer sig från de inbyggda Azure-rollerna för ägare, deltagare och läsare.

    ![Bevilja Azure RBAC-behörighet att läsa in](./media/quickstart-bulk-load-copy-tsql-examples/rbac-load-permissions.png)

2. Konfigurera Azure AD-autentisering genom att gå igenom följande [dokumentation](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure?tabs=azure-powershell#create-an-azure-ad-administrator-for-azure-sql-server). 

3. Anslut till din SQL-pool med Active Directory där du kan köra COPY-instruktionen utan att ange några autentiseringsuppgifter:

    ```sql
    COPY INTO dbo.target_table
    FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.txt'
    WITH (
        FILE_TYPE = 'CSV'
    )
    ```


## <a name="e-service-principal-authentication"></a>E. Autentisering av tjänstens huvudnamn
#### <a name="steps"></a>Steg

1. [Skapa ett Azure Active Directory program](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application)
2. [Hämta program-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)
3. [Hämta autentiseringsnyckel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret)
4. [Hämta v1 OAuth 2,0-token-slutpunkt](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications)
5. [Tilldela läs-, skriv-och körnings behörighet till ditt Azure AD-program](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#step-3-assign-the-azure-ad-application-to-the-azure-data-lake-storage-gen1-account-file-or-folder) på ditt lagrings konto
6. Nu kan du köra COPY-instruktionen:

    ```sql
    COPY INTO dbo.target_table
    FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
    WITH (
        FILE_TYPE = 'CSV'
        ,CREDENTIAL=(IDENTITY= '<application_ID>@<OAuth_2.0_Token_EndPoint>' , SECRET= '<authentication_key>')
        --CREDENTIAL should look something like this:
        --,CREDENTIAL=(IDENTITY= '92761aac-12a9-4ec3-89b8-7149aef4c35b@https://login.microsoftonline.com/72f714bf-86f1-41af-91ab-2d7cd011db47/oauth2/token', SECRET='juXi12sZ6gse]woKQNgqwSywYv]7A.M')
    )
    ```

> [!IMPORTANT]
>
> - Använd **v1** -versionen av OAuth 2,0-token-slutpunkt

## <a name="next-steps"></a>Nästa steg

- Mer information finns i artikeln om att [Kopiera instruktionen](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#syntax)
- Läs artikeln [Översikt över data inläsning](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading#what-is-elt) för att läsa in metod tips
