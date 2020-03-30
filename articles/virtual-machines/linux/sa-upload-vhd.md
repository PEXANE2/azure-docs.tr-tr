---
title: Azure CLI ile özel bir Linux diski yükleme
description: Kaynak Yöneticisi dağıtım modelini ve Azure CLI'yi kullanarak Azure'a sanal bir sabit disk (VHD) oluşturun ve yükleyin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: bc90a409dd2695ce16f8c7d5909f8e2d7867673c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060252"
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli"></a>Azure CLI ile özel diskten bir Linux VM yükleme ve oluşturma

Bu makalede, Azure CLI ile bir Azure depolama hesabına sanal sabit disk (VHD) nasıl yükleyebilirsiniz ve bu özel diskten Linux VM'leri nasıl oluşturabilirsiniz. Bu işlevsellik, bir Linux dağıtımını gereksinimlerinize göre yüklemenize ve yapılandırmanıza ve ardından bu VHD'yi hızlı bir şekilde Azure sanal makineleri (VM) oluşturmak için kullanmanıza olanak tanır.

Bu konu, son VHD'ler için depolama hesapları kullanır, ancak [yönetilen diskleri](upload-vhd.md)kullanarak da bu adımları yapabilirsiniz. 

## <a name="quick-commands"></a>Hızlı komutlar
Görevi hızlı bir şekilde gerçekleştirmeniz gerekiyorsa, aşağıdaki bölüm, Azure'a bir VHD yüklemek için temel komutları ayrıntılarıyla anlatır. Her adım için daha ayrıntılı bilgi ve bağlam [buradabaşlayan](#requirements)belgenin geri kalanını bulabilirsiniz.

