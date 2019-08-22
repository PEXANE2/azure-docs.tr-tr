---
title: Hızlı başlangıç - Azure portalında Linux sanal makinesi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure portalını kullanarak Linux sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 8/20/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: feaefef23b433a296d25cc11b5cd89d86acd280f
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69650210"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portal bir Linux sanal makinesi oluşturun

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Azure portal, Azure kaynakları oluşturmak için tarayıcı tabanlı bir kullanıcı arabirimidir. Bu hızlı başlangıçta, Ubuntu 18,04 LTS çalıştıran bir Linux sanal makinesini (VM) dağıtmak için Azure portal nasıl kullanılacağı gösterilmektedir. Ayrıca VM'nizin çalıştığını görmek için SSH ile VM bağlantısı kurup NGINX web sunucusunu da yükleyeceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için bir Bash kabuğu açın ve [ssh-keygen](https://www.ssh.com/ssh/keygen/) komutunu kullanın. Yerel bilgisayarınızda Bash kabuğunuz yoksa [Azure Cloud Shell](https://shell.azure.com/bash)'i kullanabilirsiniz.


1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sayfanın üst kısmındaki menüde Cloud Shell açmak için `>_` simgeyi seçin.
1. CloudShell 'in, sol üst köşedeki **Bash** 'i söylediğinizden emin olun. PowerShell 'i görürseniz, **Bash** ' i seçmek ve bash kabuğu ' nu değiştirmek için **Onayla** ' yı seçin.
1. SSH `ssh-keygen -t rsa -b 2048` anahtarını oluşturmak için yazın. 
1. Anahtar çiftinin kaydedileceği bir dosya girmeniz istenir. Parantez içinde listelenen varsayılan konuma kaydetmek için **ENTER** tuşuna basın. 
1. Bir parola girmeniz istenir. SSH anahtarınız için bir parola yazabilir veya parola olmadan devam etmek için **ENTER** tuşuna basabilirsiniz.
1. Komut, içindevarsayılan`id_rsa` adı ile ortak ve özel anahtarlar oluşturur. `~/.ssh directory` `ssh-keygen` Komut, ortak anahtarın tam yolunu döndürür. `cat` Yazarak`cat ~/.ssh/id_rsa.pub`içeriğini göstermek için ortak anahtarın yolunu kullanın.
1. Bu komutun çıkışını kopyalayın ve bu makalede daha sonra kullanmak üzere bir yere kaydedin. Bu, ortak anahtarınıza ve sanal makinenizde oturum açmak üzere yönetici hesabınızı yapılandırırken ihtiyacınız olacaktır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Henüz yapmadıysanız [Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

1. **Popüler**bölümünde **Ubuntu Server 18,04 LTS**' yi seçin.

1. **Temel Bilgiler** sekmesinde, **Proje ayrıntıları** altında doğru aboneliğin seçildiğinden emin olun, ardından **Kaynak grubu** altından **Yeni oluştur**'u seçin. Kaynak grubunun adı için *Myresourcegroup* yazın ve ardından **Tamam**' ı seçin. 

    ![VM'niz için yeni bir kaynak grubu oluşturma](./media/quick-create-portal/project-details.png)

1. **Örnek ayrıntıları** altında, **Sanal makine adı** için *myVM* yazın ve **Bölge** için *East US*'yi seçin. Diğer varsayılan değerleri bırakın.

    ![Örnek ayrıntıları bölümü](./media/quick-create-portal/instance-details.png)

1. **Yönetici hesabı**altında **SSH ortak anahtarı**' nı seçin, Kullanıcı adınızı yazın ve sonra ortak anahtarınızı yapıştırın. Ortak anahtarınızda varsa baştaki ve sondaki tüm boşlukları kaldırın.

    ![Yönetici hesabı](./media/quick-create-portal/administrator-account.png)

1. **Gelen bağlantı noktası kuralları** > **Ortak gelen bağlantı noktaları** altından **Seçilen bağlantı noktalarına izin ver**'i, sonra aşağı açılan listeden **SSH (22)** ve **HTTP (80)** değerlerini seçin. 

    ![RDP ve HTTP için bağlantı noktaları açma](./media/quick-create-portal/inbound-port-rules.png)

1. Kalan varsayılan ayarları bırakın, ardından sayfanın alt kısmındaki **Gözden geçir + oluştur** düğmesini seçin.

1. **Sanal makine oluştur** sayfasında oluşturmak üzere olduğunuz VM'nin ayrıntılarını görüntüleyebilirsiniz. Hazır olduğunuzda **Oluştur**'u seçin.

VM'nizin dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

    
## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

VM ile bir SSH bağlantısı oluşturun.

1. VM’nizin genel bakış sayfasından **Bağlan** düğmesini seçin. 

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png)

2. **Sanal makineye bağlan** sayfasında, 22 numaralı bağlantı noktası üzerinden IP adresine göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın. **VM yerel hesabı kullanarak oturum açın** bölümünde bir bağlantı komutu gösterilir. Komutu kopyalamak için düğmeyi seçin. Aşağıdaki örnekte SSH bağlantı komutunun nasıl göründüğü gösterilmiştir:

    ```bash
    ssh azureuser@10.111.12.123
    ```

3. SSH anahtar çiftini oluşturmak için kullandığınız Bash kabuğunu kullanarak, bir SSH oturumu oluşturmak için yeniden seçerek `>_` veya ' ye https://shell.azure.com/bash) giderek Cloud Shell yeniden açabilir.

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için NGINX web sunucusunu yükleyin. SSH oturumunuzdan paket kaynaklarınızı güncelleştirip en son NGINX paketini yükleyin.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

İşlemi tamamladığınızda `exit` yazarak SSH oturumunu kapatın.


## <a name="view-the-web-server-in-action"></a>Web sunucusunu iş başında görün

İstediğiniz web tarayıcısını kullanarak varsayılan NGINX karşılama sayfasını görüntüleyin. Web adresi olarak VM 'nin genel IP adresini yazın. Genel IP adresini VM genel bakış sayfasında veya önceden kullandığınız SSH bağlantı dizesinde bulabilirsiniz.

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu ve **Sil**’i seçip silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine dağıttınız, bir Ağ Güvenlik Grubu ve kuralı oluşturdunuz ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
