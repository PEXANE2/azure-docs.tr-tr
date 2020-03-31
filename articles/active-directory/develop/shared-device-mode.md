---
title: Android cihazlar için paylaşılan cihaz modu | Azure
description: Firstline çalışanlarının bir Android cihazını paylaşmasına olanak tanıyan paylaşılan aygıt modu hakkında bilgi edinin
services: active-directory
documentationcenter: dev-center-name
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 1/15/2020
ms.author: marsma
ms.reviwer: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 9928b64d286cc5072f28f7cc17e4af3e95662cd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085622"
---
# <a name="shared-device-mode-for-android-devices"></a>Android cihazlar için paylaşılan cihaz modu

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Perakende ortakları, uçuş ekibi üyeleri ve saha hizmeti çalışanları gibi firstline çalışanları, işlerini yapmak için genellikle paylaşılan bir mobil cihaz kullanır. Paylaşılan aygıttaki müşteri ve iş verilerine erişmek için parola paylaşmaya veya numaraları sabitlemeye başladıklarında bu durum sorunlu hale gelir.

Paylaşılan aygıt modu, bir Android cihazını birden çok çalışan tarafından kolayca paylaşılabilmek için yapılandırmanıza olanak tanır. Çalışanlar oturum açabilir ve müşteri bilgilerine hızlı bir şekilde erişebilir. Vardiyaları veya görevleri tamamlandığında, aygıtın dışına çıkış yapabilir ve bir sonraki çalışanın kullanımına hemen hazır olur.

Paylaşılan aygıt modu, aygıtın Microsoft kimlik destekli yönetimini de sağlar.

Paylaşılan bir aygıt modu uygulaması oluşturmak için geliştiriciler ve bulut aygıt yöneticileri birlikte çalışır:

- Geliştiriciler tek hesaplı bir uygulama yazar (paylaşılan aygıt modunda birden `"shared_device_mode_supported": true` fazla hesaplı uygulamalar desteklenmez), uygulamanın yapılandırmasına eklenir ve paylaşılan aygıt oturum açma gibi şeyleri işlemek için kod yazar.
- Aygıt yöneticileri, kimlik doğrulayıcı uygulamasını yükleyerek ve aygıtı kimlik doğrulayıcı uygulamasını kullanarak paylaşılan moda ayarlayarak aygıtı paylaşılacak şekilde hazırlar. Yalnızca Bulut Aygıt [Yöneticisi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#cloud-device-administrator) rolünde olan [kullanıcılar, Authenticator uygulamasını](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-overview)kullanarak aygıtı paylaşılan moda sokabilir. Azure portalındaki kuruluş rollerinizin üyeliğini Azure Active **Directory** > **Roles ve Yöneticiler** > **Bulut Aygıt Yöneticisi**aracılığıyla yapılandırabilirsiniz.

 Bu makalede, öncelikle geliştiriciler hakkında ne düşünmesi gerektiğini odaklanır.

## <a name="single-vs-multiple-account-applications"></a>Tek vs çoklu hesap uygulamaları

Microsoft Kimlik Doğrulama Kitaplığı SDK (MSAL) kullanılarak yazılan uygulamalar tek bir hesabı veya birden çok hesabı yönetebilir. Ayrıntılar için [tek hesap moduna veya çoklu hesap moduna](https://docs.microsoft.com/azure/active-directory/develop/single-multi-account)bakın. Uygulamanızın kullanabileceği Microsoft kimlik platformu özellikleri, uygulamanın tek hesap modunda mı yoksa çoklu hesap modunda mı çalıştırdığına bağlı olarak değişir.

**Paylaşılan aygıt modu uygulamaları yalnızca tek hesap modunda çalışır.**

> [!IMPORTANT]
> Yalnızca birden çok hesap modunu destekleyen uygulamalar paylaşılan bir aygıtta çalıştırılmaz. Bir çalışan tek hesap modunu desteklemeyen bir uygulama yüklerse, paylaşılan aygıtta çalışmaz.
>
> MSAL SDK yayımlanmadan önce yazılmış uygulamalar birden çok hesap modunda çalışır ve paylaşılan mod da çalıştırılabilmeleri için tek hesap modunu destekleyecek şekilde güncelleştirilmelidir.

**Hem tek hesaplı hem de çoklu hesapları destekleme**

Uygulamanız, hem kişisel cihazlarda hem de paylaşılan cihazlarda çalıştırmayı desteklemek için oluşturulabilir. Uygulamanız şu anda birden çok hesabı destekliyorsa ve paylaşılan cihaz modunu desteklemek istiyorsanız, tek hesap modu için destek ekleyin.

Uygulamanızın, üzerinde çalışan aygıtın türüne bağlı olarak davranışını değiştirmesini de isteyebilirsiniz. Tek `ISingleAccountPublicClientApplication.isSharedDevice()` hesap modunda ne zaman çalıştırılacak kullanılacağını belirlemek için kullanın.

Uygulamanızın üzerinde olduğu aygıt türünü temsil eden iki farklı arabirim vardır. MSAL'ın uygulama fabrikasından bir uygulama örneği istediğinizde, doğru uygulama nesnesi otomatik olarak sağlanır.

Aşağıdaki nesne modeli, alabileceğiniz nesne türünü ve paylaşılan bir aygıt bağlamında ne anlama geldiğini gösterir:

![kamu istemci uygulaması miras modeli](media/v2-shared-device-mode/ipublic-client-app-inheritance.png)

Nesnenizi `PublicClientApplication` aldığınızda bir tür denetimi yapmanız ve uygun arabirime döküm yapmanız gerekir. Aşağıdaki kod birden çok hesap modu nu veya tek hesap modunu denetler ve uygulama nesnesini uygun şekilde atar:

```java
private IPublicClientApplication mApplication;

        // Running in personal-device mode?
        if (mApplication instanceOf IMultipleAccountPublicClientApplication) {
          IMultipleAccountPublicClientApplication multipleAccountApplication = (IMultipleAccountPublicClientApplication) mApplication;
          ...
        // Running in shared-device mode?
        } else if (mApplication instanceOf ISingleAccountPublicClientApplication) {
           ISingleAccountPublicClientApplication singleAccountApplication = (ISingleAccountPublicClientApplication) mApplication;
            ...
        }
```

Uygulamanızın paylaşılan veya kişisel bir cihazda çalışıp çalışmadığına bağlı olarak aşağıdaki farklar geçerlidir:

|  | Paylaşılan mod aygıtı  | Kişisel cihaz |
|---------|---------|---------|
| **Hesaplar**     | Tek hesap | Birden çok hesap |
| **Oturum açma** | Genel | Genel |
| **Oturumu kapatma** | Genel | Her uygulama, oturum açmanın uygulama için veya uygulama ailesi için yerel olup olmadığını denetleyebilir. |
| **Desteklenen hesap türleri** | Yalnızca çalışma hesapları | Desteklenen kişisel ve iş hesapları  |

## <a name="why-you-may-want-to-only-support-single-account-mode"></a>Neden yalnızca tek hesap modunu desteklemek isteyebilirsiniz

Yalnızca paylaşılan bir aygıtı kullanan firstline çalışanları için kullanılacak bir uygulama yazıyorsanız, uygulamanızı yalnızca tek hesap lı modu desteklemek için yazmanızı öneririz. Buna, tıbbi kayıt uygulamaları, fatura uygulamaları ve çoğu iş yeri uygulaması gibi görev odaklı uygulamaların çoğu dahildir. Birden çok hesaplı uygulamaların parçası olan ek özellikleri uygulamanız gerekmeyeceğinden, yalnızca tek hesap lı modu desteklemek geliştirmeyi kolaylaştırır.

## <a name="what-happens-when-the-device-mode-changes"></a>Cihaz modu değiştiğinde ne olur?

Uygulamanız birden çok hesap modunda çalışıyorsa ve bir yönetici aygıtı paylaşılan aygıt moduna alıyorsa, aygıttaki tüm hesaplar uygulamadan temizlenir ve uygulama tek hesap moduna geçer.

## <a name="shared-device-sign-out-and-the-overall-app-lifecycle"></a>Paylaşılan cihaz oturum açma ve genel uygulama yaşam döngüsü

Bir kullanıcı buylayınca, kullanıcının gizliliğini ve verilerini korumak için harekete geçmeniz gerekir. Örneğin, bir tıbbi kayıt uygulaması oluşturuyorsanız, kullanıcı daha önce görüntülenen hasta kayıtlarının temizlendiğinden emin olmak istersiniz. Başvurunuz bunun için hazırlanmalı ve her ön plana her girdiğinde kontrol edilmelidir.

Uygulamanız paylaşılan modda çalışan bir uygulamada kullanıcıyı oturum ayarı yapmak için MSAL'ı kullandığında, oturum açmış hesap ve önbelleğe alınmış belirteçler hem uygulamadan hem de cihazdan kaldırılır.

Aşağıdaki diyagram, genel uygulama yaşam döngüsünü ve uygulamanız çalışırken oluşabilecek yaygın olayları gösterir. Diyagram, bir etkinliğin başlatılabı, oturum açma ve imzalama ve imzalama ve etkinliği duraklatma, sürdürme ve durdurma gibi olayların nasıl uygun olduğunu kapsar.

![Paylaşılan cihaz uygulaması yaşam döngüsü](media/v2-shared-device-mode/lifecycle.png)

## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan moddaki bir Android cihazda ilk satırda çalışan bir uygulamanın nasıl çalıştırılabildiğini gösteren [Android uygulama öğreticinizde paylaşılan aygıtı kullan modunu](tutorial-v2-shared-device-mode.md) deneyin.
