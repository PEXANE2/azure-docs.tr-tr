---
title: Windows 'da Azure Service Fabric uygulamalarında hata ayıklama
description: Yerel bir geliştirme makinesinde Microsoft Azure Service Fabric kullanarak yazılmış hizmetlerinizi izlemeyi ve tanılamayı öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 8435bb82afddd0070679768bb8d22ad9290f2279
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388405"
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Yerel makine geliştirme kurulumunda hizmetleri izleme ve tanılama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Hizmetlerin Kullanıcı deneyimine en düşük kesintilerle devam etmesine izin vermek için izleme, algılama, tanılama ve sorun giderme. İzleme ve tanılama, gerçek dağıtılan bir üretim ortamında önemli olmakla birlikte, verimlilik, hizmet geliştirme sırasında, gerçek dünya kurulumuna geçtiğinizde çalışmasını sağlamak için benzer bir modeli benimsemeye bağlıdır. Service Fabric, Service Developers 'ın hem tek makineli yerel geliştirme kurulumları hem de gerçek dünya üretim kümesi kurulumları üzerinde sorunsuz bir şekilde çalışması için tanılamayı uygulamasını kolaylaştırır.

## <a name="event-tracing-for-windows"></a>Windows için olay Izleme
[Windows Için olay izleme](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW), Service Fabric iletileri izlemek için önerilen teknolojiden sorumludur. ETW kullanmanın bazı avantajları şunlardır:

* **ETW hızlıdır.** Kod yürütme süreleri üzerinde en az etkiyle bir izleme teknolojisi olarak oluşturulmuştur.
* **ETW izleme, yerel geliştirme ortamları ve ayrıca gerçek dünya kümesi kurulumları arasında sorunsuz çalışır.** Bu, kodunuzu gerçek bir kümeye dağıtmaya hazırsanız izleme kodunuzu yeniden yazmanız gerekmediği anlamına gelir.
* **Service Fabric sistem kodu, iç izleme için ETW de kullanır.** Bu, uygulama izlemelerinizi Service Fabric sistem izlemeleri ile birlikte görüntülemenize olanak sağlar. Ayrıca, temel sistemdeki uygulama kodunuz ve olayları arasındaki dizileri ve ilişkileri daha kolay anlamanıza yardımcı olur.
* **Service Fabric Visual Studio Araçları 'nda ETW olaylarını görüntülemek için yerleşik destek vardır.** Visual Studio Service Fabric ile doğru şekilde yapılandırıldığında, ETW olayları Visual Studio 'nun tanılama olayları görünümünde görüntülenir. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Visual Studio 'da Service Fabric sistem olaylarını görüntüleme
Service Fabric, uygulama geliştiricilerinin platformda neler olduğunu anlamalarına yardımcı olmak için ETW olaylarını yayar. Daha önce yapmadıysanız, devam edin ve [Visual Studio 'da ilk uygulamanızı oluşturma](service-fabric-tutorial-create-dotnet-app.md)adımlarını izleyin. Bu bilgiler, izleme iletilerini gösteren tanılama olayları Görüntüleyicisi 'Ni kullanarak bir uygulamayı çalışır duruma getirmenize yardımcı olur.

1. Tanılama olayları penceresi otomatik olarak görünmüyorsa, Visual Studio 'daki **Görünüm** sekmesine gidin, **diğer pencereler** ' i ve ardından **Tanılama olayları Görüntüleyicisi**' ni seçin.
2. Her olay, olayın geldiği düğümü, uygulamayı ve hizmeti size bildiren standart meta veri bilgilerine sahiptir. Olayların listesini, olaylar penceresinin en üstündeki **filtre olayları** kutusunu kullanarak da filtreleyebilirsiniz. Örneğin, **düğüm adı** veya **hizmet adı** ' na filtre uygulayabilirsiniz. Olay ayrıntılarına baktığınızda, olaylar penceresinin üst kısmındaki **Duraklat** düğmesini kullanarak da duraklatabilirsiniz ve daha sonra herhangi bir olay kaybı olmadan devam edebilirsiniz.
   
   ![Visual Studio tanılama olayları Görüntüleyicisi](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-to-the-application-code"></a>Kendi özel izlemelerinizi uygulama koduna ekleyin
Service Fabric Visual Studio proje şablonları örnek kod içerir. Kod, Service Fabric 'den sistem izlemelerinin yanı sıra Visual Studio ETW görüntüleyicisinde görüntülenen özel uygulama kodu ETW izlemelerinin nasıl ekleneceğini gösterir. Bu yöntemin avantajı, meta verilerin izlemelere otomatik olarak eklendiğine ve Visual Studio tanılama olayları Görüntüleyicisi 'nin bunları görüntülemesi için zaten yapılandırılmış olması.

**Hizmet şablonlarından** (durum bilgisiz veya durum bilgisi olmayan) oluşturulan projeler için yalnızca `RunAsync` uygulamasını aratın:

1. `RunAsync` yönteminde `ServiceEventSource.Current.ServiceMessage` çağrısı, uygulama kodundan özel bir ETW izleme örneği gösterir.
2. **ServiceEventSource.cs** dosyasında, Performans nedenlerinden dolayı yüksek frekanslı olaylar için kullanılması gereken `ServiceEventSource.ServiceMessage` yöntemi için bir aşırı yükleme bulacaksınız.

**Aktör şablonlarından** oluşturulan projeler için (durum bilgisiz veya Stateful):

1. *ProjectName* 'In Visual Studio projeniz için seçtiğiniz ad olduğu **"ProjectName". cs** dosyasını açın.  
2. *Doworkasync* yönteminde kod `ActorEventSource.Current.ActorMessage(this, "Doing Work");` bulun.  Bu, uygulama kodundan yazılmış özel bir ETW izleme örneğidir.  
3. Dosya **ActorEventSource.cs**' de, Performans nedenlerinden dolayı yüksek frekanslı olaylar için kullanılması gereken `ActorEventSource.ActorMessage` yöntemi için bir aşırı yükleme bulacaksınız.

Hizmet kodunuza özel ETW izleme ekledikten sonra, tanılama olayları görüntüleyicisinde olaylarınızı görmek için uygulamayı yeniden oluşturabilir, dağıtabilir ve çalıştırabilirsiniz. **F5**ile uygulamada hata ayıklaması yaparsanız, tanılama olayları Görüntüleyicisi otomatik olarak açılır.

## <a name="next-steps"></a>Sonraki adımlar
Yerel Tanılama için yukarıdaki uygulamanıza eklediğiniz izleme kodu, uygulamanızı bir Azure kümesinde çalıştırırken bu olayları görüntülemek için kullanabileceğiniz araçlarla birlikte çalışacaktır. Araçların farklı seçeneklerini tartışan bu makalelere göz atın ve bunları nasıl ayarlayabileceğini açıklayabiliriz.

* [Azure Tanılama ile günlükleri toplama](service-fabric-diagnostics-how-to-setup-wad.md)
* [EventFlow kullanarak olay toplama ve toplama](service-fabric-diagnostics-event-aggregation-eventflow.md)

