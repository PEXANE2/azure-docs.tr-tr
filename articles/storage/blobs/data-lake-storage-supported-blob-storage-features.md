---
title: Azure Data Lake Storage 2. 'de bulunan BLOB depolama özellikleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özellikleri hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 215244204aa58cc2fdedc639d48e01b514759694
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78196017"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz.

Bu tablo, Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özelliklerini listeler. Destek genişlemeye devam ettiğinden, Bu tablolarda görüntülenen öğeler zaman içinde değişir.

## <a name="supported-blob-storage-features"></a>Desteklenen BLOB depolama özellikleri

> [!NOTE]
> Destek düzeyi yalnızca özelliğin Data Lake Storage 2. ile nasıl desteklendiğini gösterir.

|BLOB depolama özelliği |Destek düzeyi |İlgili makaleler: |
|---------------|-------------------|---|
|Sık erişim katmanı|Genel kullanıma sunuldu|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Seyrek erişim katmanı|Genel kullanıma sunuldu|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Olaylar|Genel kullanıma sunuldu|[Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)|
|Ölçümler (klasik)|Genel kullanıma sunuldu|[Azure Storage Analytics ölçümleri (klasik)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure İzleyicisi'nde ölçümler|Genel kullanıma sunuldu|[Azure İzleyici'de Azure Depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|BLOB depolama PowerShell komutları|Genel kullanıma sunuldu|[Hızlı başlangıç: PowerShell ile Blobları karşıya yükleme, indirme ve listeleme](storage-quickstart-blobs-powershell.md)|
|BLOB depolama Azure CLı komutları|Genel kullanıma sunuldu|[Hızlı başlangıç: Azure CLı ile Bloblar oluşturma, indirme ve listeleme](storage-quickstart-blobs-cli.md)|
|BLOB Depolama API'leri|Genel kullanıma sunuldu|[Hızlı başlangıç: .NET için Azure Blob depolama istemci kitaplığı V12](storage-quickstart-blobs-dotnet.md)<br>[Hızlı başlangıç: Java V12 SDK ile Blobları yönetme](storage-quickstart-blobs-java.md)<br>[Hızlı başlangıç: Python V12 SDK ile Blobları yönetme](storage-quickstart-blobs-python.md)<br>[Hızlı başlangıç: node. js ' de JavaScript V12 SDK ile Blobları yönetme](storage-quickstart-blobs-nodejs.md)|
|Arşiv erişim katmanı|Önizleme|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Yaşam döngüsü yönetim ilkeleri|Önizleme|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Tanılama günlükleri|Önizleme|[Azure depolama Analizi günlüğü](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Akışı değiştirme|Henüz desteklenmiyor|[Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md)|
|Hesap yük devretmesi|Henüz desteklenmiyor|[Olağanüstü durum kurtarma ve hesap yük devretme](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob kapsayıcı ACL 'SI|Henüz desteklenmiyor|[Kapsayıcı ACL 'sini ayarla](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Özel etki alanları|Henüz desteklenmiyor|[Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)|
|Sabit depolama|Henüz desteklenmiyor|[Sabit depolamayla iş açısından kritik blob verilerini depolayın](storage-blob-immutable-storage.md)|
|Anlık Görüntüler|Henüz desteklenmiyor|[.NET 'te blob anlık görüntüsü oluşturma ve yönetme](storage-blob-snapshots.md)|
|Geçici silme|Henüz desteklenmiyor|[Azure Depolama blobları için geçici silme](storage-blob-soft-delete.md)|
|Statik web siteleri|Henüz desteklenmiyor|[Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)|
|Azure Izleyici 'de günlüğe kaydetme|Henüz desteklenmiyor|Henüz kullanılamıyor|
|Premium blok Blobları|Henüz desteklenmiyor|[BlockBlobStorage hesabı oluşturma](storage-blob-create-account-block-blob.md)|

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)