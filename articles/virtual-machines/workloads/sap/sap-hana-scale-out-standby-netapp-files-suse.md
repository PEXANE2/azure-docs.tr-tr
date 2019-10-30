---
title: SUSE Linux Enterprise Server Azure NetApp Files ile Azure VM 'lerinde bekleme düğümüyle genişleme SAP HANA | Microsoft Docs
description: SAP NetWeaver için SUSE Linux Enterprise Server yüksek kullanılabilirlik Kılavuzu, SAP uygulamaları için Azure NetApp Files
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/30/2019
ms.author: radeltch
ms.openlocfilehash: 76369c1a4beb792de03cf0ccae5c86825812f103
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934170"
---
# <a name="sap-hana-scale-out-with-standby-node-on-azure-vms-with-azure-netapp-files-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server Azure NetApp Files ile Azure VM 'lerinde bekleme düğümüyle genişleme SAP HANA 

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all 
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]: https://launchpad.support.sap.com/#/notes/2205917
[1944799]: https://launchpad.support.sap.com/#/notes/1944799
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1984787]: https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736
[1900823]: https://launchpad.support.sap.com/#/notes/1900823

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md


Bu makalede, paylaşılan depolama birimleri için [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/) olan Azure sanal makinelerinde bekleme ile yüksek oranda kullanılabilir Hana sisteminin nasıl dağıtılacağı açıklanır.  
Örnek yapılandırmalarda, yükleme komutlarında ve bu durumda, HANA örneği **03** ' dır ve Hana sistem kimliği **HN1**' dir. Örnekler, HANA 2,0 SP4 ve SUSE Linux Enterprise Server SAP 12 SP4 tabanlıdır. 

Önce aşağıdaki SAP notlarını ve kağıtları okuyun:

* [Azure NetApp Files belgeleri][anf-azure-doc] 
* SAP Note [1928533], şunları içerir:  
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı birleşimleri
  * Microsoft Azure 'de Windows ve Linux için gereken SAP Kernel sürümü
