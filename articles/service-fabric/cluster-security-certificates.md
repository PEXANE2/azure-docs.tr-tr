---
title: Bir Hizmet Kumaş Kümesinde X.509 Sertifika Tabanlı Kimlik Doğrulama
description: Service Fabric kümelerinde sertifika tabanlı kimlik doğrulama ve sertifikayla ilgili sorunları nasıl algılayabilen, azaltan ve düzelteceğimiz hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/16/2020
ms.custom: sfrev
ms.openlocfilehash: 699015e322c599dea996b3a8b9dbc0a4589440ab
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429674"
---
# <a name="x509-certificate-based-authentication-in-service-fabric-clusters"></a>Service Fabric kümelerinde X.509 Sertifika tabanlı kimlik doğrulama

Bu makale, Service [Fabric küme güvenliğine](service-fabric-cluster-security.md)girişi tamamlar ve Service Fabric kümelerinde sertifika tabanlı kimlik doğrulamasının ayrıntılarına gider. Okuyucunun temel güvenlik kavramlarına ve ayrıca Service Fabric'in bir kümenin güvenliğini kontrol etmek için ortaya çıkardığı denetimlere aşina olduğunu varsayıyoruz.  

Bu başlık altında ele alınan konular:

* Sertifika tabanlı kimlik doğrulama temelleri
* Kimlikler ve ilgili rolleri
* Sertifika yapılandırma kuralları
* Sorun Giderme ve Sık Sorulan Sorular

## <a name="certificate-based-authentication-basics"></a>Sertifika tabanlı kimlik doğrulama temelleri
Kısa bir tazeleyici olarak, güvenlik, bir sertifika asimetrik şifreleme anahtarları bir çift sahip bir varlık (konu) ile ilgili bilgileri bağlamak için bir araçtır ve böylece ortak anahtar şifreleme çekirdek bir yapı oluşturmaktadır. Sertifika tarafından temsil edilen anahtarlar verileri korumak veya anahtar sahiplerinin kimliğini kanıtlamak için kullanılabilir; Ortak Anahtar Altyapısı (PKI) sistemiyle birlikte kullanıldığında, sertifika, bir internet etki alanının mülkiyeti veya sertifikayı veren kişi tarafından kendisine verilen belirli ayrıcalıklar (Sertifika Yetkilisi veya CA olarak bilinir) gibi öznesinin ek özelliklerini temsil edebilir. Sertifikaların yaygın bir uygulaması, bir bilgisayar ağı üzerinden güvenli iletişim için izin, Taşıma Katmanı Güvenlik (TLS) şifreleme protokolü destekleyen. Özellikle, istemci ve sunucu, iletişimlerinin gizliliğini ve bütünlüğünü sağlamak ve aynı zamanda karşılıklı kimlik doğrulaması yapmak için sertifikaları kullanır.

Service Fabric'te, bir kümenin (Federasyon) temel katmanı, güvenilir ve güvenli bir katılımcı düğüm ağına ulaşmak için TLS (diğer protokoller arasında) üzerine kuruludur. Service Fabric Client API'leri aracılığıyla kümeye bağlantılar, trafiği korumak ve aynı zamanda tarafların kimliklerini oluşturmak için TLS'yi de kullanır. Özellikle, Hizmet Kumaşı'nda kimlik doğrulaması için kullanıldığında, aşağıdaki iddiaları kanıtlamak için bir sertifika kullanılabilir: a) sertifika kimlik bilgisi sunucusu sertifikanın özel anahtarına sahip dir b) sertifikanın SHA-1 karma ('parmak izi') küme tanımında yer alan bir bildirgeyle eşleşir veya c) sertifikanın seçkin ÖzOrtak Adı küme tanımında yer alan bir bildirgeyle eşleşir ve sertifikayı veren kuruluş tanınır veya güvenilirdir.

Yukarıdaki listede, 'b' halk dilinde 'parmak izi sabitleme' olarak bilinir; Bu durumda, bildirim belirli bir sertifikayı ifade eder ve kimlik doğrulama şemasının gücü, diğer tüm açılardan geçerli, iyi biçimlendirilmiş bir nesne olmakla birlikte, başka bir belgeyle aynı karma değeri üreten bir sertifikanın oluşturulabilmesinin hesaplama açısından mümkün olmadığı varsayımına bağlıdır. 'c' öğesi, şemanın gücünün sertifikanın konusu ve veren makamın birleşimine dayandığı alternatif bir sertifika beyan biçimini temsil eder. Bu durumda, bildirim bir sertifika sınıfına başvurur - aynı özelliklere sahip iki sertifika tam eşdeğer olarak kabul edilir. 

Aşağıdaki bölümler, Hizmet Dokusu runtime'ın küme güvenliğini sağlamak için sertifikaları nasıl kullandığını ve doğruladığını derinlemesine açıklar.

## <a name="identities-and-their-respective-roles"></a>Kimlikler ve ilgili rolleri
Kimlik doğrulama veya iletişim kanallarının güvenliğinin sağlanması nın ayrıntılarına dalmadan önce, katılımcı aktörleri ve bir kümede oynadıkları ilgili rolleri listelemek önemlidir:
- 'sistem' olarak adlandırılan Hizmet Dokusu çalışma zamanı: kümeyi temsil eden soyutlamaları ve işlevselliği sağlayan hizmet kümesi. Sistem örnekleri arasındaki küme içi iletişime atıfta bulunduğımızda, 'küme kimliği' terimini kullanacağız; kümeye kümenin dışından gelen trafiğin alıcısı/hedefi olarak atıfta bulunulduğunda , 'sunucu kimliği' terimini kullanırız.
- 'uygulamalar' olarak adlandırılan barındırılan uygulamalar: kümede düzenlenen ve yürütülen kümesahibi tarafından sağlanan kod
- istemciler: küme yapılandırmasına göre kümeye bağlanmasına ve işlevselliği yürütmesine izin verilen varlıklar. İki ayrıcalık düzeyi arasında ayrım yapıyoruz : sırasıyla 'kullanıcı' ve 'yönetici'. Bir 'kullanıcı' istemcisi öncelikle salt okunur işlemleriyle (ancak salt okunur işlevsellikle değil) kısıtlanırken, 'yönetici' istemcikümenin işlevselliğine sınırsız erişimi vardır. (Daha fazla ayrıntı [için, Hizmet Kumaşı kümesindeki Güvenlik rollerine](service-fabric-cluster-security-roles.md)bakın.)
- (Yalnızca Azure'da) Yalnızca 'hizmet' olarak adlandırılan Service Fabric kümelerinin işletimi ve yönetimi için denetimleri düzenleyen ve ortaya çıkaran Service Fabric hizmetleri. Ortama bağlı olarak, 'hizmet' Azure Hizmet Kumaşı Kaynak Sağlayıcısı'na veya Service Fabric ekibi tarafından sahip olunan ve işletilen diğer Kaynak Sağlayıcılarına başvurabilir.

Güvenli bir kümede, bu rollerin her biri kendi farklı kimlikleriile yapılandırılabilir, önceden tanımlanmış bir rol adı ve buna karşılık gelen kimlik bilgisi eşleşmesi olarak beyan edilebilir. Service Fabric, kimlik bilgilerini sertifika veya etki alanı tabanlı hizmet sorumlusu olarak bildirmeyi destekler. (Windows/Kerberos tabanlı kimlikler de desteklenir, ancak bu makalenin kapsamı dışındadır; [Hizmet Dokusu kümelerinde Windows tabanlı güvenliğe](service-fabric-windows-cluster-windows-security.md)bakın.) Azure kümelerinde istemci rolleri Azure [Etkin Dizin tabanlı kimlikler](service-fabric-cluster-creation-setup-aad.md)olarak da beyan edilebilir.

Yukarıda da belirtildiği gibi, Service Fabric çalışma süresi bir kümedeki iki ayrıcalık düzeyi tanımlar: 'yönetici' ve 'kullanıcı'. Bir yönetici istemci ve bir 'sistem' bileşeni hem 'yönetici' ayrıcalıkları ile çalışır ve bu nedenle birbirinden ayırt edilemez. Kümeye giriş/to'da bir bağlantı kurduktan sonra, kimlik doğrulaması yapılan bir arayan, sonraki yetkilendirme için temel olarak iki rolden biri olan Service Fabric çalışma zamanı tarafından verilir. Kimlik doğrulamayı aşağıdaki bölümlerde derinlemesine inceleyeceğiz.

## <a name="certificate-configuration-rules"></a>Sertifika yapılandırma kuralları
### <a name="validation-rules"></a>Doğrulama kuralları
Service Fabric kümesinin güvenlik ayarları prensipte aşağıdaki yönleri açıklar:
- kimlik doğrulama türü; bu kümenin yaratılış zamanı, değişmez bir özelliğidir. Bu tür ayarlara örnek olarak 'ClusterCredentialType', 'ServerCredentialType' ve izin verilen değerler 'yok', 'x509' veya 'windows' verilebilir. Bu makalede x509 türü kimlik doğrulama üzerinde duruluyor.
- (kimlik doğrulama) doğrulama kuralları; bu ayarlar küme sahibi tarafından ayarlanır ve belirli bir rol için hangi kimlik bilgilerinin kabul edileceğini açıklar. Örnekler aşağıda derinlemesine incelenecektir.
- kimlik doğrulamanın sonucunu değiştirmek veya değiştirmek için kullanılan ayarlar; buradaki örnekler arasında sertifika iptal listelerinin uygulanmasını kısıtlayan bayraklar (de-)

> [!NOTE]
> Aşağıda verilen küme yapılandırma örnekleri, bu makalede açıklanan Hizmet Kumaşı işlevini doğrudan destekleyen en çok sindirilen biçim olarak XML biçimindeki küme manifestosundan alıntılardır. Aynı ayarlar, bağımsız json küme bildirimi veya Azure Kaynak Mangement şablonu olsun, bir küme tanımının JSON gösterimlerinde doğrudan ifade edilebilir.

Sertifika doğrulama kuralı aşağıdaki öğeleri içerir:
- ilgili rol: istemci, yönetici istemci (ayrıcalıklı rol)
- rol için kabul edilen kimlik bilgisi, ya parmak izi veya konu ortak adı ile ilan

#### <a name="thumbprint-based-certificate-validation-declarations"></a>Parmak izi tabanlı sertifika doğrulama bildirimleri
Parmak izi tabanlı doğrulama kuralları söz konusu olduğunda, kümeye bağlantı isteyen bir arayan tarafından sunulan kimlik bilgileri aşağıdaki gibi doğrulanır:
  - kimlik bilgisi geçerli, iyi biçimlendirilmiş bir sertifikadır: zinciri oluşturulabilir, imzalar eşleşir
  - sertifika geçerli (NotBefore <= şimdi < NotAfter)
  - sertifikanın SHA-1 karma tüm beyaz alanlar hariç bir büyük/küçük harf duyarsız dize karşılaştırması olarak, bildirimeşleşir

Zincir oluşturma veya doğrulama sırasında karşılaşılan güven hataları, süresi dolmuş sertifikalar dışında parmak izi tabanlı bildirimler için bastırılır - ancak bu durum için de hükümler vardır. Özellikle, ilgili hatalar: iptal durumu bilinmiyor veya çevrimdışı olmak, güvenilmeyen kök, geçersiz anahtar kullanımı, kısmi zincir önemli olmayan hatalar olarak kabul edilir; öncül, bu durumda, sertifika sadece bir anahtar çifti için bir zarf olmasıdır - güvenlik küme sahibi özel anahtarı korumak için önlemler yerlerde belirlemiştir aslında yatıyor.

Bir küme bildiriminden aşağıdaki alıntı, bu tür bir parmak izi tabanlı doğrulama kuralı kümesine örnek teşkil eder:

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

Yukarıdaki girişlerin her biri daha önce açıklandığı gibi belirli bir kimliğe başvurur; her giriş, virgülle ayrılmış dizeleri listesi olarak birden çok değer belirtmeye de olanak tanır. Bu örnekte, gelen kimlik bilgilerini başarıyla doğrulayan, SHA-1 parmak izi 'd5ec ile bir sertifika sunucusu ... 4264' 'yönetici' rolü verilecektir; tersine, bir arayan sertifika '7c8f ile kimlik doğrulaması ... 01b0' öncelikle salt okunur işlemlerle sınırlı bir 'kullanıcı' rolü verilecektir. Parmak izi ya 'abcd olan bir sertifika sunan gelen bir arayan... 1234' veya 'ef01... 5678' kümede eş düğümü olarak kabul edilecektir. Son olarak, kümenin yönetim bitiş noktasına bağlanan bir istemci sunucu sertifikasının parmak izinin 'ef01... olmasını bekler. 5678'. 

Daha önce de belirtildiği gibi, Service Fabric süresi dolmuş sertifikaları kabul etmek için hükümler yapar; bunun nedeni, sertifikaların sınırlı bir kullanım ömrüne sahip olması ve parmak izi tarafından (belirli bir sertifika örneğine atıfta bulunulan) bildirildiğinde, bir sertifikanın süresinin dolmasına izin vermek kümeye bağlanmama veya kümenin salt çökmesine neden olacaktır. Bir parmak izi sabitlenmiş sertifikayı unutmak veya ihmal etmek çok kolaydır ve ne yazık ki böyle bir durumdan kurtulması zordur.

Bu amaçla, küme sahibi, parmak izi tarafından bildirilen kendi imzalı sertifikaların geçerli kabul edileceğini açıkça belirtebilir:

```xml
  <Section Name="Security">
    <Parameter Name="AcceptExpiredPinnedClusterCertificate" Value="true" />
  </Section>
```
Bu davranış, CA tarafından verilen sertifikalara kadar uzanmaz; bu durumda, iptal edilmiş, gizliliği ihlal edilen süresi dolmuş bir sertifika, CA'nın sertifika iptal listesinde artık bir rakam vermez 'geçerli' olabilir ve böylece bir güvenlik riski sunabilir. Kendi imzalı sertifikaları ile küme sahibi, CA tarafından verilen sertifikalarda geçerli olmayan sertifikanın özel anahtarını korumaktan sorumlu tek taraf olarak kabul edilir - küme sahibi sertifikalarının nasıl ve ne zaman tehlikeye girdiği konusunda farkında olmayabilir.

#### <a name="common-name-based-certificate-validation-declarations"></a>Ortak ad tabanlı sertifika doğrulama bildirimleri
Ortak ad tabanlı bildirimler aşağıdaki formlardan birini alır:
- konu ortak adı (yalnızca)
- ihraççı sabitleme ile konu ortak adı

Önce bir küme manifestosundan her iki bildirim stilini örnekleyen bir alıntıyı ele alalım:
```xml
    <Section Name="Security/ServerX509Names">
      <Parameter Name="server.demo.system.servicefabric.azure-int" Value="" />
    </Section>
    <Section Name="Security/ClusterX509Names">
      <Parameter Name="cluster.demo.system.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
```
Bildirimler sırasıyla sunucu ve küme kimliklerine başvurur; CN tabanlı bildirimlerin küme manifestosunda standart 'Güvenlik'ten ayrı kendi bölümleri olduğunu unutmayın. Her iki bildirimde de 'Ad' sertifikanın ayırt edilen özne ortak adını, 'Değer' alanı ise beklenen vereni aşağıdaki gibi temsil eder:

- ilk durumda, bildirimde sunucu sertifikasının ayırt edilen öznesinin ortak ad öğesinin "server.demo.system.servicefabric.azure-int" dizesine uymasının beklendiği belirtilmektedir; boş 'Değer' alanı, sertifika zincirinin kökünün sunucu sertifikasının doğrulandığı düğüm/makinede güvenildiği beklentisini gösterir; Windows'da bu, sertifikanın 'Güvenilen Kök CA' deposunda yüklü olan sertifikalardan herhangi birini zincirleebileceği anlamına gelir;
- ikinci durumda, bildirgede, sertifikanın ortak adı "cluster.demo.system.servicefabric.azure-int" dizesinde eşleşirse, sertifikanın sunucusu kümede eş düğümü olarak kabul edilir *ve* sertifikayı doğrudan verenin parmak izi 'Değer' alanında virgülle ayrılmış girişlerden biriyle eşleşir. (Bu kural türü halk dilinde 'veren sabitleme ile ortak ad' olarak bilinir.)

Her iki durumda da, sertifika zinciri oluşturulur ve hatasız olması beklenir; diğer bir deyişle, iptal hataları, kısmi zincir veya zaman geçersiz güven hataları önemli kabul edilir ve sertifika doğrulama başarısız olur. İhraççıların sabitlemi, 'güvenilmeyen kök' durumunun ölümcül olmayan bir hata olarak dikkate alınmasıyla sonuçlanır; görünüşe rağmen, küme sahibinin yetkili/kabul edilen ihraççıkümesini kendi PKI'larıyla sınırlandırmasına olanak sağladığından, bu daha katı bir doğrulama biçimidir.

Sertifika zinciri kurulduktan sonra, uzak ad olarak bildirilen konu ile standart bir TLS/SSL ilkesine göre doğrulanır; öznesi ortak adı veya özne alternatif adlarından herhangi biri küme bildiriminden CN bildirimiyle eşleşirse, sertifika eşleştirilir. Joker karakterler bu durumda desteklenir ve dize eşleştirme büyük/küçük harf duyarsızdır.

(Yukarıda açıklanan sıranın sertifika tarafından bildirilen her anahtar kullanımı türü için yürütülebileceğini açıklığa kavuşturmalıyız; sertifika istemci kimlik doğrulama anahtarı kullanımını belirtirse, zincir önce istemci rolü için oluşturulur ve değerlendirilir. Başarı durumunda değerlendirme tamamlanır ve doğrulama başarılı olur. Sertifikaistemci kimlik doğrulaması kullanımı yoksa veya doğrulama başarısız olduysa, Hizmet Kumaşı çalışma zamanı sunucu kimlik doğrulaması için zinciri oluşturur ve değerlendirir.)

Örneği tamamlamak için, aşağıdaki alıntı istemci sertifikalarını ortak ada göre beyan etmeyi gösterir:
```xml
    <Section Name="Security/AdminClientX509Names">
      <Parameter Name="admin.demo.client.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
    <Section Name="Security/ClientX509Names">
      <Parameter Name="user.demo.client.servicefabric.azure-int" Value="1b45...844d,d7fe...26c8,3ac7...6960,96ea...fb5e" />
    </Section>
```

Yukarıdaki beyanlar sırasıyla yönetici ve kullanıcı kimliklerine karşılık gelir; bu şekilde beyan edilen sertifikaların doğrulanması, küme ve sunucu sertifikalarının önceki örneklerinde açıklandığı gibidir.

> [!NOTE]
> Ortak ad tabanlı bildirimler, döndürmeyi ve genel olarak küme sertifikalarının yönetimini basitleştirmek içindir. Ancak, kümenin sürekli kullanılabilirliğini ve güvenliğini sağlamak için aşağıdaki önerilere uyman önerilir:
  * güvenilir köklere güvenmek için ihraççı sabitlemeyi tercih etmeyi tercih
  * farklı PKI'lardan gelen ihraççıları karıştırmaktan kaçının
  * beklenen tüm ihraççıların sertifika beyannamesinde listelenmelerini sağlamak; bir uyumsuz veren veren başarısız bir doğrulama neden olur
  * PKI'nın sertifika ilkesi uç noktalarının bulunabilir, kullanılabilir ve erişilebilir olduğundan emin olun - bu, AIA, CRL veya OCSP uç noktalarının yaprak sertifikasında beyan edildiği ve sertifika zinciri oluşturmanın tamamlayabilmesi için erişilebilir olduğu anlamına gelir.

X.509 sertifikalarıyla güvenli bir kümede bağlantı isteği aldıktan sonra, service Fabric çalışma zamanı, yukarıda açıklandığı gibi uzak tarafın kimlik bilgilerini doğrulamak için kümenin güvenlik ayarlarını kullanır; başarılı olursa, arayan/uzak tarafın kimliği doğrulanmış olarak kabul edilir. Kimlik bilgisi birden çok doğrulama kuralıyla eşleşirse, çalışma zamanı arayana eşleşen kurallardan herhangi birinin en yüksek ayrıcalıklı rolünü verir. 

### <a name="presentation-rules"></a>Sunum kuralları
Önceki bölümde, kimlik doğrulamanın sertifika yla güvenli bir kümede nasıl çalıştığı açıklanmıştır; Bu bölümde, Hizmet Kumaşı çalışma zamanının küme içi iletişim için kullandığı sertifikaları nasıl keşfettiği ve yükleyeceği açıklanacaktır; biz bunlara "sunum" kuralları diyoruz.

Doğrulama kurallarında olduğu gibi, sunu kuralları bir rolü ve ortak adla ifade edilen ilişkili kimlik bilgisi bildirimini belirtir. Doğrulama kurallarının aksine, ortak ad tabanlı beyanlarda ihraççı sabitleme hükümleri yoktur; bu, daha fazla esnekliğin yanı sıra daha iyi performans sağlar. Sunu kuralları küme manifestosunun 'NodeType' bölümünde(ler) her ayrı düğüm türü için bildirilir; ayarlar, her düğüm türünün tek bir bölümde tam yapılandırmasına sahip olmasını sağlamak için kümenin Güvenlik bölümlerinden ayrılır. Azure Hizmet Dokusu kümelerinde, düğüm türü sertifika bildirimleri küme tanımının Güvenlik bölümündeki ilgili ayarları varsayılan olarak verir.

#### <a name="thumbprint-based-certificate-presentation-declarations"></a>Parmak izi tabanlı sertifika sunu bildirimleri
Daha önce açıklandığı gibi, Hizmet Dokusu çalışma süresi kümedeki diğer düğümlerin eş olarak rolü ve küme yönetimi işlemleri için sunucu olarak ayırt eder. Prensip olarak, bu ayarlar belirgin şekilde yapılandırılabilir, ancak uygulamada hizalama eğilimindedirler. Bu makalenin geri kalanı için, ayarların basitlik ile eşleşeceğini varsayacağız.

Bir küme manifestosundan aşağıdaki alıntıyı ele alalım:
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
'ClusterCertificate' öğesi, isteğe bağlı parametreler ('X509FindValueSecondary') veya uygun varsayılanlara ('X509StoreName') sahip olanlar da dahil olmak üzere tam şemayı gösterir; diğer bildirimler kısaltılmış bir form gösterir. Yukarıdaki küme sertifikası bildiriminde ,'nt1vm' türü düğümlerin güvenlik ayarlarının 'cc71... sertifikası ile başharflere geçirilir. 1984' birincil olarak ve '49e2... 19d6' ikincil sertifika olarak; her iki sertifikanın da LocalMachine\'My' sertifika deposunda (veya Linux eşdeğer yolu, *var/lib/sfcerts)* bulunması beklenmektedir.

#### <a name="common-name-based-certificate-presentation-declarations"></a>Ortak ad tabanlı sertifika sunu bildirimleri
Düğüm türü sertifikaları aşağıda örneklendiği gibi, özne ortak adı ile de beyan edilebilir:

```xml
  <NodeTypes>
    <NodeType Name="nt1vm">
      <Certificates>
        <ClusterCertificate X509FindType="FindBySubjectName" X509FindValue="demo.cluster.azuredocpr.system.servicefabric.azure-int" Name="ClusterCertificate" />
      </Certificates>
    </NodeType>
  </NodeTypes>
```

Her iki bildirim türü için de, Bir Hizmet Kumaş düğümü başlangıçta yapılandırmayı okuyacak, belirtilen sertifikaları bulur ve yükler ve notsonraki özniteliğinin azalan sırasına göre sıralar; süresi dolmuş sertifikalar yoksayılır ve listenin ilk öğesi bu düğüm tarafından denenen herhangi bir Hizmet Kumaşı bağlantısı için istemci kimlik bilgisi olarak seçilir. (Aslında, Service Fabric en uzak süresi dolan sertifikayı tercih eder.)

Ortak ad tabanlı sunu bildirimleri için, öznesi ortak adı, büyük/küçük harf duyarlı, tam dize karşılaştırması olarak bildirimin X509FindValue (veya X509FindValueSecondary) alanına eşitse, sertifikanın eşleme olarak kabul edilir. Bu, joker karakter eşleştirmesini destekleyen doğrulama kurallarının yanı sıra büyük/küçük harf duyarlı dize karşılaştırmalarıyla da zıttır.  

### <a name="miscellaneous-certificate-configuration-settings"></a>Çeşitli sertifika yapılandırma ayarları
Daha önce, Bir Hizmet Dokusu kümesinin güvenlik ayarlarının kimlik doğrulama kodunun davranışını kurnazca değiştirmeye de izin verdiği belirtilmişti. Service Fabric [küme ayarlarıyla](service-fabric-cluster-fabric-settings.md) ilgili makale kapsamlı ve en güncel ayarlar listesini temsil etse de, sertifika tabanlı kimlik doğrulamanın tam teşlünü tamamlamak için buradaki güvenlik ayarlarından birkaçının anlamını genişletiriz. Her ayar için, amacı, varsayılan değeri/davranışı, kimlik doğrulamasını nasıl etkilediğini ve hangi değerlerin kabul edilebilir olduğunu açıklarız.

Belirtildiği gibi, sertifika doğrulama her zaman sertifika zincirinin oluşturulması ve değerlendirilmesi anlamına gelir. CA tarafından verilen sertifikalar için, bu görünüşte basit OS API çağrısı genellikle izleyen PKI'nın çeşitli uç noktalarına, yanıtların önbelleğe alınmışlığına ve benzeri birkaç giden çağrı gerektirir. Hizmet Kumaşı kümesindeki sertifika doğrulama çağrılarının yaygınlığı göz önüne alındığında, PKI'nın uç noktalarındaki tüm sorunlar kümenin kullanılabilirliğinin azalmasına veya düpedüz arızalanmasına neden olabilir. Giden aramalar bastırılamaz (bu konuda daha fazla bilgi için SSS bölümünde aşağıya bakın), aşağıdaki ayarlar CRL çağrıları başarısız kaynaklanan doğrulama hataları maskelemek için kullanılabilir.

  * CrlCheckingFlag - 'Güvenlik' bölümü altında, dize UINT dönüştürülür. Bu ayarın değeri, zincir oluşturma nın davranışını değiştirerek sertifika zinciri durum hatalarını maskelemek için Service Fabric tarafından kullanılır; 'dwFlags' parametresi olarak Win32 CryptoAPI [CertGetCertificateChain](https://docs.microsoft.com/windows/win32/api/wincrypt/nf-wincrypt-certgetcertificatechain) çağrısına aktarılır ve işlev tarafından kabul edilen bayrakların geçerli bir kombinasyonuna ayarlanabilir. 0 değeri, Hizmet Kumaşı çalışma zamanını güven durumu hatalarını yoksaymaya zorlar - kullanımı önemli bir güvenlik pozlaması oluşturacağı için bu önerilmez. Varsayılan değer 0x40000000 (CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT).

  Ne zaman kullanılır: yerel sınama için, tam olarak oluşturulmayan/sertifikaları desteklemek için uygun bir ortak anahtar altyapısı olmayan kendi imzalı sertifikalar veya geliştirici sertifikaları ile. PKI'lar arasındaki geçiş sırasında hava boşlukları olan ortamlarda da azaltma olarak kullanabilir.

  Nasıl kullanılır: Yalnızca önbelleğe alınmış URL'lere erişmek için iptal denetimine zorlayan bir örnek alacağız. Varsay -arak:
  ```C++
  #define CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY         0x80000000
  ```
  sonra küme manifestosundaki bildirim olur:
  ```xml
    <Section Name="Security">
      <Parameter Name="CrlCheckingFlag" Value="0x80000000" />
    </Section>
  ```

  * IgnoreCrlOfflineError - 'Güvenlik' bölümü altında, 'false' varsayılan değeri ile boolean. Bir 'çevrimdışı iptal' zincir oluşturma hata durumunu (veya sonraki zincir ilkesi doğrulama hata durumunu) bastırmak için bir kısayolu temsil eder.

  Ne zaman kullanılacağı: yerel sınama veya geliştirici sertifikaları uygun bir PKI tarafından desteklenen değil. Hava boşlukları olan ortamlarda veya PKI'ya erişilemediğinde azaltma olarak kullanın.

  Nasıl kullanılır:
  ```xml
    <Section Name="Security">
      <Parameter Name="IgnoreCrlOfflineError" Value="true" />
    </Section>
  ```

  Diğer önemli ayarlar (tümü 'Güvenlik' bölümü altında):
  * AcceptExpiredPinnedClusterCertificate - parmak izi tabanlı sertifika doğrulamasına adanmış bölümde ele alınmıştır; süresi dolmuş kendi imzalı küme sertifikalarını kabul etmeye olanak tanır. 
  * SertifikaExpirySafetyMargin - aralığı, sertifikanın NotAfter zaman damgası önce dakika içinde ifade ve hangi sırasında sertifika sona erme için risk altında kabul edilir. Service Fabric küme sertifikasını(lar) izler ve kalan kullanılabilirliklerine ilişkin sağlık raporlarını periyodik olarak yayır. 'Güvenlik' aralığı içinde, bu sağlık raporları 'uyarı' durumuna yükseltilir. Varsayılan değer 30 gündür.
  * CertificateHealthReportingInterval - küme sertifikalarının kalan zaman geçerliliğiyle ilgili sağlık raporlarının sıklığını denetler. Raporlar bu aralık başına yalnızca bir kez yayılır. Değer, varsayılan 8 saat ile saniye cinsinden ifade edilir.
  * EnforcePrevalidationOnSecurityChanges - boolean, güvenlik ayarlarıdeğişiklikleri algılayarak küme yükseltme davranışını denetler. Küme yükseltmesi 'true' olarak ayarlanırsa, sunu kurallarından herhangi biriyle eşleşen sertifikalardan en az birinin karşılık gelen doğrulama kuralını geçebilmesini sağlamaya çalışır. Ön doğrulama, yeni ayarlar herhangi bir düğüme uygulanmadan önce yürütülür, ancak yükseltmeyi başlatma sırasında Cluster Manager hizmetinin birincil yinelemesini barındıran düğümde yalnızca çalışır. Bu yazı itibariyle, ayar 'false' varsayılanı vardır ve 7.1 ile başlayan bir çalışma zamanı sürümü olan yeni Azure Hizmet Kumaş kümeleri için 'true' olarak ayarlanır.
 
### <a name="end-to-end-scenario-examples"></a>Uçuca senaryo (örnekler)
Sunum kurallarına, doğrulama kurallarına ve ayar bayraklarına baktık, ama tüm bunlar birlikte nasıl işliyor? Bu bölümde, güvenlik ayarlarının güvenli küme yükseltmeleri için nasıl yararlanılabildiğini gösteren iki uçtan uca örnek üzerinde çalışacağız. Bunun Hizmet Kumaşı'nda uygun sertifika yönetimi üzerine kapsamlı bir tez olması amaçlanmamıştır, bu konuda bir yardımcı makale arayın.

Sunu ve doğrulama kurallarının ayrılması, bunların farklılaşıp ayrılaşmayacağı ve sonuçlarının ne olacağı konusunda bariz bir soru (veya endişe) doğurur. Gerçekten de, bir düğümün kimlik doğrulama sertifikası seçimi başka bir düğümün doğrulama kurallarını geçmez. Aslında, bu tutarsızlık kimlik doğrulama ile ilgili olayların birincil nedenidir. Aynı zamanda, bu kuralların ayrılması, kümenin güvenlik ayarlarını değiştiren bir yükseltme sırasında bir kümenin çalışmaya devam etmesine olanak tanır. İlk adım olarak doğrulama kurallarını artırarak, kümenin tüm düğümlerinin geçerli kimlik bilgilerini kullanmaya devam ederken yeni ayarlarda birleşeceğini düşünün. 

Service Fabric kümesinde bir yükseltmenin (5'e kadar) 'yükseltme etki alanları' veya ABD'ler aracılığıyla ilerlediğini hatırlayın. Yalnızca geçerli UD'deki düğümler belirli bir zamanda yükseltiliyor/değiştiriliyor ve yükseltme yalnızca kümenin kullanılabilirliği izin veriyorsa bir sonraki UD'ye devam edecek. (Daha fazla bilgi için [Hizmet Kumaş küme yükseltmeleri](service-fabric-cluster-upgrade.md) ve aynı konuda diğer makalelere bakın.) Kümedeki düğümleri yalıtabilecekleri veya kümeyi çoğunluk kaybının sınırında bırakabildiklerinden, sertifika/güvenlik değişiklikleri özellikle risklidir.

Düğümün güvenlik ayarlarını açıklamak için aşağıdaki gösterimi kullanacağız:

Nk: {P:{TP=A}, V:{TP=A}}, nerede:
  - 'Nk' yükseltme etki alanı *k* bir düğüm temsil eder
  - 'P' düğümün geçerli sunu kurallarını temsil eder (yalnızca küme sertifikalarına atıfta bulunduğumuz varsayılarak); 
  - 'V' düğümün geçerli doğrulama kurallarını temsil eder (yalnızca küme sertifikası)
  - 'TP=A' parmak izi tabanlı bir bildiriyi (TP) temsil eder ve 'A' sertifika parmak izi dir.
  - 'CN=B' ortak bir ad tabanlı bildiriyi (CN) temsil eder ve sertifikanın öznesi 'B' ortak adıdır 

#### <a name="rotating-a-cluster-certificate-declared-by-thumbprint"></a>Parmak izi tarafından bildirilen küme sertifikasını döndürme
Aşağıdaki sıra, 2 aşamalı bir yükseltmenin parmak izi tarafından bildirilen ikincil küme sertifikasını güvenli bir şekilde tanıtmak için nasıl kullanılabileceğini açıklar; birinci aşama doğrulama kurallarında yeni sertifika bildirimini, ikinci aşama ise sunu kurallarında tanıtır:
  - başlangıç durumu: N0 = {P:{TP=A}, V:{TP=A}} Nk = {P:{TP=A}, V:{TP=A}} - küme hazır, tüm düğümler ortak bir yapılandırmayı paylaşıyor
  - yükseltme etki alanı 0 tamamladıktan sonra: N0 = {P:{TP=A}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A}, V:{TP=A}} - UD0'daki düğümler A sertifikasını sunacak ve A veya B sertifikalarını kabul eder; diğer tüm düğümler mevcut ve sertifika kabul A sadece
  - son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{TP=A}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A}, V:{TP=A, TP=B}} - tüm düğümler A sertifikasını sunar, tüm düğümler A veya B sertifikasını kabul eder
      
Bu noktada küme yine dengededir ve yükseltme/değiştirme güvenlik ayarlarının ikinci aşaması başlayabilir:
  - yükseltme etki alanı 0 tamamladıktan sonra: N0 = {P:{TP=A, TP=B}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A}, V:{TP=A, TP=B}} - UD0'daki düğümler kümedeki herhangi bir düğüm tarafından kabul edilen B'yi sunmaya başlar.
  - son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{TP=A, TP=B}, V:{TP=A, TP=B}}, ... Nk = {P:{TP=A, TP=B}, V:{TP=A, TP=B}} - tüm düğümler B sertifikasını sunmaya geçti.

#### <a name="converting-a-cluster-from-thumbprint--to-common-name-based-certificate-declarations"></a>Kümeyi parmak izinden ortak ad tabanlı sertifika bildirimlerine dönüştürme
Benzer şekilde, sertifika bildiriminin türünü değiştirmek (küçük parmak izinden ortak ada) yukarıdaki yle aynı deseni izler. Doğrulama kurallarının, belirli bir rolün sertifikalarını aynı küme tanımında hem parmak izi hem de ortak adla bildirmeye izin verdiğini unutmayın. Buna karşılık, ancak, sunu kuralları bildirimyalnızca bir form sağlar. Bu arada, bir küme sertifikasını küçük parmak izinden ortak ada dönüştürmenin güvenli yaklaşımı, önce hedeflenen hedef sertifikayı önce parmak izi ile tanıtmak ve ardından bu bildirimi ortak bir ad tabanlı sertifikayla değiştirmektir. Aşağıdaki örnekte, 'A' ve özortak adı 'B' aynı sertifikaya atıfta bulunulan parmak izinin olduğunu varsayarız. 

  - başlangıç durumu: N0 = {P:{TP=A}, V:{TP=A}} Nk = {P:{TP=A}, V:{TP=A}} - küme hazır, tüm düğümler ortak bir yapılandırmayı paylaşır ve A birincil sertifika parmak izi dir.
  - yükseltme etki alanı 0 tamamladıktan sonra: N0 = {P:{TP=A}, V:{TP=A, CN=B}}, ... Nk = {P:{TP=A}, V:{TP=A}} - UD0'daki düğümler A sertifikasını sunacak ve a veya ortak ad B olan sertifikaları kabul eder; diğer tüm düğümler mevcut ve sertifika kabul A sadece
  - son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{TP=A}, V:{TP=A, CN=B}}, ... Nk = {P:{TP=A}, V:{TP=A, CN=B}} - tüm düğümler A sertifikasını sunar, tüm düğümler A (TP) veya B (CN) sertifikasını kabul eder.

