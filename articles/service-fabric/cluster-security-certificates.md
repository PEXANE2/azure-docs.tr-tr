---
title: Service Fabric kümesinde X. 509.440 sertifika tabanlı kimlik doğrulaması
description: Service Fabric kümelerinde sertifika tabanlı kimlik doğrulaması ve sertifikayla ilgili sorunları algılama, azaltma ve çözme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/16/2020
ms.custom: sfrev
ms.openlocfilehash: 699015e322c599dea996b3a8b9dbc0a4589440ab
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429674"
---
# <a name="x509-certificate-based-authentication-in-service-fabric-clusters"></a>Service Fabric kümelerinde X. 509.440 sertifika tabanlı kimlik doğrulaması

Bu makalede, [küme güvenliği Service Fabric](service-fabric-cluster-security.md)giriş açıklanır ve Service Fabric kümelerinde sertifika tabanlı kimlik doğrulamasının ayrıntılarına gidersiniz. Okuyucunun temel güvenlik kavramlarını öğrendiğini ve Service Fabric bir kümenin güvenliğini denetlemek için sunduğu denetimlerle ilgili olduğunu varsayalım.  

Bu başlık altında yer almayan konular:

* Sertifika tabanlı kimlik doğrulaması temelleri
* Kimlikler ve ilgili rolleri
* Sertifika yapılandırma kuralları
* Sorun giderme ve sık sorulan sorular

## <a name="certificate-based-authentication-basics"></a>Sertifika tabanlı kimlik doğrulaması temelleri
Kısa bir yenileyici olarak, güvenlik 'de bir sertifika, bir varlıkla ilgili bilgileri bir asimetrik şifreleme anahtarı sahipliğiyle bağlamaya yönelik bir araç ve bu nedenle ortak anahtar şifrelemesi için çekirdek bir yapı oluşturur. Bir sertifika tarafından temsil edilen anahtarlar, verileri korumak veya anahtar sahiplerini sağlamak için kullanılabilir; bir ortak anahtar altyapısı (PKI) sistemi ile birlikte kullanıldığında, bir sertifika, bir İnternet etki alanının sahipliği veya sertifikayı veren (sertifika yetkilisi veya CA olarak bilinir) tarafından kendisine verilen belirli ayrıcalıklar gibi ek nitelikleri temsil edebilir. Sertifikaların yaygın bir uygulaması, Aktarım Katmanı Güvenliği (TLS) şifreleme protokolünü destekleyerek, bir bilgisayar ağı üzerinden güvenli iletişimler sağlar. Özellikle, istemci ve sunucu, iletişiminin gizliliğini ve bütünlüğünü sağlamak ve ayrıca karşılıklı kimlik doğrulaması yapmak için sertifikaları kullanır.

Service Fabric, bir kümenin temel katmanı (Federasyon), güvenilir ve güvenli bir katılan düğüm ağı elde etmek için TLS 'de (diğer protokoller arasında) da oluşturulur. Service Fabric Istemci API 'Leri aracılığıyla kümeyle bağlantı kurmak, trafiği korumak ve tarafların kimliklerini sağlamak için TLS kullanır. Özellikle, Service Fabric kimlik doğrulaması için kullanıldığında, aşağıdaki talepleri kanıtlamak için bir sertifika kullanılabilir: a) sertifikanın SHA-1 karması (' parmak izi '), küme tanımına dahil edilen bir bildirimle eşleşiyor veya c) sertifikanın ayırt edici konu ortak adı, küme tanımına dahil olan bir bildirimle eşleşiyor ve sertifikanın veren bilinen veya güvenilir.

Yukarıdaki listede ' b ', ' parmak izi sabitleme ' olarak bilinir; Bu durumda, bildirim belirli bir sertifikayı ifade eder ve kimlik doğrulama düzeninin gücü şirket içinde aynı karma değeri üreten bir sertifikayı başka bir şekilde oluşturan, ancak aynı zamanda iyi biçimlendirilmiş bir nesne olmaya devam eder. ' C ' öğesi, bir sertifika bildirmesinin alternatif bir formunu temsil eder. Bu, düzenin kuvvetinin sertifika konusunun ve veren yetkilinin birleşimine göre bekletilme biçimidir. Bu durumda, bildirim bir sertifika sınıfına başvurur-aynı özelliklere sahip herhangi iki sertifika tamamen eşdeğer olarak kabul edilir. 

Aşağıdaki bölümler, Service Fabric çalışma zamanının küme güvenliğini sağlamak için sertifikaları nasıl kullandığını ve doğrulamayacağını açıklamaktadır.

## <a name="identities-and-their-respective-roles"></a>Kimlikler ve ilgili rolleri
Kimlik doğrulama ayrıntılarına girmeden veya iletişim kanallarının güvenliğini sağlamaktan önce, katılan aktörleri ve bir kümede oynadıkları ilgili rolleri listelemek önemlidir:
- ' sistem ' olarak adlandırılan Service Fabric çalışma zamanı: kümeyi temsil eden soyutlamalar ve işlevler sağlayan hizmet kümesi. Sistem örnekleri arasındaki küme içi iletişime başvururken, ' küme kimliği ' terimini kullanacağız. küme dışından gelen trafiğin alıcı/hedefi olarak kümeye başvurulduğunda, ' sunucu kimliği ' terimini kullanacağız.
- barındırılan uygulamalar, ' uygulamalar ' olarak adlandırılır: kümede düzenlenmiş ve yürütülen küme sahibi tarafından verilen kod
- istemciler: küme yapılandırmasına göre, bir kümedeki işlevlere bağlanmasına ve bu işlevleri çalıştırmasına izin verilen varlıklar. Sırasıyla ' user ' ve ' admin' arasındaki iki düzey ayrıcalıkların ayırt ettik. Bir ' Kullanıcı ' istemcisi öncelikle salt okuma işlemlerine (ancak tüm salt okunurdur) kısıtlanır, ancak bir ' admin' istemcisinin kümenin işlevselliğine sınırsız erişimi vardır. (Daha fazla ayrıntı için [Service Fabric kümesindeki güvenlik rollerine](service-fabric-cluster-security-roles.md)bakın.)
- (Yalnızca Azure) Service Fabric kümelerinin işlem ve yönetimine yönelik denetimleri düzenleyen ve sunan Service Fabric Hizmetleri, yalnızca ' Service ' olarak adlandırılır. Ortama bağlı olarak, ' hizmet ' Azure Service Fabric kaynak sağlayıcısına veya Service Fabric ekibi tarafından sağlanan ve çalıştırılan diğer kaynak sağlayıcılarına başvurabilir.

