---
title: Azure'da SAP HANA'nın yüksek kullanılabilirliği ve olağanüstü durum kurtarma durumu (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA'nın olağanüstü durum kurtarma durumu için yüksek kullanılabilirlik oluşturma ve plan (Büyük Örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0585c1251ba18e1390f3eee28a989edee6eb8591
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616939"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA Azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma örnekleri 

>[!IMPORTANT]
>Bu belge, SAP HANA yönetim belgelerinin veya SAP Notlarının yerini almaz. Okuyucunun SAP HANA yönetimi ve operasyonlarında, özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma konularında sağlam bir anlayışa ve uzmanlığa sahip olması beklenmektedir.

Ortamınızda ve HANA sürümleriniz ve sürümleriniz ile atılan adımları ve süreçleri uygulamanız önemlidir. Bu dokümantasyonda açıklanan bazı işlemler daha iyi bir genel anlayış için basitleştirilmiştir ve nihai işlem el kitapları için ayrıntılı adımlar olarak kullanılmamalıdır. Yapılandırmalarınız için işlem el kitapları oluşturmak istiyorsanız, işlemlerinizi sınamanız ve uygulamanız ve belirli yapılandırmalarınizle ilgili işlemleri belgelemeniz gerekir. 


Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DR), görev açısından kritik SAP HANA'nızı Azure (Büyük Örnekler) sunucusunda çalıştırmanın önemli yönleridir. Doğru yüksek kullanılabilirlik ve olağanüstü durum kurtarma stratejilerini doğru şekilde mimar ve uygulamak için SAP, sistem entegratörünüz veya Microsoft ile çalışmak önemlidir. Ayrıca, çevrenize özgü kurtarma noktası hedefini (RPO) ve kurtarma süresi hedefini göz önünde bulundurmak da önemlidir.

Microsoft, HANA Large Instances ile bazı SAP HANA yüksek kullanılabilirlik özelliklerini destekler. Bu özellikler şunları içerir:

