---
title: Azure VMware çözümleri (AVS)-VMware ufuk 'i kullanarak sanal masaüstü altyapısını barındırmak için AVS özel bulut sitesini kullanma
description: VMware ufuk 'i kullanarak sanal masaüstü altyapısını barındırmak için AVS özel bulut sitenizi nasıl kullanabileceğinizi açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f909ce297344e80ed6004631d5218d6a7fcd5085
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025257"
---
# <a name="use-avs-private-cloud-site-to-host-a-virtual-desktop-infrastructure-using-vmware-horizon"></a>VMware ufuk 'i kullanarak sanal masaüstü altyapısını barındırmak için AVS özel bulut sitesini kullanma

Bir sanal masaüstü altyapısını (VDı) VMware ufuk 7. x kullanarak barındırmak için, AVS özel bulut sitenizi kullanabilirsiniz. Aşağıdaki şekilde VDı için mantıksal çözüm mimarisi gösterilmektedir.

![Ufku dağıtımı](media/horizon-deployment.png)

Bu çözümle, ufuk görünümü Yöneticisi ve uygulama hacmi üzerinde tam denetime sahip olursunuz. Tanıdık UI, API ve CLı arabirimleri, mevcut betiklerinizin ve araçlarınızın kullanımını sağlar.

AVS çözümü şunları yapmanızı gerektirir:

* AVS özel bulutunuzda VMware ufuk 7. x ' i yükler, yapılandırın ve yönetin.
* Kendi ufku lisanslarınızı sağlayın.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Aşağıdaki bölümlerde, AVS özel bulutunuzda ufuk kullanarak bir VDı çözümünün nasıl dağıtılacağı açıklanır.

