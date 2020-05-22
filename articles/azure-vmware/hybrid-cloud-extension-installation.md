---
title: Karma bulut uzantısı 'nı (HCX) yükler
description: Azure VMWare çözümünüz (AVS) özel bulutunuz için VMWare hibrit bulut uzantısı (HCX) çözümünü ayarlama
ms.topic: how-to
ms.date: 05/19/2020
ms.openlocfilehash: 74bc0dece2fc565aebc6e5301ef7a36187d6a250
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83776478"
---
# <a name="install-hcx-for-azure-vmware-solution"></a>Azure VMWare çözümü için HCX 'i yükler

Bu makalede, Azure VMWare çözümünüz (AVS) özel bulutunuz için VMWare hibrit bulut uzantısı (HCX) çözümünü ayarlama yordamlarına ilerliyoruz. HCX Gelişmiş (varsayılan yükleme), her bir dış sitenin bir HCX Enterprise Manager veya bağlayıcısının yüklü ve etkin olmasını gerektiren en fazla üç dış siteyi destekler.
HCX, VMware iş yüklerinizin farklı yerleşik HCX desteklenen geçiş türleri aracılığıyla buluta ve diğer bağlı sitelere geçirilmesini sağlar. Üçten fazla site gerekliyse, müşteriler destek aracılığıyla HCX kurumsal eklentisini etkinleştirme seçeneğine sahiptir. HCX kurumsal, genel kullanılabilirlik (GA) sonrasında müşterilere ek ücretler taşır, ancak [ek özellikler](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/)sağlar.

