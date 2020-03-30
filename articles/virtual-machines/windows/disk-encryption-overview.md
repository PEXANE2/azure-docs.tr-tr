---
title: Windows VM'ler için Azure Disk Şifrelemesini Etkinleştirme
description: Bu makalede, Windows VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme ile ilgili yönergeler sağlanmaktadır.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 10/05/2019
ms.custom: seodec18
ms.openlocfilehash: 05db717f5d3adc2429431503f588f2cc7f79aef6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266786"
---
# <a name="azure-disk-encryption-for-windows-vms"></a>Windows VM'ler için Azure Disk Şifrelemesi 

Azure Disk Şifrelemesi verilerinizi koruyarak kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur. Azure sanal makinelerinin (VM) işletim sistemi ve veri diskleri için birim şifrelemesağlamak için Windows'un [Bitlocker](https://en.wikipedia.org/wiki/BitLocker) özelliğini kullanır ve disk şifreleme anahtarlarını ve sırlarını kontrol ve yönetmenize yardımcı olmak için [Azure Key Vault](../../key-vault/index.yml) ile tümleştirilir. 

[Azure Güvenlik Merkezi](../../security-center/index.yml)kullanıyorsanız, şifrelenmemiş VM'leriniz varsa uyarılanırsınız. Uyarılar Yüksek Önem derecesi olarak gösterir ve öneri bu VM'leri şifrelemektir.

