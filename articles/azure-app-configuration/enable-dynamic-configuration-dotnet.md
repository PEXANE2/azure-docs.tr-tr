---
title: '.NET Framework Tutorial: Azure Uygulama Yapılandırmasında dinamik yapılandırma'
description: Bu eğitimde, Azure Uygulama Yapılandırması'nı kullanarak .NET Framework uygulamalarının yapılandırma verilerini dinamik olarak nasıl güncelleştirdiğinizi öğreneceksiniz.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/21/2019
ms.author: lcozzens
ms.openlocfilehash: 7780bdbc92868f62e8d066d171b2a04fe06a981d
ms.sourcegitcommit: 940e16ff194d5163f277f98d038833b1055a1a3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80245812"
---
# <a name="tutorial-use-dynamic-configuration-in-a-net-framework-app"></a>Öğretici: Bir .NET Framework uygulamasında dinamik yapılandırmayı kullanın

App Configuration .NET istemci kitaplığı, bir uygulamanın yeniden başlatılmasına neden olmadan isteğe bağlı yapılandırma ayarlarını güncelleştirmeyi destekler. Bu, önce yapılandırma sağlayıcısının seçeneklerinden `IConfigurationRefresher` bir örneğini alarak ve `Refresh` ardından kodunuzun herhangi bir yerinde bu örneği çağırarak uygulanabilir.

Ayarları güncel tutmak ve yapılandırma deposuna çok fazla çağrı yapılmasını önlemek için her ayar için bir önbellek kullanılır. Bir ayarın önbelleğe alınmış değeri dolana kadar, yapılandırma deposundaki değer değişse bile yenileme işlemi değeri güncelleştirmez. Her istek için varsayılan son kullanma süresi 30 saniyedir, ancak gerekirse geçersiz kılınabilir.

Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmelerini nasıl uygulayabileceğinizi gösterir. Hızlı başlangıçlarda tanıtılan uygulama üzerine inşa edilir. Devam etmeden önce, [önce App Configuration ile bir .NET Framework uygulaması oluşturun'u](./quickstart-dotnet-app.md) tamamla.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir Uygulama Yapılandırma mağazasındaki değişikliklere yanıt olarak yapılandırmasını güncelleştirecek .NET Framework uygulamanızı ayarlayın.
> * Uygulamanızdaki en son yapılandırmayı enjekte edin.
## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.1 veya sonrası](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar değeri çiftleri eklemek için **Configuration Explorer** > **+ Create** > **Key-value'ı** seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler |

    **Etiket** ve **İçerik Türünü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-net-framework-console-app"></a>Bir .NET Framework konsol uygulaması oluşturma

1. Visual Studio'yı başlatın ve **Dosya** > **Yeni** > **Projesi'ni**seçin.

1. **Yeni bir proje oluştur'da**Konsol proje türüne filtre uygulayın ve Console **Console** **App'a (.NET Framework)** tıklayın. **İleri**'ye tıklayın.

1. **Yeni projenizi yapılandırın,** bir proje adı girin. **Çerçeve**altında **,NET Framework 4.7.1** veya üzerini seçin. **Oluştur'u**tıklatın.

## <a name="reload-data-from-app-configuration"></a>Uygulama Yapılandırmasından verileri yeniden yükleme
1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. **Gözat** sekmesinde, *Microsoft.Extensions.Configuration.AzureAppConfiguration* NuGet paketini projenize arayın ve ekleyin. Bulamıyorsanız, **Yayın Öncesi Ekle** onay kutusunu seçin.

1. *Program.cs*açın ve .NET Core App Configuration sağlayıcısına bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Yapılandırmayla ilgili nesneleri depolamak için iki değişken ekleyin.

    ```csharp
    private static IConfiguration _configuration = null;
    private static IConfigurationRefresher _refresher = null;
    ```

1. Belirtilen `Main` yenileme seçenekleriyle Uygulama Yapılandırması'na bağlanmak için yöntemi güncelleştirin.

    ```csharp
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
    ```
    Yöntem, `ConfigureRefresh` bir yenileme işlemi tetiklendiğinde, yapılandırma verilerini Uygulama Yapılandırma deposuyla güncelleştirmek için kullanılan ayarları belirtmek için kullanılır. Bir `IConfigurationRefresher` örneği, yönteme `GetRefresher` `AddAzureAppConfiguration` sağlanan seçeneklerde arama yöntemiyle alınabilir `Refresh` ve bu örnekteki yöntem, kodunuzun herhangi bir yerinde bir yenileme işlemini tetiklemek için kullanılabilir.

    > [!NOTE]
    > Yapılandırma ayarı için varsayılan önbellek son kullanma süresi 30 saniyedir, `SetCacheExpiration` ancak `ConfigureRefresh` yönteme bağımsız değişken olarak geçirilen seçenekleri başlatma yöntemini arayarak geçersiz kılınabilir.

1. Uygulama Yapılandırması'ndan yapılandırma verilerinin el ile yenilenmesini tetikleyen bir `PrintMessage()` yöntem ekleyin.

    ```csharp
    private static async Task PrintMessage()
    {
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");

        // Wait for the user to press Enter
        Console.ReadLine();

        await _refresher.Refresh();
        Console.WriteLine(_configuration["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. ConnectionString adında bir ortam **değişkeni**ayarlayın ve Uygulama Yapılandırma mağazanızın erişim anahtarına ayarlayın. Windows komut istemini kullanıyorsanız, değişikliğin etkili olmasını sağlamak için aşağıdaki komutu çalıştırın ve komut istemini yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

1. Değişikliğin etkili olmasını sağlamak için Visual Studio'yu yeniden başlatın. 

1. Konsol uygulamasını oluşturmak ve çalıştırmak için Ctrl + F5 tuşuna basın.

    ![Uygulama lansmanı yerel](./media/dotnet-app-run.png)

1. [Azure portalında](https://portal.azure.com)oturum açın. **Tüm kaynakları**seçin ve hızlı başlangıçta oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

1. **Configuration Explorer'ı**seçin ve aşağıdaki tuşların değerlerini güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler - Güncellendi |

1. Çalışan uygulamada, yenilemeyi tetiklemek için Enter tuşuna basın ve güncelleştirilmiş değeri Komut İstemi veya PowerShell penceresinde yazdırın.

    ![Uygulama yenileme yerel](./media/dotnet-app-run-refresh.png)
    
    > [!NOTE]
    > Önbellek son kullanma süresi yenileme işlemi için `SetCacheExpiration` yapılandırma yı belirtirken yöntem kullanılarak 10 saniye olarak ayarlandığından, yapılandırma ayarı değeri yalnızca bu ayar için son yenilemeden bu yana en az 10 saniye geçtiyse güncelleştirilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, .NET Framework uygulamanızın Uygulama Yapılandırması'ndan yapılandırma ayarlarını dinamik olarak yenilemesini sağladınız. Uygulama Yapılandırması'na erişimi kolaylaştırmak için Azure yönetilen bir kimliği nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
