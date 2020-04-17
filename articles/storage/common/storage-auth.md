---
title: Veri işlemlerini yetkilendirme
titleSuffix: Azure Storage
description: Azure Etkin Dizini, Paylaşılan Anahtar yetkilendirmesi veya paylaşılan erişim imzaları (SAS) dahil olmak üzere Azure Depolama'ya erişimi yetkilendirmenin farklı yolları hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 8db6140789a9e4f46e368b913cacacd21609ccd9
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81529731"
---
# <a name="authorizing-access-to-data-in-azure-storage"></a>Azure Depolama'da verilere erişim yetkisi verme

Depolama hesabınızdaki verilere her erişinizde, istemciniz AZURE Depolama'ya HTTP/HTTPS üzerinden bir istekte bulunarak. Hizmetin, istemcinin verilere erişmek için gerekli izinlere sahip olduğundan emin olabilmesi için güvenli bir kaynağa gönderilen her istek yetkilendirilmelidir.

Aşağıdaki tabloda, Azure Depolama'nın kaynaklara erişim yetkisi vermek için sunduğu seçenekler açıklanmaktadır:

|  |Paylaşılan Anahtar (depolama hesabı anahtarı)  |Paylaşılan erişim imzası (SAS)  |Azure Active Directory (Azure AD)  |Şirket Içi Aktif Dizin Etki Alanı Hizmetleri (önizleme) |Anonim genel okuma erişimi  |
|---------|---------|---------|---------|---------|---------|
|Azure Blobları     |[Desteklenen](/rest/api/storageservices/authorize-with-shared-key/)         |[Desteklenen](storage-sas-overview.md)         |[Desteklenen](storage-auth-aad.md)         |Desteklenmiyor|[Desteklenen](../blobs/storage-manage-access-to-resources.md)         |
|Azure Dosyaları (SMB)     |[Desteklenen](/rest/api/storageservices/authorize-with-shared-key/)         |Desteklenmiyor         |[Desteklenen, sadece AAD Etki Alanı Hizmetleri ile](../files/storage-files-active-directory-overview.md)         |[Desteklenen kimlik bilgileri Azure AD ile eşitlenmelidir](../files/storage-files-active-directory-overview.md)|Desteklenmiyor         |
|Azure Dosyaları (REST)     |[Desteklenen](/rest/api/storageservices/authorize-with-shared-key/)         |[Desteklenen](storage-sas-overview.md)         |Desteklenmiyor         |Desteklenmiyor |Desteklenmiyor         |
|Azure Kuyrukları     |[Desteklenen](/rest/api/storageservices/authorize-with-shared-key/)         |[Desteklenen](storage-sas-overview.md)         |[Desteklenen](storage-auth-aad.md)         |Desteklenmiyor | Desteklenmiyor         |
|Azure Tabloları     |[Desteklenen](/rest/api/storageservices/authorize-with-shared-key/)         |[Desteklenen](storage-sas-overview.md)         |Desteklenmiyor         |Desteklenmiyor| Desteklenmiyor         |

Her yetkilendirme seçeneği aşağıda kısaca açıklanmıştır:

- Blobs ve kuyruklar için **Azure Etkin Dizin (Azure AD) tümleştirmesi.** Azure AD, bir istemcinin depolama hesabındaki kaynaklara erişimini denetlemek için rol tabanlı erişim denetimi (RBAC) sağlar. Blobs ve kuyruklar için Azure AD tümleştirmesi ile ilgili daha fazla bilgi için [bkz.](storage-auth-aad.md)

- Azure Dosyaları için **Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) kimlik doğrulaması.** Azure Dosyaları, Azure AD DS aracılığıyla Sunucu İleti Bloğu (SMB) üzerinden kimlik tabanlı yetkilendirmeyi destekler. RBAC'ı, bir istemcinin depolama hesabındaki Azure Dosyaları kaynaklarına erişimi üzerinde ince gelişmiş denetim için kullanabilirsiniz. Etki alanı hizmetlerini kullanarak Azure Dosyaları kimlik doğrulaması ile ilgili daha fazla bilgi için [genel bakışa](../files/storage-files-active-directory-overview.md)bakın.

- Azure Dosyaları için **şirket içi Active Directory Domain Services (AD DS veya şirket içi AD DS) kimlik doğrulaması (önizleme).** Azure Files, AD DS aracılığıyla SMB üzerinden kimlik tabanlı yetkilendirmeyi destekler. AD DS ortamınız şirket içi makinelerde veya Azure VM'lerinde barındırılabilir. Dosyalara SMB erişimi, şirket içinde veya Azure'da birleştirilmiş etki alanından AD DS kimlik bilgileri kullanılarak desteklenir. Hisse düzeyi erişim denetimi için RBAC ve dizin/dosya düzeyi izin zorlama için NTFS DACLs bir arada kullanabilirsiniz. Etki alanı hizmetlerini kullanarak Azure Dosyaları kimlik doğrulaması ile ilgili daha fazla bilgi için [genel bakışa](../files/storage-files-active-directory-overview.md)bakın.

- Blobs, dosyalar, kuyruklar ve tablolar için **Paylaşılan Anahtar yetkilendirmesi.** Paylaşılan Anahtar'ı kullanan bir istemci, depolama hesabı erişim anahtarı kullanılarak imzalanan her istekle bir üstbilgiden geçer. Daha fazla bilgi için Bkz. [Paylaşılan Anahtarla Yetkilendirme](/rest/api/storageservices/authorize-with-shared-key/).
- Blobs, dosyalar, kuyruklar ve tablolar için **paylaşılan erişim imzaları.** Paylaşılan erişim imzaları (SAS), depolama hesabındaki kaynaklara sınırlı temsilci erişimi sağlar. İmzanın geçerli olduğu zaman aralığına veya verdiği izinlere kısıtlamalar eklemek, erişimi yönetmede esneklik sağlar. Daha fazla bilgi için bkz. [paylaşılan erişim imzalarını (SAS) kullanma.](storage-sas-overview.md)
- Anonim kamu konteyner ler ve lekeler için **erişim okuyun.** Yetkilendirme gerekli değildir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).  

Varsayılan olarak, Azure Depolama'daki tüm kaynaklar güvenlidir ve yalnızca hesap sahibi tarafından kullanılabilir. Müşterilere depolama hesabınızdaki kaynaklara erişim izni vermek için yukarıda özetlenen yetkilendirme stratejilerinden herhangi birini kullanabilirsiniz, ancak Microsoft maksimum güvenlik ve kullanım kolaylığı için mümkün olduğunda Azure AD'yi kullanmanızı önerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin'i kullanarak Azure bloblarına ve kuyruklarına erişimi yetkilendirme](storage-auth-aad.md)
- [Paylaşılan Anahtar ile yetkilendirme](/rest/api/storageservices/authorize-with-shared-key/)
- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni verme](storage-sas-overview.md)
