---
title: Python kullanarak Azure Depolama örnekleri | Microsoft Dokümanlar
description: Azure Depolama için örnek kodu ve uygulamaları görüntüleyin, indirin ve çalıştırın. Python depolama istemci kitaplıklarını kullanarak blobs, kuyruklar, tablolar ve dosyalar için başlangıç örnekleri alma keşfedin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 02/13/2020
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: fc3079d316a252bc5a658017cb6b04fe6ef20c2f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77485837"
---
# <a name="azure-storage-samples-using-v12-python-client-libraries"></a>v12 Python istemci kitaplıklarını kullanarak Azure Depolama örnekleri

Aşağıdaki tablolar, numune depomuzun ve her örnekte kapsanan senaryoların genel görünümünü sağlar. GitHub'da ilgili örnek kodu görüntülemek için bağlantıları tıklatın.

> [!NOTE]
> Bu örnekler en son Azure Depolama .NET v12 kitaplığını kullanır. Eski v2.1 kodu için bkz: Azure Depolama: GitHub deposunda [Python'da Azure Depolama ile Başlarken.](https://github.com/Azure-Samples/storage-blob-python-getting-started)

## <a name="blob-samples"></a>Blob örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Bağlantı dizesi kullanarak blob servis istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L45)
   :::column-end:::
   :::column span="":::
      [Bağlantı dizesi kullanarak kapsayıcı istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L50)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Bağlantı dizesi kullanarak blob istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L56)
   :::column-end:::
   :::column span="":::
      [Paylaşılan erişim anahtarını kullanarak blob servis istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L66)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [URL'den blob istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L75)
   :::column-end:::
   :::column span="":::
      [Blob istemcisi SAS URL'si oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L80)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [ClientSecretCredential kullanarak blob servis istemcisi oluşturun](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L88)
   :::column-end:::
   :::column span="":::
      [SAS belirteci oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L110)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Azure Kimliği'ni kullanarak blob servis istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_authentication.py#L125)
   :::column-end:::
   :::column span="":::
      [Blob anlık görüntüsü oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L56)
   :::column-end:::
:::row-end:::

### <a name="blob-service"></a>Blob hizmeti

:::row:::
   :::column span="":::
      [Blob servis hesap bilgilerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L30)
   :::column-end:::
   :::column span="":::
      [Blob servis özelliklerini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L41)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Blob servis özelliklerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L61)
   :::column-end:::
   :::column span="":::
      [Blob servis istatistiklerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L71)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Servis istemcisi kullanarak kapsayıcı oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L82)
   :::column-end:::
   :::column span="":::
      [Kapsayıcıları listeleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L90)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Servis istemcisi kullanarak kapsayıcıyı silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L103)
   :::column-end:::
   :::column span="":::
      [Konteyner istemcisi alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L117)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Blob istemcisi alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_service.py#L130)
   :::column-end:::
:::row-end:::

### <a name="container"></a>Kapsayıcı

