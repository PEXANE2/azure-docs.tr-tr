---
title: Ek-IaaS VM 'Leri için Azure disk şifrelemesi | Microsoft Docs
description: Bu makalede, Windows ve Linux IaaS VM 'Leri için Microsoft Azure disk şifrelemesi eki bulunur.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 4c065e1970a01f7e3737f8bd99672c84f2019bfe
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71822330"
---
# <a name="appendix-for-azure-disk-encryption"></a>Azure disk şifrelemesi için ek 

Bu makale, [IaaS VM 'leri Için Azure disk şifrelemesi](azure-security-disk-encryption-overview.md)için bir ek. Bağlamı anlamak için öncelikle IaaS VM 'Leri için Azure disk şifrelemesi makalelerini okuduğunuzdan emin olun. Bu makalede önceden şifrelenen VHD 'Lerin ve diğer görevlerin nasıl hazırlanacağı açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-your-subscription"></a>Aboneliğinize bağlanma
Başlamadan önce, [Önkoşullar](azure-security-disk-encryption-prerequisites.md) makalesini gözden geçirin. Tüm önkoşullar karşılandıktan sonra, aşağıdaki cmdlet 'leri çalıştırarak aboneliğinize bağlanın:

### <a name="bkmk_ConnectPSH"></a>PowerShell ile aboneliğinize bağlanma

1. Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:

     ```powershell
     Connect-AzAccount 
     ```
2. Birden çok aboneliğiniz varsa ve kullanmak üzere bir tane belirtmek istiyorsanız, hesabınıza yönelik abonelikleri görmek için aşağıdakileri yazın:
     
     ```powershell
     Get-AzSubscription
     ```
3. Kullanmak istediğiniz aboneliği belirtmek için şunu yazın:
 
     ```powershell
      Select-AzSubscription -SubscriptionName <Yoursubscriptionname>
     ```
4. Yapılandırılmış aboneliğin doğru olduğunu doğrulamak için şunu yazın:
     
     ```powershell
     Get-AzSubscription
     ```
5. Gerekirse, [Connect-AzureAD](/powershell/module/azuread/connect-azuread)Ile Azure AD 'ye bağlanın.
     
     ```powershell
     Connect-AzureAD
     ```
6. Azure disk şifrelemesi cmdlet 'lerinin yüklendiğini doğrulamak için şunu yazın:
     
     ```powershell
     Get-command *diskencryption*
     ```
                       
7. Gerekirse Azure PowerShell ve [Azuread](/powershell/module/azuread) [ile çalışmaya](/powershell/azure/get-started-azureps) başlama konusunu gözden geçirin.

### <a name="bkmk_ConnectCLI"></a>Azure CLı ile aboneliğinize bağlanma

1. [Az Login](/cli/azure/authenticate-azure-cli#sign-in-interactively)komutuyla Azure 'da oturum açın. 
     
     ```azurecli
     az login
     ```

2. Oturum açmak için bir kiracı seçmek istiyorsanız şunu kullanın:
    
     ```azurecli
     az login --tenant <tenant>
     ```

3. Birden çok aboneliğiniz varsa ve belirli bir tane belirtmek istiyorsanız, [az Account List](/cli/azure/account#az-account-list) komutuyla abonelik listenizi alın ve [az Account set](/cli/azure/account#az-account-set)ile belirtin.
     
     ```azurecli
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Yüklü sürümü doğrulayın.
     
     ```azurecli
        az --version
     ``` 

5. Gerekirse [Azure clı 2,0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli) konusunu inceleyin. 

## <a name="sample-powershell-scripts-for-azure-disk-encryption"></a>Azure disk şifrelemesi için örnek PowerShell betikleri 

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

### <a name="bkmk_prereq-script"></a>Azure disk şifrelemesi önkoşulları PowerShell betiğini kullanma
Azure disk şifrelemesi önkoşullarını zaten biliyorsanız, [Azure disk şifrelemesi önkoşulları PowerShell betiğini](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 )kullanabilirsiniz. Bu PowerShell betiğini kullanmayla ilgili bir örnek için bkz. [VM 'Yi şifreleme hızlı başlangıç](../virtual-machines/linux/disk-encryption-powershell-quickstart.md). Mevcut bir kaynak grubundaki mevcut VM 'Ler için tüm diskleri şifrelemek üzere, 211. satırdan başlayarak betiğin bir bölümünden açıklamaları kaldırabilirsiniz. 

Aşağıdaki tabloda, PowerShell komut dosyasında hangi parametrelerin kullanılabileceği gösterilmektedir: 


|Parametre|Açıklama|Zorunludur|
|------|------|------|
|$resourceGroupName| Anahtar kasasının ait olduğu kaynak grubunun adı.  Mevcut değilse, bu ada sahip yeni bir kaynak grubu oluşturulur.| True|
|$keyVaultName|Şifreleme anahtarlarının yerleştirileceği anahtar kasasının adı. Bu ada sahip yeni bir kasa, mevcut değilse oluşturulur.| True|
|$location|Anahtar kasasının konumu. Şifrelenecek anahtar kasası ve VM 'Lerin aynı konumda olduğundan emin olun. `Get-AzLocation` komutu ile bir konum listesi alın.|True|
|$subscriptionId|Kullanılacak Azure aboneliğinin tanımlayıcısı.  Abonelik Kimliğinizi `Get-AzSubscription` komutu ile alabilirsiniz.|True|
|$aadAppName|Anahtar kasasına gizli diziler yazmak için kullanılacak Azure AD uygulamasının adı. Bu ada sahip bir uygulama yoksa yeni bir uygulama oluşturulur. Bu uygulama zaten varsa, aadClientSecret parametresini betiğe geçirin.|False|
|$aadClientSecret|Daha önce oluşturulan Azure AD uygulamasının istemci gizli anahtarı.|False|
|$keyEncryptionKeyName|Anahtar Kasası 'nda isteğe bağlı anahtar şifreleme anahtarının adı. Bu ada sahip yeni bir anahtar, yoksa oluşturulur.|False|


## <a name="resource-manager-templates"></a>Resource Manager şablonları

<!--   - [Create a key vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create) -->

### <a name="encrypt-or-decrypt-vms-without-an-azure-ad-app"></a>Azure AD uygulaması olmadan VM 'Leri şifreleme veya şifrelerini çözme


- [Mevcut veya çalışan IaaS Windows VM 'lerinde disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)
- [Var olan veya IaaS Windows VM 'Leri çalıştıran disk şifrelemesini devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad)
- [Mevcut veya çalışan IaaS Linux VM 'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)  
  - [Çalışan bir Linux VM 'de şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) 
    - Şifrelemeyi devre dışı bırakmak yalnızca Linux sanal makineleri için veri birimlerinde kullanılabilir.  

### <a name="encrypt-or-decrypt-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerini şifreleme veya şifresini çözme

- [Çalışan bir Linux sanal makine ölçek kümesi üzerinde disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [Çalışan bir Windows sanal makine ölçek kümesi üzerinde disk şifrelemeyi etkinleştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [Bir sıçrama kutusu ile Linux VM 'leri sanal makine ölçek kümesi dağıtma ve Linux VMSS üzerinde şifrelemeyi mümkün](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [Bir sıçrama kutusu ile bir sanal makine ölçek kümesi Windows VM 'Leri dağıtma ve Windows VMSS 'de şifrelemeyi izin veren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [Çalışan bir Linux sanal makine ölçek kümesi üzerinde disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [Çalışan bir Windows sanal makine ölçek kümesi üzerinde disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

### <a name="encrypt-or-decrypt-vms-with-an-azure-ad-app-previous-release"></a>VM 'Leri bir Azure AD uygulamasıyla şifreleme veya şifrelerini çözme (önceki sürüm) 
 
- [Mevcut veya çalışan IaaS Windows VM 'lerinde disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)

- [Mevcut veya çalışan IaaS Linux VM 'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)    

- [Windows IaaS çalıştırırken disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) 

-  [Çalışan bir Linux VM 'de şifrelemeyi devre dışı bırakma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm) 
    - Şifrelemeyi devre dışı bırakmak yalnızca Linux sanal makineleri için veri birimlerinde kullanılabilir. 

- [Market 'ten yeni IaaS Windows VM 'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)
    - Bu şablon, Windows Server 2012 Galeri görüntüsünü kullanan yeni bir şifrelenmiş Windows VM oluşturur.

- [Galeri görüntüsünden yeni bir şifrelenmiş Windows IaaS yönetilen disk VM 'si oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)
    - Bu şablon, Windows Server 2012 Galeri görüntüsünü kullanarak yönetilen disklere sahip yeni bir şifrelenmiş Windows VM oluşturur.

- [Önceden şifrelenen bir VHD/depolama blobundan yeni bir şifrelenmiş yönetilen disk oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)
    - Önceden şifrelenen bir VHD ve buna karşılık gelen şifreleme ayarları sağlanmış olan yeni bir şifrelenmiş yönetilen disk oluşturur

- [Azure AD İstemci sertifikası parmak izi kullanarak çalışan bir Windows VM 'de disk şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-aad-client-cert)
    


## <a name="bkmk_preWin"></a>Önceden şifrelenmiş bir Windows VHD hazırlama
Aşağıdaki bölümler, Azure IaaS 'de şifrelenmiş bir VHD olarak dağıtıma önceden şifrelenmiş bir Windows VHD 'si hazırlamak için gereklidir. Azure Site Recovery veya Azure 'da yeni bir Windows VM (VHD) hazırlamak ve önyüklemek için bu bilgileri kullanın. Bir VHD 'yi hazırlama ve karşıya yükleme hakkında daha fazla bilgi için bkz. [Genelleştirilmiş BIR VHD 'Yi karşıya yükleme ve Azure 'da yeni VM 'ler oluşturmak için kullanma](../virtual-machines/windows/upload-generalized-managed.md).

### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Grup ilkesini, işletim sistemi koruması için TPM olmayan bir şekilde güncelleştirme
**Yerel bilgisayar ilkesi** > **bilgisayar yapılandırması** > **Yönetim Şablonları** >  Windows altında bulacağınız BitLocker Grup İlkesi ayarını **BitLocker Sürücü Şifrelemesi**yapılandırın. **Bileşenler**. Bu ayarı **Işletim sistemi sürücüleriyle**değiştirin  > **Başlangıçta ek kimlik doğrulaması gerektir** >  aşağıdaki şekilde GÖSTERILDIĞI gibi**uyumlu bir TPM olmadan BitLocker 'a izin verir**:

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

### <a name="install-bitlocker-feature-components"></a>BitLocker özellik bileşenlerini yükler
Windows Server 2012 ve üzeri için aşağıdaki komutu kullanın:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Windows Server 2008 R2 için aşağıdaki komutu kullanın:

    ServerManagerCmd -install BitLockers
### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>@No__t-0 kullanarak işletim sistemi birimini BitLocker için hazırlama
İşletim sistemi bölümünü sıkıştırmak ve makineyi BitLocker 'a hazırlamak için gerekirse [BdeHdCfg](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-basic-deployment) ' yi yürütün:

    bdehdcfg -target c: shrink -quiet 


### <a name="protect-the-os-volume-by-using-bitlocker"></a>BitLocker 'ı kullanarak işletim sistemi birimini koruma
Dış anahtar koruyucusu kullanarak önyükleme biriminde şifrelemeyi etkinleştirmek için [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) komutunu kullanın. Dış anahtarı (. bek dosyası) dış sürücüye veya birime da yerleştirin. Şifreleme, bir sonraki yeniden başlatmanın ardından sistem/önyükleme biriminde etkinleştirilir.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> BitLocker 'ı kullanarak dış anahtarı almak için VM 'yi ayrı bir veri/kaynak VHD ile hazırlayın.

## <a name="bkmk_LinuxRunning"></a>Çalışan bir Linux VM 'de işletim sistemi sürücüsü şifreleme

### <a name="prerequisites-for-os-disk-encryption"></a>İşletim sistemi disk şifrelemesi önkoşulları

* VM, [Azure Disk Şifrelemesi tarafından desteklenen işletim sistemlerinde](azure-security-disk-encryption-prerequisites.md#linux) listelendiği gibi işletim sistemi disk şifrelemesi ile uyumlu bir dağıtım kullanıyor olmalıdır: Linux 
* VM, Azure Resource Manager ' deki Market görüntüsünden oluşturulmalıdır.
* En az 4 GB RAM (önerilen boyut 7 GB) ile Azure VM.
* (RHEL ve CentOS için) SELinux 'u devre dışı bırakın. SELinux 'u devre dışı bırakmak için bkz. "4.4.2. SELinux kullanıcısının ve sanal makine üzerinde [yönetici kılavuzundaki](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) SELinux devre dışı bırakılıyor.
* SELinux 'u devre dışı bıraktıktan sonra, VM 'yi en az bir kez yeniden başlatın.

### <a name="steps"></a>Adımlar
1. Daha önce belirtilen dağıtımlardan birini kullanarak bir VM oluşturun.

   CentOS 7,2 için işletim sistemi disk şifrelemesi özel bir görüntü aracılığıyla desteklenir. Bu görüntüyü kullanmak için, VM 'yi oluştururken SKU olarak "7.2 n" belirtin:

   ```powershell
    Set-AzVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
   ```
2. VM 'yi gereksinimlerinize göre yapılandırın. Tüm (OS + veri) sürücüleri şifreleyecekseniz, veri sürücülerinin belirtilmesi ve/etc/fstabı tarafından takılabilir olması gerekir.

   > [!NOTE]
   > UUID = kullanın... blok cihaz adını (örneğin,/dev/sdb1) belirtmek yerine/etc/fstab içinde veri sürücüleri belirtmek için. Şifreleme sırasında, sanal makine üzerindeki sürücü sırası değişir. VM 'niz belirli bir blok cihazları sıraya alıyorsa, şifreleme sonrasında onları bağlayamaz.

3. SSH oturumlarının oturumunu kapatın.

4. İşletim sistemini şifrelemek için, şifrelemeyi etkinleştirdiğiniz sırada volumeType 'ı **All** veya **OS** olarak belirtin.

   > [!NOTE]
   > @No__t-0 hizmetleri olarak çalıştırmayan tüm Kullanıcı alanı işlemlerinin bir `SIGKILL` ile sonlandırılmalıdır. VM 'yi yeniden başlatın. Çalışan bir sanal makinede işletim sistemi disk şifrelemesini etkinleştirdiğinizde, VM kapalı kalma süresini planlayın.

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
İşletim sistemi şifreleme ilerlemesini üç şekilde izleyebilirsiniz:

* @No__t-0 cmdlet 'ini kullanın ve Ilerlemedurumuiletisi alanını inceleyin:
    ```powershell
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
  VM "işletim sistemi disk şifrelemesi başlatıldı" değerine ulaştıktan sonra, Premium Depolama ile desteklenen bir VM 'de 40 dakika 50 dakika sürer.

  Walınuxagent içindeki [sorun #388](https://github.com/Azure/WALinuxAgent/issues/388) nedeniyle, `OsVolumeEncrypted` ve `DataVolumesEncrypted`, bazı dağıtımlarda `Unknown` olarak gösterilir. Walınuxagent sürüm 2.1.5 ve sonrasında bu sorun otomatik olarak düzeltilir. Çıkışta `Unknown` görürseniz, Azure Kaynak Gezgini kullanarak disk şifreleme durumunu doğrulayabilirsiniz.

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

  InstanceView 'da sürücülerinizin şifreleme durumunu görmek için aşağı kaydırın.

  ![VM örnek görünümü](./media/azure-security-disk-encryption/vm-instanceview.png)

* [Önyükleme tanılarına](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)bakın. ADE uzantısından iletiler `[AzureDiskEncryption]` ' dan önce gelmelidir.

* SSH aracılığıyla VM 'de oturum açın ve şu kaynaktan uzantı günlüğünü alın:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

  İşletim sistemi şifrelemesi devam ederken VM 'de oturum açmanızı önermiyoruz. Günlükleri yalnızca diğer iki yöntem başarısız olduğunda kopyalayın.

## <a name="bkmk_preLinux"></a>Önceden şifrelenen bir Linux VHD hazırlama
Önceden şifrelenen VHD 'ler için hazırlık, dağıtıma bağlı olarak farklılık gösterebilir. [Ubuntu 16](#bkmk_Ubuntu), [openSUSE 13,2](#bkmk_openSUSE)ve [CentOS 7](#bkmk_CentOS) ' yi hazırlama örnekleri mevcuttur. 

### <a name="bkmk_Ubuntu"></a>Ubuntu 16
Aşağıdaki adımları uygulayarak dağıtım yüklemesi sırasında şifrelemeyi yapılandırın:

1. Diskleri bölümleyerek **şifrelenmiş birimleri Yapılandır** ' ı seçin.

   ![Ubuntu 16,04 kurulumu-şifrelenmiş birimleri yapılandırma](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Şifrelenmemelidir olması gereken ayrı bir önyükleme sürücüsü oluşturun. Kök sürücünüzü şifreleyin.

   ![Ubuntu 16,04 kurulumu-şifrelenecek cihazları seçin](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Bir parola girin. Bu, anahtar kasasına yüklediğiniz parola olur.

   ![Ubuntu 16,04 kurulumu-parola sağla](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Bölümleme bitti.

   ![Ubuntu 16,04 kurulumu-bölümlendirme sonu](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. VM 'yi önyüklediğinizde ve bir parola istendiğinde, adım 3 ' te verdiğiniz parolayı kullanın.

   ![Ubuntu 16,04 kurulumu-önyüklemede parola sağla](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. [Bu yönergeleri](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/)kullanarak VM 'yi Azure 'a yükleme için hazırlayın. Henüz son adımı çalıştırmayın (VM 'nin sağlamasını kaldırma).

Aşağıdaki adımları uygulayarak şifrelemeyi Azure ile çalışacak şekilde yapılandırın:

1. Aşağıdaki betikteki içerikle birlikte/usr/local/sbin/azure_crypt_key.sh altında bir dosya oluşturun. Azure tarafından kullanılan parola dosya adı olduğundan KeyFileName 'e dikkat edin.

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

3. Windows 'da *azure_crypt_key. sh* öğesini düzenliyorsanız ve Linux 'a kopyaladıysanız `dos2unix /usr/local/sbin/azure_crypt_key.sh` ' i çalıştırın.

4. Betiğe yürütülebilir izinleri ekle:
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
6. @No__t-1 ' in etkili olması için initramfs 'yi güncelleştirmek üzere `update-initramfs -u -k all` ' yı çalıştırın.

7. Artık VM 'yi sağlamayı seçebilirsiniz.

   ![Ubuntu 16,04 kurulumu-güncelleştirme-ınitramfs](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Sonraki adıma geçin ve VHD 'nizi Azure 'a yükleyin.

### <a name="bkmk_openSUSE"></a>openSUSE 13,2
Dağıtım yüklemesi sırasında şifrelemeyi yapılandırmak için aşağıdaki adımları uygulayın:
1. Diskleri bölümlediğinizde, **birim grubunu şifreleyin**' ı seçin ve ardından bir parola girin. Bu, anahtar kasanıza yüklediğiniz paroladır.

   ![openSUSE 13,2 kurulumu-birim grubunu şifreleme](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Parolanızı kullanarak VM 'yi önyükleyin.

   ![openSUSE 13,2 kurulumu-önyüklemede parola sağla](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. [Azure için BIR SLES veya openSUSE sanal makinesi hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131)bölümündeki YÖNERGELERI izleyerek VM 'yi Azure 'a yüklemek üzere hazırlayın. Henüz son adımı çalıştırmayın (VM 'nin sağlamasını kaldırma).

Şifrelemeyi Azure ile çalışacak şekilde yapılandırmak için aşağıdaki adımları uygulayın:
1. /Etc/Dracut.conf 'yi düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. /Usr/lib/Dracut/Modules.d/90crypt/Module-Setup.sh dosyasının sonuna kadar bu satırları açıklama olarak ekleyin:
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

3. /Usr/lib/Dracut/Modules.d/90crypt/Parse-Crypt.sh dosyasının başına şu satırı ekleyin:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm yinelemelerini değiştirin:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   yerine şunu yazın:
   ```bash
    if [ 1 ]; then
   ```
4. /Usr/lib/Dracut/Modules.d/90crypt/cryptroot-ask.sh düzenleyin ve "# Open LUKS cihazına" ekleyin:

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

6. Artık VM 'nin sağlamasını yapabilir ve VHD 'nizi Azure 'a yükleyebilirsiniz.

### <a name="bkmk_CentOS"></a>CentOS 7
Dağıtım yüklemesi sırasında şifrelemeyi yapılandırmak için aşağıdaki adımları uygulayın:
1. Diskleri bölümlediğinizde verilerimi **şifreleyin** ' ı seçin.

   ![CentOS 7 kurulumu-yükleme hedefi](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Kök bölüm için **şifreleme** seçildiğinden emin olun.

   ![CentOS 7 kurulumu-kök bölüm için şifrelemeyi seçin](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Bir parola girin. Bu, anahtar kasanıza yükleyeceksiniz.

   ![CentOS 7 kurulumu-parola sağla](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. VM 'yi önyüklediğinizde ve bir parola istendiğinde, adım 3 ' te verdiğiniz parolayı kullanın.

   ![CentOS 7 kurulumu-önyükleme sırasında parola girin](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. [Azure Için CentOS tabanlı bir sanal makine hazırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70)konusundaki "CentOS 7.0 +" yönergelerini kullanarak VM 'yi Azure 'a yükleme için hazırlayın. Henüz son adımı çalıştırmayın (VM 'nin sağlamasını kaldırma).

6. Artık VM 'nin sağlamasını yapabilir ve VHD 'nizi Azure 'a yükleyebilirsiniz.

Şifrelemeyi Azure ile çalışacak şekilde yapılandırmak için aşağıdaki adımları uygulayın:

1. /Etc/Dracut.conf 'yi düzenleyin ve aşağıdaki satırı ekleyin:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. /Usr/lib/Dracut/Modules.d/90crypt/Module-Setup.sh dosyasının sonuna kadar bu satırları açıklama olarak ekleyin:
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

3. /Usr/lib/Dracut/Modules.d/90crypt/Parse-Crypt.sh dosyasının başına şu satırı ekleyin:
   ```bash
    DRACUT_SYSTEMD=0
   ```
   Ve tüm yinelemelerini değiştirin:
   ```bash
    if [ -z "$DRACUT_SYSTEMD" ]; then
   ```
   -
   ```bash
    if [ 1 ]; then
   ```
4. /Usr/lib/Dracut/Modules.d/90crypt/cryptroot-ask.sh 'yi düzenleyin ve "# Open LUKS cihazından sonra aşağıdakini ekleyin:
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
5. Initrd 'yi güncelleştirmek için "/usr/sbin/Dracut-f-v" öğesini çalıştırın.

![CentOS 7 kurulumu-/usr/sbin/Dracut-f-v çalıştırma](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

## <a name="bkmk_UploadVHD"></a>Şifrelenmiş VHD 'YI bir Azure depolama hesabına yükleme
BitLocker şifrelemesi veya DM-Crypt şifrelemesi etkinleştirildikten sonra, yerel şifreli VHD 'nin depolama hesabınıza yüklenmesi gerekir.
```powershell
    Add-AzVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]
```
## <a name="bkmk_UploadSecret"></a>Önceden şifrelenen VM 'nin parolasını anahtar kasanıza yükleme
Bir Azure AD uygulaması (önceki sürüm) kullanılarak şifrelerken, daha önce edindiğiniz disk şifreleme parolasının anahtar kasanıza gizli bir parola olarak yüklenmesi gerekir. Anahtar kasasının Azure AD istemciniz için disk şifrelemesi ve izinlerin etkinleştirilmiş olması gerekir.

```powershell 
 $AadClientId = "My-AAD-Client-Id"
 $AadClientSecret = "My-AAD-Client-Secret"

 $key vault = New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
 Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption
``` 

### <a name="bkmk_SecretnoKEK"></a>Disk şifreleme parolası bir KEK ile şifrelenmedi
Anahtar kasasında gizli dizi ayarlamak için [set-AzKeyVaultSecret](/powershell/module/az.keyvault/set-azkeyvaultsecret)kullanın. Bir Windows sanal makineniz varsa, bek dosyası bir Base64 dizesi olarak kodlanır ve sonra `Set-AzKeyVaultSecret` cmdlet 'ini kullanarak anahtar kasanıza yüklenir. Linux için parola Base64 dizesi olarak kodlanır ve sonra anahtar kasasına yüklenir. Ayrıca, anahtar kasasında gizli dizi oluşturduğunuzda aşağıdaki etiketlerin ayarlandığından emin olun.

#### <a name="windows-bek-file"></a>Windows BEK dosyası
```powershell
# Change the VM Name, key vault name, and specify the path to the BEK file.
$VMName ="MySecureVM"
$BEKFilepath = "C:\test\BEK\12345678-90AB-CDEF-A1B2-C3D4E5F67890A.BEK"
$VeyVaultName ="MySecureVault"

# Get the name of the BEK file from the BEK file path. This will be a tag for the key vault secret.
$BEKFileName =  Split-Path $BEKFilepath -Leaf

# These tags will be added to the key vault secret so you can easily see which BEK file belongs to which VM.
$tags = @{“MachineName” = “$VMName”;"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "$BEKFileName"}

# Convert the BEK file to a Base64 string.
$FileContentEncoded = [System.convert]::ToBase64String((Get-Content -Path $BEKFilepath -Encoding Byte))

# Create a new secret in the vault from the converted BEK file. 
# The file is converted to a secure string before import into the key vault

$SecretName = [guid]::NewGuid().ToString()
$SecureSecretValue = ConvertTo-SecureString $FileContentEncoded -AsPlainText -Force
$Secret = Set-AzKeyVaultSecret -VaultName $VeyVaultName -Name $SecretName -SecretValue $SecureSecretValue -tags $tags

# Show the secret's URL and store it as a variable. This is used as -DiskEncryptionKeyUrl in Set-AzVMOSDisk when you attach your OS disk. 
$SecretUrl=$secret.Id
$SecretUrl
```

#### <a name="linux"></a>Linux
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


[Kek kullanmadan işletim sistemi diski eklemek](#bkmk_URLnoKEK)için bir sonraki adımda `$secretUrl` kullanın.

### <a name="bkmk_SecretKEK"></a>Disk şifrelemesi gizli anahtarı bir KEK ile şifrelendi
Gizli anahtarı anahtar kasasına yüklemeden önce, anahtar şifreleme anahtarını kullanarak isteğe bağlı olarak şifreleyebilirsiniz. Anahtar şifreleme anahtarını kullanarak parolayı ilk kez şifrelemek için wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) 'sini kullanın. Bu Wrap işleminin çıktısı, [`Set-AzKeyVaultSecret`](/powershell/module/az.keyvault/set-azkeyvaultsecret) cmdlet 'ini kullanarak bir gizli dizi olarak karşıya yükleyebileceğiniz Base64 URL kodlamalı bir dizedir.

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

[Kek kullanarak işletim sistemi diskini eklemek](#bkmk_URLKEK)için bir sonraki adımda `$KeyEncryptionKey` ve `$secretUrl` ' i kullanın.

##  <a name="bkmk_SecretURL"></a>Bir işletim sistemi diski iliştirmeye çalıştığınızda gizli bir URL belirtin

###  <a name="bkmk_URLnoKEK"></a>KEK kullanmadan
İşletim sistemi diskini iliştirirken, `$secretUrl` geçirmeniz gerekir. URL, "bir KEK ile şifrelenmemiş disk şifrelemesi" bölümünde oluşturulmuştur.
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
### <a name="bkmk_URLKEK"></a>KEK kullanma
İşletim sistemi diskini iliştirmeniz durumunda `$KeyEncryptionKey` ve `$secretUrl` ' i geçirin. URL, "bir KEK ile şifrelenen disk şifreleme gizli dizisi" bölümünde oluşturulmuştur.
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
