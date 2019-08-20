---
title: 'Azure Active Directory Domain Services: Güvenli LDAP sorunlarını giderme | Microsoft Docs'
description: Azure AD Domain Services için Güvenli LDAP sorun giderme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 8f9f4a8b52548dad011f5e825fa42c50da970ea7
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69613163"
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Azure AD Domain Services-Güvenli LDAP yapılandırma sorunlarını giderme

Bu makalede, Azure AD Domain Services için [GÜVENLI LDAP yapılandırılırken](tutorial-configure-ldaps.md) sık karşılaşılan sorunlara yönelik çözümler sağlanmaktadır.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Güvenli LDAP ağ güvenlik grubu yapılandırması

**Uyarı iletisi:**

*Internet üzerinden Güvenli LDAP, yönetilen etki alanı için etkinleştirilmiştir. Ancak, 636 numaralı bağlantı noktasına erişim bir ağ güvenlik grubu kullanılarak kilitlenmemiştir. Bu, yönetilen etki alanındaki Kullanıcı hesaplarını parola yanılma saldırısı saldırılarına maruz bırakabilir.*

### <a name="secure-ldap-port"></a>Güvenli LDAP bağlantı noktası

Güvenli LDAP etkinleştirildiğinde, yalnızca belirli IP adreslerinden gelen LDAPS erişimine izin vermek için ek kurallar oluşturmanız önerilir. Bu kurallar, etki alanınızı bir güvenlik tehdidi oluşturabilecek deneme yanılma saldırılarına karşı korur. Bağlantı noktası 636, yönetilen etki alanınız için erişime izin verir. Güvenli LDAP erişimine izin vermek için NSG 'nizi güncelleştirme işlemi şu şekildedir:

1. Azure portal [ağ güvenlik grupları sekmesine](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) gidin
2. Tablodan etki alanınız ile ilişkili NSG 'yi seçin.
3. **Gelen güvenlik kuralları** ' na tıklayın
4. Bağlantı noktası 636 kuralı oluşturma
   1. Üst gezinti çubuğunda **Ekle** ' ye tıklayın.
   2. Kaynak için **IP adreslerini** seçin.
   3. Bu kural için kaynak bağlantı noktası aralıklarını belirtin.
   4. Hedef bağlantı noktası aralıkları için "636" girişi.
   5. Protokol **TCP**'dir.
   6. Kurala uygun bir ad, açıklama ve öncelik verin. Bu kuralın önceliği, varsa "Tümünü Reddet" kuralının önceliği 'nden daha yüksek olmalıdır.
   7. **Tamam**'ı tıklatın.
5. Kuralınızın oluşturulduğunu doğrulayın.
6. Adımları doğru tamamladığınızdan emin olmak için etki alanının sistem durumunu iki saat içinde denetleyin.

> [!TIP]
> Bağlantı noktası 636 Azure AD Domain Services düzgün çalışması için gereken tek kural değildir. Daha fazla bilgi edinmek için [ağ kılavuzlarını](network-considerations.md) ziyaret edin veya [NSG yapılandırma makaleleriyle ilgili sorunları giderin](alert-nsg.md) .
>

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502: Güvenli LDAP sertifikanın süresi doluyor

**Uyarı iletisi:**

*Yönetilen etki alanı için Güvenli LDAP sertifikasının kullanım süreleri [Date]].*

**Çözüm:**

[GÜVENLI LDAP yapılandırma](tutorial-configure-ldaps.md) makalesinde özetlenen adımları izleyerek yeni BIR Güvenli LDAP sertifikası oluşturun.

## <a name="contact-us"></a>Bizimle iletişim kurun
[Geri bildirim paylaşmak veya destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.
