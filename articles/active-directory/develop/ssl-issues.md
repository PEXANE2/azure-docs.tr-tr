---
title: TLS/SSL sorunlarını giderme (MSAL iOS/macOS) | Mavisi
titleSuffix: Microsoft identity platform
description: MSAL ile TLS/SSL sertifikalarını kullanarak çeşitli sorunlar hakkında ne yapacağınızı öğrenin. Amaç-C Kitaplığı.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: marsma
ms.custom: aaddev
ms.openlocfilehash: 1507231c3ab395319d5ce95ec06dbb592c324aa6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80881086"
---
# <a name="how-to-troubleshoot-msal-for-ios-and-macos-tlsssl-issues"></a>Nasıl yapılır: iOS ve macOS TLS/SSL sorunları için MSAL sorunlarını giderme

Bu makalede, [iOS ve macOS Için Microsoft kimlik doğrulama kitaplığı 'nı (msal)](reference-v2-libraries.md) kullanırken, içinde karşılaşabileceğiniz sorunları gidermenize yardımcı olacak bilgiler sağlanmaktadır

## <a name="network-issues"></a>Ağ sorunları

**Hata-1200**: "bir SSL hatası oluştu ve sunucuyla güvenli bir bağlantı kurulamıyor."

Bu hata bağlantının güvenli olmadığı anlamına gelir. Bir sertifika geçersiz olduğunda meydana gelir. Hangi sunucunun TLS denetimi başarısız olduğu dahil olmak üzere daha fazla bilgi için, bkz `NSURLErrorFailingURLErrorKey` `userInfo` . Error nesnesinin sözlüğü.

Bu hata, Apple 'ın ağ kitaplığından yapılır. NSURL hata kodlarının tam listesi, macOS ve iOS SDK 'lerinde NSURLError. h içinde bulunur. Bu hatayla ilgili daha fazla bilgi için bkz. [URL yükleme sistem hata kodları](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes?language=objc).

## <a name="certificate-issues"></a>Sertifika sorunları

Geçersiz bir sertifika sağlayan URL, kimlik doğrulama akışının bir parçası olarak kullanmayı düşündüğünüz sunucuya bağlanırsa, sorunu tanılamaya yönelik iyi bir başlangıç, URL 'YI [SSL sunucusu testi](https://www.ssllabs.com/ssltest/analyze.html)gıbı bir SSL doğrulama hizmeti ile test etmek olur. Sunucuyu geniş bir senaryo ve tarayıcı dizisine karşı sınar ve bilinen birçok güvenlik açığını denetler.

Varsayılan olarak, Apple 'ın yeni [uygulama taşıma güvenliği (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) ÖZELLIĞI, TLS/SSL sertifikalarını kullanan uygulamalara daha sıkı güvenlik ilkeleri uygular. Bazı işletim sistemleri ve Web tarayıcıları, bu ilkelerin bazılarını varsayılan olarak zorlamayı kullanmaya başlamıştır. Güvenlik nedenleriyle, ATS devre dışı bırakmanızı öneririz.

SHA-1 karmaları kullanan sertifikaların bilinen güvenlik açıkları vardır. Modern Web tarayıcıları çoğu, SHA-1 karmaları olan sertifikalara izin vermez.

## <a name="captive-portals"></a>Kıltıcı portallar

Açıklamalı Portal, bir Wi-Fi ağına ilk kez erişirken ve henüz bu ağa erişim verilmemişse bir kullanıcıya Web sayfası sunar. Kullanıcı portalın gereksinimlerini karşılaana kadar internet trafiğini karşılar. Kullanıcı Portal üzerinden bağlanana kadar ağ kaynaklarına bağlanamadığı için ağ hataları beklenmektedir.

## <a name="next-steps"></a>Sonraki adımlar

[Açıklamalı portallar](https://en.wikipedia.org/wiki/Captive_portal) ve Apple 'ın yeni [uygulama TAŞıMA güvenliği (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) özelliği hakkında bilgi edinin.
