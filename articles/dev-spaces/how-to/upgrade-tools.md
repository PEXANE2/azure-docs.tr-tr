---
title: Azure Dev Spaces araçlarını yükseltme
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Azure Dev Spaces komut satırı araçlarını, Visual Studio Code uzantısını ve Visual Studio uzantısını nasıl yükselteceğinizi öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: 748ed4699591a21a5a09da924e093011b0813b46
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79265239"
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

## <a name="update-the-visual-studio-extension"></a>Visual Studio uzantısını güncelleştirme

Diğer uzantılar ve güncelleştirmelerle benzer şekilde, Visual Studio, Azure Dev Spaces de içeren Kubernetes için Visual Studio Araçları için kullanılabilir bir güncelleştirme olduğunda size bildirimde bulunur. Ekranın sağ üst kısmındaki bayrak simgesine bakın.

Visual Studio 'daki araçları güncelleştirmek için **araçlar > Uzantılar ve güncelleştirmeler** menü öğesini seçin ve sol tarafta **güncelleştirmeler**' i seçin. **Kubernetes için Visual Studio Araçları** bulun ve **Güncelleştir** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir küme oluşturarak yeni araçları test edin. [Azure dev Spaces](/azure/dev-spaces)' de hızlı başlangıç ve öğreticiler deneyin.