Güvenli bir kümede, bu rollerin her biri, önceden tanımlanmış bir rol adının ve ilgili kimlik bilgisinin eşleştirmesi olarak belirtilen kendi, farklı bir kimlikle yapılandırılabilir. Service Fabric, kimlik bilgilerinin sertifika veya etki alanı tabanlı hizmet sorumlusu olarak bildirimini destekler. (Windows-/Kerberos-based kimlikleri de desteklenir, ancak bu makalenin kapsamı dışındadır; [Service Fabric kümelerinde Windows tabanlı güvenliğe](service-fabric-windows-cluster-windows-security.md)bakın.) Azure kümelerinde, istemci rolleri de [Azure Active Directory tabanlı kimlik](service-fabric-cluster-creation-setup-aad.md)olarak bildirilemez.

Service Fabric çalışma zamanı, yukarıda belirtildiği gibi, bir kümede iki düzeyde ayrıcalık tanımlar: ' admin' ve ' Kullanıcı '. Bir yönetici istemcisi ve ' sistem ' bileşeni, her ikisi de ' admin' ayrıcalıklarıyla çalışır ve bu nedenle birbirinden ayırt edilemez. Kümede/üzerine bir bağlantı kurulduktan sonra, kimliği doğrulanmış bir arayan, sonraki yetkilendirme için temel olarak iki rolden biri olan Service Fabric çalışma zamanı tarafından verilir. Kimlik doğrulamasını aşağıdaki bölümlerde ayrıntılı olarak inceleyeceğiz.

## <a name="certificate-configuration-rules"></a>Sertifika yapılandırma kuralları
### <a name="validation-rules"></a>Doğrulama kuralları
Bir Service Fabric kümesinin güvenlik ayarları, ilke bölümünde aşağıdaki noktaları anlatmaktadır:
- kimlik doğrulama türü; Bu, kümenin bir oluşturma zamanı ve sabit özelliğidir. Bu ayarların örnekleri şunlardır ' ClusterCredentialType ', ' ServerCredentialType ' ve izin verilen değerler ' none ', ' x509 ' veya ' Windows '. Bu makale, x509 türü kimlik doğrulamasına odaklanır.
- (kimlik doğrulama) doğrulama kuralları; Bu ayarlar, küme sahibi tarafından ayarlanır ve belirli bir rol için hangi kimlik bilgilerinin kabul edileceği açıklanır. Örnekler aşağıda ayrıntılı olarak incelenmeyecektir.
- kimlik doğrulamasının sonucunu ince ayar veya güvenle değiştirmek için kullanılan ayarlar; Buraya örnek olarak, sertifika iptal listelerinin uygulanmasını kısıtlama (de) dahildir.

> [!NOTE]
> Aşağıda belirtilen küme yapılandırması örnekleri, bu makalede açıklanan Service Fabric işlevselliğini destekleyen en iyi biçimde, XML biçiminde küme bildiriminde yer aldığı, XML biçimindeki derleme bildirimidir. Aynı ayarlar doğrudan bir küme tanımının JSON temsillerine, tek başına bir JSON kümesi bildirimine veya bir Azure kaynak şablonu ' na göre ifade edilebilir.

Bir sertifika doğrulama kuralı aşağıdaki öğeleri içerir:
- karşılık gelen rol: istemci, yönetici istemcisi (ayrıcalıklı rol)
- rol için kabul edilen, parmak izi veya konu ortak adı tarafından belirtilen kimlik bilgisi

#### <a name="thumbprint-based-certificate-validation-declarations"></a>Parmak izi tabanlı sertifika doğrulama bildirimleri
Parmak izi tabanlı doğrulama kuralları söz konusu olduğunda, bir çağıran tarafından sunulan ve kümeye bir bağlantı isteyen kimlik bilgileri aşağıdaki şekilde doğrulanacak:
  - kimlik bilgisi geçerli, iyi biçimlendirilmiş bir sertifikadır: zinciri oluşturulabilir, imza eşleşiyor
  - sertifika zaman geçerli (NotBefore <= Now < NotAfter)
  - sertifikanın SHA-1 karması, tüm boşluk hariç, büyük/küçük harfe duyarsız dize karşılaştırması bildirimiyle eşleşir

Zincir oluşturma veya doğrulama sırasında karşılaşılan herhangi bir güven hatası, süre sonu olan sertifikalar dışında parmak izi tabanlı bildirimler için bastırılır, ancak bu durum için sağlama de mevcut olur. Özellikle, ile ilgili hatalar: iptal durumu bilinmeyen veya çevrimdışı, güvenilmeyen kök, geçersiz anahtar kullanımı, kısmi zincir önemli olmayan hatalar olarak kabul edilir; Şirket içi, bu durumda, sertifikanın yalnızca bir anahtar çifti için bir zarf olduğu, güvenlik, küme sahibinin özel anahtarı korumak için de konum ölçüsünde ayarlanmış olması durumunda yer alıyor.

Bir küme bildiriminin aşağıdaki alıntısında, bu tür bir parmak izi tabanlı doğrulama kuralları kümesi vardır:

