---
title: .NET ile Azure Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını kullanın | Microsoft Dokümanlar
description: Bu konu, .NET ile Azure Medya Hizmetleri (AMS) API'sine erişmek için Azure Etkin Dizin (Azure AD) kimlik doğrulamasını nasıl kullanacağımı gösterir.
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
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: b53fca292630ef988ee1357ea50adc4d7b7e9be5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162888"
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a>.NET ile Azure Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını kullanma

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Windowsazure.mediaservices 4.0.0.4 ile başlayan Azure Media Services, Azure Etkin Dizini 'ni (Azure AD) temel alarak kimlik doğrulamayı destekler. Bu konu, Microsoft .NET ile Azure Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Ayrıntılar için [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın. 
- Bir Media Services hesabı. Daha fazla bilgi için [bkz.](media-services-portal-create-account.md)
- En son [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketi.
- [Azure AD kimlik doğrulamagenel bakış ile Azure Medya Hizmetleri API'ye erişim](media-services-use-aad-auth-to-access-ams-api.md)konusuna aşinalık. 

Azure Media Hizmetleri ile Azure AD kimlik doğrulamasını kullanırken, kimlik doğrulaması iki şekilde yapabilirsiniz:

- **Kullanıcı kimlik doğrulaması,** uygulamayı kullanan bir kişinin Azure Medya Hizmetleri kaynaklarıyla etkileşimde bulunan bir kişinin kimliğini doğrular. Etkileşimli uygulama öncelikle kullanıcıdan kimlik bilgileri için istekte olmalıdır. Buna örnek olarak, kodlama işlerini veya canlı akışı izlemek için yetkili kullanıcılar tarafından kullanılan bir yönetim konsolu uygulaması dır. 
- **Hizmet temel kimlik doğrulaması** bir hizmeti doğrular. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, daemon hizmetlerini, orta katman hizmetlerini veya web uygulamaları, işlev uygulamaları, mantık uygulamaları, API'ler veya mikro hizmetler gibi zamanlanmış işleri çalıştıran uygulamalardır.

>[!IMPORTANT]
>Azure Medya Hizmeti şu anda bir Azure Erişim Denetim Hizmeti kimlik doğrulama modelini destekler. Ancak, Erişim Denetimi yetkilendirmesi 22 Haziran 2018 tarihinde iptal edilecektir. Bir Azure Etkin Dizin kimlik doğrulama modeline mümkün olan en kısa sürede geçiş yapmanızı öneririz.

## <a name="get-an-azure-ad-access-token"></a>Azure AD erişim belirteci alma

Azure AD kimlik doğrulaması ile Azure Media Services API'sine bağlanmak için istemci uygulamasının Azure AD erişim belirteci istemesi gerekir. Media Services .NET istemciSi SDK'yı kullandığınızda, Azure AD erişim belirteci edinme ile ilgili ayrıntıların çoğu [AzureAdTokenProvider ve AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) sınıflarında sizin için sarılır ve basitleştirilir. [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) 

Örneğin, Azure REKLAM yetkilisini, Medya Hizmetleri kaynağı URI'yi veya yerel Azure AD uygulama ayrıntılarını sağlamanız gerekmez. Bunlar, Azure AD erişim token sağlayıcısı sınıfı tarafından zaten yapılandırılan iyi bilinen değerlerdir. 

Azure Media Service .NET SDK kullanmıyorsanız, [Azure AD Kimlik Doğrulama Kitaplığını](../../active-directory/azuread-dev/active-directory-authentication-libraries.md)kullanmanızı öneririz. Azure AD Kimlik Doğrulama Kitaplığı ile kullanmanız gereken parametreler için değerler almak için Azure [AD kimlik doğrulama ayarlarına erişmek için Azure portalını kullanın'a](media-services-portal-get-started-with-aad.md)bakın.

**AzureAdTokenProvider'ın** varsayılan uygulamasını kendi uygulamanızla değiştirme seçeneğiniz de var.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Azure Medya Hizmetlerini yükleyin ve yapılandırdı .NET SDK

>[!NOTE] 
>Medya Hizmetleri .NET SDK ile Azure AD kimlik doğrulamasını kullanmak için en son [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketine sahip olmanız gerekir. Ayrıca, **Microsoft.IdentityModel.Clients.ActiveDirectory** derlemesine bir başvuru ekleyin. Varolan bir uygulamayı kullanıyorsanız, **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** derlemesini ekleyin. 

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun.
2. **Azure Medya Hizmetleri .NET SDK'yı**yüklemek için [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet paketini kullanın. 

    NuGet'i kullanarak referans eklemek için aşağıdaki adımları izleyin: **Çözüm Gezgini'nde**proje adını sağ tıklatın ve ardından **NuGet paketlerini yönet'i**seçin. Ardından **windowsazure.mediaservices'i** arayın ve **Yükle'yi**seçin.
    
    -veya-

    Visual Studio'da **Package Manager Console'da** aşağıdaki komutu çalıştırın.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Kaynak kodunuza **ekleme.**

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Kullanıcı kimlik doğrulamasını kullanma

Kullanıcı kimlik doğrulama seçeneğiyle Azure Media Service API'sine bağlanmak için istemci uygulamasının aşağıdaki parametreleri kullanarak bir Azure AD belirteci istemesi gerekir:  

- Azure AD kiracı bitiş noktası. Kiracı bilgileri Azure portalından alınabilir. Sağ üst köşedeki oturum açmış kullanıcının üzerine titreyin.
- Medya Hizmetleri kaynak URI.
- Medya Hizmetleri (yerel) uygulama istemci kimliği. 
- Medya Hizmetleri (yerel) uygulaması URI yönlendirme. 

Bu parametrelerin değerleri **AzureEnvironments.AzureCloudEnvironment'da**bulunabilir. **AzureEnvironments.AzureCloudEnvironment** sabiti, ortak bir Azure Veri Merkezi için doğru ortam değişken ayarlarını elde etmek için .NET SDK'daki bir yardımcıdır. 

Yalnızca genel veri merkezlerinde Medya Hizmetlerine erişmek için önceden tanımlanmış ortam ayarları içerir. Bağımsız veya bağımsız bulut bölgeleri için sırasıyla **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvironment**veya **AzureGermanCloudEnvironment'ı** kullanabilirsiniz.

Aşağıdaki kod örneği bir belirteç oluşturur:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
Medya Hizmetleri'ne karşı programlamayı başlatmak için sunucu bağlamını temsil eden bir **CloudMediaContext** örneği oluşturmanız gerekir. **CloudMediaContext,** işler, varlıklar, dosyalar, erişim ilkeleri ve bulucular gibi önemli koleksiyonlara başvurular içerir. 

Ayrıca Media REST **Hizmetleri için uri kaynağını** **CloudMediaContext** oluşturucuya aktarmanız gerekir. Media REST Services için kaynak URI'yi almak için Azure portalında oturum açın, Azure Medya Hizmetleri hesabınızı seçin, **API erişimini**seçin ve **ardından kullanıcı kimlik doğrulaması yla Azure Medya Hizmetlerine Bağlan'ı**seçin. 

Aşağıdaki kod örneği bir **CloudMediaContext** örneği oluşturur:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

Aşağıdaki örnekte, Azure AD belirteci ve bağlamNasıl oluşturulacak gösterilmektedir:

    namespace AzureADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
>"Uzak sunucu bir hata döndü: (401) Yetkisiz" yazan bir özel durum alırsanız, Azure AD kimlik doğrulama genel bakışıyla Azure Medya Hizmetleri API'ye Erişim denetim [bölümüne](media-services-use-aad-auth-to-access-ams-api.md#access-control) bakın.

## <a name="use-service-principal-authentication"></a>Hizmet temel kimlik doğrulamasını kullanma
    
Azure Medya Hizmetleri API'sine hizmet temel seçeneğiyle bağlanmak için, orta katman uygulamanızın (web API veya web uygulaması) aşağıdaki parametrelere sahip bir Azure AD belirteci istemesi gerekir:  

- Azure AD kiracı bitiş noktası. Kiracı bilgileri Azure portalından alınabilir. Sağ üst köşedeki oturum açmış kullanıcının üzerine titreyin.
- Medya Hizmetleri kaynak URI.
- Azure AD uygulama değerleri: **İstemci kimliği** ve **İstemci sırrı.**

**İstemci Kimliği** ve **İstemci gizli** parametreleri için değerler Azure portalında bulunabilir. Daha fazla bilgi için Bkz. [Azure portalını kullanarak Azure AD kimlik doğrulaması ile başlarken.](media-services-portal-get-started-with-aad.md)

Aşağıdaki kod örneği, **AzureAdClientSymmetricKey'i** parametre olarak alan **AzureAdTokenCredentials** oluşturucuyu kullanarak bir belirteç oluşturur: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

**AzureAdClientCertificate'ı** parametre olarak alan **AzureAdTokenCredentials** oluşturucuyu da belirtebilirsiniz. 

Azure AD tarafından kullanılabilecek bir formda sertifikanın nasıl oluşturulup yapılandırılacağına ilişkin talimatlar için, [sertifikalı daemon uygulamalarında Azure AD'ye Kimlik Doğrulaması -el ile yapılandırma adımları'](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md)na bakın.

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

Medya Hizmetleri'ne karşı programlamayı başlatmak için sunucu bağlamını temsil eden bir **CloudMediaContext** örneği oluşturmanız gerekir. Ayrıca Media REST **Hizmetleri için uri kaynağını** **CloudMediaContext** oluşturucuya aktarmanız gerekir. Media REST **Hizmetleri için URI kaynağını** Azure portalından da alabilirsiniz.

Aşağıdaki kod örneği bir **CloudMediaContext** örneği oluşturur:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
Aşağıdaki örnekte, Azure AD belirteci ve bağlamNasıl oluşturulacak gösterilmektedir:

    namespace AzureADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a>Sonraki adımlar

[Hesabınıza dosya yüklemeye](media-services-dotnet-upload-files.md)başlayın.
