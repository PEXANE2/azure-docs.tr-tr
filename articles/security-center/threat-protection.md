---
title: Azure Güvenlik Merkezi 'nde tehdit koruması
description: Bu konuda, Azure Güvenlik Merkezi 'nin tehdit koruması özellikleriyle korunan kaynaklar açıklanmaktadır
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 08/25/2019
ms.author: memildin
ms.openlocfilehash: 8bd66b602201dbbb47a1fdba879c52a8ac441762
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921649"
---
# <a name="threat-protection-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde tehdit koruması

Bu sayfa, standart fiyatlandırma katmanındaki kullanıcılar için Azure Güvenlik Merkezi tarafından gösterilecek güvenlik uyarılarının kaynaklarını kısaca açıklar.

Güvenlik Merkezi, ortamınızda aşağıda listelenen herhangi bir bölgede bir tehdit algıladığında bir uyarı oluşturur. Bu uyarılar, etkilenen kaynakların ayrıntılarını, önerilen düzeltme adımlarını ve bazı durumlarda bir mantıksal uygulamayı yanıt olarak tetiklemeye yönelik bir seçenek anlatmaktadır.

Bir uyarının Güvenlik Merkezi tarafından oluşturulup oluşturulmadığı veya Güvenlik Merkezi tarafından farklı bir güvenlik ürününden alınıp alınmayacağı, dışarı aktarabilirsiniz. Uyarılarınızı Azure Sentinel 'e (veya bir üçüncü taraf SıEM) veya diğer dış konumlara aktarmak için [uyarıları ve önerileri dışarı aktarma](continuous-export.md)konusundaki yönergeleri izleyin. 




## Windows makineleri için tehdit koruması<a name="windows-machines"></a>

Azure Güvenlik Merkezi, Windows tabanlı makinelerinizi izlemek ve korumak için Azure hizmetleri ile tümleşir. Güvenlik Merkezi, tüm bu hizmetlerden gelen uyarıları ve düzeltme önerilerini kullanımı kolay bir biçimde sunar.

* **Microsoft Defender ATP** <a name="windows-atp"></a> -Güvenlik Merkezi, Microsoft Defender Gelişmiş tehdit koruması (ATP) ile tümleştirerek bulut iş yükü koruma platformlarını genişletir. Birlikte, kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlar.

    > [!IMPORTANT]
    > Microsoft Defender ATP algılayıcısı, güvenlik merkezi kullanan Windows sunucularında otomatik olarak etkinleştirilir.

    Microsoft Defender ATP bir tehdit algıladığında bir uyarı tetikler. Uyarı, güvenlik merkezi panosunda gösterilir. Panodan, Microsoft Defender ATP konsoluna Pivot verebilir ve saldırının kapsamını açığa çıkarmak için ayrıntılı bir araştırma gerçekleştirebilirsiniz. Microsoft Defender ATP hakkında daha fazla bilgi için bkz. [Microsoft Defender ATP hizmetine sunucu](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)ekleme.

