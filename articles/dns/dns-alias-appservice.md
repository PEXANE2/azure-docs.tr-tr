---
title: Bölge tepe noktasında yük dengeli Azure web uygulamalarını barındırın
description: Bölge tepe noktasında yük dengeli web uygulamalarını barındırmak için Azure DNS takma adı kaydı nı kullanma
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 08/10/2019
ms.author: rohink
ms.openlocfilehash: 8ba96a028d51e6e5503bb4a8e6735b48033c9ba1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937358"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Bölge tepe noktasında yük dengeli Azure web uygulamalarını barındırın

DNS protokolü, bölge tepe noktasında ki A veya AAAA kaydı dışında herhangi bir şeyin atanmasını engeller. Örnek bir bölge tepe contoso.com. Bu kısıtlama, Trafik Yöneticisi'nin arkasında yük dengeli uygulamaları olan uygulama sahipleri için bir sorun teşkil eder. Bölge apeks kaydından Trafik Yöneticisi profilini işaret etmek mümkün değildir. Sonuç olarak, uygulama sahipleri bir geçici çözüm kullanmanız gerekir. Uygulama katmanındaki bir yönlendirme, bölge tepesinden başka bir etki alanına yönlendirilmelidir. Bir örnek, contoso.com'dan www\.contoso.com'a yönlendirmedir. Bu düzenleme, yeniden yönlendirme işlevi için tek bir hata noktası sunar.

Diğer ad kayıtlarında, bu sorun artık yok. Artık uygulama sahipleri bölge uç kayıtlarını harici uç noktaları olan bir Trafik Yöneticisi profiline yönlendirebilir. Uygulama sahipleri, DNS bölgesindeki herhangi bir başka etki alanı için kullanılan aynı Trafik Yöneticisi profilini gösterebilir.

Örneğin, contoso.com ve\.www contoso.com aynı Trafik Yöneticisi profilini gösterebilir. Trafik Yöneticisi profili yalnızca dış uç noktaları yapılandırıldıkça durum böyledir.

Bu makalede, etki alanı ekstremleriniz için bir takma ad kaydı oluşturmayı ve web uygulamalarınız için Trafik Yöneticisi profil bitiş noktalarınızı yapılandırmayı öğrenirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Azure DNS’te etki alanınızı barındırma yönergeleri için bkz. [Öğretici: Azure DNS’te etki alanınızı barındırma](dns-delegate-domain-azure-dns.md).

Bu öğreticide örnek olarak contoso.com etki alanı kullanılmaktadır ancak sizin kendi etki alanı adınızı kullanmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu makalede kullanılan tüm kaynakları tutmak için bir kaynak grubu oluşturun.

## <a name="create-app-service-plans"></a>Uygulama Hizmeti planları oluşturma

Yapılandırma bilgileri için aşağıdaki tabloyu kullanarak kaynak grubunuzda iki Web Uygulaması Hizmeti planı oluşturun. Uygulama Hizmeti planı oluşturma hakkında daha fazla bilgi için [Azure'da Bir Uygulama Hizmeti planını yönet'e](../app-service/app-service-plan-manage.md)bakın.


|Adı  |İşletim Sistemi  |Konum  |Fiyatlandırma Katmanı  |
|---------|---------|---------|---------|
|ASP-01     |Windows|Doğu ABD|Dev/Test D1-Paylaşılan|
|ASP-02     |Windows|Orta ABD|Dev/Test D1-Paylaşılan|

## <a name="create-app-services"></a>Uygulama Hizmetleri Oluştur

Her Uygulama Hizmeti planında bir tane olmak üzere iki web uygulaması oluşturun.

1. Azure portal sayfasının sol üst köşesinde **kaynak oluştur'u**seçin.
2. Arama çubuğuna **Web uygulamasını** yazın ve Enter tuşuna basın.
3. **Web Uygulamasını**Seçin.
4. **Oluştur'u**seçin.
5. Varsayılanları kabul edin ve iki web uygulamalarını yapılandırmak için aşağıdaki tabloyu kullanın:

   |Adı<br>(içinde benzersiz olmalıdır .azurewebsites.net)|Kaynak Grubu |Çalışma zamanı yığını|Bölge|Uygulama Hizmet Planı/Konumu
   |---------|---------|-|-|-------|
   |Uygulama-01|Mevcut olanı kullan<br>Kaynak grubunuzu seçin|.NET Core 2.2|Doğu ABD|ASP-01(D1)|
   |Uygulama-02|Mevcut olanı kullan<br>Kaynak grubunuzu seçin|.NET Core 2.2|Orta ABD|ASP-02(D1)|

### <a name="gather-some-details"></a>Bazı ayrıntıları toplama

Artık web uygulamalarının IP adresini ve ana bilgisayar adını not almanız gerekir.

1. Kaynak grubunuzu açın ve ilk web uygulamanızı seçin (Bu örnekte**App-01).**
2. Sol sütunda **Özellikler'i**seçin.
3. **URL**altındaki adresi ve **Giden IP Adresleri'nin** altındaki adresi not edin, listedeki ilk IP adresini not edin. Trafik Yöneticisi bitiş noktalarınızı yapılandırdığınızda bu bilgileri daha sonra kullanırsınız.
4. **App-02**için tekrarlayın.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Kaynak grubunuzda bir Trafik Yöneticisi profili oluşturun. Varsayılanları kullanın ve trafficmanager.net ad alanı içinde benzersiz bir ad yazın.

