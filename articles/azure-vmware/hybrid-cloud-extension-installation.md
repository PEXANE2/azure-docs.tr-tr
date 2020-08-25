---
title: Karma bulut uzantısı 'nı (HCX) yükler
description: Azure VMware çözümünüz özel bulutunuz için VMware karma bulut uzantısı (HCX) çözümünü ayarlama
ms.topic: how-to
ms.date: 07/15/2020
ms.openlocfilehash: fb8497af33b364c1d2ab475233bd2a83ef1befad
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752323"
---
# <a name="install-hcx-for-azure-vmware-solution"></a>Azure VMware çözümü için HCX 'i yükler

Bu makalede, Azure VMWare çözüm özel bulutunuz için VMWare hibrit bulut uzantısı (HCX) çözümünü ayarlamaya yönelik yordamları ele aldık. HCX, VMware iş yüklerinizin buluta geçirilmesini ve farklı yerleşik HCX desteklenen geçiş türleri aracılığıyla diğer bağlı siteleri sağlar.

Varsayılan yükleme, HCX gelişmiş, en fazla üç site bağlantısını (Şirket içi veya bulutta buluta) destekler. Üçten fazla site bağlantısı gerekliyse, müşteriler şu anda önizleme aşamasında olan HCX kurumsal eklentisini destek aracılığıyla etkinleştirme seçeneğine sahiptir. HCX kurumsal, genel kullanılabilirlik (GA) sonrasında müşterilere ek ücretler taşır, ancak [ek özellikler](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/)sağlar.


