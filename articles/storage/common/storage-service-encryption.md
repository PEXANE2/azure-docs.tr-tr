---
title: Bekleyen veri için Azure Depolama şifrelemesi
description: Azure depolama, verilerinizi buluta kalıcı yapmadan önce otomatik olarak şifreleyerek korur. Depolama hesabınızdaki verilerin şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.date: 06/17/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 8b4236e40e8dfbe6ce67bca007be0b6737a6e0c8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84945588"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Bekleyen veri için Azure Depolama şifrelemesi

Azure depolama, verilerinizi buluta kalıcı hale geldiğinde otomatik olarak şifreler. Azure depolama şifrelemesi, verilerinizi korur ve kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

## <a name="about-azure-storage-encryption"></a>Azure depolama şifrelemesi hakkında

Azure depolama 'daki veriler, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi, Windows 'da BitLocker şifrelemeye benzer.

Azure depolama şifrelemesi, hem Kaynak Yöneticisi hem de klasik depolama hesapları da dahil olmak üzere tüm depolama hesapları için etkinleştirilmiştir. Azure depolama şifrelemesi devre dışı bırakılamıyor. Verileriniz varsayılan olarak güvenli hale getirildiğinden, Azure depolama şifrelemesi 'nin avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

Bir depolama hesabındaki veriler, performans katmanından (Standart veya Premium), erişim katmanından (sık veya seyrek erişimli) veya dağıtım modelinden (Azure Resource Manager veya klasik) bağımsız olarak şifrelenir. Arşiv katmanındaki tüm Bloblar da şifrelenir. Tüm Azure depolama artıklığı seçenekleri şifrelemeyi destekler ve coğrafi çoğaltma etkinleştirildiğinde hem birincil hem de ikincil bölgelerdeki tüm veriler şifrelenir. Blob 'lar, diskler, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm Azure depolama kaynakları şifrelenir. Tüm nesne meta verileri de şifrelenir. Azure depolama şifrelemesi için ek bir maliyet yoktur.

20 Ekim 2017 ' den sonra Azure depolama 'ya yazılan her Blok Blobu, ekleme Blobu veya Sayfa Blobu şifrelenir. Bu tarihten önce oluşturulan Bloblar, bir arka plan işlemi tarafından şifrelenmeye devam eder. 20 Ekim 2017 ' den önce oluşturulan bir Blobun şifrelemeye zorlamak için, blobu yeniden yazabilirsiniz. Bir Blobun şifreleme durumunu denetleme hakkında bilgi edinmek için bkz. [bir Blobun şifreleme durumunu denetleme](../blobs/storage-blob-encryption-status.md).

Azure depolama şifrelemesini temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz. [şifreleme API 'si: yeni nesil](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Yeni bir depolama hesabındaki veriler, Microsoft tarafından yönetilen anahtarlarla şifrelenir. Verilerinizin şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarlarınız ile şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz iki seçeneğiniz vardır:

- Blob depolamada ve Azure dosyalarında verileri şifrelemek ve şifrelerini çözmek için Azure Key Vault ile *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. <sup>1, 2</sup> müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi için bkz. [Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma](encryption-customer-managed-keys.md).
- BLOB depolama işlemlerinde, *müşteri tarafından sağlanmış bir anahtar* belirtebilirsiniz. Blob depolamaya karşı okuma veya yazma isteği yapan bir istemci, blob verilerinin şifrelenme ve şifresinin çözülmesi üzerinde ayrıntılı denetim isteğine yönelik bir şifreleme anahtarı içerebilir. Müşteri tarafından sunulan anahtarlar hakkında daha fazla bilgi için bkz. [BLOB depolama (Önizleme) isteği için bir şifreleme anahtarı sağlama](encryption-customer-provided-keys.md).

Aşağıdaki tabloda, Azure depolama şifrelemesi için anahtar yönetim seçenekleri karşılaştırılmaktadır.

|                                        |    Microsoft tarafından yönetilen anahtarlar                             |    Müşteri tarafından yönetilen anahtarlar                                                                                                                        |    Müşteri tarafından sunulan anahtarlar                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Şifreleme/şifre çözme işlemleri    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Desteklenen Azure depolama hizmetleri    |    Tümü                                                |    BLOB depolama, Azure dosyaları<sup>1, 2</sup>                                                                                                               |    Blob depolama                                                                  |
|    Anahtar depolama                         |    Microsoft anahtar deposu    |    Azure Key Vault                                                                                                                              |    Müşterinin kendi anahtar deposu                                                                 |
|    Anahtar döndürme sorumluluğu         |    Microsoft                                          |    Müşteri                                                                                                                                     |    Müşteri                                                                      |
|    Anahtar denetimi                          |    Microsoft                                     |    Müşteri                                                                                                                    |    Müşteri                                                                 |

<sup>1</sup> müşteri tarafından yönetilen anahtarları kuyruk depolama ile kullanmayı destekleyen bir hesap oluşturma hakkında bilgi için, bkz. [kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)oluşturma.<br />
<sup>2</sup> müşteri tarafından yönetilen anahtarları tablo depolama ile kullanmayı destekleyen bir hesap oluşturma hakkında bilgi için bkz. [tablolar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json).

Azure yönetilen diskler için şifreleme ve anahtar yönetimi hakkında daha fazla bilgi için bkz. Windows VM 'ler için [Azure yönetilen disklerin sunucu tarafı şifrelemesi](../../virtual-machines/windows/disk-encryption.md) veya Linux VM 'Leri için [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](../../virtual-machines/linux/disk-encryption.md) .

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası nedir?](../../key-vault/general/overview.md)
- [Azure portalından Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-portal.md)
- [PowerShell'den Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLI'dan Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-cli.md)
