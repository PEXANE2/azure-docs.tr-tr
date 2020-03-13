---
title: Azure AD uygulama ara sunucusu bağlayıcıları anlama | Microsoft Docs
description: Azure AD uygulama ara sunucusu bağlayıcıları ile ilgili temel bilgileri kapsar.
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
ms.openlocfilehash: 1c2036bf9995725e4bbef44e4c039f8336eb81a0
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79244296"
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Azure AD uygulama ara sunucusu bağlayıcıları anlama

Bağlayıcılar, hangi Azure AD uygulama proxy'si olası yaptıran şeydir. Bunlar basit, kolayca dağıtılır ve korunur ve süper güçlü bir işlemdir. Bu makalede nasıl çalıştıkları hangi bağlayıcıları olan ve dağıtımınızın iyileştirilmesine yönelik bazı öneriler ele alınmaktadır.

## <a name="what-is-an-application-proxy-connector"></a>Uygulama Ara sunucusu bağlayıcısını nedir?

Bağlayıcılar, şirket içi yaslanın ve uygulama proxy'si Hizmeti giden bağlantı kolaylaştırmak basit aracılardır. Bağlayıcılar, arka uç uygulaması erişimi olan bir Windows Server üzerinde yüklenmelidir. Bağlayıcılar, belirli uygulamalara trafiği işleme her grubuyla bağlayıcı gruplar halinde düzenleyebilirsiniz.

## <a name="requirements-and-deployment"></a>Gereksinimler ve dağıtım

Uygulama proxy'si başarıyla dağıtmak için en az bir bağlayıcı gerekir, ancak iki veya daha fazla dayanıklılık için daha fazla öneririz. Bağlayıcıyı Windows Server 2012 R2 veya üstünü çalıştıran bir makineye yükler. Bağlayıcının, uygulama proxy hizmeti ve yayımladığınız şirket içi uygulamalarla iletişim kurması gerekir.

### <a name="windows-server"></a>Windows server
Windows Server 2012 R2 çalıştıran bir sunucuya ihtiyacınız veya daha sonra uygulama ara sunucusu Bağlayıcısı'nı yükleyebilirsiniz. Sunucunun Azure 'daki uygulama proxy hizmetlerine ve yayımlamakta olduğunuz şirket içi uygulamalara bağlanması gerekir.

Windows server TLS 1.2 uygulama ara sunucusu bağlayıcısını yüklemeden önce etkin olmalıdır. Sunucuda TLS 1,2 ' i etkinleştirmek için:

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Sunucuyu yeniden başlatın

