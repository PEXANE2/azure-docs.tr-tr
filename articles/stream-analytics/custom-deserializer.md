---
title: Öğretici - Azure Akış Analizi bulut işleri için Özel .NET deserializers
description: Bu öğretici, Visual Studio'u kullanarak Azure Akış Analizi bulut işi için özel bir .NET deserializer nasıl oluşturulacak larını gösterir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: 1fffeec1434cb066487bf383589554edec2e6a86
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75443688"
---
# <a name="tutorial-custom-net-deserializers-for-azure-stream-analytics"></a>Öğretici: Azure Akış Analizi için Özel .NET deserializers

Azure Akış Analizi, JSON, CSV ve Avro [olmak üzere üç veri biçimi için yerleşik destek](stream-analytics-parsing-json.md)ekidir. Özel .NET deserializers ile, [Protokol Arabelle,](https://developers.google.com/protocol-buffers/) [Bond](https://github.com/Microsoft/bond) ve diğer kullanıcı tanımlı biçimleri hem bulut hem de kenar işleri için diğer biçimleri verileri okuyabilirsiniz.

Bu öğretici, Visual Studio'u kullanarak Azure Akış Analizi bulut işi için özel bir .NET deserializer nasıl oluşturulacak larını gösterir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Protokol arabelleği için özel bir deserializer oluşturun.
> * Visual Studio'da bir Azure Akışı Analizi işi oluşturun.
> * Özel deserializer kullanmak için Stream Analytics iş yapılandırın.
> * Özel deserializer'ı test etmek için Stream Analytics işinizi yerel olarak çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) veya Visual [Studio 2015'i](https://www.visualstudio.com/vs/older-downloads/)yükleyin. Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez.

* [Visual Studio için Stream Analytics araçlarını yükleyin](stream-analytics-tools-for-visual-studio-install.md) veya en son sürüme güncelleştirin. Visual Studio'nun aşağıdaki sürümleri desteklenir:
   * Visual Studio 2015
   * Visual Studio 2017

* Visual Studio'da **Bulut Gezgini'ni** açın ve Azure aboneliğinizde oturum açın.

* Azure Depolama Hesabınızda bir kapsayıcı oluşturun.
Oluşturduğunuz kapsayıcı, Stream Analytics işinizle ilgili varlıkları depolamak için kullanılır. Kapsayıcı bulunan bir depolama hesabınız varsa onu kullanabilirsiniz. Yoksa [yeni bir kapsayıcı oluşturmanız](../storage/blobs/storage-quickstart-blobs-portal.md) gerekir.

## <a name="create-a-custom-deserializer"></a>Özel bir deserializer oluşturma

1. Visual Studio'u açın ve **Dosya > Yeni > Projesi'ni**seçin. Akış **Analizi'ni** arayın ve **Azure Akış Analizi Özel Deserializer Project'i (.NET)** seçin. **Protobuf Deserializer**gibi projeye bir isim verin.

   ![Visual Studio dotnet standart sınıf kitaplık projesi oluşturma](./media/custom-deserializer/create-dotnet-library-project.png)

2. Solution Explorer'da **Protobuf Deserializer** projenize sağ tıklayın ve menüden **NuGet Paketlerini Yönet'i** seçin. Ardından **Microsoft.Azure.StreamAnalytics** ve **Google.Protobuf** NuGet paketlerini yükleyin.

3. [MessageBodyProto sınıfını](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyProto.cs) ve [MessageBodyDeserializer sınıfını](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyDeserializer.cs) projenize ekleyin.

4. **Protobuf Deserializer** projesini oluşturun.

## <a name="add-an-azure-stream-analytics-project"></a>Azure Akışı Analizi projesi ekleme

1. Çözüm Gezgini'nde **Protobuf Deserializer** çözümünü sağ tıklatın ve **Yeni Proje ekle >** seçin. **Azure Akış Analizi > Akış Analizi**altında Azure Akış Analizi **Uygulamasını**seçin. Adını **ProtobufCloudDeserializer** ve **Tamam**seçin. 

2. **ProtobufCloudDeserializer** Azure Akış Analizi projesi kapsamında **Başvurular'a** sağ tıklayın. **Projeler**altında, **Protobuf Deserializer**ekleyin. Otomatik olarak sizin için doldurulmalıdır.

## <a name="configure-a-stream-analytics-job"></a>Akış Analizi işini yapılandırma

1. Çift tıklayın **JobConfig.json**. Aşağıdaki ayarlar dışında varsayılan yapılandırmaları kullanın:

   |Ayar|Önerilen Değer|
   |-------|---------------|
   |Genel Depolama Ayarları Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Genel Depolama Ayarları Aboneliği| Aboneliğinizi < >|
   |Genel Depolama Ayarları Depolama Hesabı| depolama hesabınızı < >|
   |Özel Kod Depolama Ayarları Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Özel Kod Depolama Ayarları Depolama Hesabı|depolama hesabınızı < >|
   |Özel Kod Depolama Ayarları Kapsayıcısı|Depolama kapsayıcınızı < >|

2. **Girişler**altında, **input.json'a**çift tıklayın. Aşağıdaki ayarlar dışında varsayılan yapılandırmaları kullanın:

   |Ayar|Önerilen Değer|
   |-------|---------------|
   |Kaynak|Blob Depolama|
   |Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Abonelik|Aboneliğinizi < >|
   |Depolama Hesabı|depolama hesabınızı < >|
   |Kapsayıcı|Depolama kapsayıcınızı < >|
   |Olay Serileştirme Biçimi|Diğer (Protobuf, XML, tescilli...)|
   |Kaynak|ASA Proje Referansı veya CodeBehind'dan Yük|
   |CSharp Montaj Adı|ProtobufDeserializer.dll|
   |Sınıf Adı|MessageBodyProto.MessageBodyDeserializer|
   |Olay Sıkıştırma Türü|None|

3. **Script.asaql** dosyasına aşağıdaki sorguyu ekleyin.

   ```sql
   SELECT * FROM Input
   ```

4. Örnek [protobuf giriş dosyasını](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/SimulatedTemperatureEvents.protobuf)indirin. **Girişler** klasöründe, **Input.json'a** sağ tıklayın ve **Yerel Giriş Ekle'yi**seçin. Ardından, **local_Input.json'a** çift tıklayın ve aşağıdaki ayarları yapılandırın:

   |Ayar|Önerilen Değer|
   |-------|---------------|
   |Girdi Diğer Adı|Giriş|
   |Kaynak Türü|Veri Akışı|
   |Olay Serileştirme Biçimi|Diğer (Protobuf, XML, tescilli...)|
   |CSharp Montaj Adı|ProtobufDeserializer.dll|
   |Sınıf Adı|MessageBodyProto.MessageBodyDeserializer|
   |Yerel Giriş Dosya Yolu|< indirilen örnek protobuf giriş dosyası için dosya yolu>|

## <a name="execute-the-stream-analytics-job"></a>Akış Analizi işini yürütün

1. **Script.asaql'ı** açın ve **Yerel Olarak Çalıştır'ı**seçin.

2. **Stream Analytics Yerel Çalıştırma Sonuçları'nda**sonuçları gözlemleyin.

Stream Analytics işiniz için özel bir deserializer'ı başarıyla uyguladınız! Bu öğreticide, özel deserializer yerel olarak test edilmiştir. Gerçek verileriniz için, giriş ve çıktıyı düzgün bir şekilde yapılandırabilirsiniz. Ardından, az önce uyguladığınız özel deserializer'i kullanarak işinizi bulutta çalıştırmak için İşinizi Visual Studio'dan Azure'a gönderin.

## <a name="debug-your-deserializer"></a>Deserializer'ınızı hataayıklama

.NET deserializer'ınızı, standart .NET kodunu hata ayıklamak gibi yerel olarak hata ayıklayabilirsiniz. 

1. İşlevinize kesme noktaları ekleyin.

2. Hata ayıklamaya başlamak için **F5**'e basın. Program beklendiği gibi kesme noktalarında durur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak bu öğreticiyle oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'nı ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubu sayfanızda, **Sil**'i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, protokol arabelleği giriş serileştirmesi için özel bir .NET deserializer nasıl uygulanacağını öğrendiniz. Özel deserializers oluşturma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye devam edin:

> [!div class="nextstepaction"]
> [Azure Akış Analizi işleri için farklı .NET deserializers oluşturun](custom-deserializer-examples.md)
