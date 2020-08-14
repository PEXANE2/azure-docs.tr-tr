---
title: Öğretici-Azure 'da Linux sanal makinesinde lamba dağıtma
description: Bu öğreticide, Azure’daki bir Linux sanal makinesinde LAMP yığını yüklemeyi öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: cynthn
ms.openlocfilehash: d0d86e1a9c40eb6860508cf136ab9d466cc28ecd
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88225912"
---
# <a name="tutorial-install-a-lamp-web-server-on-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure’da bir Linux sanal makinesine bir LAMP web sunucusu yükleme

Bu makalede, Azure’daki bir Ubuntu sanal makinesine Apache web sunucusunun, MySQL ve PHP’nin (LAMP yığını) nasıl dağıtılacağı gösterilmektedir. LAMP sunucusunu çalışır halde görmek için, isteğe bağlı olarak bir WordPress sitesi yükleyip yapılandırabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ubuntu sanal makinesi oluşturma (LAMP yığınındaki 'L')
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * Apache, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmayı doğrulama
> * LAMP sunucusuna WordPress yükleme

Bu kurulum, hızlı testler veya kavram kanıtı içindir. Üretim ortamına yönelik öneriler de dahil olmak üzere, LAMP yığını hakkında daha fazla bilgi için [Ubuntu belgelerine](https://help.ubuntu.com/community/ApacheMySQLPHP) bakın.

Bu öğretici, en son sürüme sürekli olarak güncellenen [Azure Cloud Shell](../../cloud-shell/overview.md)içindeki CLI 'yi kullanır. Cloud Shell açmak için herhangi bir kod bloğunun en üstünden **deneyin** ' i seçin.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Apache, MySQL ve PHP yükleme

Ubuntu paket kaynaklarını güncelleştirmek ve Apache, MySQL ve PHP yüklemek için aşağıdaki komutu çalıştırın. `lamp-server^` paket adının parçası olan, komutun sonundaki şapka karakterine (^) dikkat edin. 


```bash
sudo apt update && sudo apt install lamp-server^
```

Paketleri ve diğer bağımlılıkları yüklemeniz istenir. Bu işlem, MySQL ile PHP kullanmak için gereken en düşük PHP uzantılarını yükler.  

## <a name="verify-installation-and-configuration"></a>Yükleme ve yapılandırmayı doğrulama


### <a name="verify-apache"></a>Apache 'yi doğrula

Aşağıdaki komutla Apache sürümünü denetleyin:
```bash
apache2 -v
```

Apache yüklüyken ve sanal makinenizde 80 numaralı bağlantı noktası açıkken, web sunucusuna İnternet üzerinden erişilebilir. Apache2 Ubuntu Varsayılan Sayfasını görüntülemek için bir web tarayıcısı açın ve sanal makinenin genel IP adresini girin. Sanal makineye SSH oturumu açmak için kullandığınız genel IP adresini kullanın:

![Apache varsayılan sayfası][3]


### <a name="verify-and-secure-mysql"></a>MySQL 'i doğrulama ve güvenli hale getirme

Aşağıdaki komutla MySQL sürümünü denetleyin (ana `V` parametresini not edin):

```bash
mysql -V
```

Kök parola ayarlama da dahil olmak üzere MySQL yüklemesinin güvenliğinin sağlanmasına yardımcı olmak için betiği çalıştırın `mysql_secure_installation` . 

```bash
sudo mysql_secure_installation
```

İsteğe bağlı olarak parolayı Doğrula eklentisini ayarlayabilirsiniz (önerilir). Ardından, MySQL kök kullanıcısı için bir parola ayarlayın ve ortamınız için kalan güvenlik ayarlarını yapılandırın. Tüm sorulara "Y" (Evet) yanıtını etmenizi öneririz.

MySQL özelliklerini (MySQL veritabanı oluşturma, kullanıcı ekleme veya yapılandırma ayarlarını değiştirme) denemek istiyorsanız MySQL’de oturum açın. Bu öğreticiyi tamamlamak için bu adım gerekli değildir.

```bash
sudo mysql -u root -p
```

İşiniz bittiğinde, `\q` yazarak mysql isteminden çıkın.

### <a name="verify-php"></a>PHP 'yi doğrula

Aşağıdaki komutla PHP sürümünü denetleyin:

```bash
php -v
```

Daha fazla test etmek istiyorsanız, tarayıcıda görüntülenecek bir hızlı PHP bilgi sayfası oluşturun. Aşağıdaki komut, PHP bilgi sayfasını oluşturur:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Artık oluşturduğunuz PHP bilgi sayfasını denetleyebilirsiniz. Bir tarayıcıyı açın ve `http://yourPublicIPAddress/info.php` adresine gidin. Sanal makinenizin genel IP adresini değiştirin. Şu görüntüye benzemelidir.

![PHP bilgi sayfası][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure’da bir LAMP sunucusu dağıttınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Ubuntu sanal makinesi oluşturma
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * Apache, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmayı doğrulama
> * LAMP sunucusuna WordPress yükleme

TLS/SSL sertifikalarıyla Web sunucularının güvenliğini sağlama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [TLS ile güvenli Web sunucusu](tutorial-secure-web-server.md)

[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png
