---
title: Bekleyen veriler için Azure depolama şifrelemesi | Microsoft Docs
description: Azure depolama, verilerinizi buluta kalıcı yapmadan önce otomatik olarak şifreleyerek korur. Bir Azure depolama alanındaki tüm veriler, 256 bit AES şifrelemesi kullanılarak şifrelenmiş ve şifrelenmemiş şekilde çözülür ve FIPS 140-2 uyumludur.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 2abaa994bf507c3ffb65199af9ac609ece138ea4
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2019
ms.locfileid: "71673057"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Bekleyen veriler için Azure depolama şifrelemesi

Azure depolama, verilerinizi buluta kalıcı hale geldiğinde otomatik olarak şifreler. Şifreleme, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Azure depolama 'daki veriler, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi, Windows 'da BitLocker şifrelemeye benzer.

Azure depolama şifrelemesi, tüm yeni ve mevcut depolama hesapları için etkinleştirilmiştir ve devre dışı bırakılamaz. Verileriniz varsayılan olarak güvenli hale getirildiğinden, Azure depolama şifrelemesi 'nin avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez. 

Depolama hesapları, performans katmanlarından (Standart veya Premium) veya dağıtım modelinden (Azure Resource Manager veya klasik) bağımsız olarak şifrelenir. Tüm Azure depolama artıklığı seçenekleri şifrelemeyi destekler ve bir depolama hesabının tüm kopyaları şifrelenir. Blob 'lar, diskler, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm Azure depolama kaynakları şifrelenir. Tüm nesne meta verileri de şifrelenir.

Şifreleme, Azure depolama performansını etkilemez. Azure depolama şifrelemesi için ek bir maliyet yoktur.

Azure depolama şifrelemesini temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz. [Cryptography API: Yeni nesil @ no__t-0.

## <a name="key-management"></a>Anahtar yönetimi

Depolama hesabınızın şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya Azure Key Vault ile birlikte kendi anahtarlarınızla şifrelemeyi yönetebilirsiniz.

### <a name="microsoft-managed-keys"></a>Microsoft tarafından yönetilen anahtarlar

Varsayılan olarak, depolama hesabınız Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Aşağıdaki görüntüde gösterildiği gibi, [Azure Portal](https://portal.azure.com) **şifreleme** bölümünde depolama hesabınızın şifreleme ayarlarını görebilirsiniz.

![Microsoft tarafından yönetilen anahtarlarla şifrelenen hesabı görüntüle](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

### <a name="customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar

Azure depolama şifrelemesini, müşteri tarafından yönetilen anahtarlarla yönetebilirsiniz. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturma, döndürme, devre dışı bırakma ve iptal etme konusunda daha fazla esneklik sağlar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz. 

Anahtarlarınızı yönetmek ve anahtar kullanımınızı denetlemek için Azure Key Vault kullanın. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Depolama hesabı ve Anahtar Kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md).

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için bkz. [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) ve [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault)Azure Key Vault. Erişimi iptal etmek, şifreleme anahtarına Azure depolama tarafından erişilemediğinden, depolama hesabındaki tüm verilere erişimi etkin bir şekilde engeller.

Azure depolama ile müşteri tarafından yönetilen anahtarları kullanmayı öğrenmek için şu makalelerden birine bakın:

- [Azure portalından Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-portal.md)
- [PowerShell'den Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLı ile Azure depolama şifrelemesi ile müşteri tarafından yönetilen anahtarları kullanma](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Active Directory (Azure AD) bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Bir aboneliği bir Azure AD dizininden diğerine aktardığınızda, Yönetilen kimlikler güncellenmez ve müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. SSS 'de **Azure AD dizinleri arasında bir abonelik aktarma** [ve Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

> [!NOTE]  
> [Azure yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md)için müşteri tarafından yönetilen anahtarlar desteklenmez.

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Azure depolama şifrelemesi ile disk şifrelemesi karşılaştırması

Azure depolama şifrelemesi sayesinde, Azure sanal makine disklerini geri yükleyen sayfa Blobları da dahil olmak üzere tüm Azure depolama hesapları ve içerdikleri kaynaklar şifrelenir. Ayrıca Azure sanal makine diskleri, [Azure disk şifrelemesi](../../security/azure-security-disk-encryption-overview.md)ile şifrelenmiş olabilir. Azure disk şifrelemesi, Azure Key Vault ile tümleştirilmiş işletim sistemi tabanlı şifreleme çözümleri sağlamak için Windows üzerinde sektör standardı [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) ve Linux üzerinde [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md)
