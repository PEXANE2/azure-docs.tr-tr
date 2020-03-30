---
title: Azure gelişmiş tehdit algılama | Microsoft Dokümanlar
description: Azure AD Kimlik Koruması ve yetenekleri hakkında bilgi edinin.
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
ms.openlocfilehash: 3c1c385a87fc302d180729ec2e4bcd1c4a315f6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981460"
---
# <a name="azure-advanced-threat-detection"></a>Azure gelişmiş tehdit algılama

Azure, Azure Active Directory (Azure AD), Azure Monitor günlükleri ve Azure Güvenlik Merkezi gibi hizmetler aracılığıyla gelişmiş tehdit algılama işlevlerinde yerleşik olarak sunulur. Bu güvenlik hizmetleri ve yetenekleri koleksiyonu, Azure dağıtımlarınızda neler olduğunu anlamanın basit ve hızlı bir yolunu sağlar.

Azure, uygulama dağıtımlarınızın gereksinimlerini karşılayacak şekilde güvenliği yapılandırmak ve özelleştirmek için çok çeşitli seçenekler sunar. Bu makalede, bu gereksinimlerin nasıl karşılanılacağI

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması

[Azure AD Kimlik Koruması,](../../active-directory/identity-protection/overview.md) kuruluşunuzun kimliklerini etkileyebilecek risk algılamalarına ve olası güvenlik açıklarına genel bir bakış sağlayan bir [Azure Active Directory Premium P2](../../active-directory/active-directory-whatis.md) sürümü dür. Kimlik Koruması, [Azure AD Anormal Etkinlik Raporları](../../active-directory/active-directory-reporting-azure-portal.md)aracılığıyla kullanılabilen mevcut Azure AD anomali algılama özelliklerini kullanır ve gerçek zamanlı anormallikleri algılayabilen yeni risk algılama türleri sunar.

![Azure AD Kimlik Koruma diyagramı](./media/threat-detection/azure-threat-detection-fig1.png)

Kimlik Koruması, bir kimliğin ele geçirildiğini gösterebilecek anormallikleri ve risk algılamalarını tespit etmek için uyarlanabilir makine öğrenme algoritmaları ve buluşsal algoritmalar kullanır. Kimlik Koruması bu verileri kullanarak, bu risk algılamalarını inceleyebilmeniz ve uygun düzeltme veya azaltma eylemini gerçekleştirebilmeniz için raporlar ve uyarılar oluşturur.

Azure Active Directory Identity Protection bir izleme ve raporlama aracından daha fazlasıdır. Kimlik Koruması, risk algılamalarına bağlı olarak her kullanıcı için bir kullanıcı risk düzeyi hesaplar, böylece kuruluşunuzun kimliklerini otomatik olarak korumak için risk tabanlı ilkeleri yapılandırabilirsiniz.

Bu risk tabanlı ilkeler, Azure Etkin Dizin ve [EMS](../../active-directory/active-directory-conditional-access-azure-portal.md)tarafından sağlanan diğer [Koşullu Erişim denetimlerine](../../active-directory/active-directory-conditional-access-azure-portal.md) ek olarak, parola sıfırlama ve çok faktörlü kimlik doğrulama zorlamasını içeren uyarlanabilir düzeltme eylemlerini otomatik olarak engelleyebilir veya sunabilir.

### <a name="identity-protection-capabilities"></a>Kimlik Koruma özellikleri

Azure Active Directory Identity Protection bir izleme ve raporlama aracından daha fazlasıdır. Kuruluşunuzun kimliklerini korumak için, belirli bir risk düzeyine ulaşıldığında algılanan sorunlara otomatik olarak yanıt veren risk tabanlı ilkeleri yapılandırabilirsiniz. Bu ilkeler, Azure Etkin Dizini ve EMS tarafından sağlanan diğer Koşullu Erişim denetimlerine ek olarak, parola sıfırlama ve çok faktörlü kimlik doğrulama zorlama dahil olmak üzere uyarlamalı düzeltme eylemlerini otomatik olarak engelleyebilir veya başlatabilir.

Azure Kimlik Koruması'nın hesaplarınızın ve kimliklerinizin güvenliğini sağlamanıza yardımcı olabileceği yöntemlerden bazılarına örnek olarak şunlar verilebilir:

[Risk tespitlerinin ve riskli hesapların saptanması](../../active-directory/identity-protection/overview.md)
-   Makine öğrenimi ve sezgisel kuralları kullanarak altı risk algılama türünü tespit edin.
-   Kullanıcı risk düzeylerini hesaplayın.
-   Güvenlik açıklarını vurgulayarak genel güvenlik duruşunu iyileştirmek için özel öneriler sağlayın.

[Risk tespitlerinin araştırılması](../../active-directory/identity-protection/overview.md)
-   Risk algılamaları için bildirim gönderin.
-   İlgili ve bağlamsal bilgileri kullanarak risk algılamalarını araştırın.
-   İncelemeleri izlemek için temel iş akışları sağlayın.
-   Parola sıfırlama gibi düzeltme eylemlerine kolay erişim sağlayın.

[Risk tabanlı, koşullu erişim ilkeleri](../../active-directory/identity-protection/overview.md)
-   Oturum açma oturumlarını engelleyerek veya çok faktörlü kimlik doğrulama zorlukları gerektirerek riskli oturum açmaları azaltın.
-   Riskli kullanıcı hesaplarını engelleyin veya güvenli hale bulundu.
-   Kullanıcıların çok faktörlü kimlik doğrulama için kaydolmasını zorunlu kılmasını zorunlu kılmaktadır.

