---
title: Form Tanıma nedir?
titleSuffix: Azure Cognitive Services
description: Azure form tanıyıcı hizmeti, form belgelerinizden anahtar/değer çiftlerini ve tablo verilerini tanımlamanızı ve ayıklamanızı, Ayrıca satış alındıları ve iş kartlarından önemli bilgileri ayıklamanızı sağlar.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 09/21/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: Otomatik veri işleme, belge işleme, otomatik veri girişi, form işleme
ms.openlocfilehash: 5243c170e1f6b5f647057b8cfafbcac9b2fb4db3
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318968"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure form tanıyıcı, makine öğrenimi teknolojisini kullanarak otomatik veri işleme yazılımı oluşturmanıza imkan tanıyan bir bilişsel hizmettir. Formdan metin, anahtar/değer çiftleri ve tablo verilerini belirleyip ayıklayın &mdash; . hizmet, özgün dosyadaki ilişkileri içeren yapılandırılmış verileri çıktı olarak verir. El ile müdahale veya kapsamlı veri bilimi uzmanlığı olmadan, belirli içeriğinize kolayca doğru sonuçlar elde edersiniz. Uygulamalarınızda veri girişini otomatikleştirmek için form tanıyıcısı 'nı kullanın.

Form tanıyıcı özel belge işleme modellerden, önceden oluşturulmuş alış irsaliyesinden ve iş kartı modellerinden ve Düzen API 'sinden oluşur. Karmaşıklığı azaltmak ve iş akışınız veya uygulamanızla bütünleştirmek için bir REST API veya istemci kitaplığı SDK 'Ları kullanarak form tanıyıcı modellerini çağırabilirsiniz.

