---
title: Visual Studio 'u (ASP.NET) kullanarak Azure sıra depolama sına başlayın
description: Visual Studio Connected Services'ı kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki bir ASP.NET projesinde Azure sıra depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/23/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f229661ca78dc75adbc0b49073dc6f0feaf2ba22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75980749"
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Azure sıra depolama ve Visual Studio Bağlantılı Hizmetler (ASP.NET) ile başlayın

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış

Azure sıra depolama, uygulama bileşenleri arasında bulut iletisi sağlar. Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

Bu öğretici, Azure sıra depolama varlıklarını kullanarak bazı yaygın senaryolar için ASP.NET kodu nasıl yazılalışyapılacağını gösterir. Bu senaryolar, Azure sırası oluşturma ve sıra iletilerini ekleme, değiştirme, okuma ve kaldırma gibi yaygın görevleri içerir.

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-account-create.md)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. Çözüm **Gezgini'nde,** **Denetleyicileri**sağ tıklatın ve bağlam menüsünden **Ekle->** Denetleyicisi'ni seçin.

    ![ASP.NET bir MVC uygulamasına denetleyici ekleme](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. İskele **Ekle** iletişim kutusunda **MVC 5 Controller 'ı**seçin - Boş ve **Ekle'yi**seçin.

    ![MVC denetleyici türünü belirtin](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. Denetleyici **Ekle** iletişim kutusunda, denetleyici *QueuesController'ı*adlandırın ve **Ekle'yi**seçin.

    ![MVC denetleyicisini adlandırın](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Dosyaya yönergeleri kullanarak aşağıdakileri ekleyin: *using* `QueuesController.cs`

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
   ## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Aşağıdaki adımlar, sıranın nasıl oluşturulabildiğini gösterir:

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar. 

1. `QueuesController.cs` dosyasını açın. 

1. **ActionResult**döndüren **CreateQueue** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **CreateQueue** yönteminde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudQueueClient** nesnesi alın bir sıra hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. İstenilen sıra adına başvuruyu temsil eden bir **CloudQueue** nesnesi alın. **CloudQueueClient.GetQueueReference** yöntemi sıra depolamakarşı bir istekte bulunmaz. Başvuru, sıra nın var olup olmadığı döndürülür. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Henüz yoksa sıraoluşturmak için **CloudQueue.CreateIfNotExists** yöntemini arayın. **CloudQueue.CreateIfNotExists** yöntemi, kuyruk yoksa ve başarıyla oluşturulursa **doğru** döndürür. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. **ViewBag'i** sıranın adıyla güncelleştirin.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Kuyruklar'ı**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **CreateQueue'yi** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `CreateQueue.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **kuyruk oluştur'u** seçin:
  
    ![Sıra oluşturma](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Daha önce de belirtildiği gibi, **CloudQueue.CreateIfNotExists** yöntemi yalnızca kuyruk olmadığında ve oluşturulduğunda **doğru** döndürür. Bu nedenle, sıra olduğunda uygulamayı çalıştırıyorsanız, yöntem **yanlış**döndürür. Uygulamayı birden çok kez çalıştırmak için, uygulamayı yeniden çalıştırmadan önce sırayı silmeniz gerekir. Sırasilme **CloudQueue.Delete** yöntemi ile yapılabilir. Ayrıca, [Azure portalını](https://go.microsoft.com/fwlink/p/?LinkID=525040) veya Microsoft [Azure Depolama](../vs-azure-tools-storage-manage-with-storage-explorer.md)Gezgini'ni kullanarak sırayı silebilirsiniz.  

## <a name="add-a-message-to-a-queue"></a>Kuyruğa ileti ekleme

Bir sıra [oluşturduktan](#create-a-queue)sonra, bu kuyruğa ileti ekleyebilirsiniz. Bu bölüm, sıra *test kuyruğuna*ileti ekleme de size yol kat eder. 

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar. 

1. `QueuesController.cs` dosyasını açın.

1. **ActionResult**döndüren **AddMessage** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. **AddMessage** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. **CloudQueueClient** nesnesi alın bir sıra hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Kuyruğa başvuruyu temsil eden bir **CloudQueueContainer** nesnesi alın. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Kuyruğa eklemek istediğiniz iletiyi temsil eden **CloudQueueMessage** nesnesini oluşturun. **CloudQueueMessage** nesnesi bir dize (UTF-8 biçiminde) veya bayt dizisinden oluşturulabilir.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. İletiyi kuyruğa eklemek için **CloudQueue.AddMessage** yöntemini arayın.

    ```csharp
    queue.AddMessage(message);
    ```

1. Görünümde görüntülenmek için birkaç **ViewBag** özelliği oluşturun ve ayarlayın.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Kuyruklar'ı**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **AddMessage'ı** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `AddMessage.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **ileti ekle'yi** seçin:
  
    ![İleti ekleme](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

İki bölüm - [İletiyi kaldırmadan bir kuyruktan okuyun](#read-a-message-from-a-queue-without-removing-it) ve bir [iletiyi kuyruktan okuyup kaldırın](#read-and-remove-a-message-from-a-queue) - iletilerin kuyruktan nasıl okunduğunu gösterir.    

## <a name="read-a-message-from-a-queue-without-removing-it"></a>İletiyi kaldırmadan kuyruktan okuma

Bu bölümde, sıraya alınan bir iletiye nasıl göz atılanın (ilk iletiyi kaldırmadan okuyun) gösterin.  

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar. 

1. `QueuesController.cs` dosyasını açın.

1. **ActionResult**döndüren **PeekMessage** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. **PeekMessage** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. **CloudQueueClient** nesnesi alın bir sıra hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Kuyruğa başvuruyu temsil eden bir **CloudQueueContainer** nesnesi alın. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Sıradailk iletiyi kuyruktan çıkarmadan okumak için **CloudQueue.PeekMessage** yöntemini arayın. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. **ViewBag'i** iki değerle güncelleştirin: sıra adı ve okunan ileti. **CloudQueueMessage** nesnesi nesnenin değerini almak için iki özelliği ortaya çıkarır: **CloudQueueMessage.AsBytes** ve **CloudQueueMessage.AsString**. **AsString** (bu örnekte kullanılan) bir dize döndürür, **AsBytes** bir bayt dizisi döndürür.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Kuyruklar'ı**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **PeekMessage'ı** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `PeekMessage.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "PeekMessage";
    }
    
    <h2>Peek Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Peeked Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>    
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Peek iletisini** seçin:
  
    ![Peek iletisi](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>İletiyi sırayla okuma ve kaldırma

Bu bölümde, bir iletiyi sıradan nasıl okuyup kaldırabileceğinizi öğrenirsiniz.   

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar. 

1. `QueuesController.cs` dosyasını açın.

1. **ActionResult**döndüren **ReadMessage** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. **ReadMessage** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi edinin. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. **CloudQueueClient** nesnesi alın bir sıra hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Kuyruğa başvuruyu temsil eden bir **CloudQueueContainer** nesnesi alın. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Kuyruktaki ilk iletiyi okumak için **CloudQueue.GetMessage** yöntemini arayın. **CloudQueue.GetMessage** yöntemi, başka hiçbir kodun işlemi işlerken başka hiçbir kodun değiştiremesin veya silemesin diye iletiyi diğer kod okuma iletilerine 30 saniye (varsayılan olarak) görünmez kılar. İletinin görünmez olma süresini değiştirmek için **CloudQueue.GetMessage** yöntemine geçirilen **görünürlükZaman Çıkış** parametresini değiştirin.

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. İletiyi kuyruktan silmek için **CloudQueueMessage.Delete** yöntemini arayın.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Görünüm **Çantası'nı** ileti silinmiş ve sıranın adı ile güncelleştirin.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Kuyruklar'ı**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **ReadMessage'ı** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `ReadMessage.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "ReadMessage";
    }
    
    <h2>Read Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Read (and Deleted) Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Oku/Sil iletisini** seçin:
  
    ![İletiyi okuma ve silme](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bu bölümde sıra uzunluğu (ileti sayısı) nasıl alınılabilir. 

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar. 

1. `QueuesController.cs` dosyasını açın.

1. **ActionResult**döndüren **GetQueueLength** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. **ReadMessage** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi edinin. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. **CloudQueueClient** nesnesi alın bir sıra hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Kuyruğa başvuruyu temsil eden bir **CloudQueueContainer** nesnesi alın. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Kuyruğun özniteliklerini (uzunluğu dahil) almak için **CloudQueue.FetchAttributes** yöntemini arayın. 

    ```csharp
    queue.FetchAttributes();
    ```

1. Sıranın uzunluğunu almak için **CloudQueue.ApproximateMessageCount** özelliğine erişin.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. **ViewBag'i** sıranın adı ve uzunluğuyla güncelleştirin.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Kuyruklar'ı**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **GetQueueLength'u** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `GetQueueLengthMessage.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **sıra uzunluğunu al'ı** seçin:
  
    ![Sıra uzunluğunu alma](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Bir kuyruk silme
Bu bölümde, bir sıranın nasıl silinir gösteriş. 

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar. 

1. `QueuesController.cs` dosyasını açın.

1. **ActionResult**döndüren **DeleteQueue** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. **DeleteQueue** yönteminde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. **CloudQueueClient** nesnesi alın bir sıra hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Kuyruğa başvuruyu temsil eden bir **CloudQueueContainer** nesnesi alın. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. **CloudQueue** nesnesi tarafından temsil edilen sırayı silmek için **CloudQueue.Delete** yöntemini arayın.

    ```csharp
    queue.Delete();
    ```

1. **ViewBag'i** sıranın adı ve uzunluğuyla güncelleştirin.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Kuyruklar'ı**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **DeleteQueue'yi** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `DeleteQueue.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **sıra uzunluğunu al'ı** seçin:
  
    ![Sırayı silme](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure blob depolama ve Visual Studio Bağlantılı Hizmetler (ASP.NET) ile başlayın](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Azure tablo depolama ve Visual Studio Bağlantılı Hizmetler (ASP.NET) ile başlayın](vs-storage-aspnet-getting-started-tables.md)
