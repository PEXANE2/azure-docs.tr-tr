---
title: VM'ler için özel IP adreslerini yapılandırma - Azure CLI
description: Azure komut satırı arabirimini (CLI) kullanarak sanal makineler için özel IP adreslerini nasıl yapılandırıştırmayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78199487"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal bir makine için özel IP adreslerini yapılandırma


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> Aşağıdaki örnek Azure CLI komutları varolan basit bir ortam bekler. Komutları bu belgede görüntülenen komutları çalıştırmak istiyorsanız, önce [vnet oluşturmakta](quick-create-cli.md)açıklanan test ortamını oluşturun.

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>VM oluştururken statik özel IP adresi belirtin

*TestVNet* adlı bir VNet'in *FrontEnd* alt netinde *192.168.1.101*statik özel IP'si ile *DNS01* adında bir VM oluşturmak için aşağıdaki adımları tamamlayın:

1. Henüz yapmadıysanız, en son [Azure CLI'yi](/cli/azure/install-azure-cli) yükleyin ve yapılandırın ve [az giriş](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açın.

2. [Az ağı public-ip oluşturma](/cli/azure/network/public-ip) komutu yla VM için ortak bir IP oluşturun. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.

    > [!NOTE]
    > Ortamınıza bağlı olarak, bu ve sonraki adımlardaki bağımsız değişkenleriniz için farklı değerler kullanmak isteyebilirsiniz veya kullanmanız gerekebilir.

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

   * `--resource-group`: Genel IP oluşturmak için kaynak grubunun adı.
   * `--name`: Kamu IP adı.
   * `--location`: Genel IP'yi oluşturabilmek için Azure bölgesi.

3. Statik özel IP'ye sahip bir NIC oluşturmak için [az network nic oluşturma](/cli/azure/network/nic) komutunu çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır. 
   
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
    * `--vnet-name`: NIC oluşturmak için VNet adı.
    * `--subnet`: NIC oluşturmak için alt net adı.

4. Daha önce oluşturulan genel IP ve NIC'yi kullanarak VM oluşturmak için [azure vm oluşturma](/cli/azure/vm/nic) komutunu çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.
   
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
   
   Temel az vm dışındaki parametreler [parametreleri oluşturur.](/cli/azure/vm)

   * `--nics`: VM'nin bağlı olduğu NIC'nin adı.
   
Bir [Windows VM'ye birden fazla IP adresi atamak](virtual-network-multiple-ip-addresses-cli.md)gibi, gerekli olmadıkça, bir VM işletim sistemi içinde Azure sanal makinesine atanan özel IP'yi statik olarak atamamanız önerilir. İşletim sistemi içindeki özel IP adresini el ile ayarlarsanız, bunun Azure [ağ arabirimine](virtual-network-network-interface-addresses.md#change-ip-address-settings)atanan özel IP adresiyle aynı adres olduğundan emin olun veya sanal makineye bağlantınızı kaybedebilirsiniz. [Özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin.

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için statik özel IP adresi bilgilerini alma

*Özel IP alloc yöntemi* ve Özel IP *adresi*değerlerini gözlemlemek için aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Beklenen çıktı:

```json
"192.168.1.101"
```

Bu VM için NIC'nin belirli IP bilgilerini görüntülemek için NIC'yi özellikle sorgula:

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

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Statik özel IP adresini VM'den kaldırma

Azure Kaynak Yöneticisi dağıtımları için Azure CLI'daki bir NIC'den statik bir özel IP adresini kaldıramazsınız. Şunları yapmalısın:
- Dinamik BIR IP kullanan yeni bir NIC oluşturma
- VM nic yeni oluşturulan NIC yapmak ayarlayın. 

Önceki komutlarda kullanılan VM için NIC'yi değiştirmek için aşağıdaki adımları tamamlayın:

1. Yeni bir IP adresiyle dinamik IP ayırmayı kullanarak yeni bir NIC oluşturmak için **azure ağ nic oluşturma** komutunu çalıştırın. IP adresi belirtilmediğinden, ayırma yöntemi **Dinamik**.

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

2. VM tarafından kullanılan NIC'i değiştirmek için **azure vm kümesi** komutunu çalıştırın.
   
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
    > VM birden fazla NIC'ye sahip olacak kadar büyükse, eski NIC'yi silmek için **azure ağ nic silme** komutunu çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

[IP adresi ayarlarını](virtual-network-network-interface-addresses.md)yönetme hakkında bilgi edinin.
