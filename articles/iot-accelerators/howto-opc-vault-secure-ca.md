---
title: OPC Kasası sertifika yönetimi hizmetini güvenli bir şekilde çalıştırma-Azure | Microsoft Docs
description: OPC Kasası sertifika yönetimi hizmetini Azure 'da güvenli bir şekilde çalıştırmayı ve göz önünde bulundurmanız gereken diğer güvenlik kılavuzlarını açıklar.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: b08358680793ccdadca27c5f2aa57fbffe89b53a
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69973798"
---
# <a name="how-to-run-the-opc-ua-certificate-management-service-securely"></a>OPC UA sertifika yönetimi hizmetini güvenli bir şekilde çalıştırma

Bu makalede, OPC UA sertifika yönetimi hizmetinin Azure 'da güvenli bir şekilde çalıştırılması ve göz önünde bulundurulması gereken diğer güvenlik yönergeleri açıklanmaktadır.

## <a name="roles"></a>Roller

### <a name="trusted-and-authorized-roles"></a>Güvenilen ve yetkili roller

OPC kasa mikro hizmeti, farklı rollerin hizmetin çeşitli bölümlerine erişmesine izin verecek şekilde yapılandırılmıştır.

> [!IMPORTANT]
> Dağıtım sırasında, komut dosyası yalnızca dağıtım betiğini çalıştıran kullanıcıyı tüm roller için Kullanıcı olarak ekler.
> Bu rol ataması, bir üretim dağıtımı için incelenmeli ve aşağıdaki kurallara uygun şekilde yeniden yapılandırılmalıdır.
> Bu görev, Azure AD kurumsal uygulamalar portalında rol ve hizmetlerin el ile atanmasını gerektirir.

### <a name="certificate-management-service-roles"></a>Sertifika Yönetimi hizmet rolleri

Mikro hizmet aşağıdaki rolleri tanımlar:

- **Okuyucu**: Varsayılan olarak, Kiracıdaki tüm kimliği doğrulanmış kullanıcılar okuma erişimine sahiptir. 
  - Uygulamalar ve sertifika isteklerine yönelik okuma erişimi. Uygulamalar ve sertifika isteklerini listeleyebilir ve sorgulayabilir. Ayrıca, cihaz bulma bilgilerine ve genel sertifikalara okuma erişimiyle erişilebilir.
- **Yazıcı**: Yazıcı rolü bir kullanıcıya belirli görevler için yazma izinleri eklemek üzere atanır. 
  - Uygulamalar ve sertifika isteklerine yönelik okuma/yazma erişimi. Uygulamaları kaydedebilir, güncelleştirebilir ve kayıt silebilirsiniz. Sertifika istekleri oluşturabilir ve onaylanan özel anahtarlar ve sertifikalar alabilir. , Özel anahtarları da silebilir.
- **Onaylayan**: Onaylayan rolü, sertifika isteklerini onaylamak veya reddetmek için bir kullanıcıya atanır. Rol başka herhangi bir rol içermez.
  - OPC Kasası mikro hizmet API 'Sine erişmek için onaylayanın rolüne ek olarak, kullanıcıların sertifikaları imzalayabilmesi için Key Vault anahtar imzalama iznine de sahip olması gerekir.
  - Yazıcı ve onaylayan rolü farklı kullanıcılara atanmalıdır.
  - Onaylayanın ana rolü, sertifika isteklerinin oluşturulması ve redlini onaysıdır.
- **Yönetici**: Yönetici rolü bir kullanıcıya sertifika gruplarını yönetmek üzere atanır. Rol onaylayan rolünü desteklemez, ancak yazıcı rolünü içerir.
  - Yönetici, sertifika gruplarını yönetebilir, yapılandırmayı değiştirebilir ve yeni bir CRL vererek uygulama sertifikalarını iptal edebilir.
  - İdeal, yazıcı, onaylayan ve yönetici rolleri farklı kullanıcılara atanır. Ek güvenlik için, onaylayan veya yönetici rolüne sahip bir kullanıcının sertifika vermek veya bir veren CA sertifikasını yenilemek üzere anahtar kasasında de anahtar imzalama izni olması gerekir.
  - Mikro hizmet Yönetim rolüne ek olarak, rol de dahil değildir ancak bunlarla sınırlı değildir:
    - CA 'nın güvenlik uygulamalarının uygulanmasını yönetmekten sorumludur.
    - Sertifikaların oluşturulması, iptali ve askıya alınma yönetimi. 
    - Şifreleme anahtarı yaşam döngüsü yönetimi (örneğin, veren CA anahtarlarının yenilenmesi).
    - CA 'yı çalışan hizmetlerin yüklenmesi, yapılandırılması ve bakımı.
    - Hizmetlerin gündelik işlemi. 
    - CA ve veritabanı yedekleme ve kurtarma.

