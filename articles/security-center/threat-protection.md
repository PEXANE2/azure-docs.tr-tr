---
title: Azure Güvenlik Merkezi'nde tehdit koruması
description: Bu konu, Azure Güvenlik Merkezi'nin tehdit koruması özellikleri tarafından korunan kaynakları açıklar
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: e348180eb94c1703ceecf2f2b00ab942ba5ff0ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536335"
---
# <a name="threat-protection-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde tehdit koruması

Güvenlik Merkezi ortamınızın herhangi bir alanında bir tehdit algıladığında, bir uyarı oluşturur. Bu uyarılar, etkilenen kaynakların ayrıntılarını, önerilen düzeltme adımlarını ve bazı durumlarda yanıt olarak bir mantık uygulamasını tetikleme seçeneğini açıklar.

Azure Güvenlik Merkezi'nin tehdit koruması, ortamınız için kapsamlı savunma sağlar:

* **Azure hesaplama kaynakları için tehdit koruması**: Windows makineleri, Linux makineleri, Azure Uygulama Hizmeti ve Azure kapsayıcıları

* **Azure veri kaynakları için tehdit koruması**: SQL Veritabanı ve SQL Veri Ambarı, Azure Depolama ve Azure Cosmos DB

* **Azure hizmet katmanları için tehdit koruması**: Azure ağ katmanı, Azure yönetim katmanı (Azure Kaynak Yöneticisi) (Önizleme) ve Azure Anahtar Kasası (Önizleme)

