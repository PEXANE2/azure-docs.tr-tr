---
title: Azure AD App Linux IaaS VM ile Azure Disk Şifreleme (önceki sürüm)
description: Bu makalede, Linux IaaS VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme ile ilgili yönergeler sağlanmaktadır.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: ee365d37a957350fa8a68da0f34149d3210d6238
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970616"
---
# <a name="enable-azure-disk-encryption-with-azure-ad-on-linux-vms-previous-release"></a>Linux VM'lerde Azure AD ile Azure Disk Şifrelemesini etkinleştirme (önceki sürüm)

Azure Disk Şifreleme'nin yeni sürümü, VM disk şifrelemesini etkinleştirmek için Bir Azure Active Directory (Azure AD) uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, şifrelemeyi etkinleştir medeni sırasında Azure AD kimlik bilgilerini sağlamanız gerekmez. Tüm yeni VM'ler, yeni sürüm kullanılarak Azure AD uygulama parametreleri olmadan şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesinin nasıl etkinleştirilen hakkında talimatlar [için Linux VMS için Azure Disk](disk-encryption-linux.md)Şifrelemesi'ne bakın. Azure AD uygulama parametreleri ile zaten şifrelenmiş olan VM'ler hala desteklenir ve AAD sözdizimi ile korunmaya devam etmelidir.

Birçok disk şifreleme senaryosunu etkinleştirebilirsiniz ve adımlar senaryoya göre değişebilir. Aşağıdaki bölümler, linux altyapısı için bir hizmet (IaaS) VM'leri olarak senaryoları daha ayrıntılı olarak kapsamaktadır. Disk şifrelemeyi yalnızca [desteklenen VM boyutlarının ve işletim sistemlerinin](disk-encryption-overview.md#supported-vms-and-operating-systems)sanal makinelerine uygulayabilirsiniz. Ayrıca aşağıdaki ön koşulları karşılamanız gerekir:

- [VM'ler için ek gereksinimler](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ ve Grup Politikası](disk-encryption-overview-aad.md#networking-and-group-policy)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview-aad.md#encryption-key-storage-requirements)

Anlık [görüntü](snapshot-copy-managed-disk.md)alın, yedekleme yapın veya diskleri şifrelemeden önce her ikisini de yapın. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata oluşursa kurtarma seçeneğinin mümkün olmasını sağlar. Yönetilen disklere sahip VM'ler, şifreleme oluşmadan önce yedekleme gerektirir. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için Set-AzVMDiskEncryptionExtension cmdlet'ini kullanabilirsiniz. Şifreli VM'leri yedekleme ve geri yükleme hakkında daha fazla bilgi için [Azure Yedekleme'ye](../../backup/backup-azure-vms-encryption.md)bakın. 

>[!WARNING]
 > - Bu VM'yi şifrelemek için [Azure AD uygulamasıyla Azure Disk Şifreleme'yi](disk-encryption-overview-aad.md) daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz. Bu desteklenen bir senaryo olmadığından, bu şifreli VM için [Azure](disk-encryption-overview.md) AD uygulamasından geçiş yapmak henüz desteklenmediği anlamına gelir.
 > - Şifreleme sırlarının bölgesel sınırları aşmadığından emin olmak için Azure Disk Şifreleme'nin aynı bölgede birlikte bulunabilmesi için anahtar kasasına ve VM'lere ihtiyacı vardır. Şifrelenecek VM ile aynı bölgede bulunan bir anahtar kasası oluşturun ve kullanın.
 > - Linux işletim sistemi birimlerini şifrelediğinizde, işlem birkaç saat sürebilir. Linux işletim sistemi birimlerinin şifrelemesi veri birimlerinden daha uzun sürer.
> - Linux işletim sistemi birimlerini şifrelediğinizde, VM kullanılamıyor olarak kabul edilmelidir. Şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engellememek için şifreleme devam ederken SSH oturumlarından kaçınmanızı şiddetle öneririz. İlerlemeyi denetlemek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) veya [vm şifreleme göster](/cli/azure/vm/encryption#az-vm-encryption-show) komutlarını kullanın. Bu işlemin 30 GB'lık işletim sistemi hacmi için birkaç saat ve veri birimlerini şifrelemek için ek süre beklemeniz gerekir. Tüm seçenek **şifrelenme biçimi** kullanılmadığı sürece veri hacmi şifreleme süresi veri birimlerinin boyutu ve miktarıile orantılıdır. 
 > - Linux VM'lerinde şifrelemenin devre dışı bırakılması yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelenmişse, veri veya işletim sistemi birimlerinde desteklenmez. 

 

## <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm"></a><a name="bkmk_RunningLinux"> </a> Varolan veya çalışan bir IaaS Linux VM'de şifrelemeyi etkinleştirme

Bu senaryoda, Azure Kaynak Yöneticisi şablonunu, PowerShell cmdlets'i veya Azure CLI komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. 

>[!IMPORTANT]
 >Azure Disk Şifrelemesi'ni etkinleştirmeden önce yönetilen disk tabanlı bir VM örneğini anlık görüntü almak veya yedeklemek zorunludur. Azure portalından yönetilen diskin anlık görüntüsünü alabilir veya [Azure Yedekleme'yi](../../backup/backup-azure-vms-encryption.md)kullanabilirsiniz. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata durumunda kurtarma seçeneğinin mümkün olmasını sağlar. Yedekleme yapıldıktan sonra -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için Set-AzVMDiskEncryptionExtension cmdlet'ini kullanın. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıp bu parametre belirtilene kadar yönetilen disk tabanlı VM'lere karşı başarısız olur. 
>
>Şifrelemeyi şifrelemek veya devre dışı bırakmak VM'nin yeniden başlatılmasına neden olabilir. 
>

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-by-using-the-azure-cli"></a><a name="bkmk_RunningLinuxCLI"> </a>Azure CLI'yi kullanarak varolan veya çalışan bir Linux VM'de şifrelemeyi etkinleştirme 
[Azure CLI 2.0](/cli/azure) komut satırı aracını yükleyip kullanarak şifreli VHD'nizde disk şifrelemeyi etkinleştirebilirsiniz. [Azure Cloud Shell](../../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure'da mevcut veya çalışan IaaS Linux VM'lerinde şifrelemeyi etkinleştirmek için aşağıdaki CLI komutlarını kullanın:

Azure'da çalışan bir IaaS sanal makinede şifreleme sağlamak için [az vm şifreleme etkinleştir](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

-  **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:**
    
     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:**
    
     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

   >[!NOTE]
   > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name].</br> </br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id].

- **Disklerin şifrelenmiş olduğundan doğrulayın:** Bir IaaS VM'nin şifreleme durumunu kontrol etmek için [az vm şifreleme göster](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
         az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı kılabilir:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Şifrelemeyi devre dışı bırakmak yalnızca Linux VM'leri için veri hacimlerinde izin verilir.
    
     ```azurecli-interactive
         az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-by-using-powershell"></a><a name="bkmk_RunningLinuxPSH"> </a> PowerShell'i kullanarak varolan veya çalışan bir Linux VM'de şifrelemeyi etkinleştirme
Azure'da çalışan bir IaaS sanal makinede şifreleme sağlamak için [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'ini kullanın. Diskler şifrelenmeden önce bir [anlık görüntü](snapshot-copy-managed-disk.md) alın veya [Azure Yedekleme](../../backup/backup-azure-vms-encryption.md) ile VM'nin yedeğini yapın. -skipVmBackup parametresi, çalışan bir Linux VM'sini şifrelemek için PowerShell komut dosyasında zaten belirtilmiştir.

- **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi ortaya kaldırır ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırın. Kaynak grubu, VM, anahtar kasası, Azure AD uygulaması ve istemci sırrı zaten ön koşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MyKeyVaultResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID ve My-AAD-istemci-gizli değerleriniz ile değiştirin. Şifrelediğinizi belirtmek için -VolumeType parametresini değiştirin.

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
- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:** Azure Disk Şifreleme, şifrelemeyi etkinleştirirken oluşturulan disk şifreleme sırlarını sarmak için anahtar kasanızda varolan bir anahtar belirtmenize olanak tanır. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme anahtar kasasına yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır. Şifrelediğinizi belirtmek için -VolumeType parametresini değiştirin. 

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
  > Disk-şifreleme-anahtar tonoz parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[KVresource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name].</br> </br>
  Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]. 
    
- **Disklerin şifrelenmiş olduğundan doğrulayın:** Bir IaaS VM'nin şifreleme durumunu kontrol etmek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'ini kullanın. 
    
     ```azurepowershell-interactive 
         Get-AzVmDiskEncryptionStatus -ResourceGroupName MyVirtualMachineResourceGroup -VMName MySecureVM
     ```
    
- **Disk şifrelemeyi devre dışı devre dışı:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzureRmVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. Şifrelemeyi devre dışı bırakmak yalnızca Linux VM'leri için veri hacimlerinde izin verilir.
     
     ```azurepowershell-interactive 
         Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```


### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-with-a-template"></a><a name="bkmk_RunningLinux"> </a> Bir şablonla mevcut veya çalışan bir IaaS Linux VM'de şifrelemeyi etkinleştirme

[Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)kullanarak Azure'da varolan veya çalışan bir IaaS Linux VM'de disk şifrelemeyi etkinleştirebilirsiniz.

1. Azure hızlı başlatma şablonunda **Azure'a Dağıt'ı** seçin.

2. Aboneliği, kaynak grubunu, kaynak grubu konumunu, parametreleri, yasal koşulları ve anlaşmayı seçin. Varolan veya çalışan IaaS VM'de şifrelemeyi etkinleştirmek için **Oluştur'u** seçin.

Aşağıdaki tabloda, Azure AD istemci kimliği kullanan varolan veya çalışan VM'ler için Kaynak Yöneticisi şablon parametreleri listelenir:

| Parametre | Açıklama |
| --- | --- |
| AADClientID | Anahtar kasasına sır yazma izinleri olan Azure AD uygulamasının istemci kimliği. |
| AADClientSecret | Anahtar kasanıza sır yazma izinleri olan Azure AD uygulamasının istemci sırrı. |
| keyVaultName | Anahtarın yüklenmesi gereken anahtar kasasının adı. Azure CLI komutunu `az keyvault show --name "MySecureVault" --query KVresourceGroup`kullanarak alabilirsiniz. |
|  keyEncryptionKeyURL | Oluşturulan anahtarı şifrelemek için kullanılan anahtar şifreleme anahtarının URL'si. **UseExistingKek** açılır listesinde **nokek** seçerseniz bu parametre isteğe bağlıdır. **UseExistingKek** açılır listesinde **kek'i** seçerseniz, _anahtarEncryptionKeyURL_ değerini girmeniz gerekir. |
| volumeType | Şifreleme işleminin gerçekleştirildiği birim türü. Geçerli desteklenen değerler _işletim sistemi_ veya _tümüdir._ (Daha önceki önkoşullar bölümünde işletim sistemi ve veri diskleri için desteklenen Linux dağıtımları ve sürümlerine bakın.) |
| sequenceVersion | BitLocker işleminin sıralı sürümü. Aynı VM üzerinde her disk şifreleme işlemi yapıldığında bu sürüm numarasını artım. |
| vmName | Şifreleme işleminin yapılacağı VM'nin adı. |
| passphrase | Veri şifreleme anahtarı olarak güçlü bir parola yazın. |



## <a name="use-the-encryptformatall-feature-for-data-disks-on-linux-iaas-vms"></a><a name="bkmk_EFA"> </a>Linux IaaS VM'lerde veri diskleri için EncryptFormatAll özelliğini kullanın
EncryptFormatTüm parametre, Linux veri disklerinin şifrelenme süresini kısaltıyor. Belirli ölçütleri karşılayan bölümler biçimlendirilir (geçerli dosya sistemiyle). Sonra komuta emriolmadan önce bulundukları yere geri dönüyorlar. Ölçütleri karşılayan bir veri diskini hariç tutmak istiyorsanız, komutu çalıştırmadan önce bu diski atlayabilirsiniz.

 Bu komutu çalıştırdıktan sonra, daha önce monte edilmiş tüm sürücüler biçimlendirilir. Ardından şifreleme katmanı artık boş olan sürücünün üstünde başlar. Bu seçenek seçildiğinde, VM'ye bağlı geçici kaynak diski de şifrelenir. Geçici sürücü sıfırlanırsa, bir sonraki fırsatta Azure Disk Şifreleme çözümü tarafından VM için yeniden biçimlendirilir ve yeniden şifrelenir.

>[!WARNING]
> EncryptFormatAll, VM'nin veri hacimlerinde gerekli veriler olduğunda kullanılmamalıdır. Diskleri, bunları sayılarak şifrelemeden çıkarabilirsiniz. Üretim VM üzerinde denemeden önce özellik parametresini ve etkisini anlamak için önce bir test VM'deki EncryptFormatAll parametresini deneyin. EncryptFormatAll seçeneği veri diskini biçimlendirebilir, böylece üzerindeki tüm veriler kaybolur. Devam etmeden önce, hariç tutmak istediğiniz disklerin düzgün şekilde söküldinden doğrulayın. </br></br>
 >Şifreleme ayarlarını güncelleştirirken bu parametreyi ayarlarsanız, gerçek şifrelemeden önce yeniden başlatılabına neden olabilir. Bu durumda, fstab dosyasından biçimlendirin istemediğiniz diski de kaldırmak istiyorsunuz. Benzer şekilde, şifreleme işlemini başlatmadan önce fstab dosyasına şifreleme biçimlendirilmiş istediğiniz bölümü eklemeniz gerekir. 

### <a name="encryptformatall-criteria"></a><a name="bkmk_EFACriteria"> </a> EncryptFormatTüm kriterler
Parametre tüm bölümleri geçer ve aşağıdaki ölçütlerin *tümlerini* karşıladıkları sürece bunları şifreler: 
- Kök/işletim sistemi/önyükleme bölümü değil mi?
- Zaten şifrelenmemiş
- BEK hacmi değil mi
- RAID hacmi değil mi
- LVM hacmi değil mi
- Monte edilir

RAID veya LVM birimi yerine RAID veya LVM hacmini oluşturan diskleri şifreleyin.

### <a name="use-the-encryptformatall-parameter-with-a-template"></a><a name="bkmk_EFATemplate"> </a> EncryptFormatTüm parametreyi şablonla kullanma
EncryptFormatAll seçeneğini kullanmak için, Linux VM şifreleyen önceden varolan herhangi bir Azure Kaynak Yöneticisi şablonu kullanın ve AzureDiskEncryption kaynağının **Şifreleme İşlemi** alanını değiştirin.

1. Örnek olarak, [çalışan bir Linux IaaS VM'yi şifrelemek için Kaynak Yöneticisi şablonunu](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm)kullanın. 
2. Azure hızlı başlatma şablonunda **Azure'a Dağıt'ı** seçin.
3. Şifreleme **İşlemi** alanını **EnableEncryption'dan EnableEncryptionFormatAl'a** değiştirin. **EnableEncryptionFormatAl**
4. Aboneliği, kaynak grubunu, kaynak grubu konumunu, diğer parametreleri, yasal koşulları ve anlaşmayı seçin. Varolan veya çalışan IaaS VM'de şifrelemeyi etkinleştirmek için **Oluştur'u** seçin.


### <a name="use-the-encryptformatall-parameter-with-a-powershell-cmdlet"></a><a name="bkmk_EFAPSH"> </a> PowerShell cmdlet ile EncryptFormatTüm parametreyi kullanın
EncryptFormatAll parametresi ile [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'i kullanın.

**Çalışan bir VM'yi istemci sırrı nı ve EncryptFormatAll'ı kullanarak şifreleyin:** Örnek olarak, aşağıdaki komut dosyası değişkenlerinizi ortaya kaldırır ve EncryptFormatAll parametresi ile Set-AzVMDiskEncryptionExtension cmdlet çalıştırın. Kaynak grubu, VM, anahtar kasası, Azure AD uygulaması ve istemci sırrı zaten ön koşul olarak oluşturulmuş olmalıdır. MyKeyVaultResourceGroup, MyVirtualMachineResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID ve My-AAD-istemci-gizli değerleriniz ile değiştirin.
  
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


### <a name="use-the-encryptformatall-parameter-with-logical-volume-manager-lvm"></a><a name="bkmk_EFALVM"> </a> Mantıksal Birim Yöneticisi (LVM) ile EncryptFormatTüm parametreyi kullanın 
LVM-on-crypt kurulumu öneririz. Aşağıdaki örnekler için, cihaz yolunu ve montaj noktalarını kullanım durumunuza uygun olanlarla değiştirin. Bu kurulum aşağıdaki gibi yapılabilir:

- VM'yi oluşturacak veri disklerini ekleyin.
- Bu diskleri fstab dosyasına biçimlendirin, monte edin ve ekleyin.

    1. Yeni eklenen diski biçimlendirin. Azure tarafından oluşturulan sempozyum bağlantılarını burada kullanıyoruz. Symlinks'in kullanılması, aygıt adlarının değiştirilmesiyle ilgili sorunları önler. Daha fazla bilgi için sorun [giderme aygıt adları sorunları.](troubleshoot-device-names-problems.md)
    
             `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
        
    2. Diskleri dağın.
         
             `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`    
        
    3. fstab ekleyin.
         
            `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
        
    4. Bu diskleri şifrelemek için Set-AzVMDiskEncryptionExtension PowerShell cmdlet'i -EncryptFormatAll ile çalıştırın.
             ```azurepowershell-interactive
             Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
             ```
    5. Bu yeni disklerin üzerine LVM'yi ayarlayın. VM önyükleme bittikten sonra şifrelenmiş sürücülerin kilidinin açılmıştır. Yani, LVM montaj da daha sonra geciktirilmesi gerekecektir.




## <a name="new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a><a name="bkmk_VHDpre"> </a> Müşteri şifreli VHD ve şifreleme anahtarlarından oluşturulan yeni IaaS VM'ler
Bu senaryoda, Kaynak Yöneticisi şablonunu, PowerShell cmdlets'i veya CLI komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki bölümlerde Kaynak Yöneticisi şablonu ve CLI komutları daha ayrıntılı olarak açıklanabilir. 

Azure'da kullanılabilecek önceden şifrelenmiş görüntüleri hazırlamak için ekteki yönergeleri kullanın. Resim oluşturulduktan sonra, şifreli bir Azure VM oluşturmak için bir sonraki bölümdeki adımları kullanabilirsiniz.

* [Önceden şifrelenmiş Bir Linux VHD hazırlama](disk-encryption-sample-scripts.md)

>[!IMPORTANT]
 >Azure Disk Şifrelemesi'ni etkinleştirmeden önce yönetilen disk tabanlı bir VM örneğini anlık görüntü almak veya yedeklemek zorunludur. Yönetilen diskin anlık görüntüsünü portaldan alabilir veya [Azure Yedekleme'yi](../../backup/backup-azure-vms-encryption.md)kullanabilirsiniz. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata durumunda kurtarma seçeneğinin mümkün olmasını sağlar. Yedekleme yapıldıktan sonra -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için Set-AzVMDiskEncryptionExtension cmdlet'ini kullanın. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıp bu parametre belirtilene kadar yönetilen disk tabanlı VM'lere karşı başarısız olur. 
>
>Şifrelemeyi şifrelemek veya devre dışı bırakmak VM'nin yeniden başlatılmasına neden olabilir.



### <a name="use-azure-powershell-to-encrypt-iaas-vms-with-pre-encrypted-vhds"></a><a name="bkmk_VHDprePSH"> </a> IaaS VM'leri önceden şifrelenmiş VHD'lerle şifrelemek için Azure PowerShell'i kullanın 
PowerShell cmdlet [Set-AzVMOSDisk'i](/powershell/module/az.compute/set-azvmosdisk#examples)kullanarak şifreli VHD'nizde disk şifrelemesini etkinleştirebilirsiniz. Aşağıdaki örnek, bazı ortak parametreler verir. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen bir veri diskinde şifrelemeyi etkinleştirme
[Az vm disk ekle'yi](add-disk.md) kullanarak veya [Azure portalı üzerinden](attach-disk-portal.md)yeni bir veri diski ekleyebilirsiniz. Şifrelemeden önce, önce yeni eklenen veri diskini takmanız gerekir. Şifreleme devam ederken sürücü kullanılamaz hale geleceği için veri sürücüsünün şifrelemesini istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-the-azure-cli"></a>Azure CLI ile yeni eklenen bir diskte şifreleme yi etkinleştirme
 VM daha önce "Tümü" ile şifrelenmişse, birim türü parametresi Tümü olarak kalmalıdır. Tüm işletim sistemi ve veri diskleri içerir. VM daha önce bir birim "OS" türüyle şifrelenmişse, hem işletim sistemi hem de yeni veri diski dahil edilebilmek için birim türü parametresi Tümü olarak değiştirilmelidir. VM yalnızca "Veri" hacmi türüyle şifrelenmişse, burada gösterildiği gibi Veri olarak kalabilir. VM'ye yeni bir veri diski eklemek ve eklemek şifreleme için yeterli bir hazırlık değildir. Şifrelemeyi etkinleştirmeden önce yeni eklenen diskin vm içinde biçimlendirilmesi ve düzgün bir şekilde monte edilmesi gerekir. Linux'ta, disk /etc/fstab'a [kalıcı blok aygıt adı](troubleshoot-device-names-problems.md)ile monte edilmelidir. 

Powershell sözdiziminin aksine, CLI şifrelemeyi etkinleştirdiğinizde benzersiz bir dizi sürümü sağlamanızı gerektirmez. CLI otomatik olarak kendi benzersiz sıra sürüm değerini oluşturur ve kullanır.

-  **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:** 
    
     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:**

     ```azurecli-interactive
         az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir diskte şifreleme yi etkinleştirme
 Linux için yeni bir diski şifrelemek için Powershell'i kullandığınızda, yeni bir sıra sürümünün belirtilmesi gerekir. Dizi sürümü benzersiz olmalıdır. Aşağıdaki komut dosyası, sıra sürümü için bir GUID oluşturur. 
 

-  **Çalışan bir VM'yi istemci sırrını kullanarak şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi ortaya kaldırır ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırın. Kaynak grubu, VM, anahtar kasası, Azure AD uygulaması ve istemci sırrı zaten ön koşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MyKeyVaultResourceGroup, MySecureVM, MySecureVault, My-AAD-client-ID ve My-AAD-istemci-gizli değerleriniz ile değiştirin. -VolumeType parametresi işletim sistemi diski için değil, veri diskleri olarak ayarlanır. VM daha önce "OS" veya "Tümü" bir birim türü ile şifrelenmişse, hem işletim sistemi hem de yeni veri diski dahil edilebilmek için -VolumeType parametresi Tümü olarak değiştirilmelidir.

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
- **İstemci sırrını sarmak için KEK kullanarak çalışan bir VM'yi şifreleyin:** Azure Disk Şifreleme, şifrelemeyi etkinleştirirken oluşturulan disk şifreleme sırlarını sarmak için anahtar kasanızda varolan bir anahtar belirtmenize olanak tanır. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme anahtar kasasına yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır. -VolumeType parametresi işletim sistemi diski için değil, veri diskleri olarak ayarlanır. VM daha önce "OS" veya "Tümü" bir birim türü ile şifrelenmişse, hem işletim sistemi hem de yeni veri diski dahil edilebilmek için -VolumeType parametresi Tümü olarak değiştirilmelidir.

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
> Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]. </br> </br>
Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id].

## <a name="disable-encryption-for-linux-vms"></a>Linux VM'leri için şifrelemeyi devre dışı
Azure PowerShell, Azure CLI veya Kaynak Yöneticisi şablonu kullanarak şifrelemeyi devre dışı kullanabilirsiniz. 

>[!IMPORTANT]
>Linux VM'lerinde Azure Disk Şifrelemesi ile şifrelemenin devre dışı bırakılması yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelenmişse, veri veya işletim sistemi birimlerinde desteklenmez. 

- **Azure PowerShell ile disk şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
         Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Azure CLI ile şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
         az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı devre dışı:** Şifrelemeyi devre dışı kullanabilirsiniz, [çalışan bir Linux VM şablonundaki Devre Dışı Devre-Devre-Devre-Devre Tonu'nu](https://aka.ms/decrypt-linuxvm) kullanın.
     1. **Azure'a Dağıt'ı**seçin.
     2. Aboneliği, kaynak grubunu, konumu, VM'yi, yasal koşulları ve anlaşmayı seçin.
     3. Çalışan bir Windows VM'de disk şifrelemesini devre dışı kalmak için **Satın Al'ı** seçin. 


## <a name="next-steps"></a>Sonraki adımlar

- [Linux'a genel bakış için Azure Disk Şifrelemesi](disk-encryption-overview-aad.md)
- [Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md)