Bu noktada, sonraki bir yükseltme ile sunu kurallarını değiştirmeye devam edebiliriz:
  - yükseltme etki alanı 0 tamamladıktan sonra: N0 = {P:{CN=B}, V:{TP=A, CN=B}}, ... Nk = {P:{TP=A}, V:{TP=A, CN=B}} - UD0'daki düğümler CN tarafından bulunan B sertifikasını sunacak ve a veya ortak ad B olan sertifikaları kabul eder; diğer tüm düğümler mevcut ve sertifika kabul A sadece, parmak izi tarafından seçilen
  - son yükseltme etki alanını tamamladıktan sonra: N0 = {P:{CN=B}, V:{TP=A, CN=B}}, ... Nk = {P:{CN=B}, V:{TP=A, CN=B}} - tüm düğümler CN tarafından bulunan B sertifikasını sunar, tüm düğümler A (TP) veya B (CN) sertifikasını kabul eder.
    
Aşama 2'nin tamamlanması, kümenin ortak ad tabanlı sertifikalara dönüştürülmesini de işaretler; parmak izi tabanlı doğrulama bildirimleri sonraki küme yükseltmesinde kaldırılabilir.

> [!NOTE]
> Azure Hizmet Kumaşı kümelerinde, yukarıda sunulan iş akışları Service Fabric Resource Provider tarafından düzenlenir; küme sahibi, sertifikaları belirtilen kurallara (sunu veya doğrulama) göre kümeye sağlamaktan hala sorumludur ve birden çok adımda değişiklik gerçekleştirmesi için teşvik edilir.

