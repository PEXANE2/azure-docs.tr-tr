---
title: Azure Hizmet Kumaşı güvenliği için en iyi uygulamalar
description: Bu makalede, Azure Hizmet Kumaşı güvenliği için en iyi uygulamalar kümesi sağlanmaktadır.
author: unifycloud
ms.author: tomsh
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.date: 01/16/2019
ms.openlocfilehash: befe8945468d220a04ec7f0b515f22159cb72b0f
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549238"
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric güvenliği en iyi yöntemleri
Bir uygulamayı Azure'da dağıtmak hızlı, kolay ve uygun maliyetlidir. Bulut uygulamanızı üretime dağıtmadan önce, uygulamanızda güvenli kümeler uygulamak için temel ve önerilen en iyi uygulamalar listemizi inceleyin.

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric ayrıca bulut uygulamalarını geliştirme ve yönetme sürecinde karşılaşılan başlıca sorunların giderilmesini de sağlar. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar.

Her en iyi uygulama için, açıklarız:

-   En iyi uygulama ne olduğunu.
-   Neden en iyi uygulama uygulamak gerekir.
-   En iyi uygulamayı uygulamazsanız ne olabilir.
-   En iyi uygulamayı uygulamayı nasıl öğrenebilirsiniz.

Aşağıdaki Azure Hizmet Kumaşı güvenliği en iyi uygulamaları öneririz:

-   Güvenli kümeler oluşturmak için Azure Kaynak Yöneticisi şablonlarını ve Service Fabric PowerShell modüllerini kullanın.
-   X.509 sertifikalarını kullanın.
-   Güvenlik ilkelerini yapılandırın.
-   Güvenilir Aktörler güvenlik yapılandırmasını uygulayın.
-   Azure Hizmet Kumaşı için TLS yapılandırın.
-   Azure Hizmet Kumaşı ile ağ yalıtımı ve güvenliği kullanın.
-   Güvenlik için Azure Anahtar Kasası'nı yapılandırın.
-   Kullanıcıları rollere atama.


## <a name="best-practices-for-securing-your-clusters"></a>Kümelerinizi güvence altına almak için en iyi uygulamalar

Her zaman güvenli bir küme kullanın:
-   Sertifikaları kullanarak küme güvenliği uygulayın.
-   Azure Etkin Dizini (Azure AD) kullanarak istemci erişimi (yönetici ve salt okunur) sağlayın.

Otomatik dağıtımları kullanın:
-   Sırları oluşturmak, dağıtmak ve devirmek için komut dosyalarını kullanın.
-   Sırları Azure Key Vault'ta saklayın ve diğer tüm istemci erişimi için Azure AD'yi kullanın.
-   Sırları na insan erişimi için kimlik doğrulaması gerektirir.

Ayrıca, aşağıdaki yapılandırma seçeneklerini göz önünde bulundurun:
-   Azure Ağ Güvenlik Grupları (NSG'ler) kullanarak çevre ağları (arındırılmış bölgeler, DMZ'ler ve taranmış alt ağlar olarak da bilinir) oluşturun.
-   Sanal makinelere (VM) erişin veya Uzak Masaüstü Bağlantısı na sahip atlama sunucularını kullanarak kümenizi yönetin.

Kümeleriniz, özellikle üretimde bir küme çalışırken yetkisiz kullanıcıların bağlanmasını önlemek için güvenli olmalıdır. Güvenli olmayan bir küme oluşturmak mümkün olsa da, küme yönetim uç noktalarını genel internete açıkhale ederse anonim kullanıcılar kümenize bağlanabilir.

Küme güvenliğini çeşitli teknolojileri kullanarak uygulamak için üç [senaryo](../../service-fabric/service-fabric-cluster-security.md) vardır:

