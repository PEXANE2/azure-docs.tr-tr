---
title: SLES multi-SID kılavuzunda SAP NetWeaver için Azure VM'ler yüksek kullanılabilirlik | Microsoft Dokümanlar
description: SAP uygulamaları için SUSE Linux Enterprise Server'da SAP NetWeaver için Çoklu SID yüksek kullanılabilirlik kılavuzu
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
ms.date: 03/26/2020
ms.author: radeltch
ms.openlocfilehash: 793851780e1154b6b6a21c88ea8cae063a277790
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350067"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications-multi-sid-guide"></a>Sap uygulamaları multi-SID kılavuzu için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

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

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

Bu makalede, SAP uygulamaları için SUSE Linux Enterprise Server ile Azure VM'lerde iki düğüm kümesinde birden çok SAP NetWeaver veya S4HANA yüksek kullanılabilir sistem (yani multi-SID) nasıl dağıtılanın açıklanmaktadır.  

Örnek yapılandırmalarda, kurulum komutları vb üç SAP NetWeaver 7.50 sistemleri tek, iki düğüm yüksek kullanılabilirlik kümedağıtılır. SAP sistemleri SIT'ler şunlardır:
* **NW1**: ASCS örnek numarası **00** ve sanal ana bilgisayar adı **msnw1ascs**; ERS örnek numarası **02** ve sanal ana bilgisayar adı **msnw1ers**.  
* **NW2**: ASCS örnek numarası **10** ve sanal hostname **msnw2ascs**; ERS örnek numarası **12** ve sanal ana bilgisayar adı **msnw2ers**.  
* **NW3**: ASCS örnek numarası **20** ve sanal hostname **msnw3ascs**; ERS örnek numarası **22** ve sanal ana bilgisayar adı **msnw3ers**.  

Makale veritabanı katmanını ve SAP NFS paylaşımlarının dağıtımını kapsamaz. Bu makaledeki örneklerde, NW2 NFS hisseleri için sanal adlar ve NW3 NFS hisseleri için nw3-nfs kullanıyoruz, NFS kümesinin dağıtıldığını varsayarak.  

Başlamadan önce aşağıdaki SAP Notları ve kağıtlarına bakın:

