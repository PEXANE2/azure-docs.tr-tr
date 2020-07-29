---
title: Azure Media Services REST çağrıları için Postman ortamını içeri aktarın
description: Bu konu, Azure Media Services REST çağrıları için Postman ortamının bir tanımını sağlar.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78926706"
---
# <a name="import-the-postman-environment"></a>Postman ortamını içeri aktarma  

Bu makale, Media Services REST API 'Leri çağıran gruplandırılmış HTTP isteklerini içeren [Postman koleksiyonunu](postman-collection.md) kullanan **Postman** ortam değişkenlerinin tanımını içerir. Ortam ve koleksiyon dosyaları, [Media Services REST API çağrıları Için Postman yapılandırma](media-rest-apis-with-postman.md) öğreticisi tarafından kullanılır.

> [!NOTE]
> Değeri `AzureADSTSEndpoint `  =  `https://login.microsoftonline.com/{{TenantId}}/oauth2/token` . Kiracı KIMLIĞINIZI almak için, farenizi portalda (sağ üst köşede) Kullanıcı adınızın üzerine getirip "Dizin: Microsoft ({{TENANTıD}}) içinde bulabilirsiniz.

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
