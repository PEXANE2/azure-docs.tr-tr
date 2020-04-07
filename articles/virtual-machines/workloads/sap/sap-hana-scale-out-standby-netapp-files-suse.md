---
title: SLES'teki Azure NetApp Dosyaları ile BEKLEME dedahil olan SAP HANA ölçeklendirmesi | Microsoft Dokümanlar
description: SAP uygulamaları için Azure NetApp Dosyaları ile SUSE Linux Enterprise Server'da SAP NetWeaver için yüksek kullanılabilirlik kılavuzu
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
ms.date: 04/06/2020
ms.author: radeltch
ms.openlocfilehash: b584341931299e408b596bd6a66d1da4580cfffe
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754785"
---
# <a name="deploy-a-sap-hana-scale-out-system-with-standby-node-on-azure-vms-by-using-azure-netapp-files-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server'da Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtma 

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all 
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736
[1900823]:https://launchpad.support.sap.com/#/notes/1900823

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md


Bu makalede, paylaşılan depolama birimleri için [Azure NetApp Dosyalarını](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/) kullanarak Azure sanal makinelerinde (VM) bekleme alanı olan ölçeklendirme yapılandırmasında yüksek kullanılabilirlikte sap HANA sisteminin nasıl dağıtılancayakadar dağıtılanınca açıklanmaktadır.  

Örnek yapılandırmalarda, yükleme komutlarında ve benzeri durumlarda HANA örneği **03** ve HANA sistem kimliği **HN1'dir.** Örnekler SAP 12 SP4 için HANA 2.0 SP4 ve SUSE Linux Enterprise Server'ı temel almaktadır. 

Başlamadan önce aşağıdaki SAP notlarına ve bildirilerine bakın:

* [Azure NetApp Dosyaları belgeleri][anf-azure-doc] 
* SAP Note [1928533] şunları içerir:  
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü
* SAP Note [2015553]: Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler
* SAP Note [2205917]: SAP Uygulamaları için SUSE Linux Enterprise Server için önerilen işletim sistemi ayarlarını içerir
* SAP Note [1944799]: SAP Uygulamaları için SUSE Linux Enterprise Server için SAP Yönergeleri içerir
* SAP Note [2178632]: Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgi içerir
* SAP Note [2191498]: Azure'da Linux için gerekli SAP Host Agent sürümünü içerir
* SAP Note [2243692]: Azure'da Linux'ta SAP lisanslama hakkında bilgi içerir
* SAP Note [1984787]: SUSE Linux Enterprise Server 12 hakkında genel bilgiler içerir
* SAP Note [1999351]: SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgileri içerir
* SAP Note [1900823]: SAP HANA depolama gereksinimleri hakkında bilgi içerir
* [SAP Community Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes): Linux için gerekli tüm SAP notlarını içerir
* [Linux'ta SAP için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [SUSE SAP HA En İyi Uygulama Kılavuzları][suse-ha-guide]: NetWeaver Yüksek Kullanılabilirlik ve SAP HANA Sistem Çoğaltma şirket içinde kurmak için gerekli tüm bilgileri içerir (genel bir temel olarak kullanılacak; çok daha ayrıntılı bilgi sağlar)
* [SUSE Yüksek Kullanılabilirlik Uzantısı 12 SP3 Sürüm Notları][suse-ha-12sp3-relnotes]
* [Azure NetApp Dosyalarını Kullanarak Microsoft Azure'daki NetApp SAP Uygulamaları][anf-sap-applications-azure]


## <a name="overview"></a>Genel Bakış

HANA yüksek kullanılabilirlik elde etmek için bir yöntem ana bilgisayar otomatik failover yapılandırma gereğidir. Ana bilgisayar hatasını yapılandırmak için HANA sistemine bir veya daha fazla sanal makine ekler ve bunları bekleme düğümleri olarak yapılandırabilirsiniz. Etkin düğüm başarısız olduğunda, bekleme düğümü otomatik olarak devreye girer. Azure sanal makineleri ile sunulan yapılandırmada, [Azure NetApp Dosyaları'nda NFS'yi](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/)kullanarak otomatik olarak başarısız oluyorsunuz.  

> [!NOTE]
> Bekleme düğümütüm veritabanı birimlerine erişmesi gerekir. HANA hacimleri NFSv4 birimleri olarak monte edilmelidir. NFSv4 protokolünde geliştirilmiş dosya kiralama tabanlı kilitleme mekanizması `I/O` eskrim için kullanılır. 

> [!IMPORTANT]
> Desteklenen yapılandırmayı oluşturmak için HANA verilerini ve günlük hacimlerini NFSv4.1 birimleri olarak dağıtmanız ve NFSv4.1 protokolünü kullanarak bunları monte etmeniz gerekir. Bekleme düğümüne sahip HANA ana bilgisayar otomatik arıza yapılandırması NFSv3 ile desteklenmez.

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-suse-anf/sap-hana-scale-out-standby-netapp-files-suse.png)

SAP HANA ağ önerilerini izleyen önceki diyagramda, bir Azure sanal ağında üç alt ağ temsil edilir: 
* İstemci iletişimi için
* Depolama sistemi ile iletişim için
* Dahili HANA düğümler arası iletişim için

Azure NetApp birimleri, [Azure NetApp Dosyalarına devredilen](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet)ayrı bir alt ağdadır.  

Bu örnek yapılandırma için alt ağlar şunlardır:  

  - `client`10.23.0.0/24  
  - `storage`10.23.2.0/24  
  - `hana`10.23.3.0/24  
  - `anf`10.23.1.0/26  

## <a name="set-up-the-azure-netapp-files-infrastructure"></a>Azure NetApp Files altyapısını ayarlama 

Azure NetApp Files altyapısının kurulumuna geçmeden önce [Azure NetApp Dosyaları belgelerini][anf-azure-doc]edin. 

Azure NetApp Dosyaları çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/?products=netapp)kullanılabilir. Seçtiğiniz Azure bölgesinin Azure NetApp Dosyaları sunup sunmadığını kontrol edin.  

