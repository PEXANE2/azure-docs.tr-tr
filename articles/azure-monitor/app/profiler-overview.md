---
title: Application Insights Profiler ile Azure'daki profil üretim uygulamaları
description: Düşük ayak izi profilleyicisi ile web sunucu kodunuzdaki sıcak yolu belirleyin.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: ce952bd248640d03fcff43284707614577df8469
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671656"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Application Insights ile Azure'da profil üretim uygulamaları
## <a name="enable-application-insights-profiler-for-your-application"></a>Uygulamanız için Uygulama Öngörüleri Profilleyicisini Etkinleştirin

Azure Application Insights Profiler, Azure'da üretimde çalışan uygulamalar için performans izlemeleri sağlar. Profiler, kullanıcılarınızı olumsuz etkilemeden verileri otomatik olarak ölçekte yakalar. Profiler, belirli bir web isteğini işlerken en uzun süren "sıcak" kod yolunu belirlemenize yardımcı olur. 

Profiler, aşağıdaki Azure hizmetlerinde dağıtılan .NET uygulamalarıyla çalışır. Her hizmet türü için Profiler'ı etkinleştirmek için özel talimatlar aşağıdaki bağlantılarda dır.

* [Azure Uygulama Hizmeti](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler ve sanal makine ölçek setleri](profiler-vm.md?toc=/azure/azure-monitor/toc.json)
* [**ÖNİzleme** ASP.NET Core Azure Linux Web Uygulamaları](profiler-aspnetcore-linux.md?toc=/azure/azure-monitor/toc.json) 

Profiler'ı etkinleştirdim ancak izleme görmüyorsanız, [Sorun Giderme kılavuzumuzu kontrol edin.](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)

## <a name="view-profiler-data"></a>Profiler verilerini görüntüleme

Profiler'ın izleme yükleyebilmek için uygulamanızın istekleri etkin bir şekilde işlemesi gerekir. Bir deneme yapıyorsanız, [Application Insights performans testini](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test)kullanarak web uygulamanızda istekler oluşturabilirsiniz. Profiler'ı yeni etkinleştirdimseniz, kısa bir yükleme testi çalıştırabilirsiniz. Yük testi çalışırken [ **Profiler Ayarları** ](profiler-settings.md)bölmesindeki **Profil Şimdi** düğmesini seçin. Profiler çalışırken, saatte bir kez ve iki dakika boyunca rastgele profiller. Uygulamanız sürekli bir istek akışıyla ilgileniyorsa, Profiler her saat başı izleme yükler.

Uygulamanız biraz trafik aldıktan ve Profiler'ın izleri yüklemek için zamanı olduktan sonra, görüntülemek için izlemeleriniz olmalıdır. Bu işlem 5 ila 10 dakika sürebilir. İzlemeleri görüntülemek için **Performans** bölmesinde **Eylemleri Al'ı**ve ardından **Profiler İzleri** düğmesini seçin.

![Uygulama Öngörüleri Performans bölmesi önizleme Profiler izleri][performance-blade]

İsteği yürütmek için harcanan sürenin kod düzeyindedökümünü görüntülemek için bir örnek seçin.

![Uygulama Öngörüleri izleme gezgini][trace-explorer]

İzleme gezgini aşağıdaki bilgileri görüntüler:

* **Sıcak Yolu Göster**: En büyük yaprak düğümünü veya en azından yakın bir şeyi açar. Çoğu durumda, bu düğüm performans darboğazına yakındır.
* **Etiket**: Fonksiyonun veya olayın adı. Ağaç, SQL ve HTTP olayları gibi kod ve oluşan olayların bir karışımını görüntüler. Üstteki olay genel istek süresini temsil eder.
* **Geçen**: Operasyonun başlangıcı ile operasyonun sonu arasındaki zaman aralığıdır.
* **Ne zaman**: İşlev in veya olayın diğer işlevlerle ilişkili olarak çalıştığı zaman.

## <a name="how-to-read-performance-data"></a>Performans verileri nasıl okunur?

Microsoft hizmet profilleyicisi, uygulamanızın performansını analiz etmek için örnekleme yöntemleri ve enstrümantasyon un bir birleşimini kullanır. Ayrıntılı toplama devam ederken, servis profilleyicisi her milisaniyede bir makine işlemcisinin talimat işaretçisini örnekler. Her örnek, şu anda yürütmekte olan iş parçacığının tam çağrı yığınını yakalar. Bu iş parçacığı hem yüksek düzeyde ve soyutlama düşük düzeyde, ne yaptığını hakkında ayrıntılı bilgi verir. Hizmet profilleyicisi ayrıca, bağlam değiştirme olayları, Görev Paralel Kitaplığı (TPL) olayları ve iş parçacığı havuzu olayları da dahil olmak üzere etkinlik bağıntısını ve nedenselliği izlemek için diğer olayları da toplar.

