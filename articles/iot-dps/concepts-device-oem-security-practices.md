---
title: Üreticiler için güvenlik uygulamaları - Azure IoT Cihaz Sağlama Hizmeti
description: Azure IoT Aygıt Sağlama Hizmeti'ne (DPS) kaydolmak için aygıtlar hazırlayan OEM'ler ve aygıt üreticileri için genel güvenlik uygulamalarına genel bakış.
author: timlt
ms.author: timlt
ms.date: 3/02/2020
ms.topic: conceptual
ms.service: iot-dps
ms.custom: iot-p0-scenario, iot-devices-deviceOEM
ms.reviewer: nberdy
ms.openlocfilehash: 3854f353e4ea0b78c0162681e0b89d37419105d8
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529514"
---
# <a name="security-practices-for-azure-iot-device-manufacturers"></a>Azure IoT aygıt üreticileri için güvenlik uygulamaları
Daha fazla üretici IoT aygıtları serbest bıraktıkça, yaygın uygulamalarla ilgili kılavuzu belirlemek yararlıdır. Bu makalede, Azure IoT Aygıt Sağlama Hizmeti (DPS) ile kullanılmak üzere aygıtlar üretirken göz önünde bulundurulması gereken önerilen güvenlik uygulamaları özetlenmiştir.  

> [!div class="checklist"]
> * Aygıt kimlik doğrulama seçeneklerini seçme
> * IoT aygıtlarına sertifika yükleme
> * Güvenilir Platform Modülü 'nün (TPM) üretim sürecine entegre edilmesi

