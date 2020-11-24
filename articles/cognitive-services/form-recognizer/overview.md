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
ms.openlocfilehash: 8b790da9eaa17cbb0fbdcbcc49682786deb319a0
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95492170"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure form tanıyıcı, makine öğrenimi teknolojisini kullanarak otomatik veri işleme yazılımı oluşturmanıza imkan tanıyan bir bilişsel hizmettir. Belgelerinizden metin, anahtar/değer çiftleri, seçim işaretleri, tablolar ve yapıyı belirleyip ayıklayın &mdash; . hizmet, özgün dosyadaki ilişkileri, sınırlayıcı kutuları, güvenilirliği ve daha fazlasını içeren yapılandırılmış verileri çıktı olarak verir. El ile müdahale veya kapsamlı veri bilimi uzmanlığı olmadan, belirli içeriğinize kolayca doğru sonuçlar elde edersiniz. Uygulamalarınızda veri girişini otomatikleştirmek ve belge arama olanaklarınızı zenginleştirmek için form tanıyıcısı 'nı kullanın.

Form tanıyıcı özel belge işleme modellerden, faturalar, alındılar ve iş kartlarında ve düzen modelinin önceden oluşturulmuş modellerinden oluşur. Karmaşıklığı azaltmak ve iş akışınız veya uygulamanızla bütünleştirmek için bir REST API veya istemci kitaplığı SDK 'Ları kullanarak form tanıyıcı modellerini çağırabilirsiniz.

