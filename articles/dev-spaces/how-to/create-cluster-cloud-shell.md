---
title: Azure Dev Alanları etkinken Bir Kubernetes kümesi oluşturun - Azure Bulut Bulutu
services: azure-dev-spaces
ms.date: 10/04/2018
ms.topic: conceptual
description: Azure Dev Spaces için etkinleştirilen bir Kubernetes kümesini hiçbir şey yüklemeden doğrudan tarayıcınızdan nasıl hızlı bir şekilde oluşturabilirsiniz öğrenin.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
ms.openlocfilehash: 5e2e5cfd22eeedd3554737458caeca0b891b62fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605310"
---
# <a name="create-a-kubernetes-cluster-with-azure-dev-spaces-enabled-with-azure-cloud-shell"></a>Azure Bulut BulutU Ile Azure Dev Alanları etkinleştirilmiş bir Kubernetes kümesi oluşturun

Azure [Bulut Shell'i](/azure/cloud-shell) bu sayfadaki **Try It** düğmesini kullanarak Bir Azure Kubernetes Hizmeti kümesi oluşturmak için kullanabilirsiniz. Oturum açmadıysanız, bir Azure hesabıyla oturum açma istemlerini izleyin ve göründüğünde Komutları Azure Bulut Bulut Ustem komutuna yazın.

## <a name="create-the-cluster"></a>Kümeyi oluşturma

İlk olarak, [Azure Dev Alanları'nı destekleyen][supported-regions]bir bölgede kaynak grubu oluşturun.

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

Kümenin oluşturulması birkaç dakika sürer.  Tamamlandığında, çıktı JSON biçiminde gösterilir. Bakın `provisioningState` ve `Succeeded`doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Tam öğreticilere bağlantılar için [Azure Dev Alanları'na](/azure/dev-spaces/) bakın.

> [!IMPORTANT]
> Azure Dev Spaces'in çoğu hızlı başlangıçlar ve öğreticiler işlemleri gerçekleştirmek için Azure Dev Spaces CLI'yi kullanır. Azure Geliştirme Alanları CLI'sini Azure Bulut Kabuğu'na yükleyemezsiniz.


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service