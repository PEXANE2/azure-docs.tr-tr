---
title: Azure Batch 'de hata işleme ve algılama
description: Toplu Iş hizmeti iş akışlarında bir geliştirme açısından hata işleme hakkında bilgi edinin.
ms.topic: article
ms.date: 05/15/2020
ms.openlocfilehash: 3bd460598dae08fa18415e1c9865249f3ca4c9c2
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964286"
---
# <a name="error-handling-and-detection-in-azure-batch"></a>Azure Batch 'de hata işleme ve algılama

Her zaman, toplu Iş çözümünüzde hem görev hem de uygulama başarısızlıklarını işlemek için gerekli olduğunu fark edebilirsiniz. Bu makale, hata türleri ve bunların nasıl çözümleneceği hakkında bilgi sağlar.

## <a name="error-codes"></a>Hata kodları

Genel hata türleri şunlardır:

- Toplu Işe erişilemeyen veya toplu Iş yanıtı istemciye zamanında ulaşamadığınız istekler için ağ sorunları.
- İç sunucu hataları (Standart 5xx durum kodu HTTP yanıtı).
- Yeniden deneme-sonrası üstbilgisiyle 429 veya 503 durum kodu HTTP yanıtları gibi daraltma ile ilgili hatalar.
- AlreadyExists ve InvalidOperation gibi 4xx hataları. Bu, kaynağın durum geçişi için doğru durumda olmadığı anlamına gelir.

REST API, Batch hizmeti ve iş görevi/zamanlama için hata kodları da dahil olmak üzere belirli hata kodları hakkında ayrıntılı bilgi için bkz. [Batch durum ve hata kodları](/rest/api/batchservice/batch-status-and-error-codes).

## <a name="application-failures"></a>Uygulama hataları

Yürütme sırasında bir uygulama, sorunları gidermek için kullanabileceğiniz tanılama çıktıları üretebilir. [Dosyalar ve dizinler](files-and-directories.md)bölümünde açıklandığı gibi, Batch hizmeti standart çıkış ve standart hata çıkışını `stdout.txt` `stderr.txt` işlem düğümündeki görev dizininde bulunan dosyalara yazar.

Bu dosyaları indirmek için Azure portalını veya toplu SDK'lardan birini kullanabilirsiniz. Örneğin, Batch .NET kitaplığındaki [ComputeNode.GetNodeFile](/dotnet/api/microsoft.azure.batch.computenode) ve [CloudTask.GetNodeFile](/dotnet/api/microsoft.azure.batch.cloudtask) öğelerini kullanarak bu dosyaları ve diğer dosyaları sorun giderme amacıyla alabilirsiniz.

## <a name="task-errors"></a>Görev hataları

Görev hataları çeşitli kategorilere ayrılır.

### <a name="pre-processing-errors"></a>Ön işleme hataları

Bir görev çalışmadığında ön işleme hatası belirlenir.  

Görevin kaynak dosyaları taşındığında, depolama hesabı artık kullanılabilir olmadığında veya dosyaların düğüme başarıyla kopyalanmasını önleyen bir sorunla karşılaşıldığında, ön işleme hataları oluşabilir.

### <a name="file-upload-errors"></a>Karşıya dosya yükleme hataları

Bir görev için belirtilen dosyalar herhangi bir nedenle karşıya yüklenemeyebilir, görev için bir dosya yükleme hatası ayarlanır.

Azure Depolama erişimi için verilen SAS geçersiz olduğunda veya yazma izni vermediğinde, depolama hesabı kullanılabilir durumda olmadığında veya dosyaların düğümden başarıyla kopyalanmasını önleyen bir sorunla karşılaşıldığında karşıya dosya yükleme hataları oluşabilir.

### <a name="application-errors"></a>Uygulama hataları

Görevin komut satırı tarafından belirtilen işlem de başarısız olabilir. Görev tarafından yürütülen işlem sıfır olmayan çıkış kodu döndürdüğünde işlemin başarısız olduğu kabul edilir (sonraki bölümde yer alan *Görev çıkış kodları* kısmına bakın).

Uygulama hataları için toplu Işi, görevi belirtilen sayıda otomatik olarak yeniden deneyecek şekilde yapılandırabilirsiniz.

### <a name="constraint-errors"></a>Kısıtlama hataları

