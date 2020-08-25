---
title: 'Hızlı başlangıç: iç yük dengeleyici oluşturma-Azure CLı'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure CLı kullanarak iç yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
tags: azure-resource-manager
Customer intent: I want to create a load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: allensu
ms.custom: mvc, devx-track-javascript, devx-track-azurecli
ms.openlocfilehash: 3016825648ef45f8aa64b4228bac2b1f830004c3
ms.sourcegitcommit: e2b36c60a53904ecf3b99b3f1d36be00fbde24fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88763870"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak VM 'Lerin yükünü dengelemek için iç yük dengeleyici oluşturma

Ortak yük dengeleyici ve üç sanal makine oluşturmak için Azure CLı kullanarak Azure Load Balancer kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure CLı yerel olarak veya Azure Cloud Shell yüklendi

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.28 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[Az Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)ile bir kaynak grubu oluşturun:

* **Myresourcegrouplb**adlı adlandırılmış. 
* **Eastus** konumunda.

```azurecli-interactive
  az group create \
    --name myResourceGroupLB \
    --location eastus
```
---

# <a name="standard-sku"></a>[**Standart SKU**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'Lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

VM 'Leri dağıtmadan ve yük dengeleyiciyi dağıtmadan önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-createt)kullanarak bir sanal ağ oluşturun:

* **Myvnet**adında.
* **10.1.0.0/16**adres ön eki.
* **Mybackendsubnet**adlı alt ağ.
* **10.1.0.0/24**alt ağ ön eki.
* **Myresourcegrouplb** kaynak grubunda.
* **Eastus**konumu.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```
### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Standart yük dengeleyici için arka uç adresindeki VM 'Lerin bir ağ güvenlik grubuna ait olan ağ arabirimlerine sahip olması gerekir. 

[Az Network NSG Create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)kullanarak bir ağ güvenlik grubu oluşturun:

* **Mynsg**adlı adlandırılmış.
* Kaynak grubu **Myresourcegrouplb**.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

[Az Network NSG Rule Create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create)kullanarak bir ağ güvenlik grubu kuralı oluşturun:

* **Mynsgrutahttp**adında.
* Önceki adımda oluşturduğunuz ağ güvenlik grubunda, **Mynsg**.
* Kaynak grubu **Myresourcegrouplb**.
* Protokol **(*)**.
* Yön **gelen**.
* Kaynak **(*)**.
* Hedef **(*)**.
* Hedef bağlantı noktası **80**.
* Erişime **Izin ver**.
* Öncelik **200**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupLB \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Sanal makineler için ağ arabirimleri oluşturma

[Az Network Nic Create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create)ile iki ağ arabirimi oluşturun:

#### <a name="vm1"></a>VM1

* **MyNicVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm2"></a>VM2

* **MyNicVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde şunları oluşturursunuz:

* Sunucu yapılandırması için **cloud-init.txt** adlı bir bulut yapılandırma dosyası.
* Yük Dengeleyici için arka uç sunucular olarak kullanılacak iki sanal makine.

### <a name="create-cloud-init-configuration-file"></a>Cloud-init yapılandırma dosyası oluşturma

Bir Cloud-init yapılandırma dosyasını kullanarak NGıNX 'i yükleyip bir Linux sanal makinesinde bir ' Merhaba Dünya ' Node.js uygulaması çalıştırın. 

Geçerli kabuğunuzun içinde cloud-init.txt adlı bir dosya oluşturun. Aşağıdaki yapılandırmayı kopyalayıp kabuğa yapıştırın. Tüm Cloud-init dosyasını, özellikle de ilk satırı doğru şekilde kopyalamadığınıza emin olun:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```
### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

[Az VM Create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create)ile sanal makineler oluşturun:

#### <a name="vm1"></a>VM1
* **MyVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM1**.
* Sanal makine görüntüsü **Ubuntults**.
* Yukarıdaki adımda oluşturduğunuz yapılandırma dosyası **cloud-init.txt** .
* **Bölge 1**.

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 1 \
    --no-wait
    
```
#### <a name="vm2"></a>VM2
* **MyVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM2**.
* Sanal makine görüntüsü **Ubuntults**.
* Yukarıdaki adımda oluşturduğunuz yapılandırma dosyası **cloud-init.txt** .
* **Bölge 2**.

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 2 \
    --no-wait
```

VM 'Lerin dağıtılması birkaç dakika sürebilir.

## <a name="create-standard-load-balancer"></a>Standart yük dengeleyici oluştur

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

  * Yük dengeleyicide gelen ağ trafiğini alan bir ön uç IP Havuzu.
  * Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği bir arka uç IP Havuzu.
  * Arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması.
  * Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-the-load-balancer-resource"></a>Yük dengeleyici kaynağı oluşturma

