---
title: Azure Cosmos DB'de iş maliyeti nin en iyi duruma ilmesi
description: Bu makalede, Azure Cosmos DB'de depolanan veriler için iş maliyetinin nasıl optimize edileöğretilene açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: c80ab4acd745717e2e68ae7d9dc818594ad1ce9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501474"
---
# <a name="optimize-provisioned-throughput-cost-in-azure-cosmos-db"></a>Azure Cosmos DB’de sağlanan işlem hızını iyileştirme

Azure Cosmos DB, sağlanan iş ortası modelini sunarak her ölçekte öngörülebilir performans sunar. İş işini önceden satın alma veya sağlama, performansınız "gürültülü komşu etkisini" ortadan kaldırıyor. İhtiyacınız olan tam iş miktarını belirtirsiniz ve Azure Cosmos DB, SLA tarafından desteklenen yapılandırılmış iş ortasını garanti eder.

En az 400 RU/sn'lik bir iş bölümüyle başlayabilir ve saniyede on milyonlarca istek veya daha fazla sınayabilirsiniz. Okuma isteği, yazma isteği, sorgu isteği, depolanmış yordamlar gibi Azure Cosmos kapsayıcınıza veya veritabanınıza karşı düzenlediğiniz her isteğ, sağlanan iş ortanızdan düşülen karşılık gelen bir maliyete sahiptir. 400 RU/s'yi sağlar ve 40 RUs'a mal olan bir sorgu yazarsanız, saniyede 10 tür sorgu verebilirsiniz. Bunun ötesindeki herhangi bir istek oranı sınırlı alır ve isteği yeniden denemelisiniz. İstemci sürücüleri kullanıyorsanız, otomatik yeniden deneme mantığını desteklerler.

Veritabanlarında veya kapsayıcılarda verim sağlayabilirsiniz. Her strateji, senaryoya bağlı olarak tasarruf elde etmenize yardımcı olur.

## <a name="optimize-by-provisioning-throughput-at-different-levels"></a>Farklı düzeylerde iş getirerek optimize etme

* Bir veritabanında iş elde etmek için, örneğin bu veritabanındaki koleksiyonlar/tablolar/grafikler gibi tüm kapsayıcılar, yükü temel alarak iş verisini paylaşabilir. Veritabanı düzeyinde ayrılmış iş bölümü, belirli bir kapsayıcı kümesindeki iş yüküne bağlı olarak eşit olmayan olarak paylaşılır.

* Bir kapsayıcıya iş elde etmek için, elde edilen iş, SLA tarafından desteklenen bu kapsayıcı için garanti edilir. Mantıksal bölüm anahtarının seçimi, bir kapsayıcının tüm mantıksal bölümleri arasında yükün eşit olarak dağıtılması için çok önemlidir. Daha fazla ayrıntı için [Bölümleme](partitioning-overview.md) ve [yatay ölçekleme](partition-data.md) makalelerine bakın.

Aşağıda, sağlanan bir iş verme stratejisine karar vermek için bazı yönergeler verilmiştir:

**Aşağıdakiler aşağıdaki takdirde Bir Azure Cosmos veritabanında (bir kapsayıcı kümesi içeren) iş ortası sağlamayı düşünün:**

1. Birkaç düzine Azure Cosmos kapsayıcınız var ve bunların bir kısmı veya tümü arasında iş ortasını paylaşmak istiyorsunuz. 

2. IaaS tarafından barındırılan VM'lerde veya şirket içinde çalışmak üzere tasarlanmış tek kiracılı bir veritabanından (örneğin, NoSQL veya ilişkisel veritabanları) Azure Cosmos DB'ye geçiş yapıyorsunuz. Ve çok sayıda koleksiyon / tablolar / grafikler varsa ve veri modeliherhangi bir değişiklik yapmak istemiyorsanız. Şirket içi bir veritabanından geçiş yaparken veri modelinizi güncelleştirmezseniz Azure Cosmos DB tarafından sunulan bazı avantajlardan ödün vermeniz gerekebileceğini unutmayın. Performans açısından en iyi şekilde elde etmek ve aynı zamanda maliyetler için optimizasyon yapmak için veri modelinize her zaman yeniden erişmeniz önerilir. 

