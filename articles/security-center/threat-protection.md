---
title: Azure Güvenlik Merkezi’nde tehdit koruması
description: Bu konuda, Azure Güvenlik Merkezi 'nin tehdit koruması özellikleriyle korunan kaynaklar açıklanmaktadır
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: d049538653ea345935d40bd965afd7d2453b2aa2
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88041995"
---
# <a name="threat-protection-in-azure-security-center"></a>Azure Güvenlik Merkezi’nde tehdit koruması

Güvenlik Merkezi, ortamınızın herhangi bir alanında bir tehdit algıladığında bir uyarı oluşturur. Bu uyarılar, etkilenen kaynakların ayrıntılarını, önerilen düzeltme adımlarını ve bazı durumlarda bir mantıksal uygulamayı yanıt olarak tetiklemeye yönelik bir seçenek anlatmaktadır.

Azure Güvenlik Merkezi 'nin tehdit koruması, ortamınız için kapsamlı savunma sağlar:

* **Azure işlem kaynakları Için tehdit koruması**: Windows makineler, Linux makineleri, Azure App Service ve Azure kapsayıcıları

* **Azure veri kaynakları Için tehdit koruması**: SQL VERITABANı ve SQL veri ambarı, Azure depolama ve Azure Cosmos DB

* **Azure hizmet katmanları Için tehdit koruması**: Azure ağ katmanı, Azure Yönetim katmanı (Azure Resource Manager) (Önizleme) ve Azure Key Vault (Önizleme)

