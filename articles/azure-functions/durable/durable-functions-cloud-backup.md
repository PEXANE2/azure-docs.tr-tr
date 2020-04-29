---
title: Dayanıklı İşlevler-Azure 'da fan-çıkış/fan senaryoları
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında bir fanı-Out senaryosu uygulamayı öğrenin.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: d61600801286126ea6ffb9a97bc5655b6f233816
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77562199"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Dayanıklı İşlevler-bulut yedekleme örneğinde fan-çıkış/fan senaryosu

*Fanı-Out/fanı* , birden fazla işlevi aynı anda yürütmenin ve sonra sonuçlar üzerinde bazı toplamalar gerçekleştiren bir düzene başvurur. Bu makalede, bir fan/fan senaryosu uygulamak için [dayanıklı işlevler](durable-functions-overview.md) kullanan bir örnek açıklanmaktadır. Örnek, bir uygulamanın tüm site içeriğini Azure depolama 'ya yedekleyen dayanıklı bir işlevdir.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnekte, işlevler belirtilen bir dizin altındaki tüm dosyaları yinelemeli olarak BLOB depolama alanına yükler. Ayrıca, karşıya yüklenen toplam bayt sayısını da sayar.

Her şeyi ele alan tek bir işlev yazmak mümkündür. Çalıştırmak istediğiniz ana sorun **ölçeklenebilirlik**. Tek bir işlev yürütmesi yalnızca tek bir sanal makinede çalıştırılabilir, bu nedenle üretilen iş, bu tek VM 'nin verimlilik ile sınırlandırılır. Başka bir sorun **güvenilir**. Hata geçişli bir hata oluşursa veya işlemin tamamı 5 dakikadan fazla sürerse, yedekleme kısmen tamamlanmış durumda başarısız olabilir. Daha sonra yeniden başlatılması gerekir.

Daha sağlam bir yaklaşım, iki normal işlev yazmak olacaktır: biri, dosyaları numaralandırırın ve dosya adlarını bir kuyruğa ekleyecek ve başka bir şekilde kuyruktan yazacak ve dosyaları blob depolamaya yükleyecek. Bu yaklaşım verimlilik ve güvenilirlik açısından daha iyidir, ancak bir kuyruğu sağlamanızı ve yönetmenizi gerektirir. Daha önemlisi, **durum yönetimi** ve **koordinasyonuna** göre daha da fazla bir işlem yapmak istiyorsanız, karşıya yüklenen toplam bayt sayısını raporla gibi önemli karmaşıklıklar sunulmuştur.

