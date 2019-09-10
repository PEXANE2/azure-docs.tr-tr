---
title: Azure SQL veritabanı hiper ölçeğe genel bakış | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı 'ndaki sanal çekirdek tabanlı satın alma modelinde hiper ölçek hizmeti katmanı açıklanmakta ve bu hizmetin Genel Amaçlı ve İş Açısından Kritik hizmet katmanlarından farklı olduğu açıklanır.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/06/2019
ms.openlocfilehash: 1d70c5d86221213ae3f9a2d31fdf40857cb516be
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70845667"
---
# <a name="hyperscale-service-tier-for-up-to-100-tb"></a>100 TB 'a kadar hiper ölçek hizmeti katmanı

Azure SQL veritabanı, altyapı hatalarının durumlarında bile% 99,99 kullanılabilirlik sağlamak için bulut ortamı için ayarlanmış SQL Server veritabanı motoru mimarisini temel alır. Azure SQL veritabanı 'nda kullanılan üç mimari modeli vardır:
- Genel Amaçlı/standart 
-  Hiper ölçek
-  İş Açısından Kritik/Premium

Azure SQL veritabanı 'nda bulunan hiper ölçekli hizmet katmanı, sanal çekirdek tabanlı satın alma modelindeki en yeni hizmet katmandır. Bu hizmet katmanı, bir Azure SQL veritabanı için depolama ve işlem kaynaklarının, Genel Amaçlı ve Iş için kullanılabilen limitlerin büyük ölçüde ötesinde ölçeğini genişletmek için Azure mimarisinden yararlanan, yüksek düzeyde ölçeklenebilir bir depolama ve bilgi işlem performans katmanından yararlanır Kritik hizmet katmanları.

> 
> [!NOTE]
> Sanal çekirdek tabanlı satın alma modelindeki Genel Amaçlı ve İş Açısından Kritik hizmet katmanlarına ilişkin ayrıntılar için bkz. [genel amaçlı](sql-database-service-tier-general-purpose.md) ve [iş açısından kritik](sql-database-service-tier-business-critical.md) hizmet katmanları. DTU tabanlı satın alma modeliyle sanal çekirdek tabanlı satın alma modeli karşılaştırması için bkz. [Azure SQL veritabanı satın alma modelleri ve kaynakları](sql-database-service-tiers.md).


## <a name="what-are-the-hyperscale-capabilities"></a>Hiperscale özellikleri nelerdir?

Azure SQL veritabanı 'ndaki hiper ölçek hizmeti katmanı aşağıdaki ek özellikleri sağlar:

- 100 TB 'a kadar veritabanı boyutu desteği
- İşlem kaynaklarında GÇ etkisi olmayan, neredeyse anlık veritabanı yedeklemeleri (Azure Blob depolamada depolanan dosya anlık görüntülerine göre)  
- Kısa veritabanı geri yüklemeleri (dosya anlık görüntülerini temel alarak) saatler veya günler yerine dakikalar içinde (veri işlemi boyutu değil)
- Veri birimlerinden bağımsız olarak daha yüksek günlük verimlilik ve daha hızlı işlem işleme sürelerinden dolayı daha yüksek genel performans
- Hızlı genişleme-okuma iş yükünüzü boşaltma ve sık erişimli olarak kullanım için bir veya daha fazla salt okuma düğümü sağlayabilirsiniz
- Hızlı ölçek artırma-sabit zamanlı olarak, yoğun iş yüklerinizi uygun olduğunda ve gerektiğinde işlem kaynaklarını geriye doğru ölçeklendirerek, işlem kaynaklarını ölçeklendirebilmeniz gerekir.

Hiper ölçek hizmeti katmanı, bulut veritabanlarında geleneksel olarak görülen pratik limitlerin çoğunu ortadan kaldırır. Diğer birçok veritabanının tek bir düğümde kullanılabilen kaynaklarla sınırlı olduğu yerlerde, hiper ölçek hizmet katmanındaki veritabanları bu tür sınırlara sahip değildir. Esnek depolama mimarisi sayesinde, depolama gerektiği gibi artar. Aslında, hiper ölçek veritabanları tanımlanmış en büyük boyutla oluşturulmaz. Hiper ölçekli bir veritabanı gerektiğinde büyür ve yalnızca kullandığınız kapasite için faturalandırılırsınız. Okuma yoğunluklu iş yükleri için, hiper ölçek hizmeti katmanı, okuma iş yüklerini boşaltma için gerektiğinde ek okuma çoğaltmaları sağlayarak hızlı genişleme sağlar.

