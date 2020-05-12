---
title: Azure Data Lake Storage 2. 'de bulunan BLOB depolama özellikleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özellikleri hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 05/10/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: ab704f53e608e7021f53a5d4b0a3e06c2ab807a1
ms.sourcegitcommit: 801a551e047e933e5e844ea4e735d044d170d99a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2020
ms.locfileid: "83007459"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz.

Bu tablo, Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özelliklerini listeler. Destek genişlemeye devam ettiğinden, Bu tablolarda görüntülenen öğeler zaman içinde değişir. Bir özelliğin önizleme durumuyla ilişkili belirli sorunlar hakkında daha fazla bilgi edinmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

> [!NOTE]
> Destek düzeyi yalnızca özelliğin Data Lake Storage 2. ile nasıl desteklendiğini gösterir. 
>
> Data Lake Storage 2. için [Premium performans bloğu BLOB depolama hesapları](storage-blob-create-account-block-blob.md) Şu anda genel önizlemededir. Destek-bu tür hesapların düzeyleri **Premium Blok Blob depolama hesapları** sütununda görüntülenir.

|BLOB depolama özelliği |Genel amaçlı v2 depolama hesapları|Premium blok blobu depolama hesapları |İlgili makaleler: |
|---------------|-------------------|---|
|Sık erişim katmanı|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Seyrek erişim katmanı|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Olaylar|Genel kullanıma sunuldu|Önizleme|[Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)|
|Ölçümler (klasik)|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Storage Analytics ölçümleri (klasik)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure İzleyicisi'nde ölçümler|Genel kullanıma sunuldu|Önizleme|[Azure İzleyici’de Azure Depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|BLOB depolama PowerShell komutları|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: PowerShell ile Blobları karşıya yükleme, indirme ve listeleme](storage-quickstart-blobs-powershell.md)|
|BLOB depolama Azure CLı komutları|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: Azure CLı ile Bloblar oluşturma, indirme ve listeleme](storage-quickstart-blobs-cli.md)|
|BLOB depolama API 'Leri|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: .NET için Azure Blob depolama istemci kitaplığı V12](storage-quickstart-blobs-dotnet.md)<br>[Hızlı başlangıç: Java V12 SDK ile Blobları yönetme](storage-quickstart-blobs-java.md)<br>[Hızlı başlangıç: Python V12 SDK ile Blobları yönetme](storage-quickstart-blobs-python.md)<br>[Hızlı başlangıç: node. js ' de JavaScript V12 SDK ile Blobları yönetme](storage-quickstart-blobs-nodejs.md)|
|Tanılama günlükleri|Genel kullanıma sunuldu|Önizleme <div role="complementary" aria-labelledby="diagnostic-logging"><sup>1</sup></div> |[Azure Depolama analizini günlüğe kaydetme](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Arşiv erişim katmanı|Önizleme|Desteklenmiyor|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Yaşam döngüsü yönetim ilkeleri|Önizleme|Henüz desteklenmiyor|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Akışı değiştirme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md)|
|Hesap yük devretmesi|Henüz desteklenmiyor|Henüz desteklenmiyor|[Olağanüstü durum kurtarma ve hesap yük devretme](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob kapsayıcı ACL 'SI|Henüz desteklenmiyor|Henüz desteklenmiyor|[Kapsayıcı ACL 'sini ayarla](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Özel etki alanları|Henüz desteklenmiyor|Henüz desteklenmiyor|[Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)|
|Sabit depolama|Henüz desteklenmiyor|Henüz desteklenmiyor|[Sabit depolamayla iş açısından kritik blob verilerini depolayın](storage-blob-immutable-storage.md)|
|Anlık Görüntüler|Henüz desteklenmiyor|Henüz desteklenmiyor|[.NET 'te blob anlık görüntüsü oluşturma ve yönetme](storage-blob-snapshots.md)|
|Geçici silme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Depolama blobları için geçici silme](storage-blob-soft-delete.md)|
|Statik web siteleri|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)|
|Azure Izleyici 'de günlüğe kaydetme|Henüz desteklenmiyor|Henüz desteklenmiyor|Henüz kullanılamıyor|

<div id="diagnostic-logging"><sup>1</sup> Premium Blok Blob depolama hesapları için, tanılama günlükleri (klasik) Azure portal kullanılarak etkinleştirilemez. PowerShell kullanarak bunları etkinleştirin.</div>

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)