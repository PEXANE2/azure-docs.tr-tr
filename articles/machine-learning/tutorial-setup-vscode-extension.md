---
title: 'Öğretici: Visual Studio Code uzantısını ayarlama'
titleSuffix: Azure Machine Learning
description: Visual Studio Code Azure Machine Learning uzantısını ayarlamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 02/24/2020
ms.openlocfilehash: 583071ee22e4fb9cffc741520b1583790002a5bf
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77604839"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Azure Machine Learning Visual Studio Code uzantısını ayarlama

Azure Machine Learning Visual Studio Code uzantısını kullanarak betikleri yüklemeyi ve çalıştırmayı öğrenin.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning Visual Studio Code uzantısını yükler
> * Visual Studio Code Azure hesabınızda oturum açın
> * Örnek betik çalıştırmak için Azure Machine Learning uzantısını kullanın

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Bir hesabınız yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)denemek için kaydolun.
- Visual Studio Code. Bunu yapmazsanız, [yükleyebilirsiniz](https://code.visualstudio.com/docs/setup/setup-overview).
- [Python 3](https://www.python.org/downloads/)

## <a name="install-the-extension"></a>Uzantıyı yükleme

1. Visual Studio Code'u açın.
1. Uzantılar görünümünü açmak için **etkinlik çubuğundan** **Uzantılar** simgesini seçin.
1. Uzantılar görünümünde, "Azure Machine Learning" araması yapın.
1. **Yükle**’yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Azure Machine Learning VS Code uzantısını yükler](./media/tutorial-setup-vscode-extension/install-aml-vscode-extension.PNG)

> [!NOTE]
> Alternatif olarak, [yükleyiciyi doğrudan indirerek](https://aka.ms/vscodetoolsforai)Azure Machine Learning uzantısını Visual Studio Market aracılığıyla yükleyebilirsiniz. 

Bu öğreticideki adımların geri kalanı, uzantının **0.6.8 sürümü** ile test edilmiştir.

## <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açın

Azure 'da kaynak sağlamak ve iş yüklerini çalıştırmak için Azure hesabı kimlik bilgilerinizle oturum açmanız gerekir. Hesap yönetimine yardımcı olmak için Azure Machine Learning Azure hesap uzantısını otomatik olarak yüklenir. [Azure Hesap uzantısı hakkında daha fazla bilgi edinmek](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)için aşağıdaki siteyi ziyaret edin.

1. Menü çubuğundan **görüntüle > komut paleti** ' ni seçerek komut paleti ' ni açın. 
1. Oturum açma işlemini başlatmak için komut paletinde "Azure: oturum aç" komutunu girin.

## <a name="run-a-machine-learning-model-training-script-in-azure"></a>Azure 'da Machine Learning modeli eğitim betiği çalıştırma

Azure 'da hesap kimlik bilgilerinizle oturum açmış olduğunuza göre, bu bölümdeki adımları kullanarak, bir makine öğrenimi modelini eğitme için uzantıyı nasıl kullanacağınızı öğrenin.

1. [AI deposu için vs Code araçları](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) 'nı bilgisayarınızda herhangi bir yere indirip sıkıştırmasını açın.
1. Visual Studio Code `mnist-vscode-docs-sample` dizini ' ni açın.
1. Etkinlik çubuğunda **Azure** simgesini seçin.
1. Azure Machine Learning görünümünün en üstündeki **deneme Çalıştır** simgesini seçin.

    > [!div class="mx-imgBorder"]
    > ![çalıştırmayı deneyin](./media/tutorial-setup-vscode-extension/run-experiment.PNG)

1. Komut paleti genişlediğinde, istemleri izleyin.

    1. Azure aboneliğinizi seçin.
    1. **Yeni Azure ML çalışma alanı oluştur** ' u seçin
    1. **TensorFlow tek düğümlü eğitim** işi türünü seçin.
    1. Tren betiği olarak `train.py` girin. Bu, el ile yazılan basamakların görüntülerini sınıflandırmakta olan Machine Learning modeline kod içeren dosyadır.
    1. Aşağıdaki paketleri çalıştırmak için gereken gereksinimleri belirtin.

        ```text
        pip: azureml-defaults; conda: python=3.6.2, tensorflow=1.15.0
        ```

1. Bu noktada, aşağıdaki aşağıdakine benzer bir yapılandırma dosyası metin düzenleyicisinde görünür. Yapılandırma, modeli eğitmek için kodu ve önceki adımda belirtilen Python bağımlılıklarını içeren dosya gibi eğitim işini çalıştırmak için gereken bilgileri içerir.

    ```json
    {
        "workspace": "WS01311608",
        "resourceGroup": "WS01311608-rg1",
        "location": "South Central US",
        "experiment": "WS01311608-exp1",
        "compute": {
            "name": "WS01311608-com1",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS01311608-com1-rc1",
            "condaDependencies": [
                "python=3.6.2",
                "tensorflow=1.15.0"
            ],
            "pipDependencies": [
                "azureml-defaults"
            ]
        }
    }
    ```

1. Yapılandırmanızla memnun olduktan sonra, komut paletini açıp aşağıdaki komutu girerek denemenize iletin:

    ```text
    Azure ML: Submit Experiment
    ```

    Bu, `train.py` ve yapılandırma dosyasını Azure Machine Learning çalışma alanınıza gönderir. Daha sonra eğitim işi, Azure 'daki bir işlem kaynağında başlatılır.

### <a name="track-the-progress-of-the-training-script"></a>Eğitim betiğinin ilerlemesini izleme

Komut dosyanızı çalıştırmak birkaç dakika sürebilir. İlerleme durumunu izlemek için:

1. Etkinlik çubuğundan **Azure** simgesini seçin.
1. Abonelik düğümünü genişletin.
1. Şu anda çalışan denemenizin düğümünü genişletin. Bu, çalışma alanınızın ve denemenizin değerlerinin yapılandırma dosyasında tanımlanan özelliklerle aynı olduğu `{workspace}/Experiments/{experiment}` düğümü içinde bulunur.
1. Denemenin tüm çalıştırmaları ve durumları listelenir. En son durumu almak için Azure Machine Learning görünümünün en üstündeki Yenile simgesine tıklayın.

    > [!div class="mx-imgBorder"]
    > ![deneme Ilerlemesini Izleme](./media/tutorial-setup-vscode-extension/track-experiment-progress.PNG)

### <a name="download-the-trained-model"></a>Eğitilen modeli indirin

Deneme çalıştırması tamamlandığında, çıktı eğitilen bir modeldir. Çıkışları yerel olarak indirmek için:

1. En son çalıştırmaya sağ tıklayın ve **çıkışları indir**' i seçin.

    > [!div class="mx-imgBorder"]
    > Eğitilen modeli Indirmek ![](./media/tutorial-setup-vscode-extension/download-trained-model.PNG)

1. Çıktıların kaydedileceği konumu seçin.
1. Çalıştırdığınız adı taşıyan bir klasör yerel olarak indirilir. Bu sayfaya gidin.
1. Model dosyaları `outputs/outputs/model` dizininin içindedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Machine Learning Visual Studio Code uzantısını kullanarak görüntü sınıflandırması TensorFlow modelini eğitme ve dağıtma](tutorial-train-deploy-image-classification-model-vscode.md).
