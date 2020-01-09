---
title: Azure Depolama Blobu karşılaştırma Azure Data Lake Storage 1. | Microsoft Docs
description: Azure Depolama Blobu karşılaştırma Azure Data Lake Storage 1.
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
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438714"
---
# <a name="comparing-azure-data-lake-storage-gen1-and-azure-blob-storage"></a>Azure Data Lake Storage 1. ve Azure Blob depolamayı karşılaştırma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)] 

Bu makaledeki tabloda, büyük veri işlemenin bazı önemli yönleri üzerinde Azure Data Lake Storage 1. ve Azure Blob depolama arasındaki farklar özetlenmektedir. Azure Blob depolama, çok çeşitli depolama senaryoları için tasarlanan genel amaçlı, ölçeklenebilir bir nesne deposudur. Azure Data Lake Storage 1., büyük veri analizi iş yükleri için iyileştirilmiş bir hiper ölçekli depodur.

|  | Azure Data Lake Storage Gen1 | Azure Blob Depolama |
| --- | --- | --- |
| Amaç |Büyük veri analizi iş yükleri için iyileştirilmiş depolama |Büyük veri analizi dahil olmak üzere çok çeşitli depolama senaryoları için genel amaçlı nesne deposu |
| Kullanım Örnekleri |Batch, etkileşimli, Akış Analizi ve günlük dosyaları, IoT verileri gibi makine öğrenimi verileri, akışlar, büyük veri kümeleri |Uygulama arka ucu, yedekleme verileri, akış ve genel amaçlı veriler için medya depolaması gibi herhangi bir metin veya ikili veri türü. Ayrıca, analiz iş yükleri için tam destek; Batch, etkileşimli, Akış Analizi ve günlük dosyaları, IoT verileri gibi makine öğrenimi verileri, akışlar, büyük veri kümeleri |
| Önemli Kavramlar |Data Lake Storage 1. hesap, dosya olarak depolanan verileri de içeren klasörler içerir |Depolama hesabında kapsayıcılar vardır ve bu da blob biçiminde veri içerir |
| Yapı |Hiyerarşik dosya sistemi |Düz ad alanı olan nesne deposu |
| eklentisi |HTTPS üzerinden REST API |HTTP/HTTPS üzerinden REST API |
| Sunucu tarafı API'si |[WebHDFS ile uyumlu REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob depolama REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop dosya sistemi istemci |Evet |Evet |
| Veri Işlemleri-kimlik doğrulama |[Azure Active Directory kimliklerine](../active-directory/develop/authentication-scenarios.md) göre |Paylaşılan gizli dizi- [hesap erişim anahtarlarına](../storage/common/storage-account-keys-manage.md) ve [paylaşılan erişim imzası anahtarlarına](../storage/common/storage-dotnet-shared-access-signature-part-1.md)göre. |
| Veri Işlemleri-kimlik doğrulama protokolü |OAuth 2,0. Çağrılar, Azure Active Directory tarafından verilen geçerli bir JWT (JSON Web Token) içermelidir |Karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC). Çağrılar HTTP isteğinin bir parçası üzerinde Base64 kodlamalı bir SHA-256 karması içermelidir. |
| Veri Işlemleri-yetkilendirme |POSIX Access Control listeleri (ACL 'Ler).  Azure Active Directory kimlikleri temel alan ACL 'Ler dosya ve klasör düzeyinde ayarlanabilir. |Hesap düzeyinde yetkilendirme için – [hesap erişim anahtarlarını](../storage/common/storage-account-keys-manage.md) kullanın<br>Hesap, kapsayıcı veya blob yetkilendirmesi için- [paylaşılan erişim Imza anahtarlarını](../storage/common/storage-dotnet-shared-access-signature-part-1.md) kullanın |
| Veri Işlemleri-denetim |Kullanılabileceğini. Bilgi için [buraya](data-lake-store-diagnostic-logs.md) bakın. |Kullanılabilir |
| Bekleyen verileri şifreleme |<ul><li>Saydam, sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Azure Anahtar Kasası 'nda müşteri tarafından yönetilen anahtarlarla</li></ul></ul> |<ul><li>Saydam, sunucu tarafı</li> <ul><li>Hizmet tarafından yönetilen anahtarlarla</li><li>Azure Anahtar Kasası 'nda müşteri tarafından yönetilen anahtarlarla (Önizleme)</li></ul><li>İstemci Tarafında Şifreleme</li></ul> |
| Yönetim işlemlerini (örneğin hesap oluştur) |[Rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC), hesap yönetimi için Azure tarafından sağlanan |[Rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC), hesap yönetimi için Azure tarafından sağlanan |
| Geliştirici SDK'ları |.NET, Java, Python, Node.js |.Net, Java, Python, Node. js, C++, Ruby, php, Go, Android, iOS |
| Analiz Iş yükü performansı |Paralel analiz iş yükleri için iyileştirilmiş performans. Yüksek aktarım hızı ve IOPS. |Paralel analiz iş yükleri için iyileştirilmiş performans. |
| Boyutu sınırları |Hesap boyutları, dosya boyutları veya dosya sayısı üst sınırı yoktur |Belirli sınırlar için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](../storage/common/scalability-targets-standard-account.md) , [BLOB depolama için ölçeklenebilirlik ve performans hedefleri](../storage/blobs/scalability-targets.md). [Azure desteğiyle](https://azure.microsoft.com/support/faq/) iletişim kurarak daha büyük hesap limitleri kullanılabilir |
| Coğrafi yedeklilik |Yerel olarak yedekli (bir Azure bölgesindeki verilerin birden çok kopyasını) |Yerel olarak yedekli (LRS), bölgesel olarak yedekli (ZRS), küresel olarak yedekli (GRS), okuma erişimli küresel olarak yedekli (RA-GRS). Daha fazla bilgi için [buraya](../storage/common/storage-redundancy.md) bakın |
| Hizmet durumu |Genel kullanıma sunuldu |Genel kullanıma sunuldu |
| Bölgesel kullanılabilirlik |Bkz: [burada](https://azure.microsoft.com/regions/#services) |Tüm Azure bölgelerinde kullanılabilir |
| Fiyat |Bkz: [fiyatlandırması](https://azure.microsoft.com/pricing/details/data-lake-store/) |Bkz: [fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) |


