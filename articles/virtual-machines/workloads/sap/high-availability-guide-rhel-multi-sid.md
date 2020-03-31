---
title: RHEL multi-SID kılavuzunda SAP NW için Azure VM'ler yüksek kullanılabilirlik | Microsoft Dokümanlar
description: Red Hat Enterprise Linux'ta SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/24/2020
ms.author: radeltch
ms.openlocfilehash: 4f1bfd58e27f0cd677980ff9351d32d91a68e3e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247444"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-red-hat-enterprise-linux-for-sap-applications-multi-sid-guide"></a>SAP uygulamaları multi-SID kılavuzu için Red Hat Enterprise Linux'ta Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md
[glusterfs-ha]:high-availability-guide-rhel-glusterfs.md

Bu makalede, SAP uygulamaları için Red Hat Enterprise Linux ile Azure VM'lerde iki düğüm kümesinde birden çok SAP NetWeaver yüksek kullanılabilir sistem (yani multi-SID) nasıl dağıtılanın açıklanmaktadır.  

Örnek yapılandırmalarda, kurulum komutları vb üç SAP NetWeaver 7.50 sistemleri tek, iki düğüm yüksek kullanılabilirlik kümedağıtılır. SAP sistemleri SIT'ler şunlardır:
* **NW1**: ASCS örnek numarası **00** ve sanal ana bilgisayar adı **msnw1ascs**; ERS örnek numarası **02** ve sanal ana bilgisayar adı **msnw1ers**.  
* **NW2**: ASCS örnek numarası **10** ve sanal hostname **msnw2ascs**; ERS örnek numarası **12** ve sanal ana bilgisayar adı **msnw2ers**.  
* **NW3**: ASCS örnek numarası **20** ve sanal hostname **msnw3ascs**; ERS örnek numarası **22** ve sanal ana bilgisayar adı **msnw3ers**.  

Makale veritabanı katmanını ve SAP NFS paylaşımlarının dağıtımını kapsamaz. Bu makaledeki örneklerde, nfs paylaşımları için [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes) volume **sapMSID'i** kullanıyoruz, hacmin zaten dağıtıldığını varsayarak. Ayrıca, Azure NetApp Dosyaları hacminin NFSv3 protokolü ile dağıtılmış olduğunu ve SAP sistemleri NW1, NW2 ve NW3'ün ASCS ve ERS örnekleri için küme kaynakları için aşağıdaki dosya yollarının bulunduğunu varsayıyoruz:  

