---
title: 'Azure AD Connect eşitlemi: AD DS hesap parolasını değiştirme | Microsoft Dokümanlar'
description: Bu konu belgesi, AD DS hesabının parolası değiştirildikten sonra Azure AD Connect'in nasıl güncelleştirilebildiğini açıklar.
services: active-directory
keywords: AD DS hesabı, Active Directory hesabı, şifre
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35e04be046e20883f60c576745a29342add68a81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60241617"
---
# <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
AD DS hesabı, Azure AD Connect tarafından şirket içi Active Directory ile iletişim kurmak için kullanılan kullanıcı hesabı anlamına gelir. AD DS hesabının parolasını değiştirirseniz, Azure AD Bağlantı Eşitleme Hizmetini yeni parolayla güncelleştirmeniz gerekir. Aksi takdirde, Eşitleme artık şirket içi Active Directory ile doğru eşitlenemez ve aşağıdaki hatalarla karşılaşırsınız:

* Eşitleme Hizmet Yöneticisi'nde, şirket içi AD ile yapılan herhangi bir içe aktarma veya dışa aktarma işlemi **başlangıç kimlik bilgileri** hatası olmadan başarısız olur.

* Windows Olay Görüntüleyicisi altında, uygulama olay günlüğü **Olay Kimliği 6000** ile ilgili bir hata içerir ve **'Yönetim aracısı "contoso.com" kimlik bilgileri geçersiz olduğu için çalışmadı'** iletisi.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>AD DS hesabı için yeni parola ile Eşitleme Hizmeti nasıl güncellenir?
Eşitleme Hizmetini yeni parolayla güncelleştirmek için:

1. Eşitleme Hizmet Yöneticisi'ni başlatın (START → Eşitleme Hizmeti).
</br>![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-change-addsacct-pass/startmenu.png)  

2. **Bağlayıcılar** sekmesine gidin.

3. Parolasının değiştirildiği AD DS hesabına karşılık gelen **AD Bağlayıcısı'nı** seçin.

4. **Eylemler**altında, **Özellikleri**seçin.

5. Açılan iletişim kutusunda Etkin **Dizin Ormanı'na Bağlan'ı**seçin:

6. Ad DS hesabının yeni parolasını **Parola** metin kutusuna girin.

7. Yeni parolayı kaydetmek ve açılır pencere iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

8. Windows Service Control Manager altında Azure AD Connect Eşitleme Hizmetini yeniden başlatın. Bu, eski parolaya yapılan herhangi bir başvurunun bellek önbelleğinden kaldırıldığından emin olmak içindir.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
