---
title: Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma - Azure Key Vault
description: Key Vault güvenlik duvarlarını ve sanal ağları yapılandırmak için adım adım talimatlar
services: key-vault
author: amitbapat
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 8f483f8d383da1f9ba05eb172db185bec9406c7e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78195167"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma

Bu makalede, azure key vault güvenlik duvarlarını ve sanal ağları anahtar kasanıza erişimi kısıtlamak için yapılandırmak için adım adım yönergeler sağlanmaktadır. [Key Vault için sanal ağ hizmeti uç noktaları,](key-vault-overview-vnet-service-endpoints.md) belirli bir sanal ağa ve IPv4 (internet protokolü sürüm 4) adres aralıkları kümesine erişimi kısıtlamanızı sağlar.

> [!IMPORTANT]
> Güvenlik duvarı kuralları yürürlüğe girdiğinde, kullanıcılar Yalnızca istekleri izin verilen sanal ağlardan veya IPv4 adres aralıklarından geldiğinde Key Vault [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) işlemlerini gerçekleştirebilir. Bu, Azure portalından Key Vault'a erişmek için de geçerlidir. Kullanıcılar Azure portalından önemli bir kasaya göz atabilseler de, istemci makineleri izin verilenler listesinde değilse anahtarları, sırları veya sertifikaları listelemeyebilir. Bu, diğer Azure hizmetleri tarafından Key Vault Picker'ı da etkiler. Güvenlik duvarı kuralları istemci makinelerini engelliyorsa, kullanıcılar anahtar kasalarının listesini görebilir, ancak liste anahtarlarını göremeyebilir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalını kullanarak Key Vault güvenlik duvarlarını ve sanal ağları şu şekilde yapılandırabilirsiniz:

1. Güvenliğini sağlamak istediğiniz anahtar kasasına göz atın.
2. **Ağ'ı**seçin ve ardından **Güvenlik Duvarları ve sanal ağlar** sekmesini seçin.
3. **Erişime izin ver**altında, **Seçili ağları**seçin.
4. Varolan sanal ağları güvenlik duvarlarına ve sanal ağ kurallarına eklemek için varolan sanal ağları + **Ekle'yi**seçin.
5. Açılan yeni bıçakta, bu anahtar kasasına erişime izin vermek istediğiniz aboneliği, sanal ağları ve alt ağları seçin. Seçtiğiniz sanal ağlar ve alt ağlarda hizmet bitiş noktaları etkin değilse, hizmet uç noktalarını etkinleştirmek istediğinizi onaylayın ve **Etkinleştir'i**seçin. Etkisini görmek 15 dakika kadar sürebilir.
6. **IP Ağları**altında, [CIDR (Classless Etki Alanları Yönlendirme) notasyonuna](https://tools.ietf.org/html/rfc4632) veya tek tek IP adreslerine IPv4 adres aralıkları yazarak IPv4 adres aralıkları ekleyin.
7. **Kaydet'i**seçin.

Ayrıca yeni sanal ağlar ve alt ağlar ekleyebilir ve ardından yeni oluşturulan sanal ağlar ve alt ağlar için hizmet bitiş noktalarını etkinleştirerek **+ Yeni sanal ağ ekle**seçeneğini belirleyebilirsiniz. Ardından istemleri izleyin.

## <a name="use-the-azure-cli"></a>Azure CLI kullanma 

Azure CLI'yi kullanarak Key Vault güvenlik duvarlarını ve sanal ağları şu şekilde yapılandırabilirsiniz:

1. [Azure CLI'yi yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [oturum açın.](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)

2. Kullanılabilir sanal ağ kurallarını listele. Bu anahtar kasası için herhangi bir kural belirlemediyseniz, liste boş olacaktır.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Varolan bir sanal ağ ve alt ağda Key Vault için bir hizmet bitiş noktasını etkinleştirin.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Sanal ağ ve alt ağ için ağ kuralı ekleyin.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Trafiğe izin vermek için ip adresi aralığı ekleyin.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Bu anahtar kasasına güvenilen hizmetler tarafından `bypass` `AzureServices`erişilen sayılsa, buna göre ayarlanır.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Varsayılan eylemi ' ye `Deny`ayarlayarak ağ kurallarını açın.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i kullanarak Key Vault güvenlik duvarlarını ve sanal ağları şu şekilde yapılandırabilirsiniz:

1. En son [Azure PowerShell'i](https://docs.microsoft.com/powershell/azure/install-az-ps)yükleyin ve [oturum açın.](https://docs.microsoft.com/powershell/azure/authenticate-azureps)

2. Kullanılabilir sanal ağ kurallarını listele. Bu anahtar kasası için herhangi bir kural belirlemediyseniz, liste boş olacaktır.
   ```powershell
   (Get-AzKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Key Vault için hizmet bitiş noktasını varolan bir sanal ağ ve alt ağda etkinleştirin.
   ```powershell
   Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzVirtualNetwork
   ```

4. Sanal ağ ve alt ağ için ağ kuralı ekleyin.
   ```powershell
   $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Trafiğe izin vermek için ip adresi aralığı ekleyin.
   ```powershell
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Bu anahtar kasasına güvenilen hizmetler tarafından `bypass` `AzureServices`erişilen sayılsa, buna göre ayarlanır.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Varsayılan eylemi ' ye `Deny`ayarlayarak ağ kurallarını açın.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Başvurular

* Azure CLI komutları: [az keyvault ağ kuralı](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell cmdlets: [Get-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault), [Ekle-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Add-azKeyVaultNetworkRule), [Kaldır-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Remove-azKeyVaultNetworkRule), [Güncelleme-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Update-azKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault için sanal ağ hizmeti uç noktaları](key-vault-overview-vnet-service-endpoints.md)
* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)
