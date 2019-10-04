---
title: Windows IaaS VM 'Leri için Azure disk şifrelemesini etkinleştirme
description: Bu makalede, Windows IaaS VM 'Leri için Microsoft Azure disk şifrelemeyi etkinleştirme yönergeleri sunulmaktadır.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/12/2019
ms.custom: seodec18
ms.openlocfilehash: 109c954ab877d0b8d348fc4b9d7de01c19e41344
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958831"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Windows IaaS VM 'Leri için Azure disk şifrelemesini etkinleştirme

Bu makalede, Windows IaaS sanal makineleri (VM 'Ler) için Microsoft Azure disk şifrelemeyi etkinleştirme yönergeleri sunulmaktadır. Disk şifrelemeyi kullanabilmeniz için önce [Azure disk şifrelemesi önkoşullarını](../security/azure-security-disk-encryption-prerequisites.md)doldurmanız gerekir. 

Ayrıca, şifrelemeden önce [bir anlık görüntü oluşturup](../virtual-machines/windows/snapshot-copy-managed-disk.md) /veya disklerinizi yedeklemeniz önemle önerilir. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata oluşursa, kurtarma seçeneğinin mümkün olmasını güvence altına almanıza olanak tanır. Yönetilen disklere sahip VM 'Ler şifreleme gerçekleşmeden önce bir yedekleme gerektirir. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için [set-AzVMDiskEncryptionExtension cmdlet 'ini](/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanabilirsiniz. Şifrelenmiş VM 'Leri yedekleme ve geri yükleme hakkında daha fazla bilgi için bkz. [şifrelenmiş Azure VM 'Yi yedekleme ve geri yükleme](../backup/backup-azure-vms-encryption.md) .

>[!WARNING]
> - Bu VM 'yi şifrelemek için Azure [ad uygulaması Ile Azure disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) 'ni daha önce KULLANDıYSANıZ, VM 'nizi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Bu desteklenen bir senaryo olmadığından, bu şifrelenmiş VM 'de [Azure disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) 'ni kullanamazsınız. Bu, bu şifrelenmiş VM için AAD uygulamasından uzağa geçiş henüz desteklenmiyor. 
> - Azure disk şifrelemesi, Key Vault ve VM 'Lerin aynı bölgede birlikte bulunmasını gerektirir. Şifrelenecek VM ile aynı bölgedeki bir Key Vault oluşturun ve kullanın. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningWinVM"></a>Mevcut veya çalışan IaaS Windows VM 'lerinde şifrelemeyi etkinleştirme
Bir şablon, PowerShell cmdlet 'leri veya CLı komutları kullanarak şifrelemeyi etkinleştirebilirsiniz. Sanal makine uzantısı için şema bilgilerine ihtiyacınız varsa, bkz. [Windows uzantısı Için Azure disk şifrelemesi](../virtual-machines/extensions/azure-disk-enc-windows.md) makalesi.

>[!IMPORTANT]
 > Yönetilen disk tabanlı VM örneğinin, Azure disk şifrelemesi etkinleştirilmeden önce ve/veya dışında bir şekilde anlık görüntü ve/veya yedeklenmesi zorunludur. Yönetilen diskin bir anlık görüntüsü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata olması durumunda kurtarma seçeneğinin yapılabilmesini sağlamaktır. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
> Şifrelemeyi şifrelemek veya devre dışı bırakmak VM 'nin yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningWinVMPSH"></a>Azure PowerShell ile mevcut veya çalışan VM 'lerde şifrelemeyi etkinleştirme 
Azure 'da çalışan bir IaaS sanal makinesinde şifrelemeyi etkinleştirmek için [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın. 

-  **Çalışan bir VM 'Yi şifreleyin:** Aşağıdaki komut dosyası değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubu, VM ve Anahtar Kasası zaten ön koşullar olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Çalışan bir VM 'yi KEK kullanarak şifreleyin:** 

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı]</br> Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasaadı]. kasa. Azure. net/Keys/[kekname]/[kek-Unique-ID] ile 

- **Disklerin şifrelendiğini doğrulayın:** IaaS VM 'sinin şifreleme durumunu denetlemek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet 'ini kullanın. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifrelemesini devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. Hem işletim sistemi hem de veri diski şifrelendiğinde, Windows VM 'de veri diski şifrelemesini devre dışı bırakmak beklenen şekilde çalışmaz. Bunun yerine tüm disklerde şifrelemeyi devre dışı bırakın.

     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Azure CLı ile mevcut veya çalışan VM 'lerde şifrelemeyi etkinleştirme
Azure 'da çalışan bir IaaS sanal makinesinde şifrelemeyi etkinleştirmek için [az VM Encryption Enable](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

- **Çalışan bir VM 'yi şifreleyin:**

    ```azurecli-interactive
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
    ```

- **Çalışan bir VM 'yi KEK kullanarak şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı] </br> Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasaadı]. kasa. Azure. net/Keys/[kekname]/[kek-Unique-ID] ile 

- **Disklerin şifrelendiğini doğrulayın:** IaaS VM 'sinin şifreleme durumunu denetlemek için [az VM Encryption Show](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Hem işletim sistemi hem de veri diski şifrelendiğinde, Windows VM 'de veri diski şifrelemesini devre dışı bırakmak beklenen şekilde çalışmaz. Bunun yerine tüm disklerde şifrelemeyi devre dışı bırakın.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
 
  > [!NOTE]
  >Yönetilen disk tabanlı VM örneğinin, Azure disk şifrelemesi etkinleştirilmeden önce ve/veya dışında bir şekilde anlık görüntü ve/veya yedeklenmesi zorunludur. Yönetilen diskin bir anlık görüntüsü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata olması durumunda kurtarma seçeneğinin yapılabilmesini sağlamaktır. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Bu komut, bir yedekleme yapıldıktan ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
  >
  >Şifrelemeyi şifrelemek veya devre dışı bırakmak VM 'nin yeniden başlatılmasına neden olabilir. 

### <a name="bkmk_RunningWinVMwRM"> </a>Kaynak Yöneticisi şablonunu kullanma

