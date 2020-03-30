---
title: Azure Depolama Blob ile Azure Veri Gölü Depolama Gen1 karşılaştırması | Microsoft Dokümanlar
description: Azure Depolama Blob ile Azure Veri Gölü Depolama Gen1 karşılaştırması
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: 7c958c3ed4d6ddaabd87f053005fcfc1eba8c842
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75438714"
---
# <a name="comparing-azure-data-lake-storage-gen1-and-azure-blob-storage"></a>Azure Veri Gölü Depolama Gen1 ve Azure Blob Depolama karşılaştırma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)] 

Bu makaledeki tablo, Azure Veri Gölü Depolama Gen1 ve Azure Blob Depolama arasındaki farkları büyük veri işlemenin bazı önemli yönleri yle özetler. Azure Blob Depolama, çok çeşitli depolama senaryoları için tasarlanmış genel amaçlı, ölçeklenebilir bir nesne deposudur. Azure Veri Gölü Depolama Gen1, büyük veri analitiği iş yükleri için optimize edilmiş hiper ölçekli bir depodur.

|  | Azure Data Lake Storage Gen1 | Azure Blob Depolama |
| --- | --- | --- |
| Amaç |Büyük veri analitiği iş yükleri için optimize edilmiş depolama |Büyük veri analitiği de dahil olmak üzere çok çeşitli depolama senaryoları için genel amaçlı nesne depolama |
| Kullanım Örnekleri |Günlük dosyaları, IoT verileri, tıklama akışları, büyük veri kümeleri gibi toplu iş, etkileşimli, akışanalizi ve makine öğrenimi verileri |Uygulama arka uç, yedekleme verileri, akış için ortam depolama ve genel amaçlı veriler gibi her türlü metin veya ikili veri türü. Ayrıca, analitik iş yükleri için tam destek; toplu iş, etkileşimli, akışanalizi ve günlük dosyaları, IoT verileri, tıklama akışları, büyük veri kümeleri gibi makine öğrenimi verileri |
| Önemli Kavramlar |Data Lake Storage Gen1 hesabı, dosya olarak depolanan verileri içeren klasörler içerir |Depolama hesabı sırayla blobs şeklinde veri vardır kapsayıcılar vardır |
| Yapı |Hiyerarşik dosya sistemi |Düz ad alanı olan nesne deposu |
| API |HTTPS üzerinde REST API |HTTP/HTTPS üzerinde REST API |
| Sunucu tarafı API'si |[WebHDFS uyumlu REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Depolama REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop Dosya Sistemi İstemci |Evet |Evet |
| Veri İşlemleri - Kimlik Doğrulama |Azure [Etkin Dizin Kimliklerine](../active-directory/develop/authentication-scenarios.md) Dayalı |Paylaşılan sırlara dayalı - [Hesap Erişim Anahtarları](../storage/common/storage-account-keys-manage.md) ve Paylaşılan Erişim İmza [Anahtarları](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Veri İşlemleri - Kimlik Doğrulama Protokolü |OAuth 2.0. Aramalar, Azure Active Directory tarafından verilen geçerli bir JWT (JSON Web Belirteci) içermelidir |Karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC) . Aramalar, HTTP isteğinin bir bölümü üzerinde Base64 kodlanmış BIR SHA-256 karma içermelidir. |
| Veri İşlemleri - Yetkilendirme |POSIX Erişim Kontrol Listeleri (AçS).  Azure Etkin Dizin Kimliklerine dayalı ALA'lar dosya ve klasör düzeyinde ayarlanabilir. |Hesap düzeyinde yetkilendirme için – [Hesap Erişim Anahtarlarını](../storage/common/storage-account-keys-manage.md) Kullan<br>Hesap, kapsayıcı veya blob yetkilendirmesi için - [Paylaşılan Erişim İmza Anahtarlarını](../storage/common/storage-dotnet-shared-access-signature-part-1.md) Kullanma |
| Veri İşlemleri - Denetim |Kullanılabilir. Bilgi için [buraya](data-lake-store-diagnostic-logs.md) bakın. |Kullanılabilir |
| Şifreleme verileri istirahatte |<ul><li>Saydam, Sunucu tarafı</li> <ul><li>Servis tarafından yönetilen anahtarlarla</li><li>Azure KeyVault'ta müşteri tarafından yönetilen anahtarlarla</li></ul></ul> |<ul><li>Saydam, Sunucu tarafı</li> <ul><li>Servis tarafından yönetilen anahtarlarla</li><li>Azure KeyVault'ta müşteri tarafından yönetilen anahtarlarla (önizleme)</li></ul><li>İstemci Tarafında Şifreleme</li></ul> |
| Yönetim işlemleri (örn. Hesap Oluşturma) |Azure tarafından hesap yönetimi için sağlanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC) |Azure tarafından hesap yönetimi için sağlanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC) |
| Geliştirici SDK'ları |.NET, Java, Python, Düğüm.js |.Net, Java, Python, Düğüm.js, C++, Ruby, PHP, Go, Android, iOS |
| Analitik İş Yükü Performansı |Paralel analitik iş yükleri için optimize edilmiş performans. Yüksek İş İbılavet ve IOPS. |Paralel analitik iş yükleri için optimize edilmiş performans. |
| Boyut sınırları |Hesap boyutları, dosya boyutları veya dosya sayısı nda sınır yok |Belirli sınırlar [için, standart depolama hesapları için Ölçeklenebilirlik hedefleri](../storage/common/scalability-targets-standard-account.md) ve [Blob depolama için ölçeklenebilirlik ve performans hedeflerine](../storage/blobs/scalability-targets.md)bakın. [Azure Desteği'ne](https://azure.microsoft.com/support/faq/) başvurarak kullanılabilen daha büyük hesap sınırları |
| Coğrafi artıklık |Yerel olarak yedekli (bir Azure bölgesinde birden çok veri kopyası) |Yerel olarak yedekli (LRS), bölge yedekli (ZRS), küresel gereksiz (GRS), okuma-erişim küresel gereksiz (RA-GRS). Daha fazla bilgi için [buraya](../storage/common/storage-redundancy.md) bakın |
| Hizmet durumu |Genel kullanıma sunuldu |Genel kullanıma sunuldu |
| Bölgesel kullanılabilirlik |[Buraya](https://azure.microsoft.com/regions/#services) bakın |Tüm Azure bölgelerinde kullanılabilir |
| Fiyat |[Bkz. Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-lake-store/) |[Bkz. Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/) |


