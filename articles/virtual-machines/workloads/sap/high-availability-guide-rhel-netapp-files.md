---
title: Azure NetApp Dosyaları ile RHEL'de SAP NW için Azure VM'ler yüksek kullanılabilirlik| Microsoft Dokümanlar
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
ms.openlocfilehash: 11119d193cd08944bdff4737e8182cc7bece0abc
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351255"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux-with-azure-netapp-files-for-sap-applications"></a>Azure Sanal Makineler, SAP uygulamaları için Azure NetApp Dosyaları ile Red Hat Enterprise Linux'ta SAP NetWeaver için yüksek kullanılabilirlik

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

Bu makalede, [Azure NetApp Dosyalarını](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/)kullanarak sanal makinelerin nasıl dağıtılancaösin, sanal makineleri yapılandırmanın, küme çerçevesini nasıl yükleyeceğimiz ve yüksek oranda kullanılabilir bir SAP NetWeaver 7.50 sistemi nasıl yükleneceğiz açıklanmaktadır.
Örnek yapılandırmalarda, yükleme komutlarında vb. ASCS örneği 00 sayısı, ERS örneği 01, Birincil Uygulama örneği (PAS) 02 ve Uygulama örneği (AAS) 03'dür. SAP Sistem Kimliği QAS kullanılır. 

Veritabanı katmanı bu makalede ayrıntılı olarak ele alınmıyor.  

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun:

* [Azure NetApp Dosyaları belgeleri][anf-azure-doc] 
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

SAP Netweaver merkezi hizmetleri için yüksek kullanılabilirlik (HA) paylaşılan depolama gerektirir.
Red Hat Linux'ta bunu başarmak için şimdiye kadar ayrı yüksek kullanılabilir GlusterFS kümesi oluşturmak gerekiyordu. 

Artık Azure NetApp Dosyalarında dağıtılan paylaşılan depolama alanını kullanarak SAP Netweaver HA'ya ulaşmak mümkün. Paylaşılan depolama alanı için Azure NetApp Dosyalarını kullanmak ek [GlusterFS kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)ne kadar gereksinimi ortadan kaldırır. Kalp pili hala SAP Netweaver merkezi hizmetleri (ASCS / SCS) HA için gereklidir.

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-rhel/high-availability-guide-rhel-anf.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS ve SAP HANA veritabanı sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. [Standart yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)kullanmanızı öneririz. Aşağıdaki liste, (A)SCS ve ERS için ayrı ön uç IP'leri ile yük dengeleyicisinin yapılandırmasını gösterir.

### <a name="ascs"></a>(A) Scs

* Frontend yapılandırması
  * IP adresi 192.168.14.9
* Sonda Bağlantı Noktası
  * Bağlantı noktası 620<strong>&lt;nr&gt;</strong>
* Yük dengeleme kuralları
  * Standart Yük Dengeleyicisi kullanıyorsanız, **HA bağlantı noktalarını** seçin
  * 32<strong>&lt;nr&gt; </strong> TCP
  * 36<strong>&lt;nr&gt; </strong> TCP
  * 39<strong>&lt;nr&gt; </strong> TCP
  * 81<strong>&lt;nr&gt; </strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

### <a name="ers"></a>Ers

* Frontend yapılandırması
  * IP adresi 192.168.14.10
* Sonda Bağlantı Noktası
  * Bağlantı noktası 621<strong>&lt;nr&gt;</strong>
* Yük dengeleme kuralları
  * Standart Yük Dengeleyicisi kullanıyorsanız, **HA bağlantı noktalarını** seçin
  * 32<strong>&lt;nr&gt; </strong> TCP
  * 33<strong>&lt;nr&gt; </strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

* Arka uç yapılandırması
  * (A)SCS/ERS kümesinin bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Azure NetApp Dosyaları altyapısını ayarlama 

SAP NetWeaver, taşıma ve profil dizini için paylaşılan depolama gerektirir.  Azure NetApp dosyaları altyapısının kurulumuna geçmeden önce Azure [NetApp Dosyaları belgelerini][anf-azure-doc]edin. Seçtiğiniz Azure bölgesinin Azure NetApp Dosyaları sunsun da olmadığını kontrol edin. Aşağıdaki bağlantı, Azure bölgesine göre Azure NetApp Dosyalarının kullanılabilirliğini gösterir: [Azure Bölgesine göre Azure NetApp Dosyaları Kullanılabilirliği.][anf-avail-matrix]

Azure NetApp dosyaları çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/?products=netapp)kullanılabilir. Azure NetApp Dosyalarını dağıtmadan önce, [Azure NetApp dosyaları için Kaydol yönergelerini][anf-register]izleyerek Azure NetApp Dosyalarına binme isteğinde bulunun. 

### <a name="deploy-azure-netapp-files-resources"></a>Azure NetApp Dosyaları kaynaklarını dağıtma  

