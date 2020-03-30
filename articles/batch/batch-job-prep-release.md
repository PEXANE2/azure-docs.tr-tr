---
title: İşlem düğümlerinde işleri tamamlamak & hazırlamak için görevler oluşturun - Azure Toplu İş
description: Azure Toplu İşlem düğümlerine veri aktarımını en aza indirmek için iş düzeyinde hazırlama görevlerini kullanın ve iş tamamlandığında düğüm temizleme için görevler bırakın.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/17/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: d9f6f015c210592d5d8053b1b34d5357bb357629
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586793"
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Toplu işlem düğümlerinde iş hazırlama ve iş bırakma görevlerini çalıştırma

 Azure Toplu Iş işi, görevleri yürütülmeden önce genellikle bir tür kurulum ve görevleri tamamlandığında iş sonrası bakım gerektirir. İşlem düğümlerinize yaygın görev girişi verilerini indirmeniz veya iş tamamlandıktan sonra görev çıktı verilerini Azure Depolama'ya yüklemeniz gerekebilir. Bu işlemleri gerçekleştirmek için **iş hazırlama** ve **iş bırakma** görevlerini kullanabilirsiniz.

## <a name="what-are-job-preparation-and-release-tasks"></a>İş hazırlama ve bırakma görevleri nelerdir?
Bir işin görevleri çalışmadan önce, iş hazırlama görevi en az bir görev çalıştırmak için zamanlanan tüm işlem düğümleri üzerinde çalışır. İş tamamlandıktan sonra, iş bırakma görevi en az bir görev yürüten havuzdaki her düğümde çalışır. Normal Toplu İşlem görevklerinde olduğu gibi, bir iş hazırlama veya bırakma görevi çalıştırıldığında çağrılacak bir komut satırı belirtebilirsiniz.

İş hazırlama ve yayımlama görevleri, dosya indirme[(kaynak dosyaları),][net_job_prep_resourcefiles]yüksek yürütme, özel ortam değişkenleri, maksimum yürütme süresi, yeniden deneme sayısı ve dosya saklama süresi gibi tanıdık Toplu iş görevleri özellikleri sunar.

Aşağıdaki [bölümlerde, Toplu İşlem .NET][api_net] kitaplığında bulunan [JobPreparationTask][net_job_prep] ve [JobReleaseTask][net_job_release] sınıflarını nasıl kullanacağınızı öğreneceksiniz.

> [!TIP]
> İş hazırlama ve bırakma görevleri, özellikle bir işlem düğümü havuzunun iş çalıştırmaları arasında devam ettiği ve birçok iş tarafından kullanıldığı "paylaşılan havuz" ortamlarında yararlıdır.
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a>İş hazırlama ve bırakma görevlerinin ne zaman kullanılacağı
İş hazırlama ve iş bırakma görevleri aşağıdaki durumlar için uygundur:

**Ortak görev verilerini karşıdan yükleme**

Toplu iş işleri genellikle işin görevleri için giriş olarak ortak bir veri kümesi gerektirir. Örneğin, günlük risk analizi hesaplamalarında, piyasa verileri işe özgüdür, ancak işteki tüm görevlerde yaygındır. Genellikle birkaç gigabayt boyutunda olan bu pazar verileri, düğümüzerinde çalışan herhangi bir görevin kullanabilmesi için her işlem düğümüne yalnızca bir kez indirilmelidir. Bu verileri, işin diğer görevlerinin yürütülmesinden önce her düğüme indirmek için bir **iş hazırlama görevi** kullanın.

**İş ve görev çıktısını silme**

Bir havuzun işlem düğümlerinin işler arasında devre dışı bırakılmadığını belirten "paylaşılan havuz" ortamında, çalıştırmalar arasındaki iş verilerini silmeniz gerekebilir. Düğümlerde disk alanını korumanız veya kuruluşunuzun güvenlik ilkelerini karşılamanız gerekebilir. İş hazırlama görevi tarafından indirilen veya görev yürütme sırasında oluşturulan verileri silmek için bir **iş yayımlama görevi** kullanın.

**Günlük tutma**