-   Düğümden düğüme güvenlik: Bu senaryo, Kümedeki Bilgisayarlar ve VM'ler arasındaki iletişimi güvence altına alar. Bu güvenlik biçimi, yalnızca kümeye katılmaya yetkili bilgisayarların kümedeki uygulamaları ve hizmetleri barındırabilmesini sağlar.
Bu senaryoda, Azure'da çalışan kümeler veya Windows'da çalışan bağımsız kümeler, Windows Server makineleri için [sertifika güvenliğini](../../service-fabric/service-fabric-windows-cluster-x509-security.md) veya [Windows güvenliğini](../../service-fabric/service-fabric-windows-cluster-windows-security.md) kullanabilir.
-   İstemciden düğüme güvenlik: Bu senaryo, bir Hizmet Dokusu istemcisi ile kümedeki tek tek düğümler arasındaki iletişimi güvence altına alar.
-   Role-Based Access Control (RBAC): Bu senaryo, kümeye erişen her yönetici ve kullanıcı istemcisi rolü için ayrı kimlikler (sertifikalar, Azure AD vb.) kullanır. Küme oluştururken rol kimliklerini belirtirsiniz.

>[!NOTE]
>**Azure kümeleri için güvenlik önerisi:** Kimlik doğrulama istemcileri ve sertifikaları düğümden düğüme güvenlik için doğrulamak için Azure AD güvenliğini kullanın.

