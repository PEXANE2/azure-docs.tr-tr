---
title: Azure Service Fabric uygulama güvenliği hakkında bilgi edinin
description: Service Fabric'te mikro hizmet uygulamalarının nasıl güvenli bir şekilde çalıştırılacaklarına genel bakış. Hizmetleri ve başlangıç komut dosyasını farklı güvenlik hesapları altında çalıştırmayı, kullanıcıların kimliğini doğrulamayı ve yetkilendirmeyi, uygulama sırlarını yönetmeyi, hizmet iletişimini nasıl sağlayacağınızı, bir API ağ geçidini nasıl kullanacağınızı ve uygulama verilerini istirahatte nasıl güvenli hale erdireceklerini öğrenin.
ms.topic: conceptual
ms.date: 03/16/2018
ms.openlocfilehash: c97c5345a1a18cce8c44508542f12d3642d2b8f9
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461438"
---
# <a name="service-fabric-application-and-service-security"></a>Servis Kumaşı uygulaması ve servis güvenliği
Bir microservices mimarisi [birçok faydalar](service-fabric-overview-microservices.md)getirebilir. Ancak, mikro hizmetlerin güvenliğini yönetmek, geleneksel monolitik uygulama güvenliğini yönetmekten farklıdır ve zordur. 

Monolit ile uygulama genellikle bir ağ içinde bir veya daha fazla sunucuda çalışır ve açıkbağlantı noktalarını, API'leri ve IP adresini tanımlamak daha kolaydır. Genellikle bir çevre veya sınır ve korumak için bir veritabanı vardır. Bu sistem bir güvenlik ihlali veya saldırı nedeniyle tehlikeye girerse, sistem deki her şeyin saldırganın kullanımına açık olması olasıdır. Mikro hizmetlerle sistem daha karmaşıktır.  Hizmetler merkezi olmayan ve birçok ana bilgisayar arasında dağıtılır ve ana bilgisayardan ana bilgisayara geçiş.  Uygun güvenlikle, bir saldırganın alabilecekleri ayrıcalıkları ve tek bir saldırıda kullanılabilen veri miktarını tek bir hizmeti ihlal ederek sınırlandırın.  İletişim dahili değildir, ancak bir ağ üzerinden gerçekleşir ve hizmetler arasında birçok açık bağlantı noktası ve etkileşim vardır. Bu hizmet etkileşimlerinin ne olduğunu ve ne zaman gerçekleştiğini bilmek uygulama güvenliğiniz için çok önemlidir.

Bu makalede, mikro hizmetler güvenliği için bir rehber değildir, bu tür birçok kaynak çevrimiçi kullanılabilir, ancak hizmet Kumaş güvenlik farklı yönlerini nasıl gerçekleştirilebilir açıklar.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Genellikle bir hizmet tarafından açığa çıkarılan kaynakların ve API'lerin belirli güvenilir kullanıcılar veya istemciler ile sınırlı olması gerekir. Kimlik doğrulama, kullanıcının kimliğini güvenilir bir şekilde belirleme işlemidir.  Yetkilendirme, API'leri veya hizmetleri bazı kimlik doğrulaması yapılan kullanıcılar için kullanılabilir kılan, başkalarının değil, işlemdir.

### <a name="authentication"></a>Kimlik Doğrulaması
API düzeyinde güven kararları vermenin ilk adımı kimlik doğrulamadır. Kimlik doğrulama, kullanıcının kimliğini güvenilir bir şekilde belirleme işlemidir.  Mikro hizmet senaryolarında kimlik doğrulama genellikle merkezi olarak işlenir. Bir API Ağ Geçidi kullanıyorsanız, [kimlik doğrulamayı](/azure/architecture/patterns/gateway-offloading) ağ geçidine boşaltabilirsiniz. Bu yaklaşımı kullanırsanız, ağ geçidinden gelen iletileri doğrulamak için ek güvenlik olmadığı sürece, tek tek hizmetlere doğrudan (API Ağ Geçidi olmadan) ulaşılamayacağından emin olun.

