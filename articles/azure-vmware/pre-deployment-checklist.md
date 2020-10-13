---
title: Azure VMware çözümü dağıtım öncesi denetim listesi
description: Dağıtım öncesi planlama sürecinin bir parçası olarak bu denetim listesini kullanın.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: 51480f72d6de6b1f49aa753e006c4683bbd48cc7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91580534"
---
# <a name="azure-vmware-solution-pre-deployment-checklist"></a>Azure VMware çözümü dağıtım öncesi denetim listesi
[Planlama aşamasında](production-ready-deployment-steps.md)bu dağıtım öncesi denetim listesini kullanacaksınız.

## <a name="success-criteria"></a>Başarı ölçütleri
Hangi testlerin çalıştırılacağını ve beklenen sonucu tanımlayın.

| Gerekli temel bilgiler | Yanıtınız |
| ----------- | ------------- |
| SDDC dağıtma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Sanal ağ oluşturma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Sıçrama kutusu oluştur | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Sanal ağ geçidi oluştur | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| ExpressRoute Global Reach bağlama | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 
| NSX Manager ve vCenter 'da oturum açın | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |



| Gerekli birincil bilgiler | Yanıtınız |
| --------| --------------|
| DHCP sunucusu oluştur | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Ağ kesimleri oluşturma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Ölçek (düğüm ekleme veya silme) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| VMware HCX dağıtma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Sanal makineler (VM 'Ler) oluşturma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
| İnternet 'i etkinleştir | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Şirket içinden özel buluta sanal makine geçişi | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |



| Ek bilgi gerekli | Yanıtınız |
| --------| --------------|
| VM anlık görüntü işlemleri | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 
| NSX-T yük dengeleyiciyi dağıtma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Azure tümleştirmesi<br><ul><li>Paylaşılan içerik kitaplığı</li><li>Güvenlik Tümleştirmesi</li><li>ISO yükleme</li><li>ISO 'dan oluştur</li><li>Azure Backup</li></ul> | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Mikro segmentasyon | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Yönlendirme | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |




## <a name="azure-vmware-solution-information"></a>Azure VMware Çözüm bilgileri

#### <a name="azure-subscription"></a>Azure aboneliği
Azure VMware çözümü için abonelik adı ve abonelik KIMLIĞI ' ni belirtin. Abonelik yeni veya mevcut bir abonelik olabilir. Geliştirme ve test aboneliği kullanmayın.

| Gerekli bilgiler  | Yanıtınız |
| ------------| ------------- |
| Abonelik ve KIMLIK | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| EA ile abonelik | Evet &nbsp; &nbsp; ☐ Hayır ☐  |
| Kaynak grubu adı | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Konum | ☒ Doğu ABD |
| Azure Yöneticisi<br><br>Yöneticiye ad ve ilgili kişi sağlama<br>hizmetin Market 'ten etkinleştirilmesi için atanır.<br>Katkıda bulunan, için gereken en düşük roldür <br>[Azure VMware Çözüm kaynak sağlayıcısını kaydedin](tutorial-create-private-cloud.md#register-the-resource-provider). | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |



## <a name="on-premises-vmware-information"></a>Şirket içi VMware bilgileri

| Gerekli bilgiler  | Yanıtınız |
| ------------| --------------|
| vSphere sürümü | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| vCenter sürümü | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 
| vCenter Yöneticisi | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| L2 uzantısı<br><br>VMware HCX ile L2 ağlarını genişlettikten sonra<br>uzatılendirileceği şirket içi ağı sağlayın. | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 
| vSwitch<br><br>Ortam genelinde kullanılan vSwitch türünü belirtin. | ☐ Standart &nbsp; &nbsp; ☐ dağıtıldı<br><br>Standart kullanıyorsanız VMware HCX kullanılamaz. |
| DNS ve DHCP<br><br>Uygun bir DNS ve DHCP altyapısı gereklidir. <br>Yerleşik olarak bulunan DHCP hizmetini kullanmanız önerilir <br>NSX veya özel bulutta yerel bir DHCP sunucusu kullanın <br>yayın DHCP trafiğini yönlendirme yerine <br>WAN 'dan şirket içine geri dönün. Daha fazla bilgi için, <br>bkz. [Azure VMware çözümünde DHCP oluşturma ve yönetme](manage-dhcp.md). | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |


 

## <a name="networking---azure-vmware-solution-infrastructure"></a>Ağ oluşturma-Azure VMware Çözüm altyapısı 
Gerekli veriler, Azure VMware Çözüm ağı 'nın, tek başına ve ilk ağ testi için ihtiyaçlarını karşılamanıza yardımcı olur. 

| Gerekli bilgiler | Yanıtınız |
| ----------- | ------------- |
|Azure VMware Çözüm CıDR<br><br>VSphere Konakları, vSAN ve yönetim için gereklidir <br>Azure VMware çözümünde ağlar. Daha fazla <br>bilgi, bkz. [Yönlendirme ve alt ağ değerlendirmeleri](tutorial-network-checklist.md#routing-and-subnet-considerations). | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Azure VMware Çözüm iş yükü CıDR (isteğe bağlı)<br><br>Azure VMware 'de kullanılacak bir ağ atama<br>İlk test için çözüm. Sanal makineler<br>, ağ bağlantısını doğrulamak için dağıtılacak <br>Azure VMware çözümü. | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |



## <a name="networking---connect-azure-vmware-solution-to-azure-virtual-network"></a>Ağ-Azure VMware çözümünü Azure sanal ağına bağlama
Azure VMware Çözüm kümesi Stood sonrasında için gereken veriler, Azure VMware çözüm hizmeti 'nin bir parçası olan bir ExpressRoute bağlantı hattı aracılığıyla Azure 'a bağlanabilir.

| Gerekli bilgiler | Yanıtınız |
| ------------------ | ------------- |
| Sıçrama kutusu-Windows Server | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Sanal ağ oluşturma | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Ağ geçidi alt ağı oluştur | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Sanal ağ geçidi oluştur<br><br>Daha fazla bilgi için bkz. [sanal ağ geçidini oluşturma](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway). | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |


 

## <a name="networking---connect-azure-vmware-solution-to-an-on-premises-datacenter"></a>Ağ-Azure VMware çözümünü şirket içi veri merkezine bağlama

| Gerekli bilgiler | Yanıtınız |
| ------------------ | ------------- |
| ExpressRoute eşleme CıDR<br><br>`/29`CIDR adres bloğu. | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| ExpressRoute yetkilendirme anahtarı ve kaynak KIMLIĞI<br><br>Yetkilendirme anahtarı ve kaynak KIMLIĞI sağlayın, <br>geçerli ExpressRoute 'dan oluşturulan <br>Şirket içi veri merkezine bağlanan devre.  | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Varsayılan yolun yönü<br><br>Azure VMware çözümünde sanal makineler olmalıdır <br>Azure VMware çözümü ile internet 'e erişin <br>Internet veya ExpressRoute bağlantı hattı üzerinden geri dönüp <br>Varsayılan rota için şirket içi misiniz? | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| Hizmetle iletişim kurmak için gereken ağ bağlantı noktaları<br><br>Daha fazla bilgi için bkz. [gerekli ağ bağlantı noktaları](tutorial-network-checklist.md#required-network-ports). | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |



## <a name="networking---vmware-hcx"></a>Ağ oluşturma-VMware HCX

| Gerekli bilgiler | Yanıtınız |
| ------------------ | ------------- |
| Ağ bağlantı noktaları<br><br>Bir güvenlik duvarı varsa, gerekli ağ bağlantı noktalarından emin olun <br>, şirket içi ve Azure VMware çözümü arasında açılır. | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| DNS<br><br>DNS 'yi yapılandırma hakkında daha fazla bilgi için <br>bkz. [ağ-Azure VMware Çözüm altyapısı](#networking---azure-vmware-solution-infrastructure). | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| VMware HCX Cıdrs<br><br>`/29`İçin kullanılan iki CIDR bloğu gerekir <br>Şirket içi VMware HCX altyapısı bileşenleri.  | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |

