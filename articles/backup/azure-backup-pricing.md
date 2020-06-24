---
title: Azure Backup fiyatlandırması
description: Azure Backup fiyatlandırmadan bütçeleme maliyetlerini nasıl tahmin edebileceğiniz hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: d88587cfdbb4f60d0da8641fc0362b8f763779ad
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84908209"
---
# <a name="azure-backup-pricing"></a>Azure Backup fiyatlandırması

Azure Backup fiyatlandırması hakkında bilgi edinmek için [Azure Backup fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/backup/)ziyaret edin.

## <a name="download-detailed-estimates-for-azure-backup-pricing"></a>Azure Backup fiyatlandırmayla ilgili ayrıntılı tahminleri indirin

Maliyetlerinizi bütçeleme veya maliyet karşılaştırma amaçlarıyla tahmin etmek istiyorsanız, ayrıntılı [Azure Backup fiyatlandırma tahminleyicisi](https://aka.ms/AzureBackupCostEstimates)' ni indirin.  

### <a name="what-does-the-estimator-contain"></a>Tahmin aracı neleri içerir?

Azure Backup maliyet tahmini sayfasında, Azure Backup kullanarak yedeklemek istediğiniz tüm olası iş yüklerini tahmin etmenize yönelik bir seçenek bulunur. Bu iş yükleri şunları içerir:

- Azure VM’leri
- Şirket içi sunucular
- Azure VM 'lerinde SQL
- Azure VM 'lerinde SAP HANA
- Azure dosya paylaşımları

## <a name="estimate-costs-for-backing-up-azure-vms-or-on-premises-servers"></a>Azure VM 'lerini veya şirket içi sunucuları yedekleme maliyetlerini tahmin etme

Azure Backup kullanarak Azure VM 'Leri veya şirket içi sunucuları yedekleme maliyetlerini tahmin etmek için aşağıdaki parametrelere ihtiyacınız olacaktır:

- Yedeklemeye çalıştığınız VM 'Lerin veya şirket içi sunucuların boyutu
  - Yedeklenmesi gereken disklerin veya sunucuların "kullanılan boyutunu" girin

- Bu boyuttaki sunucu sayısı

- Bu sunucularda beklenen veri dalgalanması miktarı nedir?<br>
  Dalgalanma, verilerdeki değişiklik miktarını ifade eder. Örneğin, yedeklenecek 200 GB veri içeren bir VM olsaydıysanız ve her gün 10 GB değişirse, günlük dalgalanma %5 ' tir.

  - Daha yüksek dalgalanma, daha fazla veri yedeklediğiniz anlamına gelir

  - Dosya sunucuları için **düşük** veya **Orta** ve veritabanları çalıştırıyorsanız **yüksek** seçin

  - **% Dalgalanmasını**biliyorsanız, **kendi% seçeneğinizi girin**

- Yedekleme ilkesini seçin

  - "Günlük" yedeklemelerini ne kadar süreyle bekletmeniz beklensin mi? (gün)

  - "Haftalık" yedeklemeleri ne kadar süreyle bekletmeniz beklenir? (hafta cinsinden)

  - "Aylık" yedeklemeleri ne kadar süreyle bekletmeniz beklenir? (ay)

  - "Yıllık" yedeklemeleri ne kadar süreyle bekletmeniz beklensin mi? (yıl)

  - "Anlık geri yükleme anlık görüntülerini" ne kadar beklediğinizi düşünüyorsunuz? (1-7 gün)

    - Bu seçenek, disklerde depolanan anlık görüntüleri kullanarak hızlı bir şekilde yedi güne kadar geri yükleme yapmanızı sağlar

- **Isteğe bağlı** – seçmeli disk yedeklemesi

  - Azure VM 'lerini yedeklerken **Seçmeli disk yedekleme** seçeneğini kullanıyorsanız, **diski hariç tut** seçeneğini belirleyin ve yedeklemeden dışlanan disklerin yüzdesini boyut bakımından girin. Örneğin, her bir diskte 200 GB kullanılan üç diske bağlı bir VM varsa ve bunların ikisini de yedeklemekten dışlamak istiyorsanız,% 66,7 girin.

- **Isteğe bağlı** – yedek depolama artıklığı

  - Bu, yedekleme verilerinizin gittiği depolama hesabının yedekliği olduğunu gösterir. En yüksek kullanılabilirlik için **GRS** kullanmanızı öneririz. Yedekleme verilerinizin bir kopyasının farklı bir bölgede tutulmasını güvence altına aldığı için, bu, birden çok uyumluluk standardını karşılamanıza yardımcı olur. Kurumsal düzeyde bir yedeklemeye gerek gerektirmeyen geliştirme veya test ortamlarını yedekliyorsanız, artıklığı **LRS** olarak değiştirin. Yedeklemeleriniz için **bölgeler arası geri yüklemeyi** etkinleştirmek Istiyorsanız, **rampaları** seçin

- **Isteğe bağlı** – bölgesel fiyatlandırmayı değiştirin veya indirimli oranlar uygulayın

  - Farklı bir bölge veya indirimli ücretler için tahminlerinizi denetlemek isterseniz, **farklı bir bölge için tahminleri deneyin?** **seçeneğini belirleyin ve** tahminleri çalıştırmak istediğiniz ücretleri girin.

## <a name="estimate-costs-for-backing-up-sql-servers-in-azure-vms"></a>Azure VM 'lerinde SQL Server yedekleme maliyetlerini tahmin etme

Azure VM 'lerde çalışan SQL Server 'lar Azure Backup kullanarak yedekleme maliyetlerini tahmin etmek için aşağıdaki parametrelere ihtiyacınız olacaktır:

- Yedeklemeye çalıştığınız SQL Server 'ın boyutu

- Yukarıdaki boyuttaki SQL Server sayısı

- SQL sunucularınızın yedekleme verileri için beklenen sıkıştırma nedir?

  - Çoğu Azure Backup müşteri, SQL sıkıştırması **etkinleştirildiğinde**yedekleme verilerinde SQL Server boyutuna kıyasla %80 sıkıştırma olduğunu görür.

  - Farklı bir sıkıştırma görmeyi beklemeniz durumunda bu alana numarayı girin

- Günlük yedeklerinin beklenen boyutu nedir?

  - %, Günlük günlük boyutunu SQL Server boyutunun yüzdesi olarak gösterir

- Bu sunucularda günlük veri dalgalanması beklenen miktarı nedir?

  - Genellikle, veritabanlarının "yüksek" karmaşıklığı vardır

  - **% Dalgalanmasını**biliyorsanız, **kendi% seçeneğinizi girin**

- Yedekleme ilkesini seçin

  - Yedekleme Türü

    - Tercih ettiğiniz en etkili ilke haftalık/aylık/yıllık tam yedeklemeler için **günlük farklılıklar** . Azure Backup, tek tıklamayla de farklılıklar aracılığıyla geri yüklenebilir.

    - Ayrıca günlük/haftalık/aylık/yıllık tam yedeklemeler içeren bir ilke olmasını da tercih edebilirsiniz. Bu seçenek ilk seçenekten biraz daha fazla depolama alanı tüketir.

  - "Günlük" yedeklemelerini ne kadar süreyle bekletmeniz beklenir? (gün) [1-35]

  - "Günlük" yedeklemelerini ne kadar süreyle bekletmeniz beklensin mi? (gün)

  - "Haftalık" yedeklemeleri ne kadar süreyle bekletmeniz beklenir? (hafta cinsinden)

  - "Aylık" yedeklemeleri ne kadar süreyle bekletmeniz beklenir? (ay)

  - "Yıllık" yedeklemeleri ne kadar süreyle bekletmeniz beklensin mi? (yıl)

- **Isteğe bağlı** – yedek depolama artıklığı

  - Bu, yedekleme verilerinizin gittiği depolama hesabının yedekliği olduğunu gösterir. En yüksek kullanılabilirlik için **GRS** kullanmanızı öneririz. Yedekleme verilerinizin bir kopyasının farklı bir bölgede tutulmasını güvence altına aldığı için, bu, birden çok uyumluluk standardını karşılamanıza yardımcı olur. Kurumsal düzeyde bir yedeklemeye gerek gerektirmeyen geliştirme veya test ortamlarını yedekliyorsanız, artıklığı **LRS** olarak değiştirin.

- **Isteğe bağlı** – bölgesel fiyatlandırmayı değiştirin veya indirimli oranlar uygulayın

  - Farklı bir bölge veya indirimli ücretler için tahminlerinizi denetlemek isterseniz, **farklı bir bölge için tahminleri deneyin?** **seçeneğini belirleyin ve** tahminleri çalıştırmak istediğiniz ücretleri girin.

## <a name="estimate-costs-for-backing-up-sap-hana-servers-in-azure-vms"></a>Azure VM 'lerinde SAP HANA sunucularının yedeklenme maliyetlerini tahmin etme

Azure VM 'lerinde SAP HANA sunucularının yedeklenme maliyetlerini tahmin etmek SQL Server 'lar için tahmin gibidir. SQL sıkıştırmadan ayrı olarak, önceki bölümde bahsedilen değişkenleri kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Backup hizmeti nedir?](backup-overview.md)
