---
title: VM 'Ler için özel IP adreslerini Yapılandırma-Azure CLı
description: Azure komut satırı arabirimi 'ni (CLı) kullanarak sanal makineler için özel IP adreslerini yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: kumud
ms.openlocfilehash: f4643aae0b28861f4ddb99d8dace749e62f930b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78199487"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-cli"></a>Azure CLı kullanarak bir sanal makine için özel IP adresleri yapılandırma


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> Aşağıdaki örnek Azure CLı komutları var olan basit bir ortamı bekler. Komutları bu belgede görüntülendikleri gibi çalıştırmak istiyorsanız, önce [VNET oluşturma](quick-create-cli.md)bölümünde açıklanan test ortamını oluşturun.

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>VM oluştururken statik bir özel IP adresi belirtin

*192.168.1.101*statik özel IP 'Si Ile *Testvnet* adlı VNET 'in *ön uç* alt ağında *DNS01* adlı bir VM oluşturmak için aşağıdaki adımları izleyin:

1. Henüz yapmadıysanız, en son [Azure CLI](/cli/azure/install-azure-cli) 'yı yükleyip yapılandırın ve [az Login](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açın.

2. [Az Network public-IP Create](/cli/azure/network/public-ip) komutuyla VM IÇIN genel IP oluşturun. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.

    > [!NOTE]
    > Ortamınıza bağlı olarak bu ve sonraki adımlarda bağımsız değişkenlerinizin farklı değerlerini kullanmanız gerekebilir.

    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    Beklenen çıktı:

   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * `--resource-group`: Genel IP 'nin oluşturulacağı kaynak grubunun adı.
   * `--name`: Genel IP 'nin adı.
   * `--location`: Genel IP 'nin oluşturulacağı Azure bölgesi.

3. Statik özel IP 'si olan bir NIC oluşturmak için [az Network Nic Create](/cli/azure/network/nic) komutunu çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır. 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    Beklenen çıktı:
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    Parametreler:

    * `--private-ip-address`: NIC için statik özel IP adresi.
    * `--vnet-name`: NIC 'nin oluşturulacağı VNet 'in adı.
    * `--subnet`: NIC 'nin oluşturulacağı alt ağın adı.

4. Daha önce oluşturulan genel IP ve NIC 'yi kullanarak VM oluşturmak için [Azure VM oluşturma](/cli/azure/vm/nic) komutunu çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    Beklenen çıktı:
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   Temel [az VM Create](/cli/azure/vm) parametrelerinden farklı parametreler.

   * `--nics`: VM 'nin eklendiği NIC 'in adı.
   
[Bir WINDOWS VM 'ye birden çok IP adresi atarken](virtual-network-multiple-ip-addresses-cli.md), GEREKMEDIKÇE, sanal makinenin işletim sistemi içinde Azure sanal makinesine atanan özel IP 'yi statik olarak atamanız önerilir. İşletim sistemi içinde özel IP adresini el ile ayarlarsanız, Azure [ağ arabirimine](virtual-network-network-interface-addresses.md#change-ip-address-settings)ATANMıŞ özel IP adresi ile aynı adres olduğundan emin olun veya sanal makineyle olan bağlantıyı kaybedebilirsiniz. [Özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin.

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için statik özel IP adresi bilgilerini alma

*Özel IP ayırma yöntemi* ve *özel IP adresi*değerlerini gözlemlemek için aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Beklenen çıktı:

```json
"192.168.1.101"
```

Bu VM için NIC 'nin belirli IP bilgilerini göstermek için NIC 'i özellikle sorgulayın:

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

Çıktı aşağıdakine benzer:

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Statik özel IP adresini bir VM 'den kaldırma

Azure Resource Manager dağıtımları için Azure CLı içindeki bir NIC 'den statik bir özel IP adresi kaldıramazsınız. Şunları yapmanız gerekir:
- Dinamik IP kullanan yeni bir NIC oluşturma
- SANAL makinede NIC 'yi ayarlama yeni oluşturulan NIC 'yi yapın. 

Önceki komutlarda kullanılan VM 'nin NIC 'sini değiştirmek için aşağıdaki adımları izleyin:

1. Yeni bir IP adresi ile dinamik IP ayırmayı kullanarak yeni bir NIC oluşturmak için **Azure ağ NIC oluştur** komutunu çalıştırın. Hiçbir IP adresi belirtilmediğinden, ayırma yöntemi **dinamik**olur.

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    Beklenen çıktı:

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. VM tarafından kullanılan NIC 'ı değiştirmek için **Azure VM set** komutunu çalıştırın.
   
    ```azurecli
   az vm nic set --resource-group TestRG --vm-name DNS01 --nics TestNIC2
    ```

    Beklenen çıktı:
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > VM birden fazla NIC 'ye sahip olacak kadar büyükse, eski NIC 'yi silmek için **Azure ağ NIC silme** komutunu çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

[IP adresi ayarlarını](virtual-network-network-interface-addresses.md)yönetme hakkında bilgi edinin.
