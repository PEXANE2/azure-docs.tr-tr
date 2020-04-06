---
title: Sorun giderme etki alanı ve TLS/SSL sertifikaları
description: Azure Uygulama Hizmeti'nde bir etki alanı veya TLS/SSL sertifikası yapılandırDığınızda karşılaşabileceğiniz sık karşılaşılan sorunlara çözüm bulun.
author: genlin
manager: dcscontentpm
tags: top-support-issue
ms.topic: article
ms.date: 03/01/2019
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: d61b95c7136a4cbce11789a58d27cc1a164ae374
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668019"
---
# <a name="troubleshoot-domain-and-tlsssl-certificate-problems-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde etki alanı ve TLS/SSL sertifika sorunlarını giderme

Bu makalede, Azure Uygulama Hizmeti'ndeki web uygulamalarınız için bir etki alanı veya TLS/SSL sertifikası yapılandırDığınızda karşılaşabileceğiniz sık karşılaşılan sorunlar listelanmaktadır. Ayrıca, bu sorunların olası nedenlerini ve çözümlerini de açıklar.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="certificate-problems"></a>Sertifika sorunları

### <a name="you-cant-add-a-tlsssl-certificate-binding-to-an-app"></a>Bir uygulamaya TLS/SSL sertifikası bağlama ekemezsiniz 

#### <a name="symptom"></a>Belirti

TLS bağlama eklediğinizde, aşağıdaki hata iletisini alırsınız:

"SSL bağlama eklemek için başarısız oldu. Başka bir VIP zaten bu sertifikayı kullandığından, mevcut VIP için sertifika ayarlayamıyor."

#### <a name="cause"></a>Nedeni

Bu sorun, birden çok uygulamada aynı IP adresi için birden çok IP tabanlı SSL bağlamanız varsa oluşabilir. Örneğin, A uygulamasının eski bir sertifikası olan IP tabanlı bir SSL'si vardır. App B aynı IP adresi için yeni bir sertifika ile IP tabanlı bir SSL vardır. Uygulama TLS bağlamayı yeni sertifikayla güncellediğinizde, aynı IP adresi başka bir uygulama için kullanıldığından bu hatayla başarısız olur. 

#### <a name="solution"></a>Çözüm 

Bu sorunu gidermek için aşağıdaki yöntemlerden birini kullanın:

- Eski sertifikayı kullanan uygulamadaki IP tabanlı SSL bağlamayı silin. 
- Yeni sertifikayı kullanan yeni bir IP tabanlı SSL bağlayıcısı oluşturun.

### <a name="you-cant-delete-a-certificate"></a>Bir sertifikayı silemezsiniz 

#### <a name="symptom"></a>Belirti

Bir sertifikayı silmeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

"Şu anda TLS/SSL bağlamada kullanıldığından sertifika silinemiyor. Sertifikayı silmeden önce TLS bağlaması kaldırılmalıdır."

#### <a name="cause"></a>Nedeni

Başka bir uygulama sertifikayı kullanırsa bu sorun oluşabilir.

#### <a name="solution"></a>Çözüm

Bu sertifika için TLS bağlamayı uygulamalardan kaldırın. Ardından sertifikayı silmeyi deneyin. Sertifikayı hala silemiyorsanız, internet tarayıcıönbelleğini temizleyin ve Azure portalını yeni bir tarayıcı penceresinde yeniden açın. Ardından sertifikayı silmeyi deneyin.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Uygulama Hizmeti sertifikası satın alamazsınız 