Ayrıca, veritabanı yedeklemeleri oluşturmak için gereken süre ve ölçeği artırma veya azaltma, artık veritabanındaki veri hacmine bağlı değildir. Hiper ölçekli veritabanları neredeyse anında yedeklenebilir. Ayrıca, dakikada on terabaytlık bir veritabanını yukarı veya aşağı doğru bir şekilde ölçekleyebilirsiniz. Bu özellik, sizi ilk yapılandırma seçimlerinizde kutulandığında kaygıları ortadan boşaltır.

Hiper ölçek hizmet katmanının işlem boyutları hakkında daha fazla bilgi için bkz. [hizmet katmanı özellikleri](sql-database-service-tiers-vcore.md#service-tier-characteristics).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Hiper ölçek hizmet katmanını kimler kabul etmelidir

Hyperscale hizmet katmanı öncelikle şirket içinde büyük veritabanlarına sahip olan ve buluta ya da bulutta bulunan ve en büyük veritabanı boyutu ile sınırlı olan müşteriler için kendi uygulamalarını modernleştirin isteyen müşterilere yöneliktir kısıtlamalar (1-4 TB). Ayrıca, depolama ve işlem için yüksek performans ve yüksek ölçeklenebilirlik isteyen müşterilere de yöneliktir.

Hiper ölçek hizmeti katmanı tüm SQL Server iş yüklerini destekler, ancak öncelikle OLTP için iyileştirilmiştir. Hiper ölçek hizmeti katmanı da karma ve analitik (veri reyonu) iş yüklerini destekler.

> [!IMPORTANT]
> Elastik havuzlar hiper ölçek hizmet katmanını desteklemez.

## <a name="hyperscale-pricing-model"></a>Hiper ölçek fiyatlandırma modeli

Hiper ölçek hizmeti katmanı yalnızca [Vcore modelinde](sql-database-service-tiers-vcore.md)kullanılabilir. Yeni mimariye uyum sağlamak için, fiyatlandırma modeli Genel Amaçlı veya İş Açısından Kritik hizmet katmanlarından biraz farklıdır:

- **İşlem**:

  Hiper ölçek işlem birimi fiyatı çoğaltma başına. [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) fiyatı, otomatik olarak okuma ölçeği çoğaltmalarına uygulanır. Varsayılan olarak hiper ölçekli veritabanı başına bir birincil çoğaltma ve bir salt okuma çoğaltması oluşturacağız.  Kullanıcılar, 1-5 'den birincil dahil olmak üzere toplam çoğaltma sayısını ayarlayabilir.

- **Depolama alanı**:

  Hiper ölçekli bir veritabanını yapılandırırken en büyük veri boyutunu belirtmeniz gerekmez. Hiper ölçeklendirme katmanında, veritabanınıza yönelik depolama alanı için gerçek kullanıma göre ücret alınır. Depolama, 10 GB ila 40 GB arasında dinamik olarak ayarlanan artışlarla otomatik olarak 10 GB ve 100 TB arasında ayrılır.  

Hiperscale fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/single/)

## <a name="distributed-functions-architecture"></a>Dağıtılmış işlevler mimarisi

Veri yönetimi işlevlerinin tümünü tek bir konum/işlemde merkezi olarak kullanan geleneksel veritabanı altyapılarından farklı olarak (Bu nedenle üretimde bulunan dağıtılmış veritabanlarının bir tek parçalı veri altyapısının birden çok kopyasına sahip olduğu halde), bir hiper ölçek veritabanı ayrılır çeşitli veri altyapılarının semantiğinin, veriler için uzun süreli depolama ve dayanıklılık sağlayan bileşenlerden farklı olduğu sorgu işleme altyapısı. Bu şekilde, depolama kapasitesi gerektiği kadar düzgün şekilde ölçeklendirilebilir (başlangıç hedefi 100 TB). Salt okunur çoğaltmalar aynı depolama bileşenlerini paylaşır, böylece yeni bir okunabilir çoğaltmayı çalıştırmak için veri kopyalama gerekmez. 

Aşağıdaki diyagramda, bir hiper ölçek veritabanındaki farklı düğüm türleri gösterilmektedir:

![architecture](./media/sql-database-hyperscale/hyperscale-architecture.png)

