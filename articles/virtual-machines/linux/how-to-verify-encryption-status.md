---
title: Linux için şifreleme durumunu doğrulama
description: Bu makale, platform ve işletim sistemi düzeyinden şifreleme durumunu doğrulamaya ilişkin yönergeler sağlar.
author: kailashmsft
ms.service: security
ms.topic: article
ms.author: kaib
ms.date: 03/11/2020
ms.custom: seodec18
ms.openlocfilehash: 0aaa32c46d915eafffcfac9d95cfdd3a24d4086d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80123417"
---
# <a name="how-to-verify-encryption-status-for-linux"></a>Linux için şifreleme durumunu doğrulama 

**Bu senaryo, ADE çift geçiş ve tek Pass uzantıları için geçerlidir.**  
Bu belge kapsamı, farklı yöntemler kullanarak bir sanal makinenin şifreleme durumunu doğrulamak için kullanılır.

### <a name="environment"></a>Ortam

- Linux dağıtımları

### <a name="procedure"></a>Yordam

Bir sanal makine, çift geçişli veya tek geçişli kullanılarak şifrelendi.

Şifreleme durumu, farklı yöntemler kullanılarak şifreleme sırasında veya sonrasında doğrulanabilir.

>[!NOTE] 
>Belge boyunca değişkenler kullanıyoruz, değerleri uygun şekilde değiştirin.

### <a name="verification"></a>Doğrulama

Doğrulama, Portal, PowerShell, AZ CLı ve veya VM işletim sistemi tarafında yapılabilir. 

Bu doğrulama, belirli bir sanal makineye bağlı diskler denetlenerek yapılabilir. 

Ya da diskin bağlı veya eklenmemiş olduğu her bir diskte şifreleme ayarlarını sorgulayarak.

Farklı doğrulama yöntemlerinin altında:

## <a name="using-the-portal"></a>Portalı kullanma

Azure portal uzantılar bölümünü denetleyerek şifreleme durumunu doğrulayın.

**Uzantılar** bölümünün IÇINDE, Ade uzantısını listelendiğini görürsünüz. 

Buraya tıklayın ve **durum iletisine**göz atın, geçerli şifreleme durumunu gösterir:

![Portal denetim numarası 1](./media/disk-encryption/verify-encryption-linux/portal-check-001.png)

Uzantılar listesinde, karşılık gelen ADE uzantısı sürümünü görürsünüz. 0. x sürümü ADE çift yönlü ve sürüm 1. x 'e karşılık gelir.

Daha fazla ayrıntı için uzantıya tıklayıp *ayrıntılı durumu görüntüle*' ye gidebilirsiniz.

JSON biçiminde şifreleme işleminin daha ayrıntılı bir durumunu göreceksiniz:

![Portal denetim numarası 2](./media/disk-encryption/verify-encryption-linux/portal-check-002.png)

![Portal denetim numarası 3](./media/disk-encryption/verify-encryption-linux/portal-check-003.png)

Şifreleme durumunu doğrulamanın bir başka yolu da **diskler** bölümüne göz atabilmenizdir.

![Portal denetim numarası 4](./media/disk-encryption/verify-encryption-linux/portal-check-004.png)

>[!NOTE] 
> Bu durum, disklerin şifreleme ayarlarına atılabilecek ancak gerçekte işletim sistemi düzeyinde şifrelendikleri anlamına gelir. Tasarıma göre, diskler önce damgalı ve daha sonra şifrelenir. Şifreleme işlemi başarısız olursa, diskler kesilebilir ancak şifrelenmez. Disklerin gerçekten şifrelenip şifrelenmediğini onaylamak için, işletim sistemi düzeyinde her diskin şifrelemesini iki kez kontrol edebilirsiniz.

## <a name="using-powershell"></a>PowerShell’i kullanma

Aşağıdaki PowerShell komutlarını kullanarak, şifrelenmiş bir sanal makinenin **genel** şifreleme durumunu doğrulayabilirsiniz:

```azurepowershell
   $VMNAME="VMNAME"
   $RGNAME="RGNAME"
   Get-AzVmDiskEncryptionStatus -ResourceGroupName  ${RGNAME} -VMName ${VMNAME}
```
![PowerShell 1 ' i denetle](./media/disk-encryption/verify-encryption-linux/verify-status-ps-01.png)

Aşağıdaki PowerShell komutlarını kullanarak her bir diskten şifreleme ayarlarını yakalayabilirsiniz:

### <a name="single-pass"></a>Tek pass
Tek geçişte, şifreleme ayarları her bir diskte (işletim sistemi ve veri) damganız ise, işletim sistemi disk şifreleme ayarlarını tek geçişte aşağıda gösterildiği gibi yakalayabilirsiniz:

``` powershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$VM = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME}  
 $Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $VM.StorageProfile.OsDisk.Name
 Write-Host "============================================================================================================================================================="
 Write-Host "Encryption Settings:"
 Write-Host "============================================================================================================================================================="
 Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
 Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
 Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
 Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
 Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
 Write-Host "============================================================================================================================================================="
```
![İşletim sistemi tek geçiş 01 ' i doğrula](./media/disk-encryption/verify-encryption-linux/verify-os-single-ps-001.png)

Diskte şifreleme ayarları atımı yoksa, çıkış aşağıda gösterildiği gibi boş olur:

![İşletim sistemi şifreleme ayarları 2](./media/disk-encryption/verify-encryption-linux/os-encryption-settings-2.png)

Veri diskleri şifreleme ayarlarını yakala:

```azurepowershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$VM = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME}
 clear
 foreach ($i in $VM.StorageProfile.DataDisks|ForEach-Object{$_.Name})
 {
 Write-Host "============================================================================================================================================================="
 Write-Host "Encryption Settings:"
 Write-Host "============================================================================================================================================================="
 Write-Host "Checking Disk:" $i
 $Disk=(Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $i)
 Write-Host "Encryption Enable: " $Sourcedisk.EncryptionSettingsCollection.Enabled
 Write-Host "Encryption KeyEncryptionKey: " $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl;
 Write-Host "Encryption DiskEncryptionKey: " $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl;
 Write-Host "============================================================================================================================================================="
 }
```
![Tek PS 001 verileri doğrulama](./media/disk-encryption/verify-encryption-linux/verify-data-single-ps-001.png)

### <a name="dual-pass"></a>Çift geçiş
Çift geçişte, şifreleme ayarları, her bir diskte değil, VM modeline damgalanır.

Şifreleme ayarlarının çift geçişte damgalanmış olduğunu doğrulamak için aşağıdaki komutları kullanabilirsiniz:

```azurepowershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$vm = Get-AzVm -ResourceGroupName ${RGNAME} -Name ${VMNAME};
$Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $VM.StorageProfile.OsDisk.Name
clear
Write-Host "============================================================================================================================================================="
Write-Host "Encryption Settings:"
Write-Host "============================================================================================================================================================="
Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
Write-Host "============================================================================================================================================================="
```
![Çift Pass PowerShell 1 ' i doğrula](./media/disk-encryption/verify-encryption-linux/verify-dual-ps-001.png)

### <a name="unattached-disks"></a>Eklenmemiş diskler

Bir VM 'ye bağlı olmayan diskler için şifreleme ayarlarını kontrol edin.

### <a name="managed-disks"></a>Yönetilen diskler
```powershell
$Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName ${TARGETDISKNAME}
Write-Host "============================================================================================================================================================="
Write-Host "Encryption Settings:"
Write-Host "============================================================================================================================================================="
Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
Write-Host "============================================================================================================================================================="
```
## <a name="using-az-cli"></a>AZ CLı kullanma

Aşağıdaki AZ CLı komutlarını kullanarak, şifrelenmiş bir sanal makinenin **genel** şifreleme durumunu doğrulayabilirsiniz:

```bash
VMNAME="VMNAME"
RGNAME="RGNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"
```
![CLı kullanarak genel doğrulama ](./media/disk-encryption/verify-encryption-linux/verify-gen-cli.png)

### <a name="single-pass"></a>Tek pass
Şifreleme ayarlarını, aşağıdaki AZ CLı komutlarını kullanarak her bir diskten doğrulayabilirsiniz:

```bash
az vm encryption show -g ${RGNAME} -n ${VMNAME} --query "disks[*].[name, statuses[*].displayStatus]"  -o table
```

![Veri şifreleme ayarları](./media/disk-encryption/verify-encryption-linux/data-encryption-settings-2.png)

>[!IMPORTANT]
> Diskin şifreleme ayarlarına damgalı olmaması durumunda, "disk şifrelenmedi" olarak gösterilir

Ayrıntılı durum ve şifreleme ayarları:

İşletim sistemi diski:

```bash
RGNAME="RGNAME"
VMNAME="VNAME"

disk=`az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.osDisk.name -o tsv`
for disk in $disk; do \
echo "============================================================================================================================================================="
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![Ossingtaclı](./media/disk-encryption/verify-encryption-linux/os-single-cli.png)

Veri diskleri:

```bash
RGNAME="RGNAME"
VMNAME="VMNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"

for disk in `az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.dataDisks[].name -o tsv`; do \
echo "============================================================================================================================================================="; \
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![Veri tek CLı ](./media/disk-encryption/verify-encryption-linux/data-single-cli.png)

### <a name="dual-pass"></a>Çift geçiş

``` bash
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} -o table
```

![CLı ](./media/disk-encryption/verify-encryption-linux/verify-gen-dual-cli.png) kullanarak genel çift 'yi doğrulayın Ayrıca, işletim sistemi diskinin VM modeli depolama profilindeki şifreleme ayarlarını da kontrol edebilirsiniz:

```bash
disk=`az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.osDisk.name -o tsv`
for disk in $disk; do \
echo "============================================================================================================================================================="; \
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![CLı kullanarak VM profilini doğrulama ](./media/disk-encryption/verify-encryption-linux/verify-vm-profile-dual-cli.png)

### <a name="unattached-disks"></a>Eklenmemiş diskler

Bir VM 'ye bağlı olmayan diskler için şifreleme ayarlarını kontrol edin.

### <a name="managed-disks"></a>Yönetilen diskler

```bash
RGNAME="RGNAME"
TARGETDISKNAME="DISKNAME"
echo "============================================================================================================================================================="
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
```
### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Yönetilmeyen diskler, Azure depolama hesaplarında sayfa Blobları olarak depolanan VHD dosyalarıdır.

Belirli bir diskin ayrıntılarını almak için şunları sağlamanız gerekir:

Diski içeren depolama hesabının KIMLIĞI.
Söz konusu depolama hesabı için bir bağlantı dizesi.
Diski depolayan kapsayıcının adı.
Disk adı.

Bu komut tüm depolama hesaplarınızın tüm kimliklerini listeler:

```bash
az storage account list --query [].[id] -o tsv
```
Depolama hesabı kimlikleri aşağıdaki biçimde listelenir:

/Subscriptions/\<abonelik kimliği>/resourceGroups/\<kaynak grubu adı>/providers/microsoft.storage/storageaccounts/\<depolama hesabı adı>

Uygun KIMLIĞI seçin ve bir değişkende depolayın:
```bash
id="/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name>"
```
Bağlantı dizesi.

Bu komut, belirli bir depolama hesabı için bağlantı dizesini alır ve bir değişkende depolar:

```bash
ConnectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
```

Kapsayıcı adı.

Aşağıdaki komut, bir depolama hesabı altındaki tüm kapsayıcıları listeler:
```bash
az storage container list --connection-string $ConnectionString --query [].[name] -o tsv
```
Diskler için kullanılan kapsayıcı normalde "VHD 'ler" olarak adlandırılır

Kapsayıcı adını bir değişkende depolayın 
```bash
ContainerName="name of the container"
```

Disk adı.

Belirli bir kapsayıcıdaki tüm Blobları listelemek için bu komutu kullanın
```bash 
az storage blob list -c ${ContainerName} --connection-string $ConnectionString --query [].[name] -o tsv
```
Sorgulamak istediğiniz diski seçin ve adını bir değişkende depolayın.
```bash
DiskName="diskname.vhd"
```
Disk şifreleme ayarlarını sorgulama
```bash
az storage blob show -c ${ContainerName} --connection-string ${ConnectionString} -n ${DiskName} --query metadata.DiskEncryptionSettings
```

## <a name="from-the-os"></a>IŞLETIM sisteminden
Veri diski bölümlerinin şifrelendiğini (ve işletim sistemi diskinin olmadığını) doğrula

Bir bölüm/disk şifrelendiğinde, şifrelenmediğinde, bölüm **/disk** türü olarak **gösterildiği gibi, şifreli bir tür olarak** görüntülenir

``` bash
lsblk
```

![İşletim sistemi Crypt katmanı ](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer.png)

Aşağıdaki "lsblk" türevini kullanarak daha ayrıntılı bilgi edinebilirsiniz. 

Uzantı tarafından bağlanan bir **Crypt** türü katmanı görürsünüz.

Aşağıdaki örnek, mantıksal birimleri ve normal diskleri "**şifre\_Luks fsType**" olarak gösterir.

```bash
lsblk -o NAME,TYPE,FSTYPE,LABEL,SIZE,RO,MOUNTPOINT
```
![İşletim sistemi Crypt katman 2](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer-2.png)

Ek bir adım olarak, veri diskinde yüklenmiş herhangi bir anahtar olup olmadığını da doğrulayabilirsiniz

``` bash
cryptsetup luksDump /dev/VGNAME/LVNAME
```

``` bash
cryptsetup luksDump /dev/sdd1
```

Ve hangi DM cihazlarının şifreli olarak listelendiğini

```bash
dmsetup ls --target crypt
```

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
