---
title: Azure Avustralya 'da güvenlik için sistem izleme
description: Avustralya Kamu politika, yönetmelikler ve yasalların belirli ihtiyaçlarını karşılamak için Avustralya bölgelerinde sistem Izlemeyi yapılandırmaya yönelik rehberlik.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: b68bdb6d1c349df67943889b511eac87c940b531
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571893"
---
# <a name="system-monitoring-for-security-in-azure-australia"></a>Azure Avustralya 'da güvenlik için sistem izleme

Gerçek zamanlı izleme ve rutin güvenlik değerlendirmeleri içeren güçlü güvenlik stratejilerinin olması, bulut dahil BT ortamlarınızın gün içinde işletimsel güvenliğini geliştirmenize yönelik öneme sahiptir.

Bulut güvenliği, müşteri ile bulut sağlayıcısı arasında bir birleşme çabadır. Microsoft Azure, [Avustralya Cyber Güvenlik Merkezi 'nin (ACSC) bilgi güvenliği El Ile denetim](https://acsc.gov.au/infosec/ism/index.htm) (ISM) içindeki önerilere göz önünde bulundurarak bu gereksinimlerin kolaylaştırmasını sağlayan dört hizmet vardır. Özellikle, denetleyebileceği olay günlüğü, olay günlüğü denetimi ve güvenlik güvenlik açığı değerlendirmesi ve yönetimi uygulanmasıdır. Microsoft Azure hizmetleri şunlardır:

* Azure Güvenlik Merkezi
* Azure İzleyici
* Azure Advisor
* Azure İlkesi

ACSC, bu Hizmetleri **korunan** veriler için kullanmanızı önerir. Bu hizmetleri kullanarak BT ortamlarınızı önceden izleyebilir ve analiz etme ve güvenliği iyileştirmek için kaynakların en iyi şekilde nerede ayrılacağı hakkında bilinçli kararlar verebilirsiniz. Bu hizmetlerin her biri, mümkün olan en iyi öngörüyi, önerileri ve korumayı sağlayan bir birleştirilmiş çözüm parçasıdır.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) , Azure kaynaklarının ve barındırılan verilerinizin güvenliğini izlemek ve geliştirmek için kullanabileceğiniz Birleşik bir güvenlik Yönetimi konsolu sağlar. Azure Güvenlik Merkezi, Azure Advisor 'ın en iyi yöntem Yapılandırması durumunun analizini ve Azure Ilkesinin genel uyumluluğunu temel alan bir puan olan güvenli puanı içerir.

Azure Güvenlik Merkezi, Azure müşterilerine aşağıdaki özellikleri sağlar:

* Güvenlik ilkesi, değerlendirme ve öneriler
* Güvenlik olayı toplama ve arama
* Erişim ve uygulama denetimleri
* Gelişmiş tehdit algılama
* Tam zamanında sanal makineler erişim denetimi
* Karma güvenlik

Azure Güvenlik Merkezi tarafından izlenen kaynakların kapsamı, desteklenen şirket içi kaynakları karma bulut ortamında içerecek şekilde genişletilebilir. Bu, şu anda desteklenen bir System Center Operations Manager sürümü tarafından izlenmekte olan şirket içi kaynakları içerir.