[Çalışan bir WINDOWS VM 'yi şifrelemek için Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)kullanarak Azure 'da var olan veya çalışan IaaS Windows VM 'lerinde disk şifrelemeyi etkinleştirebilirsiniz.


1. Azure hızlı başlangıç şablonunda **Azure 'A dağıt**' a tıklayın.

2. Abonelik, kaynak grubu, konum, ayarlar, yasal koşullar ve Sözleşme ' yi seçin. Mevcut veya çalışan IaaS VM üzerinde şifrelemeyi etkinleştirmek için **satın al** ' a tıklayın.

Aşağıdaki tabloda mevcut veya çalışan VM 'Ler için Kaynak Yöneticisi şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işleminin çalıştırılacağı sanal makinenin adı. |
| keyVaultName | BitLocker anahtarının yüklenmesi gereken anahtar kasasının adı. Bunu, `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLı komutu olan `az keyvault list --resource-group "MyKeyVaultResourceGroup"` ' i kullanarak alabilirsiniz.|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  keyEncryptionKeyURL 'Si | Anahtar şifreleme anahtarının URL 'SI, https://@no__t -0keyvault-name&gt;.vault.azure.net/Key/&lt;key-Name @ no__t-3 biçimindedir. Bir KEK kullanmak istemiyorsanız, bu alanı boş bırakın. |
| Birimtürü | Şifreleme işleminin gerçekleştirildiği birimin türü. Geçerli değerler _Işletim sistemi_, _veri_ve _hepsi_. 
| forceUpdateTag | İşlemin zorla çalışması gereken her seferinde GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimini bölmeden önce IŞLETIM sistemi bölümünün tam işletim sistemi VHD 'SI kaplamaya yeniden boyutlandırılması gerekir. |
| konum | Tüm kaynakların konumu. |

## <a name="encrypt-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerini şifreleyin

[Azure sanal makine ölçek kümeleri](../virtual-machine-scale-sets/overview.md) , bir özdeş, yük dengeli VM 'ler grubu oluşturmanıza ve yönetmenize olanak sağlar. Sanal makine örneklerinin sayısı isteğe yanıt olarak veya tanımlı bir zamanlamaya göre otomatik olarak artabilir veya azalmış olabilir. Sanal makine ölçek kümelerini şifrelemek için CLı veya Azure PowerShell kullanın.

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Sanal makine ölçek kümelerini Azure PowerShell ile şifreleyin

Windows sanal makine ölçek kümesi üzerinde şifrelemeyi etkinleştirmek için [set-AzVmssDiskEncryptionExtension](/powershell/module/az.compute/set-azvmssdiskencryptionextension) cmdlet 'ini kullanın. Kaynak grubu, sanal makine ölçek kümesi ve Anahtar Kasası zaten ön koşullar olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesini şifreleyin**:
    ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:

    ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $KeyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     Set-AzRmVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

   >[!NOTE]
   > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı]</br> Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasaadı]. kasa. Azure. net/Keys/[kekname]/[kek-Unique-ID] ile 

- **Bir sanal makine ölçek kümesi için şifreleme durumu Al:** [Get-AzVmssDiskEncryption](/powershell/module/az.compute/get-azvmssdiskencryption) cmdlet 'ini kullanın.
    
    ```azurepowershell-interactive
    get-AzVmssVMDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesinde şifrelemeyi devre dışı bırak**: [Disable-AzVmssDiskEncryption](/powershell/module/az.compute/disable-azvmssdiskencryption) cmdlet 'ini kullanın. 

    ```azurepowershell-interactive
    Disable-AzVmssDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Azure CLı ile sanal makine ölçek kümelerini şifreleme

Windows sanal makine ölçek kümesi üzerinde şifrelemeyi etkinleştirmek için [az VMSS ENCRYPTION Enable](/cli/azure/vmss/encryption#az-vmss-encryption-enable) komutunu kullanın. Ölçek kümesinde yükseltme ilkesini el ile olarak ayarlarsanız, [az VMSS Update-Instances](/cli/azure/vmss#az-vmss-update-instances)ile şifrelemeyi başlatın. Kaynak grubu, sanal makine ölçek kümesi ve Anahtar Kasası zaten ön koşullar olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesini şifreleme**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Anahtarı kaydırmak için KEK kullanarak çalışan bir sanal makine ölçek kümesini şifreleyin**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
     
   >[!NOTE]
   > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı]</br> Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasaadı]. kasa. Azure. net/Keys/[kekname]/[kek-Unique-ID] ile 

- **Bir sanal makine ölçek kümesi için şifreleme durumu Al:** [Az VMSS ENCRYPTION Show](/cli/azure/vmss/encryption#az-vmss-encryption-show) komutunu kullanın

    ```azurecli-interactive
     az vmss encryption show --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı bırak**: [az VMSS ENCRYPTION Disable](/cli/azure/vmss/encryption#az-vmss-encryption-disable) komutunu kullanın
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-windows-virtual-machine-scale-sets"></a>Windows sanal makine ölçek kümeleri için Azure Resource Manager şablonları

Windows sanal makine ölçek kümelerini şifrelemek veya şifresini çözmek için aşağıdaki Azure Resource Manager şablonları ve yönergeleri kullanın:

- [Windows sanal makine ölçek kümesi üzerinde şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)
- [Windows sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

     1. **Azure 'A dağıt**' a tıklayın.
     2. Gerekli alanları doldurup hüküm ve koşulları kabul edin.
     3. Şablonu dağıtmak için **satın al** ' a tıklayın.

## <a name="bkmk_VHDpre"></a> Müşteri tarafından şifrelenen VHD ve şifreleme anahtarlarından oluşturulan yeni IaaS VM 'leri

Bu senaryoda, PowerShell cmdlet 'lerini veya CLı komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. 

Azure 'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama ekinde yer alan yönergeleri kullanın. Görüntü oluşturulduktan sonra, şifrelenmiş bir Azure VM oluşturmak için sonraki bölümdeki adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenen bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Yönetilen disk tabanlı VM örneğinin, Azure disk şifrelemesi etkinleştirilmeden önce ve/veya dışında bir şekilde anlık görüntü ve/veya yedeklenmesi zorunludur. Yönetilen diskin bir anlık görüntüsü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata olması durumunda kurtarma seçeneğinin yapılabilmesini sağlamaktır. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
>Şifrelemeyi şifrelemek veya devre dışı bırakmak VM 'nin yeniden başlatılmasına neden olabilir. 


### <a name="bkmk_VHDprePSH"></a> Azure PowerShell ile önceden şifrelenen VHD 'Ler Ile VM 'leri şifreleme
[Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples)PowerShell cmdlet 'ini kullanarak, şifrelenen VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki örnek size bazı ortak parametreler vermektedir. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen bir veri diskinde şifrelemeyi etkinleştir
PowerShell kullanarak veya [Azure Portal aracılığıyla](../virtual-machines/windows/attach-managed-disk-portal.md) [bir Windows sanal makinesine yeni bir disk ekleyebilirsiniz](../virtual-machines/windows/attach-disk-ps.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir diskte şifrelemeyi etkinleştir
 Windows VM 'Leri için yeni bir disk şifrelemek üzere PowerShell kullanırken yeni bir sıra sürümü belirtilmelidir. Sıra sürümü benzersiz olmalıdır. Aşağıdaki komut dosyası, dizi sürümü için bir GUID oluşturur. Bazı durumlarda, yeni eklenen bir veri diski Azure disk şifrelemesi uzantısı tarafından otomatik olarak şifrelenmiş olabilir. Otomatik şifreleme, genellikle yeni disk çevrimiçi olduktan sonra sanal makine yeniden başlatıldığında meydana gelir. Bu, genellikle disk şifrelemesi VM 'de daha önce çalıştırıldığında birim türü için "All" belirtildiğinde oluşur. Yeni eklenen bir veri diskinde Otomatik şifreleme gerçekleşirse, set-AzVmDiskEncryptionExtension cmdlet 'ini yeni bir sıra sürümü ile yeniden çalıştırmayı öneririz. Yeni veri diskiniz otomatik olarak şifrelenirse ve şifrelenmek istemiyorsanız, tüm sürücülerin şifresini çözün, ardından birim türü için işletim sistemini belirten yeni bir sıra sürümü ile yeniden şifreleyin. 
  
 

-  **Çalışan bir VM 'Yi şifreleyin:** Aşağıdaki komut dosyası değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubu, VM ve Anahtar Kasası zaten ön koşullar olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin. Bu örnek, hem işletim sistemi hem de veri birimlerini içeren-VolumeType parametresi için "All" kullanır. Yalnızca işletim sistemi birimini şifrelemek istiyorsanız,-VolumeType parametresi için "OS" kullanın. 

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;
    ```
- **Çalışan bir VM 'YI kek kullanarak şifreleyin:** Bu örnek, hem işletim sistemi hem de veri birimlerini içeren-VolumeType parametresi için "All" kullanır. Yalnızca işletim sistemi birimini şifrelemek istiyorsanız,-VolumeType parametresi için "OS" kullanın.

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     $sequenceVersion = [Guid]::NewGuid();

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı]</br> Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasaadı]. kasa. Azure. net/Keys/[kekname]/[kek-Unique-ID] ile 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLı ile yeni eklenen bir diskte şifrelemeyi etkinleştirme
 Şifrelemeyi etkinleştirmek için komutunu çalıştırdığınızda Azure CLı komutu sizin için otomatik olarak yeni bir dizi sürümü sağlar. Örnek, birim türü parametresi için "All" kullanır. Yalnızca işletim sistemi diskini şifreliyorsanız, birim türü parametresini işletim sistemi olarak değiştirmeniz gerekebilir. PowerShell sözdiziminin aksine, CLı, kullanıcının şifrelemeyi etkinleştirirken benzersiz bir sıra sürümü sağlamasını gerektirmez. CLı otomatik olarak kendi benzersiz dizi sürümü değerini oluşturur ve kullanır.   

-  **Çalışan bir VM 'yi şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **Çalışan bir VM 'yi KEK kullanarak şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "All"
     ```


## <a name="disable-encryption"></a>Şifrelemeyi devre dışı bırak
Azure PowerShell, Azure CLı veya Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırakabilirsiniz. Hem işletim sistemi hem de veri diski şifrelendiğinde, Windows VM 'de veri diski şifrelemesini devre dışı bırakmak beklenen şekilde çalışmaz. Bunun yerine tüm disklerde şifrelemeyi devre dışı bırakın.

- **Azure PowerShell ile disk şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Azure CLI ile şifrelemeyi devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type "all"
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırak:** 

    1. [WINDOWS VM 'leri çalıştırırken disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) ' dan **Azure 'a dağıt** ' a tıklayın.
    2. Abonelik, kaynak grubu, konum, VM, birim türü, yasal koşullar ve Sözleşme ' yi seçin.
    3.  Çalışan bir Windows VM 'de disk şifrelemeyi devre dışı bırakmak için **satın al** ' a tıklayın 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Linux için Azure disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux.md)
