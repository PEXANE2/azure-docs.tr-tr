---
title: Apple cihazlar için Microsoft Enterprise SSO eklentisi
titleSuffix: Microsoft identity platform | Azure
description: iOS ve macOS aygıtları için Microsoft'un Azure Active Directory SSO eklentisi hakkında bilgi edinin.
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
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982591"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Apple aygıtları için Microsoft Enterprise SSO eklentisi (Önizleme)

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

*Apple aygıtları için Microsoft Enterprise SSO eklentisi,* Apple'ın [Kurumsal Tek Oturum](https://developer.apple.com/documentation/authenticationservices) Açma özelliğini destekleyen tüm uygulamalarda Azure Active Directory (Azure AD) hesapları için tek oturum açma (SSO) sağlar. Microsoft, apple ve Microsoft'un sağlayabileceği en iyi korumayı sağlarken uygulamanızın kullanılabilirliğini artırmak için bu eklentiyi geliştirmek için Apple ile yakın işbirliği içinde çalışmıştır.

Bu Genel Önizleme sürümünde, Enterprise SSO eklentisi yalnızca iOS aygıtları için kullanılabilir ve belirli Microsoft uygulamalarında dağıtılır.

Enterprise SSO eklentisini ilk kullanıma yeni [paylaşılan cihaz modu](msal-ios-shared-devices.md) özelliğimizle birlikte kullanıyoruz.

## <a name="features"></a>Özellikler

Apple cihazları için Microsoft Enterprise SSO eklentisi aşağıdaki avantajları sunar:

- Apple'ın Kurumsal Tek Oturum Açma özelliğini destekleyen tüm uygulamalarda Azure AD hesapları için SSO sağlar.
- Microsoft Authenticator'da otomatik olarak teslim edilir ve herhangi bir mobil cihaz yönetimi (MDM) çözümü tarafından etkinleştirilebilir.

## <a name="requirements"></a>Gereksinimler

Apple aygıtları için Microsoft Enterprise SSO eklentisini kullanmak için:

- iOS 13.0 veya üzeri aygıta yüklenmiş olmalıdır.
- Apple aygıtları için Microsoft Enterprise SSO eklentisini sağlayan bir Microsoft uygulamasının aygıta yüklenmesi gerekir. Genel Önizleme için bu uygulamalar [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md)içerir.
- Aygıt MDM'ye kayıtlı olmalıdır (örneğin, Microsoft Intune ile).
- Aygıttaki Apple aygıtları için Microsoft Enterprise SSO eklentisini etkinleştirmek için yapılandırmanın aygıta itilmesi gerekir. Bu güvenlik kısıtlaması Apple tarafından gereklidir.

## <a name="enable-the-sso-extension-with-mobile-device-management-mdm"></a>Mobil cihaz yönetimi (MDM) ile SSO uzantısını etkinleştirin

Apple aygıtları için Microsoft Enterprise SSO eklentisini etkinleştirmek için, cihazlarınızın bir MDM hizmeti aracılığıyla bir sinyal göndermesi gerekir. Microsoft, [Microsoft Authenticator uygulamasında](..//user-help/user-help-auth-app-overview.md)Enterprise SSO eklentisini içerdiğinden, Uygulamayı Microsoft Enterprise SSO eklentisini etkinleştirmek için yapılandırmak için MDM'nizi kullanın.

Apple aygıtları için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın:

- **Tür**: Yönlendirme
- **Uzatma Kimliği**:`com.microsoft.azureauthenticator.ssoextension`
- **Takım Kimliği**:`SGGM6D27TK`
- **URL'ler**:
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

Microsoft Enterprise SSO eklentisinin yapılandırmasını kolaylaştırmak için MDM hizmetiniz olarak Microsoft Intune'u kullanabilirsiniz. Daha fazla bilgi için [Intune yapılandırma belgelerine](https://docs.microsoft.com/intune/configuration/ios-device-features-settings)bakın.

## <a name="using-the-sso-extension-in-your-application"></a>Uygulamanızda SSO uzantısını kullanma

Apple aygıtları sürüm 1.1.0 ve üzeri [için Microsoft Kimlik Doğrulama Kitaplığı (MSAL),](https://github.com/AzureAD/microsoft-authentication-library-for-objc) Apple aygıtları için Microsoft Enterprise SSO eklentisini destekler.

Apple aygıtları için Microsoft Enterprise SSO eklentisi tarafından sağlanan paylaşılan aygıt modunu desteklemek istiyorsanız, uygulamalarınızın MSAL'ın belirtilen minimum gerekli sürümünü kullandığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

iOS'ta paylaşılan aygıt modu hakkında daha fazla bilgi için [iOS aygıtları için Paylaşılan aygıt moduna](msal-ios-shared-devices.md)bakın.
