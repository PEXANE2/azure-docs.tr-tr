---
title: Sunucusuz
description: Bu makalede, yeni sunucusuz bilgi işlem katmanı açıklanır ve mevcut sağlanan işlem katmanı ile karşılaştırır
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: test
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
ms.date: 3/11/2020
ms.openlocfilehash: 00b9da150569db2972289468b1405e5087ee3321
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549148"
---
# <a name="azure-sql-database-serverless"></a>Azure SQL Veritabanı sunucusuz

Azure SQL Veritabanı sunucusuz, iş yükü talebine ve saniyede kullanılan işlem miktarıiçin faturalara göre otomatik olarak hesaplatan ölçeklendirilen tek veritabanları için bir bilgi işlem katmanıdır. Sunucusuz bilgi işlem katmanı, yalnızca depolamanın faturalandırıldığu etkin olmayan dönemlerde veritabanlarını otomatik olarak duraklatır ve etkinlik döndüğünde veritabanlarını otomatik olarak devam ettirir.

## <a name="serverless-compute-tier"></a>Sunucusuz işlem katmanı

Tek bir veritabanı için sunucusuz bilgi işlem katmanı, bir bilgi işlem otomatik ölçekleme aralığı ve otomatik duraklama gecikmesi ile parametreleştirilir.  Bu parametrelerin yapılandırması veritabanı performans deneyimini ve işlem maliyetini şekillendirir.

