---
title: 'Quickstart: Yüksek uygulama kullanılabilirliği için Ön Kapı profili oluşturun'
description: Bu hızlı başlangıç makalesinde yüksek oranda kullanılabilir ve yüksek performanslı global web uygulamanız için Front Door oluşturma adımları anlatılmaktadır.
services: front-door
documentationcenter: ''
author: sharad4u
editor: ''
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2018
ms.author: sharadag
ms.openlocfilehash: 2b44c0cdbe2d955efe20a5f9473a29bc9f500a07
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521460"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application"></a>Hızlı başlangıç: Yüksek oranda kullanılabilir global web uygulaması için Front Door oluşturma

Bu hızlı başlangıçta global web uygulamanız için yüksek oranda kullanılabilirlik ve yüksek performans sunan bir Front Door profili oluşturma adımları anlatılmaktadır. 

Bu hızlı başlangıçta anlatılan senaryo, bir web uygulamasının farklı Azure bölgelerinde çalışan iki örneğini kapsamaktadır. Kullanıcı trafiğinin uygulamayı çalıştıran en yakın site arka ucu kümesine yönlendirilmesine yardımcı olmak için eşit [ağırlıklı ve aynı öncelik düzeyine sahip arka uçları](front-door-routing-methods.md) temel alan bir Front Door yapılandırması oluşturulmaktadır. Front Door, web uygulamasını sürekli izler ve en yakın site kullanım dışı olduğunda bir sonraki kullanılabilir arka uca otomatik yük devretme gerçekleştirir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 
https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıç için bir web uygulamasının farklı Azure bölgelerinde (*Doğu ABD* ve *Batı Avrupa*) çalışan iki örneğini dağıtmış olmanız gerekir. İki web uygulaması örneği de Etkin/Etkin modunda çalışıyor olmalıdır. Başka bir deyişle birinin yük devretme durumunda devreye girdiği Etkin/Beklemede yapılandırmasından farklı olarak ikisinin de her an trafik kabul eder durumda olması gerekir.

1. Ekranın sol üst tarafında kaynak**Web** > **Web Uygulaması** **Oluştur'u** > seçin.
2. **Web Uygulaması** sayfasında aşağıdaki bilgileri girdikten veya seçtikten sonra değer belirtilmeyen yerler için varsayılan ayarları kabul edin:

     | Ayar         | Değer     |
     | ---              | ---  |
     | Kaynak grubu          | **Yeni**'yi seçin ve *myResourceGroupFD1* yazın. |
     | Adı           | Web uygulamanız için benzersiz bir ad girin  |
     | Çalışma zamanı yığını          | Uygulamanız için Bir Runtime yığını seçin |
     |      Bölge  |   Batı ABD        |
     | Uygulama hizmeti planı/Konumu         | **Yeni'yi**seçin.  App Service planına *myAppServicePlanEastUS* yazın ve **Tamam**'ı seçin.| 
     |Sku ve boyut  | **Standart** **S1 100 toplam ACU, 1,75 GB bellek** seçin |
     
3. **Gözden Geçir + Oluştur'u**seçin.
4. Web Uygulamasının Özet bilgilerini gözden geçirin. **Oluştur'u**seçin.
5. Yaklaşık 5 dakika sonra, Web Uygulaması başarıyla dağıtıldığında varsayılan bir web sitesi oluşturulur.
6. 1-3 arası adımları tekrarlayarak aşağıdaki ayarlarla farklı bir Azure bölgesinde ikinci bir web sitesi oluşturun:

     | Ayar         | Değer     |
     | ---              | ---  |
     | Kaynak grubu          | **Yeni**'yi seçin ve *myResourceGroupFD2* yazın. |
     | Adı           | Web Uygulamanız için benzersiz bir ad girin  |
     | Çalışma zamanı yığını          | Uygulamanız için Bir Runtime yığını seçin |
     |      Bölge  |   Batı Avrupa      |
     | Uygulama hizmeti planı/Konumu         | **Yeni'yi**seçin.  App Service planına *myAppServicePlanWestEurope* yazın ve **Tamam**'ı seçin.|   
     |Sku ve boyut  | **Standart** **S1 100 toplam ACU, 1,75 GB bellek** seçin |
    
## <a name="create-a-front-door-for-your-application"></a>Uygulamanız için Front Door oluşturma
### <a name="a-add-a-frontend-host-for-front-door"></a>A. Front Door için ön uç ana bilgisayar adı ekleme
Kullanıcı trafiğini iki arka uç arasındaki en düşük gecikme süresine göre yönlendiren bir Front Door yapılandırması oluşturun.

1. Ekranın sol üst tarafında, **bir kaynak** > **Oluştur'un** > **Ön Kapı'sını**seçin.
2. **Ön Kapı Oluştur'da,** aşağıdaki bilgileri girin veya seçin ve hiçbirinin belirtilmediği varsayılan ayarları girin:

     | Ayar         | Değer     |
     | ---              | ---  |
     |Abonelik  | Front Door'un oluşturulmasını istediğiniz aboneliği seçin.|
     | Kaynak grubu          | **Yeni'yi**seçin ve ardından *myResourceGroupFD0* yazın |
     | Kaynak grubu konumu  |   Orta ABD        |
     
     > [!NOTE]
     > Ön Kapı'yı dağıtmak için yeni bir Kaynak Grubu oluşturmanız gerekmez.  Varolan bir Kaynak Grubu da seçebilirsiniz.
     
