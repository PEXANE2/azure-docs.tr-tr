---
title: Öğretici - Azure depolama kuyruklarıyla çalışın - Azure Depolama
description: Kuyruk oluşturmak ve ileti eklemek, almak ve silmek için Azure Kuyruk hizmetini nasıl kullanacağınıza ilişkin bir öğretici.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2019
ms.service: storage
ms.subservice: queues
ms.topic: tutorial
ms.reviewer: cbrooks
ms.openlocfilehash: 9cbdc5231fdc9f836f300b1a3a81a237a9efc123
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75968207"
---
# <a name="tutorial-work-with-azure-storage-queues"></a>Öğretici: Azure depolama kuyruklarıyla çalışın

Azure Sıra depolama, dağıtılmış bir uygulamanın bileşenleri arasında iletişimi etkinleştirmek için bulut tabanlı kuyruklar uygular. Her kuyruğa, gönderen bileşeni tarafından eklenebilen ve bir alıcı bileşeni tarafından işlenebilen iletilerin bir listesini tutar. Bir sıra yla, uygulamanız talebi karşılamak için hemen ölçeklendirilebilir. Bu makalede, Bir Azure depolama sırası ile çalışmak için temel adımları gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> - Azure Storage hesabı oluşturma
> - Uygulama oluşturma
> - Eşzamanlı kod için destek ekleme
> - Bir kuyruk oluşturma
> - İletileri kuyruğa ekleme
> - İletileri sıradan ayır
> - Boş bir sırayı silme
> - Komut satırı bağımsız değişkenlerini denetleme
> - Uygulamayı derleme ve çalıştırma

## <a name="prerequisites"></a>Ön koşullar