### <a name="other-role-assignments"></a>Diğer rol atamaları

Hizmet çalıştırılırken aşağıdaki roller de dikkate alınmalıdır ve atanmalıdır:

- Dış kök sertifika yetkilisine sahip sertifika tedarik sözleşmesinin iş sahibi (sahibi, bir dış CA 'dan sertifika satın alsa veya harici bir CA 'ya bağımlı bir CA çalıştırırken).
- Sertifika yetkilisinin geliştirilmesi ve doğrulanması.
- Denetim kayıtlarını gözden geçirme.
- CA 'yı desteklemeye veya fiziksel ve bulut tesislerinin yönetilmesine yardımcı olan, ancak CA işlemlerini gerçekleştirmek için doğrudan güvenilir olmayan personel, yetkili rolde olacak şekilde tanımlanır. Yetkilendirilmiş roldeki kişilerin gerçekleştirmesine izin verilen görev kümesi de açıklanmalıdır.

### <a name="memberships-of-trusted-and-authorized-roles-must-be-reviewed-annually"></a>Güvenilen ve yetkili rollerin üyelikleri yıllık olarak incelenmelidir

Güvenilen ve yetkili rollerin üyeliği, her roldeki kişilerin (el ile süreçler için) veya hizmet kimliklerinin (Otomatik süreçler için) en düşük düzeyde tutulduğundan emin olmak için en az üç ayda bir incelenmelidir.

### <a name="certificate-issuance-process-must-enforce-role-separation-between-certificate-requester-and-approver"></a>Sertifika verme işlemi, sertifika isteyenin ve onaylayan arasında rol ayrımı zorlaması gerekir

Sertifika verme işlemi, sertifika isteyenin ve sertifika onaylayan rolleri (kişiler veya otomatik sistemler) arasında rol ayrımı zorlaması gerekir. Sertifika verme, sertifika istek sahibinin sertifika alma yetkisi olduğunu doğrulayan bir sertifika onaylayan rolü tarafından yetkilendirilmelidir. Sertifika onaylayan rolünü tutan kişiler, resmi olarak yetkilendirilmiş bir kişi olmalıdır.

### <a name="privileged-role-management-must-restrict-access-and-be-reviewed-quarterly"></a>Ayrıcalıklı rol yönetimi erişimi kısıtlamalı ve üç ayda bir şekilde gözden geçirilmesi gerekir

Yöneticiler ve onaylayanlar grubunun üyeliğini yetkilendirme gibi ayrıcalıklı rollerin atanması, sınırlı bir yetkili personel kümesiyle sınırlandırılmalıdır. Herhangi bir ayrıcalıklı rol değişikliği, 24 saat içinde erişimi iptal etmelidir. Son olarak, ayrıcalıklı rol atamalarının üç aylık esasına göre incelenmesi ve gereksiz veya süre dolmayan atamaların kaldırılması gerekir.

### <a name="privileged-roles-should-use-two-factor-authentication"></a>Ayrıcalıklı roller Iki öğeli kimlik doğrulaması kullanmalıdır

Çok faktörlü kimlik doğrulaması (Iki öğeli kimlik doğrulaması, MFA veya TFA), onaylayanlar ve yöneticilerin hizmete yönelik etkileşimli oturum açmaları için kullanılmalıdır.

## <a name="certificate-service-operation-guidelines"></a>Sertifika hizmeti işlem yönergeleri

### <a name="operational-contacts"></a>İşletimsel kişiler

