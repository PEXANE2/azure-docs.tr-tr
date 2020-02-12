---
title: Mobil uygulama için Web API 'Lerini üretim için hazırlama | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini çağıran bir mobil uygulama oluşturmayı öğrenin. (Uygulamaları üretime hazırlayın.)
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
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132381"
---
# <a name="prepare-mobile-apps-for-production"></a>Mobil uygulamaları üretime hazırlama

Bu makalede, üretim ortamına taşımadan önce mobil uygulamanızın kalitesini ve güvenilirliğini geliştirme hakkında ayrıntılı bilgi sağlanır.

## <a name="handle-errors"></a>Hataları işleme

Bir mobil uygulamayı üretime hazırlarken çeşitli hata koşulları oluşabilir. İşletireceğiz ana durumlar sessiz arızalardır ve etkileşime geri dönüşlerdir. Göz önünde bulundurmanız gereken diğer koşullar arasında ağ durumları, hizmet kesintileri, yönetici onayı gereksinimleri ve diğer senaryoya özgü durumlar dahil değildir.

Her Microsoft kimlik doğrulama kitaplığı (MSAL) türü için, hata koşullarının nasıl işleneceğini açıklayan örnek kodu ve wiki içeriğini bulabilirsiniz:

- [MSAL Android wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigate-and-investigate-issues"></a>Sorunları azaltma ve araştırma

Uygulamanızdaki sorunları daha iyi tanılamak için veri toplayın. Toplayacağınız veri türleri hakkında daha fazla bilgi için bkz. [msal uygulamalarında günlüğe kaydetme](https://docs.microsoft.com/azure/active-directory/develop/msal-logging).

Veri toplama için bazı öneriler aşağıda verilmiştir:

- Kullanıcılar sorun yaşadıklarında yardım isteyebilir. Günlükleri yakalamak ve geçici olarak depolamak en iyi uygulamadır. Kullanıcıların günlükleri karşıya yükleyebilecekleri bir konum belirtin. MSAL, kimlik doğrulamasıyla ilgili ayrıntılı bilgileri yakalamak için günlüğe kaydetme uzantıları sağlar.

- Telemetri varsa, kullanıcıların uygulamanızda oturum açma hakkında veri toplamak için MSAL aracılığıyla etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

Ek örnekler denemek için bkz. [Masaüstü ve mobil ortak istemci uygulamaları](sample-v2-code.md#desktop-and-mobile-public-client-apps).
