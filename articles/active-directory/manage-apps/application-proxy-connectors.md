---
title: Azure AD Uygulama Proxy bağlayıcılarını anlama | Microsoft Dokümanlar
description: Azure AD Application Proxy bağlayıcıları hakkındaki temel bilgileri kapsar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: b097ce3781a77a8c5e8a94b9c2bf0977f3efcfd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481339"
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Azure AD Uygulama Proxy bağlayıcılarını anlama

Azure AD Application Proxy'yi mümkün kılan bağlayıcılardır. Basit, kurulumu ve bakımı kolay ve süper güçlüdürler. Bu makalede, bağlayıcıların ne olduğu, nasıl çalıştıkları ve dağıtımınızı en iyi duruma getirme konusunda bazı öneriler açıklanmaktadır.

## <a name="what-is-an-application-proxy-connector"></a>Uygulama Proxy bağlayıcısı nedir?

Konektörler, şirket içinde oturup Uygulama Proxy hizmetine giden bağlantıyı kolaylaştıran hafif aracılardır. Bağlayıcılar arka uç uygulamasına erişimi olan bir Windows Server'a yüklenmelidir. Bağlayıcıları, her grup trafiği belirli uygulamalarla işleterek bağlayıcı gruplar halinde düzenleyebilirsiniz.

## <a name="requirements-and-deployment"></a>Gereksinimler ve dağıtım

Uygulama Proxy'sini başarılı bir şekilde dağıtmak için en az bir bağlayıcıya ihtiyacınız vardır, ancak daha fazla esneklik için iki veya daha fazla öneriöneririz. Bağlayıcıyı Windows Server 2012 R2 veya sonraki bir makinede töşen. Bağlayıcının Uygulama Proxy hizmeti ve yayımladıkladığınız şirket içi uygulamalarla iletişim kurması gerekir.

### <a name="windows-server"></a>Windows sunucusu
Windows Server 2012 R2 çalıştıran bir sunucu ya da daha sonra uygulama proxy bağlayıcısı yükleyebilirsiniz gerekir. Sunucunun Azure'daki Uygulama Proxy hizmetlerine ve yayımladırdığınız şirket içi uygulamalara bağlanması gerekir.

Uygulama Proxy bağlayıcısını yüklemeden önce windows sunucusunun TLS 1.2 etkinleştirilmiş olması gerekir. SUNUCUDA TLS 1.2'yi etkinleştirmek için:

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Sunucuyu yeniden başlatın

