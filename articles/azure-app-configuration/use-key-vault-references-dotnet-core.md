---
title: ASP.NET Core uygulamasındaki Azure uygulama yapılandırma Key Vault başvurularını kullanma öğreticisi | Microsoft Docs
description: Bu öğreticide, Azure Uygulama yapılandırması 'nın Key Vault başvurularını ASP.NET Core bir uygulamadan nasıl kullanacağınızı öğrenirsiniz.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/07/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: a14cb3035c159c82df44f686da7f7b78ef942943
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035861"
---
# <a name="tutorial-use-key-vault-references-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulamasındaki Key Vault başvurularını kullanma

Bu öğreticide, Azure Key Vault ile birlikte Azure uygulama yapılandırma hizmetini kullanmayı öğreneceksiniz. Bunlar, çoğu uygulama dağıtımında yan yana kullanılacak tamamlayıcı hizmetlerdir. Uygulama yapılandırması, birlikte kullanmanıza yardımcı olmak için Key Vault depolanan değerlere başvuruda bulunan anahtarlar oluşturmanıza olanak sağlar. Bunu yaptığınızda, uygulama yapılandırması URI 'yi değerin kendisi yerine Key Vault değerine depolar. Uygulamanız uygulama yapılandırması istemci sağlayıcısını kullanarak bu anahtarın değerini uygulama yapılandırmasında depolanan diğer tüm anahtarlarınız gibi alır. İstemci sağlayıcısı bunu bir Key Vault başvurusu olarak tanır ve değeri almak için Key Vault çağırır. Uygulamanız hem uygulama yapılandırması hem de Key Vault için düzgün şekilde kimlik doğrulaması yapmaktan sorumludur. İki hizmet doğrudan iletişim kurmaz.

Bu öğreticide, kodunuzda Key Vault başvurularını nasıl uygulayabileceğinizi gösterir. Hızlı başlangıçlarda tanıtılan web uygulamasında oluşturulur. Devam etmeden önce, önce [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturun](./quickstart-aspnet-core-app.md) .

