---
title: 'Azure Active Directory Domain Services: Ağ güvenlik gruplarında sorun giderme | Microsoft Docs'
description: Azure AD Domain Services için ağ güvenlik grubu yapılandırması sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 450ee5635b378ed7c4d4e4bedc1c4245f6b52d70
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "70743436"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Yönetilen etki alanınız için geçersiz ağ yapılandırması sorunlarını giderme
Bu makale, aşağıdaki uyarı iletisiyle sonuçlanan ağla ilgili yapılandırma hatalarını gidermenize ve çözmenize yardımcı olur:

## <a name="alert-aadds104-network-error"></a>Uyarı AADDS104: Ağ hatası
**Uyarı iletisi:** *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyor. Sanal ağınızda yapılandırılan bir ağ güvenlik grubu (NSG), yönetilen etki alanına erişimi engelliyorsa bu durum oluşabilir. Olası bir neden, internet 'ten gelen trafiği engelleyen Kullanıcı tanımlı bir yol olup olmalarıdır.*

Geçersiz NSG yapılandırması, Azure AD Domain Services ağ hatalarının en yaygın nedenlerdir. Sanal ağınız için yapılandırılan ağ güvenlik grubu (NSG), [belirli bağlantı noktalarına](network-considerations.md#network-security-groups-and-required-ports)erişime izin vermelidir. Bu bağlantı noktaları engellenirse, Microsoft yönetilen etki alanınızı izlemez veya güncelleştiremez. Ayrıca, Azure AD dizininiz ve yönetilen etki alanınız arasındaki eşitleme bundan etkilenir. NSG 'nizi oluştururken, hizmette kesintiye uğramamak için bu bağlantı noktalarını açık tutun.

### <a name="checking-your-nsg-for-compliance"></a>NSG 'niz uyumluluk için denetleniyor

1. Azure portal [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) sayfasına gidin
2. Tablosundan, yönetilen etki alanınızı etkin olan alt ağla ilişkili NSG 'yi seçin.
3. Sol bölmedeki **Ayarlar** altında **gelen güvenlik kuralları** ' na tıklayın.
4. Kuralları yerinde gözden geçirin ve hangi kuralların [Bu bağlantı noktalarına](network-considerations.md#network-security-groups-and-required-ports) erişimi engellediğini belirler
5. Kuralı silerek, kural ekleyerek veya tamamen yeni bir NSG oluşturarak uyumluluğu sağlamak için NSG 'yi düzenleyin. [Bir kural ekleme](#add-a-rule-to-a-network-security-group-using-the-azure-portal) veya yeni ve uyumlu bir NSG oluşturma adımları aşağıda verilmiştir

## <a name="sample-nsg"></a>Örnek NSG
Aşağıdaki tabloda, Microsoft 'un bilgileri izlemelerine, yönetmesine ve güncelleştirmesine izin verirken, yönetilen etki alanınızı güvenli tutacak bir örnek NSG gösterilmektedir.

![örnek NSG](./media/active-directory-domain-services-alerts/default-nsg.png)

>[!NOTE]
> Azure AD Domain Services sanal ağdan Kısıtlanmamış giden erişim gerektirir. Sanal ağ için giden erişimi kısıtlayan ek bir NSG kuralı oluşturmamalıdır.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Azure portal kullanarak bir ağ güvenlik grubuna bir kural ekleyin
PowerShell 'i kullanmak istemiyorsanız, Azure portal kullanarak NSG 'lere el ile tek kurallar ekleyebilirsiniz. Ağ güvenlik grubunuzda kurallar oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) sayfasına gidin.
2. Tablosundan, yönetilen etki alanınızı etkin olan alt ağla ilişkili NSG 'yi seçin.
3. Sol bölmedeki **Ayarlar** ' ın altında, **gelen güvenlik kuralları** ' na veya **giden güvenlik kuralları**' na tıklayın.
4. **Ekle** ' ye tıklayarak ve bilgileri doldurarak kuralı oluşturun. **Tamam**'ı tıklatın.
5. Kuralların, kurallar tablosunda bulunarak oluşturulduğunu doğrulayın.


## <a name="need-help"></a>Yardım mı gerekiyor?
[Geri bildirim paylaşmak veya destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.