Bağımsız bir Windows kümesini yapılandırmak [için, tek başına bir Windows kümesi için yapı ayarlarını yapılandır'a](../../service-fabric/service-fabric-cluster-manifest.md)bakın.

Güvenli bir küme oluşturmak için Azure Kaynak Yöneticisi şablonlarını ve Service Fabric PowerShell modüllerini kullanın.
Azure Kaynak Yöneticisi şablonlarını kullanarak güvenli bir Hizmet Dokusu kümesi oluşturmak için adım adım yönergeler için [bkz.](../../service-fabric/service-fabric-cluster-creation-via-arm.md)

Azure Kaynak Yöneticisi şablonundan yararlanın:
-   VM sanal sabit diskler (VHD'ler) için yönetilen depolamayı yapılandırmak için şablonu kullanarak kümenizi özelleştirin.
-   Kolay yapılandırma yönetimi ve denetimi için şablonu kullanarak kaynak grubunuzdaki değişiklikleri yönlendirin.

Küme yapılandırmanızı kod olarak ele aürün:
-   Dağıtım yapılandırmalarınızı kontrol ederken ayrıntılı olun.
-   Kaynaklarınızı doğrudan değiştirmek için örtük komutları kullanmaktan kaçının.

Service Fabric [uygulama yaşam döngüsünün](../../service-fabric/service-fabric-application-lifecycle.md) birçok yönü otomatik olabilir. [Service Fabric PowerShell modülü,](../../service-fabric/service-fabric-deploy-remove-applications.md#upload-the-application-package) Azure Hizmet Kumaşı uygulamalarını dağıtmak, yükseltmek, kaldırmak ve test etmek için ortak görevleri otomatikleştirir. Uygulama yönetimi için yönetilen API'ler ve HTTP API'leri de mevcuttur.

## <a name="use-x509-certificates"></a>X.509 sertifikalarını kullanma
X.509 sertifikalarını veya Windows güvenliğini kullanarak kümelerinizi her zaman güvene toplayarak güvence altına alanın. Güvenlik yalnızca küme oluşturma zamanında yapılandırılır. Küme oluşturulduktan sonra güvenliği açmak mümkün değildir.

[Küme sertifikası](../../service-fabric/service-fabric-windows-cluster-x509-security.md)belirtmek için **ClusterCredentialType** özelliğinin değerini X509 olarak ayarlayın. Dış bağlantılar için bir sunucu sertifikası belirtmek için **ServerCredentialType** özelliğini X509 olarak ayarlayın.

Buna ek olarak, aşağıdaki uygulamaları izleyin:
-   Doğru yapılandırılmış bir Windows Server sertifika hizmeti kullanarak üretim kümeleri için sertifikalar oluşturun. Sertifikaları onaylı bir sertifika yetkilisinden (CA) de alabilirsiniz.
-   Sertifika MakeCert.exe veya benzer bir araç kullanılarak oluşturulduysa, üretim kümeleri için asla geçici veya test sertifikası kullanmayın.
-   Test kümeleri için kendi imzalı bir sertifika kullanın, ancak üretim kümeleri için kullanmayın.

Küme güvenli değilse, herkes kümeye anonim olarak bağlanabilir ve yönetim işlemleri gerçekleştirebilir. Bu nedenle, X.509 sertifikalarını veya Windows güvenliğini kullanarak her zaman üretim kümelerini güvenli hale bulundurun.

X.509 sertifikalarını kullanma hakkında daha fazla bilgi edinmek [için, Hizmet Kumaşı kümesi için sertifika ekle veya kaldır'a](../../service-fabric/service-fabric-cluster-security-update-certs-azure.md)bakın.

## <a name="configure-security-policies"></a>Güvenlik ilkelerini yapılandırma
Service Fabric, uygulamalar tarafından kullanılan kaynakları da güvence altına alar. Dosyalar, dizinler ve sertifikalar gibi kaynaklar, uygulama dağıtıldığında kullanıcı hesaplarının altında depolanır. Bu özellik, paylaşılan barındırılan bir ortamda bile uygulamaları birbirinden daha güvenli hale getirir.

-   Active Directory etki alanı grubu veya kullanıcı kullanın: Etkin Dizin kullanıcısı veya grup hesabı için hizmeti kimlik bilgileri altında çalıştırın. Azure Etkin Dizini'ni değil, etki alanınızda Etkin Dizini'ni şirket içinde kullandığınızdan emin olun. Etki alanı kullanıcısı veya grubunu kullanarak izin verilen etki alanındaki diğer kaynaklara erişin. Örneğin, dosya paylaşımları gibi kaynaklar.

-   HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama: Hizmet bildirimi HTTP ile uç nokta kaynaklarını beyan ettiğinde bir hizmete **RunAs** ilkesi uygulamak için **SecurityAccessPolicy** özelliğini belirtin. HTTP uç noktalarına ayrılan bağlantı noktaları, hizmetin altında çalıştığı RunAs kullanıcı hesabı için doğru şekilde erişim kontrollü listelerdir. İlke ayarlı olmadığında, http.sys'nin hizmete erişimi yoktur ve istemciden gelen aramalarda hata lar alabilirsiniz.

Hizmet Dokusu kümesinde güvenlik ilkelerini nasıl kullanacağınızı öğrenmek [için uygulamanız için güvenlik ilkelerini yapılandır'a](../../service-fabric/service-fabric-application-runas-security.md)bakın.

## <a name="implement-the-reliable-actors-security-configuration"></a>Güvenilir Aktörler güvenlik yapılandırmasını uygulayın
Hizmet Kumaş Güvenilir Aktörler aktör tasarım desen bir uygulamadır. Herhangi bir yazılım tasarım deseni olduğu gibi, belirli bir desen kullanma kararı, bir yazılım sorununun desene uygun olup olmadığına bağlıdır.

Genel olarak, aşağıdaki yazılım sorunları veya güvenlik senaryoları için çözüm modellemeye yardımcı olmak için aktör tasarım deseni kullanın:
-   Sorun alanınız çok sayıda (binlerce veya daha fazla) küçük, bağımsız ve yalıtılmış durum ve mantık birimlerini içerir.
-   Bir dizi aktör arasında durum sorgulama dahil olmak üzere dış bileşenlerden önemli etkileşim gerektirmeyen tek iş parçacığı nesneleri ile çalışıyorsunuz.
-   Aktör örnekleriniz, G/Ç işlemleri düzenleyerek öngörülemeyen gecikmelerle arayanları engellemez.

Hizmet Kumaşı'nda aktörler Güvenilir Aktörler uygulama çerçevesinde uygulanmaktadır. Bu çerçeve aktör desen dayanmaktadır ve Hizmet Kumaş Güvenilir Hizmetler üstüne inşa [edilmiştir.](../../service-fabric/service-fabric-reliable-services-introduction.md) Yazdığınız her güvenilir aktör hizmeti, bölümlenmiş, durum lu güvenilir bir hizmettir.

Her aktör, .NET nesnesinin .NET türünün bir örneği olmasıyla aynı olan bir aktör türü örneği olarak tanımlanır. Örneğin, bir hesap makinesinin işlevselliğini uygulayan bir **aktör türü,** küme deki çeşitli düğümlere dağıtılan bu türdeki birçok aktöre sahip olabilir. Dağıtılan aktörlerin her biri benzersiz bir aktör tanımlayıcısı ile karakterizedir.

[Çoğaltma güvenlik yapılandırmaları](../../service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration.md) çoğaltma sırasında kullanılan iletişim kanalını güvence altına almak için kullanılır. Bu yapılandırma, hizmetlerin birbirlerinin çoğaltma trafiğini görmesini engeller ve kullanılabilir yüksek veri güvenliğini sağlar. Varsayılan olarak, boş bir güvenlik yapılandırma bölümü çoğaltma güvenliğini engeller.
Çoğalıcı yapılandırmaları, Aktör Durum Sağlayıcısı durumunu son derece güvenilir hale getirmekten sorumlu çoğaltıcıyı yapılandırır.

## <a name="configure-tls-for-azure-service-fabric"></a>Azure Hizmet Kumaşı için TLS yapılandır
Sunucu kimlik doğrulama işlemi, küme yönetimi uç noktalarını bir yönetim istemcisine [doğrular.](../../service-fabric/service-fabric-cluster-creation-via-arm.md) Yönetim istemcisi daha sonra gerçek kümeyle konuştuğunu fark eder. Bu sertifika ayrıca HTTPS yönetim API'si ve HTTPS üzerinden Service Fabric Explorer için bir [TLS](../../service-fabric/service-fabric-cluster-creation-via-arm.md) sağlar.
Kümeniz için özel bir etki alanı adı edinmeniz gerekir. Sertifika yetkilisinden sertifika istediğinizde, sertifikanın özne adı kümeniz için kullandığınız özel alan adıyla eşleşmelidir.

Bir uygulama için TLS yapılandırmak için öncelikle CA tarafından imzalanmış bir SSL/TLS sertifikası almanız gerekir. CA, TLS güvenlik amacıyla sertifika veren güvenilir bir üçüncü taraftır. Zaten SSL/TLS sertifikanız yoksa, SSL/TLS sertifikası satan bir şirketten sertifika almanız gerekir.

Sertifika, Azure'daki SSL/TLS sertifikaları için aşağıdaki gereksinimleri karşılamalıdır:
-   Sertifika özel bir anahtar içermelidir.

-   Sertifika anahtar değişimi için oluşturulmalı ve kişisel bilgi alışverişi (.pfx) dosyasına ihraç edilebilir olmalıdır.

-   Sertifikanın özne adı, bulut hizmetinize erişmek için kullanılan alan adıile eşleşmelidir.

    - Bulut hizmetinize erişmek için kullanmak üzere özel bir etki alanı adı edinin.
    - Hizmetinizin özel alan adıyla eşleşen bir konu adı olan bir CA'dan sertifika isteyin. Örneğin, özel alan adınız __contoso__**.com**ise, CA'nızdaki sertifikanın konu adı **.contoso.com** veya __www__**.contoso.com**olmalıdır.

    >[!NOTE]
    >__Cloudapp__**.net** etki alanı için CA'dan SSL/TLS sertifikası alamazsınız.

-   Sertifika en az 2.048 bit şifreleme kullanmalıdır.

HTTP protokolü güvenli değildir ve dinleme saldırılarına tabidir. HTTP üzerinden iletilen veriler, web tarayıcısından web sunucusuna veya diğer uç noktalar arasında düz metin olarak gönderilir. Saldırganlar, kredi kartı bilgileri ve hesap girişleri gibi HTTP aracılığıyla gönderilen hassas verileri ele geçirip görüntüleyebilir. Veriler HTTPS üzerinden bir tarayıcı üzerinden gönderildiğinde veya gönderildiğinde, SSL hassas bilgilerin şifrelenmelerini ve ele geçirildiğinden emin olmasını sağlar.

SSL/TLS sertifikalarını kullanma hakkında daha fazla bilgi edinmek [için Azure'daki bir uygulama için TLS Yapılandırma'ya](../../cloud-services/cloud-services-configure-ssl-certificate-portal.md)bakın.

## <a name="use-network-isolation-and-security-with-azure-service-fabric"></a>Azure Hizmet Kumaşı ile ağ yalıtımı ve güvenliği kullanma
[Azure Kaynak Yöneticisi şablonu](../../azure-resource-manager/templates/template-syntax.md) örnek olarak kullanarak 3 nodetype güvenli küme ayarlayın. Şablonu ve Ağ Güvenlik Gruplarını kullanarak gelen ve giden ağ trafiğini denetleyin.

Şablon, sanal makine ölçek kümelerinin her biri için bir NSG'ye sahiptir ve kümenin içinde ve dışında trafiği denetlemek için kullanılır. Kurallar, sistem hizmetleri ve şablonda belirtilen uygulama bağlantı noktaları için gerekli tüm trafiğe izin verecek şekilde varsayılan olarak yapılandırılır. Bu kuralları gözden geçirin ve uygulamalarınız için yeni kurallar eklemek de dahil olmak üzere ihtiyaçlarınıza uyacak değişiklikler yapın.

Daha fazla bilgi için [Azure Hizmet Kumaşı için Ortak ağ senaryolarına](../../service-fabric/service-fabric-patterns-networking.md)bakın.

## <a name="set-up-azure-key-vault-for-security"></a>Güvenlik için Azure Anahtar Kasası'nı ayarlama
Service Fabric, bir kümeyi ve uygulamalarını güvence altına almak için kimlik doğrulaması ve şifreleme sağlamak için sertifikalar kullanır.

Service Fabric, bir kümeyi güvenli hale getirmek ve uygulama güvenliği özellikleri sağlamak için X.509 sertifikalarını kullanır. Azure'da Hizmet Kumaşı [kümeleri için sertifikaları yönetmek](../../service-fabric/service-fabric-cluster-security-update-certs-azure.md) için Azure Key Vault'u kullanırsınız. Kümeleri oluşturan Azure kaynak sağlayıcısı, sertifikaları önemli bir kasadan çeker. Sağlayıcı, küme Azure'da dağıtıldığında sertifikaları VM'lere yükler.

[Azure Anahtar Kasası,](../../key-vault/key-vault-secure-your-key-vault.md)Hizmet Kumaşı kümesi ile sertifikaları kullanan kaynak sağlayıcısı arasında bir sertifika ilişkisi vardır. Küme oluşturulduğunda, sertifika ilişkisi hakkındaki bilgiler anahtar kasasında depolanır.

Anahtar kasasını kurmak için iki temel adım vardır:
1. Anahtar kasanız için özel olarak bir kaynak grubu oluşturun.

    Anahtar kasasını kendi kaynak grubuna koymanızı öneririz. Bu eylem, depolama, bilgi işlem veya kümenizi içeren grup gibi diğer kaynak grupları kaldırılırsa anahtarlarınızın ve sırlarınızın kaybını önlemeye yardımcı olur. Anahtar kasanızı içeren kaynak grubu, onu kullanan kümeyle aynı bölgede olmalıdır.

2. Yeni kaynak grubunda önemli bir kasa oluşturun.

    Dağıtım için anahtar kasası etkinleştirilmelidir. Bilgi işlem kaynak sağlayıcısı daha sonra sertifikaları kasadan alabilir ve VM örneklerine yükleyebilir.

Anahtar kasasını nasıl ayarlayınız hakkında daha fazla bilgi edinmek için Azure [Anahtar Kasası nedir?](../../key-vault/key-vault-overview.md)

## <a name="assign-users-to-roles"></a>Kullanıcıları rollere atama
Kümenizi temsil edecek uygulamaları oluşturduktan sonra, kullanıcılarınızı Service Fabric tarafından desteklenen rollere atayın: salt okunur ve yönetici. Azure portalını kullanarak bu rolleri atayabilirsiniz.

>[!NOTE]
> Hizmet Kumaşı'ndaki rolleri kullanma hakkında daha fazla bilgi için [Hizmet Kumaşı istemcileri için Rol Tabanlı Erişim Denetimi'ne](../../service-fabric/service-fabric-cluster-security-roles.md)bakın.

Azure Hizmet Kumaşı, bir [Hizmet Kumaşı kümesine](../../service-fabric/service-fabric-cluster-creation-via-arm.md)bağlı istemciler için iki erişim denetim türünü destekler: yönetici ve kullanıcı. Küme yöneticisi, farklı kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamak için erişim denetimini kullanabilir. Erişim denetimi kümeyi daha güvenli hale getirir.

## <a name="next-steps"></a>Sonraki adımlar

- [Service Fabric güvenlik kontrol listesi](service-fabric-checklist.md)
- Service Fabric [geliştirme ortamınızı](../../service-fabric/service-fabric-get-started.md)ayarlayın.
- Service [Fabric destek seçenekleri](../../service-fabric/service-fabric-support.md)hakkında bilgi edinin.
