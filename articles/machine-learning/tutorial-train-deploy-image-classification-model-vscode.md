---
title: 'Öğretici: eğitim & dağıtım modelleri: VS Code (Önizleme)'
titleSuffix: Azure Machine Learning
description: TensorFlow ve Azure Machine Learning Visual Studio Code uzantısını kullanarak görüntü sınıflandırma modelini eğitme ve dağıtma hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 07/09/2020
ms.custom: contperfq4
ms.openlocfilehash: 7d209b3434eae20b4c9a7b328f5c15032315b178
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86203555"
---
# <a name="train-and-deploy-an-image-classification-tensorflow-model-using-the-azure-machine-learning-visual-studio-code-extension-preview"></a>Azure Machine Learning Visual Studio Code uzantısı (Önizleme) kullanarak görüntü sınıflandırması TensorFlow modelini eğitme ve dağıtma

TensorFlow ve Azure Machine Learning Visual Studio Code uzantısını kullanarak el ile yazılmış sayıları tanımak için görüntü sınıflandırma modelini eğitme ve dağıtma hakkında bilgi edinin.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Kodu anlama
> * Çalışma alanı oluşturma
> * Deneme oluşturma
> * Bilgisayar hedeflerini yapılandırma
> * Bir yapılandırma dosyası çalıştırın
> * Modeli eğitme
> * Bir modeli kaydetme
> * Model dağıtma

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Bir hesabınız yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)denemek için kaydolun.
- Hafif, platformlar arası bir kod Düzenleyicisi [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)yükler.
- Azure Machine Learning Studio Visual Studio Code uzantısı. Yükleme yönergeleri için [kurulum Azure Machine Learning Visual Studio Code uzantısı öğreticisine](./tutorial-setup-vscode-extension.md) bakın

## <a name="understand-the-code"></a>Kodu anlama

Bu öğreticinin kodu, 0-9 adresinden el ile yazılmış rakamları kategorilere ayırır. Bu, 28 px x 28 piksel görüntüsünün piksel değerlerini giriş olarak alan ve sınıflandırılan basamakların her biri için bir tane olmak üzere 10 olasılıkların bir listesini çıkaran bir sinir ağı oluşturarak bunu yapar. Aşağıda verilerin nasıl göründüğü hakkında bir örnek verilmiştir.  

![EIST rakamları](./media/tutorial-train-deploy-image-classification-model-vscode/digits.png)

Bu öğreticinin, bilgisayarınızda herhangi bir yerde bulunan [AI deposunun vs Code araçlarını](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) indirip geri göndererek kodu alın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning bir uygulama oluşturmak için yapmanız gereken ilk şey, bir çalışma alanı oluşturmaktır. Çalışma alanı modelleri ve eğitilen modellerin kendilerini eğiten kaynakları içerir. Daha fazla bilgi için bkz. [çalışma alanı nedir](./concept-workspace.md). 

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçerek Azure Machine Learning görünümünü açın.
1. Azure aboneliğinize sağ tıklayın ve **çalışma alanı oluştur**' u seçin. 
    
    > [!div class="mx-imgBorder"]
    > ![Çalışma Alanı oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-workspace.png)

1. Varsayılan olarak, oluşturma tarihi ve saati içeren bir ad oluşturulur. Metin girişi kutusunda adı "TeamWorkspace" olarak değiştirin ve **ENTER**tuşuna basın.
1. **Yeni kaynak grubu oluştur**' u seçin. 
1. Kaynak grubunuzu "TeamWorkspace-RG" olarak adlandırın ve **ENTER**'a basın. 
1. Çalışma alanınız için bir konum seçin. Modelinizi dağıtmayı planladığınız konuma en yakın konumu seçmeniz önerilir. Örneğin, "Batı ABD 2".
1. Çalışma alanı türünü seçmeniz **istendiğinde temel ' yı seçerek temel** bir çalışma alanı oluşturun. Farklı çalışma alanı teklifleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning genel bakış](./overview-what-is-azure-ml.md#sku).

