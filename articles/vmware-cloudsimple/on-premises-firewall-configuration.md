---
title: Azure VMware Çözümüne CloudSimple ile şirket içinden erişin
titleSuffix: Azure VMware Solution by CloudSimple
description: Bir güvenlik duvarı aracılığıyla şirket içi ağınız üzerinden CloudSimple ile Azure VMware Çözümüne erişme
author: sharaths-cs
ms.author: dikamath
ms.date: 08/08/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 539665c4756a7dc87078922421b45a88404f58f1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81868137"
---
# <a name="accessing-your-cloudsimple-private-cloud-environment-and-applications-from-on-premises"></a>CloudSimple Private Cloud ortamınıza ve uygulamalarınıza şirket içinden erişme

Azure ExpressRoute veya Siteden Siteye VPN kullanarak şirket içi ağdan CloudSimple'a bağlantı kurulabilir.  CloudSimple Private Cloud vCenter'ınıza ve bağlantıyı kullanarak Özel Bulut'ta çalıştırdığınız tüm iş yüklerine erişin.  Şirket içi ağınızdaki güvenlik duvarını kullanarak bağlantıda hangi bağlantı noktalarının açıldığını denetleyebilirsiniz.  Bu makalede, bazı tipik uygulamalar bağlantı noktası gereksinimleri açıklamaktadır.  Diğer uygulamalar için bağlantı noktası gereksinimleri için başvuru belgelerine bakın.

## <a name="ports-required-for-accessing-vcenter"></a>vCenter'a erişmek için gereken bağlantı noktaları

Private Cloud vCenter ve NSX-T yöneticinize erişmek için, aşağıdaki tabloda tanımlanan bağlantı noktalarının şirket içi güvenlik duvarında açılması gerekir.  

| Bağlantı noktası       | Kaynak                           | Hedef                      | Amaç                                                                                                                |
|------------|----------------------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)   | Şirket içi DNS sunucuları          | Özel Bulut DNS sunucuları        | *az.cloudsimple.io DNS* aramasını şirket içi ağdan Özel Bulut DNS sunucularına iletmek için gereklidir.       |
| 53 (UDP)   | Özel Bulut DNS sunucuları        | Şirket içi DNS sunucuları          | DNS'nin iletmesi için özel bulut vCenter'dan şirket içi DNS sunucularına şirket içi alan adlarını araması gerekir. |
| 80 (TCP)   | Şirket içi ağı              | Özel Bulut yönetim ağı | vCenter URL'sini *https'ye* *https*yönlendirmek için gereklidir.                                                           |
| 443 (TCP)  | Şirket içi ağı              | Özel Bulut yönetim ağı | Şirket içi ağdan vCenter ve NSX-T yöneticisine erişmek için gereklidir.                                             |
| 8000 (TCP) | Şirket içi ağı              | Özel Bulut yönetim ağı | Sanal makinelerin şirket içi ve Özel Bulut'a vMotion'ı için gereklidir.                                            |
| 8000 (TCP) | Özel Bulut yönetim ağı | Şirket içi ağı              | Özel Bulut'tan şirket içi sanal makinelerin vMotion'ı için gereklidir.                                            |

## <a name="ports-required-for-using-on-premises-active-directory-as-an-identity-source"></a>Şirket içi etkin dizinin kimlik kaynağı olarak kullanılması için gereken bağlantı noktaları

Özel Bulut vCenter'da şirket içi etkin dizini kimlik kaynağı olarak yapılandırmak için tabloda tanımlanan bağlantı noktalarının açılması gerekir.  Bkz. Yapılandırma adımları [için CloudSimple Private Cloud'daki vCenter için bir kimlik sağlayıcısı olarak Azure AD'yi kullanın.](https://docs.microsoft.com/azure/vmware-cloudsimple/azure-ad/)