Sertifika Hizmeti, ayrıntılı işletimsel olay yanıtı kişilerini içeren dosya üzerinde güncel bir güvenlik yanıt planına sahip olmalıdır.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Tüm sistemler sürekli olarak izlenmeli ve en son güvenlik güncelleştirmeleri/yama uyumluluğuyla güncelleştirilir.

> [!IMPORTANT]
> OPC Kasası hizmetinin GitHub deposu, güvenlik düzeltme ekleriyle sürekli olarak güncelleştirilir. GitHub 'daki güncelleştirmelerin izlenmesi ve güncelleştirmelerin düzenli aralıklarla hizmete uygulanması gerekir.

### <a name="security-monitoring"></a>Güvenliği izleme

Bir merkezi izleme çözümüne abone olarak (örneğin, Azure Güvenlik Merkezi, O365 izleme çözümü) ve güvenlik olaylarının ' a aktarılmasını sağlamak üzere uygun güvenlik izlemeye abone olun veya uygulamayı uygun şekilde yapılandırın. çözüm izleniyor.

> [!IMPORTANT]
> Varsayılan olarak OPC kasa hizmeti, [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/devops) bir izleme çözümü olarak dağıtılır. [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) gibi bir güvenlik çözümü eklemek kesinlikle önerilir.

### <a name="assess-security-of-open-source-software-components"></a>Açık kaynaklı yazılım bileşenlerinin güvenliğini değerlendirin

Bir ürün veya hizmet içinde kullanılan tüm açık kaynaklı bileşenler, orta veya daha büyük güvenlik açıklarına karşı ücretsiz olmalıdır.

> [!IMPORTANT]
> OPC kasası hizmeti 'nin GitHub deposu, güvenlik açıkları için sürekli tümleştirme derlemeleri sırasında tüm bileşenleri tarıyor. GitHub 'daki güncelleştirmelerin izlenmesi ve güncelleştirmelerin düzenli aralıklarla hizmete uygulanması gerekir.

### <a name="maintain-an-inventory"></a>Envanter Bakımı

Bir varlık envanterinin tüm üretim Konakları (kalıcı sanal makineler dahil), cihazlar, tüm iç IP adresi aralıkları, VIP 'ler ve genel DNS etki alanı adları için saklanması gerekir. Bu envanter, 30 gün içinde bir sistem, aygıt IP adresi, VIP veya genel DNS etki alanı ekleme veya kaldırma ile birlikte güncelleştirilmeleri gerekir.

#### <a name="inventory-of-the-default-azure-opc-vault-microservice-production-deployment"></a>Varsayılan Azure OPC Kasası mikro hizmeti üretim dağıtımının envanteri: 

**Azure**'da:
- **App Service planı**: Hizmet ana bilgisayarları için App Service planı. Varsayılan S1.
- Mikro hizmet için **App Service** : OPC kasa hizmeti ana bilgisayarı.
- Örnek uygulama için **App Service** : OPC Kasası örnek uygulama konağı.
- **Keykasası standardı**: Web Hizmetleri için gizli dizileri ve Cosmos DB anahtarlarını depolamak için.
- **Keykasası Premium**: Veren CA anahtarlarını, imzalama hizmeti için, kasa yapılandırması ve uygulama özel anahtarlarının depolanması için barındırmak üzere.
- **Cosmos DB**: Uygulama ve sertifika istekleri için veritabanı. 
- **Application Insights**: (isteğe bağlı) Web hizmeti ve uygulaması için izleme çözümü.
- **Azuread uygulama kaydı**: Örnek uygulama, hizmet ve Edge modülü için bir kayıt.

Bulut hizmetleri için, hizmeti dağıtmak için kullanılan tüm ana bilgisayar adları, kaynak grupları, kaynak adları, abonelik kimliği, kimlik Tenantıd 'nin belgelenmelidir. 

**IoT Edge** veya yerel **IoT Edge sunucusunda**:
- **OPC kasası IoT Edge modülü**: Fabrika ağı OPC UA küresel bulma sunucusunu desteklemek için. 

IoT Edge cihazlarda, ana bilgisayar adları ve IP adresleri belgelenmelidir. 

### <a name="document-the-certification-authorities-cas"></a>Sertifika yetkililerini (CA 'Lar) belgeleme

