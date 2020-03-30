---
title: Windows VM'lerinde Azure Disk Şifrelemesi senaryoları
description: Bu makalede, çeşitli senaryolar için Windows VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme hakkında yönergeler
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: ed64ee3d0e024c32be08ed4e010a6933033c3f87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476527"
---
# <a name="azure-disk-encryption-scenarios-on-windows-vms"></a>Windows VM'lerinde Azure Disk Şifrelemesi senaryoları

Azure Disk Şifrelemesi, Azure sanal makinelerinin (VM) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için BitLocker harici anahtar koruyucusu kullanır ve disk şifreleme anahtarlarını ve sırlarını kontrol ve yönetmenize yardımcı olmak için Azure Key Vault ile tümleştirilir. Hizmete genel bir bakış için [Windows VM'leri için Azure Disk Şifreleme'ye](disk-encryption-overview.md)bakın.

Birçok disk şifreleme senaryosu vardır ve adımlar senaryoya göre değişebilir. Aşağıdaki bölümler, Windows VM'leri için senaryoları daha ayrıntılı olarak kapsamaktadır.

Disk şifrelemeyi yalnızca [desteklenen VM boyutlarının ve işletim sistemlerinin](disk-encryption-overview.md#supported-vms-and-operating-systems)sanal makinelerine uygulayabilirsiniz. Ayrıca aşağıdaki ön koşulları karşılamanız gerekir:

- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [Grup İlkesi gereksinimleri](disk-encryption-overview.md#group-policy-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

>[!IMPORTANT]
> - VM'yi şifrelemek için Azure AD ile Azure Disk Şifreleme'yi daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz. Ayrıntılar için [Azure AD (önceki sürüm) ile Azure Disk](disk-encryption-overview-aad.md) Şifrelemesi'ne bakın. 
>
> - Diskler şifrelenmeden önce [anlık görüntü almalı](snapshot-copy-managed-disk.md) ve/veya yedekleme oluşturmalısınız. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata oluşursa kurtarma seçeneğinin mümkün olmasını sağlar. Yönetilen disklere sahip VM'ler, şifreleme oluşmadan önce yedekleme gerektirir. Yedekleme yapıldıktan sonra, -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için [Set-AzVMDiskEncryptionExtension cmdlet'ini](/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanabilirsiniz. Şifreli VM'leri yedekleme ve geri yükleme hakkında daha fazla bilgi [için, yedekleme ve şifreli Azure VM'yi geri yükleme](../../backup/backup-azure-vms-encryption.md)'ye bakın. 
>
> - Şifrelemeyi şifrelemek veya devre dışı bırakmak Bir VM'nin yeniden başlatılmasına neden olabilir.

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyin ve Azure'a bağlanın

[!INCLUDE [disk-encryption-install-cli-powershell](../../../includes/disk-encryption-install-cli-powershell.md)]

## <a name="enable-encryption-on-an-existing-or-running-windows-vm"></a>Varolan veya çalışan bir Windows VM'de şifrelemeyi etkinleştirme
Bu senaryoda, Kaynak Yöneticisi şablonunu, PowerShell cmdlets'i veya CLI komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. Sanal makine uzantısı için şema bilgisine ihtiyacınız varsa, [Windows uzantısı için Azure Disk Şifreleme](../extensions/azure-disk-enc-windows.md) makalesine bakın.

## <a name="enable-encryption-on-existing-or-running-iaas-windows-vms"></a>Varolan veya çalışan IaaS Windows VM'lerde şifrelemeyi etkinleştirme
Bir şablon, PowerShell cmdlets veya CLI komutları kullanarak şifrelemeyi etkinleştirebilirsiniz. Sanal makine uzantısı için şema bilgisine ihtiyacınız varsa, [Windows uzantısı için Azure Disk Şifreleme](../extensions/azure-disk-enc-windows.md) makalesine bakın.

### <a name="enable-encryption-on-existing-or-running-vms-with-azure-powershell"></a>Azure PowerShell ile varolan veya çalışan VM'lerde şifreleme yi etkinleştirme 
Azure'da çalışan bir IaaS sanal makinede şifreleme sağlamak için [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'ini kullanın. 

-  **Çalışan vm'yi şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi çalıştırıyor ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırıyor. Kaynak grubu, VM ve anahtar kasası zaten ön koşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM ve MySecureVault'u değerlerinizle değiştirin.

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
- **KEK kullanarak çalışan bir VM'yi şifreleme:** 

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
   > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifreli olduğunu doğrulayın:** Bir IaaS VM'nin şifreleme durumunu kontrol etmek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifrelemeyi devre dışı devre dışı:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. Windows VM'de hem işletim sistemi hem de veri diskleri şifrelenmiş durumdayken yalnızca veri diskinde şifrelemenin devre dışı bırakılması istenen sonucu vermeyebilir. Bunun yerine tüm disklerde şifrelemeyi devre dışı kakın.

     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="enable-encryption-on-existing-or-running-vms-with-the-azure-cli"></a>Azure CLI ile varolan veya çalışan VM'lerde şifreleme yi etkinleştirme
Azure'da çalışan bir IaaS sanal makinede şifreleme sağlamak için [az vm şifreleme etkinleştir](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

- **Çalışan vm'yi şifreleme:**

    ```azurecli-interactive
    az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
    ```

- **KEK kullanarak çalışan bir VM'yi şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifreli olduğunu doğrulayın:** Bir IaaS VM'nin şifreleme durumunu kontrol etmek için [az vm şifreleme göster](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı kılabilir:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Windows VM'de hem işletim sistemi hem de veri diskleri şifrelenmiş durumdayken yalnızca veri diskinde şifrelemenin devre dışı bırakılması istenen sonucu vermeyebilir. Bunun yerine tüm disklerde şifrelemeyi devre dışı kakın.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```

### <a name="using-the-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanma

[Çalışan bir Windows VM'yi şifrelemek için Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)kullanarak Azure'da varolan veya çalışan IaaS Windows VM'lerinde disk şifrelemesini etkinleştirebilirsiniz.


1. Azure hızlı başlatma şablonunda, **Azure'a Dağıt'ı**tıklatın.

2. Aboneliği, kaynak grubunu, konumu, ayarları, yasal koşulları ve anlaşmayı seçin. Varolan veya çalışan IaaS VM'de şifrelemeyi etkinleştirmek için **Satın Al'ı** tıklatın.

Aşağıdaki tabloda, varolan veya çalışan VM'ler için Kaynak Yöneticisi şablon parametreleri listelenir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemini çalıştırmak için VM'nin adı. |
| keyVaultName | BitLocker anahtarının yüklenmesi gereken anahtar kasasının adı. Cmdlet `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutunu kullanarak alabilirsiniz`az keyvault list --resource-group "MyKeyVaultResourceGroup"`|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  keyEncryptionKeyURL | Anahtar şifreleme anahtarının URL'si,&lt;keyvault&gt;adı&lt;https:// .vault.azure.net/key/ anahtar adı&gt;biçiminde. KEK kullanmak istemiyorsanız, bu alanı boş bırakın. |
| volumeType | Şifreleme işleminin gerçekleştirildiği birim türü. Geçerli değerler _işletim sistemi,_ _Veri_ve _Tümü'tür._ 
| forceUpdateTag | Operasyonun kuvvet çalışması gerektiğinde GUID gibi benzersiz bir değerde geçirin. |
| yeniden boyutlandırmaOSDisk | Sistem bölümü sistem hacmi bölmeden önce tam işletim sistemi VHD işgal etmek için yeniden boyutlandırılır. |
| location | Tüm kaynaklar için konum. |


## <a name="new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Müşteri şifreli VHD ve şifreleme anahtarlarından oluşturulan yeni IaaS VM'ler

Bu senaryoda, PowerShell cmdlets veya CLI komutlarını kullanarak önceden şifrelenmiş bir VHD'den ve ilişkili şifreleme anahtarlarından yeni bir VM oluşturabilirsiniz. 

[Önceden şifrelenmiş Bir Windows VHD hazırla'da](disk-encryption-sample-scripts.md#prepare-a-pre-encrypted-windows-vhd)yönergeleri kullanın. Resim oluşturulduktan sonra, şifreli bir Azure VM oluşturmak için bir sonraki bölümdeki adımları kullanabilirsiniz.


### <a name="encrypt-vms-with-pre-encrypted-vhds-with-azure-powershell"></a>Azure PowerShell ile önceden şifrelenmiş VHD'lerle VM'leri şifreleme
PowerShell cmdlet [Set-AzVMOSDisk'i](/powershell/module/az.compute/set-azvmosdisk#examples)kullanarak şifreli VHD'nizde disk şifrelemesini etkinleştirebilirsiniz. Aşağıdaki örnekte bazı ortak parametreler verir. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen bir veri diskinde şifrelemeyi etkinleştirme
PowerShell'i kullanarak veya [Azure portalı aracılığıyla](attach-managed-disk-portal.md)Windows [VM'ye yeni bir disk ekleyebilirsiniz.](attach-disk-ps.md) 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir diskte şifreleme yi etkinleştirme
 Windows VM'ler için yeni bir diski şifrelemek için Powershell kullanılırken, yeni bir sıra sürümü belirtilmelidir. Dizi sürümü benzersiz olmalıdır. Aşağıdaki komut dosyası, sıra sürümü için bir GUID oluşturur. Bazı durumlarda, yeni eklenen bir veri diski Azure Disk Şifreleme uzantısı tarafından otomatik olarak şifrelenebilir. Otomatik şifreleme genellikle yeni disk çevrimiçi olduktan sonra VM yeniden başlatıldığında oluşur. Bunun nedeni genellikle, disk şifrelemesi vm'de daha önce çalıştırıldığında birim türü için "Tümü" belirtilmiş olduğundan kaynaklanır. Yeni eklenen bir veri diskinde otomatik şifreleme oluşuyorsa, Set-AzVmDiskEncryptionExtension cmdlet'i yeni sıra sürümüyle yeniden çalıştırmanızı öneririz. Yeni veri diskiniz otomatik olarak şifrelenmişse ve şifrelenmek istemiyorsanız, önce tüm sürücülerin şifresini çöz, ardından ses türü için işletim sistemi belirten yeni bir dizi sürümüyle yeniden şifreleyin. 
  
 

-  **Çalışan vm'yi şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi çalıştırıyor ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırıyor. Kaynak grubu, VM ve anahtar kasası zaten ön koşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM ve MySecureVault'u değerlerinizle değiştirin. Bu örnek, hem işletim sistemi hem de Veri birimlerini içeren -VolumeType parametresi için "Tümü" kullanır. Yalnızca işletim sistemi hacmini şifrelemek istiyorsanız, -VolumeType parametresi için "OS" kullanın. 

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
- **KEK kullanarak çalışan bir VM'yi şifreleme:** Bu örnek, hem işletim sistemi hem de Veri birimlerini içeren -VolumeType parametresi için "Tümü" kullanır. Yalnızca işletim sistemi hacmini şifrelemek istiyorsanız, -VolumeType parametresi için "OS" kullanın.

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
    > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir diskte şifreleme yi etkinleştirme
 Azure CLI komutu, şifrelemeyi etkinleştirmek için komutu çalıştırdığınızda sizin için otomatik olarak yeni bir sıra sürümü sağlar. Örnek, birim türü parametresi için "Tümü" kullanır. Yalnızca işletim sistemi diskini şifreliyorsanız, birim türü parametresini işletim sistemi olarak değiştirmeniz gerekebilir. Powershell sözdiziminin aksine, CLI şifrelemeyi etkinleştirirken kullanıcının benzersiz bir dizi sürümü sağlamasını gerektirmez. CLI otomatik olarak kendi benzersiz sıra sürüm değerini oluşturur ve kullanır.   

-  **Çalışan vm'yi şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **KEK kullanarak çalışan bir VM'yi şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "All"
     ```


## <a name="disable-encryption"></a>Şifrelemeyi devre dışı
Azure PowerShell, Azure CLI veya Kaynak Yöneticisi şablonu kullanarak şifrelemeyi devre dışı kullanabilirsiniz. Windows VM'de hem işletim sistemi hem de veri diskleri şifrelenmiş durumdayken yalnızca veri diskinde şifrelemenin devre dışı bırakılması istenen sonucu vermeyebilir. Bunun yerine tüm disklerde şifrelemeyi devre dışı kakın.

- **Azure PowerShell ile disk şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Azure CLI ile şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type "all"
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı devre dışı:** 

    1. Windows VM şablonu [çalıştıran Disk şifrelemesini devre dışı takın'dan](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) **Azure'a Dağıt'ı** tıklatın.
    2. Abonelik, kaynak grubu, konum, VM, birim türü, yasal koşullar ve sözleşmeyi seçin.
    3.  Çalışan bir Windows VM'de disk şifrelemesini devre dışı kalmak için **Satın Al'ı** tıklatın. 

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Azure Disk Şifrelemesi aşağıdaki senaryolar, özellikler ve teknoloji için çalışmaz:

- Klasik VM oluşturma yöntemi yle oluşturulan temel kademe VM veya VM'leri şifreleme.
- Yazılım tabanlı RAID sistemleriyle yapılandırılan VM'lerin şifrelemi.
- 2016'dan önce Depolama Alanları Doğrudan (S2D) veya Windows Server sürümleriyle yapılandırılan VM'lerin şifrelemi, Windows Depolama Alanları ile yapılandırılır.
- Şirket içi anahtar yönetim sistemiyle entegrasyon.
- Azure Dosyaları (paylaşılan dosya sistemi).
- Ağ Dosya Sistemi (NFS).
- Dinamik hacimler.
- Her kapsayıcı için dinamik birimler oluşturan Windows Server kapsayıcıları.
- Kısa ömürlü işletim sistemi diskleri.
- DFS, GFS, DRDB ve CephFS gibi paylaşılan/dağıtılmış dosya sistemlerinin şifrelemesi (ancak bununla sınırlı değildir).
- Şifreli bir VM'yi başka bir aboneye taşıma.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)
- [Azure Disk Şifrelemesi örnek betikleri](disk-encryption-sample-scripts.md)
- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
