---
title: Azure Lsv2 serisi sanal makinelerde performansı optimize edin - Depolama
description: Lsv2 serisi sanal makinelerde çözümünüz için performansı nasıl optimize edebilirsiniz öğrenin.
services: virtual-machines-windows
author: sasha-melamed
manager: gwallace
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: joelpell
ms.openlocfilehash: 57b248908a02327f2521be05920259681a26817a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920238"
---
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>Lsv2 serisi sanal makinelerde performansı optimize edin

Lsv2 serisi sanal makineler, çok çeşitli uygulamalar ve endüstriler de yerel depolama da yüksek G/Ç ve iş gücü gerektiren çeşitli iş yüklerini destekler.  Lsv2 serisi Büyük Veri, SQL, NoSQL veritabanları, veri ambarı ve Cassandra, MongoDB, Cloudera ve Redis gibi büyük işlem veritabanları için idealdir.

Lsv2 serisi Sanal Makineler (VM) tasarımı AMD EPYC™ 7551 işlemci işlemci, bellek, NVMe cihazlar ve VM'ler arasında en iyi performansı sağlamak için en üst düzeye çıkarır. Donanım performansını en üst düzeye çıkarmanın yanı sıra, Lsv2 serisi VM'ler donanım ve yazılımla daha iyi performans için Windows ve Linux işletim sistemlerinin gereksinimleriyle çalışacak şekilde tasarlanmıştır.

Yazılım ve donanımın ayarlanması, [Windows Server 2019 Datacenter'ın Aralık 2018](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview)başlarında Lsv2 serisi VM'lerde NVMe cihazlarında maksimum performansı destekleyen Azure Marketi'ne en iyi şekilde yayımlanan sürümüyle sonuçlandı.

Bu makalede, iş yüklerinizin ve uygulamalarınızın VM'lerde tasarlanan maksimum performansa ulaşmasını sağlamak için ipuçları ve öneriler sağlar. Azure Marketi'ne daha fazla Lsv2 optimize edilmiş resim eklendikçe bu sayfadaki bilgiler sürekli olarak güncellenir.

## <a name="amd-eypc-chipset-architecture"></a>AMD EYPC™ yonga seti mimarisi

Lsv2 serisi VM'ler, Zen mikromimarisine dayalı AMD EYPC™ sunucu işlemcilerini kullanır. AMD, ON-die, on-package ve çok paketli iletişim için kullanılabilecek NUMA modeli için ölçeklenebilir ara bağlantı olarak ™ EYPC için Infinity Fabric (IF) geliştirdi. Intel modern monolitik kalıp işlemcilerde kullanılan QPI (Hızlı Yol Ara Bağlantısı) ve UPI (Ultra-Path Ara Bağlantı) ile karşılaştırıldığında, AMD'nin çok sayıdaNUMA küçük kalıp mimarisi hem performans avantajları hem de zorluklar getirebilir. Bellek bant genişliği ve gecikme kısıtlamalarının gerçek etkisi, çalışan iş yüklerinin türüne bağlı olarak değişebilir.

## <a name="tips-for-maximizing-performance"></a>Performansı en üst düzeye çıkarmak için ipuçları

* Lsv2 serisi VM'lere güç veren donanım, sekiz G/Ç Kuyruk Çifti (QP) içeren NVMe aygıtlarını kullanır. Her NVMe aygıtı G/Ç kuyruğu aslında bir çifttir: bir gönderme sırası ve tamamlama sırası. NVMe sürücüsü yuvarlak robin zamanlama da Dağıtarak bu sekiz G/Ç QPs kullanımını optimize etmek için ayarlanır. Maksimum performans elde etmek için, eşleşecek şekilde aygıt başına sekiz iş çalıştırın.

* Etkin iş yükleri sırasında NVMe yönetici komutlarını (örneğin, NVMe SMART bilgi sorgusu, vb.) NVMe G/Ç komutlarıyla karıştırmaktan kaçının. Lsv2 NVMe aygıtları Hyper-V NVMe Direct teknolojisi ile desteklenen ve herhangi bir NVMe yöneticisi beklemede olduğunda "yavaş moduna" geçiş yapan teknoloji. Lsv2 kullanıcıları bu durumda NVMe G/Ç performansında dramatik bir performans düşüşü görebilirler.

* Lsv2 kullanıcıları, uygulamalarına yönelik NUMA yakınlığına karar vermek için veri sürücüleri için VM'den bildirilen cihaz NUMA bilgilerine (tüm 0) güvenmemelidir. Daha iyi performans için önerilen yol, iş yüklerini mümkünse CPU'lara yaymaktır. 

* Lsv2 VM NVMe aygıtı için G/Ç sıra çifti başına desteklenen maksimum sıra derinliği 1024'dür (Amazon i3 QD 32 sınırına karşı). Lsv2 kullanıcıları, performansı azaltabilecek tam koşulları tetiklemeyi önlemek için (sentetik) kıyaslama iş yüklerini 1024 veya daha düşük sıraya kadar sınırlandırmalıdır.

## <a name="utilizing-local-nvme-storage"></a>Yerel NVMe depolama yı kullanma

Tüm Lsv2 VM'lerinde 1.92 TB NVMe diskindeki yerel depolama geçicidir. VM'nin başarılı bir standart yeniden başlatması sırasında, yerel NVMe diskindeki veriler devam edecektir. VM yeniden dağıtılırsa, ayrılmış veya silinirse veriler NVMe'de kalıcı olmayacaktır. Başka bir sorun VM'nin veya üzerinde çalıştırdığı donanımın sağlıksız hale gelmesine neden olursa veriler kalıcı olmayacaktır. Bu durumda, eski ana bilgisayardaki tüm veriler güvenli bir şekilde silinir.

VM'nin planlı bir bakım işlemi sırasında farklı bir ana bilgisayara taşınması gereken durumlar da olacaktır. [Planlanmış Etkinlikler](scheduled-events.md)ile planlanan bakım işlemleri ve bazı donanım hataları tahmin edilebilir. Planlanmış Olaylar, tahmin edilen tüm bakım ve kurtarma işlemlerinde güncel kalmak için kullanılmalıdır.

Planlı bir bakım olayının VM'nin boş yerel disklere sahip yeni bir ana bilgisayarda yeniden oluşturulmasını gerektirmesi durumunda, verilerin yeniden eşitlenmesine gerek vardır (yine, eski ana bilgisayardaki veriler güvenli bir şekilde silinir). Lsv2 serisi VM'ler şu anda yerel NVMe diskinde canlı geçişi desteklemediği için bu durum oluşur.

Planlı bakım için iki mod vardır.

### <a name="standard-vm-customer-controlled-maintenance"></a>Standart VM müşteri kontrollü bakım

- VM, 30 günlük bir süre boyunca güncelleştirilmiş bir ana bilgisayara taşınır.
- Lsv2 yerel depolama verileri kaybolabilir, bu nedenle etkinlikten önce yedekleme verileri önerilir.

### <a name="automatic-maintenance"></a>Otomatik bakım

- Müşteri müşteri kontrollü bakım yürütmez veya bir güvenlik sıfır gün olay gibi acil durumlarda oluşur.
- Müşteri verilerini korumak için tasarlanmıştır, ancak VM dondurma veya yeniden başlatma küçük bir risk vardır.
- Lsv2 yerel depolama verileri kaybolabilir, bu nedenle etkinlikten önce yedekleme verileri önerilir.

Yaklaşan servis olayları için, güncelleştirme için size en uygun zamanı seçmek için kontrollü bakım işlemini kullanın. Etkinlikten önce, verilerinizi premium depolamada yedekleyebilirsiniz. Bakım olayı tamamlandıktan sonra, verilerinizi yenilenmiş Lsv2 VM'ler yerel NVMe depolama alanına döndürebilirsiniz.

Yerel NVMe diskleri ile ilgili verileri koruyan senaryolar şunlardır:

- VM çalışıyor ve sağlıklı.
- VM yerinde yeniden başlatılır (siz veya Azure tarafından).
- VM duraklatıldı (ayırma yapılmadan durduruldu).
- Planlanan bakım servis işlemlerinin çoğu.

Müşteriyi korumak için verileri güvenli bir şekilde silen senaryolar şunlardır:

- VM yeniden dağıtılır, durdurulur (tahsis edilir) veya silinir (sizin yeriniz tarafından).
- VM sağlıksız hale gelir ve bir donanım sorunu nedeniyle başka bir düğüme iyileşmek için hizmet etmek zorundadır.
- VM'nin servis için başka bir ana bilgisayara tahsis edilmesi gereken az sayıda planlı bakım servis işlemi.

Yerel depolama alanında verileri yedekleme seçenekleri hakkında daha fazla bilgi edinmek [için Azure IaaS diskleri için Yedekleme ve olağanüstü durum kurtarma bilgisine](backup-and-disaster-recovery-for-azure-iaas-disks.md)bakın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

* **Lsv2 serisi VM'leri dağıtmaya nasıl başlarım?**  
   Diğer VM'ler gibi, VM oluşturmak için [Portal,](quick-create-portal.md) [Azure CLI](quick-create-cli.md)veya [PowerShell'i](quick-create-powershell.md) kullanın.