## <a name="selecting-device-authentication-options"></a>Aygıt kimlik doğrulama seçeneklerini seçme
Herhangi bir IoT cihaz güvenlik önleminin nihai amacı güvenli bir IoT çözümü oluşturmaktır. Ancak donanım sınırlamaları, maliyet ve güvenlik uzmanlığı düzeyi gibi sorunlar, seçtiğiniz seçeneklerin tüm etkilerini etkiler. Ayrıca, güvenliğe yaklaşımınız IoT aygıtlarınızın buluta nasıl bağlandığını etkiler. Göz önünde bulundurulması gereken [IoT güvenliğinin birkaç öğesi](https://www.microsoft.com/research/publication/seven-properties-highly-secure-devices/) olsa da, her müşterinin karşılaştığı önemli bir öğe, kullanılacak kimlik doğrulama türüdür. 

Yaygın olarak kullanılan üç kimlik doğrulama türü X.509 sertifikaları, Güvenilir Platform Modülleri (TPM) ve simetrik tuşlardır. Diğer kimlik doğrulama türleri mevcut olsa da, Azure IoT'de çözüm oluşturan çoğu müşteri bu üç türden birini kullanır. Bu makalenin geri kalanı, her kimlik doğrulama türünü kullanmanın artılarını ve eksilerini inceler.

### <a name="x509-certificate"></a>X.509 belgesi
X.509 sertifikaları kimlik doğrulama için kullanabileceğiniz bir dijital kimlik türüdür. X.509 sertifika standardı [IETF RFC 5280'de](https://tools.ietf.org/html/rfc5280)belgelenmiştir. Azure IoT'de, sertifikaların kimliğini doğrulamanın iki yolu vardır:
- Parmak izi. Bir parmak izi algoritması bir sertifika üzerinde bir hexadecimal dize oluşturmak için çalıştırılır. Oluşturulan dize, sertifika için benzersiz bir identiferdir. 
- Tam bir zincire dayalı CA kimlik doğrulaması. Sertifika zinciri, bir son varlık (EE) sertifikasını doğrulamak için gereken tüm sertifikaların hiyerarşik bir listesidir. Bir EE sertifikasının kimliğini doğrulamak için, güvenilir bir kök CA da dahil olmak üzere zincirdeki her sertifikanın kimliğini doğrulamak gerekir. 

X.509 için artıları:
- X.509, Azure IoT'de desteklenen en güvenli kimlik doğrulama türüdür.
- X.509, sertifika yönetimi amacıyla yüksek düzeyde denetim sağlar.
- Birçok satıcı X.509 tabanlı kimlik doğrulama çözümleri sağlamak için kullanılabilir.

X.509 için eksileri:
- Birçok müşterinin sertifikaları için dış satıcılara güvenmeleri gerekebilir.
- Sertifika yönetimi maliyetli olabilir ve toplam çözüm maliyetine katkıda bulunabilir.
- Lojistik iyi düşünülmezse sertifika yaşam döngüsü yönetimi zor olabilir. 

### <a name="trusted-platform-module-tpm"></a>Güvenilir Platform Modülü (TPM)
[ISO/IEC 11889](https://www.iso.org/standard/66510.html)olarak da bilinen TPM, şifreleme anahtarlarını güvenli bir şekilde oluşturmak ve depolamak için bir standarttır. TPM, standardı uygulayan modüllerle etkileşimedebilen sanal veya fiziksel bir G/Ç aygıtını da ifade eder. Bir TPM aygıtı ayrı donanım, tümleşik donanım, yazılım tabanlı bir modül veya yazılım tabanlı bir modül olarak bulunabilir. 

TPM'ler ve simetrik tuşlar arasında iki temel fark vardır: 
- TPM yongaları X.509 sertifikalarını da saklayabilir.
- DPS'deki TPM attestation, asimetrik kimlik doğrulamanın bir biçimi olan TPM onay anahtarını (EK) kullanır. Asimetrik kimlik doğrulamaile şifreleme için ortak anahtar ve şifre çözme için ayrı bir özel anahtar kullanılır. Bunun aksine, simetrik anahtarlar, özel anahtarın hem şifreleme hem de şifre çözme için kullanıldığı simetrik kimlik doğrulamasını kullanır. 

TPM için artıları:
- TPM'ler, işletim sistemi için yerleşik destekle birçok Windows aygıtında standart donanım olarak bulunur. 
- TPM attestation'ı güvenli hale getirmek, paylaşılan erişim imzası (SAS) belirteç tabanlı simetrik anahtar attestation'ından daha kolaydır.
- Aygıt kimlik bilgilerinin süresi kolayca dolabilir ve yenileyebilir veya aygıt kimlik bilgilerini yuvarlayabilirsiniz. DPS, bir TPM aygıtının yeniden sağlanması gerektiğinde IoT Hub kimlik bilgilerini otomatik olarak yuvarlar.

TPM için eksileri: 
- TPM'ler karmaşıktır ve kullanımı zor olabilir. 
- Fiziksel bir TPM veya kaliteli bir emülatör yoksa TPM'ler ile uygulama geliştirme zordur.
- Aygıtınızın yönetim kurulunu donanıma bir TPM içerecek şekilde yeniden tasarlamanız gerekebilir. 
- EK'i TPM'ye yuvarlarsanız, TPM'nin kimliğini yok eder ve yeni bir tane oluşturur. Fiziksel çip aynı kalsa da, IoT çözümünüzde yeni bir kimliğe sahiptir.

### <a name="symmetric-key"></a>Simetrik anahtar
Simetrik tuşlarda, aynı anahtar iletileri şifrelemek ve çözmek için kullanılır. Sonuç olarak, aynı anahtar hem aygıt hem de onu doğrulayan hizmet tarafından bilinir. Azure IoT, SAS belirteci tabanlı simetrik anahtar bağlantılarını destekler. Simetrik anahtar kimlik doğrulaması, anahtarları güvenli hale getirmek ve X.509 kimlik doğrulaması ile eşit düzeyde güvenlik elde etmek için önemli bir sahip sorumluluğu gerektirir. Simetrik anahtarlar kullanıyorsanız, önerilen uygulama bir donanım güvenlik modülü (HSM) kullanarak anahtarları korumaktır.

Simetrik anahtar için artıları:
- Simetrik tuşları kullanmak, kimlik doğrulamaya başlamanın en basit ve en düşük maliyetli yoludur.
- Simetrik tuşları kullanmak, üretecek ekstra bir şey olmadığından işleminizi kolaylaştırır. 

Simetrik anahtar için eksileri: 
- Simetrik tuşlar anahtarları güvenli hale getirmek için önemli ölçüde çaba gösterir. Aynı anahtar aygıt ve bulut arasında paylaşılır, bu da anahtarın iki yerde korunması gerektiği anlamına gelir. Buna karşılık, TPM ve X.509 sertifikaları ile sorun özel anahtar ortaya çıkarmadan ortak anahtar sahip kanıtlıyor.
- Simetrik tuşlar, kötü güvenlik uygulamalarını izlemeyi kolaylaştırır. Simetrik tuşlarla ortak bir eğilim, aygıtlarda şifrelenmemiş anahtarları sabit kodlamaktır. Bu uygulama uygun olsa da, anahtarları savunmasız bırakır. Simetrik anahtarı aygıtta güvenli bir şekilde depolayarak bazı riskleri azaltabilirsiniz. Ancak, önceliğiniz kolaylık tan çok güvenlikse, kimlik doğrulaması için X.509 sertifikalarını veya TPM'yi kullanın. 

### <a name="shared-symmetric-key"></a>Paylaşılan simetrik anahtar
Paylaşılan simetrik anahtar olarak bilinen simetrik anahtar kimlik doğrulamasının bir varyasyonu vardır. Bu yaklaşım, tüm cihazlarda aynı simetrik anahtarın kullanılmasını içerir. Öneri, aygıtlarınızda paylaşılan simetrik anahtarları kullanmaktan kaçınmaktır. 

Paylaşılan simetrik anahtar için Pro:
- Basit uygulamak ve ucuz ölçekte üretmek için. 

Paylaşılan simetrik anahtar için eksileri: 
- Saldırılara karşı son derece savunmasız. Kolay uygulamanın faydası riskten çok daha ağır basmaktadır. 
- Herkes paylaşılan anahtarı edinirse aygıtlarınızın kimliğine bürünebilir.
- Tehlikeye girer paylaşılan bir simetrik anahtar güveniyorsanız, büyük olasılıkla cihazların kontrolünü kaybedersiniz. 

### <a name="making-the-right-choice-for-your-devices"></a>Aygıtlarınız için doğru seçimi yapma
Kimlik doğrulama yöntemini seçmek için, benzersiz üretim süreciniz için her yaklaşımın avantajlarını ve maliyetlerini göz önünde bulundurun.  Aygıt kimlik doğrulaması için genellikle belirli bir yaklaşımın ne kadar güvenli olduğu ile ne kadar kullanışlı olduğu arasında ters bir ilişki vardır.  

## <a name="installing-certificates-on-iot-devices"></a>IoT aygıtlarına sertifika yükleme
IoT aygıtlarınızın kimliğini doğrulamak için X.509 sertifikaları kullanıyorsanız, bu bölümde sertifikaların üretim sürecinize nasıl entegre edilebildiğim konusunda kılavuz lar sunulmaktadır. Birkaç karar vermen gerekecek.  Bunlar, yaygın sertifika değişkenleri, sertifikaların ne zaman oluşturacağı ve ne zaman yüklenecekleri hakkındaki kararları içerir. 

Parola kullanmaya alışkınsanız, aynı sertifikayı neden tüm aygıtlarınızda kullanamayacağınızı, aynı şekilde tüm aygıtlarınızda aynı parolayı kullanabileceğinizi sorabilirsiniz. Birincisi, her yerde aynı parolayı kullanmak tehlikelidir. Bu uygulama, şirketleri birkaç yıl önce ABD'nin Doğu Kıyısı'nda DNS'yi deviren de dahil olmak üzere büyük DDoS saldırılarına maruz bıraktı. Kişisel hesaplarda bile her yerde aynı parolayı kullanmayın. İkincisi, sertifika parola değildir, benzersiz bir kimlikdir. Parola, herkesin güvenli bir binada kapıyı açmak için kullanabileceği gizli bir kod gibidir.  Bu senin bildiğin bir şey ve giriş için herkese şifreyi verebilirsin.  Sertifika, güvenli bir binaya girmek için bir gardiyana gösterebileceğiniz fotoğraf Ve diğer ayrıntıların bulunduğu bir sürücü ehliyeti gibidir. Kim olduğunla bağlantılı.  Korumanın sürücü ehliyeti olan kişilerle doğru bir şekilde eşleşmesi koşuluyla, giriş için yalnızca siz lisansınızı (kimliğinizi) kullanabilirsiniz. 

### <a name="variables-involved-in-certificate-decisions"></a>Sertifika kararlarında yer alan değişkenler
Aşağıdaki değişkenleri ve her birinin genel üretim sürecini nasıl etkilediğini göz önünde bulundurun. 

#### <a name="where-the-certificate-root-of-trust-comes-from"></a>Güven sertifika kökü nereden geliyor
Ortak anahtar altyapıyı (PKI) yönetmek maliyetli ve karmaşık olabilir.  Özellikle de şirketinizin bir PKI'yı yönetme deneyimi yoksa. Seçenekleriniz şunlardır:
- Üçüncü taraf pki kullanın. Ara imza sertifikalarını üçüncü taraf sertifika satıcısından satın alabilirsiniz. Veya özel bir Sertifika Yetkilisi (CA) kullanabilirsiniz. 
- Kendi kendini yöneten bir PKI kullanın. Kendi PKI sisteminizi koruyabilir ve kendi sertifikalarınızı oluşturabilirsiniz.
- Azure [Küre](https://azure.microsoft.com/services/azure-sphere/) güvenlik hizmetini kullanın. Bu seçenek yalnızca Azure Küre aygıtları için geçerlidir. 

#### <a name="where-certificates-are-stored"></a>Sertifikaların depolandığı yerler
Sertifikaların depolandığı yerdeki kararı etkileyen birkaç faktör vardır. Bu etkenler, aygıtın türünü, beklenen kar marjlarını (güvenli depolamayı karşılayıp karşılayamayacağınızı), aygıt özelliklerini ve aygıtta kullanabileceğiniz mevcut güvenlik teknolojisini içerir. Aşağıdaki seçenekleri göz önünde bulundurun:
- Donanım güvenlik modülünde (HSM). HSM kullanılması şiddetle tavsiye edilir. Cihazınızın kontrol panosunda hsm yüklü olup olmadığını kontrol edin. HSM'niz olmadığını biliyorsanız, ihtiyaçlarınızı karşılayan bir HSM belirlemek için donanım üreticinizle birlikte çalışın.
- Güvenilir bir yürütme ortamı (TEE) gibi diskte güvenli bir yerde.
- Yerel dosya sisteminde veya sertifika deposunda. Örneğin, Windows sertifika mağazası. 

#### <a name="connectivity-at-the-factory"></a>Fabrikada bağlantı
Fabrikadaki bağlantı, aygıtlara yüklemek için sertifikaları niçin ve ne zaman alacağınızı belirler. Bağlantı seçenekleri aşağıdaki gibidir:
- Bağlantı. Bağlantının en uygun olması, yerel sertifika oluşturma işlemini kolaylaştırır. 
- Bağlantı yok. Bu durumda, aygıt sertifikalarını yerel ve çevrimdışı oluşturmak için CA'dan imzalı bir sertifika kullanırsınız. 
- Bağlantı yok. Bu durumda, önceden oluşturulan sertifikaları elde edebilirsiniz. Veya yerel olarak sertifika oluşturmak için çevrimdışı bir PKI kullanabilirsiniz.

#### <a name="audit-requirement"></a>Denetim gereksinimi
Ürettiğiniz aygıttürüne bağlı olarak, aygıt kimliklerinin aygıtlarınıza nasıl yüklendiğinden ilgili bir denetim izi oluşturmak için düzenleyici bir gereksinime sahip olabilirsiniz. Denetim önemli üretim maliyeti ekler. Yani çoğu durumda, sadece gerekirse bunu. Bir denetim gerekip gerekmediği konusunda emin değilseniz, şirketinizin hukuk departmanına danışın. Denetim seçenekleri şunlardır: 
- Hassas bir endüstri değil. Denetim gerekmez.
- Hassas endüstri. Sertifikalar, uygunluk belgelendirme gerekliliklerine göre güvenli bir odaya yerleştirilmelidir. Sertifikaları yüklemek için güvenli bir odaya ihtiyacınız varsa, büyük olasılıkla sertifikaların aygıtlarınıza nasıl yüklendiğinden haberdar olabilirsiniz. Ve muhtemelen zaten bir denetim sistemi var. 

#### <a name="length-of-certificate-validity"></a>Sertifika geçerlilik süresi
Sürücü ehliyeti gibi, sertifikaların da oluşturulduklarında ayarlanan bir son kullanma tarihi vardır. Sertifika geçerlilik süresi seçenekleri şunlardır:
- Yenileme gerekli değildir.  Bu yaklaşım uzun bir yenileme süresi kullanır, bu nedenle aygıtın ömrü boyunca sertifikayı yenilemeniz gerekmez. Böyle bir yaklaşım uygun olsa da, aynı zamanda riskli.  Aygıtlarınızda HSM gibi güvenli depolama alanı kullanarak riski azaltabilirsiniz. Ancak, önerilen uygulama uzun ömürlü sertifikaları kullanmaktan kaçınmaktır.
- Yenileme gereklidir.  Sertifikayı aygıtın ömrü boyunca yenilemeniz gerekir. Sertifika geçerliliğinin uzunluğu içeriğe bağlıdır ve yenileme için bir stratejiye ihtiyacınız vardır.  Strateji, sertifika aldığınız yeri ve cihazlarınızın yenileme işleminde ne tür bir havadan işlevsellik kullanması gerektiğini içermelidir. 

### <a name="when-to-generate-certificates"></a>Sertifikalar ne zaman üretili
Fabrikanızdaki internet bağlantısı özellikleri sertifika oluşturma sürecinizi etkileyecektir. Sertifika oluşturmanız için birkaç seçeneğiniz vardır: 

- Önceden yüklenmiş sertifikalar.  Bazı HSM satıcıları, HSM satıcısının müşteri için sertifika yüklediği bir premium hizmet sunar. İlk olarak, müşteriler HSM satıcısına imza sertifikasına erişim sağlar. Ardından HSM satıcısı, müşterinin satın aldığı her HSM'ye imza sertifikası tarafından imzalanmış sertifikaları yükler. Müşterinin tek yapması gereken HSM'yi cihaza yüklemek. Bu hizmet maliyet eklerken, üretim sürecinizi kolaylaştırmaya yardımcı olur.  Ve sertifikaları yüklemek için ne zaman sorusunu çözer.
- Aygıt tarafından oluşturulan sertifikalar.  Aygıtlarınız dahili sertifikalar oluşturuyorsa, DPS'ye kaydetmek için aygıttan ortak X.509 sertifikasını çıkarmanız gerekir. 
- Bağlı fabrika.  Fabrikanızda bağlantı varsa, ihtiyacınız olduğunda aygıt sertifikaları oluşturabilirsiniz.
- Kendi PKI ile çevrimdışı fabrika. Fabrikanızda bağlantı yoksa ve çevrimdışı destekle kendi PKI'nızı kullanıyorsanız, sertifikaları ihtiyacınız olduğunda oluşturabilirsiniz.
- Üçüncü taraf PKI ile çevrimdışı fabrika. Fabrikanızda bağlantı yoksa ve üçüncü taraf bir PKI kullanıyorsanız, sertifikaları önceden oluşturmanız gerekir. Ayrıca, sertifikaları bağlantı olan bir konumdan oluşturmak gerekir. 

### <a name="when-to-install-certificates"></a>Sertifikalar ne zaman yüklenir?
IoT aygıtlarınız için sertifikalar oluşturduktan sonra, bunları aygıtlara yükleyebilirsiniz. 

HSM ile önceden yüklenmiş sertifikalar kullanıyorsanız, işlem basitleştirilmiştir. Cihaza HSM yüklendikten sonra aygıt kodu bu aygıta erişebilir. Ardından, HSM'de depolanan sertifikaya erişmek için HSM API'lerini ararsınız. Bu yaklaşım, üretim süreciniz için en uygun yaklaşımdır. 

Önceden yüklenmiş bir sertifika kullanmıyorsanız, sertifikayı üretim işleminizin bir parçası olarak yüklemeniz gerekir. En basit yaklaşım, sertifikayı ilk firmware görüntüsünü yanıp söndediğiniz anda HSM'ye yüklemektir. İşleminiz, görüntüyü her cihaza yüklemek için bir adım eklemelidir. Bu adımdan sonra, aygıtı paketleyip göndermeden önce son kalite kontrollerini ve diğer adımları çalıştırabilirsiniz. 

Yükleme işlemini ve son kalite denetimini tek bir adımda çalıştırmanızı sağlayan yazılım araçları vardır. Bu araçları sertifika oluşturmak veya önceden oluşturulmuş bir sertifika deposundan sertifika çekmek için değiştirebilirsiniz. Daha sonra yazılım, sertifikayı yüklemeniz gereken yere yükleyebilir. Bu tür yazılım araçları, üretim kalitesinde üretim ölçeğinde çalıştırmanızı sağlar. 

Aygıtlarınıza sertifika yükledikten sonra, bir sonraki adım aygıtları [DPS'ye](about-iot-dps.md)nasıl kaydedilen öğrenmektir. 

## <a name="integrating-a-tpm-into-the-manufacturing-process"></a>Bir TPM'yi üretim sürecine entegre etme
IoT aygıtlarınızın kimliğini doğrulamak için bir TPM kullanıyorsanız, bu bölüm kılavuz sunar. Kılavuz, karma tabanlı ileti kimlik doğrulama kodu (HMAC) anahtar desteğine sahip yaygın olarak kullanılan TPM 2.0 aygıtlarını kapsar. TPM yongaları için TPM belirtimi, Güvenilir Bilgi İşlem Grubu tarafından korunan bir ISO standardıdır. TPM hakkında daha fazla şey için [TPM 2.0](https://trustedcomputinggroup.org/tpm-library-specification/) ve [ISO/IEC 11889](https://www.iso.org/standard/66510.html)teknik özelliklerine bakın. 

### <a name="taking-ownership-of-the-tpm"></a>TPM'nin mülkiyetini almak
TPM çipli bir aygıtın üretiminde kritik bir adım, TPM'nin mülkiyetini almaktır. Bu adım, aygıt sahibine bir anahtar sağlayabilmeniz için gereklidir. İlk adım, onay anahtarını (EK) aygıttan çıkarmaktır. Bir sonraki adım aslında sahiplik iddia etmektir. Bunu nasıl başardığınız, hangi TPM'yi ve işletim sistemini kullandığınıza bağlıdır. Gerekirse, sahiplik talebinde bulunmayı belirlemek için TPM üreticisine veya aygıt işletim sisteminin geliştiricisine başvurun. 

Üretim sürecinizde, EK'i ayıklayabilir ve farklı zamanlarda sahiplik talebinde bulunabilirsiniz, bu da esneklik sağlar. Birçok üretici, cihazlarının güvenliğini artırmak için bir donanım güvenlik modülü (HSM) ekleyerek bu esneklikten yararlanır. Bu bölümde EK'nin ne zaman ayıklanılması, TPM'nin sahipliğini ne zaman talep edilecek ve bu adımları bir üretim zaman çizelgesine entegre etmek için göz önünde bulundurulması gereken ler hakkında kılavuz lar sağlanmaktadır. 

> [!IMPORTANT]
> Aşağıdaki kılavuz, ayrı bir yazılım, yazılım veya tümleşik TPM kullandığınızı varsayar. Geçerli olduğu yerlerde, kılavuz, ayrık olmayan bir TPM veya yazılım kullanarak notlar ekler. Bir yazılım TPM kullanıyorsanız, bu kılavuzun içermediği ek adımlar olabilir. Yazılım TPM'leri, bu makalenin kapsamı dışında çeşitli uygulamalara sahiptir.  Genel olarak, bir yazılım TPM'yi aşağıdaki genel üretim zaman çizelgesine entegre etmek mümkündür. Ancak, tpm taklit edilmiş bir yazılım prototipoluşturma ve sınama için uygun olsa da, ayrık, firmware veya tümleşik TPM ile aynı güvenlik düzeyini sağlayamaz. Genel bir uygulama olarak, üretimde bir yazılım TPM kullanmaktan kaçının.

### <a name="general-manufacturing-timeline"></a>Genel üretim zaman çizelgesi
Aşağıdaki zaman çizelgesi, Bir TPM'nin üretim sürecinden nasıl geçtiğini ve bir aygıtta nasıl son lakarşındığını gösterir. Her üretim işlemi benzersizdir ve bu zaman çizelgesi en yaygın desenleri gösterir. Zaman çizelgesi, anahtarlarla belirli eylemlerin ne zaman gerçekleşeceğinden ilgili rehberlik sunar. 

#### <a name="step-1-tpm-is-manufactured"></a>Adım 1: TPM üretilmiştir
- Aygıtlarınızda kullanılmak üzere bir üreticiden TPM satın alırsanız, sizin için genel onay anahtarlarını (EK_pubs) ayıklayacaklar mı diye bakın. Üretici, sevk edilen aygıtlarla EK_pubs listesini sağlarsa yararlıdır. 
    > [!NOTE]
    > Sağlama hizmetinizdeki paylaşılan erişim ilkelerini kullanarak TPM üreticisine kayıt listenize erişim izni verebilirsiniz.  Bu yaklaşım, tpm'leri sizin için kayıt listenize eklemelerine olanak tanır.  Ama bu üretim sürecinin erken ve TPM üreticisigüven gerektirir. Bunu kendi riskinle yap. 

- Cihaz üreticilerine satmak için TPM'ler üretiyorsanız, müşterilerinize fiziksel TPM'leri ile birlikte EK_pubs bir listesini vermeyi düşünün.  Müşterilere EK_pubs sağlamak, süreçlerinde bir adım kaydeder. 
- Kendi cihazlarınızla kullanmak üzere TPM'ler üretiyorsanız, işlemdeki hangi noktanın EK_pub ayıklamak için en uygun nokta olduğunu belirleyin. Zaman çizelgesinde kalan noktalardan herhangi birinde EK_pub ayıklayabilirsiniz. 

#### <a name="step-2-tpm-is-installed-into-a-device"></a>Adım 2: TPM bir aygıta yüklenir
Üretim sürecinin bu noktasında, aygıtın hangi DPS örneğiyle kullanılacağını bilmeniz gerekir. Sonuç olarak, otomatik sağlama için kayıt listesine aygıtlar ekleyebilirsiniz. Otomatik aygıt sağlama hakkında daha fazla bilgi için [DPS belgelerine](about-iot-dps.md)bakın.
- Eğer EK_pub ayıklamak değil, şimdi bunu yapmak için iyi bir zaman. 
- TPM'nin yükleme işlemine bağlı olarak, bu adım TPM'nin sahipliğini almak için de iyi bir zamandır. 

#### <a name="step-3-device-has-firmware-and-software-installed"></a>Adım 3: Cihaz firmware ve yazılım yüklü
Bu işlem de, DPS istemcisini kimlik kapsamı ve sağlama için genel URL ile birlikte yükleyin.
- Şimdi EK_pub çıkarmak için son şans. Yazılımı cihazınıza üçüncü bir taraf yüklerse, önce EK_pub ayıklamak iyi bir fikirdir.
- Üretim sürecindeki bu nokta TPM'nin mülkiyetini almak için idealdir.  
    > [!NOTE]
    > Bir yazılım TPM kullanıyorsanız, şimdi yükleyebilirsiniz.  Aynı anda EK_pub ayıklayın.

#### <a name="step-4-device-is-packaged-and-sent-to-the-warehouse"></a>Adım 4: Cihaz paketlenir ve ambara gönderilir
Bir aygıt dağıtılmadan önce 6-12 ay ambarda oturabilir. 

#### <a name="step-5-device-is-installed-into-the-location"></a>Adım 5: Aygıt konuma yüklenir
Cihaz son yerine geldikten sonra DPS ile otomatik sağlama dan geçer.

Daha fazla bilgi için, [otoprovizyon kavramları](concepts-auto-provisioning.md) ve [TPM attestation](concepts-tpm-attestation.md)bakın. 

## <a name="resources"></a>Kaynaklar

Azure IoT, bu makalede önerilen güvenlik uygulamalarına ek olarak, güvenli donanım seçme ve güvenli IoT dağıtımları oluşturma konusunda yardımcı olacak kaynaklar sağlar: 
- Dağıtım işlemine rehberlik etmek için Azure IoT [güvenlik önerileri.](../iot-fundamentals/security-recommendations.md) 
- [Azure Güvenlik Merkezi,](https://azure.microsoft.com/services/security-center/) güvenli IoT dağıtımları oluşturmaya yardımcı olacak bir hizmet sunar. 
- Donanım ortamınızı değerlendirmekonusunda yardım için [IoT Güvenliğinizi Değerlendiren](https://download.microsoft.com/download/D/3/9/D3948E3C-D5DC-474E-B22F-81BA8ED7A446/Evaluating_Your_IOT_Security_whitepaper_EN_US.pdf)teknik incelemeye bakın. 
- Güvenli donanım seçimiyle ilgili yardım [için, IoT Dağıtımınız için Doğru Güvenli Donanım'a](https://download.microsoft.com/download/C/0/5/C05276D6-E602-4BB1-98A4-C29C88E57566/The_right_secure_hardware_for_your_IoT_deployment_EN_US.pdf)bakın. 