---
title: Azure Veri Paylaşımı nedir?
description: Azure Veri Paylaşımı'nı kullanarak verileri birden çok müşteri ve iş ortağıyla basit ve güvenli bir şekilde paylaşma hakkında bilgi edinin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.openlocfilehash: 363feda1409d2bb54e60d1b2168cba38f2a8a41c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77621863"
---
# <a name="what-is-azure-data-share"></a>Azure Veri Paylaşımı nedir?

Günümüz dünyasında veriler, birçok kuruluşun müşterileri ve iş ortakları ile basit ve güvenli bir şekilde paylaşması gereken önemli bir stratejik varlık olarak görülüyor. Müşterilerin ftp, e-posta, API'ler aracılığıyla birkaç isim de dahil olmak üzere, bugün bunu birçok yolu vardır. Kuruluşlar verilerini kimlerle paylaştıklarının izini kolayca kaybedebilir. FTP aracılığıyla veya kendi API altyapılarını ayakta tatarak veri paylaşımının sağlanması ve yönetilmesi genellikle pahalıdır. Bu paylaşım yöntemlerini büyük ölçekte kullanmakla ilişkili yönetim yükü vardır. 

Birçok kuruluşun paylaştığı verilerden sorumlu olması gerekir. Hesap verebilirliğe ek olarak, birçok kuruluş tüm veri paylaşımlarını basit bir şekilde kontrol etmek, yönetmek ve izlemek ister. Verilerin üstel bir hızda büyümeye devam etmesinin beklendiği günümüz dünyasında, kuruluşların büyük verileri paylaşmak için basit bir yola ihtiyacı vardır. Müşteriler, zamanında öngörüler elde edebilmeleri için en güncel verileri ister.

Azure Veri Paylaşımı, kuruluşların verileri birden çok müşteri ve iş ortağıyla basit ve güvenli bir şekilde paylaşmasını sağlar. Yalnızca birkaç tıklamayla, yeni bir veri paylaşım hesabı sağlayabilir, veri kümeleri ekleyebilir ve müşterilerinizi ve iş ortaklarınızı veri paylaşımınıza davet edebilirsiniz. Veri sağlayıcıları her zaman paylaştıkları verilerin denetimindedir. Azure Veri Paylaşımı, hangi verilerin ne zaman ve kim tarafından paylaşıldığını yönetmeyi ve izlemeyi kolaylaştırır. 

Bir veri sağlayıcısı, veri paylaşımı için kullanım koşulları belirterek verilerinin nasıl işleneceğini denetleyebilir. Veri tüketicisinin verileri alabilmek için önce bu koşulları kabul etmesi gerekir. Veri sağlayıcıları, veri tüketicilerinin güncelleştirmeleri ne sıklıkta alacağını belirtebilir. Veri sağlayıcıları istedikleri zaman yeni güncelleştirmelere erişimi iptal edebilir. 

Azure Veri Paylaşımı, analiz ve AI senaryolarını zenginleştirmek için üçüncü taraflardan gelen verileri birleştirmeyi kolaylaştırarak öngörüleri geliştirmeye yardımcı olur. Azure Veri Paylaşımı'nı kullanarak paylaşılan verileri hazırlamak, işlemek ve analiz etmek için Azure analiz araçlarının gücünü kolayca kullanın. 

Verileri paylaşmak ve almak için hem veri sağlayıcısının hem de veri tüketicisinin bir Azure aboneliğine sahip olması gerekir. Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

## <a name="scenarios-for-azure-data-share"></a>Azure Veri Paylaşımı için Senaryolar

Azure Veri Paylaşımı birçok farklı sektörde kullanılabilir. Örneğin, bir perakendeci son satış noktası verilerini tedarikçileri ile paylaşmak isteyebilir. Bir perakendeci, Azure Veri Paylaşımı'nı kullanarak tüm tedarikçileri için satış noktası verilerini içeren bir veri paylaşımı oluşturabilir ve satışları saatlik veya günlük olarak paylaşabilir. 

