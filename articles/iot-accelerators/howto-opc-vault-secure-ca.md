---
title: OPC Vault sertifika yönetimi hizmetini güvenli bir şekilde çalıştırma - Azure | Microsoft Dokümanlar
description: Azure'da OPC Vault sertifika yönetimi hizmetinin nasıl güvenli bir şekilde çalıştırılabildiğini açıklar ve göz önünde bulundurulması gereken diğer güvenlik yönergelerini gözden geçirir.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 88f8188779c5fb6b3cd07c67e9f35a6b8f9ad97d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271713"
---
# <a name="run-the-opc-vault-certificate-management-service-securely"></a>OPC Vault sertifika yönetimi hizmetini güvenli bir şekilde çalıştırın

Bu makalede, Azure'da OPC Vault sertifika yönetimi hizmetinin güvenli bir şekilde nasıl çalıştırılacak olduğu açıklanmaktadır ve göz önünde bulundurulması gereken diğer güvenlik yönergelerigözden geçirilir.

## <a name="roles"></a>Roller

### <a name="trusted-and-authorized-roles"></a>Güvenilir ve yetkili roller

OPC Vault microservice, farklı rollerin hizmetin çeşitli bölümlerine erişmesine olanak tanır.

> [!IMPORTANT]
> Komut dosyası, dağıtım sırasında yalnızca dağıtım komut dosyasını tüm roller için kullanıcı olarak çalıştıran kullanıcıyı ekler. Bir üretim dağıtımı için, bu rol atamasını gözden geçirmeniz ve aşağıdaki yönergeleri izleyerek uygun şekilde yeniden yapılandırmanız gerekir. Bu görev, Azure Etkin Dizin (Azure AD) Kurumsal Uygulamalar portalındaki rollerin ve hizmetlerin el ile atanmasını gerektirir.

### <a name="certificate-management-service-roles"></a>Sertifika yönetimi hizmeti rolleri

OPC Vault microservice aşağıdaki rolleri tanımlar:

- **Okuyucu**: Varsayılan olarak, kiracıherhangi bir kimlik doğrulamakullanıcı erişim okudu. 
  - Uygulamalara ve sertifika isteklerine erişimi okuyun. Başvurular ve sertifika istekleri ni listeleyebilir ve sorgulayabilir. Ayrıca aygıt bulma bilgilerine ve genel sertifikalara okuma erişimiyle erişilebilir.
- **Yazar**: Yazar rolü, belirli görevler için yazma izinleri eklemek üzere bir kullanıcıya atanır. 
  - Başvurulara ve sertifika isteklerine erişimi okuma/yazma. Uygulamaları kaydedebilir, güncelleyebilir ve kayıt dışı edebilir. Sertifika istekleri oluşturabilir ve onaylı özel anahtarlar ve sertifikalar alabilir. Özel tuşları da silebilirsiniz.
- **Onaylayıcı**: Onaylayıcı rolü, sertifika isteklerini onaylamak veya reddetmek üzere bir kullanıcıya atanır. Rol başka bir rol içermez.
  - OPC Vault mikro hizmet API'sine erişmek için Onaylayıcı rolüne ek olarak, kullanıcının sertifikaları imzalayabilmek için Azure Key Vault'ta anahtar imzalama iznine sahip olması gerekir.
  - Yazar ve Onaylayıcı rolü farklı kullanıcılara atanmalıdır.
  - Onaylayan'ın ana rolü, sertifika isteklerinin yenilenmesi ve reddedilmesidir.