```xml
<Section Name="Security">
  <Parameter Name="ClusterCredentialType" Value="X509" />
  <Parameter Name="ServerAuthCredentialType" Value="X509" />
  <Parameter Name="AdminClientCertThumbprints" Value="d5ec...4264" />
  <Parameter Name="ClientCertThumbprints" Value="7c8f...01b0" />
  <Parameter Name="ClusterCertThumbprints" Value="abcd...1234,ef01...5678" />
  <Parameter Name="ServerCertThumbprints" Value="ef01...5678" />
</Section>
```

Yukarıdaki girdilerden her biri, daha önce açıklandığı gibi belirli bir kimliğe başvurur; her giriş Ayrıca, virgülle ayrılmış dizeler listesi olarak birden çok değer belirtilmesine izin verir. Bu örnekte, gelen kimlik bilgilerini başarılı bir şekilde doğrulandıktan sonra, SHA-1 parmak izine sahip bir sertifikanın bulunduğu şekilde 5 EC... 4264 ', ' admin' rolü; ' olarak verilecek Buna karşılık bir çağıran, ' 7c8f ' sertifikasıyla kimlik doğrulaması yapılıyor... 01B0 ', birincil olarak salt okuma işlemleriyle kısıtlanan ' Kullanıcı ' rolüne sahip olacak. Parmak izi ' abcd... olan bir sertifika sunan bir gelen çağıranı. 1234 ' veya ' EF01... 5678 ', kümede bir eş düğüm olarak kabul edilecektir. Son olarak, kümenin yönetim uç noktasına bağlanan bir istemci, sunucu sertifikasının parmak izini ' EF01... olarak beklecektir. 5678 '. 

Daha önce belirtildiği gibi, Service Fabric, süre dolmayan sertifikaları kabul etmek için sağlama sağlar; Bu nedenle, sertifikaların süresi sınırlı ve parmak izi (belirli bir sertifika örneğine başvurur) tarafından bildirildiği zaman, bir sertifikanın süresinin dolmasına izin verilmesi, kümeye bağlanamaması ya da kümenin bir kısmını daraltmasına neden olur. Parmak izi sabitlenmiş bir sertifika döndürmeyi unutmak veya negtaştırmak çok kolaydır ve bu durum, bu durumdan kurtarma zor olur.

Bu uçta, küme sahibi, parmak izi tarafından belirtilen otomatik olarak imzalanan sertifikaların geçerli kabul edilmesi için aşağıdaki gibi bir durum olabilir:

```xml
  <Section Name="Security">
    <Parameter Name="AcceptExpiredPinnedClusterCertificate" Value="true" />
  </Section>
```
Bu davranış, CA tarafından verilen sertifikalara genişlemez; Bu durumda, geçersiz kılınabilecek ve güvenliği aşılmış bir zaman aşımına uğrayan bir sertifika, CA 'nın sertifika iptal listesinde yer almaz ve bu nedenle bir güvenlik riski sunmuyorsa ' geçerli ' hale gelebilir. Otomatik olarak imzalanan sertifikalarla, küme sahibi sertifikanın özel anahtarını koruma, CA tarafından verilen sertifikalarla ilgili olmayan tek taraf olarak kabul edilir. küme sahibi, sertifikalarının nasıl veya ne zaman tehlikede bildirildiği konusunda farkında olmayabilir.

#### <a name="common-name-based-certificate-validation-declarations"></a>Ortak ad tabanlı sertifika doğrulama bildirimleri
Ortak ad tabanlı bildirimler aşağıdaki biçimlerden birini alır:
- Konu ortak adı (yalnızca)
- veren sabitleme ile konu ortak adı

İlk olarak, her iki bildirim stilini de muaf tutmak için bir küme bildiriminden bir alıntı düşünmemize izin verin:
```xml
    <Section Name="Security/ServerX509Names">
      <Parameter Name="server.demo.system.servicefabric.azure-int" Value="" />
    </Section>
    <Section Name="Security/ClusterX509Names">
      <Parameter Name="cluster.demo.system.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
```
Bildirimler sırasıyla sunucu ve küme kimliklerine başvurur; CN tabanlı bildirimlerin küme bildiriminde kendi bölümlerine sahip olduğunu ve standart ' Güvenlik ' den ayrı olduğunu unutmayın. Her iki bildiriminde da, ' name ' sertifikanın ayırt edici konu ortak adını temsil eder ve ' Value ' alanı beklenen sertifikayı aşağıdaki gibi temsil eder:

- İlk durumda, bildiriminde, sunucu sertifikasının ayırt edici konusunun ortak ad öğesinin "Server. demo. System. servicefabric. Azure-int" dizesiyle eşleşmesi beklenildiği belirtilir. boş ' Value ' alanı, Sertifika zincirinin köküne sunucu sertifikasının doğrulandığı düğüm/makinede güvenildiğini belirtir; Windows 'da bu, sertifikanın ' güvenilen kök CA ' deposunda yüklü olan herhangi bir sertifikaya zincirleme alabileceği anlamına gelir;
- İkinci durumda, bildirim, sertifikanın ortak adı "Cluster. demo. System. servicefabric. Azure-int" dizesiyle eşleşiyorsa *ve* sertifikayı doğrudan verenin Parmak Izi ' değer ' alanındaki virgülle ayrılmış girdilerden biriyle eşleşiyorsa, bir sertifikanın sunucusunun kümede bir eş düğüm olarak kabul edildiğini belirtir. (Bu kural türü, ' veren sabitleme ile ortak ad ' olarak bilinir.)

Her iki durumda da, sertifikanın zinciri oluşturulur ve hata-boş olması beklenir; diğer bir deyişle, iptal hataları, kısmi zincir veya zaman geçersiz güven hataları önemli olarak değerlendirilir ve sertifika doğrulaması başarısız olur. Verenler sabitleyerek, ' güvenilmeyen kök ' durumunun önemli olmayan bir hata olarak ele sunulmasına neden olur. Görünümler buna rağmen bu, küme sahibinin yetkili/kabul edilen verenler kümesini kendi PKI 'lerine kısıtlamak için daha sıkı bir doğrulama biçimidir.

