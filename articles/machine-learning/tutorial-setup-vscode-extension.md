---
title: 'Öğretici: Azure Machine Learning Visual Studio Code uzantısını ayarlama'
titleSuffix: Azure Machine Learning
description: Visual Studio Code Azure Machine Learning uzantısını ayarlamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 01/16/2019
ms.openlocfilehash: 4000fcc80d507d3b1e871d7f3288fc3b77693c76
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76157615"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Azure Machine Learning Visual Studio Code uzantısını ayarlama

Azure Machine Learning Visual Studio Code uzantısını kullanarak betikleri yüklemeyi ve çalıştırmayı öğrenin.

Bu öğreticide, aşağıdaki görevleri öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning Visual Studio Code uzantısını yükler
> * Visual Studio Code Azure hesabınızda oturum açın
> * Örnek betik çalıştırmak için Azure Machine Learning uzantısını kullanın

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Bir hesabınız yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)denemek için kaydolun.
- Hafif, platformlar arası bir kod Düzenleyicisi [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)yükler. 

## <a name="install-the-extension"></a>Uzantıyı yükleme

1. Visual Studio Code'u açın.
1. Uzantılar görünümünü açmak için **etkinlik çubuğundan** **Uzantılar** simgesini seçin.
1. Uzantılar görünümünde, "Azure Machine Learning" araması yapın.
1. **Yükle**’yi seçin.

> [!NOTE]
> Alternatif olarak, [yükleyiciyi doğrudan indirerek](https://aka.ms/vscodetoolsforai)Azure Machine Learning uzantısını Visual Studio Market aracılığıyla yükleyebilirsiniz. 

Bu öğreticideki adımların geri kalanı, uzantının **0.6.8 sürümü** ile test edilmiştir.

## <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açın

Azure 'da kaynak sağlamak ve iş yüklerini çalıştırmak için Azure hesabı kimlik bilgilerinizle oturum açmanız gerekir. Hesap yönetimine yardımcı olmak için Azure Machine Learning Azure hesap uzantısını otomatik olarak yüklenir. [Azure Hesap uzantısı hakkında daha fazla bilgi edinmek](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)için aşağıdaki siteyi ziyaret edin.

1. Menü çubuğundan **görüntüle > komut paleti** ' ni seçerek komut paleti ' ni açın. 
1. Oturum açma işlemini başlatmak için metin kutusuna "Azure: oturum aç" komutunu girin.

## <a name="run-a-script-in-azure"></a>Azure 'da betik çalıştırma

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
        "workspace": "WS12191742",
        "resourceGroup": "WS12191742-rg2",
        "location": "South Central US",
        "experiment": "WS12191742-exp2",
        "compute": {
            "name": "WS12191742-com2",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS12191742-com2-rc1",
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

1. Deneme hesabınızı Azure 'da çalıştırmak için **deneme gönder** ' i seçin. Bu, `train.py` ve yapılandırma dosyasını Azure Machine Learning çalışma alanınıza gönderir. Daha sonra eğitim işi, Azure 'daki bir işlem kaynağında başlatılır.
1. Birkaç dakika sonra, `output` adlı bir dizin, eğitimli bir TensorFlow modeli içeren yerel olarak oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Machine Learning Visual Studio Code uzantısını kullanarak görüntü sınıflandırması TensorFlow modelini eğitme ve dağıtma](tutorial-train-deploy-image-classification-model-vscode.md).
