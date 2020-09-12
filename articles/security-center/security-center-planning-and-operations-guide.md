---
title: Güvenlik Merkezi planlama ve Işlemler Kılavuzu
description: Bu belge, Azure Güvenlik Merkezi'ni benimsemeden önce planlama ve günlük işlemlere ilişkin dikkat edilmesi gerekenler konusunda yardımcı olur.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 45d3ec8dc5d819464046e40bab22491a4bccde63
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89461332"
---
# <a name="planning-and-operations-guide"></a>Planlama ve işlemler kılavuzu
Bu kılavuz, Azure Güvenlik Merkezi 'ni kullanmayı planlayan bilgi teknolojisi (BT) uzmanları, BT mimarları, bilgi güvenliği analistleri ve bulut yöneticileri içindir.


## <a name="planning-guide"></a>Planlama kılavuzu
Bu kılavuzda, kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline göre güvenlik merkezi kullanımını iyileştirmek için izleyebileceğiniz görevler ele alınmaktadır. Güvenlik Merkezi'nin tüm avantajlarından yararlanabilmek için kurumunuzdaki farklı kişilerin veya ekiplerin güvenli geliştirmenin yanı sıra işlem, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullandığının anlaşılması oldukça önemlidir. Güvenlik Merkezi'ni kullanmayı planlarken dikkate alınması gereken temel alanlar şunlardır:

* Güvenlik Rolleri ve Erişim Denetimleri
* Güvenlik İlkeleri ve Öneriler
* Veri Koleksiyonu ve Storage
* Azure dışı kaynakları ekleme
* Devam Eden Güvenlik İzleme
* Olay Yanıtı

Sonraki bölümde, gereksinimlerinize bağlı olarak bu alanların her birini nasıl planlayacağınızı ve bu önerileri nasıl uygulayacağınızı öğreneceksiniz.


> [!NOTE]
> Tasarlama ve planlama aşamasında da faydalı olabilecek sık sorulan soruların bir listesi için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](faq-general.md).

## <a name="security-roles-and-access-controls"></a>Güvenlik rolleri ve erişim denetimleri
Kuruluşunuzun büyüklüğüne ve yapısına bağlı olarak birçok kişi ve ekip, güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir. Aşağıdaki diyagramda, hayali kişiler ile bunların rollerinin ve güvenlik sorumluluklarının bir örneğini bulabilirsiniz:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Güvenlik Merkezi, bu çok çeşitli sorumlulukları karşılamak için kişileri etkinleştirir. Örneğin:

**Cem (İş Yükü Sahibi)**

* Bir bulut iş yükünü ve onunla ilgili kaynakları yönetme
* Korumaları şirket güvenlik ilkesiyle uyumlu bir şekilde uygulamak ve sürdürmekle sorumlu

**Emel (CISO/CIO)**

* Şirket güvenliğinin tüm boyutlarından sorumlu
* Bulut iş yüklerinin tamamında şirketin güvenlik duruşunu anlamak istiyor
* Önemli saldırı ve risklerden haberdar olması gerekiyor

**David (BT Güvenliği)**

* Uygun korumaların uygulanmakta olduğundan emin olmak için şirketin güvenlik ilkelerini ayarlar
* İlkelerle uyumluluğu izler
* Yöneticiler veya denetçiler için raporlar oluşturur

**Zehra (Güvenlik İşlemleri)**

* Güvenlik uyarılarını 7/24 izler ve bunlara yanıt verir
* Bulut İş Yükü Sahibi veya BT Güvenlik Analistine İletir

**Salih (Güvenlik Analisti)**

* Atakları araştırır
* Bulut İş Yükü Sahibi ile birlikte çalışarak düzeltme uygulama

Güvenlik Merkezi, Azure 'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Azure rol tabanlı erişim denetimi 'NI (Azure RBAC)](../role-based-access-control/role-assignments-portal.md)kullanır. Bir kullanıcı Güvenlik Merkezi’ni açtığında, yalnızca erişimi olan kaynaklarla ilişkili bilgileri görüntüleyebilir. Bu da bir kaynağın ait olduğu abonelik veya kaynak grubu için kullanıcıya Sahip, Katkıda Bulunan veya Okuyucu rolünün atandığı anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: Bu role ait kullanıcı; öneriler, uyarılar, ilke ve sistem durumunu içeren Güvenlik Merkezi yapılandırmalarını yalnızca görüntüleyebilir, herhangi bir değişiklik yapamaz.
- **Güvenlik yöneticisi**: Güvenlik okuyucusu ile aynıdır, ancak aynı zamanda güvenlik ilkesini güncelleştirebilir ve öneriler ile uyarıları kapatabilir.

