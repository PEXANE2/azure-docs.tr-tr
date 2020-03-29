---
title: Medya İşleme genel bakış | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri ile Medya İşleme'yi ölçekleme konusuna genel bir bakıştır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/26/2019
ms.author: juliako
ms.openlocfilehash: 780d3ab5047bff321d0c554880ba2995bcf25524
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70102904"
---
# <a name="scaling-media-processing-overview"></a>Medya işlemeyi ölçeklendirmeye genel bakış 
Bu sayfa, medya işlemenin nasıl ve neden ölçeklendirilene ilgili genel bir bakış sağlar. 

## <a name="overview"></a>Genel Bakış
Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Aşağıdaki ayrılmış birim türleri arasından seçim yapabilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. Daha fazla bilgi için [Ayrılmış Birim Türleri'ne](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/)bakın.

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızı ayrılmış birimlerle birlikte sağlamanızı da belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, hesabınızda beş ayrılmış birim varsa, işlenecek görevler olduğu sürece beş ortam görevi aynı anda çalışır. Kalan görevler sırada bekler ve çalışan bir görev bittiğinde sırayla işlenmek için alınır. Bir hesapta ayrılmış herhangi bir birim yoksa, görevler sırayla alınır. Bu durumda, bir görevin tamamlanması ile bir sonraki başlangıç arasındaki bekleme süresi, sistemdeki kaynakların kullanılabilirliğine bağlıdır.

## <a name="choosing-between-different-reserved-unit-types"></a>Farklı ayrılmış birim türleri arasında seçim
Aşağıdaki tablo, farklı kodlama hızları arasında seçim yaparken bir karar vermenize yardımcı olur. Ayrıca, kendi testlerinizi gerçekleştirmek için [indirebileceğiniz bir videoda](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) birkaç kıyaslama durumda sağlar:

|RU tipi|Senaryo|7 dk [1080p video](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) için örnek sonuçlar|
|---|---|---|
| **S1**|Tek bithızı kodlama. <br/>SD veya çözünürlüklerin altında dosyalar, zamanduyarlı değil, düşük maliyetli.|"H264 Tek Bitrate SD 16x9" kullanarak tek bit hızı SD çözünürlüğü MP4 dosyası kodlama yaklaşık 7 dakika sürer.|
| **S2**|Tek bithızı ve birden çok bitrate kodlama.<br/>Hem SD hem de HD kodlama için normal kullanım.|"H264 Single Bitrate 720p" ön ayarlı kodlama yaklaşık 6 dakika sürer.<br/><br/>"H264 Çoklu Bitrate 720p" ön ayarlı kodlama yaklaşık 12 dakika sürer.|
| **S3**|Tek bithızı ve birden çok bitrate kodlama.<br/>Full HD ve 4K çözünürlüklü videolar. Zamana duyarlı, daha hızlı geri dönüş kodlaması.|"H264 Single Bitrate 1080p" ön ayarlı kodlama yaklaşık 3 dakika sürer.<br/><br/>"H264 Çoklu Bitrate 1080p" ön ayarlı kodlama yaklaşık 8 dakika sürer.|

## <a name="considerations"></a>Dikkat edilmesi gerekenler
> [!IMPORTANT]
> Bu bölümde açıklanan hususları gözden geçirin.  
> 
> 

* Media Services v3 veya Video Indexer tarafından tetiklenen Ses Analizi ve Video Analizi işleri için, S3 birim türü şiddetle tavsiye edilir.
* Paylaşılan havuzu kullanıyorsanız, yani ayrılmış birimler olmadan, kodlama görevleriniz S1 RUs ile aynı performansa sahiptir. Ancak, Görevlerinizin sıraya girerek harcayabileceği zamana bağlı bir üst sınır yoktur ve herhangi bir zamanda en fazla bir Görev çalışıyor olacaktır.

## <a name="billing"></a>Faturalandırma

Medya Ayrılmış Birimlerin hesabınızda verilen dakika sayısına bağlı olarak ücretlendirilirsiniz. Bu, hesabınızda çalışan herhangi bir İş olup olmadığından bağımsız olarak gerçekleşir. Ayrıntılı bir açıklama için Medya Hizmetleri [fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfasının SSS bölümüne bakın.   

## <a name="quotas-and-limitations"></a>Kotalar ve sınırlamalar
Kotalar ve sınırlamalar ve destek biletinin nasıl açılacağını hakkında bilgi için [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)bölümüne bakın.

## <a name="next-step"></a>Sonraki adım
Bu teknolojilerden biriyle ölçekleme ortam işleme görevini gerçekleştirin: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [Geri kalanı](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Java SDK'ın en son sürümünü almak ve Java ile geliştirmeye başlamak için bkz. [Media Services için Java istemcisi SDK’sı ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'sını indirmek üzere, [Packagist deposunda](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) Microsoft/WindowAzure paketinin 0.5.7 sürümünü arayın.  

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