Adımlar, [Azure Sanal Ağı'nı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)zaten dağıtmış olduğunuzu varsayar. Azure NetApp Dosyaları kaynakları ve Azure NetApp Dosyaları kaynaklarının monte edildiği Sanal Kaynaklar aynı Azure Sanal Ağ'da veya eşlenen Azure Sanal Ağlarda dağıtılmalıdır.  

1. Bunu daha önce yapmadıysanız, [Azure NetApp Dosyalarına binme](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register)isteğinde bulunun.  
2. [NetApp Hesabı](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account)oluşturma yönergelerini izleyerek, seçili Azure bölgesinde NetApp hesabını oluşturun.  
3. Azure NetApp Dosyaları kapasite havuzunu nasıl ayarlayabildiğini anlatan yönergeleri izleyerek [Azure NetApp Dosyaları kapasite havuzunu ayarlayın.](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool)  
Bu makalede sunulan SAP Netweaver mimarisinde tek bir Azure NetApp Files kapasite havuzu olan Premium SKU kullanır. Azure'daki SAP Netweaver uygulaması iş yükü için Azure NetApp Files Premium SKU öneririz.  
4. Bir alt ağı, yönergelerde açıklandığı gibi [Azure NetApp dosyalarına bir alt net olarak devredin.](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet)  

5. [Azure NetApp Dosyaları için bir birim oluşturmak için yönergeleri izleyerek](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes)Azure NetApp Dosyaları birimlerini dağıtın. Birimleri belirlenen Azure NetApp Dosyaları [alt ağına](https://docs.microsoft.com/rest/api/virtualnetwork/subnets)dağıtın. Azure NetApp Dosyaları kaynaklarının ve Azure Sanal M'lerinin aynı Azure Sanal Ağ'da veya eşlenen Azure Sanal Ağlarda olması gerektiğini unutmayın. Bu örnekte iki Azure NetApp Files ciltleri kullanıyoruz: sap<b>QAS</b> ve transSAP. Karşılık gelen montaj noktalarına monte edilen dosya yolları /usrsap<b>qas</b>/sapmnt<b>QAS</b>, /usrsap<b>qas</b>/usrsap<b>QAS</b>sys, vb.  

   1. hacim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/ sapmnt<b>QAS</b>)
   2. hacim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/ usrsap<b>QAS</b>ascs)
   3. hacim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/ usrsap<b>QAS</b>sys)
   4. hacim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/ usrsap<b>QAS</b>ers)
   5. hacim transSAP (nfs://192.168.24.4/transSAP)
   6. hacim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/ usrsap<b>QAS</b>pas)
   7. hacim sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/ usrsap<b>QAS</b>aas)
  
Bu örnekte, Azure NetApp Dosyalarının nasıl kullanılabileceğini göstermek için tüm SAP Netweaver dosya sistemleri için Azure NetApp Dosyalarını kullandık. NFS üzerinden monte edilmesi gerekmeyen SAP dosya sistemleri de [Azure disk depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) olarak dağıtılabilir. Bu örnekte <b>a-e</b> Azure NetApp Files ve <b>f-g</b> (yani, /usr/sap/<b>QAS</b>/D<b>02</b>, /usr/sap/<b>QAS</b>/D<b>03)</b>üzerinde olmalıdır) Azure disk depolama alanı olarak dağıtılabilir. 

### <a name="important-considerations"></a>Önemli noktalar

Sap Netweaver için Azure NetApp Dosyalarını SUSE Yüksek Kullanılabilirlik mimarisinde değerlendirirken, aşağıdaki önemli hususlara dikkat edin:

- Minimum kapasite havuzu 4 TiB'dir. Kapasite havuzu boyutu 1 TiB artışlarla artırılabilir.
- Minimum hacim 100 GiB'dir
- Azure NetApp Dosyaları ve Azure NetApp Dosyaları birimlerinin monte edildiği tüm sanal makineler aynı Azure Sanal Ağ'da veya aynı bölgedeki [eşlenen sanal ağlarda](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) olmalıdır. Azure NetApp Files'ın aynı bölgedeki VNET'ten erişimi artık desteklendi. Azure NetApp erişimi küresel bakışüzerinden henüz desteklenmedi.
- Seçili sanal ağın Azure NetApp Dosyaları'na devredilen bir alt ağı olmalıdır.
- Azure NetApp Files [dışa aktarma politikası](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy)sunar: izin verilen istemcileri, erişim türünü (Yalnızca Okuma&, Yalnızca Oku vb.) kontrol edebilirsiniz. 
- Azure NetApp Files özelliği henüz bölge farkında değil. Şu anda Azure NetApp Dosyaları özelliği, bir Azure bölgesindeki tüm Kullanılabilirlik bölgelerinde dağıtılmaz. Bazı Azure bölgelerindeki olası gecikme sonu etkilerine dikkat edin. 
- Azure NetApp Dosyaları birimleri NFSv3 veya NFSv4.1 birimleri olarak dağıtılabilir. Her iki protokol de SAP uygulama katmanı (ASCS/ERS, SAP uygulama sunucuları) için desteklenir. 

## <a name="setting-up-ascs"></a>(A)SCS kurulumu

