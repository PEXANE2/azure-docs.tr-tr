---
title: Bekleyen veriler için Azure depolama şifrelemesi
description: Azure depolama, verilerinizi buluta kalıcı yapmadan önce otomatik olarak şifreleyerek korur. Depolama hesabınızın şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.date: 01/10/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: abb9325510b52672027338314e02466f2d28e701
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942200"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Bekleyen veriler için Azure depolama şifrelemesi

Azure depolama, verilerinizi buluta kalıcı hale geldiğinde otomatik olarak şifreler. Azure depolama şifrelemesi, verilerinizi korur ve kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

## <a name="about-azure-storage-encryption"></a>Azure depolama şifrelemesi hakkında

Azure depolama 'daki veriler, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi, Windows 'da BitLocker şifrelemeye benzer.

Azure depolama şifrelemesi, hem Kaynak Yöneticisi hem de klasik depolama hesapları da dahil olmak üzere tüm yeni depolama hesapları için etkinleştirilmiştir. Azure depolama şifrelemesi devre dışı bırakılamıyor. Verileriniz varsayılan olarak güvenli hale getirildiğinden, Azure depolama şifrelemesi 'nin avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

Depolama hesapları, performans katmanlarından (Standart veya Premium) veya dağıtım modelinden (Azure Resource Manager veya klasik) bağımsız olarak şifrelenir. Tüm Azure depolama artıklığı seçenekleri şifrelemeyi destekler ve bir depolama hesabının tüm kopyaları şifrelenir. Blob 'lar, diskler, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm Azure depolama kaynakları şifrelenir. Tüm nesne meta verileri de şifrelenir.

Şifreleme, Azure depolama performansını etkilemez. Azure depolama şifrelemesi için ek bir maliyet yoktur.

20 Ekim 2017 ' den sonra Azure depolama 'ya yazılan her Blok Blobu, ekleme Blobu veya Sayfa Blobu şifrelenir. Bu tarihten önce oluşturulan Bloblar, bir arka plan işlemi tarafından şifrelenmeye devam eder. 20 Ekim 2017 ' den önce oluşturulan bir Blobun şifrelemeye zorlamak için, blobu yeniden yazabilirsiniz. Bir Blobun şifreleme durumunu denetleme hakkında bilgi edinmek için bkz. [bir Blobun şifreleme durumunu denetleme](../blobs/storage-blob-encryption-status.md).

