---
title: Öğretici - Azure Kırmızı Şapka OpenShift kümesini ölçeklendirin
description: Azure CLI'yi kullanarak Microsoft Azure Kırmızı Şapka OpenShift kümesini nasıl ölçeklendireceklerini öğrenin
author: jimzim
ms.author: jzim
ms.topic: tutorial
ms.service: container-service
ms.date: 05/06/2019
ms.openlocfilehash: c6334aa20b543dfbf87fedcfe45d54bbcf7a219a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79477026"
---
# <a name="tutorial-scale-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Kırmızı Şapka OpenShift kümesini ölçeklendirin

Bu öğretici, bir dizinin ikinci bölümüdür. Azure CLI'yi kullanarak Microsoft Azure Red Hat OpenShift kümesini nasıl oluşturacağınız, ölçeklendirmenizi ve kaynakları temizlemek için nasıl sildiğinizi öğreneceksiniz.

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Red Hat OpenShift kümesini ölçeklendirin

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
> * Azure Red Hat OpenShift kümesini ölçeklendirme
> * [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure Kırmızı Şapka [OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisini izleyerek bir küme oluşturun.

## <a name="step-1-sign-in-to-azure"></a>Adım 1: Azure'da oturum açın

Azure CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açacak şekilde çalıştırın. `az login`

```azurecli
az login
```

Birden çok aboneye erişiminiz `az account set -s {subscription ID}` varsa, `{subscription ID}` kullanmak istediğiniz abonelikle değiştirin.

## <a name="step-2-scale-the-cluster-with-additional-nodes"></a>Adım 2: Kümeyi ek düğümlerle ölçeklendirin

Bir Bash terminalinden, değişken CLUSTER_NAME kümenizin adına ayarlayın:

```bash
CLUSTER_NAME=yourclustername
```

Şimdi Azure CLI'yi kullanarak kümeyi beş düğüme ölçeklendireyelim:

```azurecli
az openshift scale --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --compute-count 5
```

Birkaç dakika sonra, `az openshift scale` başarıyla tamamlanacak ve ölçeklenmiş küme ayrıntılarını içeren bir JSON belgesi döndürecek.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini ölçeklendirme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)
