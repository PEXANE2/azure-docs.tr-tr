---
title: Azure Media Services REST aramaları için Postacı ortamını alma
description: Bu konu, Azure Media Services REST aramaları için Postacı ortamının bir tanımını sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 8254d121c62a20de0a1593920b7793195f8eb50e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78926706"
---
# <a name="import-the-postman-environment"></a>Postacı ortamını alma  

Bu makalede, Media Services REST API'leri arayan gruplanmış HTTP isteklerini içeren [Postacı koleksiyonunda](postman-collection.md) kullanılan **Postacı** ortam değişkenlerinin bir tanımı nı içerir. Ortam ve toplama dosyaları [Medya Hizmetleri REST API öğretici için Yapılandırma Postacı](media-rest-apis-with-postman.md) tarafından kullanılır.

> [!NOTE]
> `AzureADSTSEndpoint `  = Değeri. `https://login.microsoftonline.com/{{TenantId}}/oauth2/token` Kiracı kimliğinizi almak için farenizi portaldaki kullanıcı adınızın üzerinde gezinebilirsiniz (sağ üst köşede) ve "Dizin: Microsoft ( {{TENANTID}} } ) .

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
