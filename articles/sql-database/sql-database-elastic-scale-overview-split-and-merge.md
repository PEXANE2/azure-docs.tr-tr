---
title: Ölçeği genişletilen bulut veritabanları arasında veri taşıma
description: Elastik veritabanı API'lerini kullanarak parçaların nasıl işlenebildiğini ve verilerin kendi barındırılan bir hizmet aracılığıyla nasıl taşınırak değiştirilebildiğini açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: c7eb1670ee911895bdba23921845b8795f4998af
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811310"
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Ölçeği genişletilen bulut veritabanları arasında veri taşıma

Hizmet geliştiricisi olarak bir Yazılım sanız ve aniden uygulamanız muazzam bir talep görüyorsa, büyümeyi karşılamanız gerekir. Böylece daha fazla veritabanları (kırıklar) ekleyin. Veri bütünlüğünü bozmadan verileri yeni veritabanlarına nasıl yeniden dağıtabilirsiniz? Verileri kısıtlı veritabanlarından yeni veritabanlarına taşımak için **bölme birleştirme aracını** kullanın.  

Bölme birleştirme aracı Azure web hizmeti olarak çalışır. Bir yönetici veya geliştirici, parçaları (parçadaki verileri) farklı veritabanları (kırıklar) arasında taşımak için aracı kullanır. Araç, hizmet meta veri veritabanını korumak ve tutarlı eşlemeler sağlamak için basılı harita yönetimini kullanır.

![Genel Bakış][1]

## <a name="download"></a>İndirme

