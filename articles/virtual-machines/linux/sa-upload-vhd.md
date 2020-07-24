---
title: Azure CLı ile özel bir Linux diski yükleme
description: Kaynak Yöneticisi dağıtım modelini ve Azure CLı 'yi kullanarak Azure 'a bir sanal sabit disk (VHD) oluşturma ve yükleme
author: cynthn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.custom: storage accounts
ms.openlocfilehash: dcc7c69809ae623606bd091821c5f2fc661f6c8b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088759"
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli"></a>Azure CLı ile özel diskten bir Linux sanal makinesi yükleme ve oluşturma

Bu makalede, Azure CLı ile bir Azure depolama hesabına sanal sabit disk (VHD) yükleme ve bu özel diskten Linux VM oluşturma işlemlerinin nasıl yapılacağı gösterilir. Bu işlevsellik, gereksinimlerinize bir Linux yükleyip yapılandırmanıza ve ardından bu VHD 'yi kullanarak Azure sanal makinelerini (VM 'Ler) hızlı bir şekilde oluşturmanıza olanak tanır.

Bu konu, son VHD 'ler için depolama hesapları kullanır, ancak bu adımları [yönetilen diskleri](upload-vhd.md)kullanarak da yapabilirsiniz. 

## <a name="quick-commands"></a>Hızlı komutlar
Görevi hızlı bir şekilde gerçekleştirmeniz gerekirse, aşağıdaki bölümde bir VHD 'yi Azure 'a yüklemek için temel komutların ayrıntıları verilmiştir. Her adım için daha ayrıntılı bilgi ve bağlam, [buradan başlayarak](#requirements)belgenin geri kalanını tespit edebilir.

En son [Azure CLI](/cli/azure/install-az-cli2) 'nın yüklü olduğundan ve [az Login](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açtığınızdan emin olun.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları `myResourceGroup` ,, `mystorageaccount` ve içerir `mydisks` .

Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek `WestUs` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

[Az Storage Account Create](/cli/azure/storage/account)komutuyla sanal disklerinizi barındıracak bir depolama hesabı oluşturun. Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount` :

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Depolama hesabınızın erişim anahtarlarını [az Storage Account Keys List](/cli/azure/storage/account/keys)ile listeleyin. Şunlara dikkat edin `key1` :

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

[Az Storage Container Create](/cli/azure/storage/container)ile edindiğiniz depolama anahtarını kullanarak depolama hesabınızda bir kapsayıcı oluşturun. Aşağıdaki örnek, `mydisks` öğesinden depolama anahtarı değeri kullanılarak adlı bir kapsayıcı oluşturur `key1` :

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Son olarak, VHD 'nizi [az Storage blob upload](/cli/azure/storage/blob)ile oluşturduğunuz kapsayıcıya yükleyin. VHD 'niz için yerel yolu belirtin `/path/to/disk/mydisk.vhd` :

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

`--image` [Az VM Create](/cli/azure/vm)ile DISKINIZIN () URI 'sini belirtin. Aşağıdaki örnekte, `myVM` daha önce karşıya yüklenen sanal disk kullanılarak adlı BIR VM oluşturulur:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Hedef depolama hesabı, sanal diskinizi karşıya yüklediğiniz konum ile aynı olmalıdır. Ayrıca, sanal ağ, genel IP adresi, Kullanıcı adı ve SSH anahtarları gibi **az VM Create** komutu için gereken tüm ek parametreleri de belirtmeniz veya yanıt vermenizi da gerekir. [Kullanılabilir klasık clı Kaynak Yöneticisi parametreleri](../azure-cli-arm-commands.md#virtual-machines)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlayabilmeniz için şunlar gerekir:

* **Linux işletim sistemi bir. vhd dosyasına yüklenir** - [Azure tarafından onaylanan bir Linux dağıtımını](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya [onaylı olmayan dağıtımlar için bilgileri](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçimindeki bir sanal diske yükleme. VM ve VHD oluşturmak için birden çok araç mevcuttur:
  * Kemu veya [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) [KVM](https://www.linux-kvm.org/page/RunningKVM)'yi yükleyip yapılandırarak, görüntü biçimi olarak VHD 'yi kullanmaya özen gösterin. Gerekirse, kullanarak [bir görüntüyü dönüştürebilirsiniz](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) `qemu-img convert` .
  * Hyper-V ' d i [Windows 10](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) veya [Windows Server 2012/2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh846766(v=ws.11))üzerinde de kullanabilirsiniz.

> [!NOTE]
> Yeni VHDX biçimi Azure 'da desteklenmez. Bir VM oluşturduğunuzda, VHD 'yi biçim olarak belirtin. Gerekirse, [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya PowerShell cmdlet 'ini kullanarak VHDX DISKLERINI VHD 'ye dönüştürebilirsiniz [`Convert-VHD`](/powershell/module/hyper-v/convert-vhd?view=win10-ps) . Azure, Dinamik VHD 'leri karşıya yüklemeyi desteklemez, bu nedenle karşıya yüklemeden önce bu diskleri statik VHD 'lere dönüştürmeniz gerekir. Azure 'a yükleme işlemi sırasında dinamik diskleri dönüştürmek için [Azure VHD yardımcı programları](https://github.com/Microsoft/azure-vhd-utils-for-go) gibi araçları kullanabilirsiniz.
> 
> 

* Özel diskinizden oluşturulan VM 'Lerin, diskin kendisiyle aynı depolama hesabında bulunması gerekir
  * Hem özel diskinizi hem de oluşturulan VM 'Leri barındıracak bir depolama hesabı ve kapsayıcısı oluşturun
  * Tüm VM 'lerinizi oluşturduktan sonra, diskinizi güvenle silebilirsiniz

En son [Azure CLI](/cli/azure/install-az-cli2) 'nın yüklü olduğundan ve [az Login](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açtığınızdan emin olun.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları `myResourceGroup` ,, `mystorageaccount` ve içerir `mydisks` .

<a id="prepimage"> </a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Diski karşıya yüklenecek şekilde hazırlama
Azure çeşitli Linux dağıtımlarını destekler (bkz. [onaylı dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler, Azure 'da desteklenen çeşitli Linux dağıtımlarını hazırlama konusunda size rehberlik sağlar:

* **[CentOS Tabanlı Dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES ve openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer-onaylı olmayan dağıtımlar](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Ayrıca, Linux görüntülerini Azure için hazırlama hakkında daha genel ipuçları için bkz. **[Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes)** .

> [!NOTE]
> [Azure platformu SLA 'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/) yalnızca, [Azure tarafından onaylanan dağıtımlarda Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)'ta ' desteklenen sürümler ' altında belirtilen yapılandırma ayrıntıları ile belirlenen dağıtımlardan biri kullanıldığında, Linux çalıştıran VM 'ler için geçerlidir.
> 
> 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grupları, sanal ağ ve depolama gibi sanal makinelerinizi desteklemek için tüm Azure kaynaklarını mantıksal olarak bir araya getirir. Daha fazla bilgi kaynak grubu için bkz. [kaynak gruplarına genel bakış](../../azure-resource-manager/management/overview.md). Özel diskinizi karşıya yüklemeden ve VM 'Ler oluşturmadan önce, [az Group Create](/cli/azure/group)ile bir kaynak grubu oluşturmanız gerekir.

Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[Az Storage Account Create](/cli/azure/storage/account)komutuyla özel diskiniz ve VM 'niz için bir depolama hesabı oluşturun. Özel diskinizden oluşturduğunuz yönetilmeyen disklere sahip sanal makinelerin, bu diskle aynı depolama hesabında olması gerekir. 

Aşağıdaki örnek, `mystorageaccount` daha önce oluşturulan kaynak grubunda adlı bir depolama hesabı oluşturur:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure, her depolama hesabı için 2 512 bitlik erişim anahtarları oluşturur. Bu erişim anahtarları, depolama hesabında kimlik doğrulaması yapılırken (örneğin, yazma işlemlerini yürütmek için) kullanılır. Depolama hesabı erişim anahtarları hakkında daha fazla bilgi için bkz. [depolama hesabı erişim anahtarlarını yönetme](../../storage/common/storage-account-keys-manage.md). Erişim anahtarlarını [az Storage Account Keys List](/cli/azure/storage/account/keys)komutuyla görüntüleyebilirsiniz.

Oluşturduğunuz depolama hesabı için erişim anahtarlarını görüntüleyin:

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

`key1`Sonraki adımlarda depolama hesabınızla etkileşim kurmak için kullanacağınız gibi bir göz önüne alın.

## <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
Yerel dosya sisteminizi mantıksal olarak düzenlemek için farklı dizinler oluşturduğunuz gibi, disklerinizi düzenlemek için bir depolama hesabı içinde kapsayıcılar oluşturursunuz. Depolama hesabı, herhangi bir sayıda kapsayıcı içerebilir. [Az Storage Container Create](/cli/azure/storage/container)ile bir kapsayıcı oluşturun.

Aşağıdaki örnek adlı bir kapsayıcı oluşturur `mydisks` :

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>VHD 'YI karşıya yükle
Şimdi, [az Storage blob upload](/cli/azure/storage/blob)ile özel diskinizi karşıya yükleyin. Özel diskinizi bir Sayfa Blobu olarak karşıya yükleyip depoladığınızda.

Erişim anahtarınızı, önceki adımda oluşturduğunuz kapsayıcıyı ve ardından yerel bilgisayarınızdaki özel disk yolunu belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>Sanal makineyi oluşturma
Yönetilmeyen diskler içeren bir VM oluşturmak için `--image` [az VM Create](/cli/azure/vm)komutuyla diskinizin URI 'sini () belirtin. Aşağıdaki örnekte, `myVM` daha önce karşıya yüklenen sanal disk kullanılarak adlı BIR VM oluşturulur:

`--image`Özel diskinize işaret etmek için [az VM Create](/cli/azure/vm) parametresini belirtin. `--storage-account`Özel diskinizin depolandığı depolama hesabıyla eşleştiğinden emin olun. VM 'lerinizi depolamak için özel diskle aynı kapsayıcıyı kullanmanız gerekmez. Özel diskinizi karşıya yüklemeden önce, önceki adımlarla aynı şekilde ek kapsayıcılar oluşturduğunuzdan emin olun.

Aşağıdaki örnek, özel diskinizden adlı bir VM oluşturur `myVM` :

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Kullanıcı adı ve SSH anahtarları gibi **az VM Create** komutu için gerekli tüm ek parametreleri de belirtmeniz veya bu istemlere yanıt vermesi gerekir.


## <a name="resource-manager-template"></a>Resource Manager şablonu
Azure Resource Manager şablonlar, derlemek istediğiniz ortamı tanımlayan JavaScript Nesne Gösterimi (JSON) dosyalarıdır. Şablonlar, işlem veya ağ gibi farklı kaynak sağlayıcılarına bölünmüştür. Mevcut şablonları kullanabilir veya kendi kendinize yazabilirsiniz. [Kaynak Yöneticisi ve şablonları kullanma](../../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinin.

`Microsoft.Compute/virtualMachines`Şablonunuzun sağlayıcısı içinde, `storageProfile` sanal makinenizin yapılandırma ayrıntılarını içeren bir düğümünüz vardır. Düzenlenecek iki ana parametre, `image` `vhd` özel diskinize ve yeni VM 'nin sanal diskine işaret eden URI 'lardır. Aşağıda özel disk kullanımı için JSON örneği gösterilmektedir:

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

[Bu mevcut şablonu kullanarak özel görüntüden BIR VM oluşturabilir](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) veya [kendi Azure Resource Manager şablonlarınızı oluşturma](../../azure-resource-manager/templates/template-syntax.md)hakkında bilgi edinebilirsiniz. 

Bir şablonunuz yapılandırıldıktan sonra, VM 'lerinizi oluşturmak için [az Group Deployment Create](/cli/azure/group/deployment) kullanın. Parametresi ile JSON şablonunuzun URI 'sini belirtin `--template-uri` :

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Bilgisayarınızda yerel olarak depolanan bir JSON dosyanız varsa, `--template-file` bunun yerine parametresini kullanabilirsiniz:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Sonraki adımlar
Özel sanal diskinizi hazırladıktan ve karşıya yükledikten sonra, [Kaynak Yöneticisi ve şablonları kullanma](../../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinebilirsiniz. Ayrıca, yeni sanal makinelerinize [bir veri diski eklemek](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) isteyebilirsiniz. Sanal makinelerinize erişmeniz gereken uygulamalarınız varsa, [bağlantı noktalarını ve uç noktaları açmayı](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)unutmayın.
