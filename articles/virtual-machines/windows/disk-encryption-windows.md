---
title: Windows VM'lerinde Azure Disk Şifrelemesi senaryoları
description: Bu makalede çeşitli senaryolar için Windows VM 'Leri için Microsoft Azure disk şifrelemeyi etkinleştirme yönergeleri sağlanır
author: msmbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: b423cc4cd933f84fccae5c2116be7abbdc288c67
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86203678"
---
# <a name="azure-disk-encryption-scenarios-on-windows-vms"></a>Windows VM'lerinde Azure Disk Şifrelemesi senaryoları

Windows sanal makineleri (VM 'Ler) için Azure disk şifrelemesi, işletim sistemi diskinin ve veri diskinin tam disk şifrelemesini sağlamak için Windows BitLocker özelliğini kullanır. Ayrıca, VolumeType parametresi hepsi olduğunda geçici diskin şifrelenmesini sağlar.

Azure disk şifrelemesi, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault ile tümleşiktir](disk-encryption-key-vault.md) . Hizmete genel bakış için bkz. [Windows VM 'leri Için Azure disk şifrelemesi](disk-encryption-overview.md).

Yalnızca [desteklenen VM boyutlarının ve işletim sistemlerinin](disk-encryption-overview.md#supported-vms-and-operating-systems)sanal makinelerine disk şifrelemesi uygulayabilirsiniz. Ayrıca aşağıdaki önkoşulları karşılamanız gerekir:

- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [grup ilkesi gereksinimleri](disk-encryption-overview.md#group-policy-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

>[!IMPORTANT]
> - Bir VM 'yi şifrelemek için Azure AD ile Azure disk şifrelemesi 'ni daha önce kullandıysanız, VM 'nizi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Ayrıntılar için bkz. [Azure AD ile Azure disk şifrelemesi (önceki sürüm)](disk-encryption-overview-aad.md) . 
>
> - Diskler şifrelenmeden önce [bir anlık görüntü alıp](snapshot-copy-managed-disk.md) /veya bir yedek oluşturmalısınız. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata oluşursa, kurtarma seçeneğinin mümkün olmasını güvence altına almanıza olanak tanır. Yönetilen disklere sahip VM 'Ler şifreleme gerçekleşmeden önce bir yedekleme gerektirir. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için [set-AzVMDiskEncryptionExtension cmdlet 'ini](/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanabilirsiniz. Şifrelenmiş VM 'Leri yedekleme ve geri yükleme hakkında daha fazla bilgi için bkz. [şifrelenmiş Azure VM 'Yi yedekleme ve geri yükleme](../../backup/backup-azure-vms-encryption.md). 
>
> - Şifrelemeyi şifrelemek veya devre dışı bırakmak bir sanal makinenin yeniden başlatılmasına neden olabilir.

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyip Azure 'a bağlanın

[!INCLUDE [disk-encryption-install-cli-powershell](../../../includes/disk-encryption-install-cli-powershell.md)]

## <a name="enable-encryption-on-an-existing-or-running-windows-vm"></a>Mevcut veya çalışan bir Windows VM 'de şifrelemeyi etkinleştirme
Bu senaryoda, Kaynak Yöneticisi şablonu, PowerShell cmdlet 'leri veya CLı komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. Sanal makine uzantısı için şema bilgilerine ihtiyacınız varsa, bkz. [Windows uzantısı Için Azure disk şifrelemesi](../extensions/azure-disk-enc-windows.md) makalesi.

### <a name="enable-encryption-on-existing-or-running-vms-with-azure-powershell"></a>Azure PowerShell ile mevcut veya çalışan VM 'lerde şifrelemeyi etkinleştirme 
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
    
- **Disk şifrelemesini devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. Windows VM'de hem işletim sistemi hem de veri diskleri şifrelenmiş durumdayken yalnızca veri diskinde şifrelemenin devre dışı bırakılması istenen sonucu vermeyebilir. Bunun yerine tüm disklerde şifrelemeyi devre dışı bırakın.

     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="enable-encryption-on-existing-or-running-vms-with-the-azure-cli"></a>Azure CLı ile mevcut veya çalışan VM 'lerde şifrelemeyi etkinleştirme
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

- **Şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Windows VM'de hem işletim sistemi hem de veri diskleri şifrelenmiş durumdayken yalnızca veri diskinde şifrelemenin devre dışı bırakılması istenen sonucu vermeyebilir. Bunun yerine tüm disklerde şifrelemeyi devre dışı bırakın.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```

### <a name="using-the-resource-manager-template"></a>Kaynak Yöneticisi şablonunu kullanma

[Çalışan bir WINDOWS VM 'yi şifrelemek için Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)kullanarak Azure 'da var olan veya çalışan IaaS Windows VM 'lerinde disk şifrelemeyi etkinleştirebilirsiniz.


1. Azure hızlı başlangıç şablonunda **Azure 'A dağıt**' a tıklayın.

2. Abonelik, kaynak grubu, konum, ayarlar, yasal koşullar ve Sözleşme ' yi seçin. Mevcut veya çalışan IaaS VM üzerinde şifrelemeyi etkinleştirmek için **satın al** ' a tıklayın.

Aşağıdaki tabloda mevcut veya çalışan VM 'Ler için Kaynak Yöneticisi şablonu parametreleri listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işleminin çalıştırılacağı sanal makinenin adı. |
| keyVaultName | BitLocker anahtarının yüklenmesi gereken anahtar kasasının adı. Bunu, cmdlet 'ini `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutunu kullanarak edinebilirsiniz`az keyvault list --resource-group "MyKeyVaultResourceGroup"`|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  keyEncryptionKeyURL 'Si | Anahtar şifreleme anahtarının https:// &lt; keykasaadı &gt; . Vault.Azure.net/Key/ &lt; anahtar-adı biçimindeki URL 'si &gt; . Bir KEK kullanmak istemiyorsanız, bu alanı boş bırakın. |
| Birimtürü | Şifreleme işleminin gerçekleştirildiği birimin türü. Geçerli değerler _Işletim sistemi_, _veri_ve _hepsi_. 
| forceUpdateTag | İşlemin zorla çalışması gereken her seferinde GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimini bölmeden önce IŞLETIM sistemi bölümünün tam işletim sistemi VHD 'SI kaplamaya yeniden boyutlandırılması gerekir. |
| location | Tüm kaynakların konumu. |


## <a name="new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Müşteri tarafından şifrelenen VHD ve şifreleme anahtarlarından oluşturulan yeni IaaS VM 'Leri

Bu senaryoda, önceden şifrelenen bir VHD 'den ve PowerShell cmdlet 'leri ya da CLı komutlarını kullanarak ilişkili şifreleme anahtarlarıyla yeni bir sanal makine oluşturabilirsiniz. 

[Önceden şifrelenmiş WINDOWS VHD hazırlama](disk-encryption-sample-scripts.md#prepare-a-pre-encrypted-windows-vhd)bölümündeki yönergeleri kullanın. Görüntü oluşturulduktan sonra, şifrelenmiş bir Azure VM oluşturmak için sonraki bölümdeki adımları kullanabilirsiniz.


### <a name="encrypt-vms-with-pre-encrypted-vhds-with-azure-powershell"></a>Azure PowerShell ile önceden şifrelenen VHD 'Ler ile VM 'Leri şifreleme
[Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples)PowerShell cmdlet 'ini kullanarak, şifrelenen VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki örnek size bazı ortak parametreler vermektedir. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myKVresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen bir veri diskinde şifrelemeyi etkinleştir
PowerShell kullanarak veya [Azure Portal aracılığıyla](attach-managed-disk-portal.md) [bir Windows sanal makinesine yeni bir disk ekleyebilirsiniz](attach-disk-ps.md). 

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
[!INCLUDE [disk-encryption-disable-encryption-powershell](../../../includes/disk-encryption-disable-powershell.md)]

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Azure disk şifrelemesi, aşağıdaki senaryolar, Özellikler ve teknolojiler için çalışmaz:

- Klasik VM oluşturma yöntemiyle oluşturulan temel katman VM veya VM 'Leri şifreleme.
- Yazılım tabanlı RAID sistemleriyle yapılandırılmış VM 'Leri şifreleme.
- Depolama Alanları Doğrudan (S2D) veya Windows Server sürümleri ile yapılandırılmış VM 'Leri Windows depolama alanları ile yapılandırılmış 2016 öncesi şifreleme.
- Şirket içi anahtar yönetim sistemiyle tümleştirme.
- Azure dosyaları (paylaşılan dosya sistemi).
- Ağ dosya sistemi (NFS).
- Dinamik birimler.
- Her kapsayıcı için dinamik birimler oluşturan Windows Server kapsayıcıları.
- Kısa ömürlü işletim sistemi diskleri.
- DFS, GFS, DRDB ve CephFS gibi paylaşılan/dağıtılmış dosya sistemlerinin şifrelenmesi (ancak bunlarla sınırlı olmamak üzere).
- Şifrelenmiş bir sanal makineyi başka bir aboneliğe veya bölgeye taşıma.
- Şifrelenmiş bir sanal makinenin görüntüsünü veya anlık görüntüsünü oluşturma ve ek VM 'Leri dağıtmak için kullanma.
- Gen2 VM 'Ler (bkz. [Azure 'da 2. nesil VM 'ler Için destek](generation-2.md#generation-1-vs-generation-2-capabilities))
- Lsv2 serisi VM 'Ler (bkz: [Lsv2-Series](../lsv2-series.md))
- Yazma Hızlandırıcısı disklere sahip, d serisi VM 'Ler.
- Müşteri tarafından yönetilen anahtarlarla ADE tarafından şifrelenen bir VM 'ye [sunucu tarafı şifrelemeyi](disk-encryption.md) uygulama ve tam tersi.
- [Müşteri tarafından yönetilen ANAHTARLARLA](disk-encryption.md)Ade ile ŞIFRELENEN bir VM 'yi sunucu tarafı şifrelemeye geçirme.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure disk şifrelemeye genel bakış](disk-encryption-overview.md)
- [Azure Disk Şifrelemesi örnek betikleri](disk-encryption-sample-scripts.md)
- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
