---
title: Azure depolama 'ya erişim yetkisi verme | Microsoft Docs
description: Azure Active Directory, paylaşılan anahtar kimlik doğrulaması veya paylaşılan erişim imzaları dahil olmak üzere Azure depolama 'ya erişim yetkisi vermenin farklı yolları hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: a0717785f4f9c1c21a18d081d157a6cdc8c12f18
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371194"
---
# <a name="authorizing-access-to-azure-storage"></a>Azure depolama 'ya erişimi yetkilendirme

Depolama hesabınızdaki verilere her eriştiğinizde istemciniz, HTTP/HTTPS üzerinden Azure depolama 'ya bir istek yapar. Her güvenli kaynağa yönelik her istek yetkilendirilmelidir, böylece hizmet istemcinin verilere erişmek için gerekli izinlere sahip olmasını sağlar.

Aşağıdaki tabloda, Azure depolama 'nın kaynaklara erişimi yetkilendirmek için sunduğu seçenekler açıklanmaktadır:

|  |Paylaşılan anahtar (depolama hesabı anahtarı)  |Paylaşılan erişim imzası (SAS)  |Azure Active Directory (Azure AD)  |Anonim genel okuma erişimi  |
|---------|---------|---------|---------|---------|
|Azure Blob 'Ları     |[Destekleniyor](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Destekleniyor](storage-dotnet-shared-access-signature-part-1.md)         |[Destekleniyor](storage-auth-aad.md)         |[Destekleniyor](../blobs/storage-manage-access-to-resources.md)         |
|Azure dosyaları (SMB)     |[Destekleniyor](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |Desteklenmiyor         |[Yalnızca AAD etki alanı Hizmetleri ile desteklenir](../files/storage-files-active-directory-overview.md)         |Desteklenmiyor         |
|Azure dosyaları (REST)     |[Destekleniyor](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Destekleniyor](storage-dotnet-shared-access-signature-part-1.md)         |Desteklenmiyor         |Desteklenmiyor         |
|Azure Kuyrukları     |[Destekleniyor](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Destekleniyor](storage-dotnet-shared-access-signature-part-1.md)         |[Destekleniyor](storage-auth-aad.md)         |Desteklenmiyor         |
|Azure Tabloları     |[Destekleniyor](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Destekleniyor](storage-dotnet-shared-access-signature-part-1.md)         |Desteklenmiyor         |Desteklenmiyor         |

Her yetkilendirme seçeneği kısaca aşağıda açıklanmıştır:

- Blob 'lar ve kuyruklar için **Azure Active Directory (Azure AD) Tümleştirmesi** . Azure AD, bir istemcinin depolama hesabındaki kaynaklara erişimi üzerinde ayrıntılı denetim için rol tabanlı erişim denetimi (RBAC) sağlar. Blob 'lar ve kuyruklar için Azure AD tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure depolama 'ya Istekleri doğrulama](storage-auth-aad.md).

- Dosyalar için **Azure AD Domain Services (DS) Tümleştirmesi (Önizleme)** . Azure dosyaları, Azure AD DS aracılığıyla sunucu Ileti bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamasını destekler. Bu, bir istemcinin depolama hesabındaki kaynaklara erişimi üzerinde ayrıntılı denetim için RBAC sağlar. Etki alanı Hizmetleri kullanan dosyalarda Azure AD tümleştirmesi hakkında daha fazla bilgi için bkz. [SMB erişimi için Azure Active Directory etki alanı hizmeti (AAD DS) kimlik doğrulama desteği (Önizleme)](../files/storage-files-active-directory-overview.md).

- Bloblar, dosyalar, kuyruklar ve tablolar için **paylaşılan anahtar yetkilendirmesi** . Paylaşılan anahtar kullanan bir istemci, depolama hesabı erişim anahtarı kullanılarak imzalanan her isteği içeren bir üst bilgi geçirir. Daha fazla bilgi için bkz. [paylaşılan anahtarla yetkilendirme](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- Bloblar, dosyalar, kuyruklar ve tablolar için **paylaşılan erişim imzaları** . Paylaşılan erişim imzaları (SAS), bir depolama hesabındaki kaynaklara sınırlı temsilci erişimi sağlar. İmzanın geçerli olduğu zaman aralığına veya izin verdiği izinlere yönelik kısıtlamalar, erişimi yönetme konusunda esneklik sağlar. Daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- Kapsayıcılar ve BLOB 'lar için **anonim genel okuma erişimi** . Yetkilendirme gerekli değildir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).  

Varsayılan olarak, Azure Storage 'daki tüm kaynaklar güvenli hale getirilir ve yalnızca hesap sahibi tarafından kullanılabilir. İstemcilere Depolama hesabınızdaki kaynaklara erişim izni vermek için yukarıda özetlenen yetkilendirme stratejilerinden birini kullanabilseniz de, Microsoft en yüksek güvenlik ve kullanım kolaylığı için mümkün olduğunda Azure AD 'nin kullanılmasını önerir. 