- **Yönetici**: Yönetici rolü, sertifika gruplarını yönetmek için bir kullanıcıya atanır. Rol Onaylayıcı rolünü desteklemez, ancak Yazar rolünü içerir.
  - Yönetici, yeni bir Sertifika İptal Listesi (CRL) düzenleyerek sertifika gruplarını yönetebilir, yapılandırmayı değiştirebilir ve uygulama sertifikalarını iptal edebilir.
  - İdeal olarak, Yazar, Onaylayıcı ve Yönetici rolleri farklı kullanıcılara atanır. Ek güvenlik için, Onaylayıcı veya Yönetici rolüne sahip bir kullanıcının, sertifika vermek veya Bir İhraççı CA sertifikasını yenilemek için Key Vault'ta anahtar imzalama iznine de ihtiyacı vardır.
  - Mikro hizmet yönetimi rolüne ek olarak, rol şunları içerir, ancak bunlarla sınırlı değildir:
    - CA'nın güvenlik uygulamalarının uygulanmasını yönetme sorumluluğu.
    - Sertifikaların neslinin yönetimi, iptali ve askıya alınması. 
    - Şifreleme anahtar yaşam döngüsü yönetimi (örneğin, Veren CA anahtarlarının yenilenmesi).
    - CA'yı çalıştıran hizmetlerin yüklenmesi, konfigürasyonu ve bakımı.
    - Hizmetlerin günlük işleyişi. 
    - CA ve veritabanı yedekleme ve kurtarma.

### <a name="other-role-assignments"></a>Diğer rol atamaları

Ayrıca hizmeti çalıştırırken aşağıdaki rolleri göz önünde bulundurun:

- Harici kök sertifika yetkilisi ile sertifika satın alma sözleşmesinin işletme sahibi (örneğin, sahibi harici bir CA'dan sertifika satın aldığında veya harici bir CA'ya bağlı bir CA çalıştırdığında).
- Sertifika Yetkilisinin geliştirilmesi ve onaylanması.
- Denetim kayıtlarının gözden geçirilmesi.
- CA'yı desteklemeye veya fiziksel ve bulut tesislerini yönetmeye yardımcı olan, ancak CA işlemlerini gerçekleştirmek için doğrudan güvenilmeyen personel *yetkili* roldedir. Yetkili roldeki kişilerin gerçekleştirmesine izin verilen görev kümesi de belgelenmelidir.

### <a name="review-memberships-of-trusted-and-authorized-roles-quarterly"></a>Güvenilir ve yetkili rollerin üyeliklerini üç ayda bir gözden geçirme

Güvenilir ve yetkili rollerin üyeliğini en az üç ayda bir gözden geçirin. Her roldeki kişi kümesinin (el ile işlemler için) veya hizmet kimlikleri (otomatik işlemler için) en az da tutulduğundan emin olun.

### <a name="role-separation-between-certificate-requester-and-approver"></a>Sertifika istektecisi ve onaylayıcısı arasındaki rol ayrımı

Sertifika verme işlemi, sertifika istektecisi ile sertifika onaylayıcısı rolleri (kişiler veya otomatik sistemler) arasındaki rol ayrımını zorunlu kılmalıdır. Sertifika verme, sertifika isteği sahibinin sertifika almaya yetkili olduğunu doğrulayan bir sertifika onaylayıcısı rolüyle yetkilendirilmelidir. Sertifika onaylayıcı rolünü elinde bulunduran kişiler resmi olarak yetkili kişi olmalıdır.

### <a name="restrict-assignment-of-privileged-roles"></a>Ayrıcalıklı rollerin atanmasını kısıtlama

Yöneticiler ve Onaylayıcılar grubunun üyeliğine yetki vermek gibi ayrıcalıklı rollerin atanmasını sınırlı bir yetkili personel kümesiyle sınırlamanız gerekir. Tüm ayrıcalıklı rol değişiklikleri erişim 24 saat içinde iptal edilmelidir. Son olarak, ayrıcalıklı rol atamalarını üç ayda bir gözden geçirin ve gereksiz veya süresi dolmuş atamaları kaldırın.

### <a name="privileged-roles-should-use-two-factor-authentication"></a>Ayrıcalıklı roller iki faktörlü kimlik doğrulaması kullanmalıdır

Hizmete Onaylayanların ve Yöneticilerin etkileşimli oturum açmaları için çok faktörlü kimlik doğrulamayı (iki faktörlü kimlik doğrulama olarak da adlandırılır) kullanın.

## <a name="certificate-service-operation-guidelines"></a>Sertifika hizmeti çalışma yönergeleri

### <a name="operational-contacts"></a>Operasyonel bağlantılar

