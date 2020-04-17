---
title: Microsoft Azure Veri Şifreleme-at-Rest | Microsoft Dokümanlar
description: Bu makalede, microsoft Azure veri şifrelemesi dinlenme sırasında genel bir bakış, genel özellikler ve genel hususlar.
services: security
documentationcenter: na
author: msmbaldwin
manager: barbkess
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2020
ms.author: mbaldwin
ms.openlocfilehash: 1e08e758fbba911d3391794f5bab31aaf6a5fc73
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81454688"
---
# <a name="azure-data-encryption-at-rest"></a>Azure Veri Şifreleme-at-Rest

Microsoft Azure, şirketinizin güvenlik ve uyumluluk gereksinimlerine göre verileri korumak için araçlar içerir. Bu makale şu konularda duruluyor:

- Microsoft Azure'da veriler istirahatte nasıl korunur?
- Veri koruma uygulamasında yer alan çeşitli bileşenleri tartışır,
- Farklı anahtar yönetimi koruma yaklaşımlarının artılarını ve eksilerini gözden geçirir.

Rest'te şifreleme yaygın bir güvenlik gereksinimidir. Azure'da kuruluşlar, özel bir anahtar yönetimi çözümü riski veya maliyeti olmadan verileri istirahatte şifreleyebilir. Kuruluşların, Azure'un Rest'te Şifreleme'yi tamamen yönetmesine izin verme seçeneğine sahiptir. Ayrıca, kuruluşların şifreleme veya şifreleme anahtarlarını yakından yönetmek için çeşitli seçenekleri de olabilir.

## <a name="what-is-encryption-at-rest"></a>Şifreleme istirahatnedir?

Rest'te şifreleme, kalıcı olduğunda verilerin kodlanması (şifrelemesi) dir. Azure'daki Şifreleme tasarımları, basit bir kavramsal modele göre büyük miktarda veriyi hızlı bir şekilde şifrelemek ve şifresini çözmek için simetrik şifreleme kullanır:

- Depolamaya yazıldığı gibi verileri şifrelemek için simetrik şifreleme anahtarı kullanılır.
- Aynı şifreleme anahtarı, bellekte kullanıma hazır olduğu için bu verilerin şifresini çözmek için kullanılır.
- Veriler bölümlenebilir ve her bölüm için farklı tuşlar kullanılabilir.
- Anahtarlar, kimlik tabanlı erişim denetimi ve denetim ilkeleriyle güvenli bir konumda depolanmalıdır. Veri şifreleme anahtarları, erişimi daha da sınırlamak için Azure Key Vault'ta anahtar şifreleme anahtarıyla sık sık şifrelenir.

Uygulamada, temel yönetim ve kontrol senaryolarının yanı sıra ölçek ve kullanılabilirlik güvenceleri ek yapılar gerektirir. Microsoft Azure Şifreleme, Rest kavramları ve bileşenleri aşağıda açıklanmıştır.

## <a name="the-purpose-of-encryption-at-rest"></a>Istirahatte şifrelemenin amacı

Istirahatte şifreleme, depolanan veriler için veri koruması sağlar (istirahatte). Istirahat sırasında ki verilere yönelik saldırılar, verilerin depolandığı donanıma fiziksel erişim elde etme ve ardından içerdiği verileri tehlikeye atma girişimlerini içerir. Böyle bir saldırıda, bir sunucunun sabit diski bakım sırasında yanlış işlenmiş olabilir ve bu da saldırganın sabit sürücüyü kaldırmasına izin verir. Daha sonra saldırgan, verilere erişmeye çalışmak için sabit sürücüyü kendi denetimi altındaki bir bilgisayara yerlebir eder.

Istirahatteki şifreleme, saldırganın diskteyken verilerin şifrelenmesini sağlayarak şifrelenmemiş verilere erişmesini önlemek için tasarlanmıştır. Bir saldırgan şifreleme anahtarları değil de şifreli verilerle bir sabit disk elde ederse, saldırgan verileri okumak için şifrelemeyi yenmelidir. Bu saldırı, sabit diskteki şifrelenmemiş verilere erişmekten çok daha karmaşık ve kaynak tüketicidir. Bu nedenle, istirahatte şifreleme şiddetle tavsiye edilir ve birçok kuruluş için yüksek öncelikli bir gerekliliktir.

