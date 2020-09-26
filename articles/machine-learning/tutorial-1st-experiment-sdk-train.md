---
title: 'Öğretici: ilk makine öğrenimi modelinizi eğitme-Python'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Başlarken serisinin 3. bölümü, makine öğrenimi modelinin nasıl eğitede olduğunu gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: f5c2690ea97136c2b7887a8450c2788e3902d4e3
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91369969"
---
# <a name="tutorial-train-your-first-machine-learning-model-part-3-of-4"></a>Öğretici: ilk makine öğrenimi modelinizi eğitme (Bölüm 3/4)

Bu öğreticide, Azure Machine Learning bir makine öğrenimi modelinin nasıl eğiteyapılacağı gösterilmektedir.

Bu öğretici, Azure 'da Azure Machine Learning ve iş tabanlı makine öğrenimi görevlerinin temellerini öğrendiğiniz *dört bölümden oluşan bir öğretici serisinin 3. parçasıdır* . Bu öğretici, [1. Bölüm: ayarlama](tutorial-1st-experiment-sdk-setup-local.md) ve [Bölüm 2: "Hello World!" çalıştırmak](tutorial-1st-experiment-hello-world.md) için tamamladığınız işleri oluşturur serisi.

Bu öğreticide, bir makine öğrenimi modelini gösteren bir komut dosyası göndererek sonraki adıma geçin. Bu örnek, yerel hata ayıklama ve uzak çalıştırmalar arasında Azure Machine Learning tutarlı davranışı nasıl bir şekilde hareket eder.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Bir eğitim betiği oluşturun.
> * Azure Machine Learning ortamını tanımlamak için Conda kullanın.
> * Bir denetim betiği oluşturun.
> * Azure Machine Learning sınıflarını anlayın ( `Environment` , `Run` , `Metrics` ).
> * Eğitim betiğinizi gönder ve Çalıştır.
> * Kod çıktılarınızı bulutta görüntüleyin.
> * Azure Machine Learning için günlük ölçümleri.
> * Ölçümünüzü bulutta görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

* Serinin [2. bölümünü](tutorial-1st-experiment-hello-world.md) tamamlama.
* Python dili ve makine öğrenimi iş akışlarının giriş bilgisi.
* Visual Studio Code, Jupyıter veya Pydüğme gibi yerel geliştirme ortamı.
* Python (sürüm 3,5 ile 3,7 arasında).

## <a name="create-training-scripts"></a>Eğitim betikleri oluşturma

İlk olarak, sinir ağ mimarisini bir dosyada tanımlarsınız `model.py` . Tüm eğitim kodunuz `src` , dahil olmak üzere alt dizine gider `model.py` .

Aşağıdaki kod, PyTorch kaynağından [Bu giriş örneğinde](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html) alınır. Azure Machine Learning kavramların yalnızca PyTorch değil, herhangi bir makine öğrenimi kodu için uygulanacağını unutmayın.

```python
# tutorial/src/model.py
import torch.nn as nn
import torch.nn.functional as F


class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
```

Daha sonra eğitim betiğini tanımlarsınız. Bu betik, PyTorch API 'lerini kullanarak CIFAR10 veri kümesini indirir `torchvision.dataset` , içinde tanımlanan ağı ayarlar `model.py` ve standart SGD ve çapraz entropi kaybını kullanarak iki dönemler için bunu yapar.

`train.py`Alt dizinde bir betik oluşturun `src` :

```python
# tutorial/src/train.py
import torch
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

from model import Net

# download CIFAR10 data
trainset = torchvision.datasets.CIFAR10(
    root="./data",
    train=True,
    download=True,
    transform=torchvision.transforms.ToTensor(),
)
trainloader = torch.utils.data.DataLoader(
    trainset, batch_size=4, shuffle=True, num_workers=2
)

if __name__ == "__main__":

    # define convolutional network
    net = Net()

    # set up pytorch loss /  optimizer
    criterion = torch.nn.CrossEntropyLoss()
    optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)

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
                print(f"epoch={epoch + 1}, batch={i + 1:5}: loss {loss:.2f}")
                running_loss = 0.0

    print("Finished Training")

```

Artık şu dizin yapısına sahipsiniz:

```txt
tutorial
└──.azureml
|  └──config.json
└──src
|  └──hello.py
|  └──model.py
|  └──train.py
└──01-create-workspace.py
└──02-create-compute.py
└──03-run-hello.py
```

## <a name="create-a-python-environment"></a>Python ortamı oluşturma

Tanıtım amacıyla, Conda ortamını kullanacağız. (Bir PIP sanal ortamının adımları neredeyse aynıdır.)

Gizli dizinde adlı bir dosya oluşturun `pytorch-env.yml` `.azureml` :

```yml
# tutorial/.azureml/pytorch-env.yml
name: pytorch-env
channels:
    - defaults
    - pytorch
dependencies:
    - python=3.6.2
    - pytorch
    - torchvision
```

Bu ortam, modelinize ve eğitim betiğinizin gerektirdiği tüm bağımlılıklara sahiptir. Python için Azure Machine Learning SDK 'nın bağımlılığı olmadığına dikkat edin.