Ayrı bir makalede, sertifikaların bir Service Fabric kümesine yönetilmesi ve sağlanması konusunu ele alacağız.

## <a name="troubleshooting-and-frequently-asked-questions"></a>Sorun Giderme ve Sık Sorulan Sorular
Service Fabric kümelerinde kimlik doğrulamayla ilgili sorunları hata ayıklama kolay olmasa da, aşağıdaki ipuçlarının ve ipuçlarının yardımcı olabileceğinden umutluyuz. Araştırmalara başlamanın en kolay yolu, kümenin düğümlerindeki Servis Dokusu olay günlüklerini incelemektir - sadece belirtileri gösterenler değil, aynı zamanda yukarı olan ancak komşularından birine bağlanamayan düğümleri de incelemektir. Windows'da, önemli olaylar genellikle sırasıyla 'Uygulamalar ve Hizmetler Günlükleri\Microsoft-ServiceFabric\Admin' veya 'Operasyonel' kanalları altında günlüğe kaydedilir. Bazen [CAPI2 günlüğü etkinleştirmek](https://docs.microsoft.com/archive/blogs/benjaminperkins/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues)için yararlı olabilir , sertifika doğrulama ile ilgili daha fazla bilgi yakalamak için, CRL / CTL vb alma (repro tamamladıktan sonra devre dışı bırakmayı unutmayın, oldukça ayrıntılı olabilir.)

