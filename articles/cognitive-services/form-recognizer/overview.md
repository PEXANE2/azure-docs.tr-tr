---
title: Form Tanıma nedir?
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler formu tanıyıcı, form belgelerinden anahtar/değer çiftlerini ve tablo verilerini tanımlamanızı ve ayıklamanızı sağlar.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 5b7a060aa2313902e4644fdd15068e06816e3f58
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81529901"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure form tanıyıcı, form belgelerinden metin, anahtar/değer çiftleri ve tablo verilerini tanımlamak ve ayıklamak için makine öğrenimi teknolojisini kullanan bilişsel bir hizmettir. Formdan metin alır ve özgün dosyadaki ilişkileri içeren yapılandırılmış verileri çıkarır. El ile müdahale veya kapsamlı veri bilimi uzmanlığı olmadan, belirli içeriğinize kolayca doğru sonuçlar elde edersiniz. Form tanıyıcı özel modellerden, önceden oluşturulmuş makbuz modelinden ve Düzen API 'sinden oluşur. Karmaşıklığı azaltmak ve iş akışınız veya uygulamanızla bütünleştirmek için bir REST API kullanarak form tanıyıcı modellerini çağırabilirsiniz.

Form tanıyıcı aşağıdaki hizmetlerden oluşur:
* **Özel modeller** -formlardan anahtar/değer çiftlerini ve tablo verilerini ayıklayın. Bu modeller kendi verileriniz ile eğitilmiş olduğundan, formlarınıza göre uyarlanmıştır.
* **Önceden oluşturulmuş makbuz modeli** -önceden oluşturulmuş bir model kullanarak ABD satış makbuzlarından verileri ayıklayın.
* **Düzen API 'si** -metin ve tablo yapılarını, belgelerden sınırlayıcı kutu koordinatlarıyla birlikte ayıklayın.

<!-- add diagram -->

## <a name="custom-models"></a>Özel modeller

Form tanıyıcı özel modelleri kendi verilerinize eğitme ve başlamak için yalnızca beş örnek giriş formunuz olması gerekir. Eğitilen bir model, özgün form belgesindeki ilişkileri içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

Özel modeller eğlendirmede aşağıdaki seçenekleriniz vardır: etiketli verilerle ve etiketli veriler olmadan eğitim.

### <a name="train-without-labels"></a>Etiketler olmadan eğitme

Form tanıyıcı, formlardaki alanlar ve girişler arasındaki düzeni ve ilişkileri anlamak için, varsayılan olarak denetimsiz öğrenme kullanır. Giriş formlarınızı gönderdiğinizde, algoritma formları türe göre kümeler, hangi anahtarların ve tabloların mevcut olduğunu bulur ve değerleri anahtarlar ve girdilerle tablolarla ilişkilendirir. Bu, el ile veri etiketleme veya yoğun kodlama ve bakım gerektirmez ve öncelikle bu yöntemi denemenizi öneririz.

### <a name="train-with-labels"></a>Etiketlerle eğitme

Etiketli verilerle eğitedığınızda, model, sağladığınız etiketli formları kullanarak ilgilendiğiniz değerleri ayıklamak üzere öğrenir. Bu, daha iyi çalışan modellerle sonuçlanır ve anahtar içermeyen karmaşık formlarla veya formlarla çalışan modeller oluşturabilir.

