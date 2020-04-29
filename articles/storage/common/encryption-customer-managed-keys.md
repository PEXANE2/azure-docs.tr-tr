---
title: Hesap şifrelemeyi yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma
titleSuffix: Azure Storage
description: Depolama hesabınızdaki verileri korumak için kendi şifreleme anahtarınızı kullanabilirsiniz. Müşteri tarafından yönetilen bir anahtar belirttiğinizde, bu anahtar, verilerinizi şifreleyen anahtara erişimi korumak ve denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini yönetmek için daha fazla esneklik sunar.
services: storage
author: tamram
ms.service: storage
ms.date: 03/12/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: b2755d5aa5dbaa669fa2fdd8b84596e040b5dd6b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81456830"
---
# <a name="use-customer-managed-keys-with-azure-key-vault-to-manage-azure-storage-encryption"></a>Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma

Depolama hesabınızdaki verileri korumak için kendi şifreleme anahtarınızı kullanabilirsiniz. Müşteri tarafından yönetilen bir anahtar belirttiğinizde, bu anahtar, verilerinizi şifreleyen anahtara erişimi korumak ve denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini yönetmek için daha fazla esneklik sunar.

Müşteri tarafından yönetilen anahtarlarınızın depolanması için Azure Key Vault kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Depolama hesabı ve Anahtar Kasası aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../../key-vault/general/overview.md).

## <a name="about-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar hakkında

Aşağıdaki diyagramda, Azure depolama 'nın, müşteri tarafından yönetilen anahtarı kullanarak istek yapmak için Azure Active Directory ve Azure Key Vault nasıl kullandığı gösterilmektedir:

![Müşteri tarafından yönetilen anahtarların Azure Storage 'da nasıl çalıştığını gösteren diyagram](media/encryption-customer-managed-keys/encryption-customer-managed-keys-diagram.png)

Aşağıdaki listede, diyagramdaki numaralandırılmış adımlar açıklanmaktadır:

1. Azure Key Vault Yöneticisi, depolama hesabıyla ilişkili yönetilen kimliğe şifreleme anahtarları için izinler verir.
2. Azure Depolama Yöneticisi, depolama hesabı için müşteri tarafından yönetilen bir anahtarla şifrelemeyi yapılandırır.
3. Azure depolama, Azure Active Directory üzerinden Azure Key Vault erişimin kimliğini doğrulamak için depolama hesabıyla ilişkili yönetilen kimliği kullanır.
4. Azure depolama, hesap şifreleme anahtarını Azure Key Vault müşteri anahtarıyla sarmalanmış.
5. Okuma/yazma işlemleri için, Azure depolama, şifreleme ve şifre çözme işlemleri gerçekleştirmek üzere hesap şifreleme anahtarını sarmalamak için Azure Key Vault istekleri gönderir.

## <a name="create-an-account-that-supports-customer-managed-keys-for-queues-and-tables"></a>Kuyruklar ve tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturun

Kuyruk ve tablo hizmetlerinde depolanan veriler, depolama hesabı için müşteri tarafından yönetilen anahtarlar etkinleştirildiğinde müşteri tarafından yönetilen bir anahtarla otomatik olarak korunmaz. Bu hizmetleri isteğe bağlı olarak, bu korumaya dahil edilecek depolama hesabını oluşturduğunuz sırada yapılandırabilirsiniz.

Kuyruklar ve tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir depolama hesabı oluşturma hakkında daha fazla bilgi için, bkz. [Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma](account-encryption-key-create.md).

Blob ve dosya hizmetlerindeki veriler, depolama hesabı için müşteri tarafından yönetilen anahtarlar yapılandırıldığında müşterinin yönettiği anahtarlarla her zaman korunur.

## <a name="enable-customer-managed-keys-for-a-storage-account"></a>Bir depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirin

Müşterinin yönettiği anahtarlar yalnızca mevcut depolama hesaplarında etkinleştirilebilir. Anahtar kasasının, depolama hesabıyla ilişkili yönetilen kimliğe anahtar izinleri veren erişim ilkeleriyle sağlanması gerekir. Yönetilen kimlik yalnızca depolama hesabı oluşturulduktan sonra kullanılabilir.

Müşteri tarafından yönetilen bir anahtar yapılandırdığınızda, Azure depolama, ilişkili anahtar kasasındaki müşterinin yönettiği anahtarla hesaba ait kök veri şifreleme anahtarını sarmalanmış. Müşteri tarafından yönetilen anahtarların etkinleştirilmesi performansı etkilemez ve hemen yürürlüğe girer.

Müşteri tarafından yönetilen anahtarları etkinleştirerek veya devre dışı bırakarak, anahtar sürümünü güncelleştirerek veya farklı bir anahtar belirterek, Azure depolama şifrelemesi için kullanılan anahtarı değiştirdiğinizde, kök anahtarın şifrelenmesi değişir ancak Azure Storage hesabınızdaki verilerin yeniden şifrelenmesi gerekmez.

Müşteri tarafından yönetilen anahtarları etkinleştirdiğinizde veya devre dışı bıraktığınızda veya anahtarı ya da anahtar sürümünü değiştirdiğinizde, kök şifreleme anahtarının korunması değişir, ancak Azure Depolama hesabınızdaki verilerin yeniden şifrelenmesi gerekmez.

Azure depolama şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanmayı öğrenmek için şu makalelerden birine bakın:

- [Azure portal Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-portal.md)
- [Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları PowerShell 'den yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLı 'dan Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure AD 'nin bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Yönetilen kimlikler Şu anda çapraz dizin senaryolarını desteklemez. Azure portal müşteri tarafından yönetilen anahtarları yapılandırırken, yönetilen bir kimlik, kapakları altında depolama hesabınıza otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya depolama hesabını bir Azure AD dizininden diğerine taşırsanız, depolama hesabıyla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. SSS 'de **Azure AD dizinleri arasında bir abonelik aktarma** [ve Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

## <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde depola

Bir depolama hesabında müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanmanız gerekir. Anahtar kasasında hem **geçici silme** hem de **Temizleme** özelliklerini etkinleştirmeniz gerekir.

Azure depolama şifrelemesi ile yalnızca 2048 bit RSA ve RSA-HSM anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys) **Key Vault anahtarlar** .

## <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndür

Azure Key Vault, müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, depolama hesabını yeni anahtar sürümü URI 'sini kullanacak şekilde güncelleştirmeniz gerekir. Azure portal anahtarın yeni bir sürümünü kullanmak üzere depolama hesabını güncelleştirme hakkında bilgi edinmek için [Azure Portal kullanarak Azure depolama için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-portal.md)konusunun **temel sürümü güncelleştirme** başlıklı bölümüne bakın.

Anahtarın döndürülmesi, depolama hesabındaki verilerin yeniden şifrelenmesini tetiklemez. Kullanıcıdan başka bir eylem gerekli değildir.

## <a name="revoke-access-to-customer-managed-keys"></a>Müşterinin yönettiği anahtarlara erişimi iptal etme

Depolama hesabının müşterinin yönettiği anahtara erişimini istediğiniz zaman iptal edebilirsiniz. Müşteri tarafından yönetilen anahtarlara erişim iptal edildiğinde veya anahtar devre dışı bırakıldıktan veya silindikten sonra istemciler, bir Blobun veya meta verilerinden okuma veya yazma işlemlerini çağıramaz. Aşağıdaki işlemlerden herhangi birini çağırma girişimleri, tüm kullanıcılar için 403 (yasak) hata kodu ile başarısız olur:

- İstek URI 'sindeki `include=metadata` parametresiyle çağrıldığında [Blobları Listele](/rest/api/storageservices/list-blobs)
- [Blob al](/rest/api/storageservices/get-blob)
- [BLOB özelliklerini Al](/rest/api/storageservices/get-blob-properties)
- [Blob meta verilerini al](/rest/api/storageservices/get-blob-metadata)
- [Blob meta verilerini ayarla](/rest/api/storageservices/set-blob-metadata)
- `x-ms-meta-name` İstek üstbilgisiyle çağrıldığında [anlık görüntü blobu](/rest/api/storageservices/snapshot-blob)
- [İkili Büyük Nesneyi Kopyalama](/rest/api/storageservices/copy-blob)
- [Blob 'U URL 'Den Kopyala](/rest/api/storageservices/copy-blob-from-url)
- [Blob Katmanını Ayarla](/rest/api/storageservices/set-blob-tier)
- [Yerleştirme bloğu](/rest/api/storageservices/put-block)
- [Bloğu URL 'Den koy](/rest/api/storageservices/put-block-from-url)
- [Ekleme bloğu](/rest/api/storageservices/append-block)
- [URL 'Den ekleme bloğu](/rest/api/storageservices/append-block-from-url)
- [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob)
- [Yerleştirme sayfası](/rest/api/storageservices/put-page)
- [URL 'Den sayfa koy](/rest/api/storageservices/put-page-from-url)
- [Artımlı kopya blobu](/rest/api/storageservices/incremental-copy-blob)

Bu işlemleri yeniden çağırmak için, müşteri tarafından yönetilen anahtara erişimi geri yükleyin.

Bu bölümde listelenmeyen tüm veri işlemleri, müşteri tarafından yönetilen anahtarlar iptal edildiğinde veya bir anahtar devre dışı bırakıldıktan veya silindikten sonra devam edebilir.

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için [PowerShell](storage-encryption-keys-powershell.md#revoke-customer-managed-keys) veya [Azure CLI](storage-encryption-keys-cli.md#revoke-customer-managed-keys)kullanın.

## <a name="customer-managed-keys-for-azure-managed-disks"></a>Azure yönetilen diskler için müşteri tarafından yönetilen anahtarlar

Azure yönetilen disklerin şifrelenmesini yönetmek için müşteri tarafından yönetilen anahtarlar da kullanılabilir. Müşteri tarafından yönetilen anahtarlar, yönetilen diskler için Azure depolama kaynaklarından farklı şekilde davranır. Daha fazla bilgi için bkz. Windows için [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](../../virtual-machines/windows/disk-encryption.md) veya Linux için [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](../../virtual-machines/linux/disk-encryption.md) .

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-portal.md)
- [Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları PowerShell 'den yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLı 'dan Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-cli.md)
- [Bekleyen veriler için Azure depolama şifrelemesi](storage-service-encryption.md)
