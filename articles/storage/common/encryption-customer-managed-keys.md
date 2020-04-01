---
title: Hesap şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanın
titleSuffix: Azure Storage
description: Depolama hesabınızdaki verileri korumak için kendi şifreleme anahtarınızı kullanabilirsiniz. Müşteri tarafından yönetilen bir anahtar belirttiğinizde, bu anahtar verilerinizi şifreleyen anahtarı korumak ve bu anahtara erişimi denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini yönetmek için daha fazla esneklik sağlar.
services: storage
author: tamram
ms.service: storage
ms.date: 03/12/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 6b5712094b9821dfa041cd5ba8617e86f7231bde
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478009"
---
# <a name="use-customer-managed-keys-with-azure-key-vault-to-manage-azure-storage-encryption"></a>Azure Depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanın

Depolama hesabınızdaki verileri korumak için kendi şifreleme anahtarınızı kullanabilirsiniz. Müşteri tarafından yönetilen bir anahtar belirttiğinizde, bu anahtar verilerinizi şifreleyen anahtarı korumak ve bu anahtara erişimi denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini yönetmek için daha fazla esneklik sağlar.

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Key Vault'u kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Depolama hesabı ve anahtar kasası aynı bölgede ve aynı Azure Etkin Dizin (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../../key-vault/key-vault-overview.md)

## <a name="about-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar hakkında

Aşağıdaki diyagram, Azure Depolama'nın müşteri tarafından yönetilen anahtarı kullanarak istekte bulunmak için Azure Etkin Dizin ve Azure Anahtar Kasası'nı nasıl kullandığını gösterir:

