---
title: Azure'da azaltma hatalarını giderme | Microsoft Dokümanlar
description: Azure İşlemi'nde hataları, yeniden denemeleri ve geri tepmeyi azaltma.
services: virtual-machines
documentationcenter: ''
author: changov
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: changov
ms.reviewer: vashan, rajraj
ms.openlocfilehash: f5fbd80fc9a8e519cf8f49ab16d7e747c6a8171b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045366"
---
# <a name="troubleshooting-api-throttling-errors"></a>API azaltma hatalarını giderme 

Azure İşlem istekleri, hizmetin genel performansına yardımcı olmak için bir abonelikte ve bölge başına olarak azaltılabilir. Microsoft.Compute ad alanı altındaki kaynakları yöneten Azure İşlem Kaynak Sağlayıcısı'na (CRP) yapılan tüm aramaların izin verilen maksimum API istek oranını aşmamasını sağlıyoruz. Bu belge, API azaltma, azaltma sorunlarının nasıl giderilenle ilgili ayrıntıları ve daraltılmaktan kaçınmak için en iyi uygulamaları açıklar.  

## <a name="throttling-by-azure-resource-manager-vs-resource-providers"></a>Azure Kaynak Yöneticisi ve Kaynak Sağlayıcıları tarafından azaltma  

Azure'un ön kapısı olarak Azure Kaynak Yöneticisi, gelen tüm API isteklerinin kimlik doğrulamasını ve birinci sınıf doğrulamasını ve azaltmasını yapar. Azure Kaynak Yöneticisi çağrı hızı sınırları ve ilgili [here](https://docs.microsoft.com/azure/azure-resource-manager/management/request-limits-and-throttling)tanılama yanıtı HTTP üstbilgiburada açıklanmıştır.
 