Azure Veri Paylaşımı, belirli bir sektör için bir veri pazarı oluşturmak için de kullanılabilir. Örneğin, nüfus artışı yla ilgili anonim verileri düzenli olarak üçüncü taraflarla paylaşan bir hükümet veya araştırma kurumu. 

Azure Veri Paylaşımı için bir diğer kullanım örneği de bir veri konsorsiyumu oluşturmaktır. Örneğin, bir dizi farklı araştırma kurumu verileri tek bir güvenilir kuruluşla paylaşabilir. Veriler, Azure analiz araçları kullanılarak analiz edilir, toplanır veya işlenir ve ilgili taraflarla paylaşılır. 

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Veri Paylaşımı şu anda anlık görüntü tabanlı paylaşım ve yerinde paylaşım sunar. 

Anlık görüntü tabanlı paylaşımda, veriler veri sağlayıcısının Azure aboneliğinden hareket eder ve veri tüketicisinin Azure aboneliğine dahil eder. Veri sağlayıcısı olarak, bir veri paylaşımı sağlama ve alıcıları veri paylaşımına davet emzebilirsiniz. Veri tüketicileri e-posta yoluyla veri paylaşımınız için bir davet alır. Bir veri tüketicisi daveti kabul ettikten sonra, kendileriyle paylaşılan verilerin tam anlık görüntüsünü tetikleyebilir. Bu veriler veri tüketicileri depolama hesabına alınır. Veri tüketicileri, her zaman verilerin en son sürümüne sahip olmaları için kendileriyle paylaşılan verileriçin düzenli ve artımlı güncelleştirmeler alabilir. 

Veri sağlayıcıları, veri tüketicilerine anlık görüntü çizelgesi aracılığıyla kendileriyle paylaşılan verilerle ilgili artımlı güncelleştirmeler sunabilir. Anlık görüntü zamanlamaları saatlik veya günlük olarak sunulur. Bir veri tüketicisi veri paylaşımını kabul ettiğinde ve yapılandırdığında, anlık görüntü zamanlamasına abone olabilir. Bu, paylaşılan verilerin düzenli olarak güncelleştirildiği ve tüketicinin en güncel verilere ihtiyaç duyduğu senaryolarda yararlıdır. 

![veri paylaşımı akışı](media/data-share-flow.png)

Bir veri tüketicisi bir veri paylaşımını kabul ettiğinde, verileri kendi seçtikleri bir veri deposunda alabilir. Örneğin, veri sağlayıcısı Azure Blob Depolama'yı kullanarak veri paylaşıyorsa, veri tüketicisi bu verileri Azure Veri Gölü Deposu'ndan alabilir. Benzer şekilde, veri sağlayıcısı bir Azure SQL Veri Ambarı'ndan veri paylaşıyorsa, veri tüketicisi verileri Bir Azure Veri Gölü Deposu'nda mı, Azure SQL Veritabanı'nda mı yoksa Azure SQL Veri Ambarı'na mı almak istediklerini seçebilir. SQL tabanlı kaynaklardan paylaşım durumunda, veri tüketicisi de parke veya csv veri almak olup olmadığını seçebilirsiniz. 

Yerinde paylaşımla, veri sağlayıcıları verileri kopyalamadan bulunduğu yerde veri paylaşabilir. Davet akışı üzerinden paylaşım ilişkisi kurulduktan sonra, veri sağlayıcısının kaynak veri deposu ile veri tüketicisinin hedef veri deposu arasında sembolik bir bağlantı oluşturulur. Veri tüketicisi, kendi veri deposunu kullanarak verileri gerçek zamanlı olarak okuyabilir ve sorgulayabilir. Kaynak veri deposundaki değişiklikler, veri tüketicisi tarafından hemen kullanılabilir. Yerinde paylaşım şu anda Azure Veri Gezgini için önizlemededir.

## <a name="key-capabilities"></a>Temel işlevler

Azure Veri Paylaşımı, veri sağlayıcılarının şunları elde etmesini sağlar:

* [Desteklenen veri depoları](supported-data-stores.md) listesindeki verileri kuruluşunuz dışındaki müşteriler ve iş ortaklarıyla paylaşın

