---
title: IoT Edge-Azure 'da canlı video analizlerinin sorunlarını giderme
description: Bu makalede, IoT Edge üzerinde canlı video analizi için sorun giderme adımları ele alınmaktadır.
ms.topic: how-to
ms.date: 05/24/2020
ms.openlocfilehash: c235dd27da1d370531c1668c40586d4ae479aec7
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261124"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>IoT Edge 'da canlı video analizi sorunlarını giderme

Bu makalede, IoT Edge üzerinde canlı video analizi için sorun giderme adımları ele alınmaktadır.

## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

### <a name="diagnostics"></a>Tanılama

Canlı video analizine dağıtım kapsamında, IoT Hub ve IoT Edge cihaz gibi Azure kaynaklarını ayarlayacaksınız. Sorunları tanılamaya yönelik ilk adım olarak her zaman, bu yönergeleri izleyerek kenarının düzgün şekilde ayarlanmamasını sağlar:

1. [' Check ' komutunu çalıştırın](https://docs.microsoft.com/azure/iot-edge/troubleshoot#run-the-check-command)
1. [IoT Edge sürümünüzü denetleyin](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-your-iot-edge-version)
1. [IoT Edge Güvenlik Yöneticisi ve günlüklerinin durumunu kontrol edin](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-the-status-of-the-iot-edge-security-manager-and-its-logs)
1. [IoT Edge hub 'ından giden iletileri görüntüleme](https://docs.microsoft.com/azure/iot-edge/troubleshoot#view-the-messages-going-through-the-iot-edge-hub)
1. [Kapsayıcıları yeniden Başlat](https://docs.microsoft.com/azure/iot-edge/troubleshoot#restart-containers)
1. [Güvenlik Duvarı ve bağlantı noktası yapılandırma kurallarınızı denetleyin](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-your-firewall-and-port-configuration-rules)

### <a name="pre-deployment-issues"></a>Dağıtım öncesi sorunlar

Kenar altyapısı düzgün çalışıyorsa, dağıtım bildirimi dosyası ile ilgili sorunlar olup olmadığını denetleyebilirsiniz. Güncel video analizlerini diğer IoT modüllerinin yanı sıra Edge cihazında IoT Edge modüle dağıtmak için, Edge hub, Edge Aracısı ve özellikleri olan diğer modülleri içeren bir dağıtım bildirimi kullanacaksınız. JSON düzgün biçimlendirilmediyse, aşağıdaki gibi bir hata alabilirsiniz: 

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```

' <deployment manifest.json> Content ' bağımsız değişkeni için ' ' dosyasından JSON ayrıştırılamadı: "ek veriler: satır 101 sütun 1 (karakter 5325)"

Bu hatayla karşılaşırsanız, eksik parantez veya dosyanın yapısıyla ilgili diğer sorunlar için JSON dosyasını denetlemeniz önerilir. [JSON Görüntüleyicisi eklentisi Ile Notepad + +](https://riptutorial.com/notepadplusplus/example/18201/json-viewer) gibi bir istemci veya https://jsonformatter.curiousconcept.com/ dosya yapısını doğrulamak için gibi bir çevrimiçi araç kullanabilirsiniz.

### <a name="deployment--diagnose-with-media-graph-direct-methods"></a>Dağıtım – medya Graf doğrudan yöntemleriyle Tanıla 

IoT Edge modüldeki canlı video analizi, sınır cihazında doğru dağıtıldıktan sonra, [doğrudan Yöntemler](direct-methods.md)' i çağırarak medya grafiğini oluşturup çalıştırabilirsiniz. Doğrudan yöntemler aracılığıyla medya grafiğinin tanılamasını çalıştırmak için portalını kullanabilirsiniz:

1. Portal aracılığıyla, Edge cihazınıza bağlı IoT Hub gidin.
    1. IoT Hub dikey penceresinde, otomatik cihaz yönetimi->IoT Edge bakın.
    1. IoT Edge tıklamak uç cihazların bir listesini almalıdır. Tanılamak istediğiniz cihazı seçin.
         
        ![Edge cihazları](./media/troubleshoot-how-to/lva-sample-device.png)
    1. Yanıt kodu 200-Tamam olup olmadığını kontrol edin. [IoT Edge çalışma zamanına](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime) yönelik çeşitli diğer yanıt kodları vardır, örneğin:
        1. 400-dağıtım yapılandırması hatalı biçimlendirilmiş veya geçersiz.
        1. 417-cihazda bir dağıtım yapılandırma kümesi yok.
        1. 412-dağıtım yapılandırmasındaki şema sürümü geçersiz.
        1. 406-IoT Edge cihaz çevrimdışı veya durum raporları göndermiyor.
        1. 500-IoT Edge çalışma zamanında bir hata oluştu.
    1. Cihaza tıkladığınızda dağıtılan beklenen IoT Edge modüllerinin listesi ve bunların durumları da gösterilmelidir
    1. "Dağıtımda belirtilen" ve "cihaz tarafından bildirildi" sütunları "Evet" i gösteriyorsa, IoT Edge modülündeki canlı video analizinden doğrudan yöntemleri çağırabilirsiniz. Modüle tıkladığınızda, istenen ve bildirilen özellikleri kontrol ettiğiniz ve doğrudan yöntemleri çağırabileceğiniz bir ekrana götürür. 
        1. Bildirilen ve istenen özellikleri denetlemek, modül özelliklerinin dağıtımla eşitleniyor olup olmadığını anlamanıza yardımcı olur. Bu özellikler yoksa, kenarlarınızı yeniden başlatabilirsiniz 
        1. Özellikle Graphtopologyılist gibi basit olan birkaç yöntemi çağırmak için [doğrudan Yöntemler](direct-methods.md) kılavuzunu kullanın. Kılavuz ayrıca beklenen istek ve yanıt yüklerini ve hata kodlarını da belirtir. Basit doğrudan Yöntemler başarılı olduktan sonra, canlı video analizi kenar modülünün işlevsel olarak tamam olduğunu de tercih edebilirsiniz.
        
        ![Direct yöntemi](./media/troubleshoot-how-to/direct-method.png) 
1. Bir durum 501 kodu alırsanız lütfen doğrudan yöntem adının doğru olup olmadığını denetleyin. Yöntem adı ve istek yükü doğru ise, başarılı kod = 200 ile birlikte sonuçları geri almanız gerekir. İstek yükü doğru değilse, bir durum = 400 ve doğrudan yöntem çağrla ilgili sorunu tanılamanıza yardımcı olması gereken hata kodunu ve iletiyi gösteren bir yanıt yükü alırsınız. 

### <a name="post-deployment--diagnose-logs-for-issues-during-run"></a>Dağıtım sonrası – çalışma sırasında sorunlar için günlükleri tanılama 

Edge modülümüzdeki kapsayıcı günlüklerinin tanılaması olmalıdır<!--<todo:add link to diagnostics doc>--> Modül çalışma zamanı sırasında sorunlarınızı ayıklamanıza yardımcı olması gereken bilgiler. [Sorun için kapsayıcı günlüklerini denetleyebilir](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-container-logs-for-issues) ve kendi kendine tanılayabilir, ancak yukarıdaki tüm denetimler gerçekleştirildiyse ve sorun yaşamaya devam ediyorsanız, Azure ekibi tarafından daha fazla çözümlenebilecek olan [' destek paketi ' komutuyla](https://docs.microsoft.com/azure/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command) IoT Edge cihazdan günlükleri toplayın. Destek için bize [ulaşabilir](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) ve toplanan günlükleri gönderebilirsiniz.

## <a name="common-error-resolutions"></a>Genel hata çözümleri

Canlı video analizi, sınır cihazında bir IoT Edge modülü olarak dağıtılır ve IoT Edge Aracısı ve hub modülleriyle işbirliği içinde çalışmaktadır. Canlı video analizi dağıtımıyla ilgili bazı yaygın hatalardan bazıları, temeldeki IoT altyapısından kaynaklanan sorunlardan kaynaklanır. Aracının ve hub 'ın IoT Edge yaygın olarak karşılaşılan bazı hatalar şunlardır:

1. [IoT Edge aracı bir dakika sonra durdu](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-agent-stops-after-about-a-minute).
1. [IoT Edge Aracısı bir modülün görüntüsüne erişemiyor (403)](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-agent-cant-access-a-modules-image-403).
1. [Edge Aracısı modülü ' boş yapılandırma dosyası ' bildiriyor ve cihazda hiçbir modül başlamıyor](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device).
1. [IoT Edge hub 'ı başlatılamıyor](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-hub-fails-to-start).
1. [IoT Edge güvenlik arka plan programı geçersiz bir ana bilgisayar adıyla başarısız oluyor](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-security-daemon-fails-with-an-invalid-hostname).
1. [Canlı video analizi veya diğer özel IoT Edge modülü, 404 hatasıyla bir uç hub 'a ileti gönderemediğinde](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error).
1. [IoT Edge modül başarıyla dağıtılır ve sonra cihazdan kaybolur](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-module-deploys-successfully-then-disappears-from-device).

### <a name="edge-set-up-script-issues"></a>Edge betik sorunlarını ayarlama

Belgelerimizin bir parçası olarak, Live video Analytics Edge ile çalışmaya başlamak için Edge ve bulut kaynaklarını dağıtmaya yönelik bir [betik ayarlama](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) sağlıyoruz. Bu bölümde, betiğe ve bunların hatalarını ayıklamanıza olanak isteyebileceğiniz hatalar yakalandık.

Betik, kısmen birkaç kaynak oluşturuyor, ancak şu iletiyle başarısız oluyor:

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
1. Aşağıdaki uzantıların yüklü olduğundan emin olun. Bu kılavuzun yazıldığı itibariyle uzantılar sürümü aşağıdaki gibidir:

    |||
    |---|---|
    |azure-cli   |      2.5.1|
    |komut-modüller-nspkg         |   2.0.3|
    |çekirdeğin  |    2.5.1|
    |nspkg    | 3.0.4|
    |Telemetri| 1.0.4|
    |Uzantılardan    ||
    |storage-preview          |     0.2.10|
    |azure-cli-iot-ext          |    0.8.9|
    |eventgrid| 0.4.9|
    |azure-iot                       | 0.9.2|
1. Uzantılarından herhangi biri yukarıdaki sürüm numaralarına göre daha eskiyse, şu komutu kullanarak uzantıyı en son sürüme güncelleştirin:

    ```
    az extension update --name <Extension name>
    ```

    Örneğin, `az extension update --name azure-iot`

### <a name="sample-app-issues"></a>Örnek uygulama sorunları

Yayınımızın bir parçası olarak, geliştirici Community önyüklendi 'ı almak için bazı .NET örnek kodları sunuyoruz. Bu bölümde, örnek kodu çalıştırırken ve bu tür hataların hatalarını ayıklamada karşılaşabileceğiniz hatalar yakalandık.

1. Program.cs doğrudan yöntem çağrısında aşağıdaki hatayla başarısız olur:

    ```
    Unhandled exception. Microsoft.Azure.Devices.Common.Exceptions.UnauthorizedException: {"Message":"{\"errorCode\":401002,\"trackingId\":\"b1da85801b2e4faf951a2291a2c467c3-G:32-TimeStamp:04/06/2020 17:15:11\",\"message\":\"Unauthorized\",\"timestampUtc\":\"2020-04-06T17:15:11.6990676Z\"}","ExceptionMessage":""}
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpClient httpClient, HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`2 isMappedToException, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
        
        at Microsoft.Azure.Devices.HttpClientHelper.PostAsync[T,T2](Uri requestUri, T entity, TimeSpan operationTimeout, IDictionary`2 errorMappingOverrides, IDictionary`2 customHeaders, CancellationToken cancellationToken)…
    ```

    1. [Azure IoT ARAÇLARıNıN](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) vs Code ortamınızda yüklü olduğundan ve IoT Hub kuruluma bağlantı olduğundan emin olun. (CTRL + SHIFT + P ve sonra aboneliğinize bağlanmak için IoT Hub yöntemi seçin ve IoT Hub)
1. Edge modülünde VS Code aracılığıyla doğrudan bir yöntemi çağırıp çağırılacağını denetleyin (örneğin, şu yük ile GraphToplogyList çağrısı yapın {" @apiVersion ": "1,0"}) ve aşağıdaki yanıtı geri almanız gerekir. 

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

    ![Visual Studio Code](./media/troubleshoot-how-to/visual-studio-code1.png)
1. Yukarıdaki başarısız olursa, aşağıdakileri deneyin:
    1. Edge cihazınızda komut istemine gidin ve yazın.
    
    ```
    sudo systemctl restart iotedge
    ```

    Bu işlem, Edge cihazını ve tüm modülleri yeniden başlatacak. Birkaç dakika bekleyin ve DirectMethod 'ı yeniden kullanmayı denemeden önce modüllerin çalıştığını doğrulamak için aşağıdakileri çalıştırın.

    ```
    sudo iotedge list
    ```
    1. Ayrıca, daha fazla başarısız olursa, VM 'nizi veya makinenizi yeniden başlatmayı deneyin.
    1. Her şey başarısız olursa, bu [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)'ne eklenecek tüm [ilgili GÜNLÜKLERIN](https://docs.microsoft.com/azure/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command) bulunduğu bir ZIP dosyası almak için lütfen aşağıdakileri çalıştırın.

    ```
    sudo iotedge support-bundle --since 2h
    ```
1. Hata yanıtı 400 kodu alırsanız, yöntem çağırma yükünün [doğrudan yöntem](direct-methods.md) Kılavuzu ' na göre doğru biçimlendirildiğinden emin olun.
1. Durum 200 kodunu alırsanız, hub 'ınızın düzgün çalıştığını ve modül dağıtımınızın doğru ve yanıt verdiğini gösterir. Sonraki adımda, uygulama yapılandırmalarının doğru olup olmadığı kontrol edilir. Uygulama yapılandırmanız appSettings. JSON dosyasındaki aşağıdaki alanlardan oluşur. Lütfen DeviceID ve ModuleID 'nin doğru olduğundan emin olun. Bunu kontrol etmenin kolay bir yolu, VSCode 'daki Azure IoT Hub Extension bölümüdür. AppSettings. JSON dosyasındaki değerler ve IoT Hub bölümü eşleşmelidir.
    
    ```
    {
        "IoThubConnectionString" : 
        "deviceId" : 
        "moduleId" : 
    }
    ```

    ![ıOT HUB 'ı](./media/troubleshoot-how-to/iot-hub.png)

1. Son olarak, IoT Hub IoT Hub cihaz bağlantı dizesi olarak değil appSettings. JSON içinde, [biçimleri](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/) farklı olduğundan emin olun.

### <a name="live-video-analytics-working-with-external-modules"></a>Canlı video analizi dış modüllerle çalışıyor

HTTP uzantısı işlemcisi aracılığıyla canlı video analizi, medya grafiğini REST kullanarak HTTP üzerinden diğer IoT Edge modüllerden veri göndermek ve almak üzere genişletebilir.  Belirli bir [örnek](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension) olarak, medya grafiği video çerçevelerini Yolo v3 gibi bir dış çıkarım modülüne GÖNDEREBILIR ve JSON tabanlı analiz sonuçlarını geri alabilir. Böyle bir topolojide, olayların son hedefi çoğunlukla IoT Hub. Hub 'da çıkarım olaylarını görmeme durumlarında, lütfen aşağıdakileri kontrol edin:

1. Medya grafiğinin, vs 'ye yayınlanma sırasında bulunduğu hub 'ın ve aynı olup olmadığını kontrol edin. Bazen birden çok dağıtım oluşturduğunuzda, birden çok hub ile sona erdir olur ve olaylar için yanlış hub 'ı yanlışlıkla kontrol edebilirsiniz.
1. Dış Modül dağıtılıp çalışıyorsa VSCode aracılığıyla kontrol edin. Buradaki örnek görüntüde, rtspsim ve CV harici ve lvaEdge modülünü çalıştıran IoT Edge modüllerdir.

    ![ıOT HUB 'ı](./media/troubleshoot-how-to/iot-hub.png)
1. Olayları doğru URL uç noktasına gönderiyor olup olmadığınızı denetleyin. Dış AI kapsayıcısı, aldığı bir URL ve bir bağlantı noktası gösterir ve POST isteklerinden verileri döndürür. Bu URL, http uzantısı işlemcisi için bir uç nokta: URL özelliği olarak belirtilir. [Topoloji URL 'si](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json) ' nde görüldüğü gibi, ınıro URL parametresi olarak ayarlanır. Parametresi için varsayılan değerin ( http://yolov3/score) veya geçirilen değerin doğru olduğundan emin olun ve kıvrımlı kullanarak çalışıp çalışmadığını test edebilirsiniz.  
    1. Örnek olarak, yerel makinede çalışan Yolo v3 kapsayıcısı ve kapsayıcı için IP adresi 172.17.0.3 (IP adresini bulmak için Docker incelemesi kullanın).

    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    Sonuç döndürüldü:

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```

1. Bir grafiğin http uzantısı işlemcisinden yararlanan bir veya birden çok örneğini çalıştırıyorsanız, video akışındaki kare/saniye başına (fps) kare (fps) yönetimi için her http uzantısı Işlemcisinden önce bir kare hızı filtresine sahip olmanız gerekir. Uç makinenin CPU/belleğinin yüksek düzeyde kullanıldığı bazı durumlarda, belirli çıkarım olaylarını kaybedebilirsiniz. Bunu çözmek için kare hızı filtresindeki maximumFps özelliği için düşük bir değer ayarlayın. Bu ayarı, grafiğin her bir örneğinde 0,5 ("maximumFps": 0,5) olarak ayarlayabilir ve hub 'daki çıkarım olaylarını denetlemek için yeniden çalıştırabilirsiniz.
    1. Alternatif olarak, daha yüksek CPU ve bellek ile daha güçlü bir Edge makinesi de edinebilirsiniz.
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>Paralel olarak birden çok doğrudan yöntem – timeout hatası 

IoT Edge üzerindeki canlı video analizi, birden çok topolojinin ve birden çok grafik örneğinin oluşturulmasına olanak tanıyan doğrudan Yöntem tabanlı programlama modeli sağlar. Topoloji ve grafik kurulumunun bir parçası olarak, Edge modülünde birden çok doğrudan yöntem çağrısı çağıralınacaktır. Bu birden çok yöntem çağrılarını çağırırsanız, özellikle de grafikleri başlatıp durdurmakla birlikte, paralel olarak, aşağıdaki gibi bazı zaman aşımı hatalarıyla karşılaşabilirsiniz. 

Derleme başlatma yöntemi Microsoft. Media. LiveVideoAnalytics. test. Feature. Edge. Assemblybaşlatıcısı. InitializeAssemblyAsync özel durum oluşturdu. Microsoft. Azure. Devices. Common. Exceptions. ıothubexception: Microsoft. Azure. Devices. Common. Exceptions. ıothubexception:<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

Doğrudan yöntemleri paralel bir biçimde çağırmamalıdır, ancak bunu sıralı bir biçimde yapmanız önerilir, yani  yalnızca önceki bir yöntem gerçekleştirildikten sonra bir doğrudan yöntem çağrısı. 

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: bulutta buluta ve kayıttan yürütmeye yönelik olay tabanlı video kaydı](event-based-video-recording-tutorial.md)
