---
title: Güvenliğe genel bakış
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, SOC 2, HIPAA ve diğer sertifikalarla uyumludur. Filtre ifadelerinde Kullanıcı ve grup güvenlik tanımlayıcılarıyla bağlantı ve veri şifreleme, kimlik doğrulama ve kimlik erişimi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/01/2020
ms.custom: references_regions
ms.openlocfilehash: fb265f8a8ab34972dac8529d267e41edaf0acb4c
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87829297"
---
# <a name="security-in-azure-cognitive-search---overview"></a>Azure Bilişsel Arama güvenlik-genel bakış

Bu makalede, Azure Bilişsel Arama içerik ve işlemleri koruyabilecek temel güvenlik özellikleri açıklanmaktadır.

+ Depolama katmanında, dizinler, eş anlamlı eşlemeler ve Dizin oluşturucular, veri kaynakları ve becerileri tanımları dahil olmak üzere diske kaydedilen tüm hizmet tarafından yönetilen içerikler için, Rest 'ten şifreleme yerleşik olarak bulunur. Azure Bilişsel Arama, dizinli içeriğin ek şifrelenmesi için müşteri tarafından yönetilen anahtarların (CMK) eklenmesini de destekler. 1 2020 Ağustos 'Tan sonra oluşturulan hizmetler için, CMK şifrelemesi, dizini oluşturulmuş içeriğin tam çift şifrelemesi için geçici disklerdeki verilere genişletilir.

+ Gelen güvenlik, arama hizmeti uç noktasını, güvenlik seviyelerine karşı koruma sağlar: istekteki API anahtarlarından, güvenlik duvarında gelen kurallara, genel İnternet 'ten hizmetinizi tamamen koruyan özel uç noktalara.

+ Giden güvenlik, dış kaynaklardan içerik çeken Dizin oluşturucular için geçerlidir. Giden istekler için, Azure depolama, Azure SQL, Cosmos DB veya diğer Azure veri kaynaklarından verilere erişirken bir güvenilen hizmet arama yapmak üzere yönetilen bir kimlik ayarlayın. Yönetilen kimlik, bağlantıda kimlik bilgilerinin veya erişim anahtarlarının yerini alır. Giden güvenlik Bu makalede ele alınmıyor. Bu yetenek hakkında daha fazla bilgi için bkz. [yönetilen kimlik kullanarak bir veri kaynağına bağlanma](search-howto-managed-identities-data-sources.md).

Güvenlik mimarisine ve her özellik kategorisine genel bir bakış için bu hızla ilerleyebileceğiniz videoyu izleyin.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Azure-Cognitive-Search-Whats-new-in-security/player]

<a name="encryption"></a>

## <a name="encrypted-transmissions-and-storage"></a>Şifrelenmiş aktarımlar ve depolama

Azure Bilişsel Arama 'de, şifreleme bağlantılarla ve iletimlerle başlar ve diskte depolanan içeriğe genişletir. Genel internet üzerindeki arama hizmetleri için Azure Bilişsel Arama, HTTPS bağlantı noktası 443 ' i dinler. Tüm istemciden hizmete bağlantılar TLS 1,2 şifrelemesini kullanır. Önceki sürümler (1,0 veya 1,1) desteklenmez.

