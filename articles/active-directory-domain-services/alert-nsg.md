---
title: Azure AD DS'deki ağ güvenlik grubu uyarılarını çözümle | Microsoft Dokümanlar
description: Azure Active Directory Etki Alanı Hizmetleri için ağ güvenliği grubu yapılandırma uyarılarını nasıl sorun gidereceğinizi ve çözeceğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 09/19/2019
ms.author: iainfou
ms.openlocfilehash: 959f1e3f25602938d769c574ea975c4bba9300e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71257996"
---
# <a name="known-issues-network-configuration-alerts-in-azure-active-directory-domain-services"></a>Bilinen sorunlar: Azure Active Directory Etki Alanı Hizmetlerinde ağ yapılandırma uyarıları

Uygulamaların ve hizmetlerin Azure Active Directory Domain Services (Azure AD DS) ile doğru şekilde iletişim kurmasına izin vermek için, trafiğin akmasına izin vermek için belirli ağ bağlantı noktalarının açık olması gerekir. Azure'da ağ güvenlik gruplarını kullanarak trafik akışını denetlersiniz. Azure AD DS yönetilen etki alanının sistem durumu, gerekli ağ güvenlik grubu kuralları yerinde değilse bir uyarı gösterir.

Bu makale, ağ güvenliği grubu yapılandırma sorunları için yaygın uyarıları anlamanıza ve çözmenize yardımcı olur.

## <a name="alert-aadds104-network-error"></a>Uyarı AADDS104: Ağ hatası

### <a name="alert-message"></a>Uyarı iletisi

*Microsoft, yönetilen bu etki alanı için etki alanı denetleyicilerine erişemiyor. Sanal ağınızda yapılandırılan bir ağ güvenlik grubu (NSG) yönetilen etki alanına erişimi engellerse, bu durum oluşabilir. Başka bir olası nedeni internetten gelen trafiği engelleyen kullanıcı tanımlı bir rota varsa.*

Geçersiz ağ güvenliği grubu kuralları, Azure AD DS'nin ağ hatalarının en yaygın nedenidir. Sanal ağ için ağ güvenlik grubu belirli bağlantı noktalarına ve protokollere erişime izin vermelidir. Bu bağlantı noktaları engellenirse, Azure platformu yönetilen etki alanını izleyebilir veya güncelleştiremez. Azure AD dizini ile Azure AD DS yönetilen etki alanı arasındaki eşitleme de etkilenir. Hizmetkesintisini önlemek için varsayılan bağlantı noktalarını açık tuttuğundan emin olun.

## <a name="default-security-rules"></a>Varsayılan güvenlik kuralları

Aşağıdaki varsayılan gelen ve giden güvenlik kuralları, Azure AD DS yönetilen bir etki alanı için ağ güvenlik grubuna uygulanır. Bu kurallar Azure AD DS'yi güvende tutar ve Azure platformunun yönetilen etki alanını izlemesine, yönetmesine ve güncelleştirmesine olanak tanır. Ayrıca, [güvenli LDAP'yi yapılandırdığınızda][configure-ldaps]gelen trafiğe izin veren ek bir kuralınız da olabilir.

### <a name="inbound-security-rules"></a>Gelen güvenlik kuralları

| Öncelik | Adı | Bağlantı noktası | Protokol | Kaynak | Hedef | Eylem |
|----------|------|------|----------|--------|-------------|--------|
| 101      | SyncWithAzureAD'a izin verme | 443 | TCP | AzureActiveDirectoryDomainServices | Herhangi biri | İzin Ver |
| 201      | İzin VERMERD | 3389 | TCP | Corpnetsaw | Herhangi biri | İzin Ver |
| 301      | AllowPSRemoting | 5986| TCP | AzureActiveDirectoryDomainServices | Herhangi biri | İzin Ver |
| 65000    | AllVnetInBound | Herhangi biri | Herhangi biri | VirtualNetwork | VirtualNetwork | İzin Ver |
| 65001    | AllowAzureLoadBalancerInBound | Herhangi biri | Herhangi biri | AzureLoadBalancer | Herhangi biri | İzin Ver |
| 65500    | DenyAllInBound | Herhangi biri | Herhangi biri | Herhangi biri | Herhangi biri | Reddet |

### <a name="outbound-security-rules"></a>Giden güvenlik kuralları

| Öncelik | Adı | Bağlantı noktası | Protokol | Kaynak | Hedef | Eylem |
|----------|------|------|----------|--------|-------------|--------|
| 65000    | AllVnetOutBound | Herhangi biri | Herhangi biri | VirtualNetwork | VirtualNetwork | İzin Ver |
| 65001    | İzin VerinAzureLoadBalancerOutBound | Herhangi biri | Herhangi biri |  Herhangi biri | Internet | İzin Ver |
| 65500    | DenyAllOutBound | Herhangi biri | Herhangi biri | Herhangi biri | Herhangi biri | Reddet |

>[!NOTE]
> Azure AD DS'nin sanal ağdan sınırsız giden erişime ihtiyacı vardır. Sanal ağ için giden erişimi kısıtlayan ek kurallar oluşturmanızı önermiyoruz.

## <a name="verify-and-edit-existing-security-rules"></a>Varolan güvenlik kurallarını doğrulama ve bu kuralları yeniden yapma

Varolan güvenlik kurallarını doğrulamak ve varsayılan bağlantı noktalarının açık olduğundan emin olmak için aşağıdaki adımları tamamlayın:

1. Azure portalında Ağ güvenlik **gruplarını**arayın ve seçin.
1. Yönetilen etki alanınızın ilişkili ağ güvenlik grubunu seçin *(Örneğin, AADDS-contoso.com-NSG.*
1. Genel **Bakış** sayfasında, varolan gelen ve giden güvenlik kuralları gösterilir.

    Gelen ve giden kuralları gözden geçirin ve önceki bölümdeki gerekli kurallar listesiyle karşılaştırın. Gerekirse, gerekli trafiği engelleyen özel kuralları seçin ve silin. Gerekli kurallardan herhangi biri eksikse, sonraki bölüme bir kural ekleyin.

    Gerekli trafiğe izin vermek için kurallar ekledikten veya sildikten sonra, Azure AD DS yönetilen etki alanının sağlık durumu iki saat içinde otomatik olarak kendini güncelleştirir ve uyarıyı kaldırır.

### <a name="add-a-security-rule"></a>Güvenlik kuralı ekleme

Eksik güvenlik kuralı eklemek için aşağıdaki adımları tamamlayın:

1. Azure portalında Ağ güvenlik **gruplarını**arayın ve seçin.
1. Yönetilen etki alanınızın ilişkili ağ güvenlik grubunu seçin *(Örneğin, AADDS-contoso.com-NSG.*
1. Sol paneldeki **Ayarlar'ın** altında, eklemeniz gereken kurala bağlı olarak *Gelen Güvenlik Kuralları'nı* veya Giden *güvenlik kurallarını* tıklatın.
1. **Ekle'yi**seçin, ardından bağlantı noktasına, protokole, yöne vb. göre gerekli kuralı oluşturun. Hazır olduğunda **Tamam'ı**seçin.

Güvenlik kuralının eklenmesi ve listede gösterilmesi birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Sorunlarınız hala varsa, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-ldaps]: tutorial-configure-ldaps.md
