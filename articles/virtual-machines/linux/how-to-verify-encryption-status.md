---
title: Linux için şifreleme durumu nasıl doğrular?
description: Bu makalede, platform ve işletim sistemi düzeyinden şifreleme durumunu doğrulama hakkında yönergeler sağlar.
author: kailashmsft
ms.service: security
ms.topic: article
ms.author: kaib
ms.date: 03/11/2020
ms.custom: seodec18
ms.openlocfilehash: 0aaa32c46d915eafffcfac9d95cfdd3a24d4086d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123417"
---
# <a name="how-to-verify-encryption-status-for-linux"></a>Linux için şifreleme durumu nasıl doğrular? 

**Bu senaryo, ADE çift geçişli ve tek geçişli uzantılar için geçerlidir.**  
Bu Belge kapsamı, farklı yöntemler kullanarak sanal bir makinenin şifreleme durumunu doğrulamak tır.

### <a name="environment"></a>Ortam

- Linux dağıtımları

### <a name="procedure"></a>Yordam

Sanal bir makine çift geçişli veya tek geçişli olarak şifrelenmiştir.

Şifreleme durumu, şifreleme sırasında veya sonrasında farklı yöntemler kullanılarak doğrulanabilir.

>[!NOTE] 
>Belge boyunca değişkenler kullanıyoruz, değerleri buna göre değiştirin.

### <a name="verification"></a>Doğrulama

Doğrulama Portal, PowerShell, AZ CLI ve VM OS tarafından yapılabilir. 

Bu doğrulama, belirli bir VM'e bağlı diskleri denetleyerek yapılabilir. 

Veya her bir diskteki şifreleme ayarlarını sorgulayarak diskin bağlı veya eksiz olup olmadığını.

Farklı doğrulama yöntemlerinin altında:

## <a name="using-the-portal"></a>Portalı Kullanma

Azure portalındaki uzantılar bölümünü denetleyerek şifreleme durumunu doğrulayın.

**Uzantılar** bölümünde, Listelenen ADE uzantısını görürsünüz. 

Tıklayın ve **durum iletisi**bir göz atın, geçerli şifreleme durumunu gösterir:

![Portal kontrol numarası 1](./media/disk-encryption/verify-encryption-linux/portal-check-001.png)

Uzantılar listesinde, ilgili ADE uzantı sürümünü görürsünüz. Sürüm 0.x, ADE Dual-Pass'a karşılık gelir ve sürüm 1.x ADE Tek geçişli dir.

Uzantıyı tıklayarak daha fazla ayrıntı alabilir ve ardından *ayrıntılı durumu görüntüle'de*.

Şifreleme işleminin daha ayrıntılı bir durumunu json biçiminde görürsünüz:

![Portal kontrol numarası 2](./media/disk-encryption/verify-encryption-linux/portal-check-002.png)

![Portal kontrol numarası 3](./media/disk-encryption/verify-encryption-linux/portal-check-003.png)

Şifreleme durumunu doğrulamanın başka bir yolu da **Diskler** bölümüne bakmaktır.

![Portal kontrol numarası 4](./media/disk-encryption/verify-encryption-linux/portal-check-004.png)

>[!NOTE] 
> Bu durum, disklerin şifreleme ayarlarının damgalanmış olduğu, ancak işletim sistemi düzeyinde gerçekten şifrelenmedikleri anlamına gelir. Tasarım gereği, diskler önce damgalanır ve daha sonra şifrelenir. Şifreleme işlemi başarısız olursa, diskler damgalanmış ancak şifrelenmemiş olarak sona erebilir. Disklerin gerçekten şifrelenmiş olup olmadığını doğrulamak için, her diskin şifrelemesini işletim sistemi düzeyinde iki kez kontrol edebilirsiniz.

## <a name="using-powershell"></a>PowerShell’i kullanma

Aşağıdaki PowerShell komutlarını kullanarak şifreli bir VM'nin **genel** şifreleme durumunu doğrulayabilirsiniz:

```azurepowershell
   $VMNAME="VMNAME"
   $RGNAME="RGNAME"
   Get-AzVmDiskEncryptionStatus -ResourceGroupName  ${RGNAME} -VMName ${VMNAME}
```
![PowerShell 1'i kontrol edin](./media/disk-encryption/verify-encryption-linux/verify-status-ps-01.png)

Aşağıdaki PowerShell komutlarını kullanarak her bir diskin şifreleme ayarlarını yakalayabilirsiniz:

### <a name="single-pass"></a>Tek Geçişli
Tek geçişliyse, şifreleme ayarları disklerin her birine (Işletim Sistemi ve Veri) damgabasıyorsa, işletim sistemi disk şifreleme ayarlarını aşağıdaki gibi tek geçişte yakalayabilirsiniz:

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
![OS Tek geçiş01 doğrulayın](./media/disk-encryption/verify-encryption-linux/verify-os-single-ps-001.png)

Diskte şifreleme ayarları damgalanmış değilse, çıktı aşağıda gösterildiği gibi boş olacaktır:

![İşletim Sistemi Şifreleme ayarları 2](./media/disk-encryption/verify-encryption-linux/os-encryption-settings-2.png)

Veri diski(ler) şifreleme ayarlarını yakalayın:

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
![Verileri tek ps 001 doğrulayın](./media/disk-encryption/verify-encryption-linux/verify-data-single-ps-001.png)

### <a name="dual-pass"></a>Çift Geçişli
Çift Geçiş'te şifreleme ayarları her diskte değil, VM modelinde damgalanır.

Şifreleme ayarlarının çift geçişli olarak damgalandığını doğrulamak için aşağıdaki komutları kullanabilirsiniz:

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
![Çift geçişli PowerShell 1'i doğrulayın](./media/disk-encryption/verify-encryption-linux/verify-dual-ps-001.png)

### <a name="unattached-disks"></a>Eklenmemiş diskler

VM'ye bağlı olmayan disklerin şifreleme ayarlarını denetleyin.

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
## <a name="using-az-cli"></a>AZ CLI kullanma

Aşağıdaki AZ CLI komutlarını kullanarak şifrelenmiş bir VM'nin **genel** şifreleme durumunu doğrulayabilirsiniz:

```bash
VMNAME="VMNAME"
RGNAME="RGNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"
```
![CLI kullanarak genel doğrulama ](./media/disk-encryption/verify-encryption-linux/verify-gen-cli.png)

### <a name="single-pass"></a>Tek Geçiş
Aşağıdaki AZ CLI komutlarını kullanarak her bir diskten şifreleme ayarlarını doğrulayabilirsiniz:

```bash
az vm encryption show -g ${RGNAME} -n ${VMNAME} --query "disks[*].[name, statuses[*].displayStatus]"  -o table
```

![Veri şifreleme ayarları](./media/disk-encryption/verify-encryption-linux/data-encryption-settings-2.png)

>[!IMPORTANT]
> Diskin şifreleme ayarları damgalı olmaması durumunda, "Disk şifrelenmemiş" olarak gösterilir

Ayrıntılı Durum ve Şifreleme ayarları:

İşletim Sistemi Diski:

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

![OSSingleCLI](./media/disk-encryption/verify-encryption-linux/os-single-cli.png)

Veri Diskleri:

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

![Veri tek CLI ](./media/disk-encryption/verify-encryption-linux/data-single-cli.png)

### <a name="dual-pass"></a>Çift Geçiş

``` bash
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} -o table
```

![CLI ](./media/disk-encryption/verify-encryption-linux/verify-gen-dual-cli.png) kullanarak genel dual'ı doğrulayın İşletim Sistemi diskinin VM Model Depolama profilindeki Şifreleme ayarlarını da kontrol edebilirsiniz:

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

![CLI kullanarak vm profilini çift doğrulayın ](./media/disk-encryption/verify-encryption-linux/verify-vm-profile-dual-cli.png)

### <a name="unattached-disks"></a>Eklenmemiş diskler

VM'ye bağlı olmayan disklerin şifreleme ayarlarını denetleyin.

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

Yönetilmeyen diskler, Azure depolama hesaplarında sayfa blob'ları olarak depolanan VHD dosyalarıdır.

Belirli bir diskin ayrıntılarını almak için şunları sağlamanız gerekir:

Diski içeren depolama hesabının kimliği.
Belirli bir depolama hesabı için bir bağlantı dizesi.
Diski depolayan kapsayıcının adı.
Disk adı.

Bu komut, tüm depolama hesaplarınız için tüm ifadeleri listeler:

```bash
az storage account list --query [].[id] -o tsv
```
Depolama hesabı adları aşağıdaki formda listelenir:

/subscriptions/\<abonelik kimliği>/kaynakGruplar/\<kaynak grubu adı>/sağlayıcılar/Microsoft.Storage/storageAccounts/storage\<hesap adı>

Uygun kimliği seçin ve bir değişkenüzerinde saklayın:
```bash
id="/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name>"
```
Bağlantı dizesi.

Bu komut, belirli bir depolama hesabının bağlantı dizesini alır ve bir değişkende saklar:

```bash
ConnectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
```

Konteyner adı.

Aşağıdaki komut, bir depolama hesabı altındaki tüm kapsayıcıları listeler:
```bash
az storage container list --connection-string $ConnectionString --query [].[name] -o tsv
```
Diskler için kullanılan kapsayıcı normalde "vhds" olarak adlandırılır

Kapsayıcı adını bir değişkenüzerinde depolama 
```bash
ContainerName="name of the container"
```

Disk adı.

Belirli bir kapsayıcıdaki tüm lekeleri listelemek için bu komutu kullanın
```bash 
az storage blob list -c ${ContainerName} --connection-string $ConnectionString --query [].[name] -o tsv
```
Sorgulamak istediğiniz diski seçin ve adını bir değişkende depolayın.
```bash
DiskName="diskname.vhd"
```
Disk şifreleme ayarlarını sorgula
```bash
az storage blob show -c ${ContainerName} --connection-string ${ConnectionString} -n ${DiskName} --query metadata.DiskEncryptionSettings
```

## <a name="from-the-os"></a>İşletim Sistemi'nden
Veri diski bölümleri şifrelenmişse (ve işletim sistemi diski şifrelenmemişse) doğrula

Bir bölüm/disk **şifrelendiğinde, şifreleme** olmadığında **parça/disk** türü olarak görüntülenir,

``` bash
lsblk
```

![Os Crypt katmanı ](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer.png)

Aşağıdaki "lsblk" varyantını kullanarak daha fazla ayrıntı alabilirsiniz. 

Uzantı tarafından monte edilmiş bir **şifreleme** türü katmanı görürsünüz.

Aşağıdaki örnekte Mantıksal Birimler ve "**kripto\_LUKS FSTYPE**" olan normal diskler gösterilmektedir.

```bash
lsblk -o NAME,TYPE,FSTYPE,LABEL,SIZE,RO,MOUNTPOINT
```
![Os Crypt katmanı 2](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer-2.png)

Ek bir adım olarak, veri diskinde herhangi bir anahtar yüklenip yüklenmediğini de doğrulayabilirsiniz

``` bash
cryptsetup luksDump /dev/VGNAME/LVNAME
```

``` bash
cryptsetup luksDump /dev/sdd1
```

Ve hangi dm cihazları crypt olarak listelenir

```bash
dmsetup ls --target crypt
```

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
