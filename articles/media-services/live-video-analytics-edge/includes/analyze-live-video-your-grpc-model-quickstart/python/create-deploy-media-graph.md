---
ms.openlocfilehash: f2724a0ea0aa5f609be5847652973cfa03658c24
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89421550"
---
### <a name="examine-and-edit-the-sample-files"></a>Örnek dosyaları İnceleme ve düzenleme

Önkoşulların bir parçası olarak, örnek kodu bir klasöre indirdiniz. Örnek dosyaları incelemek ve düzenlemek için bu adımları izleyin.

1. Visual Studio Code, *src/Edge*bölümüne gidin. *. Env* dosyanızı ve birkaç dağıtım şablonu dosyasını görürsünüz.

    deployment.grpcyolov3icpu.template.js, Edge cihazının dağıtım bildirimini ifade eder. Bazı yer tutucu değerleri içerir. . Env dosyası bu değişkenlerin değerlerini içerir.
1. *Src/buluttan cihaza-Console-App* klasörüne gidin. Burada dosya ve diğer birkaç dosya *appsettings.js* görürsünüz:
    
    * operations.js, programın çalıştırmasını istediğiniz işlemlerin bir listesi.
    * main.py-örnek program kodu. Bu kod:

        * Uygulama ayarlarını yükler.
        * IoT Edge modülündeki canlı video analizinin sunduğu doğrudan yöntemleri çağırır. Doğrudan yöntemlerini çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz.
        * Program çıkışını **TERMINAL** penceresinde Incelemenize ve **Çıkış** penceresinde modül tarafından oluşturulan olayları incelemenize olanak tanıyan duraklar.
        * Kaynakları temizlemek için doğrudan yöntemleri çağırır.
1. Dosyadaki *operations.js* düzenleyin:
 
    * Grafik topolojisine olan bağlantıyı değiştirin:
    * `"topologyUrl"` : `"https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/grpcExtension/topology.json"`
    * Graphınstanceset altında, grafik topolojisinin adını önceki bağlantıdaki değerle eşleşecek şekilde düzenleyin:
    * `"topologyName"` : `"InferencingWithGrpcExtension"`
    * GraphTopologyDelete altında adı düzenleyin:
    * `"name"` : `"InferencingWithGrpcExtension"`

> [!NOTE]
> <p>
> <details>
> <summary>Bunu genişletin ve MediaGraphGrpcExtension düğümünün topolojide nasıl uygulandığını inceleyin</summary>
> <pre><code>
> {
>   "@type": "#Microsoft.Media.MediaGraphGrpcExtension",
>   "name": "grpcExtension",
>   "endpoint": {
>       "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
>       "url": "${grpcExtensionAddress}",
>       "credentials": {
>           "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
>           "username": "${grpcExtensionUserName}",
>           "password": "${grpcExtensionPassword}"
>       }
>   },
>   "dataTransfer": {
>       "mode": "sharedMemory",
>       "SharedMemorySizeMiB": "5"
>   },
>   "image": {
>       "scale": {
>           "mode": "${imageScaleMode}",
>           "width": "${frameWidth}",
>           "height": "${frameHeight}"
>       },
>       "format": {
>           "@type": "#Microsoft.Media.MediaGraphImageFormatEncoded",
>           "encoding": "${imageEncoding}",
>           "quality": "${imageQuality}"
>       }
>   },
>   "inputs": [
>       {
>           "nodeName": "motionDetection"
>       }
>   ]
> }          
> </code></pre>
> </details>    
> </p>
    
### <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini oluşturma ve dağıtma

