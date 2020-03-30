---
title: .Net V4 SDK kullanarak Azure Cosmos DB SQL API kaynaklarını yönetme
description: Azure Cosmos DB SQL API hesap kaynaklarını yönetmek için .Net V4 SDK'yı kullanarak bir konsol uygulaması oluşturmaya hızlı başlayın.
author: ealsur
ms.author: maquaran
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/10/2020
ms.openlocfilehash: b69d67a5c4fc1d907f676cf4e400f9fa7df2653b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77585943"
---
# <a name="quickstart-build-a-console-app-using-the-net-v4-sdk-to-manage-azure-cosmos-db-sql-api-account-resources"></a>Hızlı başlangıç: Azure Cosmos DB SQL API hesap kaynaklarını yönetmek için .Net V4 SDK'yı kullanarak bir konsol uygulaması oluşturun.

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

.NET için Azure Cosmos DB SQL API istemci kitaplığı ile başlayın. .NET V4 (Azure.Cosmos) paketini yüklemek, bir uygulama oluşturmak ve Azure Cosmos DB'de depolanan verilerdeki temel CRUD işlemleri için örnek kodu denemek için bu dokümandaki adımları izleyin. 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Anahtar/değer, belge ve grafik veritabanlarını hızla oluşturmak ve sorgulamak için Azure Cosmos DB'yi kullanabilirsiniz. Şu şekilde .NET için Azure Cosmos DB SQL API istemci kitaplığını kullanın:

* Azure Cosmos veritabanı ve kapsayıcı oluşturma
* Kapsayıcıya örnek veri ekleme
* Verileri sorgulama 
* Veritabanını silme

