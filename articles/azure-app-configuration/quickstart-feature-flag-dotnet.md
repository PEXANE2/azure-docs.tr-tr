---
title: .NET Framework uygulamalarına özellik bayrakları eklemek için hızlı başlangıç | Microsoft Dokümanlar | Microsoft Dokümanlar
description: .NET Framework uygulamalarına özellik bayrakları eklemek ve Azure Uygulama Yapılandırması'nda bunları yönetmek için hızlı bir başlangıç
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 10/21/2019
ms.author: lcozzens
ms.openlocfilehash: 5ea9749c07aadc7037e753160e9b053992bebae2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77619299"
---
# <a name="quickstart-add-feature-flags-to-a-net-framework-app"></a>Hızlı başlatma: Bir .NET Framework uygulamasına özellik bayrakları ekleme

Bu hızlı başlangıçta, özellik yönetiminin uçtan uca uygulanmasını oluşturmak için Azure Uygulama Yapılandırmasını bir .NET Framework uygulamasına dahil eleştirirsiniz. Tüm özellik bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için Uygulama Yapılandırma hizmetini kullanabilirsiniz. 

.NET Özellik Yönetimi kitaplıkları, kapsamlı özellik bayrak desteği yle çerçeveyi genişletir. Bu kitaplıklar .NET yapılandırma sisteminin üzerine inşa edilmiştir. .NET yapılandırma sağlayıcısı aracılığıyla App Configuration ile sorunsuz bir şekilde entegre olurlar.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Çerçeve 4.8](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **Özellik Yöneticisi** > **+Ekle** adlı bir `Beta`özellik bayrağı eklemek için seçin.

    > [!div class="mx-imgBorder"]
    > ![Beta adlı özellik bayrağını etkinleştirme](media/add-beta-feature-flag.png)

    Şimdilik `label` tanımsız bırak.

## <a name="create-a-net-console-app"></a>Bir .NET konsol uygulaması oluşturma

1. Visual Studio'yı başlatın ve **Dosya** > **Yeni** > **Projesi'ni**seçin.

1. **Yeni bir proje oluştur'da**Konsol proje türüne filtre uygulayın ve Console **Console** **App'a (.NET Framework)** tıklayın. **İleri**'ye tıklayın.

1. **Yeni projenizi yapılandırın,** bir proje adı girin. **Çerçeve**altında **,NET Framework 4.8** veya üzerini seçin. **Oluştur'u**tıklatın.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. **Gözat** sekmesinde, projenize aşağıdaki NuGet paketlerini arayın ve ekleyin. Bulamıyorsanız, **Yayın Öncesi Ekle** onay kutusunu seçin.

    ```
    Microsoft.Extensions.DependencyInjection
    Microsoft.Extensions.Configuration.AzureAppConfiguration
    Microsoft.FeatureManagement
    ```

1. *açık Program.cs* ve aşağıdaki ifadeleri ekleyin:

    ```csharp
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    using Microsoft.FeatureManagement;
    ```

1. Özellik `Main` bayraklarının alınması için `UseFeatureFlags` seçeneği belirterek Uygulama Yapılandırması'na bağlanmak için yöntemi güncelleştirin. Ardından özellik bayrağı `Beta` etkinse bir ileti görüntüleyin.

    ```csharp
        public static async Task Main(string[] args)
        {         
            IConfigurationRoot configuration = new ConfigurationBuilder()
                .AddAzureAppConfiguration(options =>
                {
                    options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                           .UseFeatureFlags();
                }).Build();

            IServiceCollection services = new ServiceCollection();

            services.AddSingleton<IConfiguration>(configuration).AddFeatureManagement();

            using (ServiceProvider serviceProvider = services.BuildServiceProvider())
            {
                IFeatureManager featureManager = serviceProvider.GetRequiredService<IFeatureManager>();

                if (await featureManager.IsEnabledAsync("Beta"))
                {
                    Console.WriteLine("Welcome to the beta!");
                }
            }

            Console.WriteLine("Hello World!");
        }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. App Configuration mağazanızın bağlantı dizesine **ConnectionString** adında bir ortam değişkeni ayarlayın. Windows komut istemini kullanıyorsanız, aşağıdaki komutu çalıştırın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

1. Değişikliğin etkili olmasını sağlamak için Visual Studio'yu yeniden başlatın. 

1. Konsol uygulamasını oluşturmak ve çalıştırmak için Ctrl + F5 tuşuna basın.

    ![Özellik bayrağı etkin leştirilmiş uygulama](./media/quickstarts/dotnet-app-feature-flag.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, App Configuration'da bir özellik bayrağı oluşturdunuz ve bir .NET Framework konsol uygulamasıyla kullandınız. Uygulamayı yeniden başlatmadan özellik bayraklarını ve diğer yapılandırma değerlerini dinamik olarak nasıl güncelleştireceğiz öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-dotnet.md)
