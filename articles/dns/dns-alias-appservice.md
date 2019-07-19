---
title: Tepesinde bölgesinde yük dengeli Azure Web uygulamaları barındırın
description: Tepesinde bölgesinde yük dengeli Web uygulamaları barındırmak için bir Azure DNS diğer ad kaydı kullanın
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 7/13/2019
ms.author: victorh
ms.openlocfilehash: 7d20ef750aa4556a73852982631423d3d08271f5
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854106"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Tepesinde bölgesinde yük dengeli Azure Web uygulamaları barındırın

DNS protokolü, tepesinde bölgesinde bir veya AAAA kaydı dışında herhangi bir şeyin atanmasını engeller. Örnek bir bölge tepesinde contoso.com. Bu kısıtlama, Traffic Manager arkasındaki yük dengeli uygulamalara sahip olan uygulama sahipleri için bir sorun gösterir. Bölge tepesinde kaydından Traffic Manager profile işaret etmek mümkün değildir. Sonuç olarak, uygulama sahiplerinin geçici bir çözüm kullanması gerekir. Uygulama katmanında yeniden yönlendirme, Zone tepesinde 'tan başka bir etki alanına yeniden yönlendirmelidir. Örnek, contoso.com 'ten www\.contoso.com 'e yeniden yönlendirme örneğidir. Bu düzenleme, yeniden yönlendirme işlevi için tek bir hata noktası sunar.

Diğer ad kayıtlarıyla, bu sorun artık yok. Artık uygulama sahipleri, bölge tepesinde kayıtlarını dış uç noktalara sahip bir Traffic Manager profiline işaret edebilir. Uygulama sahipleri, DNS bölgesi içindeki diğer etki alanı için kullanılan aynı Traffic Manager profilini işaret edebilir.

Örneğin, contoso.com ve www\.contoso.com aynı Traffic Manager profilini işaret edebilir. Traffic Manager profilinin yalnızca yapılandırılmış dış uç noktaları olduğu sürece bu durum geçerlidir.

Bu makalede, etki alanı tepesinde için bir diğer ad kaydı oluşturmayı ve Web uygulamalarınız için Traffic Manager profili uç noktalarınızı yapılandırmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Etki alanınızı Azure DNS barındırmanıza yönelik yönergeler için bkz [. Öğretici: Etki alanınızı Azure DNS](dns-delegate-domain-azure-dns.md)barındırın.

Bu öğreticide örnek olarak contoso.com etki alanı kullanılmaktadır ancak sizin kendi etki alanı adınızı kullanmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu makalede kullanılan tüm kaynakları barındıracak bir kaynak grubu oluşturun.

## <a name="create-app-service-plans"></a>App Service planları oluşturma

Yapılandırma bilgileri için aşağıdaki tabloyu kullanarak kaynak grubunuzda iki Web App Service planı oluşturun. App Service planı oluşturma hakkında daha fazla bilgi için bkz. [Azure 'da App Service planını yönetme](../app-service/app-service-plan-manage.md).


|Ad  |İşletim sistemi  |Location  |Fiyatlandırma Katmanı  |
|---------|---------|---------|---------|
|ASP-01     |Windows|East US|Geliştirme/test D1-paylaşılan|
|ASP-02     |Windows|Orta ABD|Geliştirme/test D1-paylaşılan|

## <a name="create-app-services"></a>Uygulama hizmetleri oluşturma

Her bir App Service planında bir tane olmak üzere iki Web uygulaması oluşturun.

1. Azure portal sayfanın sol üst köşesinde **kaynak oluştur ' a**tıklayın.
2. Arama çubuğuna **Web uygulaması** yazın ve ENTER tuşuna basın.
3. **Web uygulaması**' na tıklayın.
4. **Oluştur**’a tıklayın.
5. Varsayılan değerleri kabul edin ve iki Web uygulaması yapılandırmak için aşağıdaki tabloyu kullanın:

   |Ad<br>(. azurewebsites.net içinde benzersiz olmalıdır)|Kaynak Grubu |Plan/konum App Service
   |---------|---------|---------|
   |Uygulama-01|Mevcut olanı kullan<br>Kaynak grubunuzu seçin|ASP-01 (Doğu ABD)|
   |Uygulama-02|Mevcut olanı kullan<br>Kaynak grubunuzu seçin|ASP-02 (Orta ABD)|

### <a name="gather-some-details"></a>Bazı ayrıntılar toplayın

Şimdi, uygulamalar için IP adresini ve ana bilgisayar adını bilmeniz gerekir.

1. Kaynak grubunuzu açın ve ilk uygulamanıza tıklayın (Bu örnekteki**App-01** ).
2. Sol sütunda, **Özellikler**' e tıklayın.
3. **URL 'nin**altındaki adresi ve **giden IP adresleri** altındaki listedeki ilk IP adresini aklınızda yapın. Traffic Manager bitiş noktalarınızı yapılandırırken bu bilgileri daha sonra kullanacaksınız.
4. **App-02**için yineleyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Kaynak grubunuzda bir Traffic Manager profili oluşturun. Varsayılan değerleri kullanın ve trafficmanager.net ad alanı içinde benzersiz bir ad yazın.

Traffic Manager profili oluşturma hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Yüksek oranda kullanılabilir bir Web uygulaması](../traffic-manager/quickstart-create-traffic-manager-profile.md)için Traffic Manager profili oluşturun.

