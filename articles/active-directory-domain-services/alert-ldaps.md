---
title: Azure AD Etki Alanı Hizmetleri'ndeki güvenli LDAP uyarılarını çözümle | Microsoft Dokümanlar
description: Azure Active Directory Domain Services için güvenli LDAP ile ortak uyarıları nasıl gidereceğinizi ve çözümlemeyi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: iainfou
ms.openlocfilehash: 06b0fa1979f18981ec5cf78dc9a9dbad8b196394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71258043"
---
# <a name="known-issues-secure-ldap-alerts-in-azure-active-directory-domain-services"></a>Bilinen sorunlar: Azure Etkin Dizin Etki Alanı Hizmetlerinde Güvenli LDAP uyarıları

Azure Active Directory Domain Services (Azure AD DS) ile iletişim kurmak için hafif dizin erişim protokolü (LDAP) kullanan uygulamalar ve hizmetler [güvenli LDAP kullanacak şekilde yapılandırılabilir.](tutorial-configure-ldaps.md) Güvenli LDAP'nin düzgün çalışması için uygun bir sertifika ve gerekli ağ bağlantı noktalarının açık olması gerekir.

Bu makale, Azure AD DS'de güvenli LDAP erişimiyle sık karşılaşılan uyarıları anlamanıza ve çözmenize yardımcı olur.

## <a name="aadds101-secure-ldap-network-configuration"></a>AADDS101: Güvenli LDAP ağ yapılandırması

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı için internet üzerinden güvenli LDAP etkindir. Ancak, bağlantı noktası 636'ya erişim bir ağ güvenlik grubu kullanılarak kilitlendirilemez. Bu, yönetilen etki alanında kullanıcı hesaplarını parola kaba kuvvet saldırılarına maruz bırakabilir.*

### <a name="resolution"></a>Çözüm

Güvenli LDAP'yi etkinleştirdiğinizde, gelen LDAPS erişimini belirli IP adreslerine kısıtlayan ek kurallar oluşturulması önerilir. Bu kurallar, Azure AD DS yönetilen etki alanını kaba kuvvet saldırılarına karşı korur. Güvenli LDAP için TCP bağlantı noktası 636 erişimini kısıtlamak için ağ güvenlik grubunu güncelleştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalında Ağ güvenlik **gruplarını**arayın ve seçin.
1. Yönetilen etki alanınızın ilişkili ağ güvenlik grubunu seçin *(Örneğin, AADDS-contoso.com-NSG,* ardından **Gelen güvenlik kurallarını** seçin
1. + TCP bağlantı noktası 636 için bir kural **ekleyin.** Gerekirse, kural oluşturmak için pencerede **Gelişmiş'i** seçin.
1. **Kaynak**için açılan menüden *IP Adresleri'ni* seçin. Güvenli LDAP trafiği için erişim vermek istediğiniz kaynak IP adreslerini girin.
1. **Hedef**olarak *herhangi* birini seçin, ardından Hedef bağlantı **noktası aralıkları**için *636* girin.
1. **Protokolü** *TCP* olarak ayarlayın ve *İzin Verilen* **Eylem.**
1. Kuralın önceliğini belirtin, ardından *RestrictLDAPS*gibi bir ad girin.
1. Hazır olduğunuzda, kuralı oluşturmak için **Ekle'yi** seçin.

Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

> [!TIP]
> TCP bağlantı noktası 636, Azure AD DS'nin sorunsuz çalışması için gereken tek kural değildir. Daha fazla bilgi edinmek için [Azure AD DS Ağı güvenlik gruplarına ve gerekli bağlantı noktalarına](network-considerations.md#network-security-groups-and-required-ports)bakın.

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502: Güvenli LDAP sertifikası süresi doluyor

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı için güvenli LDAP sertifikası [tarihte] sona erer.*

### <a name="resolution"></a>Çözüm

[Güvenli LDAP için bir sertifika oluşturmak için](tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap)adımları izleyerek bir yedek güvenli LDAP sertifikası oluşturun. Değiştirme sertifikasını Azure AD DS'ye uygulayın ve sertifikayı güvenli LDAP kullanarak bağlanan istemcilere dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

Sorunlarınız hala varsa, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
