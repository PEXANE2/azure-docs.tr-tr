---
title: Windows VM'ler için Azure AD ile Azure Disk Şifrelemesi (önceki sürüm)
description: Bu makalede, Windows IaaS VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme ile ilgili yönergeler sağlanmaktadır.
author: msmbaldwin
ms.service: virtual-machines
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 8677fa2620c1edc646dcffe120938f03fd13a0e5
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085630"
---
# <a name="azure-disk-encryption-with-azure-ad-for-windows-vms-previous-release"></a>Windows VM'ler için Azure AD ile Azure Disk Şifrelemesi (önceki sürüm)

**Azure Disk Şifreleme'nin yeni sürümü, VM disk şifrelemesini etkinleştirmek için Bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle, şifrelemeyi etkinleştir medeni sırasında Azure AD kimlik bilgilerini sağlamanız artık gerekmez. Tüm yeni VM'ler, yeni sürümü kullanarak Azure AD uygulama parametreleri olmadan şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirmek için yönergeleri görüntülemek [için Windows VMS için Azure Disk Şifreleme'ye](disk-encryption-windows.md)bakın. Azure AD uygulama parametreleri ile zaten şifrelenmiş olan VM'ler hala desteklenir ve AAD sözdizimi ile korunmaya devam etmelidir.**


Birçok disk şifreleme senaryosunu etkinleştirebilirsiniz ve adımlar senaryoya göre değişebilir. Aşağıdaki bölümler, Windows IaaS VM'leri için senaryoları daha ayrıntılı olarak kapsamaktadır. Disk şifrelemeyi kullanabilmeniz için [önce Azure Disk Şifreleme ön koşullarının](disk-encryption-overview-aad.md) tamamlanması gerekir. 


>[!IMPORTANT]
> - Diskler şifrelenmeden önce [anlık görüntü almalı](snapshot-copy-managed-disk.md) ve/veya yedekleme oluşturmalısınız. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata oluşursa kurtarma seçeneğinin mümkün olmasını sağlar. Yönetilen disklere sahip VM'ler, şifreleme oluşmadan önce yedekleme gerektirir. Yedekleme yapıldıktan sonra, -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için [Set-AzVMDiskEncryptionExtension cmdlet'ini](/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanabilirsiniz. Şifreli VM'leri yedekleme ve geri yükleme hakkında daha fazla bilgi [için, yedekleme ve şifreli Azure VM'yi geri yükleme](../../backup/backup-azure-vms-encryption.md)'ye bakın. 
>
> - Şifrelemeyi şifrelemek veya devre dışı bırakmak Bir VM'nin yeniden başlatılmasına neden olabilir.

## <a name="enable-encryption-on-new-iaas-vms-created-from-the-marketplace"></a>Marketten oluşturulan yeni IaaS VM'lerde şifrelemeyi etkinleştirme
Kaynak Yöneticisi şablonu kullanarak Azure'daki Market'ten yeni IaaS Windows VM'de disk şifrelemeyi etkinleştirebilirsiniz. Şablon, Windows Server 2012 galeri görüntüsünü kullanarak yeni bir şifreli Windows VM oluşturur.

1. Kaynak [Yöneticisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)şablonunda, **Azure'a Dağıt'ı**tıklatın.

2. Aboneliği, kaynak grubunu, kaynak grubu konumunu, parametreleri, yasal koşulları ve anlaşmayı seçin. Şifrelemenin etkin olduğu yeni bir IaaS VM dağıtmak için **Satın Alma'yı** tıklatın.

3. Şablonu dağıttıktan sonra, tercih ettiğiniz yöntemi kullanarak VM şifreleme durumunu doğrulayın:
     - [Az vm şifreleme göster](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanarak Azure CLI ile doğrulayın. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
         ```

     - [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'i kullanarak Azure PowerShell ile doğrulayın. 

         ```azurepowershell-interactive
         Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
         ```

     -  VM'yi seçin ve ardından portaldaki şifreleme durumunu doğrulamak için **Ayarlar** başlığı altındaki **Diskler'i** tıklatın. **Şifreleme**altındaki grafikte etkin olup olmadığını görürsünüz. 
           ![Azure portalı - Disk Şifreleme Etkin](../media/disk-encryption/disk-encryption-fig2.png)

Aşağıdaki tabloda, Azure AD istemci kimliğini kullanarak Market senaryosundaki yeni VM'ler için Kaynak Yöneticisi şablon parametreleri listelenir:

| Parametre | Açıklama | 
| --- | --- |
| adminUserName | Sanal makine için yönetici kullanıcı adı. |
| adminPassword | Sanal makine için yönetici kullanıcı parolası. |
| newStorageAccountName | İşletim sistemi ve veri VHD'lerini depolamak için depolama hesabının adı. |
| vmSize | VM boyutu. Şu anda yalnızca Standart A, D ve G serileri desteklenir. |
| virtualNetworkName | VM NIC'nin ait olması gereken VNet'in adı. |
| subnetName | VM NIC'nin ait olması gereken VNet'teki alt netin adı. |
| AADClientID | Anahtar kasanıza sır yazma izinleri olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Anahtar kasanıza sır yazma izinleri olan Azure AD uygulamasının istemci sırrı. |
| keyVaultURL | BitLocker anahtarının yüklenmesi gereken anahtar kasasının URL'si. Cmdlet `(Get-AzKeyVault -VaultName "MyKeyVault" -ResourceGroupName "MyKeyVaultResourceGroupName").VaultURI` veya Azure CLI kullanarak alabilirsiniz`az keyvault show --name "MySecureVault" --query properties.vaultUri` |
| keyEncryptionKeyURL | Oluşturulan BitLocker anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarının URL'si (isteğe bağlı). </br> </br>KeyEncryptionKeyURL isteğe bağlı bir parametredir. Anahtar kasanızdaki veri şifreleme anahtarını (Passphrase secret) daha fazla korumak için kendi KEK'inizi getirebilirsiniz. |
| keyVaultResourceGroup | Anahtar kasasının kaynak grubu. |
| vmName | Şifreleme işleminin yapılacağı VM'nin adı. |


## <a name="enable-encryption-on-existing-or-running-iaas-windows-vms"></a><a name="bkmk_RunningWinVM"></a>Varolan veya çalışan IaaS Windows VM'lerde şifrelemeyi etkinleştirme
Bu senaryoda, bir şablon, PowerShell cmdlets veya CLI komutları kullanarak şifreleme yi etkinleştirebilirsiniz. Aşağıdaki bölümlerde Azure Disk Şifrelemesi'nin nasıl etkinleştirilen daha ayrıntılı olarak açıklanmaktadır. 


### <a name="enable-encryption-on-existing-or-running-vms-with-azure-powershell"></a><a name="bkmk_RunningWinVMPSH"></a>Azure PowerShell ile varolan veya çalışan VM'lerde şifreleme yi etkinleştirme 
Azure'da çalışan bir IaaS sanal makinede şifreleme sağlamak için [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'ini kullanın. PowerShell cmdlets kullanarak Azure Disk Şifrelemesi ile şifrelemeyi etkinleştirme hakkında bilgi için, blog gönderilerine bakın [Azure PowerShell ile Azure Disk Şifrelemesini Keşfedin - Bölüm 1](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) ve Azure [PowerShell ile Azure Disk Şifrelemesini Keşfedin - Bölüm 2](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

-  **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi çalıştırıyor ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırıyor. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci sırrı zaten ön koşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID ve My-AAD-istemci-gizli değerleriniz ile değiştirin.
     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:** Azure Disk Şifreleme, şifrelemeyi etkinleştirirken oluşturulan disk şifreleme sırlarını sarmak için anahtar kasanızda varolan bir anahtar belirtmenize olanak tanır. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme, Key Vault'a yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır. 

     ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = ‘MyExtraSecureVM’;
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifreli olduğunu doğrulayın:** Bir IaaS VM'nin şifreleme durumunu kontrol etmek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifrelemeyi devre dışı devre dışı:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzureRmVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="enable-encryption-on-existing-or-running-vms-with--azure-cli"></a><a name="bkmk_RunningWinVMCLI"></a>Azure CLI ile varolan veya çalışan VM'lerde şifreleme yi etkinleştirme
Azure'da çalışan bir IaaS sanal makinede şifreleme sağlamak için [az vm şifreleme etkinleştir](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

- **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:**

    ```azurecli-interactive
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
    ```

- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifreli olduğunu doğrulayın:** Bir IaaS VM'nin şifreleme durumunu kontrol etmek için [az vm şifreleme göster](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı kılabilir:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```


### <a name="using-the-resource-manager-template"></a><a name="bkmk_RunningWinVMwRM"> </a>Kaynak Yöneticisi şablonu kullanma
[Çalışan bir Windows VM'yi şifrelemek için Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)kullanarak Azure'da varolan veya çalışan IaaS Windows VM'lerinde disk şifrelemesini etkinleştirebilirsiniz.


1. Azure hızlı başlatma şablonunda, **Azure'a Dağıt'ı**tıklatın.

2. Aboneliği, kaynak grubunu, kaynak grubu konumunu, parametreleri, yasal koşulları ve anlaşmayı seçin. Varolan veya çalışan IaaS VM'de şifrelemeyi etkinleştirmek için **Satın Al'ı** tıklatın.

Aşağıdaki tabloda, Azure AD istemci kimliği kullanan varolan veya çalışan VM'ler için Kaynak Yöneticisi şablon parametreleri listelenir:

| Parametre | Açıklama |
| --- | --- |
| AADClientID | Anahtar kasasına sır yazma izinleri olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Anahtar kasasına sır yazma izinleri olan Azure AD uygulamasının istemci sırrı. |
| keyVaultName | BitLocker anahtarının yüklenmesi gereken anahtar kasasının adı. Cmdlet `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutunu kullanarak alabilirsiniz`az keyvault list --resource-group "MySecureGroup"`|
|  keyEncryptionKeyURL | Oluşturulan BitLocker anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarının URL'si. UseExistingKek açılır listesinde **nokek** seçerseniz bu parametre isteğe bağlıdır. UseExistingKek açılır listesinde **kek'i** seçerseniz, _anahtarEncryptionKeyURL_ değerini girmeniz gerekir. |
| volumeType | Şifreleme işleminin gerçekleştirildiği birim türü. Geçerli değerler _işletim sistemi,_ _Veri_ve _Tümü'tür._ |
| sequenceVersion | BitLocker işleminin sıralı sürümü. Aynı VM üzerinde her disk şifreleme işlemi yapıldığında bu sürüm numarasını artım. |
| vmName | Şifreleme işleminin yapılacağı VM'nin adı. |


## <a name="new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a><a name="bkmk_VHDpre"> </a>Müşteri şifreli VHD ve şifreleme anahtarlarından oluşturulan yeni IaaS VM'ler
Bu senaryoda, Kaynak Yöneticisi şablonunu, PowerShell cmdlets'i veya CLI komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki bölümlerde Kaynak Yöneticisi şablonu ve CLI komutları daha ayrıntılı olarak açıklanabilir. 

Azure'da kullanılabilecek önceden şifrelenmiş görüntüleri hazırlamak için ekteki yönergeleri kullanın. Resim oluşturulduktan sonra, şifreli bir Azure VM oluşturmak için bir sonraki bölümdeki adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](disk-encryption-sample-scripts.md#prepare-a-pre-encrypted-windows-vhd)


### <a name="encrypt-vms-with-pre-encrypted-vhds-with-azure-powershell"></a><a name="bkmk_VHDprePSH"> </a> Azure PowerShell ile önceden şifrelenmiş VHD'lerle VM'leri şifreleme
PowerShell cmdlet [Set-AzVMOSDisk'i](/powershell/module/az.compute/set-azvmosdisk#examples)kullanarak şifreli VHD'nizde disk şifrelemesini etkinleştirebilirsiniz. Aşağıdaki örnekte bazı ortak parametreler verir. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen bir veri diskinde şifrelemeyi etkinleştirme
PowerShell'i kullanarak veya [Azure portalı aracılığıyla](attach-managed-disk-portal.md)Windows [VM'ye yeni bir disk ekleyebilirsiniz.](attach-disk-ps.md) 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir diskte şifreleme yi etkinleştirme
 Windows VM'ler için yeni bir diski şifrelemek için Powershell kullanılırken, yeni bir sıra sürümü belirtilmelidir. Dizi sürümü benzersiz olmalıdır. Aşağıdaki komut dosyası, sıra sürümü için bir GUID oluşturur. Bazı durumlarda, yeni eklenen bir veri diski Azure Disk Şifreleme uzantısı tarafından otomatik olarak şifrelenebilir. Otomatik şifreleme genellikle yeni disk çevrimiçi olduktan sonra VM yeniden başlatıldığında oluşur. Bunun nedeni genellikle, disk şifrelemesi vm'de daha önce çalıştırıldığında birim türü için "Tümü" belirtilmiş olduğundan kaynaklanır. Yeni eklenen bir veri diskinde otomatik şifreleme oluşuyorsa, Set-AzVmDiskEncryptionExtension cmdlet'i yeni sıra sürümüyle yeniden çalıştırmanızı öneririz. Yeni veri diskiniz otomatik olarak şifrelenmişse ve şifrelenmek istemiyorsanız, önce tüm sürücülerin şifresini çöz, ardından ses türü için işletim sistemi belirten yeni bir dizi sürümüyle yeniden şifreleyin. 
 

-  **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi çalıştırıyor ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırıyor. Kaynak grubu, VM, anahtar kasası, AAD uygulaması ve istemci sırrı zaten ön koşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID ve My-AAD-istemci-gizli değerleriniz ile değiştirin. Bu örnek, hem işletim sistemi hem de Veri birimlerini içeren -VolumeType parametresi için "Tümü" kullanır. Yalnızca işletim sistemi hacmini şifrelemek istiyorsanız, -VolumeType parametresi için "OS" kullanın. 

     ```azurepowershell
      $sequenceVersion = [Guid]::NewGuid();
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;
    ```
- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:** Azure Disk Şifreleme, şifrelemeyi etkinleştirirken oluşturulan disk şifreleme sırlarını sarmak için anahtar kasanızda varolan bir anahtar belirtmenize olanak tanır. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme, Key Vault'a yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır. Bu örnek, hem işletim sistemi hem de Veri birimlerini içeren -VolumeType parametresi için "Tümü" kullanır. Yalnızca işletim sistemi hacmini şifrelemek istiyorsanız, -VolumeType parametresi için "OS" kullanın. 

     ```azurepowershell
     $sequenceVersion = [Guid]::NewGuid();
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir diskte şifreleme yi etkinleştirme
  Azure CLI komutu, şifrelemeyi etkinleştirmek için komutu çalıştırdığınızda sizin için otomatik olarak yeni bir sıra sürümü sağlar. Birim-yype parametresi için kabul edilebilir değerler Tümü, İşletim sistemi ve Veriler'dir. VM için yalnızca bir disk türünü şifreliyorsanız, birim türü parametresini işletim sistemi veya Veri olarak değiştirmeniz gerekebilir. Örnekler, birim türü parametresi için "Tümü"ni kullanır. 

-  **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "all"
     ```


## <a name="enable-encryption-using-azure-ad-client-certificate-based-authentication"></a>Azure AD istemci sertifikası tabanlı kimlik doğrulamasını kullanarak şifrelemeyi etkinleştirin.
KEK ile veya KEK olmadan istemci sertifikası kimlik doğrulamasını kullanabilirsiniz. PowerShell komut dosyalarını kullanmadan önce, sertifikanın anahtar kasasına yüklenmesi ve VM'ye dağıtılması gerekir. KEK'i de kullanıyorsanız, KEK zaten var olmalıdır. Daha fazla bilgi için, önkoşullar makalesinin [Azure AD bölümüiçin Sertifika tabanlı kimlik doğrulaması](disk-encryption-key-vault-aad.md#certificate-based-authentication-optional) bölümüne bakın.


### <a name="enable-encryption-using-certificate-based-authentication-with-azure-powershell"></a>Azure PowerShell ile sertifika tabanlı kimlik doğrulamayı kullanarak şifrelemeyi etkinleştirme

```powershell
## Fill in 'MyVirtualMachineResourceGroup', 'MyKeyVaultResourceGroup', 'My-AAD-client-ID', 'MySecureVault, and ‘MySecureVM’.

$VMRGName = 'MyVirtualMachineResourceGroup'
$KVRGname = 'MyKeyVaultResourceGroup';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

# Fill in the certificate path and the password so the thumbprint can be set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
```
  
### <a name="enable-encryption-using-certificate-based-authentication-and-a-kek-with-azure-powershell"></a>Azure PowerShell ile sertifika tabanlı kimlik doğrulaması ve KEK kullanarak şifreleme yi etkinleştirme

```powershell
# Fill in 'MyVirtualMachineResourceGroup', 'MyKeyVaultResourceGroup', 'My-AAD-client-ID', 'MySecureVault,, 'MySecureVM’, and "KEKName.

$VMRGName = 'MyVirtualMachineResourceGroup';
$KVRGname = 'MyKeyVaultResourceGroup';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$keyEncryptionKeyName ='KEKName';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

## Fill in the certificate path and the password so the thumbprint can be read and set as a variable.

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint and a KEK

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

## <a name="disable-encryption"></a>Şifrelemeyi devre dışı
Azure PowerShell, Azure CLI veya Kaynak Yöneticisi şablonu kullanarak şifrelemeyi devre dışı kullanabilirsiniz. 

- **Azure PowerShell ile disk şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzureRmVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

- **Azure CLI ile şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Kaynak Yöneticisi Şablonu yla şifrelemeyi devre dışı kakın:** 

    1. Windows VM şablonu [çalıştıran Disk şifrelemesini devre dışı takın'dan](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) **Azure'a Dağıt'ı** tıklatın.
    2. Aboneliği, kaynak grubunu, konumu, VM'yi, yasal koşulları ve anlaşmayı seçin.
    3.  Çalışan bir Windows VM'de disk şifrelemesini devre dışı kalmak için **Satın Al'ı** tıklatın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)