Görevlerinizin oluşturduğu günlük dosyalarının bir kopyasını tutmak veya başarısız uygulamalar tarafından oluşturulabilecek döküm dosyaları nın bir kopyasını tutmak isteyebilirsiniz. Bu gibi durumlarda bu verileri sıkıştırmak ve bir [Azure Depolama][azure_storage] hesabına yüklemek için bir iş sürümü **görevi** kullanın.

> [!TIP]
> Günlükleri ve diğer iş ve görev çıktısı verilerini kalıcı yormamanın başka bir yolu da [Azure Toplu Dosya Kuralları](batch-task-output.md) kitaplığını kullanmaktır.
>
>

## <a name="job-preparation-task"></a>İş hazırlama görevi


Bir işin görevlerini yürütmeden önce, Toplu Iş, görevi çalıştırmak için zamanlanan her işlem düğümünde iş hazırlama görevini yürütür. Varsayılan olarak, Toplu iş hazırlama görevinin düğümde yürütülmesi için zamanlanan görevleri çalıştırmadan önce tamamlanmasını bekler. Ancak, hizmeti beklememek için yapılandırabilirsiniz. Düğüm yeniden başlatılırsa, iş hazırlama görevi yeniden çalışır. Bu davranışı da devre dışı bekleyebilirsiniz. İş hazırlama göreviniz ve iş yöneticisi görevin yapılandırılan bir işiniz varsa, iş hazırlama görevi, diğer tüm görevlerde olduğu gibi iş yöneticisi görevinden önce çalışır. İş hazırlama görevi her zaman önce çalışır.

İş hazırlama görevi yalnızca bir görevi çalıştırmak için zamanlanan düğümlerde yürütülür. Bu, bir düğüme görev atanmaması durumunda hazırlık görevinin gereksiz yürütülmesini önler. Bu, bir iş için görev sayısı havuzdaki düğüm sayısından daha az olduğunda oluşabilir. Ayrıca, eşzamanlı [görev yürütme](batch-parallel-node-tasks.md) etkin olduğunda da geçerlidir, bu da görev sayısı toplam eşzamanlı görevlerin daha düşükse bazı düğümleri boşta bırakır. Boşta düğümlerde iş hazırlama görevini çalıştırmadan, veri aktarım ücretlerine daha az para harcayabilirsiniz.

> [!NOTE]
> [JobPreparationTask,][net_job_prep_cloudjob] JobPreparationTask'ın her işin başında yürüttüğü durumlarda [CloudPool.StartTask'tan][pool_starttask] farklıdır, oysa StartTask yalnızca bir işlem düğümü bir havuza ilk katıldığında veya yeniden başlatıldığında yürütür.
>


>## <a name="job-release-task"></a>İş bırakma görevi

Bir iş tamamlandıktan sonra, iş serbest bırakma görevi en az bir görev yürütülen havuzdaki her düğümüzerinde yürütülür. Sonlandırma isteği vererek tamamlanan bir işi işaretlersiniz. Toplu İşlem hizmeti daha sonra iş durumunu *sonlandırmaya*ayarlar, işle ilişkili tüm etkin veya çalıştırAn görevleri sonlandırır ve iş bırakma görevini çalıştırır. İş daha sonra *tamamlanan* duruma taşınır.

> [!NOTE]
> İş silme işlemi de iş bırakma görevini yürütür. Ancak, bir iş zaten sonlandırılmışsa, iş daha sonra silinirse, sürüm görevi ikinci kez çalıştırılmez.

