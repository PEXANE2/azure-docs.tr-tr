---
title: Azure SQL veritabanı sunucusuz (Önizleme) | Microsoft Docs
description: Bu makalede, yeni sunucusuz işlem katmanı açıklanmakta ve mevcut sağlanan işlem katmanıyla karşılaştırılır
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
ms.date: 07/05/2019
ms.openlocfilehash: 67e877609eec98e7100b34ab477dbab7c5577772
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515290"
---
# <a name="azure-sql-database-serverless-preview"></a>Azure SQL veritabanı sunucusuz (Önizleme)

Azure SQL veritabanı sunucusuz (Önizleme), her bir veritabanı için, bir saniyede kullanılan işlem miktarına göre işlem yükünü otomatik olarak ölçeklendiren bir işlem katmandır. Sunucusuz bilgi işlem katmanı Ayrıca, yalnızca depolama faturalandırılırken etkin olmayan dönemler sırasında veritabanlarını otomatik olarak duraklatır ve etkinlik döndüğünde veritabanlarını otomatik olarak sürdürür.

## <a name="serverless-compute-tier"></a>Sunucusuz işlem katmanı

Tek bir veritabanı için sunucusuz işlem katmanı, bir işlem otomatik ölçeklendirme aralığı ve otomatik duraklama gecikmesi tarafından parametrelenir.  Bu parametrelerin yapılandırması, veritabanı performans deneyimini ve işlem maliyetini şekillendirilir.

