---
title: CLı ile özel Linux kapsayıcılarını yönetme
description: Komut satırından Azure App Service içindeki özel Linux kapsayıcılarını yönetmeyi öğrenin. Uygulama sağlamayı veya bakımını otomatikleştirin.
keywords: Azure App Service, Web uygulaması, CLI, Linux, OSS
ms.topic: article
ms.date: 08/22/2017
ms.custom: seodec18
ms.openlocfilehash: 5ca5322467402af710df68c82d747f8f8d65e142
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78255935"
---
# <a name="manage-web-app-for-containers-using-azure-cli"></a>Azure CLI ile Kapsayıcılar için Web App Yönetimi

Bu makaledeki komutları kullanarak Azure CLı kullanarak bir Kapsayıcılar için Web App oluşturabilir ve yönetebilirsiniz.
CLı 'nın yeni sürümünü iki şekilde kullanmaya başlayabilirsiniz:

* Makinenizde [Azure CLI 'Yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) .
* [Azure Cloud Shell kullanma (Önizleme)](../../cloud-shell/overview.md)

## <a name="create-a-linux-app-service-plan"></a>Linux App Service planı oluşturma

Bir Linux App Service planı oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az appservice plan create -n appname -g rgname --is-linux -l "South Central US" --sku S1 --number-of-workers 1
```

## <a name="create-a-custom-docker-container-web-app"></a>Özel bir Docker kapsayıcısı Web uygulaması oluşturma

Bir Web uygulaması oluşturmak ve özel bir Docker kapsayıcısı çalıştıracak şekilde yapılandırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```

## <a name="activate-the-docker-container-logging"></a>Docker kapsayıcısı günlüğünü etkinleştirme

Docker kapsayıcı günlüğünü etkinleştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-app-for-containers-app"></a>Mevcut bir Kapsayıcılar için Web App uygulaması için özel Docker kapsayıcısını değiştirme

Önceden oluşturulmuş bir uygulamayı, geçerli Docker görüntüsünden yeni bir görüntüye değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
```

## <a name="using-docker-images-from-a-private-registry"></a>Özel bir kayıt defterinden Docker görüntülerini kullanma

Uygulamanızı özel bir kayıt defterinden görüntüleri kullanacak şekilde yapılandırabilirsiniz. Kayıt defteriniz, Kullanıcı adınız ve parolanız için URL sağlamanız gerekir. Bu, aşağıdaki komut kullanılarak sağlanabilir:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
```

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Özel Docker görüntüleri için sürekli dağıtımları etkinleştir

Aşağıdaki komutla CD işlevlerini etkinleştirebilir ve Web kancası URL 'sini alabilirsiniz. Bu URL, size DockerHub veya Azure Container Registry depoları yapılandırmak için kullanılabilir.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

## <a name="create-a-web-app-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Yerleşik çalışma zamanı çerçevelerimizden birini kullanarak bir Kapsayıcılar için Web App uygulaması oluşturun

Kapsayıcılar için Web App bir PHP 5,6 uygulaması oluşturmak için aşağıdaki komutu kullanabilirsiniz.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-app-for-containers-app"></a>Mevcut bir Kapsayıcılar için Web App uygulamasının çerçeve sürümünü değiştirme

Daha önce oluşturulan bir uygulamayı geçerli Framework sürümünden Node.js 6,11 ' e değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
```

## <a name="set-up-git-deployments-for-your-web-app"></a>Web uygulamanız için git dağıtımlarını ayarlama

Uygulamanız için git dağıtımlarını ayarlamak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Azure App Service nedir?](app-service-linux-intro.md)
* [Azure CLı 'yı yükler](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [Azure Cloud Shell (Önizleme)](../../cloud-shell/overview.md)
* [Azure App Service’te hazırlık ortamları ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Kapsayıcılar için Web App ile sürekli dağıtım](app-service-linux-ci-cd.md)
