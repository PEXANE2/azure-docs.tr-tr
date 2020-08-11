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
ms.openlocfilehash: b168328f64f599de109dbd0a5bd95c0a26f5f902
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88083236"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Kapsayıcılar için Web App ile sürekli dağıtım

Bu öğreticide, yönetilen [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) depolarından veya [Docker Hub 'ından](https://hub.docker.com)özel bir kapsayıcı görüntüsü için sürekli dağıtımı yapılandırırsınız.

## <a name="enable-continuous-deployment-with-acr"></a>ACR ile sürekli dağıtımı etkinleştirme

![ACR Web kancasının ekran görüntüsü](./media/deploy-ci-cd-custom-container/ci-cd-acr-02.png)

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sayfanın sol tarafındaki **App Service** seçeneğini belirleyin.
3. Sürekli dağıtımını yapılandırmak istediğiniz uygulamanın adını seçin.
4. **Kapsayıcı ayarları** sayfasında, **tek kapsayıcı** ' yı seçin.
5. **Azure Container Registry** seçin
6. **Üzerinde sürekli dağıtım >** seçin
7. Sürekli dağıtımı etkinleştirmek için **Kaydet** ' i seçin.

## <a name="use-the-acr-webhook"></a>ACR Web kancasını kullanma

Sürekli dağıtım etkinleştirildikten sonra, yeni oluşturulan Web kancasını Azure Container Registry Web kancaları sayfasında görüntüleyebilirsiniz.

![ACR Web kancasının ekran görüntüsü](./media/deploy-ci-cd-custom-container/ci-cd-acr-03.png)

Container Registry Web kancaları ' na tıklayarak geçerli Web kancalarını görüntüleyin.

## <a name="enable-continuous-deployment-with-docker-hub-optional"></a>Docker Hub ile sürekli dağıtımı etkinleştir (isteğe bağlı)

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sayfanın sol tarafındaki **App Service** seçeneğini belirleyin.
3. Sürekli dağıtımını yapılandırmak istediğiniz uygulamanın adını seçin.
4. **Kapsayıcı ayarları** sayfasında, **tek kapsayıcı** ' yı seçin.
5. **Docker Hub 'ı** seçin
6. **Üzerinde sürekli dağıtım >** seçin
7. Sürekli dağıtımı etkinleştirmek için **Kaydet** ' i seçin.

![Uygulama ayarının ekran görüntüsü](./media/deploy-ci-cd-custom-container/ci-cd-docker-02.png)

Web kancası URL 'sini kopyalayın. Docker Hub için Web kancası eklemek için <a href="https://docs.docker.com/docker-hub/webhooks/" target="_blank">Docker Hub için Web kancalarını</a>izleyin.

## <a name="automate-with-cli"></a>CLı ile otomatikleştirin

Azure CLı kullanarak CI/CD 'yi yapılandırmak için, Web kancası URL 'sini oluşturmak için [az WebApp Deployment Container config](https://docs.microsoft.com/cli/azure/webapp/deployment/container?view=azure-cli-latest#az-webapp-deployment-container-config) komutunu çalıştırın. URL, DockerHub 'ınızı veya Azure Container Registry yapılandırmak için kullanılabilir.

```azurecli-interactive
az webapp deployment container config --name <app-name> --resource-group <group-name> --enable-cd true
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md?pivots=platform-linux)
* [Linux üzerinde App Service Ruby Web uygulaması oluşturma](quickstart-ruby.md)
* [Hızlı başlangıç: App Service özel kapsayıcı çalıştırma](quickstart-custom-container.md?pivots=container-linux)
* [Linux’ta App Service hakkında SSS](faq-app-service-linux.md)
* [Özel Linux kapsayıcıları yapılandırma](configure-custom-container.md)
