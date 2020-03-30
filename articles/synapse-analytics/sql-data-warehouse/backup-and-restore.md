---
title: Yedekleme ve geri yükleme - anlık görüntüler, coğrafi yedekli
description: Azure Synapse Analytics SQL havuzunda yedekleme ve geri yüklemenin nasıl çalıştığını öğrenin. Veri ambarınızı birincil bölgedeki bir geri yükleme noktasına geri yüklemek için yedeklemeleri kullanın. Farklı bir coğrafi bölgeye geri yüklemek için coğrafi yedekli yedeklemeleri kullanın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/04/2020
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019"
ms.openlocfilehash: ae53380572e753a8bcfa20fcd165fa015766263e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349263"
---
# <a name="backup-and-restore-in-azure-synapse-sql-pool"></a>Azure Synapse SQL havuzunda yedekleme ve geri yükleme

Azure Synapse SQL havuzunda yedekleme ve geri yükleme yi nasıl kullanacağınızı öğrenin. Veri ambarınızı kurtarmak veya birincil bölgedeki önceki bir duruma kopyalamak için SQL havuzu geri yükleme noktalarını kullanın. Farklı bir coğrafi bölgeye geri yüklemek için veri ambarı coğrafi yedekli yedeklemeleri kullanın.

## <a name="what-is-a-data-warehouse-snapshot"></a>Veri ambarı anlık görüntüsü nedir

*Veri ambarı anlık görüntüsü,* veri ambarınızı kurtarmak veya önceki duruma kopyalamak için kullanabileceğiniz bir geri yükleme noktası oluşturur.  SQL havuzu dağıtılmış bir sistem olduğundan, veri ambarı anlık görüntüsü Azure depolama alanında bulunan birçok dosyadan oluşur. Anlık görüntüler, veri ambarınızda depolanan verilerden artan değişiklikleri yakalar.

*Veri ambarı geri* yüklemesi, varolan veya silinen bir veri ambarının geri yükleme noktasından oluşturulan yeni bir veri ambarıdır. Veri ambarınızı geri geri, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır, çünkü yanlışlıkla oluşan bozulma veya silme işleminden sonra verilerinizi yeniden oluşturur. Veri ambarı, test veya geliştirme amacıyla veri ambarınızın kopyalarını oluşturmak için de güçlü bir mekanizmadır.  SQL havuzu geri yükleme oranları, kaynak ve hedef veri ambarının veritabanı boyutuna ve konumuna bağlı olarak değişebilir. 

## <a name="automatic-restore-points"></a>Otomatik Geri Yükleme Noktaları

Anlık görüntüler, hizmetin geri yükleme noktaları oluşturan yerleşik bir özelliğidir. Bu özelliği etkinleştirmeniz gerekmez. Ancak, SQL havuzu geri yükleme noktası oluşturma için etkin bir durumda olmalıdır. SQL havuzu sık sık duraklatıldıysa, otomatik geri yükleme noktaları oluşturulamayabilir, bu nedenle SQL havuzunu duraklatmadan önce kullanıcı tanımlı geri yükleme noktası oluşturduğunuzdan emin olun. Hizmet kurtarma için SLA'ları korumak için bu geri yükleme noktalarını kullandığından, otomatik geri yükleme noktaları şu anda kullanıcılar tarafından silinemez.

Veri ambarınızın anlık görüntüleri gün boyunca alınır ve yedi gün boyunca kullanılabilir geri yükleme noktaları oluşturulur. Bu bekletme süresi değiştirilemez. SQL havuzu sekiz saatlik kurtarma noktası hedefini (RPO) destekler. Birincil bölgedeki veri ambarınızı, son yedi gün içinde çekilen anlık görüntülerden herhangi birinden geri yükleyebilirsiniz.

