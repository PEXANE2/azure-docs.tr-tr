---
title: Öğretici - Visual Studio'daki Azure Akış Analizi işleri için C# kullanıcı tanımlı işlevleri yazın (Önizleme)
description: Bu öğretici, Visual Studio'daki Stream Analytics işleri için c# kullanıcı tanımlı işlevlerinin nasıl yazılabildiğini gösterir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 1d71f4c5616efb05efe2733c49507b085ca2dcf6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75426290"
---
# <a name="tutorial-write-a-c-user-defined-function-for-azure-stream-analytics-job-preview"></a>Öğretici: Azure Akışı Analizi işi için C# kullanıcı tanımlı bir işlev yazın (Önizleme)

Visual Studio'da oluşturulan C# kullanıcı tanımlı işlevler (UDF), Azure Stream Analytics sorgu dilini kendi işlevlerinizi kullanarak genişletmenizi sağlar. C# ile var olan kodu (DLL'ler dahil) yeniden kullanabilir, matematiksel veya karmaşık mantıklardan faydalanabilirsiniz. UDF uygulamak için kullanabileceğiniz üç yöntem vardır: Stream Analytics projesinde CodeBehind dosyaları kullanma, yerel C# projesindeki UDF'leri alma veya bir depolama hesabındaki pakette bulunan UDF'leri alma. Bu öğreticide CodeBehind yöntemi kullanılarak basit bir C# işlevi uygulanmaktadır. Stream Analytics işleri için UDF özelliği şu anda önizlemededir ve üretim iş yüklerinde kullanılmamalıdır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * CodeBehind kullanarak C# ile kullanıcı tanımlı işlev oluşturma.
> * Stream Analytics işinizi yerel olarak test edin.
> * İşinizi Azure'da yayımlayın.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdaki önkoşulları tamamladığınızdan emin olun:

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.
* [Visual Studio için Stream Analytics araçlarını](stream-analytics-tools-for-visual-studio-install.md) ve **Azure geliştirme** veya **Veri Depolama ve İşleme** iş yüklerini yükleyin.
* Bir IoT Edge işi (akış-analitik-araçları-görsel-stüdyo-kenar-jobs.md için) oluşturuyorsanız, mevcut [Stream Analytics Edge geliştirme kılavuzuna) bir göz atın.

## <a name="create-a-container-in-your-azure-storage-account"></a>Azure Depolama Hesabınızda kapsayıcı oluşturma

Oluşturduğunuz kapsayıcı, derlenen C# paketini depolamak için kullanılacaktır. Bir Edge işi oluşturursanız, bu depolama hesabı paketi IoT Edge aygıtınıza dağıtmak için de kullanılır. Her Stream Analytics işi için ayrı bir kapsayıcı kullanın. Bir kapsayıcının birden fazla Stream Analytics Edge işi için kullanılması desteklenmez. Kapsayıcı bulunan bir depolama hesabınız varsa onu kullanabilirsiniz. Yoksa [yeni bir kapsayıcı oluşturmanız](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) gerekir. 

## <a name="create-a-stream-analytics-project-in-visual-studio"></a>Visual Studio'da Bir Stream Analytics projesi oluşturun

1. Visual Studio’yu çalıştırın.

2. **Dosya > Yeni > Proje**'yi seçin.

3. Soldaki şablonlar listesinde Akış **Analizi'ni**seçin ve ardından **Azure Akış Analizi Edge Uygulaması** veya Azure Akışı Analizi **Uygulamasını**seçin.

4.  Projenin **Ad**, **Konum** ve **Çözüm adı** değerlerini girip **Tamam**'ı seçin.

    ![Visual Studio’da Azure Stream Analytics Edge projesi oluşturma](./media/stream-analytics-edge-csharp-udf/stream-analytics-create-edge-app.png)

## <a name="configure-assembly-package-path"></a>Derleme paketi yolunu yapılandırma

1. Visual Studio'yu açın ve **Çözüm Gezgini**'ne gidin.

2. İş yapılandırma dosyasına (`EdgeJobConfig.json`) çift tıklayın.

3. **Kullanıcı Tanımlı Kod Yapılandırması** bölümünü genişletin ve yapılandırmaya aşağıdaki önerilen değerleri ekleyin:

   |**Ayar**|**Önerilen Değer**|
   |-------|---------------|
   |Genel Depolama Ayarları Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Genel Depolama Ayarları Aboneliği| Aboneliğinizi < >|
   |Genel Depolama Ayarları Depolama Hesabı| depolama hesabınızı < >|
   |Özel Kod Depolama Ayarları Kaynak|Geçerli hesaptaki veri kaynağını seçin|
   |Özel Kod Depolama Ayarları Depolama Hesabı|depolama hesabınızı < >|
   |Özel Kod Depolama Ayarları Kapsayıcısı|Depolama kapsayıcınızı < >|


## <a name="write-a-c-udf-with-codebehind"></a>CodeBehind ile C# UDF yazma
CodeBehind dosyası, tek bir ASA sorgu komut dosyasıyla ilişkili bir C# dosyasıdır. Gönderme işlemi sırasında Visual Studio araçları CodeBehind dosyasını otomatik olarak sıkıştırıp Azure depolama hesabınıza yükler. Tüm sınıfların *genel*, tüm nesnelerin de *statik genel* olarak tanımlanması gerekir.

