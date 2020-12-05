---
title: Azure geçişi ile geçiş projelerini ölçekli olarak yönetme
description: Atanan müşteri kaynakları için Azure geçişi 'ni etkin bir şekilde kullanmayı öğrenin.
ms.date: 12/4/2020
ms.topic: how-to
ms.openlocfilehash: d1a01149c80b30f279f7d68551946c3ffe404d5e
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2020
ms.locfileid: "96621578"
---
# <a name="manage-migration-projects-at-scale-with-azure-migrate"></a>Azure geçişi ile geçiş projelerini ölçekli olarak yönetme

Hizmet sağlayıcı olarak, [Azure açık Thouse](../overview.md)için birden fazla müşteri kiracısından eklendi olabilirsiniz. Azure Mathouse, hizmet sağlayıcılarının birden çok Azure Active Directory (Azure AD) kiracılarının aynı anda, yönetim görevlerini daha verimli hale getiren işlemleri gerçekleştirmesine olanak tanır.

[Azure geçişi](../../migrate/migrate-services-overview.md) , Azure şirket içi sunucuları, altyapısı, uygulamaları ve verileri değerlendirmek ve bu uygulamalara geçiş yapmak için merkezi bir merkez sağlar. Genellikle, birden çok müşteriye yönelik ölçekte değerlendirme ve geçiş gerçekleştiren iş ortakları, [CSP (bulut çözümü sağlayıcısı) abonelik modelini](/partner-center/customers-revoke-admin-privileges) kullanarak veya [Müşteri kiracısında bir Konuk Kullanıcı oluşturarak](/azure/active-directory/external-identities/what-is-b2b)her bir müşteri aboneliğine tek tek erişmelidir.

Azure Sthouse Azure geçişi ile tümleştirme, hizmet sağlayıcılarının iş yüklerini farklı müşterilere uygun ölçekte bulmasına, değerlendirmesine ve geçirmeye izin verirken, müşterilerin ortamlarının tam görünürlük ve denetim sahibi olmasını sağlar. Azure Temsilcili kaynak yönetimi sayesinde, hizmet sağlayıcılarının birden çok müşteri kiracısından yönettikleri tüm Azure geçiş projelerini tek bir görünümü vardır.

> [!NOTE]
> Azure Athouse aracılığıyla iş ortakları, şirket içi VMware VM 'Leri, Hyper-V VM 'Leri, fiziksel sunucular ve AWS/GCP örnekleri için bulma, değerlendirme ve geçiş işlemleri gerçekleştirebilir. [VMware VM geçişi](../../migrate/server-migrate-overview.md)için iki seçenek vardır. Şu anda, temsilci bir müşteri aboneliğinde geçiş projesi üzerinde çalışırken yalnızca aracı tabanlı geçiş yöntemi kullanılabilir; Aracısız çoğaltma kullanılarak geçiş, müşterinin kapsamına temsilci erişimi aracılığıyla şu anda desteklenmemektedir.

Bu konuda, [Azure geçişi](../../migrate/migrate-services-overview.md) 'nin ölçeklenebilir bir şekilde nasıl kullanılacağına ilişkin bir genel bakış sunulmaktadır.

> [!TIP]
> Bu konudaki hizmet sağlayıcılarına ve müşterilere başvurduğumuz halde, bu kılavuz [birden çok kiracıyı yönetmek Için Azure açık Thouse kullanan kuruluşlar](../concepts/enterprise.md)için de geçerlidir.

Senaryonuza bağlı olarak, müşteri kiracısında veya yönetim kiracınızda Azure geçişi oluşturmak isteyebilirsiniz. Aşağıdaki konuları gözden geçirin ve müşterinizin geçiş ihtiyaçlarına en uygun modeli saptayın.

## <a name="create-an-azure-migrate-project-in-the-customer-tenant"></a>Müşteri kiracısında bir Azure geçişi projesi oluşturma

Azure Aydınlathouse kullanılırken bir seçenek, müşteri kiracısında Azure geçişi projesi oluşturmaktır. Ardından, yönetim kiracısındaki kullanıcılar, bir geçiş projesi oluştururken müşteri aboneliğini seçebilir. Hizmet sağlayıcı, yönetim kiracısından gerekli geçiş işlemlerini gerçekleştirebilir. Bu, iş yüklerini keşfeterek iş yüklerini değerlendirmek ve buluta ilişkin maliyetleri hesaplamak, VM hazırlığını gözden geçirmek ve geçişi gerçekleştirmek için Azure geçişi gerecini dağıtmaya dahil olabilir.

Bu senaryoda, bulma ve değerlendirme adımları bu kiracıya başlatılıp yürütülebileceğini bile yönetim kiracısında hiçbir kaynak oluşturulmaz ve depolanmayacak. Geçiş projeleri, şirket içi iş yükleri için değerlendirme raporları ve hedef hedefteki geçirilmiş kaynaklar gibi tüm kaynaklar müşteri aboneliğine dağıtılır. Ancak, hizmet sağlayıcısı tüm müşteri projelerine kendi kiracı ve Portal deneyimlerinden erişebilir.

Bu yaklaşım, birden fazla müşteri genelinde çalışan hizmet sağlayıcıları için bağlam geçişini en aza indirir, ancak müşterilerin tüm kiracılarındaki kaynaklarını tutmasına izin verir.

Bu modelin iş akışı aşağıdakine benzer olacaktır:

1. Müşteri, [Azure açık eklendi](onboard-customer.md). Azure geçişi ile kullanılacak kimlik için katkıda bulunan yerleşik rolü gereklidir. Bu rolü kullanan bir örnek için, [temsilci-Resource-Management-azmigrate](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management-azmigrate) örnek şablonuna bakın.
1. Belirlenen Kullanıcı Azure portal yönetim kiracısında oturum açar ve Azure geçişi ' ne gider. Bu Kullanıcı, uygun Temsilcili müşteri aboneliğini seçerek [bir Azure geçişi projesi oluşturur](/migrate/create-manage-projects.md).
1. Kullanıcı daha sonra [bulma ve değerlendirme için adımları gerçekleştirir](../../migrate/tutorial-discover-vmware.md).

   VMware VM 'Leri için, gereci yapılandırmadan önce, bulmayı vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, konaklar klasörü veya ayrı VM 'Ler için sınırlayabilirsiniz. Kapsamı ayarlamak için, gerecin vCenter Server erişmek için kullandığı hesaba izinler atayın. Hiper yöneticide birden fazla müşterinin VM 'Leri barındırıldığı durumlarda bu faydalıdır. Hyper-V ' d i bulma kapsamını sınırlandıramıyoruz.

    > [!NOTE]
    > Azure 'un sunduğu temsilci erişimi aracılığıyla Azure 'da geçiş yaparak, VMware sanal makinelerini geçiş için bulabilir ve değerlendirebilirsiniz. VMware sanal makinelerinin geçirilmesi için, temsilci bir müşteri aboneliğinde geçiş projesi üzerinde çalışırken yalnızca aracı tabanlı yöntem desteklenir.

1. Hedef müşteri aboneliği hazırlandığı zaman, Azure açık Thouse tarafından verilen erişim yoluyla geçişe devam edin. Değerlendirme sonuçlarını ve geçirilen kaynakları içeren geçiş projesi, müşteri kiracısında hedef aboneliğin altında oluşturulur.

