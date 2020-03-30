---
title: Azure Cosmos DB kapasite planlayıcısı kullanarak maliyetleri tahmin edin
description: Azure Cosmos DB kapasite planlayıcısı, iş yükünüz için gereken iş bilgili (RU/s) maliyetini ve maliyetini tahmin etmenizi sağlar. Bu makalede, gerekli iş ve maliyeti tahmin etmek için kapasite planlayıcısının yeni sürümünün nasıl kullanılacağı açıklanmaktadır.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: dech
ms.openlocfilehash: f10ace47f774e31b586f7736f5fb8e5dfea0c948
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68707636"
---
# <a name="estimate-rus-using-the-azure-cosmos-db-capacity-planner"></a>Azure Cosmos DB kapasite planlayıcısı kullanarak RU/ları tahmin edin

Azure Cosmos veritabanlarınızı ve kapsayıcılarınızı doğru miktarda sağlanan iş yükü veya [İstek Birimleri (RU/s)](request-units.md)ile iş yükünüz için yapılandırmak, maliyet ve performansı optimize etmek için gereklidir. Bu makalede, gerekli RU/s ve iş yükünüzün maliyeti hakkında bir tahmin almak için Azure Cosmos DB [kapasite planlayıcısının](https://cosmos.azure.com/capacitycalculator/) nasıl kullanılacağı açıklanmaktadır. 

## <a name="how-to-estimate-throughput-and-cost-with-azure-cosmos-db-capacity-planner"></a>Azure Cosmos DB kapasite planlayıcısı ile iş ve maliyet tahmini

Kapasite planlayıcısı iki modda kullanılabilir.

|**Mod**  |**Açıklama**  |
|---------|---------|
|Temel|Hızlı, üst düzey BIR RU/s ve maliyet tahmini sağlar. Bu mod, dizin oluşturma ilkesi, tutarlılık ve diğer parametreler için varsayılan Azure Cosmos DB ayarlarını varsayar. <br/><br/>Azure Cosmos DB'de çalıştırılabilmek için olası bir iş yükünü değerlendirirken hızlı ve üst düzey bir tahmin için temel modu kullanın.|
|Gelişmiş|Ek ayarları ayarlama olanağıyla daha ayrıntılı bir RU/s ve maliyet tahmini sağlar - dizine dizini, tutarlılık düzeyi ve maliyet ve iş buzunu etkileyen diğer parametreler. <br/><br/>Yeni bir proje için RU/s tahmin ederken veya daha ayrıntılı bir tahmin isterken gelişmiş modu kullanın. |


## <a name="estimate-provisioned-throughput-and-cost-using-basic-mode"></a>Temel modu kullanarak sağlanan iş ve maliyeti tahmin edin
Temel modu kullanarak iş yükünüz için hızlı bir tahmin elde etmek için [kapasite planlayıcısına](https://cosmos.azure.com/capacitycalculator/)gidin. İş yükünüze bağlı olarak aşağıdaki parametreleri girin: 

|**Giriş**  |**Açıklama**  |
|---------|---------|
|Bölge sayısı|Azure Cosmos DB tüm Azure bölgelerinde kullanılabilir. İş yükünüz için gereken bölge sayısını seçin. Herhangi bir sayıda bölgeyi Cosmos hesabınızla ilişkilendirebilirsiniz. Daha fazla ayrıntı için Azure Cosmos DB'deki [genel dağıtıma](distribute-data-globally.md) bakın.|
|Çok bölgeli yazılar|[Çok bölgeli yazmaları](distribute-data-globally.md#key-benefits-of-global-distribution)etkinleştiriyorsanız, uygulamanız herhangi bir Azure bölgesine okuma ve yazma yapabilir. Çok bölgeli yazıları devre dışı ederseniz, uygulamanız tek bir bölgeye veri yazabilir. <br/><br/> Farklı bölgelerde düşük gecikme sonu yazma gerektiren etkin bir iş yükü olmasını bekliyorsanız çok bölgeli yazmaları etkinleştirin. Örneğin, farklı bölgelerdeyüksek hacimlerde veritabanına veri yazan bir IOT iş yükü. <br/><br/> Çok bölgeli yazılar %99,999 okuma ve yazma kullanılabilirliğini garanti eder. Çok bölgeli yazma, tek yazma bölgeleriyle karşılaştırıldığında daha fazla iş elde etme gerektirir. Daha fazla bilgi edinmek [için, RUs'un tek ve birden çok yazma bölgeleri](optimize-cost-regions.md) makalesi için nasıl farklı olduğunu görün.|
|Depolanan toplam veri (bölge başına)|Tek bir bölgede GB olarak depolanan toplam tahmini veri.|
|Öğe boyutu|Veri öğesinin tahmini boyutu (örn. belge), 1 KB ile 2 MB arasında değişmektedir. |
|Bölge başına okuma/sn|Saniyede beklenen okuma sayısı. |
|Bölgeye göre yazma/sn|Saniyede beklenen yazma sayısı. |

Gerekli ayrıntıları doldurduktan sonra **Hesapla'yı**seçin. **Maliyet Tahmini** sekmesi, depolama ve sağlanan iş maliyetinin toplam maliyetini gösterir. Okuma ve yazma istekleri için gereken iş tüttürünün dökümünü almak için bu sekmedeki **Ayrıntıları Göster** bağlantısını genişletebilirsiniz. Herhangi bir alanın değerini her değiştirseniz, tahmini maliyeti yeniden hesaplamak için **Hesapla'yı** seçin. 

![Kapasite planlayıcısı temel modu](./media/estimate-ru-with-capacity-planner/basic-mode.png)

## <a name="estimate-provisioned-throughput-and-cost-using-advanced-mode"></a>Gelişmiş modu kullanarak sağlanan iş ve maliyeti tahmin edin

Gelişmiş mod, RU/s tahminini etkileyen daha fazla ayar sağlamanıza olanak tanır. Bu seçeneği kullanmak için [kapasite planlayıcısına](https://cosmos.azure.com/capacitycalculator/) gidin ve Azure için kullandığınız bir hesapla araçta oturum açın. Oturum açma seçeneği sağ köşede mevcuttur. 

Oturum açmadan sonra, temel moddaki alanlara kıyasla ek alanlar görebilirsiniz. İş yükünüze bağlı olarak ek parametreleri girin. 

|**Giriş**  |**Açıklama**  |
|---------|---------|
|API|Azure Cosmos DB, çok modelli ve çok API hizmetidir. Yeni iş yükleri için SQL (Core) API'yi seçin. |
|Bölge sayısı|Azure Cosmos DB tüm Azure bölgelerinde kullanılabilir. İş yükünüz için gereken bölge sayısını seçin. Herhangi bir sayıda bölgeyi Cosmos hesabınızla ilişkilendirebilirsiniz. Daha fazla ayrıntı için Azure Cosmos DB'deki [genel dağıtıma](distribute-data-globally.md) bakın.|
|Çok bölgeli yazılar|[Çok bölgeli yazmaları](distribute-data-globally.md#key-benefits-of-global-distribution)etkinleştiriyorsanız, uygulamanız herhangi bir Azure bölgesine okuma ve yazma yapabilir. Çok bölgeli yazıları devre dışı ederseniz, uygulamanız tek bir bölgeye veri yazabilir. <br/><br/> Farklı bölgelerde düşük gecikme sonu yazma gerektiren etkin bir iş yükü olmasını bekliyorsanız çok bölgeli yazmaları etkinleştirin. Örneğin, farklı bölgelerdeyüksek hacimlerde veritabanına veri yazan bir IOT iş yükü. <br/><br/> Çok bölgeli yazılar %99,999 okuma ve yazma kullanılabilirliğini garanti eder. Çok bölgeli yazma, tek yazma bölgeleriyle karşılaştırıldığında daha fazla iş elde etme gerektirir. Daha fazla bilgi edinmek [için, RUs'un tek ve birden çok yazma bölgeleri](optimize-cost-regions.md) makalesi için nasıl farklı olduğunu görün.|
|Varsayılan tutarlılık|Azure Cosmos DB, geliştiricilerin tutarlılık, kullanılabilirlik ve gecikme dengeleri arasındaki dengeyi dengelemesine olanak sağlamak için 5 tutarlılık düzeyi destekler. Daha fazla bilgi edinmek için [tutarlılık düzeyleri](consistency-levels.md) makalesine bakın. <br/><br/> Varsayılan olarak, Azure Cosmos DB oturum tutarlılığı kullanır ve bu da oturumda kendi yazılarınızı okuma olanağını garanti eder. <br/><br/> Güçlü veya sınırlı bayatlık seçmek, oturum, tutarlı önek ve nihai tutarlılıkla karşılaştırıldığında okumalar için gerekli RU/s'lerin iki katını gerektirir. Çok günlük yazmaile güçlü tutarlılık desteklenmez ve güçlü tutarlılıkla tek bölgeli yazmalar için otomatik olarak varsayılan olarak varsayılan dır. |
|Dizin oluşturma ilkesi|Varsayılan olarak, Azure Cosmos DB esnek ve verimli sorgular **(Otomatik** dizin oluşturma ilkesine eşler) için tüm öğelerdeki [tüm özellikleri dizinler.](index-policy.md) <br/><br/> **Kapalı**seçerseniz, özelliklerin hiçbiri dizine eklenmez. Bu, yazmalar için en düşük RU ücretiyle sonuçlanır. Yalnızca [nokta okuma](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.container.readitemasync?view=azure-dotnet) (anahtar değer aramaları) ve/veya yazma ve sorgu yapmayı bekliyorsanız ilkeyi **kapatın.** <br/><br/> Özel dizin oluşturma ilkesi, daha düşük yazma iş başına ve depolama için dizinden belirli özellikleri eklemenize veya hariç tutmanıza olanak tanır. Daha fazla bilgi edinmek için [dizin oluşturma ilkesi](index-overview.md) ve [örnek dizini oluşturma ilkeleri makalelerine](how-to-manage-indexing-policy.md#indexing-policy-examples) bakın.|
|Depolanan toplam veri (bölge başına)|Tek bir bölgede GB olarak depolanan toplam tahmini veri.|
|İş yükü modu|İş yükü hacminiz sabitse **Sabit'i** seçin. <br/><br/> İş yükünüz zaman içinde değişirse **Değişken'i** seçin.  Örneğin, belirli bir gün veya bir ay boyunca. <br/><br/> Değişken iş yükü seçeneğini seçerseniz aşağıdaki ayarlar kullanılabilir:<ul><li>En yüksek teki zaman yüzdesi: İş yükünüzün en yüksek (en yüksek) iş hasılatı gerektirdiği bir aydaki zaman yüzdesi. <br/><br/> Örneğin, 09:00 - 18:00 hafta içi iş saatleri boyunca yüksek aktiviteye sahip bir iş yükünüz varsa, o zaman en yüksek teki saat yüzdesi: 45 saat en yüksek / 730 saat / ay = ~%6' dır.<br/><br/></li><li>En yüksek bölge başına okuma/sn - En yüksek te saniyede beklenen okuma sayısı.</li><li>En yüksek bölge başına yazma/sn – En yüksekte saniyede beklenen yazma sayısı.</li><li>Tepedeki bölge başına okuma/sn – En yoğun kapalı yken saniyede beklenen okuma sayısı.</li><li>Tepedeki bölge başına yazma/sn – En yoğun kapalı yken saniyede beklenen yazma sayısı.</li></ul>En yüksek ve yoğun olmayan aralıklarla, sağlanan iş lerinizi buna göre yukarı ve aşağı [programlı olarak ölçeklendirerek maliyetinizi](set-throughput.md#update-throughput-on-a-database-or-a-container) optimize edebilirsiniz.|
|Öğe boyutu|1 KB ile 2 MB arasında değişen veri öğesinin (örn. belge) boyutu. <br/><br/>Ayrıca daha doğru bir tahmin için **örnek (JSON)** belgesi yükleyebilirsiniz.<br/><br/>İş yükünüzde aynı kapsayıcıda birden çok öğe türü (farklı JSON içeriğiyle) varsa, birden çok JSON belgesi yükleyebilir ve tahmini alabilirsiniz. Birden çok örnek JSON belgesi eklemek için **yeni öğe ekle** düğmesini kullanın.|

Geçerli tahmini içeren bir CSV dosyasını indirmek için **Tahmin Kaydet** düğmesini de kullanabilirsiniz. 

![Kapasite planlayıcısı gelişmiş mod](./media/estimate-ru-with-capacity-planner/advanced-mode.png)

Azure Cosmos DB kapasite planlayıcısında gösterilen fiyatlar, genel olarak elde edilen fiyat lama ve depolama oranlarına dayalı tahminlerdir. Tüm fiyatlar ABD doları cinsinden gösterilir. Tüm fiyatları bölgeye göre görmek için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın.  

## <a name="estimating-throughput-for-queries"></a>Sorgular için iş bilgilerini tahmin etme

Azure Cosmos kapasite hesaplayıcısı nokta okumalarını (örneğin belgenin kimlik ve bölüm anahtarı değerine göre okunması) varsayar ve iş yükü için yazar. Sorgular için gereken iş verisini tahmin etmek için, sorgunuzu bir Cosmos kapsayıcısındaki temsili veri kümesinde çalıştırın ve [RU ücretini alın.](find-request-unit-charge.md) Ru ücretini, gereken toplam RU/s'yi almak için saniyede çalıştırmayı beklediğiniz sorgu sayısıyla çarpın. 

Örneğin, iş yükünüz saniyede ``SELECT * FROM c WHERE c.id = 'Alice'`` 100 kez çalıştırılabilen bir sorgu gerektiriyorsa ve sorgunun RU ücreti 10 RUs ise, bu istekleri yerine getirmek için toplamda 100 sorgu / sn * 10 RU / sorgu = 1000 RU/s gerekir. İş yükünüzde olan okuma lar veya yazılar için gerekli olan RU/s'lere bu RU/ları ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'nin fiyatlandırma modeli](how-pricing-works.md)hakkında daha fazla bilgi edinin.
* Yeni bir [Cosmos hesabı, veritabanı ve kapsayıcı](create-cosmosdb-resources-portal.md)oluşturun.
* Sağlanan iş maliyeti maliyetini nasıl [optimize edebilirsiniz](optimize-cost-throughput.md)öğrenin.
* [Ayrılmış kapasiteyle maliyeti](cosmos-db-reserved-capacity.md)nasıl optimize edebilirsiniz öğrenin.