[Az Network lb Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#az-network-lb-create)komutuyla bir genel yük dengeleyici oluşturun:

* **Myloadbalancer**adlı.
* **Myön uç**adlı bir ön uç Havuzu.
* **Mybackendpool**adlı bir arka uç Havuzu.
* Sanal ağ **Myvnet**ile ilişkili.
* **Mybackendsubnet**arka uç ağıyla ilişkili.

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Standard \
    --vnet-name myVnet \
    --subnet myBackendSubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### <a name="create-the-health-probe"></a>Durum araştırması oluşturma

Bir sistem durumu araştırması, tüm sanal makine örneklerini denetleyerek ağ trafiği gönderebilecekleri emin olmanızı sağlar. 

Başarısız araştırma denetimine sahip bir sanal makine yük dengeleyiciden kaldırılır. Hata çözüldüğünde sanal makine yük dengeleyiciye geri eklenir.

[Az Network lb araştırması Create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create)komutuyla bir sistem durumu araştırması oluşturun:

* Sanal makinelerin sistem durumunu izler.
* Adlandırılmış **Myhealtharaştırma**.
* Protokol **TCP**.
* İzleme **bağlantı noktası 80**.

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı şunları tanımlar:

* Gelen trafik için ön uç IP yapılandırması.
* Trafiği almak için arka uç IP Havuzu.
* Gerekli kaynak ve hedef bağlantı noktası. 

[Az Network lb Rule Create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create)ile bir yük dengeleyici kuralı oluşturun:

* Adlandırılmış **Myhttprule**
* Ön **uç**havuzundaki **80 numaralı bağlantı noktası** dinleniyor.
* **80 numaralı bağlantı noktasını**kullanan **mybackendpool** arka uç adres havuzuna yük dengeli ağ trafiği gönderiliyor. 
* Sistem durumu araştırması **Myhealtharaştırması**kullanılıyor.
* Protokol **TCP**.
* Ön uç IP adresini kullanarak giden kaynak ağ adresi çevirisini (SNAT) etkinleştirin.

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --disable-outbound-snat true 
```
>[!NOTE]
>Arka uç havuzundaki sanal makinelerin, bu yapılandırmayla giden internet bağlantısı olmayacaktır. </br> Giden bağlantı sağlama hakkında daha fazla bilgi için bkz. </br> **[Azure’da giden bağlantılar](load-balancer-outbound-connections.md)**</br> Bağlantı sağlamaya yönelik seçenekler: </br> **[Yalnızca giden yük dengeleyici yapılandırması](egress-only.md)** </br> **[Sanal ağ NAT nedir?](https://docs.microsoft.com/azure/virtual-network/nat-overview)**

### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzuna sanal makineler ekleme

[Az Network Nic IP-Config Address-Pool Add](https://docs.microsoft.com/cli/azure/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add)komutuyla sanal makineleri arka uç havuzuna ekleyin:


#### <a name="vm1"></a>VM1
* Arka uç adres havuzunda **Mybackendpool**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM1** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* Arka uç adres havuzunda **Mybackendpool**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM2** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'LAR hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

VM 'Leri dağıtmadan ve yük dengeleyiciyi dağıtmadan önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-createt)kullanarak bir sanal ağ oluşturun:

* **Myvnet**adında.
* **10.1.0.0/16**adres ön eki.
* **Mybackendsubnet**adlı alt ağ.
* **10.1.0.0/24**alt ağ ön eki.
* **Myresourcegrouplb** kaynak grubunda.
* **Eastus**konumu.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```
### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Standart yük dengeleyici için arka uç adresindeki VM 'Lerin bir ağ güvenlik grubuna ait olan ağ arabirimlerine sahip olması gerekir. 

[Az Network NSG Create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)kullanarak bir ağ güvenlik grubu oluşturun:

* **Mynsg**adlı adlandırılmış.
* Kaynak grubu **Myresourcegrouplb**.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

[Az Network NSG Rule Create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create)kullanarak bir ağ güvenlik grubu kuralı oluşturun:

* **Mynsgrutahttp**adında.
* Önceki adımda oluşturduğunuz ağ güvenlik grubunda, **Mynsg**.
* Kaynak grubu **Myresourcegrouplb**.
* Protokol **(*)**.
* Yön **gelen**.
* Kaynak **(*)**.
* Hedef **(*)**.
* Hedef bağlantı noktası **80**.
* Erişime **Izin ver**.
* Öncelik **200**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupLB \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Sanal makineler için ağ arabirimleri oluşturma

[Az Network Nic Create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create)ile iki ağ arabirimi oluşturun:

#### <a name="vm1"></a>VM1

* **MyNicVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm2"></a>VM2

* **MyNicVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde şunları oluşturursunuz:

* Sunucu yapılandırması için **cloud-init.txt** adlı bir bulut yapılandırma dosyası. 
* Sanal makineler için kullanılabilirlik kümesi
* Yük Dengeleyici için arka uç sunucular olarak kullanılacak iki sanal makine.

Yük dengeleyicinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere NGıNX yüklersiniz.

### <a name="create-cloud-init-configuration-file"></a>Cloud-init yapılandırma dosyası oluşturma

Bir Cloud-init yapılandırma dosyasını kullanarak NGıNX 'i yükleyip bir Linux sanal makinesinde bir ' Merhaba Dünya ' Node.js uygulaması çalıştırın. 

Geçerli kabuğunuzun içinde cloud-init.txt adlı bir dosya oluşturun. Aşağıdaki yapılandırmayı kopyalayıp kabuğa yapıştırın. Tüm Cloud-init dosyasını, özellikle de ilk satırı doğru şekilde kopyalamadığınıza emin olun:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-availability-set-for-virtual-machines"></a>Sanal makineler için kullanılabilirlik kümesi oluştur

Kullanılabilirlik kümesini [az VM AVAILABILITY-set create](https://docs.microsoft.com/cli/azure/vm/availability-set?view=azure-cli-latest#az-vm-availability-set-create)ile oluşturun:

* **MyAvSet**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Konum **eastus**.

```azurecli-interactive
  az vm availability-set create \
    --name myAvSet \
    --resource-group myResourceGroupLB \
    --location eastus 
    
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

[Az VM Create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create)ile sanal makineler oluşturun:

#### <a name="vm1"></a>VM1
* **MyVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM1**.
* Sanal makine görüntüsü **Ubuntults**.
* Yukarıdaki adımda oluşturduğunuz yapılandırma dosyası **cloud-init.txt** .
* Kullanılabilirlik kümesi **myAvSet**içinde.

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet \
    --no-wait
    
```
#### <a name="vm2"></a>VM2
* **MyVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM2**.
* Sanal makine görüntüsü **Ubuntults**.
* Yukarıdaki adımda oluşturduğunuz yapılandırma dosyası **cloud-init.txt** .
* **Bölge 2**.

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --admin-user azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```

VM 'Lerin dağıtılması birkaç dakika sürebilir.

## <a name="create-basic-load-balancer"></a>Temel yük dengeleyici oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

  * Yük dengeleyicide gelen ağ trafiğini alan bir ön uç IP Havuzu.
  * Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği bir arka uç IP Havuzu.
  * Arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması.
  * Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-the-load-balancer-resource"></a>Yük dengeleyici kaynağı oluşturma

[Az Network lb Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#az-network-lb-create)komutuyla bir genel yük dengeleyici oluşturun:

* **Myloadbalancer**adlı.
* **Myön uç**adlı bir ön uç Havuzu.
* **Mybackendpool**adlı bir arka uç Havuzu.
* Sanal ağ **Myvnet**ile ilişkili.
* **Mybackendsubnet**arka uç ağıyla ilişkili.

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Basic \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### <a name="create-the-health-probe"></a>Durum araştırması oluşturma

Bir sistem durumu araştırması, tüm sanal makine örneklerini denetleyerek ağ trafiği gönderebilecekleri emin olmanızı sağlar. 

Başarısız araştırma denetimine sahip bir sanal makine yük dengeleyiciden kaldırılır. Hata çözüldüğünde sanal makine yük dengeleyiciye geri eklenir.

[Az Network lb araştırması Create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create)komutuyla bir sistem durumu araştırması oluşturun:

* Sanal makinelerin sistem durumunu izler.
* Adlandırılmış **Myhealtharaştırma**.
* Protokol **TCP**.
* İzleme **bağlantı noktası 80**.

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı şunları tanımlar:

* Gelen trafik için ön uç IP yapılandırması.
* Trafiği almak için arka uç IP Havuzu.
* Gerekli kaynak ve hedef bağlantı noktası. 

[Az Network lb Rule Create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create)ile bir yük dengeleyici kuralı oluşturun:

* Adlandırılmış **Myhttprule**
* Ön **uç**havuzundaki **80 numaralı bağlantı noktası** dinleniyor.
* **80 numaralı bağlantı noktasını**kullanan **mybackendpool** arka uç adres havuzuna yük dengeli ağ trafiği gönderiliyor. 
* Sistem durumu araştırması **Myhealtharaştırması**kullanılıyor.
* Protokol **TCP**.

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```
### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzuna sanal makineler ekleme

[Az Network Nic IP-Config Address-Pool Add](https://docs.microsoft.com/cli/azure/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add)komutuyla sanal makineleri arka uç havuzuna ekleyin:


#### <a name="vm1"></a>VM1
* Arka uç adres havuzunda **Mybackendpool**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM1** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* Arka uç adres havuzunda **Mybackendpool**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM2** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

---

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

### <a name="create-azure-bastion-public-ip"></a>Azure savunma genel IP 'si oluşturma

Savunma konağı için genel bir IP adresi oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) kullanın:

* **Mybastionıp**adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Myresourcegrouplb**içinde.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myBastionIP \
    --sku Standard
```

### <a name="create-azure-bastion-subnet"></a>Azure savunma alt ağı oluştur

Alt ağ oluşturmak için [az Network VNET subnet Create](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-create) kullanın:

* **AzureBastionSubnet**adlı.
* **10.1.1.0/24**adres ön eki.
* Sanal ağ **\**sanal ağı 'nda.
* Kaynak grubu **Myresourcegrouplb**.

```azurecli-interactive
  az network vnet subnet create \
    --resource-group myResourceGroupLB \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-azure-bastion-host"></a>Azure savunma Konağı oluşturma
Bir savunma konağı oluşturmak için [az Network savunma Create](https://docs.microsoft.com/cli/azure/network/bastion?view=azure-cli-latest#az-network-bastion-create) kullanın:

* Adlandırılmış **Mybastionhost**
* **Myresourcegrouplb** içinde
* Genel IP **Mybastionıp**ile ilişkilendirildi.
* Sanal ağ **Myvnet**ile ilişkilendirildi.
* **Eastus** konumunda.

```azurecli-interactive
  az network bastion create \
    --resource-group myResourceGroupLB \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```
Savunma konağının dağıtılması birkaç dakika sürer.

### <a name="create-test-virtual-machine"></a>Test sanal makinesi oluştur

Ağ arabirimini [az ağ NIC Create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create)ile oluşturun:

* **Mynictestvm**adında.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicTestVM \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
Sanal makineyi [az VM Create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create)ile oluşturun:

* **Mytestvm**adında.
* Kaynak grubu **Myresourcegrouplb**.
* **Mynictestvm**ağ arabirimine eklendi.
* Sanal makine görüntüsü **Win2019Datacenter**.
* Ve değerlerini seçin **\<adminpass>** **\<adminuser>** .
  

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myTestVM \
    --nics myNicTestVM \
    --image Win2019Datacenter \
    --admin-username <adminuser> \
    --admin-password <adminpass> \
    --no-wait
```
Sanal makinenin dağıtılması birkaç dakika sürebilir.

### <a name="test"></a>Test

1. Azure portalında [oturum açın](https://portal.azure.com).

1. **Genel bakış** ekranında yük DENGELEYICININ özel IP adresini bulun. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından **myloadbalancer**' ı seçin.

2. **Myloadbalancer** **genel** görünümünde, **özel IP adresi** ' nin yanındaki bir yere göz atın veya adresi kopyalayın.

3. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **Myresourcegrouplb** kaynak grubunda bulunan **mytestvm** ' yi seçin.

4. **Genel bakış** sayfasında **Bağlan** **' ı ve**sonra da ' yi seçin.

6. VM oluşturma sırasında girilen kullanıcı adını ve parolayı girin.

7. **Mytestvm**'de **Internet Explorer** 'ı açın.

8. Önceki adımdan, tarayıcının adres çubuğuna IP adresini girin. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Standart iç yük dengeleyici oluşturma" border="true":::
   
Yük dengeleyicinin trafiği üç VM 'ye dağıtmasını görmek için, her bir sanal makinenin IIS Web sunucusunun varsayılan sayfasını özelleştirebilir ve sonra Web tarayıcınızı istemci makinesinden yenileyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive
  az group delete \
    --name myResourceGroupLB
```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta

* Standart veya genel yük dengeleyici oluşturdunuz
* Bağlı sanal makineler. 
* Yük dengeleyici trafik kuralı ve sistem durumu araştırması yapılandırıldı.
* Yük dengeleyici test edildi.

Azure Load Balancer hakkında daha fazla bilgi edinmek için [Azure Load Balancer nedir?](load-balancer-overview.md) ve [sık sorulan sorular Load Balancer](load-balancer-faqs.md).

[Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
