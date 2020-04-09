---
title: Linux VM'lerinde Azure Disk Şifrelemesi senaryoları
description: Bu makalede, çeşitli senaryolar için Linux VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme hakkında yönergeler
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: d860b557c5c2d6b73fad53cdf84ee8b32182d1ee
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985465"
---
# <a name="azure-disk-encryption-scenarios-on-linux-vms"></a>Linux VM'lerinde Azure Disk Şifrelemesi senaryoları


Linux sanal makineler için Azure Disk Şifreleme (VM'ler), işletim sistemi diskinin ve veri disklerinin tam disk şifrelemesini sağlamak için Linux'un DM-Crypt özelliğini kullanır. Ayrıca, EncryptFormatAll özelliğini kullanırken geçici kaynak diskinin şifrelemesini sağlar.

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını kontrol ve yönetmenize yardımcı olmak için [Azure Key Vault ile tümleşiktir.](disk-encryption-key-vault.md) Hizmete genel bir bakış için [Windows VM'leri için Azure Disk Şifreleme'ye](disk-encryption-overview.md)bakın.

Disk şifrelemeyi yalnızca [desteklenen VM boyutlarının ve işletim sistemlerinin](disk-encryption-overview.md#supported-vms-and-operating-systems)sanal makinelerine uygulayabilirsiniz. Ayrıca aşağıdaki ön koşulları karşılamanız gerekir:

- [VM'ler için ek gereksinimler](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

Her durumda, diskler şifrelenmeden önce [anlık görüntü almalı](snapshot-copy-managed-disk.md) ve/veya yedekleme oluşturmalısınız. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata oluşursa kurtarma seçeneğinin mümkün olmasını sağlar. Yönetilen disklere sahip VM'ler, şifreleme oluşmadan önce yedekleme gerektirir. Yedekleme yapıldıktan sonra, -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için [Set-AzVMDiskEncryptionExtension cmdlet'ini](/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanabilirsiniz. Şifreli VM'leri yedekleme ve geri yükleme hakkında daha fazla bilgi için [Azure Yedekleme](../../backup/backup-azure-vms-encryption.md) makalesine bakın. 

>[!WARNING]
> - VM'yi şifrelemek için Azure AD ile Azure Disk Şifreleme'yi daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz. Ayrıntılar için [Azure AD (önceki sürüm) ile Azure Disk](disk-encryption-overview-aad.md) Şifrelemesi'ne bakın. 
>
> - Linux işletim sistemi birimlerini şifrelerken, VM kullanılamıyor olarak kabul edilmelidir. Şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engelleyen sorunları önlemek için şifreleme devam ederken SSH oturum açmalarından kaçınmanızı şiddetle öneririz. İlerlemeyi kontrol etmek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) PowerShell cmdlet veya [vm şifreleme cli](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. Bu işlemin 30 GB işletim sistemi hacmi için birkaç saat ve veri birimlerini şifrelemek için ek süre alması beklenebilir. Tüm seçenek şifrelenme biçimi kullanılmadığı sürece veri hacmi şifreleme süresi veri birimlerinin boyutu ve miktarıyla orantılı olacaktır. 
> - Linux VM'lerinde şifrelemenin devre dışı bırakılması yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelenmişse, veri veya işletim sistemi birimleri nde desteklenmez.  

## <a name="install-tools-and-connect-to-azure"></a>Araçları yükleyin ve Azure'a bağlanın

Azure Disk Şifrelemesi Azure [CLI](/cli/azure) ve [Azure PowerShell](/powershell/azure/new-azureps-module-az)aracılığıyla etkinleştirilebilir ve yönetilebilir. Bunu yapmak için araçları yerel olarak yüklemeniz ve Azure aboneliğinize bağlanmanız gerekir.

### <a name="azure-cli"></a>Azure CLI

[Azure CLI 2.0,](/cli/azure) Azure kaynaklarını yönetmek için bir komut satırı aracıdır. CLI, verileri esnek bir şekilde sorgulamak, engelleme olmayan işlemler olarak uzun süren işlemleri desteklemek ve komut dosyası yürütmeyi kolaylaştırmak için tasarlanmıştır. [Azure CLI'yi](/cli/azure/install-azure-cli?view=azure-cli-latest)yükleyin'deki adımları izleyerek yerel olarak yükleyebilirsiniz.

 

[Azure CLI ile Azure hesabınızda oturum açmak](/cli/azure/authenticate-azure-cli)için az [giriş](/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanın.

```azurecli
az login
```

Oturum açacak bir kiracı seçmek istiyorsanız şunları kullanın:
    
```azurecli
az login --tenant <tenant>
```

Birden çok aboneliğiniz varsa ve belirli bir abonelik belirtmek istiyorsanız, az hesap listesi yle abonelik [listenizi](/cli/azure/account#az-account-list) alın ve [az hesap kümesi](/cli/azure/account#az-account-set)yle belirtin.
     
```azurecli
az account list
az account set --subscription "<subscription name or ID>"
```

Daha fazla bilgi için Azure [CLI 2.0 ile başlayın' a](/cli/azure/get-started-with-azure-cli)bakın. 

### <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell az modülü,](/powershell/azure/new-azureps-module-az) Azure kaynaklarınızı yönetmek için [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/overview.md) modelini kullanan bir cmdlets kümesi sağlar. [Azure Cloud Shell](../../cloud-shell/overview.md)ile tarayıcınızda kullanabilirsiniz veya Azure [PowerShell modüllerini yükleyin](/powershell/azure/install-az-ps)yönergelerini kullanarak yerel makinenize yükleyebilirsiniz. 

Yerel olarak zaten yüklediyseniz, Azure Disk Şifrelemesini yapılandırmak için Azure PowerShell SDK sürümünün en son sürümünü kullandığınızdan emin olun. [Azure PowerShell sürümünün](https://github.com/Azure/azure-powershell/releases)en son sürümünü indirin.

[Azure PowerShell ile Azure hesabınızda oturum açabilmek](/powershell/azure/authenticate-azureps?view=azps-2.5.0)için [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) cmdlet'ini kullanın.

```powershell
Connect-AzAccount
```

Birden çok aboneliğiniz varsa ve bir tane sini belirtmek istiyorsanız, bunları listelemek için [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) cmdlet'ini kullanın ve ardından [Set-AzContext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0) cmdlet'i ekleyin:

```powershell
Set-AzContext -Subscription -Subscription <SubscriptionId>
```

[Get-AzContext](/powershell/module/Az.Accounts/Get-AzContext) cmdlet'inçalıştırıldığı, doğru aboneliğin seçildiğini doğrular.

Azure Disk Şifreleme cmdletlerinin yüklü olduğunu doğrulamak için [Get-command](/powershell/module/microsoft.powershell.core/get-command?view=powershell-6) cmdlet'ini kullanın:
     
```powershell
Get-command *diskencryption*
```
Daha fazla bilgi için Azure [PowerShell'e başlarken](/powershell/azure/get-started-azureps)bakın. 

## <a name="enable-encryption-on-an-existing-or-running-linux-vm"></a>Varolan veya çalışan bir Linux VM'de şifrelemeyi etkinleştirme
Bu senaryoda, Kaynak Yöneticisi şablonunu, PowerShell cmdlets'i veya CLI komutlarını kullanarak şifrelemeyi etkinleştirebilirsiniz. Sanal makine uzantısı için şema bilgisine ihtiyacınız varsa, [Linux uzantısı için Azure Disk Şifreleme](../extensions/azure-disk-enc-linux.md) makalesine bakın.

>[!IMPORTANT]
 >Azure Disk Şifrelemesi'ni etkinleştirmeden önce yönetilen disk tabanlı bir VM örneğini anlık olarak ve/veya yedeklemek zorunludur. Yönetilen diskin anlık görüntüsü portaldan veya [Azure Yedekleme'den](../../backup/backup-azure-vms-encryption.md)alınabilir. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata durumunda kurtarma seçeneğinin mümkün olmasını sağlar. Bir yedekleme yapıldıktan sonra, Set-AzVMDiskEncryptionExtension cmdlet -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıp bu parametre belirtilene kadar yönetilen disk tabanlı VM'lere karşı başarısız olur. 
>
>Şifrelemeyi şifrelemek veya devre dışı bırakmak VM'nin yeniden başlatılmasına neden olabilir. 
>

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-using-azure-cli"></a>Azure CLI kullanarak varolan veya çalışan bir Linux VM'de şifreleme yi etkinleştirme 

[Azure CLI](/cli/azure/?view=azure-cli-latest) komut satırı aracını yükleyip kullanarak şifreli VHD'nizde disk şifrelemeyi etkinleştirebilirsiniz. [Azure Cloud Shell](../../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure'da varolan veya çalıştırılan Linux VM'lerinde şifrelemeyi etkinleştirmek için aşağıdaki CLI komutlarını kullanın:

Azure'da çalışan sanal bir makinede şifreleme sağlamak için [az vm şifreleme etkinleştir](/cli/azure/vm/encryption?view=azure-cli-latest#az-vm-encryption-show) komutunu kullanın.

- **Çalışan vm'yi şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **KEK kullanarak çalışan bir VM'yi şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br>
Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Disklerin şifreli olduğunu doğrulayın:** VM'nin şifreleme durumunu kontrol etmek için [az vm şifreleme göster](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup"
     ```

- **Şifrelemeyi devre dışı kılabilir:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. Şifrelemeyi devre dışı bırakmak yalnızca Linux VM'leri için veri hacimlerinde izin verilir.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-using-powershell"></a>PowerShell'i kullanarak varolan veya çalışan bir Linux VM'de şifreleme yi etkinleştirme
Azure'da çalışan bir sanal makinede şifreleme sağlamak için [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'i kullanın. Diskler şifrelenmeden önce Azure Yedekleme ile anlık [görüntü](snapshot-copy-managed-disk.md) alın ve/veya Azure [Yedekleme](../../backup/backup-azure-vms-encryption.md) ile VM'yi yedekleyin. -skipVmBackup parametresi, çalışan bir Linux VM'sini şifrelemek için PowerShell komut dosyasında zaten belirtilmiştir.

-  **Çalışan vm'yi şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi çalıştırıyor ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırıyor. Kaynak grubu, VM ve anahtar kasası ön koşul olarak oluşturuldu. MyVirtualMachineResourceGroup, MySecureVM ve MySecureVault'u değerlerinizle değiştirin. Şifrelediğinizi belirtmek için -VolumeType parametresini değiştirin.

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
- **KEK kullanarak çalışan bir VM'yi şifreleme:** İşletim sistemi diskini değil, veri disklerini şifreliyorsanız -VolumeType parametresini eklemeniz gerekebilir. 

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
    > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 
    
- **Disklerin şifreli olduğunu doğrulayın:** VM'nin şifreleme durumunu kontrol etmek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) cmdlet'i kullanın. 
    
     ```azurepowershell-interactive 
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```
    
- **Disk şifrelemeyi devre dışı devre dışı:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. Şifrelemeyi devre dışı bırakmak yalnızca Linux VM'leri için veri hacimlerinde izin verilir.
     
     ```azurepowershell-interactive 
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM'
     ```

### <a name="enable-encryption-on-an-existing-or-running-linux-vm-with-a-template"></a>Bir şablonla varolan veya çalışan bir Linux VM'de şifrelemeyi etkinleştirme

[Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)kullanarak Azure'da varolan veya çalışan bir Linux VM'de disk şifrelemesini etkinleştirebilirsiniz.

1. Azure hızlı başlatma şablonunda **Azure'a Dağıt'ı** tıklatın.

2. Aboneliği, kaynak grubunu, kaynak grubu konumunu, parametreleri, yasal koşulları ve anlaşmayı seçin. Varolan veya çalışan VM'de şifrelemeyi etkinleştirmek için **Oluştur'u** tıklatın.

Aşağıdaki tabloda, varolan veya çalışan VM'ler için Kaynak Yöneticisi şablon parametreleri listelenir:

| Parametre | Açıklama |
| --- | --- |
| vmName | Şifreleme işlemini çalıştırmak için VM'nin adı. |
| keyVaultName | Şifreleme anahtarının yüklenmesi gereken anahtar kasasının adı. Cmdlet `(Get-AzKeyVault -ResourceGroupName <MyKeyVaultResourceGroupName>). Vaultname` veya Azure CLI komutunu `az keyvault list --resource-group "MyKeyVaultResourceGroupName"`kullanarak alabilirsiniz.|
| keyVaultResourceGroup | Anahtar kasasını içeren kaynak grubunun adı. |
|  keyEncryptionKeyURL | Şifreleme anahtarını şifrelemek için kullanılan anahtar şifreleme anahtarının URL'si. UseExistingKek açılır listesinde **nokek** seçerseniz bu parametre isteğe bağlıdır. UseExistingKek açılır listesinde **kek'i** seçerseniz, _anahtarEncryptionKeyURL_ değerini girmeniz gerekir. |
| volumeType | Şifreleme işleminin gerçekleştirildiği birim türü. Geçerli değerler _işletim sistemi,_ _Veri_ve _Tümü'tür._ 
| forceUpdateTag | Operasyonun kuvvet çalışması gerektiğinde GUID gibi benzersiz bir değerde geçirin. |
| location | Tüm kaynaklar için konum. |

Linux VM disk şifreleme şablonu yapılandırma hakkında daha fazla bilgi için Bkz. [Linux için Azure Disk Şifrelemesi.](https://docs.microsoft.com/azure/virtual-machines/extensions/azure-disk-enc-linux)

## <a name="use-encryptformatall-feature-for-data-disks-on-linux-vms"></a>Linux VM'lerde veri diskleri için EncryptFormatAll özelliğini kullanma

**EncryptFormatTüm** parametre, Linux veri disklerinin şifrelenme süresini kısaltıyor. Belirli ölçütleri karşılayan bölümler biçimlendirilir (geçerli dosya sistemiyle birlikte), komut yürütmeden önce olduğu yere yeniden monte edilir. Ölçütleri karşılayan bir veri diskini hariç tutmak isterseniz, komutu çalıştırmadan önce diski atlayabilirsiniz.

 Bu komutu çalıştırdıktan sonra, daha önce monte edilmiş tüm sürücüler biçimlendirilir ve şifreleme katmanı şimdi boş olan sürücünün üstünde başlatılır. Bu seçenek seçildiğinde, VM'ye bağlı geçici kaynak diski de şifrelenir. Geçici sürücü sıfırlanırsa, bir sonraki fırsatta Azure Disk Şifreleme çözümü tarafından VM için yeniden biçimlendirilir ve yeniden şifrelenir. Kaynak diski şifrelendikten sonra, [Microsoft Azure Linux Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) kaynak diskini yönetemez ve takas dosyasını etkinleştiremez, ancak takas dosyasını el ile yapılandırabilirsiniz.

>[!WARNING]
> EncryptFormatAll, VM'nin veri hacimlerinde gerekli veri olduğunda kullanılmamalıdır. Diskleri sökülerek şifrelemeden çıkarabilirsiniz. Önce bir test VM üzerinde encryptFormatAll denemek gerekir, üretim VM üzerinde denemeden önce özellik parametre ve etkisini anlamak. EncryptFormatAll seçeneği veri diskini biçimlendirecek ve üzerindeki tüm veriler kaybolur. Devam etmeden önce, hariç tutmak istediğiniz disklerin düzgün şekilde söküldiyi doğrulayın. </br></br>
 >Şifreleme ayarlarını güncellerken bu parametreyi ayarlıyorsanız, gerçek şifrelemeden önce yeniden başlatılabına neden olabilir. Bu durumda, fstab dosyasından biçimlendirin istemediğiniz diski de kaldırmak isteyeceksiniz. Benzer şekilde, şifreleme işlemini başlatmadan önce fstab dosyasına şifreleme biçimlendirilmiş istediğiniz bölümü eklemeniz gerekir. 

### <a name="encryptformatall-criteria"></a>EncryptFormatTüm kriterler
Parametre, aşağıdaki tüm ölçütleri **karşıladıkları** sürece tüm bölümleri ve bunları şifreler: 
- Kök/işletim sistemi/önyükleme bölümü değil mi?
- Zaten şifrelenmemiş
- BEK hacmi değil mi
- RAID hacmi değil mi
- LVM hacmi değil mi
- Monte edilir

RAID veya LVM birimi yerine RAID veya LVM hacmini oluşturan diskleri şifreleyin.

### <a name="use-the-encryptformatall-parameter-with-azure-cli"></a>Azure CLI ile EncryptFormatTüm parametreyi kullanın
Azure'da çalışan sanal bir makinede şifreleme sağlamak için [az vm şifreleme etkinleştir](/cli/azure/vm/encryption#az-vm-encryption-enable) komutunu kullanın.

-  **EncryptFormatAll kullanarak çalışan bir VM'yi şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --encrypt-format-all
     ```

### <a name="use-the-encryptformatall-parameter-with-a-powershell-cmdlet"></a>PowerShell cmdlet ile EncryptFormatTüm parametreyi kullanın
EncryptFormatAll parametresi ile [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) cmdlet'i kullanın. 

**EncryptFormatAll kullanarak çalışan bir VM'yi şifreleme:** Örnek olarak, aşağıdaki komut dosyası değişkenlerinizi ortaya kaldırır ve EncryptFormatAll parametresi ile Set-AzVMDiskEncryptionExtension cmdlet çalıştırın. Kaynak grubu, VM ve anahtar kasası ön koşul olarak oluşturuldu. MyVirtualMachineResourceGroup, MySecureVM ve MySecureVault'u değerlerinizle değiştirin.
  
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


### <a name="use-the-encryptformatall-parameter-with-logical-volume-manager-lvm"></a>Mantıksal Birim Yöneticisi (LVM) ile EncryptFormatTüm parametreyi kullanın 
LVM-on-crypt kurulumu öneririz. Aşağıdaki örnekler için, cihaz yolunu ve montaj noktalarını kullanım durumunuza uygun olanlarla değiştirin. Bu kurulum aşağıdaki gibi yapılabilir:

- VM'yi oluşturacak veri disklerini ekleyin.
- Bu diskleri fstab dosyasına biçimlendirin, monte edin ve ekleyin.

    1. Bir bölüm standardı seçin, tüm sürücüye yayılan bir bölüm oluşturun ve ardından bölümü biçimlendirin. Azure tarafından oluşturulan sempozyum bağlantılarını burada kullanıyoruz. Symlinks'in kullanılması, aygıt adlarının değiştirilmesiyle ilgili sorunları önler. Daha fazla bilgi için [Sorun Giderme Aygıt Adları sorunları](troubleshoot-device-names-problems.md) makalesine bakın.
    
         ```azurepowershell-interactive
         parted /dev/disk/azure/scsi1/lun0 mklabel gpt
         parted -a opt /dev/disk/azure/scsi1/lun0 mkpart primary ext4 0% 100%
         
         mkfs -t ext4 /dev/disk/azure/scsi1/lun0-part1
         ```
    
    1. Diskleri dağın.
         
         `mount /dev/disk/azure/scsi1/lun0-part1 /mnt/mountpoint`
    
    1. fstab ekleyin.
         
        `echo "/dev/disk/azure/scsi1/lun0-part1 /mnt/mountpoint ext4 defaults,nofail 0 2" >> /etc/fstab`
    
    1. Bu diskleri şifrelemek için Set-AzVMDiskEncryptionExtension PowerShell cmdlet'i -EncryptFormatAll ile çalıştırın.

       ```azurepowershell-interactive
       $KeyVault = Get-AzKeyVault -VaultName "MySecureVault" -ResourceGroupName "MySecureGroup"
           
       Set-AzVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri  -DiskEncryptionKeyVaultId $KeyVault.ResourceId -EncryptFormatAll -SkipVmBackup -VolumeType Data
       ```

    1. Bu yeni disklerin üzerine LVM'yi ayarlayın. VM önyükleme bittikten sonra şifrelenmiş sürücülerin kilidinin açılmıştır. Yani, LVM montaj da daha sonra geciktirilmesi gerekecektir.


## <a name="new-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Müşteri şifreli VHD ve şifreleme anahtarlarından oluşturulan yeni VM'ler
Bu senaryoda, PowerShell cmdlets veya CLI komutları kullanarak şifreleme etkinleştirebilirsiniz. 

Azure Diski'ndeki yönergeleri, Azure'da kullanılabilecek önceden şifrelenmiş görüntüleri hazırlamak için aynı komut dosyalarını kullanın. Resim oluşturulduktan sonra, şifreli bir Azure VM oluşturmak için bir sonraki bölümdeki adımları kullanabilirsiniz.

* [Önceden şifrelenmiş Bir Linux VHD hazırlama](disk-encryption-sample-scripts.md#prepare-a-pre-encrypted-linux-vhd)

>[!IMPORTANT]
 >Azure Disk Şifrelemesi'ni etkinleştirmeden önce yönetilen disk tabanlı bir VM örneğini anlık olarak ve/veya yedeklemek zorunludur. Yönetilen diskin anlık görüntüsü portaldan alınabilir veya [Azure Yedekleme](../../backup/backup-azure-vms-encryption.md) kullanılabilir. Yedeklemeler, şifreleme sırasında beklenmeyen bir hata durumunda kurtarma seçeneğinin mümkün olmasını sağlar. Bir yedekleme yapıldıktan sonra, Set-AzVMDiskEncryptionExtension cmdlet -skipVmBackup parametresini belirterek yönetilen diskleri şifrelemek için kullanılabilir. Set-AzVMDiskEncryptionExtension komutu, bir yedekleme yapılıp bu parametre belirtilene kadar yönetilen disk tabanlı VM'lere karşı başarısız olur. 
>
> Şifrelemeyi şifrelemek veya devre dışı bırakmak VM'nin yeniden başlatılmasına neden olabilir. 



### <a name="use-azure-powershell-to-encrypt-vms-with-pre-encrypted-vhds"></a>VM'leri önceden şifrelenmiş VHD'lerle şifrelemek için Azure PowerShell'i kullanın 
PowerShell cmdlet [Set-AzVMOSDisk'i](/powershell/module/Az.Compute/Set-AzVMOSDisk#examples)kullanarak şifreli VHD'nizde disk şifrelemesini etkinleştirebilirsiniz. Aşağıdaki örnekte bazı ortak parametreler verir. 

```azurepowershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Linux -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MyVirtualMachineResourceGroup"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Yeni eklenen bir veri diskinde şifrelemeyi etkinleştirme

[Az vm disk eklemeyi](add-disk.md)kullanarak veya Azure portalı üzerinden yeni bir veri [diski](attach-disk-portal.md)ekleyebilirsiniz. Şifrelemeden önce, önce yeni eklenen veri diskini takmanız gerekir. Şifreleme devam ederken sürücü kullanılamaz olduğundan veri sürücüsünün şifrelemesini istemeniz gerekir. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI ile yeni eklenen bir diskte şifreleme yi etkinleştirme

 VM daha önce "Tümü" ile şifrelenmişse, birim türü parametresi "Tümü" olarak kalmalıdır. Tüm işletim sistemi ve veri diskleri içerir. VM daha önce bir birim "işletim sistemi" türüyle şifrelenmişse, hem işletim sistemi hem de yeni veri diskinin eklenmesi için birim türü parametresi "Tümü" olarak değiştirilmelidir. VM yalnızca "Veri" hacmi türüyle şifrelenmişse, aşağıda gösterildiği gibi "Veri" olarak kalabilir. VM'ye yeni bir veri diski eklemek ve eklemek şifreleme için yeterli bir hazırlık değildir. Yeni eklenen disk, şifrelemeyi etkinleştirmeden önce vm içinde biçimlendirilmeli ve düzgün bir şekilde monte edilmelidir. Linux'ta disk /etc/fstab'a [kalıcı blok aygıt adı](troubleshoot-device-names-problems.md)ile monte edilmelidir.  

Powershell sözdiziminin aksine, CLI şifrelemeyi etkinleştirirken kullanıcının benzersiz bir dizi sürümü sağlamasını gerektirmez. CLI otomatik olarak kendi benzersiz sıra sürüm değerini oluşturur ve kullanır.

-  **Çalışan bir VM'nin veri birimlerini şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **KEK kullanarak çalışan bir VM'nin veri hacimlerini şifreleme:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MyVirtualMachineResourceGroup" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell ile yeni eklenen bir diskte şifreleme yi etkinleştirme
 Linux için yeni bir diski şifrelemek için Powershell kullanırken, yeni bir sıra sürümünün belirtilmesi gerekir. Dizi sürümü benzersiz olmalıdır. Aşağıdaki komut dosyası, sıra sürümü için bir GUID oluşturur. Diskler şifrelenmeden önce Azure Yedekleme ile anlık [görüntü](snapshot-copy-managed-disk.md) alın ve/veya Azure [Yedekleme](../../backup/backup-azure-vms-encryption.md) ile VM'yi yedekleyin. -skipVmBackup parametresi, yeni eklenen bir veri diskini şifrelemek için PowerShell komut dosyasında zaten belirtilmiştir.
 

-  **Çalışan bir VM'nin veri birimlerini şifreleme:** Aşağıdaki komut dosyası değişkenlerinizi çalıştırıyor ve Set-AzVMDiskEncryptionExtension cmdlet'i çalıştırıyor. Kaynak grubu, VM ve anahtar kasası zaten ön koşul olarak oluşturulmuş olmalıdır. MyVirtualMachineResourceGroup, MySecureVM ve MySecureVault'u değerlerinizle değiştirin. -VolumeType parametresi için kabul edilebilir değerler Tümü, İşletim sistemi ve Veriler'dir. VM daha önce "OS" veya "Tümü" bir birim türü ile şifrelenmişse, hem işletim sistemi hem de yeni veri diskinin eklenmesi için -VolumeType parametresi "Tümü" olarak değiştirilmelidir.

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
- **KEK kullanarak çalışan bir VM'nin veri hacimlerini şifreleme:** -VolumeType parametresi için kabul edilebilir değerler Tümü, İşletim sistemi ve Veriler'dir. VM daha önce "OS" veya "Tümü" bir birim türü ile şifrelenmişse, hem işletim sistemi hem de yeni veri diski dahil edilsin diye -VolumeType parametresi Tümü olarak değiştirilmelidir.

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
    > Disk-şifreleme-keyvault parametresi değeri için sözdizimi tam tanımlayıcı dizedir: /subscriptions/[subscription-id-guid]/resourceGroups/[KVresource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Anahtar-şifreleme-anahtar parametresi değeri için sözdizimi kek için tam URI olduğu gibi: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 


## <a name="disable-encryption-for-linux-vms"></a>Linux VM'leri için şifrelemeyi devre dışı
Azure PowerShell, Azure CLI veya Kaynak Yöneticisi şablonu kullanarak şifrelemeyi devre dışı kullanabilirsiniz. 

>[!IMPORTANT]
>Linux VM'lerinde Azure Disk Şifrelemesi ile şifrelemenin devre dışı bırakılması yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelenmişse, veri veya işletim sistemi birimleri nde desteklenmez.  

- **Azure PowerShell ile disk şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı bırakmak için [Devre Dışı Bırak-AzVMDiskŞifreleme](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [-VolumeType {ALL, DATA, OS}]
     ```

- **Azure CLI ile şifrelemeyi devre dışı katın:** Şifrelemeyi devre dışı kullanabilirsiniz, [az vm şifreleme devre dışı etme](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type [ALL, DATA, OS]
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı devre dışı:** Şifrelemeyi devre dışı kalmak için [çalışan bir Linux VM şablonundaki Devre Dışı Devre-Devre-Devre Şifresini](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) kullanın.
     1. **Azure’a dağıt**’a tıklayın.
     2. Aboneliği, kaynak grubunu, konumu, VM'yi, yasal koşulları ve anlaşmayı seçin.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Azure Disk Şifrelemesi aşağıdaki Linux senaryoları, özellikleri ve teknolojisi için çalışmaz:

- Klasik VM oluşturma yöntemi yle oluşturulan temel kademe VM veya VM'leri şifreleme.
- İşletim sistemi sürücüsü şifrelendiğinde, bir Işletim Sistemi sürücüsünde veya Linux VM'nin veri sürücüsünde şifrelemeyi devre dışı bırakma.
- Linux sanal makine ölçek setleri için işletim sistemi sürücüsüşifreleme.
- Linux VM'lerde özel görüntüleri şifreleme.
- Şirket içi anahtar yönetim sistemiyle entegrasyon.
- Azure Dosyaları (paylaşılan dosya sistemi).
- Ağ Dosya Sistemi (NFS).
- Dinamik hacimler.
- Kısa ömürlü işletim sistemi diskleri.
- DFS, GFS, DRDB ve CephFS gibi paylaşılan/dağıtılmış dosya sistemlerinin şifrelemesi (ancak bununla sınırlı değildir):
- Şifreli bir VM'yi başka bir aboneye taşıma.
- Çekirdek Çökmesi Dökümü (kdump).
- Oracle ACFS (ASM Cluster Dosya Sistemi)
- Gen2 VM'ler (bakınız: [Azure'da nesil 2 VM'ler için destek)](generation-2.md#generation-1-vs-generation-2-capabilities)
- Lsv2 serisi VM'ler (bakınız: [Lsv2 serisi)](../lsv2-series.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)
- [Azure Disk Şifrelemesi örnek betikleri](disk-encryption-sample-scripts.md)
- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
