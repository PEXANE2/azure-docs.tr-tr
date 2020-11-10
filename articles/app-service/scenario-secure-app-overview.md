---
title: Öğretici-Azure App Service güvenli bir Web uygulaması oluşturun | Mavisi
description: Bu öğreticide, Azure App Service kullanarak bir Web uygulaması oluşturmayı, kimlik doğrulamasını etkinleştirmenizi, Azure depolama 'yı çağırmayı ve Microsoft Graph çağırmayı öğreneceksiniz.
services: active-directory, app-service-web, storage, microsoft-graph
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: fcddf99c3a4c53fe25db1ed653983e8ddac0edb7
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428994"
---
# <a name="tutorial-enable-authentication-in-app-service-and-access-storage-and-microsoft-graph"></a>Öğretici: App Service ve erişimli depolama ve Microsoft Graph erişimli kimlik doğrulamasını etkinleştirme

Bu öğreticide, genel bir uygulama senaryosu açıklanmakta ve şunları öğreneceksiniz:

- [(A) bir Web uygulaması için kimlik doğrulamasını yapılandırın](scenario-secure-app-authentication-app-service.md) ve kuruluşunuzdaki kullanıcılara erişimi sınırlayın.
- (B) yönetilen kimlikleri kullanarak Web uygulaması adına [Azure depolamaya güvenli bir şekilde erişin](scenario-secure-app-access-storage.md) .
- (C) Microsoft Graph içindeki verileri [, oturum açmış kullanıcı adına](scenario-secure-app-access-microsoft-graph-as-user.md) veya yönetilen kimlikleri kullanarak [Web uygulaması adına](scenario-secure-app-access-microsoft-graph-as-app.md) erişin.
- Bu öğretici için oluşturduğunuz [kaynakları temizleyin](scenario-secure-app-clean-up-resources.md) .

:::image type="content" source="./media/scenario-secure-app-overview/web-app.svg" alt-text="Microsoft Identity platformunda uygulama senaryoları" border="false":::

Başlamak için, bir Web uygulaması için kimlik doğrulamasını nasıl etkinleştireceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Bir Web uygulaması için kimlik doğrulamasını yapılandırma](scenario-secure-app-authentication-app-service.md)
