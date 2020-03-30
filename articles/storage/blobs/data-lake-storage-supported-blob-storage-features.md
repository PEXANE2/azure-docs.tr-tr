---
title: Blob depolama özellikleri Azure Veri Gölü Depolama Gen2 | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen2 ile hangi Blob depolama özelliklerini kullanabileceğiniz hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 215244204aa58cc2fdedc639d48e01b514759694
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196017"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Blob depolama özellikleri Azure Veri Gölü Depolama Gen2'de kullanılabilir

[Tanılama günlüğü,](../common/storage-analytics-logging.md) [erişim katmanları](storage-blob-storage-tiers.md)ve Blob Depolama yaşam döngüsü yönetim ilkeleri gibi [Blob Depolama](storage-lifecycle-management-concepts.md) özellikleri artık hiyerarşik ad alanına sahip hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeden Blob depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz.

Bu tablo, Azure Veri Gölü Depolama Gen2 ile kullanabileceğiniz Blob depolama özelliklerini listeler. Destek genişlemeye devam ettikçe, bu tablolarda görünen öğeler zaman içinde değişecektir.

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

> [!NOTE]
> Destek düzeyi yalnızca özelliğin Veri Gölü Depolama Gen2 ile nasıl desteklendirilebildiğini ifade eder.

|Blob Depolama özelliği |Destek düzeyi |İlgili makaleler: |
|---------------|-------------------|---|
|Sık erişim katmanı|Genel kullanıma sunuldu|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Seyrek erişim katmanı|Genel kullanıma sunuldu|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Olaylar|Genel kullanıma sunuldu|[Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)|
|Ölçümler (Klasik)|Genel kullanıma sunuldu|[Azure Depolama analiz ölçümleri (Klasik)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure İzleyicisi'nde ölçümler|Genel kullanıma sunuldu|[Azure İzleyici’de Azure Depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob depolama PowerShell komutları|Genel kullanıma sunuldu|[Quickstart: PowerShell ile yükleme, indirme ve liste blobs](storage-quickstart-blobs-powershell.md)|
|Blob depolama Azure CLI komutları|Genel kullanıma sunuldu|[Quickstart: Azure CLI ile blob oluşturma, indirme ve listele](storage-quickstart-blobs-cli.md)|
|Blob depolama API'leri|Genel kullanıma sunuldu|[Quickstart: .NET için Azure Blob depolama istemcisi kitaplığı v12](storage-quickstart-blobs-dotnet.md)<br>[Quickstart: Java v12 SDK ile blobs yönetin](storage-quickstart-blobs-java.md)<br>[Quickstart: Python v12 SDK ile blobs yönetin](storage-quickstart-blobs-python.md)<br>[Quickstart: Node.js JavaScript v12 SDK ile blobs yönetin](storage-quickstart-blobs-nodejs.md)|
|Arşiv Erişim Katmanı|Önizleme|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Yaşam döngüsü yönetimi politikaları|Önizleme|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Tanılama günlükleri|Önizleme|[Azure Depolama analizi günlüğü](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Akışı değiştirme|Henüz desteklenmedi|[Azure Blob depolama alanında özet akışı desteğini değiştirme](storage-blob-change-feed.md)|
|Hesap başarısız|Henüz desteklenmedi|[Olağanüstü durum kurtarma ve hesap başarısız](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob konteyner ACL|Henüz desteklenmedi|[Konteyner ACL'yi Ayarla](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Özel etki alanları|Henüz desteklenmedi|[Azure Blob depolama bitiş noktasıyla özel bir etki alanı haritası](storage-custom-domain-name.md)|
|Değişmez depolama|Henüz desteklenmedi|[İş açısından kritik blob verilerini değişmez depolama yla depolama](storage-blob-immutable-storage.md)|
|Anlık Görüntüler|Henüz desteklenmedi|[.NET'te bir blob anlık görüntüsü oluşturma ve yönetme](storage-blob-snapshots.md)|
|Geçici Silme|Henüz desteklenmedi|[Azure Depolama blobları için geçici silme](storage-blob-soft-delete.md)|
|Statik web siteleri|Henüz desteklenmedi|[Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)|
|Azure Monitör'de Günlüğe Kaydetme|Henüz desteklenmedi|Henüz kullanılamıyor|
|Premium blok lekeleri|Henüz desteklenmedi|[BlockBlobStorage hesabı oluşturma](storage-blob-create-account-block-blob.md)|

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Veri Gölü Depolama Gen2'yi destekleyen açık kaynak platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Veri Gölü Depolama'da çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)