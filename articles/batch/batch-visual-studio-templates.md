---
title: Visual Studio şablonları ile çözümler oluşturun - Azure Toplu İş | Microsoft Dokümanlar
description: Visual Studio proje şablonlarının Azure Toplu İş'te bilgi işlem yoğun iş yüklerinizi uygulamanıza ve çalıştırmanıza nasıl yardımcı olabileceğini öğrenin.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: a71dbd1b38ff58ccf1eb7a4d50daad5b24922e2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022758"
---
# <a name="use-visual-studio-project-templates-to-jump-start-batch-solutions"></a>Toplu Iş çözümlerini başlatmak için Visual Studio proje şablonlarını kullanma

Toplu İşlem için **İş Yöneticisi** ve **Görev İşlemci Görsel Stüdyosu şablonları,** işlem yoğun iş yüklerinizi en az çabayla Toplu İşlem'de uygulamanıza ve çalıştırmanıza yardımcı olacak kod sağlar. Bu belge, bu şablonları açıklar ve bunların nasıl kullanılacağı nasıI rehberlik sağlar.

> [!IMPORTANT]
> Bu makalede yalnızca bu iki şablon için geçerli olan bilgiler açıklanır ve Toplu İşlem hizmetine ve bununla ilgili temel kavramlara aşina olduğunuzu varsayar: havuzlar, bilgi işlem düğümleri, iş ve görevler, iş yöneticisi görevleri, çevre değişkenleri ve diğer ilgili Bilgi. Geliştiriciler için Azure Toplu İş ve [Toplu İş Temelleri'nde](batch-api-basics.md)daha fazla bilgi bulabilirsiniz. [Basics of Azure Batch](batch-technical-overview.md)
> 
> 

## <a name="high-level-overview"></a>Yüksek düzey genel bakış
İş Yöneticisi ve Görev İşlemcisi şablonları iki yararlı bileşen oluşturmak için kullanılabilir:

* Bir işi paralel olarak bağımsız olarak çalıştırabilen birden çok göreve bölebilen bir iş ayırıcısı uygulayan iş yöneticisi görevi.
* Bir uygulama komut satırı etrafında ön işleme ve post-processing gerçekleştirmek için kullanılabilecek bir görev işlemcisi.

Örneğin, bir film oluşturma senaryosunda, iş bölücü tek bir film işini tek tek kareleri ayrı ayrı işleyen yüzlerce veya binlerce ayrı göreve dönüştürür. Buna bağlı olarak, görev işlemcisi işleme uygulamasını ve her kareyi işlemek için gereken tüm bağımlı işlemleri çağırır ve ek eylemler gerçekleştirir (örneğin, işlenen çerçeveyi bir depolama konumuna kopyalama).

> [!NOTE]
> İş Yöneticisi ve Görev İşlemci şablonları birbirinden bağımsızdır, bu nedenle bilgi işlem işinizin gereksinimlerine ve tercihlerinize bağlı olarak her ikisini veya yalnızca birini kullanmayı seçebilirsiniz.
> 
> 

Aşağıdaki diyagramda gösterildiği gibi, bu şablonları kullanan bir bilgi işlem işi üç aşamadan geçer:

1. İstemci kodu (örn. uygulama, web hizmeti, vb.) iş yöneticisinin iş yöneticisi programı olarak belirtildiği şekilde Azure'daki Toplu İşlem hizmetine bir iş gönderir.
2. Toplu İşlem hizmeti, iş yöneticisi görevini bir bilgi işlem düğümünde çalıştırır ve iş bölücü, iş bölücü kodundaki parametrelere ve teknik özelliklere bağlı olarak, gerektiği kadar işlem düğümünde belirtilen sayıda görev işlemcisi görevi başlatın.
3. Görev işlemcisi görevleri, giriş verilerini işlemek ve çıktı verilerini oluşturmak için bağımsız olarak, paralel olarak çalışır.

![İstemci kodunun Toplu İşlem hizmetiyle nasıl etkileşimde olduğunu gösteren diyagram][diagram01]

## <a name="prerequisites"></a>Ön koşullar
Toplu İşlem şablonlarını kullanmak için aşağıdakilere ihtiyacınız olacaktır:

* Visual Studio 2015 yüklü bir bilgisayar. Toplu iş şablonları şu anda yalnızca Visual Studio 2015 için desteklenir.
* Visual Studio Gallery'den Visual [Studio][vs_gallery] uzantıları olarak kullanılabilen Toplu iş şablonları. Şablonları almanın iki yolu vardır:
  
  * Visual Studio'daki **Uzantılar ve Güncellemeler** iletişim kutusunu kullanarak şablonları yükleyin (daha fazla bilgi için [Visual Studio Uzantılarını Bulma ve Kullanma][vs_find_use_ext]' ya bakın). **Uzantılar ve Güncelleştirmeler** iletişim kutusunda, aşağıdaki iki uzantıyı arayın ve indirin:
    
    * İş Bölenli Azure Toplu İş Yöneticisi
    * Azure Toplu Görev İşlemcisi
  * Visual Studio: [Microsoft Azure Toplu İş Projesi Şablonları][vs_gallery_templates] için çevrimiçi galeriden şablonları indirin