- **Depolama çoğaltma**: Depolama sisteminin tüm verileri başka bir Azure bölgesindeki başka bir HANA Büyük Örnek damgasına kopyalama yeteneği. SAP HANA bu yöntemden bağımsız olarak çalışır. Bu işlevsellik, HANA Büyük Örnekleri için sunulan varsayılan olağanüstü durum kurtarma mekanizmasıdır.
- **HANA sistem çoğaltma**: [SAP HANA'daki tüm verilerin](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) ayrı bir SAP HANA sistemine çoğaltılması. Kurtarma süresi hedefi düzenli aralıklarla veri çoğaltma yoluyla en aza indirilir. SAP HANA, bellekte eşzamanlı ve eşzamanlı modları destekler. Senkron mod yalnızca aynı veri merkezi içinde veya 100 km'den daha az bir arayla SAP HANA sistemleri için kullanılır. HANA Büyük Örnek pullarının geçerli tasarımı yla, HANA sistem çoğaltma yalnızca bir bölgede yüksek kullanılabilirlik için kullanılabilir. HANA sistem çoğaltma, başka bir Azure bölgesine olağanüstü durum kurtarma yapılandırmaları için üçüncü taraf ters proxy veya yönlendirme bileşeni gerektirir. 
- **Ana bilgisayar otomatik arıza:** SAP HANA için HANA sistem çoğaltmaalternatif olan yerel bir hata kurtarma çözümü. Ana düğüm kullanılamıyorsa, ölçeklendirme modunda bir veya daha fazla bekleme SAP HANA düğümlerini yapılandırırsınız ve SAP HANA otomatik olarak bekleme düğümünde başarısız olur.

Azure'daki SAP HANA (Büyük Örnekler) dört jeopolitik alanda (ABD, Avustralya, Avrupa ve Japonya) iki Azure bölgesinde sunulur. HANA Büyük Örnek pullarını barındıran jeopolitik bir alan içinde yer alan iki bölge ayrı özel ağ devrelerine bağlanır. Bunlar, olağanüstü durum kurtarma yöntemlerini sağlamak için depolama anlık görüntülerini çoğaltmak için kullanılır. Çoğaltma varsayılan olarak kurulmadı, ancak olağanüstü durum kurtarma işlevini sipariş eden müşteriler için ayarlanır. Depolama çoğaltma HANA Büyük Örnekleri için depolama anlık kullanıma bağlıdır. Farklı bir jeopolitik alanda bulunan bir DR bölgesi olarak bir Azure bölgesi seçmek mümkün değildir. 

Aşağıdaki tablo, şu anda desteklenen yüksek kullanılabilirlik ve olağanüstü durum kurtarma yöntemleri ni ve birleşimlerini gösterir:

| HANA Büyük Örneklerinde desteklenen senaryo | Yüksek kullanılabilirlik seçeneği | Olağanüstü durum kurtarma seçeneği | Yorumlar |
| --- | --- | --- | --- |
| Tek düğüm | Kullanılamıyor. | Özel DR kurulumu.<br /> Çok amaçlı DR kurulumu. | |
| Ana bilgisayar otomatik arıza: Ölçeklendirme (bekleme de veya bekleme olmadan)<br /> dahil 1+1 | Bekleme aktif rol alarak mümkün.<br /> HANA rol anahtarını kontrol eder. | Özel DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltma kullanarak DR senkronizasyonu. | HANA ses kümeleri tüm düğümlere iliştirilir.<br /> DR sitesi aynı sayıda düğüme sahip olmalıdır. |
| HANA sistem çoğaltma | Birincil veya ikincil kurulum la mümkündür.<br /> Bir başarısız lık durumunda birincil role ikincil hamle.<br /> HANA sistem çoğaltma ve işletim sistemi kontrolü başarısız oldu. | Özel DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltma kullanarak DR senkronizasyonu.<br /> HANA sistem çoğaltma kullanarak DR henüz üçüncü taraf bileşenleri olmadan mümkün değildir. | Her düğüme ayrı disk birimleri kümesi eklenir.<br /> Üretim alanındaki ikincil çoğaltmanın yalnızca disk hacimleri DR konumuna çoğaltılır.<br /> DR sitesinde bir birim kümesi gereklidir. | 

Özel bir DR kurulumu, DR sitesindeki HANA Büyük Örnek ünitesinin başka bir iş yükü veya üretim dışı sistemi çalıştırmak için kullanılmadığı yerdir. Birim pasiftir ve yalnızca bir felaket failover yürütülür dağıtılır. Ancak, bu kurulum birçok müşteri için tercih edilen bir seçim değildir.

Mimariniz için depolama düzeni ve ethernet ayrıntılarını öğrenmek için [HLI destekli senaryolara](hana-supported-scenario.md) bakın.

> [!NOTE]
> Biş koyma senaryoları tabloda listelenen HA ve DR yöntemleriyle birlikte çalıştığı için [SAP HANA MCOD dağıtımları](https://launchpad.support.sap.com/#/notes/1681092) (bir birimde birden çok HANA Örneği). Bir istisna Pacemaker dayalı otomatik bir failover küme ile HANA Sistem Çoğaltma kullanımıdır. Böyle bir servis talebi birim başına yalnızca bir HANA örneğini destekler. [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000) dağıtımları için, birden fazla kiracı dağıtılırsa yalnızca depolama tabanlı olmayan HA ve DR yöntemleri çalışır. Bir kiracı dağıtılmışsa, listelenen tüm yöntemler geçerlidir.  

Çok amaçlı DR kurulumu, DR sitesindeki HANA Büyük Örnek biriminin üretim dışı iş yükünü çalıştırdığı yerdir. Felaket durumunda, üretim dışı sistemi kapatın, depolama da mı (ek) birim kümeleri monte edin ve sonra üretim HANA örneğini başlatın. HANA Büyük Örnek olağanüstü durum kurtarma işlevini kullanan çoğu müşteri bu yapılandırmayı kullanır. 


Aşağıdaki SAP makalelerinde SAP HANA yüksek kullanılabilirliği hakkında daha fazla bilgi bulabilirsiniz: 

- [SAP HANA Yüksek Kullanılabilirlik Teknik İnceleme](https://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA Yönetim Rehberi](https://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP HANA Akademi Video SAP HANA Sistem Çoğaltma](https://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP Destek Notu #1999880 – SAP HANA Sistem Çoğaltma sıkça](https://apps.support.sap.com/sap/support/knowledge/preview/en/1999880)
- [SAP Destek Notu #2165547 – SAP HANA Geri Yükleme ve SAP HANA Sistem Çoğaltma Ortamında Geri Yükleme](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP Destek Notu #1984882 – Minimum/Sıfır Kapalı Kalma Süresi ile Donanım Değişimi için SAP HANA Sistem Çoğaltma kullanma](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>HANA Büyük Örnekleri ile olağanüstü durum kurtarma için ağ hususlar

HANA Büyük Örnekleri'nin olağanüstü durum kurtarma işlevinden yararlanmak için iki Azure bölgesine ağ bağlantısı tasarlamanız gerekir. Ana Azure bölgenizdeki şirket içinden bir Azure ExpressRoute devre bağlantısına ve şirket içinde olağanüstü durum kurtarma bölgenize başka bir devre bağlantısına ihtiyacınız vardır. Bu önlem, Bir Azure bölgesinde Microsoft Enterprise Edge Router (MSEE) konumu da dahil olmak üzere bir sorun olan bir durumu kapsar.

İkinci bir önlem olarak, bir bölgedeki SAP HANA'ya (Büyük Örnekler) bağlanan tüm Azure sanal ağlarını, diğer bölgedeki HANA Büyük Örnekleri'ni bağlayan bir ExpressRoute devresine bağlayabilirsiniz. Bu *çapraz bağlantı*yla, Bölge 1'deki bir Azure sanal ağında çalışan hizmetler, Bölge 2'deki HANA Büyük Örnek birimlerine ve tam tersi şekilde bağlanabilir. Bu önlem, Azure ile şirket içi konumunuza bağlanan MSEE konumlarından yalnızca birinin çevrimdışı olduğu bir durumu gidermektedir.

Aşağıdaki grafik, olağanüstü durum kurtarma durumları için esnek bir yapılandırmayı göstermektedir:

![Olağanüstü durum kurtarma için en uygun yapılandırma](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>Olağanüstü durum kurtarma için HANA Büyük Örnekler depolama çoğaltma ile diğer gereksinimleri

HANA Büyük Örnekleri ile bir olağanüstü durum kurtarma kurulumu için önceki gereksinimlere ek olarak, şunları

- Üretim SUK'larınız ile aynı boyuttaki Azure (Büyük Örnekler) SUK'ları için SAP HANA siparişedin ve bunları olağanüstü durum kurtarma bölgesinde dağıtın. Geçerli müşteri dağıtımlarında, bu örnekler üretim dışı HANA örneklerini çalıştırmak için kullanılır. Bu yapılandırmalar çok *amaçlı DR kurulumları*olarak adlandırılır.   
- Olağanüstü durum kurtarma sitesinde kurtarmak istediğiniz Azure (Büyük Örnekler) SUK'larınızın her biri için DR sitesinde ek depolama alanı sipariş edin. Ek depolama alanı satın almak, depolama hacimlerini ayırmanıza olanak tanır. Üretim Azure bölgenizden depolama çoğaltmanın hedefi olan birimleri olağanüstü durum kurtarma Azure bölgesine ayırabilirsiniz.
- Birincil de HSR kurulumuna sahip olduğunuz ve depolamayı DR sitesine dayandırdığınız durumlarda, hem birincil hem de ikincil düğüm verilerinin DR sitesine çoğaltılması için DR sitesinde ek depolama alanı satın almanız gerekir.

  **Sonraki adımlar**
- [Bkz. Yedekleme ve geri yükleme.](hana-backup-restore.md)