![Azure Güvenlik Merkezi disk şifreleme uyarısı](../media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - VM'yi şifrelemek için Azure AD ile Azure Disk Şifreleme'yi daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz. Ayrıntılar için [Azure AD (önceki sürüm) ile Azure Disk](disk-encryption-overview-aad.md) Şifrelemesi'ne bakın. 
> - Bazı öneriler veri, ağ veya bilgi işlem kaynağı kullanımını artırarak ek lisans veya abonelik maliyetlerine neden olabilir. Desteklenen bölgelerde Azure'da kaynak oluşturmak için geçerli bir etkin Azure aboneliğiniz olmalıdır.

Windows için Azure Disk Şifrelemesinin temellerini, Azure CLI hızlı başlatma veya Azure Powershell hızlı başlatma ile Windows VM Oluştur ile birkaç dakika içinde öğrenebilir ve Windows [VM'yi şifreleyebilir ve](disk-encryption-cli-quickstart.md) [şifreleyebilirsiniz.](disk-encryption-powershell-quickstart.md)

## <a name="supported-vms-and-operating-systems"></a>Desteklenen VM'ler ve işletim sistemleri

### <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

Windows VM'ler [çeşitli boyutlarda](sizes-general.md)mevcuttur. Azure Disk Şifreleme, [Temel, A serisi SANAL Makinelerde](https://azure.microsoft.com/pricing/details/virtual-machines/series/)veya 2 GB'dan az belleğe sahip sanal makinelerde kullanılamaz.

Azure Disk Şifrelemesi, birinci sınıf depolama alanına sahip VM'ler için de kullanılabilir.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Windows istemcisi: Windows 8 ve sonrası.
- Windows Server: Windows Server 2008 R2 ve sonrası.  
 
> [!NOTE]
> Windows Server 2008 R2 şifreleme için .NET Framework 4.5'in yüklenmesini gerektirir; Windows Server 2008 R2 x64 tabanlı sistemler için isteğe bağlı güncelleştirme Microsoft .NET Framework 4.5.2 ile Windows Update'ten yükleyin[(KB2901983).](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2901983)  
>  
> Windows Server 2012 R2 Core ve Windows Server 2016 Core, şifreleme için VM'de bdehdcfg bileşeninin yüklenmesini gerektirir.


## <a name="networking-requirements"></a>Ağ gereksinimleri
Azure Disk Şifrelemesi'ni etkinleştirmek için, VM'lerin aşağıdaki ağ bitiş noktası yapılandırma gereksinimlerini karşılaması gerekir:
  - Anahtar kasanıza bağlanmak için bir belirteç elde etmek için, Windows VM'nin \[bir\]Azure Etkin Dizin bitiş noktasına login.microsoftonline.com bağlanabilmesi gerekir.
  - Anahtar kasanızın şifreleme anahtarlarını yazmak için Windows VM'nin anahtar kasa bitiş noktasına bağlanabilmesi gerekir.
  - Windows VM, Azure uzantı deposunu ve VHD dosyalarını barındıran bir Azure depolama hesabı barındıran bir Azure depolama bitiş noktasına bağlanabilmeli.
  -  Güvenlik ilkeniz Azure VM'lerinden Internet'e erişimi kısıtlıyorsa, önceki URI'yi çözebilir ve IP'lere giden bağlantıya izin verecek şekilde belirli bir kural yapılandırabilirsiniz. Daha fazla bilgi için [bir güvenlik duvarının arkasındaki Azure Anahtar Kasası'na](../../key-vault/key-vault-access-behind-firewall.md)bakın.    


## <a name="group-policy-requirements"></a>Grup İlkesi gereksinimleri

Azure Disk Şifreleme, Windows VM'leri için BitLocker harici anahtar koruyucusu kullanır. Birleştirilmiş etki alanı için, TPM koruyucularını zorlayan grup ilkelerini itmeyin. "Uyumlu bir TPM olmadan BitLocker'a izin ver" grup ilkesi hakkında bilgi için [Bkz. BitLocker Grup İlkesi Başvurusu.](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1)

Özel grup ilkesi ile sanal makinelere katılan etki alanı üzerindeki BitLocker ilkesi aşağıdaki ayarı içermelidir: [BitLocker kurtarma bilgilerinin kullanıcı depolamasını yapılandırın -> 256 bit kurtarma anahtarına izin verin.](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings) Azure Disk Şifreleme, BitLocker için özel grup ilkesi ayarları uyumsuz olduğunda başarısız olur. Doğru ilke ayarı olmayan makinelerde, yeni ilkeyi uygulayın, yeni ilkeyi güncelleştirmeye zorlayın (gpupdate.exe /force) ve sonra yeniden başlatma gerekebilir.

Etki alanı düzeyinde grup ilkesi BitLocker tarafından kullanılan AES-CBC algoritmasını engellerse Azure Disk Şifrelemesi başarısız olur.

## <a name="encryption-key-storage-requirements"></a>Şifreleme anahtarı depolama gereksinimleri  

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını denetlemek ve yönetmek için bir Azure Anahtar Kasası gerektirir. Anahtar kasanız ve VM'leriniz aynı Azure bölgesinde ve abonelikte ikamet etmelidir.

Ayrıntılar için Azure [Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma ya da

## <a name="terminology"></a>Terminoloji
Aşağıdaki tablo, Azure disk şifreleme belgelerinde kullanılan ortak terimlerden bazılarını tanımlar:

| Terminoloji | Tanım |
| --- | --- |
| Azure Key Vault | Key Vault, Federal Bilgi İşlem Standartları (FIPS) onaylı donanım güvenlik modüllerini temel alan bir şifreleme, anahtar yönetim hizmetidir. Bu standartlar, şifreleme anahtarlarınızın ve hassas sırlarınızın korunmasına yardımcı olur. Daha fazla bilgi için [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) belgelerine ve [Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma'ya bakın. |
| Azure CLI | [Azure CLI,](/cli/azure/install-azure-cli) komut satırından Azure kaynaklarını yönetmek ve yönetmek için optimize edilebilmektedir.|
| BitLocker |[BitLocker,](https://technet.microsoft.com/library/hh831713.aspx) Windows VM'lerde disk şifrelemeyi etkinleştirmek için kullanılan, endüstri tarafından tanınan bir Windows birim şifreleme teknolojisidir. |
| Anahtar şifreleme anahtarı (KEK) | Sırrı korumak veya sarmak için kullanabileceğiniz asimetrik anahtar (RSA 2048). Donanım güvenlik modülü (HSM) korumalı anahtar veya yazılım korumalı anahtar sağlayabilirsiniz. Daha fazla bilgi için [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) belgelerine ve [Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma'ya bakın. |
| PowerShell cmdlet'leri | Daha fazla bilgi için Azure [PowerShell cmdlets'e](/powershell/azure/overview)bakın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart - Azure CLI ile Windows VM oluşturma ve şifreleme](disk-encryption-cli-quickstart.md)
- [Quickstart - Azure Powershell ile Windows VM oluşturma ve şifreleme](disk-encryption-powershell-quickstart.md)
- [Windows VM'lerinde Azure Disk Şifrelemesi senaryoları](disk-encryption-windows.md)
- [Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure Disk Şifreleme PowerShell komut dosyası önkoşullar](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md)


