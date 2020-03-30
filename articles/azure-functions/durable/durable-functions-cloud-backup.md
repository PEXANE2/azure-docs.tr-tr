---
title: Dayanıklı İşlevlerde Fan-out/fan-in senaryoları - Azure
description: Azure İşlevler için Dayanıklı Fonksiyonlar uzantısında fan-out-fan-in senaryosunu nasıl uygulayacağınızı öğrenin.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: d61600801286126ea6ffb9a97bc5655b6f233816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562199"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Dayanıklı Fonksiyonlar'da fan-out/fan-in senaryosu - Bulut yedekleme örneği

*Fan-out/fan-in* aynı anda birden fazla işlevi yürütme ve daha sonra sonuçlar üzerinde bazı toplama gerçekleştirme deseni anlamına gelir. Bu makalede, fan-in/fan-out senaryosu uygulamak için [Dayanıklı Fonksiyonlar](durable-functions-overview.md) kullanan bir örnek açıklanmaktadır. Örnek, bir uygulamanın site içeriğinin tamamını veya bir kısmını Azure Depolama'ya yedekleyen dayanıklı bir işlevdir.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnekte, işlevler tüm dosyaları belirli bir dizin altında yinelemeli olarak blob depolamaalanına yükler. Ayrıca yüklenen baytların toplam sayısını da sayar.

Her şeyi halleden tek bir işlev yazmak mümkündür. Karşınıza olacak temel sorun **ölçeklenebilirlik.** Tek bir işlev yürütme yalnızca tek bir sanal makinede çalışabilir, bu nedenle iş, tek bir VM'nin iş bölümüyle sınırlı olacaktır. Başka bir sorun **güvenilirlik**. Yolun ortasında bir hata varsa veya tüm işlem 5 dakikadan uzun sürüyorsa, yedekleme kısmen tamamlanmış bir durumda başarısız olabilir. Daha sonra yeniden başlatılması gerekir.

Daha sağlam bir yaklaşım iki düzenli işlevleri yazmak olacaktır: bir dosyaları sıralamak ve bir kuyruğa dosya adları eklemek, ve başka bir kuyruk okumak ve blob depolama dosyaları yüklemek olacaktır. Bu yaklaşım, iş ve güvenilirlik açısından daha iyidir, ancak bir sıra sağlamanızı ve yönetmeniz için gereklidir. Daha da önemlisi, yüklenen toplam bayt sayısını rapor etmek gibi daha fazlasını yapmak istiyorsanız, **devlet yönetimi** ve **koordinasyonu** açısından önemli bir karmaşıklık ortaya konmuş olur.

Dayanıklı Fonksiyonlar yaklaşımı, çok düşük genel merkezlerle belirtilen tüm avantajları sağlar.

## <a name="the-functions"></a>Fonksiyonlar