Sertifika zinciri oluşturulduktan sonra, uzak ad olarak bildirildiği konuyla standart bir TLS/SSL ilkesine göre onaylanır. bir sertifika, konu ortak adı veya konu diğer adlarının herhangi biri küme bildiriminden CN bildirimiyle eşleşiyorsa eşleşme olarak değerlendirilir. Bu durumda joker karakterler desteklenir ve eşleşen dize, büyük/küçük harfe duyarlıdır.

(Yukarıda açıklanan sıranın sertifika tarafından tanımlanan her anahtar kullanım türü için yürütülebileceğini açıklığa kavuşturmalıdır; sertifika istemci kimlik doğrulaması anahtar kullanımını belirtiyorsa, zincir ilk olarak bir istemci rolü için oluşturulur ve değerlendirilir. Başarı durumunda değerlendirme tamamlanır ve doğrulama başarılı olur. Sertifikada istemci kimlik doğrulaması kullanımı yoksa veya doğrulama başarısız olduysa, Service Fabric çalışma zamanı sunucu kimlik doğrulaması için zinciri derler ve değerlendirir.)

Örneği gerçekleştirmek için aşağıdaki alıntıda istemci sertifikalarının ortak ada göre bildirilmesi gösterilmektedir:
```xml
    <Section Name="Security/AdminClientX509Names">
      <Parameter Name="admin.demo.client.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
    <Section Name="Security/ClientX509Names">
      <Parameter Name="user.demo.client.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
```

Yukarıdaki bildirimler sırasıyla yönetici ve Kullanıcı kimliklerine karşılık gelir; Bu şekilde bildirildiği sertifikaların doğrulanması, önceki örneklerde küme ve sunucu sertifikalarında tam olarak açıklanmaktadır.

> [!NOTE]
> Ortak ad tabanlı bildirimler, döndürmeyi basitleştirecek ve genel olarak küme sertifikalarının yönetimine yöneliktir. Ancak, kümenin sürekli kullanılabilirliğini ve güvenliğini sağlamak için aşağıdaki önerilere bağlı kalmak önerilir:
  * Güvenilen köklere bağlı olarak veren sabitleme yapmayı tercih et
  * farklı PKI 'lerden verenler karıştırmaktan kaçının
  * Tüm beklenen verenler sertifika bildiriminde listelendiğinden emin olun; eşleşmeyen bir veren, başarısız bir doğrulamaya neden olur
  * PKI 'nın sertifika ilkesi uç noktalarının bulunabilir, kullanılabilir ve erişilebilir olduğundan emin olun; bu, AIA, CRL veya OCSP uç noktalarının yaprak sertifikada bildirildiği ve sertifika zinciri oluşturmanın tamamlanabilmesi için erişilebileceği anlamına gelir.

X. 509.440 sertifikalarıyla güvenliği sağlanmış bir kümede bağlantı isteği aldıktan sonra, Service Fabric çalışma zamanı, yukarıda açıklandığı şekilde, uzak tarafın kimlik bilgilerini doğrulamak için kümenin güvenlik ayarlarını kullanır; başarılı olursa, arayanın/uzak tarafın kimlik doğrulaması kabul edilir. Kimlik bilgisi birden çok doğrulama kuralıyla eşleşiyorsa, çalışma zamanı çağrıyı yapana eşleşen kuralların herhangi birinin en yüksek ayrıcalıklı rolünü verir. 

### <a name="presentation-rules"></a>Sunum kuralları
Önceki bölümde, kimlik doğrulamasının sertifikayla güvenli bir kümede nasıl çalıştığı açıklanmaktadır; Bu bölümde, Service Fabric çalışma zamanının, küme içi iletişim için kullandığı sertifikaları nasıl bulduğu ve yüklediği açıklanmaktadır; Bu "sunu" kurallarını çağırıyoruz.

Doğrulama kurallarında olduğu gibi, sunum kuralları da bir rol ve ilişkili kimlik bilgisi bildirimini parmak izi veya ortak ad ile ifade edilir. Doğrulama kurallarından farklı olarak, ortak ad tabanlı bildirimlerin veren sabitleme için bir sağlama yoktur; Bu, daha fazla esneklik ve daha iyi performans sağlar. Sunu kuralları, her farklı düğüm türü için küme bildiriminin ' NodeType ' bölümünde belirtilir; ayarlar, her düğüm türünün tek bir bölümde tam yapılandırmasına sahip olmasını sağlamak için kümenin Güvenlik bölümlerinden ayrılır. Azure Service Fabric kümelerinde, düğüm türü sertifika bildirimleri varsayılan olarak kümenin tanımının güvenlik bölümünde bulunan ayarlarına karşılık gelir.

#### <a name="thumbprint-based-certificate-presentation-declarations"></a>Parmak izi tabanlı sertifika sunumu bildirimleri
Daha önce açıklandığı gibi, Service Fabric çalışma zamanı, rolü içindeki diğer düğümlerin eşi olarak ve küme yönetim işlemleri için sunucu olarak ayırt eder. Prensibi, bu ayarlar ayrı olarak yapılandırılabilir, ancak uygulamada hizalama eğilimindedir. Bu makalenin geri kalanında, ayarların basitlik için eşleştiğini varsayacağız.

