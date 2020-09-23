---
title: 'Öğretici: kendi verilerinizi kullanma'
titleSuffix: Azure Machine Learning
description: Azure ML Başlarken serisinin 4. bölümü, kendi verilerinizi uzaktan eğitim çalıştırmasında nasıl kullanacağınızı gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: tracking-python
ms.openlocfilehash: 876ba76655572979a1d831a1ca07e5f3871a3283
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946756"
---
# <a name="tutorial-use-your-own-data-part-4-of-4"></a>Öğretici: kendi verilerinizi kullanın (4. bölüm)

Bu öğreticide, Azure Machine Learning ' de makine öğrenimi modellerini eğitmek için kendi verilerinizi karşıya yükleme ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

Bu öğretici, Azure 'da Azure Machine Learning ve iş tabanlı makine öğrenimi görevlerinin temellerini öğrendiğiniz **dört bölümden oluşan bir öğretici serisinin dördüncü kısmıdır** . Bu öğretici, [1. Bölüm: ayarlama](tutorial-1st-experiment-sdk-setup-local.md), [Bölüm 2: "Merhaba Dünya" çalıştırma](tutorial-1st-experiment-hello-world.md)ve [3. kısım: bir modeli eğitme](tutorial-1st-experiment-sdk-train.md)bölümünde tamamladığınız işi oluşturur.

