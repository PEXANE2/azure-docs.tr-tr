---
title: Metin Analizi API'si nedir? - Yetenekler -
titleSuffix: Azure Cognitive Services
description: Duygu analizi, anahtar ifade çıkarma, dil algılama ve varlık tanıma için Azure Bilişsel Hizmetleri'ndeki Metin Analizi API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 03/04/2020
ms.author: aahi
ms.openlocfilehash: a9519be591581fa434825f1a1fb31749788a21a8
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78395740"
---
# <a name="what-is-the-text-analytics-api"></a>Metin Analizi API'si nedir?

Text Analytics API, ham metin üzerinde gelişmiş doğal dil işleme sağlayan bulut tabanlı bir hizmettir ve dört ana işlevi içerir: duygu analizi, anahtar sözcük çıkarma, dil algılama ve varlık tanıma adı.

API, geliştirme projeleriniz için bulutta makine öğrenimi ve Yapay Algı algoritmaları koleksiyonu olan [Azure Bilişsel Hizmetler'in](https://docs.microsoft.com/azure/cognitive-services/)bir parçasıdır.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

Metin çözümlemesi farklı anlama gelebilir, ancak Bilişsel Hizmetler'de Metin Analizi API'sı aşağıda açıklandığı gibi dört tür analiz sağlar. Bu özellikleri [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/)veya istemci [kitaplığı](quickstarts/text-analytics-sdk.md)ile kullanabilirsiniz.

## <a name="sentiment-analysis"></a>Yaklaşım Analizi
Olumlu veya olumsuz duygular hakkında ipuçları için ham metni analiz ederek müşterilerin markanız veya konunuz hakkında ne düşündüğünü öğrenmek için [duyarlılık analizini](how-tos/text-analytics-how-to-sentiment-analysis.md) kullanın. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir.<br /> Analiz modelleri, Microsoft tarafından sağlanan geniş kapsamlı gövde metinleri ve doğal dil teknolojileri kullanılarak önceden eğitilmiştir. API, [seçili dillerde](text-analytics-supported-languages.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir.

## <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama
Ana noktaları hızlı bir şekilde tanımlamak için [anahtar tümcecikleri](how-tos/text-analytics-how-to-keyword-extraction.md) otomatik olarak ayıklayın. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür.

## <a name="language-detection"></a>Dil Algılama
Giriş [metninin hangi dilde yazıldığını algılayabilir](how-tos/text-analytics-how-to-language-detection.md) ve istek üzerine gönderilen her belge için çok çeşitli dillerde, varyantlarda, lehçelerde ve bazı bölgesel/kültürel dillerde tek bir dil kodu bildirebilirsiniz. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir.

## <a name="named-entity-recognition"></a>Adlandırılmış Varlık Tanıma
Metninizdeki varlıkları kişi, yer, kuruluş, tarih/saat, miktarlar, yüzdeler, para birimleri ve daha fazlası olarak tanımlayın ve [kategorilere ayırın.](how-tos/text-analytics-how-to-entity-linking.md) İyi bilinen varlıklar da tanınarak web üzerindeki ek bilgilere bağlantı verilir.

## <a name="use-containers"></a>Kapsayıcıları kullanma

Verilerinize daha yakın standart Docker kapsayıcıları yükleyerek, anahtar ifadeleri ayıklamak, dili algılamak ve duyguları yerel olarak analiz etmek için [Metin Analizi kapsayıcılarını kullanın.](how-tos/text-analytics-how-to-install-containers.md)

## <a name="typical-workflow"></a>Tipik iş akışı

İş akışı basittir. Analiz edilecek verileri gönderir ve çıktıları kodunuzda işlersiniz. Çözümleyiciler olduğu gibi kullanılır, ek yapılandırma veya özelleştirme gerçekleştirilmez.

1. Metin Analizi için [bir Azure kaynağı oluşturun.](../cognitive-services-apis-create-account.md) Daha sonra, isteklerinizi doğrulamak için oluşturulan [anahtarı alın.](../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)

2. JSON biçiminde verilerinizi yapılandırılmamış ham metin olarak içeren [bir istek düzenleyin](how-tos/text-analytics-how-to-call-api.md#json-schema).

3. İstek, kayıt sırasında oluşturulan bitiş noktasına gönderin ve istenen kaynağı ekleyin: duygu analizi, anahtar tümcecik çıkarma, dil algılama veya adlandırılmış varlık tanıma.

4. Yanıtın akışını yapın veya yerel ortamda depolayın. Gönderilen isteğe bağlı olarak sonuçlar yaklaşım puanı, ayıklanan anahtar ifadelerden oluşan bir koleksiyon veya dil kodu olacaktır.

Çıktı tek bir JSON belgesi olarak döndürülür ve kimlikle birlikte gönderdiğiniz tüm metin belgelerinin sonucunu içerir. Sonuçları daha sonra analiz ederek, görselleştirerek veya kategorilere ayırarak eyleme dönüştürülebilir içgörüler elde edebilirsiniz.

Veriler hesabınızda depolanmaz. Metin Analizi API'si tarafından gerçekleştirilen işlemlerde durum bilgisi yoktur. Başka bir deyişle sağladığınız metin işlenir ve sonuçlar anında döndürülür.

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>Birden fazla programlama deneyimi düzeyi için Metin Analizi

Programlama da çok fazla deneyiminiz olmasa bile, süreçlerinizde Metin Analizi API'sini kullanmaya başlayabilirsiniz. Bu öğreticileri kullanarak API'yi kullanarak metni deneyim düzeyinizi farklı şekillerde analiz edebilirsiniz. 

* En az programlama gereklidir:
    * [Metin Analizi ve Güç Otomatikleştirme'yi kullanarak Excel'de bilgi ayıklama](tutorials/extract-excel-information.md)
    * [Bir Yammer grubundaki yorumların duyarlılığını belirlemek için Metin Analizi API'sını ve MS Akışını kullanın](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Müşteri geri bildirimlerini analiz etmek için Power BI'yi Metin Analizi API'sıyla tümleştirin](tutorials/tutorial-power-bi-key-phrases.md)
* Programlama deneyimi önerilir:
    * [Azure Databricks kullanarak akış verileri üzerinde yaklaşım analizi](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Metni çevirmek, duyguları analiz etmek ve konuşmayı sentezlemek için bir Flask uygulaması oluşturun](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>Desteklenen diller

Bu bölüm, daha kolay bulunmasını sağlama amacıyla başka bir makaleye taşınmıştır. Bu içerik için [Metin Analizi API'sında desteklenen dillere](text-analytics-supported-languages.md) bakın.

<a name="data-limits"></a>

## <a name="data-limits"></a>Veri sınırları

Tüm Metin Analizi API'si uç noktaları ham metin verisi kabul eder. Geçerli sınır her belge için 5.120 karakterdir; daha büyük belgeleri çözümlemeniz gerekiyorsa, bunları daha küçük parçalara ayırabilirsiniz.

| Sınır | Değer |
|------------------------|---------------|
| Tek belge için maksimum boyut | 5.120 karakter olarak [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements)ölçülür. |
| İsteğin tamamının maksimum boyutu | 1 MB |
| Bir istekte bulunabilecek maksimum belge sayısı | 1000 belge |

Fiyat sınırınız fiyatlandırma seviyenize göre değişir.

| Katman          | Saniyedeki istek | Dakika başına istekler |
|---------------|---------------------|---------------------|
| S / Çoklu hizmet | 1000                | 1000                |
| S0 / F0         | 100                 | 300                 |
| S1            | 200                 | 300                 |
| S2            | 300                 | 300                 |
| S3            | 500                 | 500                 |
| S4            | 1000                | 1000                |

Her Metin Analizi özelliği için istekler ayrı olarak ölçülür. Örneğin, fiyatlandırma katmanınız için her özelliğe aynı anda en fazla istek gönderebilirsiniz.      

## <a name="unicode-encoding"></a>Unicode kodlama

Metin Analizi API'si, metin gösterimi ve karakter sayısı hesaplamaları için Unicode kodlamasını kullanır. İstekler UTF-8 ve UTF-16 olarak gönderilebilir, karakter sayısında fark olmayacaktır. Karakter uzunluğu için Unicode kod noktaları buluşsal değer olarak kullanılır ve metin analizi veri sınırları için eşdeğer kabul edilir. Karakter sayısını [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) almak için kullanıyorsanız, veri boyutunu ölçmek için kullandığımız yöntemi kullanıyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

+ Uygulamalarınız için önemli ve son nokta elde etmek için Metin Analizi için bir [Azure kaynağı oluşturun.](../cognitive-services-apis-create-account.md)

+ API çağrıları göndermeye başlamak için [hızlı başlatı](quickstarts/text-analytics-sdk.md) kullanın. Minimum kodla metin göndermeyi, analiz seçmeyi ve sonuçları görüntülemeyi öğrenin.

+ Yeni sürümler ve özellikler hakkında bilgi için [Metin Analizi API'sında nelerin olduğunu](whats-new.md) görün.

+ Azure Databricks'i kullanarak bu [duygu analizi öğreticisiyle](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services) biraz daha derine inin.

+ [Dış & Topluluk İçeriği sayfamızdaki](text-analytics-resource-external-community.md)diğer araçlar ve teknolojilerle Metin Analizi API'sini nasıl kullanacağınız hakkındaki blog gönderileri ve daha fazla video listemize göz atın.
