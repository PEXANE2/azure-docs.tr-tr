---
title: Azure Data Lake Storage 2. 'de bulunan BLOB depolama özellikleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özellikleri hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/12/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: b670ec3cee2ebd1cc98ea2e04fb2b0dfd90e4e1a
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94594224"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri

[Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz.

Bu tablo, Azure Data Lake Storage 2. ile kullanabileceğiniz BLOB depolama özelliklerini listeler. Destek genişlemeye devam ettiğinden, Bu tablolarda görüntülenen öğeler zaman içinde değişir. Bir özelliğin önizleme durumuyla ilişkili belirli sorunlar hakkında daha fazla bilgi edinmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın.

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

Aşağıdaki tabloda, her bir BLOB depolama özelliğinin Data Lake Storage 2. ile nasıl desteklendiği gösterilmektedir. Standart ve [Premium performans](premium-tier-for-data-lake-storage.md) katmanları için bir sütun vardır. 

|BLOB depolama özelliği |Standart |Premium |İlgili makaleler: |
|---------------|-------------------|---|
|Sık erişim katmanı|Genel kullanıma sunuldu|Desteklenmez|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Seyrek erişim katmanı|Genel kullanıma sunuldu|Desteklenmez|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Olaylar|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)|
|Ölçümler (klasik)|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Azure Storage Analytics ölçümleri (klasik)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure İzleyicisi'nde ölçümler|Genel kullanıma sunuldu|Önizleme|[Azure İzleyici’de Azure Depolama ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|BLOB depolama PowerShell komutları|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Hızlı başlangıç: PowerShell ile Blobları karşıya yükleme, indirme ve listeleme](storage-quickstart-blobs-powershell.md)|
|BLOB depolama Azure CLı komutları|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Hızlı başlangıç: Azure CLı ile Bloblar oluşturma, indirme ve listeleme](storage-quickstart-blobs-cli.md)|
|BLOB depolama API 'Leri|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Hızlı Başlangıç: .NET için Azure Blob depolama istemci kitaplığı v12](storage-quickstart-blobs-dotnet.md)<br>[Hızlı başlangıç: Java V12 SDK ile Blobları yönetme](storage-quickstart-blobs-java.md)<br>[Hızlı başlangıç: Python V12 SDK ile Blobları yönetme](storage-quickstart-blobs-python.md)<br>[Hızlı başlangıç: Node.jsiçindeki JavaScript V12 SDK ile Blobları yönetme ](storage-quickstart-blobs-nodejs.md)|
|Tanılama günlükleri|Genel kullanıma sunuldu|Önizleme |[Azure Depolama analizini günlüğe kaydetme](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Arşiv erişim katmanı|Genel kullanıma sunuldu|Desteklenmez|[Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md)|
|Yaşam döngüsü yönetimi ilkeleri (katmanlama)|Genel kullanıma sunuldu|Henüz desteklenmiyor|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Yaşam döngüsü yönetim ilkeleri (blobu Sil)|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)|
|Azure Izleyici 'de günlüğe kaydetme|Önizleme |Önizleme|[Azure Storage 'ı izleme](../common/monitor-storage.md)|
|Anlık Görüntüler|Önizleme|Önizleme|[Blob anlık görüntüleri](snapshots-overview.md)|
|Statik web siteleri|Önizleme|Önizleme|[Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)|
|Sabit depolama|Önizleme|Önizleme|[Sabit depolamayla iş açısından kritik blob verilerini depolayın](storage-blob-immutable-storage.md)|
|Kapsayıcı geçici silme|Önizleme|Önizleme|[Kapsayıcılar için geçici silme (Önizleme)](soft-delete-container-overview.md)|
|Blob geçici silme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Bloblar için geçici silme](storage-blob-soft-delete.md)|
|Blobsigortası|Genel kullanıma sunuldu|Genel kullanıma sunuldu|[Blob depolamayı blobsigortası ile dosya sistemi olarak bağlama](storage-how-to-mount-container-linux.md)|
|Hesap yük devretmesi|Henüz desteklenmiyor|Henüz desteklenmiyor|[Olağanüstü durum kurtarma ve hesap yük devretme](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Blob kapsayıcı ACL 'SI|Desteklenmez<div role="complementary" aria-labelledby="blob-container-ACL"><sup>1</sup></div>|Desteklenmez<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|Bu tablonun altındaki ilgili nota bakın.|
|Müşteri tarafından sunulan anahtarlar|Henüz desteklenmiyor|Henüz desteklenmiyor|[BLOB depolama için istekte bir şifreleme anahtarı sağlayın](encryption-customer-provided-keys.md)|
|Özel etki alanları|Henüz desteklenmiyor|Henüz desteklenmiyor|[Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)|
|Şifreleme kapsamları|Henüz desteklenmiyor|Henüz desteklenmiyor|[Şifreleme kapsamları oluşturma ve yönetme (Önizleme)](encryption-scope-manage.md)|
|Akışı değiştirme|Henüz desteklenmiyor|Henüz desteklenmiyor|[Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md)|
|Nesne çoğaltma|Henüz desteklenmiyor|Henüz desteklenmiyor|[Blok Blobları için nesne çoğaltmasını yapılandırma](object-replication-configure.md)|
|Blob sürümü oluşturma|Henüz desteklenmiyor|Henüz desteklenmiyor|[Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)|

<div id="blob-container-ACL"><sup>1</sup> ACL 'leri kapsayıcının kök klasöründe ayarlayabilirsiniz, ancak kapsayıcının kendisi olamaz.</div><br>

<div id="preview-form"><sup>2</sup> Data Lake Storage 2. ile anlık görüntüleri, sabit depolamayı veya statik Web sitelerini kullanmak için bu <a href=https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VUOUc3NTNQSUdOTjgzVUlVT1pDTzU4WlRKRy4u>formu</a>tamamlayarak önizlemeye kaydetmeniz gerekir.  </div>

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)