Zaman çizelgesi görünümünde görüntülenen çağrı yığını, örnekleme ve enstrümantasyonun sonucudur. Her örnek iş parçacığının tam çağrı yığınını yakaladığından, Microsoft .NET Framework'den ve başvururladığınız diğer çerçevelerden kod içerir.

### <a name="object-allocation-clrjit_new-or-clrjit_newarr1"></a><a id="jitnewobj"></a>Nesne ayırma (clr! JIT\_Yeni veya clr! JIT\_Newarr1)

**Clr! JIT\_Yeni** ve **clr! JIT\_Newarr1,** .NET Framework'de belleği yönetilen bir yığından ayıran yardımcı işlevlerdir. **clr! JIT\_Yeni** bir nesne tahsis edildiğinde çağrılır. **clr! JIT\_Newarr1** bir nesne dizisi tahsis edildiğinde çağrılır. Bu iki fonksiyon genellikle hızlıdır ve nispeten küçük miktarlarda zaman alır. Eğer **clr! JIT\_Yeni** veya **clr! JIT\_Newarr1** zaman çizelgenizde çok zaman alır, kod birçok nesne ayırıyor ve önemli miktarda bellek tüketiyor olabilir.

### <a name="loading-code-clrtheprestub"></a><a id="theprestub"></a>Yükleme kodu (clr! ThePreStub)

**clr! ThePreStub,** .NET Framework'de kodu ilk kez yürütmek üzere hazırlayan bir yardımcı işlevdir. Bu yürütme genellikle içerir, ancak sadece-in-time (JIT) derleme ile sınırlı değildir. Her C# yöntemi için, **clr! ThePreStub** bir işlem sırasında en fazla bir kez çağrılmalıdır.

Eğer **clr! ThePreStub** bir istek için uzun bir zaman alır, istek bu yöntemi yürütmek için ilk biridir. .NET Framework çalışma zamanının ilk yöntemi yükleme süresi önemlidir. Kullanıcılarınızın erişmeden önce kodun bu bölümünü çalıştıran bir ısınma işlemi kullanmayı veya derlemelerinizde Yerel Görüntü Üreteci (ngen.exe) çalıştırmayı düşünebilirsiniz.

### <a name="lock-contention-clrjitutil_moncontention-or-clrjitutil_monenterworker"></a><a id="lockcontention"></a>Kilit çekişme (clr! JITutil\_MonContention veya clr! JITutil\_MonEnterWorker)

**Clr! JITutil\_MonContention** veya **clr! JITutil\_MonEnterWorker,** geçerli iş parçacığının bir kilidin serbest bırakılmasını beklediğini gösterir. Bu metin genellikle bir C# **LOCK** deyimini çalıştırdığınızda, **Monitor.Enter** yöntemini çağırdığınızda veya **MethodImplOptions.Synchronized** özniteliği ile bir yöntem çağırdığınızda görüntülenir. Kilit çekişmesi genellikle iş parçacığı _A_ bir kilit aldığında oluşur ve iş parçacığı _B_ iş parçacığı _A_ yayımlar önce aynı kilidi elde etmeye çalışır.

### <a name="loading-code-cold"></a><a id="ngencold"></a>Yükleme kodu ([COLD])

Yöntem adı **[COLD]** **içeriyorsa, mscorlib.ni gibi![ COLD]System.Reflection.CustomAttribute.IsDefined**, .NET Framework runtime [profil güdümlü optimizasyon](/cpp/build/profile-guided-optimizations)tarafından optimize değil ilk kez kod yürütülmaktadır. Her yöntem için, işlem sırasında en fazla bir kez görüntülenmelidir.

Yükleme kodu bir istek için önemli miktarda zaman alıyorsa, istek yöntemin en iyi duruma getirilmemiş bölümünü çalıştıran ilk istektir. Kullanıcılarınız kodun bu bölümüne erişmeden önce kodun bu bölümünü yürüten bir ısınma işlemi kullanmayı düşünün.

### <a name="send-http-request"></a><a id="httpclientsend"></a>HTTP isteği gönder

**HttpClient.Send** gibi yöntemler, kodun bir HTTP isteğinin tamamlanmasını beklediğini gösterir.

### <a name="database-operation"></a><a id="sqlcommand"></a>Veritabanı işlemi