1. **Çözüm Gezgini**'nde **Script.asql** dosyasını genişleterek **Script.asaql.cs** CodeBehind dosyasını bulun.

2. Kodu aşağıdaki örnekle değiştirin:

    ```csharp
        using System; 
        using System.Collections.Generic; 
        using System.IO; 
        using System.Linq; 
        using System.Text; 
    
        namespace ASAEdgeUDFDemo 
        { 
            public class Class1 
            { 
                // Public static function 
                public static Int64 SquareFunction(Int64 a) 
                { 
                    return a * a; 
                } 
            } 
        } 
    ```

## <a name="implement-the-udf"></a>UDF'yi uygulama

1. **Çözüm Gezgini**'nde **Script.asaql** dosyasını açın.

2. Var olan sorguyu aşağıdakiyle değiştirin:

    ```sql
        SELECT machine.temperature, udf.ASAEdgeUDFDemo_Class1_SquareFunction(try_cast(machine.temperature as bigint))
        INTO Output
        FROM Input 
    ```

## <a name="local-testing"></a>Yerel ortamda test etme

1. Sıcaklık [simülatörü örnek veri dosyasını](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Sample%20Data/TemperatureSampleData.json)indirin.

2. **Çözüm Gezgini**'nde **Girişler**'i genişletin, **Input.json** dosyasına sağ tıklayın ve **Yerel Giriş Ekle**'yi seçin.

   ![Visual Studio'daki Stream Analytics işine yerel giriş ekleme](./media/stream-analytics-edge-csharp-udf/stream-analytics-add-local-input.png)

3. İndirdiğiniz örnek verilerin yerel giriş dosyası yolunu belirtin ve **Kaydet**'i seçin.

    ![Visual Studio'da Stream Analytics işi için yerel giriş yapılandırması](./media/stream-analytics-edge-csharp-udf/stream-analytics-local-input-config.png)

4. Betik düzenleyicisinde **Yerel Olarak Çalıştır**'a tıklayın. Yerel çalıştırma çıkış sonuçlarını başarıyla kaydettikten sonra sonuçları tablo biçiminde görmek için herhangi bir tuşa basın. 

    ![Visual Studio'da Azure Stream Analytics işini yerel olarak çalıştırma](./media/stream-analytics-edge-csharp-udf/stream-analytics-run-locally.png)

5. İsterseniz **Sonuç Klasörünü Aç**'ı seçerek JSON ve CSV biçimindeki ham dosyaları da görüntüleyebilirsiniz.

    ![Visual Studio'da Azure Stream Analytics işinin sonuçlarını görüntüleme](./media/stream-analytics-edge-csharp-udf/stream-analytics-view-local-results.png)

## <a name="debug-a-udf"></a>UDF'de hata ayıklama
C# UDF hatalarını yerel ortamda standart C# kodunda olduğu gibi ayıklayabilirsiniz. 

1. C# işlevinize kesme noktaları ekleyin.

    ![Visual Studio'da Stream Analytics kullanıcı tanımlı işlevine kesme noktaları ekleme](./media/stream-analytics-edge-csharp-udf/stream-analytics-udf-breakpoints.png)

2. Hata ayıklamaya başlamak için **F5**'e basın. Program beklendiği gibi kesme noktalarında durur.

    ![Akış Analizi kullanıcı tanımlı işlev hata ayıklama sonuçlarını görüntüleyin](./media/stream-analytics-edge-csharp-udf/stream-analytics-udf-debug.png)

## <a name="publish-your-job-to-azure"></a>İşinizi Azure'da yayımlama
Sorgunuzu yerel ortamda test ettikten sonra işi Azure'da yayımlamak için betik düzenleyicisinde **Azure'a Gönder**'i seçin.

![Stream Analytics Edge işinizi Visual Studio'dan Azure'a gönderme](./media/stream-analytics-edge-csharp-udf/stream-analytics-udf-submit-job.png)

## <a name="deploy-to-iot-edge-devices"></a>IoT Edge'e cihazlarına dağıtma
Bir Stream Analytics Edge işi oluşturmayı seçtiyseniz, bu artık bir IoT Edge modülü olarak dağıtılabilir. [IoT Edge hızlı başlangıcını](https://docs.microsoft.com/azure/iot-edge/quickstart) izleyerek bir IoT Hub oluşturun, ona bir IoT Edge cihazını kaydedin ve cihazınızda IoT Edge çalışma zamanını çalıştırın. Ardından [işi dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics#deploy-the-job) öğreticisini izleyerek Stream Analytics işinizi IoT Edge modülü olarak dağıtın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, CodeBehind'u kullanarak basit bir C# kullanıcı tanımlı işlev oluşturdunuz, işinizi Azure'da yayınladınız ve işi Azure veya IoT Edge aygıtına dağıttınız. 

Stream Analytics işleri için C# kullanıcı tanımlı işlevleri kullanmanın farklı yolları hakkında daha fazla bilgi edinmek için şu makaleye devam edin:

> [!div class="nextstepaction"]
> [Azure Akış Analitiği için C# işlevleri yazın](stream-analytics-edge-csharp-udf-methods.md)