Bu makalede, örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E2_BackupSiteContent`: Yedeklenebilmek için `E2_GetFileList` dosyaların listesini almak için çağıran, `E2_CopyFileToBlob` sonra her dosyayı yedeklemek için çağıran bir [orkestratör işlevi.](durable-functions-bindings.md#orchestration-trigger)
* `E2_GetFileList`: Dizindeki dosyaların listesini döndüren bir [etkinlik işlevi.](durable-functions-bindings.md#activity-trigger)
* `E2_CopyFileToBlob`: Tek bir dosyayı Azure Blob Depolama'ya yedekleyen bir etkinlik işlevi.

### <a name="e2_backupsitecontent-orchestrator-function"></a>E2_BackupSiteContent orkestratör fonksiyonu

Bu orkestratör işlevi aslında aşağıdakileri yapar:

1. Bir `rootDirectory` girdi parametresi olarak bir değer alır.
2. `rootDirectory`Altında özyinelemeli dosyaların listesini almak için bir işlev çağırır.
3. Her dosyayı Azure Blob Depolama'ya yüklemek için birden çok paralel işlev çağrısı yapar.
4. Tüm yüklemelerin tamamlanmasını bekler.
5. Azure Blob Depolama'ya yüklenen toplam bayt ları döndürür.

# <a name="c"></a>[C #](#tab/csharp)

Orkestratör işlevini uygulayan kod aşağıda verebedilir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=16-42)]

Çizgiye `await Task.WhenAll(tasks);` dikkat et. `E2_CopyFileToBlob` İşlev için tüm bireysel aramalar, onları paralel çalışmasını sağlar *beklenmgündür.* Bu görev dizisini şu `Task.WhenAll`şekilde geçtiğimizde, tüm kopyalama *işlemleri tamamlanana kadar*tamamlanmayacak bir görevi geri alAbiliriz. .NET'teki Görev Paralel Kitaplığı'nı (TPL) biliyorsanız, bu sizin için yeni bir şey değildir. Aradaki fark, bu görevlerin aynı anda birden çok sanal makinede çalışıyor olması ve Dayanıklı İşlevler uzantısı uçlardan uca yürütmenin geri dönüşümü işlemeye karşı esnek olmasını sağlamasıdır.

Bekledikten `Task.WhenAll`sonra, tüm işlev çağrılarının tamamlandığını ve değerleri bize geri döndürdettiğini biliyoruz. Yüklenen bayt sayısını döndürecek `E2_CopyFileToBlob` her çağrı, bu nedenle toplam bayt sayısının hesaplanması, tüm bu iade değerlerini bir araya ekleme meselesidir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

İşlev, orkestratör işlevleri için standart *function.json* kullanır.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/function.json)]

Orkestratör işlevini uygulayan kod aşağıda verebedilir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

Çizgiye `yield context.df.Task.all(tasks);` dikkat et. `E2_CopyFileToBlob` İşlev için tüm bireysel aramalar, onları paralel çalışmasını sağlar teslim *edilmedi.* Bu görev dizisini şu `context.df.Task.all`şekilde geçtiğimizde, tüm kopyalama *işlemleri tamamlanana kadar*tamamlanmayacak bir görevi geri alAbiliriz. [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) JavaScript'te aşinaysanız, bu sizin için yeni bir şey değildir. Aradaki fark, bu görevlerin aynı anda birden çok sanal makinede çalışıyor olması ve Dayanıklı İşlevler uzantısı uçlardan uca yürütmenin geri dönüşümü işlemeye karşı esnek olmasını sağlamasıdır.

> [!NOTE]
> Görevler kavramsal olarak JavaScript vaatlerine benzer olsa da, `context.df.Task.any` orkestratör işlevleri görev paralelleştirmesi yerine `Promise.all` ve `Promise.race` yönetmek yerine kullanmalıdır. `context.df.Task.all`

'den `context.df.Task.all`sonra, tüm işlev çağrıları tamamlanmış ve bize geri değerleri döndü biliyorum. Yüklenen bayt sayısını döndürecek `E2_CopyFileToBlob` her çağrı, bu nedenle toplam bayt sayısının hesaplanması, tüm bu iade değerlerini bir araya ekleme meselesidir.

---

### <a name="helper-activity-functions"></a>Yardımcı etkinlik işlevleri

Yardımcı etkinlik işlevleri, diğer örneklerde olduğu gibi, tetikleyici `activityTrigger` bağlamayı kullanan normal işlevlerdir.

#### <a name="e2_getfilelist-activity-function"></a>E2_GetFileList etkinlik fonksiyonu

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=44-54)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

*Function.json* dosyası `E2_GetFileList` aşağıdaki gibi görünür:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/function.json)]

Ve burada uygulama:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

`readdirp` İşlev, dizin yapısını özyinelemeli olarak okumak için modülü (sürüm 2.x) kullanır.

---

> [!NOTE]
> Bu kodu neden doğrudan orkestratör işlevine koyamadığınızı merak ediyor olabilirsiniz. Yapabilirdiniz, ama bu orkestratör işlevlerinin temel kurallarından birini bozar, yani yerel dosya sistemi erişimi de dahil olmak üzere asla G/Ç yapmamaları gerekir. Daha fazla bilgi için [Orchestrator işlev kodu kısıtlamalarına](durable-functions-code-constraints.md)bakın.

#### <a name="e2_copyfiletoblob-activity-function"></a>E2_CopyFileToBlob etkinlik fonksiyonu

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=56-81)]

> [!NOTE]
> Örnek kodu çalıştırmak `Microsoft.Azure.WebJobs.Extensions.Storage` için NuGet paketini yüklemeniz gerekir.

İşlev, Azure İşlevleri bağlamalarının bazı gelişmiş özelliklerini (diğer bir şekilde [ `Binder` parametrenin](../functions-dotnet-class-library.md#binding-at-runtime)kullanımı) kullanır, ancak bu gözden geçirme nin amacı için bu ayrıntılar hakkında endişelenmenize gerek yoktur.

# <a name="javascript"></a>[Javascript](#tab/javascript)

*Function.json* dosyası `E2_CopyFileToBlob` benzer şekilde basittir:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/function.json)]

JavaScript uygulaması, dosyaları Azure Blob Depolama'ya yüklemek [için Node için Azure Depolama SDK'sını](https://github.com/Azure/azure-storage-node) kullanır.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

---

Uygulama dosyayı diskten yükler ve içeriği "yedeklemeler" kapsayıcısında aynı adı taşıyan bir blob'a eşit olarak aktarır. İade değeri, depolamaya kopyalanan bayt sayısıdır ve daha sonra toplam toplamı hesaplamak için orkestratör işlevi tarafından kullanılır.

> [!NOTE]
> Bu, G/Ç işlemlerini bir işleve `activityTrigger` taşımanın mükemmel bir örneğidir. Çalışma sadece birçok farklı makineye dağıtılabilir, ama aynı zamanda ilerleme kontrol yararları olsun. Ana bilgisayar işlemi herhangi bir nedenle sonlandırılırsa, hangi yüklemelerin tamamlandığını bilirsiniz.

## <a name="run-the-sample"></a>Örneği çalıştırma

Aşağıdaki HTTP POST isteğini göndererek orkestrasyona başlayabilirsiniz.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> Çağırmakta olduğunuz `HttpStart` işlev yalnızca JSON biçimlendirilmiş içerikle çalışır. Bu nedenle, `Content-Type: application/json` üstbilgi gereklidir ve dizin yolu JSON dizesi olarak kodlanır. Ayrıca, HTTP snippet tüm HTTP tetikleme `host.json` işlevleri URL'lerinden `api/` varsayılan önek kaldırır dosyada bir giriş olduğunu varsayar. Bu yapılandırmanın biçimlendirmesini örneklerde `host.json` dosyada bulabilirsiniz.

Bu HTTP isteği `E2_BackupSiteContent` orkestratörtetikler ve `D:\home\LogFiles` bir parametre olarak dize geçer. Yanıt, yedekleme işleminin durumunu almak için bir bağlantı sağlar:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

İşlev uygulamanızda kaç günlük dosyası var bağlı olarak, bu işlemin tamamlanması birkaç dakika sürebilir. Önceki HTTP 202 yanıtının `Location` üstbilgisinde URL'yi sorgulayarak en son durumu alabilirsiniz.

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

Bu durumda, işlev hala çalışıyor. Orkestratör durumuna kaydedilen girişi ve son güncellenen zamanı görebilirsiniz. Tamamlanmak üzere anket `Location` yapmak için üstbilgi değerlerini kullanmaya devam edebilirsiniz. Durum "Tamamlandı" olduğunda, aşağıdakilere benzer bir HTTP yanıt değeri görürsünüz:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:26Z"}
```

Şimdi orkestrasyonun tamamladığını ve tamamlanmasının yaklaşık olarak ne kadar zaman aldığını görebilirsiniz. `output` Ayrıca, yaklaşık 450 KB günlüklerin yüklendiğini gösteren alan için bir değer de görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek fan-out/fan-in deseninin nasıl uygulanacağını göstermiştir. Sonraki [örnek, dayanıklı zamanlayıcılar](durable-functions-timers.md)kullanarak monitör deseni nasıl uygulanacağını gösterir.

> [!div class="nextstepaction"]
> [Monitör örneğini çalıştırma](durable-functions-monitor.md)