- Çapraz platform Visual Studio [Code](https://code.visualstudio.com/download) editörü ücretsiz kopyasını alın.
- [.NET Core SDK'yı](https://dotnet.microsoft.com/download)indirin ve kurun.
- Geçerli bir Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

İlk olarak, bir Azure depolama hesabı oluşturun. Depolama hesabı oluşturmak için adım adım kılavuz için, [depolama hesabı oluşturma](../common/storage-account-create.md?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json) hızlı başlat'ına bakın.

## <a name="create-the-app"></a>Uygulama oluşturma

**QueueApp**adında bir .NET Core uygulaması oluşturun. Basitlik için, bu uygulama hem gönderilecek hem de sıra üzerinden ileti alır.

1. Konsol penceresinde (CMD, PowerShell veya Azure CLI gibi), `dotnet new` **QueueApp**adında yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: **Program.cs**.

   ```console
   dotnet new console -n QueueApp
   ```

2. Yeni oluşturulan **QueueApp** klasörüne geçin ve her şeyin yolunda olduğunu doğrulamak için uygulamayı oluşturun.

   ```console
   cd QueueApp
   ```

   ```console
   dotnet build
   ```

   Aşağıdakilere benzer sonuçlar görmeniz gerekir:

   ```output
   C:\Tutorials>dotnet new console -n QueueApp
   The template "Console Application" was created successfully.

   Processing post-creation actions...
   Running 'dotnet restore' on QueueApp\QueueApp.csproj...
     Restore completed in 155.62 ms for C:\Tutorials\QueueApp\QueueApp.csproj.

   Restore succeeded.

   C:\Tutorials>cd QueueApp

   C:\Tutorials\QueueApp>dotnet build
   Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

     Restore completed in 40.87 ms for C:\Tutorials\QueueApp\QueueApp.csproj.
     QueueApp -> C:\Tutorials\QueueApp\bin\Debug\netcoreapp2.1\QueueApp.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:02.40

   C:\Tutorials\QueueApp>_
   ```

## <a name="add-support-for-asynchronous-code"></a>Eşzamanlı kod için destek ekleme

Uygulama bulut kaynaklarını kullandığından, kod eşsenkronize çalışır. Ancak, C#'S **async** ve **await** C # 7.1 kadar **Ana** yöntemlerde geçerli anahtar kelimeler değildi. **Csproj** dosyasındaki bir bayrak aracılığıyla derleyiciye kolayca geçebilirsiniz.

1. Proje dizinindeki komut satırından, `code .` geçerli dizindeki Visual Studio Code'u açmak için yazın. Komut satırı penceresini açık tutun. Daha sonra yürütmek için daha fazla komut olacaktır. Oluşturmak ve hata ayıklamak için C# varlıkları eklemeniz istenirse, **Evet** düğmesini tıklatın.

2. **QueueApp.csproj** dosyasını düzenleyicide açın.

3. Yapı `<LangVersion>7.1</LangVersion>` dosyasındaki ilk **Özellik Grubu'na** ekleyin. **TargetFramework'ünüzün** hangi .NET sürümünü yüklediğinize bağlı olarak farklı olabileceğinden **langversion** etiketini eklediğinizden emin olun.

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">

     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp2.1</TargetFramework>
       <LangVersion>7.1</LangVersion>
     </PropertyGroup>

   ...

   ```

4. **QueueApp.csproj** dosyasını kaydedin.

5. kaynak **dosyayı Program.cs** açın ve eş senkronize çalışması için **Ana** yöntemi güncelleştirin. **Boşluğu** bir **eşitleme Görev** iade değeriyle değiştirin.

   ```csharp
   static async Task Main(string[] args)
   ```

6. **Program.cs** dosyasını kaydedin.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

1. **Microsoft.Azure.Storage.Common** ve **Microsoft.Azure.Storage.Queue** paketlerini komutla `dotnet add package` projeye yükleyin. Konsol penceresindeki proje klasöründen aşağıdaki dotnet komutlarını çalıştırın.

   ```console
   dotnet add package Microsoft.Azure.Storage.Common
   dotnet add package Microsoft.Azure.Storage.Queue
   ```

2. **Program.cs** dosyasının üst kısmında, deyimden hemen sonra `using System;` aşağıdaki ad boşluklarını ekleyin. Bu uygulama, Azure Depolama'ya bağlanmak ve kuyruklarla çalışmak için bu ad alanlarından türler kullanır.

   ```csharp
   using System.Threading.Tasks;
   using Microsoft.Azure.Storage;
   using Microsoft.Azure.Storage.Queue;
   ```

3. **Program.cs** dosyasını kaydedin.

### <a name="get-your-connection-string"></a>Bağlantı dizenizi alma

İstemci kitaplığı, bağlantınızı kurmak için bir bağlantı dizesi kullanır. Bağlantı dizeniz, Azure portalındaki depolama hesabınızın **Ayarlar** bölümünde kullanılabilir.

1. Web tarayıcınızda [Azure portalında](https://portal.azure.com/)oturum açın.

2. Azure portalda depolama hesabınıza gidin.

3. **Erişim tuşlarını**seçin.

4. **Bağlantı dizesi** alanının sağındaki **Kopyala** düğmesini tıklatın.

![Bağlantı dizesi](media/storage-tutorial-queues/get-connection-string.png)

Bağlantı dizesi bu biçimdedir:

   ```
   "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
   ```

### <a name="add-the-connection-string-to-the-app"></a>Bağlantı dizesini uygulamaya ekleme

Depolama hesabına erişebilmek için bağlantı dizesini uygulamaya ekleyin.

1. Visual Studio Code'a geri dön.

2. **Program** sınıfında, bağlantı `private const string connectionString =` dizesini tutmak için bir üye ekleyin.

3. Eşit işaretten sonra, Azure portalınızda daha önce kopyaladığınız dize değerini yapıştırın. **ConnectionString** değeri hesabınıza özgü olacaktır.

4. **Main'den**"Hello World" kodunu kaldırın. Kodunuz aşağıdakilere benzer ancak benzersiz bağlantı dize değeri ile görünmelidir.

   ```csharp
   namespace QueueApp
   {
       class Program
       {
           private const string connectionString = "DefaultEndpointsProtocol=https; ...";

           static async Task Main(string[] args)
           {
           }
       }
   }
   ```

5. Daha sonra gönderme ve alma yöntemlerine geçirilen bir **CloudQueue** nesnesi oluşturmak için **Main'i** güncelleştirin.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");
        }
   ```

6. Dosyayı kaydedin.

## <a name="insert-messages-into-the-queue"></a>Sıraya ileti ekleme

Sıraya ileti göndermek için yeni bir yöntem oluşturun. **Program** sınıfınıza aşağıdaki yöntemi ekleyin. Bu yöntem bir sıra başvurusu alır, sonra [createifNotExistsAsync'i](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.createifnotexistsasync)arayarak zaten yoksa yeni bir kuyruk oluşturur. Daha sonra [AddMessageAsync'i](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessageasync)arayarak iletiyi kuyruğa ekler.

1. **Program** sınıfınıza aşağıdaki **SendMessageAsync** yöntemini ekleyin.

   ```csharp
   static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
   {
       bool createdQueue = await theQueue.CreateIfNotExistsAsync();

       if (createdQueue)
       {
           Console.WriteLine("The queue was created.");
       }

       CloudQueueMessage message = new CloudQueueMessage(newMessage);
       await theQueue.AddMessageAsync(message);
   }
   ```

2. Dosyayı kaydedin.

İleti UTF-8 kodlaması olan bir XML isteğine dahil edilebilecek bir biçimde olmalıdır ve 64 KB boyutuna kadar olabilir. İleti ikili veri içeriyorsa, iletiyi Base64-kodlamanızı öneririz.

Varsayılan olarak, bir ileti için en fazla canlı süre 7 gün olarak ayarlanır. İleti nin zaman içinde yaşanması için herhangi bir pozitif sayı belirtebilirsiniz. Süresi dolmayan bir ileti eklemek `Timespan.FromSeconds(-1)` için **AddMessageAsync'e**çağrınızda kullanın.

```csharp
await theQueue.AddMessageAsync(message, TimeSpan.FromSeconds(-1), null, null, null);
```

## <a name="dequeue-messages"></a>İletileri sıradan ayır

**ReceiveMessageAsync**adlı yeni bir yöntem oluşturun. Bu yöntem [GetMessageAsync'i](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessageasync)arayarak kuyruktan bir ileti alır. İleti başarıyla alındıktan sonra, birden fazla kez işlenmeyecek şekilde sıradan silinmeniz önemlidir. İleti alındıktan sonra [DeleteMessageAsync'i](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessageasync)arayarak sil.

1. **Program** sınıfınıza aşağıdaki **ReceiveMessageAsync** yöntemini ekleyin.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
       }
   }
   ```

2. Dosyayı kaydedin.

## <a name="delete-an-empty-queue"></a>Boş bir sırayı silme

Oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek için projenin sonundaki en iyi uygulamadır. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kuyruk varsa ancak boşsa, kullanıcıya silmek isteyip istemediklerini sorun.

1. Boş sırayı silmek için bir istem içerecek şekilde **ReceiveMessageAsync** yöntemini genişletin.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
           else
           {
               Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
               string response = Console.ReadLine();

               if (response == "Y" || response == "y")
               {
                   await theQueue.DeleteIfExistsAsync();
                   return "The queue was deleted.";
               }
               else
               {
                   return "The queue was not deleted.";
               }
           }
       }
       else
       {
           return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
       }
   }
   ```