Dayanıklı İşlevler bir yaklaşım, tüm bahsedilen avantajları çok düşük ek yük ile sunar.

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E2_BackupSiteContent`: Yedeklenecek dosyaların listesini almak için `E2_GetFileList` çağıran bir `E2_CopyFileToBlob` [Orchestrator işlevi](durable-functions-bindings.md#orchestration-trigger) , ardından her dosyayı yedeklemek için çağırır.
* `E2_GetFileList`: Bir dizindeki dosyaların listesini döndüren bir [etkinlik işlevi](durable-functions-bindings.md#activity-trigger) .
* `E2_CopyFileToBlob`: Tek bir dosyayı Azure Blob depolamaya yedekleyen bir etkinlik işlevi.

### <a name="e2_backupsitecontent-orchestrator-function"></a>E2_BackupSiteContent Orchestrator işlevi

Bu Orchestrator işlevi temelde şunları yapar:

1. Bir `rootDirectory` değeri giriş parametresi olarak alır.
2. Altında `rootDirectory`dosyaların özyinelemeli bir listesini almak için bir işlev çağırır.
3. Her bir dosyayı Azure Blob depolama alanına yüklemek için birden çok paralel işlev çağrısı yapar.
4. Tüm karşıya yüklemelerinin tamamlanmasını bekler.
5. Azure Blob depolamaya karşıya yüklenen toplam bayt sayısını döndürür.

# <a name="c"></a>[, #](#tab/csharp)

Orchestrator işlevini uygulayan kod aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=16-42)]

`await Task.WhenAll(tasks);` Satıra dikkat edin. `E2_CopyFileToBlob` İşleve yapılan tüm çağrılar *beklenmez* , bu da paralel çalışmasına izin verir. Bu görev dizisini ' ye `Task.WhenAll`ilettiğimiz zaman, *tüm kopyalama işlemleri tamamlanana kadar*tamamlanmamış bir görevi geri alırız. .NET 'teki görev paralel kitaplığı (TPL) hakkında bilginiz varsa, bu sizin için yeni değildir. Fark, bu görevlerin eşzamanlı olarak birden çok sanal makinede çalışabilmesidir ve Dayanıklı İşlevler uzantısı uçtan uca yürütmenin geri dönüşüm işleminin işlenmesine dayanıklı olmasını sağlar.

' Den `Task.WhenAll`sonra, tüm işlev çağrılarının tamamlandığını ve bize doğru değerler döndürmediğini biliyoruz. Her bir çağrı `E2_CopyFileToBlob` , karşıya yüklenen bayt sayısını döndürür, bu nedenle toplam bayt sayısını hesaplamak, bu dönüş değerlerinin tümünü birlikte eklemenin bir önemi olur.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşlevi Orchestrator işlevleri için standart *function. JSON* ' i kullanır.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/function.json)]

Orchestrator işlevini uygulayan kod aşağıda verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

`yield context.df.Task.all(tasks);` Satıra dikkat edin. `E2_CopyFileToBlob` İşleve yapılan her bir çağrı, bu işlevin paralel çalışmasına izin veren bir şekilde yapılmadı. *not* Bu görev dizisini ' ye `context.df.Task.all`ilettiğimiz zaman, *tüm kopyalama işlemleri tamamlanana kadar*tamamlanmamış bir görevi geri alırız. JavaScript 'e alışkın [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) değilseniz, bu sizin için yeni bir değildir. Fark, bu görevlerin eşzamanlı olarak birden çok sanal makinede çalışabilmesidir ve Dayanıklı İşlevler uzantısı uçtan uca yürütmenin geri dönüşüm işleminin işlenmesine dayanıklı olmasını sağlar.

> [!NOTE]
> Görevler kavramsal olarak JavaScript 'e benzer olsa da, Orchestrator işlevleri görev `context.df.Task.all` paralelleştirme `context.df.Task.any` 'yı yönetmek `Promise.all` `Promise.race` için ve yerine kullanmalıdır.

' Dan `context.df.Task.all`gönderdikten sonra, tüm işlev çağrılarının tamamlandığını ve bize doğru değerler döndürmediğini biliyoruz. Her bir çağrı `E2_CopyFileToBlob` , karşıya yüklenen bayt sayısını döndürür, bu nedenle toplam bayt sayısını hesaplamak, bu dönüş değerlerinin tümünü birlikte eklemenin bir önemi olur.

---

### <a name="helper-activity-functions"></a>Yardımcı etkinlik işlevleri

Yardımcı etkinlik işlevleri, diğer örneklerde olduğu gibi, yalnızca `activityTrigger` tetikleyici bağlamayı kullanan normal işlevlerdir.

#### <a name="e2_getfilelist-activity-function"></a>E2_GetFileList Activity işlevi

# <a name="c"></a>[, #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=44-54)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`E2_GetFileList` İçin *function. JSON* dosyası aşağıdakine benzer şekilde görünür:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/function.json)]

Uygulama şu şekildedir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

İşlev, dizin yapısını `readdirp` özyinelemeli olarak okumak için modülünü (sürüm 2. x) kullanır.

---

> [!NOTE]
> Bu kodu neden henüz Orchestrator işlevine koyacağınızı merak ediyor olabilirsiniz. Ancak bu, Orchestrator işlevlerinin temel kurallarından birini bozacağından, yerel dosya sistemi erişimi de dahil olmak üzere hiç g/ç yapmam gerekir. Daha fazla bilgi için bkz. [Orchestrator işlev kodu kısıtlamaları](durable-functions-code-constraints.md).

#### <a name="e2_copyfiletoblob-activity-function"></a>E2_CopyFileToBlob Activity işlevi

# <a name="c"></a>[, #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=56-81)]

> [!NOTE]
> Örnek kodu çalıştırmak için `Microsoft.Azure.WebJobs.Extensions.Storage` NuGet paketini yüklemeniz gerekir.

İşlevi, Azure işlevleri bağlamalarının bazı gelişmiş özelliklerini (yani, [ `Binder` parametresinin](../functions-dotnet-class-library.md#binding-at-runtime)kullanımı) kullanır, ancak bu izlenecek yol için bu ayrıntılar konusunda endişelenmenize gerek yoktur.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`E2_CopyFileToBlob` İçin *function. JSON* dosyası benzer bir işlemdir:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/function.json)]

JavaScript uygulama, dosyaları Azure Blob depolama alanına yüklemek için [Azure depolama SDK 'sını](https://github.com/Azure/azure-storage-node) kullanır.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

---

Uygulama, dosyayı diskten yükler ve içeriği zaman uyumsuz olarak "yedeklemeler" kapsayıcısında aynı ada sahip bir bloba akıtır. Dönüş değeri, depolamaya kopyalanmış baytların sayısıdır, daha sonra Orchestrator işlevi tarafından toplam toplamı hesaplamak için kullanılır.

> [!NOTE]
> Bu, g/ç işlemlerini bir `activityTrigger` işleve taşımaya yönelik kusursuz bir örnektir. İş yalnızca birçok farklı makineye dağıtılmayabilir, ancak ilerlemeyi gösterme avantajlarının avantajlarından de yararlanabilirsiniz. Ana bilgisayar işlemi herhangi bir nedenle sonlandırılırsa, hangi karşıya yüklemelerinin zaten tamamlandığını bilirsiniz.

## <a name="run-the-sample"></a>Örneği çalıştırma

Aşağıdaki HTTP POST isteğini göndererek düzenleme işlemini başlatabilirsiniz.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> Çağırdığınız `HttpStart` Işlev yalnızca JSON biçimli içerikle çalışır. Bu nedenle, `Content-Type: application/json` üst bilgi gereklidir ve dizin yolu bir JSON dizesi olarak kodlanır. Ayrıca, HTTP kod parçacığı `host.json` dosyada, tüm http tetikleyici Işlevleri URL 'lerinden varsayılan `api/` öneki kaldıran bir giriş olduğunu varsayar. Bu yapılandırmanın işaretlemesini, örneklerdeki `host.json` dosyada bulabilirsiniz.

Bu HTTP isteği, `E2_BackupSiteContent` Orchestrator 'ı tetikler ve dizeyi `D:\home\LogFiles` bir parametre olarak geçirir. Yanıt, yedekleme işleminin durumunu almak için bir bağlantı sağlar:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

İşlev uygulamanızda kaç günlük dosyasına sahip olduğunuza bağlı olarak, bu işlemin tamamlanması birkaç dakika sürebilir. Önceki HTTP 202 yanıtının `Location` üstbilgisinde URL 'yi sorgulayarak en son durumu alabilirsiniz.

```
GET http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:16Z"}
```

Bu durumda, işlev hala çalışıyor olur. Orchestrator durumuna kaydedilen girişi ve son güncelleme saatini görebilirsiniz. Tamamlamayı yoklamak için `Location` üst bilgi değerlerini kullanmaya devam edebilirsiniz. Durum "tamamlandı" olduğunda aşağıdakine benzer bir HTTP yanıt değeri görürsünüz:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:26Z"}
```

Şimdi Orchestration 'un tamamlandığını ve tamamlanması için ne kadar zaman tamamlandığını görebilirsiniz. `output` Alan için bir değer de görürsünüz. bu, 450 KB 'lık Günlükler etrafında karşıya yüklendiğini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, fan/fan deseninin nasıl uygulanacağını göstermiştir. Sonraki örnek, [dayanıklı zamanlayıcılar](durable-functions-timers.md)kullanılarak izleyici deseninin nasıl uygulanacağını gösterir.

> [!div class="nextstepaction"]
> [İzleme örneğini çalıştırma](durable-functions-monitor.md)