Hizmetlere doğrudan erişilebiliyorsa, Azure Etkin Dizini gibi bir kimlik doğrulama hizmeti veya güvenlik belirteci hizmeti (STS) olarak hareket eden özel bir kimlik doğrulama mikro hizmeti, kullanıcıların kimliğini doğrulamak için kullanılabilir. Güven kararları, güvenlik belirteçleri veya tanımlama bilgileriyle hizmetler arasında paylaşılır. 

ASP.NET Core [için, kullanıcıların kimlik doğrulaması](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/) için birincil mekanizma ASP.NET Çekirdek Kimlik üyelik sistemidir. ASP.NET Core Identity, geliştirici tarafından yapılandırılan bir veri deposunda kullanıcı bilgilerini (oturum açma bilgileri, roller ve talepler dahil) saklar. ASP.NET Çekirdek Kimlik iki faktörlü kimlik doğrulamasını destekler.  Harici kimlik doğrulama sağlayıcıları da desteklenir, böylece kullanıcılar Microsoft, Google, Facebook veya Twitter gibi sağlayıcılardan gelen varolan kimlik doğrulama işlemlerini kullanarak oturum açabilir.

### <a name="authorization"></a>Yetkilendirme
Kimlik doğrulamadan sonra, hizmetlerin kullanıcı erişimini yetkilendirmesi veya kullanıcının neler yapabileceğini belirlemesi gerekir. Bu işlem, bir hizmetin API'leri bazı kimlik doğrulaması yapılan kullanıcılar için kullanılabilir hale getirmenize, ancak tümüne değil, tümüne olanak sağlamasına olanak tanır. Yetkilendirme ortogonal ve kimlik doğrulama bağımsız, hangi bir kullanıcı nın kim olduğunu tespit işlemidir. Kimlik doğrulama, geçerli kullanıcı için bir veya daha fazla kimlik oluşturabilir.

[ASP.NET Çekirdek yetkilendirme,](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/authorization-net-microservices-web-applications) kullanıcıların rollerine veya talepleri veya diğer buluşsal denetimleri içeren özel ilkelere dayalı olarak yapılabilir.

## <a name="restrict-and-secure-access-using-an-api-gateway"></a>API ağ geçidini kullanarak erişimi kısıtlama ve güvenli hale
Bulut uygulamalarının normalde kullanıcılar, cihazlar ve diğer uygulamalara tek giriş noktası sağlamak için bir ön uç ağ geçidine ihtiyacı vardır. ApI [ağ geçidi](/azure/architecture/microservices/gateway) istemciler ve hizmetler arasında yer alıyor ve uygulamanızın sağladığı tüm hizmetlerin giriş noktasıdır. İstemlerden hizmetlere istekleri yönlendirme, ters proxy olarak görür. Ayrıca kimlik doğrulama ve yetkilendirme, TLS sonlandırma ve oran sınırlama gibi çeşitli çapraz kesme görevleri gerçekleştirebilir. Bir ağ geçidi dağıtmazsanız, istemcilerin istekleri doğrudan ön uç hizmetlerine göndermesi gerekir.

Hizmet Kumaşı'nda ağ geçidi, [ASP.NET Core uygulaması](service-fabric-reliable-services-communication-aspnetcore.md)veya [Traefik,](https://docs.traefik.io/) [Event Hubs,](https://docs.microsoft.com/azure/event-hubs/) [IoT Hub](https://docs.microsoft.com/azure/iot-hub/)veya Azure [API Yönetimi](https://docs.microsoft.com/azure/api-management)gibi trafik girişi için tasarlanmış başka bir hizmet gibi herhangi bir devletsiz hizmet olabilir.

API Yönetimi, arka uç Hizmet Kumaşı hizmetlerinize zengin bir yönlendirme kuralları kümesiyle API'leri yayımlamanızı sağlayan Doğrudan Service Fabric ile tümleşir.  Arka uç hizmetlerine erişimi güvenli hale getirebilir, azaltma yı kullanarak DOS saldırılarını önleyebilir veya API anahtarlarını, JWT belirteçlerini, sertifikalarını ve diğer kimlik bilgilerini doğrulayabilirsiniz. Daha fazla bilgi için [Azure API Yönetimi'ne genel bakışla Hizmet Kumaşı'nı](service-fabric-api-management-overview.md)okuyun.

