---
title: Azure Service Fabric ters proxy tanılama
description: Azure Hizmet Kumaşı uygulaması için ters proxy'de istek işlemeyi nasıl izleyeceğinizi ve tanılamayı öğrenin.
author: kavyako
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: kavyako
ms.openlocfilehash: bbc1fe5a76ecb5720bc49e0a082d5e9151b403d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645472"
---
# <a name="monitor-and-diagnose-request-processing-at-the-reverse-proxy"></a>Ters proxy'de istek işlemeyi izleme ve tanılama

Service Fabric'in 5.7 sürümünden başlayarak, geri proxy olayları koleksiyon için kullanılabilir. Olaylar iki kanalda kullanılabilir, bir ters proxy ve hem başarılı hem de başarısız istekleri için girişleri ile ayrıntılı olaylar içeren ikinci kanal istek işleme hatası ile ilgili tek hata olayları ile.

Yerel ve Azure Hizmet Kumaş kümelerinde bu kanallardan olay toplamayı etkinleştirmek için [ters proxy olaylarını topla'ya](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations) bakın.

## <a name="troubleshoot-using-diagnostics-logs"></a>Tanılama günlüklerini kullanarak sorun giderme
Karşılaşabileceğiniz yaygın hata günlüklerinin nasıl yorumlanacağına ilişkin bazı örnekler aşağıda verilmiştir:

1. Ters proxy yanıt durum kodu 504 (Timeout) döndürür.

    Bunun nedenlerinden biri, hizmetin istek zaman dilimi içinde yanıt vermemesi olabilir.
   Aşağıdaki ilk olay ters proxy alınan istek ayrıntılarını kaydeder. 
   İkinci olay, "iç hata = ERROR_WINHTTP_TIMEOUT" nedeniyle hizmete iletilirken isteğin başarısız olduğunu gösterir 

    Taşıma yükü şunları içerir:

   * **traceId**: Bu GUID, tek bir isteğe karşılık gelen tüm olayları ilişkilendirmek için kullanılabilir. Aşağıdaki iki olay, traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, aynı istek ait ima.
   * **requestUrl**: İsteğin gönderildiği URL (Ters proxy URL'si).
   * **fiil**: HTTP fiili.
   * **remoteAddress**: İstek gönderen müşterinin adresi.
   * **resolvedServiceUrl**: Gelen isteğin çözüldüğü hizmet bitiş noktası URL'si. 
   * **hataAyrıntılar**: Hata hakkında ek bilgi.

     ```
     {
     "Timestamp": "2017-07-20T15:57:59.9871163-07:00",
     "ProviderName": "Microsoft-ServiceFabric",
     "Id": 51477,
     "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Request url = https://localhost:19081/LocationApp/LocationFEService?zipcode=98052, verb = GET, remote (client) address = ::1, resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052, request processing start time =     15:58:00.074114 (745,608.196 MSec) ",
     "ProcessId": 57696,
     "Level": "Informational",
     "Keywords": "0x1000000000000021",
     "EventName": "ReverseProxy",
     "ActivityID": null,
     "RelatedActivityID": null,
     "Payload": {
      "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
      "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?zipcode=98052",
      "verb": "GET",
      "remoteAddress": "::1",
      "resolvedServiceUrl": "Https://localhost:8491/LocationApp/?zipcode=98052",
      "requestStartTime": "2017-07-20T15:58:00.0741142-07:00"
     }
     }

     {
     "Timestamp": "2017-07-20T16:00:01.3173605-07:00",
     ...
     "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request to service: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
     ...
     "Payload": {
      "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
      "statusCode": 504,
      "description": "Reverse Proxy Timeout",
      "sendRequestPhase": "FinishSendRequest",
      "errorDetails": "internal error = ERROR_WINHTTP_TIMEOUT"
     }
     }
     ```

2. Ters proxy yanıt durum kodu 404 (Bulunamadı) döndürür. 
    
    Burada eşleşen hizmet bitiş noktasını bulmak için başarısız olduğundan ters proxy 404 döndürür örnek bir olaydır.
    Burada ilgi yük girişleri şunlardır:
   * **processRequestPhase**: Hata oluştuğunda istek işleme sırasında ki aşamayı gösterir, ***TryGetEndpoint*** yani servis bitiş noktasını iletmeye çalışırken. 
   * **hataAyrıntılar**: Uç nokta arama ölçütlerini listeler. Burada listenerName belirtilen görebilirsiniz = **FrontEndListener** çoğaltma bitiş noktası listesi sadece adı **OldListener**ile bir dinleyici içerir .
    
     ```
     {
     ...
     "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward to service: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
     "ProcessId": 57696,
     "Level": "Warning",
     "EventName": "ReverseProxy",
     "Payload": {
      "traceId": "c1cca3b7-f85d-4fef-a162-88af23604343",
      "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?ListenerName=NewListener&zipcode=98052",
      ...
      "processRequestPhase": "TryGetEndoint",
      "errorDetails": "SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Https:\/\/localhost:8491\/LocationApp\/\"}}"
     }
     }
     ```
     Ters proxy 404 Bulunamadı döndürebilir başka bir örnek: ApplicationGateway\Http yapılandırma parametresi **SecureOnlyMode** **https**ters proxy dinleme ile doğru ayarlanır , ancak tüm çoğaltma bitiş noktaları güvenli değildir (HTTP dinleme).
     Ters proxy, isteği iletmek için HTTPS'de dinlemenin bitiş noktası bulamadığı için 404'ü döndürür. Olay yükündeki parametrelerin çözümlenmesi sorunu daraltmaya yardımcı olur:
    
     ```
      "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
     ```

3. Ters proxy isteği bir zaman alahatası ile başarısız olur. 
    Olay günlükleri alınan istek ayrıntılarını içeren bir olay içerir (burada gösterilmez).
    Sonraki olay, hizmetin 404 durum koduyla yanıt verdiğini ve ters proxy'nin yeniden çözümleme sini başlattığını gösterir. 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request to service returned: status code = 404, status description = , Reresolving ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "statusCode": 404,
        "statusDescription": ""
      }
    }
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Re-resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052 ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "requestUrl": "Https://localhost:8491/LocationApp/?zipcode=98052"
      }
    }
    ```
    Tüm olayları toplarken, her çözüm ve ileri girişimini gösteren bir olaylar treni görürsünüz.
    Serinin son olayı, istek işlemenin bir zaman adabıyla birlikte başarısız olduğunu ve başarılı çözüm denemelerinin sayısını gösterir.
    
    > [!NOTE]
    > Ayrıntılı kanal olay koleksiyonunu varsayılan olarak devre dışı bırakmanız ve gereksinim bazında sorun giderme için etkinleştirme önerilir.

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Error while processing request: number of successful resolve attempts = 12, error = FABRIC_E_TIMEOUT, message = , phase = ResolveServicePartition,  ",
      "EventName": "ReverseProxy",
      ...
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "resolveCount": 12,
        "errorval": -2147017729,
        "errorMessage": "",
        "processRequestPhase": "ResolveServicePartition",
        "errorDetails": ""
      }
    }
    ```
    
    Yalnızca kritik/hata olayları için koleksiyon etkinleştirilirse, zaman anına ilişkin ayrıntıları ve çözüm denemelerinin sayısıyla ilgili ayrıntıları içeren bir olay görürsünüz. 
    
    Kullanıcıya 404 durum kodunu geri göndermeyi amaçlayan hizmetlerin yanıtına bir "X-ServiceFabric" üstbilgisi eklemesi gerekir. Üstbilgi yanıta eklendikten sonra, ters proxy durum kodunu istemciye geri iletir.  

