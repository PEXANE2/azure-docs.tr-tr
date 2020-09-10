---
title: Metin Analizi API'si-Azure bilişsel hizmetler ile metin madenciliği ve Analizi
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'si ile metin madenciliği hakkında bilgi edinin. Bu uygulamayı, yaklaşım analizi, dil algılama ve diğer doğal dil Işleme biçimleri için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 09/09/2020
ms.author: aahi
keywords: metin araştırma, yaklaşım analizi, metin analizi
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 544de4adb1891c3d558a524466a076daefb42aa4
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89647458"
---
# <a name="what-is-the-text-analytics-api"></a>Metin Analizi API'si nedir?

Metin Analizi API'si, metin madenciliği ve metin analizi için doğal dil Işleme (NLP) özellikleri sağlayan bulut tabanlı bir hizmettir: yaklaşım analizi, bakım madenciliği, anahtar tümceciği ayıklama, dil algılama ve adlandırılmış varlık tanıma.

API, geliştirme projeleriniz için bulutta makine öğrenimi ve AI algoritmaları koleksiyonu olan Azure bilişsel [Hizmetler](https://docs.microsoft.com/azure/cognitive-services/)'in bir parçasıdır. Bu özellikleri [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/)veya [istemci kitaplığıyla](quickstarts/text-analytics-sdk.md)birlikte kullanabilirsiniz.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Yaklaşım [analizini](how-tos/text-analytics-how-to-sentiment-analysis.md) kullanın ve olumlu veya olumsuz yaklaşım hakkında ipuçları için metni inceleyerek markanızı veya konuyu düşündüğünü öğrenin. Bu API özelliği her belge için 0 ile 1 arasında bir yaklaşım puanı, 1 ise en pozitif bir değer döndürür.

V 3.1 önizlemeden başlayarak, görüşme madenciliği bir Yaklaşım Analizi özelliğidir. Doğal dil Işlemede (NLP) en boy tabanlı Yaklaşım Analizi olarak da bilinen bu özellik, metinle ilgili, özelliklerle (ürünlerin veya hizmetlerin öznitelikleri gibi) ilgili daha ayrıntılı bilgiler sağlar.

## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

Metindeki ana kavramları hızlıca tanımlamak için [anahtar tümceciği ayıklama](how-tos/text-analytics-how-to-keyword-extraction.md) kullanın. Örneğin, "gıda merak ediyor ve harika personel vardı" metninde Anahtar İfade Ayıklama ana konuşmanoktaları döndürür: "yiyecek" ve "harika personel".

## <a name="language-detection"></a>Dil algılama

Dil algılama, bir [giriş metninin yazıldığı dili algılayabilir](how-tos/text-analytics-how-to-language-detection.md) ve çok çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerinde istek üzerine gönderilen her belge için tek bir dil kodu rapor edebilir. Dil kodu bir güvenirlik puanı ile eşleştirilmiş.

## <a name="named-entity-recognition"></a>Adlandırılmış varlık tanıma

Adlandırılmış varlık tanıma (NER), kişiler, konumlar, kuruluşlar, miktarlar ve Iyi bilinen varlıkların de tanınabilmesi ve Web üzerinde daha fazla bilgi ile bağlanması halinde, metinlerinizde [varlıkları tanımlayabilir ve kategorilere ayırabilirsiniz](how-tos/text-analytics-how-to-entity-linking.md) .

## <a name="use-containers"></a>Kapsayıcıları kullanma

[Metin analizi kapsayıcıları,](how-tos/text-analytics-how-to-install-containers.md) araştırma METNI ve API 'yi kullanarak bir şirket içi çözüm olarak kullanın. Bu Docker kapsayıcıları, önemli ifadeleri ayıklamanızı, dili tespit etmeyi ve verilerinize yakın yaklaşımı analiz etmenize olanak tanır.

## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir. Analiz edilecek verileri gönderir ve çıktıları kodunuzda işlersiniz. Çözümleyiciler olduğu gibi kullanılır, ek yapılandırma veya özelleştirme gerçekleştirilmez.

1. Metin Analizi için [bir Azure kaynağı oluşturun](../cognitive-services-apis-create-account.md) . Daha sonra, isteklerinizin kimlik doğrulaması için sizin için oluşturulan [anahtarı alın](../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) .

2. JSON biçiminde verilerinizi yapılandırılmamış ham metin olarak içeren [bir istek düzenleyin](how-tos/text-analytics-how-to-call-api.md#json-schema).

3. Kayıt sırasında, istenen kaynak: yaklaşım analizi, anahtar ifade ayıklama, dil algılama veya adlandırılmış varlık tanıma gibi bir istek noktasına isteği gönderin.

4. Yanıtın akışını yapın veya yerel ortamda depolayın. Gönderilen isteğe bağlı olarak sonuçlar yaklaşım puanı, ayıklanan anahtar ifadelerden oluşan bir koleksiyon veya dil kodu olacaktır.

Çıktı tek bir JSON belgesi olarak döndürülür ve kimlikle birlikte gönderdiğiniz tüm metin belgelerinin sonucunu içerir. Sonuçları daha sonra analiz ederek, görselleştirerek veya kategorilere ayırarak eyleme dönüştürülebilir içgörüler elde edebilirsiniz.

Veriler hesabınızda depolanmaz. Metin Analizi API'si tarafından gerçekleştirilen işlemlerde durum bilgisi yoktur. Başka bir deyişle sağladığınız metin işlenir ve sonuçlar anında döndürülür.

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>Birden çok programlama deneyimi düzeyi için Metin Analizi

Programlamada çok fazla deneyim olmasa bile, işlemlerinizde Metin Analizi API'si kullanmaya başlayabilirsiniz. API 'yi, deneyim düzeyinize uygun yollarla farklı şekillerde çözümlemek için nasıl kullanabileceğinizi öğrenmek için bu öğreticileri kullanın. 

* Gereken minimum programlama:
    * [Metin Analizi ve güç otomatikleştirme kullanarak Excel 'de bilgi Ayıkla](tutorials/extract-excel-information.md)
    * [Bir Yammer grubundaki yorumların yaklaşımını belirlemek için Metin Analizi API'si ve MS akışını kullanın](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Müşteri geri bildirimini çözümlemek için Power BI Metin Analizi API'si tümleştirin](tutorials/tutorial-power-bi-key-phrases.md)
* Programlama deneyimi önerilir:
    * [Azure Databricks kullanarak akış verileri üzerinde yaklaşım analizi](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Metin çevirmek, yaklaşımı çözümlemek ve konuşmayı sentezleştirmek için bir Flask uygulaması oluşturun](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>Desteklenen diller

Bu bölüm, daha kolay bulunmasını sağlama amacıyla başka bir makaleye taşınmıştır. Bu içerik için [Metin Analizi API'si desteklenen diller](text-analytics-supported-languages.md) bölümüne bakın.

<a name="data-limits"></a>

## <a name="data-limits"></a>Veri sınırları

Tüm Metin Analizi API'si uç noktaları ham metin verisi kabul eder. Daha fazla bilgi için [veri sınırları](concepts/data-limits.md) makalesine bakın.

## <a name="unicode-encoding"></a>Unicode kodlama

Metin Analizi API'si, metin gösterimi ve karakter sayısı hesaplamaları için Unicode kodlamasını kullanır. İstekler UTF-8 ve UTF-16 olarak gönderilebilir, karakter sayısında fark olmayacaktır. Karakter uzunluğu için Unicode kod noktaları buluşsal değer olarak kullanılır ve metin analizi veri sınırları için eşdeğer kabul edilir. [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements)Karakter sayısını almak için kullanırsanız, veri boyutunu ölçmek için kullandığımız yöntemi de kullanıyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

+ Uygulamalarınız için bir anahtar ve uç nokta almak üzere Metin Analizi için [bir Azure kaynağı oluşturun](../cognitive-services-apis-create-account.md) .

+ API çağrıları göndermeye başlamak için [hızlı](quickstarts/text-analytics-sdk.md) başlangıcı kullanın. Minimum kodla metin göndermeyi, analiz seçmeyi ve sonuçları görüntülemeyi öğrenin.

+ Yeni yayınlar ve özellikler hakkında bilgi edinmek için [Metin Analizi API'si](whats-new.md) yenilikleri inceleyin.

+ Azure Databricks kullanarak bu yaklaşım [Analizi öğreticisini](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services) biraz daha ayrıntılı olarak inceleyin.

+ [Dış & topluluk içeriği sayfamızda](text-analytics-resource-external-community.md)diğer araç ve teknolojilerle Metin Analizi API'si nasıl kullanılacağına ilişkin blog gönderileri ve daha fazla video listesini inceleyin.