![Azure Depolama'da müşteri tarafından yönetilen anahtarların nasıl çalıştığını gösteren diyagram](media/encryption-customer-managed-keys/encryption-customer-managed-keys-diagram.png)

Aşağıdaki liste, diyagramdaki numaralanmış adımları açıklar:

1. Azure Key Vault yöneticisi, depolama hesabıyla ilişkili yönetilen kimliğin şifreleme anahtarları için izin verir.
2. Azure Depolama yöneticisi, depolama hesabı için müşteri tarafından yönetilen bir anahtarla şifrelemeyi yapılandırır.
3. Azure Depolama, Azure Etkin Dizini aracılığıyla Azure Key Vault erişiminin kimliğini doğrulamak için depolama hesabıyla ilişkili yönetilen kimliği kullanır.
4. Azure Depolama, Azure Key Vault'ta müşteri anahtarıyla hesap şifreleme anahtarını sarar.
5. Azure Depolama, okuma/yazma işlemleri için, şifreleme ve şifre çözme işlemleri gerçekleştirmek için hesap şifreleme anahtarını açmak için Azure Key Vault'a istekler gönderir.

## <a name="create-an-account-that-supports-customer-managed-keys-for-queues-and-tables"></a>Kuyruklar ve tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma

Mağaza hesabı için müşteri tarafından yönetilen anahtarlar etkinleştirildiğinde, Sıra ve Tablo hizmetlerinde depolanan veriler müşteri tarafından yönetilen bir anahtar tarafından otomatik olarak korunmaz. Bu korumaya dahil edilecek depolama hesabını oluşturduğunuz anda bu hizmetleri isteğe bağlı olarak yapılandırabilirsiniz.

Kuyruklar ve tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir depolama hesabı oluşturma hakkında daha fazla bilgi için [bkz.](account-encryption-key-create.md)

Müşteri tarafından yönetilen anahtarlar depolama hesabı için yapılandırıldığında, Blob ve Dosya hizmetlerindeki veriler her zaman müşteri tarafından yönetilen anahtarlarla korunur.

## <a name="enable-customer-managed-keys-for-a-storage-account"></a>Depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirme

Müşteri tarafından yönetilen anahtarlar yalnızca varolan depolama hesaplarında etkinleştirilebilir. Anahtar kasası, depolama hesabıyla ilişkili yönetilen kimliğe önemli izinler veren erişim ilkeleriyle birlikte sağlanmalıdır. Yönetilen kimlik yalnızca depolama hesabı oluşturulduktan sonra kullanılabilir.

Müşteri tarafından yönetilen bir anahtarı yapılandırdığınızda, Azure Depolama ilgili anahtar kasasındamüşteri tarafından yönetilen anahtarla hesabın kök veri şifreleme anahtarını sarar. Müşteri tarafından yönetilen anahtarları etkinleştirmek performansı etkilemez ve hemen etkili olur.

Müşteri tarafından yönetilen anahtarları etkinleştirerek veya devre dışı bırakarak, anahtar sürümünü güncelleştirerek veya farklı bir anahtar belirterek Azure Depolama şifrelemesi için kullanılan anahtarı değiştirdiğinizde, kök anahtarın şifrelemesi değişir, ancak Azure Depolama hesabınızdaki verilerin yeniden şifrelenmesi gerekmez.

Müşteri yönetilen anahtarları etkinleştirdiğinizde veya devre dışı kaldığınız zaman veya anahtarı veya anahtarı değiştirdiğinizde, kök şifreleme anahtarının koruması değişir, ancak Azure Depolama hesabınızdaki verilerin yeniden şifrelenmiş olması gerekmez.

Azure Depolama şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları nasıl kullanacağınızı öğrenmek için şu makalelerden birine bakın:

- [Azure portalından Azure Depolama için Anahtar Kasa şifrelemesi ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-portal.md)
- [PowerShell'den Azure Depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-powershell.md)
- [Azure CLI'den Azure Depolama için Key Vault şifrelemesi ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure AD özelliği olan Azure kaynakları için yönetilen kimliklere dayanır. Yönetilen kimlikler şu anda çapraz dizin senaryolarını desteklemiyor. Azure portalında müşteri tarafından yönetilen anahtarları yapılandırdığınızda, yönetilen bir kimlik, kapaklar altında otomatik olarak depolama hesabınıza atanır. Daha sonra abonelik, kaynak grubu veya depolama hesabını bir Azure REKLAM dizininden diğerine taşırsanız, depolama hesabıyla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşteri tarafından yönetilen anahtarlar artık çalışmayabilir. Daha fazla bilgi için **bkz.** [FAQs and known issues with managed identities for Azure resources](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories)  

## <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası'nda saklayın

Bir depolama hesabında müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak için bir Azure Anahtar Kasası kullanmanız gerekir. Hem **Yumuşak Silme** **hem** de Temizleme özelliklerini anahtar kasasında etkinleştirmelisiniz.

Azure Depolama şifrelemesi ile yalnızca 2048 bit RSA ve RSA-HSM anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys)Key Vault **tuşlarına** bakın.

## <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndürme

Azure Key Vault'ta müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, yeni anahtar sürümü URI'yi kullanmak için depolama hesabını güncelleştirmeniz gerekir. Azure portalında anahtarın yeni bir sürümünü kullanmak için depolama hesabını nasıl güncelleştirişteğini öğrenmek [için, Azure portalını kullanarak Azure Depolama için müşteri tarafından yönetilen anahtarları yapılandır'da](storage-encryption-keys-portal.md) **anahtar sürümünü güncelleştir** başlıklı bölüme bakın.

Anahtarı döndürmek, depolama hesabındaki verilerin yeniden şifresini tetiklemez. Kullanıcıdan başka bir işlem gerekmez.

## <a name="revoke-access-to-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlara erişimi iptal etme

Depolama hesabının müşteri tarafından yönetilen anahtara erişimini istediğiniz zaman iptal edebilirsiniz. Müşteri tarafından yönetilen anahtarlara erişim iptal edildikten veya anahtar devre dışı bırakıldıktan veya silindikten sonra, istemciler bir blob'dan veya meta verilerinden okunan veya yazan işlemleri arayamaz. Aşağıdaki işlemlerden herhangi birini çağırma girişimleri tüm kullanıcılar için hata kodu 403 (Yasak) ile başarısız olur:

- [Liste Blobs](/rest/api/storageservices/list-blobs), istek `include=metadata` URI parametresi ile çağrıldığında
- [Blob alın](/rest/api/storageservices/get-blob)
- [BLOB özelliklerini Al](/rest/api/storageservices/get-blob-properties)
- [Blob Meta verilerini alın](/rest/api/storageservices/get-blob-metadata)
- [Blob Meta verilerini ayarlama](/rest/api/storageservices/set-blob-metadata)
- [Anlık Görüntü Blob](/rest/api/storageservices/snapshot-blob), `x-ms-meta-name` istek üstbilgisi ile çağrıldığında
- [İkili Büyük Nesneyi Kopyalama](/rest/api/storageservices/copy-blob)
- [Blob'u URL'den Kopyala](/rest/api/storageservices/copy-blob-from-url)
- [Blob Katmanını Ayarla](/rest/api/storageservices/set-blob-tier)
- [Blok Koy](/rest/api/storageservices/put-block)
- [URL'den Blok Koy](/rest/api/storageservices/put-block-from-url)
- [Ek Blok](/rest/api/storageservices/append-block)
- [URL'den Gelen Engellemeyi Ekle](/rest/api/storageservices/append-block-from-url)
- [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob)
- [Sayfa Koy](/rest/api/storageservices/put-page)
- [URL'den Sayfa Koy](/rest/api/storageservices/put-page-from-url)
- [Artımlı Kopya Blob](/rest/api/storageservices/incremental-copy-blob)

Bu işlemleri yeniden aramak için, müşteri tarafından yönetilen anahtara erişimi geri yükleyin.

Bu bölümde listelenmemiş tüm veri işlemleri, müşteri tarafından yönetilen anahtarlar iptal edildikten veya anahtar devre dışı bırakıldıktan veya silindikten sonra devam edebilir.

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için [PowerShell](storage-encryption-keys-powershell.md#revoke-customer-managed-keys) veya [Azure CLI'yi](storage-encryption-keys-cli.md#revoke-customer-managed-keys)kullanın.

## <a name="customer-managed-keys-for-azure-managed-disks"></a>Azure yönetilen diskler için müşteri tarafından yönetilen anahtarlar

Azure yönetilen disklerin şifrelemesini yönetmek için müşteri tarafından yönetilen anahtarlar da kullanılabilir. Müşteri tarafından yönetilen anahtarlar, yönetilen diskler için Azure Depolama kaynaklarından farklı şekilde çalışır. Daha fazla bilgi için, Linux için Azure yönetilen disklerin Windows veya [Server tarafı şifrelemesi](../../virtual-machines/linux/disk-encryption.md) için Azure yönetilen [disklerin Sunucu tarafı şifrelemesi](../../virtual-machines/windows/disk-encryption.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalından Azure Depolama için Anahtar Kasa şifrelemesi ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-portal.md)
- [PowerShell'den Azure Depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-powershell.md)
- [Azure CLI'den Azure Depolama için Key Vault şifrelemesi ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-cli.md)
- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md)
