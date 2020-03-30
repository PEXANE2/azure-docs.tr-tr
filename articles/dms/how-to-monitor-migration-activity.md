---
title: Geçiş etkinliğini izleme - Azure Veritabanı Geçiş Hizmeti
description: Geçiş etkinliğini izlemek için Azure Veritabanı Geçiş Hizmeti'ni kullanmayı öğrenin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 31b49cdd9e0e5569981b2a0b0c6efcab7239e019
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648521"
---
# <a name="monitor-migration-activity-using-the-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti'ni kullanarak geçiş etkinliğini izleme
Bu makalede, hem veritabanı hem de tablo düzeyinde geçişin ilerlemesini nasıl izleyeceğinizi öğrenirsiniz.

## <a name="monitor-at-the-database-level"></a>Veritabanı düzeyinde izleme
Veritabanı düzeyindeki etkinliği izlemek için veritabanı düzeyindeki bıçağı görüntüleyin:

![Veritabanı düzeyinde bıçak](media/how-to-monitor-migration-activity/dms-database-level-blade.png)

> [!NOTE]
> Veritabanı köprü seçilmesi tabloların listesini ve bunların geçiş ilerleme gösterir.

Aşağıdaki tablo, veritabanı düzeyindeki bıçaktaki alanları listeler ve her biriyle ilişkili çeşitli durum değerlerini açıklar.

<table id='overview' class='overview'>
  <thead>
    <tr>
      <th class="x-hidden-focus"><strong>Alan adı</strong></th>
      <th><strong>Alan alt durumu</strong></th>
      <th><strong>Açıklama</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" class="ActivityStatus"><strong>Etkinlik durumu</strong></td>
      <td>Çalışıyor</td>
      <td>Geçiş etkinliği çalışıyor.</td>
    </tr>
    <tr>
      <td>Başarılı oldu</td>
      <td>Geçiş etkinliği sorunsuz bir şekilde başarılı oldu.</td>
    </tr>
    <tr>
      <td>Hatalı</td>
      <td>Geçiş başarısız oldu. Tam hata iletisi için geçiş ayrıntılarının altındaki 'Hata ayrıntılarına bakın' bağlantısını seçin.</td>
    </tr>
    <tr>
      <td rowspan="4" class="Status"><strong>Durum</strong></td>
      <td>Başlatılıyor</td>
      <td>DMS geçiş ardışık hattını kuruyor.</td>
    </tr>
    <tr>
      <td>Çalışıyor</td>
      <td>DMS ardışık işakin çalışıyor ve geçiş gerçekleştiriyor.</td>
    </tr>
    <tr>
      <td>Complete</td>
      <td>Geçiş tamamlandı.</td>
    </tr>
    <tr>
      <td>Başarısız</td>
      <td>Geçiş başarısız oldu. Geçiş hatalarını görmek için geçiş ayrıntılarını tıklatın.</td>
    </tr>
    <tr>
      <td rowspan="5" class="migration-details"><strong>Geçiş ayrıntıları</strong></td>
      <td>Geçiş ardışık hattını başlatma</td>
      <td>DMS geçiş ardışık hattını kuruyor.</td>
    </tr>
    <tr>
      <td>Tam veri yükü devam ediyor</td>
      <td>DMS ilk yüklemeyi gerçekleştiriyor.</td>
    </tr>
    <tr>
      <td>Cutover için hazır</td>
      <td>İlk yükleme tamamlandıktan sonra, DMS veritabanını kesmeye hazır olarak işaretler. Kullanıcı, verilerin sürekli eşitleme olup olmadığını kontrol etmelidir.</td>
    </tr>
    <tr>
      <td>Uygulanan tüm değişiklikler</td>
      <td>İlk yük ve sürekli eşitleme tamamlandı. Bu durum, veritabanı başarıyla kesildikten sonra da oluşur.</td>
    </tr>
    <tr>
      <td>Hata ayrıntılarına bakın</td>
      <td>Hata ayrıntılarını göstermek için bağlantıyı tıklatın.</td>
    </tr>
    <tr>
      <td rowspan="1" class="duration"><strong>Süre</strong></td>
      <td>Yok</td>
      <td>Geçiş etkinliğinin başlatılmasından başlatılan geçiş ten mişemi tamamlanan veya taşınmaya ait toplam zaman hatalı.</td>
    </tr>
     </tbody>
