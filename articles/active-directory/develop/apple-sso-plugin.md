---
title: Apple cihazlar için Microsoft Enterprise SSO eklentisi
titleSuffix: Microsoft identity platform | Azure
description: İOS ve macOS cihazları için Microsoft 'un Azure Active Directory SSO eklentisi hakkında bilgi edinin.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/31/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: a6af1a78582ebf62aed82bad8fa06cac37f511ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80982591"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Apple cihazları için Microsoft Enterprise SSO eklentisi (Önizleme)

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

*Apple cihazları Için Microsoft ENTERPRISE SSO eklentisi* , Apple 'ın [Kurumsal Çoklu oturum açma](https://developer.apple.com/documentation/authenticationservices) özelliğini destekleyen tüm uygulamalarda Azure Active Directory (Azure AD) hesapları için çoklu oturum açma (SSO) sağlar. Microsoft, Apple ve Microsoft 'un sağlayabilecekleri en iyi korumayı sağlarken uygulamanızın kullanılabilirliğini artırmak üzere bu eklentiyi geliştirmek üzere Apple ile yakın bir şekilde çalışmıştır.

Bu genel önizleme sürümünde, Enterprise SSO eklentisi yalnızca iOS cihazlarında kullanılabilir ve belirli Microsoft uygulamalarında dağıtılır.

Kurumsal SSO eklentisinin ilk kullanımı, yeni [paylaşılan cihaz modu](msal-ios-shared-devices.md) özelliğimizi kullanıyor.

## <a name="features"></a>Özellikler

Apple cihazları için Microsoft Enterprise SSO eklentisi aşağıdaki avantajları sunar:

- Apple 'ın Kurumsal Çoklu oturum açma özelliğini destekleyen tüm uygulamalarda Azure AD hesapları için SSO sağlar.
- Microsoft Authenticator otomatik olarak teslim edilebilir ve herhangi bir mobil cihaz yönetimi (MDM) çözümü tarafından etkinleştirilebilir.

## <a name="requirements"></a>Gereksinimler

Apple cihazları için Microsoft Enterprise SSO eklentisini kullanmak için:

- cihazda iOS 13,0 veya üzeri yüklü olmalıdır.
- Apple cihazları için Microsoft Enterprise SSO eklentisini sağlayan bir Microsoft uygulamasının cihazda yüklü olması gerekir. Genel önizleme için bu uygulamalar [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md)içerir.
- Cihazın MDM ile kayıtlı olması gerekir (örneğin, Microsoft Intune ile).
- Cihazdaki Apple cihazları için Microsoft Enterprise SSO eklentisini etkinleştirmek üzere cihaza bir yapılandırma itilmiş olmalıdır. Bu güvenlik kısıtlaması Apple tarafından gerektirilir.

## <a name="enable-the-sso-extension-with-mobile-device-management-mdm"></a>Mobil cihaz yönetimi (MDM) ile SSO uzantısını etkinleştirme

Apple cihazları için Microsoft Enterprise SSO eklentisini etkinleştirmek üzere, cihazlarınızın MDM hizmeti aracılığıyla bir sinyal gönderilmesi gerekir. Microsoft, [Microsoft Authenticator](..//user-help/user-help-auth-app-overview.md)UYGULAMASıNDAKI Kurumsal SSO eklentisini de Içerdiğinden, MICROSOFT Enterprise SSO eklentisini etkinleştirmek üzere uygulamayı YAPıLANDıRMAK için MDM 'nizi kullanın.

Apple cihazları için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın:

- **Tür**: Redirect
- **UZANTı kimliği**:`com.microsoft.azureauthenticator.ssoextension`
- **Takım Kimliği**:`SGGM6D27TK`
- **URL 'ler**:
  - `https://login.microsoftonline.com`
  - `https://login.windows.net`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`

Microsoft Enterprise SSO eklentisinin yapılandırılmasını kolaylaştırmak için MDM hizmetiniz olarak Microsoft Intune kullanabilirsiniz. Daha fazla bilgi için bkz. [Intune yapılandırma belgeleri](https://docs.microsoft.com/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-extension-in-your-application"></a>Uygulamanızda SSO uzantısını kullanma

[Apple cihazları Için Microsoft kimlik doğrulama kitaplığı (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-objc) sürüm 1.1.0 ve üzeri, Apple cihazları Için MICROSOFT Enterprise SSO eklentisini destekler.

Apple cihazları için Microsoft Enterprise SSO eklentisi tarafından sağlanmış olan paylaşılan cihaz modunu desteklemek istiyorsanız, uygulamalarınızın belirtilen en düşük gerekli MSAL sürümünü kullandığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

İOS 'ta paylaşılan cihaz modu hakkında daha fazla bilgi için bkz. [iOS cihazları Için paylaşılan cihaz modu](msal-ios-shared-devices.md).