[Başlamadan önce](#before-you-begin), [yazılım sürümü gereksinimlerinden](#software-version-requirements)ve [önkoşullardan](#prerequisites) önce ayrıntılı bir şekilde gözden geçirin. 

Ardından, aşağıdakileri yapmak için gereken tüm yordamları inceleyeceğiz:

> [!div class="checklist"]
> * Şirket içi HCX OVA 'yı (bağlayıcı) dağıtma
> * HCX 'i etkinleştirme ve yapılandırma
> * Ağ yukarı ve hizmet kafesi yapılandırma
> * Gereç durumunu denetleyerek kurulumu tamamlayarak

Kurulumu tamamladıktan sonra, bu makalenin sonunda sunulan önerilen sonraki adımları izleyebilirsiniz.  

## <a name="before-you-begin"></a>Başlamadan önce
    
* Temel Azure VMware çözümü yazılım tanımlı veri merkezi (SDDC) [öğretici serisini](tutorial-network-checklist.md)gözden geçirin.
* HCX Kullanıcı Kılavuzu dahil [VMware HCX belgelerini](https://docs.vmware.com/en/VMware-HCX/index.html) inceleyin ve başvuru yapın.
* VMware [HCX Ile sanal makinelerin geçişini](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)yapan VMware docs ' i inceleyin.
* İsteğe bağlı olarak [VMware HCX dağıtım konularını](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html)gözden geçirin.
* HCX üzerinde VMware vSphere [blog serisi](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) gibi, isteğe bağlı olarak HCX üzerinde ilgili VMware malzemeleri gözden geçirin. 
* Azure VMware Çözüm desteği kanalları aracılığıyla bir Azure VMware çözümü HCX Kurumsal etkinleştirme isteyin.

İş yüklerini işlem ve depolama kaynaklarına karşı boyutlandırma, Azure VMware çözümü özel bulutu HCX çözümünü kullanmaya hazırlanırken önemli bir planlama adımıdır. İlk özel bulut ortamı planlamasının parçası olarak boyutlandırma adımını ele edin. 

Ayrıca, Azure geçişi portalındaki bir Azure VMware Çözüm değerlendirmesi tamamlayarak iş yüklerini de yazdırabilirsiniz ( https://docs.microsoft.com/azure/migrate/how-to-create-azure-vmware-solution-assessment) .

## <a name="software-version-requirements"></a>Yazılım sürümü gereksinimleri

Altyapı bileşenleri, gerekli en düşük sürümü çalıştırıyor olmalıdır. 
                                                         
| Bileşen türü    | Kaynak ortam gereksinimleri    | Hedef ortam gereksinimleri   |
| --- | --- | --- |
| vCenter Server   | 5.1<br/><br/>5,5 U1 veya daha önceki bir sürümü kullanıyorsanız HCX işlemleri için tek başına HCX Kullanıcı arabirimini kullanın.  | 6,0 U2 ve üzeri   |
| ESXi   | 5.0    | ESXi 6,0 ve üzeri   |
| NSX    | Kaynaktaki mantıksal anahtarların HCX ağ uzantısı için: NSXv 6.2 + veya NSX-T 2.4 +   | NSXv 6.2 + veya NSX-T 2,4 +<br/><br/>HCX yakınlık yönlendirmesi için: NSXv 6.4 + (yakınlık yönlendirme NSX-T ile desteklenmez) |
| vCloud Direktörü   | Gerekli değildir-kaynak sitede vCloud Director ile birlikte çalışabilirlik yok | Hedef ortamı vCloud Director ile tümleştirdiğinizde, en az 9.1.0.2 olur.  |

## <a name="prerequisites"></a>Ön koşullar

* ExpressRoute Global Reach, şirket içi ve Azure VMware çözümü SDDC ExpressRoute devreleri arasında yapılandırılmalıdır.

* Şirket içi ve Azure VMware çözümü SDDC arasında tüm gerekli bağlantı noktaları açık olmalıdır (bkz. [VMware HCX belgeleri](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).

* Şirket içinde HCX Yöneticisi için bir IP adresi ve ınterconnect (x) ve ağ uzantısı (NE) gereci için en az iki IP adresi.

* Şirket içi HCX x ve NE gibi araçlar vCenter ve ESXi altyapısına erişebilmelidir.

* WAN Interconnect gerecini dağıtmak için, Azure portal 'da SDDC dağıtımı için kullanılan/22 CıDR ağ adresi bloğunun yanı sıra, HCX bir/29 bloğu gerektirir. Bu gereksinimi ağ planlamasına katdığınızdan emin olun.

## <a name="deploy-the-vmware-hcx-ova-on-premises"></a>VMware HCX OVA 'yı şirket içinde dağıtma

1. Azure VMware Çözüm SDDC vCenter ' de oturum açın ve **HCX**' i seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/avs-vsphere-client.png" alt-text="Azure VMware Çözüm SDDC vCenter ' de oturum açın ve HCX ' i seçin.":::

1. **Yönetim**altında **sistem güncelleştirmeleri** ' ni seçin ve sonra VMware HCX ova dosyasını Indirmek için **indirme bağlantısı iste** ' yi seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/administration-updates.png" alt-text="Yönetim altında sistem güncelleştirmeleri ' ni seçin ve sonra VMware HCX OVA dosyasını indirmek için indirme bağlantısı ıste ' yi seçin.":::

1. Sonra şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-template.png" alt-text="Sonra şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.":::

1. Bir ad ve konum seçin ve ardından HCX 'in dağıtılması gereken bir kaynak/küme seçin. Ardından, ayrıntıları ve gerekli kaynakları gözden geçirin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-template.png" alt-text=" Bir ad ve konum seçin ve ardından HCX 'in dağıtılması gereken bir kaynak/küme seçin. Ardından, ayrıntıları ve gerekli kaynakları gözden geçirin.":::

1. Lisans koşullarını gözden geçirin ve kabul ediyorsanız gereken depolama ve ağ ' ı seçin. Sonra **İleri**’yi seçin.

1. **Şablonu Özelleştir**' de gerekli tüm bilgileri girin. 

   :::image type="content" source="media/hybrid-cloud-extension-installation/customize-template.png" alt-text="Şablonu Özelleştir ' de gerekli tüm bilgileri girin.":::

1. **İleri**' yi seçin, yapılandırmayı doğrulayın ve HCX ova 'yı dağıtmak için **son** ' u seçin.

## <a name="activate-hcx"></a>HCX 'i etkinleştir

Yükleme sonrasında aşağıdaki adımları gerçekleştirin.

1. Şirket içi HCX yöneticisinde oturum açın `https://HCXManagerIP:9443` ve Kullanıcı adınızla parolanızla oturum açın. 

   > [!IMPORTANT]
   > `9443`HCX YÖNETICISI IP adresine sahip bağlantı noktası numarasını eklediğinizden emin olun.

1. **Lisanslama**bölümünde **HCX gelişmiş anahtarınızı**girin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/hcx-key.png" alt-text="Lisanslama bölümünde HCX gelişmiş anahtarınızı girin.":::
    
    > [!NOTE]
    > HCX Manager 'da açık internet erişimi veya bir ara sunucu yapılandırılmış olmalıdır.

1. **VCenter**'Da gerekirse vCenter bilgilerini düzenleyin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-vcenter.png" alt-text="VCenter 'da gerekirse vCenter bilgilerini düzenleyin.":::

1. **Veri merkezi konumunda**, gerekirse, veri merkezi konumunu düzenleyin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/system-location.png" alt-text="Veri merkezi konumunda, gerekirse, veri merkezi konumunu düzenleyin.":::

## <a name="configure-hcx"></a>HCX yapılandırma 

1. Şirket içi vCenter oturumu açın ve **giriş**bölümünde **HCX**' i seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/hcx-vcenter.png" alt-text="Şirket içi vCenter oturumu açın ve giriş bölümünde HCX ' i seçin.":::

1. **Altyapı**altında **site eşleştirme**  >  **site eşleştirme Ekle**' yi seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/site-pairing.png" alt-text="Altyapı altında site eşleştirme ' yi seçerek site eşleştirme ekleyin >.":::

1. Uzak HCX URL 'sini veya IP adresini, Azure VMware çözümü cloudadmin Kullanıcı adı ve parolayı girin ve sonra **Bağlan**' ı seçin.

   Sistem bağlı siteyi gösterir.

   :::image type="content" source="media/hybrid-cloud-extension-installation/site-connection.png" alt-text="Sistem bağlı siteyi gösterir.":::

1. **Altyapı**altında, **bağlantı**  >  **çok siteli hizmet**  >  **ağı ağ profilleri**ağ  >  **profili oluştur**' u seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/create-network-profile.png" alt-text="Altyapı altında, ağ profili oluştur > ağ profilleri > çok siteli hizmet kafesi > Interconnect ' i seçin.":::

1. Yeni ağ profili için, HCX x ve NE IP adresi aralıklarını girin (x ve NE kadar gereçlerden biri için en az iki IP adresi gereklidir).

   :::image type="content" source="media/hybrid-cloud-extension-installation/enter-address-ranges.png" alt-text="Yeni ağ profili için, HCX x ve NE IP adresi aralıklarını girin (x ve NE kadar gereçlerden biri için en az iki IP adresi gereklidir).":::
  
   > [!NOTE]
   > Ağ uzantısı gereci (HCX-NE), dağıtılmış sanal anahtar (DVS) ile bire bir ilişkisi vardır.  

1. Şimdi **işlem profili**  >  **oluşturma işlem profili**' ni seçin.

1. Bir işlem profili adı girin ve **devam**' ı seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/create-compute-profile.png" alt-text="Bir işlem profili adı girin ve devam ' ı seçin.":::

1. Geçiş, ağ uzantısı veya olağanüstü durum kurtarma gibi etkinleştirilecek hizmetleri seçin ve ardından **devam**' ı seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-services.png" alt-text="Geçiş, ağ uzantısı veya olağanüstü durum kurtarma gibi etkinleştirilecek hizmetleri seçin ve ardından devam ' ı seçin.":::

1. **Hizmet kaynaklarını Seç**bölümünde, seçili HCX hizmetlerinin etkinleştirilmesi gereken bir veya daha fazla hizmet kaynağı seçin. **Devam**’ı seçin.

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-service-resources.png" alt-text="Hizmet kaynaklarını Seç bölümünde, seçili HCX hizmetlerinin etkinleştirilmesi gereken bir veya daha fazla hizmet kaynağı seçin. Devam ' ı seçin.":::
  
   > [!NOTE]
   > Kaynak VM 'Lerin HCX kullanarak geçiş için hedeflediği belirli kümeleri seçin.

1. **Veri deposu** ' nu seçip **devam**' ı seçin. 
      
   HCX Interconnect gereçlerini dağıtmak için her bir işlem ve depolama kaynağını seçin. Birden çok kaynak seçildiğinde, HCX, kapasitesi tükenene kadar seçilen ilk kaynağı kullanır.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/deployment-resources.png" alt-text="HCX Interconnect gereçlerini dağıtmak için her bir işlem ve depolama kaynağını seçin. Birden çok kaynak seçildiğinde, HCX, kapasitesi tükenene kadar seçilen ilk kaynağı kullanır.":::

1. **Ağ profillerinde** oluşturulan yönetim ağı profilini seçin ve **devam**' ı seçin.  
      
   VCenter yönetim arabirimine ve ESXi konaklarına erişilebileceği ağ profilini seçin. Böyle bir ağ profili zaten tanımlanmamışsa, burada oluşturabilirsiniz.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/management-network-profile.png" alt-text="VCenter yönetim arabirimine ve ESXi konaklarına erişilebileceği ağ profilini seçin. Böyle bir ağ profili zaten tanımlanmamışsa, burada oluşturabilirsiniz.":::

1. **Ağ yukarı** öğesini seçin ve **devam**' ı seçin.
      
   Aşağıdakilerden birinin doğru olması için bir veya daha fazla ağ profili seçin:  
   * Uzak sitedeki Interconnect gereçlerine bu ağ üzerinden ulaşılamıyor  
   * Uzak taraf gereçler, bu ağ üzerinden yerel Interconnect gereçlerine ulaşabilir.  
    
   Birden çok site genelinde paylaşılmayan doğrudan bağlantı gibi noktadan noktaya ağlara sahipseniz, işlem profillerinin birden çok siteyle paylaşılması için bu adımı atlayabilirsiniz. Bu gibi durumlarda, bağlantı Hizmetleri ağı oluşturma sırasında yukarı bağlantı ağ profilleri geçersiz kılınabilir ve belirtilebilir.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/uplink-network-profile.png" alt-text="Ağ yukarı öğesini seçin ve devam ' ı seçin.":::

1. **VMotion ağ profilini** seçin ve **devam**' ı seçin.
      
   ESXi konaklarının vMotion arabirimine erişilebileceği ağ profilini seçin. Böyle bir ağ profili zaten tanımlanmamışsa, burada oluşturabilirsiniz. VMotion ağınız yoksa, **Yönetim ağ profili**' ni seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/vmotion-network-profile.png" alt-text="VMotion ağ profilini seçin ve devam ' ı seçin.":::

1. **VSphere çoğaltma ağ profilinden**, bir ağ profili seçin ESXi konaklarının vSphere çoğaltma arabirimi ve sonra **devam**' ı seçin.
      
   Çoğu durumda, bu profil yönetim ağ profiliyle aynı olur.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/vsphere-replication-network-profile.png" alt-text="VSphere çoğaltma ağ profilinden, bir ağ profili seçin ESXi konaklarının vSphere çoğaltma arabirimi ve sonra devam ' ı seçin.":::

1. **Ağ uzantıları Için dağıtılmış anahtarlar seçin**sayfasında, tümleştirilecek ve bağlı olduğu VM 'ler için AĞLARıN bulunduğu DVS 'yi seçin.  **Devam**’ı seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/distributed-switches.png" alt-text="Ağ uzantıları için dağıtılmış anahtarlar seçin sayfasında, tümleştirilecek ve bağlı olduğu VM 'Ler için ağların bulunduğu DVS 'yi seçin.  Devam ' ı seçin.":::

1. Bağlantı kurallarını gözden geçirin ve **devam**' ı seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/complete-compute-profile.png" alt-text="Bağlantı kurallarını gözden geçirin ve devam ' ı seçin.":::

1.  İşlem profili oluşturmak için **son** ' u seçin.

## <a name="configure-network-uplink"></a>Ağ yukarı bağlantı yapılandırma

Artık ağ yukarı bağlantı için Azure VMware Çözüm SDDC 'de ağ profili değişikliğini yapılandırın.

1. Yeni bir mantıksal anahtar oluşturmak için SDDC NSX-T ' de oturum açın veya şirket içi ve Azure VMware Çözüm SDDC arasında ağ yukarı bağlantı için kullanılabilecek bir var olan mantıksal anahtar kullanın.

1. Azure VMware Çözüm SDDC 'de, şirket içi Azure VMware çözümü SDDC iletişimi için kullanılabilen HCX yukarı bağlantı için bir ağ profili oluşturun.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/network-profile-uplink.png" alt-text="Azure VMware Çözüm SDDC 'de, şirket içi Azure VMware çözümü SDDC iletişimi için kullanılabilen HCX yukarı bağlantı için bir ağ profili oluşturun.":::

1. Ağ profili için bir ad ve gereken L2 Ağ uzantısına göre en az 4-5 boş IP adresi girin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-uplink-profile.png" alt-text="Ağ profili için bir ad ve gereken L2 Ağ uzantısına göre en az 4-5 boş IP adresi girin.":::

1. Azure VMware çözümü SDDC yapılandırmasını gerçekleştirmek için **Oluştur** ' u seçin

## <a name="configure-service-mesh"></a>Hizmet kafesi yapılandırma

Artık şirket içi ve Azure VMware çözümü SDDC arasında hizmet kafesi yapılandırın.

1. Azure VMware Çözüm SDDC vCenter ' de oturum açın ve **HCX**' i seçin.

2. **Altyapı**altında, **Interconnect**  >  önceki adımlarda oluşturulan ağ ve işlem profillerini yapılandırmak için Interconnect**Service**  >  **kafeshizmet ağı oluştur** ' u seçin.    

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-service-mesh.png" alt-text="Altyapı altında, önceki adımlarda oluşturulan ağ ve işlem profillerini yapılandırmak için bağlantı > hizmet ağı oluştur > hizmet ağı oluştur ' u seçin.":::

3. Karma yeteneği etkinleştirmek için eşleştirilmiş siteler ' i seçin ve **devam**' ı seçin.   

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-paired-sites.png" alt-text="Karma yeteneği etkinleştirmek için eşleştirilmiş siteler ' i seçin ve devam ' ı seçin.":::

4. Hybridity hizmetlerini etkinleştirmek için kaynak ve Uzaktan işlem profillerini seçip **devam**' ı seçin.
      
   Seçimler, VM 'Lerin HCX hizmetlerini tüketebileceği kaynakları tanımlar.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/enable-hybridity.png" alt-text="Seçimler, VM 'Lerin HCX hizmetlerini tüketebileceği kaynakları tanımlar.":::

5. Etkinleştirilecek hizmetleri seçin ve **devam**' ı seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/hcx-services.png" alt-text="Etkinleştirilecek hizmetleri seçin ve devam ' ı seçin.":::

6. **Gelişmiş yapılandırma-yukarı bağlantı ağ profillerini geçersiz kıl** **devam**' ı seçin.  
      
   Yukarı bağlantı ağ profilleri, uzak sitenin Interconnect gereçlerine erişilebileceği ağa bağlanmak için kullanılır.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/override-uplink-profiles.png" alt-text="Yukarı bağlantı ağ profilleri, uzak sitenin Interconnect gereçlerine erişilebileceği ağa bağlanmak için kullanılır.":::

7. **Ağ uzantısını Yapılandır gereç ölçeğini**seçin. 

   :::image type="content" source="media/hybrid-cloud-extension-installation/network-extension-scale-out.png" alt-text="Ağ uzantısını Yapılandır gereç ölçeğini seçin.":::

8. DVS anahtar sayısına karşılık gelen gereç sayısını girin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/appliance-scale.png" alt-text="DVS anahtar sayısına karşılık gelen gereç sayısını girin.":::

9. Atlamak için **devam** ' ı seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/traffic-engineering.png" alt-text="Atlamak için devam ' ı seçin.":::

10. Topoloji önizlemesini inceleyin ve **devam**' ı seçin. 

11. Bu hizmet ağı için Kullanıcı dostu bir ad girin ve tamamlanacak **son** ' u seçin.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/complete-service-mesh.png" alt-text="Hizmet ağı 'nı doldurun":::

   Hizmet ağı dağıtıldı ve yapılandırıldı.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/deployed-service-mesh.png" alt-text="Dağıtılan hizmet ağı":::

## <a name="check-appliance-status"></a>Gereç durumunu denetle
Gereç durumunu denetlemek için **Interconnect**  >  **gereçlerini**seçin. 

:::image type="content" source="media/hybrid-cloud-extension-installation/appliance-status.png" alt-text="Gereç durumunu denetleyin.":::

## <a name="next-steps"></a>Sonraki adımlar

Gereç bağlantısı **tünel durumu** **Açık** ve yeşil olduğunda, HCX kullanarak Azure VMware Çözüm VM 'lerini geçirmeye ve korumaya hazırsınızdır. VMware Technical belgelerindeki VMware [HCX belgelerine](https://docs.vmware.com/en/VMware-HCX/index.html) bakın ve VMware [HCX Ile sanal makineleri geçirme](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) .