[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Belgeler

1. [Elastik veritabanı Split-Merge aracı öğretici](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Birleştirme güvenlik yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Birleştirme güvenlik konuları](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Parça eşleme yönetimi](sql-database-elastic-scale-shard-map-management.md)
5. [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Elastik veritabanı araçları](sql-database-elastic-scale-introduction.md)
7. [Elastik Veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Bölme birleştirme aracını neden kullanın?

- **Esneklik**

  Uygulamaların tek bir Azure SQL DB veritabanının sınırlarının ötesine esnek bir şekilde uzanması gerekir. Bütünlüğü korurken verileri gerektiği gibi yeni veritabanlarına taşımak için aracı kullanın.

- **Büyümek için bölün**

  Patlayıcı büyümeyi işlemek için genel kapasiteyi artırmak için, verileri parçalayarak ve kapasite gereksinimleri karşılanana kadar kademeli olarak daha fazla veritabanlarına dağıtarak ek kapasite oluşturun. Bu, **bölme** özelliğinin önemli bir örneğidir.

- **Küçültmek için birleştirme**

  Bir işletmenin mevsimsel yapısı nedeniyle kapasitenin azalması gerekir. Araç, iş yavaşladığında daha az ölçek birimine ölçeklendirmenizi sağlar. Elastik Ölçek bölme birleştirme Hizmeti'ndeki 'birleştirme' özelliği bu gereksinimi kapsar.

- **Parçaları hareket ettirerek etkin noktaları yönetme**

  Veritabanı başına birden çok kiracı ile, parçaların parçaların tahsisi bazı kırıklarda kapasite darboğazlarına yol açabilir. Bu, parçaların yeniden tahsisini veya meşgul parçaların yeni veya daha az kullanılan parçalarına taşınmasını gerektirir.

## <a name="concepts--key-features"></a>Kavramlar & temel özellikler

- **Müşteri tarafından barındırılan hizmetler**

  Bölme birleştirme, müşteri tarafından barındırılan bir hizmet olarak teslim edilir. Hizmeti Microsoft Azure aboneliğinizde dağıtmanız ve barındırmanız gerekir. NuGet'den indirdiğiniz paket, belirli dağıtımınıza ait bilgilerle tamamlanması gereken bir yapılandırma şablonu içerir. Ayrıntılar için [bölme birleştirme öğreticisine](sql-database-elastic-scale-configure-deploy-split-and-merge.md) bakın. Hizmet Azure aboneliğinizde çalıştığından, hizmetin güvenlik yönlerini n için denetleyebilir ve yapılandırabilirsiniz. Varsayılan şablon, TLS yapılandırma, sertifika tabanlı istemci kimlik doğrulaması, depolanan kimlik bilgileri için şifreleme, DoS koruma ve IP kısıtlamaları yapılandırma seçeneklerini içerir. Aşağıdaki belge [bölme birleştirme güvenlik yapılandırmasında](sql-database-elastic-scale-split-merge-security-configuration.md)güvenlik yönleri hakkında daha fazla bilgi bulabilirsiniz.

  Varsayılan dağıtılan hizmet, bir alt ve bir web rolüyle çalışır. Her biri Azure Bulut Hizmetleri'nde A1 VM boyutunu kullanır. Paketi dağıtırken bu ayarları değiştiremeseniz de, çalışan bulut hizmetinde (Azure portalı üzerinden) başarılı bir dağıtımdan sonra bunları değiştirebilirsiniz. Alt rolün teknik nedenlerle tek bir örnekten fazla yapılandırılmaması gerektiğini unutmayın.

- **Shard harita entegrasyonu**

  Bölme birleştirme hizmeti, uygulamanın parça haritasıyla etkileşime girerek çalışır. Aralıkları bölmek veya birleştirmek veya parçaları kırıklar arasında taşımak için bölme birleştirme hizmetini kullanırken, hizmet parçalı haritayı otomatik olarak güncel tutar. Bunu yapmak için, hizmet uygulamanın parça harita yöneticisi veritabanına bağlanır ve aralıkları ve eşlemeleri bölme/birleştirme/taşıma istekleri ilerleme olarak tutar. Bu, bölme birleştirme işlemleri devam ederken parçalı haritanın her zaman güncel bir görünüm sunmasını sağlar. Bölme, birleştirme ve parça hareketi işlemleri, kaynak parçasından hedef parçasına bir yığın parça taşınarak uygulanır. Shardlet hareketi işlemi sırasında, geçerli toplu iş partisine tabi olan parçaların sabit haritada çevrimdışı olarak işaretlenir ve **OpenConnectionForKey** API'sını kullanarak veriye bağımlı yönlendirme bağlantıları için kullanılamaz.

- **Tutarlı parça bağlantıları**

  Yeni bir parça grubu için veri hareketi başladığında, parçayı depolayan parçaya veri bağımlı yönlendirme bağlantıları sağlandığında ve parçalayıcı harita API'lerinden parçaparçaya sonraki bağlantılar engellenirken, veri hareketi tutarsızlıkları önlemek için devam etmektedir. Aynı parçadaki diğer parçaların bağlantıları da öldürülecek, ancak yeniden denemede hemen başarılı olacaktır. Toplu iş taşındıktan sonra, parçaparçaları hedef parça için yeniden çevrimiçi olarak işaretlenir ve kaynak veriler kaynak parçadan kaldırılır. Hizmet, tüm parçaları taşınana kadar her toplu iş için bu adımlardan geçer. Bu, tam bölme/birleştirme/taşıma işlemi sırasında birkaç bağlantı öldürme işlemine yol açar.  

- **Shardlet kullanılabilirliğini yönetme**

  Bağlantı öldürmenin yukarıda tartışıldığı gibi mevcut parça grubuyla sınırlandırılması, aynı anda bir yığın parçayla kullanılamamasının kapsamını kısıtlar. Bu, bölme veya birleştirme işlemi sırasında tüm parçaların çevrimdışı kalacağı bir yaklaşım yerine tercih edilir. Bir seferde hareket etmek için farklı parçaların sayısı olarak tanımlanan bir toplu iş boyutu, bir yapılandırma parametresi. Uygulamanın kullanılabilirlik ve performans gereksinimlerine bağlı olarak her bölme ve birleştirme işlemi için tanımlanabilir. Parça eşlelemi içinde kilitli olan aralığın belirtilen toplu iş boyutundan daha büyük olabileceğini unutmayın. Bunun nedeni, hizmetin, verilerdeki parçalama anahtar değerlerinin gerçek sayısının toplu iş boyutuyla yaklaşık olarak eşleşebilecek şekilde aralık boyutunu seçmesidir. Bu seyrek nüfuslu parçalama tuşları için özellikle hatırlamak önemlidir.

- **Meta veri depolama**

  Bölme birleştirme hizmeti, durumunu korumak ve istek işleme sırasında günlükleri tutmak için bir veritabanı kullanır. Kullanıcı bu veritabanını aboneliğinde oluşturur ve hizmet dağıtımı için yapılandırma dosyasında bunun için bağlantı dizesi sağlar. Kullanıcının kuruluşundaki yöneticiler, istek ilerlemesini gözden geçirmek ve olası hatalarla ilgili ayrıntılı bilgileri araştırmak için de bu veritabanına bağlanabilir.

- **Sharding-farkındalık**

  Bölme birleştirme hizmeti (1) ayrılmış tablolar, (2) başvuru tabloları ve (3) normal tablolar arasında ayrım lar. Bir bölme/birleştirme/taşıma işleminin anlambilimi kullanılan tablonun türüne bağlıdır ve aşağıdaki gibi tanımlanır:

  - **Kırık tablolar**

    Bölme, birleştirme ve taşıma işlemleri, parçaları kaynaktan hedef parçasına taşır. Genel isteğin başarıyla tamamlanmasından sonra, bu parçalar artık kaynakta bulunmaz. Hedef tabloların hedef parçaüzerinde bulunması gerektiğini ve işlem den önce hedef aralıktaki verileri içermemesi gerektiğini unutmayın.

  - **Başvuru tabloları**

    Başvuru tabloları için, bölme, birleştirme ve taşıma işlemleri verileri kaynaktan hedef parçaya kopyalar. Ancak, hedefteki bu tabloda zaten herhangi bir satır varsa, belirli bir tablo için hedef parçada herhangi bir değişiklik oluşmadığını unutmayın. İşlenmesi için herhangi bir başvuru tablosu kopyalama işlemi için tablonun boş olması gerekir.

  - **Diğer Tablolar**

    Diğer tablolar, bölme ve birleştirme işleminin kaynağında veya hedefinde bulunabilir. Bölme birleştirme hizmeti, herhangi bir veri hareketi veya kopyalama işlemleri için bu tabloları yok sayıyor. Ancak, kısıtlamalar durumunda bu işlemleri engelleyebilir unutmayın.

    Başvuru ve kırık tablolardaki bilgiler, parçalı `SchemaInfo` haritadaki API'ler tarafından sağlanır. Aşağıdaki örnek, bu API'lerin belirli bir parça harita yöneticisi nesnesi üzerinde kullanımını göstermektedir:

    ```csharp
    // Create the schema annotations
    SchemaInfo schemaInfo = new SchemaInfo();

    // reference tables
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region"));
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation"));

    // sharded tables
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY"));
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY"));

    // publish
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo);
    ```

    'Bölge' ve 'ulus' tabloları başvuru tabloları olarak tanımlanır ve bölme/birleştirme/taşıma işlemleriyle kopyalanır. 'müşteri' ve 'siparişler' sırayla kırık tablolar olarak tanımlanır. `C_CUSTKEY`ve `O_CUSTKEY` parçalama anahtarı olarak hizmet vermektedir.

- **Başvuru Bütünlüğü**

  Bölme birleştirme hizmeti tablolar arasındaki bağımlılıkları analiz eder ve başvuru tablolarını ve parçaları taşımak için işlemleri sahnelemek için yabancı anahtar birincil anahtar ilişkilerini kullanır. Genel olarak, başvuru tabloları önce bağımlılık sırasına göre kopyalanır, sonra her toplu iş içindeki bağımlılıkları sırasına göre basılır. Bu, yeni veriler geldiğinde hedef parçadaki FK-PK kısıtlamalarının yerine getirilen olması için gereklidir.

- **Shard Harita Tutarlılığı ve Nihai Tamamlama**

  Hataların varlığında, bölme birleştirme hizmeti herhangi bir kesintiden sonra işlemleri ne olursa olsun devam eder ve devam eden istekleri tamamlamayı amaçlar. Ancak, düzeltilemeyen durumlar olabilir, örneğin, hedef parçanın kaybolması veya tamir edilemeyecek şekilde tehlikeye atılması gibi durumlar olabilir. Bu şartlar altında, taşınması gereken bazı kırıklar kaynak parçaüzerinde yaşamaya devam edebilir. Hizmet, parçalama eşlemelerinin yalnızca gerekli veriler hedefe başarıyla kopyalandıktan sonra güncelleştirilmesini sağlar. Tüm verileri hedefe kopyalandıktan ve ilgili eşlemeler başarıyla güncelleştirildikten sonra yalnızca shardlets kaynakta silinir. Aralık hedef parça üzerinde zaten çevrimiçi iken silme işlemi arka planda gerçekleşir. Bölme birleştirme hizmeti her zaman parça lı haritada depolanan eşlemelerin doğruluğunu sağlar.

## <a name="the-split-merge-user-interface"></a>Bölme birleştirme kullanıcı arabirimi

Bölme birleştirme hizmet paketi, bir çalışan rolü ve bir web rolü içerir. Web rolü, bölme birleştirme isteklerini etkileşimli bir şekilde göndermek için kullanılır. Kullanıcı arabiriminin ana bileşenleri aşağıdaki gibidir:

- **İşlem Türü**

  İşlem türü, bu istek için hizmet tarafından gerçekleştirilen işlem türünü kontrol eden bir radyo düğmesidir. Bölme, birleştirme ve taşıma senaryoları arasında seçim yapabilirsiniz. Daha önce gönderilen bir işlemi de iptal edebilirsiniz. Aralık parçaları haritaları için bölme, birleştirme ve taşıma isteklerini kullanabilirsiniz. Liste parçaları eşlemleri yalnızca taşıma işlemlerini destekler.

- **Shard Haritası**

  İstek parametrelerinin bir sonraki bölümü, parça harita ve parça haritanızı barındıran veritabanı hakkındaki bilgileri kapsar. Özellikle, parça harita veritabanına bağlanmak için azure SQL Veritabanı sunucusunun ve veritabanının adını, parça harita veritabanına bağlanmak için kimlik bilgilerini ve son olarak da parça lı haritanın adını sağlamanız gerekir. Şu anda, işlem yalnızca tek bir kimlik bilgileri kümesini kabul eder. Bu kimlik bilgilerinin, parça lı haritada ve parçadaki kullanıcı verilerinde değişiklik yapmak için yeterli izinlere sahip olması gerekir.

- **Kaynak Aralığı (bölme ve birleştirme)**

  Bölme ve birleştirme işlemi, düşük ve yüksek anahtarını kullanarak bir aralığı işler. Sınırsız yüksek anahtar değerine sahip bir işlem belirtmek için "Yüksek anahtar maksimumdur" onay kutusunu işaretleyin ve yüksek anahtar alanını boş bırakın. Belirttiğiniz aralık anahtar değerlerinin, parça haritanızdaki eşleme ve sınırlarıyla tam olarak eşleşmesi gerekmez. Herhangi bir aralık sınırı belirtmezseniz, hizmet sizin için otomatik olarak en yakın aralığı çıkaracaktır. Belirli bir parça haritadaki geçerli eşlemeleri almak için GetMappings.ps1 PowerShell komut dosyasını kullanabilirsiniz.

- **Kaynak Davranışı Böl (bölme)**

  Bölünmüş işlemler için, kaynak aralığını bölmek için noktayı tanımlayın. Bunu, bölmenin gerçekleşmesini istediğiniz parçalama anahtarını sağlayarak yaparsınız. Radyo düğmesini kullanın, aralığın alt kısmının (bölme tuşu hariç) hareket etmesini mi yoksa üst kısmın hareket etmesini mi istediğinizi (bölme tuşu dahil) belirtin.

- **Kaynak Shardlet (hareket)**

  Taşıma işlemleri, kaynağı açıklamak için bir aralık gerektirmedikleri için bölme veya birleştirme işlemlerinden farklıdır. Taşıma kaynağı yalnızca taşımayı planladığınız parçalama anahtar değeriyle tanımlanır.

- **Hedef Shard (bölünmüş)**

  Bölünmüş işleminizin kaynağıyla ilgili bilgileri sağladıktan sonra, hedef için Azure SQL Db sunucusu ve veritabanı adını sağlayarak verilerin nerede kopyalanmasını istediğinizi tanımlamanız gerekir.

- **Hedef Aralığı (birleştirme)**

  Birleştirme işlemleri, parçaları varolan bir parçaya taşır. Birleştirmek istediğiniz varolan aralığın aralık sınırlarını sağlayarak varolan parçayı tanımlarsınız.

- **Toplu İşlem Boyutu**

  Toplu iş boyutu, veri hareketi sırasında bir seferde çevrimdışı olacak parça sayısını denetler. Bu, kırıklar için uzun süreler boyunca kapalı kalma sürelerine karşı hassas olduğunuzda daha küçük değerleri kullanabileceğiniz bir tamsayı değeridir. Daha büyük değerler, belirli bir parçanın çevrimdışı olduğu süreyi artırır, ancak performansı artırabilir.

- **İşlem Kimliği (İptal)**

  Artık gerekmeyecek devam eden bir işleminiz varsa, bu alanda işlem kimliğini sağlayarak işlemi iptal edebilirsiniz. İşlem kimliğini istek durum tablosundan (bkz. Bölüm 8.1'e bakınız) veya isteği gönderdiğiniz web tarayıcısındaki çıktıdan alabilirsiniz.

## <a name="requirements-and-limitations"></a>Gereksinimler ve Sınırlamalar

Bölme birleştirme hizmetinin geçerli uygulaması aşağıdaki gereksinimlere ve sınırlamalara tabidir:

- Bu kırıklar üzerinde bir birleştirme işlemi yapılmadan önce parçaların var olması ve parça haritasına kaydedilmesi gerekir.
- Hizmet, işlemlerinin bir parçası olarak otomatik olarak tablolar veya başka bir veritabanı nesnesi oluşturmaz. Bu, herhangi bir bölme/birleştirme/taşıma işleminden önce tüm kırık tablolar ve başvuru tabloları için şema hedef parça üzerinde var olması gerektiği anlamına gelir. Özellikle parçalanmış tabloların, bölme/birleştirme/taşıma işlemi yle yeni parçaların eklendiği aralıkta boş olması gerekir. Aksi takdirde, işlem hedef parça üzerinde ilk tutarlılık denetimi başarısız olur. Ayrıca, başvuru verilerinin yalnızca başvuru tablosu boşsa kopyalanır ve başvuru tablolarındaki diğer eşzamanlı yazma işlemleriyle ilgili tutarlılık garantisi bulunmadığını da unutmayın. Bunu öneririz: bölme/birleştirme işlemleri çalıştırılırken, başvuru tablolarında başka hiçbir yazma işlemi değişiklik yapmaz.
- Hizmet, büyük parçaların performansını ve güvenilirliğini artırmak için parçalama anahtarını içeren benzersiz bir dizin veya anahtar tarafından oluşturulan satır kimliğine dayanır. Bu, hizmetin verileri parçalama anahtar değerinden daha ince bir parçalı ayrıntıda taşımasına olanak tanır. Bu, işlem sırasında gereken maksimum günlük alanı ve kilit miktarını azaltmaya yardımcı olur. Bu tabloyu bölme/birleştirme/taşıma istekleriyle kullanmak istiyorsanız, belirli bir tablodaki parçalama anahtarını içeren benzersiz bir dizin veya birincil anahtar oluşturmayı düşünün. Performans nedenleriyle, parçalama anahtarı anahtarveya dizin deki satır aralığı olmalıdır.
- İstek işleme sırasında, bazı shardlet verileri hem kaynak ta hem de hedef parçaüzerinde bulunabilir. Bu, shardlet hareketi sırasında ki hatalara karşı korumak için gereklidir. Parça lı haritayla bölünmüş birleştirme nin tümleştirilmesi, parça lı haritadaki **OpenConnectionForKey** yöntemini kullanarak veriye bağlı yönlendirme API'leri aracılığıyla yapılan bağlantıların tutarsız ara durumları görmemesini sağlar. Ancak, **OpenConnectionForKey** yöntemini kullanmadan kaynağa veya hedef parçalarına bağlanırken, bölme/birleştirme/taşıma istekleri devam ederken tutarsız ara durumlar görülebilir. Bu bağlantılar, zamanlamaya veya bağlantının altında yatan parçaya bağlı olarak kısmi veya yinelenen sonuçlar gösterebilir. Bu sınırlama şu anda Elastik Ölçek Multi-Shard-Sorgular tarafından yapılan bağlantıları içerir.
- Bölme birleştirme hizmetiiçin meta veri veritabanı farklı roller arasında paylaşılmamalıdır. Örneğin, ayırmada çalışan bölme birleştirme hizmetinin rolünün üretim rolünden farklı bir meta veri veritabanına işaret etmesi gerekir.

## <a name="billing"></a>Faturalandırma

Bölme birleştirme hizmeti, Microsoft Azure aboneliğinizde bulut hizmeti olarak çalışır. Bu nedenle bulut hizmetleri için ücretler hizmet örneğinize uygulanır. Sık sık bölme/birleştirme/taşıma işlemleri gerçekleştirmediğiniz sürece, bölme birleştirme bulut hizmetinizi silmenizi öneririz. Bu, bulut hizmeti örneklerinin çalıştırılaması veya dağıtılması için maliyet tasarrufu sağlar. Bölme veya birleştirme işlemleri gerçekleştirmeniz gerektiğinde yeniden dağıtabilir ve kolayca çalıştırılabilir yapılandırmanızı başlatabilirsiniz.

## <a name="monitoring"></a>İzleme

### <a name="status-tables"></a>Durum tabloları

Bölünmüş birleştirme Hizmeti, tamamlanan ve devam eden isteklerin izlenmesi için meta veri deposu veritabanında **İstek Durumu** tablosunu sağlar. Tablo, bu bölme birleştirme hizmetiörneğine gönderilen her bölme birleştirme isteği için bir satır listeler. Her istek için aşağıdaki bilgileri verir:

- **Zaman damgası**

  İsteğin başlatıldığı saat ve tarih.

- **OperationId**

  İsteği benzersiz olarak tanımlayan bir GUID. Bu istek, devam ederken işlemi iptal etmek için de kullanılabilir.

- **Durum**

  İsteğin geçerli durumu. Devam eden istekler için, isteğin geçerli aşamasını da listeler.

- **İptal İsteği**

  İsteğin iptal edilip edilemediğini belirten bir bayrak.

- **Ilerleme**

  İşlemin tamamlanma yüzdesi tahmini. 50 değeri, işlemin yaklaşık %50 tamamladığını gösterir.

- **Şey**

  Daha ayrıntılı bir ilerleme raporu sağlayan bir XML değeri. Satır kümeleri kaynaktan hedefe kopyalandığında, ilerleme raporu düzenli olarak güncelleştirilir. Hatalar veya özel durumlar durumunda, bu sütun da hata hakkında daha ayrıntılı bilgiler içerir.

### <a name="azure-diagnostics"></a>Azure Tanılama

Bölme birleştirme hizmeti, izleme ve tanılama için Azure SDK 2.5'i temel alan Azure Tanılama'yı kullanır. Burada açıklandığı gibi tanılama yapılandırmasını siz denetlersiniz: [Azure Bulut Hizmetlerinde ve Sanal Makinelerde Tanılamayı Etkinleştirme.](../cloud-services/cloud-services-dotnet-diagnostics.md) İndirme paketi, biri web rolü, diğeri de işçi rolü için olmak üçünün iki araştırım konfigürasyonlarını içerir. Performans Sayaçları, IIS günlükleri, Windows Olay Günlükleri ve bölünmüş birleştirme uygulaması olay günlüklerini günlüğe kaydetmek için tanımlar içerir.

## <a name="deploy-diagnostics"></a>Tanılamayı Dağıt

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

NuGet paketi tarafından sağlanan web ve çalışan rolleri için tanılama yapılandırmasını kullanarak izleme ve tanılamayı etkinleştirmek için Azure PowerShell kullanarak aşağıdaki komutları çalıştırın:

```powershell
$storageName = "<azureStorageAccount>"
$key = "<azureStorageAccountKey"
$storageContext = New-AzStorageContext -StorageAccountName $storageName -StorageAccountKey $key
$configPath = "<filePath>\SplitMergeWebContent.diagnostics.xml"
$serviceName = "<cloudServiceName>"

Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext `
    -DiagnosticsConfigurationPath $configPath -ServiceName $serviceName `
    -Slot Production -Role "SplitMergeWeb"

Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext `
    -DiagnosticsConfigurationPath $configPath -ServiceName $serviceName `
    -Slot Production -Role "SplitMergeWorker"
```

Tanılama ayarlarını yapılandırma ve dağıtma hakkında daha fazla bilgiyi burada bulabilirsiniz: [Azure Bulut Hizmetlerinde ve Sanal Makinelerde Tanılamayı Etkinleştirme.](../cloud-services/cloud-services-dotnet-diagnostics.md)

## <a name="retrieve-diagnostics"></a>Tanılamayı al

Sunucu Gezgini ağacının Azure bölümündeki Visual Studio Server Explorer'dan tanılamalarınıza kolayca erişebilirsiniz. Visual Studio örneğini açın ve menü çubuğunda Görünüm ve Sunucu Gezgini'ni tıklatın. Azure aboneliğinize bağlanmak için Azure simgesini tıklatın. Ardından Azure -> Depolama `<your storage account>` -> -> Tabloları -> WADLogsTable'a gidin. Daha fazla bilgi için [Bkz. Sunucu Gezgini.](https://msdn.microsoft.com/library/x603htbk.aspx)

![WADLogsTablo][2]

Yukarıdaki şekilde vurgulanan WADLogsTable, bölme birleştirme hizmetinin uygulama günlüğündeki ayrıntılı olayları içerir. İndirilen paketin varsayılan yapılandırmasının bir üretim dağıtımına yönelik olduğunu unutmayın. Bu nedenle günlüklerin ve sayaçların servis örneklerinden çekildiği aralık büyüktür (5 dakika). Test ve geliştirme için, web'in tanılama ayarlarını veya çalışan rolünü gereksinimlerinize göre ayarlayarak aralığı düşürün. Visual Studio Server Explorer'daki role sağ tıklayın (yukarıya bakın) ve ardından Tanılama yapılandırma ayarları için iletişim kutusunda Aktarım Süresini ayarlayın:

![Yapılandırma][3]

## <a name="performance"></a>Performans

Genel olarak, Azure SQL Veritabanı'ndaki daha yüksek ve daha performanslı hizmet katmanlarından daha iyi performans beklenebilir. Daha yüksek hizmet katmanları için daha yüksek IO, CPU ve bellek ayırmaları, bölme birleştirme hizmetinin kullandığı toplu kopyalama ve silme işlemlerinden yararlanır. Bu nedenle, yalnızca bu veritabanları için hizmet katmanını tanımlı, sınırlı bir süre için artırın.

Hizmet, normal işlemlerinin bir parçası olarak doğrulama sorguları da gerçekleştirir. Bu doğrulama sorguları, hedef aralıktaki beklenmeyen veri varlığını denetler ve herhangi bir bölme/birleştirme/taşıma işleminin tutarlı bir durumdan başlatılmasını sağlar. Bu sorguların tümü, işlemin kapsamı ve istek tanımının bir parçası olarak sağlanan toplu iş boyutu tarafından tanımlanan parçalama anahtar aralıkları üzerinde çalışır. Bu sorgular, satır aralığı olarak parçalama anahtarı olan bir dizin bulunduğunda en iyi performansı gösterir.

Buna ek olarak, satır aralığı olarak parçalama anahtarı na sahip bir teklik özelliği, hizmetin günlük alanı ve bellek açısından kaynak tüketimini sınırlayan en iyi duruma getirilmiş bir yaklaşım kullanmasına olanak sağlar. Bu benzersizlik özelliği, büyük veri boyutlarını (genellikle 1GB'ın üzerinde) taşımak için gereklidir.

## <a name="how-to-upgrade"></a>Yükseltme nasıl

1. Bir bölme [birleştirme hizmetini dağıt'taki](sql-database-elastic-scale-configure-deploy-split-and-merge.md)adımları izleyin.
2. Yeni yapılandırma parametrelerini yansıtacak şekilde bölme birleştirme dağıtımınız için bulut hizmeti yapılandırma dosyanızı değiştirin. Yeni bir gerekli parametre şifreleme için kullanılan sertifika hakkında bilgidir. Bunu yapmanın kolay bir yolu, karşıdan yüklemedeki yeni yapılandırma şablonu dosyasını varolan yapılandırmanızla karşılaştırmaktır. Hem web hem de çalışan rolü için "DataEncryptionPrimaryCertificateThumbprint" ve "DataEncryptionPrimary" ayarlarını eklediğinizden emin olun.
3. Güncelleştirmeyi Azure'a dağıtmadan önce, şu anda çalışan tüm bölünmüş birleştirme işlemlerinin tamamlandığından emin olun. Bunu, devam eden istekler için birleştirme meta veri veritabanındaki İstek Durumu ve Bekleyen Çalışma Akışları tablolarını sorgulayarak kolayca yapabilirsiniz.
4. Azure aboneliğinizde yeni paket ve güncelleştirilmiş hizmet yapılandırma dosyanızla bölünmüş birleştirme için mevcut bulut hizmeti dağıtımınızı güncelleştirin.

Yükseltme için bölme birleştirme için yeni bir meta veri veritabanı sağlamanız gerekmez. Yeni sürüm, varolan meta veri veritabanınızı otomatik olarak yeni sürüme yükseltir.

## <a name="best-practices--troubleshooting"></a>En iyi yöntemler ve sorun giderme

- Bir test kiracıtanımlayın ve en önemli bölme/birleştirme/taşıma işlemlerinizi test kiracısıyla birkaç parça arasında çalıştırın. Tüm meta verilerin parça haritanızda doğru şekilde tanımlandığından ve işlemlerin kısıtlamaları veya yabancı anahtarları ihlal etmediğini sağlayın.
- Veri boyutuyla ilgili sorunlarla karşılaşmadığınızdan emin olmak için test kiracısı veri boyutunu en büyük kiracınızın maksimum veri boyutunun üzerinde tutun. Bu, tek bir kiracıyı hareket ettirmek için gereken süreye göre bir üst sınırı değerlendirmenize yardımcı olur.
- Şemanızın silmeye izin verdiğinden emin olun. Bölme birleştirme hizmeti, veriler hedefe başarıyla kopyalandıktan sonra verileri kaynak parçadan kaldırma olanağı gerektirir. Örneğin, **silme tetikleyicileri** hizmetin kaynaktaki verileri silmesini engelleyebilir ve işlemlerin başarısız olmasıyla ilgili olabilir.
- Parçalama anahtarı, birincil anahtar veya benzersiz dizin tanımınızda önde gelen sütun olmalıdır. Bu, bölme veya birleştirme doğrulama sorguları ve her zaman parçalama anahtar aralıkları üzerinde çalışan gerçek veri hareketi ve silme işlemleri için en iyi performansı sağlar.
- Bölünmüş birleştirme hizmetinizi veritabanlarınızın bulunduğu bölgede ve veri merkezinde birleştirin.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
