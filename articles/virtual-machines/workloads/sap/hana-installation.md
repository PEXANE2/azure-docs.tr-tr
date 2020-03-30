---
title: SAP HANA'yı Azure'da SAP HANA'ya yükleyin (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'yı Azure'da BIR SAP HANA'ya (Büyük Örnekler) yükleme.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/16/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca59305b22fcf1e81ef518612910731cb6edea5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617087"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Azure'da SAP HANA (Büyük Örnekler) nasıl yüklenir ve yapılandırılır?

Bu makaleyi okumadan önce, [HANA Büyük Örnekleri ortak terimler](hana-know-terms.md) ve [HANA Büyük Örnekleri SUs](hana-available-skus.md)aşina olsun.

SAP HANA'nın kurulumu sizin sorumluluğunuzdadır. Azure sanal ağlarınız ile HANA Büyük Örnek birimi(ler) arasındaki bağlantıyı kurduktan sonra Azure (Büyük Örnekler) sunucusuna yeni bir SAP HANA yüklemeye başlayabilirsiniz. 

> [!Note]
> SAP ilkesine göre, SAP HANA'nın kurulumu, Sertifikalı SAP Teknoloji Önlisans sınavını, SAP HANA Kurulum sertifika sınavını geçen veya SAP sertifikalı sistem entegratörü (SI) olan bir kişi tarafından gerçekleştirilmelidir.

HANA 2.0'ı yüklemeyi planlıyorken, işletim sisteminin yüklediğiniz SAP HANA sürümüyle desteklendiğinden emin olmak için [SAP destek notu #2235581 - SAP HANA: Desteklenen işletim sistemleri.](https://launchpad.support.sap.com/#/notes/2235581/E) HANA 2.0 için desteklenen işletim sistemi, HANA 1.0 için desteklenen işletim sistemi'nden daha kısıtlayıcıdır. Ayrıca, ilgilendiğiniz işletim sistemi sürümü bu yayınlanan [listede](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)belirli HLI birimi için desteklenen olarak listelenen olup olmadığını kontrol etmek gerekir. O birimin desteklenen işletim sistemi listesi ile tüm ayrıntıları almak için üniteyi tıklatın. 

