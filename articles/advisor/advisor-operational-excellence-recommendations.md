---
title: Danışman ile operasyonel Excel 'i geliştirme
description: Azure abonelikleriniz için işlemsel mükemmelliği iyileştirmek için Azure Advisor 'ı kullanın.
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: 036adb7e7d59bd78980c72b210ad41faea277d00
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2020
ms.locfileid: "88258490"
---
# <a name="achieve-operational-excellence-by-using-azure-advisor"></a>Azure Advisor 'ı kullanarak işlemsel mükemmellik elde edin

Azure Advisor 'daki işlemsel mükemmellik önerileri şunları yapmanıza yardımcı olabilir: 
- İşlem ve iş akışı verimliliği.
- Kaynak yönetilebilirliği.
- Dağıtım en iyi uygulamaları. 

Bu önerileri, danışman panosunun **Işletimsel üstün çalışma** sekmesinde edinebilirsiniz.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-problems-affect-you"></a>Azure sorunları sizi etkiliyorsa bildirim almak için Azure hizmet durumu uyarıları oluşturun

Azure hizmeti sorunları sizi etkiliyorsa size bildirilecek olan Azure hizmet durumu uyarılarını ayarlamanızı öneririz. [Azure hizmet durumu](https://azure.microsoft.com/features/service-health/) , bir Azure hizmet sorunundan etkilendiğiniz zaman kişiselleştirilmiş rehberlik ve destek sağlayan ücretsiz bir hizmettir. Danışman, uyarıları yapılandırılmış olmayan ve bunları yapılandırmanızı öneren abonelikleri tanımlar.


## <a name="design-your-storage-accounts-to-prevent-reaching-the-maximum-subscription-limit"></a>Maksimum abonelik sınırına ulaşmasını engellemek için depolama hesaplarınızı tasarlayın

Bir Azure bölgesi, abonelik başına en fazla 250 depolama hesabını destekleyebilir. Bu sınıra ulaştığınızda, bu bölge/abonelik birleşiminde depolama hesapları oluşturamazsınız. Danışman aboneliklerinizi denetler ve sınıra ulaşılmaya yakın olan herhangi bir bölge/abonelik için daha az depolama hesabı tasarlamanızı sağlayacak öneriler sağlar.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>İhtiyacınız olduğunda Azure bulut uzmanlarıyla erişime sahip olduğunuzdan emin olun

İş açısından kritik bir iş yükü çalıştırırken, ihtiyacınız olduğunda teknik desteğe erişebilmek önemlidir. Danışman, destek planına dahil olan teknik desteğe sahip olmayan iş açısından kritik abonelikleri tanımlar. Teknik destek içeren bir seçeneğe yükseltmeyi önerir.

## <a name="delete-and-re-create-your-pool-to-remove-a-deprecated-internal-component"></a>Kullanımdan kaldırılan bir iç bileşeni kaldırmak için havuzunuzu silin ve yeniden oluşturun

Havuzunuz kullanım dışı bir iç bileşen kullanıyorsa, daha iyi kararlılık ve performans için havuzu silin ve yeniden oluşturun.

## <a name="repair-invalid-log-alert-rules"></a>Geçersiz günlük uyarısı kurallarını Onar

Azure Danışmanı, durumu bölümünde geçersiz sorguları belirtilmiş olan uyarı kurallarını algılar. Azure Izleyici 'de günlük uyarı kuralları oluşturabilir ve bunları belirli aralıklarda analiz sorguları çalıştırmak için kullanabilirsiniz. Sorgu sonuçları, uyarıyı tetiklemenin gerekip gerekmediğini belirler. Başvurulan kaynaklarda, tablolarda veya komutlarda yapılan değişiklikler nedeniyle, analiz sorguları zaman içinde geçersiz hale gelebilir. Danışman, otomatik olarak devre dışı bırakılmasını ve Azure 'da kaynaklarınızın kapsamını izlemeyi güvence altına almak için uyarı kuralındaki sorguyu düzeltmenizi önerir. [Uyarı kuralları sorunlarını giderme hakkında daha fazla bilgi edinin.](https://aka.ms/aa_logalerts_queryrepair)

## <a name="use-azure-policy-recommendations"></a>Azure Ilke önerilerini kullanma

Azure Ilkesi, Azure 'da ilke oluşturmak, atamak ve yönetmek için kullanabileceğiniz bir hizmettir. Bu ilkeler, kaynaklarınızda kuralları ve etkileri uygular. Aşağıdaki Azure Ilke önerileri, işlemsel bir yardım almanıza yardımcı olabilir: 

**Etiketleri yönetin.** Bu ilke, bir kaynak oluşturulduğunda veya güncelleştirildiğinde belirtilen etiketi ve değeri ekler veya değiştirir. Bir düzeltme görevi tetikleyerek mevcut kaynakları düzeltebilirsiniz. Bu ilke, kaynak gruplarındaki etiketleri değiştirmez.

**Coğrafi uyumluluk gereksinimlerini zorunlu tutun.** Bu ilke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanıza olanak verir. 

**Dağıtımlar için izin verilen sanal makine SKU 'Larını belirtin.** Bu ilke, kuruluşunuzun dağıtabileceği bir sanal makine SKU'ları kümesi belirtmenizi sağlar.

***Yönetilen disk kullanmayan VM 'Leri denetlemeyi*zorunlu tutun.**

***Kaynak gruplarından etiket devralmayı*etkinleştirin.** Bu ilke, bir kaynak oluşturulduğunda veya güncelleştirildiğinde üst kaynak grubunda belirtilen etiketi ve değeri ekler veya değiştirir. Bir düzeltme görevi tetikleyerek mevcut kaynakları düzeltebilirsiniz.

## <a name="no-validation-environment-enabled"></a>Etkin doğrulama ortamı yok
Azure Advisor, geçerli abonelikte etkin bir doğrulama ortamınız olmadığını belirler. Konak havuzlarınızı oluştururken, \" \" \" Özellikler sekmesinde doğrulama ortamı için Hayır ' ı seçmiş olursunuz \" . Doğrulama ortamı etkinleştirilmiş en az bir konak havuzu olması, olası sorunları erken algılamada Windows sanal masaüstü hizmeti dağıtımları aracılığıyla iş sürekliliği sağlar. [Daha fazla bilgi](https://docs.microsoft.com/azure/virtual-desktop/create-validation-host-pool)

## <a name="ensure-production-non-validation-environment-to-benefit-from-stable-functionality"></a>Kararlı işlevlerden faydalanmak için üretime (doğrulama olmayan) sahip bir ortam sağlayın
Azure Advisor, ana bilgisayar havuzlarınızın çok fazla sayıda doğrulama ortamı etkinleştirildiğini algılar. Doğrulama ortamlarının amacını en iyi şekilde sunması için, doğrulama ortamında en az bir tane, ancak ana bilgisayar havuzlarınızın yarısından daha fazlası olmalıdır. Doğrulama ortamı etkinken ve BT 'nin devre dışı bırakıldığı ana bilgisayar havuzlarınız arasında sağlıklı bir denge sahibi olmak için, Windows sanal masaüstü 'nün belirli güncelleştirmelerle birlikte sunduğu çok aşamalı dağıtımların avantajlarından en iyi şekilde faydalanabileceksiniz. Bu sorunu onarmak için, konak havuzunuzun özelliklerini açın ve \" \" \" doğrulama ortamı ayarında ileri ' yi seçin \" .

## <a name="enable-traffic-analytics-to-view-insights-into-traffic-patterns-across-azure-resources"></a>Azure kaynakları genelinde trafik desenlerinin öngörülerini görüntülemek için Trafik Analizi etkinleştirin
Trafik Analizi, Azure 'da Kullanıcı ve uygulama etkinliğine görünürlük sağlayan bulut tabanlı bir çözümdür. Trafik Analizi, trafik akışına Öngörüler sağlamak için ağ Izleyicisi ağ güvenlik grubu (NSG) akış günlüklerini analiz eder. Trafik Analizi ile Azure ve Azure olmayan dağıtımlar genelinde en önemli noktaları görüntüleyebilir, ortamınızda açık bağlantı noktalarını, protokolleri ve kötü amaçlı akışları araştırabilir ve performans için ağ dağıtımınızı en iyi duruma getirebilirsiniz. Akış günlüklerini 10 dakika ve 60 dakikalık işleme aralıklarında işleyebilir ve trafiğiniz üzerinde daha hızlı analiz sağlayabilirsiniz. Azure kaynaklarınız için Trafik Analizi etkinleştirmek iyi bir uygulamadır. 


## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz.:
* [Advisor 'a giriş](advisor-overview.md)
* [Kullanmaya başlama](advisor-get-started.md)
* [Danışman maliyet önerileri](advisor-cost-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor güvenilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman REST API](/rest/api/advisor/)