* **Tek bir NVMe disk hatası ana bilgisayardaki tüm VM'lerin başarısız lığa neden olur mu?**  
   Donanım düğümünde bir disk hatası algılanırsa, donanım başarısız durumdadır. Bu durumda, düğümdeki tüm VM'ler otomatik olarak ayrılır ve sağlıklı bir düğüme taşınır. Lsv2 serisi VM'ler için bu, başarısız düğümdeki müşterinin verilerinin de güvenli bir şekilde silindiği ve yeni düğümüzerinde müşteri tarafından yeniden oluşturulması gerektiği anlamına gelir. Belirtildiği gibi, Lsv2'de canlı geçiş kullanılabilir hale gelmeden önce, başarısız düğümdeki veriler, başka bir düğüme aktarılırken VM'lerle proaktif olarak taşınır.

* **Windows Server 2012 veya Windows Server 2016'da Windows'da yoklama ayarlamaları yapmam gerekiyor mu?**  
   NVMe yoklamaları yalnızca Windows Server 2019'da Azure'da kullanılabilir.  

* **Geleneksel kesme hizmeti yordamı (ISR) modeline geri dönebilir miyim?**  
   Lsv2 serisi VM'ler NVMe yoklamaiçin optimize edilebistir. Yoklama performansını artırmak için güncelleştirmeler sürekli olarak sağlanır.

* **Windows Server 2019'daki yoklama ayarlarını ayarlayabilir miyim?**  
   Yoklama ayarları kullanıcı tarafından ayarlanılamaz.
   
## <a name="next-steps"></a>Sonraki adımlar

* Azure'da [depolama performansı için optimize edilmiş](sizes-storage.md) tüm VM'lerin teknik özelliklerine bakın