2. Dosyayı kaydedin.

## <a name="check-for-command-line-arguments"></a>Komut satırı bağımsız değişkenlerini denetleme

Uygulamaya geçirilen komut satırı bağımsız değişkenleri varsa, bunların sıraya eklenecek bir ileti olduğunu varsayalım. Bir dize yapmak için bağımsız değişkenleri birleştirin. Daha önce eklediğimiz **SendMessageAsync** yöntemini arayarak bu dizeyi ileti kuyruğuna ekleyin.

Komut satırı bağımsız değişkenleri yoksa, bir geri alma işlemi yürütün. Kuyruktaki ilk iletiyi almak için **ReceiveMessageAsync** yöntemini arayın.

Son olarak, **Console.ReadLine'ı**arayarak çıkmadan önce kullanıcı girişi bekleyin.

1. Komut satırı bağımsız değişkenlerini denetlemek ve kullanıcı girişini beklemek için **Ana** yöntemi genişletin.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received: {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }
   ```

2. Dosyayı kaydedin.

## <a name="complete-code"></a>Kodu tamamla

İşte bu projenin tam kod listesi.

   ```csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.Azure.Storage;
   using Microsoft.Azure.Storage.Queue;

   namespace QueueApp
   {
    class Program
    {
        // The string value is broken up for better onscreen formatting
        private const string connectionString = "DefaultEndpointsProtocol=https;" +
                                                "AccountName=<your storage account name>;" +
                                                "AccountKey=<your key>;" +
                                                "EndpointSuffix=core.windows.net";

        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }

        static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
        {
            bool createdQueue = await theQueue.CreateIfNotExistsAsync();

            if (createdQueue)
            {
                Console.WriteLine("The queue was created.");
            }

            CloudQueueMessage message = new CloudQueueMessage(newMessage);
            await theQueue.AddMessageAsync(message);
        }

        static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
        {
            bool exists = await theQueue.ExistsAsync();

            if (exists)
            {
                CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

                if (retrievedMessage != null)
                {
                    string theMessage = retrievedMessage.AsString;
                    await theQueue.DeleteMessageAsync(retrievedMessage);
                    return theMessage;
                }
                else
                {
                    Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
                    string response = Console.ReadLine();

                    if (response == "Y" || response == "y")
                    {
                        await theQueue.DeleteIfExistsAsync();
                        return "The queue was deleted.";
                    }
                    else
                    {
                        return "The queue was not deleted.";
                    }
                }
            }
            else
            {
                return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
            }
        }
    }
   }
   ```

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Proje dizinindeki komut satırından, projeyi oluşturmak için aşağıdaki dotnet komutunu çalıştırın.

   ```console
   dotnet build
   ```

2. Proje başarıyla inşa ettikten sonra, sıraya ilk iletiyi eklemek için aşağıdaki komutu çalıştırın.

   ```console
   dotnet run First queue message
   ```

Bu çıkışı görmelisiniz:

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter..._
   ```

3. Sırada ilk iletiyi almak ve kaldırmak için komut satırı bağımsız değişkeni olmadan uygulamayı çalıştırın.

   ```console
   dotnet run
   ```

4. Tüm iletiler kaldırılana kadar uygulamayı çalıştırmaya devam edin. Bir kez daha çalıştıran, sıraboş bir ileti ve sırayı silmek için bir istemi alırsınız.

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Second queue message
   Sent: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Third queue message
   Sent: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   The queue is empty. Attempt to delete it? (Y/N) Y
   Received: The queue was deleted.
   Press Enter...

   C:\Tutorials\QueueApp>_
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

1. Bir kuyruk oluşturma
2. İletileri kuyruktan ekleme ve kaldırma
3. Azure depolama sıranı silme

Daha fazla bilgi için Azure kuyruklarına hızlı bir şekilde başlayın.

> [!div class="nextstepaction"]
> [Kuyruklar hızlı başlat](storage-quickstart-queues-portal.md)
