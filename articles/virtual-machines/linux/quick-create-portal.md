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
ms.date: 8/16/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 189d6d0030264590986d6fe2af47d35705cfb08b
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575823"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portal bir Linux sanal makinesi oluşturun

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Azure portal, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta Azure portalı kullanarak Ubuntu 16.04 LTS çalıştıran bir Linux sanal makinesinin (VM) nasıl dağıtılacağı gösterilir. Ayrıca VM'nizin çalıştığını görmek için SSH ile VM bağlantısı kurup NGINX web sunucusunu da yükleyeceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için bir Bash kabuğu açın ve [ssh-keygen](https://www.ssh.com/ssh/keygen/) komutunu kullanın. Bunu yapmak için örnek bir komut aşağıda verilmiştir. Yerel bilgisayarınızda Bash kabuğunuz yoksa [Azure Cloud Shell](https://shell.azure.com/bash)'i kullanabilirsiniz.

```bash
ssh-keygen -t rsa -b 2048
```

Anahtar çiftinin kaydedileceği bir dosya girmeniz istenir. Belirli bir dosya konumu girebilir veya parantez içinde belirtilen varsayılan konuma kaydetmek için yalnızca ' Enter ' tuşuna basın. Bundan sonra bir parola girmeniz istenir. SSH anahtarınız için bir parola girebilir veya parola olmadan devam etmek için ' Enter ' tuşuna basabilirsiniz.

```bash
[root@linuxvm ~]$ ssh-keygen -t rsa -b 2048
Enter the file in which to save the key (home/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your public key has been saved in /home/root/.ssh/id_rsa.pub.
The key fingerprint is: SHA256:kkQS13gbaxevy4ULH0mW6wLIkcFm0twx/rIlSo1fIqU
The key's randomart image is:
+---[RSA 2018]----+
|   +oo=+         |
|  . B=o.+ .      |
|   + o+. + +     |
|    o* o+ = .    |
|   .EoB.S+ =     |
|   .o+.O. * .    |
|    . o. = =     |
|        . *      |
|         .       |
+----[SHA256]-----+
```
Komut, içindevarsayılan`id_rsa` adı ile ortak ve özel anahtarlar oluşturur. `~/.ssh directory` `ssh-keygen` Komut, ortak anahtarın tam yolunu döndürür. `cat` komutuyla ortak anahtarın yolunu kullanarak içeriğini görüntüleyebilirsiniz.

```bash
cat ~/.ssh/id_rsa.pub
```

>[!NOTE]
> SSH anahtarınızı varsayılandan farklı bir konumda kaydetmeyi seçerseniz, çalıştırdığınızda bu konumu kullanmanız gerekir`cat`

Bu komutun çıktısını kaydedin. Bu, ortak anahtarınıza ve sanal makinenizde oturum açmak üzere yönetici hesabınızı yapılandırırken ihtiyacınız olacaktır.

SSH-keygen komutu hakkında daha fazla bilgi için [man sayfasını](https://linux.die.net/man/1/ssh-keygen)ziyaret edin.

Bir Windows bilgisayarı kullanıyorsanız ve PuTTy kullanımı dahil olmak üzere SSH anahtar çiftleri oluşturma hakkında daha fazla bilgi için, bkz. [Windows Ile SSH anahtarlarını kullanma](ssh-from-windows.md).

SSH anahtar çiftinizi Cloud Shell'i kullanarak oluşturduğunuzda [Cloud Shell tarafından otomatik olarak takılan](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage) bir Azure Dosya Paylaşımında depolanır. Anahtarlarınızı alana kadar bu dosya paylaşımını veya depolama hesabını silmeyin. Aksi takdirde VM erişimini kaybedersiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

1. Azure Marketi kaynakları listesinin üzerindeki arama kutusunda **Ubuntu Server 18,04** ' i arayın ve ubuntu 18,04 LTS teklifini seçip **Oluştur**' u seçin.

1. **Temel Bilgiler** sekmesinde, **Proje ayrıntıları** altında doğru aboneliğin seçildiğinden emin olun, ardından **Kaynak grubu** altından **Yeni oluştur**'u seçin. Açılan pencerede kaynak grubunun adı için *myResourceGroup* yazın ve **Tamam**'ı seçin.

    ![VM'niz için yeni bir kaynak grubu oluşturma](./media/quick-create-portal/project-details.png)

1. **Örnek ayrıntıları** altında, **Sanal makine adı** için *myVM* yazın ve **Bölge** için *East US*'yi seçin. Diğer varsayılan değerleri bırakın.

    ![Örnek ayrıntıları bölümü](./media/quick-create-portal/instance-details.png)

1. **Yönetici hesabı**altında **SSH ortak anahtarı**' nı seçin, Kullanıcı adınızı yazın ve daha önce kaydettiğiniz ortak anahtarınızı metin kutusuna yapıştırın. Ortak anahtarınızda varsa baştaki ve sondaki tüm boşlukları kaldırın.

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

2. **Sanal makineye bağlan** sayfasında, 22 numaralı bağlantı noktası üzerinden IP adresine göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın. **VM yerel hesabı kullanarak oturum açın** bölümünde bir bağlantı komutu gösterilir. Düğmeye tıklayarak komutu kopyalayın. Aşağıdaki örnekte SSH bağlantı komutunun nasıl göründüğü gösterilmiştir:

    ```bash
    ssh azureuser@10.111.12.123
    ```

3. SSH anahtar çiftiniz oluşturmak için kullandığınız Bash kabuğunu kullanarak ( [Azure Cloud Shell](https://shell.azure.com/bash) veya yerel bash kabuğu gibi), SSH bağlantısı komutunu bir SSH oturumu oluşturmak için kabuğa yapıştırın.

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için NGINX web sunucusunu yükleyin. SSH oturumunuzdan paket kaynaklarınızı güncelleştirip en son NGINX paketini yükleyin.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

İşlemi tamamladığınızda `exit` yazarak SSH oturumunu kapatın.


## <a name="view-the-web-server-in-action"></a>Web sunucusunu iş başında görün

İstediğiniz web tarayıcısını kullanarak varsayılan NGINX karşılama sayfasını görüntüleyin. VM'nin genel IP adresini web adresi olarak girin. Genel IP adresini VM genel bakış sayfasında veya önceden kullandığınız SSH bağlantı dizesinde bulabilirsiniz.

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu ve **Sil**’i seçip silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine dağıttınız, bir Ağ Güvenlik Grubu ve kuralı oluşturdunuz ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
