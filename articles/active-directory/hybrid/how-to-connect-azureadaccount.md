---
title: Azure AD bağlayıcısı hesap parolasını değiştirme | Microsoft Docs
description: Bu konu başlığı altında, Azure AD Bağlayıcısı hesabının nasıl geri yükleneceği belgeleirsiniz.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: e4f31c560fe3dd91689b361ed520e466fd52da1c
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85360019"
---
# <a name="change-the-azure-ad-connector-account-password"></a>Azure AD Bağlayıcısı hesap parolasını değiştirme
Azure AD Bağlayıcısı hesabının hizmet ücretsiz olması gerekir. Kimlik bilgilerini sıfırlamanız gerekiyorsa, bu konu sizin için önemlidir. Örneğin, bir genel yönetici yanlışlıkla, PowerShell kullanarak hesaptaki parolayı sıfırlamadıysa.

## <a name="reset-the-credentials"></a>Kimlik bilgilerini sıfırlayın
Kimlik doğrulama sorunları nedeniyle Azure AD bağlayıcı hesabı Azure AD ile iletişim kuramıyor, parola sıfırlanabilir.

1. Azure AD Connect eşitleme sunucusunda oturum açın ve PowerShell 'i başlatın.
2. `Add-ADSyncAADServiceAccount` öğesini çalıştırın.
   ![PowerShell cmdlet 'i addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD Genel yönetici kimlik bilgilerini sağlayın.

Bu cmdlet, hizmet hesabının parolasını sıfırlar ve hem Azure AD hem de eşitleme altyapısında güncelleştirir.

## <a name="known-issues-these-steps-can-solve"></a>Bu adımların çözebildiği bilinen sorunlar
Bu bölüm, Azure AD bağlayıcı hesabında kimlik bilgileriyle düzeltilen müşteriler tarafından raporlanan hataların bir listesidir.

---
Olay 6900, sunucu bir parola değişikliği bildirimini işlerken beklenmeyen bir hatayla karşılaştı: AADSTS70002: kimlik bilgileri doğrulanırken hata oluştu. AADSTS50054: eski parola, kimlik doğrulaması için kullanılır.

---
Parola ilkesi eşitleme yapılandırması alınırken olay 659 hatası oluştu. Microsoft. IdentityModel. clients. ActiveDirectory. AdalServiceException: AADSTS70002: kimlik bilgileri doğrulanırken hata oluştu. AADSTS50054: eski parola, kimlik doğrulaması için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
