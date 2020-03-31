---
title: Azure Geliştirme Alanları araçlarını yükseltme
services: azure-dev-spaces
ms.date: 07/03/2018
ms.topic: conceptual
description: Azure Dev Spaces komut satırı araçlarını, Visual Studio Code uzantısını ve Visual Studio uzantısını nasıl yükselteceklerini öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Konteyner Hizmeti, konteynerler
ms.openlocfilehash: 748ed4699591a21a5a09da924e093011b0813b46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265239"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Azure Geliştirme Alanları araçlarını yükseltme

Yeni bir sürüm varsa ve Azure Dev Spaces'i zaten kullanıyorsanız, Azure Dev Spaces istemci araçlarını yükseltmeniz gerekebilir.

## <a name="update-the-azure-cli"></a>Azure CLI'yi güncelleştirme

En son Azure CLI'yi güncellediğinizde, Dev Spaces CLI uzantısının en son sürümünü de alırsınız.

Önceki sürümü kaldırmanız gerekmez, azure [CLI'de](/cli/azure/install-azure-cli?view=azure-cli-latest)uygun karşıdan yüklemeyi bulmanız gerekir.


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Dev Spaces CLI uzantısı nı ve komut satırı araçlarını güncelleştirme

Şu komutu çalıştırın:

```azurecli
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>VS Kodu uzantısını güncelleştirin

Yüklendikten sonra uzantı otomatik olarak güncellenir. Yeni özellikleri kullanmak için uzantıyı yeniden yüklemeniz gerekebilir. VS Code'da **Uzantılar** bölmesini açın, **Azure Geliştirme Alanları** uzantılarını seçin ve Yeniden **Yükle'yi**seçin.

## <a name="update-the-visual-studio-extension"></a>Visual Studio uzantısını güncelleştirin

Diğer uzantılar ve güncellemeler gibi Visual Studio da Kubernetes için Visual Studio Tools'ta Azure Dev Alanları içeren bir güncelleme olduğunda sizi bilgilendirecektir. Ekranın sağ üst kısmında bir bayrak simgesi arayın.

Visual Studio'daki araçları güncelleştirmek **için, Araçlar > Uzantıları ve Güncelleştirmeleri** menü öğesini seçin ve sol tarafta **Güncellemeler'i**seçin. **Kubernetes için Visual Studio Araçlarını** bulun ve **Güncelleştir** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir küme oluşturarak yeni araçları test edin. [Azure Dev Spaces'te](/azure/dev-spaces)hızlı başlangıçları ve öğreticileri deneyin.
