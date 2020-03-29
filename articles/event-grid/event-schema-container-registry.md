---
title: Azure Olay Izgara Kapsayıcı Kayıt Defteri olay şeması
description: Azure Olay Ağıt'ı ile Konteyner Kayıt Defteri etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 03/12/2019
ms.author: spelluru
ms.openlocfilehash: c5998ff428c4b6f4c1f7a4087c6ccb27d93773eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60345473"
---
# <a name="azure-event-grid-event-schema-for-container-registry"></a>Konteyner Kayıt Defteri için Azure Olay Izgara olay şeması

Bu makalede, Kapsayıcı Kayıt Defteri olayları için özellikleri ve şema sağlar.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

## <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure Kapsayıcı Kayıt Defteri aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.ContainerRegistry.ImagePushed | Bir görüntü itildiğinde yükseltilir. |
| Microsoft.ContainerRegistry.ImageDeleted | Görüntü silindiğinde yükseltilir. |
| Microsoft.ContainerRegistry.ChartPushed | Bir Helm grafiği itildiğinde yükseltilir. |
| Microsoft.ContainerRegistry.ChartDeleted | Bir Miğfer grafiği silindiğinde yükseltilir. |

## <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, bir görüntünün itilen olayışeme sini gösterir: 

```json
[{
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld:v1",
  "eventType": "ImagePushed",
  "eventTime": "2018-04-25T21:39:47.6549614Z",
  "data": {
    "id": "31c51664-e5bd-416a-a5df-e5206bc47ed0",
    "timestamp": "2018-04-25T21:39:47.276585742Z",
    "action": "push",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 3023,
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "length": 3023,
      "repository": "aci-helloworld",
      "tag": "v1"
    },
    "request": {
      "id": "7c66f28b-de19-40a4-821c-6f5f6c0003a4",
      "host": "demo.azurecr.io",
      "method": "PUT",
      "useragent": "docker/18.03.0-ce go/go1.9.4 git-commit/0520e24 os/windows arch/amd64 UpstreamClient(Docker-Client/18.03.0-ce \\\\(windows\\\\))"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Silinen bir görüntünün şeması benzer:

```json
[{
  "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld",
  "eventType": "ImageDeleted",
  "eventTime": "2018-04-26T17:56:01.8211268Z",
  "data": {
    "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
    "timestamp": "2018-04-26T17:56:00.996603117Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "repository": "aci-helloworld"
    },
    "request": {
      "id": "aeda5b99-4197-409f-b8a8-ff539edb7de2",
      "host": "demo.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Grafik itilen bir olayın şeması, görüntülenmiş bir itme olayının şemasına benzer, ancak bir istek nesnesi içermez:

```json
[{
  "id": "ea3a9c28-5b17-40f6-a500-3f02b6829277",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "mychart:1.0.0",
  "eventType": "Microsoft.ContainerRegistry.ChartPushed",
  "eventTime": "2019-03-12T22:16:31.5164086Z",
  "data": {
    "id":"ea3a9c28-5b17-40f6-a500-3f02b682927",
    "timestamp":"2019-03-12T22:16:31.0087496+00:00",
    "action":"chart_push",
    "target":{
      "mediaType":"application/vnd.acr.helm.chart",
      "size":25265,
      "digest":"sha256:7f060075264b5ba7c14c23672698152ae6a3ebac1c47916e4efe19cd624d5fab",
      "repository":"repo",
      "tag":"mychart-1.0.0.tgz",
      "name":"mychart",
      "version":"1.0.0"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Silinen bir grafik için şema, görüntülenmiş silinmiş bir olayın şemasına benzer, ancak bir istek nesnesi içermez:

```json
[{
  "id": "39136b3a-1a7e-416f-a09e-5c85d5402fca",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "mychart:1.0.0",
  "eventType": "Microsoft.ContainerRegistry.ChartDeleted",
  "eventTime": "019-03-12T22:42:08.7034064Z",
  "data": {
    "id":"ea3a9c28-5b17-40f6-a500-3f02b682927",
    "timestamp":"2019-03-12T22:42:08.3783775+00:00",
    "action":"chart_delete",
    "target":{
      "mediaType":"application/vnd.acr.helm.chart",
      "size":25265,
      "digest":"sha256:7f060075264b5ba7c14c23672698152ae6a3ebac1c47916e4efe19cd624d5fab",
      "repository":"repo",
      "tag":"mychart-1.0.0.tgz",
      "name":"mychart",
      "version":"1.0.0"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Blob depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | string | Olay kimliği. |
| timestamp | string | Olayın meydana geldiği saat. |
| action | string | Sağlanan olayı kapsayan eylem. |
| Hedef | object | Olayın hedefi. |
| istek | object | Olayı oluşturan istek. |

Hedef nesne aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Mediatype | string | Başvurulan nesnenin MIME türü. |
| size | integer | İçeriğin bayt sayısı. Uzunluk alanıyla aynı. |
| digest | string | Kayıt Defteri V2 HTTP API Belirtimi tarafından tanımlandığı gibi içeriğin özeti. |
| length | integer | İçeriğin bayt sayısı. Boyut alanıyla aynı. |
| depo | string | Depo adı. |
| etiket | string | Etiket adı. |
| ad | string | Grafik adı. |
| version | string | Grafik versiyonu. |

İstek nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | string | Olayı başlatan isteğin kimliği. |
| Adresi | string | IP veya ana bilgisayar adı ve olayı başlatan istemci bağlantısının büyük olasılıkla bağlantı noktası. Bu değer, standart http isteğinden RemoteAddr'dır. |
| konak | string | Gelen isteklerde http ana bilgisayar üstbilgisinde belirtildiği gibi, kayıt defteri örneğinin dışarıdan erişilebilir ana adı. |
| method | string | Olayı oluşturan istek yöntemi. |
| Useragent | string | İsteğin kullanıcı aracısı üstbilgisi. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