Azure bölgesine göre Azure NetApp Dosyalarının kullanılabilirliği hakkında bilgi için Azure [Bölgesine göre Azure NetApp Dosyaları Kullanılabilirliği][anf-avail-matrix]bölümüne bakın.  

Azure NetApp Dosyalarını dağıtmadan önce, [Azure NetApp Dosyaları talimatlarına kaydolarak][anf-register]Azure NetApp Dosyalarına binme isteğinde bulunun. 

### <a name="deploy-azure-netapp-files-resources"></a>Azure NetApp Dosyaları kaynaklarını dağıtma  

Aşağıdaki yönergeler, [Azure sanal ağınızı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)zaten dağıtmış olduğunuzu varsayar. Azure NetApp Dosyaları kaynaklarının ve Sanal M'lerinin, Azure NetApp Dosyaları kaynaklarının monte edildiği, aynı Azure sanal ağında veya eşlenen Azure sanal ağlarda dağıtılmalıdır.  

1. Kaynakları daha önce dağıtmadıysanız, [Azure NetApp Dosyalarına binme](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register)isteğinde bulunun.  

2. NetApp hesabı oluştur'daki yönergeleri izleyerek seçtiğiniz Azure bölgesinde [bir NetApp hesabı oluşturun.](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account)  

3. [Azure NetApp Dosyaları kapasite havuzu ayarlama'daki](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool)yönergeleri izleyerek bir Azure NetApp Files kapasite havuzu ayarlayın.  

   Bu makalede sunulan HANA mimarisi, *Ultra Hizmet* düzeyinde tek bir Azure NetApp Files kapasite havuzu kullanır. Azure'daki HANA iş yükleri için Azure NetApp Files *Ultra* veya *Premium* [hizmet Düzeyi](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)kullanmanızı öneririz.  

4. Azure NetApp Dosyalarına bir alt net temsilcilik'teki yönergelerde açıklandığı gibi [bir alt ağı Azure NetApp Dosyalarına devredin.](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet)  

5. [Azure NetApp Dosyaları için Bir NFS birimi oluştur'daki](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)yönergeleri izleyerek Azure NetApp Dosyaları birimlerini dağıtın.  

   Birimleri dağıtırken, **NFSv4.1** sürümünü seçtiğinizden emin olun. Şu anda, NFSv4.1'e erişim ek beyaz liste gerektirir. Birimleri belirlenen Azure NetApp Dosyaları [alt ağına](https://docs.microsoft.com/rest/api/virtualnetwork/subnets)dağıtın. 
   
   Azure NetApp Dosyaları kaynaklarının ve Azure Sanal M'lerinin aynı Azure sanal ağında veya eşlenen Azure sanal ağlarında olması gerektiğini unutmayın. Örneğin, **HN1**-data-mnt00001, **HN1**-log-mnt00001, ve benzeri, ses adları ve nfs://10.23.1.5/**HN1**-data-mnt0001, nfs://10.23.1.4/**HN1**-log-mnt00001, ve benzeri, AzureApp Files hacimleri için dosya yolları vardır.  

   * cilt **HN1**-data-mnt00001 (nfs://10.23.1.5/**HN1**-data-mnt00001)
   * cilt **HN1**-data-mnt00002 (nfs://10.23.1.6/**HN1**-data-mnt00002)
   * cilt **HN1**-log-mnt00001 (nfs://10.23.1.4/**HN1**-log-mnt00001)
   * cilt **HN1**-log-mnt00002 (nfs://10.23.1.6/**HN1**-log-mnt00002)
   * **hn1**-paylaşılan (nfs://10.23.1.4/**HN1**-paylaşılan)
   
   Bu örnekte, her HANA verisi ve günlük hacmi için ayrı bir Azure NetApp Files hacmi kullandık. Daha küçük veya üretken olmayan sistemlerde daha uygun maliyetli bir yapılandırma için, tüm veri montajlarını ve tüm günlükleri tek bir ses birimine yerleştirmek mümkündür.  

### <a name="important-considerations"></a>Önemli noktalar

SUSE Yüksek Kullanılabilirlik mimarisinde SAP NetWeaver için Azure NetApp Dosyalarınızı oluştururken, aşağıdaki önemli hususlara dikkat edin:

- Minimum kapasite havuzu 4 tebibayt (TiB) 'dir.  
- Minimum hacim boyutu 100 gibibayt (GiB) 'dir.
- Azure NetApp Dosyaları ve Azure NetApp Dosyaları nın monte edildiği tüm sanal makineler aynı Azure sanal ağında veya aynı bölgedeki [eşlenen sanal ağlarda](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) olmalıdır.  
- Seçili sanal ağ, Azure NetApp Dosyaları'na devredilen bir alt ağa sahip olmalıdır.
- Azure NetApp Files biriminin iş hacmi, [Azure NetApp Dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)için Hizmet düzeyinde belgelenen birim kotası ve hizmet düzeyinin bir fonksiyonudur. HANA Azure NetApp birimlerini boyutlandırıyorken, elde edilen iş hacminin HANA sistem gereksinimlerini karşıladığından emin olun.  
- Azure NetApp Files [dışa aktarma ilkesiyle,](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy)izin verilen istemcileri, erişim türünü (yalnızca okuma-yazma vb.) denetleyebilirsiniz. 
- Azure NetApp Dosyaları özelliği henüz bölge farkında değil. Şu anda, özellik bir Azure bölgesindeki tüm kullanılabilirlik bölgelerinde dağıtılamıyor. Bazı Azure bölgelerindeki olası gecikme sonu etkilerine dikkat edin.  
-  

> [!IMPORTANT]
> SAP HANA iş yükleri için düşük gecikme durumu önemlidir. Sanal makinelerin ve Azure NetApp Dosyaları birimlerinin yakın mesafede dağıtıldığından emin olmak için Microsoft temsilcinizle birlikte çalışın.  

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Azure NetApp Dosyalarında HANA veritabanı için boyutlandırma

Azure NetApp Files biriminin iş hacmi, [Azure NetApp Dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)için Hizmet düzeyinde belgelenen ses boyutu ve hizmet düzeyinin bir fonksiyonudur. 

Azure'da SAP altyapısı tasarlarken, SAP'nin minimum iş geliştirme özelliklerine dönüşen bazı minimum depolama gereksinimlerine dikkat edin:

- /hana/log/hana/log'da /hana/log'da 1 MB G/Ç boyutlarıyla saniyede 250 megabayt (MB/s) okuma-yazma olanağı nı etkinleştirin.  
- 16 MB ve 64 MB G/Ç boyutları için /hana/data için en az 400 MB/sn okuma etkinliğini etkinleştirin.  
- 16 MB ve 64 MB G/Ç boyutlarında /hana/data için en az 250 MB/s'lik yazma etkinliğini etkinleştirin. 

[Azure NetApp Dosyaları birim](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) kotasının 1 TiB'si başına iş hacmi limitleri şunlardır:
- Premium Depolama katmanı - 64 MiB/s  
- Ultra Depolama katmanı - 128 MiB/s  

Sap'ın veri ve günlük için minimum iş verme gereksinimlerini ve /hana/paylaşılan yönergeleri karşılamak için önerilen boyutlar:

| Birim | Boyutu<br>Premium Depolama katmanı | Boyutu<br>Ultra Depolama katmanı | Desteklenen NFS protokolü |
| --- | --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB | v4.1 |
| /hana/veri | 6.3 TiB | 3.2 TiB | v4.1 |
| /hana/paylaşılan | 4 işçi düğümü başına maksimum (512 GB, 1xRAM) | 4 işçi düğümü başına maksimum (512 GB, 1xRAM) | v3 veya v4.1 |

Azure NetApp Files Ultra Depolama katmanını kullanarak bu makalede sunulan düzen için SAP HANA yapılandırması şu şekilde olacaktır:

| Birim | Boyutu<br>Ultra Depolama katmanı | Desteklenen NFS protokolü |
| --- | --- | --- |
| /hana/log/mnt00001 | 2 TiB | v4.1 |
| /hana/log/mnt00002 | 2 TiB | v4.1 |
| /hana/veri/mnt00001 | 3.2 TiB | v4.1 |
| /hana/veri/mnt00002 | 3.2 TiB | v4.1 |
| /hana/paylaşılan | 2 TiB | v3 veya v4.1 |

> [!NOTE]
> Burada belirtilen Azure NetApp Dosyaları boyutlandırma önerileri, SAP'nin altyapı sağlayıcıları için önerdiği minimum gereksinimleri karşılamayı hedeflemektedir. Gerçek müşteri dağıtımlarında ve iş yükü senaryolarında bu boyutlar yeterli olmayabilir. Bu önerileri başlangıç noktası olarak kullanın ve belirli iş yükünüzün gereksinimlerini temel alan uyarlayın.  

> [!TIP]
> Azure NetApp Dosyaları birimlerini, birimleri *sökmek,* sanal makineleri durdurmak veya SAP HANA'yı durdurmak zorunda kalmadan dinamik olarak yeniden boyutlandırabilirsiniz. Bu yaklaşım, uygulamanızın beklenen ve öngörülemeyen üretim taleplerini karşılama esnekliği sağlar.

## <a name="deploy-linux-virtual-machines-via-the-azure-portal"></a>Azure portalı üzerinden Linux sanal makinelerini dağıtma

Öncelikle Azure NetApp Dosyaları ciltlerini oluşturmanız gerekir. Ardından aşağıdaki adımları yapın:
1. [Azure sanal ağınızda](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) [Azure sanal ağ alt ağlarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) oluşturun. 
1. VM'leri dağıtın. 
1. Ek ağ arabirimleri oluşturun ve ağ arabirimlerini ilgili VM'lere takın.  

   Her sanal makinenin üç Azure sanal ağ alt ağına karşılık`client` `storage` gelen `hana`üç ağ arabirimi vardır ( ve ). 

   Daha fazla bilgi için [azure'da birden çok ağ arabirimi kartı içeren bir Linux sanal makinesi oluşturun'a](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics)bakın.  

> [!IMPORTANT]
> SAP HANA iş yükleri için düşük gecikme durumu önemlidir. Düşük gecikme gecikmesi elde etmek için, sanal makinelerin ve Azure NetApp Dosyaları birimlerinin yakın mesafede dağıtıldığından emin olmak için Microsoft temsilcinizle birlikte çalışın. SAP HANA Azure NetApp Dosyalarını kullanan [yeni SAP HANA sistemine binerken,](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) gerekli bilgileri gönderin. 
 
Sonraki yönergeler, kaynak grubunu, Azure sanal ağı ve üç Azure sanal ağ `client`alt `storage` `hana`ağını zaten oluşturduğunuzu varsayar: ve . VM'leri dağıttığınızda, istemci ağ arabiriminin VM'lerin birincil arabirimi olması için istemci alt ağını seçin. Ayrıca, depolama alt ağ geçidi üzerinden Azure NetApp Dosyaları'na yetki veda eden alt ağına açık bir rota yapılandırmanız gerekir. 

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi, kullanmakta olduğunuz belirli VM türlerinde SAP HANA için SAP sertifikalı olduğundan emin olun. SAP HANA sertifikalı VM türlerinin ve bu tür işletim sistemi sürümlerinin listesi için [SAP HANA sertifikalı IaaS platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) sitesine gidin. Bu tür için SAP HANA destekli işletim sistemi sürümlerinin tam listesini almak için listelenen VM türünün ayrıntılarını tıklatın.  

1. SAP HANA için kullanılabilirlik kümesi oluşturun. Max update etki alanını ayarladıklıolun.  

2. Aşağıdaki adımları yaparak üç sanal makine **(hanadb1**, **hanadb2**, **hanadb3)** oluşturun:  

   a. SAP HANA için desteklenen Azure galerisinde bir SLES4SAP resmi kullanın. Bu örnekte bir SLES4SAP 12 SP4 görüntüsü kullandık.  

   b. SAP HANA için daha önce oluşturduğunuz kullanılabilirlik kümesini seçin.  

   c. İstemci Azure sanal ağ alt ağını seçin. [Hızlandırılmış Ağı](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)seçin.  

   Sanal makineleri dağıttığınızda, ağ arabirimi adı otomatik olarak oluşturulur. Basitlik için bu talimatlarda, istemci Azure sanal ağ alt ağına **hanadb1 istemci**, **hanadb2 istemci**ve **hanadb3 istemci**olarak bağlı olan otomatik olarak oluşturulan ağ arabirimlerine atıfta bulunacağız. 

3. `storage` Sanal ağ alt ağı için her sanal makine için bir tane olmak üzere üç ağ arabirimi oluşturun (bu örnekte **hanadb1-depolama, hanadb2-depolama**ve **hanadb2-storage** **hanadb3-depolama).**  

4. `hana` Sanal ağ alt ağı için her sanal makine için bir tane olmak üzere üç ağ arabirimi oluşturun (bu örnekte **hanadb1-hana**, **hanadb2-hana**ve **hanadb3-hana).**  

5. Yeni oluşturulan sanal ağ arabirimlerini aşağıdaki adımları yaparak ilgili sanal makinelere takın:  

    a. [Azure portalındaki](https://portal.azure.com/#home)sanal makineye gidin.  

    b. Sol bölmede Sanal **Makineler'i**seçin. Sanal makine adını filtreleyin (örneğin, **hanadb1),** ve ardından sanal makineyi seçin.  

    c. Genel **Bakış** bölmesinde, sanal makineyi bulmak için Durdur'u'yu seçin. **Stop**  

    d. **Ağ'ı**seçin ve ardından ağ arabirimini takın. Ağ **arabirimi** açılır listesini ekle, alt ağlar için `hana` zaten `storage` oluşturulmuş ağ arabirimlerini seçin.  
    
    e. **Kaydet'i**seçin. 
 
    f. Kalan sanal makineler için b adımlarını tekrarlayın (örneğimizde, **hanadb2** ve **hanadb3).**
 
    g. Şimdilik durdurulmuş durumda sanal makineleri bırakın. Ardından, yeni eklenen tüm ağ arabirimleri için [hızlandırılmış ağ etkinleştireceğiz.](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)  

6. Aşağıdaki adımları yaparak `storage` alt ağlar ve `hana` alt ağlar için ek ağ arabirimleri için hızlandırılmış ağ etkinleştirme:  

    a. [Azure portalında](https://portal.azure.com/#home) [Azure Bulut Kabuğu'nu](https://azure.microsoft.com/features/cloud-shell/) açın.  

    b. Alt ağlara ve `storage` `hana` alt ağlara bağlı ek ağ arabirimleri için hızlandırılmış ağ etkinleştirmek için aşağıdaki komutları çalıştırın.  

    <pre><code>
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-storage</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-storage</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-storage</b> --accelerated-networking true
    
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-hana</b> --accelerated-networking true

    </code></pre>

7. Aşağıdaki adımları yaparak sanal makineleri başlatın:  

    a. Sol bölmede Sanal **Makineler'i**seçin. Sanal makine adını filtreleyin (örneğin, **hanadb1**), ve sonra seçin.  

    b. Genel **Bakış** bölmesinde **Başlat'ı**seçin.  

## <a name="operating-system-configuration-and-preparation"></a>İşletim sistemi yapılandırması ve hazırlanması

Sonraki bölümlerdeki talimatlar aşağıdakilerden biriyle önceden belirlenmiştir:
* **[A]**: Tüm düğümler için geçerlidir
* **[1]**: Yalnızca düğüm 1 için geçerlidir
* **[2]**: Yalnızca düğüm 2 için geçerlidir
* **[3]**: Yalnızca düğüm için geçerli 3

Aşağıdaki adımları yaparak işletim sisteminizi yapılandırın ve hazırlayın:

1. **[A]** Sanal makinelerdeki ana bilgisayar dosyalarını koruyun. Tüm alt ağlar için girişler ekleyin. Bu örnek `/etc/hosts` için aşağıdaki girişler eklendi.  

    <pre><code>
    # Storage
    10.23.2.4   hanadb1-storage
    10.23.2.5   hanadb2-storage
    10.23.2.6   hanadb3-storage
    # Client
    10.23.0.5   hanadb1
    10.23.0.6   hanadb2
    10.23.0.7   hanadb3
    # Hana
    10.23.3.4   hanadb1-hana
    10.23.3.5   hanadb2-hana
    10.23.3.6   hanadb3-hana
    </code></pre>

2. **[A]** İstenmeyen ana bilgisayar ad değişikliklerini önlemek için depolama için ağ arabirimi için DHCP ve bulut config ayarlarını değiştirin.  

    Aşağıdaki yönergeler depolama ağı `eth1`arabiriminin . 

    <pre><code>
    vi /etc/sysconfig/network/dhcp
    # Change the following DHCP setting to "no"
    DHCLIENT_SET_HOSTNAME="no"
    vi /etc/sysconfig/network/ifcfg-<b>eth1</b>
    # Edit ifcfg-eth1 
    #Change CLOUD_NETCONFIG_MANAGE='yes' to "no"
    CLOUD_NETCONFIG_MANAGE='no'
    </code></pre>

2. **[A]** Azure NetApp Dosyaları'na iletişimin depolama ağı arabirimi üzerinden gitmesi için bir ağ rotası ekleyin.  

    Aşağıdaki yönergeler depolama ağı `eth1`arabiriminin .  

    <pre><code>
    vi /etc/sysconfig/network/ifroute-<b>eth1</b>
    # Add the following routes 
    # RouterIPforStorageNetwork - - -
    # ANFNetwork/cidr RouterIPforStorageNetwork - -
    <b>10.23.2.1</b> - - -
    <b>10.23.1.0/26</b> <b>10.23.2.1</b> - -
    </code></pre>

    Değişiklikleri etkinleştirmek için VM'yi yeniden başlatın.  

3. **[A]** [Azure NetApp Dosyalarını kullanarak Microsoft Azure'daki NetApp SAP Uygulamalarında][anf-sap-applications-azure]açıklandığı gibi, Os'u NFS ile NetApp Sistemlerinde SAP HANA'yı çalıştırmak için hazırlayın. NetApp yapılandırma ayarları için configuration file */etc/sysctl.d/netapp-hana.conf* oluşturun.  

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

4. **[A]** Azure yapılandırma ayarları için Microsoft ile yapılandırma dosyası */etc/sysctl.d/ms-az.conf* oluşturun.  

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

4. **[A]** [Azure NetApp Dosyalarını kullanarak Microsoft Azure'daki NetApp SAP Uygulamalarında][anf-sap-applications-azure]önerilen sunrpc ayarlarını ayarlayın.  

    <pre><code>
    vi /etc/modprobe.d/sunrpc.conf
    # Insert the following line
    options sunrpc tcp_max_slot_table_entries=128
    </code></pre>

## <a name="mount-the-azure-netapp-files-volumes"></a>Azure NetApp Dosyalarını ekleme

1. **[A]** HANA veritabanı birimleri için montaj noktaları oluşturun.  

    <pre><code>
    mkdir -p /hana/data/<b>HN1</b>/mnt00001
    mkdir -p /hana/data/<b>HN1</b>/mnt00002
    mkdir -p /hana/log/<b>HN1</b>/mnt00001
    mkdir -p /hana/log/<b>HN1</b>/mnt00002
    mkdir -p /hana/shared
    mkdir -p /usr/sap/<b>HN1</b>
    </code></pre>

2. **[1]** **HN1**-paylaşılan /usr/sap için düğüme özgü dizinler oluşturun.  

    <pre><code>
    # Create a temporary directory to mount <b>HN1</b>-shared
    mkdir /mnt/tmp
    mount <b>10.23.1.4</b>:/<b>HN1</b>-shared /mnt/tmp
    cd /mnt/tmp
    mkdir shared usr-sap-<b>hanadb1</b> usr-sap-<b>hanadb2</b> usr-sap-<b>hanadb3</b>
    # unmount /hana/shared
    cd
    umount /mnt/tmp
    </code></pre>

3. **[A]** NFS etki alanı ayarını doğrulayın. Etki alanının varsayılan Azure NetApp Files etki alanı olarak yapılandırıldığından, yani **`defaultv4iddomain.com`** eşlemenin hiç **kimseye**ayarlı olmadığından emin olun.  

    > [!IMPORTANT]
    > NFS etki alanını `/etc/idmapd.conf` Azure NetApp Dosyaları'ndaki varsayılan etki alanı yapılandırmasına **`defaultv4iddomain.com`** uyacak şekilde VM'de ayarladığından emin olun: . NFS istemcisindeki etki alanı yapılandırması (yani VM) ile NFS sunucusu, yani Azure NetApp yapılandırması arasında bir uyuşmazlık varsa, Azure NetApp birimlerinde VM'lere monte `nobody`edilen dosyaların izinleri görüntülenir.  

    <pre><code>
    sudo cat /etc/idmapd.conf
    # Example
    [General]
    Verbosity = 0
    Pipefs-Directory = /var/lib/nfs/rpc_pipefs
    Domain = <b>defaultv4iddomain.com</b>
    [Mapping]
    Nobody-User = <b>nobody</b>
    Nobody-Group = <b>nobody</b>
    </code></pre>

4. **[A]** `nfs4_disable_idmapping`Doğrula. **Y**olarak ayarlanmalıdır. Bulunduğu yerdeki `nfs4_disable_idmapping` dizin yapısını oluşturmak için montaj komutunu çalıştırın. /sys/modules altında dizini el ile oluşturamazsınız, çünkü erişim çekirdek / sürücüler için ayrılmıştır.  

    <pre><code>
    # Check nfs4_disable_idmapping 
    cat /sys/module/nfs/parameters/nfs4_disable_idmapping
    # If you need to set nfs4_disable_idmapping to Y
    mkdir /mnt/tmp
    mount 10.23.1.4:/HN1-shared /mnt/tmp
    umount  /mnt/tmp
    echo "Y" > /sys/module/nfs/parameters/nfs4_disable_idmapping
    # Make the configuration permanent
    echo "options nfs nfs4_disable_idmapping=Y" >> /etc/modprobe.d/nfs.conf
    </code></pre>

5. **[A]** SAP HANA grubunu ve kullanıcıyı el ile oluşturun. Grup sapsys ve kullanıcı **hn1**adm için dIss onboarding sırasında sağlanan aynı adlar için ayarlanmalıdır. (Bu örnekte, d'ler **1001**olarak ayarlanır.) Aygıtlar doğru ayarlanmazsa, birimlere erişemezsin. Grup sapsys ve kullanıcı hesapları **hn1**adm ve sapadm için dIss tüm sanal makinelerde aynı olmalıdır.  

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

6. **[A]** Paylaşılan Azure NetApp Dosyaları birimlerini monte edin.  

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

7. **[1]** **Hanadb1**düğüme özgü hacimleri monte edin.  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

8. **[2]** **Hanadb2'de**düğüme özgü hacimleri monte edin.  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb2</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

9. **[3]** **Hanadb3'te**düğüme özgü hacimleri monte edin.  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb3</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

10. **[A]** Tüm HANA birimlerinin NFS protokol sürümü **NFSv4**ile monte edilmiş olduğunu doğrulayın.  

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

Bu örnekte, SAP HANA'yı Azure ile bekleme düğümü ile ölçeklendirme yapılandırmasında dağıtmak için HANA 2.0 SP4 kullandık.  

### <a name="prepare-for-hana-installation"></a>HANA kurulumu için hazırlanın

1. **[A]** HANA yüklemesinden önce kök parolayı ayarlayın. Yükleme tamamlandıktan sonra kök parolayı devre dışı kullanabilirsiniz. Komut `passwd` `root` olarak yürütün.  

2. **[1]** SSH üzerinden **hanadb2** ve **hanadb3'e**şifre istenmeden giriş yapabileceğinizi doğrulayın.  

    <pre><code>
    ssh root@<b>hanadb2</b>
    ssh root@<b>hanadb3</b>
    </code></pre>

3. **[A]** HANA 2.0 SP4 için gerekli olan ek paketleri yükleyin. Daha fazla bilgi için SAP Note [2593824'e](https://launchpad.support.sap.com/#/notes/2593824)bakın. 

    <pre><code>
    sudo zypper install libgcc_s1 libstdc++6 libatomic1 
    </code></pre>

4. **[2], [3]** SAP HANA'nın `data` `log` ve dizinlerin sahipliğini **hn1**adm olarak değiştirin.   

    <pre><code>
    # Execute as root
    sudo chown hn1adm:sapsys /hana/data/<b>HN1</b>
    sudo chown hn1adm:sapsys /hana/log/<b>HN1</b>
    </code></pre>

### <a name="hana-installation"></a>HANA kurulumu

1. **[1]** [SAP HANA 2.0 Kurulum ve Güncelleme kılavuzundaki](https://help.sap.com/viewer/2c1988d620e04368aa4103bf26f17727/2.0.04/en-US/7eb0167eb35e4e2885415205b8383584.html)yönergeleri izleyerek SAP HANA'yı yükleyin. Bu örnekte, sap HANA ölçeğini ana, bir işçi ve bir bekleme düğümü ile yüklüyoruz.  

   a. **HDBLCM** programını HANA yükleme yazılımı dizininden başlatın. Parametreyi `internal_network` kullanın ve iç HANA düğüm iletişimi için kullanılan alt ağ için adres alanını geçirin.  

    <pre><code>
    ./hdblcm --internal_network=10.23.3.0/24
    </code></pre>

   b. İsteyerek, aşağıdaki değerleri girin:

     * Eylem **seçin**için: **1** girin (yüklemek için)
     * **Kurulum için ek bileşenler için**: girin **2, 3**
     * Yükleme yolu için: Enter tuşuna basın (varsayılan olarak /hana/shared)
     * **Yerel Ana Bilgisayar Adı için**: varsayılanı kabul etmek için Enter tuşuna basın
     * Altında **Sisteme ana bilgisayar eklemek istiyor musunuz?**: **y** girin
     * **Virgülle ayrılmış ev sahibi adlarını eklemek için**: **hanadb2 girin, hanadb3**
     * **Root Kullanıcı Adı** [root]için: varsayılanı kabul etmek için Enter tuşuna basın
     * **Kök Kullanıcı Şifresi**için : kök kullanıcının şifresini girin
     * Ev sahibi hanadb2 rolleri için: **1** girin (işçi için)
     * Ev sahibi hanadb2 [varsayılan] için **Ana Bilgisayar Failover Grubu** için: varsayılanı kabul etmek için Enter tuşuna basın
     * Ana **Storage Partition Number** bilgisayar hanadb2 [<<assign automatically>>] için Depolama Bölüm Numarası için: varsayılanı kabul etmek için Enter tuşuna basın
     * Ev sahibi hanadb2 [varsayılan]için **İşçi Grubu** için: varsayılanı kabul etmek için Enter tuşuna basın
     * Ev sahibi hanadb3 için **seç rolleri** için: **2** girin (bekleme için)
     * Ana bilgisayar hanadb3 [varsayılan] için **Ana Bilgisayar Failover Grubu** için: varsayılankabul etmek için Enter tuşuna basın
     * Ev sahibi hanadb3 [varsayılan]için **İşçi Grubu** için: varsayılanı kabul etmek için Enter tuşuna basın
     * **SAP HANA Sistem Kimliği**için : **HN1** girin
     * **Örneğin numarası** [00]: **03** girin
     * **Yerel Ana Bilgisayar İşçi Grubu** [varsayılan] için: varsayılanı kabul etmek için Enter tuşuna basın
     * **Select System Usage / Enter index [4]** için : enter **4** (özel için)
     * **Veri Birimlerinin Konumu** için [/hana/data/HN1]: varsayılanı kabul etmek için Enter tuşuna basın
     * **Günlük Hacimlerinin Konumu** için [/hana/log/HN1]: varsayılanı kabul etmek için Enter tuşuna basın
     * **Maksimum bellek tahsisini kısıtlamak** için mi? [n]: **n** girin
     * **Ev Sahibi hanadb1 [hanadb1]için Sertifika Ana Bilgisayar Adı** için: varsayılanı kabul etmek için Enter tuşuna basın
     * **Ev Sahibi hanadb2 [hanadb2]için Sertifika Host Adı** için: varsayılanı kabul etmek için Enter tuşuna basın
     * **Ev Sahibi hanadb3 [hanadb3]için Sertifika Ana Bilgisayar Adı** için: varsayılanı kabul etmek için Enter tuşuna basın
     * **Sistem Yöneticisi (hn1adm) Şifresi**için : şifreyi girin
     * **Sistem Veritabanı Kullanıcı (sistem) Şifresi**için : sistemin şifresini girin
     * **Sistem Veritabanı Kullanıcı (sistem) Şifresi onaylayın**için : sistemin şifregirin
     * **Makine yeniden başlatıldıktan sonra sistemi yeniden başlatma için mi?** [n]: **n** girin 
     * Devam **etmek istiyor musunuz (y/n)**: özeti doğrulayın ve her şey iyi görünüyorsa, **y** girin


2. **[1]** Global.ini'yi doğrulayın  

   Global.ini'yi görüntüleyin ve iç SAP HANA düğüm iletişiminin yapılandırmasının yerinde olduğundan emin olun. **İletişim** bölümünü doğrulayın. `hana` Alt ağ için adres alanı olmalı `listeninterface` ve `.internal`'' olarak ayarlanmalıdır. **internal_hostname_resolution** bölümünü doğrulayın. `hana` Alt ağa ait HANA sanal makinelerin IP adreslerine sahip olmalıdır.  

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

3. **[1]** İstemci IP adreslerinin istemci iletişimi için kullanıldığından emin olmak için ana bilgisayar eşleme ekleyin. Bölüm `public_host_resolution`ekleyin ve istemci alt netinden ilgili IP adreslerini ekleyin.  

   <pre><code>
    sudo vi /usr/sap/HN1/SYS/global/hdb/custom/config/global.ini
    #Add the section
    [public_hostname_resolution]
    map_<b>hanadb1</b> = <b>10.23.0.5</b>
    map_<b>hanadb2</b> = <b>10.23.0.6</b>
    map_<b>hanadb3</b> = <b>10.23.0.7</b>
   </code></pre>

4. **[1]** Değişiklikleri etkinleştirmek için SAP HANA'yı yeniden başlatın.  

   <pre><code>
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StopSystem HDB
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StartSystem HDB
   </code></pre>

5. **[1]** İstemci arabiriminin iletişim için `client` alt ağdaki IP adreslerini kullandığını doğrulayın.  

   <pre><code>
    sudo -u hn1adm /usr/sap/HN1/HDB03/exe/hdbsql -u SYSTEM -p "<b>password</b>" -i 03 -d SYSTEMDB 'select * from SYS.M_HOST_INFORMATION'|grep net_publicname
    # Expected result
    "<b>hanadb3</b>","net_publicname","<b>10.23.0.7</b>"
    "<b>hanadb2</b>","net_publicname","<b>10.23.0.6</b>"
    "<b>hanadb1</b>","net_publicname","<b>10.23.0.5</b>"
   </code></pre>

   Yapılandırmayı nasıl doğrulayınız hakkında bilgi için SAP Note [2183363 - SAP HANA dahili ağının yapılandırmasına](https://launchpad.support.sap.com/#/notes/2183363)bakın.  

6. Altta yatan Azure NetApp Dosyaları depolaması için SAP HANA'yı optimize etmek için aşağıdaki SAP HANA parametrelerini ayarlayın:

   - `max_parallel_io_requests` **128**
   - `async_read_submit` **on**
   - `async_write_submit_active` **on**
   - `async_write_submit_blocks`**tüm**

   Daha fazla bilgi için [Azure NetApp Dosyalarını kullanarak Microsoft Azure'daki NetApp SAP Uygulamaları'na][anf-sap-applications-azure]bakın. 

   SAP HANA 2.0 sistemlerinden başlayarak parametreleri `global.ini`. Daha fazla bilgi için SAP Note [1999930'a](https://launchpad.support.sap.com/#/notes/1999930)bakın.  
   
   SAP HANA 1.0 sistem sürümleri SPS12 ve daha önceki sürümlerde, bu parametreler SAP Note [2267798'de](https://launchpad.support.sap.com/#/notes/2267798)açıklandığı gibi kurulum sırasında ayarlanabilir.  

7. Azure NetApp Files tarafından kullanılan depolama alanı, 16 terabayt (TB) dosya boyutu sınırlamasına sahiptir. SAP HANA depolama sınırlamasının dolaylı olarak farkında değildir ve 16 TB dosya boyutu sınırına ulaşıldığında otomatik olarak yeni bir veri dosyası oluşturmaz. SAP HANA dosyayı 16 TB'nin ötesine büyütmeye çalışırken, bu girişim hatalara ve sonunda bir dizin sunucusu çökmesine neden olur. 

   > [!IMPORTANT]
   > SAP HANA'nın veri dosyalarını depolama alt sisteminin [16-TB sınırının](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-resource-limits) ötesine `global.ini`büyütmeye çalışmasından korunmak için aşağıdaki parametreleri ayarlayın.  
   > - datavolume_striping = doğru
   > - datavolume_striping_size_gb = 15000 Daha fazla bilgi için SAP Note [2400005'e](https://launchpad.support.sap.com/#/notes/2400005)bakın.
   > SAP Note [2631285'e](https://launchpad.support.sap.com/#/notes/2631285)dikkat edin. 

## <a name="test-sap-hana-failover"></a>Test SAP HANA failover 

1. SAP HANA işçi düğümünde düğüm çökmesini simüle edin. Şunları yapın: 

   a. Düğüm çökmesini simüle etmeden önce, ortamın durumunu yakalamak için aşağıdaki komutları **hn1**adm olarak çalıştırın:  

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

   b. Bir düğüm çökmesini simüle etmek için, aşağıdaki komutu bu durumda **hanadb2** olan alt düğümüzerinde kök olarak çalıştırın:  
   
   <pre><code>
    echo b > /proc/sysrq-trigger
   </code></pre>

   c. Tamamlanmayı başarısız olması için sistemi izleyin. Başarısız lık tamamlandığında, aşağıdaki gibi görünmesi gereken durumu yakalayın:  

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
   > Bir düğüm çekirdek paniği yaşadığında, `kernel.panic` *tüm* HANA sanal makinelerinde 20 saniyeye ayarlayarak SAP HANA'nın başarısız olmasıyla gecikmeleri önleyin. Yapılandırma ' da `/etc/sysctl`yapılır. Değişikliği etkinleştirmek için sanal makineleri yeniden başlatın. Bu değişiklik gerçekleştirilemiyorsa, bir düğüm çekirdek paniği yaşadığında başarısız olmak 10 dakika veya daha fazla sürebilir.  

2. Aşağıdakileri yaparak ad sunucusunu öldürün:

   a. Testten önce, aşağıdaki komutları **hn1**adm olarak çalıştırarak ortamın durumunu kontrol edin:  

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

   b. Bu durumda **hanadb1** olan etkin ana düğümüzerinde aşağıdaki komutları **hn1**adm olarak çalıştırın:  

    <pre><code>
        hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB kill
    </code></pre>
    
    Bekleme düğümü **hanadb3** ana düğüm olarak devralacak. Başarısızlık testi tamamlandıktan sonra kaynak durumu aşağıda veda edebilirsiniz:  

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

   c. **HANADB1'deki** HANA örneğini yeniden başlatın (diğer bir deyişle, ad sunucusunun öldürüldüğü aynı sanal makinede). **Hanadb1** düğümü çevreye yeniden katılacak ve bekleme rolünü koruyacaktır.  

   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   SAP HANA **hanadb1**başladıktan sonra, aşağıdaki durumu bekleyin:  

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

   d. Yine, şu anda etkin ana düğüm (yani, düğüm **hanadb3**üzerinde) ad sunucusu öldürmek.  
   
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   Düğüm **hanadb1** ana düğüm rolünü devam ettirecektir. Başarısız lık testi tamamlandıktan sonra durum şu şekilde görünür:

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

   e. Bir bekleme düğümü olarak hizmet vermeye hazır olacak **hanadb3,** SAP HANA başlatın.  

   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   SAP HANA **hanadb3**başladıktan sonra, durum aşağıdaki gibi görünüyor:  

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

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma planını öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği][sap-hana-ha]bölümüne bakın.
