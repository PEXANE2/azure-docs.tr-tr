---
title: Öğretici-VMware HCX 'i dağıtma ve yapılandırma
description: Azure VMware çözümünüz özel bulutunuz için VMware HCX çözümünü dağıtmayı ve yapılandırmayı öğrenin.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 69df9b6337674233e1f257cc509115d5f58d2e7f
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152065"
---
# <a name="deploy-and-configure-vmware-hcx"></a>VMware HCX’i dağıtma ve yapılandırma

Bu makalede, Azure VMware Çözüm özel bulutunuz için VMware HCX şirket içi "bağlayıcısını" dağıtma ve yapılandırma yordamlarına kılavuzluk ederiz. VMware HCX ile, VMware iş yüklerinizi Azure VMware çözümüne ve diğer bağlı sitelere çeşitli geçiş türleri aracılığıyla geçirebilirsiniz.  Azure VMware çözümü, "Cloud Manager" dağıtımını dağıttığı ve yapılandırdığından, şirket içi VMware veri merkezinizde "bağlayıcı" indirmeniz, etkinleştirmeniz ve yapılandırmanız gerekir.  

Azure VMware çözümünde önceden dağıtılan VMware HCX gelişmiş Bağlayıcısı, en fazla üç site bağlantısını (Şirket içi buluta veya buluta buluta) destekler. Üçten fazla site bağlantısı gerekiyorsa, [VMware HCX kurumsal](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/) eklentisini (Şu anda *önizlemede*) etkinleştirmek üzere bir [destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support) gönderebilirsiniz.  

>[!NOTE]
>VMware HCX Enterprise Edition (EE), Azure VMware çözümüyle bir *Önizleme* işlevi/hizmeti olarak sunulmaktadır. Azure VMware çözümü için VMware HCX EE *Önizleme*aşamasındadır. Bu, ücretsiz bir işlevdir/hizmettir ve hizmet hüküm ve koşullarına tabidir. VMware HCX EE hizmeti GA olduktan sonra, faturalandırmaya geçiş yapılacak 30 günlük bir bildirim alırsınız. Ayrıca hizmetin kapatılmasını/devre dışı bırakabilirsiniz.

