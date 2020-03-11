---
title: Linux sanal makineleri için Azure disk şifrelemesi sorunlarını giderme
description: Bu makalede, Linux VM 'Leri için Microsoft Azure disk şifrelemesi için sorun giderme ipuçları sunulmaktadır.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: eeacea9e3305865881747801100dc17770b7df63
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970471"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk şifrelemesi sorun giderme kılavuzu

Bu, BT uzmanları, bilgi güvenlik Çözümleyicileri ve bulut yöneticileri, kuruluşları Azure Disk şifrelemesi kullanın kılavuzudur. Bu makalede, disk şifreleme ilgili sorunları gidermeye yardımcı olmaktır.

Aşağıdaki adımlardan herhangi birini gerçekleştirmeden önce, şifrelemeye çalıştığınız VM 'Lerin [desteklenen VM boyutları ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems)arasında olduğundan ve tüm önkoşulları karşıladığınızı doğrulayın:

- [VM 'Ler için ek gereksinimler](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

 

## <a name="troubleshooting-linux-os-disk-encryption"></a>Linux işletim sistemi disk şifrelemesi sorunlarını giderme

Linux işletim sistemi (OS) disk şifrelemesi, işletim sistemi sürücüsünü tam disk şifreleme işlemini çalıştırmadan önce çıkarmanız gerekir. Bir hata iletisi, sürücü çıkarılamıyor, "sonra çıkarılamadı..." oluşma olasılığı yüksektir.

Bu hata, desteklenen hisse senedi Galerisi görüntüsünden değiştirilen bir ortama sahip bir VM 'de işletim sistemi disk şifrelemesi denendiğinde ortaya çıkabilir. Desteklenen görüntü sapmalar, işletim sistemi sürücüsünü çıkaramadı uzantının özelliği sayesinde engelleyebilir. Sapmalar örnekleri aşağıdaki öğeleri içerebilir:
- Artık eşleşen bir desteklenen dosya sistemi veya bölümleme düzeni özelleştirilmiş görüntüler.
- Yüklü ve çalışır işletim sisteminde şifreleme önce olduklarında, SAP, MongoDB, Apache Cassandra ve Docker gibi büyük uygulamalar desteklenmez. Azure Disk şifrelemesi, güvenli bir şekilde gerektiği gibi disk şifreleme için hazırlık işletim sistemi sürücüsünün bu işlemlerin kapatılması silemiyor. Dosya tanıtıcılarını Aç için işletim sistemi sürücüsünü bulunduran hala etkin işlemler varsa, işletim sistemi sürücüsünü, işletim sistemi sürücüsünü şifrelemek için bir neden, kaldırılan olamaz. 
- Özel Kapat zaman yakınlık etkinleştiriliyor şifreleme için çalışan komut dosyaları veya diğer durumunda değişiklikler VM'de şifreleme işlemi sırasında yapılmıştır. Birden çok uzantı aynı anda yürütmek için bir Azure Resource Manager şablonu tanımlar veya bir özel betik uzantısı ya da diğer eylem aynı anda disk şifrelemesi için çalıştığında, bu çakışma oluşabilir. Serileştirme ve yalıtma gibi adımlar sorunu çözebilir.
- Güvenliği artırılmış Linux (SELinux) çıkarma adım başarısız olduğu için şifreleme, etkinleştirmeden önce devre dışı edilmemiş. Şifreleme işlemi tamamlandıktan sonra SELinux yeniden iler hale.
- İşletim sistemi diski, bir mantıksal birim Yöneticisi (LVM) şeması kullanır. Sınırlı LVM veri disk desteği kullanılabilir olsa da, bir LVM işletim sistemi diski yok.
- Olmayan en düşük bellek gereksinimleri karşılanıyor (7 GB, işletim sistemi disk şifreleme için önerilir).
- Veri sürücüleri /mnt/ dizini veya birbiriyle (örneğin, /mnt/data1, /mnt/data2, /data3 + /data3/data4) altında yinelemeli olarak sağlar.

## <a name="update-the-default-kernel-for-ubuntu-1404-lts"></a>Ubuntu 14,04 LTS için varsayılan çekirdeği güncelleştirme

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

Azure disk şifrelemesi işlemleri, Azure sanal makine aracısının desteklenmeyen sürümlerini kullanan sanal makine görüntülerinde başarısız olabilir. 2\.2.38 'den önceki aracı sürümlerine sahip Linux görüntülerinin, Şifrelemeyi etkinleştirmeden önce güncelleştirilmeleri gerekir. Daha fazla bilgi için bkz. Azure 'da [Azure Linux aracısını güncelleştirme](../extensions/update-linux-agent.md) ve [sanal makine aracıları için en düşük sürüm desteği](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

Microsoft. Azure. Security. AzureDiskEncryption veya Microsoft. Azure. Security. AzureDiskEncryptionForLinux Guest aracı uzantısının doğru sürümü de gereklidir. Uzantı sürümleri, Azure sanal makine Aracısı önkoşulları karşılanacağı ve sanal makine aracısının desteklenen bir sürümü kullanıldığında platform tarafından otomatik olarak güncelleştirilir ve güncelleştirilir.

Microsoft. OSTCExtensions. AzureDiskEncryptionForLinux uzantısı kullanım dışı bırakılmış ve artık desteklenmiyor.  

## <a name="unable-to-encrypt-linux-disks"></a>Linux diskleri şifrelenemiyor

Bazı durumlarda, disk şifreleme "İşletim sistemi şifreleme kullanmaya disk" durmuş görünüyor Linux ve SSH devre dışı bırakıldı. Şifreleme işlemi, bir stok galeri görüntüsü üzerinde tamamlamak için 3-16 saat arasında sürebilir. Çoklu terabayt boyutlu veri diski eklenirse, işlem gün sürebilir.

Linux işletim sistemi disk şifreleme dizisi, işletim sistemi sürücüsünü geçici olarak çıkarır. Ardından, şifrelenmiş durumunda remounts önce tüm işletim sistemi diskinin blok blok şifreleme gerçekleştirir. Linux disk şifrelemesi, şifreleme sürerken VM 'nin eşzamanlı kullanımına izin vermez. VM performans özellikleri, şifrelemeyi tamamlamak için gereken süreyi önemli bir fark yapabilirsiniz. Bu özellikleri, disk ve depolama hesabı olup standart veya premium (SSD) depolama boyutunu içerir.

Şifreleme durumunu denetlemek için [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) komutundan döndürülen **ilerlemedurumuiletisi** alanını yoklayın. İşletim sistemi sürücüsünü şifrelenir, ancak VM hizmet durumu girer ve herhangi bir kesinti devam eden işlemi için SSH devre dışı bırakır. **Encryptionınprogress** iletisi, şifrelemenin devam ettiği sürenin büyük bölümü için rapor bildiriyor. Birkaç saat sonra, bir **VMRestartPending** iletisi VM 'yi yeniden başlatmanızı ister. Örnek:


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

[Yalıtılmış bir ağda disk şifrelemeyi](disk-encryption-isolated-network.md) görme

## <a name="troubleshooting-encryption-status"></a>Şifreleme durumu sorunlarını giderme 

Portal, sanal makine içinde şifrelenmemiş olduktan sonra bile bir disk şifreli olarak görüntülenebilir.  Bu durum, daha yüksek düzeyde Azure disk şifrelemesi yönetim komutları kullanmak yerine, diskin VM içinden doğrudan şifresini kaldırmak için düşük düzey komutlar kullanıldığında meydana gelebilir.  Üst düzey komutlar yalnızca VM 'nin içinden diskin şifresini kaldıramaz, ancak VM 'nin dışında, önemli platform düzeyi şifreleme ayarlarını ve VM ile ilişkili uzantı ayarlarını da güncelleştirir.  Bunlar hizalamayla tutulmazsa, Platform şifreleme durumunu bildiremez veya VM 'yi düzgün şekilde sağlayamaz.   

Azure disk şifrelemesini PowerShell ile devre dışı bırakmak için [Disable-azvmdiskencryption](/powershell/module/az.compute/disable-azvmdiskencryption) ' ı ve ardından [Remove-AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension)' i kullanın. Şifreleme devre dışı olmadan önce Remove-AzVMDiskEncryptionExtension çalıştırma başarısız olur.

CLı ile Azure disk şifrelemesini devre dışı bırakmak için [az VM Encryption Disable](/cli/azure/vm/encryption)seçeneğini kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk şifrelemesi ve bu sorunları gidermeye yönelik bazı yaygın sorunlar hakkında daha fazla öğrendiniz. Bu hizmet ve özellikleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi 'nde disk şifrelemeyi uygulama](../../security-center/security-center-apply-disk-encryption.md)
- [Bekleyen Azure veri şifrelemesi](../../security/fundamentals/encryption-atrest.md)
