---
title: Azure Data Lake Storage 2. üzerinde ara (Önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Data Lake Storage 2. içerik ve meta verileri nasıl dizinleyeceğinizi öğrenin. Bu özellik şu anda genel önizlemede
manager: nitinme
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 8e86721d9f8644adabd1e01920c432217354d654
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85556221"
---
# <a name="indexing-documents-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. belgeleri dizine ekleme

> [!IMPORTANT] 
> Azure Data Lake Storage 2. desteği şu anda genel önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)doldurarak önizlemelere erişim isteğinde bulabilirsiniz. [REST API sürüm 2020-06-30-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.


Bir Azure depolama hesabı ayarlarken, [hiyerarşik ad alanını](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace)etkinleştirme seçeneğiniz vardır. Bu, bir hesaptaki içerik koleksiyonunun bir Dizin hiyerarşisinde ve iç içe yerleştirilmiş alt dizinlerde düzenlenmesine olanak sağlar. Hiyerarşik ad alanını etkinleştirerek [Azure Data Lake Storage 2.](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)etkinleştirirsiniz.

Bu makalede, Azure Data Lake Storage 2. olan belgeleri dizine almaya nasıl başlacağınız açıklanır.

## <a name="set-up-azure-data-lake-storage-gen2-indexer"></a>Azure Data Lake Storage 2. Dizin oluşturucuyu ayarlama

Data Lake Storage 2. içeriği indekslemek için gerçekleştirmeniz gereken birkaç adım vardır.

### <a name="step-1-sign-up-for-the-preview"></a>1. Adım: önizleme için kaydolun

[Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)doldurarak Data Lake Storage 2. Dizin Oluşturucu önizlemesine kaydolun. Önizlemeye kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="step-2-follow-the-azure-blob-storage-indexing-setup-steps"></a>2. Adım: Azure Blob depolama dizini oluşturma kurulum adımlarını Izleyin

Önizlemenin kayıt işleminin başarılı olduğunu belirten bir onay aldıktan sonra, dizin oluşturma işlem hattını oluşturmaya hazırsınız demektir.

Data Lake Storage 2. [REST API sürüm 2020-06-30-önizleme](search-api-preview.md)kullanarak içerik ve meta verileri dizinleyebilir. Şu anda portal veya .NET SDK desteği yok.

Data Lake Storage 2. içerik dizini oluşturma, Azure Blob depolamada dizin oluşturma içeriğiyle aynıdır. Data Lake Storage 2. veri kaynağını, dizini ve Dizin oluşturucuyu ayarlamayı öğrenmek için Azure [bilişsel arama Ile Azure Blob depolamada belgelerin nasıl dizinlebileceğini](search-howto-indexing-azure-blob-storage.md)inceleyin. BLOB depolama makalesinde hangi belge biçimlerinin desteklendiği, hangi blob meta veri özelliklerinin ayıklandığı, artımlı dizin oluşturma ve daha fazlası hakkında bilgiler de sağlanmaktadır. Bu bilgiler Data Lake Storage 2. için aynı olacaktır.

## <a name="access-control"></a>Erişim denetimi

Azure Data Lake Storage 2. hem Azure rol tabanlı erişim denetimi (RBAC) hem de POSIX benzeri erişim denetim listelerini (ACL 'Ler) destekleyen bir [erişim denetimi modeli](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control) uygular. Data Lake Storage 2. içerik dizinlenirken, Azure Bilişsel Arama, içerikten RBAC ve ACL bilgilerini ayıklamaz. Sonuç olarak, bu bilgiler Azure Bilişsel Arama dizinine dahil edilmez.

Dizindeki her belge üzerinde erişim denetimini korumak önemli ise, [güvenlik kırpması](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search)uygulamak için uygulama geliştiricisinin üzerine gelir.

## <a name="change-detection"></a>Değişiklik algılama

Data Lake Storage 2. Indexer değişiklik algılamayı destekler. Bu, dizin oluşturucunun onu çalıştırması durumunda yalnızca blob 'un zaman damgasıyla belirlendiği şekilde değiştirilen Blobları yeniden dizinleyen anlamına gelir `LastModified` .

> [!NOTE] 
> Data Lake Storage 2. dizinlerin yeniden adlandırılmasına izin verir. Bir dizin yeniden adlandırıldığında, bu dizindeki Blobların zaman damgaları güncellenmez. Sonuç olarak, Dizin Oluşturucu bu Blobları yeniden kullanmaz. Bir dizin yeniden adlandırıldıktan sonra dizin yeniden Dizinlenecek olan Blobların, artık yeni URL 'Ler olduğundan, Dizin `LastModified` oluşturucunun ileride çalıştırılan bir çalışma sırasında yeniden dizine alınmasını bilmesi için, dizindeki tüm Bloblar için zaman damgasını güncelleştirmeniz gerekir.