**SqlCommand.Execute** gibi yöntemler, kodun bir veritabanı işleminin tamamlanmasını beklediğini gösterir.

### <a name="waiting-await_time"></a><a id="await"></a>Bekleme (BEKLEME\_SÜRESİ)

**AWAIT\_TIME,** kodun tamamlanması için başka bir görev beklediğini gösterir. Bu gecikme genellikle C# **AWAIT** deyimi ile olur. Kod C# **AWAIT**yaptığında, iş parçacığı garar ve denetimi iş parçacığı havuzuna döndürür ve **AWAIT'in** tamamlanmasını bekleyen engellenen iş parçacığı yoktur. Ancak, mantıksal olarak, **AWAIT'i** yapan iş parçacığı "engellendi" ve işlemin tamamlanmasını bekliyor. **AWAIT\_TIME** deyimi, görevin tamamlanmasını bekleyen engellenen zamanı gösterir.

### <a name="blocked-time"></a><a id="block"></a>Engellenen süre

**BLOCKED_TIME,** kodun başka bir kaynağın kullanılabilir olmasını beklediğini gösterir. Örneğin, bir iş parçacığının kullanılabilir olması veya bir isteğin tamamlanması için bir eşitleme nesnesi bekliyor olabilir.

### <a name="unmanaged-async"></a>Yönetilmeyen Async

.NET çerçevesi ETW olayları yayır ve iş parçacıkları arasında etkinlik kimliklerini geçirir, böylece async çağrıları iş parçacıkları arasında izlenebilir. Yönetilmeyen kod (yerel kod) ve bazı eski asynchronous kodu stilleri bu olayları ve etkinlik kimliklerini eksik, bu nedenle profil oluşturucu iş parçacığının hangi iş parçacığı ve hangi işlevlerin çalıştığını söyleyemez. Bu, çağrı yığınında 'Yönetilmeyen Async' olarak etiketlenir. ETW dosyasını indirirseniz, neler olduğuna daha fazla bilgi edinmek için [PerfView'i](https://github.com/Microsoft/perfview/blob/master/documentation/Downloading.md) kullanabilirsiniz.

### <a name="cpu-time"></a><a id="cpu"></a>CPU süresi

CPU yönergeleri yürütmekle meşgul.

### <a name="disk-time"></a><a id="disk"></a>Disk süresi

Uygulama disk işlemleri gerçekleştirmektedir.

### <a name="network-time"></a><a id="network"></a>Ağ süresi

Uygulama ağ işlemleri gerçekleştirmektedir.

### <a name="when-column"></a><a id="when"></a>Sütun ne zaman

**When** sütunu, düğüm için toplanan KAPSAYıCı örneklerin zaman adedine göre nasıl değiştiğini görselleştirmedir. İsteğin toplam aralığı 32 zaman kovasına bölünür. Bu düğüm için kapsayıcı örnekler bu 32 kova birikintisi. Her kova bir çubuk olarak temsil edilir. Çubuğun yüksekliği ölçeklenmiş bir değeri temsil eder. **CPU_TIME** veya **BLOCKED_TIME**olarak işaretlenmiş veya kaynağı (örneğin, CPU, disk veya iş parçacığı) tüketmekle bariz bir ilişki olan düğümler için çubuk, kova sırasında kaynaklardan birinin tüketimini temsil eder. Bu ölçümler için, birden çok kaynak tüketerek yüzde 100'den büyük bir değer elde etmek mümkündür. Örneğin, bir aralıkta ortalama iki CPU kullanırsanız, yüzde 200 alırsınız.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama süresi beş gündür. Günde en fazla veri 10 GB'dır.

Profiler hizmetini kullanmak için herhangi bir ücret yoktur. Bunu kullanabilmek için web uygulamanızın Azure Uygulama Hizmeti'nin Web Uygulamaları özelliğinin en azından temel katmanında barındırılması gerekir.

## <a name="overhead-and-sampling-algorithm"></a>Genel merkez ve örnekleme algoritması

Profiler, Tracer'ı yakalamak için etkinleştirilen uygulamayı barındıran her sanal makinede saatte iki dakika rastgele çalışır. Profiler çalışırken, sunucuya yüzde 5 ila 15 CPU ek yükü ekler.

## <a name="next-steps"></a>Sonraki adımlar
Azure uygulamanız için Uygulama Öngörüleri Profiloluştur'u etkinleştirin. Ayrıca bkz:
* [Uygulama Hizmetleri](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler ve sanal makine ölçek setleri](profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/profiler-overview/performance-blade-v2-examples.png
[trace-explorer]: ./media/profiler-overview/trace-explorer.png
