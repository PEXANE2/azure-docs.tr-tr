---
title: İstemci tarafındaki araçları yüklemek & AKS'de Azure Geliştirme Boşluklarını etkinleştirin
services: azure-dev-spaces
ms.date: 07/24/2019
ms.topic: conceptual
description: AkS kümesinde Azure Geliştirme Alanları'nı nasıl etkinleştirdiğinizi ve istemci tarafındaki araçları nasıl yükleyin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
ms.openlocfilehash: a6b3be5ceba5e60b99b2f75e060f3321cd3151f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898943"
---
# <a name="enable-azure-dev-spaces-on-an-aks-cluster-and-install-the-client-side-tools"></a>AkS kümesinde Azure Dev Spaces'i etkinleştirin ve istemci tarafındaki araçları yükleyin

Bu makalede, bir AKS kümesinde Azure Dev Spaces'i etkinleştirmenin yanı sıra istemci tarafındaki araçları yüklemenin çeşitli yollarını gösterilmektedir.

## <a name="enable-or-remove-azure-dev-spaces-using-the-cli"></a>CLI'yi kullanarak Azure Dev Boşluklarını etkinleştirme veya kaldırma

CLI'yi kullanarak Dev Spaces'i etkinleştirmeden önce şunları yapmanız gerekir:
* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap][az-portal-create-account] oluşturabilirsiniz.
* [Azure CLI yüklendi.][install-cli]
* Desteklenen bir [bölgede][supported-regions]bir [AKS kümesi.][create-aks-cli]

AKS `use-dev-spaces` kümenizde Dev Spaces'i etkinleştirmek ve istemleri izlemek için komutu kullanın.

```azurecli
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

Yukarıdaki komut *myRESOURCEGroup* grubunda *myAKSCluster* kümesinde Dev Spaces sağlar ve *varsayılan* bir dev alanı oluşturur.

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

Komut, `use-dev-spaces` Azure Dev Spaces CLI'yi de yükler.

AkS kümenizden Azure Dev Alanları'nı `azds remove` kaldırmak için komutu kullanın. Örnek:

```azurecli
$ azds remove -g MyResourceGroup -n MyAKS
Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup' will be deleted. This will remove Azure Dev Spaces instrumentation from the target resource for new workloads. Continue? (y/N): y

Deleting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAks' in resource group 'MyResourceGroup' (takes a few minutes)...
```

Yukarıdaki komut, *MyResourceGroup'taki* *MyAKS* kümesinden Azure Dev Alanları'nı kaldırır. Azure Geliştirme Alanları ile oluşturduğunuz tüm ad alanları iş yükleriyle birlikte kalır, ancak bu ad alanlarındaki yeni iş yükleri Azure Dev Alanları ile birlikte kullanılmaz. Ayrıca, Azure Dev Spaces ile enstrümante olan varolan bölmeleri yeniden başlatırsanız, hatalar görebilirsiniz. Bu bölmeler Azure Dev Spaces aracı olmadan yeniden dağıtılmalıdır. Azure Dev Alanları'nı kümenizden tamamen kaldırmak için, Azure Dev Spaces'in etkin olduğu tüm ad alanlarındaki tüm bölmeleri silin.

## <a name="enable-or-remove-azure-dev-spaces-using-the-azure-portal"></a>Azure portalını kullanarak Azure Geliştirme Alanlarını etkinleştirme veya kaldırma

Azure portalını kullanarak Dev Spaces'i etkinleştirmeden önce şunları yapmanız gerekir:
* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap][az-portal-create-account] oluşturabilirsiniz.
* Desteklenen bir [bölgede][supported-regions]bir [AKS kümesi.][create-aks-portal]

Azure portalını kullanarak Azure Geliştirme Alanları'nı etkinleştirmek için:
1. [Azure portalında][az-portal]oturum açın.
1. AKS kümenize gidin.
1. Dev *Spaces* menü öğesini seçin.
1. *Dev Boşlukları* *Evet'e* Etkinleştir'i değiştirin ve *Kaydet'i*tıklatın.

![Azure portalında Dev Boşlukları etkinleştirme](../media/how-to-setup-dev-spaces/enable-dev-spaces-portal.png)

Azure portalını kullanarak Azure Dev Spaces'i etkinleştirmek, Azure Dev Spaces için istemci tarafındaki araçlar **yüklemez.**

AKS kümenizden Azure Dev Alanları'nı kaldırmak *için, Dev Alanları Hayır'a etkinleştir'i* değiştirin ve *Kaydet'i*tıklatın. *No* Azure Geliştirme Alanları ile oluşturduğunuz tüm ad alanları iş yükleriyle birlikte kalır, ancak bu ad alanlarındaki yeni iş yükleri Azure Dev Alanları ile birlikte kullanılmaz. Ayrıca, Azure Dev Spaces ile enstrümante olan varolan bölmeleri yeniden başlatırsanız, hatalar görebilirsiniz. Bu bölmeler Azure Dev Spaces aracı olmadan yeniden dağıtılmalıdır. Azure Dev Alanları'nı kümenizden tamamen kaldırmak için, Azure Dev Spaces'in etkin olduğu tüm ad alanlarındaki tüm bölmeleri silin.

## <a name="install-the-client-side-tools"></a>İstemci tarafındaki araçları yükleme

Azure Dev Spaces istemci tarafındaki araçları kullanarak yerel makinenizden bir AKS kümesindeki dev boşluklarla etkileşimkurabilirsiniz. İstemci tarafı araçlarını yüklemenin birkaç yolu vardır:

* [Visual Studio Code'da][vscode]Azure Dev [Spaces uzantısını][vscode-extension]yükleyin.
* [Visual Studio 2019'da][visual-studio]Azure Geliştirme iş yükünü yükleyin.
* Visual Studio 2017'de, [Kubernetes için][visual-studio-k8s-tools]Web Geliştirme iş yükünü ve Visual Studio Araçlarını yükleyin.
* [Windows,][cli-win] [Mac][cli-mac]veya [Linux][cli-linux] CLI'yi indirin ve kurun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-development-qs]

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
