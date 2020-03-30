---
title: Depolama hesabı erişim anahtarlarını değiştirme
titleSuffix: Azure Machine Learning
description: Çalışma alanınız tarafından kullanılan Azure Depolama hesabının erişim anahtarlarını nasıl değiştireceğinizi öğrenin. Azure Machine Learning, verileri ve modelleri depolamak için bir Azure Depolama hesabı kullanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 03/06/2020
ms.openlocfilehash: f1541c177cea2d223a5e7df576d95fab7eafb310
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80296931"
---
# <a name="regenerate-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını yeniden oluşturma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning tarafından kullanılan Azure Depolama hesaplarının erişim anahtarlarını nasıl değiştireceğinizi öğrenin. Azure Machine Learning, verileri veya eğitimli modelleri depolamak için depolama hesaplarını kullanabilir.

Güvenlik amacıyla, bir Azure Depolama hesabının erişim anahtarlarını değiştirmeniz gerekebilir. Erişim anahtarını yeniden oluşturduğunuzda, yeni anahtarı kullanmak için Azure Machine Learning'in güncelleştirilmesi gerekir. Azure Machine Learning, hem model depolama için hem de veri deposu olarak depolama hesabını kullanıyor olabilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [çalışma alanı](how-to-manage-workspace.md) oluştur makalesine bakın.

* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [Azure Machine Learning CLI uzantısı.](reference-azure-machine-learning-cli.md)

> [!NOTE]
> Bu belgedeki kod parçacıkları Python SDK'nın 1.0.83 sürümüyle test edilmiştir.

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Güncellenmesi gerekenler

Depolama hesapları Azure Machine Learning çalışma alanı (günlükleri, modelleri, anlık görüntüleri vb. depolama) ve veri deposu olarak kullanılabilir. Çalışma alanını güncelleştirme işlemi tek bir Azure CLI komutudur ve depolama anahtarını güncelleştirdikten sonra çalıştırılabilir. Veri depolarını güncelleştirme işlemi daha fazla ilgilidir ve hangi veri depolarının şu anda depolama hesabını kullandığını ve sonra bunları yeniden kaydettirmelerini keşfetmeyi gerektirir.

> [!IMPORTANT]
> Çalışma alanını Azure CLI'yi ve veri depolarını Python'u kullanarak aynı anda güncelleştirin. Yalnızca birini veya diğerini güncelleştirmek yeterli değildir ve her ikisi de güncelleştirilene kadar hatalara neden olabilir.

Veri depolarınız tarafından kullanılan depolama hesaplarını keşfetmek için aşağıdaki kodu kullanın:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " +
      default_ds.account_name + ", container name: " + default_ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob":
        print("Blob store - datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
    if ds.datastore_type == "AzureFile":
        print("File share - datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
```

Bu kod, Azure Depolama'yı kullanan kayıtlı veri depolarını arar ve aşağıdaki bilgileri listeler:

* Datastore adı: Depolama hesabının kayıtlı olduğu veri deposunun adı.
* Depolama hesabı adı: Azure Depolama hesabının adı.
* Kapsayıcı: Bu kayıt tarafından kullanılan depolama hesabındaki kapsayıcı.

Ayrıca, her veri deposu türünü yeniden kaydetmek için farklı yöntemler olduğundan, veri deposunun Azure Blob'u veya Azure Dosyası paylaşımı için olup olmadığını da gösterir.

Erişim anahtarlarını yenilemeyi planladığınız depolama hesabı için bir giriş varsa, veri deposu adını, depolama hesabı adını ve kapsayıcı adını kaydedin.

## <a name="update-the-access-key"></a>Erişim anahtarını güncelleştir

Azure Machine Learning'i yeni anahtarı kullanmak üzere güncelleştirmek için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> CLI'yi kullanarak hem çalışma alanını hem de Python'u kullanarak veri depolarını güncelleştiren tüm adımları gerçekleştirin. Yalnızca biri veya diğerinin güncelleştirilmesi, her ikisi de güncelleştirilene kadar hatalara neden olabilir.

1. Anahtarı yenile. Erişim anahtarını yenileme hakkında bilgi [için](../storage/common/storage-account-keys-manage.md)bkz. Yeni anahtarı kaydet.

1. Yeni anahtarı kullanmak için çalışma alanını güncelleştirmek için aşağıdaki adımları kullanın:

    1. Aşağıdaki Azure CLI komutunu kullanarak çalışma alanınızı içeren Azure aboneliğinde oturum açmak için:

        ```azurecli-interactive
        az login
        ```

        [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)]

    1. Yeni anahtarı kullanmak için çalışma alanını güncelleştirmek için aşağıdaki komutu kullanın. Azure `myworkspace` Machine Learning çalışma alanı adınızı `myresourcegroup` değiştirin ve çalışma alanını içeren Azure kaynak grubunun adı ile değiştirin.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

        Bu komut, çalışma alanı tarafından kullanılan Azure depolama hesabının yeni anahtarlarını otomatik olarak eşitler.

1. Depolama hesabını kullanan datastore(lar)ı yeniden kaydetmek [için, güncellenmesi gerekenler](#whattoupdate) bölümündeki değerleri ve aşağıdaki kodla 1.

    ```python
    # Re-register the blob container
    ds_blob = Datastore.register_azure_blob_container(workspace=ws,
                                              datastore_name='your datastore name',
                                              container_name='your container name',
                                              account_name='your storage account name',
                                              account_key='new storage account key',
                                              overwrite=True)
    # Re-register file shares
    ds_file = Datastore.register_azure_file_share(workspace=ws,
                                          datastore_name='your datastore name',
                                          file_share_name='your container name',
                                          account_name='your storage account name',
                                          account_key='new storage account key',
                                          overwrite=True)
    
    ```

    `overwrite=True` Belirtildiğiiçin, bu kod varolan kaydın üzerine yazar ve yeni anahtarı kullanmak üzere güncelleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Veri depolarını kaydetme hakkında daha [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) fazla bilgi için sınıf başvurusuna bakın.
