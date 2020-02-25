---
title: Veri işlemlerini yetkilendirme
titleSuffix: Azure Storage
description: Azure Active Directory, paylaşılan anahtar yetkilendirmesi veya paylaşılan erişim imzaları (SAS) dahil olmak üzere Azure depolama 'ya erişim yetkisi vermenin farklı yolları hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 7deb773fae6ba56b6f601983ffd2b07d887e1480
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565769"
---
# <a name="authorizing-access-to-data-in-azure-storage"></a>Azure depolama 'daki verilere erişimi yetkilendirme

Depolama hesabınızdaki verilere her eriştiğinizde istemciniz, HTTP/HTTPS üzerinden Azure depolama 'ya bir istek yapar. Her güvenli kaynağa yönelik her istek yetkilendirilmelidir, böylece hizmet istemcinin verilere erişmek için gerekli izinlere sahip olmasını sağlar.

Aşağıdaki tabloda, Azure depolama 'nın kaynaklara erişimi yetkilendirmek için sunduğu seçenekler açıklanmaktadır:

|  |Paylaşılan anahtar (depolama hesabı anahtarı)  |Paylaşılan erişim imzası (SAS)  |Azure Active Directory (Azure AD)  |Active Directory (Önizleme)|Anonim genel okuma erişimi  |
|---------|---------|---------|---------|---------|---------|
|Azure Blobları     |[Destekleniyor](/rest/api/storageservices/authorize-with-shared-key/)         |[Destekleniyor](storage-sas-overview.md)         |[Destekleniyor](storage-auth-aad.md)         |Desteklenmiyor|[Destekleniyor](../blobs/storage-manage-access-to-resources.md)         |
|Azure dosyaları (SMB)     |[Destekleniyor](/rest/api/storageservices/authorize-with-shared-key/)         |Desteklenmiyor         |[Yalnızca AAD etki alanı Hizmetleri ile desteklenir](../files/storage-files-active-directory-overview.md)         |[Desteklenir, kimlik bilgilerinin Azure AD ile eşitlenmesi gerekir](../files/storage-files-active-directory-overview.md)|Desteklenmiyor         |
|Azure dosyaları (REST)     |[Destekleniyor](/rest/api/storageservices/authorize-with-shared-key/)         |[Destekleniyor](storage-sas-overview.md)         |Desteklenmiyor         |Desteklenmiyor |Desteklenmiyor         |
|Azure Kuyrukları     |[Destekleniyor](/rest/api/storageservices/authorize-with-shared-key/)         |[Destekleniyor](storage-sas-overview.md)         |[Destekleniyor](storage-auth-aad.md)         |Desteklenmiyor | Desteklenmiyor         |
|Azure Tabloları     |[Destekleniyor](/rest/api/storageservices/authorize-with-shared-key/)         |[Destekleniyor](storage-sas-overview.md)         |Desteklenmiyor         |Desteklenmiyor| Desteklenmiyor         |

Her yetkilendirme seçeneği kısaca aşağıda açıklanmıştır:

- Blob 'lar ve kuyruklar için **Azure Active Directory (Azure AD) Tümleştirmesi** . Azure AD, bir istemcinin depolama hesabındaki kaynaklara erişimi üzerinde ayrıntılı denetim için rol tabanlı erişim denetimi (RBAC) sağlar. Blob 'lar ve kuyruklar için Azure AD tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](storage-auth-aad.md).

- Dosyalar için **Azure Active Directory Domain Services (Azure AD DS) kimlik doğrulaması** . Azure dosyaları, Azure AD DS aracılığıyla sunucu Ileti bloğu (SMB) üzerinden kimlik tabanlı yetkilendirmeyi destekler. Bir depolama hesabındaki Azure dosya kaynaklarına yönelik bir istemcinin erişimi üzerinde ayrıntılı denetim için RBAC kullanabilirsiniz. Etki alanı Hizmetleri 'ni kullanarak Azure dosyaları kimlik doğrulaması hakkında daha fazla bilgi için bkz. [genel bakış](../files/storage-files-active-directory-overview.md).

- Dosyalar için **Active Directory (ad) kimlik doğrulaması (Önizleme)** . Azure dosyaları, AD aracılığıyla SMB üzerinden kimlik tabanlı yetkilendirmeyi destekler. AD etki alanı hizmetiniz, şirket içi makinelerde veya Azure VM 'lerinde barındırılabilir. Dosyalara SMB erişimi, şirket içinde ya da Azure 'da etki alanına katılmış makinelerden AD kimlik bilgileri kullanılarak desteklenir. Dizin/dosya düzeyi izin zorlaması için RBAC 'yi, paylaşma düzeyinde erişim denetimi ve NTFS DACL 'Leri için kullanabilirsiniz. Etki alanı Hizmetleri 'ni kullanarak Azure dosyaları kimlik doğrulaması hakkında daha fazla bilgi için bkz. [genel bakış](../files/storage-files-active-directory-overview.md).

- Bloblar, dosyalar, kuyruklar ve tablolar için **paylaşılan anahtar yetkilendirmesi** . Paylaşılan anahtar kullanan bir istemci, depolama hesabı erişim anahtarı kullanılarak imzalanan her isteği içeren bir üst bilgi geçirir. Daha fazla bilgi için bkz. [paylaşılan anahtarla yetkilendirme](/rest/api/storageservices/authorize-with-shared-key/).
- Bloblar, dosyalar, kuyruklar ve tablolar için **paylaşılan erişim imzaları** . Paylaşılan erişim imzaları (SAS), bir depolama hesabındaki kaynaklara sınırlı temsilci erişimi sağlar. İmzanın geçerli olduğu zaman aralığına veya izin verdiği izinlere yönelik kısıtlamalar, erişimi yönetme konusunda esneklik sağlar. Daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](storage-sas-overview.md).
- Kapsayıcılar ve BLOB 'lar için **anonim genel okuma erişimi** . Yetkilendirme gerekli değildir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).  

Varsayılan olarak, Azure Storage 'daki tüm kaynaklar güvenli hale getirilir ve yalnızca hesap sahibi tarafından kullanılabilir. İstemcilere Depolama hesabınızdaki kaynaklara erişim izni vermek için yukarıda özetlenen yetkilendirme stratejilerinden birini kullanabilseniz de, Microsoft en yüksek güvenlik ve kullanım kolaylığı için mümkün olduğunda Azure AD 'nin kullanılmasını önerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](storage-auth-aad.md)
- [Paylaşılan anahtarla yetkilendir](/rest/api/storageservices/authorize-with-shared-key/)
- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](storage-sas-overview.md)
