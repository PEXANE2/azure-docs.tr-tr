---
title: Packer ile Linux Azure VM Görüntüleri Oluşturun
description: Azure'da Linux sanal makinelerinin görüntülerini oluşturmak için Packer'ı nasıl kullanacağınızı öğrenin
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure
ms.date: 05/07/2019
ms.author: cynthn
ms.openlocfilehash: 3aec50b8c8f2033b7340bde15ea7670c1a0b6bb9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534228"
---
# <a name="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure"></a>Azure'da Linux sanal makine görüntüleri oluşturmak için Packer nasıl kullanılır?
Azure'daki her sanal makine (VM), Linux dağıtımını ve işletim sistemi sürümünü tanımlayan bir görüntüden oluşturulur. Görüntüler önceden yüklenmiş uygulamaları ve yapılandırmaları içerebilir. Azure Marketi, çoğu yaygın dağıtım ve uygulama ortamı için birçok birinci ve üçüncü taraf görüntü sağlar veya gereksinimlerinize göre uyarlanmış kendi özel resimlerinizi oluşturabilirsiniz. Bu makalede, Azure'da özel görüntüler tanımlamak ve oluşturmak için açık kaynak aracı [Packer'ın](https://www.packer.io/) nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.

> [!NOTE]
> Azure artık kendi özel resimlerinizi tanımlamak ve oluşturmak için Azure Image Builder (önizleme) hizmetine sahiptir. Azure Image Builder Packer üzerine kuruludur, böylece mevcut Packer kabuk provizyoner komut dosyalarınızı da onunla birlikte kullanabilirsiniz. Azure Image Builder'a başlamak için [bkz.](image-builder.md)


## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturma
Oluşturma işlemi sırasında Packer, kaynak VM'yi oluştururken geçici Azure kaynakları oluşturur. Bu kaynak VM'yi görüntü olarak kullanmak üzere yakalamak için bir kaynak grubu tanımlamanız gerekir. Packer yapı işleminden elde edilen çıktı bu kaynak grubunda depolanır.

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma
Packer, bir hizmet ilkesi ni kullanarak Azure ile kimlik doğrulaması verir. Azure hizmet prensibi, Packer gibi uygulamalar, hizmetler ve otomasyon araçlarıyla kullanabileceğiniz bir güvenlik kimliğidir. Hizmet sorumlusunun Azure'da hangi işlemleri gerçekleştirebileceğine göre izinleri denetler ve tanımlarsınız.

[Az reklam sp create-for-rbac](/cli/azure/ad/sp) ile bir hizmet müdürü oluşturun ve Packer'ın ihtiyaç duyduğu kimlik bilgilerini çıktı:

```azurecli
az ad sp create-for-rbac --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
```

Önceki komutlardan çıktı örneği aşağıdaki gibidir:

```output
{
    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Azure'da kimlik doğrulaması yapmak için Azure abonelik kimliğinizi [az hesap gösterimle](/cli/azure/account)almanız da gerekir:

```azurecli
az account show --query "{ subscription_id: id }"
```

Bir sonraki adımda bu iki komutun çıktısını kullanırsınız.


## <a name="define-packer-template"></a>Packer şablonu tanımla
Resim oluşturmak için JSON dosyası olarak bir şablon oluşturursunuz. Şablonda, gerçek yapı işlemini gerçekleştiren oluşturucuları ve geçici olarak tanımlarsınız. Packer'ın Azure için bir [geçici direci](https://www.packer.io/docs/builders/azure.html) vardır ve bu da önceki adımda oluşturulan hizmet temel kimlik bilgileri gibi Azure kaynaklarını tanımlamanıza olanak tanır.

*ubuntu.json* adlı bir dosya oluşturun ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden temin edinilsin |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Create komutundan `az ad sp` ilk çıkış satırı - *appId* |
| *client_secret*                     | Create komutundan `az ad sp` ikinci çıktı satırı - *parola* |
| *tenant_id*                         | Create komutundan `az ad sp` üçüncü çıkış satırı - *kiracı* |
| *subscription_id*                   | Komuttan `az account show` çıkış |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsünün adı |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

Bu şablon bir Ubuntu 16.04 LTS görüntü oluşturur, NGINX yükler, sonra VM deprovisions.

> [!NOTE]
> Bu şablonu kullanıcı kimlik bilgilerini sağlamak için genişletirseniz, Azure aracısını `-deprovision` yerine `deprovision+user`'yi okumak için deprovisions eden geçici komutu ayarlayın.
> Bayrak, `+user` tüm kullanıcı hesaplarını kaynak VM'den kaldırır.


## <a name="build-packer-image"></a>Packer görüntüsü oluşturun
Yerel makinenizde Packer zaten yüklü değilseniz, [Packer yükleme yönergelerini uygulayın.](https://www.packer.io/docs/install/index.html)

Packer şablon dosyanızı aşağıdaki gibi belirterek görüntüyü oluşturun:

```bash
./packer build ubuntu.json
```

Önceki komutlardan çıktı örneği aşağıdaki gibidir:

```output
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH to become available...
==> azure-arm: Connected to SSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! The waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able to login as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

Packer'ın VM'yi oluşturması, geçici maddeyi çalıştırması ve dağıtımı temizlemesi birkaç dakika sürer.


## <a name="create-vm-from-azure-image"></a>Azure Görüntüsü'nden VM oluşturma
Artık az vm oluştur ile Görüntünüzden bir [VM oluşturabilirsiniz.](/cli/azure/vm) Parametreyle oluşturduğunuz `--image` Görüntüyü belirtin. Aşağıdaki örnek, *myPackerImage'den* *myVM* adında bir VM oluşturur ve zaten yoksa SSH anahtarları oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Packer resminizden farklı bir kaynak grubunda veya bölgesinde VM oluşturmak istiyorsanız, görüntü adı yerine görüntü kimliğini belirtin. [Az resim göster](/cli/azure/image#az-image-show)ile resim kimliğini elde edebilirsiniz.

VM'nin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI `publicIpAddress` tarafından görüntülenenlere dikkat edin. Bu adres, NGINX sitesine bir web tarayıcısı üzerinden erişmek için kullanılır.

Web trafiğinin VM’nize erişmesine izin vermek için, [az vm open-port](/cli/azure/vm) komutuyla İnternet’te 80 numaralı bağlantı noktasını açın:

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Test VM ve NGINX
Artık bir web tarayıcısı açıp adres çubuğuna `http://publicIpAddress` ifadesini girebilirsiniz. VM oluşturma işleminden kendi herkese açık IP adresinizi sağlayın. Varsayılan NGINX sayfası aşağıdaki örnekte olduğu gibi görüntülenir:

![Varsayılan NGINX sitesi](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Sonraki adımlar
[Azure Image Builder](image-builder.md)ile varolan Packer geçici komut dosyalarını da kullanabilirsiniz.