Güvenlik Merkezi "standart" katmanı, [asd 'Nin önemli 8](https://acsc.gov.au/publications/protect/essential-eight-explained.htm)' i için gereken bulut tabanlı güvenlik denetimleri de sağlar. Bunlar, tam zamanında erişim aracılığıyla yönetim ayrıcalığının uygulama beyaz listesini ve kısıtlamasını içerir.

### <a name="azure-monitor"></a>Azure İzleyici

[Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) , tüm Azure kaynakları için denetleyebileceği günlük çözümüdür ve Log Analytics ve Application Insights içerir. Azure kaynaklarından iki temel veri türü toplanır: Günlükler ve ölçümler. Azure Izleyici 'de toplandıktan sonra, günlük kaydı bilgileri çok çeşitli araçlar ve çeşitli amaçlarla kullanılabilir.

![Azure İzleyici Genel Bakışı](media/overview.png)

Azure Izleyici Ayrıca "Azure etkinlik günlüğü" ni içerir. SActivity günlüğü, Azure 'da gerçekleşen tüm abonelik düzeyi olaylarını depolar. Azure müşterilerinin, Azure kaynaklarında hangi işlemlerin gerisinde olduğunu "kim, ne zaman ve ne zaman) görmesini sağlar. Azure Izleyici ve Azure etkinlik günlüğü olaylarına gönderilen kaynak tabanlı günlük kaydı, yerleşik kusto sorgu dili kullanılarak analiz edilebilir. Bu günlükler daha sonra dışarıya aktarılabilir, özel panolar ve görünümler oluşturmak için kullanılabilir ve uyarıları ve bildirimleri tetiklemek üzere yapılandırılabilir.

### <a name="azure-advisor"></a>Azure Advisor

Azure [Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview) , Azure aboneliğinizde desteklenen Azure kaynakları, sistem tarafından oluşturulan günlük dosyaları ve geçerli kaynak yapılandırmalarının analizler. Azure Advisor 'da sunulan analiz gerçek zamanlı olarak oluşturulur ve Microsoft 'un önerilen en iyi uygulamalarına dayanır. Ortamınıza eklenen desteklenen tüm Azure kaynakları analiz edilir ve uygun öneriler sağlanacaktır. Azure Advisor önerileri dört en iyi yöntem kategorisine göre kategorilere ayrılır:

* Güvenlik
* Yüksek Kullanılabilirlik
* Performans
* Maliyet

Azure Advisor tarafından oluşturulan güvenlik önerileri, Azure Güvenlik Merkezi tarafından sağlanan genel güvenlik analizinin bir parçasını oluşturur.

Azure Advisor tarafından toplanan bilgiler yöneticilere şunları sağlar:

* Önerilen en iyi yöntemi karşılamayan kaynak yapılandırması hakkında Öngörüler
* Az yapılacak belirli düzeltme eylemleriyle ilgili yönergeler
* Hangi düzeltme eylemlerinin yüksek öncelikli olarak alınması gerektiğini belirten ranlar

### <a name="azure-policy"></a>Azure İlkesi

[Azure ilkesi](https://docs.microsoft.com/azure/governance/policy/overview) , Azure Kaynak türlerini ve izin verilen yapılandırmalarını yöneten kurallar uygulama yeteneği sağlar. İlke, kaynak oluşturma ve yapılandırmayı denetlemek için kullanılabilir veya bir ortam genelinde yapılandırma ayarlarını denetlemek için kullanılabilir. Bu denetim sonuçları, Düzeltme etkinliklerinin temelini oluşturmak için kullanılabilir. Azure Ilkesi rol tabanlı erişim denetiminden (RBAC) farklıdır; Azure Ilkesi, kaynakları ve yapılandırmalarını kısıtlamak için kullanılır, Azure kullanıcılarına ayrıcalıklı erişimi kısıtlamak için RBAC kullanılır.

Belirli bir ilkenin zorlanıp zorlanmadığını belirtir, ilke uyumluluğu sürekli olarak izlenir ve genel ve kaynağa özgü uyumluluk bilgileri yöneticilere sağlanır. Azure Ilke uyumluluğu verileri, Azure Güvenlik Merkezi 'ne sağlanır ve güvenli puanınızın bir parçası olarak ayarlanır.

## <a name="key-design-considerations"></a>Önemli tasarım konuları

Bir olay günlüğü stratejisi uygularken, ACSC ıSM aşağıdaki hususları vurgular:

* Denetleyebileceği günlüğü olanakları
* Günlüğe kaydedilecek belirli olaylar
* Olay günlüğü koruması
* Olay günlüğü saklama
* Olay günlüğü denetimi

Günlük toplama ve yönetmeye ek olarak, söz konusu bir kuruluşun BT ortamına yönelik yordam güvenlik açığı değerlendirmesini de önerir.

### <a name="centralised-logging"></a>Denetleyebileceği günlüğe kaydetme

Herhangi bir günlük çözümü, mümkün olan yerlerde, yakalanan günlükleri tek bir veri deposunda birleştirmelidir. Bu, yalnızca işlemsel karmaşıklığı azaltır ve birden çok veri silolarının oluşturulmasını engeller. Bu, birden fazla kaynaktan toplanan verilerin tanımlanmasına izin vermek için bir araya bir şekilde analiz yapılmasını sağlar. Bu, herhangi bir siber güvenlik olaylarının kapsamını saptamak ve yönetmek için önemlidir.

Bu gereksinim, Azure Izleyici ile tüm Azure müşterileri için karşılanır. Bu teklif yalnızca Azure 'da tüm Azure kaynakları için bir denetleyebileceği günlük deposu sunmadığından, verilerinizi bir Azure Olay Hub 'ına akışla göndermenizi de sağlar. Azure Event Hubs, tam olarak yönetilen ve gerçek zamanlı bir veri alma hizmeti sağlar. Azure Izleyici verileri bir Azure Olay Hub 'ına akışa alındıktan sonra, veriler desteklenen güvenlik bilgilerine ve olay yönetimi (SıEM) depolarına ve diğer üçüncü taraf izleme araçlarına da kolayca bağlanabilir.

Microsoft ayrıca kendi Azure yerel SıEM çözümünü Azure Sentinel 'e sunmaktadır. Azure Sentinel, çok çeşitli veri bağlayıcılarını destekler ve tüm kuruluş genelinde güvenlik olaylarını izlemek için kullanılabilir. Desteklenen [veri bağlayıcılarından](https://docs.microsoft.com/azure/sentinel/connect-data-sources), Azure Sentinel 'in yerleşik makine öğrenimine ve kusto sorgu dilinden verileri birleştirerek güvenlik yöneticileri, uyarı algılama, tehdit görünürlüğü, proaktif olarak arama için tek bir çözümle sağlanır. ve tehdit yanıtı. Sentinel Ayrıca, güvenlik yöneticilerinin bir kuruluş içinde paylaşılabilen bir reuseable PlayBook 'ta güvenlik araştırmasının parçası olarak geçen tüm adımları kaydetmesine olanak tanıyan bir araştırma ve Not defteri özelliği de sağlar. Güvenlik yöneticileri, tek bir aday kullanıcının eylemlerini araştırmak için yerleşik [Kullanıcı analizlerini](https://docs.microsoft.com/azure/sentinel/user-analytics) bile kullanabilirler.

### <a name="logged-events-and-log-detail"></a>Günlüğe kaydedilen olaylar ve günlük ayrıntısı

ISM, herhangi bir günlüğe kaydetme stratejisine dahil edilecek olay günlüğü türlerinin ayrıntılı bir listesini sağlar. Yakalanan tüm günlüklerin, analiz ve araştırmalar yaparken pratik kullanım kullanımı için yeterli ayrıntı içermesi gerekir.

Azure 'da toplanan Günlükler aşağıdaki üç kategoriden biri altında yer almalıdır:

* **Denetim ve yönetim günlükleri**: Bu Günlükler Azure Resource Manager oluşturma, GÜNCELLEŞTIRME ve SILME işlemleri hakkında bilgiler sağlar.

* **Veri düzlemi günlükleri**: Bunlar Azure Kaynak kullanımının bir parçası olarak oluşturulan olayları içerir. Sistem, güvenlik ve uygulama günlükleri dahil Windows olay günlükleri gibi kaynakları içerir.

* **Işlenen olaylar**: Bu olaylar, Azure tarafından müşterinin adına otomatik olarak işlenen olaylar ve uyarılar hakkında bilgiler içerir. Bir Işlenen olaya örnek olarak bir Azure Güvenlik Merkezi uyarısı gönderilir.

Azure sanal makine izleme, hem Windows hem de Linux için sanal makine aracısının dağıtımı tarafından geliştirilmiştir. Bu, toplanan günlüğe kaydetme bilgilerinin kapsamını artırır. Bu aracının dağıtımı, Azure Güvenlik Merkezi aracılığıyla otomatik olarak gerçekleşecek şekilde yapılandırılabilir.

Microsoft, Azure kaynağına özgü Günlükler ve bunların [şemaları](https://docs.microsoft.com/azure/security/azure-log-audit)hakkında ayrıntılı bilgi sağlar.

### <a name="log-retention-and-protection"></a>Günlük tutma ve koruma

 Olay günlükleri, gerekli saklama dönemi için güvenli bir şekilde depolanmalıdır. Bu, günlüklerin en az yedi yıl boyunca korunduğunu önerir. Azure, toplanan günlüklerinizin uzun ömrünü sağlamak için birkaç yöntem sunar. Varsayılan olarak, Azure günlük olayları 90 gün süreyle depolanır. Azure Izleyici tarafından yakalanan günlük verileri, uzun süreli saklama için gereken şekilde Azure depolama hesabında taşınabilir ve depolanabilir. Bir Azure depolama hesabında depolanan etkinlik günlükleri, gün sayısı veya gerektiğinde süresiz olarak tutulabilirler.

Azure günlük olaylarını depolamak için kullanılan Azure depolama hesapları, coğrafi olarak yedekli hale getirilebilir ve Azure Backup kullanılarak yedeklenebilir. Azure Backup tarafından yakalandıktan sonra, günlükleri içeren yedeklemelerin silinmesi için yönetici onayı gerekir ve silinmek üzere işaretlenmiş yedeklemeler, kurtarmaya izin vermek için hala 14 gün boyunca tutulur. Azure Backup, korunan bir örneğin 27 yıldan fazla günlük yedekleme sağlayan 9999 kopyasının kullanılmasına izin verir.

Rol tabanlı Access Control, Azure günlük kaydı için kullanılan kaynaklara erişimi denetlemek için kullanılmalıdır. Günlüklerde bulunan verilerin güvenliğini sağlamak için Azure Izleyici, Azure depolama hesapları ve Azure yedeklemeleri, rol tabanlı erişim denetimleriyle yapılandırılmalıdır.

### <a name="log-auditing"></a>Günlük denetimi

Günlüklerin doğru değeri, analiz edildikten sonra yeniden oluşturulur. Hem otomatik hem de el ile analiz kullanmanın yanı sıra kullanılabilen araçlarla ilgili bilgi sahibi olmak, organimsel güvenlik ilkesinin ve siber güvenlik olaylarının ihlal olup olmadığı konusunda size yardımcı olur. Azure Izleyici, toplanan günlüklere analiz etme için zengin bir araç kümesi sağlar. Daha sonra bu çözümlemenin sonucu, birden çok biçimdeki sistemler, görselleştirmelere veya dağıtılabilir bir şekilde paylaşılabilir.

Azure Izleyici 'de depolanan günlük verileri Log Analytics çalışma alanında tutulur. Tüm analizler bir sorgu ile başlar. Azure Izleyici sorguları, kusto sorgu dilinde yazılır. Sorgular, Azure panolarından uyarı kurallarına kadar Azure Izleyici 'deki tüm çıktıların temelini oluşturur.

![Azure günlük sorgularına genel bakış](media/queries-overview.png)

Günlüklerin denetlenmesi, Izleme çözümlerinin kullanımı aracılığıyla geliştirilebilir. Bunlar, koleksiyon mantığı, sorgular ve veri görselleştirmeli görünümlerini içeren önceden paketlenmiş çözümlerdir. Microsoft, Azure Marketi 'nde çeşitli Izleme Çözümleri ve ürün satıcılarından ek çözümler [sağlar](https://docs.microsoft.com/azure/azure-monitor/insights/solutions-inventory) .

### <a name="vulnerability-assessment-and-management"></a>Güvenlik açığı değerlendirmesi ve yönetimi

Bu, yordam güvenlik açığı değerlendirmesi ve yönetimi için gerekli olan notları not edin. BT ortamınız sürekli olarak gelişiyor ve dış güvenlik tehdidi sürekli olarak değiştiriliyor. Azure Güvenlik Merkezi ile otomatikleştirilmiş güvenlik açığı değerlendirmeleri yapabilir ve düzeltme etkinliklerini planlamaya ve gerçekleştirmeye yönelik yönergeler edinebilirsiniz.

Azure Güvenlik Merkezi 'ndeki güvenli puan, uygulandığında ortamınızın güvenliğini iyileştirecek önerilerin bir listesini sağlar. Liste, en yüksekten en düşüğe kadar genel güvenli puanı üzerindeki etkiye göre sıralanır. Listenin etkiye göre sıralanması, güveninizi geliştirmek için en yüksek değeri sunan en yüksek öncelikli önerilere odaklanmanızı sağlar.

Azure Ilkesi, devam eden güvenlik açığı değerlendirmesinde önemli bir bölüm de oynar. Azure Ilke aralığında, belirli kaynak türlerinin oluşturulmasını engellemek için kaynakların oluşturulabilecek Azure bölgelerini kısıtlamak üzere kaynak etiketleri ve değerlerini zorlamadan kullanılabilir ilke türleri. Bir dizi Azure ilkesi, girişimler halinde gruplandırılabilir. Girişimler, bir grup olarak birlikte uygulandığında, belirli bir güvenlik veya uyumluluk hedefinin temelini oluşturan ilgili Azure ilkelerini uygulamak için kullanılır.

Azure Ilkesinde sürekli büyüyen yerleşik ilke tanımlarının bir kitaplığı vardır. Azure portal Ayrıca kendi özel Azure Ilke tanımlarınızı yazma seçeneği sunar. Mevcut kitaplıkta bir ilke bulduktan veya yeni bir ilke oluşturduktan sonra, ilkeyi Azure kaynaklarına atayabilirsiniz. Bu atamalar, kaynak yönetimi hiyerarşisindeki çeşitli düzeylerde [kapsam](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage) yapılabilir. İlke ataması devralınır, yani bir kapsamdaki tüm alt kaynaklar aynı ilke atamasını alır. Kaynaklar ayrıca, kapsamlı ilke atamasından gerektiği şekilde dışlanamaz.

Dağıtılan tüm Azure ilkeleri, kuruluşun güvenli puanına katkıda bulunur. Yüksek bir beslik ortamında, özel Azure Ilke tanımları, belirli iş yükleri için uygun olan denetim bilgilerini sağlamak üzere oluşturulabilir ve dağıtılabilir.

## <a name="getting-started"></a>Başlarken

Azure Güvenlik Merkezi 'ni kullanmaya başlamak ve Azure Izleyici, danışman ve Ilkesini tam olarak kullanabilmek için, Microsoft aşağıdaki ilk adımları önerir:

* Azure Güvenlik Merkezi 'Ni etkinleştir
* Standart katmana yükseltme
* Desteklenen Azure sanal makinelerine Microsoft Monitoring Agent otomatik sağlamayı etkinleştir
* Güvenlik Merkezi panosunda güvenlik önerilerini ve uyarılarını gözden geçirin, öncelikler yapın ve azaltın

## <a name="next-steps"></a>Sonraki adımlar

İlke ve mevzuata uyumluluğu sağlamak için Azure Avustralya kaynaklarınız üzerinde idare ve denetim uygulama hakkında ayrıntılı bilgi için Azure [ilkesi ve Azure şemaları](azure-policy.md) makalesini okuyun.
