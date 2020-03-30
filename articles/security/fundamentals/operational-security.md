---
title: Azure Operasyonel Güvenlik | Microsoft Dokümanlar
description: Microsoft Azure Monitor günlükleri, hizmetleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 34c0c52945abc6e0ab74b1cb180581c76464bee8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75749964"
---
# <a name="azure-operational-security"></a>Azure Operasyonel Güvenlik
## <a name="introduction"></a>Giriş

### <a name="overview"></a>Genel Bakış
Güvenliğin buluttaki bir iş olduğunu ve Azure güvenliği hakkında doğru ve zamanında bilgi bulmanın ne kadar önemli olduğunu biliyoruz. Uygulamalarınız ve hizmetleriniz için Azure'u kullanmanın en iyi nedenlerinden biri, mevcut çok çeşitli güvenlik araçlarından ve özelliklerinden yararlanmaktır. Bu araçlar ve özellikler, güvenli Azure platformunda güvenli çözümler oluşturmayı mümkün kılar. Windows Azure, müşteri verilerinin gizliliğini, bütünlüğünü ve kullanılabilirliğini sağlarken saydam hesap verebilirliği de etkinleştirmelidir.

Müşterilerin Microsoft Azure'da uygulanan güvenlik denetimleri dizisini hem müşterinin hem de Microsoft'un operasyonel perspektiflerinden daha iyi anlamalarına yardımcı olmak için, kapsamlı bir "Azure Operasyonel Güvenlik" başlıklı bu teknik belge Windows Azure ile kullanılabilen operasyonel güvenliğe bakın.

### <a name="azure-platform"></a>Azure Platformu
Azure, işletim sistemleri, programlama dilleri, çerçeveler, araçlar, veritabanları ve aygıtlardan oluşan geniş bir seçkiyi destekleyen genel bir bulut hizmeti platformudur. Docker entegrasyonu ile Linux konteynerleri çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturmak; iOS, Android ve Windows aygıtları için arka uçlar oluşturun. Azure Bulut hizmeti, milyonlarca geliştirici ve BT uzmanının zaten güvendiği ve güvendiği aynı teknolojileri destekler.

BT varlıklarını oluşturduğunuzda veya bir genel bulut hizmeti sağlayıcısına aktardığınızda, bulut tabanlı varlıklarınızın güvenliğini yönetmek için sağladıkları hizmetler ve denetimlerle uygulamalarınızı ve verilerinizi koruma yeteneklerine güvendiğiniz bir genel bulut hizmeti sağlayıcısı.

Azure'un altyapısı, aynı anda milyonlarca müşteriyi barındırmak için tesisten uygulamalara kadar tasarlanmıştır ve işletmelerin güvenlik gereksinimlerini karşılayabildiği güvenilir bir temel sağlar. Buna ek olarak Azure, kuruluşunuzun dağıtımlarının benzersiz gereksinimlerini karşılayacak şekilde güvenliği özelleştirebilmeniz için size çok çeşitli yapılandırılabilir güvenlik seçenekleri ve bunları denetleme olanağı sağlar. Bu belge, Azure güvenlik özelliklerinin bu gereksinimleri karşılamanıza nasıl yardımcı olabileceğini anlamanıza yardımcı olur.

### <a name="abstract"></a>Abstract
Azure Operasyonel Güvenlik, Microsoft Azure'daki verilerini, uygulamalarını ve diğer varlıklarını korumak için kullanıcıların kullanabileceği hizmetler, denetimler ve özellikler anlamına gelir. Azure Operasyonel Güvenlik, Microsoft Güvenlik Geliştirme Yaşam Döngüsü (SDL), Microsoft Güvenlik Yanıt Merkezi programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikler le elde edilen bilgileri içeren bir çerçeve üzerine kurulmuştur, ve siber güvenlik tehdidi manzara derin farkındalık.

Bu teknik inceleme, Microsoft'un Microsoft Azure bulut platformu nda Azure İşgüvenliği'ne yaklaşımını özetler ve aşağıdaki hizmetleri kapsar:
1.  [Azure İzleyici](../../azure-monitor/index.yml)

2.  [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)

3.  [Azure İzleyici](../../azure-monitor/overview.md)

4.  [Azure Ağ izleyicisi](../../network-watcher/network-watcher-monitoring-overview.md)

5.  [Azure Depolama analitiği](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Etkin dizini](../../active-directory/fundamentals/active-directory-whatis.md)


## <a name="microsoft-azure-monitor-logs"></a>Microsoft Azure Monitor günlükleri

