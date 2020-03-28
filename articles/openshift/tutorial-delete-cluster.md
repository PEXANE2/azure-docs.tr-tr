---
title: Öğretici - Azure Kırmızı Şapka OpenShift kümesini silme
description: Bu eğitimde, Azure CLI'yi kullanarak Azure Kırmızı Şapka OpenShift kümesini nasıl sileyizilen öğrenin
author: jimzim
ms.author: jzim
ms.topic: tutorial
ms.service: container-service
ms.date: 05/06/2019
ms.openlocfilehash: c335236a2b0b05f03bef1ebef37f1129a5d0352b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76278767"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Kırmızı Şapka OpenShift kümesini silme

Öğreticinin sonuna geldiniz. Örnek kümeyi test etmeyi tamamladığınızda, kullanmadığınız şey için ücretlendirilmemek için bu kümeyi ve ilişkili kaynaklarını şu şekilde silebilirsiniz.

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

* Azure Kırmızı Şapka [OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisini izleyerek bir küme oluşturun.

## <a name="step-1-sign-in-to-azure"></a>Adım 1: Azure'da oturum açın

Azure CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açacak şekilde çalıştırın. `az login`

```bash
az login
```

Birden çok aboneye erişiminiz `az account set -s {subscription ID}` varsa, `{subscription ID}` kullanmak istediğiniz abonelikle değiştirin.

## <a name="step-2-delete-the-cluster"></a>Adım 2: Kümeyi silme

Bir Bash terminali açın ve değişken CLUSTER_NAME kümenizin adına ayarlayın:

```bash
CLUSTER_NAME=yourclustername
```

Şimdi kümenizi silin:

```bash
az openshift delete --resource-group $CLUSTER_NAME --name $CLUSTER_NAME
```

Kümeyi silmek isteyip istemediğiniz istenir. Ile onayladıktan `y`sonra, kümeyi silmek birkaç dakika sürer. Komut bittiğinde, tüm Kaynak Grubu ve küme de dahil olmak üzere içindeki tüm kaynaklar silinir.

## <a name="deleting-a-cluster-using-the-azure-portal"></a>Azure portalını kullanarak küme silme

Alternatif olarak, çevrimiçi Azure portalı üzerinden kümenizin ilişkili kaynak grubunu silebilirsiniz. Kaynak grubunun adı küme adınız ile aynıdır.

Şu anda, küme oluştururken oluşturulan `Microsoft.ContainerService/openShiftManagedClusters` kaynak Azure portalında gizlidir. Görünümde, `Resource group` kaynak `Show hidden types` grubunu görüntülemek için denetleyin.

![Gizli tür onay kutusunun ekran görüntüsü](./media/aro-portal-hidden-type.png)

Kaynak grubunu silmek, bir Azure Red Hat OpenShift kümesi oluşturduğunuzda oluşturulan ilgili tüm kaynakları siler.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini silme

Resmi [Red Hat OpenShift belgeleriyle](https://docs.openshift.com/aro/welcome/index.html) OpenShift'i kullanma hakkında daha fazla bilgi edinin
