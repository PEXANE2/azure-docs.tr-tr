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
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ea151ee79fccd66f1d9422744d8f57829677ec0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67204526"
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
Olay 6900  
Sunucu, parola değiştirme bildirimini işlerken beklenmeyen bir hatayla karşılaştı:  
AADSTS70002: kimlik bilgileri doğrulanırken hata oluştu. AADSTS50054: eski parola, kimlik doğrulaması için kullanılır.

---
Olay 659  
Parola ilkesi eşitleme yapılandırması alınırken hata oluştu. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: kimlik bilgileri doğrulanırken hata oluştu. AADSTS50054: eski parola, kimlik doğrulaması için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