Kimlik doğrulama sorunları yaşayan bir kümede kendini gösteren tipik belirtiler şunlardır: 
  - düğümler aşağı/bisiklet 
  - bağlantı girişimleri reddedilir
  - bağlantı denemeleri zamanlamasını

Semptomların her biri farklı sorunlardan kaynaklanabilir ve aynı kök neden farklı belirtiler gösterebilir; bu nedenle, yalnızca tipik sorunların küçük bir örneğini, bunları giderme önerileriyle birlikte listeleyeceğiz. 

* Düğümler ileti alışverişinde bulunabilir, ancak bağlanamaz. Bağlantı denemelerinin sonlandırılmasının olası bir nedeni de 'eşleşmeyan sertifika' hatasıdır - Hizmet Kumaşı bağlantılarından biri, alıcının doğrulama kurallarını bozamayan bir sertifika sunmaktır. Aşağıdaki hatalardan biri eşlik edebilir: 
  ```C++
  0x80071c44    -2147017660 FABRIC_E_SERVER_AUTHENTICATION_FAILED
  ```
  Daha fazla tanıkoymak/araştırmak için: bağlantıyı deneyen düğümlerin her birinde, hangi sertifikanın sunulduğunu belirleyin; sertifikayı inceleyin ve doğrulama kurallarını taklit etmeye çalışın (parmak izi veya ortak ad eşitliği olup olmadığını kontrol edin, belirtilmişse verenin parmak izlerini kontrol edin).

  Eşlik eden başka bir yaygın hata kodu da şu olabilir:
  ```C++
  0x800b0109    -2146762487 CERT_E_UNTRUSTEDROOT
  ```
  Bu durumda, sertifika ortak adla bildirilir ve aşağıdakilerden biri geçerlidir:
    - verenler sabitlenmez ve kök sertifikasına güvenbelgesi yoktur veya
    - verenler sabitlenir, ancak beyanname, bu sertifikayı doğrudan verenin parmak izini içermez

