---
title: Evreleme ortamları ayarlama
description: Uygulamaları üretim dışı bir yuvaya nasıl dağıtacakve üretime otomatik değiştirme yi öğrenin. Güvenilirliği artırın ve uygulamanın dağıtımlardan uzak kalma süresini ortadan kaldırın.
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.topic: article
ms.date: 03/04/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 21e025088e59c7f65f848b332ecb393b05918261
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300880"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Azure App Service’te hazırlık ortamları ayarlama
<a name="Overview"></a>

Web uygulamanızı, Web uygulamanızı Linux'ta, mobil arka uçta veya API uygulamasını [Azure App Service'e](https://go.microsoft.com/fwlink/?LinkId=529714)dağıttığınızda, **Standart**, **Premium**veya **İzole** App Service planı katmanında çalışırken varsayılan üretim yuvası yerine ayrı bir dağıtım yuvası kullanabilirsiniz. Dağıtım yuvaları, kendi ana bilgisayar adlarına sahip canlı uygulamalardır. Uygulama içeriği ve yapılandırma öğeleri, üretim yuvası da dahil olmak üzere iki dağıtım yuvası arasında değiştirilebilir. 

Uygulamanızı üretim dışı bir yuvaya dağıtmak aşağıdaki avantajlara sahiptir:

* Uygulama değişikliklerini üretim yuvasıyla değiştirmeden önce bir hazırlama dağıtım yuvasında doğrulayabilirsiniz.
* Bir uygulamayı önce bir yuvaya dağıtmak ve onu üretime dönüştürmek, yuvanın tüm örneklerinin üretime geçmeden önce ısıtılmasını sağlar. Bu, uygulamanızı dağıttığınızda ki kapalı kalma süresini ortadan kaldırır. Trafik yeniden yönlendirmesi sorunsuzdur ve takas işlemleri nedeniyle hiçbir istek bırakılır. Ön takas doğrulaması gerekmediğinde [otomatik takas](#Auto-Swap) yapılandırarak tüm iş akışını otomatikleştirebilirsiniz.
* Bir takastan sonra, daha önce sahnelenen uygulamaile yuvası şimdi önceki üretim uygulaması vardır. Üretim yuvasına değiştirilen değişiklikler beklediğiniz gibi değilse, "bilinen son iyi sitenizi" geri almak için aynı takası hemen gerçekleştirebilirsiniz.

Her Uygulama Hizmeti planı katmanı farklı sayıda dağıtım yuvasını destekler. Dağıtım yuvalarını kullanmak için ek ücret alınmaz. Uygulamanızın katman desteklerinin yuva sayısını öğrenmek için [Uygulama Hizmeti sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın. 

Uygulamanızı farklı bir katmana ölçeklendirmek için, hedef kademenin uygulamanızın zaten kullandığı yuva sayısını desteklediğinden emin olun. Örneğin, uygulamanızda beşten fazla yuva varsa, **Standart** katman yalnızca beş dağıtım yuvası nı desteklediği için uygulamayı **Standart** katmana ölçeklendiremezsiniz. 

<a name="Add"></a>

## <a name="add-a-slot"></a>Yuva ekleme
Birden çok dağıtım yuvasını etkinleştirebilmeniz için uygulamanın **Standart,** **Premium**veya **İzole** katmanında çalışıyor olması gerekir.


1. Azure [portalında,](https://portal.azure.com/) **Uygulama Hizmetlerini** arayın ve seçin ve uygulamanızı seçin. 
   
    ![Uygulama Hizmetlerini Ara](./media/web-sites-staged-publishing/search-for-app-services.png)
   

2. Sol bölmede Dağıtım **yuvalarını** > seçin**Yuva Ekle' yi**seçin.
   
    ![Yeni dağıtım yuvası ekleme](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Uygulama zaten **Standart,** **Premium**veya **Yalıtılmış** katmanda değilse, aşamalı yayımlamayı etkinleştirmek için desteklenen katmanları gösteren bir ileti alırsınız. Bu noktada, devam etmeden önce **Yükseltme'yi** seçme ve uygulamanızın **Ölçek** sekmesine gitme seçeneğiniz bulunur.
   > 

3. Yuva **ekle** iletişim kutusunda, yuvaya bir ad verin ve başka bir dağıtım yuvasından bir uygulama yapılandırmasını klonlayıp klonlamayan seçeneğini belirleyin. Devam etmek için **Ekle'yi** seçin.
   
    ![Yapılandırma kaynağı](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    Varolan herhangi bir yuvadan yapılandırmayı klonlayabilirsiniz. Klonlanabilen ayarlar arasında uygulama ayarları, bağlantı dizeleri, dil çerçeve sürümleri, web soketleri, HTTP sürümü ve platform bitness yer almaktadır.

4. Yuva eklendikten sonra iletişim kutusunu kapatmak için **Kapat'ı** seçin. Yeni yuva artık **Dağıtım yuvaları** sayfasında gösterilir. Varsayılan olarak, tüm müşteri trafiği üretim yuvasına yönlendirilerek yeni yuva için **Trafik % 0** olarak ayarlanır.

5. Yuvanın kaynak sayfasını açmak için yeni dağıtım yuvasını seçin.
   
    ![Dağıtım yuvası başlığı](./media/web-sites-staged-publishing/StagingTitle.png)

    Evreleme yuvası, diğer App Service uygulamaları gibi bir yönetim sayfasına sahiptir. Yuvanın yapılandırmasını değiştirebilirsiniz. Yuvanın adı, dağıtım yuvasını görüntülediğinizi size hatırlatmak için sayfanın üst kısmında gösterilir.

6. Yuvanın kaynak sayfasındaki uygulama URL'sini seçin. Dağıtım yuvasının kendi ana bilgisayar adı vardır ve aynı zamanda canlı bir uygulamadır. Dağıtım yuvasına genel erişimi sınırlamak için [Azure App Service IP kısıtlamalarına](app-service-ip-restrictions.md)bakın.

Ayarları farklı bir yuvadan klonlasanız bile, yeni dağıtım yuvasında içerik yoktur. Örneğin, Git [ile bu yuvaya](app-service-deploy-local-git.md)yayımlayabilirsiniz. Yuvaya farklı bir depo dalından veya farklı bir depodan dağıtabilirsiniz. 

<a name="AboutConfiguration"></a>

## <a name="what-happens-during-a-swap"></a>Takas sırasında ne olur?

### <a name="swap-operation-steps"></a>Takas işlem adımları

İki yuvayı (genellikle bir evreleme yuvasından üretim yuvasına) değiştirdiğinizde, App Service hedef yuvanın kapalı kalma süresi yaşamadığından emin olmak için aşağıdakileri yapar:

1. Hedef yuvadan (örneğin, üretim yuvası) kaynak yuvasının tüm örneklerine aşağıdaki ayarları uygulayın: 
    - [Yuvaya özel](#which-settings-are-swapped) uygulama ayarları ve varsa bağlantı dizeleri.
    - [Etkinse sürekli dağıtım](deploy-continuous-deployment.md) ayarları.
    - [Etkinse Uygulama Hizmeti kimlik doğrulama](overview-authentication-authorization.md) ayarları.
    
    Bu durumlardan herhangi biri yeniden başlatmak için kaynak yuvasındaki tüm örnekleri tetikler. [Önizleme ile takas](#Multi-Phase)sırasında, bu ilk aşamanın sonu işaretler. Takas işlemi duraklatıldı ve kaynak yuvasının hedef yuvasının ayarlarıyla doğru çalıştığını doğrulayabilirsiniz.

1. Kaynak yuvasındaki her örneğin yeniden başlatılmasını tamamlamasını bekleyin. Herhangi bir örnek yeniden başlatılamazsa, takas işlemi kaynak yuvasındaki tüm değişiklikleri geri değiştirir ve işlemi durdurur.

1. [Yerel önbellek](overview-local-cache.md) etkinse, kaynak yuvanın her örneğinde uygulama köküne ("/") bir HTTP isteği yaparak yerel önbellek başlatmayı tetikler. Her örnek herhangi bir HTTP yanıtını döndürene kadar bekleyin. Yerel önbellek başlatma, her örnekte başka bir yeniden başlatmaya neden olur.

1. [Otomatik değiştirme](#Auto-Swap) özel [ısınma](#Warm-up)ile etkinleştirilirse, kaynak yuvasının her örneğinde uygulama köküne ("/") bir HTTP isteği yaparak [Uygulama Başlatma'yı](https://docs.microsoft.com/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) tetikler.

    `applicationInitialization` Belirtilmemişse, her örnekte kaynak yuvanın uygulama köküne bir HTTP isteği tetikler. 
    
    Bir örnek herhangi bir HTTP yanıtını döndürürse, Bu ısınmış olarak kabul edilir.

1. Kaynak yuvasındaki tüm örnekler başarıyla ısıtılırsa, iki yuva için yönlendirme kurallarını değiştirerek iki yuvayı değiştirin. Bu adımdan sonra, hedef yuvası (örneğin, üretim yuvası) daha önce kaynak yuvasında ısınmış bir uygulamaya sahiptir.

1. Artık kaynak yuvasında daha önce hedef yuvasında ön takas uygulaması olduğuna göre, tüm ayarları uygulayarak ve örnekleri yeniden başlatarak aynı işlemi gerçekleştirin.

Takas işleminin herhangi bir noktasında, değiştirilen uygulamaların başlatılmasıyla ilgili tüm çalışmalar kaynak yuvasında gerçekleşir. Kaynak yuvası, takasın nerede başarılı olursa olsun veya nerede başarısız olursa olsun, hazırlanıp ısıtılırken hedef yuvası çevrimiçi kalır. Bir evreleme yuvasını üretim yuvasıyla değiştirmek için, üretim yuvasının her zaman hedef yuvası olduğundan emin olun. Bu şekilde, takas işlemi üretim uygulamanızı etkilemez.

### <a name="which-settings-are-swapped"></a>Hangi ayarlar takas edilir?

[!INCLUDE [app-service-deployment-slots-settings](../../includes/app-service-deployment-slots-settings.md)]

Belirli bir yuvaya yapışmak için bir uygulama ayarını veya bağlantı dizesini yapılandırmak için (değiştirilme), bu yuva için **Yapılandırma** sayfasına gidin. Ayar ekleyin veya edinve ardından **dağıtım yuvası ayarını**seçin. Bu onay kutusunun seçilmesi, App Service'e ayarın değiştirilemez olduğunu bildirir. 

![Yuva ayarı](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>İki yuvayı değiştir 
Uygulamanızın **Dağıtım yuvaları** sayfasında ve **Genel Bakış** sayfasında dağıtım yuvalarını değiştirebilirsiniz. Yuva değişimi yle ilgili teknik ayrıntılar için takas [sırasında neler olduğunu](#AboutConfiguration)görün.

> [!IMPORTANT]
> Bir uygulamayı dağıtım yuvasından üretime dönüştürmeden önce, üretimin hedef yuvanız olduğundan ve kaynak yuvasındaki tüm ayarların tam olarak üretimde olmasını istediğiniz şekilde yapılandırıldığından emin olun.
> 
> 

Dağıtım yuvalarını değiştirmek için:

1. Uygulamanızın **Dağıtım yuvaları** sayfasına gidin ve **Takas'ı**seçin.
   
    ![Takas düğmesi](./media/web-sites-staged-publishing/SwapButtonBar.png)

    **Takas** iletişim kutusu, seçili kaynak ve değiştirilecek hedef yuvalarındaki ayarları gösterir.

2. İstenilen **Kaynak** ve **Hedef** yuvalarını seçin. Genellikle, hedef üretim yuvasıdır. Ayrıca, **Kaynak Değişiklikleri** ve Hedef **Değişiklikleri** sekmelerini seçin ve yapılandırma değişikliklerinin beklendiğini doğrulayın. Bitirdikten sonra **Swap'ı**seçerek yuvaları hemen değiştirebilirsiniz.

    ![Takası tamamlama](./media/web-sites-staged-publishing/SwapImmediately.png)

    Takas gerçekleşmeden önce hedef yuvanızın yeni ayarlarla nasıl çalıştığını görmek için **Swap'ı**seçmeyin, ancak [önizleme ile Swap'daki](#Multi-Phase)yönergeleri izleyin.

3. İşiniz bittiğinde, **Kapat'ı**seçerek iletişim kutusunu kapatın.

Herhangi bir sorununuz varsa, [Sorun Giderme takaslarına](#troubleshoot-swaps)bakın.

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>Önizleme ile takas (çok aşamalı takas)

Hedef yuva olarak üretime geçmeden önce, uygulamanın değiştirilen ayarlarla çalıştığını doğrulayın. Kaynak yuvası da görev kritik uygulamalar için arzu edilir takas tamamlanmadan önce ısıtılır.

Önizlemeyle bir takas yaptığınızda, App Service aynı [takas işlemini](#AboutConfiguration) gerçekleştirir, ancak ilk adımdan sonra duraklar. Daha sonra takas tamamlamadan önce evreleme yuvasında sonucu doğrulayabilirsiniz. 

Takası iptal ederseniz, Uygulama Hizmeti yapılandırma öğelerini kaynak yuvaya yeniden uygular.

Önizleme ile takas için:

1. [Dağıtım yuvalarını değiştir'deki](#Swap) adımları izleyin, ancak **önizlemeyle takas yap'ı**seçin.

    ![Önizleme ile takas](./media/web-sites-staged-publishing/SwapWithPreview.png)

    İletişim kutusu, kaynak yuvasındaki yapılandırmanın faz 1'de nasıl değiştiğini ve faz 2'deki kaynak ve hedef yuvasının nasıl değiştiğini gösterir.

2. Takası başlatmaya hazır olduğunuzda, **Swap'ı Başlat'ı**seçin.

    Aşama 1 bittiğinde, iletişim kutusunda bilgilendirilirsiniz. Kaynak yuvasındaki takasın önizlemesini `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Bekleyen takası tamamlamaya hazır olduğunuzda, Swap **eyleminde** Tam **Takas'ı** seçin ve **Tam Takas'ı**seçin.

    Bekleyen bir takası iptal etmek için, bunun yerine **Takas'ı İptal** et'i seçin.

4. İşiniz bittiğinde, **Kapat'ı**seçerek iletişim kutusunu kapatın.

Herhangi bir sorununuz varsa, [Sorun Giderme takaslarına](#troubleshoot-swaps)bakın.

Çok aşamalı bir değişimi otomatikleştirmek [için PowerShell ile Otomatikleştir'e](#automate-with-powershell)bakın.

<a name="Rollback"></a>

## <a name="roll-back-a-swap"></a>Bir değiş tokuşu geri alma
Bir yuva değiş tokuşundan sonra hedef yuvada (örneğin üretim yuvası) herhangi bir hata oluşursa, aynı iki yuvayı hemen değiştirerek yuvaları ön takas durumlarına geri yükleyin.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Otomatik değiştirme yapılandırması

> [!NOTE]
> Otomatik takas Linux'taki web uygulamalarında desteklenmez.

Otomatik takas, uygulamanın müşterileri için sıfır soğuk başlangıç ve sıfır kapalı kalma süresiyle uygulamanızı sürekli olarak dağıtmak istediğiniz Azure DevOps senaryolarını kolaylaştırır. Otomatik takas bir yuvadan üretime etkinleştirildiğinde, kod değişikliklerinizi bu yuvaya her itmenizde, App Service uygulamayı kaynak yuvasında ısındıktan sonra otomatik olarak [üretime dönüştürür.](#swap-operation-steps)

   > [!NOTE]
   > Üretim yuvası için otomatik takas yapılandırmadan önce, üretim dışı bir hedef yuvasında otomatik takas test etmeyi düşünün.
   > 

Otomatik takas yapılandırmak için:

1. Uygulamanızın kaynak sayfasına gidin.  >  **Configuration****General ayarları**> > istenilen*\<kaynak yuvası *dağıtım **yuvalarını** > seçin.
   
2. **Otomatik takas etkin için,** **On'u**seçin. Ardından Otomatik takas dağıtım **yuvası**için istenilen hedef yuvasını seçin ve komut çubuğunda **Kaydet'i** seçin. 
   
    ![Otomatik takas yapılandırma için seçimler](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Kaynak yuvasına bir kod itme yürütün. Otomatik takas kısa bir süre sonra gerçekleşir ve güncelleştirme hedef yuvanızın URL'sine yansıtılır.

Herhangi bir sorununuz varsa, [Sorun Giderme takaslarına](#troubleshoot-swaps)bakın.

<a name="Warm-up"></a>

## <a name="specify-custom-warm-up"></a>Özel ısınmayı belirtin

Bazı uygulamalar takastan önce özel ısınma eylemleri gerektirebilir. web.config'deki `applicationInitialization` yapılandırma öğesi, özel başlatma eylemlerini belirtmenizi sağlar. [Takas işlemi,](#AboutConfiguration) hedef yuvasıyla değiştirmeden önce bu özel ısınmanın tamamlanmasını bekler. İşte örnek bir web.config parçası.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Öğeyi `applicationInitialization` özelleştirme hakkında daha fazla bilgi için, [en yaygın dağıtım yuvası değiştirme hataları ve bunları nasıl düzelteceğimiz](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/)hakkında bilgi.

Isınma davranışını aşağıdaki [uygulama ayarlarından](configure-common.md)biriyle veya her ikisiyle de özelleştirebilirsiniz:

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Sitenizi ısıtmak için ping yolu. Değer olarak eğik çizgiyle başlayan özel bir yol belirterek bu uygulama ayarını ekleyin. `/statuscheck` bunun bir örneğidir. Varsayılan değer: `/`. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Isınma işlemi için geçerli HTTP yanıt kodları. Bu uygulama ayarını virgülle ayrılmış BIR HTTP kodları listesiyle ekleyin. Bir örnek. `200,202` Döndürülen durum kodu listede yoksa, ısınma ve takas işlemleri durdurulur. Varsayılan olarak, tüm yanıt kodları geçerlidir.

> [!NOTE]
> Yapılandırma `<applicationInitialization>` öğesi her uygulama başlatmasının bir parçasıdır, iki ısınma davranışı uygulaması ayarı ise yalnızca yuva değiştirmeleri için geçerlidir.

Herhangi bir sorununuz varsa, [Sorun Giderme takaslarına](#troubleshoot-swaps)bakın.

## <a name="monitor-a-swap"></a>Bir takası izleme

Takas [işleminin](#AboutConfiguration) tamamlanması uzun sürüyorsa, [işlem günlüğündeki](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)takas işlemi hakkında bilgi alabilirsiniz.

Uygulamanızın portaldaki kaynak sayfasında, sol bölmede Etkinlik **günlüğü'nü**seçin.

Günlük sorgusunda bir takas işlemi `Swap Web App Slots`. Ayrıntıları görmek için genişletebilir ve alt işlemlerden veya hatalardan birini seçebilirsiniz.

## <a name="route-traffic"></a>Rota trafiği

Varsayılan olarak, uygulamanın üretim URL'sine`http://<app_name>.azurewebsites.net`gelen tüm istemci istekleri ( ) üretim yuvasına yönlendirilir. Trafiğin bir kısmını başka bir yuvaya yönlendirebilirsiniz. Bu özellik, yeni bir güncelleştirme için kullanıcı geri bildirimine ihtiyacınız varsa, ancak bunu üretime bırakmaya hazır değilseniz yararlıdır.

### <a name="route-production-traffic-automatically"></a>Üretim trafiğini otomatik olarak yönlendirme

Üretim trafiğini otomatik olarak yönlendirmek için:

1. Uygulamanızın kaynak sayfasına gidin ve **Dağıtım yuvalarını**seçin.

2. Yönlendirmek istediğiniz yuvanın **Trafik %** sütununda, yönlendirmek istediğiniz toplam trafik miktarını temsil edecek bir yüzde (0 ile 100 arasında) belirtin. **Kaydet'i**seçin.

    ![Trafik yüzdesi ayarlama](./media/web-sites-staged-publishing/RouteTraffic.png)

Ayar kaydedildikten sonra, belirtilen istemci yüzdesi rasgele üretim dışı yuvaya yönlendirilir. 

İstemci otomatik olarak belirli bir yuvaya yönlendirildikten sonra, bu istemci oturumunun ömrü için bu yuvaya "sabitlenir". İstemci tarayıcısında, HTTP başlıklarınızdaki `x-ms-routing-name` çereze bakarak oturumunuzun hangi yuvaya sabitlenilmiş olduğunu görebilirsiniz. "Evreleme" yuvasına yönlendirilen bir istek çerezi `x-ms-routing-name=staging`vardır. Üretim yuvasına yönlendirilen bir istek çerezvardır. `x-ms-routing-name=self`

   > [!NOTE]
   > Azure portalının yanında, DevOps [`az webapp traffic-routing set`](/cli/azure/webapp/traffic-routing#az-webapp-traffic-routing-set) boru hatları veya diğer otomasyon sistemleri gibi CI/CD araçlarındaki yönlendirme yüzdelerini ayarlamak için Azure CLI'deki komutu da kullanabilirsiniz.
   > 

### <a name="route-production-traffic-manually"></a>Üretim trafiğini manuel olarak yönlendirme

Otomatik trafik yönlendirmesine ek olarak, App Service istekleri belirli bir yuvaya yönlendirebilir. Bu, kullanıcılarınızın beta uygulamanızı seçebilmesini veya bu uygulamadan çıkabilmesini istediğinizde kullanışlıdır. Üretim trafiğini el ile yönlendirmek `x-ms-routing-name` için sorgu parametresini kullanırsınız.

Örneğin, kullanıcıların beta uygulamanızdan çıkmalarına izin vermek için bu bağlantıyı web sayfanıza koyabilirsiniz:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

Dize `x-ms-routing-name=self` üretim yuvasını belirtir. İstemci tarayıcısı bağlantıya eriştikten sonra üretim yuvasına yönlendirilir. Sonraki her istek, oturumu üretim yuvasına sabitleyen `x-ms-routing-name=self` çereze sahiptir.

Kullanıcıların beta uygulamanızı seçmesine izin vermek için, aynı sorgu parametresini üretim dışı yuvanın adına ayarlayın. Bir örneği aşağıda verilmiştir:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

Varsayılan olarak, yeni yuvalara gri renkle `0%`gösterilen bir yönlendirme kuralı verilir. Bu değeri açıkça (siyah `0%` metinde gösterildiği şekilde) ayarladığınızda, kullanıcılarınız sorgu parametresini kullanarak hazırlama yuvasına `x-ms-routing-name` el ile erişebilir. Ancak yönlendirme yüzdesi 0 olarak ayarlandığı için yuvaya otomatik olarak yönlendirilmezler. Bu, iç ekiplerin yuvadaki değişiklikleri sınamasını sağlarken evreleme yuvanızı halktan "gizleyebileceğiniz" gelişmiş bir senaryodur.

<a name="Delete"></a>

## <a name="delete-a-slot"></a>Yuvayı silme

Uygulamanızı arayın ve seçin.  >  **Genel>Genel***\<Bakış'ı silmek için *Dağıtım **yuvasını** > seçin. Komut çubuğunda **Sil'i** seçin.  

![Dağıtım yuvasını silme](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>PowerShell ile otomatikleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell, Azure Uygulama Hizmeti'ndeki dağıtım yuvalarını yönetme desteği de dahil olmak üzere Windows PowerShell üzerinden Azure'u yönetmek için cmdlets sağlayan bir modüldür.

Azure PowerShell'i yükleme ve yapılandırma ve Azure PowerShell'inizin Azure aboneliğinizle kimlik doğrulamasını niçin [yükleyip yapılandırılabildiğini](/powershell/azure/overview)öğrenmek için Bkz.  

---
### <a name="create-a-web-app"></a>Web uygulaması oluşturma
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

---
### <a name="create-a-slot"></a>Yuva oluşturma
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

---
### <a name="initiate-a-swap-with-a-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-the-source-slot"></a>Önizleme (çok aşamalı takas) ile takas başlatın ve hedef yuva yapılandırmasını kaynak yuvaya uygulayın
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. "production"]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

---
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-the-source-slot-configuration"></a>Bekleyen bir takası iptal etme (gözden geçirmeyle takas) ve kaynak yuvası yapılandırmasını geri yükleme
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

---
### <a name="swap-deployment-slots"></a>Dağıtım yuvalarını değiştirme
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. "production"]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Etkinlik günlüğündeki takas olaylarını izleme
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

---
### <a name="delete-a-slot"></a>Yuvayı silme
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

## <a name="automate-with-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarıyla otomatikleştir

[Azure Kaynak Yöneticisi şablonları,](https://docs.microsoft.com/azure/azure-resource-manager/template-deployment-overview) Azure kaynaklarının dağıtımını ve yapılandırmasını otomatikleştirmek için kullanılan bildirimsel JSON dosyalarıdır. Kaynak Yöneticisi şablonlarını kullanarak yuvaları değiştirmek için *Microsoft.Web/sites/slot* ve *Microsoft.Web/sites* kaynaklarında iki özellik ayarlarsınız:

- `buildVersion`: bu, yuvada dağıtılan uygulamanın geçerli sürümünü temsil eden bir dize özelliğidir. Örneğin: "v1", "1.0.0.1", veya "2019-09-20T11:53:25.2887393-07:00".
- `targetBuildVersion`: bu, yuvanın ne `buildVersion` olması gerektiğini belirten bir dize özelliğidir. TargetBuildVersion akıma `buildVersion`eşit değilse, bu belirtilen `buildVersion`yuvayı bularak takas işlemini tetikleyecektir.

### <a name="example-resource-manager-template"></a>Örnek Kaynak Yöneticisi şablonu

Aşağıdaki Kaynak Yöneticisi şablonu `buildVersion` evreleme yuvasını günceller ve üretim yuvasını ayarlar. `targetBuildVersion` Bu iki yuvayı değiştirir. Şablon, "evreleme" adlı bir yuvayla oluşturulmuş bir web uygulamanız olduğunu varsayar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "my_site_name": {
            "defaultValue": "SwapAPIDemo",
            "type": "String"
        },
        "sites_buildVersion": {
            "defaultValue": "v1",
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Web/sites/slots",
            "apiVersion": "2018-02-01",
            "name": "[concat(parameters('my_site_name'), '/staging')]",
            "location": "East US",
            "kind": "app",
            "properties": {
                "buildVersion": "[parameters('sites_buildVersion')]"
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "apiVersion": "2018-02-01",
            "name": "[parameters('my_site_name')]",
            "location": "East US",
            "kind": "app",
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites/slots', parameters('my_site_name'), 'staging')]"
            ],
            "properties": {
                "targetBuildVersion": "[parameters('sites_buildVersion')]"
            }
        }        
    ]
}
```

Bu Kaynak Yöneticisi şablonu idempotenttir, yani tekrar tekrar yürütülebilir ve yuvaların aynı durumunu üretebilir. İlk yürütmeden `targetBuildVersion` sonra, geçerli `buildVersion`eşleşecek , böylece bir takas tetiklenmez.

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-the-cli"></a>CLI ile otomatikleştirin

Dağıtım yuvaları için [Azure CLI](https://github.com/Azure/azure-cli) komutları için [az webapp dağıtım yuvasına](/cli/azure/webapp/deployment/slot)bakın.

## <a name="troubleshoot-swaps"></a>Sorun giderme swapları

Bir [yuva değişimi](#AboutConfiguration)sırasında herhangi bir hata oluşursa, *D:\home\LogFiles\eventlog.xml'de*günlüğe kaydedilir. Ayrıca uygulamaya özgü hata günlüğünde de günlüğe kaydedilir.

Bazı sık kullanılan takas hataları şunlardır:

- Uygulama köküne bir HTTP isteği zamanlanır. Takas işlemi her HTTP isteği için 90 saniye bekler ve 5 keze kadar yeniden çalışır. Tüm yeniden denemeler zaman dolduysa, takas işlemi durdurulur.

- Uygulama içeriği yerel önbellek için belirtilen yerel disk kotasını aştığında yerel önbellek başlatma başarısız olabilir. Daha fazla bilgi için [Yerel önbelleğe genel bakış](overview-local-cache.md)abakın.

- [Özel ısınma](#Warm-up)sırasında, HTTP istekleri dahili olarak (harici URL'den geçmeden) yapılır. *Web.config'deki*belirli URL yeniden yazma kurallarıyla başarısız olabilirler. Örneğin, alan adlarını yeniden yönlendirme veya HTTPS'yi zorlama kuralları ısınma isteklerinin uygulama koduna ulaşmasını engelleyebilir. Bu sorunu çözmek için, aşağıdaki iki koşul ekleyerek yeniden yazma kurallarınızı değiştirin:

    ```xml
    <conditions>
      <add input="{WARMUP_REQUEST}" pattern="1" negate="true" />
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Özel bir ısınma olmadan, URL yeniden yazma kuralları yine de HTTP isteklerini engelleyebilir. Bu sorunu çözmek için, aşağıdaki koşulu ekleyerek yeniden yazma kurallarınızı değiştirin:

    ```xml
    <conditions>
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Bazı [IP kısıtlama kuralları,](app-service-ip-restrictions.md) takas işleminin uygulamanıza HTTP istekleri göndermesini engelleyebilir. IPv4 adresi, dağıtımınızla `10.` `100.` başlayan ve dahili olan aralıkları. Uygulamanıza bağlanmalarına izin vermelisiniz.

- Yuva değiştirmelerinden sonra, uygulama beklenmeyen yeniden başlatmalarla karşılaşabilir. Bunun nedeni, bir takastan sonra, ana bilgisayar adı bağlama yapılandırmasının eşitlenmemiş olması dır ve bu da tek başına yeniden başlatmaya neden olmaz. Ancak, bazı temel depolama olayları (depolama hacmi arızaları gibi) bu tutarsızlıkları algılayabilir ve tüm alt işlemleri yeniden başlatmaya zorlayabilir. Bu tür yeniden başlatmaları en aza indirmek [ `WEBSITE_ADD_SITENAME_BINDINGS_IN_APPHOST_CONFIG=1` için, uygulama ayarını](https://github.com/projectkudu/kudu/wiki/Configurable-settings#disable-the-generation-of-bindings-in-applicationhostconfig) *tüm yuvalarda*ayarlayın. Ancak, bu uygulama ayarı Windows Communication Foundation (WCF) uygulamalarında *çalışmaz.*

## <a name="next-steps"></a>Sonraki adımlar
[Üretim dışı yuvalara erişimi engelleme](app-service-ip-restrictions.md)