Bu noktada, hesabınızda yeni bir çalışma alanı oluşturmak için Azure isteği yapılır. Birkaç dakika sonra, abonelik düğümünde yeni çalışma alanı görüntülenir. 

## <a name="create-an-experiment"></a>Deneme oluşturma

Tek tek model eğitimi çalıştırmalarını izlemek ve analiz etmek için, çalışma alanınızda bir veya daha fazla denemeleri oluşturulabilir. Çalıştırmalar, Azure bulutu 'nda veya yerel makinenizde yapılabilir.

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir.
1. Abonelik düğümünü genişletin.
1. **Teamworkspace** düğümünü genişletin. 
1. **Denemeleri** düğümüne sağ tıklayın.
1. Bağlam menüsünden **deneme oluştur** ' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-experiment.png)

1. Denemenizi "MNIST" olarak adlandırın ve yeni denemeyi oluşturmak için **ENTER** 'a basın. 

Çalışma alanları gibi, belirtilen yapılandırmalara sahip bir deneme oluşturmak için Azure 'a bir istek gönderilir. Birkaç dakika sonra, yeni deneme, çalışma alanınızın *denemeleri* düğümünde görünür. 

## <a name="configure-compute-targets"></a>Işlem hedeflerini yapılandırma

İşlem hedefi, betikleri çalıştırdığınız ve eğitilen modeller dağıttığınız bilgi işlem kaynağıdır veya ortamıdır. Daha fazla bilgi için [Azure Machine Learning işlem hedefleri belgelerine](./concept-compute-target.md)bakın.

İşlem hedefi oluşturmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir. 
1. Abonelik düğümünü genişletin. 
1. **Teamworkspace** düğümünü genişletin. 
1. Çalışma alanı düğümü altında, **işlem kümeleri** düğümüne sağ tıklayıp **işlem oluştur**' u seçin. 

    > [!div class="mx-imgBorder"]
    > ![İşlem hedefi oluştur](./media/tutorial-train-deploy-image-classification-model-vscode/create-compute.png)

1. **Azure Machine Learning işlem (AmlCompute)** öğesini seçin. Azure Machine Learning Işlem, kullanıcının çalışma alanınızdaki diğer kullanıcılarla birlikte kullanılabilecek tek veya çok düğümlü bir işlem oluşturmasını sağlayan bir yönetilen işlem altyapısıdır.
1. Bir VM boyutu seçin. Seçenekler listesinden **Standard_F2s_v2** ' yi seçin. SANAL makinenizin boyutu, modellerinizi eğitmek için gereken süre miktarını etkiler. VM boyutları hakkında daha fazla bilgi için bkz. [Azure 'Da Linux sanal makineleri için Boyutlar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).
1. İşlem bilgilerinizi oluşturmak için "TeamWkspc-com" **yazın ve ENTER** tuşuna basın.

    VS Code bir dosya aşağıdakine benzer içerikle birlikte görünür:

    ```json
    {
        "location": "westus2",
        "tags": {},
        "properties": {
            "computeType": "AmlCompute",
            "description": "",
            "properties": {
                "vmSize": "Standard_F2s_v2",
                "vmPriority": "dedicated",
                "scaleSettings": {
                    "maxNodeCount": 4,
                    "minNodeCount": 0,
                    "nodeIdleTimeBeforeScaleDown": "PT120S"
                }
            }
        }
    }
    ```

1. Yapılandırma tatmin edildiğinde, komut paletini **görüntüle > komut paleti**' ni seçerek açın.
1. Çalıştırma yapılandırma dosyanızı kaydetmek için komut paletinde aşağıdaki komutu girin.

    ```text
    Azure ML: Save and Continue
    ```

Birkaç dakika sonra yeni işlem hedefi, çalışma alanınızın *işlem kümeleri* düğümünde görüntülenir.

## <a name="create-a-run-configuration"></a>Çalıştırma yapılandırması oluşturma

