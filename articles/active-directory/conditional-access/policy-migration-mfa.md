---
title: Koşullu erişim ilkelerini geçirme-Azure Active Directory
description: Bu makalede, Azure portal çok faktörlü kimlik doğrulaması gerektiren klasik bir ilkenin nasıl geçirileceği gösterilmektedir.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: tutorial
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca00ae62ba114aecef48117fd8a54b7f2e962dfd
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74380319"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Azure portal çok faktörlü kimlik doğrulaması gerektiren klasik bir ilkeyi geçirme

Bu öğreticide, bir bulut uygulaması için **çok faktörlü kimlik doğrulaması** gerektiren klasik bir ilkenin nasıl geçirileceği gösterilmektedir. Bir önkoşul olmamasına karşın, klasik ilkelerinizi geçirmeye başlamadan önce [Azure Portal klasik Ilkeleri geçirmeyi](policy-migration.md) okumanız önerilir.

## <a name="overview"></a>Genel Bakış

Bu makaledeki senaryo, bir bulut uygulaması için **çok faktörlü kimlik doğrulaması** gerektiren klasik bir ilkenin nasıl geçirilmesi gerektiğini gösterir.

![Azure Active Directory](./media/policy-migration/33.png)

Geçiş işlemi aşağıdaki adımlardan oluşur:

1. Yapılandırma ayarlarını almak için [Klasik Ilkeyi açın](#open-a-classic-policy) .
1. Klasik ilkenizi değiştirmek için yeni bir Azure AD koşullu erişim ilkesi oluşturun. 
1. Klasik ilkeyi devre dışı bırakın.

## <a name="open-a-classic-policy"></a>Klasik bir ilke açın

1. [Azure Portal](https://portal.azure.com)sol gezinti çubuğunda **Azure Active Directory**' e tıklayın.

   ![Azure Active Directory](./media/policy-migration-mfa/01.png)

1. **Azure Active Directory** sayfasında, **Yönet** bölümünde, **koşullu erişim**' e tıklayın.

   ![Koşullu Erişim](./media/policy-migration-mfa/02.png)

1. **Yönet** bölümünde **Klasik ilkeler (Önizleme)** seçeneğine tıklayın.

   ![Klasik ilkeler](./media/policy-migration-mfa/12.png)

1. Klasik ilkeler listesinde, bir bulut uygulaması için **çok faktörlü kimlik doğrulaması** gerektiren ilkeye tıklayın.

   ![Klasik ilkeler](./media/policy-migration-mfa/13.png)

## <a name="create-a-new-conditional-access-policy"></a>Yeni bir koşullu erişim ilkesi oluşturma

1. [Azure Portal](https://portal.azure.com)sol gezinti çubuğunda **Azure Active Directory**' e tıklayın.

   ![Azure Active Directory](./media/policy-migration/01.png)

1. **Azure Active Directory** sayfasında, **Yönet** bölümünde, **koşullu erişim**' e tıklayın.

   ![Koşullu Erişim](./media/policy-migration/02.png)

1. **Koşullu erişim** sayfasında, **Yeni** sayfayı açmak Için üstteki araç çubuğundan **Ekle**' ye tıklayın.

   ![Koşullu Erişim](./media/policy-migration/03.png)

1. **Yeni** sayfada, **ad** metin kutusuna ilkeniz için bir ad yazın.

   ![Koşullu Erişim](./media/policy-migration/29.png)

1. **Atamalar** bölümünde **Kullanıcılar ve gruplar**' a tıklayın.

   ![Koşullu Erişim](./media/policy-migration/05.png)

   1. Klasik ilkenizde tüm kullanıcılar ' ı seçtiyseniz, **tüm kullanıcılar**' a tıklayın. 

      ![Koşullu Erişim](./media/policy-migration/35.png)

   1. Klasik ilkenizde seçili gruplarınız varsa, **Kullanıcı ve Grup Seç**' e tıklayın ve ardından gerekli kullanıcılar ve gruplar ' ı seçin.

      ![Koşullu Erişim](./media/policy-migration/36.png)

   1. Dışlanan gruplarınız varsa, **Dışla** sekmesine tıklayın ve ardından gerekli kullanıcıları ve grupları seçin. 

      ![Koşullu Erişim](./media/policy-migration/37.png)

1. **Yeni** sayfada, **bulut uygulamaları** sayfasını açmak için, **atama** bölümünde, **bulut uygulamaları**' na tıklayın.
1. **Bulut uygulamaları** sayfasında, aşağıdaki adımları uygulayın:
   1. **Uygulama Seç**' e tıklayın.
   1. **Seç**'e tıklayın.
   1. **Seç** sayfasında, bulut uygulamanızı seçin ve ardından **Seç**' e tıklayın.
   1. **Bulut uygulamaları** sayfasında **bitti**' ye tıklayın.
1. **Multi-Factor Authentication** ' ın seçili olmasını istiyorsanız:

   ![Koşullu Erişim](./media/policy-migration/26.png)

   1. **Erişim denetimleri** bölümünde, **izin ver**' e tıklayın.

      ![Koşullu Erişim](./media/policy-migration/27.png)

   1. **İzin** sayfasında, **erişim ver**' e tıklayın ve ardından **çok faktörlü kimlik doğrulaması iste**' ye tıklayın.
   1. **Seç**'e tıklayın.
1. İlkenizi etkinleştirmek için **Açık** ' a tıklayın.

   ![Koşullu Erişim](./media/policy-migration/30.png)

## <a name="disable-the-classic-policy"></a>Klasik ilkeyi devre dışı bırak

Klasik ilkenizi devre dışı bırakmak için **Ayrıntılar** görünümünde **devre dışı bırak** ' a tıklayın.

![Klasik ilkeler](./media/policy-migration-mfa/14.png)

## <a name="next-steps"></a>Sonraki adımlar

- Klasik ilke geçişi hakkında daha fazla bilgi için, [Azure Portal klasik Ilkeleri geçirme](policy-migration.md)bölümüne bakın.
- Koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenmek isterseniz bkz. [koşullu erişim Azure Active Directory belirli uygulamalar IÇIN MFA gerektirme](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız, [Azure Active Directory Koşullu erişim için en iyi yöntemlere](best-practices.md)bakın.