Bir küme bildiriminden aşağıdaki alıntıyı göz önünde bulunduralım:
```xml
  <NodeTypes>
    <NodeType Name="nt1vm">
      <Certificates>
        <ClusterCertificate X509FindType="FindByThumbprint" X509FindValue="cc71...1984" X509FindValueSecondary="49e2...19d6" X509StoreName="my" Name="ClusterCertificate" />
        <ServerCertificate X509FindValue="cc71...1984" Name="ServerCertificate" />
        <ClientCertificate X509FindValue="cc71...1984" Name="ClientCertificate" />
      </Certificates>
    </NodeType>
  </NodeTypes>
```
' ClusterCertificate ' öğesi, isteğe bağlı parametreler (' X509FindValueSecondary ') veya uygun varsayılanlara sahip olanlar (' X509StoreName ') dahil olmak üzere tam şemayı gösterir; diğer bildirimlerde kısaltılmış bir form gösterilmektedir. Yukarıdaki küme sertifikası bildirimi, ' nt1vm ' türündeki düğümlerin güvenlik ayarlarının ' cc71 ' sertifikasıyla başlatıldığını belirtir. birincil olarak 1984 ' ve ' 49e2.. İkinci olarak "19d6" sertifikası; Her iki sertifikanın de, LocalMachine\'My sertifika deposunda bulunması beklenir (veya Linux eşdeğer yolu, *var/lib/sfcert*).

#### <a name="common-name-based-certificate-presentation-declarations"></a>Ortak ad tabanlı sertifika sunumu bildirimleri
Düğüm türü sertifikaları, aşağıda belirtildiği gibi, konu ortak adına göre de bildirilebilecek:

```xml
  <NodeTypes>
    <NodeType Name="nt1vm">
      <Certificates>
        <ClusterCertificate X509FindType="FindBySubjectName" X509FindValue="demo.cluster.azuredocpr.system.servicefabric.azure-int" Name="ClusterCertificate" />
      </Certificates>
    </NodeType>
  </NodeTypes>
```

Her iki tür bildirim için, Service Fabric bir düğüm başlangıçta yapılandırmayı okur, belirtilen sertifikaları bulup yükler ve NotAfter özniteliklerini azalan düzende sıralar; süre biten sertifikalar yok sayılır ve listenin ilk öğesi bu düğüm tarafından denenen tüm Service Fabric bağlantıları için istemci kimlik bilgileri olarak seçilir. (Aslında Service Fabric, en fazla süresi dolan sertifikayı tercih eder.)

Ortak ad tabanlı sunum bildirimleri için, özne ortak adı bildirimin X509FindValue (veya X509FindValueSecondary) alanına eşitse, büyük/küçük harfe duyarlı, tam bir dize karşılaştırması olarak bir sertifikanın eşleşme olarak kabul edileceğini unutmayın. Bu, joker karakterle eşleştirmeyi destekleyen ve büyük/küçük harfe duyarsız dize karşılaştırmalarının yanı sıra, doğrulama kurallarına karşılık gelir.  

### <a name="miscellaneous-certificate-configuration-settings"></a>Çeşitli sertifika yapılandırma ayarları
Daha önce, bir Service Fabric kümesinin güvenlik ayarlarının, kimlik doğrulama kodunun davranışını güvenle değiştirmeye izin vertiðinden bahsedilmekteydi. [Service Fabric küme ayarlarındaki](service-fabric-cluster-fabric-settings.md) makale, ayarların kapsamlı ve en güncel listesini temsil ettiğinde, sertifika tabanlı kimlik doğrulamasında tam kullanıma sunma işleminin tamamlanabilmesi için buradaki güvenlik ayarlarının kaç kez seçdiğiyle ilgili anlamı inceleyeceğiz. Her ayar için, amacı, varsayılan değeri/davranışı, kimlik doğrulamasını nasıl etkilediğini ve hangi değerlerin kabul edilebilir olduğunu açıklayacağız.