## <a name="test-locally"></a>Yerel olarak test etme

Komut dosyanızı bu ortamda yerel olarak test etmek için aşağıdaki kodu kullanın:

```bash
conda env create -f .azureml/pytorch-env.yml    # create conda environment
conda activate pytorch-env                      # activate conda environment
python src/train.py                             # train model
```

Bu betiği çalıştırdıktan sonra adlı bir dizine indirilen verileri görürsünüz `tutorial/data` .

## <a name="create-the-control-script"></a>Denetim betiğini oluşturma

Aşağıdaki denetim betiği ve "Hello World!" göndermek için kullandığınız biri arasındaki fark , ortamı ayarlamak için birkaç ek satır eklemenizi sağlar.

Adlı dizinde yeni bir Python dosyası oluşturun `tutorial` `04-run-pytorch.py` :

```python
# tutorial/04-run-pytorch.py
from azureml.core import Workspace
from azureml.core import Experiment
from azureml.core import Environment
from azureml.core import ScriptRunConfig

if __name__ == "__main__":
    ws = Workspace.from_config()
    experiment = Experiment(workspace=ws, name='day1-experiment-train')
    config = ScriptRunConfig(source_directory='src', script='train.py', compute_target='cpu-cluster')

    # set up pytorch environment
    env = Environment.from_conda_specification(name='pytorch-env', file_path='.azureml/pytorch-env.yml')
    config.run_config.environment = env

    run = experiment.submit(config)

    aml_url = run.get_portal_url()
    print(aml_url)
```

### <a name="understand-the-code-changes"></a>Kod değişikliklerini anlayın

:::row:::
   :::column span="":::
      `env = ...`
   :::column-end:::
   :::column span="2":::
      Azure Machine Learning, denemeleri çalıştırmaya yönelik tekrarlanabilir, sürümlü bir Python ortamını temsil eden bir [ortam](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true) kavramı sağlar. Yerel bir Conda veya PIP ortamından ortam oluşturmak kolaydır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config.run_config.environment = env`
   :::column-end:::
   :::column span="2":::
      Ortamı [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true)'e ekler.
   :::column-end:::
:::row-end:::

## <a name="submit-the-run-to-azure-machine-learning"></a>Çalıştırmayı Azure Machine Learning gönder

Yerel ortamları geçtiyseniz, Python için Azure Machine Learning SDK 'Sı yüklü olan bir ortama geri doğru geçdiğinizden emin olun. 

Ardından şunu çalıştırın:

```bash
python 04-run-pytorch.py
```

>[!NOTE] 
> Bu betiği ilk kez çalıştırdığınızda Azure Machine Learning, PyTorch ortamınızdan yeni bir Docker görüntüsü oluşturacak. Tüm çalıştırmanın tamamlanması 5 ila 10 dakika sürebilir. 
>
> Docker Derleme günlüklerini Azure Machine Learning Studio 'da görebilirsiniz. Studio bağlantısını izleyin, **çıktılar + Günlükler** sekmesini seçin ve ardından öğesini seçin `20_image_build_log.txt` .
>
> Bu görüntü daha hızlı çalışmasını sağlamak için gelecekteki çalışmalarda yeniden kullanılacaktır.

Görüntünüz oluşturulduktan sonra `70_driver_log.txt` eğitim betiğiniz çıktıyı görmek için seçin.

```txt
Downloading https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz to ./data/cifar-10-python.tar.gz
...
Files already downloaded and verified
epoch=1, batch= 2000: loss 2.19
epoch=1, batch= 4000: loss 1.82
epoch=1, batch= 6000: loss 1.66
epoch=1, batch= 8000: loss 1.58
epoch=1, batch=10000: loss 1.52
epoch=1, batch=12000: loss 1.47
epoch=2, batch= 2000: loss 1.39
epoch=2, batch= 4000: loss 1.38
epoch=2, batch= 6000: loss 1.37
epoch=2, batch= 8000: loss 1.33
epoch=2, batch=10000: loss 1.31
epoch=2, batch=12000: loss 1.27
Finished Training
```

> [!WARNING]
> Bir hata görürseniz `Your total snapshot size exceeds the limit` , `data` Dizin ' `source_directory` de kullanılan değerde bulunur `ScriptRunConfig` .
>
> `data`Dışına Taşı `src` .

Ortamlar, ile bir çalışma alanına kaydedilebilir `env.register(ws)` . Daha sonra kolayca paylaşılabilir, yeniden kullanılabilir ve sürümlenebilir. Ortamlar, önceki sonuçları yeniden oluşturulmasını ve ekibinizle işbirliği yapmayı kolaylaştırır.

Azure Machine Learning Ayrıca, seçkin ortamların bir koleksiyonunu tutar. Bu ortamlar ortak makine öğrenimi senaryolarını kapsar ve önbelleğe alınmış Docker görüntüleri tarafından desteklenir. Önbelleğe alınmış Docker görüntüleri, ilk uzak çalıştırmayı daha hızlı hale getirir.

Kısacası, kayıtlı ortamların kullanılması zamandan tasarruf edebilir! Daha fazla bilgi için [ortamları nasıl kullanacağınızı](./how-to-use-environments.md) okuyun.

## <a name="log-training-metrics"></a>Günlük eğitimi ölçümleri

Artık Azure Machine Learning bir model eğitimine sahip olduğunuza göre bazı performans ölçümlerini izlemeye başlayın.

Geçerli eğitim betiği, ölçümleri terminalden yazdırır. Azure Machine Learning, ölçümleri daha fazla işlevle günlüğe kaydetmek için bir mekanizma sağlar. Birkaç satır kod ekleyerek, Studio 'daki ölçümleri görselleştirme ve birden çok çalıştırma arasında ölçümleri karşılaştırma imkanına sahip olursunuz.

### <a name="modify-trainpy-to-include-logging"></a>`train.py`Günlüğe kaydetmeyi dahil etmek için değiştirin

`train.py`Betiğinizi iki satırlık kod satırı içerecek şekilde değiştirin:

```python
# train.py
import torch
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

