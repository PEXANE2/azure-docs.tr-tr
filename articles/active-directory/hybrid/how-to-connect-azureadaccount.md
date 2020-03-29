---
title: Azure AD Bağlayıcıhesap parolasını değiştirme | Microsoft Dokümanlar
description: Bu konu, Azure AD Bağlayıcısı hesabının nasıl geri yüklenir olduğunu belgelezdir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67204526"
---
# <a name="change-the-azure-ad-connector-account-password"></a>Azure AD Bağlayıcısı hesap parolasını değiştirme
Azure AD Bağlayıcısı hesabının ücretsiz olması gerekir. Kimlik bilgilerini sıfırlamanız gerekiyorsa, bu konu tam size göre. Örneğin, bir Global Administrator yanlışlıkla PowerShell kullanarak hesaptaki parolayı sıfırlamışsa.

## <a name="reset-the-credentials"></a>Kimlik bilgilerini sıfırlama
Azure AD Bağlayıcısı hesabı kimlik doğrulama sorunları nedeniyle Azure AD ile iletişim kuramıyorsa, parola sıfırlanabilir.

1. Azure AD Connect eşitleme sunucusunda oturum açın ve PowerShell'i başlatın.
2. `Add-ADSyncAADServiceAccount` öğesini çalıştırın.  
   ![PowerShell cmdlet addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD Global yönetici kimlik bilgilerini sağlayın.

Bu cmdlet, hizmet hesabının parolasını sıfırlar ve hem Azure AD'de hem de eşitleme altyapısında güncelleştirin.

## <a name="known-issues-these-steps-can-solve"></a>Bu adımların çözebileceği bilinen sorunlar
Bu bölüm, müşteriler tarafından bildirilen ve Azure AD Bağlayıcısı hesabında ki kimlik bilgileri sıfırlama sıtarafından düzeltilen hataların bir listesidir.

---
Olay 6900  
Sunucu, parola değişikliği bildirimini işlerken beklenmeyen bir hatayla karşılaştı:  
AADSTS70002: Kimlik bilgilerini doğrulama hatası. AADSTS50054: Kimlik doğrulama için eski parola kullanılır.

---
Olay 659  
Parola ilkesi eşitleme yapılandırması alırken hata. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Kimlik bilgilerini doğrulama hatası. AADSTS50054: Kimlik doğrulama için eski parola kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