Başlamadan önce, [başlamadan önce](#before-you-begin), [yazılım sürümü gereksinimleri](#software-version-requirements)ve [önkoşulları](#prerequisites)için baştan önce gözden geçirin. 

Ardından, aşağıdakileri yapmak için gereken tüm yordamları inceleyeceğiz:

> [!div class="checklist"]
> * Şirket içi VMware HCX OVA (HCX Bağlayıcısı) dağıtma
> * VMware HCX bağlayıcısını etkinleştir
> * Azure VMware çözümünüz HCX Cloud Manager ile şirket içi HCX bağlayıcınızı eşleştirin
> * InterConnect 'i yapılandırma (ağ profili, işlem profili ve hizmet ağı)
> * Gereç durumunu denetleyerek ve geçişin doğrulanmasını sağlayarak kurulumu tamamen yapın

İşiniz bittiğinde, bu makalenin sonunda önerilen sonraki adımları izleyebilirsiniz.  

## <a name="before-you-begin"></a>Başlamadan önce
   
* Temel Azure VMware Çözüm Software-Defined veri merkezi (SDDC) [öğretici serisini](tutorial-network-checklist.md)gözden geçirin.
* HCX Kullanıcı Kılavuzu dahil olmak üzere [VMware HCX belgelerini](https://docs.vmware.com/en/VMware-HCX/index.html)inceleyin ve başvuru yapın.
* VMware [HCX Ile sanal makinelerin geçişini](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)yapan VMware docs ' i inceleyin.
* İsteğe bağlı olarak [VMware HCX dağıtım konularını](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html)gözden geçirin.
* HCX üzerinde VMware vSphere [blog serisi](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) gibi, isteğe bağlı olarak HCX üzerinde ilgili VMware malzemeleri gözden geçirin. 
* İsteğe bağlı olarak Azure VMware Çözüm desteği kanalları aracılığıyla bir Azure VMware çözümü HCX Kurumsal etkinleştirme isteyin.
* İsteğe bağlı olarak [HCX için gereken ağ bağlantı noktalarını gözden geçirin](https://ports.vmware.com/home/VMware-HCX).
* Azure VMware çözümü HCX Cloud Manager, Azure VMware Çözüm özel bulutu için belirtilen/22 sürümünden önceden yapılandırılmış olsa da, HCX şirket içi Bağlayıcısı, ağ aralıklarının şirket içi ağınızdan müşteri tarafından ayrılmasını gerektirir. Bu ağlar ve aralıklar belgede daha aşağı açıklanacaktır.

İşlem ve depolama kaynaklarına karşı iş yüklerini boyutlandırma, önemli bir planlama adımıdır. İlk özel bulut ortamı planlamasının parçası olarak boyutlandırma adımını ele edin. 

Ayrıca, Azure geçişi Portalı ' nın bir [Azure VMware Çözüm değerlendirmesini](../migrate/how-to-create-azure-vmware-solution-assessment.md) tamamlayarak iş yüklerini de kullanabilirsiniz.

## <a name="software-version-requirements"></a>Yazılım sürümü gereksinimleri

Altyapı bileşenleri, gerekli en düşük sürümü çalıştırıyor olmalıdır. 
                                                         
| Bileşen türü    | Kaynak ortam gereksinimleri    | Hedef ortam gereksinimleri   |
| --- | --- | --- |
| vCenter Server   | 5.1<br/><br/>5,5 U1 veya daha önceki bir sürümü kullanıyorsanız HCX işlemleri için tek başına HCX Kullanıcı arabirimini kullanın.  | 6,0 U2 ve üzeri   |
| ESXi   | 5.0    | ESXi 6,0 ve üzeri   |
| NSX    | Kaynaktaki mantıksal anahtarların HCX ağ uzantısı için: NSXv 6.2 + veya NSX-T 2.4 +   | NSXv 6.2 + veya NSX-T 2,4 +<br/><br/>HCX yakınlık yönlendirmesi için: NSXv 6.4 + (yakınlık yönlendirme NSX-T ile desteklenmez) |
| vCloud Direktörü   | Gerekli değildir-kaynak sitede vCloud Director ile birlikte çalışabilirlik yok | Hedef ortamı vCloud Director ile tümleştirdiğinizde, en az 9.1.0.2 olur.  |

## <a name="prerequisites"></a>Önkoşullar

### <a name="network-and-ports"></a>Ağ ve bağlantı noktaları

* [Expressroute Global Reach](tutorial-expressroute-global-reach-private-cloud.md) , şirket Içi ve Azure VMware çözümü SDDC ExpressRoute devreleri arasında yapılandırıldı.

* Şirket içi bileşenler ve şirket içi bileşenler arasındaki iletişimin Azure VMware Çözüm SDDC ile olması için gerekli tüm bağlantı noktaları açık olmalıdır (bkz. [VMware HCX belgeleri](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).


### <a name="ip-addresses"></a>IP adresleri

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-connector-ova-on-premises"></a>VMware HCX bağlayıcı OVA 'yı şirket içinde dağıtma

>[!NOTE]
>Sanal Gereci şirket içi vCenter 'ınıza dağıtmadan önce, VMware HCX bağlayıcı OVA 'yı indirmeniz gerekir. 

1. Bir tarayıcı penceresi açın, 443 numaralı bağlantı noktasındaki Azure VMware Çözüm HCX Yöneticisi ' nde `https://x.x.x.9` **cloudadmin** Kullanıcı kimlik bilgileriyle oturum açın ve ardından **destek**bölümüne gidin.

   >[!TIP]
   >Azure VMware çözümünde HCX Cloud Manager 'ın IP 'sini aklınızda edin. HCX Cloud Manager 'ın IP adresini belirlemek için, Azure VMware Çözüm dikey penceresinde bağlantıyı **Yönet**' e gidin  >  **Connectivity** ve ardından **HCX** sekmesini seçin. 
   >
   >VCenter parolası, özel bulutu ayarlarken tanımlanmıştır.

1. VMware HCX bağlayıcı OVA dosyasını indirmek için **indirme** bağlantısını seçin.

1. Şirket içi vCenter 'ınızdan, HCX bağlayıcısını şirket içi vCenter 'ınıza dağıtmak için indirdiğiniz OVA dosyası olan bir OVF şablonu seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. HCX bağlayıcısını dağıttığınız bir ad ve konum, kaynak/küme seçin ve ardından ayrıntıları ve gerekli kaynakları gözden geçirin.  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. Lisans koşullarını gözden geçirin ve kabul ediyorsanız gereken depolama ve ağ ' ı seçin ve ardından **İleri**' yi seçin.

1. **Şablonu Özelleştir**' de gerekli tüm bilgileri girin. 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. **İleri**' yi seçin, yapılandırmayı doğrulayın ve sonra HCX BAĞLAYıCı ova 'yı dağıtmak için **son** ' u seçin.
     
   >[!NOTE]
   >Genellikle, dağıtmakta olduğunuz VMware HCX Bağlayıcısı, kümenin yönetim ağı üzerine dağıtılır.  
   
   > [!IMPORTANT]
   > Dağıtım tamamlandıktan sonra sanal gereci el ile açmanız gerekebilir.
   > Sonraki adıma geçmek için HCX gerecini kapatmadan sonra 10-15 dakika bekleyin.

Bu adıma uçtan uca bir genel bakış için [Azure VMware çözümü-VMware HCX gereç dağıtımı](https://www.youtube.com/embed/BwSnQeefnso) videosunu görüntüleyin. 


## <a name="activate-vmware-hcx"></a>VMware HCX 'i etkinleştir

VMware HCX bağlayıcı OVA 'yı şirket içinde dağıttıktan ve gereci başlattıktan sonra etkinleştirmeye hazırsınız demektir. İlk olarak, Azure 'da Azure VMware Çözüm Portalından bir lisans anahtarı almanız gerekir.

1. Azure VMware Çözüm portalında **Manage**  >  **bağlantıyı**Yönet ' e gidin, **HCX** sekmesini seçin ve ardından **Ekle**' yi seçin.

1. Şirket içi VMware HCX yöneticisinde oturum açın `https://HCXManagerIP:9443` ve **yönetici** Kullanıcı kimlik bilgileriyle oturum açın. 

   > [!IMPORTANT]
   > `9443`VMware HCX Manager IP adresine sahip bağlantı noktası numarasını eklediğinizden emin olun.

1. **Lisanslama**bölümünde **HCX gelişmiş anahtarınızı**girin.  
   
    > [!NOTE]
    > VMware HCX Manager 'da açık internet erişimi veya proxy yapılandırılmış olmalıdır.

1. **Veri merkezi konumunda**, VMware HCX yöneticisini şirket içi yükleme için en yakın konumu belirtin.

1. **Sistem adını** değiştirin veya varsayılanı kabul edin.
   
1. Daha sonra Bitebilmeniz veya devam edebilirsiniz, Evet seçeneğini belirleyip devam etmek için **devam** edin.
    
1. **VCenter 'Larınızı bağlayın**bölümünde vCenter sunucunuzun FQDN 'SINI veya IP adresini ve uygun kimlik bilgilerini girip **devam**' ı seçin.
   
1. **SSO/PSC yapılandırma**bölümünde, PSC 'nizin FQDN 'SINI veya IP adresini girip **devam**' ı seçin.
   
   >[!NOTE]
   >Genellikle, vCenter FQDN/IP 'niz ile aynıdır.

1. Tüm girdilerin doğru olduğundan emin olun ve **Yeniden Başlat**' ı seçin.
    
   > [!NOTE]
   > Sonraki adımda uyarılmadan önce yeniden başlattıktan sonra bir gecikme yaşarsınız.
   >
   >Hizmetler yeniden başlatıldıktan sonra, açılan ekran üzerinde görünen vCenter ' ı görmeniz önemlidir. Hem vCenter hem de SSO, ilgili yapılandırma parametrelerine sahiptir ve bu, önceki ekranla aynı olmalıdır.

   :::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

Bu adıma uçtan uca genel bakış için [Azure VMware, VMware HCX etkinleştirme](https://www.youtube.com/embed/BkAV_TNYxdE) videosunu görüntüleyin.


## <a name="configure-vmware-hcx-connector"></a>VMware HCX bağlayıcısını yapılandırma

Artık bir site eşleştirme eklemeye, bir ağ ve işlem profili oluşturmaya ve geçiş, ağ uzantısı ya da olağanüstü durum kurtarma gibi hizmetleri etkinleştirmeye hazır olursunuz. 

### <a name="add-a-site-pairing"></a>Site eşleştirmesi ekleme

Azure VMware çözümünde VMware HCX bulut yöneticisini, veri merkezinizdeki VMware HCX Bağlayıcısı ile bağlayabilirsiniz (eşleştirin). 

1. Şirket içi vCenter oturumunuzu açın ve **giriş**bölümünde **HCX**' i seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. **Altyapı**altında **site eşleştirme**' yi seçin ve ardından **uzak siteye Bağlan** seçeneğini belirleyin (ekranın ortasında). 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. Daha önce not ettiğiniz **uzak HCX URL 'sini veya IP adresini** , Azure VMware Çözüm cloudadmin@vsphere.local Kullanıcı adı ve **parolasını**girin ve sonra **Bağlan**' ı seçin.

   > [!NOTE]
   > **Uzak HCX URL 'si** , Azure VMware çözümünüz özel bulutu HCX Cloud Manager IP 'sidir, genellikle yönetim ağının ". 9" adresidir.  Örneğin, vCenter 'niz 192.168.4.2 ise, HCX URL 'niz 192.168.4.9 olur.
   >
   > **Parola** , vCenter 'da oturum açmak için kullandığınız parolayla aynı olacaktır. Bu parolayı ilk dağıtım ekranında tanımladınız.

   Azure VMware çözümünde ve HCX bağlayıcı şirket içi bağlı (eşleştirilmiş) HCX bulut yöneticinizin gösterildiği bir ekran görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.":::

Bu adıma uçtan uca genel bakış için [Azure VMware Solution-VMware HCX site eşleştirme](https://www.youtube.com/embed/sKizDCRHOko) videosunu görüntüleyin.



### <a name="create-network-profiles"></a>Ağ profilleri oluşturma

VMware HCX, birden çok IP kesimi gerektiren sanal gereçler (otomatik) bir alt kümesini dağıtır.  Ağ profillerinizi oluştururken, [VMware HCX ağ kesimleri dağıtım öncesi hazırlama ve planlama aşaması](production-ready-deployment-steps.md#vmware-hcx-network-segments)SıRASıNDA tanımladığınız IP segmentlerini tanımlarsınız.

Dört ağ profili oluşturacaksınız:

   - Yönetim
   - vMotion
   - Çoğaltma
   - Yukarı

1. **Altyapı**altında, **bağlantı**  >  **çok siteli hizmet**  >  **ağı ağ profilleri**ağ  >  **profili oluştur**' u seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. Her ağ profili için ağ, bağlantı noktası grubu ' nu seçin, bir ad girin, söz konusu segment için IP havuzu oluşturun ve ardından **Oluştur**' u seçin. 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.":::

Bu adıma uçtan uca genel bakış için [Azure VMware,-VMware HCX Create Network profile](https://www.youtube.com/embed/NhyEcLco4JY) videosunu görüntüleyin.


### <a name="create-a-compute-profile"></a>İşlem profili oluşturma

1. **İşlem profilleri**  >  **işlem profili oluştur**' u seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. Profil için bir ad girin ve **devam**' ı seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Geçiş, ağ uzantısı veya olağanüstü durum kurtarma gibi etkinleştirilecek hizmetleri seçin ve ardından **devam**' ı seçin.
  
   > [!NOTE]
   > Genellikle burada hiçbir şey değişiklik yapmaz.

1. **Hizmet kaynaklarını Seç**bölümünde, seçilen VMware HCX hizmetlerini etkinleştirmek için bir veya daha fazla hizmet kaynağı (küme) seçin.  

1. Şirket içi veri merkezinizde kümeleri gördüğünüzde **devam**' ı seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. **Veri deposundan Seç**' den VMware HCX Interconnect gereçlerini dağıtmak için veri deposu depolama kaynağını seçin ve ardından **devam**' ı seçin.

   Birden çok kaynak seçildiğinde, VMware HCX, kapasitesi tükenene kadar seçilen ilk kaynağı kullanır.   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. Önceki adımlarda oluşturduğunuz yönetim ağı profilini, **Yönetim ağı profilini**seçin ve ardından **devam**' ı seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > Yönetim ağı profili, VMware HCX gerecinin vCenter ile iletişim kurmasına izin verir ve ESXi konaklarına erişilebilir.

1. Önceki adımlarda oluşturduğunuz **yukarı bağlantı ağ**profilini ve ardından **devam**' ı seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. Önceki adımlarda oluşturduğunuz vMotion **ağ profilini seçin ve**ardından **devam**' ı seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. Önceki adımlarda oluşturduğunuz **sanal Sphere çoğaltma ağ profilini**, çoğaltma ağ profilini seçin ve ardından **devam**' ı seçin.

   Çoğu durumda, çoğaltma ağ profili, yönetim ağ profiliyle aynı olur.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. Katman 2 genişletilmiş bir ağda Azure VMware çözümüne geçirilecek sanal makineleri içeren dağıtılmış sanal anahtarlar olan **ağ uzantıları Için dağıtılmış anahtarlar**' ı seçin ve ardından **devam**' ı seçin.

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Bağlantı kurallarını gözden geçirin ve **devam**' ı seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. İşlem profili oluşturmak için **son** ' u seçin.

   Aşağıdakine benzer bir ekran görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

Bu adıma uçtan uca genel bakış için [Azure VMware,-VMware HCX işlem profili oluşturma](https://www.youtube.com/embed/qASXi5xrFzM) videosunu görüntüleyin.




### <a name="create-service-mesh"></a>Hizmet ağı oluştur

Artık şirket içi ve Azure VMware Çözüm SDDC arasında hizmet kafesi yapılandırma zamanı.

1. **Altyapı**altında, **Interconnect**  >  **Service kafesi**  >  **hizmet ağı oluştur**' u seçin.    

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Önceden doldurulmuş siteleri gözden geçirin ve ardından **devam**' ı seçin. 

   >[!NOTE]
   >Bu ilk hizmet ağı yapılandırmanızda ise, bu ekranı değiştirmeniz gerekmez.  

1. Kaynak ve Uzaktan işlem profillerinin açılan seçimini seçip **devam**' ı seçin.  

   Seçimler, VM 'Lerin VMware HCX hizmetlerini kullanabileceği kaynakları tanımlar.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Etkinleştirilecek hizmetleri gözden geçirin ve ardından **devam**' ı seçin.  

1. **Gelişmiş yapılandırma-yukarı bağlantı ağ profillerini geçersiz kıl** **devam**' ı seçin.  Yukarı bağlantı ağ profilleri, uzak sitenin Interconnect gereçlerine ulaşılmış olan ağa bağlanır.  
  
1. **Gelişmiş yapılandırma-ağ uzantısı gereç ölçeği ölçeğini**Inceleyin ve **devam**' ı seçin. 

1. **Gelişmiş yapılandırma-Trafik Mühendisliği**' nda, gereken tüm değişiklikleri gözden geçirin ve yapın ve ardından **devam**' ı seçin.

1. Topoloji önizlemesini inceleyin ve **devam**' ı seçin.

1. Bu hizmet ağı için Kullanıcı dostu bir ad girin ve tamamlanacak **son** ' u seçin.  

1. Dağıtımı izlemek için **görevleri görüntüle** ' yi seçin. 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.":::

   Hizmet kafesi dağıtımı başarıyla tamamlandığında, Hizmetleri yeşil olarak görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Gereç durumunu denetleyerek hizmet kafesinin sistem durumunu doğrulayın, **Interconnect**  >  **gereçlerini**seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

Bu adıma uçtan uca bir genel bakış için, [Azure VMware çözümü-VMware HCX Create Service kafesin](https://www.youtube.com/embed/FyZ0d3P_T24) videosunu görüntüleyin.



### <a name="optional-create-a-network-extension"></a>Seçim Ağ uzantısı oluşturma

Şirket içi ortamınızdan herhangi bir ağı Azure VMware çözümüne genişletmek istiyorsanız, bu ağları genişletmek için aşağıdaki adımları izleyin.

1. **Hizmetler**' ın altında **ağ uzantısı**' nı seçin ve ardından **ağ uzantısı oluştur**' u seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. Azure VMware çözümüne genişletmek istediğiniz ağların her birini seçin ve ardından **İleri**' yi seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.":::

1. Genişletçalıştığınız ağların her biri için şirket içi ağ geçidi IP 'sini girin ve ardından **Gönder**' i seçin. 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin.":::

   Ağ uzantısının tamamlanabilmesi birkaç dakika sürer. Ne zaman, **uzantı tamamlandığında**durum değişikliğini görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="Şirket içi vCenter ' a gidin ve şirket içi vCenter 'nize dağıtmak için bir OVF şablonu seçin." lightbox="media/tutorial-vmware-hcx/extension-complete.png":::

Bu adıma uçtan uca bir genel bakış için [Azure VMware çözümü-VMware HCX ağ uzantısı](https://www.youtube.com/embed/cNlp0f_tTr0) videosunu görüntüleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bu noktaya eriştiyseniz ve gereç bağlantısı tünel durumu **Açık** ve yeşil Ise, VMware HCX kullanarak Azure VMware Çözüm VM 'lerini geçirebilir ve koruyabilirsiniz.  Azure VMware çözümü, iş yükü geçişlerini destekler (ağ uzantısıyla veya ile birlikte).  İş yüklerini vSphere ortamınızda şirket içi ağ oluşturma ve VM 'lerin dağıtımını bu ağlarda geçirmeye devam edebilirsiniz.  HCX kullanma hakkında daha fazla bilgi için VMware teknik belgelerine gidin:

* [VMware HCX belgeleri](https://docs.vmware.com/en/VMware-HCX/index.html)
* [VMware HCX ile sanal makineleri geçirme](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) 
