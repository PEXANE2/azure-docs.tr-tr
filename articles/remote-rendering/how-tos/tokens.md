---
title: Hizmet erişim belirteçlerini alma
description: ARR REST API 'Lerine erişmek için belirteçlerin nasıl oluşturulacağını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.openlocfilehash: fd510f90887353d7486908ee076d5308db72c59d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81687077"
---
# <a name="get-service-access-tokens"></a>Hizmet erişim belirteçlerini alma

ARR REST API 'Lerine erişim yalnızca yetkili kullanıcılar için verilir. Yetkilendirmeyi kanıtlamak için, REST istekleriyle birlikte bir *erişim belirteci* göndermeniz gerekir. Bu belirteçler, bir hesap anahtarı için Exchange 'de *güvenli belirteç hizmeti* (STS) tarafından verilir. Belirteçlerin **süresi 24 saat** olur ve bu nedenle kullanıcılara tam erişim izni vermeden kullanıcılara verilebilir.

Bu makalede, bu tür erişim belirtecinin nasıl oluşturulacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Henüz yoksa bir [ARR hesabı oluşturun](create-an-account.md).

## <a name="token-service-rest-api"></a>Belirteç hizmeti REST API

Erişim belirteçleri oluşturmak için, *güvenli belirteç hizmeti* tek bir REST API sağlar. ARR STS hizmeti URL 'SI: https: \/ /STS.mixedreality.Azure.com.

### <a name="get-token-request"></a>' Belirteci Al ' isteği

| URI | Yöntem |
|-----------|:-----------|
| /accounts/**AccountID**/Token | GET |

| Üst bilgi | Değer |
|--------|:------|
| Yetkilendirme | "Taşıyıcı **AccountID**:**accountkey**" |

*AccountID* ve *accountkey* değerlerini ilgili verilerle değiştirin.

### <a name="get-token-response"></a>' Belirteci Al ' yanıtı

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | AccessToken: dize | Başarılı |

| Üst bilgi | Amaç |
|--------|:------|
| MS-CV | Bu değer, hizmet içindeki çağrıyı izlemek için kullanılabilir |

## <a name="getting-a-token-using-powershell"></a>PowerShell kullanarak belirteç alma

Aşağıdaki PowerShell kodu, gerekli REST isteğinin STS 'ye nasıl gönderileceğini gösterir. Ardından belirteci PowerShell komut istemine yazdırır.

```PowerShell
$accountId = "<account_id_from_portal>"
$accountKey = "<account_key_from_portal>"

[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;
$webResponse = Invoke-WebRequest -Uri "https://sts.mixedreality.azure.com/accounts/$accountId/token" -Method Get -Headers @{ Authorization = "Bearer ${accountId}:$accountKey" }
$response = ConvertFrom-Json -InputObject $webResponse.Content

Write-Output "Token: $($response.AccessToken)"
```

Komut dosyası, bu belirteci kopyalamak & yapıştırabilirsiniz. Gerçek bir proje için bu işlemi otomatikleştirmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
* [Azure ön uç API 'Leri](../how-tos/frontend-apis.md)
* [Oturum yönetimi REST API](../how-tos/session-rest-api.md)
