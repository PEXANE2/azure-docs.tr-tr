---
title: Azure kaynakları - QnA Maker
description: QnA Maker, her biri farklı bir amaca sahip olmak üzere birkaç Azure kaynağı kullanır. Bunların tek tek nasıl kullanıldığını anlamak, doğru fiyatlandırma katmanını planlamanıza ve seçmenize veya fiyatlandırma katmanınızı ne zaman değiştireceğinize bilmenize olanak tanır. Bunların birlikte nasıl kullanıldığını anlamak, sorunları oluştuğunda bulmanızı ve düzeltmenizi sağlar.
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: 581029d2372f7a2ef704dcf02f266b66440aa246
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80873914"
---
# <a name="azure-resources-for-qna-maker"></a>QnA Maker için azure kaynakları

QnA Maker, her biri farklı bir amaca sahip olmak üzere birkaç Azure kaynağı kullanır. Bunların tek tek nasıl kullanıldığını anlamak, doğru fiyatlandırma katmanını planlamanıza ve seçmenize veya fiyatlandırma katmanınızı ne zaman değiştireceğinize bilmenize olanak tanır. _Bunların birlikte_ nasıl kullanıldığını anlamak, sorunları oluştuğunda bulmanızı ve düzeltmenizi sağlar.

## <a name="resource-planning"></a>Kaynak planlama

Prototip aşamasında ilk olarak bir QnA Maker bilgi tabanı geliştirdiğiniz zaman, hem test hem de üretim için tek bir QnA Maker kaynağına sahip olmak yaygındır.

Projenin geliştirme aşamasına geçdiğinizde şunları göz önünde bulundurmalısınız:

* bilgi bankası sisteminizin kaç dil tutacağı
* bilgi tabanınızın kullanılabilir olması için kaç bölgeye ihtiyacınız var/
* Sisteminizin her etki alanında kaç belge tutacağını

Tek bir QnA Maker kaynağının aynı dile, aynı bölgeye ve aynı konu etki alanı birleşimine sahip tüm bilgi tabanlarını tutmasını planlayın.

## <a name="pricing-tier-considerations"></a>Fiyatlandırma katmanı hususları

Genellikle göz önünde bulundurmanız gereken üç parametre vardır:

* **Hizmetten ihtiyacınız olan iş:**
    * İhtiyaçlarınıza göre Uygulama hizmetiniz için uygun [Uygulama Planını](https://azure.microsoft.com/pricing/details/app-service/plans/) seçin. Uygulamayı [büyütebilir](https://docs.microsoft.com/azure/app-service/manage-scale-up) veya küçültebilirsiniz.
    * Bu da Azure **Bilişsel Arama** SKU seçim etkilemeli, [burada](https://docs.microsoft.com/azure/search/search-sku-tier)daha fazla bilgi bakın. Ayrıca, çoğaltmaları ile Bilişsel Arama [kapasitesini](../../../search/search-capacity-planning.md) ayarlamanız gerekebilir.

* **Boyut ve bilgi temellerinin sayısı**: Senaryonuz için uygun [Azure arama SKU'yu](https://azure.microsoft.com/pricing/details/search/) seçin. Genellikle, farklı konu etki alanlarının sayısına bağlı olarak ihtiyacınız olan bilgi tabanlarının sayısına karar verirsiniz. Bir kez konu etki alanı (tek bir dil için) tek bir bilgi tabanında olmalıdır.

    N-1 bilgi tabanlarını, N'nin katmanda izin verilen en yüksek dizinler olduğu belirli bir katmanda yayımlayabilirsiniz. Ayrıca, katman başına izin verilen maksimum boyutu ve belge sayısını da kontrol edin.

    Örneğin, katmanınızda izin verilen 15 dizin varsa, 14 bilgi tabanı (yayımlanmış bilgi tabanı başına 1 dizin) yayımlayabilirsiniz. On beşinci indeks yazma ve test için tüm bilgi üsleri için kullanılır.

* **Kaynak olarak belge sayısı**: QnA Maker yönetim hizmetinin ücretsiz SKU'su, portal ve API'ler aracılığıyla yönetebileceğiniz belge sayısını 3 ile sınırlar (her biri 1 MB boyutunda). Standart SKU'nun yönetebileceğiniz belge sayısıyla sınırı yoktur. [Burada](https://aka.ms/qnamaker-pricing)daha fazla bilgi bakın.

Aşağıdaki tablo size bazı üst düzey yönergeler verir.

|                        | QnA Maker Yönetimi | App Service | Azure Bilişsel Arama | Sınırlamalar                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Deneme        | Ücretsiz SKU             | Ücretsiz Katman   | Ücretsiz Katman    | 2 KB'ye kadar, 50 MB boyutunda yayımla  |
| Geliştirme/Test Ortamı   | Standart SKU         | Paylaşımlı      | Temel        | 14 KB'ye kadar, 2 GB boyutunda yayınlayın    |
| Üretim Ortamı | Standart SKU         | Temel       | Standart     | 49 KB'ye kadar, 25 GB boyutunda yayınlayın |

## <a name="recommended-settings"></a>Önerilen Ayarlar

|Hedef QPS | App Service | Azure Bilişsel Arama |
| -------------------- | ----------- | ------------ |
| 3             | S1, 1 Örnek   | S1, 1 Örnek    |
| 50         | S3, 10 Örnek       | S1, 12 Örnekler         |
| 80         | S3, 10 Örnek      |  S3, 12 Örnek  |
| 100         | P3V2, 10 Örnek  | S3, 12 Örnek, 3 Bölüm   |
| 200 ile 250 arasında         | P3V2, 20 Örnek | S3, 12 Örnek, 3 Bölüm    |

## <a name="when-to-change-a-pricing-tier"></a>Fiyatlandırma katmanıne ne zaman değişiklik yapmak

|Yükseltme|Neden|
|--|--|
|[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku) QnA Maker yönetimi SKU|Bilgi tabanınızda daha fazla QnA çifti veya belge kaynağı olmasını istiyorsunuz.|
|[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-app-service) App Service SKU ve Bilişsel Arama katmanı kontrol edin ve [Bilişsel Arama kopyaları oluşturmak](../../../search/search-capacity-planning.md)|Bilgi tabanınızın, sohbet botu gibi istemci uygulamanızdan daha fazla istek sunması gerekir.|
|[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service) Azure Bilişsel Arama hizmeti|Birçok bilgi tabanına sahip olmayı planlıyorsun.|

[Azure portalında Uygulama Hizmetinizi güncelleyerek](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)en son çalışma zamanı güncellemelerini alın.

## <a name="resource-naming-considerations"></a>Kaynak adlandırma hususları

QnA Maker kaynağının kaynak adı, `qna-westus-f0-b`örneğin, diğer kaynakların adlarını almak için de kullanılır.

Azure portalı oluşturma penceresi, bir QnA Maker kaynağı oluşturmanıza ve diğer kaynakların fiyatlandırma katmanlarını seçmenize olanak tanır.

> [!div class="mx-imgBorder"]
> ![QnA Maker kaynak oluşturma için Azure portalının ekran görüntüsü](../media/concept-azure-resource/create-blade.png)

Kaynaklar oluşturulduktan sonra, karakterleri ada gönderen isteğe bağlı Application Insights kaynağı dışında aynı ada sahiptirler.

> [!div class="mx-imgBorder"]
> ![Azure portal kaynak kaydının ekran görüntüsü](../media/concept-azure-resource/all-azure-resources-created-qnamaker.png)

> [!TIP]
> Bir QnA Maker kaynağı oluşturduğunuzda yeni bir kaynak grubu oluşturun. Bu, kaynak grubuna göre arama yaparken QnA Maker kaynağıyla ilişkili tüm kaynakları görmenizi sağlar.

> [!TIP]
> Kaynak veya kaynak grubu adı içindeki fiyatlandırma katmanlarını belirtmek için bir adlandırma kuralı kullanın. Yeni bir bilgi tabanı oluşturmaktan veya yeni belgeler eklemekten hatalar aldığınızda, Bilişsel Arama fiyatlandırma katmanı sınırı yaygın bir sorundur.

## <a name="resource-purposes"></a>Kaynak amaçları

QnA Maker ile oluşturulan her Azure kaynağının belirli bir amacı vardır:

* QnA Maker kaynağı
* Bilişsel Arama kaynağı
* App Service
* Uygulama Planı Hizmeti
* Uygulama Öngörüleri Hizmeti


### <a name="cognitive-search-resource"></a>Bilişsel Arama kaynağı

[Bilişsel Arama](../../../search/index.yml) kaynağı aşağıdakileri yapmak için kullanılır:

* QnA çiftlerini saklayın
* Çalışma zamanında QnA çiftlerinin ilk sıralamasını (ranker #1) sağlayın

#### <a name="index-usage"></a>Dizin kullanımı

Kaynak, test dizini olarak hareket etmek için bir dizin tutar ve kalan dizinler her biri bir yayımlanmış bilgi tabanıyla ilişkilidir.

15 dizin tutmak için fiyatlı bir kaynak, 14 yayınlanan bilgi üsleri tutar ve bir dizin tüm bilgi tabanları test etmek için kullanılır. Bu test dizini, etkileşimli test bölmesi kullanan bir sorgunun test dizinini kullanmasına, ancak yalnızca belirli bilgi tabanıyla ilişkili belirli bölümden sonuçları döndürecek şekilde bilgi tabanına göre bölümlenir.

#### <a name="language-usage"></a>Dil kullanımı

QnA Maker kaynağında oluşturulan ilk bilgi tabanı, Bilişsel Arama kaynağı ve tüm dizinleri için _tek_ dil kümesini belirlemek için kullanılır. QnA Maker hizmeti için yalnızca bir _dil ayarlayabilirsiniz._

### <a name="qna-maker-resource"></a>QnA Maker kaynağı

QnA Maker kaynağı, çalışma zamanında QnA çiftlerinin doğal dil işleme (NLP) tabanlı ikinci sıralama katmanına (ranker #2) yazma ve yayımlama API'lerine erişim sağlar.

İkinci sıralama, meta veri ve izleme istemleri içerebilen akıllı filtreler uygular.

#### <a name="qna-maker-resource-configuration-settings"></a>QnA Maker kaynak yapılandırma ayarları

[QnA Maker portalında](https://qnamaker.ai)yeni bir bilgi tabanı oluşturduğunuzda, **Dil** ayarı kaynak düzeyinde uygulanan tek ayardır. Kaynak için ilk bilgi tabanını oluştururken dili seçersiniz.

### <a name="app-service-and-app-service-plan"></a>Uygulama hizmeti ve Uygulama servis planı

[Uygulama hizmeti,](../../../app-service/index.yml) istemci uygulamanız tarafından, yayınlanmış bilgi tabanlarına çalışma süresi bitiş noktası üzerinden erişmek için kullanılır.

Yayımlanmış bilgi tabanını sorgulamak için, yayımlanmış tüm bilgi tabanları aynı URL bitiş noktasını kullanır, ancak rota içindeki **bilgi temel kimliğini** belirtin.

`{RuntimeEndpoint}/qnamaker/knowledgebases/{kbId}/generateAnswer`

### <a name="application-insights"></a>Application Insights

[Uygulama Öngörüleri](../../../azure-monitor/app/app-insights-overview.md) sohbet günlükleri ve telemetri toplamak için kullanılır. Hizmetiniz hakkında bilgi almak için ortak [Kusto sorgularını](../how-to/get-analytics-knowledge-base.md) gözden geçirin.

## <a name="share-services-with-qna-maker"></a>Hizmetleri QnA Maker ile paylaşın

QnA Maker birkaç Azure kaynağı oluşturur. Yönetimi azaltmak ve maliyet paylaşımından yararlanmak için, neyi paylaşıp paylaşamayacağınızı anlamak için aşağıdaki tabloyu kullanın:

|Hizmet|Paylaş|Neden|
|--|--|--|
|Bilişsel Hizmetler|X|Tasarım la mümkün değil|
|App Service planı|✔|Bir Uygulama Hizmeti planı için ayrılan sabit disk alanı. Aynı Uygulama Hizmeti planını paylaşan diğer uygulamalar önemli disk alanı kullanıyorsa, QnAMaker Uygulama Hizmeti örneği sorunlarla karşılaşır.|
|App Service|X|Tasarım la mümkün değil|
|Application Insights|✔|Paylaşılabilir|
|Arama hizmeti|✔|1. `testkb` QnAMaker hizmeti için ayrılmış bir addır; başkaları tarafından kullanılamaz.<br>2. Adı `synonym-map` ile eşanlamlı harita QnAMaker hizmeti için ayrılmıştır.<br>3. Yayınlanan bilgi üslerinin sayısı Arama hizmeti katmanı ile sınırlıdır. Ücretsiz dizinler varsa, diğer hizmetler bunları kullanabilir.|

### <a name="using-a-single-cognitive-search-service"></a>Tek bir Bilişsel Arama hizmetini kullanma

Portal üzerinden bir QnA hizmeti ve onun bağımlılıklarını (Arama gibi) oluşturursanız, sizin için bir Arama hizmeti oluşturulur ve QnA Maker hizmetine bağlanır. Bu kaynaklar oluşturulduktan sonra, önceden varolan bir Arama hizmetini kullanmak ve yeni oluşturduğunuz hizmeti kaldırmak için Uygulama Hizmeti ayarını güncelleştirebilirsiniz.

QnA Maker kaynak oluşturma sürecinin bir parçası olarak oluşturulan farklı bir Bilişsel Hizmet kaynağını kullanacak şekilde QnA Maker'ı [nasıl yapılandıracak](../How-To/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) şekilde yapılandırılabildiğinizi öğrenin.

## <a name="management-service-region"></a>Yönetim hizmet bölgesi

QnA Maker'ın yönetim hizmeti yalnızca QnA Maker portalı ve ilk veri işleme için kullanılır. Bu hizmet yalnızca **Batı ABD** bölgesinde kullanılabilir. Bu Batı ABD hizmetinde hiçbir müşteri verisi depolanır.

## <a name="keys-in-qna-maker"></a>QnA Maker'daki Anahtarlar

QnA Maker hizmetiniz iki tür anahtarla ilgilenir: Uygulama hizmetinde barındırılan çalışma süresiyle birlikte kullanılan anahtarları ve **sorgu bitiş noktası anahtarlarını** **yazma.**

**Abonelik anahtarınızı**arıyorsanız, [terminoloji değişti.](#subscription-keys)

API'ler aracılığıyla hizmete istekte bulunmak için bu anahtarları kullanın.

![Anahtar yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

|Adı|Konum|Amaç|
|--|--|--|
|Yazma anahtarı|[Azure portalı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)|Bu anahtarlar [QnA Maker yönetim hizmeti API'lerine](https://go.microsoft.com/fwlink/?linkid=2092179)erişmek için kullanılır. Bu API'ler, bilgi tabanınızdaki soruları ve yanıtları yeniden atamasına ve bilgi tabanınızı yayımlamanıza izin verir. Bu anahtarlar, yeni bir QnA Maker hizmeti oluşturduğunuzda oluşturulur.<br><br>Bu **anahtarları, Anahtarlar** **sayfasındaki Bilişsel Hizmetler** kaynağında bulabilirsiniz.|
|Sorgu bitiş noktası anahtarı|[Soru-Cevap Oluşturma portalı](https://www.qnamaker.ai)|Bu anahtarlar, kullanıcı sorusuna yanıt almak için yayımlanmış bilgi bankası bitiş noktasını sorgulamak için kullanılır. Bu sorgu bitiş noktasını genellikle sohbet botunuzda veya QnA Maker hizmetine bağlanan istemci uygulama kodunda kullanırsınız. Bu anahtarlar, QnA Maker bilgi tabanınızı yayımladığınızda oluşturulur.<br><br>Bu anahtarları **Hizmet ayarları** sayfasında bulun. Açılan menüdeki sayfanın sağ üst kısmındaki kullanıcı menüsünden bu sayfayı bulun.|

### <a name="subscription-keys"></a>Abonelik anahtarları

Yazma terimleri ve sorgu bitiş noktası anahtarı düzeltici terimlerdir. Önceki dönem **abonelik anahtarı**oldu. Abonelik anahtarlarına atıfta bulunan diğer belgeler görürseniz, bunlar yazma ve sorgu bitiş noktası anahtarlarına eşdeğerdir (çalışma zamanında kullanılır).

Hangi anahtarı bulmanız gerektiğini bilmek için anahtarın neye erişebildiğini, bilgi bankası yönetimi veya bilgi bankası sorgusuna erişmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* QnA Maker [bilgi tabanı](knowledge-base.md) hakkında bilgi edinin
* Bilgi [tabanı yaşam döngüsünü](development-lifecycle-knowledge-base.md) anlama
* Hizmet ve bilgi temel [limitlerini](../limits.md) gözden geçirin