Bir uyarının Güvenlik Merkezi tarafından oluşturulup oluşturulmadığı veya Güvenlik Merkezi tarafından farklı bir güvenlik ürününden alınıp alınmayacağı, dışarı aktarabilirsiniz. Uyarılarınızı Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için [uyarıları BIR SıEM 'ye aktarma](continuous-export.md)konusundaki yönergeleri izleyin. 

> [!NOTE]
> Farklı kaynaklardaki uyarıların görünmesi farklı miktarda zaman alabilir. Örneğin, ağ trafiğinin analizini gerektiren uyarıların, sanal makinelerde çalışan şüpheli işlemlerle ilgili uyarılardan görünmesi daha uzun sürebilir.

> [!TIP]
> Güvenlik Merkezi 'nin tehdit koruması yeteneklerini etkinleştirmek için, uygun iş yüklerini içeren aboneliğe standart fiyatlandırma katmanını uygulamanız gerekir.
>
> **Azure depolama hesapları** için tehdit korumasını abonelik düzeyinde veya kaynak düzeyinde etkinleştirebilirsiniz.
> **Azure SQL VERITABANı SQL Server 'lar** için tehdit korumasını abonelik düzeyinde veya kaynak düzeyinde etkinleştirebilirsiniz.
> **MariaDB/MySQL/PostgreSQL Için Azure veritabanı** için tehdit korumasını yalnızca kaynak düzeyinde etkinleştirebilirsiniz.



## <a name="threat-protection-for-windows-machines"></a>Windows makineleri için tehdit koruması<a name="windows-machines"></a>

Azure Güvenlik Merkezi, Windows tabanlı makinelerinizi izlemek ve korumak için Azure hizmetleri ile tümleşir. Güvenlik Merkezi, tüm bu hizmetlerden gelen uyarıları ve düzeltme önerilerini kullanımı kolay bir biçimde sunar.

* **Microsoft Defender Gelişmiş tehdit koruması (ATP)** <a name="windows-atp"></a> -Güvenlik Merkezi, Microsoft Defender Gelişmiş tehdit koruması (ATP) ile tümleştirerek bulut iş yükü koruma platformlarını genişletir. Birlikte, kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlar.

    > [!IMPORTANT]
    > Microsoft Defender ATP algılayıcısı, güvenlik merkezi kullanan Windows sunucularında otomatik olarak etkinleştirilir.

    Microsoft Defender ATP bir tehdit algıladığında bir uyarı tetikler. Uyarı, güvenlik merkezi panosunda gösterilir. Panodan, Microsoft Defender ATP konsoluna Pivot verebilir ve saldırının kapsamını açığa çıkarmak için ayrıntılı bir araştırma gerçekleştirebilirsiniz. Microsoft Defender ATP hakkında daha fazla bilgi için bkz. [Microsoft Defender ATP hizmetine sunucu](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)ekleme.

* **Dosya daha az saldırı algılama** <a name="windows-fileless"></a> -Fileless saldırıları, disk tabanlı tarama tekniklerine göre algılamayı önlemek için kötü amaçlı yükleri belleğe ekler. Saldırganın yükü, daha fazla riskli işlem belleği içinde devam ettirir ve çok çeşitli kötü amaçlı etkinlikler gerçekleştirir.

    Dosya daha az saldırı algılaması sayesinde, otomatik bellek oluşturma teknikleri, dosya daha az saldırı araç takımları, teknikler ve davranışları belirler. Bu çözüm, çalışma zamanında makinenizi düzenli olarak tarar ve öngörüleri doğrudan işlem belleğinden ayıklar. Linux için özel Öngörüler şunları içerir: 

    - İyi bilinen araç takımları ve şifre araştırma yazılımı 
    - Genellikle bir yazılım güvenlik açığından yararlanma aşamasında yük olarak kullanılan küçük bir kod parçası olan shellcode.
    - İşlem belleğinde kötü amaçlı yürütülebilir dosya eklendi

    Dosya daha az saldırı algılama, ağ etkinliği gibi ek işlem meta verileri ile açıklamaları içeren ayrıntılı güvenlik uyarıları oluşturur. Bu, uyarı önceliklendirme, bağıntı ve aşağı akış yanıt süresini hızlandırır. Bu yaklaşım, olay tabanlı EDR çözümlerini tamamlar ve daha fazla algılama kapsamı sağlar.

    Dosya daha az saldırı algılama uyarılarına ilişkin ayrıntılar için, [Uyarı tablosuna](alerts-reference.md#alerts-windows)bakın.

> [!TIP]
> [Azure Güvenlik Merkezi PlayBook: güvenlik uyarılarını](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046)indirerek Windows uyarılarının benzetimini yapabilirsiniz.






## <a name="threat-protection-for-linux-machines"></a>Linux makineleri için tehdit koruması<a name="linux-machines"></a>

Güvenlik Merkezi, en yaygın Linux denetim çerçevelerinden biri olan **auditd**'Yi kullanarak Linux makinelerden gelen denetim kayıtlarını toplar. auditd, ana hat çekirdekte yer yaşar. 

* **Linux auditd uyarıları ve Log Analytics aracı tümleştirmesi** <a name="linux-auditd"></a> -Auditd sistemi, sistem çağrılarını izlemekten sorumlu olan çekirdek düzeyindeki bir alt sistemden oluşur. Bu, belirli bir kural kümesi tarafından filtreleyerek filtre uygular ve iletileri bir yuvaya yazar. Güvenlik Merkezi, Log Analytics aracısında sestd paketinden işlevleri tümleştirir. Bu tümleştirme, tüm desteklenen Linux dağıtımlarına hiçbir önkoşul olmadan sestd olaylarının toplanmasını mümkün bir şekilde sunar.

    auditd kayıtları, Linux Aracısı için Log Analytics Aracısı kullanılarak toplanır, zenginleştirir ve olaylara toplanır. Güvenlik Merkezi, bulut ve şirket içi Linux makinelerinde kötü amaçlı davranışları algılamak için Linux sinyalleri kullanan yeni çözümlemeler ekler. Windows özelliklerine benzer şekilde, bu analizler şüpheli işlemlere, dubmerak eden oturum açma girişimlerini, çekirdek modülünü yüklemeyi ve diğer etkinlikleri kapsar. Bu etkinlikler, bir makinenin saldırıya açık ya da ihlal edilmiş olduğunu gösterebilir.  

    Linux uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-linux)bakın.

> [!TIP]
> [Azure Güvenlik Merkezi PlayBook: Linux algılamalarını](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef)indirerek Linux uyarılarının benzetimini yapabilirsiniz.





## <a name="threat-protection-for-azure-app-service"></a>Azure App Service için tehdit koruması<a name="app-services"></a>

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Güvenlik Merkezi, App Service üzerinde çalışan uygulamaları hedefleyen saldırıları belirlemek için bulutun ölçeğini kullanır. Saldırganlar, zayıf yanları bulmak ve yararlanmak için Web uygulamalarını araştırma. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Bu veriler daha sonra kötüye kullanım ve saldırganlar tanımlamak ve daha sonra kullanılacak yeni desenler öğrenmek için kullanılır.

Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğü kullanarak, birden çok hedef üzerinde saldırı yöntemini tanımlamak üzere iç günlükleri App Service analiz eder. Örneğin, metodolojide, yaygın tarama ve dağıtılmış saldırılar bulunur. Bu tür bir saldırı genellikle IP 'nin küçük bir alt kümesinden gelir ve birden fazla konaktaki benzer uç noktalara gezinme düzenlerini gösterir. Saldırılar, güvenlik açığı bulunan bir sayfa veya eklenti arıyor ve tek bir konağın açısından belirlenemiyor.

Windows tabanlı bir App Service planı çalıştırıyorsanız, güvenlik merkezi 'nin temel alınan korumalı makinelere ve sanal makinelere da erişimi vardır. Yukarıda bahsedilen günlük verileriyle birlikte, altyapı hikayeye, Müşteri makinelerinden muaf olmak üzere yeni bir saldırıya karşı çok daha fazla bilgi verebilir. Bu nedenle, güvenlik merkezi bir Web uygulaması kullanıldıktan sonra dağıtılsa bile, devam eden saldırıları tespit edebilir.

Azure App Service uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureappserv)bakın.

