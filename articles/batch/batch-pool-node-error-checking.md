---
title: Havuz ve düğüm hatalarını denetleyin - Azure Toplu İşlem
description: Bu makalede, denetlemek için hatalar ve havuzları ve düğümleri oluştururken bunları önlemek için nasıl birlikte oluşabilir arka plan işlemleri kapsar.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/23/2019
ms.topic: conceptual
ms.openlocfilehash: a68d812a044c776819d169d5bf179f011d06390f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472954"
---
# <a name="check-for-pool-and-node-errors"></a>Havuz ve düğüm hatalarını denetleme

Azure Toplu İş havuzları oluştururken ve yönetirken, bazı işlemler hemen gerçekleşir. Ancak, bazı işlemler asynchronous ve tamamlamak için birkaç dakika sürer, arka planda çalıştırın.

Herhangi bir hata API, CLI veya UI tarafından hemen döndürülür, çünkü hemen gerçekleşen işlemler için hataları algılamak basittir.

Bu makalede, havuzlar ve havuz düğümleri için oluşabilecek arka plan işlemleri kapsar. Hataları nasıl algılayıp önleyebileceğinizi belirtir.

## <a name="pool-errors"></a>Havuz hataları

### <a name="resize-timeout-or-failure"></a>Zaman anına veya arızaya yeniden boyutlandırma

