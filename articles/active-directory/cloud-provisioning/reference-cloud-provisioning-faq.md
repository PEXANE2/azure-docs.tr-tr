---
title: Azure AD Connect bulut sağlama hakkında SSS
description: Bu belgede, bulut sağlaması için sık sorulan sorular açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 622d38e09f65d05d7cba7a34f30a070c27d3fd37
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89658382"
---
# <a name="azure-active-directory-connect-cloud-provisioning-faq"></a>Azure Active Directory Connect bulut sağlama SSS

Azure Active Directory (Azure AD) ile ilgili bulut sağlamasını bağlama hakkında sık sorulan sorular hakkında bilgi edinin.

## <a name="general-installation"></a>Genel yükleme

**S: bulut sağlaması ne sıklıkla çalışır?**

Bulut sağlama, her 2 dakikada bir çalışacak şekilde zamanlanır. Her 2 dakikada bir Kullanıcı, Grup ve parola karma değişikliği Azure AD 'ye sağlanacak.

**S: ilk çalıştırmada Parola karması eşitleme başarısızlıklarını görme. Kaydol?**

Bu beklenen bir durumdur. Hataların nedeni, Kullanıcı nesnesi tarafından Azure AD 'de yok. Kullanıcı Azure AD 'ye sağlandıktan sonra, parola karmalarının sonraki çalıştırmada sağlanması gerekir. Birkaç çalıştırma bekleyin ve parola karması eşitleme 'nin artık hatalara sahip olmadığını onaylayın.

**S: Active Directory örneği, bulut sağlaması tarafından desteklenmeyen öznitelikler içeriyorsa ne olur (örneğin, Dizin uzantıları)?**

Bulut sağlama işlemi çalışır ve desteklenen öznitelikleri hazırlar. Desteklenmeyen öznitelikler Azure AD 'ye Hazırlanmayacak. Active Directory 'de Dizin uzantılarını inceleyin ve Azure AD 'ye Flow için bu özniteliklere ihtiyacınız olmadığından emin olun. Bir veya daha fazla öznitelik gerekliyse, Azure AD Connect eşitleme ' yi kullanmayı veya gerekli bilgileri desteklenen özniteliklerden birine taşımayı düşünün (örneğin, uzantı öznitelikleri 1-15).

**S: Azure AD Connect eşitleme ve bulut sağlama arasındaki fark nedir?**

Azure AD Connect eşitleme ile, sağlama, şirket içi eşitleme sunucusunda çalıştırılır. Yapılandırma, şirket içi eşitleme sunucusunda depolanır. Azure AD Connect bulut sağlaması sayesinde, sağlama yapılandırması bulutta depolanır ve Azure AD sağlama hizmeti 'nin bir parçası olarak bulutta çalışır. 

**S: birden çok Active Directory ormanınızdan eşitlemek için bulut sağlamayı kullanabilir miyim?**

Evet. Bulut sağlama, birden çok Active Directory ormanınızdan eşitleme yapmak için kullanılabilir. Çok ormanlı ortamda, tüm başvuruların (örneğin, yöneticinin) etki alanı içinde olması gerekir.  

**S: aracı nasıl güncelleştirilir?**

Aracılar Microsoft tarafından otomatik olarak yükseltilir. BT ekibi için bu, yeni aracı sürümlerini test etmek ve doğrulamak zorunda olma yükünü azaltır. 

**S: otomatik yükseltmeyi devre dışı bırakabilir miyim?**

Otomatik yükseltmeyi devre dışı bırakmak için desteklenen bir yol yoktur.

**S: bulut sağlaması için kaynak bağlayıcısını değiştirebilir miyim?**

Varsayılan olarak, bulut sağlaması, kaynak bağlantısı olarak Objectguıd 'e geri dönüş ile ms-DS-tutarlılık-GUID kullanır. Kaynak bağlayıcısını değiştirmek için desteklenen bir yol yoktur.

**S: bulut sağlama kullanılırken AD etki alanı adları ile yeni hizmet sorumluları görüyorum. Beklensin mi?**

Evet, bulut sağlama, hizmet sorumlusu adı olarak etki alanı adı ile sağlama yapılandırması için bir hizmet sorumlusu oluşturur. Hizmet sorumlusu yapılandırmasında herhangi bir değişiklik yapmayın.

**S: bir sonraki oturum açışında parolayı değiştirmek için eşitlenen bir kullanıcı gerektiğinde ne olur?**

Bulut sağlaması sırasında parola karması eşitleme etkinse ve şirket içi AD 'de bir sonraki oturum açma sırasında parola değiştirmek gerekirse, bulut sağlama, "yapılacak-değişiklik" parola karmasını Azure AD 'ye sağlamaz. Kullanıcı parolayı değiştirdiğinde, Kullanıcı parolası karması AD 'den Azure AD 'ye sağlanır.

**S: bulut sağlama, herhangi bir nesne için ms-DS-ımnguıd 'in geri yazma işlemini destekliyor mu?**

Hayır, bulut sağlaması herhangi bir nesne (Kullanıcı nesneleri dahil) için ms-DS-ımdsguıd ' y i geri yazmayı desteklemez. 

**S: bulut sağlamasını kullanarak kullanıcıları sağlamadım. Yapılandırmayı sildim. Azure AD 'de hala eşitlenmiş olan eski nesneleri görüyorum mi?** 

Yapılandırmayı sildiğinizde, bulut sağlaması Azure AD 'de eşitlenmiş nesneleri otomatik olarak kaldırmaz. Eski nesnelerinizin olmadığından emin olmak için, yapılandırmanın kapsamını boş bir grup veya kuruluş birimleriyle değiştirin. Sağlama çalıştıktan ve nesneleri temizledikten sonra yapılandırmayı devre dışı bırakın ve silin. 

**S: Exchange hibrit 'in desteklenmediği ne anlama geliyor?**

Exchange karma dağıtımı özelliği, Exchange posta kutularının hem şirket içinde hem de Microsoft 365 birlikte bulunması için izin verir. Azure AD Connect, Azure AD'den belirli bir öznitelikler kümesini şirket içi dizininize geri eşitler.  Bulut sağlama Aracısı Şu anda bu öznitelikleri şirket içi dizininizle eşitlemez ve bu nedenle Azure AD Connect bir değiştirme olarak desteklenmez.

**S: bulut sağlama aracısını Windows Server Core 'a yükleyebilir miyim?**

Hayır, aracıyı sunucu çekirdeği üzerine yükleme desteklenmiyor.

**S: bulut sağlama aracısına sahip bir hazırlama sunucusu kullanabilir miyim?**

Hayır, hazırlama sunucuları desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
