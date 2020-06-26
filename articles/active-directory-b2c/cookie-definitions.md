---
title: Tanımlama bilgisi tanımları
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C ' de kullanılan tanımlama bilgileri için tanımlar sağlar.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/23/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5e44e2e1eb37e808e60134a6fba5051552e84029
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85389352"
---
# <a name="cookies-definitions-for-azure-ad-b2c"></a>Azure AD B2C için tanımlama bilgisi tanımları

Aşağıdaki bölümlerde Azure Active Directory B2C (Azure AD B2C) ' de kullanılan tanımlama bilgileri hakkında bilgi sağlanmaktadır.

## <a name="samesite"></a>SameSite

Microsoft Azure AD B2C hizmeti, özniteliğiyle destek de dahil olmak üzere, SameSite tarayıcı yapılandırmalarına sahiptir `SameSite=None` `Secure` .

Sitelere erişimi korumak için Web tarayıcıları, aksi belirtilmediği takdirde, tüm tanımlama bilgilerinin dış erişimden korunması gerektiğini varsayan, yeni bir güvenli varsayılan model ortaya çıkaracak. Chrome tarayıcısı, [şubat 2020 ' de chrome 80](https://www.chromium.org/updates/same-site)' den başlayarak bu değişikliği uygulayan ilk ilkdir. Chrome 'daki değişikliğe hazırlanma hakkında daha fazla bilgi için bkz [. geliştiriciler: yeni Sameget için hazırlanma = yok; Kmıum blogundan tanımlama bilgisi ayarlarını güvenli hale](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html) getirin.

Geliştiricilerin, `SameSite=None` siteler arası erişim tanımlama bilgilerini belirlemek için yeni tanımlama bilgisi ayarını kullanması gerekir. `SameSite=None`Öznitelik mevcut olduğunda, `Secure` siteler arası tanımlama BILGILERINE yalnızca HTTPS bağlantıları üzerinden erişilebilmeleri için ek bir özniteliğin kullanılması gerekir. Azure AD B2C kullanan uygulamalar da dahil olmak üzere tüm uygulamalarınızı doğrulayın ve test edin.

Daha fazla bilgi için bkz.

* [Chrome tarayıcısında SameSite tanımlama bilgisi değişikliklerini işleme](../active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser.md)
* [Chrome sürüm 80 veya sonraki sürümlerinde müşteri web siteleri ve Microsoft Hizmetleri ve ürünleri üzerindeki etki](https://support.microsoft.com/help/4522904/potential-disruption-to-customer-websites-in-latest-chrome)

## <a name="cookies"></a>Tanımlama bilgileri

Aşağıdaki tabloda Azure AD B2C ' de kullanılan tanımlama bilgileri listelenmektedir.

| Adı | Domain | Bitiş tarihi | Amaç |
| ----------- | ------ | -------------------------- | --------- |
| `x-ms-cpim-admin` | main.b2cadmin.ext.azure.com | [Tarayıcı oturumunun](session-behavior.md) sonu | Kiracılar genelinde Kullanıcı üyeliği verilerini tutar. Bir kullanıcının üyesi ve üyelik düzeyi (yönetici veya Kullanıcı) olan kiracılar. |
| `x-ms-cpim-slice` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | İstekleri uygun üretim örneğine yönlendirmek için kullanılır. |
| `x-ms-cpim-trans` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | İşlemleri (Azure AD B2C kimlik doğrulama isteklerinin sayısı) ve geçerli işlemi izlemek için kullanılır. |
| `x-ms-cpim-sso:{Id}` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | SSO oturumunun sürdürülmesi için kullanılır. |
| `x-ms-cpim-cache:{id}_n` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumu](session-behavior.md)sonu, başarılı kimlik doğrulama | İstek durumunu korumak için kullanılır. |
| `x-ms-cpim-csrf` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | CRSF koruması için kullanılan siteler arası Istek forgery belirteci. |
| `x-ms-cpim-dc` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Azure AD B2C ağ yönlendirme için kullanılır. |
| `x-ms-cpim-ctx` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Bağlam |
| `x-ms-cpim-rp` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Kaynak sağlayıcısı kiracının üyelik verilerini depolamak için kullanılır. |
| `x-ms-cpim-rc` | b2clogin.com, login.microsoftonline.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Geçiş tanımlama bilgisini depolamak için kullanılır. |
