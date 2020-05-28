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
ms.openlocfilehash: e3bf279142383ccdb3e82ffee49bd593640bb937
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996831"
---
# <a name="whats-new-in-form-recognizer"></a>Form Tanıma'daki yenilikler

Form tanıyıcı hizmeti sürekli olarak güncelleştirilir. Özellik geliştirmeleri, düzeltmeler ve belge güncelleştirmeleriyle güncel kalmak için bu makaleyi kullanın.

> [!NOTE]
> Form tanıyıcı için hızlı başlangıç ve kılavuzlar, belirtilmediği takdirde her zaman API 'nin en son sürümünü kullanır.

## <a name="april-2020"></a>Nisan 2020

### <a name="new-features"></a>Yeni özellikler
* **Form TANıYıCı API v 2.0 genel önizlemesi Için SDK desteği** Bu ay, form tanıyıcı v 2.0 (Önizleme) sürümü için bir önizleme SDK 'Sı eklemek üzere hizmet desteğimizi genişlettik. Seçtiğiniz dille çalışmaya başlamak için aşağıdaki bağlantıları kullanın: 
   * [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/formrecognizer?view=azure-dotnet-preview)
   * [Java SDK](https://docs.microsoft.com/java/api/overview/azure/formrecognizer?view=azure-java-preview)
   * [Python SDK'sı](https://docs.microsoft.com/python/api/overview/azure/formrecognizer?view=azure-python-previewr)
   * [JavaScript SDK'sı](https://docs.microsoft.com/javascript/api/overview/azure/formrecognizer?view=azure-node-preview)

  Yeni SDK, form tanıyıcı için v 2.0 REST API tüm özelliklerini destekler. Örneğin, etiketlere sahip veya olmayan bir modeli eğitebilir ve formlarınızdaki metin, anahtar değer çiftleri ve tablolar çıkarabilir, önceden oluşturulmuş alındılar hizmeti ile alış irsaliyelerinden verileri ayıklayabilir ve belgelerden düzen hizmetiyle birlikte metin ve tabloları ayıklayabilir. [SDK geri bildirim formu aracılığıyla SDK](https://aka.ms/FR_SDK_v1_feedback)'larda geri bildirimlerinizi paylaşabilirsiniz.
 
* **Özel modeli Kopyala** Artık yeni özel model Kopyala özelliğini kullanarak bölgeler ve abonelikler arasında modeller kopyalayabilirsiniz. Özel model Kopyala API 'sini çağırmadan önce, hedef kaynak uç noktasına karşı yetkilendirme kopyalama işlemini çağırarak hedef kaynağa kopyalamak için Yetkilendirmeyi edinmeniz gerekir.
   * [Kopya yetkilendirmesi oluşturma](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/CopyCustomFormModelAuthorization) REST API
   * [Özel bir modeli kopyalama](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/CopyCustomFormModel) REST API 

### <a name="security-improvements"></a>Güvenlik iyileştirmeleri

* Müşteri tarafından yönetilen anahtarlar artık Formtanıyıcı için kullanılabilir. Daha fazla bilgi için bkz. [rest for form tanıyıcı Için veri şifreleme](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/form-recognizer-encryption-of-data-at-rest).
* Azure Active Directory ile Azure kaynaklarına erişim için Yönetilen kimlikler kullanın. Daha fazla bilgi için bkz. [yönetilen kimliklere erişim yetkisi verme](https://docs.microsoft.com/azure/cognitive-services/authentication#authorize-access-to-managed-identities).

## <a name="march-2020"></a>Mart 2020 

### <a name="new-features"></a>Yeni özellikler

* **Etiketleme Için değer türleri** Artık etiketettiğiniz değer türlerini, tanıyıcı örnek etiketleme aracı ile belirtebilirsiniz. Aşağıdaki değer türleri ve Çeşitlemeler Şu anda destekleniyor:
  * `string`
    * Varsayılan, `no-whitespaces` ,`alphanumeric`
  * `number`
    * varsayılanını`currency`
  * `date` 
    * Varsayılan, `dmy` , `mdy` ,`ymd`
  * `time`
  * `integer`

  Bu özelliği nasıl kullanacağınızı öğrenmek için bkz. [örnek etiketleme araç](./quickstarts/label-tool.md#specify-tag-value-types) Kılavuzu.


* **Tablo görselleştirme** Örnek etiketleme aracı artık belgede tanınan tabloları görüntülüyor. Bu, etiketlemeye ve Analiz etmeden önce, belgeden tanınan ve ayıklanan tabloları görüntülemenize olanak sağlar. Bu özellik katmanlar seçeneği kullanılarak değiştirilebilir/kapatılabilir.

  Bu, tabloların nasıl tanındığını ve ayıklanacağını gösteren bir örnektir:

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
  * **Etiketlerle eğitme** Artık el ile etiketlenmiş verileri olan özel bir modeli eğitebilirsiniz. Bu, daha iyi çalışan modellerle sonuçlanır ve anahtar içermeyen karmaşık formlarla veya formlarla çalışan modeller oluşturabilir.
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

[Form tanıyıcı API 'lerini](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)kullanmaya başlamak için [hızlı](quickstarts/curl-train-extract.md) başlangıcı doldurun.