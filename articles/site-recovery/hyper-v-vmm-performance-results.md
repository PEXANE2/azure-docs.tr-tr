---
title: Azure Site Kurtarma'yı kullanarak VMM'li ikincil bir siteye Hyper-V VM çoğaltmasını test edin
description: Bu makalede, VMM bulutlarında Hyper-V VM'lerin Azure Site Kurtarma'yı kullanarak ikincil bir siteye çoğaltılması için performans testi hakkında bilgi verilmektedir.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi
ms.openlocfilehash: 3edd182e335bc679d95d7be64f45b617a9f54c1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73663182"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>İkincil bir siteye Hyper-V çoğaltma için test sonuçları


Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarında Hyper-V VM'leri ikincil bir veri merkezine kopyalarken performans testi sonuçları verilmektedir.

## <a name="test-goals"></a>Test hedefleri

Sınamanın amacı, Sabit durum çoğaltma sırasında Site Kurtarma'nın nasıl performans gösterdiğini incelemekti.

- Sabit durum çoğaltma, VM'ler ilk çoğaltmayı tamamladığında ve delta değişikliklerini eşitlediğinde oluşur.
- Beklenmeyen kesintiler meydana gelmediği sürece, performansın sabit durumu kullanarak ölçülmesi önemlidir, çünkü çoğu VM'nin kaldığı durumdur.
- Test dağıtımı, her sitede bir VMM sunucusu bulunan iki şirket içi siteden oluşuyordu. Bu test dağıtımı, merkez ofis/şube dağıtımının tipik bir nedenidir ve merkez ofis birincil site olarak hareket ederken, şube ikincil veya kurtarma sitesi olarak görev yapmakta.

## <a name="what-we-did"></a>Yaptığımız şey

İşte test sınavında yaptıklarımız:

1. VMM şablonlarını kullanarak VM'ler oluşturuldu.
2. VM'leri başlattı ve 12 saat içinde temel performans ölçümlerini yakaladı.
3. Birincil ve kurtarma VMM sunucularında bulutlar oluşturuldu.
4. Kaynak ve kurtarma bulutları arasındaki eşleme de dahil olmak üzere Site Kurtarma'da yapılandırılmış çoğaltma.
5. VM'ler için koruma yı etkinleştirdi ve ilk çoğaltmayı tamamlamalarına izin verdi.
6. Sistem stabilizasyonu için birkaç saat bekledim.
7. Tüm VM'lerin bu 12 saat boyunca beklenen çoğaltma durumunda kaldığı 12 saat içinde yakalanan performans ölçümleri.
8. Temel performans ölçümleri ile çoğaltma performans ölçümleri arasındaki delta ölçüldü.


## <a name="primary-server-performance"></a>Birincil sunucu performansı

* Hyper-V Replica (Site Kurtarma tarafından kullanılan) eşleme dosyasındaki değişiklikleri eş zamanlı olarak izler ve birincil sunucuda minimum depolama yükü kullanılır.
* Hyper-V Çoğaltma, izleme için IOPS havai en aza indirmek için kendi kendini koruyan bellek önbelleği kullanır. VHDX'e bellekte yazar ve günlük kurtarma sitesine gönderilmeden önce bunları günlük dosyasına boşaltır. Yazmaönceden belirlenmiş bir sınıra çarptığında bir disk sifonu da gerçekleşir.
* Aşağıdaki grafik, çoğaltma için sabit durum IOPS yükü gösterir. Çoğaltma nedeniyle IOPS yükü oldukça düşük% 5 civarında olduğunu görebilirsiniz.

  ![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V Çoğaltma, disk performansını optimize etmek için birincil sunucuda bellek kullanır. Aşağıdaki grafikte gösterildiği gibi, birincil kümedeki tüm sunucularda bellek yükü marjinaldir. Gösterilen bellek yükü, Hyper-V sunucusundaki toplam yüklü bellekle karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesidir.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V Çoğaltma minimum CPU yükü vardır. Grafikte gösterildiği gibi, çoğaltma yükü %2-3 aralığındadır.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>İkincil sunucu performansı

Hyper-V Çoğaltma, depolama işlemlerinin sayısını en iyi duruma getirmek için kurtarma sunucusunda az miktarda bellek kullanır. Grafik, kurtarma sunucusundaki bellek kullanımını özetler. Gösterilen bellek yükü, Hyper-V sunucusundaki toplam yüklü bellekle karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesidir.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744916.png)

