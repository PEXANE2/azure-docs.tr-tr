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
ms.openlocfilehash: f5d0e9a851c41fa88461ed790ad3fe4e89d0fa21
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85957148"
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

Form tanıyıcı Ayrıca &mdash; , restoranlar, gaz istasyonları, perakende vb. ([örnek alış irsaliyesi](./media/contoso-receipt-small.png)) tarafından kullanılan türden Birleşik Devletler İngilizce satış alındılarını okumak için bir model içerir. Bu model, işlemin saati ve tarihi, ticari bilgiler, vergiler ve toplam miktar gibi önemli bilgileri ayıklar. Ayrıca, önceden oluşturulmuş makbuz modeli, bir Makbuzdaki tüm metni tanımak ve döndürmek için eğitilmiş olur.

## <a name="layout-api"></a>Düzen API 'SI

Form tanıyıcı, yüksek tanımlı optik karakter tanıma (OCR) kullanarak metin ve tablo yapısını (metinle ilişkili satır ve sütun numaraları) de ayıklayabilir.

## <a name="get-started"></a>Kullanmaya başlayın

Formlarınızın verilerini ayıklamaya başlamak için hızlı başlangıcı izleyin. Teknolojiyi öğrenirken ücretsiz hizmeti kullanmanızı öneririz. Ücretsiz sayfa sayısının ayda 500 ile sınırlı olduğunu unutmayın.

* [İstemci kitaplığı hızlı başlangıç](./quickstarts/client-library.md) (tüm diller, birden çok senaryo)
* Web UI hızlı başlangıçlarını
  * [Etiketlerle eğitme-örnek etiketleme aracı](quickstarts/label-tool.md)
* REST hızlı başlangıç
  * Özel modeller eğitme ve form verilerini ayıklama
    * [Etiketler olmadan eğitme](quickstarts/curl-train-extract.md)
    * [Etiketler olmadan eğitme-Python](quickstarts/python-train-extract.md)
    * [Etiketlerle eğitme-Python](quickstarts/python-labeled-data.md)
  * ABD satış makbuzlarından veri ayıklama
    * [Alma verilerini ayıklama-kıvrımlı](quickstarts/curl-receipts.md)
    * [Alma verilerini Ayıkla-Python](quickstarts/python-receipts.md)
  * Formdan metin ve tablo yapısını Ayıkla
    * [Düzen verilerini ayıklama-Python](quickstarts/python-layout.md)


### <a name="review-the-rest-apis"></a>REST API 'Leri gözden geçirme

Modelleri eğitmek ve formlardan yapılandırılmış verileri ayıklamak için aşağıdaki API 'Leri kullanacaksınız.

|Name |Description |
|---|---|
| **Özel modeli eğitme**| Formlarınızı aynı türden beş form kullanarak analiz etmek için yeni bir model eğitme. _Uselabelfile_ parametresini, `true` el ile etiketlenmiş verileri eğit olacak şekilde ayarlayın. |
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

Tüm bilişsel hizmetlerde olduğu gibi, form tanıyıcı hizmetini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Form tanıyıcı API 'lerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)kullanmaya başlamak için [hızlı](quickstarts/curl-train-extract.md) başlangıcı doldurun.