Sertifika hizmetinin dosyasında ayrıntılı operasyonel olay yanıt kişileri içeren güncel bir güvenlik yanıt planı olmalıdır.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Tüm sistemler en son güvenlik güncelleştirmeleriyle sürekli olarak izlenmeli ve güncellenmelidir.

> [!IMPORTANT]
> OPC Vault hizmetinin GitHub deposu sürekli olarak güvenlik yamaları ile güncellenir. Bu güncelleştirmeleri izleyin ve düzenli aralıklarla hizmete uygulayın.

### <a name="security-monitoring"></a>Güvenliği izleme

Abone olun veya uygun güvenlik izleme uygulayın. Örneğin, merkezi bir izleme çözümüne (Azure Güvenlik Merkezi veya Office 365 izleme çözümü gibi) abone olun ve güvenlik olaylarının izleme çözümüne iletilmesini sağlamak için uygun şekilde yapılandırın.

> [!IMPORTANT]
> Varsayılan olarak, OPC Vault hizmeti bir izleme çözümü olarak [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/devops) ile dağıtılır. [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) gibi bir güvenlik çözümü eklemenizi şiddetle tavsiye edilir.

### <a name="assess-the-security-of-open-source-software-components"></a>Açık kaynak yazılım bileşenlerinin güvenliğini değerlendirmek

Bir ürün veya hizmet te kullanılan tüm açık kaynak bileşenleri orta veya daha büyük güvenlik açıklarından arındırılmış olmalıdır.

> [!IMPORTANT]
> Sürekli tümleştirme oluşturmaları sırasında, OPC Vault hizmetinin GitHub deposu tüm bileşenleri güvenlik açıklarına karşı tarar. Bu güncelleştirmeleri GitHub'da izleyin ve düzenli aralıklarla servise uygulayın.

### <a name="maintain-an-inventory"></a>Envanteri koruma

Tüm üretim ana bilgisayarları (kalıcı sanal makineler dahil), aygıtlar, tüm dahili IP adres aralıkları, VIP'ler ve genel DNS alan adları için bir varlık envanterini koruyun. Bir sistem, cihaz IP adresi, VIP veya herkese açık DNS etki alanı eklediğinizde veya kaldırdığınızda, envanteri 30 gün içinde güncelleştirmeniz gerekir.

#### <a name="inventory-of-the-default-azure-opc-vault-microservice-production-deployment"></a>Varsayılan Azure OPC Vault mikro hizmet üretim dağıtımıenvanteri 

Azure'da:
- **Uygulama Hizmet Planı**: Hizmet barındıran lar için uygulama hizmet planı. Varsayılan S1.
- Mikro hizmet için **Uygulama Hizmeti:** OPC Vault servis ana bilgisayarı.
- Örnek uygulama için **Uygulama Hizmeti:** OPC Vault örnek uygulama ana bilgisayarı.
- **Anahtar Vault Standardı**: Web hizmetleri için sırları ve Azure Cosmos DB anahtarlarını depolamak için.
- **Key Vault Premium**: İhraççı CA anahtarlarını barındırmak, imza hizmeti için ve uygulama özel anahtarlarının kasa konfigürasyonu ve depolanması için.
- **Azure Cosmos DB**: Uygulama ve sertifika istekleri için veritabanı. 
- **Uygulama Öngörüleri**: (isteğe bağlı) Web hizmeti ve uygulaması için izleme çözümü.
- **Azure AD Uygulama Kaydı**: Örnek uygulama, hizmet ve kenar modülü için bir kayıt.

Bulut hizmetleri için, hizmeti dağıtmak için kullanılan tüm ana bilgisayar adları, kaynak grupları, kaynak adları, abonelik kimlikleri ve kiracı kimlikleri belgelenmelidir. 

Azure IoT Edge veya yerel bir IoT Edge sunucusunda:
- **OPC Vault IoT Edge modülü**: Bir fabrika ağını desteklemek için OPC UA Global Discovery Server. 

IoT Edge aygıtları için ana bilgisayar adları ve IP adresleri belgelenmelidir. 

### <a name="document-the-certification-authorities-cas"></a>Belgelendirme Yetkililerini (CA) Belgele

