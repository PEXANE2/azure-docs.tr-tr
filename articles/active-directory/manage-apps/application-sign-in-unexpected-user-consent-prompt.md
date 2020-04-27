---
title: Bir uygulamada oturum açarken beklenmeyen onay istemi | Microsoft Docs
description: Bir Kullanıcı Azure AD ile tümleştirmiş olduğunuz bir uygulama için bir onay istemi gördüğünde sorun giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83d043ecef152f977437e21e2caec40d1c40ce0d
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "65781144"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Bir uygulamada oturum açarken beklenmeyen onay istemi

Azure Active Directory ile tümleştirilen birçok uygulama, çalıştırmak için çeşitli kaynaklara izinler gerektirir. Bu kaynaklar ayrıca Azure Active Directory ile tümleştirildiğinde, bunlara erişim izinleri Azure AD onay çerçevesi kullanılarak istenir. 

Bu, genellikle tek seferlik bir işlem olan bir uygulama ilk kez kullanıldığında bir onay istemi gösterilmekte olur. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Kullanıcıların onay istemlerini göreceği senaryolar

Çeşitli senaryolarda ek istemler beklenebilir:

* Uygulamanın gerektirdiği izinler kümesi değişti.

* Uygulamaya ilk olarak bağlanan kullanıcı yönetici değildi ve artık farklı (yönetici olmayan) bir Kullanıcı uygulamayı ilk kez kullanıyor.

* Uygulamayı ilk olarak uygulamaya veren kullanıcı bir yöneticiydi, ancak kuruluşun tamamı adına izin vermedi.

* Uygulama, izin ilk kez verildikten sonra ek izinler istemek için [artımlı ve dinamik onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) kullanıyor. Bu, genellikle bir uygulamanın isteğe bağlı özelliklerinin temel işlevselliği için gerekenlerden daha fazla izin gerektirmesi durumunda kullanılır.

* Onay, başlangıçta verildikten sonra iptal edildi.

* Geliştirici uygulamayı her kullanıldığında bir onay istemi gerektirecek şekilde yapılandırmıştır (Not: Bu en iyi yöntem değildir).

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure Active Directory uygulamalar, izinler ve onay (v 1.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Azure Active Directory (v 2.0 uç noktası) kapsamlar, izinler ve onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