## <a name="manage-application-secrets"></a>Uygulama parolalarını yönetme
Sırlar, depolama bağlantı dizeleri, parolalar veya düz metinde işlenmemesi gereken diğer değerler gibi hassas bilgiler olabilir. Bu makalede, anahtarları ve sırları yönetmek için Azure Key Vault kullanır. Ancak, bir uygulamada sırları *kullanarak* bulut platformu-agnostik uygulamalar her yerde barındırılan bir kümeye dağıtılmak için izin verir.

Hizmet yapılandırma ayarlarını yönetmenin önerilen yolu [hizmet yapılandırma paketlerinden][config-package]geçer. Yapılandırma paketleri, sistem durumu doğrulama ve otomatik geri alma ile yönetilen yuvarlama yükseltmeleri yoluyla sürülür ve güncellenir. Bu, genel hizmet kesintisi olasılığını azalttığı ndan, genel yapılandırmaya tercih edilir. Şifreli sırlar istisna değildir. Service Fabric, sertifika şifrelemesini kullanarak yapılandırma paketi Settings.xml dosyasındaki değerleri şifrelemek ve çözmek için yerleşik özelliklere sahiptir.

Aşağıdaki diyagram, Bir Hizmet Kumaşı uygulamasında gizli yönetim için temel akışı göstermektedir:

![gizli yönetim genel bakış][overview]

Bu akışın dört ana adımı vardır:

1. Bir veri çözme sertifikası edinin.
2. Sertifikayı kümenize yükleyin.
3. Bir uygulamayı sertifikayla dağıtırken gizli değerleri şifreleyin ve bunları bir hizmetin Settings.xml yapılandırma dosyasına enjekte edin.
4. Aynı şifreleme sertifikası ile şifresini çözerek Settings.xml'den şifrelenmiş değerleri okuyun. 

[Azure Key Vault][key-vault-get-started] burada sertifikalar için güvenli bir depolama konumu ve Azure'daki Hizmet Kumaşı kümelerine sertifika yüklemenin bir yolu olarak kullanılır. Azure'a dağıtım yapmıyorsanız, Service Fabric uygulamalarındaki sırları yönetmek için Key Vault'u kullanmanız gerekmez.

Örneğin, [bkz. uygulama sırlarını yönet.](service-fabric-application-secret-management.md)

## <a name="secure-the-hosting-environment"></a>Barındırma ortamını güvenli hale
Azure Hizmet Kumaşı'nı kullanarak, kümede çalışan uygulamaları farklı kullanıcı hesapları altında güvenebilirsiniz. Hizmet Kumaşı, dosyalar, dizinler ve sertifikalar gibi kullanıcı hesapları altında dağıtım sırasında uygulamalar tarafından kullanılan kaynakların güvenliğini sağlamaya da yardımcı olur. Bu, paylaşılan barındırılan bir ortamda bile çalışan uygulamaları birbirinden daha güvenli hale getirir.

Uygulama bildirimi, hizmetin çalıştırılması ve güvenli kaynakları gerektiren güvenlik ilkelerini (kullanıcılar ve gruplar) bildirir.  Bu güvenlik ilkeleri, örneğin run-as, endpoint bağlama, paket paylaşımı veya güvenlik erişim ilkeleri gibi ilkelere başvurur.  İlkeler daha sonra uygulama bildiriminin **ServiceManifestImport** bölümündeki hizmet kaynaklarına uygulanır.

Büyükmüdürler bildirirken, birlikte yönetilecek her gruba bir veya daha fazla kullanıcı eklenebilecek kullanıcı grupları da tanımlayabilir ve oluşturabilirsiniz. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı olduğunda ve grup düzeyinde kullanılabilen belirli ortak ayrıcalıklara sahip olmaları gerektiğinde yararlıdır.

Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işleminin altında çalıştığı hesabın altında çalışır. Service Fabric ayrıca uygulamaları yerel bir kullanıcı hesabı veya uygulama bildiriminde belirtilen yerel sistem hesabı altında çalıştırma olanağı da sağlar. Daha fazla bilgi için [bkz.](service-fabric-application-runas-security.md)  Ayrıca, [bir hizmet başlangıç komut dosyasını yerel bir kullanıcı veya sistem hesabı olarak](service-fabric-run-script-at-service-startup.md)çalıştırabilirsiniz.