Yukarıda açıklanan Güvenlik Merkezi rolleri, Azure’un Depolama, Web ve Mobil veya Nesnelerin İnterneti gibi diğer hizmet alanlarına erişemez.

Önceki diyagramda açıklanan kişiler kullanıldığında aşağıdaki RBAC gerekli olur:

**Cem (İş Yükü Sahibi)**

* Kaynak grubu sahibi/katkıda bulunan

**Emel (CISO/CIO)**

* Abonelik sahibi/katkıda bulunan veya Güvenlik Yöneticisi

**David (BT Güvenliği)**

* Abonelik sahibi/katkıda bulunan veya Güvenlik Yöneticisi

**Zehra (Güvenlik İşlemleri)**

* Uyarıları görüntülemek için Abonelik Okuyucusu veya Güvenlik Okuyucusu
* Abonelik sahibi/katkıda bulunan veya güvenlik yöneticisi uyarıları kapatmak için gereklidir

**Salih (Güvenlik Analisti)**

* Uyarıları görüntülemek için Abonelik Okuyucusu
* Abonelik sahibi/bu katılımcı uyarıları kapatmak için gereklidir
* Çalışma alanına erişim gerekli olabilir

Dikkate alınması gereken bazı diğer önemli bilgiler:

* Güvenlik ilkesini yalnızca abonelik Sahipleri/Katkıda Bulunanları ve Güvenlik Yöneticileri düzenleyebilir.
* Yalnızca abonelik ve kaynak grubu Sahipleri ve Katkıda bulunanları bir kaynak için güvenlik önerilerini uygulayabilir.

Güvenlik Merkezi için RBAC kullanarak erişim denetimini planlarken Güvenlik Merkezi'ni kuruluşunuzdaki hangi kişilerin kullanacağını anladığınızdan emin olun. Ayrıca, bunların hangi türde görevler gerçekleştireceğini anlayın ve daha sonra RBAC’yi buna göre yapılandırın.

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, yalnızca kaynakların güvenlik durumu hakkındaki bilgileri görüntülemesi gereken ancak eyleme geçmeyecek olan kullanıcıların (örneğin, önerileri uygulamak veya ilkeleri düzeltmek), Okuyucu rolüne atanmaları gerekir.
>
>

## <a name="security-policies-and-recommendations"></a>Güvenlik ilkeleri ve öneriler
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Güvenlik Merkezi'nde Azure aboneliğinize yönelik ilkeleri tanımlayabilir, bunları iş yükü türüne veya veri gizlilik düzeyine göre ayarlayabilirsiniz.