En son [Azure CLI'sini](/cli/azure/install-az-cli2) az giriş kullanarak bir Azure hesabına yüklediğinizden ve oturum [açtığından](/cli/azure/reference-index)emin olun.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları `myResourceGroup` `mystorageaccount`dahil `mydisks`, , ve .

Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek `WestUs` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

Sanal disklerinizi az depolama hesabı [oluşturarak](/cli/azure/storage/account)tutmak için bir depolama hesabı oluşturun. Aşağıdaki örnek, adlı `mystorageaccount`bir depolama hesabı oluşturur:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Depolama hesabınıza ait erişim anahtarlarını [az depolama hesabı anahtarları listesiyle](/cli/azure/storage/account/keys)listele. Bir not `key1`ala:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

[Az depolama kapsayıcısı oluşturarak](/cli/azure/storage/container)elde ettiğiniz depolama anahtarını kullanarak depolama hesabınızda bir kapsayıcı oluşturun. Aşağıdaki örnekte depolama anahtar `mydisks` değeri kullanılarak adlı `key1`bir kapsayıcı oluşturur:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Son olarak, [az depolama blob yükleme](/cli/azure/storage/blob)ile oluşturduğunuz kapsayıcıya VHD yükleyin. Altında VHD için yerel `/path/to/disk/mydisk.vhd`yol belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

AZ vm oluştur`--image`ile diskinize URI ( ) [belirtin.](/cli/azure/vm) Aşağıdaki örnekte, daha önce `myVM` yüklenen sanal disk kullanılarak adlandırılan bir VM oluşturulur:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Hedef depolama hesabı, sanal diskinizi yüklediğiniz yerle aynı olmalıdır. Sanal ağ, genel IP adresi, kullanıcı adı ve SSH anahtarları gibi **az vm oluşturma** komutu tarafından gerekli tüm ek parametreleri belirtmeniz veya yanıtlamanız gerekir. [Mevcut klasik CLI Kaynak Yöneticisi parametreleri](../azure-cli-arm-commands.md#virtual-machines)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için şunları yapmanız gerekir:

* **.vhd dosyasına yüklenen Linux işletim sistemi** - Azure onaylı bir Linux [dağıtımını](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yükleyin (veya [onaylanmamış dağıtımlar için bilgilere](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın) VHD biçimindeki bir sanal diske. VM ve VHD oluşturmak için birden çok araç vardır:
  * [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM'yi](https://www.linux-kvm.org/page/RunningKVM)yükleyin ve yapılandırın, görüntü biçiminiz olarak VHD'yi kullanmaya özen. Gerekirse, bir [görüntüyü](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak `qemu-img convert`dönüştürebilirsiniz.
  * Hyper-V'yi [Windows 10'da](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2'de](https://technet.microsoft.com/library/hh846766.aspx)de kullanabilirsiniz.

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmez. Bir VM oluşturduğunuzda, biçim olarak VHD belirtin. Gerekirse, VHDX diskleri VHD diskleri [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet kullanarak VHDX'e dönüştürebilirsiniz. Ayrıca, Azure dinamik VHD'ler yüklemeyi desteklemez, bu nedenle yüklemeden önce bu tür diskleri statik VHD'lere dönüştürmeniz gerekir. Azure'a yükleme işlemi sırasında dinamik diskleri dönüştürmek [için GO için Azure VHD Yardımcı Programları](https://github.com/Microsoft/azure-vhd-utils-for-go) gibi araçları kullanabilirsiniz.
> 
> 

* Özel diskinizden oluşturulan VM'ler, diskin kendisiyle aynı depolama hesabında olmalıdır
  * Hem özel diskinizi hem de oluşturulan VM'leri tutmak için bir depolama hesabı ve kapsayıcı oluşturun
  * Tüm VM'lerinizi oluşturduktan sonra, diskinizi güvenle silebilirsiniz

En son [Azure CLI'sini](/cli/azure/install-az-cli2) az giriş kullanarak bir Azure hesabına yüklediğinizden ve oturum [açtığından](/cli/azure/reference-index)emin olun.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları `myResourceGroup` `mystorageaccount`dahil `mydisks`, , ve .

<a id="prepimage"> </a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Diskin yüklenmesini hazırlama
Azure çeşitli Linux dağıtımlarını destekler [(bkz.](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Aşağıdaki makaleler, Azure'da desteklenen çeşitli Linux dağıtımlarını nasıl hazırlayacağınızı size yönlendiriyor:

* **[CentOS Tabanlı Dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Kırmızı Şapka Kurumsal Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer - Onaylanmayan Dağıtımlar](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Ayrıca, Azure için Linux görüntüleri hazırlamayla ilgili daha genel ipuçları için **[Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes)** bakın.

> [!NOTE]
> [Azure platformu SLA,](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux'u çalıştıran VM'ler için yalnızca onaylanan dağıtımlardan biri Azure Onaylı Dağıtımlarda Linux'ta 'Desteklenen [Sürümler'](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)altında belirtildiği şekilde yapılandırma ayrıntılarıyla kullanıldığında geçerlidir.
> 
> 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grupları, sanal ağ ve depolama gibi sanal makinelerinizi desteklemek için tüm Azure kaynaklarını mantıksal olarak bir araya getirir. Daha fazla bilgi kaynak grupları için [kaynak gruplarına genel bakış](../../azure-resource-manager/management/overview.md)alabakın. Özel diskinizi yüklemeden ve VM'ler oluşturmadan [önce, önce az grubu oluşturarak](/cli/azure/group)bir kaynak grubu oluşturmanız gerekir.

Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Özel diskiniz için bir depolama hesabı oluşturun ve [az depolama hesabı oluşturarak](/cli/azure/storage/account)VM'ler oluşturun. Özel diskinizden oluşturduğunuz yönetilmeyen disklere sahip tüm VM'lerin bu diskle aynı depolama hesabında olması gerekir. 

Aşağıdaki örnek, daha önce `mystorageaccount` oluşturulmuş kaynak grubunda adı geçen bir depolama hesabı oluşturur:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını listele
Azure, her depolama hesabı için iki adet 512 bit erişim anahtarı oluşturur. Bu erişim anahtarları, yazma işlemleri gerçekleştirmek gibi depolama hesabına kimlik doğrulaması yaparken kullanılır. Depolama hesabı erişim anahtarları hakkında daha fazla bilgi için [bkz.](../../storage/common/storage-account-keys-manage.md) Az depolama hesap [anahtarları listesi](/cli/azure/storage/account/keys)ile erişim anahtarlarını görüntüleyebilirsiniz.

Oluşturduğunuz depolama hesabının erişim anahtarlarını görüntüleyin:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Çıktı şuna benzer olacaktır:

```output
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```

Sonraki adımlarda `key1` depolama hesabınızla etkileşimde bulunmak için kullanacağınızı not alın.

## <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
Yerel dosya sisteminizi mantıksal olarak düzenlemek için farklı dizinler oluşturduğunuz gibi, disklerinizi düzenlemek için bir depolama hesabı içinde kapsayıcılar oluşturursunuz. Depolama hesabı herhangi bir sayıda kapsayıcı içerebilir. az depolama kapsayıcı oluşturmak ile bir kapsayıcı [oluşturun.](/cli/azure/storage/container)

Aşağıdaki örnek, adlı `mydisks`bir kapsayıcı oluşturur:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>VHD Yükle
Şimdi az depolama [blob upload](/cli/azure/storage/blob)ile özel disk yükleyin. Özel diskinizi bir sayfa blob'u olarak yükler ve saklarsınız.

Erişim anahtarınızı, önceki adımda oluşturduğunuz kapsayıcıyı ve ardından yerel bilgisayarınızdaki özel diske giden yolu belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>Sanal makine oluşturma
Yönetilmeyen disklere sahip bir VM oluşturmak için diskinize URI ()`--image`az vm oluştur ile [belirtin.](/cli/azure/vm) Aşağıdaki örnekte, daha önce `myVM` yüklenen sanal disk kullanılarak adlandırılan bir VM oluşturulur:

Özel diskinizi `--image` işaret etmek için [az vm oluşturarak](/cli/azure/vm) parametreyi belirtirsiniz. Özel `--storage-account` diskinizin depolandığı depolama hesabıyla eşleştiğinden emin olun. VM'lerinizi depolamak için özel diskle aynı kapsayıcıyı kullanmanız gerekmez. Özel diskinizi yüklemeden önce önceki adımlarla aynı şekilde ek kapsayıcılar oluşturduğunuzdan emin olun.

Aşağıdaki örnek, özel diskinizden bir VM oluşturur: `myVM`

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Yine de az **vm oluşturma** komutu kullanıcı adı ve SSH tuşları gibi gerekli tüm ek parametreleri belirtmek veya istemleri yanıtlamak gerekir.


## <a name="resource-manager-template"></a>Resource Manager şablonu
Azure Kaynak Yöneticisi şablonları, oluşturmak istediğiniz ortamı tanımlayan JavaScript Nesne Gösterimi (JSON) dosyalarıdır. Şablonlar, bilgi işlem veya ağ gibi farklı kaynak sağlayıcılarına ayrılmıştır. Varolan şablonları kullanabilir veya kendi şablonlarınızı yazabilirsiniz. [Kaynak Yöneticisi ve şablonları kullanma](../../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinin.

Şablonunuzun `Microsoft.Compute/virtualMachines` sağlayıcısıiçinde, VM'nizin yapılandırma ayrıntılarını içeren bir `storageProfile` düğüme sahipsiniz. İki ana parametre, özel `image` `vhd` diskinizi ve yeni VM'nizin sanal diskini gösteren ve URI'leri dir. Aşağıdaki özel bir disk kullanmak için JSON bir örnek gösterir:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

[Bu varolan şablonu özel bir resimden Bir VM oluşturmak](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) veya [kendi Azure Kaynak Yöneticisi şablonlarınızı oluşturma](../../azure-resource-manager/templates/template-syntax.md)hakkında bilgi almak için kullanabilirsiniz. 

Bir şablon yapılandırıldıktan sonra, VM'lerinizi oluşturmak için [az grup dağıtımı oluşturmayı](/cli/azure/group/deployment) kullanın. JSON şablonunuzun URI'sini `--template-uri` parametreyle belirtin:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Bilgisayarınızda yerel olarak depolanmış bir JSON dosyanız varsa, `--template-file` bunun yerine parametreyi kullanabilirsiniz:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Sonraki adımlar
Özel sanal diskinizi hazırlayıp yükledikten [sonra, Kaynak Yöneticisi ve şablonları kullanma](../../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinebilirsiniz. Ayrıca, yeni Sanal M'lerinize [bir veri diski eklemek](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) isteyebilirsiniz. VM'lerinizde erişmeniz gereken uygulamalar çalışıyorsa, [bağlantı noktalarını ve bitiş noktalarını açtığınızdan](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)emin olun.

