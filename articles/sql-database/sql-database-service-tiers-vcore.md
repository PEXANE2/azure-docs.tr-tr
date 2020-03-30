---
title: Sanal çekirdek modeline genel bakış
description: vCore satın alma modeli, bağımsız olarak hesaplama ve depolama kaynaklarını ölçeklendirmenize, şirket içi performansı eşleştirmenize ve fiyatı optimize etmenizi sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/27/2019
ms.openlocfilehash: 5fd69dcd30292630862887ab5434764ba377b396
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481091"
---
# <a name="vcore-model-overview"></a>Sanal çekirdek modeline genel bakış

Sanal çekirdek (vCore) modeli çeşitli avantajlar sağlar:

- Daha yüksek işlem, bellek, IO ve depolama sınırları.
- İş yükünün bilgi işlem ve bellek gereksinimlerini daha iyi eşleştirmek için donanım oluşturma üzerinde denetim.
- Azure Karma [Avantajı (AHB)](sql-database-azure-hybrid-benefit.md) ve [Ayrılmış Örnek (RI)](sql-database-reserved-capacity.md)için fiyatlandırma indirimleri.
- İşleme güç veren donanım ayrıntılarında daha fazla şeffaflık; şirket içi dağıtımlardan geçişlerin planlanmasına yardımcı olur.

## <a name="service-tiers"></a>Hizmet katmanları

vCore modelindeki hizmet katmanı seçenekleri genel amaç, İş Açısından Kritik ve Hiper ölçek içerir. Hizmet katmanı genellikle depolama mimarisini, alanı ve IO sınırlarını ve kullanılabilirlik ve olağanüstü durum kurtarmayla ilgili iş sürekliliği seçeneklerini tanımlar.

