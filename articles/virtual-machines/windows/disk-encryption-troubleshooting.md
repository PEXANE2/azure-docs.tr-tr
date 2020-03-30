---
title: Azure Disk Şifreleme sorun giderme kılavuzu
description: Bu makalede, Windows VM'ler için Microsoft Azure Disk Şifrelemesi için sorun giderme ipuçları verilmektedir.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 0d4e76f4d02b0287770243bfddf995a19f90d232
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73749455"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk Şifreleme sorun giderme kılavuzu

Bu kılavuz, kuruluşları Azure Disk Şifrelemesi kullanan BT uzmanları, bilgi güvenliği analistleri ve bulut yöneticileri içindir. Bu makale, disk şifrelemeyle ilgili sorunları giderme de yardımcı olmak içindir.

Aşağıdaki adımlardan herhangi birini almadan önce, şifrelemeye çalıştığınız [VM'lerin desteklenen VM boyutları ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems)arasında olduğundan ve tüm ön koşulları yerine getirebildiğinizden emin olun:

- [Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)
- [Grup ilkesi gereksinimleri](disk-encryption-overview.md#group-policy-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](disk-encryption-overview.md#encryption-key-storage-requirements)

 

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Güvenlik duvarının arkasındaki Azure Disk Şifrelemesorununa Sorun Giderme

Bağlantı bir güvenlik duvarı, proxy gereksinimi veya ağ güvenlik grubu (NSG) ayarlarıyla kısıtlandığında, uzantının gerekli görevleri gerçekleştirme yeteneği kesintiye uğrayabilir. Bu bozulma, "VM'de uzantı durumu yok" gibi durum iletilerine neden olabilir. Beklenen senaryolarda şifreleme tamamlanamazsa. İzleyen bölümlerde, araştırabileceğiniz bazı yaygın güvenlik duvarı sorunları vardır.

### <a name="network-security-groups"></a>Ağ güvenlik grupları
Uygulanan tüm ağ güvenliği grubu ayarları, bitiş noktasının disk şifrelemesi için belgelenmiş ağ yapılandırma [ön koşullarıyla](disk-encryption-overview.md#networking-requirements) karşılayabilmesine izin vermelidir.

### <a name="azure-key-vault-behind-a-firewall"></a>Güvenlik duvarının arkasındaki Azure Key Vault

Azure AD kimlik [bilgileriyle](disk-encryption-windows-aad.md#)şifreleme etkinleştirilirken, hedef VM hem Azure Etkin Dizin uç noktalarına hem de Key Vault uç noktalarına bağlantı sağlar. Geçerli Azure Etkin Dizin kimlik doğrulama bitiş [noktaları, Office 365 URL'lerinin](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges) 56 ve 59. Anahtar Vault yönergeleri, [güvenlik duvarının arkasındaki Azure Anahtar Kasası'na](../../key-vault/key-vault-access-behind-firewall.md)nasıl erişilene ilişkin belgelerde verilmiştir.

### <a name="azure-instance-metadata-service"></a>Azure Örneği Meta veri Hizmeti 
VM, yalnızca VM içinden erişilebilen, iyi bilinen, routable olmayan BIR`169.254.169.254`IP adresi () kullanan [Azure Örneği Meta veri hizmet](../windows/instance-metadata-service.md) bitiş noktasına erişebilmeli.  Bu adrese yerel HTTP trafiğini değiştiren proxy yapılandırmaları (örneğin, X-Forwarded-For üstbilgisi eklemek) desteklenmez.

## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Server Core Sorun Giderme

Windows Server 2016 Server Core'da bdehdcfg bileşeni varsayılan olarak kullanılamaz. Bu bileşen, Azure Disk Şifrelemesi tarafından gereklidir. VM'nin ömrü boyunca yalnızca bir kez yapılan işletim sistemi hacminden sistem hacmini bölmek için kullanılır. Bu ikililer daha sonraki şifreleme işlemleri sırasında gerekli değildir.

Bu sorunu çözmek için, aşağıdaki dört dosyayı Bir Windows Server 2016 Veri Merkezi VM'sinden Server Core'da aynı konuma kopyalayın:

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

1. Bu komut 550 MB'lık bir sistem bölümü oluşturur. Sistemi yeniden başlatın.

1. Birimleri denetlemek için DiskPart'i kullanın ve sonra devam edin.  

Örnek:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```

## <a name="troubleshooting-encryption-status"></a>Sorun giderme şifreleme durumu 

Portal, VM içinde şifresiz hale gelmiştir sonra bile şifreli olarak bir disk görüntüleyebilir.  Bu, daha yüksek düzeydeki Azure Disk Şifreleme yönetimi komutlarını kullanmak yerine diski VM'nin içinden doğrudan şifrelemek için düşük düzeyli komutlar kullanıldığında oluşabilir.  Üst düzey komutlar yalnızca Diskin VM içinden şifresini şifrelemeyi değil, VM dışında da VM ile ilişkili önemli platform düzeyi şifreleme ayarlarını ve uzantı ayarlarını günceller.  Bunlar hizada tutulmazsa, platform şifreleme durumunu bildiremez veya VM'yi düzgün bir şekilde sağamaz.   

PowerShell ile Azure Disk Şifrelemesini devre dışı kaldırmak için Devre [Dışı-AzVMDiskŞifreleme'yi](/powershell/module/az.compute/disable-azvmdiskencryption) ve ardından [Remove-AzVMDiskEncryptionExtension'ı](/powershell/module/az.compute/remove-azvmdiskencryptionextension)kullanın. Şifreleme devre dışı bırakılmadan önce Remove-AzVMDiskEncryptionExtension'ı çalıştırmak başarısız olur.

CLI ile Azure Disk Şifrelemesini devre dışı kullanabilirsiniz, [az vm şifrelemeyi devre dışı kullanabilirsiniz.](/cli/azure/vm/encryption) 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Disk Şifrelemesindeki bazı yaygın sorunlar ve bu sorunları nasıl giderdiğiniz hakkında daha fazla bilgi edinebilirsiniz. Bu hizmet ve özellikleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde disk şifreleme uygulayın](../../security-center/security-center-apply-disk-encryption.md)
- [Azure veri şifrelemesi istirahatte](../../security/fundamentals/encryption-atrest.md)