Bir kuruluşun veri yönetimi ve uyumluluk çabalarına olan ihtiyacı da, istirahatte şifreleme gerektirebilir. HIPAA, PCI ve FedRAMP gibi endüstri ve devlet yönetmelikleri, veri koruma ve şifreleme gereksinimleriyle ilgili özel güvenlik önlemleri ortaya koyar. Istirahatte şifreleme, bu düzenlemelerin bazılarına uygunluk için gerekli olan zorunlu bir önlemdir. Microsoft'un FIPS 140-2 doğrulamayaklaşımı hakkında daha fazla bilgi için [Federal Bilgi İşlem Standardı (FIPS) Yayını 140-2'ye](https://docs.microsoft.com/microsoft-365/compliance/offering-fips-140-2)bakın. 

Uyumluluk ve mevzuat gerekliliklerini karşılamanın yanı sıra, istirahatte şifreleme de derinlemesine savunma koruması sağlar. Microsoft Azure, hizmetler, uygulamalar ve veriler için uyumlu bir platform sağlar. Ayrıca kapsamlı tesis ve fiziksel güvenlik, veri erişim kontrolü ve denetim sağlar. Ancak, diğer güvenlik önlemlerinden birinin başarısız olması ve istirahatte şifrelemenin böyle bir güvenlik önlemi sağlaması durumunda ek "örtüşen" güvenlik önlemleri sağlamak önemlidir.

Microsoft, bulut hizmetleri genelinde dinlenme seçeneklerinde şifreleme yi ve müşterilere şifreleme anahtarları ve anahtar kullanım günlükleri üzerinde denetim sağlamaya kararlıdır. Ayrıca, Microsoft varsayılan olarak tüm müşteri verilerini uzaktan şifrelemeye yönelik çalışmaktadır.

## <a name="azure-encryption-at-rest-components"></a>Dinlenme Bileşenlerinde Azure Şifreleme

Daha önce açıklandığı gibi, şifrelemenin amacı, diskte kalıcı olan verilerin gizli bir şifreleme anahtarıyla şifrelenmiş olmasıdır. Bu amaca ulaşmak için güvenli anahtar oluşturma, depolama, erişim denetimi ve şifreleme anahtarlarının yönetimi sağlanmalıdır. Ayrıntılar farklılık gösterse de, Azure hizmetleri Rest uygulamalarının şifrelemesi aşağıdaki diyagramda gösterildiği şekilde açıklanabilir.

![Bileşenler](./media/encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

Şifreleme anahtarlarının depolama konumu ve bu anahtarlara erişim denetimi, dinlenme modelindeki bir şifrelemenin merkezinde yer almaktadır. Anahtarların son derece güvenli olması ancak belirli kullanıcılar tarafından yönetilebilir olması ve belirli hizmetler tarafından kullanılabilmesi gerekir. Azure hizmetleri için Azure Key Vault önerilen anahtar depolama çözümüdür ve hizmetler arasında ortak bir yönetim deneyimi sağlar. Anahtarlar anahtar kasalarında saklanır ve yönetilir ve anahtar kasasına erişim kullanıcılara veya hizmetlere verilebilir. Azure Key Vault, müşteri tarafından yönetilen şifreleme anahtar senaryolarında kullanılmak üzere müşteri anahtarlarının oluşturulmasını veya müşteri anahtarlarının içe aktarımı nın kullanılmasını destekler.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Key Vault'ta depolanan anahtarları, Şifreleme için Yönetme veya Bunlara Erişmek için Kullanma Şifreleme ve Şifre Çözme'yi kullanma izinleri Azure Active Directory hesaplarına verilebilir.

### <a name="key-hierarchy"></a>Anahtar Hiyerarşisi

Dinlenme uygulamasında bir şifrelemede birden fazla şifreleme anahtarı kullanılır. Azure Key Vault'ta bir şifreleme anahtarı nın saklanması, güvenli anahtar erişimi ve anahtarların merkezi yönetimini sağlar. Ancak, şifreleme anahtarlarına hizmet yerel erişim, toplu şifreleme ve şifre çözme için her veri işlemi için Key Vault ile etkileşime girmekten daha verimlidir ve daha güçlü şifreleme ve daha iyi performans sağlar. Tek bir şifreleme anahtarının kullanımını sınırlamak, anahtarın tehlikeye girme riskini ve bir anahtarın değiştirilmesi gerektiğinde yeniden şifreleme maliyetini azaltır. Dinlenme modellerindeki azure şifrelemeleri, tüm bu gereksinimleri karşılamak için aşağıdaki anahtar türlerinden oluşan anahtar hiyerarşisini kullanır:

- **Veri Şifreleme Anahtarı (DEK)** – Bir veri bölmesini veya bloğunu şifrelemek için kullanılan simetrik bir AES256 anahtarı.  Tek bir kaynağın birçok bölümü ve birçok Veri Şifreleme Anahtarı olabilir. Her veri bloğunu farklı bir anahtarla şifrelemek, kripto analizi saldırılarını daha da zorlaştırır. DEK'lere erişim, belirli bir bloğu şifreleyen ve şifresini çözen kaynak sağlayıcı veya uygulama örneği tarafından gereklidir. BIR DEK yeni bir anahtarla değiştirildiğinde, yalnızca ilişkili bloğundaki veriler yeni anahtarla yeniden şifrelenmelidir.
- **Anahtar Şifreleme Anahtarı (KEK)** – Veri Şifreleme Anahtarlarını şifrelemek için kullanılan bir şifreleme anahtarı. Key Vault'tan hiç ayrılmamış bir Anahtar Şifreleme Anahtarı nın kullanılması, veri şifreleme anahtarlarının şifrelenmesini ve kontrol edilmesine olanak tanır. KEK erişimi olan varlık DEK gerektiren varlık farklı olabilir. Bir varlık, her DEK'in erişimini belirli bir bölümle sınırlamak için DEK'e erişim sağlayabilir. KEK'in DEK'lerin şifresini çözmesi gerektiğinden, KEK, KEK'in silinmesi ile DEK'lerin etkin bir şekilde silinebileceği tek bir noktadır.

Anahtar Şifreleme Anahtarları ile şifrelenen Veri Şifreleme Anahtarları ayrı olarak depolanır ve yalnızca Anahtar Şifreleme Anahtarına erişimi olan bir varlık bu Veri Şifreleme Anahtarlarının şifresini çözebilir. Anahtar depolamanın farklı modelleri desteklenir. Her modeli daha sonra bir sonraki bölümde daha ayrıntılı olarak tartışacağız.

## <a name="data-encryption-models"></a>Veri Şifreleme Modelleri

Azure'daki çeşitli kaynak sağlayıcıların Rest'te şifrelemeyi nasıl uyguladığını anlamak için çeşitli şifreleme modelleri ve bunların artıları ve eksileri hakkında bir anlayış önemlidir. Bu tanımlar, ortak dil ve taksonomi sağlamak için Azure'daki tüm kaynak sağlayıcılar arasında paylaşılır.

Sunucu tarafı şifrelemesi için üç senaryo vardır:

- Servis Yönetilen anahtarları kullanarak sunucu tarafı şifreleme
  - Azure Kaynak Sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirir
  - Microsoft anahtarları yönetir
  - Tam bulut işlevselliği

- Azure Key Vault'ta müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme
  - Azure Kaynak Sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirir
  - Azure Key Vault ile müşteri anahtarları kontrol eder
  - Tam bulut işlevselliği

- Müşteri tarafından yönetilen donanımda müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme
  - Azure Kaynak Sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirir
  - Müşteri kontrollü donanımda anahtarları kontrol eder
  - Tam bulut işlevselliği

İstemci tarafı şifrelemeiçin aşağıdakileri göz önünde bulundurun:

- Azure hizmetleri şifresi çözülmüş verileri göremez
- Müşteriler anahtarları şirket içinde (veya diğer güvenli mağazalarda) yönetir ve saklar. Anahtarlar Azure hizmetleri için kullanılamaz
- Azaltılmış bulut işlevselliği

Azure'daki desteklenen şifreleme modelleri iki ana gruba ayrılmıştır: daha önce belirtildiği gibi "İstemci Şifreleme" ve "Sunucu Tarafı Şifrelemesi". Kullanılan dinlenme modelindeki şifrelemeden bağımsız olarak, Azure hizmetleri her zaman TLS veya HTTPS gibi güvenli bir aktarım kullanılmasını önerir. Bu nedenle, aktarımda şifreleme taşıma protokolü tarafından ele alınmalıdır ve hangi şifrelemenin istirahat modelinde kullanılacağını belirlemede önemli bir faktör olmamalıdır.

### <a name="client-encryption-model"></a>İstemci şifreleme modeli

İstemci Şifreleme modeli, hizmet veya arama uygulaması tarafından Kaynak Sağlayıcı veya Azure dışında gerçekleştirilen şifreleme anlamına gelir. Şifreleme, Azure'daki hizmet uygulaması veya müşteri veri merkezinde çalışan bir uygulama tarafından gerçekleştirilebilir. Her iki durumda da, bu şifreleme modelinden yararlanırken, Azure Kaynak Sağlayıcısı verilerin şifresini hiçbir şekilde çözme veya şifreleme anahtarlarına erişebilme özelliği olmadan şifrelenmiş bir veri blob'u alır. Bu modelde, anahtar yönetimi arama hizmeti/uygulaması tarafından yapılır ve Azure hizmeti için opaktır.

![İstemci](./media/encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Sunucu tarafı şifreleme modeli

Sunucu tarafı Şifreleme modelleri, Azure hizmeti tarafından gerçekleştirilen şifrelemeyi ifade eder. Bu modelde, Kaynak Sağlayıcı şifreleme ve şifre çözme işlemlerini gerçekleştirir. Örneğin, Azure Depolama düz metin işlemlerinde veri alabilir ve şifreleme ve şifre çözme işlemlerini dahili olarak gerçekleştirir. Kaynak Sağlayıcı, sağlanan yapılandırmaya bağlı olarak Microsoft veya müşteri tarafından yönetilen şifreleme anahtarlarını kullanabilir.

![Sunucu](./media/encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Sunucu tarafı şifreleme anahtar yönetim modelleri

Dinlenme modellerindeki sunucu tarafı şifrelemesinin her biri, anahtar yönetiminin ayırt edici özelliklerini ifade eder. Bu, şifreleme anahtarlarının nerede ve nasıl oluşturulduğunu ve depolandığı, erişim modellerini ve anahtar döndürme yordamlarını içerir.

#### <a name="server-side-encryption-using-service-managed-keys"></a>Hizmet yönetilen anahtarları kullanarak sunucu tarafı şifreleme

Birçok müşteri için temel gereksinim, verilerin istirahat olduğunda şifrelendiğinden emin olmaktır. Hizmet tarafından yönetilen Keys'i kullanarak sunucu tarafı şifrelemesi, müşterilerin şifreleme için belirli kaynağı (Depolama Hesabı, SQL DB, vb.) işaretlemesine izin vererek ve anahtar verme, döndürme ve Microsoft'a yedekleme gibi tüm temel yönetim yönlerini bırakarak bu modeli etkinleştirmektedir. Şifrelemeyi uzaktan destekleyen çoğu Azure Hizmeti genellikle bu şifreleme anahtarlarını Azure'a devre dışı bıraketme modelini destekler. Azure kaynak sağlayıcısı anahtarları oluşturur, güvenli depolama alanına yerleştirir ve gerektiğinde alır. Bu, hizmetin anahtarlara tam erişime sahip olduğu ve hizmetin kimlik bilgisi yaşam döngüsü yönetimi üzerinde tam denetime sahip olduğu anlamına gelir.

![Yönetilen](./media/encryption-atrest/azure-security-encryption-atrest-fig4.png)

Bu nedenle, hizmet tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme, müşteriye düşük ek yükü olan bir şekilde şifreleme gereksinimini hızlı bir şekilde giderer. Kullanılabilir olduğunda bir müşteri genellikle hedef abonelik ve kaynak sağlayıcısı için Azure portalını açar ve verilerin şifrelenmesini istediğinizi belirten bir kutuyu denetler. Bazı Kaynak Yöneticileri'nde hizmet yönetilen anahtarlara sahip sunucu tarafı şifrelemesi varsayılan olarak açıktır.

Microsoft tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi, hizmetin anahtarları depolamak ve yönetmek için tam erişime sahip olduğu anlamına gelmez. Bazı müşteriler daha fazla güvenlik elde ettiklerini düşündükleri için anahtarları yönetmek isteseler de, bu modeli değerlendirirken özel bir anahtar depolama çözümüyle ilişkili maliyet ve risk göz önünde bulundurulmalıdır. Çoğu durumda, bir kuruluş, şirket içi bir çözümün kaynak kısıtlamalarının veya risklerinin, dinlenme tuşlarındaki şifrelemenin bulut yönetimi riskinden daha büyük olabileceğini belirleyebilir.  Ancak, bu model, şifreleme anahtarlarının oluşturulmasını veya yaşam döngüsünü denetlemesi veya bir hizmetin şifreleme anahtarlarını yönetmesi için hizmeti yönetenlerden farklı personelin (diğer bir şekilde, hizmetin genel yönetim modelinden anahtar yönetiminin ayrılması) gereksinimi olan kuruluşlar için yeterli olmayabilir.

##### <a name="key-access"></a>Anahtar erişimi

Hizmet tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi kullanıldığında, anahtar oluşturma, depolama ve hizmet erişimi hizmet tarafından yönetilir. Genellikle, temel Azure kaynak sağlayıcıları Veri Şifreleme Anahtarlarını verilere yakın ve Anahtar Şifreleme Anahtarları güvenli bir iç depoda depolanırken hızlı bir şekilde kullanılabilir ve erişilebilir bir depoda depolar.

**Avantaj -ları**

- Basit kurulum
- Microsoft anahtar döndürme, yedekleme ve artıklık yönetir
- Müşteri, uygulamayla ilişkili maliyete veya özel bir anahtar yönetim şeması riskine sahip değildir.

**Dezavantajlar**

- Şifreleme anahtarları üzerinde müşteri denetimi yok (anahtar belirtimi, yaşam döngüsü, iptal, vb.)
- Hizmet için anahtar yönetimini genel yönetim modelinden ayırma becerisi yok

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Azure Key Vault'ta müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme

Gereksinimin verileri istirahatte şifrelemek ve şifreleme anahtarlarını denetlemek olduğu senaryolarda müşteriler Anahtar Kasası'nda müşteri tarafından yönetilen Anahtarlar'ı kullanarak sunucu tarafındaki şifrelemeyi kullanabilir. Bazı hizmetler Yalnızca Kök Anahtar Şifreleme Anahtarını Azure Anahtar Kasası'nda depolayabilir ve şifrelenmiş Veri Şifreleme Anahtarını verilere yakın bir iç konumda depolayabilir. Bu senaryoda müşteriler kendi anahtarlarını Key Vault'a (BYOK – Kendi Anahtarınızı Getir) getirebilir veya yenilerini oluşturabilir ve istenilen kaynakları şifrelemek için kullanabilir. Kaynak Sağlayıcı şifreleme ve şifre çözme işlemlerini gerçekleştirirken, tüm şifreleme işlemleri için kök anahtar olarak yapılandırılan anahtar şifreleme anahtarını kullanır.

Anahtar şifreleme anahtarlarının kaybı veri kaybı anlamına gelir. Bu nedenle, anahtarlar silinmemelidir. Anahtarlar oluşturulduğunda veya döndürüldüğünde yedeklenmelidir. [Soft-Delete](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) anahtar şifreleme anahtarlarını depolayan herhangi bir kasada etkinleştirilmelidir. Bir anahtarı silerken yerine, yanlış yapmak veya son kullanma tarihini ayarlamak için etkinleştirilir.

##### <a name="key-access"></a>Anahtar Erişimi

Azure Key Vault'ta müşteri tarafından yönetilen anahtarlara sahip sunucu tarafındaki şifreleme modeli, hizmetin gerektiğinde şifrelemek ve şifresini çözmek için anahtarlara erişmelerini içerir. Dinlenme tuşlarındaki şifreleme, bir erişim denetimi ilkesi aracılığıyla bir hizmetiçin erişilebilir hale getirilir. Bu ilke, anahtarı almak için hizmet kimliği erişimi verir. İlişkili bir abonelik adına çalışan bir Azure hizmeti, bu abonelikteki bir kimlikle yapılandırılabilir. Hizmet, Azure Etkin Dizin kimlik doğrulaması gerçekleştirebilir ve kendisini abonelik adına hareket eden hizmet olarak tanımlayan bir kimlik doğrulama belirteci alabilir. Bu belirteç daha sonra erişim verilmiş bir anahtar elde etmek için Key Vault sunulabilir.

Şifreleme anahtarlarını kullanan işlemler için hizmet kimliği aşağıdaki işlemlerden herhangi biri için erişim hakkı verebilir: şifre çözme, şifreleme, açma Anahtar, wrapKey, doğrulamak, imzalamak, almak, listele, güncellemek, oluşturmak, içe aktarma, silme, yedekleme ve geri yükleme.

Veri şifreleme veya şifre çözme de use kullanmak için bir anahtar elde etmek için kaynak yöneticisi hizmet örneği unwrapkey (şifre çözme için anahtar almak için) ve WrapKey (yeni bir anahtar oluştururken anahtar kasasına bir anahtar eklemek için) olması gerektiği gibi çalışacak hizmet kimliği dinlenme hizmet kimliği.

>[!NOTE]
>Key Vault yetkilendirmesi hakkında daha fazla ayrıntı için [Azure Anahtar Kasası belgelerinde](../../key-vault/general/secure-your-key-vault.md)anahtar kasası sayfanıza bakın.

**Avantaj -ları**

- Kullanılan anahtarlar üzerinde tam kontrol – şifreleme anahtarları müşterinin Anahtar Kasası'nda müşterinin kontrolü altında yönetilir.
- Birden çok hizmeti tek bir ana yla şifreleme olanağı
- Hizmet için genel yönetim modelinden anahtar yönetimini ayırabilir mi?
- Bölgeler arasında hizmet ve anahtar konumu tanımlayabilir

**Dezavantajlar**

- Müşteri, anahtar erişim yönetimi için tüm sorumluluğa sahiptir
- Müşteri, anahtar yaşam döngüsü yönetimi için tüm sorumluluğa sahiptir
- Ek Kurulum & yapılandırma yükü

#### <a name="server-side-encryption-using-customer-managed-keys-in-customer-controlled-hardware"></a>Müşteri kontrollü donanımda müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme

Bazı Azure hizmetleri, Ana Bilgisayar Ana Bilgisayar Anahtarınızın (HYOK) anahtar yönetim modelini etkinleştirir. Bu yönetim modu, verileri istirahatte şifrelemek ve Microsoft'un denetimi dışında özel bir depoda anahtarları yönetme gereksinimi olan senaryolarda yararlıdır. Bu modelde, hizmetin anahtarı harici bir siteden alması gerekir. Performans ve kullanılabilirlik garantileri etkilenir ve yapılandırma daha karmaşıktır. Ayrıca, hizmetşifreleme ve şifre çözme işlemleri sırasında DEK erişimi olduğundan, bu modelin genel güvenlik garantileri, anahtarların Azure Anahtar Kasası'nda müşteri tarafından yönetilmesine benzer.  Sonuç olarak, belirli temel yönetim gereksinimleri olmadığı sürece bu model çoğu kuruluş için uygun değildir. Bu sınırlamalar nedeniyle, çoğu Azure Hizmeti, müşteri kontrolündeki donanımda sunucu tarafından yönetilen anahtarları kullanarak sunucu tarafı şifrelemesini desteklemez.

##### <a name="key-access"></a>Anahtar Erişimi

Müşteri kontrolündeki donanımda servis yönetilen anahtarlar kullanılarak sunucu tarafında şifreleme kullanıldığında, anahtarlar müşteri tarafından yapılandırılan bir sistemde tutulur. Bu modeli destekleyen Azure hizmetleri, müşteri tarafından sağlanan anahtar deposuna güvenli bir bağlantı kurma nın bir aracı sağlar.

**Avantaj -ları**

- Kullanılan kök anahtarı üzerinde tam kontrol – şifreleme anahtarları sağlanan mağaza müşteri tarafından yönetilir
- Birden çok hizmeti tek bir ana yla şifreleme olanağı
- Hizmet için genel yönetim modelinden anahtar yönetimini ayırabilir mi?
- Bölgeler arasında hizmet ve anahtar konumu tanımlayabilir

**Dezavantajlar**

- Anahtar depolama, güvenlik, performans ve kullanılabilirlik için tam sorumluluk
- Anahtar erişim yönetimi için tam sorumluluk
- Anahtar yaşam döngüsü yönetimi için tam sorumluluk
- Önemli kurulum, yapılandırma ve devam eden bakım maliyetleri
- Müşteri veri merkezi ve Azure veri merkezleri arasında ağ kullanılabilirliğine daha fazla bağımlılık.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Microsoft bulut hizmetlerinde şifreleme

Microsoft Cloud hizmetleri üç bulut modelinde de kullanılır: IaaS, PaaS, SaaS. Aşağıda her modele nasıl uyduklarına ilişkin örnekler var:

- Yazılım hizmetleri, Office 365 gibi bulut tarafından sağlanan bir sunucu veya SaaS olarak Yazılım olarak adlandırılır.
- Müşterilerin uygulamalarında bulutu kullanarak depolama, analitik ve servis veri yolundan işlevsellik gibi şeyler için bulutu kullanan platform hizmetleri.
- Altyapı hizmetleri veya müşterinin bulutta barındırılan işletim sistemleri ve uygulamaları dağıtdığı ve muhtemelen diğer bulut hizmetlerinden yararlandığı Bir Hizmet Olarak Altyapı (IaaS).

### <a name="encryption-at-rest-for-saas-customers"></a>SaaS müşterileri için istirahatte şifreleme

Hizmet olarak yazılım (SaaS) müşterileri genellikle her hizmette etkinleştirilen veya kullanılabilir durumda şifrelemeye sahiptir. Office 365'in müşterilerin şifrelemeyi doğrulaması veya etkinleştirmesi için çeşitli seçenekleri vardır. Office 365 hizmetleri hakkında daha fazla bilgi için [Office 365'te Şifreleme'ye](https://docs.microsoft.com/office365/securitycompliance/encryption)bakın.

### <a name="encryption-at-rest-for-paas-customers"></a>PaaS müşterileri için istirahatte şifreleme

Hizmet Olarak Platform (PaaS) müşterinin verileri genellikle Blob Depolama gibi bir depolama hizmetinde bulunur, ancak sanal makine gibi uygulama yürütme ortamında önbelleğe alınabilir veya depolanabilir. Kullanabileceğiniz dinlenme seçeneklerinde şifrelemeyi görmek için, kullandığınız depolama ve uygulama platformları için aşağıdaki tabloyu inceleyin.

### <a name="encryption-at-rest-for-iaas-customers"></a>IaaS müşterileri için istirahatte şifreleme

Hizmet olarak altyapı (IaaS) müşterileri çeşitli hizmet ve uygulamalar alabilirsiniz. IaaS hizmetleri, Azure Disk Şifrelemesi'ni kullanarak Azure barındırılan sanal makinelerde ve VHD'lerde şifrelemeyi tam olarak etkinleştirebilir.

#### <a name="encrypted-storage"></a>Şifreli depolama

PaaS gibi IaaS çözümleri de, verileri istirahatte depolayan diğer Azure hizmetlerinden yararlanabilir. Bu gibi durumlarda, tüketilen her Azure hizmeti tarafından sağlanan Kullanımda Şifreleme desteğini etkinleştirebilirsiniz. Aşağıdaki tabloda ana depolama, hizmetler ve uygulama platformları ve Desteklenen Rest'te Şifreleme modeli yer almaktadır. 

#### <a name="encrypted-compute"></a>Şifreli işlem

Tüm Yönetilen Diskler, Anlık Görüntüler ve Görüntüler, hizmet tarafından yönetilen bir anahtar kullanılarak Depolama Hizmeti Şifrelemesi kullanılarak şifrelenir. Daha eksiksiz bir Şifreleme de Rest çözümü, verilerin şifrelenmemiş biçimde asla kalıcı olmamasını sağlar. Sanal bir makinede veri işlenirken, veriler Windows sayfa dosyasına veya Linux takas dosyasına, bir kilitlenme dökümüne veya bir uygulama günlüğüne kalıcı olarak kullanılabilir. Bu verilerin istirahatte şifrelendiğinden emin olmak için, IaaS uygulamaları Azure IaaS sanal makinede (Windows veya Linux) ve sanal diskte Azure Disk Şifrelemesi kullanabilir.

#### <a name="custom-encryption-at-rest"></a>Özel şifreleme istirahatte

Mümkün olduğunda, IaaS uygulamalarının azure disk şifreleme ve şifreleme seçeneklerinde yararlanarak tüketilen Azure hizmetleri tarafından sağlanmalıdır. Düzensiz şifreleme gereksinimleri veya Azure tabanlı olmayan depolama gibi bazı durumlarda, bir IaaS uygulamasıgeliştiricisinin şifrelemeyi kendi başlarına uygulaması gerekebilir. IaaS çözümlerinin geliştiricileri, belirli Azure bileşenlerinden yararlanarak Azure yönetimi ve müşteri beklentileriyle daha iyi entegre olabilir. Özellikle, geliştiriciler güvenli anahtar depolama sağlamak ve müşterilerine çoğu Azure platform hizmetiyle tutarlı anahtar yönetimi seçenekleri sağlamak için Azure Key Vault hizmetini kullanmalıdır. Ayrıca, özel çözümler, hizmet hesaplarının şifreleme anahtarlarına erişmesini sağlamak için Azure Tarafından Yönetilen Hizmet Kimlikleri'ni kullanmalıdır. Azure Anahtar Kasası ve Yönetilen Hizmet Kimlikleri hakkında geliştirici bilgileri için ilgili SDK'larına bakın.

## <a name="azure-resource-providers-encryption-model-support"></a>Azure kaynak sağlayıcıları şifreleme modeli desteği

Microsoft Azure Hizmetleri'nin her biri, dinlenme modellerindeki şifrelemenin bir veya daha fazlasını destekler. Ancak bazı hizmetler için şifreleme modellerinden biri veya birkaçı geçerli olmayabilir. Müşteri tarafından yönetilen anahtar senaryoları destekleyen hizmetler için, yalnızca Azure Anahtar Atlama'nın anahtar şifreleme anahtarları için desteklediği anahtar türlerinin bir alt kümesini destekleyebilirler. Ayrıca, hizmetler bu senaryolar ve anahtar türleri için farklı zamanlamalarda destek bırakabilir. Bu bölümde, büyük Azure veri depolama hizmetlerinin her biri için bu yazı nın yazıldığı sırada istirahat desteğindeki şifreleme açıklanmaktadır.

### <a name="azure-disk-encryption"></a>Azure disk şifreleme

Azure Altyapı'yı Hizmet Olarak Kullanan (IaaS) özelliklerini kullanan tüm müşteriler, Azure Disk Şifrelemesi aracılığıyla IaaS VM'leri ve diskleri için tam bir şifreleme elde edebilir. Azure Disk şifrelemesi hakkında daha fazla bilgi için [Azure Disk Şifreleme belgelerine](../azure-security-disk-encryption-overview.md)bakın.

#### <a name="azure-storage"></a>Azure Storage

Tüm Azure Depolama hizmetleri (Blob depolama, Sıra depolama, Tablo depolama ve Azure Dosyaları) sunucu tarafındaki şifrelemeyi destekler; bazı hizmetler ayrıca müşteri tarafından yönetilen anahtarları ve istemci tarafı şifrelemeyi de destekler. 

- Sunucu tarafı: Tüm Azure Depolama Hizmetleri, uygulama için saydam olan hizmet yönetilen anahtarları kullanarak varsayılan olarak sunucu tarafı şifrelemesi sağlar. Daha fazla bilgi [için, Veriler için Azure Depolama Hizmeti Şifrelemesi'ne](../../storage/common/storage-service-encryption.md)bakın. Azure Blob depolama ve Azure Dosyaları, Azure Key Vault'ta RSA 2048 bit müşteri tarafından yönetilen anahtarları da destekler. Daha fazla bilgi için [Azure Anahtar Kasası'nda müşteri tarafından yönetilen anahtarları kullanarak Depolama Hizmeti Şifrelemesi'ne](../../storage/common/storage-encryption-keys-portal.md)bakın.
- İstemci tarafı: Azure Blobs, Tablolar ve Kuyruklar istemci tarafı şifrelemeyi destekler. İstemci tarafı şifrelemekullanırken, müşteriler verileri şifreler ve verileri şifreli bir blob olarak yükler. Anahtar yönetimi müşteri tarafından yapılır. Daha fazla bilgi için Microsoft [Azure Depolama için İstemci Tarafı Şifreleme ve Azure Anahtar Kasası'na](../../storage/common/storage-client-side-encryption.md)bakın.

#### <a name="azure-sql-database"></a>Azure SQL Veritabanı

Azure SQL Veritabanı şu anda Microsoft tarafından yönetilen hizmet tarafı ve istemci tarafı şifreleme senaryoları için şifrelemeyi destekler.

Sunucu şifreleme desteği şu anda Şeffaf Veri Şifreleme adı verilen SQL özelliği aracılığıyla sağlanmaktadır. Bir Azure SQL Veritabanı müşterisi, TDE tuşunun otomatik olarak oluşturulmasını ve onlar için yönetilmesini sağladığında. Veritabanı ve sunucu düzeylerinde şifreleme hazır da etkinleştirilebilir. Haziran 2017 itibariyle, [Saydam Veri Şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) varsayılan olarak yeni oluşturulan veritabanlarında etkinleştirilir. Azure SQL Veritabanı, Azure Key Vault'ta RSA 2048 bit müşteri tarafından yönetilen anahtarları destekler. Daha fazla bilgi için Azure [SQL Veritabanı ve Veri Ambarı için Kendi Anahtar Desteğinizi Getir ile Saydam Veri Şifreleme'ye](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql?view=azuresqldb-current)bakın.

Azure SQL Veritabanı verilerinin istemci tarafı şifrelemesi [Her Zaman Şifrelenmiş](https://msdn.microsoft.com/library/mt163865.aspx) özelliği aracılığıyla desteklenir. Her zaman Encrypted, istemci tarafından oluşturulan ve depolanan bir anahtar kullanır. Müşteriler ana anahtarı windows sertifika mağazasında, Azure Key Vault'ta veya yerel bir Donanım Güvenlik Modülünde saklayabilir. SQL Server Management Studio'u kullanan SQL kullanıcıları, hangi sütunu şifrelemek için hangi anahtarı kullanmak istediklerini seçer.

#### <a name="encryption-model-and-key-management-table"></a>Şifreleme Modeli ve anahtar yönetimi tablosu

|                                  |                    | **Şifreleme Modeli ve Anahtar Yönetimi** |                    |
|----------------------------------|--------------------|-----------------------------------------|--------------------|
|                                  | **Sunucu Tarafı Hizmet Yönetilen Anahtarı Kullanma**     | **Müşteri Tarafından Yönetilen Anahtarı Kullanarak Sunucu Tarafı**             | **İstemci Tarafından Yönetilen Anahtarı Kullanma İstemci Tarafı**      |
| **Yapay Al ve Makine Öğrenimi**      |                    |                    |                    |
| Azure Bilişsel Arama           | Evet                | Evet                | -                  |
| Azure Bilişsel Hizmetler         | Evet                | Evet                | -                  |
| Azure Machine Learning           | Evet                | Evet                | -                  |
| Azure Machine Learning Studio    | Evet                | Önizleme, RSA 2048-bit | -               |
| Content Moderator                | Evet                | Evet                | -                  |
| Yüz                             | Evet                | Evet                | -                  |
| Language Understanding           | Evet                | Evet                | -                  |
| Kişiselleştirme                     | Evet                | Evet                | -                  |
| Soru-Cevap Oluşturucu                        | Evet                | Evet                | -                  |
| Konuşma Hizmetleri                  | Evet                | Evet                | -                  |
| Translator Metin Çevirisi                  | Evet                | Evet                | -                  |
| Power BI                         | Evet                | Önizleme, RSA 2048-bit | -                  |
| **Analiz**                    |                    |                    |                    |
| Azure Stream Analytics           | Evet                | Yok\*            | -                  |
| Event Hubs                       | Evet                | Evet, tüm RSA Uzunlukları. | -                  |
| İşlevler                        | Evet                | Evet, tüm RSA Uzunlukları. | -                  |
| Azure Analysis Services          | Evet                | -                  | -                  |
| Azure Veri Kataloğu               | Evet                | -                  | -                  |
| Azure HDInsight                  | Evet                | Tümü                | -                  |
| Azure Monitör Uygulama Öngörüleri | Evet                | Evet                | -                  |
| Azure Monitör Günlük Analizi      | Evet                | Evet                | -                  |
| Azure Veri Gezgini              | Evet                | Evet                | -                  |
| Azure Data Factory               | Evet                | Evet                | -                  |
| Azure Data Lake Store            | Evet                | Evet, RSA 2048-bit  | -                  |
| **Kapsayıcılar**                   |                    |                    |                    |
| Azure Kubernetes Service         | Evet                | Evet                | -                  |
| Container Instances              | Evet                | Evet                | -                  |
| Container Kayıt Defteri               | Evet                | Evet                | -                  |
| **İşlem**                      |                    |                    |                    |
| Virtual Machines                 | Evet                | Evet, RSA 2048-bit  | -                  |
| Sanal Makine Ölçek Seti        | Evet                | Evet, RSA 2048-bit  | -                  |
| SAP HANA                         | Evet                | Evet, RSA 2048-bit  | -                  |
| App Service                      | Evet                | Evet\*\*            | -                  |
| Otomasyon                       | Evet                | Evet\*\*            | -                  |
| Azure İşlevleri                  | Evet                | Evet\*\*            | -                  |
| Azure Portal                     | Evet                | Evet\*\*            | -                  |
| Logic Apps                       | Evet                | Evet                | -                  |
| Azure Yönetilen Uygulamalar       | Evet                | Evet\*\*            | -                  |
| Service Bus                      | Evet                | Evet                | -                  |
| Site Recovery                    | Evet                | Evet                | -                  |
| **Veritabanları**                    |                    |                    |                    |
| Sanal Makinelerde SQL Server   | Evet                | Evet, RSA 2048-bit  | Evet                |
| Azure SQL Veritabanı               | Evet                | Evet, RSA 2048-bit  | Evet                |
| MariaDB için Azure SQL Veritabanı   | Evet                | -                  | -                  |
| MySQL için Azure SQL Veritabanı     | Evet                | Evet                | -                  |
| PostgreSQL için Azure SQL Veritabanı | Evet               | Evet                | -                  |
| Azure Synapse Analytics          | Evet                | Evet, RSA 2048-bit  | -                  |
| SQL Server Stretch Database      | Evet                | Evet, RSA 2048-bit  | Evet                |
| Tablo Depolama                    | Evet                | Evet                | Evet                |
| Azure Cosmos DB                  | Evet                | Evet                | -                  |
| Azure Databricks                 | Evet                | Evet                | -                  |
| Azure Veritabanı Geçiş Hizmeti | Evet                | Yok\*              | -                  |
| **DevOps**                       |                    |                    |                    |
| Azure DevOps Services            | Evet                | -                  | Evet                |
| Azure Repos                      | Evet                | -                  | Evet                |
| **Kimlik**                     |                    |                    |                    |
| Azure Active Directory           | Evet                | -                  | -                  |
| Azure Active Directory Domain Services | Evet          | Evet, RSA 2048-bit  | -                  |
| **Tümleştirme**                  |                    |                    |                    |
| Service Bus                      | Evet                | Evet                | Evet                |
| Event Grid                       | Evet                | -                  | -                  |
| API Management                   | Evet                | -                  | -                  |
| **IoT Hizmetleri**                 |                    |                    |                    |
| IoT Hub                          | Evet                | Evet                | Evet                |
| IoT Hub Cihaz Sağlama      | Evet                | Evet                | -                  |
| **Yönetim ve İdare**    |                    |                    |                    |
| Azure Site Recovery              | Evet                | -                  | -                  |
| Azure Geçişi                    | Evet                | Evet                | -                  |
| **Medya**                        |                    |                    |                    |
| Media Services                   | Evet                | -                  | Evet                |
| **Güvenlik**                     |                    |                    |                    |
| IoT için Azure Güvenlik Merkezi    | Evet                | Evet                | -                  |
| Azure Sentinel                   | Evet                | Evet                | -                  |
| **Depolama**                      |                    |                    |                    |
| Blob Depolama                     | Evet                | Evet, RSA 2048-bit  | Evet                |
| Premium Blob Depolama             | Evet                | Evet, RSA 2048-bit  | Evet                |
| Disk Depolama                     | Evet                | Evet                | -                  |
| Ultra Disk Depolama               | Evet                | Evet                | -                  |
| Yönetilen Disk Depolama             | Evet                | Evet                | -                  |
| Dosya Depolama                     | Evet                | Evet, RSA 2048-bit  | -                  |
| Dosya Premium Depolama             | Evet                | Evet, RSA 2048-bit  | -                  |
| Dosya Eşitleme                        | Evet                | Evet, RSA 2048-bit  | -                  |
| Kuyruk Depolama                    | Evet                | Evet                | Evet                |
| Avere vFXT                       | Evet                | -                  | -                  |
| Redis için Azure Önbelleği            | Evet                | Yok\*              | -                  |
| Azure NetApp Files               | Evet                | Evet                | -                  |
| Arşiv Depolama                  | Evet                | Evet, RSA 2048-bit  | -                  |
| StorSimple                       | Evet                | Evet, RSA 2048-bit  | Evet                |
| Azure Backup                     | Evet                | Evet                | Evet                |
| Data Box                         | Evet                | -                  | Evet                |
| Data Box Edge                    | Evet                | Evet                | -                  |

\*Bu hizmet verileri kalıcı olarak devam etmez. Geçici önbellekler, varsa, bir Microsoft anahtarı ile şifrelenir.

\*\*Bu hizmet, verileri kendi Key Vault, Storage Account veya Müşteri Tarafından Yönetilen Anahtarla Sunucu Tarafı Şifrelemesini zaten destekleyen diğer veri kalıcı hizmetinizde depolamayı destekler.

## <a name="conclusion"></a>Sonuç

Azure Hizmetleri'nde depolanan müşteri verilerinin korunması Microsoft için son derece önemlidir. Tüm Azure barındırılan hizmetler, Rest'te Şifreleme seçenekleri sunmaya kendini adamıştır. Azure hizmetleri, hizmet tarafından yönetilen anahtarları, müşteri tarafından yönetilen anahtarları veya istemci tarafı şifrelemeyi destekler. Azure hizmetleri, Rest kullanılabilirliği nde Şifreleme'yi genel olarak geliştirmeve önümüzdeki aylarda önizleme ve genel kullanılabilirlik için yeni seçenekler planlanmaktadır.
