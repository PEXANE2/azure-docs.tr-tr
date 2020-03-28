---
title: Azure IoT Hub ileti yönlendirme sonuçlarını görüntüleyin (.NET) | Microsoft Dokümanlar
description: Öğreticinin Bölüm 1'ini kullanarak tüm kaynakları ayarladıktan sonra, iletileri Azure Akış Analizi'ne yönlendirme ve PowerBI'de sonuçları görüntüleme olanağı nı ekleyin.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: bfee4e64070e5f37eaa3d63280409f00c0ed8672
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73890403"
---
# <a name="tutorial-part-2---view-the-routed-messages"></a>Öğretici: Bölüm 2 - Yönlendirilen iletileri görüntüleyin

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="rules-for-routing-the-messages"></a>İletileri yönlendirme kuralları

İleti yönlendirme kuralları şunlardır; Bu bölüm 1 bu öğretici kuruldu ve onları bu ikinci bölümde çalışmak bakın.

|Değer |Sonuç|
|------|------|
|level="storage" |Azure Depolama'ya yazın.|
|level="critical" |Service Bus kuyruğuna yazın. Mantıksal Uygulama iletiyi kuyruktan alır ve Office 365 kullanarak iletiyi e-postayla gönderir.|
|default |Power BI'ı kullanarak bu verileri görüntüleyin.|

Artık iletilerin yönlendirileceği kaynakları oluşturursunuz, hub'a ileti göndermek için bir uygulama çalıştırın ve yönlendirmeyi iş başında görebilirsiniz.

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma  

Service Bus kuyruğu kritik olarak belirlenmiş iletileri almak için kullanılacaktır. Service Bus kuyruğunu izlemek ve kuyruğa ileti eklendiğinde bir e-posta göndermek için bir Mantıksal uygulama ayarlayın.

