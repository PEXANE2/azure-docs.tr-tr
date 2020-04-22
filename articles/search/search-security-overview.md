---
title: Güvenlik ve veri gizliliği
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, SOC 2, HIPAA ve diğer sertifikalarla uyumludur. Filtre ifadelerinde kullanıcı ve grup güvenlik tanımlayıcıları aracılığıyla bağlantı ve veri şifreleme, kimlik doğrulama ve kimlik erişimi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 4db9624fbc71e48fcc10ae1d9a1d700d301248a2
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759538"
---
# <a name="security-and-data-privacy-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da güvenlik ve veri gizliliği

Kapsamlı güvenlik özellikleri ve erişim denetimleri, özel içeriğin bu şekilde kalmasını sağlamak için Azure Bilişsel Arama'da yerleşiktir. Bu makalede, Azure Bilişsel Arama'da yerleşik güvenlik özellikleri ve standartlar adedi ne kadar dır.

Azure Bilişsel Arama güvenlik mimarisi fiziksel güvenlik, şifreli aktarımlar, şifreli depolama ve platform genelinde standartlara uygunluk kapsar. Azure Bilişsel Arama, operasyonel olarak yalnızca kimlik doğrulama isteklerini kabul eder. İsteğe bağlı olarak, güvenlik filtreleri aracılığıyla içerik üzerindeki kullanıcı başına erişim denetimleri ekleyebilirsiniz. Bu makale, her katmandaki güvenliğe değinmektedir, ancak öncelikle Azure Bilişsel Arama'da verilerin ve işlemlerin nasıl güvence altına alındığına odaklanır.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Standartlara uygunluk: ISO 27001, SOC 2, HIPAA