||**Genel amaç**|**İş kritik**|**Hiper Ölçek**|
|---|---|---|---|
|En iyi kullanım alanı:|Çoğu iş yükü. Bütçe odaklı, dengeli ve ölçeklenebilir hesaplama ve depolama seçenekleri sunar. |İş uygulamalarına birkaç yalıtılmış yineleme kullanarak hatalara karşı en yüksek esnekliği sunar ve veritabanı yinelemesi başına en yüksek G/Ç performansını sağlar.|Yüksek ölçeklenebilir depolama ve okuma ölçeği gereksinimleri ile çoğu iş yükleri.  Birden fazla yalıtılmış veritabanı yinelemeyapılandırmasına izin vererek hatalara karşı daha yüksek esneklik sağlar. |
|Depolama|Uzak depolama alanı kullanır.<br/>**Tek veritabanları ve elastik havuzları sağlanan bilgi işlem:**<br/>5 GB – 4 TB<br/>**Sunucusuz bilgi:**<br/>5 GB - 3 TB<br/>**Yönetilen Örnek**: 32 GB - 8 TB |Yerel SSD depolama alanını kullanır.<br/>**Tek veritabanları ve elastik havuzları sağlanan bilgi işlem:**<br/>5 GB – 4 TB<br/>**Yönetilen Örnek**:<br/>32 GB - 4 TB |Gerektiğinde depolamanın esnek otomatik büyümesi. 100 TB'a kadar depolama yı destekler. Yerel arabellek havuzu önbelleği ve yerel veri depolama için yerel SSD depolama kullanır. Son uzun vadeli veri deposu olarak Azure uzak depolamayı kullanır. |
|IOPS ve iş artışı (yaklaşık)|**Tek veritabanları ve elastik havuzlar**: Tek veritabanları ve [elastik havuzlar](../sql-database/sql-database-vcore-resource-limits-elastic-pools.md)için kaynak sınırlarını görün. [single databases](../sql-database/sql-database-vcore-resource-limits-single-databases.md)<br/>**Yönetilen Örnek**: [Bkz. Genel Bakış Azure SQL Veritabanı yönetilen örnek kaynak sınırları](../sql-database/sql-database-managed-instance-resource-limits.md#service-tier-characteristics).|Tek veritabanları ve esnek [havuzlar](../sql-database/sql-database-vcore-resource-limits-single-databases.md) için kaynak [sınırlarına](../sql-database/sql-database-vcore-resource-limits-elastic-pools.md)bakın.|Hiperölçek, birden çok düzeyde önbelleğe alınmış çok katmanlı bir mimaridir. Etkili IOPS ve iş yükü iş yüküne bağlıdır.|
|Kullanılabilirlik|1 çoğaltma, hiçbir okuma ölçeği çoğaltmalar|3 çoğaltma, 1 [okuma ölçeği çoğaltma,](sql-database-read-scale-out.md)<br/>bölge yedekli yüksek kullanılabilirlik (HA)|1 okuma-yazma yinelemesi, artı 0-4 [okuma ölçeği yinelemesi](sql-database-read-scale-out.md)|
|Yedeklemeler|[Okuma-erişim coğrafi yedekli depolama (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|Azure uzak depolama alanında anlık görüntü tabanlı yedeklemeler. Geri yüklemeler, hızlı kurtarma için bu anlık görüntüleri kullanır. Yedeklemeler anlıktir ve işlem G/Ç performansını etkilemez. Geri yüklemeler hızlıdır ve veri boyutu işlemi değildir (saatler veya günler yerine dakikalar sürer).|
|Bellek içi|Desteklenmiyor|Destekleniyor|Desteklenmiyor|
|||


### <a name="choosing-a-service-tier"></a>Hizmet katmanı seçme

Belirli iş yükünüz için bir hizmet katmanı seçme hakkında bilgi için aşağıdaki makalelere bakın:

- [Genel amaçlı hizmet katmanı ne zaman seçilir?](sql-database-service-tier-general-purpose.md#when-to-choose-this-service-tier)
- [İş Açısından Kritik hizmet katmanıne ne zaman seçilir?](sql-database-service-tier-business-critical.md#when-to-choose-this-service-tier)
- [Hyperscale hizmet katmanı ne zaman seçilir?](sql-database-service-tier-hyperscale.md#who-should-consider-the-hyperscale-service-tier)


## <a name="compute-tiers"></a>İşlem katmanları

vCore modelindeki işlem katmanı seçenekleri, sağlanan ve sunucusuz bilgi işlem katmanlarını içerir.


### <a name="provisioned-compute"></a>Sağlanan işlem

Sağlanan bilgi işlem katmanı, iş yükü etkinliğinden bağımsız olarak sürekli olarak sağlanan belirli bir miktarda bilgi işlem kaynağı ve saat başına sabit bir fiyatla sağlanan işlem miktarı için faturalar sağlar.


### <a name="serverless-compute"></a>Sunucusuz işlem

[Sunucusuz bilgi işlem katmanı](sql-database-serverless.md) otomatik ölçeklendirme kaynakları iş yükü etkinliğine ve saniyede kullanılan işlem miktarına ilişkin faturaları temel alınarak ölçeklendirir.



## <a name="hardware-generations"></a>Donanım nesilleri

vCore modelinde donanım oluşturma seçenekleri Gen 4/5, M-serisi (önizleme) ve Fsv2-serisi (önizleme) içerir. Donanım oluşturma genellikle iş yükünün performansını etkileyen işlem ve bellek sınırları ve diğer özellikleri tanımlar.

### <a name="gen4gen5"></a>Gen4/Gen5

- Gen4/Gen5 donanımı dengeli bilgi işlem ve bellek kaynakları sağlar ve Fsv2 serisi veya M serisi tarafından sağlanan daha yüksek belleğe, daha yüksek vCore'a veya daha hızlı tek vCore gereksinimlerine sahip olmayan çoğu veritabanı iş yükü için uygundur.

Gen4/Gen5'in mevcut olduğu bölgeler için [Gen4/Gen5 kullanılabilirliğine](#gen4gen5-1)bakın.

### <a name="fsv2-seriespreview"></a>Fsv2 serisi (önizleme)

- Fsv2 serisi, en cpu gerektiren iş yükleri için düşük CPU gecikmesi ve yüksek saat hızı sunan bir bilgi işlem optimize donanım seçeneğidir.
- İş yüküne bağlı olarak, Fsv2 serisi vCore başına Gen5'ten daha fazla CPU performansı sunabilir ve 72 vCore boyutu Gen5'te 80 vCore'dan daha düşük maliyetle daha fazla CPU performansı sağlayabilir. 
- Fsv2 vCore başına diğer donanımlara göre daha az bellek ve tempdb sağlar, bu nedenle bu sınırlara duyarlı iş yükleri bunun yerine Gen5 veya M serisi dikkate almak isteyebilir.  

Fsv2 serisi sadece Genel Amaç katmanında desteklenir.  Fsv2 serisinin kullanılabildiği bölgeler için Bkz. [Fsv2 serisi kullanılabilirlik.](#fsv2-series)


### <a name="m-seriespreview"></a>M serisi (önizleme)

- M serisi, Gen5 tarafından sağlanandan daha fazla bellek ve daha yüksek bilgi işlem limitleri gerektiren iş yükleri için bellek için optimize edilmiş bir donanım seçeneğidir.
- M serisi vCore başına 29 GB ve 128 vCores sağlar, hangi 8x ile yaklaşık 4 TB Gen5 görebellek sınırı nı artırır.

M serisi yalnızca İş Açısından Kritik katmanında desteklenir ve bölge artıklığını desteklemez.

Bir abonelik ve bölge için M serisi donanımı etkinleştirmek için bir destek isteğinin açılması gerekir. Abonelik, Ödeme Olarak Öde veya Kurumsal Sözleşme (EA) dahil olmak üzere ücretli bir teklif türü olmalıdır.  Destek isteği onaylanırsa, M serisinin seçimi ve sağlama deneyimi diğer donanım nesilleri için olduğu gibi aynı deseni izler. M serisinin kullanılabildiği bölgeler için M [serisi kullanılabilirlik](#m-series)tarihine bakın.


### <a name="compute-and-memory-specifications"></a>İşlem ve bellek özellikleri


|Donanım üretimi  |İşlem  |Bellek  |
|:---------|:---------|:---------|
|Gen4     |- Intel E5-2673 v3 (Haswell) 2.4 GHz işlemciler<br>- 24 vCores'a kadar sağlama (1 vCore = 1 fiziksel çekirdek)  |- vCore başına 7 GB<br>- 168 GB'a kadar sağlama|
|Gen5     |**Sağlanan işlem**<br>- Intel E5-2673 v4 (Broadwell) 2.3-GHz ve Intel SP-8160 (Skylake)* işlemciler<br>- 80 vCores'a kadar sağlama (1 vCore = 1 hiper-iş parçacığı)<br><br>**Sunucusuz işlem**<br>- Intel E5-2673 v4 (Broadwell) 2.3-GHz ve Intel SP-8160 (Skylake)* işlemciler<br>- 16 vCore'a kadar otomatik ölçeklendirme (1 vCore = 1 hiper iş parçacığı)|**Sağlanan işlem**<br>- VCore başına 5,1 GB<br>- 408 GB'a kadar sağlama<br><br>**Sunucusuz işlem**<br>- VCore başına 24 GB'a kadar otomatik ölçeklendirme<br>- Maksimum 48 GB'a kadar otomatik ölçeklendirme|
|Fsv2 serisi     |- Intel Xeon Platinum 8168 (SkyLake) işlemciler<br>- 3,4 GHz'lik sürekli tüm çekirdekturbo saat hızına ve maksimum tek çekirdekli turbo saat hızına sahip 3,7 GHz.<br>- Hüküm 72 vCores (1 vCore = 1 hiper-iplik)|- vCore başına 1,9 GB<br>- Karşılık 136 GB|
|M serisi     |- Intel Xeon E7-8890 v3 2.5 GHz işlemciler<br>- Hüküm 128 vCores (1 vCore = 1 hiper-iplik)|- vCore başına 29 GB<br>- Hüküm 3.7 TB|

\*[sys.dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) dinamik yönetim görünümünde, Intel SP-8160 (Skylake) işlemcileri kullanan Gen5 veritabanları için donanım üretimi Gen6 olarak görünür. Tüm Gen5 veritabanları için kaynak sınırları işlemci türüne (Broadwell veya Skylake) bakılmaksızın aynıdır.

Kaynak sınırları hakkında daha fazla bilgi için, [tek veritabanları (vCore) veya](sql-database-vcore-resource-limits-single-databases.md) [elastik havuzlar (vCore) için Kaynak sınırları için Kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md)bakın.

### <a name="selecting-a-hardware-generation"></a>Donanım oluşturma seçeneği seçme

Azure portalında, oluşturuldukları sırada bir SQL veritabanı veya havuzu için donanım oluşturmayı seçebilir veya varolan bir SQL veritabanının veya havuzunun donanım oluşturmasını değiştirebilirsiniz.

**SQL veritabanı veya havuz oluştururken donanım oluşturma seçmek için**

Ayrıntılı bilgi için [bkz.](sql-database-single-database-get-started.md)

Temel **Bilgiler** sekmesinde, **İşlem + depolama** bölümündeki **Veritabanını Yapılanla** tonu bağlantısını seçin ve ardından **yapılandırmayı değiştir** bağlantısını seçin:

  ![veritabanı yapılandırma](media/sql-database-service-tiers-vcore/configure-sql-database.png)

İstenilen donanım oluşturma yı seçin:

  ![donanımı seçin](media/sql-database-service-tiers-vcore/select-hardware.png)


**Varolan bir SQL veritabanının veya havuzunun donanım oluşturmasını değiştirmek için**

Bir veritabanı için, Genel Bakış sayfasında **Fiyatlandırma katmanı** bağlantısını seçin:

  ![donanımı değiştirme](media/sql-database-service-tiers-vcore/change-hardware.png)

Havuz için Genel Bakış sayfasında **Yapıla'yı**seçin.

Yapılandırmayı değiştirmek için adımları izleyin ve önceki adımlarda açıklandığı gibi donanım oluşturmayı seçin.

**Yönetilen bir örnek oluştururken bir donanım oluşturma seçmek için**

Ayrıntılı bilgi için [bkz.](sql-database-managed-instance-get-started.md)

Temel **Bilgiler** sekmesinde, **İşlem + depolama** bölümündeki **Yapılveritabanı** bağlantısını seçin ve ardından istenen donanım oluşturmayı seçin:

  ![yönetilen örneği yapılandırma](media/sql-database-service-tiers-vcore/configure-managed-instance.png)
  
**Varolan yönetilen bir örneğin donanım oluşturmasını değiştirmek için**

# <a name="portal"></a>[Portal](#tab/azure-portal)

Yönetilen örnek sayfasından, Ayarlar bölümü ne sokulmuş **fiyatlandırma katmanı** bağlantısını seçin

![yönetilen örnek donanımı değiştirme](media/sql-database-service-tiers-vcore/change-managed-instance-hardware.png)

Fiyatlandırma **katmanı** sayfasında, önceki adımlarda açıklandığı gibi donanım oluşturmayı değiştirebilirsiniz.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Aşağıdaki PowerShell komut dosyasını kullanın:

```powershell-interactive
Set-AzSqlInstance -Name "managedinstance1" -ResourceGroupName "ResourceGroup01" -ComputeGeneration Gen5
```

Daha fazla bilgi için [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance) komutunu kontrol edin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki CLI komutunu kullanın:

```azurecli-interactive
az sql mi update -g mygroup -n myinstance --family Gen5
```

Daha fazla bilgi için [az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update) komutunu kontrol edin.

---

### <a name="hardware-availability"></a>Donanım kullanılabilirliği

#### <a name="gen4gen5"></a><a name="gen4gen5-1"></a>Gen4/Gen5

Gen4 donanımı [aşamalı olarak devre dışı ediliyor](https://azure.microsoft.com/updates/gen-4-hardware-on-azure-sql-database-approaching-end-of-life-in-2020/) ve yeni dağıtımlar için artık kullanılamıyor. Tüm yeni veritabanları Gen5 donanımında dağıtılmalıdır.

Gen5 dünya çapında birçok bölgede mevcuttur.

#### <a name="fsv2-series"></a>Fsv2 serisi

Fsv2 serisi aşağıdaki bölgelerde mevcuttur: Avustralya Orta, Avustralya Orta 2, Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Doğu Asya, Doğu Amerika, Fransa Orta, Hindistan Orta, Hindistan Batı, Kore Orta, Kore Güney, Kuzey Avrupa, Güney Afrika Kuzey, Güneydoğu Asya, İngiltere Güney, İngiltere Batı, Batı Avrupa, Batı Amerika 2.


#### <a name="m-series"></a>M serisi

M serisi aşağıdaki bölgelerde mevcuttur: Doğu ABD, Kuzey Avrupa, Batı Avrupa, Batı ABD 2.
M serisi ek bölgelerde de sınırlı kullanılabilirlik olabilir. Burada listelenenden farklı bir bölge isteyebilirsiniz, ancak farklı bir bölgede yerine getirilmesi mümkün olmayabilir.

Bir abonelikte M serisi kullanılabilirliğini etkinleştirmek için, [yeni bir destek isteği sunarak](#create-a-support-request-to-enable-m-series)erişim istenmelidir.


##### <a name="create-a-support-request-to-enable-m-series"></a>M-serisi etkinleştirmek için bir destek isteği oluşturun: 

1. Portalda **Yardım + destek'i** seçin.
2. **Yeni destek isteği**’ni seçin.

Temel **Bilgiler** sayfasında aşağıdakileri sağlayın:

1. **Sorun türü için**Hizmet ve abonelik **limitlerini (kotalar)** seçin.
2. **For Subscription** = M-serisi etkinleştirmek için aboneliği seçin.
3. **Kota türü için**SQL **veritabanını**seçin.
4. **Ayrıntılar** sayfasına gitmek için **İleri'yi** seçin.

**Ayrıntılar** sayfasında aşağıdakileri sağlayın:

1. PROBLEM **Detaylar** bölümünde **ayrıntıları ver** bağlantısını seçin. 
2. **SQL Veritabanı kota türü** için M **serisi**seçin.
3. **Bölge**için, M-serisini etkinleştirmek için bölgeyi seçin.
    M serisinin kullanılabildiği bölgeler için M [serisi kullanılabilirlik](#m-series)tarihine bakın.

Onaylanan destek istekleri genellikle 5 iş günü içinde yerine getirilir.


## <a name="next-steps"></a>Sonraki adımlar

- BIR SQL veritabanı oluşturmak için Azure [portalını kullanarak bir SQL veritabanı oluşturma'ya](sql-database-single-database-get-started.md)bakın.
- Tek veritabanları için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri [için, tek veritabanları için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın.
- Elastik havuzlar için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri [için, elastik havuzlar için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md)bakın.
- Fiyatlandırma ayrıntıları için [Azure SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/single/)bakın.
