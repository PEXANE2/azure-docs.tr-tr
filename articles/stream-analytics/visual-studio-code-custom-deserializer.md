---
title: Visual Studio Code kullanarak Azure Stream Analytics bulut işleri için özel .NET seri hale Getiricileri oluşturun
description: Bu öğreticide, Visual Studio Code kullanarak Azure Stream Analytics bir bulut işi için özel bir .NET seri hale getirici oluşturma işlemi gösterilmektedir.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 12/22/2020
ms.openlocfilehash: 0f041910d1b02cc6d1fd48bfd773711c3361c0f5
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97822511"
---
# <a name="create-custom-net-deserializers-for-azure-stream-analytics-in-visual-studio-code"></a>Visual Studio Code Azure Stream Analytics için özel .NET seri hale Getiricileri oluşturma

Azure Stream Analytics [üç veri biçimi için yerleşik desteğe](stream-analytics-parsing-json.md)sahiptir: JSON, CSV ve avro. Özel .NET seri hale Getiricileri sayesinde [protokol arabelleği](https://developers.google.com/protocol-buffers/), [Bono](https://github.com/Microsoft/bond) ve diğer Kullanıcı tanımlı biçimler gibi diğer biçimlerden verileri de bulut işleri için okuyabilirsiniz.

## <a name="custom-net-deserializers-in-visual-studio-code"></a>Visual Studio Code 'de özel .NET seri hale getiriciler

Visual Studio Code kullanarak Azure Stream Analytics bir bulut işi için özel bir .NET seri hale getirici oluşturabilir, test edebilir ve hatalarını ayıklayabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* [.NET Core SDK 'sını](https://dotnet.microsoft.com/download) yükleyip Visual Studio Code yeniden başlatın.

* Visual Studio Code kullanarak Stream Analytics işi oluşturmayı öğrenmek için bu [hızlı](quick-create-visual-studio-code.md) başlangıcı kullanın.

### <a name="create-a-custom-deserializer"></a>Özel seri hale getirici oluşturma

1. Bir Terminal açın ve **Prototipbufdeserializer** adlı özel seri hale getirici için Visual Studio Code bir .NET sınıf kitaplığı oluşturmak üzere aşağıdaki komutu çalıştırın.

   ```dotnetcli
   dotnet new classlib -o ProtobufDeserializer
   ```

2. **Prototipbufdeserializer** proje dizinine gidin ve [Microsoft. Azure. StreamAnalytics](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics/) ve [Google. prototip,](https://www.nuget.org/packages/Google.Protobuf/) NuGet paketlerini yükler.

   ```dotnetcli
   dotnet add package Microsoft.Azure.StreamAnalytics
   ```

   ```dotnetcli
   dotnet add package Google.Protobuf
   ```

3. [Messagebodyproto sınıfını](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyProto.cs) ve [Messagebodydeserializer sınıfını](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyDeserializer.cs) projenize ekleyin.

4. **Prototipsifdeserializer** projesini oluşturun.

### <a name="add-an-azure-stream-analytics-project"></a>Azure Stream Analytics projesi ekleme

1. Visual Studio Code açın ve komut paletini açmak için **CTRL + SHIFT + P** ' yi seçin. Ardından ASA girin ve **asa: yeni proje oluştur**' u seçin. **Prototipbufcloudserializer** olarak adlandırın.

### <a name="configure-a-stream-analytics-job"></a>Stream Analytics işi yapılandırma

1. **ÜzerindeJobConfig.js** çift tıklayın. Aşağıdaki ayarlar dışında varsayılan konfigürasyonları kullanın:

   |Ayar|Önerilen Değer|
   |-------|---------------|
   |Genel depolama ayarları kaynağı|Geçerli hesaptaki veri kaynağını seçin|
   |Genel depolama ayarları aboneliği| Aboneliğinizi < >|
   |Genel depolama ayarları depolama hesabı| Depolama hesabınızı < >|
   |CustomCodeStorage ayarları depolama hesabı|Depolama hesabınızı < >|
   |CustomCodeStorage ayarları kapsayıcısı|Depolama kapsayıcınızı < >|

2. **Giriş** klasörü altında **input.js** açın. **Canlı giriş Ekle** ' yi seçin ve Azure Data Lake Storage 2./blob depolamadan bir giriş ekleyin, **Azure aboneliğinizden Seç**' i seçin. Aşağıdaki ayarlar dışında varsayılan konfigürasyonları kullanın:

   |Ayar|Önerilen Değer|
   |-------|---------------|
   |Ad|Giriş|
   |Abonelik|Aboneliğinizi < >|
   |Depolama Hesabı|Depolama hesabınızı < >|
   |Kapsayıcı|Depolama kapsayıcınızı < >|
   |Serileştirme türü|**Özel** ' i seçin|
   |SerializationProjectPath|CodeLens 'ten **Kütüphane proje yolu seç** ' i seçin ve önceki bölümde oluşturulan **prototipici** kitaplık projesini seçin. Projeyi derlemek için **derleme projesi** seçin|
   |SerializationClassName|Sınıf adını ve DLL yolunu otomatik olarak doldurmak için CodeLens 'ten **seri kaldırma sınıfını Seç** ' i seçin|
   |Sınıf Adı|MessageBodyProto. MessageBodyDeserializer|

   :::image type="content" source="./media/custom-deserializer/create-input-vscode.png" alt-text="Özel seri hale getirici girişi ekleyin.":::

3. Aşağıdaki sorguyu **Protobufclouddeserializer. aşama QL** dosyasına ekleyin.

   ```sql
   SELECT * FROM Input
   ```

4. [Örnek prototip giriş dosyasını](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/SimulatedTemperatureEvents.protobuf)indirin. **Girişler** klasöründe **input.jsüzerinde** sağ tıklayın ve **yerel giriş Ekle**' yi seçin. Ardından, **üzerindelocal_input1.js** çift tıklayın ve aşağıdaki ayarlar dışında varsayılan konfigürasyonları kullanın.

   |Ayar|Önerilen Değer|
   |-------|---------------|
   |Yerel dosya yolu seçin|İndirilen örnek prototipme giriş dosyasının dosya yolunu < seçmek için CodeLens ' e tıklayın>|

### <a name="execute-the-stream-analytics-job"></a>Stream Analytics işini yürütün

1. **Prototipbufclouddeserializer. aşama QL** ' i açın ve CodeLens 'Ten **yerel olarak çalıştır** ' ı seçin ve açılan listeden **Yerel girişi kullan** ' ı seçin

2. Sağ tarafta iş diyagramı görünümündeki **sonuçlar** sekmesindeki sonuçları gözlemleyin. Ara sonucu görüntülemek için iş diyagramındaki adımlara de tıklayabilirsiniz. Daha fazla ayrıntı için bkz. [iş diyagramını kullanarak yerel olarak hata ayıklama](debug-locally-using-job-diagram-vs-code.md).

   :::image type="content" source="./media/custom-deserializer/check-local-run-result-vscode.png" alt-text="Yerel çalıştırma sonucunu denetleyin.":::

Stream Analytics işiniz için özel bir seri hale getirici başarıyla uyguladık! Bu öğreticide, özel seri hale getirici 'yi yerel giriş verileriyle yerel olarak test edersiniz. Ayrıca [, bulutta canlı veri girişi kullanarak](visual-studio-code-local-run-live-input.md)da test edebilirsiniz. İşi bulutta çalıştırmak için girişi ve çıktıyı doğru şekilde yapılandırmanız gerekir. Ardından, yeni uyguladığınız özel seri hale getirici 'yi kullanarak işinizi bulutta çalıştırmak için Visual Studio Code 'den Azure 'a gönderebilirsiniz.

### <a name="debug-your-deserializer"></a>Seri hale getiricide hata ayıklama

.NET seri hale getiricinizi standart .NET kodunda hata ayıklamanıza benzer şekilde yerel olarak hata ayıklaması yapabilirsiniz. 

1. .NET işlevinizde kesme noktaları ekleyin.

2. Visual Studio Code etkinlik çubuğundan **Çalıştır** ' a tıklayın ve **dosyada launch.jsoluştur**' u seçin.
   :::image type="content" source="./media/custom-deserializer/create-launch-file-vscode.png" alt-text="Başlatma dosyası oluştur.":::

   **Prototipbufclouddeserializer** ' ı seçin ve ardından açılır listeden **Azure Stream Analytics** .
   :::image type="content" source="./media/custom-deserializer/create-launch-file-vscode-2.png" alt-text="Başlatma dosyası oluştur 2.":::

    <ASAScript> . Asaql öğesini Prototipbufclouddeserializer. aşama QL ile değiştirmek için dosyadakilaunch.jsdüzenleyin.
   :::image type="content" source="./media/custom-deserializer/configure-launch-file-vscode.png" alt-text="Başlatma dosyasını yapılandırın.":::

3. Hata ayıklamaya başlamak için **F5**'e basın. Program beklendiği gibi kesme noktalarında durur. Bu, hem yerel giriş hem de canlı giriş verileri için geçerlidir.

   :::image type="content" source="./media/custom-deserializer/debug-vscode.png" alt-text="Hata ayıklama özel seri hale getirici.":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu öğreticiyle oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, protokol arabelleği giriş serileştirme için özel bir .NET seri hale getiricisi nasıl uygulayacağınızı öğrendiniz. Özel seri hale getiriciler oluşturma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> * [Azure Stream Analytics işleri için farklı .NET seri hale Getiricileri oluşturma](custom-deserializer-examples.md)
> * [Visual Studio Code kullanarak canlı giriş ile Azure Stream Analytics işleri yerel olarak test etme](visual-studio-code-local-run-live-input.md)