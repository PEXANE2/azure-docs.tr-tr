---
title: SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı | Microsoft Dokümanlar
description: SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 007e8d87c670376ad334c1c4e58fd93995930b78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616247"
---
# <a name="sap-ase-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı

Bu belgede, Azure IaaS'da SAP ASE dağıtılırken göz önünde bulundurulması gereken birkaç farklı alanı kapsamaktadır. Bu belgenin ön koşulu olarak, Azure belgelerinde [SAP iş yükü için Azure Sanal Makineler DBMS dağıtımı](dbms_guide_general.md) için gerekli hususları ve SAP iş [yükündeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started)diğer kılavuzları okumuş olmalısınız. Bu belge, Linux ve Windows İşletim Sistemleri üzerinde çalışan SAP ASE'yi kapsar. Azure'da desteklenen minimum sürüm SAP ASE 16.0 Patch Level 2'dir.  SAP'nin en son sürümünü ve en son Yama Düzeyini dağıtması önerilir.  Minimum SAP ASE 16.3 Patch Level 7 önerilir.  SAP'nin en son sürümü [Hedefli ASE 16.0 Yayın Çizelgesi ve CR listesi Bilgileri](https://wiki.scn.sap.com/wiki/display/SYBASE/Targeted+ASE+16.0+Release+Schedule+and+CR+list+Information)bulunabilir.

Sap uygulamaları veya yükleme ortamı konumu ile sürüm desteği hakkında ek bilgiler, bu konumlarda SAP Ürün Kullanılabilirlik Matrisi'nin yanı sıra bulunur:

- [SAP destek notu #2134316](https://launchpad.support.sap.com/#/notes/2134316)
- [SAP destek notu #1941500](https://launchpad.support.sap.com/#/notes/1941500)
- [SAP destek notu #1590719](https://launchpad.support.sap.com/#/notes/1590719)
- [SAP destek notu #1973241](https://launchpad.support.sap.com/#/notes/1973241)

Açıklama: SAP dünyası içinde ve dışında belgeler boyunca, ürünün adı Sybase ASE veya SAP ASE veya bazı durumlarda her ikisi olarak başvurulur. Tutarlı kalmak için bu belgelerde **SAP ASE** adını kullanıyoruz.

## <a name="operating-system-support"></a>İşletim sistemi desteği
SAP Ürün Kullanılabilirlik Matrisi, her SAP uygulaması için desteklenen İşletim Sistemi ve SAP Çekirdeği birleşimlerini içerir.  Linux dağıtımları SUSE 12.x, SUSE 15.x, Red Hat 7.x tam olarak desteklenir.  SAP ASE işletim sistemi olarak Oracle Linux desteklenmez.  Mevcut en son Linux sürümlerini kullanmanız önerilir. Windows müşterileri Windows Server 2016 veya Windows Server 2019 sürümlerini kullanmalıdır.  Windows 2012 gibi Windows'un eski sürümleri teknik olarak desteklenir, ancak en son Windows sürümü her zaman önerilir.


## <a name="specifics-to-sap-ase-on-windows"></a>Windows'da SAP ASE'ye özgü
Microsoft Azure'dan başlayarak, mevcut SAP ASE uygulamalarınızı Azure Sanal Makineleri'ne geçirebilirsiniz. Azure Sanal Makine'deki SAP ASE, bu uygulamaları Microsoft Azure'a kolayca geçirerek dağıtım, yönetim ve kurumsal genişlik uygulamalarının toplam sahip olma maliyetini azaltmanızı sağlar. Bir Azure Sanal Makinesi'ndeki SAP ASE ile yöneticiler ve geliştiriciler şirket içinde kullanılabilen aynı geliştirme ve yönetim araçlarını kullanmaya devam edebilir.

Microsoft Azure, en küçük SAP sistemlerini ve manzaralarını binlerce kullanıcıyla birlikte büyük SAP sistemlerine ve manzaralara kadar çalıştırmanızı sağlayan çok sayıda farklı sanal makine türü sunar. Sap boyutlandırma SAPS numaraları farklı SAP sertifikalı VM SKUs [SAP destek notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)sağlanır.

SAP ASE'yi Windows'a yüklemek için belgeler [SAP ASE Yükleme Kılavuzu'nda](https://help.sap.com/viewer/36031975851a4f82b1022a9df877280b/16.0.3.7/en-US/a660d3f1bc2b101487cbdbf10069c3ac.html) bulunabilir

Sayfaları Bellekte Kilitle, SAP ASE veritabanı arabellesinin çağrı dışı olmasını engelleyecek bir ayardır.  Bu ayar, çok fazla belleğe sahip büyük meşgul sistemler için yararlıdır. Daha fazla bilgi için BC-DB-SYB ile iletişime geçin. 


## <a name="linux-operating-system-specific-settings"></a>Linux işletim sistemi belirli ayarları
Linux VM'lerde, SAP-ASE Linux Huge Pages profiliyle çalıştırılan `saptune` lar varsayılan olarak etkinleştirilmeli ve komutla doğrulanabilir  

`cat /proc/meminfo` 

Sayfa boyutu genellikle 2048 KB'dir. Ayrıntılar için [Linux'ta](https://help.sap.com/viewer/ecbccd52e7024feaa12f4e780b43bc3b/16.0.3.7/en-US/a703d580bc2b10149695f7d838203fad.html) Büyük Sayfalar makalesine bakın 


## <a name="recommendations-on-vm-and-disk-structure-for-sap-ase-deployments"></a>SAP ASE dağıtımları için VM ve disk yapısı yla ilgili öneriler

SAP NetWeaver Uygulamaları için SAP ASE, SAP destek [notunda](https://launchpad.support.sap.com/#/notes/1928533) listelenen herhangi bir VM türünde desteklenir #1928533 orta ölçekli SAP ASE veritabanı sunucuları için kullanılan tipik VM türleri Esv3 içerir.  Büyük çok terabaytlı veritabanları M serisi VM türlerinden yararlanabilir. SAP ASE işlem günlüğü disk yazma performansı M serisi Yazma Hızlandırıcısı etkinleştirilerek geliştirilebilir. Yazma Hızlandırıcısı, SAP ASE'nin Log Writes'ı gerçekleştirme şekli nedeniyle SAP ASE ile dikkatle test edilmelidir.  [SAP destek notunu #2816580](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator) gözden geçirin ve bir performans testi çalıştırmayı düşünün.  
Yazma Hızlandırıcı yalnızca işlem günlüğü diski için tasarlanmıştır. Disk düzeyi önbelleği NONE olarak ayarlanmalıdır. Azure Yazma Hızlandırıcısı diğer DBMS'lerde olduğu gibi benzer iyileştirmeler göstermiyorsa şaşırmayın. SAP ASE'nin işlem günlüğüne yazdığı şekilden yola çıkarak, Azure Yazma Hızlandırıcısı'nın çok az veya hiç ivmelenmemesi olabilir.
Veri aygıtları ve Günlük Aygıtları için ayrı diskler önerilir.  Sistem veritabanları sybsecurity `saptools` ve özel diskler gerektirmez ve SAP veritabanı verileri ve günlük aygıtları içeren diskler yerleştirilebilir 

![SAP ASE için depolama yapılandırması](./media/dbms-guide-sap-ase/sap-ase-disk-structure.png)

### <a name="file-systems-stripe-size--io-balancing"></a>Dosya sistemleri, şerit boyutu & IO dengeleme 
SAP ASE, aksi yapılandırılmadığı sürece verileri disk depolama aygıtlarına sırayla yazar. Bu, dört aygıtiçeren boş bir SAP ASE veritabanının yalnızca ilk aygıta veri yazacağı anlamına gelir.  Diğer disk aygıtları yalnızca ilk aygıt dolduğunda yazılır.  Her SAP ASE aygıtına READ ve WRITE IO miktarı nın farklı olması olasıdır. Disk IO'yu kullanılabilir tüm Azure diskleri arasında dengelemek için Windows Depolama Alanları veya Linux LVM2'nin kullanılması gerekir. Linux'ta diskleri biçimlendirmek için XFS dosya sisteminin kullanılması önerilir. LVM şerit boyutu bir performans testi ile test edilmelidir. 128 KB şerit boyutu iyi bir başlangıç noktasıdır. Windows'da NTFS Ayırma Birimi Boyutu (AUS) sınanmalıdır. 64 KB başlangıç değeri olarak kullanılabilir. 

[SAP Adaptive Server Enterprise](https://blogs.sap.com/2014/07/09/configuring-automatic-database-space-expansion-in-sap-adaptive-server-enterprise/) ve SAP destek notu [#1815695](https://launchpad.support.sap.com/#/notes/1815695)Otomatik Veritabanı Alanı Genişletme Yapılandırma makalede açıklandığı gibi Otomatik Veritabanı Genişletme yapılandırmak için önerilir. 

### <a name="sample-sap-ase-on-azure-virtual-machine-disk-and-file-system-configurations"></a>Azure sanal makine, disk ve dosya sistemi yapılandırmalarında SAP ASE örneği 
Aşağıdaki şablonlar hem Linux hem de Windows için örnek yapılandırmaları gösterir. Sanal makine ve disk yapılandırmasını onaylamadan önce, tek tek VM'nin ağ ve depolama bant genişliği kotalarının iş gereksinimini karşılamak için yeterli olduğundan emin olun. Ayrıca, farklı Azure VM türlerinin VM'ye ekilebilen farklı maksimum disk sayısına sahip olduğunu da unutmayın. Örneğin, bir E4s_v3 VM'nin limit48 MB/sn depolama IO iş birliği vardır. Veritabanı yedekleme etkinliği tarafından gerekli depolama iştimi 48 MB/sn'den fazla gerektiriyorsa, daha fazla depolama bant genişliği elde ine sahip daha büyük bir VM türü kaçınılmazdır. Azure depolama alanını yapılandırırken, özellikle [Azure Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance) ile GB kapasite başına iş ve IOPS'nin değiştiğini de aklınızdan korumanız gerekir. Makalede bu konuda daha fazla bilgi için [Azure'da hangi disk türleri kullanılabilir?](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) Belirli Azure VM türlerine yönelik kotalar, Bellek tarafından [optimize edilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/sizes-memory) ve ona bağlı makaleler makalesinde belgelenmiştir. 

> [!NOTE]
>  Bir DBMS sistemi şirket içinde Azure'a taşınıyorsa, VM'de izleme gerçekleştirmesi ve CPU, bellek, IOPS ve depolama işbretini değerlendirmesi önerilir. Yukarıda belirtilen makalelerde belgelenen VM kota limitleri ile gözlenen tepe değerlerini karşılaştırın

Aşağıda verilen örnekler açıklayıcı amaçlar içindir ve bireysel ihtiyaçlara göre değiştirilebilir. SAP ASE'nin tasarımı nedeniyle, veri aygıtlarının sayısı diğer veritabanları kadar kritik değildir. Bu belgede ayrıntılı veri aygıtlarının sayısı yalnızca bir kılavuzdur. 

50 GB - 250 GB arasında bir veritabanı boyutu ile küçük bir SAP ASE DB Sunucusu için bir yapılandırma örneği, SAP çözüm Yöneticisi gibi görünebilir

| Yapılandırma | Windows | Linux | Yorumlar |
| --- | --- | --- | --- |
| VM Tipi | E4s_v3 (4 vCPU/32 GB RAM) | E4s_v3 (4 vCPU/32 GB RAM) | --- |
| Hızlandırılmış Ağ | Etkinleştirme | Etkinleştirme | ---|
| SAP ASE sürümü | 16.3 PL 7 veya üstü | 16.3 PL 7 veya üstü | --- |
| # veri cihazları | 4 | 4 | ---|
| # günlük cihazlar | 1 | 1 | --- |
| # geçici cihazlar | 1 | 1 | SAP BW iş yükü için daha fazla |
| İşletim sistemi | Windows Server 2019 | SUSE 12 SP4/ 15 SP1 veya RHEL 7.6 | --- |
| Disk toplama | Depolama Alanları | LVM2 | --- |
| Dosya sistemi | NTFS | XFS |
| Blok boyutunu biçimlendirme | iş yükü testi ne ihtiyacı var | iş yükü testi ne ihtiyacı var | --- |
| # ve veri disklerinin türü | Premium depolama alanı: 2 x P10 (RAID0) | Premium depolama alanı: 2 x P10 (RAID0)| Önbellek = Yalnızca Oku |
| # ve günlük disklerinin türü | Premium depolama: 1 x P20  | Premium depolama: 1 x P20 | Önbellek = YOK |
| ASE MaxMemory parametresi | Fiziksel RAM'in %90'ı | Fiziksel RAM'in %90'ı | tek örnek varsayarak |
| # yedek cihazlar | 4 | 4| --- |
| # ve yedekleme disklerinin türü | 1 | 1 | --- |


Daha küçük bir SAP Business Suite sistemi gibi 250 GB – 750 GB arasında bir veritabanı boyutuna sahip orta bir SAP ASE DB Sunucusu için bir yapılandırma örneği,

| Yapılandırma | Windows | Linux | Yorumlar |
| --- | --- | --- | --- |
| VM Tipi | E16s_v3 (16 vCPU/128 GB RAM) | E16s_v3 (16 vCPU/128 GB RAM) | --- |
| Hızlandırılmış Ağ | Etkinleştirme | Etkinleştirme | ---|
| SAP ASE sürümü | 16.3 PL 7 veya üstü | 16.3 PL 7 veya üstü | --- |
| # veri cihazları | 8 | 8 | ---|
| # günlük cihazlar | 1 | 1 | --- |
| # geçici cihazlar | 1 | 1 | SAP BW iş yükü için daha fazla |
| İşletim sistemi | Windows Server 2019 | SUSE 12 SP4/ 15 SP1 veya RHEL 7.6 | --- |
| Disk toplama | Depolama Alanları | LVM2 | --- |
| Dosya sistemi | NTFS | XFS |
| Blok boyutunu biçimlendirme | iş yükü testi ne ihtiyacı var | iş yükü testi ne ihtiyacı var | --- |
| # ve veri disklerinin türü | Premium depolama alanı: 4 x P20 (RAID0) | Premium depolama alanı: 4 x P20 (RAID0)| Önbellek = Yalnızca Oku |
| # ve günlük disklerinin türü | Premium depolama: 1 x P20  | Premium depolama: 1 x P20 | Önbellek = YOK |
| ASE MaxMemory parametresi | Fiziksel RAM'in %90'ı | Fiziksel RAM'in %90'ı | tek örnek varsayarak |
| # yedek cihazlar | 4 | 4| --- |
| # ve yedekleme disklerinin türü | 1 | 1 | --- |

Daha büyük bir SAP Business Suite sistemi gibi 750 GB – 2000 GB arasında bir veritabanı boyutuna sahip küçük bir SAP ASE DB Sunucusu için bir yapılandırma örneği,

| Yapılandırma | Windows | Linux | Yorumlar |
| --- | --- | --- | --- |
| VM Tipi | E64s_v3 (64 vCPU/432 GB RAM) | E64s_v3 (64 vCPU/432 GB RAM) | --- |
| Hızlandırılmış Ağ | Etkinleştirme | Etkinleştirme | ---|
| SAP ASE sürümü | 16.3 PL 7 veya üstü | 16.3 PL 7 veya üstü | --- |
| # veri cihazları | 16 | 16 | ---|
| # günlük cihazlar | 1 | 1 | --- |
| # geçici cihazlar | 1 | 1 | SAP BW iş yükü için daha fazla |
| İşletim sistemi | Windows Server 2019 | SUSE 12 SP4/ 15 SP1 veya RHEL 7.6 | --- |
| Disk toplama | Depolama Alanları | LVM2 | --- |
| Dosya sistemi | NTFS | XFS |
| Blok boyutunu biçimlendirme | iş yükü testi ne ihtiyacı var | iş yükü testi ne ihtiyacı var | --- |
| # ve veri disklerinin türü | Premium depolama alanı: 4 x P30 (RAID0) | Premium depolama alanı: 4 x P30 (RAID0)| Önbellek = Yalnızca Oku |
| # ve günlük disklerinin türü | Premium depolama: 1 x P20  | Premium depolama: 1 x P20 | Önbellek = YOK |
| ASE MaxMemory parametresi | Fiziksel RAM'in %90'ı | Fiziksel RAM'in %90'ı | tek örnek varsayarak |
| # yedek cihazlar | 4 | 4| --- |
| # ve yedekleme disklerinin türü | 1 | 1 | --- |


Daha büyük bir küresel olarak kullanılan SAP Business Suite sistemi gibi 2 TB+ veritabanı boyutuna sahip küçük bir SAP ASE DB Sunucusu için bir yapılandırma örneği,

| Yapılandırma | Windows | Linux | Yorumlar |
| --- | --- | --- | --- |
| VM Tipi | M Serisi (1,0 - 4,0 TB RAM)  | M Serisi (1,0 - 4,0 TB RAM) | --- |
| Hızlandırılmış Ağ | Etkinleştirme | Etkinleştirme | ---|
| SAP ASE sürümü | 16.3 PL 7 veya üstü | 16.3 PL 7 veya üstü | --- |
| # veri cihazları | 32 | 32 | ---|
| # günlük cihazlar | 1 | 1 | --- |
| # geçici cihazlar | 1 | 1 | SAP BW iş yükü için daha fazla |
| İşletim sistemi | Windows Server 2019 | SUSE 12 SP4/ 15 SP1 veya RHEL 7.6 | --- |
| Disk toplama | Depolama Alanları | LVM2 | --- |
| Dosya sistemi | NTFS | XFS |
| Blok boyutunu biçimlendirme | iş yükü testi ne ihtiyacı var | iş yükü testi ne ihtiyacı var | --- |
| # ve veri disklerinin türü | Premium depolama alanı: 4+ x P30 (RAID0) | Premium depolama alanı: 4+ x P30 (RAID0)| Önbellek = Salt Okunur, Azure Ultra diskini düşünün |
| # ve günlük disklerinin türü | Premium depolama: 1 x P20  | Premium depolama: 1 x P20 | Önbellek = YOK, Azure Ultra diski düşünün |
| ASE MaxMemory parametresi | Fiziksel RAM'in %90'ı | Fiziksel RAM'in %90'ı | tek örnek varsayarak |
| # yedek cihazlar | 16 | 16 | --- |
| # ve yedekleme disklerinin türü | 4 | 4 | LVM2/Depolama Alanlarını Kullanma |


### <a name="backup--restore-considerations-for-sap-ase-on-azure"></a>Yedekleme & Azure'da SAP ASE için dikkatleri geri yükleme
Veri ve yedekleme aygıtlarının sayısını artırmak yedekleme ve performansı geri yüklemeyi artırır. SAP ASE yedekleme aygıtını barındıran Azure disklerini daha önce gösterilen tablolarda gösterildiği gibi şeritle göstermenizi önerilir. Yedekleme aygıtları ve disklerinin sayısını dengelemeye ve yedekleme iş kısmının toplam VM iş meseninin %40-50'sini aşmamasını sağlamaya özen denmelidir. Varsayılan olarak SAP Yedekleme Sıkıştırma'nın kullanılması önerilir. Daha fazla bilgi makalelerde bulunabilir:

- [SAP destek notu #1588316](https://launchpad.support.sap.com/#/notes/1588316)
- [SAP destek notu #1801984](https://launchpad.support.sap.com/#/notes/1801984)
- [SAP destek notu #1585981](https://launchpad.support.sap.com/#/notes/1585981) 

Sürücü D kullanmayın:\ veya /temp space veritabanı veya günlük dökümü hedef olarak.

### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırma etkisi
G/Ç bant genişliğinin sınırlayıcı bir faktör haline gelebileceği yapılandırmalarda, IOPS'yi azaltan ölçüler, Azure gibi bir IaaS senaryosunda çalıştırılabilecek iş yükünü n için genişletmeye yardımcı olabilir. Bu nedenle, varolan bir SAP veritabanını Azure'a yüklemeden önce SAP ASE sıkıştırmanın kullanıldığından emin olmak önerilir.

Azure'a yüklemeden önce sıkıştırma uygulama önerisi çeşitli nedenlerden dolayı verilmiştir:

* Azure'a yüklenecek veri miktarı daha düşüktür
* Sıkıştırma yürütme süresi, daha fazla CPU veya daha yüksek G/Ç bant genişliği veya daha az G/Ç gecikmesi ile daha güçlü donanım kullanabileceğini varsayarsak daha kısadır
* Daha küçük veritabanı boyutları, disk ayırma için daha az maliyete neden olabilir

Veri ve LOB Sıkıştırma, azure sanal makinelerinde barındırılan bir VM'de şirket içinde olduğu gibi çalışır. Varolan bir SAP ASE veritabanında sıkıştırmanın zaten kullanIlip kullanılmadığını nasıl denetlenebildiğiniz hakkında daha fazla bilgi için [SAP destek notu 1750510'u](https://launchpad.support.sap.com/#/notes/1750510)işaretleyin. SAP ASE veritabanı sıkıştırma denetimi [SAP destek notu #2121797](https://launchpad.support.sap.com/#/notes/2121797) hakkında daha fazla bilgi için

## <a name="high-availability-of-sap-ase-on-azure"></a>Azure'da SAP ASE'nin yüksek kullanılabilirliği 
HADR Kullanıcıları Kılavuzu, 2 düğüm SAP ASE "Her zaman anına" çözümünün kurulum ve yapılandırmasını ayrıntılarıyla anlatır.  Buna ek olarak, üçüncü bir olağanüstü durum kurtarma düğümü de desteklenir. SAP ASE paylaşılan disk ve yerel işletim sistemi kümeleme (kayan IP) dahil olmak üzere birçok Yüksek Kullanılabilir yapılandırmaları destekler. Azure'da desteklenen tek yapılandırma, Floating IP olmadan Hata Yöneticisi kullanmaktır.  Kayan IP Adresi yöntemi Azure'da çalışmaz.  SAP Çekirdeği bir "HA Aware" uygulamasıdır ve birincil ve ikincil SAP ASE sunucularını bilir. SAP ASE ile Azure arasında yakın tümleştirme yoktur, Azure Dahili yük dengeleyicisi kullanılmaz. Bu nedenle, standart SAP ASE belgeleri [SAP ASE HADR Kullanıcı Kılavuzu](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.7/en-US/a6645e28bc2b1014b54b8815a64b87ba.html) ile başlayarak takip edilmelidir 

> [!NOTE]
> Azure'da desteklenen tek yapılandırma, Floating IP olmadan Hata Yöneticisi kullanmaktır.  Kayan IP Adresi yöntemi Azure'da çalışmaz. 

### <a name="third-node-for-disaster-recovery"></a>Olağanüstü durum kurtarma için üçüncü düğüm
Yerel yüksek kullanılabilirlik için SAP ASE Always-On'u kullanmanın ötesinde, yapılandırmayı başka bir Azure bölgesinde eşzamanlı olarak çoğaltılan bir düğüme genişletmek isteyebilirsiniz. Böyle bir senaryo için dokümantasyon [burada](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/installation-procedure-for-sybase-16-3-patch-level-3-always-on/ba-p/368199)bulabilirsiniz.

## <a name="sap-ase-database-encryption--ssl"></a>SAP ASE veritabanı şifreleme & SSL 
SAP Yazılım sağlama Yöneticisi (SWPM), yükleme sırasında veritabanını şifreleme seçeneği sunar.  Şifreleme kullanmak istiyorsanız, SAP Tam Veritabanı Şifrelemesi kullanılması önerilir.  Belgelenen ayrıntılara bakın:

- [SAP destek notu #2556658](https://launchpad.support.sap.com/#/notes/2556658)
- [SAP destek notu #2224138](https://launchpad.support.sap.com/#/notes/2224138)
- [SAP destek notu #2401066](https://launchpad.support.sap.com/#/notes/2401066)
- [SAP destek notu #2593925](https://launchpad.support.sap.com/#/notes/2593925) 

> [!NOTE]
> SAP ASE veritabanı şifrelenirse Yedek Döküm Sıkıştırma çalışmaz. Ayrıca bakınız [SAP destek notu #2680905](https://launchpad.support.sap.com/#/notes/2680905) 

## <a name="sap-ase-on-azure-deployment-checklist"></a>Azure dağıtım denetim listesinde SAP ASE
 
- SAP ASE 16.3 PL7 veya üzeri dağıtma
- Hata Yöneticisi ve SAPHostAgent'ın en son sürümü nerelerine ve düzeltme elerine güncelleştirin
- Windows 2019, Suse 15.1 veya Redhat 7.6 veya üzeri gibi en son sertifikalı işletim sistemi üzerinde dağıtma
- SAP Sertifikalı VM'leri kullanın – Es_v3 veya x-large sistemler de dahil olmak üzere yüksek bellekli Azure VM SK'ları M Serisi VM SK'ler önerilir
- VM'nin disk IOPS ve toplam VM toplam iş bölümü kotasını disk tasarımıyla eşleştirin.  Yeterli sayıda disk dağıtma
- Doğru şerit boyutu ve dosya sistemine sahip Windows Depolama Alanları veya Linux LVM2 kullanan toplu diskler
- Veri, günlük, geçici ve yedekleme amaçları için yeterli sayıda aygıt oluşturun
- X-large sistemler için UltraDisk kullanmayı düşünün 
- LINUX `saptune` OS'de SAP-ASE'yi çalıştırın 
- DB Şifreleme ile veritabanını güvenli hale getirin – tuşları Azure Key Vault'ta el ile saklayın 
- Azure [Denetim Listesi'nde SAP'yi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist) tamamlayın 
- Günlük yedeklemeve tam yedekleme yapılandırma 
- TEST HA/DR, yedekleme ve geri yükleme ve ses & stres testi gerçekleştirmek 
- Otomatik Veritabanı Uzantısının çalıştığını doğrulayın 

## <a name="using-dbacockpit-to-monitor-database-instances"></a>Veritabanı örneklerini izlemek için DBACockpit'i kullanma
SAP ASE'yi veritabanı platformu olarak kullanan SAP sistemleri için DBACockpit'e işlem DBACockpit'te gömülü tarayıcı pencereleri veya Webdynpro olarak erişilebilir. Ancak, veritabanını izlemek ve yönetmek için tam işlevsellik yalnızca DBACockpit'in Webdynpro uygulamasında kullanılabilir.

Şirket içi sistemlerde olduğu gibi, DBACockpit'in Webdynpro uygulaması tarafından kullanılan tüm SAP NetWeaver işlevlerini etkinleştirmek için birkaç adım gereklidir. Webdynpros kullanımını etkinleştirmek ve gerekli olanları oluşturmak için [SAP destek notu #1245200](https://launchpad.support.sap.com/#/notes/1245200) izleyin. Yukarıdaki notlarda talimatları izleyerek, internet iletişim yöneticisini`ICM`( ) http ve https bağlantıları için kullanılacak bağlantı noktalarıyla birlikte yapılandırırsınız. http için varsayılan ayar gibi görünüyor:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

ve işlem DBACockpit oluşturulan bağlantılar benzer:

> https:\//\<fullqualifiedhostname>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<tam nitelikli hostname>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

SAP sistemini barındıran Azure Sanal Makine'nin AD ve DNS'nize nasıl bağlı olduğuna bağlı olarak, ICM'nin DBACockpit'i açtığınız makinede çözülebilecek tam nitelikli bir ana bilgisayar adı kullandığından emin olmanız gerekir. ICM'nin profil parametrelerine göre tam nitelikli ana bilgisayar adını nasıl belirlediğini anlamak için [SAP destek notu #773830](https://launchpad.support.sap.com/#/notes/773830) bakın ve gerekirse parametre icm/host_name_full açıkça ayarlayın.

VM'yi şirket içi ve Azure arasında şirket içi bağlantı olmadan Yalnızca Bulut senaryosunda dağıttıysanız, ortak `domainlabel`bir IP adresi ve bir . VM'nin genel DNS adının biçimi aşağıdaki gibi görünür:

> `<custom domainlabel`>. `<azure region`>.cloudapp.azure.com
> 
> 

DNS adı ile ilgili daha fazla ayrıntı [burada][virtual-machines-azurerm-versus-azuresm].

SAP profil parametresi icm/host_name_full Azure VM'nin DNS adına ayarbağlantısı aşağıdakilere benzer olabilir:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

Bu durumda emin olmanız gerekir:

* ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için Azure portalındaki Ağ Güvenlik Grubu'na Gelen kurallar ekleme
* ICM ile iletişim kurmak için kullanılan TCP/IP bağlantı noktaları için Windows Güvenlik Duvarı yapılandırmasına Gelen kurallar ekleme

Kullanılabilir tüm düzeltmelerin otomatik olarak alınması için, SAP sürümünüz için geçerli düzeltme koleksiyonu SAP Notu'nu düzenli olarak uygulamanız önerilir:

* [SAP destek notu #1558958](https://launchpad.support.sap.com/#/notes/1558958)
* [SAP destek notu #1619967](https://launchpad.support.sap.com/#/notes/1619967)
* [SAP destek notu #1882376](https://launchpad.support.sap.com/#/notes/1882376)

SAP ASE için DBA Kokpit hakkında daha fazla bilgiyi aşağıdaki SAP Notları'nda bulabilirsiniz:

* [SAP destek notu #1605680](https://launchpad.support.sap.com/#/notes/1605680)
* [SAP destek notu #1757924](https://launchpad.support.sap.com/#/notes/1757924)
* [SAP destek notu #1757928](https://launchpad.support.sap.com/#/notes/1757928)
* [SAP destek notu #1758182](https://launchpad.support.sap.com/#/notes/1758182)
* [SAP destek notu #1758496](https://launchpad.support.sap.com/#/notes/1758496)    
* [SAP destek notu #1814258](https://launchpad.support.sap.com/#/notes/1814258)
* [SAP destek notu #1922555](https://launchpad.support.sap.com/#/notes/1922555)
* [SAP destek notu #1956005](https://launchpad.support.sap.com/#/notes/1956005)


## <a name="useful-links-notes--whitepapers-for-sap-ase"></a>SAP ASE için yararlı bağlantılar, notlar & teknik incelemeler
[Sybase ASE 16.3 PL7 Dokümantasyon](https://help.sap.com/viewer/product/SAP_ASE/16.0.3.7/en-US) için başlangıç sayfası çeşitli belgelere bağlantılar verir:

- SAP ASE Öğrenme Yolculuğu - Yönetim & İzleme
- SAP ASE Öğrenme Yolculuğu - Yükleme & Yükseltme

yararlıdır. Başka bir yararlı belge [SAP Uygulamaları SAP Adaptive Server Enterprise Geçiş ve Runtime için En İyi Uygulamalar.](https://assets.cdn.sap.com/sapcom/docs/2016/06/26450353-767c-0010-82c7-eda71af511fa.pdf)

Diğer yararlı SAP destek notları şunlardır:

- [SAP destek notu #2134316](https://launchpad.support.sap.com/#/notes/2134316) 
- [SAP destek notu #1748888](https://launchpad.support.sap.com/#/notes/1748888) 
- [SAP destek notu #2588660](https://launchpad.support.sap.com/#/notes/2588660) 
- [SAP destek notu #1680803](https://launchpad.support.sap.com/#/notes/1680803) 
- [SAP destek notu #1724091](https://launchpad.support.sap.com/#/notes/1724091) 
- [SAP destek notu #1775764](https://launchpad.support.sap.com/#/notes/1775764) 
- [SAP destek notu #2162183](https://launchpad.support.sap.com/#/notes/2162183) 
- [SAP destek notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)
- [SAP destek notu #2015553](https://launchpad.support.sap.com/#/notes/2015553)
- [SAP destek notu #1750510](https://launchpad.support.sap.com/#/notes/1750510) 
- [SAP destek notu #1752266](https://launchpad.support.sap.com/#/notes/1752266) 
- [SAP destek notu #2162183](https://launchpad.support.sap.com/#/notes/2162183) 
- [SAP destek notu #1588316](https://launchpad.support.sap.com/#/notes/158831) 

Diğer bilgiler 

- [SAP Adaptif Server Enterprise'da SAP Uygulamaları](https://community.sap.com/topics/applications-on-ase)
- [Sybase bilgi merkezi](http://infocenter.sybase.com/help/index.jsp) 

Aylık bülten SAP [destek notu #2381575](https://launchpad.support.sap.com/#/notes/2381575) aracılığıyla yayınlanır 

[Sybase ASE Her zaman 3 DR Düğüm Kurulum ile](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/installation-procedure-for-sybase-16-3-patch-level-3-always-on/ba-p/368199) 

## <a name="next-steps"></a>Sonraki adımlar
[Azure'daki MAKALE SAP iş yüklerini kontrol edin: planlama ve dağıtım denetim listesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)