</table>

## <a name="monitor-at-table-level--quick-summary"></a>Tablo düzeyinde monitör – Hızlı Özet
Tablo düzeyindeki etkinliği izlemek için tablo düzeyindeki bıçağı görüntüleyin. Bıçağın üst kısmı, tam yük ve artımlı güncelleştirmelerle geçirilen ayrıntılı satır sayısını gösterir. 

Bıçağın alt kısmı tabloları listeler ve geçiş ilerlemesinin hızlı bir özetini gösterir.

![Masa düzeyinde bıçak - hızlı özet](media/how-to-monitor-migration-activity/dms-table-level-blade-summary.png)

Aşağıdaki tabloda tablo düzeyinde ayrıntılarda gösterilen alanlar açıklanmaktadır.

| Alan adı        | Açıklama       |
| ------------- | ------------- |
| **Tam yük tamamlandı**      | Tam veri yükünü tamamlayan tablo sayısı. |
| **Tam yük sıralandı**      | Tam yük için sıraya tutulan tablo sayısı.      |
| **Tam yük yüklemesi** | Tablo sayısı başarısız oldu.      |
| **Artımlı güncelleştirmeler**      | Hedefe uygulanan satırlarda değişiklik veri yakalama (CDC) güncelleştirmelerinin sayısı. |
| **Artımlı kesici uçlar**      | Hedefe uygulanan satırdaki CDC uçlarının sayısı.      |
| **Artımlı silmeler** | Hedefe uygulanan satırlarda CDC silme sayısı.      |
| **Bekleyen değişiklikler**      | Hedefe uygulanmayı bekleyen satırlarda CDC sayısı. |
| **Uygulanan değişiklikler**      | Hedefe uygulanan satırlarda CDC güncelleştirmeleri, ekler ve silmelerin toplamı.      |
| **Hata durumundaki tablolar** | Geçiş sırasında 'hata' durumunda olan tablo sayısı. Tabloların hata durumuna girebileceği bazı örnekler, hedefte tanımlanan yinelenenler olduğunda veya veri hedef tabloda uyumlu yükleme olmadığında verilebilir.      |

## <a name="monitor-at-table-level--detailed-summary"></a>Tablo düzeyinde monitör – Ayrıntılı Özet
Tam yük ve Artımlı veri eşitlemesinde geçiş ilerlemesini gösteren iki sekme vardır.
    
![Tam yük sekmesi](media/how-to-monitor-migration-activity/dms-full-load-tab.png)

![Artımlı veri eşitleme sekmesi](media/how-to-monitor-migration-activity/dms-incremental-data-sync-tab.png)

Aşağıdaki tabloda tablo düzeyi geçiş ilerleme gösterilen alanları açıklanmaktadır.

| Alan adı        | Açıklama       |
| ------------- | ------------- |
| **Durum - Eşitleme**      | Sürekli eşitleme çalışıyor. |
| **Ekle**      | Hedefe uygulanan satırdaki CDC uçlarının sayısı.      |
| **Güncelleştirme** | Hedefe uygulanan satırlarda CDC güncelleştirmelerinin sayısı.      |
| **Sil**      | Hedefe uygulanan satırlarda CDC silme sayısı. |
| **Toplam Uygulanan**      | Hedefe uygulanan satırlarda CDC güncelleştirmeleri, ekler ve silmelerin toplamı. |
| **Veri Hataları** | Bu tabloda veri hataları nın sayısı gerçekleşti. Hatalara örnek olarak *511: İzin verilen maksimum satır boyutu %d'den büyük %d boyutu, 8114: Hata dönüştürme veri türü %ls'den %ls'e dönüştürülemez.*  Müşteri hata ayrıntılarını görmek için Azure hedefindeki dms_apply_exceptions tablodan sorgu lamalıdır.    |

> [!NOTE]
> Veritabanı kesildiğinde veya geçiş yeniden başlatıldığında Ekle, Güncelleştirme ve Silme ve Toplam Uygulanan CDC değerleri düşebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft [Veritabanı Geçiş Kılavuzu'ndaki](https://datamigration.microsoft.com/)geçiş kılavuzunu gözden geçirin.