[Başlamadan önce](#before-you-begin), [yazılım sürümü gereksinimlerinden](#software-version-requirements)ve [önkoşullardan](#prerequisites) önce ayrıntılı bir şekilde gözden geçirin. 

Ardından, aşağıdakileri yapmak için gereken tüm yordamları ele aldık:

> [!div class="checklist"]
> * Şirket içi HCX OVA 'yı dağıtma
> * HCX 'i etkinleştirme ve yapılandırma
> * Ağ yukarı ve hizmet kafesi yapılandırma
> * Gereç durumunu denetleyerek kurulumu tamamlayarak

Kurulumu tamamladıktan sonra önerilen sonraki adımlar sağlanır.

## <a name="before-you-begin"></a>Başlamadan önce
    
* Temel AVS yazılım tanımlı veri merkezi (SDDC) [öğretici serisini](tutorial-network-checklist.md) gözden geçirin
* HCX Kullanıcı Kılavuzu dahil [VMware HCX belgelerini](https://docs.vmware.com/en/VMware-HCX/index.html) gözden geçirin ve başvurun
* VMware [HCX Ile sanal makineleri geçirme](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) VMware docs
* İsteğe bağlı olarak [VMware HCX dağıtım konularını](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html) gözden geçirin
* HCX üzerinde VMware vSphere [blog serisi](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) gibi, isteğe bağlı olarak HCX üzerinde ilgili VMware malzemeleri gözden geçirin. 
* AVS 'yi kullanarak bir AVS HCX Kurumsal etkinleştirmesi siparişi, kanalları destekler.

Çalışma yüklerini işlem ve depolama kaynaklarına karşı boyutlandırma, AVS özel bulut HCX çözümünü kullanmaya hazırlanırken önemli bir planlama adımıdır. Bu boyutlandırma adımı, ilk özel bulut ortamı planlamasının parçası olarak değinilmesi gerekir. 

## <a name="software-version-requirements"></a>Yazılım sürümü gereksinimleri
Altyapı bileşenleri, gerekli en düşük sürümü çalıştırıyor olmalıdır. 
                                                         
| Bileşen türü                                                          | Kaynak ortam gereksinimleri                                                                   | Hedef ortam gereksinimleri                                                                      |
| --- | --- | --- |
| vCenter Server                                                          | 5.1<br/><br/>5,5 U1 veya daha önceki bir sürümü kullanıyorsanız HCX işlemleri için tek başına HCX Kullanıcı arabirimini kullanın.         | 6,0 U2 ve üzeri                                                                                          |
| ESXi                                                                    | 5.0                                                                                               | ESXi 6,0 ve üzeri                                                                                        |
| NSX                                                                     | Kaynaktaki mantıksal anahtarların HCX ağ uzantısı için: NSXv 6.2 + veya NSX-T 2.4 +              | NSXv 6.2 + veya NSX-T 2,4 +<br/><br/HCX yakınlık yönlendirmesi: NSXv 6.4 + (NSX-T ile yakınlık yönlendirmesi desteklenmez) |
| vCloud Direktörü                                                         | Gerekli değildir-kaynak sitede vCloud Director ile birlikte çalışabilirlik yok | Hedef ortam vCloud Director ile tümleştirildiğinde, en az 9.1.0.2 olur.              |

## <a name="prerequisites"></a>Ön koşullar

* Şirket içi ve AVS SDDC ER devreleri arasında küresel erişim yapılandırılmalıdır.

* Tüm gerekli bağlantı noktaları şirket içi ve AVS SDDC arasında açık olmalıdır (bkz. [VMware HCX belgeleri](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).

* Şirket içinde HCX Yöneticisi için bir IP adresi ve ınterconnect (x) ve ağ uzantısı (NE) gereci için en az iki IP adresi.

* Şirket içi HCX x ve NE gibi araçlar vCenter ve ESXi altyapısına erişebilmelidir.

* WAN Interconnect gerecini dağıtmak için, Azure portal 'da SDDC dağıtımı için kullanılan/22 CıDR ağ adresi bloğunun yanı sıra, HCX bir/29 bloğu gerektirir. Bunu ağ planlamasına göre çarpanalmalısınız.

## <a name="deploy-the-vmware-hcx-ova-on-premises"></a>VMware HCX OVA 'yı şirket içinde dağıtma

1. AVS SDDC vCenter oturumunu açın ve **HCX**' i seçin.

    ![AVS vCenter 'da HCX 'i seçme](./media/hybrid-cloud-extension-installation/avs-vsphere-client.png)

1. VMware HCX ova dosyasını indirmek için **Yönetim**  >  **sistem güncelleştirmeleri**' ni seçin.

    ![Sistem güncelleştirmelerini al](./media/hybrid-cloud-extension-installation/administration-updates.png)

1. Şirket içi vCenter 'a dağıtılacak bir OVF şablonu seçin.  

    ![OVF şablonu seçin](./media/hybrid-cloud-extension-installation/select-template.png)

1. Bir ad ve konum seçin ve ardından HCX 'in dağıtılması gereken bir kaynak/küme seçin. Ardından, ayrıntıları ve gerekli kaynakları gözden geçirin.  

    ![Şablon ayrıntılarını gözden geçirme](./media/hybrid-cloud-extension-installation/configure-template.png)

1. Lisans koşullarını gözden geçirin ve kabul ediyorsanız gereken depolama ve ağ ' ı seçin. Ardından **İleri**' yi seçin.

1. **Şablonu Özelleştir**' de gerekli tüm bilgileri girin. 

    ![Şablonu Özelleştir](./media/hybrid-cloud-extension-installation/customize-template.png)  

1. **İleri**' yi seçin, yapılandırmayı doğrulayın ve HCX ova 'yı dağıtmak için **son** ' u seçin.

## <a name="activate-hcx"></a>HCX 'i etkinleştir

Yükleme sonrasında aşağıdaki adımları gerçekleştirin.

1. ' De HCX Yöneticisi ' ni açın `https://HCXManagerIP:9443` ve Kullanıcı adınızla parolanızla oturum açın. 

1. **Lisanslama**bölümünde **HCX gelişmiş anahtarınızı**girin.  

    ![HCX anahtarını girin](./media/hybrid-cloud-extension-installation/hcx-key.png)  
    
    > [!NOTE]
    > HCX Manager 'da açık internet erişimi veya bir ara sunucu yapılandırılmış olmalıdır.

1. VCenter 'ı yapılandırın.

    ![VCenter 'ı yapılandırma](./media/hybrid-cloud-extension-installation/configure-vcenter.png)

1. **Veri merkezi konumunda**, gerekirse, veri merkezi konumunu düzenleyin.

    ![Veritabanı konumu](./media/hybrid-cloud-extension-installation/system-location.png)

## <a name="configure-hcx"></a>HCX yapılandırma 

1. Şirket içi vCenter oturumu açın ve **Home**  >  **HCX**öğesini seçin.

    ![VCenter 'da HCX](./media/hybrid-cloud-extension-installation/hcx-vcenter.png)

1. **Altyapı**  >  **site eşleştirmesini**seçin  >  **site eşleştirmesi ekleyin**.

    ![Site eşleştirme Ekle](./media/hybrid-cloud-extension-installation/site-pairing.png)

1. **Uzak HCX URL 'si**, **Kullanıcı adı**ve **parola**girin. Ardından **Bağlan**'ı seçin.

   Sistem bağlı siteyi gösterir.
   
    ![Site bağlantısı](./media/hybrid-cloud-extension-installation/site-connection.png)

1. **Interconnect**  >  **çok siteli hizmet**  >  **ağı ağ profilleri**ağ  >  **profili oluştur**' u seçin.

    ![Ağ profili oluştur](./media/hybrid-cloud-extension-installation/create-network-profile.png)

1. HCX x ve NE IP adres aralıklarını girin (x ve NE yapmak için en az 2 IP adresi gereklidir).
    
   ![IP adresi aralıklarını girin](./media/hybrid-cloud-extension-installation/enter-address-ranges.png)
  
   > [!NOTE]
   > Ağ uzantısı gereci (HCX-NE), dağıtılmış sanal anahtar (DVS) ile bire bir ilişkisi vardır.  

1. Şimdi **işlem profili**  >  **oluşturma işlem profili**' ni seçin.

1. Bir işlem profili adı girin ve **devam**' ı seçin.  

    ![İşlem profili oluştur](./media/hybrid-cloud-extension-installation/create-compute-profile.png)

1. Geçiş, ağ uzantısı, PR olağanüstü durum kurtarma gibi etkinleştirilecek hizmetleri seçin. **Devam**’ı seçin.

    ![Hizmetleri seçin](./media/hybrid-cloud-extension-installation/select-services.png)

1. **Hizmet kaynaklarını Seç**bölümünde, seçili HCX hizmetlerinin etkinleştirilmesi gereken bir veya daha fazla hizmet kaynağı seçin. **Devam**’ı seçin.
    
   ![Hizmet kaynaklarını seçin](./media/hybrid-cloud-extension-installation/select-service-resources.png)
  
   > [!NOTE]
   > Kaynak VM 'Lerin HCX kullanarak geçiş için hedeflediği belirli kümeleri seçin.

1. **Veri deposu** ' nu seçip **devam**' ı seçin. 
      
    HCX Interconnect gereçlerini dağıtmak için her bir işlem ve depolama kaynağını seçin. Birden çok kaynak seçildiğinde, HCX, kapasitesi tükenene kadar seçilen ilk kaynağı kullanır.  
    
    ![Dağıtım kaynaklarını seçin](./media/hybrid-cloud-extension-installation/deployment-resources.png)

1. **Ağ profillerinde** oluşturulan yönetim ağı profilini seçin ve **devam**' ı seçin.  
      
    VCenter yönetim arabirimine ve ESXi konaklarına erişilebileceği ağ profilini seçin. Böyle bir ağ profili zaten tanımlanmamışsa, burada oluşturabilirsiniz.  
    
    ![Yönetim ağı profilini seçin](./media/hybrid-cloud-extension-installation/management-network-profile.png)

1. **Ağ yukarı** öğesini seçin ve **devam**' ı seçin.
      
    Aşağıdakilerden biri true olduğunda bir veya daha fazla ağ profili seçin:  
    * Uzak sitedeki Interconnect gereçlerine bu ağ üzerinden ulaşılamıyor  
    * Uzak taraf gereçler, bu ağ üzerinden yerel Interconnect gereçlerine ulaşabilir.  
    
    Birden çok site genelinde paylaşılmayan doğrudan bağlantı gibi noktadan noktaya ağlara sahipseniz, işlem profillerinin birden çok siteyle paylaşılması için bu adımı atlayabilirsiniz. Bu gibi durumlarda, bağlantı Hizmetleri ağı oluşturma sırasında yukarı bağlantı ağ profilleri geçersiz kılınabilir ve belirtilebilir.  
    
    ![Yukarı bağlantı ağ profilini seçin](./media/hybrid-cloud-extension-installation/uplink-network-profile.png)

1. **VMotion ağ profilini** seçin ve **devam**' ı seçin.
      
    ESXi konaklarının vMotion arabirimine erişilebileceği ağ profilini seçin. Böyle bir ağ profili zaten tanımlanmamışsa, burada oluşturabilirsiniz. VMotion ağınız yoksa, **Yönetim ağ profili**' ni seçin.  
    
    ![VMotion ağ profilini seçin](./media/hybrid-cloud-extension-installation/vmotion-network-profile.png)

1. **VSphere çoğaltma ağ profilini** seçin ve **devam**' ı seçin.
      
    ESXi konaklarının vSphere çoğaltma arabirimine erişilebileceği bir ağ profili seçin. Çoğu durumda, bu profil yönetim ağ profiliyle aynı olur.  
    
    ![VSphere çoğaltma ağ profilini seçin](./media/hybrid-cloud-extension-installation/vsphere-replication-network-profile.png)

1. **Ağ uzantıları Için dağıtılmış anahtarlar** ' ı seçin ve **devam**' ı seçin.  
      
    Geçiş yapılacak sanal makinelerin bağlı olduğu ağların bulunduğu dağıtılmış sanal anahtarları seçin.

    ![Dağıtılmış sanal anahtarlar seçin](./media/hybrid-cloud-extension-installation/distributed-switches.png)

1. Bağlantı kurallarını gözden geçirin ve **devam**' ı seçin. İşlem profili oluşturmak için **son** ' u seçin.  

    ![İşlem profili oluştur](./media/hybrid-cloud-extension-installation/complete-compute-profile.png)

## <a name="configure-network-uplink"></a>Ağ yukarı bağlantı yapılandırma

Artık ağ yukarı bağlantı için AVS SDDC 'deki ağ profili değişikliğini yapılandırın.

1. Yeni bir mantıksal anahtar oluşturmak için SDDC NSX-T ' de oturum açın veya şirket içi ve AVS SDDC arasındaki ağ yukarı kaydı için kullanılabilecek bir var olan mantıksal anahtar kullanın.

1. AVS SDDC 'de, şirket içi ve AVS SDDC iletişimi için kullanılabilecek HCX yukarı bağlantı için bir ağ profili oluşturun.  
    
   ![Yukarı bağlantı için ağ profili oluşturma](./media/hybrid-cloud-extension-installation/network-profile-uplink.png)

1. Ağ profili için bir ad ve gereken L2 Ağ uzantısına göre en az 4-5 boş IP adresi girin.  
    
   ![Yukarı bağlantı için ağ profilini yapılandırma](./media/hybrid-cloud-extension-installation/configure-uplink-profile.png)

1. AVS SDDC yapılandırmasını gerçekleştirmek için **Oluştur** ' u seçin

## <a name="configure-service-mesh"></a>Hizmet kafesi yapılandırma

Artık şirket içi ve AVS SDDC arasında hizmet kafesi yapılandırın.

1. AVS SDDC vCenter oturumunu açın ve **HCX**' i seçin.

1. **Altyapı**  >  **Interconnect**  >  **hizmet ağı**  >  **Oluştur hizmet ağı**' nı seçin.  Önceki adımlarda oluşturulan ağ ve işlem profillerini yapılandırın.    
      
    ![Hizmet kafesi yapılandırma](./media/hybrid-cloud-extension-installation/configure-service-mesh.png)

1. **Hizmet ağı oluştur** ' u seçin ve **devam**' ı seçin.  
      
    Karma Mobility 'in etkinleştirileceği arasında eşleştirilmiş siteler seçin.  
    
    ![Eşleştirilmiş siteleri seçin](./media/hybrid-cloud-extension-installation/select-paired-sites.png)

1. **İşlem profilini** seçin ve **devam**' ı seçin.
      
    Hybridity hizmetlerini etkinleştirmek için kaynak ve uzak sitelerde bir işlem profili seçin. Seçimler, sanal makinelerin HCX hizmetlerini tüketebileceği kaynakları tanımlayacaktır.  
      
    ![Hybridity hizmetlerini etkinleştir](./media/hybrid-cloud-extension-installation/enable-hybridity.png)

1. HCX için etkinleştirilecek hizmetleri seçin ve **devam**' ı seçin.  
      
    ![HCX hizmetleri seçin](./media/hybrid-cloud-extension-installation/hcx-services.png)

1. **Gelişmiş yapılandırma-yukarı bağlantı ağ profillerini geçersiz kıl** **devam**' ı seçin.  
      
    Yukarı bağlantı ağ profilleri, uzak sitenin Interconnect gereçlerine erişilebileceği ağa bağlanmak için kullanılır.  
      
    ![Yukarı bağlantı profillerini geçersiz kıl](./media/hybrid-cloud-extension-installation/override-uplink-profiles.png)

1. **Gelişmiş yapılandırma – ağ uzantısı gereç ölçeğini**genişletme ' de, **ağ uzantısı gereç ölçeğini genişlet**' i seçin. 
      
    ![Ağ uzantısının ölçeğini genişletme](./media/hybrid-cloud-extension-installation/network-extension-scale-out.png)

1. DVS anahtar sayısına karşılık gelen gereç sayısını girin.  
      
    ![Gereç sayısını Yapılandır](./media/hybrid-cloud-extension-installation/appliance-scale.png)

1. **Gelişmiş yapılandırma-Trafik Mühendisliği**' nda **devam**' ı seçin.  
      
    ![Trafik Mühendisliği yapılandırma](./media/hybrid-cloud-extension-installation/traffic-engineering.png)

1. Topoloji önizlemesini inceleyin ve **devam**' ı seçin. Ardından, bu hizmet ağı için Kullanıcı dostu bir ad girin ve tamamlanacak **son** ' u seçin.  
      
    ![Hizmet ağı 'nı doldurun](./media/hybrid-cloud-extension-installation/complete-service-mesh.png)

    Hizmet ağı dağıtıldı ve yapılandırıldı.  
      
    ![Dağıtılan hizmet ağı](./media/hybrid-cloud-extension-installation/deployed-service-mesh.png)

## <a name="check-appliance-status"></a>Gereç durumunu denetle
Gereç durumunu denetlemek için **Interconnect**  >  **gereçlerini**seçin. 
      
![Gereç durumu](./media/hybrid-cloud-extension-installation/appliance-status.png)

## <a name="next-steps"></a>Sonraki adımlar

Gereç bağlantısı **tünel durumu** **Açık** ve yeşil olduğunda, HCX kullanarak AVS VM 'lerini geçirmeye ve korumaya hazırsınızdır. VMware Technical belgelerindeki VMware [HCX belgelerine](https://docs.vmware.com/en/VMware-HCX/index.html) bakın ve VMware [HCX Ile sanal makineleri geçirme](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) .