:::row:::
   :::column span="":::
      [Hizmetten kapsayıcı istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L35)
   :::column-end:::
   :::column span="":::
      [SAS URL'yi kullanarak kapsayıcı istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L44)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Kapsayıcı istemcisi kullanarak kapsayıcı oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L52)
   :::column-end:::
   :::column span="":::
      [Kapsayıcı özelliklerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L56)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Kapsayıcı istemcisi kullanarak kapsayıcıyı silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L61)
   :::column-end:::
   :::column span="":::
      [Konteyner üzerinde kira elde edin](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L80)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Kapsayıcı meta verilerini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L101)
   :::column-end:::
   :::column span="":::
      [Kapsayıcı erişim ilkesini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L128)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Kapsayıcı erişim ilkesini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L141)
   :::column-end:::
   :::column span="":::
      [SAS belirteci oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L145)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [SAS belirteci kullanarak kapsayıcı istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L158)
   :::column-end:::
   :::column span="":::
      [Blob'u konteynere yükleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L182)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Konteynerdeki lekeleri listele](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L189)
   :::column-end:::
   :::column span="":::
      [Blob istemcisi alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_containers.py#L213)
   :::column-end:::
:::row-end:::

### <a name="blob"></a>Blob

:::row:::
   :::column span="":::
      [Blobu karşıya yükleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_hello_world.py#L68)
   :::column-end:::
   :::column span="":::
      [Blob indirme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_hello_world.py#L74)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Lekeyi silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_hello_world.py#L80)
   :::column-end:::
   :::column span="":::
      [Undelete blob](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L100)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Blob özelliklerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L105)
   :::column-end:::
   :::column span="":::
      [Birden çok blobsilme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L133)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Blob'u URL'den kopyala](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L193)
   :::column-end:::
   :::column span="":::
      [URL'den kopyalama blob abort](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L205)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Blob üzerinde kira edinin](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-blob/samples/blob_samples_common.py#L167)
   :::column-end:::
:::row-end:::

## <a name="data-lake-storage-gen2-samples"></a>Data Lake Depolama Gen2 örnekleri

### <a name="data-lake-service"></a>Veri Gölü hizmeti

:::row:::
   :::column span="2":::
      [Veri Gölü hizmet istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L64)
   :::column-end:::
:::row-end:::

### <a name="file-system"></a>Dosya sistemi

:::row:::
   :::column span="":::
      [Dosya sistemi istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L74)
   :::column-end:::
   :::column span="":::
      [Dosya sistemini silme](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L81)
   :::column-end:::
:::row-end:::

### <a name="directory"></a>Dizin

:::row:::
   :::column span="":::
      [Dizin istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L14)
   :::column-end:::
   :::column span="":::
      [Dizin izinleri alma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L19)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dizin izinlerini ayarlama](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L23)
   :::column-end:::
   :::column span="":::
      [Dizini yeniden adlandır](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_directory.py#L19)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dizin özelliklerini alma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_directory.py#L25)
   :::column-end:::
   :::column span="":::
      [Dizini silme](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_directory.py#L29)
   :::column-end:::
:::row-end:::

### <a name="file"></a>Dosya

:::row:::
   :::column span="":::
      [Dosya istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_upload_download.py#L13)
   :::column-end:::
   :::column span="":::
      [Dosya oluşturma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L45)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya izinleri alma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L35)
   :::column-end:::
   :::column span="":::
      [Dosya izinlerini ayarlama](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control.py#L38)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosyaya veri ekle](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_upload_download.py#L18)
   :::column-end:::
   :::column span="":::
      [Dosyadaki verileri okuma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_upload_download.py#L29)
   :::column-end:::
:::row-end:::

## <a name="azure-files-samples"></a>Azure Dosyaları örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Bağlantı dizesinden paylaşım hizmeti istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_authentication.py#L42)
   :::column-end:::
   :::column span="":::
      [Hesaptan paylaşım hizmeti istemcisi oluşturma ve erişim anahtarı](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_authentication.py#L49)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [SAS belirteci oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_authentication.py#L59)
   :::column-end:::
:::row-end:::

### <a name="file-service"></a>Dosya hizmeti

:::row:::
   :::column span="":::
      [Hizmet özelliklerini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_service.py#L39)
   :::column-end:::
   :::column span="":::
      [Hizmet özelliklerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_service.py#L68)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya hizmeti istemcisi kullanarak paylaşım oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_service.py#L77)
   :::column-end:::
   :::column span="":::
      [Dosya hizmeti istemcisi kullanarak paylaşımları listele](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_service.py#L81)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Dosya hizmeti istemcisi kullanarak paylaşımları silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_service.py#L91)
   :::column-end:::
:::row-end:::

### <a name="file-share"></a>Dosya paylaşımı

:::row:::
   :::column span="":::
      [Bağlantı dizesinden paylaşım istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L52)
   :::column-end:::
   :::column span="":::
      [Paylaşım istemcisi alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_service.py#L96)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya paylaşımı istemcisi kullanarak paylaşım oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L39)
   :::column-end:::
   :::column span="":::
      [Paylaşım anlık görüntüsü oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L43)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya paylaşımı istemcisi kullanarak paylaşımı silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L47)
   :::column-end:::
   :::column span="":::
      [Hisse kotası ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L61)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Paylaşım meta verilerini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L66)
   :::column-end:::
   :::column span="":::
      [Paylaşım özelliklerini alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_hello_world.py#L47)
   :::column-end:::
:::row-end:::

### <a name="directory"></a>Dizin

:::row:::
   :::column span="":::
      [Dizin oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L45)
   :::column-end:::
   :::column span="":::
      [Dosyayı dizine yükleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L49)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosyayı dizinden silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L55)
   :::column-end:::
   :::column span="":::
      [Dizini silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L60)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Alt dizin oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L80)
   :::column-end:::
   :::column span="":::
      [Liste dizinleri ve dosyaları](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L96)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Alt dizini silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L104)
   :::column-end:::
   :::column span="":::
      [Alt dizin istemcisi alın](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_directory.py#L121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Dosyaları dizinde listele](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_share.py#L87)
   :::column-end:::
:::row-end:::

### <a name="file"></a>Dosya

:::row:::
   :::column span="":::
      [Dosya istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_hello_world.py#L65)
   :::column-end:::
   :::column span="":::
      [Dosya oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_client.py#L48)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya yükleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_client.py#L54)
   :::column-end:::
   :::column span="":::
      [Dosyayı indirin](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_client.py#L60)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosyayı silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_client.py#L67)
   :::column-end:::
   :::column span="":::
      [DOSYAYı URL'den kopyalama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-share/samples/file_samples_client.py#L101)
   :::column-end:::
:::row-end:::

## <a name="queue-samples"></a>Sıra örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Bağlantı dizelerini kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_authentication.py#L50)
   :::column-end:::
   :::column span="":::
      [Sıra hizmeti istemci belirteci oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_authentication.py#L69)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Bağlantı dizesinden sıra istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L35)
   :::column-end:::
   :::column span="":::
      [Sıra istemcisi SAS belirteci oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L61)
   :::column-end:::
:::row-end:::

### <a name="queue-service"></a>Sıra hizmeti

:::row:::
   :::column span="":::
      [Sıra hizmeti istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_authentication.py#L60)
   :::column-end:::
   :::column span="":::
      [Sıra hizmet özelliklerini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_service.py#L35)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Sıra hizmeti özelliklerini alma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_service.py#L67)
   :::column-end:::
   :::column span="":::
      [Servis istemcisi kullanarak sıra oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_service.py#L76)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Servis istemcisi kullanarak sırayı silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_service.py#L94)
   :::column-end:::
:::row-end:::

### <a name="queue"></a>Kuyruk

:::row:::
   :::column span="":::
      [Sıra istemcisi oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L72)
   :::column-end:::
   :::column span="":::
      [Sıra meta verilerini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L95)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Sıra özelliklerini alma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L100)
   :::column-end:::
   :::column span="":::
      [Sıra istemcisi kullanarak sıra oluşturma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_hello_world.py#L45)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Sıra istemcisi kullanarak sırayı silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_hello_world.py#L62)
   :::column-end:::
   :::column span="":::
      [Liste kuyrukları](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_service.py#L81)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Sıra istemcisi alma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_service.py#L103)
   :::column-end:::
:::row-end:::

### <a name="message"></a>İleti

:::row:::
   :::column span="":::
      [İleti gönderme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L117)
   :::column-end:::
   :::column span="":::
      [İleti alma](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L125)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Peek iletisi](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L197)
   :::column-end:::
   :::column span="":::
      [Güncelleştirme iletisi](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L222)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [İletiyi silme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L165)
   :::column-end:::
   :::column span="":::
      [İletileri temizleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L173)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [İleti erişim ilkesini ayarlama](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-queue/samples/queue_samples_message.py#L47)
   :::column-end:::
:::row-end:::


## <a name="table-samples-sdk-v21"></a>Tablo örnekleri (SDK v2.1)

:::row:::
   :::column span="":::
      [Tablo oluşturma](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_basic_samples.py#L46)
   :::column-end:::
   :::column span="":::
      [Varlığı/tabloyu silme](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_basic_samples.py#L79)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Varlığı ekleme/birleştirme/değiştirme](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_basic_samples.py#L57)
   :::column-end:::
   :::column span="":::
      [Sorgu varlıkları](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_basic_samples.py#L62)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Sorgu tabloları](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_basic_samples.py)
   :::column-end:::
   :::column span="":::
      [Tablo ACL/özellikleri](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_advanced_samples.py#L138)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Varlığı güncelleştir](https://github.com/Azure-Samples/storage-table-python-getting-started/blob/master/table_basic_samples.py#L68)
   :::column-end:::
:::row-end:::

## <a name="azure-code-sample-libraries"></a>Azure kodu örnek kitaplıkları

Python örnek kitaplıklarını tam olarak görüntülemek için şu na gidin:

* [Azure blob kod örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-blob/samples)
* [Azure Veri Gölü kod örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)
* [Azure Dosyaları kod örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-share/samples)
* [Azure sıra kodu örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue/samples)

Her kitaplık için GitHub deposuna göz atabilir ve klonlayabilirsiniz.

## <a name="getting-started-guides"></a>Kılavuzlara başlama

Azure Depolama istemci kitaplıklarını nasıl yükleyip başlatacağınız la ilgili yönergeler arıyorsanız aşağıdaki kılavuzlara göz atın.

* [Python'da Azure Blob Hizmetine Başlarken](../blobs/storage-quickstart-blobs-python.md)
* [Python'da Azure Kuyruk Hizmetiyle Başlarken](../queues/storage-quickstart-queues-python.md)
* [Python'da Azure Tablo Hizmetine Başlarken](../../cosmos-db/table-storage-how-to-use-python.md)
* [Python'da Azure Dosya Hizmetine Başlarken](../files/storage-python-how-to-use-file-storage.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer dillere ait örnekler hakkında bilgi için:

* .NET: [.NET kullanarak Azure Depolama örnekleri](storage-samples-dotnet.md)
* Java: [Java kullanarak Azure Depolama örnekleri](storage-samples-java.md)
* JavaScript/Node.js: [JavaScript kullanarak Azure Depolama örnekleri](storage-samples-javascript.md)
* Diğer tüm diller: [Azure Depolama örnekleri](storage-samples.md)