App Service planları hakkında daha fazla bilgi için bkz. [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).





## <a name="threat-protection-for-containers"></a>Kapsayıcılar için tehdit koruması<a name="azure-containers"></a>

### <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir|
|Fiyat|Standart katmanı|
|Gerekli roller ve izinler:|**Güvenlik Yöneticisi** , uyarıları kapatabilir.<br>**Güvenlik okuyucusu** bulguları görüntüleyebilir.|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

[!INCLUDE [AKS in ASC threat protection](../../includes/security-center-azure-kubernetes-threat-protection.md)]








## <a name="threat-protection-for-sql-database-and-sql-data-warehouse"></a>SQL veritabanı ve SQL veri ambarı için tehdit koruması<a name="data-sql"></a>

Azure SQL veritabanı için Gelişmiş tehdit koruması, veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar.

Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları veya SQL ekleme saldırıları ve anormal veritabanı erişimi ve sorgu desenleri olduğunda uyarılar görürsünüz.

Azure SQL veritabanı ve SQL için Gelişmiş tehdit koruması, Azure SQL veritabanı, Azure SQL yönetilen örnekleri, Azure SQL veri ambarı veritabanları ve Azure sanal makinelerinde SQL Server 'lar kapsayan gelişmiş SQL güvenlik özelliklerine yönelik [Gelişmiş veri güvenliği (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) Birleşik paketinin bir parçasıdır.

Daha fazla bilgi için bkz.