* **Kilitlenme bilgi dökümü Analizi** <a name="windows-dump"></a> -yazılım kilitlenirse, kilitlenme bilgi döküm kilitlenme sırasında belleğin bir kısmını yakalar.

    Kötü amaçlı yazılımdan koruma veya kötü amaçlı yazılım içeren bir kilitlenme oluşmuş olabilir. Güvenlik ürünleri tarafından algılanmayı önlemek için çeşitli kötü amaçlı yazılım biçimleri, diske yazmayı veya diske yazılmış yazılım bileşenlerini şifrelemeyi önleyen bir dosya daha az saldırı kullanır. Bu tür bir saldırı geleneksel disk tabanlı yaklaşımlar kullanılarak algılanarak tespit edilir.

    Ancak, bellek analizini kullanarak bu tür bir saldırıyı tespit edebilirsiniz. Güvenlik Merkezi, kilitlenme dökümündeki belleği çözümleyerek saldırının kullandığı teknikleri algılayabilir. Örneğin, saldırı yazılımda güvenlik açıklarından yararlanmaya, gizli verilere erişebilmeye ve güvenliği aşılmış bir makinede sürekli olarak devam ediyor olabilir. Güvenlik Merkezi, ana bilgisayarlara en az performans etkisi ile çalışır.

    Kilitlenme bilgi döküm Analizi uyarılarının ayrıntıları için, [Uyarı tablosuna](alerts-reference.md#alerts-windows)bakın.

* **Dosya daha az saldırı algılama** <a name="windows-fileless"></a> -uç noktalarınızı hedefleyen dosya daha az saldırı yaygındır. Algılamayı önlemek için, dosya daha az saldırı kötü amaçlı yükleri belleğe ekler. Saldırgan yükleri, güvenliği aşılmış işlemlerin belleğinde kalır ve çok çeşitli kötü amaçlı etkinlikler gerçekleştirir.

    Dosya daha az saldırı algılaması sayesinde, otomatik bellek oluşturma teknikleri, dosya daha az saldırı araç takımları, teknikler ve davranışları belirler. Bu çözüm, çalışma zamanında makinenizi düzenli olarak tarar ve öngörüleri doğrudan güvenlik açısından kritik işlemlerin belleğinden ayıklar.

    Yararlanma, kod ekleme ve kötü amaçlı yükleri yürütme kanıtlarını bulur. Dosya daha az saldırı algılama, uyarı önceliklendirme, bağıntı ve aşağı akış yanıt süresini hızlandırmak için ayrıntılı güvenlik uyarıları üretir. Bu yaklaşım, daha fazla algılama kapsamı sağlayan olay tabanlı EDR çözümlerini tamamlar.

    Dosya daha az saldırı algılama uyarılarına ilişkin ayrıntılar için, [Uyarı tablosuna](alerts-reference.md#alerts-windows)bakın.

> [!TIP]
> [Azure Güvenlik Merkezi PlayBook: güvenlik uyarılarını](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046)indirerek Windows uyarılarının benzetimini yapabilirsiniz.






## Linux makineleri için tehdit koruması<a name="linux-machines"></a>

Güvenlik Merkezi, en yaygın Linux denetim çerçevelerinden biri olan **auditd**'Yi kullanarak Linux makinelerden gelen denetim kayıtlarını toplar. auditd, ana hat çekirdekte yer yaşar. 

* **Linux auditd uyarıları ve Microsoft Monitoring Agent (MMA) Tümleştirmesi** <a name="linux-auditd"></a> -auditd sistemi, sistem çağrılarını izlemekten sorumlu olan çekirdek düzeyindeki bir alt sistemden oluşur. Bu, belirli bir kural kümesi tarafından filtreleyerek filtre uygular ve iletileri bir yuvaya yazar. Güvenlik Merkezi, Microsoft Monitoring Agent (MMA) içindeki auditd paketinden işlevleri tümleştirir. Bu tümleştirme, tüm desteklenen Linux dağıtımlarına hiçbir önkoşul olmadan sestd olaylarının toplanmasını mümkün bir şekilde sunar.  

    auditd kayıtları toplanır, zenginleştirir ve Linux MMA Aracısı kullanılarak olaylara toplanır. Güvenlik Merkezi, bulut ve şirket içi Linux makinelerinde kötü amaçlı davranışları algılamak için Linux sinyalleri kullanan yeni çözümlemeler ekler. Windows özelliklerine benzer şekilde, bu analizler şüpheli işlemlere, dubemte oturum açma girişimleri, çekirdek modülü yükleme ve diğer etkinlikleri kapsar. Bu etkinlikler, bir makinenin saldırıya açık ya da ihlal edilmiş olduğunu gösterebilir.  

    Linux uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-linux)bakın.

> [!TIP]
> [Azure Güvenlik Merkezi PlayBook: Linux algılamalarını](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef)indirerek Linux uyarılarının benzetimini yapabilirsiniz.





## Azure App Service için tehdit koruması<a name="app-services"></a>

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Güvenlik Merkezi, App Service üzerinde çalışan uygulamaları hedefleyen saldırıları belirlemek için bulutun ölçeğini kullanır. Saldırganlar, zayıf yanları bulmak ve yararlanmak için Web uygulamalarını araştırma. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Bu veriler daha sonra kötüye kullanım ve saldırganlar tanımlamak ve daha sonra kullanılacak yeni desenler öğrenmek için kullanılır.

Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğü kullanarak, birden çok hedef üzerinde saldırı yöntemini tanımlamak üzere iç günlükleri App Service analiz eder. Örneğin, metodolojide, yaygın tarama ve dağıtılmış saldırılar bulunur. Bu tür bir saldırı genellikle IP 'nin küçük bir alt kümesinden gelir ve birden fazla konaktaki benzer uç noktalara gezinme düzenlerini gösterir. Saldırılar, güvenlik açığı bulunan bir sayfa veya eklenti arıyor ve tek bir konağın açısından belirlenemiyor.

Windows tabanlı bir App Service planı çalıştırıyorsanız, güvenlik merkezi 'nin temel alınan korumalı makinelere ve sanal makinelere da erişimi vardır. Yukarıda bahsedilen günlük verileriyle birlikte, altyapı hikayeye, Müşteri makinelerinden muaf olmak üzere yeni bir saldırıya karşı çok daha fazla bilgi verebilir. Bu nedenle, güvenlik merkezi bir Web uygulaması kullanıldıktan sonra dağıtılsa bile, devam eden saldırıları tespit edebilir.

Azure App Service uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureappserv)bakın.