![sunucusuz faturalandırma](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance-configuration"></a>Performans yapılandırması

- **Minimum vCores** ve **maksimum vCores** veritabanı için kullanılabilir bilgi işlem kapasitesi aralığını tanımlayan yapılandırılabilir parametrelerdir. Bellek ve IO sınırları belirtilen vCore aralığı ile orantılıdır.  
- **Otomatik duraklatma** gecikmesi, veritabanının otomatik olarak duraklatıldıktan önce devre dışı kalma süresini tanımlayan yapılandırılabilir bir parametredir. Bir sonraki oturum açma veya başka bir etkinlik gerçekleştiğinde veritabanı otomatik olarak devam ettirilir.  Alternatif olarak, otomatik otopatokullanma devre dışı bilebilir.

### <a name="cost"></a>Maliyet

- Sunucusuz bir veritabanının maliyeti, bilgi işlem maliyeti nin ve depolama maliyetinin toplamıdır.
- İşlem kullanımı yapılandırılan min ve maksimum limitler arasında olduğunda, işlem maliyeti vCore ve kullanılan belleğe dayanır.
- İşlem kullanımı yapılandırılan min limitlerinin altında olduğunda, işlem maliyeti min vCores ve min bellek yapılandırılan dayanmaktadır.
- Veritabanı duraklatıldığında, işlem maliyeti sıfırdır ve yalnızca depolama maliyetleri tahakkuk ettirilir.
- Depolama maliyeti, sağlanan işlem katmanındaki gibi belirlenir.

Daha fazla maliyet ayrıntıları için [Faturalandırma'ya](sql-database-serverless.md#billing)bakın.

## <a name="scenarios"></a>Senaryolar

Serverless, boşta kullanım dönemlerinden sonra bilgi işlem ısınmasında bazı gecikmeleri karşılayabilecek aralıklı, öngörülemeyen kullanım desenlerine sahip tek veritabanları için fiyat performansı en iyi duruma getirilmiştir. Buna karşılık, sağlanan bilgi işlem katmanı, tek veritabanları veya daha yüksek ortalama kullanımile daha yüksek ortalama kullanımile birden fazla veritabanları için optimize edilmiş fiyat performansıdır.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Sunucusuz bilgi işlem için uygun senaryolar

- Aralıklı, öngörülemeyen kullanım desenleri ile tek veritabanları hareketsizlik dönemleri ve zaman içinde daha düşük ortalama hesaplama kullanımı ile serpiştirilmiş.
- Verilen işlem katmanında sık sık yeniden ölçeklenen tek veritabanları ve hizmete yeniden işlem yeniden oluşturmayı temsilci olarak vermeyi tercih eden müşteriler.
- Bilgi işlem boyutlandırmasının zor olduğu veya SQL Veritabanı'nda dağıtımdan önce tahmin edilemeyeceğinin kullanım geçmişi olmayan yeni tek veritabanları.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Sağlanan işlem için uygun senaryolar

- Zaman içinde daha düzenli, öngörülebilir kullanım kalıpları ve daha yüksek ortalama bilgi işlem kullanımı içeren tek veritabanları.
- Daha sık bellek kırpma veya duraklatılmış bir durumdan otomatik olarak otomatik olarak geri alma gecikmesi kaynaklanan performans dengelemetotoreedilemeyen veritabanları.
- Daha iyi fiyat performansı optimizasyonu için elastik havuzlarda birlebilen aralıklı, öngörülemeyen kullanım kalıplarına sahip birden çok veritabanları.

## <a name="comparison-with-provisioned-compute-tier"></a>Sağlanan işlem katmanı ile karşılaştırma

Aşağıdaki tablo, sunucusuz bilgi işlem katmanı ile sağlanan bilgi işlem katmanı arasındaki ayrımları özetleyilmiştir:

| | **Sunucusuz işlem** | **Sağlanan işlem** |
|:---|:---|:---|
|**Veritabanı kullanım deseni**| Zaman içinde daha düşük ortalama bilgi işlem kullanımı ile aralıklı, öngörülemeyen kullanım. |  Zaman içinde daha yüksek ortalama bilgi işlem kullanımı veya elastik havuzlar kullanarak birden fazla veritabanları ile daha düzenli kullanım desenleri.|
| **Performans yönetimi çabası** |Lower|Daha yüksek|
|**Hesaplama ölçekleme**|Automatic|El ile|
|**İşlem yanıt verme**|Etkin olmayan dönemlerden sonra daha düşük|Hemen|
|**Faturalandırma parçalı**|Saniye|Saatte|

## <a name="purchasing-model-and-service-tier"></a>Satın alma modeli ve hizmet katmanı

SQL Database serverless şu anda yalnızca vCore satın alma modelinde Generation 5 donanımındaki Genel Amaç katmanında desteklenir.

## <a name="autoscaling"></a>Otomatik ölçeklendirme

### <a name="scaling-responsiveness"></a>Yanıt verme yeteneğini ölçekleme

Genel olarak, sunucusuz veritabanları, maksimum vCores değeri tarafından belirlenen sınırlar içinde talep edilen herhangi bir miktarda bilgi işlem için kesintisiz kaynak talebini karşılamak için yeterli kapasiteye sahip bir makinede çalıştırılır. Bazen, makine birkaç dakika içinde kaynak talebini karşılayamıyorsa, yük dengelemesi otomatik olarak gerçekleşir. Örneğin, kaynak talebi 4 vCores ise, ancak yalnızca 2 vCores kullanılabilir, o zaman 4 vCores sağlanmadan önce bakiye yüklemek için birkaç dakika kadar sürebilir. Bağlantı bırakıldığında, işlem sonunda ki kısa bir süre dışında, yük dengeleme sırasında veritabanı çevrimiçi kalır.

### <a name="memory-management"></a>Bellek yönetimi

Sunucusuz veritabanları için bellek, sağlanan bilgi işlem veritabanlarına göre daha sık geri alınır. Bu davranış, sunucusuz maliyetleri denetlemek için önemlidir ve performansı etkileyebilir.

#### <a name="cache-reclamation"></a>Önbellek ıslahı

Sağlanan işlem veritabanlarından farklı olarak, CPU veya önbellek kullanımı düşük olduğunda SQL önbelleğindeki bellek sunucusuz bir veritabanından geri alınır.

- En son kullanılan önbellek girişlerinin toplam boyutu belirli bir süre için bir eşiğin altına düştüğünde önbellek kullanımı düşük olarak kabul edilir.
- Önbellek ıslahı tetiklendiğinde, hedef önbellek boyutu artımlı olarak önceki boyutunun bir kısmına düşürülür ve yalnızca kullanım düşük kalırsa geri alma devam eder.
- Önbellek ıslahı gerçekleştiğinde, önbellek girişlerini çıkarma ilkesi, bellek baskısı yüksek olduğunda sağlanan işlem veritabanlarıyla aynı seçim ilkesidir.
- Önbellek boyutu, yapılandırılabilen min vCores tarafından tanımlandığı şekilde min bellek sınırının altına asla düşürülemez.

Hem sunucusuz hem de sağlanan bilgi işlem veritabanlarında, kullanılabilir tüm bellek kullanılırsa önbellek girişleri boşaltılabilir.

#### <a name="cache-hydration"></a>Önbellek hidrasyonu

Sql önbelleği, veriler diskten aynı şekilde ve sağlanan veritabanlarıyla aynı hızda alındıkça büyür. Veritabanı meşgul olduğunda, önbelleğin maksimum bellek sınırına kadar sınırlandırılmamış büyümesine izin verilir.

## <a name="autopausing-and-autoresuming"></a>Otomatik patoslama ve otomatiktamamlama

### <a name="autopausing"></a>Otomatik patokullanma

Autopause gecikmesi süresince aşağıdaki koşulların tümü doğruysa otomatik patokullanma tetiklenir:

- Sayı seansları = 0
- Kullanıcı havuzunda çalışan kullanıcı iş yükü için CPU = 0

İstenirse otomatik patosu devre dışı etmek için bir seçenek sağlanır.

Aşağıdaki özellikler otomatik otopatokullanmayı desteklemez.  Diğer bir deyişle, aşağıdaki özelliklerden herhangi biri kullanılırsa, veritabanı etkinlik süresine bakılmaksızın veritabanı çevrimiçi kalır:

- Coğrafi çoğaltma (etkin coğrafi çoğaltma ve otomatik arıza grupları).
- Uzun süreli yedekleme tutma (LTR).
- SQL veri eşitleme kullanılan eşitleme veritabanı.  Eşitleme veritabanlarının aksine hub ve üye veritabanları otomatik patosu destekler.
- Elastik işlerde kullanılan iş veritabanı.

Veritabanının çevrimiçi olmasını gerektiren bazı hizmet güncelleştirmelerinin dağıtımı sırasında otomatik patokullanma geçici olarak engellenir.  Bu gibi durumlarda, hizmet güncelleştirmesi tamamlandıktan sonra otomatik kullanıma tekrar izin verilir.

### <a name="autoresuming"></a>Otomatik tamamlama

Aşağıdaki koşullardan herhangi biri herhangi bir zamanda doğruysa otomatik tamamlama tetiklenir:

|Özellik|Otomatik başlatma tetikleyicisi|
|---|---|
|Kimlik doğrulama ve yetkilendirme|Oturum Aç|
|Tehdit algılama|Veritabanı veya sunucu düzeyinde tehdit algılama ayarlarını etkinleştirme/devre dışı bırakma.<br>Veritabanı veya sunucu düzeyinde tehdit algılama ayarlarını değiştirme.|
|Veri bulma ve sınıflandırma|Duyarlılık etiketleri ekleme, değiştirme, silme veya görüntüleme|
|Denetim|Denetim kayıtlarını görüntüleme.<br>Denetim ilkesini güncelleştirme veya görüntüleme.|
|Veri maskeleme|Veri maskeleme kuralları ekleme, değiştirme, silme veya görüntüleme|
|Saydam veri şifrelemesi|Saydam veri şifreleme durumunu veya durumunu görüntüleme|
|Sorgu (performans) veri deposu|Sorgu deposu ayarlarını değiştirme veya görüntüleme|
|Otomatik tuning|Otomatik dizin oluşturma gibi otomatik dizinoluşturma önerilerinin uygulanması ve doğrulanması|
|Veritabanı kopyalama|Veritabanını kopya olarak oluşturun.<br>BACPAC dosyasına dışa aktarın.|
|SQL veri eşitleme|Hub ve üye veritabanları arasında yapılandırılabilir bir zamanlamada çalışan veya el ile gerçekleştirilen eşitleme|
|Belirli veritabanı meta verilerini değiştirme|Yeni veritabanı etiketleri ekleme.<br>Maksimum vCores, min vCores veya otomatik duraklatma gecikme değiştirme.|
|SQL Server Management Studio (SSMS)|18.1'den önce SSMS sürümlerini kullanmak ve sunucudaki herhangi bir veritabanı için yeni bir sorgu penceresi açmak, aynı sunucudaki otomatik duraklatılmış veritabanını devam ettirir. Bu davranış, SSMS sürüm 18.1 veya daha sonra kullanıyorsanız oluşmaz.|

Otomatik başlatma, veritabanının çevrimiçi olmasını gerektiren bazı hizmet güncelleştirmelerinin dağıtımı sırasında da tetiklenir.

### <a name="connectivity"></a>Bağlantı

Sunucusuz bir veritabanı duraklatılırsa, ilk giriş veritabanını devam ettirecek ve veritabanının hata kodu 40613 ile kullanılanınolmadığını belirten bir hata döndürecektir. Veritabanı na devam edildikten sonra, bağlantı kurmak için giriş yeniden denenmelidir. Bağlantı yeniden deneme mantığına sahip veritabanı istemcilerinin değiştirilmesi gerekmez.

### <a name="latency"></a>Gecikme süresi

Sunucusuz bir veritabanını otomatik olarak otomatik olarak devam ettirmek ve otomatik duraklatmak için gecikme genellikle otomatik devam etmek için 1 dakika ve otomatik duraklatmak için 1-10 dakika sıralanır.

### <a name="customer-managed-transparent-data-encryption-byok"></a>Müşteri tarafından yönetilen şeffaf veri şifreleme (BYOK)

Müşteri [tarafından yönetilen saydam veri şifreleme](transparent-data-encryption-byok-azure-sql.md) (BYOK) ve sunucusuz veritabanı anahtar silme veya iptal gerçekleştiğinde otomatik olarak duraklatılmışsa, veritabanı otomatik duraklatma durumunda kalır.  Bu durumda, veritabanı sonraki devam edildikten sonra, veritabanına yaklaşık 10 dakika içinde erişilemez hale gelir.  Veritabanına erişilemiyor olduğunda, kurtarma işlemi, sağlanan işlem veritabanlarıyla aynıdır.  Anahtar silme veya iptal gerçekleştiğinde sunucusuz veritabanı çevrimiçiyse, veritabanına yaklaşık 10 dakika veya daha kısa bir süre sonra, sağlanan bilgi işlem veritabanlarında olduğu gibi erişilemez hale gelir.

## <a name="onboarding-into-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanına binme

Yeni bir veritabanı oluşturma veya varolan bir veritabanını sunucusuz bir bilgi işlem katmanına taşıma, verilen işlem katmanında yeni bir veritabanı oluşturmakla aynı deseni izler ve aşağıdaki iki adımı içerir.

1. Hizmet hedef adını belirtin. Hizmet hedefi, hizmet katmanı, donanım oluşturma ve maksimum vCores reçete. Aşağıdaki tabloda hizmet amacı seçenekleri gösterilmektedir:

   |Hizmet hedef adı|Hizmet katmanı|Donanım üretimi|Maksimum vCores|
   |---|---|---|---|
   |GP_S_Gen5_1|Genel Amaçlı|Gen5|1|
   |GP_S_Gen5_2|Genel Amaçlı|Gen5|2|
   |GP_S_Gen5_4|Genel Amaçlı|Gen5|4|
   |GP_S_Gen5_6|Genel Amaçlı|Gen5|6|
   |GP_S_Gen5_8|Genel Amaçlı|Gen5|8|
   |GP_S_Gen5_10|Genel Amaçlı|Gen5|10|
   |GP_S_Gen5_12|Genel Amaçlı|Gen5|12|
   |GP_S_Gen5_14|Genel Amaçlı|Gen5|14|
   |GP_S_Gen5_16|Genel Amaçlı|Gen5|16|

2. İsteğe bağlı olarak, varsayılan değerlerini değiştirmek için min vCores ve autopause gecikmesini belirtin. Aşağıdaki tabloda bu parametreler için kullanılabilir değerleri gösterilmektedir.

   |Parametre|Değer seçenekleri|Varsayılan değer|
   |---|---|---|---|
   |Min vCores|Yapılandırılan maksimum vCores bağlıdır - [kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5)bakın.|0,5 vCores|
   |Otomatik duraklatma gecikmesi|Minimum: 60 dakika (1 saat)<br>Maksimum: 10080 dakika (7 gün)<br>Artışlar: 60 dakika<br>Otomatik duraklat'ı devre dışı: -1|60 dakika|


### <a name="create-new-database-in-serverless-compute-tier"></a>Sunucusuz bilgi işlem katmanında yeni veritabanı oluşturma 

Aşağıdaki örnekler, sunucusuz bilgi işlem katmanında yeni bir veritabanı oluşturur. Örnekler açıkça min vCores, max vCores ve otomatik duraklatma gecikme belirtin.

#### <a name="use-azure-portal"></a>Azure portalı kullanma

Bkz. [Hızlı Başlangıç: Azure portalını kullanarak Azure SQL Veritabanı'nda tek bir veritabanı oluşturun.](sql-database-single-database-get-started.md)


#### <a name="use-powershell"></a>PowerShell kullanma

```powershell
New-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
  -ComputeModel Serverless -Edition GeneralPurpose -ComputeGeneration Gen5 `
  -MinVcore 0.5 -MaxVcore 2 -AutoPauseDelayInMinutes 720
```
#### <a name="use-azure-cli"></a>Azure CLI kullanma

```azurecli
az sql db create -g $resourceGroupName -s $serverName -n $databaseName `
  -e GeneralPurpose -f Gen5 -min-capacity 0.5 -c 2 --compute-model Serverless --auto-pause-delay 720
```


#### <a name="use-transact-sql-t-sql"></a>Transact-SQL (T-SQL) kullanın

Aşağıdaki örnek, sunucusuz bilgi işlem katmanında yeni bir veritabanı oluşturur.

```sql
CREATE DATABASE testdb
( EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_S_Gen5_1' ) ;
```

Ayrıntılar için CREATE [DATABASE'e](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current)bakın.  

### <a name="move-database-from-provisioned-compute-tier-into-serverless-compute-tier"></a>Veritabanını sağlanan bilgi işlem katmanından sunucusuz bilgi işlem katmanına taşıma

Aşağıdaki örnekler, bir veritabanını sağlanan bilgi işlem katmanından sunucusuz bilgi işlem katmanına taşır. Örnekler açıkça min vCores, max vCores ve otomatik duraklatma gecikme belirtin.

#### <a name="use-powershell"></a>PowerShell kullanma


```powershell
Set-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
  -Edition GeneralPurpose -ComputeModel Serverless -ComputeGeneration Gen5 `
  -MinVcore 1 -MaxVcore 4 -AutoPauseDelayInMinutes 1440
```

#### <a name="use-azure-cli"></a>Azure CLI kullanma

```azurecli
az sql db update -g $resourceGroupName -s $serverName -n $databaseName `
  --edition GeneralPurpose --min-capacity 1 --capacity 4 --family Gen5 --compute-model Serverless --auto-pause-delay 1440
```


#### <a name="use-transact-sql-t-sql"></a>Transact-SQL (T-SQL) kullanın

Aşağıdaki örnek, bir veritabanını sağlanan bilgi işlem katmanından sunucusuz bilgi işlem katmanına taşır.

```sql
ALTER DATABASE testdb 
MODIFY ( SERVICE_OBJECTIVE = 'GP_S_Gen5_1') ;
```

Ayrıntılar için [ALTER DATABASE'e](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current)bakın.

### <a name="move-database-from-serverless-compute-tier-into-provisioned-compute-tier"></a>Veritabanını sunucusuz bilgi işlem katmanından sağlanan bilgi işlem katmanına taşıma

Sunucusuz bir veritabanı, sağlanan bir bilgi işlem veritabanını sunucusuz bir bilgi işlem katmanına taşımakla aynı şekilde, sağlanan bir bilgi işlem katmanına taşınabilir bir bilgi işlem katmanına taşınabilir.

## <a name="modifying-serverless-configuration"></a>Sunucusuz yapılandırmayı değiştirme

### <a name="use-powershell"></a>PowerShell kullanma

Maksimum veya minimum vCores ve otomatik duraklatma gecikmesi değiştirme, PowerShell'de [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) komutu kullanılarak , `MaxVcore`ve `MinVcore` `AutoPauseDelayInMinutes` bağımsız değişkenler kullanılarak gerçekleştirilir.

### <a name="use-azure-cli"></a>Azure CLI kullanma

Maksimum veya minimum vCores ve otomatik duraklatma gecikmesi, Azure CLI az [sql db güncelleştirme](/cli/azure/sql/db#az-sql-db-update) komutu kullanılarak , `capacity`ve `min-capacity` `auto-pause-delay` bağımsız değişkenler kullanılarak gerçekleştirilir.


## <a name="monitoring"></a>İzleme

### <a name="resources-used-and-billed"></a>Kullanılan ve faturalanan kaynaklar

Sunucusuz bir veritabanının kaynakları uygulama paketi, SQL örneği ve kullanıcı kaynağı havuzu varlıkları tarafından kapsüllenir.

#### <a name="app-package"></a>Uygulama paketi

Uygulama paketi, veritabanının sunucusuz veya hükümlü bir bilgi işlem katmanında olup olmadığına bakılmaksızın, veritabanı için en dış kaynak yönetimi sınırıdır. Uygulama paketi, SQL Veritabanı'ndaki bir veritabanı tarafından kullanılan tüm kullanıcı ve sistem kaynaklarını birlikte içeren SQL örneğini ve harici hizmetleri içerir. Dış hizmetlere örnek olarak R ve tam metin arama verilebilir. SQL örneği genellikle uygulama paketi genelinde genel kaynak kullanımı hakimdir.

#### <a name="user-resource-pool"></a>Kullanıcı kaynak havuzu

Kullanıcı kaynak havuzu, veritabanının sunucusuz veya uygun bir bilgi işlem katmanında olup olmadığına bakılmaksızın, veritabanı için iç en çok kaynak yönetim sınırıdır. Kullanıcı kaynak havuzu, CREATE ve ALTER ve SELECT, INSERT, UPDATE ve DELETE gibi DML sorguları tarafından oluşturulan kullanıcı iş yükü için CPU ve IO'yu scopes. Bu sorgular genellikle uygulama paketi içindeki en önemli kullanım oranını temsil eder.

### <a name="metrics"></a>Ölçümler

Uygulama paketinin ve sunucusuz bir veritabanının kullanıcı havuzunun kaynak kullanımını izlemek için ölçümler aşağıdaki tabloda listelenir:

|Varlık|Ölçüm|Açıklama|Birimler|
|---|---|---|---|
|Uygulama paketi|app_cpu_percent|Uygulama için izin verilen max vCores göre uygulama tarafından kullanılan vCores yüzdesi.|Yüzde|
|Uygulama paketi|app_cpu_billed|Raporlama döneminde uygulama için faturalanan işlem tutarı. Bu dönemde ödenen tutar, bu ölçümün ürünü dür ve vCore birim fiyatıdır. <br><br>Bu metriğin değerleri, zaman içinde kullanılan en fazla CPU ve her saniye kullanılan bellek toplayarak belirlenir. Kullanılan tutar, min vCores ve min bellek tarafından belirlenen minimum miktardan azsa, sağlanan minimum tutar faturalandırılır.CPU'yu faturalandırma amacıyla bellekle karşılaştırmak için, bellek, GB'deki bellek miktarını vCore başına 3 GB'a yeniden ölçeklandırarak vCore birimlerine normalleştirilir.|vCore saniye|
|Uygulama paketi|app_memory_percent|Uygulama tarafından kullanılan bellek yüzdesi, uygulama için izin verilen maksimum belleğe göre.|Yüzde|
|Kullanıcı havuzu|cpu_percent|Kullanıcı iş yükü için izin verilen maksimum vCores göreizkullanıcı iş yükü tarafından kullanılan vCores yüzdesi.|Yüzde|
|Kullanıcı havuzu|data_IO_percent|Kullanıcı iş yükü tarafından kullanılan veri IOPS yüzdesi, kullanıcı iş yükü için izin verilen maksimum veri IOPS göreli.|Yüzde|
|Kullanıcı havuzu|log_IO_percent|Kullanıcı iş yükü için izin verilen maksimum günlük MB/s göreli kullanıcı iş yükü tarafından kullanılan günlük MB/s yüzdesi.|Yüzde|
|Kullanıcı havuzu|workers_percent|Kullanıcı iş yükü için izin verilen maksimum işçilere göre kullanıcı iş yükü tarafından kullanılan işçilerin yüzdesi.|Yüzde|
|Kullanıcı havuzu|sessions_percent|Kullanıcı iş yükü tarafından kullanılan oturumların yüzdesi, kullanıcı iş yükü için izin verilen maksimum oturumlara göre.|Yüzde|

### <a name="pause-and-resume-status"></a>Duraklatma ve devam durumu

Azure portalında veritabanı durumu, içerdiği veritabanlarını listeleyen sunucunun genel bakış bölmesinde görüntülenir. Veritabanı durumu, veritabanı için genel bakış bölmesinde de görüntülenir.

Veritabanının duraklatma ve devam durumunu sorgulamak için aşağıdaki komutları kullanma:

#### <a name="use-powershell"></a>PowerShell kullanma

```powershell
Get-AzSqlDatabase -ResourceGroupName $resourcegroupname -ServerName $servername -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

#### <a name="use-azure-cli"></a>Azure CLI kullanma

```azurecli
az sql db show --name $databasename --resource-group $resourcegroupname --server $servername --query 'status' -o json
```


## <a name="resource-limits"></a>Kaynak sınırları

Kaynak sınırları için [sunucusuz bilgi işlem katmanına](sql-database-vCore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5)bakın.

## <a name="billing"></a>Faturalandırma

Faturalanan işlem miktarı, kullanılan cpu ve her saniye kullanılan bellek maksimum. Kullanılan CPU miktarı ve kullanılan bellek her biri için sağlanan minimum miktardan azsa, sağlanan tutar faturalandırılır. CPU'yu faturalandırma amacıyla bellekle karşılaştırmak için, bellek, GB'deki bellek miktarını vCore başına 3 GB'a yeniden ölçeklandırarak vCore birimlerine normalleştirilir.

- **Faturalanan kaynak**: CPU ve bellek
- **Faturalanan miktar**: vCore birim fiyatı * max (min vCores, kullanılan vCores, min bellek GB * 1/3, bellek GB kullanılan * 1/3) 
- **Faturalama sıklığı**: Saniye

vCore birim fiyatı saniye başına vCore başına maliyettir. Belirli bir bölgedeki belirli birim fiyatlar için [Azure SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/single/) bakın.

Faturalanan işlem miktarı aşağıdaki metrik tarafından ortaya çıkarır:

- **Metrik**: app_cpu_billed (vCore saniye)
- **Tanım**: max (min vCores, kullanılan vCores, min bellek GB * 1/3, bellek GB kullanılan * 1/3)
- **Raporlama sıklığı**: Dakika başına

Bu miktar her saniye hesaplanır ve 1 dakika içinde toplanır.

1 dk vCore ve 4 max vCore si ile yapılandırılan sunucusuz bir veritabanı düşünün.  Bu yaklaşık 3 GB dk bellek ve 12 GB max belleğe karşılık gelir.  Otomatik duraklatma gecikmesinin 6 saat olarak ayarlanır ve veritabanı iş yükünün 24 saatlik bir sürenin ilk 2 saatinde etkin olduğunu ve bunun dışında etkin olmadığını varsayalım.    

Bu durumda, veritabanı ilk 8 saat içinde işlem ve depolama için faturalandırılır.  Veritabanı ikinci saatten itibaren etkin olmasa da, veritabanı çevrimiçiyken sağlanan minimum işlem temel alınarak sonraki 6 saat içinde işlem için faturalandırılır.  Veritabanı duraklatılmışken yalnızca depolama alanı 24 saatlik sürenin geri kalanında faturalandırılır.

Daha doğrusu, bu örnekteki işlem faturası aşağıdaki gibi hesaplanır:

|Zaman Aralığı|vCores her saniye kullanılan|GB her saniye kullanılır|Faturalı işlem boyutu|vCore saniye zaman aralığı içinde fatura|
|---|---|---|---|---|
|0:00-1:00|4|9|kullanılan vCores|4 vCores * 3600 saniye = 14400 vCore saniye|
|1:00-2:00|1|12|Kullanılan bellek|12 GB * 1/3 * 3600 saniye = 14400 vCore saniye|
|2:00-8:00|0|0|Min bellek sağlanmış|3 GB * 1/3 * 21600 saniye = 21600 vCore saniye|
|8:00-24:00|0|0|Duraklatılmış iken faturalandırılmama|0 vCore saniye|
|24 saat içinde faturalanan toplam vCore saniye||||50400 vCore saniye|

İşlem birim fiyatının $0.000145/vCore/second olduğunu varsayalım.  Sonra bu 24 saatlik dönem için fatura hesaplama işlem birimi fiyat ve vCore saniye fatura ürünüdür: $ 0.000145/vCore /second * 50400 vCore saniye ~ 7,31 $

### <a name="azure-hybrid-benefit-and-reserved-capacity"></a>Azure Karma Avantajı ve ayrılmış kapasite

Azure Karma Avantajı (AHB) ve ayrılmış kapasite indirimleri sunucusuz bilgi işlem katmanı için geçerli değildir.

## <a name="available-regions"></a>Kullanılabilir bölgeler

Sunucusuz bilgi işlem katmanı aşağıdaki bölgeler dışında dünya çapında kullanılabilir: Çin Doğu, Çin Kuzey, Almanya Orta, Almanya Kuzeydoğu, İngiltere Kuzey, İngiltere Güney 2, Batı Orta ABD ve US Gov Central (Iowa).

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için [Bkz. Quickstart: Azure portalını kullanarak Azure SQL Veritabanı'nda tek bir veritabanı oluşturun.](sql-database-single-database-get-started.md)
- Kaynak sınırları için Bkz. [Serverless bilgi işlem katmanı kaynak sınırları.](sql-database-vCore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5)
