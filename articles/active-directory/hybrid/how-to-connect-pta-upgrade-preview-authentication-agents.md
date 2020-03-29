---
title: Azure AD Connect - Geçiş Kimlik Doğrulaması - Auth aracılarını yükseltme | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulama yapılandırmanızı nasıl yükseltiler.
services: active-directory
keywords: Azure AD Connect Pass-through Authentication, yükleme Active Directory, Azure AD, SSO, Tek Oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 494ccc3b90b8c249ee935087dcf0f0b5264b02ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60386780"
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory Pass-through Authentication: Yükseltme önizleme Kimlik Doğrulama Aracıları

## <a name="overview"></a>Genel Bakış

Bu makale, önizleme yoluyla Azure AD Geçiş Kimlik Doğrulaması kullanan müşteriler içindir. Kimlik Doğrulama Aracısı yazılımını yakın zamanda yükselttik (ve yeniden markaladık). Şirket içi sunucularınıza yüklenen önizleme Kimlik Doğrulama Aracılarını _el ile_ yükseltmeniz gerekir. Bu el ile yükseltme yalnızca tek seferlik bir eylemdir. Kimlik Doğrulama Aracıları için gelecekteki tüm güncelleştirmeler otomatiktir. Yükseltme nedenleri şunlardır:

- Kimlik Doğrulama Aracıları önizleme sürümleri başka güvenlik veya hata düzeltmeleri almaz.
-   Kimlik Doğrulama Aracılarının önizleme sürümleri, yüksek kullanılabilirlik için ek sunuculara yüklenemez.

## <a name="check-versions-of-your-authentication-agents"></a>Kimlik Doğrulama Aracılarınızın sürümlerini kontrol edin

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Adım 1: Kimlik Doğrulama Aracılarınızın nerede yüklü olduğunu kontrol edin

Kimlik Doğrulama Aracılarınızın nerede yüklendiğinden kontrol etmek için aşağıdaki adımları izleyin:

