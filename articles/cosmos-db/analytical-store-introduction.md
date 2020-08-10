---
title: Azure Cosmos DB analitik depo (Önizleme) nedir?
description: Azure Cosmos DB işlemsel (satır tabanlı) ve analitik (sütun tabanlı) depolama hakkında bilgi edinin. Analitik deponun avantajları, büyük ölçekli iş yükleri için performans etkisi ve verileri işlemsel depodan analitik depoya otomatik olarak eşitleme
author: Rodrigossz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: rosouz
ms.openlocfilehash: 3b210ea558f857d017504d07e571e94e34c0d4f6
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88037108"
---
# <a name="what-is-azure-cosmos-db-analytical-store-preview"></a>Azure Cosmos DB analitik depo (Önizleme) nedir?

> [!IMPORTANT]
> Analitik depolama Azure Cosmos DB Şu anda önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Analitik depo Azure Cosmos DB, işlem iş yüklerinize herhangi bir etkisi olmadan, Azure Cosmos DB işletimsel verilere karşı, büyük ölçekli analiz sağlamak için tamamen yalıtılmış bir sütun deposudur.  

## <a name="challenges-with-large-scale-analytics-on-operational-data"></a>İşletimsel verilerde büyük ölçekli analiz sorunları

Azure Cosmos DB kapsayıcısındaki çok modelli işletimsel veriler, dahili olarak sıralı satır tabanlı "işlem deposunda" depolanır. Satır deposu biçimi, milisaniye olarak yanıt süreleriyle ve işletimsel sorgularda hızlı işlem okuma ve yazma işlemleri için tasarlanmıştır. Veri kümeniz büyük büyürse, karmaşık analitik sorgular bu biçimde depolanan veriler üzerinde sağlanan aktarım hızı açısından pahalı olabilir. Sağlanan aktarım hızının yüksek kullanımı, gerçek zamanlı Uygulamalarınız ve hizmetleriniz tarafından kullanılan işlem iş yüklerinin performansını etkiler.

Geleneksel olarak, büyük miktarlarda verileri analiz etmek için, işletimsel veriler Azure Cosmos DB işlem deposundan ayıklanır ve ayrı bir veri katmanında depolanır. Örneğin, veriler bir veri ambarında veya Veri Gölü uygun bir biçimde depolanır. Bu veriler daha sonra büyük ölçekli çözümlemeler için kullanılır ve Apache Spark kümeleri gibi işlem altyapısı kullanılarak çözümlenir. İşlem iş yüklerinizde olası etkiyi en aza indirmek için ETL (Ayıkla, Dönüştür, Yükle) işlem hatları daha az sıklıkta çalıştırıldığından, analitik depolama ve işlem katmanlarının işlemsel verilerden ayrılması ek gecikme süresine neden olur.

ETL ardışık düzenleri, işletimsel verilere yapılan güncelleştirmeleri işlerken yalnızca yeni alınan işletimsel verileri işlemeye kıyasla de karmaşık hale gelir. 

## <a name="column-oriented-analytical-store"></a>Sütun odaklı analitik depo

Azure Cosmos DB analitik depo, geleneksel ETL işlem hatları ile oluşan karmaşıklık ve gecikme süreli zorluklara yöneliktir. Azure Cosmos DB analitik depo, işletimsel verilerinizi otomatik olarak ayrı bir sütun deposuna eşitleyebilir. Sütun deposu biçimi, büyük ölçekli analitik sorguların en iyi duruma getirilmiş bir şekilde gerçekleştirilmesi için uygundur ve bu tür sorguların gecikme süresini artırır.

Azure SYNAPSE bağlantısı 'nı kullanarak artık SYNAPSE Analytics 'ten Azure Cosmos DB analitik depoya doğrudan bağlayarak hiçbir ETL HTAP çözümü oluşturabilirsiniz. İşletimsel verilerinizde neredeyse gerçek zamanlı büyük ölçekli analiz çalıştırmanızı sağlar.

## <a name="analytical-store-details"></a>Analitik depo ayrıntıları

