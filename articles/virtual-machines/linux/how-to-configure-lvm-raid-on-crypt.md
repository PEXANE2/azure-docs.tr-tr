---
title: LVM ve RAID on-crypt'i Linux VM'de yapılandırma
description: Bu makalede, Linux VM'lerde kripto üzerinde LVM ve RAID yapılandırma hakkında talimatlar sağlar.
author: jofrance
ms.service: security
ms.topic: article
ms.author: jofrance
ms.date: 03/17/2020
ms.custom: seodec18
ms.openlocfilehash: 78ba47ba887cf7c90adf70d9d444fbd8a3c721cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284917"
---
# <a name="how-to-configure-lvm-and-raid-on-crypt"></a>LVM ve RAID on-crypt nasıl yapılandırılatır

Bu belge, kripto yapılandırmalarında LVM ve Raid'in nasıl gerçekleştirilebildiğini anlatan adım adım bir işlemdir.

### <a name="environment"></a>Ortam

- Linux Dağıtımları
    - RHEL 7.6+
    - Ubuntu 18.04+
    - SUSE 12+
- ADE Tek Geçiş
- ADE Çift Geçiş


## <a name="scenarios"></a>Senaryolar

**Bu senaryo, ADE çift geçişli ve tek geçişli uzantılar için geçerlidir.**  

- LVM'yi şifreli aygıtların üzerine yapılandırın (LVM-on-Crypt)
- RAID'i şifreli aygıtların üzerine yapılandırın (RAID-on-Crypt)

Altta yatan aygıt(lar) şifrelendikten sonra, bu şifreli katmanın üstünde LVM veya RAID yapıları oluşturabilirsiniz. Fiziksel Birimler (PV) şifreli Katman'ın üstünde oluşturulur.
Fiziksel Birimler birim grubu oluşturmak için kullanılır.
Hacimleri oluşturur ve /etc/fstab üzerinde gerekli girişleri ekleyin. 