Windows bağımsız kümesinde Service Fabric çalıştırırken, [etkin dizin etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya grup yönetilen hizmet [hesapları](service-fabric-run-service-as-gmsa.md)altında bir hizmet çalıştırabilirsiniz.

## <a name="secure-containers"></a>Güvenli kapsayıcılar
Service Fabric, Windows veya Linux kümesindeki düğümlere (sürüm 5.7 veya daha yüksek) yüklenen bir sertifikaya erişmek için kapsayıcı içindeki hizmetler için bir mekanizma sağlar. Bu PFX sertifikası, uygulamanın veya hizmetin doğruluğunu doğrulamak veya diğer hizmetlerle güvenli iletişim sağlamak için kullanılabilir. Daha fazla bilgi için [bkz.](service-fabric-securing-containers.md)

Buna ek olarak, Service Fabric, Windows kapsayıcıları için gMSA 'yı (grup Yönetilen Hizmet Hesapları) da destekler. Daha fazla bilgi için windows [kapsayıcıları için gMSA'yı ayarlama'ya](service-fabric-setup-gmsa-for-windows-containers.md)bakın.

## <a name="secure-service-communication"></a>Güvenli servis iletişimi
Hizmet Kumaşı'nda, bir hizmet genellikle birden çok VM arasında dağıtılan Bir Hizmet Kumaşı kümesinde bir yerde çalışır. Service Fabric, hizmet iletişimlerinizi güvence altına almak için çeşitli seçenekler sunar.

[ASP.NET Core veya Java](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) web hizmetlerinizde HTTPS uç noktalarını etkinleştirebilirsiniz.

Ters proxy ve hizmetler arasında güvenli bağlantı kurarak güvenli kanalı sona erdirmeyi sağlayabilirsiniz. Güvenli hizmetlere bağlanma, yalnızca ters proxy HTTPS'yi dinleyecek şekilde yapılandırıldıklarında desteklenir. Ters proxy'yi yapılandırma hakkında daha fazla bilgi için [Azure Hizmet Kumaşı'nda Ters proxy'yi](service-fabric-reverseproxy.md)okuyun.  [Güvenli bir hizmete bağlanma,](service-fabric-reverseproxy-configure-secure-communication.md) ters proxy ve hizmetler arasında nasıl güvenli bağlantı kurulabileceğinizi açıklar.

Güvenilir Hizmetler uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz birkaç önceden oluşturulmuş iletişim yığını ve aracı sağlar. Hizmet remotingini [(C#](service-fabric-reliable-services-secure-communication.md) veya Java'da) [Java](service-fabric-reliable-services-secure-communication-java.md)veya [WCF](service-fabric-reliable-services-secure-communication-wcf.md)kullanırken güvenliği nasıl artırabileceğinizi öğrenin.

## <a name="encrypt-application-data-at-rest"></a>Uygulama verilerini istirahatte şifreleme
Azure'da çalışan Bir Hizmet Kumaşı kümesindeki her [düğüm türü](service-fabric-cluster-nodetypes.md) [sanal makine ölçeği kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)tarafından yedeklenir. Azure Resource Manager şablonunu kullanarak, Service Fabric kümesini oluşturan ölçek kümelerine veri diskleri ekleyebilirsiniz.  Hizmetleriniz verileri ekli bir veri diskine kaydederse, uygulama verilerinizi korumak için [bu veri disklerini şifreleyebilirsiniz.](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md)

<!--TO DO: Enable BitLocker on Windows standalone clusters?
TO DO: Encrypt disks on Linux clusters?-->


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Hizmet başlatmada bir kurulum komut dosyası çalıştırma](service-fabric-run-script-at-service-startup.md)
* [Hizmet bildiriminde kaynakları belirtin](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)
* [Küme güvenliği hakkında bilgi edinin](service-fabric-cluster-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/general/overview.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-and-service-security/overview.png