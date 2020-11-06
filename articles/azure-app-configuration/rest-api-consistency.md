---
title: Azure uygulama yapılandırma REST API tutarlılığı
description: Azure Uygulama yapılandırması ile gerçek zamanlı tutarlılık sağlamaya yönelik başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 4f11e6edcd4bc128f815db7e93b00b72bf990ea8
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424541"
---
# <a name="real-time-consistency"></a>Gerçek zamanlı tutarlılık

Bazı dağıtılmış sistemlerin doğası nedeniyle, istekler arasındaki gerçek zamanlı tutarlılık örtük olarak zorlamak zordur. Bir çözüm, birden fazla **eşitleme belirteci** biçiminde protokol desteğine izin verdir. Eşitleme belirteçleri isteğe bağlıdır.

## <a name="initial-request"></a>İlk istek

Farklı istemci örnekleri ve istekleri arasındaki gerçek zamanlı tutarlılığı güvence altına almak için isteğe bağlı `Sync-Token` istek/yanıt üst bilgilerini kullanın.

Söz dizimi:

```http
Sync-Token: <id>=<value>;sn=<sn>
```

|Parametre|Açıklama|
|--|--|
| `<id>` | Belirteç KIMLIĞI (donuk) |
| `<value>` | Belirteç değeri (donuk). Base64 kodlu dizeye izin verir |
| `<sn>` | Belirteç sıra numarası (sürüm). Daha yüksek olan aynı belirtecin daha yeni sürümü anlamına gelir. Daha iyi eşzamanlılık ve istemci önbelleklemesini sağlar. Belirteç sürümleri dahil olduğundan istemci yalnızca belirtecin son sürümünü kullanmayı seçebilir. İstekler için gerekli değildir. |

## <a name="response"></a>Yanıt

Hizmet, `Sync-Token` Her yanıta sahip bir üst bilgi sağlar.

```http
Sync-Token: jtqGc1I4=MDoyOA==;sn=28
```

## <a name="subsequent-requests"></a>Sonraki istekler

Sonraki tüm istekler, belirtilen **zamana göre gerçek zamanlı** tutarlı bir yanıt garanti edilir `Sync-Token` .

```http
Sync-Token: <id>=<value>
```

`Sync-Token`Üst bilgi istekten atlanırsa, hizmetin, dahili olarak bir süre boyunca (birkaç saniye kadar) önbelleğe alınmış verilerle yanıt vermesini sağlamak mümkündür. Bu davranış, okumadan hemen önce değişiklikler gerçekleştiyse tutarsız okumaların oluşmasına neden olabilir.

## <a name="multiple-sync-tokens"></a>Birden çok eşitleme belirteci

Sunucu, tek bir istek için birden çok eşitleme belirteci ile yanıt VEREBILIR. Bir sonraki istek için **gerçek zamanlı** tutarlılık sağlamak üzere istemcinin tüm alınan eşitleme belirteçleriyle yanıt vermesi gerekir. RFC başına, birden çok üst bilgi değeri virgülle ayrılmış olmalıdır.

```http
Sync-Token: <token1-id>=<value>,<token2-id>=<value>
```
