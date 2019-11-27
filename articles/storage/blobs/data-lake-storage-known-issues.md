---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar ve sınırlamalar hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 3ea77eb5dd8a03f877164179e3accc3a6f6d0aef
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74548327"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makalede henüz desteklenmeyen veya hiyerarşik ad alanı (Azure Data Lake Storage 2.) olan depolama hesaplarıyla kısmen desteklenen özellikler ve araçlar listelenmektedir.

<a id="blob-apis-disabled" />

## <a name="issues-and-limitations-with-using-blob-apis"></a>Blob API 'Leri kullanma ile ilgili sorunlar ve sınırlamalar

Blob API 'Leri ve Data Lake Storage 2. API 'Leri aynı verilerde çalışabilir.

Bu bölümde, aynı verilerde çalışacak blob API 'Leri ve Data Lake Storage 2. API 'Leri kullanımıyla ilgili sorunlar ve sınırlamalar açıklanmaktadır.

* Aynı dosyanın aynı örneğine yazmak için hem blob API 'Leri hem de Data Lake Storage API 'Leri kullanamazsınız. Data Lake Storage 2. API 'Leri kullanarak bir dosyaya yazarsanız, bu dosyanın blokları [Get Block list](https://docs.microsoft.com/rest/api/storageservices/get-block-list) blob API 'sine yapılan çağrılara görünmez. Data Lake Storage 2. API 'leri ya da blob API 'Lerini kullanarak bir dosyanın üzerine yazabilirsiniz. Bu dosya özelliklerini etkilemez.

