---
title: Azure Content Moderator nedir?
titleSuffix: Azure Cognitive Services
description: Kullanıcıların oluşturduğu içeriği izlemek, işaretlemek, değerlendirmek ve uygunsuz malzemeleri filtrelemek için Content Moderator kullanmayı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 570d55c8523e1c1deca3242a8841b0cc34322786
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80053884"
---
# <a name="what-is-azure-content-moderator"></a>Azure Content Moderator nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure İçerik Moderatörü, rahatsız edici, riskli veya başka bir şekilde istenmeyen materyaller için metin, resim ve video içeriğini kontrol eden bir bilişsel hizmettir. Bu malzeme bulunduğunda, hizmet içeriğe uygun etiketler (bayraklar) uygular. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir. Farklı içerik bayraklarının ne gösterdiği hakkında daha fazla bilgi edinmek için [Moderasyon API'leri](#moderation-apis) bölümüne bakın.

## <a name="where-its-used"></a>Kullanıldığı yer

Aşağıda bir yazılım geliştirme uzmanının veya ekibinin Content Moderator özelliklerinden faydalanmak isteyebileceği birkaç senaryo verilmiştir:

- Ürün kataloglarını ve kullanıcı tarafından oluşturulan diğer içeriği ılımlı yapan çevrimiçi pazar yerleri.
- Oyun şirketleri bu ılımlı kullanıcı tarafından oluşturulan oyun eserler ve sohbet odaları.
- Kullanıcıları tarafından eklenen görüntüleri, metinleri ve videoları gösteren sosyal mesajlaşma platformları.
- İçerikleri için merkezi ılımlılık uygulayan kurumsal medya şirketleri.
- K-12 eğitim çözüm sağlayıcıları, öğrenciler ve eğitimciler için uygun olmayan içerikleri filtreler.

> [!NOTE]
> Yasadışı çocuk istismarı görüntülerini algılamak için İçerik Moderatörü'ü kullanamazsınız. Ancak, nitelikli kuruluşlar bu tür içerikleri taramak için [PhotoDNA Bulut Hizmeti'ni](https://www.microsoft.com/photodna "Microsoft PhotoDNA Bulut Hizmeti") kullanabilir.

## <a name="what-it-includes"></a>Neleri içerir

Content Moderator hizmeti hem REST çağrıları hem de .NET SDK'sı ile sunulan çeşitli web hizmeti API'lerinden oluşur. Ayrıca hizmete yardımcı olmak veya denetleme işlevinde ince ayar yapmak için insanlar tarafından gerçekleştirilen inceleme hizmeti de sunar.

## <a name="moderation-apis"></a>Yönetim API’leri

İçerik Moderatörhizmeti, içeriği uygunsuz veya sakıncalı olabilecek materyalleriçin kontrol eden Moderasyon API'leri içerir.

![İçerik Moderatör ılımlılık API'leri için blok diyagramı](images/content-moderator-mod-api.png)

Aşağıdaki tabloda farklı ılımlılık API türleri açıklanmaktadır.

| API grubu | Açıklama |
| ------ | ----------- |
|[**Metin denetimi**](text-moderation-api.md)| Saldırgan içerik, müstehcen veya müstehcen içerik, küfür ve kişisel veriler için metni tarar.|
|[**Özel terim listeleri**](try-terms-list-api.md)| Metni yerleşik terimlerle birlikte özel bir terim listesine göre tarar. İçerik ilkelerinize göre içerik engellemek veya izin vermek için özel listeler kullanabilirsiniz.|  
|[**Görüntü denetimi**](image-moderation-api.md)| Görüntülerde yetişkinlere yönelik veya müstehcen görüntü taraması yapar, Optik Karakter Tanıyıcı (OCR) ile görüntüdeki metinleri tanır ve yüzleri algılar.|
|[**Özel görüntü listeleri**](try-image-list-api.md)| Görüntüleri özel görüntü listesine göre tarar. Özel görüntü listelerini kullanarak tekrar tekrar sınıflandırmak istemediğiniz yaygın içerik örneklerini filtreleyebilirsiniz.|
|[**Video moderasyonu**](video-moderation-api.md)| Videolarda yetişkinlere yönelik veya müstehcen içerik taraması yapar ve bu içeriklerle ilgili zaman işaretçilerini döndürür.|

## <a name="review-apis"></a>API’leri inceleme

İnceleme API'leri, ılımlılık ardışıklığınızı insan gözden geçirenlerle tümleştirmenize izin sağlar. [İnceleme aracıyla](#the-review-tool) (aşağıda) döngüdeki insan iş akışlarını oluşturmak ve otomatikleştirmek için [İşler,](review-api.md#jobs) [İncelemeler](review-api.md#reviews)ve [İş Akışı](review-api.md#workflows) işlemlerini kullanın.

> [!NOTE]
> İş Akışı API'si henüz .NET SDK'da mevcut değildir, ancak REST bitiş noktası yla kullanılabilir.

![İçerik Moderatör üe'leri için blok diyagramı](images/content-moderator-rev-api.png)

## <a name="the-review-tool"></a>İnceleme aracı

İçerik Moderatör hizmeti, insan moderatörlerin işlemesi için içerik incelemelerini barındıran web tabanlı [İnceleme aracını](Review-Tool-User-Guide/human-in-the-loop.md)da içerir. İnsan girişi hizmeti eğitmez, ancak hizmetin ve insan inceleme ekiplerinin birleştirilmiş çalışması, geliştiricilerin verimlilik ve doğruluk arasında doğru dengeyi sağlamasına olanak tanır. İnceleme aracı, çeşitli İçerik Moderatör kaynakları için kullanıcı dostu bir ön uç da sağlar.

![Content Moderator insan inceleme aracı giriş sayfası](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm Bilişsel Hizmetlerde olduğu gibi, İçerik Moderatörü hizmetini kullanan geliştiriciler microsoft'un müşteri verilerine ilişkin ilkelerinden haberdar olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi'ndeki [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Web'de İçerik Moderatörü'nden Deneyin'deki](quick-start.md)yönergeleri izleyerek İçerik Moderatörhizmetini kullanmaya başlayın.
