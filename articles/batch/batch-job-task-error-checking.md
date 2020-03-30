---
title: İş ve görev hatalarını denetleyin - Azure Toplu İş | Microsoft Dokümanlar
description: Denetlenen hatalar ve iş ve görevlerin sorun giderme
services: batch
author: mscurrell
ms.service: batch
ms.topic: article
ms.date: 03/10/2019
ms.author: markscu
ms.openlocfilehash: 4ace0de6d252680eb64990277b9478adf752f54d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087007"
---
# <a name="job-and-task-error-checking"></a>İş ve görev hatası denetimi

İş ve görev eklerken oluşabilecek çeşitli hatalar vardır. Bu işlemler için hataları algılamak basittir, çünkü herhangi bir hata API, CLI veya UI tarafından hemen döndürülür.  Ancak, işler ve görevler zamanlanıp çalıştırıldığında daha sonra meydana gelebilecek hatalar vardır.

Bu makalede, işler ve görevler gönderildikten sonra oluşabilecek hataları kapsar. Denetlenmeleri ve işlenmeleri gereken hataları listeler ve açıklar.

## <a name="jobs"></a>İşler

İş, bir veya daha fazla görevin gruplandırması, fiilen yürütülecek komut satırlarını belirten görüşmelerdir.

Bir iş eklerken, işin nasıl başarısız olabileceğini etkileyebilecek aşağıdaki parametreler belirtilebilir:

- [İş Kısıtlamaları](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)
  - Özellik `maxWallClockTime` isteğe bağlı olarak, bir işin etkin veya çalıştırılan maksimum süreyi ayarlamak için belirtilebilir. Aşıldığında, iş için [executionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#cloudjob) `terminateReason` ayarlanmış özellik ile iş sonlandırılır.
- [İş Hazırlama Görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask)
  - Belirtilirse, bir görev hazırlama görevi ilk kez bir düğüm üzerinde bir iş için çalıştırılır çalıştırılır. İş hazırlama görevi başarısız olabilir ve bu da görevin çalıştırılmamasına ve işin tamamlanmamasıyla sonuçlandırılabilir.
- [İş Bırakma Görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask)
  - Bir iş yayımlama görevi yalnızca bir iş hazırlama görevi yapılandırılırsa belirtilebilir. Bir iş sonlandırıldığında, iş bırakma görevi, iş hazırlama görevinin çalıştırıldığı havuz düğümlerinin her birinde çalıştırılır. Bir iş bırakma görevi başarısız olabilir, ancak `completed` iş yine de bir duruma taşınır.

### <a name="job-properties"></a>İş özellikleri

Aşağıdaki iş özellikleri hataları için denetlenmelidir:

- '[executionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#jobexecutioninformation)':
  - `maxWallClockTime`Özellik, `terminateReason` iş kısıtlamalarında belirtilen , aşıldığını ve bu nedenle işin sonlandırıldığını gösteren değerlere sahip olabilir. Ayrıca, iş `onTaskFailure` özelliği uygun şekilde ayarlanmışsa, bir görevin başarısız olduğunu belirtmek için de ayarlanabilir.
  - [Zamanlama Hatası](https://docs.microsoft.com/rest/api/batchservice/job/get#jobschedulingerror) özelliği, bir zamanlama hatası varsa ayarlanır.
 
### <a name="job-preparation-tasks"></a>İş hazırlama görevleri

Bir iş için iş hazırlama görevi belirtilirse, bu görevin bir örneği, iş için ilk görev düğümünde çalıştırılın. İş üzerinde yapılandırılan iş hazırlama görevi, birden çok iş hazırlama görev örneklerinin çalıştırıldığı bir görev şablonu olarak, havuzdaki düğüm sayısına kadar düşünülebilir.

Hata olup olmadığını belirlemek için iş hazırlama görev örnekleri denetlenmelidir:
- Bir iş hazırlama görevi çalıştırıldığında, iş hazırlama görevini tetikleyen görev [state](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate) bir `preparing`duruma taşınır; iş hazırlama görevi başarısız olursa, tetikleyici görev `active` duruma geri döner ve çalıştırılmaz.  
- Çalıştırılan iş hazırlama görevinin tüm örnekleri Liste Hazırlama ve [Sürüm Görev Durumu](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) API'sini kullanarak işten elde edilebilir. Herhangi bir görevde olduğu [execution information](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) gibi, `failureInfo`yürütme bilgileri `exitCode`, `result`, ve .
- İş hazırlama görevleri başarısız olursa, tetikleyen iş görevleri çalıştırılmayır, iş tamamlanmaz ve sıkışmış olur. Zamanlanabilecek görevler olan başka işler yoksa havuz kullanılmamış olabilir.

### <a name="job-release-tasks"></a>İş bırakma görevleri

Bir iş için bir iş bırakma görevi belirtilirse, bir iş sonlandırıldığında, iş bırakma görevinin bir örneği, iş hazırlama görevinin çalıştırıldığı havuz düğümlerinin her birinde çalıştırılır.  Hata olup olmadığını belirlemek için iş serbest bırakma görev örnekleri denetlenmelidir:
- Çalıştırılan iş bırakma görevinin tüm örnekleri API [Listesi Hazırlama ve Sürüm Görev Durumu](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus)kullanılarak iş elde edilebilir. Herhangi bir görevde olduğu [execution information](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) gibi, `failureInfo`yürütme bilgileri `exitCode`, `result`, ve .
- Bir veya daha fazla iş bırakma görevi başarısız olursa, iş `completed` yine de sonlandırılır ve bir duruma taşınır.

## <a name="tasks"></a>Görevler

İş görevleri birden çok nedenden dolayı başarısız olabilir:

- Görev komut satırı, sıfır olmayan bir çıkış koduyla döndürülerek başarısız olur.
- Bir `resourceFiles` görev için belirtilen, ancak bir veya daha fazla dosya indirmedi anlamına gelen bir hata vardı.
- Bir `outputFiles` görev için belirtilen, ancak bir veya daha fazla dosya yüklemedi anlamına gelen bir hata vardı.
- Görev `maxWallClockTime` [kısıtlamalarındaki](https://docs.microsoft.com/rest/api/batchservice/task/add#taskconstraints)özellik tarafından belirtilen görev için geçen süre aşıldı.

Her durumda aşağıdaki özellikleri hatalar ve hatalar hakkında bilgi için kontrol edilmelidir:
- Görevler [yürütmeBilgi](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutioninformation) özelliği bir hata hakkında bilgi sağlayan birden çok özellik içerir. [sonuç,](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutionresult) görevin herhangi bir nedenle `exitCode` `failureInfo` başarısız olup olmadığını, hata yla birlikte ve başarısızlık hakkında daha fazla bilgi sağlar.
- Görev her zaman başarılı `completed` ya da başarısız olup olmadığını bağımsız [olarak, devlete](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate)taşınır.

Görev hatalarının iş üzerindeki etkisi ve görev bağımlılıkları göz önünde bulundurulmalıdır.  [ExitConditions](https://docs.microsoft.com/rest/api/batchservice/task/add#exitconditions) özelliği, bağımlılıklar ve iş için bir eylemi yapılandırmak için bir görev için belirtilebilir.
- Bağımlılıklar [için, BağımlılıkEylem,](https://docs.microsoft.com/rest/api/batchservice/task/add#dependencyaction) başarısız göreve bağlı görevlerin engellenip engellenmediğini veya çalıştırılıp çalıştırılmadığını denetler.
- İş [için, İş Eylemi,](https://docs.microsoft.com/rest/api/batchservice/task/add#jobaction) başarısız olan görevin işin devre dışı bırakılmasına, sonlandırılmasına veya değişmeden bırakılmasına yol açıp açmayacağını denetler.

### <a name="task-command-line-failures"></a>Görev komut satırı hataları

Görev komut satırı çalıştırıldığında, çıktı `stderr.txt` yazılır `stdout.txt`ve . Buna ek olarak, uygulama uygulamaya özgü günlük dosyalarına yazabilir.

Bir görevin çalıştırıldığı havuz düğümü hala varsa, günlük dosyaları elde edilebilir ve görüntülenebilir. Örneğin, Azure portalı bir görev veya havuz düğümü için günlük dosyalarını listeler ve görüntüleyebilir. Birden çok API, [görevden al](https://docs.microsoft.com/rest/api/batchservice/file/getfromtask)gibi görev dosyalarının listelenmesine ve elde edilmesine de izin verir.

Havuzlar ve havuz düğümleri sık sık geçici olması nedeniyle, düğümleri sürekli eklenen ve silinen, daha sonra günlük dosyaları kalıcı önerilir. [Görev çıktıdosyaları,](https://docs.microsoft.com/azure/batch/batch-task-output-files) günlük dosyalarını Azure Depolama'ya kaydetmenin kullanışlı bir yoludur.

### <a name="output-file-failures"></a>Çıktı dosyası hataları
Her dosya yükleme, Batch işlem düğümüne iki günlük `fileuploadout.txt` dosyası `fileuploaderr.txt`yazar ve . Belirli bir hata hakkında daha fazla bilgi edinmek için bu günlük dosyalarını inceleyebilirsiniz. Dosya yüklemesinin hiç denenmediği durumlarda, örneğin görevin kendisi çalıştırılamadığı için, bu günlük dosyaları bulunmaz.  

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızın kapsamlı hata denetimi uyguladığından denetleyin; sorunları hemen algılamak ve tanılamak çok önemli olabilir.
