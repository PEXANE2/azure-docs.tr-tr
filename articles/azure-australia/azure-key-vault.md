---
title: Azure Avustralya 'da Azure Key Vault
description: Avustralya kamu ilkesi, yönetmelikler ve yasalların belirli gereksinimlerini karşılamak üzere Avustralya bölgelerindeki anahtar yönetimi için Azure Key Vault yapılandırma ve kullanma konusunda rehberlik.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 263765c777f994134befc52f0c63c7f18e590b39
ms.sourcegitcommit: 15f7b641a67f3d6cf4fb4b4c11eaee18cf335923
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602119"
---
# <a name="azure-key-vault-in-azure-australia"></a>Azure Avustralya 'da Azure Key Vault

Şifreleme anahtarları ve şifreleme anahtar yaşam döngüsünün yönetimi güvenli depolama, şifreleme sistemleri içindeki kritik öğelerdir.  Bu özelliği Azure 'da sağlayan hizmet Azure Key Vault. Key Vault, ıRAP güvenliğine erişildi ve korunan için ACSC sertifikalıdır.  Bu makalede, Avustralya sinyallerinin (ASD) [bilgi güvenliği El Ile denetim](https://acsc.gov.au/infosec/ism/) (ISM) ile uyumlu olması için Key Vault kullanırken dikkat etmeniz gereken önemli noktalar özetlenmektedir.

Azure Key Vault, şifreleme anahtarlarını ve gizli dizileri korumalarını sağlayan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, Key Vault, yalnızca yetkili kullanıcılara ve uygulamalara izin vermek için, anahtar kasalarına güvenli erişim sağlar. Key Vault tarafından yönetilen ve denetlenen üç ana yapıt vardır:

- anahtar
- gizli dizi
- sertifika

Bu makale Key Vault kullanarak anahtarların yönetimine odaklanacaktır.

![Azure Key Vault](media/azure-key-vault-overview.png)

*Diyagram 1 – Azure Key Vault*

## <a name="key-design-considerations"></a>Önemli tasarım konuları

### <a name="deployment-options"></a>Dağıtım seçenekleri

Azure Anahtar Kasası oluşturmak için iki seçenek vardır. Her iki seçenek de, donanım güvenlik modülleri (HSM) Thales nShield ailesini kullanır, Federal bilgi Işleme standartları (FIPS) onaylanır ve korunan ortamlarda anahtarları depolamak için onaylanır. Seçenekler şunlardır:

- **Yazılım korumalı kasaları:** FIPS 140-2 düzey 1 kimliği doğrulanmadı. Bir HSM 'de depolanan anahtarlar. Şifreleme ve şifre çözme işlemleri Azure 'daki işlem kaynaklarında gerçekleştirilir.
- **HSM korumalı kasa:** FIPS 140-2 düzey 2 doğrulanmadı. Bir HSM 'de depolanan anahtarlar. Şifreleme ve şifre çözme işlemleri HSM üzerinde gerçekleştirilir.

Key Vault, Rivest-Shamir-Adliman (RSA) ve eliptik eğri şifreleme (ECC) anahtarlarını destekler. Varsayılan değer RSA 2048-bit anahtardır, ancak RSA 3072 bit, RSA 4096 bit ve ECC anahtarları için gelişmiş bir seçenek vardır.  Tüm anahtarlar ıSM denetimlerini karşılar, ancak eliptik eğri tuşları tercih edilir.

### <a name="resource-operations"></a>Kaynak işlemleri

Azure Key Vault ilgili birkaç kişi vardır:

- **Key Vault Yöneticisi:** Kasanın yaşam döngüsünü yönetir
- **Anahtar Yöneticisi:** Kasadaki anahtarların yaşam döngüsünü yönetir
- **Geliştirici/operatör:** Anahtarları kasadan uygulamalar ve hizmetlerle tümleştirin
- **Ekleme** Anahtar kullanımını ve erişimi izleme
- **Uygulamaları** Bilgileri güvenli hale getirmek için anahtarları kullanın

Azure Key Vault iki ayrı arabirim ile güvenli hale getirilir:

- **Yönetim düzlemi:** Bu düzlem, kasayı yönetme ve RBAC tarafından güvenli hale getirilmesi ile ilgilidir.
- **Veri düzlemi:** Bu düzlem, kasadaki yapıtların yönetilmesi ve bunlara erişme ile ilgilidir.  Key Vault erişim ilkesi kullanılarak güvenli hale getirilir.

