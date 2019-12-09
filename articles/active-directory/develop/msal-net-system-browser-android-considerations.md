---
title: Xamarin Android Sistem tarayıcısı konuları (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android üzerinde sistem tarayıcıları kullanırken belirli hususlar hakkında bilgi edinin.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d3ea2554fac8654b052e3e38633af23e7c778b3
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74915460"
---
#  <a name="xamarin-android-system-browser-considerations-with-msalnet"></a>MSAL.NET ile Xamarin Android Sistem tarayıcısı konuları

Bu makalede, .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android üzerinde sistem tarayıcısı kullanılırken belirli noktalar ele alınmaktadır.

MSAL.NET 2.4.0-Preview sürümünden itibaren, MSAL.NET Chrome dışındaki tarayıcıları destekler ve artık, kimlik doğrulaması için Android cihazında Chrome 'un yüklü olmasını gerektirmez.

Bunlar gibi özel sekmeleri destekleyen tarayıcılar kullanmanızı öneririz:

| Özel sekmeler desteği olan tarayıcılar | Paket Adı |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com. Microsoft. emmx|
|Firefox | org. Mozilla. Firefox|
|Eko | com. ekosı. Android|
|Kivi | com. kiwibrowser. Browser|
|Brave | com. brave. Browser|

Özel sekmeleri desteklemeyen tarayıcıların yanı sıra, test etemizi temel alarak, özel sekmeleri desteklemeyen birkaç tarayıcı da kimlik doğrulaması için de çalışır: Opera, Opera Mini, InBrowser ve Maxthon. Daha fazla bilgi için, [test sonuçları için tablo](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested)okuyun.

## <a name="known-issues"></a>Bilinen sorunlar

- Kullanıcı cihazda etkin bir tarayıcı yoksa, MSAL.NET bir `AndroidActivityNotFound` özel durumu oluşturur. 
  - **Risk azaltma**: kullanıcıya, cihazlarında bir tarayıcıyı (tercihen özel sekmeler desteğiyle) etkinleştirmeleri gerektiğini bildirin.

- Kimlik doğrulaması başarısız olursa (örn. kimlik doğrulaması DuckDuckGo ile başlatılır), MSAL.NET bir `AuthenticationCanceled MsalClientException`döndürür. 
  - **Kök sorunu**: cihazda özel sekmeler desteğiyle bir tarayıcı etkinleştirilmemiş. Kimlik doğrulaması, kimlik doğrulamasının tamamlanamayacak alternatif bir tarayıcıyla başlatıldı. 
  - **Risk azaltma**: kullanıcıya, cihazlarına bir tarayıcı yüklemeleri gerektiğini bildirin (tercihen özel sekme desteği olan tercihen).

## <a name="devices-and-browsers-tested"></a>Test edilen cihazlar ve tarayıcılar
Aşağıdaki tabloda, sınanan cihazlar ve tarayıcılar listelenmektedir.

| | Tarayıcı&ast;     |  Sonuç  | 
| ------------- |:-------------:|:-----:|
| Huawei/One + | Chrome&ast; | Geç|
| Huawei/One + | Edge&ast; | Geç|
| Huawei/One + | Firefox&ast; | Geç|
| Huawei/One + | Brave&ast; | Geç|
| Tek + | Eko&ast; | Geç|
| Tek + | Kiwi&ast; | Geç|
| Huawei/One + | Opera | Geç|
| Huawei | İşletimsel mini | Geç|
| Huawei/One + | InBrowser | Geç|
| Tek + | Maxthon | Geç|
| Huawei/One + | DuckDuckGo | Kullanıcı kimlik doğrulamasını iptal etti|
| Huawei/One + | UC tarayıcısı | Kullanıcı kimlik doğrulamasını iptal etti|
| Tek + | Dolphin | Kullanıcı kimlik doğrulamasını iptal etti|
| Tek + | CM tarayıcısı | Kullanıcı kimlik doğrulamasını iptal etti|
| Huawei/One + | hiçbiri yüklü değil | AndroidActivityNotFound EX|

&ast; özel sekmeleri destekler

## <a name="next-steps"></a>Sonraki adımlar
Kod parçacıkları ve Xamarin Android ile sistem tarayıcısını kullanma hakkında ek bilgiler için bu [Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid)okuyun.  