App Service planları hakkında daha fazla bilgi için bkz. [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).





## Azure kapsayıcıları için tehdit koruması<a name="azure-containers"></a>

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Güvenlik Merkezi, Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit koruması sağlar ve şüpheli etkinlikler için uyarı oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Güvenlik Merkezi, farklı düzeylerde tehdit koruması sağlar: 

* **Ana bilgisayar düzeyi** -Güvenlik Merkezi 'nin Aracısı (Standart katmanda mevcuttur. Ayrıntılar için [fiyatlandırma](security-center-pricing.md) ) Linux 'u şüpheli etkinlikler için izler. Aracı, düğüm veya üzerinde çalışan bir kapsayıcıdan kaynaklanan şüpheli etkinlikler için uyarıları tetikler. Web kabuğu algılama ve bilinen şüpheli IP adresleriyle bağlantı gibi etkinliklere örnek olarak verilebilir.

    Kapsayıcılı ortamınızın güvenliğine daha ayrıntılı bir bakış için, aracı kapsayıcıya özgü Analizi izler. Ayrıcalıklı kapsayıcı oluşturma, API sunucularına şüpheli erişim ve bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucuları gibi olaylar için uyarı tetikleyecektir.

    >[!IMPORTANT]
    > Aracıları konaklarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajları ve güvenlik uyarılarının bir alt kümesini alırsınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.

    Konak düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-containerhost)bakın.


* **Aks kümesi düzeyinde**tehdit koruması, Kubernetes 'ın denetim günlüklerini çözümlemeye dayalıdır. **Aracısız** izlemeyi etkinleştirmek Için, **fiyatlandırma & ayarları** sayfasından Kubernetes seçeneğini aboneliğinize ekleyin (bkz. [fiyatlandırma](security-center-pricing.md)). Bu düzeyde uyarı oluşturmak için, güvenlik merkezi AKS tarafından yönetilen Hizmetleri AKS tarafından alınan günlükleri kullanarak izler. Bu düzeydeki olay örnekleri, sunulan Kubernetes panoları, yüksek ayrıcalıklı rollerin oluşturulmasını ve hassas takmaları oluşturmayı içerir.

    >[!NOTE]
    > Güvenlik Merkezi, Azure Kubernetes hizmet eylemleri ve abonelik ayarlarında Kubernetes seçeneği etkinleştirildikten sonra gerçekleşen dağıtımlar için güvenlik uyarıları oluşturur. 

    AKS küme düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-akscluster)bakın.

Ayrıca, güvenlik araştırmacıları küresel takımımız tehdidi sürekli olarak izler. Bulundukları gibi kapsayıcıya özgü uyarıları ve güvenlik açıklarını ekler.






## Azure ağ katmanı için tehdit koruması<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analizi, Azure çekirdek yönlendiricileri tarafından toplanan paket üstbilgileri olan örnek [ıpdüzeltme verilerini](https://en.wikipedia.org/wiki/IP_Flow_Information_Export)temel alır. Güvenlik Merkezi, bu veri akışına bağlı olarak kötü amaçlı trafik etkinliklerini belirlemek ve işaretlemek için makine öğrenimi modellerini kullanır. Ayrıca, Güvenlik Merkezi, IP adreslerini zenginleştirmek için Microsoft Threat Intelligence veritabanını kullanır.

