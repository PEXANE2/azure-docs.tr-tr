---
title: Azure Çok Faktörlü Auth Sağlayıcıları - Azure Etkin Dizini
description: Azure MFA'lı bir Auth Sağlayıcısı'nı ne zaman kullanmalısınız?
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
ms.openlocfilehash: bf50a8f58978a010fe3d8228ace8579fcf52eb38
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309896"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Azure Çok Faktörlü Kimlik Doğrulama Sağlayıcısı ne zaman kullanılır?

> [!IMPORTANT]
> 1 Eylül 2018 tarihinden itibaren yeni auth sağlayıcıları oluşturulamaz. Varolan auth sağlayıcıları kullanılmaya ve güncelliğe devam edebilir, ancak geçiş artık mümkün değildir. Çok faktörlü kimlik doğrulama, Azure AD Premium lisanslarında bir özellik olarak kullanılabilir durumda olmaya devam edecektir.

İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](howto-mfa-mfasettings.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Azure Çok Faktörlü Auth Sağlayıcısı, **lisansı olmayan**kullanıcılar için Azure Çok Faktörlü Kimlik Doğrulama tarafından sağlanan özelliklerden yararlanmak için kullanılır.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Azure MFA SDK ile ilgili uyarılar

SDK'nın amortismana alındığını ve sadece 14 Kasım 2018 tarihine kadar çalışmaya devam edeceğini unutmayın. O tarihten sonra SDK çağrıları başarısız olacaktır.

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

İki tür Auth sağlayıcısı vardır ve bu ayrım Azure aboneliğinizin nasıl ücretlendirildiğini içerir. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="manage-your-mfa-provider"></a>MFA Sağlayıcınızı yönetme

Bir MFA sağlayıcısı oluşturulduktan sonra kullanım modelini (etkin kullanıcı başına veya kimlik doğrulaması başına) değiştiremezsiniz.

MFA için etkinleştirilen tüm kullanıcıları kapsayacak kadar lisans satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, varolan Azure MFA Sunucuları'nın MFA Sağlayıcısı tarafından oluşturulan etkinleştirme kimlik bilgilerini kullanarak yeniden etkinleştirilmesi gerekir.

### <a name="removing-an-authentication-provider"></a>Kimlik doğrulama sağlayıcısını kaldırma

> [!CAUTION]
> Kimlik doğrulama sağlayıcısı nı silerken onay yoktur. **Sil'i** seçmek kalıcı bir işlemdir.

Kimlik doğrulama sağlayıcıları Azure **portalı** > **Azure Active Directory** > **Security** > **MFA** > **Sağlayıcıları'nda**bulunabilir. Bu sağlayıcıyla ilişkili ayrıntıları ve yapılandırmaları görmek için listelenen sağlayıcıları tıklatın.

Kimlik doğrulama sağlayıcısını kaldırmadan önce, sağlayıcınızda yapılandırılan özelleştirilmiş ayarları dikkate alın. Sağlayıcınızdan genel MFA ayarlarına hangi ayarların geçirilmesi gerektiğine karar verin ve bu ayarların geçişini tamamlayın. 

Sağlayıcılara bağlı Azure MFA Sunucuları'nın Azure **portalı** > **Azure Active Directory** > **Security** > **MFA** > **Server ayarları**altında oluşturulan kimlik bilgilerini kullanarak yeniden etkinleştirilmesi gerekir. Reavating önce, aşağıdaki dosyaları ortamınızda `\Program Files\Multi-Factor Authentication Server\Data\` Azure MFA Sunucular dizini silinmelidir:

- caCert
- cert
- groupCACert
- grupKey
- grupAdı
- lisansAnahtar
- Pkey

![Azure portalından bir auth sağlayıcısını silme](./media/concept-mfa-authprovider/authentication-provider-removal.png)

Tüm ayarların geçirildiğini doğruladığınızda, **Azure portalı** > **Azure Active Directory** > **Security** > **MFA** > **Sağlayıcıları'na** göz atabilir ve elipsleri seçebilir ve **Sil'i**seçebilirsiniz. **...**

> [!WARNING]
> Kimlik doğrulama sağlayıcısının silinmesiyle ilgili raporlama bilgileri bu sağlayıcıyla ilişkili tüm bilgileri siler. Sağlayıcınızı silmeden önce etkinlik raporlarını kaydetmek isteyebilirsiniz.

> [!NOTE]
> Microsoft Authenticator uygulamasının ve Azure MFA Server'ın eski sürümlerine sahip kullanıcıların uygulamalarını yeniden kaydetmeleri gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)
