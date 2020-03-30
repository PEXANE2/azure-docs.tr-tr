---
title: SLES'te SAP NetWeaver için Azure VM'ler yüksek kullanılabilirlik | Microsoft Dokümanlar
description: SAP uygulamaları için SUSE Linux Enterprise Server'da SAP NetWeaver için yüksek kullanılabilirlik kılavuzu
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
ms.openlocfilehash: 05effb7d2e64c5f27acabad4b086ba27d6849cc8
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348814"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

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

Bu makalede, sanal makinelerin nasıl dağıtılancaör, sanal makineleri yapılandırma, küme çerçevesini yükleme ve yüksek kullanılabilir sap NetWeaver 7.50 sistemi nasıl yüklenir.
Örnek yapılandırmalarda, yükleme komutlarında vb. ASCS örnek numarası 00, ERS örnek numarası 02 ve SAP System ID NW1 kullanılır. Örnekteki kaynakların adları (örneğin sanal makineler, sanal ağlar) kaynakları oluşturmak için SAP sistem kimliği NW1 ile [yakınsanan şablonu][template-converged] kullandığınızı varsayar.

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun

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

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için SAP NetWeaver bir NFS sunucusu gerektirir. NFS sunucusu ayrı bir kümede yapılandırılır ve birden çok SAP sistemi tarafından kullanılabilir.

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-suse/ha-suse.png)

NFS sunucusu, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. [Standart yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)kullanmanızı öneririz. Aşağıdaki liste (A)SCS ve ERS yük dengeleyicisinin yapılandırmasını gösterir.

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


## <a name="setting-up-a-highly-available-nfs-server"></a>Yüksek oranda kullanılabilir bir NFS sunucusu ayarlama

SAP NetWeaver, taşıma ve profil dizini için paylaşılan depolama gerektirir. SUSE [Linux Enterprise Server'da Azure VM'lerinde NFS için][nfs-ha] SAP NetWeaver için bir NFS sunucusu nun nasıl kurulabildiğini okuyun.

## <a name="setting-up-ascs"></a>(A)SCS kurulumu

Sanal makineler, kullanılabilirlik kümesi ve yük dengeleyicisi de dahil olmak üzere gerekli tüm Azure kaynaklarını dağıtmak için GitHub'dan bir Azure Şablonu kullanabilir veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure Şablonu ile Linux'u dağıtma

Azure Marketi, SAP Applications 12 için SUSE Linux Enterprise Server için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir görüntü içerir. Pazar yeri görüntüsü SAP NetWeaver için kaynak aracısı içerir.

Gerekli tüm kaynakları dağıtmak için GitHub'daki hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablon sanal makineleri, yük dengeleyicisini, kullanılabilirlik kümesini vb. dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. [ASCS/SCS Multi SID şablonuna][template-multisid-xscs] veya Azure portalında [yakınsanan şablonu][template-converged] açın. 
   ASCS/SCS şablonu yalnızca SAP NetWeaver ASCS/SCS ve ERS (yalnızca Linux) örnekleri için yük dengeleme kuralları oluştururken, yakınsanan şablon da bir veritabanı için yük dengeleme kuralları oluşturur (örneğin Microsoft SQL Server veya SAP HANA). SAP NetWeaver tabanlı bir sistem yüklemeyi planlıyorsanız ve veritabanını aynı makinelere yüklemek istiyorsanız, [yakınsanmış şablonu][template-converged]kullanın.
1. Aşağıdaki parametreleri girin
   1. Kaynak Öneki (yalnızca ASCS/SCS Multi SID şablonu)  
      Kullanmak istediğiniz önek girin. Değer, dağıtılan kaynaklar için önek olarak kullanılır.
   3. Sap Sistem Kimliği (yalnızca birleştirilmiş şablon)  
      Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
   4. Yığın Türü  
      SAP NetWeaver yığın türünü seçin
   5. Os Tipi  
      Linux dağıtımlarından birini seçin. Bu örnekiçin, SLES 12 BYOS'u seçin
   6. Db Tipi  
      HANA'yı seçin
   7. Sap Sistem Boyutu.  
      Yeni sistemin sağladığı SAPS miktarı. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze sorun
   8. Sistem Kullanılabilirliği  
      HA'yı seçin
   9. Admin Kullanıcı Adı ve Yönetici Şifresi  
      Makinede oturum açmak için kullanılabilecek yeni bir kullanıcı oluşturulur.
   10. Alt net kimliği  
   VM'yi, VM'nin atanması gereken tanımlanmış bir alt ağınız olduğu varolan bir VNet'e dağıtmak istiyorsanız, bu alt ağın kimliğini adlandırın. Kimlik genellikle /abonelik/**&lt;abonelik kimliği&gt;**/kaynakGruplar/**&lt;kaynak&gt;grup adı**/providers/Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;subnet adı&gt; ** gibi görünür

