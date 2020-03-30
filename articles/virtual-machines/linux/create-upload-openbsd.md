---
title: OpenBSD resmi oluşturma ve yükleme
description: Azure CLI aracılığıyla bir Azure sanal makinesi oluşturmak için OpenBSD işletim sistemi içeren sanal bir sabit disk (VHD) oluşturma ve yükleme yi öğrenin
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 05/24/2017
ms.author: guybo
ms.openlocfilehash: 1ad1a66d67be7aefe4d9a7acae993e8788cbb193
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066753"
---
# <a name="create-and-upload-an-openbsd-disk-image-to-azure"></a>OpenBSD disk görüntüsü oluşturma ve Azure'a yükleme
Bu makalede, OpenBSD işletim sistemi içeren sanal bir sabit disk (VHD) oluşturmak ve yüklemek nasıl gösterir. Yükledikten sonra, Azure CLI aracılığıyla Azure'da sanal bir makine (VM) oluşturmak için kendi resminiz olarak kullanabilirsiniz.


## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşağıdaki öğelerin olduğunu varsayar:

* **Azure aboneliği** - Hesabınız yoksa, birkaç dakika içinde bir hesap oluşturabilirsiniz. MSDN aboneliğiniz varsa Visual [Studio aboneleri için Aylık Azure kredisine](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)bakın. Aksi takdirde, [nasıl ücretsiz bir deneme hesabı oluşturabilirsiniz](https://azure.microsoft.com/pricing/free-trial/)öğrenin.  
* **Azure CLI** - En son [Azure CLI'yi](/cli/azure/install-azure-cli) az giriş le Azure hesabınıza yüklediğinizden ve oturum [açtığınızdan](/cli/azure/reference-index)emin olun.
* **.vhd dosyasına yüklenen OpenBSD işletim sistemi** - desteklenen openbsd işletim sistemi ([6.6 sürüm AMD64)](https://ftp.openbsd.org/pub/OpenBSD/6.6/amd64/)sanal bir sabit diske takılmalıdır. .vhd dosyaları oluşturmak için birden çok araç vardır. Örneğin, .vhd dosyasını oluşturmak ve işletim sistemini yüklemek için Hyper-V gibi bir sanallaştırma çözümü kullanabilirsiniz. Hyper-V'nin nasıl yüklenir ve kullanılacağı yla ilgili talimatlar için [Hyper-V'yi yükle ve sanal bir makine oluşturma](https://technet.microsoft.com/library/hh846766.aspx)bilgisini görün.


## <a name="prepare-openbsd-image-for-azure"></a>Azure için OpenBSD görüntüsünü hazırlayın
Hyper-V desteğini ekleyen OpenBSD işletim sistemi 6.1'i kurduğunuz VM'de aşağıdaki yordamları tamamlayın:

1. Yükleme sırasında DHCP etkin değilse, hizmeti aşağıdaki gibi etkinleştirin:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Seri konsolu aşağıdaki gibi ayarlayın:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. Paket yüklemeyi aşağıdaki gibi yapılandırın:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Varsayılan olarak, `root` kullanıcı Azure'daki sanal makinelerde devre dışı bırakılır. Kullanıcılar OpenBSD VM'deki komutu `doas` kullanarak yüksek ayrıcalıklara sahip komutları çalıştırabilirler. Doas varsayılan olarak etkinleştirilir. Daha fazla bilgi için [doas.conf](https://man.openbsd.org/doas.conf.5)' a bakın. 

5. Azure Aracısı için ön koşulları aşağıdaki gibi yükleyin ve yapılandırır:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. Azure aracısının en son sürümü her zaman [GitHub'da](https://github.com/Azure/WALinuxAgent/releases)bulunabilir. Aracıyı aşağıdaki gibi yükleyin:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Azure Aracısı'nı yükledikten sonra aşağıdaki gibi çalıştığını doğrulamak iyi bir fikirdir:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Sistemi temizlemek ve yeniden sağlama için uygun hale getirmek için deprovision. Aşağıdaki komut, son sağlanan kullanıcı hesabını ve ilişkili verileri de siler:

    ```sh
    waagent -deprovision+user -force
    ```

Artık VM'ni kapatabilirsin.


## <a name="prepare-the-vhd"></a>VHD'yi hazırlayın
VHDX biçimi Azure'da desteklenmez, yalnızca **Sabit VHD'de**dir. Hyper-V Manager veya Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet'i kullanarak diski sabit VHD biçimine dönüştürebilirsiniz. Bir örnek aşağıdaki gibidir.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Depolama kaynakları oluşturma ve yükleme
Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

VHD'nizi yüklemek [için, az depolama hesabı oluşturarak](/cli/azure/storage/account)bir depolama hesabı oluşturun. Depolama hesabı adları benzersiz olmalıdır, bu nedenle kendi adınızı sağlayın. Aşağıdaki örnek, *mystorageaccount*adlı bir depolama hesabı oluşturur:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

Depolama hesabına erişimi denetlemek için, [az depolama hesap anahtarları listesi](/cli/azure/storage/account/keys) aşağıdaki gibi depolama anahtarı nı edinin:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

Yüklediğiniz VHD'leri mantıksal olarak ayırmak için, az [depolama kapsayıcısı oluşturduğu](/cli/azure/storage/container)depolama hesabı içinde bir kapsayıcı oluşturun:

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Son olarak, aşağıdaki gibi [az depolama blob yükleme](/cli/azure/storage/blob) ile VHD yükleyin:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>VHD'nizden VM oluşturun
Bir [örnek komut dosyası](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) veya doğrudan az vm oluşturmak ile bir VM [oluşturabilirsiniz.](/cli/azure/vm) Yüklediğiniz OpenBSD VHD'yi belirtmek `--image` için parametreyi aşağıdaki gibi kullanın:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

[Az vm list-ip adresleri](/cli/azure/vm) ile OpenBSD VM ip adresini aşağıdaki gibi edinin:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

Artık OpenBSD VM'inize normal olarak SSH yapabilirsiniz:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Sonraki adımlar
OpenBSD6.1'de Hyper-V desteği hakkında daha fazla bilgi almak istiyorsanız [OpenBSD 6.1](https://www.openbsd.org/61.html) ve [hyperv.4'ü](https://man.openbsd.org/hyperv.4)okuyun.

Yönetilen diskten bir VM oluşturmak istiyorsanız, [az diski](/cli/azure/disk)okuyun. 
