---
title: Windows VM 'Leri için Azure disk şifrelemesini etkinleştirme
description: Bu makale, Windows VM 'Leri için Microsoft Azure disk şifrelemeyi etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 10/05/2019
ms.custom: seodec18
ms.openlocfilehash: 05db717f5d3adc2429431503f588f2cc7f79aef6
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72435741"
---
# <a name="azure-disk-encryption-for-windows-vms"></a>Windows VM 'Leri için Azure disk şifrelemesi 

Azure disk şifrelemesi, kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Azure sanal makinelerinin (VM 'Ler) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un [BitLocker](https://en.wikipedia.org/wiki/BitLocker) özelliğini kullanır ve disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](../../key-vault/index.yml) ile tümleşiktir. 

[Azure Güvenlik Merkezi](../../security-center/index.yml)'ni kullanırsanız, şifrelenmeyen VM 'ler varsa uyarılırsınız. Uyarılar yüksek önem derecesine sahip olarak gösterir ve bu VM'lerin şifrelemek için önerilir.

![Azure Güvenlik Merkezi disk Şifreleme Uyarısı](../media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - Bir VM 'yi şifrelemek için Azure AD ile Azure disk şifrelemesi 'ni daha önce kullandıysanız, VM 'nizi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Ayrıntılar için bkz. [Azure AD ile Azure disk şifrelemesi (önceki sürüm)](disk-encryption-overview-aad.md) . 
> - Bazı öneriler, veri, ağ veya ek lisans ya da abonelik maliyetlerinizi kaynaklanan işlem kaynak kullanımını artırabilir. Desteklenen bölgelerdeki Azure kaynakları oluşturmak için geçerli bir etkin Azure aboneliğinizin olması gerekir.

Windows için Azure disk şifrelemesi temellerini yalnızca birkaç dakika içinde [Azure CLI hızlı başlangıç Ile WINDOWS VM oluşturma ve şifreleme](disk-encryption-cli-quickstart.md) Ile Windows [VM oluşturma](disk-encryption-powershell-quickstart.md)ve şifreleme hızlı başlangıcı ile öğrenin.

## <a name="supported-vms-and-operating-systems"></a>Desteklenen VM 'Ler ve işletim sistemleri

### <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

Windows VM 'Leri [çeşitli boyutlarda](sizes-general.md)kullanılabilir. Azure disk şifrelemesi, [temel, A serisi VM](https://azure.microsoft.com/pricing/details/virtual-machines/series/)'lerde veya 2 GB 'den az belleğe sahip sanal makinelerde kullanılamaz.

Azure disk şifrelemesi, Premium depolama özellikli VM 'Ler için de kullanılabilir.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Windows istemcisi: Windows 8 ve üzeri.
- Windows Server: Windows Server 2008 R2 ve üzeri.  
 
> [!NOTE]
> Windows Server 2008 R2, şifreleme için .NET Framework 4,5 ' nin yüklenmesini gerektirir; Windows Server 2008 R2 x64 tabanlı sistemler için isteğe bağlı güncelleştirme Microsoft .NET Framework 4.5.2 ([KB2901983](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2901983)) ile Windows Update yüklersiniz.  
>  
> Windows Server 2012 R2 Core ve Windows Server 2016 Core, şifreleme için sanal makinede BdeHdCfg bileşeninin yüklü olmasını gerektirir.


## <a name="networking-requirements"></a>Ağ gereksinimleri
Azure disk şifrelemeyi etkinleştirmek için VM 'Lerin aşağıdaki ağ uç noktası yapılandırma gereksinimlerini karşılaması gerekir:
  - Anahtar kasanıza bağlanma belirteci almak için Windows VM 'nin bir Azure Active Directory uç noktasına bağlanabilmesi gerekir \[login.microsoftonline.com\].
  - Şifreleme anahtarlarını anahtar kasanıza yazmak için Windows VM 'nin Anahtar Kasası uç noktasına bağlanabiliyor olması gerekir.
  - Windows VM, Azure uzantı deposunu barındıran bir Azure depolama uç noktasına ve VHD dosyalarını barındıran bir Azure depolama hesabına bağlanabilmelidir.
  -  Güvenlik ilkeniz Azure vm'lerinden Internet erişimi sınırlayan, önceki URI çözmek ve IP'ler giden bağlantıya izin verecek bir kuralı yapılandırın. Daha fazla bilgi için [bir güvenlik duvarının ardındayken Azure anahtar kasası](../../key-vault/key-vault-access-behind-firewall.md).    


## <a name="group-policy-requirements"></a>grup ilkesi gereksinimleri

Azure disk şifrelemesi, Windows VM 'Leri için BitLocker dış anahtar koruyucusunu kullanır. Etki alanına katılmış sanal makineleri, TPM koruyucusu zorlamak için tüm grup ilkeleri anında iletme yok. "Uyumlu TPM'siz BitLocker izin ver" için Grup İlkesi hakkında bilgi için bkz: [BitLocker Grup İlkesi başvurusu](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1).

Özel grup ilkesiyle etki alanına katılmış sanal makinelerde BitLocker ilkesi şu ayarı içermelidir: [BitLocker kurtarma bilgileri 'nin Kullanıcı depolamasını yapılandırma-> Izin ver 256-bit kurtarma anahtarı](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). BitLocker için özel Grup İlkesi ayarları uyumsuz olduğunda Azure disk şifrelemesi başarısız olur. Doğru ilkeyi gerekmedi makinelerde yeni ilkeyi uygulamak, (gpupdate.exe/Force) güncelleştirmek için yeni ilke zorlayın ve daha sonra yeniden başlatmak gerekli olabilir.

Etki alanı düzeyi Grup ilkesi, BitLocker tarafından kullanılan AES-CBC algoritmasını engelliyorsa Azure disk şifrelemesi başarısız olur.

## <a name="encryption-key-storage-requirements"></a>Şifreleme anahtarı depolama gereksinimleri  

Azure disk şifrelemesi, disk şifreleme anahtarlarını ve gizli dizileri denetlemek ve yönetmek için bir Azure Key Vault gerektirir. Anahtar kasanızın ve sanal makinelerinizin aynı Azure bölgesinde ve abonelikte bulunması gerekir.

Ayrıntılar için bkz. [Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md).

## <a name="terminology"></a>Terminoloji
Aşağıdaki tabloda, Azure disk şifrelemesi belgelerinde kullanılan bazı yaygın terimler tanımlanmaktadır:

| Terminoloji | Tanım |
| --- | --- |
| Azure Key Vault | Key Vault, Federal Bilgi işleme standartları (FIPS) üzerinde doğrulanmış donanım güvenlik modülleri dayanıyor bir şifreleme ve anahtar yönetim hizmetidir. Bu standartlar, şifreleme anahtarlarını ve gizli gizli anahtarlarınızı yardımcı olur. Daha fazla bilgi için bkz. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) belgeleri ve [Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md). |
| Azure CLI | [Azure CLI'yı](/cli/azure/install-azure-cli) Azure kaynaklarını komut satırından yönetmek ve için optimize edilmiştir.|
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) , Windows VM 'lerinde disk şifrelemeyi etkinleştirmek için kullanılan sektör tarafından tanınan bir Windows birimi şifreleme teknolojisidir. |
| Anahtar şifreleme anahtarı (KEK) | Gizli anahtarı korumak veya kaydırmak için kullanabileceğiniz asimetrik anahtar (RSA 2048). Bir donanım güvenlik modülü (HSM) sağladığınız-korumalı bir anahtar veya yazılım korumalı anahtarı. Daha fazla bilgi için bkz. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) belgeleri ve [Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md). |
| PowerShell cmdlet'leri | Daha fazla bilgi için [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |


## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç-Azure CLı ile Windows VM oluşturma ve şifreleme](disk-encryption-cli-quickstart.md)
- [Hızlı başlangıç-Azure PowerShell ile Windows VM oluşturma ve şifreleme](disk-encryption-powershell-quickstart.md)
- [Windows VM'lerinde Azure Disk Şifrelemesi senaryoları](disk-encryption-windows.md)
- [Azure disk şifrelemesi önkoşulları CLı betiği](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure disk şifrelemesi önkoşulları PowerShell betiği](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md)


