---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar ve sınırlamalar hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: bacb677b8354c0b0e219ce7483a1446a96c28288
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855525"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makalede henüz desteklenmeyen veya hiyerarşik ad alanı (Azure Data Lake Storage 2.) olan depolama hesaplarıyla kısmen desteklenen özellikler ve araçlar listelenmektedir.

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>BLOB Depolama API'leri

BLOB depolama API 'leri Azure Data Lake Gen2 API 'Leri ile birlikte kullanılabilir olmadığı için, BLOB depolama API 'Leri devre dışı bırakılmış olan özellik sorunlarını engellemek için devre dışıdır.

> [!NOTE]
> Data Lake Storage üzerindeki çoklu protokol erişiminin genel önizlemesine kaydoldıysanız, blob API 'Leri ve Data Lake Storage 2. API 'Leri aynı verilerde çalışabilir. Daha fazla bilgi için bkz. [Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md).

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>Mevcut araçlar, uygulamalar ve hizmetlerle ne yapmalı

Bunlardan herhangi biri blob API 'Leri kullanıyorsa ve hesabınıza yüklediğiniz tüm içerikle çalışmak üzere bunları kullanmak istiyorsanız iki seçeneğiniz vardır.

* **Seçenek 1**: Blob API 'Leri Azure Data Lake Gen2 API 'Leri ile birlikte çalışabilir hale gelene kadar BLOB depolama hesabınızda hiyerarşik bir ad alanı etkinleştirmeyin. Hiyerarşik ad alanı olmadan bir depolama hesabı kullanmak, dizin ve dosya sistemi erişim denetim listeleri gibi belirli özelliklere Data Lake Storage 2. erişiminizin olmadığı anlamına gelir.

* **Seçenek 2**: [Data Lake Storage üzerindeki çoklu protokol erişiminin](data-lake-storage-multi-protocol-access.md)genel önizlemesine kaydolun. Blob API 'Leri çağıran araç ve uygulamaların yanı sıra, tanılama günlükleri gibi BLOB depolama özellikleri de hiyerarşik bir ad alanı olan hesaplarla çalışabilir.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Blob API 'lerinin devre dışı bırakılmadan önce veri yüklemek için blob API 'Leri kullandıysanız yapmanız gerekenler

Önce devre dışı bırakıldı ve bu verilere erişmek için bir üretim ihtiyacı olan verileri yüklemek için bu API'leri kullandıysanız, lütfen Microsoft Support aşağıdaki bilgilerle başvurun:

> [!div class="checklist"]
> * Abonelik KIMLIĞI (adı değil GUID).
> * Depolama hesabı adları.
> * Üretimde etkin bir şekilde etkilenip etkilenmediğinizi ve varsa, hangi depolama hesapları için?.
> * Bu verileri başka bir depolama hesabına herhangi bir nedenden dolayı kopyalanması gerekip gerekmediğini ve bu durumda, üretimde etkin bir şekilde etkilenmez olsa bile, bize neden?

Bu koşullarda, bu verileri hiyerarşik ad alanı özelliği etkin olmayan bir depolama hesabına kopyalayabilmeniz için, blob API 'sine erişimi sınırlı bir süre boyunca geri yükleyebilirsiniz.

### <a name="issues-and-limitations-with-using-blob-apis-on-accounts-that-have-a-hierarchical-namespace"></a>Hiyerarşik ad alanı olan hesaplarda blob API 'Leri kullanmayla ilgili sorunlar ve sınırlamalar

Data Lake Storage üzerindeki çoklu protokol erişiminin genel önizlemesine kaydoldıysanız, blob API 'Leri ve Data Lake Storage 2. API 'Leri aynı verilerde çalışabilir.

Bu bölümde, aynı verilerde çalışacak blob API 'Leri ve Data Lake Storage 2. API 'Leri kullanımıyla ilgili sorunlar ve sınırlamalar açıklanmaktadır.

* Aynı dosyanın aynı örneğine yazmak için hem blob API 'Leri hem de Data Lake Storage API 'Leri kullanamazsınız.

* Data Lake Storage 2. API 'Leri kullanarak bir dosyaya yazarsanız, bu dosyanın blokları [Get Block list](https://docs.microsoft.com/rest/api/storageservices/get-block-list) blob API 'sine yapılan çağrılara görünmez.

* Data Lake Storage 2. API 'leri ya da blob API 'Lerini kullanarak bir dosyanın üzerine yazabilirsiniz. Bu dosya özelliklerini etkilemez.