Belirtildiği gibi, sertifika doğrulama her zaman sertifikanın zincirini oluşturmayı ve değerlendirmeyi gerektirir. CA tarafından verilen sertifikalar için bu görünüşe göre basit işletim sistemi API çağrısı genellikle veren PKI 'nın çeşitli uç noktalarına giden çok sayıda giden çağrı ve yanıtları önbelleğe alma gibi işlemleri kapsar. Bir Service Fabric kümesinde sertifika doğrulama çağrılarının ön sürümü verildiğinde, PKI 'nın uç noktalarında herhangi bir sorun, kümenin veya sağ bir dökümdeki kullanılabilirliğinin düşmesine neden olabilir. Giden çağrılar gizlenemez (bunun hakkında daha fazla bilgi için bkz. SSS bölümünde aşağıda verilmiştir), başarısız olan CRL çağrılarının neden olduğu doğrulama hatalarını maskelemek için aşağıdaki ayarlar kullanılabilir.

  * CrlCheckingFlag-' Güvenlik ' bölümünün altında, dize UINT öğesine dönüştürüldü. Bu ayarın değeri, zincir oluşturma davranışını değiştirerek sertifika zinciri durum hatalarını maskelemek için Service Fabric tarafından kullanılır; ' dwFlags ' parametresi olarak Win32 CryptoAPI [Certgetcertificatezincirine](https://docs.microsoft.com/windows/win32/api/wincrypt/nf-wincrypt-certgetcertificatechain) çağrı ' ya geçirilir ve işlev tarafından kabul edilen herhangi bir geçerli bayrak birleşimine ayarlanabilir. 0 değeri, Service Fabric çalışma zamanını herhangi bir güven durumu hatasını yoksayacak şekilde zorlar. Bu, kullanımı önemli bir güvenlik pozlaması oluşturduğundan önerilmez. Varsayılan değer 0x40000000 (CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT) ' dir.

  Ne zaman kullanılır: Yerel test için, otomatik olarak imzalanan sertifikalar veya sertifikaları desteklemek için uygun bir ortak anahtar altyapısına sahip olmayan geliştirici sertifikaları. Ayrıca, PKI 'lar arasında geçiş sırasında, AIR-gapped ortamlarında azaltma olarak da kullanılabilir.

  Nasıl kullanılır: iptal denetimini yalnızca önbelleğe alınmış URL 'Lere erişmeye zorlayan bir örnek inceleyeceğiz. Varsayılıyor
  ```C++
  #define CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY         0x80000000
  ```
  ardından, küme bildirimindeki bildirim şu şekilde olur:
  ```xml
    <Section Name="Security">
      <Parameter Name="CrlCheckingFlag" Value="0x80000000" />
    </Section>
  ```

  * IgnoreCrlOfflineError-' Security ' bölümü altında, varsayılan değeri ' false ' olan Boolean. ' İptal çevrimdışı ' zinciri oluşturma hatası durumunu (veya sonraki bir zincir ilkesi doğrulama hatası durumunu) gizleme kısayolunu temsil eder.

  Ne zaman kullanılır: Yerel test veya uygun bir PKI tarafından desteklenmeyen geliştirici sertifikaları. AIR-gapped ortamlarında veya PKI 'nın erişilemeyen bilindiğinde risk azaltma olarak kullanın.

  Nasıl kullanılır:
  ```xml
    <Section Name="Security">
      <Parameter Name="IgnoreCrlOfflineError" Value="true" />
    </Section>
  ```

  Diğer önemli ayarları (' Güvenlik ' bölümünün altında):
  * AcceptExpiredPinnedClusterCertificate-parmak izi tabanlı sertifika doğrulamasına adanmış bölümde açıklanmıştır; süre sonu otomatik olarak imzalanan küme sertifikalarının kabul edilmesini sağlar. 
  * Sertifika NotAfter zaman damgasından önce dakika olarak ifade edilen ve sertifikanın süre sonu için risk altında kabul edildiği zaman aralığı. Service Fabric, küme sertifikalarını izler ve sistem durumu raporlarını kalan kullanılabilirliğine göre düzenli olarak yayar. ' Güvenlik ' aralığının içinde, bu durum raporları ' uyarı ' durumunda yükseltilir. Varsayılan değer 30 gündür.
  * Certificatehealthreportingınterval-küme sertifikalarının kalan zaman geçerliliği ile ilgili sistem durumu raporlarının sıklığını denetler. Raporlar, bu aralığa göre yalnızca bir kez yayınlanır. Değer, varsayılan olarak 8 saat olacak şekilde saniye cinsinden ifade edilir.
  * EnforcePrevalidationOnSecurityChanges-Boolean, güvenlik ayarlarının değişiklikleri algılandıktan sonra küme yükseltmesinin davranışını denetler. ' True ' olarak ayarlanırsa, küme yükseltmesi, herhangi bir sunu kuralından eşleşen sertifikalardan en az birinin karşılık gelen bir doğrulama kuralını geçirebilmesi için denenecek. Ön doğrulama, yeni ayarlar herhangi bir düğüme uygulanmadan önce yürütülür, ancak yalnızca yükseltmeyi başlatma sırasında Küme Yöneticisi hizmetinin birincil çoğaltmasını barındıran düğümde çalışır. Bu yazma itibariyle, ayarın varsayılan değeri ' false ' olur ve 7,1 ile başlayan bir çalışma zamanı sürümü olan yeni Azure Service Fabric kümeleri için ' true ' olarak ayarlanır.
 
### <a name="end-to-end-scenario-examples"></a>Uçtan uca senaryo (örnekler)
Sunum kurallarına, doğrulama kurallarına ve fazla çalışma bayraklarını inceledik, ancak bunların hepsi birlikte nasıl çalışır? Bu bölümde, güvenlik ayarlarının güvenli küme yükseltmeleri için nasıl yararlanılabilir kullanabileceğinizi gösteren iki uçtan uca örnek üzerinden çalışacağız. Bunun, Service Fabric ' de uygun sertifika yönetiminde kapsamlı bir ayırma olması amaçlanmadığını unutmayın. bu konu başlığı altında bir yardımcı makaleye bakın.

Sunum ve doğrulama kurallarının ayrımı, ayrılmasının ve sonuçlarının ne olacağı gibi belirgin bir soru (veya sorun) doğurur. Aslında bir düğümün kimlik doğrulama sertifikası seçiminin başka bir düğümün doğrulama kurallarına geçememesi olasıdır. Aslında bu tutarsızlık, kimlik doğrulaması ile ilgili olayların birincil nedeninden oluşur. Aynı zamanda, bu kuralların ayrılması, kümenin güvenlik ayarlarını değiştiren bir yükseltme sırasında makinenin çalışmaya devam etmesini sağlar. İlk adım olarak ilk doğrulama kurallarını genişleterek, tüm küme düğümlerinin, geçerli kimlik bilgilerini kullanmaya devam ederken yeni ayarları yakınlaşmasını istediğinizi göz önünde bulundurun. 

Service Fabric kümesinde, yükseltmenin (en fazla 5) ' yükseltme etki alanı ' veya UDs) ilerlediğini hatırlayın. Yalnızca geçerli UD içindeki düğümler, belirli bir zamanda yükseltilmekte/değiştirilmekte ve yükseltme yalnızca kümenin kullanılabilirliği izin veriyorsa bir sonraki UD 'ye devam edecektir. (Daha fazla bilgi için bkz. [Service Fabric küme yükseltmeleri](service-fabric-cluster-upgrade.md) ve diğer makalelere bakın.) Sertifika/güvenlik değişiklikleri özellikle riskli olduğundan, düğümleri kümeden ayırabilirler veya kümeyi çekirdek kaybı kenarında bırakabilirsiniz.

Bir düğümün güvenlik ayarlarını anlatmak için aşağıdaki gösterimi kullanacağız:

NK: {P:{TP = A}, V:{TP = A}}, burada:
  - ' NK ', yükseltme etki alanı *k* 'da bir düğümü temsil eder
  - ' P ', düğümün geçerli Sunu kurallarını temsil eder (yalnızca küme sertifikalarına başvurduğumuz varsayılarak); 
  - ' V ', düğümün geçerli doğrulama kurallarını temsil eder (yalnızca küme sertifikası)
  - ' TP = A ', ' A ' sertifika parmak izi olan bir parmak izi tabanlı bildirimi (TP) temsil eder
  - ' CN = B ', sertifikanın konu ortak adı olan ortak bir ad tabanlı bildirimi (CN) temsil eder 