3. **Sonraki'yi tıklatın: Yapılandırma.**
4. Frontends/domains kartındaki '+' simgesini tıklatın.  **Ana Bilgisayar** `<Your Initials>frontend`adı için girin. Bu ana bilgisayar adı küresel benzersiz olması gerekir, Ön Kapı doğrulama ilgilenir.
5. **Ekle**’ye tıklayın.

### <a name="b-add-application-backend-and-backend-pools"></a>B. Uygulama arka ucunu ve arka uç havuzlarını ekleme

Ardından, uygulamanızın nerede bulunduğunu bilmek için Frontends/etki alanlarını ve uygulama arka uçunuzu ön uç havuzunda yapılandırmanız gerekir. 

1. Arka uç havuzları kartındaki '+' simgesine tıklayarak arka **Name** uç havuzunuz için `myBackendPool`Ad için bir arka uç havuzu ekleyin, girin.
2. Ardından, daha önce oluşturulmuş web sitelerinizi eklemek için **arka uç ekle'yi** tıklatın.
3. 'Uygulama Hizmeti' olarak **Backend ana bilgisayar türünü** seçin, web sitesini oluşturduğunuz aboneliği seçin ve ardından **Backend ana bilgisayar adı** açılır sayfasından ilk web sitesini seçin.
4. Kalan alanları şimdilik olduğu gibi bırakın ve **Ekle**'ye tıklayın.
5. 'Uygulama Hizmeti' olarak **Backend ana bilgisayar türünü** seçin, web sitesini oluşturduğunuz aboneliği seçin ve ardından **Backend ana bilgisayar adı** açılır sayfasından **ikinci** web sitesini seçin.
6. Kalan alanları şimdilik olduğu gibi bırakın ve **Ekle**'ye tıklayın.
7. İsteğe bağlı olarak arka uç havuzu için Sistem Durumu Sondaları ve Yük Dengeleme ayarlarını güncelleştirmeyi seçebilirsiniz, ancak varsayılan değerler de çalışmalıdır. Her iki durumda da **Ekle'yi**tıklatın.


### <a name="c-add-a-routing-rule"></a>C. Yönlendirme kuralı ekleme
1. Son olarak, yönlendirme kuralını yapılandırmak için Yönlendirme kuralları kartındaki '+' simgesini tıklatın. Bu kural ön uç ana bilgisayarınızı arka uç havuzuyla eşlemek için gereklidir ve temelde `myappfrontend.azurefd.net` hedefine ulaşan bir isteği `myBackendPool` arka ucuna yönlendirme işlemidir. 
2. **Ad** için 'LocationRule' girin.
3. Front Door'unuza yönlendirme kuralı eklemek için **Ekle**'ye tıklayın. 
4. Gözden **Geçir ve Oluştur'a**tıklayın.
5. Ön Kapı oluşturma ayarlarını gözden geçirin. **Oluştur'u** tıklatın

>[!WARNING]
> Front Door'unuzdaki her ön uç ana bilgisayar adı ile ilişkilendirilmiş varsayılan yola ('/\*') sahip yönlendirme kuralı olduğundan **emin olmanız** gerekir. Başka bir deyişle tüm yönlendirme kurallarınızın arasında ön uç ana bilgisayar adlarınızın her biri için varsayılan yolda ('/\*') tanımlanmış en az bir yönlendirme kuralı olmalıdır. Aksi takdirde son kullanıcı trafiği doğru şekilde yönlendirilmeyebilir.

## <a name="view-front-door-in-action"></a>Front Door'u uygulamalı olarak görme
Front Door'u oluşturduğunuzda yapılandırmanın dünyanın her yerine dağıtılması birkaç dakika sürecektir. İşlem tamamlandığında oluşturduğunuz ana bilgisayar adına gidin. Bunun için bir web tarayıcısı açın ve `myappfrontend.azurefd.net` URL'sini yazın. İsteğiniz otomatik olarak arka uç havuzunda belirtilen arka uçlardan en yakın olana yönlendirilir. 

### <a name="view-front-door-handle-application-failover"></a>Front Door'un uygulama yük devretme adımlarını görüntüleme
Front Door'un anında genel yük devretme özelliğini uygulamalı olarak görmek istiyorsanız oluşturduğunuz web sitelerinden birini durdurabilirsiniz. Arka uç havuzunun Sistem Durumu Yoklama ayarlarına bağlı olarak trafik anında diğer web sitesi dağıtımına devredilir. Ayrıca bu davranışı test etmek için arka uç havuzu yapılandırmasında Front Door'unuzun arka ucunu da devre dışı bırakabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekmediğinde **myResourceGroupFD1,** **myResourceGroupFD2**ve **myResourceGroupFD0** kaynak gruplarını silin:

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta yüksek oranda kullanılabilirlik ve maksimum performans gerektiren web uygulamaları için kullanıcı trafiğini yönlendirmenizi sağlayan bir Front Door oluşturdunuz. Trafik yönlendirme hakkında daha fazla bilgi için Front Door tarafından kullanılan [Yönlendirme Yöntemlerini](front-door-routing-methods.md) inceleyin.
