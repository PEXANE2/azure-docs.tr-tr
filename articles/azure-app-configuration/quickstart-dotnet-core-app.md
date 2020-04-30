---
title: .NET Core ile Azure Uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: .NET Core uygulamalarıyla Azure Uygulama yapılandırması 'nı kullanmaya yönelik hızlı başlangıç
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 1/9/2019
ms.author: lcozzens
ms.openlocfilehash: 420d9b48013f5f6debe588667fe1cc0390517e66
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80245387"
---
# <a name="quickstart-create-a-net-core-app-with-app-configuration"></a>Hızlı başlangıç: uygulama yapılandırmasıyla .NET Core uygulaması oluşturma

Bu hızlı başlangıçta, kodınızdan ayrı uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure uygulama yapılandırmasını bir .NET Core konsol uygulamasına katabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download) , [Azure Cloud Shell](https://shell.azure.com)de mevcuttur.

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar-değer çiftlerini eklemek için **yapılandırma Gezgini** > **Create** > **anahtar değeri** oluştur ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-net-core-console-app"></a>.NET Core konsol uygulaması oluşturma

[.NET Core komut satırı arabirimi 'ni (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanarak yeni bir .NET Core konsol uygulaması projesi oluşturun. Visual Studio üzerinden .NET Core CLI kullanmanın avantajı, Windows, macOS ve Linux platformları genelinde kullanılabilir hale gelir.  Alternatif olarak, [Azure Cloud Shell](https://shell.azure.com)bulunan önceden yüklenmiş araçları kullanın.

1. Projeniz için yeni bir klasör oluşturun.

2. Yeni klasörde, yeni bir ASP.NET Core konsol uygulama projesi oluşturmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet new console
    ```

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Aşağıdaki komutu çalıştırarak `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet paketine bir başvuru ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration
    ```

2. Projenizin paketlerini geri yüklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet restore
    ```

3. *Program.cs*'i açın ve .NET Core uygulama yapılandırma sağlayıcısına bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

4. Yöntemini çağırarak `Main` `builder.AddAzureAppConfiguration()` uygulama yapılandırmasını kullanmak için yöntemi güncelleştirin.

    ```csharp
    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));

        var config = builder.Build();
        Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. **ConnectionString**adlı bir ortam değişkeni ayarlayın ve uygulama yapılandırma deponuzu için erişim anahtarı olarak ayarlayın. Komut satırında aşağıdaki komutu çalıştırın:

    ```cmd
    setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

    ```azurepowershell
    $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    MacOS veya Linux kullanıyorsanız şu komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

    Değişikliğin etkili olması için komut istemi ' ni yeniden başlatın. Düzgün ayarlandığını doğrulamak için ortam değişkeninin değerini yazdırın.

2. Konsol uygulamasını derlemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

3. Oluşturma başarıyla tamamlandıktan sonra, uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet run
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturdunuz ve [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir .NET Core konsol uygulamasıyla kullandınız. .NET Core uygulamanızı yapılandırma ayarlarını dinamik olarak yenilemek üzere nasıl yapılandıracağınızı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştir](./enable-dynamic-configuration-dotnet-core.md)
