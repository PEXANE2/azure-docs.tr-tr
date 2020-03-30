---
title: Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen2'nin sınırlamaları ve bilinen sorunları hakkında bilgi edinin.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 4f8fae6580272ed53b8d440ba3e74c6a1ed1e61a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061502"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar

Bu makalede, Azure Veri Gölü Depolama Gen2'nin sınırlamaları ve bilinen sorunları açıklanmaktadır.

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

Giderek artan sayıda Blob depolama özelliği artık hiyerarşik ad alanına sahip hesaplarla çalışır. Tam liste için Azure [Veri Gölü Depolama Gen2'de bulunan Blob Depolama özelliklerine](data-lake-storage-supported-blob-storage-features.md)bakın.

## <a name="supported-azure-service-integrations"></a>Desteklenen Azure hizmet tümleştirmeleri

Veri Gölü Depolama gen2, verileri yutmak, analitik gerçekleştirmek ve görsel gösterimler oluşturmak için kullanabileceğiniz birkaç Azure hizmetlerini destekler. Desteklenen Azure hizmetlerinin listesi için, [Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetlerine](data-lake-storage-supported-azure-services.md)bakın.

[Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetlerine](data-lake-storage-supported-azure-services.md)bakın.

## <a name="supported-open-source-platforms"></a>Desteklenen açık kaynak platformları

Çeşitli açık kaynak platformları Data Lake Storage Gen2'yi destekler. Tam liste için bkz: [Azure Veri Gölü Depolama Gen2'yi destekleyen Açık kaynak platformları.](data-lake-storage-supported-open-source-platforms.md)

Bkz. [Azure Veri Gölü Depolama Gen2'yi destekleyen açık kaynak platformlar.](data-lake-storage-supported-open-source-platforms.md)

## <a name="blob-storage-apis"></a>Blob depolama API'leri

Blob API'leri ve Veri Gölü Depolama Gen2 API'leri aynı veriler üzerinde çalışabilir.

Bu bölümde, aynı veriler üzerinde çalışmak için blob API'leri ve Veri Gölü Depolama Gen2 API'leri kullanılarak ilgili sorunlar ve sınırlamalar açıklanmaktadır.

* Bir dosyanın aynı örneğine yazmak için hem Blob API'lerini hem de Veri Gölü Depolama API'lerini kullanamazsınız. Veri Gölü Depolama Gen2 API'lerini kullanarak bir dosyaya yazarsanız, bu dosyanın blokları [Blok Listesi blob](https://docs.microsoft.com/rest/api/storageservices/get-block-list) API'sına yapılan çağrılara görünmez. Veri Gölü Depolama Gen2 API'lerini veya Blob API'lerini kullanarak dosyanın üzerine yazabilirsiniz. Bu, dosya özelliklerini etkilemez.

* [Liste Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) işlemini bir sınır layıcı belirtmeden kullandığınızda, sonuçlar hem dizinleri hem de blobları içerir. Bir sınır layıcı kullanmayı seçerseniz, yalnızca ileri`/`eğik çizgi (). Desteklenen tek delimiter bu.

* Bir dizini silmek için [Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API'sini sil'i kullanırsanız, bu dizin yalnızca boşsa silinir. Bu, Blob API silme dizinlerini özyinelemeli olarak kullanamadığınız anlamına gelir.

Bu Blob REST API'leri desteklenmez:

* [Blob Koyun (Sayfa)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Sayfa Koy](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Sayfa Aralıklarını Alın](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Artımlı Kopya Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [URL'den Sayfa Koy](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Blob Koyun (Ek)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Ek Blok](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [URL'den Ek Engelleme](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

Yönetilmeyen VM diskleri hiyerarşik ad alanına sahip hesaplarda desteklenmez. Depolama hesabında hiyerarşik bir ad alanı etkinleştirmek istiyorsanız, yönetilmeyen VM disklerini hiyerarşik ad alanı özelliği etkin olmayan bir depolama hesabına yerleştirin.

<a id="api-scope-data-lake-client-library" />

## <a name="file-system-support-in-sdks"></a>SDK'larda dosya sistemi desteği

ACL işlemlerini alın ve ayarlayın şu anda özyinelemeli değildir.

## <a name="file-system-support-in-powershell-and-azure-cli"></a>PowerShell ve Azure CLI'de dosya sistemi desteği

- [PowerShell](data-lake-storage-directory-file-acl-powershell.md) ve [Azure CLI](data-lake-storage-directory-file-acl-cli.md) desteği genel önizlemede yer alıyor.
- ACL işlemlerini alın ve ayarlayın şu anda özyinelemeli değildir.

## <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetimi politikaları

* Blob anlık görüntüleri silme henüz desteklenmez.  

## <a name="archive-tier"></a>Arşiv Katmanı

Şu anda arşiv erişim katmanını etkileyen bir hata var.


## <a name="blobfuse"></a>Blobfuse

Blobfuse desteklenmez.

<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

AzCopy sadece en son sürümünü kullanın ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)).AzCopy v8.1 gibi AzCopy önceki sürümleri desteklenmez.

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Azure Storage Gezgini

Yalnızca sürümleri `1.6.0` veya daha yüksek sürümleri kullanın.

<a id="explorer-in-portal" />

## <a name="storage-explorer-in-the-azure-portal"></a>Azure portalında Depolama Gezgini

ALAK'lar henüz desteklenmedi.

<a id="third-party-apps" />

## <a name="thirdpartyapplications"></a>Üçüncü taraf uygulamaları

Çalışmak için REST API'lerini kullanan üçüncü taraf uygulamalar, Blob API'lerini arayan Veri Gölü Depolama Gen2 Uygulamaları yla kullanırsanız çalışmaya devam eder.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Erişim denetim listeleri (ACL) ve anonim okuma erişimi

Bir kapsayıcıya [anonim okuma erişimi](storage-manage-access-to-resources.md) verilmişse, ALAK'ların o kapsayıcı veya o kapsayıcıdaki dosyalar üzerinde hiçbir etkisi yoktur.

## <a name="windows-azure-storage-blob-wasb-driver"></a>Windows Azure Depolama Blob (WASB) sürücüsü

Şu anda, hiyerarşik ad alanına sahip hesaplarla birlikte WASB sürücüsünün kullanımıyla ilişkili çeşitli sorunlar vardır. İş yüklerinizde [Azure Blob Dosya Sistemi (ABFS)](data-lake-storage-abfs-driver.md) sürücüsünü kullanmanızı öneririz. 





