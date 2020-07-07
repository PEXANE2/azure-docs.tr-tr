---
title: Görüntü İşleme nedir? -Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme hizmeti geliştiricilerin görüntü işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 3e1c67ee91298b9e8d0c3c427988c9966771aeaa
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920566"
---
# <a name="what-is-computer-vision"></a>Görüntü İşleme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Görüntü İşleme Hizmeti, geliştiricilere resimleri işleyen ve ilgilendiğiniz görsel özelliklere göre bilgi döndüren gelişmiş algoritmalara erişim sağlar. Örneğin, Görüntü İşleme bir görüntünün yetişkinlere yönelik içerik içerip içermediğini, belirli marka veya nesne bulmasını veya insan yüzlerini bulmanızı sağlayabilir.

Uygulamanızdaki Görüntü İşleme, bir istemci kitaplığı SDK 'Sı aracılığıyla veya REST API doğrudan çağırarak kullanabilirsiniz. Bu sayfa, Görüntü İşleme ile yapabileceklerinizi büyük ölçüde ele alır.

## <a name="computer-vision-for-digital-asset-management"></a>Dijital varlık yönetimi için Görüntü İşleme

Görüntü İşleme, birçok dijital varlık yönetimi (DAM) senaryosunu güçlendirin. DAM, zengin medya varlıklarını düzenleme, depolama ve alma ve dijital hakları ve izinleri yönetme iş sürecleridir. Örneğin, bir şirket görünür logo, yüz, nesne, renk vb. temel alarak görüntüleri gruplamak ve tanımlamak isteyebilir. Ya da [görüntüler için otomatik olarak açıklamalı alt yazılar oluşturmak](./Tutorials/storage-lab-tutorial.md) ve anahtar sözcükler eklemek isteyebilirsiniz. Bilişsel hizmetler, Azure Bilişsel Arama ve akıllı Raporlama kullanan hepsi bir arada DAM çözümü için GitHub 'da [bilgi araştırma Çözüm Hızlandırıcısı kılavuzuna](https://github.com/Azure-Samples/azure-search-knowledge-mining) bakın. Diğer DAM örnekleri için [görüntü işleme çözüm şablonları](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates) deposuna bakın.

## <a name="analyze-images-for-insight"></a>İçgörü edinmek için görüntüleri analiz etme