* Bir düğüm yukarı, ancak diğer düğümlere bağlanamıyor; diğer düğümler başarısız düğümgelen trafik almaz. Bu durumda, sertifika yüklemesinin yerel düğümde başarısız olması mümkündür. Aşağıdaki hataları arayın:
  - sertifika bulunamadı - sunu kurallarında beyan edilen sertifikaların LocalMachine\My (veya belirtildiği gibi) sertifika deposunun içeriği yle çözülebileceğinden emin olun. 
    Olası başarısızlık nedenleri arasında şunlar sayılabilir: 
      - parmak izi bildiriminde geçersiz karakterler
      - sertifika yüklü değil
      - sertifikanın süresi doldu
      - ortak ad bildirimi 'CN=' önekini içerir
      - bildirgede bir joker karakter belirtilir ve sertifika deposunda tam eşleşme yoktur (bildirim: CN=*.mydomain.com, gerçek sertifika: CN=server.mydomain.com)

  - bilinmeyen kimlik bilgileri - genellikle hata kodu eşliğinde, sertifikaya karşılık gelen eksik bir özel anahtarı gösterir: 
    ```C++ 
    0x8009030d  -2146893043 SEC_E_UNKNOWN_CREDENTIALS
    0x8009030e  -2146893042 SEC_E_NO_CREDENTIALS
    ```
    Çare bulmak için, özel anahtarın varlığını kontrol edin; SFAdmins özel anahtara 'read|execute' erişimi verilir doğrulayın.

  - kötü sağlayıcı türü - Bir Crypto New Generation (CNG) sertifikasını ("Microsoft Software Key Storage Provider"); şu anda Service Fabric yalnızca CAPI1 sertifikalarını destekler. Genellikle hata kodu eşlik ediyor:
    ```C++
    0x80090014  -2146893804 NTE_BAD_PROV_TYPE
    ```
    Çözüm bulmak için, bir CAPI1 (örneğin"Microsoft Enhanced RSA ve AES Şifreleme Sağlayıcısı") sağlayıcısını kullanarak küme sertifikasını yeniden oluşturun. Kripto sağlayıcılar hakkında daha fazla bilgi [için, Şifreleme Sağlayıcılarını Anlama](https://docs.microsoft.com/windows/win32/seccertenroll/understanding-cryptographic-providers)

