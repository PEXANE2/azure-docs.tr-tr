---
title: 'Öğretici: Visual Studio Code uzantısını kullanarak bir modeli eğitin ve dağıtın'
titleSuffix: Azure Machine Learning
description: TensorFlow ve Azure Machine Learning Visual Studio Code Extension'ı kullanarak bir görüntü sınıflandırma modelini nasıl eğitip dağıttığınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 04/13/2020
ms.openlocfilehash: f793f8c4cb84f821c098cc5ce98e693d272e725f
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272823"
---
# <a name="train-and-deploy-an-image-classification-tensorflow-model-using-the-azure-machine-learning-visual-studio-code-extension"></a>Azure Machine Learning Visual Studio Code Extension'ı kullanarak bir görüntü sınıflandırma TensorFlow modelini eğitin ve dağıtın

TensorFlow ve Azure Machine Learning Visual Studio Code Extension'ı kullanarak elle yazılmış numaraları tanımak için bir görüntü sınıflandırma modelini nasıl eğitip dağıttıyacağınızı öğrenin.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Kodu anlama
> * Çalışma alanı oluşturma
> * Deneme oluşturma
> * Bilgisayar Hedeflerini Yapılandırma
> * Yapılandırma dosyası çalıştırma
> * Modeli eğitme
> * Bir model kaydetme
> * Model dağıtma

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Yoksa, Azure Machine Learning'in ücretsiz [veya ücretli sürümünü](https://aka.ms/AMLFree)denemek için kaydolun.
- [Hafif,](https://code.visualstudio.com/docs/setup/setup-overview)platformlar arası kod düzenleyicisi olan Visual Studio Code'u yükleyin.
- Azure Machine Learning Studio Visual Studio Code uzantısı. Yükleme yönergeleri için [Kurulum Azure Machine Learning Visual Studio Code uzantı lı öğreticiye](./tutorial-setup-vscode-extension.md) bakın

## <a name="understand-the-code"></a>Kodu anlama

Bu öğreticinin kodu, 0-9 arası el yazısı rakamları kategorize eden bir görüntü sınıflandırma makinesi öğrenme modelini eğitmek için TensorFlow'u kullanır. Bunu, 28 px x 28 px görüntünün piksel değerlerini giriş olarak alan ve sınıflandırılan her basamak için bir tane olmak üzere 10 olasılıklı bir liste çıkaran bir sinir ağı oluşturarak yapar. Aşağıda verilerin neye benzede olduğuna bir örnek verilmiştir.  

![MNIST Rakamları](./media/tutorial-train-deploy-image-classification-model-vscode/digits.png)

Bilgisayarınızın herhangi bir yerindeki [AI deposu için VS Code Tools'u](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) indirip gününüzü kaldırarak bu öğreticinin kodunu alın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning'de bir uygulama oluşturmak için yapmanız gereken ilk şey bir çalışma alanı oluşturmaktır. Çalışma alanı, modelleri eğitmek için gereken kaynakları ve eğitimli modelleri kendileri içerir. Daha fazla bilgi için [çalışma alanının ne olduğunu](./concept-workspace.md)görün. 

1. Visual Studio Code etkinlik çubuğunda, Azure Machine Learning görünümünü açmak için **Azure** simgesini seçin.
1. Azure aboneliğinize sağ tıklayın ve **Çalışma Alanı Oluştur'u**seçin. 
    
    > [!div class="mx-imgBorder"]
    > ![Çalışma Alanı oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-workspace.png)

1. Varsayılan olarak, oluşturma tarihi ve saatini içeren bir ad oluşturulur. Metin giriş kutusunda, adı "TeamWorkspace" olarak değiştirin ve **Enter**tuşuna basın.
1. **Yeni bir kaynak grubu oluştur'u**seçin. 
1. Kaynak grubunuzun adını "TeamWorkspace-rg" olarak adlandırın ve **Enter**tuşuna basın. 
1. Çalışma alanınız için bir konum seçin. Modelinizi dağıtmayı planladığınız konuma en yakın konumu seçmeniz önerilir. Örneğin, "Batı ABD 2".
1. Çalışma alanı türünü seçmek istendiğinde, temel bir çalışma alanı oluşturmak için **Temel'i** seçin. Farklı çalışma alanı teklifleri hakkında daha fazla bilgi için Azure [Machine Learning'e genel bakış](./overview-what-is-azure-ml.md#sku)bilgisi için bkz.

Bu noktada, hesabınızda yeni bir çalışma alanı oluşturmak için Azure'a bir istek yapılır. Birkaç dakika sonra, yeni çalışma alanı abonelik düğümünüzde görünür. 

## <a name="create-an-experiment"></a>Deneme oluşturma

Tek tek model eğitim çalıştırmalarını izlemek ve analiz etmek için çalışma alanınızda bir veya daha fazla deneme oluşturulabilir. Çalıştırmalar Azure bulutunda veya yerel makinenizde yapılabilir.

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir.
1. Abonelik düğümünüzgenişletin.
1. **TeamWorkspace** düğümlerini genişletin. 
1. **Denemeler** düğümüne sağ tıklayın.
1. Bağlam menüsünden **Deneme Oluştur'u** seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-experiment.png)

1. Denemenize "MNIST" adını girin ve yeni denemeyi oluşturmak için **Enter** tuşuna basın. 

Çalışma alanları gibi, sağlanan yapılandırmalarla deneme oluşturmak için Azure'a bir istek gönderilir. Birkaç dakika sonra, yeni deneme çalışma alanınızın *Denemeler* düğümünde görünür. 

## <a name="configure-compute-targets"></a>İşlem Hedeflerini Yapılandırma

Bilgi işlem hedefi, komut dosyalarını çalıştırdığınız ve eğitimli modelleri dağıttığınız bilgi işlem kaynağıdır veya ortamdır. Daha fazla bilgi için [Azure Machine Learning bilgi işlem hedeflerini n](./concept-compute-target.md)için görün.

Bir işlem hedefi oluşturmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir. 
1. Abonelik düğümünüzgenişletin. 
1. **TeamWorkspace** düğümlerini genişletin. 
1. Çalışma alanı düğümünün altında, **Bilgi İşlem** düğümüne sağ tıklayın ve Bilgi **İşlem Oluştur'u**seçin. 

    > [!div class="mx-imgBorder"]
    > ![İşlem hedefi oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-compute.png)

1. **Azure Machine Learning Compute (AmlCompute) seçeneğini belirleyin.** Azure Machine Learning Compute, kullanıcının çalışma alanınızdaki diğer kullanıcılarla birlikte kullanılabilecek tek veya çok düğümlü bir işlem oluşturmasını sağlayan yönetilen bir bilgi işlem altyapısıdır.
1. Bir VM boyutu seçin. Seçenekler listesinden **Standard_F2s_v2'ı** seçin. VM'nizin boyutu, modellerinizi eğitmek için gereken süreyi etkiler. VM boyutları hakkında daha fazla bilgi için [Azure'daki Linux sanal makinelerinin boyutlarına](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)bakın.
1. İşleminizi "TeamWkspc-com" olarak adlandırın ve işleminizi oluşturmak için **Enter** tuşuna basın.

    VS Kodu'nda aşağıdakine benzer içeriğe sahip bir dosya görünür:

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
                    "nodeIdleTimeBeforeScaleDown": 120
                },
                "userAccountCredentials": {
                    "adminUserName": "",
                    "adminUserPassword": "",
                    "adminUserSshPublicKey": ""
                },
                "subnetName": "",
                "vnetName": "",
                "vnetResourceGroupName": "",
                "remoteLoginPortPublicAccess": ""
            }
        }
    }
    ```

1. Yapılandırmadan memnun kaldığında, **Komut Paleti >'yi**>'yi seçerek komut paletini açın.
1. Çalıştır yapılandırma dosyanızı kaydetmek için komut paletine aşağıdaki komutu girin.

    ```text
    Azure ML: Save and Continue
    ```

Birkaç dakika sonra, yeni işlem hedefi çalışma alanınızın *İşlem* düğümünde görünür.

## <a name="create-a-run-configuration"></a>Çalıştır yapılandırması oluşturma

Bir işlem hedefine bir eğitim çalışması gönderdiğinde, eğitim işini çalıştırmak için gereken yapılandırmayı da gönderirsiniz. Örneğin, eğitim kodunu ve python bağımlılıklarını içeren komut dosyası çalıştırmak için gereklidir.

Çalıştır yapılandırması oluşturmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir. 
1. Abonelik düğümünüzgenişletin. 
1. **TeamWorkspace > İşlem** düğümlerini genişletin. 
1. Bilgi işlem düğümü altında **TeamWkspc-com** compute düğüme sağ tıklayın ve **Çalıştır Yapılandırması Oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Çalıştır yapılandırması oluşturma](./media/tutorial-train-deploy-image-classification-model-vscode/create-run-configuration.png)

1. Run yapılandırmanızı "MNIST-rc" olarak adlandırın ve çalıştıryapılandırmanızı oluşturmak için **Enter** tuşuna basın.
1. Ardından, **yeni Azure ML Ortamı Oluştur'u**seçin. Ortamlar, komut dosyalarınızı çalıştırmak için gereken bağımlılıkları tanımlar.
1. Çevrenize "MNIST-env" adını ve **Enter**tuşuna basın.
1. Listeden **Conda bağımlılıkları dosyasını** seçin.
1. Conda bağımlılıkları dosyasına göz atmak için **Enter** tuşuna basın. Bu durumda, bağımlılıklar dosyası `env.yml` `vscode-tools-for-ai/mnist-vscode-docs-sample` dizin içindeki dosyadır.

    VS Kodu'nda aşağıdakine benzer içeriğe sahip bir dosya görünür:

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

1. Yapılandırmanızdan memnun olduğunuzda, komut paletini açıp aşağıdaki komutu girerek kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

1. İşlemde çalıştırmak için komut dosyası dosyası dosyasına göz atmak için **Enter** tuşuna basın. Bu durumda, modeli eğitmek için komut `train.py` dosyası `vscode-tools-for-ai/mnist-vscode-docs-sample` dizinin içindeki dosyadır.

    Vs Kodu'nda aşağıdakine benzer içeriğe sahip bir dosya görünür: `MNIST-rc.runconfig`

    ```json
    {
        "script": "train.py",
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

1. Yapılandırmanızdan memnun olduğunuzda, komut paletini açıp aşağıdaki komutu girerek kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

Run *yapılandırması TeamWkspc-compute* düğümü altında eklenir `MNIST-env` ve ortam yapılandırması *Ortamdüğümü* altında eklenir. `MNIST-rc`

## <a name="train-the-model"></a>Modeli eğitme

Eğitim sürecinde, bir TensorFlow modeli, sınıflandırılan ilgili basamakların her biri için içine gömülü eğitim verileri ve öğrenme kalıpları işleyerek oluşturulur. 

Azure Machine Learning denemesini çalıştırmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir. 
1. Abonelik düğümünüzgenişletin. 
1. **TeamWorkspace > Denemeler** düğümlerini genişletin. 
1. **MNIST** deneyine sağ tıklayın.
1. **Denemeyi Çalıştır'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme çalıştırma](./media/tutorial-train-deploy-image-classification-model-vscode/run-experiment.png)

1. İşlem hedef seçenekleri listesinden **TeamWkspc-comcome** hedefini seçin.
1. Ardından, **MNIST-rc** çalıştır yapılandırmasını seçin.
1. Bu noktada, denemenizi çalışma alanınızda seçili bilgi işlem hedefiüzerinde çalıştırmak için Azure'a bir istek gönderilir. Bu işlem birkaç dakika sürer. Eğitim işini çalıştırmak için gereken süre, bilgi işlem türü ve eğitim veri boyutu gibi çeşitli faktörlerden etkilenir. Denemenizin ilerlemesini izlemek için geçerli çalıştır düğümüne sağ tıklayın ve **Azure portalında Çalıştır'ı görüntüle'yi**seçin.
1. Harici bir web sitesi açmak isteyen iletişim kutusu görüntülendiğinde **Aç'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme ilerlemesini izleme](./media/tutorial-train-deploy-image-classification-model-vscode/track-experiment-progress.png)

Model eğitim tamamlandığında, run düğümünün yanındaki durum etiketi "Tamamlandı" için güncellenir.

## <a name="register-the-model"></a>Modeli kaydedin

Modelinizi eğittiğinize göre, modelinizi çalışma alanınıza kaydedebilirsiniz. 

Modelinizi kaydetmek için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir.
1. Abonelik düğümünüzgenişletin. 
1. **TeamWorkspace > Denemeler > MNIST** düğümlerini genişletin.
1. Modeli eğitmekten oluşturulan model çıktılarını alın. **Çalıştır 1** çalıştır düğümüne sağ tıklayın ve **İndir çıkışları**seçin. 

    > [!div class="mx-imgBorder"]
    > ![Model çıktılarını indirin](./media/tutorial-train-deploy-image-classification-model-vscode/download-outputs.png)

1. İndirilen çıktıları kaydetmek için dizini seçin. Varsayılan olarak, çıktılar şu anda Visual Studio Code'da açılan dizine yerleştirilir.
1. **Modeller** düğümüne sağ tıklayın ve **Model'i kaydedin.**

    > [!div class="mx-imgBorder"]
    > ![Bir model kaydetme](./media/tutorial-train-deploy-image-classification-model-vscode/register-model.png)

1. Modelinizi "MNIST-TensorFlow modeli" olarak adlandırın ve **Enter**tuşuna basın.
1. TensorFlow modeli çeşitli dosyalardan oluşur. Seçenekler listesinden model yolu biçimi olarak **Model klasörünü** seçin. 
1. Dizini `azureml_outputs/Run_1/outputs/outputs/model` seçin.

    Model yapılandırmalarınızı içeren bir dosya Visual Studio Code'da aşağıdakine benzer içeriğe sahip görünür:

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

1. Yapılandırmanızdan memnun olduğunuzda, komut paletini açıp aşağıdaki komutu girerek kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

Birkaç dakika sonra, model *Modeller* düğümünün altında görünür.

## <a name="deploy-the-model"></a>Modeli dağıtma

Visual Studio Code'da, modelinizi bir web hizmeti olarak şularına dağıtabilirsiniz:

+ Azure Kapsayıcı Örnekleri (ACI).
+ Azure Kubernetes Hizmeti (AKS).

ACI kapsayıcıları gerektiği gibi oluşturulduğundan, önceden test etmek için bir ACI kapsayıcıoluşturmanız gerekmez. Ancak, AKS kümelerini önceden yapılandırmanız gerekir. Dağıtım seçenekleri hakkında daha fazla bilgi için [Azure Machine Learning ile dağıtım modellerine](how-to-deploy-and-where.md) bakın.

Bir web hizmetini ACI olarak dağıtmak için:

1. Visual Studio Code etkinlik çubuğunda **Azure** simgesini seçin. Azure Machine Learning görünümü görüntülenir.
1. Abonelik düğümünüzgenişletin. 
1. **TeamWorkspace > Modeller** düğümlerini genişletin. 
1. **MNIST-TensorFlow modeline** sağ tıklayın ve **Kayıtlı Modelden Hizmeti Dağıt'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Modeli dağıtma](./media/tutorial-train-deploy-image-classification-model-vscode/deploy-model.png)

1. **Azure Kapsayıcı Örnekleri'ni**seçin.
1. Hizmetinizi "mnist-tensorflow-svc" olarak adlandırın ve **Enter**tuşuna basın.
1. Giriş kutusuna **Enter** tuşuna basarak ve dizindeki `score.py` dosyaya göz atarak `mnist-vscode-docs-sample` kapsayıcıda çalışacak komut dosyasını seçin.
1. Giriş kutusuna **Gir'e** basarak ve `env.yml` `mnist-vscode-docs-sample` dizindeki dosyaya göz atarak komut dosyasını çalıştırmak için gereken bağımlılıkları sağlayın.

    Model yapılandırmalarınızı içeren bir dosya Visual Studio Code'da aşağıdakine benzer içeriğe sahip görünür:

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

1. Yapılandırmanızdan memnun olduğunuzda, komut paletini açıp aşağıdaki komutu girerek kaydedin:

    ```text
    Azure ML: Save and Continue
    ```

Bu noktada, web hizmetinizi dağıtmak için Azure'a bir istek gönderilir. Bu işlem birkaç dakika sürer. Dağıtıldıktan sonra, yeni hizmet *Uç Noktaları* düğümüaltında görünür.

## <a name="next-steps"></a>Sonraki adımlar

* Visual Studio Code dışında Azure Machine Learning ile nasıl eğitim alınabildiğini öğrenmek için [Bkz.](tutorial-train-models-with-aml.md)
* Kodu yerel olarak nasıl kodlandırılamalı, çalıştırılır ve hata ayıklama konusunda bir yol için [Python merhaba dünyası öğreticisine](https://code.visualstudio.com/docs/Python/Python-tutorial)bakın.