Hiper ölçekli bir veritabanı aşağıdaki farklı düğüm türlerini içerir:

### <a name="compute-node"></a>İşlem düğümü

İşlem düğümü, ilişkisel altyapının bulunduğu yerdir, bu nedenle tüm dil öğeleri, sorgu işleme vb. oluşur. Hiper ölçekli veritabanı olan tüm kullanıcı etkileşimleri, bu işlem düğümleri aracılığıyla gerçekleşir. İşlem düğümlerinde, veri sayfasını getirmek için gereken ağ gidiş dönüşlerin sayısını en aza indirmek için SSD tabanlı önbellekler (önceki diyagramda RBPEX-dayanıklı arabellek havuzu uzantısı olarak etiketlenir) vardır. Tüm okuma/yazma iş yüklerinin ve işlemlerinin işlendiği bir birincil işlem düğümü vardır. Yük devretme amaçlarıyla etkin bekleme düğümleri görevi gören bir veya daha fazla ikincil işlem düğümü vardır ve okuma iş yüklerini boşaltma için salt okunurdur işlem düğümleri görevi görür (Bu işlev isteniyorsa).

### <a name="page-server-node"></a>Sayfa sunucusu düğümü

Sayfa sunucuları, ölçeği genişletilmiş bir depolama altyapısını temsil eden sistemlerdir.  Her sayfa sunucusu, veritabanındaki sayfaların bir alt kümesinden sorumludur.  Aday olarak, her sayfa sunucusu 128 GB ve 1 TB veri arasında denetler. Birden fazla sayfa sunucusunda (artıklık ve kullanılabilirlik için tutulan çoğaltmalar dışında) hiç veri paylaşılmıyor. Bir sayfa sunucusunun işi, veritabanı sayfalarını istek üzerine işlem düğümlerine sunmaya ve sayfaların işlem güncellik olarak güncelleştirilmesini sağlamak için kullanılır. Sayfa sunucuları günlük hizmetinden günlük kayıtlarını oynatarak güncel tutulur. Sayfa sunucuları, performansı geliştirmek için SSD tabanlı önbellekleri de korur. Veri sayfalarının uzun süreli depolaması, ek güvenilirlik sağlamak için Azure depolama 'da tutulur.

### <a name="log-service-node"></a>Günlük hizmeti düğümü

Günlük hizmeti düğümü, birincil işlem düğümündeki günlük kayıtlarını kabul eder, bunları dayanıklı bir önbellekte devam ettirir ve günlük kayıtlarını işlem düğümlerinin geri kalanına iletir (Bu nedenle, bu sayede önbellekler güncelleştirilebilmeleri için) ve verilerin güncelleştirilebilmesi için ilgili sayfa sunucuları şöyledir. Bu şekilde, birincil işlem düğümünden yapılan tüm veriler günlük hizmetinden tüm ikincil işlem düğümlerine ve sayfa sunucularına yayılır. Son olarak, günlük kayıtları, sınırsız bir depolama deposu olan Azure Storage 'daki uzun süreli depolamaya gönderilir. Bu mekanizma, sık kullanılan günlük kesilmesi için zorunludur 'yi kaldırır. Günlük hizmeti 'nin erişimi hızlandırmak için yerel önbelleği de vardır.

### <a name="azure-storage-node"></a>Azure depolama düğümü

Azure depolama düğümü, sayfa sunucularındaki verilerin son hedefüdür. Bu depolama, yedekleme amaçları ve Azure bölgeleri arasında çoğaltma için kullanılır. Yedeklemeler veri dosyalarının anlık görüntülerinden oluşur. Geri yükleme işlemi bu anlık görüntülerden hızlıdır ve veriler zaman içinde herhangi bir noktaya geri yüklenebilir.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Yedekler dosya anlık görüntü tabanınlardır ve bu nedenle neredeyse anında yapılır. Depolama ve işlem ayrımı, birincil işlem düğümündeki yük yükünü azaltmak için yedekleme/geri yükleme işleminin depolama katmanına itilemesini sağlar. Sonuç olarak, büyük bir veritabanının yedeklemesi birincil işlem düğümünün performansını etkilemez. Benzer şekilde, geri yükleme işlemi, dosya anlık görüntüsü kopyalanarak ve bu gibi bir veri boyutu olmadığından yapılır. Aynı depolama hesabı içindeki geri yüklemeler için geri yükleme işlemi hızlıdır.

