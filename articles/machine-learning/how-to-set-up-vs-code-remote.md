---
title: 'Etkileşimli hata ayıklama: VS Code & ML işlem örnekleri'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile kodunuzda etkileşimli olarak hata ayıklamada uzak VS Code ayarlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.date: 12/09/2019
ms.openlocfilehash: 1999d29db21f820fbcdbca08f2258b657673be3e
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169747"
---
# <a name="debug-interactively-on-an-azure-machine-learning-compute-instance-with-vs-code-remote"></a>VS Code uzaktan Azure Machine Learning Işlem örneğinde etkileşimli olarak hata ayıklama

Bu makalede, Azure Machine Learning bir Işlem örneği üzerinde uzaktan Visual Studio Code ayarlamayı öğrenirsiniz, böylece VS Code **kodunuzda kodunuzun hatalarını etkileşimli olarak ayıklayabilirsiniz** . 

+ [Azure Machine Learning Işlem örneği](concept-compute-instance.md) , veri bilimcileri için tam olarak yönetilen bulut tabanlı bir iş istasyonudur ve BT yöneticileri için yönetim ve kurumsal hazırlık özellikleri sağlar. 


+ [Visual Studio Code uzak](https://code.visualstudio.com/docs/remote/remote-overview) Geliştirme, bir kapsayıcı, uzak makine veya Linux için Windows alt sistemi 'ni (WSL) tam özellikli bir geliştirme ortamı olarak kullanmanıza olanak tanır. 

## <a name="prerequisite"></a>Önkoşul  

Windows platformlarında, zaten yoksa bir [OpenSSH uyumlu SSH istemcisi yüklemelisiniz](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client) . 

> [!Note]
> SSH komutunun yolda olması gerektiğinden, PuTTY Windows üzerinde desteklenmez. 

## <a name="get-ip-and-ssh-port"></a>IP ve SSH bağlantı noktası al 

1. https://ml.azure.com/Azure Machine Learning Studio 'ya gidin.

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
   ```
   notepad C:\Users\<username>\.ssh\id_azmlcitest_rsa 
   ```

   Özel anahtar şöyle görünür:
   ```
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

## <a name="add-instance-as-a-host"></a>Örneği konak olarak ekle 

Dosya `~/.ssh/config` (Linux) veya `C:\Users<username>.ssh\config` (Windows) bir düzenleyicide açın ve şuna benzer yeni bir giriş ekleyin:

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
|Host|İşlem örneği için istediğiniz toplu şeyi kullanın |
|ana bilgisayar adı|Bu, işlem örneğinin IP adresidir |
|Bağlantı noktası|Bu, yukarıdaki SSH iletişim kutusunda gösterilen bağlantı noktasıdır |
|Kullanıcı|Bunun `azureuser` olması gerekir |
|Identityfile|Özel anahtarı kaydettiğiniz dosyaya işaret etmelidir |

Şimdi, `ssh azmlci1`yukarıda kullandığınız toplu işlemi kullanarak işlem örneğinizi SSH ile sağlayabilmelisiniz. 

## <a name="connect-vs-code-to-the-instance"></a>Örneğe VS Code bağlama 

1. [Visual Studio Code 'ı yükler](https://code.visualstudio.com/).

1. [Uzak SSH uzantısını yükler](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh). 

1. SSH yapılandırmalarınızı göstermek için sol taraftaki uzaktan SSH simgesine tıklayın.

1. Yeni oluşturduğunuz SSH ana bilgisayar yapılandırmasına sağ tıklayın.

1. **Geçerli pencerede konağa Bağlan '** ı seçin. 

Buradan, işlem örneği üzerinde tamamen çalıştısınız ve artık, yerel Visual Studio Code gibi düzenleyebilir, hata ayıklaması yapabilir, git kullanabilir, uzantıları kullanabilirsiniz vb.). 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code uzak olarak ayarladığınıza göre, kodunuzda etkileşimli olarak hata ayıklamak için Visual Studio Code uzaktan işlem olarak bir işlem örneği kullanabilirsiniz. 

[Öğretici: Ilk ml modelinize eğitme](tutorial-1st-experiment-sdk-train.md) bir işlem örneğinin tümleşik bir not defteriyle nasıl kullanılacağını gösterir.