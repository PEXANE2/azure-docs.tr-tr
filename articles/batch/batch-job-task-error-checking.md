---
title: İş ve görev hatalarını denetleme
description: İşleri ve görevleri denetleme ve sorunlarını giderme hataları
author: mscurrell
ms.topic: how-to
ms.date: 03/10/2019
ms.author: markscu
ms.openlocfilehash: 368b803ae3e62c0e27da9e52d9df9842037757ff
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726630"
---
# <a name="job-and-task-error-checking"></a>İş ve görev hatası denetimi

İşler ve görevler eklenirken oluşabilecek çeşitli hatalar vardır. API, CLı veya Kullanıcı arabirimi tarafından hemen herhangi bir başarısızlık döndürüldüğünden, bu işlemler için hataların algılanması basittir.  Ancak, daha sonra işler ve görevler zamanlandığında ve çalıştırıldığında oluşabilecek sorunlar vardır.

Bu makalede, işler ve görevler gönderildikten sonra oluşabilecek hatalar ele alınmaktadır. Denetlenmesi ve işlenmesi gereken hataları listeler ve açıklar.

## <a name="jobs"></a>İşler

Bir iş, bir veya daha fazla görevin gruplandırılması, görevleri gerçekten çalıştırılacak komut satırlarını belirtmektir.

Bir iş eklenirken, işin başarısız olmasına neden olabilecek aşağıdaki parametreler belirtilebilir:

- [İş kısıtlamaları](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)
  - `maxWallClockTime`Özellik isteğe bağlı olarak, bir işin etkin veya çalışıyor olduğu maksimum süreyi ayarlamak için belirtilebilir. Bu süre aşılırsa, iş `terminateReason` Için [ExecutionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#cloudjob) 'da ayarlanan özellik ile iş sonlandırılır.
- [İş hazırlama görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask)
  - Belirtilmişse, bir iş için bir iş için görev ilk kez çalıştırıldığında bir iş hazırlama görevi çalıştırılır. İş hazırlama görevi başarısız olabilir, bu, görevin çalıştırılmadığından ve işin tamamlanmemesine neden olacak.
- [İş serbest bırakma görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask)
  - İş serbest bırakma görevi, yalnızca bir iş hazırlama görevi yapılandırılmışsa belirtilebilir. Bir iş sonlandırıldığı zaman iş bırakma görevi, bir iş hazırlama görevinin çalıştırıldığı havuz düğümlerinin her birinde çalıştırılır. İş serbest bırakma görevi başarısız olabilir, ancak iş yine de bir duruma geçmeyecektir `completed` .

### <a name="job-properties"></a>İş özellikleri

Aşağıdaki iş özellikleri hatalar için denetlenmelidir:

- '[ExecutionInfo](https://docs.microsoft.com/rest/api/batchservice/job/get#jobexecutioninformation)':
  - `terminateReason`Özelliği, `maxWallClockTime` iş kısıtlamalarında belirtilen değerinin aşıldığını ve bu nedenle işin sonlandırıldığı belirten değerlere sahip olabilir. İş özelliği uygun şekilde ayarlandıysa bir görevin başarısız olduğunu belirtmek için de ayarlanabilir `onTaskFailure` .
  - [SchedulingError](https://docs.microsoft.com/rest/api/batchservice/job/get#jobschedulingerror) özelliği, bir zamanlama hatası varsa ayarlanır.
 
### <a name="job-preparation-tasks"></a>İş hazırlama görevleri

İş için bir iş hazırlama görevi belirtilmişse bu görevin bir örneği, bir düğümde iş için bir görev çalıştırıldığında ilk kez çalıştırılır. İş üzerinde yapılandırılan iş hazırlama görevi, bir havuzdaki düğüm sayısına kadar, birden çok iş hazırlama görevi örneği çalıştırılan bir görev şablonu olarak düşünülebilir.

Hata olup olmadığını anlamak için iş hazırlama görev örnekleri denetlenmelidir:
- Bir iş hazırlama görevi çalıştırıldığında, iş hazırlama görevini tetikleyen görev [durumuna](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate) geçer `preparing` ; iş hazırlama görevi başarısız olursa, tetikleme görevi `active` duruma döndürülür ve çalıştırılmaz.  
- İş hazırlama görevinin çalıştırıldığı tüm örnekler, iş için [liste hazırlama ve bırakma görev durumu](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) API 'si kullanılarak alınabilir. Herhangi bir görevde olduğu gibi,, ve gibi özelliklerle birlikte bulunan [yürütme bilgileri](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) de vardır `failureInfo` `exitCode` `result` .
- İş hazırlama görevleri başarısız olursa, tetikleme işi görevleri çalıştırılmayacak, iş tamamlanmaz ve takılacaktır. Zamanlanabilecek görevlerle başka iş yoksa, havuz unutilized go görünebilir.

### <a name="job-release-tasks"></a>İş bırakma görevleri

İş için bir iş serbest bırakma görevi belirtilmişse, bir işin sonlandırıldığı zaman, iş hazırlama görevinin çalıştırıldığı havuz düğümlerinin her birinde iş bırakma görevinin bir örneği çalıştırılır.  İş serbest bırakma görevi örnekleri, hata olup olmadığını anlamak için denetlenmelidir:
- İş yayınlama görevinin çalıştırılmakta olan tüm örnekleri, API [listesi hazırlama ve sürüm görevinin durumu](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus)kullanılarak iş 'ten elde edilebilir. Herhangi bir görevde olduğu gibi,, ve gibi özelliklerle birlikte bulunan [yürütme bilgileri](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) de vardır `failureInfo` `exitCode` `result` .
- Bir veya daha fazla iş bırakma görevi başarısız olursa, iş hala sonlandırılır ve bir `completed` duruma geçer.

## <a name="tasks"></a>Görevler

İş görevleri birden çok nedenden dolayı başarısız olabilir:

- Görev komut satırı, sıfır olmayan bir çıkış koduyla dönerek başarısız olur.
- `resourceFiles`Bir görev için belirtilmiş, ancak bir veya daha fazla dosyanın indirmediği bir hata oluştu.
- `outputFiles`Bir görev için belirtilmiş, ancak bir veya daha fazla dosyanın karşıya yüklememediği bir hata oluştu.
- Görev kısıtlamalarında özelliği tarafından belirtilen görev için geçen süre `maxWallClockTime` aşıldı. [constraints](https://docs.microsoft.com/rest/api/batchservice/task/add#taskconstraints)

Her durumda, hatalar ve hatalar hakkında bilgi için aşağıdaki özellikler denetlenmelidir:
- Tasks [ExecutionInfo](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutioninformation) özelliği bir hata hakkında bilgi sağlayan birden çok özellik içerir. [sonuç](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutionresult) , ile ilgili herhangi bir nedenle görevin başarısız olup olmadığını gösterir `exitCode` ve `failureInfo` hata hakkında daha fazla bilgi sağlar.
- Görev, `completed` başarılı veya başarısız olsun etmeksizin her zaman [duruma](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate)taşınır.

Görev hatalarının iş ve görev bağımlılıklarındaki etkileri göz önünde bulundurulmalıdır.  Bir görevin bağımlılıklar ve iş için bir eylem yapılandırması için [Exitconditions](https://docs.microsoft.com/rest/api/batchservice/task/add#exitconditions) özelliği belirtilebilir.
- Bağımlılıklar için [DependencyAction](https://docs.microsoft.com/rest/api/batchservice/task/add#dependencyaction) , başarısız göreve bağımlı görevlerin engellenip engellenmeyeceğini veya çalıştırılmadığını denetler.
- Bu iş için, [Jobaction](https://docs.microsoft.com/rest/api/batchservice/task/add#jobaction) başarısız görevin devre dışı, sonlandırılmış veya sola değiştirilmemiş iş olup olmadığını denetler.

### <a name="task-command-line-failures"></a>Görev komut satırı sorunları

Görev komut satırı çalıştırıldığında, çıkış ve ' a yazılır `stderr.txt` `stdout.txt` . Ayrıca, uygulama uygulamaya özgü günlük dosyalarına yazabilir.

Bir görevin çalıştırıldığı havuz düğümü hala mevcutsa, günlük dosyaları elde edilebilir ve görüntülenebilir. Örneğin Azure portal, bir görev veya havuz düğümü için günlük dosyalarını listeler ve görüntüleyebilir. Aynı zamanda birden çok API, görev dosyalarının da listelenmesine ve almaya izin verir (örneğin, [görevden al](https://docs.microsoft.com/rest/api/batchservice/file/getfromtask)).

Havuzlar ve havuz düğümleri genellikle kısa ömürlü olduğundan, düğümler sürekli olarak eklenerek ve silinirken, günlük dosyalarının kalıcı olması önerilir. [Görev çıktı dosyaları](https://docs.microsoft.com/azure/batch/batch-task-output-files) , günlük dosyalarını Azure depolama 'ya kaydetmek için kullanışlı bir yoldur.

### <a name="output-file-failures"></a>Çıkış dosyası sorunları
Her dosya yükleme işleminde toplu Işlem, işlem düğümüne iki günlük dosyası yazar `fileuploadout.txt` ve `fileuploaderr.txt` . Belirli bir hata hakkında daha fazla bilgi edinmek için bu günlük dosyalarını inceleyebilirsiniz. Dosya yüklemesinin hiçbir zaman denenmediği durumlarda (örneğin, görevin kendisi çalıştırılmadığından), bu günlük dosyaları mevcut olmayacaktır.  

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızın kapsamlı hata denetimi uyguladığından emin olun; sorunları saptamak ve tanılamak için kritik öneme sahip olabilir.
