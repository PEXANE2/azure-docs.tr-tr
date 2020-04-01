---
title: Güvenlik Merkezi Planlama ve İşlemler Kılavuzu | Microsoft Belgeleri
description: Bu belge, Azure Güvenlik Merkezi'ni benimsemeden önce planlama ve günlük işlemlere ilişkin dikkat edilmesi gerekenler konusunda yardımcı olur.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 46994413ba765e18a826eebfe85a38bb65efc749
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435610"
---
# <a name="azure-security-center-planning-and-operations-guide"></a>Azure Güvenlik Merkezi planlama ve işlemler kılavuzu
Bu kılavuz, bilgi teknolojisi (BT) uzmanları, BT mimarları, bilgi güvenliği analistleri ve Azure Güvenlik Merkezi'ni kullanmayı planlayan bulut yöneticileri içindir.


## <a name="planning-guide"></a>Planlama Kılavuzu
Bu kılavuz, kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline göre Güvenlik Merkezi kullanımınızı optimize etmek için izleyebileceğiniz görevleri kapsar. Güvenlik Merkezi'nin tüm avantajlarından yararlanabilmek için kurumunuzdaki farklı kişilerin veya ekiplerin güvenli geliştirmenin yanı sıra işlem, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullandığının anlaşılması oldukça önemlidir. Güvenlik Merkezi'ni kullanmayı planlarken dikkate alınması gereken temel alanlar şunlardır:

* Güvenlik Rolleri ve Erişim Denetimleri
* Güvenlik İlkeleri ve Öneriler
* Veri Koleksiyonu ve Storage
* Devam eden Azure dışı kaynaklar
* Devam Eden Güvenlik İzleme
* Olay Yanıtı

Sonraki bölümde, gereksinimlerinize bağlı olarak bu alanların her birini nasıl planlayacağınızı ve bu önerileri nasıl uygulayacağınızı öğreneceksiniz.


> [!NOTE]
> Tasarlama ve planlama aşamasında da faydalı olabilecek sık sorulan soruların bir listesi için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](faq-general.md).

## <a name="security-roles-and-access-controls"></a>Güvenlik rolleri ve erişim denetimleri
Kuruluşunuzun büyüklüğüne ve yapısına bağlı olarak birçok kişi ve ekip, güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir. Aşağıdaki diyagramda, hayali kişiler ile bunların rollerinin ve güvenlik sorumluluklarının bir örneğini bulabilirsiniz:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Güvenlik Merkezi, bu çok çeşitli sorumlulukları karşılamak için kişileri etkinleştirir. Örnek:

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

Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır. Bir kullanıcı Güvenlik Merkezi’ni açtığında, yalnızca erişimi olan kaynaklarla ilişkili bilgileri görüntüleyebilir. Bu da bir kaynağın ait olduğu abonelik veya kaynak grubu için kullanıcıya Sahip, Katkıda Bulunan veya Okuyucu rolünün atandığı anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: Bu role ait kullanıcı; öneriler, uyarılar, ilke ve sistem durumunu içeren Güvenlik Merkezi yapılandırmalarını yalnızca görüntüleyebilir, herhangi bir değişiklik yapamaz.
- **Güvenlik yöneticisi**: Güvenlik okuyucusu ile aynıdır, ancak aynı zamanda güvenlik ilkesini güncelleştirebilir ve öneriler ile uyarıları kapatabilir.

Yukarıda açıklanan Güvenlik Merkezi rolleri, Azure’un Depolama, Web ve Mobil veya Nesnelerin İnterneti gibi diğer hizmet alanlarına erişemez.

Önceki diyagramda açıklanan kişiler kullanıldığında aşağıdaki RBAC gerekli olur:

**Cem (İş Yükü Sahibi)**

* Kaynak Grubu Sahibi/Katılımcısı

**Emel (CISO/CIO)**

* Abonelik Sahibi/Katılımcı veya Güvenlik Yöneticisi

**David (BT Güvenliği)**

* Abonelik Sahibi/Katılımcı veya Güvenlik Yöneticisi

**Zehra (Güvenlik İşlemleri)**

* Uyarıları görüntülemek için Abonelik Okuyucusu veya Güvenlik Okuyucusu
* Uyarıları kapatmak için Abonelik Sahibi/Katılımcısı veya Güvenlik Yöneticisi

**Salih (Güvenlik Analisti)**

