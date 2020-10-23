---
title: Öğretici-VMware HCX 'i dağıtma ve yapılandırma
description: Azure VMware çözümünüz özel bulutunuz için bir VMware HCX çözümünü dağıtmayı ve yapılandırmayı öğrenin.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: c78eae11497702054bb54b5980228fd0a3962577
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367780"
---
# <a name="deploy-and-configure-vmware-hcx"></a>VMware HCX’i dağıtma ve yapılandırma

Bu makalede, Azure VMware çözümünüz özel bulutunuz için şirket içi VMware HCX bağlayıcısını dağıtma ve yapılandırma yordamlarına adım adım ilerliyoruz. VMware HCX ile, VMware iş yüklerinizi Azure VMware çözümüne ve diğer bağlı sitelere çeşitli geçiş türleri aracılığıyla geçirebilirsiniz. Azure VMware çözümü HCX Cloud Manager 'ı dağıttığı ve yapılandıracağından, HCX bağlayıcısını şirket içi VMware veri merkezinizde indirmeniz, etkinleştirmeniz ve yapılandırmanız gerekir.

VMware HCX gelişmiş Bağlayıcısı, Azure VMware çözümünde önceden dağıtılır. En fazla üç site bağlantısını (Şirket içi buluta veya buluta buluta) destekler. Üçten fazla site bağlantısına ihtiyacınız varsa [VMware HCX kurumsal](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/) eklentisini etkinleştirmek için bir [destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support) gönderebilirsiniz. Eklenti şu anda önizleme aşamasındadır. 

>[!NOTE]
>VMware HCX Enterprise Edition (EE), Azure VMware çözümüyle bir önizleme hizmeti olarak sunulmaktadır. Bu ücretsizdir ve bir önizleme hizmeti için hüküm ve koşullara tabidir. VMware HCX EE hizmeti genel kullanıma sunulduğunda, faturalandırmaya geçiş yapılacak 30 günlük bir bildirim alırsınız. Ayrıca hizmeti kapatma veya devre dışı bırakma seçeneğiniz de vardır.

