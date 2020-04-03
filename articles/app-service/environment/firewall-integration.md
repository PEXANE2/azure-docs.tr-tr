---
title: Giden trafiği kilitleme
description: Bir Uygulama Hizmeti ortamından giden trafiği güvence altına almak için Azure Güvenlik Duvarı ile nasıl entegre olacağınızı öğrenin.
author: ccompy
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.topic: article
ms.date: 03/31/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 3dadb57c6358623974de1a27e1601d99b28fee32
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80584325"
---
# <a name="locking-down-an-app-service-environment"></a>Uygulama Hizmet Ortamını Kilitleme

Uygulama Hizmet Ortamı (ASE), düzgün çalışması için erişmesi gereken bir dizi dış bağımlılık vardır. ASE, müşteri Azure Sanal Ağı'nda (VNet) yaşar. Müşteriler, VNet'lerinden tüm çıkışları kilitlemek isteyen müşteriler için bir sorun olan ASE bağımlılık trafiğine izin vermelidir.

Bir ASE'yi yönetmek için kullanılan gelen uç noktalar vardır. Gelen yönetim trafiği bir güvenlik duvarı aygıtı ndan gönderilemez. Bu trafiğin kaynak adresleri bilinir ve App [Service Environment yönetim adresleri](https://docs.microsoft.com/azure/app-service/environment/management-addresses) belgesinde yayımlanır. Gelen trafiği güvenli hale getirmek için Ağ Güvenlik Grupları (NSG'ler) ile kullanılabilen AppServiceManagement adlı bir Hizmet Etiketi de vardır.

ASE giden bağımlılıkları, arkalarında statik adresleri olmayan FQDN'lerle neredeyse tamamen tanımlanır. Statik adreslerin olmaması, Ağ Güvenlik Grupları'nın bir ASE'den giden trafiği kilitlemek için kullanılamayacağı anlamına gelir. Adresler, geçerli çözünürlüğe göre kurallar ayarlayamayacağı ve bunu NSG'ler oluşturmak için kullanamayacağı kadar sık değişir. 

Giden adresleri güvence altına almanın çözümü, alan adlarına göre giden trafiği denetlenebilen bir güvenlik duvarı aygıtının kullanılmasıdır. Azure Güvenlik Duvarı, giden HTTP ve HTTPS trafiğini hedefin FQDN'sini temel alınabiliyor.  

## <a name="system-architecture"></a>Sistem mimarisi

Güvenlik duvarı aygıtından geçen giden trafikle bir ASE dağıtmak, ASE alt ağı üzerindeki yolları değiştirmeyi gerektirir. Rotalar IP düzeyinde çalışır. Rotalarınızı tanımlarken dikkatli değilseniz, TCP yanıt trafiğini başka bir adresten kaynağa zorlayabilirsiniz. Yanıt adresiniz trafiğin gönderildiği adresten farklı olduğunda, soruna asimetrik yönlendirme denir ve TCP kırılır.

ASE'ye gelen trafiğin, trafiğin geldiği şekilde yanıtlanabilmesi için tanımlanmış rotalar olmalıdır. Gelen yönetim istekleri ve gelen uygulama istekleri için rotalar tanımlanmalıdır.

Bir ASE'ye gelen ve gelen trafik aşağıdaki kurallara uymak zorundadır.

* Azure SQL, Depolama ve Etkinlik Hub trafiği bir güvenlik duvarı aygıtı kullanımıyla desteklenmez. Bu trafik doğrudan bu hizmetlere gönderilmelidir. Bunu gerçekleştirmenin yolu, bu üç hizmet için hizmet bitiş noktalarını yapılandırmaktır. 
* Gelen yönetim trafiğini geldiği yerden geri gönderen rota tablosu kuralları tanımlanmalıdır.
* Gelen uygulama trafiğini geldiği yerden geri gönderen rota tablosu kuralları tanımlanmalıdır. 
* ASE'den ayrılan diğer tüm trafik, bir rota tablosu kuralıyla güvenlik duvarı aygıtınıza gönderilebilir.

![Azure Güvenlik Duvarı bağlantı akışı ile ASE][5]

## <a name="locking-down-inbound-management-traffic"></a>Gelen yönetim trafiğini kilitleme

