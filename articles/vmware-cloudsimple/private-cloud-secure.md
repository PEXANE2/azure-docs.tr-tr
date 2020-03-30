---
title: CloudSimple'a göre Azure VMware Çözümleri - Güvenli Özel Bulut
description: CloudSimple Private Cloud tarafından Azure VMware Çözümlerinin nasıl güvence altına alabildiğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4541874a9e8fc4111e5c65d02f07535c4d14f9f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77565987"
---
# <a name="how-to-secure-your-private-cloud-environment"></a>Özel Bulut ortamınızı nasıl güvene alalarınız?

Azure'dan CloudSimple Hizmeti, CloudSimple portalı ve Private Cloud için rol tabanlı erişim denetimi (RBAC) tanımlayın.  VCenter of Private Cloud'a erişmek için kullanıcılar, gruplar ve roller VMware SSO kullanılarak belirtilir.  

## <a name="rbac-for-cloudsimple-service"></a>CloudSimple hizmeti için RBAC

CloudSimple hizmetinin oluşturulması için Azure aboneliğinde **Sahip** veya **Katılımcı** rolü gerekir.  Varsayılan olarak, tüm sahipler ve katkıda bulunanlar bir CloudSimple hizmeti oluşturabilir ve Özel Bulutlar oluşturmak ve yönetmek için CloudSimple portalına erişebilir.  Bölge başına yalnızca bir CloudSimple hizmeti oluşturulabilir.  Belirli yöneticilere erişimi kısıtlamak için aşağıdaki yordamı izleyin.

1. Azure portalında yeni bir **kaynak grubunda** CloudSimple Hizmeti oluşturma
2. Kaynak grubu için RBAC belirtin.
3. Düğümleri satın alın ve CloudSimple hizmetiyle aynı kaynak grubunu kullanın

Yalnızca kaynak grubunda **Sahip** veya **Katılımcı** ayrıcalıklarına sahip kullanıcılar CloudSimple hizmetini görür ve CloudSimple portalını başlatacaktır.

RBAC hakkında daha fazla bilgi için Azure [kaynakları için rol tabanlı erişim denetimi (RBAC) nedir'e](../role-based-access-control/overview.md)bakın.

## <a name="rbac-for-private-cloud-vcenter"></a>Özel Bulut vCenter için RBAC

Özel Bulut `CloudOwner@cloudsimple.local` oluşturulduğunda vCenter SSO etki alanında varsayılan bir kullanıcı oluşturulur.  CloudOwner kullanıcı vCenter yönetmek için ayrıcalıkları vardır. Farklı kullanıcılara erişim sağlamak için vCenter SSO'ya ek kimlik kaynakları eklenir.  VCenter'da ek kullanıcılar eklemek için kullanılabilecek önceden tanımlanmış roller ve gruplar ayarlanır.

### <a name="add-new-users-to-vcenter"></a>vCenter'a yeni kullanıcılar ekleme

1. **CloudOwner\@cloudsimple.local** kullanıcı için [Özel Bulut'taki ayrıcalıkları artırın.](escalate-private-cloud-privileges.md)
2. **CloudOwner\@cloudsimple.local** kullanarak vCenter'da oturum açın
3. [vCenter Tek Oturum Açma Kullanıcıları ekleyin.](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-72BFF98C-C530-4C50-BF31-B5779D2A4BBB.html)
4. kullanıcıları [vCenter tek oturum açma gruplarına](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html)ekleyin.

Önceden tanımlanmış roller ve gruplar hakkında daha fazla bilgi için [VMware vCenter makalesinin CloudSimple Private Cloud izin modeline](learn-private-cloud-permissions.md) bakın.

### <a name="add-new-identity-sources"></a>Yeni kimlik kaynakları ekleme

Özel Bulut'unuzun vCenter SSO etki alanı için ek kimlik sağlayıcıları ekleyebilirsiniz.  Kimlik sağlayıcıları kimlik doğrulaması sağlar ve vCenter SSO grupları kullanıcılar için yetkilendirme sağlar.

* [Active Directory'yi](set-vcenter-identity.md) Private Cloud vCenter'da kimlik sağlayıcısı olarak kullanın.
* [Azure AD'yi](azure-ad.md) Private Cloud vCenter'da kimlik sağlayıcısı olarak kullanma

1. **CloudOwner\@cloudsimple.local** kullanıcı için [Özel Bulut'taki ayrıcalıkları artırın.](escalate-private-cloud-privileges.md)
2. **CloudOwner\@cloudsimple.local** kullanarak vCenter'da oturum açın
3. Kimlik sağlayıcısından kullanıcıları [vCenter tek oturum açma gruplarına](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html)ekleyin.

## <a name="secure-network-on-your-private-cloud-environment"></a>Özel Bulut ortamınızda güvenli ağ

Özel Bulut ortamının ağ güvenliği, ağ erişimini güvence altına alarak ve kaynaklar arasındaki ağ trafiğini denetleyerek denetlenir.

### <a name="access-to-private-cloud-resources"></a>Özel Bulut kaynaklarına erişim

Özel Bulut vCenter ve kaynak erişimi güvenli bir ağ bağlantısı üzerinden:

* **[ExpressRoute bağlantısı](on-premises-connection.md)**. ExpressRoute, şirket içi ortamınızdan güvenli, yüksek bant genişliğine ve düşük gecikmeli bağlantı sağlar.  Bağlantıyı kullanmak, şirket içi hizmetlerinizin, ağlarınızın ve kullanıcılarınızın Özel Bulut vCenter'ınıza erişmesine olanak tanır.
* **[Siteden Siteye VPN ağ geçidi.](vpn-gateway.md)** Siteden Siteye VPN, Özel Bulut kaynaklarınıza şirket içinde güvenli bir tünel den erişim sağlar.  Özel Bulut'unuza hangi şirket içi ağların ağ trafiğini gönderip alabileceğini belirtirsiniz.
* **[Noktadan Siteye VPN ağ geçidi.](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)** Özel Bulut vCenter'ınıza hızlı uzaktan erişim için Site'ye Nokta VPN bağlantısını kullanın.

### <a name="control-network-traffic-in-private-cloud"></a>Özel Bulut'ta ağ trafiğini kontrol edin

Güvenlik duvarı tabloları ve kuralları Özel Bulut'taki ağ trafiğini denetler.  Güvenlik duvarı tablosu, tabloda tanımlanan kuralların birleşimine bağlı olarak bir kaynak ağ veya IP adresi ile hedef ağ veya IP adresi arasındaki ağ trafiğini denetlemenize olanak tanır.

1. Güvenlik [duvarı tablosu](firewall.md#add-a-new-firewall-table)oluşturun.
2. Güvenlik duvarı tablosuna [kurallar ekleyin.](firewall.md#create-a-firewall-rule)
3. [Bir VLAN/alt ağına güvenlik duvarı tablosu takın.](firewall.md#attach-vlans-subnet)
