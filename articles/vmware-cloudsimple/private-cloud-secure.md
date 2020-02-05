---
title: Azure VMware çözümleri (AVS)-güvenli AVS özel bulutu
description: Azure VMware Solutions (AVS) özel bulutunun nasıl güvence altına alınacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b053f1493c2380153711176a4748ebf90b479a6c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020089"
---
# <a name="how-to-secure-your-avs-private-cloud-environment"></a>AVS özel bulut ortamınızın güvenliğini sağlama

AVS hizmeti, AVS portalı ve AVS özel bulutu için Azure 'dan rol tabanlı erişim denetimi (RBAC) tanımlayın. Kullanıcı, Grup ve AVS 'ye erişim için roller VMware SSO kullanılarak belirtilir. 

## <a name="rbac-for-avs-service"></a>AVS için RBAC hizmeti

AVS hizmetini oluşturma, Azure aboneliğinde **sahip** veya **katkıda bulunan** rolü gerektirir. Varsayılan olarak, tüm sahipler ve katkıda bulunanlar bir AVS hizmeti oluşturabilir ve AVS özel bulutları oluşturmak ve yönetmek için AVS portalına erişebilir. Her bölge için yalnızca bir AVS hizmeti oluşturulabilir. Belirli yöneticilere erişimi kısıtlamak için aşağıdaki yordamı izleyin.

1. Azure portal yeni bir **kaynak grubunda** bir AVS hizmeti oluşturun
2. Kaynak grubu için RBAC belirtin.
3. AVS hizmeti ile aynı kaynak grubunu satın alın ve düğümleri kullanın

Yalnızca kaynak grubunda **sahip** veya **katkıda bulunan** ayrıcalıklarına sahip kullanıcılar AVS hizmetini görebilir ve AVS portalını başlatacaktır.

RBAC hakkında daha fazla bilgi için bkz. [Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)

## <a name="rbac-for-avs-private-cloud-vcenter"></a>AVS için RBAC özel bulut vCenter

Bir AVS özel bulutu oluşturulduğunda vCenter SSO etki alanında varsayılan kullanıcı `CloudOwner@AVS.local` oluşturulur. CloudOwner kullanıcısının vCenter yönetimi ayrıcalıkları vardır. Farklı kullanıcılara erişim vermek için vCenter SSO 'ya ek kimlik kaynakları eklenir. Önceden tanımlanmış roller ve gruplar, ek kullanıcı eklemek için kullanılabilecek vCenter üzerinde ayarlanır.

### <a name="add-new-users-to-vcenter"></a>VCenter 'a Yeni Kullanıcı ekleme

1. AVS özel bulutu üzerinde **CloudOwner@AVS.local** Kullanıcı Için [ayrıcalıkları ilerletin](escalate-private-cloud-privileges.md) .
2. **CloudOwner@AVS.local** kullanarak vCenter 'da oturum açın
3. [VCenter çoklu oturum açma kullanıcıları ekleyin](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-72BFF98C-C530-4C50-BF31-B5779D2A4BBB.html).
4. [VCenter çoklu oturum açma gruplarına](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html)Kullanıcı ekleyin.

Önceden tanımlanmış roller ve gruplar hakkında daha fazla bilgi için bkz. [AVS özel bulut izin modeli VMware vCenter](learn-private-cloud-permissions.md) article.

### <a name="add-new-identity-sources"></a>Yeni kimlik kaynakları ekle

AVS özel bulutunuzun vCenter SSO etki alanı için ek kimlik sağlayıcıları ekleyebilirsiniz. Kimlik sağlayıcıları kimlik doğrulama ve vCenter SSO grupları, kullanıcılar için yetkilendirme sağlar.

* AVS özel bulutu vCenter üzerinde [kimlik sağlayıcısı olarak Active Directory kullanın](set-vcenter-identity.md) .
* AVS özel bulutu vCenter üzerinde [kimlik sağlayıcısı olarak Azure ad kullanma](azure-ad.md)

1. AVS özel bulutu üzerinde **CloudOwner@AVS.local** Kullanıcı Için [ayrıcalıkları ilerletin](escalate-private-cloud-privileges.md) .
2. **CloudOwner@AVS.local** kullanarak vCenter 'da oturum açın
3. Kimlik sağlayıcısından [vCenter çoklu oturum açma gruplarına](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html)Kullanıcı ekleyin.

## <a name="secure-network-on-your-avs-private-cloud-environment"></a>AVS özel bulut ortamınızda güvenli ağ

AVS özel bulut ortamının ağ güvenliği, ağ erişiminin güvenliğini sağlayarak ve kaynaklar arasındaki ağ trafiğini denetleyerek denetlenir.

### <a name="access-to-avs-private-cloud-resources"></a>AVS özel bulut kaynaklarına erişim

AVS özel bulut vCenter ve kaynakları erişimi, güvenli bir ağ bağlantısı üzerinden yapılır:

* **[ExpressRoute bağlantısı](on-premises-connection.md)** . ExpressRoute, şirket içi ortamınızdan güvenli, yüksek bant genişliğine sahip, düşük gecikme süreli bir bağlantı sağlar. Bağlantının kullanılması, şirket içi hizmetlerinizin, ağlarınızın ve kullanıcılarınızın AVS özel bulut vCenter 'a erişmesine izin verir.
* **[Siteden sıteye VPN ağ geçidi](vpn-gateway.md)** . Siteden siteye VPN, güvenli bir tünelden Şirket içinden AVS özel bulut kaynaklarınıza erişim sağlar. Hangi şirket içi ağların, AVS özel bulutuna ağ trafiği gönderebileceğini ve alabileceğini belirtirsiniz.
* **[Noktadan sıteye VPN ağ geçidi](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)** . AVS özel bulut vCenter 'unuzda hızlı uzaktan erişim için Noktadan siteye VPN bağlantısı kullanın.

### <a name="control-network-traffic-in-avs-private-cloud"></a>AVS özel bulutundaki ağ trafiğini denetleme

Güvenlik Duvarı tabloları ve kuralları, AVS özel bulutundaki ağ trafiğini denetler. Güvenlik Duvarı tablosu, tabloda tanımlanan kuralların birleşimine bağlı olarak bir kaynak ağ veya IP adresi ile hedef ağ veya IP adresi arasındaki ağ trafiğini denetlemenize olanak tanır.

1. Bir [güvenlik duvarı tablosu](firewall.md#add-a-new-firewall-table)oluşturun.
2. Güvenlik Duvarı tablosuna [kurallar ekleyin](firewall.md#create-a-firewall-rule) .
3. [Bir VLAN/subnet 'e güvenlik duvarı tablosu iliştirme] (Firewall. MD # Attach-vlanssubnet.
