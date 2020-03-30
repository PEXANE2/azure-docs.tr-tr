---
title: ÖZEL Linux kaplarına CI/CD
description: Azure Uygulama Hizmeti'nde özel bir Linux kapsayıcısına sürekli dağıtım ayarlamayı öğrenin. Docker Hub ve ACR için sürekli dağıtım desteklenir.
keywords: azure uygulama hizmeti, linux, docker, acr,oss
author: msangapu-msft
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.topic: article
ms.date: 11/08/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: d43491de7500204ed470757a1b744017a8180b57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687627"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Kapsayıcılar için Web App ile sürekli dağıtım

Bu öğreticide, yönetilen [Azure Kapsayıcı Kayıt Defteri](https://azure.microsoft.com/services/container-registry/) depolarından veya Docker [Hub'ından](https://hub.docker.com)özel bir kapsayıcı görüntüsü için sürekli dağıtım yapılandırırsınız.

## <a name="enable-continuous-deployment-with-acr"></a>ACR ile sürekli dağıtımı etkinleştirme

![ACR webhook ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-02.png)

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sayfanın sol tarafındaki **Uygulama Hizmeti** seçeneğini seçin.
3. Sürekli dağıtımı yapılandırmak istediğiniz uygulamanın adını seçin.
4. Kapsayıcı **Ayarları** sayfasında **Tek Kapsayıcı'yı** seçin
5. **Azure Kapsayıcı Kayıt Defteri'ni** seçin
6. **Sürekli Dağıtım >'ı** Seçin
7. Sürekli dağıtımı etkinleştirmek için **Kaydet'i** seçin.

## <a name="use-the-acr-webhook"></a>ACR webhook'u kullanma

Sürekli Dağıtım etkinleştirildikten sonra, yeni oluşturulan web hook'unu Azure Konteyner Kayıt Defteri webhooks sayfanızda görüntüleyebilirsiniz.

![ACR webhook ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-03.png)

Konteyner Kayıt Defterinizde, geçerli webhook'ları görüntülemek için Webhooks'a tıklayın.

## <a name="enable-continuous-deployment-with-docker-hub-optional"></a>Docker Hub ile sürekli dağıtımı etkinleştirin (isteğe bağlı)

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sayfanın sol tarafındaki **Uygulama Hizmeti** seçeneğini seçin.
3. Sürekli dağıtımı yapılandırmak istediğiniz uygulamanın adını seçin.
4. Kapsayıcı **Ayarları** sayfasında **Tek Kapsayıcı'yı** seçin
5. **Docker Hub'ı** seçin
6. **Sürekli Dağıtım >'ı** Seçin
7. Sürekli dağıtımı etkinleştirmek için **Kaydet'i** seçin.

![Uygulama ayarı ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-docker-02.png)

Webhook URL'sini kopyalayın. Docker Hub için bir webhook eklemek <a href="https://docs.docker.com/docker-hub/webhooks/" target="_blank">için, Docker Hub için webhooks</a>izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux’ta Azure App Service’e Giriş](./app-service-linux-intro.md)
* [Azure Kapsayıcı Kayıt Defteri](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md)
* [Linux'ta Uygulama Hizmeti'nde Ruby web uygulaması oluşturma](quickstart-ruby.md)
* [Kapsayıcılar için Web App’te bir Docker/Go web uygulaması dağıtma](quickstart-docker-go.md)
* [Linux’ta Azure App Service hakkında SSS](./app-service-linux-faq.md)
* [Azure CLI ile Kapsayıcılar için Web App Yönetimi](./app-service-linux-cli.md)
