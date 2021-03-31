---
title: Azure Active Directory ile dizin içi sağlama nedir? | Microsoft Belgeleri
description: Kimlik-Dizin sağlama için genel bakış açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 10/30/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4246bc4c62fd8e5e73ff18e383b8bf115d25ede8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101643855"
---
# <a name="what-is-inter-directory-provisioning"></a>Dizinler arası sağlama nedir?

Dizin, öğeleri ve ağ kaynaklarını bulmak, yönetmek, yönetmek ve düzenlemek için kullanılan paylaşılan bir bilgi altyapısıdır.  Dizin Hizmetleri kullanan uygulama örnekleri Microsoft Active Directory ve Azure AD ' dir.  Kimlikler Yardım dizin sistemleri, kimin ne erişebileceği ve belirli kaynakları kullanmasına izin verilen gibi belirleyici hale getirir.

Dizin içi sağlama iki farklı dizin hizmeti sistemi arasında bir kimlik sağlamadır.   Dizin içi sağlama için en yaygın senaryo, Azure AD 'de zaten Active Directory bir kullanıcının sağlandığı bir kullanıcı. Bu sağlama, Azure AD Connect eşitleme veya Azure AD Connect bulut sağlama gibi aracılardan gerçekleştirilebilir.

Dizin içi sağlama, [karma kimlik](../hybrid/whatis-hybrid-identity.md) ortamları oluşturmamıza olanak tanır.


## <a name="what-types-of-inter-directory-provisioning-does-azure-ad-support"></a>Azure AD 'de dizin içi sağlama türleri arasında ne tür destek vardır

Azure AD, dizin içi sağlama işlemini gerçekleştirmeye yönelik üç yöntemi şu anda desteklemektedir. Bu yöntemler şunlardır:

- [Azure AD Connect](../hybrid/whatis-azure-ad-connect.md) -ACTIVE DIRECTORY Azure AD 'ye yönelik siteler arası sağlama de dahil olmak üzere karma kimliğinizi karşılamak ve gerçekleştirmek Için tasarlanan Microsoft aracı.

- [Azure AD Connect bulut sağlama](../cloud-sync/what-is-cloud-sync.md) -karma kimlik hedeflerinizi karşılamak ve başarmak için tasarlanan yeni bir Microsoft Aracısı.  Active Directory ile Azure AD arasında bir hafif Dizin sağlama deneyimi sağlar.

- [Microsoft Identity Manager](/microsoft-identity-manager/microsoft-identity-manager-2016) -Microsoft 'un Şirket içi kimlik ve erişim yönetimi çözümü, kuruluşunuzdaki kullanıcıları, kimlik bilgilerini, ilkeleri ve erişimi yönetmenize yardımcı olur. Ayrıca, MıM Active Directory, Azure AD ve diğer dizinlere yönelik karma kimlik ortamları elde etmek için Gelişmiş dizin içi sağlama sağlar.

### <a name="key-benefits"></a>Önemli avantajlar

Dizin içi sağlamanın bu özelliği, aşağıdaki önemli iş avantajlarını sunmaktadır:

- [Parola karması eşitlemesi](../hybrid/whatis-phs.md) -Azure AD ile ŞIRKET içi ad parolasının karmasını eşitleyen bir oturum açma yöntemi.
- [Geçişli kimlik doğrulaması](../hybrid/how-to-connect-pta.md) -kullanıcıların şirket içinde ve bulutta aynı parolayı kullanmasına izin veren bir oturum açma yöntemi, ancak Federasyon ortamında ek altyapı gerektirmez.
- [Federasyon tümleştirme](../hybrid/how-to-connect-fed-whatis.md) -şirket içi AD FS altyapısını kullanarak karma ortam yapılandırmak için kullanılabilir. Ayrıca sertifika yenileme ve ek AD FS sunucu dağıtımları gibi AD FS yönetim özellikleri de sağlar.
- [Eşitleme](../hybrid/how-to-connect-sync-whatis.md) -Kullanıcı, Grup ve diğer nesneleri oluşturmaktan sorumludur.  Ayrıca, şirket içi kullanıcılarınız ve gruplarınız için kimlik bilgilerinin bulutla aynı olduğundan emin olun.  Bu eşitleme, parola karmaları da içerir.
- [Sistem durumu izleme](../hybrid/whatis-azure-ad-connect.md) -bu etkinliği görüntülemek için güçlü izleme sağlayabilir ve Azure Portal merkezi bir konum sağlayabilir. 


## <a name="next-steps"></a>Sonraki adımlar 
- [Kimlik yaşam döngüsü yönetimi nedir?](what-is-identity-lifecycle-management.md)
- [Sağlama nedir?](what-is-provisioning.md)
- [HR odaklı sağlama nedir?](what-is-hr-driven-provisioning.md)
- [Uygulama sağlama nedir?](what-is-app-provisioning.md)