1. Dosyasında *src/Edge/* *deployment.grpcyolov3icpu.template.js* öğesine sağ tıklayın ve ardından **IoT Edge dağıtım bildirimi oluştur**' u seçin.

    ![IoT Edge dağıtım bildirimi oluştur](../../../media/quickstarts/generate-iot-edge-deployment-manifest-grpc.png)

    Bildirim dosyasındaki *deployment.grpcyolov3icpu.amd64.js* *src/Edge/config* klasöründe oluşturulur.
1. [Hareket ve yayma olaylarını Algıla](../../../detect-motion-emit-events-quickstart.md) hızlı başlangıcı ' nı tamamlayıp bu adımı atlayın.

    Aksi halde, sol alt köşedeki **Azure ıOT hub** bölmesinin yakınında, **diğer eylemler** simgesini ve ardından **IoT Hub bağlantı dizesi ayarla**' yı seçin. Dizeyi dosyadaki *appsettings.js* kopyalayabilirsiniz. Ya da Visual Studio Code içinde doğru IoT Hub 'ı yapılandırdığınızdan emin olmak için, [IoT Hub 'ı Seç komutunu](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub)kullanın.

    ![IoT Hub bağlantı dizesi](../../../media/quickstarts/iot-hub-connection-string-grpc.png)
1. *Kaynak/kenar/yapılandırma/* *deployment.grpcyolov3icpu.amd64.jsüzerinde* sağ tıklayın ve **tek cihaz için dağıtım oluştur**' u seçin.

    ![dağıtım tek cihaz oluştur](../../../media/quickstarts/create-deployment-single-device-grpc.png)
1. IoT Hub bir cihaz seçmeniz istendiğinde, **LVA-örnek-cihaz**' ı seçin.
1. 30 saniye sonra, pencerenin sol alt köşesinde Azure IoT Hub ' yi yenileyin. Edge cihazında artık aşağıdaki dağıtılan modüller gösterilmektedir:

    * **Lvaedge**adlı canlı video analizi modülü.
    * Bir RTSP sunucusuna benzetir ve canlı video akışı kaynağı görevi gören **rtspsim** modülü.

        > [!NOTE]
        > Kurulum betiğimizden temin yerine kendi Edge cihazınızı kullanıyorsanız, bu hızlı başlangıç için kullanılan örnek video dosyasını çekmek ve depolamak için uç cihazınıza gidin ve **yönetici haklarıyla**aşağıdaki komutları çalıştırın:  

        ```
        mkdir /home/lvaadmin/samples
        mkdir /home/lvaadmin/samples/input    
        curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
        chown -R lvaadmin /home/lvaadmin/samples/  
        ```
    * GRPC 'yi iletişim yöntemi olarak kullanan YOLOv3 nesne algılama modeli olan **Lvaextension** modülü, görüntülere görüntü görüntüsü uygular ve nesne türlerinin birden çok sınıfını döndürür.
    
    ![LVA uzantısı](../../../media/quickstarts/lvaextension-grpc.png)

### <a name="prepare-to-monitor-events"></a>Olayları izlemeye hazırlanma

Canlı video analizi cihazına sağ tıklayın ve **Izlemeyi Başlat yerleşik olay uç noktası**' nı seçin. Visual Studio Code **Çıkış** penceresinde IoT Hub olaylarını izlemek için bu adıma ihtiyacınız vardır.

![İzlemeyi Başlat](../../../media/quickstarts/start-monitoring-built-event-endpoint-grpc.png)

### <a name="run-the-sample-program"></a>Örnek programı çalıştırma

1. Bir hata ayıklama oturumu başlatmak için F5 tuşunu seçin. TERMINAL penceresinde yazdırılan iletileri görürsünüz.
1. Kod * üzerindekioperations.js* doğrudan yöntemlere ve çağrılarıyla başlatılır `GraphTopologyList` `GraphInstanceList` . Önceki hızlı başlangıçlarını tamamladıktan sonra kaynakları temizledikten sonra bu işlem boş listeleri döndürür ve ardından duraklatılır. Devam etmek için Enter tuşunu seçin.
    
    ```
    -------------------------------Executing operation GraphTopologyList-----------------------  
    Request: GraphTopologyList  --------------------------------------------------
    {
    "@apiVersion": "1.0"
    }
    ---------------  
    Response: GraphTopologyList - Status: 200  ---------------
    {
    "value": []
    }
    --------------------------------------------------------------------------
    Executing operation WaitForInput
    Press Enter to continue
    ```
    
    **TERMINAL** penceresinde, bir sonraki doğrudan yöntem çağrısı kümesi gösterilir:
    
    * `GraphTopologySet`Önceki Topologyıurl 'yi kullanan öğesine yapılan bir çağrı
    * Aşağıdaki gövdesini kullanan öğesine yapılan bir çağrı `GraphInstanceSet` :
    
    ```
    {
      "@apiVersion": "1.0",
      "name": "Sample-Graph-1",
      "properties": {
        "topologyName": "InferencingWithGrpcExtension",
        "description": "Sample graph description",
        "parameters": [
          {
            "name": "rtspUrl",
            "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
          },
          {
            "name": "rtspUserName",
            "value": "testuser"
          },
          {
            "name": "rtspPassword",
            "value": "testpassword"
          }
        ]
      }
    }
    ```
    
    * `GraphInstanceActivate`Grafik örneğini ve videonun akışını başlatan öğesine yönelik bir çağrı.
    * `GraphInstanceList`Grafik örneğinin çalışır durumda olduğunu gösteren ikinci bir çağrı.
1. **TERMINAL** penceresindeki çıktı, bir ENTER tuşuna basarak komut istemine devam edebilir. Henüz ENTER ' ı seçmeyin. Doğrudan çağrdığınız yöntemler için JSON yanıtı yüklerini görmek üzere yukarı kaydırın.
1. Visual Studio Code **Çıkış** penceresine geçin. IoT Edge modülündeki canlı video analizinin IoT Hub 'ına gönderdiğini iletiler görürsünüz. Bu hızlı başlangıçta aşağıdaki bölümde bu iletiler ele alınmaktadır.
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, **TERMINAL** penceresine dönün ve ENTER ' u seçin.
1. Sonraki çağrı dizisi kaynakları temizler:
    
    * `GraphInstanceDeactivate`Grafik örneğini devre dışı bırakmak için bir çağrı.
    * `GraphInstanceDelete`Örneği silme çağrısı.
    * İçin bir çağrı `GraphTopologyDelete` , topolojiyi siler.
    * İçin nihai bir çağrı `GraphTopologyList` , listenin boş olduğunu gösterir.

