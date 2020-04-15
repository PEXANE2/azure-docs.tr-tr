---
title: Azure App Configuration Key Vault başvurularını ASP.NET Core uygulamasında kullanma nın öğreticisi | Microsoft Dokümanlar
description: Bu eğitimde, azure app configuration'ın Key Vault referanslarını ASP.NET Core uygulamasından nasıl kullanacağınızı öğreniyorsunuz
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
ms.date: 04/08/2020
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 4641c50f0579e2a8db514df58c0401eb2173d793
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309043"
---
# <a name="tutorial-use-key-vault-references-in-an-aspnet-core-app"></a>Öğretici: Key Vault referanslarını ASP.NET Core uygulamasında kullanma

Bu eğitimde, Azure Anahtar Kasası ile birlikte Azure Uygulama Yapılandırma hizmetini nasıl kullanacağınızı öğreneceksiniz. Uygulama Yapılandırması ve Anahtar Kasası, çoğu uygulama dağıtımında yan yana kullanılan tamamlayıcı hizmetlerdir.

Uygulama Yapılandırması, Key Vault'ta depolanan değerlere başvuru yapan anahtarlar oluşturarak hizmetleri birlikte kullanmanıza yardımcı olur. App Configuration bu tür anahtarları oluşturduğunda, değerlerin kendileri yerine Key Vault değerlerinin URI'lerini depolar.

Uygulamanız, Uygulama Yapılandırması'nda depolanan diğer anahtarlarda olduğu gibi Key Vault başvurularını almak için App Configuration istemci sağlayıcısını kullanır. Bu durumda, Uygulama Yapılandırmasında depolanan değerler, Anahtar Kasası'ndaki değerlere başvuran ÜR'lerdir. Bunlar Key Vault değerleri veya kimlik bilgileri değildir. İstemci sağlayıcı anahtarları Key Vault başvuruları olarak tanıdığından, değerlerini almak için Key Vault'u kullanır.

Uygulamanız, hem Uygulama Yapılandırması hem de Key Vault'a doğru şekilde doğru doğrulamadan sorumludur. İki servis doğrudan iletişim kurmaz.

