---
title: Linux VM'leri için Azure Disk Şifrelemesorununa Sorun Giderme
description: Bu makalede, Linux VM'leri için Microsoft Azure Disk Şifrelemesi için sorun giderme ipuçları verilmektedir.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: eeacea9e3305865881747801100dc17770b7df63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970471"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk Şifreleme sorun giderme kılavuzu

Bu kılavuz, kuruluşları Azure Disk Şifrelemesi kullanan BT uzmanları, bilgi güvenliği analistleri ve bulut yöneticileri içindir. Bu makale, disk şifrelemeyle ilgili sorunları giderme de yardımcı olmak içindir.

Aşağıdaki adımlardan herhangi birini almadan önce, şifrelemeye çalıştığınız [VM'lerin desteklenen VM boyutları ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems)arasında olduğundan ve tüm ön koşulları yerine getirebildiğinizden emin olun:

- [VM'ler için ek gereksinimler](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

 

## <a name="troubleshooting-linux-os-disk-encryption"></a>Sorun giderme Linux işletim sistemi disk şifreleme

Linux işletim sistemi (OS) disk şifrelemetam disk şifreleme işlemi ile çalıştırmadan önce işletim sistemi sürücüsünün aşması gerekir. Sürücünün sökülememesi durumunda, "sonra sökülemedi..." hatası iletisi oluşma olasılığı yüksektir.

Bu hata, desteklenen stok galerisi görüntüsünden değiştirilen bir ortamla Bir VM'de işletim sistemi disk şifrelemesi denendiğinde oluşabilir. Desteklenen görüntüdeki sapmalar, uzantın işletim sistemi sürücüsünü sökme yeteneğini etkileyebilir. Sapmaörnekleri aşağıdaki öğeleri içerebilir:
- Özelleştirilmiş görüntüler artık desteklenen bir dosya sistemi veya bölümleme düzeniyle eşleşmiyor.
- SAP, MongoDB, Apache Cassandra ve Docker gibi büyük uygulamalar, şifrelemeden önce işletim sistemi içinde yüklendiğinde ve çalıştırıldığında desteklenmez. Azure Disk Şifreleme, disk şifreleme için işletim sistemi sürücüsünün hazırlanmasında gerektiğinde bu işlemleri güvenli bir şekilde kapatamaz. İşletim sistemi sürücüsünde açık dosya tutamaçları tutan hala etkin işlemler varsa, işletim sistemi sürücüsü sökülemez ve bu da işletim sistemi sürücüsünün şifrelenmemesine neden olur. 
- Etkinleştirilen şifrelemeye yakın zamanda çalışan veya şifreleme işlemi sırasında VM'de başka değişiklikler yapılıyorsa özel komut dosyaları. Bu çakışma, bir Azure Kaynak Yöneticisi şablonu aynı anda yürütülmesi için birden çok uzantı tanımladığında veya özel bir komut dosyası uzantısı veya başka bir eylem disk şifrelemesi için aynı anda çalıştığında meydana gelebilir. Bu tür adımları seri hale getirmek ve yalıtma sorunu çözebilir.
- Security Enhanced Linux (SELinux) şifrelemeyi etkinleştirmeden önce devre dışı bırakılmamıştır, bu nedenle montajsız bırakma adımı başarısız olur. Şifreleme tamamlandıktan sonra SELinux yeniden etkinleştirilebilir.
- İşletim sistemi diski mantıksal birim yöneticisi (LVM) şeması kullanır. Sınırlı LVM veri diskdesteği olsa da, bir LVM işletim sistemi diski yok.
- Minimum bellek gereksinimleri karşılanmaz (işletim sistemi disk şifrelemesi için 7 GB önerilir).
- Veri sürücüleri /mnt/ dizinin altına veya birbirlerine (örneğin, /mnt/data1, /mnt/data2, /data3 + /data3/data4) monte edilir.

## <a name="update-the-default-kernel-for-ubuntu-1404-lts"></a>Ubuntu 14.04 LTS için varsayılan çekirdeği güncelleştirme

Ubuntu 14.04 LTS görüntü gemileri ile varsayılan çekirdek sürümü 4.4. Bu çekirdek sürümü, Out of Memory Killer'ın işletim sistemi şifreleme işlemi sırasında dd komutunu yanlış bir şekilde sonlandırdıği bilinen bir soruna sahiptir. Bu hata en son Azure ayarlı Linux çekirdeğinde düzeltilmiştir. Bu hatayı önlemek için, görüntüde şifrelemeyi etkinleştirmeden önce, [Azure ayarlı çekirdek 4.15'e](https://packages.ubuntu.com/trusty/linux-azure) güncelleştirin veya aşağıdaki komutları kullanarak:

```
sudo apt-get update
sudo apt-get install linux-azure
sudo reboot
```

VM yeni çekirdek içine yeniden başladıktan sonra, yeni çekirdek sürümü kullanılarak teyit edilebilir:

```
uname -a
```

## <a name="update-the-azure-virtual-machine-agent-and-extension-versions"></a>Azure Sanal Makine Aracısı ve uzantı sürümlerini güncelleştirme

Azure Disk Şifreleme işlemleri, Azure Sanal Makine Aracısı'nın desteklenmeyen sürümlerini kullanarak sanal makine görüntülerinde başarısız olabilir. 2.2.38'den önce aracı sürümlerine sahip Linux görüntüleri şifrelemeyi etkinleştirmeden önce güncelleştirilmelidir. Daha fazla bilgi için Azure Linux Aracısı'nı [Azure'daki sanal makine aracıları için](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)VM ve Minimum sürüm [desteğinde nasıl güncelleştireceğime](../extensions/update-linux-agent.md) bakın.

Microsoft.Azure.Security.AzureDiskEncryption veya Microsoft.Azure.Security.AzureDiskForLinux konuk aracı uzantısı nın doğru sürümü de gereklidir. Azure Sanal Makine aracısı ön koşulları karşılandığında ve sanal makine aracısının desteklenen bir sürümü kullanıldığında, uzantı sürümleri platform tarafından otomatik olarak korunur ve güncelleştirilir.

Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux uzantısı amortismana tabidir ve artık desteklenmez.  

## <a name="unable-to-encrypt-linux-disks"></a>Linux diskleri şifreleneme

Bazı durumlarda, Linux disk şifrelemesi "işletim sistemi disk şifrelemesi başlatıldı" ve SSH devre dışı bırakılmış gibi görünür. Şifreleme işleminin stok galerisi görüntüsünde tamamlanması 3-16 saat arasında sürebilir. Çok terabayt boyutunda veri diskleri eklenirse, işlem günler sürebilir.

Linux OS disk şifreleme dizisi işletim sistemi sürücüsünün montajını geçici olarak kaldırır. Daha sonra, şifreli durumuna yeniden monte etmeden önce tüm işletim sistemi diskinin blok blok şifrelemesini gerçekleştirir. Linux Disk Şifreleme, şifreleme devam ederken VM'nin eşzamanlı kullanımına izin vermez. VM'nin performans özellikleri, şifrelemeyi tamamlamak için gereken süre içinde önemli bir fark yaratabilir. Bu özellikler, diskin boyutunu ve depolama hesabının standart mı yoksa premium (SSD) depolama alanı mı olduğunu içerir.

Şifreleme durumunu denetlemek için, [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) komutundan döndürülen **ProgressMessage** alanını yokleyin. İşletim sistemi sürücüsü şifrelenirken, VM bir servis durumuna girer ve devam eden süreçte herhangi bir aksama oluşmasını önlemek için SSH'yi devre dışı kılmaktadır. Şifreleme devam ederken çoğu zaman **şifreleme inProgress** iletisi raporlar. Birkaç saat sonra, **VMStartPending** iletisi VM'yi yeniden başlatmanızı ister. Örnek:


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

VM'yi yeniden başlatmanız istendikten sonra ve VM yeniden başlatıldıktan sonra, yeniden başlatma ve hedefüzerinde son adımların gerçekleştirilmesi için 2-3 dakika beklemeniz gerekir. Şifreleme sonunda tamamlandığında durum iletisi değişir. Bu ileti kullanılabilir hale geldikten sonra, şifreli işletim sistemi sürücüsünün kullanıma hazır olması ve VM'nin yeniden kullanıma hazır olması beklenir.

Aşağıdaki durumlarda, VM'yi şifrelemeden hemen önce alınan anlık görüntüye veya yedeklemeye geri yüklemenizi öneririz:
   - Daha önce açıklanan yeniden başlatma sırası gerçekleşmezse.
   - Önyükleme bilgileri, ilerleme iletisi veya diğer hata göstergeleri işletim sistemi şifrelemesinin bu işlemin ortasında başarısız olduğunu bildiriyorsa. İletinin bir örneği, bu kılavuzda açıklanan "çözülemeyen" hatasıdır.

Bir sonraki denemeden önce, VM'nin özelliklerini yeniden değerlendirin ve tüm ön koşulların karşılandıklarından emin olun.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Güvenlik duvarının arkasındaki Azure Disk Şifrelemesorununa Sorun Giderme

[Yalıtılmış bir ağda Disk Şifreleme'ye](disk-encryption-isolated-network.md) bakın

## <a name="troubleshooting-encryption-status"></a>Sorun giderme şifreleme durumu 

Portal, VM içinde şifresiz hale gelmiştir sonra bile şifreli olarak bir disk görüntüleyebilir.  Bu, daha yüksek düzeydeki Azure Disk Şifreleme yönetimi komutlarını kullanmak yerine diski VM'nin içinden doğrudan şifrelemek için düşük düzeyli komutlar kullanıldığında oluşabilir.  Üst düzey komutlar yalnızca Diskin VM içinden şifresini şifrelemeyi değil, VM dışında da VM ile ilişkili önemli platform düzeyi şifreleme ayarlarını ve uzantı ayarlarını günceller.  Bunlar hizada tutulmazsa, platform şifreleme durumunu bildiremez veya VM'yi düzgün bir şekilde sağamaz.   

PowerShell ile Azure Disk Şifrelemesini devre dışı kaldırmak için Devre [Dışı-AzVMDiskŞifreleme'yi](/powershell/module/az.compute/disable-azvmdiskencryption) ve ardından [Remove-AzVMDiskEncryptionExtension'ı](/powershell/module/az.compute/remove-azvmdiskencryptionextension)kullanın. Şifreleme devre dışı bırakılmadan önce Remove-AzVMDiskEncryptionExtension'ı çalıştırmak başarısız olur.

CLI ile Azure Disk Şifrelemesini devre dışı kullanabilirsiniz, [az vm şifrelemeyi devre dışı kullanabilirsiniz.](/cli/azure/vm/encryption) 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk Şifrelemesindeki bazı yaygın sorunlar ve bu sorunları nasıl giderdiğiniz hakkında daha fazla bilgi edinebilirsiniz. Bu hizmet ve özellikleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde disk şifreleme uygulayın](../../security-center/security-center-apply-disk-encryption.md)
- [Azure veri şifrelemesi istirahatte](../../security/fundamentals/encryption-atrest.md)
