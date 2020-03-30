---
title: Şablondan Azure'da Linux VM oluşturma
description: Kaynak Yöneticisi şablonundan Linux VM oluşturmak için Azure CLI nasıl kullanılır?
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.openlocfilehash: 581eadc60835b758f67ae616d4413800f1d6d718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969518"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonları ile Linux sanal makinesi oluşturma

Azure Kaynak Yöneticisi şablonu ve Azure Bulut uyruzundan Azure CLI şablonu kullanarak nasıl bir Linux sanal makinesi (VM) oluşturabilirsiniz öğrenin. Windows sanal makinesi oluşturmak için [bkz.](../windows/ps-template.md)

## <a name="templates-overview"></a>Şablonlara genel bakış

Azure Kaynak Yöneticisi şablonları, Azure çözümünüzün altyapısını ve yapılandırmasını tanımlayan JSON dosyalarıdır. Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. Şablonun biçimi ve nasıl oluşturduğunuz hakkında daha fazla bilgi edinmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) Kaynak türleri için JSON söz dizimini görüntülemek üzere bkz. [Azure Resource Manager şablonlarında kaynak tanımlama](/azure/templates/microsoft.compute/allversions).

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Azure sanal makinesi oluşturmak genellikle iki adım içerir:

1. Bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır.
1. Sanal makine oluşturur.

Aşağıdaki örnekte, [Azure Quickstart şablonundan](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)bir VM oluşturulur. Bu dağıtım için yalnızca SSH kimlik doğrulaması izin verilir. İstendiğinde, *~/.ssh/id_rsa.pub'ın*içeriği gibi kendi SSH ortak anahtarınızın değerini sağlayın. Bir SSH anahtar çifti oluşturmanız gerekiyorsa, [Azure'da Linux VM'leri için bir SSH anahtar çiftinin nasıl oluşturulup kullanılacağına](mac-create-ssh-keys.md)bakın. Şablonun bir kopyası aşağıda verilmiştir:

[!code-json[create-linux-vm](~/quickstart-templates/101-vm-sshkey/azuredeploy.json)]

CLI komut dosyasını çalıştırmak için Azure Bulutu kabuğunu açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the project name (used for generating resource names):" &&
read projectName &&
echo "Enter the administrator username:" &&
read username &&
echo "Enter the SSH public key:" &&
read key &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json --parameters projectName=$projectName adminUsername=$username adminPublicKey="$key" &&
az vm show --resource-group $resourceGroupName --name "$projectName-vm" --show-details --query publicIps --output tsv
```

Son Azure CLI komutu, yeni oluşturulan VM'nin genel IP adresini gösterir. Sanal makineye bağlanmak için ortak IP adresine ihtiyacınız var. Bu makalenin sonraki bölümüne bakın.

Önceki örnekte, GitHub'da depolanan bir şablon belirtilmişsiniz. Ayrıca bir şablon indirebilir veya oluşturabilir ve `--template-file` parametreile yerel yolu belirtebilirsiniz.

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

- Kaynak Yöneticisi şablonlarını nasıl geliştireceklerini öğrenmek için [Azure Kaynak Yöneticisi belgelerine](/azure/azure-resource-manager/)bakın.
- Azure sanal makine şemalarını görmek için [Azure şablonu başvurusuna](/azure/templates/microsoft.compute/allversions)bakın.
- Daha fazla sanal makine şablonu örneği görmek için [Azure Quickstart şablonlarına](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular)bakın.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Daha sonra Normal olarak VM SSH olabilir. Yukarıdaki komuttan kendi genel IP adresinizi sağlayın:

```bash
ssh <adminUsername>@<ipAddress>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, temel bir Linux VM oluşturdunuz. Uygulama çerçeveleri içeren veya daha karmaşık ortamlar oluşturan daha fazla Kaynak Yöneticisi şablonu için [Azure Hızlı Başlatma şablonlarına](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular)göz atın.

Şablon oluşturma hakkında daha fazla bilgi edinmek için, dağıttığınız kaynak türlerinin JSON sözdizimini ve özelliklerini görüntüleyin:

- [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups)
- [Microsoft.Network/publicIPAdresleri](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