Bir arayan (bir kullanıcı veya uygulamadan önce), her iki düzlem tarafından anahtar kasasına erişebilmek için, ıSM için gereken şekilde, uygun kimlik doğrulaması ve yetkilendirici gerekir.

Azure RBAC, anahtar kasalarının yönetimini denetlemek için Key Vault, [katkıda bulunan Key Vault](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#key-vault-contributor)bir yerleşik role sahiptir. Anahtar kasalarınızı yönetmeye yönelik daha ayrıntılı rollere hizalı özel rollerin oluşturulması önerilir.

>[!WARNING]
>Anahtarlara erişim Key Vault erişim ilkesi aracılığıyla etkinleştirildiğinde, Kullanıcı veya uygulama anahtar kasasındaki tüm anahtarlara erişime sahip olur (örneğin, bir kullanıcının ' Sil ' erişimi varsa, tüm anahtarları silebilir).  Bu nedenle, güvenlik etki alanları/sınırları ile hizalamak için birden çok Anahtar Kasası dağıtılmalıdır.

### <a name="networking"></a>Ağ

Veri düzlemine erişimi denetlemek için Key Vault güvenlik duvarlarını ve sanal ağları yapılandırabilirsiniz.  Tüm diğer ağlardaki kullanıcılara veya uygulamalara erişimi reddetirken, belirtilen ağlardaki kullanıcılara veya uygulamalara erişim izni verebilirsiniz. [Güvenilen hizmetler](https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints#trusted-services) , "Güvenilen hizmetlere izin ver" etkinse bu denetimin bir istisnadır.  Sanal ağ denetimi yönetim düzlemi için uygulanmıyor.

Anahtar kasalarına erişim, kullanıcıların veya uygulamaların anahtarlara erişmesi gereken en düşük ağ kümesiyle açıkça sınırlandırılmalıdır.

### <a name="bring-your-own-key-byok"></a>Kendi Anahtarını Getir (BYOK)

Key Vault, BYOK 'ı destekler.  BYOK, kullanıcıların anahtarları mevcut anahtar altyapılarından içe aktarmasını sağlar.  Thales, bir dış HSM 'den (örneğin, çevrimdışı bir iş istasyonu ile oluşturulan anahtarlar) Key Vault olarak güvenli aktarım ve içeri aktarma desteği sunan bir [Avustralya araç](https://www.microsoft.com/download/details.aspx?id=45345) seti sağlar.

### <a name="key-vault-auditing-and-logging"></a>Denetim ve günlüğe kaydetme Key Vault

ACSC, Azure iş yüklerinde gerçek zamanlı izleme ve raporlama yapmak için uygun Azure hizmetlerini kullanmak üzere, bir hizmet veren varlıkları gerektirir.

Anahtar değerlerinde **_"auditevent"_** tanılama ayarı etkinleştirilerek günlüğe kaydetme etkinleştirilir.  Denetim olayları, belirtilen depolama hesabında günlüğe kaydedilir.  **_"Retentionındays"_** dönemi, veri bekletme ilkesine göre ayarlanmalıdır.  Hem yönetim düzlemi hem de veri düzlemi üzerinde [işlemler](https://docs.microsoft.com/azure/key-vault/key-vault-logging#interpret) denetlenir ve günlüğe kaydedilir. [Azure izleyici 'deki Azure Key Vault çözümü](https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault) Key Vault auditevent günlüklerini gözden geçirmek için kullanılabilir.  Diğer birçok Azure hizmeti, Key Vault AuditEvents işlemek ve dağıtmak için kullanılabilir.

### <a name="key-rotation"></a>Anahtar döndürme

Key Vault anahtarların depolanması, uygulamaların davranışını etkilemeden anahtarların güncelleştirilmesini sağlayan uygulamalar dışındaki anahtarların bakımını yapmak için tek bir nokta sağlamıştır. Anahtarları Azure Key Vault içinde depolamak, anahtar döndürmeyi desteklemek için çeşitli stratejiler sunar:

- El İle
- API 'Ler aracılığıyla program aracılığıyla
- Otomasyon betikleri (örneğin, PowerShell ve Azure Otomasyonu kullanma)

Bu seçenekler, anahtarların tehlikede olduğu endişeler varsa, uyumluluk gereksinimlerini karşılamak için anahtarların düzenli aralıklarla veya geçici olarak döndürülmesine olanak sağlar.

#### <a name="key-rotation-strategies"></a>Anahtar döndürme stratejileri

Anahtar Kasası 'nda depolanan anahtarlar için uygun bir anahtar döndürme stratejisi geliştirmek önemlidir.  Yanlış anahtarın kullanılması, bilgilerin hatalı şekilde şifresinin oluşmasına neden olur ve anahtarların kaybolması, bilgilere erişimin tamamen kaybedilmesine neden olabilir.  Farklı senaryolar için anahtar döndürme stratejilerinin örnekleri şunlardır:

- **Infaçık veriler:** geçici bilgiler 2 taraf arasında iletilir.  Bir anahtar döndürüldüğünde her iki taraf da anahtar kasasından güncelleştirilmiş anahtarları almak için zaman uyumlu bir mekanizmaya sahip olmalıdır.
- **Rest olarak veri:** Bir taraf şifrelenmiş verileri depolar ve gelecekte kullanmak üzere şifresini çözer.  Bir anahtar döndürüldüğünde, verilerin eski anahtarla çözülmesi ve ardından yeni, döndürülmüş anahtarla şifrelenmesi gerekir.  Anahtar şifreleme anahtarlarını kullanarak şifre çözme/şifreleme işleminin etkilerini en aza indirmek için yaklaşımlar vardır (bkz. örnek).  Microsoft, Azure depolama için anahtar dönüşüyle ilgili işlemin büyük çoğunluğunu yönetir (bkz....)
- **Erişim anahtarları:** birçok Azure hizmeti, Key Vault depolanabilecek erişim anahtarlarına sahiptir (örneğin, cosmosdb).  Azure Hizmetleri, birincil ve ikincil erişim anahtarlarına sahiptir.  Her iki anahtarın de aynı anda döndürülmediğinden emin olmanız önemlidir.  Bu nedenle, bir anahtarın bir süre sonra döndürülmeli ve anahtar işlem doğrulandıktan sonra ikinci anahtar döndürülecektir.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

ISM, Iş sürekliliği ile ilgili birkaç denetime sahiptir.
Azure Key Vault, bölgede ve ikincil, [eşleştirilmiş bölge](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)içinde çoğaltılan içerikle birden çok artıklık katmanına sahiptir.

Anahtar Kasası yük devretme durumunda olduğunda, salt okunurdur ve birincil hizmetin geri yüklendiği okuma yazma moduna geri döner.

ISM, yedeklemeyle ilgili birkaç denetime sahiptir.  Kasaların ve anahtarlarının uygun yedekleme/geri yükleme planlarını geliştirmek ve yürütmek önemlidir.

## <a name="key-lifecycle"></a>Anahtar yaşam döngüsü

### <a name="key-operations"></a>Anahtar işlemleri

Key Vault bir anahtarda aşağıdaki işlemleri destekler:

- **oluşturma** İstemcinin Key Vault bir anahtar oluşturmasına izin verir. Anahtarın değeri Key Vault tarafından oluşturulur ve depolanır ve istemciye serbest bırakılır. Asimetrik anahtarlar Key Vault oluşturulabilir.
- **aktarmaya** Bir istemcinin mevcut bir anahtarı Key Vault içeri aktarmasını sağlar. Asimetrik anahtarlar, bir JWK yapısı içinde çok sayıda farklı paketleme yöntemi kullanılarak Key Vault içeri aktarılabilir.
- **Update** Daha önce Key Vault içinde depolanan bir anahtarla ilişkili meta verileri (anahtar öznitelikleri) değiştirmek için yeterli izinlere sahip bir istemciye izin verir.
- **silmeli** Key Vault bir anahtarı silmek için yeterli izinlere sahip bir istemciye izin verir.
- **Listele** Bir istemcinin belirli bir Key Vault tüm anahtarları listeetmesine izin verir.
- **liste sürümleri:** Bir istemcinin belirli bir Key Vault verilen bir anahtarın tüm sürümlerini listeetmesine izin verir.
- **Al:** Bir istemcinin bir Key Vault verilen bir anahtarın ortak parçalarını almasına izin verir.
- **yedeklenmiş** Bir anahtarı korumalı bir biçimde dışa aktarır.
- **yükleyebilmek** Daha önce yedeklenen bir anahtarı içeri aktarır.

Kullanıcılara, hizmet sorumlularına veya uygulamalara, anahtar işlemleri yürütmesine olanak tanımak için Key Vault Access Control girdileri kullanılarak verilebilecek ilgili bir izin kümesi vardır.

Key Vault, silinen kasaların ve anahtarların kurtarılmasına izin veren bir geçici silme özelliğine sahiptir. Varsayılan olarak, **_"geçici silme"_** etkin değildir, ancak etkinleştirildikten sonra nesneler, silinmek üzere görünmekte olan 90 gün boyunca (Bekletme dönemi) tutulur.  " **_Korumayı temizle"_** seçeneği devre dışıysa, ek bir **_"Temizle"_** izni, anahtarların kalıcı olarak silinmesine izin verir.

Mevcut bir anahtarı oluşturma veya içeri aktarma anahtarın yeni bir sürümünü oluşturur.

### <a name="cryptographic-operations"></a>Şifreleme işlemleri

Key Vault anahtarları kullanarak şifreleme işlemlerini de destekler:

- **imzala ve Doğrula:** bu işlem bir "imza karması" veya "karmayı Doğrula" dır. Key Vault, imza oluşturmanın bir parçası olarak içeriğin karma oluşturmayı desteklemez.
- **anahtar şifreleme/sarmalama:** bu işlem, başka bir anahtarı korumak için kullanılır.
- **şifreleme ve şifre çözme:** depolanan anahtar, tek bir veri bloğunu şifrelemek veya şifresini çözmek için kullanılır

Kullanıcılara, hizmet sorumlularına veya uygulamalara, şifreleme işlemlerinin yürütülmesine olanak tanımak için Key Vault erişim denetimi girdileri kullanılarak verilebilecek ilgili bir izin kümesi vardır.

Anahtarın etkinleştirilip etkinleştirilmeyeceğini ve şifreleme işlemlerinin kullanılıp kullanılamayacağını denetlemek için ayarlanmış üç anahtar özniteliği vardır:

- **etkinletir**
- **NBF:** belirtilen tarihten önce etkin değil
- **Exp:** sona erme tarihi

## <a name="storage-and-keys"></a>Depolama ve anahtarlar

Müşteri tarafından yönetilen anahtarlar daha fazla esneklik sağlar ve denetlenmek üzere anahtarların değerlendirilmesi ve yönetimine olanak tanır. Ayrıca, verileri korumak için kullanılan şifreleme anahtarlarının denetlenmesini da sağlar.
Key Vault depolanan depolama ve anahtarların üç yönü vardır:

- Yönetilen depolama hesabı anahtarlarını Key Vault
- Bekleyen veriler için Azure Depolama Hizmeti Şifrelemesi (SSE)
- Yönetilen diskler ve Azure disk şifrelemesi

Key Vault Azure depolama hesabı anahtar yönetimi, depolama hesabı anahtarlarının eşitlenmesini ve yeniden oluşturulmasını (döndürmeyi) destekleyen bir Key Vault anahtar hizmeti uzantısıdır.  [Azure Active Directory Ile Azure depolama tümleştirmesi](https://docs.microsoft.com/azure/storage/common/storage-auth-aad) (Önizleme), serbest bir güvenlik ve kullanım kolaylığı sağladığından, sunulduktan sonra önerilir.
SSE, bekleyen verileri şifrelemeyi yönetmek için iki anahtar kullanır:

- Anahtar şifreleme anahtarları (KEK)
- Veri şifreleme anahtarları (DEK)

Microsoft, DEKs 'Leri yönetirken, SSE Key Vault ' de depolanabilecek, müşteri tarafından yönetilen KEKs kullanma seçeneği vardır. Bu, uygun uyumluluk ilkelerine göre Azure Key Vault anahtarların dönüşmesine izin vermez. Anahtarlar döndürüldüğünde, Azure Storage bu depolama hesabı için hesap şifreleme anahtarını yeniden şifreler. Bu, tüm verilerin yeniden şifrelenme sonucu vermez ve başka bir eylem gerekmez.

SSE yönetilen diskler için kullanılır ancak müşteri tarafından yönetilen anahtarlar desteklenmez.  Yönetilen disklerin şifrelenmesi, Key Vault 'de müşteri tarafından yönetilen KEK anahtarlarla Azure disk şifrelemesi kullanılarak yapılabilir.

## <a name="next-steps"></a>Sonraki Adımlar

[Kimlik Federasyonu](identity-federation.md) hakkındaki makaleyi gözden geçirin

[Başvuru kitaplığındaki](reference-library.md) ek Azure Key Vault belgelerini ve öğreticileri gözden geçirin