İlk olarak, bu makalenin [başlamadan önce](#before-you-begin), [yazılım sürümü gereksinimleri](#software-version-requirements)ve [Önkoşullar](#prerequisites) bölümündeki ' yi ayrıntılı olarak gözden geçirin. 

Ardından, aşağıdakileri yapmak için gereken tüm yordamları inceleyeceğiz:

> [!div class="checklist"]
> * Şirket içi VMware HCX OVA (HCX bağlayıcısını) dağıtın.
> * VMware HCX bağlayıcısını etkinleştirin.
> * Azure VMware çözümünüz HCX Cloud Manager ile şirket içi HCX bağlayıcınızı eşleştirin.
> * InterConnect 'i yapılandırın (ağ profili, işlem profili ve hizmet ağı).
> * Gereç durumunu denetleyerek ve geçişin mümkün olduğunu doğrulayarak kurulumu tamamen doldurun.

İşiniz bittiğinde, bu makalenin sonunda önerilen sonraki adımları izleyebilirsiniz.  

## <a name="before-you-begin"></a>Başlamadan önce
   
* Temel Azure VMware Çözüm Software-Defined veri merkezi (SDDC) [öğretici serisini](tutorial-network-checklist.md)gözden geçirin.
* HCX Kullanıcı Kılavuzu dahil olmak üzere [VMware HCX belgelerini](https://docs.vmware.com/en/VMware-HCX/index.html)inceleyin ve başvuru yapın.
* VMware docs üzerinde [VMware HCX Ile sanal makinelerin geçişini](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) inceleyin.
* İsteğe bağlı olarak [VMware HCX dağıtım konularını](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html)gözden geçirin.
* İsteğe bağlı olarak, VMware vSphere [blog serisi](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html)gibi HCX üzerinde ilgili VMware malzemelerini gözden geçirin. 
* İsteğe bağlı olarak Azure VMware Çözüm desteği kanalları aracılığıyla bir Azure VMware çözümü HCX Kurumsal etkinleştirme isteyin.
* İsteğe bağlı olarak [HCX için gereken ağ bağlantı noktalarını gözden geçirin](https://ports.vmware.com/home/VMware-HCX).
* Azure VMware çözümü HCX Cloud Manager, Azure VMware Çözüm özel bulutu için sunulan/22 ağından önceden yapılandırılmış olsa da şirket içi HCX Bağlayıcısı, şirket içi ağınızdan ağ aralıklarını ayırmanız gerekir. Bu ağlar ve aralıklar Bu makalenin ilerleyen kısımlarında açıklanmıştır.

İşlem ve depolama kaynaklarına karşı iş yüklerini boyutlandırma, önemli bir planlama adımıdır. Bir özel bulut ortamı için ilk planlamanın parçası olarak boyutlandırma adımını ele edin. 

Azure geçişi portalında bir [Azure VMware Çözüm değerlendirmesi](../migrate/how-to-create-azure-vmware-solution-assessment.md) tamamlayarak iş yüklerini boyut olarak kullanabilirsiniz.

## <a name="software-version-requirements"></a>Yazılım sürümü gereksinimleri

Altyapı bileşenleri, gerekli en düşük sürümü çalıştırıyor olmalıdır. 
                                                         
| Bileşen türü    | Kaynak ortam gereksinimleri    | Hedef ortam gereksinimleri   |
| --- | --- | --- |
| vCenter Server   | 5.1<br/><br/>5,5 U1 veya daha önceki bir sürümü kullanıyorsanız HCX işlemleri için tek başına HCX Kullanıcı arabirimini kullanın.  | 6,0 U2 ve üzeri   |
| ESXi   | 5.0    | ESXi 6,0 ve üzeri   |
| NSX    | Kaynak: NSXv 6.2 + veya NSX-T 2.4 + konumundaki mantıksal anahtarların HCX ağ uzantısı için.   | NSXv 6.2 + veya NSX-T 2,4 +<br/><br/>HCX yakınlık yönlendirmesi için: NSXv 6.4 +. (Yakınlık yönlendirme NSX-T ile desteklenmez.) |
| vCloud Direktörü   | Gerekli değildir. Kaynak sitede vCloud Director ile birlikte çalışabilirlik yoktur. | Hedef ortamı vCloud Director ile tümleştirdiğinizde, en az 9.1.0.2 olur.  |

## <a name="prerequisites"></a>Ön koşullar

### <a name="network-and-ports"></a>Ağ ve bağlantı noktaları

* Şirket içi ve Azure VMware çözümü SDDC ExpressRoute devreleri arasında [Azure expressroute Global Reach](tutorial-expressroute-global-reach-private-cloud.md) yapılandırın.

* Şirket içi bileşenler ve Azure VMware çözümü SDDC arasındaki iletişim için [gereken tüm bağlantı noktaları](https://ports.vmware.com/home/VMware-HCX) açık olmalıdır.

Daha fazla bilgi için bkz. [VMware HCX belgeleri](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html).


### <a name="ip-addresses"></a>IP adresleri

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-connector-ova-on-premises"></a>VMware HCX bağlayıcı OVA 'yı şirket içinde dağıtma

> [!NOTE]
> Sanal Gereci şirket içi vCenter 'ınıza dağıtmadan önce, VMware HCX bağlayıcı OVA 'yı indirmeniz gerekir. 

1. Bir tarayıcı penceresi açın, 443 numaralı bağlantı noktasındaki Azure VMware Çözüm HCX Yöneticisi ' nde `https://x.x.x.9` **cloudadmin** Kullanıcı kimlik bilgileriyle oturum açın ve ardından **destek**bölümüne gidin.

   > [!TIP]
   > Azure VMware çözümünde HCX bulut yöneticisinin IP adresini göz önünde edin. IP adresini belirlemek için, Azure VMware Çözüm bölmesinde bağlantıyı **Yönet**' e gidin  >  **Connectivity** ve ardından **HCX** sekmesini seçin. 
   >
   >VCenter parolası, özel bulutu ayarlarken tanımlanmıştır.

1. VMware HCX bağlayıcı OVA dosyasını indirmek için **indirme** bağlantısını seçin.

1. Şirket içi vCenter ' a gidin. HCX bağlayıcısını şirket içi vCenter 'ınıza dağıtmak için indirdiğiniz OVA dosyası olan bir OVF şablonu seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. Bir ad ve konum seçin ve HCX bağlayıcısını dağıttığınız kaynağı/kümeyi seçin. Ardından ayrıntıları ve gerekli kaynakları gözden geçirin.  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. Lisans koşullarını gözden geçirin. Kabul ediyorsanız gereken depolama ve ağ ' ı seçin ve ardından **İleri**' yi seçin.

1. **Şablonu Özelleştir**' de gerekli tüm bilgileri girin. 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. **İleri**' yi seçin, yapılandırmayı doğrulayın ve sonra HCX BAĞLAYıCı ova 'yı dağıtmak için **son** ' u seçin.
     
   > [!NOTE]
   > Genellikle, dağıtmakta olduğunuz VMware HCX Bağlayıcısı, kümenin yönetim ağı üzerine dağıtılır.  
   
   > [!IMPORTANT]
   > Sanal Gereci el ile açmanız gerekebilir.  Bu durumda, sonraki adıma geçmeden önce 10-15 dakika bekleyin.

Bu yordama uçtan uca genel bakış için [Azure VMware çözümü: HCX gereç dağıtımı](https://www.youtube.com/embed/BwSnQeefnso) videosunu görüntüleyin. 


## <a name="activate-vmware-hcx"></a>VMware HCX 'i etkinleştir

VMware HCX bağlayıcısını şirket içinde dağıttıktan sonra gereci başlattıktan sonra, ' yi etkinleştirmeye hazırsınız demektir. İlk olarak, Azure VMware Çözüm Portalından bir lisans anahtarı almanız gerekir.

1. Azure VMware Çözüm portalında **Manage**  >  **bağlantıyı**Yönet ' e gidin, **HCX** sekmesini seçin ve ardından **Ekle**' yi seçin.

1. Şirket içi VMware HCX yöneticisinde oturum açmak için **yönetici** kimlik bilgilerini kullanın `https://HCXManagerIP:9443` . 

   > [!IMPORTANT]
   > `9443`VMware HCX Manager IP adresine sahip bağlantı noktası numarasını eklediğinizden emin olun.

1. **Lisanslama**bölümünde **HCX gelişmiş anahtarı**için anahtarınızı girin.  
   
    > [!NOTE]
    > VMware HCX Manager 'da açık internet erişimi veya proxy yapılandırılmış olmalıdır.

1. **Veri merkezi konumunda**, VMware HCX yöneticisini şirket içi yükleme için en yakın konumu belirtin.

1. **Sistem adı**' nda adı değiştirin veya varsayılanı kabul edin.
   
1. **Evet, devam et ' i**seçin.
    
1. **VCenter 'Larınızı bağlayın**bölümünde vCenter sunucunuzun FQDN 'SINI veya IP adresini ve uygun kimlik bilgilerini girip **devam**' ı seçin.
   
1. **SSO/PSC yapılandırma**bölümünde, platform HIZMETLERI denetleyicinizin FQDN 'SINI veya IP adresini girip **devam**' ı seçin.
   
   >[!NOTE]
   >Genellikle, bu giriş vCenter FQDN 'niz veya IP adresiniz ile aynıdır.

1. Tüm girdilerin doğru olduğundan emin olun ve **Yeniden Başlat**' ı seçin.
    
   > [!NOTE]
   > Sonraki adımda uyarılmadan önce yeniden başlattıktan sonra bir gecikme yaşarsınız.

Hizmetler yeniden başlatıldıktan sonra, açılan ekran üzerinde görünen vCenter ' ı görmeniz önemlidir. Hem vCenter hem de SSO, önceki ekranla aynı olması gereken uygun yapılandırma parametrelerine sahip olmalıdır.

:::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

Bu yordama uçtan uca genel bakış için [Azure VMware çözümünü görüntüleyin: HCX videosunu etkinleştirin](https://www.youtube.com/embed/BkAV_TNYxdE) .


## <a name="configure-the-vmware-hcx-connector"></a>VMware HCX bağlayıcısını yapılandırma

Artık bir site eşleştirme eklemeye, bir ağ ve bilgi işlem profili oluşturmaya ve geçiş, ağ uzantısı ya da olağanüstü durum kurtarma gibi hizmetleri etkinleştirmeye hazır olursunuz. 

### <a name="add-a-site-pairing"></a>Site eşleştirmesi ekleme

Azure VMware çözümünde VMware HCX bulut yöneticisini, veri merkezinizdeki VMware HCX Bağlayıcısı ile bağlayabilirsiniz (eşleştirin). 

1. Şirket içi vCenter oturumunuzu açın ve **giriş**bölümünde **HCX**' i seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. **Altyapı**altında **site eşleştirme**' yi seçin ve ardından **uzak siteye Bağlan** seçeneğini belirleyin (ekranın ortasında). 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. Daha önce not ettiğiniz uzak HCX URL 'sini veya IP adresini, Azure VMware Çözüm cloudadmin@vsphere.local Kullanıcı adı ' nı ve parolayı girin. Ardından **Bağlan**'ı seçin.

   > [!NOTE]
   > Bir site çiftini başarıyla oluşturmak için, HCX bağlayıcısının 443 numaralı bağlantı noktası üzerinden HCX Cloud Manager IP 'nize yönlendirilebilmesi gerekir.
   >
   > Parola, vCenter 'da oturum açmak için kullandığınız parolayla aynıdır. Bu parolayı ilk dağıtım ekranında tanımladınız.

   Azure VMware çözümünde HCX bulut yöneticinizin ve şirket içi HCX bağlayıcısının bağlı (eşleştirilmiş) olduğunu gösteren bir ekran görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="OVF şablonuna göz atma ekran görüntüsü.":::

Bu yordama uçtan uca genel bakış için [Azure VMware çözümü: HCX site eşleştirme](https://www.youtube.com/embed/sKizDCRHOko) videosunu görüntüleyin.



### <a name="create-network-profiles"></a>Ağ profilleri oluşturma

VMware HCX, birden çok IP kesimi gerektiren sanal gereçler (otomatik) bir alt kümesini dağıtır. Ağ profillerinizi oluştururken, [VMware HCX ağ kesimleri dağıtım öncesi hazırlama ve planlama aşaması](production-ready-deployment-steps.md#vmware-hcx-network-segments)SıRASıNDA tanımladığınız IP segmentlerini tanımlarsınız.

Dört ağ profili oluşturacaksınız:

   - Yönetim
   - vMotion
   - Çoğaltma
   - Yukarı

1. **Altyapı**altında, **bağlantı**  >  **çok siteli hizmet**  >  **ağı ağ profilleri**ağ  >  **profili oluştur**' u seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. Her ağ profili için, ağ ve bağlantı noktası grubunu seçin, bir ad girin ve bu segment için IP havuzu oluşturun. Ardından **Oluştur**’u seçin. 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="OVF şablonuna göz atma ekran görüntüsü.":::

Bu yordama uçtan uca genel bakış için [Azure VMware çözümü: HCX ağ profili](https://www.youtube.com/embed/NhyEcLco4JY) videosunu görüntüleyin.


### <a name="create-a-compute-profile"></a>İşlem profili oluşturma

1. **İşlem profilleri**  >  **işlem profili oluştur**' u seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. Profil için bir ad girin ve **devam**' ı seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Geçiş, ağ uzantısı veya olağanüstü durum kurtarma gibi etkinleştirilecek hizmetleri seçin ve ardından **devam**' ı seçin.
  
   > [!NOTE]
   > Genellikle burada hiçbir şey değişiklik yapmaz.

1. **Hizmet kaynaklarını Seç**bölümünde, seçilen VMware HCX hizmetlerini etkinleştirmek için bir veya daha fazla hizmet kaynağı (küme) seçin.  

1. Şirket içi veri merkezinizde kümeleri gördüğünüzde **devam**' ı seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. **Veri deposundan Seç**' den, VMware HCX Interconnect gereçlerini dağıtmak için veri deposu depolama kaynağını seçin. Daha sonra **Devam** seçeneğini belirleyin.

   Birden çok kaynak seçildiğinde, VMware HCX, kapasitesi tükenene kadar seçilen ilk kaynağı kullanır.   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. **Yönetim ağı profilini seç**' den önceki adımlarda oluşturduğunuz yönetim ağı profilini seçin. Daha sonra **Devam** seçeneğini belirleyin.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > Yönetim ağı profili, VMware HCX gereçlerinin vCenter ile iletişim kurmasına izin verir. ESXi konaklarına bu profil üzerinden ulaşırmış olabilir.

1. **Yukarı bağlantı ağı profilini seç**' den önceki adımlarda oluşturduğunuz yukarı bağlantı ağ profilini seçin. Daha sonra **Devam** seçeneğini belirleyin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. **VMotion ağ profilini seç**' den önceki adımlarda oluşturduğunuz vMotion ağ profilini seçin. Daha sonra **Devam** seçeneğini belirleyin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. **VSphere çoğaltma ağı profilini seçin**bölümünde, önceki adımlarda oluşturduğunuz çoğaltma ağ profilini seçin. Daha sonra **Devam** seçeneğini belirleyin.

   Çoğu durumda, çoğaltma ağ profili, yönetim ağ profiliyle aynı olur.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. **Ağ uzantıları Için dağıtılmış anahtarlar seçin**sayfasında, katman 2 genişletilmiş ağ üzerinde Azure VMware çözümüne geçirilecek sanal makineleri içeren dağıtılmış sanal anahtarları seçin. Daha sonra **Devam** seçeneğini belirleyin.

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Bağlantı kurallarını gözden geçirin ve **devam**' ı seçin.  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. İşlem profili oluşturmak için **son** ' u seçin.


   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

Bu yordama uçtan uca genel bakış için [Azure VMware çözümü: Işlem profili](https://www.youtube.com/embed/qASXi5xrFzM) videosu ' nı görüntüleyin.

### <a name="create-a-service-mesh"></a>Hizmet ağı oluşturma

Artık şirket içi ve Azure VMware Çözüm SDDC arasında bir hizmet ağı yapılandırma zamanı.

   > [!NOTE]
   > Azure VMware çözümü ile başarılı bir şekilde hizmet ağı kurmak için:
   >
   > UDP 500/4500 bağlantı noktaları, şirket içi HCX bağlayıcı tanımlı ' yukarı bağlantı ' ağ profili adresiniz ve Azure VMware çözümü HCX bulutu ' yukarı ağ profili adresleri arasında açıktır.
   >
   > [HCX gereken bağlantı noktalarını](https://ports.vmware.com/home/VMware-HCX)gözden geçirdiğinizden emin olun.

1. **Altyapı**altında, **Interconnect**  >  **Service kafesi**  >  **hizmet ağı oluştur**' u seçin.    

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Önceden doldurulmuş siteleri gözden geçirin ve ardından **devam**' ı seçin. 

   >[!NOTE]
   >Bu ilk hizmet ağı yapılandırmanızda ise, bu ekranı değiştirmeniz gerekmez.  

1. Açılan listelerden kaynak ve Uzaktan işlem profillerini seçip **devam**' ı seçin.  

   Seçimler, VM 'Lerin VMware HCX hizmetlerini kullanabileceği kaynakları tanımlar.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Etkinleştirilecek hizmetleri gözden geçirin ve ardından **devam**' ı seçin.  

1. **Gelişmiş yapılandırma-yukarı bağlantı ağ profillerini geçersiz kıl**, **devam**' ı seçin.  

   Yukarı bağlantı ağ profilleri, uzak sitenin Interconnect gereçlerine erişilebileceği ağa bağlanır.  
  
1. **Gelişmiş yapılandırma-ağ uzantısı gereç ölçeği ölçeğini**Inceleyin ve **devam**' ı seçin. 

1. **Gelişmiş yapılandırma-Trafik Mühendisliği**' nda, gereken tüm değişiklikleri gözden geçirin ve yapın ve ardından **devam**' ı seçin.

1. Topoloji önizlemesini inceleyin ve **devam**' ı seçin.

1. Bu hizmet ağı için Kullanıcı dostu bir ad girin ve tamamlanacak **son** ' u seçin.  

1. Dağıtımı izlemek için **görevleri görüntüle** ' yi seçin. 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="OVF şablonuna göz atma ekran görüntüsü.":::

   Hizmet kafesi dağıtımı başarıyla tamamlandığında, Hizmetleri yeşil olarak görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Gereç durumunu denetleyerek hizmet kafesinin sistem durumunu doğrulayın. 
1. **Interconnect**  >  **gereçlerini**seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

Bu yordama uçtan uca genel bakış için [Azure VMware çözümü: hizmet ağı](https://www.youtube.com/embed/FyZ0d3P_T24) videosu ' nı görüntüleyin.



### <a name="optional-create-a-network-extension"></a>Seçim Ağ uzantısı oluşturma

Şirket içi ortamınızdan herhangi bir ağı Azure VMware çözümüne genişletmek istiyorsanız aşağıdaki adımları izleyin:

1. **Hizmetler**' ın altında **ağ uzantısı**' nı seçin ve ardından **ağ uzantısı oluştur**' u seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. Azure VMware çözümüne genişletmek istediğiniz ağların her birini seçin ve ardından **İleri**' yi seçin.

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="OVF şablonuna göz atma ekran görüntüsü.":::

1. Genişletçalıştığınız ağların her biri için şirket içi ağ geçidi IP 'sini girin ve ardından **Gönder**' i seçin. 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="OVF şablonuna göz atma ekran görüntüsü.":::

   Ağ uzantısının tamamlanabilmesi birkaç dakika sürer. Ne zaman, **uzantı tamamlandığında**durum değişikliğini görürsünüz.

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="OVF şablonuna göz atma ekran görüntüsü." lightbox="media/tutorial-vmware-hcx/extension-complete.png":::

Bu adıma uçtan uca genel bakış için [Azure VMware çözümü: ağ uzantısı](https://www.youtube.com/embed/cNlp0f_tTr0) videosunu görüntüleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bu noktaya eriştiyseniz ve gereç bağlantısı tünel durumu **Açık** ve yeşil Ise, VMware HCX kullanarak Azure VMware Çözüm VM 'lerini geçirebilir ve koruyabilirsiniz. Azure VMware çözümü, iş yükü geçişlerini destekler (ağ uzantısı olan veya olmayan). Yine de sanal iş yüklerini vSphere ortamınızda geçirebilirsiniz, bu ağlarda ağların şirket içi olarak oluşturulmasını ve VM 'lerin dağıtımını yapabilirsiniz.  

HCX kullanma hakkında daha fazla bilgi için VMware teknik belgelerine gidin:

* [VMware HCX belgeleri](https://docs.vmware.com/en/VMware-HCX/index.html)
* [VMware HCX Ile sanal makineler geçiriliyor](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* [HCX gereken bağlantı noktaları](https://ports.vmware.com/home/VMware-HCX)