* SAP Note [1928533][1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutları listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü

* SAP Note [2015553,][2015553] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2205917][2205917] SAP Uygulamaları için SUSE Linux Enterprise Server için işletim sistemi ayarlarını tavsiye etti
* SAP Note [1944799][1944799] SAP Uygulamaları için SUSE Linux Enterprise Server için SAP HANA Yönergeleri vardır
* SAP Note [2178632,][2178632] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [2191498,][2191498] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,][2243692] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1984787,][1984787] SUSE Linux Enterprise Server 12 hakkında genel bilgilere sahiptir.
* SAP Note [1999351,][1999351] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notları'na sahiptir.
* [Linux'ta SAP için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [SUSE SAP HA En İyi Uygulama Kılavuzları][suse-ha-guide] Kılavuzlar, Netweaver HA ve SAP HANA Sistem Çoğaltma'yı şirket içinde kurmak için gerekli tüm bilgileri içerir. Bu kılavuzları genel bir taban çizgisi olarak kullanın. Onlar çok daha ayrıntılı bilgi sağlar.
* [SUSE Yüksek Kullanılabilirlik Uzantısı 12 SP3 Sürüm Notları][suse-ha-12sp3-relnotes]
* [SLES 12 ve SLES 15 için SUSE multi-SID küme kılavuzu](https://documentation.suse.com/sbp/all/html/SBP-SAP-MULTI-SID/index.html)
* [Azure NetApp Dosyalarını Kullanarak Microsoft Azure'daki NetApp SAP Uygulamaları][anf-sap-applications-azure]
## <a name="overview"></a>Genel Bakış

Kümeye katılan sanal makinelerin, başarısız olması durumunda tüm kaynakları çalıştırabilmek için boyutlandırılmaları gerekir. Her SAP SID, çoklu SID yüksek kullanılabilirlik kümesinde birbirinden bağımsız olarak başarısız olabilir.  SBD eskrim kullanıyorsanız, SBD aygıtları birden çok küme arasında paylaşılabilir.  

Yüksek kullanılabilirlik elde etmek için SAP NetWeaver yüksek kullanılabilirlik li NFS hisselerine ihtiyaç duramaz. Bu örnekte, SAP NFS hisselerinin birden çok SAP sistemi tarafından kullanılabilen yüksek kullanılabilir [NFS dosya sunucusunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)barındırıldığı varsayıyoruz. Veya paylaşımlar Azure [NetApp Files NFS birimlerinde](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)dağıtılır.  

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-suse/ha-suse-multi-sid.png)

> [!IMPORTANT]
> SUSE Linux ile SAP ASCS/ERS'in Azure VM'lerde konuk işletim sistemi olarak çoklu SID kümelemesi desteği, aynı kümedeki **beş** SAP SID ile sınırlıdır. Her yeni SID karmaşıklığı artırır. Aynı kümedeki SAP Enqueue Replication Server 1 ve Enqueue Replication Server 2'nin bir karışımı **desteklenmez.** Multi-SID kümeleme, tek bir Pacemaker kümesinde farklı SID'ler içeren birden çok SAP ASCS/ERS örneğinin yüklenmesini açıklar. Şu anda çoklu SID kümeleme yalnızca ASCS/ERS için desteklenir.  

> [!TIP]
> SAP ASCS/ERS'in çoklu SID kümelemesi daha yüksek karmaşıklığa sahip bir çözümdür. Bu uygulamak için daha karmaşıktır. Ayrıca, bakım faaliyetleri (işletim sistemi yama gibi) yürütüldüğünde daha yüksek yönetim çabası içerir. Gerçek uygulamayı başlatmadan önce, dağıtımı ve VM'ler, NFS yuvaları, VIP'ler, yük dengeleyici yapılandırmaları gibi tüm ilgili bileşenleri dikkatle planlamak için zaman ayırın.  

NFS sunucusu, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. [Standart yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)kullanmanızı öneririz.  

Aşağıdaki liste, üç SAP sistemi olan bu çok SID küme örneği için (A)SCS ve ERS yük dengeleyicisinin yapılandırmasını gösterir. Her bir SAND örneği için ayrı ön uç IP'si, sistem durumu sondaları ve yük dengeleme kurallarına ihtiyacınız olacaktır. ASCS/ASCS kümesinin bir parçası olan tüm VM'leri tek bir arka uç havuzuna atayın.  

### <a name="ascs"></a>(A) Scs

* Frontend yapılandırması
  * NW1 için IP adresi: 10.3.1.14
  * NW2 için IP adresi: 10.3.1.16
  * NW3 için IP adresi: 10.3.1.13
* Sonda Bağlantı Noktaları
  * Port 620<strong>&lt;&gt;nr,</strong>bu nedenle NW1, NW2 ve NW3 prob bağlantı noktaları için 620**00**, 620**10** ve 620**20**
* Yük dengeleme kuralları - 
* nw1/ASCS, NW2/ASCS ve NW3/ASCS olmak üzere her örnek için bir tane oluşturun.
  * Standart Yük Dengeleyicisi kullanıyorsanız, **HA bağlantı noktalarını** seçin
  * Temel Yük Dengeleyicisi kullanıyorsanız, aşağıdaki bağlantı noktaları için Yük dengeleme kuralları oluşturun
    * 32<strong>&lt;nr&gt; </strong> TCP
    * 36<strong>&lt;nr&gt; </strong> TCP
    * 39<strong>&lt;nr&gt; </strong> TCP
    * 81<strong>&lt;nr&gt; </strong> TCP
    * 5<strong>&lt;nr&gt;</strong>13 TCP
    * 5<strong>&lt;nr&gt;</strong>14 TCP
    * 5<strong>&lt;nr&gt;</strong>16 TCP

### <a name="ers"></a>Ers

* Frontend yapılandırması
  * NW1 10.3.1.15 için IP adresi
  * NW2 10.3.1.17 için IP adresi
  * NW3 10.3.1.19 için IP adresi
* Sonda Bağlantı Noktası
  * Port 621<strong>&lt;&gt;nr,</strong>bu nedenle NW1, NW2 ve N# prob bağlantı noktaları için 621**02**, 621**12** ve 621**22**
* Yük dengeleme kuralları - her örnek için bir tane oluşturun, yani NW1/ERS, NW2/ERS ve NW3/ERS.
  * Standart Yük Dengeleyicisi kullanıyorsanız, **HA bağlantı noktalarını** seçin
  * Temel Yük Dengeleyicisi kullanıyorsanız, aşağıdaki bağlantı noktaları için Yük dengeleme kuralları oluşturun
    * 32<strong>&lt;nr&gt; </strong> TCP
    * 33<strong>&lt;nr&gt; </strong> TCP
    * 5<strong>&lt;nr&gt;</strong>13 TCP
    * 5<strong>&lt;nr&gt;</strong>14 TCP
    * 5<strong>&lt;nr&gt;</strong>16 TCP

* Arka uç yapılandırması
  * (A)SCS/ERS kümesinin bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı


> [!Note]
> Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ortak bitiş noktalarına yönlendirmeye izin verecek ek yapılandırma yapılmadığı sürece giden internet bağlantısı olmaz. Giden bağlantının nasıl elde edilene ilişkin ayrıntılar [için, SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için Genel uç nokta bağlantısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections)bakın.  

> [!IMPORTANT]
> Azure Yük Bakiyesi'nin arkasına yerleştirilen Azure VM'lerinde TCP zaman damgalarını etkinleştirme. TCP zaman damgalarını etkinleştirmek sistem durumu sondalarının başarısız lığa neden olur. Parametre **net.ipv4.tcp_timestamps** **0**'a ayarlayın. Ayrıntılar için [Bkz. Yük Dengeleyici sağlık probları.](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

## <a name="sap-nfs-shares"></a>SAP NFS hisseleri

SAP NetWeaver, taşıma, profil dizini ve benzeri için paylaşılan depolama gerektirir. Yüksek kullanılabilir SAP sistemi için, yüksek kullanılabilir NFS hisselerine sahip olmak önemlidir. SAP NFS hisselerinizin mimarisine karar vermeniz gerekir. Seçeneklerden biri, birden çok SAP sistemi arasında paylaşılabilen [SUSE Linux Enterprise Server'da Azure VM'lerde Yüksek Oranda kullanılabilir NFS kümesi][nfs-ha]oluşturmaktır. 

Başka bir seçenek Azure [NetApp Files NFS hacimleri](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)üzerinde hisse dağıtmak için.  Azure NetApp Dosyaları ile SAP NFS paylaşımları için yerleşik yüksek kullanılabilirlik elde elabilirsiniz.

## <a name="deploy-the-first-sap-system-in-the-cluster"></a>Kümedeki ilk SAP sistemini dağıtma

SAP NFS hisselerinin mimarisine karar verdiğiniz için, ilgili belgeleri izleyerek kümedeki ilk SAP sistemini dağıtın.

* Yüksek kullanılabilirlikteki NFS sunucusu kullanıyorsanız, [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)izleyin.  
* Azure NetApp Files NFS birimlerini kullanıyorsanız, [SAP uygulamaları için Azure NetApp Dosyaları ile SUSE Linux Enterprise Server'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) takip edin

Yukarıda listelenen belgeler, gerekli altyapıları hazırlamak, küme oluşturmak, SAP uygulamasını çalıştırmak için işletim sistemi hazırlamak için adımlar boyunca size rehberlik edecektir.  

> [!TIP]
> Kümeye ek SAP SIT'leri eklemeden önce, ilk sistem dağıtıldıktan sonra kümenin işlevselliği üzerinde her zaman başarısız lığı test edin. Bu şekilde küme işlevselliğinin, kümeye ek SAP sistemlerinin karmaşıklığını eklemeden önce çalıştığını bileceksiniz.   

## <a name="deploy-additional-sap-systems-in-the-cluster"></a>Kümeye ek SAP sistemleri dağıtma

Bu örnekte, **NW1** sisteminin kümede zaten dağıtıldığını varsayıyoruz. Sap sistemleri **NW2** ve **NW3**kümede nasıl dağıtılanın göstereceğiz. 

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

### <a name="prerequisites"></a>Ön koşullar 

> [!IMPORTANT]
> Kümede ek SAP sistemleri dağıtma yönergelerini izlemeden önce, yalnızca ilk sistem dağıtımı sırasında gerekli olan adımlar olduğundan, kümedeki ilk SAP sistemini dağıtmak için yönergeleri izleyin.  

Bu belge, şunları varsayar:
* Kalp Pili kümesi zaten yapılandırılmış ve çalışıyor.  
* En az bir SAP sistemi (ASCS / ERS örneği) zaten dağıtılmış ve kümede çalışıyor.  
* Küme işlevsellik üzerinde başarısız sınandı.  
* Tüm SAP sistemleri için NFS hisseleri dağıtılır.  

### <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver Kurulumu için hazırlanın

1. Azure portalı üzerinden Azure [Yük Bakiyesini el ile dağıt'ı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files#deploy-azure-load-balancer-manually-via-azure-portal)uygulayın yönergeleri izleyerek, mevcut Azure Yük Dengeleyicisine yeni dağıtılan sistem için yapılandırma (diğer bir şekilde **NW2**, **NW3)** ekleyin. Yapılandırmanız için IP adreslerini, sistem durumu sonda bağlantı noktalarını, yük dengeleme kurallarını ayarlayın.  

2. **[A]** Ek SAP sistemleri için ad çözümlemesi ayarlayın. DNS sunucusu'nı kullanabilir `/etc/hosts` veya tüm düğümlerde değişiklik yapabilirsiniz. Bu örnek, dosyanın `/etc/hosts` nasıl kullanılacağını gösterir.  IP adreslerini ve ana bilgisayar adlarını ortamınıza uyarlayın. 

    ```
    sudo vi /etc/hosts
    # IP address of the load balancer frontend configuration for NW2 ASCS
    10.3.1.16 msnw2ascs
    # IP address of the load balancer frontend configuration for NW3 ASCS
    10.3.1.13 msnw3ascs
    # IP address of the load balancer frontend configuration for NW2 ERS
    10.3.1.17 msnw2ers
    # IP address of the load balancer frontend configuration for NW3 ERS
    10.3.1.19 msnw3ers
    # IP address for virtual host name for the NFS server for NW2
    10.3.1.31 nw2-nfs
    # IP address for virtual host name for the NFS server for NW3
    10.3.1.32 nw3-nfs
   ```

3. **[A]** Kümeye dağıttığınız ek **NW2** ve **NW3** SAP sistemleri için paylaşılan dizinleri oluşturun. 

    ```
    sudo mkdir -p /sapmnt/NW2
    sudo mkdir -p /usr/sap/NW2/SYS
    sudo mkdir -p /usr/sap/NW2/ASCS10
    sudo mkdir -p /usr/sap/NW2/ERS12
    sudo mkdir -p /sapmnt/NW3
    sudo mkdir -p /usr/sap/NW3/SYS
    sudo mkdir -p /usr/sap/NW3/ASCS20
    sudo mkdir -p /usr/sap/NW3/ERS22

    
    sudo chattr +i /sapmnt/NW2
    sudo chattr +i /usr/sap/NW2/SYS
    sudo chattr +i /usr/sap/NW2/ASCS10
    sudo chattr +i /usr/sap/NW2/ERS12
    sudo chattr +i /sapmnt/NW3
    sudo chattr +i /usr/sap/NW3/SYS
    sudo chattr +i /usr/sap/NW3/ASCS20
    sudo chattr +i /usr/sap/NW3/ERS22
   ```

4. **[A]** `autofs` Kümeye dağıttığınız ek SAP sistemleri için /sapmnt/SID ve /usr/sap/SID/SID/SYS dosya sistemlerini monte etmek için yapılandırın. Bu örnekte **NW2** ve **NW3**.  

   Kümeye `/etc/auto.direct` dağıttığınız ek SAP sistemleri için dosyayı dosyayı güncelleştirin.  

   * NFS dosya sunucusu kullanıyorsanız, [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse#prepare-for-sap-netweaver-installation) yönergeleri izleyin
   * Azure NetApp Dosyalarını kullanıyorsanız, [aşağıdaki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files#prepare-for-sap-netweaver-installation) talimatları izleyin 

   Yeni eklenen paylaşımları `autofs` monte etmek için hizmeti yeniden başlatmanız gerekir.  

### <a name="install-ascs--ers"></a>ASCS / ERS'i yükleyin

1. Kümeye dağıttıkladığınız ek SAP sisteminin ASCS örneği için sanal IP ve sistem sondası küme kaynaklarını oluşturun. Burada gösterilen örnek, yüksek **kullanılabilirLikteki NFS** sunucusunu kullanan NW2 ve **NW3** ASCS içindir.  

   > [!IMPORTANT]
   > Son testler, netcat'in biriktirme listesi ve yalnızca bir bağlantı taşıma sınırlaması nedeniyle isteklere yanıt vermeyi bıraktığı durumları ortaya çıkardı. Netcat kaynağı Azure Yük dengeleyici isteklerini dinlemeyi durdurur ve kayan IP kullanılamaz hale gelir.  
   > Mevcut Pacemaker kümeleri için, geçmişte netcat'i socat ile değiştirmemizi tavsiye ettik. Şu anda, paket kaynak aracılarının bir parçası olan azure-lb kaynak aracısını aşağıdaki paket sürüm gereksinimleriyle birlikte kullanmanızı öneririz:
   > - SLES 12 SP4/SP5 için, sürüm en az kaynak-aracılar-4.3.018.a7fb5035-3.30.1 olmalıdır.  
   > - SLES 15/15 SP1 için, sürüm en az kaynak-ajanlar-4.3.0184.6ee15eb2-4.13.1 olmalıdır.  
   >
   > Değişikliğin kısa bir kapalı kalma süresi gerektireceğini unutmayın.  
   > Varolan Pacemaker kümeleri için yapılandırma Azure [Load-Balancer Detection Hardening'de](https://www.suse.com/support/kb/doc/?id=7024128)açıklandığı gibi socat kullanmak üzere zaten değiştirildiyse, hemen azure-lb kaynak aracısına geçmeniz gerek yoktur.

    ```
      sudo crm configure primitive fs_NW2_ASCS Filesystem device='nw2-nfs:/NW2/ASCS' directory='/usr/sap/NW2/ASCS10' fstype='nfs4' \
       op start timeout=60s interval=0 \
       op stop timeout=60s interval=0 \
       op monitor interval=20s timeout=40s
   
      sudo crm configure primitive vip_NW2_ASCS IPaddr2 \
        params ip=10.3.1.16 cidr_netmask=24 \
        op monitor interval=10 timeout=20
   
      sudo crm configure primitive nc_NW2_ASCS azure-lb port=62010
   
      sudo crm configure group g-NW2_ASCS fs_NW2_ASCS nc_NW2_ASCS vip_NW2_ASCS \
         meta resource-stickiness=3000

      sudo crm configure primitive fs_NW3_ASCS Filesystem device='nw3-nfs:/NW3/ASCS' directory='/usr/sap/NW3/ASCS20' fstype='nfs4' \
        op start timeout=60s interval=0 \
        op stop timeout=60s interval=0 \
        op monitor interval=20s timeout=40s
   
      sudo crm configure primitive vip_NW3_ASCS IPaddr2 \
       params ip=10.3.1.13 cidr_netmask=24 \
       op monitor interval=10 timeout=20
   
      sudo crm configure primitive nc_NW3_ASCS azure-lb port=62020
   
      sudo crm configure group g-NW3_ASCS fs_NW3_ASCS nc_NW3_ASCS vip_NW3_ASCS \
        meta resource-stickiness=3000
    ```

   Kaynakları oluştururken, farklı küme kaynaklarına atanabilirler. Bunları grupladığınızda, küme düğümlerinden birine taşınırlar. Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

2. **[1]** SAP NetWeaver ASCS yükleyin  

   SAP NetWeaver ASCS'yi, ASCS için yük dengeleyici ön uç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adı kullanarak kök olarak yükleyin. Örneğin, sistem **NW2**için, sanal ana bilgisayar adı <b>msnw2ascs</b>, <b>10.3.1.16</b> ve yük dengeleyici, örneğin <b>10</b>sonda sı için kullanılan örnek numarasıdır. sistem **NW3**için , sanal hostname <b>msnw3ascs</b>, <b>10.3.1.13</b> ve yük dengeleyici, örneğin <b>20</b>prob için kullanılan örnek numarasıdır.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz. Sanal ana bilgisayar adını kullanarak SAP'yi yüklemek için parametre SAPINST_USE_HOSTNAME kullanabilirsiniz.  

     ```
      sudo swpm/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=virtual_hostname
     ```

   Yükleme /usr/sap/**SID**/ASCS**Instance#** bir alt klasör oluşturamazsa, sahibini **SID**Adm'e ayarlamayı deneyin ve ASCS**Instance#** ve yeniden deneyin.

3. **[1]** Kümeye dağıttığınız ek SAP sisteminin ERS örneği için sanal bir IP ve sistem sondası kümesi kaynakları oluşturun. Burada gösterilen örnek, yüksek **kullanılabilirLikteki NFS** sunucusunu kullanan NW2 ve **NW3** ERS içindir. 

   ```
    sudo crm configure primitive fs_NW2_ERS Filesystem device='nw2-nfs:/NW2/ASCSERS' directory='/usr/sap/NW2/ERS12' fstype='nfs4' \
      op start timeout=60s interval=0 \
      op stop timeout=60s interval=0 \
      op monitor interval=20s timeout=40s
   
    sudo crm configure primitive vip_NW2_ERS IPaddr2 \
      params ip=10.3.1.17 cidr_netmask=24 \
      op monitor interval=10 timeout=20
   
    sudo crm configure primitive nc_NW2_ERS azure-lb port=62112
   
    sudo crm configure group g-NW2_ERS fs_NW2_ERS nc_NW2_ERS vip_NW2_ERS

    sudo crm configure primitive fs_NW3_ERS Filesystem device='nw3-nfs:/NW3/ASCSERS' directory='/usr/sap/NW3/ERS22' fstype='nfs4' \
      op start timeout=60s interval=0 \
      op stop timeout=60s interval=0 \
      op monitor interval=20s timeout=40s
   
    sudo crm configure primitive vip_NW3_ERS IPaddr2 \
      params ip=10.3.1.19 cidr_netmask=24 \
      op monitor interval=10 timeout=20
   
    sudo crm configure primitive nc_NW3_ERS azure-lb port=62122
   
    sudo crm configure group g-NW3_ERS fs_NW3_ERS nc_NW3_ERS vip_NW3_ERS
   ```

   Kaynakları oluştururken farklı küme düğümlerine atanabilirler. Bunları grupladığınızda, küme düğümlerinden birine taşınırlar. Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun.  

   Ardından, yeni oluşturulan ERS grubunun kaynaklarının küme düğümünde, aynı SAP sistemi için ASCS örneğinin yüklendiği küme düğümünün karşısında çalıştığından emin olun.  Örneğin, NW2 ASCS'nin üzerine `slesmsscl1`yüklenmişse, NW2 ERS `slesmsscl2`grubunun üzerinde çalışıyorduklarından emin olun.  Aşağıdaki komutu `slesmsscl2` çalıştırarak NW2 ERS grubunu geçirebilirsiniz: 

    ```
      crm resource migrate g-NW2_ERS slesmsscl2 force
    ```

4. **[2]** SAP NetWeaver ERS'i yükleyin

   ERS için yük dengeleyici ön uç yapılandırmasının IP adresine eşlenen sanal bir ana bilgisayar adı kullanarak SAP NetWeaver ERS'i diğer düğüme kök olarak yükleyin. Örneğin sistem **NW2**için, sanal ana bilgisayar adı <b>msnw2ers</b>olacak , <b>10.3.1.17</b> ve yük dengeleyici sondası için kullandığınız örnek numarası, örneğin <b>12</b>. Sistem **NW3**için , sanal ana bilgisayar adı <b>msnw3ers</b>, <b>10.3.1.19</b> ve yük dengeleyici sondası için kullandığınız örnek numarası, örneğin <b>22</b>. 

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz. Sanal ana bilgisayar adını kullanarak SAP'yi yüklemek için parametre SAPINST_USE_HOSTNAME kullanabilirsiniz.  

    ```
     sudo swpm/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=virtual_hostname
    ```

   > [!NOTE]
   > SWPM SP 20 PL 05 veya daha yüksek kullanın. Alt sürümler izinleri doğru ayarlamaz ve yükleme başarısız olur.

   Yükleme /usr/sap/**NW2**/ERS**Instance#** bir alt klasör oluşturamazsa, **sahibisid**adm ve grup ERS**Instance#** klasörünün sapsys ve yeniden deneyin sahibi ayarlamayı deneyin.

   Yeni dağıtılan SAP sisteminin ERS grubunu farklı bir küme düğümüne geçirmeniz gerekliyse, ERS grubunun konum kısıtlamasını kaldırmayı unutmayın. Aşağıdaki komutu çalıştırarak kısıtlamayı kaldırabilirsiniz (örnek SAP sistemleri **NW2** ve **NW3**için verilir).  

    ```
      crm resource unmigrate g-NW2_ERS
      crm resource unmigrate g-NW3_ERS
    ```

5. **[1]** Yeni yüklenen SAP sistemi(ler) için ASCS/SCS ve ERS örnek profillerini uyarlayın. Aşağıda gösterilen örnek NW2 içindir. Kümeye eklenen tüm SAP örnekleri için ASCS/SCS ve ERS profillerini uyarlamanız gerekir.  
 
 * ASCS/SCS profili

   ```
   sudo vi /sapmnt/NW2/profile/NW2_ASCS10_msnw2ascs
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   ```

 * ERS profili

   ```
   sudo vi /sapmnt/NW2/profile/NW2_ERS12_msnw2ers
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # remove Autostart from ERS profile
   # Autostart = 1
   ```

6. **[A]** Bu örnekte, yeni dağıtılan SAP sistemi için SAP kullanıcılarını yapılandırın, **NW2** ve **NW3**. 

   ```
   # Add sidadm to the haclient group
   sudo usermod -aG haclient nw2adm
   sudo usermod -aG haclient nw3adm
   ```

7. Yeni yüklenen SAP sistemi için ASCS ve ERS `sapservice` SAP hizmetlerini dosyaya ekleyin. Aşağıda gösterilen örnek SAP sistemleri **NW2** ve **NW3**içindir.  

   Ascs hizmet girişini ikinci düğüme ekleyin ve ERS hizmet girişini ilk düğüme kopyalayın. SAP sistemi için ASCS örneğinin yüklendiği düğümdeki her SAP sisteminin komutlarını çalıştırın.  

    ```
     # Execute the following commands on slesmsscl1,assuming the NW2 ASCS instance was installed on slesmsscl1
     cat /usr/sap/sapservices | grep ASCS10 | sudo ssh slesmsscl2 "cat >>/usr/sap/sapservices"
     sudo ssh slesmsscl2 "cat /usr/sap/sapservices" | grep ERS12 | sudo tee -a /usr/sap/sapservices
     # Execute the following commands on slesmsscl2, assuming the NW3 ASCS instance was installed on slesmsscl2
     cat /usr/sap/sapservices | grep ASCS20 | sudo ssh slesmsscl1 "cat >>/usr/sap/sapservices"
     sudo ssh slesmsscl1 "cat /usr/sap/sapservices" | grep ERS22 | sudo tee -a /usr/sap/sapservices
    ```

8. **[1]** Yeni yüklenen SAP sistemi için SAP küme kaynaklarını oluşturun. 

   Enqueue server 1 mimarisi (ENSA1) kullanıyorsanız, SAP sistemleri **NW2** ve **NW3** kaynaklarını aşağıdaki gibi tanımlayın:

    ```
     sudo crm configure property maintenance-mode="true"
    
     sudo crm configure primitive rsc_sap_NW2_ASCS10 SAPInstance \
      operations \$id=rsc_sap_NW2_ASCS10-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW2_ASCS10_msnw2ascs START_PROFILE="/sapmnt/NW2/profile/NW2_ASCS10_msnw2ascs" \
      AUTOMATIC_RECOVER=false \
      meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
    
     sudo crm configure primitive rsc_sap_NW2_ERS12 SAPInstance \
      operations \$id=rsc_sap_NW2_ERS12-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW2_ERS12_msnw2ers START_PROFILE="/sapmnt/NW2/profile/NW2_ERS12_msnw2ers" AUTOMATIC_RECOVER=false IS_ERS=true \
      meta priority=1000
    
     sudo crm configure modgroup g-NW2_ASCS add rsc_sap_NW2_ASCS10
     sudo crm configure modgroup g-NW2_ERS add rsc_sap_NW2_ERS12
    
     sudo crm configure colocation col_sap_NW2_no_both -5000: g-NW2_ERS g-NW2_ASCS
     sudo crm configure location loc_sap_NW2_failover_to_ers rsc_sap_NW2_ASCS10 rule 2000: runs_ers_NW2 eq 1
     sudo crm configure order ord_sap_NW2_first_start_ascs Optional: rsc_sap_NW2_ASCS10:start rsc_sap_NW2_ERS12:stop symmetrical=false
   
     sudo crm configure primitive rsc_sap_NW3_ASCS20 SAPInstance \
      operations \$id=rsc_sap_NW3_ASCS20-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW3_ASCS10_msnw3ascs START_PROFILE="/sapmnt/NW3/profile/NW3_ASCS20_msnw3ascs" \
      AUTOMATIC_RECOVER=false \
      meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
    
     sudo crm configure primitive rsc_sap_NW3_ERS22 SAPInstance \
      operations \$id=rsc_sap_NW3_ERS22-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW3_ERS22_msnw3ers START_PROFILE="/sapmnt/NW3/profile/NW3_ERS22_msnw2ers" AUTOMATIC_RECOVER=false IS_ERS=true \
      meta priority=1000
    
     sudo crm configure modgroup g-NW3_ASCS add rsc_sap_NW3_ASCS20
     sudo crm configure modgroup g-NW3_ERS add rsc_sap_NW3_ERS22
    
     sudo crm configure colocation col_sap_NW3_no_both -5000: g-NW3_ERS g-NW3_ASCS
     sudo crm configure location loc_sap_NW3_failover_to_ers rsc_sap_NW3_ASCS10 rule 2000: runs_ers_NW3 eq 1
     sudo crm configure order ord_sap_NW3_first_start_ascs Optional: rsc_sap_NW3_ASCS20:start rsc_sap_NW3_ERS22:stop symmetrical=false
     sudo crm configure property maintenance-mode="false"
    ```

   SAP, SAP NW 7.52 itibariyle çoğaltma da dahil olmak üzere enqueue server 2 için destek sundu. ABAP Platform 1809 ile başlayarak, enqueue server 2 varsayılan olarak yüklenir. Enqueue server 2 desteği için SAP note [2630416'ya](https://launchpad.support.sap.com/#/notes/2630416) bakın.
   Enqueue server 2 mimarisi[(ENSA2)](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)kullanıyorsanız, SAP sistemleri **NW2 ve NW3** kaynaklarını aşağıdaki gibi tanımlayın: **NW3**

    ```
     sudo crm configure property maintenance-mode="true"
    
     sudo crm configure primitive rsc_sap_NW2_ASCS10 SAPInstance \
      operations \$id=rsc_sap_NW2_ASCS10-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW2_ASCS10_msnw2ascs START_PROFILE="/sapmnt/NW2/profile/NW2_ASCS10_msnw2ascs" \
      AUTOMATIC_RECOVER=false \
      meta resource-stickiness=5000 
    
     sudo crm configure primitive rsc_sap_NW2_ERS12 SAPInstance \
      operations \$id=rsc_sap_NW2_ERS12-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW2_ERS12_msnw2ers START_PROFILE="/sapmnt/NW2/profile/NW2_ERS12_msnw2ers" AUTOMATIC_RECOVER=false IS_ERS=true 
    
     sudo crm configure modgroup g-NW2_ASCS add rsc_sap_NW2_ASCS10
     sudo crm configure modgroup g-NW2_ERS add rsc_sap_NW2_ERS12
    
     sudo crm configure colocation col_sap_NW2_no_both -5000: g-NW2_ERS g-NW2_ASCS
     sudo crm configure order ord_sap_NW2_first_start_ascs Optional: rsc_sap_NW2_ASCS10:start rsc_sap_NW2_ERS12:stop symmetrical=false
   
     sudo crm configure primitive rsc_sap_NW3_ASCS20 SAPInstance \
      operations \$id=rsc_sap_NW3_ASCS20-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW3_ASCS10_msnw3ascs START_PROFILE="/sapmnt/NW3/profile/NW3_ASCS20_msnw3ascs" \
      AUTOMATIC_RECOVER=false \
      meta resource-stickiness=5000
    
     sudo crm configure primitive rsc_sap_NW3_ERS22 SAPInstance \
      operations \$id=rsc_sap_NW3_ERS22-operations \
      op monitor interval=11 timeout=60 on-fail=restart \
      params InstanceName=NW3_ERS22_msnw3ers START_PROFILE="/sapmnt/NW3/profile/NW3_ERS22_msnw2ers" AUTOMATIC_RECOVER=false IS_ERS=true
    
     sudo crm configure modgroup g-NW3_ASCS add rsc_sap_NW3_ASCS20
     sudo crm configure modgroup g-NW3_ERS add rsc_sap_NW3_ERS22
    
     sudo crm configure colocation col_sap_NW3_no_both -5000: g-NW3_ERS g-NW3_ASCS
     sudo crm configure order ord_sap_NW3_first_start_ascs Optional: rsc_sap_NW3_ASCS20:start rsc_sap_NW3_ERS22:stop symmetrical=false
     sudo crm configure property maintenance-mode="false"
    ```

   Eski bir sürümden yükseltiyor ve enqueue server 2'ye geçiyorsanız, SAP note [2641019'a](https://launchpad.support.sap.com/#/notes/2641019)bakın. 

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.
   Aşağıdaki örnek, SAP sistemleri **NW2** ve **NW3** kümeye eklendikten sonra küme kaynakları durumunu gösterir. 

    ```
     sudo crm_mon -r
    
    # Online: [ slesmsscl1 slesmsscl2 ]
    
    #Full list of resources:
    
    #stonith-sbd     (stonith:external/sbd): Started slesmsscl1
    # Resource Group: g-NW1_ASCS
    #     fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
    #     nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
    #     vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
    #     rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
    # Resource Group: g-NW1_ERS
    #     fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
    #     nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
    #     vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
    #     rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
    # Resource Group: g-NW2_ASCS
    #     fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
    #     nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
    #     vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
    #     rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
    # Resource Group: g-NW2_ERS
    #     fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
    #     nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
    #     vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
    #     rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
    # Resource Group: g-NW3_ASCS
    #     fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
    #     nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
    #     vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
    #     rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
    # Resource Group: g-NW3_ERS
    #     fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
    #     nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
    #     vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
    #     rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
    ```

   Aşağıdaki resim, SAP sistemi **NW2** kaynakları genişletilerek HA Web Konsole(Hawk) kaynakların nasıl görüneceğini göstermektedir.  

   [![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-suse/ha-suse-multi-sid-hawk.png)](./media/high-availability-guide-suse/ha-suse-multi-sid-hawk-detail.png#lightbox)

### <a name="proceed-with-the-sap-installation"></a>SAP yüklemesi ile devam edin 

SAP yüklemenizi şu şekilde tamamlayın:

* [SAP NetWeaver uygulama sunucularınızı hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse#2d6008b0-685d-426c-b59e-6cd281fd45d7)
* [DBMS örneğini yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse#install-database)
* [Birincil SAP uygulama sunucusu yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse#sap-netweaver-application-server-installation)
* Bir veya daha fazla ek SAP uygulama örneğinin yüklenmesi

## <a name="test-the-multi-sid-cluster-setup"></a>Multi-SID küme kurulumını test edin

Aşağıdaki testler, SUSE'nin en iyi uygulama kılavuzlarındaki test çalışmalarının bir alt kümesidir. Onlar sizin rahatınız için dahildir. Küme testlerinin tam listesi için aşağıdaki belgelere başvurun:

* Yüksek kullanılabilirlikteki NFS sunucusu kullanıyorsanız, [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)izleyin.  
* Azure NetApp Files NFS birimlerini kullanıyorsanız, [SAP uygulamaları için Azure NetApp Dosyaları ile SUSE Linux Enterprise Server'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) takip edin

SUSE en iyi uygulamalar kılavuzlarını her zaman okuyun ve ekilen tüm ek testleri gerçekleştirin.  
Sunulan testler, üç SAP sistemi yüklü iki düğüm, çok SID kümebulunmaktadır.  

1. Test HAGetFailoverConfig ve HACheckFailoverConfig

   ASCS örneğinin <sapsid>şu anda çalıştığı düğümde aşağıdaki komutları adm olarak çalıştırın. Komutlar FAIL: Yetersiz bellek ile başarısız olursa, ana bilgisayar adınızdaki tirelerden kaynaklanabilir. Bu bilinen bir sorundur ve sap-suse-cluster-konektör paketinde SUSE tarafından düzeltilecektir.

   ```
    slesmsscl1:nw1adm 57> sapcontrol -nr 00 -function HAGetFailoverConfig

   # 10.12.2019 21:33:08
   # HAGetFailoverConfig
   # OK
   # HAActive: TRUE
   # HAProductVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP4
   # HASAPInterfaceVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP4 (sap_suse_cluster_connector 3.1.0)
   # HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   # HAActiveNode: slesmsscl1
   # HANodes: slesmsscl1, slesmsscl2

    slesmsscl1:nw1adm 53> sapcontrol -nr 00 -function HACheckFailoverConfig

    # 19.12.2019 21:19:58
    # HACheckFailoverConfig
    # OK
    # state, category, description, comment
    # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch

    slesmsscl2:nw2adm 35> sapcontrol -nr 10 -function HAGetFailoverConfig

   # 10.12.2019 21:37:09
   # HAGetFailoverConfig
   # OK
   # HAActive: TRUE
   # HAProductVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP4
   # HASAPInterfaceVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP4 (sap_suse_cluster_connector 3.1.0)
   # HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   # HAActiveNode: slesmsscl2
   # HANodes: slesmsscl2, slesmsscl1

    slesmsscl2:nw2adm 52> sapcontrol -nr 10 -function HACheckFailoverConfig

    # 19.12.2019 21:17:39
    # HACheckFailoverConfig
    # OK
    # state, category, description, comment
    # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch

    slesmsscl1:nw3adm 49> sapcontrol -nr 20 -function HAGetFailoverConfig

   # 10.12.2019 23:35:36
   # HAGetFailoverConfig
   # OK
   # HAActive: TRUE
   # HAProductVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP4
   # HASAPInterfaceVersion: SUSE Linux Enterprise Server for SAP Applications 12 SP4 (sap_suse_cluster_connector 3.1.0)
   # HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   # HAActiveNode: slesmsscl1
   # HANodes: slesmsscl1, slesmsscl2

    slesmsscl1:nw3adm 52> sapcontrol -nr 20 -function HACheckFailoverConfig

    # 19.12.2019 21:10:42
    # HACheckFailoverConfig
    # OK
    # state, category, description, comment
    # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch
   ```

2. ASCS örneğini el ile geçirin. Örnek, SAP sistemi NW2 için ASCS örneğini geçirerek gösterir.  
   Kaynak durumu, test başlamadan önce:
   ```
    Full list of resources:
    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
   ```

   NW2 ASCS örneğini geçirmek için aşağıdaki komutları kök olarak çalıştırın.

   ```
    crm resource migrate rsc_sap_NW2_ASCS10 force
    # INFO: Move constraint created for rsc_sap_NW2_ASCS10
    
    crm resource unmigrate rsc_sap_NW2_ASCS10
   # INFO: Removed migration constraints for rsc_sap_NW2_ASCS10
   
   # Remove failed actions for the ERS that occurred as part of the migration
    crm resource cleanup rsc_sap_NW2_ERS12
   ```

   Testten sonra kaynak durumu:

   ```
    Full list of resources:
    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
   ```

1. Test HAFailoverToNode. Burada sunulan test, SAP sistemi NW2 için ASCS örneğini geçirerek gösterir.  

   Teste başlamadan önce kaynak durumu:

   ```
    Full list of resources:
    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
   ```

   NW2 ASCS örneğini geçirmek için aşağıdaki komutları **nw2**adm olarak çalıştırın.

   ```
    slesmsscl2:nw2adm 53> sapcontrol -nr 10 -host msnw2ascs -user nw2adm password -function HAFailoverToNode ""
   
   # run as root
   # Remove failed actions for the ERS that occurred as part of the migration
   crm resource cleanup rsc_sap_NW2_ERS12
   # Remove migration constraints
   crm resource clear rsc_sap_NW2_ASCS10
   #INFO: Removed migration constraints for rsc_sap_NW2_ASCS10
   ```

   Testten sonra kaynak durumu:

   ```
    Full list of resources:
    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
   ```

1. Düğüm çökmesini simüle edin

   Teste başlamadan önce kaynak durumu:

   ```
    Full list of resources:
    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
   ```

   Aşağıdaki komutu, en az bir ASCS örneğinin çalıştığı düğümde kök olarak çalıştırın. Bu örnekte, NW1 `slesmsscl2`ve NW3 için ASCS örneklerinin çalıştığı komutu uyguladık.  

   ```
    slesmsscl2:~ # echo b > /proc/sysrq-trigger
   ```

   SBD kullanıyorsanız, Pacemaker otomatik olarak öldürülen düğüm üzerinde başlamamalıdır. Düğüm yeniden başlatıldıktan sonraki durum aşağıdaki gibi görünmelidir.

   ```
    Online: [ slesmsscl1 ]
    OFFLINE: [ slesmsscl2 ]
    Full list of resources:

    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl1
    
    Failed Resource Actions:
    * rsc_sap_NW1_ERS02_monitor_11000 on slesmsscl1 'not running' (7): call=125, status=complete, exitreason='',
        last-rc-change='Fri Dec 13 19:32:10 2019', queued=0ms, exec=0ms
    * rsc_sap_NW2_ERS12_monitor_11000 on slesmsscl1 'not running' (7): call=126, status=complete, exitreason='',
        last-rc-change='Fri Dec 13 19:32:10 2019', queued=0ms, exec=0ms
    * rsc_sap_NW3_ERS22_monitor_11000 on slesmsscl1 'not running' (7): call=127, status=complete, exitreason='',
        last-rc-change='Fri Dec 13 19:32:10 2019', queued=0ms, exec=0ms
   ```

   Öldürülen düğümde Pacemaker'ı başlatmak, SBD iletilerini temizlemek ve başarısız kaynakları temizlemek için aşağıdaki komutları kullanın.

   ```# run as root
   # list the SBD device(s)
   slesmsscl2:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   slesmsscl2:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message slesmsscl2 clear
   
   slesmsscl2:~ # systemctl start pacemaker
   slesmsscl2:~ # crm resource cleanup rsc_sap_NW1_ERS02
   slesmsscl2:~ # crm resource cleanup rsc_sap_NW2_ERS12
   slesmsscl2:~ # crm resource cleanup rsc_sap_NW3_ERS22
   ```

   Testten sonra kaynak durumu:

   ```
    Full list of resources:
    stonith-sbd     (stonith:external/sbd): Started slesmsscl1
     Resource Group: g-NW1_ASCS
         fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW1_ERS
         fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW2_ASCS
         fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW2_ERS
         fs_NW2_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW2_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW2_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
     Resource Group: g-NW3_ASCS
         fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started slesmsscl1
         nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started slesmsscl1
         vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started slesmsscl1
         rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started slesmsscl1
     Resource Group: g-NW3_ERS
         fs_NW3_ERS (ocf::heartbeat:Filesystem):    Started slesmsscl2
         nc_NW3_ERS (ocf::heartbeat:azure-lb):      Started slesmsscl2
         vip_NW3_ERS        (ocf::heartbeat:IPaddr2):       Started slesmsscl2
         rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started slesmsscl2
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