CA hiyerarşisi belgelerinin, hizmet tarafından yönetilmeseler bile, tüm ilgili alt CA 'Lar, üst CA 'Lar ve kök CA 'lar dahil olmak üzere tüm çalıştırılan CA 'Ları içermesi gerekir. Tüm süre dolmayan CA sertifikalarının kapsamlı bir kümesi resmi belgeler yerine sağlanmayabilir.

> [!IMPORTANT]
> OPC Kasası örnek uygulaması, belgeleri için hizmette kullanılan ve üretilen tüm sertifikaların indirilmesini destekler.

### <a name="document-the-issued-certificates-by-all-certification-authorities-cas"></a>Tüm sertifika yetkililerine (CAs) verilen sertifikaları belgeleme

Son 12 ay içinde verilen tüm sertifikaların kapsamlı bir kümesi belgeler için sağlanmalıdır.

> [!IMPORTANT]
> OPC Kasası örnek uygulaması, belgeleri için hizmette kullanılan ve üretilen tüm sertifikaların indirilmesini destekler.

### <a name="document-the-sop-for-securely-deleting-cryptographic-keys"></a>Şifreleme anahtarlarını güvenli bir şekilde silmek için SOP 'yi belgeleyin

Anahtar silme yalnızca bir CA 'nın ömrü boyunca nadiren gerçekleşiyorsa, hiçbir kullanıcının hiç Anahtar Kasası sertifikası silme hakkı yok ve bir veren CA sertifikasını silmeye yönelik hiçbir API 'nin neden olmadığı. Sertifika yetkilisi şifreleme anahtarlarını güvenli bir şekilde silmenin el ile standart işletim yordamı yalnızca Azure portal ve Keykasasındaki sertifika grubunu silerek doğrudan anahtar kasasına erişerek kullanılabilir. Anında silme [Keykasası geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) devre dışı bırakılmalıdır.

## <a name="certificates"></a>Sertifikalar

### <a name="certificates-must-comply-with-minimum-certificate-profile"></a>Sertifikalar en düşük sertifika profiliyle uyumlu olmalıdır

OPC kasası hizmeti, abonelere son varlık sertifikaları veren bir çevrimiçi sertifika yetkilidir (CAs).
OPC Kasası mikro hizmeti bu yönergeleri varsayılan uygulamada izler.

- Tüm sertifikaların aşağıda belirtilen X. 509.440 alanlarını içermesi gerekir:
  - Sürüm alanının içeriği v3 olmalıdır. 
  - SerialNumber alanının içeriği, FIPS (Federal bilgi Işleme standartları) 140 onaylı rastgele sayı üreticisi tarafından alınan en az 8 baytlık entropi içermelidir.<br>
    > [!IMPORTANT]
    > OPC Kasası seri numarası varsayılan olarak 20 bayttır ve işletim sistemi şifreleme rastgele sayı oluşturucusından elde edilir. Rastgele sayı üreticisi, Windows cihazlarında FIPS 140 onaylı, ancak Linux türleri üzerinde onaylanmamış bir sayıdır. Bu olgu, temeldeki teknoloji OpenSSL 'nin FIPS 140 onaylı olmadığı Linux VM 'lerini veya Linux Docker kapsayıcılarını kullanan bir hizmet dağıtımı seçerken dikkate alınması gerekir.
  - Issueruniqueıd ve Subjectuniqueıd alanları mevcut olmamalıdır.
  - Son varlık sertifikalarının IETF RFC 5280 ' ye uygun olarak temel kısıtlamalar Uzantısı ile tanımlanması gerekir.
  - Veren CA sertifikası için pathLenConstraint alanının 0 olarak ayarlanması gerekir. 
  - Genişletilmiş anahtar kullanımı uzantısı mevcut olmalı ve en düşük genişletilmiş anahtar kullanımı nesne tanımlayıcıları (OID) kümesini içermelidir. AnyExtendedKeyUsage OID (2.5.29.37.0) belirtilmemelidir. 
  - CRL dağıtım noktası (CDP) uzantısının veren CA sertifikasında mevcut olması gerekir.<br>
    > [!IMPORTANT]
    > CRL dağıtım noktası (CDP) uzantısı OPC kasa CA sertifikalarında bulunur, ancak OPC UA cihazları CRL 'Leri dağıtmak için özel yöntemler kullanır.
  - Yetkili bilgi erişimi uzantısının abone sertifikalarında mevcut olması gerekir.<br>
    > [!IMPORTANT]
    > Yetkili bilgi erişimi uzantısı OPC Kasası abone sertifikalarında bulunur, ancak OPC UA cihazları veren CA bilgilerini dağıtmak için özel yöntemler kullanır.
