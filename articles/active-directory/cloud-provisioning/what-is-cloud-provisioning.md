---
title: Azure AD Connect bulut sağlama nedir? | Microsoft Docs
description: Azure AD Connect bulut sağlamayı açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10c778025c3def384f621cb5311d0d71ccb457d7
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951131"
---
# <a name="what-is-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama nedir?
Azure AD Connect bulut sağlama, kullanıcıların, grupların ve kişilerin Azure AD 'ye eşitlenmesi için karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek üzere tasarlanan yeni bir Microsoft aracısıdır.  Azure AD Connect eşitleme ' de birlikte kullanılabilir ve aşağıdaki avantajları sağlar:
    
- Çok ormanlı bir Active Directory orman ortamından bir Azure AD kiracısına eşitleme desteği: genel senaryolar, alınan şirketin ad ormanlarının üst şirketin ad 'sinden yalıtıldığı, birleşme & alımı içerir Geçmişte birden çok AD Ormanı olan ormanlar ve şirketler.
- Hafif sağlama aracılarıyla Basitleştirilmiş Yükleme: aracılar, bulutta yönetilen tüm eşitleme yapılandırması ile Azure AD 'ye bir köprü görevi görür. 
- Çoklu sağlama aracıları, AD 'den Azure AD 'ye Parola karması eşitlemesine bağlı olan kuruluşlar için özellikle kritik olan yüksek kullanılabilirlik dağıtımlarını basitleştirmek üzere kullanılabilir.


![Azure AD Connect nedir?](media/what-is-cloud-provisioning/architecture.png)

## <a name="how-is-azure-ad-connect-cloud-provisioning-different-from-azure-ad-connect-sync"></a>Azure AD Connect bulut Sağlama Azure AD Connect eşitlemeden farklı midir?
Azure AD Connect bulut sağlaması sayesinde, AD 'den Azure AD 'ye sağlama, Microsoft Online Services içinde düzenlenir. Kuruluşun, Azure AD ile AD arasında köprü görevi gören hafif bir aracı olan şirket içi ve IaaS barındırılan ortamlarında dağıtılması gerekir. Sağlama yapılandırması Azure AD 'de depolanır ve hizmetin bir parçası olarak yönetilir.

Aşağıdaki tabloda Azure AD Connect ve Azure AD Connect bulut sağlama arasında bir karşılaştırma sunulmaktadır:

| Özellik | Azure Active Directory Connect eşitleme| Azure Active Directory Connect bulutu sağlama |
|:--- |:---:|:---:|
|Tek şirket içi AD ormanına bağlanma|● |● |
| Birden çok şirket içi AD ormanına bağlanma |● |● |
| Birden fazla bağlantısı kesik şirket içi AD ormanına bağlanma | |● |
| Hafif aracı yükleme modeli | |● |
| Yüksek kullanılabilirlik için birden çok etkin aracı | |● |
| LDAP dizinlerine bağlanma|●| | 
| Kullanıcı nesneleri için destek |● |● |
| Grup nesneleri için destek |● |● |
| İletişim nesneleri için destek |● |● |
| Cihaz nesneleri için destek |● | |
| Öznitelik akışları için temel özelleştirmeye izin ver |● |● |
| Müşteri tanımlı AD özniteliklerini (Dizin uzantıları) eşitler |● | |
| Parola karması eşitleme desteği |●|●|
| Geçişli kimlik doğrulaması desteği |●||
| Federasyon desteği |●|●|
| Sorunsuz Çoklu Oturum Açma|● |●|
| Etki Alanı Denetleyicisi üzerinde yüklemeyi destekler |● |● |
| Windows Server 2012 ve Windows Server 2012 R2 desteği |● |● |
| Etki alanları/OU 'Lar/gruplar üzerinde filtrele |● |● |
| Nesnelerin öznitelik değerleri üzerinde filtreleme |● | |
| Minimal öznitelik kümesinin eşitlenmesine izin ver (MinSync) |● |● |
| Öznitelik kaldırma işleminin AD'den Azure AD'ye akışına izin ver |● |● |
| Öznitelik akışları için gelişmiş özelleştirmeye izin ver |● | |
| Geri yazma desteği (parolalar, cihazlar, gruplar) |● | |
| Azure AD Domain Services desteği|● | |
| Exchange karma yapılandırması |● | |

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Bulut sağlamasını yükler](how-to-install.md)
