---
title: Azure Medya Hizmetleri v3 API'ye bağlanın - .NET
description: Bu makalede, .NET ile Media Services v3 API'ye nasıl bağlanılalışiz gösteriş ve gösteriş.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2019
ms.author: juliako
ms.openlocfilehash: b8f4de1a5b9d8216ae2442631f5f9135c3c72d0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269815"
---
# <a name="connect-to-media-services-v3-api---net"></a>Medya Hizmetlerine bağlan v3 API - .NET

Bu makalede, hizmet temel giriş yöntemini kullanarak Azure Media Services v3 .NET SDK'ya nasıl bağlanabileceğinizgösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun
- .NET geliştirme için kullanmak istediğiniz bir araç yükleyin. Bu makaledeki adımlar Visual [Studio 2019 Community Edition'ın](https://www.visualstudio.com/downloads/)nasıl kullanılacağını göstermektedir. Visual Studio Code'u kullanabilir, [Bkz. C# ile Çalışma](https://code.visualstudio.com/docs/languages/csharp). Veya farklı bir kod düzenleyicisi kullanabilirsiniz.

> [!IMPORTANT]
> Adlandırma kurallarını gözden [geçirin.](media-services-apis-overview.md#naming-conventions)

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

1. Visual Studio’yu çalıştırın. 
1. **Dosya** menüsünden **Yeni** > **Proje'yi**tıklatın. 
1. Bir **.NET Core** konsol uygulaması oluşturun.

Bu konuda örnek uygulama, `netcoreapp2.0`hedefler . Kod, C# 7.1 ile başlayan 'async main'i kullanır. Daha fazla bilgi için bu [bloga](https://blogs.msdn.microsoft.com/benwilli/2017/12/08/async-main-is-available-but-hidden/) bakın.

## <a name="add-required-nuget-packages"></a>Gerekli NuGet paketlerini ekleme

1. Visual Studio'da **Araçlar** > **NuGet Paket Yöneticisi** > **NuGet Manager Console'u**seçin.
2. Paket **Yöneticisi Konsolu** penceresinde, aşağıdaki NuGet paketlerini eklemek için komutu kullanın. `Install-Package` Örneğin, `Install-Package Microsoft.Azure.Management.Media`.

|Paket|Açıklama|
|---|---|
|`Microsoft.Azure.Management.Media`|Azure Medya Hizmetleri SDK. <br/>En son Azure Medya Hizmetleri paketini kullandığınızdan emin olmak için [Microsoft.Azure.Management.Media'ya](https://www.nuget.org/packages/Microsoft.Azure.Management.Media)bakın.|
|`Microsoft.Rest.ClientRuntime.Azure.Authentication`|NET için Azure SDK için ADAL kimlik doğrulama kitaplığı|
|`Microsoft.Extensions.Configuration.EnvironmentVariables`|Ortam değişkenlerinden ve yerel JSON dosyalarından yapılandırma değerlerini okuma|
|`Microsoft.Extensions.Configuration.Json`|Ortam değişkenlerinden ve yerel JSON dosyalarından yapılandırma değerlerini okuma
|`WindowsAzure.Storage`|Depolama SDK|

## <a name="create-and-configure-the-app-settings-file"></a>Uygulama ayarları dosyasını oluşturma ve yapılandırma

### <a name="create-appsettingsjson"></a>appsettings.json oluşturma

1. **Genel** > **Metin dosyası**gidin .
1. Adını "appsettings.json" olarak adlandır.
1. .json dosyasının "Çıktı Dizini"ne kopyala özelliğini "Daha yeniyse kopyala" olarak ayarlayın (böylece uygulama yayımlandığında dosyaya erişebilsin).

### <a name="set-values-in-appsettingsjson"></a>appsettings.json'da değerleri ayarlama

Access `az ams account sp create` [API'lerinde](access-api-cli-how-to.md)açıklandığı gibi komutu çalıştırın. Komut, "appsettings.json"unuza kopyalamanız gerektiğini gösterir.
 
## <a name="add-configuration-file"></a>Yapılandırma dosyasını ekleme

Kolaylık sağlamak için, "appsettings.json" adresinden değerleri okumaktan sorumlu bir yapılandırma dosyası ekleyin.

1. Projenize yeni bir .cs sınıfı ekleyin. Bunu, `ConfigWrapper` olarak adlandırın. 
1. Bu dosyaya aşağıdaki kodu yapıştırın (bu örnek, ad `ConsoleApp1`alanı olduğunu varsayar).

```csharp
using System;

using Microsoft.Extensions.Configuration;

namespace ConsoleApp1
{
    public class ConfigWrapper
    {
        private readonly IConfiguration _config;

        public ConfigWrapper(IConfiguration config)
        {
            _config = config;
        }

        public string SubscriptionId
        {
            get { return _config["SubscriptionId"]; }
        }

        public string ResourceGroup
        {
            get { return _config["ResourceGroup"]; }
        }

        public string AccountName
        {
            get { return _config["AccountName"]; }
        }

        public string AadTenantId
        {
            get { return _config["AadTenantId"]; }
        }

        public string AadClientId
        {
            get { return _config["AadClientId"]; }
        }

        public string AadSecret
        {
            get { return _config["AadSecret"]; }
        }

        public Uri ArmAadAudience
        {
            get { return new Uri(_config["ArmAadAudience"]); }
        }

        public Uri AadEndpoint
        {
            get { return new Uri(_config["AadEndpoint"]); }
        }

        public Uri ArmEndpoint
        {
            get { return new Uri(_config["ArmEndpoint"]); }
        }

        public string Region
        {
            get { return _config["Region"]; }
        }
    }
}
```

## <a name="connect-to-the-net-client"></a>.NET istemcisine bağlanın

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Aşağıdaki kodda, GetCredentialsAsync işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClient Credentials nesnesini oluşturur.

1. `Program.cs` dosyasını açın.
1. Aşağıdaki kodu yapıştırın:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;

using Microsoft.Azure.Management.Media;
using Microsoft.Azure.Management.Media.Models;
using Microsoft.Extensions.Configuration;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace ConsoleApp1
{
    class Program
    {
        public static async Task Main(string[] args)
        {
            
            ConfigWrapper config = new ConfigWrapper(new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddEnvironmentVariables()
                .Build());

            try
            {
                IAzureMediaServicesClient client = await CreateMediaServicesClientAsync(config);
                Console.WriteLine("connected");
            }
            catch (Exception exception)
            {
                if (exception.Source.Contains("ActiveDirectory"))
                {
                    Console.Error.WriteLine("TIP: Make sure that you have filled out the appsettings.json file before running this sample.");
                }

                Console.Error.WriteLine($"{exception.Message}");

                ApiErrorException apiException = exception.GetBaseException() as ApiErrorException;
                if (apiException != null)
                {
                    Console.Error.WriteLine(
                        $"ERROR: API call failed with error code '{apiException.Body.Error.Code}' and message '{apiException.Body.Error.Message}'.");
                }
            }

            Console.WriteLine("Press Enter to continue.");
            Console.ReadLine();
        }
 
        private static async Task<ServiceClientCredentials> GetCredentialsAsync(ConfigWrapper config)
        {
            // Use ApplicationTokenProvider.LoginSilentWithCertificateAsync or UserTokenProvider.LoginSilentAsync to get a token using service principal with certificate
            //// ClientAssertionCertificate
            //// ApplicationTokenProvider.LoginSilentWithCertificateAsync

            // Use ApplicationTokenProvider.LoginSilentAsync to get a token using a service principal with symmetric key
            ClientCredential clientCredential = new ClientCredential(config.AadClientId, config.AadSecret);
            return await ApplicationTokenProvider.LoginSilentAsync(config.AadTenantId, clientCredential, ActiveDirectoryServiceSettings.Azure);
        }

        private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
        {
            var credentials = await GetCredentialsAsync(config);

            return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
            {
                SubscriptionId = config.SubscriptionId,
            };
        }

    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Yükleme, kodlama ve akış videoları - .NET](stream-files-tutorial-with-api.md) 
- [Öğretici: Medya Hizmetleri v3 ile canlı yayın - .NET](stream-live-tutorial-with-api.md)
- [Öğretici: Medya Hizmetleri v3 ile videoları analiz - .NET](analyze-videos-tutorial-with-api.md)
- [Yerel bir dosyadan iş girişi oluşturma - .NET](job-input-from-local-file-how-to.md)
- [HTTPS URL'sinden iş girişi oluşturma - .NET](job-input-from-http-how-to.md)
- [Özel Bir Dönüştür ile kodlayın - .NET](customize-encoder-presets-how-to.md)
- [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanın - .NET](protect-with-aes128.md)
- [DRM dinamik şifreleme ve lisans teslim hizmetini kullanın - .NET](protect-with-drm.md)
- [Varolan ilkeden imza anahtarı alın - .NET](get-content-key-policy-dotnet-howto.md)
- [Medya Hizmetleri ile filtre oluşturma - .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Media Services v3 ile Azure Fonksiyonları v2 gelişmiş video isteğe bağlı örnekleri](https://aka.ms/ams3functions)

## <a name="see-also"></a>Ayrıca bkz.

* [.NET başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* Daha fazla kod örneği için [.NET SDK örnekleri](https://github.com/Azure-Samples/media-services-v3-dotnet) repo'ya bakın.