[3. Bölüm 'de, bir modeli eğitme](tutorial-1st-experiment-sdk-train.md), `torchvision.datasets.CIFAR10` PYTORCH API 'sindeki yerleşik yöntem kullanılarak veriler indirildi. Ancak çoğu durumda, kendi verilerinizi bir uzaktan eğitim çalıştırmasında kullanmak isteyeceksiniz. Bu makalede, Azure Machine Learning içinde kendi verilerinize çalışmak için kullanabileceğiniz iş akışı gösterilmektedir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Eğitim betiğini yerel bir dizinde veri kullanacak şekilde yapılandırma
> * Eğitim betiğini yerel olarak test etme
> * Verileri Azure 'a yükleme
> * Denetim betiği oluştur
> * Yeni Azure Machine Learning kavramlarını anlama (parametreler, veri kümeleri, veri depoları geçirme)
> * Eğitim betiğinizi gönderme ve çalıştırma
> * Kod çıktılarınızı bulutta görüntüleyin

## <a name="prerequisites"></a>Önkoşullar

* Serinin [3. kısmını](tutorial-1st-experiment-sdk-train.md) doldurun.
* Python dili ve makine öğrenimi iş akışlarının giriş bilgisi.
* Yerel geliştirme ortamı. Bu, Visual Studio Code, Jupyıter veya Pydüğme ile sınırlı değildir.
* Python (sürüm 3.5-3.7).

## <a name="adjust-the-training-script"></a>Eğitim betiğini ayarlama
Artık eğitim betiğinizin (öğretici/src/eğitme. Kopyala) Azure Machine Learning ' de çalışır ve model performansını izleyebilir. Bağımsız değişkenler sunarak eğitim betiğine parametrize. Bağımsız değişkenlerin kullanılması, farklı hiper ölçerler için kolayca karşılaştırmanıza imkan tanır.

Şu anda eğitim betiğimiz, her çalıştırmada CIFAR10 veri kümesini indirmek üzere ayarlanmıştır. Aşağıdaki Python kodu bir dizinden verileri okumak için ayarlandı.

>[!NOTE] 
> `argparse`Betiği parametize için kullanımı.

```python
# tutorial/src/train.py
import os
import argparse
import torch
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

from model import Net
from azureml.core import Run

run = Run.get_context()

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument('--data_path', type=str, help='Path to the training data')
    parser.add_argument('--learning_rate', type=float, default=0.001, help='Learning rate for SGD')
    parser.add_argument('--momentum', type=float, default=0.9, help='Momentum for SGD')
    args = parser.parse_args()
    
    print("===== DATA =====")
    print("DATA PATH: " + args.data_path)
    print("LIST FILES IN DATA PATH...")
    print(os.listdir(args.data_path))
    print("================")
    
    # prepare DataLoader for CIFAR10 data
    transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])
    trainset = torchvision.datasets.CIFAR10(
        root=args.data_path,
        train=True,
        download=False,
        transform=transform,
    )
    trainloader = torch.utils.data.DataLoader(trainset, batch_size=4, shuffle=True, num_workers=2)

    # define convolutional network
    net = Net()

    # set up pytorch loss /  optimizer
    criterion = torch.nn.CrossEntropyLoss()
    optimizer = optim.SGD(
        net.parameters(),
        lr=args.learning_rate,
        momentum=args.momentum,
    )

    # train the network
    for epoch in range(2):

        running_loss = 0.0
        for i, data in enumerate(trainloader, 0):
            # unpack the data
            inputs, labels = data

            # zero the parameter gradients
            optimizer.zero_grad()

            # forward + backward + optimize
            outputs = net(inputs)
            loss = criterion(outputs, labels)
            loss.backward()
            optimizer.step()

            # print statistics
            running_loss += loss.item()
            if i % 2000 == 1999:
                loss = running_loss / 2000
                run.log('loss', loss) # log loss metric to AML
                print(f'epoch={epoch + 1}, batch={i + 1:5}: loss {loss:.2f}')
                running_loss = 0.0

    print('Finished Training')
```

### <a name="understanding-the-code-changes"></a>Kod değişikliklerini anlama

İçinde kullanılan kodda `train.py` `argparse` ,, ve ayarlamak için kitaplığı yararlanılabilir vardır `data_path` `learning_rate` `momentum` .

```python
# .... other code
parser = argparse.ArgumentParser()
parser.add_argument('--data_path', type=str, help='Path to the training data')
parser.add_argument('--learning_rate', type=float, default=0.001, help='Learning rate for SGD')
parser.add_argument('--momentum', type=float, default=0.9, help='Momentum for SGD')
args = parser.parse_args()
# ... other code
```

Ayrıca `train.py` komut dosyası, Kullanıcı tanımlı parametreleri kullanmak üzere İyileştiriciyi güncelleştirmek için uyarlanmıştır:

```python
optimizer = optim.SGD(
    net.parameters(),
    lr=args.learning_rate,     # get learning rate from command-line argument
    momentum=args.momentum,    # get momentum from command-line argument
)
```

## <a name="test-the-script-locally"></a>Betiği yerel olarak test etme

Komut dosyası artık bir bağımsız değişken olarak _veri yolunu_ kabul eder. İle başlamak için, yerel olarak test edin. Öğretici dizin yapınıza adlı bir klasör ekleyin `data` . Dizin yapınız şöyle görünmelidir:

```txt
tutorial
└──.azureml
|  └──config.json
|  └──pytorch-env.yml
└──data
└──src
|  └──hello.py
|  └──model.py
|  └──train.py
└──01-create-workspace.py
└──02-create-compute.py
└──03-run-hello.py
└──04-run-pytorch.py
```

`train.py`Önceki öğreticide yerel olarak çalıştırmadınız, `data/` dizininiz olmayacaktır. Bu durumda, `torchvision.datasets.CIFAR10` yöntemini `download=True` betiğinizdeki ile yerel olarak çalıştırın `train.py` .

Değiştirilen eğitim betiğini yerel olarak çalıştırmak için şunu çağırın:

```bash
python src/train.py --data_path ./data --learning_rate 0.003 --momentum 0.92
```

Verilere yerel bir yol geçirerek CIFAR10 veri kümesini indirmek zorunda kalmaktan kaçının. Ayrıca, eğitim betiğine sabit kodlamadan, _öğrenme oranı_ ve _itici güç_ ayarlama hiperparametreleri için farklı değerlerle denemeler yapabilirsiniz.

## <a name="upload-the-data-to-azure"></a>Verileri Azure 'a yükleme

Bu betiği Azure Machine Learning çalıştırmak için eğitim verilerinizi Azure 'da kullanılabilir hale getirmeniz gerekir. Azure Machine Learning çalışma alanınız, eğitim verilerinizi depolamak için kullanabileceğiniz bir Azure Blob depolama hesabı olan _varsayılan_ **veri deposu** ile donatılmıştır.

>[!NOTE] 
> Azure Machine Learning, verilerinizi depolayan diğer bulut tabanlı veri depolarına bağlanmanızı sağlar. Daha ayrıntılı bilgi için bkz. [datamağazalar belgeleri](./concept-data.md).  

Dizininde adlı yeni bir Python denetim betiği oluşturun `05-upload-data.py` `tutorial` :

```python
# tutorial/05-upload-data.py
from azureml.core import Workspace
ws = Workspace.from_config()
datastore = ws.get_default_datastore()
datastore.upload(src_dir='./data', target_path='datasets/cifar10', overwrite=True)
```

`target_path`CIFAR10 verilerinin karşıya yükleneceği veri deposundaki yolu belirtir.

>[!TIP] 
> Verileri karşıya yüklemek için Azure Machine Learning kullandığınızı, geçici dosyaları karşıya yüklemek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) kullanabilirsiniz. Bir ETL aracına ihtiyacınız varsa, [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) verilerinizi Azure 'a almak için kullanılabilir.

Verileri karşıya yüklemek için Python dosyasını çalıştırın (Note: karşıya yükleme, 60 saniyeden kısa olmalıdır.)

```bash
python 05-upload-data.py
```
Aşağıdaki Standart çıktıyı görmeniz gerekir:
```txt
Uploading ./data\cifar-10-batches-py\data_batch_2
Uploaded ./data\cifar-10-batches-py\data_batch_2, 4 files out of an estimated total of 9
.
.
Uploading ./data\cifar-10-batches-py\data_batch_5
Uploaded ./data\cifar-10-batches-py\data_batch_5, 9 files out of an estimated total of 9
Uploaded 9 files
```


## <a name="create-a-control-script"></a>Denetim betiği oluşturma

Daha önce yaptığınız gibi, adlı yeni bir Python denetim betiği oluşturun `06-run-pytorch-data.py` :

