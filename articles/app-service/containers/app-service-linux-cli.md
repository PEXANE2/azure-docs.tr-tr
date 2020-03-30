---
title: CLI ile özel Linux kaplarını yönetme
description: Komut satırından Azure Uygulama Hizmeti'ndeki özel Linux kapsayıcılarını nasıl yönetebilirsiniz öğrenin. Uygulama sağlama veya bakım sağlamayı otomatikleştirin.
keywords: azure uygulama hizmeti, web uygulaması, cli, linux, oss
ms.topic: article
ms.date: 08/22/2017
ms.custom: seodec18
ms.openlocfilehash: 5ca5322467402af710df68c82d747f8f8d65e142
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255935"
---
# <a name="manage-web-app-for-containers-using-azure-cli"></a>Azure CLI ile Kapsayıcılar için Web App Yönetimi

Bu makaledeki komutları kullanarak Azure CLI'yi kullanarak Kapsayıcılar için bir Web Uygulaması oluşturabilir ve yönetebilirsiniz.
CLI'nin yeni sürümünü iki şekilde kullanmaya başlayabilirsiniz:

* Azure [CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli) makinenize yükleme.
* Azure Bulut Kabuğunu Kullanma [(Önizleme)](../../cloud-shell/overview.md)

## <a name="create-a-linux-app-service-plan"></a>Linux Uygulama Hizmet Planı Oluşturma

Bir Linux Uygulama Hizmet Planı oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az appservice plan create -n appname -g rgname --is-linux -l "South Central US" --sku S1 --number-of-workers 1
```

## <a name="create-a-custom-docker-container-web-app"></a>Özel bir Docker kapsayıcı Web Uygulaması oluşturma

Bir web uygulaması oluşturmak ve özel bir Docker kapsayıcısı çalıştıracak şekilde yapılandırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```

## <a name="activate-the-docker-container-logging"></a>Docker konteyner günlüğe kaydetmeyi etkinleştirme

Docker kapsayıcı günlüğe kaydetmeyi etkinleştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-app-for-containers-app"></a>Kapsayıcılar Uygulaması için varolan bir Web Uygulaması için özel Docker kapsayıcısını değiştirme

Daha önce oluşturulmuş bir uygulamayı geçerli Docker görüntüsünden yeni bir resme değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
```

## <a name="using-docker-images-from-a-private-registry"></a>Özel kayıt defterinden Docker görüntülerini kullanma

Uygulamanızı özel bir kayıt defterinden görüntüleri kullanacak şekilde yapılandırabilirsiniz. Kayıt defterinizin, kullanıcı adınızın ve parolanızın url'sini sağlamanız gerekir. Bu, aşağıdaki komut kullanılarak elde edilebilir:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
```

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Özel Docker görüntüleri için sürekli dağıtımları etkinleştirme

Aşağıdaki komutla CD işlevselliğini etkinleştirebilir ve webhook url'sini alabilirsiniz. Bu url, DockerHub veya Azure Konteyner Kayıt Defteri depolarını yapılandırmak için kullanılabilir.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

## <a name="create-a-web-app-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Yerleşik çalışma zamanı çerçevelerimizden birini kullanarak Kapsayıcılar Uygulaması için bir Web Uygulaması oluşturun

Kapsayıcılar Uygulaması için bir PHP 5.6 Web Uygulaması oluşturmak için aşağıdaki komutu kullanabilirsiniz.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-app-for-containers-app"></a>Kapsayıcılar Uygulaması için varolan bir Web Uygulaması için çerçeve sürümünü değiştirme

Daha önce oluşturulmuş bir uygulamayı geçerli çerçeve sürümünden Node.js 6.11'e değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
```

## <a name="set-up-git-deployments-for-your-web-app"></a>Web Uygulamanız için Git dağıtımlarını ayarlama

Uygulamanız için Git dağıtımlarını ayarlamak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Linux'ta Azure Uygulama Hizmeti nedir?](app-service-linux-intro.md)
* [Azure CLI'yi yükle](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [Azure Bulut Kabuğu (Önizleme)](../../cloud-shell/overview.md)
* [Azure App Service’te hazırlık ortamları ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Kapsayıcılar için Web Uygulaması ile Sürekli Dağıtım](app-service-linux-ci-cd.md)
