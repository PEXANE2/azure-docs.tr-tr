---
title: Azure disk şifrelemesi örnek betikleri
description: Bu makalede, Linux sanal makineleri için Microsoft Azure disk şifrelemesi eki bulunur.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: c98da4b41da183f56d80fad1e8c01706d1cfcf23
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970502"
---
# <a name="azure-disk-encryption-sample-scripts"></a>Azure disk şifrelemesi örnek betikleri 

Bu makalede önceden şifrelenen VHD 'ler ve diğer görevler için örnek betikler sağlanmaktadır.

 

## <a name="sample-powershell-scripts-for-azure-disk-encryption"></a>Azure Disk şifrelemesi için örnek PowerShell betikleri 

- **Aboneliğinizdeki tüm şifreli VM 'Leri listeleyin**

     ```azurepowershell-interactive
     $osVolEncrypted = {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).OsVolumeEncrypted}
     $dataVolEncrypted= {(Get-AzVMDiskEncryptionStatus -ResourceGroupName $_.ResourceGroupName -VMName $_.Name).DataVolumesEncrypted}
     Get-AzVm | Format-Table @{Label="MachineName"; Expression={$_.Name}}, @{Label="OsVolumeEncrypted"; Expression=$osVolEncrypted}, @{Label="DataVolumesEncrypted"; Expression=$dataVolEncrypted}
     ```

- **Bir anahtar kasasındaki VM 'Leri şifrelemek için kullanılan tüm disk şifreleme gizli dizilerini listeleyin** 

     ```azurepowershell-interactive
     Get-AzKeyVaultSecret -VaultName $KeyVaultName | where {$_.Tags.ContainsKey('DiskEncryptionKeyFileName')} | format-table @{Label="MachineName"; Expression={$_.Tags['MachineName']}}, @{Label="VolumeLetter"; Expression={$_.Tags['VolumeLetter']}}, @{Label="EncryptionKeyURL"; Expression={$_.Id}}
     ```

