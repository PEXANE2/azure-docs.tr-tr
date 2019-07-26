---
title: VMM bulutlarındaki Hyper-V VM 'lerinin Azure Site Recovery bir ikincil siteye çoğaltılmasına yönelik test sonuçları | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak VMM bulutlarındaki Hyper-V VM 'lerinin ikincil bir siteye çoğaltılmasına yönelik performans testi hakkında bilgi sağlanır.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi
ms.openlocfilehash: a7413b2dcb24a42092eb2af9816b1d29a8306e19
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377216"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>İkincil siteye Hyper-V çoğaltması için test sonuçları


Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini ikincil veri merkezine çoğaltılırken performans testinin sonuçları sağlanmaktadır.

## <a name="test-goals"></a>Test hedefleri

Testin amacı, Site Recovery kararlı durum çoğaltması sırasında nasıl gerçekleştiğini incelemektir.

- Sabit durum çoğaltma, VM 'Lerin ilk çoğaltmasını tamamladığı ve Delta değişikliklerini eşitlemekte meydana gelir.
- Beklenmeyen kesintiler gerçekleşmedikleri takdirde, çoğu VM 'nin kalacağı durum olduğundan, düzenli durum kullanılarak performansı ölçmek önemlidir.
- Test dağıtımı, her sitede bir VMM sunucusu ile iki şirket içi siteden oluşur. Bu test dağıtımı, Merkez Ofis/Şube ofisi dağıtımının, birincil site olarak hareket eden ve şube ofisi ikincil veya kurtarma sitesi olarak çalışan bir merkez ofisidir.

## <a name="what-we-did"></a>Yaptığımız

İşte test geçişimiz:

1. VMM şablonları kullanılarak VM 'Ler oluşturuldu.
2. Başlatılmış VM 'Ler ve 12 saat boyunca yakalanan temel performans ölçümleri.
3. Birincil ve kurtarma VMM sunucularında bulutlar oluşturuldu.
4. Kaynak ve kurtarma bulutları arasında eşleme de dahil olmak üzere Site Recovery olarak yapılandırılmış çoğaltma.
5. Sanal makineler için koruma etkinleştirildi ve ilk çoğaltmayı tamammaları için izin verildi.
6. Sistem sabitlemesi için birkaç saat bekledi.
7. 12 saat üzerinden yakalanan performans ölçümleri, tüm VM 'Lerin bu 12 saat için beklenen bir çoğaltma durumunda kaldığı.
8. Taban çizgisi performans ölçümleri ve çoğaltma performansı ölçümleri arasındaki Delta değerini ölçülür.


## <a name="primary-server-performance"></a>Birincil sunucu performansı

* Hyper-V çoğaltma (Site Recovery tarafından kullanılan), bir günlük dosyasındaki değişiklikleri zaman uyumsuz olarak birincil sunucuda en düşük depolama ek yüküne göre izler.
* Hyper-V çoğaltma, izleme için ıOPS yükünü en aza indirmek üzere kendi kendine korunan bellek önbelleğini kullanır. Yazma işlemlerini bellekte depoladığında depolar ve günlüğü kurtarma sitesine gönderilmeden önce günlük dosyasına boşaltır. Bir disk temizleme işlemi, yazmaları önceden belirlenmiş bir sınıra ulaşıyorsa da oluşur.
* Aşağıdaki grafikte, çoğaltma için sabit durum ıOPS ek yükü gösterilmektedir. Çoğaltma nedeniyle ıOPS ek yükünün% 5 ' ün üzerinde olduğunu görebiliriz. Bu, oldukça düşüktür.

  ![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V çoğaltma, disk performansını iyileştirmek için birincil sunucuda bellek kullanır. Aşağıdaki grafikte gösterildiği gibi, birincil kümedeki tüm sunuculardaki bellek ek yükü marguinal olur. Gösterilen bellek yükü, Hyper-V sunucusundaki toplam yüklü bellekle karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesidir.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V çoğaltmasında en düşük CPU ek yükü vardır. Grafikte gösterildiği gibi, çoğaltma ek yükü% 2-3 aralığındadır.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>İkincil sunucu performansı

Hyper-V çoğaltma, depolama işlemlerinin sayısını iyileştirmek için kurtarma sunucusunda az miktarda bellek kullanır. Grafik, kurtarma sunucusundaki bellek kullanımını özetler. Gösterilen bellek yükü, Hyper-V sunucusundaki toplam yüklü bellekle karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesidir.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744916.png)