Yeni bir havuz oluştururken veya varolan bir havuzu yeniden boyutlandırmaya çalışırken, düğümlerin hedef sayısını belirtirsiniz.  Oluşturma veya yeniden boyutlandırma işlemi hemen tamamlar, ancak yeni düğümlerin gerçek tahsisi veya varolan düğümlerin kaldırılması birkaç dakika sürebilir.  [Oluşturma](https://docs.microsoft.com/rest/api/batchservice/pool/add) veya yeniden boyutlandırma API'deki yeniden boyutlandırma zaman öncesini [belirtirsiniz.](https://docs.microsoft.com/rest/api/batchservice/pool/resize) Toplu İşlem, yeniden boyutlandırma zaman döneminde hedef düğüm sayısını elde edemezse, havuz sabit bir duruma geçer ve hataları yeniden boyutlandırır.

En son değerlendirme için [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) özelliği, oluşan hataları listeler.

Yeniden boyutlandırma hatalarının yaygın nedenleri şunlardır:

- Yeniden boyutlandırma zaman dilimi çok kısa
  - Çoğu durumda, 15 dakikalık varsayılan zaman dilimi havuz düğümlerinin ayrılması veya kaldırılması için yeterince uzundur.
  - Çok sayıda düğüm ayırıyorsanız, yeniden boyutlandırma zaman dilimini 30 dakikaya ayarlamanızı öneririz. Örneğin, bir Azure Marketi görüntüsünden 1.000'den fazla düğüme veya özel bir VM görüntüden 300'den fazla düğüme yeniden boyutlandırma yaparken.
- Yetersiz çekirdek kota
  - Toplu işlem, tüm havuzlara ayırabileceği çekirdek sayısıyla sınırlıdır. Toplu işlem, kotaya ulaşıldıktan sonra düğümayırmayı durdurur. Toplu İşlem'in daha fazla düğüm ayırabilmesi için çekirdek kotayı [artırabilirsiniz.](https://docs.microsoft.com/azure/batch/batch-quota-limit)
- [Bir havuz sanal ağda](https://docs.microsoft.com/azure/batch/batch-virtual-network) olduğunda yetersiz alt ağ IP'leri
  - Sanal ağ alt ağının, istenen her havuz düğümüne tahsis edecek kadar atanmamış IP adresi olmalıdır. Aksi takdirde düğümler oluşturulamaz.
- Bir havuz [sanal ağda](https://docs.microsoft.com/azure/batch/batch-virtual-network) olduğunda yetersiz kaynaklar
  - Toplu Iş hesabıyla aynı abonelikte yük bakiyesi verenler, genel IP'ler ve ağ güvenlik grupları gibi kaynaklar oluşturabilirsiniz. Abonelik kotalarının bu kaynaklar için yeterli olup olmadığını denetleyin.
- Özel VM görüntüleri ne kadar büyük havuzlar
  - Özel VM görüntüleri kullanan büyük havuzların zaman ayırması ve yeniden boyutlandırması daha uzun sürebilir.  Bkz. Sınırlar ve yapılandırma yla ilgili öneriler için [Paylaşılan Resim Galerisi'nde bir havuz oluşturun.](batch-sig-images.md)

### <a name="automatic-scaling-failures"></a>Otomatik ölçekleme hataları

Azure Toplu İş'i havuzdaki düğüm sayısını otomatik olarak ölçeklendirecek şekilde de ayarlayabilirsiniz. Bir havuz için [otomatik ölçekleme formülü için](https://docs.microsoft.com/azure/batch/batch-automatic-scaling)parametreleri tanımlarsınız. Toplu İşlem hizmeti, havuzdaki düğüm sayısını düzenli olarak değerlendirmek ve yeni bir hedef numarası ayarlamak için formülü kullanır. Aşağıdaki türsorunlar oluşabilir:

- Otomatik ölçeklendirme değerlendirmesi başarısız olur.
- Ortaya çıkan yeniden boyutlandırma işlemi başarısız olur ve zaman ları uzar.
- Otomatik ölçekleme formülü ile ilgili bir sorun yanlış düğüm hedef değerlerine yol açar. Yeniden boyutlandırma ya çalışır ya da zaman dışarı.

[AutoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) özelliğini kullanarak son otomatik ölçeklendirme değerlendirmesi hakkında bilgi alabilirsiniz. Bu özellik, değerlendirme süresini, değerleri ve sonucu ve performans hatalarını bildirir.

[Havuz yeniden boyutlandırma olayı tüm](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event) değerlendirmeler hakkında bilgi yakalar.

### <a name="delete"></a>Sil

Düğümiçeren bir havuzu sildiğinizde, ilk Toplu İşlem düğümleri siler. Sonra havuz nesnesinin kendisini siler. Havuz düğümlerinin silinmesi birkaç dakika sürebilir.

Toplu işlem, silme işlemi sırasında [havuz durumunu](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) **silmeye** ayarlar. Arama **uygulaması, durum** ve **stateTransitionTime** özelliklerini kullanarak havuz silme işleminin çok uzun sürdüğünü algılayabilir.

## <a name="pool-compute-node-errors"></a>Havuz işlem düğümü hataları

Toplu İşlem bir havuzda düğümleri başarıyla ayırsa bile, çeşitli sorunlar bazı düğümlerin sağlıksız olmasına ve görevleri çalıştıramamasına neden olabilir. Bu düğümler hala ücrete tabi olduğundan, kullanılanınamayan düğümler için ödeme yapmaktan kaçınmak için sorunları algılamak önemlidir. Ortak düğüm hatalarına ek olarak, geçerli [iş durumunu](/rest/api/batchservice/job/get#jobstate) bilmek sorun giderme için yararlıdır.

### <a name="start-task-failures"></a>Görev hatalarını başlatma

Havuz için isteğe bağlı [bir başlangıç görevi](/rest/api/batchservice/pool/add#starttask) belirtmek isteyebilirsiniz. Herhangi bir görevde olduğu gibi, depolamadan indirmek için bir komut satırı ve kaynak dosyaları kullanabilirsiniz. Başlangıç görevi, başlatıldıktan sonra her düğüm için çalıştırılır. **WaitForSuccess** özelliği, Toplu İşlemin herhangi bir görevi düğüme planlamadan önce başlangıç görevinin başarıyla tamamlanmasını bekleyip beklemediğini belirtir.

Düğümü başarılı başlangıç görevi nin tamamlanmasını bekleyecek şekilde yapılandırırsanız, ancak başlangıç görevi başarısız olduysa ne olur? Bu durumda, düğüm kullanılabilir olmayacaktır, ancak yine de ücrete tabi olacaktır.

Üst düzey [startTaskInfo](/rest/api/batchservice/computenode/get#starttaskinformation) düğüm özelliğinin [sonuç](/rest/api/batchservice/computenode/get#taskexecutionresult) ve [failureInfo](/rest/api/batchservice/computenode/get#taskfailureinformation) özelliklerini kullanarak başlangıç görev hatalarını algılayabilirsiniz.

Başarısız bir başlangıç görevi, **WaitForSuccess** **doğru**ayarlanmışsa, Toplu İşlem'in düğüm [durumunu](/rest/api/batchservice/computenode/get#computenodestate) **başlangıç taskfailed** olarak ayarlamasına da neden olur.

Her görevde olduğu gibi, başlangıç görevinin başarısız olmasının birçok nedeni olabilir.  Sorun gidermek için, stdout, stderr ve diğer görev özel günlük dosyaları kontrol edin.

Başlangıç görevleri yeniden başlatılmalıdır, mümkün olduğu gibi başlangıç görevi aynı düğüm üzerinde birden çok kez çalıştırılır; bir düğüm yeniden görüntülendiğinde veya yeniden başlatıldığında başlangıç görevi çalıştırılır. Nadir durumlarda, bir başlangıç görevi, bir düğüm yeniden başlatmaya neden olduktan sonra çalıştırılır ve işletim sistemi veya kısa ömürlü disklerden biri diğeri yeniden görüntülenmezken yeniden görüntülenebilir. Toplu başlangıç görevleri (tüm Toplu İşlem görevleri gibi) kısa ömürlü diskten çalıştırıldığı için, bu normalde bir sorun değildir, ancak başlangıç görevinin işletim sistemi diskine bir uygulama yüklendiği ve geçici diskte diğer verileri tuttuğu bazı durumlarda, bu durum sorunlar, çünkü şeyler senkronize değildir. Her iki diskkullanıyorsanız uygulamanızı buna göre koruyun.

### <a name="application-package-download-failure"></a>Uygulama paketi indirme hatası

Havuz için bir veya daha fazla uygulama paketi belirtebilirsiniz. Toplu işlem, belirtilen paket dosyalarını her düğüme karşı karşı karşı yüklenir ve düğüm başladıktan sonra ancak görevler zamanlanmadan önce dosyaları geri alır. Uygulama paketleriyle birlikte bir başlangıç görev komut satırı kullanmak yaygındır. Örneğin, dosyaları farklı bir konuma kopyalamak veya kurulumu çalıştırmak için.

Düğüm [hataları](/rest/api/batchservice/computenode/get#computenodeerror) özelliği, bir uygulama paketinin karşıdan yüklememesi ve sıkıştırılamaması nı bildirir; düğüm durumu **kullanılamaz**olarak ayarlanır.

### <a name="container-download-failure"></a>Konteyner indirme hatası

Havuzda bir veya daha fazla kapsayıcı referansı belirtebilirsiniz. Toplu işlem, belirtilen kapsayıcıları her düğüme indirir. Düğüm [hataları](/rest/api/batchservice/computenode/get#computenodeerror) özelliği, bir kapsayıcının karşıdan yüklenememesini bildirir ve düğüm durumunu **kullanılamaz**olarak ayarlar.

### <a name="node-in-unusable-state"></a>Kullanılamaz durumda düğüm

Azure Toplu [İş, düğüm durumunu](/rest/api/batchservice/computenode/get#computenodestate) birçok nedenden dolayı **kullanılamaz** olarak ayarlayabilir. Düğüm durumu **kullanılamaz**olarak ayarlanmışsa, görevler düğüme zamanlanamaz, ancak yine de ücrete tabi dir.

Kullanılamaz **durumda** düğümler, ancak [hatasız](/rest/api/batchservice/computenode/get#computenodeerror) Toplu VM ile iletişim kuramadığı anlamına gelir. Bu durumda, Toplu her zaman VM'yi kurtarmaya çalışır. Toplu iş, durumları **kullanılamaz**olmasına rağmen uygulama paketlerini veya kapsayıcılarını yüklemeyi başaramayan VM'leri otomatik olarak kurtarmaya çalışmaz.

Toplu İşlem nedeni belirleyebiliyorsa, düğüm [hataları](/rest/api/batchservice/computenode/get#computenodeerror) özelliği bunu bildirir.

**Kullanılamaz** düğümler için ek örnekler şunlardır:

- Özel bir VM görüntüsü geçersizdir. Örneğin, düzgün hazırlanmayan bir görüntü.

- Bir VM, altyapı hatası veya düşük düzeyyükseltme nedeniyle taşınır. Toplu işlem düğümü kurtarır.

- Donanımda onu desteklemeyen bir VM görüntüsü dağıtıldı. Örneğin, [bir Standard_D1_v2](../virtual-machines/dv2-dsv2-series.md) VM'de CentOS HPC görüntüsünü çalıştırmaya çalışıyorum.

- Sanal M'ler bir [Azure sanal ağındadır](batch-virtual-network.md)ve trafik önemli bağlantı noktalarına engellenmiştir.

- SANAL M'ler sanal bir ağdadır, ancak Azure depolamasına giden trafik engellenir.

- Sanal M'ler, müşteri DNS yapılandırması olan sanal bir ağdadır ve DNS sunucusu Azure depolamasını çözemez.

### <a name="node-agent-log-files"></a>Düğüm aracısı günlük dosyaları

Her havuz düğümünde çalışan Toplu Iş aracısı işlemi, bir havuz düğümü sorunu hakkında desteğe başvurmanız gerektiğinde yararlı olabilecek günlük dosyaları sağlayabilir. Bir düğümün günlük dosyaları Azure portalı, Toplu Gezgin veya [API](/rest/api/batchservice/computenode/uploadbatchservicelogs)üzerinden yüklenebilir. Günlük dosyalarını yüklemek ve kaydetmek yararlıdır. Daha sonra, çalışan düğümlerin maliyetini kaydetmek için düğümü veya havuzu silebilirsiniz.

### <a name="node-disk-full"></a>Düğüm diski tam

Havuz düğümü VM için geçici sürücü, iş dosyaları, görev dosyaları ve paylaşılan dosyalar için Toplu İşlem tarafından kullanılır.

- Uygulama paketleri dosyaları
- Görev kaynağı dosyaları
- Toplu Işlem klasörlerinden birine indirilen uygulamaya özgü dosyalar
- Her görev uygulama yürütme için stdout ve stderr dosyaları
- Uygulamaya özgü çıktı dosyaları

Bu dosyalardan bazıları, havuz uygulama paketleri veya havuz başlangıç görev kaynağı dosyaları gibi havuz düğümleri oluşturulduğunda yalnızca bir kez yazılır. Düğüm oluşturulduğunda yalnızca bir kez yazılmış olsa bile, bu dosyalar çok büyükse geçici sürücüyü doldurabilirler.

Diğer dosyalar, stdout ve stderr gibi bir düğüm üzerinde çalıştırılan her görev için yazılır. Aynı düğümüzerinde çok sayıda görev çalıştırılırsa ve/veya görev dosyaları çok büyükse, geçici sürücüyü doldurabilirler.

Geçici sürücünün boyutu VM boyutuna bağlıdır. VM boyutu seçerken göz önünde bulundurulması gereken bir şey, geçici sürücünün yeterli alana sahip olmasını sağlamaktır.

- Azure portalında havuz eklerken, VM boyutlarının tam listesi görüntülenebilir ve bir 'Kaynak Disk Boyutu' sütunu bulunur.
- Tüm VM boyutlarını açıklayan makalelerde 'Geçici Depolama' sütununa sahip tablolar vardır; örneğin [İşlem Optimize Edilmiş VM boyutları](/azure/virtual-machines/windows/sizes-compute)

Her görev tarafından yazılan dosyalar için, görev dosyalarının otomatik olarak temizlenmeden önce ne kadar süreyle tutulabileceğini belirleyen her görev için bir bekletme süresi belirtilebilir. Saklama süresi depolama gereksinimlerini azaltmak için azaltılabilir.


Geçici diskin alanı biterse (veya boş alan tükenmeye çok yakınsa), düğüm [Kullanılamaz](/rest/api/batchservice/computenode/get#computenodestate) durumuna taşınır ve diskin dolu olduğunu belirten bir düğüm hatası bildirilir.

### <a name="what-to-do-when-a-disk-is-full"></a>Bir disk dolduğunda ne yapmalı?

Diskin neden dolu olduğunu belirleyin: Düğümde neyin yer kapladığından emin değilseniz, düğüme uzaktan uzaklaşması ve alanın nereye gittiğini el ile araştırmanız önerilir. Toplu İş Bölümü yönetilen klasörlerdeki dosyaları (örneğin, görev çıktıları) incelemek için [Toplu Liste Dosyaları API'sini](https://docs.microsoft.com/rest/api/batchservice/file/listfromcomputenode) de kullanabilirsiniz. Bu API'nin yalnızca Toplu İşlem yönetilen dizinlerindeki dosyaları listelediğini ve görevleriniz başka yerlerde dosyalar oluşturduysa bunları göremeyeceğiniunutmayın.

İhtiyacınız olan verilerin düğümden alındığından veya dayanıklı bir depoya yüklendiğinden emin olun. Disk dolu sorunun tüm azaltıcı etkenleri, alanı boşaltmak için verilerin silmesini içerir.

### <a name="recovering-the-node"></a>Düğümü kurtarma

1. Havuzunuz bir [C.loudServiceConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#cloudserviceconfiguration) havuzuysa, [Toplu yeniden görüntü API'si](https://docs.microsoft.com/rest/api/batchservice/computenode/reimage)aracılığıyla düğümü yeniden görüntüleyebilirsiniz. Bu, tüm diski temizler. Yeniden görüntü şu anda [VirtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration) havuzları için desteklenmez.

2. Havuzunuz bir [VirtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration)ise, [kaldır düğümleri API](https://docs.microsoft.com/rest/api/batchservice/pool/removenodes)kullanarak havuzdan düğüm kaldırabilirsiniz. Daha sonra, yeni bir ile kötü düğüm yerine tekrar havuz büyüyebilir.

3.  Görev verileri düğümlerde olan eski tamamlanmış işleri veya eski tamamlanmış görevleri silin. Düğümlerde iş/görev verilerinin ne olduğuna dair ipucu için düğümdeki [Son Görevler koleksiyonuna](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskinformation) veya [düğümdeki dosyalara](https://docs.microsoft.com//rest/api/batchservice/file/listfromcomputenode)bakabilirsiniz. İşi silmek, işteki tüm görevleri siler ve işteki görevlerin silineceği düğümdeki verileri tetikler ve böylece yer açılır. Yeterli alanı serbest bıraktıktan sonra düğümü yeniden başlatın ve düğümün "Kullanılamaz" durumundan çıkıp tekrar "Boşta" durumuna geçmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı özellikle eşzamanlı işlemler için kapsamlı hata denetimi uygulayacak şekilde ayarladığınızdan denetleyin. Sorunları hemen algılamak ve tanılamak çok önemli olabilir.
