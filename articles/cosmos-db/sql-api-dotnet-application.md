---
title: 'Azure Cosmos DB için ASP.NET Core MVC öğreticisi: Web uygulaması geliştirme'
description: Azure Cosmos DB kullanarak bir MVC web uygulaması oluşturmak için MVC öğreticisi ASP.NET Core. Azure App Service-ASP NET Core MVC öğreticisi adım adım ' da barındırılan bir yapılacaklar uygulamasından JSON ve Access verilerini depolayacaksınız.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: sngun
ms.openlocfilehash: b1d8d2539ae89dfdb8feb2e38f00bf4440411d8a
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815143"
---
# <a name="tutorial-develop-an-aspnet-core-mvc-web-application-with-azure-cosmos-db-by-using-net-sdk"></a>Öğretici: .NET SDK kullanarak Azure Cosmos DB ile ASP.NET Core MVC web uygulaması geliştirme 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


Bu öğreticide Azure üzerinde barındırılan bir ASP.NET MVC uygulaması erişim verileri ve depolamak için Azure Cosmos DB kullanmayı gösterir. Bu öğreticide .NET SDK V3 ' i kullanırsınız. Aşağıdaki resimde, bu makaledeki örnek kullanarak derler web sayfası gösterilmektedir:
 
![Bu öğretici tarafından oluşturulan yapılacaklar listesi MVC web uygulaması ekran görüntüsü-ASP NET Core MVC öğretici adım adım](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Öğreticiyi tamamlamaya yönelik bir zaman yoksa, tüm örnek projeyi [GitHub][GitHub]'dan indirebilirsiniz.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> * Bir Azure Cosmos hesabı oluşturma
> * ASP.NET Core MVC uygulaması oluşturma
> * Uygulamayı Azure Cosmos DB'ye bağlanma 
> * Veri çubuğunda CRUD işlemleri gerçekleştirme

> [!TIP]
> Bu öğreticide, ASP.NET Core MVC ve Azure App Service kullanarak önceki deneyiminiz olduğunu varsaymaktadır. ASP.NET Core veya [Önkoşul araçları](#prerequisites)' nı yeni kullanıyorsanız, [GitHub][GitHub]'dan tüm örnek projeyi Indirmenizi, gerekli NuGet paketlerini eklemenizi ve çalıştırmayı öneririz. Projeyi derledikten sonra proje bağlamında kodu daha iyi kavramak için bu makaleyi inceleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar 

Bu makaledeki yönergeleri izlemeden önce aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* **Etkin bir Azure hesabı:** Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

Bu makaledeki tüm ekran görüntüleri Microsoft Visual Studio Community 2019 kullanılarak alınmıştır. Sisteminiz farklı bir sürümü ile yapılandırılmışsa ekranlarınızın ve seçeneklerinizin tamamen eşleşmiyor olabilir, ancak yukarıdaki önkoşulları karşılarsanız bu çözümün çalışması gerekir.

## <a name="create-an-azure-cosmos-account"></a>1. Adım: Azure Cosmos hesabı oluşturma

Bir Azure Cosmos hesabı oluşturarak başlayalım. Zaten bir Azure Cosmos DB SQL API hesabınızın olması veya Bu öğretici için Azure Cosmos DB öykünücüsü'nü kullanıyorsanız, adımına atlayabilirsiniz [yeni bir ASP.NET MVC uygulaması oluşturma](#create-a-new-mvc-application) bölümü.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

Sonraki bölümde, yeni bir ASP.NET Core MVC uygulaması oluşturacaksınız. 

## <a name="create-a-new-mvc-application"></a>2. Adım: Yeni bir ASP.NET Core MVC uygulaması oluşturma

1. Visual Studio'da gelen **dosya** menüsünde seçin **yeni**ve ardından **proje**. **Yeni Proje** iletişim kutusu görünür.

2. **Yeni proje** penceresinde, "Web" araması yapmak Için **şablon ara** giriş kutusunu kullanın ve ardından **ASP.NET Core Web uygulaması**' nı seçin. 

   ![Yeni ASP.NET Core Web uygulaması projesi oluştur](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. **Ad** kutusunda projenin adını yazın. Bu öğretici "todo" adını kullanır. Bu ad dışında bir şey kullanmayı tercih ederseniz, Bu öğreticinin Todo ad alanı hakkında konuştuğu her yerde, sağlanan kod örneklerini uygulamanıza verdiğiniz her şeyi kullanacak şekilde ayarlayın. 

4. Projeyi oluşturmak istediğiniz klasöre gitmek için **Araştır** ' ı seçin. **Oluştur**’u seçin. 

5. **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusu görüntülenir. Şablonlar listesinde, **Web uygulaması (Model-View-Controller)** öğesini seçin.

6. **Oluştur** ' u seçin ve Visual Studio 'nun boş ASP.NET Core MVC şablonu etrafında dolandırıcılamayı yapmasına izin verin. 

7. Visual Studio Demirbaş MVC uygulamasını oluşturmayı tamamlandıktan sonra yerel olarak çalıştırabileceğiniz boş bir ASP.NET uygulamasına sahip olursunuz.

## <a name="add-nuget-packages"></a>Adım 3: Projeye Azure Cosmos DB NuGet paketi Ekle

Bu çözüm için ihtiyaç duyduğumuz ASP.NET Core MVC Framework kodunun çoğunu kullandığımıza göre, Azure Cosmos DB bağlanmak için gereken NuGet paketlerini ekleyelim.

1. Azure Cosmos DB .NET SDK’sı, bir NuGet paketi olarak paketlenir ve dağıtılır. Visual Studio'da NuGet paketini almak için NuGet Paket Yöneticisi Visual Studio'da projeye sağ tıklayarak kullanmak **Çözüm Gezgini** seçip **NuGet paketlerini Yönet**.
   
   ![NuGet Paketlerini Yönet vurgulanmış şekilde, Çözüm Gezgini Web uygulaması projesi için sağ tıklama seçeneklerinin ekran görüntüsü.](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
2. **NuGet Paketlerini Yönet** iletişim kutusu görünür. Nuget **Gözat** kutusuna **Microsoft.Azure.Cosmos**. Sonuçlardan **Microsoft. Azure. Cosmos** paketini yükledikten sonra. Azure Cosmos DB paketini ve bağımlılıklarını indirir ve yükler. Yüklemeyi gerçekleştirmek için **Lisans kabul** penceresinde **kabul ediyorum** ' u seçin.
   
   Alternatif olarak, NuGet paketini yüklemek için Paket Yöneticisi konsolu kullanabilirsiniz. Bunu yapmak için **Araçları** menüsünde **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**. İsteminde aşağıdaki komutu yazın:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos
   ```        

3. Paket yüklendikten sonra Visual Studio projenizin Microsoft. Azure. Cosmos kitaplık başvurusunu içermesi gerekir.
  
## <a name="set-up-the-mvc-application"></a>4. Adım: ASP.NET Core MVC uygulamasını ayarlama

Şimdi şimdi bu MVC uygulamasına modeller, görünümler ve denetleyiciler ekleyelim:

* [Model ekleme](#add-a-model).
* [Denetleyici ekleme](#add-a-controller).
* [Görünümler ekleme](#add-views).

### <a name="add-a-model"></a> Model ekleme

1. Gelen **Çözüm Gezgini**, sağ **modelleri** klasörüne **Ekle**ve ardından **sınıfı**. **Yeni Öğe Ekle** iletişim kutusu görünür.

1. Yeni sınıfınızı **Item.cs** olarak adlandırın ve **Ekle**' yi seçin. 

1. Sonra, "Item.cs" sınıfındaki kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Models/Item.cs)]
   
   Azure Cosmos DB'de depolanan veriler kablo üzerinden geçer ve JSON olarak depolanır. JSON.NET tarafından nesnelerin serileştirilme/seri durumdan çıkarılma şeklini denetlemek için, oluşturduğunuz **öğe** sınıfında gösterildiği gibi **jsonproperty** özniteliğini kullanabilirsiniz. Yalnızca JSON 'a giden Özellik adının biçimini denetleyemezseniz, .NET özelliklerinizi de **tamamlanmış** özelliği ile yaptığınız gibi yeniden adlandırabilirsiniz. 

### <a name="add-a-controller"></a>Denetleyici ekleme

1. Gelen **Çözüm Gezgini**, sağ **denetleyicileri** klasörüne **Ekle**ve ardından **denetleyicisi**. **İskele Ekle** iletişim kutusu görünür.

1. **MVC denetleyicisi-boş** öğesini seçin ve **Ekle**' yi seçin.

   ![MVC denetleyicisi-boş seçeneği vurgulanmış şekilde Scafkatlama Ekle iletişim kutusunun ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Yeni denetleyicinizi, **ıtemcontroller**' ı adlandırın ve bu dosyadaki kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Controllers/ItemController.cs)]

   **ValidateAntiForgeryToken** özniteliği burada bu uygulamayı siteler arası istek sahteciliği saldırılarına karşı korumaya yardımcı olmak için kullanılır. Yalnızca bu özniteliği eklemek için daha fazla, kendi görünümlerinizi de bu sahteciliğe karşı koruma belirteci ile çalışması gerekir. Konu hakkında daha fazla bilgi ve bunu doğru şekilde uygulama örnekleri için bkz. [siteler arası Istek sahteciliğini önleme][Preventing Cross-Site Request Forgery]. [GitHub][GitHub]'da sağlanan kaynak kodu tam uygulamayı içerir.

   Biz de **bağlama** korumaya gönderim saldırılarına karşı korunmaya yardımcı olmak için yöntem parametresinde özniteliği. Daha fazla ayrıntı için lütfen [ASP.NET MVC 'de temel CRUD işlemlerine][Basic CRUD Operations in ASP.NET MVC]bakın.

### <a name="add-views"></a>Görünümler ekleme

Ardından, aşağıdaki üç görünümleri oluşturalım: 

* [Bir liste öğesi görünümü ekleme](#AddItemIndexView).
* [Yeni öğe görünümü ekleme](#AddNewIndexView).
* [Düzenleme öğesi görünümü ekleme](#AddEditIndexView).

#### <a name="AddItemIndexView"></a>Bir liste öğesi görünümü ekleme

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasör boş sağ **öğesi** eklediğiniz zaman Visual Studio için oluşturduğunuz klasöre  **Itemcontroller** daha önce tıklayın **Ekle**ve ardından **görünümü**.
   
   ![Görünüm Ekle komutları vurgulanmış şekilde Visual Studio 'Nun oluşturduğu öğe klasörünü gösteren Çözüm Gezgini ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view.png)

2. İçinde **Görünüm Ekle** iletişim kutusunda, aşağıdaki değerleri güncelleştirin:
   
   * **Görünüm adı** kutusunda ***Index*** yazın.
   * **Şablon** kutusunda ***Liste***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
     
   ![Görünüm Ekle iletişim kutusunu gösteren ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)

3. Bu değerleri ekledikten sonra seçin **Ekle** ve Visual Studio'nun yeni bir şablon görünümü oluşturmasına izin verin. Bunu yaptıktan sonra oluşturulan cshtml dosyasını açar. Sizin için daha sonra geri dönebilir şekilde bu dosyayı Visual Studio'daki kapatabilirsiniz.

#### <a name="AddNewIndexView"></a>Yeni öğe görünümü ekleme

Liste öğeleri için bir görünüm oluşturun aşağıdaki adımları kullanarak öğeleri oluşturmak için yeni bir görünüm nasıl oluşturduğunuz benzer:

1. Gelen **Çözüm Gezgini**, sağ **öğesi** klasörü yeniden seçin **Ekle**ve ardından **görünümü**.

1. İçinde **Görünüm Ekle** iletişim kutusunda, aşağıdaki değerleri güncelleştirin:
   
   * **Görünüm adı** kutusunda ***Create*** yazın.
   * **Şablon** kutusunda ***Oluştur***'u seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Add (Ekle)** seçeneğini belirleyin.
   
#### <a name="AddEditIndexView"></a>Düzenleme öğesi görünümü ekleme

Ve son olarak, aşağıdaki adımlarla bir öğeyi düzenlemek için bir görünüm ekleyin:

1. Gelen **Çözüm Gezgini**, sağ **öğesi** klasörü yeniden seçin **Ekle**ve ardından **görünümü**.

1. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Edit*** yazın.
   * **Şablon** kutusunda ***Düzenle***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Add (Ekle)** seçeneğini belirleyin.

Bunu yaptıktan sonra Bu görünümlere daha sonra geri olarak Visual Studio'daki tüm cshtml belgelerini kapatın.

## <a name="connect-to-cosmosdb"></a>5. Adım: Azure Cosmos DB’ye bağlanma 

Standart MVC hallolduğuna göre Azure Cosmos DB'ye bağlanmak ve CRUD işlemleri gerçekleştirmek için kod eklemeye dönelim. 

### <a name="perform-crud-operations"></a> Veri çubuğunda CRUD işlemleri gerçekleştirme

Burada yapılacak ilk şey, bağlanmasına ve Azure Cosmos DB için mantığı içeren bir sınıf eklemektir. Bu öğreticide, bu mantığı adlı `CosmosDBService` bir sınıfa ve adlı `ICosmosDBService`bir arabirime kapsülliyoruz. Bu hizmet, tamamlanmamış öğeleri Listeleme, öğeleri oluşturma, düzenlemesi ve silme gibi CRUD ve okuma akışı işlemlerini gerçekleştirir. 

1. Gelen **Çözüm Gezgini**, adlı projenize altında yeni bir klasör oluşturun **Hizmetleri**.

1. Sağ **Hizmetleri** klasörüne **Ekle**ve ardından **sınıfı**. Yeni sınıfı **Cosmosdbservice** olarak adlandırın ve **Ekle**' yi seçin.

1. Aşağıdaki kodu **Cosmosdbservice** sınıfına ekleyin ve bu dosyadaki kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/CosmosDbService.cs)]

1. **Icosmosdbservice**adlı bir sınıf için 2-3, ancak bu kez adımları yineleyin ve aşağıdaki kodu ekleyin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/ICosmosDbService.cs)]
 
1. Önceki kod, oluşturucunun bir `CosmosClient` parçası olarak bir alır. ASP.NET Core işlem hattında, projenin **Startup.cs** 'e gitmemiz ve Istemciyi, [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)yoluyla eklenmesi için tek bir örnek olarak yapılandırmayı temel alarak başlatmemiz gerekir. **ConfigureServices** işleyicisinde şunları tanımlayacağız:

    ```csharp
    services.AddSingleton<ICosmosDbService>(InitializeCosmosClientInstanceAsync(Configuration.GetSection("CosmosDb")).GetAwaiter().GetResult());
    ```

1. Aynı dosyada, yapılandırmayı okuyacak ve istemciyi başlatacak olan **ınitializecosmosclientınstanceasync**yardımcı yöntemimizi tanımlayacağız.

    [!code-csharp[](~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs?name=InitializeCosmosClientInstanceAsync)] 

1. Yapılandırma projenin **appSettings. JSON** dosyasında tanımlanmıştır. Açın ve **Cosmosdb**adlı bir bölüm ekleyin:

   ```csharp
     "CosmosDb": {
        "Account": "<enter the URI from the Keys blade of the Azure Portal>",
        "Key": "<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>",
        "DatabaseName": "Tasks",
        "ContainerName": "Items"
      }
   ```
 
Uygulamayı çalıştırırsanız, ASP.NET Core işlem hattı **Cosmosdbservice** örneğini oluşturur ve tek bir örneği tek bir örnek olarak tutar; **ıtemcontroller** , istemci tarafı isteklerini işlemek için kullanıldığında bu tek örneği alır ve bunu CRUD işlemleri gerçekleştirmek için kullanabilir.

Şimdi bu projeyi derleyip çalıştırırsanız şöyle görünen bir şey görmeniz gerekir:

![Bu veritabanı öğreticisi tarafından oluşturulan yapılacaklar listesi Web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>Adım 6: Uygulamayı yerel olarak çalıştırma

Yerel makinenizde uygulamayı test etmek için aşağıdaki adımları kullanın:

1. Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5 tuşuna basın. Bu işlemin uygulamayı oluşturması ve bir tarayıcıyı daha önce gördüğümüz boş kılavuz sayfasıyla başlatması gerekir:
   
   ![Bu öğretici tarafından oluşturulan yapılacaklar listesi Web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. **Yeni Oluştur** bağlantısına tıklayın ve **Ad** ve **Açıklama** alanlarına değerler ekleyin. Bırakın **tamamlandı** onay kutusunu seçili Aksi halde yeni öğe tamamlanmış durumda eklenir ve ilk listede görünmez.
   
3. Tıklayın **Oluştur** ve geri yönlendirilirsiniz **dizin** görünümü ve, öğe listede görünür. Yapılacaklar listenize birkaç daha fazla öğe ekleyebilirsiniz.

    ![Dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
  
4. Listedeki bir **Öğe**'nin yanındaki **Düzenle**'ye tıklayın, böylece **Tamamlandı** bayrağı dahil olmak üzere nesnenizin herhangi bir özelliğini güncelleştirebileceğiniz **Düzenle** görünümüne gidersiniz. **Tamamlandı** bayrağını işaret eder ve **Kaydet**' e tıklarsanız, **öğe** listede tamamlandı olarak görüntülenir.
   
   ![Tamamlandı kutusu işaretli dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)

5. Azure Cosmos DB hizmetindeki verilerin durumunu [Cosmos Gezginini](https://cosmos.azure.com) veya Azure Cosmos DB öykünücü Veri Gezgini kullanarak doğrulayabilirsiniz.

6. Uygulamayı test ettikten sonra, uygulamanın hata ayıklamasını durdurmak için Ctrl+F5'e basın. Dağıtıma hazırsınız!

## <a name="deploy-the-application-to-azure"></a>Adım 7: Uygulamayı dağıtma 
Artık uygulamanın tamamı Azure Cosmos DB ile doğru şekilde çalıştığına göre, bu web uygulamasını Azure App Service’e dağıtacağız.  

1. Bu uygulamayı yayımlamak için projeyi sağ **Çözüm Gezgini** seçip **Yayımla**.
   
2. **Yayımla** iletişim kutusunda **App Service**' yi seçin ve ardından **yeni oluştur** ' u seçerek App Service profili oluşturun veya var olan bir profili kullanmak için **Varolanı Seç** ' i seçin.

3. Mevcut bir Azure App Service profili varsa, seçin bir **abonelik** açılır listeden. Kullanım **görünümü** filtresi kaynak grubu veya kaynak türüne göre sıralayabilirsiniz. Ardından gerekli Azure App Service'ı arayın ve seçin **Tamam**.
   
   ![Visual Studio’da App Service iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. Yeni bir Azure App Service profili oluşturmak için, **Yayımla** iletişim kutusunda **Yeni Oluştur**’a tıklayın. İçinde **App Service Oluştur** iletişim kutusunda, Web uygulaması adınız ve uygun aboneliği, kaynak grubu ve App Service planı'nı girin ve ardından **Oluştur**.

   ![Visual Studio’da App Service’i Oluştur iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Visual Studio, birkaç saniye içinde Web uygulamanızı yayımlar ve projenizi Azure 'da çalıştırdığınız yerde görebileceğiniz bir tarayıcı başlatır!

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Cosmos DB depolanan verilere erişebilen ASP.NET Core MVC web uygulaması oluşturmayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

* [Azure Cosmos DB verilerinizi bölümleme hakkında bilgi edinin](./partitioning-overview.md)
* [Azure Cosmos DB ' de daha gelişmiş sorgular yapma hakkında bilgi edinin](./how-to-sql-query.md)
* [Daha gelişmiş bir senaryoda verilerinizi nasıl modelleyeceğinizi öğrenin](./how-to-model-partition-example.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app
