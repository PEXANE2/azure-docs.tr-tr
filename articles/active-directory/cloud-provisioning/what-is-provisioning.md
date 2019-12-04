---
title: Azure AD ile kimlik sağlama nedir? | Microsoft Docs
description: Kimlik sağlamaya genel bakış açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: acde7f30649e03d44fd891c959b53113b0e46f51
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793517"
---
# <a name="what-is-identity-provisioning"></a>Kimlik sağlama nedir?

Bugün, işletmeler ve şirketler, şirket içi ve bulut uygulamalarının daha fazla ve daha fazlasını haline geliyor.  Kullanıcıların hem şirket içinde hem de bulutta uygulamalara erişmesi gerekir. Bu çeşitli uygulamalar (Şirket içi ve bulut) genelinde tek bir kimliğe sahip olması gerekir.

Sağlama, belirli koşullara göre nesne oluşturma, nesneyi güncel tutma ve koşullar artık karşılanmamaları durumunda nesneyi silme işlemidir. Örneğin, yeni bir Kullanıcı kuruluşunuza katıldığında, bu kullanıcı HR sistemine girilir.  Bu noktada, sağlama bulutta karşılık gelen bir kullanıcı hesabı, Active Directory ve kullanıcının erişmesi gereken farklı uygulamalar oluşturabilir.  Bu, kullanıcının çalışmaya başlamasını ve gün içinde ihtiyaç duydukları uygulamalara ve sistemlere erişmesini sağlar. 

![bulut sağlama](media/what-is-provisioning/cloud1.png)

Azure Active Directory açısından, sağlama aşağıdaki temel senaryolara ayrılabilir.  

- **[HR odaklı sağlama](#hr-driven-provisioning)**  
- **[Uygulama sağlama](#app-provisioning)**  
- **[Dizin sağlama](#directory-provisioning)** 

## <a name="hr-driven-provisioning"></a>HR odaklı sağlama

![bulut sağlama](media/what-is-provisioning/cloud2.png)

HR 'dan buluta sağlama, HR sisteminizdeki bilgilere göre nesnelerin (kullanıcılar, roller, gruplar, vb.) oluşturulmasını içerir.  

En yaygın senaryo, yeni bir çalışan şirketinize katıldığında bu durumda HR sistemine girilmiştir.  Bu durumda, bulut tarafından sağlanır.  Bu durumda, Azure AD.  HR 'dan sağlama aşağıdaki senaryoları kapsayabilir. 

- **Yeni çalışanların işe** Alım-bulut HR 'a yeni bir çalışan eklendiğinde, bir kullanıcı hesabı Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarında otomatik olarak oluşturulur. Bu, e-posta adresi Ile bulut HR 'ye geri yazılır.
- **Çalışan özniteliği ve profil güncelleştirmeleri** -bir çalışan kaydı bulut HR (ad, başlık veya yönetici gibi) güncelleştirildiğinde, kullanıcı hesabı Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarında otomatik olarak güncelleştirilir.
- **Çalışan sonlandırmaları** -bulut HR ' de bir çalışan sonlandırıldığında, kullanıcı hesapları Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarında otomatik olarak devre dışı bırakılır.
- **Çalışan rehires** -bir çalışan bulut HR 'da yeniden çalıştırıldığında, eski hesapları Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamaları için otomatik olarak yeniden etkinleştirilebilir veya yeniden sağlanabilir.


## <a name="app-provisioning"></a>Uygulama sağlama

![bulut sağlama](media/what-is-provisioning/cloud3.png)

Uygulama sağlama, kullanıcının erişmesi gereken uygulamalardaki kullanıcı ve rolleri sağlamayı içerir.  

En yaygın senaryo, Azure AD 'deki bir Kullanıcı O365 veya Salesforce 'a sağlandığında olur.

## <a name="directory-provisioning"></a>Dizin sağlama

![bulut sağlama](media/what-is-provisioning/cloud4.png)

Şirket içi sağlama, şirket içi kaynaklardan (Active Directory benzer) Azure AD 'ye sağlamayı içerir.  

En yaygın senaryo, Active Directory (AD) ' deki bir Kullanıcı Azure AD 'ye sağlandığında olur.

Bu, bulut sağlama ve Microsoft Identity Manager Azure AD Connect Azure AD Connect eşitleme tarafından gerçekleştirilir. 
 
## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