> [!TIP]
> Geçiş öncesinde, temel altyapı kaynaklarını sağlamak ve sanal makinelerin geçirilebileceği aboneliği hazırlamak için bir giriş bölgesinin dağıtılması gerekir. Bu giriş bölgesindeki bazı kaynaklara erişmek veya bu kaynakları oluşturmak için, mevcut yerleşik rol gerekli olabilir. Bu, şu anda Azure 'un açık bir kullanım sürümünde desteklenmemektedir. Bu tür senaryolarda, müşterinin, CSP abonelik modeli aracılığıyla Konuk erişim rolü sağlaması veya yönetici erişimi sağlaması gerekebilir. Çok kiracılı giriş bölgelerini oluşturmaya yönelik bir yaklaşım için bkz. GitHub 'da [çok kiracılı-iniş-Zones tanıtım çözümü](https://github.com/Azure/Multi-tenant-Landing-Zones) .

## <a name="create-an-azure-migrate-project-in-the-managing-tenant"></a>Yönetim kiracısında bir Azure geçişi projesi oluşturma

Bu senaryoda, bulma ve değerlendirme gibi geçişiyle ilgili işlemler, yönetim kiracısındaki kullanıcılar tarafından yine de gerçekleştirilir. Bununla birlikte, geçiş projesi ve tüm ilgili kaynaklar iş ortağı kiracısında yer alır ve müşterinin proje için doğrudan görünürlüğü olmaz (ancak istenirse, değerlendirmeler müşteriler ile paylaşılabilir). Her müşteri için geçiş hedefi müşterinin aboneliği olacaktır.

Bu yaklaşım, hizmet sağlayıcılarının geçiş bulma ve değerlendirme projelerini hızlı bir şekilde başlatmasını ve bu ilk adımları müşteri aboneliklerinden ve kiracılarından soyutlamanıza olanak sağlar.

Bu modelin iş akışı aşağıdakine benzer olacaktır:

1. Müşteri, [Azure açık eklendi](onboard-customer.md). Azure geçişi ile kullanılacak kimlik için katkıda bulunan yerleşik rolü gereklidir. Bu rolü kullanan bir örnek için, [temsilci-Resource-Management-azmigrate](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management-azmigrate) örnek şablonuna bakın.
1. Belirlenen Kullanıcı Azure portal yönetim kiracısında oturum açar ve Azure geçişi ' ne gider. Bu Kullanıcı, yönetim kiracısına ait olan abonelikte bir [Azure geçişi projesi oluşturur](/migrate/create-manage-projects.md) .
1. Kullanıcı daha sonra [bulma ve değerlendirme için adımları gerçekleştirir](../../migrate/tutorial-discover-vmware.md). Şirket içi VM 'Ler, yönetim kiracısında oluşturulan geçiş projesi içinde keşfedilir ve değerlendirilir ve ardından buradan geçirilir.

   Aynı Hyper-V konağında birden çok müşteriyi yönetiyorsanız, tüm iş yüklerini tek seferde bulabilirsiniz. Müşteriye özgü VM 'Ler aynı grupta seçilebilir, daha sonra bir değerlendirme oluşturulabilir ve hedef hedef olarak uygun müşterinin aboneliği seçilerek geçiş gerçekleştirilebilir. Bulma kapsamını sınırlandırmaya gerek yoktur ve tek bir geçiş projesindeki tüm müşteri iş yüklerine tam bir genel bakış sağlayabilirsiniz.

1. Hazırlık sırasında, iş yüklerini çoğaltmak ve geçirmek için hedef hedef olarak atanan müşteri aboneliğini seçerek geçişe devam edin. Yeni oluşturulan kaynaklar müşteri aboneliğinde mevcut olacaktır, ancak geçiş projesiyle ilgili değerlendirme verileri ve kaynakları, yönetim kiracısında kalır.

NOTE: dağıtım yapmadan önce, parametre dosyasını ortamınızı yansıtacak şekilde değiştirmeniz gerekir https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegated-resource-management-azmigrate

## <a name="partner-recognition-for-customer-migrations"></a>Müşteri geçişleri için iş ortağı tanıma

[Microsoft iş ortağı ağı](https://partner.microsoft.com)bir üyesi olarak, Iş ortağı kimliğinizi, temsilcili müşteri kaynaklarını yönetmek için kullanılan kimlik bilgileriyle bağlantılandırabilirsiniz. Iş ortağı yönetici bağlantısı (PAL) aracılığıyla, Microsoft, geçiş projeleri de dahil olmak üzere, müşteriler için gerçekleştirdiğiniz görevlere bağlı olarak, kuruluşunuza ve Azure tarafından tüketilen gelir üzerinde özellik etkileyebilir.

Daha fazla bilgi için bkz. [İş ortağı kimliğinizi, temsilci atanan kaynaklardaki etkinizi izleyecek şekilde bağlama](partner-earned-credit.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure geçişi](../../migrate/migrate-services-overview.md)hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.

