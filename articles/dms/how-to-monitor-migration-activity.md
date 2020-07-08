---
title: Geçiş etkinliğini izleme-Azure veritabanı geçiş hizmeti
description: Geçiş etkinliğini izlemek için Azure veritabanı geçiş hizmeti 'ni kullanmayı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77648521"
---
# <a name="monitor-migration-activity-using-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti 'ni kullanarak geçiş etkinliğini izleme
Bu makalede, bir geçişin ilerlemesini hem veritabanı düzeyinde hem de tablo düzeyinde nasıl izleyebileceğinizi öğreneceksiniz.

## <a name="monitor-at-the-database-level"></a>Veritabanı düzeyinde izleme
Etkinliği veritabanı düzeyinde izlemek için veritabanı düzeyi dikey penceresini görüntüleyin:

![Veritabanı düzeyi dikey penceresi](media/how-to-monitor-migration-activity/dms-database-level-blade.png)

> [!NOTE]
> Veritabanı Köprüsü seçilirse, Tablo listesi ve bunların geçiş ilerlemesi görüntülenir.

Aşağıdaki tabloda veritabanı düzeyi dikey penceresindeki alanlar listelenmekte ve her biriyle ilişkili çeşitli durum değerleri açıklanmaktadır.

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
      <td>Geçiş etkinliği sorunlar olmadan başarılı oldu.</td>
    </tr>
    <tr>
      <td>Alındı</td>
      <td>Geçiş başarısız oldu. Tüm hata mesajı için geçiş ayrıntıları altındaki ' hata ayrıntılarını gör ' bağlantısını seçin.</td>
    </tr>
    <tr>
      <td rowspan="4" class="Status"><strong>Durum</strong></td>
      <td>Başlatılıyor</td>
      <td>DMS, geçiş işlem hattını ayarlıyor.</td>
    </tr>
    <tr>
      <td>Çalışıyor</td>
      <td>DMS işlem hattı çalışıyor ve geçiş gerçekleştiriyor.</td>
    </tr>
    <tr>
      <td>Tamamla</td>
      <td>Geçiş tamamlandı.</td>
    </tr>
    <tr>
      <td>Başarısız</td>
      <td>Geçiş başarısız oldu. Geçiş hatalarını görmek için geçiş ayrıntıları ' na tıklayın.</td>
    </tr>
    <tr>
      <td rowspan="5" class="migration-details"><strong>Geçiş ayrıntıları</strong></td>
      <td>Geçiş işlem hattı başlatılıyor</td>
      <td>DMS, geçiş işlem hattını ayarlıyor.</td>
    </tr>
    <tr>
      <td>Tam veri yükleme devam ediyor</td>
      <td>DMS ilk yük gerçekleştiriyor.</td>
    </tr>
    <tr>
      <td>Cutover için hazırlanma</td>
      <td>İlk yükleme tamamlandıktan sonra, DMS veritabanını cutover için hazırlanıyor olarak işaretler. Kullanıcı, sürekli eşitlemeye göre verilerin yakalanıp yakalanmadığında denetlemelidir.</td>
    </tr>
    <tr>
      <td>Tüm değişiklikler uygulandı</td>
      <td>İlk yükleme ve sürekli eşitleme tamamlanmıştır. Bu durum, veritabanının başarıyla başarılı olduktan sonra da oluşur.</td>
    </tr>
    <tr>
      <td>Hata ayrıntılarına bakın</td>
      <td>Hata ayrıntılarını göstermek için bağlantıya tıklayın.</td>
    </tr>
    <tr>
      <td rowspan="1" class="duration"><strong>Süre</strong></td>
      <td>YOK</td>
      <td>Geçiş etkinliğinin başlatıldığı veya geçişinin hatalı olarak başlatıldığı toplam süre.</td>
    </tr>
     </tbody>
</table>

