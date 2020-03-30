---
title: Mobil uygulama arama web API'lerini üretime hazırlama | Azure
titleSuffix: Microsoft identity platform
description: Web API'lerini çağıran bir mobil uygulama nın nasıl oluşturulabildiğini öğrenin. (Uygulamaları üretime hazırlayın.)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1ea19b8b76f4eb4a2c984f0e39eb0fd373c8b83c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132381"
---
# <a name="prepare-mobile-apps-for-production"></a>Mobil uygulamaları üretime hazırlayın

Bu makalede, mobil uygulamanızın üretime geçmeden önce kalitesini ve güvenilirliğini nasıl artırabileceğiniz hakkında ayrıntılı bilgi verilmektedir.

## <a name="handle-errors"></a>Hataları işleme

Bir mobil uygulamayı üretim için hazırlarken, çeşitli hata koşulları oluşabilir. Ele alacağımana durumlarda sessiz hatalar ve etkileşim için geri dönüşler vardır. Göz önünde bulundurmanız gereken diğer koşullar arasında ağ dışı durumlar, hizmet kesintileri, yönetici onayı gereksinimleri ve senaryoya özgü diğer durumlar yer alır.

Her Microsoft Kimlik Doğrulama Kitaplığı (MSAL) türü için, hata koşullarının nasıl işleyeceğini açıklayan örnek kod ve wiki içeriği bulabilirsiniz:

- [MSAL Android wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigate-and-investigate-issues"></a>Sorunları azaltmak ve araştırmak

Uygulamanızdaki sorunları daha iyi tanılamak için veri toplayın. Toplayabileceğiniz veri türleri hakkında bilgi için [MSAL uygulamalarında günlüğe kaydetme](https://docs.microsoft.com/azure/active-directory/develop/msal-logging)bölümüne bakın.

Veri toplama için bazı öneriler şunlardır:

- Kullanıcılar sorun olduğunda yardım isteyebilir. En iyi yöntem günlükleri yakalamak ve geçici olarak depolamaktır. Kullanıcıların günlükleri yükleyebileceği bir konum sağlayın. MSAL, kimlik doğrulama hakkında ayrıntılı bilgi yakalamak için günlük uzantıları sağlar.

- Telemetri varsa, kullanıcıların uygulamanızda nasıl oturum açaçalıştıkları hakkında veri toplamak için MSAL aracılığıyla etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

Ek örnekleri denemek için [Masaüstü ve mobil ortak istemci uygulamalarına](sample-v2-code.md#desktop-and-mobile-public-client-apps)bakın.