Form tanıyıcı aşağıdaki hizmetlerden oluşur:
* **[Düzen API 'si](#layout-api)** -metin, seçim işaretleri ve tablo yapılarını, belgelerden sınırlayıcı kutu koordinatlarıyla birlikte ayıklayın.
* **[Özel modeller](#custom-models)** -metin, anahtar/değer çiftleri, seçim işaretleri ve formlardan tablo verilerini ayıklar. Bu modeller kendi verileriniz ile eğitilmiş olduğundan, formlarınıza göre uyarlanmıştır.
* **[Önceden oluşturulmuş modeller](#prebuilt-models)** -önceden oluşturulmuş modeller kullanarak benzersiz form türlerinden veri ayıklayın. Şu anda aşağıdaki önceden oluşturulmuş modeller mevcuttur
    * [Faturalar](./concept-invoices.md) 
    * [Satış alındıları](./concept-receipts.md)
    * [Kartvizitler](./concept-business-cards.md) 


## <a name="try-it-out"></a>Deneyin

Form tanıyıcı hizmetini denemek için çevrimiçi örnek UI aracına gidin:


# <a name="v20"></a>[v2.0](#tab/v2-0)
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri deneyin](https://fott.azurewebsites.net/)

# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri deneyin](https://fott-preview.azurewebsites.net/)

---

Form tanıyıcı hizmetini denemek için bir Azure aboneliğine ([ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)) ve bir [form tanıyıcı kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) uç noktası ve anahtarına ihtiyacınız olacaktır. 

## <a name="layout-api"></a>Düzen API 'SI

Form tanıyıcı, yüksek tanımlı optik karakter tanıma (OCR) ve belgelerden gelişmiş bir derin öğrenme modeli kullanarak metin, seçim işaretlerini ve tablo yapısını (metinle ilişkili satır ve sütun numaralarını) ayıklayabilir. Daha fazla bilgi için bkz. [Düzen](./concept-layout.md) kavramsal Kılavuzu.

:::image type="content" source="./media/tables-example.jpg" alt-text="tablolar örneği" lightbox="./media/tables-example.jpg":::

## <a name="custom-models"></a>Özel modeller

Form tanıyıcı özel modelleri kendi verilerinize eğitme ve başlamak için yalnızca beş örnek giriş formunuz olması gerekir. Eğitilen bir belge işleme modeli, özgün form belgesindeki ilişkileri içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

Özel modeller eğlendirmede aşağıdaki seçenekleriniz vardır: etiketli verilerle ve etiketli veriler olmadan eğitim.

### <a name="train-without-labels"></a>Etiketler olmadan eğitme

Form tanıyıcı, formlardaki alanlar ve girişler arasındaki düzeni ve ilişkileri anlamak için, varsayılan olarak denetimsiz öğrenme kullanır. Giriş formlarınızı gönderdiğinizde, algoritma formları türe göre kümeler, hangi anahtarların ve tabloların mevcut olduğunu bulur ve değerleri anahtarlar ve girdilerle tablolarla ilişkilendirir. Bu, el ile veri etiketleme veya yoğun kodlama ve bakım gerektirmez ve öncelikle bu yöntemi denemenizi öneririz.

Eğitim belgelerinizi nasıl toplayacağınız hakkında ipuçları için bkz. [eğitim verileri kümesi oluşturma](./build-training-data-set.md) .

### <a name="train-with-labels"></a>Etiketlerle eğitme

Etiketli verilerle eğitedığınızda, model, sağladığınız etiketli formları kullanarak ilgilendiğiniz değerleri ayıklamak üzere öğrenir. Bu, daha iyi çalışan modellerle sonuçlanır ve anahtar içermeyen karmaşık formlarla veya formlarla çalışan modeller oluşturabilir.

Form tanıyıcı, yazdırılan ve el yazısı metin öğelerinin beklenen boyutlarını ve konumlarını öğrenmek için [Düzen API](#layout-api) 'sini kullanır. Ardından, belgelerde anahtar/değer ilişkilendirmelerini öğrenmek için Kullanıcı tarafından belirtilen etiketleri kullanır. Yeni bir modeli eğitmek ve model doğruluğunu artırmak için gerektiğinde daha fazla etiketli veri eklemek için aynı türde (aynı yapıyla) beş el ile etiketlenmiş beş form kullanmanızı öneririz.

[Etiketlerle eğitme ile çalışmaya başlama](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/quickstarts/label-tool)


> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Azure-Form-Recognizer/player]


## <a name="prebuilt-models"></a>Önceden oluşturulmuş modeller

Form tanıyıcı ayrıca benzersiz form türlerinin otomatik veri işlemeye yönelik önceden oluşturulmuş modeller de içerir.

### <a name="prebuilt-invoice-model"></a>Önceden oluşturulmuş fatura modeli
Önceden oluşturulmuş fatura modeli, verileri çeşitli biçimlerdeki faturalardan ayıklar ve yapılandırılmış verileri döndürür. Bu model, fatura KIMLIĞI, müşteri ayrıntıları, satıcı ayrıntıları, sevk yeri, fatura, toplam, vergi, alt toplam ve daha fazlası gibi önemli bilgileri ayıklar. Ayrıca, önceden oluşturulmuş fatura modeli, faturadaki tüm metin ve tabloları tanımak ve döndürmek için eğitilmiş olur. Daha fazla bilgi için [faturalara](./concept-invoices.md) kavramsal kılavuza bakın.

:::image type="content" source="./media/overview-invoices.jpg" alt-text="örnek fatura" lightbox="./media/overview-invoices.jpg":::

### <a name="prebuilt-receipt-model"></a>Önceden oluşturulmuş makbuz modeli

Önceden oluşturulmuş makbuz modeli, Avustralya, Kanada, Büyük Britanya, Hindistan ve &mdash; Restoranlar, gaz istasyonları, perakende vb. tarafından kullanılan türden Birleşik Devletler İngilizce satış alındılarını okumak için kullanılır. Bu model, işlemin saati ve tarihi, ticari bilgiler, vergiler, satır öğeleri, toplamlar ve daha fazlası gibi önemli bilgileri ayıklar. Ayrıca, önceden oluşturulmuş makbuz modeli, bir Makbuzdaki tüm metni tanımak ve döndürmek için eğitilmiş olur. Daha fazla bilgi için bkz. [alındılar](./concept-receipts.md) kavramsal Kılavuzu.

:::image type="content" source="./media/overview-receipt.jpg" alt-text="örnek alındısı" lightbox="./media/overview-receipt.jpg":::

### <a name="prebuilt-business-cards-model"></a>Önceden oluşturulmuş Iş kartları modeli

Iş kartları modeli, kişinin adı, iş unvanı, adres, e-posta, şirket ve telefon numarası gibi bilgileri, iş kartlarından Ingilizce olarak ayıklamanızı sağlar. Daha fazla bilgi için bkz. [iş kartları](./concept-business-cards.md) kavramsal Kılavuzu.

:::image type="content" source="./media/overview-business-card.jpg" alt-text="örnek iş kartı" lightbox="./media/overview-business-card.jpg":::


## <a name="get-started"></a>Kullanmaya başlayın

Formlarınızın verileri çıkarmaya başlamak için [örnek form tanıyıcı aracını](https://fott.azurewebsites.net/) kullanın veya bir hızlı başlangıcı izleyin. Teknolojiyi öğrenirken ücretsiz hizmeti kullanmanızı öneririz. Ücretsiz sayfa sayısının ayda 500 ile sınırlı olduğunu unutmayın.

* [İstemci kitaplığı hızlı başlangıç](./quickstarts/client-library.md) (tüm diller, birden çok senaryo)
* Web UI hızlı başlangıçlarını
  * [Etiketlerle eğitme-örnek etiketleme aracı](quickstarts/label-tool.md)
* REST hızlı başlangıç
 * Belgelerden metin, seçim işaretleri ve tablo yapısını Ayıkla
    * [Düzen verilerini ayıklama-Python](quickstarts/python-layout.md)
  * Özel modeller eğitme ve form verilerini ayıklama
    * [Etiketler olmadan eğitme](quickstarts/curl-train-extract.md)
    * [Etiketler olmadan eğitme-Python](quickstarts/python-train-extract.md)
    * [Etiketlerle eğitme-Python](quickstarts/python-labeled-data.md)
  * Faturalardan verileri Ayıkla
    * [Fatura verilerini Ayıkla-Python](quickstarts/python-invoices.md)
  * Satış makbuzlarından veri ayıklama
    * [Alma verilerini ayıklama-kıvrımlı](quickstarts/curl-receipts.md)
    * [Alma verilerini Ayıkla-Python](quickstarts/python-receipts.md)
  * Kartvizitlerden veri ayıklama
    * [İş kartı verilerini Ayıkla-Python](quickstarts/python-business-cards.md)
 


### <a name="review-the-rest-apis"></a>REST API 'Leri gözden geçirme

Modelleri eğitmek ve formlardan yapılandırılmış verileri ayıklamak için aşağıdaki API 'Leri kullanacaksınız.

|Ad |Açıklama |
|---|---|
| **Düzeni çözümle** | Belgedeki metin, seçim işaretleri, tablolar ve yapıyı ayıklamak için akış olarak geçirilmiş bir belgeyi çözümleme |
| **Özel modeli eğitme**| Formlarınızı aynı türden beş form kullanarak analiz etmek için yeni bir model eğitme. _Uselabelfile_ parametresini, `true` el ile etiketlenmiş verileri eğit olacak şekilde ayarlayın. |
| **Formu çözümle** |Metin, anahtar/değer çiftleri ve tablolardaki tabloları özel modelinize çıkarmak için akış olarak geçirilmiş bir formu analiz edin.  |
| **Faturayı çözümle** | Ana bilgileri, tabloları ve diğer fatura metinlerini ayıklamak için bir faturayı çözümleyin.|
| **Okundu bilgisi** | Anahtar bilgilerini ve diğer makbuz metnini ayıklamak için bir makbuz belgesi çözümleyin.|
| **Iş kartını çözümle** | Anahtar bilgileri ve metin ayıklamak için bir iş kartını analiz edin.|


# <a name="v20"></a>[v2.0](#tab/v2-0)
Daha fazla bilgi edinmek için [REST API başvuru belgelerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) inceleyin. API 'nin önceki bir sürümüne alışkın değilseniz, son değişiklikler hakkında bilgi edinmek için [Yenilikler](./whats-new.md) makalesine bakın.

# <a name="v21"></a>[v2.1](#tab/v2-1)
Daha fazla bilgi edinmek için [REST API başvuru belgelerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeWithCustomForm) inceleyin. API 'nin önceki bir sürümüne alışkın değilseniz, son değişiklikler hakkında bilgi edinmek için [Yenilikler](./whats-new.md) makalesine bakın.

---

## <a name="input-requirements"></a>Giriş gereksinimleri

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, form tanıyıcı hizmetini kullanan geliştiriciler müşteri verilerinde Microsoft ilkeleriyle uyumlu olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Form tanıyıcı ile bir form işleme uygulamasını tercih ettiğiniz dilde yazmaya başlamak için bir [istemci kitaplığı hızlı](quickstarts/client-library.md) başlangıcını doldurun.