### <a name="azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management

[Azure Active Directory Ayrıcalıklı Kimlik Yönetimi (PIM)](../../active-directory/privileged-identity-management/pim-configure.md)ile kuruluşunuzdaki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Bu özellik, Azure AD'deki kaynaklara ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi içerir.

![Azure AD Ayrıcalıklı Kimlik Yönetimi diyagramı](./media/threat-detection/azure-threat-detection-fig2.png)

PIM size yardımcı olur:

-   Azure AD yöneticileri ve Office 365 ve Intune gibi Microsoft çevrimiçi hizmetlerine tam zamanında (JIT) yönetim erişimi hakkında uyarılar ve raporlar alın.

-   Yönetici erişim geçmişi ve yönetici atamalarındaki değişiklikler hakkında raporlar alın.

-   Ayrıcalıklı bir role erişim konusunda uyarılar alın.

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure Monitor günlükleri,](../../azure-monitor/index.yml) şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan Microsoft bulut tabanlı bt yönetimi çözümüdür. Azure Monitor günlükleri bulut tabanlı bir hizmet olarak uygulandığından, altyapı hizmetlerine en az yatırımla bu hizmeti hızlı bir şekilde çalışır hale getirebilirsiniz. Yeni güvenlik özellikleri otomatik olarak teslim edilir ve devam eden bakım ve yükseltme maliyetleri tasarrufu sağlar.

Azure Monitor günlükleri, değerli hizmetleri kendi başına sağlamanın yanı sıra, mevcut güvenlik yönetimi yatırımlarınızı buluta genişletmek için [Sistem Merkezi Operasyon Yöneticisi](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/)gibi Sistem Merkezi bileşenleriyle tümleştirilebilir. Sistem Merkezi ve Azure Monitör günlükleri tam bir karma yönetim deneyimi sağlamak için birlikte çalışabilir.

### <a name="holistic-security-and-compliance-posture"></a>Bütünsel güvenlik ve uyumluluk duruşu

[Log Analytics Security and Audit panosu,](../../security-center/security-center-intro.md) dikkatinizi gerektiren önemli sorunlar için yerleşik arama sorgularıyla kuruluşunuzun BT güvenlik duruşuna kapsamlı bir görünüm sağlar. Güvenlik ve Denetim panosu, Azure Monitor günlüklerinde güvenlikle ilgili her şeyin ana ekranıdır. Bu pano, size bilgisayarlarınızın güvenlik durumuyla ilgili yüksek düzeyde öngörü sağlar. Ayrıca, son 24 saat, 7 gün veya diğer özel zaman dilimlerindeki tüm olayları görüntüleyebilirsiniz.

Azure Monitor günlükleri, yazılım güncelleştirme değerlendirmesi, kötü amaçlı yazılımdan koruma değerlendirmesi ve yapılandırma taban çizgileri de dahil olmak üzere BT İşlemleri bağlamında tüm ortamın genel güvenlik duruşunu hızlı ve kolay bir şekilde anlamanıza yardımcı olur. Güvenlik günlüğü verilerine, güvenlik ve uyumluluk denetim süreçlerini kolaylaştırmak için kolayca erişilebilir.

![Log Analytics Güvenlik ve Denetim panosu](./media/threat-detection/azure-threat-detection-fig3.jpg)

Log Analytics Security and Audit panosu dört ana kategoride düzenlenir:

-   **Güvenlik Etki Alanları**: Güvenlik kayıtlarını zaman içinde daha fazla keşfetmenizi sağlar; kötü amaçlı yazılım değerlendirmelerine erişin; değerlendirmeleri güncellemek; ağ güvenliğini, kimliğini ve erişim bilgilerini görüntülemek; güvenlik olayları olan bilgisayarları görüntüleme; ve Azure Güvenlik Merkezi panosuna hızla erişin.

-   **Önemli Sorunlar**: Etkin sorunların sayısını ve sorunların önem derecesini hızlı bir şekilde belirlemenizi sağlar.

-   **Algılamalar (Önizleme)**: Kaynaklarınıza karşı güvenlik uyarıları görüntüleyerek saldırı desenlerini belirlemenizi sağlar.

-   **Tehdit İstihbaratı**: Giden kötü amaçlı IP trafiğine, kötü amaçlı tehdit türüne ve IP konumlarının haritasına sahip toplam sunucu sayısını görüntüleyerek saldırı modellerini belirlemenizi sağlar.

-   **Sık karşılaşılan güvenlik sorguları**: Ortamınızı izlemek için kullanabileceğiniz en yaygın güvenlik sorgularını listeler. Herhangi bir sorgu seçtiğinizde, Arama bölmesi açılır ve bu sorgunun sonuçlarını görüntüler.

### <a name="insight-and-analytics"></a>Öngörü ve analitik
Azure Monitor [günlüklerinin](../../log-analytics/log-analytics-queries.md) merkezinde, Azure tarafından barındırılan depo bulunmaktadır.

![Öngörü ve analiz diyagramı](./media/threat-detection/azure-threat-detection-fig4.png)

Veri kaynaklarını yapılandırarak ve aboneliğinize çözümler ekleyerek bağlı kaynaklardan depoya veri toplarsınız.

![Azure Monitörü oturum açar panosu](./media/threat-detection/azure-threat-detection-fig5.png)

Veri kaynakları ve çözümleri her biri kendi özellikleri kümesi ile ayrı kayıt türleri oluşturmak, ancak yine de deposorgularında bunları birlikte çözümleyebilirsiniz. Çeşitli kaynaklar tarafından toplanan çeşitli verilerle çalışmak için aynı araçları ve yöntemleri kullanabilirsiniz.


