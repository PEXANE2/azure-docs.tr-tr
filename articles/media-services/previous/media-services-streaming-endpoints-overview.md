---
title: Azure Medya Hizmetleri Akış Bitiş Noktası genel bakışı | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri akış uç noktalarına genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 95d8d819aa1b418b4a7ec736cef64cb989f7e37b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74885645"
---
# <a name="streaming-endpoints-overview"></a>Akış uç noktalarına genel bakış  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Microsoft Azure Media Services'da (AMS) **Akış Bitiş Noktası,** içeriği doğrudan istemci oynatıcı uygulamasına veya daha fazla dağıtım için Bir İçerik Dağıtım Ağı'na (CDN) sunabilen bir akış hizmetini temsil eder. Medya Hizmetleri ayrıca sorunsuz Azure CDN tümleştirmesi sağlar. Bir StreamingEndpoint hizmetinden giden akış, canlı akış, isteğe bağlı bir video veya aracınızın Medya Hizmetleri hesabınıza aşamalı olarak indirilmesi olabilir. Her Azure Medya Hizmetleri hesabı varsayılan bir AkışSon Noktası içerir. Ek StreamingEndpoints hesap altında oluşturulabilir. StreamingEndpoints, 1.0 ve 2.0 olmak üzere iki sürümü vardır. 10 Ocak 2017'den itibaren, yeni oluşturulan TÜM AMS hesapları sürüm 2.0 **varsayılan** StreamingEndpoint'i içerecektir. Bu hesaba eklediğiniz ek akış uç noktaları da sürüm 2.0 olacaktır. Bu değişiklik varolan hesapları etkilemez; mevcut StreamingEndpoints sürüm 1.0 olacak ve sürüm 2.0 yükseltilebilir. Bu değişiklikle davranış, faturalandırma ve özellik değişiklikleri olacaktır (daha fazla bilgi için aşağıda belgelenen **Akış türleri ve sürümler** bölümüne bakın).

Azure Medya Hizmetleri, Akış Bitiş Noktası varlığına aşağıdaki özellikleri ekledi: **CdnProvider**, **CdnProfile**, **StreamingEndpointVersion**. Bu özelliklerin ayrıntılı genel bakışı için [bkz.](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint) 

Bir Azure Medya Hizmetleri hesabı oluşturduğunuzda, **Durduruldu** durumunda sizin için varsayılan standart akış bitiş noktası oluşturulur. Varsayılan akış bitiş noktasını silemezsiniz. Hedeflenen bölgedeki Azure CDN kullanılabilirliğine bağlı olarak, varsayılan olarak yeni oluşturulan varsayılan akış bitiş noktası "StandardVerizon" CDN sağlayıcı tümleştirmesini de içerir. 
                
> [!NOTE]
> Akış bitiş noktasını başlatmadan önce Azure CDN tümleştirmesi devre dışı bilebilir. CDN'yi `hostname` etkinleştirseniz de etkinleştirmeseniz de akış URL'si aynı kalır.

Bu konu, akış uç noktaları tarafından sağlanan ana işlevlerin genel bir özetini verir.

## <a name="naming-conventions"></a>Adlandırma kuralları

Varsayılan bitiş noktası için:`{AccountName}.streaming.mediaservices.windows.net`

Ek uç noktalar için:`{EndpointName}-{AccountName}.streaming.mediaservices.windows.net`

## <a name="streaming-types-and-versions"></a>Akış türleri ve sürümleri

### <a name="standardpremium-types-version-20"></a>Standart/Premium türleri (sürüm 2.0)

Media Services'In Ocak 2017 sürümünden başlayarak iki akış türü vardır: **Standart** (önizleme) ve **Premium.** Bu türler, Akış uç noktası sürümü "2.0" bir parçasıdır.


|Tür|Açıklama|
|--------|--------|  
|**Standart**|Varsayılan Akış Bitiş Noktası **Standart** türüdür, akış birimleri ayarlanarak Premium türüne değiştirilebilir.|
|**Premium** |Bu seçenek, daha yüksek ölçek veya denetim gerektiren profesyonel senaryolar için uygundur. Akış birimlerini ayarlayarak **Premium** türüne geçersiniz.<br/>Özel Akış Uç Noktaları yalıtılmış ortamda yaşar ve kaynaklar için rekabet etmez.|

Büyük internet hedef kitlelerine içerik sunmak isteyen müşteriler için, Akış Bitiş Noktası'nda CDN'yi etkinleştirmenizi öneririz.

