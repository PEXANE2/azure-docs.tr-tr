---
title: Öğretici-Azure App Service güvenli bir Web uygulaması oluşturun | Mavisi
description: Bu öğreticide, Azure App Service kullanarak bir Web uygulaması oluşturma, kimlik doğrulamasını etkinleştirme, Azure depolama 'yı çağırma ve Microsoft Graph çağırma hakkında bilgi edineceksiniz.
services: active-directory, app-service-web, storage, microsoft-graph
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: ef7782c68746d4c20df5a9ae5e27ffca3e60efbe
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024135"
---
# <a name="tutorial-enable-authentication-in-app-service-and-access-storage-and-microsoft-graph"></a>Öğretici: App Service ve erişimli depolama ve Microsoft Graph erişimli kimlik doğrulamasını etkinleştirme

Bu öğreticide, aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

- [Bir Web uygulaması için kimlik doğrulamasını yapılandırın](scenario-secure-app-authentication-app-service.md) ve kuruluşunuzdaki kullanıcılara erişimi sınırlayın. Diyagramda bir öğesine bakın.
- Yönetilen kimlikleri kullanarak Web uygulaması için [Azure depolama Ile güvenli bir şekilde erişin](scenario-secure-app-access-storage.md) . Diyagramda B bölümüne bakın.
- [Oturum açmış kullanıcı](scenario-secure-app-access-microsoft-graph-as-user.md) veya yönetilen kimlikleri kullanan [web uygulaması](scenario-secure-app-access-microsoft-graph-as-app.md) için Microsoft Graph verilere erişin. Diyagramda C bölümüne bakın.
- Bu öğretici için oluşturduğunuz [kaynakları temizleyin](scenario-secure-app-clean-up-resources.md) .

:::image type="content" source="./media/scenario-secure-app-overview/web-app.svg" alt-text="Microsoft Identity platformunda uygulama senaryolarını gösteren diyagram." border="false":::

Başlamak için, bir Web uygulaması için kimlik doğrulamasını nasıl etkinleştireceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Bir Web uygulaması için kimlik doğrulamasını yapılandırma](scenario-secure-app-authentication-app-service.md)
