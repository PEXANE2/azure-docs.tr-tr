---
title: 'Hızlı başlangıç: iç yük dengeleyici oluşturma-Azure CLı'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure CLı kullanarak iç yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2020
ms.author: allensu
ms.custom: mvc, devx-track-js, devx-track-azurecli
ms.openlocfilehash: edf893f1f6ba0691da5764420017282d7a8bde84
ms.sourcegitcommit: 61d2b2211f3cc18f1be203c1bc12068fc678b584
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2021
ms.locfileid: "98562820"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak VM 'Lerin yükünü dengelemek için iç yük dengeleyici oluşturma

İç yük dengeleyici ve üç sanal makine oluşturmak için Azure CLı kullanarak Azure Load Balancer kullanmaya başlayın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)] 

- Bu hızlı başlangıç, Azure CLı 'nin sürüm 2.0.28 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[Az Group Create](/cli/azure/group#az_group_create)ile bir kaynak grubu oluşturun:

* Adlandırılmış **Createıntlbqs-RG**. 
* **Eastus** konumunda.

```azurecli-interactive
  az group create \
    --name CreateIntLBQS-rg \
    --location eastus

```
---

# <a name="standard-sku"></a>[**Standart SKU**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'Lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

Bu bölümde, sanal makinelerin yükünü dengeleyen bir yük dengeleyici oluşturacaksınız. 

Bir iç yük dengeleyici oluşturduğunuzda, yük dengeleyici için ağ olarak bir sanal ağ yapılandırılır. 

Aşağıdaki diyagramda bu hızlı başlangıçta oluşturulan kaynaklar gösterilmektedir:

:::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/resources-diagram-internal.png" alt-text="Hızlı başlangıç için oluşturulan Standart yük dengeleyici kaynakları." border="false":::

## <a name="configure-virtual-network---standard"></a>Sanal ağ yapılandırma-standart

VM 'Leri dağıtmadan ve yük dengeleyiciyi dağıtmadan önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Az Network VNET Create](/cli/azure/network/vnet#az-network-vnet-create)kullanarak bir sanal ağ oluşturun:

* **Myvnet** adında.
* **10.1.0.0/16** adres ön eki.
* **Mybackendsubnet** adlı alt ağ.
* **10.1.0.0/24** alt ağ ön eki.
* **Createıntlbqs-RG** kaynak grubunda.
* **Eastus** konumu.

```azurecli-interactive
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Savunma konağı için genel bir IP adresi oluşturmak için [az Network public-ip Create](/cli/azure/network/public-ip#az-network-public-ip-create) kullanın:

* **Mybastionıp** adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Createıntlbqs-RG** içinde.

```azurecli-interactive
az network public-ip create \
    --resource-group CreateIntLBQS-rg  \
    --name myBastionIP \
    --sku Standard
```
### <a name="create-a-bastion-subnet"></a>Savunma alt ağı oluşturma

Bir savunma alt ağı oluşturmak için [az Network VNET subnet Create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) kullanın:

* **AzureBastionSubnet** adlı.
* **10.1.1.0/24** adres ön eki.
* Sanal ağ **\** sanal ağı 'nda.
* **Createıntlbqs-RG** kaynak grubunda.

```azurecli-interactive
az network vnet subnet create \
    --resource-group CreateIntLBQS-rg  \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-bastion-host"></a>Savunma Konağı oluştur

