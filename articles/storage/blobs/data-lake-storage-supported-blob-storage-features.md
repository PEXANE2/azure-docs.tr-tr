---
title: Azure Data Lake Storage 2. 'de bulunan BLOB depolama özellikleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özellikleri hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 15e6cf7adfda995148a75ec21b8d8e5d8a5cab2e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559071"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz.

Bu tablo, Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özelliklerini listeler. Destek genişlemeye devam ettiğinden, Bu tablolarda görüntülenen öğeler zaman içinde değişir. Bir özelliğin önizleme durumuyla ilişkili belirli sorunlar hakkında daha fazla bilgi edinmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

> [!NOTE]
> Destek düzeyi yalnızca özelliğin Data Lake Storage 2. ile nasıl desteklendiğini gösterir. 
>
> Data Lake Storage 2. için [Premium-performans blok Blobstorage hesapları](storage-blob-create-account-block-blob.md) Şu anda genel önizlemededir. Destek-bu tür hesapların düzeyleri **Blockblobstorage (Premium)** sütununda görüntülenir.

|BLOB depolama özelliği |Genel amaçlı v2 |BlockBlobStorage (Premium) |İlgili makaleler: |
|---------------|-------------------|---|
|Sık erişim katmanı|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Seyrek erişim katmanı|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Olaylar|Genel kullanıma sunuldu|Önizleme|[Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)|
|Ölçümler (klasik)|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Storage Analytics ölçümleri (klasik)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure İzleyicisi'nde ölçümler|Genel kullanıma sunuldu|Önizleme|[Azure Izleyici 'de Azure depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|BLOB depolama PowerShell komutları|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: PowerShell ile Blobları karşıya yükleme, indirme ve listeleme](storage-quickstart-blobs-powershell.md)|
|BLOB depolama Azure CLı komutları|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: Azure CLı ile Bloblar oluşturma, indirme ve listeleme](storage-quickstart-blobs-cli.md)|
|BLOB depolama API 'Leri|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: .NET için Azure Blob depolama istemci kitaplığı V12](storage-quickstart-blobs-dotnet.md)<br>[Hızlı başlangıç: Java V12 SDK ile Blobları yönetme](storage-quickstart-blobs-java.md)<br>[Hızlı başlangıç: Python V12 SDK ile Blobları yönetme](storage-quickstart-blobs-python.md)<br>[Hızlı başlangıç: Node.jsiçindeki JavaScript V12 SDK ile Blobları yönetme](storage-quickstart-blobs-nodejs.md)|
|Tanılama günlükleri|Genel kullanıma sunuldu|Önizleme <div role="complementary" aria-labelledby="diagnostic-logging"><sup>1</sup></div> |[Azure Depolama analizini günlüğe kaydetme](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Arşiv erişim katmanı|Genel kullanıma sunuldu|Desteklenmiyor|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Azure Izleyici 'de günlüğe kaydetme|Önizleme |Henüz desteklenmiyor|[Azure Storage 'ı izleme](../common/monitor-storage.md)|
|Anlık Görüntüler|Önizleme|Henüz desteklenmiyor|[Blob anlık görüntüleri](snapshots-overview.md)|
|Statik web siteleri|Önizleme|Henüz desteklenmiyor|[Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)|
|Sabit depolama|Önizleme|Henüz desteklenmiyor|[Sabit depolamayla iş açısından kritik blob verilerini depolayın](storage-blob-immutable-storage.md)|
|Yaşam döngüsü yönetim ilkeleri|Önizleme|Henüz desteklenmiyor|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Blobsigortası|Önizleme|Henüz desteklenmiyor|[Blob depolamayı blobsigortası ile dosya sistemi olarak bağlama](storage-how-to-mount-container-linux.md)|
|Akışı değiştirme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md)|
|Hesap yük devretmesi|Henüz desteklenmiyor|Henüz desteklenmiyor|[Olağanüstü durum kurtarma ve hesap yük devretme](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob kapsayıcı ACL 'SI|Henüz desteklenmiyor|Henüz desteklenmiyor|[Kapsayıcı ACL 'sini ayarla](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Özel etki alanları|Henüz desteklenmiyor|Henüz desteklenmiyor|[Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)|
|Geçici silme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Depolama blobları için geçici silme](storage-blob-soft-delete.md)|

<div id="diagnostic-logging"><sup>1</sup> Premium Blok Blob depolama hesapları için, tanılama günlükleri (klasik) Azure Portal kullanılarak etkinleştirilemez. PowerShell kullanarak bunları etkinleştirin.</div>

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)