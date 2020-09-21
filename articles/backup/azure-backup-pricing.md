---
title: Azure Backup fiyatlandırması
description: Azure Backup fiyatlandırmasını inceleyerek bütçenizi oluşturmak için gerekli tahmini maliyetleri nasıl hesaplayacağınızı öğrenin.
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: 03ec0076d3089562ddaace6db413fb3f1ba949a6
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88654540"
---
# <a name="azure-backup-pricing"></a>Azure Backup fiyatlandırması

Azure Backup fiyatlandırması hakkında daha fazla bilgi edinmek için [Azure Backup fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/backup/) ziyaret edin.

## <a name="download-detailed-estimates-for-azure-backup-pricing"></a>Azure Backup fiyatlandırmasıyla ilgili ayrıntılı tahminleri indirme

Bütçe oluşturma veya fiyat karşılaştırma amacıyla tahmini maliyet hesabı yapmak istiyorsanız ayrıntılı [Azure Backup fiyatlandırma tahmin aracını](https://aka.ms/AzureBackupCostEstimates) indirin.  

### <a name="what-does-the-estimator-contain"></a>Tahmin aracı neleri içerir?

Azure Backup maliyet tahmin aracı sayfası, Azure Backup kullanarak yedeklemeyi düşündüğünüz tüm olası iş yükleriyle ilgili tahmini maliyetleri hesaplama imkanı sunar. Bu iş yükleri şunları kapsar:

- Azure VM’leri
- Şirket içi sunucular
- Azure VM'de SQL
- Azure VM’de SAP HANA
- Azure dosya paylaşımları

## <a name="estimate-costs-for-backing-up-azure-vms-or-on-premises-servers"></a>Azure VM'lerini veya şirket içi sunucuları yedeklemek için tahmini maliyetleri hesaplama

Azure Backup ile Azure VM'lerini veya şirket içi sunucuları yedekleme maliyetlerini hesaplamak için şu parametreleri kullanmanız gerekir:

- Yedeklemeye çalıştığınız VM'lerin veya şirket içi sunucuların boyutu
  - Yedeklenecek disklerin veya sunucuların "kullanılan boyutunu" girin

- Bu boyuta sahip sunucu sayısı

- Bu sunucular için beklenen veri değişim sıklığı nedir?<br>
  Değişim sıklığı, verilerin değiştirilme oranını belirtir. Örneğin bir VM'de yedeklenecek 200 GB veri varsa ve her gün 10 GB değişiklik yapılıyorsa günlük veri değişim sıklığı %5 olur.

  - Veri değişim sıklığının artması, yedeklenecek verilerin de artacağı anlamına gelir

  - Dosya sunucuları için **Düşük** veya **Orta**, veritabanları için ise **Yüksek** seçeneğini belirleyin

  - **Veri değişim sıklığı yüzdesini** biliyorsanız **Kendi yüzde değerinizi girin** seçeneğini kullanabilirsiniz

- Yedekleme ilkesini seçme

  - "Günlük" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (gün)

  - "Haftalık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (hafta)

  - "Aylık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (ay)

  - "Yıllık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (yıl)

  - "Anında geri yükleme anlık görüntülerini" ne kadar süreyle bekletmek istiyorsunuz? (1-5 gün)

    - Bu seçenek, disklerde depolanan anlık görüntüleri kullanarak son yedi güne ait verileri hızlı bir şekilde geri yüklemenizi sağlar.

- **İsteğe bağlı**: Seçmeli Disk Yedeklemesi

  - Azure VM'lerini yedeklerken **Seçmeli Disk Yedeklemesi** seçeneğini kullanıyorsanız **Diski Hariç Tut** seçeneğini belirleyerek yedeklemeden hariç tutulan disklerin boyuta göre yüzdesini girin. Örneğin her birinde 200 GB kullanılan alan bulunan üç diske bağlı bir VM varsa ve bu disklerin ikisini yedeklemeden hariç tutmak istiyorsanız %66,7 yazın.

- **İsteğe bağlı**: Yedekleme Alanı Yedekliliği

  - Bu değer, yedekleme verilerinizin gönderildiği Depolama Hesabının yedeklilik durumunu belirtir. En yüksek kullanılabilirlik için **GRS**'yi seçmeniz önerilir. Yedekleme verilerinizin bir kopyasının farklı bir bölgede tutulmasını sağlayan bu seçenek, birden çok uyumluluk standardını karşılamanıza yardımcı olur. Kurumsal düzeyde yedeklemeye ihtiyaç duymayan geliştirme veya test ortamlarını yedekliyorsanız yedeklilik ayarını **LRS** olarak değiştirebilirsiniz. Yedekleriniz için [Bölgeler Arası Geri Yükleme](backup-azure-arm-restore-vms.md#cross-region-restore) etkinleştirildiğinde karşılaşacağınız maliyetleri anlamak istiyorsanız sayfadaki **RAGRS** seçeneğini belirleyin.

- **İsteğe bağlı**: Bölgeye özgü fiyatlandırmayı değiştirin veya indirimli fiyatları uygulayın

  - Farklı bir bölge için veya indirimli fiyatlar üzerinden maliyet tahmininde bulunmak isterseniz **Farklı bir bölge için tahminde bulunmayı denemek ister misiniz?** seçeneğinde **Evet**'i seçin ve tahminleri çalıştırmak istediğiniz fiyatları girin.

## <a name="estimate-costs-for-backing-up-sql-servers-in-azure-vms"></a>Azure VM'lerindeki SQL sunucularını yedeklemek için tahmini maliyetleri hesaplama

Azure Backup ile Azure VM'lerinde çalışan SQL sunucularını yedekleme maliyetlerini hesaplamak için şu parametreleri kullanmanız gerekir:

- Yedeklemeye çalıştığınız SQL sunucularının boyutu

- Yukarıda seçtiğiniz boyuta sahip olan SQL sunucusu sayısı

- SQL sunucularınızın yedekleme verileri için beklenen sıkıştırma düzeyi nedir?

  - Çoğu Azure Backup müşterisi, SQL sıkıştırma özelliği **etkinleştirildiğinde** yedekleme verilerinin SQL sunucusunun boyutuna kıyasla %80 sıkıştırıldığını görmektedir.

  - Farklı bir sıkıştırma oranı görmeyi bekliyorsanız buraya ilgili oranı girin

- Günlük yedeklerinin beklenen boyutu nedir?

  - % işareti SQL sunucusunun boyutuna kıyasla % olarak günlük boyutunu belirtir

- Bu sunucular için beklenen günlük veri değişim sıklığı nedir?

  - Genellikle veritabanlarının veri değişim sıklığı "Yüksek" olur

  - **Veri değişim sıklığı yüzdesini** biliyorsanız **Kendi yüzde değerinizi girin** seçeneğini kullanabilirsiniz

- Yedekleme ilkesini seçme

  - Yedekleme Türü

    - Seçebileceğiniz en etkili ilke, haftalık/aylık/yıllık tam yedeklemelerle **Günlük değişiklik** yedekleridir. Azure Backup, tek tıklamayla değişiklik yedeklerinden geri yükleme yapabilir.

    - Ayrıca günlük/haftalık/aylık/yıllık tam yedekleme yapma ilkesini de seçebilirsiniz. Bu seçenek ilk seçenekten biraz daha fazla depolama alanı kullanır.

  - "İşlem günlüğü" yedeklerini ne kadar süreyle bekletmek istiyorsunuz? (gün) [7-35]

  - "Günlük" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (gün)

  - "Haftalık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (hafta)

  - "Aylık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (ay)

  - "Yıllık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (yıl)

- **İsteğe bağlı**: Yedekleme Alanı Yedekliliği

  - Bu değer, yedekleme verilerinizin gönderildiği Depolama Hesabının yedeklilik durumunu belirtir. En yüksek kullanılabilirlik için **GRS**'yi seçmeniz önerilir. Yedekleme verilerinizin bir kopyasının farklı bir bölgede tutulmasını sağlayan bu seçenek, birden çok uyumluluk standardını karşılamanıza yardımcı olur. Kurumsal düzeyde yedeklemeye ihtiyaç duymayan geliştirme veya test ortamlarını yedekliyorsanız yedeklilik ayarını **LRS** olarak değiştirebilirsiniz.

- **İsteğe bağlı**: Bölgeye özgü fiyatlandırmayı değiştirin veya indirimli fiyatları uygulayın

  - Farklı bir bölge için veya indirimli fiyatlar üzerinden maliyet tahmininde bulunmak isterseniz **Farklı bir bölge için tahminde bulunmayı denemek ister misiniz?** seçeneğinde **Evet**'i seçin ve tahminleri çalıştırmak istediğiniz fiyatları girin.

## <a name="estimate-costs-for-backing-up-sap-hana-servers-in-azure-vms"></a>Azure VM'lerindeki SAP HANA sunucularını yedeklemek için tahmini maliyetleri hesaplama

Azure Backup ile Azure VM'lerinde çalışan SAP HANA sunucularını yedekleme maliyetlerini hesaplamak için şu parametreleri kullanmanız gerekir:

- Yedeklemeye çalıştığınız SAP HANA veritabanlarının toplam boyutu. Bu, SAP HANA tarafından bildirilen her bir veritabanının tam yedekleme boyutunun toplamı olmalıdır.
- Yukarıda seçtiğiniz boyuta sahip olan SAP HANA sunucusu sayısı
- Günlük yedeklerinin beklenen boyutu nedir?
  
  - %, SAP HANA sunucusunda yedeklemekte olduğunuz SAP HANA veritabanlarının toplam boyutunun % değeri olarak ortalama günlük işlem günlüğü boyutunu gösterir
- Bu sunucular için beklenen günlük veri değişim sıklığı nedir?
  - %, SAP HANA sunucusunda yedeklemekte olduğunuz SAP HANA veritabanlarının toplam boyutunun % değeri olarak ortalama günlük veri değişim sıklığını gösterir
  - Genellikle veritabanlarının veri değişim sıklığı "Yüksek" olur
  - **Veri değişim sıklığı yüzdesini** biliyorsanız **Kendi yüzde değerinizi girin** seçeneğini kullanabilirsiniz
- Yedekleme ilkesini seçme
  - Yedekleme Türü
    - Seçebileceğiniz en etkili ilke, **haftalık/aylık/yıllık** tam yedeklemelerle **Günlük değişiklik** yedekleridir. Azure Backup, tek tıklamayla değişiklik yedeklerinden geri yükleme yapabilir.
    - Ayrıca **günlük/haftalık/aylık/yıllık** tam yedekleme yapma ilkesini de seçebilirsiniz. Bu seçenek ilk seçenekten biraz daha fazla depolama alanı kullanır.
  - "İşlem günlüğü" yedeklerini ne kadar süreyle bekletmek istiyorsunuz? (gün) [7-35]
  - "Günlük" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (gün)
  - "Haftalık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (hafta)
  - "Aylık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (ay)
  - "Yıllık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (yıl)
- **İsteğe bağlı**: Yedekleme Alanı Yedekliliği
  
  - Bu değer, yedekleme verilerinizin gönderildiği Depolama Hesabının yedeklilik durumunu belirtir. En yüksek kullanılabilirlik için **GRS**'yi seçmeniz önerilir. Yedekleme verilerinizin bir kopyasının farklı bir bölgede tutulmasını sağlayan bu seçenek, birden çok uyumluluk standardını karşılamanıza yardımcı olur. Kurumsal düzeyde yedeklemeye ihtiyaç duymayan geliştirme veya test ortamlarını yedekliyorsanız yedeklilik ayarını **LRS** olarak değiştirebilirsiniz.
- **İsteğe bağlı**: Bölgeye özgü fiyatlandırmayı değiştirin veya indirimli fiyatları uygulayın
  
  - Farklı bir bölge için veya indirimli fiyatlar üzerinden maliyet tahmininde bulunmak isterseniz **Farklı bir bölge için tahminde bulunmayı denemek ister misiniz?** seçeneğinde **Evet**'i seçin ve tahminleri çalıştırmak istediğiniz fiyatları girin.
  
## <a name="estimate-costs-for-backing-up-azure-file-shares"></a>Azure dosya paylaşımlarını yedeklemek için tahmini maliyetleri hesaplama

Azure Backup tarafından sunulan [anlık görüntü tabanlı yedekleme çözümünü](azure-file-share-backup-overview.md) kullanarak Azure dosya paylaşımlarını yedeklemek için tahmini maliyetlerini hesaplamak isterseniz aşağıdaki parametreleri kullanmanız gerekir:

- Yedeklemek istediğiniz dosya paylaşımlarının boyutu (**GB**).

- Birden çok depolama hesabında bulunan dosya paylaşımlarını yedeklemek istiyorsanız yukarıdaki dosya boyutuna sahip olan dosya paylaşımlarını barındıran depolama hesabı sayısını belirtin.

- Yedeklemek istediğiniz dosya paylaşımlarındaki beklenen veri değişim sıklığı. <br>Değişim sıklığı, verilerdeki değişiklik miktarını belirtir ve anlık görüntü depolama boyutunu doğrudan etkiler. Örneğin bir dosya paylaşımında yedeklenecek 200 GB veri varsa ve her gün 10 GB değişiklik yapılıyorsa günlük veri değişim sıklığı %5 olur.
  - Veri değişim sıklığının yüksek olması, dosya paylaşımının içeriğindeki günlük veri değişikliği miktarının yüksek olduğunu belirtir ve bu durumda değişiklik anlık görüntüsü (yalnızca veri değişikliklerini yakalar) boyutu daha yüksek olacaktır.
  - Dosya paylaşımınızın özelliklerine ve kullanım durumuna göre Düşük (%1), Orta (%3) veya Yüksek (%5) seçeneklerinden birini belirleyin.
  - Dosya paylaşımınızın **Veri değişim sıklığı yüzdesini** biliyorsanız açılan menüden **Kendi yüzde değerinizi girin**'i seçebilirsiniz. Günlük, haftalık, aylık ve yıllık veri değişim sıklığı değerlerini (% olarak) belirtin.

- Yedeklenen dosya paylaşımını barındıran depolama hesabının türü (standart veya premium) ve depolama yedekliliği ayarı. <br>Azure dosya paylaşımlarına yönelik mevcut yedekleme çözümünde anlık görüntüler, yedeklenen dosya paylaşımıyla aynı depolama hesabında depolanır. Dolayısıyla anlık görüntüler için geçerli depolama ücretleri, yedeklenen dosya paylaşımı ile anlık görüntüleri barındıran depolama hesabının türüne ve yedeklilik ayarına göre belirlenen anlık görüntü fiyatlandırması üzerinden Azure Dosyalar faturanıza eklenir.

- Değişiklik yedekleri için bekletme seçenekleri
  - "Günlük" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (gün)
  - "Haftalık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (hafta)
  - "Aylık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (ay)
  - "Yıllık" yedekleri ne kadar süreyle bekletmek istiyorsunuz? (yıl)

  Her bir kategoride desteklenen maksimum bekletme değerleri için [Azure dosya paylaşımı destek tablosuna](azure-file-share-support-matrix.md#retention-limits) bakın.

- **İsteğe bağlı**: Bölgeye özgü fiyatlandırmayı değiştirin veya indirimli fiyatları uygulayın.
  - Tahmin aracında GB başına anlık görüntü depolama maliyeti ile korumalı örnek maliyeti için belirtilen varsayılan değerler, Doğu ABD bölgesine aittir. Farklı bir bölge için veya indirimli fiyatlar üzerinden maliyet tahmininde bulunmak isterseniz **Farklı bir bölge için tahminde bulunmayı denemek ister misiniz?** seçeneğinde **Evet**'i seçin ve tahminleri çalıştırmak istediğiniz fiyatları girin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Backup hizmeti nedir?](backup-overview.md)
