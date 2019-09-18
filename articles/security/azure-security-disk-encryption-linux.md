---
title: Linux Iaas sanal makineleri için Azure Disk şifrelemesini etkinleştirme
description: Bu makalede, Linux Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 09/16/2019
ms.custom: seodec18
ms.openlocfilehash: 03d50fbd9c3138f4d34dd748da50faefc3d8b24d
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71067834"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms"></a>Linux Iaas sanal makineleri için Azure Disk şifrelemesini etkinleştirme 

Çok sayıda disk şifreleme senaryoları etkinleştirebilirsiniz ve adımları senaryoya göre değişiklik gösterebilir. Aşağıdaki bölümlerde, Linux Iaas sanal makineleri için daha ayrıntılı senaryoları kapsar. Disk şifrelemeyi kullanabilmeniz için, [Azure disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md) doldurulmalıdır ve [Linux IaaS VM 'leri için ek önkoşullar](azure-security-disk-encryption-prerequisites.md#additional-prerequisites-for-linux-iaas-vms) bölümü incelenmelidir.

Ele bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya diskler şifrelenir önce yedekleyin. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'ini kullanabilirsiniz. Yedekleme ve şifrelenmiş Vm'leri geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../backup/backup-azure-vms-encryption.md) makalesi. 

>[!WARNING]
> - Bu VM 'yi şifrelemek için Azure [ad uygulaması Ile Azure disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) 'ni daha önce KULLANDıYSANıZ, VM 'nizi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Bu desteklenen bir senaryo olmadığından, bu şifrelenmiş VM 'de [Azure disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) 'ni kullanamazsınız. Bu, bu şifrelenmiş VM için AAD uygulamasından uzağa geçiş henüz desteklenmiyor.
 > - Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın.
> - Linux işletim sistemi birimlerini şifrelerken, sanal makinenin kullanılamaz kabul edilmesi gerekir. Şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engelleyen sorunlardan kaçınmak için şifreleme sürerken SSH oturum açma işlemlerini kesinlikle öneririz. İlerlemeyi denetlemek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) veya [VM Encryption Show](/cli/azure/vm/encryption#az-vm-encryption-show) komutları kullanılabilir. Bu işlemin, bir 30 GB işletim sistemi birimi için birkaç saat, ayrıca veri birimlerini şifrelemek için de ek süre gelmesi beklenmelidir. Şifreleme biçimi ALL seçeneği kullanılmamışsa, veri hacmi şifreleme süresi veri birimlerinin boyutuyla ve miktarıyla orantılıdır. 
> - Linux vm'lerinde şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningLinux"> </a> Var olan veya çalışan bir Iaas Linux VM üzerinde şifrelemeyi etkinleştir
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. Sanal makine uzantısı için şema bilgileri gerekiyorsa bkz [Linux uzantısı için Azure Disk şifrelemesi](../virtual-machines/extensions/azure-disk-enc-linux.md) makalesi.

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Bir var olan veya çalışan Azure CLI kullanarak Linux VM üzerinde şifrelemeyi etkinleştir 
Disk şifrelemesi yüklenerek ve kullanılarak şifrelenmiş VHD'nizi etkinleştirebilirsiniz [Azure CLI 2.0](/cli/azure) komut satırı aracı. [Azure Cloud Shell](../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Var olan veya Iaas sanal makineleri Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdaki CLI komutları kullanın:

Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

- **Çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **KEK kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br>
Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifrelendiğini doğrulayın:** IaaS VM 'sinin şifreleme durumunu denetlemek için [az VM Encryption Show](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> Bir var olan veya çalışan PowerShell kullanarak Linux VM üzerinde şifrelemeyi etkinleştir
Azure 'da çalışan bir IaaS sanal makinesinde şifrelemeyi etkinleştirmek için [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın. Diskler şifrelenmeden önce bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) alın ve/veya VM 'yi [Azure Backup](../backup/backup-azure-vms-encryption.md) yedekleyin. -SkipVmBackup parametresi, çalışan bir Linux sanal makinesini şifrelemek için PowerShell betiklerine zaten belirtildi.

-  **Çalışan bir VM şifrele:** Aşağıdaki komut dosyası değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin. Hangi diskleri şifrelediğiniz belirlemek için-VolumeType parametresini değiştirin.

     ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();  

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```
- **KEK kullanarak çalışan bir VM şifrele:** Veri diskleri ve işletim sistemi disk şifreleme, - VolumeType parametresi eklemeniz gerekebilir. 

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

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Disklerin şifrelendiğini doğrulayın:** IaaS VM 'sinin şifreleme durumunu denetlemek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet 'ini kullanın. 
    
     ```azurepowershell-interactive 
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifrelemesini devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.
     
     ```azurepowershell-interactive 
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> Var olan veya çalışan Iaas Linux VM'de bir şablonla şifrelemeyi etkinleştir

Var olan veya çalışan Iaas Linux VM'de Azure disk şifrelemesi kullanarak etkinleştirebilirsiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad).

1. Tıklayın **azure'a Dağıt** Azure Hızlı Başlangıç şablonu.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Tıklayın **Oluştur** var olan veya çalışan Iaas VM üzerinde şifrelemeyi etkinleştirmek için.

Resource Manager şablonu parametreleri, mevcut veya sanal makineleri çalıştırmak için aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemi çalıştırmak için sanal makinenin adı. |
| keyVaultName | BitLocker anahtarı karşıya yüklenmelidir anahtar kasasının adı. Bunu, cmdlet 'ini `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutunu kullanarak edinebilirsiniz`az keyvault list --resource-group "MyKeyVaultResourceGroupName"`|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı|
|  KeyEncryptionKeyURL | Oluşturulan BitLocker anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. Bu seçerseniz isteğe bağlı bir parametredir **nokek** UseExistingKek aşağı açılan listesinde. Seçerseniz **kek** UseExistingKek aşağı açılan listeden girmelisiniz _keyEncryptionKeyURL_ değeri. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _işletim sistemi_, _veri_, ve _tüm_. 
| forceUpdateTag | İşlemi çalıştırmak olması zorlamak gereken her zaman bir GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimi bölme önce tam işletim sistemi VHD'si kaplayacak şekilde işletim sistemi bölümü yeniden boyutlandırılması. |
| location | Tüm kaynaklar için konum. |



## <a name="encrypt-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri şifrele
[Azure sanal makine ölçek kümeleri](../virtual-machine-scale-sets/overview.md) oluşturma ve yönetme bir grup özdeş, yük dengeli sanal makineler sağlar. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Sanal makine ölçek kümeleri şifrelemek için CLI veya Azure PowerShell kullanın. Linux ölçek kümesi sanal makinelerinde yalnızca veri disklerinin şifrelenmesi desteklenir.

Linux ölçek kümesi veri disk şifreleme için bir toplu iş dosyası örneği bulunabilir [burada](https://github.com/Azure-Samples/azure-cli-samples/tree/master/disk-encryption/vmss). Bu örnek, bir kaynak grubu, Linux ölçek kümesi oluşturur, 5 GB'lık veri diskini bağlar ve sanal makine ölçek kümesi şifreler.

### <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Azure CLI ile sanal makine ölçek kümeleri şifrele

Kullanım [az vmss şifrelemeyi etkinleştirme](/cli/azure/vmss/encryption#az-vmss-encryption-enable) bir Windows sanal makine ölçek kümesi şifrelemesini etkinleştirmek için. Yükseltme İlkesi el ile ölçek kümesi, şifreleme ile Başlat [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. 

-  **Çalışan bir sanal makine ölçek kümesi şifrele**
    ```azurecli-interactive
    az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --volume-type DATA --disk-encryption-keyvault "MySecureVault"
    ```

-  **Şifreleme anahtarı sarmalama için KEK kullanarak çalışan bir sanal makine ölçek**
     ```azurecli-interactive
     az vmss encryption enable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss" --volume-type DATA --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault"
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Bir sanal makine ölçek kümesi için şifreleme durumu Al:** [Az VMSS ENCRYPTION Show](/cli/azure/vmss/encryption#az-vmss-encryption-show) komutunu kullanın

    ```azurecli-interactive
    az vmss encryption show --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesinde şifrelemeyi devre dışı bırak**: [Az VMSS ENCRYPTION Disable](/cli/azure/vmss/encryption#az-vmss-encryption-disable) komutunu kullanın
    ```azurecli-interactive
    az vmss encryption disable --resource-group "MyVMScaleSetResourceGroup" --name "MySecureVmss"
    ```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Azure PowerShell ile sanal makine ölçek kümeleri şifrele

Windows sanal makine ölçek kümesi üzerinde şifrelemeyi etkinleştirmek için [set-AzVmssDiskEncryptionExtension](/powershell/module/az.compute/set-azvmssdiskencryptionextension) cmdlet 'ini kullanın. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır.

-  **Çalışan bir sanal makine ölçek kümesi şifrelemek**:
      ```powershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
      $VmssName = "MySecureVmss";
      $KeyVaultName= "MySecureVault";
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -VolumeType Data -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
      ```

-  **Anahtarı kaydırmak IÇIN kek kullanarak çalışan bir sanal makine ölçek kümesini şifreleyin**:
      ```powershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMSSRGname = 'MyVMScaleSetResourceGroup';
      $VmssName = "MySecureVmss";
      $KeyVaultName= "MySecureVault";
      $keyEncryptionKeyName = "MyKeyEncryptionKey";
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $KeyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
      Set-AzVmssDiskEncryptionExtension -ResourceGroupName $VMSSRGname -VMScaleSetName $VmssName -VolumeType Data -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl  -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
      ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Bir sanal makine kümesi için şifreleme durumu Al**: [Get-Azvmssvmdiskencryptıon](/powershell/module/az.compute/get-azvmssvmdiskencryption) cmdlet 'ini kullanın.
    
    ```powershell
    Get-AzVmssVMDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

- **Bir sanal makine ölçek kümesinde şifrelemeyi devre dışı bırak**: [Disable-AzVmssDiskEncryption](/powershell/module/az.compute/disable-azvmssdiskencryption) cmdlet 'ini kullanın. 

    ```powershell
    Disable-AzVmssDiskEncryption -ResourceGroupName "MyVMScaleSetResourceGroup" -VMScaleSetName "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-linux-virtual-machine-scale-sets"></a>Linux sanal makine ölçek kümeleri için Azure Resource Manager şablonları

Linux sanal makine ölçek kümelerini şifrelemek veya şifresini çözmek için aşağıdaki Azure Resource Manager şablonları ve yönergeleri kullanın:

- [Linux sanal makine ölçek kümesi üzerinde şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)
- [Linux sanal makine ölçek kümesi üzerinde şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

     1. **Azure’a dağıt**’a tıklayın.
     2. Gerekli alanları doldurup hüküm ve koşulları kabul edin.
     3. Şablonu dağıtmak için **satın al** ' a tıklayın.

## <a name="bkmk_EFA"> </a>Linux Iaas sanal makinesi, veri diskleri için EncryptFormatAll özelliğini kullanın

**EncryptFormatAll** parametresi şifrelenmesini Linux veri diskleri için zaman azaltır. Belirli bir ölçüte uyan bölümleri (kendi geçerli dosya sistemi ile) biçimlendirilir. Ardından bunlar geri bu komutu yürütmeden önce olduğu için yeniden bağlanması. Ölçütleri karşılayan bir veri diski dışlamak istiyorsanız, komutu çalıştırmadan önce çıkarma.

 Bu komutu çalıştırdıktan sonra bağlı herhangi bir sürücü önceden biçimlendirilir. Ardından üzerine artık boş sürücü şifreleme katmanı başlatılacak. Bu seçenek belirlendiğinde VM'ye bağlı kısa ömürlü kaynak diski de şifrelenir. Kısa ömürlü sürücü sıfırlarsanız, yeniden biçimlendirildi ve sanal makine için Azure Disk şifrelemesi çözümü sonraki fırsatta yeniden şifrelenir. Kaynak disk şifrelendikten sonra, [Microsoft Azure Linux Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) kaynak diski yönetemez ve takas dosyasını etkinleştiremeyecektir, ancak takas dosyasını el ile yapılandırabilirsiniz.

>[!WARNING]
> Bir sanal makinenin veri birimlerinde gerekli verileri olduğunda EncryptFormatAll kullanılmamalıdır. Çıkarma tarafından şifrelemeden diskler dışarıda bırakılabilir. İlk önce bir test sanal makinesi üzerinde EncryptFormatAll denemek, özellik parametre ve VM üretimde denemeden önce itiraz hakkının düşmesi anlamak. EncryptFormatAll seçeneği veri diski biçimlendirir ve tüm veriler kaybolacak. Devam etmeden önce hariç tutmak istediğiniz disklerin düzgün şekilde takılmamış olduğunu doğrulayın. </br></br>
 >Bu parametre şifreleme ayarları güncelleştirilirken tutunun, gerçek şifrelemeyi önce yeniden başlatma için neden olabilir. Bu durumda, biçimlendirilmiş fstab dosyasını istemediğiniz diski kaldırmak ayrıca isteyeceksiniz. Benzer şekilde, şifreleme işlemi başlatmadan önce şifrelemek için fstab dosyasını biçimli istediğiniz bölümü eklemeniz gerekir. 

### <a name="bkmk_EFACriteria"> </a> EncryptFormatAll ölçütleri
Parametresi, ancak tüm bölümleri geçerlidir ve karşıladıkları sürece şifreler **tüm** aşağıdaki ölçütlerden biri: 
- Bir işletim sistemi/kök/önyükleme bölümü değildir
- Zaten şifreli değil
- BEK birimi değil
- Bir RAID birimine değil
- LVM'yi birimi değil
- Bağlanmıştır

RAID veya LVM birimin yerine RAID veya LVM birimin oluşturan diskleri şifreleme.

### <a name="bkmk_EFAPSH"> </a> Azure CLI ile EncryptFormatAll parametresini kullanın
Kullanım [az vm şifrelemeyi etkinleştirme](/cli/azure/vm/encryption#az-vm-encryption-enable) azure'da çalıştırılan Iaas sanal makine üzerinde şifrelemeyi etkinleştirmek için komutu.

-  **EncryptFormatAll kullanarak çalışan bir VM şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="bkmk_EFAPSH"> </a> Bir PowerShell cmdlet'i ile EncryptFormatAll parametresini kullanın
EncryptFormatAll parametresiyle [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın. 

**EncryptFormatAll kullanarak çalışan bir VM şifrele:** Örnek olarak, aşağıdaki komut dosyası değişkenlerinizi başlatır ve EncryptFormatAll parametresiyle set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin.
  
```azurepowershell
$KVRGname = 'MyKeyVaultResourceGroup';
$VMRGName = 'MyVirtualMachineResourceGroup';
$vmName = 'MySecureVM';
$KeyVaultName = 'MySecureVault';
$KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
```


### <a name="bkmk_EFALVM"> </a> Mantıksal birim Yöneticisi (LVM) ile EncryptFormatAll parametresini kullanın 
LVM'yi üzerinde crypt Kurulum öneririz. Aşağıdaki örnekler tüm için bağlama ve cihaz yolunu ne olursa olsun, kullanım örneği uygun ile değiştirin. Bu Kurulum şu şekilde yapabilirsiniz:

- Sanal Makineyi oluşturan veri diski ekleyin.
- Biçim, bağlamak ve bu disklerin fstab dosyaya ekleyin.

    1. Yeni eklenen diski biçimlendirin. Burada Azure tarafından oluşturulan çözümlemeyin kullanırız. Çözümlemeyin kullanarak cihaz adları değiştirme ilgili sorunları önler. Daha fazla bilgi için [cihaz adları sorun giderme sorunları](../virtual-machines/linux/troubleshoot-device-names-problems.md) makalesi.
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. Diskleri bağlayın.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. Fstab için ekleyin.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. Bu diskleri şifrelemek için set-AzVMDiskEncryptionExtension PowerShell cmdlet 'ini-EncryptFormatAll ile çalıştırın.

         ```azurepowershell-interactive
         $KeyVault = Get-AzKeyVault -VaultName "MySecureVault" -ResourceGroupName "MySecureGroup"
         
         Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri  -DiskEncryptionKeyVaultId $KeyVault.ResourceId -EncryptFormatAll -SkipVmBackup -VolumeType Data
         ```

    5. LVM'yi, bu yeni diskler üzerinde ayarlayın. VM önyükleme tamamlandıktan sonra şifrelenmiş sürücülerin kilidi olduğuna dikkat edin. Bu nedenle, LVM bağlama sonradan geciktirileceği da gerekir.


## <a name="bkmk_VHDpre"> </a> VHD ve şifreleme anahtarları müşteri şifrelenmiş oluşturulan yeni Iaas VM'ler
Bu senaryoda, PowerShell cmdlet'lerini veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenmiş bir Windows VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Önceden şifrelenmiş bir Linux VHD hazırlama](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin anlık görüntüsünü portaldan alınabilir veya [Azure Backup](../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 



### <a name="bkmk_VHDprePSH"> </a> Azure Iaas Vm'leri önceden şifrelenmiş VHD'ler ile şifrelemek için PowerShell kullanma 
[Set-AzVMOSDisk](/powershell/module/Az.Compute/Set-AzVMOSDisk#examples)PowerShell cmdlet 'ini kullanarak, şifrelenen VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki örnek, bazı ortak parametreler sunar. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin

Kullanarak yeni bir veri diski ekleyebilirsiniz [az vm disk ekleme](../virtual-machines/linux/add-disk.md), veya [Azure portalı üzerinden](../virtual-machines/linux/attach-disk-portal.md). Ayrıca şifreleme önce yeni eklenen veri diski ilk bağlama gerekir. Sürücü Şifreleme işlemi devam ederken kullanılamaz olduğundan veri sürücüsü şifrelenmesi istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin

 VM ile önceden şifrelenmişse sonra "Tüm" birim türü parametresi tüm kalmalıdır. Hem işletim sistemi ve veri diskleri içerir. VM "OS" ile bir birim türü önceden şifrelenmişse sonra birim türü parametre değiştirilmesi gereken tüm böylece hem işletim sistemi hem de yeni bir veri diski eklenecek. VM yalnızca "Data" birim türüyle şifrelenmişse, ardından bunu "Veri" aşağıda gösterildiği gibi kalır. Ekleme ve bir VM için yeni bir veri diski ekleme şifreleme için yeterli hazırlık değil. Yeni eklenen diski ayrıca olmalıdır biçimlendirilmiş ve şifreleme etkinleştirilmeden önce VM içinde düzgün bir şekilde bağlanır. Linux üzerinde disk ile/etc/fstab dosyasında takılmalıdır bir [kalıcı blok cihaz adı](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems).  

PowerShell sözdizimi aksine, şifreleme etkinleştirilirken benzersiz dizisi sürümü sağlamak kullanıcı CLI gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.

-  **Çalışan bir VM'ye veri birimlerini şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Veri birimleri KEK kullanarak çalışan bir sanal makinenin şifrele:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Linux için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir sıra sürüm belirtilmesi gerekiyor. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. Diskler şifrelenmeden önce bir [anlık görüntü](../virtual-machines/windows/snapshot-copy-managed-disk.md) alın ve/veya VM 'yi [Azure Backup](../backup/backup-azure-vms-encryption.md) yedekleyin. Yeni eklenen bir veri diskini şifrelemek için PowerShell betiklerine-skipVmBackup parametresi zaten belirtilmiş.
 

-  **Çalışan bir VM'ye veri birimlerini şifrele:** Aşağıdaki komut dosyası değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin. -VolumeType parametresi için kabul edilebilir değerler, işletim sistemi ve veri tümü. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

      ```azurepowershell
      $KVRGname = 'MyKeyVaultResourceGroup';
      $VMRGName = 'MyVirtualMachineResourceGroup';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;
      $sequenceVersion = [Guid]::NewGuid();

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion -skipVmBackup;
      ```
- **Veri birimleri KEK kullanarak çalışan bir sanal makinenin şifrele:** -VolumeType parametresi için kabul edilebilir değerler, işletim sistemi ve veri tümü. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

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

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion -skipVmBackup;
     ```

    >[!NOTE]
    > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-Guid]/resourceGroups/[KVresource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı]</br> Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


## <a name="disable-encryption-for-linux-vms"></a>Linux VM'ler için şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLI kullanarak şifreleme devre dışı bırakabilir veya bir Resource Manager şablonu ile. 

>[!IMPORTANT]
>Linux vm'lerinde Azure Disk şifreleme ile şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

- **Azure PowerShell ile disk şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [-VolumeType {ALL, DATA, OS}]
     ```

- **Azure CLı ile şifrelemeyi devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Resource Manager şablonu ile şifreleme devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [çalışan bir LINUX VM şablonunda şifrelemeyi devre dışı bırak '](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) i kullanın.
     1. **Azure’a dağıt**’a tıklayın.
     2. Abonelik, kaynak grubu, konum, VM, yasal koşulları ve Sözleşmesi'ni seçin.
     3.  Tıklayın **satın alma** çalışan bir Windows VM'de disk şifreleme devre dışı bırakmak için. 


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-windows.md)
