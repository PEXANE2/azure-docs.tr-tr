---
title: Trafik Yöneticisi ile trafiği kontrol edin
description: Azure Trafik Yöneticisi'ni Azure Uygulama Hizmeti ile tümleştirirken yapılandırmak için en iyi uygulamaları bulun.
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.topic: article
ms.date: 02/25/2016
ms.custom: seodec18
ms.openlocfilehash: 040f84288c66f4506919e775b9ea41324b617cfa
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437892"
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Azure Trafik Yöneticisi ile Azure Uygulama Hizmeti trafiğini denetleme
> [!NOTE]
> Bu makalede, Azure Uygulama Hizmeti ile ilgili olarak Microsoft Azure Trafik Yöneticisi için özet bilgiler sağlar. Azure Trafik Yöneticisi'nin kendisi hakkında daha fazla bilgiyi bu makalenin sonundaki bağlantıları ziyaret ederek bulabilirsiniz.
> 
> 

## <a name="introduction"></a>Giriş
Azure Traffic Manager'ı kullanarak web istemcilerinden gelen isteklerin Azure App Service'teki uygulamalara nasıl dağıtılacağını denetleyebilirsiniz. App Service uç noktaları bir Azure Traffic Manager profiline eklendiğinde Azure Traffic Manager, App Service uygulamalarının durumunu (çalışıyor, durduruldu veya silindi) izleyerek trafik gönderilecek uç noktaları belirleyebilir.

## <a name="routing-methods"></a>Yönlendirme yöntemleri
Azure Trafik Yöneticisi dört farklı yönlendirme yöntemi kullanır. Bu yöntemler, Azure Uygulama Hizmeti ile ilgili olarak aşağıdaki listede açıklanmıştır.

* ** [Öncelik](../traffic-manager/traffic-manager-routing-methods.md#priority-traffic-routing-method):** tüm trafik için birincil bir uygulama kullanın ve birincil veya yedek uygulamaların kullanılamaması durumunda yedekleme sağlayın.
* ** [Ağırlıklı](../traffic-manager/traffic-manager-routing-methods.md#weighted):** trafiği, tanımladığınız bir uygulama kümesine eşit olarak veya ağırlıklara göre dağıtın.
* ** [Performans](../traffic-manager/traffic-manager-routing-methods.md#performance):** farklı coğrafi konumlarda uygulamalarınız olduğunda, en düşük ağ gecikmesi açısından "en yakın" uygulamayı kullanın.
* ** [Coğrafi:](../traffic-manager/traffic-manager-routing-methods.md#geographic)** kullanıcıları DNS sorgularının hangi coğrafi konumdan geldiğine göre belirli uygulamalara yönlendirin. 

Daha fazla bilgi için [Trafik Yöneticisi yönlendirme yöntemlerine](../traffic-manager/traffic-manager-routing-methods.md)bakın.

## <a name="app-service-and-traffic-manager-profiles"></a>Uygulama Servisi ve Trafik Yöneticisi Profilleri
Uygulama Hizmeti uygulama trafiğinin denetimini yapılandırmak için, Azure Trafik Yöneticisi'nde daha önce açıklanan dört yük dengeleme yönteminden birini kullanan bir profil oluşturur ve ardından trafiği denetlemek istediğiniz uç noktaları (bu durumda, App Service) eklersiniz. Uygulama durumunuz (çalışan, durdurulan veya silinmiş) düzenli olarak profile iletilir, böylece Azure Trafik Yöneticisi trafiği buna göre yönlendirebilir.

Azure ile Azure Trafik Yöneticisi'ni kullanırken aşağıdaki noktaları göz önünde bulundurun:

* Uygulama Hizmeti, yalnızca aynı bölgedeki uygulama dağıtımları için, uygulama moduna bakılmaksızın zaten başarısız ve yuvarlak robin işlevselliği sağlar.
* Uygulama Hizmetini başka bir Azure bulut hizmetiyle birlikte kullanan aynı bölgedeki dağıtımlar için, karma senaryoları etkinleştirmek için her iki uç nokta türünü birleştirebilirsiniz.
* Bir profilde bölge başına yalnızca bir Uygulama Hizmeti bitiş noktası belirtebilirsiniz. Bir uygulamayı bir bölge için bitiş noktası olarak seçtiğinizde, o bölgedeki kalan uygulamalar bu profil için seçim için kullanılamaz hale gelir.
* Azure Trafik Yöneticisi profilinde belirttiğiniz Uygulama Hizmeti bitiş noktaları, profildeki uygulamanın Yapılandırılan sayfasındaki **Alan Adları** bölümünün altında görünür, ancak burada yapılandırılamaz.
* Bir profile bir uygulama ekledikten sonra, uygulamanın portal sayfasının Panosundaki Site URL'si, bir uygulama ayarladıysanız uygulamanın özel etki alanı URL'sini görüntüler. **Site URL** Aksi takdirde, Trafik Yöneticisi profil URL'sini görüntüler (örneğin,). `contoso.trafficmanager.net` Uygulamanın doğrudan etki alanı adı ve Trafik Yöneticisi URL'si, uygulamanın **Alan Adları** bölümü altındaki Yapılandırma sayfasında görünür.
* Özel alan adlarınız beklendiği gibi çalışır, ancak bunları uygulamalarınız için eklemenin yanı sıra, DNS haritanızı Trafik Yöneticisi URL'sini işaret etmek üzere yapılandırmanız gerekir. Bir Uygulama Hizmeti uygulaması için özel bir etki alanı ayarlama hakkında bilgi için, [Trafik Yöneticisi tümleştirmesiyle Azure Uygulama Hizmeti'nde özel bir etki alanı adı yapılandır'a](configure-domain-traffic-manager.md)bakın.
* Azure Trafik Yöneticisi profiline yalnızca standart veya premium modda olan uygulamaları ekleyebilirsiniz.
* Trafik Yöneticisi profiline uygulama eklemek, uygulamanın yeniden başlatılmasına neden olur.

## <a name="next-steps"></a>Sonraki Adımlar
Azure Trafik Yöneticisi'ne kavramsal ve teknik bir genel bakış için Trafik [Yöneticisine Genel Bakış](../traffic-manager/traffic-manager-overview.md)bölümüne bakın.