Bir işlem hedefine bir eğitim çalıştırması gönderdiğinizde, eğitim işini çalıştırmak için gereken yapılandırmayı da gönderebilirsiniz. Örneğin, eğitim kodunu ve onu çalıştırmak için gereken Python bağımlılıklarını içeren komut dosyası.

Bir çalıştırma yapılandırması oluşturmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir. 
1. Abonelik düğümünü genişletin. 
1. **Ekip çalışma alanı > işlem kümeleri** düğümünü genişletin. 
1. İşlem düğümü altında **Teamwkspc-com** işlem düğümüne sağ tıklayın ve **çalıştırma yapılandırması oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Çalıştırma yapılandırması oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-run-configuration.png)

1. Çalışma yapılandırmanızı oluşturmak için "MNIST-RC" adlı çalışma yapılandırmanızı adlandırın ve **ENTER** tuşuna basın.
1. Ardından **yeni Azure ML ortamı oluştur**' u seçin. Ortamlar, betikleri çalıştırmak için gereken bağımlılıkları tanımlar.
1. Ortamınızı "MNIST-env" olarak adlandırın ve **ENTER**tuşuna basın.
1. Listeden **Conda Dependencies dosyasını** seçin.
1. Conda Dependencies dosyasına gitmek için **ENTER** tuşuna basın. Bu durumda, bağımlılıklar dosyası `env.yml` dizinin içindeki dosyadır `vscode-tools-for-ai/mnist-vscode-docs-sample` .

    VS Code bir dosya aşağıdakine benzer içerikle birlikte görünür:

    ```json
    {
        "name": "MNIST-env",
        "version": "1",
        "python": {
            "interpreterPath": "python",
            "userManagedDependencies": false,
            "condaDependencies": {
                "name": "vs-code-azure-ml-tutorial",
                "channels": [
                    "defaults"
                ],
                "dependencies": [
                    "python=3.6.2",
                    "tensorflow=1.15.0",
                    "pip",
                    {
                        "pip": [
                            "azureml-defaults"
                        ]
                    }
                ]
            },
            "baseCondaEnvironment": null
        },
        "environmentVariables": {},
        "docker": {
            "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
            "baseDockerfile": null,
            "baseImageRegistry": {
                "address": null,
                "username": null,
                "password": null
            },
            "enabled": false,
            "arguments": []
        },
        "spark": {
            "repositories": [],
            "packages": [],
            "precachePackages": true
        },
        "inferencingStackVersion": null
    }
    ```

1. Yapılandırmanızla memnun olduktan sonra, komut paletini açıp aşağıdaki komutu girerek dosyayı kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

1. Bu örnek, Azure Machine Learning kayıtlı bir veri kümesini kullanmaz. Bunun yerine, *train.py* çalıştırıldığında yüklenir. Eğitim çalıştıralım için bir veri başvurusu oluşturmanız istendiğinde, istemine "n" yazın ve **ENTER**tuşuna basın.
1. İşlem üzerinde çalıştırılacak betik dosyasına gitmek için **ENTER** tuşuna basın. Bu durumda, modeli eğitme betiği `train.py` Dizin içindeki dosyadır `vscode-tools-for-ai/mnist-vscode-docs-sample` .

    VS Code adlı bir dosya `MNIST-rc.runconfig` , aşağıdakine benzer içerikle birlikte görüntülenir:

    ```json
    {
        "script": "train.py",
        "arguments": [],
        "framework": "Python",
        "communicator": "None",
        "target": "TeamWkspc-com",
        "environment": {
            "name": "MNIST-env",
            "version": "1",
            "python": {
                "interpreterPath": "python",
                "userManagedDependencies": false,
                "condaDependencies": {
                    "name": "vs-code-azure-ml-tutorial",
                    "channels": [
                        "defaults"
                    ],
                    "dependencies": [
                        "python=3.6.2",
                        "tensorflow=1.15.0",
                        "pip",
                        {
                            "pip": [
                                "azureml-defaults"
                            ]
                        }
                    ]
                },
                "baseCondaEnvironment": null
            },
            "environmentVariables": {},
            "docker": {
                "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
                "baseDockerfile": null,
                "baseImageRegistry": {
                    "address": null,
                    "username": null,
                    "password": null
                },
                "enabled": false,
                "arguments": []
            },
            "spark": {
                "repositories": [],
                "packages": [],
                "precachePackages": true
            },
            "inferencingStackVersion": null
        },
        "history": {
            "outputCollection": true,
            "snapshotProject": false,
            "directoriesToWatch": [
                "logs"
            ]
        }
    }
    ```

