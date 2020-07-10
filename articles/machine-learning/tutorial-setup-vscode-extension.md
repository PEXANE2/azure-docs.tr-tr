---
title: 'Öğretici: Visual Studio Code uzantısını ayarlama (Önizleme)'
titleSuffix: Azure Machine Learning
description: Visual Studio Code Azure Machine Learning uzantısını ayarlamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 07/09/2020
ms.openlocfilehash: 48fb2ce3672634af202500ccb256e7730933fff8
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86199769"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension-preview"></a>Azure Machine Learning Visual Studio Code uzantısını ayarlama (Önizleme)

Azure Machine Learning Visual Studio Code uzantısını kullanarak betikleri yüklemeyi ve çalıştırmayı öğrenin.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning Visual Studio Code uzantısını yükler
> * Visual Studio Code Azure hesabınızda oturum açın
> * Örnek betik çalıştırmak için Azure Machine Learning uzantısını kullanın

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Bir hesabınız yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)denemek için kaydolun.
- Visual Studio Code. Bunu yapmazsanız, [yükleyebilirsiniz](https://code.visualstudio.com/docs/setup/setup-overview).
- [Python 3](https://www.python.org/downloads/)

## <a name="install-the-extension"></a>Uzantıyı yükleme

1. Visual Studio Code’u açın.
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
1. `mnist-vscode-docs-sample`Dizini Visual Studio Code açın.
1. Etkinlik çubuğunda **Azure** simgesini seçin.
1. Azure Machine Learning görünümünün en üstündeki **deneme Çalıştır** simgesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Deneme Çalıştır](./media/tutorial-setup-vscode-extension/run-experiment.PNG)

1. Komut paleti genişlediğinde, istemleri izleyin.

    > [!NOTE]
    > Zaten sağlanmış olan Azure Machine Learning kaynaklarınız varsa, bkz. [denemeleri vs Code in nasıl çalıştırılır?](./how-to-manage-resources-vscode.md#run-experiment)

    1. Azure aboneliğinizi seçin.
    1. Ortam listesinden **Conda Dependencies dosyası**' nı seçin.
    1. Conda Dependencies dosyasına gitmek için **ENTER** tuşuna basın. Bu dosya, komut dosyanızı çalıştırmak için gereken bağımlılıkları içerir. Bu durumda, bağımlılıklar dosyası `env.yml` dizinin içindeki dosyadır `mnist-vscode-docs-sample` .
    1. Eğitim betik dosyasına gitmek için **ENTER** tuşuna basın. Bu, el ile yazılan basamakların görüntülerini sınıflandırmakta olan Machine Learning modeline kod içeren dosyadır. Bu durumda, modeli eğitme betiği `train.py` Dizin içindeki dosyadır `mnist-vscode-docs-sample` .

1. Bu noktada, aşağıdaki aşağıdakine benzer bir yapılandırma dosyası metin düzenleyicisinde görünür. Yapılandırma, modeli eğitmek için kodu ve önceki adımda belirtilen Python bağımlılıklarını içeren dosya gibi eğitim işini çalıştırmak için gereken bilgileri içerir.

    ```json
    {
        "workspace": "WS06271500",
        "resourceGroup": "WS06271500-rg2",
        "location": "South Central US",
        "experiment": "WS06271500-exp1",
        "compute": {
            "name": "WS06271500-com1",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS06271500-com1-rc1",
            "environment": {
                "name": "WS06271500-env1",
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

1. Yapılandırmanızla memnun olduktan sonra, komut paletini açıp aşağıdaki komutu girerek denemenize iletin:

    ```text
    Azure ML: Submit Experiment
    ```

    Böylece, `train.py` ve yapılandırma dosyası Azure Machine Learning çalışma alanınıza gönderilir. Daha sonra eğitim işi, Azure 'daki bir işlem kaynağında başlatılır.

### <a name="track-the-progress-of-the-training-script"></a>Eğitim betiğinin ilerlemesini izleme

Komut dosyanızı çalıştırmak birkaç dakika sürebilir. İlerleme durumunu izlemek için:

1. Etkinlik çubuğundan **Azure** simgesini seçin.
1. Abonelik düğümünü genişletin.
1. Şu anda çalışan denemenizin düğümünü genişletin. Bu, `{workspace}/Experiments/{experiment}` çalışma alanınızın ve denemenizin değerlerinin yapılandırma dosyasında tanımlanan özelliklerle aynı olduğu düğüm içinde bulunur.
1. Denemenin tüm çalıştırmaları ve durumları listelenir. En son durumu almak için Azure Machine Learning görünümünün en üstündeki Yenile simgesine tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Deneme Ilerlemesini izleme](./media/tutorial-setup-vscode-extension/track-experiment-progress.PNG)

### <a name="download-the-trained-model"></a>Eğitilen modeli indirin

Deneme çalıştırması tamamlandığında, çıktı eğitilen bir modeldir. Çıkışları yerel olarak indirmek için:

1. En son çalıştırmaya sağ tıklayın ve **çıkışları indir**' i seçin.

    > [!div class="mx-imgBorder"]
    > ![Eğitilen modeli indir](./media/tutorial-setup-vscode-extension/download-trained-model.PNG)

1. Çıktıların kaydedileceği konumu seçin.
1. Çalıştırdığınız adı taşıyan bir klasör yerel olarak indirilir. Bu sayfaya gidin.
1. Model dosyaları `outputs/outputs/model` dizinin içindedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Machine Learning Visual Studio Code uzantısını kullanarak görüntü sınıflandırması TensorFlow modelini eğitme ve dağıtma](tutorial-train-deploy-image-classification-model-vscode.md).