4. İstemcinin isteği kestiği durumlar.

    Ters proxy yanıtı istemciye iletirken aşağıdaki olay kaydedilir, ancak istemci keser:

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable to send response to client: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
      "ProcessId": 57696,
      "Level": "Warning",
      ...
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8",
        "sendResponsePhase": "SendResponseHeaders",
        "errorval": -805306367,
        "winHttpError": "ERROR_SUCCESS"
      }
    }
    ```
5. Ters Proxy 404 FABRIC_E_SERVICE_DOES_NOT_EXIST döndürür

    hizmet bildirimindeki hizmet bitiş noktası için URI düzeni belirtilmemişse FABRIC_E_SERVICE_DOES_NOT_EXIST hatası döndürülür.

    ```
    <Endpoint Name="ServiceEndpointHttp" Port="80" Protocol="http" Type="Input"/>
    ```

    Sorunu çözmek için, bildirimde URI düzenini belirtin.
    ```
    <Endpoint Name="ServiceEndpointHttp" UriScheme="http" Port="80" Protocol="http" Type="Input"/>
    ```

> [!NOTE]
> Websocket istek işleme ile ilgili olaylar şu anda günlüğe kaydedilmedi. Bu, bir sonraki sürümde eklenecektir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure kümelerinde günlük toplamayı etkinleştirmek için [Windows Azure Tanılama'yı kullanarak olay toplama ve toplama.](service-fabric-diagnostics-event-aggregation-wad.md)
* Visual Studio'daki Service Fabric olaylarını görüntülemek için Monitör'e bakın [ve yerel olarak tanıkoyun.](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* Farklı hizmet sertifikası doğrulama seçenekleriyle güvenli ters proxy yapılandırmak için Azure Kaynak Yöneticisi şablon örnekleriiçin güvenli hizmetlere bağlanmak için [ters proxy'yi yapılandırmaya](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Reverse-Proxy-Sample#configure-reverse-proxy-to-connect-to-secure-services) bakın.
* Daha fazla bilgi edinmek için [Service Fabric ters proxy'yi](service-fabric-reverseproxy.md) okuyun.