Azure depolama şifrelemesini temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz. [şifreleme API 'si: yeni nesil](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Depolama hesabınızın şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz iki seçeneğiniz vardır:

- Blob depolamada ve Azure dosyalarında verileri şifrelemek ve şifrelerini çözmek için Azure Key Vault ile *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. <sup>1, 2</sup>
- BLOB depolama işlemlerinde, *müşteri tarafından sağlanmış bir anahtar* belirtebilirsiniz. Blob depolamaya karşı okuma veya yazma isteği yapan bir istemci, blob verilerinin şifrelenme ve şifresinin çözülmesi üzerinde ayrıntılı denetim isteğine yönelik bir şifreleme anahtarı içerebilir.

Aşağıdaki tabloda, Azure depolama şifrelemesi için anahtar yönetim seçenekleri karşılaştırılmaktadır.

|                                        |    Microsoft tarafından yönetilen anahtarlar                             |    Müşteri tarafından yönetilen anahtarlar                                                                                                                        |    Müşteri tarafından sunulan anahtarlar                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Şifreleme/şifre çözme işlemleri    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Desteklenen Azure depolama hizmetleri    |    Tümü                                                |    BLOB depolama, Azure dosyaları<sup>1, 2</sup>                                                                                                               |    Blob depolaması                                                                  |
|    Anahtar depolama                         |    Microsoft anahtar deposu    |    Azure Key Vault                                                                                                                              |    Azure Key Vault veya başka bir anahtar deposu                                                                 |
|    Anahtar döndürme sorumluluğu         |    Microsoft                                          |    Müşteri                                                                                                                                     |    Müşteri                                                                      |
|    Anahtar kullanımı                           |    Microsoft                                          |    Azure portal, depolama kaynak sağlayıcısı REST API, Azure depolama yönetim kitaplıkları, PowerShell, CLı        |    Azure depolama REST API (BLOB depolama), Azure depolama istemci kitaplıkları    |
|    Anahtar erişimi                          |    Yalnızca Microsoft                                     |    Microsoft, müşteri                                                                                                                    |    Yalnızca müşteri                                                                 |

<sup>1</sup> müşteri tarafından yönetilen anahtarları kuyruk depolama ile kullanmayı destekleyen bir hesap oluşturma hakkında bilgi için, bkz. [kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)oluşturma.<br />
<sup>2</sup> müşteri tarafından yönetilen anahtarları tablo depolama ile kullanmayı destekleyen bir hesap oluşturma hakkında bilgi için bkz. [tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json).

Aşağıdaki bölümlerde, anahtar yönetimiyle ilgili seçeneklerin her biri daha ayrıntılı olarak açıklanır.

## <a name="microsoft-managed-keys"></a>Microsoft tarafından yönetilen anahtarlar

Varsayılan olarak, depolama hesabınız Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Aşağıdaki görüntüde gösterildiği gibi, [Azure Portal](https://portal.azure.com) **şifreleme** bölümünde depolama hesabınızın şifreleme ayarlarını görebilirsiniz.

![Microsoft tarafından yönetilen anahtarlarla şifrelenen hesabı görüntüle](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Key Vault ile müşteri tarafından yönetilen anahtarlar

Azure depolama şifrelemesini, kendi anahtarlarınızın bulunduğu depolama hesabı düzeyinde yönetebilirsiniz. Depolama hesabı düzeyinde müşteri tarafından yönetilen bir anahtar belirttiğinizde, bu anahtar, tüm blob ve dosya verilerini şifrelemek ve şifrelerini çözmek için kullanılan depolama hesabı için kök şifreleme anahtarına erişimi korumak ve denetlemek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Müşteri tarafından yönetilen anahtarlarınızın depolanması için Azure Key Vault kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Depolama hesabı ve Anahtar Kasası aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md).

Bu diyagramda, Azure depolama 'nın, müşteri tarafından yönetilen anahtarı kullanarak istek yapmak için Azure Active Directory ve Azure Key Vault nasıl kullandığı gösterilmektedir:

![Müşteri tarafından yönetilen anahtarların Azure Storage 'da nasıl çalıştığını gösteren diyagram](media/storage-service-encryption/encryption-customer-managed-keys-diagram.png)

Aşağıdaki listede, diyagramdaki numaralandırılmış adımlar açıklanmaktadır:

1. Azure Key Vault Yöneticisi, depolama hesabıyla ilişkili yönetilen kimliğe şifreleme anahtarları için izinler verir.
2. Azure Depolama Yöneticisi, depolama hesabı için müşteri tarafından yönetilen bir anahtarla şifrelemeyi yapılandırır.
3. Azure depolama, Azure Active Directory üzerinden Azure Key Vault erişimin kimliğini doğrulamak için depolama hesabıyla ilişkili yönetilen kimliği kullanır.
4. Azure depolama, hesap şifreleme anahtarını Azure Key Vault müşteri anahtarıyla sarmalanmış.
5. Okuma/yazma işlemleri için, Azure depolama, şifreleme ve şifre çözme işlemleri gerçekleştirmek üzere hesap şifreleme anahtarını sarmalamak ve sarmalamak için Azure Key Vault istek gönderir.

### <a name="enable-customer-managed-keys-for-a-storage-account"></a>Bir depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirin

Bir depolama hesabı için müşteri tarafından yönetilen anahtarlarla şifrelemeyi etkinleştirdiğinizde, Azure depolama, hesap şifreleme anahtarını ilişkili anahtar kasasındaki müşteri tarafından yönetilen anahtarla sarar. Müşteri tarafından yönetilen anahtarların etkinleştirilmesi performansı etkilemez ve hesap, her zaman gecikmesi olmadan yeni anahtarla şifrelenir.

Yeni bir depolama hesabı her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Hesap oluşturulduğu sırada müşteri tarafından yönetilen anahtarları etkinleştirmek mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Key Vault depolanır ve Anahtar Kasası, depolama hesabıyla ilişkili yönetilen kimliğe anahtar izinleri veren erişim ilkeleriyle sağlanmalıdır. Yönetilen kimlik yalnızca depolama hesabı oluşturulduktan sonra kullanılabilir.

Müşteri tarafından yönetilen anahtarları etkinleştirerek veya devre dışı bırakarak, anahtar sürümünü güncelleştirerek veya farklı bir anahtar belirterek, Azure depolama şifrelemesi için kullanılan anahtarı değiştirdiğinizde, kök anahtarın şifrelenmesi değişir ancak Azure Storage hesabınızdaki veriler yeniden şifrelenmesi gerekir.

Azure depolama şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanmayı öğrenmek için şu makalelerden birine bakın:

- [Azure portal Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-portal.md)
- [Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları PowerShell 'den yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLı 'dan Azure depolama şifrelemesi için Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure AD 'nin bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Yönetilen kimlikler Şu anda çapraz dizin senaryolarını desteklemez. Azure portal müşteri tarafından yönetilen anahtarları yapılandırırken, yönetilen bir kimlik, kapakları altında depolama hesabınıza otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya depolama hesabını bir Azure AD dizininden diğerine taşırsanız, depolama hesabıyla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. SSS 'de **Azure AD dizinleri arasında bir abonelik aktarma** [ve Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde depola

Bir depolama hesabında müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanmanız gerekir. Anahtar kasasında hem **geçici silme** hem de **Temizleme** özelliklerini etkinleştirmeniz gerekir.

Azure depolama şifrelemesi ile yalnızca 2048 boyutundaki RSA anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys) **Key Vault anahtarlar** .

### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndür

Azure Key Vault, müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, depolama hesabını yeni anahtar URI 'sini kullanacak şekilde güncelleştirmeniz gerekir. Azure portal anahtarın yeni bir sürümünü kullanmak üzere depolama hesabını güncelleştirme hakkında bilgi edinmek için [Azure Portal kullanarak Azure depolama için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-portal.md)konusunun **temel sürümü güncelleştirme** başlıklı bölümüne bakın.

Anahtarın döndürülmesi, depolama hesabındaki verilerin yeniden şifrelenmesini tetiklemez. Kullanıcıdan başka bir eylem gerekli değildir.

### <a name="revoke-access-to-customer-managed-keys"></a>Müşterinin yönettiği anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLı kullanın. Daha fazla bilgi için bkz. PowerShell veya [Azure Key Vault clı](/cli/azure/keyvault) [Azure Key Vault](/powershell/module/az.keyvault//) . Erişimi iptal etmek, şifreleme anahtarına Azure depolama tarafından erişilemediğinden, depolama hesabındaki tüm verilere erişimi etkin bir şekilde engeller.

### <a name="customer-managed-keys-for-azure-managed-disks-preview"></a>Azure yönetilen diskler için müşteri tarafından yönetilen anahtarlar (Önizleme)

Azure yönetilen diskler şifrelemesini (Önizleme) yönetmek için müşteri tarafından yönetilen anahtarlar da mevcuttur. Müşteri tarafından yönetilen anahtarlar, yönetilen diskler için Azure depolama kaynaklarından farklı şekilde davranır. Daha fazla bilgi için bkz. Windows için [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](../../virtual-machines/windows/disk-encryption.md) veya Linux için [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](../../virtual-machines/linux/disk-encryption.md) .

## <a name="customer-provided-keys-preview"></a>Müşteri tarafından sunulan anahtarlar (Önizleme)

Azure Blob depolamada istek yapan istemcilerin, tek bir istekte şifreleme anahtarı sağlama seçeneği vardır. İstek üzerine şifreleme anahtarı dahil olmak üzere, BLOB depolama işlemleri için şifreleme ayarları üzerinde ayrıntılı denetim sağlar. Müşteri tarafından sunulan anahtarlar (Önizleme), Azure Key Vault veya başka bir anahtar deposunda depolanabilir.

Blob depolamaya yönelik bir istekte müşteri tarafından sağlanmış bir anahtarın nasıl belirtileceğini gösteren bir örnek için, bkz. [.net Ile BLOB depolama için bir istekte müşteri tarafından sağlanmış anahtar belirtme](../blobs/storage-blob-customer-provided-key.md). 

### <a name="encrypting-read-and-write-operations"></a>Okuma ve yazma işlemlerini şifreleme

İstemci uygulaması istekte bir şifreleme anahtarı sağlıyorsa, blob verilerini okurken ve yazarken Azure Storage şifreleme ve şifre çözme işlemlerini saydam bir şekilde gerçekleştirir. Azure depolama, blob içeriklerinin yanı sıra şifreleme anahtarının SHA-256 karmasını yazar. Karma, blob 'a yönelik tüm sonraki işlemlerin aynı şifreleme anahtarını kullanmasını doğrulamak için kullanılır. 

Azure depolama, istemcinin istekle birlikte gönderdiği şifreleme anahtarını depolamaz veya yönetemez. Şifreleme veya şifre çözme işlemi tamamlandıktan hemen sonra anahtar güvenle atılır.

Bir istemci, müşteri tarafından sağlanmış bir anahtar kullanarak bir blobu oluşturduğunda veya güncelleştir, sonra o blob için sonraki okuma ve yazma isteklerinin de anahtarı sağlaması gerekir. Anahtar, zaten müşteri tarafından sağlanmış bir anahtarla şifrelenen bir blob isteği üzerinde sağlanmazsa, istek 409 (çakışma) hata koduyla başarısız olur.

İstemci uygulaması istekte bir şifreleme anahtarı gönderirse ve depolama hesabı Microsoft tarafından yönetilen bir anahtar veya müşteri tarafından yönetilen anahtar kullanılarak da şifrelenirse Azure Storage, şifreleme ve şifre çözme isteğinde belirtilen anahtarı kullanır.

Şifreleme anahtarını isteğin bir parçası olarak göndermek için, bir istemcinin HTTPS kullanarak Azure depolama ile güvenli bir bağlantı kurması gerekir.

Her blob anlık görüntüsünün kendi şifreleme anahtarı olabilir.

### <a name="request-headers-for-specifying-customer-provided-keys"></a>Müşteri tarafından sunulan anahtarları belirtmek için istek üstbilgileri

İstemciler, REST çağrıları için aşağıdaki üst bilgileri kullanarak blob depolamaya yönelik bir istek üzerindeki şifreleme anahtarı bilgilerini güvenli bir şekilde geçirebilir:

|İstek Başlığı | Açıklama |
|---------------|-------------|
|`x-ms-encryption-key` |Hem yazma hem de okuma istekleri için gereklidir. Base64 kodlamalı AES-256 şifreleme anahtarı değeri. |
|`x-ms-encryption-key-sha256`| Hem yazma hem de okuma istekleri için gereklidir. Şifreleme anahtarının Base64 ile kodlanmış SHA256. |
|`x-ms-encryption-algorithm` | Yazma istekleri için gereklidir, okuma istekleri için isteğe bağlıdır. Verilen anahtar kullanılarak verileri şifrelerken kullanılacak algoritmayı belirtir. AES256 olmalıdır. |

İstekte şifreleme anahtarlarının belirtilmesi isteğe bağlıdır. Ancak, bir yazma işlemi için yukarıda listelenen üst bilgilerden birini belirtirseniz, bunların tümünü belirtmeniz gerekir.

### <a name="blob-storage-operations-supporting-customer-provided-keys"></a>Müşteri tarafından sunulan anahtarları destekleyen BLOB depolama işlemleri

Aşağıdaki BLOB depolama işlemleri, bir istekte müşterinin sunduğu şifreleme anahtarlarını göndermeyi destekler:

- [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob)
- [Öbek listesini yerleştirme](/rest/api/storageservices/put-block-list)
- [Yerleştirme bloğu](/rest/api/storageservices/put-block)
- [Bloğu URL 'den koy](/rest/api/storageservices/put-block-from-url)
- [Yerleştirme sayfası](/rest/api/storageservices/put-page)
- [URL 'den sayfa koy](/rest/api/storageservices/put-page-from-url)
- [Ekleme bloğu](/rest/api/storageservices/append-block)
- [Blob özelliklerini ayarla](/rest/api/storageservices/set-blob-properties)
- [Blob meta verilerini ayarla](/rest/api/storageservices/set-blob-metadata)
- [Blob al](/rest/api/storageservices/get-blob)
- [BLOB özelliklerini Al](/rest/api/storageservices/get-blob-properties)
- [Blob meta verilerini al](/rest/api/storageservices/get-blob-metadata)
- [İkili Büyük Nesne Anlık Görüntüsünü Alma](/rest/api/storageservices/snapshot-blob)

### <a name="rotate-customer-provided-keys"></a>Müşteri tarafından sunulan anahtarları döndür

İstek üzerine geçirilen bir şifreleme anahtarını döndürmek için, blobu indirin ve yeni şifreleme anahtarıyla yeniden yükleyin.

> [!IMPORTANT]
> Azure portal, istekte sağlanmış bir anahtarla şifrelenen bir kapsayıcı veya Blobun okumak veya buradan yazmak için kullanılamaz.
>
> Azure Key Vault gibi güvenli bir anahtar deposunda blob depolamaya bir istekte sağladığınız şifreleme anahtarını koruduğunuzdan emin olun. Şifreleme anahtarı olmadan bir kapsayıcıda veya Blobun üzerinde yazma işlemi denerseniz, işlem başarısız olur ve nesneye erişiminizi kaybedersiniz.

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Azure depolama şifrelemesi ile disk şifrelemesi karşılaştırması

Azure depolama şifrelemesi, Azure sanal makine disklerini geri yükleyen sayfa bloblarını şifreler. Ayrıca, yerel geçici diskler dahil olmak üzere tüm Azure sanal makine diskleri, isteğe bağlı olarak [Azure disk şifrelemesi](../../security/azure-security-disk-encryption-overview.md)ile şifreli olabilir. Azure disk şifrelemesi, Azure Key Vault ile tümleştirilmiş işletim sistemi tabanlı şifreleme çözümleri sağlamak için Windows üzerinde sektör standardı [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) ve Linux üzerinde [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md)
- [Azure portalından Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-portal.md)
- [PowerShell'den Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLI'dan Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-cli.md)
