---
title: Veriler için Azure Depolama şifrelemesi
description: Azure Depolama, verilerinizi bulutta kalıcı hale gelmeden önce otomatik olarak şifreleyerek korur. Depolama hesabınızdaki verilerin şifrelemesi için Microsoft tarafından yönetilen anahtarlara güvenebilir veya şifrelemeyi kendi anahtarlarınızla yönetebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.date: 04/10/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f112a4523bc5af9ecae57e93dfb90795d3fe9c50
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113269"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Veriler için Azure Depolama şifrelemesi

Azure Depolama, verilerinizi bulutta kalıcı hale geldiğinde otomatik olarak şifreler. Azure Depolama şifrelemesi verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur.

## <a name="about-azure-storage-encryption"></a>Azure Depolama şifrelemesi hakkında

Azure Depolama'daki veriler, mevcut en güçlü blok şifrelerinden biri olan 256 bit [AES şifrelemekullanılarak](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)şifrelenir ve şifresi saydam olarak çözülür ve FIPS 140-2 uyumludur. Azure Depolama şifrelemesi, Windows'daki BitLocker şifrelemesine benzer.

Azure Depolama şifrelemesi, hem Kaynak Yöneticisi hem de klasik depolama hesapları da dahil olmak üzere tüm depolama hesapları için etkinleştirilir. Azure Depolama şifrelemesi devre dışı tutulamaz. Verileriniz varsayılan olarak güvenli olduğundan, Azure Depolama şifrelemesinden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

Depolama hesabındaki veriler, performans katmanı (standart veya premium), erişim katmanı (sıcak veya havalı) veya dağıtım modeline (Azure Kaynak Yöneticisi veya klasik) bakılmaksızın şifrelenir. Arşiv katmanındaki tüm lekeler de şifrelenir. Tüm Azure Depolama artıklık seçenekleri şifrelemeyi destekler ve coğrafi çoğaltma etkinleştirildiğinde hem birincil hem de ikincil bölgelerdeki tüm veriler şifrelenir. Blobs, diskler, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm Azure Depolama kaynakları şifrelenir. Tüm nesne meta verileri de şifrelenir. Azure Depolama şifrelemesi için ek bir ücret yoktur.

20 Ekim 2017 tarihinden sonra Azure Depolama'ya yazılan her blok blob, ek blob veya sayfa blobşifre edilir. Bu tarihten önce oluşturulan blobs bir arka plan işlemi tarafından şifrelenmiş olmaya devam ediyor. 20 Ekim 2017'den önce oluşturulan bir lekenin şifrelemesini zorlamak için, blob'u yeniden yazabilirsiniz. Bir blob'un şifreleme durumunu nasıl denetleyeceklerini öğrenmek için [bkz.](../blobs/storage-blob-encryption-status.md)

Azure Depolama şifrelemesinin altında yatan şifreleme modülleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Yeni bir depolama hesabındaki veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Verilerinizin şifrelemesi için Microsoft tarafından yönetilen anahtarlara güvenebilir veya şifrelemeyi kendi anahtarlarınızla yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınızla yönetmeyi seçerseniz, iki seçeneğiniz var:

- Blob depolama ve Azure Dosyaları'ndaki verileri şifrelemek ve çözmek için kullanmak üzere Azure Key Vault ile *müşteri tarafından yönetilen* bir anahtar belirtebilirsiniz. <sup>1,2</sup> Müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi için Azure [Depolama şifrelemesini yönetmek için Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları kullan'a](encryption-customer-managed-keys.md)bakın.
- Blob depolama işlemlerinde *müşteri tarafından sağlanan* bir anahtar belirtebilirsiniz. Blob depolama sına karşı okuma veya yazma isteği nde bulunma isteğinde bulunduğu bir istemci, blob verilerinin nasıl şifrelendiği ve şifresinin çözülmesi üzerinde ayrıntılı denetim isteğine bir şifreleme anahtarı ekleyebilir. Müşteri tarafından sağlanan anahtarlar hakkında daha fazla bilgi için [bkz.](encryption-customer-provided-keys.md)

Aşağıdaki tablo, Azure Depolama şifrelemesi için anahtar yönetim seçeneklerini karşılaştırmaktadır.

|                                        |    Microsoft tarafından yönetilen anahtarlar                             |    Müşteri tarafından yönetilen anahtarlar                                                                                                                        |    Müşteri tarafından sağlanan anahtarlar                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Şifreleme/şifre çözme işlemleri    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Azure Depolama hizmetleri desteklendi    |    Tümü                                                |    Blob depolama, Azure Dosyaları<sup>1,2</sup>                                                                                                               |    Blob depolama                                                                  |
|    Anahtar depolama                         |    Microsoft anahtar mağazası    |    Azure Key Vault                                                                                                                              |    Azure Anahtar Kasası veya başka bir anahtar mağazası                                                                 |
|    Anahtar döndürme sorumluluğu         |    Microsoft                                          |    Müşteri                                                                                                                                     |    Müşteri                                                                      |
|    Anahtar kontrolü                          |    Microsoft                                     |    Müşteri                                                                                                                    |    Müşteri                                                                 |

<sup>1</sup> Sıra depolama alanıyla müşteri tarafından yönetilen anahtarları kullanmayı destekleyen bir hesap oluşturma hakkında bilgi [için](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)bkz.<br />
<sup>2</sup> Tablo depolama alanıyla müşteri tarafından yönetilen anahtarları kullanmayı destekleyen bir hesap oluşturma hakkında bilgi [için](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json)bkz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası nedir?](../../key-vault/key-vault-overview.md)
- [Azure portalından Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-portal.md)
- [PowerShell'den Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLI'dan Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](storage-encryption-keys-cli.md)
