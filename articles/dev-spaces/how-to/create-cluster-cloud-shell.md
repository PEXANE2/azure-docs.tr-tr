---
title: Azure Dev Spaces için etkin bir Kubernetes kümesi oluşturma Azure Cloud Shell kullanarak
services: azure-dev-spaces
ms.date: 10/04/2018
ms.topic: conceptual
description: Her şeyi yüklemeden doğrudan tarayıcınızdan Azure Dev Spaces için etkin bir Kubernetes kümesini kolayca oluşturmayı öğrenin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
ms.openlocfilehash: dbdc9226e417b3142284386ae3586819cda802d9
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280109"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Azure Cloud Shell kullanarak Kubernetes kümesi oluşturma

Bu sayfadan **deneyin** düğmesini kullanarak bir Azure Kubernetes hizmet kümesi oluşturmak için [Azure Cloud Shell](/azure/cloud-shell) kullanabilirsiniz. Oturum açmadıysanız, bir Azure hesabıyla oturum açmak için istemleri izleyin ve sonra Azure Cloud Shell istemine komutları yazın.

## <a name="create-the-cluster"></a>Kümeyi oluşturma

İlk olarak, [Azure dev Spaces destekleyen bir bölgede][supported-regions]kaynak grubunu oluşturun.

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

Kümenin oluşturulması birkaç dakika sürer.  Tamamlandığında, çıktı JSON biçiminde gösterilir. `provisioningState` bulun ve `Succeeded`doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Tam öğreticilerin bağlantıları için bkz. [Azure dev Spaces](/azure/dev-spaces/) .

> [!IMPORTANT]
> Azure Dev Spaces hızlı başlangıç ve öğreticilerin birçoğu, işlemleri gerçekleştirmek için Azure Dev Spaces CLı kullanır. Azure Dev Spaces CLı 'yi Azure Cloud Shell yükleyemezsiniz.


[supported-regions]: ../about.md#supported-regions-and-configurations