![PowerShell'e bağlı diskleri denetleme](./media/disk-encryption/lvm-raid-on-crypt/000-lvm-raid-crypt-diagram.png)

Benzer şekilde, RAID aygıtı disklerde şifrelenmiş katmanın üstünde oluşturulur. RAID aygıtının üstüne bir dosya sistemi oluşturulur ve normal bir aygıt olarak /etc/fstab'a eklenir.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Kullanılması önerilen yöntem LVM-on-Crypt'dir.

RAID, belirli uygulama/ortam sınırlamaları nedeniyle LVM kullanılamadığında kabul edilir.

EncryptFormatAll seçeneğini kullanacaksınız, bu özellik hakkında bilgi burada https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-linux#use-encryptformatall-feature-for-data-disks-on-linux-vmsbulunuyor: .

Bu yöntem işletim sistemi şifrelenirken de yapılabilse de, biz sadece Veri sürücülerini şifreliyoruz.

Bu yordam, burada belirtilen ön koşulları gözden geçirdiğiniz https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-linux varsayar: ve burada https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-cli-quickstart.

ADE çift geçişli sürüm amortisman yolundadır ve artık yeni ADE şifrelemelerinde kullanılmamalıdır.

### <a name="procedure"></a>Yordam

"On crypt" yapılandırmalarını kullanırken, aşağıda özetlenen işlemi takip edeceksiniz:

>[!NOTE] 
>Belge boyunca değişkenler kullanıyoruz, değerleri buna göre değiştirin.
## <a name="general-steps"></a>Genel adımlar
### <a name="deploy-a-vm"></a>VM'yi dağıtma 
>[!NOTE] 
>Bu isteğe bağlı olsa da, bunu yeni dağıtılan bir VM'ye uygulamanızı öneririz.

PowerShell
```powershell
New-AzVm -ResourceGroupName ${RGNAME} `
-Name ${VMNAME} `
-Location ${LOCATION} `
-Size ${VMSIZE} `
-Image ${OSIMAGE} `
-Credential ${creds} `
-Verbose
```
Clı:
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
### <a name="attach-disks-to-the-vm"></a>VM'ye diskekleme:
VM PowerShell'e takmak istediğiniz $N yeni disk sayısı için yineleme
```powershell
$storageType = 'Standard_LRS'
$dataDiskName = ${VMNAME} + '_datadisk0'
$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $LOCATION -CreateOption Empty -DiskSizeGB 5
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName ${RGNAME}
$vm = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME} 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 0
Update-AzVM -VM ${VM} -ResourceGroupName ${RGNAME}
```
Clı:
```bash
az vm disk attach \
-g ${RGNAME} \
--vm-name ${VMNAME} \
--name ${VMNAME}datadisk1 \
--size-gb 5 \
--new \
-o table
```
### <a name="verify-the-disks-are-attached-to-the-vm"></a>Disklerin VM'ye bağlı olduğunu doğrulayın:
PowerShell:
```powershell
$VM = Get-AzVM -ResourceGroupName ${RGNAME} -Name ${VMNAME}
$VM.StorageProfile.DataDisks | Select-Object Lun,Name,DiskSizeGB
```
![PowerShell](./media/disk-encryption/lvm-raid-on-crypt/001-lvm-raid-check-disks-powershell.png) CLI'ye bağlı diskleri denetleyin:
```bash
az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.dataDisks -o table
```
![CLI](./media/disk-encryption/lvm-raid-on-crypt/002-lvm-raid-check-disks-cli.png) Portal'a bağlı ![diskleri denetleyin:](./media/disk-encryption/lvm-raid-on-crypt/003-lvm-raid-check-disks-portal.png) CLI OS'ye bağlı diskleri denetleyin:
```bash
lsblk 
```
![Diskleri bağlı portalı denetleme](./media/disk-encryption/lvm-raid-on-crypt/004-lvm-raid-check-disks-os.png)
### <a name="configure-the-disks-to-be-encrypted"></a>Diskleri şifrelenecek şekilde yapılandırma
Bu yapılandırma işletim sistemi düzeyi, ilgili diskler geleneksel ADE şifreleme için yapılandırılır yapılır:

Dosya sistemleri disklerin üstünde oluşturulur.

Dosya sistemlerini takmak için geçici montaj noktaları oluşturulur.

Filesystems /etc/fstab üzerinde önyükleme zamanında monte edilecek şekilde yapılandırılır.

Yeni disklere atanan aygıt harfini kontrol edin, bu örnekte dört veri diski kullanıyoruz

```bash
lsblk 
```
![İşletim sistemi eklenmiş diskleri denetleme](./media/disk-encryption/lvm-raid-on-crypt/004-lvm-raid-check-disks-os.png)

### <a name="create-a-filesystem-on-top-of-each-disk"></a>Her diskin üstünde bir dosya sistemi oluşturun.
Bu komut, "için" döngüsünün "in" bölümünde tanımlanan her diskte bir ext4 dosya sistemi oluşturmayı yineler.
```bash
for disk in c d e f; do echo mkfs.ext4 -F /dev/sd${disk}; done |bash
```
![Ekli diskleri kontrol](./media/disk-encryption/lvm-raid-on-crypt/005-lvm-raid-create-temp-fs.png) edin son zamanlarda oluşturulan dosya sistemlerinin UUID'sini bulun, monte etmek için geçici bir klasör oluşturun, /etc/fstab'a karşılık gelen girişleri ekleyin ve tüm dosya sistemlerini monte edin.

Bu komut, "for" döngüsünün "in" bölümünde tanımlanan her diskte de yineler:
```bash
for disk in c d e f; do diskuuid="$(blkid -s UUID -o value /dev/sd${disk})"; \
mkdir /tempdata${disk}; \
echo "UUID=${diskuuid} /tempdata${disk} ext4 defaults,nofail 0 0" >> /etc/fstab; \
mount -a; \
done
``` 
### <a name="verify-the-disks-are-mounted-properly"></a>Disklerin düzgün monte edilemeyi doğrulayın:
```bash
lsblk
```
![Temp filesystems](./media/disk-encryption/lvm-raid-on-crypt/006-lvm-raid-verify-temp-fs.png) monte ve yapılandırılmış kontrol edin:
```bash
cat /etc/fstab
```
![fstab'ı kontrol et](./media/disk-encryption/lvm-raid-on-crypt/007-lvm-raid-verify-temp-fstab.png)
### <a name="encrypt-the-data-disks"></a>Veri disklerini şifreleme:
KEK kullanarak PowerShell:
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
KEK kullanarak CLI:
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
![Şifreleme ps](./media/disk-encryption/lvm-raid-on-crypt/008-lvm-raid-verify-encryption-status-ps.png) CLI kontrol edin:
```bash
az vm encryption show -n ${VMNAME} -g ${RGNAME} -o table
```
![Şifreleme CLI](./media/disk-encryption/lvm-raid-on-crypt/009-lvm-raid-verify-encryption-status-cli.png) Portalını kontrol edin: ![Şifreleme İşletim Sistemi](./media/disk-encryption/lvm-raid-on-crypt/010-lvm-raid-verify-encryption-status-portal.png) Düzeyini kontrol edin:
```bash
lsblk
```
![Şifreleme CLI'yi denetle](./media/disk-encryption/lvm-raid-on-crypt/011-lvm-raid-verify-encryption-status-os.png)

Uzantı, dosya sistemlerini "/var/lib/azure_disk_encryption_config/azure_crypt_mount" (eski bir şifreleme) veya "/etc/crypttab" (yeni şifreleme) eklenmiştir.

Bu dosyalardan hiçbirini değiştirmeyin.

Bu dosya, daha sonra LVM veya RAID tarafından kullanılabilecek şekilde önyükleme işlemi sırasında bu diskleri etkinleştirme dikkat edilecektir. 

Bu dosyadaki montaj noktaları hakkında endişelenmeyin, çünkü ADE, bu şifreli cihazların üzerine fiziksel bir ses veya baskın cihazı oluşturduktan sonra diskleri normal bir dosya sistemi olarak monte etme yeteneğini kaybedecektir (bu dosya sırasında kullandığımız dosya sistemi formatından kurtulacak hazırlama süreci).
### <a name="remove-the-temp-folders-and-temp-fstab-entries"></a>Geçici klasörleri ve geçici fstab girişlerini kaldırma
LVM'nin bir parçası olarak kullanılacak disklerde dosya sistemlerini boşaltın
```bash
for disk in c d e f; do umount /tempdata${disk}; done
```
Ve / etc / fstab girişleri kaldırın:
```bash
vi /etc/fstab
```
### <a name="verify-that-the-disks-are-not-mounted-and-that-the-entries-on-etcfstab-were-removed"></a>Disklerin monte edilemediğini ve /etc/fstab'daki girişlerin kaldırıldığını doğrulayın
```bash
lsblk
```
![Geçici dosya sistemlerinin](./media/disk-encryption/lvm-raid-on-crypt/012-lvm-raid-verify-disks-not-mounted.png) monte edilmeden ve yapılandırılın:
```bash
cat /etc/fstab
```
![Geçici fstab girişlerini denetle kaldırıldı](./media/disk-encryption/lvm-raid-on-crypt/013-lvm-raid-verify-fstab-temp-removed.png)
## <a name="for-lvm-on-crypt"></a>LVM-on-crypt için
Artık temel diskler şifrelenmiş olduğundan, LVM yapılarını oluşturmaya devam edebilirsiniz.

Aygıt adını kullanmak yerine, fiziksel bir birim oluşturmak için disklerin her biri için /dev/mapper yollarını kullanın (diskin üstündeki şifreleme katmanında diskin kendisinde değil).
### <a name="configure-lvm-on-top-of-the-encrypted-layers"></a>LVM'yi şifreli katmanların üzerine yapılandırın
#### <a name="create-the-physical-volumes"></a>Fiziksel birimleri oluşturma
Dosya sistemi imzasını silmenin sorun olup olmadığını soran bir uyarı alırsınız. 

'y' girerek devam edebilir veya gösterildiği gibi yankı "y" kullanabilirsiniz:
```bash
echo "y" | pvcreate /dev/mapper/c49ff535-1df9-45ad-9dad-f0846509f052
echo "y" | pvcreate /dev/mapper/6712ad6f-65ce-487b-aa52-462f381611a1
echo "y" | pvcreate /dev/mapper/ea607dfd-c396-48d6-bc54-603cf741bc2a
echo "y" | pvcreate /dev/mapper/4159c60a-a546-455b-985f-92865d51158c
```
![pvcreate](./media/disk-encryption/lvm-raid-on-crypt/014-lvm-raid-pvcreate.png)
>[!NOTE] 
>Buradaki /dev/mapper/device adlarının lsblk çıktısına göre gerçek değerleriniz için değiştirilmesi gerekir.
#### <a name="verify-the-physical-volumes-information"></a>Fiziksel birimler bilgilerini doğrulama
```bash
pvs
```
![fiziksel hacimleri kontrol et 1](./media/disk-encryption/lvm-raid-on-crypt/015-lvm-raid-pvs.png)
#### <a name="create-the-volume-group"></a>Birim grubunu oluşturma
Zaten başharfe alınmış aynı cihazları kullanarak VG'yi oluşturun
```bash
vgcreate vgdata /dev/mapper/
```
### <a name="check-the-volume-group-information"></a>Birim grup bilgilerini kontrol edin
```bash
vgdisplay -v vgdata
```
```bash
pvs
```
![fiziksel hacimleri kontrol et 2](./media/disk-encryption/lvm-raid-on-crypt/016-lvm-raid-pvs-on-vg.png)
#### <a name="create-logical-volumes"></a>Mantıksal birimler oluşturma
```bash
lvcreate -L 10G -n lvdata1 vgdata
lvcreate -L 7G -n lvdata2 vgdata
``` 
#### <a name="check-the-logical-volumes-created"></a>Oluşturulan mantıksal birimleri denetleme
```bash
lvdisplay
lvdisplay vgdata/lvdata1
lvdisplay vgdata/lvdata2
```
![kontrol lvs](./media/disk-encryption/lvm-raid-on-crypt/017-lvm-raid-lvs.png)
#### <a name="create-filesystems-on-top-of-the-logical-volumes-structures"></a>Mantıksal birim(ler) yapısının üstüne dosya sistemleri oluşturma
```bash
echo "yes" | mkfs.ext4 /dev/vgdata/lvdata1
echo "yes" | mkfs.ext4 /dev/vgdata/lvdata2
```
#### <a name="create-the-mount-points-for-the-new-filesystems"></a>Yeni dosya sistemleri için montaj noktalarını oluşturma
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
#### <a name="verify-that-the-new-filesystems-are-mounted"></a>Yeni dosya sistemlerinin monte edilmiş olduğundan doğrulayın
```bash
lsblk -fs
df -h
```
![lsblk bu varyasyon üzerinde mantıksal birimleri](./media/disk-encryption/lvm-raid-on-crypt/018-lvm-raid-lsblk-after-lvm.png) kontrol, biz ters sipariş bağımlılıklarını gösteren cihazları listeliyoruz, bu seçenek özgün / dev / sd[disk] aygıt adları yerine mantıksal birim tarafından gruplanan cihazları tanımlamak için yardımcı olur.

Önemli: ADE şifreli bir aygıtın üzerinde oluşturulan LVM birimlerinin montaj noktası seçeneklerine "nofail" seçeneğinin eklenmiş olduğundan emin olun. İşletim sistemi önyükleme işlemi sırasında (veya bakım modunda) takılıp kalmaktan kaçınmak için önemlidir. 

Şifreli disk önyükleme işleminin sonunda açılır, LVM birimleri ve dosya sistemleri otomatik olarak monte edilir.

Nofail seçeneği kullanılmazsa, işletim sistemi hiçbir zaman ADE'nin başlatıldığı aşamaya geçmez ve veri diski(ler) kilidi açılır ve monte edilir.

VM'yi yeniden başlatmayı ve dosya sistemlerini doğrulamayı test edebilirsiniz. 

Bu işlemin dosya sistemlerinin sayısına ve boyutlarına bağlı olarak birkaç dakika sürebileceğini göz önünde bulundurun
#### <a name="reboot-the-vm-and-verify-after-reboot"></a>VM'yi yeniden başlatın ve yeniden başlatıldıktan sonra doğrulayın
```bash
shutdown -r now
```
```bash
lsblk
df -h
```
## <a name="for-raid-on-crypt"></a>RAID-on-Crypt için
Şimdi altta yatan diskler, aygıt adını kullanmak yerine LVM ile aynı RAID yapılarını oluşturmaya devam edebilirsiniz, disklerin her biri için /dev/mapper yollarını kullanabilirsiniz.

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
![mdadm oluşturmak](./media/disk-encryption/lvm-raid-on-crypt/019-lvm-raid-md-creation.png)
>[!NOTE] 
>Buradaki /dev/mapper/device adlarının lsblk çıktısına göre gerçek değerleriniz için değiştirilmesi gerekir.
#### <a name="checkmonitor-the-raid-creation"></a>RAID oluşturmayı kontrol edin/izleyin:
```bash
watch -n1 cat /proc/mdstat
mdadm --examine /dev/mapper/[]
mdadm --detail /dev/md10
```
![kontrol mdadm](./media/disk-encryption/lvm-raid-on-crypt/020-lvm-raid-md-details.png)
#### <a name="create-a-filesystem-on-top-of-the-new-raid-device"></a>Yeni Raid aygıtının üstünde bir dosya sistemi oluşturun:
```bash
mkfs.ext4 /dev/md10
```
Dosya sistemi için yeni bir montaj noktası oluşturun, yeni dosya sistemini /etc/fstab'a ekleyin ve
```bash
for device in md10; do diskuuid="$(blkid -s UUID -o value /dev/${device})"; \
mkdir /raiddata; \
echo "UUID=${diskuuid} /raiddata ext4 defaults,nofail 0 0" >> /etc/fstab; \
mount -a; \
done
```
Yeni dosya sistemlerinin monte edilmiş olduğundan doğrulayın
```bash
lsblk -fs
df -h
```
![kontrol mdadm](./media/disk-encryption/lvm-raid-on-crypt/021-lvm-raid-lsblk-md-details.png)

Önemli: ADE şifreli bir aygıtın üstünde oluşturulan RAID birimlerinin montaj noktası seçeneklerine "nofail" seçeneğinin eklenmiş olduğundan emin olun. 

Önyükleme işlemi sırasında (veya bakım modunda) takılıp kalan işletim sistemi önlemek için çok önemlidir. 

Şifrelenmiş disk önyükleme işleminin sonunda kilidi açılır ve nofail seçeneği kullanılmazsa RAID birimleri ve dosya sistemleri ADE tarafından kilidi açılana kadar otomatik olarak monte edilir.

İşletim sistemi, ADE'nin başlatıldığı ve veri disklerinin açıldığı ve monte edildiği aşamaya asla girmez.

VM'yi yeniden başlatmayı ve dosya sistemlerini doğrulamayı test edebilirsiniz. Bu işlemin dosya sistemlerinin sayısına ve boyutlarına bağlı olarak birkaç dakika sürebileceğini göz önünde bulundurun
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

