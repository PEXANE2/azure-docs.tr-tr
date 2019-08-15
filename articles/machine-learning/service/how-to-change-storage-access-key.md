---
title: Depolama hesabı erişim anahtarlarını değiştirme
titleSuffix: Azure Machine Learning service
description: Çalışma alanınız tarafından kullanılan Azure depolama hesabı için erişim anahtarlarını değiştirme hakkında bilgi edinin. Azure Machine Learning hizmeti, verileri ve modelleri depolamak için bir Azure depolama hesabı kullanır. Depolama hesabı için erişim anahtarını yeniden oluşturduğunuzda, yeni anahtarları kullanmak için Azure Machine Learning hizmetini güncelleştirmeniz gerekir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/08/2019
ms.openlocfilehash: 7c6b85bd1f5935fb3722f82efcdfc921fc9cb2ec
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990549"
---
# <a name="regenerate-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını yeniden oluştur

Azure Machine Learning hizmeti tarafından kullanılan Azure depolama hesapları için erişim anahtarlarını değiştirme hakkında bilgi edinin. Azure Machine Learning, verileri veya eğitilen modelleri depolamak için depolama hesaplarını kullanabilir.

Güvenlik nedeniyle, bir Azure depolama hesabının erişim anahtarlarını değiştirmeniz gerekebilir. Erişim anahtarını yeniden oluşturduğunuzda Azure Machine Learning yeni anahtarı kullanmak için güncelleştirilmeleri gerekir. Azure Machine Learning, hem model depolama hem de bir veri deposu olarak depolama hesabı kullanıyor olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](how-to-manage-workspace.md) makalesine bakın.

* [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [Azure MACHINE LEARNING CLI uzantısı](reference-azure-machine-learning-cli.md).

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Nelerin güncelleştirilmesi gerekiyor

Depolama hesapları Azure Machine Learning hizmet çalışma alanı tarafından (Günlükler, modeller, anlık görüntüler vb.) ve bir veri deposu olarak kullanılabilir. Çalışma alanını güncelleştirme işlemi tek bir Azure CLı komutu ve depolama anahtarı güncelleştirildikten sonra çalıştırılabilir. Veri depolarını güncelleştirme işlemi daha fazla yer alır ve şu anda depolama hesabını kullanan veri depolarının keşfedilmelerini ve sonra yeniden kaydettirmesini gerektirir.

> [!IMPORTANT]
> Azure CLı kullanarak çalışma alanını ve aynı zamanda Python kullanarak veri depolarını güncelleştirin. Yalnızca birini veya diğerini güncelleştirmek yeterli değildir ve her ikisi de güncelleştirilene kadar hatalara neden olabilir.

Veri depfederler tarafından kullanılan depolama hesaplarını öğrenmek için aşağıdaki kodu kullanın:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " +
      default_ds.account_name + ", container name: " + ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob" or ds.datastore_type == "AzureFile":
        print("datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
```

Bu kod, Azure Storage kullanan kayıtlı veri depolarına bakar ve aşağıdaki bilgileri listeler:

* Veri deposu adı: Depolama hesabının kaydedildiği veri deposunun adı.
* Depolama hesabı adı: Azure depolama hesabının adı.
* Kapsayıcı Depolama hesabındaki kapsayıcı bu kayıt tarafından kullanılır.

İçin erişim anahtarlarını yeniden üretmeyi planladığınız depolama hesabı için bir giriş varsa, veri deposu adını, depolama hesabı adını ve kapsayıcı adını kaydedin.

## <a name="update-the-access-key"></a>Erişim anahtarını güncelleştirme

Yeni anahtarı kullanmak üzere Azure Machine Learning hizmetini güncelleştirmek için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Tüm adımları gerçekleştirin, CLı kullanarak çalışma alanını ve Python kullanarak veri depolarını güncelleştirme. Yalnızca bir veya diğerini güncelleştirmek, her ikisi de güncelleştirilene kadar hatalara neden olabilir.

1. Anahtarı yeniden oluşturun. Bir erişim anahtarı yeniden oluşturma hakkında bilgi için, [Depolama hesabını yönetme](/azure/storage/common/storage-account-manage#access-keys) makalesine bakın. Yeni anahtarı kaydedin.

1. Yeni anahtarı kullanmak üzere çalışma alanını güncelleştirmek için aşağıdaki adımları kullanın:

    1. Aşağıdaki Azure CLı komutunu kullanarak çalışma alanınızı içeren Azure aboneliğinde oturum açın:

        ```azurecli-interactive
        az login
        ```

    1. Yeni anahtarı kullanmak üzere çalışma alanını güncelleştirmek için aşağıdaki komutu kullanın. Azure Machine Learning `myworkspace` çalışma alanı adınızla değiştirin ve öğesini, çalışma `myresourcegroup` alanını içeren Azure Kaynak grubunun adıyla değiştirin.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        [!INCLUDE [install extension](../../../includes/machine-learning-service-install-extension.md)]

        Bu komut, çalışma alanı tarafından kullanılan Azure depolama hesabı için yeni anahtarları otomatik olarak eşitler.

1. Depolama hesabını kullanan veri depolarınızı yeniden kaydettirmek için, [güncelleştirilmesi gereken](#whattoupdate) Özellikler bölümünde bulunan değerleri ve adım 1 ' deki anahtarı aşağıdaki kodla kullanın:

    ```python
    ds = Datastore.register_azure_blob_container(workspace=ws, 
                                              datastore_name='your datastore name', 
                                              container_name='your container name',
                                              account_name='your storage account name', 
                                              account_key='new storage account key',
                                              overwrite=True)
    ```

    `overwrite=True` Belirtildiği için, bu kod var olan kaydın üzerine yazar ve yeni anahtarı kullanacak şekilde günceller.

## <a name="next-steps"></a>Sonraki adımlar

Veri depolarını kaydetme hakkında daha fazla bilgi için bkz [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) . sınıf başvurusu.
