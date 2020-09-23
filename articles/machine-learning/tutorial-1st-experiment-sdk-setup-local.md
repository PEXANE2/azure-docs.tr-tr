---
title: 'Öğretici: çalışmaya başlama makine öğrenimi-Python'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, kişisel geliştirme ortamınızda çalışan Azure Machine Learning Python SDK 'sını kullanmaya başlayacaksınız.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: 48edc138e7ab16e712339d6291db52a4a2b36b32
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946753"
---
# <a name="tutorial-get-started-with-azure-machine-learning-on-your-development-environment-part-1-of-4"></a>Öğretici: geliştirme ortamınızda Azure Machine Learning kullanmaya başlayın (Bölüm 1/4)

Bu **dört bölümden oluşan öğretici serisinde**, Azure bulutu 'nda aşağıdakiler de dahil olmak üzere Azure Machine Learning ve aşağıdaki Iş tabanlı Python ml görevlerinin temellerini öğreneceksiniz:

1. Çalışma alanını ve yerel makine öğrenimi geliştirici ortamınızı ayarlayın.
2. Azure Machine Learning Python SDK 'sını kullanarak bulutta kod çalıştırın.
3. Model eğitimi için kullandığınız Python ortamını yönetin.
4. Verileri Azure 'a yükleyin ve eğitiminde bu verileri kullanın.

**Bu öğretici serisinin 1. bölümünde**şunları yapmanız gerekir:

> [!div class="checklist"]
> * Azure Machine Learning SDK 'sını yükler
> * Kod için dizin yapısını ayarla
> * Azure Machine Learning çalışma alanı oluşturma
> * Yerel geliştirme ortamınızı yapılandırma
> * İşlem kümesi ayarlama