* İş yöneticisi ve görev işlemcisini Toplu İşlem düğümlerine dağıtmak için [Uygulama Paketleri](batch-application-packages.md) özelliğini kullanmayı planlıyorsanız, bir depolama hesabını Toplu Iş hesabınıza bağlamanız gerekir.

## <a name="preparation"></a>Hazırlık
İş yöneticinizin yanı sıra görev işlemcinizi de içerebilecek bir çözüm oluşturmanızı öneririz, çünkü bu, iş yöneticiniz ve görev işlemcisi programlarınız arasında kod paylaşımını kolaylaştırabilir. Bu çözümü oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio'u açın ve **Dosya** > **Yeni** > **Projesi'ni**seçin.
2. **Şablonlar**altında, **Diğer Proje Türlerini**genişletin, **Visual Studio Çözümleri'ni**tıklatın ve ardından **Boş Çözüm'u**seçin.
3. Uygulamanızı ve bu çözümün amacını açıklayan bir ad yazın (örn. "LitwareBatchTaskPrograms").
4. Yeni çözümü oluşturmak için **Tamam'ı**tıklatın.

## <a name="job-manager-template"></a>İş Yöneticisi şablonu
İş Yöneticisi şablonu, aşağıdaki eylemleri gerçekleştirebilecek bir iş yöneticisi görevi uygulamanıza yardımcı olur:

* Bir işi birden çok göreve bölün.
* Toplu Iş üzerinde çalıştırmak için bu görevleri gönderin.