### <a name="deploy-linux-manually-via-azure-portal"></a>Azure portalı üzerinden Linux'u el ile dağıtın

Öncelikle bu NFS kümesi için sanal makineler oluşturmanız gerekir. Daha sonra, bir yük dengeleyici oluşturmak ve arka uç havuzunda sanal makineleri kullanın.

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluşturma
1. Kullanılabilirlik Kümesi Oluşturma  
   Max update etki alanını ayarlama
1. Sanal Makine Oluşturma 1  
   Bu örnekte en az SLES4SAP 12 SP1 kullanınhttps://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SAP Uygulamaları için SLES 12 SP1 kullanılır  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Sanal Makine Oluşturma 2  
   Bu örnekte en az SLES4SAP 12 SP1 kullanınhttps://portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP1PremiumImage-ARM  
   SAP Uygulamaları için SLES 12 SP1 kullanılır  
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
         * ERS için bir IP adresi oluşturmak için yukarıdaki adımları yineleme (örneğin **10.0.0.8** ve **nw1-aers-backend)**
   1. Arka uç havuzunu oluşturma
      1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
      1. Yeni arka uç havuzunun adını girin (örneğin **nw1-backend)**
      1. Sanal makine ekle'yi tıklatın.
      1. Sanal Makine'yi Seçin
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

Bu (A)SCS sunucusu için temel bir Pacemaker kümesi oluşturmak için [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama](high-availability-guide-suse-pacemaker.md) adımlarını izleyin.

### <a name="installation"></a>Yükleme

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** SUSE Konektörü Yükleyin

   <pre><code>sudo zypper install sap-suse-cluster-connector
   </code></pre>

   > [!NOTE]
   > Ana bilgisayar adlarında tire kullanma yla ilgili bilinen sorun, paket **sap-suse-cluster-bağlayıcısının** **3.1.1** sürümüyle giderilmiştir. Ana bilgisayar adında tireli küme düğümleri kullanıyorsanız, paket sap-suse-cluster-konektörünün en az 3.1.1 sürümünü kullandığınızdan emin olun. Aksi takdirde kümeniz çalışmaz. 

   SAP SUSE küme bağlayıcısının yeni sürümünü yüklediğinizden emin olun. Eski bir sap_suse_cluster_connector denir ve yeni bir **sap-suse-küme-bağlayıcı**denir.

   ```
   sudo zypper info sap-suse-cluster-connector
   
   Information for package sap-suse-cluster-connector:
   ---------------------------------------------------
   Repository     : SLE-12-SP3-SAP-Updates
   Name           : sap-suse-cluster-connector
   <b>Version        : 3.0.0-2.2</b>
   Arch           : noarch
   Vendor         : SUSE LLC <https://www.suse.com/>
   Support Level  : Level 3
   Installed Size : 41.6 KiB
   <b>Installed      : Yes</b>
   Status         : up-to-date
   Source package : sap-suse-cluster-connector-3.0.0-2.2.src
   Summary        : SUSE High Availability Setup for SAP Products
   ```

1. **[A]** SAP kaynak aracılarını güncelleştirme  
   
   Kaynak aracıları paketi için bir yama, bu makalede açıklanan yeni yapılandırmayı kullanmak için gereklidir. Yama zaten aşağıdaki komutla yüklenmiş se, kontrol edebilirsiniz

   <pre><code>sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   Çıktı benzer olmalıdır

   <pre><code>&lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   grep komutu IS_ERS parametresini bulamazsa, [SUSE indirme sayfasında](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents) listelenen düzeltme eki yüklemeniz gerekir

   <pre><code># example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme   

   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver kurulumu için hazırlanın

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

1. **[A]** Otomatik leri yapılandırma

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Bir dosya oluşturma

   <pre><code>sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   /usr/sap/<b>NW1</b>/SYS -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sidsys
   </code></pre>

   Yeni paylaşımları monte etmek için otomatik başlatma

   <pre><code>sudo systemctl enable autofs
   sudo service autofs restart
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


### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/ERS kurulumu

