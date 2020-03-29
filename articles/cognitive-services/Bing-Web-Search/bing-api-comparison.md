---
title: Bing Arama API'leri nedir?
titleSuffix: Azure Cognitive Services
description: Bing Arama API'leri ve uygulamalarınızda ve hizmetlerinizde bilişsel internet aramalarını nasıl etkinleştirebileceğiniz hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 82b1f13562a49284059c25bcbd39a33daf949dcc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74775549"
---
# <a name="what-are-the-bing-search-apis"></a>Bing Arama API'leri nedir?

Bing Arama API'leri, web sayfalarını, resimleri, haberleri, konumları ve daha fazlasını reklamlar olmadan bulan web bağlantılı uygulamalar ve hizmetler oluşturmanıza olanak sağlar. Bing Search REST API'lerini veya SDK'ları kullanarak arama istekleri göndererek, web aramaları için alakalı bilgi ve içerik alabilirsiniz. Farklı Bing arama API'leri ve bilişsel aramaları uygulamalarınız ve hizmetleriniz ile nasıl entegre edebileceğiniz hakkında bilgi edinmek için bu makaleyi kullanın. Fiyatlandırma ve fiyat sınırları API'ler arasında farklılık gösterebilir.

## <a name="the-bing-web-search-api"></a>Bing Web Arama API'si

[Bing Web Arama API'sı](../Bing-Web-Search/overview.md) web sayfalarını, görüntüleri, görüntüleri, videoyu, haberleri ve daha fazlasını döndürür. Belirli içerik türlerini eklemek veya hariç tutmak için bu API'ye gönderilen arama sorgularını filtreleyebilirsiniz.

Bing Web Arama API'sini, ilgili web içeriğinin her türlüsünün aranması gerekebilecek uygulamalarda kullanmayı düşünün. Uygulamanız belirli bir çevrimiçi içerik türünü arıyorsa, aşağıdaki arama API'lerinden birini göz önünde bulundurun:

## <a name="content-specific-bing-search-apis"></a>İçeriğe özgü Bing arama API'leri

Aşağıdaki Bing arama API'leri, resimler, haberler, yerel işletmeler ve videolar gibi web'den belirli içeriği döndürer.

| Bing API | Açıklama |
| -- | -- |
| [Varlık Arama](../Bing-Entities-Search/overview.md) | Bing Varlık Arama API'si, kişiler, yerler veya başka şeyler olabilecek varlıkları içeren arama sonuçlarını döndürür. Sorguya bağlı olarak, API arama sorgusunu karşılayan bir veya daha fazla varlık döndürecek. Arama sorgusu kayda değer kişileri, yerel işletmeleri, yer işaretlerini, hedefleri ve daha fazlasını içerebilir. |
| [Resim Arama](../Bing-Image-Search/overview.md) | Bing Resim Arama API'si, [Bing.com/images](https://www.Bing.com/images)benzer yüksek kaliteli statik ve animasyonlu görüntüleri aramanızı ve bulmanızı sağlar. Aramaları, boyut, renk, lisans ve tazelik gibi öznitelik olarak görüntüleri içerecek veya hariç tutmak üzere hassaslaştırabilirsiniz. Ayrıca popüler görüntüleri arayabilir, bunlar hakkında bilgi edinmek için resim yükleyebilir ve küçük resim önizlemelerini görüntüleyebilirsiniz. |
| [Haber Arama](../Bing-News-Search/search-the-web.md) | Bing Haberler Arama [API'si, Bing.com/news](https://www.Bing.com/news)benzer haberleri bulmanızı sağlar. API, haber makalelerini birden çok kaynaktan veya belirli etki alanlarından döndürür. Popüler makaleler, en iyi haberler ve başlıklar almak için kategoriler arasında arama yapabilirsiniz. |
| [Video Arama](../Bing-Video-Search/overview.md) | Bing Video Arama API'sı, web'deki videoları bulmanızı sağlar. Popüler videolar, ilgili içerik ve küçük resim önizlemeleri alın. |
| [Görsel Arama](../Bing-visual-search/overview.md) | Görsel olarak benzer ürünler, resimler ve ilgili aramalar gibi bu resim hakkında anlayışlı bilgiler edinmek için bir resim yükleyin veya URL kullanın. |
 [Yerel İş Arama](../bing-local-business-search/overview.md) | Bing Yerel İş Arama API'si, uygulamalarınızın arama sorgularına dayalı olarak yerel işletmeler le ilgili kişi ve konum bilgilerini bulmasına olanak tanır. |

## <a name="the-bing-custom-search-api"></a>Bing Özel Arama API'si

[Bing Özel Arama](../Bing-Custom-Search/overview.md) API'si ile özel bir arama örneği oluşturmak, yalnızca önem verdiğiniz içeriğe ve konulara odaklanmış bir arama deneyimi oluşturmanıza olanak tanır. Örneğin, Bing'in arayacağı etki alanlarını, web sitelerini ve belirli web sayfalarını belirttikten sonra Bing Özel Arama, sonuçları belirli içeriğe göre uyarlar. Arama deneyiminizi daha da özelleştirmek için Bing Özel Otomatik Önerme, Resim ve Video Arama API'lerini birleştirebilirsiniz.

## <a name="additional-bing-search-apis"></a>Ek Bing Arama API'leri

Aşağıdaki Bing Arama API'leri, bunları diğer Bing arama API'leriyle birleştirerek arama deneyiminizi geliştirmenize izin verebiliyor.

| API | Açıklama |
| -- | -- |
| [Bing Otomatik Öneri](../Bing-Autosuggest/get-suggested-search-terms.md) | Önerilen aramaları gerçek zamanlı olarak döndürerek Bing Otomatik Öner API'si ile uygulamanızın arama deneyimini geliştirin.  |
| [Bing İstatistikleri](bing-web-stats.md) | Bing İstatistikleri, uygulamanızın kullandığı Bing Arama API'leri için analiz sağlar. Kullanılabilir analizlerden bazıları çağrı hacmi, üst sorgu dizeleri ve coğrafi dağılım içerir. |

## <a name="next-steps"></a>Sonraki adımlar

* Bing Arama API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