Son anlık görüntünün ne zaman başladığını görmek için bu sorguyu çevrimiçi SQL havuzunuzda çalıştırın.

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs
order by run_id desc
;
```

## <a name="user-defined-restore-points"></a>Kullanıcı Tanımlı Geri Yükleme Noktaları

Bu özellik, büyük değişikliklerden önce ve sonra veri ambarınızın geri yükleme noktalarını oluşturmak için anlık görüntüleri el ile tetiklemenizi sağlar. Bu özellik, geri yükleme noktalarının mantıksal olarak tutarlı olmasını sağlar ve bu da hızlı kurtarma süresi için iş yükü kesintileri veya kullanıcı hataları durumunda ek veri koruması sağlar. Kullanıcı tanımlı geri yükleme noktaları yedi gün boyunca kullanılabilir ve sizin adınıza otomatik olarak silinir. Kullanıcı tanımlı geri yükleme noktalarının bekletme süresini değiştiremezsiniz. **42 kullanıcı tanımlı geri yükleme noktası,** başka bir geri yükleme noktası oluşturmadan önce [silinmesi](https://go.microsoft.com/fwlink/?linkid=875299) gerektiğinden, herhangi bir zamanda garanti edilir. [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint#examples) veya Azure portalı aracılığıyla kullanıcı tanımlı geri yükleme noktaları oluşturmak için anlık görüntüleri tetikleyebilirsiniz.

> [!NOTE]
> Eğer 7 günden daha uzun geri yükleme noktaları gerekiyorsa, [burada](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/35114410-user-defined-retention-periods-for-restore-points)bu yetenek için oy lütfen. Ayrıca, kullanıcı tanımlı bir geri yükleme noktası oluşturabilir ve yeni oluşturulan geri yükleme noktasından yeni bir veri ambarına geri yükleyebilirsiniz. Geri yüklendikten sonra, ÇEVRIMIÇI SQL havuzuna sahipsiniz ve işlem maliyetlerinden tasarruf etmek için bu havuzu süresiz olarak duraklatabilirsiniz. Duraklatılmış veritabanı, Azure Premium Depolama fiyatından depolama ücretlerine neden olabilir. Geri yüklenen veri ambarının etkin bir kopyasına ihtiyacınız varsa, yalnızca birkaç dakika sürecek şekilde devam edebilirsiniz.

### <a name="restore-point-retention"></a>Nokta tutma geri yükleme

Geri yükleme noktası bekletme dönemleri için aşağıdaki ayrıntıları listeler:

1. SQL havuzu, 7 günlük bekletme dönemine ulaştığında **ve** en az 42 toplam geri yükleme noktası olduğunda (hem kullanıcı tanımlı hem de otomatik dahil) bir geri yükleme noktasını siler.
2. SQL havuzu duraklatıldığında anlık görüntüler alınmaz.
3. Geri yükleme noktasının yaşı, SQL havuzunun duraklatılattığı zaman da dahil olmak üzere geri yükleme noktasının alındığı tarihten itibaren mutlak takvim günsayısıyla ölçülür.
4. Herhangi bir zamanda, bu geri yükleme noktaları 7 günlük saklama süresine ulaşmadığı sürece, bir SQL havuzunun 42 kullanıcı tanımlı geri yükleme noktası ve 42 otomatik geri yükleme noktasını depolayabilme garantisi verilir
5. Anlık görüntü alınırsa, SQL havuzu 7 günden uzun süre duraklatıldı ve ardından devam eder, geri yükleme noktası 42 toplam geri yükleme noktası (hem kullanıcı tanımlı hem de otomatik dahil) olana kadar devam eder.

### <a name="snapshot-retention-when-a-sql-pool-is-dropped"></a>SQL havuzu bırakıldığında anlık görüntü tutma

Bir SQL havuzu bıraktığınızda, son bir anlık görüntü oluşturulur ve yedi gün boyunca kaydedilir. SQL havuzunu silme işleminde oluşturulan son geri yükleme noktasına geri yükleyebilirsiniz. SQL havuzu duraklatılmış durumda bırakılırsa, anlık görüntü alınmaz. Bu senaryoda, SQL havuzunu bırakmadan önce kullanıcı tanımlı bir geri yükleme noktası oluşturduğunuzdan emin olun.

> [!IMPORTANT]
> Mantıksal bir SQL sunucu örneğini silerseniz, örne ait tüm veritabanları da silinir ve kurtarılamaz. Silinen bir sunucugeri yükleyemezsiniz.

## <a name="geo-backups-and-disaster-recovery"></a>Coğrafi yedeklemeler ve olağanüstü durum kurtarma

[Eşli](../../best-practices-availability-paired-regions.md)bir veri merkezine günde bir kez coğrafi yedekleme oluşturulur. Coğrafi geri yükleme için RPO 24 saattir. Coğrafi yedeklemeyi SQL havuzunun desteklendiği başka bir bölgedeki bir sunucuya geri yükleyebilirsiniz. Coğrafi yedekleme, birincil bölgenizdeki geri yükleme noktalarına erişemiyorsanız veri ambarını geri yüklemenizi sağlar.

> [!NOTE]
> Coğrafi yedeklemeler için daha kısa bir RPO'ya ihtiyacınız varsa, [burada](https://feedback.azure.com/forums/307516-sql-data-warehouse)bu kapasiteye oy verin. Ayrıca, kullanıcı tanımlı bir geri yükleme noktası oluşturabilir ve yeni oluşturulan geri yükleme noktasından farklı bir bölgedeki yeni bir veri ambarına geri yükleyebilirsiniz. Geri yükledikten sonra, veri ambarı çevrimiçi olur ve işlem maliyetlerinden tasarruf etmek için veri ambarını süresiz olarak duraklatabilirsiniz. Duraklatılmış veritabanı, Azure Premium Depolama fiyatından depolama ücretlerine neden olabilir. Veri ambarının etkin bir kopyasına ihtiyacınız olursa, yalnızca birkaç dakika sürecek şekilde devam edebilirsiniz.

## <a name="backup-and-restore-costs"></a>Yedekleme ve geri yükleme maliyetleri

Azure faturasında Depolama için bir satır öğesi ve Olağanüstü Durum Kurtarma Depolama için bir satır öğesi olduğunu fark edeceksiniz. Depolama ücreti, anlık görüntüler tarafından yakalanan artımlı değişikliklerle birlikte verilerinizi birincil bölgede depolamanın toplam maliyetidir. Anlık görüntünün nasıl ücretlendirildiğini daha ayrıntılı bir açıklama için, [Anlık Görüntülerin Ücretleri Nasıl Tahakkuk Ettirğini Anlama](https://docs.microsoft.com/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges?redirectedfrom=MSDN#snapshot-billing-scenarios)bölümüne bakın. Coğrafi yedekli ücret, coğrafi yedeklemelerin depolanması maliyetini kapsar.  

Birincil veri ambarınızın toplam maliyeti ve yedi günlük anlık görüntü değişiklikleri en yakın TB'ye yuvarlanır. Örneğin, veri ambarınız 1,5 TB ise ve anlık görüntüler 100 GB'ı yakalarsa, Azure Premium Depolama fiyatlarıyla 2 TB veri için faturalandırılırsınız.

Coğrafi yedekli depolama alanı kullanıyorsanız, ayrı bir depolama ücreti alırsınız. Coğrafi yedekdepolama, standart Okuma-Erişim Coğrafi Olarak Yedekdepolama (RA-GRS) oranında faturalandırılır.

Azure Synapse fiyatlandırması hakkında daha fazla bilgi için Azure [Synapse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/)fiyatlandırması'na bakın. Bölgeler arasında geri alırken veri çıkış için ücretlendirilmezsiniz.

## <a name="restoring-from-restore-points"></a>Geri yükleme noktalarından geri yükleme

Her anlık görüntü, anlık görüntünün başladığı zamanı gösteren bir geri yükleme noktası oluşturur. Veri ambarı geri yüklemek için bir geri yükleme noktası seçin ve bir geri yükleme komutu sorun.  

Geri yüklenen veri ambarını ve geçerli sini tutabilir veya bunlardan birini silebilirsiniz. Geçerli veri ambarını geri yüklenen veri ambarıyla değiştirmek istiyorsanız, [ALTER DATABASE (SQL pool)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) kullanarak değiştir Inadı seçeneğiyle yeniden adlandırabilirsiniz.

Veri ambarı geri yüklemek için [bkz.](sql-data-warehouse-restore-points.md#create-user-defined-restore-points-through-the-azure-portal)

Silinmiş veya duraklatılmış veri ambarı geri yüklemek için [bir destek bileti oluşturabilirsiniz.](sql-data-warehouse-get-started-create-support-ticket.md)

## <a name="cross-subscription-restore"></a>Çapraz abonelik geri yükleme

Abonelik genelinde doğrudan geri yüklemeniz gerekiyorsa, [burada](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/36256231-enable-support-for-cross-subscription-restore)bu yetenek için oy verin. Çapraz abonelik geri yüklemesi gerçekleştirmek için farklı bir mantıksal sunucuya geri yükleyin ve sunucuyu abonelikler arasında ['Taşı'](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources) edin. 

## <a name="geo-redundant-restore"></a>Coğrafi yedekli geri yükleme

Seçtiğiniz performans düzeyinde SQL havuzu destekleyen herhangi bir bölgeye [SQL havuzu geri yükleyebilirsiniz.](sql-data-warehouse-restore-from-geo-backup.md#restore-from-an-azure-geographical-region-through-powershell)

> [!NOTE]
> Coğrafi yedekli bir geri yükleme gerçekleştirmek için bu özelliği devre dışı bırakmamış olmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Afet planlaması hakkında daha fazla bilgi için [bkz.](../../sql-database/sql-database-business-continuity.md)