CA hiyerarşisi belgeleri, işletilen tüm CA'ları içermelidir. Bu, hizmet tarafından yönetilmeseler bile ilgili tüm alt CA'ları, ana CA'ları ve kök CA'ları içerir. Resmi belgeler yerine, süresi dolmamış tüm CA sertifikalarının ayrıntılı bir kümesini sağlayabilirsiniz.

> [!NOTE]
> OPC Vault örnek uygulaması, dokümantasyon hizmetinde kullanılan ve üretilen tüm sertifikaların indirilmelerini destekler.

### <a name="document-the-issued-certificates-by-all-certification-authorities-cas"></a>Tüm Sertifika Yetkilileri (CA) tarafından verilen sertifikaları belgeleme

Son 12 ay içinde verilen tüm sertifikaların kapsamlı bir setini sağlayın.

> [!NOTE]
> OPC Vault örnek uygulaması, dokümantasyon hizmetinde kullanılan ve üretilen tüm sertifikaların indirilmelerini destekler.

### <a name="document-the-standard-operating-procedure-for-securely-deleting-cryptographic-keys"></a>Şifreleme anahtarlarını güvenli bir şekilde silen standart çalışma yordamını belgeleme

CA'nın ömrü boyunca anahtar silme işlemi yalnızca nadiren gerçekleşebilir. Bu nedenle hiçbir kullanıcının Anahtar Kasa Sertifikası Silme hakkı yoktur ve bu nedenle Bir İhraççı CA sertifikasını silmeye açık HIÇBIR API yoktur. Sertifika yetkilisi şifreleme anahtarlarını güvenli bir şekilde silen manuel standart kullanım prosedürü, yalnızca Azure portalındaki Key Vault'a doğrudan erişerek kullanılabilir. Anahtar Kasası'ndaki sertifika grubunu da silebilirsiniz. Anında silinmesini sağlamak için Key [Vault yumuşak silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) işlevini devre dışı edin.

## <a name="certificates"></a>Sertifikalar

### <a name="certificates-must-comply-with-minimum-certificate-profile"></a>Sertifikalar minimum sertifika profiline uygun olmalıdır

OPC Vault hizmeti, abonelere son varlık sertifikaları veren çevrimiçi bir CA'dır. OPC Vault microservice varsayılan uygulamada bu yönergeleri izler.

- Tüm sertifikalar aşağıda belirtildiği gibi aşağıdaki X.509 alanlarını içermelidir:
  - Sürüm alanının içeriği v3 olmalıdır. 
  - SerialNumber alanının içeriği, FIPS (Federal Bilgi İşlem Standartları) 140 onaylı rasgele sayı üretecisinden elde edilen entropi nin en az 8 baytını içermelidir.<br>
    > [!IMPORTANT]
    > OPC Vault seri numarası varsayılan olarak 20 bayttır ve işletim sistemi kriptografik rasgele sayı üretecisinden elde edilir. Rasgele sayı üreteci FIPS 140 Windows cihazlarda onaylı, ancak Linux üzerinde değil. Linux VM'leri veya Linux docker kapsayıcıları kullanan ve temel teknoloji OpenSSL'nin FIPS 140 onaylı olmadığı bir hizmet dağıtımı seçerken bunu göz önünde bulundurun.
  - VerenUniqueID ve subjectUniqueID alanları mevcut olmamalıdır.
  - Son varlık sertifikaları, IETF RFC 5280 uyarınca temel kısıtlama uzantısı ile tanımlanmalıdır.
  - Veren CA sertifikası için pathLenConstraint alanı 0 olarak ayarlanmalıdır. 
  - Genişletilmiş Anahtar Kullanımı uzantısı mevcut olmalı ve genişletilmiş anahtar kullanım nesne tanımlayıcıları (OSB) en az kümesini içermelidir. AnyExtendedKeyUsage OID (2.5.29.37.0) belirtilmemelidir. 
  - CRL Dağıtım Noktası (CDP) uzantısı İhraççı CA sertifikasında bulunmalıdır.<br>
    > [!IMPORTANT]
    > CDP uzantısı OPC Vault CA sertifikalarında bulunur. Bununla birlikte, OPC UA aygıtları CRL'leri dağıtmak için özel yöntemler kullanır.
  - Yetkili Bilgi Erişimi uzantısı abone belgelerinde bulunmalıdır.<br>
    > [!IMPORTANT]
    > Yetki Bilgilerine Erişim uzantısı OPC Vault abone belgelerinde mevcuttur. Bununla birlikte, OPC UA aygıtları İhraççı CA bilgilerini dağıtmak için özel yöntemler kullanır.
