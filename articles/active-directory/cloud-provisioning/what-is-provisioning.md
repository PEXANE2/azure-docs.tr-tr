---
title: Azure AD ile kimlik sağlama nedir? | Microsoft Belgeleri
description: Kimlik sağlama genel görünümünü açıklar.
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
ms.openlocfilehash: 28513c57101af67695d10056b3dc8e6537dcddb2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76712544"
---
# <a name="what-is-identity-provisioning"></a>Kimlik sağlama nedir?

Günümüzde işletmeler ve şirketler şirket içi ve bulut uygulamalarının daha fazla bir karışımı haline gelmektedir.  Kullanıcılar, hem şirket içinde hem de bulutta uygulamalara erişim gerektirir. Bu çeşitli uygulamalar (şirket içi ve bulut) arasında tek bir kimliğe sahip olmak gerekir.

Sağlama, belirli koşullara dayalı bir nesne oluşturma, nesneyi güncel tutma ve koşullar artık karşılanmadığında nesneyi silme işlemidir. Örneğin, kuruluşunuza yeni bir kullanıcı katıldığında, bu kullanıcı İk sistemine girilir.  Bu noktada, sağlama bulutta, Active Directory'de ve kullanıcının erişmesi gereken farklı uygulamalarda karşılık gelen bir kullanıcı hesabı oluşturabilir.  Bu, kullanıcının çalışmaya başlamasını ve ilk gün ihtiyaç duydukları uygulamalara ve sistemlere erişmesine olanak tanır. 

![bulut sağlama](media/what-is-provisioning/cloud1.png)

Azure Etkin Dizini ile ilgili olarak, sağlama aşağıdaki önemli senaryolara bölünebilir.  

- **[İk odaklı sağlama](#hr-driven-provisioning)**  
- **[Uygulama sağlama](#app-provisioning)**  
- **[Dizin sağlama](#directory-provisioning)** 

## <a name="hr-driven-provisioning"></a>İk odaklı sağlama

![bulut sağlama](media/what-is-provisioning/cloud2.png)

İk'dan buluta sağlama, İk sisteminizdeki bilgilere dayalı nesnelerin (kullanıcılar, roller, gruplar, vb.) oluşturulmasını içerir.  

En yaygın senaryo, yeni bir çalışan şirketinize katıldığında İk sistemine girilmesidir.  Bu gerçekleştiğinde, buluta göre sağlanırlar.  Bu durumda, Azure AD.  İk'dan temin aşağıdaki senaryoları kapsayabilir. 

- **Yeni çalışanların işe alınması** - Bulut İk'ya yeni bir çalışan eklendiğinde, Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarında otomatik olarak bir kullanıcı hesabı oluşturulur ve e-posta adresinin Cloud HR'ye yazılması yla birlikte.
- **Çalışan özniteliği ve profil güncelleştirmeleri** - Bir çalışan kaydı bulut İk'da (adı, unvanı veya yöneticisi gibi) güncelleştirildiğinde, kullanıcı hesapları Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarında otomatik olarak güncelleştirilir.
- **Çalışan sonlandırmaları** - Bir çalışanın bulut İk'da sonlandırılması durumunda, kullanıcı hesabı Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarında otomatik olarak devre dışı bırakılır.
- **Çalışan yeniden işe alma -** Bir çalışan bulut İk'da yeniden işe alındığında, eski hesabı otomatik olarak yeniden etkinleştirilebilir veya (tercihinize bağlı olarak) Active Directory, Azure Active Directory ve isteğe bağlı olarak Office 365 ve Azure AD tarafından desteklenen diğer SaaS uygulamalarına yeniden sağlanabilir.


## <a name="app-provisioning"></a>Uygulama sağlama

![bulut sağlama](media/what-is-provisioning/cloud3.png)

Azure Etkin Dizini'nde (Azure AD) **[uygulama sağlama](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)** terimi, kullanıcıların erişilmesi gereken bulut uygulamalarında otomatik olarak kullanıcı kimlikleri ve rolleri oluşturmayı ifade eder. Kullanıcı kimlikleri oluşturmaya ek olarak, otomatik sağlama, durum veya roller değiştikçe kullanıcı kimliklerinin bakımını ve kaldırılmasını içerir. Sık karşılaşılan senaryolar arasında Bir Azure AD kullanıcısına [Dropbox,](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial) [Salesforce](https://docs.microsoft.com/azure/active-directory/saas-apps/salesforce-provisioning-tutorial), [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial)ve daha fazlası gibi uygulamalarda yardımcı olmak yer almaktadır.

## <a name="directory-provisioning"></a>Dizin sağlama

![bulut sağlama](media/what-is-provisioning/cloud4.png)

Şirket içi sağlama, şirket içi kaynaklardan (Active Directory gibi) Azure AD'ye sağlama yı içerir.  

En yaygın senaryo, Active Directory (AD)'deki bir kullanıcı Azure AD'de sağlandığında olur.

Bu, Azure AD Connect eşitleme, Azure AD Connect bulut sağlama ve Microsoft Identity Manager tarafından gerçekleştirilmiştir. 
 
## <a name="next-steps"></a>Sonraki adımlar 

- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
- [Bulut sağlama yı yükleme](how-to-install.md)
