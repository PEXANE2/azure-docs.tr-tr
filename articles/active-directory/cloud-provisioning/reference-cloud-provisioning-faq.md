---
title: Azure AD Connect bulut sağlama hakkında SSS
description: Bu belge, bulut sağlama için sık sorulan soruları açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbc1baa86bb81c8975587e84427a72ccc044805e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77916583"
---
# <a name="azure-active-directory-connect-faq"></a>Azure Active Directory Connect SSS

Azure Active Directory (Azure AD) Connect bulut sağlama için sık sorulan sorular hakkında bilgi edinin.

## <a name="general-installation"></a>Genel kurulum

**S: Bulut sağlama ne sıklıkta çalışır?**

Bulut sağlama her 2 dakikada bir çalışacak şekilde planlanır. Her 2 dakikada bir, herhangi bir kullanıcı, grup ve parola karma değişikliği Azure AD'ye göre sağlanacaktır.

**S: İlk çalıştırmada parola karma eşitleme hatalarını görme. Neden?**

Bu beklenen bir durumdur. Hatalar, Azure AD'de bulunmayan kullanıcı nesnesi sayesinde dir. Kullanıcı Azure AD'ye sağlandıktan sonra, sonraki çalıştırmada parola işyapışı oluşturmaları gerekir. Birkaç çalıştırma bekleyin ve parola karma eşitlemeartık hataları olduğunu onaylayın.

**S: Etkin Dizin örneği bulut öngöremezliği (örneğin, dizin uzantıları) tarafından desteklenmeyen özniteliklere sahipse ne olur?**

Bulut sağlama çalışır ve desteklenen öznitelikleri karşılar. Desteklenmeyen öznitelikler Azure AD'ye sağlanmaz. Etkin Dizin'deki dizin uzantılarını gözden geçirin ve Azure AD'ye akmak için bu özniteliğe ihtiyacınız olmadığından emin olun. Bir veya daha fazla özetmen gerekiyorsa, Azure AD Connect eşitlemesini kullanmayı veya gerekli bilgileri desteklenen özniteliklerden birine taşımayı düşünün (örneğin, uzantı öznitelikleri 1-15).

**S: Azure AD Connect eşitleme ile bulut sağlama arasındaki fark nedir?**

Azure AD Connect eşitlemeile, sağlama şirket içi eşitleme sunucusunda çalışır. Yapılandırma şirket içi eşitleme sunucusunda depolanır. Azure AD Connect bulut sağlama ile sağlama yapılandırması bulutta depolanır ve Azure AD sağlama hizmetinin bir parçası olarak bulutta çalışır. 

**S: Birden çok Active Directory ormanlarından eşitlemek için bulut sağlamayı kullanabilir miyim?**

Evet. Bulut sağlama, birden çok Active Directory ormanlarından eşitlemek için kullanılabilir. Çok ormanlı ortamda, tüm başvuruların (örnek, yönetici) etki alanı içinde olması gerekir.  

**S: Aracı nasıl güncellenir?**

Aracılar Microsoft tarafından otomatik olarak yükseltilir. BT ekibi için bu, yeni aracı sürümlerini sınamak ve doğrulamak zorunda kalma nın yükünü azaltır. 

**S: Otomatik yükseltmeyi devre dışı layabilir miyim?**

Otomatik yükseltmeyi devre dışı kılmış bir yol yoktur.

**S: Bulut sağlama için kaynak çapayı değiştirebilir miyim?**

Varsayılan olarak, bulut sağlama kaynak çapa olarak ObjectGUID'e geri dönüş le ms-ds-tutarlılık-GUID kullanır. Kaynak çapasını değiştirmenin desteklenen bir yolu yoktur.

**S: Bulut sağlama kullanırken AD etki alanı adı(lar) ile yeni hizmet ilkeleri görüyorum. Bu beklenen bir şey mi?**

Evet, bulut sağlama, hizmet ana adı olarak alan adı ile sağlama yapılandırması için bir hizmet ilkesi oluşturur. Hizmet temel yapılandırmasında herhangi bir değişiklik yapmayın.

**S: Senkronize edilmiş bir kullanıcının bir sonraki oturum açmada parolayı değiştirmesi gerektiğinde ne olur?**

Bulut sağlamada parola karma eşitleme etkinleştirilmişse ve senkronize kullanıcının şirket içi REKLAM'da bir sonraki oturum açmada parolayı değiştirmesi gerekiyorsa, bulut sağlama, değiştirilecek parola karmasını Azure AD olarak sağlamaz. Kullanıcı parolayı değiştirdiğinde, kullanıcı parolası karması AD'den Azure AD'ye doğru lanır.

**S: Bulut sağlama desteği herhangi bir nesne için ms-ds-tutarlılıkGUID writeback mu?**

Hayır, bulut sağlama, herhangi bir nesne (kullanıcı nesneleri dahil) için ms-ds-tutarlılıkGUID'in geri yazılmayı desteklemez. 

**S: Bulut sağlama kullanarak kullanıcıları temin ediyorum. Yapılandırmayı sildim. Azure AD'de neden hala eski senkronize edilmiş nesneleri görüyorum?** 

Yapılandırmayı sildiğinizde, bulut sağlama Azure AD'de eşitlenen nesneleri temizlemez. Eski nesnelere sahip olmadığınızı sağlamak için yapılandırmanın kapsamını boş bir grup veya Kuruluş Birimleri olarak değiştirin. Sağlama, nesneleri çalıştırDıktan ve temizledikten sonra yapılandırmayı devre dışı bırakıp silin. 

**S: Exchange hibritinin desteklenmemesi ne anlama gelir?**

Exchange Karma Dağıtımı özelliği, Exchange posta kutularının hem şirket içinde hem de Office 365'te aynı anda var olmalarına olanak sağlar. Azure AD Connect, Azure AD'den belirli bir öznitelikler kümesini şirket içi dizininize geri eşitler.  Bulut sağlama aracısı şu anda bu öznitelikleri şirket dizininize yeniden eşitlemez ve bu nedenle Azure AD Connect'in yerine desteklenmez.

**S: Bulut sağlama aracısını Windows Server Core'a yükleyebilir miyim?**

Hayır, aracıyı sunucu çekirdeğine yüklemek desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