HANA yüklemesini başlatmadan önce aşağıdakileri doğrulayın:
- [HLI birimi(ler)](#validate-the-hana-large-instance-units)
- [İşletim sistemi yapılandırması](#operating-system)
- [Ağ yapılandırması](#networking)
- [Depolama yapılandırması](#storage)


## <a name="validate-the-hana-large-instance-units"></a>HANA Büyük Örnek birimi(ler) doğrulayın

HANA Büyük Örnek birimini Microsoft'tan aldıktan sonra, aşağıdaki ayarları doğrulayın ve gerektiği gibi ayarlayın.

HANA Büyük Örneği'ni aldıktan ve örneklere erişim ve bağlantı kurduktan sonra **ilk adım,** Azure portalında örneğin doğru SUK'lar ve işletim sistemiyle gösterip gösterilmediğini kontrol etmektir. Denetimleri gerçekleştirmek için gerekli adımlar için [Azure portalı üzerinden Azure HANA Büyük Örnekler denetimini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal) okuyun.

HANA Büyük Örneği'ni aldıktan ve örneklere erişim ve bağlantı kurduktan sonra **ikinci adım,** örneğin işletim sistemi işletim sistemi sağlayıcısına kaydetmektir. Bu adım, SUSE Linux işletim sisteminizi Azure'da bir VM'de dağıtılan SUSE SMT örneğine kaydetmeyi içerir. 

HANA Büyük Örnek birimi bu SMT örneğine bağlanabilir. (Daha fazla bilgi için, [SUSE Linux için SMT sunucusu nasıl ayarlanıyor) bakın.](hana-setup-smt.md) Alternatif olarak, Red Hat OS'nizin bağlanmanız gereken Red Hat Abonelik Yöneticisi'ne kaydolması gerekir. Daha fazla bilgi için Azure'daki [SAP HANA (Büyük Örnekler) nedir?](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Bu adım, müşterinin sorumluluğunda olan işletim sistemi yama için gereklidir. SUSE için, [SMT yükleme](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html)hakkında bu sayfada SMT yükleme ve yapılandırma için belgeleri bulun.

**Üçüncü adım,** yeni düzeltme sistemi sürümü/sürümünün yeni düzeltmelerini ve düzeltmelerini kontrol etmektir. HANA Büyük Örneği'nin yama düzeyinin en son durumda olduğunu doğrulayın. En son düzeltme ekleri dahil olmayan durumlar olabilir. HANA Büyük Örnek birimini devraldıktan sonra, düzeltme emültlerinin uygulanması gerekip gerekmediğini kontrol etmek zorunludur.

**Dördüncü adım,** sap HANA'yı belirli işletim sistemi sürümüne/sürümüne yüklemek ve yapılandırmak için ilgili SAP notlarını kontrol etmektir. Tek tek yükleme senaryolarına bağlı olan SAP notlarında veya yapılandırmalarında değişen öneriler veya değişiklikler nedeniyle, Microsoft her zaman bir HANA Büyük Örnek birimini mükemmel bir şekilde yapılandıramayacaktır. 

Bu nedenle, bir müşteri olarak sap HANA ile ilgili SAP notlarını tam Linux sürümüiçin okumanız zorunludur. Ayrıca işletim sistemi sürümü/sürümünün yapılandırmalarını kontrol edin ve henüz yapmadıysanız yapılandırma ayarlarını uygulayın.

Özellikle, aşağıdaki parametreleri kontrol edin ve sonunda ayarlayın:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- net.core.wmem_default = 16777216
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

SLES12 SP1 ve RHEL 7.2 ile başlayarak, bu parametreler /etc/sysctl.d dizininde bir yapılandırma dosyasında ayarlanmalıdır. Örneğin, 91-NetApp-HANA.conf adında bir yapılandırma dosyası oluşturulmalıdır. Eski SLES ve RHEL sürümleri için bu parametreler in/etc/sysctl.conf olarak ayarlanmalıdır.

RHEL 6.3 ile başlayan tüm RHEL sürümleri için aklınızda bulunsun: 
- Sunrpc.tcp_slot_table_entries = 128 parametre in/etc/modprobe.d/sunrpc-local.conf ayarlanmalıdır. Dosya yoksa, önce girişi ekleyerek dosyayı oluşturmanız gerekir: 
    - seçenekler sunrpc tcp_max_slot_table_entries=128

**Beşinci adım,** HANA Büyük Örnek biriminizin sistem saatini kontrol etmektir. Örnekler bir sistem saat dilimi ile dağıtılır. Bu saat dilimi, HANA Büyük Örnek damgasının bulunduğu Azure bölgesinin konumunu temsil eder. Sahip olduğunuz örneklerin sistem saatini veya saat dilimini değiştirebilirsiniz. 

Kiracınıza daha fazla örnek sipariş ederseniz, yeni teslim edilen örneklerin saat dilimini uyarlamanız gerekir. Microsoft' un devir teslimden sonraki örneklerle ayarladığınız sistem saat dilimi hakkında hiçbir içgörüsü yoktur. Bu nedenle, yeni dağıtılan örnekler değiştirdiğiniz saat diliminde ayarlanmayabilir. Müşteri olarak, gerekirse teslim edilen örneğin saat dilimini uyarlamak sizin sorumluluğunuzdadır. 

**Altıncı adım** vb / hosts kontrol etmektir. Bıçaklar teslim edildikçe, farklı amaçlar için atanmış farklı IP adreslerine sahiptirler. Etc/hosts dosyasını kontrol edin. Birimler varolan bir kiracıya eklendiğinde, yeni dağıtılan sistemlerin vb/ana bilgisayarların daha önce teslim edilen sistemlerin IP adresleriyle doğru şekilde tutulmasını beklemeyin. Yeni dağıtılan bir örneğin kiracınızda daha önce dağıttığınız birimlerin adlarını etkileyip çözebilmesini sağlamak müşteri olarak sizin sorumluluğunuzdadır. 


## <a name="operating-system"></a>İşletim sistemi

Teslim edilen işletim sistemi görüntüsünün takas alanı, SAP destek notuna göre 2 GB olarak ayarlanır [#1999997 - SSS: SAP HANA bellek.](https://launchpad.support.sap.com/#/notes/1999997/E) Bir müşteri olarak, farklı bir ayar istiyorsanız, kendiniz ayarlamanız gerekir.

[SAP uygulamaları için SUSE Linux Enterprise Server 12 SP1,](https://www.suse.com/products/sles-for-sap/download/) SAP HANA için Azure'da (Büyük Örnekler) yüklenen Linux dağıtımıdır. Bu özel dağıtım SAP'ye özgü yetenekleri "kutunun dışında" sağlar (SLES'te SAP'yi etkin bir şekilde çalıştırmak için önceden ayarlanmış parametreler dahil).

SAP Topluluk Ağı'nda (SCN) SAP HANA'nın sles'te dağıtılmasıyla ilgili çeşitli yararlı kaynaklar için SUSE web sitesinde [ki](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) Kaynak [kitaplığı/teknik incelemelere](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) (yüksek kullanılabilirlik kurulumu, SAP işlemlerine özgü güvenlik sertleştirmesi ve daha fazlası dahil) bakın.

SUSE ile ilgili bağlantılarda ek ve kullanışlı SAP aşağıdaki gibidir:

- [SAP HANA Üzerinde SUSE Linux sitesi](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [SAP için en iyi uygulamalar: Enqueue çoğaltma - SAP NetWeaver SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113)
- [ClamSAP – SAP için SLES virüs koruması](https://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (SAP uygulamaları için SLES 12 dahil)

SLES 12'de SAP HANA'nın uygulanması için geçerli olan SAP destek notları aşağıda veda edecektir:

- [SAP destek notu #1944799 – SLES işletim sistemi kurulumu için SAP HANA yönergeleri](http://service.sap.com/sap/support/notes/1944799)
- [SAP destek notu #2205917 – SAP HANA DB, SAP uygulamaları için SLES 12 için işletim sistemi ayarlarını tavsiye etti](https://launchpad.support.sap.com/#/notes/2205917/E)
- [SAP destek notu #1984787 – SUSE Linux Enterprise Server 12: kurulum notları](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP destek notu #171356 - Linux'ta SAP yazılımı: Genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP destek notu #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070)

[SAP HANA için Red Hat Enterprise Linux](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) HANA Büyük Örnekleri SAP HANA çalıştırmak için başka bir tekliftir. RHEL 7.2 ve 7.3 sürümleri mevcuttur ve desteklenir. 

Red Hat ile ilgili bağlantılarda ek yararlı SAP şunlardır:
- [SAP HANA Red Hat Linux sitesinde](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

Aşağıda, Red Hat'te SAP HANA'nın uygulanması için geçerli olan SAP destek notları veremistir:

- [SAP destek notu #2009879 - Red Hat Enterprise Linux (RHEL) işletim sistemi için SAP HANA yönergeleri](https://launchpad.support.sap.com/#/notes/2009879/E)
- [SAP destek notu #2292690 - SAP HANA DB: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/2292690)
- [SAP destek notu #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070)
- [SAP destek notu #2228351 - Linux: SAP HANA Veritabanı SPS 11 revizyon 110 (veya daha yüksek) RHEL 6 veya SLES 11](https://launchpad.support.sap.com/#/notes/2228351)
- [SAP destek notu #2397039 - SSS: RHEL'de SAP](https://launchpad.support.sap.com/#/notes/2397039)
- [SAP destek notu #2002167 - Red Hat Enterprise Linux 7.x: Kurulum ve yükseltme](https://launchpad.support.sap.com/#/notes/2002167)

### <a name="time-synchronization"></a>Zaman eşitleme

SAP NetWeaver mimarisi üzerine inşa edilen SAP uygulamaları, SAP sistemini oluşturan çeşitli bileşenleriçin zaman farklılıklarına duyarlıdır. ZDATE\_LARGE\_TIME\_DIFF hata başlığı ile SAP ABAP kısa dökümleri muhtemelen tanıdık. Bunun nedeni, farklı sunucuların veya VM'lerin sistem zamanı birbirinden çok uzaklara sürüklendiğinde bu kısa dökümlerin görünmesidir.

Azure'daki SAP HANA (Büyük Örnekler) için, Azure'da yapılan zaman eşitleme, Büyük Örnek damgalarında bulunan işlem birimleri için geçerli değildir. Azure, bir sistemin zamanının düzgün eşitlenmesine izin verdiğinden, bu eşitleme yerel Azure VM'lerde SAP uygulamalarını çalıştırmak için geçerli değildir. 

Sonuç olarak, Azure VM'lerde çalışan SAP uygulama sunucuları ve HANA Büyük Örnekleri'nde çalışan SAP HANA veritabanı örnekleri tarafından kullanılabilecek ayrı bir zaman sunucusu ayarlamanız gerekir. Büyük Örnek damgalarında depolama altyapısı, NTP sunucularıyla zaman senkronize edilir.


## <a name="networking"></a>Ağ Oluşturma
Azure sanal ağlarınızı tasarlamak ve bu sanal ağları HANA Büyük Örnekleri'ne bağlamak için aşağıdaki belgelerde açıklandığı gibi önerileri izlediğinizi varsayıyoruz:

- [Azure'da SAP HANA (Büyük Örnek) genel bakışı ve mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Azure'da SAP HANA (Büyük Örnekler) altyapısı ve bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Tek birimlerin ağ hakkında söz değer bazı ayrıntılar vardır. Her HANA Büyük Örnek birimi, iki veya üç NIC bağlantı noktalarına atanan iki veya üç IP adresiyle birlikte gelir. HANA ölçeklendirme yapılandırmalarında ve HANA sistem çoğaltma senaryosunda üç IP adresi kullanılır. Birimin NIC'sine atanan IP adreslerinden biri, [Azure'da SAP HANA (Büyük Örnekler) genel bakışı nda ve mimarisinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)açıklanan sunucu IP havuzunun dışındadır.

Mimariniz için Ethernet ayrıntıları hakkında daha fazla bilgi için [HLI desteklenen senaryolara](hana-supported-scenario.md)bakın.

## <a name="storage"></a>Depolama

Azure'daki SAP HANA'nın depolama düzeni (Büyük Örnekler) SAP `service management` TARAFıNDAN Azure'da SAP TARAFıNDAN ÖNERILEN yönergeler aracılığıyla yapılandırılır. Bu yönergeler [SAP HANA depolama gereksinimleri](https://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemede belgelenmiştir. 

Farklı HANA Büyük Örnekleri SKUs ile farklı birimlerin kaba boyutları [SAP HANA (Büyük Örnekler) genel bakış ve Azure mimarisi](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)belgelenmiştir.

Depolama birimlerinin adlandırma kuralları aşağıdaki tabloda listelenmiştir:

| Depolama kullanımı | Montaj adı | Birim adı | 
| --- | --- | ---|
| HANA verileri | /hana/veri/SID/mnt0000\<m> | Depolama IP:/hana_data_SID_mnt00001_tenant_vol |
| HANA günlüğü | /hana/log/SID/mnt0000\<m> | Depolama IP:/hana_log_SID_mnt00001_tenant_vol |
| HANA günlük yedekleme | /hana/log/yedekler | Depolama IP:/hana_log_backups_SID_mnt00001_tenant_vol |
| HANA paylaştı | /hana/paylaşılan/SID | Depolama IP:/hana_shared_SID_mnt00001_tenant_vol/paylaşılan |
| usr/sap | /usr/sap/SID | Depolama IP:/hana_shared_SID_mnt00001_tenant_vol/usr_sap |

*SID,* HANA örnek Sistem Kimliği'dir. 

*Kiracı,* bir kiracıyı dağıtırken işlemlerin dahili bir numaralandırmasıdır.

HANA usr/sap aynı hacmi paylaşır. Montaj noktalarının terminolojisi HANA örneklerinin sistem kimliğini ve montaj numarasını içerir. Ölçeklendirme dağıtımlarında mnt00001 gibi yalnızca bir montaj vardır. Ölçeklendirme li dağıtımlarda ise, işçi ve ana düğümlere sahip olduğunuz kadar montaj görürsünüz. 

Ölçeklendirme ortamları için, veriler, günlük ve günlük yedekleme birimleri ölçeklendirme yapılandırmasındaki her düğüme paylaşılır ve eklenir. Birden çok SAP örneği olan yapılandırmalar için farklı bir birim kümesi oluşturulur ve HANA Büyük Örnek birimine eklenir. Senaryonuz için depolama düzeni ayrıntıları için [HLI destekli senaryolara](hana-supported-scenario.md)bakın.

Bir HANA Büyük Örnek birimine baktığınızda, birimlerin HANA/veri için cömert disk hacmiyle geldiğini ve hana/log/backup birimi olduğunu fark esiniz. HANA/veri'yi bu kadar büyük yapmamızın nedeni, müşteri olarak size sunduğumuz depolama anlık görüntülerinin aynı disk hacmini kullanmasıdır. Ne kadar çok depolama anlık görüntüsü gerçekleştirirseniz, atanan depolama birimlerinizdeki anlık görüntüler o kadar çok yer tüketilir. 

HANA/log/yedekleme hacmi veritabanı yedeklemeleri için birim olması gerekmez. HANA hareket günlüğü yedeklemeleri için yedek birim olarak kullanılacak boyutlandırılır. Daha fazla bilgi için, [Azure'da SAP HANA (Büyük Örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bilgisine bakın. 

Sağlanan depolamaya ek olarak, 1-TB artışlarla ek depolama kapasitesi satın alabilirsiniz. Bu ek depolama hana büyük örneğine yeni birimler olarak eklenebilir.

Azure'da `service management`SAP HANA ile devam sırasında müşteri, sidadm kullanıcısı ve sapsys grubu için bir kullanıcı kimliği (UID) ve grup kimliği (GID) belirtir (örneğin: 1000.500). SAP HANA sisteminin yüklenmesi sırasında bu değerleri kullanmanız gerekir. Bir birimde birden çok HANA örneği dağıtmak istediğinizden, birden çok birim kümesi (her örnek için bir küme) alırsınız. Sonuç olarak, dağıtım zamanında şunları tanımlamanız gerekir:

- Farklı HANA örneklerinin SID 'si (sidadm ondan türetilir).
- Farklı HANA örneklerinin bellek boyutları. Örnek başına bellek boyutu, her bir birim kümesindeki birimlerin boyutunu tanımlar.

Depolama sağlayıcısının önerilerine bağlı olarak, aşağıdaki montaj seçenekleri tüm monte edilmiş birimler için yapılandırılır (önyükleme LUN hariç):

- nfs rw, vers=4, hard, timeo=600, rsize=1048576, wsize=1048576, intr, noatime, kilit 0 0

Bu montaj noktaları aşağıdaki grafiklerde gösterildiği gibi /etc/fstab olarak yapılandırılır:

![HANA Büyük Örnek biriminde monte edilmiş hacimlerin fstab](./media/hana-installation/image1_fstab.PNG)

Bir S72m HANA Büyük Örnek biriminde df -h komutunun çıktısı aşağıdaki gibi görünür:

![HANA Büyük Örnek biriminde monte edilmiş hacimlerin fstab](./media/hana-installation/image2_df_output.PNG)


Büyük Örnek damgalarında bulunan depolama denetleyicisi ve düğümleri NTP sunucularına eşitlenir. Azure (Büyük Örnekler) birimlerindeki SAP HANA'yı ve Azure VM'lerini bir NTP sunucusuyla eşitlediğinizde, Azure veya Büyük Örnek damgalarında altyapı ile bilgi işlem birimleri arasında önemli bir zaman kayması olmamalıdır.

SAP HANA'yı altında kullanılan depolama ya da depolamaya optimize etmek için aşağıdaki SAP HANA yapılandırma parametrelerini ayarlayın:

- max_parallel_io_requests 128
- async_read_submit üzerinde
- async_write_submit_active üzerinde
- async_write_submit_blocks tüm
 
SAP HANA 1.0 sürümlerinde SPS12'ye kadar olan sürümler için, sap not #2267798 - [SAP HANA veritabanının yapılandırmasında](https://launchpad.support.sap.com/#/notes/2267798)açıklandığı gibi, sap HANA veritabanının yüklenmesi sırasında bu parametreler ayarlanabilir.

Ayrıca hdbparam çerçevesini kullanarak SAP HANA veritabanı yüklemesi sonra parametreleri yapılandırabilirsiniz. 

HANA Büyük Örnekleri'nde kullanılan depolama alanının dosya boyutu sınırlaması vardır. Boyut sınırlaması dosya başına [16 TB'dir.](https://docs.netapp.com/ontap-9/index.jsp?topic=%2Fcom.netapp.doc.dot-cm-vsmg%2FGUID-AA1419CF-50AB-41FF-A73C-C401741C847C.html) EXT3 dosya sistemlerindeki dosya boyutu sınırlamalarının aksine, HANA, HANA Büyük Örnekler depolama sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağısı yoktur. Sonuç olarak HANA, 16TB dosya boyutu sınırına ulaşıldığında otomatik olarak yeni bir veri dosyası oluşturmaz. HANA dosyayı 16 TB'nin ötesine büyütmeye çalışırken, HANA hataları bildirir ve dizin sunucusu sonunda çöker.

> [!IMPORTANT]
> HANA Büyük Örnek depolamanın 16 TB dosya boyutu sınırının ötesinde veri dosyalarını büyütmeye çalışan HANA'yı önlemek için SAP HANA global.ini yapılandırma dosyasında aşağıdaki parametreleri ayarlamanız gerekir
> 
> - datavolume_striping=doğru
> - datavolume_striping_size_gb = 15000
> - Ayrıca bakınız SAP not [#2400005](https://launchpad.support.sap.com/#/notes/2400005)
> - SAP [not](https://launchpad.support.sap.com/#/notes/2631285) #2631285


SAP HANA 2.0 ile hdbparam çerçevesi amortismana hazır hale gelmiştir. Sonuç olarak, parametreler SQL komutları kullanılarak ayarlanmalıdır. Daha fazla bilgi için, [SAP not #2399079 bakınız: HANA 2 hdbparam ortadan kaldırılması](https://launchpad.support.sap.com/#/notes/2399079).

Mimarinizin depolama düzeni hakkında daha fazla bilgi edinmek için [HLI destekli senaryolara](hana-supported-scenario.md) bakın.


**Sonraki adımlar**

- [HLI'daki HANA Kurulumuna](hana-example-installation.md) Bakın










































 







 




