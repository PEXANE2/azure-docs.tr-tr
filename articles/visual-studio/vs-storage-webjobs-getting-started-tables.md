---
title: Visual Studio (WebJob projeleri) kullanarak Azure depolamasına Başlarken
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki bir Azure Webİşler projesinde Azure Tablo depolama sını kullanmaya başlama
services: storage
author: ghogen
manager: jillfra
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e4d8299c06bfa5b0f33bff8fa592a2fa549c695c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74707610"
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Azure Depolama (Azure WebJob Projeleri) ile Başlarken

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure tablo depolama hizmetiyle Azure Web İşleri SDK sürüm 1.x'in nasıl kullanılacağını gösteren C# kodu örnekleri verilmektedir. Kod örnekleri [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) sürüm 1.x'i kullanır.

Azure Tablo depolama hizmeti, büyük miktarda yapılandırılmış veri depolamanıza olanak tanır. Hizmet, Azure bulutu içinden ve dışından kimlik doğrulamalı aramaları kabul eden bir NoSQL veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.  Bkz. Daha fazla bilgi için [.NET'i kullanarak Azure Tablo depolama alanına başlayın.](../cosmos-db/tutorial-develop-table-dotnet.md#create-a-table)

Bazı kod parçacıkları, tetikleyici özniteliklerden birini kullanarak değil, el ile çağrılan işlevlerde kullanılan **Tablo** özniteliğini gösterir.

## <a name="how-to-add-entities-to-a-table"></a>Tabloya varlıklar ekleme

Tabloya varlıklar eklemek için Tablo **Table** özniteliğini, Eklemek istediğiniz varlıkların şemasını **T'nin** belirttiği **iCollector\<T>** veya **\<IAsyncCollector T>** parametresi ile kullanın. Öznitelik oluşturucu, tablonun adını belirten bir dize parametresi alır.

Aşağıdaki kod örneği *Giriş*adlı bir tabloya **Kişi** varlıkları ekler.

```csharp
[NoAutomaticTrigger]
public static void IngressDemo(
    [Table("Ingress")] ICollector<Person> tableBinding)
{
    for (int i = 0; i < 100000; i++)
    {
        tableBinding.Add(
            new Person() {
                PartitionKey = "Test",
                RowKey = i.ToString(),
                Name = "Name" }
            );
    }
}
```

Genellikle **ICollector** ile kullandığınız tür **TableEntity** türetilmiştir veya **ITableEntity**uygular, ancak bunu yapmak zorunda değildir. Aşağıdaki **Kişi** sınıflarından biri önceki **Giriş** yönteminde gösterilen kodla çalışır.

```csharp
public class Person : TableEntity
{
    public string Name { get; set; }
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Doğrudan Azure depolama API'si ile çalışmak istiyorsanız, yöntem imzasına bir **CloudStorageAccount** parametresi ekleyebilirsiniz.

## <a name="real-time-monitoring"></a>Gerçek zamanlı izleme

Veri giriş işlevleri genellikle büyük hacimli verileri işlediğinden, WebJobs SDK panosu gerçek zamanlı izleme verileri sağlar. **Çağırma Günlüğü** bölümü, işlevin hala çalışıp çalışmadığınızı bildirir.

![Giriş fonksiyonu çalışıyor](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

**Çağrı Ayrıntıları** sayfası, çalışırken işlevin ilerlemesini (yazılan varlık sayısı) bildirir ve iptal etme fırsatı verir.

![Giriş fonksiyonu çalışıyor](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

İşlev sona erdiğinde, **Çağrı Ayrıntıları** sayfası yazılan satır sayısını bildirir.

![Giriş fonksiyonu tamamlandı](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a>Bir tablodan birden çok varlık nasıl okunur?

Tabloyu okumak için **Table** Tablo özniteliğini, **T** türünün **TableEntity'den** türediği veya **ITableEntity'i**uyguladığı **IQueryable\<T>** parametresi ile kullanın.

Aşağıdaki kod örneği **Giriş** tablosundaki tüm satırları okur ve kaydeder:

```csharp
public static void ReadTable(
    [Table("Ingress")] IQueryable<Person> tableBinding,
    TextWriter logger)
{
    var query = from p in tableBinding select p;
    foreach (Person person in query)
    {
        logger.WriteLine("PK:{0}, RK:{1}, Name:{2}",
            person.PartitionKey, person.RowKey, person.Name);
    }
}
```

### <a name="how-to-read-a-single-entity-from-a-table"></a>Tablodan tek bir varlık nasıl okunur

Tek bir tablo varlığına bağlamak istediğinizde bölüm anahtarı nı ve satır anahtarını belirtmenize izin veren iki ek parametreye sahip bir **Tablo** özniteliği oluşturucu vardır.

Aşağıdaki kod örneği, bir **kişi** varlığı için bir satır satırı, sıra iletisinde alınan bölüm anahtarı ve satır anahtar değerlerini temel alan bir tablo satırını okur:

```csharp
public static void ReadTableEntity(
    [QueueTrigger("inputqueue")] Person personInQueue,
    [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
    TextWriter logger)
{
    if (personInTable == null)
    {
        logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                personInQueue.PartitionKey, personInQueue.RowKey);
    }
    else
    {
        logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
    }
}
```

Bu örnekte **Kişi** sınıfı **ITableEntity**uygulamak zorunda değildir.

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a>Bir tabloyla çalışmak için .NET Depolama API'sini doğrudan kullanma

**Tabloyla** çalışırken daha fazla esneklik için **CloudTable** nesnesi ile Tablo özniteliğini de kullanabilirsiniz.

Aşağıdaki kod örneği, *Giriş* tablosuna tek bir varlık eklemek için bir **CloudTable** nesnesi kullanır.

```csharp
public static void UseStorageAPI(
    [Table("Ingress")] CloudTable tableBinding,
    TextWriter logger)
{
    var person = new Person()
        {
            PartitionKey = "Test",
            RowKey = "100",
            Name = "Name"
        };
    TableOperation insertOperation = TableOperation.Insert(person);
    tableBinding.Execute(insertOperation);
}
```

**CloudTable** nesnesinin nasıl kullanılacağı hakkında daha fazla bilgi için [bkz.](../storage/storage-dotnet-how-to-use-tables.md)

## <a name="related-topics-covered-by-the-queues-how-to-article"></a>Kuyruklar tarafından kapsanan ilgili konular nasıl-makale

Bir sıra iletisi tarafından tetiklenen tablo işlemenin nasıl işleyeceğiniz veya tablo işlemeye özgü olmayan Webİşler SDK senaryoları hakkında bilgi [için](../storage/vs-storage-webjobs-getting-started-queues.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tabloları ile çalışmak için ortak senaryoların nasıl işleyeceğini gösteren kod örnekleri sağlanmıştır. Azure Web İşleri ve Web İşler SDK'nın nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Web İşleri dokümankaynakları'na](https://go.microsoft.com/fwlink/?linkid=390226)bakın.