Kurtarma sitesindeki g/ç işlemlerinin miktarı, birincil sitede yazma işlemi sayısı işlevidir. Birincil sitedeki toplam g/ç işlemleri ve yazma işlemleri ile karşılaştırıldığında, kurtarma sitesindeki toplam g/ç işlemlerine göz atalım. Grafiklerde, kurtarma sitesindeki toplam ıOPS 'nin olduğunu gösterir

* Birincil üzerinde yazma ıOPS 1,5 ' dir.
* Birincil sitedeki toplam ıOPS 'nin% 37 ' i.

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744917.png)

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Ağ kullanımı üzerindeki etki

Birincil ve kurtarma düğümleri arasında (sıkıştırma etkin), saniye başına 5 GB olan mevcut bir bant genişliğine karşı bir ortalama 275 MB 'Lık ağ bant genişliği kullanılır.

![Sonuç ağ kullanımı](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM performansı üzerindeki etki

Önemli bir değerlendirme, sanal makinelerde çalışan üretim iş yüklerindeki çoğaltmanın etkisidir. Birincil site çoğaltma için yeterli şekilde sağlandıysa, iş yükleri üzerinde herhangi bir etkisi olmamalıdır. Hyper-V çoğaltmasının hafif izleme mekanizması, sanal makinelerde çalışan iş yüklerinin kararlı durum çoğaltma sırasında etkilenmemesini sağlar. Bu, aşağıdaki grafiklerde gösterilmiştir.

Bu grafik, çoğaltma etkinleştirilmeden önce ve sonra farklı iş yükleri çalıştıran sanal makineler tarafından gerçekleştirilen ıOPS 'yi gösterir. İkisi arasında fark olmadığını gözlemleyebilirsiniz.

![Çoğaltma efekti sonuçları](./media/hyper-v-vmm-performance-results/IC744920.png)

Aşağıdaki grafikte, çoğaltma etkinleştirilmeden önce ve sonra farklı iş yüklerini çalıştıran sanal makinelerin aktarım hızı gösterilmektedir. Çoğaltmanın önemli bir etkisi olmadığını gözlemleyebilirsiniz.

![Sonuçlar çoğaltma etkileri](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Sonuç

Sonuçlar, Hyper-V çoğaltmayla bağlanmış Site Recovery açıkça gösterir, büyük bir küme için en düşük ek yüke göre ölçeklenebilen. Site Recovery basit dağıtım, çoğaltma, yönetim ve izleme sağlar. Hyper-V çoğaltma, başarılı çoğaltma ölçeklendirmesi için gerekli altyapıyı sağlar. 

## <a name="test-environment-details"></a>Test ortamı ayrıntıları

### <a name="primary-site"></a>Birincil site

* Birincil sitenin, 470 sanal makine çalıştıran beş Hyper-V sunucusu içeren bir kümesi vardır.
* VM 'Ler farklı iş yüklerini çalıştırır ve tümünün Site Recovery koruması etkinleştirilmiştir.
* Küme düğümü için depolama, bir Iscsı SAN tarafından sağlanır. Model – Hitachi HUS130.
* Her küme sunucusunda dört GB/sn 'lik bir ağ kartı (NIC) vardır.
* Ağ kartlarının ikisi bir Iscsı özel ağına bağlıdır ve iki harici bir kurumsal ağa bağlanır. Dış ağlardan biri yalnızca küme iletişimleri için ayrılmıştır.

![Birincil donanım gereksinimleri](./media/hyper-v-vmm-performance-results/IC744922.png)

| Sunucusu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümedeki Hyper-V sunucuları: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128<br />ESTLAB-HOST25 256 |Dell™ PowerEdge™ R820 |Intel (r) Xeon (r) CPU E5-4620 0 \@ 2.20 GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server veritabanı 2012 R2 (x64) + VMM 2012 R2 |

### <a name="secondary-site"></a>İkincil site

* İkincil sitede altı düğümlü bir yük devretme kümesi vardır.
* Küme düğümü için depolama, bir Iscsı SAN tarafından sağlanır. Model – Hitachi HUS130.

![Birincil donanım belirtimi](./media/hyper-v-vmm-performance-results/IC744923.png)

| Sunucusu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümedeki Hyper-V sunucuları: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel (r) Xeon (r) CPU E5-2630 0 \@ 2.30 GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST17 |128 |Dell™ PowerEdge™ R820 |Intel (r) Xeon (r) CPU E5-4620 0 \@ 2.20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST24 |256 |Dell™ PowerEdge™ R820 |Intel (r) Xeon (r) CPU E5-4620 0 \@ 2.20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Sunucusu |2 | | |2 |1 Gbps |Windows Server veritabanı 2012 R2 (x64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Sunucu iş yükleri

* Test amacıyla kurumsal müşteri senaryolarında yaygın olarak kullanılan iş yüklerini seçtik.
* Simülasyonu için tabloda özetlenen iş yükü özelliği ile [Iometer](http://www.iometer.org) kullanıyoruz.
* Tüm Iometer profilleri, iş yükleri için en kötü durum yazma desenlerinin benzetimini yapmak için rastgele baytlar yazmak üzere ayarlanır.

| İş yükü | G/ç boyutu (KB) | Erişim yüzdesi | % Okuma | Bekleyen g/ç | G/ç stili |
| --- | --- | --- | --- | --- | --- |
| Dosya Sunucusu |4<br />8<br />16<br />32<br />64 |60%<br />%20<br />%5<br />%5<br />%10 |%80<br />%80<br />%80<br />%80<br />%80 |8<br />8<br />8<br />8<br />8 |Tüm% 100 rastgele |
| SQL Server (birim 1)<br />SQL Server (birim 2) |8<br />64 |100%<br />100% |70%<br />0% |8<br />8 |% 100 rastgele<br />% 100 sıralı |
| Exchange |32 |100% |67% |8 |% 100 rastgele |
| İş istasyonu/VDı |4<br />64 |% 66<br />% 34 |70%<br />% 95 |1\.<br />1\. |Hem% 100 rastgele |
| Web dosya sunucusu |4<br />8<br />64 |% 33<br />% 34<br />% 33 |% 95<br />% 95<br />% 95 |8<br />8<br />8 |Tüm% 75 rastgele |

### <a name="vm-configuration"></a>VM yapılandırması

* birincil kümedeki 470 VM 'Ler.
* VHDX diski olan tüm VM 'Ler.
* Tabloda özetlenen iş yüklerini çalıştıran VM 'Ler. Tümü VMM şablonlarıyla oluşturulmuştur.

| İş yükü | VM 'Ler | En düşük RAM (GB) | Maksimum RAM (GB) | VM başına mantıksal disk boyutu (GB) | Maksimum ıOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1\. |4 |167 |10 |
| Exchange Server |71 |1\. |4 |552 |10 |
| Dosya Sunucusu |50 |1\. |2 |552 |22 |
| VDI |149 |.5 |1\. |80 |6 |
| Web sunucusu |149 |.5 |1\. |80 |6 |
| TOPLAM |470 | | |96,83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery ayarları

* Site Recovery şirket içi ve şirket içi koruma için yapılandırıldı
* VMM sunucusunda, Hyper-V küme sunucularını ve bunların sanal makinelerini içeren dört bulut yapılandırılır.

| Birincil VMM bulutu | Korunan VM'ler | Çoğaltma sıklığı | Ek kurtarma noktaları |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 dakika |Yok. |
| PrimaryCloudRpo30s |47 |30 saniye |Yok. |
| PrimaryCloudRpo30sArp1 |47 |30 saniye |1\. |
| PrimaryCloudRpo5m |235 |5 dk. |Yok. |

### <a name="performance-metrics"></a>Performans ölçümleri

Tablo, dağıtımda ölçülen performans ölçümlerini ve sayaçlarını özetler.

| Ölçüm | Sayaç |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Available memory |\Memory\Available MBytes |
| IOPS |\PhysicalDisk(_Total)\Disk Transfers/sec |
| VM read (IOPS) operations/sec |\Hyper-V Virtual Storage Device(\<VHD>)\Read Operations/Sec |
| VM write (IOPS) operations/sec |\Hyper-V Virtual Storage Device(\<VHD>)\Write Operations/S |
| VM read throughput |\Hyper-V Virtual Storage Device(\<VHD>)\Read Bytes/sec |
| VM write throughput |\Hyper-V Virtual Storage Device(\<VHD>)\Write Bytes/sec |

## Next steps

[Set up replication](hyper-v-vmm-disaster-recovery.md)\Processor(_Total)\` Processor Timeation of Hyper-V VMs in VMM clouds to a secondary site with Azure Site Recovery | Microsoft Docs
description: This article provides information about performance testing for replication of Hyper-V VMs in VMM clouds to a secondary site using Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi

---
# Test results for Hyper-V replication to a secondary site


This article provides the results of performance testing when replicating Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds, to a secondary datacenter.

## Test goals

The goal of testing was to examine how Site Recovery performs during steady state replication.

- Steady state replication occurs when VMs have completed initial replication, and are synchronizing delta changes.
- It’s important to measure performance using steady state, because it’s the state in which most VMs remain, unless unexpected outages occur.
- The test deployment consisted of two on-premises sites, with a VMM server in each site. This test deployment is typical of a head office/branch office deployment, with head office acting as the primary site, and the branch office as the secondary or recovery site.

## What we did

Here's what we did in the test pass:

1. Created VMs using VMM templates.
2. Started VMs, and captured baseline performance metrics over 12 hours.
3. Created clouds on the primary and recovery VMM servers.
4. Configured replication in Site Recovery, including mapping between source and recovery clouds.
5. Enabled protection for VMs, and allowed them to complete initial replication.
6. Waited a couple of hours for system stabilization.
7. Captured performance metrics over 12 hours, where all VMs remained in an expected replication state for those 12 hours.
8. Measured the delta between the baseline performance metrics, and the replication performance metrics.


## Primary server performance

* Hyper-V Replica (used by Site Recovery) asynchronously tracks changes to a log file, with minimum storage overhead on the primary server.
* Hyper-V Replica utilizes self-maintained memory cache to minimize IOPS overhead for tracking. It stores writes to the VHDX in memory, and flushes them into the log file before the time that the log is sent to the recovery site. A disk flush also happens if the writes hit a predetermined limit.
* The graph below shows the steady state IOPS overhead for replication. We can see that the IOPS overhead due to replication is around 5%, which is quite low.

  ![Primary results](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V Replica uses memory on the primary server, to optimize disk performance. As shown in the following graph, memory overhead on all servers in the primary cluster is marginal. The memory overhead shown is the percentage of memory used by replication, compared to the total installed memory on the Hyper-V server.

![Primary results](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V Replica has minimum CPU overhead. As shown in the graph, replication overhead is in the range of 2-3%.

![Primary results](./media/hyper-v-vmm-performance-results/IC744915.png)

## Secondary server performance

Hyper-V Replica uses a small amount of memory on the recovery server, to optimize the number of storage operations. The graph summarizes the memory usage on the recovery server. The memory overhead shown is the percentage of memory used by replication, compared to the total installed memory on the Hyper-V server.

![Secondary results](./media/hyper-v-vmm-performance-results/IC744916.png)

The amount of I/O operations on the recovery site is a function of the number of write operations on the primary site. Let’s look at the total I/O operations on the recovery site in comparison with the total I/O operations and write operations on the primary site. The graphs show that the total IOPS on the recovery site is

* Around 1.5 times the write IOPS on the primary.
* Around 37% of the total IOPS on the primary site.

![Secondary results](./media/hyper-v-vmm-performance-results/IC744917.png)

![Secondary results](./media/hyper-v-vmm-performance-results/IC744918.png)

## Effect on network utilization

An average of 275 Mb per second of network bandwidth was used between the primary and recovery nodes (with compression enabled), against an existing bandwidth of 5 Gb per second.

![Results network utilization](./media/hyper-v-vmm-performance-results/IC744919.png)

## Effect on VM performance

An important consideration is the impact of replication on production workloads running on the virtual machines. If the primary site is adequately provisioned for replication, there shouldn’t be any impact on the workloads. Hyper-V Replica’s lightweight tracking mechanism ensures that workloads running in the virtual machines are not impacted during steady-state replication. This is illustrated in the following graphs.

This graph shows IOPS performed by virtual machines running different workloads, before and after replication was enabled. You can observe that there is no difference between the two.

![Replica effect results](./media/hyper-v-vmm-performance-results/IC744920.png)

The following graph shows the throughput of virtual machines running different workloads, before and after replication was enabled. You can observe that replication has no significant impact.

![Results replica effects](./media/hyper-v-vmm-performance-results/IC744921.png)

## Conclusion

The results clearly show that Site Recovery, coupled with Hyper-V Replica, scales well with minimum overhead for a large cluster. Site Recovery provides simple deployment, replication, management and monitoring. Hyper-V Replica provides the necessary infrastructure for successful replication scaling. 

## Test environment details

### Primary site

* The primary site has a cluster containing five Hyper-V servers, running 470 virtual machines.
* The VMs run different workloads, and all have Site Recovery protection enabled.
* Storage for the cluster node is provided by an iSCSI SAN. Model – Hitachi HUS130.
* Each cluster server has four network cards (NICs) of one Gbps each.
* Two of the network cards are connected to an iSCSI private network, and two are connected to an external enterprise network. One of the external networks is reserved for cluster communications only.

![Primary hardware requirements](./media/hyper-v-vmm-performance-results/IC744922.png)

| Server | RAM | Model | Processor | Number of processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V servers in cluster: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128<br />ESTLAB-HOST25 has 256 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Server |2 | | |2 |1 Gbps |Windows Server Database 2012 R2 (x64) + VMM 2012 R2 |

### Secondary site

* The secondary site has a six-node failover cluster.
* Storage for the cluster node is provided by an iSCSI SAN. Model – Hitachi HUS130.

![Primary hardware specification](./media/hyper-v-vmm-performance-results/IC744923.png)

| Server | RAM | Model | Processor | Number of processors | NIC | Software |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V servers in cluster: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell ™ PowerEdge ™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 \@ 2.30GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST17 |128 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| ESTLAB-HOST24 |256 |Dell ™ PowerEdge ™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 \@ 2.20GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V role |
| VMM Server |2 | | |2 |1 Gbps |Windows Server Database 2012 R2 (x64) + VMM 2012 R2 |

### Server workloads

* For test purposes we picked workloads commonly used in enterprise customer scenarios.
* We use [IOMeter](http://www.iometer.org) with the workload characteristic summarized in the table for simulation.
* All IOMeter profiles are set to write random bytes to simulate worst-case write patterns for workloads.

| Workload | I/O size (KB) | % Access | %Read | Outstanding I/Os | I/O pattern |
| --- | --- | --- | --- | --- | --- |
| File Server |4<br />8<br />16<br />32<br />64 |60%<br />20%<br />5%<br />5%<br />10% |80%<br />80%<br />80%<br />80%<br />80% |8<br />8<br />8<br />8<br />8 |All 100% random |
| SQL Server (volume 1)<br />SQL Server (volume 2) |8<br />64 |100%<br />100% |70%<br />0% |8<br />8 |100% random<br />100% sequential |
| Exchange |32 |100% |67% |8 |100% random |
| Workstation/VDI |4<br />64 |66%<br />34% |70%<br />95% |1<br />1 |Both 100% random |
| Web File Server |4<br />8<br />64 |33%<br />34%<br />33% |95%<br />95%<br />95% |8<br />8<br />8 |All 75% random |

### VM configuration

* 470 VMs on the primary cluster.
* All VMs with VHDX disk.
* VMs running workloads summarized in the table. All were created with VMM templates.

| Workload | # VMs | Minimum RAM (GB) | Maximum RAM (GB) | Logical disk size (GB) per VM | Maximum IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| File Server |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Web server |149 |.5 |1 |80 |6 |
| TOTAL |470 | | |96.83 TB |4108 |

### Site Recovery settings

* Site Recovery was configured for on-premises to on-premises protection
* The VMM server has four clouds configured, containing the Hyper-V cluster servers and their VMs.

| Primary VMM cloud | Protected VMs | Replication frequency | Additional recovery points |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 mins |None |
| PrimaryCloudRpo30s |47 |30 secs |None |
| PrimaryCloudRpo30sArp1 |47 |30 secs |1 |
| PrimaryCloudRpo5m |235 |5 mins |None |

### Performance metrics

The table summarizes the performance metrics and counters that were measured in the deployment.

| Metric | Counter |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Uygun bellek |\Bellek\kullanılabilir MBayt |
| IOPS |\Fiziksel disk (_Total) \Disk aktarımı/sn |
| VM okuma (ıOPS) işlemi/sn |\Hyper-v sanal depolama cihazı (\<VHD >) \okuma işlemi/sn |
| VM yazma (ıOPS) işlemi/sn |\Hyper-v sanal depolama cihazı (\<VHD >) \yazma işlemleri/sn |
| VM okuma performansı |\Hyper-v sanal depolama cihazı (\<VHD >) \okunan bayt/sn |
| VM yazma performansı |\Hyper-v sanal depolama cihazı (\<VHD >) \yazma bayt/sn |

## <a name="next-steps"></a>Sonraki adımlar

[Çoğaltmayı ayarlama](hyper-v-vmm-disaster-recovery.md)
