---
title: Sürekli tümleştirme ve teslim ardışık hatlar kullanarak Azure Uygulama Yapılandırması'nı tümleştirin
description: Azure Uygulama Yapılandırmasını kullanarak sürekli tümleştirme ve teslimat uygulamayı öğrenin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 01/30/2020
ms.author: lcozzens
ms.openlocfilehash: c744557471a9b37bd620bb9195bdb709c24649ab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77047280"
---
# <a name="integrate-with-a-cicd-pipeline"></a>CI/CD işlem hattıyla tümleştirme

Bu makalede, Azure Uygulama Yapılandırması'ndan gelen verilerin sürekli tümleştirme ve sürekli dağıtım sisteminde nasıl kullanılacağı açıklanmaktadır.

## <a name="use-app-configuration-in-your-azure-devops-pipeline"></a>Azure DevOps Ardışık Alan Nızda Uygulama Yapılandırması'nı kullanma

Azure DevOps Ardışık Alan'A sahipseniz, temel değerleri Uygulama Yapılandırmasından alabilir ve bunları görev değişkenleri olarak ayarlayabilirsiniz. [Azure Uygulama Yapılandırma Sıtkı DevOps uzantısı,](https://go.microsoft.com/fwlink/?linkid=2091063) bu işlevselliği sağlayan bir eklenti modülüdür. Uzantıyı bir yapı veya sürüm görev sırasına göre kullanmak için yönergeleriizleyin.

## <a name="deploy-app-configuration-data-with-your-application"></a>Uygulamanızla Uygulama Yapılandırma verilerini dağıtma

Uygulamanız Azure Uygulama Yapılandırmasına bağlıysa ve bu uygulamaya ulaşamıyorsa çalışmayabilir. Yapılandırma verilerini uygulamayla birlikte dağıtılan ve uygulama nın başlatılması sırasında yerel olarak yüklenen bir dosyaya paketleyerek uygulamanızın esnekliğini artırın. Bu yaklaşım, uygulamanızın başlangıç ta varsayılan ayar değerlerine sahip olduğunu garanti eder. Bu değerler, kullanılabilir olduğunda Bir Uygulama Yapılandırma deposundaki yeni değişiklikler tarafından üzerine yazılır.

Azure Uygulama Yapılandırmasının [Dışa Aktarma](./howto-import-export-data.md#export-data) işlevini kullanarak, geçerli yapılandırma verilerini tek bir dosya olarak alma işlemini otomatikleştirebilirsiniz. Daha sonra bu dosyayı sürekli tümleştirme ve sürekli dağıtım (CI/CD) ardışık alanınızda bir yapı veya dağıtım adımına gömebilirsiniz.

Aşağıdaki örnek, hızlı başlangıçlarda tanıtılan web uygulaması için bir yapı adımı olarak Uygulama Yapılandırma verilerinin nasıl dahil edilebildiğini gösterir. Devam etmeden önce, [önce App Configuration ile ASP.NET Core uygulaması oluşturun'u](./quickstart-aspnet-core-app.md) tamamla.

Bu öğreticideki adımları yapmak için herhangi bir kod düzenleyicisini kullanabilirsiniz. [Visual Studio Code,](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarında mükemmel bir seçenektir.

### <a name="prerequisites"></a>Ön koşullar

Yerel olarak oluşturmanız durumunda, azure [CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) indirin ve yükleyin.

Örneğin Azure DevOps ile bulut oluşturma yapmak için, yapı sisteminizde [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yüklü olduğundan emin olun.

### <a name="export-an-app-configuration-store"></a>Uygulama Yapılandırma deposuna Dışa Aktarma

1. *.csproj* dosyanızı açın ve aşağıdaki komut dosyasını ekleyin:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -d file --path $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```
1. *Program.cs*açın ve `CreateWebHostBuilder` yöntemi arayarak dışa aktarılan JSON dosyasını kullanma yöntemini güncelleştirin. `config.AddJsonFile()`  `System.Reflection` Ad alanını da ekleyin.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var directory = System.IO.Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
                var settings = config.Build();

                config.AddJsonFile(Path.Combine(directory, "azureappconfig.json"));
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

### <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. ConnectionString adında bir ortam **değişkeni**ayarlayın ve Uygulama Yapılandırma mağazanızın erişim anahtarına ayarlayın. 
    Windows komut istemini kullanıyorsanız, değişikliğin etkili olmasını sağlamak için aşağıdaki komutu çalıştırın ve komut istemini yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    macOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. .NET Core CLI kullanarak uygulamayı oluşturmak için komut kabuğunda aşağıdaki komutu çalıştırın:

        dotnet build

3. Yapı başarıyla tamamlandıktan sonra, web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

4. Bir tarayıcı penceresi açın `http://localhost:5000`ve yerel olarak barındırılan web uygulamasının varsayılan URL'si olan adrese gidin.

    ![Quickstart uygulaması yerel başlatmak](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, dağıtım ardışık bir işlem de kullanılmak üzere Azure Uygulama Yapılandırması verilerini dışa aktarmış sınız. Uygulama Yapılandırmasını nasıl kullanacağınız hakkında daha fazla bilgi edinmek için Azure CLI örneklerine devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