1. [VMware ürün sürümlerinin uyumlu olduğunu doğrulama](#verify-that-vmware-product-versions-are-compatible)
2. [Masaüstü ortamınızın boyutunu tahmin etme](#estimate-the-size-of-your-desktop-environment)
3. [Ortamınız için bir AVS özel bulutu oluşturun](#create-an-avs-private-cloud-for-your-environment)
4. [AVS özel bulutunuzda VMware ufuk 'i 'ni yükler](#install-vmware-horizon-in-your-avs-private-cloud)

### <a name="verify-that-vmware-product-versions-are-compatible"></a>VMware ürün sürümlerinin uyumlu olduğunu doğrulama

* Geçerli ve planlı sürümlerinin, uygulama birimlerinin, Birleşik erişim ağ geçidinizin ve Kullanıcı ortam yöneticisi 'nin birbirleriyle ve AVS özel bulutundaki vCenter ve PSC ile uyumlu olduğunu doğrulayın. Uyumluluk bilgileri için bkz. [ufuk 7,5 Için VMware uyumluluk matrisi](https://www.vmware.com/resources/compatibility/sim/interop_matrix.php#interop&260=2877&0=).
* AVS özel bulutunuzda vCenter ve PSC 'nin güncel sürümlerini bulmak için, [AVS portalındaki](access-cloudsimple-portal.md) **kaynaklar** ' a gidin, AVS özel bulutunuzu seçin ve **vSphere yönetim ağı** sekmesine tıklayın.

![vCenter ve PSC sürümleri](media/private-cloud-vsphere-versions.png)

### <a name="estimate-the-size-of-your-desktop-environment"></a>Masaüstü ortamınızın boyutunu tahmin etme

* Tanımlı yapılandırmanızın VMware işlemsel sınırları içinde olduğunu doğrulayın.
* Tüm masaüstleri ve ufuk yönetimi bileşenleriniz için gereken kaynakları tahmin edin.

### <a name="create-an-avs-private-cloud-for-your-environment"></a>Ortamınız için bir AVS özel bulutu oluşturun

1. AVS [özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md)konusundaki yönergeleri izleyerek AVS PORTALıNDAN bir AVS özel bulutu oluşturun. AVS, yeni oluşturulan AVS özel bulutunda ' cloudowner ' adlı varsayılan bir vCenter kullanıcısı oluşturur. Varsayılan AVS özel bulut kullanıcısı ve izin modeli hakkında daha fazla bilgi için bkz. [AVS özel bulut izinleri modelini öğrenme](learn-private-cloud-permissions.md).
2. Ufıt yönetim düzlemi için AVS özel bulutunuzda bir VLAN oluşturun ve bir alt ağ CıDR atayın. Yönergeler için bkz. [VLAN/alt ağlar oluşturma ve yönetme](create-vlan-subnet.md). Bu, tüm çözüm bileşenlerinin (Birleşik erişim ağ geçidi, bağlantı sunucusu, App Volume Server ve Kullanıcı ortam yöneticisi sunucuları) yükleneceği ağ ' dır.
3. AVS özel bulut vCenter 'unuzda bir dış kimlik sağlayıcısı kullanmak istediğinize karar verin. Yanıt Evet ise, şu seçeneklerden birini seçin:
    * Şirket içi Active Directory dış kimlik sağlayıcısı olarak kullanın. Yönergeler için bkz. [vCenter Identity Sources](set-vcenter-identity.md).
    * Dış kimlik sağlayıcınız olarak kullanmak için, ufuk yönetim düzlemi VLAN 'daki AVS özel bulutu 'nda bir Active Directory sunucusu ayarlayın. Yönergeler için bkz. [vCenter Identity Sources](set-vcenter-identity.md).
    * AVS özel bulutu 'nda ufuk yönetimi düzlemi VLAN 'da bir DHCP ve DNS sunucusu ayarlayın. Yönergeler için bkz. [AVS özel bulutunuzda DNS ve DHCP uygulamalarını ve iş yüklerini ayarlama](dns-dhcp-setup.md).
4. AVS özel bulutunda yüklü olan DNS sunucusunda DNS iletmeyi yapılandırın. Yönergeler için bkz. [koşullu Iletici oluşturma](on-premises-dns-setup.md#create-a-conditional-forwarder).

### <a name="install-vmware-horizon-in-your-avs-private-cloud"></a>AVS özel bulutunuzda VMware ufuk 'i 'ni yükler

Aşağıdaki dağıtım diyagramı, bir AVS özel bulutu 'nda dağıtılan bir ufuk çözümünü göstermektedir. Birleşik erişim ağ geçidi, AD/DC, görünüm ve App Volume sunucusu, Kullanıcı tarafından oluşturulan VLAN 234 ' de yüklenir. Birleşik erişim ağ geçidi, Internet 'ten erişilebilen atanmış bir genel IP adresine sahiptir. Ufuk masaüstü havuzu VM 'Leri, ek yalıtım ve güvenlik sağlamak için VLAN 235 ' de dağıtılır.

![AVS özel bulutu 'nda ufuk dağıtımı](media/horizon-private-cloud.png)

Aşağıdaki bölümlerde, şekilde gösterildiği gibi bir dağıtımı ayarlama yönergeleri ana hatlarıyla gösterilmiştir. Başlamadan önce, aşağıdakilere sahip olduğunuzu doğrulayın:

* Masaüstü havuzlarınızı çalıştırmak için yeterli kapasiteye sahip AVS Portalı kullanılarak oluşturulan bir AVS özel bulutu.
* Masaüstleri için ağ trafiğini desteklemek üzere şirket içi ortamınız ve AVS özel bulut ortamı arasında yeterli bant genişliği.
* Şirket içi veri merkeziniz ve AVS özel bulutu arasında bir siteden siteye VPN tüneli ayarlanır.
* Şirket içi ortamınızdaki Son Kullanıcı alt ağlarından, AVS özel bulut alt ağlarına IP erişilebilirliği.
* AVS özel bulutunuz için AD/DHCP/DNS yüklendi.

#### <a name="avs-portal-create-a-dedicated-vlansubnet-for-desktop-pools"></a>AVS Portalı: masaüstü havuzları için adanmış VLAN/alt ağ oluşturma

Ufıt masaüstü havuzları için bir VLAN oluşturun ve bir alt ağ CıDR atayın. Yönergeler için bkz. [VLAN/alt ağlar oluşturma ve yönetme](create-vlan-subnet.md). Bu, tüm Masaüstü sanal makinelerinin çalışacağı ağ olur.

Ufuk dağıtımınızın güvenliğini sağlamak için standart güvenlik en iyi yöntemlerini izleyin:

* Masaüstü sanal makinelerinize yalnızca masaüstü RDP trafiğine/SSH trafiğine izin verin.
* Yalnızca Ufkyönetim düzlemi VLAN ve masaüstü havuzu VLAN arasındaki Yönetim trafiğine izin verin.
* Yalnızca şirket içi ağdan gelen Yönetim trafiğine izin verin.

Bu en iyi uygulamaları, AVS portalından [güvenlik duvarı kurallarını](firewall.md) yapılandırarak uygulayabilirsiniz.

#### <a name="avs-portal-configure-firewall-rules-to-secure-horizon-management-plane"></a>AVS Portalı: güvenlik duvarı kurallarını güvenli hale getirmek için yönetim düzleminde yapılandırma

AVS portalında aşağıdaki kuralları ayarlayın. Yönergeler için bkz. [güvenlik duvarı tablolarını ve kurallarını ayarlama](firewall.md).

1. Yalnızca VMware belge [ufku bağlantı noktası listesinde](https://docs.vmware.com/en/VMware-Horizon-7/7.1/com.vmware.horizon-client-agent.security.doc/GUID-52807839-6BB0-4727-A9C7-EA73DE61ADAB.html) listelenen ağ bağlantı noktalarına izin verilmesi IÇIN, AVS N-S güvenlik duvarında güvenlik duvarı kurallarını şirket içi alt ağlar ve ufuk yönetimi VLAN 'ları arasında iletişime izin verecek şekilde yapılandırın.

2. AVS özel bulutu 'ndaki ufuk yönetimi VLAN ve masaüstü havuzu VLAN arasında E-W güvenlik duvarı kuralları oluşturun.

#### <a name="avs-portal-create-a-public-ip-address-for-unified-access-gateway"></a>AVS Portalı: Birleşik erişim ağ geçidi için genel bir IP adresi oluşturma

Internet 'ten masaüstü istemci bağlantılarını etkinleştirmek üzere Birleşik erişim ağ geçidi gereci için genel bir IP adresi oluşturun. Yönergeler için bkz. [genel IP adresleri ayırma](public-ips.md).

Kurulum tamamlandığında genel IP adresi atanır ve genel IP 'Ler sayfasında listelenir.

#### <a name="avs-portal-escalate-privileges"></a>AVS Portalı: Ilerleme ayrıcalıkları

Varsayılan ' cloudowner ' kullanıcısının, ufku yüklemek için AVS özel bulut vCenter 'da yeterli ayrıcalıkları yoktur, bu nedenle kullanıcının vCenter ayrıcalıkları ilerletilmiş olmalıdır. Daha fazla bilgi için bkz. [ayrıcalıkları Yükselt](escalate-private-cloud-privileges.md).

#### <a name="vcenter-ui-create-a-user-in-avs-private-cloud-for-horizon-installation"></a>vCenter kullanıcı ARABIRIMI: AVS için özel bulutta bir Kullanıcı oluşturma-ufku yüklemesi

1. ' Cloudowner ' Kullanıcı kimlik bilgilerini kullanarak vCenter 'da oturum açın.
2. VCenter 'da ' ufksoln-admin ' yeni bir kullanıcı oluşturun ve Kullanıcı ' yı vCenter 'daki Yöneticiler grubuna ekleyin.
3. ' Cloudowner ' kullanıcısı olarak vCenter oturumunu kapatın ve ' ufuk-soln-Yönetici ' kullanıcısı olarak oturum açın.

#### <a name="vcenter-ui-install-vmware-horizon"></a>vCenter Kullanıcı arabirimi: VMware ufku 'ı yükler

Önceki mantıksal mimari bölümünde belirtildiği gibi, ufuk çözümü aşağıdaki bileşenlere sahiptir:

* VMware ufuk görünümü
* VMware Birleşik erişim ağ geçidi
* VMware App Volume Manager
* VMware Kullanıcı ortamı Yöneticisi

Bileşenleri aşağıdaki gibi yükler:

1. VMware [birleştirilmiş erişim ağ geçidini dağıtma ve yapılandırma](https://docs.vmware.com/en/Unified-Access-Gateway/3.3.1/com.vmware.uag-331-deploy-config.doc/GUID-F5CE0D5E-BE85-4FA5-BBCF-0F86C9AB8A70.html)VMware belgesinde belirtilen yönergeleri Izleyerek Birleşik erişim ağ geçidini yükleyip yapılandırın.

2. [Yükleme kılavuzunu görüntüle](https://docs.vmware.com/en/VMware-Horizon-7/7.4/horizon-installation/GUID-37D39B4F-5870-4188-8B11-B6C41AE9133C.html)bölümündeki yönergeleri IZLEYEREK, AVS özel bulutuna ufuk görünümü ' nu yükleme.

3. [VMware App Volumes 'ı yükleyip yapılandırma](https://docs.vmware.com/en/VMware-App-Volumes/2.10/com.vmware.appvolumes.user.doc/GUID-5E8BAF8C-F5A6-412C-9424-266BA7109BA4.html)bölümündeki yönergeleri Izleyerek App Volume Manager 'ı yükler.

4. [VMware Kullanıcı ortamı yöneticisini yükleme ve yapılandırma hakkındaki](https://docs.vmware.com/en/VMware-User-Environment-Manager/9.4/com.vmware.user.environment.manager-install-config/GUID-DBBC82E4-483F-4B28-9D49-4D28E08715BC.html)yönergeleri Izleyerek Kullanıcı ortam yöneticisi 'ni yükleme ve yapılandırma.

#### <a name="file-a-support-request-to-upload-vmware-horizon-pre-packaged-app-volumes"></a>VMware ufuk ön paketlenmiş uygulama birimlerini karşıya yüklemek için bir destek isteği dosyası oluşturma

App Volume Manager, yükleme işleminin bir parçası olarak, uygulama yığınları ve yazılabilir birimler sağlamak için önceden paketlenmiş birimleri kullanır. Bu birimler, uygulama yığınları ve yazılabilir birimler için şablon olarak görev yapar.

Birimleri AVS özel bulut veri deposuna yüklemek için ESXi kök parolası gerekir. Yardım almak için bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderebilirsiniz. AVS destek personelinin, şablonları AVS özel bulut ortamınıza yükleyebilmeleri için AppVolumes yükleyici paketini iliştirin.

#### <a name="avs-portal-de-escalate-privileges"></a>AVS Portalı: yükseltme ayrıcalıkları

Artık ' cloudowner ' kullanıcısı için [ayrıcalıkların kullanımını devre dışı](escalate-private-cloud-privileges.md#de-escalate-privileges) bırakabilirsiniz.

## <a name="ongoing-management-of-your-horizon-solution"></a>Ufuk çözümünüzün devam eden yönetimi

AVS özel bulut ortamınızda ufku ve App Volume Manager yazılımı üzerinde tam denetime sahipsiniz ve gerekli yazılım yaşam döngüsü yönetimini gerçekleştiriyorduk. Ufuk veya uygulama birimini güncelleştirmeden veya yükseltmeden önce yazılım sürümlerinin herhangi bir yeni sürümünün AVS özel bulut vCenter ve PSC ile uyumlu olduğundan emin olun.
