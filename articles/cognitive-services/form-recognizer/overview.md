---
title: Form Tanıma nedir?
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler Formu Tanıyır, form belgelerinden anahtar/değer çiftleri ve tablo verilerini tanımlamanıza ve ayıklamanıza olanak tanır.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 6919849d28573ad7388a7f2e317d2b8433f35559
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399444"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Form Tanıyıcısı, form belgelerinden metin, anahtar/değer çiftleri ve tablo verilerini tanımlamak ve ayıklamak için makine öğrenimi teknolojisini kullanan bir bilişsel hizmettir. Formlardan metin alır ve özgün dosyadaki ilişkileri içeren yapılandırılmış verilerden çıktı alır. Ağır manuel müdahale veya kapsamlı veri bilimi uzmanlığı olmadan özel içeriğinize göre uyarlanmış doğru sonuçları hızlı bir şekilde elde edersiniz. Form Tanıyıcısı özel modeller, önceden oluşturulmuş giriş modeli ve düzen API'den oluşur. Karmaşıklığı azaltmak ve iş akışınıza veya uygulamanıza entegre etmek için REST API kullanarak Form Recognizer modellerini arayabilirsiniz.

Form Tanıyıcısı aşağıdaki hizmetlerden oluşur:
* **Özel modeller** - Formlardan anahtar/değer çiftleri ve tablo verilerini ayıklayın. Bu modeller kendi verilerinizle eğitilir, böylece formlarınıza göre uyarlanırlar.
* **Önceden oluşturulmuş giriş modeli** - Önceden oluşturulmuş bir modeli kullanarak ABD satış makbuzlarından veri ayıklayın.
* **Düzen API** ' si - Metin ve tablo yapılarını, sınırlayıcı kutu koordinatlarıyla birlikte belgelerden ayıklayın.

<!-- add diagram -->

## <a name="custom-models"></a>Özel modeller

Form Recognizer özel modelleri kendi verilerinize çalışır ve başlamak için yalnızca beş örnek giriş formuna ihtiyacınız vardır. Eğitilmiş bir model, özgün form belgesindeki ilişkileri içeren yapılandırılmış veriler çıkartabilir. Modeli eğittikten sonra, modeli test edebilir ve yeniden eğitebilir ve sonunda ihtiyaçlarınıza göre daha fazla formdan güvenilir bir şekilde veri ayıklamak için kullanabilirsiniz.

Özel modeller eğitirken aşağıdaki seçeneklere sahipsiniz: etiketli verilerle ve etiketli veriler olmadan eğitim.

### <a name="train-without-labels"></a>Etiketsiz tren

Varsayılan olarak, Form Tanıyıcısı formlarınızdaki alanlar ve girişler arasındaki düzeni ve ilişkileri anlamak için denetimsiz öğrenmeyi kullanır. Giriş formlarınızı gönderdiğinizde, algoritma formları türüne göre kümeler, hangi anahtarların ve tabloların bulunduğunu keşfeder ve değerleri anahtarlarla ve tablolara girişlerle ilişkilendirir. Bu, manuel veri etiketleme veya yoğun kodlama ve bakım gerektirmez ve önce bu yöntemi denemenizi öneririz.

### <a name="train-with-labels"></a>Etiketlerle tren

Etiketli verilerle eğitim aldığınızda, model, sağladığınız etiketli formları kullanarak ilgi değerlerini ayıklamak için denetimli öğrenme yapar. Bu, daha iyi performans gösteren modeller ile sonuçlanır ve anahtarsız değerler içeren karmaşık formlarla veya formlarla çalışan modeller üretebilir.

