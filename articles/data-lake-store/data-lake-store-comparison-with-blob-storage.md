---
title: BLOB depolama ile Azure Data Lake Storage 1. karşılaştırması
description: Azure Data Lake Storage 1. ile Azure Blob depolama arasındaki farkların özetini sağlar.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: 3565af1f3e0a002cf3852faef9c2853ff88d46e9
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691734"
---
# <a name="comparing-azure-data-lake-storage-gen1-and-azure-blob-storage"></a>Azure Data Lake Storage 1. ve Azure Blob depolamayı karşılaştırma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)] 

Bu makaledeki tabloda, büyük veri işlemenin bazı önemli yönleri üzerinde Azure Data Lake Storage 1. ve Azure Blob depolama arasındaki farklar özetlenmektedir. Azure Blob depolama, çok çeşitli depolama senaryoları için tasarlanan genel amaçlı, ölçeklenebilir bir nesne deposudur. Azure Data Lake Storage 1., büyük veri analizi iş yükleri için iyileştirilmiş bir hiper ölçekli depodur.

|  | Azure Data Lake Storage Gen1 | Azure Blob Depolama |
| --- | --- | --- |
| Amaç |Büyük veri analizi iş yükleri için iyileştirilmiş depolama |Büyük veri analizi dahil olmak üzere çok çeşitli depolama senaryoları için genel amaçlı nesne deposu |
| Kullanım örnekleri |Batch, etkileşimli, Akış Analizi ve günlük dosyaları, IoT verileri gibi makine öğrenimi verileri, akışlar, büyük veri kümeleri |Uygulama arka ucu, yedekleme verileri, akış ve genel amaçlı veriler için medya depolaması gibi herhangi bir metin veya ikili veri türü. Ayrıca, analiz iş yükleri için tam destek; Batch, etkileşimli, Akış Analizi ve günlük dosyaları, IoT verileri gibi makine öğrenimi verileri, akışlar, büyük veri kümeleri |
| Önemli Kavramlar |Data Lake Storage 1. hesap, dosya olarak depolanan verileri de içeren klasörler içerir |Depolama hesabında kapsayıcılar vardır ve bu da blob biçiminde veri içerir |
| Yapı |Hiyerarşik dosya sistemi |Düz ad alanı olan nesne deposu |
| API |HTTPS üzerinden REST API |HTTP/HTTPS üzerinden REST API |
| Sunucu tarafı API 'SI |[Web, ile uyumlu REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob depolama REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop dosya sistemi Istemcisi |Yes |Yes |
| Veri Işlemleri-kimlik doğrulama |[Azure Active Directory kimliklerine](../active-directory/develop/authentication-scenarios.md) göre |Paylaşılan gizli dizi- [hesap erişim anahtarlarına](../storage/common/storage-account-keys-manage.md) ve [paylaşılan erişim imzası anahtarlarına](../storage/common/storage-dotnet-shared-access-signature-part-1.md)göre. |
| Veri Işlemleri-kimlik doğrulama protokolü |OAuth 2,0. Çağrılar, Azure Active Directory tarafından verilen geçerli bir JWT (JSON Web Token) içermelidir |Karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC). Çağrılar HTTP isteğinin bir parçası üzerinde Base64 kodlamalı bir SHA-256 karması içermelidir. |
| Veri Işlemleri-yetkilendirme |POSIX Access Control listeleri (ACL 'Ler).  Azure Active Directory kimlikleri temel alan ACL 'Ler dosya ve klasör düzeyinde ayarlanabilir. |Hesap düzeyinde yetkilendirme için – [hesap erişim anahtarlarını](../storage/common/storage-account-keys-manage.md) kullanın<br>Hesap, kapsayıcı veya blob yetkilendirmesi için- [paylaşılan erişim Imza anahtarlarını](../storage/common/storage-dotnet-shared-access-signature-part-1.md) kullanın |
| Veri Işlemleri-denetim |Kullanılabileceğini. Bilgi için [buraya](data-lake-store-diagnostic-logs.md) bakın. |Kullanılabilir |
| Bekleyen şifreleme verileri |<ul><li>Saydam, sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Azure Anahtar Kasası 'nda müşteri tarafından yönetilen anahtarlarla</li></ul></ul> |<ul><li>Saydam, sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Azure Anahtar Kasası 'nda müşteri tarafından yönetilen anahtarlarla (Önizleme)</li></ul><li>İstemci Tarafında Şifreleme</li></ul> |
| Yönetim işlemleri (örneğin, hesap oluşturma) |Hesap yönetimi için Azure tarafından sunulan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC) |Hesap yönetimi için Azure tarafından sunulan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC) |
| Geliştirici SDK 'Ları |.NET, Java, Python, Node. js |.NET, Java, Python, Node. js, C++, Ruby, PHP, Go, Android, iOS |
| Analiz Iş yükü performansı |Paralel analiz iş yükleri için iyileştirilmiş performans. Yüksek aktarım hızı ve ıOPS. |Paralel analiz iş yükleri için iyileştirilmiş performans. |
| Boyut sınırları |Hesap boyutları, dosya boyutları veya dosya sayısı sınırı yoktur |Belirli sınırlar için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](../storage/common/scalability-targets-standard-account.md) , [BLOB depolama için ölçeklenebilirlik ve performans hedefleri](../storage/blobs/scalability-targets.md). [Azure desteğiyle](https://azure.microsoft.com/support/faq/) iletişim kurarak daha büyük hesap limitleri kullanılabilir |
| Coğrafi yedeklilik |Yerel olarak yedekli (bir Azure bölgesindeki verilerin birden fazla kopyası) |Yerel olarak yedekli (LRS), bölgesel olarak yedekli (ZRS), küresel olarak yedekli (GRS), okuma erişimli küresel olarak yedekli (RA-GRS). Daha fazla bilgi için [buraya](../storage/common/storage-redundancy.md) bakın |
| Hizmet durumu |Genel kullanıma sunuldu |Genel kullanıma sunuldu |
| Bölgesel kullanılabilirlik |[Buraya](https://azure.microsoft.com/regions/#services) bakın |Tüm Azure bölgelerinde kullanılabilir |
| Fiyat |[Fiyatlandırmaya](https://azure.microsoft.com/pricing/details/data-lake-store/) bakın |[Fiyatlandırmaya](https://azure.microsoft.com/pricing/details/storage/) bakın |