* SAP Note [2015553] , Azure 'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP Note [2205917] , SAP uygulamaları için SUSE Linux Enterprise Server önerilen işletim sistemi ayarlarına sahiptir
* SAP Note [1944799] , SAP uygulamaları IÇIN SUSE Linux Enterprise Server SAP yönergelerine sahiptir
* SAP Note [2178632] , Azure 'da SAP için raporlanan tüm izleme ölçümleriyle ilgili ayrıntılı bilgiler içerir.
* SAP Note [2191498] , Azure 'da Linux IÇIN gereken SAP konak Aracısı sürümüne sahiptir.
* SAP Note [2243692] , Azure 'da LINUX üzerinde SAP lisanslama hakkında bilgi içerir.
* SAP Note [1984787] , SUSE Linux Enterprise Server 12 hakkında genel bilgiler içerir.
* SAP Note [1999351] , SAP Için Azure Gelişmiş izleme uzantısı için ek sorun giderme bilgilerine sahiptir.
* SAP Note [1900823] SAP HANA depolama gereksinimleriyle ilgili bilgiler içerir
* [SAP COMMUNITY WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) 'nin Linux için gereklı tüm sap notları vardır.
* [Linux 'ta SAP için Azure sanal makineleri planlama ve uygulama][planning-guide]
* [Linux 'ta SAP için Azure sanal makineleri dağıtımı][deployment-guide]
* [Linux üzerinde SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* [SUSE SAP ha En Iyi Yöntem Kılavuzu][suse-ha-guide] Kılavuzlar, NetWeaver HA ayarlamak için gerekli tüm bilgileri ve şirket içi SAP HANA sistem çoğaltmasını içerir. Bu kılavuzlarınızı genel bir taban çizgisi olarak kullanın. Çok daha ayrıntılı bilgi sağlar.
* [SUSE yüksek kullanılabilirlik uzantısı 12 SP3 sürüm notları][suse-ha-12sp3-relnotes]
* [Microsoft Azure Azure NetApp Files kullanarak NetApp SAP uygulamaları][anf-sap-applications-azure]
* [NFS Ile NetApp sistemlerinde SAP HANA](https://www.netapp.com/us/media/tr-4435.pdf). Yapılandırma Kılavuzu, Azure NetApp Files tarafından sunulan NFS kullanarak SAP HANA ayarlama hakkında bilgiler içerir.


## <a name="overview"></a>Genel Bakış

HANA yüksek kullanılabilirlik elde etmek için metotlardan biri, ana bilgisayar otomatik yük devreder. Ana bilgisayar otomatik yük devretme yapılandırması için, HANA sistemine bir veya daha fazla sanal makine eklenir ve bekleme düğümleri olacak şekilde yapılandırılır. Etkin düğüm başarısız olduğunda, bir bekleme düğümü otomatik olarak alır. Azure sanal makineler ile sunulan, [Azure NetApp Files üzerinde NFS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/)tarafından gerçekleştirilen yapılandırmada.  

Bekleme düğümünün tüm veritabanı birimlerine erişimi olması gerekir. HANA birimlerinin NFSv4 birimleri olarak bağlanması gerekir. NFSv4 protokolündeki geliştirilmiş dosya Kiralama tabanlı kilitleme mekanizması `I/O` bir sınırlama için kullanılır. 

> [!IMPORTANT]
> Desteklenen yapılandırmaya sahip olması için, HANA verilerinin ve günlük birimlerinin NFSv 4.1 birimleri olarak dağıtılması ve bunları NFSv 4.1 protokolünü kullanarak bağlaması zorunludur. NFSv3 ile HANA konak otomatik yük devretme yapılandırması, bekleme düğümüyle desteklenmez.

![SAP NetWeaver yüksek kullanılabilirliğe genel bakış](./media/high-availability-guide-suse-anf/sap-hana-scale-out-standby-netapp-files-suse.png)

SAP HANA ağ önerilerini takip eden bir Azure sanal ağı içinde üç alt ağ oluşturulmuştur: depolama sistemiyle iletişim için, dahili HANA düğümler arası iletişim ve istemci iletişimi için. Azure NetApp birimleri, [Azure NetApp Files atanan](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet)ayrı alt ağdadır.  

Bu örnek yapılandırma için alt ağlar şunlardır:  

  - `storage` 10.23.2.0/24  
  - `hana` 10.23.3.0/24  
  - `client` 10.23.0.0/24  
  - `anf` 10.23.1.0/26  

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Azure NetApp Files altyapısını ayarlama 

Azure NetApp dosyaları altyapısına yönelik kuruluma devam etmeden önce [Azure NetApp Files belgelerini inceleyin][anf-azure-doc]. Azure NetApp dosyaları çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/?products=netapp)kullanılabilir. Seçtiğiniz Azure bölgesinin Azure NetApp Files sunmadığını denetleyin.  

Aşağıdaki bağlantıda Azure bölgesine göre Azure NetApp Files kullanılabilirliği gösterilmektedir: [Azure bölgesine göre Azure NetApp Files kullanılabilirliği][anf-avail-matrix].  
Azure NetApp Files dağıtılmadan önce, [Azure NetApp dosyaları Için kaydolmayı][anf-register]izleyerek Azure NetApp Files ekleme isteği yapın. 

### <a name="deploy-azure-netapp-files-resources"></a>Azure NetApp Files kaynaklarını dağıtma  

Aşağıdaki adımlarda, [Azure sanal ağını](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)zaten dağıtmış olduğunuz varsayılmaktadır. Azure NetApp Files kaynaklarının bağlanmasının gerektiği Azure NetApp Files kaynakları ve VM 'Ler aynı Azure sanal ağında veya eşlenmiş Azure sanal ağlarında dağıtılmalıdır.  

1. Henüz yapmadıysanız, [Azure NetApp Files ekleme](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register)isteyin.  

2. [Newtapp hesabını oluşturma yönergelerini](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account)izleyerek, seçili Azure bölgesinde NetApp hesabını oluşturun.  

3. [Azure NetApp Files kapasite havuzunu ayarlama yönergelerini](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool)izleyerek Azure NetApp Files kapasite havuzunu ayarlayın.  
Bu makalede sunulan HANA mimarisi, tek Azure NetApp Files kapasite havuzunu, Ultra hizmet düzeyini kullanır. Azure 'da HANA iş yükleri için Ultra veya Premium [hizmet düzeyi](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) Azure NetApp Files önerilir.  

4. [Azure NetApp Files için bir alt ağ temsilcisine yönelik yönergeler](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet)bölümünde açıklandığı gibi Azure NetApp dosyalarına bir alt ağ atayın.  

5. [Azure NetApp Files için bir birim oluşturma yönergelerini](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)izleyerek Azure NetApp Files birimleri dağıtın.  Birimleri dağıttığınızda **Nfsv 4.1** sürümünü seçtiğinizden emin olun. Şu anda NFSv 4.1 'e erişim ek beyaz listeleme gerektirir. Birimleri belirlenen Azure NetApp Files [alt ağına](https://docs.microsoft.com/rest/api/virtualnetwork/subnets)dağıtın. Azure NetApp Files kaynaklarının ve Azure VM 'lerinin aynı Azure sanal ağında veya eşlenmiş Azure sanal ağlarında olması gerektiğini unutmayın. Örneğin, <b>HN1</b>-Data-Mnt00001, <b>HN1</b>-log-mnt00001, vb. birim adları ve NFS://10.23.1.5/<b>HN1</b>-Data-mnt00001, NFS://10.23.1.4/<b>HN1</b>-log-mnt00001, vb. Azure NetApp Files birimlerinin dosya yollarıdır.  

   1. Volume <b>HN1</b>-Data-mnt00001 (NFS://10.23.1.5/<b>HN1</b>-Data-mnt00001)
   2. Volume <b>HN1</b>-Data-mnt00002 (NFS://10.23.1.6/<b>HN1</b>-Data-mnt00002)
   3. Volume <b>HN1</b>-log-mnt00001 (NFS://10.23.1.4/<b>HN1</b>-log-mnt00001)
   4. Volume <b>HN1</b>-log-mnt00002 (NFS://10.23.1.6/<b>HN1</b>-log-mnt00002)
   5. Volume <b>HN1</b>-shared (NFS://10.23.1.4/<b>HN1</b>-Shared)
   
   Bu örnekte, her bir HANA veri ve günlük birimi için ayrı Azure NetApp Files kullandık. Daha küçük veya üretken olmayan sistemlerde daha uygun maliyetli yapılandırma için, tüm veri takmaları ve tüm günlükleri tek bir birimde takar.  

### <a name="important-considerations"></a>Önemli konular

SUSE yüksek kullanılabilirlik mimarisinde SAP NetWeaver için Azure NetApp Files düşünürken, aşağıdaki önemli noktalara dikkat edin:

- En düşük kapasite havuzu 4 TiB 'dir.  
- En küçük birim boyutu 100 GiB 'dir
- Azure NetApp Files ve Azure NetApp Files birimlerinin takılabileceği tüm sanal makineler aynı bölgedeki aynı Azure sanal ağında veya eşlenmiş [sanal ağlarda](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) olmalıdır.  
- Seçilen sanal ağ, Azure NetApp Files atanmış bir alt ağa sahip olmalıdır.
- Azure NetApp biriminin performansı, [Azure NetApp Files Için hizmet düzeyinde](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)belgelendiği gibi birim kotasının ve hizmet düzeyinin bir işlevidir. HANA Azure NetApp birimlerini boyutlandırdığınızda, elde edilen aktarım hızının HANA sistem gereksinimlerini karşıladığından emin olun.  
- Azure NetApp Files, [dışarı aktarma ilkesi](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy)sunar: izin verilen istemcileri, erişim türünü (okuma & yazma, salt okuma, vb.) denetleyebilirsiniz. 
- Azure NetApp Files Özellik henüz bölge farkında değildir. Şu anda Azure NetApp Files özelliği bir Azure bölgesindeki tüm kullanılabilirlik bölgelerinde dağıtılmaz. Bazı Azure bölgelerindeki olası gecikme etkilerine yönelik etkileri göz önünde bulundurun.  
- Düşük gecikme süresi boyunca sanal makinelerin Azure NetApp depolama alanına yakın bir şekilde dağıtılmasını sağlamak önemlidir. SAP HANA iş yükleri için düşük gecikme süresi kritik öneme sahiptir. Sanal makinelerin ve Azure NetApp Files birimlerinin yakın bir yerde dağıtıldığından emin olmak için Microsoft temsilcinizle birlikte çalışın.  
- <b>SID</b>adm IÇIN Kullanıcı kimliği ve sanal makinelerdeki `sapsys` grup kimliği, Azure NetApp Files yapılandırmasıyla aynı olmalıdır. 

> [!IMPORTANT]
> SAP HANA iş yükleri için düşük gecikme süresi kritik öneme sahiptir. Sanal makinelerin ve Azure NetApp Files birimlerinin yakın bir yerde dağıtıldığından emin olmak için Microsoft temsilcinizle birlikte çalışın.  

> [!IMPORTANT]
> <b>SID</b>adm Kullanıcı kimliği ve sanal makine Ile Azure NetApp yapılandırması arasındaki `sapsys` IÇIN grup kimliği arasında bir uyumsuzluk varsa, sanal makinelere takılan Azure NetApp birimlerindeki dosyaların izinleri `nobody` olarak görüntülenecektir. Azure NetApp Files için [Yeni bir sistem](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) oluştururken, <b>SID</b>adm için doğru Kullanıcı kimliğini ve `sapsys` için grup kimliğini belirttiğinizden emin olun.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Azure NetApp Files HANA veritabanı için boyutlandırma

Azure NetApp biriminin performansı, [Azure NetApp Files Için hizmet düzeyi](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)bölümünde belgelendiği gibi, birim boyutu ve hizmet düzeyi işlevindedir. 

Azure 'da SAP altyapısını tasarlarken, en düşük işleme özelliklerine çeviren SAP tarafından bazı minimum depolama gereksinimlerinden haberdar olmanız gerekir:

- 1 MB g/ç boyutlarıyla 250 MB/sn 'nin/Hana/log tarihinde okuma/yazma özelliğini etkinleştirin  
- 16 MB ve 64 MB g/ç boyutları için/Hana/Data için en az 400 MB/sn okuma etkinliğini etkinleştirin  
- 16 MB ve 64 MB g/ç boyutları ile/Hana/Data için en az 250 MB/sn yazma etkinliğini etkinleştirin  

Birim kotasının 1 TiB başına [Azure NetApp Files verimlilik limitleri](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) şunlardır:
- Premium depolama katmanı-64 MIB/s  
- Ultra depolama katmanı-128 MIB/sn  

Veri ve günlük SAP minimum aktarım hızı gereksinimlerini karşılamak ve `/hana/shared` yönelik yönergelere göre, önerilen boyutlar şöyle görünür:

| Birim | Boyut<br /> Premium depolama katmanı | Boyut<br /> Ultra depolama katmanı | Desteklenen NFS Protokolü |
| --- | --- | --- |
| /Hana/log/ | 4 TiB | 2 TiB | v 4.1 |
| /Hana/Data | 6,3 TiB | 3,2 TiB | v 4.1 |
| /Hana/Shared | 4 çalışan düğümü başına en fazla (512 GB, 1xRAM) | 4 çalışan düğümü başına en fazla (512 GB, 1xRAM) | V3 veya v 4.1 |

Bu makalede sunulan düzenin SAP HANA yapılandırması, Azure NetApp Files Ultra Storage katmanını kullanarak şöyle görünür:

| Birim | Boyut<br /> Ultra depolama katmanı | Desteklenen NFS Protokolü |
| --- | --- |
| /Hana/log/mnt00001 | 2 TiB | v 4.1 |
| /Hana/log/mnt00002 | 2 TiB | v 4.1 |
| /Hana/Data/mnt00001 | 3,2 TiB | v 4.1 |
| /Hana/Data/mnt00002 | 3,2 TiB | v 4.1 |
| /Hana/Shared | 2 TiB | V3 veya v 4.1 |

> [!NOTE]
> Burada belirtilen Azure NetApp Files boyutlandırma önerileri, altyapı sağlayıcıları doğrultusunda, SAP 'nin ifade edilen en düşük gereksinimlerini karşılayacak şekilde hedefleniyor. Gerçek müşteri dağıtımları ve iş yükü senaryolarında bu yeterli olmayabilir. Bu önerileri bir başlangıç noktası olarak kullanın ve belirli iş yükünüzün gereksinimlerine göre uyarlayın.  

> [!TIP]
> Birimleri `unmount`, sanal makineleri durdurmanız veya SAP HANA durdurmanız gerekmeden Azure NetApp Files birimleri dinamik olarak yeniden boyutlandırabilir. Bu, uygulamanızı hem beklenen hem de öngörülemeyen üretilen iş taleplerini karşılamak için esneklik sağlar.

## <a name="deploy-linux-virtual-machines-via-azure-portal"></a>Linux sanal makinelerini Azure portal aracılığıyla dağıtma

Öncelikle Azure NetApp Files birimleri oluşturmanız gerekir. Azure sanal ağı 'nda [Azure sanal ağ alt ağlarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) [oluşturun.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) Sanal makineleri dağıtın. Ek ağ arabirimlerini oluşturun ve ağ arabirimlerini ilgili VM 'lere ekleyin. Her sanal makinenin üç Azure sanal ağ alt ağına (`storage`, `hana` ve `client`) karşılık gelen üç ağ arabirimi vardır.  Bkz. [Azure 'da birden çok ağ arabirimi kartı Ile Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics).  

> [!IMPORTANT]
> SAP HANA iş yükleri için düşük gecikme süresi kritik öneme sahiptir. Düşük gecikme süresine ulaşmak için sanal makinelerin ve Azure NetApp Files birimlerinin yakın bir yerde dağıtıldığından emin olmak için Microsoft temsilcinizle birlikte çalışın. SAP HANA Azure NetApp Files kullanan [yeni SAP HANA sistemi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u)oluştururken gerekli bilgileri gönder.  
> 
Sonraki adımlarda, kaynak grubu, Azure sanal ağı ve üç Azure sanal ağ alt ağı oluşturmuş olduğunuz varsayılır: `storage`, `hana` ve `client`.  Sanal makineleri dağıttığınızda depolama alt ağını, depolama ağı arabiriminin sanal makinelerde birincil arabirim olması için seçin. Bu mümkün değilse, depolama alt ağı ağ geçidi aracılığıyla Azure NetApp Temsilcili alt ağına açık bir rota yapılandırılmalıdır. 

> [!IMPORTANT]
> Seçtiğiniz işletim sisteminin, kullanmakta olduğunuz belirli VM türlerinde SAP HANA için SAP sertifikalı olduğundan emin olun. Bu kişiler için SAP HANA sertifikalı VM türlerinin ve işletim sistemi sürümlerinin listesi, [SAP HANA sertifikalı IaaS platformlarında](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)aranabilir. Belirli bir sanal makine türü için desteklenen SAP HANA işletim sistemi sürümlerinin tüm listesini almak üzere listelenen VM türünün ayrıntılarına tıkladığınızdan emin olun.  

1. SAP HANA için bir kullanılabilirlik kümesi oluşturun. En yüksek güncelleştirme etki alanını ayarladığınızdan emin olun.  

2. Üç sanal makine oluşturma (**hanadb1**, **hanadb2**, **hanadb3**)  
   - Azure galerisinde SAP HANA için desteklenen bir SLES4SAP görüntüsü kullanın. Bu örnekte SLES4SAP 12 SP4 görüntüsünü kullandık.  
   - Daha önce SAP HANA için oluşturulan kullanılabilirlik kümesini seçin.  
   - Depolama Azure sanal ağ alt ağını seçin. [Hızlandırılmış ağ](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)' ı seçin.  
Sanal makineleri dağıtırken, ağ arabirimi adı otomatik olarak oluşturulur. Depolama Azure sanal ağ alt ağına **hanadb1-Storage**, **hanadb2-Storage**ve **hanadb3-Storage**olarak eklenen ağ arabirimlerine başvuracağız. 
3. `hana` sanal ağ alt ağı için her bir sanal makine için bir tane olmak üzere üç ağ arabirimi oluşturun. Bu örnekte: **hanadb1-Hana**, **hanadb2-Hana**ve **hanadb3-Hana**.  
4. **İstemci** sanal ağ alt ağı için her bir sanal makine için bir tane olmak üzere üç ağ arabirimi oluşturun. Bu örnekte: **hanadb1-Client**, **hanadb2-Client**ve **hanadb3-Client**.  
5. Yeni oluşturulan sanal ağ arabirimlerini karşılık gelen sanal makinelere Ekle  

    1. [Azure Portal](https://portal.azure.com/#home)sanal makineye gidin.  
    2. Sol taraftaki Gezinti bölmesinde sanal makineler ' i seçin. Sanal makine adını filtreleyin, örneğin **hanadb1**. Sanal makineye tıklayın.  
    3. Genel bakışta, sanal makineyi serbest bırakmak için Durdur ' u seçin.  
    4. Ağ Iletişimi, ağ arabirimi Ekle ' yi seçin. **`hana`** ve **istemci** alt ağları için önceden oluşturulmuş ağ arabirimleri olan "ağ arabirimini Ekle" altındaki açılan listeden seçim yapın.  Kaydedin. 
    5. Kalan sanal makineler için tekrarlayın. Örneğimizde, **hanadb2** ve **hanadb3**.
    6. Sanal makineleri şimdilik durdurulmuş durumda bırakın. Daha sonra, yeni eklenen tüm ağ arabirimleri için [hızlandırılmış ağı](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) etkinleştireceğiz.  

6. **`hana`** ve **`client`** alt ağları için ek ağ arabirimleri için hızlandırılmış ağı etkinleştirin.  

    1. [Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) 'i [Azure Portal](https://portal.azure.com/#home) açın  
    2. **`hana`** ve **`client`** alt ağlarına bağlı ek ağ arabirimleri için hızlandırılmış ağı etkinleştirmek üzere aşağıdaki komutları yürütün.  

    <pre><code>
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-hana</b> --accelerated-networking true
    
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-client</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-client</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-client</b> --accelerated-networking true
    </code></pre>
7. Sanal makineleri Başlat  

    1. Sol taraftaki Gezinti bölmesinde sanal makineler ' i seçin. Sanal makine adını filtreleyin, örneğin **hanadb1**. Sanal makineye tıklayın.  
    2. Genel bakışta, Başlat ' ı seçin.  

## <a name="operating-system-configuration-and-preparation"></a>İşletim sistemi yapılandırması ve hazırlığı

Şu öğeler **[A]** ön eki olan tüm düğümlere uygulanabilir, **[1]** -yalnızca düğüm 2 veya **[3]** için **geçerlidir-yalnızca** düğüm 3 için geçerlidir.

1. **[A]** sanal makinelerdeki ana bilgisayar dosyalarını koruyun. Tüm alt ağların girdilerini dahil edin. Bu örnek için `/etc/hosts` aşağıdaki girişler eklenmiştir.  
    <pre><code>
    # Storage
    10.23.2.4   hanadb1
    10.23.2.5   hanadb2
    10.23.2.6   hanadb3
    # Client
    10.23.0.5   hanadb1-client
    10.23.0.6   hanadb2-client
    10.23.0.7   hanadb3-client
    # Hana
    10.23.3.4   hanadb1-hana
    10.23.3.5   hanadb2-hana
    10.23.3.6   hanadb3-hana
    </code></pre>

2. **[A]** istenmeyen konak adı değişikliklerini önlemek için DHCP ve bulut yapılandırması ayarlarını değiştirin.  
    <pre><code>
    vi /etc/sysconfig/network/dhcp
    #Change the following DHCP setting to "no"
    DHCLIENT_SET_HOSTNAME="no"
    vi /etc/sysconfig/network/ifcfg-eth0
    # Edit ifcfg-eth0 
    #Change CLOUD_NETCONFIG_MANAGE='yes' to "no"
    CLOUD_NETCONFIG_MANAGE='no'
    </code></pre>

3. **[A]** [NFS yapılandırma kılavuzu ile NetApp aff sistemlerinde SAP HANA](https://www.netapp.com/us/media/tr-4435.pdf)açıklandığı şekilde, NFS ile NetApp sistemlerinde SAP HANA çalıştırmak için işletim sistemini hazırlayın. NetApp yapılandırma ayarları için yapılandırma dosyası oluştur: `/etc/sysctl.d/netapp-hana.conf`.  

    <pre><code>
    vi /etc/sysctl.d/netapp-hana.conf
    # Add the following entries in the configuration file
    net.core.rmem_max = 16777216
    net.core.wmem_max = 16777216
    net.core.rmem_default = 16777216
    net.core.wmem_default = 16777216
    net.core.optmem_max = 16777216
    net.ipv4.tcp_rmem = 65536 16777216 16777216
    net.ipv4.tcp_wmem = 65536 16777216 16777216
    net.core.netdev_max_backlog = 300000
    net.ipv4.tcp_slow_start_after_idle=0
    net.ipv4.tcp_no_metrics_save = 1
    net.ipv4.tcp_moderate_rcvbuf = 1
    net.ipv4.tcp_window_scaling = 1
    net.ipv4.tcp_timestamps = 1
    net.ipv4.tcp_sack = 1
    </code></pre>

4. **[A]** Azure yapılandırma ayarları için Microsoft ile yapılandırma dosyası oluşturma: `/etc/sysctl.d/ms-az.conf`.  

    <pre><code>
    vi /etc/sysctl.d/ms-az.conf
    # Add the following entries in the configuration file
    ipv6.conf.all.disable_ipv6 = 1
    net.ipv4.tcp_max_syn_backlog = 16348
    net.ipv4.ip_local_port_range = 40000 65300
    net.ipv4.conf.all.rp_filter = 0
    sunrpc.tcp_slot_table_entries = 128
    vm.swappiness=10
    </code></pre>

4. **[A]** [NFS yapılandırma kılavuzu ile NetApp aff sistemlerinde SAP HANA](https://www.netapp.com/us/media/tr-4435.pdf)önerildiği gibi sunrpc ayarlarını yapın.  
    <pre><code>
    vi /etc/modprobe.d/sunrpc.conf
    # Insert the following line
    options sunrpc tcp_max_slot_table_entries=128
    </code></pre>

## <a name="mount-the-azure-netapp-files-volumes"></a>Azure NetApp Files birimleri bağlama

1. **[A]** Hana veritabanı birimleri için bağlama noktaları oluşturun.  
    <pre><code>
    mkdir -p /hana/data/<b>HN1</b>/mnt00001
    mkdir -p /hana/data/<b>HN1</b>/mnt00002
    mkdir -p /hana/log/<b>HN1</b>/mnt00001
    mkdir -p /hana/log/<b>HN1</b>/mnt00002
    mkdir -p /hana/shared
    mkdir -p /usr/sap/<b>HN1</b>
    </code></pre>

2. **[1]** **HN1**-Shared üzerinde `/usr/sap` için düğüme özgü dizinler oluşturun.  

    <pre><code>
    # Create a temporary directory to mount  <b>HN1</b>-shared
    mkdir /mnt/tmp
    mount <b>10.23.1.4</b>:/<b>HN1</b>-shared /mnt/tmp
    cd /mnt/tmp
    mkdir shared usr-sap-<b>hanadb1</b> usr-sap-<b>hanadb2</b> usr-sap-<b>hanadb3</b>
    # unmount /hana/shared
    cd
    umount /mnt/tmp
    </code></pre>

3. **[A]** NFS etki alanı ayarını doğrulayın. Etki alanının **`localdomain`** olarak yapılandırıldığından ve eşlemenin hiçbir **bir şekilde ayarlandığından emin olun.**  
    <pre><code>
    sudo cat  /etc/idmapd.conf
    # Example
    [General]
    Verbosity = 0
    Pipefs-Directory = /var/lib/nfs/rpc_pipefs
    Domain = <b>localdomain</b>
    [Mapping]
    Nobody-User = <b>nobody</b>
    Nobody-Group = <b>nobody</b>
    </code></pre>

4. **[A]** NFSv4 ID eşlemesini devre dışı bırakın. `nfs4_disable_idmapping` bulunduğu dizin yapısını oluşturmak için Mount komutunu yürütün.  Çekirdek/sürücü için erişim ayrıldığından,/sys/modules altında el ile dizin oluşturabilemeyeceksiniz.  

    <pre><code>
    
    mkdir /mnt/tmp
    mount 10.23.1.4:/HN1-shared /mnt/tmp
    umount  /mnt/tmp
    # Disable NFSv4 idmapping. 
    echo "N" > /sys/module/nfs/parameters/nfs4_disable_idmapping
    </code></pre>

5. **[A]** SAP HANA grubunu ve kullanıcıyı el ile oluşturun. Grup sapsys ve Kullanıcı **hn1**adm kimlikleri, ekleme sırasında sağlanmış olan kimliklere ayarlanmalıdır. Bu örnekte, kimlikler **1001**olarak ayarlanır. Aksi takdirde, birimlere erişmek mümkün olmayacaktır.  Grup sapsys ve Kullanıcı hesapları **hn1**adm ve sapadm için kimlikler, tüm sanal makinelerde aynı olmalıdır.  

    <pre><code>
    # Create user group 
    sudo groupadd -g 1001 sapsys
    # Create  users 
    sudo useradd <b>hn1</b>adm -u 1001 -g 1001 -d /usr/sap/<b>HN1</b>/home -c "SAP HANA Database System" -s /bin/sh
    sudo useradd sapadm -u 1002 -g 1001 -d /home/sapadm -c "SAP Local Administrator" -s /bin/sh
    # Set the password  for both user ids
    sudo passwd hn1adm
    sudo passwd sapadm
    </code></pre>

6. **[A]** paylaşılan Azure NetApp Files birimlerini bağlama.  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.5:/<b>HN1</b>-data-mnt00001 /hana/data/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-data-mnt00002 /hana/data/<b>HN1</b>/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-log-mnt00001 /hana/log/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-log-mnt00002 /hana/log/HN1/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-shared/shared /hana/shared  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount all volumes
    sudo mount -a 
    </code></pre>

7. **[1]** düğüme özgü birimleri **hanadb1**üzerinde bağlayın.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

8. **[2]** düğüme özgü birimleri **hanadb2**üzerine bağlayın.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb2</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

9. **[3]** düğüme özgü birimleri **hanadb3**üzerinde bağlayın.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb3</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

10. **[A]** tüm Hana birimlerinin NFS protokol sürümü **NFSv4**bağlandığından emin olun.  
    <pre><code>
    sudo nfsstat -m
    # Verify that flag vers is set to <b>4.1</b> 
    # Example from <b>hanadb1</b>
    /hana/data/<b>HN1</b>/mnt00001 from 10.23.1.5:/<b>HN1</b>-data-mnt00001
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.5
    /hana/log/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-log-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/data/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-data-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/log/<b>HN1</b>/mnt00001 from 10.23.1.4:/<b>HN1</b>-log-mnt00001
    Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /usr/sap/<b>HN1</b> from 10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b>
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /hana/shared from 10.23.1.4:/<b>HN1</b>-shared/shared
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    </code></pre>

## <a name="installation"></a>Yükleme  

Bu örnekte, Azure ile bekleme moduna sahip genişleme yapılandırmasında SAP HANA dağıtmak için HANA 2,0 SP4 kullandık.  

### <a name="prepare-for-hana-installation"></a>HANA yüklemesine hazırlanma

1. **[A]** Hana yüklemesinden önce kök parolayı ayarlayın (yükleme geri alındıktan sonra kök parolayı devre dışı bırakabilirsiniz). `root` komut `passwd`olarak yürütün.  

2. **[1]** parola istenmeden **hanadb2** ve **hanadb3**için SSH ile ilgili bir SSH olduğunu doğrulayın.  
    <pre><code>
    ssh root@<b>hanadb2</b>
    ssh root@<b>hanadb3</b>
    </code></pre>

3. **[A]** Hana 2,0 SP4 için gereken ek paketleri yükler. Ayrıntılar için bkz. sap Note [2593824](https://launchpad.support.sap.com/#/notes/2593824) . 
    <pre><code>
    sudo zypper install libgcc_s1 libstdc++6 libatomic1 
    </code></pre>

4. **[2, 3]** SAP HANA `data` ve `log` dizinlerinin sahipliğini **hn1**adm olarak değiştirin.   
    <pre><code>
    # Execute as root
    sudo chown hn1adm:sapsys /hana/data/<b>HN1</b>
    sudo chown hn1adm:sapsys /hana/log/<b>HN1</b>
    </code></pre>

### <a name="hana-installation"></a>HANA yüklemesi

1. **[1]** [SAP HANA 2,0 yükleme ve güncelleştirme kılavuzundaki](https://help.sap.com/viewer/2c1988d620e04368aa4103bf26f17727/2.0.04/en-US/7eb0167eb35e4e2885415205b8383584.html)yönergeleri izleyerek SAP HANA 'yi yükleme. Bu örnekte, ana, bir çalışan ve tek bir bekleme düğümüyle SAP HANA genişleme bir şekilde yüklenir.  
   HANA yükleme yazılımı dizininden **hdblcm** programını başlatın. `internal_network` parametresini kullanın ve iç HANA düğümler arası iletişim için kullanılan alt ağ için adres alanını geçirin.  

    <pre><code>./hdblcm --internal_network=10.23.3.0/24
    </code></pre>

   Komut istemine aşağıdaki değerleri girin.

     * Bir eylem seçin: **1** girin (Install için)
     * Yükleme için ek bileşenler seçin: **2, 3** girin
     * Yükleme yolunu girin: ENTER tuşuna basın (varsayılan olarak/Hana/Shared olarak)
     * Yerel ana bilgisayar adını girin: varsayılanı kabul etmek için ENTER tuşuna basın
     * Sisteme ana bilgisayar eklemek istiyor musunuz? **Y** girin
     * Eklenecek Coma ile ayrılmış ana bilgisayar adlarını girin: **hanadb2, hanadb3**
     * Kök Kullanıcı adı girin [root]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Kök kullanıcı parolasını girin: kökün parolasını girin
     * Konak hanadb2 için rolleri seçin: **1** girin (çalışan için)
     * Konak hanadb2 için konak yük devretme grubunu girin [varsayılan]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Host hanadb2 [<<assign automatically>>] için depolama bölümü numarasını girin: varsayılanı kabul etmek için ENTER tuşuna basın
     * Konak hanadb2 için çalışan grubunu girin [varsayılan]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Konak hanadb3 için rolleri seçin: **2** girin (bekleme için)
     * Konak hanadb3 için konak yük devretme grubunu girin [varsayılan]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Konak hanadb3 için çalışan grubunu girin [varsayılan]: varsayılanı kabul etmek için ENTER tuşuna basın
     * SAP HANA sistem KIMLIĞINI girin: **HN1** girin
     * Örnek numarasını girin [00]: **03** girin
     * Yerel konak çalışan grubunu girin [varsayılan]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Sistem kullanımı seçin/dizin girin [4]: **4** girin (özel için)
     * Veri birimlerinin konumunu girin [/hana/data/HN1]: varsayılanı kabul etmek için ENTER tuşuna basın
     * [/Hana/log/HN1] günlük birimlerinin konumunu girin: varsayılanı kabul etmek için ENTER tuşuna basın
     * Maksimum bellek ayırmayı kısıtla mı? [n]: **n** girin
     * Host hanadb1 Için sertifika ana bilgisayar adını girin [hanadb1]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Host hanadb2 Için sertifika ana bilgisayar adını girin [hanadb2]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Host hanadb3 Için sertifika ana bilgisayar adını girin [hanadb3]: varsayılanı kabul etmek için ENTER tuşuna basın
     * Sistem Yöneticisi (hn1adm) parolasını girin: parolayı girin
     * Sistem veritabanı kullanıcısı (SISTEM) parolasını girin: SISTEM parolasını girin
     * Sistem veritabanı kullanıcı (SISTEM) parolasını onaylayın: SISTEM parolasını girin
     * Makine yeniden başlatıldıktan sonra sistem yeniden başlatılsın mı? [n]: **n** girin 
     * Devam etmek istiyor musunuz (e/h): Özeti doğrula ve her şey iyi görünüyorsa **y** girin


2. **[1]** Global. ini 'yi doğrula  

   Global. ini ' i görüntüleyin ve iç SAP HANA düğümler arası iletişimin yapılandırmasının yerinde olduğundan emin olun. Bölüm **iletişimini**doğrulayın. **`hana`** alt ağ için adres alanına sahip olmalıdır ve `listeninterface` `.internal`olarak ayarlanmalıdır. Bölüm **internal_hostname_resolution**doğrulayın. **`hana`** alt ağına AIT olan Hana sanal makinelerinin IP adreslerine sahip olmalıdır.  

   <pre><code>
    sudo cat /usr/sap/<b>HN1</b>/SYS/global/hdb/custom/config/global.ini
    # Example 
    #global.ini last modified 2019-09-10 00:12:45.192808 by hdbnameserve
    [communication]
    internal_network = <b>10.23.3/24</b>
    listeninterface = .internal
    [internal_hostname_resolution]
    <b>10.23.3.4</b> = <b>hanadb1</b>
    <b>10.23.3.5</b> = <b>hanadb2</b>
    <b>10.23.3.6</b> = <b>hanadb3</b>
   </code></pre>

3. **[1]** istemci IP adreslerinin istemci iletişimi için kullanıldığından emin olmak için konak eşlemesi ekleyin. Bölüm `public_host_resolution` ekleyin ve istemci alt ağından ilgili IP adreslerini ekleyin.  

   <pre><code>
    sudo vi /usr/sap/HN1/SYS/global/hdb/custom/config/global.ini
    #Add the section
    [public_hostname_resolution]
    map_<b>hanadb1</b> = <b>10.23.0.5</b>
    map_<b>hanadb2</b> = <b>10.23.0.6</b>
    map_<b>hanadb3</b> = <b>10.23.0.7</b>
   </code></pre>

4. **[1]** değişiklikleri etkinleştirmek için SAP HANA yeniden başlatın.  
   <pre><code>
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StopSystem HDB
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StartSystem HDB
   </code></pre>

5. **[1]** istemci arabiriminin iletişim için **ISTEMCI** alt ağından IP adreslerini kullandığını doğrulayın.  
   <pre><code>
    sudo -u hn1adm /usr/sap/HN1/HDB03/exe/hdbsql -u SYSTEM -p "<b>password</b>" -i 03 -d SYSTEMDB 'select * from SYS.M_HOST_INFORMATION'|grep net_publicname
    # Expected result
    "<b>hanadb3</b>","net_publicname","<b>10.23.0.7</b>"
    "<b>hanadb2</b>","net_publicname","<b>10.23.0.6</b>"
    "<b>hanadb1</b>","net_publicname","<b>10.23.0.5</b>"
   </code></pre>

   Yapılandırmayı doğrulamaya ilişkin ayrıntılar için bkz. sap Note [2183363-SAP HANA iç ağ yapılandırması](https://launchpad.support.sap.com/#/notes/2183363) .  

6. Temel alınan Azure NetApp dosyaları depolaması için SAP HANA iyileştirmek üzere aşağıdaki SAP HANA parametrelerini ayarlayın:

   - `max_parallel_io_requests` **128**
   - **üzerinde** `async_read_submit`
   - **üzerinde** `async_write_submit_active`
   - `async_write_submit_blocks` **Tümü**

   Ayrıntılar için bkz. [NFS yapılandırma kılavuzu Ile NetApp AFF sistemlerinde SAP HANA](https://www.netapp.com/us/media/tr-4435.pdf). 

   SAP HANA 2,0 sistemlerinden başlayarak, `global.ini` parametreleri ayarlayabilirsiniz. Bkz. SAP Note [1999930](https://launchpad.support.sap.com/#/notes/1999930).  
   SAP HANA 1,0 sistemleri, SPS12 'e kadar olan sürümler için, SAP Note [2267798](https://launchpad.support.sap.com/#/notes/2267798)' de açıklandığı gibi, yükleme sırasında bu parametreler ayarlanabilir.  

7. Azure NetApp Files tarafından kullanılan depolamanın dosya boyutu sınırlaması veya 16 TB vardır. SAP HANA, depolama sınırlamasından örtük bir şekilde haberdar değildir ve 16 TB 'lık dosya boyutu sınırına ulaşıldığında otomatik olarak yeni veri dosyası oluşturmaz. SAP HANA, dosyayı 16 TB 'den daha fazla büyütmeye çalışır, bu da hatalara ve sonunda sunucu kilitlenmesine neden olur. 

   > [!IMPORTANT]
   > Veri dosyalarını depolama sybsystem 'in [16 TB sınırının](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-resource-limits) ötesinde büyütmeye çalışan SAP HANA engellemek için, aşağıdaki parametreleri `global.ini` ayarlayın.  
   > -  datavolume_striping = true
   > - Ayrıntılar Için datavolume_striping_size_gb = 15000 bkz. SAP Note [2400005](https://launchpad.support.sap.com/#/notes/2400005).
   > SAP Note [2631285](https://launchpad.support.sap.com/#/notes/2631285)'e dikkat edin. 

## <a name="test-sap-hana-failover"></a>Test SAP HANA yük devretme 

1. SAP HANA çalışan düğümünde düğüm kilitlenmesinin benzetimini yapma  

   **Hn1**adm olarak, düğüm kilitlenmesinin benzetimini yapmadan önce ortamın durumunu yakalamak için aşağıdaki komutları çalıştırın.  

   <pre><code>
    # Check the landscape status
    python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
   </code></pre>

   Bu örnekte, düğüm kilitlenmesinin benzetimini yapmak için **hanadb2** çalışan düğümünde kök olarak aşağıdaki komutu çalıştırın.  
   <pre><code>
    echo b > /proc/sysrq-trigger
   </code></pre>

   Yük devretme tamamlamayı sistem için izleyin. Yük devretme tamamlandığında durumu yakala-aşağıda sunulan durum gibi görünmelidir.  
   <pre><code>
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    # Check the landscape status
    /usr/sap/HN1/HDB03/exe/python_support> python landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | no     | info   |          |        |         2 |         0 | default  | default  | master 2   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb3 | yes    | info   |          |        |         0 |         2 | default  | default  | master 3   | slave      | standby     | slave       | standby | worker  | default | default |
   </code></pre>

   > [!IMPORTANT]
   > SAP HANA yük devretmeyle ilgili gecikmeleri önlemek için, bir düğüm çekirdek Panic ile karşılaştığında, **Tüm** Hana sanal makinelerde `kernel.panic` 20 saniyeye ayarlayın. Yapılandırma `/etc/sysctl` yapılır. Değişikliği etkinleştirmek için sanal makineleri yeniden başlatın. Yük devretme, bir düğüm çekirdek panik ile karşılaşıyorsa, bu değişiklik gerçekleştirilmediğinden 10 veya daha fazla dakika sürebilir.  

2. Ad sunucusunu Sonlandır  
   Testten önce ortamın durumunu denetlemek için, **hn1**adm olarak aşağıdaki komutları çalıştırın:  

   <pre><code>
    #Landscape status 
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
   </code></pre>

   **Hn1**adm olarak, bu örnekte **hanadb1**olan etkin ana düğümde aşağıdaki komutları çalıştırın.  

   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   Bekleme düğümü **hanadb3** , ana düğüm olarak ele alınacaktır. Yük devretme testi tamamlandıktan sonra kaynak durumu:  

   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | no     | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   Ad sunucusunun sonlandıralındığı sanal makinede, diğer bir deyişle, **hanadb1**üzerindeki Hana örneğini yeniden başlatın. **Hanadb1** düğümü, ortama yeniden katılacaktır ve bekleme rolünü tutar.  
   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   **Hanadb1**üzerinde SAP HANA başladıktan sonra aşağıdaki durumu bekliyor:  
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   Şimdi, hanadb3 üzerindeki geçerli etkin ana düğümdeki ad sunucusunu yeniden sonlandırın.  
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   Node **hanadb1** , ana düğümün rolünü sürdürecek. Durum, yük devretme testi tamamlandıktan sonra şöyle görünür:
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

   **Hanadb3** üzerinde SAP HANA başlatın-Bu, bir bekleme düğümü olarak hazırlanmaya hazır olacaktır.  
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   **Hanadb3**üzerinde SAP HANA başladıktan sonra durum.  
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [SAP için Azure sanal makineleri planlama ve uygulama][planning-guide]
* [SAP için Azure sanal makineleri dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* SAP 'nin olağanüstü durum kurtarma için yüksek kullanılabilirlik ve plan yapmayı öğrenin 
* Azure 'da HANA (büyük örnekler) için bkz. [Azure 'da SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Azure VM 'lerinde SAP HANA olağanüstü durum kurtarma için yüksek kullanılabilirlik ve plan planı oluşturma hakkında bilgi edinmek için bkz. [Azure sanal makinelerinde (VM) SAP HANA yüksek kullanılabilirliği][sap-hana-ha]
