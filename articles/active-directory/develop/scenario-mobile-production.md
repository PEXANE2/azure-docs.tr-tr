---
title: Mobil uygulamayı Web API 'Lerini üretime taşıma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir mobil uygulama oluşturmayı öğrenin (üretime geçin)
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e267b2f398480da6949a4c85133342aad5c2ba7
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74919911"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Web API 'Lerini çağıran mobil uygulama-üretime taşı

Bu makalede, üretim ortamına taşımadan önce uygulamanızın kalite ve güvenilirliğini geliştirme hakkında ayrıntılı bilgi sağlanır.

## <a name="handling-errors-in-mobile-applications"></a>Mobil uygulamalardaki hataları işleme

Bu noktada uygulamanızda bazı hata koşulları oluşabilir. İşlenecek ana senaryolar sessiz arızalardır ve etkileşime geri dönüşlerdir. Üretime göz önünde bulundurmanız gereken diğer koşullar arasında ağ durumları, hizmet kesintileri, yönetici onayı gereksinimleri ve diğer senaryoya özgü durumlar yer alır.

Her MSAL kitaplığı, bu koşulların nasıl işleneceğini açıklayan örnek kod ve wiki içeriğine sahiptir:

- [MSAL Android wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Sorunları azaltıcı ve araştırın

Uygulamanızdaki sorunları tanılamak için veri toplamaya yardımcı olur. Toplayacağınız veri türleri hakkında daha fazla bilgi için bkz. MSAL platform wiksıs.

- Kullanıcılar, sorunlarla karşılaştığında yardım isteyebilir. En iyi uygulama, günlükleri yakalamak ve geçici olarak depolamak ve kullanıcıların bunları karşıya yükleyebilecekleri bir konum sağlamaktır. MSAL, kimlik doğrulamasıyla ilgili ayrıntılı bilgileri yakalamak için günlüğe kaydetme uzantıları sağlar.
- Varsa, kullanıcıların uygulamanızda nasıl oturum açtığı hakkında veri toplamak için MSAL aracılığıyla Telemetriyi etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

Örneklerden sunulan ek örnekleri deneyin [| Masaüstü ve mobil ortak istemci uygulamaları](sample-v2-code.md#desktop-and-mobile-public-client-apps)