ASE alt netinizin zaten atanmış bir NSG'si yoksa, bir tane oluşturun. NSG içinde, 454, 455 bağlantı noktalarında AppServiceManagement adlı Servis Etiketinden trafiğe izin vermek için ilk kuralı ayarlayın. AppServiceManagement etiketinden erişime izin veren kural, ASE'nizi yönetmek için kamuDAKI IP'lerden istenen tek şeydir. Bu Hizmet Etiketinin arkasındaki adresler yalnızca Azure Uygulama Hizmeti'ni yönetmek için kullanılır. Bu bağlantılar da akan yönetim trafiği şifrelenir ve kimlik doğrulama sertifikalarıyla güvenli hale gelir. Bu kanaldaki tipik trafik, müşteri tarafından başlatılan komutlar ve sistem durumu sondaları gibi şeyleri içerir. 

Portal üzerinden yeni bir alt ağ la yapılan ASE'ler, AppServiceManagement etiketi için izin kuralını içeren bir NSG ile yapılır.  

ASE'niz ayrıca 16001 bağlantı noktasındaki Yük Dengeleyici etiketinden gelen isteklere de izin vermelidir. 16001 portundaki Yük Dengeleyicisi'nden gelen talepler, Yük Dengeleyicisi ile ASE ön uçları arasındaki kontrolleri canlı tutar. Bağlantı noktası 16001 engellenirse, ASE'niz sağlıksız gider.

## <a name="configuring-azure-firewall-with-your-ase"></a>Azure Güvenlik Duvarını ASE'nizle yapılandırma 

Azure Güvenlik Duvarı ile mevcut ASE'nizden çıkışları kilitleme adımları şunlardır:

1. ASE alt abununızdaki SQL, Depolama ve Etkinlik Hub'ına hizmet uç noktalarını etkinleştirin. Hizmet uç noktalarını etkinleştirmek için ağ portalına > alt ağlarına gidin ve Hizmet uç noktaları açılır noktasından Microsoft.EventHub, Microsoft.SQL ve Microsoft.Storage'ı seçin. Azure SQL'de hizmet bitiş noktaları etkinleştirildiğinde, uygulamalarınızın sahip olduğu tüm Azure SQL bağımlılıklarının hizmet bitiş noktalarıyla da yapılandırılması gerekir. 

   ![hizmet bitiş noktalarını seçin][2]
  
1. ASE'nizin bulunduğu VNet'te AzureFirewallSubnet adında bir alt ağ oluşturun. Azure Güvenlik Duvarınızı oluşturmak için [Azure Güvenlik Duvarı belgelerindeki](https://docs.microsoft.com/azure/firewall/) yönergeleri izleyin.

1. Azure Güvenlik Duvarı Kullanıcı > Kuralları > Uygulama kural koleksiyonundan uygulama kuralı koleksiyonu ekle'yi seçin. Bir ad, öncelik sağlayın ve İzin Ver'i ayarlayın. FQDN etiketleri bölümünde, bir ad sağlayın, kaynak adreslerini * olarak ayarlayın ve Uygulama Hizmet Ortamı FQDN Etiketi ve Windows Güncelleştirmesi'ni seçin. 
   
   ![Uygulama kuralı ekleme][1]
   
1. Ağ kural koleksiyonu > Azure Güvenlik Duvarı > Kuralları'ndan Ağ kuralı koleksiyonu ekle'yi seçin. Bir ad, öncelik sağlayın ve İzin Ver'i ayarlayın. IP adresleri altında Kurallar bölümünde, bir ad sağlayın, **Herhangi**bir ptocol seçin , Kaynak ve Hedef adresleri * ayarlayın ve 123 bağlantı noktaları ayarlayın. Bu kural, sistemin NTP kullanarak saat eşitleme gerçekleştirmesine olanak sağlar. Herhangi bir sistem sorunlarını eşleme yardımcı olmak için bağlantı noktası 12000 için aynı şekilde başka bir kural oluşturun. 

   ![NTP ağ kuralı ekle][3]
   
1. Ağ kural koleksiyonu > Azure Güvenlik Duvarı > Kuralları'ndan Ağ kuralı koleksiyonu ekle'yi seçin. Bir ad, öncelik sağlayın ve İzin Ver'i ayarlayın. Hizmet Etiketleri altındaki Kurallar bölümünde, bir ad sağlayın, Kaynak adreslerine * kümesi olan **Herhangi**bir protokolü seçin, AzureMonitor'un bir hizmet etiketini seçin ve bağlantı noktalarını 80,443 olarak ayarlayın. Bu kural, sistemin Azure Monitor'a sistem durumu ve ölçümleri bilgileri sağlamasına olanak tanır.

   ![NTP hizmet etiketi ağ kuralı ekle][6]
   
1. Bir sonraki Internet atlamaile [App Service Environment yönetim adreslerinden]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) yönetim adresleriyle bir rota tablosu oluşturun. Rota tablosu girişleri asimetrik yönlendirme sorunları önlemek için gereklidir. Bir sonraki Internet atlamasıyla IP adresi bağımlılıklarında aşağıda belirtilen IP adresi bağımlılıkları için rotalar ekleyin. Bir sonraki atlama Azure Güvenlik Duvarı özel IP adresiniz olmak üzere 0.0.0.0/0 için rota tablonuza bir Sanal Cihaz rotası ekleyin. 

   ![Rota tablosu oluşturma][4]
   
