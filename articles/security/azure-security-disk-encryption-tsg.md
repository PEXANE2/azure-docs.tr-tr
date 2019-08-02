---
title: Sorun giderme - Iaas Vm'leri için Azure Disk şifrelemesi | Microsoft Docs
description: Bu makale için Microsoft Azure Disk şifrelemesi Windows ve Linux Iaas Vm'leri için sorun giderme ipuçları sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/12/2019
ms.custom: seodec18
ms.openlocfilehash: e2464332727b0ef1e616c04a975df5ac475a7b19
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610280"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk şifrelemesi sorun giderme kılavuzu

Bu, BT uzmanları, bilgi güvenlik Çözümleyicileri ve bulut yöneticileri, kuruluşları Azure Disk şifrelemesi kullanın kılavuzudur. Bu makalede, disk şifreleme ilgili sorunları gidermeye yardımcı olmaktır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="troubleshooting-linux-os-disk-encryption"></a>Linux işletim sistemi disk şifrelemesi sorunlarını giderme

Linux işletim sistemi (OS) disk şifrelemesi, işletim sistemi sürücüsünü tam disk şifreleme işlemini çalıştırmadan önce çıkarmanız gerekir. Bir hata iletisi, sürücü çıkarılamıyor, "sonra çıkarılamadı..." oluşma olasılığı yüksektir.

İşletim sistemi disk şifreleme desteklenen stok galeri görüntüden değiştirildi bir hedef VM ortamında çalıştığında bu hata oluşabilir. Desteklenen görüntü sapmalar, işletim sistemi sürücüsünü çıkaramadı uzantının özelliği sayesinde engelleyebilir. Sapmalar örnekleri aşağıdaki öğeleri içerebilir:
- Artık eşleşen bir desteklenen dosya sistemi veya bölümleme düzeni özelleştirilmiş görüntüler.
- Yüklü ve çalışır işletim sisteminde şifreleme önce olduklarında, SAP, MongoDB, Apache Cassandra ve Docker gibi büyük uygulamalar desteklenmez. Azure Disk şifrelemesi, güvenli bir şekilde gerektiği gibi disk şifreleme için hazırlık işletim sistemi sürücüsünün bu işlemlerin kapatılması silemiyor. Dosya tanıtıcılarını Aç için işletim sistemi sürücüsünü bulunduran hala etkin işlemler varsa, işletim sistemi sürücüsünü, işletim sistemi sürücüsünü şifrelemek için bir neden, kaldırılan olamaz. 
- Özel Kapat zaman yakınlık etkinleştiriliyor şifreleme için çalışan komut dosyaları veya diğer durumunda değişiklikler VM'de şifreleme işlemi sırasında yapılmıştır. Birden çok uzantı aynı anda yürütmek için bir Azure Resource Manager şablonu tanımlar veya bir özel betik uzantısı ya da diğer eylem aynı anda disk şifrelemesi için çalıştığında, bu çakışma oluşabilir. Serileştirme ve yalıtma gibi adımlar sorunu çözebilir.
- Güvenliği artırılmış Linux (SELinux) çıkarma adım başarısız olduğu için şifreleme, etkinleştirmeden önce devre dışı edilmemiş. Şifreleme işlemi tamamlandıktan sonra SELinux yeniden iler hale.
- İşletim sistemi diski, bir mantıksal birim Yöneticisi (LVM) şeması kullanır. Sınırlı LVM veri disk desteği kullanılabilir olsa da, bir LVM işletim sistemi diski yok.
- Olmayan en düşük bellek gereksinimleri karşılanıyor (7 GB, işletim sistemi disk şifreleme için önerilir).
- Veri sürücüleri /mnt/ dizini veya birbiriyle (örneğin, /mnt/data1, /mnt/data2, /data3 + /data3/data4) altında yinelemeli olarak sağlar.
- Diğer Azure Disk şifrelemesi [önkoşulları](azure-security-disk-encryption-prerequisites.md) Linux için karşılanmadığı.

## <a name="bkmk_Ubuntu14"></a>Ubuntu 14,04 LTS için varsayılan çekirdeği güncelleştirme