Microsoft Azure Monitor günlükleri, karma bulut için BT yönetimi çözümüdür. Tek başına veya mevcut Sistem Merkezi dağıtımınızı genişletmek için kullanılan Azure Monitor günlükleri, altyapınızın bulut tabanlı yönetimi için maksimum esneklik ve denetim sağlar.

![Azure İzleyici günlükleri](./media/operational-security/azure-operational-security-fig1.png)

Azure Monitör günlükleri ile, şirket içi, Azure, AWS, Windows Server, Linux, VMware ve OpenStack dahil olmak üzere herhangi bir buluttaki herhangi bir örneği rekabetçi çözümlerden daha düşük bir maliyetle yönetebilirsiniz. Bulutun ilk dünyası için tasarlanan Azure Monitor günlükleri, yeni iş zorluklarıyla başa çıkmak ve yeni iş yüklerini, uygulamaları ve bulut ortamlarını barındırmanın en hızlı ve en uygun maliyetli yolu olan işletmenizi yönetmek için yeni bir yaklaşım sunar.

### <a name="azure-monitor-services"></a>Azure Monitör hizmetleri

Azure Monitor günlüklerinin temel işlevselliği, Azure'da çalışan bir dizi hizmet tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

| Hizmet  | Açıklama|
| :------------- | :-------------|
| Azure İzleyici günlükleri | Fiziksel ve sanal makineler dahil olmak üzere çeşitli kaynakların kullanılabilirliğini ve performansını izleyin ve analiz edin. |
|Automation | El ile gerçekleştirilen işlemleri otomatikleştirin; fiziksel ve sanal makinelere yapılandırma uygulayın. |
| Backup | Kritik verileri yedekle ve geri yükleyin. |
| Site Recovery | Kritik uygulamalar için yüksek kullanılabilirlik sağlayın. |

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure Monitor günlükleri,](https://azure.microsoft.com/documentation/services/log-analytics) yönetilen kaynaklardan merkezi bir depoya veri toplayarak izleme hizmetleri sağlar. Bu verilere olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler dahil olabilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir.


Bu yöntem, çeşitli kaynaklardan gelen verileri birleştirmenize olanak sağlar, böylece Azure hizmetlerinizdeki verileri mevcut şirket ortamınızla birleştirebilirsiniz. Ayrıca, veri toplama işlemini veriler üzerinde gerçekleştirilen eylemden ayırarak tüm eylemlerin her tür veri üzerinde kullanılabilmesini mümkün kılar.


![Azure İzleyici günlükleri](./media/operational-security/azure-operational-security-fig2.png)

Azure Monitör hizmeti, bulut tabanlı verilerinizi aşağıdaki yöntemleri kullanarak güvenli bir şekilde yönetir:
-   veri ayrımı
-   veri saklama
-   fiziksel güvenlik
-   olay yönetimi
-   Uyumlu -luk
-   güvenlik standartları sertifikaları

### <a name="azure-backup"></a>Azure Backup

[Azure Yedekleme](https://azure.microsoft.com/documentation/services/backup) veri yedekleme ve geri yükleme hizmetleri sağlar ve Azure Monitor ürün ve hizmet paketinin bir parçasıdır.
Uygulama verilerinizi korur ve herhangi bir sermaye yatırımı olmadan en düşük işletim giderleriyle yıllar boyunca saklar. SQL Server ve SharePoint gibi uygulama iş yüklerine ek olarak fiziksel ve sanal Windows sunucularından gelen verileri yedekleyebilir. Ayrıca, sistem [merkezi veri koruma yöneticisi (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) tarafından, korumalı verileri artıklık ve uzun süreli depolama için Azure'a çoğaltmak için de kullanılabilir.


Azure Backup'ta korunan veriler belirli bir coğrafi bölgede yer alan bir yedekleme kasasında depolanır. Veriler aynı bölge içinde çoğaltılır ve kasanın türüne bağlı olarak, daha fazla esneklik için başka bir bölgeye çoğaltılabilir.

### <a name="management-solutions"></a>Yönetim Çözümleri
[Azure Monitor,](../../security-center/security-center-intro.md) şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür.


[Yönetim Çözümleri,](../../monitoring/monitoring-solutions.md) bir veya daha fazla Azure Monitörü hizmetlerini kullanarak belirli bir yönetim senaryosu uygulayan önceden paketlenmiş mantık kümeleridir. Azure Monitor'a yaptığınız yatırımın değerini artırmak için Azure aboneliğinize kolayca ekleyebileceğiniz Microsoft ve iş ortaklarından farklı çözümler mevcuttur. İş ortağı olarak, uygulamalarınızı ve hizmetlerinizi desteklemek ve azure marketi veya hızlı başlangıç şablonları aracılığıyla kullanıcılara sunmak için kendi çözümlerinizi oluşturabilirsiniz.


![Yönetim Çözümleri](./media/operational-security/azure-operational-security-fig4.png)

Ek işlevsellik sağlamak için birden çok hizmet kullanan bir çözüme iyi bir örnek [Güncelleştirme Yönetimi çözümüdür.](../../automation/automation-update-management.md) Bu çözüm, her aracı için gerekli güncelleştirmeler hakkında bilgi toplamak için Windows ve Linux için [Azure Monitor günlükleri](../../log-analytics/log-analytics-queries.md) aracısını kullanır. Bu verileri, dahil edilmiş bir panoyla analiz edebileceğiniz Azure Monitor günlük deposuna yazar.

Bir dağıtım oluşturduğunuzda, [gerekli](../../automation/automation-intro.md) güncelleştirmeleri yüklemek için Azure Otomasyonu'ndaki runbook'lar kullanılır. Bu işlemi baştan sona portalda yönetirsiniz ve altyapısal ayrıntılar konusunda endişelenmeniz gerekmez.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Azure kaynaklarınızın korunmasına yardımcı olur. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Hizmet içinde, polisleri yalnızca Azure aboneliklerinize karşı değil, aynı zamanda [Kaynak Gruplarına](../../azure-resource-manager/management/overview.md#resource-groups)karşı da tanımlayabilirsiniz, böylece daha ayrıntılı olabilirsiniz.

### <a name="security-policies-and-recommendations"></a>Güvenlik ilkeleri ve öneriler

Güvenlik ilkeleri, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar.

Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlarsınız.

![Güvenlik ilkeleri ve öneriler](./media/operational-security/azure-operational-security-fig5.png)


Abonelik düzeyinde etkinleştirilen ilkeler, sağ taraftaki diyagramda gösterildiği gibi abonelik içindeki tüm kaynak gruplarına otomatik olarak yayılır:


### <a name="data-collection"></a>Veri toplama

Güvenlik Merkezi, verilerinizin güvenlik durumunu değerlendirmek, güvenlik önerileri sağlamak ve sizi tehditlere karşı uyarmak için sanal makinelerinizden (VM) veri toplar. İlk erişim Güvenlik Merkezinizde, aboneliğinizdeki tüm Sanal Taşıtlarda veri toplama etkinleştirilir. Veri toplama önerilir, ancak Güvenlik Merkezi ilkesinde veri toplamayı kapatarak bu seçimi kaldırabilirsiniz.

### <a name="data-sources"></a>Veri kaynakları

- Azure Güvenlik Merkezi, güvenlik durumunuzu görüntüleme, güvenlik açıklarını tanımlayıp çözümler önerme ve etkin tehditleri saptama amacıyla aşağıdaki kaynaklarda bulunan verileri analiz eder:

-   Azure Hizmetleri: Dağıttığınız Azure hizmetlerinin yapılandırmasına ilişkin bilgileri, ilgili hizmetin kaynak sağlayıcısıyla iletişim kurarak kullanır.

- Ağ Trafiği: Microsoft’un altyapısından kaynak/hedef IP/bağlantı noktası, paket boyutu ve ağ protokolü gibi örneği alınmış ağ trafiği meta verilerini kullanır.

-   İş Ortağı Çözümleri: Güvenlik duvarları ve kötü amaçlı yazılımdan koruma çözümleri gibi tümleşik iş ortağı çözümlerinden güvenlik uyarılarını kullanır.

-   Sanal Makineleriniz: Sanal makinelerinizdeki yapılandırma bilgilerini ve Windows olay ve denetim günlükleri, IIS günlükleri, syslog iletileri ve kilitlenme dökümü dosyaları gibi güvenlik olaylarına ilişkin bilgileri kullanır.

### <a name="data-protection"></a>Veri koruma

Müşterilerin tehditleri önlemesine, algılamasına ve yanıt vermesine yardımcı olmak amacıyla Azure Güvenlik Merkezi, güvenlikle ilgili veriler, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve diğer verileri toplar ve işler. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.

-   **Veri ayırma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır.

-   **Veri erişimi**: Güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak için Microsoft personeli, kilitlenme döküm dosyaları, işlem oluşturma etkinlikleri, VM disk anlık görüntüleri ve sanal makinelerinizden gelen Müşteri Verilerini veya kişisel verileri istemeden içerebilecek yapılar da dahil olmak üzere Azure hizmetleri tarafından toplanan veya analiz edilen bilgilere erişebilir. Microsoft'un Müşteri Verilerini kullanmadığını veya herhangi bir reklam veya benzer ticari amaçla bu verilerden bilgi elde etmediğini belirten [Microsoft Çevrimiçi Hizmetler Koşulları ve Gizlilik Bildirimi'ne](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)bağlıyız.

-   **Veri kullanımı**: Microsoft önleme ve algılama özelliklerimizi geliştirmek amacıyla birden fazla kiracıda görülen modelleri ve tehdit bilgilerini kullanır; bunu [Gizlilik Bildirimi](https://www.microsoft.com/en-us/privacystatement/OnlineServices/) belgemizde açıklanan gizlilik taahhütlerine uygun şekilde yaparız.

### <a name="data-location"></a>Veri konumu

Azure Güvenlik Merkezi, kilitlenme döküm dosyalarının kısa ömürlü kopyalarını toplar ve açıktan yararlanma girişimlerinin ve başarılı uzlaşmaların kanıtı olarak analiz eder. Azure Güvenlik Merkezi bu analizi çalışma alanıyla aynı coğrafi bölgede gerçekleştirir ve analiz tamamlandığında kısa ömürlü kopyaları siler. Makine yapıları VM ile aynı bölgede merkezi olarak depolanır.

-   **Depolama Hesaplarınız**: Sanal makinelerin çalıştığı her bölge için bir depolama hesabı belirtilir. Bunun yapılması verileri, verilerin toplandığı sanal makine ile aynı bölgede depolamanızı sağlar.

-   **Azure Güvenlik Merkezi Depolama**: İş ortağı uyarıları, öneriler ve güvenlik durumu gibi güvenlik uyarıları hakkında bilgiler, şu anda Birleşik Devletler’de bulunan merkezde depolanmaktadır. Bu bilgiler size güvenlik uyarısı, öneri veya sistem durumu sağlamak üzere gerektiğinde sanal makinelerinizden toplanan ilgili yapılandırma bilgilerini ve güvenlik olaylarını içerebilir.


## <a name="azure-monitor"></a>Azure İzleyici

[Azure Monitor, Güvenlik](../../security-center/security-center-monitoring.md) ve Denetim çözümünü kaydeder ve BT'nin tüm kaynakları etkin bir şekilde izlemesine olanak tanır ve bu da güvenlik olaylarının etkisini en aza indirmeye yardımcı olur. Azure Monitor günlükleri Güvenlik ve Denetim kaynakları izlemek için kullanılabilecek güvenlik etki alanları vardır. Güvenlik etki alanı, güvenlik izleme için seçeneklere hızlı erişim sağlar, aşağıdaki etki alanları daha ayrıntılı olarak ele alınmıştır:

-   Kötü amaçlı yazılım değerlendirmesi
-   Güncelleştirme değerlendirmesi
-   Kimlik ve Erişim.

Azure Monitor, belirli kaynak türleri hakkında bilgi için işaretçiler sağlar. Hem Azure altyapısından (Etkinlik Günlüğü) hem de her bir Azure kaynağından (Tanılama Günlükleri) veriler üzerinde görselleştirme, sorgu, yönlendirme, uyarı, otomatik ölçek ve otomasyon sunar.

![Azure İzleyici](./media/operational-security/azure-operational-security-fig6.png)


Bulut uygulamaları birçok hareketli parçayla karmaşıktır. İzleme, uygulamanızın sağlıklı bir durumda çalışır durumda kalmasını sağlamak için veri sağlar. Ayrıca, olası sorunları savuşturmanıza veya geçmiş sorunları gidermenize de yardımcı olur.

Ayrıca, uygulamanız hakkında derin bilgiler edinmek için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansını veya bakımı geliştirmenize veya aksi takdirde el ile müdahale gerektiren eylemleri otomatikleştirmenize yardımcı olabilir.

### <a name="azure-activity-log"></a>Azure Etkinlik Günlüğü


Aboneliğinizdeki kaynaklarüzerinde gerçekleştirilen işlemlerhakkında bilgi sağlayan bir günlüktir. Etkinlik Günlüğü, abonelikleriniz için denetim düzlemi olaylarını bildirdiği için daha önce "Denetim Günlükleri" veya "Operasyonel Günlükler" olarak biliniyordu.

![Azure Etkinlik Günlüğü](./media/operational-security/azure-operational-security-fig7.png)

Etkinlik Günlüğü'nü kullanarak, aboneliğinizdeki kaynaklarda alınan herhangi bir yazma işlemi (PUT, POST, DELETE) için 'ne, kim ve ne zaman' olduğunu belirleyebilirsiniz. Ayrıca işlemin durumunu ve diğer ilgili özellikleri anlayabilirsiniz. Etkinlik Günlüğü, Klasik modeli kullanan kaynaklar için okuma (GET) işlemlerini veya işlemleri içermez.

### <a name="azure-diagnostic-logs"></a>Azure Tanı Günlükleri

Bu günlükler bir kaynak tarafından yayılır ve bu kaynağın çalışması hakkında zengin, sık veri sağlar. Bu günlüklerin içeriği kaynak türüne göre değişir.

Örneğin, Windows olay sistem günlükleri VM'ler için Tanıgünlüğü bir kategoridir ve blob, tablo ve sıra günlükleri depolama hesapları için Tanılama Günlükleri kategorileridir.

Tanılama Günlükleri, [Etkinlik Günlüğü'nden (eski adıyla Denetim Günlüğü veya Operasyon Günlüğü)](../../azure-monitor/platform/platform-logs-overview.md)farklıdır. Etkinlik günlüğü, aboneliğinizdeki kaynaklarüzerinde gerçekleştirilen işlemlerhakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisi tarafından gerçekleştirilen işlemler hakkında bilgi sağlar.

### <a name="metrics"></a>Ölçümler

Azure Monitor, Azure'daki iş yüklerinizin performansı ve sağlığı hakkında görünürlük elde etmek için telemetri tüketmenizi sağlar. Azure telemetri verilerinin en önemli türü, çoğu Azure kaynağı tarafından yayılan ölçümlerdir (performans sayaçları olarak da adlandırılır). Azure Monitor, izleme ve sorun [giderme](../../monitoring/monitoring-data-collection.md) için bu ölçümleri yapılandırmak ve tüketmek için çeşitli yollar sağlar. Ölçümler değerli bir telemetri kaynağıdır ve aşağıdaki görevleri yapmanızı sağlar:

-   Ölçümlerini bir portal grafiğine çizerek ve bu grafiği bir panoya sabitleyerek kaynağınızın performansını (VM, web sitesi veya mantık uygulaması gibi) **izleyin.**

-   Bir metrik belirli bir eşiği geçtiğinde kaynağınızın performansını etkileyen bir **sorundan haberdar olun.**

-   Bir metrik belirli bir eşiği geçtiğinde kaynağı otomatik ölçekleme veya runbook'u çalıştırma gibi **otomatik eylemleri yapılandırın.**

-   Kaynağınızın performans veya kullanım eğilimleri hakkında **gelişmiş analizler** veya raporlama gerçekleştirin.

-   Uyumluluk veya denetim amacıyla kaynağınızın performans veya sistem durumu geçmişini **arşivleyin.**

### <a name="azure-diagnostics"></a>Azure Tanılama

Dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlayan, Azure içindeki özelliktir. Tanılama uzantısını farklı kaynaklardan kullanabilirsiniz. Şu anda desteklenen [Azure Bulut Hizmeti Web ve İşçi Rolleri,](/visualstudio/azure/vs-azure-tools-configure-roles-for-cloud-service)Microsoft Windows çalıştıran Azure Sanal [Makineler](../../virtual-machines/windows/overview.md) ve [Hizmet Kumaşı.](../../azure-monitor/platform/diagnostics-extension-overview.md) Diğer Azure hizmetlerinin kendi ayrı tanılamaları vardır.

## <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Ağ güvenliğinizi denetlemek, ağ açıklarını algılamak ve BT güvenlik ve düzenleyici yönetim modelinize uygunluğu sağlamak için hayati önem taşır. Güvenlik Grubu görünümüyle, yapılandırılan Ağ Güvenlik Grubu'nu ve güvenlik kurallarını ve etkili güvenlik kurallarını alabilirsiniz. Uygulanan kurallar listesiyle, açık olan bağlantı noktalarını belirleyebilir ve ağ güvenlik açığını değerlendirebilirsiniz.

[Ağ İzleyicisi,](../../network-watcher/network-watcher-monitoring-overview.md) Azure'da ağ düzeyinde, azure'da ve Azure'dan koşulları izlemenize ve tanılamanıza olanak tanıyan bölgesel bir hizmettir. Ağ İzleyicisi ile kullanılabilen ağ tanılama ve görselleştirme araçları, Azure'da ağınızı anlamanıza, tanılamanıza ve öngörüler elde etmeye yardımcı olur. Bu hizmet paket yakalama, sonraki atlama, IP akışı doğrulamak, güvenlik grubu görünümü, NSG akış günlükleri içerir. Senaryo düzeyi izleme, tek tek ağ kaynağı izlemenin aksine ağ kaynaklarının uçtan uca görünümünü sağlar.

![Azure Ağ İzleyicisi](./media/operational-security/azure-operational-security-fig8.png)

Ağ İzleyicisi şu anda aşağıdaki özelliklere sahiptir:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Denetim Günlükleri</a>**- Ağların yapılandırmasının bir parçası olarak gerçekleştirilen işlemler günlüğe kaydedilir. Bu günlükler Azure portalında görüntülenebilir veya Power BI veya üçüncü taraf araçları gibi Microsoft araçları kullanılarak alınabilir. Denetim günlükleri portal, PowerShell, CLI ve Rest API üzerinden kullanılabilir. Denetim günlükleri hakkında daha fazla bilgi için Kaynak Yöneticisi ile Denetim işlemlerine bakın. Denetim günlükleri tüm ağ kaynaklarında yapılan işlemler için kullanılabilir.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP akışı doğrulanır</a>** - Akış bilgileri5-tuple paket parametrelerine (Hedef IP, Kaynak IP, Hedef Bağlantı Noktası, Kaynak Bağlantı Noktası ve Protokol) dayalı olarak bir pakete izin verilip verilmediğini veya reddedilip reddedilip reddedildiğini denetler. Paket bir Ağ Güvenlik Grubu tarafından reddedilirse, paketi reddeden kural ve Ağ Güvenliği Grubu döndürülür.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Sonraki atlama</a>** - Azure Ağ Dokusunda yönlendirilen paketler için bir sonraki atlamayı belirler ve yanlış yapılandırılmış kullanıcı tanımlı yolları tanılamanızı sağlar.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Güvenlik grubu görünümü</a>** - VM'de uygulanan etkili ve uygulanan güvenlik kurallarını alır.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">NSG Akış günlüğü</a>** - Ağ Güvenlik Grupları için akış günlükleri, gruptaki güvenlik kuralları tarafından izin verilen veya reddedilen trafikle ilgili günlükleri yakalamanızı sağlar. Akış 5-tuple bilgi ile tanımlanır - Kaynak IP, Hedef IP, Kaynak Bağlantı Noktası, Hedef Bağlantı Noktası ve Protokol.

## <a name="azure-storage-analytics"></a>Azure Depolama Analitiği

[Depolama Analitiği,](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) bir depolama hizmetine Talepler hakkında toplu işlem istatistikleri ve kapasite verilerini içeren ölçümler depolayabilir. Hareketler hem API işlem düzeyinde hem de depolama hizmeti düzeyinde raporlanır ve kapasite depolama hizmeti düzeyinde bildirilir. Ölçüm verileri, depolama hizmeti kullanımını çözümlemek, depolama hizmetine karşı yapılan isteklerle ilgili sorunları tanılamak ve bir hizmeti kullanan uygulamaların performansını artırmak için kullanılabilir.

[Azure Depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme yapar ve bir depolama hesabı için metrik verileri sağlar. Bu verileri istekleri izlemek, kullanım eğilimlerini çözümlemek ve depolama hesabınızla ilgili sorunları tanılamak için kullanabilirsiniz. Depolama Analizi günlüğü [Blob, Queue ve Tablo hizmetleri](../../storage/common/storage-introduction.md)için kullanılabilir. Depolama Analizi, bir depolama cihazına gönderilen başarılı ve başarısız isteklerle ilgili ayrıntılı bilgileri günlüğe kaydeder.

Bu bilgileri kullanarak istekleri ayrı ayrı izleyebilir ve depolama hizmetiyle ilgili sorunları tanılayabilirsiniz. İstekler en iyi çaba temelinde günlüğe kaydedilir. Günlük girişleri yalnızca hizmet bitiş noktasına karşı yapılan istekler varsa oluşturulur. Örneğin, bir depolama hesabının Blob bitiş noktasında etkinliği varsa, ancak Tablo veya Sıra bitiş noktalarında yoksa, yalnızca Blob hizmetiyle ilgili günlükler oluşturulur.

Depolama Analizi'ni kullanmak için, izlemek istediğiniz her hizmet için bunu ayrı ayrı etkinleştirmeniz gerekir. [Azure portalında](https://portal.azure.com/)etkinleştirebilirsiniz; ayrıntılar için Azure [portalındaki bir depolama hesabını izleyin'e](../../storage/common/storage-monitor-storage-account.md)bakın. Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi'ni programlı olarak etkinleştirebilirsiniz. Her hizmet için Depolama Analizi'ni ayrı ayrı etkinleştirmek için Hizmet Özelliklerini Ayarla işlemini kullanın.

Toplanan veriler, Blob hizmeti ve Tablo hizmeti API'leri kullanılarak erişilebilen tanınmış bir blob (günlüğe kaydetme için) ve tanınmış tablolarda (ölçümler için) depolanır.

Depolama Analitiği, depolama hesabınız için toplam sınırdan bağımsız olarak depolanan veri miktarı için 20-TB sınırına sahiptir. Tüm günlükler, depolama hesabı için Depolama Analitiği etkinleştirildiğinde otomatik olarak oluşturulan $logs adlı bir kapsayıcıda [blok bloblarında](../../storage/common/storage-analytics.md) depolanır.

Depolama Analizi tarafından gerçekleştirilen aşağıdaki eylemler faturalandırılabilir:

-   Günlüğe kaydetme için blobs oluşturma istekleri
-   Ölçümler için tablo varlıkları oluşturma istekleri.

> [!Note]
> Faturalandırma ve veri saklama ilkeleri hakkında daha fazla bilgi için Bkz. [Depolama Analizi ve Faturalandırma.](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing)
> En iyi performans için, olası azaltmayı önlemek için sanal makineye bağlı çok kullanılan disk sayısını sınırlamak istiyorsunuz. Tüm diskler aynı anda yüksek oranda kullanılıyorsa, depolama hesabi daha büyük bir numara diskini destekleyebilir.

> [!Note]
> Depolama hesabı sınırları hakkında daha fazla bilgi [için, standart depolama hesapları için Ölçeklenebilirlik hedeflerine](../../storage/common/scalability-targets-standard-account.md)bakın.


Aşağıdaki kimlik doğrulaması ve anonim istek türleri günlüğe kaydedilir.

| Kimlik doğrulaması  | Anonim|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|Zaman aşımı, azaltma, ağ, yetkilendirme hatalarını ve diğer hataları içeren başarısız istekler | Başarısız ve başarılı istekler de dahil olmak üzere Paylaşılan Erişim İmzası (SAS) kullanan istekler |
| Başarısız ve başarılı istekler de dahil olmak üzere Paylaşılan Erişim İmzası (SAS) kullanan istekler |Hem istemci hem de sunucuya yönelik zaman aşımı hataları |
|   Analiz verisi istekleri |    304 hata koduna sahip başarısız GET istekleri (Değiştirilmemiş) |
| Günlük oluşturma veya silme gibi Depolama Analitiği kendisi tarafından yapılan istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi Depolama [Analitiği Oturum Açmış İşlemler ve Durum İletileri ve](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) [Depolama Analizi Günlük Biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) konularında belgelenmiştir. | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi Depolama [Analitiği Oturum Açmış İşlemler ve Durum İletileri ve](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) [Depolama Analizi Günlük Biçimi'nde](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format)belgelenmiştir. |

## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD ayrıca çok faktörlü kimlik doğrulama, cihaz kaydı, self servis şifre yönetimi, self servis grup yönetimi, ayrıcalıklı hesap yönetimi, rol tabanlı erişim gibi tam bir kimlik yönetimi özellikleri paketi içerir kontrol, uygulama kullanım izleme, zengin denetim ve güvenlik izleme ve uyarı.

-   Azure AD çok faktörlü kimlik doğrulaması ve Koşullu Erişim ile uygulama güvenliğini artırın.

-   Güvenlik raporlaması ve izleme ile uygulama kullanımını izleyin ve işinizi gelişmiş tehditlere karşı koruyun.

Azure Active Directory (Azure AD), dizininize yönelik güvenlik, etkinlik ve denetim raporlarını içerir. [Azure Etkin Dizin Denetim Raporu,](../../active-directory/active-directory-reporting-azure-portal.md) müşterilerin Azure Etkin Dizini'nde gerçekleşen ayrıcalıklı eylemleri belirlemelerine yardımcı olur. Ayrıcalıklı eylemler arasında yükseklik değişiklikleri (örneğin, rol oluşturma veya parola sıfırlama), ilke yapılandırmalarını değiştirme (örneğin parola ilkeleri) veya dizin yapılandırmasına yapılan değişiklikler (örneğin, etki alanı federasyonu ayarlarındaki değişiklikler) yer alır.

Raporlar, olay adının denetim kaydını, eylemi gerçekleştiren aktörü, değişiklikten etkilenen hedef kaynağı ve tarih ve saati (UTC'de) sağlar. Müşteriler, Azure Etkin Dizini için [denetim etkinliklerilistesini, Denetim Günlüklerinizi Görüntüle'de](../../active-directory/reports-monitoring/overview-reports.md)açıklandığı şekilde [Azure portalı](https://portal.azure.com/)üzerinden alabilirler. Kapsama dahil olan raporların listesi şu şekildedir:

| Güvenlik raporları  | Etkinlik raporları| Denetim raporları |
| :------------- | :-------------| :-------------|
|Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri | Uygulama kullanımı: özet | Dizin denetimi raporu |
|Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri | Uygulama kullanımı: ayrıntılı |   |
|Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri | Uygulama panosu |  |
|Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |Hesap hazırlama hataları |  |
|Düzensiz oturum açma etkinliği |Bireysel kullanıcı cihazları |  |
|Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri |Bireysel kullanıcı Etkinliği |   |
|Anormal oturum açma etkinliği gösteren kullanıcılar |Grup etkinlik raporu |   |
| |Parola Sıfırlama Kayıt Etkinlik Raporu |   |
| |Parola sıfırlama etkinliği |   |



Bu raporların verileri, SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalarınız için yararlı olabilir. Azure AD raporlama [API'leri,](../../active-directory/active-directory-reporting-api-getting-started-azure-portal.md) REST tabanlı API'ler kümesi aracılığıyla verilere programlı erişim sağlar. Bu API'leri çeşitli programlama dillerinden ve araçlarından arayabilirsiniz.

Azure AD Denetimi raporundaki olaylar 180 gün boyunca saklanır.

> [!Note]
> Raporlarda bekletme hakkında daha fazla bilgi için [Azure Etkin Dizin Raporu Bekletme İlkeleri'ne](../../active-directory/reports-monitoring/reference-reports-data-retention.md)bakın.

[Denetim etkinliklerini](../../active-directory/active-directory-reporting-activity-audit-logs.md) daha uzun bekletme süreleri için depolamak isteyen müşteriler için Raporlama API'si, denetim olaylarını düzenli olarak ayrı bir veri deposuna çekmek için kullanılabilir.

## <a name="summary"></a>Özet

Bu makalede, kuruluşunuzun BT altyapısını yönetmenize yardımcı olan yazılım ve hizmetler sunarken gizliliğinizi koruyan ve verilerinizin güvenliğini sağlayan özetler yer alıyor. Microsoft, verilerini başkalarına emanet ettiklerinde, bu güvenin sıkı güvenlik gerektirdiğini kabul eder. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. Verilerin güvenliğini sağlamak ve korumak Microsoft'un en önemli önceliklerini dir.

Bu makalede, açıklar

-   Azure Monitor paketinde verilerin nasıl toplandığı, işlendiği ve güvenli hale edildiği.

-   Birden çok veri kaynağında olayları hızla analiz edin. Güvenlik risklerini belirleyin ve bir güvenlik ihlalinin zararını azaltmak için tehdit ve saldırıların kapsamını ve etkisini anlayın.

-   Giden kötü amaçlı IP trafiğini ve kötü amaçlı tehdit türlerini görselleştirerek saldırı desenlerini belirleyin. Platformdan bağımsız olarak tüm çevrenizin güvenlik duruşunu anlayın.

-   Güvenlik veya uyumluluk denetimi için gereken tüm günlük ve olay verilerini yakalayın. Tam, aranabilir ve dışa aktarılabilir günlük ve olay veri kümesiyle bir güvenlik denetimi sağlamak için gereken zamanı ve kaynakları kısın.

<ul>
<li>Varlıklarınızı yakından takip etmek için güvenlikle ilgili olayları, denetimi ve ihlal çözümlemesi toplayın:</li>
<ul>
<li>Güvenlik duruşu</li>
<li>Önemli konu</li>
<li>Özetler tehditleri</li>
</ul>
</ul>

## <a name="next-steps"></a>Sonraki Adımlar

- [Tasarım ve operasyonel güvenlik](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft, bulut altyapısının esnek olmasını ve saldırılara karşı korunmasını sağlamaya yardımcı olmak için hizmetlerini ve yazılımlarını güvenlik göz önünde bulundurarak tasarlar.

- [Azure Monitör günlükleri | Güvenlik & Uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance)

Daha akıllı ve etkili tehdit algılama gerçekleştirmek için Microsoft güvenlik verilerini ve çözümlemesi kullanın.

- [Azure Güvenlik Merkezi planlama ve işlemleri](../../security-center/security-center-planning-and-operations-guide.md) Kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline göre Güvenlik Merkezi kullanımınızı optimize etmek için izleyebileceğiniz adımlar ve görevler kümesi.

