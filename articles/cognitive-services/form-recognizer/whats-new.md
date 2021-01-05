---
title: Form Tanıma'daki yenilikler
titleSuffix: Azure Cognitive Services
description: Form tanıyıcı API 'sindeki en son değişiklikleri anlayın.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: pafarley
ms.openlocfilehash: 7539e896d810a2f0fbf3c07f389f25e2d3c31fe1
ms.sourcegitcommit: 5ef018fdadd854c8a3c360743245c44d306e470d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845002"
---
# <a name="whats-new-in-form-recognizer"></a>Form Tanıma'daki yenilikler

Form tanıyıcı hizmeti sürekli olarak güncelleştirilir. Özellik geliştirmeleri, düzeltmeler ve belge güncelleştirmeleriyle güncel kalmak için bu makaleyi kullanın.

## <a name="november-2020"></a>Kasım 2020

### <a name="new-features"></a>Yeni özellikler

**Form tanıyıcı v 2.1 genel önizleme 2 artık kullanılabilir.** Aşağıdaki özellikler dahil olmak üzere v 2.1-Preview. 2 yayımlandı: 

- **Yeni önceden oluşturulmuş fatura modeli** -yeni önceden oluşturulmuş fatura modeli, müşterilerin çeşitli biçimlerde faturalar almasını ve fatura işlemenin otomatikleştirilmesi için yapılandırılmış verileri döndürmesini sağlar. Güçlü optik karakter tanıma (OCR) olanaklarımızı, fatura ile ilgili ayrıntılı öğrenme modellerini anlamak için, gelişmiş bir deyişle, önemli bilgileri Ingilizce olarak birleştirir. Metin, tablo ve müşteri, satıcı, fatura KIMLIĞI, fatura son tarihi, toplam, ödenecek süre, vergi tutarı, sevk edilecek, fatura vb. gibi bilgileri ayıklar.

  > [Önceden oluşturulmuş fatura modeli hakkında daha fazla bilgi edinin](concept-invoices.md)

  :::image type="content" source="./media/invoice-example.jpg" alt-text="Fatura örneği" lightbox="./media/invoice-example.jpg":::

- **Gelişmiş tablo ayıklama** -form tanıyıcı artık güçlü optik karakter tanıma (OCR) yeteneklerini derin bir öğrenme tablosu ayıklama modeliyle birleştiren gelişmiş tablo ayıklama özelliği sağlıyor. Form tanıyıcı, birleştirilmiş sütunları, satırları, kenarlıkları ve daha fazlasını içeren karmaşık tablolar da dahil olmak üzere tablolardan verileri ayıklayabilir. 
 
  :::image type="content" source="./media/tables-example.jpg" alt-text="tablolar örneği" lightbox="./media/tables-example.jpg":::

 
  > [Düzen ayıklama hakkında daha fazla bilgi edinin](concept-layout.md)