* Verilerinizi kiminle paylaştığınızı takip edin

* Anlık görüntü veya yerinde paylaşım seçimi

* Veri tüketicilerinizin verilerinizde ne sıklıkta güncelleme ler aldığı

* Müşterilerinizin verilerinizin en son sürümünü gerektiği gibi çekmesine veya sizin tanımladığınız bir aralıkta verilerinizde otomatik olarak artımlı değişiklikler almasına izin verme

Azure Veri Paylaşımı, veri tüketicilerinin şunları yapmalarını sağlar: 

* Paylaşılan veri türünün açıklamasını görüntüleme

* Veriler için kullanım koşullarını görüntüleme

* Azure Veri Paylaşımı davetini kabul veya reddetme

* Sizinle paylaşılan verileri desteklenen bir veri deposunda kabul [edin.](supported-data-stores.md)

* Kuruluşun sizinle paylaştığı Veri Paylaşımının tam veya artımlı anlık görüntüsünü tetikleme

* Artımlı anlık görüntü aracılığıyla verilerin en son kopyasını almak için veri paylaşımına abone olun

Yukarıda listelenen tüm önemli özellikler Azure portalı veya REST API'leri aracılığıyla desteklenir. REST API'leri aracılığıyla Azure Veri Paylaşımı'nı kullanma hakkında daha fazla bilgi için başvuru belgelerimize göz atın. 

## <a name="security"></a>Güvenlik

Azure Veri Paylaşımı, Azure'un verileri istirahatte ve aktarım sırasında korumak için sunduğu temel güvenlikten yararlanır. Veriler, temel veri deposu tarafından desteklenen istirahatte şifrelenir. Veriler de aktarım sırasında şifrelenir. Veri paylaşımıyla ilgili meta veriler de istirahatte ve aktarım sırasında şifrelenir. 

Erişim denetimleri, yetkili kişiler tarafından erişilmesini sağlamak için Azure Veri Paylaşımı kaynak düzeyinde ayarlanabilir. 

Azure Veri Paylaşımı, Azure Etkin Dizini'nde otomatik kimlik yönetimi için Azure Kaynakları için Yönetilen Kimliklerden (önceden MSIs olarak bilinir) yararlanır. Azure Kaynakları için yönetilen kimlikler, veri paylaşımı için kullanılan veri depolarına erişmek için kullanılır. Veri sağlayıcısı ile veri tüketicisi arasında kimlik bilgisi alışverişi yoktur. Daha fazla bilgi için [Azure Kaynakları için Yönetilen Kimlikler sayfasına](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)bakın. 


## <a name="supported-regions"></a>Desteklenen bölgeler

Azure Veri Paylaşımını kullanılabilir hale getiren Azure bölgelerinin listesi için lütfen bölge sayfasına göre [kullanılabilen ürünlere](https://azure.microsoft.com/global-infrastructure/services/?products=data-share) bakın ve Azure Veri Paylaşımı'nı arayın. 

Azure Veri Paylaşımı, verilerin bir kopyasını depolamaz. Veriler, paylaşılan temel veri deposunda depolanır. Örneğin, bir veri üreticisi verilerini Batı ABD'de bulunan bir Azure Veri Gölü Deposu hesabında depolarsa, bu verilerin depolandığı yerdir. Verileri anlık görüntü aracılığıyla Batı Avrupa'da bulunan bir Azure Depolama hesabıyla paylaşıyorlarsa, genellikle veriler doğrudan Batı Avrupa'da bulunan Azure Depolama hesabına aktarılır.

Azure Veri Paylaşımı hizmetinin hizmetten yararlanmak için bölgenizde kullanılabilmesi gerekmez. Örneğin, Azure Veri Paylaşımı'nın henüz kullanılamadığı bir bölgede bulunan bir Azure Depolama hesabında depolanan verileriniz varsa, verilerinizi paylaşmak için hizmetten yararlanmaya devam edebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Veri paylaşmaya nasıl başlayacağınızı öğrenmek [için, veri öğreticinizi paylaşmaya devam edin.](share-your-data.md)