Trafik Yöneticisi profili oluşturma hakkında daha fazla bilgi [için, bkz.](../traffic-manager/quickstart-create-traffic-manager-profile.md)

### <a name="create-endpoints"></a>Uç noktalar oluşturma

Artık iki web uygulaması için uç noktaları oluşturabilirsiniz.

1. Kaynak grubunuzu açın ve Trafik Yöneticisi profilinizi seçin.
2. Sol sütunda **Uç Noktaları'nı**seçin.
3. **Ekle'yi**seçin.
4. Uç noktaları yapılandırmak için aşağıdaki tabloyu kullanın:

   |Tür  |Adı  |Hedef  |Konum  |Özel Üstbilgi ayarları|
   |---------|---------|---------|---------|---------|
   |Dış uç nokta     |Son-01|App-01 için kaydettiğiniz IP adresi|Doğu ABD|ana\<bilgisayar: App-01 için kaydettiğiniz URL\><br>Örnek: **host:app-01.azurewebsites.net**|
   |Dış uç nokta     |Son-02|App-02 için kaydettiğiniz IP adresi|Orta ABD|ana\<bilgisayar: App-02 için kaydettiğiniz URL\><br>Örnek: **host:app-02.azurewebsites.net**

## <a name="create-dns-zone"></a>DNS bölgesi oluşturma

Sınama için varolan bir DNS bölgesini kullanabilir veya yeni bir bölge oluşturabilirsiniz. Azure'da yeni bir DNS bölgesi oluşturmak ve temsilci vermek için [Bkz. Öğretici: Etki alanınızı Azure DNS'de barındırın.](dns-delegate-domain-azure-dns.md)

## <a name="add-a-txt-record-for-custom-domain-validation"></a>Özel etki alanı doğrulaması için Bir TXT kaydı ekleme

Web uygulamalarınız için özel bir ana bilgisayar adı eklediğinizde, etki alanınızı doğrulamak için belirli bir TXT kaydı arar.

1. Kaynak grubunuzu açın ve DNS bölgesini seçin.
2. **Kayıt kümesi**’ni seçin.
3. Aşağıdaki tabloyu kullanarak kayıt kümesini ekleyin. Değer için, daha önce kaydettiğiniz gerçek web uygulaması URL'sini kullanın:

   |Adı  |Tür  |Değer|
   |---------|---------|-|
   |@     |TXT|App-01.azurewebsites.net|


## <a name="add-a-custom-domain"></a>Özel etki alanı ekleme

Her iki web uygulaması için de özel bir etki alanı ekleyin.

1. Kaynak grubunuzu açın ve ilk web uygulamanızı seçin.
2. Sol sütunda Özel **etki alanlarını**seçin.
3. **Özel Etki Alanları**altında, özel etki alanı **ekle'yi**seçin.
4. **Özel etki alanı**altında, özel etki alanı adınızı yazın. Örneğin, contoso.com.
5. **Doğrula**'yı seçin.

   Etki alanınız doğrulamayı geçirmeli ve **Ana Bilgisayar kullanılabilirliği** ve **Etki Alanı sahipliğinin**yanında yeşil onay işaretleri göstermelidir.
5. **Özel etki alanı ekle**'yi seçin.
6. **Siteye atanan Hostnames**altında yeni ana bilgisayar adını görmek için tarayıcınızı yenileyin. Sayfadaki yenileme her zaman değişiklikleri hemen göstermez.
7. İkinci web uygulamanız için bu yordamı tekrarlayın.

## <a name="add-the-alias-record-set"></a>Diğer ad kayıt kümesini ekleme

Şimdi bölge tepe için bir takma ad kaydı ekleyin.

1. Kaynak grubunuzu açın ve DNS bölgesini seçin.
2. **Kayıt kümesi**’ni seçin.
3. Aşağıdaki tabloyu kullanarak kayıt kümesini ekleyin:

   |Adı  |Tür  |Diğer ad kayıt kümesi  |Diğer ad türü  |Azure kaynağı|
   |---------|---------|---------|---------|-----|
   |@     |A|Evet|Azure kaynağı|Traffic Manager - profiliniz|


## <a name="test-your-web-apps"></a>Web uygulamalarınızı test edin

Artık web uygulamanıza erişebildiğinizden ve yük dengeli olduğundan emin olmak için test edebilirsiniz.

1. Bir web tarayıcısı açın ve etki alanınıza göz atın. Örneğin, contoso.com. Varsayılan web uygulaması sayfasını görmeniz gerekir.
2. İlk web uygulamanızı durdurun.
3. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
4. Web tarayıcınızı başlatın ve etki alanınıza göz atın. Varsayılan web uygulaması sayfasını görmeye devam edebilirsiniz.
5. İkinci web uygulamanızı durdurun.
6. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
7. Web tarayıcınızı başlatın ve etki alanınıza göz atın. Web uygulamasının durdurulduğunu belirten Hata 403'e bakmalısınız.
8. İkinci web uygulamanızı başlatın.
9. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
10. Web tarayıcınızı başlatın ve etki alanınıza göz atın. Varsayılan web uygulaması sayfasını yeniden görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Öğretici: Azure genel IP adresine başvurmak için diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Traffic Manager ile tepe etki alanı adlarını desteklemek için diğer ad kaydı yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)

Etkin bir DNS adını nasıl geçirteceklerini öğrenmek [için](../app-service/manage-custom-dns-migrate-domain.md)bkz.
