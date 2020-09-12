---
title: Visual Studio Code (Önizleme) içinde işlem örneğine bağlanma
titleSuffix: Azure Machine Learning
description: Visual Studio Code Azure Machine Learning bir işlem örneğine nasıl bağlanacağınızı öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: jmartens
author: j-martens
ms.date: 09/03/2020
ms.openlocfilehash: 2c7ff633705d3db327c563b41ce199a5342dda82
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89461570"
---
# <a name="connect-to-an-azure-machine-learning-compute-instance-in-visual-studio-code-preview"></a>Visual Studio Code bir Azure Machine Learning işlem örneğine bağlanma (Önizleme)

Bu makalede, Visual Studio Code kullanarak Azure Machine Learning bir işlem örneğine nasıl bağlanacağınızı öğreneceksiniz.

[Azure Machine Learning Işlem örneği](concept-compute-instance.md) , veri bilimcileri için tam olarak yönetilen bulut tabanlı bir iş istasyonudur ve BT yöneticileri için yönetim ve kurumsal hazırlık özellikleri sağlar.

Visual Studio Code bir işlem örneğine bağlanmak için kullanabileceğiniz iki yol vardır:

* Uzak Jupyter Notebook sunucusu. Bu seçenek, bir işlem örneğini uzak Jupyter Notebook sunucusu olarak ayarlamanıza olanak tanır.
* [Uzaktan geliştirme Visual Studio Code](https://code.visualstudio.com/docs/remote/remote-overview). Uzaktan geliştirme Visual Studio Code, Linux (WSL) için bir kapsayıcı, uzak makine veya Windows alt sistemini tam özellikli bir geliştirme ortamı olarak kullanmanıza olanak tanır.

## <a name="configure-compute-instance-as-remote-notebook-server"></a>İşlem örneğini uzak Not defteri sunucusu olarak yapılandırma

Bir işlem örneğini uzak bir Jupyter Notebook sunucusu olarak yapılandırmak için birkaç önkoşul olması gerekir:

* Azure Machine Learning Visual Studio Code uzantısı. Daha fazla bilgi için [Azure Machine Learning Visual Studio Code uzantısı kurulum kılavuzuna](tutorial-setup-vscode-extension.md)bakın.
* Azure Machine Learning çalışma alanı. Henüz bir tane yoksa [Yeni bir çalışma alanı oluşturmak için Azure Machine Learning Visual Studio Code uzantısını kullanın](how-to-manage-resources-vscode.md#create-a-workspace) .

Bir işlem örneğine bağlanmak için:

1. Visual Studio Code bir Jupyter Notebook açın.
1. Tümleşik Not Defteri Deneyimi yüklendiğinde **Jupyter sunucusu**' nu seçin.

    > [!div class="mx-imgBorder"]
    > ![Azure Machine Learning uzak Jupyter Not defteri sunucusu açılan menüsünü Başlat](media/how-to-set-up-vs-code-remote/launch-server-selection-dropdown.png)

    Alternatif olarak, komut paletini de kullanabilirsiniz:

    1. Menü çubuğundan **görüntüle > komut paleti** ' ni seçerek komut paleti ' ni açın.
    1. Metin kutusuna girin `Azure ML: Connect to Compute instance Jupyter server` .

1. `Azure ML Compute Instances`Jupyıter sunucu seçenekleri listesinden seçin.
1. Abonelikler listesinden aboneliğinizi seçin. Varsayılan Azure Machine Learning çalışma alanınızı daha önce yapılandırdıysanız, bu adım atlanır.
1. Çalışma alanınızı seçin.
1. Listeden işlem örneğinizi seçin. Bir tane yoksa, **yeni Azure ML işlem örneği oluştur** ' u seçin ve komut istemlerini izleyerek bir tane oluşturun.
1. Değişikliklerin etkili olabilmesi için Visual Studio Code yeniden yüklemeniz gerekir.
1. Bir Jupyter Notebook açın ve bir hücre çalıştırın.

> [!IMPORTANT]
> Bağlantıyı kurmak için bir hücre çalıştırmanız **gerekir** .

Bu noktada, Jupyter Not defterinizde hücreleri çalıştırmaya devam edebilirsiniz.

> [!TIP]
> Ayrıca, Jupyıter benzeri kod hücrelerini içeren Python betik dosyalarıyla (. Kopyala) çalışabilirsiniz. Daha fazla bilgi için bkz. [Python etkileşimli belgelerine Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support-py).

## <a name="configure-compute-instance-remote-development"></a>İşlem örneği uzaktan geliştirmeyi yapılandırma

Tam özellikli bir uzaktan geliştirme deneyimi için birkaç önkoşul olması gerekir:

* [Visual Studio Code uzak SSH uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).
* SSH özellikli işlem örneği. Daha fazla bilgi için [bkz. işlem örneği oluşturma kılavuzu](concept-compute-instance.md#create).

> [!NOTE]
> Windows platformlarında, zaten yoksa bir [OpenSSH uyumlu SSH istemcisi yüklemelisiniz](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client) . SSH komutunun yolda olması gerektiğinden, PuTTY Windows üzerinde desteklenmez.

### <a name="get-the-ip-and-ssh-port-for-your-compute-instance"></a>İşlem örneğiniz için IP ve SSH bağlantı noktasını alın

1. Konumundaki Azure Machine Learning Studio 'ya gidin https://ml.azure.com/ .
2. [Çalışma alanınızı](concept-workspace.md)seçin.
1. **Işlem örnekleri** sekmesine tıklayın.
1. **Uygulama URI 'si** sütununda, uzak işlem olarak kullanmak istediğiniz Işlem örneğinin **SSH** bağlantısına tıklayın. 
1. İletişim kutusunda, IP adresi ve SSH bağlantı noktasını göz önünde atın. 
1. Özel anahtarınızı yerel bilgisayarınızdaki ~/PST SSH/dizinine kaydedin; Örneğin, yeni bir dosya için bir düzenleyici açın ve anahtarı içine yapıştırın: 

   **Linux**:

   ```sh
   vi ~/.ssh/id_azmlcitest_rsa  
   ```

   **Windows**:

   ```cmd
   notepad C:\Users\<username>\.ssh\id_azmlcitest_rsa
   ```

   Özel anahtar şöyle görünür:

   ```text
   -----BEGIN RSA PRIVATE KEY-----

   MIIEpAIBAAKCAQEAr99EPm0P4CaTPT2KtBt+kpN3rmsNNE5dS0vmGWxIXq4vAWXD
   ..... 
   ewMtLnDgXWYJo0IyQ91ynOdxbFoVOuuGNdDoBykUZPQfeHDONy2Raw==

   -----END RSA PRIVATE KEY-----
   ```

1. Dosyayı yalnızca sizin okuyadığınızdan emin olmak için dosya izinlerini değiştirin.  

   ```sh
   chmod 600 ~/.ssh/id_azmlcitest_rsa
   ```

### <a name="add-instance-as-a-host"></a>Örneği konak olarak ekle

Dosyayı `~/.ssh/config` (Linux) veya `C:\Users<username>.ssh\config` (Windows) bir düzenleyicide açın ve aşağıdaki içeriğe benzer şekilde yeni bir giriş ekleyin:

```
Host azmlci1 

    HostName 13.69.56.51 

    Port 50000 

    User azureuser 

    IdentityFile ~/.ssh/id_azmlcitest_rsa
```

Burada, alanlarla ilgili bazı ayrıntılar verilmiştir:

|Alan|Açıklama|
|----|---------|
|Konak|İşlem örneği için istediğiniz toplu şeyi kullanın |
|HostName|Bu, işlem örneğinin IP adresidir |
|Bağlantı noktası|Bu, yukarıdaki SSH iletişim kutusunda gösterilen bağlantı noktasıdır |
|Kullanıcı|Bunun olması gerekir `azureuser` |
|Identityfile|Özel anahtarı kaydettiğiniz dosyaya işaret etmelidir |

Şimdi, yukarıda kullandığınız toplu işlemi kullanarak işlem örneğinizi SSH ile sağlayabilmelisiniz `ssh azmlci1` .

### <a name="connect-vs-code-to-the-instance"></a>Örneğe VS Code bağlama

1. SSH yapılandırmalarınızı göstermek için Visual Studio Code etkinlik çubuğundan uzak-SSH simgesine tıklayın.

1. Yeni oluşturduğunuz SSH ana bilgisayar yapılandırmasına sağ tıklayın.

1. **Geçerli pencerede konağa Bağlan '** ı seçin. 

Buradan, işlem örneği üzerinde tamamen çalıştısınız ve artık, yerel Visual Studio Code gibi düzenleyebilir, hata ayıklaması yapabilir, git kullanabilir, uzantıları kullanabilirsiniz vb.).

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code uzak olarak ayarladığınıza göre, [kodunuzda etkileşimli olarak hata ayıklamak](how-to-debug-visual-studio-code.md)için Visual Studio Code uzaktan işlem olarak bir işlem örneği kullanabilirsiniz.

[Öğretici: Ilk ml modelinize eğitme](tutorial-1st-experiment-sdk-train.md) bir işlem örneğinin tümleşik bir not defteriyle nasıl kullanılacağını gösterir.