from model import Net
from azureml.core import Run


# ADDITIONAL CODE: get Azure Machine Learning run from the current context
run = Run.get_context()

# download CIFAR10 data
trainset = torchvision.datasets.CIFAR10(
    root="./data",
    train=True,
    download=True,
    transform=torchvision.transforms.ToTensor(),
)
trainloader = torch.utils.data.DataLoader(
    trainset, batch_size=4, shuffle=True, num_workers=2
)

if __name__ == "__main__":

    # define convolutional network
    net = Net()

    # set up pytorch loss /  optimizer
    criterion = torch.nn.CrossEntropyLoss()
    optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)

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
                run.log('loss', loss) # ADDITIONAL CODE: log loss metric to Azure Machine Learning
                print(f'epoch={epoch + 1}, batch={i + 1:5}: loss {loss:.2f}')
                running_loss = 0.0

    print('Finished Training')
```

#### <a name="understand-the-additional-two-lines-of-code"></a>Kodun ek iki satırını anlayın

' De `train.py` , yöntemini kullanarak, çalışma nesnesine eğitim betiğinin _içinden_ erişirsiniz `Run.get_context()` ve ölçümleri günlüğe kaydetmek için kullanabilirsiniz:

```python
# in train.py
run = Run.get_context()

...

run.log('loss', loss)
```

Azure Machine Learning ölçümler şunlardır:

- Deneme ve çalıştırma tarafından düzenlenmiştir, bu nedenle ölçümleri izlemek ve karşılaştırmak kolaydır.
- , Studio 'da eğitim performansını görselleştirebilmeniz için bir kullanıcı arabirimi ile donatılmış.
- Ölçeklendirilmesi için tasarlanan, yüzlerce denemeleri çalıştırırken bu avantajları de koruyabilirsiniz.

### <a name="update-the-conda-environment-file"></a>Conda ortam dosyasını güncelleştirme

`train.py`Komut dosyası yeni bir bağımlılık almış `azureml.core` . `pytorch-env.yml`Bu değişikliği yansıtacak şekilde güncelleştirin:

```yaml
# tutorial/.azureml/pytorch-env.yml
name: pytorch-env
channels:
    - defaults
    - pytorch
dependencies:
    - python=3.6.2
    - pytorch
    - torchvision
    - pip
    - pip:
        - azureml-sdk
```

### <a name="submit-the-run-to-azure-machine-learning"></a>Çalıştırmayı Azure Machine Learning gönder
Bu betiği bir kez daha gönder:

```bash
python 04-run-pytorch.py
```

Bu kez, Studio 'yu ziyaret ettiğinizde, artık model eğitimi kaybından canlı güncelleştirmeleri görebileceğiniz **ölçümler** sekmesine gidin!

:::image type="content" source="media/tutorial-1st-experiment-sdk-train/logging-metrics.png" alt-text="Ölçümler sekmesinde eğitim kaybı grafiği.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu oturumda, temel bir "Hello World!" üzerinden yükselttiniz belirli bir Python ortamının çalışmasını gerektiren daha gerçekçi bir eğitim betiğinin betiği. Azure Machine Learning ortamları ile buluta yerel bir Conda ortamını nasıl alacağınızı gördünüz. Son olarak, Azure Machine Learning ölçümleri günlüğe kaydetmek için birkaç satır kod satırını gördünüz.

[Bir PIP requirements.txt](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true#from-pip-requirements-name--file-path-) dosyasından veya [var olan bir yerel conda ortamından](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true#from-existing-conda-environment-name--conda-environment-name-)Azure Machine Learning ortamlar oluşturmanın başka yolları da vardır.

Bir sonraki oturumda, CIFAR10 veri kümesini Azure 'a yükleyerek Azure Machine Learning verilerle nasıl çalışacaksınız görürsünüz.

> [!div class="nextstepaction"]
> [Öğretici: kendi verilerinizi getirin](tutorial-1st-experiment-bring-data.md)

>[!NOTE] 
> Öğretici serisini burada bitirebilmeniz ve bir sonraki adımda ilerlemeniz istiyorsanız [kaynaklarınızı temizlemeyi](tutorial-1st-experiment-bring-data.md#clean-up-resources)unutmayın.
