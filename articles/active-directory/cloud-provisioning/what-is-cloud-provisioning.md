---
title: Azure AD Connect bulut sağlama nedir? | Microsoft Belgeleri
description: Azure AD Connect bulut sağlamayı açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a86d34fca9a88b0df601533a0f3de1cc97ad1a2f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80050591"
---
# <a name="what-is-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama nedir?
Azure AD Connect bulut sağlama, kullanıcıları, grupları ve kişileri Azure AD ile senkronize etmek için karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek üzere tasarlanmış yeni bir Microsoft aracısıdır.  Azure AD Connect eşitleminin yanında kullanılabilir ve aşağıdaki avantajları sağlar:
    
- Çok ormanla bağlantısı kesilmiş Active Directory orman ortamından Azure AD kiracısına eşitleme desteği: Ortak senaryolar, satın alınan şirketin AD ormanlarının ana şirketin AD ormanlarından ve geçmişte birden fazla AD ormanı olan şirketlerden izole edildiği birleşme & satın alma içerir.
- Hafif sağlama aracıları ile basitleştirilmiş yükleme: Aracılar, bulutta yönetilen tüm eşitleme yapılandırmasıyla AD'den Azure AD'ye köprü görevi gösterir. 
- Yüksek kullanılabilirlik dağıtımlarını basitleştirmek için birden çok sağlama aracısı kullanılabilir, özellikle AD'den Azure AD'ye parola karma eşitlemesine güvenen kuruluşlar için kritik öneme sahiptir.


![Azure AD Connect nedir?](media/what-is-cloud-provisioning/architecture.png)

## <a name="how-is-azure-ad-connect-cloud-provisioning-different-from-azure-ad-connect-sync"></a>Azure AD Connect bulut sağlamanın Azure AD Connect eşitlemesinden farkı nedir?
Azure AD Connect bulut sağlama ile AD'den Azure AD'ye sağlama Microsoft Çevrimiçi Hizmetler'de düzenlenir. Bir kuruluşun yalnızca, Azure AD ve AD arasında köprü görevi gören hafif bir aracı olan şirket içi ve IaaS tarafından barındırılan ortamda dağıtması gerekir. Sağlama yapılandırması Azure AD'de depolanır ve hizmetin bir parçası olarak yönetilir.

Aşağıdaki tablo, Azure AD Connect ve Azure AD Connect bulut sağlama arasında bir karşılaştırma sağlar:

| Özellik | Azure Active Directory Connect eşitle| Azure Active Directory Connect bulut sağlama |
|:--- |:---:|:---:|
|Tek şirket içi AD ormanına bağlanma|● |● |
| Birden çok şirket içi AD ormanına bağlanma |● |● |
| Birden fazla bağlantısı kesilmiş şirket içi AD ormanlarına bağlanın | |● |
| Hafif ajan kurulum modeli | |● |
| Yüksek kullanılabilirlik için birden fazla etkin ajan | |● |
| LDAP dizinlerine bağlanın|●| | 
| Kullanıcı nesneleri için destek |● |● |
| Grup nesneleri için destek |● |● |
| Kişi nesneleri için destek |● |● |
| Aygıt nesneleri için destek |● | |
| Öznitelik akışları için temel özelleştirmeye izin ver |● |● |
| Sychronize Exchange çevrimiçi öznitelikleri |● |● |
| Uzantı özniteliklerini eşitle 1-15 |● |● |
| Müşteri tanımlı AD özniteliklerini eşitle (dizin uzantıları) |● | |
| Şifre Hash Sync desteği |●|●|
| Geçiş Kimlik Doğrulama sı için destek |●||
| Federasyon desteği |●|●|
| Sorunsuz Çoklu Oturum Açma|● |●|
| Etki Alanı Denetleyicisi üzerinde yüklemeyi destekler |● |● |
| Windows Server 2012 ve Windows Server 2012 R2 desteği |● |● |
| Etki Alanları/OSB'ler/gruplar afiltreleme |● |● |
| Nesnelerin öznitelik değerlerine filtre uygulama |● | |
| Minimal öznitelik kümesinin eşitlenmesine izin ver (MinSync) |● |● |
| Öznitelik kaldırma işleminin AD'den Azure AD'ye akışına izin ver |● |● |
| Öznitelik akışları için gelişmiş özelleştirmeye izin ver |● | |
| Geri yazma desteği (parolalar, aygıtlar, gruplar) |● | |
| Azure AD Etki Alanı Hizmetleri desteği|● | |
| [Exchange hibrid writeback](../hybrid/reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) |● | |
| AD etki alanı başına 50.000'den fazla nesne desteği |● | |

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Bulut sağlama yı yükleme](how-to-install.md)
