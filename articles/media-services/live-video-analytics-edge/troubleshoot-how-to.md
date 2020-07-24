---
title: IoT Edge-Azure 'da canlı video analizlerinin sorunlarını giderme
description: Bu makalede, IoT Edge üzerinde canlı video analizi için sorun giderme adımları ele alınmaktadır.
author: IngridAtMicrosoft
ms.topic: how-to
ms.author: inhenkel
ms.date: 05/24/2020
ms.openlocfilehash: 62163217862f586be7ed5c0a6000693f8e7fcdd6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87043190"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>IoT Edge 'da canlı video analizi sorunlarını giderme

Bu makalede, Azure IoT Edge üzerindeki canlı video analizi (LVA) için sorun giderme adımları ele alınmaktadır.

## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

### <a name="diagnostics"></a>Tanılama

Canlı video analizi dağıtımınızın bir parçası olarak, IoT Hub ve IoT Edge cihazları gibi Azure kaynaklarını ayarlarsınız. Sorunları tanılamaya yönelik ilk adım olarak, aşağıdaki yönergeleri izleyerek sınır cihazının düzgün şekilde ayarlandığından emin olun:

1. [ `check` Komutunu çalıştırın](../../iot-edge/troubleshoot.md#run-the-check-command).
1. [IoT Edge sürümünüzü denetleyin](../../iot-edge/troubleshoot.md#check-your-iot-edge-version).
1. [IoT Edge Güvenlik Yöneticisi ve günlüklerinin durumunu kontrol edin](../../iot-edge/troubleshoot.md#check-the-status-of-the-iot-edge-security-manager-and-its-logs).
1. [IoT Edge hub 'ı üzerinden giden Iletileri görüntüleyin](../../iot-edge/troubleshoot.md#view-the-messages-going-through-the-iot-edge-hub).
1. [Kapsayıcıları yeniden başlatın](../../iot-edge/troubleshoot.md#restart-containers).
1. [Güvenlik duvarı ve bağlantı noktası yapılandırma kurallarınızı denetleyin](../../iot-edge/troubleshoot.md#check-your-firewall-and-port-configuration-rules).

### <a name="pre-deployment-issues"></a>Dağıtım öncesi sorunlar

Kenar altyapısı düzgün çalışıyorsa, dağıtım bildirim dosyası ile ilgili sorunları arayabilirsiniz. Canlı video analizlerini diğer IoT modüllerinin yanı sıra IoT Edge cihazında IoT Edge modüle dağıtmak için, IoT Edge hub, IoT Edge Aracısı ve diğer modülleri ve bunların özelliklerini içeren bir dağıtım bildirimi kullanırsınız. JSON kodu düzgün biçimlendirilmemiş ise, şu hatayı alabilirsiniz: 

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```

' <deployment manifest.json> Content ' bağımsız değişkeni için ' ' DOSYASıNDAN JSON ayrıştırılamadı: "ek veriler: satır 101 sütun 1 (karakter 5325)"

Bu hatayla karşılaşırsanız, eksik parantezler veya dosyanın yapısıyla ilgili diğer sorunlar için JSON 'ı denetlemeniz önerilir. Dosya yapısını doğrulamak için, [JSON Görüntüleyici eklentisi Ile Notepad + +](https://riptutorial.com/notepadplusplus/example/18201/json-viewer) gibi bir Istemciyi veya [JSON biçimlendirici & Doğrulayıcı](https://jsonformatter.curiousconcept.com/)gibi bir çevrimiçi aracı kullanabilirsiniz.

### <a name="during-deployment-diagnose-with-media-graph-direct-methods"></a>Dağıtım sırasında: doğrudan medya Graf yöntemi ile tanılama yöntemleri 

IoT Edge modülündeki canlı video analizi IoT Edge cihazında doğru dağıtıldıktan sonra, [doğrudan Yöntemler](direct-methods.md)' i çağırarak medya grafiğini oluşturabilir ve çalıştırabilirsiniz. Doğrudan yöntemler aracılığıyla medya grafiğinin bir tanılamasını çalıştırmak için Azure portal kullanabilirsiniz:

1. Azure portal, IoT Edge cihazınıza bağlı olan IoT Hub 'ına gidin.

1. **Otomatik cihaz yönetimini**arayın ve ardından **IoT Edge**' yi seçin.  

1. Sınır aygıtları listesinde, tanılamak istediğiniz cihazı seçin.  
         
    ![Sınır cihazlarının listesini görüntüleyen Azure portal ekran görüntüsü](./media/troubleshoot-how-to/lva-sample-device.png)

1. Yanıt kodu *200-Tamam*olup olmadığını kontrol edin. [IoT Edge çalışma zamanına](../../iot-edge/iot-edge-runtime.md) yönelik diğer yanıt kodları şunlardır:
    * 400-dağıtım yapılandırması hatalı biçimlendirilmiş veya geçersiz.
    * 417-cihazda bir dağıtım yapılandırma kümesi yok.
    * 412-dağıtım yapılandırmasındaki şema sürümü geçersiz.
    * 406-IoT Edge cihaz çevrimdışı veya durum raporları göndermiyor.
    * 500-IoT Edge çalışma zamanında bir hata oluştu.

1. Bir durum 501 kodu alırsanız doğrudan yöntem adının doğru olduğundan emin olun. Yöntem adı ve istek yükü doğru ise, başarılı kod = 200 ile birlikte sonuçları almalısınız. İstek yükü doğru değilse, bir durum = 400 ve doğrudan yöntem çağrla ilgili sorunu tanılamanıza yardımcı olması gereken hata kodunu ve iletiyi gösteren bir yanıt yükü alırsınız.
    * Bildirilen ve Desired özelliklerinin denetlenmesi, modül özelliklerinin dağıtımla eşitlenip eşitlenmediğini anlamanıza yardımcı olabilir. Aksi takdirde, IoT Edge cihazınızı yeniden başlatabilirsiniz. 
    * Özellikle Graphtopologyılist gibi basit olan birkaç yöntemi çağırmak için [doğrudan Yöntemler](direct-methods.md) kılavuzunu kullanın. Kılavuz ayrıca beklenen istek ve yanıt yüklerini ve hata kodlarını da belirtir. Basit doğrudan Yöntemler başarılı olduktan sonra, canlı video analizi IoT Edge modülünün işlevsel olarak tamam olduğundan emin olabilirsiniz.
        
       ![IoT Edge modülü için "doğrudan yöntem" bölmesinin ekran görüntüsü.](./media/troubleshoot-how-to/direct-method.png) 

1. **Dağıtımda belirtilen** ve **cihaz sütunları tarafından rapor** edildiğinde, IoT Edge modülündeki canlı video *analizine doğrudan*yöntemleri çağırabilirsiniz. İstenen ve bildirilen özellikleri denetleyebilir ve doğrudan yöntemleri çağırabileceğiniz bir sayfaya gitmek için modülü seçin. Aşağıdakileri göz önünde bulundurun: 

### <a name="post-deployment-diagnose-logs-for-issues-during-the-run"></a>Dağıtım sonrası: çalıştırma sırasında sorunlar için günlükleri tanılama 

IoT Edge modülünüzün kapsayıcı günlükleri modül çalışma zamanı sırasında sorunlarınızı ayıklamanıza yardımcı olmak için tanılama bilgileri içermelidir. Sorun [için kapsayıcı günlüklerini denetleyebilir](../../iot-edge/troubleshoot.md#check-container-logs-for-issues) ve sorunu kendi kendine tanılayabilirsiniz. 

Önceki tüm denetimleri çalıştırırsanız ve sorun yaşamaya devam ediyorsanız, Azure ekibi tarafından daha fazla [analiz için IoT Edge `support bundle` ](../../iot-edge/troubleshoot.md#gather-debug-information-with-support-bundle-command) cihazdan günlükleri toplayın. Destek için [bizimle iletişime geçerek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) toplanan günlükleri gönderebilirsiniz.

## <a name="common-error-resolutions"></a>Genel hata çözümleri

Canlı video analizi, IoT Edge cihazında bir IoT Edge modülü olarak dağıtılır ve IoT Edge Aracısı ve hub modülleriyle işbirliği içinde çalışmaktadır. Canlı video analizi dağıtımıyla karşılaşacağınız bazı yaygın hatalardan bazıları, temel IoT altyapısından kaynaklanan sorunlardan kaynaklanır. Hatalar şunlardır:

* [IoT Edge Aracısı yaklaşık bir dakika sonra durdu](../../iot-edge/troubleshoot-common-errors.md#iot-edge-agent-stops-after-about-a-minute).
* [IoT Edge Aracısı bir modülün görüntüsüne erişemiyor (403)](../../iot-edge/troubleshoot-common-errors.md#iot-edge-agent-cant-access-a-modules-image-403).
* [IoT Edge aracı modülü "boş yapılandırma dosyası" raporlar ve cihazda hiçbir modül başlatılmaz](../../iot-edge/troubleshoot-common-errors.md#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device).
* [IoT Edge hub 'ı başlatılamıyor](../../iot-edge/troubleshoot-common-errors.md#iot-edge-hub-fails-to-start).
* [IoT Edge güvenlik arka plan programı geçersiz bir ana bilgisayar adıyla başarısız oluyor](../../iot-edge/troubleshoot-common-errors.md#iot-edge-security-daemon-fails-with-an-invalid-hostname).
* [Canlı video analizi veya başka bir özel IoT Edge modülü, 404 hatası ile uç hub 'ına ileti gönderemediğinde](../../iot-edge/troubleshoot-common-errors.md#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error).
* [IoT Edge modülü başarıyla dağıtıldı ve sonra cihazdan kayboluyor](../../iot-edge/troubleshoot-common-errors.md#iot-edge-module-deploys-successfully-then-disappears-from-device).

### <a name="edge-setup-script-issues"></a>Edge kurulum betiği sorunları

Belgelerimizin bir parçası olarak Edge ve bulut kaynaklarını dağıtmaya yönelik bir [kurulum betiği](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) sunuyoruz ve canlı video analizi kenarıyla çalışmaya başlamanızı sağlıyoruz. Bu bölümde karşılaşabileceğiniz bazı betik hataları ve bunları hata ayıklama çözümleriyle birlikte görebilirsiniz.

Sorun: betik çalışıyor, kısmen birkaç kaynak oluşturuyor, ancak şu iletiyle başarısız oluyor:

```
registering device...

Unable to load extension 'eventgrid: unrecognized kwargs: ['min_profile']'. Use --debug for more information.
The command failed with an unexpected error. Here is the traceback:

No module named 'azure.mgmt.iothub.iot_hub_client'
Traceback (most recent call last):
File "/opt/az/lib/python3.6/site-packages/knack/cli.py", line 215, in invoke
  cmd_result = self.invocation.execute(args)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 631, in execute
  raise ex
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 695, in _run_jobs_serially
  results.append(self._run_job(expanded_arg, cmd_copy))
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 688, in _run_job
  six.reraise(*sys.exc_info())
File "/opt/az/lib/python3.6/site-packages/six.py", line 693, in reraise
  raise value
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 665, in _run_job
  result = cmd_copy(params)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 324, in __call__
  return self.handler(*args, **kwargs)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/__init__.py", line 574, in default_command_handler
  return op(**command_args)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 75, in iot_device_list
  result = iot_query(cmd, query, hub_name, top, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 45, in iot_query
  target = get_iot_hub_connection_string(cmd, hub_name, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/common/_azure.py", line 112, in get_iot_hub_connection_string
  client = iot_hub_service_factory(cmd.cli_ctx)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/_factory.py", line 28, in iot_hub_service_factory
  from azure.mgmt.iothub.iot_hub_client import IotHubClient
ModuleNotFoundError: No module named 'azure.mgmt.iothub.iot_hub_client'
```
    
Bu sorunu düzeltmek için:

1. Şu komutu çalıştırın:

    ```
    az --version
    ```
1. Aşağıdaki uzantıların yüklü olduğundan emin olun. Bu makalenin yayımında, Uzantılar ve sürümleri şunlardır:

    | Uzantı | Sürüm |
    |---|---|
    |azure-cli   |      2.5.1|
    |komut-modüller-nspkg         |   2.0.3|
    |çekirdeğin  |    2.5.1|
    |nspkg    | 3.0.4|
    |telemetri| 1.0.4|
    |storage-preview          |     0.2.10|
    |azure-cli-iot-ext          |    0.8.9|
    |eventgrid| 0.4.9|
    |azure-iot                       | 0.9.2|
1. Sürümü burada listelenen sürüm numarasından daha eski olan yüklü bir uzantıya sahipseniz, aşağıdaki komutu kullanarak uzantıyı güncelleştirin:

    ```
    az extension update --name <Extension name>
    ```

    Örneğin, öğesini çalıştırabilirsiniz `az extension update --name azure-iot` .

### <a name="sample-app-issues"></a>Örnek uygulama sorunları

Yayınımızın bir parçası olarak, geliştirici topluluğumuzu önyüklendi almak için bazı .NET örnek kodları sunuyoruz. Bu bölüm, örnek kodu çalıştırdığınızda karşılaşabileceğiniz bazı hataları ve bunları hata ayıklama çözümleriyle birlikte gösterir.

Sorun: Program.cs doğrudan yöntem çağrısında aşağıdaki hata ile başarısız oluyor:

```
Unhandled exception. Microsoft.Azure.Devices.Common.Exceptions.UnauthorizedException: {"Message":"{\"errorCode\":401002,\"trackingId\":\"b1da85801b2e4faf951a2291a2c467c3-G:32-TimeStamp:04/06/2020 17:15:11\",\"message\":\"Unauthorized\",\"timestampUtc\":\"2020-04-06T17:15:11.6990676Z\"}","ExceptionMessage":""}
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpClient httpClient, HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`2 isMappedToException, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
        
        at Microsoft.Azure.Devices.HttpClientHelper.PostAsync[T,T2](Uri requestUri, T entity, TimeSpan operationTimeout, IDictionary`2 errorMappingOverrides, IDictionary`2 customHeaders, CancellationToken cancellationToken)…
```

1. Visual Studio Code ortamınızda [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) 'nın yüklü olduğundan ve IoT Hub 'ınız ile kurulan bağlantıyı ayarlamış olduğunuzdan emin olun. Bunu yapmak için CTRL + SHIFT + P ' yi seçin ve ardından **IoT Hub yöntemi seç**' i seçin.

1. Visual Studio Code aracılığıyla IoT Edge modülünde doğrudan bir yöntemi çağırıp çağıramayacağını denetleyin. Örneğin, şu yük ile graphtopologylist çağrısı yapın { &nbsp; " @apiVersion ": "1,0"}. Aşağıdaki yanıtı almalısınız: 

    ```
    {
      "status": 200,
      "payload": {
        "values": [
          {…
    …}
          ]
        }
    }
    ```

    ![Visual Studio Code yanıtın ekran görüntüsü.](./media/troubleshoot-how-to/visual-studio-code1.png)
1. Önceki çözüm başarısız olursa, aşağıdakileri deneyin:

    a. IoT Edge cihazınızda komut istemine gidin ve şu komutu çalıştırın:
    
      ```
      sudo systemctl restart iotedge
      ```

      Bu komut IoT Edge cihazını ve tüm modülleri yeniden başlatır. Birkaç dakika bekleyin ve ardından doğrudan yöntemi yeniden kullanmayı denemeden önce, aşağıdaki komutu çalıştırarak modüllerin çalıştığını doğrulayın:

      ```
      sudo iotedge list
      ```

    b. Yukarıdaki yaklaşım da başarısız olursa, sanal makinenizi veya bilgisayarınızı yeniden başlatmayı deneyin.

    c. Tüm yaklaşımlar başarısız olursa, tüm [ilgili günlüklere](../../iot-edge/troubleshoot.md#gather-debug-information-with-support-bundle-command)sahip daraltılmış bir dosyayı edinmek ve bir [destek biletini](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)eklemek için aşağıdaki komutu çalıştırın.

    ```
    sudo iotedge support-bundle --since 2h
    ```

1. Hata yanıtı *400* kodu alırsanız, yöntem çağırma yükünün [doğrudan Yöntemler](direct-methods.md) Kılavuzu uyarınca düzgün biçimlendirildiğinden emin olun.
1. Bir durum *200* kodu alırsanız, hub 'ınızın düzgün çalıştığını ve modül dağıtımınızın doğru ve yanıt verdiğini gösterir. 

1. Uygulama yapılandırmasının doğru olup olmadığını denetleyin. Uygulama yapılandırmanız *appsettings.js* dosyadaki aşağıdaki alanlardan oluşur. DeviceID ve ModuleID 'nin doğru olduğundan emin olmak için iki kez denetleyin. Bir denetim yapmanın kolay yolu, Visual Studio Code Azure IoT Hub uzantısı bölümüne gitmenin bir yoludur. Dosyadaki *appsettings.js* ve IoT Hub bölümündeki değerler eşleşmelidir.
    
    ```
    {
        "IoThubConnectionString" : 
        "deviceId" : 
        "moduleId" : 
    }
    ```

1. Dosyadaki *appsettings.js* , [bağlantı dizesi biçimleri farklı](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/)olduğundan, IoT Hub cihaz bağlantı dizesini *değil* IoT Hub bağlantı dizesi girdiğinizden emin olun.

### <a name="live-video-analytics-working-with-external-modules"></a>Canlı video analizi dış modüllerle çalışıyor

HTTP uzantısı işlemcisi aracılığıyla canlı video analizi, medya grafiğini REST kullanarak HTTP üzerinden diğer IoT Edge modüllerden veri göndermek ve almak üzere genişletebilir. Belirli bir [örnek](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension)olarak, medya grafiği video çerçevelerini Yolo v3 gibi harici bir çıkarım modülüne GÖNDEREBILIR ve JSON tabanlı analiz sonuçlarını alabilir. Böyle bir topolojide, olayların hedefi çoğunlukla IoT Hub 'idir. Hub 'da çıkarım olaylarını görmeme durumlarında, aşağıdakileri denetleyin:

* Medya grafiğinin hub 'ına ve incelediğiniz hub 'ın aynı olup olmadığını kontrol edin. Birden çok dağıtım oluştururken, birden çok hub ile bitemez ve olaylar için yanlış hub 'ı yanlışlıkla kontrol edebilirsiniz.
* Visual Studio Code, dış modülün dağıtılıp dağıtılmadığını ve çalışıp çalışmadığını denetleyin. Buradaki örnek görüntüde, rtspsim ve CV, lvaEdge modülüne harici olarak çalışan IoT Edge modüllerdir.

    ![Azure IoT Hub 'de modüllerin çalışma durumunu gösteren ekran görüntüsü.](./media/troubleshoot-how-to/iot-hub.png)

* Doğru URL uç noktasına olay gönderiyor olup olmadığınızı denetleyin. Dış AI kapsayıcısı, üzerinden bir URL ve bir bağlantı noktası kullanıma sunar ve POST isteklerinden verileri döndürür. Bu URL `endpoint: url` , http uzantısı işlemcisi için bir özellik olarak belirtilir. [TOPOLOJI URL](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json)'sinde görüldüğü gibi, uç nokta, ıncobir URL parametresi olarak ayarlanır. [Parametresi](http://yolov3/score) için varsayılan değerin veya geçirilen değerin doğru olduğundan emin olun. Istemci URL 'SI (kıvrımlı) kullanarak çalışıp çalışmadığını görmek için test edebilirsiniz.  

    Örnek olarak, 172.17.0.3 IP adresine sahip yerel makinede çalışan bir Yolo v3 kapsayıcısı aşağıda verilmiştir. IP adresini bulmak için Docker İnceleme ' i kullanın.

    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    Sonuç döndürüldü:

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```

* HTTP uzantısı işlemcisini kullanan bir grafiğin bir veya daha fazla örneğini çalıştırıyorsanız, her HTTP uzantısı işlemcisinin video akışındaki kare/saniye (fps) hızını yönetmesi için bir kare oranı filtresine sahip olmanız gerekir. 

   Belirli durumlarda, uç makinenin CPU veya belleğinin yüksek oranda kullanıldığı durumlar için bazı çıkarım olaylarını kaybedebilirsiniz. Bu sorunu gidermek için kare hızı filtresinde maximumFps özelliği için düşük bir değer ayarlayın. Grafiği her bir örneği üzerinde 0,5 ("maximumFps": 0,5) olarak ayarlayabilir ve ardından, hub üzerindeki çıkarım olaylarını denetlemek için örneği yeniden çalıştırabilirsiniz.

   Alternatif olarak, daha yüksek CPU ve bellek ile daha güçlü bir Edge makinesi elde edebilirsiniz.
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>Paralel olarak birden çok doğrudan yöntem – timeout hatası 

IoT Edge üzerindeki canlı video analizi, birden fazla topoloji ve birden çok grafik örneği ayarlamanıza olanak tanıyan doğrudan Yöntem tabanlı bir programlama modeli sağlar. Topoloji ve grafik kurulumunun bir parçası olarak, IoT Edge modülünde birden çok doğrudan yöntem çağrısı çağırılır. Bu birden çok yöntem çağrılarını paralel olarak çağırırsanız, özellikle de grafikleri başlatıp durdurmuş gibi, aşağıdaki gibi bir zaman aşımı hatası yaşayabilirsiniz: 

Derleme başlatma yöntemi Microsoft.Media.LiveVideoAnalytics.Test.Feature.Edge.AssemblyInitializer.InitializeAssemblyAsync özel durum oluşturdu. Microsoft. Azure. Devices. Common. Exceptions. ıothubexception: Microsoft. Azure. Devices. Common. Exceptions. ıothubexception:<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

Paralel olarak doğrudan yöntemleri *çağırmamalıdır* . Bunları sırayla çağırın (yani, yalnızca önceki bir yöntem bittikten sonra bir doğrudan yöntem çağrısı yapın).

### <a name="collect-logs-for-submitting-a-support-ticket"></a>Destek bileti göndermek için günlükleri toplayın

Kendi kendine destekli sorun giderme adımları sorununuzu gidermezse, Azure portal gidin ve [bir destek bileti açın](../../azure-portal/supportability/how-to-create-azure-support-request.md).

Bilet içine eklenmesi gereken ilgili günlükleri toplamak için sonraki bölümlerde yer alan yönergeleri izleyin. Günlük dosyalarını, destek isteğinin **Ayrıntılar** bölmesine yükleyebilirsiniz.

### <a name="use-the-support-bundle-command"></a>Support-demeti komutunu kullanma

IoT Edge cihazdan Günlükler toplamanız gerektiğinde, en kolay yöntem `support-bundle` komutunu kullanmaktır. Bu komut şunu toplar:

- Modül günlükleri
- IoT Edge Güvenlik Yöneticisi ve kapsayıcı altyapısı günlükleri
- Iotedge JSON çıkışını denetle
- Yararlı hata ayıklama bilgileri

1. `support-bundle`Günlüklerinizin ne kadar zaman kapsamasını istediğinizi belirtmek için *--since* bayrağıyla komutunu çalıştırın. Örneğin, 2H, son iki saat için günlükleri alacak. Bu bayrağın değerini farklı dönemler için Günlükler içerecek şekilde değiştirebilirsiniz.

    ```
    sudo iotedge support-bundle --since 2h
    ```

   Bu komut, komutu çalıştırdığınız dizinde *support_bundle.zip* adlı bir dosya oluşturur. 
   
1. *support_bundle.zip* dosyasını destek bileti 'ne ekleyin.

### <a name="live-video-analytics-debug-logs"></a>Canlı video analizi hata ayıklama günlükleri

IoT Edge modülündeki canlı video analizlerini hata ayıklama günlükleri oluşturacak şekilde yapılandırmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com)oturum açın ve IoT Hub 'ınıza gidin.
1. Sol bölmede **IoT Edge**' yi seçin.
1. Cihazlar listesinde hedef cihazın KIMLIĞINI seçin.
1. Bölmenin üst kısmında, **modülleri ayarla**' yı seçin.

   ![Azure portal "modülleri ayarlama" düğmesinin ekran görüntüsü.](media/troubleshoot-how-to/set-modules.png)

1. **IoT Edge modüller** bölümünde, **lvaedge**' i bulup seçin.
1. **Kapsayıcı oluşturma seçeneklerini**belirleyin.
1. **Bağlamalar** bölümünde aşağıdaki komutu ekleyin:

    `/var/local/mediaservices/logs:/var/lib/azuremediaservices/logs`

    > [!NOTE] 
    > Bu komut, uç aygıtı ve kapsayıcısı arasındaki Günlükler klasörlerini bağlar. Günlükleri farklı bir konumda toplamak istiyorsanız, aşağıdaki komutu kullanın ve **$LOG _LOCATION_ON_EDGE_DEVICE** , kullanmak istediğiniz konumla değiştirin:`/var/$LOG_LOCATION_ON_EDGE_DEVICE:/var/lib/azuremediaservices/logs`

1. **Güncelleştir**’i seçin.
1. **Gözden geçir + oluştur**' u seçin. Başarılı bir doğrulama iletisi yeşil bir başlık altında gönderilir.
1. **Oluştur**’u seçin.
1. **Modül kimliği ikizi** öğesini, günlüklerin toplandığı dizine Işaret eden DebugLogsDirectory parametresini işaret etmek üzere güncelleştirin:

    a. **Modüller** tablosu altında **lvaedge**' i seçin.  
    b. Bölmenin üst kısmında **modül kimliği ikizi**' nı seçin. Düzenlenebilir bir bölme açılır.  
    c. **İstenen anahtar**altında aşağıdaki anahtar/değer çiftini ekleyin:  
    `"DebugLogsDirectory": "/var/lib/azuremediaservices/logs"`

    > [!NOTE] 
    > Bu komut, uç aygıtı ve kapsayıcısı arasındaki Günlükler klasörlerini bağlar. Günlükleri farklı bir konumda toplamak istiyorsanız, aşağıdaki komutu kullanın ve **$DEBUG _LOG_LOCATION_ON_EDGE_DEVICE** , kullanmak istediğiniz konumla değiştirin:  
    > `"DebugLogsDirectory": "/var/$DEBUG_LOG_LOCATION_ON_EDGE_DEVICE"`  

    d. **Kaydet**'i seçin.

1. Sorunu yeniden üretin.
1. Portaldaki **IoT Hub** sayfasından sanal makineye bağlanın.
1. *Hata günlüğü* klasöründeki tüm dosyaları ZIP.

   > [!NOTE]
   > Bu günlük dosyaları kendi kendine Tanılama için tasarlanmamıştır. Bunlar, Azure mühendislik ekibinin sorunlarınızı analiz edebilmesi için tasarlanmıştır.

   a. Aşağıdaki komutta **$DEBUG _LOG_LOCATION_ON_EDGE_DEVICE** , daha önce ayarladığınız uç cihazdaki hata ayıklama günlüklerinin konumuyla değiştirdiğinizden emin olun.  

   ```
   sudo apt install zip unzip  
   zip -r debugLogs.zip $DEBUG_LOG_LOCATION_ON_EDGE_DEVICE 
   ```

   b. *debugLogs.zip* dosyasını destek bileti 'ne ekleyin.

1. **Modül kimliği ikizi** içindeki değeri *null*olarak ayarlayarak günlük toplamayı durdurabilirsiniz. **Modül kimliği ikizi** sayfasına dönün ve aşağıdaki parametreyi şu şekilde güncelleştirin:

    `"DebugLogsDirectory": ""`

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: bulutta buluta ve kayıttan yürütmeye yönelik olay tabanlı video kaydı](event-based-video-recording-tutorial.md)
