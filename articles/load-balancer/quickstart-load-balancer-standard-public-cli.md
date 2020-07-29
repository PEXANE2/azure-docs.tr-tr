---
title: 'Hızlı başlangıç: ortak yük dengeleyici oluşturma-Azure CLı'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure CLI kullanarak genel bir yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
tags: azure-resource-manager
Customer intent: I want to create a load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: aa0d29c5c2a4cf8eebbf530b42a25d8924e031bc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290263"
---
# <a name="quickstart-create-a-public-load-balancer-to-load-balance-vms-using-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak sanal makinelerin yük dengelemesi için genel yük dengeleyici oluşturma

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

# <a name="option-1-default-create-a-load-balancer-standard-sku"></a>[Seçenek 1 (varsayılan): yük dengeleyici oluşturma (Standart SKU)](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.


## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. 

[Az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) to kullanın:

* **Mypublicıp**adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Myresourcegrouplb**içinde.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Standard
```

Bölge 1 ' de gereksiz bir genel IP adresi oluşturmak için:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Standard \
    --zone 1
```

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
* Önceki adımda oluşturduğunuz **Mypublicıp** genel IP adresiyle ilişkili. 

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Standard \
    --public-ip-address myPublicIP \
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

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

VM 'Leri dağıtmadan ve yük dengeleyicinizi test etmeden önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-createt)kullanarak bir sanal ağ oluşturun:

* **Myvnet**adında.
* **Mybackendsubnet**adlı alt ağ.
* **Myresourcegrouplb** kaynak grubunda.
* **Eastus**konumu.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVNet \
    --subnet-name myBackendSubnet
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
* Protokol **TCP**.
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
    --protocol tcp \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Sanal makineler için ağ arabirimleri oluşturma

[Az Network Nic Create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create)ile üç ağ arabirimi oluşturun:

#### <a name="vm1"></a>VM1

* **MyNicVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurecli-interactive

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
```
#### <a name="vm2"></a>VM2

* **MyNicVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
```
#### <a name="vm3"></a>VM3

* **MyNicVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM3 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde şunları oluşturursunuz:

* Sunucu yapılandırması için **cloud-init.txt** adlı bir bulut yapılandırma dosyası.
* Yük Dengeleyici için arka uç sunucular olarak kullanılacak üç sanal makine.

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
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 2 \
    --no-wait
```

#### <a name="vm3"></a>VM3
* **MyVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM3**.
* Sanal makine görüntüsü **Ubuntults**.
* Yukarıdaki adımda oluşturduğunuz yapılandırma dosyası **cloud-init.txt** .
* **Bölge 3**.

```azurecli-interactive
   az vm create \
    --resource-group myResourceGroupLB \
    --name myVM3 \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 3 \
    --no-wait
```
VM 'Lerin dağıtılması birkaç dakika sürebilir.

## <a name="create-outbound-rule-configuration"></a>Giden kuralı yapılandırması oluştur
Yük dengeleyici giden kuralları arka uç havuzundaki VM 'Ler için giden SNAT 'yi yapılandırır. 

Giden bağlantılar hakkında daha fazla bilgi için bkz. [Azure 'Da giden bağlantılar](load-balancer-outbound-connections.md).

### <a name="create-outbound-public-ip-address-or-public-ip-prefix"></a>Giden genel IP adresi veya genel IP öneki oluşturun.

Giden bağlantı için tek bir IP oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) kullanın.  

Giden bağlantı için genel bir IP öneki oluşturmak için [az Network public-ip Create oluştur](https://docs.microsoft.com/cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-create) kullanın.

Giden NAT ve giden bağlantıların ölçeklendirilmesi hakkında daha fazla bilgi için bkz. [birden çok IP adresi Ile genişleme gıden NAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#scale).

#### <a name="public-ip"></a>Genel IP

* Adlandırılmış **Mypublicıpoıb Utbağlanmadı**.
* **Myresourcegrouplb**içinde.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIPOutbound \
    --sku Standard
```

Bölge 1 ' de gereksiz bir genel IP adresi oluşturmak için:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIPOutbound \
    --sku Standard \
    --zone 1
```
#### <a name="public-ip-prefix"></a>Genel IP ön eki

* **MyPublicIPPrefixOutbound**adlı.
* **Myresourcegrouplb**içinde.
* **28**önek uzunluğu.

```azurecli-interactive
  az network public-ip prefix create \
    --resource-group myResourceGroupLB \
    --name myPublicIPPrefixOutbound \
    --length 28
```
Bölge 1 ' de gereksiz bir genel IP öneki oluşturmak için:

```azurecli-interactive
  az network public-ip prefix create \
    --resource-group myResourceGroupLB \
    --name myPublicIPPrefixOutbound \
    --length 28 \
    --zone 1