Bağlayıcı sunucusunun ağ gereksinimleri hakkında daha fazla bilgi için Bkz. [Uygulama Proxy'si ile başlayın ve bir bağlayıcı yükleyin.](application-proxy-add-on-premises-application.md)

## <a name="maintenance"></a>Bakım

Konektörler ve hizmet tüm yüksek kullanılabilirlik görevlerini halleder. Dinamik olarak eklenebilir veya kaldırılabilir. Yeni bir istek her geldiğinde, şu anda kullanılabilen bağlayıcılardan birine yönlendirilir. Bağlayıcı geçici olarak kullanılamıyorsa, bu trafiğe yanıt vermez.

Konektörler durum suz ve makinede yapılandırma verileri yok. Depoladıkları tek veri, hizmeti ve kimlik doğrulama sertifikasını bağlama ayarlarıdır. Hizmete bağlandıklarında, gerekli tüm yapılandırma verilerini çeker ve birkaç dakikada bir yenilerler.

Bağlayıcılar ayrıca, bağlayıcının daha yeni bir sürümü olup olmadığını öğrenmek için sunucuyu yoklar. Biri bulunursa, bağlayıcılar kendilerini günceller.

Olay günlüğü ve performans sayaçlarını kullanarak konektörlerinizi çalıştıkları makineden izleyebilirsiniz. Veya durumlarını Azure portalının Uygulama Proxy sayfasından görüntüleyebilirsiniz:

![Örnek: Azure AD Application Proxy bağlayıcıları](./media/application-proxy-connectors/app-proxy-connectors.png)

Kullanılmayan bağlayıcıları el ile silmeniz gerekmez. Bir bağlayıcı çalışırken, hizmete bağlanırken etkin kalır. Kullanılmayan bağlayıcılar _etkin olmayan_ olarak etiketlenir ve 10 günlük hareketsizlikten sonra kaldırılır. Ancak bir bağlayıcıyı kaldırmak istiyorsanız, sunucudan hem Bağlayıcı hizmetini hem de Updater hizmetini kaldırın. Hizmeti tam olarak kaldırmak için bilgisayarınızı yeniden başlatın.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler

Azure AD, dağıttığınız tüm bağlayıcılar için otomatik güncelleştirmeler sağlar. Application Proxy BağlayıcıSı güncellemesi hizmeti çalışır durumda olduğu sürece, bağlayıcılarınız otomatik olarak güncellenir. Sunucunuzda Bağlayıcı Güncelleyici hizmetini görmüyorsanız, herhangi bir güncelleştirme almak için [bağlayıcınızı yeniden yüklemeniz](application-proxy-add-on-premises-application.md) gerekir.

Bağlayıcınıza otomatik bir güncelleştirme gelmesini beklemek istemiyorsanız, el ile yükseltme yapabilirsiniz. Bağlayıcınızın bulunduğu sunucudaki [bağlayıcı indirme sayfasına](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) gidin ve **İndir'i**seçin. Bu işlem, yerel bağlayıcı için bir yükseltme başladı.

Birden çok bağlayıcısı olan kiracılar için otomatik güncelleştirmeler, ortamınızdaki kapalı kalma sürelerini önlemek için her grupta aynı anda bir bağlayıcıyı hedeflemektedir.

Konektörünüz güncellediğinde şu süreyle kesinti yaşayabilirsiniz:
  
- İkinci bir bağlayıcı yüklemenizi ve bir bağlayıcı [grubu oluşturmanızı](application-proxy-connector-groups.md)tavsiye ettiğimiz tek bir konektörvar. Bu, kapalı kalma süresini önler ve daha yüksek kullanılabilirlik sağlar.  
- Güncelleştirme başladığında bir bağlayıcı işlemin ortasındaydı. İlk işlem kaybolsa da, tarayıcınız işlemi otomatik olarak yeniden denemelidir veya sayfanızı yenileyebilirsiniz. İstek gücendiğinde, trafik bir yedek bağlayıcıya yönlendirilir.

Daha önce yayımlanan sürümler ve hangi değişiklikleri içerdiği hakkında bilgi görmek için [Bkz. Uygulama Proxy-Sürüm Yayın Geçmişi](application-proxy-release-version-history.md).

## <a name="creating-connector-groups"></a>Bağlayıcı grupları oluşturma

Bağlayıcı grupları, belirli uygulamalara hizmet etmek için belirli bağlayıcılar atamanızı sağlar. Bir dizi bağlayıcıyı bir araya getirip her uygulamayı bir gruba atayabilirsiniz.

Bağlayıcı grupları, büyük dağıtımların yönetilmesini kolaylaştırır. Ayrıca, yalnızca yerel uygulamalara hizmet vermek için konum tabanlı bağlayıcı grupları oluşturabileceğinizden, farklı bölgelerde barındırılan uygulamaları olan kiracıların gecikme sürelerini de artırırlar.

Bağlayıcı grupları hakkında daha fazla bilgi edinmek için [bkz.](application-proxy-connector-groups.md)

## <a name="capacity-planning"></a>Kapasite planlaması

Beklenen trafik hacmini işlemek için bağlayıcılar arasında yeterli kapasite yi planladığından emin olmak önemlidir. Her bağlayıcı grubunun yüksek kullanılabilirlik ve ölçek sağlamak için en az iki konektöre sahip olmasını öneririz. Herhangi bir noktada bir makineye hizmet vermeniz gerekebilir sayılsa da üç konektöre sahip olmak en uygun uyrabilir.

Genel olarak, ne kadar çok kullanıcınız olursa, o kadar büyük bir makineye ihtiyacınız olur. Aşağıda, ses düzeyinin anahatlarını veren ve farklı makinelerin işleyebilir beklenen gecikme si tablosu verilmiştir. Kullanım şekilleri değiştiğinden ve yükü tahmin etmek için kullanılamadığından, bunların hepsinin kullanıcı tarafından değil, Saniye Başına Beklenen İşlemler'e (TPS) dayandığını unutmayın. Ayrıca yanıtların boyutuna ve arka uç uygulama yanıt süresine bağlı olarak bazı farklılıklar olacaktır - daha büyük yanıt boyutları ve daha yavaş yanıt süreleri daha düşük bir Max TPS ile sonuçlanır. Ayrıca, makineler arasında dağıtılan yükün her zaman yeterli arabellek sağlaması için ek makinelere sahip olmanızı öneririz. Ekstra kapasite, yüksek kullanılabilirlik ve esneklik sağlar.

|Çekirdekler|RAM|Beklenen Gecikme (MS)-P99|Maksimum TPS|
| ----- | ----- | ----- | ----- |
|2|8|325|586|
|4|16|320|1150|
|8|32|270|1190|
|16|64|245|1200*|

\*Bu makine, varsayılan bağlantı sınırlarından bazılarını .NET önerilen ayarların ötesine yükseltmek için özel bir ayar kullandı. Bu sınırın kiracınız için değiştirilmesi için desteğe başvurmadan önce varsayılan ayarları içeren bir test çalıştırmanızı öneririz.

> [!NOTE]
> Maksimum TPS'de 4, 8 ve 16 çekirdekli makine arasında çok fazla fark yoktur. Bunlar arasındaki temel fark beklenen gecikme dir.
>
> Bu tablo aynı zamanda, yüklendiğindeki makinenin türüne göre bir konektörün beklenen performansına da odaklanır. Bu, Uygulama Proxy hizmetinin azaltma sınırlarından ayrıdır, [bkz.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions)

## <a name="security-and-networking"></a>Güvenlik ve ağ

Bağlayıcılar, ağüzerinde herhangi bir yere yüklenebilir ve bu da uygulama proxy hizmetine istek göndermelerine olanak tanır. Önemli olan, konektörü çalıştıran bilgisayarın uygulamalarınıza da erişimi olmasıdır. Konektörleri şirket ağınızın içine veya bulutta çalışan sanal bir makineye yükleyebilirsiniz. Bağlayıcılar, arındırılmış bölge (DMZ) olarak da bilinen bir çevre ağı içinde çalıştırılabilir, ancak ağınızın güvende kalması için tüm trafiğin giden olması nedeniyle gerekli değildir.

Bağlayıcılar yalnızca giden istekleri gönderir. Giden trafik, Uygulama Proxy hizmetine ve yayınlanan uygulamalara gönderilir. Bir oturum oluşturulduktan sonra trafik her iki yönde akar, çünkü gelen bağlantı noktalarını açmak zorunda değilsiniz. Ayrıca, gelen erişimi güvenlik duvarlarınız aracılığıyla yapılandırmanız da gerek yoktur.

Giden güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için, [bkz.](application-proxy-configure-connectors-with-proxy-servers.md)

## <a name="performance-and-scalability"></a>Performans ve ölçeklenebilirlik

Uygulama Proxy hizmeti için ölçek saydamdır, ancak ölçek bağlayıcılar için bir faktördür. Yoğun trafiği işlemek için yeterli konektöre sahip olmanız gerekir. Bağlayıcılar devletsiz olduğundan, kullanıcı veya oturum sayısından etkilenmezler. Bunun yerine, isteklerin sayısına ve taşıma yükü boyutuna yanıt verirler. Standart web trafiği ile, ortalama bir makine saniyede birkaç bin istek işleyebilir. Belirli kapasite makine nin tam özelliklerine bağlıdır.

Bağlayıcı performansı CPU ve ağ ile bağlıdır. TLS şifreleme ve şifre çözme için CPU performansı gerekirken, Azure'daki uygulamalara ve çevrimiçi hizmete hızlı bağlantı sağlamak için ağ oluşturmak önemlidir.

Buna karşılık, bellek bağlayıcılar için bir sorun daha azdır. Çevrimiçi hizmet, işlemenin ve tüm kimliği doğrulanmamış trafiğin çoğunu halleder. Bulutta yapılabilecek her şey bulutta yapılır.

Herhangi bir nedenle bu konektör veya makine kullanılamıyorsa, trafik gruptaki başka bir bağlayıcıya gitmeye başlar. Bu esneklik aynı zamanda birden fazla konektöre sahip olmanızı öneririz nedenidir.

Performansı etkileyen bir diğer faktör de konektörler arasındaki ağ kalitesinin aşağıdakiler de dahil olmak üzere

- **Çevrimiçi hizmet**: Azure'daki Uygulama Proxy hizmetine yavaş veya yüksek gecikmeli bağlantılar bağlayıcı performansını etkiler. En iyi performans için, Express Route ile kuruluşunuzu Azure'a bağlayın. Aksi takdirde, ağ ekibinizin Azure bağlantılarının mümkün olduğunca verimli bir şekilde işlenmesini sağlamasını sağlayın.
- **Arka uç uygulamaları**: Bazı durumlarda, bağlayıcı ile arka uç uygulamaları arasında bağlantıları yavaşlatabilecek veya engelleyebilen ek yakınlıklar vardır. Bu senaryoyu gidermek için bağlayıcı sunucudan bir tarayıcı açın ve uygulamaya erişmeye çalışın. Konektörleri Azure'da çalıştırıyorsanız ancak uygulamalar şirket içindeyse, bu deneyim kullanıcılarınızın beklediği gibi olmayabilir.
- **Etki alanı denetleyicileri**: Bağlayıcılar Kerberos Kısıtlı Temsilciliği'ni kullanarak tek oturum açma (SSO) gerçekleştirirse, isteği arka uca göndermeden önce etki alanı denetleyicilerine başvururlar. Konektörlerin Bir Kerberos bileti önbelleği vardır, ancak yoğun bir ortamda etki alanı denetleyicilerinin yanıt verme hızı performansı etkileyebilir. Bu sorun, Azure'da çalışan ancak şirket içinde olan etki alanı denetleyicileriyle iletişim kuran bağlayıcılar için daha yaygındır.

Ağınızı en iyi duruma alma hakkında daha fazla bilgi için [Azure Active Directory Application Proxy'yi kullanırken Ağ topolojisi hususlarına](application-proxy-network-topology.md)bakın.

## <a name="domain-joining"></a>Etki alanına katılma

Bağlayıcılar etki alanı birleştirilmiş olmayan bir makinede çalıştırılabilir. Ancak, Tümleşik Windows Kimlik Doğrulama (IWA) kullanan uygulamalariçin tek oturum açma (SSO) istiyorsanız, etki alanı yla birleştirilmiş bir makineye ihtiyacınız vardır. Bu durumda, konektör makineleri yayınlanan uygulamalar için kullanıcılar adına [Kerberos](https://web.mit.edu/kerberos) Kısıtlı Temsilci gerçekleştirebilecek bir etki alanına birleştirilmelidir.

Bağlayıcılar, kısmi güvene sahip etki alanları veya ormanlara veya salt okunur etki alanı denetleyicilerine de katılabilir.

## <a name="connector-deployments-on-hardened-environments"></a>Sertleştirilmiş ortamlarda bağlayıcı dağıtımları

Genellikle, bağlayıcı dağıtımı basittir ve özel yapılandırma gerektirmez. Ancak, göz önünde bulundurulması gereken bazı benzersiz koşullar vardır:

- Giden trafiği sınırlayan kuruluşlar [gerekli bağlantı noktalarını açmalıdır.](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment)
- FIPS uyumlu makinelerin, konektör işlemlerinin bir sertifika oluşturmasına ve depolamasına izin vermek için yapılandırmalarını değiştirmeleri gerekebilir.
- Ağ isteklerini veren işlemlere bağlı olarak ortamlarını kilitleyen kuruluşlar, her iki bağlayıcı hizmetinin de gerekli tüm bağlantı noktalarına ve IP'lere erişebilmek için etkinleştirildiğinden emin olmak zorundadır.
- Bazı durumlarda, giden ileri vekiller iki yönlü sertifika kimlik doğrulamasını bozabilir ve iletişimin başarısız lığa neden olabilir.

## <a name="connector-authentication"></a>Bağlayıcı kimlik doğrulaması

Güvenli bir hizmet sağlamak için, bağlayıcıların hizmete doğru kimlik doğrulaması ve hizmetin bağlayıcıya doğru kimlik doğrulaması gerekir. Bu kimlik doğrulaması, bağlayıcılar bağlantıyı başlattığında istemci ve sunucu sertifikaları kullanılarak yapılır. Bu şekilde yöneticinin kullanıcı adı ve şifresi bağlayıcı makinede depolanmaz.

Kullanılan sertifikalar Uygulama Proxy hizmetine özgür. İlk kayıt sırasında oluşturulurlar ve bağlayıcılar tarafından birkaç ayda bir otomatik olarak yenilenirler.

Bir bağlayıcı hizmete birkaç ay bağlı değilse, sertifikaları eski olabilir. Bu durumda, kaydı tetiklemek için konektörü kaldırın ve yeniden yükleyin. Aşağıdaki PowerShell komutlarını çalıştırabilirsiniz:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-the-hood"></a>Başlık altında

Bağlayıcılar Windows Server Web Application Proxy'yi temel aldıklarından, Windows Olay Günlükleri de dahil olmak üzere aynı yönetim araçlarının çoğuna sahiptirler

![Olay Görüntüleyicisi ile etkinlik günlüklerini yönetme](./media/application-proxy-connectors/event-view-window.png)

ve Windows performans sayaçları.

![Performans Monitörü ile konektöre sayaç ekleme](./media/application-proxy-connectors/performance-monitor.png)

Bağlayıcılar hem yönetici hem de oturum günlükleri vardır. Yönetici günlükleri önemli olayları ve hatalarını içerir. Oturum günlükleri tüm hareketleri ve bunların işlem ayrıntılarını içerir.

Günlükleri görmek için Olay Görüntüleyicisi'ne gidin, **Görünüm** menüsünü açın ve **analitik ve hata ayıklama günlüklerini göster'i**etkinleştirin. Ardından, olayları toplamaya başlamalarını sağlar. Bu günlükler, bağlayıcılar daha yeni bir sürüme dayandığı ndan, Windows Server 2012 R2'deki Web Application Proxy'de görünmez.

Hizmetler penceresinde hizmetin durumunu inceleyebilirsiniz. Bağlayıcı iki Windows Hizmetinden oluşur: gerçek bağlayıcı ve güncelleyici. İkisi de sürekli koşmak zorunda.

 ![Örnek: Azure AD hizmetlerini yerel gösteren hizmetler penceresi](./media/application-proxy-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Bağlayıcı gruplarını kullanarak uygulamaları ayrı ağlarda ve konumlarda yayımlama](application-proxy-connector-groups.md)
- [Varolan şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
- [Sorun Giderme Uygulama Proxy ve bağlayıcı hataları](application-proxy-troubleshoot.md)
- [Azure AD Uygulama Proxy Bağlayıcısı sessizce nasıl yüklenir?](application-proxy-register-connector-powershell.md)
