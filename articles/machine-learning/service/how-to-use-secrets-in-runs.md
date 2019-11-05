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
ms.date: 08/23/2019
ms.custom: seodec18
ms.openlocfilehash: c0d696e3fc060a2779eba7d7e895397ea3245383
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489285"
---
# <a name="use-secrets-in-training-runs"></a>Eğitim çalışmalarından gizli dizileri kullanma
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, eğitimin nasıl gizli bir şekilde çalıştığını öğrenirsiniz. Örneğin, eğitim verilerini sorgulamak üzere bir dış veritabanına bağlanmak için, uzak çalışma bağlamına Kullanıcı adı ve parola geçirmeniz gerekir. Bu tür değerlerin, şifresiz metin 'teki eğitim betiklerine kodlanması, gizli dizi ortaya çıkarmak için güvenli değildir. 

Bunun yerine, Azure Machine Learning Çalışma Alanı ilişkili kaynak olarak [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) . Bu Key Vault, Azure Machine Learning Python SDK 'daki bir API kümesi aracılığıyla uzak çalışmaların uzaktan çalışmasına karşı güvenli şekilde geçirilmesi için kullanılabilir

Gizli dizileri kullanmak için temel akış:
 1. Yerel bilgisayarda Azure 'da oturum açın ve çalışma alanınıza bağlanın
 2. Yerel bilgisayarda, çalışma alanında bir gizli dizi ayarlayın Key Vault
 3. Uzaktan çalıştırma gönder.
 4. Uzaktan çalıştırma içinde anahtar değerinden parolayı alın ve kullanın.

## <a name="set-secrets"></a>Gizli dizileri ayarla

Azure Machine Learning Python SDK 'da, [Anahtar Kasası](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py) sınıfı gizli dizileri ayarlamaya yönelik yöntemleri içerir. Yerel Python oturumunda, önce Key Vault çalışma alanı için bir başvuru alın ve ardından [set_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-) yöntemini kullanarak bir gizli dizi adı ve değeri ayarlayın.

```python
from azureml.core import Workspace
import os

ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Gizli bir değeri, şifresiz metin olarak depolamak için güvenli olmadığı için Python koduna yerleştirmeyin. Bunun yerine, ortam değişkeninden, örneğin Azure DevOps derleme gizliliğine veya etkileşimli Kullanıcı girişinden gizli değer elde edin.

[List_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-) yöntemini kullanarak gizli adları listeleyebilirsiniz. Ad zaten varsa, __set_secret__ yöntemi gizli değeri günceller.

## <a name="get-secrets"></a>Gizli dizileri al

Yerel kodunuzda, [Anahtar Kasası. Get _secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-) metodunu kullanarak gizli değeri ada göre alabilirsiniz.

[Denemeler. Gönder](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-)kullanılarak gönderilen çalıştırmalar, [Run. Get _secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-) metodunu kullanın. Gönderilen çalıştırma çalışma alanının farkında olduğundan, bu yöntem çalışma alanı örneklemesini kısayollar ve gizli değeri doğrudan döndürür.

```python
# Code in submitted run
from azureml.core import Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Yazarak veya yazdırarak gizli değeri göstermemeye dikkat edin.

Set ve Get yöntemlerinde aynı anda birden çok gizli dizi için [set_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) ve [get_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-) toplu iş sürümleri de vardır.

## <a name="next-steps"></a>Sonraki adımlar

 * [Örnek Not defterini görüntüle](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Azure Machine Learning ile kurumsal güvenlik hakkında bilgi edinin](concept-enterprise-security.md)