Bu öğreticideki adımları uygulamak için herhangi bir kod düzenleyicisi kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) , Windows, MacOS ve Linux platformlarında kullanılabilen harika bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Key Vault depolanan bir değere başvuran bir uygulama yapılandırma anahtarı oluşturun
> * Bu anahtarın değerine bir ASP.NET Core Web uygulamasından erişin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi yapmak için [.NET Core SDK](https://dotnet.microsoft.com/download)' yi yüklemelisiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** seçeneğini belirleyin

    ![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](./media/quickstarts/search-services.png)
2. Arama kutusuna **Key Vault** yazın.
3. Sonuç listesinden **Key Vault**’u seçin.
4. Key Vault bölümünde **Oluştur**’u seçin.
5. **Anahtar kasası oluşturma** bölümünde aşağıdaki bilgileri sağlayın:
    - **Ad**: Benzersiz bir ad gereklidir. Bu hızlı başlangıçta **contoso-vault2**kullanıyoruz. 
    - **Abonelik**: Bir abonelik seçin.
    - **Kaynak grubu**altında **Yeni oluştur** ' u seçin ve bir kaynak grubu adı girin.
    - **Konum** açılır menüsünden bir konum seçin.
    - Diğer seçenekleri varsayılan değerlerinde bırakın.
6. Yukarıdaki bilgileri girdikten sonra **Oluştur**’u seçin.

Bu noktada, Azure hesabınız, bu yeni kasaya erişme yetkisine sahip tek bir hesaptır.

![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](./media/quickstarts/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir gizli dizi eklemek için birkaç ek adım uygulamanız gerekir. Bu durumda, Key Vault alımı test etmek için kullandığımız bir ileti ekleyeceğiz. İleti **ileti** olarak adlandırılır ve Içindeki **Key Vault Hello** değerini depolarız.

1. Key Vault Özellikler sayfalarında **gizli**dizileri ' ni seçin.
1. **Oluştur/İçeri Aktar**’a tıklayın.
1. **Bir gizli dizi oluştur** ekranında aşağıdaki değerleri seçin:
    - **Karşıya yükleme seçenekleri**: El ile.
    - **Ad**: ileti
    - **Değer**: Key Vault 'den Merhaba
    - Diğer değerleri varsayılan değerlerinde bırakın. **Oluştur**’a tıklayın.

## <a name="add-a-key-vault-reference-to-app-config"></a>App config 'e Key Vault başvurusu ekleme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın. **Tüm kaynaklar**' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

1. **Yapılandırma Gezgini** ' ne tıklayın

1. **+** @No__t-1**Anahtar Kasası başvurusu** ' na tıklayın ve aşağıdaki değerleri seçin:
    - **Anahtar**: TestApp: ayarlar: KeyVaultMessage
    - **Etiket**: boş bırakın
    - **Abonelik**, **kaynak grubu**, **anahtar kasası**: önceki bölümde oluşturduğunuz Key Vault karşılık gelen seçenekleri seçin.
    - **Gizli**dizi: önceki bölümde oluşturduğunuz gizli dizi **iletisini** seçin.

## <a name="connect-to-key-vault"></a>Key Vault Bağlan

1. Bu öğretici için, Anahtar Kasası kimlik doğrulaması için bir hizmet sorumlusu kullanacaksınız. Bu hizmet sorumlusunu oluşturmak için Azure CLı [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanın:

    ```azurecli
    az ad sp create-for-rbac -n "http://mySP" --sdk-auth
    ```

    Bu işlem, bir dizi anahtar/değer çifti döndürür. 

    ```console
    {
    "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
    "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
    "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
    "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

1. Hizmet sorumlusunun anahtar kasanıza erişmesine izin vermek için aşağıdaki komutu çalıştırın:

        az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey

1. Gizli anahtar ve *ClientSecret* için gizli diziler yöneticisine gizli diziler ekleyin. Bu komutların *. csproj* dosyasıyla aynı dizinde yürütülmesi gerekir.

        dotnet user-secrets set ConnectionStrings:KeyVaultClientId <clientId-of-your-service-principal>        
        dotnet user-secrets set ConnectionStrings:KeyVaultClientSecret <clientSecret-of-your-service-principal>

> [!NOTE]
> Bu Key Vault kimlik bilgileri yalnızca uygulamanız içinde kullanılır. Uygulamanız bu kimlik bilgileriyle Key Vault doğrudan kimlik doğrulaması yapar. Bunlar hiçbir şekilde uygulama yapılandırma hizmetine geçirilmez.

## <a name="update-your-code-to-use-a-key-vault-reference"></a>Kodunuzu Key Vault bir başvuru kullanacak şekilde güncelleştirin

1. *Program.cs*'i açın ve gerekli paketlere başvurular ekleyin.

    ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

1. @No__t-0 yöntemini, `config.AddAzureAppConfiguration()` yöntemini çağırarak uygulama yapılandırmasını kullanacak şekilde güncelleştirin. Key Vault yeni bir @no__t 1 başvurusunu geçirerek `UseAzureKeyVault` seçeneğini ekleyin.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                KeyVaultClient kvClient = new KeyVaultClient(async (authority, resource, scope) =>
                {
                    var adCredential = new ClientCredential(settings["ConnectionStrings:KeyVaultClientId"], settings["ConnectionStrings:KeyVaultClientSecret"]);
                    var authenticationContext = new AuthenticationContext(authority, null);
                    return (await authenticationContext.AcquireTokenAsync(resource, adCredential)).AccessToken;
                });

                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                            .UseAzureKeyVault(kvClient); });
            })
            .UseStartup<Startup>();
    ```

1. Uygulama yapılandırmasına bağlantıyı başlatırken *Keyvaultclient* başvurusunu `UseAzureKeyVault` yöntemine geçirdikten sonra, Key Vault başvuruların değerlerine, normal uygulama yapılandırma anahtarlarının değerlerine erişirken aynı şekilde erişebilirsiniz. Bu işlemi eylemde görmek için, Görünümler > giriş dizininde *Index. cshtml* dosyasını açın. İçeriğini şu kodla değiştirin:

    ```html
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]
        and @Configuration["TestApp:Settings:KeyVaultMessage"]</h1>
    ```

    *TestApp: Settings: KeyVaultMessage* Key Vault Reference değerine, *TestApp: Settings: Message* yapılandırma değeri ile aynı şekilde erişirsiniz.

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğu 'nda aşağıdaki komutu çalıştırın:

        dotnet build

2. Oluşturma başarıyla tamamlandıktan sonra, Web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

3. Bir tarayıcı penceresi açın ve yerel olarak barındırılan Web uygulamasının varsayılan URL 'SI olan `http://localhost:5000` ' a gidin.

    ![Hızlı başlangıç uygulaması başlatma yerel](./media/key-vault-reference-launch-local.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırmasına erişimi kolaylaştırmak ve uygulamanız için kimlik bilgisi yönetimini geliştirmek üzere bir Azure yönetilen hizmet kimliği eklediniz. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için Azure CLı örneklerine devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