Form tanıyıcı, yazdırılan ve el yazısı metin öğelerinin beklenen boyutlarını ve konumlarını öğrenmek için [Düzen API](#layout-api) 'sini kullanır. Ardından, belgelerde anahtar/değer ilişkilendirmelerini öğrenmek için Kullanıcı tarafından belirtilen etiketleri kullanır. Yeni bir modeli eğitmek ve model doğruluğunu artırmak için gerektiğinde daha fazla etiketli veri eklemek için aynı türden beş el ile etiketlenmiş beş form kullanmanızı öneririz.

## <a name="prebuilt-receipt-model"></a>Önceden oluşturulmuş makbuz modeli

Form tanıyıcı Ayrıca, restoranlar, gaz istasyonları, perakende vb. (&mdash;[örnek alış irsaliyesi](./media/contoso-receipt-small.png)) tarafından kullanılan türden Birleşik Devletler İngilizce satış alındılarını okumak için bir model içerir. Bu model, işlemin saati ve tarihi, ticari bilgiler, vergiler ve toplam miktar gibi önemli bilgileri ayıklar. Ayrıca, önceden oluşturulmuş makbuz modeli, bir Makbuzdaki tüm metni tanımak ve döndürmek için eğitilmiş olur.

## <a name="layout-api"></a>Düzen API 'SI

Form tanıyıcı, yüksek tanımlı optik karakter tanıma (OCR) kullanarak metin ve tablo yapısını (metinle ilişkili satır ve sütun numaraları) de ayıklayabilir.

## <a name="get-started"></a>başlarken

Formlarınızın verilerini ayıklamaya başlamak için hızlı başlangıcı izleyin. Teknolojiyi öğrenirken ücretsiz hizmeti kullanmanızı öneririz. Ücretsiz sayfa sayısının ayda 500 ile sınırlı olduğunu unutmayın.

* Özel-formlarınızı modelize eğitme
  * Etiketler olmadan eğitme
    * [Hızlı başlangıç: kıvrımlı ile REST API kullanarak form tanıyıcı modeli eğitme ve form verilerini ayıklama](quickstarts/curl-train-extract.md)
    * [Hızlı başlangıç: Python ile REST API kullanarak form tanıyıcı modeli eğitme ve form verilerini ayıklama](quickstarts/python-train-extract.md)
  * Etiketlerle eğitme
    * [Örnek etiketleme aracını kullanarak form tanıyıcı modelini etiketlerle eğitme](quickstarts/label-tool.md)
    * [REST API ve Python kullanarak etiketli form tanıyıcı modelini eğitme](quickstarts/python-labeled-data.md)
* Önceden oluşturulmuş alındılar-ABD satış makbuzlarından veri ayıklama
  * [Hızlı başlangıç: kıvrımlı kullanarak alış verilerini ayıklama](quickstarts/curl-receipts.md)
  * [Hızlı başlangıç: Python kullanarak alış verilerini ayıklama](quickstarts/python-receipts.md)
* Düzen-formlardan metin ve tablo yapısını Ayıkla
  * [Hızlı başlangıç: Python kullanarak düzen verilerini ayıklama](quickstarts/python-layout.md)

### <a name="review-the-rest-apis"></a>REST API 'Leri gözden geçirme

Modelleri eğitmek ve formlardan yapılandırılmış verileri ayıklamak için aşağıdaki API 'Leri kullanacaksınız.

|Adı |Açıklama |
|---|---|
| **Özel modeli eğitme**| Formlarınızı aynı türden beş form kullanarak analiz etmek için yeni bir model eğitme. _Uselabelfile_ parametresini, el ile `true` etiketlenmiş verileri eğit olacak şekilde ayarlayın. |
| **Formu çözümle** |Metin, anahtar/değer çiftleri ve tablolardaki tabloları özel modelinize çıkarmak için akış olarak geçirilmiş tek bir belgeyi analiz edin.  |
| **Okundu bilgisi** |Anahtar bilgilerini ve diğer makbuz metnini ayıklamak için tek bir alış belgesi çözümleyin.|
| **Düzeni çözümle** |Metin ve tablo yapısını ayıklamak için formun yerleşimini çözümleyin.|

Daha fazla bilgi edinmek için [REST API başvuru belgelerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm) inceleyin. API 'nin önceki bir sürümüne alışkın değilseniz, son değişiklikler hakkında bilgi edinmek için [Yenilikler](./whats-new.md) makalesine bakın.

## <a name="input-requirements"></a>Giriş gereksinimleri
### <a name="custom-model"></a>Özel model

[!INCLUDE [input requirements](./includes/input-requirements.md)]

### <a name="prebuilt-receipt-model"></a>Önceden oluşturulmuş makbuz modeli

Makbuz modeli için giriş gereksinimleri biraz farklıdır.

* Biçim JPEG, PNG, PDF (metin veya taranan) veya TIFF olmalıdır.
* Dosya boyutu 20 MB 'tan az olmalıdır.
* Resim boyutları 50 x 50 piksel ve 10000 x 10000 piksel arasında olmalıdır.
* PDF boyutları, yasal veya A3 kağıt boyutlarına ve daha küçük boyutlara karşılık gelen en fazla 17 x 17 inç olmalıdır.
* PDF ve TIFF için yalnızca ilk 200 sayfa işlenir (ücretsiz bir katman aboneliğiyle yalnızca ilk iki sayfa işlenir).

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Bu hizmet, [çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)kapsamında bir Azure hizmetinin [önizlemesi](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) olarak sunulur. Tüm bilişsel hizmetlerde olduğu gibi, form tanıyıcı hizmetini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Form tanıyıcı API 'lerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)kullanmaya başlamak için [hızlı](quickstarts/curl-train-extract.md) başlangıcı doldurun.