> [!NOTE]
> İş yöneticisi görevleri hakkında daha fazla bilgi için [geliştiriciler için Toplu Iş özelliğine genel bakış](batch-api-basics.md#job-manager-task)alameti'ne bakın.
> 
> 

### <a name="create-a-job-manager-using-the-template"></a>Şablonu kullanarak Bir İş Yöneticisi Oluşturma
Daha önce oluşturduğunuz çözüme bir iş yöneticisi eklemek için aşağıdaki adımları izleyin:

1. Mevcut çözümünüzü Visual Studio'da açın.
2. Çözüm Gezgini'nde, çözüme sağ tıklayın,**Yeni Proje Ekle'yi** **Add** > tıklatın.
3. **Visual C#** **altında, Bulut'u**tıklatın ve ardından **İş Bölücü ile Azure Toplu İş Yöneticisi'ni**tıklatın.
4. Uygulamanızı açıklayan ve bu projeyi iş yöneticisi olarak tanımlayan bir ad yazın (örn. "LitwareJobManager").
5. Projeyi oluşturmak için **Tamam'ı**tıklatın.
6. Son olarak, Visual Studio'yu başvurulan tüm NuGet paketlerini yüklemeye zorlamak ve projeyi değiştirmeye başlamadan önce projenin geçerli olduğunu doğrulamak için projeyi oluşturun.

### <a name="job-manager-template-files-and-their-purpose"></a>İş Yöneticisi şablon dosyaları ve amaçları
İş Yöneticisi şablonu kullanarak bir proje oluşturduğunuzda, üç kod dosyası grubu oluşturur:

* Ana program dosyası (Program.cs). Bu, program giriş noktasını ve üst düzey özel durum işlemeyi içerir. Normalde bunu değiştirmeniz gerekmez.
* Çerçeve dizini. Bu, iş yöneticisi programı tarafından yapılan 'kazan plakası' çalışmadan sorumlu dosyaları içerir – parametrelerin boşaltılması, Toplu iş için görevlerin eklenmesi, vb. Normalde bu dosyaları değiştirmeniz gerekmez.
* İş bölücü dosyası (JobSplitter.cs). Bir işi görevlere bölmek için uygulamaya özgü mantığınızı burada koyacağız.

Tabii ki iş bölme mantığı karmaşıklığına bağlı olarak, iş bölücü kodunu desteklemek için gerekli ek dosyalar ekleyebilirsiniz.

Şablon ayrıca .csproj dosyası, app.config, packages.config, vb. gibi standart .NET proje dosyaları da oluşturur.

Bu bölümün geri kalanı farklı dosyaları ve kod yapılarını açıklar ve her sınıfın ne yaptığını açıklar.

![İş Yöneticisi şablon çözümlerini gösteren Visual Studio Solution Explorer][solution_explorer01]

**Çerçeve dosyaları**

* `Configuration.cs`: Toplu iş hesabı ayrıntıları, bağlantılı depolama hesabı kimlik bilgileri, iş ve görev bilgileri ve iş parametreleri gibi iş yapılandırma verilerinin yüklenmesi özetlenir. Ayrıca Configuration.EnvironmentVariable sınıfı aracılığıyla Toplu Olarak tanımlanan ortam değişkenlerine (Toplu İşlem belgelerinde görevler için Ortam ayarlarına bakın) erişim sağlar.
* `IConfiguration.cs`: Sahte veya sahte yapılandırma nesnesi kullanarak iş bölücünüzü birim olarak test edebilmeniz için Configuration sınıfının uygulanmasını özetler.
* `JobManager.cs`: İş yöneticisi programının bileşenlerini yönetir. İş bölücünün başlatılmasından, iş bölücüse çağrıda ndan ve iş bölücü tarafından döndürülen görevlerin görev gönderene gönderilmesinden sorumludur.
* `JobManagerException.cs`: İş yöneticisinin sonlandırmasını gerektiren bir hatayı temsil eder. JobManagerException, sonlandırmanın bir parçası olarak belirli tanılama bilgilerinin sağlanabileceği 'beklenen' hataları sarmak için kullanılır.
* `TaskSubmitter.cs`: Bu sınıf, iş bölücü tarafından toplu iş için döndürülen görevleri eklemekten sorumludur. JobManager sınıfı, göreve verimli ancak zamanında eklenmesi için görev sırasını toplu olarak toplar ve ardından tasksubmitter.submittasks'i her bir toplu iş için bir arka plan iş parçacığı üzerinde çağırır.

**İş Bölücü**

`JobSplitter.cs`: Bu sınıf, işi görevlere bölmek için uygulamaya özgü mantık içerir. Çerçeve, yöntem bunları döndürdükçe işe eklediği bir dizi görev elde etmek için JobSplitter.Split yöntemini çağırır. Bu, işinizin mantığını enjekte edeceğiniz sınıftır. İşi bölmek istediğiniz görevleri temsil eden bir cloudtask örneği dizisini döndürmek için Bölme yöntemini uygulayın.

**Standart .NET komut satırı proje dosyaları**

* `App.config`: Standart .NET uygulama yapılandırma dosyası.
* `Packages.config`: Standart NuGet paket bağımlılık dosyası.
* `Program.cs`: Program giriş noktasını ve üst düzey özel durum işlemeyi içerir.

### <a name="implementing-the-job-splitter"></a>İş bölücü uygulaması
İş Yöneticisi şablonu projesini açtığınızda, proje varsayılan olarak JobSplitter.cs dosyayı açar. Aşağıdaki Split() yöntemini kullanarak iş yükünüzdeki görevlerin bölme mantığını uygulayabilirsiniz:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

> [!NOTE]
> `Split()` Yöntemdeki açıklamalı bölüm, İş Yöneticisi şablon kodunun, işlerinizi farklı görevlere bölmek için mantığı ekleyerek değiştirmeniz için tasarlanmış tek bölümüdür. Şablonun farklı bir bölümünü değiştirmek istiyorsanız, lütfen Toplu İşlem'in nasıl çalıştığını bildiğinizden emin olun ve [Toplu İş kodu örneklerinden][github_samples]birkaçını deneyin.
> 
> 

Split() uygulamanızın şu larına erişimi vardır:

* İş parametreleri, `_parameters` alan üzerinden.
* Alanı üzerinden işi temsil eden CloudJob nesnesi. `_job`
* Alan üzerinden iş yöneticisi görevini temsil eden `_jobManagerTask` CloudTask nesnesi.

Uygulamanızın `Split()` doğrudan işe görev eklemesine gerek yoktur. Bunun yerine, kodunuz bir CloudTask nesne dizisi döndürmelidir ve bunlar iş ayırıcısını çağıran çerçeve sınıfları tarafından otomatik olarak işe eklenir. Tüm görevlerin hesaplanmasını beklemek yerine görevlerin`yield return`mümkün olan en kısa sürede çalışmaya başlamasına izin verdiğinden, iş bölücülerini uygulamak için C#'ın yineleyici ( ) özelliğini kullanmak yaygındır.

**İş bölücü hatası**

İş ayırıcınız bir hatayla karşılaşırsa, aşağıdakilerden biri olmalıdır:

* C# `yield break` deyimini kullanarak sırayı sonlandırın, bu durumda iş yöneticisi başarılı olarak kabul edilir; Veya
* Bir özel durum at, bu durumda iş yöneticisi başarısız olarak kabul edilir ve istemci nin nasıl yapılandırdığına bağlı olarak yeniden denenebilir).

Her iki durumda da, iş bölücü tarafından döndürülen ve Toplu Iş'e eklenen tüm görevler çalıştırılabilir. Bunun olmasını istemiyorsan, şunları yapabilirsiniz:

* İş bölücüden dönmeden önce işi sonlandırma
* İade etmeden önce tüm görev koleksiyonunu formüle edin (diğer bir şekilde, c# yineleyici kullanarak iş ayırıcınızı uygulamak yerine bir `ICollection<CloudTask>` veya `IList<CloudTask>` yerine döndürün)
* Tüm görevleri iş yöneticisinin başarıyla tamamlanmasına bağlı hale getirmek için görev bağımlılıklarını kullanın

**İş yöneticisi yeniden deneme**

İş yöneticisi başarısız olursa, istemci yeniden deneme ayarlarına bağlı olarak Toplu İşlem hizmeti tarafından yeniden denenebilir. Genel olarak, bu güvenlidir, çünkü çerçeve işe görevler eklediğinde, zaten var olan tüm görevleri yok sayar. Ancak, görevleri hesaplamak pahalıysa, zaten işe eklenmiş olan görevleri yeniden hesaplama maliyetine maruz kalmak istemeyebilirsiniz; tersine, yeniden çalıştırma aynı görev adlarını oluşturmak için garanti edilmezse, 'yinelenenleri yoksay' davranışı devreye alınmaz. Bu gibi durumlarda, örneğin görevleri vermeye başlamadan önce cloudjob.listtasks gerçekleştirerek, daha önce yapılmış olan işi algılamak ve yinelememek için iş ayırıcınızı tasarlamanız gerekir.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>İş Yöneticisi şablonundaki çıkış kodları ve özel durumlar
Çıkış kodları ve özel durumlar, bir programı çalıştırmanın sonucunu belirlemek için bir mekanizma sağlar ve programın yürütülmesiyle ilgili sorunları belirlemeye yardımcı olabilir. İş Yöneticisi şablonu, bu bölümde açıklanan çıkış kodlarını ve özel durumlarını uygular.

İş Yöneticisi şablonuyla uygulanan bir iş yöneticisi görevi üç olası çıkış kodu döndürebilir:

| Kod | Açıklama |
| --- | --- |
| 0 |İş yöneticisi başarıyla tamamlandı. İş bölücü kodunuz tamamlanmak üzere yken tüm görevler işe eklendi. |
| 1 |İş yöneticisi görevi, programın 'beklenen' bölümünde bir özel durum la başarısız oldu. Özel durum, tanılama bilgileri ve mümkünse başarısızlığı niçin çözüme yönelik öneriler içeren bir JobManagerException'a çevrildi. |
| 2 |İş yöneticisi görevi 'beklenmeyen' bir özel durumla başarısız oldu. Özel durum standart çıktıya günlüğe kaydedildi, ancak iş yöneticisi ek tanılama veya düzeltme bilgileri ekleyemedi. |

İş yöneticisi görev hatası durumunda, hata oluşmadan önce bazı görevler yine de hizmete eklenmiş olabilir. Bu görevler normal olarak çalışacaktır. Bu kod yolunutartışmak için yukarıdaki "İş Bölünen Hata" bölümüne bakın.

İstisnalarla döndürülen tüm bilgiler stdout.txt ve stderr.txt dosyalarına yazılır. Daha fazla bilgi için [Hata İşleme'ye](batch-api-basics.md#error-handling)bakın.

### <a name="client-considerations"></a>İstemci hususları
Bu bölümde, bu şablona dayalı bir iş yöneticisi çağırıldığında bazı istemci uygulama gereksinimleri açıklanır. Parametreleri ve ortam ayarlarını geçirme yle ilgili ayrıntılar için [parametreleri ve ortam değişkenlerini istemci kodundan nasıl geçirin.](#pass-environment-settings)

**Zorunlu kimlik bilgileri**

Azure Toplu İş işine görev eklemek için iş yöneticisi nin görevi için Azure Toplu İş hesabı URL'niz ve anahtarınız gerekiyor. Bunları YOUR_BATCH_URL ve YOUR_BATCH_KEY adlı ortam değişkenlerinde geçirmeniz gerekir. Bunları İş Yöneticisi görev ortamı ayarlarında ayarlayabilirsiniz. Örneğin, bir C# istemcisinde:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Depolama kimlik bilgileri**

Genellikle, (a) çoğu iş yöneticisinin bağlantılı depolama hesabına açıkça erişmesi gerekmedığından ve (b) bağlı depolama hesabı genellikle tüm görevlere iş için ortak ortam ayarı. Bağlantılı depolama hesabını ortak ortam ayarları üzerinden sağlayamıyorsanız ve iş yöneticisi bağlantılı depolamaya erişim gerektiriyorsa, bağlantılı depolama kimlik bilgilerini aşağıdaki gibi sağlamanız gerekir:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**İş yöneticisi görev ayarları**

İstemci iş yöneticisi *öldürmek JobOnCompletion* bayrağı **yanlış**ayarlamalıdır.

Genellikle istemci için *runExclusive'i* **yanlış**olarak ayarlamak güvenlidir.

İstemci, iş yöneticisinin çalıştırılabilir (ve gerekli DLL'leri) bilgi işlem düğümüne dağıtılması için *kaynakDosyaları* veya *applicationPackageReferences* koleksiyonunu kullanmalıdır.

Varsayılan olarak, başarısız olursa iş yöneticisi yeniden denenmez. İş yöneticisi mantığınıza bağlı olarak, istemci/*kısıtlamalar maxTaskRetryCount* *üzerinden*yeniden denemeleri etkinleştirmek isteyebilirsiniz.

**İş ayarları**

İş bölücü görevleri bağımlılıklarla yayarsa, istemci nin işin kullanımlarını TaskDependencies'i doğru ayarlaması gerekir.

İş bölücü modelinde, istemcilerin iş bölücünün oluşturduklarının üzerinde ve üstündeki işlere görev eklemek istemeleri alışılmadık bir durumdur. Bu nedenle istemci normalde **işi sonlandırmak**için *işin onAllTasksComplete* ayarlamalıdır.

## <a name="task-processor-template"></a>Görev İşlemcisi şablonu
Görev İşlemcisi şablonu, aşağıdaki eylemleri gerçekleştirebilen bir görev işlemcisi uygulamanıza yardımcı olur:

* Çalıştırmak için her Toplu İşlem görevi nin gerektirdiği bilgileri ayarlayın.
* Her Toplu İşlem görevi nin gerektirdiği tüm eylemleri çalıştırın.
* Görev çıktılarını kalıcı depolama alanına kaydedin.

Toplu İş'te görevleri çalıştırmak için bir görev işlemcisi gerekli olmasa da, görev işlemcisi kullanmanın en önemli avantajı, tüm görev yürütme eylemlerini tek bir konumda uygulamak için bir sarıcı sağlamasıdır. Örneğin, her görev bağlamında birden fazla uygulama çalıştırmanız gerekiyorsa veya her görevi tamamladıktan sonra verileri kalıcı depolama alanına kopyalamanız gerekiyorsa.

Görev işlemcisi tarafından gerçekleştirilen eylemler, iş yükünüzün gerektirdiği kadar basit veya karmaşık ve çok veya az olabilir. Ayrıca, tüm görev eylemlerini tek bir görev işlemcisine uygulayarak, uygulamalardaki veya iş yükü gereksinimlerindeki değişiklikleri temel alan eylemleri kolayca güncelleyebilir veya ekleyebilirsiniz. Ancak, bazı durumlarda bir görev işlemcisi, örneğin basit bir komut satırından hızlı bir şekilde başlatılabilen işleri çalıştırırken gereksiz karmaşıklık ekleyebileceğinden, uygulamanız için en uygun çözüm olmayabilir.

### <a name="create-a-task-processor-using-the-template"></a>Şablonu kullanarak Görev İşlemcisi Oluşturma
Daha önce oluşturduğunuz çözüme görev işlemcisi eklemek için aşağıdaki adımları izleyin:

1. Mevcut çözümünüzü Visual Studio'da açın.
2. Çözüm Gezgini'nde, çözüme sağ tıklayın, **Ekle'yi**tıklatın ve ardından **Yeni Proje'yi**tıklatın.
3. **Visual C#** **altında, Bulut'u**tıklatın ve ardından **Azure Toplu Görev İşlemcisi'ni**tıklatın.
4. Uygulamanızı açıklayan ve bu projeyi görev işlemcisi olarak tanımlayan bir ad yazın (örn. "LitwareTaskProcessor").
5. Projeyi oluşturmak için **Tamam'ı**tıklatın.
6. Son olarak, Visual Studio'yu başvurulan tüm NuGet paketlerini yüklemeye zorlamak ve projeyi değiştirmeye başlamadan önce projenin geçerli olduğunu doğrulamak için projeyi oluşturun.

### <a name="task-processor-template-files-and-their-purpose"></a>Görev İşlemcisi şablon dosyaları ve amaçları
Görev işlemcisi şablonu kullanarak bir proje oluşturduğunuzda, üç kod dosyası grubu oluşturur:

* Ana program dosyası (Program.cs). Bu, program giriş noktasını ve üst düzey özel durum işlemeyi içerir. Normalde bunu değiştirmeniz gerekmez.
* Çerçeve dizini. Bu, iş yöneticisi programı tarafından yapılan 'kazan plakası' çalışmadan sorumlu dosyaları içerir – parametrelerin boşaltılması, Toplu iş için görevlerin eklenmesi, vb. Normalde bu dosyaları değiştirmeniz gerekmez.
* Görev işlemci dosyası (TaskProcessor.cs). Bu, bir görevi yürütmek için uygulamaya özgü mantığınızı (genellikle varolan bir yürütülebilir ekive çağırarak) koyacağınız yerdir. Ek veri indirme veya sonuç dosyaları yükleme gibi işleme öncesi ve sonrası kod da buraya gider.

Tabii ki iş bölme mantığıkarmaşıklığına bağlı olarak, görev işlemci kodunu desteklemek için gerekli ek dosyalar ekleyebilirsiniz.

Şablon ayrıca .csproj dosyası, app.config, packages.config, vb. gibi standart .NET proje dosyaları da oluşturur.

Bu bölümün geri kalanı farklı dosyaları ve kod yapılarını açıklar ve her sınıfın ne yaptığını açıklar.

![Görev İşlemcisi şablon çözümlerini gösteren Visual Studio Solution Explorer][solution_explorer02]

**Çerçeve dosyaları**

* `Configuration.cs`: Toplu iş hesabı ayrıntıları, bağlantılı depolama hesabı kimlik bilgileri, iş ve görev bilgileri ve iş parametreleri gibi iş yapılandırma verilerinin yüklenmesi özetlenir. Ayrıca Configuration.EnvironmentVariable sınıfı aracılığıyla Toplu Olarak tanımlanan ortam değişkenlerine (Toplu İşlem belgelerinde görevler için Ortam ayarlarına bakın) erişim sağlar.
* `IConfiguration.cs`: Sahte veya sahte yapılandırma nesnesi kullanarak iş bölücünüzü birim olarak test edebilmeniz için Configuration sınıfının uygulanmasını özetler.
* `TaskProcessorException.cs`: İş yöneticisinin sonlandırmasını gerektiren bir hatayı temsil eder. TaskProcessorException, sonlandırmanın bir parçası olarak belirli tanılama bilgilerinin sağlanabileceği 'beklenen' hataları sarmak için kullanılır.

**Görev İşlemcisi**

* `TaskProcessor.cs`: Görevi çalıştırAn. Çerçeve TaskProcessor.Run yöntemini çağırır. Bu, görevinizin uygulamaya özgü mantığını enjekte edeceğiniz sınıftır. Çalıştır yöntemini uygulayın:
  * Herhangi bir görev parametrelerini ayrıştın ve doğrulayın
  * Çağırmak istediğiniz herhangi bir harici program için komut satırını oluşturun
  * Hata ayıklama amacıyla gerektirebilecek tanılama bilgilerini kaydedin
  * Bu komut satırLarını kullanarak bir işlem başlatma
  * İşlemin çıkmasını bekleyin
  * Başarılı olup olmadığını veya başarısız olup olmadığını belirlemek için işlemin çıkış kodunu yakalama
  * Kalıcı depolama alanına saklamak istediğiniz çıktı dosyalarını kaydetme

**Standart .NET komut satırı proje dosyaları**

* `App.config`: Standart .NET uygulama yapılandırma dosyası.
* `Packages.config`: Standart NuGet paket bağımlılık dosyası.
* `Program.cs`: Program giriş noktasını ve üst düzey özel durum işlemeyi içerir.

## <a name="implementing-the-task-processor"></a>Görev işlemcisini uygulama
Görev İşlemcisi şablonu projesini açtığınızda, proje varsayılan olarak TaskProcessor.cs dosyayı açar. Aşağıda gösterilen Çalıştır() yöntemini kullanarak iş yükünüzdeki görevlerin çalışma mantığını uygulayabilirsiniz:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
> [!NOTE]
> Çalıştır() yöntemindeki açıklamalı bölüm, Görev İşlemcisi şablon kodunun iş yükündeki görevlerin çalışma mantığını ekleyerek değiştirmeniz için tasarlanmış tek bölümüdür. Şablonun farklı bir bölümünü değiştirmek istiyorsanız, lütfen toplu iş belgelerini inceleyerek ve Toplu İş kodu örneklerinden birkaçını deneyerek toplu iş lerle ilgili bilgi edinin.
> 
> 

Run() yöntemi komut satırını başlatmaktan, bir veya daha fazla işlemi başlatmaktan, tüm işlemlerin tamamlanmasını beklemekten, sonuçları kaydetmekten ve son olarak bir çıkış koduyla dönmekten sorumludur. Run() yöntemi, görevleriniziçin işleme mantığını uyguladığınız yöntemdir. Görev işlemcisi çerçevesi sizin için Çalıştır() yöntemini çağırır; kendiniz demeye gerek yok.

Run() uygulamanızın şu larına erişimi vardır:

* Görev parametreleri, `_parameters` alan üzerinden.
* İş ve görev kimlikleri, `_jobId` `_taskId` ve alanları üzerinden.
* Görev yapılandırması, `_configuration` alan üzerinden.

**Görev hatası**

Hata durumunda, bir özel durum atarak Çalıştır() yönteminden çıkabilirsiniz, ancak bu, görev çıkış kodunun denetiminde üst düzey özel durum işleyicisini bırakır. Çıkış kodunu, örneğin tanılama amacıyla farklı hata türlerini ayırt edebilmeniz için denetlemeniz gerekiyorsa veya bazı hata modları işi sonlandırmalı ve diğerleri sona erdirmemelidir, o zaman sıfır olmayan bir döndürme ile Çalıştır() yönteminden çıkmalısınız çıkış kodu. Bu, görev çıkış kodu olur.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Görev İşlemcisi şablonundaki çıkış kodları ve özel durumlar
Çıkış kodları ve özel durumlar, bir programı çalıştırmanın sonucunu belirlemek için bir mekanizma sağlar ve programın yürütülmesiyle ilgili sorunları belirlemeye yardımcı olabilir. Görev İşlemcisi şablonu, bu bölümde açıklanan çıkış kodlarını ve özel durumlarını uygular.

Görev İşlemcisi şablonuyla uygulanan bir görev işlemcisi görevi üç olası çıkış kodunu döndürebilir:

| Kod | Açıklama |
| --- | --- |
| [Process.ExitCode][process_exitcode] |Görev işlemcisi tamamlanmak üzere koştu. Bunun, çağırdığınız programın başarılı olduğu anlamına gelmez - yalnızca görev işlemcisinin bu programı başarıyla çağırdığını ve istisnasız herhangi bir işlem sonrası gerçekleştirdiğini unutmayın. Çıkış kodunun anlamı çağrılan programa bağlıdır – genellikle çıkış kodu 0, programın başarılı olduğu anlamına gelir ve diğer çıkış kodu programın başarısız olduğu anlamına gelir. |
| 1 |Görev işlemcisi, programın 'beklenen' bölümünde bir özel durum dışında başarısız oldu. Özel durum tanılama `TaskProcessorException` bilgileri ve mümkünse, başarısızlığı çözmek için öneriler ile bir çevrildi. |
| 2 |Görev işlemcisi 'beklenmeyen' bir özel durumla başarısız oldu. Özel durum standart çıktıya günlüğe kaydedildi, ancak görev işlemcisi ek tanılama veya düzeltme bilgileri ekleyemedi. |

> [!NOTE]
> Çağırdığınız program belirli hata modlarını belirtmek için çıkış kodları 1 ve 2 kullanıyorsa, görev işlemcihataları için çıkış kodları 1 ve 2'yi kullanmak belirsizdir. Bu görev işlemcisi hata kodlarını, Program.cs dosyasındaki özel durum durumlarını düzenleyerek farklı çıkış kodlarıyla değiştirebilirsiniz.
> 
> 

İstisnalarla döndürülen tüm bilgiler stdout.txt ve stderr.txt dosyalarına yazılır. Daha fazla bilgi için Toplu İşlem belgelerinde Hata İşleme'ye bakın.

### <a name="client-considerations"></a>İstemci hususları
**Depolama kimlik bilgileri**

Görev işlemciniz çıktıları sürdürmek için Azure blob depolama alanını kullanıyorsa (örneğin dosya kuralları yardımcı kitaplığını kullanarak, bulut depolama hesabı kimlik *bilgilerine* *veya* paylaşılan erişim imzası (SAS) içeren blob kapsayıcı URL'sine erişmesi gerekir. Şablon, ortak ortam değişkenleri aracılığıyla kimlik bilgileri sağlamak için destek içerir. Müşteriniz depolama kimlik bilgilerini aşağıdaki gibi geçirebilir:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Depolama hesabı daha sonra `_configuration.StorageAccount` özellik üzerinden TaskProcessor sınıfında kullanılabilir.

SAS içeren bir kapsayıcı URL kullanmayı tercih ederseniz, bunu ortak bir ortam ayarı üzerinden de geçirebilirsiniz, ancak görev işlemcisi şablonu şu anda bunun için yerleşik destek içermemektedir.

**Depolama kurulumu**

İstemci veya iş yöneticisi görevi, görevleri işe eklemeden önce görevlerin gerektirdiği kapsayıcıları oluşturması önerilir. Bu, SAS içeren bir kapsayıcı URL kullanıyorsanız, örneğin bir URL kapsayıcıyı oluşturma izni içermediği için bu zorunludur. Kapsayıcıda CloudBlobContainer.CreateIfNotExistsAsync aramak zorunda her görevi kaydeder gibi, depolama hesabı kimlik bilgilerini geçirseniz bile önerilir.

## <a name="pass-parameters-and-environment-variables"></a>Geçiş parametreleri ve çevre değişkenleri
### <a name="pass-environment-settings"></a>Ortam ayarlarını geç
İstemci bilgileri iş yöneticisi görevine ortam ayarları şeklinde aktarabilir. Bu bilgiler daha sonra işlem işinin bir parçası olarak çalışacak görev işlemcisi görevleri oluştururken iş yöneticisi görevi tarafından kullanılabilir. Ortam ayarları olarak aktarabileceğiniz bilgilere örnekler:

* Depolama hesabı adı ve hesap anahtarları
* Toplu hesap URL'si
* Toplu hesap anahtarı

Toplu İşlem hizmeti, [Microsoft.Azure.Batch.JobManagerTask'taki][net_jobmanagertask] `EnvironmentSettings` özelliği kullanarak ortam ayarlarını iş yöneticisi görevine geçirmek için basit bir mekanizmaya sahiptir.

Örneğin, Toplu Iş `BatchClient` hesabı örneğini almak için, istemci kodundan URL'den ortam değişkenleri olarak geçebilir ve Toplu Iş hesabı için paylaşılan anahtar kimlik bilgilerini paylaşabilirsiniz. Aynı şekilde, Toplu İşlem hesabına bağlı depolama hesabına erişmek için depolama hesabı adını ve depolama hesabı anahtarını ortam değişkenleri olarak geçirebilirsiniz.

### <a name="pass-parameters-to-the-job-manager-template"></a>Parametreleri İş Yöneticisi şablonuna geçirin
Çoğu durumda, iş bölme işlemini denetlemek veya iş için görevleri yapılandırmak için iş başına parametreleri iş yöneticisi görevine aktarmak yararlıdır. Bunu, iş yöneticisi görevi için kaynak dosyası olarak parameters.json adlı bir JSON dosyasını yükleyerek yapabilirsiniz. Parametreler daha sonra İş `JobSplitter._parameters` Yöneticisi şablonundaki alanda kullanılabilir hale gelebilir.

> [!NOTE]
> Yerleşik parametre işleyicisi yalnızca dize-dize sözlükleri destekler. Karmaşık JSON değerlerini parametre değerleri olarak geçirmek istiyorsanız, bunları dizeleri olarak geçirmeniz ve bunları iş ayırıcısında ayrıştırman veya çerçevenin `Configuration.GetJobParameters` yöntemini değiştirmeniz gerekir.
> 
> 

### <a name="pass-parameters-to-the-task-processor-template"></a>Parametreleri Görev İşlemcisi şablonuna geçirin
Parametreleri Görev İşlemcisi şablonu kullanılarak uygulanan tek tek görevlere de geçirebilirsiniz. İş yöneticisi şablonunda olduğu gibi, görev işlemcisi şablonu da adlı bir kaynak dosyasını arar

parametreler.json ve eğer parametre sözlüğü olarak yükler bulundu. Parametrelerin görev işlemcisi görevlerine nasıl geçirilen ekiilere yönelik birkaç seçenek vardır:

* JSON iş parametrelerini yeniden kullanın. Bu, yalnızca iş çapındaki parametreler (örneğin, render yüksekliği ve genişliği) ise iyi çalışır. Bunu yapmak için, iş bölücüde bir CloudTask oluştururken, iş yöneticisi görevin Kaynak Dosyaları 'ndan (`JobSplitter._jobManagerTask.ResourceFiles`) CloudTask'ın Kaynak Dosyaları koleksiyonuna parametreler.json kaynak dosyası nesnesine bir başvuru ekleyin.
* İş bölücü yürütmenin bir parçası olarak göreve özgü bir parameters.json belgesi oluşturun ve yükleyin ve görevin kaynak dosyaları koleksiyonundaki blob'a başvurun. Farklı görevlerin farklı parametreleri varsa bu gereklidir. Çerçeve dizininin göreve parametre olarak aktarıldığı bir 3B oluşturma senaryosu örnek olabilir.

> [!NOTE]
> Yerleşik parametre işleyicisi yalnızca dize-dize sözlükleri destekler. Karmaşık JSON değerlerini parametre değerleri olarak geçirmek istiyorsanız, bunları dizeleri olarak geçirmeniz ve bunları görev işlemcisinde ayrıştırman veya çerçevenin `Configuration.GetTaskParameters` yöntemini değiştirmeniz gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
### <a name="persist-job-and-task-output-to-azure-storage"></a>İş ve görev çıktısını Azure Depolama'da kalıcı olarak sataştırın
Toplu Iş çözümü geliştirmede yararlı bir diğer araç da [Azure Toplu Dosya Kuralları'dır.][nuget_package] Toplu İşleme uygulamalarınızda bu .NET sınıf kitaplığını (şu anda önizlemede) kullanarak Azure Depolama'ya ve Azure Depolama'dan görev çıktılarını kolayca depolayın ve alın. [Kalıcı Azure Toplu Iş ve görev çıktısı](batch-task-output.md) kitaplık ve kullanımı tam bir tartışma içerir.


[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
