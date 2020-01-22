---
title: Azure AD Connect bulut sağlama hakkında SSS
description: Bu belgede, bulut sağlaması için sık sorulan sorular açıklanmaktadır.
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
ms.openlocfilehash: dc3c4a943f24ba1f987aa1daf513b9e05ada65a7
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76310008"
---
# <a name="azure-active-directory-connect-faq"></a>Azure Active Directory Connect SSS

Azure Active Directory (Azure AD) ile ilgili bulut sağlamasını bağlama hakkında sık sorulan sorular hakkında bilgi edinin.

## <a name="general-installation"></a>Genel yükleme

**S: bulut sağlaması ne sıklıkla çalışır?**

Bulut sağlama, her 2 dakikada bir çalışacak şekilde zamanlanır. Her 2 dakikada bir Kullanıcı, Grup ve parola karma değişikliği Azure AD 'ye sağlanacak.

**S: ilk çalıştırmada Parola karması eşitleme başarısızlıklarını görme. Kaydol?**

Bu beklenen bir durumdur. Hataların nedeni, Kullanıcı nesnesi tarafından Azure AD 'de yok. Kullanıcı Azure AD 'ye sağlandıktan sonra, parola karmalarının sonraki çalıştırmada sağlanması gerekir. Birkaç çalıştırma bekleyin ve parola karması eşitleme 'nin artık hatalara sahip olmadığını onaylayın.

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

Bulut sağlaması sırasında parola karması eşitleme etkinse ve şirket içi AD 'de bir sonraki oturum açma sırasında parola değiştirmek gerekirse, bulut sağlama, parola karmasını Azure AD 'ye değiştirmeyecektir. Kullanıcı parolayı değiştirdiğinde, Kullanıcı parolası karması AD 'den Azure AD 'ye sağlanır.

**S: bulut sağlama, herhangi bir nesne için ms-DS-ımnguıd 'in geri yazma işlemini destekliyor mu?**

Hayır, bulut sağlaması herhangi bir nesne (Kullanıcı nesneleri dahil) için ms-DS-ımdsguıd ' y i geri yazmayı desteklemez. 

**S: bulut sağlamasını kullanarak kullanıcıları sağlamadım. Yapılandırmayı sildim. Azure AD 'de hala eşitlenmiş olan eski nesneleri görüyorum mi?** 

Yapılandırmayı sildiğinizde, bulut sağlaması Azure AD 'de eşitlenmiş nesneleri temizlemez. Eski nesnelerinizin olmadığından emin olmak için, yapılandırmanın kapsamını boş bir grup veya kuruluş birimleriyle değiştirin. Sağlama çalıştıktan ve nesneleri temizledikten sonra yapılandırmayı devre dışı bırakın ve silin. 

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
