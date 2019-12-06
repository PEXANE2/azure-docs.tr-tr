---
title: Azure Multi-Factor auth sağlayıcıları-Azure Active Directory
description: Azure MFA ile bir kimlik doğrulama sağlayıcısı ne zaman kullanılmalıdır?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57ac23bbb0465be2f15e0a35bb4fd7c331e83988
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848723"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Azure Multi-Factor Authentication sağlayıcısı ne zaman kullanılır?

İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](howto-mfa-mfasettings.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Azure Multi-Factor auth sağlayıcısı, **lisanslarına sahip olmayan**kullanıcılar için Azure Multi-Factor Authentication tarafından belirtilen özelliklerden faydalanmak için kullanılır.

> [!NOTE]
> 1 Eylül 2018 ' den itibaren geçerli olan yeni kimlik doğrulama sağlayıcıları artık oluşturulmayabilir. Mevcut kimlik doğrulama sağlayıcıları çalışmaya ve güncelleştirilmesine devam edebilir, ancak geçiş artık mümkün değildir. Multi-Factor Authentication, Azure AD Premium lisanlarında bir özellik olarak kullanılabilir olmaya devam edecektir.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Azure MFA SDK ile ilgili uyarılar

SDK kullanım dışı bırakılmıştır ve yalnızca 14 Kasım 2018 ' e kadar çalışmaya devam eder. O tarihten sonra SDK çağrıları başarısız olacaktır.

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

İki tür kimlik doğrulama sağlayıcısı vardır ve bu ayrım, Azure aboneliğinizin ücretlendirildiği yerlerdir. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="manage-your-mfa-provider"></a>MFA Sağlayıcınızı yönetme

Bir MFA sağlayıcısı oluşturulduktan sonra kullanım modelini (etkin kullanıcı başına veya kimlik doğrulaması başına) değiştiremezsiniz.

MFA için etkinleştirilen tüm kullanıcıları kapsayacak sayıda lisans satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, mevcut Azure MFA sunucularının MFA sağlayıcısı aracılığıyla oluşturulan etkinleştirme kimlik bilgileri kullanılarak yeniden etkinleştirilmesi gerekir. MFA sunucularını MFA sağlayıcısına bağlamak üzere yeniden etkinleştirmek, telefon araması ve kısa mesaj kimlik doğrulamasını etkilemez, ancak mobil uygulama bildirimleri mobil uygulamayı yeniden etkinleştirene kadar tüm kullanıcılar için çalışmayı durdurur.

### <a name="removing-an-authentication-provider"></a>Kimlik doğrulama sağlayıcısını kaldırma

> [!CAUTION]
> Bir kimlik doğrulama sağlayıcısı silinirken hiçbir onay yok. **Sil** seçildiğinde kalıcı bir işlemdir.

Kimlik doğrulama sağlayıcıları **Azure portal** > **Azure Active Directory** > **MFA** > **sağlayıcıları**'nda bulunabilir. Söz konusu sağlayıcıyla ilişkili ayrıntıları ve konfigürasyonları görmek için listelenen sağlayıcılar ' a tıklayın.

Bir kimlik doğrulama sağlayıcısını kaldırmadan önce, sağlayıcınızda yapılandırılmış tüm özelleştirilmiş ayarları göz önüne alın. Sağlayıcınızdan hangi ayarların genel MFA ayarlarına geçirilmesi gerektiğine ve bu ayarların geçişini tamamlamaya karar verin. 

Sağlayıcılara bağlı Azure MFA sunucularının, **MFA** > **sunucu ayarları** > **Azure Active Directory** **Azure Portal** > altında oluşturulan kimlik bilgileri kullanılarak yeniden etkinleştirilmesi gerekir. Yeniden etkinleştirmeden önce, aşağıdaki dosyaların ortamınızdaki Azure MFA sunucularındaki `\Program Files\Multi-Factor Authentication Server\Data\` dizininden silinmesi gerekir:

- caCert
- cert
- groupCACert
- groupKey
- adýdýr
- licenseKey
- PKEY

![Azure portal bir kimlik doğrulama sağlayıcısını silme](./media/concept-mfa-authprovider/authentication-provider-removal.png)

Tüm ayarların geçirildiğini onayladığınızda, **MFA** > **sağlayıcıları** > **Azure Active Directory** **Azure Portal** > ve üç nokta (.. **.** ) simgesini seçip **Sil**' i seçebilirsiniz.

> [!WARNING]
> Bir kimlik doğrulama sağlayıcısı silindiğinde, bu sağlayıcı ile ilişkili tüm raporlama bilgileri silinir. Sağlayıcınızı silmeden önce etkinlik raporlarını kaydetmek isteyebilirsiniz.

> [!NOTE]
> Microsoft Authenticator uygulamasının ve Azure MFA sunucusu 'nun eski sürümlerine sahip olan kullanıcıların, uygulamalarını yeniden kaydetmesi gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)