#### <a name="rotating-a-cluster-certificate-declared-by-thumbprint"></a>Parmak iziyle belirtilen küme sertifikasını döndürme
Aşağıdaki sıra, bir 2 aşamalı yükseltmenin parmak izi tarafından belirtilen ikincil bir küme sertifikasını güvenli bir şekilde tanıtmak için nasıl kullanılabileceğini açıklar; İlk aşama doğrulama kurallarında yeni sertifika bildirimini tanıtır ve ikinci aşama bunu sunum kurallarında tanıtır:
  - İlk durum: N0 = {P:{TP = A}, V:{TP = A}},... NK = {P:{TP = A}, V:{TP = A}}-küme bekleyen, tüm düğümler ortak bir yapılandırmayı paylaşır
  - yükseltme etki alanını tamamladıktan sonra 0: N0 = {P:{TP = A}, V:{TP = A, TP = B}},... NK = {P:{TP = A}, V:{TP = A}}-UD0 içindeki düğümler A sertifikası sunar ve A veya B sertifikalarını kabul eder; diğer tüm düğümler var ve yalnızca sertifikayı kabul et
  - Son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{TP = A}, V:{TP = A, TP = B}},... NK = {P:{TP = A}, V:{TP = A, TP = B}}-tüm düğümler sertifika A, tüm düğümler A ya da B sertifikasını kabul eder
      
Bu noktada, küme eşitlenmiş olarak yeniden gelir ve yükseltme/değiştirme güvenlik ayarlarının ikinci aşaması şu şekilde olabilir:
  - yükseltme etki alanını tamamladıktan sonra 0: N0 = {P:{TP = A, TP = B}, V:{TP = A, TP = B}},... NK = {P:{TP = A}, V:{TP = A, TP = B}}-UD0 içindeki düğümler, kümedeki başka bir düğüm tarafından kabul edilen B 'yi sunmaya başlayacaktır.
  - Son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{TP = A, TP = B}, V:{TP = A, TP = B}},... NK = {P:{TP = A, TP = B}, V:{TP = A, TP = B}}-tüm düğümler sertifika B 'yi sunmaya geçti. sertifika A artık, sonraki bir yükseltme kümesiyle küme tanımından kullanımdan kaldırılabilir/kaldırılabilir.

#### <a name="converting-a-cluster-from-thumbprint--to-common-name-based-certificate-declarations"></a>Bir kümeyi parmak iziyle ortak ad tabanlı sertifika bildirimlerine dönüştürme
Benzer şekilde, sertifika bildiriminin türünü (parmak izinden ortak ada) değiştirmek, yukarıdaki şekilde aynı kalıbı izler. Doğrulama kurallarının, aynı küme tanımında hem parmak izi hem de ortak ad ile belirli bir rolün sertifikalarını bildirme izni verildiğini unutmayın. Buna karşın, sunum kuralları yalnızca bir bildirim biçimine izin veriyor. Arada, bir küme sertifikasını parmak izinden ortak ada dönüştürmeye yönelik güvenli yaklaşım, önce parmak izine göre amaçlanan hedef sertifikayı tanıtmak ve ardından bu bildirimi ortak bir ad tabanlı bir ad ile değiştirmek. Aşağıdaki örnekte, parmak izi ' A ' ve konu ortak adı ' B ' aynı sertifikaya başvuracağız. 

  - İlk durum: N0 = {P:{TP = A}, V:{TP = A}},... NK = {P:{TP = A}, V:{TP = A}}-küme Rest durumunda, tüm düğümler birincil sertifika parmak izi olacak şekilde ortak bir yapılandırma paylaşır
  - yükseltme etki alanını tamamladıktan sonra 0: N0 = {P:{TP = A}, V:{TP = A, CN = B}},... NK = {P:{TP = A}, V:{TP = A}}-UD0 içindeki düğümler A sertifikası sunar ve parmak izine ya da ortak ad B ile sertifika kabul eder; diğer tüm düğümler var ve yalnızca sertifikayı kabul et
  - Son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{TP = A}, V:{TP = A, CN = B}},... NK = {P:{TP = A}, V:{TP = A, CN = B}}-tüm düğümler sertifika A, tüm düğümler sertifika A (TP) ya da B (CN) kabul eder

Bu noktada, sonraki bir yükseltme ile Sunu kurallarını değiştirmeye devam edebiliriz:
  - yükseltme etki alanını tamamladıktan sonra 0: N0 = {P:{CN = B}, V:{TP = A, CN = B}},... NK = {P:{TP = A}, V:{TP = A, CN = B}}-UD0 içindeki düğümler CN tarafından bulunan B sertifikasını ve parmak izi A veya ortak adı B olan sertifikaları kabul eder; diğer tüm düğümler mevcut ve yalnızca sertifika parmak iziyle seçili olan sertifikayı kabul ediyor
  - Son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{CN = B}, V:{TP = A, CN = B}},... NK = {P:{CN = B}, V:{TP = A, CN = B}}-tüm düğümler, CN tarafından bulunan B sertifikası var, tüm düğümler sertifika A (TP) veya B (CN) kabul eder
    
Aşama 2 ' nin tamamlanması, kümenin ortak ad tabanlı sertifikalara dönüştürülmesini de işaretler; parmak izi tabanlı doğrulama bildirimleri, sonraki bir küme yükseltmesinde kaldırılabilir.