Bir uyarı, Güvenlik Merkezi tarafından oluşturulsun veya Güvenlik Merkezi tarafından farklı bir güvenlik ürününden alınsın, bunu dışa aktarabilirsiniz. Uyarılarınızı Azure Sentinel'e (veya üçüncü taraf bir SIEM'e) veya başka bir harici araca aktarmak [için, bir SIEM'e uyarı verme](continuous-export.md)yönergelerini izleyin. 




## <a name="threat-protection-for-windows-machines"></a>Windows makineleri için tehdit koruması<a name="windows-machines"></a>

Azure Güvenlik Merkezi, Windows tabanlı makinelerinizi izlemek ve korumak için Azure hizmetleriyle bütünleşir. Güvenlik Merkezi, tüm bu hizmetlerden gelen uyarıları ve düzeltme önerilerini kullanımı kolay bir biçimde sunar.

* **Microsoft Defender ATP** <a name="windows-atp"></a> - Security Center, Microsoft Defender Advanced Threat Protection (ATP) ile tümleştirerek bulut iş yükü koruma platformlarını genişletir. Birlikte, kapsamlı uç nokta algılama ve yanıt (EDR) yetenekleri sağlar.

    > [!IMPORTANT]
    > Microsoft Defender ATP sensörü, Güvenlik Merkezi'ni kullanan Windows sunucularında otomatik olarak etkinleştirilir.

    Microsoft Defender ATP bir tehdit algıladığında, bir uyarı tetikler. Uyarı, Güvenlik Merkezi panosunda gösterilir. Panodan Microsoft Defender ATP konsoluna dönebilir ve saldırının kapsamını ortaya çıkarmak için ayrıntılı bir araştırma gerçekleştirebilirsiniz. Microsoft Defender ATP hakkında daha fazla bilgi [için, Microsoft Defender ATP hizmetine ait Yerleşik sunuculara](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)bakın.

* **Kilitlenme dökümü analizi** <a name="windows-dump"></a> - Yazılım çöktüğünde, kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar.

    Bir kilitlenme kötü amaçlı yazılımdan kaynaklanmış veya kötü amaçlı yazılım içermiş olabilir. Güvenlik ürünleri tarafından algılanmamak için, çeşitli kötü amaçlı yazılım biçimleri, diske yazmaveya diske yazılmış yazılım bileşenlerini şifrelemekten kaçınan dosyasız bir saldırı kullanır. Bu tür saldırıları geleneksel disk tabanlı yaklaşımlar kullanarak algılamak zordur.

    Ancak, bellek çözümlemesi kullanarak, bu tür bir saldırı algılayabilirsiniz. Güvenlik Merkezi, kilitlenme dökümündeki belleği analiz ederek saldırının kullandığı teknikleri algılayabilir. Örneğin, saldırı yazılımdaki güvenlik açıklarından yararlanmaya, gizli verilere erişmeye ve gizliliği ihlal edilen bir makinede gizlice devam etmeye çalışıyor olabilir. Güvenlik Merkezi bu işi ev sahiplerine en az performans etkisiyle yapar.

    Kilitlenme dökümü çözümlemesi uyarılarının ayrıntıları [için, uyarılar Başvuru tablosuna](alerts-reference.md#alerts-windows)bakın.

* **Dosyasız saldırı algılama** <a name="windows-fileless"></a> - Uç noktalarınızı hedefleyen dosyasız saldırılar yaygındır. Algılanmasını önlemek için, dosyasız saldırılar belleğe kötü amaçlı yükler enjekte ediyor. Saldırgan ın yükleri, tehlikeye giren işlemlerin belleği içinde devam eder ve çok çeşitli kötü amaçlı etkinlikler gerçekleştirir.

    Dosyasız saldırı algılama ile, otomatik bellek adli teknikleri dosyasız saldırı araç kitleri, teknikler ve davranışları tanımlar. Bu çözüm, makinenizi çalışma zamanında düzenli aralıklarla tarar ve doğrudan güvenlik açısından kritik işlemlerin belleğinden öngörüler ayıklar.

    Bu sömürü, kod enjeksiyon ve kötü niyetli yüklerin yürütülmesi kanıt bulur. Dosyasız saldırı algılama, uyarı triajını, korelasyonunu ve akış aşağı yanıt süresini hızlandırmak için ayrıntılı güvenlik uyarıları oluşturur. Bu yaklaşım, daha fazla algılama kapsamı sağlayarak olay tabanlı EDR çözümlerini tamamlar.

    Dosyasız saldırı algılama uyarılarının ayrıntıları [için, uyarıların Başvuru tablosuna](alerts-reference.md#alerts-windows)bakın.

> [!TIP]
> [Azure Güvenlik Merkezi Playbook: Güvenlik Uyarıları](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046)indirerek Windows uyarılarını simüle edebilirsiniz.






## <a name="threat-protection-for-linux-machines"></a>Linux makineleri için tehdit koruması<a name="linux-machines"></a>

Güvenlik Merkezi, en yaygın Linux denetim çerçevelerinden biri olan **denetlenmiş**bilgisayar kullanarak Linux makinelerinden denetim kayıtları toplar. ana hat çekirdeğinde denetlenmiş yaşamları. 

* **Linux denetlenmiş uyarılar ve Microsoft Monitoring Agent (MMA) entegrasyonu** <a name="linux-auditd"></a> - Denetlenmiş sistem, sistem çağrılarının izlenmesinden sorumlu çekirdek düzeyinde bir alt sistemden oluşur. Bunları belirli bir kural kümesine göre filtreler ve iletileri bir sokete yazar. Güvenlik Merkezi, denetlenmiş paketteki işlevleri Microsoft İzleme Aracısı (MMA) içinde tümleştirir. Bu tümleştirme, desteklenen tüm Linux dağıtımlarında denetlenen olayların herhangi bir ön koşul olmaksızın toplanmasını sağlar.  

    denetlenen kayıtlar Linux MMA aracısı kullanılarak toplanır, zenginlenir ve olaylara ayrılır. Security Center, bulut ve şirket içi Linux makinelerindeki kötü amaçlı davranışları algılamak için Linux sinyallerini kullanan yeni analizler sürekli olarak ekler. Windows özelliklerine benzer şekilde, bu analizler şüpheli işlemler, denemelerde şüpheli oturum açma, çekirdek modülü yüklemeve diğer etkinlikler arasında uzanır. Bu etkinlikler, bir makinenin saldırı altında olduğunu veya ihlal edildiğini gösterebilir.  

    Linux uyarılarının listesi [için, uyarıların Başvuru tablosuna](alerts-reference.md#alerts-linux)bakın.

> [!TIP]
> [Azure Güvenlik Merkezi Playbook: Linux Algılamaları](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef)indirerek Linux uyarılarını simüle edebilirsiniz.





## <a name="threat-protection-for-azure-app-service"></a>Azure Uygulama Hizmeti için tehdit koruması<a name="app-services"></a>

> [!NOTE]
> Bu hizmet şu anda Azure devlet ve egemen bulut bölgelerinde kullanılamıyor.

Güvenlik Merkezi, Uygulama Hizmeti üzerinden çalışan uygulamaları hedefleyen saldırıları tanımlamak için bulut ölçeğini kullanır. Saldırganlar zayıflıkları bulmak ve yararlanmak için web uygulamalarını araştırır. Belirli ortamlara yönlendirilmeden önce, Azure'da çalışan uygulamalara yapılan istekler, denetlendikleri ve günlüğe kaydedildikleri çeşitli ağ geçitlerinden geçer. Bu veriler daha sonra açıkları ve saldırganları tanımlamak ve daha sonra kullanılacak yeni desenleri öğrenmek için kullanılır.

Güvenlik Merkezi, Azure'un bir bulut sağlayıcısı olarak sahip olduğu görünürlüğü kullanarak, birden çok hedefteki saldırı metodolojisini belirlemek için Uygulama Hizmeti dahili günlüklerini analiz eder. Örneğin, metodoloji yaygın tarama ve dağıtılmış saldırıları içerir. Bu tür saldırılar genellikle küçük bir IP alt kümesinden gelir ve birden çok ana bilgisayarda benzer uç noktalara tarama desenleri gösterir. Saldırılar, savunmasız bir sayfa veya eklenti arıyor ve tek bir ana bilgisayar açısından tanımlanamıyor.

Windows tabanlı bir Uygulama Hizmeti planı çalıştırıyorsanız, Güvenlik Merkezi'nin temel sandbox'lara ve VM'lere erişimi de vardır. Yukarıda belirtilen günlük verileriyle birlikte, altyapı, vahşi doğada dolaşan yeni bir saldırıdan müşteri makinelerindeki uzlaşmalara kadar hikayeyi anlatabilir. Bu nedenle, Güvenlik Merkezi bir web uygulaması ndan yararlandıktan sonra dağıtılsa bile, devam eden saldırıları algılayabilir.

Azure Uygulama Hizmeti uyarılarının listesi [için, uyarılara başvuru tablosuna](alerts-reference.md#alerts-azureappserv)bakın.

Uygulama Hizmeti planları hakkında daha fazla bilgi için [Uygulama Hizmeti planlarına](https://azure.microsoft.com/pricing/details/app-service/plans/)bakın.





## <a name="threat-protection-for-azure-containers"></a>Azure kapsayıcıları için tehdit koruması<a name="azure-containers"></a>

> [!NOTE]
> Bu hizmet şu anda Azure devlet ve egemen bulut bölgelerinde kullanılamıyor.

Güvenlik Merkezi, konteynerleştirilmiş ortamlarınız için gerçek zamanlı tehdit koruması sağlar ve şüpheli etkinlikler için uyarılar oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Güvenlik Merkezi farklı düzeylerde tehdit koruması sağlar: 

* **Ana bilgisayar düzeyi** - Security Center aracısı (Standart katmanda mevcuttur, ayrıntılar için [fiyatlandırmaya](security-center-pricing.md) bakın) şüpheli etkinlikler için Linux'u izler. Aracı, düğümden veya üzerinde çalışan bir kapsayıcıdan kaynaklanan şüpheli etkinlikler için uyarıları tetikler. Bu tür etkinliklere örnek olarak web kabuk algılama ve bilinen şüpheli IP adresleriyle bağlantı verilebilir.

    Aracı, konteynerleştirilmiş ortamınızın güvenliği hakkında daha derin bir bilgi edinmek için konteynere özgü analitiği izler. Ayrıcalıklı kapsayıcı oluşturma, API sunucularına şüpheli erişim ve Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucuları gibi olaylar için uyarılar tetikler.

    >[!IMPORTANT]
    > Aracıları ana bilgisayarlarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajlarının ve güvenlik uyarılarının bir alt kümesini alırsınız. Ağ çözümlemesi ve kötü amaçlı sunucularla iletişimle ilgili uyarılar almaya devam edeyim.

    Ana bilgisayar düzeyi uyarılarının listesi [için, uyarıların Başvuru tablosuna](alerts-reference.md#alerts-containerhost)bakın.


* **AKS küme düzeyinde,** tehdit koruması Kubernetes'in denetim günlüklerini analiz etmek üzerine kuruludur. Bu **aracısız** izlemeyi etkinleştirmek için, **Fiyatlandırma & ayarları** sayfasından aboneliğinize Kubernetes seçeneğini ekleyin [(fiyatlandırmaya](security-center-pricing.md)bakın). Bu düzeyde uyarılar oluşturmak için, Güvenlik Merkezi AKS tarafından alınan günlükleri kullanarak AKS tarafından yönetilen hizmetleri izler. Bu düzeydeki olaylara örnek olarak açıkta kalan Kubernetes panoları, yüksek ayrıcalıklı rollerin oluşturulması ve hassas bineklerin oluşturulması verilebilir.

    >[!NOTE]
    > Güvenlik Merkezi, abonelik ayarlarında Kubernetes seçeneği etkinleştirildikten sonra gerçekleşen Azure Kubernetes Hizmeti eylemleri ve dağıtımları için güvenlik uyarıları oluşturur. 

    AKS küme düzeyi uyarılarının listesi [için, uyarılar Başvuru tablosuna](alerts-reference.md#alerts-akscluster)bakın.

Ayrıca, güvenlik araştırmacılarından oluşan küresel ekibimiz tehdit ortamını sürekli olarak izleyebiyi zedeboluşturmaktadır. Konteynerlere özel uyarılar ve güvenlik açıkları keşfedildiklerinde eklerler.

> [!TIP]
> Bu blog gönderisindeki yönergeleri [this blog post](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270)izleyerek konteyner uyarılarını simüle edebilirsiniz.








## <a name="threat-protection-for-sql-database-and-sql-data-warehouse"></a>SQL Veritabanı ve SQL Veri Ambarı için tehdit koruması<a name="data-sql"></a>

Azure SQL Veritabanı için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları veya SQL enjeksiyon saldırıları ve anormal veritabanı erişimi ve sorgu desenleri olduğunda uyarılar görürsünüz.

Azure SQL Veritabanı ve SQL için Gelişmiş Tehdit Koruması, Azure SQL Veritabanları, Azure SQL Veritabanı yönetilen örnekleri, Azure SQL Veri Ambarı veritabanları ve Azure Sanal Makinelerdeki SQL sunucularını kapsayan gelişmiş SQL güvenlik özellikleri için [Gelişmiş Veri Güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) birleşik paketinin bir parçasıdır.

Daha fazla bilgi için bkz.

* [Azure SQL Veritabanı için Gelişmiş Tehdit Koruması nasıl etkinleştirilir?](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Azure Sanal Makinelerde SQL sunucuları için Gelişmiş Tehdit Koruması nasıl etkinleştirilir?](security-center-iaas-advanced-data.md)
* [SQL Veritabanı ve SQL Veri Ambarı için tehdit koruma uyarıları listesi](alerts-reference.md#alerts-sql-db-and-warehouse)




## <a name="threat-protection-for-azure-storage"></a>Azure Depolama için tehdit koruması<a name="azure-storage"></a>

> [!NOTE]
> Bu hizmet ABD devlet bulutlarında kullanılabilir, ancak başka egemen veya Azure devlet bulutu bölgesi yoktur.

Depolama için Gelişmiş Tehdit Koruması (şu anda yalnızca Blob depolama için kullanılabilir) depolama hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, bir güvenlik uzmanı olmanızı gerektirmeden tehditleri ele almanızı sağlar ve güvenlik izleme sistemlerinizi yönetmenize yardımcı olur.

Daha fazla bilgi için bkz.

* [Azure Depolama için Gelişmiş Tehdit Koruması nasıl etkinleştirilir?](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [Azure Depolama için tehdit koruması uyarıları listesi](alerts-reference.md#alerts-azurestorage)


> [!TIP]
> [Bu blog gönderisindeki](https://techcommunity.microsoft.com/t5/azure-security-center/validating-atp-for-azure-storage-detections-in-azure-security/ba-p/1068131)yönergeleri izleyerek Azure Depolama uyarılarını simüle edebilirsiniz.




## <a name="threat-protection-for-azure-cosmos-db"></a>Azure Cosmos DB için tehdit koruması<a name="cosmos-db"></a>

Azure Cosmos DB uyarıları, Azure Cosmos DB hesaplarına erişmek veya bundan yararlanmak için alışılmadık ve zararlı olabilecek girişimlerle oluşturulur.

Daha fazla bilgi için bkz.

* [Azure Cosmos DB için Gelişmiş Tehdit Koruması (Önizleme)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [Azure Cosmos DB için tehdit koruması uyarıları listesi (Önizleme)](alerts-reference.md#alerts-azurecosmos)




## <a name="threat-protection-for-azure-network-layer"></a>Azure ağ katmanı için tehdit koruması<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analitiği, Azure temel yönlendiricileri tarafından toplanan paket üstbilgileri olan örnek [IPFIX verilerine](https://en.wikipedia.org/wiki/IP_Flow_Information_Export)dayanır. Bu veri akışına dayanarak, Güvenlik Merkezi kötü amaçlı trafik etkinliklerini tanımlamak ve işaretlemek için makine öğrenimi modellerini kullanır. Security Center, IP adreslerini zenginleştirmek için Microsoft Threat Intelligence veritabanını da kullanır.

Bazı ağ yapılandırmaları, Güvenlik Merkezi'nin şüpheli ağ etkinliği hakkında uyarı lar oluşturmalarını kısıtlayabilir. Güvenlik Merkezi'nin ağ uyarıları oluşturması için şunları emin olun:

- Sanal makinenizin ortak bir IP adresi vardır (veya ortak IP adresi olan bir yük dengeleyicisi üzerindedir).

- Sanal makinenizin ağ çıkış trafiği harici bir IDS çözümü tarafından engellenmez.

- Sanal makinenize şüpheli iletişimin gerçekleştiği tüm saat boyunca aynı IP adresi atanmıştır. Bu, yönetilen bir hizmetin parçası olarak oluşturulan VM'ler için de geçerlidir (örneğin, AKS, Databricks).

Azure ağ katmanı uyarılarının listesi [için, uyarılara başvuru tablosuna](alerts-reference.md#alerts-azurenetlayer)bakın.

Güvenlik Merkezi'nin tehdit koruması uygulamak için ağla ilgili sinyalleri nasıl kullanabileceğine ilişkin ayrıntılar için, [Güvenlik Merkezi'ndeki Sezgisel DNS algılamalarına](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/)bakın.



## <a name="threat-protection-for-azure-management-layer-azure-resource-manager-preview"></a>Azure yönetim katmanı için tehdit koruması (Azure Kaynak Yöneticisi) (Önizleme)<a name ="management-layer"></a>

Güvenlik Merkezi'nin Azure Kaynak Yöneticisi'ni temel alan koruma katmanı şu anda önizlemededir.

Güvenlik Merkezi, Azure'un denetim düzlemi olarak kabul edilen Azure Kaynak Yöneticisi etkinliklerini kullanarak ek bir koruma katmanı sunar. Güvenlik Merkezi, Azure Kaynak Yöneticisi kayıtlarını analiz ederek Azure abonelik ortamında olağandışı veya zararlı olabilecek işlemleri algılar.

Azure Kaynak Yöneticisi (Önizleme) uyarılarının listesi [için, uyarılara başvuru tablosuna](alerts-reference.md#alerts-azureresourceman)bakın.



>[!NOTE]
> Önceki analizlerin birçoğu Microsoft Cloud App Security tarafından desteklenmektedir. Bu analizlerden yararlanabilmek için bir Bulut Uygulaması Güvenliği lisansBelgesietkinleştirmeniz gerekir. Bulut Uygulaması Güvenlik lisansınvarsa, bu uyarılar varsayılan olarak etkinleştirilir. Uyarıları devre dışı kalım için:
>
> 1. Güvenlik **Merkezi** bıçak, **Güvenlik ilkesi**ni seçin. Değiştirmek istediğiniz abonelik için **Ayarları Edit'i**seçin.
> 2. **Tehdit algılama'yı**seçin.
> 3. **Tümleştirmeleri Etkinleştir'in**altında, **Microsoft Cloud App Security'nin verilerime erişmesine izin ver'i**ve **Kaydet'i**seçin.

>[!NOTE]
>Güvenlik Merkezi, güvenlikle ilgili müşteri verilerini kaynağıyla aynı coğrafi kdurumunda depolar. Microsoft, güvenlik merkezini kaynağın coğrafi alanında henüz dağıtmadıysa, verileri ABD'de depolar. Bulut Uygulaması Güvenliği etkinleştirildiğinde, bu bilgiler Bulut Uygulaması Güvenliği'nin coğrafi konum kurallarına uygun olarak depolanır. Daha fazla bilgi [için, bölgesel olmayan hizmetler için Veri depolama](https://azuredatacentermap.azurewebsites.net/)alanına bakın.








## <a name="threat-protection-for-azure-key-vault-preview"></a>Azure Anahtar Kasası için tehdit koruması (Önizleme)<a name="azure-keyvault"></a>

> [!NOTE]
> Bu hizmet şu anda Azure devlet ve egemen bulut bölgelerinde kullanılamıyor.

Azure Key Vault, sertifikalar, bağlantı dizeleri ve parolalar gibi şifreleme anahtarlarını ve sırlarını koruyan bir bulut hizmetidir. 

Azure Güvenlik Merkezi, Azure Key Vault için Azure'a özgü, gelişmiş tehdit koruması içerir ve ek bir güvenlik zekası katmanı sağlar. Güvenlik Merkezi, Key Vault hesaplarına erişmek veya bundan yararlanmak için alışılmadık ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, bir güvenlik uzmanı olmadan ve üçüncü taraf güvenlik izleme sistemlerini yönetmeye gerek kalmadan tehditleri ele almanızı sağlar.  

Anormal etkinlikler oluştuğunda, Güvenlik Merkezi uyarıları gösterir ve isteğe bağlı olarak bunları abonelik yöneticilerine e-posta yoluyla gönderir. Bu uyarılar, şüpheli etkinliğin ayrıntılarını ve tehditleri nasıl araştırıp düzelteceklerine ilişkin önerileri içerir. 

Azure Anahtar Kasası uyarılarının listesi [için, uyarılara başvuru tablosuna](alerts-reference.md#alerts-azurekv)bakın.





## <a name="threat-protection-for-other-microsoft-services"></a>Diğer Microsoft hizmetleri için tehdit koruması<a name="alerts-other"></a>

### <a name="threat-protection-for-azure-waf"></a>Azure WAF için tehdit koruması<a name="azure-waf"></a>

Azure Application Gateway, web uygulamalarınız için açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir web uygulaması güvenlik duvarı (WAF) sunar.

Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılar tarafından giderek daha fazla hedef alınır. Uygulama Ağ Geçidi WAF, Açık Web Uygulama Güvenliği Projesi'nden Temel Kural Kümesi 3.0 veya 2.2.9'u temel almaktadır. WAF, yeni güvenlik açıklarına karşı korunmak için otomatik olarak güncellenir. 

Azure WAF için lisansınız varsa, WAF uyarılarınız ek yapılandırma gerekmeden Güvenlik Merkezi'ne aktarılır. WAF tarafından oluşturulan uyarılar hakkında daha fazla bilgi için [Bkz. Web uygulaması güvenlik duvarı CRS kural grupları ve kuralları.](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31)


### <a name="threat-protection-for-azure-ddos-protection"></a>Azure DDoS Koruması için tehdit koruması<a name="azure-ddos"></a>

Dağıtılmış hizmet reddi (DDoS) saldırılarının yürütülmesinin kolay olduğu bilinmektedir. Özellikle uygulamalarınızı buluta taşıyorsanız, bunlar büyük bir güvenlik sorunu haline gelmiştir. 

Bir DDoS saldırısı, uygulamanın kaynaklarını tüketerek uygulamayı yasal kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden ulaşılabilen herhangi bir bitiş noktasını hedef alabilir.

DDoS saldırılarına karşı savunmak için Azure DDoS Koruması için bir lisans satın alın ve uygulama tasarımının en iyi uygulamalarını takip ettiğinizden emin olun. DDoS Protection farklı hizmet katmanları sağlar. Daha fazla bilgi için Azure [DDoS Koruması genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)bilgisine bakın.

Azure DDoS Koruması uyarılarının listesi için [uyarılardan oluşan Başvuru tablosuna](alerts-reference.md#alerts-azureddos)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bu tehdit koruma özelliklerinden güvenlik uyarıları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosu](alerts-reference.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Güvenlik uyarılarını ve önerilerini dışa aktarma (Önizleme)](continuous-export.md)