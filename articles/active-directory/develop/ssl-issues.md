---
title: TLS/SSL sorunlarını giderme (MSAL iOS/macOS) | Azure
titleSuffix: Microsoft identity platform
description: MSAL ile TLS/SSL sertifikalarını kullanarak çeşitli sorunlar hakkında ne yapmanız gerektiğini öğrenin. Objective-C kütüphanesi.
services: active-directory
documentationcenter: ''
author: mmacy
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: 42bd016a9d0882cc97eaa800235226b2de9569a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369408"
---
# <a name="how-to-troubleshoot-msal-for-ios-and-macos-tlsssl-issues"></a>Nasıl yapilir: iOS ve macOS TLS/SSL sorunları için Sorun Giderme

Bu makale, [iOS ve macOS için Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL)](reference-v2-libraries.md) kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olacak bilgiler sağlar

## <a name="network-issues"></a>Ağ sorunları

**Hata -1200**: "Bir SSL hatası oluştu ve sunucuya güvenli bir bağlantı yapılamaz."

Bu hata, bağlantının güvenli olmadığı anlamına gelir. Sertifika geçersiz olduğunda oluşur. TLS denetiminde hangi sunucunun başarısız olduğu da `NSURLErrorFailingURLErrorKey` dahil `userInfo` olmak üzere daha fazla bilgi için hata nesnesinin sözlüğüne bakın.

Bu hata Apple'ın ağ kitaplığından. NSURL hata kodlarının tam listesi macOS ve iOS SDK'larda NSURLError.h'de bulunur. Bu hata hakkında daha fazla bilgi [için, URL Yükleme Sistemi Hata Kodları'na](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes?language=objc)bakın.

## <a name="certificate-issues"></a>Sertifika sorunları

Geçersiz bir sertifika sağlayan URL kimlik doğrulama akışının bir parçası olarak kullanmak istediğiniz sunucuya bağlanırsa, sorunu tanılamak için iyi bir başlangıç, URL'yi [SSL Server Testi](https://www.ssllabs.com/ssltest/analyze.html)gibi bir SSL doğrulama hizmetiyle test etmektir. Sunucuyu çok çeşitli senaryolara ve tarayıcılara karşı sınar ve bilinen birçok güvenlik açığını denetler.

Varsayılan olarak, Apple'ın yeni [App Transport Security (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) özelliği TLS/SSL sertifikaları kullanan uygulamalariçin daha sıkı güvenlik ilkeleri uygular. Bazı işletim sistemleri ve web tarayıcıları varsayılan olarak bu ilkelerin bazılarını uygulamaya başlamıştır. Güvenlik nedeniyle, ATS'yi devre dışı etmemenizi öneririz.

SHA-1 hashes kullanan sertifikalar bilinen güvenlik açıkları var. Çoğu modern web tarayıcısı SHA-1 hashes sertifikaları izin vermez.

## <a name="captive-portals"></a>Tutsak portallar

Tutsak bir portal, bir Wi-Fi ağına ilk erişiğinde ve bu ağa henüz erişim izni verilmediğinde kullanıcıya bir web sayfası sunar. Kullanıcı portalın gereksinimlerini karşılayana kadar internet trafiğini keser. Kullanıcı portal üzerinden bağlanana kadar ağ kaynaklarına bağlanamadığı için ağ hataları beklenir.

## <a name="next-steps"></a>Sonraki adımlar

[Tutsak portallar](https://en.wikipedia.org/wiki/Captive_portal) ve Apple'ın yeni [App Transport Security (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) özelliği hakkında bilgi edinin.
