---
title: Azure Key Vault güvenlik duvarlarını ve sanal ağları Yapılandırma-Azure Key Vault
description: Key Vault güvenlik duvarlarını ve sanal ağları yapılandırmak için adım adım yönergeler
services: key-vault
author: amitbapat
manager: rkarlin
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 870064406c86e9cef6a45dfbe47c61cdaa9ecab0
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77426318"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma

Bu makalede, anahtar kasanıza erişimi kısıtlamak için Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırmaya yönelik adım adım yönergeler sağlanmaktadır. [Key Vault için sanal ağ hizmet uç noktaları](key-vault-overview-vnet-service-endpoints.md) , belirtilen sanal ağa erişimi ve IPv4 (Internet Protokolü sürüm 4) adres aralıklarını kısıtlamanıza izin verir.

> [!IMPORTANT]
> Güvenlik duvarı kuralları etkin olduktan sonra, kullanıcılar, istekleri izin verilen sanal ağlardan veya IPv4 adres aralıklarından başlatıldığında yalnızca Key Vault [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) işlemleri gerçekleştirebilir. Bu, Azure portal Key Vault erişmek için de geçerlidir. Kullanıcılar Azure portal bir anahtar kasasına gözatabilse de, istemci makineleri izin verilenler listesinde yoksa anahtarları, parolaları veya sertifikaları listelemeyebilir. Bu, diğer Azure hizmetleri tarafından Key Vault seçiciyi de etkiler. Kullanıcılar, güvenlik duvarı kuralları istemci makinesini engelliyorsa, anahtar kasalarının listesini görebilirler, ancak liste anahtarlarını göremez.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portal kullanarak Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma:

1. Güvenli hale getirmek istediğiniz anahtar kasasını inceleyin.
2. **Ağ iletişimi**' ni seçin ve ardından **güvenlik duvarları ve sanal ağlar** sekmesini seçin.
3. **Erişime Izin ver**' ın altında **Seçili ağlar**' ı seçin.
4. Var olan sanal ağları güvenlik duvarları ve sanal ağ kurallarına eklemek için **+ var olan sanal ağları Ekle**' yi seçin.
5. Açılan yeni dikey pencerede, bu Anahtar Kasası 'na erişime izin vermek istediğiniz aboneliği, sanal ağları ve alt ağları seçin. Seçtiğiniz sanal ağlarda ve alt ağlarda hizmet uç noktaları etkinleştirilmemişse, hizmet uç noktalarını etkinleştirmek istediğinizi onaylayın ve **Etkinleştir**' i seçin. Etkili olması 15 dakika kadar sürebilir.
6. **IP ağları**altında [CIDR (sınıfsız etki alanları arası yönlendirme) gösteriminde](https://tools.ietf.org/html/rfc4632) veya tek tek IP adreslerinde IPv4 adresi aralıklarını yazarak IPv4 adres aralıkları ekleyin.
7. **Kaydet**’i seçin.

Ayrıca yeni sanal ağlar ve alt ağlar ekleyebilir ve ardından **+ Yeni sanal ağ ekle**' yi seçerek yeni oluşturulan sanal ağlar ve alt ağlar için hizmet uç noktalarını etkinleştirebilirsiniz. Ardından istemleri izleyin.

## <a name="use-the-azure-cli"></a>Azure CLI kullanma 

Azure CLı kullanarak Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma

1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 'yı yükleyip [oturum açın](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Kullanılabilir sanal ağ kurallarını listeleyin. Bu anahtar kasası için herhangi bir kural ayarlamadıysanız liste boş olur.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Mevcut bir sanal ağ ve alt ağ üzerinde Key Vault için bir hizmet uç noktası etkinleştirin.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Trafiğe izin verilecek bir IP adresi aralığı ekleyin.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Bu anahtar kasasının herhangi bir güvenilir hizmet tarafından erişilebilir olması gerekiyorsa `bypass` `AzureServices`olarak ayarlayın.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Varsayılan eylemi `Deny`olarak ayarlayarak ağ kurallarını açın.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell kullanarak Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma:

1. En son [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)yükleyip [oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Kullanılabilir sanal ağ kurallarını listeleyin. Bu anahtar kasası için herhangi bir kural ayarlanmamışsa liste boş olur.
   ```powershell
   (Get-AzKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Mevcut bir sanal ağ ve alt ağ üzerindeki Key Vault için hizmet uç noktasını etkinleştirin.
   ```powershell
   Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzVirtualNetwork
   ```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.
   ```powershell
   $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Trafiğe izin verilecek bir IP adresi aralığı ekleyin.
   ```powershell
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Bu anahtar kasasının herhangi bir güvenilir hizmet tarafından erişilebilir olması gerekiyorsa `bypass` `AzureServices`olarak ayarlayın.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Varsayılan eylemi `Deny`olarak ayarlayarak ağ kurallarını açın.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Başvurular

* Azure CLı komutları: [az keykasa Network-Rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell cmdlet 'leri: [Get-Azkeykasa,](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault) [Add-azkeyvaultnetworkrule](https://docs.microsoft.com/powershell/module/az.KeyVault/Add-azKeyVaultNetworkRule), [Remove-azkeyvaultnetworkrule](https://docs.microsoft.com/powershell/module/az.KeyVault/Remove-azKeyVaultNetworkRule), [Update-azkeyvaultnetworkruleset](https://docs.microsoft.com/powershell/module/az.KeyVault/Update-azKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault için sanal ağ hizmeti uç noktaları](key-vault-overview-vnet-service-endpoints.md)
* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)
