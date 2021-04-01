---
title: .NET ' te oto ml ONNX modeliyle tahminlere sahip olun
description: ML.NET ile .NET 'teki bir oto ml ONNX modelini kullanarak tahmine dayalı hale getirme hakkında bilgi edinin
titleSuffix: Azure Machine Learning
author: luisquintanilla
ms.author: luquinta
ms.date: 10/30/2020
ms.topic: conceptual
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.custom: how-to, automl
ms.openlocfilehash: 4fb147dc5c57c3a98607a025f566fa583bf87460
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "93358822"
---
# <a name="make-predictions-with-an-automl-onnx-model-in-net"></a>.NET 'teki bir oto ml ONNX modeliyle tahminlere sahip olun

Bu makalede, ML.NET ile bir C# .NET Core konsol uygulamasında tahmine dayalı hale getirmek için otomatik ML (Otomatikml) Open sinir Network Exchange (ONNX) modelini nasıl kullanacağınızı öğreneceksiniz.

[Ml.net](/dotnet/machine-learning/) , .net ekosistemi için bir açık kaynaklı, platformlar arası, makine öğrenimi çerçevesidir ve C# ya da F # ' ta bir kod-birinci yaklaşım ve [Model Oluşturucu](/dotnet/machine-learning/automate-training-with-model-builder) ve [ml.net CLI](/dotnet/machine-learning/automate-training-with-cli)gibi düşük kod araçları aracılığıyla özel makine öğrenimi modellerini tükemenize ve kullanmanızı sağlar. Çerçeve ayrıca Genişletilebilir ve TensorFlow ve ONNX gibi diğer popüler makine öğrenimi çerçevelerinden yararlanmanızı sağlar.

