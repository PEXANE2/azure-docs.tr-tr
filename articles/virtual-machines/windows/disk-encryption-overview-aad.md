---
title: Azure AD ile Azure Disk Şifreleme (önceki sürüm)
description: Bu makalede, IaaS VM'ler için Microsoft Azure Disk Şifrelemesi kullanmak için ön koşullar sağlanmaktadır.
author: msmbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 025d02ccdf38e72682cf67cc07a8b2edd549e599
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82081583"
---
# <a name="azure-disk-encryption-with-azure-ad-previous-release"></a>Azure AD ile Azure Disk Şifreleme (önceki sürüm)

**Azure Disk Şifreleme'nin yeni sürümü, VM disk şifrelemesini etkinleştirmek için Bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle, şifrelemeyi etkinleştir medeni sırasında Azure AD kimlik bilgilerini sağlamanız artık gerekmez. Tüm yeni VM'ler, yeni sürümü kullanarak Azure AD uygulama parametreleri olmadan şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirmek için yönergeleri görüntülemek [için Windows VM'ler için Azure Disk Şifreleme'ye](disk-encryption-overview.md)bakın. Azure AD uygulama parametreleri ile zaten şifrelenmiş olan VM'ler hala desteklenir ve AAD sözdizimi ile korunmaya devam etmelidir.**

Bu makalede, [Windows VM'ler için Azure Disk](disk-encryption-overview.md) Şifrelemesi, Azure AD (önceki sürüm) ile Azure Disk Şifrelemesi için ek gereksinimler ve ön koşullar ekler. [Desteklenen VM'ler ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems) bölümü aynı kalır.

## <a name="networking-and-group-policy"></a>Ağ ve Grup Politikası

**Eski AAD parametre sözdizimini kullanarak Azure Disk Şifreleme özelliğini etkinleştirmek için IaaS VM'lerin aşağıdaki ağ bitiş noktası yapılandırma gereksinimlerini karşılaması gerekir:** 
  - IaaS VM'nin anahtar kasanıza bağlanmak için bir belirteç elde etmek için \[azure\]Active Directory bitiş noktasına login.microsoftonline.com.
  - Anahtar kasanızın şifreleme anahtarlarını yazmak için, IaaS VM'nin anahtar kasa bitiş noktasına bağlanabilmesi gerekir.
  - IaaS VM, Azure uzantı deposunu ve VHD dosyalarını barındıran bir Azure depolama hesabı barındıran bir Azure depolama bitiş noktasına bağlanabilmeli.
  -  Güvenlik ilkeniz Azure VM'lerinden Internet'e erişimi kısıtlıyorsa, önceki URI'yi çözebilir ve IP'lere giden bağlantıya izin verecek şekilde belirli bir kural yapılandırabilirsiniz. Daha fazla bilgi için [bir güvenlik duvarının arkasındaki Azure Anahtar Kasası'na](../../key-vault/key-vault-access-behind-firewall.md)bakın.
  - Şifrelenecek VM varsayılan protokol olarak TLS 1.2 kullanacak şekilde yapılandırılmalıdır. TLS 1.0 açıkça devre dışı bırakılmışsa ve .NET sürümü 4,6 veya daha yüksek olarak güncelleştirilmediyse, aşağıdaki kayıt defteri değişikliği ADE'nin daha yeni TLS sürümünü seçmesini sağlar:

```console
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001` 
```

**Grup İlkesi:**
 - Azure Disk Şifreleme çözümü, Windows IaaS VM'ler için BitLocker harici anahtar koruyucusu kullanır. Birleştirilmiş etki alanı için, TPM koruyucularını zorlayan grup ilkelerini itmeyin. "Uyumlu bir TPM olmadan BitLocker'a izin ver" grup ilkesi hakkında bilgi için [Bkz. BitLocker Grup İlkesi Başvurusu.](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1)

-  Özel grup ilkesi ile sanal makinelere katılan etki alanı üzerindeki BitLocker ilkesi aşağıdaki ayarı içermelidir: [BitLocker kurtarma bilgilerinin kullanıcı depolamasını yapılandırın -> 256 bit kurtarma anahtarına izin verin.](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings) Azure Disk Şifreleme, BitLocker için özel grup ilkesi ayarları uyumsuz olduğunda başarısız olur. Doğru ilke ayarı olmayan makinelerde, yeni ilkeyi uygulayın, yeni ilkeyi güncelleştirmeye zorlayın (gpupdate.exe /force) ve sonra yeniden başlatma gerekebilir.  

## <a name="encryption-key-storage-requirements"></a>Şifreleme anahtarı depolama gereksinimleri  

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını denetlemek ve yönetmek için bir Azure Anahtar Kasası gerektirir. Anahtar kasanız ve VM'leriniz aynı Azure bölgesinde ve abonelikte ikamet etmelidir.

Ayrıntılar için, [Azure AD (önceki sürüm) ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault-aad.md)ya da yapılandırma ya da bkz.
 
## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md)
- [Windows VM'lerde Azure AD ile Azure Disk Şifrelemesini etkinleştirme (önceki sürüm)](disk-encryption-windows-aad.md)
- [Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure Disk Şifreleme PowerShell komut dosyası önkoşullar](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
