---
title: Öğretici-Azure Red Hat OpenShift kümesini silme
description: Bu öğreticide, Azure CLı kullanarak bir Azure Red Hat OpenShift kümesini silmeyi öğrenin
author: jimzim
ms.author: jzim
ms.topic: tutorial
ms.service: container-service
ms.date: 05/06/2019
ms.openlocfilehash: c335236a2b0b05f03bef1ebef37f1129a5d0352b
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278767"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Red Hat OpenShift kümesini silme

Öğreticinin sonuna geldiniz. Örnek kümeyi test etmeyi bitirdiğinizde, kullanmadığınız şeyler için ücretlendirilmemek üzere onu ve ilişkili kaynaklarını nasıl silebileceğinizi aşağıda bulabilirsiniz.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini silme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
> * [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)
> * Azure Red Hat OpenShift kümesini silme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md) öğreticisini izleyerek bir küme oluşturun.

## <a name="step-1-sign-in-to-azure"></a>1\. Adım: Azure 'da oturum açma

Azure CLı 'yi yerel olarak çalıştırıyorsanız Azure 'da oturum açmak için `az login` çalıştırın.

```bash
az login
```

Birden çok aboneliğe erişiminiz varsa, `{subscription ID}` yerine kullanmak istediğiniz abonelikle değiştirin `az account set -s {subscription ID}` çalıştırın.

## <a name="step-2-delete-the-cluster"></a>2\. Adım: kümeyi silme

Bir bash terminali açın ve CLUSTER_NAME değişkenini kümenizin adına ayarlayın:

```bash
CLUSTER_NAME=yourclustername
```

Şimdi kümenizi silin:

```bash
az openshift delete --resource-group $CLUSTER_NAME --name $CLUSTER_NAME
```

Kümeyi silmek isteyip istemediğiniz sorulur. `y`doğruladıktan sonra, kümenin silinmesi birkaç dakika sürer. Komut tamamlandığında, küme dahil olmak üzere tüm kaynak grubu ve içindeki tüm kaynaklar silinir.

## <a name="deleting-a-cluster-using-the-azure-portal"></a>Azure portal kullanarak küme silme

Alternatif olarak, çevrimiçi Azure portal, kümenizin ilişkili kaynak grubunu silebilirsiniz. Kaynak grubunun adı, kümenizin adıyla aynıdır.

Şu anda, kümeyi oluştururken oluşturulan `Microsoft.ContainerService/openShiftManagedClusters` kaynak Azure portal gizlenir. `Resource group` görünümünde, kaynak grubunu görüntülemek için `Show hidden types` ' i işaretleyin.

![Gizli tür onay kutusunun ekran görüntüsü](./media/aro-portal-hidden-type.png)

Kaynak grubunun silinmesi, bir Azure Red Hat OpenShift kümesi oluşturduğunuzda oluşturulan tüm ilgili kaynakları silecektir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini silme

Resmi [Red Hat OpenShift belgeleriyle](https://docs.openshift.com/aro/welcome/index.html) OpenShift kullanma hakkında daha fazla bilgi edinin