3. İş yükünde beklenmeyen bir artışa maruz kalan veritabanı düzeyinde birleştirilmiş iş yükü sayesinde iş yüklerinde planlanmamış artışları absorbe etmek istiyorsunuz. 

4. Tek tek kapsayıcılar üzerinde belirli bir iş verime ayarlamak yerine, veritabanı içindeki bir dizi kapsayıcı arasında toplam iş elde etmek le ilgileniyorsunuz.

**Şu şekilde yse, tek bir konteynerüzerinde iş elde etme sağlamayı düşünün:**

1. Birkaç Azure Cosmos kapsayıcınız var. Azure Cosmos DB şema-agnostik olduğundan, kapsayıcı heterojen şemalara sahip öğeler içerebilir ve müşterilerin her varlık için bir tane olmak üzere birden çok kapsayıcı türü oluşturmasını gerektirmez. Tek bir kap içine 10-20 kapsayıcılar ayrı gruplandırma mantıklı olup olmadığını dikkate almak her zaman bir seçenektir. Konteynerler için minimum 400 RUs ile, 10-20 konteynırın tamamını bir araya getirerek daha uygun maliyetli olabilir. 

2. Belirli bir kapsayıcıda elde edilen imasını kontrol etmek ve SLA tarafından desteklenen belirli bir kapta garantili iş elde etmek istiyorsunuz.

**Yukarıdaki iki stratejinin bir melezdüşünün:**

1. Daha önce de belirtildiği gibi, Azure Cosmos DB yukarıdaki iki stratejiyi karıştırmanızı ve eşleştirmenize olanak tanır, böylece artık Azure Cosmos veritabanında bazı kapsayıcılar olabilir, bu da veritabanında sağlanan iş ortası yanı sıra aynı veritabanıiçindeki bazı kapsayıcıları paylaşabilir , özel miktarda verilen iş miktarı olabilir. 

2. Her iki veritabanı düzeyi özel iş geleni olan bazı kapsayıcılar ile sağlanan iş haline sahip karma yapılandırma ile gelmek için yukarıdaki stratejileri uygulayabilirsiniz.

Aşağıdaki tabloda gösterildiği gibi, API seçimine bağlı olarak, farklı taneciklerde iş artışı sağlayabilirsiniz.

|API|**Paylaşılan** iş için, yapılandırın |**Özel** iş için, yapılandırın |
|----|----|----|
|SQL API’si|Database|Kapsayıcı|
|MongoDB için Azure Cosmos DB API'si|Database|Koleksiyon|
|Cassandra API’si|Anahtar Alanı|Tablo|
|Gremlin API|Veritabanı hesabı|Graf|
|Tablo API’si|Veritabanı hesabı|Tablo|

Farklı düzeylerde iş yükü sağlayarak, maliyetlerinizi iş yükünüzün özelliklerine göre optimize edebilirsiniz. Daha önce de belirtildiği gibi, programlı ve istediğiniz zaman, tek tek konteyner(ler) veya topluca bir kapsayıcı kümesi arasında sağlanan iş lerinizi artırabilir veya azaltabilirsiniz. İş yükünüz değiştikçe iş yerini elastik olarak ölçeklendirerek, yalnızca yapılandırdığınız iş için ödeme yapmışsınız. Kapsayıcınız veya kapsayıcı kümesiniz birden çok bölgeye dağıtılırsa, kapsayıcıda veya kapsayıcılar kümesinde yapılandırdığınız iş yapımının tüm bölgeler e göre kullanıma sunulması garanti edilir.

## <a name="optimize-with-rate-limiting-your-requests"></a>İsteklerinizi fiyat sınırlayarak optimize etme

Gecikme süresine duyarlı olmayan iş yükleri için, daha az iş ortası sağlayabilir ve gerçek iş ortası sağlanan iş ortasını aştığında uygulamanın oranı sınırlayıcı işlemesine izin verebilirsiniz. Sunucu, isteği (HTTP durum kodu `RequestRateTooLarge` 429) ile önceden sonlar ve kullanıcının isteği yeniden denemeden önce beklemesi gereken milisaniye cinsinden süreyi belirten `x-ms-retry-after-ms` üstbilgidöndürür. 

```html
HTTP Status 429, 
 Status Line: RequestRateTooLarge 
 x-ms-retry-after-ms :100
```

### <a name="retry-logic-in-sdks"></a>SDK'larda mantığı yeniden deneyin 

