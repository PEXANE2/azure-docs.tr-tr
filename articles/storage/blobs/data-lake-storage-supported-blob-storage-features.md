---
title: Azure Data Lake Storage 2. 'de bulunan BLOB depolama özellikleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özellikleri hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 08/25/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: f84298c2c43afbb9e0379516be1e83002a7e3926
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89228690"
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
|Sık erişim katmanı|Genel kullanıma sunuldu|Desteklenmez|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Seyrek erişim katmanı|Genel kullanıma sunuldu|Desteklenmez|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Ekinlikler|Genel kullanıma sunuldu|Önizleme|[Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)|
|Ölçümler (klasik)|Genel kullanıma sunuldu|Desteklenmez|[Azure Storage Analytics ölçümleri (klasik)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure İzleyicisi'nde ölçümler|Genel kullanıma sunuldu|Önizleme|[Azure İzleyici’de Azure Depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|BLOB depolama PowerShell komutları|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: PowerShell ile Blobları karşıya yükleme, indirme ve listeleme](storage-quickstart-blobs-powershell.md)|
|BLOB depolama Azure CLı komutları|Genel kullanıma sunuldu|Önizleme|[Hızlı başlangıç: Azure CLı ile Bloblar oluşturma, indirme ve listeleme](storage-quickstart-blobs-cli.md)|
|BLOB depolama API 'Leri|Genel kullanıma sunuldu|Önizleme|[Hızlı Başlangıç: .NET için Azure Blob depolama istemci kitaplığı v12](storage-quickstart-blobs-dotnet.md)<br>[Hızlı başlangıç: Java V12 SDK ile Blobları yönetme](storage-quickstart-blobs-java.md)<br>[Hızlı başlangıç: Python V12 SDK ile Blobları yönetme](storage-quickstart-blobs-python.md)<br>[Hızlı başlangıç: Node.jsiçindeki JavaScript V12 SDK ile Blobları yönetme ](storage-quickstart-blobs-nodejs.md)|
|Tanılama günlükleri|Genel kullanıma sunuldu|Önizleme <div role="complementary" aria-labelledby="diagnostic-logging"><sup>1</sup></div> |[Azure Depolama analizi günlüğü](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Arşiv erişim katmanı|Genel kullanıma sunuldu|Desteklenmez|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Yaşam döngüsü yönetim ilkeleri|Genel kullanıma sunuldu|Henüz desteklenmiyor|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Azure Izleyici 'de günlüğe kaydetme|Önizleme |Henüz desteklenmiyor|[Azure Storage 'ı izleme](../common/monitor-storage.md)|
|Anlık Görüntüler|Önizleme|Henüz desteklenmiyor|[Blob anlık görüntüleri](snapshots-overview.md)|
|Statik web siteleri|Önizleme|Henüz desteklenmiyor|[Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)|
|Sabit depolama|Önizleme|Henüz desteklenmiyor|[Sabit depolamayla iş açısından kritik blob verilerini depolayın](storage-blob-immutable-storage.md)|
|Kapsayıcı geçici silme|Önizleme|Önizleme|[Kapsayıcılar için geçici silme (Önizleme)](soft-delete-container-overview.md)|
|Blob geçici silme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Bloblar için geçici silme](storage-blob-soft-delete.md)|
|Blobsigortası|Önizleme|Henüz desteklenmiyor|[Blob depolamayı blobsigortası ile dosya sistemi olarak bağlama](storage-how-to-mount-container-linux.md)|
|Hesap yük devretmesi|Henüz desteklenmiyor|Henüz desteklenmiyor|[Olağanüstü durum kurtarma ve hesap yük devretme](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob kapsayıcı ACL 'SI|Henüz desteklenmiyor<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|Henüz desteklenmiyor<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|[Kapsayıcı ACL 'sini ayarla](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Özel etki alanları|Henüz desteklenmiyor|Henüz desteklenmiyor|[Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)|
|Şifreleme kapsamları|Henüz desteklenmiyor|Henüz desteklenmiyor|[Şifreleme kapsamları oluşturma ve yönetme (Önizleme)](encryption-scope-manage.md)|
|Akışı değiştirme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md)|
|Nesne çoğaltma|Henüz desteklenmiyor|Henüz desteklenmiyor|[Blok Blobları için nesne çoğaltmasını yapılandırma (Önizleme)](object-replication-configure.md)|
|Blob sürümü oluşturma|Henüz desteklenmiyor|Henüz desteklenmiyor|[Blob sürüm oluşturmayı etkinleştirme ve yönetme (Önizleme)](versioning-enable.md)|

<div id="diagnostic-logging"><sup>1</sup> Premium Blok Blob depolama hesapları için, tanılama günlükleri (klasik) Azure Portal kullanılarak etkinleştirilemez. PowerShell kullanarak bunları etkinleştirin.</div><br>

<div id="blob-container-ACL"><sup>2</sup> ACL 'leri kapsayıcının kök klasöründe ayarlayabilirsiniz, ancak kapsayıcının kendisi olamaz.</div><br>

<div id="preview-form"><sup>3</sup> Data Lake Storage 2. ile anlık görüntüleri, sabit depolamayı veya statik Web sitelerini kullanmak için bu <a href=https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VUOUc3NTNQSUdOTjgzVUlVT1pDTzU4WlRKRy4u>formu</a>tamamlayarak önizlemeye kaydetmeniz gerekir.  </div>

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)