Bu öğretici, anahtarınıza Anahtar Kasa başvurularının nasıl uygulanacağını gösterir. Bu quickstarts tanıtılan web uygulaması üzerine oluşturur. Devam etmeden önce, [önce App Configuration ile ASP.NET Core uygulaması oluşturun'u](./quickstart-aspnet-core-app.md) tamamla.

Bu öğreticideki adımları yapmak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Örneğin, [Visual Studio Code,](https://code.visualstudio.com/) Windows, macOS ve Linux işletim sistemleri için kullanılabilen bir çapraz platform kod düzenleyicisidir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Key Vault'ta depolanan bir değere başvuran bir Uygulama Yapılandırması anahtarı oluşturun.
> * Bu anahtarın değerine ASP.NET Core web uygulamasından erişin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi başlatmadan önce [.NET Core SDK'yı](https://dotnet.microsoft.com/download)yükleyin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalının sol üst köşesinde **kaynak oluştur** seçeneğini seçin:

    ![Anahtar kasası oluşturma tamamlandıktan sonra çıktı](./media/quickstarts/search-services.png)
1. Arama kutusuna, **Anahtar Kasası**girin.
1. Sonuç listesinden soldaki **Anahtar kasalarını** seçin.
1. **Anahtar kasalarında** **Ekle'yi**seçin.
1. Sağdaki **Create anahtar kasasında,** aşağıdaki bilgileri sağlayın:
    - Abonelik seçmek için **Abonelik'i** seçin.
    - **Kaynak Grubu'nda** **yeni oluştur'u** seçin ve bir kaynak grubu adı girin.
    - **Anahtar kasası adında,** benzersiz bir ad gereklidir. Bu öğretici için **Contoso-vault2'yi**girin.
    - **Bölge** açılır listesinde bir konum seçin.
1. Diğer **Create anahtar kasa** seçeneklerini varsayılan değerleriyle bırakın.
1. **Oluştur'u**seçin.

Bu noktada, azure hesabınız bu yeni kasaya erişmeye yetkili tek hesaptır.

![Anahtar kasası oluşturma tamamlandıktan sonra çıktı](./media/quickstarts/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir sır eklemek için birkaç ek adım atmalısınız. Bu durumda, Anahtar Kasa sıyrıksını test etmek için kullanabileceğiniz bir ileti ekleyin. İletinin adı **İleti**ve içinde "Key Vault'tan Merhaba" değerini saklarsınız.

1. Key Vault özellikleri sayfalarından **Sırlar'ı**seçin.
1. **Oluştur/İçe Aktar'ı**seçin.
1. Gizli **bölmeoluştur'da** aşağıdaki değerleri girin:
    - **Yükleme seçenekleri**: **Kılavuzu**Girin .
    - **Adı**: **İleti**girin .
    - **Değer**: **Key Vault'tan Merhaba**girin.
1. Diğer invarsayılan değerleri ile **gizli özellikler oluştur** bırakın.
1. **Oluştur'u**seçin.

## <a name="add-a-key-vault-reference-to-app-configuration"></a>Uygulama Yapılandırmasına Anahtar Kasa sı başvurusu ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynakları**seçin ve ardından hızlı başlatmada oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

1. **Configuration Explorer'ı**seçin.

1. +**Anahtar kasası başvurusu** **oluştur'u** > seçin ve ardından aşağıdaki değerleri belirtin:
    - **Anahtar**: **TestApp'i seçin:Ayarlar:KeyVaultMessage**.
    - **Etiket**: Bu değeri boş bırakın.
    - **Abonelik**, **Kaynak grubu**ve **Anahtar kasası**: Önceki bölümde oluşturduğunuz anahtar kasasına karşılık gelen değerleri girin.
    - **Gizli**: Önceki bölümde oluşturduğunuz **İleti** adlı sırrı seçin.

## <a name="connect-to-key-vault"></a>Anahtar Kasasına Bağlan

1. Bu öğreticide, Key Vault kimlik doğrulaması için bir hizmet ilkesi kullanın. Bu hizmet ilkesini oluşturmak için Azure CLI [az reklam sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanın:

    ```azurecli
    az ad sp create-for-rbac -n "http://mySP" --sdk-auth
    ```

    Bu işlem bir dizi anahtar/değer çifti döndürür:

    ```console
    {
    "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
    "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
    "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
    "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

1. Servis müdürünün anahtar kasanıza erişebilsin diye aşağıdaki komutu çalıştırın:

    ```cmd
    az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
    ```

1. *ClientId,* *clientSecret*ve *tenantId*değerlerini depolamak için ortam değişkenleri ekleyin.

    #### <a name="windows-command-prompt"></a>[Windows komut istemi](#tab/cmd)

    ```cmd
    setx AZURE_CLIENT_ID <clientId-of-your-service-principal>
    setx AZURE_CLIENT_SECRET <clientSecret-of-your-service-principal>
    setx AZURE_TENANT_ID <tenantId-of-your-service-principal>
    ```

    #### <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```PowerShell
    $Env:AZURE_CLIENT_ID = <clientId-of-your-service-principal>
    $Env:AZURE_CLIENT_SECRET = <clientSecret-of-your-service-principal>
    $Env:AZURE_TENANT_ID = <tenantId-of-your-service-principal>
    ```

    #### <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    export AZURE_CLIENT_ID = <clientId-of-your-service-principal>
    export AZURE_CLIENT_SECRET = <clientSecret-of-your-service-principal>
    export AZURE_TENANT_ID = <tenantId-of-your-service-principal>
    ```

    ---

    > [!NOTE]
    > Bu Anahtar Kasa kimlik bilgileri yalnızca uygulamanız içinde kullanılır. Uygulamanız bu kimlik bilgileriyle doğrudan Key Vault'a doğrular. Hiçbir zaman Uygulama Yapılandırması hizmetine geçirilir.

1. Bu yeni ortam değişkenlerini yüklemek için terminalinizi yeniden başlatın.

## <a name="update-your-code-to-use-a-key-vault-reference"></a>Key Vault referansını kullanmak için kodunuzu güncelleştirin

1. Aşağıdaki komutu çalıştırarak gerekli NuGet paketlerine bir başvuru ekleyin:

    ```dotnetcli
    dotnet add package Azure.Identity
    ```

1. *Program.cs*açın ve aşağıdaki gerekli paketlere başvurular ekleyin:

    ```csharp
    using Azure.Identity;
    ```

1. Yöntemi `CreateWebHostBuilder` arayarak Uygulama Yapılandırması'nı kullanmak için yöntemi güncelleştirin. `config.AddAzureAppConfiguration` `ConfigureKeyVault` Seçeneği ekleyin ve doğru kimlik bilgilerini Anahtar Kasanıza iletin.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                config.AddAzureAppConfiguration(options =>
                {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                            .ConfigureKeyVault(kv =>
                            {
                                kv.SetCredential(new DefaultAzureCredential());
                            });
                });
            })
            .UseStartup<Startup>();
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                config.AddAzureAppConfiguration(options =>
                {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                            .ConfigureKeyVault(kv =>
                            {
                                kv.SetCredential(new DefaultAzureCredential());
                            });
                });
            })
            .UseStartup<Startup>());
    ```

1. App Configuration bağlantısını açtığınızda, `ConfigureKeyVault` yöntemi arayarak Key Vault bağlantısını ayarlarsınız. Başlatmadan sonra, Anahtar Kasa başvurularının değerlerine normal Uygulama Yapılandırma anahtarlarının değerlerine eriştiği gibi erişebilirsiniz.

    Bu işlemi iş başında görmek için **Görünümler** > **Ana** Klasöründe *Index.cshtml'i* açın. İçeriğini aşağıdaki kodla değiştirin:

    ```html
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"]px;
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]
        and @Configuration["TestApp:Settings:KeyVaultMessage"]</h1>
    ```

    Key Vault referans **TestApp:Ayarlar:KeyVaultMessage** değerine **TestApp:Settings:Message'ın**yapılandırma değeriyle aynı şekilde erişebilirsiniz.

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. .NET Core CLI kullanarak uygulamayı oluşturmak için komut kabuğunda aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

1. Yapı tamamlandıktan sonra, web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu kullanın:

    ```dotnetcli
    dotnet run
    ```

1. Bir tarayıcı penceresi açın `http://localhost:5000`ve yerel olarak barındırılan web uygulamasının varsayılan URL'si olan adrese gidin.

    ![Quickstart yerel uygulama lansmanı](./media/key-vault-reference-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Key Vault'ta depolanan bir değere başvuran bir Uygulama Yapılandırması anahtarı oluşturdunuz. Uygulama Yapılandırması ve Key Vault'a erişimi kolaylaştıran Azure yönetilen bir hizmet kimliğinin nasıl ekleyeceğinizi öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