Yerel SDK'lar (.NET/.NET Core, Java, Node.js ve Python) bu yanıtı dolaylı olarak yakalar, sunucu tarafından belirtilen yeniden deneme üstbilgisini karşılar ve isteği yeniden deneyin. Hesabınıza aynı anda birden çok istemci erişilmediği sürece, bir sonraki yeniden deneme başarılı olur.

İstem oranının üzerinde sürekli olarak çalışan birden fazla istemciniz varsa, şu anda 9 olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli olmayabilir. Bu gibi durumlarda, istemci `RequestRateTooLargeException` uygulamaya durum kodu 429 ile bir atar. Varsayılan yeniden deneme sayısı, ConnectionPolicy `RetryOptions` örneğini ayarlayarak değiştirilebilir. Varsayılan olarak, `RequestRateTooLargeException` istek istek oranının üzerinde çalışmaya devam ederse, durum kodu 429 30 saniyelik bir toplu bekleme süresinden sonra döndürülür. Bu, geçerli yeniden deneme sayısı en yüksek yeniden deneme sayısından daha az olsa bile, varsayılan 9 veya kullanıcı tanımlı değer olsun oluşur. 

[MaxRetryTriesOnThrottledRequests](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.retryoptions.maxretryattemptsonthrottledrequests?view=azure-dotnet) 3 olarak ayarlanır, bu nedenle bu durumda, bir istek işlemi konteyner için ayrılmış iş buzunu aşarak sınırlı oran ise, istek işlemi uygulamaya istisna atmadan önce üç kez yeniden çalışır. [MaxRetryWaitTimeInSeconds](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.retryoptions.maxretrywaittimeinseconds?view=azure-dotnet#Microsoft_Azure_Documents_Client_RetryOptions_MaxRetryWaitTimeInSeconds) 60 olarak ayarlanır, bu nedenle bu durumda ilk istek 60 saniyeyi aştığından bu yana kümülatif yeniden deneme bekleme süresi 60 saniyeyi aşarsa, özel durum atılır.

```csharp
ConnectionPolicy connectionPolicy = new ConnectionPolicy(); 
connectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 3; 
connectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 60;
```

## <a name="partitioning-strategy-and-provisioned-throughput-costs"></a>Bölümleme stratejisi ve sağlanan işleme hızı maliyetleri

Azure Cosmos DB'deki maliyetleri optimize etmek için iyi bölümleme stratejisi önemlidir. Depolama ölçümleri aracılığıyla açığa çıkan bölümlerin eğriltilmesini sağlayın. İş metriği ile açığa çıkan bir bölüm için iş elde edilen iş parçası olmadığından emin olun. Belirli bölüm tuşlarına doğru eğrilik olmadığından emin olun. Depolamadaki baskın anahtarlar ölçümler aracılığıyla açıklanır, ancak anahtar uygulama erişim deseninize bağlıdır. En iyisi doğru mantıksal bölüm anahtarı hakkında düşünmek. İyi bir bölüm anahtarının aşağıdaki özelliklere sahip olması beklenir:

* İş yükünü tüm bölümlere eşit olarak ve zaman içinde eşit olarak yayan bir bölüm anahtarı seçin. Başka bir deyişle, verilerin çoğunluğu yla bazı anahtarlara ve daha az veya hiç veri olmayan bazı anahtarlara sahip olmamalıdır. 

* Erişim desenlerinin mantıksal bölümlere eşit olarak yayılmasını sağlayan bir bölüm anahtarı seçin. İş yükü tüm tuşlara göre bile makul. Başka bir deyişle, iş yükünün çoğu birkaç belirli anahtara odaklanmamalıdır. 

* Çok çeşitli değerlere sahip bir bölüm anahtarı seçin. 

Temel fikir, veri depolama ve iş elde etme kaynaklarının mantıksal bölümler arasında dağıtılaabilmesi için, kapsayıcınızdaki verileri ve etkinliği mantıksal bölümler kümesine yaymaktır. Bölüm anahtarları için adaylar sorgularınıza filtre olarak sık sık görünen özellikleri içerebilir. Sorgular, filtre yüklenine bölüm anahtarı nı ekleyerek verimli bir şekilde yönlendirilebilir. Böyle bir bölümleme stratejisi ile, sağlanan iş tükenen en iyi duruma çok daha kolay olacaktır. 

### <a name="design-smaller-items-for-higher-throughput"></a>Daha yüksek iş elde etmek için daha küçük öğeler tasarlama 

Belirli bir işlemin istek ücreti veya istek işleme maliyeti, maddenin boyutuyla doğrudan ilişkilidir. Büyük maddelerüzerindeki işlemler, daha küçük maddelerdeki işlemlerden daha pahalıdır. 

## <a name="data-access-patterns"></a>Veri erişim desenleri 

Verilerinize ne sıklıkta eriştettiğinizize bağlı olarak verilerinizi mantıksal kategorilere ayırmak her zaman iyi bir uygulamadır. Sıcak, orta veya soğuk veri olarak sınıflandırarak tüketilen depolama ve gerekli iş için ince ayar yapabilirsiniz. Erişim sıklığına bağlı olarak, verileri ayrı kapsayıcılara (örneğin tablolar, grafikler ve koleksiyonlar) yerleyebilir ve bu veri kesiminin gereksinimlerini karşılamak için bu veriler üzerinde sağlanan iş ortası üzerinde ince ayar yapabilirsiniz. 

Ayrıca, Azure Cosmos DB kullanıyorsanız ve belirli veri değerlerine göre arama yapmamanızı veya bunlara nadiren erişmeyeceğinizi biliyorsanız, bu özniteliklerin sıkıştırılmış değerlerini depolamanız gerekir. Bu yöntemle depolama alanından, dizin alanından ve sağlanan üretim den tasarruf edin ve daha düşük maliyetlere neden oluyorsunuz.

## <a name="optimize-by-changing-indexing-policy"></a>Dizin oluşturma ilkesini değiştirerek optimize etme 

Varsayılan olarak, Azure Cosmos DB her kaydın tüm özelliğini otomatik olarak dizinler. Bu, geliştirmeyi kolaylaştırmak ve birçok farklı özel sorgu türünde mükemmel performans sağlamak için tasarlanmıştır. Binlerce özelliği olan büyük kayıtlarınız varsa, her özelliği dizine ekleme için iş maliyeti ödemek yararlı olmayabilir, özellikle de bu özelliklerden yalnızca 10 veya 20'sini sorgulıyorsanız. Belirli iş yükünüzü ele almaya yaklaştıkça, rehberimiz dizin ilkenizi ayarlamaktır. Azure Cosmos DB dizin oluşturma ilkesiyle ilgili tüm ayrıntılara [buradan](indexing-policies.md)buradan bulabilirsiniz. 

## <a name="monitoring-provisioned-and-consumed-throughput"></a>Sağlanan ve tüketilen iş bilgili ve tüketilen iş bilgili 

Azure portalında tükettiğiniz RUs sayısını, oranı sınırlı istek sayısını ve tükettiğiniz RUs sayısını izleyebilirsiniz. Aşağıdaki resim bir örnek kullanım ölçüsü gösterir:

![Azure portalındaki istek birimlerini izleme](./media/optimize-cost-throughput/monitoring.png)

Ayrıca, oran sınırlı istek sayısının belirli bir eşiği aşıp aşmadığına işaret etmek için uyarılar ayarlayabilirsiniz. Daha fazla ayrıntı için [Azure Cosmos DB makalesini nasıl izleyebilirsiniz.](use-metrics.md) Bu uyarılar, hesap yöneticilerine e-posta gönderebilir veya sağlanan iş buzun otomatik olarak artırılması için özel bir HTTP Webhook veya Azure İşi'ni arayabilir. 

## <a name="scale-your-throughput-elastically-and-on-demand"></a>İş inizi elastik ve isteğe bağlı ölçeklendirin 

Sağlanan iş için faturalandırıldığından, sağlanan iş meçlanın gereksinimlerinize eşleştirilmesi, kullanılmayan iş meçla nın ücretlerini önlemenize yardımcı olabilir. Sağlanan çıktınızı gerektiği gibi istediğiniz zaman yukarı veya aşağı ölçeklendirebilirsiniz. İş metodlarınız çok tahmin edilebilirse, Azure İşlevlerini kullanabilir ve [zamanlamadaki iş bilgililiği artırmak veya azaltmak](scale-on-schedule.md)için Bir Zamanlayıcı Tetikleyicisi kullanabilirsiniz. 

* RUs'larınızın tüketimini ve oran sınırlı isteklerinin oranını izlemek, gün veya hafta boyunca sabit tutmanız gerekmediğini ortaya çıkarabilir. Gece veya hafta sonu daha az trafik alabilirsiniz. Azure portalı veya Azure Cosmos DB yerel SDK'ları veya REST API'lerini kullanarak, sağlanan iş lerinizi istediğiniz zaman ölçeklendirebilirsiniz. Azure Cosmos DB'nin REST API'si, kapsayıcılarınızın performans düzeyini programlı olarak güncelleştirmek için uç noktalar sağlar ve kodunuzdan gelen iş kısmını günün saatine veya haftanın gününe bağlı olarak ayarlamayı kolaylaştırır. İşlem herhangi bir kapalı kalma süresi olmadan gerçekleştirilir ve genellikle bir dakikadan kısa bir sürede etkili olur. 

* Veri alanını ölçeklendirmeniz gereken alanlardan biri, örneğin veri geçişi sırasında Verileri Azure Cosmos DB'ye yuttuğunuz zamandır. Geçişi tamamladıktan sonra, çözümün sabit durumunu işlemek için sağlanan iş hacmini ölçeklendirebilirsiniz.  

* Unutmayın, faturalandırma bir saatlik ayrıntılıdır, bu nedenle sağlanan iş lerinizi bir seferde bir saatten daha sık değiştirirseniz para dan tasarruf etmezsiniz.

## <a name="determine-the-throughput-needed-for-a-new-workload"></a>Yeni bir iş yükü için gereken iş yükünü belirleme 

Yeni bir iş yükü için sağlanan iş yükünü belirlemek için aşağıdaki adımları kullanabilirsiniz: 

1. Kapasite planlayıcısını kullanarak ilk ve zorlu bir değerlendirme gerçekleştirin ve Azure portalındaki Azure Cosmos Explorer yardımıyla tahminlerinizi ayarlayın. 

2. Beklenenden daha yüksek iş elde edilen kaplar oluşturmak ve gerektiğinde küçültülmek önerilir. 

3. İstekler fiyat sınırlı olduğunda otomatik yeniden denemelerden yararlanmak için yerel Azure Cosmos DB SDK'lardan birini kullanmanız önerilir. Desteklenmeyen bir platform üzerinde çalışıyorsanız ve Cosmos DB'nin REST API'sini kullanıyorsanız, `x-ms-retry-after-ms` üstbilgikullanarak kendi yeniden deneme ilkenizi uygulayın. 

4. Tüm yeniden denemeler başarısız olduğunda uygulama kodunuzdan servis talebinin servis talebine zarif bir şekilde destek lendiğinden emin olun. 

5. Hız sınırlayıcı bildirimler almak için Azure portalındaki uyarıları yapılandırabilirsiniz. Son 15 dakika içinde 10 oran sınırlı istek gibi tutucu limitlerle başlayabilir ve gerçek tüketiminizi anladıktan sonra daha istekli kurallara geçebilirsiniz. Ara sıra fiyat limitleri iyidir, belirlediğiniz limitlerle oynadığınızı gösterirler ve tam olarak yapmak istediğiniz de budur. 

6. Trafik düzeninizi anlamak için izlemeyi kullanın, böylece iş lerinizi gün veya bir hafta boyunca dinamik olarak ayarlama gereksinimini göz önünde bulundurabilirsiniz. 

7. Gerekli sayıdan fazla kapsayıcı ve veritabanları sağlamadığınızdan emin olmak için, sağlanan ve tüketilen iş verme oranınızı düzenli olarak izleyin. Verilen iş bilgili bir bit olması iyi bir güvenlik kontrolüdür.  

### <a name="best-practices-to-optimize-provisioned-throughput"></a>Sağlanan iş akışını optimize etmek için en iyi uygulamalar 

Aşağıdaki adımlar, Azure Cosmos DB kullanırken çözümlerinizi yüksek ölçeklenebilir ve uygun maliyetli hale getirmenize yardımcı olur.  

1. Kapsayıcılar ve veritabanları arasında sağlanan iş verisinin üzerinde önemli ölçüde fazla sınanmışsanız, RUs'un karşılık verilen Vs tüketilen RUs'larını gözden geçirmeli ve iş yüklerinde ince ayar yapmalısın.  

2. Uygulamanız tarafından gerekli olan ayrılmış iş miktarını tahmin etmek için kullanılan yöntemlerden biri, uygulamanız tarafından kullanılan temsili bir Azure Cosmos kapsayıcısına veya veritabanına karşı tipik işlemlerin yürütülmesiyle ilişkili istek birimi RU ücretini kaydetmek ve ardından, her saniye gerçekleştirmeyi beklediğiniz işlem sayısını tahmin edin. Tipik sorguları ve bunların kullanımını da ölçtüğünden ve içerdiğinden emin olun. Sorguların RU maliyetlerini programlı olarak veya portalı kullanarak nasıl tahmin [edileceğini](../synapse-analytics/sql-data-warehouse/backup-and-restore.md)öğrenmek için bkz. 

3. RUs'ta işlem ve maliyetlerini elde etmenin bir diğer yolu da, size işlemin/sürenin ve istek ücretinin dökümünü sağlayacak olan Azure Monitor günlüklerini etkinleştirmektir. Azure Cosmos DB her işlem için istek ücreti sağlar, böylece her işlem ücreti yanıttan geri depolanabilir ve daha sonra analiz için kullanılabilir. 

4. İş yükü ihtiyaçlarınızı karşılamak için ihtiyaç duyduğunuzda, sağlanan iş yükünü elastik olarak yukarı ve aşağı ölçeklendirebilirsiniz. 

5. Azure Cosmos hesabınızla ilişkili bölgeleri ihtiyacınız olduğu şekilde ekleyebilir ve kaldırabilirsiniz ve maliyetleri kontrol edebilirsiniz. 

6. Kapsayıcılarınızın mantıksal bölümleri arasında veri ve iş yüklerinin eşit olarak dağıtıladığından emin olun. Eşit olmayan bölüm dağıtımVar, bu gerekli değerden daha yüksek miktarda iş elde etme sağlanmasına neden olabilir. Çarpık bir dağıtıma sahip olduğunuzu belirlerseniz, iş yükünü bölümler arasında eşit olarak dağıtmanızı veya verileri yeniden bölmenizi öneririz. 

7. Çok sayıda kapsayıcınız varsa ve bu kapsayıcılar SLA'lar gerektirmiyorsa, her kap iş ortası SLA'larının geçerli olmadığı durumlar için veritabanı tabanlı teklifi kullanabilirsiniz. Veritabanı düzeyi iş hacmi teklifine geçirmek istediğiniz Azure Cosmos kapsayıcılarından hangisini belirlemeli ve bunları değişiklik akışı tabanlı bir çözüm kullanarak geçirmelisiniz. 

8. "Cosmos DB Free Tier" (bir yıl için ücretsiz), Cosmos DB (en fazla üç bölgeye kadar) veya dev/test senaryoları için indirilebilir Cosmos DB emülatörü kullanmayı düşünün. Test-dev için bu seçenekleri kullanarak maliyetlerinizi önemli ölçüde düşürebilirsiniz.  

9. İş yüküne özel maliyet optimizasyonları (örneğin, toplu iş boyutunu artırma, birden çok bölgede yük dengeleme okumaları ve varsa verileri çoğaltma) daha da gerçekleştirebilirsiniz.

10. Azure Cosmos DB rezerve etme kapasitesi yle, üç yıl boyunca %65'e varan önemli indirimlerden yararlanabilirsiniz. Azure Cosmos DB ayrılmış kapasite modeli, zaman içinde ihtiyaç duyulan istek birimleri için önceden bir taahhüttür. İndirimler, daha uzun bir süre boyunca ne kadar çok istek birimi kullanırsanız, indiriminiz o kadar fazla olacak şekilde katmanlanır. Bu indirimler hemen uygulanır. Verilen değerlerin üzerinde kullanılan tüm RUs'lar, rezerve edilmeyen kapasite maliyetine göre ücretlendirilir. Daha fazla bilgi için [Cosmos DB ayrılmış kapasite)](cosmos-db-reserved-capacity.md)bakın. Sağlanan iş maliyeti maliyetlerinizi daha da düşürmek için ayrılmış kapasite satın almayı düşünün.  

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makalelerle Azure Cosmos DB'de maliyet optimizasyonu hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin

