---
title: Linux VM 'lerinde Azure disk şifrelemesi senaryoları
description: Bu makale çeşitli senaryolar için Linux VM 'Leri için Microsoft Azure disk şifrelemeyi etkinleştirme hakkında yönergeler sağlar
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 19dcfb96f29939fd92f49ba288ddb6d9264e0f9a
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970590"
---
# <a name="azure-disk-encryption-scenarios-on-linux-vms"></a>Linux VM 'lerinde Azure disk şifrelemesi senaryoları

Azure disk şifrelemesi, Azure sanal makinelerinin (VM 'Ler) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Linux 'un DM-Crypt özelliğini kullanır ve disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için Azure Key Vault ile tümleşiktir. Hizmete genel bakış için bkz. [Linux VM 'leri Için Azure disk şifrelemesi](disk-encryption-overview.md).

Birçok disk şifreleme senaryosu vardır ve adımlar senaryoya göre farklılık gösterebilir. Aşağıdaki bölümlerde Linux sanal makinelerinde daha ayrıntılı senaryolar ele alınmaktadır.

Yalnızca [desteklenen VM boyutlarının ve işletim sistemlerinin](disk-encryption-overview.md#supported-vms-and-operating-systems)sanal makinelerine disk şifrelemesi uygulayabilirsiniz. Ayrıca aşağıdaki önkoşulları karşılamanız gerekir:

- [VM 'Ler için ek gereksinimler](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

Her durumda, [bir anlık görüntü](snapshot-copy-managed-disk.md) almalısınız ve/veya diskler şifrelenmeden önce bir yedek oluşturmalısınız. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata oluşması durumunda olası olduğundan emin olun. Şifreleme gerçekleşmeden önce yönetilen disklere sahip VM'ler yedeklemesini gerektirir. Bir yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için [set-AzVMDiskEncryptionExtension cmdlet 'ini](/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanabilirsiniz. Şifrelenmiş VM 'Leri yedekleme ve geri yükleme hakkında daha fazla bilgi için [Azure Backup](../../backup/backup-azure-vms-encryption.md) makalesine bakın. 

>[!WARNING]
> - Bir VM 'yi şifrelemek için Azure AD ile Azure disk şifrelemesi 'ni daha önce kullandıysanız, VM 'nizi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Ayrıntılar için bkz. [Azure AD ile Azure disk şifrelemesi (önceki sürüm)](disk-encryption-overview-aad.md) . 
>
> - Linux işletim sistemi birimlerini şifrelerken, sanal makinenin kullanılamaz kabul edilmesi gerekir. Şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engelleyen sorunlardan kaçınmak için şifreleme sürerken SSH oturum açma işlemlerini kesinlikle öneririz. İlerlemeyi denetlemek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) PowerShell cmdlet 'ini veya [VM şifrelemesi Show](/cli/azure/vm/encryption#az-vm-encryption-show) CLI komutunu kullanın. Bu işlemin, bir 30 GB işletim sistemi birimi için birkaç saat, ayrıca veri birimlerini şifrelemek için de ek süre gelmesi beklenmelidir. Şifreleme biçimi ALL seçeneği kullanılmamışsa, veri hacmi şifreleme süresi veri birimlerinin boyutuyla ve miktarıyla orantılıdır. 
> - Linux vm'lerinde şifreleme devre dışı bırakıldığında, yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifreli değilse veri veya işletim sistemi birimleri üzerinde desteklenmiyor.  

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyip Azure 'a bağlanın

Azure disk şifrelemesi, [Azure CLI](/cli/azure) ve [Azure PowerShell](/powershell/azure/new-azureps-module-az)aracılığıyla etkinleştirilebilir ve yönetilebilir. Bunu yapmak için araçları yerel olarak yüklemeli ve Azure aboneliğinize bağlamanız gerekir.

### <a name="azure-cli"></a>Azure CLI

[Azure clı 2,0](/cli/azure) , Azure kaynaklarını yönetmeye yönelik bir komut satırı aracıdır. CLI, esnek bir şekilde verileri sorgulamak, engelleyici olmayan süreçler olarak uzun süreli işlemleri desteklemek ve kolay bir komut dosyası yapmak için tasarlanmıştır. [Azure CLI 'Yı yüklemeye](/cli/azure/install-azure-cli?view=azure-cli-latest)ilişkin adımları izleyerek yerel olarak yükleyebilirsiniz.

 

Azure [CLI Ile Azure hesabınızda oturum açmak](/cli/azure/authenticate-azure-cli)için [az Login](/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanın.

```azurecli
az login
```

Altında oturum açmak için bir kiracı seçmek istiyorsanız bu seçeneği kullanın:
    
```azurecli
az login --tenant <tenant>
```

Birden çok aboneliğiniz varsa ve belirli bir tane belirtmek istiyorsanız, [az Account List](/cli/azure/account#az-account-list) komutuyla abonelik listenizi alın ve [az Account set](/cli/azure/account#az-account-set)ile belirtin.
     
```azurecli
az account list
az account set --subscription "<subscription name or ID>"
```

Daha fazla bilgi için bkz. [Azure CLI ile çalışmaya başlama 2,0](/cli/azure/get-started-with-azure-cli). 

### <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell az Module](/powershell/azure/new-azureps-module-az) , Azure kaynaklarınızı yönetmek için [Azure Resource Manager](../../azure-resource-manager/management/overview.md) modelini kullanan bir cmdlet kümesi sağlar. Bunu tarayıcınızda [Azure Cloud Shell](../../cloud-shell/overview.md)ile kullanabilir veya [Azure PowerShell modülünü yüklemeye](/powershell/azure/install-az-ps)ilişkin yönergeleri kullanarak yerel makinenize yükleyebilirsiniz. 

Yerel olarak yüklü zaten varsa, Azure Disk şifrelemesini yapılandırmak için en son Azure PowerShell SDK'sı sürümü kullandığınızdan emin olun. [Azure PowerShell sürümünün](https://github.com/Azure/azure-powershell/releases)en son sürümünü indirin.

[Azure hesabınızda Azure PowerShell oturum açmak](/powershell/azure/authenticate-azureps?view=azps-2.5.0)için [Connect-azaccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) cmdlet 'ini kullanın.

```powershell
Connect-AzAccount
```

Birden çok aboneliğiniz varsa ve bir tane belirtmek istiyorsanız, [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) cmdlet 'ini kullanarak bunları listeleyin ve ardından [set-azcontext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0) cmdlet 'ini kullanın:

```powershell
Set-AzContext -Subscription -Subscription <SubscriptionId>
```

[Get-AzContext](/powershell/module/Az.Accounts/Get-AzContext) cmdlet 'ini çalıştırmak, doğru aboneliğin seçildiğini doğrular.

Azure disk şifrelemesi cmdlet 'lerinin yüklendiğini doğrulamak için [Get-Command](/powershell/module/microsoft.powershell.core/get-command?view=powershell-6) cmdlet 'ini kullanın:
     
```powershell
Get-command *diskencryption*
```
Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya](/powershell/azure/get-started-azureps)başlama. 

## <a name="enable-encryption-on-an-existing-or-running-linux-vm"></a>Mevcut veya çalışan bir Linux VM 'de şifrelemeyi etkinleştirme
Bu senaryoda, Resource Manager şablonu, PowerShell cmdlet'leri veya CLI komutları kullanarak şifreleyebilirsiniz. Sanal makine uzantısı için şema bilgilerine ihtiyacınız varsa, [Linux uzantısı Için Azure disk şifrelemesi](../extensions/azure-disk-enc-linux.md) makalesine bakın.

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin bir anlık görüntüsü portaldan veya [Azure Backup](../../backup/backup-azure-vms-encryption.md)aracılığıyla alınabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
>Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 
>

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-using-azure-cli"></a>Azure CLı kullanarak mevcut veya çalışan bir Linux VM 'de şifrelemeyi etkinleştirme 

[Azure CLI](/cli/azure/?view=azure-cli-latest) komut satırı aracını yükleyip kullanarak, şifrelenmiş VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. [Azure Cloud Shell](../../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure 'da mevcut veya çalışan Linux VM 'lerinde şifrelemeyi etkinleştirmek için aşağıdaki CLı komutlarını kullanın:

Azure 'da çalışan bir sanal makinede şifrelemeyi etkinleştirmek için [az VM Encryption Enable](/cli/azure/vm/encryption?view=azure-cli-latest#az-vm-encryption-show) komutunu kullanın.

- **Çalışan bir VM 'yi şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Çalışan bir VM 'yi KEK kullanarak şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı bir dizedir: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br>
Anahtar şifreleme anahtarı parametresinin değeri sözdizimi KEK olarak tam URI şudur: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifrelendiğini doğrulayın:** Bir sanal makinenin şifreleme durumunu denetlemek için [az VM Encryption Show](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-using-powershell"></a>PowerShell kullanarak mevcut veya çalışan bir Linux VM 'de şifrelemeyi etkinleştirme
Azure 'da çalışan bir sanal makinede şifrelemeyi etkinleştirmek için [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın. Diskler şifrelenmeden önce bir [anlık görüntü](snapshot-copy-managed-disk.md) alın ve/veya VM 'yi [Azure Backup](../../backup/backup-azure-vms-encryption.md) yedekleyin. -SkipVmBackup parametresi, çalışan bir Linux sanal makinesini şifrelemek için PowerShell betiklerine zaten belirtildi.

-  **Çalışan bir VM 'Yi şifreleyin:** Aşağıdaki komut dosyası değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubu, VM ve Anahtar Kasası, ön koşullar olarak oluşturulmuştur. MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin. Hangi diskleri şifrelediğiniz belirlemek için-VolumeType parametresini değiştirin.

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
- **Çalışan bir VM 'YI kek kullanarak şifreleyin:** İşletim sistemi diskini değil, veri disklerini şifreliyorsanız-VolumeType parametresini eklemeniz gerekebilir. 

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
    
- **Disklerin şifrelendiğini doğrulayın:** Bir VM 'nin şifreleme durumunu denetlemek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet 'ini kullanın. 
    
     ```azurepowershell-interactive 
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifrelemesini devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.
     
     ```azurepowershell-interactive 
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-with-a-template"></a>Mevcut veya çalışan bir Linux sanal makinesinde şablonla şifrelemeyi etkinleştirme

[Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)kullanarak Azure 'da var olan veya çalışan BIR Linux sanal makinesinde disk şifrelemeyi etkinleştirebilirsiniz.

1. Azure hızlı başlangıç şablonunda **Azure 'A dağıt** ' a tıklayın.

2. Abonelik, kaynak grubu, kaynak grubu konumu, parametreleri, yasal koşulları ve Sözleşmesi'ni seçin. Mevcut veya çalışan VM 'de şifrelemeyi etkinleştirmek için **Oluştur** ' a tıklayın.

Resource Manager şablonu parametreleri, mevcut veya sanal makineleri çalıştırmak için aşağıdaki tabloda listelenmektedir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemi çalıştırmak için sanal makinenin adı. |
| keyVaultName | Şifreleme anahtarının karşıya yüklenmesi gereken anahtar kasasının adı. Bunu, cmdlet `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLı komutu `az keyvault list --resource-group "MyKeyVaultResourceGroupName"`kullanarak alabilirsiniz.|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı. |
|  KeyEncryptionKeyURL | Şifreleme anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarının URL 'SI. UseExistingKek açılır listesinde **nokek** ' ı seçerseniz bu parametre isteğe bağlıdır. UseExistingKek açılır listesinde **kek** ' yi seçerseniz _Keyencryptionkeyurl_ değerini girmeniz gerekir. |
| VolumeType | Şifreleme işlemi gerçekleştirilir birim türü. Geçerli değerler _Işletim sistemi_, _veri_ve _hepsi_. 
| forceUpdateTag | İşlemi çalıştırmak olması zorlamak gereken her zaman bir GUID gibi benzersiz bir değer geçirin. |
| resizeOSDisk | Sistem birimi bölme önce tam işletim sistemi VHD'si kaplayacak şekilde işletim sistemi bölümü yeniden boyutlandırılması. |
| location | Tüm kaynaklar için konum. |


## <a name="use-encryptformatall-feature-for-data-disks-on-linux-vms"></a>Linux VM 'lerinde veri diskleri için EncryptFormatAll özelliğini kullanın

**Encryptformatall** parametresi, Linux veri disklerinin şifrelenme süresini azaltır. Belirli ölçütlere uyan bölümler biçimlendirilecek (geçerli dosya sistemi ile) ve ardından komut yürütmeden önce olduğu yere yeniden takılacaktır. Ölçütleri karşılayan bir veri diski dışlamak istiyorsanız, komutu çalıştırmadan önce çıkarma.

 Bu komutu çalıştırdıktan sonra, daha önce bağlanan tüm sürücüler biçimlendirilir ve şifreleme katmanı artık boş sürücünün üstünde başlatılır. Bu seçenek belirlendiğinde VM'ye bağlı kısa ömürlü kaynak diski de şifrelenir. Kısa ömürlü sürücü sıfırlarsanız, yeniden biçimlendirildi ve sanal makine için Azure Disk şifrelemesi çözümü sonraki fırsatta yeniden şifrelenir. Kaynak disk şifrelendikten sonra, [Microsoft Azure Linux Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) kaynak diski yönetemez ve takas dosyasını etkinleştiremeyecektir, ancak takas dosyasını el ile yapılandırabilirsiniz.

>[!WARNING]
> Bir sanal makinenin veri birimlerinde gerekli verileri olduğunda EncryptFormatAll kullanılmamalıdır. Çıkarma tarafından şifrelemeden diskler dışarıda bırakılabilir. İlk önce bir test sanal makinesi üzerinde EncryptFormatAll denemek, özellik parametre ve VM üretimde denemeden önce itiraz hakkının düşmesi anlamak. EncryptFormatAll seçeneği veri diski biçimlendirir ve tüm veriler kaybolacak. Devam etmeden önce hariç tutmak istediğiniz disklerin düzgün şekilde takılmamış olduğunu doğrulayın. </br></br>
 >Bu parametre şifreleme ayarları güncelleştirilirken tutunun, gerçek şifrelemeyi önce yeniden başlatma için neden olabilir. Bu durumda, biçimlendirilmiş fstab dosyasını istemediğiniz diski kaldırmak ayrıca isteyeceksiniz. Benzer şekilde, şifreleme işlemi başlatmadan önce şifrelemek için fstab dosyasını biçimli istediğiniz bölümü eklemeniz gerekir. 

### <a name="encryptformatall-criteria"></a>EncryptFormatAll ölçütü
Parametresi tüm bölümlere geçer ve aşağıdaki ölçütlerin **Tümünü** karşılayan sürece bunları şifreler: 
- Bir işletim sistemi/kök/önyükleme bölümü değildir
- Zaten şifreli değil
- BEK birimi değil
- Bir RAID birimine değil
- LVM'yi birimi değil
- Bağlanmıştır

RAID veya LVM birimin yerine RAID veya LVM birimin oluşturan diskleri şifreleme.

### <a name="use-the-encryptformatall-parameter-with-azure-cli"></a>Azure CLı ile EncryptFormatAll parametresini kullanma
Azure 'da çalışan bir sanal makinede şifrelemeyi etkinleştirmek için [az VM Encryption Enable](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

-  **Çalışan bir VM 'yi EncryptFormatAll kullanarak şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="use-the-encryptformatall-parameter-with-a-powershell-cmdlet"></a>Bir PowerShell cmdlet 'i ile EncryptFormatAll parametresini kullanma
EncryptFormatAll parametresiyle [set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet 'ini kullanın. 

**Çalışan bir VM 'Yi EncryptFormatAll kullanarak şifreleyin:** Örnek olarak, aşağıdaki komut dosyası değişkenlerinizi başlatır ve EncryptFormatAll parametresiyle set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubu, VM ve Anahtar Kasası, ön koşullar olarak oluşturulmuştur. MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin.
  
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


### <a name="use-the-encryptformatall-parameter-with-logical-volume-manager-lvm"></a>Mantıksal birim Yöneticisi (LVM) ile EncryptFormatAll parametresini kullanın 
LVM'yi üzerinde crypt Kurulum öneririz. Aşağıdaki örnekler tüm için bağlama ve cihaz yolunu ne olursa olsun, kullanım örneği uygun ile değiştirin. Bu Kurulum şu şekilde yapabilirsiniz:

- Sanal Makineyi oluşturan veri diski ekleyin.
- Biçim, bağlamak ve bu disklerin fstab dosyaya ekleyin.

    1. Yeni eklenen diski biçimlendirin. Burada Azure tarafından oluşturulan çözümlemeyin kullanırız. Çözümlemeyin kullanarak cihaz adları değiştirme ilgili sorunları önler. Daha fazla bilgi için bkz. [cihaz adları sorunlarını giderme](troubleshoot-device-names-problems.md) makalesi.
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    1. Diskleri bağlayın.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    1. Fstab için ekleyin.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    1. Bu diskleri şifrelemek için set-AzVMDiskEncryptionExtension PowerShell cmdlet 'ini-EncryptFormatAll ile çalıştırın.

       ```azurepowershell-interactive
       $KeyVault = Get-AzKeyVault -VaultName "MySecureVault" -ResourceGroupName "MySecureGroup"
           
       Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri  -DiskEncryptionKeyVaultId $KeyVault.ResourceId -EncryptFormatAll -SkipVmBackup -VolumeType Data
       ```

    1. LVM'yi, bu yeni diskler üzerinde ayarlayın. VM önyükleme tamamlandıktan sonra şifrelenmiş sürücülerin kilidi olduğuna dikkat edin. Bu nedenle, LVM bağlama sonradan geciktirileceği da gerekir.


## <a name="new-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Müşteri tarafından şifrelenen VHD ve şifreleme anahtarlarından oluşturulan yeni VM 'Ler
Bu senaryoda, PowerShell cmdlet'lerini veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. 

Azure 'da kullanılabilen önceden şifrelenmiş görüntüleri hazırlamak için Azure disk şifrelemesi ile aynı betiklerdeki yönergeleri kullanın. Görüntü oluşturulduktan sonra şifrelenmiş bir Azure VM oluşturmak için sonraki bölümde adımları kullanabilirsiniz.

* [Önceden şifrelenen bir Linux VHD hazırlama](disk-encryption-sample-scripts.md#prepare-a-pre-encrypted-linux-vhd)

>[!IMPORTANT]
 >Anlık görüntü için zorunludur ve/veya yedekleme yönetilen disk sanal makine örneğinin dışında ve Azure Disk şifrelemesi etkinleştirilmeden önce temel. Yönetilen diskin bir anlık görüntüsü portaldan alınabilir veya [Azure Backup](../../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedekleme kurtarma seçeneğini şifreleme sırasında beklenmeyen bir hata olması durumunda olası olduğundan emin olun. Yedekleme yapıldıktan sonra,-skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için set-AzVMDiskEncryptionExtension cmdlet 'i kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıncaya ve bu parametre belirtilene kadar yönetilen disk tabanlı VM 'Lere karşı başarısız olur. 
>
> Şifrelenirken veya şifreleme devre dışı bırakıldığında VM yeniden başlatılmasına neden olabilir. 



### <a name="use-azure-powershell-to-encrypt-vms-with-pre-encrypted-vhds"></a>VM 'Leri önceden şifrelenen VHD 'lerle şifrelemek için Azure PowerShell kullanma 
[Set-AzVMOSDisk](/powershell/module/Az.Compute/Set-AzVMOSDisk#examples)PowerShell cmdlet 'ini kullanarak, şifrelenen VHD 'niz üzerinde disk şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki örnek, bazı ortak parametreler sunar. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Linux -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen verileri disk şifrelemeyi etkinleştirin

[Az VM disk Attach](add-disk.md)veya [Azure Portal aracılığıyla](attach-disk-portal.md)yeni bir veri diski ekleyebilirsiniz. Ayrıca şifreleme önce yeni eklenen veri diski ilk bağlama gerekir. Sürücü Şifreleme işlemi devam ederken kullanılamaz olduğundan veri sürücüsü şifrelenmesi istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir disk şifrelemeyi etkinleştirin

 VM daha önce "All" ile şifrelendiyse,--Volume-Type parametresi "All" olarak kalmalıdır. Hem işletim sistemi ve veri diskleri içerir. VM daha önce "OS" birim türüyle şifrelendiyse, hem işletim sistemi hem de yeni veri diskinin dahil edilmesini sağlamak için--Volume-Type parametresi "All" olarak değiştirilmelidir. VM yalnızca "Data" birim türüyle şifrelenmişse, ardından bunu "Veri" aşağıda gösterildiği gibi kalır. Ekleme ve bir VM için yeni bir veri diski ekleme şifreleme için yeterli hazırlık değil. Yeni eklenen diski ayrıca olmalıdır biçimlendirilmiş ve şifreleme etkinleştirilmeden önce VM içinde düzgün bir şekilde bağlanır. Linux 'ta, diskin [kalıcı bir blok cihaz adıyla](troubleshoot-device-names-problems.md)/etc/fstab 'e bağlanması gerekir.  

PowerShell sözdizimi aksine, şifreleme etkinleştirilirken benzersiz dizisi sürümü sağlamak kullanıcı CLI gerektirmez. CLI'yı otomatik olarak oluşturur ve kendi benzersiz dizisi sürümü değeri kullanır.

-  **Çalışan bir VM 'nin veri birimlerini şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Çalışan bir VM 'nin veri birimlerini KEK kullanarak şifreleyin:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir disk şifrelemeyi etkinleştirin
 Linux için yeni bir disk şifrelemek için PowerShell kullanırken, yeni bir sıra sürüm belirtilmesi gerekiyor. Dizisi sürümü benzersiz olması gerekir. Aşağıdaki komut dizisi sürümü için bir GUID oluşturur. Diskler şifrelenmeden önce bir [anlık görüntü](snapshot-copy-managed-disk.md) alın ve/veya VM 'yi [Azure Backup](../../backup/backup-azure-vms-encryption.md) yedekleyin. Yeni eklenen bir veri diskini şifrelemek için PowerShell betiklerine-skipVmBackup parametresi zaten belirtilmiş.
 

-  **Çalışan BIR VM 'nin veri birimlerini şifreleyin:** Aşağıdaki komut dosyası değişkenlerinizi başlatır ve set-AzVMDiskEncryptionExtension cmdlet 'ini çalıştırır. Kaynak grubunu, VM'yi ve anahtar kasası zaten önkoşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MySecureVM ve Mysecurekasasını değerlerinizle değiştirin. -VolumeType parametresi için kabul edilebilir değerler, işletim sistemi ve veri tümü. VM daha önce "OS" veya "All" birim türüyle şifrelendiyse, hem işletim sistemi hem de yeni veri diskinin dahil edilmesini sağlamak için-VolumeType parametresi "All" olarak değiştirilmelidir.

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
- **Çalışan BIR VM 'nin veri BIRIMLERINI kek kullanarak şifreleyin:** -VolumeType parametresi için kabul edilebilir değerler tümü, işletim sistemi ve veri. VM, "OS" veya "All" birim türü ile önceden şifrelenmişse, böylece hem işletim sistemi hem de yeni bir veri diski eklenecek ardından - VolumeType parametresi tüm değiştirilmelidir.

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

- **Azure CLI ile şifrelemeyi devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [çalışan bir LINUX VM şablonunda şifrelemeyi devre dışı bırak '](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) i kullanın.
     1. **Azure’a Dağıt**’a tıklayın.
     2. Abonelik, kaynak grubu, konum, VM, yasal koşulları ve Sözleşmesi'ni seçin.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Azure disk şifrelemesi, aşağıdaki Linux senaryoları, özellikleri ve teknolojileri için çalışmaz:

- Klasik VM oluşturma yöntemiyle oluşturulan temel katman VM veya VM 'Leri şifreleme.
- IŞLETIM sistemi sürücüsü şifrelendiğinde bir işletim sistemi sürücüsünde veya Linux sanal makinesinin veri sürücüsünde şifrelemeyi devre dışı bırakma.
- Linux sanal makine ölçek kümeleri için işletim sistemi sürücüsü şifreleme.
- Linux VM 'lerde özel görüntüleri şifreleme.
- Şirket içi anahtar yönetim sistemiyle tümleştirme.
- Azure dosyaları (paylaşılan dosya sistemi).
- Ağ dosya sistemi (NFS).
- Dinamik birimler.
- Kısa ömürlü işletim sistemi diskleri.
- Paylaşılan/dağıtılmış dosya sistemlerinin şifrelenmesi (ancak bunlarla sınırlı olmamak üzere): DFS, GFS, DRDB ve CephFS gibi.
- Çekirdek kilitlenme dökümü (kdump).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure disk şifrelemeye genel bakış](disk-encryption-overview.md)
- [Azure Disk Şifrelemesi örnek betikleri](disk-encryption-sample-scripts.md)
- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
