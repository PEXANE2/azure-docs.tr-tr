---
title: Azure VMware çözümü-özel buluttan şirket içine DNS iletimi
description: CloudSimple özel bulut DNS sunucunuzun şirket içi kaynakların aramasını ileri bir şekilde nasıl etkinleştireceğinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 02/29/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: aa2af4302613aad23bfd78b4883bbb46c5e5ddbb
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961133"
---
# <a name="enable-cloudsimple-private-cloud-dns-servers-to-forward-dns-lookup-of-on-premises-resources-to-your-dns-servers"></a>Şirket içi kaynakların DNS aramasını DNS sunucularınıza iletmek için CloudSimple özel bulut DNS sunucularını etkinleştirin

Özel bulut DNS sunucuları, herhangi bir şirket içi kaynak için DNS aramasını DNS sunucularınıza iletebilir.  Aramanın etkinleştirilmesi, özel bulut vSphere bileşenlerinin şirket içi ortamınızda çalışan hizmetleri aramasına ve tam etki alanı adları (FQDN) kullanarak onlarla iletişim kurmasına izin verir.

## <a name="scenarios"></a>Senaryolar 

Şirket içi DNS sunucunuz için DNS aramasını iletme, aşağıdaki senaryolar için özel bulutunuzu kullanmanıza olanak sağlar:

* Şirket içi VMware çözümünüz için özel bulutu olağanüstü durum kurtarma kurulumu olarak kullanın
* Özel bulut vSphere 'niz için kimlik kaynağı olarak şirket içi Active Directory kullanma
* Şirket içinden özel buluta sanal makineler geçirmek için HCX kullanın

## <a name="before-you-begin"></a>Başlamadan önce

DNS iletmesi 'nın çalışması için özel bulut ağınızdan şirket içi ağınıza yönelik bir ağ bağlantısı bulunmalıdır.  Kullanarak ağ bağlantısı kurabilirsiniz:

* [ExpressRoute kullanarak şirket içinden CloudSimple 'a bağlanma](on-premises-connection.md)
* [Siteden siteye VPN ağ geçidi ayarlama](https://docs.microsoft.com/azure/vmware-cloudsimple/vpn-gateway#set-up-a-site-to-site-vpn-gateway)

DNS iletmesi 'nın çalışması için bu bağlantıda güvenlik duvarı bağlantı noktalarının açılması gerekir.  Kullanılan bağlantı noktaları TCP bağlantı noktası 53 veya UDP bağlantı noktası 53 ' dir.

> [!NOTE]
> Siteden siteye VPN kullanıyorsanız, şirket içi DNS sunucusu alt ağınızın şirket içi öneklerin bir parçası olarak eklenmesi gerekir.

## <a name="request-dns-forwarding-from-private-cloud-to-on-premises"></a>Özel buluttan şirket içine DNS iletimi isteği

Özel buluttan şirket içine DNS iletmeyi etkinleştirmek için, aşağıdaki bilgileri sağlayarak bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderebilirsiniz.

* Sorun türü: **Teknik**
* Abonelik: **CloudSimple hizmetinin dağıtıldığı abonelik**
* Hizmet: **CloudSimple tarafından VMware çözümü**
* Sorun türü: **Danışma belgesi veya nasıl yaparım?...**
* Sorun alt türü: **NW ile ilgili yardım gerekiyor**
* Ayrıntılar bölmesinde şirket içi etki alanının etki alanı adını sağlayın.
* Ayrıntılar bölmesinde, aramanın özel buluttan iletileceği şirket içi DNS sunucularınızın listesini belirtin.

## <a name="next-steps"></a>Sonraki adımlar

* [Şirket içi güvenlik duvarı yapılandırması hakkında daha fazla bilgi edinin](on-premises-firewall-configuration.md)
* [Şirket içi DNS sunucusu yapılandırması](on-premises-dns-setup.md)