Azure Cosmos DB kapsayıcısında analitik depoyu etkinleştirdiğinizde, yeni bir sütun deposu, kapsayıcıdaki işletimsel verilere göre dahili olarak oluşturulur. Bu sütun deposu, bu kapsayıcı için satır odaklı işlem deposundan ayrı olarak kalıcı hale getirilir. İşletimsel verilerinize ekler, güncelleştirmeler ve silmeler otomatik olarak analitik depola eşitlenir. Verileri eşitlemek için değişiklik akışına veya ETL 'ye ihtiyacınız yoktur.

### <a name="column-store-for-analytical-workloads-on-operational-data"></a>İşletimsel verilerdeki analitik iş yükleri için sütun deposu

Analitik iş yükleri genellikle Seçili alanların toplamaları ve sıralı taramalarını içerir. Verileri sütun birincil sırada depolayarak, analitik mağaza her alan için bir değer grubunun birlikte serileştirilmesine olanak tanır. Bu biçim, belirli alanlar üzerinde istatistikleri taramak veya hesaplamak için gereken ıOPS 'yi azaltır. Büyük veri kümeleri üzerindeki taramaların sorgu yanıt sürelerini önemli ölçüde geliştirir. 

Örneğin, işletimsel tablolarınız aşağıdaki biçimdedir:

:::image type="content" source="./media/analytical-store-introduction/sample-operational-data-table.png" alt-text="Örnek işletimsel tablo" border="false":::

Satır deposu, yukarıdaki verileri diskte satır başına serileştirilmiş biçimde devam ettirir. Bu biçim, "Product1 hakkında bilgi Döndür" gibi daha hızlı işlem okuma, yazma ve işlem sorgularının kullanılmasına olanak tanır. Ancak, veri kümesi büyük olarak büyüdükçe ve karmaşık analitik sorguları verilerde çalıştırmak istiyorsanız, pahalı olabilir. Örneğin, farklı iş birimleri ve aylar arasında ' ekipman ' adlı kategori altında bir ürüne ilişkin satış eğilimleri almak istiyorsanız, karmaşık bir sorgu çalıştırmanız gerekir. Bu veri kümesindeki büyük taramalar, sağlanan aktarım hızı açısından pahalı olabilir ve ayrıca gerçek zamanlı uygulamalarınızı ve hizmetlerinizi destekleyen işlem iş yüklerinin performansını etkileyebilir.

Sütun deposu olan analitik depo, benzer veri alanlarını birlikte serileştirdiğinden ve disk ıOPS 'yi azalttığından bu sorgular için daha uygundur.

Aşağıdaki görüntüde işlem satır deposu ve analitik sütun deposu Azure Cosmos DB gösterilmektedir:

:::image type="content" source="./media/analytical-store-introduction/transactional-analytical-data-stores.png" alt-text="Azure Cosmos DB 'de işlem satır deposu vs analitik sütun deposu" border="false":::

### <a name="decoupled-performance-for-analytical-workloads"></a>Analitik iş yükleri için ayrılmış performans

Analitik depo, işlem deposundan ayrı olduğundan, analitik sorgular nedeniyle işlem iş yüklerinizin performansı üzerinde hiçbir etkisi yoktur.  Analitik depoda, ayrılan ayrı istek birimleri (ru) gerekli değildir.

### <a name="auto-sync"></a>Otomatik eşitleme

Otomatik eşitleme, işlem verileri ekleme, güncelleştirme ve silme işlevinin, 5 dakika içinde neredeyse gerçek zamanlı olarak, işlemsel depodan analitik depoya otomatik olarak eşitlendiği Azure Cosmos DB tam olarak yönetilen özelliğine başvurur.

Otomatik eşitleme özelliği analitik depolarla birlikte aşağıdaki başlıca avantajları sağlar:

#### <a name="scalability--elasticity"></a>Ölçeklenebilirlik & ölçeklenebilirlik

Azure Cosmos DB işlem deposu, yatay bölümleme kullanarak depolama ve aktarım hızını herhangi bir kesinti olmadan ölçeklendirebilir esnek olabilir. İşlemsel depodaki yatay bölümlendirme, verilerin neredeyse gerçek zamanlı olarak analitik depola eşitlendiğinden emin olmak için otomatik eşitlemede ölçeklenebilirlik & ölçeklenebilirlik sağlar. Veri eşitleme, işlem trafiği aktarım hızına bakılmaksızın, 1000 işlem/sn veya 1.000.000 işlem/sn olmasına bakılmaksızın gerçekleşir ve işlem deposunda sağlanan aktarım hızını etkilemez. 