ONNX, AI modelleri için açık kaynaklı bir biçimdir. ONNX çerçeveler arasında birlikte çalışabilirliği destekler. Bu, bir modeli PyTorch gibi birçok popüler makine öğrenimi çerçevelerinden birinde eğitebileceğiniz, ONNX biçimine dönüştürebileceğiniz ve ML.NET gibi farklı bir çerçevede ONNX modelini tükettiğiniz anlamına gelir. Daha fazla bilgi edinmek için [Onnx Web sitesini](https://onnx.ai/)ziyaret edin.

## <a name="prerequisites"></a>Önkoşullar

- [.NET Core SDK 3,1 veya üzeri](https://dotnet.microsoft.com/download)
- Metin düzenleyici veya IDE ( [Visual Studio](https://visualstudio.microsoft.com/vs/) veya [Visual Studio Code](https://code.visualstudio.com/Download)gibi)
- ONNX modeli. Bir oto ml ONNX modelini eğitme hakkında bilgi edinmek için, aşağıdaki [Banka pazarlama sınıflandırması not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)bakın.
- [Netron](https://github.com/lutzroeder/netron) (isteğe bağlı)

## <a name="create-a-c-console-application"></a>C# konsol uygulaması oluşturma

Bu örnekte, uygulamanızı oluşturmak için .NET Core CLI kullanırsınız ancak Visual Studio 'Yu kullanarak aynı görevleri gerçekleştirebilirsiniz. [.NET Core CLI](/dotnet/core/tools/)hakkında daha fazla bilgi edinin.

1. Bir Terminal açın ve yeni bir C# .NET Core konsol uygulaması oluşturun. Bu örnekte, uygulamanın adı `AutoMLONNXConsoleApp` . Uygulamanızın içeriğiyle aynı ada sahip bir dizin oluşturulur.

    ```dotnetcli
    dotnet new console -o AutoMLONNXConsoleApp
    ```

1. Terminalde, *oto Mlonnxconsoleapp* dizinine gidin.

    ```bash
    cd AutoMLONNXConsoleApp
    ```

## <a name="add-software-packages"></a>Yazılım paketleri Ekle

1. .NET Core CLI kullanarak **Microsoft.ml**, **Microsoft. ml. OnnxRuntime** ve **Microsoft. ml. onnxtransformer** NuGet paketlerini yükler.

    ```dotnetcli
    dotnet add package Microsoft.ML
    dotnet add package Microsoft.ML.OnnxRuntime
    dotnet add package Microsoft.ML.OnnxTransformer
    ```

    Bu paketler bir .NET uygulamasında ONNX modelini kullanmak için gereken bağımlılıkları içerir. ML.NET, tahminler için [Onnx çalışma zamanını](https://github.com/Microsoft/onnxruntime) kullanan bir API sağlar.

1. *Program. cs* dosyasını açın ve `using` ilgili paketlere başvurmak için en üst kısımdaki aşağıdaki deyimleri ekleyin.

    ```csharp
    using System.Linq;
    using Microsoft.ML;
    using Microsoft.ML.Data;
    using Microsoft.ML.Transforms.Onnx;
    ```

## <a name="add-a-reference-to-the-onnx-model"></a>ONNX modeline bir başvuru ekleyin

Konsol uygulamasının ONNX modeline erişmesi için bir yol derleme çıkış dizinine eklemektir.  MSBuild ortak öğeleri hakkında daha fazla bilgi için bkz. [MSBuild Kılavuzu](/visualstudio/msbuild/common-msbuild-project-items).

Uygulamanızdaki ONNX model dosyanıza bir başvuru ekleyin

1. ONNX modelinizi uygulamanızın *oto Klonnxconsoleapp* kök dizinine kopyalayın.
1. *Oto Mlonnxconsoleapp. csproj* dosyasını açın ve düğümü içine aşağıdaki içeriği ekleyin `Project` .

    ```xml
    <ItemGroup>
        <None Include="automl-model.onnx">
            <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
        </None>
    </ItemGroup>
    ```

    Bu durumda, ONNX model dosyasının adı, *oto ml-model. onnx*' dir.

1. *Program. cs* dosyasını açın ve sınıfının içine aşağıdaki satırı ekleyin `Program` .

    ```csharp
    static string ONNX_MODEL_PATH = "automl-model.onnx";
    ```

## <a name="initialize-mlcontext"></a>MLContext 'i Başlat

`Main`Sınıfınızın yöntemi içinde `Program` Yeni bir örneğini oluşturun [`MLContext`](xref:Microsoft.ML.MLContext) .

```csharp
MLContext mlContext = new MLContext();
```

[`MLContext`](xref:Microsoft.ML.MLContext)Sınıfı tüm ml.NET işlemleri için bir başlangıç noktasıdır ve başlatma, `mlContext` model yaşam döngüsü genelinde paylaşılabilen yeni bir ml.net ortamı oluşturur. Bu, kavramsal olarak, Entity Framework içinde DbContext 'e benzer.

## <a name="define-the-model-data-schema"></a>Model veri şemasını tanımlama

Modeliniz, giriş ve çıkış verilerinizi belirli bir biçimde bekliyor. ML.NET, veri biçimlerini sınıflar aracılığıyla tanımlamanızı sağlar. Bazen bu biçimin nasıl göründüğünü zaten bilirsiniz. Veri biçimini bilmiyorsanız, ONNX modelinizi incelemek için netron gibi araçları kullanabilirsiniz.

Bu örnekte kullanılan model, NYC TLC TAXI seyahat veri kümesindeki verileri kullanır. Verilerin bir örneği aşağıda görülebilir:

|vendor_id|rate_code|passenger_count|trip_time_in_secs|trip_distance|payment_type|fare_amount|
|---|---|---|---|---|---|---|
|VTS|1|1|1140|3,75|CRD|15.5|
|VTS|1|1|480|2,72|CRD|10.0|
|VTS|1|1|1680|7.8|CSH|26.5|

### <a name="inspect-the-onnx-model-optional"></a>ONNX modelini İnceleme (isteğe bağlı)

Modelinizin giriş ve çıktılarını incelemek için netron gibi bir araç kullanın.

1. Netron 'i açın.
1. Üst menü çubuğunda **dosya > aç** ' ı seçin ve modelinizi seçmek için dosya tarayıcısını kullanın.
1. Modeliniz açılır. Örneğin, *oto ml-model. onnx* modelinin yapısı aşağıdaki gibi görünür:

    :::image type="content" source="media/how-to-use-automl-onnx-model-dotnet/netron-automl-onnx-model.png" alt-text="Netron oto ml ONNX modeli":::

1. Modelin meta verilerini göstermek için grafiğin alt kısmındaki son düğümü ( `variable_out1` Bu örnekte) seçin. Kenar çubuğundaki girişler ve çıkışlar, modelin beklenen giriş, çıkış ve veri türlerini gösterir. Modelinizin giriş ve çıkış şemasını tanımlamak için bu bilgileri kullanın.

### <a name="define-model-input-schema"></a>Model giriş şemasını tanımla

`OnnxInput` *Program. cs* dosyası içinde aşağıdaki özelliklerle adlı yeni bir sınıf oluşturun.

```csharp
public class OnnxInput
{
    [ColumnName("vendor_id")]
    public string VendorId { get; set; }

    [ColumnName("rate_code"),OnnxMapType(typeof(Int64),typeof(Single))]
    public Int64 RateCode { get; set; }

    [ColumnName("passenger_count"), OnnxMapType(typeof(Int64), typeof(Single))]
    public Int64 PassengerCount { get; set; }

    [ColumnName("trip_time_in_secs"), OnnxMapType(typeof(Int64), typeof(Single))]
    public Int64 TripTimeInSecs { get; set; }

    [ColumnName("trip_distance")]
    public float TripDistance { get; set; }

    [ColumnName("payment_type")]
    public string PaymentType { get; set; }
}
```

Özelliklerin her biri veri kümesindeki bir sütunla eşlenir. Özellikler, özniteliklerle Daha fazla açıklama eklenir.

[`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute)Özniteliği, ml.NET veri üzerinde çalışırken sütuna nasıl başvurulacağını belirlemenizi sağlar. Örneğin, `TripDistance` özelliği standart .net adlandırma kurallarına uygun olsa da, model yalnızca olarak bilinen bir sütunu veya özelliği bilir `trip_distance` . Bu adlandırma tutarsızlığı için, öznitelik, [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) `TripDistance` özelliği adıyla bir sütuna veya özelliğe eşler `trip_distance` .
  
Sayısal değerler için, ML.NET yalnızca [`Single`](xref:System.Single) değer türlerinde çalışır. Ancak, bazı sütunların özgün veri türü tamsayılardır. [`OnnxMapType`](xref:Microsoft.ML.Transforms.Onnx.OnnxMapTypeAttribute)Öznitelik, türü ONNX ve ml.net arasında eşlenir.

Veri öznitelikleri hakkında daha fazla bilgi için bkz. [ml.net Load Data Guide](/dotnet/machine-learning/how-to-guides/load-data-ml-net).

### <a name="define-model-output-schema"></a>Model çıkış şemasını tanımla

Veriler işlendikten sonra, belirli bir biçimin çıkışını üretir. Veri çıktı şemanızı tanımlayın. `OnnxOutput` *Program. cs* dosyası içinde aşağıdaki özelliklerle adlı yeni bir sınıf oluşturun.

```csharp
public class OnnxOutput
{
    [ColumnName("variable_out1")]
    public float[] PredictedFare { get; set; }
}
```

Benzer şekilde `OnnxInput` , [`ColumnName`](xref:Microsoft.ML.Data.ColumnNameAttribute) `variable_out1` çıktıyı daha açıklayıcı bir ada eşlemek için özniteliğini kullanın `PredictedFare` .

## <a name="define-a-prediction-pipeline"></a>Tahmin işlem hattı tanımlama

ML.NET içindeki bir işlem hattı genellikle çıkış oluşturmak için giriş verilerinde çalışan bir dizi zincir dönüşümlerdir. Veri dönüştürmeleri hakkında daha fazla bilgi için bkz. [ml.NET Data Transformation Guide](/dotnet/machine-learning/resources/transforms).

1. Sınıf içinde çağrılan yeni bir yöntem oluştur `GetPredictionPipeline` `Program`

    ```csharp
    static ITransformer GetPredictionPipeline(MLContext mlContext)
    {

    }
    ```

1. Giriş ve çıkış sütunlarının adını tanımlayın. Aşağıdaki kodu yönteminin içine ekleyin `GetPredictionPipeline` .

    ```csharp
    var inputColumns = new string []
    {
        "vendor_id", "rate_code", "passenger_count", "trip_time_in_secs", "trip_distance", "payment_type"
    };

    var outputColumns = new string [] { "variable_out1" };
    ```

1. İşlem hattınızı tanımlayın. , [`IEstimator`](xref:Microsoft.ML.IEstimator%601) İşlem hattınızdaki işlemler, giriş ve çıkış şemaları şeması sağlar.

    ```csharp
    var onnxPredictionPipeline =
        mlContext
            .Transforms
            .ApplyOnnxModel(
                outputColumnNames: outputColumns,
                inputColumnNames: inputColumns,
                ONNX_MODEL_PATH);
    ```

    Bu durumda, işlem hattındaki [`ApplyOnnxModel`](xref:Microsoft.ML.OnnxCatalog.ApplyOnnxModel%2A) tek dönüşüm, giriş ve çıkış sütunlarının adlarını ve ONNX model dosyasının yolunu alır.

1. [`IEstimator`](xref:Microsoft.ML.IEstimator%601)Yalnızca verilerinize uygulanacak işlem kümesini tanımlar. Verilerinize ne kadar işlem denir [`ITransformer`](xref:Microsoft.ML.ITransformer) . ' `Fit` Den bir tane oluşturmak için yöntemini kullanın `onnxPredictionPipeline` .

    ```csharp
    var emptyDv = mlContext.Data.LoadFromEnumerable(new OnnxInput[] {});

    return onnxPredictionPipeline.Fit(emptyDv);
    ```

    [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit%2A)Yöntemi, [`IDataView`](xref:Microsoft.ML.IDataView) üzerinde işlemleri gerçekleştirmek için bir as girişi bekler. [`IDataView`](xref:Microsoft.ML.IDataView), Ml.NET içindeki verileri tablolu biçim kullanarak temsil etmenin bir yoludur. Bu durumda, işlem hattı yalnızca tahminlerde kullanıldığında, [`IDataView`](xref:Microsoft.ML.IDataView) [`ITransformer`](xref:Microsoft.ML.ITransformer) gerekli giriş ve çıkış şeması bilgilerini sağlamak için boş değer sağlayabilirsiniz. Daha sonra, bu, [`ITransformer`](xref:Microsoft.ML.ITransformer) uygulamanızda daha fazla kullanım için döndürülür.

    > [!TIP]
    > Bu örnekte, işlem hattı tanımlanır ve aynı uygulama içinde kullanılır. Ancak, tahminleri yapmak için işlem hattınızı tanımlamak ve kullanmak üzere ayrı uygulamalar kullanmanız önerilir. ML.NET ' de, işlem hatlarınız diğer .NET Son Kullanıcı uygulamalarında daha fazla kullanım için sıralanabilir ve kaydedilebilir. ML.NET, masaüstü uygulamaları, Web Hizmetleri, WebAssembly uygulamaları * ve çok daha fazlası gibi çeşitli dağıtım hedeflerini destekler. İşlem hatlarını kaydetme hakkında daha fazla bilgi için, bkz. [ml.net Save and Load eğitilen modeller Kılavuzu](/dotnet/machine-learning/how-to-guides/save-load-machine-learning-models-ml-net).
    >
    > * WebAssembly yalnızca .NET Core 5 veya daha sonraki bir sürümünde destekleniyor

1. Yöntemi içinde `Main` , `GetPredictionPipeline` Gerekli parametrelerle yöntemi çağırın.

    ```csharp
    var onnxPredictionPipeline = GetPredictionPipeline(mlContext);
    ```

## <a name="use-the-model-to-make-predictions"></a>Tahminleri yapmak için modeli kullanın

Artık bir işlem hattına sahip olduğunuza göre, tahminleri tahmin etmek için kullanmanın zamanı vardır. ML.NET, adlı tek bir veri örneğinde tahmin yapmak için kullanışlı bir API sağlar [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) .

1. Yöntemi içinde `Main` yöntemini kullanarak bir oluşturun [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) [`CreatePredictionEngine`](xref:Microsoft.ML.ModelOperationsCatalog.CreatePredictionEngine%2A) .

    ```csharp
    var onnxPredictionEngine = mlContext.Model.CreatePredictionEngine<OnnxInput, OnnxOutput>(onnxPredictionPipeline);
    ```

1. Sınama verisi girişi oluşturun.

    ```csharp
    var testInput = new OnnxInput
    {
        VendorId = "CMT",
        RateCode = 1,
        PassengerCount = 1,
        TripTimeInSecs = 1271,
        TripDistance = 3.8f,
        PaymentType = "CRD"
    };
    ```

1. `predictionEngine`Yöntemini kullanarak yeni verilere göre tahmine dayalı hale getirmek için kullanın `testInput` [`Predict`](xref:Microsoft.ML.PredictionEngineBase%602.Predict%2A) .

    ```csharp
    var prediction = onnxPredictionEngine.Predict(testInput);
    ```

1. Tahmine ilişkin sonucu konsola çıkış.

    ```csharp
    Console.WriteLine($"Predicted Fare: {prediction.PredictedFare.First()}");
    ```

1. Uygulamanızı çalıştırmak için .NET Core CLI kullanın.

    ```dotnetcli
    dotnet run
    ```

    Sonuç aşağıdaki çıktıya benzer şekilde görünmelidir:

    ```text
    Predicted Fare: 15.621523
    ```

ML.NET ' de tahminleri yapma hakkında daha fazla bilgi edinmek için bkz. [bir model kullanarak](/dotnet/machine-learning/how-to-guides/machine-learning-model-predictions-ml-net)tahmine dayalı hale getirme Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

- [Modelinizi ASP.NET Core Web API 'SI olarak dağıtma](/dotnet/machine-learning/how-to-guides/serve-model-web-api-ml-net)
- [Modelinizi sunucusuz .NET Azure Işlevi olarak dağıtın](/dotnet/machine-learning/how-to-guides/serve-model-serverless-azure-functions-ml-net)