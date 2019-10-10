---
title: 'Hızlı başlangıç: Azure CLı kullanarak bir Linux VM oluşturma'
description: Bu hızlı başlangıçta, bir Linux sanal makinesi oluşturmak için Azure CLı 'yi kullanmayı öğrenirsiniz.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 0208e72263991daceed52073bcce8b3e01f8e8c2
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174290"
---
# <a name="quickstart-create-a-linux-virtual-machine-with-the-azure-cli"></a>Hızlı başlangıç: Azure CLı ile Linux sanal makinesi oluşturma

Bu hızlı başlangıç, Azure 'da bir Linux sanal makinesini (VM) dağıtmak için Azure komut satırı arabirimi 'ni (CLı) nasıl kullanacağınızı gösterir. Azure CLı, komut satırından veya betiklerden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu öğreticide Ubuntu 16,04 LTS 'yi yükleyeceğiz. VM 'yi eylemde görüntülemek için SSH kullanarak buna bağlanır ve NGıNX Web sunucusunu yüklersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell Başlat

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış ortak Azure araçları vardır. 

Cloud Shell açmak için, yalnızca bir kod bloğunun sağ üst köşesinden **dene** ' yi seçin. Cloud Shell, [https://shell.azure.com/bash](https://shell.azure.com/bash)' e giderek ayrı bir tarayıcı sekmesinde de açabilirsiniz. Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve çalıştırmak için **ENTER** ' u seçin.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.30 veya üstünü gerektirir. Sürümü bulmak için `az --version` ' yı çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Az Group Create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Sanal makine oluştur

[Az VM Create](/cli/azure/vm) komutuyla bir VM oluşturun.

Aşağıdaki örnek, *Myvm* ADLı bir VM oluşturur ve *azureuser*adlı bir kullanıcı hesabı ekler. @No__t-0 parametresi, otomatik olarak bir SSH anahtarı oluşturmak ve varsayılan anahtar konumuna ( *~/PST*) koymak için kullanılır. Bunun yerine belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM ve destekleyici kaynakların oluşturulması birkaç dakika sürer. Aşağıdaki örnek çıktı, VM oluşturma işleminin başarılı olduğunu gösterir.

```output
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

SANAL makinenizin çıktısında kendi @no__t (0) yapın. Bu adres, sonraki adımlarda sanal makineye erişmek için kullanılır.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 bağlantı noktasını açın

Varsayılan olarak, Azure 'da bir Linux VM oluşturduğunuzda yalnızca SSH bağlantıları açılır. NGıNX Web sunucusu ile kullanmak üzere 80 numaralı TCP bağlantı noktasını açmak için [az VM Open-Port](/cli/azure/vm) kullanın:

```azurecli-interactive
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlan

Normal olarak sanal makinenize SSH. **Publicıpaddress** değerini sanal makinenizin önceki çıktısında BELIRTILDIĞI gibi VM 'NIZIN genel IP adresiyle değiştirin:

```bash
ssh azureuser@publicIpAddress
```

## <a name="install-web-server"></a>Web sunucusu yükler

VM 'nizi eylemde görmek için NGıNX Web sunucusunu yükledikten sonra. Paket kaynaklarınızı güncelleştirin ve ardından en son NGıNX paketini yükledikten sonra.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

İşiniz bittiğinde SSH oturumunu bırakmak için `exit` yazın.

## <a name="view-the-web-server-in-action"></a>Web sunucusunu eylemde görüntüleme

Varsayılan NGıNX karşılama sayfasını görüntülemek için seçtiğiniz bir Web tarayıcısını kullanın. Web adresi olarak sanal makinenizin genel IP adresini kullanın. Aşağıdaki örnek, varsayılan NGıNX Web sitesini göstermektedir:

![NGıNX varsayılan sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group) komutunu kullanarak kaynak grubunu, VM 'yi ve tüm ilgili kaynakları kaldırabilirsiniz. 

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine dağıttıysanız, Web trafiği için bir ağ bağlantı noktası açarsınız ve temel bir Web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi edinmek için Linux VM 'lerine yönelik öğreticiye geçin.


> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
