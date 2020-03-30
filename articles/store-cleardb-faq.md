---
title: Azure Uygulama Hizmeti ile ClearDB MySql veritabanları için SSS
description: Azure Uygulama Hizmeti ile ClearDB MySQL veritabanlarını kullanma yla ilgili sık sorulan soruların yanıtları.
documentationcenter: php
services: mysql
author: sunbuild
manager: yochayk
tags: mysql
ms.service: multiple
ms.topic: article
ms.date: 10/27/2016
ms.author: sumuth
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ddbf1eb960a24cfaa8d09ab45b6febca07e6d504
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979922"
---
# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Azure Uygulama Hizmeti ile ClearDB MySql veritabanları için SSS
Bu SSS, Azure Web Uygulamaları için ClearDB MySQL veritabanlarını kullanma ve satın alma yla ilgili sık sorulan soruları yanıtlar.

> [!IMPORTANT]
> 13 Haziran 2018 itibarıyla ClearDB, Microsoft tarafından faturalanan Azure tabanlı müşterileri ClearDB içeren doğrudan bir faturalandırma modeline geçiş lettirdi. Bu makaledeki bilgiler artık güncel değil. Azure'da oluşturulan bir ClearDB veritabanını oluşturamaz veya yükseltemezsiniz.
>
> Daha fazla ayrıntı ve sonraki adımlar [için ClearDB hizmet planlarında yapılan değişikliklere](https://w2.cleardb.net/important-change-of-billing-notice-for-all-azure-cleardb-service-plans/)bakın.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Azure'da MySQL için hangi seçeneklerim var?
Bu hizmetle ilgili en son bilgiler için [ClearDB'ye](https://w2.cleardb.net/) bakın. ClearDB bir MySQL barındırma hizmetidir ve MySQL altyapısını sizin için yönetir. 

MySQL'i Azure'da barındırmak için başka seçenekleriniz de var:
* [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql/)
* [Azure VM'de çalışan MySQL kümesi](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)
* [Azure VM'de çalışan MySQL'in tek örneği](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Azure Marketi'ndeki Web uygulaması + MySQL şablonu için kredi kartına ihtiyacım var mı?
Bu, kullandığınız abonelik türüne bağlıdır. Sık kullanılan bazı abonelik türleri şunlardır:

* [Gittiğiniz Gibi Öde](https://azure.microsoft.com/offers/ms-azr-0003p/): Bir kredi kartı gerektirir ve ücretli bir MySQL veritabanı satın aldığınızda kredi kartınızdan ücret alınır.
* [Ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/): Microsoft Azure hizmetlerinde kullanılmak üzere kredileri içerir, ancak üçüncü taraf kaynaklarının satın alınmasına izin vermez. Üçüncü taraf hizmetleri veya ücretli bir MySQL veritabanı satın almak için kredi kartı etkin leştirilmiş bir abonelik kullanmanız gerekir. Web Apps için ÜCRETSİz ClearDB MySQL veritabanı oluşturabilirsiniz.
* [MSDN aboneliği](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/) ve **MSDN Dev Test Ödeme gibi gidin**: Ücretsiz deneme benzer şekilde, bir MSDN aboneliği ClearDB'den ücretli bir MySQL çözümü satın almak için bir kredi kartına sahip olmayı gerektirir.
* [Kurumsal Sözleşme (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/): EA müşterileri, ayrı, konsolide bir faturada tüm Azure Marketi (üçüncü taraf) satın alımları için her üç ayda bir EA'larına göre faturalandırılır. Herhangi bir pazar yeri alımları için parasal taahhüdün dışında faturalandırılırsınız. Azure Store şu anda Azerbaycan, Hırvatistan, Norveç ve Porto Riko'ya kayıtlı müşteriler tarafından kullanılamıyor. 

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Azure Marketi'nden bir Web uygulaması + MySQL için neden 3,50 TL ücretlendirildim?
Varsayılan veritabanı seçeneği 3,50 $ olan Titan vardır. Veritabanı oluşturma sırasında maliyeti göstermiyoruz ve yanlışlıkla istemediğiniz bir veritabanı satın alabilirsiniz. Deneyimi geliştirmenin bir yolunu bulmaya çalışıyoruz, ancak o zamana kadar **Create'i** tıklatıp kaynakların dağıtımını başlatmadan önce web uygulaması ve veritabanı için seçtiğiniz tüm fiyatlandırma katmanlarını kontrol etmelisiniz.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>MySQL'i kendi Azure sanal makinemde çalıştırıyorum. Azure web uygulamamı veritabanıma bağlayabilir miyim?
Evet. Azure VM'niz web uygulamanıza uzaktan erişim izni verdiği sürece web uygulamanızı veritabanınıza bağlayabilirsiniz. Daha fazla bilgi için [mysql'i sanal bir makineye yükleyin'](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)e bakın.

## <a name="in-which-countriesregions-are-cleardb-premium-mysql-clusters-supported"></a>ClearDB Premium MySQL kümeleri hangi ülkelerde/bölgelerde desteklenir?
ClearDB Premium MySQL kümeleri, Hindistan, Avustralya, Brezilya Güney ve Çin hariç tüm Azure bölgelerinde kullanılabilir.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>ClearDB premium küme çözümü içeren bir veritabanı oluşturmadan önce yeni bir küme oluşturabilir miyim?
Hayır, boş ClearDB kümeleri oluşturma desteklenmez. Azure portalı, kümede veritabanları oluşturmanıza olanak tanır ve bu da aynı anda yeni bir küme oluşturabilir.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Uygulamalarımdan biri tarafından kullanılan ClearDB MySQL veritabanını silmeyi denersem uyarılacak mıyım?
Hayır, Azure, uygulamanızın bağlı olduğu bir pazar yeri satın alımını silerseniz sizi uyarmayınız.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>ClearDB veritabanlarını hangi bölgelerde oluşturabilirim?
Azure Marketi, Azerbaycan, Hırvatistan, Norveç veya Porto Riko'da kayıtlı müşteriler tarafından kullanılamaz. ClearDB bu bölgelerde kullanılamaz.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Bir üretim web uygulaması ve veritabanı için hangi fiyatlandırma katmanını seçmeliyim?
Web Apps için Temel veya daha yüksek bir fiyatlandırma katmanı kullanın. ClearDB için, bir Satürn veya Jüpiter planı öneririz. İhtiyaçlarınıza uyan ı seçmek için hem [Web Uygulamaları](https://azure.microsoft.com/pricing/details/app-service/) hem de [ClearDB MySQL veritabanları](https://w2.cleardb.net/important-change-of-billing-notice-for-all-azure-cleardb-service-plans/) için her fiyatlandırma katmanının & sınırlamalarının özelliklerini gözden geçirin.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>ClearDB veritabanımı bir plandan diğerine nasıl yükseltebilirim?
Azure [portalında,](https://portal.azure.com)ClearDB paylaşılan barındırma veritabanını ölçeklendirebilirsiniz. Daha fazla bilgi edinmek için bu [makaleyi](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) okuyun. Şu anda Azure portalındaki ClearDB Premium kümelerinin yükseltisini destekliyoruz.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Azure portalında ClearDB veritabanımı göremiyorum?
Klasik olarak bir ClearDB veritabanı oluşturduysanız, veritabanınızı [Azure portalında](https://portal.azure.com)göremezsiniz. Bu senaryo için geçici çözüm yoktur.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Veritabanım çöktüğinde destek için kime başvuruyorum?
Veritabanıyla ilgili sorunlar için [ClearDB desteğine](https://www.cleardb.com/developers/help/support) başvurun. Azure abonelik bilgilerinizi sağlamaya hazır olun.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>ClearDB MySQL veritabanı küme çözümüm için ek kullanıcılar oluşturabilir miyim?
Hayır. Ek kullanıcılar oluşturamazsınız, ancak ClearDB veritabanı kümenizde ek veritabanları oluşturabilirsiniz.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Basic/Pro serisi veritabanları, ClearDB portalında bugün Planetary planlarına benzer bir yerinde yükseltilebilir mi?
Evet, Temel seri veritabanları yerinde yükseltilebilir (Basic 60 ile Basic 500). Pro serisi, Pro 60 hariç yerinde (Pro 125'ten Pro 1000'e kadar) yükseltilebilir. Şu anda Pro 60 veritabanının yükseltilmesine destek liyoruz. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Kaynaklarımı bir abonelikten diğerine geçirtdiğimde, ClearDB MySQL veritabanım da geçirilir mi?
Abonelikler arasında kaynak geçişi gerçekleştirdiğinizde, bazı [sınırlamalar](azure-resource-manager/management/move-support-resources.md) uygulanır. ClearDB MySQL veritabanı bir üçüncü taraf hizmetidir ve bu nedenle Azure abonelik geçişi sırasında geçirilmeyecektir. Azure kaynaklarını geçirmeden önce MySQL veritabanınızın geçişini yönetemezseniz, ClearDB MySQL veritabanlarınız devre dışı olabilir. Önce veritabanlarınızı el ile geçirin ve ardından web uygulamanız için Azure abonelik geçişi gerçekleştirin. 

## <a name="i-hit-the-spending-limit-on-my-subscription-i-removed-the-limit-and-my-app-service-is-online-however-the-database-is-not-accessible-how-do-i-re-enable-the-cleardb-database"></a>Aboneliğimin harcama limitine ulaştım. Sınırı kaldırdım ve Uygulama Hizmetim çevrimiçi, ancak veritabanına erişilemedi. ClearDB veritabanını nasıl yeniden etkinleştirebilirim?
Veritabanını yeniden etkinleştirmek için [ClearDB desteğine](https://www.cleardb.com/developers/help/support) başvurun. Azure abonelik bilgilerinizi ve veritabanı adınızı sağlayın.

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>ClearDB veritabanını kredi kartı aboneliğinden EA aboneliğine aktarabilir miyim?
Varolan ClearDB veritabanları, varolan aboneliklerle ilişkili kredi kartını kullanır. Bir EA aboneliğini kullanmak için verilerinizi yeni bir veritabanına geçirmeniz gerekir:

* EA aboneliğinizle yeni bir ClearDB veritabanı satın alın.
* Verilerinizi yeni veritabanınıza geçirin.
* Yeni veritabanını kullanmak için uygulamanızı güncelleştirin.
* Eski ClearDB veritabanınızı silin.

MySQL (ClearDB) ile yeni bir web uygulaması oluşturduğunuzda veya bir MySQL veritabanı (ClearDB) oluşturduğunuzda, seçtiğiniz abonelik hizmet için nasıl ödeme yapacağınızı belirler. EA aboneliği yle, Azure portalında ClearDB gibi üçüncü taraf hizmetlerinin satın alımını engellemeyiz. EA abonelikleri Parasal Taahhüt dışında faturalandırılır ve üç ayda bir ve borç olarak faturalandırılır. EA müşterisi, herhangi bir üçüncü taraf pazar yeri hizmetlerini ödemek için kredi kartı gibi bir ödeme yöntemi ayarlaması gerekir.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>EA aboneliğinde ClearDB kaynaklarının ücretlerini nerede görebiliyorum?
Doğrudan EA müşterileri için Azure Market ücretleri Kurumsal Portal'da görülebilir. Tüm pazar yeri satın alımlarının ve tüketimin Parasal Taahhüt dışında faturalandırıldıgini ve üç ayda bir ve borçlandirin. EA müşterileri doğrudan üçüncü taraf hizmet sağlayıcılarına ödeme yapmak zorunda ve bunu EA hesabı olan kredi kartı gibi bir ödeme yöntemini etkinleştirerek yapabilir.

Dolaylı EA müşterileri Azure Marketi aboneliklerini Kurumsal Portal'ın **Abonelikleri Yönet** sayfasında bulabilir, ancak fiyatlandırma gizlidir. Müşteriler, Market fiyatları için LSP ile iletişime geçmelidir.

Üçüncü taraf hizmetleri için Azure Marketi'ne erişim, EA Azure kayıt yöneticileriniz tarafından yönetilebilir. Kurumsal Portal'daki Hesaplar bölümü altında Hesapları yönet ve Abonelikleri Yönet'te Mağaza'dan yapılan üçüncü taraf satın alımlarını devre dışı bırakabilir veya yeniden etkinleştirebilirler.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>EA aboneliğimdeki ClearDB hizmetleri için faturamla ilgili sorularınız için kiminle iletişim kuracağım?
EA kayıtları altında faturalandırma ile ilgili olarak [Kurumsal Müşteri Desteği'ne](https://aka.ms/AzureEntSupport) başvurun. EA Portal Destek Ekibi sorunuza yanıt verecek veya sorununuzu çözmeye yardımcı olacaktır.

## <a name="more-information"></a>Daha fazla bilgi
[Azure Market Hakkında SSS](https://azure.microsoft.com/marketplace/faq/)