>[!NOTE]
> Bu öğretici serisi, işlem yoğunluklu ve/veya reproducibility gerektiren Python __işleri tabanlı__ makine öğrenimi görevlerine uygun Azure Machine Learning kavramlara odaklanır. Machine Learning görevleriniz bu profile uymuyorsa, Azure Machine Learning eklemek için [bir Azure Machine Learning işlem örneğinde Jupyıter veya RStudio işlevini](tutorial-1st-experiment-sdk-setup.md) kullanın.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning](https://aka.ms/AMLFree) bugün deneyin.
- Python ve [Machine Learning kavramlarıyla](concept-azure-machine-learning-architecture.md)benzerlik. Örneğin, ortamlar, eğitim, skor ve benzeri.
- Yerel bir geliştirme ortamı: Python yüklü bir dizüstü bilgisayar ve en sevdiğiniz IDE 'niz (örneğin, VSCode, Pylt, Jupyter vb.).

## <a name="install-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK 'sını yükler

Bu öğreticide, Azure Machine Learning Python SDK 'sını kullanırız.

Bu öğreticide kullanmak üzere bir ortam ayarlamak için en tanıdık olan araçları kullanabilirsiniz. Örneğin: Conda, PIP ve benzeri. Azure Machine Learning Python SDK 'Yı PIP aracılığıyla ortama yükler:

```bash
pip install azureml-sdk
```

## <a name="create-directory-structure-for-code"></a>Kod için dizin yapısı oluştur
Bu öğretici için aşağıdaki basit Dizin kurulumunu yapmanızı öneririz:

```markdown
tutorial
└──.azureml
```

- **öğretici** (projenin üst düzey dizini)
- **. azureml** (öğreticinin gizli alt dizini): `.azureml` Dizin Azure Machine Learning yapılandırma dosyalarını depolamak için kullanılır.

## <a name="create-an-azure-machine-learning-workspace"></a>Azure Machine Learning Çalışma Alanı oluşturma

Çalışma alanı, Azure Machine Learning için en üst düzey kaynaktır ve merkezi bir yerdir:

- İşlem gibi kaynakları yönetme
- Varlıkları Not defterleri, ortamlar, veri kümeleri, işlem hatları, modeller, uç noktaları gibi depolayın
- Diğer ekip üyeleriyle işbirliği yapın

Üst-üst dizinde- `tutorial` -aşağıdaki kodla adlı yeni bir Python dosyası ekleyin `01-create-workspace.py` . Parametreleri (ad, abonelik KIMLIĞI, kaynak grubu ve [konum](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service) ile tercihlerinizi uyarlayın.

Kodu etkileşimli bir oturumda veya bir Python dosyası olarak çalıştırabilirsiniz.

>[!NOTE]
> Yerel bir geliştirme ortamı (ör. dizüstü) kullanırken, aşağıdaki kodu ilk kez çalıştırdığınızda bir *cihaz kodu* kullanarak çalışma alanınızda kimlik doğrulaması yapmanız istenir. Ekrandaki yönergeleri takip edin.

```python
# tutorial/01-create-workspace.py
from azureml.core import Workspace

ws = Workspace.create(name='<my_workspace_name>', # provide a name for your workspace
                      subscription_id='<azure-subscription-id>', # provide your subscription ID
                      resource_group='<myresourcegroup>', # provide a resource group name
                      create_resource_group=True,
                      location='<NAME_OF_REGION>') # For example: 'westeurope' or 'eastus2' or 'westus2' or 'southeastasia'.

# write out the workspace details to a configuration file: .azureml/config.json
ws.write_config(path='.azureml')
```

Bu kodu `tutorial` dizinden çalıştırın:

```bash
cd <path/to/tutorial>
python ./01-create-workspace.py
```

Yukarıdaki kod parçacığı çalıştırıldıktan sonra klasör yapınız şöyle görünür:

```markdown
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
```

Dosya, `.azureml/config.json` Azure Machine Learning çalışma alanınıza bağlanmak için gereken meta verileri içerir (ABONELIK kimliğiniz, kaynak grubunuz ve çalışma alanı adınız). 

> [!NOTE]
> İçeriği `config.json` gizli değil; bu ayrıntıları paylaşmak çok güzel.
> Azure Machine Learning çalışma alanınız ile etkileşimde bulunmak için kimlik doğrulaması yine de gereklidir.

## <a name="create-an-azure-machine-learning-compute-cluster"></a>Azure Machine Learning işlem kümesi oluşturma

`tutorial`Adlı üst düzey dizinde bir Python betiği oluşturun `02-create-compute.py` ve sıfır ve dört düğüm arasında otomatik olarak ölçeklenebilen bir Azure Machine Learning işlem kümesi oluşturmak için aşağıdaki kodla doldurun:

```python
# tutorial/02-create-compute.py
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

ws = Workspace.from_config() # this automatically looks for a directory .azureml

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=4, 
                                                            idle_seconds_before_scaledown=2400)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)
```

Python dosyasını çalıştırın:

```bash
python ./02-create-compute.py
```


> [!NOTE]
> Küme oluşturulduğunda, 0 düğüm sağlanacaktır. Bu nedenle, bir iş gönderene kadar küme ücret **ödemez** . Bu küme, 2400 saniye boyunca boşta kaldığında ölçeği ölçeklendirecektir (40 dakika).

Klasör yapınız şimdi şöyle görünür:

```bash
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
└──02-create-compute.py
```

## <a name="next-steps"></a>Sonraki adımlar

Bu kurulum öğreticisinde şunları yapabilirsiniz:

- Azure Machine Learning çalışma alanı oluşturuldu
- Yerel geliştirme ortamınızı ayarlama
- Azure Machine Learning işlem kümesi oluşturuldu.

Bir sonraki öğreticide, Azure Machine Learning işlem kümesine bir betik göndermenizi adım adım ilerlereceğiz.

> [!div class="nextstepaction"]
> [Öğretici: Azure 'da "Merhaba Dünya" Python betiği çalıştırma](tutorial-1st-experiment-hello-world.md)
