---
title: Coğrafi dağıtılmış ölçek
description: Trafik Yöneticisi ve Uygulama Hizmeti Ortamları ile coğrafi dağıtımı kullanarak uygulamaları yatay olarak nasıl ölçteğiz öğrenin.
author: stefsch
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 7ab04e23b838f2dfd39b73476db7492947d62e6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688807"
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>App Service Ortamları ile Coğrafi Olarak Dağıtılmış Ölçek
## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Çok yüksek ölçek gerektiren uygulama senaryoları, bir uygulamanın tek bir dağıtımı için kullanılabilen bilgi işlem kaynağı kapasitesini aşabilir.  Oylama uygulamaları, spor etkinlikleri ve televizyonda yayınlanan eğlence etkinlikleri, son derece yüksek ölçekli senaryoların örnekleridir. Yüksek ölçekli gereksinimler, aşırı yük gereksinimlerini karşılamak için tek bir bölgede ve bölgeler arasında birden fazla uygulama dağıtımı yapılırken, uygulamaları yatay olarak ölçeklendirerek karşılanabilir.

Uygulama Hizmet Ortamları yatay ölçeklendirme için ideal bir platformdur.  Bilinen bir istek oranını desteklenebilen bir Uygulama Hizmet Ortamı yapılandırması seçildikten sonra, geliştiriciler istenen en yüksek yük kapasitesine ulaşmak için ek Uygulama Hizmet Ortamlarını "çerez kesici" şekilde dağıtabilir.

Örneğin, Bir Uygulama Hizmeti Ortamı yapılandırması üzerinde çalışan bir uygulamanın saniyede 20K isteklerini (RPS) işlemek için test edildiğini varsayalım.  İstenilen en yüksek yük kapasitesi 100K RPS ise, uygulamanın öngörülen maksimum yükü kaldırabilmesi için beş (5) Uygulama Hizmeti Ortamı oluşturulabilir ve yapılandırılabilir.

Müşteriler genellikle özel (veya makyaj) etki alanı kullanarak uygulamalara erişebildiğimden, geliştiricilerin uygulama isteklerini tüm Uygulama Hizmeti Ortamı örneklerine dağıtmanın bir yolunu kullanması gerekir.  Bunu başarmanın en iyi yolu, azure [trafik yöneticisi profilini][AzureTrafficManagerProfile]kullanarak özel etki alanını çözmektir.  Trafik Yöneticisi profili, tüm tek tek Uygulama Hizmeti Ortamlarını işaret etmek üzere yapılandırılabilir.  Trafik Yöneticisi, Trafik Yöneticisi profilindeki yük dengeleme ayarlarını temel alınarak tüm App Service Ortamlarında müşterileri otomatik olarak dağıtarak yönetir.  Bu yaklaşım, tüm Uygulama Hizmeti Ortamlarının tek bir Azure bölgesinde mi yoksa birden çok Azure bölgesinde dünya çapında dağıtılıp dağıtılmadığına bakılmaksızın çalışır.

Ayrıca, müşteriler uygulamalara makyaj alanı üzerinden eriştiklerinden, müşteriler bir uygulamayı çalıştıran Uygulama Hizmeti Ortamlarının sayısından habersizdir.  Sonuç olarak, geliştiriciler, gözlenen trafik yüküne bağlı olarak App Service Ortamlarını hızlı ve kolay bir şekilde ekleyebilir ve kaldırabilir.

Aşağıdaki kavramsal diyagram, tek bir bölgedeki üç Uygulama Hizmeti Ortamı arasında yatay olarak ölçeklenmiş bir uygulamayı göstermektedir.

![Kavramsal Mimari][ConceptualArchitecture] 

Bu konunun geri kalanı, birden çok Uygulama Hizmeti Ortamını kullanarak örnek uygulama için dağıtılmış bir topoloji oluşturmayla ilgili adımlardan geçer.

## <a name="planning-the-topology"></a>Topolojinin Planlanması
Dağıtılmış bir uygulama ayak izini oluşturmadan önce, birkaç parça bilginin önceden olmasına yardımcı olur.

