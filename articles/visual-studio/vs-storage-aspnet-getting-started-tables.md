---
title: Visual Studio 'u (ASP.NET) kullanarak Azure tablo depolamasına başlayın
description: Visual Studio Connected Services'ı kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki bir ASP.NET projesinde Azure tablo depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/21/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c49df689ae859c93046c19af043aa2001dbb5481
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979624"
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Azure tablo depolama ve Visual Studio Bağlantılı Hizmetler (ASP.NET) ile başlayın
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış

Azure Tablo depolama, büyük miktarda yapılandırılmış veri depolamanıza olanak tanır. Hizmet, Azure bulutu içinden ve dışından kimlik doğrulamalı aramaları kabul eden bir NoSQL veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

Bu öğretici, Azure tablo depolama varlıklarını kullanarak bazı yaygın senaryolar için ASP.NET kodu nasıl yazılır şekilde gösterilmektedir. Bu senaryolar arasında tablo oluşturma ve tablo varlıklarını ekleme, sorgulama ve silme yer alıyor. 

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-account-create.md)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. Çözüm **Gezgini'nde,** **Denetleyicileri**sağ tıklatın ve bağlam menüsünden **Ekle->** Denetleyicisi'ni seçin.

    ![ASP.NET bir MVC uygulamasına denetleyici ekleme](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. İskele **Ekle** iletişim kutusunda **MVC 5 Controller 'ı**seçin - Boş ve **Ekle'yi**seçin.

    ![MVC denetleyici türünü belirtin](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. Denetleyici **Ekle** iletişim kutusunda, denetleyici *tablesController'ı*adlandırın ve **Ekle'yi**seçin.

    ![MVC denetleyicisini adlandırın](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Dosyaya yönergeleri kullanarak aşağıdakileri ekleyin: *using* `TablesController.cs`

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Model sınıfı oluşturma

Bu makaledeki örneklerin **çoğu, CustomerEntity**adlı **tableEntity**türetilmiş sınıf kullanın. Aşağıdaki adımlar, bu sınıfı bir model sınıfı olarak beyan etme de size yol gösterin:

1. Çözüm **Gezgini'nde** **Modeller'i**sağ tıklatın ve bağlam menüsünden **>Ekle'yi**seçin.

1. Yeni **Öğe Ekle** iletişim kutusunda, sınıfın adı **CustomerEntity'** dir.

1. Dosyayı `CustomerEntity.cs` açın ve yönergeyi **kullanarak** aşağıdakileri ekleyin:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. Sınıfı, tamamlandığında aşağıdaki kodda olduğu gibi bildirilen şekilde değiştirin. Sınıf, müşterinin ilk adını satır anahtarı ve soyadını bölüm anahtarı olarak kullanan **CustomerEntity** adlı bir varlık sınıfını bildirir.

    ```csharp
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }
    }
    ```

## <a name="create-a-table"></a>Bir tablo oluşturma

Aşağıdaki adımlar, tablonun nasıl oluşturulabildiğini gösterir:

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama'daki](#set-up-the-development-environment)adımları tamamladığınızı varsayar. 

1. `TablesController.cs` dosyasını açın.

1. **ActionResult**döndüren **CreateTable** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **CreateTable** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil alın.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. İstenilen tablo adına başvuruyu temsil eden bir **CloudTable** nesnesi alın. **CloudTableClient.GetTableReference** yöntemi tablo depolamakarşı bir istekte bulunmaz. Tablo var olsun veya olmasın başvuru döndürülür. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Henüz yoksa tablo oluşturmak için **CloudTable.CreateIfNotExists** yöntemini arayın. **CloudTable.CreateIfNotExists** yöntemi, tablo yoksa ve başarıyla oluşturulursa **doğru** döndürür. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. **ViewBag'i** tablonun adı ile güncelleştirin.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Tablolar'ı**sağ tıklatın ve bağlam menüsünden **Ekle->Görünümü'nü**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **CreateTable'ı** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `CreateTable.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Tablo Oluştur'u** seçin:
  
    ![Tablo oluşturma](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Daha önce de belirtildiği gibi, **CloudTable.CreateIfNotExists** yöntemi yalnızca tablo yokken ve oluşturulduğunda **doğru** döndürür. Bu nedenle, tablo olduğunda uygulamayı çalıştırıyorsanız, yöntem **yanlış**döndürür. Uygulamayı birden çok kez çalıştırmak için, uygulamayı yeniden çalıştırmadan önce tabloyu silmeniz gerekir. Tablosilme **CloudTable.Delete** yöntemi ile yapılabilir. Azure [portalını](https://go.microsoft.com/fwlink/p/?LinkID=525040) veya [Microsoft Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md)kullanarak tabloyu da silebilirsiniz.  

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

*Varlıklar,* \# **TableEntity'den**türetilen özel bir sınıf kullanarak C nesnelerini eşler. Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Bu bölümde, satır anahtarı ve son ad olarak satır anahtarı olarak müşterinin ilk adını kullanan bir varlık sınıfının nasıl tanımlanacağını görürsünüz. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması paralel işlemler için daha büyük ölçeklendirme sağlar. Tablo hizmetinde depolanması gereken herhangi bir özellik için, özellik hem ayar hem de alma değerlerini ortaya çıkaran desteklenen bir türe ait bir kamu malı olmalıdır.
Varlık sınıfı, ortak parametresiz bir oluşturucu *bildirmelidir.*

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama'daki](#set-up-the-development-environment)adımları tamamladığınızı varsayar.

1. `TablesController.cs` dosyasını açın.

1. Dosyadaki kodun `TablesController.cs` **CustomerEntity** sınıfına erişebilmeleri için aşağıdaki yönergeyi ekleyin:

    ```csharp
    using StorageAspnet.Models;
    ```

1. **ActionResult**döndüren **AddEntity** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **AddEntity** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil alın.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Yeni varlığı ekleyeceğiniz tabloya başvuruyu temsil eden bir **CloudTable** nesnesi alın. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. **CustomerEntity** sınıfını anında ve baş harfe çevirin.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Müşteri varlığını ekleyen bir **TableOperation** nesnesi oluşturun.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. **CloudTable.Execute** yöntemini arayarak ekleme işlemini yürütün. **TableResult.HttpStatusCode** özelliğini inceleyerek işlemin sonucunu doğrulayabilirsiniz. 2xx'in durum kodu, istemci tarafından istenen eylemin başarıyla işlenirolduğunu gösterir. Örneğin, yeni varlıkların başarılı eklemeleri 204'ün HTTP durum koduyla sonuçlanır, bu da işlemin başarıyla işlendiği ve sunucunun herhangi bir içerik döndürmediği anlamına gelir.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Görünüm **Çantası'nı** tablo adı ve ekleme işleminin sonuçlarıyla güncelleştirin.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Tablolar'ı**sağ tıklatın ve bağlam menüsünden **Ekle->Görünümü'nü**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **AddEntity'i** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `AddEntity.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **varlık ekle'yi** seçin:
  
    ![Varlık ekleme](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    Bölümdeki adımları izleyerek varlığın eklandığını doğrulayabilirsiniz, [Tek bir varlık alın.](#get-a-single-entity) Tablolarınızın tüm varlıklarını görüntülemek için [Microsoft Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md) de kullanabilirsiniz.

## <a name="add-a-batch-of-entities-to-a-table"></a>Tabloya bir yığın varlık ekleme

Bir [tabloya bir varlığı teker teker](#add-an-entity-to-a-table)ekleyebilmeek, toplu olarak varlıklar da ekleyebilirsiniz. Toplu olarak varlık eklemek, kodunuz la Azure tablo hizmeti arasındaki gidiş-dönüş sayısını azaltır. Aşağıdaki adımlar, tek bir ekleme işlemi olan bir tabloya birden çok varlığın nasıl ekleyeceğini gösteriş ve gösteriş:

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama'daki](#set-up-the-development-environment)adımları tamamladığınızı varsayar.

1. `TablesController.cs` dosyasını açın.

1. **ActionResult**döndüren **AddE ntitiesı** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **AddEntities** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil alın.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Yeni varlıkları ekleyeceğiniz tabloya başvuruyu temsil eden bir **CloudTable** nesnesi alın. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Bölümde sunulan **CustomerEntity** model sınıfına göre bazı müşteri nesnelerini anında anlayın, [tabloya varlık ekleyin.](#add-an-entity-to-a-table)

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. **TableBatchOperation** nesnesi alın.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Toplu iş ekleme işlemi nesnesine varlıklar ekleyin.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. **CloudTable.ExecuteBatch** yöntemini arayarak toplu ekleme işlemini gerçekleştirin.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. **CloudTable.ExecuteBatch** yöntemi, her bir işlemin başarısını veya başarısızlığını belirlemek için her **TableResult** nesnesinin incelenebileceği **TableResult** nesnelerinin bir listesini döndürür. Bu örnekte, listeyi bir görünüme geçirin ve görünümün her işlemin sonuçlarını görüntülemesine izin verin. 
 
    ```csharp
    return View(results);
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Tablolar'ı**sağ tıklatın ve bağlam menüsünden **Ekle->Görünümü'nü**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **AddEntities** girin ve **Ekle'yi**seçin.

1. Açın `AddEntities.cshtml`ve aşağıdaki gibi görünmesi için değiştirin.

    ```csharp
    @model IEnumerable<Microsoft.WindowsAzure.Storage.Table.TableResult>
    @{
        ViewBag.Title = "AddEntities";
    }
    
    <h2>Add-entities results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        @foreach (var result in Model)
        {
        <tr>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@result.HttpStatusCode</td>
        </tr>
        }
    </table>
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **varlıkları ekle'yi** seçin:
  
    ![Varlık ekleme](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    Bölümdeki adımları izleyerek varlığın eklandığını doğrulayabilirsiniz, [Tek bir varlık alın.](#get-a-single-entity) Tablolarınızın tüm varlıklarını görüntülemek için [Microsoft Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md) de kullanabilirsiniz.

## <a name="get-a-single-entity"></a>Tek bir varlık alma

Bu bölümde, varlığın satır anahtarı ve bölüm anahtarını kullanarak tablodan tek bir varlığın nasıl alınılacağından açıklanmıştır. 

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar ve [tabloya bir yığın varlık ekle'den](#add-a-batch-of-entities-to-a-table)gelen verileri kullanır. 

1. `TablesController.cs` dosyasını açın.

1. **ActionResult**döndüren **GetSingle** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **GetSingle** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil alın.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Varlığı geri aldığınız tabloya yapılan başvuruyu temsil eden bir **CloudTable** nesnesi alın. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. **TableEntity'den**türetilen bir varlık nesnesi alan bir retrieve işlem nesnesi oluşturun. İlk parametre *partitionKey,* ikinci parametre *rowKey*olduğunu. **CustomerEntity** sınıfını ve bu bölümde sunulan verileri kullanarak [Tabloya bir yığın varlık ekleyin](#add-a-batch-of-entities-to-a-table), aşağıdaki kod snippet"Smith" bölümüKey değeri ve "Ben" *rowKey* değeri ile **CustomerEntity** varlığı için tabloyu sorgular: *rowKey*

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. Geri alma işlemini yürütün.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Sonucu görüntülemek için görünüme geçirin.

    ```csharp
    return View(result);
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Tablolar'ı**sağ tıklatın ve bağlam menüsünden **Ekle->Görünümü'nü**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **GetTek'i** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `GetSingle.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "GetSingle";
    }
    
    <h2>Get Single results</h2>
    
    <table border="1">
        <tr>
            <th>HTTP result</th>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        <tr>
            <td>@Model.HttpStatusCode</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).Email)</td>
        </tr>
    </table>
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Tek Al'ı** seçin:
  
    ![Bekar olun](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma

Bölümde belirtildiği gibi, [tabloya bir varlık ekleyin,](#add-an-entity-to-a-table)bir bölüm ve satır anahtarının birleşimi tablodaki bir varlığı benzersiz olarak tanımlar. Aynı bölüm tuşuna sahip varlıklar, farklı bölüm tuşlarına sahip varlıklardan daha hızlı sorgulanabilir. Bu bölümde, belirtilen bir bölümden tüm varlıklar için bir tablonun nasıl sorgulanır şekilde sorgulanır.  

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar ve [tabloya bir yığın varlık ekle'den](#add-a-batch-of-entities-to-a-table)gelen verileri kullanır. 

1. `TablesController.cs` dosyasını açın.

1. **ActionResult**döndüren **GetPartition** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **GetPartition** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil alın.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Varlıkları geri aldığınız tabloya yapılan başvuruyu temsil eden bir **CloudTable** nesnesi alın. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. **Nerede** yan tümcesi ndeki sorguyu belirten bir **TableQuery** nesnesini anında belirleyin. **CustomerEntity** sınıfını ve bu bölümde sunulan verileri kullanarak [Bir tabloya bir yığın varlık ekleyin,](#add-a-batch-of-entities-to-a-table) **PartitionKey'in** (müşterinin soyadı) "Smith" değerine sahip olduğu tüm varlıklar için tabloyu aşağıdaki kod parçacıkları sorgular:

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. Bir döngü içinde, önceki adımda anlık olarak girdiğiniz sorgu nesnesini geçen **CloudTable.ExecuteQuerySegmented** yöntemini arayın.  **CloudTable.ExecuteQuerySegmented** yöntemi, bir **TableContinuationToken** nesnesini döndürür ve bu nesne - **null** olduğunda - alınacak başka varlık olmadığını gösterir. Döngü içinde, döndürülen varlıklar üzerinde yinelemek için başka bir döngü kullanın. Aşağıdaki kod örneğinde, döndürülen her varlık bir listeye eklenir. Döngü sona erdiğinde, liste görüntülenmek üzere bir görünüme geçirilir: 

    ```csharp
    List<CustomerEntity> customers = new List<CustomerEntity>();
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = table.ExecuteQuerySegmented(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity customer in resultSegment.Results)
        {
            customers.Add(customer);
        }
    } while (token != null);

    return View(customers);
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Tablolar'ı**sağ tıklatın ve bağlam menüsünden **Ekle->Görünümü'nü**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **GetPartition'ı** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `GetPartition.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @model IEnumerable<StorageAspnet.Models.CustomerEntity>
    @{
        ViewBag.Title = "GetPartition";
    }
    
    <h2>Get Partition results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        @foreach (var customer in Model)
        {
        <tr>
            <td>@(customer.RowKey)</td>
            <td>@(customer.PartitionKey)</td>
            <td>@(customer.Email)</td>
        </tr>
        }
    </table>
    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **Bölüm Al'ı** seçin:
  
    ![Bölüm alın](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Bir varlığı silme

Bu bölümde, bir varlığın tablodan nasıl silinir olduğu gösterilebilir.

> [!NOTE]
> 
> Bu bölüm, [geliştirme ortamını ayarlama](#set-up-the-development-environment)adımlarını tamamladığınızı varsayar ve [tabloya bir yığın varlık ekle'den](#add-a-batch-of-entities-to-a-table)gelen verileri kullanır. 

1. `TablesController.cs` dosyasını açın.

1. **ActionResult**döndüren **DeleteEntity** adlı bir yöntem ekleyin.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. **DeleteEntity** yöntemi nde, depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (Depolama * &lt;hesabı adı>* erişidiğiniz Azure depolama hesabının adına değiştirin.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. **CloudTableClient** nesnesi bir tablo hizmeti istemcisi temsil alın.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Varlığı silerken tabloya yapılan başvuruyu temsil eden bir **CloudTable** nesnesi alın. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. **TableEntity'den**türetilen bir varlık nesnesi alan bir silme işlemi nesnesi oluşturun. Bu durumda, **CustomerEntity** sınıfını ve bölümünde sunulan verileri [kullanarak tabloya bir yığın varlık ekleyin.](#add-a-batch-of-entities-to-a-table) Varlığın **ETag** geçerli bir değer olarak ayarlanmalıdır.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. Silme işlemini yürütün.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Sonucu görüntülemek için görünüme geçirin.

    ```csharp
    return View(result);
    ```

1. Çözüm **Gezgini'nde** **Görünümler** klasörünü genişletin, **Tablolar'ı**sağ tıklatın ve bağlam menüsünden **Ekle->Görünümü'nü**seçin.

1. Görünüm **Ekle** iletişim kutusunda, görünüm adı için **DeleteEntity'i** girin ve **Ekle'yi**seçin.

1. Aşağıdaki `DeleteEntity.cshtml`kod parçacığı gibi görünmesi için açın ve değiştirin:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "DeleteEntity";
    }
    
    <h2>Delete Entity results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        <tr>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@Model.HttpStatusCode</td>
        </tr>
    </table>

    ```

1. Çözüm **Gezgini'nde,** **Görünümler >Paylaşılan** `_Layout.cshtml`klasörünü genişletin ve açın.

1. Son **Html.ActionLink**sonra , aşağıdaki **Html.ActionLink**ekleyin:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Uygulamayı çalıştırın ve aşağıdaki ekran görüntüsüne benzer sonuçları görmek için **varlığı sil'i** seçin:
  
    ![Bekar olun](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure blob depolama ve Visual Studio Bağlantılı Hizmetler (ASP.NET) ile başlayın](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Azure sıra depolama ve Visual Studio Bağlantılı Hizmetler (ASP.NET) ile başlayın](../storage/vs-storage-aspnet-getting-started-queues.md)
