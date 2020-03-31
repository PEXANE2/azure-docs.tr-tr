---
title: Xamarin Android sistemi tarayıcı hususlar (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile Xamarin Android'de sistem tarayıcılarını kullanmak için dikkat edilmesi gereken hususlar hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad26a4d619a7984f08a8decc87f9339adae47cdd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132604"
---
#  <a name="xamarin-android-system-browser-considerations-for-using-msalnet"></a>MSAL.NET kullanmak için Xamarin Android sistemi tarayıcı hususlar

Bu makalede, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile Xamarin Android'deki sistem tarayıcısını kullandığınızda nelere dikkat edilmesi gerektiğini anlatılmaktadır.

MSAL.NET 2.4.0 Önizleme ile başlayan MSAL.NET, Chrome dışındaki tarayıcıları destekler. Artık chrome'un kimlik doğrulaması için Android cihaza yüklenmesini gerektirmez.

Özel sekmeleri destekleyen tarayıcılar kullanmanızı öneririz. Aşağıda bu tarayıcılardan bazı örnekler verilmiştir:

| Özel sekme desteğine sahip tarayıcılar | Paket adı |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.mozilla.firefox|
|Ecosia | com.ecosia.android|
|Kivi | com.kiwibrowser.browser|
|Cesur | com.brave.browser|

Özel sekmedesteği sunan tarayıcıları tanımlamaya ek olarak, testlerimiz özel sekmeleri desteklemeyen birkaç tarayıcının da kimlik doğrulama için çalıştığını gösterir. Bu tarayıcılar Opera, Opera Mini, InBrowser ve Maxthon içerir. 

## <a name="tested-devices-and-browsers"></a>Test edilmiş cihazlar ve tarayıcılar
Aşağıdaki tabloda kimlik doğrulama uyumluluğu için test edilmiş aygıtlar ve tarayıcılar listelenir.

| Cihaz | Tarayıcı     |  Sonuç  | 
| ------------- |:-------------:|:-----:|
| Huawei/Bir+ | Chrome\* | Geçirmek|
| Huawei/Bir+ | Edge\* | Geçirmek|
| Huawei/Bir+ | Firefox\* | Geçirmek|
| Huawei/Bir+ | Cesur\* | Geçirmek|
| Bir+ | Ecosia\* | Geçirmek|
| Bir+ | Kivi\* | Geçirmek|
| Huawei/Bir+ | Opera | Geçirmek|
| Huawei | OperaMini | Geçirmek|
| Huawei/Bir+ | Tarayıcı Nın | Geçirmek|
| Bir+ | Maxthon | Geçirmek|
| Huawei/Bir+ | DuckDuckGo | Kullanıcı kimlik doğrulaması iptal|
| Huawei/Bir+ | UC Tarayıcı | Kullanıcı kimlik doğrulaması iptal|
| Bir+ | Yunus | Kullanıcı kimlik doğrulaması iptal|
| Bir+ | CM Tarayıcı | Kullanıcı kimlik doğrulaması iptal|
| Huawei/Bir+ | Hiçbiri yüklü değil | AndroidActivityNotFound özel durum|

\*Özel sekmeleri destekler

## <a name="known-issues"></a>Bilinen sorunlar

Kullanıcının aygıtta tarayıcısı etkin değilse, MSAL.NET `AndroidActivityNotFound` bir özel durum oluşturur.  
  - **Azaltma**: Kullanıcıdan cihazında bir tarayıcıyı etkinleştirmesini isteyin. Özel sekmeleri destekleyen bir tarayıcı önerin.

Kimlik doğrulama başarısız olursa (örneğin, kimlik doğrulama DuckDuckGo ile `AuthenticationCanceled MsalClientException`başlatılırsa), MSAL.NET geri döner. 
  - **Kök sorunu**: Aygıtta özel sekmeleri destekleyen bir tarayıcı etkinleştirilen değildi. Kimlik doğrulaması, kimlik doğrulamasını tamamlayamayan bir tarayıcıyla başlatılır. 
  - **Azaltma**: Kullanıcıdan cihazında bir tarayıcıyı etkinleştirmesini isteyin. Özel sekmeleri destekleyen bir tarayıcı önerin.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi ve kod örnekleri için, [gömülü bir web tarayıcısı ile Xamarin Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid) ve Gömülü karşı sistem web Kullanıcı Arabirimi'nde bir sistem tarayıcısı arasında seçim ebakına bakın. [Embedded versus system web UI](msal-net-web-browsers.md#embedded-vs-system-web-ui)  