* Bir sınırlayıcı belirtmeden [Blobları Listele](https://docs.microsoft.com/rest/api/storageservices/list-blobs) işlemini kullandığınızda, sonuçlar hem dizinleri hem de Blobları içerecektir. Bir sınırlayıcı kullanmayı seçerseniz yalnızca eğik çizgi kullanın (`/`). Bu, desteklenen tek sınırlayıcıdır.

* Bir dizini silmek için [blobu silme](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API 'sini kullanırsanız, bu dizin yalnızca boşsa silinir. Bu, blob API 'SI silme dizinlerini yinelemeli olarak kullanamayacağı anlamına gelir.

Bu blob REST API 'Leri desteklenmez:

* [Blobu Yerleştir (sayfa)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Yerleştirme sayfası](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Sayfa aralıklarını al](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Artımlı kopya blobu](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [URL 'den sayfa koy](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Blobu Yerleştir (Ekle)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Ekleme bloğu](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [URL 'den ekleme bloğu](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

Yönetilmeyen VM diskleri hiyerarşik bir ad alanına sahip hesaplarda desteklenmez. Bir depolama hesabında hiyerarşik bir ad alanı etkinleştirmek istiyorsanız, yönetilmeyen VM disklerini hiyerarşik ad alanı özelliği etkin olmayan bir depolama hesabına yerleştirin.

<a id="api-scope-data-lake-client-library" />

## <a name="api-scope-for-data-lake-storage-client-library-for-sdk-powershell-and-cli"></a>SDK, PowerShell ve CLı için Data Lake Storage Istemci kitaplığı için API kapsamı

### <a name="filesystem-support-in-sdks"></a>SDK 'larda dosya sistemi desteği

- .NET, Java ve Python genel önizlemede. Diğer SDK 'lar Şu anda desteklenmiyor.
- Get ve set ACL 'Leri işlemleri şu anda özyinelemeli değil.

### <a name="filesystem-support-in-powershell-and-azure-cli"></a>PowerShell ve Azure CLı 'de dosya sistemi desteği

Get ve set ACL 'Leri işlemleri şu anda özyinelemeli değil.

## <a name="support-for-other-blob-storage-features"></a>Diğer BLOB depolama özellikleri için destek

Aşağıdaki tabloda henüz desteklenmeyen veya hiyerarşik ad alanı (Azure Data Lake Storage 2.) olan depolama hesaplarıyla kısmen desteklenen tüm diğer özellikler ve araçlar listelenmektedir.

| Özellik/Araç    | Daha fazla bilgi    |
|--------|-----------|
| **AzCopy** | Sürüme özgü destek <br><br>AzCopy 'in yalnızca en son sürümünü kullanın ([AzCopy ile v10 arasındaki](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). AzCopy v 8.1 gibi önceki AzCopy sürümleri desteklenmez.|
| **Azure Blob depolama yaşam döngüsü yönetim ilkeleri** | Yaşam döngüsü yönetimi ilkeleri desteklenir (Önizleme).  Tüm erişim katmanları desteklenir. Arşiv erişim katmanı Şu anda önizleme aşamasındadır. Blob anlık görüntülerini silme henüz desteklenmiyor. <br><br> Şu anda yaşam döngüsü yönetim ilkelerini ve arşiv erişim katmanını etkileyen hatalar var.  Yaşam döngüsü yönetim ilkelerinin önizlemesine ve arşiv erişim katmanını [buradan](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VURjFLTDRGS0Q4VVZCRFY5MUVaTVJDTkROMi4u)Arşivle için kaydolun.   |
| **Azure Content Delivery Network (CDN)** | Henüz desteklenmiyor|
| **Azure Arama** |Desteklenir (Önizleme)|
| **Azure Depolama Gezgini** | Sürüme özgü destek. <br><br>Yalnızca `1.6.0` veya üzeri sürümleri kullanın. <br> Şu anda, belirli senaryolarda kimlik doğrulama hatalarıyla sonuçlanabileceğinden sürümü `1.11.0` etkileyen bir depolama hatası vardır. Depolama hatasına yönelik bir onarım kullanıma alınıyor, ancak geçici bir çözüm olarak, [ücretsiz indirme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-relnotes)olarak kullanılabilen sürüm `1.10.x` kullanmanızı öneririz. `1.10.x`, depolama hatası tarafından etkilenmez.|
| **Blob kapsayıcı ACL 'Leri** |Henüz desteklenmiyor|
| **Blobsigortası** |Henüz desteklenmiyor|
| **Özel etki alanları** |Henüz desteklenmiyor|
| **Azure portal Depolama Gezgini** | Sınırlı destek. ACL 'Ler henüz desteklenmiyor. |
| **Tanılama günlüğüne kaydetme** |Tanılama günlükleri desteklenir (Önizleme).<br><br>Azure portal günlüklerin etkinleştirilmesi Şu anda desteklenmiyor. Günlükleri PowerShell kullanarak nasıl etkinleştireceğinizi gösteren bir örnek aşağıda verilmiştir. <br><br>`$storageAccount = Get-AzStorageAccount -ResourceGroupName <resourceGroup> -Name <storageAccountName>`<br><br>`Set-AzStorageServiceLoggingProperty -Context $storageAccount.Context -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays <days>`. <br><br>Bu örnekte gösterildiği gibi, `-ServiceType` parametresinin değeri olarak `Blob` belirttiğinizden emin olun. <br><br>Şu anda Azure Depolama Gezgini tanılama günlüklerini görüntülemek için kullanılamaz. Günlükleri görüntülemek için lütfen AzCopy veya SDK 'Ları kullanın.
| **Sabit depolama** |Henüz desteklenmiyor <br><br>Sabit depolama, verileri bir solucan içinde depolamanıza olanak tanır [(bir kez yaz, çok oku)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) durumu sağlar.|
| **Nesne düzeyi katmanları** |Seyrek erişimli ve arşiv katmanları desteklenir. Arşiv katmanı önizlemededir. Diğer tüm erişim katmanları henüz desteklenmiyor. <br><br> Şu anda arşiv erişim katmanını etkileyen hatalar var.  Arşiv erişim katmanının önizlemesine [buradan](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VURjFLTDRGS0Q4VVZCRFY5MUVaTVJDTkROMi4u)kaydolun.|
| **PowerShell ve CLı desteği** | Sınırlı işlevsellik <br><br>Blob işlemleri desteklenir. Dizinlerle çalışma ve erişim denetim listelerini (ACL 'Ler) ayarlama henüz desteklenmiyor. |
| **Statik Web siteleri** |Henüz desteklenmiyor <br><br>Özellikle, dosyaları [statik Web sitelerine](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)sunma özelliği.|
| **Üçüncü taraf uygulamalar** | Sınırlı destek <br><br>REST API 'Leri kullanan üçüncü taraf uygulamalar, Data Lake Storage 2. ile birlikte kullandığınızda çalışmaya devam edecektir. <br>Blob API 'Leri çağıran uygulamalar muhtemelen işe sunulacaktır.|
|**Geçici Silme** |Henüz desteklenmiyor|
| **Sürüm oluşturma özellikleri** |Henüz desteklenmiyor <br><br>Bu, [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)ve [anlık görüntüler](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)gibi diğer sürüm oluşturma özelliklerini içerir.|