Görsel özellikleri ve özellikleri hakkında Öngörüler sağlamak için görüntüleri çözümleyebilirsiniz. Aşağıdaki tabloda bulunan tüm özellikler, [görüntüyü çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'si tarafından sağlanır.

| Eylem | Description |
| ------ | ----------- |
|**[Görsel özellikleri etiketleme](concept-tagging-images.md)**|Görüntüde görsel özellikleri, binlerce tanınabilir nesne, çok sayıda, manzara ve eylem kümesinden belirleyin ve etiketleyin. Etiketler belirsiz veya ortak olmayan bir bilgi olmadığında, API yanıtı etiketin bağlamını açıklığa kavuşturmak için ipuçları sağlar. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzer öğeleri de kapsar.|
|**[Nesneleri Algıla](concept-object-detection.md)**| Nesne algılama, etiketlemeyle benzerdir ancak API, uygulanan her bir etiket için sınırlayıcı kutu koordinatlarını döndürür. Örneğin, bir görüntü köpek, kedi ve kişi içeriyorsa, Algıla işlemi bu nesneleri görüntüdeki koordinatlarıyla birlikte listeler. Görüntüdeki nesneler arasında daha fazla ilişki işlemek için bu işlevi kullanabilirsiniz. Ayrıca, görüntüde aynı etiketin birden çok örneği olduğunu da bilmenizi sağlar.|
|**[Markalar Algıla](concept-brand-detection.md)**|Binlerce küresel logo veritabanındaki resimlerde veya videolarda ticari markalar belirler. Bu özelliği örneğin, sosyal medya veya en popüler medya ürün yerleştirmesi üzerinde en popüler olan markalar öğrenmek için kullanabilirsiniz.|
|**[Bir görüntüyü kategorilere ayırma](concept-categorizing-images.md)**|Üst/alt öğe kalıtım hiyerarşileri içeren bir [kategori sınıflandırması](Category-Taxonomy.md) kullanarak bir görüntüyü bütünüyle tanımlayın ve kategorilere ayırın. Kategoriler tek başına veya yeni etiketleme modellerimizle birlikte kullanılabilir.<br/>Şu anda, görüntülerin etiketlenmesi ve kategorilere ayrılması için yalnızca İngilizce desteklenmektedir.|
|**[Bir görüntüyü açıklama](concept-describing-images.md)**|Tam cümleler kullanarak bir görüntünün tamamı için okunabilir açıklamalar oluşturun. Görüntü İşleme algoritmaları, görüntüde tanımlanan nesneleri temel alan çeşitli açıklamalar oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.|
|**[Yüz algılama](concept-detecting-faces.md)** |Bir görüntüdeki yüzleri algılayın ve algılanan her bir yüz için bilgiler sunun. Görüntü İşleme algılanan her bir yüz için koordinatları, dikdörtgeni, cinsiyeti ve yaşı döndürür.<br/>Görüntü İşleme [yüz](/azure/cognitive-services/face/) hizmeti işlevlerinin bir alt kümesini sağlar. Yüz tanıma ve poz algılama gibi daha ayrıntılı analizler için yüz hizmetini kullanabilirsiniz.|
|**[Görüntü türünü algılama](concept-detecting-image-types.md)**|Bir görüntü ile ilgili özellikleri (görüntünün çizim olup olmaması veya küçük resim olup olmama olasılığı gibi) algılayın.|
|**[Etki alanına özgü içeriği algılama](concept-detecting-domain-content.md)**|Bir görüntüde yer alan, ünlüler ve önemli yerler gibi etki alanına özgü içerikleri algılamak ve tanımak için etki alanı modelleri kullanın. Örneğin, bir görüntü kişi içeriyorsa Görüntü İşleme, görüntüde algılanan kişilerin bilinen ünlüler olup olmadığını tespit etmek üzere Ünlüler için bir etki alanı modeli kullanabilir.|
|**[Renk düzenini algılama](concept-detecting-color-schemes.md)**|Bir görüntüdeki renk kullanımını analiz edin. Görüntü İşleme, bir görüntünün siyah-beyaz olup olmadığını belirlemenin yanı sıra renkli görüntüler için baskın renkleri ve vurgu renklerini tanıyabilir.|
|**[Küçük resim oluşturma](concept-generating-thumbnails.md)**|Görüntülere uygun küçük resimler oluşturmak üzere söz konusu görüntülerin içeriklerini analiz edin. Görüntü İşleme ilk olarak yüksek kaliteli bir küçük resim oluşturur ve sonra *ilgilendiğiniz alanı*öğrenmek için görüntü içindeki nesneleri analiz eder. Görüntü İşleme sonra görüntüyü, ilgilendiğiniz alanın gereksinimlerine uyacak şekilde kırpar. Oluşturulan küçük resim, ihtiyaçlarınız doğrultusunda, özgün görüntünün en boy oranından farklı bir en boy oranı kullanılarak sunulabilir.|
|**[İlgilendiğiniz alanı alın](concept-generating-thumbnails.md#area-of-interest)**|*İlgilendiğiniz alanın*koordinatlarını döndürmek için bir görüntünün içeriğini analiz edin. Görüntüyü kırpmak ve küçük resim oluşturmak yerine, Görüntü İşleme bölgenin sınırlayıcı kutusu koordinatlarını döndürür, böylece çağıran uygulama özgün görüntüyü istediğiniz gibi değiştirebilir.|

## <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)

Görüntü İşleme, [optik karakter tanıma (OCR)](concept-recognizing-text.md) özelliklerini içerir. Görüntü ve belgelerden yazdırılmış ve el yazısı metinleri ayıklamak için yeni okuma API 'sini kullanabilirsiniz. En son modelleri kullanır ve çeşitli yüzeyler ve arka planlar üzerinde metinle birlikte kullanılır. Bu ınlü alındıları, posterleri, iş kartları, mektuplar ve beyaz tahtalar. İki OCR API 'si, yazdırılmış metnin [birkaç dilde](./language-support.md)ayıklanarak desteklenir.

## <a name="moderate-content-in-images"></a>Görüntü içeriklerini denetleme

Görüntü İşleme kullanarak bir görüntüdeki [yetişkinlere yönelik içeriği algılayabilir](concept-detecting-adult-content.md) ve farklı sınıflandırmaların güvenilirlik puanlarını getirebilirsiniz. İçerik bayrak belirleme eşiği, tercihlerinize uyum sağlamak için bir kayan ölçekte ayarlanabilir.

## <a name="use-containers"></a>Kapsayıcıları kullanma

Verilerinize yaklaşarak standartlaştırılmış bir Docker kapsayıcısı yükleyerek, yazdırılmış ve el yazısı metinleri yerel olarak tanımak için [görüntü işleme kapsayıcıları kullanın](computer-vision-how-to-install-containers.md) .

## <a name="image-requirements"></a>Görüntü gereksinimleri

Görüntü İşleme, şu gereksinimleri karşılayan görüntüleri analiz edebilir:

- Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
- Görüntünün dosya boyutunun 4 megabayt (MB) değerini aşmaması gerekir
- Görüntünün boyutlarının 50 x 50 pikselden büyük olması gerekir
  - Okuma API 'SI için, resmin boyutları 50 x 50 ve 10000 x 10000 piksel arasında olmalıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, Görüntü İşleme hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine göre farkında olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Hızlı Başlangıç kılavuzunu izleyerek Görüntü İşleme kullanmaya başlayın:

- [Hızlı başlangıç: Görüntü İşleme .NET istemci kitaplığı](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Hızlı başlangıç: Python istemci kitaplığı Görüntü İşleme](./quickstarts-sdk/client-library.md?pivots=programming-language-python)
- [Hızlı başlangıç: Java istemci kitaplığı Görüntü İşleme](./quickstarts-sdk/client-library.md?pivots=programming-language-java)