```python
# tutorial/06-run-pytorch-data.py
from azureml.core import Workspace
from azureml.core import Experiment
from azureml.core import Environment
from azureml.core import ScriptRunConfig
from azureml.core import Dataset

if __name__ == "__main__":
    ws = Workspace.from_config()
    
    datastore = ws.get_default_datastore()
    dataset = Dataset.File.from_files(path=(datastore, 'datasets/cifar10'))

    experiment = Experiment(workspace=ws, name='day1-experiment-data')

    config = ScriptRunConfig(
        source_directory='./src',
        script='train.py',
        compute_target='cpu-cluster',
        arguments=[
            '--data_path', dataset.as_named_input('input').as_mount(),
            '--learning_rate', 0.003,
            '--momentum', 0.92],
        )
    
    # set up pytorch environment
    env = Environment.from_conda_specification(name='pytorch-env',file_path='.azureml/pytorch-env.yml')
    config.run_config.environment = env

    run = experiment.submit(config)
    aml_url = run.get_portal_url()
    print("Submitted to an Azure Machine Learning compute cluster. Click on the link below")
    print("")
    print(aml_url)
```

### <a name="understand-the-code-changes"></a>Kod değişikliklerini anlayın

Denetim betiği, [Bu serinin 3. bölümümü](tutorial-1st-experiment-sdk-train.md) ile aşağıdaki yeni satırları içeren bire benzerdir:

:::row:::
   :::column span="":::
      `dataset = Dataset.File.from_files( ... )`
   :::column-end:::
   :::column span="2":::
      Veri [kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py&preserve-view=true) , Azure Blob deposuna yüklediğiniz verilere başvurmak için kullanılır. Veri kümeleri, verilerinizin üzerinde güvenilirlik ve güvenilirliği artırmak için tasarlanan bir Özet katmandır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig(...)`
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) , içine geçirilecek bağımsız değişkenlerin bir listesini içerecek şekilde değiştirilir `train.py` . `dataset.as_named_input('input').as_mount()`Bağımsız değişkeni, belirtilen dizinin işlem hedefine _bağlanması_ anlamına gelir.
   :::column-end:::
:::row-end:::

## <a name="submit-run-to-azure-machine-learning"></a>Azure Machine Learning çalışmayı gönder

Şimdi yeni yapılandırmayı kullanmak için çalıştırmayı yeniden gönderin:

```bash
python 06-run-pytorch-data.py
```

Bu, Azure Machine Learning Studio denemesinde bir URL yazdırır. Bu bağlantıya gittiğinizde kodunuzun çalıştığını görebilirsiniz.

### <a name="inspect-the-70_driver_log-log-file"></a>70_driver_log günlük dosyasını inceleyin

Azure Machine Learning Studio, deneme çalıştırmasına gidin (yukarıdaki hücreden URL çıktısına tıklayarak) ve ardından **çıktılar + Günlükler**' i tıklatın. 70_driver_log.txt dosyasına tıkladığınızda aşağıdaki çıktıyı görmeniz gerekir:

```txt
Processing 'input'.
Processing dataset FileDataset
{
  "source": [
    "('workspaceblobstore', 'datasets/cifar10')"
  ],
  "definition": [
    "GetDatastoreFiles"
  ],
  "registration": {
    "id": "XXXXX",
    "name": null,
    "version": null,
    "workspace": "Workspace.create(name='XXXX', subscription_id='XXXX', resource_group='X')"
  }
}
Mounting input to /tmp/tmp9kituvp3.
Mounted input to /tmp/tmp9kituvp3 as folder.
Exit __enter__ of DatasetContextManager
Entering Run History Context Manager.
Current directory:  /mnt/batch/tasks/shared/LS_root/jobs/dsvm-aml/azureml/tutorial-session-3_1600171983_763c5381/mounts/workspaceblobstore/azureml/tutorial-session-3_1600171983_763c5381
Preparing to call script [ train.py ] with arguments: ['--data_path', '$input', '--learning_rate', '0.003', '--momentum', '0.92']
After variable expansion, calling script [ train.py ] with arguments: ['--data_path', '/tmp/tmp9kituvp3', '--learning_rate', '0.003', '--momentum', '0.92']

Script type = None
===== DATA =====
DATA PATH: /tmp/tmp9kituvp3
LIST FILES IN DATA PATH...
['cifar-10-batches-py', 'cifar-10-python.tar.gz']
```

Değiştirilebileceğini

1. Azure Machine Learning, blob deposunu sizin için otomatik olarak işlem kümesine bağlamıştır.
2. ``dataset.as_named_input('input').as_mount()``Denetim betikte kullanılan, bağlama noktasına çözümleniyor

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanarak Azure 'a veri yüklemeyi gördük `Datastore` . Çalışma alanınız için bulut depolaması olarak hizmet veren veri deposu, verilerinizi korumak için kalıcı ve esnek bir yer sağlar.

Komut satırı aracılığıyla bir veri yolunu kabul etmek için eğitim betiğinizi nasıl değiştireceğiniz gördünüz. Kullanarak `Dataset` , uzak çalıştırmaya bir dizin bağlayabilmiş olabilirsiniz. 

Artık bir modelinize sahip olduğunuza göre şunları öğrenin:

* [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md)