[Kütüphane kaynak kodu](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/v4) | [Paketi (NuGet)](https://www.nuget.org/packages/Azure.Cosmos)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/) veya Azure aboneliği olmadan ücretsiz, ücretsiz ve taahhütler için Azure [Cosmos DB'yi ücretsiz olarak deneyebilirsiniz.](https://azure.microsoft.com/try/cosmosdb/) 
* [NET Çekirdek 3 SDK](https://dotnet.microsoft.com/download/dotnet-core). Ortamınızda hangi sürümün kullanılabildiği çalıştırılarak `dotnet --version`doğrulayabilirsiniz.

## <a name="setting-up"></a>Ayarlama

Bu bölüm, bir Azure Cosmos hesabı oluşturmanız ve kaynakları yönetmek için .NET için Azure Cosmos DB SQL API istemci kitaplığını kullanan bir proje oluşturmanız için size yol kat maktadır. Bu makalede açıklanan örnek kod, `FamilyDatabase` bu veritabanıiçinde bir veritabanı ve aile üyeleri (her aile üyesi bir öğedir) oluşturur. Her aile üyesinin `Id, FamilyName, FirstName, LastName, Parents, Children, Address,`. Özellik `LastName` kapsayıcı için bölme anahtarı olarak kullanılır. 

### <a name="create-an-azure-cosmos-account"></a><a id="create-account"></a>Azure Cosmos hesabı oluşturma

Azure Cosmos hesabı oluşturmak için ücretsiz seçenek için [Azure Cosmos DB'yi deneyin'i](https://azure.microsoft.com/try/cosmosdb/) kullanıyorsanız, **SQL API**türünden bir Azure Cosmos DB hesabı oluşturmanız gerekir. Sizin için zaten bir Azure Cosmos DB test hesabı oluşturuldu. Hesabı açıkça oluşturmanız gerekmesin, böylece bu bölümü atlayıp bir sonraki bölüme geçebilirsiniz.

Kendi Azure aboneliğiniz varsa veya ücretsiz bir abonelik oluşturduysanız, açıkça bir Azure Cosmos hesabı oluşturmanız gerekir. Aşağıdaki kod, oturum tutarlılığı olan bir Azure Cosmos hesabı oluşturur. Hesap çoğaltılır `South Central US` ve `North Central US`.  

Azure Cosmos hesabını oluşturmak için Azure Bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell, Azure kaynaklarını yönetmek için kullanabileceğiniz etkileşimli, kimliği doğrulanmış ve tarayıcı ile erişilebilen bir kabuktur. Bu hizmet, Bash veya PowerShell ile çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği getirir. Bu hızlı başlangıç için **Bash** modunu seçin. Azure Cloud Shell de bir depolama hesabı gerektirir, istendiğinde bir tane oluşturabilirsiniz.

Aşağıdaki kodun yanındaki **Try It** düğmesini seçin, **Bash** modunu seçin bir depolama **hesabı oluşturun** ve Cloud Shell'e giriş yapın. Sonraki kopyalayıp aşağıdaki kodu Azure bulut kabuğuna yapıştırın ve çalıştırın. Azure Cosmos hesap adı genel olarak benzersiz olmalıdır, `mysqlapicosmosdb` komutu çalıştırmadan önce değeri güncelleştirdiğinizden emin olun.

```azurecli-interactive

# Set variables for the new SQL API account, database, and container
resourceGroupName='myResourceGroup'
location='southcentralus'

# The Azure Cosmos account name must be globally unique, make sure to update the `mysqlapicosmosdb` value before you run the command
accountName='mysqlapicosmosdb'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a SQL API Cosmos DB account with session consistency and multi-master enabled
az cosmosdb create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --kind GlobalDocumentDB \
    --locations regionName="South Central US" failoverPriority=0 --locations regionName="North Central US" failoverPriority=1 \
    --default-consistency-level "Session" \
    --enable-multiple-write-locations true

```

Azure Cosmos hesabının oluşturulması biraz zaman alır, işlem başarılı olduğunda onay çıktısını görebilirsiniz. Komut başarıyla tamamlandıktan sonra [Azure portalında](https://portal.azure.com/) oturum açın ve belirtilen ada sahip Azure Cosmos hesabının var olduğunu doğrulayın. Kaynak oluşturulduktan sonra Azure Bulut Bulutu penceresini kapatabilirsiniz. 

### <a name="create-a-new-net-app"></a><a id="create-dotnet-core-app"></a>Yeni bir .NET uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET uygulaması oluşturun. Yerel bilgisayarınızdan Windows komut istemini veya Terminal penceresini açın. Komut istemi veya terminalsonraki bölümlerdeki tüm komutları çalıştırın.  Adı `todo`ile yeni bir uygulama oluşturmak için aşağıdaki dotnet yeni komutu çalıştırın. --langVersion parametresi, oluşturulan proje dosyasındaki LangVersion özelliğini ayarlar.

   ```bash
   dotnet new console –langVersion:8 -n todo
   ```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

   ```bash
   cd todo
   dotnet build
   ```

Yapıdan beklenen çıktı şuna benzer:

```bash
  Restore completed in 100.37 ms for C:\Users\user1\Downloads\CosmosDB_Samples\todo\todo.csproj.
  todo -> C:\Users\user1\Downloads\CosmosDB_Samples\todo\bin\Debug\netcoreapp3.0\todo.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:34.17
```

### <a name="install-the-azure-cosmos-db-package"></a><a id="install-package"></a>Azure Cosmos DB paketini yükleme

Uygulama dizinindeyken, dotnet ekle paket komutunu kullanarak .NET Core için Azure Cosmos DB istemci kitaplığını yükleyin.

   ```bash
   dotnet add package Azure.Cosmos --version 4.0.0-preview3
   ```

### <a name="copy-your-azure-cosmos-account-credentials-from-the-azure-portal"></a>Azure Cosmos hesap kimlik bilgilerinizi Azure portalından kopyalama

Örnek uygulamanın Azure Cosmos hesabınızda kimliğinin doğrulanması gerekir. Kimlik doğrulaması yapmak için Azure Cosmos hesap kimlik bilgilerini uygulamaya geçirmeniz gerekir. Aşağıdaki adımları izleyerek Azure Cosmos hesap kimlik bilgilerinizi alın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure Cosmos hesabınıza gidin.

1. **Keys** bölmesini açın ve hesabınızın **URI** ve **BIRINCIL ANAHTARINI** kopyalayın. Bir sonraki adımda URI ve tuş değerlerini bir ortam değişkenine eklersiniz.

## <a name="object-model"></a><a id="object-model"></a>Nesne modeli

Uygulamayı oluşturmaya başlamadan önce, Azure Cosmos DB'deki kaynaklar hiyerarşisini ve bu kaynakları oluşturmak ve bunlara erişmek için kullanılan nesne modelini inceleyelim. Azure Cosmos DB aşağıdaki sırada kaynak oluşturur:

* Azure Cosmos hesabı 
* Veritabanları 
* Kapsayıcılar 
* Öğeler

Farklı varlıkların hiyerarşisi hakkında daha fazla bilgi edinmek için [Azure Cosmos DB makalesinde veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md) bölümüne bakın. Bu kaynaklarla etkileşim kurmak için aşağıdaki .NET sınıflarını kullanırsınız:

* CosmosClient - Bu sınıf, Azure Cosmos DB hizmeti için istemci tarafı mantıksal gösterimi sağlar. İstemci nesnesi, istekleri hizmete karşı yapılandırmak ve yürütmek için kullanılır.
* CreateDatabaseIfNotExistsAsync - Bu yöntem (yoksa) oluşturur veya (zaten varsa) bir veritabanı kaynağı asynchronous işlem olarak alır. 
* CreateContainerIfNotExistsAsync - Bu yöntem (yoksa) oluşturur veya (zaten varsa) bir kapsayıcı bir eşzamanlı işlem olarak alır. Kapsayıcının yeni oluşturulduğunu (201) veya varolan bir kapsayıcının döndürülüp döndürülmediğini (200) belirlemek için yanıttan durum kodunu denetleyebilirsiniz. 
* CreateItemAsync - Bu yöntem kapsayıcı içinde bir öğe oluşturur.
* UpsertItemAsync - Bu yöntem, zaten yoksa kapsayıcı içinde bir öğe oluşturur veya zaten varsa öğenin yerini alır. 
* GetItemQueryIterator - Bu yöntem, parametreli değerlere sahip bir SQL deyimi kullanarak Azure Cosmos veritabanında bir kapsayıcı altında öğeler için bir sorgu oluşturur. 
* DeleteAsync - Belirtilen veritabanını Azure Cosmos hesabınızdan siler. `DeleteAsync`yöntem yalnızca veritabanını siler.

 ## <a name="code-examples"></a><a id="code-examples"></a>Kod örnekleri

Bu makalede açıklanan örnek kod, Azure Cosmos DB'de bir aile veritabanı oluşturur. Aile veritabanı, ad, adres, konum, ilişkili ebeveynler, çocuklar ve evcil hayvanlar gibi aile ayrıntılarını içerir. Verileri Azure Cosmos hesabınıza doldurmadan önce bir aile öğesinin özelliklerini tanımlayın. Örnek uygulamanızın `Family.cs` kök düzeyinde adlandırılmış yeni bir sınıf oluşturun ve bu sınıfa aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Family.cs)]

### <a name="add-the-using-directives--define-the-client-object"></a>İstemci nesnesini tanımlamak & kullanarak yönergeleri ekleme

Proje dizininden, düzenleyicinizdeki `Program.cs` dosyayı açın ve uygulamanızın üst kısmındaki yönergeleri kullanarak aşağıdaki yönergeleri ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=Usings)]


Sınıfınızda `Program` aşağıdaki genel değişkenleri ekleyin. Bunlar bitiş noktası ve yetkilendirme anahtarlarını, veritabanının adını ve oluşturacağınız kapsayıcıyı içerir. Ortamınıza göre bitiş noktası ve yetkilendirme anahtarları değerlerini değiştirdiğinizden emin olun. 

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=Constants)]

