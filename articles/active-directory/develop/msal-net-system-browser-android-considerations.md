---
title: Xamarin Android konuları (.NET için Microsoft kimlik doğrulama kitaplığı) | Mavisi
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
ms.openlocfilehash: 1b0c810097913e896027245b15600ed75aabcd25
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532584"
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

Özel sekmeler desteği olan tarayıcılara ek olarak, test etemizi temel alarak, özel sekmeleri desteklemeyen birkaç tarayıcı da kimlik doğrulaması için de çalışır: Opera, Opera Mini, InBrowser ve Maxthon. Daha fazla bilgi için, [test sonuçları için tablo](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested)okuyun.

## <a name="known-issues"></a>Bilinen sorunlar

- Kullanıcı cihazda hiç tarayıcı etkinleştirilmemişse, msal.net bir `AndroidActivityNotFound` özel durum oluşturur. 
  - **Risk azaltma**: Kullanıcılara cihazındaki bir tarayıcıyı (tercihen özel sekmeler desteğiyle) etkinleştirmeleri gerektiğini bildirin.

- Kimlik doğrulaması başarısız olursa (örn. kimlik doğrulaması DuckDuckGo ile başlatılır), MSAL.NET döndürür `AuthenticationCanceled MsalClientException`. 
  - **Kök sorun**: Özel sekmeler desteği olan bir tarayıcı cihazda etkinleştirilmemiş. Kimlik doğrulaması, kimlik doğrulamasının tamamlanamayacak alternatif bir tarayıcıyla başlatıldı. 
  - **Risk azaltma**: Kullanıcıya, cihazlarına bir tarayıcı yüklemeleri gerektiğini bildirin (tercihen özel sekme desteği olan tercihen).

## <a name="devices-and-browsers-tested"></a>Test edilen cihazlar ve tarayıcılar
Aşağıdaki tabloda, sınanan cihazlar ve tarayıcılar listelenmektedir.

| | Tarayıcı&ast;     |  Sonuç  | 
| ------------- |:-------------:|:-----:|
| Huawei/One + | Mu&ast; | Geçiş|
| Huawei/One + | Kenarını&ast; | Geçiş|
| Huawei/One + | 'U&ast; | Geçiş|
| Huawei/One + | Brave&ast; | Geçiş|
| Tek + | Eko&ast; | Geçiş|
| Tek + | Kivi&ast; | Geçiş|
| Huawei/One + | Opera | Geçiş|
| Huawei | İşletimsel mini | Geçiş|
| Huawei/One + | InBrowser | Geçiş|
| Tek + | Maxthon | Geçiş|
| Huawei/One + | DuckDuckGo | Kullanıcı kimlik doğrulamasını iptal etti|
| Huawei/One + | UC tarayıcısı | Kullanıcı kimlik doğrulamasını iptal etti|
| Tek + | Dolphin | Kullanıcı kimlik doğrulamasını iptal etti|
| Tek + | CM tarayıcısı | Kullanıcı kimlik doğrulamasını iptal etti|
| Huawei/One + | hiçbiri yüklü değil | AndroidActivityNotFound EX|

&ast;Özel sekmeleri destekler

## <a name="next-steps"></a>Sonraki adımlar
Kod parçacıkları ve Xamarin Android ile sistem tarayıcısını kullanma hakkında ek bilgiler için bu [Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid)okuyun.  