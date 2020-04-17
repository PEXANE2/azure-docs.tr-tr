---
title: Azure Hizmet Kumaşı kümesini güvenli hale
description: Azure Hizmet Kumaşı kümesinin güvenlik senaryoları ve bunları uygulamak için kullanabileceğiniz çeşitli teknolojiler hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 08/14/2018
ms.custom: sfrev
ms.openlocfilehash: c43cfbd4468a64867d50482d9c8055622602f159
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461591"
---
# <a name="service-fabric-cluster-security-scenarios"></a>Hizmet Kumaş küme güvenlik senaryoları

Azure Hizmet Kumaşı kümesi, sahip olduğunuz bir kaynaktır. Yetkisiz kullanıcıların bu kümelere bağlanmasını önlemeye yardımcı olmak için kümelerinizi güvenli hale getirmek sizin sorumluluğunuzdadır. Kümede üretim iş yüklerini çalıştırırken güvenli küme özellikle önemlidir. Güvenli olmayan bir küme oluşturmak mümkündür, ancak küme yönetim uç noktalarını genel internete açıklarsa, anonim kullanıcılar bu kümeye bağlanabilir. Güvenli olmayan kümeler üretim iş yükleri için desteklenmez. 

Bu makalede, Azure kümeleri ve bağımsız kümeler için güvenlik senaryolarına ve bunları uygulamak için kullanabileceğiniz çeşitli teknolojilere genel bir bakış:

* Düğümden düğüme güvenlik
* İstemciden düğüme güvenlik
* Rol Tabanlı Erişim Denetimi (RBAC)

## <a name="node-to-node-security"></a>Düğümden düğüme güvenlik

Düğümden düğüme güvenlik, kümedeki VM'ler veya bilgisayarlar arasındaki iletişimin güvenli hale ilmesine yardımcı olur. Bu güvenlik senaryosu, kümeye yalnızca kümeye katılmaya yetkili bilgisayarların kümedeki uygulamaları ve hizmetleri barındırmaya katılmasını sağlar.

![Düğümden düğüme iletişim diyagramı][Node-to-Node]

