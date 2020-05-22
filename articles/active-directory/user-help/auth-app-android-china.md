---
title: Çin 'de Android için kullanılabilirliği ve sınırlamaları Microsoft Authenticator | Microsoft Docs
description: Çin 'de Microsoft Authenticator uygulama kullanılabilirliğini alma hakkında bilgi edinin
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 05/20/2020
ms.author: curtand
ms.openlocfilehash: 6d6d32a7a340ef4b07138d5ec12c561e958cf739
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83739507"
---
# <a name="microsoft-authenticator-for-android-in-the-public-cloud-in-china"></a>Çin 'de Genel bulutta Android için Microsoft Authenticator

Android için Microsoft Authenticator uygulaması Çin 'de indirilebilir. Google Play Store Çin 'de kullanılamaz, bu nedenle uygulama diğer Çince uygulama marketlerinden indirilmelidir. Android için Microsoft Authenticator uygulaması şu anda Çin 'deki şu depolarda sunulmaktadır:

- [Baidu](https://shouji.baidu.com/software/26638379.html)
- [Lenovo](https://www.lenovomm.com/appdetail/com.azure.authenticator/20197724)
- [Huawei](https://appgallery.cloud.huawei.com/uowap/index.html#/detailApp/C100262999?source=appshare&subsource=C100262999&shareTo=weixin&locale=zh_CN)
- [Samsung Galaxy Mağazası](http://apps.samsung.com/appquery/appDetail.as?appId=com.azure.authenticator)

Uygulamanın en güncel derlemesi Google Play Store, ancak diğer tüm uygulama mağazalarında uygulamayı yaptığımız kadar hızlı bir şekilde güncelleştiriyoruz. Herhangi bir uygulama deposuna dağıtılan özel bir Android uygulama paketi (APK) olmadığından, uygulama aşağıdaki konumlardan birinden sorunsuzca bir şekilde güncelleştirilemeyebilir:

- İndirilen mağaza
- Kullanıcı bölgelerden kesiştiği durumlarda Google Play Store

## <a name="limitations"></a>Sınırlamalar

Android için Microsoft Authenticator uygulaması, Google 'ın Firebase bulut mesajlaşma sistemini kullanır ve anında iletme bildirimleri almak için Google Play Hizmetleri. Çin 'de hiçbir hizmet kullanılamadığından, uygulamanın işlevleri üzerinde bazı sınırlamalar vardır:

- Kimlik doğrulayıcı uygulamasını, anında iletme bildirimleri kullanılarak Multi-Factor Authentication (MFA) yöntemi olarak kaydetme çalışmıyor.

- [Telefonla oturum açma](../authentication/howto-authentication-sms-signin.md) ayarlanamıyor. Bu, kullanıcının şu anda çalışmayan anında iletme bildirimleri kullanılarak uygulamayı bir MFA yöntemi olarak kurulumunu gerektirir.

Bir Kullanıcı, uygulamayı kullanarak telefon oturum açma veya Multi-Factor Authentication 'ı daha önce yönetmiş ise, uygulamadaki bildirim isteklerini el ile denetler ve kimlik doğrulaması için kullanabilir.

## <a name="multi-factor-authentication-workaround"></a>Multi-Factor Authentication geçici çözümü

Kullanıcılar, Multi-Factor Authentication için anında iletme bildirimleri kullanmak yerine, kendi cihazlarında kendi kimliklerini doğrulamak için kullanabilecekleri doğrulama kodları almak üzere MFA 'yı ayarlayabilir. Bu doğrulama kodları 30 saniye için geçerlidir ve bunları kullanmak için, yöneticilerin, kiracının zaman tabanlı bir kerelik parola (TOTP) doğrulama kodlarını kullanarak doğrulama gerçekleştirmesini etkinleştirmesi gerekir.

## <a name="availability"></a>Kullanılabilirlik

Microsoft Authenticator özelliği | Çin 'de kullanılabilirlik
------------------------------- | ---------------------
Anında iletme bildirimlerini kullanarak MFA kaydı | No
Önceden var olan MFA hesabı anında iletme bildirimleri kullanılarak kimlik doğrulanıyor | No
Önceden var olan MFA hesabı, bildirimler için el ile denetim gerçekleştiriliyor | Yes
Yalnızca TOTP/doğrulama kodlarını kullanarak MFA kaydı/kimlik doğrulaması | Yes
Telefonla oturum açma kaydı | No
Anında iletme bildirimlerini kullanarak mevcut telefonda oturum açma | No
Kimlik doğrulama istekleri için el ile denetim gerçekleştirerek mevcut telefon oturum açma doğrulaması | Evet

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Authenticator uygulamasını indirme ve yükleme](user-help-auth-app-download-install.md)
