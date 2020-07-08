---
title: Arka uç havuzu yönetimi
description: Load Balancer arka uç havuzunu yapılandırmaya yönelik kılavuz
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: overview
ms.date: 07/06/2020
ms.author: errobin
ms.openlocfilehash: 6d9700e134a9e3d6c53524d15c8b3503cf5773c7
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050192"
---
# <a name="backend-pool-management"></a>Arka uç havuzu yönetimi
Arka uç havuzu, belirli bir yük dengeleme kuralı için trafik sunacak işlem kaynağı grubunu tanımlayan Load Balancer temel bir bileşenidir. Bir arka uç havuzunu düzgün şekilde yapılandırarak, trafiği sunacak uygun bir makine grubu tanımlamış olursunuz. Bir arka uç havuzunu, ağ arabirimi kartı (NIC) ve bir bileşim IP adresi ve sanal ağ (VNET) kaynak KIMLIĞI ile yapılandırmanın iki yolu vardır. 

Sanal makineleri ve sanal makine ölçek kümelerini kapsayan Çoğu senaryoda, bu yöntem, kaynağınız ile arka uç havuzu arasında en doğrudan bağlantıyı oluşturdığından, arka uç havuzunuzun NIC tarafından yapılandırılması önerilir. Bir NIC 'e sahip olmayan kapsayıcılar ve Kubernetes pods içeren senaryolar ve arka uç kaynakları için bir IP adresi aralığının ön ayırması için, arka uç havuzunuzu IP adresine ve VNET KIMLIĞI birleşimine göre yapılandırabilirsiniz.

Portal aracılığıyla NIC veya IP adresi ve VNET KIMLIĞI ile yapılandırırken, Kullanıcı arabirimi her adımda size kılavuzluk eder ve tüm yapılandırma güncelleştirmeleri arka uçta işlenir. Bu makalenin yapılandırma bölümleri, arka uç havuzlarının her bir yapılandırma seçeneği için nasıl yapılandırıldığı hakkında bilgi vermek için Azure PowerShell, CLı, REST API ve ARM şablonlarına odaklanacaktır.

## <a name="configuring-backend-pool-by-nic"></a>NIC tarafından arka uç havuzunu yapılandırma
Bir arka uç havuzunu NIC ile yapılandırırken, arka uç havuzunun Load Balancer işleminin bir parçası olarak oluşturulduğunu ve üyelerin, ağ arabirimi işlemi sırasında ağ arabiriminin IP Yapılandırması özelliğinin bir parçası olarak arka uç havuzuna eklendiğini aklınızda bulundurmanız önemlidir. Aşağıdaki örnekler, bu iş akışını ve ilişkiyi vurgulamak için arka uç havuzunun oluştur ve doldur işlemlerine odaklanılmıştır.

  >[!NOTE] 
  >Ağ arabirimi aracılığıyla yapılandırılan arka uç havuzlarının, arka uç havuzundaki bir işlemin parçası olarak güncelleştirileceğini unutmayın. Arka uç kaynaklarının herhangi bir eklenmesi veya silinmesi, kaynağın ağ arabiriminde gerçekleşmelidir.

### <a name="powershell"></a>PowerShell
Yeni bir arka uç havuzu oluşturun: 
```powershell
$backendPool = New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup   -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName  
```

Yeni bir ağ arabirimi oluşturun ve arka uç havuzuna ekleyin:
```powershell
$nic = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic' -LoadBalancerBackendAddressPool $bepool -Subnet $vnet.Subnets[0]
```

Bu ağ arabiriminin arka uç havuzuna eklendiğini onaylamak için Load Balancer arka uç havuzu bilgilerini alın:
```powershell
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup  -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName -BackendAddressPool  $bePool  
```

Yeni bir sanal makine oluşturun ve ağ arabirimini, arka uç havuzuna yerleştirmek üzere bağlayın:
```powershell
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM1' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM1.Id
 
# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $rgName -Zone 1 -Location $location -VM $vmConfig
```


  
### <a name="cli"></a>CLI
Arka uç havuzunu oluşturma:
```bash
az network lb address-pool create --resourceGroup myResourceGroup --lb-name myLB --name myBackendPool 
```

Yeni bir ağ arabirimi oluşturun ve arka uç havuzuna ekleyin:
```bash
az network nic create --resource-group myResourceGroup --name myNic --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup --lb-name myLB --lb-address-pools myBackEndPool
```

IP adresinin doğru şekilde eklendiğini onaylamak için arka uç havuzunu alın:
```bash
az network lb address-pool show -g MyResourceGroup --lb-name MyLb -n MyBackendPool
```

Yeni bir sanal makine oluşturun ve ağ arabirimini, arka uç havuzuna yerleştirmek üzere bağlayın:
```bash
az vm create --resource-group myResourceGroup --name myVM --nics myNic --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
```

### <a name="rest-api"></a>REST API
Arka uç havuzunu oluşturma:
```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/loadBalancers/{load-balancer-name}/backendAddressPools/{backend-pool-name}?api-version=2020-05-01
```

Ağ arabirimi oluşturun ve ağ arabiriminin IP yapılandırması özelliği aracılığıyla oluşturduğunuz arka uç havuzuna ekleyin:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/networkInterfaces/{nic-name}?api-version=2020-05-01
```

JSON Istek gövdesi:
```json
{
  "properties": {
    "enableAcceleratedNetworking": true,
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "subnet": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}/subnets/{subnet-name}"
          },
          "loadBalancerBackendAddressPools": {
                                    "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/loadBalancers/{load-balancer-name}/backendAddressPools/{backend-pool-name}"
          }
        }
      }
    ]
  },
  "location": "eastus"
}
```

Bu ağ arabiriminin arka uç havuzuna eklendiğini onaylamak için Load Balancer arka uç havuzu bilgilerini alın:

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name/providers/Microsoft.Network/loadBalancers/{load-balancer-name/backendAddressPools/{backend-pool-name}?api-version=2020-05-01
```

Bir VM oluşturun ve arka uç havuzuna başvuran NIC 'yi iliştirin:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Compute/virtualMachines/{vm-name}?api-version=2019-12-01
```

JSON Istek gövdesi:
```JSON
{
  "location": "easttus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "adminPassword": "{your-password}"
    }
  }
}
```

### <a name="arm-template"></a>ARM Şablonu
Bir Load Balancer ve sanal makine dağıtmak ve ağ arabirimi aracılığıyla sanal makineleri arka uç havuzuna eklemek için bu [hızlı başlangıç ARM şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-load-balancer-standard-create/) izleyin.

## <a name="configuring-backend-pool-by-ip-address-and-virtual-network"></a>IP adresi ve sanal ağ ile arka uç havuzunu yapılandırma
Kapsayıcı kaynaklarına yük dengelemekte veya bir IP adresi aralığına sahip bir arka uç havuzu önceden dolduruluyorsanız, ağ arabirimine sahip olup olmadığına bakılmaksızın geçerli bir kaynağa yönlendirmek için IP adresi ve sanal ağ üzerinden yararlanabilirsiniz. IP adresi ve VNET aracılığıyla yapılandırırken, tüm arka uç havuzu yönetimi, aşağıdaki örneklerde vurgulanan şekilde doğrudan arka uç havuzu nesnesi üzerinde yapılır.

  >[!IMPORTANT] 
  >Bu özellik şu anda önizleme aşamasındadır ve aşağıdaki sınırlamalara sahiptir:
  >* 100 IP adresi sınırı eklendi
  >* Arka uç kaynakları, Load Balancer ile aynı sanal ağda olmalıdır
  >* Bu özellik şu anda portal UX sürümünde desteklenmemektedir
  >* Bu yalnızca standart yük dengeleyiciler için kullanılabilir
  
### <a name="powershell"></a>PowerShell
Yeni arka uç havuzu oluştur: 
```powershell
$backendPool = New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup   -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPooName  
```

Bu arka uç havuzunu mevcut VNET 'ten yeni bir IP ile güncelleştirin:  
```powershell
$virtualNetwork = Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup 
 
$ip1 = New-AzLoadBalancerBackendAddressConfig -IpAddress "10.0.0.5" -Name "TestVNetRef" -VirtualNetwork $virtualNetwork  
 
$backendPool.LoadBalancerBackendAddresses.Add($ip1) 

Set-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup  -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName -BackendAddressPool  $backendPool  
```

Arka uç adreslerinin arka uç havuzuna eklendiğini doğrulamak üzere Load Balancer için arka uç havuzu bilgilerini alın:
```powershell
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup  -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName -BackendAddressPool  $backendPool  
```
IP adresini arka uç adreslerinden birine ayarlayarak bir ağ arabirimi oluşturun ve arka uç havuzuna ekleyin:
```
$nic = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic' -PrivateIpAddress 10.0.0.4 -Subnet $vnet.Subnets[0]
```

Bir VM oluşturun ve NIC 'yi arka uç havuzunda bir IP adresi ile ekleyin:
```powershell
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nic.Id
 
# Create a virtual machine using the configuration
$vm = New-AzVM -ResourceGroupName $rgName -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>CLI
CLı kullanarak, arka uç havuzunu komut satırı parametreleriyle ya da bir JSON yapılandırma dosyası aracılığıyla doldurabilirsiniz. 

