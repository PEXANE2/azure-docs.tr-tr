---
title: 'Öğretici: Visual Studio Code uzantısını ayarlama'
titleSuffix: Azure Machine Learning
description: Visual Studio Code Azure Machine Learning uzantısını nasıl ayarlayabilirsiniz öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 04/13/2020
ms.openlocfilehash: 731ab18346ac9f100862174312c2c9950026f1eb
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272941"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Azure Machine Learning Visual Studio Code uzantısını ayarlama

Azure Machine Learning Visual Studio Code uzantısını kullanarak komut dosyalarını nasıl yükleyip çalıştırılacak yapılacağını öğrenin.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning Visual Studio Code uzantısını yükleyin
> * Visual Studio Kodu'ndan Azure hesabınızda oturum açma
> * Örnek bir komut dosyası çalıştırmak için Azure Machine Learning uzantısını kullanma

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Yoksa, Azure Machine Learning'in ücretsiz [veya ücretli sürümünü](https://aka.ms/AMLFree)denemek için kaydolun.
- Visual Studio Code. Eğer yoksa, [yükleyin.](https://code.visualstudio.com/docs/setup/setup-overview)
- [Python 3](https://www.python.org/downloads/)

## <a name="install-the-extension"></a>Uzantıyı yükleme

1. Visual Studio Code'u açın.
1. Uzanlar görünümünü açmak için **Etkinlik Çubuğu'ndan** **Uzanlar** simgesini seçin.
1. Uzantılar görünümünde "Azure Machine Learning"i arayın.
1. **Yükle**’yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Azure Machine Learning VS Kod Uzantısını Yükleyin](./media/tutorial-setup-vscode-extension/install-aml-vscode-extension.PNG)

> [!NOTE]
> Alternatif olarak, [yükleyiyi doğrudan indirerek](https://aka.ms/vscodetoolsforai)Visual Studio Marketplace üzerinden Azure Machine Learning uzantısını yükleyebilirsiniz. 

Bu öğreticideki adımların geri kalanı, uzantının **0.6.8 sürümüyle** test edilmiştir.

## <a name="sign-in-to-your-azure-account"></a>Azure Hesabınızda Oturum Açın

Azure'da kaynak sağlamak ve iş yüklerini çalıştırmak için Azure hesap kimlik bilgilerinizle oturum açmanız gerekir. Azure Machine Learning, hesap yönetimine yardımcı olmak için Azure Hesabı uzantısını otomatik olarak yükler. Azure Hesabı uzantısı hakkında daha fazla bilgi edinmek için aşağıdaki siteyi ziyaret [edin.](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

1. Menü çubuğundan **> Komut Paleti'ni** seçerek komut paletini açın. 
1. Oturum açma işlemini başlatmak için komut paletine "Azure: Oturum Aç" komutunu girin.

## <a name="run-a-machine-learning-model-training-script-in-azure"></a>Azure'da makine öğrenimi modeli eğitim komut dosyası çalıştırma

Artık azure'da hesap kimlik bilgilerinizle oturum açtıysanız, bir makine öğrenimi modelini eğitmek için uzantıyı nasıl kullanacağınızı öğrenmek için bu bölümdeki adımları kullanın.

1. Bilgisayarınızın herhangi bir yerindeki [AI deposu için VS Kod Araçları'nı](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) indirin ve fermuarını açın.
1. Visual `mnist-vscode-docs-sample` Studio Code'da dizini açın.
1. Etkinlik Çubuğu'ndaki **Azure** simgesini seçin.
1. Azure Machine Learning View'ın üst kısmındaki **Denemeyi Çalıştır** simgesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Denemeyi Çalıştır](./media/tutorial-setup-vscode-extension/run-experiment.PNG)

1. Komut paleti genişlettiğinde, istemleri izleyin.

    1. Azure aboneliğinizi seçin.
    1. Ortamlar listesinden, **Conda bağımlılıkları dosyasını**seçin.
    1. Conda bağımlılıkları dosyasına göz atmak için **Enter** tuşuna basın. Bu dosya, komut dosyanızı çalıştırmak için gereken bağımlılıkları içerir. Bu durumda, bağımlılıklar dosyası `env.yml` `mnist-vscode-docs-sample` dizin içindeki dosyadır.
    1. Eğitim komut dosyası dosyası dosyasına göz atmak için **Enter** tuşuna basın. Bu, el yazısı yla yazılmış basamakgörüntülerini kategorilere ayıran bir makine öğrenme modelinin kodunu içeren dosyadır. Bu durumda, modeli eğitmek için komut `train.py` dosyası `mnist-vscode-docs-sample` dizinin içindeki dosyadır.

1. Bu noktada, metin düzenleyicisinde aşağıdakine benzer bir yapılandırma dosyası görüntülenir. Yapılandırma, modeli ve önceki adımda belirtilen Python bağımlılıklarını eğitmek için kodu içeren dosya gibi eğitim işini çalıştırmak için gereken bilgileri içerir.

    ```json
    {
        "workspace": "WS04131142",
        "resourceGroup": "WS04131142-rg1",
        "location": "South Central US",
        "experiment": "WS04131142-exp1",
        "compute": {
            "name": "WS04131142-com1",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS04131142-com1-rc1",
            "environment": {
                "name": "WS04131142-env1",
                "conda_dependencies": [
                    "python=3.6.2",
                    "tensorflow=1.15.0",
                    "pip"
                ],
                "pip_dependencies": [
                    "azureml-defaults"
                ],
                "environment_variables": {}
            }
        }
    }
    ```

1. Yapılandırmanızdan memnun kaldıktan sonra, komut paletini açarak ve aşağıdaki komutu girerek denemenizi gönderin:

    ```text
    Azure ML: Submit Experiment
    ```

    Bu, `train.py` yapılandırma dosyasını Azure Machine Learning çalışma alanınıza gönderir. Eğitim işi daha sonra Azure'daki bir bilgi işlem kaynağında başlatılır.

### <a name="track-the-progress-of-the-training-script"></a>Eğitim komut dosyasının ilerlemesini izleme

Komut dosyanızı çalıştırmak birkaç dakika sürebilir. İlerlemesini izlemek için:

1. Etkinlik çubuğundan **Azure** simgesini seçin.
1. Abonelik düğümünüzgenişletin.
1. Şu anda çalışan denemenizin düğüm'üne genişletin. Bu, çalışma alanınız ve denemeniz için değerlerin yapılandırma dosyasında tanımlanan özelliklerle aynı olduğu `{workspace}/Experiments/{experiment}` düğümün içinde yer alır.
1. Denemenin tüm çalıştırmaları ve durumları listelenir. En son durumu almak için Azure Machine Learning View'ın üst kısmındaki yenileme simgesini tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Deneme İlerlemeyi İzleme](./media/tutorial-setup-vscode-extension/track-experiment-progress.PNG)

### <a name="download-the-trained-model"></a>Eğitimli modeli indirin

Deneme çalışması tamamlandığında, çıktı eğitimli bir modeldir. Çıktıları yerel olarak indirmek için:

1. En son çalıştır'a sağ tıklayın ve **İndirme Çıktılarını**seçin.

    > [!div class="mx-imgBorder"]
    > ![Eğitimli Modeli İndir](./media/tutorial-setup-vscode-extension/download-trained-model.PNG)

1. Çıktıları kaydetmek için bir konum seçin.
1. Çalışmanızın adını içeren bir klasör yerel olarak indirilir. Bu sayfaya gidin.
1. Model dosyaları `outputs/outputs/model` dizinin içindedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Machine Learning Visual Studio Code Extension'ı kullanarak bir görüntü sınıflandırma TensorFlow modelini eğitin ve dağıtın.](tutorial-train-deploy-image-classification-model-vscode.md)