#### <a name="automatically-handle-schema-updates"></a><a id="analytical-schema"></a>Şema güncelleştirmelerini otomatik olarak işle

Azure Cosmos DB işlemsel depolama şeması belirsiz ve şema veya dizin yönetimiyle uğraşmak zorunda kalmadan işlem uygulamalarınızda yineleme yapmanızı sağlar. Bunun aksine, analitik depolamanın, analitik sorgu performansını iyileştirmek için şema Azure Cosmos DB. Otomatik eşitleme özelliği ile, Azure Cosmos DB işlem deposundan en son güncelleştirmeler üzerinde şema çıkarımını yönetir.  Ayrıca, iç içe geçmiş veri türlerini işlemeyi içeren analitik depodaki şema gösterimini de yönetir.

Zaman içinde yeni özelliklerin eklendiği bir şema evrimi varsa, analitik depo otomatik olarak, işlem deposundaki tüm geçmiş şemalar genelinde bir birleştirilmiş şema gösterir.

Azure Cosmos DB ' deki tüm işletimsel veriler analiz için iyi tanımlanmış bir şemaya uyar, daha sonra şema otomatik olarak algılanır ve analitik depoda doğru şekilde gösterilir. Aşağıda tanımlandığı gibi analizler için iyi tanımlanmış şema bazı öğeler tarafından ihlal edilirse, bu, analitik depoya dahil edilmez. Analiz tanımının iyi tanımlanmış şeması nedeniyle senaryolar engellenirse [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)e-posta gönderin.

Analiz için iyi tanımlanmış bir şema aşağıdaki noktalara göre tanımlanmıştır:

* Bir özellik, birden çok öğe arasında her zaman aynı türe sahiptir

  * Örneğin, `{"a":123} {"a": "str"}` `"a"` bazen bir dize ve bazen bir sayı olduğundan, iyi tanımlanmış bir şemaya sahip değildir. 
  
    Bu durumda, analitik depo veri türünü `“a”` `“a”` kapsayıcının kullanım ömrü içinde ilk oluşan öğede veri türü olarak kaydeder. Veri türünün farklı olduğu öğeler `“a”` analitik depoya dahil edilmez.
  
    Bu koşul, null özellikler için uygulanmaz. Örneğin, `{"a":123} {"a":null}` hala iyi tanımlanmış.

* Dizi türleri tek bir tekrarlanmış tür içermelidir

  * Örneğin, `{"a": ["str",12]}` dizi tamsayı ve dize türleri karışımı içerdiğinden iyi tanımlanmış bir şema değildir

* Bir şemanın herhangi bir iç içe geçme düzeyinde en fazla 200 özellik ve en fazla iç içe geçme derinliği 5 olur

  * En üst düzeyde 201 özelliklerine sahip bir öğenin iyi tanımlanmış bir şeması yok.

  * Şemada beşten fazla iç içe geçmiş düzeyi olan bir öğe ayrıca iyi tanımlanmış bir şemaya sahip değildir. Örneğin, `{"level1": {"level2":{"level3":{"level4":{"level5":{"too many":12}}}}}}`

* Özellik adları, büyük/küçük harfe duyarsız bir şekilde karşılaştırıldığında benzersizdir

  * Örneğin, aşağıdaki öğelerin iyi tanımlanmış bir şeması yoktur `{"Name": "fred"} {"name": "john"}` `"Name"` ve `"name"` büyük/küçük harfe duyarsız bir şekilde karşılaştırıldığında aynıdır

### <a name="cost-effective-archival-of-historical-data"></a>Geçmiş verilerin düşük maliyetli Arşivi

Veri katmanlaması, farklı senaryolar için optimize edilmiş depolama altyapıları arasındaki verilerin ayrılmasının anlamına gelir. Böylece uçtan uca veri yığınının genel performansını ve maliyet verimliliğini artırmaktır. Analitik depolarla Azure Cosmos DB artık, işlem deposundan farklı veri düzenleriyle analitik depoya verilerin otomatik olarak katmanlamasını desteklemektedir. Depolama maliyeti ile karşılaştırıldığında, işlemsel depolamayla karşılaştırıldığında analitik depolamayla en iyi duruma getirilmiş bir işlem, geçmiş analizi için işletimsel verilerin çok daha uzun Horizons.