1. Azure [portalında](https://portal.azure.com), **+ Kaynak oluştur'u**seçin. Arama kutusuna **mantıksal uygulama** yazın ve Enter tuşuna basın. Görüntülenen arama sonuçlarından Mantık Uygulaması'nı seçin ve ardından Mantık Oluştur uygulaması bölmesine devam etmek için **Oluştur'u** seçin. **Create** Alanları doldurun.

   **Ad**: bu alan, mantıksal uygulamanın adıdır. Bu öğreticide **ContosoLogicApp** kullanılır.

   **Abonelik**: Azure aboneliğinizi seçin.

   **Kaynak grubu**: **Varolan Kullan'ı** seçin ve kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

   **Konum**: Konumunuzu kullanın. Bu öğreticide **Batı ABD** kullanılır.

   **Log Analytics**: Bu iki durumlu düğme kapalı konuma getirilmelidir.

   ![Mantık Uygulama Oluştur ekranı](./media/tutorial-routing-view-message-routing-results/create-logic-app.png)

   **Oluştur'u**seçin. Uygulamanın dağıtılması birkaç dakika sürebilir.

2. Şimdi Mantıksal Uygulama'ya gidin. Mantık Uygulamasına gitmenin en kolay yolu **Kaynak gruplarını**seçmek, kaynak grubunuzu seçmektir (bu öğretici **ContosoResources**kullanır), ardından kaynak listesinden Mantık Uygulamasını seçin. 

    Logic Apps Tasarımcısı sayfası gösterilir (sayfanın tamamını görmek için ekranı sağa doğru kaydırmanız gerekebilir). Logic Apps Designer sayfasında, **Boş Mantık Uygulaması +** yazan döşemeyi görene kadar aşağı kaydırın ve seçin. Varsayılan sekme "Sizin İçin"dir. Bu bölme boşsa, kullanılabilir tüm bağlayıcıları ve tetikleyicileri görmek için **Tümü'nü** seçin.

3. Bağlayıcılar listesinden **Servis Veri Servisi'ni** seçin.

   ![Bağlayıcılar listesi](./media/tutorial-routing-view-message-routing-results/logic-app-connectors.png)

4. Tetikleyici listesi görüntülenir. **Bir ileti sırada (otomatik tamamlama) / Servis Veri Gönderi'nde ne zaman alındığını**seçin.

   ![Servis Veri Günü tetikleyicileri listesi](./media/tutorial-routing-view-message-routing-results/logic-app-triggers.png)

5. Sonraki ekranda, Bağlantı Adı alanını doldurun. Bu öğreticide **ContosoConnection** kullanılır.

   ![Servis Veri Servisi kuyruğu için bağlantı ayarlama](./media/tutorial-routing-view-message-routing-results/logic-app-define-connection.png)

   Hizmet Veri Günü ad alanını seçin. Bu öğreticide **ContosoSBNamespace** kullanılır. Ad alanını seçtiğinizde, portal anahtarları almak için Service Bus ad alanını sorgular. **RootManageSharedAccessKey'i** seçin ve **Oluştur'u**seçin.

   ![Bağlantıyı ayarlamayı bitirme](./media/tutorial-routing-view-message-routing-results/logic-app-finish-connection.png)

6. Sonraki ekranda, açılan listeden kuyruğun adını seçin (bu öğreticide **contososbqueue** kullanılır). Kalan alanlar için varsayılan değerleri kullanabilirsiniz.

   ![Sıra seçenekleri](./media/tutorial-routing-view-message-routing-results/logic-app-queue-options.png)

7. Şimdi kuyrukta bir ileti alındığında e-posta gönderme eylemini ayarlayın. Logic Apps Designer'da, bir adım eklemek için **+ Yeni adım'ı** seçin ve ardından kullanılabilir tüm seçenekleri görmek için **Tümü'nü** seçin. Eylem **bölmesinde** **Office 365 Outlook'u**bulun ve seçin. Eylemler ekranında, **e-posta gönder / Office 365 Outlook'u**seçin.  

   ![Office365 seçenekleri](./media/tutorial-routing-view-message-routing-results/logic-app-select-outlook.png)

8. Bağlantıyı kurmak için Office 365 hesabınızda oturum açın. Eğer bu zaman dolursa, sadece tekrar deneyin. E-postaların alıcıları için e-posta adreslerini belirtin. Ayrıca konuyu da belirtin ve alıcının gövdede görmesini istediğiniz iletiyi yazın. Test amacıyla, alıcı olarak kendi e-posta adresinizi girin.

   Ekle seçeneğini belirleyin, ekleyebileceğiniz iletideki içeriği göstermek için **dinamik içerik ekleyin'i** seçin. **İçerik** öğesini seçin; e-postadaki iletiyi içerecektir.

   ![Mantık uygulaması için e-posta seçenekleri](./media/tutorial-routing-view-message-routing-results/logic-app-send-email.png)

9. **Kaydet'i**seçin. Sonra Logic App Tasarımcısı'nı kapatın.

## <a name="set-up-azure-stream-analytics"></a>Azure Stream Analytics'i ayarlama

Verileri Power BI görselleştirmesinde görmek için, önce bir Stream Analytics işi ayarlayarak verileri alın. Yalnızca **düzeyi****normal** olan iletilerin varsayılan uç noktaya gönderildiğini ve Power BI görselleştirmesi için Stream Analytics işi tarafından alınacağını unutmayın.

### <a name="create-the-stream-analytics-job"></a>Stream Analytics işi oluşturma

1. Azure [portalında,](https://portal.azure.com) **Create a resource** > **Bir** > kaynak Nesnelerin İnterneti**Oluşturma Akış Analizi işini**seçin.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir. Bu öğreticide **contosoJob** kullanılır.

   **Abonelik**: Öğretici için kullandığınız Azure aboneliği.

   **Kaynak grubu**: IoT hub'ınız tarafından kullanılan kaynak grubunun aynısını kullanın. Bu öğreticide **ContosoResources** kullanılır.

   **Konum**: Kurulum betiğinde kullanılan konumun aynısını kullanın. Bu öğreticide **Batı ABD** kullanılır.

   ![Akış analizi işini oluşturun](./media/tutorial-routing-view-message-routing-results/stream-analytics-create-job.png)

3. İşi oluşturmak için **Oluştur'u** seçin. Dağıtmak birkaç dakika sürebilir.

    İşe geri dönmek için **Kaynak gruplarını**seçin. Bu öğreticide **ContosoResources** kullanılır. Kaynak grubunu seçin ve ardından kaynaklar listesindeki Akış Analizi işini seçin.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. **İş Topolojisi** **altında, Girişleri**seçin.

2. **Girişler** bölmesinde akış **girişi ekle'yi** seçin ve IoT Hub'ını seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Giriş diğer adı**: Bu öğreticide **contosoinputs** kullanılır.

   **Aboneliğinizden IoT Hub'ı seçin:** Bu radyo düğmesi seçeneğini seçin.

   **Abonelik**: Bu öğretici için kullandığınız Azure aboneliğini seçin.

   **IoT Hub : IoT**merkezini seçin. Bu öğreticide **ContosoTestHub** kullanılır.

   **Uç nokta**: **Mesajlaşma**'yı seçin. (İşlem İzleme'yi seçerseniz, gönderdiğiniz veriler yerine IoT hub'ı hakkındaki telemetri verilerini alırsınız.) 

   **Paylaşılan erişim ilkesi adı**: **Hizmet**seçin. Paylaşılan Erişim İlkesi Anahtarı'nı portal sizin için doldurur.

   **Tüketici grubu**: Bu öğreticinin Bölüm 1'de ayarlanan tüketici grubunu seçin. Bu öğreticide **contosoconsumers** kullanılır.
   
   Kalan alanlar için varsayılan değerleri kabul edin. 

   ![Akış analizi işi için girdileri ayarlama](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-inputs.png)

3. **Kaydet'i**seçin.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**altında **Çıktıları**seçin.

2. **Çıktılar** bölmesinde **Ekle'yi**ve ardından **Power BI'yi**seçin. Görüntülenen ekranda aşağıdaki alanları doldurun:

   **Çıkış diğer adı**: Çıkışın benzersiz diğer adı. Bu öğreticide **contosooutputs** kullanılır. 

   **Veri kümesi adı**: Power BI'da kullanılacak veri kümesinin adı. Bu öğreticide **contosodataset** kullanılır. 

   **Tablo adı**: Power BI'da kullanılacak tablonun adı. Bu öğreticide **contosotable** kullanılır.

   Kalan alanlarda varsayılan değerleri kabul edin.

3. **Yetkilendirme'yi**seçin ve Power BI hesabınızda oturum açın. (Bu birden fazla deneme alabilir).

   ![Akış analizi işi için çıktıları ayarlama](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-outputs.png)

4. **Kaydet'i**seçin.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'yu seçin.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin. Bu öğreticide **contosoinputs** kullanılır.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin. Bu öğreticide **contosooutputs** kullanılır.

   ![Akış analizi işi için sorguyu ayarlama](./media/tutorial-routing-view-message-routing-results/stream-analytics-job-query.png)

4. **Kaydet'i**seçin.

5. Sorgu bölmesini kapatın. Kaynak Grubu'ndaki kaynakların görünümüne geri dönersiniz. Akış Analizi işini seçin. Bu öğreticide **contosoJob** olarak adlandırılmıştır.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Akış Analizi işinde, > **Şimdi** > **Başlat'ı**seçin. **Start** İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

Power BI raporunu ayarlamak için verilere ihtiyacınız vardır; bu nedenle, cihazı oluşturup cihaz simülasyon uygulamasını çalıştırdıktan sonra Power BI'ı ayarlarsınız.

## <a name="run-simulated-device-app"></a>Simüle edilmiş cihaz uygulamasını çalıştırma

Bu öğreticinin Bölüm 1'inde, bir IoT aygıtı nı kullanarak simüle etmek için bir aygıt ayarlarsınız. Bu bölümde, aygıtın bir IoT hub'ına cihazdan buluta ileti göndermesini simüle eden .NET konsol uygulamasını indirebilirsiniz (Uygulamayı ve kaynakları Bölüm 1'de zaten indirmediğinizi varsayarsak).

Bu uygulama, farklı ileti yönlendirme yöntemlerinin her biri için iletigönderir. İndirmede Azure Kaynak Yöneticisi şablonu ve parametreler dosyasının yanı sıra Azure CLI ve PowerShell komut dosyalarını içeren bir klasör de vardır.

Bu öğretici Bölüm 1'deki depodan dosyaları indirmediyseniz, devam edin ve [şimdi IoT Aygıt](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)Simülasyonu'ndan indirin. Bu bağlantıyı seçmek, içinde çeşitli uygulamalar bulunan bir depo indirir; aradığınız çözüm iot-hub/Tutorials/Routing/IoT_SimulatedDevice.sln. 

Visual Studio'da kodu açmak için çözüm dosyasına (IoT_SimulatedDevice.sln) çift tıklayın ve Program.cs açın. `{your hub name}` değerini IoT hub'ı konak adıyla değiştirin. IoT hub'ı konak adı **{iot-hub-adı}.azure-devices.net** biçimindedid. bu öğreticide, hub konak adı olarak **ContosoTestHub.azure-devices.net** kullanılır. Ardından, `{your device key}` değerini daha önce simülasyon cihazını ayarlarken kaydettiğiniz cihaz anahtarıyla değiştirin. 

   ```csharp
        static string s_myDeviceId = "Contoso-Test-Device";
        static string s_iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string s_deviceKey = "{your device key}";
   ```

## <a name="run-and-test"></a>Çalıştırma ve test etme

Konsol uygulamasını çalıştırın. Birkaç dakika bekleyin. Uygulamanın konsol ekranında iletilerin gönderildiğini görebilirsiniz.

Uygulama, IoT hub'ına her saniye yeni bir cihazdan buluta iletisi gönderir. İleti, cihaz kimliği, sıcaklık, nem düzeyi ve ileti düzeyi (varsayılan `normal` değeriyle) bilgileriyle bir JSON seri hale getirilmiş nesnesi içerir. Buna rastgele olarak `critical` veya `storage` düzeyi atanır; bu, iletinin depolama hesabına veya Service Bus kuyruğuna (Mantıksal Uygulamanızı e-posta göndermesi için tetikler) yönlendirilmesine neden olur. Varsayılan (`normal`) okumalar, bundan sonra ayarlayacağınız BI raporunda görüntülenir.

Her şey düzgün ayarlandıysa, bu noktada aşağıdaki sonuçları görüyor olmalısınız:

1. Kritik iletilerle ilgili e-postalar almaya başlarsınız.

   ![Ortaya çıkan e-postalar](./media/tutorial-routing-view-message-routing-results/results-in-email.png)

   Bu sonuç, aşağıdaki ifadelerin doğru olduğu anlamına gelir. 

   * Service Bus kuyruğunda yönlendirme düzgün çalışıyor.
   * İletiyi Service Bus kuyruğundan alan Mantıksal Uygulama düzgün çalışıyor.
   * Mantıksal Uygulama'nın Outlook bağlayıcısı düzgün çalışıyor. 

2. Azure [portalında](https://portal.azure.com) **Kaynak gruplarını** seçin ve Kaynak Grubu'nuzu seçin. Bu öğreticide **ContosoResources** kullanılır. 

    Depolama hesabını seçin, **Kapsayıcılar'ı**seçin, ardından Kapsayıcı'yı seçin. Bu öğreticide **contosoresults** kullanılır. Bir klasör görüyor olmalısınız ve bir veya birden çok dosya görünceye kadar dizinlerde ayrıntıya gidebilirsiniz. Bu dosyalardan birini açın; depolama hesabına yönlendirilen girdileri içerir. 

   ![Depolama daki sonuç dosyaları](./media/tutorial-routing-view-message-routing-results/results-in-storage.png)

Bu sonuç, aşağıdaki ifadenin doğru olduğu anlamına gelir.

   * Depolama hesabına yapılan yönlendirme düzgün çalışıyor.

Şimdi, uygulama hala çalışıyor, varsayılan yönlendirme yoluyla gelen iletileri görmek için Power BI görselleştirme ayarlayın.

## <a name="set-up-the-power-bi-visualizations"></a>Power BI görselleştirmelerini ayarlama

1. [Power BI](https://powerbi.microsoft.com/) hesabınızda oturum açın.

2. **Çalışma Alanları**'na gidin ve Stream Analytics işi için çıkış oluştururken ayarladığını çalışma alanını seçin. Bu öğreticide **My Workspace** kullanılır. 

3. **Veri Kümeleri'ni**seçin. Veri kümeniz yoksa, birkaç dakika bekleyin ve tekrar kontrol edin.

   Stream Analytics işi için çıkış oluştururken belirttiğiniz veri kümesinin listelendiğini görüyor olmalısınız. Bu öğreticide **contosodataset** kullanılır. (Veri kümesinin ilk kez görüntülenmesi 5-10 dakika kadar sürebilir.)

4. **EYLEMLER**altında, rapor oluşturmak için ilk simgeyi seçin.

   ![Eylemler ve rapor simgesi vurgulanan Power BI çalışma alanı](./media/tutorial-routing-view-message-routing-results/power-bi-actions.png)

5. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   * Rapor oluşturma sayfasında, satır grafiği simgesini seçerek bir satır grafiği ekleyin.

     ![Görselleştirmeler ve alanlar](./media/tutorial-routing-view-message-routing-results/power-bi-visualizations-and-fields.png)

   * **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin. Bu öğreticide **contosotable** kullanılır.

   * **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.

   * **temperature** öğesini **Değerler** üzerine sürükleyin.

   Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

6. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. İkinci grafiği ayarlamak için, ilk grafik için aynı işlemi izleyin ve X eksenine **EventEnqueuedUtcTime'ı** ve y eksenine**Axis****(Değerler)** **nem** yerleştirin.

   ![İki grafikli son Power BI raporu](./media/tutorial-routing-view-message-routing-results/power-bi-report.png)

7. İstenirse rapor için bir ad girerek raporu kaydetmek için **Kaydet'i** seçin.

Her iki grafikteki verileri de görüyor olmalısınız. Bu sonuç, aşağıdaki ifadelerin doğru olduğu anlamına gelir:

   * Varsayılan uç noktaya yapılan yönlendirme düzgün çalışıyor.
   * Azure Stream Analytics işi doğru akış yapıyor.
   * Power BI Görselleştirmesi doğru ayarlanmış.

Power BI penceresinin üst kısmındaki Yenile düğmesini seçerek en son verileri görmek için grafikleri yenileyebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu öğreticinin her iki bölümü aracılığıyla oluşturduğunuz Azure kaynaklarının tümünü kaldırmak istiyorsanız, kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda IoT hub'ını, Service Bus ad alanıyla kuyruğunu, Mantıksal Uygulamayı, depolama hesabını ve kaynak grubunun kendisini kaldırır. Ayrıca Power BI kaynaklarını kaldırabilir ve eğitim sırasında gönderilen e-postaları temizleyebilirsiniz.

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Power BI görselleştirmesinde kaynakları temizleme

[Power BI](https://powerbi.microsoft.com/) hesabınızda oturum açın. Çalışma alanınıza gidin. Bu öğreticide **My Workspace** kullanılır. Power BI görselleştirmesini kaldırmak için DataSets'e gidin ve veri kümesini silmek için çöp kutusu simgesini seçin. Bu öğreticide **contosodataset** kullanılır. Veri kümesini kaldırdığınızda, rapor da kaldırılır.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Kaynakları temizlemek için Azure CLI'yi kullanın

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın. `$resourceGroup`bu öğreticinin başında **ContosoResources** olarak ayarlanmıştır.

```azurecli-interactive
az group delete --name $resourceGroup
```

### <a name="use-powershell-to-clean-up-resources"></a>Kaynakları temizlemek için PowerShell'i kullanın

Kaynak grubunu kaldırmak için [Kaldır-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanın. `$resourceGroup`bu öğreticinin başında **ContosoResources** olarak ayarlanmıştır.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $resourceGroup
```

### <a name="clean-up-test-emails"></a>Test e-postalarını temizleme

Ayrıca, gelen kutunuzda, aygıt uygulaması çalışırken Mantık Uygulaması aracılığıyla oluşturulan e-postaların miktarını da silmek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu 2 bölümlük eğitimde, aşağıdaki görevleri gerçekleştirerek IoT Hub iletilerini farklı hedeflere yönlendirmek için ileti yönlendirmeyi nasıl kullanacağınızı öğrendiniz.  

**Bölüm I: Kaynak oluşturma, ileti yönlendirmesi ayarlama**
> [!div class="checklist"]
> * Kaynakları oluşturun -- bir IoT hub'ı, depolama hesabı, Servis Veri Hizmeti veri sırası ve benzetimli aygıt.
> * Depolama hesabı ve Servis Veri Yolu kuyruğu için IoT Hub'daki uç noktaları ve ileti yollarını yapılandırın.

**Bölüm II: Hub'a ileti gönderme, yönlendirilen sonuçları görüntüleme**
> [!div class="checklist"]
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [Bir IoT Hub ile ölçümleri ve tanılamaları ayarlama ve kullanma](tutorial-use-metrics-and-diags.md)
