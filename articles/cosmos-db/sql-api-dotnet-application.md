---
title: Azure Cosmos DB kullanarak ASP.NET Core MVC web uygulaması öğreticisi
description: ASP.NET Azure Cosmos DB kullanarak bir MVC web uygulaması oluşturmak için Core MVC öğretici. JSON'u saklayacak ve Azure App Service 'de barındırılan bir todo uygulamasından verilere erişeceksiniz - ASP NET Core MVC öğretici adım adım.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 02/27/2020
ms.author: sngun
ms.openlocfilehash: 1f2051addfa1266b754d230c3804834c63f89002
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78274068"
---
# <a name="tutorial-develop-an-aspnet-core-mvc-web-application-with-azure-cosmos-db-by-using-net-sdk"></a>Öğretici: .NET SDK kullanarak Azure Cosmos DB ile ASP.NET Core MVC web uygulaması geliştirin

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)

Bu öğretici, Azure'da barındırılan ASP.NET bir MVC uygulamasından gelen verileri depolamak ve bunlara erişmek için Azure Cosmos DB'yi nasıl kullanacağınızı gösterir. Bu eğitimde .NET SDK V3'ü kullanıyorsunuz. Aşağıdaki resim, bu makaledeki örneği kullanarak oluşturacağınız web sayfasını gösterir:

![Bu öğretici tarafından oluşturulan todo listesi MVC web uygulaması ekran görüntüsü - ASP NET Core MVC öğretici adım adım](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Öğreticiyi tamamlamak için zamanın yoksa, örnek projenin tamamını [GitHub'dan][GitHub]indirebilirsiniz.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
>
> * Azure Cosmos hesabı oluşturma
> * ASP.NET Core MVC uygulaması oluşturma
> * Uygulamayı Azure Cosmos DB'ye bağlama
> * Veriler üzerinde oluşturma, okuma, güncelleme ve silme (CRUD) işlemlerini gerçekleştirme

> [!TIP]
> Bu öğretici, ASP.NET Core MVC ve Azure Uygulama Hizmeti'ni kullanarak daha önce deneyime sahip olduğunuzu varsayar. Core'ASP.NET veya [ön koşul araçlarında](#prerequisites)yeniyseniz, tüm örnek projeyi [GitHub'dan][GitHub]indirmenizi, gerekli NuGet paketlerini eklemenizi ve çalıştırmanızı öneririz. Projeyi oluşturduğunuzda, proje bağlamında kod hakkında bilgi edinmek için bu makaleyi gözden geçirebilirsiniz.

## <a name="prerequisites"></a><a name="prerequisites"></a>Ön koşullar

Bu makaledeki yönergeleri takip etmeden önce, aşağıdaki kaynaklara sahip olduğundan emin olun:

* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Görsel Stüdyo 2019. [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

Bu makaledeki tüm ekran görüntüleri Microsoft Visual Studio Community 2019'dan alınmaktadır. Farklı bir sürüm kullanıyorsanız, ekranlarınız ve seçenekleriniz tamamen eşleşmeyebilir. Ön koşulları karşılıyorsanız çözüm işe yaramalıdır.

## <a name="step-1-create-an-azure-cosmos-account"></a><a name="create-an-azure-cosmos-account"></a>Adım 1: Azure Cosmos hesabı oluşturma

Bir Azure Cosmos hesabı oluşturarak başlayalım. Zaten bir Azure Cosmos DB SQL API hesabınız varsa veya Azure Cosmos DB emülatörü kullanıyorsanız, [Adım 2'ye geçin: Yeni bir ASP.NET MVC uygulaması oluşturun.](#create-a-new-mvc-application)

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

Sonraki bölümde, yeni bir ASP.NET Core MVC uygulaması oluşturursunuz.

## <a name="step-2-create-a-new-aspnet-core-mvc-application"></a><a name="create-a-new-mvc-application"></a>Adım 2: Core MVC uygulaması ASP.NET yeni bir ASP.NET oluşturma

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin.

1. **Yeni bir proje oluştur'da**C# için ASP.NET Çekirdek Web **Uygulaması** bulun ve seçin. Devam etmek için **İleri**’yi seçin.

   ![Yeni ASP.NET Core web uygulama projesi oluşturma](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

1. **Yeni projenizi yapılandırın,** *projeyi adlandırın* ve **Oluştur'u**seçin.

1. **Yeni bir ASP.NET Core Web Uygulaması Oluştur'da**Web Uygulaması **(Model-View-Controller) seçeneğini belirleyin.** Devam etmek için **Oluştur'u** seçin.

   Visual Studio boş bir MVC uygulaması oluşturur.

1. ASP.NET uygulamanızı yerel olarak çalıştırmak için **Hata** > **Ayıklama Başlatma Hata Ayıklama** veya F5'i seçin.

## <a name="step-3-add-azure-cosmos-db-nuget-package-to-the-project"></a><a name="add-nuget-packages"></a>Adım 3: Projeye Azure Cosmos DB NuGet paketi ekleyin

Artık bu çözüm için ihtiyacımız olan ASP.NET Core MVC çerçeve kodunun çoğuna sahip olduğumuza göre, Azure Cosmos DB'ye bağlanmak için gereken NuGet paketlerini ekleyelim.

1. **Çözüm Gezgini'nde**projenizi sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Paket Yöneticisi'nde** **Microsoft.Azure.Cosmos'u**arayın ve seçin. **Yükle**’yi seçin.

   ![NuGet paketini yükleyin](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-nuget.png)

   Visual Studio, Azure Cosmos DB paketini ve bağımlılıklarını indirir ve yükler.

   NuGet paketini yüklemek için **Package Manager Console'u** da kullanabilirsiniz. Bunu yapmak için **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu**seçin. İsteyerek, aşağıdaki komutu yazın:

   ```ps
   Install-Package Microsoft.Azure.Cosmos
   ```
  
## <a name="step-4-set-up-the-aspnet-core-mvc-application"></a><a name="set-up-the-mvc-application"></a>Adım 4: ASP.NET Core MVC uygulamasını ayarlama

Şimdi bu MVC uygulamasına modelleri, görünümleri ve denetleyicileri ekleyelim.

### <a name="add-a-model"></a><a name="add-a-model"></a>Model ekleme

1. **Çözüm Gezgini'nde** **, Modeller** klasörüne sağ tıklayın,**Sınıf** **Ekle'yi** > seçin.

1. **Yeni Öğe Ekle'de**yeni sınıf adı *Item.cs* ve **Ekle'yi**seçin.

1. *Item.cs* sınıfın içeriğini aşağıdaki kodla değiştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Models/Item.cs":::

Azure Cosmos DB, verileri taşımak ve depolamak için JSON'u kullanır. Bu özniteliği, JSON'un `JsonProperty` nesneleri nasıl seri hale erdirip deserialize etmesini denetlemek için kullanabilirsiniz. Sınıf `Item` özniteliği `JsonProperty` gösterir. Bu kod, JSON'a giren özellik adının biçimini denetler. Ayrıca .NET özelliğini `Completed`yeniden adlandırır.

### <a name="add-views"></a><a name="add-views"></a>Görünümler ekleme

Ardından, aşağıdaki üç görünümü oluşturalım.

* Liste öğesi görünümü ekleme
* Yeni öğe görünümü ekleme
* Öğe yi ekle görünümü

#### <a name="add-a-list-item-view"></a><a name="AddItemIndexView"></a>Liste öğesi görünümü ekleme

1. **Çözüm Gezgini'nde,** **Görünümler** klasörüne sağ tıklayın ve**Yeni Klasör** **Ekle'yi** > seçin. *Klasör Öğesini*adlandırın.

1. Boş **Öğe** klasörüne sağ tıklayın ve**ardından Görünüm** **Ekle'yi** > seçin.

1. **MVC Görünümü Ekle'de**aşağıdaki değerleri sağlayın:

   * **Görünüm adına**, *Dizini*girin.
   * **Şablon'da**Liste'yi **seçin.**
   * **Model sınıfında** **Öğe'yi seçin (todo. Modeller)**.
   * **Düzen sayfası kullan'ı** seçin ve *~/Görünümler/Paylaşılan/_Layout.cshtml*girin.

   ![MVC Görünüm Ekle iletişim kutusunu gösteren ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-mvc-view.png)

1. Bu değerleri ekledikten sonra **Ekle'yi** seçin ve Visual Studio'nun yeni bir şablon görünümü oluşturmasına izin verin.

Bir kez yapılır, Visual Studio oluşturduğu *cshtml* dosyasını açar. Bu dosyayı Visual Studio'da kapatabilirsiniz. Daha sonra geri döneriz.

#### <a name="add-a-new-item-view"></a><a name="AddNewIndexView"></a>Yeni öğe görünümü ekleme

Öğeleri listelemek için bir görünüm oluşturduğunuz adedine benzer şekilde, aşağıdaki adımları kullanarak öğeleri oluşturmak için yeni bir görünüm oluşturun:

1. **Çözüm Gezgini'nde**Öğe **klasörünü** yeniden tıklatın,**Görünüm** **Ekle'yi** > seçin.

1. **MVC Görünümü Ekle'de**aşağıdaki değişiklikleri yapın:

   * **Görünüm adına**, *Oluştur*'u girin .
   * **Şablon'da** **Oluştur'u**seçin.
   * **Model sınıfında** **Öğe'yi seçin (todo. Modeller)**.
   * **Düzen sayfası kullan'ı** seçin ve *~/Görünümler/Paylaşılan/_Layout.cshtml*girin.
   * **Ekle'yi**seçin.

#### <a name="add-an-edit-item-view"></a><a name="AddEditIndexView"></a>Öğe yi ekle görünümü

Ve son olarak, aşağıdaki adımları içeren bir öğeyi görüntülemek için bir görünüm ekleyin:

1. Çözüm **Gezgini'nden** **Öğe** klasörünü yeniden tıklatın,**Görünüm** **Ekle'yi** > seçin.

1. **MVC Görünümü Ekle'de**aşağıdaki değişiklikleri yapın:

   * **Görünüm adı** kutusunda *Edit* yazın.
   * **Şablon** kutusunda **Düzenle**'yi seçin.
   * **Model sınıfı** kutusunda **Öğe (todo.Models)** seçeneğini belirleyin.
   * **Düzen sayfası kullan'ı** seçin ve *~/Görünümler/Paylaşılan/_Layout.cshtml*girin.
   * **Ekle'yi**seçin.

Bu adımları tamamladıktan sonra Visual Studio'daki tüm *cshtml* belgelerini daha sonra bu görünümlere dönerken kapatın.

### <a name="declare-and-initialize-services"></a><a name="initialize-services"></a>Hizmetleri bildirme ve başlatma

İlk olarak, Azure Cosmos DB'ye bağlanma ve kullanma mantığını içeren bir sınıf ekleyeceğiz. Bu öğretici için, bu mantığı adı verilen `CosmosDBService` bir sınıfa ve `ICosmosDBService`.' adlı bir arabirimde kapsüllayacağız. Bu hizmet CRUD işlemlerini yapar. Ayrıca, eksik öğeleri listeleme, öğeleri oluşturma, düzenleme ve silme gibi özet akışı işlemlerini de okur.

1. **Çözüm Gezgini'nde,** projenize sağ tıklayın ve**Yeni Klasör** **Ekle'yi** > seçin. Klasöre Hizmetler adını vereb'i *adlandırın.*

1. **Hizmetler** klasörüne sağ tıklayın,**Sınıf** **Ekle'yi** > seçin. Yeni sınıf *CosmosDBService'i* adlandırın ve **Ekle'yi**seçin.

1. *CosmosDBService.cs* içeriğini aşağıdaki kodla değiştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Services/CosmosDbService.cs":::

1. **Hizmetler** klasörüne sağ tıklayın,**Sınıf** **Ekle'yi** > seçin. Yeni sınıf *ICosmosDBService'i* adlandırın ve **Ekle'yi**seçin.

1. *ICosmosDBService* sınıfına aşağıdaki kodu ekleyin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Services/ICosmosDbService.cs":::

1. Çözümünüzdeki *Startup.cs* dosyasını açın `ConfigureServices` ve yöntemi aşağıdakilerle değiştirin:

    :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs" id="ConfigureServices":::

    Bu adımdaki kod, ASP.NET Core Bağımlılık [enjeksiyonu](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)yoluyla enjekte edilecek tekton örneği olarak yapılandırmaya dayalı istemciyi devreye sayar.

1. Aynı dosya içinde, yapılandırmayı okuyan ve istemciyi başharfleyen aşağıdaki yöntem **InitializeCosmosClientInstanceAsync'i**ekleyin.

   [!code-csharp[](~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs?name=InitializeCosmosClientInstanceAsync)]

1. Aşağıdaki parçacıkta gösterildiği gibi projenin *appsettings.json* dosyasındaki yapılandırmayı tanımlayın:

   :::code language="json" source="~/samples-cosmosdb-dotnet-core-web-app/src/appsettings.json":::

### <a name="add-a-controller"></a><a name="add-a-controller"></a>Denetleyici ekleme

1. **Çözüm Gezgini'nde,** **Denetleyiciler** klasörüne sağ tıklayın,**Denetleyici** **Ekle'yi** > seçin.

1. **İskele Ekle'de** **MVC Denetleyicisi 'ni** seçin - Boş alın ve **Ekle'yi**seçin.

   ![MVC Denetleyicisi seçin - İskele Ekle'de Boş](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Yeni *denetleyiciitemi ItemController*olarak adlandırın.

1. *ItemController.cs* içeriğini aşağıdaki kodla değiştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Controllers/ItemController.cs":::

**ValidateAntiForgeryToken** özniteliği burada, bu uygulamayı site arası istek sahteciliği saldırılarına karşı korumaya yardımcı olmak için kullanılır. Görüşleriniz de bu anti-sahtecilik belirteci ile çalışması gerekir. Daha fazla bilgi ve örnekler için, [ASP.NET MVC Uygulamasında Site Ötesi İstek Sahteciliği (CSRF) Saldırılarını Önleme'ye][Preventing Cross-Site Request Forgery]bakın. [GitHub][GitHub]'da sağlanan kaynak kodu tam uygulamayı içerir.

Ayrıca, aşırı deftere nakil saldırılarına karşı korunmaya yardımcı olmak için yöntem parametresi üzerindeki **Bind** özniteliğini de kullanırız. Daha fazla bilgi için [bkz: Tutorial: ASP.NET MVC'de Entity Framework ile CRUD İşlevselliği uygulayın.][Basic CRUD Operations in ASP.NET MVC]

## <a name="step-5-run-the-application-locally"></a><a name="run-the-application"></a>Adım 5: Uygulamayı yerel olarak çalıştırın

Uygulamayı yerel bilgisayarınızda sınamak için aşağıdaki adımları kullanın:

1. Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5 tuşuna basın. Bu işlemin uygulamayı oluşturması ve bir tarayıcıyı daha önce gördüğümüz boş kılavuz sayfasıyla başlatması gerekir:

   ![Bu öğretici tarafından oluşturulan todo listesi web uygulaması ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
   Uygulama bunun yerine ana sayfaya açılırsa, url'ye ekleyin. `/Item`

1. Yeni **Oluştur** bağlantısını seçin ve **Ad** ve **Açıklama** alanlarına değerler ekleyin. **Tamamlanan** onay kutusunu seçilmeden bırakın. Bunu seçerseniz, uygulama yeni öğeyi tamamlanmış bir duruma ekler. Öğe artık ilk listede görünmüyor.

1. **Oluştur'u**seçin. Uygulama sizi **Dizin** görünümüne geri gönderir ve öğeniz listede görünür. **Yapılacaklar** listenize birkaç öğe daha ekleyebilirsiniz.

    ![Dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
  
1. Listedeki bir **Öğenin** yanında **Edit'i** seçin. Uygulama, **Tamamlanan** bayrak da dahil olmak üzere nesnenizin herhangi bir özelliğini güncelleştirebileceğiniz **Edit** görünümünü açar. **Tamamlanmış'ı** ve **Kaydet'i**seçerseniz, uygulama **Öğeyi** listede tamamlanmış olarak görüntüler.

   ![Tamamlanan kutu işaretli Dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)

1. [Cosmos Explorer'ı](https://cosmos.azure.com) veya Azure Cosmos DB Emulator'un Veri Gezgini'ni kullanarak Azure Cosmos DB hizmetindeki verilerin durumunu doğrulayın.

1. Uygulamayı test ettikten sonra, uygulamanın hata ayıklamasını durdurmak için Ctrl+F5'i seçin. Dağıtıma hazırsınız!

## <a name="step-6-deploy-the-application"></a><a name="deploy-the-application-to-azure"></a>Adım 6: Uygulamayı dağıtma

Artık uygulamanın tamamı Azure Cosmos DB ile doğru şekilde çalıştığına göre, bu web uygulamasını Azure App Service’e dağıtacağız.  

1. Bu uygulamayı yayımlamak için Solution **Explorer'da** projeyi sağ tıklatın ve **Yayımla'yı**seçin.

1. **Yayımlama hedefini**seç'te, **Uygulama Hizmeti'ni**seçin.

1. Varolan bir Uygulama Hizmeti profilini kullanmak için **Varolan'ı Seç'i**seçin ve ardından **Yayımla'yı**seçin.

1. **Uygulama Hizmeti'nde** **Bir Abonelik**seçin. Kaynak grubuna veya kaynak türüne göre sıralamak için **Görünüm** filtresini kullanın.

1. Profilinizi bulun ve ardından **Tamam'ı**seçin. Sonraki arama gerekli Azure App Service ve **Tamam**seçin.

   ![Visual Studio’da App Service iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service-2019.png)

Başka bir seçenek yeni bir profil oluşturmaktır:

1. Önceki yordamda olduğu gibi, Solution **Explorer'da** projeyi sağ tıklatın ve **Yayımla'yı**seçin.
  
1. **Yayımlama hedefini**seç'te, **Uygulama Hizmeti'ni**seçin.

1. **Yayımlama hedefini seç'te**Yeni **Oluştur'u** seçin ve **Yayımla'yı**seçin.

1. **App**Service'te, Web App adınızı ve uygun abonelik, kaynak grubu ve barındırma planını girin ve ardından **Oluştur'u**seçin.

   ![Visual Studio’da App Service’i Oluştur iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service-2019.png)

Visual Studio birkaç saniye içinde web uygulamanızı yayınlar ve projenizin Azure'da çalıştığını görebileceğiniz bir tarayıcı başlatır!

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, nasıl bir ASP.NET Core MVC web uygulaması oluşturmak için öğrendim. Uygulamanız Azure Cosmos DB'de depolanan verilere erişebilir. Artık bu kaynaklarla devam edebilirsiniz:

* [Azure Cosmos DB'de bölümleme](./partitioning-overview.md)
* [SQL sorguları ile başlarken](./how-to-sql-query.md)
* [Gerçek dünyadan bir örnek kullanarak Azure Cosmos DB'de verileri modelleme ve bölümleme](./how-to-model-partition-example.md)

[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://docs.microsoft.com/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app