- Onaylı asimetrik algoritmalar, anahtar uzunlukları, karma fonksiyonlar ve dolgu modları kullanılmalıdır.
  - RSA ve SHA-2 desteklenen tek algoritmalardır.
  - RSA şifreleme, anahtar değişimi ve imza için kullanılabilir.
  - RSA şifreleme yalnızca OAEP, RSA-KEM veya RSA-PSS doldurma modlarını kullanmalıdır.
  - 2048 bit'ten büyük veya eşit anahtar uzunlukları gereklidir.
  - SHA-2 karma algoritmaailesi (SHA256, SHA384 ve SHA512) kullanın.
  - 20 yıldan daha büyük veya eşit olan tipik bir ömür boyu 4096 bit veya daha büyük olan RSA Root CA anahtarları olmalıdır.
  - RSA Veren CA tuşları en az 2048 bit olmalıdır. CA sertifikasının son kullanma tarihi 2030'dan sonraysa, CA anahtarı 4096 bit veya daha büyük olmalıdır.
- Sertifika ömrü
  - Kök CA sertifikaları: Kök CA'lar için maksimum sertifika geçerlilik süresi 25 yılı geçmemelidir.
  - Alt CA veya çevrimiçi İhraççı CA sertifikaları: Çevrimiçi olan ve yalnızca abone sertifikası veren CA'ların maksimum sertifika geçerlilik süresi 6 yılı geçmemelidir. Bu C'ler için, ilgili özel imza anahtarı yeni sertifikalar vermek için 3 yıldan uzun süre kullanılmamalıdır.<br>
    > [!IMPORTANT]
    > Dış Root CA olmadan varsayılan OPC Vault microservice oluşturulan Veren sertifika, ilgili gereksinimleri ve yaşam ları ile, bir online Alt CA gibi kabul edilir. Varsayılan ömür, 2048'den büyük veya eşit bir anahtar uzunluğuyla 5 yıla ayarlanır.
  - Tüm asimetrik anahtarlar en fazla 5 yıllık bir kullanım ömrüne ve önerilen 1 yıllık kullanım ömrüne sahip olmalıdır.<br>
    > [!IMPORTANT]
    > Varsayılan olarak, OPC Vault ile verilen başvuru sertifikalarının kullanım ömürleri 2 yıldır ve her yıl değiştirilmelidir. 
  - Bir sertifika yenilendiğinde, yeni bir anahtarla yenilenir.
- Uygulama örneği sertifikalarında OPC UA'ya özel uzantılar
  - ÖzneAltName uzantısı uygulama Uri ve ana bilgisayar adlarını içerir. Bunlar FQDN, IPv4 ve IPv6 adreslerini de içerebilir.
  - AnahtarKullanım digitalSignature, nonRepudiation, keyEncipherment ve dataEncipherment içerir.
  - Genişletilmiş KeyUsage serverAuth ve clientAuth içerir.
  - AuthorityKeyIdentifier imzalı sertifikalarda belirtilir.

### <a name="ca-keys-and-certificates-must-meet-minimum-requirements"></a>CA anahtarları ve sertifikaları minimum gereksinimleri karşılamalıdır

- **Özel tuşlar**: RSA tuşları en az 2048 bit olmalıdır. CA sertifikasının son kullanma tarihi 2030'dan sonraysa, CA anahtarı 4096 bit veya daha büyük olmalıdır.
- **Ömür Süresi**: Çevrimiçi olan ve yalnızca abone sertifikası veren C'ler için maksimum sertifika geçerlilik süresi 6 yılı geçmemelidir. Bu C'ler için, ilgili özel imza anahtarı yeni sertifikalar vermek için 3 yıldan uzun süre kullanılmamalıdır.

