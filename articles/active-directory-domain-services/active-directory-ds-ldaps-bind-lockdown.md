---
title: Güvenli LDAP (LDAPS) kullanarak Azure AD Domain Services yönetilen bir etki alanına bağlama | Microsoft Docs
description: Güvenli LDAP (LDAPS) kullanarak Azure AD Domain Services yönetilen bir etki alanına bağlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6871374a-0300-4275-9a45-a39a52c65ae4
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: cbc5bee0f4cc59f59af6e3f57219279cd8fcb030
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988574"
---
# <a name="bind-to-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Güvenli LDAP (LDAPS) kullanarak Azure AD Domain Services yönetilen bir etki alanına bağlama

## <a name="before-you-begin"></a>Başlamadan önce
Tam [görev 4-DNS 'i yönetilen etki alanına internet 'ten erişecek şekilde yapılandırın](active-directory-ds-ldaps-configure-dns.md).


## <a name="task-5-bind-to-the-managed-domain-over-ldap-using-ldpexe"></a>5\. Görev: LDP. exe kullanarak LDAP üzerinden yönetilen etki alanına bağlama
LDAP üzerinden bağlanmak ve arama yapmak için uzak sunucu yönetim araçları paketine dahil edilen LDP. exe aracını kullanabilirsiniz.

İlk olarak, LDP 'yi açın ve yönetilen etki alanına bağlanın. **Bağlantı** ' ya tıklayın ve menüden **Bağlan...** öğesine tıklayın. Yönetilen etki alanının DNS etki alanı adını belirtin. Bağlantılar için kullanılacak bağlantı noktasını belirtin. LDAP bağlantıları için 389 numaralı bağlantı noktasını kullanın. LDAPS bağlantıları için 636 numaralı bağlantı noktasını kullanın. Yönetilen etki alanına bağlanmak için **Tamam** düğmesine tıklayın.

Ardından, yönetilen etki alanına bağlayın. **Bağlantı** ' ya tıklayın ve menüde **bağla...** ' ya tıklayın. ' AAD DC Administrators ' grubuna ait bir kullanıcı hesabının kimlik bilgilerini sağlayın.

> [!IMPORTANT]
> Azure AD Domain Services örneğiniz üzerinde NTLM parola karması eşitlemesini devre dışı bırakmış kullanıcılar (ve hizmet hesapları) LDAP basit bağlamalar gerçekleştiremez.  NTLM parola karma eşitlemesini devre dışı bırakma hakkında daha fazla bilgi için [Azure AD Domain Services yönetilen etki alanınızı güvenli bir şekilde](secure-your-domain.md)okuyun.
>
>

**Görünüm**' ü seçin ve ardından menüdeki **ağaç** ' ı seçin. Temel DN alanını boş bırakın ve Tamam ' a tıklayın. Aramak istediğiniz kapsayıcıya gidin, kapsayıcıya sağ tıklayın ve ara ' yı seçin.

> [!TIP]
> - Azure AD 'den eşitlenen kullanıcılar ve gruplar **Aaddc kullanıcıları** kuruluş biriminde depolanır. Bu kuruluş birimi için arama yolu gibi ```OU=AADDC Users,DC=CONTOSO100,DC=COM```görünür.
> - Yönetilen etki alanına katılmış bilgisayarlar için bilgisayar hesapları, **Aaddc bilgisayarları** kurumsal biriminde depolanır. Bu kuruluş birimi için arama yolu gibi ```OU=AADDC Computers,DC=CONTOSO100,DC=COM```görünür.
>
>

Daha fazla bilgi- [LDAP sorgusu temelleri](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)


## <a name="task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>Görev 6: İnternet üzerinden yönetilen etki alanınız için Güvenli LDAP erişimini kilitleme
> [!NOTE]
> Yönetilen etki alanına Internet üzerinden LDAPS erişimini etkinleştirmediyseniz, bu yapılandırma görevini atlayın.
>
>

Bu göreve başlamadan önce, [görev 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)' te özetlenen adımları uygulayın.

Yönetilen etki alanına Internet üzerinden LDAPS erişimini etkinleştirdiğinizde, bir güvenlik tehdidi oluşturur. Yönetilen etki alanına, Güvenli LDAP (bağlantı noktası 636) için kullanılan bağlantı noktasında internet 'ten erişilebilir. Yönetilen etki alanına erişimi, bilinen belirli IP adresleriyle kısıtlamayı tercih edebilirsiniz. Bir ağ güvenlik grubu (NSG) oluşturun ve Azure AD Domain Services etkinleştirdiğiniz alt ağ ile ilişkilendirin.

Aşağıdaki tablodaki örnek NSG, internet üzerinden güvenli LDAP erişimini kilitler. NSG 'deki kurallar, TCP bağlantı noktası 636 üzerinden yalnızca belirtilen bir IP adresi kümesinden gelen güvenli LDAP erişimine izin verir. Varsayılan ' DenyAll ' kuralı internet 'ten gelen diğer tüm trafiğe uygulanır. Belirtilen IP adreslerinden Internet üzerinden LDAPS erişimine izin veren NSG kuralı, DenyAll NSG kuralından daha yüksek önceliğe sahiptir.

![Internet üzerinden LDAPS erişiminin güvenliğini sağlamak için örnek NSG](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Daha fazla bilgi** - [ağ güvenlik grupları](../virtual-network/security-overview.md).


## <a name="related-content"></a>İlgili içerik
* [Azure AD Domain Services-Başlarken Kılavuzu](create-instance.md)
* [Azure AD Domain Services etki alanını yönetme](manage-domain.md)
* [LDAP sorgusu temelleri](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)
* [Azure AD Domain Services için grup ilkesi yönetme](manage-group-policy.md)
* [Ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)


## <a name="next-step"></a>Sonraki adım
[Yönetilen bir etki alanında güvenli LDAP sorunlarını giderme](tshoot-ldaps.md)
