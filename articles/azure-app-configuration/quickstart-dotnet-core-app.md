---
title: .NET Core ile Azure Uygulama Yapılandırması için hızlı başlatma | Microsoft Dokümanlar
description: .NET Core uygulamalarıyla Azure Uygulama Yapılandırması'nı kullanmak için hızlı bir başlangıç
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 1/9/2019
ms.author: lcozzens
ms.openlocfilehash: 420d9b48013f5f6debe588667fe1cc0390517e66
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80245387"
---
# <a name="quickstart-create-a-net-core-app-with-app-configuration"></a>Quickstart: Uygulama Yapılandırması ile bir .NET Core uygulaması oluşturun

Bu hızlı başlangıçta, uygulama ayarlarının kodunuzdan ayrı olarak depolanmasını ve yönetimini merkezileştirmek için Azure Uygulama Yapılandırmasını bir .NET Core konsol uygulamasına dahil edersiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download) - [Azure Bulut Kabuğu'nda](https://shell.azure.com)da mevcuttur.

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar değeri çiftleri eklemek için **Configuration Explorer** > **Create** > Key**değerini** seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler |

    **Etiket** ve **İçerik Türünü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-net-core-console-app"></a>Bir .NET Core konsol uygulaması oluşturma

Yeni bir .NET Core konsol uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanırsınız. .NET Core CLI'yi Visual Studio'ya göre kullanmanın avantajı, Windows, macOS ve Linux platformlarında kullanılabilmesidir.  Alternatif olarak, [Azure Bulut Kabuğu'nda](https://shell.azure.com)bulunan önceden yüklenmiş araçları kullanın.

1. Projeniz için yeni bir klasör oluşturun.

2. Yeni klasörde, yeni bir ASP.NET Core konsol uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet new console
    ```

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. `Microsoft.Extensions.Configuration.AzureAppConfiguration` Aşağıdaki komutu çalıştırarak NuGet paketine bir başvuru ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration
    ```

2. Projeniz için paketleri geri yüklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet restore
    ```

3. *Program.cs*açın ve .NET Core App Configuration sağlayıcısına bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

4. Yöntemi `Main` arayarak Uygulama Yapılandırması'nı kullanmak için yöntemi güncelleştirin. `builder.AddAzureAppConfiguration()`

    ```csharp
    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));

        var config = builder.Build();
        Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. ConnectionString adında bir ortam **değişkeni**ayarlayın ve Uygulama Yapılandırma mağazanızın erişim anahtarına ayarlayın. Komut satırında aşağıdaki komutu çalıştırın:

    ```cmd
    setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```azurepowershell
    $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    macOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

    Değişikliğin etkili olmasını sağlamak için komut istemini yeniden başlatın. Düzgün ayarlanmış olduğunu doğrulamak için ortam değişkeninin değerini yazdırın.

2. Konsol uygulamasını oluşturmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

3. Yapı başarıyla tamamlandıktan sonra, uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet run
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve [Uygulama Yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir .NET Core konsol uygulaması yla kullandınız. .NET Core uygulamanızı yapılandırma ayarlarını dinamik olarak yenilecek şekilde nasıl yapılandırılacak öğrenebilirsiniz, bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-dotnet-core.md)
