---
title: Azure SignalR Hizmetini nasıl kullanacağınızı öğrenmek için hızlı başlangıç
description: Azure SignalR Hizmeti’ni kullanarak ASP.NET Core MVC uygulamaları ile bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/04/2019
ms.author: zhshang
ms.openlocfilehash: 022780f2b37c8bed49c81774d443b69bae41e5e7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73476753"
---
# <a name="quickstart-create-a-chat-room-by-using-signalr-service"></a>Quickstart: SignalR Hizmetini kullanarak sohbet odası oluşturun


Azure SignalR Hizmeti, geliştiricilerin gerçek zamanlı özelliklerle web uygulamalarını kolayca derlemesine yardımcı olan bir Azure hizmetidir. Bu hizmet [ASP.NET Core 2.1 için SignalR'ı](https://docs.microsoft.com/aspnet/core/signalr/introduction?view=aspnetcore-2.1)temel alan ancak [ASP.NET Core 3.0 için SignalR'ı](https://docs.microsoft.com/aspnet/core/signalr/introduction?view=aspnetcore-3.0)da destekler.

Bu makalede Azure SignalR Hizmeti ile çalışmaya başlama işlemi gösterilmektedir. Bu hızlı başlangıçta, ASP.NET Core MVC web uygulaması kullanarak bir sohbet uygulaması oluşturursunuz. Bu uygulama, gerçek zamanlı içerik güncelleştirmelerini etkinleştirmek üzere Azure SignalR Hizmeti kaynağınızla bağlantı kuracaktır. Web uygulamasını yerel olarak barındıracak ve birden çok tarayıcı istemcisiyle bağlantı kuracaksınız. Her istemci, diğer tüm istemcilere içerik güncelleştirmeleri gönderebilecektir. 

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Bir seçenek [Visual Studio Code](https://code.visualstudio.com/), Windows, macOS ve Linux platformlarında kullanılabilir.

Bu öğreticinin kodu [AzureSignalR-samples GitHub deposundan](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom) indirilebilir. Ayrıca, [signalr hizmeti komut dosyası oluştur'u](scripts/signalr-cli-create-service.md)izleyerek bu hızlı başlatmada kullanılan Azure kaynaklarını da oluşturabilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

* [.NET Çekirdek SDK'yı](https://www.microsoft.com/net/download/windows)yükleyin.
* [AzureSignalR örnekli](https://github.com/aspnet/AzureSignalR-samples) GitHub deposunu indirin veya klonla. 

## <a name="create-an-azure-signalr-resource"></a>Azure SignalR kaynağı oluşturma

[!INCLUDE [azure-signalr-create](../../includes/signalr-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Bu bölümde, bir ASP.NET Core MVC web uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanırsınız. .NET Core CLI'yi Visual Studio'ya göre kullanmanın avantajı, Windows, macOS ve Linux platformlarında kullanılabilmesidir. 

1. Projeniz için bir klasör oluşturun. Bu hızlı *başlatma, E:\Testing\chattest* klasörünü kullanır.

2. Yeni klasörde, projeyi oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet new mvc


## <a name="add-secret-manager-to-the-project"></a>Projeye Gizli Dizi Yöneticisi ekleme

Bu bölümde, Projenize [Gizli Yönetici aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) eklersiniz. Gizli Yönetici aracı, geliştirme çalışmaları için hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama sırlarının kaynak kodda yanlışlıkla paylaşılmasını önlemeye yardımcı olur.

1. *.csproj* dosyanızı açın. *Microsoft.Extensions.SecretManager.Tools* öğesini dahil etmek için bir `DotNetCliToolReference` öğesi ekleyin. Ayrıca `UserSecretsId` *chattest.csproj*için aşağıdaki kodda gösterildiği gibi bir öğe ekleyin ve dosyayı kaydedin.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <UserSecretsId>SignalRChatRoomEx</UserSecretsId>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    </ItemGroup>
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
        <DotNetCliToolReference Include="Microsoft.Extensions.SecretManager.Tools" Version="2.0.0" />
    </ItemGroup>
    </Project>    
    ```

## <a name="add-azure-signalr-to-the-web-app"></a>Web uygulamasına Azure SignalR ekleme

1. `Microsoft.Azure.SignalR` Aşağıdaki komutu çalıştırarak NuGet paketine bir başvuru ekleyin:

        dotnet add package Microsoft.Azure.SignalR

2. Projeniz için paketleri geri yüklemek için aşağıdaki komutu çalıştırın:

        dotnet restore

3. Gizli Yönetici'ye *Azure:SignalR:ConnectionString* adlı bir sır ekleyin. 

    Bu gizli dizi, SignalR Hizmetinizin kaynağına erişmeye yarayan bağlantı dizesini içerir. *Azure:SignalR:ConnectionString,* SignalR'ın bağlantı kurmak için aradığı varsayılan yapılandırma anahtarıdır. Aşağıdaki komuttaki değeri SignalR Service kaynağınızın bağlantı dizesiyle değiştirin.

    Bu komutu *.csproj* dosyasıyla aynı dizinde çalıştırmalısınız.

    ```
    dotnet user-secrets set Azure:SignalR:ConnectionString "<Your connection string>"    
    ```

    Secret Manager yalnızca web uygulamasını yerel olarak barındırılan web uygulamasını test etmek için kullanılır. Daha sonraki bir öğreticide, sohbet web uygulamasını Azure'a dağıtacaksınız. Web uygulaması Azure'a dağıtıldıktan sonra, bağlantı dizesini Gizli Yönetici ile depolamak yerine bir uygulama ayarı kullanırsınız.

    Bu gizli Yapılandırma API ile erişilir. Bir kolon (:) desteklenen tüm platformlarda Configuration API ile yapılandırma adı üzerinde çalışır. [Ortama göre Yapılandırma'ya](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0)bakın. 


4. *yalnızca* ASP.NET Core `ConfigureServices` 2 yöntemini `services.AddSignalR().AddAzureSignalR()` arayarak Azure SinyalR Hizmetini kullanma yöntemini Startup.cs açın ve güncelleştirme:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        services.AddSignalR().AddAzureSignalR();
    }
    ```
    Core 3+ ASP.NET için yöntem için `ConfigureServices` değişiklik gerekmez.

    Bir parametreyi `AddAzureSignalR()`geçerek, bu kod SignalR Service kaynak bağlantı dizesi için varsayılan yapılandırma anahtarını kullanır. Varsayılan yapılandırma anahtarı *Azure:SignalR:ConnectionString'* tir.

5. Ayrıca *Startup.cs,* aramayı `Configure` aşağıdaki `app.UseStaticFiles()` kodla değiştirerek yöntemi güncelleştirin ve yalnızca ASP.NET Core 2 için dosyayı kaydedin.

    ```csharp
    app.UseFileServer();
    app.UseAzureSignalR(routes =>
    {
        routes.MapHub<Chat>("/chat");
    });
    ```            
    Core 3+ ASP.NET için yukarıdaki kodu aşağıdakilerle değiştirin:

    ```csharp
    app.UseFileServer();
    app.UseRouting();
    app.UseAuthorization();

    app.UseEndpoints(routes =>
    {
        routes.MapHub<Chat>("/chat");
    });
    ```

### <a name="add-a-hub-class"></a>Hub sınıfı ekleme

SignalR'da hub, istemciden çağrılabilen bir dizi yöntemi ortaya çıkaran temel bir bileşendir. Bu bölümde, iki yöntemle bir hub sınıf tanımlayacaksınız: 

* `Broadcast`: Bu yöntem bir iletiyi tüm istemcilere yayınlar.
* `Echo`: Bu yöntem bir iletiyi çağırana geri gönderir.

Her iki `Clients` yöntem de ASP.NET Core SignalR SDK'nın sağladığı arabirimi kullanır. Bu arabirim, tüm bağlı istemcilere erişim sağlar, böylece içeriği müşterilerinize iletebilirsiniz.

1. Proje dizininizde *Hub* adlı yeni bir klasör ekleyin. Yeni klasöre *Chat.cs* adlı yeni bir hub kod dosyası ekleyin.

2. Hub sınıfınızı tanımlamak ve dosyayı kaydetmek için *Chat.cs* için aşağıdaki kodu ekleyin. 

    *Chattest* dışında bir proje adı kullandıysanız bu sınıfın ad alanını güncelleştirin.

    ```csharp
    using Microsoft.AspNetCore.SignalR;

    namespace chattest
    {

        public class Chat : Hub
        {
            public void BroadcastMessage(string name, string message)
            {
                Clients.All.SendAsync("broadcastMessage", name, message);
            }

            public void Echo(string name, string message)
            {
                Clients.Client(Context.ConnectionId).SendAsync("echo", name, message + " (echo from server)");
            }
        }
    }
    ```

### <a name="add-the-client-interface-for-the-web-app"></a>Web uygulaması için istemci arabirimini ekleme

Bu sohbet odası uygulamasının istemci kullanıcı *arabirimi, wwwroot* dizininde *index.html* adlı bir dosyada HTML ve JavaScript'ten oluşacaktır.

*Index.html* dosyasını, *css* klasörünü ve *komut dosyalarını* [örnekler deposunun](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom/wwwroot) *wwwroot* klasöründen kopyalayın. Bunları projenizin *wwwroot* klasörüne yapıştırın.

Burada *index.html*ana kodu: 

```javascript
var connection = new signalR.HubConnectionBuilder()
                            .withUrl('/chat')
                            .build();
bindConnectionMessage(connection);
connection.start()
    .then(function () {
        onConnected(connection);
    })
    .catch(function (error) {
        console.error(error.message);
    });
```    

*Index.html'deki* kod, Azure SignalR kaynağına HTTP bağlantısı yapmak için çağrıda bulunur. `HubConnectionBuilder.build()`

Bağlantı başarılı olursa, o bağlantı `bindConnectionMessage` konumuna geçirilir ve istemciye gönderilen gelen içerik için olay işleyicileri ekler. 

`HubConnection.start()`, hub ile iletişim başlatır. Ardından, `onConnected()` düğme olay işleyicileri ekler. Bu işleyiciler, bağlantıyı kullanarak bu istemcinin tüm bağlı istemcilere içerik güncelleştirmeleri göndermesine olanak tanır.

## <a name="add-a-development-runtime-profile"></a>Geliştirme çalışma zamanı profili ekleme

Bu bölümde, ASP.NET Core için bir geliştirme çalışma zamanı ortamı eklersiniz. Daha fazla bilgi için ASP.NET [Core'da birden fazla ortamla çalışma](https://docs.microsoft.com/aspnet/core/fundamentals/environments)'ya bakın.

1. Projenizde *Özellikler* adlı bir klasör oluşturun.

2. Aşağıdaki içeriği içeren *klasöre launchSettings.json* adlı yeni bir dosya ekleyin ve dosyayı kaydedin.

    ```json
    {
        "profiles" : 
        {
            "ChatRoom": 
            {
                "commandName": "Project",
                "launchBrowser": true,
                "environmentVariables": 
                {
                    "ASPNETCORE_ENVIRONMENT": "Development"
                },
                "applicationUrl": "http://localhost:5000/"
            }
        }
    }
    ```


## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. .NET Core CLI kullanarak uygulamayı oluşturmak için komut kabuğunda aşağıdaki komutu çalıştırın:

        dotnet build

2. Yapı başarıyla bittikten sonra, web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

    Uygulama, geliştirme çalışma zamanı profilimizde de belirtildiği gibi, 5000 no'lu bağlantı noktasında yerel olarak barındırılacaktır:

        E:\Testing\chattest>dotnet run
        Hosting environment: Development
        Content root path: E:\Testing\chattest
        Now listening on: http://localhost:5000
        Application started. Press Ctrl+C to shut down.    

3. İki tarayıcı penceresi açın. Her tarayıcıda, `http://localhost:5000`' ye gidin. Adınızı girmeniz istenir. **Gönder** düğmesini kullanarak her iki istemci için bir istemci adı girin ve her iki istemci arasında ileti içeriğini test edin.

    ![Azure SignalR grup sohbeti örneği](media/signalr-quickstart-dotnet-core/signalr-quickstart-complete-local.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye devam ederseniz, oluşturulan kaynakları bu hızlı başlatmada tutup yeniden kullanabilirsiniz.

Hızlı başlatma örnek uygulamasını bitirdiyseniz, ücretlendirmeleri önlemek için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Kaynak grubunu silerken geri alınamaz ve bu gruptaki tüm kaynakları içerir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği tutmak istediğiniz kaynakları içeren varolan bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı bıçağdan ayrı ayrı silebilirsiniz.
> 
> 

[Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.

Ad metin kutusuna **göre Filtre'de** kaynak grubunuzun adını yazın. Bu hızlı başlangıçtaki yönergelerde *SignalRTestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda elips (**...**) > **Kaynak grubunu sil'i**seçin.

   
![Kaynak grubunu silme ye yönelik seçimler](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)


Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını girin ve **Sil'i**seçin.
   
Birkaç dakika sonra kaynak grubu ve bu gruptaki kaynakların tümü silinir.



## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Azure Sinyal Hizmeti kaynağı oluşturdunuz. Daha sonra içerik güncellemelerini birden çok bağlı istemciye gerçek zamanlı olarak itmek için bir ASP.NET Core web uygulamasıyla kullandınız. Azure SinyalR Hizmetini kullanma hakkında daha fazla bilgi edinmek için kimlik doğrulamasını gösteren öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure SignalR Hizmeti kimlik doğrulaması](./signalr-concept-authenticate-oauth.md)