* hacim sapMSID (nfs://10.42.0.4/sapmnt<b>NW1</b>)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW1</b>ascs)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW1</b>sys)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW1</b>ers)
* hacim sapMSID (nfs://10.42.0.4/sapmnt<b>NW2</b>)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW2</b>ascs)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW2</b>sys)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW2</b>ers)
* hacim sapMSID (nfs://10.42.0.4/sapmnt<b>NW3</b>)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW3</b>ascs)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW3</b>sys)
* hacim sapMSID (nfs://10.42.0.4/usrsap<b>NW3</b>ers)

Başlamadan önce aşağıdaki SAP Notları ve kağıtlarına bakın:

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutları listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü
* [Azure NetApp Dosyaları belgeleri][anf-azure-doc]
* SAP Note [2015553,] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2002167] Red Hat Enterprise Linux için işletim sistemi ayarlarını tavsiye etti
* SAP Not [2009879] Red Hat Enterprise Linux için SAP HANA Yönergeleri vardır
* SAP Note [2178632,] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [2191498,] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1999351,] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notları'na sahiptir.
* [Linux'ta SAP için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [Linux'ta SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [SAP Netweaver kalp pili kümesinde](https://access.redhat.com/articles/3150081)
* Genel RHEL belgeleri
  * [Yüksek Kullanılabilirlik Eklentisi Genel Bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek Kullanılabilirlik Eklenti Yönetimi](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek Kullanılabilirlik Eklenti Başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [RHEL 7.5'te bağımsız kaynaklarla SAP Netweaver için ASCS/ERS yapılandırması](https://access.redhat.com/articles/3569681)
  * [RHEL'de Pacemaker'da SAP S/4HANA ASCS/ERS'i Tek Başına Enqueue Server 2 (ENSA2) ile yapılandırın](https://access.redhat.com/articles/3974941)
* Azure'a özel RHEL belgeleri:
  * [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - Küme Üyesi Olarak Microsoft Azure Sanal Makineleri](https://access.redhat.com/articles/3131341)
  * [Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve sonrası) Yüksek Kullanılabilirlik Kümesi'ni yükleme ve yapılandırma](https://access.redhat.com/articles/3252491)
* [Azure NetApp Dosyalarını Kullanarak Microsoft Azure'daki NetApp SAP Uygulamaları][anf-sap-applications-azure]

## <a name="overview"></a>Genel Bakış

Kümeye katılan sanal makinelerin, başarısız olması durumunda tüm kaynakları çalıştırabilmek için boyutlandırılmaları gerekir. Her SAP SID, çoklu SID yüksek kullanılabilirlik kümesinde birbirinden bağımsız olarak başarısız olabilir.  

Yüksek kullanılabilirlik elde etmek için SAP NetWeaver yüksek kullanılabilirlik hisse gerektirir. Bu dokümantasyonda, [Azure NetApp Files NFS birimlerinde](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)dağıtılan SAP paylaşımları ile örnekler savuruyoruz. Ayrıca, birden fazla SAP sistemi tarafından kullanılabilen yüksek kullanılabilir [GlusterFS kümesindeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)hisseleri barındırmak da mümkündür.  

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-rhel/ha-rhel-multi-sid.png)

> [!IMPORTANT]
> Azure VM'lerde konuk işletim sistemi olarak Red Hat Linux ile SAP ASCS/ERS'in çoklu SID kümelemesi desteği, aynı kümedeki **beş** SAP SID ile sınırlıdır. Her yeni SID karmaşıklığı artırır. Aynı kümedeki SAP Enqueue Replication Server 1 ve Enqueue Replication Server 2'nin bir karışımı **desteklenmez.** Multi-SID kümeleme, tek bir Pacemaker kümesinde farklı SID'ler içeren birden çok SAP ASCS/ERS örneğinin yüklenmesini açıklar. Şu anda çoklu SID kümeleme yalnızca ASCS/ERS için desteklenir.  

> [!TIP]
> SAP ASCS/ERS'in çoklu SID kümelemesi daha yüksek karmaşıklığa sahip bir çözümdür. Bu uygulamak için daha karmaşıktır. Ayrıca, bakım faaliyetleri (işletim sistemi yama gibi) yürütüldüğünde daha yüksek yönetim çabası içerir. Gerçek uygulamayı başlatmadan önce, dağıtımı ve VM'ler, NFS yuvaları, VIP'ler, yük dengeleyici yapılandırmaları gibi tüm ilgili bileşenleri dikkatle planlamak için zaman ayırın.  

SAP NetWeaver ASCS, SAP NetWeaver SCS ve SAP NetWeaver ERS sanal hostname ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. [Standart yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)kullanmanızı öneririz.  

Aşağıdaki liste, üç SAP sistemi olan bu çok SID küme örneği için (A)SCS ve ERS yük dengeleyicisinin yapılandırmasını gösterir. Her bir SAND örneği için ayrı ön uç IP'si, sistem durumu sondaları ve yük dengeleme kurallarına ihtiyacınız olacaktır. ASCS/ASCS kümesinin bir parçası olan tüm VM'leri tek bir ILB'nin bir arka uç havuzuna atayın.  

### <a name="ascs"></a>(A) Scs

* Frontend yapılandırması
  * NW1 için IP adresi: 10.3.1.50
  * NW2 için IP adresi: 10.3.1.52
  * NW3 için IP adresi: 10.3.1.54

* Sonda Bağlantı Noktaları
  * Port 620<strong>&lt;&gt;nr,</strong>bu nedenle NW1, NW2 ve NW3 prob bağlantı noktaları için 620**00**, 620**10** ve 620**20**
* Yük dengeleme kuralları - her örnek için bir tane oluşturun, yani NW1/ASCS, NW2/ASCS ve NW3/ASCS.
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
  * NW1 10.3.1.51 için IP adresi
  * NW2 10.3.1.53 için IP adresi
  * NW3 10.3.1.55 için IP adresi

* Sonda Bağlantı Noktası
  * Port 621<strong>&lt;&gt;nr,</strong>bu nedenle NW1, NW2 ve N3 prob bağlantı noktaları için 621**02**, 621**12** ve 621**22**
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

## <a name="sap-shares"></a>SAP hisseleri

SAP NetWeaver, taşıma, profil dizini ve benzeri için paylaşılan depolama gerektirir. Yüksek kullanılabilir SAP sistemi için, yüksek kullanılabilir hisselere sahip olmak önemlidir. SAP hisselerinizin mimarisine karar vermeniz gerekir. Bir seçenek [Azure NetApp Files NFS hacimleri](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)üzerinde hisse dağıtmak için.  Azure NetApp Dosyaları ile SAP NFS paylaşımları için yerleşik yüksek kullanılabilirlik elde elabilirsiniz.

Başka bir seçenek, birden çok SAP sistemleri arasında paylaşılabilir [SAP NetWeaver için Red Hat Enterprise Linux Üzerinde Azure VMs GlusterFS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)oluşturmaktır. 

## <a name="deploy-the-first-sap-system-in-the-cluster"></a>Kümedeki ilk SAP sistemini dağıtma

SAP hisselerinin mimarisine karar verdiğiniz için, ilgili belgeleri izleyerek kümedeki ilk SAP sistemini dağıtın.

* Azure NetApp Files NFS birimlerini kullanıyorsanız, [SAP uygulamaları için Azure NetApp Dosyaları ile Red Hat Enterprise Linux'ta SAP NetWeaver için Azure VM'leri yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) izleyin  
* GlusterFS kümesini kullanıyorsanız, [SAP NetWeaver için Red Hat Enterprise Linux'ta Azure VM'lerde GlusterFS'ı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)izleyin.  

Yukarıda listelenen belgeler, gerekli altyapıyı hazırlamak, küme oluşturmak, SAP uygulamasını çalıştırmak için işletim sistemi hazırlamak için gereken adımlar boyunca size rehberlik edecektir.  

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
* Küme failover işlevselliği sınanmıştır.  
* Tüm SAP sistemleri için NFS hisseleri dağıtılır.  

### <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver Kurulumu için hazırlanın

1. Azure portalı üzerinden Azure [Yük Bakiyesini el ile dağıt'ı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files#deploy-linux-manually-via-azure-portal)uygulayın yönergeleri izleyerek, mevcut Azure Yük Dengeleyicisine yeni dağıtılan sistem için yapılandırma (diğer bir şekilde **NW2**, **NW3)** ekleyin. Yapılandırmanız için IP adreslerini, sistem durumu sonda bağlantı noktalarını, yük dengeleme kurallarını ayarlayın.  

2. **[A]** Ek SAP sistemleri için kurulum adı çözümlemesi. DNS sunucusu'nı kullanabilir `/etc/hosts` veya tüm düğümlerde değişiklik yapabilirsiniz. Bu örnek, dosyanın `/etc/hosts` nasıl kullanılacağını gösterir.  IP adreslerini ve ana bilgisayar adlarını ortamınıza uyarlayın. 

    ```
    sudo vi /etc/hosts
    # IP address of the load balancer frontend configuration for NW2 ASCS
    10.3.1.52 msnw2ascs
    # IP address of the load balancer frontend configuration for NW3 ASCS
    10.3.1.54 msnw3ascs
    # IP address of the load balancer frontend configuration for NW2 ERS
    10.3.1.53 msnw2ers
    # IP address of the load balancer frontend configuration for NW3 ERS
    10.3.1.55 msnw3ers
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

4. **[A]** Kümeye dağıttığınız ek SAP sistemleri için /sapmnt/SID ve /usr/sap/SID/SID/SYS dosya sistemlerinin montaj girişlerini ekleyin. Bu örnekte **NW2** ve **NW3**.  

   Kümeye `/etc/fstab` dağıttığınız ek SAP sistemleri için dosyayı dosyayı güncelleştirin.  

   * Azure NetApp Dosyalarını kullanıyorsanız, [aşağıdaki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files#prepare-for-sap-netweaver-installation) talimatları izleyin  
   * GlusterFS kümesi kullanıyorsanız, [aşağıdaki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel#prepare-for-sap-netweaver-installation) talimatları izleyin  

### <a name="install-ascs--ers"></a>ASCS / ERS'i yükleyin

1. Kümeye dağıttığınız ek SAP sistemlerinin ASCS örnekleri için sanal IP ve sistem durumu sonda kümesi kaynaklarını oluşturun. Burada gösterilen örnek, NFSv3 protokolü ile Azure NetApp Dosyaları birimlerinde NFS kullanan **NW2** ve **NW3** ASCS içindir.  

    ```
    sudo pcs resource create fs_NW2_ASCS Filesystem device='10.42.0.4:/sapMSIDR/usrsapNW2ascs' \
    directory='/usr/sap/NW2/ASCS10' fstype='nfs' force_unmount=safe \
    op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
     --group g-NW2_ASCS

    sudo pcs resource create vip_NW2_ASCS IPaddr2 \
    ip=10.3.1.52 cidr_netmask=24 \
     --group g-NW2_ASCS
    
    sudo pcs resource create nc_NW2_ASCS azure-lb port=62010 \
     --group g-NW2_ASCS

    sudo pcs resource create fs_NW3_ASCS Filesystem device='10.42.0.4:/sapMSIDR/usrsapNW3ascs' \
    directory='/usr/sap/NW3/ASCS20' fstype='nfs' force_unmount=safe \
    op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
    --group g-NW3_ASCS

    sudo pcs resource create vip_NW3_ASCS IPaddr2 \
    ip=10.3.1.54 cidr_netmask=24 \
    --group g-NW3_ASCS

    sudo pcs resource create nc_NW3_ASCS azure-lb port=62020 \
    --group g-NW3_ASCS
    ```

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.  

2. **[1]** SAP NetWeaver ASCS yükleyin  

   SAP NetWeaver ASCS'yi, ASCS için yük dengeleyici ön uç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adı kullanarak kök olarak yükleyin. Örneğin, sistem **NW2**için, sanal ana bilgisayar adı <b>msnw2ascs</b>, <b>10.3.1.52</b> ve yük dengeleyici, örneğin <b>10</b>sonda sı için kullanılan örnek numarasıdır. Sistem **NW3**için, sanal hostname <b>msnw3ascs</b>, <b>10.3.1.54</b> ve yük dengeleyici, örneğin <b>20</b>prob için kullanılan örnek numarasıdır. Her SAP SID için ASCS yüklediğiniz küme düğümüne not edin.  

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz. Sanal ana bilgisayar adını kullanarak SAP'yi yüklemek için parametre SAPINST_USE_HOSTNAME kullanabilirsiniz.  

    ```
    # Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again
    sudo firewall-cmd --zone=public --add-port=4237/tcp
    sudo swpm/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=virtual_hostname
    ```

   Yükleme /usr/sap/**SID**/ASCS**Instance#** bir alt klasör oluşturamazsa, sahibini **SID**Adm'e ayarlamayı deneyin ve ASCS**Instance#** ve yeniden deneyin.

3. **[1]** Kümeye dağıttığınız ek SAP sisteminin ERS örneği için sanal bir IP ve sistem sondası kümesi kaynakları oluşturun. Burada gösterilen örnek, NFSv3 protokolü ile Azure NetApp Dosyaları birimlerinde NFS kullanan **NW2** ve **NW3** ERS içindir.  

    ```
    sudo pcs resource create fs_NW2_AERS Filesystem device='10.42.0.4:/sapMSIDR/usrsapNW2ers' \
    directory='/usr/sap/NW2/ERS12' fstype='nfs' force_unmount=safe \
    op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
     --group g-NW2_AERS

    sudo pcs resource create vip_NW2_AERS IPaddr2 \
    ip=10.3.1.53 cidr_netmask=24 \
     --group g-NW2_AERS

    sudo pcs resource create nc_NW2_AERS azure-lb port=62112 \
     --group g-NW2_AERS

    sudo pcs resource create fs_NW3_AERS Filesystem device='10.42.0.4:/sapMSIDR/usrsapNW3ers' \
    directory='/usr/sap/NW3/ERS22' fstype='nfs' force_unmount=safe \
    op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
     --group g-NW3_AERS

    sudo pcs resource create vip_NW3_AERS IPaddr2 \
    ip=10.3.1.55 cidr_netmask=24 \
     --group g-NW3_AERS

    sudo pcs resource create nc_NW3_AERS azure-lb port=62122 \
     --group g-NW3_AERS
   ```

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun.  

   Ardından, yeni oluşturulan ERS grubunun kaynaklarının küme düğümünde, aynı SAP sistemi için ASCS örneğinin yüklendiği küme düğümünün karşısında çalıştığından emin olun.  Örneğin, NW2 ASCS'nin üzerine `rhelmsscl1`yüklenmişse, NW2 ERS `rhelmsscl2`grubunun üzerinde çalışıyorduklarından emin olun.  Gruptaki küme kaynaklarından biri için `rhelmsscl2` aşağıdaki komutu çalıştırarak NW2 ERS grubunu geçirebilirsiniz: 

    ```
      pcs resource move fs_NW2_AERS rhelmsscl2
    ```

4. **[2]** SAP NetWeaver ERS'i yükleyin

   ERS için yük dengeleyici ön uç yapılandırmasının IP adresine eşlenen sanal bir ana bilgisayar adı kullanarak SAP NetWeaver ERS'i diğer düğüme kök olarak yükleyin. Örneğin sistem **NW2**için, sanal ana bilgisayar adı <b>msnw2ers</b>olacak , <b>10.3.1.53</b> ve yük dengeleyici sondası için kullandığınız örnek numarası, örneğin <b>12</b>. Sistem **NW3**için , sanal ana bilgisayar adı <b>msnw3ers</b>, <b>10.3.1.55</b> ve yük dengeleyici sondası için kullanılan örnek numarası, örneğin <b>22</b>. 

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz. Sanal ana bilgisayar adını kullanarak SAP'yi yüklemek için parametre SAPINST_USE_HOSTNAME kullanabilirsiniz.  

    ```
    # Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again
    sudo firewall-cmd --zone=public --add-port=4237/tcp
    sudo swpm/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=virtual_hostname
    ```

   > [!NOTE]
   > SWPM SP 20 PL 05 veya daha yüksek kullanın. Alt sürümler izinleri doğru ayarlamaz ve yükleme başarısız olur.

   Yükleme /usr/sap/**NW2**/ERS**Instance#** bir alt klasör oluşturamazsa, **sahibisid**adm ve grup ERS**Instance#** klasörünün sapsys ve yeniden deneyin sahibi ayarlamayı deneyin.

   Yeni dağıtılan SAP sisteminin ERS grubunu farklı bir küme düğümüne geçirmeniz gerekliyse, ERS grubunun konum kısıtlamasını kaldırmayı unutmayın. Aşağıdaki komutu çalıştırarak kısıtlamayı kaldırabilirsiniz (örnek SAP sistemleri **NW2** ve **NW3**için verilir). ERS küme grubunu taşımak için komutta kullandığınız aynı kaynak için geçici kısıtlamaları kaldırdığınızdan emin olun.

    ```
      pcs resource clear fs_NW2_AERS
      pcs resource clear fs_NW3_AERS
    ```

5. **[1]** Yeni yüklenen SAP sistemi(ler) için ASCS/SCS ve ERS örnek profillerini uyarlayın. Aşağıda gösterilen örnek NW2 içindir. Kümeye eklenen tüm SAP örnekleri için ASCS/SCS ve ERS profillerini uyarlamanız gerekir.  
 
   * ASCS/SCS profili

      ```
      sudo vi /sapmnt/NW2/profile/NW2_ASCS10_msnw2ascs
   
      # Change the restart command to a start command
      #Restart_Program_01 = local $(_EN) pf=$(_PF)
      Start_Program_01 = local $(_EN) pf=$(_PF)

      # Add the keep alive parameter
      enque/encni/set_so_keepalive = true
      ```

   * ERS profili

      ```
      sudo vi /sapmnt/NW2/profile/NW2_ERS12_msnw2ers
   
      # Change the restart command to a start command
      #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
      Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
      # remove Autostart from ERS profile
      # Autostart = 1
      ```

6. **[A]** /usr/sap/sapservices dosyasını güncelleştir

   Sapinit başlangıç komut dosyası tarafından örneklerin başlatılmasını önlemek için, Pacemaker tarafından `/usr/sap/sapservices` yönetilen tüm örneklerin dosyadan yorumlanmalıdır.  Aşağıda gösterilen örnek SAP sistemleri **NW2** ve **NW3**içindir.  

   ```
    # On the node where ASCS was installed, comment out the line for the ASCS instacnes
    #LD_LIBRARY_PATH=/usr/sap/NW2/ASCS10/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/NW2/ASCS10/exe/sapstartsrv pf=/usr/sap/NW2/SYS/profile/NW2_ASCS10_msnw2ascs -D -u nw2adm
    #LD_LIBRARY_PATH=/usr/sap/NW3/ASCS20/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/NW3/ASCS20/exe/sapstartsrv pf=/usr/sap/NW3/SYS/profile/NW3_ASCS20_msnw3ascs -D -u nw3adm

    # On the node where ERS was installed, comment out the line for the ERS instacnes
    #LD_LIBRARY_PATH=/usr/sap/NW2/ERS12/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/NW2/ERS12/exe/sapstartsrv pf=/usr/sap/NW2/ERS12/profile/NW2_ERS12_msnw2ers -D -u nw2adm
    #LD_LIBRARY_PATH=/usr/sap/NW3/ERS22/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/NW3/ERS22/exe/sapstartsrv pf=/usr/sap/NW3/ERS22/profile/NW3_ERS22_msnw3ers -D -u nw3adm
   ```

7. **[1]** Yeni yüklenen SAP sistemi için SAP küme kaynaklarını oluşturun.  

   Enqueue server 1 mimarisi (ENSA1) kullanıyorsanız, SAP sistemleri **NW2** ve **NW3** kaynaklarını aşağıdaki gibi tanımlayın:

    ```
     sudo pcs property set maintenance-mode=true

    sudo pcs resource create rsc_sap_NW2_ASCS10 SAPInstance \
    InstanceName=NW2_ASCS10_msnw2ascs START_PROFILE="/sapmnt/NW2/profile/NW2_ASCS10_msnw2ascs" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW2_ASCS

    sudo pcs resource create rsc_sap_NW2_ERS12 SAPInstance \
    InstanceName=NW2_ERS12_msnw2ers START_PROFILE="/sapmnt/NW2/profile/NW2_ERS12_msnw2ers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW2_AERS

    sudo pcs constraint colocation add g-NW2_AERS with g-NW2_ASCS -5000
    sudo pcs constraint location rsc_sap_NW2_ASCS10 rule score=2000 runs_ers_NW2 eq 1
    sudo pcs constraint order g-NW2_ASCS then g-NW2_AERS kind=Optional symmetrical=false

    sudo pcs resource create rsc_sap_NW3_ASCS20 SAPInstance \
    InstanceName=NW3_ASCS20_msnw3ascs START_PROFILE="/sapmnt/NW3/profile/NW3_ASCS20_msnw3ascs" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW3_ASCS

    sudo pcs resource create rsc_sap_NW3_ERS22 SAPInstance \
    InstanceName=NW3_ERS22_msnw3ers START_PROFILE="/sapmnt/NW3/profile/NW2_ERS22_msnw3ers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW3_AERS

    sudo pcs constraint colocation add g-NW3_AERS with g-NW3_ASCS -5000
    sudo pcs constraint location rsc_sap_NW3_ASCS20 rule score=2000 runs_ers_NW3 eq 1
    sudo pcs constraint order g-NW3_ASCS then g-NW3_AERS kind=Optional symmetrical=false

    sudo pcs property set maintenance-mode=false
    ```

   SAP, SAP NW 7.52 itibariyle çoğaltma da dahil olmak üzere enqueue server 2 için destek sundu. ABAP Platform 1809 ile başlayarak, enqueue server 2 varsayılan olarak yüklenir. Enqueue server 2 desteği için SAP note [2630416'ya](https://launchpad.support.sap.com/#/notes/2630416) bakın.
   Enqueue server 2 mimarisi[(ENSA2)](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)kullanıyorsanız, SAP sistemleri **NW2 ve NW3** kaynaklarını aşağıdaki gibi tanımlayın: **NW3**

    ```
     sudo pcs property set maintenance-mode=true

    sudo pcs resource create rsc_sap_NW2_ASCS10 SAPInstance \
    InstanceName=NW2_ASCS10_msnw2ascs START_PROFILE="/sapmnt/NW2/profile/NW2_ASCS10_msnw2ascs" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW2_ASCS

    sudo pcs resource create rsc_sap_NW2_ERS12 SAPInstance \
    InstanceName=NW2_ERS12_msnw2ers START_PROFILE="/sapmnt/NW2/profile/NW2_ERS12_msnw2ers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW2_AERS

    sudo pcs constraint colocation add g-NW2_AERS with g-NW2_ASCS -5000
    sudo pcs constraint order g-NW2_ASCS then g-NW2_AERS kind=Optional symmetrical=false
    sudo pcs constraint order start g-NW2_ASCS then stop g-NW2_AERS symmetrical=false

    sudo pcs resource create rsc_sap_NW3_ASCS20 SAPInstance \
    InstanceName=NW3_ASCS20_msnw3ascs START_PROFILE="/sapmnt/NW3/profile/NW3_ASCS20_msnw3ascs" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW3_ASCS

    sudo pcs resource create rsc_sap_NW3_ERS22 SAPInstance \
    InstanceName=NW3_ERS22_msnw3ers START_PROFILE="/sapmnt/NW3/profile/NW2_ERS22_msnw3ers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
     --group g-NW3_AERS

    sudo pcs constraint colocation add g-NW3_AERS with g-NW3_ASCS -5000
    sudo pcs constraint order g-NW3_ASCS then g-NW3_AERS kind=Optional symmetrical=false
    sudo pcs constraint order start g-NW3_ASCS then stop g-NW3_AERS symmetrical=false

    sudo pcs property set maintenance-mode=false
    ```

   Eski bir sürümden yükseltiyor ve enqueue server 2'ye geçiyorsanız, SAP note [2641019'a](https://launchpad.support.sap.com/#/notes/2641019)bakın. 

   > [!NOTE]
   > Yukarıdaki yapılandırmadaki zaman ekmeleri yalnızca örneklerdir ve belirli SAP kurulumuna uyarlanmış olması gerekebilir. 

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.
   Aşağıdaki örnek, SAP sistemleri **NW2** ve **NW3** kümeye eklendikten sonra küme kaynakları durumunu gösterir. 

    ```
     sudo pcs status

    Online: [ rhelmsscl1 rhelmsscl2 ]

    Full list of resources:

    rsc_st_azure   (stonith:fence_azure_arm):      Started rhelmsscl1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_ASCS
        fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW2_AERS
        fs_NW2_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW2_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW2_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_ASCS
        fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_AERS
        fs_NW3_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW3_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW3_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    ```

8. **[A]** Her iki düğüme DE ASCS ve ERS için güvenlik duvarı kuralları ekleyin.  Aşağıdaki örnek, hem SAP sistemleri **NW2** hem de **NW3**için güvenlik duvarı kurallarını gösterir.  

   ```
    # NW2 - ASCS
    sudo firewall-cmd --zone=public --add-port=62010/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=62010/tcp
    sudo firewall-cmd --zone=public --add-port=3210/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3210/tcp
    sudo firewall-cmd --zone=public --add-port=3610/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3610/tcp
    sudo firewall-cmd --zone=public --add-port=3910/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3910/tcp
    sudo firewall-cmd --zone=public --add-port=8110/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=8110/tcp
    sudo firewall-cmd --zone=public --add-port=51013/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=51013/tcp
    sudo firewall-cmd --zone=public --add-port=51014/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=51014/tcp
    sudo firewall-cmd --zone=public --add-port=51016/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=51016/tcp
    # NW2 - ERS
    sudo firewall-cmd --zone=public --add-port=62112/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=62112/tcp
    sudo firewall-cmd --zone=public --add-port=3312/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3312/tcp
    sudo firewall-cmd --zone=public --add-port=51213/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=51213/tcp
    sudo firewall-cmd --zone=public --add-port=51214/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=51214/tcp
    sudo firewall-cmd --zone=public --add-port=51216/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=51216/tcp
    # NW3 - ASCS
    sudo firewall-cmd --zone=public --add-port=62020/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=62020/tcp
    sudo firewall-cmd --zone=public --add-port=3220/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3220/tcp
    sudo firewall-cmd --zone=public --add-port=3620/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3620/tcp
    sudo firewall-cmd --zone=public --add-port=3920/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3920/tcp
    sudo firewall-cmd --zone=public --add-port=8120/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=8120/tcp
    sudo firewall-cmd --zone=public --add-port=52013/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=52013/tcp
    sudo firewall-cmd --zone=public --add-port=52014/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=52014/tcp
    sudo firewall-cmd --zone=public --add-port=52016/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=52016/tcp
    # NW3 - ERS
    sudo firewall-cmd --zone=public --add-port=62122/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=62122/tcp
    sudo firewall-cmd --zone=public --add-port=3322/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=3322/tcp
    sudo firewall-cmd --zone=public --add-port=52213/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=52213/tcp
    sudo firewall-cmd --zone=public --add-port=52214/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=52214/tcp
    sudo firewall-cmd --zone=public --add-port=52216/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=52216/tcp
   ```

### <a name="proceed-with-the-sap-installation"></a>SAP yüklemesi ile devam edin 

SAP yüklemenizi şu şekilde tamamlayın:

* [SAP NetWeaver uygulama sunucularınızı hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files#2d6008b0-685d-426c-b59e-6cd281fd45d7)
* [DBMS örneğini yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files#install-database)
* [Birincil SAP uygulama sunucusu yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files#sap-netweaver-application-server-installation)
* Bir veya daha fazla ek SAP uygulama örneğinin yüklenmesi

## <a name="test-the-multi-sid-cluster-setup"></a>Multi-SID küme kurulumını test edin

Aşağıdaki testler, Red Hat'in en iyi uygulama kılavuzlarındaki test çalışmalarının bir alt kümesidir. Onlar sizin rahatınız için dahildir. Küme testlerinin tam listesi için aşağıdaki belgelere başvurun:

* Azure NetApp Files NFS birimlerini kullanıyorsanız, [SAP uygulamaları için Azure NetApp Dosyaları ile RHEL'de SAP NetWeaver için Azure VM'leri yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) izleyin
* Yüksek kullanılabilirlik `GlusterFS`kullanıyorsanız, [SAP uygulamaları için RHEL'de SAP NetWeaver için Azure VM'leri yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)izleyin.  

Her zaman Red Hat en iyi uygulamalar kılavuzlarını okuyun ve ekilen tüm ek testleri gerçekleştirin.  
Sunulan testler, üç SAP sistemi yüklü iki düğüm, çok SID kümebulunmaktadır.  

1. ASCS örneğini el ile geçirin. Örnek, SAP sistemi NW3 için ASCS örneğini geçirerek gösterir.

   Teste başlamadan önce kaynak durumu:

   ```
    Online: [ rhelmsscl1 rhelmsscl2 ]

    Full list of resources:

    rsc_st_azure   (stonith:fence_azure_arm):      Started rhelmsscl1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_ASCS
        fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_AERS
        fs_NW2_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW2_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW2_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_ASCS
        fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW3_AERS
        fs_NW3_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW3_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW3_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
   ```

   NW3 ASCS örneğini geçirmek için aşağıdaki komutları kök olarak çalıştırın.

   ```
    pcs resource move rsc_sap_NW3_ASCS200
    # Clear temporary migration constraints
    pcs resource clear rsc_sap_NW3_ASCS20

    # Remove failed actions for the ERS that occurred as part of the migration
    pcs resource cleanup rsc_sap_NW3_ERS22
   ```

   Testten sonra kaynak durumu:

   ```
    Online: [ rhelmsscl1 rhelmsscl2 ]

    Full list of resources:

    rsc_st_azure   (stonith:fence_azure_arm):      Started rhelmsscl1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_ASCS
        fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_AERS
        fs_NW2_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW2_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW2_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_ASCS
        fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_AERS
        fs_NW3_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW3_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW3_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
   ```

1. Düğüm çökmesini simüle edin

   Teste başlamadan önce kaynak durumu:

   ```
    Online: [ rhelmsscl1 rhelmsscl2 ]

    Full list of resources:

    rsc_st_azure   (stonith:fence_azure_arm):      Started rhelmsscl1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_ASCS
        fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW2_AERS
        fs_NW2_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW2_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW2_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW3_ASCS
        fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_AERS
        fs_NW3_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW3_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW3_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
   ```

   Aşağıdaki komutu, en az bir ASCS örneğinin çalıştığı bir düğümüzerinde kök olarak çalıştırın. Bu örnekte, `rhelmsscl1`NW1, NW2 ve NW3 için ASCS örneklerinin çalıştığı komutu uyguladık.  

   ```
   echo c > /proc/sysrq-trigger
   ```

   Testten sonraki durum ve kilitlenen düğüm yeniden başladıktan sonra durum şu şekilde görünmelidir.

   ```
    Full list of resources:

    rsc_st_azure    (stonith:fence_azure_arm):      Started rhelmsscl2
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW2_ASCS
        fs_NW2_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW2_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW2_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW2_ASCS10 (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW2_AERS
        fs_NW2_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW2_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW2_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW2_ERS12  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
    Resource Group: g-NW3_ASCS
        fs_NW3_ASCS        (ocf::heartbeat:Filesystem):    Started rhelmsscl2
        vip_NW3_ASCS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl2
        nc_NW3_ASCS        (ocf::heartbeat:azure-lb):      Started rhelmsscl2
        rsc_sap_NW3_ASCS20 (ocf::heartbeat:SAPInstance):   Started rhelmsscl2
    Resource Group: g-NW3_AERS
        fs_NW3_AERS        (ocf::heartbeat:Filesystem):    Started rhelmsscl1
        vip_NW3_AERS       (ocf::heartbeat:IPaddr2):       Started rhelmsscl1
        nc_NW3_AERS        (ocf::heartbeat:azure-lb):      Started rhelmsscl1
        rsc_sap_NW3_ERS22  (ocf::heartbeat:SAPInstance):   Started rhelmsscl1
   ```

   Başarısız kaynaklar için iletiler varsa, başarısız kaynakların durumunu temizleyin. Örnek:

   ```
   pcs resource cleanup rsc_sap_NW1_ERS02
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
