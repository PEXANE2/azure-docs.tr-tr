---
title: Dayanıklı İşlevler (Python)-Azure 'da izleyiciler
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nı (Python) kullanarak bir durum İzleyicisi uygulamayı öğrenin.
author: davidmrdavid
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: azfuncdf
ms.openlocfilehash: 62b3c9bb1c6fd53d9f11227a9d7e774d56859d04
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102434772"
---
# <a name="monitor-scenario-in-durable-functions---github-issue-monitoring-sample"></a>Dayanıklı İşlevler-GitHub sorun izleme örneğinde izleme senaryosu

İzleyici stili, bir iş akışında esnek bir yinelenen işleme anlamına gelir. Örneğin, belirli koşullar karşılanana kadar yoklama yapar. Bu makalede, izlemeyi uygulamak için Dayanıklı İşlevler kullanan bir örnek açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç makalesini doldurun](quickstart-python-vscode.md)
* [Örnek projeyi GitHub 'dan kopyalayın veya indirin](https://github.com/Azure/azure-functions-durable-python/tree/main/samples/)


## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnek, GitHub deposunda sorun sayısını izler ve 3 ' ten fazla açık sorun varsa kullanıcıyı uyarır. Düzenli aralıklarla açılmış sorun sayılarını istemek için, normal bir Zamanlayıcı tarafından tetiklenen bir işlev kullanabilirsiniz. Ancak, Bu yaklaşımdaki bir sorun **ömür yönetirüdir**. Yalnızca bir uyarının gönderilmesi gerekiyorsa, izleme 3 veya daha fazla sorun algılandıktan sonra kendisini devre dışı bırakmanız gerekir. İzleme deseninin diğer avantajları arasında kendi yürütmesi sona erdirmek için:

* İzleyiciler, zamanlama değil, aralıklar üzerinde çalışır: bir Zamanlayıcı tetikleyicisi her saat *çalışır* ; izleyici, eylemler arasında bir saat *bekler* . Bir izleyicinin eylemleri belirtilmediyse, bu işlemler, uzun süre çalışan görevler için önemli olabilen, belirtilmediği sürece örtüşmeyecektir.
* İzleyicilerin dinamik aralıkları olabilir: bekleme süresi bazı koşula göre değişebilir.
* İzleyiciler, bazı koşullar karşılandığında veya başka bir işlem tarafından sonlandırılırsa sonlandırılabilir.
* İzleyiciler parametre alabilir. Örnek, aynı depo izleme işleminin istenen genel GitHub deposu ve telefon numarasına nasıl uygulanabileceğini gösterir.
* İzleyiciler ölçeklenebilir. Her izleyici bir düzenleme örneği olduğundan, yeni işlevler oluşturmak veya daha fazla kod tanımlamak zorunda kalmadan birden fazla izleyici oluşturulabilir.
* İzleyiciler daha büyük iş akışlarıyla kolayca tümleşir. İzleyici, daha karmaşık bir düzenleme işlevinin veya [alt Orchestration](durable-functions-sub-orchestrations.md)'un bir bölümü olabilir.

## <a name="configuration"></a>Yapılandırma

### <a name="configuring-twilio-integration"></a>Twilio tümleştirmesini yapılandırma

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E3_Monitor`: Düzenli aralıklarla çağıran bir [Orchestrator işlevi](durable-functions-bindings.md#orchestration-trigger) `E3_TooManyOpenIssues` . `E3_SendAlert`Öğesinin dönüş değeri olup olmadığını çağırır `E3_TooManyOpenIssues` `True` .
* `E3_TooManyOpenIssues`: Bir deponun çok fazla açık soruna sahip olup olmadığını denetleyen bir [etkinlik işlevi](durable-functions-bindings.md#activity-trigger) . Göz önünde bulundurulması için 3 ' ten fazla açık sorunu çok fazla olacak şekilde ele alalım.
* `E3_SendAlert`: Twilio aracılığıyla SMS iletisi gönderen bir etkinlik işlevi.

### <a name="e3_monitor-orchestrator-function"></a>E3_Monitor Orchestrator işlevi


**E3_Monitor** işlevi, Orchestrator işlevleri için *üzerinde standartfunction.js* kullanır.

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_Monitor/function.json)]

İşlevi uygulayan kod aşağıda verilmiştir:

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_Monitor/\_\_init\_\_.py)]


Bu Orchestrator işlevi aşağıdaki eylemleri gerçekleştirir:

1. İzlenecek depoyu *ve* SMS bildirimi gönderecek *telefon numarasını* alır.
2. İzleyicinin sona erme süresini belirler. Örnek, breçekimi için sabit kodlanmış bir değer kullanır.
3. İstenen depoda çok fazla açık sorun olup olmadığını belirleme **E3_TooManyOpenIssues** çağırır.
4. Çok sayıda sorun varsa, istenen telefon numarasına SMS bildirimi göndermek için **E3_SendAlert** çağırır.
5. Bir sonraki yoklama aralığında Orchestration işlemini sürdürecek dayanıklı bir zamanlayıcı oluşturur. Örnek, breçekimi için sabit kodlanmış bir değer kullanır.
6. Geçerli UTC saati izleyicinin sona erme zamanını geçirene veya bir SMS uyarısı gönderildiğinde çalışmaya devam eder.

Orchestrator işlevini birden çok kez çağırarak, birden fazla Orchestrator örneği aynı anda çalışabilir. İzlenecek depo ve SMS uyarısı gönderilecek telefon numarası belirtilebilir. Son olarak, Orchestrator işlevinin Zamanlayıcı *beklenirken çalışmadığını,* bu nedenle ücretlendirilmemek istediğinizi unutmayın.


### <a name="e3_toomanyopenissues-activity-function"></a>E3_TooManyOpenIssues Activity işlevi

Diğer örneklerde olduğu gibi, yardımcı etkinlik işlevleri de tetikleyici bağlamayı kullanan normal işlevlerdir `activityTrigger` . **E3_TooManyOpenIssues** işlevi, depoda Şu anda açık olan sorunların bir listesini alır ve "çok fazla" olup olmadığını belirler: örneğimize göre 3 ' ten fazla.


*function.js* , aşağıdaki gibi tanımlanır:

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/function.json)]

İşte uygulama.

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/\_\_init\_\_.py)]


### <a name="e3_sendalert-activity-function"></a>E3_SendAlert Activity işlevi

**E3_SendAlert** işlevi, son kullanıcıya bir çözümü bekleyen en az 3 açık sorun olduğunu BILDIREN bir SMS iletisi göndermek için Twilio bağlamasını kullanır.


*Üzerindefunction.js* basittir:

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/function.json)]

SMS iletisini gönderen kod aşağıda verilmiştir:

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_SendAlert/\_\_init\_\_.py)]


## <a name="run-the-sample"></a>Örneği çalıştırma

Bir [GitHub](https://github.com/) hesabına ihtiyacınız olacak. Bununla birlikte, sorunları açabileceğiniz geçici bir genel depo oluşturun.

Örneğe dahil edilen HTTP ile tetiklenen işlevleri kullanarak, aşağıdaki HTTP POST isteğini göndererek düzenleme işlemini başlatabilirsiniz:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "repo": "<your github handle>/<a new github repo under your user>", "phone": "+1425XXXXXXX" }
```

Örneğin, GitHub Kullanıcı adınız `foo` ve deponuz varsa, sizin `bar` için değer `"repo"` olmalıdır `"foo/bar"` .

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

**E3_Monitor** örnek başlatılır ve açık sorunlar için sağlanmış depoyu sorgular. En az 3 açık sorun bulunursa, bir uyarı göndermek için bir etkinlik işlevi çağırır; Aksi takdirde, bir Zamanlayıcı ayarlar. Süreölçerin süresi dolduğunda düzenleme sürdürülecek.

Azure Işlevleri portalındaki işlev günlüklerine bakarak Orchestration 'un etkinliğini görebilirsiniz.

```
[2020-12-04T18:24:30.007Z] Executing 'Functions.HttpStart' (Reason='This function was programmatically 
called via the host APIs.', Id=93772f6b-f4f0-405a-9d7b-be9eb7a38aa6)
[2020-12-04T18:24:30.769Z] Executing 'Functions.E3_Monitor' (Reason='(null)', Id=058e656e-bcb1-418c-95b3-49afcd07bd08)
[2020-12-04T18:24:30.847Z] Started orchestration with ID = '788420bb31754c50acbbc46e12ef4f9c'.
[2020-12-04T18:24:30.932Z] Executed 'Functions.E3_Monitor' (Succeeded, Id=058e656e-bcb1-418c-95b3-49afcd07bd08, Duration=174ms)
[2020-12-04T18:24:30.955Z] Executed 'Functions.HttpStart' (Succeeded, Id=93772f6b-f4f0-405a-9d7b-be9eb7a38aa6, Duration=1028ms)
[2020-12-04T18:24:31.229Z] Executing 'Functions.E3_TooManyOpenIssues' (Reason='(null)', Id=6fd5be5e-7f26-4b0b-98df-c3ac39125da3)
[2020-12-04T18:24:31.772Z] Executed 'Functions.E3_TooManyOpenIssues' (Succeeded, Id=6fd5be5e-7f26-4b0b-98df-c3ac39125da3, Duration=555ms)
[2020-12-04T18:24:40.754Z] Executing 'Functions.E3_Monitor' (Reason='(null)', Id=23915e4c-ddbf-46f9-b3f0-53289ed66082)
[2020-12-04T18:24:40.789Z] Executed 'Functions.E3_Monitor' (Succeeded, Id=23915e4c-ddbf-46f9-b3f0-53289ed66082, Duration=38ms)
(...trimmed...)
```

Zaman aşımı süresine ulaşıldığında veya 3 ' ten fazla açık sorun saptandıktan sonra Orchestration tamamlanacaktır. `terminate`API 'yi başka bir işlev içinde de kullanabilir veya yukarıdaki 202 yanıtında belirtilen **Terminateposturi** http post Web kancasını çağırabilirsiniz. Web kancasını kullanmak için, `{text}` erken sonlandırma nedeni ile değiştirin. HTTP POST URL 'SI kabaca şöyle görünür:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, bir dış kaynağın durumunu [dayanıklı zamanlayıcılar](durable-functions-timers.md) ve koşullu mantık kullanarak izlemek için dayanıklı işlevler nasıl kullanılacağı gösterilmiştir. Sonraki örnek, insan etkileşimini işlemek için dış olayların ve [dayanıklı zamanlayıcıları](durable-functions-timers.md) nasıl kullanacağınızı gösterir.

> [!div class="nextstepaction"]
> [İnsan etkileşimi örneğini çalıştırma](durable-functions-phone-verification.md)