#### <a name="symptom"></a>Belirti
Azure portalından [Azure Uygulama Hizmeti sertifikası](./configure-ssl-certificate.md#import-an-app-service-certificate) satın alamazsınız.

#### <a name="cause-and-solution"></a>Neden ve çözüm
Bu sorun aşağıdaki nedenlerden herhangi biri için oluşabilir:

- Uygulama Hizmeti planı Ücretsiz veya Paylaşılan. Bu fiyatlandırma katmanları TLS'yi desteklemez. 

    **Çözüm**: Uygulama için Uygulama Hizmeti planını Standart'a yükseltin.

- Aboneliğin geçerli bir kredi kartı yoktur.

    **Çözüm**: Aboneliğinize geçerli bir kredi kartı ekleyin. 

- Abonelik teklifi, Microsoft Student gibi bir Uygulama Hizmeti sertifikası satın alma desteklemez.  

    **Çözüm**: Aboneliğinizi yükseltin. 

- Abonelik, abonelikte izin verilen satın alma sınırına ulaştı.

    **Çözüm**: Uygulama Hizmeti sertifikaları, Pay-As-You-Go ve EA abonelik türleri için 10 sertifika satın alma sınırına sahiptir. Diğer abonelik türleri için sınır 3'tür. Sınırı artırmak için [Azure desteğine](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvurun.
- App Service sertifikası sahtekarlık olarak işaretlenmişti. Aşağıdaki hata iletisini aldınız: "Sertifikanız olası bir sahtekarlık için işaretlendi. İstek şu anda inceleme altında. Sertifika 24 saat içinde kullanılabilir hale gelmezse Azure Desteği'ne başvurun."

    **Çözüm**: Sertifika sahtekarlık olarak işaretlenmişse ve 24 saat sonra çözülmezse aşağıdaki adımları izleyin:

    1. [Azure portalında](https://portal.azure.com)oturum açın.
    2. Uygulama **Hizmeti Sertifikaları'na**gidin ve sertifikayı seçin.
    3. **Sertifika Yapılandırma** > Adım 2'yi seçin:**Etki Alanı Doğrulamasını****Doğrulayın.** >  Bu adım, sorunu çözmek için Azure sertifika sağlayıcısına bir e-posta bildirimi gönderir.

## <a name="custom-domain-problems"></a>Özel etki alanı sorunları

### <a name="a-custom-domain-returns-a-404-error"></a>Özel etki alanı 404 hata döndürür 

#### <a name="symptom"></a>Belirti

Özel alan adını kullanarak siteye göz attığınızda aşağıdaki hata iletisini alırsınız:

"Hata 404-Web uygulaması bulunamadı."

#### <a name="cause-and-solution"></a>Neden ve çözüm

**Neden 1** 

Yapılandırdığınız özel etki alanı bir CNAME veya A kaydı eksik. 

**Neden 1 için çözüm**

- Bir A kaydı eklediyseniz, bir TXT kaydının da eklenmiştir. Daha fazla bilgi için Bkz. [A kaydı oluştur.](./app-service-web-tutorial-custom-domain.md#create-the-a-record)
- Uygulamanızın kök etki alanını kullanmak zorunda değilseniz, A kaydı yerine CNAME kaydı kullanmanızı öneririz.
- Aynı etki alanı için hem CNAME kaydı hem de A kaydı kullanmayın. Bu sorun bir çakışma neden olabilir ve etki alanının çözülmesini engelleyebilir. 

**Neden 2** 

İnternet tarayıcısı etki alanınızın eski IP adresini önbelleğe almaya devam ediyor olabilir. 

**Neden 2 için Çözüm**

Tarayıcıyı temizleyin. Windows aygıtları için komutu `ipconfig /flushdns`çalıştırabilirsiniz. Etki alanınızın uygulamanızın uygulamanın IP adresini işaret ettiğini doğrulamak için [WhatsmyDNS.net](https://www.whatsmydns.net/) kullanın. 

### <a name="you-cant-add-a-subdomain"></a>Bir alt etki alanı ekemezsiniz 

#### <a name="symptom"></a>Belirti

Bir alt etki alanı atamak için uygulamaya yeni bir ana bilgisayar adı ekemezsiniz.

#### <a name="solution"></a>Çözüm

- Uygulamaya ana bilgisayar adı eklemek için izinlere sahip olduğundan emin olmak için abonelik yöneticisine danışın.
- Daha fazla alt etki alanına ihtiyacınız varsa, etki alanı barındırma alanını Azure Alan Adı Hizmeti (DNS) olarak değiştirmenizi öneririz. Azure DNS'yi kullanarak uygulamanıza 500 ana bilgisayar adı ekleyebilirsiniz. Daha fazla bilgi için [bkz.](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/)

### <a name="dns-cant-be-resolved"></a>DNS çözülemez

#### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini aldınız:

"DNS kaydı bulunamadı."

#### <a name="cause"></a>Nedeni
Bu sorun aşağıdaki nedenlerden biri için oluşur:

- Yaşama süresi (TTL) süresi dolmadı. TTL değerini belirlemek için etki alanınızın DNS yapılandırmasını denetleyin ve ardından sürenin dolmasını bekleyin.
- DNS yapılandırması yanlış.

#### <a name="solution"></a>Çözüm
- Bu sorunun kendi kendine çözülmesi için 48 saat bekleyin.
- DNS yapılandırmanızdaki TTL ayarını değiştirebiliyorsanız, bunun sorunu çözüp çözmediğini görmek için değeri 5 dakikaya değiştirin.
- Etki alanınızın uygulamanızın uygulamanın IP adresini işaret ettiğini doğrulamak için [WhatsmyDNS.net](https://www.whatsmydns.net/) kullanın. Değilse, A kaydını uygulamanın doğru IP adresine yapılandırın.

### <a name="you-need-to-restore-a-deleted-domain"></a>Silinen bir etki alanını geri yüklemeniz gerekir 

#### <a name="symptom"></a>Belirti
Etki alanınız artık Azure portalında görünmüyor.

#### <a name="cause"></a>Nedeni 
Aboneliğin sahibi etki alanını yanlışlıkla silmiş olabilir.

#### <a name="solution"></a>Çözüm
Etki alanınız yedi günden kısa bir süre önce silinmişse, etki alanı silme işlemini henüz başlatmamıştır. Bu durumda, aynı etki alanını aynı abonelik altında Azure portalında yeniden satın alabilirsiniz. (Arama kutusuna tam alan adını yazdığından emin olun.) Bu etki alanı için tekrar ücretlendirilmezsiniz. Etki alanı yedi günden uzun bir süre önce silinmişse, etki alanını geri alma konusunda yardım için [Azure desteğine](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) başvurun.

## <a name="domain-problems"></a>Etki alanı sorunları

### <a name="you-purchased-a-tlsssl-certificate-for-the-wrong-domain"></a>Yanlış etki alanı için bir TLS/SSL sertifikası satın aldınız

#### <a name="symptom"></a>Belirti

Yanlış etki alanı için bir Uygulama Hizmeti sertifikası satın aldınız. Doğru etki alanını kullanmak için sertifikayı güncelleştiremezsiniz.

#### <a name="solution"></a>Çözüm

Bu sertifikayı silin ve ardından yeni bir sertifika satın alın.

Yanlış etki alanını kullanan geçerli sertifika "Verilen" durumdaysa, bu sertifika için de faturalandırılırsınız. Uygulama Hizmeti sertifikaları iade edilmez, ancak başka seçenekler olup olmadığını görmek için [Azure desteğine](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) başvurabilirsiniz. 

### <a name="an-app-service-certificate-was-renewed-but-the-app-shows-the-old-certificate"></a>Uygulama Hizmeti sertifikası yenilendi, ancak uygulama eski sertifikayı gösterir 

#### <a name="symptom"></a>Belirti

Uygulama Hizmeti sertifikası yenilendi, ancak App Service sertifikasını kullanan uygulama hala eski sertifikayı kullanıyor. Ayrıca, HTTPS protokolünün gerekli olduğuna dair bir uyarı aldınız.

#### <a name="cause"></a>Nedeni 
Uygulama Hizmeti sertifikanızı 48 saat içinde otomatik olarak senkronize eder. Bir sertifikayı döndürdüğünüzde veya güncellediğinizde, bazen uygulama yeni güncelleştirilen sertifikayı değil, eski sertifikayı hala alıyor. Bunun nedeni, sertifika kaynağını eşitleme işinin henüz çalışmamış olmasıdır. Eşitle'yi tıklatın. Eşitleme işlemi, uygulamanızda herhangi bir kapalı kalma süresine neden olmadan App Hizmeti'ndeki sertifikanın ana bilgisayar adı bağlamalarını otomatik olarak güncelleştirir.
 
#### <a name="solution"></a>Çözüm

Sertifikanın eşitlemi zorlayabilir:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Uygulama Hizmeti Sertifikaları'nı**seçin ve ardından sertifikayı seçin.
2. **Rekey ve Eşitle'yi**seçin ve ardından **Eşitle'yi**seçin. Eşitlemenin tamamlanması biraz zaman alır. 
3. Eşitleme tamamlandığında, aşağıdaki bildirimi görürsünüz: "Tüm kaynakları en son sertifikayla başarıyla güncelleştirin."

### <a name="domain-verification-is-not-working"></a>Etki alanı doğrulaması çalışmıyor 

#### <a name="symptom"></a>Belirti 
Uygulama Hizmeti sertifikası, sertifikanın kullanıma hazır olması için alan adı doğrulaması gerektirir. **Doğrula'yı**seçtiğinizde, işlem başarısız olur.

#### <a name="solution"></a>Çözüm
Bir TXT kaydı ekleyerek etki alanınızı el ile doğrulayın:
 
1.  Etki alanı adınızı barındıran Alan Adı Hizmeti (DNS) sağlayıcısına gidin.
2.  Azure portalında gösterilen etki alanı belirteci değerini kullanan etki alanınız için bir TXT kaydı ekleyin. 

DNS yayılmasının çalışması için birkaç dakika bekleyin ve doğrulamayı tetiklemek için **Yenile** düğmesini seçin. 

Alternatif olarak, etki alanınızı el ile doğrulamak için HTML web sayfası yöntemini kullanabilirsiniz. Bu yöntem, sertifika yetkilisinin sertifika için verilen etki alanının etki alanı sahipliğini onaylamasına olanak tanır.

1.  {etki alanı doğrulama belirteci}.html adlı bir HTML dosyası oluşturun. Bu dosyanın içeriği etki alanı doğrulama belirteci değeri olmalıdır.
3.  Bu dosyayı etki alanınızı barındıran web sunucusunun köküne yükleyin.
4.  Sertifika durumunu denetlemek için **Yenile'yi** seçin. Doğrulamanın tamamlanması birkaç dakika sürebilir.

Örneğin, etki alanı doğrulama belirteci 1234abcd ile azure.com için standart bir sertifika https://azure.com/1234abcd.html satın alıyorsanız, 1234abcd döndürmek için yapılan bir web isteği gerekir. 

> [!IMPORTANT]
> Sertifika siparişinin etki alanı doğrulama işlemini tamamlamak için yalnızca 15 günü vardır. 15 gün sonra, sertifika yetkilisi sertifikayı reddeder ve sertifika için ücret lendirilmezsiniz. Bu durumda, bu sertifikayı silin ve yeniden deneyin.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Etki alanı satın alamazsınız

#### <a name="symptom"></a>Belirti
Azure portalında bir Uygulama Hizmeti etki alanı satın alamazsınız.

#### <a name="cause-and-solution"></a>Neden ve çözüm

Bu sorun aşağıdaki nedenlerden biri için oluşur:

- Azure aboneliğinde kredi kartı yoktur veya kredi kartı geçersizdir.

    **Çözüm**: Aboneliğinize geçerli bir kredi kartı ekleyin.

- Abonelik sahibi değilsiniz, dolayısıyla etki alanı satın alma izniniz yok.

    **Çözüm**: Sahibi ni hesabınıza [atayın.](../role-based-access-control/role-assignments-portal.md) Veya bir etki alanı satın almak için izin almak için abonelik yöneticisine başvurun.
- Aboneliğinizde etki alanı satın alma sınırına ulaştınız. Geçerli sınır 20 etki alanıdır.

    **Çözüm**: Sınıra bir artış istemek için [Azure desteğine](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvurun.
- Azure aboneliğinizin türü App Service etki alanı satın almayı desteklemiyor.

    **Çözüm**: Azure aboneliğinizi, Olabildiğince Öde aboneliği gibi başka bir abonelik türüne yükseltin.

### <a name="you-cant-add-a-host-name-to-an-app"></a>Bir uygulamaya ana bilgisayar adı ekemezsiniz 

#### <a name="symptom"></a>Belirti

Bir ana bilgisayar adı eklediğinizde, işlem etki alanını doğrulamak ve doğrulamak için başarısız olur.

#### <a name="cause"></a>Nedeni 

Bu sorun aşağıdaki nedenlerden biri için oluşur:

- Ana bilgisayar adı ekleme izniniz yok.

    **Çözüm**: Abonelik yöneticisinden ana bilgisayar adı eklemenize izin vermesini isteyin.
- Etki alanı sahipliğiniz doğrulanamadı.

    **Çözüm**: CNAME veya A kaydınızın doğru şekilde yapılandırıldığından doğrulayın. Özel bir etki alanını bir uygulamayla eşlemek için CNAME kaydı veya A kaydı oluşturun. Kök etki alanı kullanmak istiyorsanız, A ve TXT kayıtlarını kullanmanız gerekir:

    |Kayıt türü|Host|Nokta|
    |------|------|-----|
    |A|@|Bir uygulamanın IP adresi|
    |TXT|@|`<app-name>.azurewebsites.net`|
    |CNAME|www|`<app-name>.azurewebsites.net`|

## <a name="faq"></a>SSS

**Satın aldıktan sonra web sitem için özel etki alanımı yapılandırmam gerekiyor mu?**

Azure portalından bir etki alanı satın aldığınızda, Uygulama Hizmeti uygulaması otomatik olarak bu özel etki alanını kullanacak şekilde yapılandırılır. Herhangi bir ek adım atmak zorunda değilsiniz. Daha fazla bilgi için [Azure App Service Self Help:](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name) Channel9'a Özel Alan Adı Ekleyin'i izleyin.

**Bunun yerine bir Azure VM'ine işaret etmek için Azure portalından satın alınan bir etki alanını kullanabilir miyim?**

Evet, etki alanını vm'ye yönlendirebilirsiniz. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](../dns/dns-custom-domain.md).

**Etki alanım GoDaddy veya Azure DNS tarafından barındırılıyor mu?**

Uygulama Hizmeti Etki Alanları etki alanı kaydı için GoDaddy'yi ve etki alanlarını barındırmak için Azure DNS'yi kullanır. 

**Otomatik yenileme etkin var ama yine de e-posta yoluyla etki alanıiçin bir yenileme bildirimi aldı. Ne yapmalıyım?**

Otomatik yenileme etkinse, herhangi bir işlem yapmanız gerekmez. Bildirim e-postası, etki alanının süresinin dolmasına yakın olduğunu bildirmek ve otomatik yenileme etkin değilse el ile yenilemek için sağlanır.

**Etki alanıma ev sahipliği yapan Azure DNS için ücretlendirilir miyim?**

Etki alanı satın alma nın ilk maliyeti yalnızca etki alanı kaydı için geçerlidir. Kayıt maliyetine ek olarak, kullanımınıza bağlı olarak Azure DNS için tahakkuk eden ücretler vardır. Daha fazla bilgi için azure [DNS fiyatlandırması](https://azure.microsoft.com/pricing/details/dns/) için bkz.

**Etki alanımı daha önce Azure portalından satın aldım ve GoDaddy barındırmadan Azure DNS barındırmasına geçmek istiyorum. Bunu nasıl yapabilirim?**

Azure DNS barındırma'na geçiş yapmak zorunlu değildir. Azure DNS'ye geçiş yapmak istiyorsanız, Azure portalındaki etki alanı yönetimi deneyimi Azure DNS'ye geçmek için gereken adımlar hakkında bilgi sağlar. Etki alanı App Service aracılığıyla satın alındıysa, GoDaddy barındırmasından Azure DNS'ye geçiş nispeten sorunsuz bir yordamdır.

**Etki alanımı App Service Domain'den satın almak istiyorum, ancak etki alanımı Azure DNS yerine GoDaddy'de barındırabilir miyim?**

24 Temmuz 2017'den itibaren portaldan satın alınan Uygulama Hizmeti etki alanları Azure DNS'de barındırılır. Farklı bir barındırma sağlayıcısı kullanmayı tercih ederseniz, bir etki alanı barındırma çözümü elde etmek için kendi web sitesine gitmek gerekir.

**Etki alanım için gizlilik koruması için ödeme yapmam gerekiyor mu?**

Azure portalı üzerinden bir etki alanı satın aldığınızda, ek ücret ödemeden gizlilik eklemeyi seçebilirsiniz. Bu, Azure Uygulama Hizmeti aracılığıyla etki alanınızı satın almanın avantajlarından biridir.

**Artık etki alanımı istemediğime karar verirsem, paramı geri alabilir miyim?**

Bir etki alanı satın aldığınızda, etki alanını istemediğiniz ekibe karar verebileceğiniz beş günlük bir süre için ücretlendirilmezsiniz. Bu beş günlük süre içinde etki alanını istemediğiniz karar verirseniz, ücretlendirilmezsiniz. (.uk etki alanları bunun bir istisnasidir. Bir .uk etki alanı satın aldıysanız, hemen ücretlendirilirsiniz ve size para iadesi yapılamaz.)

**Etki alanını aboneliğimdeki başka bir Azure Uygulama Hizmeti uygulamasında kullanabilir miyim?**

Evet. Azure portalında Özel Etki Alanları ve TLS bıçağına erişdiğinizde, satın aldığınız etki alanlarını görürsünüz. Uygulamanızı bu etki alanlarından herhangi birini kullanacak şekilde yapılandırabilirsiniz.

**Bir etki alanını bir abonelikten başka bir aboneye aktarabilir miyim?**

[Taşı-AzResource](https://docs.microsoft.com/powershell/module/az.Resources/Move-azResource) PowerShell cmdlet'ini kullanarak etki alanını başka bir abonelik/kaynak grubuna taşıyabilirsiniz.

**Şu anda bir Azure Uygulama Hizmeti uygulamam yoksa özel etki alanımı nasıl yönetebilirim?**

Bir App Service Web App'iniz olmasa bile etki alanınızı yönetebilirsiniz. Etki alanı Sanal makine, Depolama vb. gibi Azure hizmetleri için kullanılabilir. Etki alanını App Service Web Apps için kullanmayı planlıyorsanız, etki alanını web uygulamanıza bağlamak için Ücretsiz Uygulama Hizmeti planında olmayan bir Web Uygulaması eklemeniz gerekir.

**Özel etki alanına sahip bir web uygulamasını başka bir aboneye veya App Service Environment v1'den V2'ye taşıyabilir miyim?**

Evet, web uygulamanızı abonelikler arasında taşıyabilirsiniz. Azure'da [kaynakların nasıl taşınır'daki](../azure-resource-manager/management/move-resource-group-and-subscription.md)kılavuzu izleyin. Web uygulamasını hareket ettirirken birkaç sınırlama vardır. Daha fazla bilgi için, [Uygulama Hizmeti kaynaklarını taşımak için Sınırlamalar'a](../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)bakın.

Web uygulamasını hareket ettirdikten sonra, özel etki alanları ayarındaki etki alanlarının ana bilgisayar adı bağlamaları aynı kalmalıdır. Ana bilgisayar ad bağlamalarını yapılandırmak için ek adım gerekmez.