- **İstemci kitaplığı güncelleştirmesi** -.net, Python, Java ve JavaScript için [istemci kitaplıklarının](quickstarts/client-library.md) en son SÜRÜMLERINDE, tanıyıcı 2,1 API biçimi desteklenir.
- **Desteklenen yeni dil: Japonca** -şu yeni diller artık desteklenmektedir: `AnalyzeLayout` ve için `AnalyzeCustomForm` : Japonca ( `ja` ). [Dil desteği](language-support.md)
- **Metin çizgisi stil göstergesi (el ile/diğer) (yalnızca Latin dilleri)** -form tanıyıcı `appearance` , her metin satırının bir güvenilirlik puanıyla birlikte el ile yazılmış tarzına göre bir nesne sınıflandırılmasına neden olur. Bu özellik yalnızca Latin dilleri için desteklenir.
- **Kalite geliştirmeleri** -tek basamaklı ayıklama geliştirmeleri dahil ayıklama geliştirmeleri.
- **Form tanıyıcı örneği ve etiketleme aracında yeni TRY-It-Out özelliği** , önceden oluşturulmuş fatura, giriş ve iş kartı modellerini ve Düzen API 'sini kullanarak, tanıyıcı örnek etiketleme aracı 'nı kullanarak bir kez deneme özelliği. Verilerinizin herhangi bir kod yazmadan ayıklanmasını görün.

  > [Form tanıyıcı örnek aracını deneyin](https://fott-preview.azurewebsites.net/)

  ![FOTT örneği](./media/ui-preview.jpg)
  
- **Geri bildirim döngüsü** -dosyaları örnek etiketleme aracı aracılığıyla analiz edilirken, artık bunu eğitim kümesine ekleyebilir ve gerekiyorsa etiketleri ayarlayabilir ve modeli geliştirmek için eğitebilirsiniz.
- **Belgeleri otomatik etiketle** -projedeki önceki etiketli belgelere göre ek belgelere otomatik olarak Etiketler.

## <a name="august-2020"></a>Ağustos 2020

### <a name="new-features"></a>Yeni özellikler

**Form tanıyıcı v 2.1 Genel önizlemesi kullanıma sunuldu.** Aşağıdaki özellikler dahil olmak üzere v 2.1-Preview. 1 yayımlandı: 


- **REST API başvuru kullanılabilir** - [v 2.1-Preview. 1 başvurusunu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) görüntüleyin 
- **İngilizce 'nin yanı sıra desteklenen yeni diller** [şunlardır:](language-support.md) `Layout` ve için `Train Custom Model` : Ingilizce ( `en` ), Çince (Basitleştirilmiş) (), Felemenkçe (), Fransızca (), Almanca (), İtalyanca (), `zh-Hans` `nl` `fr` `de` `it` Portekizce () `pt` ve İspanyolca ( `es` ).
- **Onay kutusu/seçim işareti algılama** – form tanıyıcı, onay kutuları ve radyo düğmeleri gibi seçim işaretlerinin algılanmasını ve ayıklanmasını destekler. Seçim işaretleri ' de ayıklanır `Layout` ve artık, `Train Custom Model`  -  seçim işaretleri için anahtar değer çiftlerini ayıklamak üzere _etiketlerle eğitme_ ' de etiketleyebilir ve eğitebilirsiniz. 
- **Model oluşturma** -birden çok modelin tek BIR model kimliğiyle oluşturulmasına ve çağrılmasına izin verir. Bir belge, oluşturulmuş bir model KIMLIĞIYLE analiz edilecek şekilde gönderildiğinde, ilk olarak doğru özel modele yönlendirmek için bir sınıflandırma adımı yapılır. Model oluşturma, `Train Custom Model`  -  _etiketlerle eğitebilir_.
- **Model adı** -daha kolay yönetim ve izleme için özel modellerinize kolay bir ad ekleyin.
- Ingilizce, dil iş kartlarında ortak alanların ayıklanması için **[Iş kartları için önceden oluşturulmuş yeni model](concept-business-cards.md)** .
- **[Önceden oluşturulmuş alındılar Için yeni yerel ayarlar](concept-receipts.md)** en-US, destek artık en-au, en-CA, en-GB, en-ın için kullanılabilir
- İçin **kalite iyileştirmeleri** `Layout` , `Train Custom Model`  -  _Etiketler olmadan eğitme_ ve _etiketlerle eğitme_.


**v 2.0** aşağıdaki güncelleştirmeyi içerir:

- NET, Python, Java ve JavaScript için [istemci kitaplıkları](quickstarts/client-library.md) genel kullanılabilirliği olarak girilmiş. 


GitHub 'da **yeni örnekler** mevcuttur. 
- [Bilgi ayıklama Tarifimlerini-Forms PlayBook](https://github.com/microsoft/knowledge-extraction-recipes-forms) , gerçek form tanıyıcı müşteri görevlendirmelerinden en iyi uygulamaları toplar ve bu projeleri geliştirmede kullanılan kullanılabilir kod örnekleri, Yapılacaklar listeleri ve örnek işlem hatları sağlar. 
- [Örnek etiketleme aracı](https://github.com/microsoft/OCR-Form-Tools) , yeni v 2.1 işlevlerini destekleyecek şekilde güncelleştirilmiştir. Aracı kullanmaya başlamak için bu [hızlı](quickstarts/label-tool.md) başlangıca bakın. 
- [Akıllı bilgi noktası](https://github.com/microsoft/Cognitive-Samples-IntelligentKiosk/blob/master/Documentation/FormRecognizer.md) formu tanıyıcı örneği, `Analyze Receipt` Etiketler olmadan tümleştirme ve `Train Custom Model`  -  _eğitme_ yöntemini gösterir.



## <a name="july-2020"></a>Temmuz 2020

### <a name="new-features"></a>Yeni özellikler

* **v 2.0 başvurusu kullanılabilir** - [.net](/dotnet/api/overview/azure/ai.formrecognizer-readme?view=azure-dotnet), [Python](/python/api/overview/azure/?view=azure-python), [Java](/java/api/overview/azure/ai-formrecognizer-readme?view=azure-java-preview)ve [JavaScript](/javascript/api/overview/azure/?view=azure-node-latest)için [v 2.0 API başvurusunu](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) ve güncelleştirilmiş SDK 'ları görüntüleyin.
* **Tablo geliştirmeleri ve ayıklama geliştirmeleri** -özellikle, _Etiketler olmadan özel eğilelerde_ Tablo üstbilgilerini ve yapıları öğrenme özelliğini içeren doğruluk iyileştirmeleri ve tablo dışlamaları geliştirmeleri içerir. 

* **Para birimi desteği** -genel para birimi simgelerinin algılanması ve ayıklanması.
* **Azure gov** -form tanıyıcı artık Azure gov 'de de kullanılabilir.
* **Gelişmiş güvenlik özellikleri**: 
   * **Kendi anahtar** formu tanıyıcı, buluta kalıcı hale geldiğinde verilerinizi otomatik olarak şifreler ve kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Aboneliğiniz varsayılan olarak Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Ayrıca, aboneliğinizi kendi şifreleme anahtarlarınız ile yönetebilirsiniz. [Kendi anahtarını getir (BYOK) olarak da bilinen, müşteri tarafından yönetilen anahtarlar](./form-recognizer-encryption-of-data-at-rest.md), erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.  
   * **Özel uç noktalar** – [özel bir bağlantı üzerinden verilerinize güvenli](../../private-link/private-link-overview.md) bir şekilde erişmek için bir sanal ağ (VNet) sağlar.


## <a name="june-2020"></a>Haziran 2020

### <a name="new-features"></a>Yeni özellikler
* **Istemci SDK 'larına eklenen Copymodel API 'si** -artık bir abonelikten diğerine model kopyalamak Için istemci SDK 'larını kullanabilirsiniz. Bu özellikle ilgili genel bilgiler için bkz. [yedekleme ve kurtarma modelleri](./disaster-recovery.md) .
* **Azure Active Directory tümleştirme** -artık SDK 'Larda form tanıyıcı istemci nesnelerinizin kimliğini doğrulamak IÇIN Azure AD kimlik bilgilerinizi kullanabilirsiniz.
* **SDK 'ya özgü değişiklikler** -bu, hem küçük özellik eklemeleri hem de son değişiklikleri içerir. Daha fazla bilgi için bkz. SDK changelog.
  * [C# SDK Preview 3 CHANGELOG](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/formrecognizer/Azure.AI.FormRecognizer/CHANGELOG.md)
  * [Python SDK Preview 3 CHANGELOG](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/CHANGELOG.md)
  * [Java SDK Preview 3 CHANGELOG](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/CHANGELOG.md)
  * [JavaScript SDK Preview 3 CHANGELOG](https://github.com/Azure/azure-sdk-for-js/blob/%40azure/ai-form-recognizer_1.0.0-preview.3/sdk/formrecognizer/ai-form-recognizer/CHANGELOG.md)

## <a name="april-2020"></a>Nisan 2020

### <a name="new-features"></a>Yeni özellikler
* **Form TANıYıCı API v 2.0 genel önizlemesi Için SDK desteği** -bu ay, form tanıyıcı v 2.0 (Önizleme) sürümü için bir önizleme SDK 'sı eklemek üzere hizmet desteğimizi genişlettik. Seçtiğiniz dille çalışmaya başlamak için aşağıdaki bağlantıları kullanın: 
   * [.NET SDK](/dotnet/api/overview/azure/ai.formrecognizer-readme?view=azure-dotnet)
   * [Java SDK](/java/api/overview/azure/ai-formrecognizer-readme?view=azure-java-preview)
   * [Python SDK'sı](/python/api/overview/azure/ai-formrecognizer-readme?view=azure-python-preview)
   * [JavaScript SDK'sı](/javascript/api/overview/azure/ai-form-recognizer-readme?view=azure-node-preview)

  Yeni SDK, form tanıyıcı için v 2.0 REST API tüm özelliklerini destekler. Örneğin, etiketlere sahip veya olmayan bir modeli eğitebilir ve formlarınızdaki metin, anahtar değer çiftleri ve tablolar çıkarabilir, önceden oluşturulmuş alındılar hizmeti ile alış irsaliyelerinden verileri ayıklayabilir ve belgelerden düzen hizmetiyle birlikte metin ve tabloları ayıklayabilir. [SDK geri bildirim formu aracılığıyla SDK](https://aka.ms/FR_SDK_v1_feedback)'larda geri bildirimlerinizi paylaşabilirsiniz.
 
* **Özel modeli Kopyala** Artık yeni özel model Kopyala özelliğini kullanarak bölgeler ve abonelikler arasında modeller kopyalayabilirsiniz. Özel model Kopyala API 'sini çağırmadan önce, hedef kaynak uç noktasına karşı yetkilendirme kopyalama işlemini çağırarak hedef kaynağa kopyalamak için Yetkilendirmeyi edinmeniz gerekir.
   * [Kopya yetkilendirmesi oluşturma](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/CopyCustomFormModelAuthorization) REST API
   * [Özel bir modeli kopyalama](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/CopyCustomFormModel) REST API 

### <a name="security-improvements"></a>Güvenlik iyileştirmeleri

* Customer-Managed anahtarlar artık Formtanıyıcı için kullanılabilir. Daha fazla bilgi için bkz. [rest for form tanıyıcı Için veri şifreleme](./form-recognizer-encryption-of-data-at-rest.md).
* Azure Active Directory ile Azure kaynaklarına erişim için Yönetilen kimlikler kullanın. Daha fazla bilgi için bkz. [yönetilen kimliklere erişim yetkisi verme](../authentication.md#authorize-access-to-managed-identities).

## <a name="march-2020"></a>Mart 2020 

### <a name="new-features"></a>Yeni özellikler

* **Etiketleme Için değer türleri** Artık etiketettiğiniz değer türlerini, tanıyıcı örnek etiketleme aracı ile belirtebilirsiniz. Aşağıdaki değer türleri ve Çeşitlemeler Şu anda destekleniyor:
  * `string`
    * Varsayılan, `no-whitespaces` , `alphanumeric`
  * `number`
    * varsayılanını `currency`
  * `date` 
    * Varsayılan, `dmy` , `mdy` , `ymd`
  * `time`
  * `integer`

  Bu özelliği nasıl kullanacağınızı öğrenmek için bkz. [örnek etiketleme araç](./quickstarts/label-tool.md#specify-tag-value-types) Kılavuzu.


* **Tablo görselleştirme** Örnek etiketleme aracı artık belgede tanınan tabloları görüntülüyor. Bu özellik, etiketlemeye ve Analiz etmeden önce, belgeden tanınan ve ayıklanan tabloları görüntülemenize olanak sağlar. Bu özellik katmanlar seçeneği kullanılarak değiştirilebilir/kapatılabilir.

  Aşağıdaki görüntü, tabloların nasıl tanındığını ve ayıklanacağını gösteren bir örnektir:

  > [!div class="mx-imgBorder"]
  > ![Örnek etiketleme aracını kullanarak tablo görselleştirme](./media/whats-new/formre-table-viz.png)

    Ayıklanan tablolar altındaki JSON çıktısında kullanılabilir `"pageResults"` .

  > [!IMPORTANT]
  > Tabloların etiketlenmesi desteklenmiyor. Tablolar tanınmazsa ve otomatik olarak yoksa, bunları anahtar/değer çiftleri olarak etiketleyebilir. Tabloları anahtar/değer çiftleri olarak etiketleyerek, her hücreyi benzersiz bir değer olarak etiketleyin.

### <a name="extraction-enhancements"></a>Ayıklama geliştirmeleri

Bu sürüm, ayıklama geliştirmelerini ve doğruluk geliştirmelerini, özellikle de aynı metin satırında birden çok anahtar/değer çiftini etiketleyip ayıklama özelliğini içerir. 
 
### <a name="sample-labeling-tool-is-now-open-source"></a>Örnek etiketleme aracı artık açık kaynaklı

Form tanıyıcı örnek etiketleme aracı artık açık kaynaklı bir proje olarak kullanılabilir. Çözümlerinizi gereksinimlerinize göre tümleştirebilir ve gereksinimlerinizi karşılamak için müşteriye özgü değişiklikler yapabilirsiniz.

Form tanıyıcı örnek etiketleme aracı hakkında daha fazla bilgi için [GitHub](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md)'da bulunan belgeleri gözden geçirin.

### <a name="tls-12-enforcement"></a>TLS 1.2 zorlaması

TLS 1,2, bu hizmete yönelik tüm HTTP istekleri için de zorlanır. Daha fazla bilgi için bkz. Azure bilişsel [Hizmetler güvenliği](../cognitive-services-security.md).

## <a name="january-2020"></a>Ocak 2020

Bu sürümde, tanıyıcı 2,0 (Önizleme) formu tanıtılmıştır. Aşağıdaki bölümlerde yeni özellikler, geliştirmeler ve değişiklikler hakkında daha fazla bilgi bulacaksınız. 

### <a name="new-features"></a>Yeni özellikler

* **Özel model**
  * **Etiketlerle eğitme** Artık el ile etiketlenmiş verileri olan özel bir modeli eğitebilirsiniz. Bu yöntem, daha iyi çalışan modellerle sonuçlanır ve anahtar içermeyen karmaşık formlarla veya formlarla çalışan modeller oluşturabilir.
  * **Zaman uyumsuz API** Zaman uyumsuz API çağrılarını, büyük veri kümelerini ve dosyalarını eğitmek ve analiz etmek için kullanabilirsiniz.
  * **TIFF dosyası desteği** Artık TIFF belgelerinden eğitim alabilir ve verileri ayıklayabilirsiniz.
  * **Ayıklama doğruluk iyileştirmeleri**

* **Önceden oluşturulmuş makbuz modeli**
  * **İpucu tutarları** Artık tıp tutarlarını ve diğer el yazısı değerlerini ayıklayabilirsiniz.
  * **Satır öğesi ayıklama** Makbuzlardan satır öğesi değerlerini ayıklayabilirsiniz.
  * **Güvenirlik değerleri** Her Ayıklanan değer için modelin güvenini görüntüleyebilirsiniz.
  * **Ayıklama doğruluk iyileştirmeleri**

* **Düzen ayıklama** Artık formlarınızın metin verilerini ve tablo verilerini ayıklamak için Düzen API 'sini kullanabilirsiniz.

### <a name="custom-model-api-changes"></a>Özel model API 'SI değişiklikleri

Eğitimin tüm API 'Leri ve özel modelleri kullanma yeniden adlandırıldı ve bazı zaman uyumlu yöntemler artık zaman uyumsuzdur. Önemli değişiklikler aşağıda verilmiştir:

* Bir modele eğitim süreci artık zaman uyumsuzdur. **/Custom/modeller** API çağrısı üzerinden eğitim başlatırsınız. Bu çağrı, eğitim sonuçlarını döndürmek için **özel/modeller/{ModelId}** içine geçirebilmeniz için BIR işlem kimliği döndürür.
* Anahtar/değer ayıklama işlemi artık **/Custom/models/{modelid}/Analyze** API çağrısı tarafından başlatıldı. Bu çağrı, ayıklama sonuçlarını döndürmek için **özel/modeller/{ModelId}/analiz Zeresults/{Resultıd}** içine geçirebilmeniz için BIR işlem kimliği döndürür.
* Eğitme işlemi için işlem kimlikleri, **Işlem konumu** üst bilgisi DEĞIL, http yanıtlarının **konum** üstbilgisinde bulunur.

### <a name="receipt-api-changes"></a>Giriş API 'SI değişiklikleri

Satış alındılarını okumak için API 'Ler yeniden adlandırıldı.

* Alındı verileri ayıklama işlemi artık **/prebuilt/teselt/Analyze** API çağrısı tarafından başlatılır. Bu çağrı, ayıklama sonuçlarını döndürmek için **/prebuilt/pot/analanaliz Zeresults/{resultid}** öğesine geçirebilen BIR işlem kimliği döndürür.

### <a name="output-format-changes"></a>Çıkış biçimi değişiklikleri

Tüm API çağrılarına yönelik JSON yanıtlarının yeni biçimleri vardır. Bazı anahtarlar ve değerler eklenmiştir, kaldırılmıştır veya yeniden adlandırılır. Geçerli JSON biçimlerinin örnekleri için hızlı başlangıçlara bakın.

## <a name="next-steps"></a>Sonraki adımlar

Form tanıyıcıyla seçtiğiniz dilde bir form işleme uygulaması yazmaya başlamak için [hızlı](quickstarts/client-library.md) başlangıcı doldurun.

## <a name="see-also"></a>Ayrıca bkz.

* [Form Tanıma nedir?](./overview.md)
