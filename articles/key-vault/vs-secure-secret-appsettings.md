---
title: Bir web uygulaması için gizli uygulama ayarlarını güvenli bir şekilde kaydetme - Azure Key Vault | Microsoft Dokümanlar
description: Temel Key Vault Sağlayıcısı, Kullanıcı Sırrı veya .NET 4.7.1 yapılandırma oluşturucularını kullanarak Azure kimlik bilgileri veya üçüncü taraf API anahtarları gibi gizli uygulama ayarlarını güvenli ASP.NET bir şekilde kaydetme
services: visualstudio
author: cawaMS
manager: paulyuk
editor: ''
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: cawa
ms.openlocfilehash: 6bbadd0deb1b7b6fe1056ed7bb47dc3e666a7b9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197717"
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Bir web uygulaması için gizli uygulama ayarlarını güvenli bir şekilde kaydedin

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure uygulamaları için gizli uygulama yapılandırma ayarlarını güvenli bir şekilde nasıl kaydedebilirsiniz.

Geleneksel olarak tüm web uygulaması yapılandırma ayarları Web.config gibi yapılandırma dosyalarına kaydedilir. Bu uygulama, GitHub gibi ortak kaynak denetim sistemlerine Bulut kimlik bilgileri gibi gizli ayarların denetlemesine yol açar. Bu arada, kaynak kodunu değiştirmek ve geliştirme ayarlarını yeniden yapılandırmak için gereken ek yükü nedeniyle güvenlik en iyi uygulama izlemek zor olabilir.

