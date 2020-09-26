---
title: BLOB depolama için şifreleme kapsamları (Önizleme)
description: Şifreleme kapsamları, kapsayıcının veya ayrı bir blob düzeyinde şifrelemeyi yönetme yeteneği sağlar. Aynı depolama hesabında bulunan ancak farklı müşterilere ait olan veriler arasında güvenli sınırlar oluşturmak için şifreleme kapsamlarını kullanabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.date: 09/22/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 6fb3c9b6dbbab036ddb00edd7e1d5980bb425ebe
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326125"
---
# <a name="encryption-scopes-for-blob-storage-preview"></a>BLOB depolama için şifreleme kapsamları (Önizleme)

Şifreleme kapsamları, kapsayıcının veya ayrı bir blob düzeyinde şifrelemeyi yönetme yeteneği sağlar. Aynı depolama hesabında bulunan ancak farklı müşterilere ait olan veriler arasında güvenli sınırlar oluşturmak için şifreleme kapsamlarını kullanabilirsiniz.

Varsayılan olarak, bir depolama hesabı tüm depolama hesabının kapsamına alınmış bir anahtarla şifrelenir. Bir şifreleme kapsamı ile, bir veya daha fazla kapsayıcının yalnızca bu kapsayıcılar kapsamındaki bir anahtarla şifrelenmesini belirtebilirsiniz.

Verilerinizi şifreleyen anahtara erişimi korumak ve denetlemek için, Microsoft tarafından yönetilen anahtarları veya Azure Key Vault depolanan müşteri tarafından yönetilen anahtarları kullanmayı seçebilirsiniz. Aynı depolama hesabındaki farklı şifreleme kapsamları, Microsoft tarafından yönetilen veya müşteri tarafından yönetilen anahtarları kullanabilir.

Bir şifreleme kapsamı oluşturduktan sonra, bir kapsayıcı veya blob oluşturmak için bu şifreleme kapsamının bir istekte belirtebilirsiniz. Şifreleme kapsamı oluşturma hakkında daha fazla bilgi için bkz. [şifreleme kapsamları oluşturma ve yönetme (Önizleme)](encryption-scope-manage.md).

> [!NOTE]
> Önizleme sırasında Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) veya Okuma Erişimli Coğrafi bölge-yedekli depolama (RA-GZRS) hesapları ile şifreleme kapsamları desteklenmez.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

> [!IMPORTANT]
> Şifreleme kapsamları önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA 'Lar) Şu anda kullanılamıyor.
>
> Beklenmeyen maliyetleri önlemek için, şu anda ihtiyaç duymayan tüm şifreleme kapsamlarını devre dışı bıraktığınızdan emin olun.

## <a name="create-a-container-or-blob-with-an-encryption-scope"></a>Şifreleme kapsamı ile bir kapsayıcı veya blob oluşturma

Bir şifreleme kapsamı altında oluşturulan Bloblar, bu kapsam için belirtilen anahtarla şifrelenir. Blob oluştururken tek bir blob için şifreleme kapsamı belirtebilir veya bir kapsayıcı oluştururken varsayılan bir şifreleme kapsamı belirtebilirsiniz. Bir kapsayıcı düzeyinde varsayılan bir şifreleme kapsamı belirtildiğinde, bu kapsayıcıdaki tüm Bloblar varsayılan kapsamla ilişkili anahtarla şifrelenir.

Varsayılan şifreleme kapsamına sahip bir kapsayıcıda bir blob oluşturduğunuzda, kapsayıcı varsayılan şifreleme kapsamının geçersiz kılmalara izin verecek şekilde yapılandırıldıysa varsayılan şifreleme kapsamını geçersiz kılan bir şifreleme kapsamı belirtebilirsiniz. Varsayılan şifreleme kapsamının geçersiz kılmalarını engellemek için kapsayıcıyı tek bir blob için geçersiz kılmaları reddedecek şekilde yapılandırın.

Şifreleme kapsamı devre dışı bırakılmadığı sürece, bir şifreleme kapsamına ait olan bir Blobun okuma işlemleri saydam bir şekilde gerçekleşir.

## <a name="disable-an-encryption-scope"></a>Şifreleme kapsamını devre dışı bırakma

Bir şifreleme kapsamını devre dışı bıraktığınızda, şifreleme kapsamıyla gerçekleştirilen sonraki okuma veya yazma işlemleri, HTTP hata kodu 403 (yasak) ile başarısız olur. Şifreleme kapsamını yeniden etkinleştirirseniz okuma ve yazma işlemleri normal olarak devam eder.

Bir şifreleme kapsamı devre dışı bırakıldığında, artık bu için faturalandırılırsınız. Gereksiz ücretlerden kaçınmak için gerekli olmayan tüm şifreleme kapsamlarını devre dışı bırakın.

Şifreleme kapsamınız müşteri tarafından yönetilen anahtarlarla korunuyorsa, şifreleme kapsamını devre dışı bırakmak için anahtar kasasında ilişkili anahtarı da silebilirsiniz. Müşteri tarafından yönetilen anahtarların, anahtar kasasındaki geçici silme ve Temizleme koruması ile korunduğunu ve silinen bir anahtarın bu özellikler tarafından tanımlanan davranışa tabi olduğunu aklınızda bulundurun. Daha fazla bilgi için Azure Key Vault belgelerinde aşağıdaki konulardan birine bakın:

- [PowerShell ile geçici silmeyi kullanma](../../key-vault/general/soft-delete-powershell.md)
- [CLı ile geçici silmeyi kullanma](../../key-vault/general/soft-delete-cli.md)

> [!NOTE]
> Bir şifreleme kapsamının silinmesi mümkün değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure Depolama şifrelemesi](../common/storage-service-encryption.md)
- [Şifreleme kapsamları oluşturma ve yönetme (Önizleme)](encryption-scope-manage.md)
- [Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarlar](../common/customer-managed-keys-overview.md)
- [Azure Key Vault nedir?](../../key-vault/general/overview.md)
