---
title: Azure App konfiguration REST API-versions hantering
description: Referens sidor för versions hantering med hjälp av Azure App konfigurations REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 3a7f50b26d59501d2be3a0147fe89919819b50e6
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/22/2020
ms.locfileid: "95246375"
---
# <a name="versioning"></a>Versionshantering

Varje klientbegäran måste ha en explicit API-version som en frågesträngparametern. Till exempel: `https://{myconfig}.azconfig.io/kv?api-version=1.0`.

`api-version` uttrycks i formatet SemVer (Major. minor). Intervall-eller versions förhandling stöds inte.

Den här artikeln gäller API version 1,0.

Nedan visas en sammanfattning av de möjliga fel svaren som returneras av servern när den begärda API-versionen inte kan matchas.

## <a name="api-version-unspecified"></a>Ingen API-version har angetts

Felet uppstår när en klient gör en begäran utan att ange en API-version.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "API version is not specified",
  "name": "api-version",
  "detail": "An API version is required, but was not specified.",
  "status": 400
}
```

## <a name="unsupported-api-version"></a>API-versionen stöds inte

Felet uppstår när en klient begärd API-version inte matchar någon av de API-versioner som stöds av servern.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Unsupported API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

## <a name="invalid-api-version"></a>Ogiltig API-version

Felet uppstår när en klient gör en begäran med en API-version, men värdet är felaktigt eller kan inte parsas av servern.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8  
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

## <a name="ambiguous-api-version"></a>Tvetydig API-version

Felet uppstår när en klient begär en API-version som är tvetydig för servern (till exempel flera olika värden).

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Ambiguous API version",
  "name": "api-version",
  "detail": "The following API versions were requested: {comma separated api versions}. At most, only a single API version may be specified. Please update the intended API version and retry the request.",
  "status": 400
}
```