1. **[1]** ASCS örneği için sanal bir IP kaynağı ve sistem durumu sondası oluşturma

   > [!IMPORTANT]
   > Son testler, netcat'in biriktirme listesi ve yalnızca bir bağlantı taşıma sınırlaması nedeniyle isteklere yanıt vermeyi bıraktığı durumları ortaya çıkardı. Netcat kaynağı Azure Yük dengeleyici isteklerini dinlemeyi durdurur ve kayan IP kullanılamaz hale gelir.  
   > Mevcut Pacemaker kümeleri için, geçmişte netcat'i socat ile değiştirmemizi tavsiye ettik. Şu anda, paket kaynak aracılarının bir parçası olan azure-lb kaynak aracısını aşağıdaki paket sürüm gereksinimleriyle birlikte kullanmanızı öneririz:
   > - SLES 12 SP4/SP5 için, sürüm en az kaynak-aracılar-4.3.018.a7fb5035-3.30.1 olmalıdır.  
   > - SLES 15/15 SP1 için, sürüm en az kaynak-ajanlar-4.3.0184.6ee15eb2-4.13.1 olmalıdır.  
   >
   > Değişikliğin kısa bir kapalı kalma süresi gerektireceğini unutmayın.  
   > Varolan Pacemaker kümeleri için yapılandırma Azure [Load-Balancer Detection Hardening'de](https://www.suse.com/support/kb/doc/?id=7024128)açıklandığı gibi socat kullanmak üzere zaten değiştirildiyse, hemen azure-lb kaynak aracısına geçmeniz gerek yoktur.

   <pre><code>sudo crm node standby <b>nw1-cl-1</b>
   
   sudo crm configure primitive fs_<b>NW1</b>_ASCS Filesystem device='<b>nw1-nfs</b>:/<b>NW1</b>/ASCS' directory='/usr/sap/<b>NW1</b>/ASCS<b>00</b>' fstype='nfs4' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>NW1</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.7</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ASCS azure-lb port=620<b>00</b>
   
   sudo crm configure group g-<b>NW1</b>_ASCS fs_<b>NW1</b>_ASCS nc_<b>NW1</b>_ASCS vip_<b>NW1</b>_ASCS \
      meta resource-stickiness=3000
   </code></pre>

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   <pre><code>sudo crm_mon -r
   
   # Node nw1-cl-1: standby
   # <b>Online: [ nw1-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-0</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-0</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   </code></pre>

1. **[1]** SAP NetWeaver ASCS yükleyin  

   SAP NetWeaver ASCS'yi, ascs için yük dengeleyici ön uç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adı kullanarak ilk düğüme kök olarak yükleyin, örneğin <b>nw1-ascs</b>, <b>10.0.0.7</b> ve yük dengeleyicisinin sondası için kullandığınız örnek numarası, örneğin <b>00</b>.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   Yükleme /usr/sap/**NW1**/ASCS**00'da**bir alt klasör oluşturamazsa, ASCS**00** klasörünün sahibini ve grubunu ayarlamayı deneyin ve yeniden deneyin.

   <pre><code>chown nw1adm /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ASCS<b>00</b>
   </code></pre>

1. **[1]** ERS örneği için sanal bir IP kaynağı ve sistem durumu sondası oluşturma

   <pre><code>sudo crm node online <b>nw1-cl-1</b>
   sudo crm node standby <b>nw1-cl-0</b>
   
   sudo crm configure primitive fs_<b>NW1</b>_ERS Filesystem device='<b>nw1-nfs</b>:/<b>NW1</b>/ASCSERS' directory='/usr/sap/<b>NW1</b>/ERS<b>02</b>' fstype='nfs4' \
     op start timeout=60s interval=0 \
     op stop timeout=60s interval=0 \
     op monitor interval=20s timeout=40s
   
   sudo crm configure primitive vip_<b>NW1</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.8</b> cidr_netmask=<b>24</b> \
     op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_ERS azure-lb port=621<b>02</b>
   
   sudo crm configure group g-<b>NW1</b>_ERS fs_<b>NW1</b>_ERS nc_<b>NW1</b>_ERS vip_<b>NW1</b>_ERS
   </code></pre>

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   <pre><code>sudo crm_mon -r
   
   # Node <b>nw1-cl-0: standby</b>
   # <b>Online: [ nw1-cl-1 ]</b>
   # 
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      fs_NW1_ERS (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ERS (ocf::heartbeat:azure-lb):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   </code></pre>

1. **[2]** SAP NetWeaver ERS'i yükleyin

   SAP NetWeaver ERS'i, örneğin <b>nw1-aers</b>, <b>10.0.0.8</b> ve yük bakiyesi sondası için kullandığınız örnek numarası, örneğin <b>02</b>gibi, ERS için yük dengeleyicisi önuç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adını kullanarak ikinci düğüme kök olarak yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > SWPM SP 20 PL 05 veya daha yüksek kullanın. Alt sürümler izinleri doğru ayarlamaz ve yükleme başarısız olur.

   Yükleme /usr/sap/**NW1**/ERS**02'de**bir alt klasör oluşturamazsa, ERS**02** klasörünün sahibini ve grubunu ayarlamayı deneyin ve yeniden deneyin.

   <pre><code>chown nw1adm /usr/sap/<b>NW1</b>/ERS<b>02</b>
   chgrp sapsys /usr/sap/<b>NW1</b>/ERS<b>02</b>
   </code></pre>


1. **[1]** ASCS/SCS ve ERS örnek profillerini uyarlama
 
   * ASCS/SCS profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_<b>ASCS00</b>_<b>nw1-ascs</b>
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * ERS profili

   <pre><code>sudo vi /sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   
   # remove Autostart from ERS profile
   # Autostart = 1
   </code></pre>

1. **[A]** Canlı Tutun'u Yapılandır

   SAP NetWeaver uygulama sunucusu ile ASCS/SCS arasındaki iletişim bir yazılım yük dengeleyicisi aracılığıyla yönlendirilir. Yük dengeleyici, yapılandırılabilir bir zaman anından sonra etkin olmayan bağlantıları keser. Bunu önlemek için SAP NetWeaver ASCS/SCS profilinde bir parametre ayarlamanız ve Linux sistem ayarlarını değiştirmeniz gerekir. Daha fazla bilgi için [SAP Note 1410736'yı][1410736] okuyun.

   ASCS/SCS profil parametresi enque/encni/set_so_keepalive zaten son adımda eklendi.

   <pre><code># Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]** Yüklemeden sonra SAP kullanıcılarını yapılandırma

   <pre><code># Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nw1</b>adm
   </code></pre>

1. **[1]** ASCS ve ERS SAP hizmetlerini sapservice dosyasına ekleme

   Ascs hizmet girişini ikinci düğüme ekleyin ve ERS hizmet girişini ilk düğüme kopyalayın.

   <pre><code>cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nw1-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nw1-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]** SAP küme kaynaklarını oluşturma

Enqueue server 1 mimarisi (ENSA1) kullanıyorsanız, kaynakları aşağıdaki gibi tanımlayın:

   <pre><code>sudo crm configure property maintenance-mode="true"
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on-fail=restart \
    params InstanceName=<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on-fail=restart \
    params InstanceName=<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000
   
   sudo crm configure modgroup g-<b>NW1</b>_ASCS add rsc_sap_<b>NW1</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>NW1</b>_ERS add rsc_sap_<b>NW1</b>_ERS<b>02</b>
   
   sudo crm configure colocation col_sap_<b>NW1</b>_no_both -5000: g-<b>NW1</b>_ERS g-<b>NW1</b>_ASCS
   sudo crm configure location loc_sap_<b>NW1</b>_failover_to_ers rsc_sap_<b>NW1</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NW1</b> eq 1
   sudo crm configure order ord_sap_<b>NW1</b>_first_start_ascs Optional: rsc_sap_<b>NW1</b>_ASCS<b>00</b>:start rsc_sap_<b>NW1</b>_ERS<b>02</b>:stop symmetrical=false
   
   sudo crm node online <b>nw1-cl-0</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

  SAP, SAP NW 7.52 itibariyle çoğaltma da dahil olmak üzere enqueue server 2 için destek sundu. ABAP Platform 1809 ile başlayarak, enqueue server 2 varsayılan olarak yüklenir. Enqueue server 2 desteği için SAP note [2630416'ya](https://launchpad.support.sap.com/#/notes/2630416) bakın.
Enqueue server 2 mimarisi[(ENSA2)](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)kullanıyorsanız, kaynakları aşağıdaki gibi tanımlayın:

<pre><code>sudo crm configure property maintenance-mode="true"
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ASCS<b>00</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on-fail=restart \
    params InstanceName=<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ASCS<b>00</b>_<b>nw1-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000
   
   sudo crm configure primitive rsc_sap_<b>NW1</b>_ERS<b>02</b> SAPInstance \
    operations \$id=rsc_sap_<b>NW1</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on-fail=restart \
    params InstanceName=<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b> START_PROFILE="/sapmnt/<b>NW1</b>/profile/<b>NW1</b>_ERS<b>02</b>_<b>nw1-aers</b>" AUTOMATIC_RECOVER=false IS_ERS=true 
   
   sudo crm configure modgroup g-<b>NW1</b>_ASCS add rsc_sap_<b>NW1</b>_ASCS<b>00</b>
   sudo crm configure modgroup g-<b>NW1</b>_ERS add rsc_sap_<b>NW1</b>_ERS<b>02</b>
   
   sudo crm configure colocation col_sap_<b>NW1</b>_no_both -5000: g-<b>NW1</b>_ERS g-<b>NW1</b>_ASCS
   sudo crm configure order ord_sap_<b>NW1</b>_first_start_ascs Optional: rsc_sap_<b>NW1</b>_ASCS<b>00</b>:start rsc_sap_<b>NW1</b>_ERS<b>02</b>:stop symmetrical=false
   
   sudo crm node online <b>nw1-cl-0</b>
   sudo crm configure property maintenance-mode="false"
   </code></pre>

  Eski bir sürümden yükseltiyor ve enqueue server 2'ye geçiyorsanız, SAP note [2641019'a](https://launchpad.support.sap.com/#/notes/2641019)bakın. 

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.


   <pre><code>sudo crm_mon -r
   
   # Online: <b>[ nw1-cl-0 nw1-cl-1 ]</b>
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ASCS
   #      fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-1</b>
   #      nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      <b>Started nw1-cl-1</b>
   #      vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-1</b>
   #      rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-1</b>
   #  Resource Group: g-NW1_ERS
   #      fs_NW1_ERS (ocf::heartbeat:Filesystem):    <b>Started nw1-cl-0</b>
   #      nc_NW1_ERS (ocf::heartbeat:azure-lb):      <b>Started nw1-cl-0</b>
   #      vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nw1-cl-0</b>
   #      rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nw1-cl-0</b>
   </code></pre>

## <a name="sap-netweaver-application-server-preparation"></a><a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver uygulama sunucusu hazırlama

Bazı veritabanları veritabanı örneği yüklemebir uygulama sunucusunda yürütülmesini gerektirir. Bu gibi durumlarda bunları kullanabilmek için uygulama sunucusu sanal makineleri hazırlayın.

Adım ları körüklüyor, uygulama sunucusunu ASCS/SCS ve HANA sunucularından farklı bir sunucuya yüklediğinizi varsayar. Aksi takdirde aşağıdaki adımlardan bazıları (ana bilgisayar ad çözümlemesi yapılandırma gibi) gerekli değildir.

1. İşletim sistemini yapılandırma

   Kirli önbelleğin boyutunu küçültün. Daha fazla bilgi için, [büyük RAM'li SLES 11/12 sunucularında düşük yazma performansı](https://www.suse.com/support/kb/doc/?id=7010287)na bakın.

   <pre><code>sudo vi /etc/sysctl.conf

   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. Kurulum ana bilgisayar ad çözünürlüğü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

   ```bash
   sudo vi /etc/hosts
   ```

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme

   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.7 nw1-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.8 nw1-aers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.13 nw1-db</b>
   # IP address of all application servers
   <b>10.0.0.20 nw1-di-0</b>
   <b>10.0.0.21 nw1-di-1</b>
   </code></pre>

1. Sapmnt dizinini oluşturma

   <pre><code>sudo mkdir -p /sapmnt/<b>NW1</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NW1</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. Otomatik leri yapılandırma

   <pre><code>sudo vi /etc/auto.master
   
   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Yeni bir dosya oluşturma

   <pre><code>sudo vi /etc/auto.direct
   
   # Add the following lines to the file, save and exit
   /sapmnt/<b>NW1</b> -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nw1-nfs</b>:/<b>NW1</b>/trans
   </code></pre>

   Yeni paylaşımları monte etmek için otomatik başlatma

   <pre><code>sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. SWAP dosyalarını yapılandırma

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

## <a name="install-database"></a>Veritabanını yükleme

Bu örnekte, SAP NetWeaver SAP HANA'ya yüklenir. Bu yükleme için desteklenen tüm veritabanını kullanabilirsiniz. AZURE'da SAP HANA'nın nasıl yüklenir hakkında daha fazla bilgi için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği][sap-hana-ha]bölümüne bakın. Desteklenen veritabanlarının listesi için [SAP Note 1928533'e][1928533]bakın.

1. SAP veritabanı örneği yüklemesini çalıştırma

   Örneğin <b>nw1-db</b> ve <b>10.0.0.0.13</b>gibi veritabanı için yük dengeleyici ön uç yapılandırmasının IP adresine eşlenen sanal bir ana bilgisayar adı kullanarak SAP NetWeaver veritabanı örneğini kök olarak yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver uygulama sunucusu kurulumu

SAP uygulama sunucusu yüklemek için aşağıdaki adımları izleyin.

1. Uygulama sunucusu hazırlama

   Uygulama sunucusu hazırlamak için yukarıdaki bölümde [SAP NetWeaver uygulama sunucusu hazırlama](high-availability-guide-suse.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) adımları izleyin.

1. SAP NetWeaver uygulama sunucusunu kurun

   Birincil veya ek SAP NetWeaver uygulamaları sunucusu yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   <pre><code>sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. SAP HANA güvenli mağazayı güncelleştirin

   SAP HANA güvenli deposunu, SAP HANA Sistem Çoğaltma kurulumunun sanal adını belirtmek için güncelleştirin.

   Girişleri listelemek için aşağıdaki komutu çalıştırın
   <pre><code>hdbuserstore List
   </code></pre>

   Bu, tüm girişleri listelemelidir ve
   <pre><code>DATA FILE       : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.DAT
   KEY FILE        : /home/nw1adm/.hdb/nw1-di-0/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 10.0.0.14:<b>30313</b>
     USER: <b>SAPABAP1</b>
     DATABASE: <b>HN1</b>
   </code></pre>

   Çıktı, varsayılan girişin IP adresinin yük bakiyesinin IP adresine değil, sanal makineyi işaret ettiğini gösterir. Bu giriş, yük bakiyesi sanal ana bilgisayar adını işaret etmek için değiştirilmesi gerekir. Aynı bağlantı noktası (yukarıdaki çıktıda**30313)** ve veritabanı adını (Yukarıdaki çıktıda**HN1)** kullandığınızdan emin olun!

   <pre><code>su - <b>nw1</b>adm
   hdbuserstore SET DEFAULT <b>nw1-db:30313@HN1</b> <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

Aşağıdaki testler, SUSE'nin en iyi uygulama kılavuzlarındaki test çalışmalarının bir kopyasıdır. Onlar sizin rahatınız için kopyalanır. Her zaman da en iyi uygulamalar kılavuzları okuyun ve eklenmiştir olabilir tüm ek testler gerçekleştirin.

1. Test HAGetFailoverConfig, HACheckConfig ve HACheckFailoverConfig

   Aşağıdaki komutları ASCS örneğinin şu anda çalıştığı düğümde sapsid>adm olarak \<çalıştırın. Komutlar FAIL: Yetersiz bellek ile başarısız olursa, ana bilgisayar adınızdaki tirelerden kaynaklanabilir. Bu bilinen bir sorundur ve sap-suse-cluster-konektör paketinde SUSE tarafından düzeltilecektir.

   <pre><code>nw1-cl-0:nw1adm 54> sapcontrol -nr <b>00</b> -function HAGetFailoverConfig
   
   # 15.08.2018 13:50:36
   # HAGetFailoverConfig
   # OK
   # HAActive: TRUE
   # HAProductVersion: Toolchain Module
   # HASAPInterfaceVersion: Toolchain Module (sap_suse_cluster_connector 3.0.1)
   # HADocumentation: https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
   # HAActiveNode:
   # HANodes: nw1-cl-0, nw1-cl-1
   
   nw1-cl-0:nw1adm 55> sapcontrol -nr 00 -function HACheckConfig
   
   # 15.08.2018 14:00:04
   # HACheckConfig
   # OK
   # state, category, description, comment
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP instance configuration, 2 ABAP instances detected
   # SUCCESS, SAP CONFIGURATION, Redundant Java instance configuration, 0 Java instances detected
   # SUCCESS, SAP CONFIGURATION, Enqueue separation, All Enqueue server separated from application server
   # SUCCESS, SAP CONFIGURATION, MessageServer separation, All MessageServer separated from application server
   # SUCCESS, SAP CONFIGURATION, ABAP instances on multiple hosts, ABAP instances on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP SPOOL service configuration, 2 ABAP instances with SPOOL service detected
   # SUCCESS, SAP STATE, Redundant ABAP SPOOL service state, 2 ABAP instances with active SPOOL service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP SPOOL service on multiple hosts, ABAP instances with active ABAP SPOOL service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP BATCH service configuration, 2 ABAP instances with BATCH service detected
   # SUCCESS, SAP STATE, Redundant ABAP BATCH service state, 2 ABAP instances with active BATCH service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP BATCH service on multiple hosts, ABAP instances with active ABAP BATCH service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP DIALOG service configuration, 2 ABAP instances with DIALOG service detected
   # SUCCESS, SAP STATE, Redundant ABAP DIALOG service state, 2 ABAP instances with active DIALOG service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP DIALOG service on multiple hosts, ABAP instances with active ABAP DIALOG service on multiple hosts detected
   # SUCCESS, SAP CONFIGURATION, Redundant ABAP UPDATE service configuration, 2 ABAP instances with UPDATE service detected
   # SUCCESS, SAP STATE, Redundant ABAP UPDATE service state, 2 ABAP instances with active UPDATE service detected
   # SUCCESS, SAP STATE, ABAP instances with ABAP UPDATE service on multiple hosts, ABAP instances with active ABAP UPDATE service on multiple hosts detected
   # SUCCESS, SAP STATE, SCS instance running, SCS instance status ok
   # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version (nw1-ascs_NW1_00), SAPInstance includes is-ers patch
   # SUCCESS, SAP CONFIGURATION, Enqueue replication (nw1-ascs_NW1_00), Enqueue replication enabled
   # SUCCESS, SAP STATE, Enqueue replication state (nw1-ascs_NW1_00), Enqueue replication active
   
   nw1-cl-0:nw1adm 56> sapcontrol -nr 00 -function HACheckFailoverConfig
   
   # 15.08.2018 14:04:08
   # HACheckFailoverConfig
   # OK
   # state, category, description, comment
   # SUCCESS, SAP CONFIGURATION, SAPInstance RA sufficient version, SAPInstance includes is-ers patch
   </code></pre>

1. ASCS örneğini el ile geçirin

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   ASCS örneğini geçirmek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource migrate rsc_sap_NW1_ASCS00 force
   # INFO: Move constraint created for rsc_sap_NW1_ASCS00
   
   nw1-cl-0:~ # crm resource unmigrate rsc_sap_NW1_ASCS00
   # INFO: Removed migration constraints for rsc_sap_NW1_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Test HAFailoverToNode

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   ASCS örneğini \<geçirmek için aşağıdaki komutları sapsid>adm olarak çalıştırın.

   <pre><code>nw1-cl-0:nw1adm 55> sapcontrol -nr 00 -host nw1-ascs -user nw1adm &lt;password&gt; -function HAFailoverToNode ""
   
   # run as root
   # Remove failed actions for the ERS that occurred as part of the migration
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   # Remove migration constraints
   nw1-cl-0:~ # crm resource clear rsc_sap_NW1_ASCS00
   #INFO: Removed migration constraints for rsc_sap_NW1_ASCS00
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Düğüm çökmesini simüle edin

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-0
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   ASCS örneğinin çalıştığı düğümde aşağıdaki komutu kök olarak çalıştırın

   <pre><code>nw1-cl-0:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   SBD kullanıyorsanız, Pacemaker otomatik olarak öldürülen düğüm üzerinde başlamamalıdır. Düğüm yeniden başlatıldıktan sonraki durum aşağıdaki gibi görünmelidir.

   <pre><code>Online: [ nw1-cl-1 ]
   OFFLINE: [ nw1-cl-0 ]
   
   Full list of resources:
   
   stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   
   Failed Actions:
   * rsc_sap_NW1_ERS02_monitor_11000 on nw1-cl-1 'not running' (7): call=219, status=complete, exitreason='none',
       last-rc-change='Wed Aug 15 14:38:38 2018', queued=0ms, exec=0ms
   </code></pre>

   Öldürülen düğümde Pacemaker'ı başlatmak, SBD iletilerini temizlemek ve başarısız kaynakları temizlemek için aşağıdaki komutları kullanın.

   <pre><code># run as root
   # list the SBD device(s)
   nw1-cl-0:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   nw1-cl-0:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message nw1-cl-0 clear
   
   nw1-cl-0:~ # systemctl start pacemaker
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. ASCS örneğinin test kılavuzunu yeniden başlatma

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Örneğin işlem su01 bir kullanıcı düzenleme tarafından bir enqueue kilidi oluşturun. ASCS örneğinin \<çalıştığı düğümde aşağıdaki komutları sapsid>adm olarak çalıştırın. Komutlar ASCS örneğini durdurur ve yeniden başlatır. Enqueue server 1 mimarisi kullanıyorsanız, enqueue kilidi bu testte kaybolması bekleniyor. Enqueue server 2 mimarisi kullanıyorsanız, enqueue korunur. 

   <pre><code>nw1-cl-1:nw1adm 54> sapcontrol -nr 00 -function StopWait 600 2
   </code></pre>

   ASCS örneği artık Pacemaker'da devre dışı bırakılmalı

   <pre><code>rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Stopped (disabled)
   </code></pre>

   ASCS örneğini aynı düğümde yeniden başlatın.

   <pre><code>nw1-cl-1:nw1adm 54> sapcontrol -nr 00 -function StartWait 600 2
   </code></pre>

   İşlem su01 enqueue kilidi kaybedilmeli ve arka uç sıfırlanmalıdır. Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. İleti sunucusu işlemini öldürme

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   İleti sunucusunun işlemini tanımlamak ve öldürmek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>nw1-cl-1:~ # pgrep ms.sapNW1 | xargs kill -9
   </code></pre>

   İleti sunucusunu yalnızca bir kez öldürürseniz, bu sunucu sapstart tarafından yeniden başlatılır. Yeterince sık öldürürseniz, Pacemaker sonunda diğer düğüm için ASCS örneğini taşıyacak. Testten sonra ASCS ve ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

1. Enqueue sunucu işlemini öldür

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
   </code></pre>

   Aşağıdaki komutları, ASCS örneğinin enqueue sunucusunu öldürmek için çalıştığı düğümde kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # pgrep en.sapNW1 | xargs kill -9
   </code></pre>

   ASCS örneği hemen diğer düğüm üzerinde başarısız olmalıdır. ASCS örneği başladıktan sonra ERS örneği de başarısız olmalıdır. Testten sonra ASCS ve ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ASCS00
   nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Enqueue çoğaltma sunucu işlemini öldür

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   Aşağıdaki komutu, ERS örneğinin enqueue çoğaltma sunucu işlemini öldürmek için çalıştığı düğümde kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # pgrep er.sapNW1 | xargs kill -9
   </code></pre>

   Komutu yalnızca bir kez çalıştırsanız, sapstart işlemi yeniden başlatacak. Yeterince sık çalıştırAn sapstart işlemi yeniden başlatmaz ve kaynak durdurulmuş durumda olur. Testten sonra ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>nw1-cl-0:~ # crm resource cleanup rsc_sap_NW1_ERS02
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

1. Enqueue sapstartsrv işlemini öldür

   Teste başlamadan önce kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

   ASCS'nin çalıştığı düğümde aşağıdaki komutları kök olarak çalıştırın.

   <pre><code>nw1-cl-1:~ # pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   nw1-cl-1:~ # kill -9 59545
   </code></pre>

   Sapstartsrv işlemi her zaman Pacemaker kaynak aracısı tarafından yeniden başlatılmalıdır. Testten sonra kaynak durumu:

   <pre><code>stonith-sbd     (stonith:external/sbd): Started nw1-cl-1
    Resource Group: g-NW1_ASCS
        fs_NW1_ASCS        (ocf::heartbeat:Filesystem):    Started nw1-cl-1
        nc_NW1_ASCS        (ocf::heartbeat:azure-lb):      Started nw1-cl-1
        vip_NW1_ASCS       (ocf::heartbeat:IPaddr2):       Started nw1-cl-1
        rsc_sap_NW1_ASCS00 (ocf::heartbeat:SAPInstance):   Started nw1-cl-1
    Resource Group: g-NW1_ERS
        fs_NW1_ERS (ocf::heartbeat:Filesystem):    Started nw1-cl-0
        nc_NW1_ERS (ocf::heartbeat:azure-lb):      Started nw1-cl-0
        vip_NW1_ERS        (ocf::heartbeat:IPaddr2):       Started nw1-cl-0
        rsc_sap_NW1_ERS02  (ocf::heartbeat:SAPInstance):   Started nw1-cl-0
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [SAP uygulamaları multi-SID kılavuzu için SLES'te Azure VM'lerde SAP NW için HA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid)
* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
