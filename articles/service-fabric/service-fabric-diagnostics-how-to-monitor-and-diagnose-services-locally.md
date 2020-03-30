---
title: Windows'da Azure Hizmet Kumaşı uygulamalarını hata ayıklama
description: Yerel bir geliştirme makinesinde Microsoft Azure Hizmet Dokusu'u kullanarak yazılan hizmetlerinizi nasıl izleyeceğinizi ve tanılamayı öğrenin.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 8435bb82afddd0070679768bb8d22ad9290f2279
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258518"
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Yerel makine geliştirme kurulumundaki hizmetleri izleme ve tanılama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

İzleme, algılama, tanılama ve sorun giderme, hizmetlerin kullanıcı deneyiminde en az kesintiye yol açmayla devam etmesine olanak sağlar. Gerçek bir dağıtım lı üretim ortamında izleme ve tanılama kritik öneme sahip olsa da, verimlilik, gerçek bir kuruluma geçdiğinizde bunların çalışmasını sağlamak için hizmetlerin geliştirilmesi sırasında benzer bir modelin benimsenmesi gerekir. Service Fabric, hizmet geliştiricilerin hem tek makineli yerel geliştirme kurulumları hem de gerçek dünya üretim küme kurulumlarında sorunsuz bir şekilde çalışabilen tanılamaları uygulamalarını kolaylaştırır.

## <a name="event-tracing-for-windows"></a>Windows için Olay İzleme
[Windows için Olay İzleme](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW), Hizmet Kumaşı'ndaki iletileri izlemek için önerilen teknolojidir. ETW kullanmanın bazı avantajları şunlardır:

* **ETW hızlıdır.** Kod yürütme süreleri üzerinde en az etkiye sahip bir izleme teknolojisi olarak inşa edilmiştir.
* **ETW izleme, yerel geliştirme ortamlarında ve gerçek dünya küme kurulumlarında sorunsuz bir şekilde çalışır.** Bu, kodunuzu gerçek bir kümeye dağıtmaya hazır olduğunuzda izleme kodunuzu yeniden yazmak zorunda olmadığınız anlamına gelir.
* **Service Fabric sistem kodu da dahili izleme için ETW kullanır.** Bu, Servis Kumaşı sistem izleri ile ara sıra yapılan uygulama izlerinizi görüntülemenizi sağlar. Ayrıca, uygulama kodunuz la temel sistemdeki olaylar arasındaki dizileri ve ilişkileri daha kolay anlamanıza yardımcı olur.
* **ETW olaylarını görüntülemek için Service Fabric Visual Studio araçlarında yerleşik destek vardır.** ETW olayları, Visual Studio Service Fabric ile doğru şekilde yapılandırıldıktan sonra Visual Studio'nun Tanısal Etkinlikler görünümünde görünür. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Visual Studio'da Servis Kumaşı sistem etkinliklerini görüntüleyin
Service Fabric, uygulama geliştiricilerin platformda neler olduğunu anlamalarına yardımcı olmak için ETW etkinlikleri yayar. Bunu zaten yapmadıysanız, devam edin ve [Visual Studio'da ilk uygulamanızı oluşturma](service-fabric-tutorial-create-dotnet-app.md)adımlarını izleyin. Bu bilgiler, izleme iletilerini gösteren Tanılama Olayları Görüntüleyicisi ile bir uygulamayı çalışır hale getiresiniz yardımcı olur.

1. Tanılama olayları penceresi otomatik olarak görünmüyorsa, Visual Studio'da **Görünüm** sekmesine gidin, **Diğer Windows'u** seçin ve ardından **Olayları Tanılayın Görüntüleyici.'**
2. Her olay, olayın geldiği düğümü, uygulamayı ve hizmeti size söyleyen standart meta veri bilgilerine sahiptir. Ayrıca, olaylar penceresinin üst kısmındaki **Filtre olayları** kutusunu kullanarak olaylar listesini filtreleyebilirsiniz. Örneğin, **Düğüm Adı** veya Hizmet **Adı filtreleyebilirsiniz.** Ayrıca etkinlik ayrıntılarına baktığınızda, olaylar penceresinin üst kısmındaki **Duraklat** düğmesini kullanarak duraklatabilir ve daha sonra herhangi bir olay kaybı olmadan devam edebilirsiniz.
   
   ![Visual Studio Tanılama Etkinlikleri Görüntüleyici](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-to-the-application-code"></a>Uygulama koduna kendi özel izlerinizi ekleyin
Service Fabric Visual Studio proje şablonları örnek kod içerir. Kod, Service Fabric'ten gelen sistem izlerinin yanında Visual Studio ETW görüntüleyicide görünen özel uygulama kodu ETW izlemelerinin nasıl ekleyeceğini gösterir. Bu yöntemin avantajı, meta verilerin izlemelere otomatik olarak eklenmesi ve Visual Studio Diagnostic Events Viewer'ın bunları görüntülemek üzere zaten yapılandırılmış olmasıdır.

**Hizmet şablonlarından** (durum belirteçsiz veya durum disi) oluşturulan projeler için, uygulamayı aramanız `RunAsync` gerekir:

1. `RunAsync` Yöntemde yapılan `ServiceEventSource.Current.ServiceMessage` arama, uygulama kodundan gelen özel bir ETW izleme örneğini gösterir.
2. **ServiceEventSource.cs** dosyasında, performans nedenleriyle yüksek `ServiceEventSource.ServiceMessage` frekanslı olaylar için kullanılması gereken yöntem için aşırı yükleme bulacaksınız.

**Aktör şablonlarından** (durum suz veya durumlu) oluşturulan projeler için:

1. Visual Studio projeniz için seçtiğiniz *adın ProjectName* olduğu **"ProjectName".cs** dosyasını açın.  
2. `ActorEventSource.Current.ActorMessage(this, "Doing Work");` *DoWorkAsync* yönteminde kodu bulun.  Bu, uygulama kodundan yazılmış özel bir ETW izleme örneğidir.  
3. Dosya **ActorEventSource.cs,** performans nedenleriyle yüksek `ActorEventSource.ActorMessage` frekanslı olaylar için kullanılması gereken yöntem için aşırı yükleme bulacaksınız.

Hizmet kodunuza özel ETW izleme ekledikten sonra, Tanılama Olayları Görüntüleyicisi'nde olay(lar) görmek için uygulamayı oluşturabilir, dağıtabilir ve yeniden çalıştırabilirsiniz. F5 ile uygulamayı **F5**hata ayıklama, Tanılama Olayları Görüntüleyici otomatik olarak açılır.

## <a name="next-steps"></a>Sonraki adımlar
Yerel tanılama için yukarıdaki uygulamanıza eklediğiniz aynı izleme kodu, uygulamanızı bir Azure kümesinde çalıştırırken bu olayları görüntülemek için kullanabileceğiniz araçlarla çalışır. Araçlar için farklı seçenekleri tartışan ve bunları nasıl ayarlaabileceğinizi açıklayan bu makalelere göz atın.

* [Azure Tanılama ile günlükleri nasıl toplanır?](service-fabric-diagnostics-how-to-setup-wad.md)
* [EventFlow'u kullanarak olay toplama ve toplama](service-fabric-diagnostics-event-aggregation-eventflow.md)