Komut satırı parametreleri aracılığıyla arka uç havuzunu oluşturun ve doldurun:
```bash
az network lb address-pool create --lb-name myLB --name myBackendPool --vnet {VNET resource ID} --backend-address name=addr1 ip-address=10.0.0.4 --backend-address name=addr2 ip-address=10.0.0.5
```

Arka uç havuzunu JSON yapılandırma dosyası aracılığıyla oluşturun ve doldurun:
```bash
az network lb address-pool create --lb-name myLB --name myBackendPool --vnet {VNET resource ID} --backend-address-config-file @config_file.json
```

JSON yapılandırma dosyası:
```JSON
        [
          {
            "name": "address1",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.4"
          },
          {
            "name": "address2",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.5"
          }
        ]
```

Arka uç adreslerinin arka uç havuzuna eklendiğini doğrulamak üzere Load Balancer için arka uç havuzu bilgilerini alın:
```bash
az network lb address-pool show -g MyResourceGroup --lb-name MyLb -n MyBackendPool
```

IP adresini arka uç adreslerinden birine ayarlayarak bir ağ arabirimi oluşturun ve arka uç havuzuna ekleyin:
```bash
az network nic create \
  --resource-group myResourceGroup \
  --name myNic \
  --vnet-name myVnet \
  --subnet mySubnet \
  --network-security-group myNetworkSecurityGroup \
  --lb-name myLB \
  --private-ip-address 10.0.0.4
```

Bir VM oluşturun ve NIC 'yi arka uç havuzunda bir IP adresi ile ekleyin:
```bash
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --nics myNic \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

### <a name="rest-api"></a>REST API


Bir arka uç havuzu isteği ile arka uç havuzu oluşturun ve arka uç adreslerini tanımlayın. PUT isteğinin JSON gövdesinde adres adı, IP adresi ve sanal ağ KIMLIĞI aracılığıyla eklemek istediğiniz arka uç adreslerini yapılandırın:

```
PUT https://management.azure.com/subscriptions/subid/resourceGroups/testrg/providers/Microsoft.Network/loadBalancers/lb/backendAddressPools/backend?api-version=2020-05-01
```

JSON Istek gövdesi:
```JSON
{
  "properties": {
    "loadBalancerBackendAddresses": [
      {
        "name": "address1",
        "properties": {
          "virtualNetwork": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}"
          },
          "ipAddress": "10.0.0.4"
        }
      },
      {
        "name": "address2",
        "properties": {
          "virtualNetwork": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}"
          },
          "ipAddress": "10.0.0.5"
        }
      }
    ]
  }
}
```

Arka uç adreslerinin arka uç havuzuna eklendiğini doğrulamak üzere Load Balancer için arka uç havuzu bilgilerini alın:
```
GET https://management.azure.com/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/loadBalancers/{load-balancer-name}/backendAddressPools/{backend-pool-name}?api-version=2020-05-01
```

IP adresini arka uç adreslerinden birine ayarlayarak bir ağ arabirimi oluşturun ve arka uç havuzuna ekleyin:
```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/networkInterfaces/{nic-name}?api-version=2020-05-01
```

JSON Istek gövdesi:
```JSON
{
  "properties": {
    "enableAcceleratedNetworking": true,
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "privateIPAddress": "10.0.0.4",
          "subnet": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}/subnets/{subnet-name}"
          }
        }
      }
    ]
  },
  "location": "eastus"
}
```

Bir VM oluşturun ve NIC 'yi arka uç havuzunda bir IP adresi ile ekleyin:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Compute/virtualMachines/{vm-name}?api-version=2019-12-01
```

JSON Istek gövdesi:
```JSON
{
  "location": "eastus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "adminPassword": "{your-password}"
    }
  }
}
```

