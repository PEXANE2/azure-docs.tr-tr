---
title: LVM ve RAID'i şifreli cihazlarda yapılandırın - Azure Disk Şifreleme
description: Bu makalede, Linux VM'ler için şifreli cihazlarda LVM ve RAID yapılandırmak için yönergeler sağlar.
author: jofrance
ms.service: security
ms.topic: article
ms.author: jofrance
ms.date: 03/17/2020
ms.custom: seodec18
ms.openlocfilehash: 4e342ff44af38b8e79dc8695c1270b1f5c68e0a8
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657437"
---
# <a name="configure-lvm-and-raid-on-encrypted-devices"></a>LVM ve RAID'i şifreli cihazlarda yapılandırma

Bu makale, şifreli aygıtlarda Mantıksal Birim Yönetimi (LVM) ve RAID'in nasıl gerçekleştirilebildiğini anlatan adım adım bir işlemdir. İşlem aşağıdaki ortamlar için geçerlidir:

- Linux dağıtımları
    - RHEL 7.6+
    - Ubuntu 18.04+
    - SUSE 12+
- Azure Disk Şifreleme tek geçişli uzantı
- Azure Disk Şifreleme çift geçişli uzantı


## <a name="scenarios"></a>Senaryolar

Bu makaledeki yordamlar aşağıdaki senaryoları destekler:  

- LVM'yi şifreli aygıtların üzerine yapılandırın (LVM-on-crypt)
- RAID'i şifreli aygıtların üzerine yapılandırın (RAID-on-crypt)

Altta yatan aygıt veya aygıtlar şifrelendikten sonra, bu şifreli katmanın üstünde LVM veya RAID yapıları oluşturabilirsiniz. 

Fiziksel birimler (PVs) şifreli katmanın üstünde oluşturulur. Fiziksel birimler birim grubu oluşturmak için kullanılır. Hacimleri oluşturur ve /etc/fstab üzerinde gerekli girişleri ekleyin. 

![LVM yapılarının katmanlarının diyagramı](./media/disk-encryption/lvm-raid-on-crypt/000-lvm-raid-crypt-diagram.png)

Benzer şekilde, RAID aygıtı disklerde şifrelenmiş katmanın üstünde oluşturulur. RAID cihazının üstüne bir dosya sistemi oluşturulur ve normal bir aygıt olarak /etc/fstab'a eklenir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

LVM-on-crypt kullanmanızı öneririz. RAID, LVM'nin belirli uygulama veya ortam sınırlamaları nedeniyle kullanılamadığı bir seçenektir.

