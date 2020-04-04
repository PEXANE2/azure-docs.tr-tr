---
title: Bulut Hizmeti yaşam döngüsü etkinliklerini ele alın | Microsoft Dokümanlar
description: Bulut Hizmeti rolünün yaşam döngüsü yöntemlerinin .NET'te nasıl kullanılabileceğini öğrenin
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/18/2017
ms.author: tagore
ms.openlocfilehash: a6030cbb756525137497834ac911835033858401
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652086"
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>.NET'te Bir Web veya İşçi rolünün Yaşam Döngüsünü özelleştirin
Bir alt rol oluşturduğunuzda, yaşam döngüsü olaylarına yanıt vermenize izin veren yöntemleri geçersiz kılmanız için gereken yöntemleri sağlayan [RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) sınıfını genişletirsiniz. Web rolleri için bu sınıf isteğe bağlıdır, bu nedenle yaşam döngüsü olaylarına yanıt vermek için kullanmanız gerekir.

## <a name="extend-the-roleentrypoint-class"></a>RoleEntryPoint sınıfını genişletme
[RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) sınıfı, bir web veya çalışan rolünü **başlattığında,** **çalıştırdığında**veya **durdurduğunda** Azure tarafından çağrılan yöntemleri içerir. Rol başlatma, rol kapatma dizileri veya rolün yürütme iş parçacığı yönetmek için isteğe bağlı olarak bu yöntemleri geçersiz kılabilirsiniz. 

**RoleEntryPoint'i**genişletirken, yöntemlerin aşağıdaki davranışlarına dikkat etmeniz gerekir:

* [OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) yöntemi boolean değerini döndürür, bu nedenle bu yöntemden **yanlış** dönmek mümkündür.
  
   Kodunuz **yanlış**döndürürse, rol işlemi, herhangi bir kapatma sırası çalıştırmadan aniden sonlandırılır. Genel olarak, **OnStart** yönteminden **false** dönmekten kaçınmalısınız.
* **RoleEntryPoint** yönteminin aşırı yüklenmesi nde yakalanmamış herhangi bir özel durum işlenmemiş bir özel durum olarak kabul edilir.
  
   Yaşam döngüsü yöntemlerinden birinde bir özel durum oluşursa, Azure [İşlenmemiş Özel Durum](/dotnet/api/system.appdomain.unhandledexception) olayını yükseltir ve işlem sonlandırılır. Rolünüz çevrimdışı hale alındıktan sonra Azure tarafından yeniden başlatılır. İşlenmemiş bir özel durum oluştuğunda, [Durdurma](/previous-versions/azure/reference/ee758136(v=azure.100)) olayı yükseltilmez ve **OnStop** yöntemi çağrılmaz.

