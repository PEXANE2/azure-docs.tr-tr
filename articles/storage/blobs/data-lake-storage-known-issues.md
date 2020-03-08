---
title: Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar | Microsoft Docs
description: Azure Data Lake Storage 2. kısıtlamaları ve bilinen sorunlar hakkında bilgi edinin.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 7fd76be8d17dc1c632e555a56d038d4f5c1e1486
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78669155"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar

Bu makalede, Azure Data Lake Storage 2. kısıtlamaları ve bilinen sorunlar açıklanmaktadır.

## <a name="supported-blob-storage-features"></a>Desteklenen BLOB depolama özellikleri

Daha fazla sayıda BLOB depolama özelliği, hiyerarşik bir ad alanı olan hesaplarla çalışır. Tüm liste için, bkz. [Azure Data Lake Storage 2. bulunan BLOB Storage özellikleri](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Desteklenen Azure hizmeti tümleştirmeleri

Data Lake Storage Gen2, verileri almak, analiz yapmak ve görsel gösterimler oluşturmak için kullanabileceğiniz çeşitli Azure hizmetlerini destekler. Desteklenen Azure hizmetlerinin listesi için bkz. [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md).

[Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md)bölümüne bakın.

## <a name="supported-open-source-platforms"></a>Desteklenen açık kaynaklı platformlar

Data Lake depolama Gen2'ye birden fazla açık kaynak platformları destekler. Tüm liste için, bkz. [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md).

[Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)bölümüne bakın.

## <a name="blob-storage-apis"></a>BLOB Depolama API'leri

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

## <a name="file-system-support-in-sdks"></a>SDK 'larda dosya sistemi desteği

- [.Net](data-lake-storage-directory-file-acl-dotnet.md), [Java](data-lake-storage-directory-file-acl-java.md) ve [Python](data-lake-storage-directory-file-acl-python.md), [JavaScript](data-lake-storage-directory-file-acl-javascript.md) ve destek genel önizlemededir. Diğer SDK 'lar Şu anda desteklenmiyor.
- Get ve set ACL işlemleri şu anda özyinelemeli değil.

## <a name="file-system-support-in-powershell-and-azure-cli"></a>PowerShell ve Azure CLı 'de dosya sistemi desteği

- [PowerShell](data-lake-storage-directory-file-acl-powershell.md) ve [Azure CLI](data-lake-storage-directory-file-acl-cli.md) desteği genel önizlemeye sunuldu.
- Get ve set ACL işlemleri şu anda özyinelemeli değil.

## <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetim ilkeleri

* Blob anlık görüntülerini silme henüz desteklenmiyor.  

* Şu anda yaşam döngüsü yönetim ilkelerini ve arşiv erişim katmanını etkileyen hatalar var. 

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Tanılama günlüklerini görüntülemek için Azure Depolama Gezgini 1,10. x kullanılamaz. Günlükleri görüntülemek için lütfen AzCopy veya SDK 'Ları kullanın.

## <a name="blobfuse"></a>Blobsigortası

Blobsigortası desteklenmez.



<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

AzCopy 'in yalnızca en son sürümünü kullanın ([AzCopy ile v10 arasındaki](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). AzCopy v 8.1 gibi önceki AzCopy sürümleri desteklenmez.

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Azure Depolama Gezgini

Yalnızca `1.6.0` veya üzeri sürümleri kullanın. Şu anda `1.11.0` etkileyen bir depolama hatası vardır. Bu, belirli senaryolarda kimlik doğrulama hatalarına neden olabilir. Depolama hatasına yönelik bir onarım kullanıma alınıyor, ancak geçici bir çözüm olarak, [ücretsiz indirme](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-relnotes)olarak kullanılabilen sürüm `1.10.x` kullanmanızı öneririz. `1.10.x` depolama hatası tarafından etkilenmez.

<a id="explorer-in-portal" />

## <a name="storage-explorer-in-the-azure-portal"></a>Azure portal Depolama Gezgini

ACL 'Ler henüz desteklenmiyor.

<a id="third-party-apps" />

## <a name="thirdpartyapplications"></a>Üçüncü taraf uygulamalar

REST API 'leri kullanan üçüncü taraf uygulamalar, bunları blob API 'Leri çağıran Data Lake Storage 2. uygulamalarla birlikte kullandığınızda çalışmaya devam edecektir.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Erişim denetim listeleri (ACL) ve anonim okuma erişimi

Bir kapsayıcıya [anonim okuma erişimi](storage-manage-access-to-resources.md) verildiyse, ACL 'lerin bu kapsayıcıya veya o kapsayıcıdaki dosyalara hiçbir etkisi olmaz.

## <a name="windows-azure-storage-blob-wasb-driver"></a>Windows Azure Depolama Blobu (te b) sürücüsü

Şu anda, ıDB sürücüsünü, hiyerarşik bir ad alanı olan hesaplarla birlikte kullanmayla ilişkili birkaç sorun vardır. İş yüklerinizde [Azure blob dosya sistemi (ABFS)](data-lake-storage-abfs-driver.md) sürücüsünü kullanmanızı öneririz. 





