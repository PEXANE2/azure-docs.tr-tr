---
title: Eğitim koşularında sırları kullanma
titleSuffix: Azure Machine Learning
description: Workspace Key Vault kullanarak güvenli bir şekilde çalışan eğitim için sırları pass
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: d877794abf12b8b412cd1ecf4efd72fd1179d768
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942282"
---
# <a name="use-secrets-in-training-runs"></a>Eğitim koşularında sırları kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, güvenli bir şekilde çalışır eğitim sırlarını nasıl kullanılacağını öğrenirler. Kullanıcı adınız ve parolanız gibi kimlik doğrulama bilgileri sırlardır. Örneğin, eğitim verilerini sorgulamak için harici bir veritabanına bağlanırsanız, kullanıcı adınızı ve parolanızı uzak çalışma bağlamına geçirmeniz gerekir. Bu tür değerleri açık metin olarak eğitim komut dosyalarına kodlamak, sırrı ortaya çıkaracağı için güvensizdir. 

Bunun yerine, Azure Machine Learning çalışma alanınızda [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview)adı verilen ilişkili bir kaynak vardır. Azure Machine Learning Python SDK'daki bir dizi API'den güvenli bir şekilde çalıştırılabilmek için sırları aktarmak için bu Anahtar Kasasını kullanın.

Sırları kullanmak için temel akış:
 1. Yerel bilgisayarda Azure'da oturum açın ve çalışma alanınıza bağlanın.
 2. Yerel bilgisayarda, Workspace Key Vault'ta bir sır ayarlayın.
 3. Uzaktan çalıştırma gönderin.
 4. Uzaktan çalışma da, Key Vault'tan sırrı alın ve kullanın.

## <a name="set-secrets"></a>Sırları ayarlayın

Azure Machine Learning'de [Keyvault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py) sınıfı sırları ayarlamak için yöntemler içerir. Yerel Python oturumunuzda, önce çalışma alanı Anahtar Kasanıza bir [`set_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-) başvuru alın ve ardından ada ve değere göre bir sır ayarlamak için yöntemi kullanın. Ad zaten varsa __set_secret__ yöntemi gizli değeri güncelleştirir.

```python
from azureml.core import Workspace
from azureml.core import Keyvault
import os


ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Python kodunuza gizli değeri dosyada açık metin olarak depolamak güvenli olmadığı için koymayın. Bunun yerine, gizli değeri bir ortam değişkeninden (örneğin Azure DevOps oluşturma gizli veya etkileşimli kullanıcı girişinden) edinin.

Yöntemi kullanarak [`list_secrets()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#list-secrets--) gizli adları listeleyebilirsiniz ve aynı anda birden fazla sır ayarlamanızı sağlayan[set_secrets()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) toplu bir sürüm de vardır.

## <a name="get-secrets"></a>Sırları alın

Yerel kodunuzda, gizli[`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-) değeri ada göre almak için yöntemi kullanabilirsiniz.

Gönderilen çalıştırmalar [`Experiment.submit`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-) için, [`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-) [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) sınıfla birlikte yöntemi kullanın. Gönderilen bir çalıştırma çalışma alanının farkında olduğundan, bu yöntem Çalışma Alanı anlık değerini kısalttır ve gizli değeri doğrudan döndürür.

```python
# Code in submitted run
from azureml.core import Experiment, Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Gizli değeri yazdırarak veya yazdırarak ortaya çıkarmamaya dikkat edin.

Aynı anda birden fazla sırlara erişmek için [get_secrets()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-) bir toplu sürümü de vardır.

## <a name="next-steps"></a>Sonraki adımlar

 * [Örnek not defterini görüntüleme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Azure Machine Learning ile kurumsal güvenlik hakkında bilgi edinin](concept-enterprise-security.md)