Kurtarma sitesindeki G/Ç işlemlerinin miktarı, birincil sitedeki yazma işlemi sayısının bir fonksiyonudur. Kurtarma alanındaki toplam G/Ç işlemlerine, toplam G/Ç işlemleriyle karşılaştırıldığında bakalım ve birincil sitedeki işlemleri yazalım. Grafikler, kurtarma alanındaki toplam IOPS'nin

* Yaklaşık 1.5 kez birincil IOPS yazmak.
* Birincil sitede toplam IOPS yaklaşık% 37.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744917.png)

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Ağ kullanımı üzerindeki etkisi

Birincil ve kurtarma düğümleri (sıkıştırma etkinken) arasında, saniyede 5 Gb'lık mevcut bant genişliğine karşı saniyede ortalama 275 Mb ağ bant genişliği kullanılmıştır.

![Sonuç ağı kullanımı](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM performansı üzerindeki etkisi

Önemli bir husus, çoğaltmanın sanal makinelerde çalışan üretim iş yükleri üzerindeki etkisidir. Birincil site çoğaltma için yeterli şekilde sağlanmışsa, iş yükleri üzerinde herhangi bir etkisi olmamalıdır. Hyper-V Replica'nın hafif izleme mekanizması, sanal makinelerde çalışan iş yüklerinin sabit durum çoğaltma sırasında etkilenmemesini sağlar. Bu, aşağıdaki grafiklerde gösterilmiştir.

Bu grafik, çoğaltma etkinleştirilmeden önce ve sonra, farklı iş yükleri çalıştıran sanal makineler tarafından gerçekleştirilen IOPS gösterir. İkisi arasında bir fark olmadığını gözlemleyebilirsiniz.

![Çoğaltma efekti sonuçları](./media/hyper-v-vmm-performance-results/IC744920.png)

Aşağıdaki grafik, çoğaltma etkinleştirilmeden önce ve sonra farklı iş yüklerinde çalışan sanal makinelerin iş kısmını gösterir. Çoğaltmanın önemli bir etkisi olmadığını gözlemleyebilirsiniz.

![Sonuç çoğaltma efektleri](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Sonuç

Sonuçlar açıkça Site Kurtarma, Hyper-V Çoğaltma ile birleştiğinde, büyük bir küme için minimum yükü ile iyi ölçekler göstermektedir. Site Kurtarma basit dağıtım, çoğaltma, yönetim ve izleme sağlar. Hyper-V Çoğaltma başarılı çoğaltma ölçekleme için gerekli altyapıyı sağlar. 

## <a name="test-environment-details"></a>Test ortamı ayrıntıları

### <a name="primary-site"></a>Birincil site

* Birincil site, 470 sanal makine çalıştıran beş Hyper-V sunucusu içeren bir kümeye sahiptir.
* VM'ler farklı iş yükleri çalıştırıyor ve hepsi site kurtarma koruması etkin.
* Küme düğümü için depolama bir iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.
* Her küme sunucusunda her biri bir Gbps'den oluşan dört ağ kartı (NIC) vardır.
* Ağ kartlarından ikisi iSCSI özel ağına, ikisi de harici bir kuruluş ağına bağlıdır. Dış ağlardan biri yalnızca küme iletişimi için ayrılmıştır.

![Birincil donanım gereksinimleri](./media/hyper-v-vmm-performance-results/IC744922.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümedeki Hyper-V sunucuları: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128<br />ESTLAB-HOST25 256 var |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server Veritabanı 2012 R2 (x64) + VMM 2012 R2 |

### <a name="secondary-site"></a>İkincil site

* İkincil sitealtı düğümlü bir failover kümesine sahiptir.
* Küme düğümü için depolama bir iSCSI SAN tarafından sağlanır. Model – Hitachi HUS130.

![Birincil donanım belirtimi](./media/hyper-v-vmm-performance-results/IC744923.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümedeki Hyper-V sunucuları: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 \@ 2.30GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| ESTLAB-HOST17 |128 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| ESTLAB-HOST24 |256 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server Veritabanı 2012 R2 (x64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Sunucu iş yükleri

* Test amacıyla, kurumsal müşteri senaryolarında yaygın olarak kullanılan iş yüklerini seçtik.
* [IOMeter'ı](http://www.iometer.org) simülasyon tablosunda özetlenen iş yükü özelliğiyle birlikte kullanıyoruz.
* Tüm IOMeter profilleri, iş yükleri için en kötü durum yazma desenlerini simüle etmek için rasgele baytlar yazacak şekilde ayarlanmıştir.

| İş yükü | G/Ç boyutu (KB) | % Erişim | %Okuma | Olağanüstü I/Os | G/Ç deseni |
| --- | --- | --- | --- | --- | --- |
| Dosya Sunucusu |4<br />8<br />16<br />32<br />64 |%60<br />%20<br />%5<br />%5<br />%10 |%80<br />%80<br />%80<br />%80<br />%80 |8<br />8<br />8<br />8<br />8 |Tüm% 100 rasgele |
| SQL Server (cilt 1)<br />SQL Server (cilt 2) |8<br />64 |%100<br />%100 |%70<br />%0 |8<br />8 |%100 rastgele<br />%100 sıralı |
| Exchange |32 |%100 |%67 |8 |%100 rastgele |
| İş İstasyonu/VDI |4<br />64 |66%<br />34% |%70<br />95% |1<br />1 |Her ikisi de% 100 rastgele |
| Web Dosya Sunucusu |4<br />8<br />64 |33%<br />34%<br />33% |95%<br />95%<br />95% |8<br />8<br />8 |Tüm 75% rasgele |

### <a name="vm-configuration"></a>VM yapılandırması

* Birincil kümede 470 VM.
* VHDX diskli tüm VM'ler.
* VM'ler tabloda özetlenen iş yüklerini çalıştırıyor. Tüm VMM şablonları ile oluşturuldu.

| İş yükü | # VMs | Minimum RAM (GB) | Maksimum RAM (GB) | VM başına mantıksal disk boyutu (GB) | Maksimum IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Dosya Sunucusu |50 |1 |2 |552 |22 |
| Vdı |149 |.5 |1 |80 |6 |
| Web sunucusu |149 |.5 |1 |80 |6 |
| TOPLAM |470 | | |96,83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Kurtarma ayarları

* Site Kurtarma şirket içi koruma için yapılandırılmıştır
* VMM sunucusunda Hyper-V küme sunucularını ve VM'lerini içeren yapılandırılmış dört bulut vardır.

| Birincil VMM bulutu | Korumalı VM'ler | Çoğaltma frekansı | Ek kurtarma noktaları |
| --- | --- | --- | --- |
| BirincilCloudRpo15m |142 |15 dakika |None |
| BirincilCloudRpo30s |47 |30 saniye |None |
| BirincilCloudRpo30sArp1 |47 |30 saniye |1 |
| BirincilCloudRpo5m |235 |5 dk. |None |

### <a name="performance-metrics"></a>Performans ölçümleri

Tablo, dağıtımda ölçülen performans ölçümlerini ve sayaçlarını özetler.

| Ölçüm | Sayaç |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Kullanılabilir bellek |\Bellek\Kullanılabilir MBytes |
| IOPS |\PhysicalDisk(_Total)\Disk Aktarımları/sn |
| VM okuma (IOPS) işlemleri/sn |\Hyper-V Sanal Depolama\<Aygıtı(VHD>)\Okuma İşlemleri/Sn |
| VM yazma (IOPS) işlemleri/sn |\Hyper-V Sanal Depolama\<Aygıtı(VHD>)\Yazma İşlemleri/S |
| VM okuma iş |\Hyper-V Sanal Depolama\<Aygıtı(VHD>)\Okunma Bayt/sn |
| VM yazma iş |\Hyper-V Sanal Depolama\<Aygıtı(VHD>)\Bayt/sn Yaz |

## <a name="next-steps"></a>Sonraki adımlar

[Çoğaltmayı ayarlama](hyper-v-vmm-disaster-recovery.md)