Daha ayrıntılı bilgi için aşağıdaki [Akış türlerini karşılaştır](#comparing-streaming-types) bölümüne bakın.

### <a name="classic-type-version-10"></a>Klasik tip (sürüm 1.0)

10 Ocak 2017 sürümünden önce AMS hesaplarını oluşturan kullanıcılar **için, Klasik** bir akış bitiş noktası türüne sahipsiniz. Bu tür, akış uç noktası sürümü "1.0" bir parçasıdır.

Sürüm **"1.0"** akış bitiş noktası >=1 premium akış birimleri (SU) varsa, bu premium akış bitiş noktası olacaktır ve ek yapılandırma adımları olmadan tüm AMS özelliklerini **(Standart/Premium** türü gibi) sağlayacaktır.

>[!NOTE]
>**Klasik** akış uç noktaları (sürüm "1.0" ve 0 SU), sınırlı özellikler sağlar ve SLA içermez. Daha iyi bir deneyim elde etmek ve dinamik paketleme veya şifreleme gibi özellikleri ve **Standart** türüyle birlikte gelen diğer özellikleri kullanmak için **Standart** türe geçiş önerilir. **Standart** türüne geçiş yapmak için [Azure portalına](https://portal.azure.com/) gidin ve **Standart'a Kabul Et'i**seçin. Geçiş hakkında daha fazla bilgi için [geçiş](#migration-between-types) bölümüne bakın.
>
>Bu işlemin geri alınamayacağını ve fiyatlandırma etkisi olduğunu sakının.
>
 
## <a name="comparing-streaming-types"></a>Akış türlerini karşılaştırma

### <a name="versions"></a>Sürümler

|Tür|StreamingEndpointVersion|Ölçek Birimleri|CDN|Faturalandırma|
|--------------|----------|-----------------|-----------------|-----------------|
|Klasik|1.0|0|NA|Ücretsiz|
|Standart Akış Bitiş Noktası (önizleme)|2,0|0|Evet|Ödenen|
|Premium Akış Birimleri|1.0|>0|Evet|Ödenen|
|Premium Akış Birimleri|2,0|>0|Evet|Ödenen|

### <a name="features"></a>Özellikler

Özellik|Standart|Premium
---|---|---
Aktarım hızı |600 Mbps'ye kadar ve CDN kullanıldığında çok daha yüksek etkili bir iş çıkarımı sağlayabilir.|Akış birimi (SU) başına 200 Mbps. CDN kullanıldığında çok daha yüksek etkili bir iş elde edilebilir.
CDN|Azure CDN, üçüncü taraf CDN veya CDN yok.|Azure CDN, üçüncü taraf CDN veya CDN yok.
Faturalandırma eşit olarak eşit| Günlük|Günlük
Dinamik şifreleme|Evet|Evet
Dinamik paketleme|Evet|Evet
Ölçek|Otomatik ölçeklendirme hedeflenen iş sonuna kadar.|Ek akış birimleri.
IP filtreleme/G20/Özel ana bilgisayar <sup>1</sup>|Evet|Evet
Aşamalı indirme|Evet|Evet
Önerilen kullanım |Akış senaryolarının büyük çoğunluğu için önerilir.|Profesyonel kullanım. 

<sup>1</sup> Yalnızca CDN bitiş noktasında etkinleştirilmediğinde Doğrudan Akış Bitiş Noktası'nda kullanılır.<br/>

SLA bilgileri için [Fiyatlandırma ve SLA'ya](https://azure.microsoft.com/pricing/details/media-services/)bakın.

## <a name="migration-between-types"></a>Türler arasında geçiş

Başlangıç | Alıcı | Eylem
---|---|---
Klasik|Standart|Kabul etme ihtiyacı
Klasik|Premium| Ölçek (ek akış birimleri)
Standart / Premium|Klasik|Kullanılamıyor (Akış uç noktası sürümü 1.0 ise. Ölçek birimleri "0" olarak ayarlanarak klasik olarak değiştirilebilir)
Standart (CDN'li/CDN'siz)|Aynı yapılandırmalara sahip premium|**Başlangıç** durumunda izin verilir. (Azure portalı üzerinden)
Premium (CDN'li/CDN'siz)|Aynı yapılandırmalara sahip standart|**Başlangıç** durumunda izin verilir (Azure portalı üzerinden)
Standart (CDN'li/CDN'siz)|Farklı config ile Premium|**Durduruldu** durumda (Azure portalı üzerinden) izin verilir. Aday durumda izin verilmiyor.
Premium (CDN'li/CDN'siz)|Farklı config ile standart|**Durduruldu** durumda (Azure portalı üzerinden) izin verilir. Aday durumda izin verilmiyor.
SÜRÜM 1.0 SU >= 1 CDN ile|CDN'siz standart/Premium|**Durdurulan** durumda izin verilir. **Başlangıç** durumunda izin verilmiyor.
SÜRÜM 1.0 SU >= 1 CDN ile|CDN ile/CDN'siz standart|**Durdurulan** durumda izin verilir. **Başlangıç** durumunda izin verilmiyor. Sürüm 1.0 CDN silinir ve yeni bir oluşturulan ve başlatılır.
SÜRÜM 1.0 SU >= 1 CDN ile|CDN ile/CDN'siz premium|**Durdurulan** durumda izin verilir. **Başlangıç** durumunda izin verilmiyor. Klasik CDN silinir ve yeni bir oluşturulan ve başlatılır.

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