Geliştirme işleminin güvenli olduğundan emin olmak için, uygulama gizli ayarlarını en az veya kaynak kodu değişikliği olmadan güvenli bir şekilde kaydetmek için araç ve çerçeve kitaplıkları oluşturulur.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET ve .NET Çekirdek uygulamaları

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Kaynak denetim klasörünün dışındaki Kullanıcı Gizli deposunda gizli ayarları kaydetme
Hızlı bir prototip yapıyorsanız veya internet erişiminiz yoksa, gizli ayarlarınızı kaynak denetim klasörünün dışına, Kullanıcı Sırrı mağazasına taşıyarak başlayın. Kullanıcı Gizli deposu kullanıcı profiler klasörü altında kaydedilmiş bir dosya, bu nedenle sırları kaynak denetimi için iade edilmez. Aşağıdaki [diyagram, Kullanıcı Sırrı'nın](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio) nasıl çalıştığını gösterir.

![Kullanıcı Sırrı gizli ayarları kaynak denetimi dışında tutar](./media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

.NET çekirdek konsol uyrağı uygulamasını çalıştırıyorsanız, sırrınızı güvenli bir şekilde kaydetmek için Key Vault'u kullanın.

### <a name="save-secret-settings-in-azure-key-vault"></a>Azure Anahtar Kasası'nda gizli ayarları kaydetme
Bir proje geliştiriyorsanız ve kaynak kodunu güvenli bir şekilde paylaşmanız gerekiyorsa, [Azure Anahtar Kasası'nı](https://azure.microsoft.com/services/key-vault/)kullanın.

1. Azure aboneliğinizde bir Anahtar Kasası oluşturun. UI'de gerekli tüm alanları doldurun ve bıçağın altında *Oluştur'u* tıklatın

    ![Azure Anahtar Kasası Oluşturma](./media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. Size ve ekip üyelerinize Key Vault'a erişim izni verir. Büyük bir ekibiniz varsa, bir [Azure Etkin Dizin grubu](../active-directory/active-directory-groups-create-azure-portal.md) oluşturabilir ve bu güvenlik grubunun Anahtar Kasası'na erişimini ekleyebilirsiniz. Gizli *İzinler* açılır durumda, *Gizli Yönetim İşlemleri*altında *Al* ve *Liste'yi* kontrol edin.
Web uygulamanızı zaten oluşturduysanız, Web uygulamasına Key Vault'a erişim izni vererek, Uygulama Ayarları'nda veya dosyalarda gizli yapılandırma depolamadan anahtar kasasına erişebilsin. Web uygulamanızı adına göre arayın ve kullanıcılara erişim izni verdiğiniz şekilde ekleyin.

    ![Key Vault erişim ilkesi ekle](./media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Azure portalındaki Key Vault'a sırrınızı ekleyin. İç içe yapılandırma ayarları için Anahtar Kasa gizli adının geçerli olması için ':' ile '--' değiştirin. ':' bir Key Vault sırrı adına izin verilmez.

    ![Key Vault gizli ekle](./media/vs-secure-secret-appsettings/add-keyvault-secret.png)

    > [!NOTE]
    > Visual Studio 2017 V15.6'dan önce Visual Studio için Azure Hizmetleri Kimlik Doğrulama uzantısını yüklemenizi öneririz. Ama işlevsellik Visual Studio entegre olarak şimdi deprecated olduğunu. Bu nedenle Visual Studio 2017'nin eski bir sürümündeyseniz, bu işlevselliği yerel olarak kullanabilmeniz ve Visual Studio oturum açma Kimliğini kullanarak Anahtar kasasına erişebilmeniz için en az VS 2017 15.6 veya üzeri güncellemenizi öneririz.
    >

4. Projenize aşağıdaki NuGet paketlerini ekleyin:

    ```
    Microsoft.Azure.KeyVault
    Microsoft.Azure.Services.AppAuthentication
    Microsoft.Extensions.Configuration.AzureKeyVault
    ```
5. Program.cs dosyaya aşağıdaki kodu ekleyin:

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
             Host.CreateDefaultBuilder(args)
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
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });

        private static string GetKeyVaultEndpoint() => Environment.GetEnvironmentVariable("KEYVAULT_ENDPOINT");
    ```
6. Launchsettings.json dosyasına Key Vault URL'nizi ekleyin. KEYVAULT_ENDPOINT ortam *değişkeni* adı, 6.

    ![Proje ortamı değişkeni olarak Anahtar Kasa URL'si ekleme](./media/vs-secure-secret-appsettings/add-keyvault-url.png)

7. Projehatasını çıkarmaya başlayın. Başarılı bir şekilde çalışmalı.

## <a name="aspnet-and-net-applications"></a>ASP.NET ve .NET uygulamaları

.NET 4.7.1, sırların kod değişikliği olmadan kaynak denetim klasörünün dışına taşınabilmesini sağlayan Key Vault ve Secret yapılandırma oluşturucularını destekler.
Devam etmek için [.NET 4.7.1'i indirin](https://www.microsoft.com/download/details.aspx?id=56115) ve .NET çerçevesinin eski bir sürümünü kullanıyorsa uygulamanızı geçirin.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Gizli ayarları kaynak denetim klasörünün dışında bulunan gizli bir dosyaya kaydetme
Hızlı bir prototip yazıyorsanız ve Azure kaynaklarını sağlamak istemiyorsanız, bu seçeneği belirleyin.

1. Aşağıdaki NuGet paketini projenize yükleyin
    ```
    Microsoft.Configuration.ConfigurationBuilders.Base
    ```

2. Aşağıdakilere benzer bir dosya oluşturun. Proje klasörünüzün dışındaki bir konumun altına kaydedin.

    ```xml
    <root>
        <secrets ver="1.0">
            <secret name="secret1" value="foo_one" />
            <secret name="secret2" value="foo_two" />
        </secrets>
    </root>
    ```

3. Gizli dosyayı Web.config dosyanızda yapılandırma oluşturucusu olarak tanımlayın. Bu bölümü *appSettings* bölümünün önüne koyun.

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. Özel Ayarlar bölümünün gizli yapılandırma oluşturucuyu kullandığını belirtin. Sahte değeri olan gizli ayar için bir giriş olduğundan emin olun.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. Uygulamanızı hata ayıklama. Başarılı bir şekilde çalışmalı.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Azure Anahtar Kasası'nda gizli ayarları kaydetme
Projeniz için bir Anahtar Kasası yapılandırmak için temel ASP.NET bölümdeki yönergeleri izleyin.

1. Aşağıdaki NuGet paketini projenize yükleyin
   ```
   Microsoft.Configuration.ConfigurationBuilders.UserSecrets
   ```

2. Web.config'de Key Vault yapılandırma oluşturucusu tanımlayın. Bu bölümü *appSettings* bölümünün önüne koyun. VaultName'i, Key Vault'unuz herkese açık Azure'daysa key vault adı olarak değiştirin veya Sovereign bulutu kullanıyorsanız tam URI' yi değiştirin. *vaultName*

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="AzureKeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3. Key Vault yapılandırma oluşturucusu kullanıyor appSettings bölümünü belirtin. Sahte değeri olan gizli ayar için herhangi bir giriş olduğundan emin olun.

   ```xml
   <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="secret" value="" />
   </appSettings>
   ```

4. Projehatasını çıkarmaya başlayın. Başarılı bir şekilde çalışmalı.
