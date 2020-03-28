---
title: 'Öğretici: .NET Core uygulamasında dinamik yapılandırmayı kullanın'
titleSuffix: Azure App Configuration
description: Bu eğitimde, .NET Core uygulamalarının yapılandırma verilerini dinamik olarak nasıl güncelleştirdiğinizi öğrenirsiniz
services: azure-app-configuration
documentationcenter: ''
author: abarora
manager: zhenlan
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: abarora
ms.openlocfilehash: afecc84748ae8ce85c07e3b482bd9b596bdca251
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75433672"
---
# <a name="tutorial-use-dynamic-configuration-in-a-net-core-app"></a>Öğretici: .NET Core uygulamasında dinamik yapılandırmayı kullanın

App Configuration .NET Core istemci kitaplığı, bir uygulamanın yeniden başlatılmasına neden olmadan isteğe bağlı yapılandırma ayarlarını güncelleştirmeyi destekler. Bu, önce yapılandırma sağlayıcısının seçeneklerinden `IConfigurationRefresher` bir örneğini alarak ve `Refresh` ardından kodunuzun herhangi bir yerinde bu örneği çağırarak uygulanabilir.

Ayarları güncel tutmak ve yapılandırma deposuna çok fazla çağrı yapılmasını önlemek için her ayar için bir önbellek kullanılır. Bir ayarın önbelleğe alınmış değeri dolana kadar, yapılandırma deposundaki değer değişse bile yenileme işlemi değeri güncelleştirmez. Her istek için varsayılan son kullanma süresi 30 saniyedir, ancak gerekirse geçersiz kılınabilir.

Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmelerini nasıl uygulayabileceğinizi gösterir. Hızlı başlangıçlarda tanıtılan uygulama üzerine inşa edilir. Devam etmeden önce, [önce App Configuration ile bir .NET Core uygulaması oluşturun'u](./quickstart-dotnet-core-app.md) tamamla.

Bu öğreticideki adımları yapmak için herhangi bir kod düzenleyicisini kullanabilirsiniz. [Visual Studio Code,](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarında kullanılabilen mükemmel bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir Uygulama Yapılandırma mağazasındaki değişikliklere yanıt olarak yapılandırmasını güncelleştirecek .NET Core uygulamanızı ayarlayın.
> * Uygulamanızdaki en son yapılandırmayı tüketin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyapmak için [,.NET Core SDK'yı](https://dotnet.microsoft.com/download)yükleyin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Uygulama Yapılandırmasından verileri yeniden yükleme

*Program.cs* açın ve `System.Threading.Tasks` ad alanına bir başvuru eklemek, `AddAzureAppConfiguration` yöntemde yapılandırmayı yenilemeyi belirtmek ve `Refresh` yöntemi kullanarak el ile yenilemeyi tetiklemek için dosyayı güncelleştirin.

```csharp
using System;
using System.Threading.Tasks;

namespace TestConsole
{
class Program
{
    private static IConfiguration _configuration = null;
    private static IConfigurationRefresher _refresher = null;

    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(options =>
        {
            options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                    .ConfigureRefresh(refresh =>
                    {
                        refresh.Register("TestApp:Settings:Message")
                               .SetCacheExpiration(TimeSpan.FromSeconds(10));
                    });
                    
                    _refresher = options.GetRefresher();
        });

        _configuration = builder.Build();
        PrintMessage().Wait();
    }

    private static async Task PrintMessage()
    {
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");

        // Wait for the user to press Enter
        Console.ReadLine();

        await _refresher.Refresh();
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");
    }
}
}
```

Yöntem, `ConfigureRefresh` bir yenileme işlemi tetiklendiğinde, yapılandırma verilerini Uygulama Yapılandırma deposuyla güncelleştirmek için kullanılan ayarları belirtmek için kullanılır. Bir `IConfigurationRefresher` örneği, yönteme `GetRefresher` `AddAzureAppConfiguration` sağlanan seçeneklerde arama yöntemiyle alınabilir `Refresh` ve bu örnekteki yöntem, kodunuzun herhangi bir yerinde bir yenileme işlemini tetiklemek için kullanılabilir.
    
> [!NOTE]
> Yapılandırma ayarı için varsayılan önbellek son kullanma süresi 30 saniyedir, `SetCacheExpiration` ancak `ConfigureRefresh` yönteme bağımsız değişken olarak geçirilen seçenekleri başlatma yöntemini arayarak geçersiz kılınabilir.

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. ConnectionString adında bir ortam **değişkeni**ayarlayın ve Uygulama Yapılandırma mağazanızın erişim anahtarına ayarlayın. Windows komut istemini kullanıyorsanız, değişikliğin etkili olmasını sağlamak için aşağıdaki komutu çalıştırın ve komut istemini yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    macOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

1. Konsol uygulamasını oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet build

1. Yapı başarıyla tamamlandıktan sonra, uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

    ![Quickstart uygulaması yerel başlatmak](./media/quickstarts/dotnet-core-app-run.png)

1. [Azure portalında](https://portal.azure.com)oturum açın. **Tüm kaynakları**seçin ve hızlı başlangıçta oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

1. **Configuration Explorer'ı**seçin ve aşağıdaki tuşların değerlerini güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler - Güncellendi |

1. Yenilemeyi tetiklemek için Enter tuşuna basın ve güncelleştirilmiş değeri Komut İstemi veya PowerShell penceresinde yazdırın.

    ![Quickstart uygulaması yerel yenileme](./media/quickstarts/dotnet-core-app-run-refresh.png)
    
    > [!NOTE]
    > Önbellek son kullanma süresi yenileme işlemi için `SetCacheExpiration` yapılandırma yı belirtirken yöntem kullanılarak 10 saniye olarak ayarlandığından, yapılandırma ayarı değeri yalnızca bu ayar için son yenilemeden bu yana en az 10 saniye geçtiyse güncelleştirilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, .NET Core uygulamanızın Uygulama Yapılandırması'ndan yapılandırma ayarlarını dinamik olarak yenilemesini sağladınız. Uygulama Yapılandırması'na erişimi kolaylaştırmak için Azure yönetilen bir kimliği nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