* **Uygulama için özel etki alanı:**  Müşterilerin uygulamaya erişmek için kullanacağı özel alan adı nedir?  Örnek uygulama için özel alan adı`www.scalableasedemo.com`
* **Trafik Yöneticisi etki alanı:**  [Azure Trafik Yöneticisi profili][AzureTrafficManagerProfile]oluşturulurken bir etki alanı adı nın seçilmesi gerekir.  Bu ad, Trafik Yöneticisi tarafından yönetilen bir etki alanı girişini kaydetmek için *trafficmanager.net* sonekile birleştirilir.  Örnek uygulama için seçilen ad *ölçeklenebilir-ase-demo'dur.*  Sonuç olarak Trafik Yöneticisi tarafından yönetilen tam etki alanı adı *scalable-ase-demo.trafficmanager.net.*
* **Uygulama ayak izini ölçekleme stratejisi:**  Uygulama ayak izi tek bir bölgedeki birden çok Uygulama Hizmeti Ortamına dağıtılacak mı?  Birden fazla bölge mi?  Her iki yaklaşımın karışımı ve eşleşmesi mi?  Karar, müşteri trafiğinin nereden kaynaklanacağına ilişkin beklentilerin yanı sıra bir uygulamanın destekleyici arka uç altyapısının geri kalanının ne kadar ölçeklendirebileceğine bağlı olmalıdır.  Örneğin, %100 bağımsız bir uygulamayla, bir uygulama Azure bölgesi başına birden çok Uygulama Hizmeti Ortamının birleşimi kullanılarak büyük ölçüde ölçeklendirilebilir ve birden çok Azure bölgesinde dağıtılan Uygulama Hizmeti Ortamları ile çarpılabilir.  Aralarından seçim yapabileceğiniz 15'ten fazla genel Azure bölgesiyle, müşteriler gerçekten dünya çapında hiper ölçekli bir uygulama ayak izi oluşturabilir.  Bu makalede kullanılan örnek uygulama için, tek bir Azure bölgesinde (Güney Orta ABD) üç Uygulama Hizmeti Ortamı oluşturuldu.
* **Uygulama Hizmet Ortamları için adlandırma kuralı:**  Her Uygulama Hizmet Ortamı benzersiz bir ad gerektirir.  Bir veya iki Uygulama Hizmeti Ortamının ötesinde, her Bir Uygulama Hizmeti Ortamını belirlemeye yardımcı olacak bir adlandırma kuralına sahip olmak yararlıdır.  Örnek uygulama için basit bir adlandırma kuralı kullanılmıştır.  Üç App Servis Ortamları isimleri *fe1ase*, *fe2ase*, ve *fe3ase*vardır.
* **Uygulamalar için adlandırma kuralı:**  Uygulamanın birden çok örneği dağıtılacağı için, dağıtılan uygulamanın her örneği için bir ad gereklidir.  App Service Environments'ın az bilinen ancak çok kullanışlı özelliklerinden biri, aynı uygulama adının birden çok Uygulama Hizmeti Ortamlarında kullanılabilmesidir.  Her Uygulama Hizmeti Ortamının benzersiz bir etki alanı sonekine sahip olması nedeniyle, geliştiriciler her ortamda aynı uygulama adını yeniden kullanmayı seçebilir.  Örneğin, bir geliştiricinin aşağıdaki gibi adlandırılmış uygulamaları olabilir: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net,* *myapp.foo3.p.azurewebsites.net,* vb.  Örnek uygulama için her uygulama örneğinin de benzersiz bir adı vardır.  Kullanılan uygulama örneği adları *webfrontend1*, *webfrontend2*ve *webfrontend3*.

## <a name="setting-up-the-traffic-manager-profile"></a>Trafik Yöneticisi Profilini Ayarlama
Bir uygulamanın birden çok örneği birden çok Uygulama Hizmeti Ortamı'nda dağıtıldıktan sonra, tek tek uygulama örnekleri Trafik Yöneticisi'ne kaydedilebilir.  Örnek uygulama için, müşterileri aşağıdaki dağıtılan uygulama örneklerinden herhangi biri için yönlendirebilen *scalable-ase-demo.trafficmanager.net* için Trafik Yöneticisi profili gereklidir:

* **webfrontend1.fe1ase.p.azurewebsites.net:**  İlk Uygulama Hizmet Ortamı'nda dağıtılan örnek uygulamanın bir örneği.
* **webfrontend2.fe2ase.p.azurewebsites.net:**  İkinci App Service Environment'da dağıtılan örnek uygulamanın bir örneği.
* **webfrontend3.fe3ase.p.azurewebsites.net:**  Üçüncü App Service Environment'da dağıtılan örnek uygulamanın bir örneği.

Hepsi **aynı** Azure bölgesinde çalışan birden çok Azure Uygulama Hizmeti uç noktasını kaydetmenin en kolay yolu Powershell [Azure Kaynak Yöneticisi Trafik Yöneticisi desteğidir.][ARMTrafficManager]  

İlk adım, bir Azure Trafik Yöneticisi profili oluşturmaktır.  Aşağıdaki kod, profilin örnek uygulama için nasıl oluşturulduğunu gösterir:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

*RelativeDnsName* parametresinin *ölçeklenebilir ase-demo*olarak nasıl ayarlandığını fark edin.  Alan adı *scalable-ase-demo.trafficmanager.net* nasıl oluşturulur ve trafik yöneticisi profiliyle ilişkilendirilir.

*TrafficRoutingMethod* parametresi, trafik yöneticisinin müşteri yükünü niçin kullanılabilir uç noktalara yayacağını belirlemek için kullanacağı yük dengeleme ilkesini tanımlar.  Bu örnekte *Ağırlıklı* yöntem seçilmiştir.  Bu, müşteri isteklerinin her bitiş noktasıyla ilişkili göreli ağırlıkları temel alınarak tüm kayıtlı uygulama uç noktalarına yayılmasına neden olur. 

