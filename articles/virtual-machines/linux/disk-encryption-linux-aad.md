---
title: Azure AD uygulama Linux Iaas sanal makineleri (önceki sürüm) ile Azure Disk şifrelemesi
description: Bu makalede, Linux Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: ee365d37a957350fa8a68da0f34149d3210d6238
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970616"
---
# <a name="enable-azure-disk-encryption-with-azure-ad-on-linux-vms-previous-release"></a>Linux sanal makinelerinde Azure AD ile Azure disk şifrelemesini etkinleştirme (önceki sürüm)

Azure disk şifrelemesi 'nin yeni sürümü, VM disk şifrelemeyi etkinleştirmek için bir Azure Active Directory (Azure AD) uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, şifrelemeyi Etkinleştir adımını kullanarak artık Azure AD kimlik bilgilerini sağlamanız gerekmez. Yeni sürüm kullanılarak tüm yeni VM 'Ler Azure AD uygulama parametreleri olmadan şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirme hakkında yönergeler için bkz. [Linux VM 'leri Için Azure disk şifrelemesi](disk-encryption-linux.md). Azure AD uygulama parametreleriyle zaten şifrelenmiş VM 'Ler hala desteklenmektedir ve AAD sözdizimi ile sürdürülmeye devam etmelidir.

Birçok disk şifreleme senaryosunu etkinleştirebilirsiniz ve adımlar senaryoya göre farklılık gösterebilir. Aşağıdaki bölümlerde Linux hizmet olarak altyapı (IaaS) VM 'Leri için daha ayrıntılı senaryolar ele alınmaktadır. Yalnızca [desteklenen VM boyutlarının ve işletim sistemlerinin](disk-encryption-overview.md#supported-vms-and-operating-systems)sanal makinelerine disk şifrelemesi uygulayabilirsiniz. Ayrıca aşağıdaki önkoşulları karşılamanız gerekir:

- [VM 'Ler için ek gereksinimler](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ ve grup ilkesi](disk-encryption-overview-aad.md#networking-and-group-policy)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview-aad.md#encryption-key-storage-requirements)

Diskleri şifrelemeden önce bir [anlık görüntü](snapshot-copy-managed-disk.md)alın, yedekleme yapın veya her ikisini birden yapın. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'ini kullanabilirsiniz. Şifrelenmiş VM 'Leri yedekleme ve geri yükleme hakkında daha fazla bilgi için bkz. [Azure Backup](../../backup/backup-azure-vms-encryption.md). 

>[!WARNING]
 > - Bu VM 'yi şifrelemek için [Azure AD uygulaması Ile Azure disk şifrelemesi](disk-encryption-overview-aad.md) 'ni daha önce kullandıysanız, sanal makineyi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Bu şifrelenmiş sanal makinede [Azure disk şifrelemesi](disk-encryption-overview.md) 'ni kullanamazsınız çünkü bu desteklenen bir senaryo olmadığından, bu şifrelenmiş VM IÇIN Azure AD uygulamasından geçiş henüz desteklenmez.
 > - Şifreleme gizli dizileri arasında bölge dizileri olmadığından emin olmak için, Azure disk şifrelemesi, anahtar kasasının ve VM 'Lerin aynı bölgede birlikte bulunması gerekir. Şifrelenecek VM ile aynı bölgede olan bir Anahtar Kasası oluşturun ve kullanın.
 > - Linux işletim sistemi birimlerini şifrelerken, işlem birkaç saat sürebilir. Linux işletim sistemi birimlerinin şifrelenmesi için veri birimlerinden daha uzun sürme normaldir.
> - Linux işletim sistemi birimlerini şifrelerken, sanal makinenin kullanılamaz kabul edilmesi gerekir. Şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engellemeyi önlemek için şifreleme sürerken SSH oturumlarını önlemenize kesinlikle tavsiye ederiz. İlerlemeyi denetlemek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) veya [VM Encryption Show](/cli/azure/vm/encryption#az-vm-encryption-show) komutlarını kullanın. Bu işlemin 30 GB 'lık bir işletim sistemi birimi için birkaç saat, ayrıca veri birimlerini şifrelemek için de ek süre bekleniyor. Veri birimi şifreleme süresi, **şifreleme biçimi tümünü şifreleyin** seçeneği kullanılmadığı takdirde veri birimlerinin boyut ve miktarıyla orantılıdır. 
 > - Linux vm'lerinde şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelendiyse veri veya işletim sistemi birimlerinde desteklenmez. 

 

## <a name="bkmk_RunningLinux"></a> Mevcut veya çalışan IaaS Linux VM 'de şifrelemeyi etkinleştirme

Bu senaryoda, Azure Resource Manager şablonu, PowerShell cmdlet 'leri veya Azure CLı komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. 

>[!IMPORTANT]
 >Azure disk şifrelemesini etkinleştirmeden önce, bir anlık görüntü almak veya yönetilen disk tabanlı bir sanal makine örneğini yedeklemek zorunludur. Azure portal yönetilen diskin anlık görüntüsünü alabilirsiniz veya [Azure Backup](../../backup/backup-azure-vms-encryption.md)kullanabilirsiniz. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'ini kullanın. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
>Şifrelemeyi şifrelemek veya devre dışı bırakmak, sanal makinenin yeniden başlatılmasına neden olabilir. 
>

### <a name="bkmk_RunningLinuxCLI"> </a>Azure CLI kullanarak mevcut veya çalışan BIR Linux VM 'de şifrelemeyi etkinleştirme 
[Azure clı 2,0](/cli/azure) komut satırı aracını yükleyip kullanarak şifrelenmiş VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. [Azure Cloud Shell](../../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Var olan veya Iaas sanal makineleri Azure'da çalışan şifrelemeyi etkinleştirmek için aşağıdaki CLI komutları kullanın:

Azure 'da çalışan bir IaaS sanal makinesinde şifrelemeyi etkinleştirmek için [az VM Encryption Enable](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

-  **Çalışan bir VM 'yi bir istemci parolası kullanarak şifreleyin:**
    
     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **İstemci gizliliğini kaydırmak için KEK kullanarak çalışan bir VM 'yi şifreleyin:**
    
     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

   >[!NOTE]
   > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı].</br> </br> Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasa-adı]. kasa. Azure. net/Keys/[kekadı]/[kek-Unique-ID] gibi tam URI 'dir.

- **Disklerin şifrelendiğini doğrulayın:** IaaS VM 'sinin şifreleme durumunu denetlemek için [az VM Encryption Show](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
         az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.
    
     ```azurecli-interactive
         az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"></a> PowerShell kullanarak mevcut veya çalışan BIR Linux VM 'de şifrelemeyi etkinleştirme
Azure 'da çalışan bir IaaS sanal makinesinde şifrelemeyi etkinleştirmek için [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın. Diskler şifrelenmeden önce bir [anlık görüntü](snapshot-copy-managed-disk.md) alın veya [Azure Backup](../../backup/backup-azure-vms-encryption.md) VM yedeklemesi yapın. -SkipVmBackup parametresi, çalışan bir Linux sanal makinesini şifrelemek için PowerShell betiklerine zaten belirtildi.

- **Çalışan bir VM 'yi bir istemci parolası kullanarak şifreleyin:** Aşağıdaki betik, değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubu, VM, Anahtar Kasası, Azure AD uygulaması ve istemci parolası zaten ön koşullar olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MyKeyVaultResourceGroup, MySecureVM, Mysecurekasası, My-AAD-Client-ID ve My-AAD-Client-Secret değerlerini değerlerinizle değiştirin. Hangi diskleri şifrelediğiniz belirlemek için-VolumeType parametresini değiştirin.

     ```azurepowershell
         $VMRGName = 'MyVirtualMachineResourceGroup';
         $KVRGname = 'MyKeyVaultResourceGroup';
         $vmName = 'MySecureVM';
         $aadClientID = 'My-AAD-client-ID';
         $aadClientSecret = 'My-AAD-client-secret';
         $KeyVaultName = 'MySecureVault';
         $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
         $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
         $KeyVaultResourceId = $KeyVault.ResourceId;
         $sequenceVersion = [Guid]::NewGuid();
    
         Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```
- **İstemci gizliliğini kaydırmak IÇIN kek kullanarak çalışan bir VM 'Yi şifreleyin:** Azure disk şifrelemesi, şifreleme etkinleştirilirken oluşturulan disk şifreleme gizli dizilerini kaydırmak için Anahtar Kasanızda mevcut bir anahtar belirtmenize olanak tanır. Anahtar şifreleme anahtarı belirtildiğinde Azure disk şifrelemesi, anahtar kasasına yazmadan önce şifreleme gizli dizilerini kaydırmak için bu anahtarı kullanır. Hangi diskleri şifrelediğiniz belirlemek için-VolumeType parametresini değiştirin. 

     ```azurepowershell
         $KVRGname = 'MyKeyVaultResourceGroup';
         $VMRGName = 'MyVirtualMachineResourceGroup';
         $aadClientID = 'My-AAD-client-ID';
         $aadClientSecret = 'My-AAD-client-secret';
         $KeyVaultName = 'MySecureVault';
         $keyEncryptionKeyName = 'MyKeyEncryptionKey';
         $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
         $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
         $KeyVaultResourceId = $KeyVault.ResourceId;
         $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
         $sequenceVersion = [Guid]::NewGuid();
    
         Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType '[All|OS|Data]' -SequenceVersion $sequenceVersion -skipVmBackup;
     ```

  >[!NOTE]
  > Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[KVresource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı].</br> </br>
  Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasa-adı]. kasa. Azure. net/Keys/[kekadı]/[kek-Unique-ID] gibi tam URI 'dir. 
    
- **Disklerin şifrelendiğini doğrulayın:** IaaS VM 'sinin şifreleme durumunu denetlemek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet 'ini kullanın. 
    
     ```azurepowershell-interactive 
         Get-AzVmDiskEncryptionStatus -ResourceGroupName MyVirtualMachineResourceGroup -VMName MySecureVM
     ```
    
- **Disk şifrelemesini devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azurermvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.
     
     ```azurepowershell-interactive 
         Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"></a> Bir şablonla mevcut veya çalışan bir IaaS Linux sanal makinesinde şifrelemeyi etkinleştirme

[Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)kullanarak Azure 'da var olan veya çalışan bir IaaS Linux sanal makinesinde disk şifrelemeyi etkinleştirebilirsiniz.

1. Azure hızlı başlangıç şablonunda **Azure 'A dağıt** ' ı seçin.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Mevcut veya çalışan IaaS sanal makinesinde şifrelemeyi etkinleştirmek için **Oluştur** ' u seçin.

Var olan veya bir Azure AD İstemci Kimliğini kullanan sanal makineleri çalıştırmak için Resource Manager şablon parametreleri aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| Aadclientıd | Bir anahtar kasasına gizli anahtarları yazmak için izne sahip olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Gizli anahtar kasanız için yazma iznine sahip Azure AD uygulamasının istemci gizli anahtarı. |
| keyVaultName | Anahtarı karşıya yüklenmelidir anahtar kasasının adı. Azure CLı komutu `az keyvault show --name "MySecureVault" --query KVresourceGroup`kullanarak bunu edinebilirsiniz. |
|  KeyEncryptionKeyURL | Oluşturulan anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarı URL'si. **UseExistingKek** açılır listesinde **nokek** ' ı seçerseniz bu parametre isteğe bağlıdır. **UseExistingKek** açılır listesinde **kek** ' yi seçerseniz _Keyencryptionkeyurl_ değerini girmeniz gerekir. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Desteklenen geçerli değerler _OS_ veya _All_. (Önkoşul bölümünde daha önce desteklenen Linux dağıtımlarını ve işletim sistemi ve veri diskleri sürümlerini inceleyin.) |
| SequenceVersion | BitLocker işlemi dizisi sürümü. Bu sürüm numarası, aynı sanal makinede her bir disk şifreleme işlemi gerçekleştirildiğinde artırın. |
| vmName | Şifreleme işlemi gerçekleştirilecek olan bir VM adı. |
| passphrase | Veri şifreleme anahtarı güçlü bir parola yazın. |



## <a name="bkmk_EFA"> </a>Linux IaaS VM 'lerinde veri diskleri Için EncryptFormatAll özelliğini kullanın
EncryptFormatAll parametresi, Linux veri disklerinin şifrelenme süresini azaltır. Belirli ölçütlere uyan bölümler biçimlendirilir (geçerli dosya sistemiyle birlikte). Ardından, komut yürütmeden önce oldukları yere geri takılırlar. Ölçütlere uyan bir veri diskini dışlamak istiyorsanız, komutu çalıştırmadan önce bunu kaldırabilirsiniz.

 Bu komutu çalıştırdıktan sonra, daha önce bağlanan tüm sürücüler biçimlendirilir. Ardından, şifreleme katmanı artık boş sürücünün üstünde başlar. Bu seçenek belirlendiğinde, VM 'ye bağlı olan kısa ömürlü kaynak diski de şifrelenir. Kısa ömürlü sürücü sıfırlandığında, sonraki fırsatta Azure disk şifrelemesi çözümü tarafından VM için yeniden biçimlendirilir ve yeniden şifrelenir.

>[!WARNING]
> Bir VM 'nin veri birimlerinde gerekli veriler olduğunda, EncryptFormatAll kullanılmamalıdır. Diskleri şifrelemeden hariç tutun. Özellik parametresini ve uygulamayı üretim VM 'sinde denemeden önce anlamak için öncelikle bir test sanal makinesinde EncryptFormatAll parametresini deneyin. EncryptFormatAll seçeneği veri diskini biçimlendirir, bu nedenle üzerindeki tüm veriler kaybedilir. Devam etmeden önce, dışlamak istediğiniz disklerin düzgün şekilde takılmamış olduğunu doğrulayın. </br></br>
 >Şifreleme ayarlarını güncelleştirdiğinizde bu parametreyi ayarlarsanız, gerçek şifrelemeden önce yeniden başlatmaya neden olabilir. Bu durumda, fstab dosyasından biçimlendirilmesini istemediğiniz diski de kaldırmak isteyeceksiniz. Benzer şekilde, şifreleme işlemini çalıştırmadan önce fstab dosyasına şifrelemek istediğiniz bölümü eklemeniz gerekir. 

### <a name="bkmk_EFACriteria"></a> Encryptformatall ölçütü
Parametresi tüm bölümler arasında gider ve aşağıdaki ölçütlerin *Tümünü* karşılayan sürece bunları şifreler: 
- Bir işletim sistemi/kök/önyükleme bölümü değildir
- Zaten şifreli değil
- BEK birimi değil
- Bir RAID birimine değil
- LVM'yi birimi değil
- Bağlanmıştır

RAID veya LVM birimin yerine RAID veya LVM birimin oluşturan diskleri şifreleme.

### <a name="bkmk_EFATemplate"></a> Bir şablonla EncryptFormatAll parametresini kullanın
EncryptFormatAll seçeneğini kullanmak için, bir Linux sanal makinesini şifreleyen ve AzureDiskEncryption kaynağı için **Encryptionoperation** alanını değiştiren önceden var olan Azure Resource Manager şablonu kullanın.

1. Örnek olarak, [çalışan bir Linux IaaS VM 'sini şifrelemek için Kaynak Yöneticisi şablonunu](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm)kullanın. 
2. Azure hızlı başlangıç şablonunda **Azure 'A dağıt** ' ı seçin.
3. **Encryptionoperation** alanını **Enableencryption** Iken **enableencryptionformatal**olarak değiştirin.
4. Abonelik, kaynak grubu, kaynak grubu konumu, diğer parametreler, yasal koşulları ve Sözleşmesi'ni seçin. Mevcut veya çalışan IaaS sanal makinesinde şifrelemeyi etkinleştirmek için **Oluştur** ' u seçin.


### <a name="bkmk_EFAPSH"></a> Bir PowerShell cmdlet 'ı Ile EncryptFormatAll parametresini kullanma
EncryptFormatAll parametresiyle [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın.

**Çalışan bir VM 'yi bir istemci gizli anahtarı ve EncryptFormatAll kullanarak şifreleyin:** Örnek olarak, aşağıdaki betik değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini EncryptFormatAll parametresiyle çalıştırır. Kaynak grubu, VM, Anahtar Kasası, Azure AD uygulaması ve istemci parolası zaten ön koşullar olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, Mysecurekasası, My-AAD-Client-ID ve My-AAD-Client-Secret değerlerini değerlerinizle değiştirin.
  
   ```azurepowershell
     $KVRGname = 'MyKeyVaultResourceGroup';
     $VMRGName = 'MyVirtualMachineResourceGroup'; 
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
   ```


### <a name="bkmk_EFALVM"></a> Mantıksal birim Yöneticisi (LVM) Ile EncryptFormatAll parametresini kullanın 
LVM'yi üzerinde crypt Kurulum öneririz. Aşağıdaki örneklerin tümü için, cihaz yolu ve bağlama noktalarını kullanım örnekinizdeki her türlü uygun olacak şekilde değiştirin. Bu Kurulum şu şekilde yapabilirsiniz:

- Sanal Makineyi oluşturan veri diski ekleyin.
- Biçim, bağlamak ve bu disklerin fstab dosyaya ekleyin.

    1. Yeni eklenen diski biçimlendirin. Burada Azure tarafından oluşturulan çözümlemeyin kullanırız. Çözümlemeyin kullanarak cihaz adları değiştirme ilgili sorunları önler. Daha fazla bilgi için bkz. [Cihaz adı sorunlarını giderme](troubleshoot-device-names-problems.md).
    
             `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
        
    2. Diskleri bağlayın.
         
             `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`    
        
    3. Fstab için ekleyin.
         
            `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
        
    4. Bu diskleri şifrelemek için set-AzVMDiskEncryptionExtension PowerShell cmdlet 'ini-EncryptFormatAll ile çalıştırın.
             ```azurepowershell-interactive
             Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
             ```
    5. LVM'yi, bu yeni diskler üzerinde ayarlayın. VM önyükleme tamamlandıktan sonra şifrelenmiş sürücülerin kilidi olduğuna dikkat edin. Bu nedenle, LVM bağlama sonradan geciktirileceği da gerekir.




## <a name="bkmk_VHDpre"></a> Müşteri tarafından şifrelenen VHD ve şifreleme anahtarlarından oluşturulan yeni IaaS VM 'leri
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. Aşağıdaki bölümlerde Resource Manager şablonu ve CLI komutları daha ayrıntılı olarak açıklanmaktadır. 

Yönergeleri ekte, Azure'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlama için kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenen bir Linux VHD hazırlama](disk-encryption-sample-scripts.md)

>[!IMPORTANT]
 >Azure disk şifrelemesini etkinleştirmeden önce, bir anlık görüntü almak veya yönetilen disk tabanlı bir sanal makine örneğini yedeklemek zorunludur. Portaldan yönetilen diskin anlık görüntüsünü alabilirsiniz veya [Azure Backup](../../backup/backup-azure-vms-encryption.md)kullanabilirsiniz. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'ini kullanın. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
>Şifrelemeyi şifrelemek veya devre dışı bırakmak, sanal makinenin yeniden başlatılmasına neden olabilir.



### <a name="bkmk_VHDprePSH"></a> IaaS VM 'lerini önceden şifrelenen VHD 'lerle şifrelemek için Azure PowerShell kullanma 
[Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples)PowerShell cmdlet 'ini kullanarak, şifrelenen VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki örnek size bazı ortak parametreler vermektedir. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin
[Az VM disk Attach](add-disk.md) veya [Azure Portal aracılığıyla](attach-disk-portal.md)yeni bir veri diski ekleyebilirsiniz. Ayrıca şifreleme önce yeni eklenen veri diski ilk bağlama gerekir. Şifreleme sürerken sürücü kullanılamaz olacağı için veri sürücüsünün şifrelemesini istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-the-azure-cli"></a>Azure CLı ile yeni eklenen bir diskte şifrelemeyi etkinleştirme
 VM daha önce "All" ile şifrelendiyse,--Volume-Type parametresi Tümü kalmalıdır. Hem işletim sistemi ve veri diskleri içerir. VM daha önce "OS" birim türüyle şifrelendiyse, hem işletim sistemi hem de yeni veri diskinin dahil edilmesini sağlamak için--Volume-Type parametresinin tümü olarak değiştirilmesi gerekir. VM yalnızca "Data" birim türüyle şifrelendiyse, burada gösterildiği gibi veri kalabilir. Yeni bir veri diski eklemek ve bir VM 'ye eklemek, şifreleme için yeterli hazırlık gerektirmez. Şifrelemeyi etkinleştirmeden önce, yeni eklenen diskin da biçimlendirilmesi ve sanal makine içinde düzgün şekilde bağlanması gerekir. Linux 'ta, diskin [kalıcı bir blok cihaz adıyla](troubleshoot-device-names-problems.md)/etc/fstab 'e bağlanması gerekir. 

PowerShell sözdiziminin aksine, şifrelemeyi etkinleştirdiğinizde CLı, benzersiz bir sıra sürümü sağlamanızı gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.

-  **Çalışan bir VM 'yi bir istemci parolası kullanarak şifreleyin:** 
    
     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **İstemci gizliliğini kaydırmak için KEK kullanarak çalışan bir VM 'yi şifreleyin:**

     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Linux için yeni bir disk şifrelemek üzere PowerShell kullandığınızda yeni bir sıra sürümünün belirtilmesi gerekir. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki betik, dizi sürümü için bir GUID üretir. 
 

-  **Çalışan bir VM 'yi bir istemci parolası kullanarak şifreleyin:** Aşağıdaki betik, değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubu, VM, Anahtar Kasası, Azure AD uygulaması ve istemci parolası zaten ön koşullar olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MyKeyVaultResourceGroup, MySecureVM, Mysecurekasası, My-AAD-Client-ID ve My-AAD-Client-Secret değerlerini değerlerinizle değiştirin. -VolumeType parametre veri diskleri ve işletim sistemi diski için ayarlanır. VM daha önce "OS" veya "All" birim türüyle şifrelendiyse, hem işletim sistemi hem de yeni veri diskinin dahil edilmesini sağlamak için-VolumeType parametresinin tümü olarak değiştirilmesi gerekir.

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
         $sequenceVersion = [Guid]::NewGuid();
    
         Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
     ```
- **İstemci gizliliğini kaydırmak IÇIN kek kullanarak çalışan bir VM 'Yi şifreleyin:** Azure disk şifrelemesi, şifreleme etkinleştirilirken oluşturulan disk şifreleme gizli dizilerini kaydırmak için Anahtar Kasanızda mevcut bir anahtar belirtmenize olanak tanır. Anahtar şifreleme anahtarı belirtildiğinde Azure disk şifrelemesi, anahtar kasasına yazmadan önce şifreleme gizli dizilerini kaydırmak için bu anahtarı kullanır. -VolumeType parametre veri diskleri ve işletim sistemi diski için ayarlanır. VM daha önce "OS" veya "All" birim türüyle şifrelendiyse, hem işletim sistemi hem de yeni veri diskinin dahil edilmesini sağlamak için-VolumeType parametresinin tümü olarak değiştirilmesi gerekir.

     ```azurepowershell
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
         $sequenceVersion = [Guid]::NewGuid();
    
         Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
     ```


>[!NOTE]
> Disk-Encryption-keykasası parametresinin değeri için sözdizimi tam tanımlayıcı dizesidir:/Subscriptions/[Subscription-ID-GUID]/resourceGroups/[resource-Group-Name]/providers/Microsoft.KeyVault/vaults/[keykasaadı]. </br> </br>
Anahtar şifreleme-anahtar parametresinin değeri için sözdizimi, KEK: https://[keykasa-adı]. kasa. Azure. net/Keys/[kekadı]/[kek-Unique-ID] gibi tam URI 'dir.

## <a name="disable-encryption-for-linux-vms"></a>Linux VM'ler için şifrelemeyi devre dışı bırakma
Azure PowerShell, Azure CLı veya Kaynak Yöneticisi şablonunu kullanarak şifrelemeyi devre dışı bırakabilirsiniz. 

>[!IMPORTANT]
>Linux vm'lerinde Azure Disk şifreleme ile şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelendiyse veri veya işletim sistemi birimlerinde desteklenmez. 

- **Azure PowerShell ile disk şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azurermvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. 
     ```azurepowershell-interactive
         Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Azure CLI ile şifrelemeyi devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
         az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [çalışan bir LINUX VM şablonunda şifrelemeyi devre dışı bırak](https://aka.ms/decrypt-linuxvm) ' ı kullanın.
     1. **Azure 'A dağıt**' ı seçin.
     2. Abonelik, kaynak grubu, konum, VM, yasal koşulları ve Sözleşmesi'ni seçin.
     3. Çalışan bir Windows VM 'de disk şifrelemeyi devre dışı bırakmak için **satın al** ' ı seçin 


## <a name="next-steps"></a>Sonraki adımlar

- [Linux için Azure disk şifrelemesi genel bakış](disk-encryption-overview-aad.md)
- [Azure AD ile Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md)