| Bağlantı noktası         | Kaynak                           | Hedef                                         | Amaç                                                                                                                                          |
|--------------|----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)      | Özel Bulut DNS sunucuları        | Şirket içi DNS sunucuları                             | DNS'nin iletmesi için özel bulut vCenter'dan şirket içi DNS sunucularına şirket içi etkin dizin alan adlarını arayın.          |
| 389 (TCP/UDP) | Özel Bulut yönetim ağı | Şirket içi etkin dizin etki alanı denetleyicileri     | Özel Bulut vCenter sunucusundan kullanıcı kimlik doğrulaması için etkin dizin etki alanı denetleyicilerine LDAP iletişimi için gereklidir.                |
| 636 (TCP)     | Özel Bulut yönetim ağı | Şirket içi etkin dizin etki alanı denetleyicileri     | Özel Bulut vCenter sunucusundan kullanıcı kimlik doğrulaması için etkin dizin etki alanı denetleyicilerine güvenli LDAP (LDAPS) iletişimi için gereklidir. |
| 3268 (TCP)    | Özel Bulut yönetim ağı | Şirket içi etkin dizin global katalog sunucuları | Çok etki alanı denetleyici dağıtımlarında LDAP iletişimi için gereklidir.                                                                        |
| 3269 (TCP)    | Özel Bulut yönetim ağı | Şirket içi etkin dizin global katalog sunucuları | Çok etki alanı denetleyici dağıtımlarında LDAPS iletişimi için gereklidir.                                                                       |                                           |

## <a name="common-ports-required-for-accessing-workload-virtual-machines"></a>İş yükü sanal makinelere erişmek için gereken ortak bağlantı noktaları

Özel Bulut'ta çalışan iş yükü sanal makinelere erişmek için şirket içi güvenlik duvarınızda bağlantı noktalarının açılmasını gerektirir.  Aşağıdaki tablo, gereken ortak bağlantı noktalarından bazılarını ve amaçlarını göstermektedir.  Herhangi bir uygulama özel bağlantı noktası gereksinimleri için, başvuru belgelerine bakın.

| Bağlantı noktası         | Kaynak                         | Hedef                          | Amaç                                                                              |
|--------------|--------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
| 22 (TCP)      | Şirket içi ağı            | Özel Bulut iş yükü ağı       | Private Cloud üzerinde çalışan Linux sanal makinelerine güvenli kabuk erişimi.              |
| 3389 (TCP)    | Şirket içi ağı            | Özel Bulut iş yükü ağı       | Özel Bulut'ta çalışan sanal makinelere uzak masaüstü.                 |
| 80 (TCP)      | Şirket içi ağı            | Özel Bulut iş yükü ağı       | Özel Bulut'ta çalışan sanal makinelerde dağıtılan tüm web sunucularına erişin.        |
| 443 (TCP)     | Şirket içi ağı            | Özel Bulut iş yükü ağı       | Private Cloud'da çalışan sanal makinelerde dağıtılan tüm güvenli web sunucularına erişin. |
| 389 (TCP/UDP) | Özel Bulut iş yükü ağı | Şirket içi etkin dizin ağı | Windows iş yükü sanal makineleri şirket içi etkin dizin etki alanına katılın.       |
| 53 (UDP)      | Özel Bulut iş yükü ağı | Şirket içi ağı                  | İş yükü sanal makineleri için Şirket içi DNS sunucularına DNS hizmet erişimi.         |

## <a name="next-steps"></a>Sonraki adımlar

* [V'ler ve Alt Ağlar oluşturma ve yönetme](https://docs.microsoft.com/azure/vmware-cloudsimple/create-vlan-subnet/)
* [Azure ExpressRoute'u kullanarak şirket içi ağa bağlanma](https://docs.microsoft.com/azure/vmware-cloudsimple/on-premises-connection/)
* [Şirket içinde Siteden Siteye VPN Kurulumu](https://docs.microsoft.com/azure/vmware-cloudsimple/vpn-gateway/)
