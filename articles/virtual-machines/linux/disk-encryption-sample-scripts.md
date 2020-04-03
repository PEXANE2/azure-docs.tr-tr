---
title: Azure Disk Şifrelemesi örnek betikleri
description: Bu makale, Linux VM'leri için Microsoft Azure Disk Şifreleme si için ektir.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: b54f9f3466fe5f7e2da622077f53575d6f43f72d
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585951"
---
# <a name="azure-disk-encryption-sample-scripts"></a>Azure Disk Şifrelemesi örnek betikleri 

Bu makalede, önceden şifrelenmiş VHD'ler ve diğer görevleri hazırlamak için örnek komut dosyaları sağlar.

 

## <a name="sample-powershell-scripts-for-azure-disk-encryption"></a>Azure Disk Şifrelemesi için örnek PowerShell komut dosyaları 

- **Aboneliğinizdeki tüm şifreli VM'leri listeleyin**

     ```azurepowershell-interactive
     $osVolEncrypted = {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).OsVolumeEncrypted}
     $dataVolEncrypted= {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).DataVolumesEncrypted}
     Get-AzVm | Format-Table @{Label="MachineName"; Expression={$_.Name}}, @{Label="OsVolumeEncrypted"; Expression=$osVolEncrypted}, @{Label="DataVolumesEncrypted"; Expression=$dataVolEncrypted}
     ```

- **Anahtar kasasında VM şifrelemek için kullanılan tüm disk şifreleme sırlarını listeleyin** 

     ```azurepowershell-interactive
     Get-AzKeyVaultSecret -VaultName $KeyVaultName | where {$_.Tags.ContainsKey('DiskEncryptionKeyFileName')} | format-table @{Label="MachineName"; Expression={$_.Tags['MachineName']}}, @{Label="VolumeLetter"; Expression={$_.Tags['VolumeLetter']}}, @{Label="EncryptionKeyURL"; Expression={$_.Id}}
     ```

### <a name="using-the-azure-disk-encryption-prerequisites-powershell-script"></a>Azure Disk Şifrelemesini kullanma PowerShell komut dosyası önkoşullarını
Azure Disk Şifreleme için ön koşulları zaten biliyorsanız, [Azure Disk Şifreleme ön koşullarını powershell komut dosyası](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 )nda kullanabilirsiniz. Bu PowerShell komut dosyasını kullanma örneği için [VM Quickstart'ı şifrele'ye](disk-encryption-powershell-quickstart.md)bakın. Varolan bir kaynak grubundaki varolan VM'lerin tüm disklerini şifrelemek için, satır 211'den başlayarak, komut dosyasının bir bölümünden açıklamaları kaldırabilirsiniz. 

Aşağıdaki tablo, PowerShell komut dosyasında hangi parametrelerin kullanılabileceğini gösterir: 


|Parametre|Açıklama|Zorunlu?|
|------|------|------|
|$resourceGroupName| KeyVault'un ait olduğu kaynak grubunun adı.  Yoksa, bu ada sahip yeni bir kaynak grubu oluşturulur.| True|
|$keyVaultName|Şifreleme anahtarlarının yerleştirilmeye alındığı KeyVault'un adı. Yoksa bu ada sahip yeni bir kasa oluşturulur.| True|
|$location|KeyVault'un konumu. Şifrelenecek KeyVault ve VM'lerin aynı konumda olduğundan emin olun. `Get-AzLocation` komutu ile bir konum listesi alın.|True|
|$subscriptionId|Kullanılacak Azure aboneliğinin tanımlayıcısı.  Abonelik Kimliğinizi `Get-AzSubscription` komutu ile alabilirsiniz.|True|
|$aadAppName|KeyVault'a sır yazmak için kullanılacak Azure AD uygulamasının adı. Bu ada sahip bir uygulama yoksa yeni bir uygulama oluşturulur. Bu uygulama zaten varsa, aadClientSecret parametresini komut dosyasına geçirin.|False|
|$aadClientSecret|Daha önce oluşturulan Azure AD uygulamasının istemci sırrı.|False|
|$keyEncryptionKeyName|KeyVault'ta isteğe bağlı anahtar şifreleme anahtarının adı. Yoksa, bu ada sahip yeni bir anahtar oluşturulur.|False|


### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Azure AD uygulaması olmadan VM'leri şifreleme veya çözme

- [Varolan veya çalışan bir Linux VM'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)  
- [Çalışan bir Linux VM'de şifrelemeyi devre dışı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) 
    - Şifrelemeyi devre dışı bırakmak yalnızca Linux VM'leri için Veri birimlerinde izin verilir.  

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>Azure AD uygulamasıyla VM'leri şifreleme veya şifresini çözme (önceki sürüm) 
 
- [Varolan veya çalışan bir Linux VM'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)    


-  [Çalışan bir Linux VM'de şifrelemeyi devre dışı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm) 
    - Şifrelemeyi devre dışı bırakmak yalnızca Linux VM'leri için Veri birimlerinde izin verilir. 


- [Önceden şifrelenmiş vhd/depolama blob'undan yeni bir şifreli yönetilen disk oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Önceden şifrelenmiş vhd ve ilgili şifreleme ayarları sağlanan yeni bir şifreli yönetilen disk oluşturur





## <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Çalışan bir Linux VM'de işletim sistemi sürücüsünü şifreleme

### <a name="prerequisites-for-os-disk-encryption"></a>İşletim sistemi disk şifrelemesi için ön koşullar

* VM, [Azure Disk Şifreleme destekli işletim sistemlerinde](disk-encryption-overview.md#supported-vms) listelenen işletim sistemi disk şifrelemesiyle uyumlu bir dağıtım kullanıyor olmalıdır 
* VM, Azure Kaynak Yöneticisi'ndeki Market görüntüsünden oluşturulmalıdır.
* En az 4 GB RAM'e sahip Azure VM (önerilen boyut 7 GB'dır).
* (RHEL ve CentOS için) SELinux'u devre dışı kınla. SELinux'u devre dışı kalım için bkz: "4.4.2. SELinux'un VM'deki [SELinux Kullanıcı ve Yönetici Kılavuzu'nda](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) devre dışı bırakılması.
* SELinux'u devre dışı attıktan sonra VM'yi en az bir kez yeniden başlatın.

### <a name="steps"></a>Adımlar
1. Daha önce belirtilen dağıtımlardan birini kullanarak bir VM oluşturun.

   CentOS 7.2 için işletim sistemi disk şifrelemesi özel bir görüntü ile desteklenir. Bu görüntüyü kullanmak için VM oluştururken SKU olarak "7.2n" belirtin:

   ```powershell
    Set-AzVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
   ```
2. VM'yi ihtiyaçlarınıza göre yapılandırın. Tüm (OS + veri) sürücülerini şifrelemek için veri sürücülerinin belirtilmesi ve /etc/fstab'dan monte edilebilmelidir.

   > [!NOTE]
   > UUID kullanın=... blok aygıt adını belirtmek yerine /etc/fstab'da veri sürücüleri belirtmek için (örneğin, /dev/sdb1). Şifreleme sırasında, VM'deki sürücülerin sırası değişir. VM'niz belirli bir blok aygıt sırasına dayanıyorsa, şifrelemeden sonra bunları takmak için başarısız olur.

3. SSH oturumlarını oturumu n için imzalayın.

4. İşletim sistemi şifrelemek için, şifrelemeyi etkinleştirdiğinizde volumeType'ı **Tümü** veya **İşletim Sistemi** olarak belirtin.

   > [!NOTE]
   > Hizmet olarak `systemd` çalışmayan tüm kullanıcı alanı işlemleri bir `SIGKILL`. VM'yi yeniden başlatın. Çalışan bir VM'de işletim sistemi disk şifrelemesini etkinleştirdiğinizde, VM kapalı kalma süresini planlayın.

5. Bir [sonraki bölümdeki](#monitoring-os-encryption-progress)yönergeleri kullanarak şifrelemenin ilerlemesini periyodik olarak izleyin.

6. Get-AzVmDiskEncryptionStatus "VMRestartPending" gösterir sonra, ya oturum açarak ya da portal, PowerShell veya CLI kullanarak VM yeniden başlatın.
    ```powershell
    C:\> Get-AzVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
   Yeniden başlatmadan önce, [VM'nin önyükleme tanılamalarını](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) kaydetmenizi öneririz.

## <a name="monitoring-os-encryption-progress"></a>İşletim sistemi şifreleme ilerlemesini izleme
İşletim sistemi şifreleme ilerlemesini üç şekilde izleyebilirsiniz:

* Cmdlet'i `Get-AzVmDiskEncryptionStatus` kullanın ve ProgressMessage alanını inceleyin:
    ```powershell
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
  VM "Işletim Sistemi disk şifrelemesi başladı" ulaştıktan sonra, Premium depolama destekli VM yaklaşık 40 ila 50 dakika sürer.

  Çünkü [sorun](https://github.com/Azure/WALinuxAgent/issues/388) WALinuxAgent #388 `OsVolumeEncrypted` `DataVolumesEncrypted` ve bazı `Unknown` dağıtımlarda olduğu gibi gösterir. WALinuxAgent sürüm 2.1.5 ve sonraki sürümlerle bu sorun otomatik olarak giderilir. Çıktıda `Unknown` görürseniz, Azure Kaynak Gezgini'ni kullanarak disk şifreleme durumunu doğrulayabilirsiniz.

  Azure [Kaynak Gezgini'ne](https://resources.azure.com/)gidin ve soldaki seçim panelinde bu hiyerarşiyi genişletin:

  ~~~~
  |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
  ~~~~                

  InstanceView'de, sürücülerinizin şifreleme durumunu görmek için aşağı kaydırın.

  ![VM Örnek Görünümü](./media/disk-encryption/vm-instanceview.png)

* [Önyükleme teşhisine](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)bak. ADE uzantısından gelen iletiler `[AzureDiskEncryption]`.

* SSH üzerinden VM'de oturum açın ve uzantı günlüğünü şu adresten alın:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

  İşletim sistemi şifrelemesi devam ederken VM'de oturum açmamanızı öneririz. Günlükleri yalnızca diğer iki yöntem başarısız olduğunda kopyalayın.

## <a name="prepare-a-pre-encrypted-linux-vhd"></a>Önceden şifrelenmiş Bir Linux VHD hazırlama
Önceden şifrelenmiş VHD'lerin hazırlanması dağıtıma bağlı olarak değişebilir. Ubuntu 16, openSUSE 13.2 ve CentOS 7'nin hazırlanmasıyla ilgili örnekler mevcuttur. 

### <a name="ubuntu-16"></a>Ubuntu 16
Dağıtım yüklemesi sırasında şifrelemeyi aşağıdaki adımları yaparak yapılandırın:

1. Diskleri bölümlere ayırırken **şifrelenmiş birimleri yapılandır'ı** seçin.

   ![Ubuntu 16.04 Kurulum - Şifreli birimleri yapılandırma](./media/disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Şifrelenmemesi gereken ayrı bir önyükleme sürücüsü oluşturun. Kök sürücünüzü şifreleyin.

   ![Ubuntu 16.04 Kurulum - Şifrelemek için aygıtları seçin](./media/disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Bir parola sağlayın. Bu anahtar kasasına yüklediğiniz şifre.

   ![Ubuntu 16.04 Kurulum - Geçiş cümlesi ver](./media/disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Bölümle'yi bitirin.

   ![Ubuntu 16.04 Kurulum - Bitiş bölümleme](./media/disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. VM önyükleme ve bir parola için istendiğinde, adım 3 sağlanan parolayı kullanın.

   ![Ubuntu 16.04 Kurulum - Önyüklemede parola sağlayın](./media/disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Bu yönergeleri kullanarak Azure'a yüklemek için [VM'yi](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/)hazırlayın. Son adımı (VM'yi devre denerek) henüz çalıştırmayın.

Aşağıdaki adımları yaparak şifrelemeyi Azure ile çalışacak şekilde yapılandırın:

1. /usr/local/sbin/azure_crypt_key.sh altında, içeriği aşağıdaki komut dosyasında içeren bir dosya oluşturun. Azure tarafından kullanılan parola dosyası adı olduğundan KeyFileName'ye dikkat edin.

    ```bash
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
   ```

2. */etc/crypttab'daki*crypt config'i değiştirin. Şu şekilde görünmelidir:
   ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

4. Komut dosyasına çalıştırılabilir izinler ekleyin:
   ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
   ```
5. */etc/initramfs-tools/modules'yi* ek satırlara göre düzenleme:
   ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
   ```
6. Etkili `update-initramfs -u -k all` olmak için initramfs `keyscript` güncelleştirmek için çalıştırın.

7. Artık VM'yi etkisiz kullanabilirsiniz.

   ![Ubuntu 16.04 Kurulum - güncelleme-initramfs](./media/disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Bir sonraki adıma devam edin ve VHD'nizi Azure'a yükleyin.

### <a name="opensuse-132"></a>openSUSE 13.2
Dağıtım yüklemesi sırasında şifrelemeyi yapılandırmak için aşağıdaki adımları yapın:
1. Diskleri bölümlere ayırırken, **Birim Grubu Şifrele'yi**seçin ve ardından bir parola girin. Bu, anahtar kasanıza yükleyeceğiniz şifredir.

   ![openSUSE 13.2 Kurulum - Volume Group'u Şifrele](./media/disk-encryption/opensuse-encrypt-fig1.png)

2. Parolanızı kullanarak VM'yi önyükleme.

   ![openSUSE 13.2 Kurulum - Önyüklemede parola sağlayın](./media/disk-encryption/opensuse-encrypt-fig2.png)

3. [SLES Hazırla'daki](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131)yönergeleri izleyerek veya Azure için SUSE sanal makinesini açarak Azure'a yüklemek için VM'yi hazırlayın. Son adımı (VM'yi devre denerek) henüz çalıştırmayın.

Şifrelemeyi Azure ile çalışacak şekilde yapılandırmak için aşağıdaki adımları yapın:
1. /etc/dracut.conf'ı düzenle ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Dosyanın sonuna kadar bu satırları yorum /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
   ```bash
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
   ```

3. Dosyanın başındaki aşağıdaki satırı ekleyerek /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm oluşumları değiştirmek:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   yerine şunu yazın:
   ```bash
    if [ 1 ]; then
   ```
4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh'yi edin ve "# Open LUKS cihazı"na eklayın:

    ```bash
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Initrd'i güncellemek için çalıştırın. `/usr/sbin/dracut -f -v`

6. Artık VM'yi etkisiz kılabilir ve VHD'nizi Azure'a yükleyebilirsiniz.

### <a name="centos-7-and-rhel-81"></a>CentOS 7 ve RHEL 8.1

Dağıtım yüklemesi sırasında şifrelemeyi yapılandırmak için aşağıdaki adımları yapın:
1. Diskleri bölümlere ayırırken **verilerimi** şifrele'yi seçin.

   ![CentOS 7 Kurulum -Kurulum hedefi](./media/disk-encryption/centos-encrypt-fig1.png)

2. Kök bölümü için **Şifreleme'nin** seçildiğinden emin olun.

   ![CentOS 7 Kurulumu -Kök bölümle şifreyi seçin](./media/disk-encryption/centos-encrypt-fig2.png)

3. Bir parola sağlayın. Bu anahtar kasanıza yükleyeceğiniz şifre.

   ![CentOS 7 Kurulumu - geçiş cümlesi sağlayın](./media/disk-encryption/centos-encrypt-fig3.png)

4. VM önyükleme ve bir parola için istendiğinde, adım 3 sağlanan parolayı kullanın.

   ![CentOS 7 Kurulumu - Bootup'ta parola girin](./media/disk-encryption/centos-encrypt-fig4.png)

5. Azure için [CentOS tabanlı bir sanal makine](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70)hazırlayın'daki "CentOS 7.0+" yönergelerini kullanarak VM'yi Azure'a yüklemek için hazırlayın. Son adımı (VM'yi devre denerek) henüz çalıştırmayın.

6. Artık VM'yi etkisiz kılabilir ve VHD'nizi Azure'a yükleyebilirsiniz.

Şifrelemeyi Azure ile çalışacak şekilde yapılandırmak için aşağıdaki adımları yapın:

1. /etc/dracut.conf'ı düzenle ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Dosyanın sonuna kadar bu satırları yorum /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
   ```bash
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
   ```

3. Dosyanın başındaki aşağıdaki satırı ekleyerek /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm oluşumları değiştirmek:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   -
   ```bash
    if [ 1 ]; then
   ```
4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh'yi edin ve "# Open LUKS cihazı"ndan sonra aşağıdakileri eklayın:
    ```bash
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. İnisiyatuarı güncellemek için "/usr/sbin/dracut -f -v" çalıştırın.

    ![CentOS 7 Kurulum - çalıştır /usr/sbin/dracut -f -v](./media/disk-encryption/centos-encrypt-fig5.png)

## <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Şifreli VHD'yi Azure depolama hesabına yükleme
DM-Crypt şifrelemesi etkinleştirildikten sonra, yerel şifreli VHD'nin depolama hesabınıza yüklenmesi gerekir.
```powershell
    Add-AzVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```
## <a name="upload-the-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Önceden şifrelenmiş VM'nin sırrını anahtar kasanıza yükleyin
Azure AD uygulamasını (önceki sürüm) kullanarak şifreleme yaparken, daha önce elde ettiğiniz disk şifreleme sırrının anahtar kasanıza gizli olarak yüklenmesi gerekir. Anahtar kasasının, Azure AD istemciniz için disk şifrelemesi ve izinleri etkinleştirmesi gerekir.

```powershell 
 $AadClientId = "My-AAD-Client-Id"
 $AadClientSecret = "My-AAD-Client-Secret"

 $key vault = New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption
``` 

### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>KEK ile şifrelenmemiş disk şifreleme sırrı
Anahtarı kasanızda gizli kurmak için [Set-AzKeyVaultSecret'ı](/powershell/module/az.keyvault/set-azkeyvaultsecret)kullanın. Parola base64 dizesi olarak kodlanır ve anahtar kasasına yüklenir. Ayrıca, anahtar kasasında sırrı oluştururken aşağıdaki etiketlerin ayarlandığınızdan emin olun.

```powershell

 # This is the passphrase that was provided for encryption during the distribution installation
 $passphrase = "contoso-password"

 $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
 $secretName = [guid]::NewGuid().ToString()
 $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
 $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

 $secret = Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
 $secretUrl = $secret.Id
```


`$secretUrl` [KEK kullanmadan OS diskini takmak](#without-using-a-kek)için bir sonraki adımı kullanın.

### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>KEK ile şifrelenmiş disk şifreleme sırrı
Gizli yi anahtar kasasına yüklemeden önce, isteğe bağlı olarak bir anahtar şifreleme anahtarı kullanarak şifreleyebilirsiniz. Önce anahtarı şifreleme anahtarını kullanarak sırrı şifrelemek için kaydırma [API'sini](https://msdn.microsoft.com/library/azure/dn878066.aspx) kullanın. Bu kaydırma işleminin çıktısı, [`Set-AzKeyVaultSecret`](/powershell/module/az.keyvault/set-azkeyvaultsecret) cmdlet'i kullanarak gizli olarak yükleyebileceğiniz base64 URL kodlu bir dizedir.

```powershell
    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id
```

Kek `$KeyEncryptionKey` `$secretUrl` [kullanarak işletim sistemi diskini takmak](#using-a-kek)için bir sonraki adımı kullanın.

##  <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>İşletim sistemi diski eklediğinizde gizli bir URL belirtin

###  <a name="without-using-a-kek"></a>KEK kullanmadan
İşletim sistemi diskini takarken, `$secretUrl`.'yi geçmeniz gerekir. URL, "KEK ile şifrelenmemiş disk şifreleme sırrı" bölümünde oluşturuldu.
```powershell
    Set-AzVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl
```
### <a name="using-a-kek"></a>KEK kullanma
İşletim sistemi diskini taktığınız zaman, geçirin `$KeyEncryptionKey` ve. `$secretUrl` URL, "KEK ile şifrelenmiş disk şifreleme sırrı" bölümünde oluşturuldu.
```powershell
    Set-AzVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id
```
