---
title: .NET Core uygulamasında Azure Uygulama yapılandırması dinamik yapılandırmasını kullanma öğreticisi | Microsoft Docs
description: Bu öğreticide, .NET Core uygulamaları için yapılandırma verilerini dinamik olarak güncelleştirme hakkında bilgi edineceksiniz.
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
ms.openlocfilehash: ae753758a3cd5b7dfa8794ccf98f7a8a063f5b18
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185189"
---
# <a name="tutorial-use-dynamic-configuration-in-a-net-core-app"></a>Öğretici: .NET Core uygulamasında dinamik yapılandırma kullanma

Uygulama yapılandırması .NET Core istemci kitaplığı, bir uygulamanın yeniden başlatılmasına yol açmadan bir yapılandırma ayarları kümesinin güncelleştirilmesini destekler. Bu, önce yapılandırma sağlayıcısı seçeneklerinden bir `IConfigurationRefresher` örneği alınarak ve sonra bu örneğe kodunuzda herhangi bir yerde `Refresh` çağırarak uygulanabilir.

Ayarları güncel tutmak ve yapılandırma deposuna çok fazla çağrı önlemek için, her bir ayar için bir önbellek kullanılır. Bir ayarın önbelleğe alınmış değeri sona erene kadar, yenileme işlemi değeri, değer yapılandırma deposunda değiştiği zaman bile güncelleştirmez. Her istek için varsayılan kullanım süresi 30 saniyedir, ancak gerekirse geçersiz kılınabilir.

Bu öğreticide, kodunuzda dinamik yapılandırma güncelleştirmelerini nasıl uygulayabileceğinizi gösterir. Hızlı başlangıçlarda tanıtılan uygulamada oluşturulur. Devam etmeden önce, önce [uygulama yapılandırması ile bir .NET Core uygulaması oluşturun](./quickstart-dotnet-core-app.md) .

Bu öğreticideki adımları uygulamak için herhangi bir kod düzenleyicisi kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) , Windows, MacOS ve Linux platformlarında kullanılabilen harika bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir uygulama yapılandırma deposundaki değişikliklere yanıt olarak yapılandırmasını güncelleştirmek için .NET Core uygulamanızı ayarlayın.
> * Uygulamanızdaki en son yapılandırmayı tükettin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi yapmak için [.NET Core SDK](https://dotnet.microsoft.com/download)' yi yüklemelisiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Uygulama yapılandırmasından verileri yeniden yükleme

`System.Threading.Tasks` ad alanına bir başvuru eklemek, `AddAzureAppConfiguration` yönteminde yenileme yapılandırmasını belirtmek ve `Refresh` yöntemini kullanarak el ile yenilemeyi tetiklemek için *program.cs* açın ve dosyayı güncelleştirin.

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

`ConfigureRefresh` yöntemi, bir yenileme işlemi tetiklendiğinde, yapılandırma verilerini uygulama yapılandırma deposu ile güncelleştirmek için kullanılan ayarları belirtmek için kullanılır. `IConfigurationRefresher` örneği, `AddAzureAppConfiguration` yöntemine sunulan seçeneklere `GetRefresher` yöntemi çağırarak alınabilir ve bu örnekteki `Refresh` yöntemi kodunuzda herhangi bir yerde yenileme işlemini tetiklemek için kullanılabilir.
    
> [!NOTE]
> Yapılandırma ayarı için varsayılan önbellek süre sonu zamanı 30 saniyedir, ancak `SetCacheExpiration` yöntemi bir bağımsız değişken olarak geçirilen seçenekler başlatıcısında `ConfigureRefresh` yöntemine çağırarak geçersiz kılınabilir.

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. **ConnectionString**adlı bir ortam değişkeni ayarlayın ve uygulama yapılandırma deponuzu için erişim anahtarı olarak ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için komut istemi ' ni yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux kullanıyorsanız şu komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

1. Konsol uygulamasını derlemek için aşağıdaki komutu çalıştırın:

        dotnet build

1. Oluşturma başarıyla tamamlandıktan sonra, uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

    ![Hızlı başlangıç uygulaması başlatma yerel](./media/quickstarts/dotnet-core-app-run.png)

1. [Azure portalında](https://portal.azure.com) oturum açın. **Tüm kaynaklar**' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

1. **Yapılandırma Gezgini**' ni seçin ve aşağıdaki anahtarların değerlerini güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından alınan veriler-güncelleştirildi |

1. Yenileme tetiklemek ve komut Istemi veya PowerShell penceresinde güncelleştirilmiş değeri yazdırmak için ENTER tuşuna basın.

    ![Hızlı başlangıç uygulaması yerel yenileme](./media/quickstarts/dotnet-core-app-run-refresh.png)
    
    > [!NOTE]
    > Önbellek sona erme saati, yenileme işleminin yapılandırmasını belirtirken `SetCacheExpiration` yöntemi kullanılarak 10 saniye olarak ayarlandığı için, yapılandırma ayarı değeri yalnızca bu ayar için en son yenilemeden bu yana en az 10 saniye geçtikten sonra güncelleştirilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırmasından yapılandırma ayarlarını dinamik olarak yenilemek için .NET Core uygulamanızı etkinleştirdiniz. Azure yönetilen kimliğin uygulama yapılandırmasına erişimi kolaylaştırmak için nasıl kullanılacağını öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
