---
title: Yerel önbellek
description: Azure Uygulama Hizmeti'nde yerel önbelleğin nasıl çalıştığını ve uygulamanızın yerel önbelleğinin durumunu nasıl etkinleştireceğinizi, yeniden boyutlandıracağını ve sorgulayacağınıöğrenin.
tags: optional
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.topic: article
ms.date: 03/04/2016
ms.custom: seodec18
ms.openlocfilehash: 1945730acaddb0c1c7ee1b28eeb926635efad643
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78227891"
---
# <a name="azure-app-service-local-cache-overview"></a>Azure App Service Yerel Önbellek'e genel bakış

> [!NOTE]
> Yerel önbellek, Windows [Kapsayıcıları](app-service-web-get-started-windows-container.md) veya Linux'taki Uygulama Hizmeti gibi Işlev uygulamalarında veya kapsayıcı Uygulama Hizmeti [uygulamalarında](containers/app-service-linux-intro.md)desteklenmez.


Azure Uygulama Hizmeti içeriği Azure Depolama'da depolanır ve içerik paylaşımı olarak dayanıklı bir şekilde ortaya çıkar. Bu tasarım çeşitli uygulamalarla çalışmak üzere tasarlanmıştır ve aşağıdaki özelliklere sahiptir:  

* İçerik, uygulamanın birden çok sanal makine (VM) örneğinde paylaşılır.
* İçerik dayanıklıdır ve çalışan uygulamalarla değiştirilebilir.
* Günlük dosyaları ve tanılama veri dosyaları aynı paylaşılan içerik klasörü altında kullanılabilir.
* Yeni içerik yayımlama doğrudan içerik klasörünü güncelleştirir. Aynı içeriği SCM web sitesi ve çalışan uygulama aracılığıyla hemen görüntüleyebilirsiniz (genellikle ASP.NET gibi bazı teknolojiler, en son içeriği almak için bazı dosya değişikliklerinde bir uygulamayı yeniden başlatmayı başlatır).

Birçok uygulama bu özelliklerden birini veya tümlerini kullanırken, bazı uygulamaların yüksek kullanılabilirlik ile çalıştırabilecekleri yüksek performanslı, salt okunur içerik deposuna ihtiyacı vardır. Bu uygulamalar, belirli bir yerel önbelleğin VM örneğinden yararlanabilir.

Azure App Service Local Cache özelliği, içeriğinizin web rol görünümünü sağlar. Bu içerik, depolama alanı içeriğinizin yerinde eş zamanlı olarak oluşturulan bir yazma önbelleğidir. Önbellek hazır olduğunda, site önbelleğe alınmış içeriğe karşı çalışacak şekilde değiştirilir. Yerel Önbellekte çalışan uygulamaların aşağıdaki avantajları vardır:

* Azure Depolama'daki içeriğe erişirken oluşan gecikmelerden etkilenmezler.
* İçerik paylaşımına hizmet veren sunucularda meydana gelen Azure Depolama ile planlanan yükseltmelere veya planlanmamış kesinti sürelerine ve diğer aksaklıklara karşı bağışıktırlar.
* Depolama payı değişiklikleri nedeniyle daha az uygulama yeniden başlatmaları var.

