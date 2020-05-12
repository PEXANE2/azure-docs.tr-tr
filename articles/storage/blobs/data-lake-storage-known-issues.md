---
title: Azure Data Lake Storage 2. ile ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake Storage 2. kısıtlamaları ve bilinen sorunlar hakkında bilgi edinin.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 05/10/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: e80d1a05765d224dc4682c6f64faccc8c81f8ebd
ms.sourcegitcommit: 801a551e047e933e5e844ea4e735d044d170d99a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2020
ms.locfileid: "83007468"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. ile ilgili bilinen sorunlar

Bu makalede, Azure Data Lake Storage 2. kısıtlamaları ve bilinen sorunlar açıklanmaktadır.

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

Daha fazla sayıda BLOB depolama özelliği, hiyerarşik bir ad alanı olan hesaplarla çalışır. Tüm liste için, bkz. [Azure Data Lake Storage 2. bulunan BLOB Storage özellikleri](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Desteklenen Azure hizmeti tümleştirmeleri

Azure Data Lake Storage 2., verileri almak, analiz gerçekleştirmek ve görsel gösterimler oluşturmak için kullanabileceğiniz çeşitli Azure hizmetlerini destekler. Desteklenen Azure hizmetlerinin listesi için bkz. [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md).

[Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)bölümüne bakın.

## <a name="supported-open-source-platforms"></a>Desteklenen açık kaynaklı platformlar

Birçok açık kaynak platformu Data Lake Storage 2. destekler. Tüm liste için, bkz. [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md).

[Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)bölümüne bakın.

## <a name="blob-storage-apis"></a>BLOB depolama API 'Leri

Blob API 'Leri ve Data Lake Storage 2. API 'Leri aynı verilerde çalışabilir.

Bu bölümde, aynı verilerde çalışacak blob API 'Leri ve Data Lake Storage 2. API 'Leri kullanımıyla ilgili sorunlar ve sınırlamalar açıklanmaktadır.

* Aynı dosyanın aynı örneğine yazmak için hem blob API 'Leri hem de Data Lake Storage API 'Leri kullanamazsınız. Data Lake Storage 2. API 'Leri kullanarak bir dosyaya yazarsanız, bu dosyanın blokları [Get Block list](https://docs.microsoft.com/rest/api/storageservices/get-block-list) blob API 'sine yapılan çağrılara görünmez. Data Lake Storage 2. API 'leri ya da blob API 'Lerini kullanarak bir dosyanın üzerine yazabilirsiniz. Bu dosya özelliklerini etkilemez.

* Bir sınırlayıcı belirtmeden [Blobları Listele](https://docs.microsoft.com/rest/api/storageservices/list-blobs) işlemini kullandığınızda, sonuçlar hem dizinleri hem de Blobları içerecektir. Bir sınırlayıcı kullanmayı seçerseniz yalnızca eğik çizgi ( `/` ) kullanın. Bu, desteklenen tek sınırlayıcıdır.

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

## <a name="file-system-support-in-sdks-powershell-and-azure-cli"></a>SDK 'lar, PowerShell ve Azure CLı 'da dosya sistemi desteği

- Get ve set ACL işlemleri şu anda özyinelemeli değil.
- [Azure CLI](data-lake-storage-directory-file-acl-cli.md) desteği genel önizlemeye sunuldu.


## <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetim ilkeleri

Blob anlık görüntülerini silme henüz desteklenmiyor. 

## <a name="archive-tier"></a>Arşiv katmanı

Şu anda arşiv erişim katmanını etkileyen bir hata var.

## <a name="blobfuse"></a>Blobsigortası

Blobsigortası desteklenmez.

<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

AzCopy 'in yalnızca en son sürümünü kullanın ([AzCopy ile v10 arasındaki](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)).AzCopy v 8.1 gibi önceki AzCopy sürümleri desteklenmez.

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Azure Depolama Gezgini

Yalnızca sürümlerini  `1.6.0`   veya üstünü kullanın.

<a id="explorer-in-portal" />

## <a name="storage-explorer-in-the-azure-portal"></a>Azure portal Depolama Gezgini

ACL 'Ler henüz desteklenmiyor.

<a id="third-party-apps" />

## <a name="thirdpartyapplications"></a>Üçüncü taraf uygulamalar

REST API 'leri kullanan üçüncü taraf uygulamalar, bunları blob API 'Leri çağıran Data Lake Storage 2. uygulamalarla birlikte kullandığınızda çalışmaya devam edecektir.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Erişim denetim listeleri (ACL) ve anonim okuma erişimi

Bir kapsayıcıya [anonim okuma erişimi](storage-manage-access-to-resources.md) verildiyse, ACL 'lerin bu kapsayıcıya veya o kapsayıcıdaki dosyalara hiçbir etkisi olmaz.

## <a name="premium-performance-block-blob-storage-accounts"></a>Premium-performans Blok Blobu depolama hesapları

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Tanılama günlükleri henüz Azure portal kullanılarak etkinleştirilemez. PowerShell kullanarak bunları etkinleştirebilirsiniz. Örneğin:

```powershell
#To login
Connect-AzAccount

#Set default block blob storage account.
Set-AzCurrentStorageAccount -Name premiumGen2Account -ResourceGroupName PremiumGen2Group

#Enable logging
Set-AzStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays 14
```

### <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetim ilkeleri

- Yaşam döngüsü yönetimi ilkeleri henüz Premium Blok Blob depolama hesaplarında desteklenmemektedir. 

- Veriler Premium katmandan daha düşük katmanlara taşınamaz. 

- **BLOB silme** eylemi şu anda desteklenmiyor. 

### <a name="hdinsight-support"></a>HDInsight desteği

N HDInsight kümesi oluşturduğunuzda, hiyerarşik ad alanı özelliği etkinleştirilmiş bir Blok Blobu depolama hesabı seçemezsiniz. Ancak, hesabı oluşturduktan sonra kümeye ekleyebilirsiniz.

### <a name="dremio-support"></a>Dremio desteği

Dremio henüz hiyerarşik ad alanı özelliği etkinleştirilmiş bir Blok Blobu depolama hesabına bağlanmıyor. 

## <a name="windows-azure-storage-blob-wasb-driver-unsupported-with-data-lake-storage-gen2"></a>Windows Azure Depolama Blobu (. b) sürücüsü (Data Lake Storage 2. ile desteklenmez)

Şu anda, yalnızca blob API 'SI ile çalışmak üzere tasarlanan olan, çok sayıda yaygın senaryoda sorunlarla karşılaştığında. Özellikle, hiyerarşik bir ad alanı etkin depolama hesabına yönelik bir istemcudur. Data Lake Storage çoklu protokol erişimi, bu sorunları azaltmaz. 

Kullanım süresi (ve olasılı olma olasılığı yüksektir) için, bir istemci olarak, bir hiyerarşik ad alanı etkin depolama hesabına sahip olan müşterileri desteklemezler. Bunun yerine, Hadoop ortamınızda [Azure blob dosya sistemi (ABFS)](data-lake-storage-abfs-driver.md) sürücüsünü kullanmayı tercih etmenizi öneririz. Hadoop dalı-3 ' ten önceki bir sürüme sahip şirket içi Hadoop ortamından geçiş yapmaya çalışıyorsanız, sizin ve kuruluşunuz için doğru yolda sizinle iletişime geçebilmemiz için lütfen bir Azure destek bileti açın.
