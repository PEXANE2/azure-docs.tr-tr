---
title: Azure Uygulama yapılandırması için .NET Framework ile hızlı başlangıç | Microsoft Docs
description: Azure uygulama yapılandırmasını .NET Framework uygulamalarla kullanmaya yönelik hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 8aa8c8132220965d55097c4fed8ba1b2e9501301
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326522"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Hızlı Başlangıç: Azure Uygulama yapılandırması ile .NET Framework uygulaması oluşturma

Bu hızlı başlangıçta, kodınızdan ayrı uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure uygulama yapılandırmasını .NET Framework tabanlı bir konsol uygulamasına katabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.1](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar-değer çiftlerini eklemek için **yapılandırma Gezgini** >  **+ Oluştur** ' u seçin:

    | Anahtar | Value |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

## <a name="create-a-net-console-app"></a>.NET konsol uygulaması oluşturma

1. Visual Studio 'yu başlatın ve **Dosya** > **Yeni** > **Proje**' yi seçin.

2. **Yeni proje**' de, **yüklü** >   > **Visual C#**  **Windows Desktop**' ı seçin. **Konsol uygulaması (.NET Framework)** öğesini seçin ve projeniz için bir ad girin. **.NET Framework 4.7.1** veya yukarı ' yı seçin ve **Tamam**' ı seçin.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. **Araştır** sekmesinde, aşağıdaki NuGet paketlerini arayıp projenize ekleyin. Bunları bulamıyorsanız, **ön sürümü dahil et** onay kutusunu seçin.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Projenizin *app. config* dosyasını aşağıdaki gibi güncelleştirin:

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

   Uygulama yapılandırma deponuzın bağlantı dizesi, ortam değişkeninden `ConnectionString`okundu. Bölümünün özelliğindeki öğesinden önce `MyConfigStore` `Environment`yapılandırmaoluşturucuyuekleyin `configBuilders`. `appSettings`

3. *Program.cs*'i açın ve çağırarak `Main` `ConfigurationManager`uygulama yapılandırmasını kullanmak üzere yöntemi güncelleştirin.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. **ConnectionString** adlı bir ortam değişkenini uygulama yapılandırma deponuzın bağlantı dizesine ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Değişikliğin etkili olması için Visual Studio 'Yu yeniden başlatın. Konsol uygulamasını derlemek ve çalıştırmak için CTRL + F5 tuşlarına basın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir uygulama yapılandırma deposu oluşturdunuz ve bir .NET Framework konsol uygulamasıyla kullandınız. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için, kimlik doğrulamasını gösteren bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