Analitik depo etkinleştirildikten sonra, işlem iş yüklerinin veri bekletme gereksinimlerine bağlı olarak, belirli bir süre sonra işlem deposundan kayıtları otomatik olarak silmek için ' Işlem depolama süresi canlı (Işlem TTL) ' özelliğini yapılandırabilirsiniz. Benzer şekilde, ' etkin analitik depolama süresi (analitik TTL) ', analitik depoda saklanan veri yaşam döngüsünü işlem deposundan bağımsız olarak yönetmenizi sağlar. Analitik depoyu etkinleştirerek ve TTL özelliklerini yapılandırarak, iki depo için veri saklama süresini sorunsuzca düzenleyebilir ve tanımlayabilirsiniz.

### <a name="global-distribution"></a>Genel Dağıtım

Küresel olarak dağıtılmış bir Azure Cosmos DB hesabınız varsa, bir kapsayıcı için analitik depoyu etkinleştirdikten sonra, bu hesabın tüm bölgelerinde kullanılabilir olacaktır.  İşletimsel verilerde yapılan tüm değişiklikler genel olarak tüm bölgelerde çoğaltılır. Analitik sorguları, Azure Cosmos DB verilerinizin en yakın bölgesel kopyasına göre etkili bir şekilde çalıştırabilirsiniz.

### <a name="security"></a>Güvenlik

Analitik depo ile kimlik doğrulaması, belirli bir veritabanı için işlem deposuyla aynıdır. Kimlik doğrulaması için ana veya salt okunurdur anahtarlarını kullanabilirsiniz. Spark Not defterlerindeki Azure Cosmos DB anahtarlarının yapıştırmayı engellemek için SYNAPSE Studio 'daki bağlı hizmetten yararlanabilirsiniz. Bu bağlı hizmete erişim, çalışma alanına erişimi olan herkes tarafından kullanılabilir.

### <a name="support-for-multiple-azure-synapse-analytics-runtimes"></a>Birden çok Azure SYNAPSE Analytics çalışma zamanı desteği

Analitik depo, işlem çalıştırma sürelerine hiçbir bağımlılığı olmadan analitik iş yükleri için ölçeklenebilirlik, esneklik ve performans sağlamak üzere iyileştirilmiştir. Depolama teknolojisi, el ile gerçekleştirilen çalışmalar olmadan analiz iş yüklerinizi iyileştirmek için kendi kendine yönetilir.

Analitik depolama sistemi analitik işlem sisteminden ayrıldıktan sonra, Azure Cosmos DB analitik depodaki veriler Azure SYNAPSE Analytics tarafından desteklenen farklı analiz çalışma zamanları ile aynı anda sorgulanabilir. SYNAPSE Analytics, bugün Apache Spark ve Azure Cosmos DB analitik depolarla SQL sunucusuz destekler.

> [!NOTE]
> Yalnızca SYNAPSE Analytics çalışma zamanını kullanarak analitik depodan okuyabilirsiniz. Verileri işlem deponuza bir hizmet katmanı olarak geri yazabilirsiniz.

## <a name="pricing"></a><a id="analytical-store-pricing"></a>Fiyat

Analitik mağaza, ücretlendirilebilen tüketim tabanlı bir fiyatlandırma modeli izler:

* Depolama: analitik TTL tarafından tanımlanan geçmiş veriler de dahil olmak üzere her ay analitik depoda tutulan verilerin hacmi.

* Analitik yazma işlemleri: işletimsel veri güncelleştirmelerinin tam olarak yönetilen işlem, işlem deposundan analitik depoya eşitlenmesi (otomatik eşitleme)

* Analitik okuma işlemleri: SYNAPSE Analytics Spark ve SQL sunucusuz çalışma süreleriyle analitik depoya karşı gerçekleştirilen okuma işlemleri.

