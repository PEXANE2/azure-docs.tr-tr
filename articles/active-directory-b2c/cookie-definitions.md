---
title: Tanımlama bilgisi tanımları
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de kullanılan tanımlama bilgileri için tanımlar sağlar.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b984b75b3a12606aa0d82c7e7b399d5dce59df33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189523"
---
# <a name="cookies-definitions-for-azure-ad-b2c"></a>Azure AD B2C için tanımlama bilgileri tanımları

Aşağıdaki bölümler, Azure Etkin Dizin Ii B2C'de (Azure AD B2C) kullanılan tanımlama bilgileri hakkında bilgi sağlar.

## <a name="samesite"></a>Aynı Site

Microsoft Azure AD B2C hizmeti, öznitelik `SameSite=None` desteği `Secure` de dahil olmak üzere SameSite tarayıcı yapılandırmalarıyla uyumludur.

Sitelere erişimi korumak için, web tarayıcıları aksi belirtilmedikçe tüm tanımlama bilgilerinin dış erişimden korunması gerektiğini varsayan yeni bir güvenli model sunar. Chrome tarayıcısı, [Şubat 2020'de Chrome 80](https://www.chromium.org/updates/same-site)ile başlayan bu değişikliği ilk uygulayan tarayıcıdır. Chrome'daki değişime hazırlanma hakkında daha fazla bilgi için [Geliştiriciler: Yeni SameSite=None'a Hazır Olun; ](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)Chromium Blog'da Güvenli Çerez Ayarları.

Geliştiriciler, `SameSite=None`siteleri arası erişim için tanımlama bilgilerini belirlemek için yeni çerez ayarını kullanmalıdır. `SameSite=None` Öznitelik mevcut olduğunda, siteler arası tanımlama bilgilerine yalnızca HTTPS bağlantıları üzerinden erişilebilmek için ek `Secure` bir öznitelik kullanılmalıdır. Azure AD B2C kullanan uygulamalar da dahil olmak üzere tüm uygulamalarınızı doğrulayın ve test edin.

Daha fazla bilgi için bkz.

* [Chrome tarayıcısında SameSite tanımlama bilgisi değişikliklerini işleme](../active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser.md)
* [Chrome sürüm 80 veya sonraki sürümdeki müşteri web siteleri ve Microsoft hizmetleri ve ürünleri üzerindeki etkisi](https://support.microsoft.com/help/4522904/potential-disruption-to-customer-websites-in-latest-chrome)

## <a name="cookies"></a>Tanımlama bilgileri

Aşağıdaki tabloda Azure AD B2C'de kullanılan tanımlama bilgileri listelanmaktadır.

| Adı | Domain | Bitiş tarihi | Amaç |
| ----------- | ------ | -------------------------- | --------- |
| `x-ms-cpim-admin` | main.b2cadmin.ext.azure.com | Tarayıcı [oturumunun](session-behavior.md) sonu | Kullanıcı üyelik verilerini kiracılar arasında tutar. Kiracı bir kullanıcı üyesi ve üyelik düzeyi (Yönetici veya Kullanıcı). |
| `x-ms-cpim-slice` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | İstekleri uygun üretim örneğine yönlendirmek için kullanılır. |
| `x-ms-cpim-trans` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | İşlemleri (Azure AD B2C kimlik doğrulama isteği sayısı) ve geçerli hareketi izlemek için kullanılır. |
| `x-ms-cpim-sso:{Id}` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | SSO oturumunu korumak için kullanılır. |
| `x-ms-cpim-cache:{id}_n` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md)sonu , başarılı kimlik doğrulama | İstek durumunu korumak için kullanılır. |
| `x-ms-cpim-csrf` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | CRSF koruması için kullanılan Site Arası İstek Sahteciliği belirteci. |
| `x-ms-cpim-dc` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | Azure AD B2C ağ yönlendirmesi için kullanılır. |
| `x-ms-cpim-ctx` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | Bağlam |
| `x-ms-cpim-rp` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | Kaynak sağlayıcı kiracı için üyelik verilerini depolamak için kullanılır. |
| `x-ms-cpim-rc` | b2clogin.com, login.microsoftonline.com, markalı alan adı | Tarayıcı [oturumunun](session-behavior.md) sonu | Röle çerezini depolamak için kullanılır. |