- Onaylanan Asimetrik algoritmalar, anahtar uzunlukları, karma işlevler ve doldurma modları kullanılmalıdır.
  - **RSA** ve **SHA-2** desteklenen tek algoritmalardır (*).
  - RSA, şifreleme, anahtar değişim ve imza için kullanılabilir.
  - RSA şifrelemesi yalnızca OAEP, RSA-KED veya RSA-PSS doldurma modlarını kullanmalıdır.
  - Anahtar uzunlukları > = 2048 bit gereklidir.
  - SHA-2 karma algoritmaları ailesini (SHA256, SHA384 ve SHA512 olur) kullanın.
  - Genellikle ömrü > = 20 yıl olan RSA kök CA anahtarları 4096 bit veya daha büyük olmalıdır.
  - RSA veren CA anahtarları en az 2048 bit olmalıdır; CA sertifikasının sona erme tarihi 2030 sonrasında, CA anahtarı 4096 bit veya daha büyük olmalıdır.
- Sertifika ömrü
  - Kök CA sertifikaları: Kök CA 'Lar için en fazla sertifika geçerlilik süresi 25 yıldan fazla olmamalıdır.
  - Alt CA veya çevrimiçi sertifika veren CA sertifikaları: Çevrimiçi olan ve yalnızca abone sertifikaları veren CA 'Lar için en fazla sertifika geçerlilik süresi altı yıldan fazla olmamalıdır. Bu CA 'Lar için, ilgili özel imza anahtarı, yeni sertifika vermek için üç yıldan daha uzun bir süre kullanılmamalıdır.<br>
    > [!IMPORTANT]
    > Dış kök CA 'sı olmayan varsayılan OPC Kasası mikro hizmetinde oluşturulduğu haliyle veren sertifikası, ilgili gereksinimleri ve ömürleri içeren bir çevrimiçi alt CA gibi davranır. Varsayılan ömür, anahtar uzunluğu > = 2048 olan beş yıl olarak ayarlanır.
  - Tüm asimetrik anahtarların en fazla beş yıllık ömrü ve önerilen bir yıllık ömür olması gerekir.<br>
    > [!IMPORTANT]
    > Varsayılan olarak OPC kasası ile verilen uygulama sertifikalarının yaşam sürelerinin ömrü iki yıldır ve her yıl değiştirilmelidir. 
  - Bir sertifika her yenilendiğinde, yeni bir anahtarla yenilenir.
- Uygulama örneği sertifikalarındaki OPC UA 'a özgü uzantılar
  - SubjectAltName uzantısı, FQDN, IPv4 ve IPv6 adreslerini de içerebilen uygulama URI 'sini ve ana bilgisayar adlarını içerir.
  - KeyUsage, digitalSignature, reddetme, keyEncipherment ve Dataencment öğelerini içerir.
  - ExtendedKeyUsage, serverAuth ve/veya clientAuth bilgilerini içerir.
  - Authoritykeyıdentifier, imzalanmış sertifikalarda belirtilir.

### <a name="certificate-authority-ca-keys-and-certificates-must-meet-minimum-requirements"></a>Sertifika yetkilisi (CA) anahtarları ve sertifikalarının en düşük gereksinimleri karşılaması gerekir

- **Özel anahtarlar**: **RSA** anahtarları en az 2048 bit olmalıdır; CA sertifikasının sona erme tarihi 2030 sonrasında, CA anahtarı 4096 bit veya daha büyük olmalıdır.
- **Ömür**: Çevrimiçi olan ve yalnızca abone sertifikaları veren CA 'Lar için en fazla sertifika geçerlilik süresi altı yıldan fazla olmamalıdır. Bu CA 'Lar için, ilgili özel imza anahtarı, yeni sertifika vermek için üç yıldan daha uzun bir süre kullanılmamalıdır.

