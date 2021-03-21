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
ms.custom: azureday1
ms.openlocfilehash: 17931ee92072059d94cd950e9ce8ef6588f7e891
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96905511"
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
