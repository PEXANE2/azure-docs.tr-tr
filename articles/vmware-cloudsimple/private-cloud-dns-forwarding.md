---
title: Azure VMware Çözümü - DNS özel buluttan şirket içi yönlendirme
description: CloudSimple Private Cloud DNS sunucunuzun şirket içi kaynakları nasıl ileriye taşıyabildiğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 02/29/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: aa2af4302613aad23bfd78b4883bbb46c5e5ddbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961133"
---
# <a name="enable-cloudsimple-private-cloud-dns-servers-to-forward-dns-lookup-of-on-premises-resources-to-your-dns-servers"></a>CloudSimple Private Cloud DNS sunucularının şirket içi kaynakları DNS sunucularınıza iletmesini sağlayın

Özel Cloud DNS sunucuları, dns kaynaklarını şirket içi olarak aramayı DNS sunucularınıza iletebilir.  Aramayı etkinleştirmek, Özel Bulut vSphere bileşenlerinin şirket içi ortamınızda çalışan tüm hizmetleri aramasına ve tam nitelikli alan adları (FQDN) kullanarak onlarla iletişim kurmasına olanak tanır.

## <a name="scenarios"></a>Senaryolar 

Şirket içi DNS sunucunuz için DNS araması yapmak, Özel Bulut'unuzu aşağıdaki senaryolar için kullanmanıza olanak tanır:

* Şirket içi VMware çözümünüz için Özel Bulut'u olağanüstü durum kurtarma kurulumu olarak kullanın
* Özel Bulut vSphere'iniz için şirket içi Active Directory'yi kimlik kaynağı olarak kullanma
* Sanal makineleri şirket içinden Özel Bulut'a geçirmek için HCX'i kullanın

## <a name="before-you-begin"></a>Başlamadan önce

DNS'nin işe sevk ilerlediği için Özel Bulut ağınızdan şirket içi ağınıza bir ağ bağlantısı bulunmalıdır.  Ağ bağlantısını şu şekilde ayarlayabilirsiniz:

* [ExpressRoute'u kullanarak şirket içinde CloudSimple'a bağlanın](on-premises-connection.md)
* [Siteden Siteye VPN ağ geçidi ayarlama](https://docs.microsoft.com/azure/vmware-cloudsimple/vpn-gateway#set-up-a-site-to-site-vpn-gateway)

DNS yönlendirmesi için bu bağlantıda güvenlik duvarı bağlantı noktalarının açılması gerekir.  Kullanılan bağlantı noktaları TCP portu 53 veya UDP portu 53'tür.

> [!NOTE]
> Site-to-Site VPN kullanıyorsanız, şirket içi DNS sunucu alt ağınızın şirket içi öneklerinin bir parçası olarak eklenmesi gerekir.

## <a name="request-dns-forwarding-from-private-cloud-to-on-premises"></a>Özel Bulut'tan şirket içi dns yönlendirmeisteğin

Özel Bulut'tan şirket içi DNS yönlendirmesini etkinleştirmek için, aşağıdaki bilgileri sağlayan bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderin.

* Sorun türü: **Teknik**
* Abonelik: **CloudSimple hizmetinin dağıtıldığı abonelik**
* Hizmet: **CloudSimple tarafından VMware Çözümü**
* Sorun türü: **Danışma veya nasıl yapabilirim ...**
* Sorun alt türü: **NW ile yardıma ihtiyacınız var**
* Ayrıntılar bölmesinde şirket içi etki alanınızın etki alanı adını sağlayın.
* Aramanın ayrıntılar bölmesinde özel bulutunuzdan iletilecek şirket içi DNS sunucularınızın listesini sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Şirket içi güvenlik duvarı yapılandırması hakkında daha fazla bilgi edinin](on-premises-firewall-configuration.md)
* [Şirket içi DNS sunucu yapılandırması](on-premises-dns-setup.md)