Bir savunma konağı oluşturmak için [az Network savunma Create](/cli/azure/network/bastion#az-network-bastion-create) kullanın:

* Adlandırılmış **Mybastionhost**.
* **Createıntlbqs-RG** içinde.
* Genel IP **Mybastionıp** ile ilişkilendirildi.
* Sanal ağ **Myvnet** ile ilişkilendirildi.
* **Eastus** konumunda.

```azurecli-interactive
az network bastion create \
    --resource-group CreateIntLBQS-rg  \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```

Azure savunma konağının dağıtılması birkaç dakika sürebilir.


### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Standart yük dengeleyici için arka uç adresindeki VM 'Lerin bir ağ güvenlik grubuna ait olan ağ arabirimlerine sahip olması gerekir. 

[Az Network NSG Create](/cli/azure/network/nsg#az-network-nsg-create)kullanarak bir ağ güvenlik grubu oluşturun:

* **Mynsg** adlı adlandırılmış.
* **Createıntlbqs-RG** kaynak grubunda.

```azurecli-interactive
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

[Az Network NSG Rule Create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)kullanarak bir ağ güvenlik grubu kuralı oluşturun:

* **Mynsgrutahttp** adında.
* Önceki adımda oluşturduğunuz ağ güvenlik grubunda, **Mynsg**.
* **Createıntlbqs-RG** kaynak grubunda.
* Protokol **(*)**.
* Yön **gelen**.
* Kaynak **(*)**.
* Hedef **(*)**.
* Hedef bağlantı noktası **80**.
* Erişime **Izin ver**.
* Öncelik **200**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
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

## <a name="create-backend-servers---standard"></a>Arka uç sunucuları oluşturma-standart

Bu bölümde şunları oluşturursunuz:

* Sanal makineler için üç ağ arabirimi.
* Yük Dengeleyici için arka uç sunucular olarak kullanılacak üç sanal makine.

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Sanal makineler için ağ arabirimleri oluşturma

[Az Network Nic Create](/cli/azure/network/nic#az-network-nic-create)ile üç ağ arabirimi oluşturun:

* Adlandırılmış **myNicVM1**, **myNicVM2** ve **myNicVM3**.
* **Createıntlbqs-RG** kaynak grubunda.
* Sanal ağ **\** sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive
  array=(myNicVM1 myNicVM2 myNicVM3)
  for vmnic in "${array[@]}"
  do
    az network nic create \
        --resource-group CreateIntLBQS-rg \
        --name $vmnic \
        --vnet-name myVNet \
        --subnet myBackEndSubnet \
        --network-security-group myNSG
  done
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create)ile sanal makineler oluşturun:

* Adlandırılmış **myVM1**, **myVM2** ve **myVM3**.
* **Createıntlbqs-RG** kaynak grubunda.
* **MyNicVM1**, **myNicVM2** ve **myNicVM3** ağ arabirimine iliştirilir.
* Sanal makine görüntüsü **win2019datacenter**.
* **Bölge 1**, **bölge 2** ve **Bölge 3**.

```azurecli-interactive
  array=(1 2 3)
  for n in "${array[@]}"
  do
    az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM$n \
    --nics myNicVM$n \
    --image win2019datacenter \
    --admin-username azureuser \
    --zone $n \
    --no-wait
  done
```

VM 'Lerin dağıtılması birkaç dakika sürebilir.

## <a name="create-standard-load-balancer"></a>Standart yük dengeleyici oluştur

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

  * Yük dengeleyicide gelen ağ trafiğini alan bir ön uç IP Havuzu.
  * Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği bir arka uç IP Havuzu.
  * Arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması.
  * Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-the-load-balancer-resource"></a>Yük dengeleyici kaynağı oluşturma

[Az Network lb Create](/cli/azure/network/lb#az-network-lb-create)komutuyla bir genel yük dengeleyici oluşturun:

* **Myloadbalancer** adlı.
* **Myön uç** adlı bir ön uç Havuzu.
* **Mybackendpool** adlı bir arka uç Havuzu.
* Sanal ağ **Myvnet** ile ilişkili.
* **Mybackendsubnet** arka uç ağıyla ilişkili.

```azurecli-interactive
  az network lb create \
    --resource-group CreateIntLBQS-rg \
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

[Az Network lb araştırması Create](/cli/azure/network/lb/probe#az-network-lb-probe-create)komutuyla bir sistem durumu araştırması oluşturun:

* Sanal makinelerin sistem durumunu izler.
* Adlandırılmış **Myhealtharaştırma**.
* Protokol **TCP**.
* İzleme **bağlantı noktası 80**.

```azurecli-interactive
  az network lb probe create \
    --resource-group CreateIntLBQS-rg \
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

[Az Network lb Rule Create](/cli/azure/network/lb/rule#az-network-lb-rule-create)ile bir yük dengeleyici kuralı oluşturun:

* Adlandırılmış **Myhttprule**
* Ön **uç** havuzundaki **80 numaralı bağlantı noktası** dinleniyor.
* **80 numaralı bağlantı noktasını** kullanan **mybackendpool** arka uç adres havuzuna yük dengeli ağ trafiği gönderiliyor. 
* Sistem durumu araştırması **Myhealtharaştırması** kullanılıyor.
* Protokol **TCP**.
* **15 dakikalık** boşta zaman aşımı.
* TCP sıfırlamayı etkinleştirin.

```azurecli-interactive
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15 \
    --enable-tcp-reset true
```

### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzuna sanal makineler ekleme

[Az Network Nic IP-Config Address-Pool Add](/cli/azure/network/nic/ip-config/address-pool#az-network-nic-ip-config-address-pool-add)komutuyla sanal makineleri arka uç havuzuna ekleyin:

* Arka uç adres havuzunda **Mybackendpool**.
* **Createıntlbqs-RG** kaynak grubunda.
* **MyNicVM1**, **myNicVM2** ve **myNicVM3** ağ arabirimiyle ilişkili.
* Yük dengeleyici **Myloadbalancer** ile ilişkili.

```azurecli-interactive
  array=(VM1 VM2 VM3)
  for vm in "${array[@]}"
  do
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNic$vm \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
  done

```

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'LAR hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

Bu bölümde, sanal makinelerin yükünü dengeleyen bir yük dengeleyici oluşturacaksınız. 

Bir iç yük dengeleyici oluşturduğunuzda, yük dengeleyici için ağ olarak bir sanal ağ yapılandırılır. 

Aşağıdaki diyagramda bu hızlı başlangıçta oluşturulan kaynaklar gösterilmektedir:

:::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/resources-diagram-internal-basic.png" alt-text="Hızlı başlangıçta oluşturulan temel yük dengeleyici kaynakları." border="false":::

## <a name="configure-virtual-network---basic"></a>Sanal ağı yapılandırma-temel

VM 'Leri dağıtmadan ve yük dengeleyiciyi dağıtmadan önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Az Network VNET Create](/cli/azure/network/vnet#az-network-vnet-createt)kullanarak bir sanal ağ oluşturun:

* **Myvnet** adında.
* **10.1.0.0/16** adres ön eki.
* **Mybackendsubnet** adlı alt ağ.
* **10.1.0.0/24** alt ağ ön eki.
* **Createıntlbqs-RG** kaynak grubunda.
* **Eastus** konumu.

```azurecli-interactive
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Savunma konağı için genel bir IP adresi oluşturmak için [az Network public-ip Create](/cli/azure/network/public-ip#az-network-public-ip-create) kullanın:

* **Mybastionıp** adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Createıntlbqs-RG** içinde.

```azurecli-interactive
az network public-ip create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionIP \
    --sku Standard
```
### <a name="create-a-bastion-subnet"></a>Savunma alt ağı oluşturma

Bir savunma alt ağı oluşturmak için [az Network VNET subnet Create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) kullanın:

* **AzureBastionSubnet** adlı.
* **10.1.1.0/24** adres ön eki.
* Sanal ağ **\** sanal ağı 'nda.
* **Createıntlbqs-RG** kaynak grubunda.

```azurecli-interactive
az network vnet subnet create \
    --resource-group CreateIntLBQS-rg \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-bastion-host"></a>Savunma Konağı oluştur

Bir savunma konağı oluşturmak için [az Network savunma Create](/cli/azure/network/bastion#az-network-bastion-create) kullanın:

* Adlandırılmış **Mybastionhost**.
* **Createıntlbqs-RG** içinde.
* Genel IP **Mybastionıp** ile ilişkilendirildi.
* Sanal ağ **Myvnet** ile ilişkilendirildi.
* **Eastus** konumunda.

```azurecli-interactive
az network bastion create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```

Azure savunma konağının dağıtılması birkaç dakika sürebilir.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Standart yük dengeleyici için arka uç adresindeki VM 'Lerin bir ağ güvenlik grubuna ait olan ağ arabirimlerine sahip olması gerekir. 

[Az Network NSG Create](/cli/azure/network/nsg#az-network-nsg-create)kullanarak bir ağ güvenlik grubu oluşturun:

* **Mynsg** adlı adlandırılmış.
* **Createıntlbqs-RG** kaynak grubunda.

```azurecli-interactive
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

[Az Network NSG Rule Create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)kullanarak bir ağ güvenlik grubu kuralı oluşturun:

* **Mynsgrutahttp** adında.
* Önceki adımda oluşturduğunuz ağ güvenlik grubunda, **Mynsg**.
* **Createıntlbqs-RG** kaynak grubunda.
* Protokol **(*)**.
* Yön **gelen**.
* Kaynak **(*)**.
* Hedef **(*)**.
* Hedef bağlantı noktası **80**.
* Erişime **Izin ver**.
* Öncelik **200**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
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

## <a name="create-backend-servers---basic"></a>Arka uç sunucuları oluşturma-temel

Bu bölümde şunları oluşturursunuz:

* Sanal makineler için üç ağ arabirimi.
* Sanal makineler için kullanılabilirlik kümesi
* Yük Dengeleyici için arka uç sunucular olarak kullanılacak üç sanal makine.

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Sanal makineler için ağ arabirimleri oluşturma

[Az Network Nic Create](/cli/azure/network/nic#az-network-nic-create)ile üç ağ arabirimi oluşturun:

* Adlandırılmış **myNicVM1**, **myNicVM2** ve **myNicVM3**.
* **Createıntlbqs-RG** kaynak grubunda.
* Sanal ağ **\** sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive
  array=(myNicVM1 myNicVM2 myNicVM3)
  for vmnic in "${array[@]}"
  do
    az network nic create \
        --resource-group CreateIntLBQS-rg \
        --name $vmnic \
        --vnet-name myVNet \
        --subnet myBackEndSubnet \
        --network-security-group myNSG
  done
```

### <a name="create-availability-set-for-virtual-machines"></a>Sanal makineler için kullanılabilirlik kümesi oluştur

Kullanılabilirlik kümesini [az VM AVAILABILITY-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create)ile oluşturun:

* **MyAvailabilitySet** adlı.
* **Createıntlbqs-RG** kaynak grubunda.
* Konum **eastus**.

```azurecli-interactive
  az vm availability-set create \
    --name myAvailabilitySet \
    --resource-group CreateIntLBQS-rg \
    --location eastus 
    
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create)ile sanal makineler oluşturun:

* Adlandırılmış **myVM1**, **myVM2** ve **myVM3**.
* **Createıntlbqs-RG** kaynak grubunda.
* **MyNicVM1**, **myNicVM2** ve **myNicVM3** ağ arabirimine iliştirilir.
* Sanal makine görüntüsü **win2019datacenter**.
* **MyAvailabilitySet** içinde.


```azurecli-interactive
  array=(1 2 3)
  for n in "${array[@]}"
  do
    az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM$n \
    --nics myNicVM$n \
    --image win2019datacenter \
    --admin-username azureuser \
    --availability-set myAvailabilitySet \
    --no-wait
  done
```
VM 'Lerin dağıtılması birkaç dakika sürebilir.

## <a name="create-basic-load-balancer"></a>Temel yük dengeleyici oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

  * Yük dengeleyicide gelen ağ trafiğini alan bir ön uç IP Havuzu.
  * Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği bir arka uç IP Havuzu.
  * Arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması.
  * Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-the-load-balancer-resource"></a>Yük dengeleyici kaynağı oluşturma

[Az Network lb Create](/cli/azure/network/lb#az-network-lb-create)komutuyla bir genel yük dengeleyici oluşturun:

* **Myloadbalancer** adlı.
* **Myön uç** adlı bir ön uç Havuzu.
* **Mybackendpool** adlı bir arka uç Havuzu.
* Sanal ağ **Myvnet** ile ilişkili.
* **Mybackendsubnet** arka uç ağıyla ilişkili.

```azurecli-interactive
  az network lb create \
    --resource-group CreateIntLBQS-rg \
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

[Az Network lb araştırması Create](/cli/azure/network/lb/probe#az-network-lb-probe-create)komutuyla bir sistem durumu araştırması oluşturun:

* Sanal makinelerin sistem durumunu izler.
* Adlandırılmış **Myhealtharaştırma**.
* Protokol **TCP**.
* İzleme **bağlantı noktası 80**.

```azurecli-interactive
  az network lb probe create \
    --resource-group CreateIntLBQS-rg \
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

[Az Network lb Rule Create](/cli/azure/network/lb/rule#az-network-lb-rule-create)ile bir yük dengeleyici kuralı oluşturun:

* Adlandırılmış **Myhttprule**
* Ön **uç** havuzundaki **80 numaralı bağlantı noktası** dinleniyor.
* **80 numaralı bağlantı noktasını** kullanan **mybackendpool** arka uç adres havuzuna yük dengeli ağ trafiği gönderiliyor. 
* Sistem durumu araştırması **Myhealtharaştırması** kullanılıyor.
* Protokol **TCP**.
* **15 dakikalık** boşta zaman aşımı.

```azurecli-interactive
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15 
```
### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzuna sanal makineler ekleme

[Az Network Nic IP-Config Address-Pool Add](/cli/azure/network/nic/ip-config/address-pool#az-network-nic-ip-config-address-pool-add)komutuyla sanal makineleri arka uç havuzuna ekleyin:

* Arka uç adres havuzunda **Mybackendpool**.
* **Createıntlbqs-RG** kaynak grubunda.
* **MyNicVM1**, **myNicVM2** ve **myNicVM3** ağ arabirimiyle ilişkili.
* Yük dengeleyici **Myloadbalancer** ile ilişkili.

```azurecli-interactive
  array=(VM1 VM2 VM3)
  for vm in "${array[@]}"
  do
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNic$vm \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
  done

```
---

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

### <a name="create-test-virtual-machine"></a>Test sanal makinesi oluştur

Ağ arabirimini [az ağ NIC Create](/cli/azure/network/nic#az-network-nic-create)ile oluşturun:

* **Mynictestvm** adında.
* **Createıntlbqs-RG** kaynak grubunda.
* Sanal ağ **\** sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.

```azurecli-interactive
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicTestVM \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
Sanal makineyi [az VM Create](/cli/azure/vm#az-vm-create)ile oluşturun:

* **Mytestvm** adında.
* **Createıntlbqs-RG** kaynak grubunda.
* **Mynictestvm** ağ arabirimine eklendi.
* Sanal makine görüntüsü **Win2019Datacenter**.

```azurecli-interactive
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myTestVM \
    --nics myNicTestVM \
    --image Win2019Datacenter \
    --admin-username azureuser \
    --no-wait
```
Sanal makinenin dağıtılması birkaç dakika sürebilir.

## <a name="install-iis"></a>IIS yükleme

Sanal makinelere IIS yüklemek ve varsayılan Web sitesini bilgisayar adına ayarlamak için [az VM Extension set](/cli/azure/vm/extension#az_vm_extension_set) kullanın.

```azurecli-interactive
  array=(myVM1 myVM2 myVM3)
    for vm in "${array[@]}"
    do
     az vm extension set \
       --publisher Microsoft.Compute \
       --version 1.8 \
       --name CustomScriptExtension \
       --vm-name $vm \
       --resource-group CreateIntLBQS-rg \
       --settings '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
  done

```

### <a name="test"></a>Test

1. Azure portalında [oturum açın](https://portal.azure.com).

2. **Genel bakış** ekranında yük DENGELEYICININ özel IP adresini bulun. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından **myloadbalancer**' ı seçin.

3. **Myloadbalancer** **genel** görünümünde, **özel IP adresi** ' nin yanındaki bir yere göz atın veya adresi kopyalayın.

4. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **createıntlbqs-RG** kaynak grubunda bulunan **mytestvm** ' yi seçin.

5. **Genel bakış** sayfasında **Bağlan** **' ı ve** sonra da ' yi seçin.

6. VM oluşturma sırasında girilen kullanıcı adını ve parolayı girin.

7. **Mytestvm**'de **Internet Explorer** 'ı açın.

8. Önceki adımdan, tarayıcının adres çubuğuna IP adresini girin. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Standart iç yük dengeleyici oluşturma" border="true":::
   
Yük dengeleyicinin trafiği üç VM 'ye dağıtmasını görmek için, her bir sanal makinenin IIS Web sunucusunun varsayılan sayfasını özelleştirebilir ve sonra Web tarayıcınızı istemci makinesinden yenileyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive
  az group delete \
    --name CreateIntLBQS-rg
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta:

* Standart veya genel yük dengeleyici oluşturdunuz
* Bağlı sanal makineler. 
* Yük dengeleyici trafik kuralı ve sistem durumu araştırması yapılandırıldı.
* Yük dengeleyici test edildi.

Azure Load Balancer hakkında daha fazla bilgi edinmek için devam edin:
> [!div class="nextstepaction"]
> [Azure Load Balancer nedir?](load-balancer-overview.md)