## <a name="how-the-local-cache-changes-the-behavior-of-app-service"></a>Yerel önbelleğin Uygulama Hizmeti davranışını nasıl değiştirdiği
* _D:\ev,_ uygulama başlatıldığında VM örneğinde oluşturulan yerel önbelleğe işaret ediyor. _D:\yerel_ geçici VM özel depolama işaret etmeye devam ediyor.
* Yerel önbellek, paylaşılan içerik deposunun _/site_ ve _/site uzantıları_ klasörlerinin sırasıyla _D:\home\site_ ve _D:\home\site uzantılarında_bir defalık bir kopyasını içerir. Uygulama başlatıldığında dosyalar yerel önbelleğe kopyalanır. Her uygulama için iki klasörün boyutu varsayılan olarak 300 MB ile sınırlıdır, ancak bunu 2 GB'a kadar artırabilirsiniz. Kopyalanan dosyalar yerel önbelleğin boyutunu aşarsa, Uygulama Hizmeti yerel önbelleği sessizce yok sayar ve uzak dosya paylaşımından okunur.
* Yerel önbellek okuma-yazma dır. Ancak, uygulama sanal makineleri hareket ettirdiğinde veya yeniden başlatıldığında herhangi bir değişiklik atılır. İçerik deposunda görev açısından kritik verileri depolayan uygulamalar için yerel önbelleği kullanmayın.
* _D:\home\LogFiles_ ve _D:\home\Data_ günlük dosyaları ve uygulama verileri içerir. İki alt klasör VM örneğinde yerel olarak depolanır ve düzenli olarak paylaşılan içerik deposuna kopyalanır. Uygulamalar, bu klasörlere yazarak günlük dosyalarını ve verilerini devam ettir. Ancak, paylaşılan içerik deposuna kopya en iyi çabadır, bu nedenle günlük dosyalarının ve verilerinin bir VM örneğinin ani bir şekilde çökmesi nedeniyle kaybolması mümkündür.
* [Günlük akışı](troubleshoot-diagnostic-logs.md#stream-logs) en iyi çaba kopya etkilenir. Akışlı günlüklerde bir dakikaya kadar gecikme gözlemleyebilirsiniz.
* Paylaşılan içerik deposunda, yerel önbelleği kullanan uygulamalar için _LogFiles_ ve _Data_ klasörlerinin klasör yapısında bir değişiklik vardır. Artık içinde "benzersiz tanımlayıcı" + zaman damgasının adlandırma desenini izleyen alt klasörler vardır. Alt klasörlerin her biri, uygulamanın çalıştığı veya çalıştığı bir VM örneğine karşılık gelir.
* _D:\home'daki_ diğer klasörler yerel önbellekte kalır ve paylaşılan içerik deposuna kopyalanmaz.
* Desteklenen herhangi bir yöntemle uygulama dağıtımı doğrudan dayanıklı paylaşılan içerik deposuna yayınlanır. Yerel önbellekteki _D:\home\site_ ve _D:\home\site uzantıları_ klasörlerini yenilemek için uygulamanın yeniden başlatılması gerekir. Yaşam döngüsünü sorunsuz hale getirmek için, bu makaledeki bilgileri daha sonra görün.
* SCM sitesinin varsayılan içerik görünümü paylaşılan içerik deposuolmaya devam eder.

## <a name="enable-local-cache-in-app-service"></a>Uygulama Hizmetinde Yerel Önbelleği Etkinleştir
Ayrılmış uygulama ayarlarının bir birleşimini kullanarak Yerel Önbellek'i yapılandırırsınız. Aşağıdaki yöntemleri kullanarak bu uygulama ayarlarını yapılandırabilirsiniz:

* [Azure portalında](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Azure portalını kullanarak Yerel Önbelleği yapılandırma
<a name="Configure-Local-Cache-Portal"></a>

Bu uygulama ayarını kullanarak Web uygulaması başına Yerel Önbellek'i etkinleştirin:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure portalı uygulama ayarları: Yerel Önbellek](media/app-service-local-cache-overview/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak Yerel Önbelleği yapılandırma
<a name="Configure-Local-Cache-ARM"></a>

```json

...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],

    "properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-the-size-setting-in-local-cache"></a>Yerel Önbellek'te boyut ayarını değiştirme
Varsayılan olarak, yerel önbellek boyutu **300**MB'dır. Buna, içerik deposundan kopyalanan /site ve /site uzantıları klasörlerinin yanı sıra yerel olarak oluşturulan günlükleri ve veri klasörleri de dahildir. Bu sınırı artırmak için uygulama `WEBSITE_LOCAL_CACHE_SIZEINMB`ayarını kullanın. Boyutu uygulama başına **2 GB'a** (2000 MB) kadar artırabilirsiniz.

## <a name="best-practices-for-using-app-service-local-cache"></a>Uygulama Hizmeti Yerel Önbelleği kullanmak için en iyi uygulamalar
[Hazırlama Ortamları](../app-service/deploy-staging-slots.md) özelliğiyle birlikte Yerel Önbellek kullanmanızı öneririz.

* **Üretim** *sticky* yuvanıza değeri `WEBSITE_LOCAL_CACHE_OPTION` `Always` olan yapışkan uygulama ayarını ekleyin. `WEBSITE_LOCAL_CACHE_SIZEINMB`Kullanıyorsanız, üretim yuvanıza yapışkan bir ayar olarak da ekleyin.
* Bir **Evreleme** yuvası oluşturun ve Evreleme yuvanıza yayınlayın. Üretim yuvası için Yerel Önbellek'in avantajlarından yararlanırsanız, evreleme için sorunsuz bir yapı dağıtma-test yaşam döngüsü sağlamak için genellikle hazırlama yuvasını Yerel Önbellek'i kullanacak şekilde ayarlamazsınız.
* Sitenizi Evreleme yuvanıza karşı test edin.  
* Hazır olduğunuzda, Evreleme ve Üretim yuvalarınız arasında bir [takas işlemi](../app-service/deploy-staging-slots.md#Swap) düzenleyin.  
* Yapışkan ayarlar ad ve yapışkan bir yuva içerir. Böylece Evreleme yuvası Üretim'e dönüştürüldüğünde, Yerel Önbellek uygulaması ayarlarını devralır. Yeni değiştirilen Üretim yuvası birkaç dakika sonra yerel önbelleğe karşı çalışacak ve takas tan sonra yuva ısınmasının bir parçası olarak ısıtılır. Bu nedenle, yuva değişimi tamamlandığında, Üretim yuvanız yerel önbelleğe göre çalışır.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

### <a name="how-can-i-tell-if-local-cache-applies-to-my-app"></a>Yerel Önbellek'in uygulamama uygulanıp uygulanmadığını nasıl anlayabilirim?
Uygulamanızın yüksek performanslı ve güvenilir bir içerik deposuna ihtiyacı varsa, çalışma zamanında kritik verileri yazmak için içerik deposunu kullanmıyorsa ve toplam boyutu 2 GB'dan azsa, yanıt "evet"! /site nizin ve /site uzantıları klasörlerinizin toplam boyutunu elde etmek için site uzantısını kullanabilirsiniz "Azure Web Apps Disk Kullanımı."

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Sitemin Yerel Önbellek'i kullanmaya geçiş olup olmadığını nasıl anlayabilirim?
Hazırlama Ortamları ile Yerel Önbellek özelliğini kullanıyorsanız, Yerel Önbellek ısıtılana kadar takas işlemi tamamlanmaz. Sitenizin Yerel Önbellekle karşı olup olmadığını kontrol etmek için, alt işlem ortamı değişkenini `WEBSITE_LOCALCACHE_READY`denetleyebilirsiniz. Birden çok durumda alt işlem ortamı değişkenine erişmek için [alt işlem ortamı değişken](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) sayfasındaki yönergeleri kullanın.  

### <a name="i-just-published-new-changes-but-my-app-does-not-seem-to-have-them-why"></a>Ben sadece yeni değişiklikler yayınladı, ama benim uygulama onları yok gibi görünüyor. Neden?
Uygulamanız Yerel Önbellek kullanıyorsa, en son değişiklikleri almak için sitenizi yeniden başlatmanız gerekir. Üretim sitesinde değişiklik yayınlamak istemiyor musunuz? Önceki en iyi uygulamalar bölümündeki yuva seçeneklerine bakın.

### <a name="where-are-my-logs"></a>Kütüklerim nerede?
Yerel Önbellek ile günlükleriniz ve veri klasörleriniz biraz farklı görünür. Ancak, alt klasörlerinizin yapısı, alt klasörlerin "benzersiz VM tanımlayıcısı" + zaman damgası biçiminde bir alt klasörün altına yer leşmesi dışında aynı kalır.

### <a name="i-have-local-cache-enabled-but-my--app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Yerel Önbellek etkinleştirilmiş, ancak uygulamam yine de yeniden başlatılıyor. Bunun nedeni nedir? Yerel Önbellek'in sık sık uygulama yeniden başlatmaya yardımcı olduğunu düşündüm.
Yerel Önbellek, depolama yla ilgili uygulamanın yeniden başlatılmasını önlemeye yardımcı olur. Ancak, uygulamanız VM'nin planlanan altyapı yükseltmeleri sırasında yeniden başlatılabilir. Yerel Önbellek etkinleştirilmiş ile karşılaştığınız genel uygulama yeniden başlatılır daha az olmalıdır.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-to-the-faster-local-drive"></a>Yerel Önbellek, dizinlerin daha hızlı yerel sürücüye kopyalanmasını dışlıyor mu?
Depolama içeriğini kopyalayan adımın bir parçası olarak, depo adlı tüm klasör hariç tutulur. Bu, site içeriğinizin uygulamanın günlük çalışmasında gerek meyebilecek bir kaynak denetim deposu içerebileceği senaryolara yardımcı olur. 