1. Yapılandırmanızla memnun olduktan sonra, komut paletini açıp aşağıdaki komutu girerek dosyayı kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

`MNIST-rc`Çalıştırma yapılandırması, *Teamwkspc-com* işlem düğümünün altına eklenir ve `MNIST-env` ortam yapılandırması *ortamlar* düğümünün altına eklenir.

## <a name="train-the-model"></a>Modeli eğitme

Eğitim süreci sırasında, sınıflandırılan ilgili basamakların her biri için eğitim verileri ve bu içinde gömülü öğrenme desenleri işlenerek bir TensorFlow modeli oluşturulur. 

Azure Machine Learning deneme çalıştırmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir. 
1. Abonelik düğümünü genişletin. 
1. **Teamworkspace > denemeleri** düğümünü genişletin. 
1. **Mnist** denemenize sağ tıklayın.
1. **Deneme Çalıştır**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme çalıştırma](./media/tutorial-train-deploy-image-classification-model-vscode/run-experiment.png)

1. İşlem hedefi seçenekleri listesinden **Teamwkspc-com** işlem hedefini seçin.
1. Ardından, **Mnist-RC** Run Configuration ' ı seçin.
1. Bu noktada, çalışma alanınızdaki seçili işlem hedefi üzerinde denemenizi çalıştırmak için Azure 'a bir istek gönderilir. Bu işlem birkaç dakika sürer. Eğitim işini çalıştırmak için gereken süre, işlem türü ve eğitim verileri boyutu gibi çeşitli faktörlerden etkilenir. Denemenizin ilerlemesini izlemek için, geçerli çalıştırma düğümüne sağ tıklayıp **Azure Portal Çalıştır**' ı seçin.
1. Bir dış Web sitesi açmak isteyen iletişim kutusu göründüğünde **Aç**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme ilerlemesini izleme](./media/tutorial-train-deploy-image-classification-model-vscode/track-experiment-progress.png)

Model tamamlandığında, çalışma düğümü ' nün yanındaki durum etiketi "tamamlandı" olarak güncelleştirilir.

## <a name="register-the-model"></a>Modeli Kaydet

Modelinize eğitim edindiniz. bu aşamada, çalışma alanınıza kaydedebilirsiniz. 

Modelinizi kaydetmek için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir.
1. Abonelik düğümünü genişletin. 
1. **Teamworkspace > denemeleri > MNIST** düğümünü genişletin.
1. Model eğitimi 'nden oluşturulan model çıkışlarını alın. **Çalıştır 1** çalıştırma düğümüne sağ tıklayın ve **çıkışları indir**' i seçin. 

    > [!div class="mx-imgBorder"]
    > ![Model çıkışlarını indir](./media/tutorial-train-deploy-image-classification-model-vscode/download-outputs.png)

1. İndirilen çıktıların kaydedileceği dizini seçin. Varsayılan olarak, çıktılar Visual Studio Code Şu anda açılmış dizine yerleştirilir.
1. **Modeller** düğümüne sağ tıklayın ve **modeli Kaydet**' i seçin.

    > [!div class="mx-imgBorder"]
    > ![Bir modeli kaydetme](./media/tutorial-train-deploy-image-classification-model-vscode/register-model.png)