İşler yayımı görevleri, Toplu İşlem hizmeti tarafından sonlandırılmadan önce en fazla 15 dakika çalıştırılabilir. Daha fazla bilgi için [REST API başvuru belgelerine](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask)bakın.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Batch .NET ile iş hazırlama ve bırakma görevleri
İş hazırlama görevini kullanmak için, işinizin [CloudJob.JobPreparationTask][net_job_prep_cloudjob] özelliğine bir [JobPreparationTask][net_job_prep] nesnesi atayın. Benzer şekilde, bir [JobReleaseTask'ı][net_job_release] başlatma ve işin serbest bırakma görevini ayarlamak için işinizin [CloudJob.JobReleaseTask][net_job_prep_cloudjob] özelliğine atayın.

Bu kod snippet, `myBatchClient` [BatchClient][net_batch_client]bir örneğidir ve `myPool` Toplu Iş hesabı içinde varolan bir havuzdur.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobReleaseTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Daha önce de belirtildiği gibi, bir iş sonlandırıldığında veya silindiğinde sürüm görevi yürütülür. [İş İşlemleri.TerminateJobAsync][net_job_terminate]ile bir işi sonlandırma. [JobOperations.DeleteJobAsync][net_job_delete]ile bir işi silme. Genellikle bir işi, görevleri tamamlandığında veya tanımladığınız bir zaman anına ulaşıldığında sonlandırır veya silersiniz.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>GitHub'da kod örneği
İş hazırlama ve serbest bırakma görevlerini iş başında görmek için GitHub'daki [JobPrepRelease][job_prep_release_sample] örnek projesine göz atın. Bu konsol uygulaması aşağıdakileri yapar:

1. İki düğümlü bir havuz oluşturur.
2. İş hazırlama, serbest bırakma ve standart görevler içeren bir iş oluşturur.
3. Düğüm kimliğini önce düğümün "paylaşılan" dizinindeki metin dosyasına yazan iş hazırlama görevini çalıştırAr.
4. Görev kimliğini aynı metin dosyasına yazan her düğümde bir görev çalıştırın.
5. Tüm görevler tamamlandıktan (veya zaman adedine ulaşıldıktan sonra), her düğümün metin dosyasının içeriğini konsola yazdırır.
6. İş tamamlandığında, dosyayı düğümden silmek için iş yayımı görevini çalıştırın.
7. İş hazırlığı ve çalıştırdıkları her düğüm için serbest bırakma görevlerinin çıkış kodlarını yazdırır.
8. İş ve/veya havuz silme onayını sağlamak için yürütmeyi duraklatır.

Örnek uygulamadan elde edilen çıktı aşağıdakilere benzer:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

> [!NOTE]
> Değişken oluşturma ve yeni bir havuzda düğümlerin başlangıç zamanı nedeniyle (bazı düğümler diğerlerinden önce görevler için hazır), farklı çıktı görebilirsiniz. Özellikle, görevler hızlı bir şekilde tamamlandığından, havuzun düğümlerinden biri işin tüm görevlerini yürütebilir. Bu durumda, hiçbir görev çalıştıran düğüm için iş hazırlık ve sürüm görevlerinin bulunmadığını fark edeceksiniz.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Azure portalında iş hazırlama ve sürüm görevlerini inceleyin
Örnek uygulamayı çalıştırdığınızda, işin özelliklerini ve görevlerini görüntülemek için [Azure portalını][portal] kullanabilir, hatta işin görevleri tarafından değiştirilen paylaşılan metin dosyasını indirebilirsiniz.

Aşağıdaki ekran görüntüsü, örnek uygulamanın bir çalışmasından sonra Azure portalındaki **Hazırlık görevleri bıçaklarını** gösterir. Görevleriniz tamamlandıktan sonra (ancak işinizi ve havuzunuzu silmeden önce) *JobPrepReleaseSampleJob* özelliklerine gidin ve özelliklerini görüntülemek için Hazırlık görevlerini veya **Serbest Bırakma** **görevlerini** tıklatın.

![Azure portalında iş hazırlama özellikleri][1]

## <a name="next-steps"></a>Sonraki adımlar
### <a name="application-packages"></a>Uygulama paketleri
İş hazırlama görevine ek olarak, görev yürütme için bilgi işlem düğümleri hazırlamak için Toplu İşlem'in [uygulama paketleri](batch-application-packages.md) özelliğini de kullanabilirsiniz. Bu özellik, özellikle yükleyici çalıştırmayı gerektirmeyen uygulamaları, çok sayıda (100+) dosya içeren uygulamaları veya sıkı sürüm denetimi gerektiren uygulamaları dağıtmak için kullanışlıdır.

### <a name="installing-applications-and-staging-data"></a>Uygulamaların yüklenmesi ve verilerin evreleme
Bu MSDN forum gönderisi, düğümlerinizi görevleri çalıştırmak için hazırlamanın çeşitli yöntemlerine genel bir bakış sağlar:

[Toplu işlem düğümlerine uygulama yükleme ve veri hazırlama][forum_post]

Azure Toplu Iş ekibi üyelerinden biri tarafından yazılan bu yazıda, düğümleri hesaplamak için uygulamaları ve verileri dağıtmak için kullanabileceğiniz çeşitli teknikler anlatılır.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
