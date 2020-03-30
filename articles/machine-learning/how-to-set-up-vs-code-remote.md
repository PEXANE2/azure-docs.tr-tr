---
title: 'Etkileşimli hata ayıklama: VS Kodu & ML bilgi işlem örnekleri'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile kodunuzu etkileşimli olarak hata ayıklamak için VS Code Remote'u ayarlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.date: 12/09/2019
ms.openlocfilehash: 1999d29db21f820fbcdbca08f2258b657673be3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77169747"
---
# <a name="debug-interactively-on-an-azure-machine-learning-compute-instance-with-vs-code-remote"></a>VS Code Remote ile Azure Machine Learning Compute Instance'da etkileşimli hata ayıklama

Bu makalede, Visual Studio Code Remote'u bir Azure Machine Learning Compute Instance'da nasıl ayarlayacağımı öğreneceksiniz, böylece kodunuzu VS Kodu'ndan **etkileşimle çıkarabilirsiniz.** 

+ [Azure Machine Learning Compute Instance,](concept-compute-instance.md) veri bilimciler için tam olarak yönetilen bulut tabanlı bir iş istasyonudur ve BT yöneticileri için yönetim ve kurumsal hazırlık özellikleri sağlar. 


+ [Görsel Stüdyo Kodu Uzaktan Kumanda](https://code.visualstudio.com/docs/remote/remote-overview) Geliştirme, tam özellikli bir geliştirme ortamı olarak bir kapsayıcı, uzak makine veya Linux için Windows Alt Sistemi (WSL) kullanmanıza olanak tanır. 

## <a name="prerequisite"></a>Önkoşul  

Windows platformlarında, [openssh uyumlu bir SSH istemcisi yüklemeniz](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client) gerekir. 

> [!Note]
> Ssh komutu yolda olması gerektiğinden, PuTTY Windows'da desteklenmez. 

## <a name="get-ip-and-ssh-port"></a>IP ve SSH bağlantı noktasını alın 

1. Adresindeki https://ml.azure.com/Azure Machine Learning stüdyosuna gidin.

2. Çalışma [alanınızı](concept-workspace.md)seçin.
1. Örnekleri **İşlem sekmesini** tıklatın.
1. Uygulama **URI** sütununda, uzak bilgi işlem olarak kullanmak istediğiniz bilgi işlem örneğinin **SSH** bağlantısını tıklatın. 
1. İletişim kutusunda IP Adresi ve SSH bağlantı noktasını not alın. 
1. Özel anahtarınızı yerel bilgisayarınızdaki ~/.ssh/ dizine kaydedin; örneğin, yeni bir dosya için bir düzenleyici açın ve anahtarı yapıştırın: 

   **Linux**: 
   ```sh
   vi ~/.ssh/id_azmlcitest_rsa  
   ```

   **Windows**: 
   ```
   notepad C:\Users\<username>\.ssh\id_azmlcitest_rsa 
   ```

   Özel anahtar biraz şuna benzer:
   ```
   -----BEGIN RSA PRIVATE KEY----- 

   MIIEpAIBAAKCAQEAr99EPm0P4CaTPT2KtBt+kpN3rmsNNE5dS0vmGWxIXq4vAWXD 
   ..... 
   ewMtLnDgXWYJo0IyQ91ynOdxbFoVOuuGNdDoBykUZPQfeHDONy2Raw== 

   -----END RSA PRIVATE KEY----- 
   ```

1. Dosyayı yalnızca sizin okuyabildiğinizden emin olmak için dosyadaki izinleri değiştirin.  
   ```sh
   chmod 600 ~/.ssh/id_azmlcitest_rsa   
   ```

## <a name="add-instance-as-a-host"></a>Örnek ana bilgisayar olarak ekleme 

Dosyayı `~/.ssh/config` (Linux) `C:\Users<username>.ssh\config` veya (Windows) bir düzenleyicide açın ve buna benzer yeni bir giriş ekleyin:

```
Host azmlci1 

    HostName 13.69.56.51 

    Port 50000 

    User azureuser 

    IdentityFile ~/.ssh/id_azmlcitest_rsa   
```

Burada alanlar hakkında bazı ayrıntılar: 

|Alan|Açıklama|
|----|---------|
|Host|İşlem örneği için istediğiniz stenoyu kullanın |
|Ana bilgisayar adı|Bu, işlem örneğinin IP adresidir |
|Bağlantı noktası|Bu, yukarıdaki SSH iletişim kutusunda gösterilen bağlantı noktasıdır |
|Kullanıcı|Bu olması gerekir `azureuser` |
|IdentityFile|Özel anahtarı kaydettiğiniz dosyayı işaret etmeli |

Şimdi, yukarıda kullandığınız stenosu kullanarak işlem örneğinize ssh yapabilmeniz `ssh azmlci1`gerekir. 

## <a name="connect-vs-code-to-the-instance"></a>VS Kodunu örne bağla 

1. [Visual Studio Kodunu Yükleyin.](https://code.visualstudio.com/)

1. [Uzaktan SSH Uzantısı'nı yükleyin.](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) 

1. SSH yapılandırmalarınızı göstermek için soldaki Uzak SSH simgesine tıklayın.

1. Az önce oluşturduğunuz SSH ana bilgisayar yapılandırmasını sağ tıklatın.

1. **Geçerli Pencerede Ana Bilgisayara Bağlan'ı**seçin. 

Şu andan itibaren, tamamen bilgi işlem örneği üzerinde çalışıyorsunuz ve artık yerel Visual Studio Code'unuzda olduğu gibi, işlem ayıklama, git, kullanım uzantıları, vb. olarak edinebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code Remote'u ayarladığınızda, kodunuzu devre-dilayıklamak için Visual Studio Code'dan uzaktan bilgi işlem olarak bir işlem örneği kullanabilirsiniz. 

[Öğretici: Tren ilk ML modeli](tutorial-1st-experiment-sdk-train.md) entegre bir dizüstü bilgisayar ile bir işlem örneği nasıl kullanılacağını gösterir.