**EncryptFormatAll** seçeneğini kullanırsınız. Bu seçenek hakkında daha fazla bilgi için, [Linux VM'lerde veri diskleri için EncryptFormatAll özelliğini kullanın'a](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-linux#use-encryptformatall-feature-for-data-disks-on-linux-vms)bakın.

Bu yöntemi işletim sistemi şifreleme yaparken de kullanabiliyor olsanız da, burada yalnızca veri sürücülerini şifreliyoruz.

Yordamlar, Linux VM'lerde ve Quickstart'ta [Azure Disk Şifreleme senaryolarında](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-linux) ön koşulları zaten gözden aldığınızı varsayıyor: Azure [CLI ile bir Linux VM oluşturun ve şifreleyin.](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-cli-quickstart)

Azure Disk Şifreleme çift geçişli sürüm bir amortisman yolundadır ve artık yeni şifrelemelerde kullanılmamalıdır.

## <a name="general-steps"></a>Genel adımlar

"On-crypt" yapılandırmalarını kullanırken, aşağıdaki yordamlarda özetlenen işlemi kullanın.

>[!NOTE] 
>Makale boyunca değişkenler kullanıyoruz. Değerleri buna göre değiştirin.

### <a name="deploy-a-vm"></a>VM'yi dağıtma 
Aşağıdaki komutlar isteğe bağlıdır, ancak bunları yeni dağıtılan sanal makineye (VM) uygulamanızı öneririz.

PowerShell:

```powershell
New-AzVm -ResourceGroupName ${RGNAME} `
-Name ${VMNAME} `
-Location ${LOCATION} `
-Size ${VMSIZE} `
-Image ${OSIMAGE} `
-Credential ${creds} `
-Verbose
```
Azure CLI:

```bash
az vm create \
-n ${VMNAME} \
-g ${RGNAME} \
--image ${OSIMAGE} \
--admin-username ${username} \
--admin-password ${password} \
-l ${LOCATION} \
--size ${VMSIZE} \
-o table
```
### <a name="attach-disks-to-the-vm"></a>Diskleri VM'ye ekleme
VM'ye eklemek `$N` istediğiniz yeni disk sayısı için aşağıdaki komutları yineleyin.

PowerShell:

```powershell
$storageType = 'Standard_LRS'
$dataDiskName = ${VMNAME} + '_datadisk0'
$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $LOCATION -CreateOption Empty -DiskSizeGB 5
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName ${RGNAME}
$vm = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME} 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 0
Update-AzVM -VM ${VM} -ResourceGroupName ${RGNAME}
```

Azure CLI:

```bash
az vm disk attach \
-g ${RGNAME} \
--vm-name ${VMNAME} \
--name ${VMNAME}datadisk1 \
--size-gb 5 \
--new \
-o table
```

### <a name="verify-that-the-disks-are-attached-to-the-vm"></a>Disklerin VM'ye bağlı olduğunu doğrulayın
PowerShell:
```powershell
$VM = Get-AzVM -ResourceGroupName ${RGNAME} -Name ${VMNAME}
$VM.StorageProfile.DataDisks | Select-Object Lun,Name,DiskSizeGB
```
![PowerShell'deki ekli diskler listesi](./media/disk-encryption/lvm-raid-on-crypt/001-lvm-raid-check-disks-powershell.png)

Azure CLI:

```bash
az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.dataDisks -o table
```
![Azure CLI'deki ekli diskler listesi](./media/disk-encryption/lvm-raid-on-crypt/002-lvm-raid-check-disks-cli.png)

Portal:

![Portaldaki ekli disklerin listesi](./media/disk-encryption/lvm-raid-on-crypt/003-lvm-raid-check-disks-portal.png)

İşletim sistemi:

```bash
lsblk 
```
![İşletim sistemi'ndeki ekli diskler listesi](./media/disk-encryption/lvm-raid-on-crypt/004-lvm-raid-check-disks-os.png)

### <a name="configure-the-disks-to-be-encrypted"></a>Diskleri şifrelenecek şekilde yapılandırma
Bu yapılandırma işletim sistemi düzeyinde yapılır. İlgili diskler Azure Disk Şifrelemesi aracılığıyla geleneksel şifreleme için yapılandırılır:

- Dosya sistemleri disklerin üstünde oluşturulur.
- Dosya sistemlerini takmak için geçici montaj noktaları oluşturulur.
- Dosya sistemleri /etc/fstab üzerinde önyükleme zamanında monte edilecek şekilde yapılandırılır.

Yeni disklere atanan aygıt harfini denetleyin. Bu örnekte, dört veri diski kullanıyoruz.

```bash
lsblk 
```
![İşletim sistemi'ne bağlı veri diskleri](./media/disk-encryption/lvm-raid-on-crypt/004-lvm-raid-check-disks-os.png)

### <a name="create-a-file-system-on-top-of-each-disk"></a>Her diskin üstünde bir dosya sistemi oluşturma
Bu komut, "için" döngüsünün "in" bölümünde tanımlanan her diskte bir ext4 dosya sisteminin oluşturulmasını yineler.

```bash
for disk in c d e f; do echo mkfs.ext4 -F /dev/sd${disk}; done |bash
```
![Ext4 dosya sisteminin oluşturulması](./media/disk-encryption/lvm-raid-on-crypt/005-lvm-raid-create-temp-fs.png)

Yakın zamanda oluşturduğunuz dosya sistemlerinin evrensel benzersiz tanımlayıcısını (UUID) bulun, geçici bir klasör oluşturun, /etc/fstab'a karşılık gelen girişleri ekleyin ve tüm dosya sistemlerini monte edin.

Bu komut, "for" döngüsünün "in" bölümünde tanımlanan her diskte de yineler:

```bash
for disk in c d e f; do diskuuid="$(blkid -s UUID -o value /dev/sd${disk})"; \
mkdir /tempdata${disk}; \
echo "UUID=${diskuuid} /tempdata${disk} ext4 defaults,nofail 0 0" >> /etc/fstab; \
mount -a; \
done
``` 

### <a name="verify-that-the-disks-are-mounted-properly"></a>Disklerin düzgün monte edilebilmektedir
```bash
lsblk
```
![Monte edilmiş geçici dosya sistemleri listesi](./media/disk-encryption/lvm-raid-on-crypt/006-lvm-raid-verify-temp-fs.png)

Ayrıca disklerin yapılandırıldığından da doğrulayın:

```bash
cat /etc/fstab
```
![fstab ile yapılandırma bilgileri](./media/disk-encryption/lvm-raid-on-crypt/007-lvm-raid-verify-temp-fstab.png)

### <a name="encrypt-the-data-disks"></a>Veri disklerini şifreleme
PowerShell anahtar şifreleme anahtarı (KEK) kullanarak:

```powershell
$sequenceVersion = [Guid]::NewGuid() 
Set-AzVMDiskEncryptionExtension -ResourceGroupName $RGNAME `
-VMName ${VMNAME} `
-DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
-DiskEncryptionKeyVaultId $KeyVaultResourceId `
-KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
-KeyEncryptionKeyVaultId $KeyVaultResourceId `
-VolumeType 'DATA' `
-EncryptFormatAll `
-SequenceVersion $sequenceVersion `
-skipVmBackup;
```

KEK kullanan Azure CLI:

```bash
az vm encryption enable \
--resource-group ${RGNAME} \
--name ${VMNAME} \
--disk-encryption-keyvault ${KEYVAULTNAME} \
--key-encryption-key ${KEYNAME} \
--key-encryption-keyvault ${KEYVAULTNAME} \
--volume-type "DATA" \
--encrypt-format-all \
-o table
```
### <a name="verify-the-encryption-status"></a>Şifreleme durumunu doğrulama

Yalnızca tüm diskler şifrelendiğinde bir sonraki adıma devam edin.

PowerShell:

```powershell
Get-AzVmDiskEncryptionStatus -ResourceGroupName ${RGNAME} -VMName ${VMNAME}
```
![PowerShell'de şifreleme durumu](./media/disk-encryption/lvm-raid-on-crypt/008-lvm-raid-verify-encryption-status-ps.png)

Azure CLI:

```bash
az vm encryption show -n ${VMNAME} -g ${RGNAME} -o table
```
![Azure CLI'de şifreleme durumu](./media/disk-encryption/lvm-raid-on-crypt/009-lvm-raid-verify-encryption-status-cli.png)

Portal:

![Portaldaki şifreleme durumu](./media/disk-encryption/lvm-raid-on-crypt/010-lvm-raid-verify-encryption-status-portal.png)

İşletim sistemi düzeyi:

```bash
lsblk
```
![İşletim sistemi şifreleme durumu](./media/disk-encryption/lvm-raid-on-crypt/011-lvm-raid-verify-encryption-status-os.png)

Uzantı, dosya sistemlerini /var/lib/azure_disk_encryption_config/azure_crypt_mount (eski bir şifreleme) veya /etc/crypttab 'a (yeni şifrelemeler) ekler.

>[!NOTE] 
>Bu dosyalardan hiçbirini değiştirmeyin.

Bu dosya, LVM veya RAID'in bunları daha sonra kullanabilmesi için önyükleme işlemi sırasında bu diskleri etkinleştirme işlemini halleder. 

Bu dosyadaki montaj noktaları hakkında endişelenmeyin. Azure Disk Şifreleme, bu şifrelenmiş aygıtların üzerinde fiziksel bir birim veya RAID aygıtı oluşturduktan sonra diskleri normal bir dosya sistemi olarak monte etme yeteneğini kaybeder. (Bu, hazırlama işlemi sırasında kullandığımız dosya sistemi biçimini kaldırır.)

### <a name="remove-the-temporary-folders-and-temporary-fstab-entries"></a>Geçici klasörleri ve geçici fstab girişlerini kaldırma
LVM'nin bir parçası olarak kullanılacak disklerde dosya sistemlerini kaldırırsınız.

```bash
for disk in c d e f; do unmount /tempdata${disk}; done
```
Ve / etc / fstab girişleri kaldırın:

```bash
vi /etc/fstab
```
### <a name="verify-that-the-disks-are-not-mounted-and-that-the-entries-on-etcfstab-were-removed"></a>Disklerin monte edilemediğini ve /etc/fstab'daki girişlerin kaldırıldığını doğrulayın

```bash
lsblk
```
![Geçici dosya sistemlerinin söküldin doğrulama](./media/disk-encryption/lvm-raid-on-crypt/012-lvm-raid-verify-disks-not-mounted.png)

Ve disklerin yapılandırıldığından doğrulayın:
```bash
cat /etc/fstab
```
![Geçici fstab girişlerinin kaldırıldığını doğrulama](./media/disk-encryption/lvm-raid-on-crypt/013-lvm-raid-verify-fstab-temp-removed.png)

## <a name="steps-for-lvm-on-crypt"></a>LVM-on-crypt için adımlar
Artık temel diskler şifrelenmiş olduğundan, LVM yapılarını oluşturabilirsiniz.

Aygıt adını kullanmak yerine, fiziksel bir birim oluşturmak için disklerin her biri için /dev/mapper yollarını kullanın (diskin üstündeki şifreleme katmanında, diskin kendisinde değil).

### <a name="configure-lvm-on-top-of-the-encrypted-layers"></a>LVM'yi şifreli katmanların üzerine yapılandırın
#### <a name="create-the-physical-volumes"></a>Fiziksel birimleri oluşturma
Dosya sistemi imzasını silmenin sorun olup olmadığını soran bir uyarı alırsınız. **Y**girerek devam edin veya gösterildiği gibi **yankı "y"** kullanın:

```bash
echo "y" | pvcreate /dev/mapper/c49ff535-1df9-45ad-9dad-f0846509f052
echo "y" | pvcreate /dev/mapper/6712ad6f-65ce-487b-aa52-462f381611a1
echo "y" | pvcreate /dev/mapper/ea607dfd-c396-48d6-bc54-603cf741bc2a
echo "y" | pvcreate /dev/mapper/4159c60a-a546-455b-985f-92865d51158c
```
![Fiziksel bir birimin oluşturulduğunu doğrulama](./media/disk-encryption/lvm-raid-on-crypt/014-lvm-raid-pvcreate.png)

>[!NOTE] 
>Buradaki /dev/mapper/device adlarının **lsblk**çıktısına göre gerçek değerleriniz için değiştirilmesi gerekir.

#### <a name="verify-the-information-for-physical-volumes"></a>Fiziksel birimler için bilgileri doğrulama
```bash
pvs
```

![Fiziksel birimler için bilgiler](./media/disk-encryption/lvm-raid-on-crypt/015-lvm-raid-pvs.png)

#### <a name="create-the-volume-group"></a>Birim grubunu oluşturma
Zaten başolarak başlatmış olan aynı aygıtları kullanarak birim grubunu oluşturun:

```bash
vgcreate vgdata /dev/mapper/
```

### <a name="check-the-information-for-the-volume-group"></a>Birim grubundaki bilgileri kontrol edin

```bash
vgdisplay -v vgdata
```
```bash
pvs
```
![Hacim grubu için bilgiler](./media/disk-encryption/lvm-raid-on-crypt/016-lvm-raid-pvs-on-vg.png)

#### <a name="create-logical-volumes"></a>Mantıksal birimler oluşturma

```bash
lvcreate -L 10G -n lvdata1 vgdata
lvcreate -L 7G -n lvdata2 vgdata
``` 

#### <a name="check-the-created-logical-volumes"></a>Oluşturulan mantıksal birimleri denetleme

```bash
lvdisplay
lvdisplay vgdata/lvdata1
lvdisplay vgdata/lvdata2
```
![Mantıksal birimler için bilgiler](./media/disk-encryption/lvm-raid-on-crypt/017-lvm-raid-lvs.png)

#### <a name="create-file-systems-on-top-of-the-structures-for-logical-volumes"></a>Mantıksal birimler için yapıların üstüne dosya sistemleri oluşturma

```bash
echo "yes" | mkfs.ext4 /dev/vgdata/lvdata1
echo "yes" | mkfs.ext4 /dev/vgdata/lvdata2
```

#### <a name="create-the-mount-points-for-the-new-file-systems"></a>Yeni dosya sistemleri için montaj noktalarını oluşturma

```bash
mkdir /data0
mkdir /data1
```

#### <a name="add-the-new-file-systems-to-etcfstab-and-mount-them"></a>Yeni dosya sistemlerini /etc/fstab'a ekleyin ve monte edin

```bash
echo "/dev/mapper/vgdata-lvdata1 /data0 ext4 defaults,nofail 0 0" >>/etc/fstab
echo "/dev/mapper/vgdata-lvdata2 /data1 ext4 defaults,nofail 0 0" >>/etc/fstab
mount -a
```

#### <a name="verify-that-the-new-file-systems-are-mounted"></a>Yeni dosya sistemlerinin monte edilmiş olduğundan doğrulayın

```bash
lsblk -fs
df -h
```
![Monte edilmiş dosya sistemleri için bilgiler](./media/disk-encryption/lvm-raid-on-crypt/018-lvm-raid-lsblk-after-lvm.png)

**LSBLK'nin**bu varyasyonunda, bağımlılıkları ters sırada gösteren aygıtları listeliyoruz. Bu seçenek, özgün /dev/sd[disk] aygıt adları yerine mantıksal birim tarafından gruplanan aygıtların tanımlanmasına yardımcı olur.

Azure Disk Şifrelemesi ile şifrelenmiş bir aygıtın üzerinde oluşturulan LVM birimlerinin montaj noktası seçeneklerine **nofail** seçeneğinin eklenmiş olduğundan emin olmak önemlidir. İşletim sistemi önyükleme işlemi sırasında (veya bakım modunda) sıkışmasını önler.

**Nofail** seçeneğini kullanmazsanız:

- İşletim sistemi, Azure Disk Şifrelemesi'nin başlatıldığı ve veri disklerinin kilidinin açıldığı ve monte edildiği aşamaya asla girmez. 
- Önyükleme işleminin sonunda şifrelenmiş disklerin kilidi açılır. LVM birimleri ve dosya sistemleri, Azure Disk Şifrelemesi kilidini açana kadar otomatik olarak monte edilecektir. 

VM'yi yeniden başlatmayı test edebilir ve dosya sistemlerinin önyükleme süresinden sonra otomatik olarak monte edilip edilebiyi doğrulayabilirsiniz. Bu işlem, dosya sistemlerinin sayısına ve boyutlarına bağlı olarak birkaç dakika sürebilir.

#### <a name="reboot-the-vm-and-verify-after-reboot"></a>VM'yi yeniden başlatın ve yeniden başlatıldıktan sonra doğrulayın

```bash
shutdown -r now
```
```bash
lsblk
df -h
```
## <a name="steps-for-raid-on-crypt"></a>RAID-on-crypt için adımlar
Artık temel diskler şifrelenmiş olduğundan RAID yapılarını oluşturmaya devam edebilirsiniz. İşlem LVM için olanla aynıdır, ancak aygıt adını kullanmak yerine her disk için /dev/mapper yollarını kullanın.

#### <a name="configure-raid-on-top-of-the-encrypted-layer-of-the-disks"></a>RAID'i disklerin şifreli katmanının üzerine yapılandırma
```bash
mdadm --create /dev/md10 \
--level 0 \
--raid-devices=4 \
/dev/mapper/c49ff535-1df9-45ad-9dad-f0846509f052 \
/dev/mapper/6712ad6f-65ce-487b-aa52-462f381611a1 \
/dev/mapper/ea607dfd-c396-48d6-bc54-603cf741bc2a \
/dev/mapper/4159c60a-a546-455b-985f-92865d51158c
```
![Mdadm komutu ile yapılandırılan RAID için bilgiler](./media/disk-encryption/lvm-raid-on-crypt/019-lvm-raid-md-creation.png)

>[!NOTE] 
>Buradaki /dev/mapper/device adlarının **lsblk**çıktısına bağlı olarak gerçek değerlerinizle değiştirilmesi gerekir.

### <a name="checkmonitor-raid-creation"></a>RAID oluşturmayı denetleme/izleme
```bash
watch -n1 cat /proc/mdstat
mdadm --examine /dev/mapper/[]
mdadm --detail /dev/md10
```
![RAID'in Durumu](./media/disk-encryption/lvm-raid-on-crypt/020-lvm-raid-md-details.png)

### <a name="create-a-file-system-on-top-of-the-new-raid-device"></a>Yeni RAID aygıtının üstünde bir dosya sistemi oluşturma
```bash
mkfs.ext4 /dev/md10
```

Dosya sistemi için yeni bir montaj noktası oluşturun, yeni dosya sistemini /etc/fstab'a ekleyin ve monte edin:

```bash
for device in md10; do diskuuid="$(blkid -s UUID -o value /dev/${device})"; \
mkdir /raiddata; \
echo "UUID=${diskuuid} /raiddata ext4 defaults,nofail 0 0" >> /etc/fstab; \
mount -a; \
done
```

Yeni dosya sisteminin monte edilmiş olduğundan doğrulayın:

```bash
lsblk -fs
df -h
```
![Monte edilmiş dosya sistemleri için bilgiler](./media/disk-encryption/lvm-raid-on-crypt/021-lvm-raid-lsblk-md-details.png)

Azure Disk Şifrelemesi ile şifrelenmiş bir aygıtın üzerinde oluşturulan RAID birimlerinin montaj noktası seçeneklerine **nofail** seçeneğinin eklenmiş olduğundan emin olmak önemlidir. İşletim sistemi önyükleme işlemi sırasında (veya bakım modunda) sıkışmasını önler.

**Nofail** seçeneğini kullanmazsanız:

- İşletim sistemi, Azure Disk Şifrelemesi'nin başlatıldığı ve veri disklerinin kilidinin açıldığı ve monte edildiği aşamaya asla girmez.
- Önyükleme işleminin sonunda şifrelenmiş disklerin kilidi açılır. RAID birimleri ve dosya sistemleri, Azure Disk Şifrelemesi kilidini açana kadar otomatik olarak monte edilecektir.

VM'yi yeniden başlatmayı test edebilir ve dosya sistemlerinin önyükleme süresinden sonra otomatik olarak monte edilip edilebiyi doğrulayabilirsiniz. Bu işlem, dosya sistemlerinin sayısına ve boyutlarına bağlı olarak birkaç dakika sürebilir.

```bash
shutdown -r now
```

Ve ne zaman giriş yapabilirsiniz:

```bash
lsblk
df -h
```
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)

