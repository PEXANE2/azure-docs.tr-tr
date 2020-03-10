---
title: Eğitim çalışmalarından gizli dizileri kullanma
titleSuffix: Azure Machine Learning
description: Çalışma alanını kullanarak güvenli şekilde, eğitim çalıştırmaları için gizli dizileri geçirin Key Vault
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: d877794abf12b8b412cd1ecf4efd72fd1179d768
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78942282"
---
# <a name="use-secrets-in-training-runs"></a>Eğitim çalışmalarından gizli dizileri kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, eğitimin nasıl gizli bir şekilde çalıştığını öğrenirsiniz. Kullanıcı adınız ve parolanız gibi kimlik doğrulama bilgileri gizli dizilerlerdir. Örneğin, eğitim verilerini sorgulamak için bir dış veritabanına bağlanıyorsanız, Kullanıcı adınızı ve parolanızı uzak çalışma bağlamına geçirmeniz gerekir. Bu tür değerleri şifresiz metin olarak kodlamak, gizli dizi sergilediğinden güvenli değildir. 

Bunun yerine, Azure Machine Learning çalışma alanınızın [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)adlı ilişkili bir kaynağı vardır. Gizli dizileri Azure Machine Learning Python SDK 'sında bir dizi API aracılığıyla güvenli bir şekilde geçirmek için bu Key Vault kullanın.

Gizli dizileri kullanmak için temel akış:
 1. Yerel bilgisayarda Azure 'da oturum açın ve çalışma alanınıza bağlanın.
 2. Yerel bilgisayarda Key Vault çalışma alanında bir gizli dizi belirleyin.
 3. Uzaktan çalıştırma gönder.
 4. Uzaktan çalıştırma içinde Key Vault gizli anahtarı alın ve kullanın.

## <a name="set-secrets"></a>Gizli dizileri ayarla

Azure Machine Learning, [Keykasası](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py) sınıfı gizli dizileri ayarlamaya yönelik yöntemleri içerir. Yerel Python oturumunda, önce çalışma alanınızın Key Vault bir başvurusunu alın ve ardından [`set_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-) metodunu kullanarak bir gizli dizi adı ve değeri ayarlayın. __Set_secret__ yöntemi, ad zaten varsa gizli değeri günceller.

```python
from azureml.core import Workspace
from azureml.core import Keyvault
import os


ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Gizli bir değeri, şifresiz metin olarak depolamak için güvenli olmadığı için Python kodunuza yerleştirmeyin. Bunun yerine, bir ortam değişkeninden (örneğin, Azure DevOps derleme sırrı) veya etkileşimli Kullanıcı girişinden gizli değer elde edin.

[`list_secrets()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#list-secrets--) yöntemi kullanarak gizli adları listeleyebilir ve aynı anda birden çok gizli dizi ayarlamanıza olanak tanıyan bir Batch sürümü[(set_secrets ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) vardır.

## <a name="get-secrets"></a>Gizli dizileri al

Yerel kodunuzda[`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-) yöntemini kullanarak gizli değeri ada göre alabilirsiniz.

[`Experiment.submit`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-) gönderilen çalıştırmalar için [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) sınıfıyla [`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-) yöntemini kullanın. Gönderilen bir çalıştırma çalışma alanının farkında olduğundan, bu yöntem çalışma alanı örneklemesini kısayollar ve gizli değeri doğrudan döndürür.

```python
# Code in submitted run
from azureml.core import Experiment, Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Yazarak veya yazdırarak gizli değeri göstermemeye dikkat edin.

Aynı zamanda birden çok gizli dizi sürümüne erişmek için [get_secrets ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-) Batch sürümü de vardır.

## <a name="next-steps"></a>Sonraki adımlar

 * [Örnek Not defterini görüntüle](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Azure Machine Learning ile kurumsal güvenlik hakkında bilgi edinin](concept-enterprise-security.md)
