---
title: Azure Uygulama yapılandırması için .NET Framework ile hızlı başlangıç | Microsoft Docs
description: Azure uygulama yapılandırmasını .NET Framework uygulamalarla kullanmaya yönelik hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: lcozzens
ms.openlocfilehash: 1c56088a2c51c50c7f9cf1ff1e790d580fdb08d8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80245404"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama yapılandırmasıyla .NET Framework uygulaması oluşturma

Bu hızlı başlangıçta, kodınızdan ayrı uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure uygulama yapılandırmasını .NET Framework tabanlı bir konsol uygulamasına katabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar-değer çiftlerini eklemek için **yapılandırma Gezgini** > **Create** > **anahtar değeri** oluştur ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-net-console-app"></a>.NET konsol uygulaması oluşturma

1. Visual Studio 'yu başlatın ve **Dosya** > **Yeni** > **Proje**' yi seçin.

1. **Yeni proje oluştur**' da **konsol** proje türü ' ne filtre uygulayın ve konsol uygulaması ' na tıklayın **(.NET Framework)**. **İleri**’yi seçin.

1. **Yeni projenizi yapılandırma**bölümünde bir proje adı girin. **Framework**altında **.NET Framework 4.7.1** veya üstünü seçin. **Oluştur**’u seçin.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. **Araştır** sekmesinde, aşağıdaki NuGet paketlerini arayıp projenize ekleyin. Bunları bulamıyorsanız, **ön sürümü dahil et** onay kutusunu seçin.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    System.Configuration.ConfigurationManager version 4.6.0 or later
    ```

1. Projenizin *app. config* dosyasını aşağıdaki gibi güncelleştirin:

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

   Uygulama yapılandırma deponuzın bağlantı dizesi, ortam değişkeninden `ConnectionString`okundu. `appSettings` Bölümünün `configBuilders` özelliğindeki `Environment` öğesinden önce `MyConfigStore` yapılandırma oluşturucuyu ekleyin.

1. *Program.cs*'i açın ve çağırarak `Main` `ConfigurationManager`uygulama yapılandırmasını kullanmak üzere yöntemi güncelleştirin.

    ```csharp
    static void Main(string[] args)
    {
        string message = System.Configuration.ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. **ConnectionString** adlı bir ortam değişkenini uygulama yapılandırma deponuzın bağlantı dizesine ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın:

    ```cmd
        setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

    ```azurepowershell
        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```
1. Değişikliğin etkili olması için Visual Studio 'Yu yeniden başlatın. Konsol uygulamasını derlemek ve çalıştırmak için CTRL + F5 tuşlarına basın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir uygulama yapılandırma deposu oluşturdunuz ve bir .NET Framework konsol uygulamasıyla kullandınız. Değeri `AppSettings` `ConfigurationManager` uygulama başlatıldıktan sonra değişmez. Uygulama yapılandırma .NET Standard yapılandırma sağlayıcısı kitaplığı, ancak aynı zamanda bir .NET Framework uygulamasında da kullanılabilir. Yapılandırma ayarlarını dinamik olarak yenilemek üzere .NET Framework uygulamanızın nasıl etkinleştirileceğini öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştir](./enable-dynamic-configuration-dotnet.md)
