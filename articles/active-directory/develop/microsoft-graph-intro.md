---
title: API Microsoft Graph | Microsoft Docs
description: Microsoft Graph API 'SI, Microsoft Bulut hizmet kaynaklarına erişmenize olanak tanıyan bir yeniden takip eden Web API 'sidir.
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
ms.openlocfilehash: 65465c6897853617f7f33a989f97d01fb0518a5e
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304426"
---
# <a name="microsoft-graph-api"></a>Microsoft Graph API 'SI

Microsoft Graph API 'SI, Microsoft Bulut hizmet kaynaklarına erişmenize olanak tanıyan bir yeniden takip eden Web API 'sidir. Uygulamanızı kaydettikten ve bir kullanıcı veya hizmet için kimlik doğrulama belirteçleri aldıktan sonra, Microsoft Graph API 'sine istek yapabilirsiniz. Daha fazla bilgi için bkz. [Microsoft Graph genel bakış](https://docs.microsoft.com/graph/overview).

Microsoft Graph, aşağıdaki Microsoft 365 hizmetlerindeki verilere erişmek için REST API 'Leri ve istemci kitaplıklarını kullanıma sunar:
- Office 365 hizmetleri: Delve, Excel, Microsoft kayıtları, Microsoft ekipleri, OneDrive, OneNote, Outlook/Exchange, Planner ve SharePoint
- Enterprise Mobility and Security Services: Gelişmiş tehdit analizi, Gelişmiş tehdit koruması, Azure Active Directory, Identity Manager ve Intune
- Windows 10 Hizmetleri: Etkinlikler, cihazlar, bildirimler
- Dynamics 365 Iş Merkezi

## <a name="versions"></a>Sürümler

Microsoft Graph Şu anda iki sürümü desteklemektedir: v 1.0 ve Beta. V 1.0 sürümü, genel olarak kullanılabilir API 'Leri içerir. Tüm üretim uygulamaları için v 1.0 sürümünü kullanın. Beta, şu anda önizleme aşamasında olan API 'Leri içerir. Beta API 'lerimiz üzerinde önemli değişiklikler sunabileceğimizden, yalnızca geliştirme aşamasında olan uygulamaları test etmek için beta sürümünü kullanmanızı öneririz. üretim uygulamalarınızda Beta API 'Lerini kullanmayın. Daha fazla bilgi için, bkz. [Microsoft Graph Için sürüm oluşturma, destek ve bölme değiştirme ilkeleri](https://docs.microsoft.com/graph/versioning-and-support).

Beta API 'Lerini kullanmaya başlamak için bkz. [Microsoft Graph Beta uç noktası başvurusu](https://docs.microsoft.com/graph/api/overview?view=graph-rest-beta)

V 1.0 API 'Lerini kullanmaya başlamak için bkz. [Microsoft Graph REST API v 1.0 başvurusu](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)

## <a name="get-started"></a>başlarken

Kullanıcı veya e-posta iletisi gibi bir kaynağı okumak veya bir kaynağa yazmak için aşağıdaki gibi görünen bir istek oluşturursunuz:

`{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}`

Oluşturulan isteğin öğeleri hakkında daha fazla bilgi için bkz [. MICROSOFT Graph API 'Sini kullanma](https://docs.microsoft.com/graph/use-the-api)

Hızlı başlangıç örnekleri, Microsoft Graph API 'sinin gücüne nasıl erişekullanacağınızı göstermek için kullanılabilir. Kullanılabilir örnekler, bir kimlik doğrulaması ile iki hizmete erişir: Microsoft hesabı ve Outlook. Her hızlı başlangıç, Microsoft hesabı kullanıcıların profillerinin bilgilerine erişir ve takviminden olayları görüntüler.
Hızlı başlangıçlarda dört adım vardır:
- Platformunuzu seçin
- Uygulama KIMLIĞINIZI (istemci KIMLIĞI) alın
- Örneği oluşturma
- Oturum açın ve takviminizde olayları görüntüleyin

Hızlı başlangıcı tamamladığınızda, çalıştırmaya hazırlamış bir uygulamanız vardır. Daha fazla bilgi için [Microsoft Graph hızlı başlangıç hakkında SSS](https://docs.microsoft.com/graph/quick-start-faq)bölümüne bakın. Örnekleri kullanmaya başlamak için bkz. [Microsoft Graph hızlı başlangıç](https://developer.microsoft.com/graph/quick-start).

## <a name="tools"></a>Araçlar

Microsoft Graph Explorer, Microsoft Graph API 'Leri kullanarak istek derlemek ve test etmek için kullanabileceğiniz web tabanlı bir araçtır. Microsoft Graph Explorer 'a şu adreste erişebilirsiniz: `https://developer.microsoft.com/graph/graph-explorer`.

Postman, Microsoft Graph API 'Lerini kullanarak istek derlemek ve test etmek için de kullanabileceğiniz bir araçtır. Postman 'ı şu adresten indirebilirsiniz: `https://www.getpostman.com/`. Postman 'da Microsoft Graph etkileşimde bulunmak için Postman 'da Microsoft Graph koleksiyonunu kullanırsınız. Daha fazla bilgi için bkz. [MICROSOFT Graph API Ile Postman kullanma](https://docs.microsoft.comgraph/use-postman?context=graph%2Fapi%2Fbeta&view=graph-rest-beta).
