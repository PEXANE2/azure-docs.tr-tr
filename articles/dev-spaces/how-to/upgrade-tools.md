---
title: Azure Dev Spaces araçlarını yükseltme
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
ms.custom: devx-track-azurecli
description: Azure Dev Spaces komut satırı araçlarını, Visual Studio Code uzantısını ve Visual Studio uzantısını nasıl yükselteceğinizi öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: f17643e6130abbc9d5da8b484144c95b0e803f33
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102199246"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Azure Dev Spaces araçlarını yükseltme

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

Yeni bir sürüm varsa ve zaten Azure Dev Spaces kullanıyorsanız, Azure Dev Spaces istemci araçlarınızı yükseltmeniz gerekebilir.

## <a name="update-the-azure-cli"></a>Azure CLı 'yı güncelleştirme

En son Azure CLı 'yı güncelleştirdiğinizde, dev Spaces CLı uzantısının en son sürümünü de alırsınız.

Önceki sürümü kaldırmanız gerekmez, yalnızca [Azure CLI](/cli/azure/install-azure-cli)'de uygun indirmeyi bulabilirsiniz.


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