Bazı ağ yapılandırmalarında, güvenlik merkezi 'nin şüpheli ağ etkinliğinde Uyarı oluşturması kısıtlanabilir. Güvenlik Merkezi 'nin ağ uyarıları oluşturması için şunları doğrulayın:

- Sanal makineniz genel bir IP adresine sahiptir (veya genel IP adresine sahip bir yük dengeleyicide bulunur).

- Sanal makinenizin ağ çıkış trafiği bir dış KIMLIK çözümü tarafından engellenmiyor.

- Sanal makinenize, şüpheli iletişimin gerçekleştiği saatin tamamında aynı IP adresi atandı. Bu, yönetilen bir hizmetin parçası olarak oluşturulan VM 'Ler için de geçerlidir (örneğin, AKS, Databricks).

Azure ağ katmanı uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azurenetlayer)bakın.

Güvenlik Merkezi 'nin tehdit koruması uygulamak için ağla ilgili sinyalleri nasıl kullanabileceği hakkında ayrıntılı bilgi için bkz. [Güvenlik Merkezi 'Nde BULUŞSAL DNS algılamaları](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).







## Azure Key Vault için tehdit koruması (Önizleme)<a name="azure-keyvault"></a>

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

Azure Güvenlik Merkezi, ek bir güvenlik zekası katmanı sağlayan Azure Key Vault için Azure yerel, Gelişmiş tehdit koruması içerir. Güvenlik Merkezi, Key Vault hesaplarına erişme veya açıktan yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmadan ve üçüncü taraf güvenlik izleme sistemlerini yönetmeye gerek kalmadan tehditleri ele almanıza olanak sağlar.  

Anormal etkinlikler gerçekleştiğinde, güvenlik merkezi uyarıları gösterir ve isteğe bağlı olarak bunları abonelik yöneticilerine e-posta ile gönderir. Bu uyarılar, şüpheli etkinliğin ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir. 

Azure Key Vault uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azurekv)bakın.








## SQL veritabanı ve SQL veri ambarı için tehdit koruması<a name="data-sql"></a>

Azure SQL veritabanı için Gelişmiş tehdit koruması, veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar.

Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları veya SQL ekleme saldırıları ve anormal veritabanı erişimi ve sorgu desenleri olduğunda uyarılar görürsünüz.

Azure SQL veritabanı ve SQL için Gelişmiş tehdit koruması, Azure SQL veritabanları, Azure SQL veritabanı yönetilen örnekleri, Azure SQL veri ambarı veritabanları ve Azure sanal makinelerinde SQL Server 'lar kapsayan gelişmiş SQL güvenlik özelliklerine yönelik [Gelişmiş veri güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) Birleşik paketinin bir parçasıdır.

Daha fazla bilgi için bkz.