Ubuntu 14,04 LTS görüntüsü, varsayılan bir 4,4 çekirdek sürümü ile birlikte gelir. Bu çekirdek sürümü, işletim sistemi şifreleme işlemi sırasında, bellek yetersiz olduğundan, dd komutunu yanlış bir şekilde sonlandırdığı bilinen bir sorunla karşılaştı. Bu hata, en son Azure ayarlanmış Linux çekirdekte düzeltildi. Bu hatadan kaçınmak için, görüntüde Şifrelemeyi etkinleştirmeden önce, aşağıdaki komutları kullanarak [Azure ayarlanmış çekirdek 4,15](https://packages.ubuntu.com/trusty/linux-azure) veya sonraki bir sürüme güncelleştirin:

```
sudo apt-get update
sudo apt-get install linux-azure
sudo reboot
```

VM yeni çekirdekte yeniden başlatıldıktan sonra yeni çekirdek sürümü şu kullanılarak onaylanabilir:

```
uname -a
```

## <a name="update-the-azure-virtual-machine-agent-and-extension-versions"></a>Azure sanal makine Aracısı ve uzantı sürümlerini güncelleştirme

Azure disk şifrelemesi işlemleri, Azure sanal makine aracısının desteklenmeyen sürümlerini kullanan sanal makine görüntülerinde başarısız olabilir. 2\.2.38 'den önceki aracı sürümlerine sahip Linux görüntülerinin, Şifrelemeyi etkinleştirmeden önce güncelleştirilmeleri gerekir. Daha fazla bilgi için bkz. Azure 'da [Azure Linux aracısını güncelleştirme](../virtual-machines/extensions/update-linux-agent.md) ve [sanal makine aracıları için en düşük sürüm desteği](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

Microsoft. Azure. Security. AzureDiskEncryption veya Microsoft. Azure. Security. AzureDiskEncryptionForLinux Guest aracı uzantısının doğru sürümü de gereklidir. Uzantı sürümleri, Azure sanal makine Aracısı önkoşulları karşılanacağı ve sanal makine aracısının desteklenen bir sürümü kullanıldığında platform tarafından otomatik olarak güncelleştirilir ve güncelleştirilir.

Microsoft. OSTCExtensions. AzureDiskEncryptionForLinux uzantısı kullanım dışı bırakılmış ve artık desteklenmiyor.  

## <a name="unable-to-encrypt-linux-disks"></a>Linux diskleri şifrelenemiyor

Bazı durumlarda, disk şifreleme "İşletim sistemi şifreleme kullanmaya disk" durmuş görünüyor Linux ve SSH devre dışı bırakıldı. Şifreleme işlemi, bir stok galeri görüntüsü üzerinde tamamlamak için 3-16 saat arasında sürebilir. Çoklu terabayt boyutlu veri diski eklenirse, işlem gün sürebilir.

Linux işletim sistemi disk şifreleme dizisi, işletim sistemi sürücüsünü geçici olarak çıkarır. Ardından, şifrelenmiş durumunda remounts önce tüm işletim sistemi diskinin blok blok şifreleme gerçekleştirir. Şifreleme işlemi devam ederken Windows üzerinde Azure Disk şifrelemesi, eş zamanlı kullanımını VM için Linux Disk şifrelemesi izin vermez. VM performans özellikleri, şifrelemeyi tamamlamak için gereken süreyi önemli bir fark yapabilirsiniz. Bu özellikleri, disk ve depolama hesabı olup standart veya premium (SSD) depolama boyutunu içerir.

Şifreleme durumunu denetlemek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) komutundan döndürülen **ilerlemedurumuiletisi** alanını yoklayın. İşletim sistemi sürücüsünü şifrelenir, ancak VM hizmet durumu girer ve herhangi bir kesinti devam eden işlemi için SSH devre dışı bırakır. **EncryptionInProgress** şifreleme işlemi devam ederken çoğu zaman raporları iletisi. Birkaç saat sonra bir **VMRestartPending** ileti sanal Makineyi yeniden başlatmanızı ister. Örneğin:


```azurepowershell
PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzVMDiskEncryptionStatus -ResourceGroupName "MyVirtualMachineResourceGroup" -VMName "VirtualMachineName"
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Sonra sanal Makineyi yeniden başlatmanız istenir ve VM yeniden başlatıldıktan sonra hedef üzerinde gerçekleştirilecek ilişkin son adımlar ve yeniden başlatma için 2-3 dakika beklemeniz gerekir. Şifreleme son olduğunda durum iletisi değişiklikleri tamamlayın. Bu ileti kullanılabilir olduktan sonra şifrelenmiş işletim sistemi sürücüsünü kullanıma hazır olması beklenir ve VM yeniden kullanılmak üzere hazırdır.

Aşağıdaki durumlarda anlık görüntü veya hemen önce şifreleme alınan yedeklemeyi dön VM geri yükleme öneririz:
   - Daha önce açıklanan yeniden başlatma sırası gerçekleşmezse.
   - Önyükleme bilgileri, ilerleme iletisi ya da diğer hata göstergeleri, işletim sistemi şifreleme bildirirse ortasında bu işlem başarısız oldu. Bu kılavuzda açıklanan "çıkarma başarısız oldu" hata iletisi bir örneğidir.

Ve sonraki girişiminden önce VM özelliklerini yeniden ve tüm önkoşulların karşılandığından emin olun.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Güvenlik duvarının arkasında Azure Disk şifrelemesi sorunlarını giderme
Bağlantı bir güvenlik duvarı, proxy gereksinim veya ağ güvenlik grubu (NSG) ayarları tarafından kısıtlanmış, görevleri gerçekleştirmek için uzantının özelliği kesilmiş olabilir. Durum iletileri "Uzantı durumu sanal makinede kullanılabilir değil." gibi bu kesintiye neden olabilir Beklenen senaryolarda şifrelemeyi tamamlamak başarısız olur. Aşağıdaki bölümlerde, araştırabilirsiniz bazı yaygın bir güvenlik duvarı sorunlar var.

### <a name="network-security-groups"></a>Ağ güvenlik grupları
Uygulanan ağ güvenlik grubu ayarlarını belgelenmiş ağ yapılandırmasını karşılamak uç nokta hala izin vermelidir [önkoşulları](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) disk şifrelemesi için.

### <a name="azure-key-vault-behind-a-firewall"></a>Bir güvenlik duvarının arkasındaki Azure Key Vault

Şifreleme, [Azure AD kimlik bilgileriyle](azure-security-disk-encryption-prerequisites-aad.md)etkinleştirildiğinde, hedef VM hem Azure Active Directory uç noktalarına hem de Key Vault uç noktalarına bağlantı kurulmasına izin vermelidir. Geçerli Azure Active Directory kimlik doğrulama uç noktaları, [Office 365 URL 'leri ve IP adresi aralıkları](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges) belgelerinin 56 ve 59 bölümlerinde saklanır. Key Vault yönergeler, [bir güvenlik duvarının arkasındaki Azure Key Vault erişme](../key-vault/key-vault-access-behind-firewall.md)hakkındaki belgelerde sunulmaktadır.

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service 
VM, yalnızca VM içinden erişilebilen, iyi bilinen yönlendirilemeyen IP adresini (`169.254.169.254`) kullanan [Azure örnek meta veri hizmeti](../virtual-machines/windows/instance-metadata-service.md) uç noktasına erişebilmelidir.  Yerel HTTP trafiğini bu adrese dönüştüren ara sunucu (örneğin, X-Iletilmiş-for üstbilgisi ekleme) desteklenmez.

### <a name="linux-package-management-behind-a-firewall"></a>Bir güvenlik duvarının arkasında Linux paket Yönetimi

Çalışma zamanında, Linux için Azure Disk şifrelemesi şifreleme etkinleştirilmeden önce gerekli Önkoşul bileşenlerini yüklemek için hedef dağıtım ait paket yönetim sistemini kullanır. Güvenlik Duvarı ayarlarını VM indirebilir ve bu bileşenleri yüklemek engellemek, ardından sonraki hataları beklenmektedir. Bu paket yönetim sistemini yapılandırma adımları dağıtım göre farklılık gösterebilir. Bir proxy gerektiğinde Red Hat üzerinde size Abonelik Yöneticisi ve yum düzgün ayarlandığından emin olmanız gerekir. Daha fazla bilgi için [Abonelik Yöneticisi ve yum sorunları nasıl giderilir](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Sunucu Çekirdeği sorunlarını giderme

Windows Server 2016 Server Core üzerinde bdehdcfg bileşeni varsayılan olarak sağlanmaz. Bu bileşen tarafından Azure Disk şifrelemesi gereklidir. Sistem biriminin sanal makinenin yaşam süresi için yalnızca bir kez gerçekleştirilir işletim sistemi birimden bölmek için kullanılır. Bu ikili dosyalar daha sonra şifreleme işlemleri sırasında gerekli değildir.

Bu sorunu çözmek için sunucu çekirdeği üzerinde aynı konuma bir Windows Server 2016 veri merkezi sanal makinesinden aşağıdaki dört dosyaları kopyalayın:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

1. Aşağıdaki komutu girin:

   ```
   bdehdcfg.exe -target default
   ```

1. Bu komut, 550 MB sistem bölümü oluşturur. Sistemi yeniden başlatın.

1. Birimleri denetleyin ve ardından devam etmek için DiskPart kullanın.  

Örneğin:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="troubleshooting-encryption-status"></a>Şifreleme durumu sorunlarını giderme 

Portal, sanal makine içinde şifrelenmemiş olduktan sonra bile bir disk şifreli olarak görüntülenebilir.  Bu durum, daha yüksek düzeyde Azure disk şifrelemesi yönetim komutları kullanmak yerine, diskin VM içinden doğrudan şifresini kaldırmak için düşük düzey komutlar kullanıldığında meydana gelebilir.  Üst düzey komutlar yalnızca VM 'nin içinden diskin şifresini kaldıramaz, ancak VM 'nin dışında, önemli platform düzeyi şifreleme ayarlarını ve VM ile ilişkili uzantı ayarlarını da güncelleştirir.  Bunlar hizalamayla tutulmazsa, Platform şifreleme durumunu bildiremez veya VM 'yi düzgün şekilde sağlayamaz.   

Azure disk şifrelemesini PowerShell ile devre dışı bırakmak için [Disable-azvmdiskencryption](/powershell/module/az.compute/disable-azvmdiskencryption) ' ı ve ardından [Remove-AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension)' i kullanın. Şifreleme devre dışı olmadan önce Remove-AzVMDiskEncryptionExtension çalıştırma başarısız olur.

CLı ile Azure disk şifrelemesini devre dışı bırakmak için [az VM Encryption Disable](/cli/azure/vm/encryption)seçeneğini kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk şifrelemesi ve bu sorunları gidermeye yönelik bazı yaygın sorunlar hakkında daha fazla öğrendiniz. Bu hizmet ve özellikleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Güvenlik Merkezi'nde Azure disk şifrelemesi Uygula](../security-center/security-center-apply-disk-encryption.md)
- [Azure veri bekleme sırasında şifreleme](fundamentals/encryption-atrest.md)
