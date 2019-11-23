---
title: Azure AD ile Azure disk şifrelemesi (önceki sürüm)
description: Bu makale, Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi kullanılarak önkoşulları sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 33b257e9d344fc31df072509f105d2e8fd1bd29b
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72245181"
---
# <a name="azure-disk-encryption-with-azure-ad-previous-release"></a>Azure AD ile Azure disk şifrelemesi (önceki sürüm)

**Azure disk şifrelemesi 'nin yeni sürümü, VM disk şifrelemeyi etkinleştirmek için bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümde, artık şifrelemeyi Etkinleştir adımını kullanarak Azure AD kimlik bilgilerini sağlamanız gerekmez. Yeni sürüm kullanılarak Azure AD uygulama parametreleri olmadan tüm yeni VM 'Ler şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirme yönergelerini görüntülemek için bkz. [Windows VM 'leri Için Azure disk şifrelemesi](disk-encryption-overview.md). Azure AD uygulama parametreleriyle zaten şifrelenmiş VM 'Ler hala desteklenmektedir ve AAD sözdizimi ile sürdürülmeye devam etmelidir.**

Bu makale, Azure AD (önceki sürüm) ile Azure disk şifrelemesi için ek gereksinimler ve Önkoşullar içeren [Windows VM 'leri Için Azure disk şifrelemesi](disk-encryption-overview.md) 'ni tamamlar. [Desteklenen VM 'ler ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems) bölümü aynı kalır.

## <a name="networking-and-group-policy"></a>Ağ ve grup ilkesi

**Parametresi söz dizimi, Iaas Vm'leri eski AAD kullanan Azure Disk şifrelemesi özelliği etkinleştirmek için aşağıdaki ağ uç noktası yapılandırması gereksinimleri karşılaması gerekir:** 
  - Anahtar kasanızı bağlanmak için bir belirteç almak üzere Iaas VM bir Azure Active Directory uç noktasına bağlanabilir olmalıdır \[login.microsoftonline.com\].
  - Şifreleme anahtarları için anahtar kasanıza yazmak için Iaas VM anahtar kasası uç noktasına bağlanabilir olmalıdır.
  - Iaas VM Azure uzantısı depoyu ve VHD dosyalarını barındıran Azure depolama hesabı'nı barındıran bir Azure depolama uç noktasına bağlanabiliyor olmanız gerekir.
  -  Güvenlik ilkeniz Azure vm'lerinden Internet erişimi sınırlayan, önceki URI çözmek ve IP'ler giden bağlantıya izin verecek bir kuralı yapılandırın. Daha fazla bilgi için [bir güvenlik duvarının ardındayken Azure anahtar kasası](../../key-vault/key-vault-access-behind-firewall.md).
  - Windows üzerinde TLS 1.0 açıkça devre dışı bırakıldı ve .NET sürümünü 4.6 veya üzeri güncelleştirilmemiş ADE daha yeni TLS sürümünü seçmek aşağıdaki kayıt defteri değişikliği olanak tanıyacak:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

**Grup İlkesi:**
 - Azure Disk şifrelemesi çözümü, BitLocker dış anahtar koruyucusu Windows Iaas Vm'leri için kullanır. Etki alanına katılmış sanal makineleri, TPM koruyucusu zorlamak için tüm grup ilkeleri anında iletme yok. "Uyumlu TPM'siz BitLocker izin ver" için Grup İlkesi hakkında bilgi için bkz: [BitLocker Grup İlkesi başvurusu](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1).

-  Özel grup ilkesiyle etki alanına katılmış sanal makinelerde BitLocker ilkesi şu ayarı içermelidir: [BitLocker kurtarma bilgileri 'nin Kullanıcı depolamasını yapılandırma-> Izin ver 256-bit kurtarma anahtarı](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). BitLocker için özel Grup İlkesi ayarları uyumsuz olduğunda Azure disk şifrelemesi başarısız olur. Doğru ilkeyi gerekmedi makinelerde yeni ilkeyi uygulamak, (gpupdate.exe/Force) güncelleştirmek için yeni ilke zorlayın ve daha sonra yeniden başlatmak gerekli olabilir.  

## <a name="encryption-key-storage-requirements"></a>Şifreleme anahtarı depolama gereksinimleri  

Azure disk şifrelemesi, disk şifreleme anahtarlarını ve gizli dizileri denetlemek ve yönetmek için bir Azure Key Vault gerektirir. Anahtar kasanızın ve sanal makinelerinizin aynı Azure bölgesinde ve abonelikte bulunması gerekir.

Ayrıntılar için bkz. Azure [ad Ile Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md).
 
## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD ile Azure disk şifrelemesi için bir Anahtar Kasası oluşturma ve yapılandırma (önceki sürüm)](disk-encryption-key-vault-aad.md)
- [Windows VM 'lerde Azure AD ile Azure disk şifrelemesini etkinleştirme (önceki sürüm)](disk-encryption-windows-aad.md)
- [Azure disk şifrelemesi önkoşulları CLı betiği](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure disk şifrelemesi önkoşulları PowerShell betiği](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)