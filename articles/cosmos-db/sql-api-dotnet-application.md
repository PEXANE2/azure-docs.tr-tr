---
title: Azure Cosmos DB kullanarak MVC web uygulaması öğreticisini ASP.NET Core
description: Azure Cosmos DB kullanarak bir MVC web uygulaması oluşturmak için MVC öğreticisi ASP.NET Core. Azure App Service-ASP NET Core MVC öğreticisi adım adım ' da barındırılan bir yapılacaklar uygulamasından JSON ve Access verilerini depolayacaksınız.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 02/27/2020
ms.author: sngun
ms.openlocfilehash: 1f2051addfa1266b754d230c3804834c63f89002
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274068"
---
# <a name="tutorial-develop-an-aspnet-core-mvc-web-application-with-azure-cosmos-db-by-using-net-sdk"></a>Öğretici: .NET SDK kullanarak Azure Cosmos DB ile ASP.NET Core MVC web uygulaması geliştirme

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)

Bu öğreticide Azure üzerinde barındırılan bir ASP.NET MVC uygulaması erişim verileri ve depolamak için Azure Cosmos DB kullanmayı gösterir. Bu öğreticide .NET SDK V3 ' i kullanırsınız. Aşağıdaki görüntüde, bu makaledeki örneği kullanarak oluşturacağınız Web sayfası gösterilmektedir:

![Bu öğretici tarafından oluşturulan yapılacaklar listesi MVC web uygulaması ekran görüntüsü-ASP NET Core MVC öğretici adım adım](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Öğreticiyi tamamlamaya yönelik bir zaman yoksa, tüm örnek projeyi [GitHub][GitHub]'dan indirebilirsiniz.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
>
> * Bir Azure Cosmos hesabı oluşturma
> * ASP.NET Core MVC uygulaması oluşturma
> * Uygulamayı Azure Cosmos DB'ye bağlanma
> * Veriler üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiriliyor

> [!TIP]
> Bu öğreticide, ASP.NET Core MVC ve Azure App Service kullanarak önceki deneyiminiz olduğunu varsaymaktadır. ASP.NET Core veya [Önkoşul araçları](#prerequisites)' nı yeni kullanıyorsanız, [GitHub][GitHub]'dan tüm örnek projeyi indirmeniz, gerekli NuGet paketlerini eklemeniz ve çalıştırmanız önerilir. Projeyi derledikten sonra proje bağlamında kodu daha iyi kavramak için bu makaleyi inceleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki yönergeleri izleyerek önce aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Visual Studio 2019. [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

Bu makaledeki tüm ekran görüntüleri Microsoft Visual Studio Community 2019 ' dir. Farklı bir sürüm kullanıyorsanız, ekranlarınız ve seçenekleriniz tamamen eşleşmeyebilir. Önkoşulları karşıladıysanız çözüm çalışmalıdır.

## <a name="create-an-azure-cosmos-account"></a>1. Adım: Azure Cosmos hesabı oluşturma

Bir Azure Cosmos hesabı oluşturarak başlayalım. Zaten bir Azure Cosmos DB SQL API hesabınız varsa veya Azure Cosmos DB öykünücüsü 'nü kullanıyorsanız [2. Adım: yeni bir ASP.NET MVC uygulaması oluşturma](#create-a-new-mvc-application)bölümüne atlayın.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

Sonraki bölümde, yeni bir ASP.NET Core MVC uygulaması oluşturacaksınız.

## <a name="create-a-new-mvc-application"></a>2. Adım: yeni bir ASP.NET Core MVC uygulaması oluşturma

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.

1. **Yeni proje oluştur**' da, Için C# **ASP.NET Core Web uygulaması** bulun ve seçin. Devam etmek için **İleri**’yi seçin.

   ![Yeni ASP.NET Core Web uygulaması projesi oluştur](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

1. **Yeni projenizi yapılandırın**bölümünde, projeyi *Todo* olarak adlandırın ve **Oluştur**' u seçin.

1. **Yeni bir ASP.NET Core Web uygulaması oluşturma**bölümünde **Web uygulaması (Model-View-Controller)** öğesini seçin. Devam etmek için **Oluştur** ' u seçin.

   Visual Studio boş bir MVC uygulaması oluşturur.

1. ASP.NET uygulamanızı yerel olarak çalıştırmak için hata **ayıklamayı başlatın** veya F5 > **Hata Ayıkla** ' yı seçin.

## <a name="add-nuget-packages"></a>3. Adım: projeye Azure Cosmos DB NuGet paketi ekleme

Bu çözüm için ihtiyaç duyduğumuz ASP.NET Core MVC Framework kodunun çoğunu kullandığımıza göre, Azure Cosmos DB bağlanmak için gereken NuGet paketlerini ekleyelim.

1. **Çözüm Gezgini**, projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.

1. **NuGet Paket Yöneticisi**' nde, **Microsoft. Azure. Cosmos**arayın ve seçin. **Yükle**’yi seçin.

   ![NuGet paketini yükler](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-nuget.png)

   Visual Studio, Azure Cosmos DB paketini ve bağımlılıklarını indirir ve yükler.

   NuGet paketini yüklemek için **Paket Yöneticisi konsolu 'nu** da kullanabilirsiniz. Bunu yapmak için **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin. İsteminde aşağıdaki komutu yazın:

   ```ps
   Install-Package Microsoft.Azure.Cosmos
   ```
  
## <a name="set-up-the-mvc-application"></a>4. Adım: ASP.NET Core MVC uygulamasını ayarlama

Şimdi modelleri, görünümleri ve denetleyicileri bu MVC uygulamasına ekleyelim.

### <a name="add-a-model"></a>Model ekleme

1. **Çözüm Gezgini**, **modeller** klasörüne sağ tıklayın, > **sınıfı** **Ekle** ' yi seçin.

1. **Yeni öğe Ekle**' de, yeni sınıfınızı *Item.cs* olarak adlandırın ve **Ekle**' yi seçin.

1. *Item.cs* sınıfının içeriğini şu kodla değiştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Models/Item.cs":::

Azure Cosmos DB verileri taşımak ve depolamak için JSON kullanır. `JsonProperty` özniteliğini kullanarak JSON serileştirmelerini ve nesneleri seri hale getirir. `Item` sınıfı `JsonProperty` özniteliğini gösterir. Bu kod, JSON 'a giden Özellik adının biçimini denetler. Ayrıca, `Completed`.NET özelliğini yeniden adlandırır.

### <a name="add-views"></a>Görünümler ekleme

Ardından, aşağıdaki üç görünümü oluşturalım.

* Liste öğesi görünümü ekleme
* Yeni öğe görünümü ekleme
* Düzenleme öğesi görünümü ekleme

#### <a name="AddItemIndexView"></a>Liste öğesi görünümü ekleme

1. **Çözüm Gezgini**, **Görünümler** klasörüne sağ tıklayın ve > **Yeni klasör** **Ekle** ' yi seçin. Klasör *öğesini*adlandırın.

1. Boş **öğe** klasörüne sağ tıklayın ve ardından > **görünümü** **Ekle** ' yi seçin.

1. **MVC görünümü Ekle**' de, aşağıdaki değerleri sağlayın:

   * **Görünüm adı**' nda, *Dizin*girin.
   * **Şablon**' da **liste**' yi seçin.
   * **Model sınıfı**' nda, **öğe (Todo) öğesini seçin. Modeller)** .
   * **Düzen kullan sayfasını** seçin ve *~/views/Shared/_Layout. cshtml*yazın.

   ![MVC görünümü Ekle iletişim kutusunu gösteren ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-mvc-view.png)

1. Bu değerleri ekledikten sonra **Ekle** ' yi seçin ve Visual Studio 'nun yeni bir şablon görünümü oluşturmasına izin verin.

İşiniz bittiğinde, Visual Studio oluşturduğu *cshtml* dosyasını açar. Bu dosyayı, Visual Studio 'da kapatabilirsiniz. Daha sonra bu yüklemeye geri döneceğiz.

#### <a name="AddNewIndexView"></a>Yeni öğe görünümü ekleme

Liste öğeleri için bir görünüm oluşturun aşağıdaki adımları kullanarak öğeleri oluşturmak için yeni bir görünüm nasıl oluşturduğunuz benzer:

1. **Çözüm Gezgini**, **öğe** klasörünü yeniden sağ tıklatın, > **görünümü** **Ekle** ' yi seçin.

1. **MVC görünümü Ekle**' de, aşağıdaki değişiklikleri yapın:

   * **Görünüm adı**' nda *Oluştur*' u girin.
   * **Şablon**' da **Oluştur**' u seçin.
   * **Model sınıfı**' nda, **öğe (Todo) öğesini seçin. Modeller)** .
   * **Düzen kullan sayfasını** seçin ve *~/views/Shared/_Layout. cshtml*yazın.
   * **Add (Ekle)** seçeneğini belirleyin.

#### <a name="AddEditIndexView"></a>Düzenleme öğesi görünümü ekleme

Ve son olarak, aşağıdaki adımlarla bir öğeyi düzenlemek için bir görünüm ekleyin:

1. **Çözüm Gezgini**, **öğe** klasörünü yeniden sağ tıklatın, > **görünümü** **Ekle** ' yi seçin.

1. **MVC görünümü Ekle**' de, aşağıdaki değişiklikleri yapın:

   * **Görünüm adı** kutusunda *Edit* yazın.
   * **Şablon** kutusunda **Düzenle**'yi seçin.
   * **Model sınıfı** kutusunda **Öğe (todo.Models)** seçeneğini belirleyin.
   * **Düzen kullan sayfasını** seçin ve *~/views/Shared/_Layout. cshtml*yazın.
   * **Add (Ekle)** seçeneğini belirleyin.

Bu adımları tamamladıktan sonra, bu görünümlere daha sonra geri döndüğünüzde Visual Studio 'daki tüm *cshtml* belgelerini kapatın.

### <a name="initialize-services"></a>Hizmetleri bildirme ve başlatma

İlk olarak, Azure Cosmos DB bağlanma ve kullanma mantığını içeren bir sınıf ekleyeceğiz. Bu öğreticide, bu mantığı `CosmosDBService` adlı bir sınıfa ve `ICosmosDBService`adlı bir arabirime kapsülliyoruz. Bu hizmet CRUD işlemlerini yapar. Ayrıca, tamamlanmamış öğeleri Listeleme, öğeleri oluşturma, düzenlemesi ve silme gibi akış işlemlerini de okur.

1. **Çözüm Gezgini**, projenize sağ tıklayın ve > **Yeni klasör** **Ekle** ' yi seçin. Klasör *hizmetlerini*adlandırın.

1. **Hizmetler** klasörüne sağ tıklayın, > **sınıf** **Ekle** ' yi seçin. Yeni sınıfı *Cosmosdbservice* olarak adlandırın ve **Ekle**' yi seçin.

1. *CosmosDBService.cs* içeriğini aşağıdaki kodla değiştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Services/CosmosDbService.cs":::

1. **Hizmetler** klasörüne sağ tıklayın, > **sınıf** **Ekle** ' yi seçin. Yeni sınıfı *ıosmosdbservice* olarak adlandırın ve **Ekle**' yi seçin.

1. Aşağıdaki kodu *ıcosmosdbservice* sınıfına ekleyin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Services/ICosmosDbService.cs":::

1. *Startup.cs* dosyasını çözümünüzde açın ve `ConfigureServices` yöntemini ile değiştirin:

    :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs" id="ConfigureServices":::

    Bu adımdaki kod, [ASP.NET Core ' de bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)yoluyla eklenecek tek bir örnek olarak istemciyi başlatır.

1. Aynı dosya içinde, yapılandırmayı okuyan ve istemcisini başlatan **ınitializecosmosclientınstanceasync**yöntemini ekleyin.

   [!code-csharp[](~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs?name=InitializeCosmosClientInstanceAsync)]

1. Aşağıdaki kod parçacığında gösterildiği gibi projenin *appSettings. JSON* dosyasında yapılandırmayı tanımlayın:

   :::code language="json" source="~/samples-cosmosdb-dotnet-core-web-app/src/appsettings.json":::

### <a name="add-a-controller"></a>Denetleyici ekleme

1. **Çözüm Gezgini**, **denetleyiciler** klasörüne sağ tıklayın, > **denetleyicisi** **Ekle** ' yi seçin.

1. **Yapı Iskelesi Ekle**' de **MVC denetleyicisi-boş** ' yı seçin ve **Ekle**' yi seçin.

   ![Yapı Iskelesi Ekle bölümünde MVC denetleyicisini seçin-boş](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Yeni Controller *ıtemcontroller*'ı adlandırın.

1. *ItemController.cs* içeriğini aşağıdaki kodla değiştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-core-web-app/src/Controllers/ItemController.cs":::

Bu uygulamayı siteler arası istek sahteciliği saldırılarına karşı korumaya yardımcı olmak için **Validateantiforgerıtoken** özniteliği burada kullanılır. Görünümleriniz, bu karşı koruma belirteci ile birlikte çalışmalıdır. Daha fazla bilgi ve örnek için bkz. [ASP.NET MVC uygulamasındaki siteler arası Istek forgery (CSRF) saldırılarını önleme][Preventing Cross-Site Request Forgery]. [GitHub][GitHub]'da sağlanan kaynak kodu tam uygulamayı içerir.

Ayrıca, aşırı gönderme saldırılarına karşı korumaya yardımcı olmak için yöntem parametresindeki **bind** özniteliğini de kullanırız. Daha fazla bilgi için bkz. [öğretici: ASP.NET MVC 'de Entity Framework CRUD Işlevselliği uygulama][Basic CRUD Operations in ASP.NET MVC].

## <a name="run-the-application"></a>5. Adım: uygulamayı yerel olarak çalıştırma

Uygulamayı yerel bilgisayarınızda test etmek için aşağıdaki adımları kullanın:

1. Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5 tuşuna basın. Bu işlemin uygulamayı oluşturması ve bir tarayıcıyı daha önce gördüğümüz boş kılavuz sayfasıyla başlatması gerekir:

   ![Bu öğretici tarafından oluşturulan yapılacaklar listesi Web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
   Uygulama bunun yerine giriş sayfasında açılırsa URL 'ye `/Item` ekleyin.

1. **Yeni oluştur** bağlantısını seçin ve **ad** ve **Açıklama** alanlarına değer ekleyin. **Tamamlandı** onay kutusunu seçilmemiş olarak bırakın. Bu seçeneği belirlerseniz, uygulama yeni öğeyi tamamlanmış durumuna ekler. Öğe artık ilk listede görünmüyor.

1. **Oluştur**’u seçin. Uygulama sizi **Dizin** görünümüne geri gönderir ve öğe listede görüntülenir. **Yapılacaklar** listenize birkaç öğe ekleyebilirsiniz.

    ![Dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
  
1. Listedeki bir **öğenin** yanındaki **Düzenle** ' yi seçin. Uygulama, **Tamamlanan** bayrak dahil olmak üzere, nesnenizin herhangi bir özelliğini güncelleştirebileceğiniz **düzenleme** görünümünü açar. **Tamamlandı** ' ı ve **Kaydet**' i seçerseniz, uygulama **öğeyi** listede tamamlandı olarak görüntüler.

   ![Tamamlandı kutusu işaretli dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)

1. [Cosmos Gezginini](https://cosmos.azure.com) veya Azure Cosmos DB öykünücü Veri Gezgini kullanarak Azure Cosmos DB hizmetindeki verilerin durumunu doğrulayın.

1. Uygulamayı sınadıktan sonra, uygulamanın hata ayıklamasını durdurmak için CTRL + F5 ' i seçin. Dağıtıma hazırsınız!

## <a name="deploy-the-application-to-azure"></a>6. Adım: uygulamayı dağıtma

Artık uygulamanın tamamı Azure Cosmos DB ile doğru şekilde çalıştığına göre, bu web uygulamasını Azure App Service’e dağıtacağız.  

1. Bu uygulamayı yayımlamak için **Çözüm Gezgini** ' de projeye sağ tıklayın ve **Yayımla**' yı seçin.

1. **Bir yayımlama hedefi**seçin kısmında **App Service**' yi seçin.

1. Mevcut bir App Service profilini kullanmak için **Varolanı Seç**' i seçin ve ardından **Yayımla**' yı seçin.

1. **App Service**bir **abonelik**seçin. Kaynak grubuna veya kaynak türüne göre sıralamak için **Görünüm** filtresini kullanın.

1. Profilinizi bulun ve **Tamam**' ı seçin. Sonra gerekli Azure App Service arayın ve **Tamam**' ı seçin.

   ![Visual Studio’da App Service iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service-2019.png)

Başka bir seçenek de yeni bir profil oluşturmaktır:

1. Önceki yordamda olduğu gibi **Çözüm Gezgini** projeye sağ tıklayın ve **Yayımla**' yı seçin.
  
1. **Bir yayımlama hedefi**seçin kısmında **App Service**' yi seçin.

1. **Bir yayımlama hedefi**seçin bölümünde **Yeni oluştur** ' u seçin ve **Yayımla**' yı seçin.

1. **App Service**, Web uygulaması adınızı ve uygun aboneliği, kaynak grubunu ve barındırma planını girip **Oluştur**' u seçin.

   ![Visual Studio’da App Service’i Oluştur iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service-2019.png)

Visual Studio, birkaç saniye içinde Web uygulamanızı yayımlar ve projenizi Azure 'da çalıştırdığınız yerde görebileceğiniz bir tarayıcı başlatır!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ASP.NET Core MVC web uygulaması oluşturmayı öğrendiniz. Uygulamanız, Azure Cosmos DB depolanan verilere erişebilir. Artık şu kaynaklarla devam edebilirsiniz:

* [Azure Cosmos DB'de bölümleme](./partitioning-overview.md)
* [SQL sorgularıyla çalışmaya başlama](./how-to-sql-query.md)
* [Gerçek dünyadan bir örnek kullanarak Azure Cosmos DB'de verileri modelleme ve bölümleme](./how-to-model-partition-example.md)

[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://docs.microsoft.com/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app
