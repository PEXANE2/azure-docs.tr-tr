---
title: Azure Dev Spaces araçlarını yükseltme
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Azure Dev Spaces komut satırı araçlarını, Visual Studio Code uzantısını ve Visual Studio uzantısını nasıl yükselteceğinizi öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: af4ef92d335c350042b74ccfbfaf5266615f92a6
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229286"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Azure Dev Spaces araçlarını yükseltme

Yeni bir sürüm varsa ve zaten Azure Dev Spaces kullanıyorsanız, Azure Dev Spaces istemci araçlarınızı yükseltmeniz gerekebilir.

## <a name="update-the-azure-cli"></a>Azure CLı 'yı güncelleştirme

En son Azure CLı 'yı güncelleştirdiğinizde, dev Spaces CLı uzantısının en son sürümünü de alırsınız.

Önceki sürümü kaldırmanız gerekmez, yalnızca [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)'de uygun indirmeyi bulabilirsiniz.


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Geliştirme alanları CLı uzantısını ve komut satırı araçlarını güncelleştirme

Şu komutu çalıştırın:

```azurecli
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>VS Code uzantısını güncelleştirme

Yüklendikten sonra uzantı otomatik olarak güncelleştirilir. Yeni özellikleri kullanmak için uzantıyı yeniden yüklemeniz gerekebilir. VS Code, **Uzantılar** bölmesini açın, **Azure dev Spaces** uzantıları ' nı seçin ve **yeniden yükle**' yi seçin.

## <a name="update-visual-studio"></a>Visual Studio’yu güncelleştirme

Azure Dev Spaces, Azure geliştirme iş yükünün bir parçasıdır ve tüm Visual Studio güncelleştirmelerine dahildir.

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir küme oluşturarak yeni araçları test edin. [Azure dev Spaces](../index.yml)' de hızlı başlangıç ve öğreticiler deneyin.