### <a name="using-the-azure-disk-encryption-prerequisites-powershell-script"></a>Azure disk şifrelemesi önkoşulları PowerShell betiğini kullanma
Azure disk şifrelemesi önkoşullarını zaten biliyorsanız, [Azure disk şifrelemesi önkoşulları PowerShell betiğini](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 )kullanabilirsiniz. Bu PowerShell betiğini kullanmayla ilgili bir örnek için bkz. [VM 'Yi şifreleme hızlı başlangıç](disk-encryption-powershell-quickstart.md). Satırında 211, mevcut bir kaynak grubunda var olan VM'ler için tüm diskler şifrelemek için başlatma komut dosyasının bir bölümünden açıklamaları kaldırabilirsiniz. 

Aşağıdaki tabloda, PowerShell betik parametreleri kullanılabileceğini gösterir: 


|Parametre|Açıklama|Girilmesi?|
|------|------|------|
|$resourceGroupName| Anahtar kasası ait olduğu kaynak grubunun adı.  Yoksa, bu ada sahip yeni bir kaynak grubu oluşturulur.| True|
|$keyVaultName|Şifreleme anahtarları, yerleştirilecek olan anahtar kasası adı. Yoksa, bu ada sahip yeni bir kasa oluşturulur.| True|
|$location|Anahtar kasası konumu. Anahtar kasası ve VM'lerin şifrelenmesini aynı konumda olduğundan emin olun. `Get-AzLocation` komutu ile bir konum listesi alın.|True|
|$subscriptionId|Kullanılacak Azure aboneliğini tanımlayıcısı.  Abonelik Kimliğinizi `Get-AzSubscription` komutu ile alabilirsiniz.|True|
|$aadAppName|Anahtar kasası için gizli anahtarları yazmak için kullanılacak Azure AD uygulamasının adı. Bu ada sahip bir uygulama yoksa yeni bir uygulama oluşturulur. Bu uygulama zaten varsa, aadClientSecret parametre betiğine geçirin.|False|
|$aadClientSecret|Daha önce oluşturulan Azure AD uygulamasının istemci gizli anahtarı.|False|
|$keyEncryptionKeyName|KeyVault, isteğe bağlı anahtar şifreleme anahtarı adı. Bu ada sahip yeni bir anahtar yoksa oluşturulur.|False|


### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Şifrelemek veya VM'lerin şifresini çözmek bir Azure AD uygulaması

- [Mevcut veya çalışan bir Linux VM 'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)  
- [Çalışan bir Linux VM 'de şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) 
    - Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir.  

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>Şifrelemek veya Vm'leri bir Azure AD uygulamasını (önceki sürüm) şifresini çözme 
 
- [Mevcut veya çalışan bir Linux VM 'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)    


-  [Çalışan bir Linux VM 'de şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm) 
    - Şifreleme devre dışı bırakıldığında yalnızca veri birimlerinde Linux VM'ler için izin verilir. 


- [Önceden şifrelenen bir VHD/depolama blobundan yeni bir şifrelenmiş yönetilen disk oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Önceden şifrelenmiş bir VHD ve karşılık gelen şifreleme ayarları ile sağlanan yeni şifrelenmiş yönetilen disk oluşturur





## <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Çalışan bir Linux VM 'de işletim sistemi sürücüsü şifreleme

### <a name="prerequisites-for-os-disk-encryption"></a>İşletim sistemi disk şifrelemesi önkoşulları

* VM, [Azure Disk Şifrelemesi tarafından desteklenen işletim sistemlerinde](disk-encryption-overview.md#supported-vm-sizes) listelendiği gibi işletim sistemi disk şifrelemesi ile uyumlu bir dağıtım kullanıyor olmalıdır 
* Azure Resource Manager'daki Market görüntüsünden VM yeniden oluşturulması gerekir.
* En az 4 GB RAM ile Azure VM (boyutudur, 7 GB önerilir).
* (RHEL ve CentOS) SELinux devre dışı bırakın. SELinux devre dışı bırakmak için "4.4.2. bkz. SELinux kullanıcısının ve sanal makine üzerinde [yönetici kılavuzundaki](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) SELinux devre dışı bırakılıyor.
* SELinux devre dışı bıraktıktan sonra VM'yi en az bir kez yeniden başlatın.

### <a name="steps"></a>Adımlar
1. Daha önce belirtilen dağıtımları birini kullanarak bir VM oluşturun.

   CentOS 7.2 için işletim sistemi disk şifreleme özel bir görüntü desteklenir. Bu görüntü kullanmak için VM'yi oluştururken "7.2n" SKU olarak belirtin:

   ```powershell
    Set-AzVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
   ```
2. Sanal makine gereksinimlerinize göre yapılandırın. Gideceğinizi varsa tüm (işletim sistemi ve veri) şifrelemek için sürücüleri, veri sürücüleri belirtilen ve gelen bağlanabilir /etc/fstab olmanız gerekir.

   > [!NOTE]
   > UUID kullanmak =... veri sürücülerinde blok cihaz adı (örneğin, / dev/sdb1) belirtmek yerine/etc/fstab dosyasında belirtmek için. Şifreleme sırasında sanal makinede sürücüleri sırasını değiştirir. Sanal makinenizin blok cihazları belirli bir sırada dayanıyorsa, şifreleme sonrasında bağlamak başarısız olur.

3. SSH oturumları dışında oturum açın.

4. İşletim sistemini şifrelemek için, şifrelemeyi etkinleştirdiğiniz sırada volumeType 'ı **All** veya **OS** olarak belirtin.

   > [!NOTE]
   > `systemd` Hizmetleri olarak çalıştırmayan tüm Kullanıcı alanı işlemlerinin bir `SIGKILL`sonlandırılmalıdır. VM'yi yeniden başlatın. VM kapalı kalma süresi üzerinde çalışan bir VM'deki işletim sistemi disk şifreleme etkinleştirdiğinizde planlayın.

5. Bir [sonraki bölümdeki](#monitoring-os-encryption-progress)yönergeleri kullanarak şifreleme ilerlemesini düzenli olarak izleyin.

6. Get-AzVmDiskEncryptionStatus "VMRestartPending" değerini görüntülendikten sonra, sanal makineyi oturum açarak veya Portal, PowerShell veya CLı kullanarak yeniden başlatın.
    ```powershell
    C:\> Get-AzVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
   Yeniden başlatmadan önce, VM 'nin [önyükleme tanılamayı](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) kaydetmenizi öneririz.

## <a name="monitoring-os-encryption-progress"></a>İşletim sistemi şifreleme ilerlemesini izleme
İşletim sistemi şifreleme ilerleme üç yolla izleyebilirsiniz:

* `Get-AzVmDiskEncryptionStatus` cmdlet 'ini kullanın ve Ilerlemedurumuiletisi alanını inceleyin:
    ```powershell
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
  "Şifreleme başlatılan işletim sistemi diski" VM ulaştıktan sonra yaklaşık 40 ila 50 dakika sürer. VM üzerinde bir Premium depolama desteklenir.

  Walınuxagent içindeki [sorun #388](https://github.com/Azure/WALinuxAgent/issues/388) nedeniyle `OsVolumeEncrypted` ve `DataVolumesEncrypted` bazı dağıtımlarda `Unknown` olarak gösterilir. WALinuxAgent 2.1.5 sürümü ile ve daha sonra bu sorun otomatik olarak düzeltildi. Çıkışta `Unknown` görürseniz, Azure Kaynak Gezgini kullanarak disk şifreleme durumunu doğrulayabilirsiniz.

  [Azure Kaynak Gezgini](https://resources.azure.com/)gidin ve sol taraftaki seçim panelinde bu hiyerarşiyi genişletin:

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

  Instanceview içinde sürücülerinizin şifreleme durumunu görmek için aşağı kaydırın.

  ![Sanal makine örnek görünümü](./media/disk-encryption/vm-instanceview.png)

* [Önyükleme tanılarına](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)bakın. ADE uzantısından iletiler `[AzureDiskEncryption]`ön eki olmalıdır.

* Sanal makineye SSH aracılığıyla oturum açın ve uzantı günlüğünden alın:

    /var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

  VM için işletim sistemi şifreleme işlemi devam ederken oturum emin değilseniz öneririz. Diğer iki yöntemden yalnızca başarısız olan günlükler kopyalayın.

## <a name="prepare-a-pre-encrypted-linux-vhd"></a>Önceden şifrelenen bir Linux VHD hazırlama
Önceden şifrelenmiş VHD'ler için hazırlık, dağıtım bağlı olarak farklılık gösterebilir. Ubuntu 16, openSUSE 13,2 ve CentOS 7 ' yi hazırlama örnekleri mevcuttur. 

### <a name="ubuntu-16"></a>Ubuntu 16
Şifreleme dağıtım yüklemesi sırasında aşağıdaki adımları uygulayarak yapılandırın:

1. Diskleri bölümleyerek **şifrelenmiş birimleri Yapılandır** ' ı seçin.

   ![Ubuntu 16.04 Kurulumu - şifrelenmiş hacimlerini yapılandırma](./media/disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Şifrelenmemiş olması bir ayrı önyükleme sürücüsü oluşturun. Kök drive'ınızdaki şifreleyin.

   ![Ubuntu 16.04 Kurulumu - şifrelemek için cihazları seçin](./media/disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Bir parola girin. Anahtar kasasına yüklenmiş parolayı budur.

   ![Ubuntu 16.04 Kurulumu - parola girin](./media/disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Bölümleme tamamlayın.

   ![Ubuntu 16.04 Kurulumu - son bölümleme](./media/disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. VM'yi önyüklemek için bir parola istendiğinde, 3. adımda belirttiğiniz parolayı kullanın.

   ![Ubuntu 16.04 Kurulumu - önyüklemede parolayı girin](./media/disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. [Bu yönergeleri](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/)kullanarak VM 'yi Azure 'a yükleme için hazırlayın. (VM sağlamayı kaldırma) işleminin son adımında çalıştırma henüz.

Şifreleme aşağıdaki adımları uygulayarak Azure ile çalışacak şekilde yapılandırın:

1. Aşağıdaki betiği içerikle /usr/local/sbin/azure_crypt_key.sh altında bir dosya oluşturun. Azure tarafından kullanılan parola dosya adı olduğundan KeyFileName dikkat edin.

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

2. */Etc/crypttab*içinde Crypt yapılandırmasını değiştirin. Şu şekilde görünmelidir:
   ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

4. Yürütülebilir izinleri komut dosyasına ekleyin:
   ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
   ```
5. */Etc/initramfs-Tools/modules* satırları ekleyerek düzenleyin:
   ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
   ```
6. `keyscript` etkili olması için initramfs 'yi güncelleştirmek üzere `update-initramfs -u -k all` çalıştırın.

7. Artık VM yetkisini kaldırabilirsiniz.

   ![Ubuntu 16.04 kurulumu - güncelleştirme initramfs](./media/disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Sonraki adıma devam edin ve VHD'nizi Azure'a yükleyin.

### <a name="opensuse-132"></a>openSUSE 13.2
Dağıtım yüklenmesi sırasında şifreleme yapılandırmak için aşağıdaki adımları uygulayın:
1. Diskleri bölümlediğinizde, **birim grubunu şifreleyin**' ı seçin ve ardından bir parola girin. Bu anahtar kasanız için karşıya yükleyelim paroladır.

   ![openSUSE 13.2 Kurulumu - şifreleme birim grubu](./media/disk-encryption/opensuse-encrypt-fig1.png)

2. Parolanızı kullanarak VM'yi önyükleyin.

   ![openSUSE 13.2 Kurulumu - önyüklemede parolayı girin](./media/disk-encryption/opensuse-encrypt-fig2.png)

3. [Azure için BIR SLES veya openSUSE sanal makinesi hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131)bölümündeki YÖNERGELERI izleyerek VM 'yi Azure 'a yüklemek üzere hazırlayın. (VM sağlamayı kaldırma) işleminin son adımında çalıştırma henüz.

Azure ile çalışacak şekilde şifrelemesini yapılandırmak için aşağıdaki adımları uygulayın:
1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Bu satırları açıklama satırı yapar dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonunda:
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

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başındaki aşağıdaki satırı ekleyin:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm oluşumlarını değiştirin:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   yerine şunu yazın:
   ```bash
    if [ 1 ]; then
   ```
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve "# açık LUKS cihaza" ekleyin:

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
5. Initrd 'yi güncelleştirmek için `/usr/sbin/dracut -f -v` çalıştırın.

6. Artık VM'nin sağlamasını kaldırmak ve Azure'a VHD'nizi karşıya yükleyin.

### <a name="centos-7-and-rhel-81"></a>CentOS 7 ve RHEL 8,1

Dağıtım yüklenmesi sırasında şifreleme yapılandırmak için aşağıdaki adımları uygulayın:
1. Diskleri bölümlediğinizde verilerimi **şifreleyin** ' ı seçin.

   ![CentOS 7 Kurulumu - yükleme hedefi](./media/disk-encryption/centos-encrypt-fig1.png)

2. Kök bölüm için **şifreleme** seçildiğinden emin olun.

   ![CentOS 7 Kurulumu - şifrelemek için kök bölümü seçin](./media/disk-encryption/centos-encrypt-fig2.png)

3. Bir parola girin. Anahtar kasanız için karşıya yükleyelim parolayı budur.

   ![CentOS 7 Kurulumu - parola girin](./media/disk-encryption/centos-encrypt-fig3.png)

4. VM'yi önyüklemek için bir parola istendiğinde, 3. adımda belirttiğiniz parolayı kullanın.

   ![CentOS 7 Kurulumu - önyükleme parola girin](./media/disk-encryption/centos-encrypt-fig4.png)

5. [Azure Için CentOS tabanlı bir sanal makine hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70)konusundaki "CentOS 7.0 +" yönergelerini kullanarak VM 'yi Azure 'a yükleme için hazırlayın. (VM sağlamayı kaldırma) işleminin son adımında çalıştırma henüz.

6. Artık VM'nin sağlamasını kaldırmak ve Azure'a VHD'nizi karşıya yükleyin.

Azure ile çalışacak şekilde şifrelemesini yapılandırmak için aşağıdaki adımları uygulayın:

1. /Etc/dracut.conf düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Bu satırları açıklama satırı yapar dosya /usr/lib/dracut/modules.d/90crypt/module-setup.sh sonunda:
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

3. Dosya /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh başındaki aşağıdaki satırı ekleyin:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm oluşumlarını değiştirin:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   -
   ```bash
    if [ 1 ]; then
   ```
4. /Usr/lib/dracut/Modules.d/90crypt/cryptroot-ASK.sh düzenleyin ve sonra "# açık LUKS cihaz" aşağıdakini ekleyin:
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
5. Çalıştır "/ usr/sbin/dracut - f - v" initrd güncelleştirilecek.

    ![CentOS 7 Kurulumu - /usr/sbin/dracut -f - v çalıştıran](./media/disk-encryption/centos-encrypt-fig5.png)

## <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Şifrelenmiş VHD 'YI bir Azure depolama hesabına yükleme
DM-Crypt şifrelemesi etkinleştirildikten sonra, yerel şifreli VHD 'nin depolama hesabınıza yüklenmesi gerekir.
```powershell
    Add-AzVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```
## <a name="upload-the-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Önceden şifrelenen VM 'nin parolasını anahtar kasanıza yükleme
Bir Azure AD uygulamasını (önceki sürüm) kullanarak şifreleme, daha önce edindiğiniz disk şifreleme gizli anahtar kasanızdaki gizli dizi olarak yüklenmelidir. Anahtar kasası disk şifrelemesi ve Azure AD istemciniz için etkinleştirilmiş olmalıdır.

```powershell 
 $AadClientId = "My-AAD-Client-Id"
 $AadClientSecret = "My-AAD-Client-Secret"

 $key vault = New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption
``` 

### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Disk şifreleme parolası bir KEK ile şifrelenmedi
Anahtar kasasında gizli dizi ayarlamak için [set-AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret)kullanın. Parola Base64 dizesi olarak kodlanır ve sonra anahtar kasasına yüklenir. Ayrıca, anahtar kasasında gizli dizi oluşturduğunuzda, aşağıdaki etiketleri ayarlandığından emin olun.

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


[Kek kullanmadan işletim sistemi diskini eklemek](#without-using-a-kek)için bir sonraki adımda `$secretUrl` kullanın.

### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Disk şifrelemesi gizli anahtarı bir KEK ile şifrelendi
Gizli anahtar Kasası'na yüklemeden önce bir anahtar şifreleme anahtarı kullanarak isteğe bağlı olarak şifreleyebilirsiniz. Anahtar şifreleme anahtarını kullanarak parolayı ilk kez şifrelemek için wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) 'sini kullanın. Bu Wrap işleminin çıktısı, [`Set-AzKeyVaultSecret`](/powershell/module/az.keyvault/set-azkeyvaultsecret) cmdlet 'ini kullanarak bir gizli dizi olarak karşıya yükleyebileceğiniz Base64 URL kodlamalı bir dizedir.

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

[Kek kullanarak işletim sistemi diskini eklemek](#using-a-kek)için bir sonraki adımda `$KeyEncryptionKey` ve `$secretUrl` kullanın.

##  <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Bir işletim sistemi diski iliştirmeye çalıştığınızda gizli bir URL belirtin

###  <a name="without-using-a-kek"></a>KEK kullanmadan
İşletim sistemi diskini iliştirirken `$secretUrl`geçirmeniz gerekir. URL "Disk şifreleme gizli bir KEK ile şifrelenmiş değil" bölümünde oluşturuldu.
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
İşletim sistemi diskini iliştirmeniz durumunda `$KeyEncryptionKey` geçirin ve `$secretUrl`. URL "Disk şifreleme gizli bir KEK ile şifrelenmiş" bölümünde oluşturuldu.
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
