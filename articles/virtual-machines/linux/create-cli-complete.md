---
title: Azure CLI ile bir Linux ortamı oluşturun
description: Azure CLI'yi kullanarak sıfırdan depolama, Linux VM, sanal ağ ve alt ağ, yük dengeleyicisi, NIC, genel IP ve ağ güvenlik grubu oluşturun.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: 7ee4674f5e7c04709256459c3417a1379a65aedc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969556"
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a>Azure CLI ile eksiksiz bir Linux sanal makinesi oluşturun
Azure'da hızlı bir sanal makine (VM) oluşturmak için, gerekli destek kaynaklarını oluşturmak için varsayılan değerleri kullanan tek bir Azure CLI komutunu kullanabilirsiniz. Sanal ağ, genel IP adresi ve ağ güvenliği grubu kuralları gibi kaynaklar otomatik olarak oluşturulur. Üretim kullanımında ortamınız üzerinde daha fazla denetim sağlamak için, bu kaynakları önceden oluşturabilir ve vm'lerinizi bunlara ekleyebilirsiniz. Bu makalede, bir VM ve destekleyen kaynakların her biri tek tek oluşturmak için nasıl size rehberlik eder.

En son [Azure CLI'sini](/cli/azure/install-az-cli2) yüklediğinizden ve [az giriş](/cli/azure/reference-index)le bir Azure hesabına giriş yaptığınızdan emin olun.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myVnet*ve *myVM'i*içerir.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sanal bir makineden önce ve sanal ağ kaynaklarını destekleyen bir kaynak grubu oluşturulmalıdır. az grubu oluşturma ile kaynak grubu [oluşturun.](/cli/azure/group) Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Varsayılan olarak, Azure CLI komutlarının çıktısı JSON 'da (JavaScript Object Notation) bulunur. Varsayılan çıktıyı bir liste veya tabloya değiştirmek için, örneğin, [az yapılandırma --çıktı](/cli/azure/reference-index)kullanın. Ayrıca, çıktı `--output` biçiminde bir kez değişiklik için herhangi bir komuta ekleyebilirsiniz. Aşağıdaki örnek, komuttan JSON `az group create` çıktısını gösterir:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma
Ardından Azure'da sanal bir ağ ve Sanal M'lerinizi oluşturabileceğiniz bir alt ağ oluşturursunuz. *192.168.0.0/16* adresi öneki ile *myVnet* adlı bir sanal ağ oluşturmak için [az network vnet oluşturun](/cli/azure/network/vnet) kullanın. Ayrıca *192.168.1.0/24*adres öneki ile *mySubnet* adlı bir alt ağ ekleyin:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

Çıktı, alt ağın sanal ağ içinde mantıksal olarak oluşturulduğunu gösterir:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Şimdi [az ağ public-ip oluşturmak](/cli/azure/network/public-ip)ile ortak bir IP adresi oluşturalım. Bu genel IP adresi, Internet'ten VM'lerinize bağlanmanızı sağlar. Varsayılan adres dinamik olduğundan, `--domain-name-label` parametreyle birlikte adlandırılmış bir DNS girişi oluşturun. Aşağıdaki örnek, *mypublicdns*DNS adı ile *myPublicIP* adlı bir ortak IP oluşturur. DNS adı benzersiz olduğundan, kendi benzersiz DNS adınızı sağlayın:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Çıktı:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma
Sanal M'lerinize giriş ve çıkış trafiğin akışını denetlemek için, sanal bir NIC veya alt ağa bir ağ güvenlik grubu uygularsınız. Aşağıdaki örnek, *myNetworkSecurityGroup*adlı bir ağ güvenlik grubu oluşturmak için [az ağ nsg oluşturur:](/cli/azure/network/nsg)

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Belirli bir trafiğe izin veren veya reddeden kurallar tanımlarsınız. Bağlantı noktası 22'de gelen bağlantılara izin vermek için (SSH erişimini etkinleştirmek için), [az ağ nsg kuralı oluşturarak](/cli/azure/network/nsg/rule)gelen bir kural oluşturun. Aşağıdaki örnek *myNetworkSecurityGroupRuleSSH*adlı bir kural oluşturur:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

Bağlantı noktası 80'de gelen bağlantılara izin vermek için (web trafiği için), başka bir ağ güvenlik grubu kuralı ekleyin. Aşağıdaki örnek *myNetworkSecurityGroupRuleHTTP*adlı bir kural oluşturur:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

[Az network nsg show](/cli/azure/network/nsg)ile ağ güvenlik grubunu ve kurallarını inceleyin:

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Çıktı:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou",
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to Internet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a>Sanal bir NIC oluşturma
Sanal ağ arabirim kartları (NIC'ler) programlanabilir, çünkü bunların kullanımına kurallar uygulayabilirsiniz. [VM boyutuna](sizes.md)bağlı olarak, bir VM'ye birden çok sanal NIC ekleyebilirsiniz. Aşağıdaki [az network nic oluşturma](/cli/azure/network/nic) komutunda, *myNic* adında bir NIC oluşturur ve ağ güvenlik grubunuzla ilişkilendirin. Genel IP adresi *myPublicIP* de sanal NIC ile ilişkilidir.

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Çıktı:

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Kullanılabilirlik kümeleri, VM'lerinizin hata etki alanları arasında yayılmasına ve etki alanlarını güncelleştirmeye yardımcı olur. Şu anda yalnızca bir VM oluştursanız da, gelecekte daha kolay genişlemek için kullanılabilirlik kümelerini kullanmak en iyi uygulamadır. 

Hata etki alanları, ortak bir güç kaynağını ve ağ anahtarını paylaşan sanal makinelerin gruplandırmalarını tanımlar. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılan sanal makineler en fazla üç hata etki alanına ayrılır. Bu hata etki alanlarından birinde bir donanım sorunu, uygulamanızı çalıştıran her VM'yi etkilemez.

Güncelleştirme etki alanları, aynı anda yeniden başlatılabilen sanal makine gruplarını ve altta yatan fiziksel donanım gruplarını gösterir. Planlı bakım sırasında, güncelleştirme etki alanlarının yeniden başlatıldırılma sırası sıralı olmayabilir, ancak aynı anda yalnızca bir güncelleştirme etki alanı yeniden başlatılır.

Azure, VM'leri otomatik olarak hataya dağıtır ve etki alanlarını kullanılabilirlik kümesine yerleştirirken günceller. Daha fazla bilgi için [VM'lerin kullanılabilirliğini yönetme](manage-availability.md)bölümüne bakın.

[Az vm kullanılabilirlik seti oluşturmak](/cli/azure/vm/availability-set)ile VM için bir kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Çıktı notları etki alanlarını hata ve güncelleştirme etki alanları:

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-a-vm"></a>VM oluşturma
Internet'e erişilebilen VM'leri desteklemek için ağ kaynaklarını oluşturdunuz. Şimdi bir VM oluşturun ve bir SSH tuşu ile güvenli. Bu örnekte, en son LTS'ye dayalı bir Ubuntu VM oluşturalım. [Azure VM görüntülerini bulurken](cli-ps-findimage.md)açıklandığı gibi [az vm resim listesi](/cli/azure/vm/image)ile ek görüntüler bulabilirsiniz.

Kimlik doğrulaması için kullanılacak bir SSH anahtarı belirtin. Bir SSH ortak anahtar çiftiniz yoksa, [bunları oluşturabilir](mac-create-ssh-keys.md) `--generate-ssh-keys` veya sizin için oluşturmak için parametreyi kullanabilirsiniz. Zaten bir anahtar çiftinvarsa, bu parametre `~/.ssh`varolan anahtarları .

[Az vm create](/cli/azure/vm) komutu ile birlikte tüm kaynakları ve bilgileri getirerek VM oluşturun. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Ortak IP adresini oluşturduğunuzda sağladığınız DNS girişi ile VM'inize SSH. VM'nizi oluştururken bu `fqdn` çıktıda gösterilir:

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Çıktı:

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    https://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

NGINX'i yükleyebilir ve VM'ye giden trafik akışını görebilirsiniz. NGINX'i aşağıdaki gibi yükleyin:

```bash
sudo apt-get install -y nginx
```

Varsayılan NGINX sitesini iş başında görmek için web tarayıcınızı açın ve FQDN'nizi girin:

![VM'nizde varsayılan NGINX sitesi](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Şablon olarak dışa aktarma
Şimdi aynı parametrelere sahip ek bir geliştirme ortamı veya onunla eşleşen bir üretim ortamı oluşturmak isterseniz ne olur? Kaynak Yöneticisi, ortamınız için tüm parametreleri tanımlayan JSON şablonlarını kullanır. Bu JSON şablonuna başvurarak tüm ortamları oluşturursunuz. Sizin için JSON şablonu oluşturmak için [JSON şablonlarını el ile oluşturabilir](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya varolan bir ortamı dışa aktarabilirsiniz. Kaynak grubunuzu dışa aktarmak için [az grubu dışa aktarmayı](/cli/azure/group) aşağıdaki gibi kullanın:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Bu komut, `myResourceGroup.json` geçerli çalışma dizininizdeki dosyayı oluşturur. Bu şablondan bir ortam oluşturduğunuzda, tüm kaynak adları için istenir. Bu adları `--include-parameter-default-value` `az group export` komuta parametre ekleyerek şablon dosyanızda doldurabilirsiniz. Kaynak adlarını belirtmek için JSON şablonunuzu edin veya kaynak adlarını belirten [bir parameters.json dosyası oluşturun.](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Şablonunuzdan bir ortam oluşturmak için az grup dağıtım ını aşağıdaki gibi [kullanın:](/cli/azure/group/deployment)

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

[Şablonlardan nasıl dağıtılanın](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)çatladığınız hakkında daha fazla bilgi almak isteyebilirsiniz. Ortamları aşamalı olarak nasıl güncelleştirecek, parametreler dosyasını kullanma ve şablonlara tek bir depolama konumundan nasıl erişilir hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Artık birden çok ağ bileşeni ve VM ile çalışmaya başlamaya hazırsınız. Burada tanıtılan temel bileşenleri kullanarak uygulamanızı oluşturmak için bu örnek ortamını kullanabilirsiniz.
