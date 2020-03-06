---
title: AKS üzerinde Azure Dev Spaces etkinleştirme & istemci tarafı araçları 'nı yükler
services: azure-dev-spaces
ms.date: 07/24/2019
ms.topic: conceptual
description: AKS kümesinde Azure Dev Spaces etkinleştirmeyi ve istemci tarafı araçları yüklemeyi öğrenin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
ms.openlocfilehash: 0b7f6cb4a801c84df59bd5157d8c2a1a15eaaf7e
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302908"
---
# <a name="enable-azure-dev-spaces-on-an-aks-cluster-and-install-the-client-side-tools"></a>AKS kümesinde Azure Dev Spaces etkinleştirme ve istemci tarafı araçları 'nı yüklemeye

Bu makalede, bir AKS kümesinde Azure Dev Spaces etkinleştirmenin yanı sıra istemci tarafı araçları yüklemek için kullanabileceğiniz çeşitli yollar gösterilmektedir.

## <a name="enable-azure-dev-spaces-using-the-cli"></a>CLı kullanarak Azure Dev Spaces etkinleştirme

CLı kullanarak dev alanlarını etkinleştirebilmeniz için şunlar gerekir:
* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap][az-portal-create-account] oluşturabilirsiniz.
* [Azure CLI yüklendi][install-cli].
* [Desteklenen bir bölgedeki][supported-regions] [aks kümesi][create-aks-cli] .

AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için `use-dev-spaces` komutunu kullanın.

```azurecli
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

Yukarıdaki komut, *Myresourcegroup* grubundaki *Myakscluster* kümesinde dev alanları sunar ve *varsayılan* bir dev alanı oluşturur.

```console
'An Azure Dev Spaces Controller' will be created that targets resource 'myAKSCluster' in resource group 'myResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'myAKSCluster' in resource group 'myResourceGroup' that targets resource 'myAKSCluster' in resource group 'myResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'myAKSCluster' in resource group 'myResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

`use-dev-spaces` komutu aynı zamanda Azure Dev Spaces CLı 'yi de yüklüyor.

## <a name="enable-azure-dev-spaces-using-the-azure-portal"></a>Azure portal kullanarak Azure Dev Spaces etkinleştirin

Azure portal kullanarak dev alanlarını etkinleştirebilmeniz için önce şunları yapmanız gerekir:
* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap][az-portal-create-account] oluşturabilirsiniz.
* [Desteklenen bir bölgedeki][supported-regions] [aks kümesi][create-aks-portal] .

Azure Dev Spaces Azure portal kullanarak etkinleştirmek için:
1. [Azure Portal][az-portal] oturum açın.
1. AKS kümenize gidin.
1. *Geliştirme alanları* menü öğesini seçin.
1. Değişiklik *geliştirme alanlarını* *Evet* olarak etkinleştirin ve *Kaydet*' e tıklayın.

![Azure portal dev alanlarını etkinleştirme](../media/how-to-setup-dev-spaces/enable-dev-spaces-portal.png)

Azure portal kullanarak Azure Dev Spaces etkinleştirilmesi Azure Dev Spaces için herhangi bir istemci tarafı **Aracı yüklemez.**

## <a name="install-the-client-side-tools"></a>İstemci tarafı araçları 'nı yükler

Yerel makinenizden bir AKS kümesindeki dev Spaces ile etkileşim kurmak için Azure Dev Spaces istemci tarafı araçlarını kullanabilirsiniz. İstemci tarafı araçları yüklemek için çeşitli yollar vardır:

* [Visual Studio Code][vscode], [Azure dev Spaces uzantısını][vscode-extension]yükler.
* [Visual Studio 2019][visual-studio]' de Azure geliştirme iş yükünü yükledikten sonra.
* Visual Studio 2017 ' de Web geliştirme iş yükünü ve [Kubernetes için Visual Studio Araçları][visual-studio-k8s-tools]yüklemesini yapın.
* [Windows][cli-win], [Mac][cli-mac]veya [Linux][cli-linux] CLI 'yı indirip yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces 'de takım geliştirme][team-development-qs]

[create-aks-cli]: ../../aks/kubernetes-walkthrough.md#create-a-resource-group
[create-aks-portal]: ../../aks/kubernetes-walkthrough-portal.md#create-an-aks-cluster
[install-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-development-qs]: ../quickstart-team-development.md

[az-portal]: https://portal.azure.com
[az-portal-create-account]: https://azure.microsoft.com/free
[cli-linux]: https://aka.ms/get-azds-linux
[cli-mac]: https://aka.ms/get-azds-mac
[cli-win]: https://aka.ms/get-azds-windows
[visual-studio]: https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs
[visual-studio-k8s-tools]: https://aka.ms/get-vsk8stools
[vscode]: https://code.visualstudio.com/download
[vscode-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