Azure Monitor günlükleriyle etkileşiminizin çoğu, herhangi bir tarayıcıda çalışan ve toplanan verileri analiz etmek ve bunları etkinleştirmek için yapılandırma ayarlarına ve birden çok araça erişim sağlayan Azure portalı aracılığıyla yapılır. Portaldan şunları kullanabilirsiniz:
* Toplanan verileri çözümlemek için sorgular oluşturacağınız [aramaları günlüğe](../../log-analytics/log-analytics-queries.md) kaydedin.
* En değerli aramalarınızın grafik görünümleriyle özelleştirebileceğiniz [panolar.](../../azure-monitor/learn/tutorial-logs-dashboards.md)
* Ek işlevsellik ve analiz araçları sağlayan [çözümler.](../../monitoring/monitoring-solutions.md)

![Analiz araçları](./media/threat-detection/azure-threat-detection-fig6.png)

Çözümler Azure Monitor günlüklerine işlevsellik ekler. Bunlar öncelikle bulutta çalışır ve günlük analizi deposunda toplanan verilerin analizini sağlar. Çözümler ayrıca, günlük aramalarıyla veya çözümün günlük analizi panosunda sağladığı ek bir kullanıcı arabirimi kullanılarak analiz edilebilen toplanacak yeni kayıt türleri de tanımlayabilir.

Güvenlik ve Denetim panosu bu tür çözümlere bir örnektir.

### <a name="automation-and-control-alert-on-security-configuration-drifts"></a>Otomasyon ve denetim: Güvenlik yapılandırma sıyrıklarına karşı uyarı

Azure Otomasyonu, yönetim süreçlerini PowerShell'i temel alan ve bulutta çalışan runbook'larla otomatikleştirir. Runbook'lar yerel kaynakların yönetilmesi için yerel veri merkezinizdeki bir sunucuda da yürütülebilir. Azure Otomasyonu PowerShell İstenilen Durum Yapılandırması (DSC) ile yapılandırma yönetimi sağlar.

![Azure Otomasyon diyagramı](./media/threat-detection/azure-threat-detection-fig7.png)

Azure'da barındırılan DSC kaynaklarını oluşturabilir ve yönetebilir ve bunları bulut ve şirket içi sistemlere uygulayabilirsiniz. Bunu yaparak, güvenlik yapılandırmalarının ilke içinde kalmasını sağlamaya yardımcı olmak için yapılandırmalarını tanımlayabilir ve otomatik olarak uygulayabilir veya sürüklenme yle ilgili raporlar alabilirsiniz.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Azure kaynaklarınızın korunmasına yardımcı olur. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Hizmet içinde, daha fazla parçalıbilgi için polisleri hem Azure aboneliklerinize hem de [kaynak gruplarınıza](../../azure-resource-manager/management/manage-resources-portal.md) göre tanımlayabilirsiniz.

![Azure Güvenlik Merkezi diyagramı](./media/threat-detection/azure-threat-detection-fig8.png)

Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Bunlar Microsoft’un bulut ve şirket içindeki genel varlığından edinilen kapsamlı bir telemetri kümesine erişebilmektedir. Geniş kapsamlı ve çeşitlilik barındıran bu veri kümeleri Microsoft’un yeni saldırı modellerini ve şirket içi müşteri ve kuruluş ürünlerinin yanı sıra çevrimiçi hizmetleri üzerindeki eğilimleri keşfetmesini sağlamaktadır.

Böylece, Güvenlik Merkezi, saldırganlar yeni ve giderek daha karmaşık açıkları serbest olarak algılama algoritmaları hızla güncelleyebilirsiniz. Bu yaklaşım, hızlı hareket eden tehdit ortamına ayak uydurmanıza yardımcı olur.