Güvenlik Merkezi ilkeleri aşağıdaki bileşenleri içerir:
- [Veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection): Aracı sağlama ve veri toplama ayarları.
- [Güvenlik ilkesi](https://docs.microsoft.com/azure/security-center/security-center-policies): hangi denetimlerin güvenlik merkezi tarafından izleneceğini ve önerildiğini belirleyen bir [Azure ilkesidir](../governance/policy/overview.md) veya yeni tanımlar oluşturmak, ek ilkeler tanımlamak ve yönetim grupları arasında Ilkeler atamak için Azure ilkesi 'ni kullanın.
- [E-posta bildirimleri](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details): Güvenlik ilgili kişileri ve bildirim ayarları.
- [Fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing): Kapsam dahilindeki kaynaklar (abonelikler, kaynak grupları ve çalışma alanları için belirtilebilir) için kullanılabilecek olan Güvenlik Merkezi özelliklerini belirleyen ücretsiz veya standart fiyatlandırma katmanı.

> [!NOTE]
> Güvenlik ilgili kişisi belirtmeniz bir güvenlik olayı ortaya çıktığında Azure'un kuruluşunuzdaki doğru kişiyle iletişime geçmesini sağlayacaktır. Bu öneriyi etkinleştirme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) konusunu okuyun.

### <a name="security-policies-definitions-and-recommendations"></a>Güvenlik ilkelerinin tanımları ve öneriler
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. İlkeyi Güvenlik Merkezi'nde düzenleyebilir veya Azure ilke yönetimini kullanarak yeni tanım oluşturabilir, ek ilke tanımlayabilir, ilkeleri farklı Yönetim Gruplarına (kuruluşun tamamı, tek bir iş birimi vs. olabilir) atayabilir ve bu kapsamlarda bu ilkelere uyum sağlanıp sağlanmadığını izleyebilirsiniz.

Güvenlik ilkelerini yapılandırmadan önce her bir [güvenlik önerisini](https://docs.microsoft.com/azure/security-center/security-center-recommendations) gözden geçirip bu ilkelerin sahip olduğunuz çeşitli abonelikler ve kaynak grupları için uygun olup olmadığını belirleyin. Güvenlik Önerilerini ele almak için hangi eylemlerde bulunulacağını ve kuruluşunuzda yeni önerileri izlemekten ve gerekli adımların atılmasından kimin sorumlu olacağını anlamak da önemlidir.

## <a name="data-collection-and-storage"></a>Veri koleksiyonu ve depolama
Azure Güvenlik Merkezi, Azure Izleyici hizmeti tarafından kullanılan aracı, sanal makinelerinizden güvenlik verilerini toplamak için Log Analytics aracısını kullanır. Bu aracıdan [toplanan veriler](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection), Log Analytics çalışma alanlarınızda depolanır.

### <a name="agent"></a>Aracı

Güvenlik ilkesinde otomatik sağlama etkinleştirildiğinde, desteklenen tüm Azure VM 'lerine ve oluşturulan tüm yeni sanal makinelere Log Analytics Aracısı ( [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) veya [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)için) yüklenir. VM veya bilgisayarda Log Analytics Aracısı zaten yüklüyse, Azure Güvenlik Merkezi geçerli yüklü aracıyı kullanacaktır. Aracının işlemi, bir arada olmayan ve VM performansı üzerinde çok az etkiye sahip olacak şekilde tasarlanmıştır.

Windows için Log Analytics Aracısı, 443 numaralı TCP bağlantı noktasını kullanmalıdır. Daha fazla bilgi için [Sorun giderme makalesine](security-center-troubleshooting-guide.md) bakın.

Belirli bir noktada Veri Koleksiyonu'nu devre dışı bırakmak isterseniz koleksiyonu güvenlik ilkesinden kapatabilirsiniz. Ancak, Log Analytics Aracısı diğer Azure yönetim ve izleme hizmetleri tarafından kullanıldığından, güvenlik merkezi 'nde veri toplamayı kapattığınızda aracı otomatik olarak kaldırılmaz. Gerekirse aracıyı el ile kaldırabilirsiniz.

> [!NOTE]
> Desteklenen VM 'lerin listesini bulmak için, [Azure Güvenlik Merkezi sık sorulan sorular (SSS)](faq-vms.md)makalesini okuyun.

### <a name="workspace"></a>Çalışma alanı

Çalışma alanı, veri kapsayıcısı olarak görev yapan bir Azure kaynağıdır. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Log Analytics aracısından toplanan veriler (Azure Güvenlik Merkezi adına), sanal makinenin coğrafi bölge 'sini hesaba katarak Azure aboneliğinizle ilişkili mevcut bir Log Analytics çalışma alanında veya yeni çalışma alanlarında depolanır.

Azure portalında, Azure Güvenlik Merkezi tarafından oluşturulanlar dahil olmak üzere Log Analytics çalışma alanlarınızın listesine göz atabilirsiniz. Yeni çalışma alanları için bir ilgili kaynak grubu oluşturulur. İkisi de şu adlandırma kuralını izler:

* Çalışma Alanı: *DefaultWorkspace-[abonelik-kimliği]-[bölge]*
* Kaynak grubu: *Defaultresourcegroup-[coğrafi]*

Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanları için veriler 30 gün boyunca tutulur. Mevcut çalışma alanları için elde tutma süresi, çalışma alanının fiyatlandırma katmanını temel alır. İsterseniz var olan bir çalışma alanını kullanabilirsiniz.

> [!NOTE]
> Microsoft, bu verilerin gizlilik ve güvenliğini korumak için önemli taahhütlerde bulunur. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. Veri işleme ve gizlilik hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Veri Güvenliği](security-center-data-security.md) makalesini okuyun.
>

## <a name="onboarding-non-azure-resources"></a>Azure dışı kaynakları ekleme

Güvenlik Merkezi, Azure dışı bilgisayarların güvenlik durumunu izleyebilir ancak öncelikle bu kaynakları eklemeniz gerekir. Azure dışı kaynakları nasıl ekleyeceğiniz hakkında daha fazla bilgi almak için [Gelişmiş güvenlik için Azure Güvenlik Merkezi Standart'a kaynak ekleme](https://docs.microsoft.com/azure/security-center/security-center-onboarding#onboard-non-azure-computers) konusuna bakın.

## <a name="ongoing-security-monitoring"></a>Devam eden güvenlik izleme
Güvenlik Merkezi önerilerinin ilk yapılandırması ve uygulamasından sonraki adım, Güvenlik Merkezi işlem süreçlerini dikkate almaktır.

Güvenlik Merkezine Genel Bakış, tüm Azure kaynaklarınızın ve bağladığınız Azure dışı kaynakların birleştirilmiş görünümünü sunar. Aşağıdaki örnekte ilgilenilmesi gereken birçok soruna sahip bir ortam gösterilmektedir:

![pano](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig11.png)

> [!NOTE]
> Güvenlik Merkezi normal işlem yordamlarınıza müdahale etmez, dağıtımlarınızı pasif olarak izler ve etkinleştirdiğiniz güvenlik ilkelerine bağlı olarak öneriler sağlar.

Geçerli Azure ortamınız için Güvenlik Merkezi 'ni ilk kez kullanmaya çalıştığınızda, **öneriler** sayfasında yapılabilecek tüm önerileri gözden geçirdiğinizden emin olun.

Tehdit zekası seçeneğini ziyaret etmeyi günlük işlemlerinizin bir parçası haline getirin. Burada belirli bir bilgisayarın bir botnetin parçası olup olmadığını belirleme gibi ortamdaki güvenlik tehditlerini belirleyebilirsiniz.

### <a name="monitoring-for-new-or-changed-resources"></a>Yeni veya değiştirilmiş kaynakları izleme

Azure ortamlarının çoğu dinamik, düzenli olarak oluşturulan kaynaklar, artırma veya azaltma, yeniden yapılandırılmış ve değiştirilmiş kaynakları içeren dinamiktir. Güvenlik Merkezi, bu yeni kaynakların güvenlik durumuyla ilgili görünürlüğe sahip olduğunuzdan emin olmanıza yardımcı olur.

Azure ortamınıza yeni kaynaklar (VM'ler, SQL DB'leri) eklediğinizde Güvenlik Merkezi otomatik olarak bu kaynakları keşfeder ve güvenliklerini izlemeye başlar. Buna PaaS web rolleri ve çalışan rolleri de dahildir. [Güvenlik Ilkesinde](tutorial-security-policy.md)veri toplama etkinse, sanal makineleriniz için ek izleme özellikleri otomatik olarak etkinleştirilir.

Ayrıca, güvenlik riskleri oluşturan, önerilen taban çizgileri ve güvenlik uyarılarından oluşan yapılandırma değişiklikleri için de düzenli olarak mevcut kaynakları izlemeniz gerekir. 

### <a name="hardening-access-and-applications"></a>Erişimi ve uygulamaları sağlamlaştırma

Güvenlik sürecinizin bir parçası olarak VM erişimini kısıtlamak ve VM'ler üzerinde çalışan uygulamaları denetlemek için önlemler de almanız gerekir. Azure VM'lerinize gelen trafiği engelleyerek saldırı riskini azaltabilir ve aynı zamanda gerekli olduğunda VM'lere kolayca bağlanabilirsiniz. Sanal makinelerinize erişimi sağlamlaştırma için [tam ZAMANıNDA VM](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) erişimi özelliğini kullanın.

Azure 'da bulunan sanal makinelerinizdeki hangi uygulamaların çalıştırılacağını sınırlamak için [Uyarlamalı uygulama denetimleri](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application) kullanabilirsiniz. Diğer avantajların yanı sıra bu, VM 'lerinizi kötü amaçlı yazılımlara karşı korumanıza yardımcı olur. Güvenlik Merkezi, makine öğrenimini kullanarak beyaz listeleme kuralları oluşturmanıza yardımcı olması için VM 'de çalışan işlemlerin dökümünü yapar.


## <a name="incident-response"></a>Olay yanıtı
Güvenlik Merkezi, tehditler oluşunca algılar ve sizi tehditlere karşı uyarır. Kuruluşlar, yeni güvenlik uyarılarını izlemeli ve gerekirse daha fazla araştırmak veya saldırıyı düzeltmek için eyleme geçmelidir. Güvenlik Merkezi tehdit korumasının nasıl çalıştığı hakkında daha fazla bilgi için, [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiğini](security-center-alerts-overview.md#detect-threats)okuyun.

Bu makale kendi olay yanıtı planınızı oluşturmanıza yardımcı olmaya yönelik bir amaç olmasa da, bulut yaşam döngüsünde olay yanıtı aşamaları için temel olarak Microsoft Azure Güvenlik yanıtı kullanacağız. Aşamalar aşağıdaki diyagramda gösterilmiştir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Kendi planınızı oluşturmanıza yardımcı olması için National Institute of Standards and Technology'nin (NIST) [Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) belgesini kullanabilirsiniz.
>

Güvenlik Merkezi Uyarılarını aşağıdaki aşamalar sırasında kullanabilirsiniz:

* **Algılama**: bir veya daha fazla kaynakta şüpheli bir etkinliği tanımlayın.
* **Değerlendirme**: şüpheli etkinlik hakkında daha fazla bilgi edinmek için ilk değerlendirmeyi gerçekleştirin.
* **Tanılama**: sorunu çözmeye yönelik teknik yordamı yürütmek için düzeltme adımlarını kullanın.

Her Güvenlik Uyarısı, saldırının yapısını daha iyi anlamanız ve olası risk azaltmalarını önermek için kullanılabilecek bilgiler sağlar. Ayrıca bazı uyarılar, daha fazla bilgi veya Azure'daki diğer bilgi kaynakları için bağlantılar sağlar. Verilen bilgileri daha fazla araştırma ya da risk azaltma için kullanabilir ve ayrıca çalışma alanınızda depolanmış güvenlikle ilgili verileri arayabilirsiniz.

Aşağıdaki örnek, gerçekleşmekte olan şüpheli bir RDP etkinliğini gösterir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Bu sayfa, saldırının gerçekleştiği zaman, kaynak ana bilgisayar adı, hedef VM ile ilgili ayrıntıları gösterir ve ayrıca öneri adımları sunar. Bazı durumlarda, saldırının kaynak bilgileri boş olabilir. Bu türden bir davranış hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Uyarıları'nda Eksik Kaynak Bilgileri](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

Bu sayfadan bir araştırma başlatarak saldırının zaman çizelgesi, gerçekleşme şekli, gizliliği bozulmuş olabilecek sistemler ve kullanılan kimlik bilgileri hakkında bilgi edinebilir, saldırı zincirinin tamamını grafiklerle görebilirsiniz.

Güvenliği aşılmış sistemi tanımladıktan sonra, daha önce oluşturulmuş bir [Iş akışı Otomasyonu](workflow-automation.md) çalıştırabilirsiniz. Bunlar, bir uyarı tarafından tetiklendiğinde Güvenlik Merkezi 'nden yürütülebilecek yordamlar topluluğudur.

[Azure Güvenlik Merkezi 'Nden yararlanma & bir olay yanıtı videosu için Microsoft Operations Management Suite](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) , bu aşamaların her birinde güvenlik merkezi 'nin nasıl kullanılabileceğini anlamanıza yardımcı olabilecek bazı gösteriler görebilirsiniz.

> [!NOTE]
> [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) hakkında daha fazla bilgi için bkz. Güvenlik Merkezi özelliklerini kullanarak olay yanıtlama sürecinizde size yardımcı olun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi benimsemeyi nasıl planlayacağınızı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi 'Nde güvenlik sistem durumu izleme](security-center-monitoring.md) — Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi hakkında SSS](faq-general.md) — hizmeti kullanma hakkında sık sorulan soruları bulun.
* [Azure Güvenlik blogu](https://docs.microsoft.com/archive/blogs/azuresecurity/) : Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini bulun.