* Bir sınırlayıcı belirtmeden [Blobları Listele](https://docs.microsoft.com/rest/api/storageservices/list-blobs) işlemini kullandığınızda, sonuçlar hem dizinleri hem de Blobları içerecektir.

  Bir sınırlayıcı kullanmayı seçerseniz yalnızca eğik çizgi (`/`) kullanın. Bu, desteklenen tek sınırlayıcıdır.

* Bir dizini silmek için [blobu silme](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API 'sini kullanırsanız, bu dizin yalnızca boşsa silinir.

  Bu, blob API 'SI silme dizinlerini yinelemeli olarak kullanamayacağı anlamına gelir.

Bu blob REST API 'Leri desteklenmez:

* [Blobu Yerleştir (sayfa)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Yerleştirme sayfası](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Sayfa aralıklarını al](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Artımlı kopya blobu](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [URL 'den sayfa koy](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Blobu Yerleştir (Ekle)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Ekleme bloğu](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [URL 'den ekleme bloğu](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

## <a name="issues-with-unmanaged-virtual-machine-vm-disks"></a>Yönetilmeyen sanal makine (VM) disklerinde sorunlar

Yönetilmeyen VM diskleri hiyerarşik bir ad alanına sahip hesaplarda desteklenmez. Bir depolama hesabında hiyerarşik bir ad alanı etkinleştirmek istiyorsanız, yönetilmeyen VM disklerini hiyerarşik ad alanı özelliği etkin olmayan bir depolama hesabına yerleştirin.


## <a name="support-for-other-blob-storage-features"></a>Diğer BLOB depolama özellikleri için destek

Aşağıdaki tabloda henüz desteklenmeyen veya hiyerarşik ad alanı (Azure Data Lake Storage 2.) olan depolama hesaplarıyla kısmen desteklenen tüm diğer özellikler ve araçlar listelenmektedir.

| Özellik/Araç    | Daha fazla bilgi    |
|--------|-----------|
| **Data Lake Storage 2. depolama hesapları için API 'Ler** | Kısmen destekleniyor <br><br>Data Lake Storage çoklu protokol erişimi şu anda genel önizleme aşamasındadır. Bu önizleme, .NET, Java, Python SDK 'Lerinde hiyerarşik bir ad alanına sahip hesaplara sahip blob API 'Lerini kullanmanıza olanak sağlar.  SDK 'Lar henüz dizinlerle etkileşime girebilmeniz veya erişim denetim listelerini (ACL 'Ler) ayarlamanıza olanak tanıyan API 'Ler içermez. Bu işlevleri gerçekleştirmek için Data Lake Storage 2. **rest** API 'lerini kullanabilirsiniz. |
| **AzCopy** | Sürüme özgü destek <br><br>AzCopy 'in yalnızca en son sürümünü kullanın ([AzCopy ile v10 arasındaki](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). AzCopy v 8.1 gibi önceki AzCopy sürümleri desteklenmez.|
| **Azure Blob depolama yaşam döngüsü yönetim ilkeleri** | Yalnızca [Data Lake Storage önizlemede çoklu protokol erişimine](data-lake-storage-multi-protocol-access.md) kaydoldıysanız desteklenir. Seyrek erişimli ve arşiv erişim katmanları yalnızca önizleme tarafından desteklenir. Blob anlık görüntülerini silme henüz desteklenmiyor. |
| **Azure Content Delivery Network (CDN)** | Henüz desteklenmiyor|
| **Azure Arama** |Yalnızca [Data Lake Storage önizlemede çoklu protokol erişimine](data-lake-storage-multi-protocol-access.md) kaydoldıysanız desteklenir.|
| **Azure Depolama Gezgini** | Sürüme özgü destek <br><br>Yalnızca sürümü `1.6.0` veya üstünü kullanın. <br>Sürüm `1.6.0` [ücretsiz bir indirme](https://azure.microsoft.com/features/storage-explorer/)olarak sunulmaktadır.|
| **Blob kapsayıcı ACL 'Leri** |Henüz desteklenmiyor|
| **Blobsigortası** |Henüz desteklenmiyor|
| **Özel etki alanları** |Henüz desteklenmiyor|
| **Dosya sistemi Gezgini** | Sınırlı destek |
| **Tanılama günlüğüne kaydetme** |Tanılama günlükleri yalnızca [Data Lake Storage önizlemede çoklu protokol erişimine](data-lake-storage-multi-protocol-access.md) kaydoldıysanız desteklenir. <br><br>Azure portal günlüklerin etkinleştirilmesi Şu anda desteklenmiyor. Günlükleri PowerShell kullanarak nasıl etkinleştireceğinizi gösteren bir örnek aşağıda verilmiştir. <br><br>`$storageAccount = Get-AzStorageAccount -ResourceGroupName <resourceGroup> -Name <storageAccountName>`<br><br>`Set-AzureStorageServiceLoggingProperty -Context $storageAccount.Context -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays <days>`. <br><br>Bu örnekte gösterildiği gibi `Blob` `-ServiceType` parametrenin değeri olarak belirttiğinizden emin olun. 
| **Sabit depolama** |Henüz desteklenmiyor <br><br>Sabit depolama, verileri bir solucan içinde depolamanıza olanak tanır [(bir kez yaz, çok oku)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) durumu sağlar.|
| **Nesne düzeyi katmanları** |Seyrek erişimli ve arşiv katmanları yalnızca [Data Lake Storage önizlemesinde çoklu protokol erişimine](data-lake-storage-multi-protocol-access.md) kaydoldıysanız desteklenir. <br><br> Diğer tüm erişim katmanları henüz desteklenmiyor.|
| **PowerShell ve CLı desteği** | Sınırlı işlevsellik <br><br>Hesap oluşturma gibi yönetim işlemleri desteklenir. Dosya yükleme ve indirme gibi veri düzlemi işlemleri, [Data Lake Storage üzerindeki çoklu protokol erişiminin](data-lake-storage-multi-protocol-access.md)bir parçası olarak genel önizlemededir. Dizinlerle çalışma ve erişim denetim listelerini (ACL 'Ler) ayarlama henüz desteklenmiyor. |
| **Statik Web siteleri** |Henüz desteklenmiyor <br><br>Özellikle, dosyaları [statik Web sitelerine](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)sunma özelliği.|
| **Üçüncü taraf uygulamalar** | Sınırlı destek <br><br>REST API 'Leri kullanan üçüncü taraf uygulamalar, Data Lake Storage 2. ile birlikte kullandığınızda çalışmaya devam edecektir. <br>[Data Lake Storage üzerindeki çoklu protokol erişiminin](data-lake-storage-multi-protocol-access.md)genel önizlemesine Kaydoldıysanız, blob API 'leri çağıran uygulamalar muhtemelen çalışır. 
| **Sürüm oluşturma özellikleri** |Henüz desteklenmiyor <br><br>Bu [anlık görüntüleri](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) ve [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)bilgilerini içerir.|