> [!NOTE]
> Azure Service Fabric kümelerinde, yukarıda sunulan iş akışları Service Fabric kaynak sağlayıcısı tarafından düzenlenir; küme sahibi, belirtilen kurallara (sunu veya doğrulama) göre kümeye sertifika sağlamaktan hala sorumludur ve birden çok adımda değişiklik gerçekleştirmeleri önerilir.

Ayrı bir makalede, sertifikaların Service Fabric bir kümeye yönetilmesi ve sağlanması konusu ele alınacaktır.

## <a name="troubleshooting-and-frequently-asked-questions"></a>Sorun giderme ve sık sorulan sorular
Service Fabric kümelerinde kimlik doğrulama ile ilgili sorunların hata ayıklaması kolay olmadığından, aşağıdaki ipuçları ve ipuçları yardımcı olabilir. Araştırmalardan başlamak için en kolay yol, kümenin düğümlerinde Service Fabric olay günlüklerini incelemektir, ancak aynı zamanda yalnızca belirtileri gösterme, ancak bunların komşu bir sürümüne bağlanamama düğümlerdir. Windows 'da, anlam olayları genellikle sırasıyla ' uygulamalar ve hizmetler Logs\Microsoft-ServiceFabric\Admin ' veya ' Operational ' kanalları altında günlüğe kaydedilir. Bazen [CAPI2 günlüğünü etkinleştirmek](https://docs.microsoft.com/archive/blogs/benjaminperkins/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues), sertifika doğrulama, CRL/CTL alma vb. ile ilgili daha fazla ayrıntı yakalamak için yararlı olabilir. (yeniden oluşturma işlemini tamamladıktan sonra bunu devre dışı bırakmayı unutmayın, oldukça ayrıntılı olabilir.)

Kimlik doğrulama sorunları yaşayan bir kümede kendilerine bildirimde bulunan tipik belirtiler şunlardır: 
  - düğümler aşağı/geçiş döngüsüne 
  - bağlantı denemeleri reddedilir
  - bağlantı denemeleri zaman aşımına uğruyor

Belirtilerin her biri farklı sorunlardan kaynaklanmış olabilir ve aynı temel neden farklı bildirim gösterebilir; Bu nedenle, bunları düzeltmeye yönelik önerilerle tipik sorunların küçük bir örneğini listeliyoruz. 

* Düğümler ileti alışverişi yapabilir ancak bağlanamaz. Bağlantı girişimlerinin sonlandırılmasının olası nedeni, ' sertifika eşleşmedi ' hatasıdır-Service Fabric Service Fabric bir bağlantı içindeki taraflardan biri, alıcının doğrulama kurallarına neden olan bir sertifika sunuyor. Şu hatalardan biriyle birlikte olabilir: 
  ```C++
  0x80071c44    -2147017660 FABRIC_E_SERVER_AUTHENTICATION_FAILED
  ```
  Daha fazla tanılama/araştırma yapmak için: bağlantıyı denemekte olan düğümlerin her birinde sunulan sertifikayı saptayın; sertifikayı inceleyin ve doğrulama kurallarına benzetim yapın (parmak izi veya ortak ad eşitlik için denetim yapın, belirtilmişse veren parmak izlerini denetleyin).

  Yaygın olarak kullanılan diğer hata kodu şu şekilde olabilir:
  ```C++
  0x800b0109    -2146762487 CERT_E_UNTRUSTEDROOT
  ```
  Bu durumda, sertifika ortak ad tarafından bildirildiği gibi, aşağıdakilerden biri de geçerlidir:
    - verenler sabitlenemez ve kök sertifika güvenilir değil veya
    - verenler sabitlenir ancak bildirim, bu sertifikanın doğrudan vereni parmak izini içermez

* Düğüm çalışır, ancak diğer düğümlere bağlanamaz; diğer düğümler, hatalı düğümden gelen trafiği almaz. Bu durumda, sertifika yüklemesi yerel düğümde başarısız olabilir. Aşağıdaki hataları arayın:
  - sertifika bulunamadı-sunum kurallarında belirtilen sertifikaların Localmachine\(veya belirtilen) sertifika deposunun içeriğiyle çözümlenebildiğinden emin olun. 
    Hatanın olası nedenleri şunlar olabilir: 
      - parmak izi bildiriminde geçersiz karakterler
      - sertifika yüklü değil
      - Sertifikanın geçerliliği zaman aşımına uğradı
      - ortak ad bildirimi ' CN = ' önekini içerir
      - bildirim bir joker karakter belirtiyor ve sertifika deposunda tam eşleşme yok (bildirim: CN = *. etkialanım. com, gerçek sertifika: CN = Server. etkialanım. com)

  - Bilinmeyen kimlik bilgileri-sertifikaya karşılık gelen eksik özel anahtarı (genellikle hata kodu ile birlikte) gösterir: 
    ```C++ 
    0x8009030d  -2146893043 SEC_E_UNKNOWN_CREDENTIALS
    0x8009030e  -2146893042 SEC_E_NO_CREDENTIALS
    ```
    Sorunu gidermek için, özel anahtarın varlığını denetleyin; SFAdmins 'in özel anahtara ' okuma | yürütme ' erişimi verildiğini doğrulayın.

  - Hatalı sağlayıcı türü-yeni bir şifre oluşturma (CNG) sertifikasını ("Microsoft yazılım anahtarı depolama sağlayıcısı") belirtir; Şu anda Service Fabric yalnızca CAPI1 sertifikalarını destekler. Genellikle hata kodu ile birlikte:
    ```C++
    0x80090014  -2146893804 NTE_BAD_PROV_TYPE
    ```
    CAPI1 (örneğin, "Microsoft Gelişmiş RSA ve AES şifreleme sağlayıcısı") sağlayıcısını kullanarak küme sertifikasını yeniden oluşturun. Şifreleme sağlayıcıları hakkında daha fazla bilgi için bkz. [şifreleme sağlayıcılarını anlama](https://docs.microsoft.com/windows/win32/seccertenroll/understanding-cryptographic-providers)