![Sunucusuz faturalandırma](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance-configuration"></a>Performans yapılandırması

- **Minimum sanal çekirdekler** ve **maksimum sanal çekirdekler** , veritabanı için kullanılabilir işlem kapasitesi aralığını tanımlayan yapılandırılabilir parametrelerdir. Bellek ve GÇ sınırları belirtilen vCore aralığıyla orantılıdır.  
- Otomatik **duraklatma gecikmesi** , veritabanının otomatik olarak duraklatılmadan önce devre dışı olması gereken süreyi tanımlayan yapılandırılabilir bir parametredir. Sonraki oturum açma veya diğer etkinlik gerçekleştiğinde veritabanı otomatik olarak sürdürülür.  Alternatif olarak, oto duraklamayı devre dışı bırakılabilir.

### <a name="cost"></a>Maliyet

- Sunucusuz bir veritabanının maliyeti, işlem maliyeti ve depolama maliyetinin özetidir.
- İşlem kullanımı, yapılandırılan minimum ve maksimum limitlerin arasında olduğunda, işlem maliyeti sanal çekirdeği ve kullanılan belleği temel alır.
- İşlem kullanımı, yapılandırılan minimum limitlerin altındaysa, işlem maliyeti en düşük sanal çekirdekleri ve en az belleğe göre yapılandırılır.
- Veritabanı duraklatıldığında, işlem maliyeti sıfırdır ve yalnızca depolama maliyetleri tahakkuk edilir.
- Depolama maliyeti, sağlanan işlem katmanının ile aynı şekilde belirlenir.

Daha fazla maliyet ayrıntısı için bkz. [faturalandırma](sql-database-serverless.md#billing).

## <a name="scenarios"></a>Senaryolar

Sunucusuz, boşta kullanım dönemlerinden sonra işlem ısınma süresi boyunca zaman aralıklı, öngörülemeyen kullanım desenleriyle tek veritabanları için en iyi duruma getirilmiş fiyat performanslarıdır. Buna karşılık, sağlanan işlem katmanı, tek veritabanları veya esnek havuzlardaki birden çok veritabanı için en iyi duruma getirilmiş, işlem ısınma sırasında herhangi bir gecikmeyi karşılayamamakta olan bir gecikme süresi daha yüksektir.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Daha az işlem için senaryolar iyi uygun

- Kesintili kullanım desenlerine sahip tek veritabanları, zaman içinde etkinlik dışı ve düşük ortalama işlem kullanımı dönemleriyle birlikte oluşmuştur.
- Sağlanan işlem katmanındaki tek veritabanları, genellikle ölçeklendirildi ve hizmet için işlem yeniden oluşturmayı tercih eden müşteriler.
- SQL veritabanı 'nda dağıtımdan önce işlem boyutlandırmanın zor veya tahmin edilmesi mümkün olmayan kullanım geçmişi olmayan yeni tek veritabanları.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Senaryolar, sağlanan işlem için iyi uygun

- Daha düzenli, öngörülebilir kullanım desenleri ve zaman içinde daha fazla ortalama işlem kullanımı olan tek veritabanları.
- Daha sık bellek kırpmasını veya duraklatılmış bir durumdan kaynaklanan gecikme süresi ile ilgili performansı karşılamaz olan veritabanları.
- Daha iyi fiyat performansı iyileştirmesi için esnek havuzlarda birleştirilemeyen, kesintili kullanım desenlerine sahip birden çok veritabanı.

## <a name="comparison-with-provisioned-compute-tier"></a>Sağlanan işlem katmanıyla karşılaştırma

Aşağıdaki tabloda sunucusuz bilgi işlem katmanı ve sağlanan işlem katmanı arasındaki farklılıklar özetlenmektedir:

| | **Sunucusuz işlem** | **Sağlanan işlem** |
|:---|:---|:---|
|**Veritabanı kullanım deseninin**| Zaman içinde daha düşük ortalama işlem kullanımı ile öngörülemeyen kullanım |  Zamana göre daha fazla ortalama işlem kullanımı veya elastik havuzlar kullanan birden çok veritabanı içeren daha düzenli kullanım düzenleri.|
| **Performans yönetimi çabaları** |Düşürül|Daha yüksek|
|**İşlem ölçekleme**|Otomatik|El ile|
|**İşlem yanıtlama hızı**|Etkin olmayan dönemlerden sonra düşük|Hemen|
|**Faturalandırma ayrıntı düzeyi**|/Saniye|Saatlik|

## <a name="purchasing-model-and-service-tier"></a>Model ve hizmet katmanı satın alma

SQL veritabanı sunucusuz, şu anda yalnızca sanal çekirdek satın alma modelinde 5. nesil donanımda Genel Amaçlı katmanında desteklenir.

## <a name="autoscaling"></a>Otomatik ölçeklendirme

### <a name="scaling-responsiveness"></a>Ölçeklendirme yanıt hızı

Genel olarak sunucusuz veritabanları, en fazla Vçekirdekler değeri tarafından ayarlanan limitlerde istenen herhangi bir işlem için kesintiye uğramadan kaynak talebini karşılamak üzere yeterli kapasiteye sahip bir makine üzerinde çalıştırılır. Bazen, makine kaynak talebini birkaç dakika içinde karşılayamaz, Yük Dengeleme otomatik olarak gerçekleşir. Örneğin, kaynak talebi 4 sanal çekirdektir, ancak yalnızca 2 sanal çekirdek varsa, 4 sanal çekirdek sağlanmadan önce yük dengelenmesi birkaç dakika sürebilir. Veritabanı, bağlantı kesildiğinde işlemin sonunda kısa bir dönem haricinde yük dengelemesi sırasında çevrimiçi kalır.

### <a name="memory-management"></a>Bellek yönetimi

Sunucusuz veritabanları için bellek, sağlanan işlem veritabanlarından daha sık geri kazanılır. Bu davranış sunucusuz 'de maliyetleri denetlemek için önemlidir ve performansı etkileyebilir.

#### <a name="cache-reclamation"></a>Önbellek geri kazanma

Sağlanan işlem veritabanlarının aksine, CPU veya önbellek kullanımı düşük olduğunda SQL önbelleğinden alınan bellek sunucusuz bir veritabanından geri kazanılır.

- En son kullanılan önbellek girişlerinin toplam boyutu bir süre eşiğinin altına düştüğünde önbellek kullanımı düşük kabul edilir.
- Cache geri kazanma tetiklendiğinde, hedef önbellek boyutu artımlı olarak önceki boyutunun bir kesirine düşürülür ve geri kazanma yalnızca kullanım düşük kalırsa devam eder.
- Cache geri kazanma gerçekleştiğinde, çıkarmak için önbellek girdileri seçme ilkesi, bellek baskısı yüksek olduğunda sağlanan işlem veritabanları için aynı seçim ilkesidir.
- Önbellek boyutu, yapılandırılabilecek minimum sanal çekirdekler tarafından tanımlanan en düşük bellek sınırının altına hiç düşürülmez.

Hem sunucusuz hem de sağlanan işlem veritabanlarında, kullanılabilir tüm bellek kullanılıyorsa önbellek girdileri çıkartılamayabilir.

#### <a name="cache-hydration"></a>Önbellek hidrasyonu

Veriler diskten aynı şekilde ve sağlanan veritabanları için aynı hızda getirilirken, SQL önbelleği artar. Veritabanı meşgulse, önbelleğin en yüksek bellek sınırına kadar kısıtlı olarak büyümesine izin verilir.

## <a name="autopausing-and-autoresuming"></a>Oto duraklatıp ve oto sürdürülüyor

### <a name="autopausing"></a>Oto duraklatma

Aşağıdaki koşulların tümü, oto duraklatma gecikmesi süresince doğru olursa, oto duraklatma tetiklenir:

- Sayı oturumu = 0
- Kullanıcı havuzunda çalışan Kullanıcı iş yükü için CPU = 0

İstenirse, oto duraklamayı devre dışı bırakmak için bir seçenek sağlanır.

Aşağıdaki özellikler, oto duraklamayı desteklemez.  Diğer bir deyişle, aşağıdaki özelliklerden herhangi biri kullanılırsa veritabanı, etkinlik dışı kalma süresi ne olursa olsun çevrimiçi kalır:

- Coğrafi çoğaltma (etkin coğrafi çoğaltma ve otomatik yük devretme grupları).
- Uzun süreli yedek saklama (LTR).
- SQL Data Sync 'de kullanılan eşitleme veritabanı.

Veritabanının çevrimiçi olmasını gerektiren bazı hizmet güncelleştirmelerinin dağıtımı sırasında, oto duraklatma geçici olarak engellenir.  Bu gibi durumlarda, hizmet güncelleştirmesi tamamlandıktan sonra yeniden duraklatma yeniden kullanılabilir duruma gelir.

### <a name="autoresuming"></a>Oto yeniden sürdürülüyor

Aşağıdaki koşullardan herhangi biri herhangi bir zamanda doğruysa, oto yeniden sürdürme tetiklenir:

|Özellik|Oto özgeçmişi tetikleyicisi|
|---|---|
|Kimlik doğrulama ve yetkilendirme|Oturum aç|
|Tehdit algılama|Veritabanı veya sunucu düzeyinde tehdit algılama ayarlarını etkinleştirme/devre dışı bırakma.<br>Tehdit algılama ayarlarını veritabanı veya sunucu düzeyinde değiştirme.|
|Veri bulma ve sınıflandırma|Duyarlılık etiketlerini ekleme, değiştirme, silme veya görüntüleme|
|Denetim|Denetim kayıtlarını görüntüleme.<br>Denetim ilkesini güncelleştirme veya görüntüleme.|
|Veri maskeleme|Veri maskeleme kuralları ekleme, değiştirme, silme veya görüntüleme|
|Saydam veri şifrelemesi|Saydam veri şifrelemesinin durumunu veya durumunu görüntüleme|
|Sorgu (performans) veri deposu|Sorgu deposu ayarlarını değiştirme veya görüntüleme; otomatik ayarlama|
|Oto ayarlama|Otomatik Dizin oluşturma gibi otomatik ayarlama önerilerini uygulama ve doğrulama|
|Veritabanı kopyalama|Kopya olarak veritabanı oluşturun.<br>BACPAC dosyasına dışarı aktarın.|
|SQL Data Sync|Yapılandırılabilir bir zamanlamaya göre çalışan ya da el ile gerçekleştirilen merkez ve üye veritabanları arasında eşitleme|
|Belirli veritabanı meta verilerini değiştirme|Yeni veritabanı etiketleri ekleniyor.<br>Maksimum sanal çekirdekler, en az sanal çekirdek veya oto duraklatma gecikmesi değiştiriliyor.|
|SQL Server Management Studio (SSMS)|SSMS sürüm 18 ' i kullanma ve sunucudaki herhangi bir veritabanı için yeni bir sorgu penceresi açma, aynı sunucuda otomatik duraklatılmış bir veritabanını sürdürecek. IntelliSense ile SSMS sürüm 17.9.1 kullanılıyorsa bu davranış oluşmaz.|

Ayrıca, veritabanının çevrimiçi olmasını gerektiren bazı hizmet güncelleştirmelerinin dağıtımı sırasında, oto devam etme işlemi tetiklenir.

### <a name="connectivity"></a>Bağlantı

Sunucusuz bir veritabanı duraklatıldığında, ilk oturum açma işlemi veritabanını sürdürür ve 40613 hata koduyla veritabanının kullanılamadığını belirten bir hata döndürür. Veritabanı devam ettirdikten sonra, bağlantı kurmak için oturum açma yeniden denenmelidir. Bağlantı yeniden deneme mantığının bulunduğu veritabanı istemcilerinin değiştirilmesi gerekmez.

### <a name="latency"></a>Gecikme süresi

Bir sunucusuz veritabanını oto Resume ve oto duraklatma gecikmesi genellikle 1 dakikalık ve oto duraklamaya 1-10 dakika sıradır.

## <a name="onboarding-into-serverless-compute-tier"></a>Sunucusuz işlem katmanına ekleme

Yeni bir veritabanı oluşturmak veya var olan bir veritabanını sunucusuz bir işlem katmanına taşımak, sağlanan işlem katmanında yeni bir veritabanı oluşturma ile aynı kalıbı izler ve aşağıdaki iki adımı içerir.

1. Hizmet hedefi adını belirtin. Hizmet hedefi, hizmet katmanını, donanım üretimini ve maks. sanal çekirdekleri bir bütün olarak kullanıma önerir. Aşağıdaki tabloda hizmet hedefi seçenekleri gösterilmektedir:

   |Hizmet hedefi adı|Hizmet katmanı|Donanım oluşturma|En fazla sanal çekirdek|
   |---|---|---|---|
   |GP_S_Gen5_1|Genel Amaçlı|Gen5|1\.|
   |GP_S_Gen5_2|Genel Amaçlı|Gen5|2|
   |GP_S_Gen5_4|Genel Amaçlı|Gen5|4|

2. İsteğe bağlı olarak, varsayılan değerlerini değiştirmek için en düşük sanal çekirdekleri ve oto duraklatma gecikmesini belirtin. Aşağıdaki tabloda bu parametreler için kullanılabilir değerler gösterilmektedir.

   |Parametre|Değer seçimleri|Varsayılan değer|
   |---|---|---|---|
   |En düşük sanal çekirdekler|Herhangi biri {0,5, 1, 2, 4} maksimum sanal çekirdekleri aşmıyor|0,5 sanal çekirdekler|
   |Oto duraklatma gecikmesi|Minimum: 60 dakika (1 saat)<br>Maksimum: 10080 dakika (7 gün)<br>Halinde 60 dakika<br>Oto duraklamayı devre dışı bırak:-1|60 dakika|

> [!NOTE]
> Var olan bir veritabanını sunucusuz 'e taşımak veya işlem boyutunu değiştirmek için T-SQL kullanmak, şu anda desteklenmemektedir ancak Azure portal veya PowerShell aracılığıyla yapılabilir.

### <a name="create-new-database-in-serverless-compute-tier"></a>Sunucusuz işlem katmanında yeni veritabanı oluştur 

#### <a name="use-azure-portal"></a>Azure portalı kullanma

Bkz [. hızlı başlangıç: Azure SQL veritabanı 'nda Azure portal](sql-database-single-database-get-started.md)kullanarak tek bir veritabanı oluşturun.

#### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki örnek sunucusuz işlem katmanında yeni bir veritabanı oluşturur.  Bu örnek, en az sanal çekirdekler, en fazla sanal çekirdek ve oto duraklatma gecikmesini açıkça belirtir.

```powershell
New-AzSqlDatabase `
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -ComputeModel Serverless `
  -Edition GeneralPurpose `
  -ComputeGeneration Gen5 `
  -MinVcore 0.5 `
  -MaxVcore 2 `
  -AutoPauseDelayInMinutes 720
```

### <a name="move-database-from-provisioned-compute-tier-into-serverless-compute-tier"></a>Veritabanını sağlanan işlem katmanından sunucusuz işlem katmanına taşıma

#### <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki örnek, bir veritabanını sağlanan işlem katmanından sunucusuz işlem katmanına taşımakta. Bu örnek, en az sanal çekirdekler, en fazla sanal çekirdek ve oto duraklatma gecikmesini açıkça belirtir.

```powershell
Set-AzSqlDatabase
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -Edition GeneralPurpose `
  -ComputeModel Serverless `
  -ComputeGeneration Gen5 `
  -MinVcore 1 `
  -MaxVcore 4 `
  -AutoPauseDelayInMinutes 1440
```

### <a name="move-database-from-serverless-compute-tier-into-provisioned-compute-tier"></a>Veritabanını sunucusuz işlem katmanından sağlanan işlem katmanına taşıma

Bir sunucusuz veritabanı, sağlanan bir işlem veritabanını sunucusuz bir işlem katmanına taşıma ile aynı şekilde, sağlanan bir işlem katmanına taşınabilir.

## <a name="modifying-serverless-configuration"></a>Sunucusuz yapılandırmayı değiştirme

### <a name="maximum-vcores"></a>En fazla vCore

#### <a name="use-powershell"></a>PowerShell kullanma

Maksimum sanal çekirdekleri değiştirmek, PowerShell 'de `MaxVcore` bağımsız değişkeni kullanılarak [set-azsqldatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) komutu kullanılarak gerçekleştirilir.

### <a name="minimum-vcores"></a>En Az vCore

#### <a name="use-powershell"></a>PowerShell kullanma

Min sanal çekirdeklerinin değiştirilmesi, PowerShell `MinVcore` 'deki [set-azsqldatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) komutu kullanılarak bağımsız değişkeni kullanılarak gerçekleştirilir.

### <a name="autopause-delay"></a>Oto duraklatma gecikmesi

#### <a name="use-powershell"></a>PowerShell kullanma

Oto duraklatma gecikmesini değiştirmek, PowerShell 'de `AutoPauseDelayInMinutes` bağımsız değişkeni kullanılarak [set-azsqldatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) komutu kullanılarak gerçekleştirilir.

## <a name="monitoring"></a>İzleme

### <a name="resources-used-and-billed"></a>Kullanılan ve faturalandırılan kaynaklar

Sunucusuz bir veritabanının kaynakları uygulama paketi, SQL örneği ve Kullanıcı kaynak havuzu varlıkları tarafından kapsüllenir.

#### <a name="app-package"></a>Uygulama paketi

Uygulama paketi, veritabanının sunucusuz veya sağlanmış bir işlem katmanında olup olmamasına bakılmaksızın bir veritabanı için en dıştaki kaynak yönetimi sınırıdır. Uygulama paketi, SQL veritabanı 'nda bir veritabanı tarafından kullanılan tüm Kullanıcı ve sistem kaynakları kapsamındaki SQL örneğini ve dış hizmetleri içerir. Dış hizmet örnekleri R ve tam metin araması içerir. SQL örneği genellikle uygulama paketi genelinde genel kaynak kullanımını ayırır.

#### <a name="user-resource-pool"></a>Kullanıcı kaynak havuzu

Kullanıcı kaynak havuzu, veritabanının sunucusuz veya sağlanmış bir işlem katmanında olup olmamasından bağımsız olarak bir veritabanı için en çok kaynak yönetimi sınırıdır. Kullanıcı kaynak havuzu kapsamları, SELECT, INSERT, UPDATE ve DELETE gibi DDL sorguları tarafından oluşturulan kullanıcı iş yükü için CPU ve g/ç sorguları. Bu sorgular genellikle uygulama paketindeki kullanım oranının en önemli oranını temsil eder.

### <a name="metrics"></a>Ölçümler

Bir sunucusuz veritabanının uygulama paketinin ve Kullanıcı havuzunun kaynak kullanımını izlemeye yönelik ölçümler aşağıdaki tabloda listelenmiştir:

|Varlık|Ölçüm|Açıklama|Birimler|
|---|---|---|---|
|Uygulama paketi|app_cpu_percent|Uygulama tarafından, uygulama için izin verilen en fazla Vçekirdelere göre kullanılan sanal çekirdekler yüzdesi.|Yüzde|
|Uygulama paketi|app_cpu_billed|Raporlama döneminde uygulama için faturalandırılan işlem miktarı. Bu süre boyunca ödenen miktar, bu ölçümün ve vCore birim fiyatının ürünüdür. <br><br>Bu ölçümün değerleri, en fazla CPU kullanımı ve her saniye kullanılan bellek için toplanan zamana göre belirlenir. Kullanılan miktar, en düşük sanal çekirdekler ve minimum bellek tarafından ayarlanan şekilde sağlanan minimum miktardan azsa, sağlanan minimum miktar faturalandırılır. İşlemci amacıyla CPU 'yu bellek ile karşılaştırmak için, bellek miktarı GB cinsinden vCore başına 5 GB olarak yeniden ayarlayarak sanal çekirdek birimlerine normalleştirilmelidir.|Sanal çekirdek Saniyeler|
|Uygulama paketi|app_memory_percent|Uygulama tarafından uygulama için izin verilen en fazla belleğe göre kullanılan bellek yüzdesi.|Yüzde|
|Kullanıcı havuzu|cpu_percent|Kullanıcı iş yükü tarafından Kullanıcı iş yükü için izin verilen en fazla sanal çekirdeğe göre kullanılan sanal çekirdekler yüzdesi.|Yüzde|
|Kullanıcı havuzu|data_IO_percent|Kullanıcı iş yükü tarafından Kullanıcı iş yükü için izin verilen en fazla veri ıOPS 'ye göre kullanılan veri ıOPS yüzdesi.|Yüzde|
|Kullanıcı havuzu|log_IO_percent|Kullanıcı iş yükü tarafından Kullanıcı iş yükü için izin verilen en fazla günlük MB/sn 'ye göre kullanılan günlük MB/sn yüzdesi.|Yüzde|
|Kullanıcı havuzu|workers_percent|Kullanıcı iş yükü tarafından Kullanıcı iş yükü için izin verilen en fazla çalışanlara göre kullanılan çalışanların yüzdesi.|Yüzde|
|Kullanıcı havuzu|sessions_percent|Kullanıcı iş yükü tarafından Kullanıcı iş yükü için izin verilen en fazla oturumlara göre kullanılan oturumların yüzdesi.|Yüzde|

### <a name="pause-and-resume-status"></a>Durumu duraklatma ve devam etmeyi

Azure portal veritabanı durumu, içerdiği veritabanlarını listeleyen sunucunun Genel Bakış bölmesinde görüntülenir. Veritabanı durumu, veritabanının genel bakış bölmesinde de görüntülenir.

Aşağıdaki PowerShell komutunu kullanarak bir veritabanının duraklatma ve devam durumunu sorgulama:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Kaynak sınırları

Kaynak sınırları için bkz. [sunucusuz işlem katmanı](sql-database-vCore-resource-limits-single-databases.md#general-purpose-service-tier-for-serverless-compute).

## <a name="billing"></a>Faturalandırma

Faturalandırılan işlem miktarı, her saniye kullanılan en yüksek CPU ve bellek sayısıdır. Kullanılan CPU miktarı ve kullanılan bellek miktarı her biri için sağlanan minimum tutardan azsa, sağlanan miktar faturalandırılır. İşlemci amacıyla CPU 'yu bellek ile karşılaştırmak için, bellek miktarı GB cinsinden vCore başına 5 GB olarak yeniden ayarlayarak sanal çekirdek birimlerine normalleştirilmelidir.

- **Faturalandırılan kaynak**: CPU ve bellek
- **Faturalandırılan miktar**: Vcore birim fiyatı * Max (en az sanal çekirdek, sanal çekirdek, en az bellek gb * 1/3, bellek GB kullanıldı * 1/3) 
- **Faturalandırma sıklığı**: /Saniye

Saniyedeki sanal çekirdek birim fiyatı. Belirli bir bölgedeki belirli birim fiyatları için [Azure SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/single/) bakın.

Faturalandırılan işlem miktarı aşağıdaki ölçüm tarafından sunulur:

- **Ölçüm**: App_cpu_billed (sanal çekirdek saniye)
- **Tanım**: Max (min sanal çekirdekler, sanal çekirdekler, en az bellek gb * 1/3, bellek GB kullanılan * 1/3)
- **Raporlama sıklığı**: Dakika başına

Bu miktar saniyede hesaplanır ve 1 dakikadan fazla toplanır.

1 dakikalık sanal çekirdek ve 4 maks sanal çekirdeklerle yapılandırılmış sunucusuz bir veritabanını göz önünde bulundurun.  Bu, 3 GB ve daha fazla bellek ve 12 GB maksimum bellek ile aynıdır.  Otomatik duraklatma gecikmesini 6 saat olarak, veritabanı iş yükünün ise 24 saatlik bir dönemde ilk 2 saat boyunca etkin olduğunu ve aksi takdirde devre dışı olduğunu varsayalım.    

Bu durumda, veritabanı ilk 8 saat boyunca işlem ve depolama için faturalandırılır.  Veritabanı, ikinci saatten sonra devre dışı bırakılsa bile, veritabanı çevrimiçi olduğunda sağlanan minimum işlem temelinde sonraki 6 saat içinde işlem için faturalandırılır.  Veritabanı duraklatıldıktan sonra yalnızca depolama, 24 saatlik sürenin geri kalanı üzerinden faturalandırılır.

Daha kesin olarak, bu örnekteki işlem faturanız aşağıdaki gibi hesaplanır:

|Zaman Aralığı|her saniye kullanılan sanal çekirdekler|Her saniye kullanılan GB|Faturalandırılan işlem boyutu|zaman aralığı içinde faturalandırılan sanal çekirdek Saniyeler|
|---|---|---|---|---|
|0:00-1:00|4|9|kullanılan sanal çekirdekler|4 sanal çekirdek * 3600 saniye = 14400 sanal çekirdek saniye|
|1:00-2:00|1\.|12|Kullanılan bellek|12 GB * 1/3 * 3600 saniye = 14400 sanal çekirdek saniye|
|2:00-8:00|0|0|Sağlanan minimum bellek|3 GB * 1/3 * 21600 saniye = 21600 sanal çekirdek saniye|
|8:00-24:00|0|0|Durakladığında faturalandırılan işlem yok|0 sanal çekirdek saniye|
|24 saat üzerinden faturalandırılan toplam vCore saniye||||50400 sanal çekirdek saniye|

İşlem birimi fiyatının $0.000073/vCore/Second olduğunu varsayalım.  Ardından bu 24 saatlik dönem için faturalandırılan işlem, faturalandırılan işlem birimi fiyatının ve sanal çekirdek saniyenin ürünüdür: $0.000073/vCore/Second * 50400 sanal çekirdek saniyeler = $3,68

## <a name="available-regions"></a>Kullanılabilen bölgeler

Sunucusuz bilgi işlem katmanı, aşağıdaki bölgeler dışında Dünya çapında kullanılabilir: Avustralya Orta, Çin Doğu, Çin Kuzey, Fransa Güney, Almanya Orta, Almanya Kuzeydoğu, Hindistan Batı, Kore Güney, Güney Afrika Batı, UK Kuzey, UK Güney, UK Batı ve Orta Batı ABD.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz [. hızlı başlangıç: Azure SQL veritabanı 'nda Azure portal](sql-database-single-database-get-started.md)kullanarak tek bir veritabanı oluşturun.
- Kaynak sınırları için bkz. [sunucusuz işlem katmanı kaynak sınırları](sql-database-vCore-resource-limits-single-databases.md#general-purpose-service-tier-for-serverless-compute).
