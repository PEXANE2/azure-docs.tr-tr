---
title: Azure geçişi 'ni ISV araçları/Movere ile çalışmaya hazırlama
description: Azure geçişi 'ni ISV araçları/Movere ile çalışmaya hazırlama
ms.topic: how-to
ms.date: 05/07/2020
ms.openlocfilehash: b2ac95743e97bce41f38a4078e5e38f6b1bf7cd9
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82906988"
---
# <a name="prepare-to-work-with-isv-toolsmovere"></a>ISV araçları/Movere ile çalışmaya hazırlanma

Bir Azure geçişi projesine bir [ISV aracı](migrate-services-overview.md#isv-integration)veya Movere eklediyseniz, aracı paylaşmadan önce hazırlanmanız gereken birkaç adım vardır ve Azure geçişi 'ne veri gönderebilirsiniz. 

## <a name="check-azure-ad-permissions"></a>Azure AD izinlerini denetle

Azure Kullanıcı hesabınız birkaç izne ihtiyaç duyuyor:

- Azure kiracınızla bir Azure AD uygulaması kaydetme izni.
- Abonelik düzeyinde Azure AD uygulamasına bir rol ayırma izni.


## <a name="set-permissions-to-register-an-azure-ad-app"></a>Azure AD uygulaması kaydetme izinlerini ayarlama

1. Azure Active Directory, hesabınızın rolünü kontrol edin. Kullanıcı rolüne sahipseniz, sol taraftaki **Kullanıcı ayarları** ' na tıklayın ve kullanıcıların uygulamaları kaydedip kaydedemeyeceğini doğrulayın.
2. **Evet**olarak ayarlanırsa, Azure AD kiracısındaki tüm kullanıcılar bir uygulamayı kaydedebilir. Bu durumda, yalnızca yönetici kullanıcılar uygulamaları kaydedebilir.
3. İzinleriniz yoksa, Yönetici Kullanıcı Kullanıcı hesabınıza [Uygulama Yöneticisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-administrator) rolünü sağlayabilir, böylece uygulamayı kaydedebilirsiniz.
4. Araç Azure geçişi ile bağlandıktan sonra, yönetici hesabınızdan rolü kaldırabilir.

## <a name="set-permissions-to-assign-a-role-to-an-azure-ad-app"></a>Azure AD uygulamasına rol atama izinlerini ayarlama
 
Azure aboneliğinizde, Azure AD uygulamasına bir rol atamak için hesabınızın **Microsoft. Authorization/*/Write erişimi**olması gerekir. 

1. Abonelik izinlerini denetlemek için, Azure portal **abonelikler**' i açın.
2. Uygun aboneliği seçin. Bunu görmüyorsanız **genel abonelikler filtresini**seçin. 
3. **Izinlerim**' i seçin. Ardından, **bu aboneliğin tüm erişim ayrıntılarını görüntülemek için buraya tıklayın '** ı seçin.
4. **Rol atamaları** > **görünümünde**, izinleri denetleyin.
5. Hesabınızın izinleri yoksa, abonelik yöneticisinden sizi [Kullanıcı erişimi yönetici](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) rolüne veya [sahip](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rolüne eklemesini isteyin.
 

## <a name="start-using-the-tool"></a>Aracı kullanmaya başlayın

1. Araç için henüz bir lisansa veya ücretsiz deneme sürümü yoksa, **Kaydet**bölümünde, **daha fazla bilgi**' ye tıklayın.
2. Araçta, aracından Azure geçişi projesine bağlanmak ve Azure geçişi 'ne veri göndermek için yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi 'ne veri göndermek için ISV veya Movere yönergelerini izleyin.

   
