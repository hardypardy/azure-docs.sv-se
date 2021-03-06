---
title: Skapa och Använd vyer i SQL-poolen utan server (för hands version)
description: I det här avsnittet får du lära dig hur du skapar och använder vyer för att figursätta frågor om server utan SQL-pool (för hands version). Med vyer kan du återanvända dessa frågor. Vyer behövs också om du vill använda verktyg, till exempel Power BI, tillsammans med SQL-poolen utan server.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/20/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 302cf97db7c1d2ba489a84b6be912816d20f6091
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685570"
---
# <a name="create-and-use-views-using-serverless-sql-pool-preview-in-azure-synapse-analytics"></a>Skapa och Använd vyer med Server lös SQL-pool (för hands version) i Azure Synapse Analytics

I det här avsnittet får du lära dig hur du skapar och använder vyer för att figursätta frågor om server utan SQL-pool (för hands version). Med vyer kan du återanvända dessa frågor. Vyer behövs också om du vill använda verktyg, till exempel Power BI, tillsammans med SQL-poolen utan server.

## <a name="prerequisites"></a>Krav

Ditt första steg är att skapa en databas där vyn kommer att skapas och initiera de objekt som behövs för att autentisera i Azure Storage genom att köra [installations skriptet](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) på den databasen. Alla frågor i den här artikeln utförs i exempel databasen.

## <a name="create-a-view"></a>Skapa en vy

Du kan skapa vyer på samma sätt som du skapar vanliga SQL Server vyer. I följande fråga skapas en vy som läser *population.csv* -filen.

> [!NOTE]
> Ändra den första raden i frågan, t. ex. [mydbname], så att du använder den databas som du har skapat.

```sql
USE [mydbname];
GO

DROP VIEW IF EXISTS populationView;
GO

CREATE VIEW populationView AS
SELECT * 
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', 
        FIELDTERMINATOR =',', 
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r];
```

Vyn i det här exemplet använder `OPENROWSET` funktionen som använder absoluta sökvägar till de underliggande filerna. Om du har `EXTERNAL DATA SOURCE` en rot-URL för lagringen kan du använda `OPENROWSET` med `DATA_SOURCE` och relativ fil Sök väg:

```sql
CREATE VIEW TaxiView
AS SELECT *, nyc.filepath(1) AS [year], nyc.filepath(2) AS [month]
FROM
    OPENROWSET(
        BULK 'parquet/taxi/year=*/month=*/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc
```

## <a name="use-a-view"></a>Använda en vy

Du kan använda vyer i dina frågor på samma sätt som du använder vyer i SQL Server frågor.

Följande fråga visar hur du använder *population_csv* vy som vi skapade i [skapa en vy](#create-a-view). Namnet på land/region returneras med deras population i 2019 i fallande ordning.

> [!NOTE]
> Ändra den första raden i frågan, t. ex. [mydbname], så att du använder den databas som du har skapat.

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationView
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Nästa steg

Information om hur du frågar olika filtyper finns i artikeln [fråga en enkel CSV-fil](query-single-csv-file.md), [fråga Parquet-filer](query-parquet-files.md)och [fråga JSON-filer](query-json-files.md) .