Azure'da çalışan kümeler ve Windows'da çalışan bağımsız kümeler, Windows Server bilgisayarlar için [sertifika güvenliğini](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows güvenliğini](https://msdn.microsoft.com/library/ff649396.aspx) kullanabilir.

### <a name="node-to-node-certificate-security"></a>Düğümden düğüme sertifika güvenliği

Service Fabric, küme oluşturduğunuzda düğüm türü yapılandırmasının bir parçası olarak belirttiğiniz X.509 sunucu sertifikalarını kullanır. Bu makalenin sonunda, bu sertifikaların ne olduğuna ve bunları nasıl elde edebileceğinize veya oluşturabileceğinize kısa bir genel bakış görebilirsiniz.

Azure portalında, Azure Kaynak Yöneticisi şablonu kullanarak veya bağımsız bir JSON şablonu kullanarak küme oluştururken sertifika güvenliğini ayarlayın. Service Fabric SDK'nın varsayılan davranışı, sertifikayı gelecekteki süresi dolan sertifikaya en uzak sertifikaya sahip olarak dağıtmak ve yüklemektir; klasik davranış, birincil ve ikincil sertifikaların tanımlanmasına, el ile başlatılan rolover'lara izin verebilmek için izin verir ve yeni işlevler üzerinde kullanılması önerilmez. Kullanılacak birincil sertifikalar, gelecekteki son kullanma tarihine en uzak olana sahiptir, yönetici istemciden ve [istemciden düğüme güvenlik](#client-to-node-security)için ayarladığınız salt okunur istemci sertifikalarından farklı olmalıdır.

Azure için bir kümede sertifika güvenliği nasıl ayarlayılabildiğini öğrenmek için [bkz.](service-fabric-cluster-creation-via-arm.md)

Bağımsız bir Windows Server kümesi için bir kümede sertifika güvenliğini nasıl ayarlayatış edebilirsiniz öğrenmek için, [X.509 sertifikalarını kullanarak Windows'da bağımsız bir kümeyi güvenli](service-fabric-windows-cluster-x509-security.md)olarak ele 'e bakın.

### <a name="node-to-node-windows-security"></a>Düğümden düğüme Windows güvenliği

Bağımsız bir Windows Server kümesi için Windows güvenliğini nasıl ayarlayatılabildiğini öğrenmek için Windows [security'yi kullanarak Windows'da bağımsız bir kümeyi güvenli](service-fabric-windows-cluster-windows-security.md)olarak ele abakın.

## <a name="client-to-node-security"></a>İstemciden düğüme güvenlik

İstemciden düğüme güvenlik istemcilerin kimliğini doğrular ve kümedeki istemci ve tek tek düğümler arasındaki iletişimin güvenli hale ilmesine yardımcı olur. Bu tür güvenlik, kümeye yalnızca yetkili kullanıcıların kümeye ve kümede dağıtılan uygulamalara erişebilmesini sağlamaya yardımcı olur. İstemciler, Windows güvenlik kimlik bilgileri veya sertifika güvenlik kimlik bilgileri aracılığıyla benzersiz olarak tanımlanır.

![İstemciden düğüme iletişim diyagramı][Client-to-Node]

Azure'da çalışan kümeler ve Windows'da çalışan bağımsız kümeler sertifika [güvenliğini](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows güvenliğini](https://msdn.microsoft.com/library/ff649396.aspx)kullanabilir.

### <a name="client-to-node-certificate-security"></a>İstemciden düğüme sertifika güvenliği

Azure portalında, Kaynak Yöneticisi şablonu kullanarak veya bağımsız bir JSON şablonu kullanarak küme oluştururken istemciden düğüme sertifika güvenliğini ayarlayın. Sertifikayı oluşturmak için bir yönetici istemci sertifikası veya kullanıcı istemci sertifikası belirtin. En iyi uygulama olarak, belirttiğiniz yönetici istemci ve kullanıcı istemcisi [sertifikaları, düğümden düğüme güvenlik](#node-to-node-security)için belirttiğiniz birincil ve ikincil sertifikalardan farklı olmalıdır. Küme sertifikaları istemci yönetici sertifikaları ile aynı haklara sahiptir. Ancak, bunlar yalnızca küme tarafından değil, bir güvenlik en iyi uygulama olarak yönetim kullanıcıları tarafından kullanılmalıdır.

Yönetici sertifikasını kullanarak kümeye bağlanan istemciler yönetim yeteneklerine tam erişime sahiptir. Salt okunur kullanıcı istemcisi sertifikasını kullanarak kümeye bağlanan istemciler yalnızca yönetim yeteneklerine erişim edilebilmiştir. Bu sertifikalar, bu makalede daha sonra açıklanan RBAC için kullanılır.

Azure için bir kümede sertifika güvenliği nasıl ayarlayılabildiğini öğrenmek için [bkz.](service-fabric-cluster-creation-via-arm.md)

Bağımsız bir Windows Server kümesi için bir kümede sertifika güvenliğini nasıl ayarlayatış edebilirsiniz öğrenmek için, [X.509 sertifikalarını kullanarak Windows'da bağımsız bir kümeyi güvenli](service-fabric-windows-cluster-x509-security.md)olarak ele 'e bakın.

### <a name="client-to-node-azure-active-directory-security-on-azure"></a>Azure'da istemciden düğüme Azure Etkin Dizin güvenliği

Azure AD, kuruluşların (kiracı olarak da bilinir) uygulamalara kullanıcı erişimini yönetmesini sağlar. Uygulamalar, web tabanlı oturum açma kullanıcı ui'si ve yerel istemci deneyimi olanlara ayrılır. Daha önce bir kiracı oluşturmadıysanız, [Azure Etkin Dizin kiracısını nasıl edinirsiniz'i][active-directory-howto-tenant]okuyarak başlayın.

Bir Service Fabric küme web tabanlı [Hizmet Kumaş Explorer][service-fabric-visualizing-your-cluster] ve Visual [Studio][service-fabric-manage-application-in-visual-studio]dahil olmak üzere yönetim işlevselliği için çeşitli giriş noktaları sunuyor. Sonuç olarak, kümeye, bir web uygulamasına ve bir yerel uygulamaya erişimi denetlemek için iki Azure REKLAM uygulaması oluşturursunuz.

Azure'da çalışan kümeler için, Azure Etkin Dizini (Azure AD) kullanarak yönetim bitiş noktalarına erişimi de güvence altına alabilirsiniz. Gerekli Azure AD yapılarını nasıl oluşturabileceğinizi ve kümeoluştururken bunları nasıl doldurabileceğinizi öğrenmek [için, istemcilerin kimliğini doğrulamak için Azure AD'yi ayarla'ya](service-fabric-cluster-creation-setup-aad.md)bakın.

## <a name="security-recommendations"></a>Güvenlik önerileri

Azure'da barındırılan ortak bir ağda dağıtılan Hizmet Kumaşı kümeleri için istemciden düğüme karşılıklı kimlik doğrulaması için öneri şudur:

* İstemci kimliği için Azure Etkin Dizini'ni kullanma
* Sunucu kimliği ve TLS şifreleme için bir sertifika http iletişim

Azure'da barındırılan ortak bir ağda dağıtılan Hizmet Kumaşı kümeleri için düğümden düğüme güvenlik önerisi, düğümlerin kimliğini doğrulamak için bir Küme sertifikası kullanmaktır.

Bağımsız Windows Server kümeleri için, Windows Server 2012 R2 ve Windows Active Directory'niz varsa, Windows security'yi grup Yönetilen Hizmet Hesapları ile kullanmanızı öneririz. Aksi takdirde, Windows hesapları ile Windows güvenlik kullanın.

## <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)

Farklı kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamak için erişim denetimini kullanabilirsiniz. Bu, kümenin daha güvenli hale getirmeye yardımcı olur. Bir kümeye bağlanan istemciler için iki erişim denetimi türü desteklenir: Yönetici rolü ve Kullanıcı rolü.

Yönetici rolüatanan kullanıcılar, okuma ve yazma özellikleri de dahil olmak üzere yönetim yeteneklerine tam erişime sahiptir. Varsayılan olarak, Kullanıcı rolüatanan kullanıcılar yalnızca yönetim yeteneklerine (örneğin sorgu yetenekleri) erişimi okumuştur. Ayrıca uygulamaları ve hizmetleri de çözebilirler.

Küme oluştururken Yönetici ve Kullanıcı istemcisi rollerini ayarlayın. Her rol türü için ayrı kimlikler (örneğin, sertifikaları veya Azure AD'yi kullanarak) sağlayarak roller atayın. Varsayılan erişim denetimi ayarları ve varsayılan ayarların nasıl değiştirilen hakkında daha fazla bilgi için [Hizmet Dokusu istemcileri için Rol Tabanlı Erişim Denetimi'ne](service-fabric-cluster-security-roles.md)bakın.

## <a name="x509-certificates-and-service-fabric"></a>X.509 sertifikaları ve Servis Kumaşı

X.509 dijital sertifikalar genellikle istemcileri ve sunucuları doğrulamak için kullanılır. İletileri şifrelemek ve dijital olarak imzalamak için de kullanılırlar. Service Fabric, bir kümeyi güvenli hale getirmek ve uygulama güvenliği özellikleri sağlamak için X.509 sertifikalarını kullanır. X.509 dijital sertifikalar hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms731899.aspx) Azure'da Hizmet Kumaşı kümeleri için sertifikaları yönetmek için [Key Vault'u](../key-vault/general/overview.md) kullanırsınız.

Göz önünde bulundurulması gereken bazı önemli şeyler:

* Üretim iş yüklerini çalıştıran kümeler için sertifikalar oluşturmak için doğru yapılandırılmış bir Windows Server sertifika hizmeti veya onaylı bir [sertifika yetkilisinden (CA)](https://en.wikipedia.org/wiki/Certificate_authority)bir sertifika kullanın.
* Üretim ortamında MakeCert.exe gibi araçları kullanarak oluşturduğunuz geçici veya test sertifikalarını asla kullanmayın.
* Kendi imzalı bir sertifika kullanabilirsiniz, ancak yalnızca bir test kümesinde. Üretimde kendi imzalı sertifika kullanmayın.
* Sertifika parmak izini oluştururken SHA1 parmak izi oluşturduğundan emin olun. SHA1, İstemci ve Küme sertifikası parmak izlerini yapılandırırken kullanılan şeydir.

### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)

Bu sertifikalar (bir birincil ve isteğe bağlı olarak ikincil) bir kümeyi güvenli ve yetkisiz erişimi önlemek için gereklidir. Bu sertifikalar küme ve sunucu kimlik doğrulaması sağlar.

Küme kimlik doğrulaması küme federasyonu için düğümden düğüme iletişimi doğrular. Kümeye yalnızca bu sertifikayla kimliklerini kanıtlayabilen düğümler katılabilir. Sunucu kimlik doğrulaması küme yönetimi uç noktalarını bir yönetim istemcisine doğru lar, böylece yönetim istemcisi 'ortadaki adam' ile değil, gerçek kümeyle konuştuğunu bilir. Bu sertifika ayrıca HTTPS yönetim API'si ve HTTPS üzerinden Service Fabric Explorer için bir TLS sağlar. İstemci veya düğüm bir düğümü doğruladığında, ilk denetimlerden biri **Özne** alanındaki ortak adın değeridir. Bu ortak ad veya sertifikaların Özne Alternatif Adları (SN'ler) izin verilen ortak adlar listesinde bulunmalıdır.

Sertifika aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika özel bir anahtar içermelidir. Bu sertifikalar genellikle .pfx veya .pem uzantıları vardır  
* Sertifika, Kişisel Bilgi Alışverişi (.pfx) dosyasına ihraç edilebilir anahtar değişimi için oluşturulmalıdır.
* **Sertifikanın özne adı, Hizmet Kumaşı kümesine erişmek için kullandığınız etki alanıyla eşleşmelidir.** Bu eşleştirme kümenin HTTPS yönetim bitiş noktası ve Hizmet Kumaş Explorer için bir TLS sağlamak için gereklidir. *.cloudapp.azure.com etki alanı için bir sertifika yetkilisinden (CA) TLS/SSL sertifikası alamazsınız. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Göz önünde bulundurulması gereken diğer bazı şeyler:

* **Konu** alanı nın birden çok değeri olabilir. Her değer, değer türünü belirtmek için bir başlatma ile önceden belirlenmiştir. Genellikle, başlatma **CN** *(ortak ad*için); örneğin, **CN =\.www contoso.com**.
* **Konu** alanı boş olabilir.
* İsteğe bağlı **Konu Alternatif Adı** alanı doldurulursa, hem sertifikanın ortak adı hem de SAN başına bir giriş olmalıdır. Bunlar **DNS Adı** değerleri olarak girilir. SN'leri olan sertifikaları nasıl oluşturacağınızı öğrenmek [için, güvenli bir LDAP sertifikasına Özne Alternatif Adı nasıl ekleyeceğinizi](https://support.microsoft.com/kb/931351)öğrenin.
* Sertifikanın **Amaçlanan Amaçlar** alanının değeri, Sunucu Kimlik **Doğrulaması** veya **İstemci Kimlik Doğrulaması**gibi uygun bir değer içermelidir.

### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)

Uygulama güvenliği amacıyla bir kümeye herhangi bir sayıda ek sertifika yüklenebilir. Kümenizi oluşturmadan önce, düğümlere bir sertifika nın yüklenmesini gerektiren uygulama güvenlik senaryolarını düşünün:

* Uygulama yapılandırma değerlerinin şifresini şifreleme ve çözme.
* Çoğaltma sırasında düğümler arasında veri şifreleme.

İster Linux ister Windows kümeleri olsun, güvenli kümeler oluşturma kavramı aynıdır.

### <a name="client-authentication-certificates-optional"></a>İstemci kimlik doğrulama sertifikaları (isteğe bağlı)

Yönetici veya kullanıcı istemciişlemleri için herhangi bir sayıda ek sertifika belirtilebilir. Ortak kimlik doğrulaması gerektiğinde istemci bu sertifikayı kullanabilir. İstemci sertifikaları genellikle bir üçüncü taraf CA tarafından verilmez. Bunun yerine, geçerli kullanıcı konumunun Kişisel deposu genellikle bir kök yetkilisi tarafından yerleştirilen istemci sertifikalarını içerir. Sertifika, **İstemci Kimlik Doğrulaması'nın** **Amaçlanan Amaçları** na sahip olmalıdır.  

Varsayılan olarak küme sertifikası yönetici istemci ayrıcalıkları vardır. Bu ek istemci sertifikaları kümeye yüklenmemeli, ancak küme yapılandırmasında izin verildiği belirtilir.  Ancak, kümeye bağlanmak ve herhangi bir işlemi gerçekleştirmek için istemci sertifikalarının istemci makinelere yüklenmesi gerekir.

> [!NOTE]
> Service Fabric kümesindeki tüm yönetim işlemleri sunucu sertifikaları gerektirir. İstemci sertifikaları yönetim için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonu kullanarak Azure'da küme oluşturma](service-fabric-cluster-creation-via-arm.md)
* [Azure portalını kullanarak küme oluşturma](service-fabric-cluster-creation-via-portal.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png

[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