Form Tanıyıcısı, yazdırılan ve el yazısımetin yazıldığı metin öğelerinin beklenen boyutlarını ve konumlarını öğrenmek için [Düzen API'sini](#layout-api) kullanır. Daha sonra belgelerdeki anahtar/değer çağrışımlarını öğrenmek için kullanıcı tarafından belirtilen etiketleri kullanır. Yeni bir modeli eğitirken başlamak ve modeldoğruluğunu artırmak için gerektiğinde daha fazla etiketli veri eklemek için aynı türde beş el ile etiketlenmiş form kullanmanızı öneririz.

## <a name="prebuilt-receipt-model"></a>Önceden oluşturulmuş makbuz modeli

Form Recognizer ayrıca, amerika birleşik devletleri&mdash;tarafından kullanılan tip, benzin istasyonları, perakende, vb[(örnek makbuz)](./media/contoso-receipt-small.png)İngilizce satış makbuzları okumak için bir model içerir. Bu model, işlemin saati ve tarihi, satıcı bilgileri, vergi tutarları ve toplamlar ve daha fazlası gibi önemli bilgileri ayıklar. Buna ek olarak, önceden oluşturulmuş makbuz modeli, tüm metni bir makbuzüzerindeki tanıma ve iade etmek üzere eğitilmiştir.

## <a name="layout-api"></a>Düzen API'si

Form Tanıyıcısı, yüksek tanımlı optik karakter tanıma (OCR) kullanarak metin ve tablo yapısını (metinle ilişkili satır ve sütun numaraları) ayıklayabilir.

## <a name="get-started"></a>başlarken

Formlarınızdan veri ayıklamaya başlamak için hızlı bir başlangıç izleyin. Teknolojiyi öğrenirken ücretsiz hizmeti kullanmanızı öneririz. Ücretsiz sayfa sayısının ayda 500 ile sınırlı olduğunu unutmayın.

* Özel - formlarıiçin bir model eğitmek
  * Etiketsiz tren
    * [Quickstart: CURL ile REST API kullanarak bir Form Tanıyıcı modeli ve ayıklama formu verileri tren](quickstarts/curl-train-extract.md)
    * [Quickstart: Python ile REST API'sini kullanarak form Tanıyıcı modelini eğitin ve form verilerini ayıklayın](quickstarts/python-train-extract.md)
  * Etiketlerle tren
    * [Örnek etiketleme aracını kullanarak etiketlerle form tanıyıcı modelini eğitin](quickstarts/label-tool.md)
    * [REST API ve Python kullanarak etiketleri olan bir Form Tanıyıcı modeli eğitin](quickstarts/python-labeled-data.md)
* Önceden oluşturulmuş makbuzlar - ABD satış makbuzlarından veri ayıklamak
  * [Quickstart: cURL kullanarak makbuz verilerini ayıklama](quickstarts/curl-receipts.md)
  * [Quickstart: Python kullanarak giriş verilerini ayıklama](quickstarts/python-receipts.md)
* Düzen - formlardan metin ve tablo yapısını ayıkla
  * [Quickstart: Python kullanarak düzen verilerini ayıklama](quickstarts/python-layout.md)

### <a name="review-the-rest-apis"></a>REST API'lerini gözden geçirin

Modelleri eğitmek ve formlardan yapılandırılmış verileri ayıklamak için aşağıdaki API'leri kullanırsınız.

|Adı |Açıklama |
|---|---|
| **Tren Özel Model**| Aynı türde beş form kullanarak formlarınızı analiz etmek için yeni bir model eğitin. El ile etiketlenmiş verilerle eğitmek için `true` _useLabelFile_ parametresini ayarlayın. |
| **Analiz Formu** |Özel modelinizle birlikte metin, anahtar/değer çiftleri ve tabloları formdan ayıklamak için akış olarak geçirilen tek bir belgeyi çözümleştirin.  |
| **Makbuzu Analiz Et** |Anahtar bilgileri ve diğer makbuz metnini ayıklamak için tek bir giriş belgesini çözümle.|
| **Düzeni Analiz Et** |Metin ve tablo yapısını ayıklamak için formun düzenini çözümle.|

Daha fazla bilgi edinmek için [REST API başvuru belgelerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm) keşfedin. API'nin önceki bir sürümünü biliyorsanız, son değişiklikler hakkında bilgi edinmek için [yeni](./whats-new.md) makaleye bakın.

## <a name="input-requirements"></a>Giriş gereksinimleri
### <a name="custom-model"></a>Özel model

[!INCLUDE [input requirements](./includes/input-requirements.md)]

### <a name="prebuilt-receipt-model"></a>Önceden oluşturulmuş makbuz modeli

Makbuz modeli için giriş gereksinimleri biraz farklıdır.

* Biçim JPEG, PNG, BMP, PDF (metin veya taranmış) veya TIFF olmalıdır.
* Dosya boyutu 20 MB'dan az olmalıdır.
* Görüntü boyutları 50 x 50 piksel ile 10000 x 10000 piksel arasında olmalıdır.
* PDF boyutları en fazla 17 x 17 inç olmalıdır, Yasal veya A3 kağıt boyutlarına karşılık gelir ve daha küçüktür.
* PDF ve TIFF için yalnızca ilk 200 sayfa işlenir (ücretsiz katman aboneliği yle yalnızca ilk iki sayfa işlenir).

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Bu hizmet, [Çevrimiçi Hizmet Koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)kapsamında bir Azure hizmetinin [önizlemesi](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) olarak sunulur. Tüm bilişsel hizmetlerde olduğu gibi, Form Recognizer hizmetini kullanan geliştiriciler, müşteri verilerine ilişkin Microsoft ilkelerinden haberdar olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi'ndeki [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Form Recognizer API'leri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)ile başlamak için hızlı bir [başlangıç](quickstarts/curl-train-extract.md) tamamlayın.
