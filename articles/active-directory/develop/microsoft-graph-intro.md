---
title: Microsoft Grafik API | Microsoft Dokümanlar
description: Microsoft Graph API, Microsoft Cloud hizmet kaynaklarına erişmenizi sağlayan yeni bir web API'sidir.
author: davidmu1
services: active-directory
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/13/2020
ms.author: davidmu
ms.custom: aaddev
ms.openlocfilehash: 6c1b4390282f6d54178365714b1e2e665b4cf061
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136509"
---
# <a name="microsoft-graph-api"></a>Microsoft Graph API'si

Microsoft Graph API, Microsoft Cloud hizmet kaynaklarına erişmenizi sağlayan yeni bir web API'sidir. Uygulamanızı kaydettikten ve bir kullanıcı veya hizmet için kimlik doğrulama belirteçleri aldıktan sonra, Microsoft Graph API'ye istekte bulunabilirsiniz. Daha fazla bilgi için Microsoft [Graph'a Genel Bakış](https://docs.microsoft.com/graph/overview)bölümüne bakın.

Microsoft Graph, aşağıdaki Microsoft 365 hizmetlerindeki verilere erişmek için REST API'larını ve istemci kitaplıklarını ortaya çıkarır:
- Office 365 hizmetleri: Delve, Excel, Microsoft Rezervasyonları, Microsoft Teams, OneDrive, OneNote, Outlook/Exchange, Planlayıcı ve SharePoint
- Kurumsal Mobilite ve Güvenlik hizmetleri: Gelişmiş Tehdit Analizi, Gelişmiş Tehdit Koruması, Azure Active Directory, Identity Manager ve Intune
- Windows 10 hizmetleri: etkinlikler, cihazlar, bildirimler
- Dynamics 365 Business Central

## <a name="versions"></a>Sürümler

Microsoft Graph şu anda iki sürümü destekler: v1.0 ve beta. v1.0 sürümü genellikle kullanılabilir API'leri içerir. Tüm üretim uygulamaları için v1.0 sürümünü kullanın. Beta, şu anda önizlemede olan API'leri içerir. Beta API'lerimizde çığır açan değişiklikler getirebileceğimizden, beta sürümünü yalnızca geliştirilmekte olan uygulamaları test etmek için kullanmanızı öneririz; üretim uygulamalarınızda beta API'leri kullanmayın. Daha fazla bilgi için Microsoft [Graph için Sürüm, destek ve kesme değişiklik ilkeleri](https://docs.microsoft.com/graph/versioning-and-support)ne resitiğe bakın.

Beta API'lerini kullanmaya başlamak için [Microsoft Graph beta uç nokta başvurusuna](https://docs.microsoft.com/graph/api/overview?view=graph-rest-beta) bakın

v1.0 API'lerini kullanmaya başlamak için [Microsoft Graph REST API v1.0 başvurusuna](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0) bakın

## <a name="get-started"></a>Kullanmaya başlayın

Kullanıcı veya e-posta iletisi gibi bir kaynaktan okumak veya yazmak için aşağıdakigibi görünen bir istek oluşturuyorsunuz:

`{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}`

Yapılandırılan isteğin öğeleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/graph/use-the-api)

Quickstart örnekleri, Microsoft Graph API'nin gücüne nasıl erişilenleri göstermek için kullanılabilir. Kullanılabilir örnekler, tek bir kimlik doğrulaması olan iki hizmete erişi: Microsoft hesabı ve Outlook. Her hızlı başlangıç Microsoft hesap kullanıcılarının profillerinden bilgilere erişir ve takvimlerindeki olayları görüntüler.
Hızlı başlatmalar dört adım içerir:
- Platformunuzu seçin
- Uygulama kimliğinizi alın (istemci kimliği)
- Örneği oluşturma
- Oturum açın ve takviminizdeki etkinlikleri görüntüleyin

Hızlı başlatmayı tamamladığınızda, çalışmaya hazır bir uygulamanız vardır. Daha fazla bilgi için [Microsoft Graph hızlı başlatSS SSS'ye](https://docs.microsoft.com/graph/quick-start-faq)bakın. Örneklerle başlamak için [Microsoft Graph QuickStart'a](https://developer.microsoft.com/graph/quick-start)bakın.

## <a name="tools"></a>Araçlar

Microsoft Graph Explorer, Microsoft Graph API'lerini kullanarak istekleri oluşturmak ve sınamak için kullanabileceğiniz web tabanlı bir araçtır. Microsoft Graph Explorer'a `https://developer.microsoft.com/graph/graph-explorer`şu andan erişebilirsiniz.

Postacı, Microsoft Graph API'lerini kullanarak istekleri oluşturmak ve sınamak için de kullanabileceğiniz bir araçtır. Postacı'yı şu `https://www.getpostman.com/`telefondan indirebilirsiniz: . Postacı'da Microsoft Graph ile etkileşimde kalmak için Postacı'da Microsoft Graph koleksiyonunu kullanırsınız. Daha fazla bilgi için [Microsoft Graph API'si ile Postacı'yı kullan'a](/graph/use-postman?context=graph%2Fapi%2Fbeta&view=graph-rest-beta)bakın.