### <a name="ca-keys-are-protected-using-hardware-security-modules"></a>CA tuşları Donanım Güvenlik Modülleri kullanılarak korunur

OpcVault Azure Key Vault Premium kullanır ve tuşlar FIPS 140-2 Düzey 2 Donanım Güvenlik Modülleri (HSM) tarafından korunur. 

Key Vault'un kullandığı şifreleme modülleri, HSM veya yazılım olsun, FIPS doğrulanır. HSM korumalı olarak oluşturulan veya içe aktarılan anahtarlar, BIR HSM içinde işlenir ve FIPS 140-2 Düzey 2'ye doğrulanır. Yazılım korumalı olarak oluşturulan veya içe aktarılan anahtarlar, FIPS 140-2 Düzey 1'e doğrulanmış şifreleme modülleri içinde işlenir.

## <a name="operational-practices"></a>Operasyonel uygulamalar

### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-enrollment"></a>Sertifika kaydı için standart operasyonel PKI uygulamalarını belgeleme ve koruma

Örneğin, ÖRNEĞIN CA'ların sertifikaları nasıl düzenleyene ilişkin standart çalışma prosedürlerini (SOP'leri) belgele ve koruyun: 
- Abonenin nasıl tanımlanır ve kimlik doğrulaması yapılır. 
- Sertifika isteğinin nasıl işlenir ve doğrulanır (varsa, sertifika yenileme ve yeniden anahtar isteklerinin nasıl işlenir olduğunu da içerir). 
- Verilen sertifikaların abonelere nasıl dağıtıldığı. 

OPC Vault microservice [SOP, OPC Vault mimarisinde](overview-opc-vault-architecture.md) ve [OPC Vault sertifika hizmetinde](howto-opc-vault-manage.md)tanımlanmıştır. Uygulamalar "OPC Birleşik Mimari Belirtimi Bölüm 12: Discovery and Global Services" uygulamalarını takip eder.


### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-revocation"></a>Sertifika iptali için standart operasyonel PKI uygulamalarını belgeleme ve koruma

Sertifika iptal işlemi [OPC Vault mimarisinde](overview-opc-vault-architecture.md) açıklanmıştır ve [OPC Vault sertifika hizmetini yönetin.](howto-opc-vault-manage.md)
    
### <a name="document-ca-key-generation-ceremony"></a>Belge CA anahtar oluşturma töreni 

Azure Key Vault'taki güvenli depolama sayesinde OPC Vault mikrohizmetindeki Veren CA anahtar üretimi basitleştirilmiştir. Daha fazla bilgi için Bkz. [OPC Vault sertifika hizmetini yönet.](howto-opc-vault-manage.md)

Ancak, harici bir Root sertifika yetkilisi kullanıyorsanız, CA anahtar oluşturma töreni aşağıdaki gereksinimlere uymalıdır.

CA anahtar oluşturma töreni, en az aşağıdaki öğeleri içeren belgelenmiş bir komut dosyasına karşı gerçekleştirilmelidir: 
- Rollerin tanımı ve katılımcı sorumlulukları.
- CA anahtar oluşturma töreninin yürütülmesi için onay.
- Tören için gerekli olan kriptografik donanım ve aktivasyon malzemeleri.
- Donanım hazırlama (varlık/yapılandırma bilgileri güncelleştirme ve imzalama dahil).
- İşletim sistemi kurulumu.
- CA anahtar oluşturma töreni sırasında gerçekleştirilen belirli adımlar, örneğin: 
  - CA uygulama yükleme ve yapılandırma.
  - CA anahtar üretimi.
  - CA tuşu yedekleme.
  - CA sertifikası imzalama.
  - Hizmetin korumalı HSM'sinde imzalı anahtarların içe aktarılır.
  - CA sistemi kapatma.
  - Depolama için malzemelerin hazırlanması.


## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Vault'u güvenli bir şekilde nasıl yönetebileceğinizi öğrendiğiniz için şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [OPC Vault ile Güvenli OPC UA cihazları](howto-opc-vault-secure.md)