### <a name="arm-template"></a>ARM Şablonu
Load Balancer, arka uç havuzunu oluşturun ve arka uç havuzunu arka uç adresleriyle doldurun:
```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "loadBalancers_myLB_location": {
            "type": "SecureString"
        },
        "loadBalancers_myLB_location_1": {
            "type": "SecureString"
        },
        "backendAddressPools_myBackendPool_location": {
            "type": "SecureString"
        },
        "backendAddressPools_myBackendPool_location_1": {
            "type": "SecureString"
        },
        "loadBalancers_myLB_name": {
            "defaultValue": "myLB",
            "type": "String"
        },
        "virtualNetworks_myVNET_externalid": {
            "defaultValue": "/subscriptions/6bb4a28a-db84-4e8a-b1dc-fabf7bd9f0b2/resourceGroups/ErRobin4/providers/Microsoft.Network/virtualNetworks/myVNET",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2020-04-01",
            "name": "[parameters('loadBalancers_myLB_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LoadBalancerFrontEnd",
                        "properties": {
                            "privateIPAddress": "10.0.0.7",
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[concat(parameters('virtualNetworks_myVNET_externalid'), '/subnets/Subnet-1')]"
                            },
                            "privateIPAddressVersion": "IPv4"
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "myBackendPool",
                        "properties": {
                            "loadBalancerBackendAddresses": [
                                {
                                    "name": "addr1",
                                    "properties": {
                                        "ipAddress": "10.0.0.4",
                                        "virtualNetwork": {
                                            "location": "[parameters('loadBalancers_myLB_location')]"
                                        }
                                    }
                                },
                                {
                                    "name": "addr2",
                                    "properties": {
                                        "ipAddress": "10.0.0.5",
                                        "virtualNetwork": {
                                            "location": "[parameters('loadBalancers_myLB_location_1')]"
                                        }
                                    }
                                }
                            ]
                        }
                    }
                ],
                "loadBalancingRules": [],
                "probes": [],
                "inboundNatRules": [],
                "outboundRules": [],
                "inboundNatPools": []
            }
        },
        {
            "type": "Microsoft.Network/loadBalancers/backendAddressPools",
            "apiVersion": "2020-04-01",
            "name": "[concat(parameters('loadBalancers_myLB_name'), '/myBackendPool')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLB_name'))]"
            ],
            "properties": {
                "loadBalancerBackendAddresses": [
                    {
                        "name": "addr1",
                        "properties": {
                            "ipAddress": "10.0.0.4",
                            "virtualNetwork": {
                                "location": "[parameters('backendAddressPools_myBackendPool_location')]"
                            }
                        }
                    },
                    {
                        "name": "addr2",
                        "properties": {
                            "ipAddress": "10.0.0.5",
                            "virtualNetwork": {
                                "location": "[parameters('backendAddressPools_myBackendPool_location_1')]"
                            }
                        }
                    }
                ]
            }
        }
    ]
}
```

Bir sanal makine ve bağlı ağ arabirimi oluşturun. Ağ arabiriminin IP adresini arka uç adreslerinden birine ayarlayın:
```
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "String",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "storageAccountDomain": {
      "type": "String",
      "metadata": {
        "description": "The domain of the storage account to be created."
      }
    },
    "adminUsername": {
      "type": "String",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "SecureString",
      "metadata": {
        "description": "Admin password"
      }
    },
    "vmName": {
      "defaultValue": "myVM",
      "type": "String",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "String",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "defaultValue": "WindowsServer",
      "type": "String",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "defaultValue": "2012-R2-Datacenter",
      "type": "String",
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "defaultValue": "myLB",
      "type": "String",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicName": {
      "defaultValue": "nic",
      "type": "String",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "privateIpAddress": {
      "defaultValue": "10.0.0.5",
      "type": "String",
      "metadata": {
        "description": "Private IP Address of the VM"
      }
    },
    "vnetName": {
      "defaultValue": "myVNET",
      "type": "String",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "defaultValue": "Standard_A1",
      "type": "String",
      "metadata": {
        "description": "Size of the VM"
      }
    },
    "storageLocation": {
      "type": "String",
      "metadata": {
        "description": "Location of the Storage Account."
      }
    },
    "location": {
      "type": "String",
      "metadata": {
        "description": "Location to deploy all the resources in."
      }
    }
  },
  "variables": {
    "networkSecurityGroupName": "networkSecurityGroup1",
    "storageAccountType": "Standard_LRS",
    "subnetName": "Subnet-1",
    "publicIPAddressType": "Static",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2015-05-01-preview",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('storageLocation')]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "apiVersion": "2016-03-30",
      "name": "[variables('networkSecurityGroupName')]",
      "location": "[parameters('location')]",
      "properties": {
        "securityRules": []
      }
    },
    {
      "type": "Microsoft.Network/networkInterfaces",
      "apiVersion": "2015-05-01-preview",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Static",
              "privateIpAddress": "[parameters('privateIpAddress')]",
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2015-05-01-preview",
      "name": "[parameters('vmName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
        "[parameters('nicName')]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[parameters('vmSize')]"
        },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[parameters('imagePublisher')]",
            "offer": "[parameters('imageOffer')]",
            "sku": "[parameters('imageSKU')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',parameters('storageAccountName'),'.blob.',parameters('storageAccountDomain'),'/vhds/','osdisk', '.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
            }
          ]
        }
      }
    }
  ]
}
```
