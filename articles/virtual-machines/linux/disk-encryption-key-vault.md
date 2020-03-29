---
title: Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma
description: Bu makale, Azure Disk Şifrelemesi ile kullanılmak üzere önemli bir kasa oluşturmak ve yapılandırmak için adımlar sağlar
ms.service: virtual-machines-linux
ms.topic: article
author: msmbaldwin
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: a818d9fe9707d1789fbe8e77489fc380fd2c92dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970636"
---
# <a name="creating-and-configuring-a-key-vault-for-azure-disk-encryption"></a>Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını kontrol etmek ve yönetmek için Azure Key Vault'u kullanır.  Anahtar kasaları hakkında daha fazla bilgi için Azure Key Vault ve [Secure anahtar kasanızı](../../key-vault/key-vault-secure-your-key-vault.md) [ile işe başlayın.](../../key-vault/key-vault-get-started.md) 

> [!WARNING]
> - VM'yi şifrelemek için Azure AD ile Azure Disk Şifreleme'yi daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz. Ayrıntılar [için Azure AD (önceki sürüm) ile Azure Disk Şifreleme için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault-aad.md) yada bkz.

Azure Disk Şifrelemesi ile kullanılmak üzere anahtar kasa oluşturma ve yapılandırma üç adım içerir:

1. Gerekirse bir kaynak grubu oluşturma.
2. Anahtar kasası oluşturuyor. 
3. Anahtar kasası gelişmiş erişim ilkeleri ayarlama.

Bu adımlar aşağıdaki hızlı başlangıçlarda gösterilmiştir:

- [Azure CLI ile Linux VM oluşturma ve şifreleme](disk-encryption-cli-quickstart.md)
- [Azure PowerShell ile Linux VM oluşturma ve şifreleme](disk-encryption-cli-quickstart.md)

Ayrıca, isterseniz bir anahtar şifreleme anahtarı (KEK) oluşturabilir veya içe aktarabilirsiniz.

