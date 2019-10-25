---
title: Xamarin Android konuları (.NET için Microsoft kimlik doğrulama kitaplığı)
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android kullanırken belirli hususlar hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f5caad4b136c9ef2686cc4befc70e6720e27855
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802742"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>MSAL.NET ile Xamarin Android 'e özgü konular
Bu makalede, .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android üzerinde sistem tarayıcısı kullanılırken belirli noktalar ele alınmaktadır.

MSAL.NET 2.4.0-Preview sürümünden itibaren, MSAL.NET Chrome dışındaki tarayıcıları destekler ve artık, kimlik doğrulaması için Android cihazında Chrome 'un yüklü olmasını gerektirmez.

Bunlar gibi özel sekmeleri destekleyen tarayıcılar kullanmanızı öneririz:

| Özel sekmeler desteği olan tarayıcılar | Paket adı |
|------| ------- |
|Chrome | com. Android. Chrome|
|Microsoft Edge | com. Microsoft. emmx|
|'U | org. Mozilla. Firefox|
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
| Huawei/One + | Chrome&ast; | Aktar|
| Huawei/One + | Edge&ast; | Aktar|
| Huawei/One + | Firefox&ast; | Aktar|
| Huawei/One + | Brave&ast; | Aktar|
| Tek + | Eko&ast; | Aktar|
| Tek + | Kiwi&ast; | Aktar|
| Huawei/One + | Opera | Aktar|
| Huawei | İşletimsel mini | Aktar|
| Huawei/One + | InBrowser | Aktar|
| Tek + | Maxthon | Aktar|
| Huawei/One + | DuckDuckGo | Kullanıcı kimlik doğrulamasını iptal etti|
| Huawei/One + | UC tarayıcısı | Kullanıcı kimlik doğrulamasını iptal etti|
| Tek + | Dolphin | Kullanıcı kimlik doğrulamasını iptal etti|
| Tek + | CM tarayıcısı | Kullanıcı kimlik doğrulamasını iptal etti|
| Huawei/One + | hiçbiri yüklü değil | AndroidActivityNotFound EX|

&ast; özel sekmeleri destekler

## <a name="next-steps"></a>Sonraki adımlar
Kod parçacıkları ve Xamarin Android ile sistem tarayıcısını kullanma hakkında ek bilgiler için bu [Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid)okuyun.  