Oluşturulan profille, her uygulama örneği ana Azure bitiş noktası olarak profile eklenir.  Aşağıdaki kod, her ön uç web uygulamasına bir başvuru getirir ve ardından her uygulamayı *TargetResourceId* parametresi ile Trafik Yöneticisi bitiş noktası olarak ekler.

    $webapp1 = Get-AzWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Her bir uygulama örneği için *Add-AzureTrafficManagerEndpointConfig'e* nasıl bir çağrı olduğuna dikkat edin.  Her Powershell komutundaki *TargetResourceId* parametresi, dağıtılan üç uygulama örneğinden birine başvurur.  Trafik Yöneticisi profili, profilinde kayıtlı olan üç uç noktaya da yük yayacaktır.

Üç uç *noktanın* tümü ağırlık parametresi için aynı değeri (10) kullanır.  Bu, Trafik Yöneticisi'nin müşteri isteklerini üç uygulama örneğine de nispeten eşit olarak yayarak sonuçlanmasına neden olabilir. 

## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Uygulamanın Özel Etki Alanını Trafik Yöneticisi Etki Alanına İşaret Etmek
Gerekli son adım, uygulamanın özel etki alanını Trafik Yöneticisi etki alanına işaret etmektir.  Örnek uygulama için `www.scalableasedemo.com` bu, `scalable-ase-demo.trafficmanager.net`''yi işaret etmek anlamına gelir.  Bu adımın özel etki alanını yöneten etki alanı kayıt şirketiyle tamamlanması gerekir.  

Kayıt şirketinizin etki alanı yönetim araçlarını kullanarak, Trafik Yöneticisi etki alanında özel etki alanını işaret eden bir CNAME kaydı oluşturulması gerekir.  Aşağıdaki resimde bu CNAME yapılandırması nasıl göründüğünü gösteren bir örnek:

![Özel Etki Alanı için CNAME][CNAMEforCustomDomain] 

Bu konu kapsamında olmasa da, her bir uygulama örneğinin özel etki alanının da bu uygulamada kayıtlı olması gerektiğini unutmayın.  Aksi takdirde, bir istek bir uygulama örneğine uymazsa ve uygulama da uygulamaya kayıtlı özel etki alanı yoksa, istek başarısız olur.  

Bu örnekte özel `www.scalableasedemo.com`etki alanı ve her uygulama örneği ile ilişkili özel etki alanı vardır.

![Özel Etki Alanı][CustomDomain] 

Azure Uygulama Hizmeti uygulamalarına özel bir etki alanı kaydetmenin özeti için, özel etki alanlarını kaydetme yle ilgili aşağıdaki [makaleye][RegisterCustomDomain]bakın.

## <a name="trying-out-the-distributed-topology"></a>Dağıtılmış Topolojiyi Deneme
Trafik Yöneticisi ve DNS yapılandırmasının sonucu, `www.scalableasedemo.com` isteklerin aşağıdaki sırayla akmasıdır:

1. Bir tarayıcı veya cihaz, DNS araması yapacaktır`www.scalableasedemo.com`
2. Etki alanı kayıt şirketindeki CNAME girişi, DNS aramasının Azure Trafik Yöneticisi'ne yönlendirilmesine neden olur.
3. Azure Trafik Yöneticisi DNS sunucularından birine karşı *scalable-ase-demo.trafficmanager.net* için Bir DNS araması yapılır.
4. Yük dengeleme ilkesine (Trafik Yöneticisi profilini oluştururken daha önce kullanılan *TrafficRoutingMethod* parametresi) bağlı olarak, Trafik Yöneticisi yapılandırılan uç noktalardan birini seçer ve bu bitiş noktasının FQDN'sini tarayıcıya veya cihaza döndürecektir.
5. Bitiş noktasının FQDN'si, Bir Uygulama Hizmet Ortamında çalışan bir uygulama örneğinin Url'si olduğundan, tarayıcı veya aygıt Microsoft Azure DNS sunucusundan FQDN'yi bir IP adresine çözmesini ister. 
6. Tarayıcı veya cihaz HTTP/S isteğini IP adresine gönderir.  
7. İstek, Uygulama Hizmet Ortamlarından birinde çalışan uygulama örneklerinden birine ulaşır.

Aşağıdaki konsol resmi, örnek uygulamanın özel etki alanını arayan bir DNS aramasını, üç örnek Uygulama Hizmeti Ortamından birinde (bu durumda üç App Service Ortamı'ndan ikincisi) çalışan bir uygulama örneğine başarıyla çözümünü gösterir:

![DNS Arama][DNSLookup] 

## <a name="additional-links-and-information"></a>Ek Linkler ve Bilgiler
Powershell Azure [Kaynak Yöneticisi Trafik Yöneticisi desteğindeki][ARMTrafficManager]belgeler.  

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: ../app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
