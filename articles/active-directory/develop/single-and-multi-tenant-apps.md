---
title: Azure AD'de tek ve çok kiracılı uygulamalar
titleSuffix: Microsoft identity platform
description: Azure AD'deki tek kiracılı ve çok kiracılı uygulamalar arasındaki özellikler ve farklar hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: justhu
ms.custom: aaddev
ms.openlocfilehash: 38cb1222a64b1759528749caa15dfb1bb906cef6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159922"
---
# <a name="tenancy-in-azure-active-directory"></a>Azure Etkin Dizinde Kiralama

Azure Etkin Dizin (Azure AD), kullanıcılar ve uygulamalar gibi nesneleri *kiracı*adı verilen gruplar halinde düzenler. Kiracılar, bir yöneticinin güvenlik ve operasyonel ilkelerini karşılamak için kuruluş içindeki kullanıcılar ve kuruluşun sahip olduğu uygulamalar üzerinde ilkeler belirlemesine izin verir. 

## <a name="who-can-sign-in-to-your-app"></a>Uygulamanızda kimler oturum açabilir?

Uygulama geliştirme söz konusu olduğunda, geliştiriciler uygulamalarını [Azure portalında](https://portal.azure.com)uygulama kaydı sırasında tek kiracı veya çok kiracı olacak şekilde yapılandırmayı seçebilirler.
* Tek kiracılı uygulamalar yalnızca kayıtlı oldukları kiracıda kullanılabilir, ev kiracı olarak da bilinir.
* Çok kiracılı uygulamalar, hem ev kiracılarında hem de diğer kiracılarda kullanıcılar tarafından kullanılabilir.

Azure portalında, hedef kitleyi aşağıdaki gibi ayarlayarak uygulamanızı tek kiracı veya çok kiracı olacak şekilde yapılandırabilirsiniz.

| Hedef kitle | Tek/çok kiracılı | Kimler oturum açabilir | 
|----------|--------| ---------|
| Yalnızca bu dizindeki hesaplar | Tek kiracılı | Dizininizdeki tüm kullanıcı ve konuk hesapları uygulamanızı veya API'nizi kullanabilir.<br>*Hedef kitleniz kuruluşunuz için dahiliyse bu seçeneği kullanın.* |
| Herhangi bir Azure REKLAM dizinindeki hesaplar | Çok Kiracılı | Microsoft'tan iş veya okul hesabı olan tüm kullanıcılar ve konuklar uygulamanızı veya API'nizi kullanabilir. Buna Office 365 kullanan okullar ve işletmeler dahildir.<br>*Hedef kitleniz iş veya eğitim amaçlı müşterilerse bu seçeneği kullanın.* |
| Tüm Azure AD dizinindeki ve kişisel Microsoft hesaplarındaki hesaplar (Skype, Xbox, Outlook.com gibi) | Çok Kiracılı | Bir işi veya okulu veya kişisel Microsoft hesabı olan tüm kullanıcılar uygulamanızı veya API'nizi kullanabilir. Office 365'i kullanan okullar ve işletmelerin yanı sıra Xbox ve Skype gibi hizmetlerde oturum açmada kullanılan kişisel hesapları da içerir.<br>*En geniş Microsoft hesabı kümesini hedeflemek için bu seçeneği kullanın.* | 

## <a name="best-practices-for-multi-tenant-apps"></a>Çok kiracılı uygulamalar için en iyi uygulamalar

BT yöneticilerinin kiracılarında belirleyebileceği farklı ilkelerin sayısı nedeniyle harika çok kiracılı uygulamalar oluşturmak zor olabilir. Çok kiracılı bir uygulama oluşturmayı seçerseniz, aşağıdaki en iyi uygulamaları izleyin:

* [Uygulamanızı Koşullu Erişim ilkelerini](../azuread-dev/conditional-access-dev-guide.md)yapılandırılan bir kiracıda test edin.
* Uygulamanızın yalnızca gerçekten ihtiyaç duyduğu izinleri istediğinden emin olmak için en az kullanıcı erişimi ilkesine uyun. Bazı kuruluşlarda kullanıcıların uygulamanızı satın alabEtmesini engelleyebileceğinden, yönetici onayı gerektiren izinleri istemekten kaçının. 
* Uygulamanızın bir parçası olarak ortaya çıkardığınız izinler için uygun adlar ve açıklamalar sağlayın. Bu, kullanıcıların ve yöneticilerin uygulamanızın API'lerini kullanmaya çalıştıklarında neyi kabul ettiklerini bilmelerine yardımcı olur. Daha fazla bilgi [için, izinler kılavuzundaki](v2-permissions-and-consent.md)en iyi uygulamalar bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir uygulamayı çok kiracı lı olarak dönüştürme](howto-convert-app-to-be-multi-tenant.md)
