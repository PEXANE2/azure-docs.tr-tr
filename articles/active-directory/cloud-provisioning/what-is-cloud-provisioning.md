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
ms.openlocfilehash: a9433945ffe1181b62ee193baf1c2dd0b89ec14c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91856815"
---
# <a name="what-is-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama nedir?
Azure AD Connect bulut sağlama, kullanıcıların, grupların ve kişilerin Azure AD 'ye eşitlenmesi için karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek üzere tasarlanan yeni bir Microsoft aracısıdır.  Azure AD Connect eşitleme ' de birlikte kullanılabilir ve aşağıdaki avantajları sağlar:
    
- Çok ormanlı bir Active Directory orman ortamından bir Azure AD kiracısına eşitleme desteği: genel senaryolar, elde edilen şirketin ad ormanlarının, genellikle birden çok ad ormanına sahip olduğu üst şirketin ad ormanlarından ve şirketlerinden yalıtılmış olduğu birleşme & alımı içerir.
- Hafif sağlama aracılarıyla Basitleştirilmiş Yükleme: aracılar, bulutta yönetilen tüm eşitleme yapılandırması ile Azure AD 'ye bir köprü görevi görür. 
- Çoklu sağlama aracıları, AD 'den Azure AD 'ye Parola karması eşitlemesine bağlı olan kuruluşlar için özellikle kritik olan yüksek kullanılabilirlik dağıtımlarını basitleştirmek üzere kullanılabilir.


![Azure AD Connect nedir?](media/what-is-cloud-provisioning/architecture.png)

## <a name="how-is-azure-ad-connect-cloud-provisioning-different-from-azure-ad-connect-sync"></a>Azure AD Connect bulut Sağlama Azure AD Connect eşitlemeden farklı midir?
Azure AD Connect bulut sağlaması sayesinde, AD 'den Azure AD 'ye sağlama, Microsoft Online Services içinde düzenlenir. Kuruluşun, Azure AD ile AD arasında köprü görevi gören hafif bir aracı olan şirket içi ve IaaS barındırılan ortamlarında dağıtılması gerekir. Sağlama yapılandırması Azure AD 'de depolanır ve hizmetin bir parçası olarak yönetilir.

## <a name="azure-ad-connect-cloud-provisioning-video"></a>Azure AD Connect bulut sağlama videosu
Aşağıdaki kısa video, Azure AD Connect bulut sağlama için mükemmel bir aşırı sahip sağlar:

> [!VIDEO https://youtube.com/embed/mOT3ID02_YQ]


## <a name="comparison-between-azure-ad-connect-and-cloud-provisioning"></a>Azure AD Connect ile bulut sağlama arasında karşılaştırma

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
| Exchange Online özniteliklerini eşitleme |● |● |
| Uzantı özniteliklerini eşitler 1-15 |● |● |
| Müşteri tanımlı AD özniteliklerini (Dizin uzantıları) eşitler |● | |
| Parola karması eşitleme desteği |●|●|
| Pass-Through kimlik doğrulaması desteği |●||
| Federasyon desteği |●|●|
| Sorunsuz Çoklu Oturum Açma|● |●|
| Etki Alanı Denetleyicisi üzerinde yüklemeyi destekler |● |● |
| Windows Server 2012 ve Windows Server 2012 R2 desteği |● |● |
| Etki alanları/OU 'Lar/gruplar üzerinde filtrele |● |● |
| Nesnelerin öznitelik değerlerini filtrele |● | |
| Minimal öznitelik kümesinin eşitlenmesine izin ver (MinSync) |● |● |
| Öznitelik kaldırma işleminin AD'den Azure AD'ye akışına izin ver |● |● |
| Öznitelik akışları için gelişmiş özelleştirmeye izin ver |● | |
| Geri yazma desteği (parolalar, cihazlar, gruplar) |● | |
| Azure AD Domain Services desteği|● | |
| [Exchange karma geri yazma](../hybrid/reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) |● | |
| AD etki alanı başına 50.000 taneden fazla nesne desteği |● | |

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Bulut sağlamasını yükler](how-to-install.md)