* Uyarıları görüntülemek için Abonelik Okuyucusu
* Uyarıları kapatmak için Abonelik Sahibi/Katılımcısı
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
- [Güvenlik ilkesi:](https://docs.microsoft.com/azure/security-center/security-center-policies)Güvenlik Merkezi tarafından hangi denetimlerin izlendiğini ve tavsiye edildiğini belirleyen veya yeni tanımlar oluşturmak, ek ilkeler tanımlamak ve yönetim grupları arasında ilkeler atamak için Azure İlkesi'ni kullanan bir Azure İlkesidir. [Azure Policy](../governance/policy/overview.md)
- [E-posta bildirimleri](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details): Güvenlik ilgili kişileri ve bildirim ayarları.
- [Fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing): Kapsam dahilindeki kaynaklar (abonelikler, kaynak grupları ve çalışma alanları için belirtilebilir) için kullanılabilecek olan Güvenlik Merkezi özelliklerini belirleyen ücretsiz veya standart fiyatlandırma katmanı.

> [!NOTE]
> Güvenlik ilgili kişisi belirtmeniz bir güvenlik olayı ortaya çıktığında Azure'un kuruluşunuzdaki doğru kişiyle iletişime geçmesini sağlayacaktır. Bu öneriyi etkinleştirme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) konusunu okuyun.

### <a name="security-policies-definitions-and-recommendations"></a>Güvenlik ilkelerinin tanımları ve öneriler
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. İlkeyi Güvenlik Merkezi'nde düzenleyebilir veya Azure ilke yönetimini kullanarak yeni tanım oluşturabilir, ek ilke tanımlayabilir, ilkeleri farklı Yönetim Gruplarına (kuruluşun tamamı, tek bir iş birimi vs. olabilir) atayabilir ve bu kapsamlarda bu ilkelere uyum sağlanıp sağlanmadığını izleyebilirsiniz.

Güvenlik ilkelerini yapılandırmadan önce her bir [güvenlik önerisini](https://docs.microsoft.com/azure/security-center/security-center-recommendations) gözden geçirip bu ilkelerin sahip olduğunuz çeşitli abonelikler ve kaynak grupları için uygun olup olmadığını belirleyin. Güvenlik Önerilerini ele almak için hangi eylemlerde bulunulacağını ve kuruluşunuzda yeni önerileri izlemekten ve gerekli adımların atılmasından kimin sorumlu olacağını anlamak da önemlidir.

## <a name="data-collection-and-storage"></a>Veri koleksiyonu ve depolama
Azure Güvenlik Merkezi, sanal makinelerinizden güvenlik verileri toplamak için Azure Monitor hizmeti tarafından kullanılan aracının aynısı olan Log Analytics aracısını kullanır. Bu aracıdan [toplanan veriler](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection), Log Analytics çalışma alanlarınızda depolanır.

### <a name="agent"></a>Aracı

Güvenlik ilkesinde otomatik sağlama etkinleştirildiğinde, Log Analytics aracısı [(Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) veya [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)için) desteklenen tüm Azure VM'lerine ve oluşturulan yeni bilgisayarlara yüklenir. VM veya bilgisayarda Log Analytics aracısı zaten yüklüyse, Azure Güvenlik Merkezi geçerli yüklü aracıdan yararlanır. Aracının işlemi müdahaleci olmayacak şekilde tasarlanmıştır ve VM performansı üzerinde çok az etki yapar.

Windows için Log Analytics aracısı, TCP port 443'ün kullanılmasını gerektirir. Daha fazla bilgi için [Sorun giderme makalesine](security-center-troubleshooting-guide.md) bakın.

Belirli bir noktada Veri Koleksiyonu'nu devre dışı bırakmak isterseniz koleksiyonu güvenlik ilkesinden kapatabilirsiniz. Ancak, Log Analytics aracısı diğer Azure yönetim ve izleme hizmetleri tarafından kullanılabildiği için, Güvenlik Merkezi'nde veri toplamayı kapattığınızda aracı otomatik olarak kaldırılmaz. Gerekirse aracıyı el ile kaldırabilirsiniz.

> [!NOTE]
> Desteklenen VM'lerin listesini bulmak için [Azure Güvenlik Merkezi'ni sık sorulan soruları (SSS)](faq-vms.md)okuyun.

### <a name="workspace"></a>Çalışma alanı

Çalışma alanı, veri kapsayıcısı olarak görev yapan bir Azure kaynağıdır. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Log Analytics temsilcisinden (Azure Güvenlik Merkezi adına) toplanan veriler, VM'nin Geo'su dikkate alınarak Azure aboneliğinizle ilişkili mevcut bir Log Analytics çalışma alanında veya yeni bir çalışma alanı(lar) içinde depolanır.

Azure portalında, Azure Güvenlik Merkezi tarafından oluşturulanlar dahil olmak üzere Log Analytics çalışma alanlarınızın listesine göz atabilirsiniz. Yeni çalışma alanları için bir ilgili kaynak grubu oluşturulur. İkisi de şu adlandırma kuralını izler:

* Çalışma Alanı: *DefaultWorkspace-[abonelik-kimliği]-[bölge]*
* Kaynak Grubu: *VarsayılanKaynak Grubu-[geo]*

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

Geçerli Azure ortamınız için Güvenlik Merkezi’ni kullanmayı ilk kez seçtiğinizde, **Öneriler** kutucuğunda yer alan veya kaynak başına (**İşlem**, **Ağ**, **Depolama ve Veriler**, **Uygulama**) sunulan tüm önerileri gözden geçirdiğinizden emin olun.

Tüm önerilere değindikten sonra değinilen tüm kaynaklar için **Önleme** bölümünün yeşil olması gerekir. Yalnızca kaynak güvenlik durumu ve öneriler kutucuklarındaki değişikliklere göre eyleme geçeceğiniz için devam eden izleme bu noktada daha kolay olur.

**Algılama** bölümü daha reaktiftir; bunlar, ya şu anda var olan veya geçmişte oluşan ya da Güvenlik Merkezi denetimleri ile 3. taraf sistemleri tarafından algılanan sorunlarla ilgili uyarılardır. Güvenlik Uyarıları döşemesi, her gün bulunan uyarı sayısını ve bunların farklı önem kategorileri (düşük, orta, yüksek) arasındaki dağılımlarını temsil eden çubuk grafikleri gösterir. Güvenlik Uyarıları hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve uyarılara yanıt verme](security-center-managing-and-responding-alerts.md)

