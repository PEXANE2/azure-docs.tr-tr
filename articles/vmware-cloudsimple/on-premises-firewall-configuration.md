---
title: Şirket içinden Azure VMware çözümlerine (AVS) erişme
description: Azure VMware çözümlerinizi (AVS) Şirket içi ağınızdan bir güvenlik duvarı aracılığıyla erişme
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/08/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a4a9760b5c7a70c58a1afe1b14b781a35f2b9b18
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77082959"
---
# <a name="accessing-your-avs-private-cloud-environment-and-applications-from-on-premises"></a>AVS özel bulut ortamınıza ve uygulamalarına Şirket içinden erişme

Azure ExpressRoute veya siteden siteye VPN kullanan bir bağlantı, şirket içi ağdan AVS 'ye ayarlanabilir. AVS özel bulut vCenter ve bu bağlantıyı kullanarak AVS özel bulutu üzerinde çalıştırdığınız tüm iş yüklerinize erişin. Şirket içi ağınızda bir güvenlik duvarı kullanarak bağlantıda hangi bağlantı noktalarının açıldığını kontrol edebilirsiniz. Bu makalede tipik uygulamaların bağlantı noktası gereksinimlerinin bazıları açıklanmaktadır. Diğer uygulamalar için, bağlantı noktası gereksinimleri için uygulama belgelerine bakın.

## <a name="ports-required-for-accessing-vcenter"></a>VCenter 'a erişmek için gereken bağlantı noktaları

AVS özel bulut vCenter ve NSX-T yöneticinize erişmek için, aşağıdaki tabloda tanımlanan bağlantı noktaları şirket içi güvenlik duvarında açılmalıdır. 

| Bağlantı noktası       | Kaynak                           | Hedef                      | Amaç                                                                                                                |
|------------|----------------------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)   | Şirket içi DNS sunucuları          | AVS özel bulut DNS sunucuları        | *Az.cloudsimple.io* 'in DNS aramasını, şirket ıçı ağdan AVS özel bulut DNS sunucularına iletmek için gereklidir.     |
| 53 (UDP)   | AVS özel bulut DNS sunucuları        | Şirket içi DNS sunucuları          | AVS özel bulut vCenter 'dan şirket içi DNS sunucularına DNS 'in şirket içi etki alanı adlarının iletilmesi için gereklidir. |
| 80 (TCP)   | Şirket içi ağı              | AVS özel bulut yönetimi ağı | *Http* 'den *https*'ye vCenter URL 'sini yeniden yönlendirmek için gereklidir.                                                         |
| 443 (TCP)  | Şirket içi ağı              | AVS özel bulut yönetimi ağı | Şirket içi ağdan vCenter ve NSX-T yöneticisine erişmek için gereklidir.                                           |
| 8000 (TCP) | Şirket içi ağı              | AVS özel bulut yönetimi ağı | Sanal makinelerin Şirket içinden AVS 'ye özel buluta vMotion için gereklidir.                                          |
| 8000 (TCP) | AVS özel bulut yönetimi ağı | Şirket içi ağı              | AVS özel buluttan şirket içine sanal makinelerin vMotion için gereklidir.                                          |

## <a name="ports-required-for-using-on-premises-active-directory-as-an-identity-source"></a>Kimlik kaynağı olarak şirket içi Active Directory 'yi kullanmak için gereken bağlantı noktaları

Şirket içi Active Directory 'yi AVS özel bulutu vCenter üzerinde bir kimlik kaynağı olarak yapılandırmak için, tabloda tanımlanan bağlantı noktaları açılmalıdır. Yapılandırma adımları için bkz. [AVS AVS özel bulutu üzerinde vCenter için kimlik sağlayıcısı olarak Azure ad kullanma](https://docs.azure.cloudsimple.com/azure-ad/) .