![Güvenlik Merkezi tehdit algılama](./media/threat-detection/azure-threat-detection-fig9.jpg)

Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik verilerini otomatik olarak toplayarak çalışır. Tehditleri belirlemek için birden fazla kaynaktan gelen bilgileri ilişkilendiren bu bilgileri analiz eder.

Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri ve [makine öğrenimi](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojilerinde yapılan atılımlar, tüm bulut dokusundaki olayları değerlendirmek için kullanılır. Gelişmiş analizler, manuel yaklaşımlar la ve saldırıların evrimini öngörerek tanımlanması imkansız olan tehditleri algılayabilir. Bu güvenlik analizi türleri sonraki bölümlerde ele alınmıştır.

### <a name="threat-intelligence"></a>Tehdit bilgileri

Microsoft, çok büyük miktarda küresel tehdit istihbaratına erişebilir.

Telemetri, Azure, Office 365, Microsoft CRM çevrimiçi, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Dijital Suçlar Birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden çok kaynaktan akar.

![Tehdit istihbaratı bulguları](./media/threat-detection/azure-threat-detection-fig10.jpg)

Araştırmacılar ayrıca, büyük bulut hizmeti sağlayıcıları arasında paylaşılan tehdit istihbaratı bilgilerini alır ve üçüncü taraflardan gelen tehdit istihbarat beslemelerine abone olurlar. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir. Bazı örnekler:

-   **Makine öğreniminin gücünden yararlanma**: Azure Güvenlik Merkezi, Azure dağıtımlarınızı hedefleyen tehditleri algılamak için kullanılabilecek bulut ağı etkinliği yle ilgili çok büyük miktarda veriye erişebilir.

-   **Kaba kuvvet algılama**: Makine öğrenimi, Secure Shell (SSH), Remote Desktop Protocol (RDP) ve SQL bağlantı noktalarına karşı kaba kuvvet saldırılarını algılamasına olanak tanıyan, uzaktan erişim girişimlerinin tarihsel bir modelini oluşturmak için kullanılır.

-   **Giden DDoS ve botnet algılama**: Bulut kaynaklarını hedef alan saldırıların ortak bir amacı, diğer saldırıları gerçekleştirmek için bu kaynakların işlem gücünü kullanmaktır.

-   **Yeni davranış analizi sunucuları ve Sanal Bilgisayarlar**: Bir sunucu veya sanal makine ele geçirildikten sonra, saldırganlar algılamadan kaçınırken, kalıcılığı sağlarken ve güvenlik denetimlerini geçersiz kılırken, o sistemde kötü amaçlı kod yürütmek için çok çeşitli teknikler kullanırlar.

-   **Azure SQL Veritabanı Tehdit Algılama**: Veritabanlarına erişmek veya veritabanlarından yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal veritabanı etkinliklerini tanımlayan Azure SQL Veritabanı için tehdit algılama.

### <a name="behavioral-analytics"></a>Davranış analizi

Davranış analizi, verileri analiz eden ve bilinen modeller koleksiyonuyla karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir.

![Davranış analizi bulguları](./media/threat-detection/azure-threat-detection-fig11.jpg)

Desenler de uzman analistler tarafından kötü niyetli davranışların dikkatli analizi ile belirlenir. Azure Güvenlik Merkezi, sanal makine günlüklerinin, sanal ağ aygıtı günlüklerinin, kumaş günlüklerinin, kilitlenme dökümlerinin ve diğer kaynakların analizini temel alan, tehlikeye atılan kaynakları belirlemek için davranış analizini kullanabilir.

Buna ek olarak, desenler yaygın bir kampanyanın destekleyici kanıtlarını denetlemek için diğer sinyallerle ilişkilidir. Bu bağıntı yerleşik tehlike göstergeleriyle tutarlı olayları tanımlamaya yardımcı olur.

Bazı örnekler:
-   **Şüpheli işlem yürütme**: Saldırganlar tespit edilmeden kötü amaçlı yazılım yürütmeye yönelik çeşitli teknikler kullanmaktadır. Örneğin, bir saldırgan kötü amaçlı yazılıma yasal sistem dosyalarıyla aynı adları verebilir, ancak bu dosyaları alternatif bir konuma yerleyebilir, iyi huylu bir dosyanınkine benzer bir ad kullanabilir veya dosyanın gerçek uzantısını maskeleyebilir. Güvenlik Merkezi modelleri, bu gibi aykırılıkları algılamak için davranışları ve işlem yürütmelerini izler.

-   **Gizli kötü amaçlı yazılım ve yararlanma girişimleri**: Gelişmiş kötü amaçlı yazılımlar, diske hiç yazmayarak veya diskte depolanan yazılım bileşenlerini şifreleyerek geleneksel kötü amaçlı yazılımdan koruma ürünlerinden kaçınabilir. Ancak, bu tür kötü amaçlı yazılımlar bellek çözümlemesi kullanılarak algılanabilir, çünkü kötü amaçlı yazılım bellekte işlevlerini bırakmak zorundadır. Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar. Azure Güvenlik Merkezi, kilitlenme dökümündeki belleği analiz ederek, yazılımdaki güvenlik açıklarından yararlanmak, gizli verilere erişmek ve gizliliğiniz tehlikeye giren bir makinede performansınızı etkilemeden gizlice devam etmek için kullanılan teknikleri algılayabilir Makine.

-   **Yanal hareket ve dahili keşif**: Tehlikeye atılmış bir ağda devam etmek ve değerli verileri bulmak ve toplamak için saldırganlar genellikle aynı ağ içinde tehlikeye makineden başkalarına yanal hareket etmeye çalışırlar. Güvenlik Merkezi, uzaktan komut yürütme, ağ sondalama ve hesap numaralandırma gibi saldırganın ağ içindeki dayanağını genişletme girişimlerini bulmak için süreci ve oturum açma etkinliklerini izler.

-   **Kötü amaçlı PowerShell komut dosyaları**: PowerShell saldırganlar tarafından çeşitli amaçlariçin hedef sanal makinelerde kötü amaçlı kod yürütmek için kullanılabilir. Güvenlik Merkezi şüpheli etkinliklerin kanıtı için PowerShell etkinliğini inceler.

-   **Giden saldırılar**: Saldırganlar genellikle bulut kaynaklarını ek saldırılar yerleştirmek üzere kullanma amacıyla bulut kaynaklarını hedefler. Örneğin, tehlikeye giren sanal makineler, diğer sanal makinelere karşı kaba kuvvet saldırıları başlatmak, spam göndermek veya açık bağlantı noktalarını ve internet üzerindeki diğer aygıtları tarayan makineler için kullanılabilir. Ağ trafiğine machine learning uygulayan Güvenlik Merkezi giden ağ iletişimlerinin normu aştığını algılayabilir. Spam algılandığında, Güvenlik Merkezi, postanın büyük olasılıkla hain mi yoksa meşru bir e-posta kampanyasının sonucu mu olduğunu belirlemek için olağandışı e-posta trafiğini Office 365'in istihbaratıyla da ilişkilendirer.

### <a name="anomaly-detection"></a>Anormallik algılama

Azure Güvenlik Merkezi, tehditleri tanımlamak için anormallik algılamayı da kullanır. Davranış analizinden (büyük veri kümelerinden türetilmiş bilinen modellere bağlıdır) farklı olarak anormallik algılama daha fazla “kişiselleştirilmiştir” ve dağıtımlarınıza özel taban çizgilerine odaklanır. Makine öğrenimi, dağıtımlarınız için normal etkinliği belirlemek için uygulanır ve ardından bir güvenlik olayını temsil edebilecek aykırı koşulları tanımlamak için kurallar oluşturulur. Bir örneği aşağıda verilmiştir:

-   **Gelen RDP/SSH kaba kuvvet saldırıları**: Dağıtımlarınızın her gün çok sayıda girişi olan meşgul sanal makineleri ve varsa az sayıda girişi olan diğer sanal makineler olabilir. Azure Güvenlik Merkezi, bu sanal makineler için temel oturum açma etkinliğini belirleyebilir ve normal oturum açma etkinliklerini tanımlamak için makine öğrenimini kullanabilir. Oturum açma yla ilgili özellikler için tanımlanan taban çizgisiyle herhangi bir tutarsızlık varsa, bir uyarı oluşturulabilir. Yine machine learning neyin önemli olduğunu belirler.

### <a name="continuous-threat-intelligence-monitoring"></a>Sürekli tehdit bilgisi izleme

Azure Güvenlik Merkezi, tehdit ortamındaki değişiklikleri sürekli olarak izleyen tüm dünyada güvenlik araştırma ve veri bilimi ekipleriyle çalışır. Buna aşağıdaki girişimler dahildir:

-   **Tehdit istihbaratı izleme**: Tehdit istihbaratı, mevcut veya ortaya çıkan tehditler hakkında mekanizmalar, göstergeler, etkileri ve eyleme geçirilebilir tavsiyeler içerir. Bu bilgiler güvenlik topluluğunda paylaşılır ve Microsoft tehdit istihbaratı akışlarını sürekli olarak iç ve dış kaynaklardan izler.

-   **Sinyal paylaşımı**: Microsoft'un geniş bulut ve şirket içi hizmetler, sunucular ve istemci uç noktası aygıtları portföyündeki güvenlik ekiplerinin öngörüleri paylaşılır ve analiz edilir.

-   **Microsoft güvenlik uzmanları**: Microsoft genelinde adli tıp ve web saldırısı algılama gibi özel güvenlik alanlarında çalışan ekiplerle sürekli etkileşim.

-   **Algılama atonlama**: Algoritmalar gerçek müşteri veri kümelerine karşı çalıştırılır ve güvenlik araştırmacıları sonuçları doğrulamak için müşterilerle birlikte çalışır. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu birleşik çabalar, anında yararlanabileceğiniz yeni ve geliştirilmiş algılamalarla sonuçlanır. Yapacak bir şey yok.

## <a name="advanced-threat-detection-features-other-azure-services"></a>Gelişmiş tehdit algılama özellikleri: Diğer Azure hizmetleri

### <a name="virtual-machines-microsoft-antimalware"></a>Sanal makineler: Microsoft antimalware

Azure için [Microsoft kötü amaçlı yazılımdan koruma,](antimalware.md) uygulamalar ve kiracı ortamları için insan müdahalesi olmadan arka planda çalışmak üzere tasarlanmış tek aracılı bir çözümdür. Kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere temel güvenli veya gelişmiş özel yapılandırmayla uygulama iş yüklerinizin gereksinimlerine göre koruma dağıtabilirsiniz. Azure kötü amaçlı yazılımdan koruma, tüm Azure PaaS sanal makinelerine otomatik olarak yüklenen Azure sanal makineleri için bir güvenlik seçeneğidir.

#### <a name="microsoft-antimalware-core-features"></a>Microsoft kötü amaçlı yazılımdan koruma temel özellikleri

Azure'un uygulamalarınız için Microsoft kötü amaçlı yazılımdan koruma özelliğini dağıtan ve etkinleştiren özellikleri şunlardır:

-   **Gerçek zamanlı koruma**: Kötü amaçlı yazılımların yürütülmesini tespit etmek ve engellemek için bulut hizmetlerinde ve sanal makinelerdeki etkinlikleri izler.

-   **Planlanmış tarama**: Etkin olarak çalışan programlar da dahil olmak üzere kötü amaçlı yazılımları tespit etmek için düzenli olarak hedefli tarama yapar.

-   **Kötü amaçlı yazılım düzeltme**: Kötü amaçlı dosyaları silerken veya karantinaya almak ve kötü amaçlı kayıt defteri girişlerini temizlemek gibi algılanan kötü amaçlı yazılımlar üzerinde otomatik olarak hareket eder.

-   **İmza güncelleştirmeleri**: Korumanın önceden belirlenmiş bir frekansta güncel olduğundan emin olmak için en son koruma imzalarını (virüs tanımları) otomatik olarak yükler.

-   **Kötü Amaçlı Yazılımdan Koruma Motoru güncelleştirmeleri**: Microsoft Antimalware Engine'i otomatik olarak güncelleştirir.

-   **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri**: Microsoft kötü amaçlı yazılımdan koruma platformlarını otomatik olarak günceller.

-   **Etkin koruma**: Microsoft etkin koruma sistemi aracılığıyla gerçek zamanlı eşzamanlı imza teslimini etkinleştirerek, algılanan tehdit ortamına hızlı yanıt vermek için algılanan tehditler ve şüpheli kaynaklarla ilgili telemetri meta verilerini Microsoft Azure'a bildirir.

-   **Örnekler raporlama**: Hizmeti geliştirmeye ve sorun gidermeye yardımcı olmak için örnekleri Microsoft kötü amaçlı yazılımdan koruma hizmetine sağlar ve raporlar.

-   **Dışlamalar**: Uygulama ve hizmet yöneticilerinin belirli dosyaları, süreçleri ve sürücüleri performans ve diğer nedenlerle koruma ve taramadan muaf tutulmak üzere yapılandırmalarına olanak tanır.

-   **Kötü amaçlı yazılımdan koruma olayı koleksiyonu**: İşletim sistemi olay günlüğünde gerçekleştirilen kötü amaçlı yazılımdan koruma hizmeti durumunu, şüpheli etkinlikleri ve düzeltme eylemlerini kaydeder ve müşterinin Azure depolama hesabına toplar.

### <a name="azure-sql-database-threat-detection"></a>Azure SQL Veritabanı Tehdit Algılama

[Azure SQL Veritabanı Tehdit Algılama,](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) Azure SQL Veritabanı hizmetinde yerleşik olarak yerleşik yeni bir güvenlik zekası özelliğidir. Anormal veritabanı etkinliklerini öğrenmek, profilini çıkarmak ve algılamak için 24 saat çalışan Azure SQL Veritabanı Tehdit Algılaması, veritabanına yönelik olası tehditleri tanımlar.

Güvenlik görevlileri veya diğer atanmış yöneticiler, şüpheli veritabanı etkinlikleri hakkında anında bildirim de alabilir. Her bildirim şüpheli etkinliğin ayrıntılarını sağlar ve tehdidi nasıl daha fazla araştırıp azledeceğimi önerir.

Şu anda, Azure SQL Veritabanı Tehdit Algılama olası güvenlik açıklarını ve SQL enjeksiyon saldırıları ve anormal veritabanı erişim desenleri algılar.

Tehdit algılama e-posta bildirimi aldıktan sonra, kullanıcılar postadaki derin bir bağlantı aracılığıyla ilgili denetim kayıtlarında gezinebilir ve görüntüleyebilir. Bağlantı, aşağıdakilere göre, şüpheli olayın olduğu sıralarda ilgili denetim kayıtlarını gösteren bir denetim görüntüleyicisi veya önceden yapılandırılmış bir denetim Excel şablonu açar:

-   Anormal veritabanı etkinlikleri ile veritabanı/sunucu için denetim depolama.

-   Denetim günlüğü yazmak için olay sırasında kullanılan ilgili denetim depolama tablosu.

-   Olay meydana gelişini hemen izleyen saatin denetim kayıtları.

-   Olay sırasında benzer bir olay kimliğine sahip denetim kayıtları (bazı dedektörler için isteğe bağlı).

SQL Veritabanı tehdit dedektörleri aşağıdaki algılama yöntemlerinden birini kullanır:

-   **Deterministic algılama**: Bilinen saldırılarla eşleşen SQL istemci sorgularında şüpheli desenleri (kurallara dayalı) algılar. Bu metodoloji yüksek algılama ve düşük yanlış pozitif, ancak sınırlı kapsama alanı vardır çünkü "atomik algılamalar" kategorisine girer.

-   **Davranışsal algılama**: Veritabanında en son 30 gün içinde görülmeyen anormal davranışı algılar. SQL istemcia anormal etkinlik örnekleri başarısız oturum açma veya sorgular, çıkarılan yüksek miktarda veri, olağandışı kanonik sorgular veya veritabanına erişmek için kullanılan yabancı IP adresleri nin artması olabilir.

### <a name="application-gateway-web-application-firewall"></a>Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı

[Web Application Güvenlik Duvarı (WAF),](../../app-service/environment/app-service-app-service-environment-web-application-firewall.md) standart [uygulama teslim denetimi](https://kemptechnologies.com/in/application-delivery-controllers) işlevleri için uygulama ağ geçidi kullanan web uygulamalarına koruma sağlayan Azure Uygulama Ağ [Geçidi'nin](../../application-gateway/application-gateway-web-application-firewall-overview.md) bir özelliğidir. Web Application Firewall [Açık Web Uygulama Güvenlik Projesi (OWASP) en yaygın 10 ortak web güvenlik açıkları](https://www.owasp.org/index.php/Top_10_2010-Main)onları koruyarak bunu yapar.

![Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı diyagramı](./media/threat-detection/azure-threat-detection-fig13.png)

Korumalar şunlardır:

-   SQL enjeksiyon koruması.

-   Çapraz site komut dosyası koruması.

-   Komut enjeksiyonu, HTTP isteği kaçakçılık, HTTP yanıt bölme ve uzaktan dosya ekleme saldırısı gibi Yaygın Web Saldırıları Koruması.

-   HTTP protokol ihlallerine karşı koruma.

-   Eksik ana bilgisayar aracısı ve üstbilgi kabul gibi HTTP protokolü anormalliklerine karşı koruma.

-   Botlara, tarayıcılara ve tarayıcılara karşı önleme.

-   Yaygın uygulama yanlış yapılandırmalarının algılanması (diğer bir şey, Apache, IIS vb.).

WAF'ı uygulama ağ geçidinizde yapılandırmak aşağıdaki avantajları sağlar:

-   Arka uç kodunda değişiklik yapmadan web uygulamanızı web güvenlik açıklarından ve saldırılara karşı korur.

-   Bir uygulama ağ geçidinin arkasında aynı anda birden çok web uygulamasını korur. Bir uygulama ağ geçidi, en fazla 20 web sitesini barındırmayı destekler.

-   Uygulama ağ geçidi WAF günlükleri tarafından oluşturulan gerçek zamanlı raporlar kullanarak saldırılara karşı web uygulamalarını izler.

-   Uyumluluk gereksinimlerinin karşılatılmAmasına yardğ Bazı uyumluluk denetimleri, internete bakan tüm uç noktaların bir WAF çözümü tarafından korunmasını gerektirir.

### <a name="anomaly-detection-api-built-with-azure-machine-learning"></a>Anomali Algılama API'sı: Azure Machine Learning ile oluşturulmuştur

Anomali Algılama API'si, zaman serisi verilerinizdeki çeşitli anormal desenleri algılamak için yararlı olan bir API'dir. API, zaman serisindeki her veri noktasına, uyarı oluşturmak, panolar üzerinden izleme veya bilet sistemlerinize bağlanmak için kullanılabilecek bir anormallik puanı atar.

[Anomali Algılama API](../../machine-learning/team-data-science-process/apps-anomaly-detection-api.md) zaman serisi verileri anomaliaşağıdaki türleri tespit edebilirsiniz:

-   **Ani artışlar ve dipler**: Bir e-ticaret sitesindeki bir hizmete giriş hatası sayısını veya ödeme sayısını izlerken, olağandışı ani artışlar veya dipler güvenlik saldırılarını veya hizmet kesintilerini gösterebilir.

-   **Olumlu ve olumsuz eğilimler**: Bilgisayarda bellek kullanımını izlerken, serbest bellek boyutunun küçülmesi olası bir bellek sızıntısını gösterir. Hizmet sırası uzunluğu izleme için, kalıcı bir artış eğilimi altta yatan bir yazılım sorunu gösterebilir.

-   **Düzey değişiklikleri ve dinamik değerler aralığındaki değişiklikler**: Bir hizmet yükseltmesi veya yükseltmeden sonra daha düşük özel durumlar düzeyi sonrasında bir hizmetin gecikmesindeki düzey değişiklikleri izlemek ilginç olabilir.

Makine öğrenimi tabanlı API şunları sağlar:

-   **Esnek ve sağlam algılama**: Anomali algılama modelleri, kullanıcıların duyarlılık ayarlarını yapılandırmalarına ve mevsimsel ve mevsimsel olmayan veri kümeleri arasındaki anormallikleri tespit etmelerini sağlar. Kullanıcılar, algılama API'sini ihtiyaçlarına göre daha az veya daha hassas hale getirmek için anomali algılama modelini ayarlayabilirler. Bu, mevsimsel desenleri olan ve olmayan verilerdeki az veya daha fazla görünür anomalilerin tespit ilerler.

-   **Ölçeklenebilir ve zamanında algılama**: Uzmanların etki alanı bilgisi tarafından belirlenen mevcut eşiklerle geleneksel izleme yolu maliyetlidir ve dinamik olarak değişen milyonlarca veri kümesi için ölçeklenebilir değildir. Bu API'deki anomali algılama modelleri öğrenilir ve modeller hem geçmiş hem de gerçek zamanlı verilerden otomatik olarak ayarlanır.

-   **Proaktif ve uygulanabilir algılama**: Erken anomali tespiti için yavaş eğilim ve seviye değişikliği tespiti uygulanabilir. Tespit edilen erken anormal sinyaller, insanları sorunlu alanları araştırmaya ve harekete geçmeye yönlendirmek için kullanılabilir. Buna ek olarak, bu anomali algılama API hizmetinin üzerine kök neden analizi modelleri ve uyarı araçları geliştirilebilir.

Anomali algılama API' si, hizmet durumu ve KPI izleme, IoT, performans izleme ve ağ trafiği izleme gibi çok çeşitli senaryolar için etkili ve etkili bir çözümdür. Bu API'nin yararlı olabileceği bazı popüler senaryolar şunlardır:

- BT departmanlarının olayları, hata kodunu, kullanım günlüğünü ve performansı (CPU, bellek vb.) zamanında izlemek için araçlara ihtiyacı vardır.

-   Çevrimiçi ticaret siteleri müşteri etkinliklerini, sayfa görünümlerini, tıklamaları ve benzerlerini izlemek ister.

-   Kamu hizmetleri şirketleri su, gaz, elektrik ve diğer kaynakların tüketimini izlemek istiyor.

-   Tesis veya bina yönetim hizmetleri sıcaklık, nem, trafik ve benzeri izlemek istiyorum.

-   IoT/üreticileri, iş akışını, kaliteyi ve benzeri bilgileri izlemek için zaman serisindeki sensör verilerini kullanmak ister.

-   Çağrı merkezleri gibi hizmet sağlayıcılarının hizmet talep eğilimini, olay hacmini, bekleme sıra sını ve benzeri hizmetleri izlemesi gerekir.

-   İş analitiği grupları, iş KP'lerinin (satış hacmi, müşteri duyarlılığı veya fiyatlandırma gibi) anormal hareketini gerçek zamanlı olarak izlemek ister.

### <a name="cloud-app-security"></a>Cloud App Security

[Bulut Uygulaması Güvenliği,](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) Microsoft Bulut Güvenliği yığınının önemli bir bileşenidir. Bulut uygulamaları vaadinden tam olarak yararlanmak için hareket ederken kuruluşunuza yardımcı olabilecek kapsamlı bir çözümdür. Daha iyi görünürlük sayesinde sizi kontrol altında tutar. Ayrıca, bulut uygulamaları genelinde kritik veri korumasını artırmaya da yardımcı olur.

Kuruluşunuz, gölge BT’yi ortaya çıkarmaya, riskleri değerlendirmeye, ilkeleri zorunlu tutmaya, etkinlikleri araştırmaya ve tehditleri durdurmaya yardımcı olan araçlarla kritik verilerin denetimini elde tutarak buluta daha güvenli bir şekilde geçebilir.

| | |
|---|---|
| Bul | Cloud App Security ile gölge BT’yi ortaya çıkarın. Bulut ortamınızdaki uygulamaları, etkinlikleri, kullanıcıları, verileri ve dosyaları bularak görünürlük kazanın. Bulutunuza bağlı olan üçüncü taraf uygulamalarını keşfedin.|
|Araştır | Riskli uygulamaları, belirli kullanıcıları ve ağınızdaki dosyaları derinlemesine incelemek üzere bulut inceleme araçlarını kullanarak bulut uygulamalarınızı araştırın. Buluttan toplanan verilerdeki modelleri bulun. Bulutunuzu izlemek için raporlar oluşturun. |
| Denetim | Ağ bulut trafiği üzerinde en yüksek denetim düzeyine ulaşmak için ilkeler ve uyarılar ayarlayarak riski azaltın. Kullanıcılarınızı güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için Cloud App Security’yi kullanın. |
| Koruma | Uygulamaları uygulamak veya yasaklamak, veri kaybı önlemeyi uygulamak, izinleri ve paylaşımı denetlemek ve özel raporlar ve uyarılar oluşturmak için Bulut Uygulama Güvenliği'ni kullanın. |
| Denetim | Ağ bulut trafiği üzerinde en yüksek denetim düzeyine ulaşmak için ilkeler ve uyarılar ayarlayarak riski azaltın. Kullanıcılarınızı güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için Cloud App Security’yi kullanın. |
| | |


![Bulut Uygulaması Güvenlik diyagramı](./media/threat-detection/azure-threat-detection-fig14.png)

Cloud App Security görünürlüğü bulutunuzun la tümleştirerek:

-   Bulut ortamınızı ve kuruluşunuzun kullanmakta olduğu bulut uygulamalarını haritalamak ve tanımlamak için Bulut Bulma'yı kullanma.

-   Bulutunuzdaki uygulamalara yaptırım uygulama ve yasaklama.

-   Bağlandığınız uygulamaların görünürlüğü ve yönetimi için sağlayıcı API'lerinden yararlanan kolay dağıtılabilen uygulama bağlayıcılarını kullanma.

-   İlkeleri ayarlayarak sürekli denetime sahip olmanın ve ardından sürekli olarak ince ayar yapmanıza yardımcı olur.

Bulut App Security, bu kaynaklardan veri toplarken bu konuda gelişmiş analizler yürüter. Anormal etkinlikler hakkında derhal uyarı verir ve bulut ortamınızda derin görünürlük edinmenizi sağlar. Cloud App Security’de bir ilke yapılandırabilir ve bu ilkeyi kullanarak bulut ortamınızdaki her şeyi koruyabilirsiniz.

## <a name="third-party-advanced-threat-detection-capabilities-through-the-azure-marketplace"></a>Azure Marketi üzerinden üçüncü taraf Gelişmiş Tehdit Algılama yetenekleri

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı

Web Uygulaması Güvenlik Duvarı gelen web trafiğini inceler ve SQL enjeksiyonlarını, siteleri arası komut dosyalarını, kötü amaçlı yazılım yüklemelerini, uygulama DDoS saldırılarını ve web uygulamalarınızı hedefleyen diğer saldırıları engeller. Ayrıca, arka uç web sunucularından gelen yanıtları veri kaybını önleme (DLP) için de inceler. Tümleşik erişim denetim altyapısı, yöneticilerin kuruluşlara güçlü kimlik doğrulama ve kullanıcı denetimi sağlayan kimlik doğrulama, yetkilendirme ve muhasebe (AAA) için parçalı erişim denetimi ilkeleri oluşturmasına olanak tanır.

Web Uygulama Güvenlik Duvarı aşağıdaki avantajları sağlar:

-   SQL enjeksiyonlarını, Siteleri Arası Komut Dosyası'nı, kötü amaçlı yazılım yüklemelerini, uygulama DDoS'unu veya uygulamanıza yönelik diğer saldırıları algılar ve engeller.

-   Kimlik doğrulama ve erişim denetimi.

-   Hassas verileri algılamak için giden trafiği tarar ve bilgilerin sızdırılmasını maskeleyebilir veya engelleyebilir.

-   Önbelleğe alma, sıkıştırma ve diğer trafik optimizasyonları gibi yetenekleri kullanarak web uygulama içeriğinin teslimini hızlandırır.

Azure Marketi'nde bulunan web uygulaması güvenlik duvarları örnekleri için bkz: [Barracuda WAF, Brocade sanal web uygulaması güvenlik duvarı (vWAF), Imperva SecureSphere ve ThreatSTOP IP güvenlik duvarı.](https://azuremarketplace.microsoft.com/marketplace/apps/barracudanetworks.waf)

## <a name="next-steps"></a>Sonraki adımlar

- [Günümüzün tehditlerine yanıt verme](../../security-center/security-center-alerts-overview.md#respond-threats): Azure kaynaklarınızı hedefleyen etkin tehditlerin belirlenmesine yardımcı olur ve hızlı bir şekilde yanıt vermek için gereken öngörüleri sağlar.

- [Azure SQL Veritabanı Tehdit Algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/): Veritabanlarınıza yönelik olası tehditlerle ilgili endişelerinizi gidermeye yardımcı olur.