Arama hizmeti tarafından dahili olarak işlenen veriler için aşağıdaki tabloda [veri şifreleme modelleri](../security/fundamentals/encryption-atrest.md#data-encryption-models)açıklanmaktadır. Bilgi deposu, artımlı zenginleştirme ve Dizin Oluşturucu tabanlı dizin oluşturma, diğer Azure hizmetlerinde veri yapılarına okuma veya yazma gibi bazı özellikler. Bu hizmetlerin Azure Bilişsel Arama ayrı ayrı şifreleme desteği düzeyleri vardır.

| Model | Belirlenmesine&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Gereklilik&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Kısıtlamalar | Şunlara uygulanır |
|------------------|-------|-------------|--------------|------------|
| sunucu tarafı şifreleme | Microsoft tarafından yönetilen anahtarlar | Hiçbiri (yerleşik) | Hiçbiri, tüm bölgelerde, tüm bölgelerde bulunan ve 24 2018 Ocak 'tan sonra oluşturulan içerikler için kullanılabilir. | İçerik (dizinler ve eş anlamlı haritalar) ve tanımlar (Dizin oluşturucular, veri kaynakları, becerileri) |
| sunucu tarafı şifreleme | Müşteri tarafından yönetilen anahtarlar | Azure Key Vault | Faturalandırılabilir katmanda, tüm bölgelerde, Ocak 2019 ' den sonra oluşturulan içerikler için kullanılabilir. | Veri disklerinde içerik (dizinler ve eş anlamlılar haritaları) |
| sunucu tarafı çift şifreleme | Müşteri tarafından yönetilen anahtarlar | Azure Key Vault | 1 2020 Ağustos 'Tan sonra Arama hizmetlerinde, seçili bölgelerde faturalanabilir katmanlarda kullanılabilir. | Veri disklerinde ve geçici disklerde içerik (dizinler ve eş anlamlılar haritaları) |

### <a name="service-managed-keys"></a>Hizmet tarafından yönetilen anahtarlar

Hizmet tarafından yönetilen şifreleme, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak [Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md)temel alan Microsoft iç işlemidir. Tam olarak şifrelenmeyen dizinlerin Artımlı güncelleştirmeleri de dahil olmak üzere tüm dizin oluşturma işleminde otomatik olarak gerçekleşir (2018 Ocak 'tan önce oluşturulmuştur).

### <a name="customer-managed-keys-cmk"></a>Müşteri tarafından yönetilen anahtarlar (CMK)

Müşteri tarafından yönetilen anahtarlar, farklı bir bölgede, ancak Azure Bilişsel Arama olarak aynı abonelik altında olabilen ek bir faturalanabilir hizmet Azure Key Vault gerektirir. CMK şifrelemesini etkinleştirmek, dizin boyutunu artırır ve sorgu performansını düşürür. Tarih gözlemlerini temel alarak sorgu süreleriyle %30 oranında %60 oranında bir artış görmeniz beklenir, ancak gerçek performans, Dizin tanımına ve sorgu türlerine göre değişir. Bu performans etkisi nedeniyle, bu özelliği yalnızca gerçekten gereken dizinlerde etkinleştirmenizi öneririz. Daha fazla bilgi için bkz. [Azure bilişsel arama müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](search-security-manage-encryption-keys.md).

<a name="double-encryption"></a>

### <a name="double-encryption"></a>Çift şifreleme 

Azure Bilişsel Arama 'de, Çift şifreleme CMK 'nin bir uzantısıdır. İki katlı şifreleme (CMK tarafından bir kez ve hizmet tarafından yönetilen anahtarlar tarafından), kapsam içi ve bir veri diskine yazılan uzun süreli depolamayı ve geçici disklere yazılan kısa vadeli depolamayı içeren kapsamlı bir şifreleme olduğu anlaşıldı. CMK ile 1 2020 Ağustos 'Tan önce ve sonrasında CMK arasındaki fark ve Azure Bilişsel Arama 'de CMK 'nin Çift şifreleme özelliği ne kadar bir kez yapılır, geçici disklerde bekleyen verilerin ek şifrelemesi.

Çift şifreleme Şu anda bu bölgelerde oluşturulan yeni hizmetlerde 1 Ağustos 'Tan sonra kullanılabilir:

+ Batı ABD 2
+ Doğu ABD
+ Orta Güney ABD
+ US Gov Virginia
+ US Gov Arizona

<a name="service-access-and-authentication"></a>

## <a name="inbound-security-and-endpoint-protection"></a>Gelen güvenlik ve uç nokta koruması

Gelen güvenlik özellikleri, güvenlik ve karmaşıklık düzeylerini artırarak arama hizmeti uç noktasını korur. İlk olarak, tüm istekler kimliği doğrulanmış erişim için bir API anahtarı gerektirir. İkinci olarak, belirli IP adreslerine erişimi sınırlayan güvenlik duvarı kuralları belirleyebilirsiniz. Gelişmiş koruma için, üçüncü bir seçenek Azure özel bağlantısının, hizmet uç noktanızı tüm internet trafiğinden korunmasını sağlar.

### <a name="public-access-using-api-keys"></a>API anahtarları kullanarak genel erişim

Varsayılan olarak, bir arama hizmetine yönetim için anahtar tabanlı kimlik doğrulaması kullanılarak veya arama hizmeti uç noktasına sorgu erişimi kullanılarak genel bulut üzerinden erişilir. Bir API anahtarı rastgele oluşturulan rakamlardan ve harflerden oluşan bir dizedir. Anahtar türü (yönetici veya sorgu), erişim düzeyini belirler. Geçerli bir anahtarın gönderilmesi, isteğin güvenilir bir varlıktan kaynaklandığı kanıtları kabul edilir. 

Arama hizmetinize aşağıdaki API anahtarları tarafından etkinleştirilen iki erişim düzeyi vardır:

+ Yönetici anahtarı (arama hizmetinde [oluşturma-okuma-güncelleştirme-silme](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) işlemleri için okuma-yazma erişimine izin verir)

+ Sorgu anahtarı (bir dizinin belgeler koleksiyonuna salt okuma erişimi sağlar)

Hizmet sağlandığında *yönetici anahtarları* oluşturulur. *Birincil* ve *İkincil* olarak tasarlanan iki yönetici anahtarı vardır, ancak bunlar aslında değiştirilebilir. Her hizmette iki yönetici anahtarı bulunur, bu sayede hizmetinize erişimi kaybetmeksizin bir veya daha fazla dağıtım yapabilirsiniz. Azure Güvenlik en iyi uygulamaları başına [yönetici anahtarını](search-security-api-keys.md#regenerate-admin-keys) düzenli aralıklarla yeniden oluşturabilirsiniz, ancak toplam yönetici anahtar sayısına ekleyemezsiniz. Arama hizmeti başına en fazla iki yönetici anahtarı vardır.

*Sorgu anahtarları* gerekli olarak oluşturulur ve sorgular veren istemci uygulamalar için tasarlanmıştır. En çok 50 sorgu anahtarı oluşturabilirsiniz. Uygulama kodu ' nda, belirli bir dizinin belgeler koleksiyonuna salt okuma erişimi sağlamak için arama URL 'sini ve sorgu API 'si anahtarını belirtirsiniz. Bir arada, uç nokta, salt okuma erişimi için bir API anahtarı ve bir hedef dizin, istemci uygulamanızdan bağlantının kapsamını ve erişim düzeyini tanımlar.

Her istekte bir zorunlu anahtar, bir işlem ve bir nesneden oluşan her istekte kimlik doğrulaması gerekir. Birlikte zincirleme yaparken, iki izin düzeyi (tam veya salt okuma) ve bağlam (örneğin, dizin üzerinde bir sorgu işlemi), hizmet işlemlerinde tam SPI güvenliği sağlamak için yeterlidir. Anahtarlar hakkında daha fazla bilgi için bkz. [API anahtarları oluşturma ve yönetme](search-security-api-keys.md).

### <a name="ip-restricted-access"></a>IP kısıtlı erişim

Arama hizmetinize erişimi daha fazla denetlemek için, belirli IP adreslerine veya bir IP adresi aralığına erişime izin veren gelen güvenlik duvarı kuralları oluşturabilirsiniz. Tüm istemci bağlantıları izin verilen bir IP adresi üzerinden yapılmalıdır veya bağlantı reddedildi.

[Gelen erişimi yapılandırmak](service-configure-firewall.md)için portalı kullanabilirsiniz. 

Alternatif olarak, yönetim REST API 'Lerini de kullanabilirsiniz. API sürüm 2020-03-13, [ıprule](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service#IpRule) parametresi ile, tek tek veya bir aralıkta, arama hizmetinize erişim vermek istediğiniz IP adreslerini tanımlayarak hizmetinize erişimi kısıtlamanıza olanak sağlar. 

### <a name="private-endpoint-no-internet-traffic"></a>Özel uç nokta (Internet trafiği yok)

Azure Bilişsel Arama için [özel bir uç nokta](../private-link/private-endpoint-overview.md) , bir [sanal ağ](../virtual-network/virtual-networks-overview.md) üzerindeki istemcinin bir [özel bağlantı](../private-link/private-link-overview.md)üzerinden bir arama dizinindeki verilere güvenli bir şekilde erişmesini sağlar. 

Özel uç nokta, arama hizmetinize bağlantı için sanal ağ adres alanından bir IP adresi kullanır. İstemci ile arama hizmeti arasındaki ağ trafiği, sanal ağın ve Microsoft omurga ağındaki özel bir bağlantının üzerinde geçiş yaparken, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. VNET, kaynaklar arasında, şirket içi ağınızdan ve Internet 'Ten güvenli iletişim sağlar. 

Bu çözüm en güvenli hale geldiğinden, ek hizmetlerin kullanılması, ek bir maliyettir. bu nedenle, ' de kullanmadan önce avantajların net bir şekilde anlaşıldığından emin olun. veya maliyetler hakkında daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/private-link/). Bu bileşenlerin birlikte nasıl çalıştığı hakkında daha fazla bilgi için bu makalenin en üstündeki videoyu izleyin. Özel uç nokta seçeneğinin kapsamı, video 5:48 ' de başlar. Uç noktanın nasıl ayarlanacağı hakkında yönergeler için bkz. [Azure bilişsel arama Için özel uç nokta oluşturma](service-create-private-endpoint.md).

## <a name="index-access"></a>Dizin erişimi

Azure Bilişsel Arama, tek bir dizin, güvenli kılınabilir bir nesne değildir. Bunun yerine, bir dizin erişimi, hizmet katmanında (hizmete okuma veya yazma erişimi), bir işlemin içeriğiyle birlikte belirlenir.

Son Kullanıcı erişimi için, bir sorgu anahtarı kullanarak bağlanmak üzere sorgu istekleri yapılandırabilirsiniz, bu da tüm istekleri salt okunurdur ve uygulamanız tarafından kullanılan belirli bir dizini içerir. Bir sorgu isteğinde, Dizin bağlama veya birden çok dizine aynı anda erişme kavramı yoktur, bu nedenle tüm istekler tanım ile tek bir dizin hedefleyin. Bu nedenle, sorgu isteğinin kendisi (bir anahtar ve tek bir hedef dizin), güvenlik sınırını tanımlar.

Dizinlere yönetici ve geliştirici erişimi farklılaştırılabilir: her ikisi de hizmet tarafından yönetilen nesneleri oluşturmak, silmek ve güncelleştirmek için yazma erişimine ihtiyaç duyar. Hizmetinize yönetici anahtarına sahip olan herkes aynı hizmette bulunan herhangi bir dizini okuyabilir, değiştirebilir veya silebilir. Dizinlerin yanlışlıkla veya kötü amaçlı olarak silinmesine karşı koruma için, kod varlıkları için şirket içi kaynak denetiminiz, istenmeyen bir dizin silme veya değiştirme işlemi için bir çözüm oluşturur. Azure Bilişsel Arama, kullanılabilirliği sağlamak için küme içinde yük devretmeye sahiptir, ancak dizinleri oluşturmak veya yüklemek için kullanılan özel kodunuzu depolamaz veya yürütmez.

Dizin düzeyinde güvenlik sınırları gerektiren çok kiracılı çözümler için, bu tür çözümler genellikle müşterilerin Dizin yalıtımını işlemek için kullanacağı bir orta katman içerir. Çok kiracılı kullanım durumu hakkında daha fazla bilgi için bkz. [çok kiracılı SaaS uygulamaları ve Azure bilişsel arama Için tasarım desenleri](search-modeling-multitenant-saas-applications.md).

## <a name="user-access"></a>Kullanıcı erişimi

Bir Kullanıcı bir dizine nasıl eriştiğinde ve diğer nesneler istekteki API anahtarı türüne göre belirlenir. Çoğu geliştirici, istemci tarafı arama istekleri için [*sorgu anahtarları*](search-security-api-keys.md) oluşturur ve atar. Sorgu anahtarı, dizin içinde aranabilir içeriğe salt okuma erişimi verir.

Arama sonuçları üzerinde ayrıntılı, Kullanıcı başına denetim istiyorsanız, sorgularda güvenlik filtreleri oluşturabilir, bu, belirli bir güvenlik kimliğiyle ilişkili belgeleri döndürür. Kimlik tabanlı erişim denetimi, önceden tanımlanmış roller ve rol atamaları yerine, belgelerin ve içeriğin arama sonuçlarını kimliklere göre kırpan bir *filtre* olarak uygulanır. Aşağıdaki tabloda yetkisiz içeriğin arama sonuçlarını kırpma için iki yaklaşım açıklanmaktadır.

| Yaklaşım | Açıklama |
|----------|-------------|
|[Kimlik filtrelerine göre güvenlik kırpması](search-security-trimming-for-azure-search.md)  | Kullanıcı kimliği erişim denetimi uygulamak için temel iş akışını belgeler. Bir dizine güvenlik tanımlayıcıları eklenmesini ve sonra yasaklanmış içeriğin sonuçlarını kırpmak için bu alana karşı filtrelemeyi açıklar. |
|[Azure Active Directory kimliklerine göre güvenlik kırpması](search-security-trimming-for-azure-search-with-aad.md)  | Bu makale, Azure bulut platformunda [ücretsiz hizmetlerden](https://azure.microsoft.com/free/) biri olan Azure ACTIVE DIRECTORY (AAD) ' den kimlik alma adımlarını sağlayan önceki makaleye genişletilir. |

## <a name="administrative-rights"></a>Yönetim hakları

[Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md) , Azure kaynaklarının sağlanması için [Azure Resource Manager](../azure-resource-manager/management/overview.md) yerleşik bir yetkilendirme sistemidir. Azure Bilişsel Arama 'de, hizmeti oluşturmak veya silmek, API anahtarlarını yönetmek ve hizmeti ölçeklendirmek için Kaynak Yöneticisi kullanılır. Bu nedenle, Azure rol atamaları, [Portal](search-manage.md), [POWERSHELL](search-manage-powershell.md)veya [Yönetim REST API 'lerini](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)kullanıp kullanmadıklarından bağımsız olarak kimlerin bu görevleri gerçekleştirebileceğini tespit eder.

Buna karşılık, bir dizin oluşturma veya silme yeteneği gibi, hizmette barındırılan içerik üzerinde yönetici hakları, [önceki bölümde](#index-access)AÇıKLANDıĞı gibi API anahtarları üzerinden basıp basmadığını değerlendirerek.

> [!TIP]
> Azure genelinde mekanizmalar kullanarak, yönetici haklarına sahip kullanıcılar tarafından, arama hizmetinizin yanlışlıkla veya izinsiz olarak silinmesini engellemek için bir aboneliği veya kaynağı kilitleyebilir. Daha fazla bilgi için bkz. [beklenmeyen silme işlemini engellemek için kaynakları kilitleme](../azure-resource-manager/management/lock-resources.md).

## <a name="certifications-and-compliance"></a>Sertifikalamaları ve uyumluluk

Azure Bilişsel Arama, genel bulut ve Azure Kamu için birden çok küresel, bölgesel ve sektöre özgü standartlar için sertifikalandırilmiştir. Tüm liste için, resmi denetim raporları sayfasından [ **Microsoft Azure uyumluluk teklifleri** teknik](https://azure.microsoft.com/resources/microsoft-azure-compliance-offerings/) incelemesini indirin.

Uyumluluk için Azure [ilkesi](../governance/policy/overview.md) 'Ni kullanarak [Azure Güvenlik kıyaslaması](../security/benchmarks/introduction.md)için yüksek güvenlikli en iyi yöntemleri uygulayabilirsiniz. Azure Güvenlik kıyaslaması, hizmet ve veri tehditlerini azaltmak için gerçekleştirmeniz gereken önemli eylemlerle eşlenen güvenlik denetimlerine yönelik güvenlik önerileri koleksiyonudur. [Ağ güvenliği](../security/benchmarks/security-control-network-security.md), [günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md)dahil olmak üzere 11 güvenlik denetimi ve birkaç ad vermek için [veri koruması](../security/benchmarks/security-control-data-protection.md) vardır.

Azure Ilkesi, Azure 'da yerleşik olarak bulunan ve Azure Güvenlik kıyaslaması dahil olmak üzere birden çok standart için uyumluluğu yönetmenize yardımcı olan bir özelliktir. İyi bilinen kıyaslamalar için Azure Ilkesi, hem ölçütü hem de uyumsuz bir yanıt sağlayan yerleşik tanımları, uyumsuz olmayan bir yanıt sağlar. 

Azure Bilişsel Arama için, şu anda bir yerleşik tanım vardır. Bu, tanılama günlüğe kaydetme içindir. Bu yerleşik ile, tanılama günlüğü eksik olan herhangi bir arama hizmetini tanımlayan bir ilke atayabilir ve sonra onu açabilir. Daha fazla bilgi için bkz. [Azure Ilke mevzuatı uyumluluk denetimleri bilişsel arama](security-controls-policy.md).

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure güvenliğinin temelleri](../security/fundamentals/index.yml)
+ [Azure güvenliği](https://azure.microsoft.com/overview/security)
+ [Azure Güvenlik Merkezi](../security-center/index.yml)