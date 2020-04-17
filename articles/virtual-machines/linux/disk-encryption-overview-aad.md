---
title: Azure AD ön koşullarıyla Azure Disk Şifrelemesi (önceki sürüm)
description: Bu makalede, IaaS VM'ler için Microsoft Azure Disk Şifrelemesi kullanmak için ön koşullar sağlanmaktadır.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 029743dbb44ab0ebb6ab8f695c0c966137c45107
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459806"
---
# <a name="azure-disk-encryption-with-azure-ad-previous-release"></a>Azure AD ile Azure Disk Şifreleme (önceki sürüm)

Azure Disk Şifreleme'nin yeni sürümü, VM disk şifrelemesini etkinleştirmek için Bir Azure Active Directory (Azure AD) uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, şifrelemeyi etkinleştir medeni sırasında Azure AD kimlik bilgilerini sağlamanız gerekmez. Tüm yeni VM'ler, yeni sürüm kullanılarak Azure AD uygulama parametreleri olmadan şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirme yle ilgili talimatlar [için Linux VM'ler için Azure Disk](disk-encryption-overview.md)Şifrelemesi'ne bakın. Azure AD uygulama parametreleri ile zaten şifrelenmiş olan VM'ler hala desteklenir ve AAD sözdizimi ile korunmaya devam etmelidir.

Bu makalede, Azure AD ile Azure Disk Şifrelemesi için ek gereksinimler ve Azure Disk Şifrelemesi için ön koşullar (önceki sürüm) içeren [Linux VM'leri için Azure Disk Şifrelemesi](disk-encryption-overview.md) ekleri sağlanmaktadır.

Bu bölümlerdeki bilgiler aynı kalır:

- [Desteklenen VM'ler ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ek VM gereksinimleri](disk-encryption-overview.md#additional-vm-requirements)


 

## <a name="networking-and-group-policy"></a>Ağ ve Grup Politikası

Eski AAD parametre sözdizimini kullanarak Azure Disk Şifreleme özelliğini etkinleştirmek için hizmet olarak altyapı (IaaS) VM'ler aşağıdaki ağ bitiş noktası yapılandırma gereksinimlerini karşılamalıdır: 
  - IaaS VM'nin anahtar kasanıza bağlanmak için bir belirteç elde etmek için \[Azure\]AD bitiş noktasına login.microsoftonline.com.
  - Anahtar kasanızın şifreleme anahtarlarını yazmak için, IaaS VM'nin anahtar kasa bitiş noktasına bağlanabilmesi gerekir.
  - IaaS VM, Azure uzantı deposunu ve VHD dosyalarını barındıran bir Azure depolama hesabı barındıran bir Azure depolama bitiş noktasına bağlanabilmeli.
  -  Güvenlik politikanız Azure VM'lerinden Internet'e erişimi kısıtlıyorsa, önceki URI'yi çözebilir ve IP'lere giden bağlantıya izin verecek şekilde belirli bir kural yapılandırabilirsiniz. Daha fazla bilgi için [bir güvenlik duvarının arkasındaki Azure Anahtar Kasası'na](../../key-vault/general/access-behind-firewall.md)bakın.
  - Windows'da, TLS 1.0 açıkça devre dışı bırakılırsa ve .NET sürümü 4,6 veya daha yüksek olarak güncelleştirilmezse, aşağıdaki kayıt defteri değişikliği Azure Disk Şifrelemesinin daha yeni TLS sürümünü seçmesini sağlar:
    
            [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
            "SystemDefaultTlsVersions"=dword:00000001
            "SchUseStrongCrypto"=dword:00000001
    
            [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
            "SystemDefaultTlsVersions"=dword:00000001
            "SchUseStrongCrypto"=dword:00000001` 
         
    
### <a name="group-policy"></a>Grup İlkesi
 - Azure Disk Şifreleme çözümü, Windows IaaS VM'ler için BitLocker harici anahtar koruyucusu kullanır. Etki alanına katılan VM'ler için, TPM koruyucularını zorlayan Grup İlkeleri'ni itmeyin. **Uyumlu bir TPM olmadan BitLocker'a İzin Ver**seçeneği için Grup İlkesi hakkında bilgi için [Bkz. BitLocker Grup İlkesi başvurusu.](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1)

- Özel bir Grup İlkesi ile etki alanı birleştirilmiş sanal makinelerde BitLocker ilkesi aşağıdaki ayarı içermelidir: [BitLocker kurtarma bilgilerinin kullanıcı depolama yapılandırma -> 256 bit kurtarma anahtarı na izin ver.](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings) Azure Disk Şifreleme, BitLocker için özel Grup İlkesi ayarları uyumsuz olduğunda başarısız olur. Doğru ilke ayarı olmayan makinelerde, yeni ilkeyi uygulayın, yeni ilkeyi güncelleştirmeye (gpupdate.exe /force) zorlayın ve gerekirse yeniden başlatın. 

## <a name="encryption-key-storage-requirements"></a>Şifreleme anahtarı depolama gereksinimleri 

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını denetlemek ve yönetmek için Azure Key Vault gerektirir. Anahtar kasanız ve VM'leriniz aynı Azure bölgesinde ve abonelikte ikamet etmelidir.

Daha fazla bilgi için bkz. Azure [AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md).
 
## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md)
- [Linux VM'lerde Azure AD ile Azure Disk Şifrelemesini etkinleştirme (önceki sürüm)](disk-encryption-linux-aad.md)
- [Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure Disk Şifreleme PowerShell komut dosyası önkoşullar](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)