Bir iş ya da görev için maksimum yürütme süresini belirleyen bir kısıtlama (*maxWallClockTime*) ayarlayabilirsiniz. Bu ilerleme göstermeyen görevlerin sonlandırılması için faydalı olabilir.

Maksimum süre aşıldığında görev *tamamlandı* olarak işaretlenir ancak çıkış kodu `0xC000013A` olarak ayarlanır ve *schedulingError* alanı `{ category:"ServerError", code="TaskEnded"}` olarak işaretlenir.

## <a name="task-exit-codes"></a>Görev çıkış kodları

Daha önce belirtildiği gibi, görev tarafından yürütülen işlem sıfır olmayan bir çıkış kodu döndürürse görev Batch hizmeti tarafından başarısız olarak işaretlenir. Bir görev bir işlemi yürüttüğünde Batch, görevin çıkış kodu özelliğini işlemin dönüş kodu ile doldurur.

Bir görevin çıkış kodunun Batch hizmeti tarafından belirlenmediğine dikkat etmeniz önemlidir. Bir görevin çıkış kodu işlemin kendisi ya da işlemin yürütüldüğü işletim sistemi tarafından belirlenir.

## <a name="task-failures-or-interruptions"></a>Görev arızaları veya kesintiler

Görevler zaman zaman başarısız olabilir veya kesintiye uğrayabilir. Görev uygulamasının kendisi başarısız olabilir, görevin üzerinde çalıştığı düğüm yeniden başlatılabilir veya bir yeniden boyutlandırma işlemi sırasında düğüm havuzdan kaldırılabilir (havuzun ayırmayı kaldırma ilkesi, görevlerin tamamlanmasını beklemeden düğümleri hemen kaldırmak üzere ayarlandıysa). Her durumda, görev başka bir düğümde yürütülmek üzere Batch tarafından otomatik olarak yeniden kuyruğa alınabilir.

Bir görevin yanıt vermemesine veya yürütülmesi çok uzun sürmesine neden olan aralıklı bir sorun da mümkündür. Bir görev için en fazla yürütme aralığını ayarlayabilirsiniz. Maksimum yürütme aralığı aşılırsa Batch hizmeti görev uygulamasını kesintiye uğratır.

## <a name="connect-to-compute-nodes"></a>İşlem düğümlerine bağlanma

Bir işlem düğümünde uzaktan oturum açarak ek hata ayıklama ve sorun giderme işlemlerini gerçekleştirebilirsiniz. Windows düğümleri için bir Uzak Masaüstü Protokolü (RDP) dosyası indirmek ve Linux düğümleri için Güvenli Kabuk (SSH) bağlantı bilgilerini elde etmek üzere Azure portalını kullanabilirsiniz. Bunu, Batch [.net](/dotnet/api/microsoft.azure.batch.computenode) veya [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh)gibi Batch API 'lerini kullanarak da yapabilirsiniz.

> [!IMPORTANT]
> RDP veya SSH aracılığıyla bir düğüme bağlanmak için düğümde bir kullanıcı oluşturmanız gerekir. Bunu yapmak için Azure portalını kullanabilir, Batch REST API’sini kullanarak [bir düğüme kullanıcı hesabı ekleyebilir](/rest/api/batchservice/computenode/adduser), Batch .NET içinde [ComputeNode.CreateComputeNodeUser](/dotnet/api/microsoft.azure.batch.computenode) yöntemini çağırabilir veya Batch Python modülünde [add_user](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh) yöntemini çağırabilirsiniz.

İşlem düğümlerini kısıtlamanız veya bu düğümlere RDP ya da SSH erişimini devre dışı bırakmanız gerekiyorsa, bkz. [Azure Batch havuzunda işlem düğümlerine uzaktan erişimi yapılandırma veya devre dışı bırakma](pool-endpoint-configuration.md).

## <a name="troubleshoot-problem-nodes"></a>Sorun düğümlerinde sorun giderme

Bazı görevlerinizin başarısız olduğu durumlarda, Batch istemci uygulamanız ya da hizmetiniz, hatalı davranan düğümü tanımlamak üzere başarısız görevlerin meta verilerini inceleyebilir. Bir havuzdaki her düğüme benzersiz bir kimlik verilir ve bir görevin çalıştığı düğüm görev meta verilerine eklenir. Bir sorun düğümünü tanımladıktan sonra bununla birkaç eylem gerçekleştirebilirsiniz:

- **Düğümü yeniden başlatma** ([rest](/rest/api/batchservice/computenode/reboot)  |  [.net](/dotnet/api/microsoft.azure.batch.computenode.reboot)))

    Düğümü yeniden başlatmak bazen takılan veya çöken işlemler gibi görünmeyen sorunları temizleyebilir. Havuzunuz bir başlangıç görevi kullanıyorsa ya da işiniz iş hazırlama görevi kullanıyorsa, düğüm yeniden başlatıldığında yürütülür.
- **Düğümün görüntüsünü yeniden oluşturma** ([REST](/rest/api/batchservice/computenode/reimage) | [.NET](/dotnet/api/microsoft.azure.batch.computenode.reimage))

    Bu işlem, işletim sistemini düğüme yeniden yükler. Düğümün yeniden başlatılmasıyla düğümün görüntüsünün yeniden oluşturulmasının ardından başlangıç görevleri ve iş hazırlama görevleri yeniden çalıştırılır.
- **Düğümü havuzdan kaldırma** ([REST](/rest/api/batchservice/pool/removenodes) | [.NET](/dotnet/api/microsoft.azure.batch.pooloperations))

    Bazen düğümün havuzdan tamamen kaldırılması gereklidir.
- **Düğümde görev zamanlamasını devre dışı bırakma** ([REST](/rest/api/batchservice/computenode/disablescheduling) | [.NET](/dotnet/api/microsoft.azure.batch.computenode.disablescheduling))

    Bu, başka görev atanmayacak şekilde düğümü çevrimdışı durumuna alır, ancak düğümün çalışır durumda ve havuzda kalmasına izin verir. Bu, başarısız olan görevin verilerini kaybetmeden ve düğüm, başka görev hatalarına neden olmadan hatalara ilişkin ayrıntılı araştırma gerçekleştirmenizi sağlar. Örneğin düğümde görev zamanlamayı devre dışı bırakabilir, sonra düğümün olay günlüklerini incelemek üzere uzaktan oturum açabilir ya da başka sorun giderme işlemleri uygulayabilirsiniz. Araştırmanızı bitirdikten sonra görev zamanlamayı ([rest](/rest/api/batchservice/computenode/enablescheduling)  |  [.net](/dotnet/api/microsoft.azure.batch.computenode.enablescheduling)veya daha önce bahsedilen diğer eylemlerden birini gerçekleştirerek, daha sonra düğümü yeniden çevrimiçi duruma getirebilirsiniz.

> [!IMPORTANT]
> Yukarıda açıklanan eylemlerle, youc, eylemi gerçekleştirdiğinizde düğümde çalışmakta olan görevlerin nasıl işleneceğini belirtebilir. Örneğin, Batch .NET istemci kitaplığını kullanarak görev zamanlamayı devre dışı bıraktığınızda, çalışan görevleri **Sonlandırma**, diğer düğümlerde zamanlama için **Yeniden kuyruğa alma** ya da eylemi gerçekleştirmeden önce çalışan görevlerin tamamlanmasına izin vermeyi (**TaskCompletion**) belirtmek üzere [DisableComputeNodeSchedulingOption](/dotnet/api/microsoft.azure.batch.common.disablecomputenodeschedulingoption) listeleme değeri belirleyebilirsiniz.

## <a name="retry-after-errors"></a>Hatalardan sonra yeniden dene

Bir hata oluşursa Batch API 'Leri size bildirir. Hepsi yeniden denenebilirler ve hepsi bu amaçla küresel bir yeniden deneme işleyicisi içerir. Bu yerleşik mekanizmayı kullanmak en iyisidir.

Bir hatadan sonra yeniden denemeden önce bir bit (denemeler arasında birkaç saniye) beklemeniz gerekir. Çok sık veya çok hızlı yeniden deneme yaparsanız, yeniden deneme işleyicisi azalacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Havuz ve düğüm hatalarını denetleme](batch-pool-node-error-checking.md)hakkında bilgi edinin.
- [İş ve görev hatalarını denetleme](batch-job-task-error-checking.md)hakkında bilgi edinin.
- [Toplu Iş durumu ve hata kodları](/rest/api/batchservice/batch-status-and-error-codes)listesini gözden geçirin.