[Tehdit zekası](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) seçeneğini ziyaret etmeyi günlük işlemlerinizin bir parçası haline getirin. Burada belirli bir bilgisayarın bir botnetin parçası olup olmadığını belirleme gibi ortamdaki güvenlik tehditlerini belirleyebilirsiniz.

### <a name="monitoring-for-new-or-changed-resources"></a>Yeni veya değiştirilmiş kaynakları izleme
Azure ortamlarının çoğu dinamiktir ve kaynaklar düzenli olarak oluşturulur, yukarı veya aşağı döndürülmez, yeniden yapılandırılır ve değiştirilir. Güvenlik Merkezi, bu yeni kaynakların güvenlik durumuyla ilgili görünürlüğe sahip olduğunuzdan emin olmanıza yardımcı olur.

Azure ortamınıza yeni kaynaklar (VM'ler, SQL DB'leri) eklediğinizde Güvenlik Merkezi otomatik olarak bu kaynakları keşfeder ve güvenliklerini izlemeye başlar. Buna PaaS web rolleri ve çalışan rolleri de dahildir. [Güvenlik İlkesi'nde](tutorial-security-policy.md)Veri Toplama etkinleştirilirse, sanal makineleriniz için otomatik olarak ek izleme özellikleri etkinleştirilir.

![Temel alanlar](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Sanal makineler için **Kaynak Güvenliği Hijyeni** bölümü altında & **uygulamaları bilgi**işlem'i tıklatın. Verileri etkinleştirme veya ilgili öneriler hakkında tüm sorunlar **Genel Bakış** sekmesinde ve **İzleme Önerileri** bölümünde gösterilir.
2. Yeni kaynak için (eğer varsa) hangi güvenlik risklerinin tanımlandığını görmek için **Öneriler**'i görüntüleyin.
3. Ortamınıza yeni VM'ler eklendiğinde, başlangıçta yalnızca işletim sisteminin yüklenmesi durumu çok yaygındır. Bu VM'ler tarafından kullanılacak diğer uygulamaları dağıtmak için kaynak sahibine bir miktar süre gerekebilir.  İdeal olarak, bu iş yükünün son amacını bilmeniz gerekir. Bu iş yükü bir Uygulama Sunucusu mu olacak? Bu yeni iş yükünün ne olacağına bağlı olarak, bu iş akışındaki üçüncü adım olan uygun **Güvenlik İlkesi**'ni etkinleştirebilirsiniz.
4. Azure ortamınıza yeni kaynaklar eklendikçe, **Güvenlik Uyarıları** döşemesinde yeni uyarılar görünebilir. Bu döşemede yeni uyarılar arayın ve önerileri izleyin.

Güvenlik riskleri oluşturmuş, önerilen taban satırlarından sürüklenen yapılandırma değişiklikleri ve güvenlik uyarıları için varolan kaynakları da düzenli olarak izlemeniz gerekir. Güvenlik Merkezi Panosu'nda başlayın. Oradan, tutarlı bir temelde gözden geçirmek için üç ana alanlar var.

![İşlemler](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. **Önleme** bölüm paneli, temel kaynaklarınıza hızlı erişim sağlar. İşlem, Ağ, Depolama ve veriler ile Uygulamalar’ı izlemek için bu seçeneği kullanın.
2. **Öneriler** paneli Güvenlik Merkezi önerilerini gözden geçirmenizi sağlar. Devam eden izlemeniz sırasında, ilk Güvenlik Merkezi kurulumundaki tüm önerileri ele aldığınızdan beri normal olan günlük olarak önerileriniz olmadığını görebilirsiniz. Bundan dolayı bu bölümde her gün yeni bilgilere sahip olmayabilirsiniz ve bilgilere sadece gerekli olduğunda erişmeniz gerekebilir.
3. **Algılama** bölümü, çok sık veya çok nadir değişebilir. Güvenlik uyarılarınızı her zaman gözden geçirin ve Güvenlik Merkezi önerilerine göre eyleme geçin.

### <a name="hardening-access-and-applications"></a>Erişimi ve uygulamaları sağlamlaştırma

Güvenlik sürecinizin bir parçası olarak VM erişimini kısıtlamak ve VM'ler üzerinde çalışan uygulamaları denetlemek için önlemler de almanız gerekir. Azure VM'lerinize gelen trafiği engelleyerek saldırı riskini azaltabilir ve aynı zamanda gerekli olduğunda VM'lere kolayca bağlanabilirsiniz. VM'lerinize erişimi sertleştirmek için [tam zamanında VM](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) erişim özelliğini kullanın.

Azure'da bulunan VM'lerinizde hangi uygulamaların çalıştırılabilen uygulamaları sınırlamak için [Uyarlanabilir Uygulama](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application) Denetimleri'ni kullanabilirsiniz. Diğer faydaların yanı sıra, bu kötü amaçlı yazılıma karşı VM'lerinizi sertleştirmenize yardımcı olur. Güvenlik Merkezi, makine öğrenimini kullanarak, beyaz liste kuralları oluşturmanıza yardımcı olmak için VM'de çalışan işlemleri analiz eder.


## <a name="incident-response"></a>Olay yanıtı
Güvenlik Merkezi, tehditler oluşunca algılar ve sizi tehditlere karşı uyarır. Kuruluşlar, yeni güvenlik uyarılarını izlemeli ve gerekirse daha fazla araştırmak veya saldırıyı düzeltmek için eyleme geçmelidir. Güvenlik Merkezi tehdit korumasının nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nin tehditleri nasıl algılayıp yanıtlayıştOkuduğunu](security-center-alerts-overview.md#detect-threats)okuyun.

Bu makale kendi Olay Yanıtı planınızı oluşturmanıza yardımcı olmaya yönelik değildir, ancak olay yanıtı aşamalarının temeli olarak Bulut yaşam döngüsünde Microsoft Azure Güvenlik Yanıtı kullanılacaktır. Aşamalar aşağıdaki diyagramda gösterilmiştir:

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

Bu sayfadan bir [araştırma](https://docs.microsoft.com/azure/security-center/security-center-investigation) başlatarak saldırının zaman çizelgesi, gerçekleşme şekli, gizliliği bozulmuş olabilecek sistemler ve kullanılan kimlik bilgileri hakkında bilgi edinebilir, saldırı zincirinin tamamını grafiklerle görebilirsiniz.

Tehlikeye atılmış sistemi tanımladıktan sonra, daha önce oluşturulmuş bir [İş Akışı Otomasyonu](workflow-automation.md) çalıştırabilirsiniz. Bunlar, bir uyarı tarafından tetiklendikten sonra Güvenlik Merkezi'nden yürütülebilen yordamlar topluluğu.

Bir Olay Yanıtı videosu [için Azure Güvenlik Merkezi & Microsoft Operations Management Suite'ten nasıl yararlanılır?](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703)

> [!NOTE]
> Olay Yanıtı işlemisırasında size yardımcı olmak için Güvenlik Merkezi yeteneklerinin nasıl kullanılacağı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtlamayı](security-center-managing-and-responding-alerts.md) okuyun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi benimsemeyi nasıl planlayacağınızı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) — Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSSS](faq-general.md) - Hizmeti kullanma yla ilgili sık sorulan soruları bulun.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) — Azure güvenliği ve uyumluluğu yla ilgili blog gönderilerini bulun.
