---
title: Azure AD 'de uygulama sağlama için bilinen sorunlar
description: Azure AD 'de otomatik uygulama sağlamalarıyla çalışırken bilinen sorunlar hakkında bilgi edinin.
author: kenwith
ms.author: kenwith
manager: celestedg
services: active-directory
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 08/12/2020
ms.reviewer: arvinh
ms.openlocfilehash: 127629cb0102c2736995364db9202cd837d99a17
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88214291"
---
# <a name="known-issues-application-provisioning"></a>Bilinen sorunlar: uygulama sağlama
Uygulama sağlama ile çalışırken farkında olmak üzere bilinen sorunlar. UserVoice üzerinde uygulama sağlama hizmeti hakkında geri bildirimde bulunmak için bkz. [Azure AD uygulama sağlama UserVoice](https://aka.ms/appprovisioningfeaturerequest). Hizmeti iyileştirebilmemiz için UserVoice 'ı yakından izliyoruz. 

> [!NOTE]
> Bu, bilinen sorunların kapsamlı bir listesi değildir. Listelenmeyen bir sorunu biliyorsanız, sayfanın en altında geri bildirim sağlayın.

## <a name="authorization"></a>Yetkilendirme 

**Başarılı bağlantı testinin ardından kaydedilemiyor**

Bir bağlantıyı başarıyla test edebilir, ancak kaydedebiliyorsanız kimlik bilgileri için izin verilen depolama sınırını aştınız. Daha fazla bilgi edinmek için bkz. [yönetici kimlik bilgilerini kaydetme sorunu](application-provisioning-config-problem-storage-limit.md).

**Kaydedilemiyor**

Kaydetmek için kiracı URL 'SI, gizli belirteç ve bildirim e-postası doldurulmalıdır. Bunlardan yalnızca birini sağlayamıyoruz. 

**Sağlama modu geri el ile değiştirilemiyor**

Hazırlama işlemini ilk kez yapılandırdıktan sonra sağlama modunun el ile otomatik 'e geçtiğine dikkat edin. El ile geri değiştiremezsiniz. Ancak, Kullanıcı arabirimi aracılığıyla sağlamayı kapatabilirsiniz. Kullanıcı arabiriminde sağlamayı devre dışı bırakmak, açılan menüyü el ile ayarlama ile aynı şekilde devre dışı bırakır.  


## <a name="attribute-mappings"></a>Öznitelik eşlemeleri 

**Öznitelik SamAccountName veya userType, kaynak özniteliği olarak kullanılamaz**

SamAccountName ve userType öznitelikleri varsayılan olarak bir kaynak öznitelik olarak kullanılamaz. Özniteliği eklemek için şemanızı genişletin. Şemayı genişleterek, kullanılabilir kaynak öznitelikleri listesine öznitelikleri ekleyebilirsiniz. Daha fazla bilgi için bkz. [eksik kaynak özniteliği](user-provisioning-sync-attributes-for-mapping.md). 

**Şema uzantısı için kaynak öznitelik açılan kutusu eksik**

Şemanıza yönelik uzantılar bazen Kullanıcı arabirimindeki kaynak özniteliği açılan listesinde eksik olabilir. Öznitelik eşlemelerinizin gelişmiş ayarlarına gidin ve öznitelikleri el ile ekleyin. Daha fazla bilgi için bkz. [öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).

**Null öznitelik sağlanamıyor**

Azure AD Şu anda null öznitelikler sağlayamaz. Kullanıcı nesnesinde bir öznitelik null ise atlanacak. 

**Öznitelik eşleme ifadeleri için en fazla karakter**

Öznitelik eşleme ifadelerinde en fazla 10.000 karakter olabilir. 


## <a name="service-issues"></a>Hizmet sorunları 

**Desteklenmeyen senaryolar**

- Sağlama parolaları desteklenmez. 
- İç içe grupların sağlanması desteklenmez. 
- Bu kiracılar için sağlama, kiracının boyutu nedeniyle desteklenmiyor. 

**Değişiklikler hedef uygulamadan Azure AD 'ye taşınmıyor**

Uygulama sağlama hizmeti dış uygulamalarda yapılan değişikliklerden haberdar değildir. Bu nedenle, geri alınacak bir eylem yapılmaz. Uygulama sağlama hizmeti, Azure AD 'de yapılan değişiklikleri temel alır.  

**Sağlama çevrimi tamamlanana kadar devam ediyor**

Sağlama `enabled = off` veya geçiş durdurma işlemleri yapıldığında, geçerli sağlama çevrimi tamamlanana kadar çalışmaya devam edecektir. Hizmeti, sağlama işlemini yeniden yapana kadar gelecekteki döngülerin yürütülmesini durdurur.

**Grubun üyesi sağlanmadı**

Bir grup kapsamdadır ve üye kapsam dışı olduğunda, Grup sağlanır. Kapsam dışı Kullanıcı sağlanmayacaktır. Üye kapsama geri dönerse, hizmet değişikliği hemen algılamaz. Sağlama yeniden başlatıldığında sorun ele alınacaktır. Tüm kullanıcıların doğru şekilde sağlandığından emin olmak için hizmeti düzenli aralıklarla yeniden başlatmanızı öneririz.  


## <a name="next-steps"></a>Sonraki adımlar
- [Sağlama nasıl çalışır?](how-provisioning-works.md)