Bağlayıcı sunucusu için ağ gereksinimleri hakkında daha fazla bilgi için bkz. [uygulama proxy 'si ile çalışmaya başlama ve bağlayıcı yüklemeyi](application-proxy-add-on-premises-application.md).

## <a name="maintenance"></a>Bakım

Bağlayıcılar ve hizmetin tüm yüksek kullanılabilirlik görevlerini ilgileniriz. Bunlar eklenebilir veya dinamik olarak kaldırılmış. Yeni bir istek geldiğinde her zaman, şu anda kullanılabilen bağlayıcıların birine yönlendirilir. Bir bağlayıcıyı geçici olarak kullanılamıyor, giden trafiğin yanıt vermiyor.

Bağlayıcılar, durum bilgisiz olduğundan ve hiçbir yapılandırma verilerini makine üzerinde sahip. Yalnızca bunlar depolamak verileri, hizmet ve kimlik doğrulama sertifikasını bağlamak için ayarlarıdır. Hizmete bağlandıklarında tüm gerekli yapılandırma verilerini çekme ve her birkaç dakika yenileyin.

Bağlayıcılar ayrıca olup bulmak için sunucuyu yoklamak connector'ın daha yeni bir sürümü. Bağlayıcılar, bulunması durumunda, kendilerini güncelleştirin.

Olay günlüğü ve performans sayaçlarını kullanarak, üzerinde çalıştıkları makineden bağlayıcılarınızı izleyebilirsiniz. Ya da Azure portal'ın uygulama proxy'si sayfasından durumlarını görüntüleyebilirsiniz:

![Örnek: Azure AD Uygulama Ara Sunucusu bağlayıcıları](./media/application-proxy-connectors/app-proxy-connectors.png)

Kullanılmayan bağlayıcılar el ile silmeniz gerekmez. Bir bağlayıcı çalışırken hizmete bağlandığında etkin kalır. Kullanılmayan bağlayıcılar, _etkin değil_ olarak etiketlenebilir ve 10 gün etkin olmama durumunda kaldırılır. Ancak, bir bağlayıcıyı kaldırmak isterseniz hem bağlayıcı hizmetini hem de Updater hizmetini sunucudan kaldırın. Hizmeti tam olarak kaldırmak için bilgisayarınızı yeniden başlatın.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler

Azure AD, dağıttığınız tüm bağlayıcıları otomatik güncelleştirmeler sağlar. Application Proxy Connector Updater hizmeti çalışıyor olduğu sürece, bağlayıcılarınızı otomatik olarak güncelleştirilir. Sunucunuzda Bağlayıcı Güncelleştiricisi hizmetini görmüyorsanız, tüm güncelleştirmeleri almak için [bağlayıcınızı yeniden yüklemeniz](application-proxy-add-on-premises-application.md) gerekir.

Otomatik güncelleştirme 'nin bağlayıcınıza gelmesini beklemek istemiyorsanız, el ile yükseltme yapabilirsiniz. Bağlayıcının bulunduğu sunucudaki [bağlayıcı indirme sayfasına](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) gidin ve **İndir**' i seçin. Bu işlem yerel bağlayıcı için bir yükseltme başlatan.

Birden çok Bağlayıcılarla kiracılar için Otomatik Güncelleştirmeler ortamınızdaki kesinti süresini önlemek için her grupta bir anda tek bir bağlayıcıyı hedefleyin.

Bağlayıcınızı güncelleştirir, kapalı kalma süresi karşılaşabilirsiniz:
  
- İkinci bir bağlayıcı yüklemenizi ve [bir bağlayıcı grubu oluşturmanızı](application-proxy-connector-groups.md)öneririz. Bu, kapalı kalma süresini ortadan kaldırmak ve daha yüksek kullanılabilirlik sağlar.  
- Güncelleştirme başladığında bir bağlayıcı ortasında bir işlem oluştu. İlk işlem kayıp olsa da, tarayıcınız otomatik olarak işlemi yeniden denemesinin veya sayfanızı yenileyebilirsiniz. İstek gönderilir, trafik için yedek bir bağlayıcı yönlendirilir.

Önceden yayınlanan sürümler ve içerdikleri değişiklikler hakkında bilgi almak için bkz. [uygulama proxy sürümü yayın geçmişi](application-proxy-release-version-history.md).

## <a name="creating-connector-groups"></a>Bağlayıcı grupları oluşturma

Bağlayıcı grupları belirli uygulamalar sunmak için özel bağlayıcılar atamanızı sağlar. Bağlayıcılar sayısı gruplamak ve her bir uygulama bir gruba atayın.

Bağlayıcı grupları büyük dağıtımları yönetmeyi kolaylaştırır. Bunlar ayrıca gecikme kiracılar için yalnızca yerel uygulamalar sunmak için konum temelli bağlayıcı grupları oluşturabildiğinden, farklı bölgelerde barındırılan uygulamalar geliştirin.

Bağlayıcı grupları hakkında daha fazla bilgi edinmek için bkz. [bağlayıcı gruplarını kullanarak ayrı ağlarda ve konumlarda uygulama yayımlama](application-proxy-connector-groups.md).

## <a name="capacity-planning"></a>Kapasite planlaması

Beklenen trafik birimini işlemek için bağlayıcılar arasında yeterli kapasiteye sahip olduğunuzdan emin olmak önemlidir. Yüksek kullanılabilirlik ve ölçek sağlamak için her bir bağlayıcı grubunda en az iki bağlayıcı olmasını öneririz. Her zaman bir makineye hizmet etmeniz gerekebilmeniz için üç bağlayıcı olması en iyi durumdur.

Genellikle, ne kadar çok kullanıcı varsa, ihtiyacınız olacak bir makine daha büyük olur. Aşağıda birimin ana hattını sağlayan bir tablo ve farklı makinelerin işleyebileceği beklenen gecikme süresi verilmiştir. Bu, kullanım desenleri farklılık gösterdiğinden ve yükü tahmin etmek için kullanılamadığından, Kullanıcı tarafından değil, bir saniyede beklenen Işlem (TPS) temel alır. Ayrıca, yanıtların boyutuna ve arka uç uygulama yanıt süresi ve daha yavaş yanıt süreleriyle ilgili bazı farklılıklar da daha düşük olan en büyük TPS 'ye neden olur. Ayrıca, makineler genelinde dağıtılmış yükün her zaman geniş bir arabellek sağladığından, ek makineler de yapmanızı öneririz. Ekstra kapasite yüksek kullanılabilirlik ve dayanıklılık sahibi olmanızı sağlayacaktır.

|Çekirdekler|RAM|Gecikme süresi (MS) bekleniyordu-P99|En fazla TPS|
| ----- | ----- | ----- | ----- |
|2|8|325|586|
|4|16|320|1150|
|8|32|270|1190|
|16|64|245|1200*|

\* bu makine, .NET tarafından önerilen ayarların ötesinde bazı varsayılan bağlantı sınırlarını yükseltmek için özel bir ayar kullandı. Kiracınız için değiştirildi. Bu sınırı almak için destek ile irtibat kurmadan önce varsayılan ayarlarla bir test çalıştırmanızı öneririz.

> [!NOTE]
> Kadar en fazla TPS 4, 8 ve 16 çekirdekli makinelerde arasındaki farkı yoktur. Bunlar arasındaki temel fark, beklenen gecikme ' dir.
>
> Bu tablo, üzerinde yüklü olduğu makinenin türüne bağlı olarak bir bağlayıcının beklenen performansına de odaklanır. Bu, uygulama proxy 'Si hizmetinin azaltma limitlarından ayrıdır, bkz. [hizmet limitleri ve kısıtlamaları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions).

## <a name="security-and-networking"></a>Güvenlik ve ağ özellikleri

Bağlayıcılar için uygulama proxy'si hizmeti istekleri göndermesine izin veren ağ her yerden yüklenebilir. Önemli olan, ayrıca bağlayıcıyı çalıştıran bilgisayarın uygulamalarınıza erişim sahip olur. Bağlayıcılar, Kurumsal ağınızın içinde veya bulutta çalışan bir sanal makine üzerinde yükleyebilirsiniz. Bağlayıcılar, sivilleştirilmiş bölge (DMZ) olarak da bilinen bir çevre ağı içinde çalışabilir, ancak ağınızın güvenliği güvende kalacak şekilde tüm trafik giden bir çıkış olduğundan bu gerekli değildir.

Bağlayıcılar, yalnızca giden istekler gönderin. Giden trafik uygulama proxy'si hizmeti ve yayımlanan uygulamalara gönderilir. Oturum kurulduktan sonra iki yolu vardır ve trafik akışları için gelen bağlantı noktalarını açmanız gerekmez. Ayrıca, güvenlik duvarlarınız aracılığıyla gelen erişimi yapılandırmanız gerekmez.

Giden güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için bkz. [mevcut şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md).

## <a name="performance-and-scalability"></a>Performans ve ölçeklenebilirlik

Uygulama proxy'si hizmeti için ölçek saydamdır, ancak ölçek bağlayıcıların bir faktördür. Yoğun trafiği işlemek için yeterli bağlayıcılar olması gerekir. Bağlayıcılar durum bilgisiz olduğundan Kullanıcı veya oturum sayısından etkilenmemektedir. Bunun yerine, istek sayısı ve bunların yükü boyutu için yanıt. Standart web trafiği ile ortalama bir makine saniyede birkaç bin istekleri işleyebilir. Belirli kapasite tam makine özelliklerine bağlıdır.

Bağlayıcı performansı CPU ve ağ ile ilişkilidir. Ağ Azure'da hızlı bağlantı uygulamalara ve çevrimiçi hizmete almak önemli olmakla birlikte bir CPU performans SSL şifreleme ve şifre çözme, için gereklidir.

Buna karşılık, Bağlayıcılarla ilgili sorun bellektir. Çevrimiçi hizmet işleme çoğunu ve kimliği doğrulanmamış tüm trafiği üstlenir. Bulutta yapılabilir her şey bulutta yapılır.

Bağlayıcısı veya makine kullanılamaz hale herhangi bir nedenle, başka bir bağlayıcı grubunda gidip trafik başlar. Bu da neden olan birden fazla bağlayıcıyı öneririz dayanıklılıktır.

Performansını etkileyen başka bir ağ dahil olmak üzere bu bağlayıcıları arasında kalitesini etkendir:

- **Çevrimiçi hizmet**: Azure 'Da uygulama proxy hizmeti için yavaş veya yüksek gecikmeli bağlantılar, bağlayıcı performansını etkiler. En iyi performans için kuruluşunuz Azure Express Route ile bağlanın. Aksi takdirde, ağ ekibiniz Azure bağlantıları mümkün olduğunca verimli bir şekilde işlendiğinden emin olmak gerekir.
- **Arka uç uygulamaları**: bazı durumlarda bağlayıcı ve bağlantıları yavaşlatabilecek ya da engelleyebilen arka uç uygulamaları arasında ek proxy 'ler vardır. Bu senaryoyla ilgili sorunları gidermek için bağlayıcı sunucusundan bir tarayıcı açın ve uygulamaya erişmeyi deneyin. Bağlayıcılar Azure'da çalıştırdığınız ancak şirket içi uygulamaları, ne kullanıcılarınızın beklediği deneyimi olmayabilir.
- **Etki alanı denetleyicileri**: bağlayıcılar, Kerberos kısıtlanmış temsilciyi kullanarak çoklu oturum açma (SSO) gerçekleştirdiklerinde, isteği arka uca göndermeden önce etki alanı denetleyicileriyle iletişim kurabilirler. Kerberos biletlerinin bir önbellek bağlayıcılara sahiptir, ancak meşgul bir ortamda, etki alanı denetleyicilerinin yanıtlama performansını etkileyebilir. Bu sorun, Azure'da çalıştırmak, ancak şirket içi etki alanı denetleyicileriyle iletişim bağlayıcıların daha yaygındır.

Ağınızı en iyi duruma getirme hakkında daha fazla bilgi için, [Azure Active Directory uygulama ara sunucusu kullanırken ağ topolojisi konularına](application-proxy-network-topology.md)bakın.

## <a name="domain-joining"></a>Etki alanına katılma

Bağlayıcılar değil etki alanına katılmış bir makinede çalıştırabilirsiniz. Ancak, tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar için çoklu oturum açma (SSO) istiyorsanız, etki alanına katılmış bir makine gerekir. Bu durumda, bağlayıcı makineleri yayımlanan uygulamalar için kullanıcılar adına [Kerberos](https://web.mit.edu/kerberos) kısıtlı temsili gerçekleştirebilen bir etki alanına katılmalıdır.

Bağlayıcılar ayrıca etki alanı ya da kısmi bir güvene sahip ormanlara veya salt okunur etki alanı denetleyicilerine katılabilir.

## <a name="connector-deployments-on-hardened-environments"></a>Sağlamlaştırılmış ortamlarda bağlayıcı dağıtımları

Genellikle, bağlayıcı dağıtım oldukça basittir ve özel bir yapılandırma gerektirir. Ancak, ele alınması gereken benzersiz bazı koşullar vardır:

- Giden trafiği sınırlayan kuruluşların [gereken bağlantı noktalarını açması](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment)gerekir.
- FIPS uyumlu makineler oluşturmak ve bir sertifika deposu bağlayıcı işlemlere izin verecek şekilde yapılandırmalarını değiştirmek için gerekli olabilir.
- Tüm gerekli bağlantı noktaları ve IP'ler erişmek için her iki bağlayıcıyı hizmetin etkin olduğundan emin olmak ağ istekleri işlemleri temel alan ortamlarında kilitleme kuruluşların sahip.
- Bazı durumlarda, giden iletme proxy'leri karşılıklı sertifika kimlik doğrulaması bölün ve iletişimin başarısız olmasına neden.

## <a name="connector-authentication"></a>Bağlayıcı kimlik doğrulaması

Güvenli bir hizmet sağlamak için doğru hizmet kimlik doğrulaması bağlayıcılar gerekir ve hizmet bağlayıcı kimlik doğrulaması gerekir. Bu kimlik doğrulaması yapılır istemci ve sunucu sertifikaları kullandığınızda bağlayıcıları bağlantıyı başlatırsınız. Bu şekilde, bağlayıcı makinesinde yönetici kullanıcı adı ve parola depolanmaz.

Uygulama proxy'si hizmeti için kullanılan sertifikaları özgüdür. İlk kayıt sırasında oluşturulan ve otomatik olarak bağlayıcılar tarafından her birkaç ay yenilenir.

Bir bağlayıcı birkaç ay boyunca hizmetine bağlı değilse, sertifikaları güncel olmayabilir. Bu durumda, kaldırın ve bağlayıcıyı tetikleyici kayıt için yeniden yükleyin. Aşağıdaki PowerShell komutlarını çalıştırabilirsiniz:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-the-hood"></a>Başlık altında

Çoğu Windows olay günlükleri de dahil olmak üzere aynı yönetim araçlarını, sahip oldukları için bağlayıcılar Windows Server Web Uygulama Proxy üzerinde temel alır

![Olay günlükleri Olay Görüntüleyicisi'ni ile yönetme](./media/application-proxy-connectors/event-view-window.png)

ve Windows performans sayaçları.

![Bağlayıcı ile Performans İzleyicisi sayaçları ekleyin](./media/application-proxy-connectors/performance-monitor.png)

Hem yönetim hem de oturum takılabilecek günlükleri. Yönetici günlükler anahtar olayları ve bunların hatalarını içerir. Oturum günlükleri tüm işlemleri ve bunların işleme ayrıntılarını içerir.

Günlükleri görmek için Olay Görüntüleyicisi gidin, **Görünüm** menüsünü açın ve **analitik ve hata ayıklama günlüklerini göster**' i etkinleştirin. Ardından, bunları olayları toplamaya başlamak etkinleştirin. Bağlayıcılar daha yeni bir sürümü üzerinde dayalı olarak bu günlükleri Web uygulaması Ara sunucusu Windows Server 2012 R2'de görünmez.

Hizmetler penceresini hizmeti durumunu inceleyebilirsiniz. Bağlayıcı iki Windows hizmetinden oluşur: gerçek bağlayıcı ve Güncelleştirici. Her ikisi de her zaman çalıştırmalısınız.

 ![Örnek: Azure AD hizmetleri yerel ' i gösteren hizmetler penceresi](./media/application-proxy-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Bağlayıcı gruplarını kullanarak ayrı ağlarda ve konumlarda uygulama yayımlama](application-proxy-connector-groups.md)
- [Mevcut şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
- [Uygulama proxy 'Si ve bağlayıcı hatalarında sorun giderme](application-proxy-troubleshoot.md)
- [Azure AD Uygulama Ara Sunucusu bağlayıcısını sessizce yüklemek](application-proxy-register-connector-powershell.md)