Rolünüz başlamazsa veya başlatma, meşgul ve durdurma durumları arasında geri dönüşüm başlatıyorsa, kodunuz, rol her yeniden başlatılınca yaşam döngüsü olaylarından biri içinde işlenmemiş bir özel durum atıyor olabilir. Bu durumda, özel durumun nedenini belirlemek ve uygun şekilde işlemek için [İşlenmemiş Özel Durum](/dotnet/api/system.appdomain.unhandledexception) olayını kullanın. Rolünüz, rolün yeniden başlatılmasına neden olan [Çalıştır](/previous-versions/azure/reference/ee772746(v=azure.100)) yönteminden de dönüyor olabilir. Dağıtım durumları hakkında daha fazla bilgi için, [Rolleri Geri Dönüştürmeye Neden Olan Sık Karşılaşılan Sorunlar'a](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)bakın.

> [!NOTE]
> Uygulamanızı geliştirmek **için Microsoft Visual Studio için Azure Araçlarını** kullanıyorsanız, rol projesi şablonları *WebRole.cs* ve *WorkerRole.cs* dosyalarında **RoleEntryPoint** sınıfını sizin için otomatik olarak genişletir.
> 
> 

## <a name="onstart-method"></a>OnStart yöntemi
Rol örneğiniz Azure tarafından çevrimiçi duruma getirildiğinde **OnStart** yöntemi çağrılır. OnStart kodu yürütülse de, rol örneği **Meşgul** olarak işaretlenir ve yük bakiyesi tarafından dış trafik yönlendirilmez. Olay işleyicileri uygulama ve [Azure Tanılama'yı](cloud-services-how-to-monitor.md)başlatma gibi başlatma çalışması gerçekleştirmek için bu yöntemi geçersiz kılabilirsiniz.

**OnStart** **doğru**döndürürse, örnek başarıyla başlatılır ve Azure **RoleEntryPoint.Run** yöntemini çağırır. **OnStart** yanlış döndürürse, rol planlanan kapatma sıralarını yürütmeden hemen sona erer. **false**

Aşağıdaki kod örneği, **OnStart** yöntemini nasıl geçersiz kılınan gösteriş gösterir. Bu yöntem, rol örneği başladığında ve günlüğe kaydetme verilerinin bir depolama hesabına aktarılmasını ayarladığında bir tanılama monitörünü yapılandırır ve başlatır:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop yöntemi
**OnStop** yöntemi, bir rol örneği Azure tarafından çevrimdışı hale alındıktan sonra ve işlem çıkmadan önce çağrılır. Rol örneğinizin temiz bir şekilde kapanması için gereken kodu çağırmak için bu yöntemi geçersiz kılabilirsiniz.

> [!IMPORTANT]
> **OnStop** yönteminde çalışan kodun, kullanıcı tarafından başlatılan kapatma dışındaki nedenlerden ötürü çağrıldığında tamamlanması sınırlı bir süre vardır. Bu süre geçtikten sonra işlem sonlandırılır, bu nedenle **OnStop** yöntemindeki kodun hızlı bir şekilde çalıştırıladığından veya tamamlanmak üzere çalışmayan tolere edebileceğinden emin olmalısınız. **Durdurma** olayı yükseltildikten sonra **OnStop** yöntemi çağrılır.
> 
> 

## <a name="run-method"></a>Çalıştırma yöntemi
Rol örneğiniz için uzun soluklu bir iş parçacığı uygulamak için **Çalıştır** yöntemini geçersiz kılabilirsiniz.

**Çalıştır** yöntemini geçersiz kılmak gerekli değildir; varsayılan uygulama sonsuza kadar uyuyan bir iş parçacığı başlatır. **Çalıştır** yöntemini geçersiz kılarsanız, kodunuz süresiz olarak engellemelidir. **Çalıştır** yöntemi geri dönerse, rol otomatik olarak geri dönüştürülür; başka bir deyişle, Azure **Durdurma** olayını yükseltir ve kapatma dizilerinizin rol çevrimdışı olmadan önce yürütülebileceği **onstop** yöntemini çağırır.

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Bir web rolü için ASP.NET yaşam döngüsü yöntemlerini uygulama
**RoleEntryPoint** sınıfı tarafından sağlananlara ek olarak, bir web rolü için başlatma ve kapatma dizilerini yönetmek için ASP.NET yaşam döngüsü yöntemlerini kullanabilirsiniz. Bu, varolan bir ASP.NET uygulamasını Azure'a taşımanız durumunda uyumluluk amacıyla yararlı olabilir. ASP.NET yaşam döngüsü yöntemleri **RoleEntryPoint** yöntemleri içinden çağrılır. **\_Application Start** yöntemi **RoleEntryPoint.OnStart** yöntemi bittikten sonra çağrılır. **\_Application End** yöntemi **RoleEntryPoint.OnStop** yöntemi çağrılmadan önce çağrılır.

## <a name="next-steps"></a>Sonraki adımlar
[Bulut hizmet paketi oluşturmayı](cloud-services-model-and-package.md)öğrenin.




