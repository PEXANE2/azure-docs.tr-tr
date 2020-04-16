---
title: Visual Studio - Azure Key ASP.NET Vault | Microsoft Dokümanlar
description: Bir ASP.NET veya ASP.NET Core web uygulamasına Key Vault desteği eklemeyi öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: jillfra
ms.service: key-vault
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: ghogen
ms.openlocfilehash: 68f8607db25b70ea5e6bfe21a7d6e002aeb127d6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429778"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Visual Studio Connected Services'ı kullanarak web uygulamanıza Key Vault ekleyin

Bu eğitimde, Visual Studio'daki web projeleri için sırlarınızı yönetmek için Azure Key Vault'u kullanmaya başlamak için ihtiyacınız olan her şeyi nasıl kolayca ekleyeceğinizi öğreneceksiniz, ister ASP.NET Core'u veya herhangi bir ASP.NET projeyi kullanıyor olun. Visual Studio'da Bağlı Hizmetler özelliğini kullanarak Visual Studio'nun Azure'da Key Vault'a bağlanmak için ihtiyaç duyduğunuz tüm NuGet paketlerini ve yapılandırma ayarlarını otomatik olarak eklemesini sağlayabilirsiniz.

Bağlantılı Hizmetler Key Vault etkinleştirmek için projenizde yaptığı değişiklikler hakkında ayrıntılı bilgi için, Key Vault Bağlı Hizmet bakın [- benim ASP.NET 4.7.1 proje](#how-your-aspnet-framework-project-is-modified) veya Key Vault Bağlı Hizmet ne oldu - benim ASP.NET Core proje ne [oldu](#how-your-aspnet-core-project-is-modified).

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**. Aboneliğiniz yoksa, [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/)için kaydolun.
- **Visual Studio 2019 sürüm 16.3** veya sonraki veya **Visual Studio 2017 sürümü 15.7** **Web Geliştirme** iş yükü yüklü. [Şimdi indir](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Visual Studio 2017 ile ASP.NET (Core değil) için varsayılan olarak yüklenmeyen .NET Framework 4.7.1 veya daha sonra Geliştirme Araçları'na ihtiyacınız vardır. Bunları yüklemek için, Visual Studio Installer başlatmak, **Değiştir'i**seçin ve sonra **Bireysel Bileşenleri**seçin , sonra sağ tarafta, **ASP.NET ve web geliştirme**genişletmek , ve **.NET Framework 4.7.1 Geliştirme Araçları**seçin.
- Bir ASP.NET 4.7.1 veya daha sonra veya Core 2.0 veya daha sonra ASP.NET web projesi açılır.

## <a name="add-key-vault-support-to-your-project"></a>Projenize Key Vault desteği ekleyin

Başlamadan önce Visual Studio'da oturum açmış olduğunuzdan emin olun. Azure aboneliğiniz için kullandığınız hesapla oturum açın. Ardından bir ASP.NET 4.7.1 veya daha sonra açın veya Core 2.0 web projesi ASP.NET ve aşağıdaki adımları yapın:

1. **Çözüm Gezgini'nde,** Anahtar Kasası desteğini eklemek istediğiniz projeyi sağ tıklatın ve**Bağlı Hizmet** **Ekle'yi** > seçin.
   Projenize ekleyebileceğiniz hizmetlerle birlikte Bağlı Hizmet sayfası görüntülenir.
1. Kullanılabilir hizmetler menüsünde, **Azure Anahtar Kasası ile Güvenli Sırlar'ı**seçin.

   !["Azure Key Vault ile Güvenli Sırlar" seçeneğini belirleyin](../media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

1. Kullanmak istediğiniz aboneliği seçin ve ardından yeni veya varolan bir Key Vault seçin. Yeni Anahtar Kasası'nı seçerseniz, bir **Edit** bağlantısı görüntülenir. Yeni Key Vault'unuzu yapılandırmak için seçin.

   ![Aboneliğinizi seçme](../media/vs-key-vault-add-connected-service/key-vault-connected-service-select-vault.png)

1. **Azure Anahtar Kasasını Edit'te,** Anahtar Kasası için kullanmak istediğiniz adı girin.

1. Varolan bir **Kaynak Grubu**seçin veya otomatik olarak oluşturulan benzersiz bir ada sahip yeni bir grup oluşturmayı seçin.  Farklı bir ada sahip yeni bir grup oluşturmak istiyorsanız, [Azure portalını](https://portal.azure.com)kullanabilir ve ardından sayfayı kapatıp kaynak grupları listesini yeniden yüklemek için yeniden başlatabilirsiniz.
1. Anahtar Kasası'nı oluşturmak için **Konumu** seçin. Web uygulamanız Azure'da barındırılıyorsa, optimum performans için web uygulamasını barındıran bölgeyi seçin.
1. Bir **Fiyatlandırma katmanı**seçin. Ayrıntılar [için, Key Vault Fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/)bakın.
1. Yapılandırma seçeneklerini kabul etmek için **Tamam'ı** seçin.
1. Visual Studio'nun **Azure Key Vault** sekmesinde varolan bir Key Vault'u seçtikten veya yeni bir Key Vault yapılandırdıktan sonra, Bağlı Hizmeti eklemek için **Ekle'yi** seçin.
1. Key Vault'unuzun **Sırlar** sayfasını açmak için bu Anahtar Kasa sı **bağlantısında saklanan sırları yönet'i** seçin. Sayfayı veya projeyi kapattıysanız, **Tüm Hizmetleri** seçerek [Azure portalında](https://portal.azure.com) sayfaya gidebilir ve **Güvenlik**altında Key Vault'u seçerek Anahtar **Kasanızı**seçebilirsiniz.
1. Oluşturduğunuz Anahtar Kasası için Anahtar Kasası **bölümünde, Sırlar'ı**seçin, ardından **Oluştur/Al.**

   ![Bir sır oluşturma/alma](../media/vs-key-vault-add-connected-service/azure-generate-secrets.png)

1. *MySecret* gibi bir gizli girin ve test olarak herhangi bir dize değeri verin, ardından **Oluştur** düğmesini seçin.

   ![Gizli anahtar oluşturma](../media/vs-key-vault-add-connected-service/azure-create-a-secret.png)

1. (isteğe bağlı) Başka bir sır girin, ama bu sefer *sırlar adlandırarak*bir kategoriiçine koymak - MySecret . Bu sözdizimi, gizli bir "MySecret" içeren bir kategori "Secrets" belirtir.

Şimdi, sırlarına şifreli olarak erişebilirsin. Sonraki adımlar, 4.7.1 veya ASP.NET Core ASP.NET kullanıp kullanmadığınıza bağlı olarak farklıdır.

## <a name="access-your-secrets-in-code-aspnet-core"></a>Sırlarınıza kodla erişin (ASP.NET Core)

1. Çözüm Gezgini'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. **Gözat** sekmesinde, bu iki NuGet paketini bulun ve yükleyin: [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve .NET Core 2 için [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) veya .NET Core 3 için[Microsoft.Azure.KeyVault.Core'u](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core)ekleyin.

1. .NET Core 2 için `Program.cs` sekmeyi `BuildWebHost` seçin ve Program sınıfındaki tanımı aşağıdaki yle değiştirin:

   ```csharp
        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>();

        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    }
   ```

   .NET Core 3 için aşağıdaki kodu kullanın.

   ```csharp
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((context, config) =>
                {
                    var keyVaultEndpoint = GetKeyVaultEndpoint();
                    if (!string.IsNullOrEmpty(keyVaultEndpoint))
                    {
                        var azureServiceTokenProvider = new AzureServiceTokenProvider();
                        var keyVaultClient = new KeyVaultClient(
                            new KeyVaultClient.AuthenticationCallback(
                                azureServiceTokenProvider.KeyVaultTokenCallback));
                        config.AddAzureKeyVault(keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                    }
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    ```

1. Sonraki *Index.cshtml.cs* gibi sayfa dosyalarından birini açın ve aşağıdaki kodu yazın:
   1. Bu `Microsoft.Extensions.Configuration` yönergeyi kullanarak bir referans ekleyin:

       ```csharp
       using Microsoft.Extensions.Configuration;
       ```

   1. Yapılandırma değişkenini ekleyin.

      ```csharp
      private static readonly IConfiguration _configuration;
      ```

   1. Bu oluşturucuyu ekleyin veya varolan oluşturucuyu şuyla değiştirin:

       ```csharp
       public IndexModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```

   1. `OnGet` yöntemini güncelleştirin. Yukarıda komutlarda oluşturduğunuz gizli adla burada gösterilen yer tutucu değerini güncelleştirin.

       ```csharp
       public void OnGet()
       {
           ViewData["Message"] = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

   1. Çalışma zamanındadeğeri onaylamak için, bir `ViewData["Message"]` iletide gizli görüntülemek için *.cshtml* dosyasına görüntülenecek kod ekleyin.

      ```cshtml
          <p>@ViewData["Message"]</p>
      ```

Gizlinin Key Vault'tan başarıyla elde edileli olduğunu doğrulamak için uygulamayı yerel olarak çalıştırabilirsiniz.

## <a name="access-your-secrets-aspnet"></a>Sırlarınıza erişin (ASP.NET)

Yapılandırmayı, web.config dosyasının çalışma zamanında gerçek değerle `appSettings` değiştirilen öğede sahte bir değere sahip olacak şekilde ayarlayabilirsiniz. Daha sonra `ConfigurationManager.AppSettings` buna veri yapısı üzerinden erişebilirsiniz.

1. Web.config dosyanızı edin.  appAyarlar etiketini bulun, bir `configBuilders="AzureKeyVault"`öznitelik ekleyin ve bir satır ekleyin:

   ```xml
      <add key="mysecret" value="dummy"/>
   ```

1. Doğrulama değerini `About` görüntülemek için *yöntemi HomeController.cs*olarak edin.

   ```csharp
   public ActionResult About()
   {
       ViewBag.Message = "Key vault value = " + ConfigurationManager.AppSettings["mysecret"];
   }
   ```
1. Uygulamayı hata ayıklayıcının altında yerel olarak çalıştırın, **Hakkında** sekmesine geçin ve Anahtar Kasası'ndaki değerin görüntülendiğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubunu silin. Bu, Anahtar Kasası'nı ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **Tİp'TE KAYNAK GRUBU ADI:** kutu, kaynak grubunun adına girin ve **Sil'i**seçin.

## <a name="troubleshooting"></a>Sorun giderme

Key Vault'unuz Visual Studio'da oturum açtığınız hesaptan farklı bir Microsoft hesabında çalışıyorsa (örneğin, Key Vault iş hesabınızda çalışıyor, ancak Visual Studio özel hesabınızı kullanıyorsa) Program.cs dosyanızda bir hata alırsınız, Visual Studio'nun Key Vault'a erişememesi. Bu sorunu düzeltmek için:

1. [Azure portalına](https://portal.azure.com) gidin ve Key Vault'unuzu açın.

1. **Erişim ilkeleri'ni**seçin, ardından **Erişim İlkesi Ekle**ve Ana para olarak oturum açtığınız hesabı seçin.

1. Visual Studio'da **Dosya** > **Hesabı Ayarları'nı**seçin.
**Tüm hesap** bölümünden **hesap ekle'yi** seçin. Erişim politikanızın Müdürü olarak seçtiğiniz hesapla oturum açın.

1. **Araç** > **Seçenekleri'ni**seçin ve Azure Hizmet Kimlik **Doğrulaması'nı**arayın. Ardından Visual Studio'ya eklediğiniz hesabı seçin.

Şimdi, başvurunuzu hata ayıklama ettiğinizde, Visual Studio Key Vault'unuzun bulunduğu hesaba bağlanır.

## <a name="how-your-aspnet-core-project-is-modified"></a>ASP.NET Core projeniz nasıl değiştirilir?

Bu bölümde, Visual Studio kullanarak Key Vault bağlantılı hizmeti eklerken ASP.NET bir projede yapılan tam değişiklikleri tanımlar.

### <a name="added-references-for-aspnet-core"></a>ASP.NET Core için ekleme

Proje dosyasını etkiler .NET başvuruları ve NuGet paket başvuruları.

| Tür | Başvuru |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingBaşlangıç |

### <a name="added-files-for-aspnet-core"></a>ASP.NET Core için dosya eklendi

- `ConnectedService.json`bağlı servis sağlayıcısı, sürümü ve belgelerle ilgili bir bağlantı hakkında bazı bilgileri kaydeden eklendi.

### <a name="project-file-changes-for-aspnet-core"></a>ASP.NET Core için proje dosyası değişiklikleri

- Bağlı Hizmetler Öğe Grubu `ConnectedServices.json` ve dosyası eklendi.

### <a name="launchsettingsjson-changes-for-aspnet-core"></a>ASP.NET Core için launchsettings.json değişiklikleri

- Hem IIS Express profiline hem de web proje adınıza uyan profile aşağıdaki ortam değişkeni girişleri eklendi:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

### <a name="changes-on-azure-for-aspnet-core"></a>ASP.NET Core için Azure'daki değişiklikler

- Bir kaynak grubu oluşturuldu (veya varolan bir grup kullanılır).
- Belirtilen kaynak grubunda bir Anahtar Kasası oluşturuldu.

## <a name="how-your-aspnet-framework-project-is-modified"></a>ASP.NET Framework projeniz nasıl değiştirilir?

Bu bölümde, Visual Studio kullanarak Key Vault bağlantılı hizmeti eklerken ASP.NET bir projede yapılan tam değişiklikleri tanımlar.

### <a name="added-references-for-aspnet-framework"></a>ASP.NET Framework için ekleme referansları

Proje dosyasını etkiler .NET başvuruları ve `packages.config` (NuGet başvuruları).

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

### <a name="added-files-for-aspnet-framework"></a>ASP.NET Framework için dosya eklendi

- `ConnectedService.json`bağlı servis sağlayıcısı, sürümü ve belgelere bağlantı hakkında bazı bilgileri kaydeden eklendi.

### <a name="project-file-changes-for-aspnet-framework"></a>ASP.NET Framework için proje dosyası değişiklikleri

- Bağlı Hizmetler ItemGroup ve ConnectedServices.json dosyası eklendi.
- Eklenen başvurular bölümünde açıklanan .NET derlemelerine [başvurular.](#added-references-for-aspnet-framework)

### <a name="webconfig-or-appconfig-changes"></a>web.config veya app.config değişiklikleri

- Aşağıdaki yapılandırma girişleri eklendi:

    ```xml
    <configSections>
      <section
           name="configBuilders"
           type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
           restartOnExternalChanges="false"
           requirePermission="false" />
    </configSections>
    <configBuilders>
      <builders>
        <add
             name="AzureKeyVault"
             vaultName="vaultname"
             type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=1.0.0.0, Culture=neutral"
             vaultUri="https://vaultname.vault.azure.net" />
      </builders>
    </configBuilders>
    ```

### <a name="changes-on-azure-for-aspnet-framework"></a>ASP.NET Framework için Azure'daki değişiklikler

- Bir kaynak grubu oluşturuldu (veya varolan bir grup kullanılır).
- Belirtilen kaynak grubunda bir Anahtar Kasası oluşturuldu.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticiyi izlediyseniz, Key Vault izinleriniz kendi Azure aboneliğinizle çalışacak şekilde ayarlanmıştır, ancak bu bir üretim senaryosu için isden olmayabilir. Uygulamanız için Key Vault erişimini yönetmek için yönetilen bir kimlik oluşturabilirsiniz. Bkz. [Yönetilen bir kimlikle Anahtar Kasakimlik doğrulaması sağlayın.](/azure/key-vault/managed-identity)

[Key Vault Geliştirici Kılavuzu'nu](developers-guide.md)okuyarak Key Vault geliştirme hakkında daha fazla bilgi edinin.