Son olarak, `Main` yöntemi değiştirin:

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=Main)]

### <a name="create-a-database"></a>Veritabanı oluşturma 

`CreateDatabaseAsync` Yöntemsınıf içinde `program.cs` tanımlayın. Bu yöntem, `FamilyDatabase` if'in zaten var olmadığını oluşturur.

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=CreateDatabaseAsync)]

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

`CreateContainerAsync` Yöntemsınıf içinde `Program` tanımlayın. Bu yöntem, `FamilyContainer` if'in zaten var olmadığını oluşturur. 

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=CreateContainerAsync)]

### <a name="create-an-item"></a>Öğe oluşturma

`AddItemsToContainerAsync` Yöntemi aşağıdaki kodla ekleyerek bir aile öğesi oluşturun. Bir öğe `CreateItemAsync` oluşturmak `UpsertItemAsync` için veya yöntemleri kullanabilirsiniz:

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=AddItemsToContainerAsync)]

### <a name="query-the-items"></a>Öğeleri sorgula

Bir öğe ekledikten sonra, "Andersen" ailesinin ayrıntılarını almak için bir sorgu çalıştırabilirsiniz. Aşağıdaki kod, doğrudan SQL sorgusunu kullanarak sorgunun nasıl yürütüleceklerini gösterir. "Anderson" aile bilgilerini almak için SQL `SELECT * FROM c WHERE c.LastName = 'Andersen'`sorgusu: . Sınıf `QueryItemsAsync` içindeki yöntemi `Program` tanımlayın ve buna aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=QueryItemsAsync)]

