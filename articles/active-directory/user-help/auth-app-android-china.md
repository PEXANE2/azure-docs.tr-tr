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
ms.date: 12/08/2020
ms.author: curtand
ms.openlocfilehash: 7b3af7c5bd55e748b1d1f4cf8f192797187da30a
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96901805"
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

Kullanıcılar, Multi-Factor Authentication için anında iletme bildirimleri kullanmak yerine, kendi kendi cihazlarındaki kimliklerini doğrulamak için kullanabilecekleri kimlik [doğrulama kodlarını almak üzere kimlik doğrulayıcı uygulamasını ayarlayabilir](multi-factor-authentication-setup-auth-app.md#set-up-the-microsoft-authenticator-app-to-use-verification-codes) . Bu doğrulama kodları 30 saniye için geçerlidir ve bunları kullanmak için, yöneticilerin, zaman tabanlı One-Time parola (TOTP) doğrulama kodlarını kullanarak kimlik doğrulaması gerçekleştirmesi gerekir.

## <a name="availability"></a>Kullanılabilirlik

Microsoft Authenticator özelliği | Çin 'de kullanılabilirlik
------------------------------- | ---------------------
Anında iletme bildirimlerini kullanarak MFA kaydı | No
Önceden var olan MFA hesabı anında iletme bildirimleri kullanılarak kimlik doğrulanıyor | No
Önceden var olan MFA hesabı, bildirimler için el ile denetim gerçekleştiriliyor | Yes
Yalnızca TOTP/doğrulama kodlarını kullanarak MFA kaydı/kimlik doğrulaması | Yes
Telefonla oturum açma kaydı | No
Anında iletme bildirimlerini kullanarak mevcut telefonda oturum açma | No
Kimlik doğrulama istekleri için el ile denetim gerçekleştirerek mevcut telefon oturum açma doğrulaması | Yes
Kişisel Microsoft hesapları için kimlik doğrulayıcı uygulaması desteği | Hayır

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Authenticator uygulamasını indirme ve yükleme](user-help-auth-app-download-install.md)