* [Azure SQL veritabanı için Gelişmiş tehdit korumasını etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Azure sanal makinelerinde SQL Server 'lar için Gelişmiş tehdit korumasını etkinleştirme](security-center-iaas-advanced-data.md)
* [SQL veritabanı ve SQL veri ambarı için tehdit koruması uyarılarının listesi](alerts-reference.md#alerts-sql-db-and-warehouse)



## <a name="threat-protection-for-azure-storage"></a>Azure depolama için tehdit koruması<a name="azure-storage"></a>

### <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|[BLOB depolama](https://azure.microsoft.com/services/storage/blobs/) (genel kullanılabilirlik)<br>[Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) (Önizleme)<br>[Azure Data Lake Storage 2.](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) (Önizleme)|
|Fiyat|Standart katmanı|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) US Gov<br>![No](./media/icons/no-icon.png) Çin gov, diğer gov|
|||


### <a name="whats-protected"></a>Ne korumalıdır?

Azure depolama için tehdit koruması, Azure depolama hesaplarınızdaki zararlı olabilecek etkinlikleri algılar. Verileriniz, blob kapsayıcıları, dosya paylaşımları veya veri Lakes olarak depolanıp saklanmadığı için korunabilir.

Bu koruma katmanı, güvenlik uzmanı olmanıza gerek *kalmadan* tehditleri sağlamanıza olanak tanır ve güvenlik izleme sistemlerinizi yönetmenize yardımcı olur.

Depolama hesaplarınız korunuyor 

### <a name="what-kind-of-alerts-does-threat-protection-for-azure-storage-provide"></a>Azure Storage için tehdit koruması ne tür uyarılar sağlar?

Güvenlik uyarıları şunları yaparken tetiklenir:

- **Şüpheli etkinlik** -Örneğin, depolama hesabına Tor 'ın etkin çıkış düğümü olarak BILINEN bir IP adresinden başarıyla erişildi
- **Anormal davranış** ; Örneğin, erişim düzenindeki bir depolama hesabına yapılan değişiklikler
- **Karşıya yüklenen olası kötü amaçlı yazılım** -karma saygınlığı Analizi karşıya yüklenen bir dosyanın kötü amaçlı

Uyarılar, bunları tetikleyen olayın ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir.

### <a name="what-is-hash-reputation-analysis-for-malware"></a>Kötü amaçlı yazılım için karma saygınlığı analizi nedir?

Karşıya yüklenen bir dosyanın şüpheli olup olmadığını öğrenmek için Azure depolama için tehdit koruması, [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684)tarafından desteklenen karma saygınlığı analizini kullanır. Tehdit koruması araçları karşıya yüklenen dosyaları taramaz, böylece depolama günlüklerini inceler ve yeni yüklenen dosyaların karmalarını, bilinen virüsler, Truva atları, casus yazılım ve fidye ile karşılaştırın. 

Bir dosyanın kötü amaçlı yazılım içermesi şüpheli olduğu durumlarda, güvenlik merkezi bir uyarı görüntüler ve isteğe bağlı olarak, şüpheli dosyayı silmek üzere depolama sahibini onay için e-posta ile gönderebilir. Karma saygınlığı analizinin kötü amaçlı yazılım içerdiğini gösterdiği dosyaların bu otomatik olarak kaldırılmasını ayarlamak için, ["depolama hesabına yüklenen potansiyel kötü amaçlı yazılımlar" içeren uyarılarda tetiklemek üzere bir iş akışı Otomasyonu](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-respond-to-potential-malware-uploaded-to-azure-storage/ba-p/1452005)dağıtın.



### <a name="next-steps"></a>Sonraki adımlar 

30 günlük ücretsiz deneme sürümü de dahil olmak üzere fiyatlandırma ayrıntıları için bkz. [Azure Güvenlik Merkezi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Daha fazla bilgi için bkz.

* [Azure depolama için Gelişmiş tehdit koruması 'nı etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [Azure depolama için tehdit koruması uyarılarının listesi](alerts-reference.md#alerts-azurestorage)
* [Microsoft 'un tehdit bilgileri özellikleri](https://go.microsoft.com/fwlink/?linkid=2128684)

> [!TIP]
> [Bu blog gönderisine](https://techcommunity.microsoft.com/t5/azure-security-center/validating-atp-for-azure-storage-detections-in-azure-security/ba-p/1068131)ait yönergeleri izleyerek depolama uyarılarının benzetimini yapabilirsiniz.







## <a name="threat-protection-for-azure-cosmos-db"></a>Azure Cosmos DB için tehdit koruması<a name="cosmos-db"></a>

Azure Cosmos DB uyarıları, Azure Cosmos DB hesaplarına erişmek veya bu hesaplara yararlanmaya yönelik olağan dışı ve potansiyel olarak zararlı denemelere göre oluşturulur.

Daha fazla bilgi için bkz.

* [Azure Cosmos DB için Gelişmiş tehdit koruması (Önizleme)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [Azure Cosmos DB için tehdit koruması uyarıları listesi (Önizleme)](alerts-reference.md#alerts-azurecosmos)




## <a name="threat-protection-for-azure-network-layer"></a>Azure ağ katmanı için tehdit koruması<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analizi, Azure çekirdek yönlendiricileri tarafından toplanan paket üstbilgileri olan örnek [ıpdüzeltme verilerini](https://en.wikipedia.org/wiki/IP_Flow_Information_Export)temel alır. Güvenlik Merkezi, bu veri akışına bağlı olarak kötü amaçlı trafik etkinliklerini belirlemek ve işaretlemek için makine öğrenimi modellerini kullanır. Ayrıca, Güvenlik Merkezi, IP adreslerini zenginleştirmek için Microsoft Threat Intelligence veritabanını kullanır.

Bazı ağ yapılandırmalarında, güvenlik merkezi 'nin şüpheli ağ etkinliğinde Uyarı oluşturması kısıtlanabilir. Güvenlik Merkezi 'nin ağ uyarıları oluşturması için şunları doğrulayın:

- Sanal makineniz genel bir IP adresine sahiptir (veya genel IP adresine sahip bir yük dengeleyicide bulunur).

- Sanal makinenizin ağ çıkış trafiği bir dış KIMLIK çözümü tarafından engellenmiyor.

- Sanal makinenize, şüpheli iletişimin gerçekleştiği saatin tamamında aynı IP adresi atandı. Bu, yönetilen bir hizmetin parçası olarak oluşturulan VM 'Ler için de geçerlidir (örneğin, AKS, Databricks).

Azure ağ katmanı uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azurenetlayer)bakın.

Güvenlik Merkezi 'nin tehdit koruması uygulamak için ağla ilgili sinyalleri nasıl kullanabileceği hakkında ayrıntılı bilgi için bkz. [Güvenlik Merkezi 'Nde BULUŞSAL DNS algılamaları](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).



## <a name="threat-protection-for-azure-management-layer-azure-resource-manager-preview"></a>Azure Yönetim Katmanı (Azure Resource Manager) için tehdit koruması (Önizleme)<a name ="management-layer"></a>

Güvenlik Merkezi 'nin Azure Resource Manager tabanlı koruma katmanı Şu anda önizleme aşamasındadır.

Güvenlik Merkezi, Azure için denetim düzlemi olarak kabul edilen Azure Resource Manager olaylarını kullanarak ek bir koruma katmanı sunar. Güvenlik Merkezi, Azure Resource Manager kayıtlarını analiz ederek Azure abonelik ortamında olağandışı veya potansiyel olarak zararlı işlemleri algılar.

Azure Resource Manager (Önizleme) uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureresourceman)bakın.



>[!NOTE]
> Yukarıdaki analizler Microsoft Cloud App Security tarafından desteklenmektedir. Bu analizlerden yararlanmak için bir Cloud App Security lisansını etkinleştirmeniz gerekir. Cloud App Security lisansınız varsa, bu uyarılar varsayılan olarak etkinleştirilir. Uyarıları devre dışı bırakmak için:
>
> 1. Güvenlik Merkezi 'nin menüsünde **fiyatlandırma & ayarları**' nı seçin.
> 1. Değiştirmek istediğiniz aboneliği seçin.
> 1. **Tehdit algılamayı**seçin.
> 1. **Verilerinize erişmek için Microsoft Cloud App Security Izin ver**' i temizleyin ve **Kaydet**' i seçin.

>[!NOTE]
>Güvenlik Merkezi, güvenlikle ilgili müşteri verilerini kaynakla aynı coğrafi bölgede depolar. Microsoft, kaynak coğrafi bölgede henüz Güvenlik Merkezi dağıtmamışsa, verileri Birleşik Devletler depolar. Cloud App Security etkinleştirildiğinde, bu bilgiler Cloud App Security coğrafi konum kurallarına uygun olarak depolanır. Daha fazla bilgi için bkz. [bölgesel olmayan hizmetler Için veri depolama](https://azuredatacentermap.azurewebsites.net/).

1. Aracıyı yüklemekte olduğunuz çalışma alanını ayarlayın. Çalışma alanının, güvenlik merkezi 'nde kullandığınız abonelikte olduğundan ve çalışma alanında okuma/yazma izinlerine sahip olduğunuzdan emin olun.

1. Standart fiyatlandırma katmanını ayarlayın ve **Kaydet**' i seçin.






## <a name="threat-protection-for-azure-key-vault-preview"></a>Azure Key Vault için tehdit koruması (Önizleme)<a name="azure-keyvault"></a>

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

Azure Güvenlik Merkezi, ek bir güvenlik zekası katmanı sağlayan Azure Key Vault için Azure yerel, Gelişmiş tehdit koruması içerir. Güvenlik Merkezi, Key Vault hesaplarına erişme veya açıktan yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmadan ve üçüncü taraf güvenlik izleme sistemlerini yönetmeye gerek kalmadan tehditleri ele almanıza olanak sağlar.  

Anormal etkinlikler gerçekleştiğinde, güvenlik merkezi uyarıları gösterir ve isteğe bağlı olarak bunları abonelik yöneticilerine e-posta ile gönderir. Bu uyarılar, şüpheli etkinliğin ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir. 

Azure Key Vault uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azurekv)bakın.





## <a name="threat-protection-for-other-microsoft-services"></a>Diğer Microsoft Hizmetleri için tehdit koruması<a name="alerts-other"></a>

### <a name="threat-protection-for-azure-waf"></a>Azure WAF için tehdit koruması<a name="azure-waf"></a>

Azure Application Gateway, web uygulamalarınız için açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir web uygulaması güvenlik duvarı (WAF) sunar.

Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılara karşı giderek daha da yöneliktir. WAF Application Gateway, açık Web uygulaması güvenlik projesinden 3,0 veya 2.2.9 temel kural kümesini temel alır. WAF, yeni güvenlik açıklarına karşı korumak için otomatik olarak güncelleştirilir. 

Azure WAF lisansına sahipseniz, WAF uyarılarınız ek bir yapılandırma gerekmeden Güvenlik Merkezi 'ne akışla kaydedilir. WAF tarafından oluşturulan uyarılar hakkında daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarı Ayrıntılar kural grupları ve kuralları](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).


### <a name="threat-protection-for-azure-ddos-protection"></a>Azure DDoS koruması için tehdit koruması<a name="azure-ddos"></a>

Dağıtılmış hizmet reddi (DDoS) saldırılarına kolayca çalıştırılabilir. Özellikle de uygulamalarınızı buluta taşıyorsanız harika bir güvenlik sorunu haline gelmiştir. 

DDoS saldırısı, uygulamanın kaynaklarını tüketmeye çalışır ve uygulamayı meşru kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları Internet üzerinden erişilebilecek tüm uç noktaları hedefleyebilir.

DDoS saldırılarına karşı savunmak için, Azure DDoS koruması için bir lisans satın alın ve en iyi uygulama tasarımı uygulamalarından olduğunuzdan emin olun. DDoS koruması farklı hizmet katmanları sağlar. Daha fazla bilgi için bkz. [Azure DDoS korumasına genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Azure DDoS koruması uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureddos)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bu tehdit koruması özelliklerinden güvenlik uyarıları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosu](alerts-reference.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Güvenlik uyarılarını ve önerilerini dışarı aktarma (Önizleme)](continuous-export.md)