### <a name="replace-an-item"></a>Bir öğeyi değiştirme 

Bir aile öğesini `ReplaceFamilyItemAsync` okuyun ve ardından aşağıdaki kodla yöntemi ekleyerek güncelleştirin.

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=ReplaceFamilyItemAsync)]

### <a name="delete-an-item"></a>Öğeyi silme 

Aşağıdaki kodla yöntemi `DeleteFamilyItemAsync` ekleyerek bir aile öğesini silin.

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=DeleteFamilyItemAsync)]

### <a name="delete-the-database"></a>Veritabanını silme 

Son olarak aşağıdaki kodu `DeleteDatabaseAndCleanupAsync` ile yöntem ekleyerek veritabanı silebilir:

[!code-csharp[Main](~/cosmos-dotnet-v4-getting-started/src/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

Gerekli tüm yöntemleri ekledikten sonra `Program` dosyayı kaydedin. 

## <a name="run-the-code"></a>Kodu çalıştırma

Sonraki yapı ve Azure Cosmos DB kaynakları oluşturmak için uygulama çalıştırın.

   ```bash
   dotnet run
   ```

Uygulamayı çalıştırdığınızda aşağıdaki çıktı oluşturulur. Azure portalında oturum açabilir ve kaynakların oluşturulduğunu doğrulayabilirsiniz:

   ```bash
   Created Database: FamilyDatabase
   
   Created Container: FamilyContainer
   
   Created item in database with id: Andersen.1
   
   Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'
   
           Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null   "FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets": [{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}
   
   Updated Family [Wakefield,Wakefield.7].
           Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"}   {"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6   "Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1   "Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}
   
   Deleted Family [Wakefield,Wakefield.7]
   
   Deleted Database: FamilyDatabase
   
   End of demo, press any key to exit.
   ```

Verilerin Azure portalında oturum açarak oluşturulduğunu doğrulayabilir ve Azure Cosmos hesabınızda gerekli öğeleri görebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, Azure Cosmos hesabını ve ilgili kaynak grubunu kaldırmak için Azure CLI veya Azure PowerShell'i kullanabilirsiniz. Aşağıdaki komut, Azure CLI'yi kullanarak kaynak grubunun nasıl silinir olduğunu gösterir:

```azurecli
az group delete -g "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos hesabı oluşturmayı, bir veritabanı ve bir kapsayıcı oluşturmayı bir .NET Core uygulamasını kullanarak öğrendiniz. Artık aşağıdaki makalede yer alan yönergelerle Azure Cosmos hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