1. Kiracınız için Global Administrator kimlik bilgilerini içeren [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
2. Sol navigasyonda **Azure Etkin Dizini'ni** seçin.
3. **Azure AD Connect'i**seçin. 
4. **Geçiş Kimlik Doğrulaması'nı**seçin. Bu bıçak, Kimlik Doğrulama Aracılarınızın yüklendiği sunucuları listeler.

![Azure Active Directory yönetici merkezi - Geçiş Kimlik Doğrulama bıçağı](./media/how-to-connect-pta-upgrade-preview-authentication-agents/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>Adım 2: Kimlik Doğrulama Aracılarınızın sürümlerini kontrol edin

Kimlik Doğrulama Aracılarınızın sürümlerini kontrol etmek için, önceki adımda tanımlanan her sunucuda aşağıdaki yönergeleri izleyin:

1. Şirket içi sunucuda **Denetim Paneli -> Programları -> Programlar a gidin.**
2. "**Microsoft Azure AD Connect Authentication Agent**" için bir giriş varsa, bu sunucuda herhangi bir işlem yapmanız gerekmez.
3. "**Microsoft Azure AD Application Proxy Proxy "** için bir giriş varsa, bu sunucuda el ile yükseltmeniz gerekir.

![Kimlik Doğrulama Aracısı önizleme sürümü](./media/how-to-connect-pta-upgrade-preview-authentication-agents/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Yükseltmeye başlamadan önce izlenmeye n için en iyi uygulamalar

Yükseltmeden önce, aşağıdaki öğelerin yerinde olduğundan emin olun:

1. **Yalnızca buluta özel Global Administrator hesabı oluşturma**: Geçiş Kimlik Doğrulama Aracılarınızın düzgün çalışmadığı acil durumlarda kullanmak üzere yalnızca buluta özel Global Administrator hesabı olmadan yükseltme yapmayın. Yalnızca [buluta özel Global Administrator hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımı yapmak çok önemlidir ve kiracınızın dışında kalmamanızı sağlar.
2.  **Yüksek kullanılabilirlik sağlayın**: Daha önce tamamlanmadıysa, bu yönergeleri kullanarak oturum açma istekleri için yüksek kullanılabilirlik sağlamak için ikinci bir bağımsız Kimlik Doğrulama [Aracısı yükleyin.](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Azure AD Connect sunucunuzdaki Kimlik Doğrulama Aracısını Yükseltme

Kimlik Doğrulama Aracısını aynı sunucuda yükseltmeden önce Azure AD Connect'i yükseltmeniz gerekir. Hem birincil hem de azure AD Connect sunucularınızda aşağıdaki adımları izleyin:

1. **Azure AD Connect'i Yükseltme**: Bu [makaleyi](how-to-upgrade-previous-version.md) izleyin ve en son Azure AD Connect sürümüne yükseltin.
2. **Kimlik Doğrulama Aracısı'nın önizleme sürümünü kaldırın**: [Bu PowerShell komut dosyasını](https://aka.ms/rmpreviewagent) indirin ve sunucuda Yönetici olarak çalıştırın.
3. **Kimlik Doğrulama Aracısı'nın en son sürümünü indirin (sürüm 1.5.389.0 veya sonraki sürümler)**: Kiracınızın Genel Yönetici kimlik bilgileriyle [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) oturum açın. **Azure Etkin Dizini -> Azure AD Bağlantısı -> Geçiş Kimlik Doğrulaması -> İndir aracısını**seçin. Hizmet koşullarını kabul [edin](https://aka.ms/authagenteula) ve Kimlik Doğrulama Aracısı'nın en son sürümünü indirin. Kimlik Doğrulama Aracısını buradan [here](https://aka.ms/getauthagent)da indirebilirsiniz.
4. **Kimlik Doğrulama Aracısı'nın en son sürümünü yükleyin**: Adım 3'te indirilen çalıştırılabilir sürümü çalıştırın. İstendiğinde kiracınızın Global Administrator kimlik bilgilerini sağlayın.
5. **En son sürümün yüklendiğini doğrulayın**: Daha önce gösterildiği **gibi, Programları ve Özellikleri > > Denetim Masası'na** gidin ve "**Microsoft Azure AD Connect Authentication Agent**" için bir giriş olduğunu doğrulayın.

>[!NOTE]
>Önceki adımları tamamladıktan sonra [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) Geçiş Kimlik Doğrulama bıçağını denetlerseniz, sunucu başına iki Kimlik Doğrulama Aracısı girişi görürsünüz - biri Kimlik Doğrulama Aracısını **Etkin,** diğeri **Etkin Olmayan**olarak. Bu _bekleniyor._ **Etkin olmayan** giriş birkaç gün sonra otomatik olarak bırakılır.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Diğer sunucularda Kimlik Doğrulama Aracısını Yükseltme

Diğer sunucularda (Azure AD Connect'in yüklü olmadığı durumlarda) Kimlik Doğrulama Aracılarını yükseltmek için aşağıdaki adımları izleyin:

1. **Kimlik Doğrulama Aracısı'nın önizleme sürümünü kaldırın**: [Bu PowerShell komut dosyasını](https://aka.ms/rmpreviewagent) indirin ve sunucuda Yönetici olarak çalıştırın.
2. **Kimlik Doğrulama Aracısı'nın en son sürümünü indirin (sürüm 1.5.389.0 veya sonraki sürümler)**: Kiracınızın Genel Yönetici kimlik bilgileriyle [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) oturum açın. **Azure Etkin Dizini -> Azure AD Bağlantısı -> Geçiş Kimlik Doğrulaması -> İndir aracısını**seçin. Hizmet koşullarını kabul edin ve en son sürümü indirin.
3. **Kimlik Doğrulama Aracısı'nın en son sürümünü yükleyin**: Adım 2'de indirilen çalıştırılabilir sürümü çalıştırın. İstendiğinde kiracınızın Global Administrator kimlik bilgilerini sağlayın.
4. **En son sürümün yüklendiğini doğrulayın**: Daha önce gösterildiği gibi, **Programları ve Özellikleri > Denetim > Masası'na** gidin ve Microsoft Azure AD Connect **Authentication Agent**adında bir giriş olduğunu doğrulayın.

>[!NOTE]
>Önceki adımları tamamladıktan sonra [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) Geçiş Kimlik Doğrulama bıçağını denetlerseniz, sunucu başına iki Kimlik Doğrulama Aracısı girişi görürsünüz - biri Kimlik Doğrulama Aracısını **Etkin,** diğeri **Etkin Olmayan**olarak. Bu _bekleniyor._ **Etkin olmayan** giriş birkaç gün sonra otomatik olarak bırakılır.

## <a name="next-steps"></a>Sonraki adımlar
- [**Sorun Giderme**](tshoot-connect-pass-through-authentication.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
