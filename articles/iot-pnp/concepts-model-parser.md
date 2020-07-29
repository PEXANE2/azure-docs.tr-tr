---
title: Dijital TWINS modeli ayrıştırıcısı 'nı anlayın | Microsoft Docs
description: Geliştirici olarak, modelleri doğrulamak için DTDL ayrıştırıcısının nasıl kullanılacağını öğrenin
author: rido-min
ms.author: rmpablos
ms.date: 04/29/2020
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: peterpr
ms.openlocfilehash: 20c4452a32c791f33e08c883d8cec89a345ab188
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352431"
---
# <a name="understand-the-digital-twins-model-parser"></a>Dijital TWINS modeli ayrıştırıcısını anlama

Dijital TWINS tanım dili (DTDL) [Dtdl belirtiminde](https://github.com/Azure/opendigitaltwins-dtdl)açıklanmıştır. Kullanıcılar, birden çok dosyada tanımlanan bir modeli doğrulamak ve sorgulamak için _dijital TWINS modeli ayrıştırıcısı_ NuGet paketini kullanabilir.

## <a name="install-the-dtdl-model-parser"></a>DTDL model ayrıştırıcısı 'nı yükle

Ayrıştırıcının KIMLIĞI: [Microsoft. Azure. DigitalTwins. Parser](https://www.nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser)ile NuGet.org ' de kullanılabilir. Ayrıştırıcıyı yüklemek için, Visual Studio veya CLI 'de olduğu gibi uyumlu herhangi bir NuGet paket yöneticisini kullanın `dotnet` .

```bash
dotnet add package Microsoft.Azure.DigitalTwins.Parser
```

## <a name="use-the-parser-to-validate-a-model"></a>Bir modeli doğrulamak için ayrıştırıcısı kullanma

Doğrulamak istediğiniz model, JSON dosyalarında açıklanan bir veya daha fazla arabirimden oluşabilir. Ayrıştırıcıyı, belirli bir klasördeki tüm dosyaları yüklemek ve bu dosya arasındaki tüm başvuruları dahil tüm dosyaları bir bütün olarak doğrulamak üzere kullanmak için kullanabilirsiniz:

1. `IEnumerable<string>`Tüm model içeriklerinin listesini içeren bir oluştur:

    ```csharp
    using System.IO;

    string folder = @"c:\myModels\";
    string filespec = "*.json";

    List<string> modelJson = new List<string>();
    foreach (string filename in Directory.GetFiles(folder, filespec))
    {
        using StreamReader modelReader = new StreamReader(filename);
        modelJson.Add(modelReader.ReadToEnd());
    }
    ```

1. `ModelParser`Ve çağrısını oluşturun `ParseAsync` :

    ```csharp
    using Microsoft.Azure.DigitalTwins.Parser;

    ModelParser modelParser = new ModelParser();
    IReadOnlyDictionary<Dtmi, DTEntityInfo> parseResult = await modelParser.ParseAsync(modelJson);
    ```

1. Doğrulama hatalarını kontrol edin. Ayrıştırıcı herhangi bir hata bulursa, `AggregateException` ayrıntılı hata iletilerinin listesini içeren bir oluşturur:

    ```csharp
    try
    {
        IReadOnlyDictionary<Dtmi, DTEntityInfo> parseResult = await modelParser.ParseAsync(modelJson);
    }
    catch (AggregateException ae)
    {
        foreach (var e in ae.InnerExceptions)
        {
            Console.WriteLine(e.Message);
        }
    }
    ```

1. ' Nı inceleyin `Model` . Doğrulama başarılı olursa modeli incelemek için model ayrıştırıcısı API 'sini kullanabilirsiniz. Aşağıdaki kod parçacığı, ayrıştırılabilen tüm modellerin nasıl yineleneceğini gösterir ve var olan özellikleri görüntüler:

    ```csharp
    foreach (var m in parseResult)
    {
        Console.WriteLine(m.Key);
        foreach (var item in m.Value.AsEnumerable<DTEntityInfo>())
        {
            var p = item as DTInterfaceInfo;
            if (p!=null)
            {
                Console.WriteLine($"\t{p.Id}");
                Console.WriteLine($"\t{p.Description.FirstOrDefault()}");
            }
            Console.WriteLine("--------------");
        }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede gözden geçirilen model ayrıştırıcısı API 'SI, DTDL modellerine bağlı olan görevleri otomatik hale getirmek veya doğrulamak için birçok senaryo sağlar. Örneğin, modeldeki bilgilerden dinamik olarak bir kullanıcı arabirimi oluşturabilirsiniz.