### <a name="create-endpoints"></a>Uç noktalar oluşturma

Artık iki Web uygulaması için uç noktalar oluşturabilirsiniz.

1. Kaynak grubunuzu açın ve Traffic Manager profilinize tıklayın.
2. Sol sütunda **uç noktalar**' a tıklayın.
3. **Ekle**'yi tıklatın.
4. Uç noktaları yapılandırmak için aşağıdaki tabloyu kullanın:

   |Type  |Ad  |Hedef  |Location  |Özel üstbilgi ayarları|
   |---------|---------|---------|---------|---------|
   |Dış uç nokta     |Son-01|Uygulama için kaydettiğiniz IP adresi-01|East US|Ana bilgisayar\<: App-01 için kaydettiğiniz URL\><br>Örnek: **Host: App-01.azurewebsites.net**|
   |Dış uç nokta     |End-02|App-02 için kaydettiğiniz IP adresi|Orta ABD|Ana bilgisayar\<: App-02 için kaydettiğiniz URL\><br>Örnek: **Host: App-02.azurewebsites.net**

## <a name="create-dns-zone"></a>DNS bölgesi oluştur

Test için varolan bir DNS bölgesi kullanabilir veya yeni bir bölge oluşturabilirsiniz. Azure 'da yeni bir DNS bölgesi oluşturup atamak için bkz [. Öğretici: Etki alanınızı Azure DNS](dns-delegate-domain-azure-dns.md)barındırın.

### <a name="add-the-alias-record-set"></a>Diğer ad kayıt kümesini ekleme

DNS bölgeniz hazırsanız, tepesinde bölgesi için bir diğer ad kaydı ekleyebilirsiniz.

1. Kaynak grubunuzu açın ve DNS bölgesine tıklayın.
2. **Kayıt kümesi**’ne tıklayın.
3. Aşağıdaki tabloyu kullanarak kayıt kümesini ekleyin:

   |Ad  |Type  |Diğer ad kayıt kümesi  |Diğer ad türü  |Azure kaynağı|
   |---------|---------|---------|---------|-----|
   |@     |A|Evet|Azure kaynağı|Traffic Manager-profiliniz|

## <a name="add-custom-hostnames"></a>Özel ana bilgisayar adları ekleyin

Web uygulamalarına özel bir ana bilgisayar adı ekleyin.

1. Kaynak grubunuzu açın ve ilk Web uygulamanıza tıklayın.
2. Sol sütunda **özel etki alanları**' na tıklayın.
3. **Konak adı Ekle**' ye tıklayın.
4. Ana bilgisayar adı ' nın altında, etki alanı adınızı yazın. Örneğin, contoso.com.

   Etki alanınız, **ana bilgisayar adı kullanılabilirliği** ve **etki alanı sahipliğinin**yanında doğrulamayı geçmelidir ve yeşil onay işaretlerini göstermelidir.
5. **Konak adı Ekle**' ye tıklayın.
6. **Siteye atanan ana bilgisayar adları**altındaki yeni ana bilgisayar adını görmek için tarayıcınızı yenileyin. Sayfadaki yenileme her zaman değişiklikleri hemen göstermez.
7. İkinci Web uygulamanız için bu yordamı tekrarlayın.

## <a name="test-your-web-apps"></a>Web uygulamalarınızı test etme

Artık Web uygulamanıza ulaşabildiğinizden ve yük dengeli olduğundan emin olmak için test edebilirsiniz.

1. Bir Web tarayıcısı açın ve etki alanına gidin. Örneğin, contoso.com. Varsayılan Web uygulaması sayfasını görmeniz gerekir.
2. İlk Web uygulamanızı durdurun.
3. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
4. Web tarayıcınızı başlatın ve etki alanına gidin. Yine de varsayılan Web uygulaması sayfasını görmeniz gerekir.
5. İkinci Web uygulamanızı durdurun.
6. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
7. Web tarayıcınızı başlatın ve etki alanına gidin. Web uygulamasının durdurulduğunu belirten 403 hatasını görmeniz gerekir.
8. İkinci Web uygulamanızı başlatın.
9. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
10. Web tarayıcınızı başlatın ve etki alanına gidin. Varsayılan Web uygulaması sayfasını yeniden görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Öğretici: Azure genel IP adresine başvurmak için bir diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Traffic Manager ile tepesinde etki alanı adlarını desteklemek için bir diğer ad kaydı yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)

Etkin bir DNS adını nasıl geçirebileceğinizi öğrenmek için bkz. [Azure App Service için etkin BIR DNS adı geçirme](../app-service/manage-custom-dns-migrate-domain.md).