```

### <a name="create-outbound-frontend-ip-configuration"></a>Giden ön uç IP yapılandırması oluştur

[Az Network lb ön uç-IP Create ](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create)ile yeni bir ön uç IP yapılandırması oluşturun:

Önceki adımdaki kararı temel alarak genel IP veya genel IP öneki komutlarını seçin.

#### <a name="public-ip"></a>Genel IP

* **MyFrontEndOutbound**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Genel IP adresiyle ilişkili **Mypublicıpoıb Utbağlanmadı**.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myResourceGroupLB \
    --name myFrontEndOutbound \
    --lb-name myLoadBalancer \
    --public-ip-address myPublicIPOutbound 
```

#### <a name="public-ip-prefix"></a>Genel IP ön eki

* **MyFrontEndOutbound**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Genel IP önekiyle ilişkili **myPublicIPPrefixOutbound**.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myResourceGroupLB \
    --name myFrontEndOutbound \
    --lb-name myLoadBalancer \
    --public-ip-prefix myPublicIPPrefixOutbound 
```

### <a name="create-outbound-pool"></a>Giden Havuz oluştur

[Az Network lb Address-Pool Create](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create)komutuyla yeni bir giden havuz oluşturun:

* **Mybackendpooloutbound**adlı adlandırılmış.
* Kaynak grubu **Myresourcegrouplb**.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myBackendPoolOutbound
```
### <a name="create-outbound-rule"></a>Giden kuralı oluştur

Giden arka uç havuzu için [az Network lb giden kuralı oluştur](https://docs.microsoft.com/cli/azure/network/lb/outbound-rule?view=azure-cli-latest#az-network-lb-outbound-rule-create)ile yeni bir giden kuralı oluşturun:

* Adlandırılmış **mbir Boundrule**.
* Kaynak grubu **Myresourcegrouplb**.
* Yük dengeleyici **Myloadbalancer** ile ilişkili
* Ön uç **myFrontEndOutbound**ile ilişkili.
* **Tüm**protokol.
* **15**' in boşta kalma süresi.
* **10000** giden bağlantı noktası.
* Arka uç havuzuyla ilişkili **Mybackendpooloutbound**.

```azurecli-interactive
  az network lb outbound-rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myOutboundRule \
    --frontend-ip-configs myFrontEndOutbound \
    --protocol All \
    --idle-timeout 15 \
    --outbound-ports 10000 \
    --address-pool myBackEndPoolOutbound
```
### <a name="add-virtual-machines-to-outbound-pool"></a>Sanal makineleri giden havuzuna Ekle

[Az Network Nic IP-Config Address-Pool Add](https://docs.microsoft.com/cli/azure/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add)komutuyla sanal makineleri giden havuza ekleyin:


#### <a name="vm1"></a>VM1
* Arka uç adres havuzunda **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM1** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* Arka uç adres havuzunda **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM2** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* Arka uç adres havuzunda **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM3** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

# <a name="option-2-create-a-load-balancer-basic-sku"></a>[2. seçenek: yük dengeleyici oluşturma (temel SKU)](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.


## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. 

[Az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) to kullanın:

* **Mypublicıp**adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Myresourcegrouplb**içinde.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Basic
```

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
* Önceki adımda oluşturduğunuz **Mypublicıp** genel IP adresiyle ilişkili. 

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Basic \
    --public-ip-address myPublicIP \
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

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

VM 'Leri dağıtmadan ve yük dengeleyicinizi test etmeden önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-createt)kullanarak bir sanal ağ oluşturun:

* **Myvnet**adında.
* **Mybackendsubnet**adlı alt ağ.
* **Myresourcegrouplb** kaynak grubunda.
* **Eastus**konumu.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVNet \
    --subnet-name myBackendSubnet
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
* Protokol **TCP**.
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
    --protocol tcp \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Sanal makineler için ağ arabirimleri oluşturma

[Az Network Nic Create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create)ile üç ağ arabirimi oluşturun:

#### <a name="vm1"></a>VM1

* **MyNicVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurecli-interactive

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
```
#### <a name="vm2"></a>VM2

* **MyNicVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
```
#### <a name="vm3"></a>VM3

* **MyNicVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM3 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde şunları oluşturursunuz:

* Sunucu yapılandırması için **cloud-init.txt** adlı bir bulut yapılandırma dosyası. 
* Sanal makineler için kullanılabilirlik kümesi
* Yük Dengeleyici için arka uç sunucular olarak kullanılacak üç sanal makine.


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
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```

#### <a name="vm3"></a>VM3
* **MyVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM3**.
* Sanal makine görüntüsü **Ubuntults**.
* Yukarıdaki adımda oluşturduğunuz yapılandırma dosyası **cloud-init.txt** .
* **Bölge 3**.

```azurecli-interactive
   az vm create \
    --resource-group myResourceGroupLB \
    --name myVM3 \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```
VM 'Lerin dağıtılması birkaç dakika sürebilir.

---

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Yük dengeleyicinin genel IP adresini almak için [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show) komutunu kullanın. 

Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```
:::image type="content" source="./media/load-balancer-standard-public-cli/running-nodejs-app.png" alt-text="Yük dengeleyiciyi test etme" border="true":::

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