1. Modelinize "MNIST-TensorFlow-model" adını girin ve **ENTER**tuşuna basın.
1. Bir TensorFlow modeli birçok dosyadan oluşur. Seçenekler listesinden model yolu biçimi olarak **model klasörü** ' nü seçin. 
1. Dizini seçin `azureml_outputs/Run_1/outputs/outputs/model` .

    Model yapılandırmalarınızı içeren bir dosya aşağıdaki gibi benzer içerikle Visual Studio Code görüntülenir:

    ```json
    {
        "modelName": "MNIST-TensorFlow-model",
        "tags": {
            "": ""
        },
        "modelPath": "c:\\Dev\\vscode-tools-for-ai\\mnist-vscode-docs-sample\\azureml_outputs\\Run_1\\outputs\\outputs\\model",
        "description": ""
    }
    ```

1. Yapılandırmanızla memnun olduktan sonra, komut paletini açıp aşağıdaki komutu girerek dosyayı kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

Birkaç dakika sonra model *modeller* düğümünün altında görüntülenir.

## <a name="deploy-the-model"></a>Modeli dağıtma

Visual Studio Code, modelinizi bir Web hizmeti olarak dağıtabilirsiniz:

+ Azure Container Instances (ACI).
+ Azure Kubernetes hizmeti (AKS).

ACI kapsayıcıları gerektiği şekilde oluşturulduğundan, önceden test etmek için bir ACI kapsayıcısı oluşturmanız gerekmez. Ancak, AKS kümelerini önceden yapılandırmanız gerekir. Dağıtım seçenekleri hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md) .

Bir Web hizmetini bir ACI olarak dağıtmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir.
1. Abonelik düğümünü genişletin. 
1. **Teamworkspace > modeller** düğümünü genişletin. 
1. **Mnist-TensorFlow-model** ' e sağ tıklayın ve **kayıtlı modelden Hizmeti Dağıt**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Modeli dağıtma](./media/tutorial-train-deploy-image-classification-model-vscode/deploy-model.png)

1. **Azure Container Instances**seçin.
1. Hizmetinizi "mnist-TensorFlow-svc" olarak adlandırın ve **ENTER**tuşuna basın.
1. Giriş kutusuna **ENTER** tuşuna basarak ve dizindeki dosyaya göz atarak kapsayıcıda çalıştırılacak betiği seçin `score.py` `mnist-vscode-docs-sample` .
1. Giriş kutusuna **ENTER** tuşuna basarak ve dizindeki dosyaya göz atarak betiği çalıştırmak için gereken bağımlılıkları sağlayın `env.yml` `mnist-vscode-docs-sample` .

    Model yapılandırmalarınızı içeren bir dosya aşağıdaki gibi benzer içerikle Visual Studio Code görüntülenir:

    ```json
    {
        "name": "mnist-tensorflow-svc",
        "imageConfig": {
            "runtime": "python",
            "executionScript": "score.py",
            "dockerFile": null,
            "condaFile": "env.yml",
            "dependencies": [],
            "schemaFile": null,
            "enableGpu": false,
            "description": ""
        },
        "deploymentConfig": {
            "cpu_cores": 1,
            "memory_gb": 10,
            "tags": {
                "": ""
            },
            "description": ""
        },
        "deploymentType": "ACI",
        "modelIds": [
            "MNIST-TensorFlow-model:1"
        ]
    }
    ```

1. Yapılandırmanızla memnun olduktan sonra, komut paletini açıp aşağıdaki komutu girerek dosyayı kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

Bu noktada, Web hizmetinizi dağıtmak üzere Azure 'a bir istek gönderilir. Bu işlem birkaç dakika sürer. Dağıtıldıktan sonra yeni hizmet *uç noktalar* düğümünün altında görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

* Visual Studio Code dışında Azure Machine Learning eğitme hakkında yönergeler için bkz. [öğretici: modelleri Azure Machine Learning Ile eğitme](tutorial-train-models-with-aml.md).
* Kodu yerel olarak düzenleme, çalıştırma ve hata ayıklama hakkında yönergeler için bkz. [Python Hello-World öğreticisi](https://code.visualstudio.com/docs/Python/Python-tutorial).