## <a name="monitor-at-table-level--quick-summary"></a>Tablo düzeyinde izleme – hızlı özet
Etkinliği tablo düzeyinde izlemek için tablo düzeyi dikey penceresini görüntüleyin. Dikey pencerenin üst kısmında, tam yük ve artımlı güncelleştirmelerde geçirilmiş satırların ayrıntılı sayısı gösterilmektedir. 

Dikey pencerenin alt kısmı tabloları listeler ve geçiş ilerlemesinin hızlı bir özetini gösterir.

![Tablo düzeyi dikey penceresi-hızlı özet](media/how-to-monitor-migration-activity/dms-table-level-blade-summary.png)

Aşağıdaki tabloda Tablo düzeyi ayrıntılarında gösterilen alanlar açıklanmaktadır.

| Alan adı        | Açıklama       |
| ------------- | ------------- |
| **Tam yük tamamlandı**      | Tam veri yükünü tamamlayan tablo sayısı. |
| **Tam yük sıraya alındı**      | Tam yük için sıraya alınan tablo sayısı.      |
| **Tam yük yükleme** | Tablo sayısı başarısız oldu.      |
| **Artımlı güncelleştirmeler**      | Hedefe uygulanan satırlardaki değişiklik verilerini yakalama (CDC) güncelleştirmelerinin sayısı. |
| **Artımlı eklemeler**      | Hedefe uygulanan CDC ekleme sayısı.      |
| **Artımlı silmeler** | Hedefe uygulanan, satırlardaki CDC silmesi sayısı.      |
| **Bekleyen değişiklikler**      | Hala hedefe uygulanmasını bekleyen satırlardaki CDC sayısı. |
| **Uygulanan değişiklikler**      | Hedefe uygulanan satırlarda CDC güncelleştirme, ekleme ve silme toplamı.      |
| **Hata durumundaki tablolar** | Geçiş sırasında ' Error ' durumunda olan tablo sayısı. Tabloların hata durumuna gidebilme örnekleri, hedefte tanımlanan yinelenen öğeler olduğunda veya veriler hedef tabloda uyumlu olmayan bir yüklemedir.      |

## <a name="monitor-at-table-level--detailed-summary"></a>Tablo düzeyinde izleme – ayrıntılı özet
Geçiş ilerlemesini tam yükleme ve artımlı veri eşitleme içinde gösteren iki sekme vardır.
    
![Tam yükleme sekmesi](media/how-to-monitor-migration-activity/dms-full-load-tab.png)

![Artımlı veri eşitleme sekmesi](media/how-to-monitor-migration-activity/dms-incremental-data-sync-tab.png)

Aşağıdaki tabloda tablo düzeyinde geçiş ilerlemede gösterilen alanlar açıklanmaktadır.

| Alan adı        | Açıklama       |
| ------------- | ------------- |
| **Durum-eşitleniyor**      | Sürekli eşitleme çalışıyor. |
| **Ekle**      | Hedefe uygulanan CDC ekleme sayısı.      |
| **Güncelleştir** | Hedefe uygulanan satırlardaki CDC güncelleştirme sayısı.      |
| **Sil**      | Hedefe uygulanan, satırlardaki CDC silmesi sayısı. |
| **Toplam uygulandı**      | Hedefe uygulanan satırlarda CDC güncelleştirme, ekleme ve silme toplamı. |
| **Veri hataları** | Bu tabloda gerçekleşen veri hatalarının sayısı. Hatalara ilişkin bazı örnekler 511 şunlardır *:% d, izin verilen en büyük satır boyutu olan% d, 8114:% ls veri türü% ls öğesine dönüştürülürken hata.*  Müşteri, hata ayrıntılarını görmek için Azure hedefi 'ndeki dms_apply_exceptions tablosundan sorgu sağlamalıdır.    |

> [!NOTE]
> INSERT, Update ve DELETE değerlerinin ve toplam uygulanmış olan CDC değerleri, veritabanı tam geçişi veya geçiş yeniden başlatıldığında azalabilir.

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft [veritabanı geçiş kılavuzu](https://datamigration.microsoft.com/)' nda geçiş kılavuzunu gözden geçirin.