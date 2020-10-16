---
title: 'Öğretici: Machine Learning ile çalışmaya başlama-Python'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, kişisel geliştirme ortamınızda çalışan Python için Azure Machine Learning SDK 'sını kullanmaya başlayacaksınız.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: c0fe3c3808709de732bec8ce0599d380094405e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91368490"
---
# <a name="tutorial-get-started-with-azure-machine-learning-in-your-development-environment-part-1-of-4"></a>Öğretici: geliştirme ortamınızda Azure Machine Learning kullanmaya başlayın (Bölüm 1/4)

Bu *dört bölümden oluşan öğretici serisinde*, Azure bulut platformunda Azure Machine Learning ve işlerin temel aldığı Python Machine Learning görevlerinin temellerini öğrenirsiniz. Bu görevler arasında şunlar yer alır:

1. Machine Learning için bir çalışma alanı ve yerel geliştirici ortamınızı ayarlayın.
2. Python için Azure Machine Learning SDK kullanarak bulutta kod çalıştırın.
3. Model eğitimi için kullandığınız Python ortamını yönetin.
4. Verileri Azure 'a yükleyin ve eğitiminde bu verileri kullanın.

Bu öğretici serisinin 1. bölümünde şunları yapmanız gerekir:

> [!div class="checklist"]
> * Azure Machine Learning SDK 'sını yükler.
> * Kod için dizin yapısını ayarlayın.
> * Azure Machine Learning çalışma alanı oluşturun.
> * Yerel geliştirme ortamınızı yapılandırın.
> * Bir işlem kümesi ayarlayın.

>[!NOTE]
> Bu öğretici serisi, işlem yoğunluklu ve/veya reproducibility gerektiren Python *işleri tabanlı* makine öğrenimi görevlerine uygun Azure Machine Learning kavramlara odaklanır. Machine Learning görevleriniz bu profile uymuyorsa, Azure Machine Learning ilerlemek için [bir Azure Machine Learning işlem örneğinde Jupyıter veya RStudio işlevini](tutorial-1st-experiment-sdk-setup.md) kullanın.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning](https://aka.ms/AMLFree)deneyin.
- Python ve [Machine Learning kavramlarıyla](concept-azure-machine-learning-architecture.md)benzerlik. Ortamları, eğitimi ve Puanlama örnekleri içerir.
- Yerel bir geliştirme ortamı: Python yüklü bir dizüstü bilgisayar ve en sevdiğiniz IDE 'niz (örneğin, Visual Studio Code, Pylt veya Jupyter).

## <a name="install-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK 'sını yükler

Bu öğreticide, Python için Azure Machine Learning SDK 'sını kullanırız.

Bu öğreticide kullanmak üzere bir ortam ayarlamak için size bildiğiniz (örneğin, Conda ve PIP) araçları kullanabilirsiniz. PIP ile Python için Azure Machine Learning SDK 'sını ortama yükler:

```bash
pip install azureml-sdk
```

## <a name="create-a-directory-structure-for-code"></a>Kod için dizin yapısı oluşturma
Bu öğretici için aşağıdaki basit dizin yapısını ayarlamanızı öneririz:

```markdown
tutorial
└──.azureml
```

- `tutorial`: Projenin en üst düzey dizini.
- `.azureml`: Azure Machine Learning yapılandırma dosyalarını depolamak için gizli alt dizin.

## <a name="create-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanı oluşturma

Çalışma alanı, Azure Machine Learning için en üst düzey kaynaktır ve merkezi bir yerdir:

- İşlem gibi kaynakları yönetin.
- Not defterleri, ortamlar, veri kümeleri, işlem hatları, modeller ve uç noktalar gibi varlıkları depolayın.
- Diğer ekip üyeleriyle işbirliği yapın.

Üst düzey dizinde, `tutorial` aşağıdaki kodu kullanarak adlı yeni bir Python dosyası ekleyin `01-create-workspace.py` . Parametreleri (ad, abonelik KIMLIĞI, kaynak grubu ve [konum](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service)) tercihlerinizi uyarlayın.

Kodu etkileşimli bir oturumda veya bir Python dosyası olarak çalıştırabilirsiniz.

>[!NOTE]
> Yerel bir geliştirme ortamı (örneğin, dizüstü bilgisayar) kullandığınızda, aşağıdaki kodu ilk kez çalıştırdığınızda bir *cihaz kodu* kullanarak çalışma alanınızda kimlik doğrulaması yapmanız istenir. Ekrandaki yönergeleri takip edin.

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

Yukarıdaki kod parçacığını çalıştırdıktan sonra klasör yapınız şöyle görünür:

```markdown
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
```

Dosya, `.azureml/config.json` Azure Machine Learning çalışma alanınıza bağlanmak için gereken meta verileri içerir. Yani, abonelik KIMLIĞINIZI, kaynak grubunuzu ve çalışma alanı adını içerir. 

> [!NOTE]
> İçeriği `config.json` gizli değil. Bu ayrıntıların paylaşılması iyidir.
>
> Azure Machine Learning çalışma alanınız ile etkileşimde bulunmak için kimlik doğrulaması yine de gereklidir.

## <a name="create-an-azure-machine-learning-compute-cluster"></a>Azure Machine Learning işlem kümesi oluşturma

`tutorial`Adlı üst düzey dizinde bir Python betiği oluşturun `02-create-compute.py` . Sıfır ve dört düğüm arasında otomatik ölçeklendirme yapılacak bir Azure Machine Learning işlem kümesi oluşturmak için aşağıdaki kodla doldurun:

```python
# tutorial/02-create-compute.py
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

ws = Workspace.from_config() # This automatically looks for a directory .azureml

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that the cluster does not exist already
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
> Küme oluşturulduğunda, 0 düğümü sağlanır. Bir iş gönderene kadar küme ücret *ödemez* . Bu küme, 2.400 saniye boyunca boşta kaldığında ölçeği ölçeklendirecektir (40 dakika).

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

- Azure Machine Learning çalışma alanı oluşturuldu.
- Yerel geliştirme ortamınızı ayarlayın.
- Azure Machine Learning işlem kümesi oluşturuldu.

Bir sonraki öğreticide, Azure Machine Learning işlem kümesine bir betik göndermenizi adım adım ilerlereceğiz.

> [!div class="nextstepaction"]
> [Öğretici: "Hello World!" Çalıştır Azure 'da Python betiği](tutorial-1st-experiment-hello-world.md)
