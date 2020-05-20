---
title: Azure geçişini bir ISV aracı/Movere ile çalışacak şekilde hazırlama
description: Bu makalede, Azure geçişi 'nin bir ISV aracı veya Movere ile çalışacak şekilde nasıl hazırlanacağı ve sonra aracın nasıl kullanılacağına nasıl başlayabileceğiniz açıklanır.
ms.topic: how-to
ms.date: 05/07/2020
ms.openlocfilehash: 9513e783d4f9d7be83f1434d4dd9011844af8993
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682657"
---
# <a name="prepare-to-work-with-an-isv-tool-or-movere"></a>ISV aracıyla veya Movere ile çalışmaya hazırlanma

Bir Azure geçişi projesine bir [ISV aracı](migrate-services-overview.md#isv-integration) veya Movere eklediyseniz, aracı paylaşmadan ve Azure geçişi 'ne veri gönderebilmeniz için izlemeniz gereken birkaç adım vardır. 

## <a name="check-azure-ad-permissions"></a>Azure AD izinlerini denetle

Azure Kullanıcı hesabınız bu izinlere ihtiyaç duyuyor:

- Azure kiracınızla Azure Active Directory (Azure AD) uygulaması kaydetme izni
- Abonelik düzeyinde Azure AD uygulamasına bir rol ayırma izni


### <a name="set-permissions-to-register-an-azure-ad-app"></a>Azure AD uygulaması kaydetme izinlerini ayarlama

1. Azure AD 'de hesabınızın rolünü kontrol edin.
2. Kullanıcı rolüne sahipseniz, sol taraftaki **Kullanıcı ayarları** ' nı seçin ve kullanıcıların uygulamaları kaydedip edemeyeceğini doğrulayın. **Evet**olarak ayarlanırsa, Azure AD kiracısındaki tüm kullanıcılar bir uygulamayı kaydedebilir. **Hayır**olarak ayarlanırsa, uygulamaları yalnızca yönetici kullanıcılar kaydedebilir.   
3. İzinleriniz yoksa, Yönetici Kullanıcı Kullanıcı hesabınıza [Uygulama Yöneticisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-administrator) rolünü sağlayabilir, böylece uygulamayı kaydedebilirsiniz.
4. Araç Azure geçişi ile bağlandıktan sonra, yönetici hesabınızdan rolü kaldırabilir.

### <a name="set-permissions-to-assign-a-role-to-an-azure-ad-app"></a>Azure AD uygulamasına rol atama izinlerini ayarlama
 
Azure aboneliğinizde hesabınız, Azure AD uygulamasına bir rol atamak için **Microsoft. Authorization/*/Write** erişimi gerektirir. 

1. Azure portal **abonelikler**' i açın.
2. Uygun aboneliği seçin. Bunu görmüyorsanız **genel abonelikler filtresini**seçin. 
3. **Izinlerim**' i seçin. Ardından, **bu aboneliğin tüm erişim ayrıntılarını görüntülemek için buraya tıklayın '** ı seçin.
4. **Rol atamaları**  >  **görünümünde**, izinleri denetleyin. Hesabınızın izinleri yoksa, abonelik yöneticisinden sizi [Kullanıcı erişimi Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) rolüne veya [sahip](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rolüne eklemesini isteyin.
 

## <a name="start-using-the-tool"></a>Aracı kullanmaya başlayın

1. Araç için henüz bir lisansa veya ücretsiz deneme sürümü yoksa, **Kaydet**bölümünde, **daha fazla bilgi**için Azure geçişi ' ni seçin.
2. Araçta, aracından Azure geçişi projesine bağlantı sağlamak ve Azure geçişi 'ne veri göndermek için yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi 'ne veri göndermek için ISV veya Movere 'inizdeki yönergeleri izleyin.

   