### <a name="ca-keys-are-protected-using-hardware-security-modules-hsm"></a>CA anahtarları, donanım güvenlik modülleri (HSM) kullanılarak korunur

- Opckasa, Azure Keykasası Premium kullanır ve anahtarlar FIPS 140-2 düzey 2 donanım güvenlik modülleri (HSM) tarafından korunur. 

Key Vault kullandığı şifreleme modülleri, HSM veya yazılımın FIPS (Federal bilgi Işleme standartları) tarafından doğrulanıp onaylanmayacağı.<br>
HSM korumalı olarak oluşturulan veya içeri aktarılan anahtarlar, FIPS 140-2 düzey 2 olarak doğrulanan bir HSM içinde işlenir.<br>
Yazılım korumalı olarak oluşturulan veya içeri aktarılan anahtarlar FIPS 140-2 düzey 1 ' e doğrulanan şifreleme modülleri içinde işlenir.

## <a name="operational-practices"></a>İşletimsel uygulamalar

### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-enrollment"></a>Sertifika kaydı için standart işletimsel PKI uygulamalarını belgeleyin ve bakımını yapın

CA 'Ların sertifika verme yöntemi (SOP 'ler belirli) için aşağıdakiler dahil olmak üzere standart işletimsel yordamları belgeleyin ve saklayın: 
- Abonenin nasıl tanımlandığı ve kimliği doğrulandı 
- Sertifika isteği nasıl işlenir ve onaylanır (varsa, sertifika yenileme ve yeniden anahtarlama isteklerinin nasıl işleneceğini de içerir) 
- Verilen sertifikalar abonelere nasıl dağıtılır 

OPC Kasası mikro hizmeti SOP, [genel bakış](overview-opc-vault-architecture.md) ve belgeleri [yönetme](howto-opc-vault-manage.md) konularında açıklanmaktadır. Uygulamalar OPC Birleşik mimari belirtimi Bölüm 12 ' ye uyar: Bulma ve küresel hizmetler.


### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-revocation"></a>Sertifika iptali için standart işletimsel PKI uygulamalarını belgeleyin ve bakımını yapın

Sertifika iptal işlemi, [genel bakış](overview-opc-vault-architecture.md) ve belgeleri [yönetme](howto-opc-vault-manage.md) konularında açıklanmaktadır.
    
### <a name="document-certification-authority-key-generation-ceremony"></a>Belge sertifika yetkilisi anahtar oluşturma seremonisi 

OPC Kasası mikro hizmetindeki sertifikayı veren CA anahtarı oluşturma işlemi, Azure Keykasasındaki güvenli depolama nedeniyle basitleştirilmiştir ve belgelerde [nasıl yönetileceği](howto-opc-vault-manage.md) açıklanmaktadır.

Ancak, bir dış kök sertifika yetkilisi kullanıldığında, bir sertifika yetkilisi (CA) anahtar oluşturma sertifikası aşağıdaki gereksinimlere uymalıdır:

CA anahtarı oluşturma sertifikası, en azından aşağıdaki öğeleri içeren belgelenmiş bir betikte gerçekleştirilmelidir: 
1. Rollerin ve katılımcı sorumlulukların tanımı
2. CA anahtar oluşturma seremonisi 'nin kullanım için onay
3. Seremoni için gereken şifreleme donanımı ve etkinleştirme malzemeleri
4. Donanım hazırlama (varlık/yapılandırma bilgileri güncelleştirmesi ve oturumu kapatma dahil)
5. İşletim sistemi yüklemesi
6. CA anahtarı oluşturma sertifikası sırasında gerçekleştirilen belirli adımlar: 
7. CA uygulaması yükleme ve yapılandırma
8. CA anahtarı oluşturma
9. CA anahtarı yedeklemesi
10. CA sertifika imzalama
9. İmzalı anahtarların hizmetin korumalı HSM 'de içeri aktarılması.
11. CA sistemi kapatması
12. Depolama için malzemelerin hazırlanması


## <a name="next-steps"></a>Sonraki adımlar

Artık OPC kasasını güvenli bir şekilde yönetmeyi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC kasası ile OPC UA cihazlarını güvenli hale getirme](howto-opc-vault-secure.md)