| Bağlantı noktası         | Kaynak                           | Hedef                                         | Amaç                                                                                                                                          |
|--------------|----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)      | AVS özel bulut DNS sunucuları        | Şirket içi DNS sunucuları                             | Şirket içi Active Directory etki alanı adlarını AVS özel bulut vCenter 'dan şirket içi DNS sunucularına iletmek için gereklidir.        |
| 389 (TCP/UDP) | AVS özel bulut yönetimi ağı | Şirket içi Active Directory etki alanı denetleyicileri     | AVS özel bulut vCenter Server 'dan Kullanıcı kimlik doğrulaması için Active Directory etki alanı denetleyicilerine LDAP iletişimi için gereklidir.              |
| 636 (TCP)     | AVS özel bulut yönetimi ağı | Şirket içi Active Directory etki alanı denetleyicileri     | AVS özel bulut vCenter Server 'dan Kullanıcı kimlik doğrulaması için Active Directory etki alanı denetleyicilerine Güvenli LDAP (LDAPS) iletişimi için gereklidir. |
| 3268 (TCP)    | AVS özel bulut yönetimi ağı | Şirket içi Active Directory genel katalog sunucuları | Birden çok etki alanı denetleyicisi dağıtımlarında LDAP iletişimi için gereklidir.                                                                      |
| 3269 (TCP)    | AVS özel bulut yönetimi ağı | Şirket içi Active Directory genel katalog sunucuları | Çoklu etki alanı denetleyicisi dağıtımlarında LDAPS iletişimi için gereklidir.                                                                     |                                           |

## <a name="common-ports-required-for-accessing-workload-virtual-machines"></a>İş yükü sanal makinelerine erişmek için gereken ortak bağlantı noktaları

AVS özel bulutu üzerinde çalışan sanal makinelerin erişim iş yükü, şirket içi güvenlik duvarınızda bağlantı noktalarının açılmasını gerektirir. Aşağıdaki tabloda, gerekli ortak bağlantı noktalarından bazıları ve bunların amaçları gösterilmektedir. Uygulamaya özgü herhangi bir bağlantı noktası gereksinimi için uygulama belgelerine bakın.

| Bağlantı noktası         | Kaynak                         | Hedef                          | Amaç                                                                              |
|--------------|--------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
| 22 (TCP)      | Şirket içi ağı            | AVS özel bulut iş yükü ağı       | AVS özel bulutu 'nda çalışan Linux sanal makinelerine güvenli kabuk erişimi.            |
| 3389 (TCP)    | Şirket içi ağı            | AVS özel bulut iş yükü ağı       | AVS özel bulutu üzerinde çalışan Windows sanal makinelerine uzak masaüstü.               |
| 80 (TCP)      | Şirket içi ağı            | AVS özel bulut iş yükü ağı       | AVS özel bulutu üzerinde çalışan sanal makinelere dağıtılan tüm Web sunucularına erişin.      |
| 443 (TCP)     | Şirket içi ağı            | AVS özel bulut iş yükü ağı       | AVS özel bulutu üzerinde çalışan sanal makinelerde dağıtılan güvenli Web sunucularına erişin. |
| 389 (TCP/UDP) | AVS özel bulut iş yükü ağı | Şirket içi Active Directory ağı | Windows iş yükü sanal makinelerini şirket içi Active Directory etki alanına ekleyin.     |
| 53 (UDP)      | AVS özel bulut iş yükü ağı | Şirket içi ağı                  | Şirket içi DNS sunucularına iş yükü sanal makineleri için DNS hizmeti erişimi.       |

## <a name="next-steps"></a>Sonraki adımlar

* [VLAN 'Ları ve alt ağları oluşturma ve yönetme](https://docs.azure.cloudsimple.com/create-vlan-subnet/)
* [Azure ExpressRoute kullanarak şirket içi ağa bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Şirket içinden siteden siteye VPN kurma](https://docs.azure.cloudsimple.com/vpn-gateway/)
