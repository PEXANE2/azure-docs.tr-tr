---
title: RHEL'de SAP NW için Azure VM'ler yüksek kullanılabilirlik | Microsoft Dokümanlar
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
ms.date: 03/26/2020
ms.author: radeltch
ms.openlocfilehash: 73b958149d9d6d907785fe1c2c56b8198bb91f70
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351096"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux"></a>Red Hat Enterprise Linux'ta SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

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

Bu makalede, sanal makinelerin nasıl dağıtılancaör, sanal makineleri yapılandırma, küme çerçevesini yükleme ve yüksek kullanılabilir sap NetWeaver 7.50 sistemi nasıl yüklenir.
Örnek yapılandırmalarda, yükleme komutlarında vb. ASCS örnek numarası 00, ERS örnek numarası 02 ve SAP System ID NW1 kullanılır. Örnekteki kaynakların adları (örneğin sanal makineler, sanal ağlar) kaynakları oluşturmak için Kaynak Öneki NW1 ile [ASCS/SCS şablonu][template-multisid-xscs] kullandığınızı varsayar.

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutları listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü

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
* [Red Hat Gluster Depolama için Ürün Dokümantasyonu](https://access.redhat.com/documentation/red_hat_gluster_storage/)
* [SAP Netweaver kalp pili kümesinde](https://access.redhat.com/articles/3150081)
* Genel RHEL belgeleri
  * [Yüksek Kullanılabilirlik Eklentisi Genel Bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek Kullanılabilirlik Eklenti Yönetimi](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek Kullanılabilirlik Eklenti Başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [RHEL 7.5'te bağımsız kaynaklarla SAP Netweaver için ASCS/ERS yapılandırması](https://access.redhat.com/articles/3569681)
  * [RHEL'de Pacemaker'da SAP S/4HANA ASCS/ERS'i Tek Başına Enqueue Server 2 (ENSA2) ile yapılandırın](https://access.redhat.com/articles/3974941)
* Azure'a özgü RHEL belgeleri:
  * [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - Küme Üyesi Olarak Microsoft Azure Sanal Makineleri](https://access.redhat.com/articles/3131341)
  * [Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve sonrası) Yüksek Kullanılabilirlik Kümesi'ni yükleme ve yapılandırma](https://access.redhat.com/articles/3252491)

## <a name="overview"></a>Genel Bakış

Sap NetWeaver, yüksek kullanılabilirlik elde etmek için paylaşılan depolama yı gerektirir. GlusterFS ayrı bir kümede yapılandırılmıştır ve birden çok SAP sistemi tarafından kullanılabilir.

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-rhel/ha-rhel.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. [Standart yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)kullanmanızı öneririz. Aşağıdaki liste (A)SCS ve ERS yük dengeleyicisinin yapılandırmasını gösterir.

### <a name="ascs"></a>(A) Scs

* Frontend yapılandırması
  * IP adresi 10.0.0.7
* Sonda Bağlantı Noktası
  * Bağlantı noktası 620<strong>&lt;nr&gt;</strong>
* Yük dengeleme kuralları
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
  * IP adresi 10.0.0.8
* Sonda Bağlantı Noktası
  * Bağlantı noktası 621<strong>&lt;nr&gt;</strong>
* Yük dengeleme kuralları
  * Standart Yük Dengeleyicisi kullanıyorsanız, **HA bağlantı noktalarını** seçin
  * Temel Yük Dengeleyicisi kullanıyorsanız, aşağıdaki bağlantı noktaları için Yük dengeleme kuralları oluşturun
    * 32<strong>&lt;nr&gt; </strong> TCP
    * 33<strong>&lt;nr&gt; </strong> TCP
    * 5<strong>&lt;nr&gt;</strong>13 TCP
    * 5<strong>&lt;nr&gt;</strong>14 TCP
    * 5<strong>&lt;nr&gt;</strong>16 TCP

* Arka uç yapılandırması
  * (A)SCS/ERS kümesinin bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı

## <a name="setting-up-glusterfs"></a>GlusterFS kurulumu

SAP NetWeaver, taşıma ve profil dizini için paylaşılan depolama gerektirir. SAP NetWeaver için GlusterFS'ı nasıl kurabilirsiniz üzerine [Red Hat Enterprise Linux'ta Azure VM'lerde GlusterFS'ı][glusterfs-ha] okuyun.

## <a name="setting-up-ascs"></a>(A)SCS kurulumu

Sanal makineler, kullanılabilirlik kümesi ve yük dengeleyicisi de dahil olmak üzere gerekli tüm Azure kaynaklarını dağıtmak için GitHub'dan bir Azure Şablonu kullanabilir veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure Şablonu ile Linux'u dağıtma

Azure Marketi, Red Hat Enterprise Linux için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir resim içerir. Gerekli tüm kaynakları dağıtmak için GitHub'daki hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablon sanal makineleri, yük dengeleyicisini, kullanılabilirlik kümesini vb. dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Azure portalında [ASCS/SCS şablonu][template-multisid-xscs] açma  
1. Aşağıdaki parametreleri girin
   1. Kaynak Öneki  
      Kullanmak istediğiniz önek girin. Değer, dağıtılan kaynaklar için önek olarak kullanılır.
   1. Yığın Türü  
      SAP NetWeaver yığın türünü seçin
   1. Os Tipi  
      Linux dağıtımlarından birini seçin. Bu örnek için RHEL 7'yi seçin
   1. Db Tipi  
      HANA'yı seçin
   1. Sap Sistem Sayısı  
      Bu kümede çalışan SAP sisteminin sayısı. 1'i seçin.
   1. Sistem Kullanılabilirliği  
      HA'yı seçin
   1. Admin Kullanıcı Adı, Yönetici Şifresi veya SSH anahtarı  
      Makinede oturum açabilen yeni bir kullanıcı oluşturulur.
   1. Alt net kimliği  
   VM'yi, VM'nin atanması gereken tanımlanmış bir alt ağınız olduğu varolan bir VNet'e dağıtmak istiyorsanız, bu alt ağın kimliğini adlandırın. Kimlik genellikle /abonelik/**&lt;abonelik kimliği&gt;**/kaynakGruplar/**&lt;kaynak&gt;grup adı**/providers/Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;subnet adı&gt; ** gibi görünür

### <a name="deploy-linux-manually-via-azure-portal"></a>Azure portalı üzerinden Linux'u el ile dağıtın

Öncelikle bu küme için sanal makineler oluşturmanız gerekir. Daha sonra, bir yük dengeleyici oluşturmak ve arka uç havuzunda sanal makineleri kullanın.

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluşturma
1. Kullanılabilirlik Kümesi Oluşturma  
   Max update etki alanını ayarlama
1. Sanal Makine Oluşturma 1  
   Bu örnekte red hat Enterprise Linux 7.4 görüntüsüen az RHEL 7'yi kullanın<https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Sanal Makine Oluşturma 2  
   Bu örnekte red hat Enterprise Linux 7.4 görüntüsüen az RHEL 7'yi kullanın<https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Her iki sanal makineye de en az bir veri diski ekleme  
   Veri diskleri /usr/sap/`<SAPSID`> dizini için kullanılır
1. Yük dengeleyici (dahili, standart):  
   1. Ön uç IP adreslerini oluşturma
      1. ASCS için IP adresi 10.0.0.7
         1. Yük bakiyesini açın, frontend IP havuzunu seçin ve Ekle'yi tıklatın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1-ascs-frontend)**
         1. Atamayı Statik olarak ayarlayın ve IP adresini girin (örneğin **10.0.0.7**)
         1. Tamam'ı tıklatın
      1. ASCS ERS için IP adresi 10.0.0.8
         * ERS için bir IP adresi oluşturmak için yukarıdaki adımları yineleme (örneğin **10.0.0.8** ve **nw1-aers-frontend)**
   1. Arka uç havuzunu oluşturma
      1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
      1. Yeni arka uç havuzunun adını girin (örneğin **nw1-backend)**
      1. Sanal makine ekle'yi tıklatın.
      1. Sanal makine'yi seçin.
      1. (A)SCS kümesinin sanal makinelerini ve IP adreslerini seçin.
      1. Ekle'ye tıklayın.
   1. Sağlık sondalarını oluşturma
      1. ASCS için Bağlantı Noktası 620**00**
         1. Yük bakiyesini açın, sistem durumu sondalarını seçin ve Ekle'yi tıklatın
         1. Yeni sağlık sondasının adını girin (örneğin **nw1-ascs-hp)**
         1. Protokol olarak TCP'yi seçin, bağlantı noktası 620**00,** Aralık 5'i ve Sağlıksız eşiği 2'yi koruyun
         1. Tamam'ı tıklatın
      1. ASCS ERS için Port 621**02**
         * ERS için bir sistem durumu sondası oluşturmak için yukarıdaki adımları tekrarlayın (örneğin 621**02** ve **nw1-aers-hp)**
   1. Yük dengeleme kuralları
      1. ASCS için yük dengeleme kuralları
         1. Yük bakiyesini açın, yük dengeleme kurallarını seçin ve Ekle'yi tıklatın
         1. Yeni yük dengeleyici kuralının adını girin (örneğin **nw1-lb-ascs)**
         1. Daha önce oluşturduğunuz ön uç IP adresini, arka uç havuzunu ve sistem sondasını seçin (örneğin **nw1-ascs-frontend,** **nw1-backend** ve **nw1-ascs-hp)**
         1. **HA bağlantı noktalarını** seçin
         1. Boşta kalma süresini 30 dakikaya çıkarma
         1. **Kayan IP'yi etkinleştirdiğinden emin olun**
         1. Tamam'ı tıklatın
         * ERS için yük dengeleme kuralları oluşturmak için yukarıdaki adımları yineleyin (örneğin **nw1-lb-ers)**
1. Alternatif olarak, senaryonuz temel yük dengeleyicisi (dahili) gerektiriyorsa, aşağıdaki adımları izleyin:  
   1. Ön uç IP adreslerini oluşturma
      1. ASCS için IP adresi 10.0.0.7
         1. Yük bakiyesini açın, frontend IP havuzunu seçin ve Ekle'yi tıklatın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1-ascs-frontend)**
         1. Atamayı Statik olarak ayarlayın ve IP adresini girin (örneğin **10.0.0.7**)
         1. Tamam'ı tıklatın
      1. ASCS ERS için IP adresi 10.0.0.8
         * ERS için bir IP adresi oluşturmak için yukarıdaki adımları yineleme (örneğin **10.0.0.8** ve **nw1-aers-frontend)**
   1. Arka uç havuzunu oluşturma
      1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
      1. Yeni arka uç havuzunun adını girin (örneğin **nw1-backend)**
      1. Sanal makine ekle'yi tıklatın.
      1. Daha önce oluşturduğunuz Kullanılabilirlik Kümesini seçin
      1. (A)SCS kümesinin sanal makinelerini seçin
      1. Tamam'ı tıklatın
   1. Sağlık sondalarını oluşturma
      1. ASCS için Bağlantı Noktası 620**00**
         1. Yük bakiyesini açın, sistem durumu sondalarını seçin ve Ekle'yi tıklatın
         1. Yeni sağlık sondasının adını girin (örneğin **nw1-ascs-hp)**
         1. Protokol olarak TCP'yi seçin, bağlantı noktası 620**00,** Aralık 5'i ve Sağlıksız eşiği 2'yi koruyun
         1. Tamam'ı tıklatın
      1. ASCS ERS için Port 621**02**
         * ERS için bir sistem durumu sondası oluşturmak için yukarıdaki adımları tekrarlayın (örneğin 621**02** ve **nw1-aers-hp)**
   1. Yük dengeleme kuralları
      1. ASCS için 32**00** TCP
         1. Yük bakiyesini açın, yük dengeleme kurallarını seçin ve Ekle'yi tıklatın
         1. Yeni yük dengeleyici kuralının adını girin (örneğin **nw1-lb-3200)**
         1. Daha önce oluşturduğunuz ön uç IP adresini, arka uç havuzunu ve sistem durumu sondasını seçin (örneğin **nw1-ascs-frontend)**
         1. Protokol **TCP**tutun , port **3200** girin
         1. Boşta kalma süresini 30 dakikaya çıkarma
         1. **Kayan IP'yi etkinleştirdiğinden emin olun**
         1. Tamam'ı tıklatın
      1. ASCS için ek bağlantı noktaları
         * ASCS için 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 ve TCP bağlantı noktaları için yukarıdaki adımları tekrarlayın
      1. ASCS ERS için ek bağlantı noktaları
         * ASCS ERS için 33**02**, 5**02**13, 5**02**14, 5**02**16 ve TCP bağlantı noktaları için yukarıdaki adımları tekrarlayın

> [!Note]
> Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ortak bitiş noktalarına yönlendirmeye izin verecek ek yapılandırma yapılmadığı sürece giden internet bağlantısı olmaz. Giden bağlantının nasıl elde edilene ilişkin ayrıntılar [için, SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için Genel uç nokta bağlantısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections)bakın.  

> [!IMPORTANT]
> Azure Yük Bakiyesi'nin arkasına yerleştirilen Azure VM'lerinde TCP zaman damgalarını etkinleştirme. TCP zaman damgalarını etkinleştirmek sistem durumu sondalarının başarısız lığa neden olur. Parametre **net.ipv4.tcp_timestamps** **0**'a ayarlayın. Ayrıntılar için [Bkz. Yük Dengeleyici sağlık probları.](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

### <a name="create-pacemaker-cluster"></a>Kalp Pili kümesi ni oluştur

Bu (A)SCS sunucusu için temel bir Pacemaker kümesi oluşturmak için [Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama](high-availability-guide-rhel-pacemaker.md) adımlarını izleyin.

### <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver kurulumu için hazırlanın

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme

   <pre><code># IP addresses of the GlusterFS nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   </code></pre>

1. **[A]** Paylaşılan dizinleri oluşturma

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NW1</b>/SYS
   sudo mkdir -p /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo mkdir -p /usr/sap/<b>NW1</b>/ERS<b>02</b>

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NW1</b>/SYS
   sudo chattr +i /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chattr +i /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[A]** GlusterFS istemcisi ve diğer gereksinimleri yükleyin

   <pre><code>sudo yum -y install glusterfs-fuse resource-agents resource-agents-sap
   </code></pre>

1. **[A]** Kaynak-aracılar-sap sürümünü denetleyin

   Yüklenen kaynak-aracılar-sap paketinin sürümünün en az 3.9.5-124.el7 olduğundan emin olun
   <pre><code>sudo yum info resource-agents-sap
   
   # Loaded plugins: langpacks, product-id, search-disabled-repos
   # Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
   # Installed Packages
   # Name        : resource-agents-sap
   # Arch        : x86_64
   # Version     : <b>3.9.5</b>
   # Release     : <b>124.el7</b>
   # Size        : 100 k
   # Repo        : installed
   # From repo   : rhel-sap-for-rhel-7-server-rpms
   # Summary     : SAP cluster resource agents and connector script
   # URL         : https://github.com/ClusterLabs/resource-agents
   # License     : GPLv2+
   # Description : The SAP resource agents and connector script interface with
   #          : Pacemaker to allow SAP instances to be managed in a cluster
   #          : environment.
   </code></pre>


1. **[A]** Montaj girişleri ekleme

   <pre><code>sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   <b>glust-0</b>:/<b>NW1</b>-sapmnt /sapmnt/<b>NW1</b> glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-trans /usr/sap/trans glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-sys /usr/sap/<b>NW1</b>/SYS glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   </code></pre>

   Yeni paylaşımları monte edin

   <pre><code>sudo mount -a
   </code></pre>

1. **[A]** SWAP dosyalarını yapılandırma

   <pre><code>sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Değişikliği etkinleştirmek için Aracıyı yeniden başlatın

   <pre><code>sudo service waagent restart
   </code></pre>

1. **[A]** RHEL yapılandırması

   SAP Note [2002167'de] açıklandığı şekilde RHEL'i yapılandırın

### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/ERS kurulumu

1. **[1]** ASCS örneği için sanal bir IP kaynağı ve sistem durumu sondası oluşturma

   <pre><code>sudo pcs node standby <b>nw1-cl-1</b>
   
   sudo pcs resource create fs_<b>NW1</b>_ASCS Filesystem device='<b>glust-0</b>:/<b>NW1</b>-ascs' \
     directory='/usr/sap/<b>NW1</b>/ASCS<b>00</b>' fstype='glusterfs' \
     options='backup-volfile-servers=<b>glust-1:glust-2</b>' \
     --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create vip_<b>NW1</b>_ASCS IPaddr2 \
     ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create nc_<b>NW1</b>_ASCS azure-lb port=620<b>00</b> \
     --group g-<b>NW1</b>_ASCS
   </code></pre>

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   <pre><code>sudo pcs status
   
   # Node <b>nw1-cl-1</b>: standby
   # Online: [ <b>nw1-cl-0</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-0</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-0</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-0</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-0</b>
   </code></pre>

1. **[1]** SAP NetWeaver ASCS yükleyin  

   SAP NetWeaver ASCS'yi, ascs için yük dengeleyici ön uç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adı kullanarak ilk düğüme kök olarak yükleyin, örneğin <b>nw1-ascs</b>, <b>10.0.0.7</b> ve yük dengeleyicisinin sondası için kullandığınız örnek numarası, örneğin <b>00</b>.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code># Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp
   
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Yükleme /usr/sap/**NW1**/ASCS**00'da**bir alt klasör oluşturamazsa, ASCS**00** klasörünün sahibini ve grubunu ayarlamayı deneyin ve yeniden deneyin.

   <pre><code>sudo chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   sudo chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]** ERS örneği için sanal bir IP kaynağı ve sistem durumu sondası oluşturma

   <pre><code>sudo pcs node unstandby <b>nw1-cl-1</b>
   sudo pcs node standby <b>nw1-cl-0</b>
   
   sudo pcs resource create fs_<b>NW1</b>_AERS Filesystem device='<b>glust-0</b>:/<b>NW1</b>-aers' \
     directory='/usr/sap/<b>NW1</b>/ERS<b>02</b>' fstype='glusterfs' \
     options='backup-volfile-servers=<b>glust-1:glust-2</b>' \
    --group g-<b>NW1</b>_AERS
   
   sudo pcs resource create vip_<b>NW1</b>_AERS IPaddr2 \
     ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
    --group g-<b>NW1</b>_AERS
   
   sudo pcs resource create nc_<b>NW1</b>_AERS azure-lb port=621<b>02</b> \
    --group g-<b>NW1</b>_AERS
   </code></pre>
 
   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   <pre><code>sudo pcs status
   
   # Node <b>nw1-cl-0</b>: standby
   # Online: [ <b>nw1-cl-1</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_AERS
   #      fs_<b>NW1</b>_AERS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_AERS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_AERS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   </code></pre>

1. **[2]** SAP NetWeaver ERS'i yükleyin  

   SAP NetWeaver ERS'i, örneğin <b>nw1-aers</b>, <b>10.0.0.8</b> ve yük bakiyesi sondası için kullandığınız örnek numarası, örneğin <b>02</b>gibi, ERS için yük dengeleyicisi önuç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adını kullanarak ikinci düğüme kök olarak yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code># Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp

   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Yükleme /usr/sap/**NW1**/ERS**02'de**bir alt klasör oluşturamazsa, ERS**02** klasörünün sahibini ve grubunu ayarlamayı deneyin ve yeniden deneyin.

   <pre><code>sudo chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   sudo chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>

1. **[1]** ASCS/SCS ve ERS örnek profillerini uyarlama

   * ASCS/SCS profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * ERS profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>


1. **[A]** Canlı Tutun'u Yapılandır

   SAP NetWeaver uygulama sunucusu ile ASCS/SCS arasındaki iletişim bir yazılım yük dengeleyicisi aracılığıyla yönlendirilir. Yük dengeleyici, yapılandırılabilir bir zaman anından sonra etkin olmayan bağlantıları keser. Bunu önlemek için SAP NetWeaver ASCS/SCS profilinde bir parametre ayarlamanız ve Linux sistem ayarlarını değiştirmeniz gerekir. Daha fazla bilgi için [SAP Note 1410736'yı][1410736] okuyun.

   ASCS/SCS profil parametresi enque/encni/set_so_keepalive zaten son adımda eklendi.

   <pre><code># Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]** /usr/sap/sapservices dosyasını güncelleştir

   Sapinit başlangıç komut dosyası tarafından örneklerin başlatılmasını önlemek için, Pacemaker tarafından yönetilen tüm örnekler /usr/sap/sapservices dosyasından yorumlanmalıdır. HANA SR ile kullanılacaksa SAP HANA örneğini açıklamayın.

   <pre><code>
   sudo vi /usr/sap/sapservices
   
   # On the node where you installed the ASCS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/<b>NW1</b>/ASCS<b>00</b>/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/<b>NW1</b>/ASCS<b>00</b>/exe/sapstartsrv pf=/usr/sap/<b>NW1</b>/SYS/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> -D -u <b>nw1</b>adm
   
   # On the node where you installed the ERS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/<b>NW1</b>/ERS<b>02</b>/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/<b>NW1</b>/ERS<b>02</b>/exe/sapstartsrv pf=/usr/sap/<b>NW1</b>/ERS<b>02</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> -D -u <b>nw1</b>adm
   </code></pre>

1. **[1]** SAP küme kaynaklarını oluşturma

  Enqueue server 1 mimarisi (ENSA1) kullanıyorsanız, kaynakları aşağıdaki gibi tanımlayın:

   <pre><code>sudo pcs property set maintenance-mode=true
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ASCS00 SAPInstance \
    InstanceName=<b>NW1</b>_ASCS00_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS00_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    InstanceName=<b>NW1</b>_ERS02_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS02_<b>nw1-aers</b>" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-<b>NW1</b>_AERS
      
   sudo pcs constraint colocation add g-<b>NW1</b>_AERS with g-<b>NW1</b>_ASCS -5000
   sudo pcs constraint location rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule score=2000 runs_ers_<b>NW1</b> eq 1
   sudo pcs constraint order g-<b>NW1</b>_ASCS then g-<b>NW1</b>_AERS kind=Optional symmetrical=false
   
   sudo pcs node unstandby <b>nw1-cl-0</b>
   sudo pcs property set maintenance-mode=false
   </code></pre>

   SAP, SAP NW 7.52 itibariyle çoğaltma da dahil olmak üzere enqueue server 2 için destek sundu. ABAP Platform 1809 ile başlayarak, enqueue server 2 varsayılan olarak yüklenir. Enqueue server 2 desteği için SAP note [2630416'ya](https://launchpad.support.sap.com/#/notes/2630416) bakın.
   Enqueue server 2 mimarisi[(ENSA2)](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)kullanıyorsanız, kaynak aracısı kaynak-aracı-sap-4.1.1-12.el7.x86_64 veya daha yeni yükleyin ve kaynakları aşağıdaki gibi tanımlayın:

<pre><code>sudo pcs property set maintenance-mode=true
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ASCS00 SAPInstance \
    InstanceName=<b>NW1</b>_ASCS00_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS00_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-<b>NW1</b>_ASCS
   
   sudo pcs resource create rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    InstanceName=<b>NW1</b>_ERS02_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS02_<b>nw1-aers</b>" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-<b>NW1</b>_AERS
      
   sudo pcs constraint colocation add g-<b>NW1</b>_AERS with g-<b>NW1</b>_ASCS -5000
   sudo pcs constraint order g-<b>NW1</b>_ASCS then g-<b>NW1</b>_AERS kind=Optional symmetrical=false
   sudo pcs constraint order start g-<b>NW1</b>_ASCS then stop g-<b>NW1</b>_AERS symmetrical=false
   
   sudo pcs node unstandby <b>nw1-cl-0</b>
   sudo pcs property set maintenance-mode=false
   </code></pre>

   Eski bir sürümden yükseltiyor ve enqueue server 2'ye geçiyorsanız, SAP note [2641322'ye](https://launchpad.support.sap.com/#/notes/2641322)bakın. 

   > [!NOTE]
   > Yukarıdaki yapılandırmadaki zaman ekmeleri yalnızca örneklerdir ve belirli SAP kurulumuna uyarlanmış olması gerekebilir. 

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   <pre><code>sudo pcs status
   
   # Online: [ <b>nw1-cl-0</b> <b>nw1-cl-1</b> ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started <b>nw1-cl-0</b>
   #  Resource Group: g-<b>NW1</b>_ASCS
   #      fs_<b>NW1</b>_ASCS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-1</b>
   #      nc_<b>NW1</b>_ASCS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-1</b>
   #      vip_<b>NW1</b>_ASCS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-1</b>
   #      rsc_sap_<b>NW1</b>_ASCS00 (ocf::heartbeat:SAPInstance):   Started <b>nw1-cl-1</b>
   #  Resource Group: g-<b>NW1</b>_AERS
   #      fs_<b>NW1</b>_AERS        (ocf::heartbeat:Filesystem):    Started <b>nw1-cl-0</b>
   #      nc_<b>NW1</b>_AERS        (ocf::heartbeat:azure-lb):      Started <b>nw1-cl-0</b>
   #      vip_<b>NW1</b>_AERS       (ocf::heartbeat:IPaddr2):       Started <b>nw1-cl-0</b>
   #      rsc_sap_<b>NW1</b>_ERS02  (ocf::heartbeat:SAPInstance):   Started <b>nw1-cl-0</b>
   </code></pre>

1. **[A]** Her iki düğümde DE ASCS ve ERS için güvenlik duvarı kuralları ekleme

   <pre><code># Probe Port of ASCS
   sudo firewall-cmd --zone=public --add-port=620<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=620<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=32<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=32<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=36<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=36<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=39<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=39<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=81<b>00</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=81<b>00</b>/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>13/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>13/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>14/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>14/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>16/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>00</b>16/tcp
   # Probe Port of ERS
   sudo firewall-cmd --zone=public --add-port=621<b>02</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=621<b>02</b>/tcp
   sudo firewall-cmd --zone=public --add-port=33<b>02</b>/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=33<b>02</b>/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>13/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>13/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>14/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>14/tcp
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>16/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=5<b>02</b>16/tcp
   </code></pre>

## <a name="sap-netweaver-application-server-preparation"></a><a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver uygulama sunucusu hazırlama

Bazı veritabanları veritabanı örneği yüklemebir uygulama sunucusunda yürütülmesini gerektirir. Bu gibi durumlarda bunları kullanabilmek için uygulama sunucusu sanal makineleri hazırlayın.

Adım ları körüklüyor, uygulama sunucusunu ASCS/SCS ve HANA sunucularından farklı bir sunucuya yüklediğinizi varsayar. Aksi takdirde aşağıdaki adımlardan bazıları (ana bilgisayar ad çözümlemesi yapılandırma gibi) gerekli değildir.

1. Kurulum ana bilgisayar ad çözünürlüğü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme

   <pre><code># IP addresses of the GlusterFS nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

1. Sapmnt dizinini oluşturma

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. GlusterFS istemcisi ve diğer gereksinimleri yükleyin

   <pre><code>sudo yum -y install glusterfs-fuse uuidd
   </code></pre>

1. Montaj girişleri ekleme

   <pre><code>sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   <b>glust-0</b>:/<b>NW1</b>-sapmnt /sapmnt/<b>NW1</b> glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   <b>glust-0</b>:/<b>NW1</b>-trans /usr/sap/trans glusterfs backup-volfile-servers=<b>glust-1:glust-2</b> 0 0
   </code></pre>

   Yeni paylaşımları monte edin

   <pre><code>sudo mount -a
   </code></pre>

1. SWAP dosyalarını yapılandırma
 
   <pre><code>
   sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Değişikliği etkinleştirmek için Aracıyı yeniden başlatın

   <pre><code>
   sudo service waagent restart
   </code></pre>

## <a name="install-database"></a>Veritabanını yükleme

Bu örnekte, SAP NetWeaver SAP HANA'ya yüklenir. Bu yükleme için desteklenen tüm veritabanını kullanabilirsiniz. AZURE'da SAP HANA'nın nasıl yüklenir hakkında daha fazla bilgi için Red Hat Enterprise Linux'taki. For a list of supported databases, see [SAP Note 1928533][1928533] [Azure VM'lerinde SAP HANA'nın yüksek kullanılabilirliği][sap-hana-ha]bölümüne bakın.

1. SAP veritabanı örneği yüklemesini çalıştırma

   Örneğin <b>nw1-db</b> ve <b>10.0.0.0.13</b>gibi veritabanı için yük dengeleyici ön uç yapılandırmasının IP adresine eşlenen sanal bir ana bilgisayar adı kullanarak SAP NetWeaver veritabanı örneğini kök olarak yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver uygulama sunucusu kurulumu

SAP uygulama sunucusu yüklemek için aşağıdaki adımları izleyin.

1. Uygulama sunucusu hazırlama

   Uygulama sunucusu hazırlamak için yukarıdaki bölümde [SAP NetWeaver uygulama sunucusu hazırlama](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) adımları izleyin.

1. SAP NetWeaver uygulama sunucusunu kurun

   Birincil veya ek SAP NetWeaver uygulamaları sunucusu yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA güvenli mağazayı güncelleştirin

   SAP HANA güvenli deposunu, SAP HANA Sistem Çoğaltma kurulumunun sanal adını belirtmek için güncelleştirin.

   Girişleri sapsid>adm \<olarak listelemek için aşağıdaki komutu çalıştırın

   <pre><code>hdbuserstore List
   </code></pre>

   Bu, tüm girişleri listelemelidir ve
   <pre><code>
   DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>NW1</b>
   </code></pre>

   Çıktı, varsayılan girişin IP adresinin yük bakiyesinin IP adresine değil, sanal makineyi işaret ettiğini gösterir. Bu giriş, yük bakiyesi sanal ana bilgisayar adını işaret etmek için değiştirilmesi gerekir. Aynı bağlantı noktası (yukarıdaki çıktıda**30313)** ve veritabanı adını (Yukarıdaki çıktıda**HN1)** kullandığınızdan emin olun!

   <pre><code>su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db</b>:<b>30313@NW1</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

1. ASCS örneğini el ile geçirin

   Teste başlamadan önce kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   ASCS örneğini geçirmek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource move rsc_sap_NW1_ASCS00
   
   [root@nw1-cl-0 ~]# pcs resource clear rsc_sap_NW1_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Düğüm çökmesini simüle edin

   Teste başlamadan önce kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   ASCS örneğinin çalıştığı düğümde aşağıdaki komutu kök olarak çalıştırın

   <pre><code>[root@nw1-cl-1 ~]# echo b > /proc/sysrq-trigger
   </code></pre>

   Düğüm yeniden başlatıldıktan sonraki durum aşağıdaki gibi görünmelidir.

   <pre><code>Online: [ nw1-cl-0 nw1-cl-1 ]
   
   Full list of resources:
   
   rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   
   Failed Actions:
   * rsc_sap_NW1_ERS02_monitor_11000 on nw1-cl-0 'not running' (7): call=45, status=complete, exitreason='',
       last-rc-change='Tue Aug 21 13:52:39 2018', queued=0ms, exec=0ms
   </code></pre>

   Başarısız kaynakları temizlemek için aşağıdaki komutu kullanın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. İleti sunucusu işlemini öldürme

   Teste başlamadan önce kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   İleti sunucusunun işlemini tanımlamak ve öldürmek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pgrep ms.sapNW1 | xargs kill -9
   </code></pre>

   İleti sunucusunu yalnızca bir kez öldürürseniz, `sapstart`ileti sunucu tarafından yeniden başlatılır. Yeterince sık öldürürseniz, Pacemaker sonunda diğer düğüm için ASCS örneğini taşıyacak. Testten sonra ASCS ve ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ASCS00
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Enqueue sunucu işlemini öldür

   Teste başlamadan önce kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Aşağıdaki komutları, ASCS örneğinin enqueue sunucusunu öldürmek için çalıştığı düğümde kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-1 ~]# pgrep en.sapNW1 | xargs kill -9
   </code></pre>

   ASCS örneği hemen diğer düğüm üzerinde başarısız olmalıdır. ASCS örneği başladıktan sonra ERS örneği de başarısız olmalıdır. Testten sonra ASCS ve ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ASCS00
   [root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Enqueue çoğaltma sunucu işlemini öldür

   Teste başlamadan önce kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Aşağıdaki komutu, ERS örneğinin enqueue çoğaltma sunucu işlemini öldürmek için çalıştığı düğümde kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-1 ~]# pgrep er.sapNW1 | xargs kill -9
   </code></pre>

   Komutu yalnızca bir kez `sapstart` çalıştırsanız, işlemi yeniden başlatın. Yeterince sık çalıştırArsanız, `sapstart` işlemi yeniden başlatmaz ve kaynak durdurulmuş durumda olur. Testten sonra ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pcs resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Enqueue sapstartsrv işlemini öldür

   Teste başlamadan önce kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   ASCS'nin çalıştığı düğümde aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>[root@nw1-cl-0 ~]# pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   [root@nw1-cl-0 ~]# kill -9 59545
   </code></pre>

   Sapstartsrv işlemi her zaman izlemenin bir parçası olarak Pacemaker kaynak aracısı tarafından yeniden başlatılmalıdır. Testten sonra kaynak durumu:

   <pre><code>rsc_st_azure    (stonith:fence_azure_arm):      Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_AERS
        fs_NW1_AERS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_AERS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_AERS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [SAP uygulamaları multi-SID kılavuzu için RHEL'de Azure VM'lerde SAP NW için HA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid)
* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik oluşturmayı ve AZURE'da SAP HANA'nın olağanüstü durum kurtarmasını nasıl planlayabilirsiniz (büyük örnekler), Bkz. [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve Azure'da olağanüstü durum kurtarma.](hana-overview-high-availability-disaster-recovery.md)
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