Bir Azure API istemcisi azaltma hatası aldığında, HTTP durumu 429 Çok Fazla İstek'tir. İstek azaltmanın Azure Kaynak Yöneticisi veya CRP gibi altta yatan bir kaynak `x-ms-ratelimit-remaining-subscription-reads` sağlayıcısı tarafından `x-ms-ratelimit-remaining-subscription-writes` yapIlip yapılmadığını anlamak için GET olmayan istekler için GET isteklerini ve yanıt üstbilgilerini inceleyin. Kalan çağrı sayısı 0'a yaklaşıyorsa, aboneliğin Azure Kaynak Yöneticisi tarafından tanımlanan genel çağrı sınırına ulaşıldı. Tüm abonelik istemcilerinin etkinlikleri birlikte sayılır. Aksi takdirde, azaltma hedef kaynak sağlayıcısından (istek URL'sinin `/providers/<RP>` segmenti tarafından ele alınan) gelir. 

## <a name="call-rate-informational-response-headers"></a>Arama oranı bilgilendirme yanıtı üstbilgileri 

| Üst bilgi                            | Değer biçimi                           | Örnek                               | Açıklama                                                                                                                                                                                               |
|-----------------------------------|----------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-ms-ratelimit-kalan-kaynak |```<source RP>/<policy or bucket>;<count>```| Microsoft.Compute/HighCostGet3Min;159 | Bu isteğin hedefini içeren kaynak kovasını veya işlem grubunu kapsayan azaltma ilkesi için kalan API çağrı sayısı                                                                   |
| x-ms-istek-şarj               | ```<count>```                             | 1                                     | Arama sayısı, ilgili politikanın sınırına yönelik bu HTTP isteği için "ücretlendirilir" sayılır. Bu en tipik olarak 1'dir. Sanal makine ölçeği kümesini ölçeklendirmek gibi toplu iş istekleri birden çok sayı yı şarj edebilir. |


Bir API isteğinin birden çok azaltma ilkelerine maruz kabileceğini unutmayın. Her ilke `x-ms-ratelimit-remaining-resource` için ayrı bir üstbilgi olacaktır. 

Burada sanal makine ölçek kümesi isteği silmek için örnek bir yanıttır.

```
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet3Min;107 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet30Min;587 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VMScaleSetBatchedVMRequests5Min;3704 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VmssQueuedVMOperations;4720 
```

## <a name="throttling-error-details"></a>Azaltma hatası ayrıntıları

429 HTTP durumu, çağrı hızı sınırına ulaşıldığı için isteği reddetmek için yaygın olarak kullanılır. Bilgi İşlem Kaynak Sağlayıcısı'ndan gelen tipik bir azaltma hatası yanıtı aşağıdaki örnekgibi görünür (yalnızca ilgili üstbilgi gösterilir):

```
HTTP/1.1 429 Too Many Requests
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet3Min;46
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet30Min;0
Retry-After: 1200
Content-Type: application/json; charset=utf-8
{
  "code": "OperationNotAllowed",
  "message": "The server rejected the request because too many requests have been received for this subscription.",
  "details": [
    {
      "code": "TooManyRequests",
      "target": "HighCostGet30Min",
      "message": "{\"operationGroup\":\"HighCostGet30Min\",\"startTime\":\"2018-06-29T19:54:21.0914017+00:00\",\"endTime\":\"2018-06-29T20:14:21.0914017+00:00\",\"allowedRequestCount\":800,\"measuredRequestCount\":1238}"
    }
  ]
}

```

Kalan çağrı sayısı 0 olan ilke, azaltma hatasının döndürüldİğİ politikadır. Bu durumda. `HighCostGet30Min` Yanıt gövdesinin genel biçimi genel Azure Kaynak Yöneticisi API hata biçimidir (OData ile uyumludur). Ana hata kodu, `OperationNotAllowed`, hesaplama kaynak sağlayıcısı (istemci hataları diğer türleri arasında) azaltma hataları bildirmek için kullandığı biridir. İç `message` hata(lar) özelliği azaltma ihlali ayrıntıları ile seri leştirilmiş bir JSON yapısı içerir.

Yukarıda gösterildiği gibi, her azaltma hatası, istemcinin isteği yeniden denemeden önce beklemesi gereken en az saniye sayısını sağlayan `Retry-After` üstbilgi içerir. 

## <a name="api-call-rate-and-throttling-error-analyzer"></a>API çağrı hızı ve azaltma hata çözümleyicisi
Bir sorun giderme özelliğinin önizleme sürümü, Bilgi İşlem kaynak sağlayıcısının API'si için kullanılabilir. Bu PowerShell cmdlets operasyon başına zaman aralığı başına API istek oranı ve operasyon grubu (ilke) başına azaltma ihlalleri hakkında istatistikler sağlar:
-   [Export-azloganalyticrequestratebyInterval](https://docs.microsoft.com/powershell/module/az.compute/export-azloganalyticrequestratebyinterval)
-   [İhracat-AzLogAnalyticThrottledRequest](https://docs.microsoft.com/powershell/module/az.compute/export-azloganalyticthrottledrequest)

API çağrı istatistikleri, bir aboneliğin istemcisinin(ler) davranışı hakkında harika bir fikir verebilir ve daraltma neden olan çağrı desenlerinin kolayca tanımlanmasını sağlayabilir.

Çözümleyicinin şimdilik bir sınırlaması, disk ve anlık görüntü kaynak türleri (yönetilen diskleri destekleyen) isteklerini saymamasıdır. CRP'nin telemetrisinden veri topladığı için ARM'den azaltma hatalarını belirlemede de yardımcı olamaz. Ancak bunlar, daha önce de ele alındığı gibi, ayırt edici ARM yanıt başlıklarına göre kolayca tanımlanabilir.

PowerShell cmdlets kolayca doğrudan istemciler tarafından çağrılabilir bir REST hizmet API kullanıyor (henüz resmi bir destek olsa da). HTTP istek biçimini görmek için cmdlets'i -Hata Ayıklama anahtarıyla çalıştırın veya Fiddler ile yürütmelerini incele.


## <a name="best-practices"></a>En iyi uygulamalar 

- Azure hizmet API hatalarını koşulsuz ve/veya hemen yeniden denemeyin. Sık karşılaşılan bir durum, istemci kodunun yeniden denene edilemeyen bir hatayla karşılaştığında hızlı bir yeniden deneme döngüsüne girmesidir. Yeniden denemeler sonunda hedef işlemin grubu için izin verilen arama sınırını tüketir ve aboneliğin diğer istemcilerini etkiler. 
- Yüksek hacimli API otomasyon durumlarında, hedef işlem grubu için kullanılabilir çağrı sayısı bazı düşük eşiğin altına düştüğünde proaktif istemci tarafı kendi kendini azaltma uygulamayı düşünün. 
- Async işlemlerini takip ederken, Yeniden Deneme-Sonra üstbilgi ipuçlarına saygı gösterin. 
- İstemci kodu belirli bir Sanal Makine hakkında bilgiye ihtiyaç duyarsa, içeren kaynak grubundaki veya tüm abonelikteki tüm VM'leri listelemek ve ardından istemci tarafında gerekli VM'yi seçmek yerine doğrudan VM sorgulayın. 
- İstemci kodunun belirli bir Azure konumundan VM'lere, disklere ve anlık görüntülere ihtiyacı varsa, tüm abonelik VM'lerini `GET /subscriptions/<subId>/providers/Microsoft.Compute/locations/<location>/virtualMachines?api-version=2017-03-30` sorgulamak ve ardından istemci tarafındaki konuma göre filtreleme yapmak yerine sorgunun konum tabanlı formunu kullanın: Kaynak Sağlayıcısı'nı bilgi işlem sağlayıcıya sorgulayın. 
-   Özellikle API kaynaklarını oluştururken veya güncellerken, VM'ler ve sanal makine ölçeği kümeleri, döndürülen async işlemini tamamlanmak `provisioningState`için kaynak URL'sinin kendisiüzerinde yoklama yapmaktan çok daha verimlidir (temel alınarak).

## <a name="next-steps"></a>Sonraki adımlar

Azure'daki diğer hizmetler için yeniden deneme kılavuzu hakkında daha fazla bilgi [için, belirli hizmetler için Yeniden Deneme kılavuzuna](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific) bakın