## <a name="scale-and-performance-advantages"></a>Ölçek ve performans avantajları

Ek salt okuma işlem düğümlerini hızlı bir şekilde artırma/azaltma özelliği sayesinde, hiper ölçek mimarisi önemli okuma ölçeği özelliklerine izin verir ve ayrıca, daha fazla yazma isteği sunmak için birincil işlem düğümünü de serbest bırakabilirsiniz. Ayrıca, hiper ölçek mimarisinin paylaşılan depolama mimarisi nedeniyle işlem düğümleri hızlı bir şekilde ölçeklenebilir/aşağı ölçeklendirilebilir.

## <a name="create-a-hyperscale-database"></a>Hiper ölçekli veritabanı oluşturma

[Azure Portal](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)kullanılarak bir hiper ölçek veritabanı oluşturulabilir. Hiper ölçekli veritabanları yalnızca [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)kullanılarak kullanılabilir.

Aşağıdaki T-SQL komutu, hiper ölçekli bir veritabanı oluşturur. `CREATE DATABASE` Bildiriminde hem sürüm hem de hizmet hedefi belirtmeniz gerekir. Geçerli hizmet amaçları listesinin [kaynak sınırlarına](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases#hyperscale-service-tier-for-provisioned-compute) bakın.

```sql
-- Create a HyperScale Database
CREATE DATABASE [HyperScaleDB1] (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```
Bu, 4 çekirdek içeren 5. nesil donanımında bir hiper ölçek veritabanı oluşturur.

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Mevcut bir Azure SQL veritabanını hiper ölçekli hizmet katmanına geçirme

Mevcut Azure SQL veritabanlarınızı [Azure Portal](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)kullanarak hiper ölçeğe taşıyabilirsiniz. Bu sırada, tek yönlü geçişdir. Veritabanlarını Hyperscale 'den başka bir hizmet katmanına taşıyamazsınız. Üretim veritabanlarınızın bir kopyasını oluşturmanızı ve kavram kanıtı (POCs) için Hiperscale 'e geçirmeniz önerilir.

Aşağıdaki T-SQL komutu bir veritabanını hiper ölçek hizmeti katmanına taşıdır. `ALTER DATABASE` Bildiriminde hem sürüm hem de hizmet hedefi belirtmeniz gerekir.

```sql
-- Alter a database to make it a HyperScale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Hiper ölçekli bir veritabanının okuma ölçeğinde bir çoğaltmasını bağlama

Hiper ölçekli veritabanlarında, `ApplicationIntent` istemci tarafından belirtilen bağlantı dizesindeki bağımsız değişken bağlantının yazma çoğaltmasına mı yoksa salt bir ikincil çoğaltmayla mi yönlendirildiğini belirler. `ReadWrite` Olarak `ApplicationIntent` ayarlanmışveveritabanındabirikincilçoğaltmayoksa,bağlantıbirincilçoğaltmayayönlendirilirvevarsayılanolarakdavranışolur.`READONLY`

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```
## <a name="disaster-recovery-for-hyperscale-databases"></a>Hiper ölçekli veritabanları için olağanüstü durum kurtarma
### <a name="restoring-a-hyperscale-database-to-a-different-geography"></a>Hiper ölçek veritabanını farklı bir Coğrafya 'ya geri yükleme
Bir Azure SQL veritabanı hiper ölçek veritabanını, bir olağanüstü durum kurtarma işleminin veya detaya gitme, yeniden konumlandırma ya da başka bir nedenden dolayı barındırmakta olduğu bir bölgeye geri yüklemeniz gerekiyorsa, birincil yöntem, veritabanının coğrafi olarak geri yüklenmesini sağlar.  Bu, başka bir AZURE SQL DB 'yi farklı bir bölgeye geri yüklemek için kullandığınız adımlarla tam olarak aynı adımları içerir:
1. Henüz uygun bir sunucunuz yoksa hedef bölgede bir SQL veritabanı sunucusu oluşturun.  Bu sunucunun özgün (kaynak) sunucuyla aynı aboneliğe sahip olması gerekir.
2. Azure SQL veritabanlarını otomatik yedeklemelerden geri yükleme sayfasındaki [coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#geo-restore) konusundaki yönergeleri izleyin.

> [!NOTE]
> Kaynak ve hedef ayrı bölgelerde olduğundan, veritabanı coğrafi olarak son derece hızlı bir şekilde tamamlanan, coğrafi olmayan geri yüklemeler halinde kaynak veritabanıyla birlikte anlık görüntü depolamayı paylaşamaz.  Hiper ölçekli bir veritabanının coğrafi geri yüklemesi söz konusu olduğunda, hedef coğrafi olarak çoğaltılan depolamanın eşleştirilmiş bölgesinde olsa bile, veri boyutu bir işlem olur.  Bu, coğrafi geri yükleme yapmanın, geri yüklenmekte olan veritabanının boyutuyla orantılı bir zaman alabileceği anlamına gelir.  Hedef eşleştirilmiş bölgedeyse, kopya bir veri merkezinde yer alacak ve bu, internet üzerinden uzun mesafe kopyasından önemli ölçüde daha hızlı olacaktır, ancak yine de tüm bitleri kopyalayacaktır.

## <a name=regions></a>Kullanılabilir bölgeler

Azure SQL veritabanı hiper ölçek katmanı Şu anda aşağıdaki bölgelerde kullanılabilir:

- Avustralya Doğu
- Avustralya Güneydoğu
- Güney Brezilya
- Orta Kanada
- Orta ABD
- Çin Doğu 2
- Çin Kuzey 2
- Doğu Asya
- East US
- Doğu ABD 2
- Fransa Orta
- Japonya Doğu
- Japonya Batı
- Kore Orta
- Kore Güney
- Orta Kuzey ABD
- Kuzey Avrupa
- Güney Afrika Kuzey
- Orta Güney ABD
- Güneydoğu Asya
- Birleşik Krallık Güney
- Birleşik Krallık Batı
- Batı Avrupa
- Batı ABD
- Batı ABD 2

Desteklenen olarak listelenmeyen bir bölgede hiper ölçekli veritabanı oluşturmak istiyorsanız, Azure portal aracılığıyla bir ekleme isteği gönderebilirsiniz. Desteklenen bölgelerin listesini genişletmek için çalışıyoruz. bu nedenle lütfen en son bölge listesini inceleyin.

Listelenmeyen bölgelerde hiper ölçekli veritabanları oluşturma özelliğini istemek için:

1. [Azure yardım ve destek dikey penceresine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) gidin

2. [ **Yeni destek isteğine** tıklayın](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

    ![Azure yardım ve destek dikey penceresi](media/sql-database-service-tier-hyperscale/request-screen-1.png)

3. **Sorun türü**için **hizmet ve abonelik sınırları (kotalar)** seçeneğini belirleyin

4. Veritabanlarını oluşturmak için kullanacağınız aboneliği seçin

5. **Kota türü**için **SQL veritabanı** ' nı seçin.

6. İleri **' ye tıklayın: Çözümlere**

1. **Ayrıntıları sağla** ' ya tıklayın

    ![Sorun ayrıntıları](media/sql-database-service-tier-hyperscale/request-screen-2.png)

8. **SQL veritabanı kota türünü**seçin: **Diğer kota isteği**

9. Aşağıdaki şablonu doldur:

    ![Kota ayrıntıları](media/sql-database-service-tier-hyperscale/request-screen-3.png)

    Şablonda, aşağıdaki bilgileri sağlayın

    > Yeni bir bölgede Azure hiper ölçek SQL veritabanı oluşturma isteği<br/> Bölge: [istediğiniz bölgeyi doldur]  <br/>
    > Okunabilir çoğaltmalar dahil olmak üzere SKU/toplam çekirdekleri hesaplama <br/>
    > Tahmini TB sayısı 
    >

10. **Önem Derecesi C**’yi seçin

11. Uygun iletişim yöntemini seçin ve ayrıntıları girin.

12. **Kaydet** ve **devam et** 'e tıklayın

## <a name="known-limitations"></a>Bilinen sınırlamalar
Bunlar, GA ile aynı hiper ölçek hizmet katmanına yönelik geçerli sınırlamalardır.  Mümkün olduğunca bu sınırlamaların çoğunu kaldırmak için etkin bir şekilde çalışıyoruz.

| Sorun | Açıklama |
| :---- | :--------- |
| Bir mantıksal sunucu için yedeklemeleri Yönet bölmesi, hiper ölçek veritabanlarını göstermez ve SQL Server 'dan filtrelenecektir  | Hiper ölçek, yedeklemeleri yönetmek için ayrı bir yönteme sahiptir ve bu nedenle uzun süreli bekletme ve zaman içinde yedekleme bekletme ayarları uygulanmaz/geçersiz kılınır. Buna uygun olarak, hiper ölçekli veritabanları yedekleme bölmesini yönetme bölmesinde görünmez. |
| Belirli bir noktaya geri yükleme | Bir veritabanı hiper ölçek hizmeti katmanına geçirildikten sonra, geçiş işleminden önce bir zaman noktasına geri yükleme desteklenmez.|
| Hiperzölçekli DB 'yi Hypserscale 'e geri yükleme ve tam tersi | Hiper ölçekli bir veritabanını hiper olmayan bir veritabanına geri yükleyemezsiniz veya hiper ölçekli bir veritabanını hiper ölçekli bir veritabanına geri yükleyemezsiniz.|
| Bir veritabanı dosyası, etkin bir iş yükü nedeniyle geçiş sırasında büyürse ve dosya sınırı başına 1 TB kesişmezse, geçiş başarısız olur | Karşı <br> Mümkünse, çalışan bir güncelleştirme iş yükü olmadığında veritabanını geçirin.<br> -Geçişi yeniden deneyin, geçiş sırasında 1 TB sınırının çapraz olmadığı sürece başarılı olur.|
| Yönetilen Örnek | Azure SQL veritabanı yönetilen örneği şu anda hiper ölçekli veritabanları ile desteklenmiyor. |
| Elastik Havuzlar |  Esnek havuzlar Şu anda SQL veritabanı Hyperscale ile desteklenmemektedir.|
| Hyperscale 'e geçiş Şu anda tek yönlü bir işlemdir | Bir veritabanı hiper ölçeğe geçirildiğinde doğrudan Hyperscale olmayan bir hizmet katmanına geçirilemez. Mevcut olduğunda, bir veritabanını Hyperscale 'den hiper olmayan ölçeğe geçirmenin tek yolu BACPAC dosyasını kullanarak dışarı ve içeri aktarma işlemi kullanmaktır.|
| Kalıcı bellek içi nesneleri olan veritabanlarının geçirilmesi | Hiper ölçek yalnızca kalıcı olmayan bellek nesnelerini destekler (tablo türleri, yerel SPs 'ler ve işlevler).  Kalıcı bellek Içi tablolar ve diğer nesneler, bir veritabanını hiper ölçek hizmet katmanına geçirmeden önce, bellek içi nesneler olarak bırakılmalıdır ve yeniden oluşturulmalıdır.|
| Değişiklik İzleme | Hiper ölçekli veritabanları ile Değişiklik İzleme kullanamazsınız. |
| Coğrafi Çoğaltma  | Azure SQL veritabanı hiper ölçek için Coğrafi çoğaltmayı henüz yapılandıramazsınız.  Coğrafi geri yükleme gerçekleştirebilirsiniz (veritabanını farklı bir Coğrafya 'da, DR veya diğer amaçlar için geri yükleme) |
| TDE/AKV tümleştirmesi | Azure Key Vault kullanılarak Saydam veritabanı şifrelemesi (genellikle kendi anahtarını getir veya BYOK olarak adlandırılır), Azure SQL veritabanı Hyperscale için henüz desteklenmemiştir, ancak hizmet tarafından yönetilen anahtarlarla birlikte TDE desteklenir. |
|Akıllı veritabanı özellikleri | 1. Dizin Oluştur, Drop Index danışmanlayıcı modelleri hiper ölçekli veritabanları için eğitilemiyor. <br/>2. Şema sorunu, Dbparameterleştirme-son eklenen danışmanlar hiper ölçek veritabanı için desteklenmiyor.|



## <a name="next-steps"></a>Sonraki adımlar

- Hyperscale hakkında bir SSS için bkz. [hyperscale hakkında sık sorulan sorular](sql-database-service-tier-hyperscale-faq.md).
- Hizmet katmanları hakkında daha fazla bilgi için bkz. [hizmet katmanları](sql-database-service-tiers.md)
- Sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için bkz. [mantıksal sunucudaki kaynak sınırlarına genel bakış](sql-database-resource-limits-logical-server.md) .
- Tek bir veritabanı için model sınırları satın almak için, [tek bir veritabanı için bkz. Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sınırları](sql-database-vcore-resource-limits-single-databases.md).
- Özellikler ve karşılaştırma listesi için bkz. [SQL ortak özellikleri](sql-database-features.md).