1. Oluşturduğunuz rota tablosunu ASE alt ağınıza atayın.

#### <a name="deploying-your-ase-behind-a-firewall"></a>ASE'nizi bir güvenlik duvarının arkasına dağıtma

ASE'nizi bir güvenlik duvarının arkasına dağıtma adımları, ase alt ağınızı oluşturmanız ve ardından önceki adımları izlemeniz gerekse de, mevcut ASE'nizi bir Azure Güvenlik Duvarı yla yapılandırmakla aynıdır. ASE'nizi önceden varolan bir alt ağda oluşturmak için, [ASE'nizi Kaynak Yöneticisi şablonuyla oluşturma](https://docs.microsoft.com/azure/app-service/environment/create-from-template)konusunda belgede açıklandığı gibi bir Kaynak Yöneticisi şablonu kullanmanız gerekir.

## <a name="application-traffic"></a>Uygulama trafiği 

Yukarıdaki adımlar, ASE'nizin sorunsuz çalışmasını sağlayacaktır. Yine de uygulama gereksinimlerinize uyacak şekilde şeyleri yapılandırmanız gerekir. Azure Güvenlik Duvarı ile yapılandırılan bir ASE'deki uygulamalar için iki sorun vardır.  

- Uygulama bağımlılıklarının Azure Güvenlik Duvarı'na veya rota tablosuna eklenmesi gerekir. 
- Asimetrik yönlendirme sorunlarını önlemek için uygulama trafiği için rotalar oluşturulmalıdır

Uygulamalarınız bağımlılıkları varsa, Azure Güvenlik Duvarınıza eklenmesi gerekir. Her şey için HTTP/HTTPS trafiğine ve Ağ kurallarına izin vermek için Uygulama kuralları oluşturun. 

Uygulama isteği trafiğinizin geleceği adres aralığını biliyorsanız, bunu ASE alt ağınıza atanan rota tablosuna ekleyebilirsiniz. Adres aralığı büyük veya belirtilmemişse, rota tablonuza eklemek için size bir adres vermek için Uygulama Ağ Geçidi gibi bir ağ cihazı kullanabilirsiniz. ILB ASE'niz ile Bir Uygulama Ağ Geçidi'ni yapılandırma hakkında daha fazla bilgi [için, ILB ASE'nizi Bir Uygulama Ağ Geçidi ile Tümleştirme'yi](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway) okuyun

Uygulama Ağ Geçidi'nin bu kullanımı, sisteminizi nasıl yapılandırılabildiğinize yalnızca bir örnektir. Bu yolu takip ettiyseniz, Uygulama Ağ Geçidi'ne gönderilen yanıt trafiğinin doğrudan oraya gitmesi için ASE alt net rota tablosuna bir rota eklemeniz gerekir. 

## <a name="logging"></a>Günlüğe Kaydetme 

Azure Güvenlik Duvarı günlükleri Azure Depolama, Etkinlik Hub'ı veya Azure Monitor günlüklerine gönderebilir. Uygulamanızı desteklenen herhangi bir hedefle bütünleştirmek için, Tanı Günlükleri > Azure Güvenlik Duvarı portalına gidin ve istediğiniz hedefin günlüklerini etkinleştirin. Azure Monitor günlükleriyle tümleşirseniz, Azure Güvenlik Duvarı'na gönderilen tüm trafik trafiğinin günlüğünü görebilirsiniz. Reddedilen trafiği görmek için Log Analytics çalışma alanı portalınızı > Günlükler'i açın ve 

    AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
 
Azure Güvenlik Duvarınızı Azure Monitor günlükleriyle tümleştirmek, tüm uygulama bağımlılıklarından haberdar olmadığınız bir uygulamayı ilk çalışırken kullanışlıdır. [Azure Monitor'daki günlük verilerini çözümleyerek](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)Azure Monitor günlükleri hakkında daha fazla bilgi edinebilirsiniz.
 
## <a name="dependencies"></a>Bağımlılıklar

Aşağıdaki bilgiler yalnızca Azure Güvenlik Duvarı dışında bir güvenlik duvarı cihazı yapılandırmak istiyorsanız gereklidir. 

- Hizmet Bitiş Noktası özellikli hizmetler, hizmet bitiş noktalarıyla yapılandırılmalıdır.
- IP Adresi bağımlılıkları HTTP/S olmayan trafik içindir (hem TCP hem de UDP trafiği)
- FQDN HTTP/HTTPS uç noktaları güvenlik duvarı aygıtınıza yerleştirilebilir.
- Joker KARAKTER HTTP/HTTPS bitiş noktaları, bir dizi niteleyiciye bağlı olarak ASE'nize göre değişiklik gösterebilen bağımlılıklardır. 
- Linux uygulamaları yalnızca ASE'nize Linux uygulamaları dağıtıyorsanız, linux bağımlılıkları endişe vericidir. Linux uygulamalarını ASE'nize dağıtmıyorsanız, bu adreslerin güvenlik duvarınıza eklenmesi gerekmez. 

#### <a name="service-endpoint-capable-dependencies"></a>Hizmet Bitiş Noktası özellikli bağımlılıklar 

| Uç Nokta |
|----------|
| Azure SQL |
| Azure Storage |
| Azure Event Hub |

#### <a name="ip-address-dependencies"></a>IP Adresi bağımlılıkları

| Uç Nokta | Ayrıntılar |
|----------| ----- |
| \*:123 | NTP saat kontrolü. Trafik, 123 bağlantı noktasındabirden fazla uç noktada denetlenir |
| \*:12000 | Bu bağlantı noktası bazı sistem izleme için kullanılır. Eğer engellenirse, bazı sorunların triyajı daha zor olacaktır ancak ASE'niz çalışmaya devam edecektir |
| 40.77.24.27:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 40.77.24.27:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.90.249.229:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.90.249.229:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 104.45.230.69:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 104.45.230.69:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.82.184.151:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.82.184.151:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |

Azure Güvenlik Duvarı ile, fqdn etiketleri ile yapılandırılan altındaki her şeyi otomatik olarak alırsınız. 

#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları 

| Uç Nokta |
|----------|
|graph.microsoft.com:443 |
|login.live.com:443 |
|login.windows.com:443 |
|login.windows.net:443 |
|login.microsoftonline.com:443 |
|client.wns.windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|ocsp.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|crl.microsoft.com:443 |
|crl.microsoft.com:80 |
|www.thawte.com:443 |
|crl3.digicert.com:80 |
|ocsp.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|crl.verisign.com:80 |
|ocsp.verisign.com:80 |
|cacerts.digicert.com:80 |
|azperfcounters1.blob.core.windows.net:443 |
|azurewatsonanalysis-prod.core.windows.net:443 |
|global.metrics.nsatc.net:80 |
|global.metrics.nsatc.net:443 |
|az-prod.metrics.nsatc.net:443 |
|antares.metrics.nsatc.net:443 |
|azglobal-black.azglobal.metrics.nsatc.net:443 |
|azglobal-red.azglobal.metrics.nsatc.net:443 |
|antares-black.antares.metrics.nsatc.net:443 |
|antares-red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.core.windows.net:443 |
|clientconfig.passport.net:443 |
|packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|management.core.windows.net:443 |
|management.core.windows.net:80 |
|management.azure.com:443 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge.net:443 |
|validation-v2.sls.microsoft.com:443 |
|flighting.cp.wd.microsoft.com:443 |
|dmd.metaservices.microsoft.com:80 |
|admin.core.windows.net:443 |
|prod.warmpath.msftcloudes.com:443 |
|prod.warmpath.msftcloudes.com:80 |
|azureprofileruploads.blob.core.windows.net:443 |
|azureprofileruploads2.blob.core.windows.net:443 |
|azureprofileruploads3.blob.core.windows.net:443 |
|azureprofileruploads4.blob.core.windows.net:443 |
|azureprofileruploads5.blob.core.windows.net:443 |
|azperfmerges.blob.core.windows.net:443 |
|azprofileruploads1.blob.core.windows.net:443 |
|azprofileruploads10.blob.core.windows.net:443 |
|azprofileruploads2.blob.core.windows.net:443 |
|azprofileruploads3.blob.core.windows.net:443 |
|azprofileruploads4.blob.core.windows.net:443 |
|azprofileruploads6.blob.core.windows.net:443 |
|azprofileruploads7.blob.core.windows.net:443 |
|azprofileruploads8.blob.core.windows.net:443 | 
|azprofileruploads9.blob.core.windows.net:443 |
|azureprofilerfrontdoor.cloudapp.net:443 |
|settings-win.data.microsoft.com:443 |
|maupdateaccount2.blob.core.windows.net:443 |
|maupdateaccount3.blob.core.windows.net:443 |
|dc.services.visualstudio.com:443 |
|gmstorageprodsn1.blob.core.windows.net:443 |
|gmstorageprodsn1.file.core.windows.net:443 |
|gmstorageprodsn1.queue.core.windows.net:443 |
|gmstorageprodsn1.table.core.windows.net:443 |
|rteventservice.trafficmanager.net:443 |
|ctldl.windowsupdate.com:80 |
|ctldl.windowsupdate.com:443 |

#### <a name="wildcard-httphttps-dependencies"></a>Joker Karakter HTTP/HTTPS bağımlılıkları 

| Uç Nokta |
|----------|
|gr-Prod-\*.cloudapp.net:443 |
| \*.management.azure.com:443 |
| \*.update.microsoft.com:443 |
| \*.windowsupdate.microsoft.com:443 |
| \*.identity.azure.net:443 |
| \*.ctldl.windowsupdate.com:80 |
| \*.ctldl.windowsupdate.com:443 |

#### <a name="linux-dependencies"></a>Linux bağımlılıkları 

| Uç Nokta |
|----------|
|wawsinfraprodbay063.blob.core.windows.net:443 |
|registry-1.docker.io:443 |
|auth.docker.io:443 |
|production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|us.archive.ubuntu.com:80 |
|download.mono-project.com:80 |
|packages.treasuredata.com:80|
|security.ubuntu.com:80 |
| \*.cdn.mscr.io:443 |
|mcr.microsoft.com:443 |
|packages.fluentbit.io:80 |
|packages.fluentbit.io:443 |
|apt-mo.trafficmanager.net:80 |
|apt-mo.trafficmanager.net:443 |
|azure.archive.ubuntu.com:80 |
|azure.archive.ubuntu.com:443 |
|changelogs.ubuntu.com:80 |
|13.74.252.37:11371 |
|13.75.127.55:11371 |
|13.76.190.189:11371 |
|13.80.10.205:11371 |
|13.91.48.226:11371 |
|40.76.35.62:11371 |
|104.215.95.108:11371 |

## <a name="us-gov-dependencies"></a>ABD Gov bağımlılıkları

ABD Gov bölgelerindeki ASE'ler için, ASE'nizle bir Azure Güvenlik Duvarı yapılandırmak için bu belgenin [ASE bölümüyle Yapılandırma Azure Güvenlik Duvarını yapılandırılatan](https://docs.microsoft.com/azure/app-service/environment/firewall-integration#configuring-azure-firewall-with-your-ase) yönergeleri izleyin.

ABD Gov'da Azure Güvenlik Duvarı dışında bir aygıt kullanmak istiyorsanız 

* Hizmet Bitiş Noktası özellikli hizmetler, hizmet bitiş noktalarıyla yapılandırılmalıdır.
* FQDN HTTP/HTTPS uç noktaları güvenlik duvarı aygıtınıza yerleştirilebilir.
* Joker KARAKTER HTTP/HTTPS bitiş noktaları, bir dizi niteleyiciye bağlı olarak ASE'nize göre değişiklik gösterebilen bağımlılıklardır.

Linux, ABD Gov bölgelerinde kullanılamaz ve bu nedenle isteğe bağlı yapılandırma olarak listelenmez.

#### <a name="service-endpoint-capable-dependencies"></a>Hizmet Bitiş Noktası özellikli bağımlılıklar ####

| Uç Nokta |
|----------|
| Azure SQL |
| Azure Storage |
| Azure Event Hub |

#### <a name="ip-address-dependencies"></a>IP Adresi bağımlılıkları

| Uç Nokta | Ayrıntılar |
|----------| ----- |
| \*:123 | NTP saat kontrolü. Trafik, 123 bağlantı noktasındabirden fazla uç noktada denetlenir |
| \*:12000 | Bu bağlantı noktası bazı sistem izleme için kullanılır. Eğer engellenirse, bazı sorunların triyajı daha zor olacaktır ancak ASE'niz çalışmaya devam edecektir |
| 40.77.24.27:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 40.77.24.27:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.90.249.229:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.90.249.229:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 104.45.230.69:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 104.45.230.69:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.82.184.151:80 | ASE sorunlarını izlemek ve uyarmak için gerekli |
| 13.82.184.151:443 | ASE sorunlarını izlemek ve uyarmak için gerekli |

#### <a name="dependencies"></a>Bağımlılıklar ####

| Uç Nokta |
|----------|
| \*.ctldl.windowsupdate.com:80 |
| \*.management.usgovcloudapi.net:80 |
| \*.update.microsoft.com:80 |
|admin.core.usgovcloudapi.net:80 |
|azperfmerges.blob.core.windows.net:80 |
|azperfmerges.blob.core.windows.net:80 |
|azprofileruploads1.blob.core.windows.net:80 |
|azprofileruploads10.blob.core.windows.net:80 |
|azprofileruploads2.blob.core.windows.net:80 |
|azprofileruploads3.blob.core.windows.net:80 |
|azprofileruploads4.blob.core.windows.net:80 |
|azprofileruploads5.blob.core.windows.net:80 |
|azprofileruploads6.blob.core.windows.net:80 |
|azprofileruploads7.blob.core.windows.net:80 |
|azprofileruploads8.blob.core.windows.net:80 |
|azprofileruploads9.blob.core.windows.net:80 |
|azureprofilerfrontdoor.cloudapp.net:80 |
|azurewatsonanalysis.usgovcloudapp.net:80 |
|cacerts.digicert.com:80 |
|client.wns.windows.com:80 |
|crl.microsoft.com:80 |
|crl.verisign.com:80 |
|crl3.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|ctldl.windowsupdate.com:80 |
|definitionupdates.microsoft.com:80 |
|download.windowsupdate.com:80 |
|fairfax.warmpath.usgovcloudapi.net:80 |
|flighting.cp.wd.microsoft.com:80 |
|gcwsprodgmdm2billing.queue.core.usgovcloudapi.net:80 |
|gcwsprodgmdm2billing.table.core.usgovcloudapi.net:80 |
|global.metrics.nsatc.net:80 |
|go.microsoft.com:80 |
|gr-gcws-prod-bd3.usgovcloudapp.net:80 |
|gr-gcws-prod-bn1.usgovcloudapp.net:80 |
|gr-gcws-prod-dd3.usgovcloudapp.net:80 |
|gr-gcws-prod-dm2.usgovcloudapp.net:80 |
|gr-gcws-prod-phx20.usgovcloudapp.net:80 |
|gr-gcws-prod-sn5.usgovcloudapp.net:80 |
|login.live.com:80 |
|login.microsoftonline.us:80 |
|management.core.usgovcloudapi.net:80 |
|management.usgovcloudapi.net:80 |
|maupdateaccountff.blob.core.usgovcloudapi.net:80 |
|mscrl.microsoft.com
|ocsp.digicert.0 |
|ocsp.msocsp.co|
|ocsp.verisign.0 |
|rteventse.trafficmanager.net:80 |
|settings-n.data.microsoft.com:80 |
|shavamafestcdnprod1.azureedge.net:80 |
|shavanifestcdnprod1.azureedge.net:80 |
|v10ortex-win.data.microsoft.com:80 |
|wp.microsoft.com:80 |
|dcpalt.microsoft.com:80 |
|www.microsoft.com:80 |
|www.msftconnecttest.com:80 |
|www.thawte.com:80 |
|\*ctldl.windowsupdate.com:443 |
|\*.management.usgovcloudapi.net:443 |
|\*.update.microsoft.com:443 |
|admin.core.usgovcloudapi.net:443 |
|azperfmerges.blob.core.windows.net:443 |
|azperfmerges.blob.core.windows.net:443 |
|azprofileruploads1.blob.core.windows.net:443 |
|azprofileruploads10.blob.core.windows.net:443 |
|azprofileruploads2.blob.core.windows.net:443 |
|azprofileruploads3.blob.core.windows.net:443 |
|azprofileruploads4.blob.core.windows.net:443 |
|azprofileruploads5.blob.core.windows.net:443 |
|azprofileruploads6.blob.core.windows.net:443 |
|azprofileruploads7.blob.core.windows.net:443 |
|azprofileruploads8.blob.core.windows.net:443 |
|azprofileruploads9.blob.core.windows.net:443 |
|azureprofilerfrontdoor.cloudapp.net:443 |
|azurewatsonanalysis.usgovcloudapp.net:443 |
|cacerts.digicert.com:443 |
|client.wns.windows.com:443 |
|crl.microsoft.com:443 |
|crl.verisign.com:443 |
|crl3.digicert.com:443 |
|csc3-2009-2.crl.verisign.com:443 |
|ctldl.windowsupdate.com:443 |
|definitionupdates.microsoft.com:443 |
|download.windowsupdate.com:443 |
|fairfax.warmpath.usgovcloudapi.net:443 |
|flighting.cp.wd.microsoft.com:443 |
|gcwsprodgmdm2billing.queue.core.usgovcloudapi.net:443 |
|gcwsprodgmdm2billing.table.core.usgovcloudapi.net:443 |
|global.metrics.nsatc.net:443 |
|go.microsoft.com:443 |
|gr-gcws-prod-bd3.usgovcloudapp.net:443 |
|gr-gcws-prod-bn1.usgovcloudapp.net:443 |
|gr-gcws-prod-dd3.usgovcloudapp.net:443 |
|gr-gcws-prod-dm2.usgovcloudapp.net:443 |
|gr-gcws-prod-phx20.usgovcloudapp.net:443 |
|gr-gcws-prod-sn5.usgovcloudapp.net:443 |
|login.live.com:443 |
|login.microsoftonline.us:443 |
|management.core.usgovcloudapi.net:443 |
|management.usgovcloudapi.net:443 |
|maupdateaccountff.blob.core.usgovcloudapi.net:443 |
|mscrl.microsoft.com:443 |
|ocsp.digicert.com:443 |
|ocsp.msocsp.com:443 |
|ocsp.verisign.com:443 |
|rteventservice.trafficmanager.net:443 |
|settings-win.data.microsoft.com:443 |
|shavamanifestcdnprod1.azureedge.net:443 |
|shavamanifestcdnprod1.azureedge.net:443 |
|v10.vortex-win.data.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|www.microsoft.com:443 |
|www.msftconnecttest.com:443 |
|www.thawte.com:443 |

<!--Image references-->
[1]: ./media/firewall-integration/firewall-apprule.png
[2]: ./media/firewall-integration/firewall-serviceendpoints.png
[3]: ./media/firewall-integration/firewall-ntprule.png
[4]: ./media/firewall-integration/firewall-routetable.png
[5]: ./media/firewall-integration/firewall-topology.png
[6]: ./media/firewall-integration/firewall-ntprule-monitor.png