Form tanıyıcı aşağıdaki hizmetlerden oluşur:
* **[Özel modeller](#custom-models)** -formlardan anahtar/değer çiftlerini ve tablo verilerini ayıklayın. Bu modeller kendi verileriniz ile eğitilmiş olduğundan, formlarınıza göre uyarlanmıştır.
* **[Önceden oluşturulmuş modeller](#prebuilt-models)** -önceden oluşturulmuş modeller kullanarak benzersiz form türlerinden veri ayıklayın. Şu anda kullanılabilir, satış alındıları ve iş kartları için Ingilizce olarak önceden oluşturulmuş modellerdir.
* **[Düzen API 'si](#layout-api)** -metin ve tablo yapılarını, belgelerden sınırlayıcı kutu koordinatlarıyla birlikte ayıklayın.

## <a name="custom-models"></a>Özel modeller

Form tanıyıcı özel modelleri kendi verilerinize eğitme ve başlamak için yalnızca beş örnek giriş formunuz olması gerekir. Eğitilen bir belge işleme modeli, özgün form belgesindeki ilişkileri içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

Özel modeller eğlendirmede aşağıdaki seçenekleriniz vardır: etiketli verilerle ve etiketli veriler olmadan eğitim.

### <a name="train-without-labels"></a>Etiketler olmadan eğitme

Form tanıyıcı, formlardaki alanlar ve girişler arasındaki düzeni ve ilişkileri anlamak için, varsayılan olarak denetimsiz öğrenme kullanır. Giriş formlarınızı gönderdiğinizde, algoritma formları türe göre kümeler, hangi anahtarların ve tabloların mevcut olduğunu bulur ve değerleri anahtarlar ve girdilerle tablolarla ilişkilendirir. Bu, el ile veri etiketleme veya yoğun kodlama ve bakım gerektirmez ve öncelikle bu yöntemi denemenizi öneririz.

Eğitim belgelerinizi nasıl toplayacağınız hakkında ipuçları için bkz. [eğitim verileri kümesi oluşturma](./build-training-data-set.md) .

### <a name="train-with-labels"></a>Etiketlerle eğitme

Etiketli verilerle eğitedığınızda, model, sağladığınız etiketli formları kullanarak ilgilendiğiniz değerleri ayıklamak üzere öğrenir. Bu, daha iyi çalışan modellerle sonuçlanır ve anahtar içermeyen karmaşık formlarla veya formlarla çalışan modeller oluşturabilir.

Form tanıyıcı, yazdırılan ve el yazısı metin öğelerinin beklenen boyutlarını ve konumlarını öğrenmek için [Düzen API](#layout-api) 'sini kullanır. Ardından, belgelerde anahtar/değer ilişkilendirmelerini öğrenmek için Kullanıcı tarafından belirtilen etiketleri kullanır. Yeni bir modeli eğitmek ve model doğruluğunu artırmak için gerektiğinde daha fazla etiketli veri eklemek için aynı türden beş el ile etiketlenmiş beş form kullanmanızı öneririz.

## <a name="prebuilt-models"></a>Önceden oluşturulmuş modeller

Form tanıyıcı ayrıca benzersiz form türlerinin otomatik veri işlemeye yönelik önceden oluşturulmuş modeller de içerir.

### <a name="prebuilt-receipt-model"></a>Önceden oluşturulmuş makbuz modeli

Önceden oluşturulmuş makbuz modeli, Avustralya, Kanada, Büyük Britanya, Hindistan ve &mdash; Restoranlar, gaz istasyonları, perakende vb. tarafından kullanılan türden Birleşik Devletler İngilizce satış alındılarını okumak için kullanılır. Bu model, işlemin saati ve tarihi, ticari bilgiler, vergiler, satır öğeleri, toplamlar ve daha fazlası gibi önemli bilgileri ayıklar. Ayrıca, önceden oluşturulmuş makbuz modeli, bir Makbuzdaki tüm metni tanımak ve döndürmek için eğitilmiş olur. Daha fazla bilgi için bkz. [alındılar](./concept-receipts.md) kavramsal Kılavuzu.

![örnek alındısı](./media/contoso-receipt-small.png)

### <a name="prebuilt-business-cards-model"></a>Önceden oluşturulmuş Iş kartları modeli

Iş kartları modeli, kişinin adı, iş unvanı, adres, e-posta, şirket ve telefon numarası gibi bilgileri, iş kartlarından Ingilizce olarak ayıklamanızı sağlar. Daha fazla bilgi için bkz. [iş kartları](./concept-business-cards.md) kavramsal Kılavuzu.

![örnek iş kartı](./media/business-card-english.jpg)

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
  * Satış makbuzlarından veri ayıklama
    * [Alma verilerini ayıklama-kıvrımlı](quickstarts/curl-receipts.md)
    * [Alma verilerini Ayıkla-Python](quickstarts/python-receipts.md)
  * Kartvizitlerden veri ayıklama
    * [İş kartı verilerini Ayıkla-Python](quickstarts/python-business-cards.md)
  * Formdan metin ve tablo yapısını Ayıkla
    * [Düzen verilerini ayıklama-Python](quickstarts/python-layout.md)


### <a name="review-the-rest-apis"></a>REST API 'Leri gözden geçirme

Modelleri eğitmek ve formlardan yapılandırılmış verileri ayıklamak için aşağıdaki API 'Leri kullanacaksınız.

|Ad |Açıklama |
|---|---|
| **Özel modeli eğitme**| Formlarınızı aynı türden beş form kullanarak analiz etmek için yeni bir model eğitme. _Uselabelfile_ parametresini, `true` el ile etiketlenmiş verileri eğit olacak şekilde ayarlayın. |
| **Formu çözümle** |Metin, anahtar/değer çiftleri ve tablolardaki tabloları özel modelinize çıkarmak için akış olarak geçirilmiş tek bir belgeyi analiz edin.  |
| **Okundu bilgisi** |Anahtar bilgilerini ve diğer makbuz metnini ayıklamak için tek bir alış belgesi çözümleyin.|
| **Iş kartını çözümle** |Anahtar bilgileri ve metin ayıklamak için bir iş kartını analiz edin.|
| **Düzeni çözümle** |Metin ve tablo yapısını ayıklamak için formun yerleşimini çözümleyin.|

Daha fazla bilgi edinmek için [REST API başvuru belgelerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) inceleyin. API 'nin önceki bir sürümüne alışkın değilseniz, son değişiklikler hakkında bilgi edinmek için [Yenilikler](./whats-new.md) makalesine bakın.

## <a name="input-requirements"></a>Giriş gereksinimleri

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, form tanıyıcı hizmetini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Form tanıyıcı ile bir form işleme uygulamasını tercih ettiğiniz dilde yazmaya başlamak için bir [istemci kitaplığı hızlı](quickstarts/client-library.md) başlangıcını doldurun.