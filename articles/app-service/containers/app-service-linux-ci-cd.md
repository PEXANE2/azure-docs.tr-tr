---
title: Özel Linux kapsayıcılarına CI/CD
description: Azure App Service bir özel Linux kapsayıcısına sürekli dağıtım ayarlamayı öğrenin. Docker Hub ve ACR için sürekli dağıtım desteklenir.
keywords: Azure App Service, Linux, Docker, ACR, OSS
author: msangapu-msft
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.topic: article
ms.date: 11/08/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: d43491de7500204ed470757a1b744017a8180b57
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74687627"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Kapsayıcılar için Web App ile sürekli dağıtım

Bu öğreticide, yönetilen [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) depolarından veya [Docker Hub 'ından](https://hub.docker.com)özel bir kapsayıcı görüntüsü için sürekli dağıtımı yapılandırırsınız.

## <a name="enable-continuous-deployment-with-acr"></a>ACR ile sürekli dağıtımı etkinleştirme

![ACR Web kancasının ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-02.png)

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sol tarafındaki **App Service** seçeneğini belirleyin.
3. Sürekli dağıtımını yapılandırmak istediğiniz uygulamanın adını seçin.
4. **Kapsayıcı ayarları** sayfasında, **tek kapsayıcı** ' yı seçin.
5. **Azure Container Registry** seçin
6. **Üzerinde sürekli dağıtım >** seçin
7. Sürekli dağıtımı etkinleştirmek için **Kaydet** ' i seçin.

## <a name="use-the-acr-webhook"></a>ACR Web kancasını kullanma

Sürekli dağıtım etkinleştirildikten sonra, yeni oluşturulan Web kancasını Azure Container Registry Web kancaları sayfasında görüntüleyebilirsiniz.

![ACR Web kancasının ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-03.png)

Container Registry Web kancaları ' na tıklayarak geçerli Web kancalarını görüntüleyin.

## <a name="enable-continuous-deployment-with-docker-hub-optional"></a>Docker Hub ile sürekli dağıtımı etkinleştir (isteğe bağlı)

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sol tarafındaki **App Service** seçeneğini belirleyin.
3. Sürekli dağıtımını yapılandırmak istediğiniz uygulamanın adını seçin.
4. **Kapsayıcı ayarları** sayfasında, **tek kapsayıcı** ' yı seçin.
5. **Docker Hub 'ı** seçin
6. **Üzerinde sürekli dağıtım >** seçin
7. Sürekli dağıtımı etkinleştirmek için **Kaydet** ' i seçin.

![Uygulama ayarının ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-docker-02.png)

Web kancası URL 'sini kopyalayın. Docker Hub için Web kancası eklemek için <a href="https://docs.docker.com/docker-hub/webhooks/" target="_blank">Docker Hub için Web kancalarını</a>izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux’ta Azure App Service’e Giriş](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md)
* [Linux üzerinde App Service Ruby Web uygulaması oluşturma](quickstart-ruby.md)
* [Kapsayıcılar için Web App’te bir Docker/Go web uygulaması dağıtma](quickstart-docker-go.md)
* [Linux’ta Azure App Service hakkında SSS](./app-service-linux-faq.md)
* [Azure CLI ile Kapsayıcılar için Web App Yönetimi](./app-service-linux-cli.md)