> [!Note]
> Bu makaledeki [adımlar, Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started) ve [Azure Disk Şifreleme önkoşullar PowerShell komut otomatiktir.](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyin ve Azure'a bağlanın

Bu makaledeki adımlar [Azure CLI](/cli/azure/), [Azure PowerShell Az modülü](/powershell/azure/overview)veya Azure [portalı](https://portal.azure.com)ile tamamlanabilir. 

Portala tarayıcınız üzerinden erişilebilir ken, Azure CLI ve Azure PowerShell yerel yükleme gerektirir; Bkz. [Linux için Azure Disk Şifrelemesi:](disk-encryption-linux.md#install-tools-and-connect-to-azure) Ayrıntılar için araçlar yükleyin.

### <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

Azure CLI veya Azure PowerShell'i kullanmadan önce öncelikle Azure aboneliğinize bağlanmanız gerekir. Bunu, [Azure CLI ile oturum açarak](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), [Azure Powershell ile oturum açarak](/powershell/azure/authenticate-azureps?view=azps-2.5.0)veya istendiğinde kimlik bilgilerinizi Azure portalına sağlayarak yaparsınız.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

*Zaten bir kaynak grubunuz varsa, [anahtar kasası oluşturmak](#create-a-key-vault)için atlayabilirsiniz.*

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Az grubunu kullanarak bir kaynak grubu oluşturun Azure CLI komutu, [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) Azure PowerShell komutu veya [Azure portalından](https://portal.azure.com) [oluşturun.](/cli/azure/group?view=azure-cli-latest#az-group-create)

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az group create --name "myResourceGroup" --location eastus
```
### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

*Zaten bir anahtar kasanız varsa, [anahtar kasasını ayarla gelişmiş erişim ilkelerine](#set-key-vault-advanced-access-policies)atlayabilirsiniz.*

Az keyvault kullanarak bir anahtar atlama oluşturun Azure CLI komutu, [Yeni-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault) Azure Powershell komutu, [Azure portalı](https://portal.azure.com)veya [Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) [oluşturun.](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create)

>[!WARNING]
> Şifreleme sırlarının bölgesel sınırları aşmamasını sağlamak için Azure Disk Şifrelemesi, Anahtar Kasası ve VM'lerin aynı bölgede birlikte bulunmasını gerektirir. Şifrelenecek VM'ler ile aynı bölgede bulunan bir Anahtar Kasası oluşturun ve kullanın. 

Her Key Vault'un benzersiz bir adı olmalıdır. Aşağıdaki örneklerde benzersiz-keyvault-adınızı> anahtar kasanızın adı ile <değiştirin.

### <a name="azure-cli"></a>Azure CLI

Azure CLI kullanarak bir anahtar kasası oluştururken, "--disk şifreleme için etkin" bayrağını ekleyin.

```azurecli-interactive
az keyvault create --name "<your-unique-keyvault-name>" --resource-group "myResourceGroup" --location "eastus" --enabled-for-disk-encryption
```

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell kullanarak bir anahtar kasası oluştururken "-EnabledForDiskEncryption" bayrağını ekleyin.

```azurepowershell-interactive
New-AzKeyvault -name "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup" -Location "eastus" -EnabledForDiskEncryption
```
### <a name="resource-manager-template"></a>Resource Manager şablonu

[Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)kullanarak bir anahtar kasası da oluşturabilirsiniz.

1. Azure hızlı başlatma şablonunda, **Azure'a Dağıt'ı**tıklatın.
2. Abonelik, kaynak grubu, kaynak grubu konumu, Anahtar Kasa adı, Nesne Kimliği, yasal terimler ve anlaşma seçin ve ardından **Satın Al'ı**tıklatın. 


##  <a name="set-key-vault-advanced-access-policies"></a>Anahtar kasası gelişmiş erişim ilkelerini ayarlama

Azure platformu, birimlerin önyükleme ve şifreçözme için VM'de kullanılabilmesi için anahtar kasanızdaki şifreleme anahtarlarına veya sırlarına erişmeye ihtiyaç duyar. 

Disk şifreleme, dağıtım veya şablon dağıtımı için anahtar kasanızı oluşturma sırasında etkinleştirmediyseniz (önceki adımda gösterildiği gibi), gelişmiş erişim ilkelerini güncelleştirmeniz gerekir.  

### <a name="azure-cli"></a>Azure CLI

Anahtar kasası için disk şifrelemesini etkinleştirmek için [az keyvault güncelleştirmesini](/cli/azure/keyvault#az-keyvault-update) kullanın. 

 - **Disk şifrelemesi için Anahtar Kasasını etkinleştirin:** Disk için etkin şifreleme gereklidir. 

     ```azurecli-interactive
     az keyvault update --name "<your-unique-keyvault-name>" --resource-group "MyResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Gerekirse dağıtım için Anahtar Kasasını etkinleştirin:** Microsoft.Compute kaynak sağlayıcısının, örneğin sanal bir makine oluştururken kaynak oluşturmada bu anahtar kasasına başvurulduğunda bu anahtar kasasından sırları almasına olanak tanır.

     ```azurecli-interactive
     az keyvault update --name "<your-unique-keyvault-name>" --resource-group "MyResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Gerekirse şablon dağıtımı için Anahtar Kasasını etkinleştirin:** Kaynak Yöneticisi'nin kasadan sırları almasına izin verin.
     ```azurecli-interactive  
     az keyvault update --name "<your-unique-keyvault-name>" --resource-group "MyResourceGroup" --enabled-for-template-deployment "true"
     ```

###  <a name="azure-powershell"></a>Azure PowerShell
 Anahtar kasası için disk şifrelemesini etkinleştirmek için PowerShell cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) tuşlarını kullanın.

  - **Disk şifrelemesi için Anahtar Kasasını etkinleştirin:** Azure Disk şifrelemesi için EnabledForDiskEncryption gereklidir.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup" -EnabledForDiskEncryption
     ```

  - **Gerekirse dağıtım için Anahtar Kasasını etkinleştirin:** Microsoft.Compute kaynak sağlayıcısının, örneğin sanal bir makine oluştururken kaynak oluşturmada bu anahtar kasasına başvurulduğunda bu anahtar kasasından sırları almasına olanak tanır.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup" -EnabledForDeployment
     ```

  - **Gerekirse şablon dağıtımı için Anahtar Kasasını etkinleştirin:** Bu anahtar kasasına bir şablon dağıtımında başvurulduğunda Azure Kaynak Yöneticisi'nin bu anahtar kasasından sır lar elde etmesini sağlar.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup" -EnabledForTemplateDeployment
     ```

### <a name="azure-portal"></a>Azure portalında

1. Anahtar kasanızı seçin, **Access İlkeleri'ne**gidin ve **gelişmiş erişim ilkelerini göstermek için tıklatın.**
2. **Birim şifreleme için Azure Disk Şifrelemesine erişimi etkinleştir**etiketiyle kutuyu seçin.
3. **Dağıtım için Azure Sanal Makinelerine Erişimi Etkinleştir'i** ve/veya gerekirse şablon dağıtımı için Azure Kaynak **Yöneticisi'ne Erişimi**Etkinleştir'i'ni seçin. 
4. **Kaydet**'e tıklayın.

    ![Azure anahtar kasası gelişmiş erişim ilkeleri](./media/disk-encryption/keyvault-portal-fig4.png)


## <a name="set-up-a-key-encryption-key-kek"></a>Anahtar şifreleme anahtarı (KEK) ayarlama

Şifreleme anahtarları için ek bir güvenlik katmanı için bir anahtar şifreleme anahtarı (KEK) kullanmak istiyorsanız, anahtar kasanıza bir KEK ekleyin. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme, Key Vault'a yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır.

Azure CLI [az keyvault tuşu oluşturma](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) komutu, Azure PowerShell [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet veya [Azure portalını](https://portal.azure.com/)kullanarak yeni bir KEK oluşturabilirsiniz. Bir RSA anahtar türü oluşturmanız gerekir; Azure Disk Şifreleme henüz Eliptik Eğri anahtarlarını kullanmayı desteklemez.

Bunun yerine şirket içi anahtar yönetimi HSM'nizden bir KEK alabilirsiniz. Daha fazla bilgi için [Anahtar Kasa Belgeleri'ne](../../key-vault/key-vault-hsm-protected-keys.md)bakın. 

Anahtar kasa kek URL'leriniz versiyona alınmalıdır. Azure bu sürüm kısıtlaması uygular. Geçerli gizli ve KEK URL'leri için aşağıdaki örneklere bakın:

* Geçerli bir gizli URL örneği:*https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
* Geçerli bir KEK URL örneği:*https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

Azure Disk Şifreleme, önemli kasa sırları ve KEK URL'lerinin bir parçası olarak bağlantı noktası numaralarının belirtilmesine destek vermez. Desteklenmeyen ve desteklenen anahtar tonoz URL'lerine örnekler için aşağıdaki örneklere bakın:

  * Kabul edilebilir anahtar kasa URL'si:*https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilemez anahtar kasa URL'si:*https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="azure-cli"></a>Azure CLI

Yeni bir KEK oluşturmak ve anahtar kasanızda depolamak için Azure CLI [az keyvault tuşunu](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) kullanın.

```azurecli-interactive
az keyvault key create --name "myKEK" --vault-name "<your-unique-keyvault-name>" --kty RSA-HSM
```

. Bunun yerine Azure CLI [az keyvault anahtar alma](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) komutunu kullanarak özel bir anahtar içe aktarabilirsiniz:

Her iki durumda da KEK'nizin adını Azure CLI [az vm şifreleme etkinleştirme](/cli/azure/vm/encryption?view=azure-cli-latest#az-vm-encryption-enable) anahtarı-şifreleme anahtarı parametresi'ne verirsiniz. 

```azurecli-interactive
az vm encryption enable -g "MyResourceGroup" --name "myVM" --disk-encryption-keyvault "<your-unique-keyvault-name>" --key-encryption-key "myKEK"
```

###  <a name="azure-powershell"></a>Azure PowerShell 

Yeni bir KEK oluşturmak ve anahtar kasanızda saklamak için Azure PowerShell [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey?view=azps-2.5.0) cmdlet'i kullanın.

 ```powershell-interactive
Add-AzKeyVaultKey -Name "myKEK" -VaultName "<your-unique-keyvault-name>" -Destination "HSM"
```

Bunun yerine Azure PowerShell [az keyvault anahtar alma](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) komutunu kullanarak özel bir anahtar içe aktarabilirsiniz.

Her iki durumda da KEK anahtar Vault'unuzun kimliğini ve KEK'inizin URL'sini Azure PowerShell [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.5.0) -KeyEncryptionKeyVaultId ve -KeyEncryptionKeyUrl parametrelerine vereceksiniz. Bu örneğin, hem disk şifreleme anahtarı hem de KEK için aynı anahtar kasasını kullandığınızı varsayalım.

 ```powershell-interactive
$KeyVault = Get-AzKeyVault -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup"
$KEK = Get-AzKeyVaultKey -VaultName "<your-unique-keyvault-name>" -Name "myKEK"

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName "MyVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -KeyEncryptionKeyVaultId $KeyVault.ResourceId -KeyEncryptionKeyUrl $KEK.Id -SkipVmBackup -VolumeType All
 ```
 
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure Disk Şifreleme PowerShell komut dosyası önkoşullar](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Linux VM'lerde Azure Disk Şifreleme senaryolarını](disk-encryption-linux.md) öğrenin
- Azure Disk Şifreleme sorununa nasıl [sorun giderilen](disk-encryption-troubleshooting.md) öğrenin
- Azure [Disk Şifreleme örnek komut dosyalarını](disk-encryption-sample-scripts.md) okuyun
