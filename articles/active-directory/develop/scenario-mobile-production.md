---
title: Mobil uygulama için Web API 'Lerini üretim için hazırlama | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini çağıran bir mobil uygulama oluşturmayı öğrenin. (Uygulamaları üretime hazırlayın.)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 28ace84f9a80b71209d7963d02b66317292b151b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80882548"
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
