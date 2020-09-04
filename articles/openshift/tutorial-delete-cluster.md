---
title: Öğretici-Azure Red Hat OpenShift kümesini silme
description: Bu öğreticide, Azure CLı kullanarak bir Azure Red Hat OpenShift kümesini silmeyi öğrenin
author: sakthi-vetrivel
ms.custom: fasttrack-edit
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: ae2c1072b9490931609dd61f1cac2a47f1cffef3
ms.sourcegitcommit: 4feb198becb7a6ff9e6b42be9185e07539022f17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89469958"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-4-cluster"></a>Öğretici: Azure Red Hat OpenShift 4 kümesini silme

Bu öğreticide, üç üçüncü kısım, OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesi silinir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini silme


## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, OpenShift web konsolunu kullanarak bir Azure Red Hat OpenShift kümesi oluşturulup bağlanmıştı. Bu adımları yapmadıysanız ve birlikte takip etmek istiyorsanız, [öğretici 1 ile başlayın-Azure Red Hat OpenShift 4 kümesi oluşturun.](tutorial-create-cluster.md)

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLı 'yi yerel olarak çalıştırıyorsanız `az login` Azure 'da oturum açmak için öğesini çalıştırın.

```bash
az login
```

Birden çok aboneliğe erişiminiz varsa, `az account set -s {subscription ID}` kullanmak istediğiniz abonelikle değiştirmeyi çalıştırın `{subscription ID}` .

## <a name="delete-the-cluster"></a>Küme silme

Önceki öğreticilerde aşağıdaki değişkenler ayarlanmıştır.

```bash
CLUSTER=yourclustername
RESOURCE_GROUP=yourresourcegroup
```

Bu değerleri kullanarak kümenizi silin:

```bash
az aro delete --resource-group $RESOURCE_GROUP --name $CLUSTER
```

Daha sonra kümeyi silmek isteyip istemediğinizi onaylamanız istenir. İle doğruladıktan sonra `y` , kümenin silinmesi birkaç dakika sürer. Komut tamamlandığında, tüm kaynak grubu ve içindeki tüm kaynaklar (küme dahil) silinir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift 4 kümesini silme

Resmi [Red Hat OpenShift belgeleriyle](https://www.openshift.com/try) OpenShift kullanma hakkında daha fazla bilgi edinin