> [!NOTE]
> Azure Cosmos DB analitik mağaza, 30 Ağustos 2020 ' a kadar tüm ücretlerden ücretsiz olarak sunulmaktadır.

Analitik mağaza fiyatlandırması, işlem Mağazası fiyatlandırma modelinden ayrıdır. Analitik depoda sağlanan RUs kavramı yoktur. Analitik mağaza için fiyatlandırma modeliyle ilgili tüm ayrıntılar için bkz. [Azure Cosmos DB fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).

Azure Cosmos DB kapsayıcısında analitik depolamayı etkinleştirmek üzere yüksek düzeyde bir maliyet tahmini almak için, [Azure Cosmos DB kapasite planlayıcısını](https://cosmos.azure.com/capacitycalculator/) kullanabilir ve analitik depolama ve yazma işlemleri maliyetlerinizin bir tahminini alabilirsiniz. Analitik okuma işlemleri, analiz iş yükü özelliklerine bağlıdır, ancak üst düzey bir tahmin olarak, analitik depoda 1 TB 'lık verilerin taranması genellikle 130.000 analitik okuma işlemlerine neden olur ve maliyeti $0,065 ile sonuçlanır.

## <a name="analytical-time-to-live-ttl"></a><a id="analytical-ttl"></a>Analitik yaşam süresi (TTL)

Analiz TTL değeri, bir kapsayıcı için verilerin analiz deponuzda ne kadar süreyle tutulacağını belirtir. 

İşletimsel verileri ekleme, güncelleştirme, silme, işlem TTL yapılandırmasından bağımsız olarak, işlem deposundan otomatik olarak analitik depoya eşitlenir. Bu işletimsel verileri analitik depoda bekletme, aşağıda belirtildiği gibi, kapsayıcı düzeyindeki analitik TTL değeri tarafından denetlenebilir:

Bir kapsayıcıda analitik TTL, özelliği kullanılarak ayarlanır `AnalyticalStoreTimeToLiveInSeconds` :

* Değer "0" olarak ayarlanırsa (veya null olarak ayarlandıysa): analitik depo devre dışıdır ve hiçbir veri işlemsel depodan analitik depoya çoğaltılmaz

* Varsa ve değer "-1" olarak ayarlandıysa, analitik mağaza verilerin işlem deposunda saklanması ne olursa olsun tüm geçmiş verilerini korur. Bu ayar, analitik deponun işletimsel verilerinizin sonsuz bir şekilde bekletildiğini belirtir

* Varsa ve değer bazı pozitif bir sayıya ayarlanırsa "n": öğelerin, işlem deposunda son değiştirilme zamanından sonraki "n" saniye içinde süresi dolacak. Bu ayar, işlemsel depodaki verilerin saklama zamanından bağımsız olarak, işletimsel verilerinizi analitik depoda sınırlı bir süre boyunca saklamak istiyorsanız yararlanılabilir olabilir.

Dikkat edilmesi gereken bazı noktalar:
*   Analitik depo bir analitik TTL değeri ile etkinleştirildikten sonra, daha sonra farklı bir geçerli değere güncelleştirilebilen olabilir. 
*   İşlem TTL 'SI kapsayıcı veya öğe düzeyinde ayarlanırken, analitik TTL yalnızca şu anda kapsayıcı düzeyinde ayarlanabilir.
*   Analitik deposunda, kapsayıcı düzeyinde analitik TTL >= işlem TTL ayarını yaparak, işletimsel verilerinizin daha uzun süre bekletmesini elde edebilirsiniz.
*   Analitik depo, analitik TTL = işlem TTL 'SI ayarlanarak işlem deposunu yansıtmak için yapılabilir.

Daha fazla bilgi için bkz. [bir kapsayıcıda ANALITIK TTL yapılandırma](configure-synapse-link.md#create-analytical-ttl).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Cosmos DB için Azure SYNAPSE bağlantısı](synapse-link.md)

* [Azure Cosmos DB için Azure Synapse Link'i kullanmaya başlama](configure-synapse-link.md)

* [Azure Cosmos DB için Synapse Link hakkında sık sorulan sorular](synapse-link-frequently-asked-questions.md)

* [Azure Cosmos DB için Azure Synapse Link kullanım örnekleri](synapse-link-use-cases.md)