Bu örnekte, kaynaklar [Azure portalı](https://portal.azure.com/#home)üzerinden el ile dağıtıldı.

### <a name="deploy-linux-manually-via-azure-portal"></a>Azure portalı üzerinden Linux'u el ile dağıtın

Öncelikle Azure NetApp Dosyaları ciltlerini oluşturmanız gerekir. VM'leri dağıtın. Daha sonra, bir yük dengeleyici oluşturmak ve arka uç havuzunda sanal makineleri kullanın.

1. Yük dengeleyici (dahili, standart):  
   1. Ön uç IP adreslerini oluşturma
      1. ASCS için IP adresi 192.168.14.9
         1. Yük bakiyesini açın, frontend IP havuzunu seçin ve Ekle'yi tıklatın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **frontend. Qas. ASCS**)
         1. Atamayı Statik olarak ayarlayın ve IP adresini girin (örneğin **192.168.14.9**)
         1. Tamam'ı tıklatın
      1. ASCS ERS için IP adresi 192.168.14.10
         * ERS için bir IP adresi oluşturmak için yukarıdaki adımları "a" altında tekrarlayın (örneğin **192.168.14.10** ve **frontend. Qas. ERS**)
   1. Arka uç havuzunu oluşturma
      1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
      1. Yeni arka uç havuzunun adını girin (örneğin **arka uç. QAS**)
      1. Sanal makine ekle'yi tıklatın.
      1. Sanal makine'yi seçin. 
      1. (A)SCS kümesinin sanal makinelerini ve IP adreslerini seçin.
      1. Ekle'ye tıklayın.
   1. Sağlık sondalarını oluşturma
      1. ASCS için Bağlantı Noktası 620**00**
         1. Yük bakiyesini açın, sistem durumu sondalarını seçin ve Ekle'yi tıklatın
         1. Yeni sağlık sondasının adını girin (örneğin **sağlık. Qas. ASCS**)
         1. Protokol olarak TCP'yi seçin, bağlantı noktası 620**00,** Aralık 5'i ve Sağlıksız eşiği 2'yi koruyun
         1. Tamam'ı tıklatın
      1. ASCS ERS için Port 621**01**
            * ERS için bir sağlık sondası oluşturmak için yukarıdaki adımları "c" altında tekrarlayın (örneğin 621**01** ve **sağlık. Qas. ERS**)
   1. Yük dengeleme kuralları
      1. ASCS için yük dengeleme kuralları
         1. Yük bakiyesini açın, Yük dengeleme kurallarını seçin ve Ekle'yi tıklatın
         1. Yeni yük dengeleyici kuralının adını girin (örneğin **lb. Qas. ASCS**)
         1. Daha önce oluşturduğunuz ASCS, arka uç havuzu ve sistem durumu sondası için ön uç IP adresini seçin (örneğin **ön uç. Qas. ASCS**, **arka uç. QAS** ve **sağlık. Qas. ASCS**)
         1. **HA bağlantı noktalarını** seçin
         1. Boşta kalma süresini 30 dakikaya çıkarma
         1. **Kayan IP'yi etkinleştirdiğinden emin olun**
         1. Tamam'ı tıklatın
         * ERS için yük dengeleme kuralları oluşturmak için yukarıdaki adımları yineleyin (örneğin **lb. Qas. ERS**)
1. Alternatif olarak, senaryonuz temel yük dengeleyicisi (dahili) gerektiriyorsa, aşağıdaki adımları izleyin:  
   1. Ön uç IP adreslerini oluşturma
      1. ASCS için IP adresi 192.168.14.9
         1. Yük bakiyesini açın, frontend IP havuzunu seçin ve Ekle'yi tıklatın
         1. Yeni ön uç IP havuzunun adını girin (örneğin **frontend. Qas. ASCS**)
         1. Atamayı Statik olarak ayarlayın ve IP adresini girin (örneğin **192.168.14.9**)
         1. Tamam'ı tıklatın
      1. ASCS ERS için IP adresi 192.168.14.10
         * ERS için bir IP adresi oluşturmak için yukarıdaki adımları "a" altında tekrarlayın (örneğin **192.168.14.10** ve **frontend. Qas. ERS**)
   1. Arka uç havuzunu oluşturma
      1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
      1. Yeni arka uç havuzunun adını girin (örneğin **arka uç. QAS**)
      1. Sanal makine ekle'yi tıklatın.
      1. ASCS için daha önce oluşturduğunuz Kullanılabilirlik Kümesini seçin 
      1. (A)SCS kümesinin sanal makinelerini seçin
      1. Tamam'ı tıklatın
   1. Sağlık sondalarını oluşturma
      1. ASCS için Bağlantı Noktası 620**00**
         1. Yük bakiyesini açın, sistem durumu sondalarını seçin ve Ekle'yi tıklatın
         1. Yeni sağlık sondasının adını girin (örneğin **sağlık. Qas. ASCS**)
         1. Protokol olarak TCP'yi seçin, bağlantı noktası 620**00,** Aralık 5'i ve Sağlıksız eşiği 2'yi koruyun
         1. Tamam'ı tıklatın
      1. ASCS ERS için Port 621**01**
            * ERS için bir sağlık sondası oluşturmak için yukarıdaki adımları "c" altında tekrarlayın (örneğin 621**01** ve **sağlık. Qas. ERS**)
   1. Yük dengeleme kuralları
      1. ASCS için 32**00** TCP
         1. Yük bakiyesini açın, Yük dengeleme kurallarını seçin ve Ekle'yi tıklatın
         1. Yeni yük dengeleyici kuralının adını girin (örneğin **lb. Qas. ASCS.3200**)
         1. Daha önce oluşturduğunuz ASCS, arka uç havuzu ve sistem durumu sondası için ön uç IP adresini seçin (örneğin **ön uç. Qas. ASCS**)
         1. Protokol **TCP**tutun , port **3200** girin
         1. Boşta kalma süresini 30 dakikaya çıkarma
         1. **Kayan IP'yi etkinleştirdiğinden emin olun**
         1. Tamam'ı tıklatın
      1. ASCS için ek bağlantı noktaları
         * ASCS için 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 ve TCP bağlantı noktaları için yukarıdaki adımları tekrarlayın
      1. ASCS ERS için ek bağlantı noktaları
         * ASCS ERS için 32**01**, 33**01**, 5**01**13, 5**01**14, 5**01**16 ve TCP bağlantı noktaları için yukarıdaki adımları "d" altında tekrarlayın

      > [!Note]
      > Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ortak bitiş noktalarına yönlendirmeye izin verecek ek yapılandırma yapılmadığı sürece giden internet bağlantısı olmaz. Giden bağlantının nasıl elde edilene ilişkin ayrıntılar [için, SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için Genel uç nokta bağlantısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections)bakın.  

      > [!IMPORTANT]
      > Azure Yük Bakiyesi'nin arkasına yerleştirilen Azure VM'lerinde TCP zaman damgalarını etkinleştirme. TCP zaman damgalarını etkinleştirmek sistem durumu sondalarının başarısız lığa neden olur. Parametre **net.ipv4.tcp_timestamps** **0**'a ayarlayın. Ayrıntılar için [Bkz. Yük Dengeleyici sağlık probları.](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

## <a name="disable-id-mapping-if-using-nfsv41"></a>Kimlik eşlemi devre dışı (NFSv4.1 kullanıyorsanız)

Bu bölümdeki talimatlar yalnızca NFSv4.1 protokolüne sahip Azure NetApp Files birimleri kullanılarak uygulanabilir. Azure NetApp Files NFSv4.1 birimlerinin monte edileceği tüm VM'lerde yapılandırmayı gerçekleştirin.  

1. NFS etki alanı ayarını doğrulayın. Etki alanının varsayılan Azure NetApp Files etki alanı olarak yapılandırıldığından, yani **`defaultv4iddomain.com`** eşlemenin hiç **kimseye**ayarlı olmadığından emin olun.  

    > [!IMPORTANT]
    > NFS etki alanını `/etc/idmapd.conf` Azure NetApp Dosyaları'ndaki varsayılan etki alanı yapılandırmasına **`defaultv4iddomain.com`** uyacak şekilde VM'de ayarladığından emin olun: . NFS istemcisindeki etki alanı yapılandırması (yani VM) ile NFS sunucusu, yani Azure NetApp yapılandırması arasında bir uyuşmazlık varsa, Azure NetApp birimlerinde VM'lere monte `nobody`edilen dosyaların izinleri görüntülenir.  

    <pre><code>
    sudo cat /etc/idmapd.conf
    # Example
    [General]
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
    mount 192.168.24.5:/sap<b>QAS</b>
    umount  /mnt/tmp
    echo "Y" > /sys/module/nfs/parameters/nfs4_disable_idmapping
    # Make the configuration permanent
    echo "options nfs nfs4_disable_idmapping=Y" >> /etc/modprobe.d/nfs.conf
    </code></pre>

   Parametrenin nasıl değiştirilebildiğini `nfs4_disable_idmapping` https://access.redhat.com/solutions/1749883hakkında daha fazla bilgi için bkz.

### <a name="create-pacemaker-cluster"></a>Kalp Pili kümesi ni oluştur

Bu (A)SCS sunucusu için temel bir Pacemaker kümesi oluşturmak için [Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama](high-availability-guide-rhel-pacemaker.md) adımlarını izleyin.

### <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver kurulumu için hazırlanın

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

    ```
    sudo vi /etc/hosts
    ```

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme

    ```
    # IP address of cluster node 1
    192.168.14.5    anftstsapcl1
    # IP address of cluster node 2
    192.168.14.6     anftstsapcl2
    # IP address of the load balancer frontend configuration for SAP Netweaver ASCS
    192.168.14.9    anftstsapvh
    # IP address of the load balancer frontend configuration for SAP Netweaver ERS
    192.168.14.10    anftstsapers
    ```

1. **[1]** Azure NetApp Dosyaları hacminde SAP dizinleri oluşturun.  
   Azure NetApp Dosyaları hacmini geçici olarak VM'lerden birine monte edin ve SAP dizinlerini (dosya yolları) oluşturun.  

    ```
     # mount temporarily the volume
     sudo mkdir -p /saptmp
     # If using NFSv3
     sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 192.168.24.5:/sapQAS /saptmp
     # If using NFSv4.1
     sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys,tcp 192.168.24.5:/sapQAS /saptmp
     # create the SAP directories
     sudo cd /saptmp
     sudo mkdir -p sapmntQAS
     sudo mkdir -p usrsapQASascs
     sudo mkdir -p usrsapQASers
     sudo mkdir -p usrsapQASsys
     sudo mkdir -p usrsapQASpas
     sudo mkdir -p usrsapQASaas
     # unmount the volume and delete the temporary directory
     sudo cd ..
     sudo umount /saptmp
     sudo rmdir /saptmp
    ``` 

1. **[A]** Paylaşılan dizinleri oluşturma

   ```
   sudo mkdir -p /sapmnt/QAS
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/QAS/SYS
   sudo mkdir -p /usr/sap/QAS/ASCS00
   sudo mkdir -p /usr/sap/QAS/ERS01
   
   sudo chattr +i /sapmnt/QAS
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/QAS/SYS
   sudo chattr +i /usr/sap/QAS/ASCS00
   sudo chattr +i /usr/sap/QAS/ERS01
   ```

1. **[A]** NFS istemcisi ve diğer gereksinimleri yükleyin

   ```
   sudo yum -y install nfs-utils resource-agents resource-agents-sap
   ```

1. **[A]** Kaynak-aracılar-sap sürümünü denetleyin

   Yüklenen kaynak-aracılar-sap paketinin sürümünün en az 3.9.5-124.el7 olduğundan emin olun
   ```
   sudo yum info resource-agents-sap
   
   # Loaded plugins: langpacks, product-id, search-disabled-repos
   # Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
   # Installed Packages
   # Name        : resource-agents-sap
   # Arch        : x86_64
   # Version     : 3.9.5
   # Release     : 124.el7
   # Size        : 100 k
   # Repo        : installed
   # From repo   : rhel-sap-for-rhel-7-server-rpms
   # Summary     : SAP cluster resource agents and connector script
   # URL         : https://github.com/ClusterLabs/resource-agents
   # License     : GPLv2+
   # Description : The SAP resource agents and connector script interface with
   #          : Pacemaker to allow SAP instances to be managed in a cluster
   #          : environment.
   ```


1. **[A]** Montaj girişleri ekleme

   NFSv3 kullanıyorsanız:
   ```
   sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
    192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=3
    192.168.24.5:/sapQAS/usrsapQASsys /usr/sap/QAS/SYS nfs rw,hard,rsize=65536,wsize=65536,vers=3
    192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=3
   ```

   NFSv4.1 kullanıyorsanız:
   ```
   sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
    192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
    192.168.24.5:/sapQAS/usrsapQASsys /usr/sap/QAS/SYS nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
    192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
   ```

   > [!NOTE]
   > Birimleri monte ederken Azure NetApp Dosyaları birimlerinin NFS protokol sürümüyle eşleştiğinden emin olun. Azure NetApp Dosyaları birimleri NFSv3 birimleri olarak oluşturulursa, ilgili NFSv3 yapılandırmasını kullanın. Azure NetApp Dosyaları birimleri NFSv4.1 birimleri olarak oluşturulursa, kimlik eşlemesini devre dışı bırakıp ilgili NFSv4.1 yapılandırmasını kullandığınızdan emin olun yönergeleri izleyin. Bu örnekte Azure NetApp Dosyaları birimleri NFSv3 birimleri olarak oluşturulmuştur.  

   Yeni paylaşımları monte edin

   ```
   sudo mount -a  
   ```

1. **[A]** SWAP dosyalarını yapılandırma

   ```
   sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=y
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=2000
   ```

   Değişikliği etkinleştirmek için Aracıyı yeniden başlatın

   ```
   sudo service waagent restart
   ```

1. **[A]** RHEL yapılandırması

   SAP Note [2002167'de] açıklandığı şekilde RHEL'i yapılandırın

### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASCS/ERS kurulumu

1. **[1]** ASCS örneği için sanal bir IP kaynağı ve sistem durumu sondası oluşturma

   ```
   sudo pcs node standby anftstsapcl2
   # If using NFSv3
   sudo pcs resource create fs_QAS_ASCS Filesystem device='192.168.24.5:/sapQAS/usrsapQASascs' \
     directory='/usr/sap/QAS/ASCS00' fstype='nfs' force_unmount=safe \
     op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
     --group g-QAS_ASCS
   
   # If using NFSv4.1
   sudo pcs resource create fs_QAS_ASCS Filesystem device='192.168.24.5:/sapQAS/usrsapQASascs' \
     directory='/usr/sap/QAS/ASCS00' fstype='nfs' force_unmount=safe options='sec=sys,vers=4.1' \
     op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
     --group g-QAS_ASCS
   
   sudo pcs resource create vip_QAS_ASCS IPaddr2 \
     ip=192.168.14.9 cidr_netmask=24 \
     --group g-QAS_ASCS
   
   sudo pcs resource create nc_QAS_ASCS azure-lb port=62000 \
     --group g-QAS_ASCS
   ```

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   ```
   sudo pcs status
   
   # Node anftstsapcl2: standby
   # Online: [ anftstsapcl1 ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
   #  Resource Group: g-QAS_ASCS
   #      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
   #      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
   #      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
   ```

1. **[1]** SAP NetWeaver ASCS yükleyin  

   SAP NetWeaver ASCS'yi, ascs için yük dengeleyici ön uç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adı kullanarak ilk düğüme kök olarak yükleyin, örneğin <b>anftstsapvh</b>, <b>192.168.14.9</b> ve yük bakiyesi probiçin kullandığınız örnek numarası, örneğin <b>00</b>.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   ```
   # Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp
   
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=<virtual_hostname>
   ```

   Yükleme /usr/sap/**QAS**/ASCS**00'da**bir alt klasör oluşturamazsa, ASCS**00** klasörünün sahibini ve grubunu ayarlamayı deneyin ve yeniden deneyin.

   ```
   sudo chown qasadm /usr/sap/QAS/ASCS00
   sudo chgrp sapsys /usr/sap/QAS/ASCS00
   ```

1. **[1]** ERS örneği için sanal bir IP kaynağı ve sistem durumu sondası oluşturma

   ```
   sudo pcs node unstandby anftstsapcl2
   sudo pcs node standby anftstsapcl1
   
   # If using NFSv3
   sudo pcs resource create fs_QAS_AERS Filesystem device='192.168.24.5:/sapQAS/usrsapQASers' \
     directory='/usr/sap/QAS/ERS01' fstype='nfs' force_unmount=safe \
     op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
    --group g-QAS_AERS
   
   # If using NFSv4.1
   sudo pcs resource create fs_QAS_AERS Filesystem device='192.168.24.5:/sapQAS/usrsapQASers' \
     directory='/usr/sap/QAS/ERS01' fstype='nfs' force_unmount=safe options='sec=sys,vers=4.1' \
     op start interval=0 timeout=60 op stop interval=0 timeout=120 op monitor interval=200 timeout=40 \
    --group g-QAS_AERS
   
   sudo pcs resource create vip_QAS_AERS IPaddr2 \
     ip=192.168.14.10 cidr_netmask=24 \
    --group g-QAS_AERS
   
   sudo pcs resource create nc_QAS_AERS azure-lb port=62101 \
    --group g-QAS_AERS
   ```
 
   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

   ```
   sudo pcs status
   
   # Node anftstsapcl1: standby
   # Online: [ anftstsapcl2 ]
   #
   # Full list of resources:
   #
   # rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl2
   #  Resource Group: g-QAS_ASCS
   #      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
   #      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2<
   #      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
   #  Resource Group: g-QAS_AERS
   #      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
   #      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
   #      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
   ```

1. **[2]** SAP NetWeaver ERS'i yükleyin  

   SAP NetWeaver ERS'i, örneğin <b>anftstsapers</b>, <b>192.168.14.10</b> ve yük dengeleyicisinin sondası için kullandığınız örnek numarası, örneğin <b>01</b>.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   ```
   # Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp

   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=<virtual_hostname>
   ```

   Yükleme /usr/sap/**QAS**/ERS**01'de**bir alt klasör oluşturamazsa, ERS**01** klasörünün sahibini ve grubunu ayarlamayı deneyin ve yeniden deneyin.

   ```
   sudo chown qaadm /usr/sap/QAS/ERS01
   sudo chgrp sapsys /usr/sap/QAS/ERS01
   ```

1. **[1]** ASCS/SCS ve ERS örnek profillerini uyarlama

   * ASCS/SCS profili

   ```
   sudo vi /sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh
   
   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   ```

   * ERS profili

   ```
   sudo vi /sapmnt/QAS/profile/QAS_ERS01_anftstsapers
   
   # Change the restart command to a start command
   #Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # remove Autostart from ERS profile
   # Autostart = 1
   ```


1. **[A]** Canlı Tutun'u Yapılandır

   SAP NetWeaver uygulama sunucusu ile ASCS/SCS arasındaki iletişim bir yazılım yük dengeleyicisi aracılığıyla yönlendirilir. Yük dengeleyici, yapılandırılabilir bir zaman anından sonra etkin olmayan bağlantıları keser. Bunu önlemek için SAP NetWeaver ASCS/SCS profilinde bir parametre ayarlamanız ve Linux sistem ayarlarını değiştirmeniz gerekir. Daha fazla bilgi için [SAP Note 1410736'yı][1410736] okuyun.

   ASCS/SCS profil parametresi enque/encni/set_so_keepalive zaten son adımda eklendi.

   ```
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   ```

1. **[A]** /usr/sap/sapservices dosyasını güncelleştir

   Sapinit başlangıç komut dosyası tarafından örneklerin başlatılmasını önlemek için, Pacemaker tarafından yönetilen tüm örnekler /usr/sap/sapservices dosyasından yorumlanmalıdır. HANA SR ile kullanılacaksa SAP HANA örneğini açıklamayın.

   ```
   sudo vi /usr/sap/sapservices
   
   # On the node where you installed the ASCS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/QAS/ASCS00/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/QAS/ASCS00/exe/sapstartsrv pf=/usr/sap/QAS/SYS/profile/QAS_ASCS00_anftstsapvh -D -u qasadm
   
   # On the node where you installed the ERS, comment out the following line
   # LD_LIBRARY_PATH=/usr/sap/QAS/ERS01/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/QAS/ERS01/exe/sapstartsrv pf=/usr/sap/QAS/ERS01/profile/QAS_ERS01_anftstsapers -D -u qasadm
   ```

1. **[1]** SAP küme kaynaklarını oluşturma

   Enqueue server 1 mimarisi (ENSA1) kullanıyorsanız, kaynakları aşağıdaki gibi tanımlayın:

   ```
   sudo pcs property set maintenance-mode=true
   
    sudo pcs resource create rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName=QAS_ASCS00_anftstsapvh START_PROFILE="/sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-QAS_ASCS
   
    sudo pcs resource create rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName=QAS_ERS01_anftstsapers START_PROFILE="/sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-QAS_AERS
      
    sudo pcs constraint colocation add g-QAS_AERS with g-QAS_ASCS -5000
    sudo pcs constraint location rsc_sap_QAS_ASCS00 rule score=2000 runs_ers_QAS eq 1
    sudo pcs constraint order g-QAS_ASCS then g-QAS_AERS kind=Optional symmetrical=false
    
    sudo pcs node unstandby anftstsapcl1
    sudo pcs property set maintenance-mode=false
    ```

   SAP, SAP NW 7.52 itibariyle çoğaltma da dahil olmak üzere enqueue server 2 için destek sundu. ABAP Platform 1809 ile başlayarak, enqueue server 2 varsayılan olarak yüklenir. Enqueue server 2 desteği için SAP note [2630416'ya](https://launchpad.support.sap.com/#/notes/2630416) bakın.
   Enqueue server 2 mimarisi[(ENSA2)](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)kullanıyorsanız, kaynak aracısı kaynak-aracı-sap-4.1.1-12.el7.x86_64 veya daha yeni yükleyin ve kaynakları aşağıdaki gibi tanımlayın:

    ```
    sudo pcs property set maintenance-mode=true
    
    sudo pcs resource create rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName=QAS_ASCS00_anftstsapvh START_PROFILE="/sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 failure-timeout=60 \
    op monitor interval=20 on-fail=restart timeout=60 \
    op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-QAS_ASCS
   
    sudo pcs resource create rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName=QAS_ERS01_anftstsapers START_PROFILE="/sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    op monitor interval=20 on-fail=restart timeout=60 op start interval=0 timeout=600 op stop interval=0 timeout=600 \
    --group g-QAS_AERS
      
    sudo pcs constraint colocation add g-QAS_AERS with g-QAS_ASCS -5000
    sudo pcs constraint order g-QAS_ASCS then g-QAS_AERS kind=Optional symmetrical=false
    sudo pcs constraint order start g-QAS_ASCS then stop g-QAS_AERS symmetrical=false
   
    sudo pcs node unstandby anftstsapcl1
    sudo pcs property set maintenance-mode=false
    ```

   Eski bir sürümden yükseltiyor ve enqueue server 2'ye geçiyorsanız, SAP note [2641322'ye](https://launchpad.support.sap.com/#/notes/2641322)bakın. 

   > [!NOTE]
   > Yukarıdaki yapılandırmadaki zaman ekmeleri yalnızca örneklerdir ve belirli SAP kurulumuna uyarlanmış olması gerekebilir. 

   Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıkolduğundan emin olun. Hangi düğümün hangi düğümün çalıştığının önemli değildir.

    ```
    sudo pcs status
    
    # Online: [ anftstsapcl1 anftstsapcl2 ]
    #
    # Full list of resources:
    #
    # rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl2
    #  Resource Group: g-QAS_ASCS
    #      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
    #      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
    #      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
    #      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
    #  Resource Group: g-QAS_AERS
    #      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
    #      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
    #      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
    #      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

1. **[A]** Her iki düğümde DE ASCS ve ERS için güvenlik duvarı kuralları ekleyin Her iki düğümde de ASCS ve ERS için güvenlik duvarı kurallarını ekleyin.
   ```
   # Probe Port of ASCS
   sudo firewall-cmd --zone=public --add-port=62000/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=62000/tcp
   sudo firewall-cmd --zone=public --add-port=3200/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=3200/tcp
   sudo firewall-cmd --zone=public --add-port=3600/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=3600/tcp
   sudo firewall-cmd --zone=public --add-port=3900/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=3900/tcp
   sudo firewall-cmd --zone=public --add-port=8100/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=8100/tcp
   sudo firewall-cmd --zone=public --add-port=50013/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=50013/tcp
   sudo firewall-cmd --zone=public --add-port=50014/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=50014/tcp
   sudo firewall-cmd --zone=public --add-port=50016/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=50016/tcp
   # Probe Port of ERS
   sudo firewall-cmd --zone=public --add-port=62101/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=62101/tcp
   sudo firewall-cmd --zone=public --add-port=3301/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=3301/tcp
   sudo firewall-cmd --zone=public --add-port=50113/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=50113/tcp
   sudo firewall-cmd --zone=public --add-port=50114/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=50114/tcp
   sudo firewall-cmd --zone=public --add-port=50116/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=50116/tcp
   ```

## <a name="sap-netweaver-application-server-preparation"></a><a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver uygulama sunucusu hazırlama

   Bazı veritabanları veritabanı örneği yüklemebir uygulama sunucusunda yürütülmesini gerektirir. Bu gibi durumlarda bunları kullanabilmek için uygulama sunucusu sanal makineleri hazırlayın.  

   Adım ları körüklüyor, uygulama sunucusunu ASCS/SCS ve HANA sunucularından farklı bir sunucuya yüklediğinizi varsayar. Aksi takdirde aşağıdaki adımlardan bazıları (ana bilgisayar ad çözümlemesi yapılandırma gibi) gerekli değildir.  

   Aşağıdaki öğeler, hem PAS hem de AAS için geçerli olan **[A]** ile önceden belirlenmiştir, **[P] -** yalnızca PAS veya **[S]** için geçerlidir - yalnızca AAS için geçerlidir.  

1. **[A]** Kurulum ana bilgisayar ad çözünürlüğü Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirin:  

   ```
   sudo vi /etc/hosts
   ```

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirin.

   ```
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   192.168.14.9 anftstsapvh
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   192.168.14.10 anftstsapers
   192.168.14.7 anftstsapa01
   192.168.14.8 anftstsapa02
   ```

1. **[A]** Sapmnt dizini oluşturun Sapmnt dizini oluşturun.
   ```
   sudo mkdir -p /sapmnt/QAS
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/QAS
   sudo chattr +i /usr/sap/trans
   ```

1. **[A]** NFS istemcisi ve diğer gereksinimleri yükleyin  

   ```
   sudo yum -y install nfs-utils uuidd
   ```

1. **[A]** Montaj girişleri ekleme  
   NFSv3 kullanıyorsanız:
   ```
   sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=3
   192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=3
   ```

   NFSv4.1 kullanıyorsanız:
   ```
   sudo vi /etc/fstab
   
   # Add the following lines to fstab, save and exit
   192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
   192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
   ```

   Yeni paylaşımları monte edin

   ```
   sudo mount -a
   ```

1. **[P]** PAS dizinini oluşturun ve monte edin  
   NFSv3 kullanıyorsanız:
   ```
   sudo mkdir -p /usr/sap/QAS/D02
   sudo chattr +i /usr/sap/QAS/D02
   
   sudo vi /etc/fstab
   # Add the following line to fstab
   92.168.24.5:/sapQAS/usrsapQASpas /usr/sap/QAS/D02 nfs rw,hard,rsize=65536,wsize=65536,vers=3
   
   # Mount
   sudo mount -a
   ```

   NFSv4.1 kullanıyorsanız:
   ```
   sudo mkdir -p /usr/sap/QAS/D02
   sudo chattr +i /usr/sap/QAS/D02
   
   sudo vi /etc/fstab
   # Add the following line to fstab
   92.168.24.5:/sapQAS/usrsapQASpas /usr/sap/QAS/D02 nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
   
   # Mount
   sudo mount -a
   ```

1. **[S]** AAS dizinini oluşturun ve monte edin  
   NFSv3 kullanıyorsanız:
   ```
   sudo mkdir -p /usr/sap/QAS/D03
   sudo chattr +i /usr/sap/QAS/D03
   
   sudo vi /etc/fstab
   # Add the following line to fstab
   92.168.24.5:/sapQAS/usrsapQASaas /usr/sap/QAS/D03 nfs rw,hard,rsize=65536,wsize=65536,vers=3
   
   # Mount
   sudo mount -a
   ```

   NFSv4.1 kullanıyorsanız:
   ```
   sudo mkdir -p /usr/sap/QAS/D03
   sudo chattr +i /usr/sap/QAS/D03
   
   sudo vi /etc/fstab
   # Add the following line to fstab
   92.168.24.5:/sapQAS/usrsapQASaas /usr/sap/QAS/D03 nfs rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys
   
   # Mount
   sudo mount -a
   ```

1. **[A]** SWAP dosyalarını yapılandırma
 
   ```
   sudo vi /etc/waagent.conf
   
   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=y
   
   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=2000
   ```

   Değişikliği etkinleştirmek için Aracıyı yeniden başlatın

   ```
   sudo service waagent restart
   ```

## <a name="install-database"></a>Veritabanını yükleme

Bu örnekte, SAP NetWeaver SAP HANA'ya yüklenir. Bu yükleme için desteklenen tüm veritabanını kullanabilirsiniz. AZURE'da SAP HANA'nın nasıl yüklenir hakkında daha fazla bilgi için Red Hat Enterprise Linux'taki. For a list of supported databases, see [SAP Note 1928533][1928533] [Azure VM'lerinde SAP HANA'nın yüksek kullanılabilirliği][sap-hana-ha]bölümüne bakın.

1. SAP veritabanı örneği yüklemesini çalıştırma

   SAP NetWeaver veritabanı örneğini, veritabanı için yük dengeleyici ön uç yapılandırmasının IP adresine eşleyen sanal bir ana bilgisayar adı kullanarak kök olarak yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin
   ```

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver uygulama sunucusu kurulumu

SAP uygulama sunucusu yüklemek için aşağıdaki adımları izleyin.

1. Uygulama sunucusu hazırlama

   Uygulama sunucusu hazırlamak için yukarıdaki bölümde [SAP NetWeaver uygulama sunucusu hazırlama](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) adımları izleyin.

1. SAP NetWeaver uygulama sunucusunu kurun

   Birincil veya ek SAP NetWeaver uygulamaları sunucusu yükleyin.

   Kök olmayan bir kullanıcının sapinst'e bağlanmasına izin vermek için SAPINST_REMOTE_ACCESS_USER sapinst parametresini kullanabilirsiniz.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin
   ```

1. SAP HANA güvenli mağazayı güncelleştirin

   SAP HANA güvenli deposunu, SAP HANA Sistem Çoğaltma kurulumunun sanal adını belirtmek için güncelleştirin.

   Girişleri sapsid>adm \<olarak listelemek için aşağıdaki komutu çalıştırın

   ```
   hdbuserstore List
   ```

   Bu, tüm girişleri listelemelidir ve
   ```
   DATA FILE       : /home/qasadm/.hdb/anftstsapa01/SSFS_HDB.DAT
   KEY FILE        : /home/qasadm/.hdb/anftstsapa01/SSFS_HDB.KEY
   
   KEY DEFAULT
     ENV : 192.168.14.4:30313
     USER: SAPABAP1
     DATABASE: QAS
   ```

   Çıktı, varsayılan girişin IP adresinin yük bakiyesinin IP adresine değil, sanal makineyi işaret ettiğini gösterir. Bu giriş, yük bakiyesi sanal ana bilgisayar adını işaret etmek için değiştirilmesi gerekir. Aynı bağlantı noktası (yukarıdaki çıktıda**30313)** ve veritabanı adını (yukarıdaki çıktıda**QAS)** kullandığınızdan emin olun!

   ```
   su - qasadm
   hdbuserstore SET DEFAULT qasdb:30313@QAS SAPABAP1 <password of ABAP schema>
   ```

## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

1. ASCS örneğini el ile geçirin

   Teste başlamadan önce kaynak durumu:

   ```
    rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

   ASCS örneğini geçirmek için aşağıdaki komutları kök olarak çalıştırın.

   ```
   [root@anftstsapcl1 ~]# pcs resource move rsc_sap_QAS_ASCS00
   
   [root@anftstsapcl1 ~]# pcs resource clear rsc_sap_QAS_ASCS00
   
   # Remove failed actions for the ERS that occurred as part of the migration
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Testten sonra kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

1. Düğüm çökmesini simüle edin

   Teste başlamadan önce kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

   ASCS örneğinin çalıştığı düğümde aşağıdaki komutu kök olarak çalıştırın

   ```
   [root@anftstsapcl2 ~]# echo b > /proc/sysrq-trigger
   ```

   Düğüm yeniden başlatıldıktan sonraki durum aşağıdaki gibi görünmelidir.

   ```
   Online: [ anftstsapcl1 anftstsapcl2 ]
   
   Full list of resources:
   
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   
   Failed Actions:
   * rsc_sap_QAS_ERS01_monitor_11000 on anftstsapcl1 'not running' (7): call=45, status=complete, exitreason='',
   ```

   Başarısız kaynakları temizlemek için aşağıdaki komutu kullanın.

   ```
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Testten sonra kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

1. İleti sunucusu işlemini öldürme

   Teste başlamadan önce kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```
   
   İleti sunucusunun işlemini tanımlamak ve öldürmek için aşağıdaki komutları kök olarak çalıştırın.

   ```
   [root@anftstsapcl1 ~]# pgrep ms.sapQAS | xargs kill -9
   ```

   İleti sunucusunu yalnızca bir kez öldürürseniz, `sapstart`ileti sunucu tarafından yeniden başlatılır. Yeterince sık öldürürseniz, Pacemaker sonunda diğer düğüm için ASCS örneğini taşıyacak. Testten sonra ASCS ve ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   ```
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ASCS00
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Testten sonra kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

1. Enqueue sunucu işlemini öldür

   Teste başlamadan önce kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

   Aşağıdaki komutları, ASCS örneğinin enqueue sunucusunu öldürmek için çalıştığı düğümde kök olarak çalıştırın.

   ```
   [root@anftstsapcl2 ~]# pgrep en.sapQAS | xargs kill -9
   ```

   ASCS örneği hemen diğer düğüm üzerinde başarısız olmalıdır. ASCS örneği başladıktan sonra ERS örneği de başarısız olmalıdır. Testten sonra ASCS ve ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   ```
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ASCS00
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Testten sonra kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

1. Enqueue çoğaltma sunucu işlemini öldür

   Teste başlamadan önce kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

   Aşağıdaki komutu, ERS örneğinin enqueue çoğaltma sunucu işlemini öldürmek için çalıştığı düğümde kök olarak çalıştırın.

   ```
   [root@anftstsapcl2 ~]# pgrep er.sapQAS | xargs kill -9
   ```

   Komutu yalnızca bir kez `sapstart` çalıştırsanız, işlemi yeniden başlatın. Yeterince sık çalıştırArsanız, `sapstart` işlemi yeniden başlatmaz ve kaynak durdurulmuş durumda olur. Testten sonra ERS örneğinin kaynak durumunu temizlemek için aşağıdaki komutları kök olarak çalıştırın.

   ```
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Testten sonra kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

1. Enqueue sapstartsrv işlemini öldür

   Teste başlamadan önce kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

   ASCS'nin çalıştığı düğümde aşağıdaki komutları kök olarak çalıştırın.

   ```
   [root@anftstsapcl1 ~]# pgrep -fl ASCS00.*sapstartsrv
   # 59545 sapstartsrv
   
   [root@anftstsapcl1 ~]# kill -9 59545
   ```

   Sapstartsrv işlemi her zaman izlemenin bir parçası olarak Pacemaker kaynak aracısı tarafından yeniden başlatılmalıdır. Testten sonra kaynak durumu:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
    Resource Group: g-QAS_ASCS
        fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
        nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
        vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
        rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
    Resource Group: g-QAS_AERS
        fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
        nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
        vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
        rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [SAP uygulamaları multi-SID kılavuzu için RHEL'de Azure VM'lerde SAP NW için HA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid)
* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik oluşturmayı ve AZURE'da SAP HANA'nın olağanüstü durum kurtarmasını nasıl planlayabilirsiniz (büyük örnekler), Bkz. [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve Azure'da olağanüstü durum kurtarma.](hana-overview-high-availability-disaster-recovery.md)
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
