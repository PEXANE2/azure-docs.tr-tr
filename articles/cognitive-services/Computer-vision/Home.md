---
title: Görüntü İşleme nedir? - BilgisayarLı Görme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme hizmeti geliştiricilerin görüntü işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 01/27/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: af49e80766d7ac4bd2d9ed677a2571fb0a5a3189
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80131760"
---
# <a name="what-is-computer-vision"></a>Görüntü İşleme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure'un Computer Vision hizmeti, geliştiricilere ilgilendiğiniz görsel özelliklere bağlı olarak görüntüleri işleyen ve bilgileri döndüren gelişmiş algoritmalara erişim sağlar. Örneğin, Computer Vision bir görüntünün yetişkinlere uygun içerik içerip içermeden veya görüntüdeki tüm insan yüzlerini bulup bulmayabileceğini belirleyebilir.

Bilgisayarınızdaki Görme'yi yerel bir SDK aracılığıyla veya doğrudan REST API'sini çağırarak kullanabilirsiniz. Bu sayfa, Computer Vision ile yapabileceklerini genel olarak kapsar.

## <a name="computer-vision-for-digital-asset-management"></a>Dijital varlık yönetimi için Bilgisayar Vizyonu

Computer Vision birçok dijital varlık yönetimi (DAM) senaryosuna güç verebilir. DAM, zengin medya varlıklarının düzenlenmesi, depolanması ve alınması ve dijital hak ve izinlerin yönetilmesi iş sürecidir. Örneğin, bir şirket görüntüleri görünür logolara, yüzlere, nesnelere, renklere ve benzeri ne rendeme dayalı olarak gruplandırmak ve tanımlamak isteyebilir. Veya, resimler için otomatik olarak [altyazılar oluşturmak](./Tutorials/storage-lab-tutorial.md) ve aranabilir olacak şekilde anahtar kelimeler eklemek isteyebilirsiniz. Bilişsel Hizmetler, Azure Bilişsel Arama ve akıllı raporlama yı kullanan hepsi bir arada DAM çözümü için GitHub'daki [Bilgi Madenciliği Çözüm Hızlandırıcı Kılavuzu'na](https://github.com/Azure-Samples/azure-search-knowledge-mining) bakın. Diğer DAM örnekleri [için, BilgisayarLı Vizyon Çözüm Şablonları](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates) deposuna bakın.

## <a name="analyze-images-for-insight"></a>İçgörü edinmek için görüntüleri analiz etme

Görsel özellikleri ve özellikleri hakkında algılamak ve öngörüler sağlamak için görüntüleri analiz edebilirsiniz. Aşağıdaki tablodaki tüm özellikler [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API tarafından sağlanmaktadır.

| Eylem | Açıklama |
| ------ | ----------- |
|**[Görsel özellikleri etiketleme](concept-tagging-images.md)**|Binlerce tanınabilir nesne, canlı, sahne ve eylem kümesinden görüntüdeki görsel özellikleri tanımlayın ve etiketleyin. Etiketler belirsiz olduğunda veya yaygın bilgi olmadığında, API yanıtı etiketin bağlamını açıklığa kavuşturmak için ipuçları sağlar. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzer öğeleri de kapsar.|
|**[Nesneleri algılama](concept-object-detection.md)**| Nesne algılama etiketlemeye benzer, ancak API uygulanan her etiket için sınırlayıcı kutu koordinatlarını döndürür. Örneğin, bir görüntü bir köpek, kedi ve kişi içeriyorsa, Algıla işlemi bu nesneleri görüntüdeki koordinatlarıyla birlikte listeler. Görüntüdeki nesneler arasındaki ilişkileri daha fazla işlemek için bu işlevselliği kullanabilirsiniz. Ayrıca, görüntüde aynı etiketin birden çok örneği olduğunda da bilmenizi sağlar.|
|**[Markaları algılama](concept-brand-detection.md)**|Binlerce küresel logodan oluşan bir veritabanından resimlerdeki veya videolardaki ticari markaları tanımlayın. Bu özelliği, örneğin, hangi markaların sosyal medyada en popüler olduğunu veya medya ürün yerleştirmesinde en yaygın olduğunu keşfetmek için kullanabilirsiniz.|
|**[Bir görüntüyü kategorilere ayırma](concept-categorizing-images.md)**|Üst/alt öğe kalıtım hiyerarşileri içeren bir [kategori sınıflandırması](Category-Taxonomy.md) kullanarak bir görüntüyü bütünüyle tanımlayın ve kategorilere ayırın. Kategoriler tek başına veya yeni etiketleme modellerimizle birlikte kullanılabilir.<br/>Şu anda, görüntülerin etiketlenmesi ve kategorilere ayrılması için yalnızca İngilizce desteklenmektedir.|
|**[Bir görüntüyü açıklama](concept-describing-images.md)**|Tam cümleler kullanarak bir görüntünün tamamı için okunabilir açıklamalar oluşturun. Görüntü İşleme algoritmaları, görüntüde tanımlanan nesneleri temel alan çeşitli açıklamalar oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür.|
|**[Yüz algılama](concept-detecting-faces.md)** |Bir görüntüdeki yüzleri algılayın ve algılanan her bir yüz için bilgiler sunun. Görüntü İşleme algılanan her bir yüz için koordinatları, dikdörtgeni, cinsiyeti ve yaşı döndürür.<br/>Computer [Vision, Face](/azure/cognitive-services/face/) servis işlevinin bir alt kümesini sağlar. Yüz tanımlama ve poz algılama gibi daha ayrıntılı analizler için Yüz hizmetini kullanabilirsiniz.|
|**[Görüntü türünü algılama](concept-detecting-image-types.md)**|Bir görüntü ile ilgili özellikleri (görüntünün çizim olup olmaması veya küçük resim olup olmama olasılığı gibi) algılayın.|
|**[Etki alanına özgü içeriği algılama](concept-detecting-domain-content.md)**|Bir görüntüde yer alan, ünlüler ve önemli yerler gibi etki alanına özgü içerikleri algılamak ve tanımak için etki alanı modelleri kullanın. Örneğin, bir görüntü de kişiler içeriyorsa, Bilgisayar Görüşü, görüntüde algılanan kişilerin bilinen ünlüler olup olmadığını belirlemek için ünlüler için bir etki alanı modeli kullanabilir.|
|**[Renk düzenini algılama](concept-detecting-color-schemes.md)**|Bir görüntüdeki renk kullanımını analiz edin. Görüntü İşleme, bir görüntünün siyah-beyaz olup olmadığını belirlemenin yanı sıra renkli görüntüler için baskın renkleri ve vurgu renklerini tanıyabilir.|
|**[Küçük resim oluşturma](concept-generating-thumbnails.md)**|Görüntülere uygun küçük resimler oluşturmak üzere söz konusu görüntülerin içeriklerini analiz edin. Computer Vision önce yüksek kaliteli bir küçük resim oluşturur ve daha sonra *ilgi alanını*belirlemek için görüntü içindeki nesneleri analiz eder. Computer Vision daha sonra ilgi alanının gereksinimlerine uyacak şekilde görüntüyü ekinler. Oluşturulan küçük resim, ihtiyaçlarınız doğrultusunda, özgün görüntünün en boy oranından farklı bir en boy oranı kullanılarak sunulabilir.|
|**[İlgi alanını alın](concept-generating-thumbnails.md#area-of-interest)**|İlgi alanının koordinatlarını döndürmek için görüntünün içeriğini analiz *edin.* Bilgisayarı Görüntü kırpmak ve küçük resim oluşturmak yerine, Bilgisayar Görüşü bölgenin sınırlayıcı kutusu koordinatlarını döndürür, böylece arama uygulaması özgün görüntüyü istediğiniz gibi değiştirebilir.|

## <a name="extract-text-from-images"></a>Görüntülerdeki metinleri ayıklama

Resimlerden yazdırılan ve el yazısıyla yazılmış metinleri makine tarafından okunabilir bir karakter akışına aktarmak için Computer Vision [Read](concept-recognizing-text.md#read-api) API'yi kullanabilirsiniz. Read API en son modellerimizi kullanır ve makbuzlar, posterler, kartvizitler, mektuplar ve beyaz tahtalar gibi çeşitli yüzeylerde ve arka planlarda metinle çalışır. Şu anda, İngilizce ve İspanyolca desteklenen tek dillerdir.

Yazdırılan metni çeşitli dillerde ayıklamak için [optik karakter tanıma (OCR)](concept-recognizing-text.md#ocr-optical-character-recognition-api) API'sini de kullanabilirsiniz. Gerekirse, OCR tanınan metnin döndürmesini düzeltir ve her sözcüğün çerçeve koordinatlarını sağlar. OCR 25 dili destekler ve tanınan metnin dilini otomatik olarak algılar.

## <a name="moderate-content-in-images"></a>Görüntü içeriklerini denetleme

Bir görüntüdeki [yetişkinlere uygun içeriği algılamak](concept-detecting-adult-content.md) ve farklı sınıflandırmalar için güven puanlarını döndürmek için Computer Vision'ı kullanabilirsiniz. İçeriği işaretleme eşiği, tercihlerinize uygun olacak şekilde kayan bir ölçekte ayarlanabilir.

## <a name="use-containers"></a>Kapsayıcıları kullanma

Verilerinize daha yakın standart bir Docker kapsayıcısı yükleyerek yazdırılmış ve el yazısıyla yazılmış metni yerel olarak tanımak için [Computer Vision kapsayıcılarını kullanın.](computer-vision-how-to-install-containers.md)

## <a name="image-requirements"></a>Görüntü gereksinimleri

Görüntü İşleme, şu gereksinimleri karşılayan görüntüleri analiz edebilir:

- Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
- Görüntünün dosya boyutunun 4 megabayt (MB) değerini aşmaması gerekir
- Görüntünün boyutlarının 50 x 50 pikselden büyük olması gerekir
  - Read API için görüntünün boyutları 50 x 50 ile 10000 x 10000 piksel arasında olmalıdır.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm Bilişsel Hizmetlerde olduğu gibi, Computer Vision hizmetini kullanan geliştiriciler microsoft'un müşteri verilerine ilişkin ilkelerinden haberdar olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi'ndeki [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Hızlı başlangıç kılavuzunu izleyerek Computer Vision ile başlayın:

- [Quickstart: Computer Vision .NET istemci kitaplığı](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Quickstart: Computer Vision Python istemci kitaplığı](./quickstarts-sdk/client-library.md?pivots=programming-language-python)
- [Quickstart: Computer Vision Java istemci kitaplığı](./quickstarts-sdk/client-library.md?pivots=programming-language-java)
