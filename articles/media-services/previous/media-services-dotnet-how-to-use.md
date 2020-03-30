---
title: .NET ile Medya Hizmetleri Geliştirme için Bilgisayar Nasıl Ayarlanır?
description: .NET için Medya Hizmetleri SDK'yı kullanarak Medya Hizmetleri için ön koşullar hakkında bilgi edinin. Ayrıca Visual Studio uygulaması oluşturmayı da öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 51fffbd170daecfec6fcea95caa0526e6d881407
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64724108"
---
# <a name="media-services-development-with-net"></a>.NET ile Medya Hizmetleri geliştirme 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Bu makalede, .NET kullanarak Medya Hizmetleri uygulamalarını geliştirmeye nasıl başlarıaçıklanmıştır.

**Azure Media Services .NET SDK** kitaplığı ,NET'i kullanarak Medya Hizmetlerine karşı program yapmanızı sağlar. .NET ile geliştirmeyi daha da kolaylaştırmak için **Azure Media Services .NET SDK Uzantıları** kitaplığı sağlanır. Bu kitaplık, .NET kodunuzu basitleştiren bir dizi uzantı yöntemi ve yardımcı işlevi içerir. Her iki kitaplık da **NuGet** ve **GitHub**üzerinden kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
* Yeni veya mevcut bir Azure aboneliğinde bir Media Services hesabı. Makaleye bakın [Medya Hizmetleri Hesabı Oluşturma .](media-services-portal-create-account.md)
* İşletim Sistemleri: Windows 10, Windows 7, Windows 2008 R2 veya Windows 8.
* .NET Framework 4.5 veya sonrası.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma
Bu bölümde Visual Studio'da nasıl bir proje oluşturulup Medya Hizmetleri geliştirmeiçin nasıl ayarlayabileceğiniz gösterilmektedir.  Bu durumda, proje bir C# Windows konsol uygulamasıdır, ancak burada gösterilen aynı kurulum adımları Medya Hizmetleri uygulamaları için oluşturabileceğiniz diğer proje türleri (örneğin, Windows Forms uygulaması veya ASP.NET Web uygulaması) için geçerlidir.

Bu bölümde, Medya Hizmetleri .NET SDK uzantıları ve diğer bağımlı kitaplıkları eklemek için **NuGet'in** nasıl kullanılacağı gösterilmektedir.

Alternatif olarak, GitHub 'dan[(github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) veya [github.com/Azure/azure-sdk-for-media-services-extensions)](https://github.com/Azure/azure-sdk-for-media-services-extensions)en son Medya Hizmetleri .NET SDK bitlerini alabilir), çözümü oluşturabilir ve müşteri projesine başvuruları ekleyebilirsiniz. Gerekli tüm bağımlılıklar otomatik olarak indirilir ve ayıklanır.

1. Visual Studio’da yeni bir C# Konsol Uygulaması oluşturun. **Ad,** **Konum**ve **Çözüm adını**girin ve ardından Tamam'ı tıklatın.
2. Çözümü derleyin.
3. **Azure Medya Hizmetleri .NET SDK Uzantılarını** **(windowsazure.mediaservices.extensions)** yüklemek ve eklemek için **NuGet'i** kullanın. Bu paketin yüklenmesiyle **Media Services .NET SDK** da yüklenir ve diğer tüm gerekli bağımlılıklar eklenir.
   
    NuGet'in en yeni sürümünün yüklü olduğundan emin olun. Daha fazla bilgi ve yükleme yönergeleri için [NuGet'e](https://nuget.codeplex.com/)bakın.

    1. Çözüm Gezgini'nde, projenin adını sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin.

    2. NuGet Paketlerini Yönet iletişim kutusu görünür.

    3. Çevrimiçi galeride Azure MediaServices Uzantıları'nı arayın, **Azure Media Services .NET SDK Uzantıları** **'nı (windowsazure.mediaservices.extensions)** seçin ve sonra **Yükle** düğmesini tıklatın.
   
    4. Proje değiştirilir ve Medya Hizmetleri .NET SDK Uzantıları, Medya Hizmetleri .NET SDK ve diğer bağımlı derlemelere başvurular eklenir.
4. Daha temiz bir geliştirme ortamını tanıtmak için NuGet Paketi Geri Yükleme'yi etkinleştirmeyi düşünün. Daha fazla bilgi için [NuGet Paketi Geri Yükleme"](https://docs.nuget.org/consume/package-restore)başlıklı bilgi için.
5. **System.Configuration** derlemesine bir başvuru ekleyin. Bu derleme System.Configuration içerir. Yapılandırma dosyalarına erişmek için kullanılan **ConfigurationManager** sınıfı (örneğin, App.config).
   
    1. Başvuruları Yönet iletişim kutusunu kullanarak başvuru eklemek için Çözüm Gezgini'ndeki proje adını sağ tıklatın. Ardından **Ekle'yi**tıklatın, ardından **Başvuru...'u tıklatın.**
   
    2. Başvuruları Yönet iletişim kutusu görüntülenir.
    3. .NET çerçeve derlemeleri altında System.Configuration derlemesini bulun ve seçin ve **Tamam**tuşuna basın.
6. App.config dosyasını açın ve dosyaya bir **uygulama Ayarları** bölümü ekleyin. Medya Hizmetleri API'sine bağlanmak için gereken değerleri ayarlayın. Daha fazla bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

    **Hizmet ana** kimlik doğrulama yöntemini kullanarak bağlanmak için gereken değerleri ayarlayın.

        ```csharp
                <configuration>
                ...
                    <appSettings>
                        <add key="AMSAADTenantDomain" value="tenant"/>
                        <add key="AMSRESTAPIEndpoint" value="endpoint"/>
                        <add key="AMSClientId" value="id"/>
                        <add key="AMSClientSecret" value="secret"/>
                    </appSettings>
                </configuration>
        ```

7. Projenize **System.Configuration** referansını ekleyin.
8. Program.cs dosyanın başındaki mevcut **ifadeleri** aşağıdaki kodla üzerine yazın:

    ```csharp      
            using System;
            using System.Configuration;
            using System.IO;
            using Microsoft.WindowsAzure.MediaServices.Client;
            using System.Threading;
            using System.Collections.Generic;
            using System.Linq;
    ```

    Bu noktada, bir Medya Hizmetleri uygulaması geliştirmeye başlamaya hazırsınız.    

## <a name="example"></a>Örnek

Burada AMS API'ye bağlanan ve kullanılabilir tüm Medya İşlemcileri listeleyen küçük bir örnek verilmiştir.

```csharp
        class Program
        {
            // Read values from the App.config file.

            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AMSAADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
            private static readonly string _AMSClientId =
                ConfigurationManager.AppSettings["AMSClientId"];
            private static readonly string _AMSClientSecret =
                ConfigurationManager.AppSettings["AMSClientSecret"];
        
            private static CloudMediaContext _context = null;
            static void Main(string[] args)
            {
                AzureAdTokenCredentials tokenCredentials = 
                    new AzureAdTokenCredentials(_AADTenantDomain,
                        new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                        AzureEnvironments.AzureCloudEnvironment);

                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
        
                // List all available Media Processors
                foreach (var mp in _context.MediaProcessors)
                {
                    Console.WriteLine(mp.Name);
                }
        
            }
 ```

## <a name="next-steps"></a>Sonraki adımlar

Artık [AMS API'sine bağlanabilir](media-services-use-aad-auth-to-access-ams-api.md) ve [geliştirmeye başlayabilirsiniz.](media-services-dotnet-get-started.md)


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

