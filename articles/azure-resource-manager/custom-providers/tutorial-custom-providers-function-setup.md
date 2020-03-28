---
title: Azure İşlevleri’ni ayarlama
description: Bu öğretici, bir Azure işlev uygulamasının nasıl oluşturulup Azure Özel Sağlayıcılarıyla çalışacak şekilde ayarlanın
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: b6984bfdf7ec078d02999bccf77f3592cfbcab27
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75649991"
---
# <a name="set-up-azure-functions-for-azure-custom-providers"></a>Azure Özel Sağlayıcıları için Azure İşlevlerini Ayarlama

Özel sağlayıcı, Azure ile bitiş noktası arasındaki bir sözleşmedir. Özel sağlayıcılarla Azure'da iş akışlarını değiştirebilirsiniz. Bu öğretici, özel bir sağlayıcı bitiş noktası olarak çalışacak bir Azure işlev uygulamasının nasıl ayarlanabildiğini gösterir.

## <a name="create-the-azure-function-app"></a>Azure işlev uygulamasını oluşturma

> [!NOTE]
> Bu öğreticide, bir Azure işlev uygulaması kullanan basit bir hizmet bitiş noktası oluşturursunuz. Ancak, özel bir sağlayıcı genel olarak erişilebilen herhangi bir bitiş noktasını kullanabilir. Alternatifler arasında Azure Mantık Uygulamaları, Azure API Yönetimi ve Azure Uygulama Hizmeti'nin Web Uygulamaları özelliği yer almaktadır.

Bu öğreticiyi başlatmak için öncelikle Azure [portalındaki ilk Azure işlev uygulamanızı oluşturun öğreticisini](../../azure-functions/functions-create-first-azure-function.md)takip etmelisiniz. Bu öğretici, Azure portalında değiştirilebilen bir .NET çekirdek webhook işlevi oluşturur. Aynı zamanda mevcut öğretici için temelidir.

## <a name="install-azure-table-storage-bindings"></a>Azure Tablo depolama bağlamalarını yükleme

Azure Tablo depolama bağlamalarını yüklemek için:

1. HttpTrigger için **Tümleştir** sekmesine gidin.
1. Seçin **+ Yeni Giriş**.
1. **Azure Tablo Depolama'yı**seçin.
1. Microsoft.Azure.WebJobs.Extensions.Storage uzantısını zaten yüklü değilse yükleyin.
1. Tablo **parametre adı** kutusuna **tabloDepolama'yı**girin.
1. Tablo **adı** kutusuna, **myCustomResources'ı**girin.
1. Güncelleştirilmiş giriş parametresini kaydetmek için **Kaydet'i** seçin.

![Tablo bağlamalarını gösteren özel sağlayıcıya genel bakış](./media/create-custom-provider/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>RESTful HTTP yöntemlerini güncelleştirin

Azure işlevini özel sağlayıcı RESTful istek yöntemlerini içerecek şekilde ayarlamak için:

1. HttpTrigger için **Tümleştir** sekmesine gidin.
1. **Seçili HTTP yöntemleri**altında GET **,** **POST**, **DELETE**ve **PUT**' u seçin.

![HTTP yöntemlerini gösteren özel sağlayıcıya genel bakış](./media/create-custom-provider/azure-functions-http-methods.png)

## <a name="add-azure-resource-manager-nuget-packages"></a>Azure Kaynak Yöneticisi NuGet paketleri ekleme

> [!NOTE]
> C# proje dosyanız proje dizininde eksikse, el ile ekleyebilirsiniz. Veya işlev uygulamasında Microsoft.Azure.WebJobs.Extensions.Storage uzantısı yüklendikten sonra görünür.

Ardından, C# proje dosyasını yararlı NuGet kitaplıklarını içerecek şekilde güncelleştirin. Bu kitaplıklar, özel sağlayıcılardan gelen istekleri ayrışdırmayı kolaylaştırır. [Portaldan uzantı eklemek](../../azure-functions/install-update-binding-extensions-manual.md) ve C# proje dosyasını aşağıdaki paket başvurularını içerecek şekilde güncelleştirmek için adımları izleyin:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

Aşağıdaki XML öğesi örnek bir C# proje dosyasıdır:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure özel sağlayıcısı bitiş noktası olarak çalışacak bir Azure işlev uygulaması ayarlarsınız.

ReSTful özel sağlayıcı bitiş noktası nasıl yazılanın öğrenebilirsiniz, [bkz.](./tutorial-custom-providers-function-authoring.md)

