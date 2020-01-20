---
title: Öğretici-Azure Red Hat OpenShift kümesini ölçeklendirme
description: Azure CLı kullanarak Microsoft Azure Red Hat OpenShift kümesini ölçeklendirmeyi öğrenin
author: jimzim
ms.author: jzim
ms.topic: tutorial
ms.service: container-service
ms.date: 05/06/2019
ms.openlocfilehash: bf9172f0c84834c951446520ff0bfcc3ef756c9c
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278316"
---
# <a name="tutorial-scale-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Red Hat OpenShift kümesini ölçeklendirme

Bu öğretici, bir dizinin ikinci bölümüdür. Azure CLı kullanarak Microsoft Azure Red Hat OpenShift kümesi oluşturmayı öğrenirsiniz, ölçeklendirerek kaynakları temizleyebilirsiniz.

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Red Hat OpenShift kümesini ölçeklendirme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
> * Azure Red Hat OpenShift kümesini ölçeklendirme
> * [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md) öğreticisini izleyerek bir küme oluşturun.

## <a name="step-1-sign-in-to-azure"></a>1\. Adım: Azure 'da oturum açma

Azure CLı 'yi yerel olarak çalıştırıyorsanız Azure 'da oturum açmak için `az login` çalıştırın.

```bash
az login
```

Birden çok aboneliğe erişiminiz varsa, `{subscription ID}` yerine kullanmak istediğiniz abonelikle değiştirin `az account set -s {subscription ID}` çalıştırın.

## <a name="step-2-scale-the-cluster-with-additional-nodes"></a>2\. Adım: kümeyi ek düğümlerle ölçeklendirme

Bash terminalinde, CLUSTER_NAME değişkenini kümenizin adına ayarlayın:

```bash
CLUSTER_NAME=yourclustername
```

Şimdi de Azure CLı 'yı kullanarak kümeyi beş düğüme ölçeklendirelim:

```bash
az openshift scale --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --compute-count 5
```

Birkaç dakika sonra, `az openshift scale` başarıyla tamamlanır ve ölçeklendirilmiş küme ayrıntılarını içeren bir JSON belgesi döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini ölçeklendirme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)
