---
title: Azure AD Connect-geçişli kimlik doğrulaması-kimlik doğrulama aracılarını yükseltme | Microsoft Docs
description: Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulama yapılandırmanızın nasıl yükseltileceği açıklanır.
services: active-directory
keywords: Azure AD Connect geçişli kimlik doğrulaması, Active Directory yüklemesi, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: d27018a19db85e8544029db4f1b638ef7cae448a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85358149"
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Azure Active Directory geçişli kimlik doğrulaması: yükseltme önizlemesi kimlik doğrulama aracıları

## <a name="overview"></a>Genel Bakış

Bu makale, Önizleme aracılığıyla Azure AD geçişli kimlik doğrulaması kullanan müşteriler içindir. Son zamanlarda kimlik doğrulama Aracısı yazılımını yükselttik (ve yeniden markalı). Şirket içi sunucularınızda yüklü Önizleme kimlik doğrulama aracılarını _el ile_ yükseltmeniz gerekir. Bu el ile yükseltme yalnızca tek seferlik bir eylemdir. Kimlik doğrulama aracılarında gelecekteki tüm güncelleştirmeler otomatiktir. Yükseltme nedenleri şunlardır:

- Kimlik doğrulama aracılarının önizleme sürümleri, başka güvenlik veya hata düzeltmeleri almaz.
-   Kimlik doğrulama aracılarının önizleme sürümleri, yüksek kullanılabilirlik için ek sunuculara yüklenemez.

## <a name="check-versions-of-your-authentication-agents"></a>Kimlik doğrulama aracılarınızın sürümlerini denetleyin

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>1. Adım: kimlik doğrulama aracılarınızın nereye yüklendiğini denetleyin

Kimlik doğrulama aracılarınızın nereye yüklendiğini denetlemek için şu adımları izleyin:

1. [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgileriyle oturum açın.
2. Sol gezinti çubuğunda **Azure Active Directory** ' yi seçin.
3. **Azure AD Connect**seçin. 
4. **Doğrudan kimlik doğrulaması**' nı seçin. Bu dikey pencere, kimlik doğrulama aracılarınızın yüklendiği sunucuları listeler.

![Azure Active Directory Yönetim Merkezi-geçişli kimlik doğrulama dikey penceresi](./media/how-to-connect-pta-upgrade-preview-authentication-agents/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>2. Adım: kimlik doğrulama aracılarınızın sürümlerini denetleyin

Kimlik doğrulama aracılarınızın sürümlerini, önceki adımda tanımlanan her bir sunucuda denetlemek için şu yönergeleri izleyin:

1. Şirket içi sunucudaki **Denetim Masası-> programlar-> programlar ve Özellikler** ' e gidin.
2. "**Microsoft Azure AD Connect Authentication Agent**" için bir giriş varsa, bu sunucuda herhangi bir işlem yapmanız gerekmez.
3. "**Microsoft Azure AD uygulama proxy Bağlayıcısı**" için bir giriş varsa, bu sunucuda el ile yükseltmeniz gerekir.

![Kimlik doğrulama aracısının önizleme sürümü](./media/how-to-connect-pta-upgrade-preview-authentication-agents/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Yükseltmeye başlamadan önce izlenecek en iyi uygulamalar

Yükseltmeden önce, aşağıdaki öğelerin yerinde olduğundan emin olun:

1. **Yalnızca bulutta bulunan genel yönetici hesabı oluştur**: doğrudan kimlik doğrulama aracılarınızın düzgün şekilde çalışmadığını acil durumlarda kullanmak üzere yalnızca bulut genel yönetici hesabına sahip olmadan yükseltme yapmayın. [Yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımın yapılması kritik bir öneme sahiptir ve kiracınızdan kilitlenmemesini sağlar.
2.  **Yüksek kullanılabilirlik sağlayın**: daha önce tamamlanmazsa, bu [yönergeleri](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)kullanarak, oturum açma istekleri için yüksek kullanılabilirlik sağlamak üzere Ikinci bir tek başına kimlik doğrulama Aracısı yükler.

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Azure AD Connect sunucunuzdaki kimlik doğrulama aracısını yükseltme

Kimlik Doğrulama aracısını aynı sunucuda yükseltmeden önce yükseltme Azure AD Connect gerekir. Hem birincil hem de hazırlama Azure AD Connect sunucularınızda bu adımları izleyin:

1. **Azure AD Connect yükseltin**: Bu [makaleyi](how-to-upgrade-previous-version.md) izleyin ve en son Azure AD Connect sürümüne yükseltin.
2. **Kimlik doğrulama aracısının önizleme sürümünü kaldırın**: [Bu PowerShell betiğini](https://aka.ms/rmpreviewagent) indirip sunucuda yönetici olarak çalıştırın.
3. **Kimlik doğrulama aracısının en son sürümünü indirin (sürümler 1.5.389.0 veya üzeri)**: [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgileriyle oturum açın. **Azure Active Directory-> Azure AD Connect-> geçişli kimlik doğrulaması-> indirme Aracısı**' nı seçin. [Hizmet koşullarını](https://aka.ms/authagenteula) kabul edin ve kimlik doğrulama aracısının en son sürümünü indirin. Kimlik Doğrulama aracısını [buradan](https://aka.ms/getauthagent)da indirebilirsiniz.
4. **Kimlik doğrulama aracısının en son sürümünü yükler**: adım 3 ' te indirilen yürütülebilir dosyayı çalıştırın. İstendiğinde kiracınızın genel yönetici kimlik bilgilerini sağlayın.
5. **En son sürümün yüklendiğini doğrulayın**: daha önce gösterildiği gibi, **denetim masası-> programlar-> programlar ve Özellikler** ' e gidin ve "**Microsoft Azure AD Connect Authentication Aracısı**" için bir giriş olduğunu doğrulayın.

>[!NOTE]
>Yukarıdaki adımları tamamladıktan sonra [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) doğrudan kimlik doğrulama dikey penceresini denetledikten sonra sunucu başına Iki kimlik doğrulama aracı girişi görürsünüz. kimlik doğrulama aracısını **etkin** ve diğeri **devre dışı**olarak gösteren bir girdi. Bu _beklenen_bir değer. **Etkin olmayan** giriş birkaç gün sonra otomatik olarak bırakılır.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Diğer sunuculardaki kimlik doğrulama aracısını yükseltme

Diğer sunuculardaki (Azure AD Connect yüklü değil) kimlik doğrulama aracılarını yükseltmek için aşağıdaki adımları izleyin:

1. **Kimlik doğrulama aracısının önizleme sürümünü kaldırın**: [Bu PowerShell betiğini](https://aka.ms/rmpreviewagent) indirip sunucuda yönetici olarak çalıştırın.
2. **Kimlik doğrulama aracısının en son sürümünü indirin (sürümler 1.5.389.0 veya üzeri)**: [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgileriyle oturum açın. **Azure Active Directory-> Azure AD Connect-> geçişli kimlik doğrulaması-> indirme Aracısı**' nı seçin. Hizmet koşullarını kabul edin ve en son sürümü indirin.
3. **Kimlik doğrulama aracısının en son sürümünü yükler**: adım 2 ' de indirilen yürütülebilir dosyayı çalıştırın. İstendiğinde kiracınızın genel yönetici kimlik bilgilerini sağlayın.
4. **En son sürümün yüklendiğini doğrulayın**: daha önce gösterildiği gibi, **denetim masası-> programlar-> programlar ve Özellikler** ' e gidin ve **Microsoft Azure AD Connect Authentication Aracısı**adlı bir giriş olduğunu doğrulayın.

>[!NOTE]
>Yukarıdaki adımları tamamladıktan sonra [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) doğrudan kimlik doğrulama dikey penceresini denetledikten sonra sunucu başına Iki kimlik doğrulama aracı girişi görürsünüz. kimlik doğrulama aracısını **etkin** ve diğeri **devre dışı**olarak gösteren bir girdi. Bu _beklenen_bir değer. **Etkin olmayan** giriş birkaç gün sonra otomatik olarak bırakılır.

## <a name="next-steps"></a>Sonraki adımlar
- [**Sorun giderme**](tshoot-connect-pass-through-authentication.md) -özellikle ilgili yaygın sorunları çözmeyi öğrenin.
