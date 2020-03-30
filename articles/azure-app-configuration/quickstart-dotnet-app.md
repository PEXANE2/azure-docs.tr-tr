---
title: .NET Framework ile Azure Uygulama Yapılandırması için Hızlı Başlatma | Microsoft Dokümanlar
description: .NET Framework uygulamalarıyla Azure Uygulama Yapılandırması'nı kullanmak için hızlı bir başlangıç
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: lcozzens
ms.openlocfilehash: 1c56088a2c51c50c7f9cf1ff1e790d580fdb08d8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80245404"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Quickstart: Azure Uygulama Yapılandırması ile bir .NET Framework uygulaması oluşturun

Bu hızlı başlangıçta, uygulama ayarlarının kodunuzdan ayrı olarak depolanmasını ve yönetimini merkezileştirmek için Azure Uygulama Yapılandırmasını .NET Framework tabanlı bir konsol uygulamasına dahil edersiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Çerçeve 4.7.2](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar değeri çiftleri eklemek için **Configuration Explorer** > **Create** > Key**değerini** seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler |

    **Etiket** ve **İçerik Türünü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-net-console-app"></a>Bir .NET konsol uygulaması oluşturma

1. Visual Studio'yı başlatın ve **Dosya** > **Yeni** > **Projesi'ni**seçin.

1. **Yeni bir proje oluştur'da**Konsol proje türüne filtre uygulayın ve Console **Console** **App'a (.NET Framework)** tıklayın. **Sonraki'ni**seçin.

1. **Yeni projenizi yapılandırın,** bir proje adı girin. **Çerçeve**altında **,NET Framework 4.7.1** veya üzerini seçin. **Oluştur'u**seçin.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. **Gözat** sekmesinde, projenize aşağıdaki NuGet paketlerini arayın ve ekleyin. Bulamıyorsanız, **Yayın Öncesi Ekle** onay kutusunu seçin.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    System.Configuration.ConfigurationManager version 4.6.0 or later
    ```

1. Projenizin *App.config* dosyasını aşağıdaki gibi güncelleyin:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   App Configuration store'unuzun bağlantı dizesi `ConnectionString`ortam değişkeninden okunur. Bölümün `Environment` `MyConfigStore` `configBuilders` özelliğinden önce yapılandırma oluşturucuyu `appSettings` ekleyin.

1. *Program.cs*açın ve `Main` App Configuration'ı kullanarak `ConfigurationManager`arama yöntemini güncelleştirin.

    ```csharp
    static void Main(string[] args)
    {
        string message = System.Configuration.ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. App Configuration mağazanızın bağlantı dizesine **ConnectionString** adında bir ortam değişkeni ayarlayın. Windows komut istemini kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```cmd
        setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```azurepowershell
        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```
1. Değişikliğin etkili olmasını sağlamak için Visual Studio'yu yeniden başlatın. Konsol uygulamasını oluşturmak ve çalıştırmak için Ctrl + F5 tuşuna basın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve bir .NET Framework konsol uygulamasıyla kullandınız. Uygulama `AppSettings` başlatıldıktan sonra değeri `ConfigurationManager` değişmez. Ancak App Configuration .NET Standard yapılandırma sağlayıcısı kitaplığı bir .NET Framework uygulamasında da kullanılabilir. .NET Framework uygulamanızın yapılandırma ayarlarını dinamik olarak nasıl yenileyebildiğini öğrenmek için bir sonraki eğitime devam edin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-dotnet.md)