Azure Bilişsel Arama, [Haziran 2018'de duyurulduğu](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/)üzere aşağıdaki standartlar için onaylanmaktadır:

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 Tip 2 uyumluluğu](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) Raporun tamamı için [Azure ve Azure Devlet SOC 2 Türü II Raporu'na](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports)gidin. 
+ [Sağlık Sigortası Taşınabilirlik ve Sorumluluk Yasası (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Bölüm 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS Düzey 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)

Standartlara uygunluk genel olarak kullanılabilir özellikler için geçerlidir. Önizleme özellikleri genel kullanılabilirliğe geçiş sırasında sertifikalandırılır ve katı standart gereksinimleriolan çözümlerde kullanılmamalıdır. Uyumluluk [sertifikası, Microsoft Azure uyumluluğuna](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ve [Güven Merkezi'ne](https://www.microsoft.com/en-us/trustcenter)Genel Bakış olarak belgelenmiştir. 

## <a name="encrypted-transmission-and-storage"></a>Şifreli iletim ve depolama

Şifreleme, bağlantılardan iletime ve Azure Bilişsel Arama'da depolanan dizine bağlı verilere kadar tüm dizin oluşturma ardışık alanı boyunca uzanır.

| Güvenlik katmanı | Açıklama |
|----------------|-------------|
| Aktarım sırasında şifreleme <br>(HTTPS/SSL/TLS) | Azure Bilişsel Arama, HTTPS bağlantı noktası 443'ü dinler. Platform genelinde, Azure hizmetlerine bağlantılar şifrelenir. <br/><br/>Tüm istemciden hizmete Azure Bilişsel Arama etkileşimleri SSL/TLS 1.2 şifrelemesini kullanır. Önceki sürümler (1.0 veya 1.1) desteklenmez.|
| Bekleme sırasında şifreleme <br>Microsoft yönetilen anahtarlar | Şifreleme, dizin oluşturma işleminde tamamen içselleştirilmiştir ve tamamlanma süresi veya dizin boyutu üzerinde ölçülebilir bir etkisi yoktur. Tam olarak şifrelenmemiş (Ocak 2018'den önce oluşturulan) bir dizin için artımlı güncelleştirmeler de dahil olmak üzere tüm dizin oluşturmada otomatik olarak oluşur.<br><br>Dahili olarak şifreleme, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak Azure Depolama Hizmeti Şifrelemesi'ne dayanır. [Azure Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)<br><br> Şifreleme, Microsoft tarafından dahili olarak yönetilen ve evrensel olarak uygulanan sertifikalar ve şifreleme anahtarlarıyla Azure Bilişsel Arama'ya dahilidir. Şifrelemeyi açamaz veya kapatamazsınız, kendi anahtarlarınızı yönetemez veya değiştiremezsiniz veya portaldaki veya programlı olarak şifreleme ayarlarını görüntüleyemezsiniz.<br><br>24 Ocak 2018'de duyurulan şifreleme, tüm bölgelerde ki ücretsiz katman da dahil olmak üzere tüm hizmet katmanları için geçerlidir. Tam şifreleme için, şifrelemenin oluşması için bu tarihten önce oluşturulan dizinlerin düşürülmesi ve yeniden oluşturulması gerekir. Aksi takdirde, yalnızca 24 Ocak'tan sonra eklenen yeni veriler şifrelenir.|
| Bekleme sırasında şifreleme <br>Müşteri tarafından yönetilen anahtarlar | Müşteri tarafından yönetilen anahtarlarla şifreleme artık ocak 2019'da veya sonrasında oluşturulan arama hizmetleri için genel olarak kullanılabilir. Ücretsiz (paylaşılan) hizmetlerde desteklenmez.<br><br>Azure Bilişsel Arama dizinleri ve eşanlamlı haritalar artık Azure Key Vault'ta müşteri tarafından yönetilen anahtarlarla birlikte şifrelenebilir. Daha fazla bilgi için Azure [Bilişsel Arama'da şifreleme anahtarlarını yönet'e](search-security-manage-encryption-keys.md)bakın.<br><br>Bu özellik, varsayılan şifrelemenin yerine istirahatte değil, buna ek olarak uygulanır.<br><br>Bu özelliği etkinleştirmek dizin boyutunu artırır ve sorgu performansını düşürür. Bugüne kadarki gözlemlere dayanarak, gerçek performans dizin tanımına ve sorgu türlerine bağlı olarak değişse de, sorgu sürelerinde %30-60'lık bir artış görmeyi bekleyebilirsiniz. Bu performans etkisi nedeniyle, bu özelliği yalnızca gerçekten gerektiren dizinlerde etkinleştirmenizi öneririz.

## <a name="azure-wide-user-access-controls"></a>Azure genelinde kullanıcı erişim denetimleri

Azure genelinde çeşitli güvenlik mekanizmaları kullanılabilir ve böylece oluşturduğunuz Azure Bilişsel Arama kaynakları tarafından otomatik olarak kullanılabilir.

+ [Silinmesini önlemek için abonelik veya kaynak düzeyinde kilitler](../azure-resource-manager/management/lock-resources.md)
+ [Bilgiye ve yönetim işlemlerine erişimi denetlemek için rol tabanlı Erişim Denetimi (RBAC)](../role-based-access-control/overview.md)

Tüm Azure hizmetleri, tüm hizmetlerde sürekli erişim düzeylerini ayarlamak için rol tabanlı erişim denetimlerini (RBAC) destekler. Örneğin, yönetici anahtarı gibi hassas verileri görüntülemek, Sahibi ve Katılımcı rolleri ile sınırlıdır. Ancak, görüntüleme hizmeti durumu herhangi bir rolün üyeleri tarafından kullanılabilir. RBAC, Sahibi, Katılımcısı ve Okuyucu rollerini sağlar. Varsayılan olarak, tüm hizmet yöneticileri Sahibi rolünün üyeleridir.

<a name="service-access-and-authentication"></a>

## <a name="endpoint-access"></a>Uç nokta erişimi

### <a name="public-access"></a>Genel erişim

Azure Bilişsel Arama, Azure platformunun güvenlik önlemlerini devralır ve kendi anahtar tabanlı kimlik doğrulamasını sağlar. Api-tuşu rasgele oluşturulan sayılar ve harflerden oluşan bir dizedir. Anahtar türü (yönetici veya sorgu) erişim düzeyini belirler. Geçerli bir anahtarın gönderilmesi, isteğin güvenilir bir varlıktan kaynaklandığının kanıtı olarak kabul edilir. 

Arama hizmetinize iki tür anahtar tarafından etkinleştirilen iki erişim düzeyi vardır:

* Yönetici erişimi (hizmete karşı herhangi bir okuma-yazma işlemi için geçerlidir)
* Sorgu erişimi (bir dizinin belge koleksiyonuna karşı sorgular gibi salt okunur işlemler için geçerlidir)

*Hizmet* sağlandığında yönetici anahtarları oluşturulur. Onları düz tutmak için *birincil* ve *ikincil* olarak belirlenmiş iki yönetici anahtarları vardır, ama aslında değiştirilebilir. Her hizmetin iki yönetici anahtarı vardır, böylece hizmetinize erişimikaybetmeden bir tanesini devredebilirsiniz. Yönetici [anahtarını](search-security-api-keys.md#regenerate-admin-keys) Azure güvenlik en iyi uygulamalarına göre düzenli aralıklarla yeniden oluşturabilirsiniz, ancak toplam yönetici anahtar sayısına ekleyemezsiniz. Arama hizmeti başına en fazla iki yönetici anahtarı vardır.

*Sorgu anahtarları* gerektiği gibi oluşturulur ve sorgu veren istemci uygulamaları için tasarlanmıştır. En fazla 50 sorgu anahtarı oluşturabilirsiniz. Uygulama kodunda, belirli bir dizinin belge koleksiyonuna salt okunur erişime izin vermek için arama URL'sini ve sorgu api anahtarını belirtirsiniz. Birlikte, bitiş noktası, salt okunur erişim için api anahtarı ve hedef dizini istemci uygulamanızdan bağlantının kapsamını ve erişim düzeyini tanımlar.

Her istek zorunlu anahtar, bir işlem ve bir nesneden oluştuğu her istek te kimlik doğrulaması gereklidir. Birlikte zincirlendiğinde, iki izin düzeyi (tam veya salt okunur) ve bağlam (örneğin, bir dizindeki sorgu işlemi) hizmet işlemlerinde tam spektrum güvenliği sağlamak için yeterlidir. Anahtarlar hakkında daha fazla bilgi için [api tuşlarını oluştur ve yönet'](search-security-api-keys.md)e bakın.

### <a name="restricted-access"></a>Kısıtlı erişim

Bir kamu hizmetiniz varsa ve hizmetin kullanımını kısıtlamak istediğinizde, Management REST API sürümünde IP kısıtlama kuralını kullanabilirsiniz: 2020-03-13, [IpRule](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service#IpRule). IpRule, arama hizmetinize erişim izni vermek istediğiniz IP adreslerini tek tek veya bir aralıkta tanımlayarak hizmetinize erişimi kısıtlamanızı sağlar. 

### <a name="private-access"></a>Özel erişim

Azure Bilişsel Arama için [Özel Bitiş Noktaları,](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) sanal ağdaki bir istemcinin [Özel Bağlantı](https://docs.microsoft.com/azure/private-link/private-link-overview)üzerinden arama dizinindeki verilere güvenli bir şekilde erişmesine olanak tanır. Özel bitiş noktası, arama hizmetiniz için sanal ağ adres alanından bir IP adresi kullanır. İstemci ve arama hizmeti arasındaki ağ trafiği sanal ağ üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden geçerek genel internetten maruz kalmayı ortadan kaldırır.

[Azure Sanal Ağı (VNet),](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) şirket içi ağınızın yanı sıra Internet ile kaynaklar arasında güvenli iletişim sağlar. 

## <a name="index-access"></a>Dizin erişimi

Azure Bilişsel Arama'da, tek bir dizin securable nesne değildir. Bunun yerine, bir dizin erişimi hizmet katmanı (okuma veya yazma erişimi) ve bir işlem bağlamında belirlenir.

Son kullanıcı erişimi için, sorgu isteklerini, herhangi bir isteğin salt okunur olmasını sağlayan bir sorgu anahtarı kullanarak bağlanmak üzere yapılandırabilir ve uygulamanız tarafından kullanılan belirli dizini ekleyebilirsiniz. Sorgu isteğinde, dizinleri birleştirme veya birden çok dizine aynı anda erişme kavramı yoktur, bu nedenle tüm istekler tanım olarak tek bir dizini hedeflemez. Bu nedenle, sorgu isteğinin kendisi (anahtar artı tek bir hedef dizini) yapısı güvenlik sınırını tanımlar.

Yönetici ve geliştiricinin dizinlere erişimi farklılaşmaz: hizmet tarafından yönetilen nesneleri oluşturmak, silmek ve güncelleştirmek için her ikisinin de yazma erişimine ihtiyacı vardır. Hizmetinizin yönetici anahtarı olan herkes aynı hizmetteki herhangi bir dizini okuyabilir, değiştirebilir veya silebilir. Dizinlerin yanlışlıkla veya kötü amaçlı silinmesine karşı koruma için, kod varlıkları için şirket içi kaynak denetiminiz istenmeyen bir dizin silme veya değiştirmenin geri döndürülmesinin çözümüdür. Azure Bilişsel Arama kullanılabilirliği sağlamak için küme içinde başarısız olmuştur, ancak dizin oluşturmak veya yüklemek için kullanılan özel kodunuzu depolamaz veya yürütmez.

Dizin düzeyinde güvenlik sınırları gerektiren çoklu hizmet çözümleri için, bu tür çözümler genellikle müşterilerin dizin yalıtımını işlemek için kullandıkları bir orta katman içerir. Çok kiracılı kullanım örneği hakkında daha fazla bilgi için, [çok kiracılı SaaS uygulamaları ve Azure Bilişsel Arama için Tasarım desenleri](search-modeling-multitenant-saas-applications.md)bölümüne bakın.

## <a name="authentication"></a>Kimlik Doğrulaması

### <a name="admin-access"></a>Yönetici erişimi

[Rol tabanlı erişim (RBAC),](https://docs.microsoft.com/azure/role-based-access-control/overview) hizmet ve içeriği üzerindeki denetimlere erişip erişemeyeceğiniz belirlenir. Azure Bilişsel Arama hizmetinde Sahip veya Katılımcıysanız, hizmetteki nesneleri oluşturmak, güncelleştirmek veya silmek için portalı veya PowerShell **Az.Search** modülünü kullanabilirsiniz. [Ayrıca Azure Bilişsel Arama Yönetimi REST API'yi](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)de kullanabilirsiniz.

### <a name="user-access"></a>Kullanıcı erişimi

Varsayılan olarak, bir dizin kullanıcı erişimi sorgu isteği üzerinde erişim anahtarı tarafından belirlenir. Çoğu geliştirici istemci tarafındaki arama istekleri için [*sorgu anahtarları*](search-security-api-keys.md) oluşturur ve atar. Sorgu anahtarı, dizindeki tüm içeriğe okuma erişimi sağlar.

İçerik üzerinde parçalı, kullanıcı başına denetime ihtiyaç duyarsanız, sorgularınıza güvenlik filtreleri oluşturabilir ve belirli bir güvenlik kimliğiyle ilişkili belgeleri döndürebilirsiniz. Önceden tanımlanmış roller ve rol atamaları yerine, kimlik tabanlı erişim denetimi, kimlikleri temel alan belgelerin ve içeriğin arama sonuçlarını kesen bir *filtre* olarak uygulanır. Aşağıdaki tabloda, yetkisiz içeriğin arama sonuçlarını kırpmak için iki yaklaşım açıklanmaktadır.

| Yaklaşım | Açıklama |
|----------|-------------|
|[Kimlik filtrelerine dayalı güvenlik kırpma](search-security-trimming-for-azure-search.md)  | Kullanıcı kimliği erişim denetimini uygulamak için temel iş akışını belgeler. Bir dizin için güvenlik tanımlayıcıları eklemeyi kapsar ve ardından yasaklı içeriğin sonuçlarını kırpmak için bu alana karşı filtreleme açıklar. |
|[Azure Etkin Dizin kimliklerine dayalı güvenlik kırpma](search-security-trimming-for-azure-search-with-aad.md)  | Bu makale, Azure bulut platformundaki [ücretsiz hizmetlerden](https://azure.microsoft.com/free/) biri olan Azure Active Directory'den (AAD) kimlik alma adımları sağlayarak önceki makalede genişletir. |

## <a name="table-permissioned-operations"></a>Tablo: İzin verilen işlemler

Aşağıdaki tablo, Azure Bilişsel Arama'da izin verilen işlemleri ve belirli bir işlemiçin hangi anahtarın kilidini açtığını özetler.

| İşlem | İzinler |
|-----------|-------------------------|
| Hizmet oluşturma | Azure abonelik sahibi|
| Bir hizmeti ölçeklendirin | Yönetici anahtarı, RBAC Sahibi veya Kaynakta Katkıda Bulunan  |
| Bir hizmeti silme | Yönetici anahtarı, RBAC Sahibi veya Kaynakta Katkıda Bulunan |
| Hizmetteki nesneleri oluşturma, değiştirme, silme: <br>Dizinler ve bileşen parçaları (çözümleyici tanımları, puanlama profilleri, CORS seçenekleri dahil), dizinleyiciler, veri kaynakları, eşanlamlılar, önericiler. | Yönetici anahtarı, RBAC Sahibi veya Kaynakta Katkıda Bulunan  |
| Dizin sorgusu | Yönetici veya sorgu anahtarı (RBAC geçerli değildir) |
| Dönen istatistikler, sayımlar ve nesne listeleri gibi sistem bilgilerini sorgulayın. | Yönetici anahtarı, kaynak üzerinde RBAC (Sahibi, Katkıda Bulunan, Okuyucu) |
| Yönetici anahtarlarını yönetme | Kaynakta yönetici anahtarı, RBAC Sahibi veya Katkıda Bulunan. |
| Sorgu tuşlarını yönetme |  Kaynakta yönetici anahtarı, RBAC Sahibi veya Katkıda Bulunan.  |

## <a name="physical-security"></a>Fiziksel güvenlik

Microsoft veri merkezleri sektör lideri fiziksel güvenlik sağlar ve kapsamlı bir standart ve düzenleme portföyüyle uyumludır. Daha fazla bilgi edinmek için [Global veri merkezleri](https://www.microsoft.com/cloud-platform/global-datacenters) sayfasına gidin veya veri merkezi güvenliği yle ilgili kısa bir video izleyin.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Ayrıca bkz.

+ [.NET'e başlayın (dizin oluşturmak için yönetici anahtarını kullanarak gösterir)](search-create-index-dotnet.md)
+ [REST'e başlayın (dizin oluşturmak için yönetici anahtarını kullanarak gösterilir)](search-create-index-rest-api.md)
+ [Azure Bilişsel Arama filtrelerini kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Bilişsel Arama filtrelerini kullanarak Etkin Dizin kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Bilişsel Arama'daki Filtreler](search-filters.md)