* [Azure SQL veritabanı için Gelişmiş tehdit korumasını etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Azure sanal makinelerinde SQL Server 'lar için Gelişmiş tehdit korumasını etkinleştirme](security-center-iaas-advanced-data.md)
* [SQL veritabanı ve SQL veri ambarı için tehdit koruması uyarılarının listesi](alerts-reference.md#alerts-sql-db-and-warehouse)









## Azure depolama için tehdit koruması<a name="azure-storage"></a>

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Depolama için Gelişmiş tehdit koruması (Şu anda yalnızca BLOB depolama için kullanılabilir), depolama hesaplarına erişme veya açıktan yararlanma amacıyla olağan dışı ve potansiyel olarak zararlı girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmanıza gerek kalmadan tehditleri sağlamanıza olanak tanır ve güvenlik izleme sistemlerinizi yönetmenize yardımcı olur.

Daha fazla bilgi için bkz.

* [Azure depolama için Gelişmiş tehdit koruması 'nı etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [Azure depolama için tehdit koruması uyarılarının listesi](alerts-reference.md#alerts-azurestorage)







## Azure Cosmos DB için tehdit koruması<a name="cosmos-db"></a>

Azure Cosmos DB uyarıları, Azure Cosmos DB hesaplarına erişmek veya bu hesaplara yararlanmaya yönelik olağan dışı ve potansiyel olarak zararlı denemelere göre oluşturulur.

Daha fazla bilgi için bkz.

* [Azure Cosmos DB için Gelişmiş tehdit koruması (Önizleme)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [Azure Cosmos DB için tehdit koruması uyarıları listesi (Önizleme)](alerts-reference.md#alerts-azurecosmos)







## Azure Yönetim Katmanı (Azure Resource Manager) için tehdit koruması (Önizleme)<a name ="management-layer"></a>

Güvenlik Merkezi 'nin Azure Resource Manager tabanlı koruma katmanı Şu anda önizleme aşamasındadır.

Güvenlik Merkezi, Azure için denetim düzlemi olarak kabul edilen Azure Resource Manager olaylarını kullanarak ek bir koruma katmanı sunar. Güvenlik Merkezi, Azure Resource Manager kayıtlarını analiz ederek Azure abonelik ortamında olağandışı veya potansiyel olarak zararlı işlemleri algılar.

Azure Resource Manager (Önizleme) uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureresourceman)bakın.



>[!NOTE]
> Yukarıdaki analizler Microsoft Cloud App Security tarafından desteklenmektedir. Bu analizlerden yararlanmak için bir Cloud App Security lisansını etkinleştirmeniz gerekir. Cloud App Security lisansınız varsa, bu uyarılar varsayılan olarak etkinleştirilir. Uyarıları devre dışı bırakmak için:
>
> 1. **Güvenlik Merkezi** dikey penceresinde **güvenlik ilkesi**' ni seçin. Değiştirmek istediğiniz abonelik için **Ayarları Düzenle**' yi seçin.
> 2. **Tehdit algılamayı**seçin.
> 3. **Tümleştirmelere**izin ver ' in altında, **verilerinize erişim Microsoft Cloud App Security izin ver**' i temizleyin ve **Kaydet**' i seçin

>[!NOTE]
>Güvenlik Merkezi, güvenlikle ilgili müşteri verilerini kaynakla aynı coğrafi bölgede depolar. Microsoft, kaynak coğrafi bölgede henüz Güvenlik Merkezi dağıtmamışsa, verileri Birleşik Devletler depolar. Cloud App Security etkinleştirildiğinde, bu bilgiler Cloud App Security coğrafi konum kurallarına uygun olarak depolanır. Daha fazla bilgi için bkz. [bölgesel olmayan hizmetler Için veri depolama](https://azuredatacentermap.azurewebsites.net/).



## Diğer Microsoft hizmetlerinden güvenlik uyarıları<a name="alerts-other"></a>

### Azure WAF için tehdit koruması<a name="azure-waf"></a>

Azure Application Gateway, web uygulamalarınız için açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir web uygulaması güvenlik duvarı (WAF) sunar.

Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılara karşı giderek daha da yöneliktir. WAF Application Gateway, açık Web uygulaması güvenlik projesinden 3,0 veya 2.2.9 temel kural kümesini temel alır. WAF, yeni güvenlik açıklarına karşı korumak için otomatik olarak güncelleştirilir. 

Azure WAF lisansına sahipseniz, WAF uyarılarınız ek bir yapılandırma gerekmeden Güvenlik Merkezi 'ne akışla kaydedilir. WAF tarafından oluşturulan uyarılar hakkında daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarı Ayrıntılar kural grupları ve kuralları](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).


### Azure DDoS koruması için tehdit koruması<a name="azure-ddos"></a>

Dağıtılmış hizmet reddi (DDoS) saldırılarına kolayca çalıştırılabilir. Özellikle de uygulamalarınızı buluta taşıyorsanız harika bir güvenlik sorunu haline gelmiştir. 

DDoS saldırıları, bir uygulamanın kaynaklarını tüketerek uygulamanın geçerli kullanıcılar için kullanılamaz duruma gelmesini amaçlar. DDoS saldırıları Internet üzerinden erişilebilecek tüm uç noktaları hedefleyebilir.

DDoS saldırılarına karşı savunmak için, Azure DDoS koruması için bir lisans satın alın ve en iyi uygulama tasarımı uygulamalarından olduğunuzdan emin olun. DDoS koruması farklı hizmet katmanları sağlar. Daha fazla bilgi için bkz. [Azure DDoS korumasına genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Azure DDoS koruması uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureddos)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bu tehdit koruması özelliklerinden güvenlik uyarıları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosu](alerts-reference.md)
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarıları](security-center-alerts-overview.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Güvenlik uyarılarını ve önerilerini dışarı aktarma (Önizleme)](continuous-export.md)