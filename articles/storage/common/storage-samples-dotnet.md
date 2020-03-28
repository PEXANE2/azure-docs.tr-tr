---
title: .NET kullanarak Azure Depolama örnekleri | Microsoft Dokümanlar
description: Azure Depolama için örnek kodu ve uygulamaları görüntüleyin, indirin ve çalıştırın. .NET depolama istemcikitaplıklarını kullanarak blobs, kuyruklar, tablolar ve dosyalar için başlangıç örnekleri alma keşfedin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 02/13/2020
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: a4c9188c8f1315a3af452cd0c1fb5cf45ab82081
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77486126"
---
# <a name="azure-storage-samples-using-v12-net-client-libraries"></a>v12 .NET istemci kitaplıklarını kullanarak Azure Depolama örnekleri

Aşağıdaki tablo, numune depomuzun ve her örnekte kapsanan senaryoların genel görünümünü sağlar. GitHub'da ilgili örnek kodu görüntülemek için bağlantıları tıklatın.

> [!NOTE]
> Bu örnekler en son Azure Depolama .NET v12 kitaplığını kullanır. Eski v11 kodu için GitHub deposunda [.NET için Azure Blob Depolama Örnekleri'ne](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started) bakın.

## <a name="blob-samples"></a>Blob örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Bağlantı dizesi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample02_Auth.cs#L27)
   :::column-end:::
   :::column span="":::
      [Paylaşılan bir anahtar kimlik bilgisi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample02_Auth.cs#L91)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Azure Kimliğiyle kimlik doğrulama](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample01a_HelloWorld.cs#L210)
   :::column-end:::
   :::column span="":::
      [Etkin Dizin belirteci kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample02_Auth.cs#L177)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Anonim olarak genel bir blob erişin](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample02_Auth.cs#L55)
   :::column-end:::
:::row-end:::

### <a name="batching"></a>Toplu İşleme

:::row:::
   :::column span="":::
      [Bir istekte birkaç blob silme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample03b_BatchingAsync.cs#L22)
   :::column-end:::
   :::column span="":::
      [Tek bir istekte birkaç blob erişim katmanı ayarlama](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample03b_BatchingAsync.cs#L56)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Toplu iş isteğinde ince taneli denetim](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample03b_BatchingAsync.cs#L90)
   :::column-end:::
   :::column span="":::
      [Başarısız bir alt işlemdeki hataları yakalama](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample03b_BatchingAsync.cs#L136)
   :::column-end:::
:::row-end:::

### <a name="blob"></a>Blob

:::row:::
   :::column span="":::
      [Dosyayı blob'a yükleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample01b_HelloWorldAsync.cs#L21)
   :::column-end:::
   :::column span="":::
      [Dosyaya blob indirme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample01b_HelloWorldAsync.cs#L66)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Resim indirin](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample01b_HelloWorldAsync.cs#L109)
   :::column-end:::
   :::column span="":::
      [Tüm lekeleri bir kapta listele](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample01b_HelloWorldAsync.cs#L128)
   :::column-end:::
:::row-end:::

### <a name="troubleshooting"></a>Sorun giderme
:::row:::
   :::column span="2":::
      [Kapsayıcı istemcisi kullanarak kurtarılabilir bir hatayı tetikleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples/Sample01b_HelloWorldAsync.cs#L166)
   :::column-end:::
:::row-end:::

## <a name="data-lake-storage-gen2-samples"></a>Data Lake Depolama Gen2 örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Genel bir dosyaya anonim olarak erişin](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample02_Auth.cs#L28)
   :::column-end:::
   :::column span="":::
      [Paylaşılan bir anahtar kimlik bilgisi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample02_Auth.cs#L79)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Paylaşılan erişim imzası (SAS) kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample02_Auth.cs#L114)
   :::column-end:::
   :::column span="":::
      [Etkin Dizin belirteci kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample02_Auth.cs#L164)
   :::column-end:::
:::row-end:::

### <a name="file-system"></a>Dosya sistemi
:::row:::
   :::column span="":::
      [Dosya sistemi istemcisi kullanarak dosya oluşturma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L22)
   :::column-end:::
   :::column span="":::
      [Bir dosya ve dizin üzerinde özellikleri alma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L560)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Dosyayı ve dizini yeniden adlandırın](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L511)
   :::column-end:::
:::row-end:::

### <a name="directory"></a>Dizin

:::row:::
   :::column span="":::
      [Dizin oluşturma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L93)
   :::column-end:::
   :::column span="":::
      [Dizin istemcisi kullanarak dosya oluşturma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L55)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Liste dizinleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L275)
   :::column-end:::
   :::column span="":::
      [Dosyaları ve dizinleri geçiş](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L318)
   :::column-end:::
:::row-end:::

### <a name="file"></a>Dosya
:::row:::
   :::column span="":::
      [Dosyayı karşıya yükleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L126)
   :::column-end:::
   :::column span="":::
      [Dosyaya ekleyerek yükleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L169)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya indirme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L224)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Dosya erişim denetim listesini ayarlama ve alma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L468)
   :::column-end:::
   :::column span="":::
      [Dosyayı ayarlama ve izin alma](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L426)
   :::column-end:::
:::row-end:::

### <a name="troubleshooting"></a>Sorun giderme

:::row:::
   :::column span="2":::
      [Kurtarılabilir bir hatayı tetikleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples/Sample01b_HelloWorldAsync.cs#L389)
   :::column-end:::
:::row-end:::

## <a name="azure-files-samples"></a>Azure Dosyaları örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Bağlantı dizesi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample02_Auth.cs#L24)
   :::column-end:::
   :::column span="":::
      [Paylaşılan bir anahtar kimlik bilgisi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample02_Auth.cs#L52)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Paylaşılan erişim imzası (SAS)' kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample02_Auth.cs#L86)
   :::column-end:::
:::row-end:::

### <a name="file-shares"></a>Dosya paylaşımları

:::row:::
   :::column span="":::
      [Paylaşım oluşturma ve dosya yükleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample01b_HelloWorldAsync.cs#L21)
   :::column-end:::
   :::column span="":::
      [Dosya indirme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample01b_HelloWorldAsync.cs#L68)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Dosyaları ve dizinleri geçiş](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample01b_HelloWorldAsync.cs#L107)
   :::column-end:::
:::row-end:::

### <a name="troubleshooting"></a>Sorun giderme

:::row:::
   :::column span="2":::
      [Paylaşım istemcisi kullanarak kurtarılabilir bir hatayı tetikleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples/Sample01b_HelloWorldAsync.cs#L141)
   :::column-end:::
:::row-end:::

## <a name="queue-samples"></a>Sıra örnekleri

### <a name="authentication"></a>Kimlik doğrulaması

:::row:::
   :::column span="":::
      [Azure Active Directory'yi kullanarak kimlik doğrulama](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample01b_HelloWorldAsync.cs#L167)
   :::column-end:::
   :::column span="":::
      [Bağlantı dizesi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample02_Auth.cs#L24)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Paylaşılan bir anahtar kimlik bilgisi kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample02_Auth.cs#L52)
   :::column-end:::
   :::column span="":::
      [Paylaşılan erişim imzası (SAS)' kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample02_Auth.cs#L86)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Etkin Dizin belirteci kullanarak kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample02_Auth.cs#L140)
   :::column-end:::
:::row-end:::

### <a name="queue"></a>Kuyruk

:::row:::
   :::column span="2":::
      [Sıra oluşturma ve ileti ekleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample01b_HelloWorldAsync.cs#L24)
   :::column-end:::
:::row-end:::

### <a name="message"></a>İleti

:::row:::
   :::column span="":::
      [İletileri alma ve işleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample01b_HelloWorldAsync.cs#L61)
   :::column-end:::
   :::column span="":::
      [İletilere göz atın](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample01b_HelloWorldAsync.cs#L90)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [İletileri alma ve görünürlük zaman anına güncelleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample01b_HelloWorldAsync.cs#L115)
   :::column-end:::
:::row-end:::

### <a name="troubleshooting"></a>Sorun giderme 
:::row:::
   :::column span="2":::
      [Sıra istemcisi kullanarak kurtarılabilir bir hatayı tetikleme](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples/Sample01b_HelloWorldAsync.cs#L188)
   :::column-end:::
:::row-end:::

## <a name="table-samples-v11"></a>Tablo örnekleri (v11)

:::row:::
   :::column span="":::
      [Tablo oluşturma](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/Common.cs#L40)
   :::column-end:::
   :::column span="":::
      [Varlığı/tabloyu silme](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Varlığı ekleme/birleştirme/değiştirme](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/SamplesUtils.cs#L41)
   :::column-end:::
   :::column span="":::
      [Sorgu varlıkları](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs#L672)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [Sorgu tabloları](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs)
   :::column-end:::
   :::column span="":::
      [Tablo ACL/özellikleri](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs#L224)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Varlığı güncelleştir](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs#L51)
   :::column-end:::
:::row-end:::

## <a name="azure-code-sample-libraries"></a>Azure kodu örnek kitaplıkları

.NET örnek kitaplıklarını tam olarak görüntülemek için şu

* [Azure blob kod örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples)
* [Azure Veri Gölü kod örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/samples)
* [Azure Dosyaları kod örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.Shares/samples)
* [Azure sıra kodu örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples)

Her kitaplık için GitHub deposuna göz atabilir ve klonlayabilirsiniz.

## <a name="getting-started-guides"></a>Kılavuzlara başlama

Azure Depolama İstemci Kitaplıkları'nı nasıl yükleyip başlatacağınız la ilgili yönergeler arıyorsanız aşağıdaki kılavuzlara göz atın.

* [.NET'te Azure Blob Hizmetine Başlarken](../blobs/storage-quickstart-blobs-dotnet.md)
* [.NET'te Azure Kuyruk Hizmetine Başlarken](../queues/storage-quickstart-queues-dotnet.md)
* [.NET'te Azure Tablo Hizmetine Başlarken](../../cosmos-db/tutorial-develop-table-dotnet.md)
* [.NET'te Azure Dosya Hizmetine Başlarken](../files/storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Sonraki adımlar

Diğer dillere ait örnekler hakkında bilgi için:

* Java: [Java kullanarak Azure Depolama örnekleri](storage-samples-java.md)
* Python: [Python kullanarak Azure Depolama örnekleri](storage-samples-python.md)
* JavaScript/Node.js: [JavaScript kullanarak Azure Depolama örnekleri](storage-samples-javascript.md)
* Diğer tüm diller: [Azure Depolama örnekleri](storage-samples.md)
