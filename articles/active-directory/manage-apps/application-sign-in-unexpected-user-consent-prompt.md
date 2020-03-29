---
title: Bir uygulamada oturum açarken beklenmeyen onay istemi | Microsoft Dokümanlar
description: Bir kullanıcı, Azure AD ile tümleştirdiğiniz ve beklemediğiniz bir uygulama için onay istemi gördüğünde sorun giderme
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65781144"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Bir uygulamada oturum açarken beklenmeyen onay istemi

Azure Etkin Dizinile tümleşen birçok uygulama, çalıştırmak için çeşitli kaynaklara izin gerektirir. Bu kaynaklar Azure Etkin Dizinile de entegre edildiğinde, azure AD onay çerçevesi kullanılarak bunlara erişim izni istenir. 

Bu, genellikle tek seferlik bir işlem olan bir uygulama ilk kez kullanıldığında bir onay isteminin gösterilmesiyle sonuçlanır. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Kullanıcıların onay istemlerini gördüğü senaryolar

Çeşitli senaryolarda ek istemler beklenebilir:

* Uygulamanın gerektirdiği izinler kümesi değişti.

* Uygulamayı ilk olarak kabul eden kullanıcı yönetici değildi ve şimdi farklı (yönetici olmayan) Kullanıcı uygulamayı ilk kez kullanıyor.

* Başvuruyu ilk olarak kabul eden kullanıcı bir yöneticiydi, ancak tüm kuruluş adına izin vermedi.

* Uygulama, başlangıçta onay verildikten sonra ek izin istemek için [artımlı ve dinamik onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) kullanıyor. Bu genellikle, bir uygulama ekinin isteğe bağlı özellikleri temel işlevsellik için gerekli olanların ötesinde izinler gerektirdiğinde kullanılır.

* Başlangıçta onay verildikten sonra onay iptal edildi.

* Geliştirici, uygulamayı her kullanıldığında bir onay istemi gerektirecek şekilde yapılandırmıştır (not: bu en iyi uygulama değildir).

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure Etkin Dizin 'de uygulamalar, izinler ve onay (v1.0 bitiş noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Azure Etkin Dizini'nde kapsamlar, izinler ve onay (v2.0 bitiş noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


