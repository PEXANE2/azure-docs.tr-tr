---
title: İşlem Yöneticisini Azure Monitörüne Bağlayın | Microsoft Dokümanlar
description: System Center Operations Manager'a yaptığınız mevcut yatırımı korumak ve Log Analytics'le sağlanan genişletilmiş özellikleri kullanmak için, Operations Manager'ı çalışma alanınızla tümleştirebilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/13/2019
ms.openlocfilehash: 92b6737f48d8d8704f461c9adac92284b323b05f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274352"
---
# <a name="connect-operations-manager-to-azure-monitor"></a>İşlem Yöneticisini Azure Monitörüne Bağlayın

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

System Center Operations [Manager'daki](https://docs.microsoft.com/system-center/scom/key-concepts?view=sc-om-1807) mevcut yatırımınızı sürdürmek ve Azure Monitor ile genişletilmiş yetenekleri kullanmak için, İşlem Yöneticisi'ni Log Analytics çalışma alanınızla tümleştirebilirsiniz. Bu, Azure Monitor'daki günlük fırsatlarından yararlanırken Operations Manager'ı aşağıdakiler için kullanmaya devam etmenizi sağlar:

* Operations Manager ile BT hizmetlerinizin durumunu izleyebilirsiniz.
* Olay ve sorun yönetimini destekleyen ITSM çözümlerinizle tümleştirmeyi koruyabilirsiniz.
* Operations Manager ile izlediğiniz şirket içi ve genel bulut IaaS sanal makinelerine dağıtılmış aracıların yaşam döngüsünü yönetebilirsiniz

System Center Operations Manager ile tümleştirme, İşlem Yöneticisi'nden günlük verilerini toplama, depolama ve çözümlemede Azure Monitor'un hızını ve verimliliğini kullanarak hizmet işlemleri stratejinize değer katar. Azure Monitor günlük sorguları, varolan sorun yönetimi sürecinizi desteklemek için sorunların ve yüzey yinelemelerinin hatalarını belirlemeye yardımcı olur. Sorgu motorunun performans, olay ve uyarı verilerini, zengin panolar ve raporlama özellikleriyle bu verileri anlamlı şekillerde ortaya çıkarma esnekliği, Azure Monitor'un Operasyon Yöneticisi'ne iltifat etme de getirdiği gücü gösterir.

Operasyon Yöneticisi yönetim grubuna rapor eden aracılar, çalışma alanınızda etkinleştirdiğiniz [Log Analytics veri kaynaklarına](agent-data-sources.md) ve çözümleri temel alınarak sunucularınızdan veri toplar. Etkinleştirilen çözümlere bağlı olarak, verileri doğrudan bir Operations Manager yönetim sunucusundan hizmete gönderilir veya aracı tarafından yönetilen sistemde toplanan veri hacmi nedeniyle doğrudan aracıdan Log Analytics çalışma alanına gönderilir. Yönetim sunucusu verileri doğrudan hizmete iletir; bunlar hiçbir zaman işlem veya veri ambarı veritabanına yazılmaz. Bir yönetim sunucusu Azure Monitor ile bağlantı sını kaybettiğinde, iletişim yeniden kurulana kadar verileri yerel olarak önbelleğe ayarır. Planlı bakım veya planlanmamış kesinti nedeniyle yönetim sunucusu çevrimdışıysa, yönetim grubundaki başka bir yönetim sunucusu Azure Monitor ile bağlantı kurmaya devam eder.  

Aşağıdaki diyagram, sistem merkezi operasyon yöneticisi yönetim grubundaki yönetim sunucuları ve aracıları ile yön ve bağlantı noktaları da dahil olmak üzere Azure Monitor arasındaki bağlantıyı gösterir.

![oms-operations-manager-integration-diagram](./media/om-agents/oms-operations-manager-connection.png)

BT güvenlik ilkeleriniz ağınızdaki bilgisayarların Internet'e bağlanmasına izin vermiyorsa, yönetim sunucuları yapılandırma bilgilerini almak ve çözümlere bağlı olarak toplanan verileri göndermek için Log Analytics ağ geçidine bağlanacak şekilde yapılandırılabilir Etkin. Azure Monitor'a bir Log Analytics ağ geçidi aracılığıyla iletişim kuracak şekilde Operasyon Yöneticisi yönetim grubunuzu yapılandırmakonusunda daha fazla bilgi ve adımlar için, [Günlük Analizi ağ geçidini kullanarak bilgisayarları Azure Monitor'a](../../azure-monitor/platform/gateway.md)bağlayın'a bakın.  

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdaki gereksinimleri gözden geçirin.

* Azure Monitor yalnızca System Center Operations Manager 2016 veya sonraki, Operations Manager 2012 SP1 UR6 veya sonraki ve Operations Manager 2012 R2 UR2 veya sonraki destekler. Operations Manager 2012 SP1 UR7 ve Operations Manager 2012 R2 UR3'e ara sunucu desteği eklenmiştir.
* System Center Operations Manager 2016'yı ABD Hükümeti bulutuyla tümleştirmek için Güncelleme Rollup 2 veya sonraki düzey bir güncelleştirmeyle birlikte güncelleştirilmiş bir Danışman yönetim paketi gerektirir. System Center Operations Manager 2012 R2, Update Rollup 3 veya sonraki ile birlikte güncelleştirilmiş bir Danışman yönetim paketi gerektirir.
* Tüm Operations Manager aracılarının en düşük destek gereksinimlerini karşılaması gerekir. Aracıların minimum güncelleştirmede olduğundan emin olun, aksi takdirde Windows aracı iletişimi başarısız olabilir ve Operations Manager olay günlüğünde hatalara neden olabilir.
* Log Analytics çalışma alanı. Daha fazla bilgi için [Log Analytics çalışma alanına genel bakışı](design-logs-deployment.md)gözden geçirin. 
* [Log Analytics Katılımcısı rolünün](manage-access.md#manage-access-using-azure-permissions)bir üyesi olan bir hesapla Azure'a kimlik doğrulaması yapıyorsun.

* Desteklenen Bölgeler - Bir Log Analytics çalışma alanına bağlanmak için yalnızca aşağıdaki Azure bölgeleri Sistem Merkezi Operasyon Yöneticisi tarafından desteklenir:
    - Orta Batı ABD
    - Avustralya Güneydoğu
    - Batı Avrupa
    - Doğu ABD
    - Güneydoğu Asya
    - Doğu Japonya
    - Güney Birleşik Krallık
    - Orta Hindistan
    - Orta Kanada
    - Batı ABD 2

>[!NOTE]
>Azure API'lerinde yapılan son değişiklikler, müşterilerin yönetim grubu yla Azure Monitörü arasındaki tümleştirmeyi ilk kez başarıyla yapılandırabilmelerini önler. Yönetim gruplarını hizmetle zaten bütünleştirmiş müşteriler için, varolan bağlantınızı yeniden yapılandırmanız gerekmedikçe etkilenmezsiniz.  
>Operations Manager'ın aşağıdaki sürümleri için yeni bir yönetim paketi yayımlanmıştır:
> - System Center Operations Manager 2019 için bu yönetim paketi kaynak ortama dahil edilir ve yeni bir yönetim grubunun kurulumu sırasında veya yükseltme sırasında yüklenir.
>- Operations Manager 1801 yönetim paketi, Operations Manager 1807 için de geçerlidir.
>- System Center Operations Manager 1801 için yönetim paketini [buradan](https://www.microsoft.com/download/details.aspx?id=57173)indirebilirsiniz.
>- Sistem Merkezi 2016 - Operasyon Yöneticisi için yönetim paketini [buradan](https://www.microsoft.com/download/details.aspx?id=57172)indirebilirsiniz.  
>- Sistem Merkezi Operasyon Yöneticisi 2012 R2 için yönetim paketini [buradan](https://www.microsoft.com/download/details.aspx?id=57171)indirebilirsiniz.  


### <a name="network"></a>Ağ

Aşağıdaki bilgiler, Azure Monitor ile iletişim kurmak için Operations Manager aracısı, yönetim sunucuları ve Operations konsolu için gereken proxy ve güvenlik duvarı yapılandırma bilgilerini listelemektedir. Her bileşenden gelen trafik ağınızdan Azure Monitor'a giden tir.

|Kaynak | Bağlantı noktası numarası| HTTP İncelemesini atlama|  
|---------|------|-----------------------|  
|**Aracı**|||  
|\*.ods.opinsights.azure.com| 443 |Evet|  
|\*.oms.opinsights.azure.com| 443|Evet|  
|\*.blob.core.windows.net| 443|Evet|  
|\*.azure-automation.net| 443|Evet|  
|**Yönetim sunucusu**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Evet|  
|\*.ods.opinsights.azure.com| 443| Evet|  
|*.azure-automation.net | 443| Evet|  
|**Azure Monitor'a Operasyon Yöneticisi konsolu**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 ve 443||  
|\*.microsoft.com| 80 ve 443||  
|\*.microsoftonline.com| 80 ve 443||  
|\*.mms.microsoft.com| 80 ve 443||  
|login.windows.net| 80 ve 443||  
|portal.loganalytics.io| 80 ve 443||
|api.loganalytics.io| 80 ve 443||
|docs.loganalytics.io| 80 ve 443||  

### <a name="tls-12-protocol"></a>TLS 1.2 protokolü

Azure Monitor'a aktarılan verilerin güvenliğini sağlamak için, aracıyı ve yönetim grubunu en az Aktarım Katmanı Güvenliği (TLS) 1.2 kullanacak şekilde yapılandırmanızı önemle öneririz. TLS/Secure Sockets Layer'ın (SSL) eski sürümlerinin savunmasız olduğu tespit edilmiştir ve hala geriye dönük uyumluluğa izin vermek için **çalışırken, bunlar önerilmez.** Daha fazla bilgi için [TLS 1.2'yi kullanarak veri gönderme'yi](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12)güvenli bir şekilde gözden geçirin.

## <a name="connecting-operations-manager-to-azure-monitor"></a>İşlem yöneticisini Azure Monitörüne Bağlama

Operations Manager yönetim grubunuzu Log Analytics çalışma alanlarınızdan birine bağlanacak şekilde yapılandırmak için aşağıdaki adım serisini uygulayın.

İşlem Yöneticisi yönetim grubunuzun Bir Log Analytics çalışma alanına ilk kaydı sırasında, yönetim grubu için proxy yapılandırmasını belirtme seçeneği İşlemler konsolunda kullanılamaz.  Bu seçeneğin sağlanması için önce yönetim grubunun hizmete başarıyla kaydedilmiş olması gerekir.  Bunu aşmak için, sistemdeki Netsh'i kullanarak sistem proxy yapılandırmasını güncelleştirmeniz gerekir, işlemleri yapılandırmak için Operations konsolunu çalıştırdığınızdan ve yönetim grubundaki tüm yönetim sunucularını.  

1. Yükseltilmiş bir komut istemi açın.
   a. **Başlat'a** gidin ve **cmd**yazın.
   b. Komut **istemi'ne** sağ tıklayın ve yönetici olarak Çalıştır'ı seçin**.
1. Aşağıdaki komutu girin ve **Enter** tuşuna basın:

    `netsh winhttp set proxy <proxy>:<port>`

Azure Monitor ile tümleştirme için aşağıdaki adımları tamamladıktan `netsh winhttp reset proxy` sonra, çalıştırarak yapılandırmayı kaldırabilir ve ardından Proxy veya Log Analytics ağ geçidi sunucusunu belirtmek için Operations konsolundaki **Yapılproxy sunucusu** seçeneğini kullanabilirsiniz.

1. Operations Manager konsolunda **Yönetim** çalışma alanını seçin.
1. Operations Management Suite düğümünü genişletin ve **Bağlantı**'ya tıklayın.
1. **Operations Management Suite'e kaydolun** bağlantısına tıklayın.
1. **Operations Management Suite Ekleme Sihirbazı: Kimlik Doğrulama** sayfasında, OMS aboneliğinizle ilişkilendirilmiş yönetici hesabının e-posta adresi veya telefon numarasını ve parolasını girin ve **Oturum aç**'a tıklayın.

   >[!NOTE]
   >Operasyon Yönetimi Paketi adı kullanımdan kaldırıldı.

1. Başarılı bir şekilde kimlik doğrulaması alındıktan sonra, **Operations Management Suite Onboarding Wizard: Select Workspace** sayfasında, Azure kiracınızı, aboneliğinizi ve Log Analytics çalışma alanınızı seçmeniz istenir. Birden çok çalışma alanınız varsa, açılan listeden Operations Manager yönetim grubuna kaydetmek istediğiniz çalışma alanını seçin ve ardından **İleri**'ye tıklayın.

   > [!NOTE]
   > Operations Manager bir kerede tek bir Log Analytics çalışma alanını destekler. Önceki çalışma alanıyla Azure Monitor'a kayıtlı olan bağlantı ve bilgisayarlar Azure Monitor'dan kaldırılır.
   >
   >
1. **Operations Management Suite Ekleme Sihirbazı: Özet** sayfasında ayarlarınızı onaylayın ve bunlar doğruysa **Oluştur**'a tıklayın.
1. **Operations Management Suite Ekleme Sihirbazı: Son** sayfasında **Kapat**'a tıklayın.

### <a name="add-agent-managed-computers"></a>Aracı tarafından yönetilen bilgisayarlar ekleme

Log Analytics çalışma alanınızla tümleştirmeyi yapılandırdıktan sonra, yalnızca hizmetle bağlantı kurar, yönetim grubunuza rapor veren aracılardan veri toplanmaz. Bu, hangi aracı tarafından yönetilen bilgisayarların Azure Monitor için günlük verilerini topladığını yapılandırana kadar gerçekleşmez. Bilgisayar nesnelerini tek tek seçebileceğiniz gibi, Windows bilgisayar nesnelerini içeren bir grup da seçebilirsiniz. Mantıksal diskler veya SQL veritabanları gibi başka bir sınıfın örneklerini içeren grupları seçemezsiniz.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. Operations Management Suite düğümünü genişletin ve **Bağlantı**'ya tıklayın.
1. Bölmenin sağ tarafındaki Eylemler başlığı altında **Bilgisayar/Grup Ekle** bağlantısına tıklayın.
1. Bilgisayar **Arama** iletişim kutusunda, Operations Manager tarafından izlenen bilgisayarları veya grupları arayabilirsiniz. Azure Monitor'a dahil olmak üzere Operations Manager Management Server gibi bilgisayarları veya grupları seçin, **Ekle'yi**tıklatın ve ardından **Tamam'ı**tıklatın.

İşletim konsolunun **Yönetim** çalışma alanında Operations Manager Suite'in altındaki Yönetilen Bilgisayarlar düğümünden, veri toplamak için yapılandırılmış bilgisayarları ve grupları görüntüleyebilirsiniz. Burada, gerekirse bilgisayarları ve grupları ekleyebilir veya kaldırabilirsiniz.

### <a name="configure-proxy-settings-in-the-operations-console"></a>İşletim konsolunda ara sunucu ayarlarını yapılandırma

Yönetim grubu ile Azure Monitor arasında bir dahili proxy sunucusu varsa aşağıdaki adımları gerçekleştirin. Bu ayarlar, yönetim grubundan merkezi olarak yönetilir ve Azure Monitor için günlük verilerini toplamak için kapsama dahil edilen aracı tarafından yönetilen sistemlere dağıtılır.  Bazı çözümlerin yönetim sunucusunu atladığı ve doğrudan hizmete veri gönderdiği durumlarda, bu yararlı olur.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. Operations Management Suite'i genişletin ve **Bağlantılar**'a tıklayın.
1. OMS Bağlantısı görünümünde, **Ara Sunucuyu Yapılandır**'a tıklayın.
1. **Operations Management Suite Sihirbazı: Ara Sunucu** sayfasında **Operations Management Suite erişimi için bir ara sunucu kullan**'ı seçin, ardından bağlantı noktası numarasını içeren URL'yi yazın (örneğin, http://corpproxy:80) ve **Son**'a tıklayın.

Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, yönetim grubunda Azure Monitor'a rapor veren yönetilen bilgisayarlara yayılması gereken kimlik bilgilerini ve ayarlarını yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. **RunAs Yapılandırması** altında, **Profiller**'i seçin.
1. **System Center Advisor Farklı Çalıştır Profili Ara Sunucusu** profilini açın.
1. Farklı Çalıştır Profili Sihirbazı'nda, bir Farklı Çalıştır hesabı kullanmak için Ekle'ye tıklayın. [Farklı Çalıştır hesabı](https://technet.microsoft.com/library/hh321655.aspx) oluşturabilir veya mevcut bir hesabı kullanabilirsiniz. Doğrudan ara sunucuya geçiş yapmak için bu hesabın yeterli izinlere sahip olması gerekir.
1. Yönetilecek hesabı belirlemek için, **Seçilen bir sınıf, grup veya nesne**'yi seçin, **Seç...** düğmesine tıklayın ve ardından **Grup...** öğesine tıklayarak **Grup Araması** kutusunu açın.
1. **Microsoft System Center Advisor İzleme Sunucusu Grubu**'nu arayın ve ardından seçin. Grubu seçtikten sonra **Grup Araması** kutusunu kapatmak için **Tamam**'a tıklayın.
1. **Hesap Olarak Ekle** kutusunu kapatmak için **Tamam'ı** tıklatın.
1. Sihirbazı tamamlamak ve yaptığınız değişiklikleri kaydetmek için **Kaydet**'e tıklayın.

Bağlantı oluşturulduktan ve hangi aracıların azure monitor'a günlük verilerini toplayıp rapor edeceğini yapılandırdıktan sonra, aşağıdaki yapılandırma yönetim grubunda uygulanır, sırasıyla sırayla değil:

* **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** Farklı Çalıştır Hesabı oluşturulur. Bu hesap **Microsoft System Center Advisor Farklı Çalıştır Profili Blobu** Farklı Çalıştır profiliyle ilişkilendirilmiştir ve iki sınıfı hedefler: **Koleksiyon Sunucusu** ve **Operations Manager Yönetim Grubu**.
* İki bağlayıcı oluşturulur.  İlki **Microsoft.SystemCenter.Advisor.DataConnector** olarak adlandırılır ve yönetim grubundaki tüm sınıfların örneklerinden oluşturulan tüm uyarıları Azure Monitor'a ileten bir abonelikle otomatik olarak yapılandırılır. İkinci bağlayıcı, Azure Monitor ile iletişim kurmak ve verileri paylaşmaktan sorumlu **Olan Advisor Connector'dır.**
* Yönetim grubunda veri toplamak için seçilmiş olan aracılar ve gruplar **Microsoft System Center Advisor Sunucu İzleme Grubu**'na eklenir.

## <a name="management-pack-updates"></a>Yönetim paketi güncelleştirmeleri

Yapılandırma tamamlandıktan sonra, İşlem Yöneticisi yönetim grubu Azure Monitor ile bir bağlantı kurar. Yönetim sunucusu web hizmetiyle eşitlenir ve Operations Manager'la tümleştirilmek üzere etkinleştirmiş olduğunuz çözümler için yönetim paketleri biçiminde güncelleştirilmiş yapılandırma bilgilerini alır. Operations Manager bu yönetim paketlerinin güncelleştirmelerini denetler ve kullanılabilir olduklarında otomatik olarak karşıdan yükleyip içeri yükler. Bu davranışı denetleyen özellikle iki kural vardır:

* **Microsoft.SystemCenter.Advisor.MPUpdate** - Temel Azure Monitor yönetim paketlerini güncelleştirir. Varsayılan olarak her 12 saatte bir çalıştırılır.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - Çalışma alanınızda etkinleştirilmiş olan çözüm yönetim paketlerini güncelleştirir. Varsayılan olarak her beş (5) dakikada bir çalıştırılır.

Devre dışı bırakarak otomatik karşıdan yüklemeyi önlemek için bu iki kuralı geçersiz kılabilir veya yeni bir yönetim paketinin kullanılabilir olup olmadığını ve indirilmesi gerekip gerekeceğini belirlemek için yönetim sunucusunun Azure Monitor ile ne sıklıkta eşitlediği sıklığını değiştirebilirsiniz. **Frequency** parametresini saniye cinsinden bir değerle değiştirip eşitleme zamanlamasında değişiklik yapmak veya **Enabled** parametresini değiştirip kuralları devre dışı bırakmak için, [Kuralı veya İzlemeyi Geçersiz Kılma](https://technet.microsoft.com/library/hh212869.aspx) altındaki adımları izleyin. Geçersiz kılmalarda, Operations Manager Yönetim Grubu sınıfındaki tüm nesneleri hedefleyin.

Üretim yönetim grubunuzdaki yönetim paketi sürümlerini denetlemek için varolan değişiklik denetim sürecinizi takip etmeye devam etmek için, kuralları devre dışı bırakabilir ve güncelleştirmelere izin verilen belirli zamanlarda bunları etkinleştirebilirsiniz. Ortamınızda bir geliştirme veya QA yönetim grubu varsa ve İnternet'e bağlıysa, bu senaryoyu desteklemek için söz konusu yönetim grubunu Log Analytics çalışma alanıyla yapılandırabilirsiniz. Bu, Azure Monitörü yönetim paketlerinin yinelemeli sürümlerini üretim yönetim grubunuzun içine bırakmadan önce gözden geçirmenize ve değerlendirmenize olanak tanır.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Operations Manager grubunu yeni bir Log Analytics Çalışma Alanına geçirme

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**'i seçin ve bir çalışma alanı oluşturun.  
1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. Günlük Analitiğini genişletin ve **Bağlantılar'ı**seçin.
1. Bölmenin orta kısmında **Operation Management Suite'i Yeniden Yapılandır** bağlantısını seçin.
1. Günlük **Analizi Onboarding** Sihirbazı'nı izleyin ve yeni Log Analytics çalışma alanınızla ilişkili yönetici hesabının e-posta adresini veya telefon numarasını ve parolasını girin.

   > [!NOTE]
   > **Operations Management Suite Ekleme Sihirbazı: Çalışma Alanı Seçin** sayfasında kullanımda olan mevcut çalışma alanı gösterilir.
   >
   >

## <a name="validate-operations-manager-integration-with-azure-monitor"></a>Azure Monitörile İşlem Yöneticisi Tümleştirmeyi Doğrula

Azure Monitor to Operations Manager tümleştirmesi'nin başarılı olduğunu doğrulamanın birkaç farklı yolu vardır.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Azure portalında tümleştirmeyi onaylamak için

1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir.
1. Log Analytics çalışma alanlarınızın listesinde uygun çalışma alanını seçin.  
1. **Gelişmiş ayarlar**'ı, **Bağlı Kaynaklar**'ı ve sonra da **System Center**'ı seçin.
1. System Center Operations Manager bölümünün altındaki tabloda yönetim grubu adının listelendiğini, ayrıca aracı sayısının ve veriler son alındığındaki durumun gösterildiğini görüyor olmalısınız.

   ![oms-settings-connectedsources](./media/om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>İşletim konsolunda tümleştirmeyi onaylamak için

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. **Yönetim Paketleri**'ni seçin ve **Aranan:** metin kutusuna **Advisor** veya **Intelligence** yazın.
1. Etkinleştirdiğiniz çözümlere bağlı olarak, arama sonuçlarında ilgili yönetim paketinin listelendiğini görürsünüz.  Örneğin Uyarı Yönetimi çözümünü etkinleştirdiyseniz, listede Microsoft System Center Advisor Uyarı Yönetimi yönetim paketi yer alır.
1. **İzleme** görünümünden **Operations Management Suite\Sistem Durumu** görünümüne gidin.  **Yönetim Sunucusu Durumu** bölmesinin altında bir Yönetim sunucusu seçin ve **Ayrıntı Görünümü** bölmesinde **Kimlik doğrulama hizmeti URI'si** özelliğinin değerinin Log Analytics Çalışma Alanı Kimliği ile eşleştiğini onaylayın.

   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-azure-monitor"></a>Azure Monitörü ile Tümleştirmeyi Kaldırma

Artık Operations Manager yönetim grubunuzda Log Analytics çalışma alanı arasında tümleştirmeye ihtiyacınız kalmadığında, yönetim grubunda bağlantıyı ve yapılandırmayı düzgün kaldırmak için izlenmesi gereken bazı adımlar vardır. Aşağıdaki yordam, yönetim grubunuzun başvurularını silerek Log Analytics çalışma alanınızı güncelleştirmenizi, Azure Monitor bağlayıcılarını silmenizi ve ardından hizmetle tümleştirmeyi destekleyen yönetim paketlerini silmenizi sağlar.  

Operations Manager ile tümleştirme yi etkinleştirdiğiniz çözümler için yönetim paketleri ve Azure Monitor ile tümleştirmeyi desteklemek için gereken yönetim paketleri yönetim grubundan kolayca silinemez. Bunun nedeni, Azure Monitöryönetim paketlerinden bazılarının ilgili diğer yönetim paketlerine bağımlı olmasıdır. Diğer yönetim paketlerinde bağımlılıkları olan yönetim paketlerini silmek için, TechNet Betik Merkezi'nden [bağımlılıkları olan yönetim paketini kaldırma](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) betiğini indirin.  

1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager Komut Kabuğu'nu açın.

    > [!WARNING]
    > Devam etmeden önce adında Advisor veya IntelligencePack terimi bulunan hiçbir özel yönetim paketiniz olmadığını doğrulayın; aksi takdirde, aşağıdaki adımları o paketleri de yönetim grubundan siler.
    >

1. Komut kabuğu istemcisine `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue` yazın
1. Sonra `Get-SCOMManagementPack -name "*IntelligencePack*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue` yazın
1. Diğer System Center Advisor yönetim paketlerinde bağımlılığı olan kalan yönetim paketlerini kaldırmak için, daha önce TechNet Betik Merkezi'nden indirmiş olduğunuz *RecursiveRemove.ps1* betiğini kullanın.  

    > [!NOTE]
    > PowerShell ile Danışman yönetim paketlerini kaldırma adımı, Microsoft System Center Advisor veya Microsoft System Center Advisor Internal management paketlerini otomatik olarak silmez.  Silmeye çalışmayın.  
    >  

1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager İşletim konsolunu açın.
1. **Yönetim**'in altında **Yönetim Paketleri** düğümünü açın, **Aranan:** kutusuna **Advisor** yazın ve aşağıdaki yönetim paketlerinin yönetim grubunuza aktarılmış durumda olduğunu doğrulayın:

   * Microsoft System Center Advisor
   * Microsoft System Center Advisor Internal

1. Azure portalında **Ayarlar** döşemesini tıklatın.
1. **Bağlı Kaynakları**seçin.
1. Sistem Merkezi Operasyon Yöneticisi bölümünün altındaki tabloda, çalışma alanından kaldırmak istediğiniz yönetim grubunun adını görmeniz gerekir. **Son Veriler** sütununun altında **Kaldır**'a tıklayın.  

    > [!NOTE]
    > Bağlı yönetim grubundan hiçbir etkinlik algılanmazsa 14 gün geçene kadar **Kaldır** bağlantısı kullanılamaz.  
    >

1. Kaldırma işlemine devam etmek istediğinizi onaylamanızı isteyen bir pencere görüntülenir.  Devam etmek için **Evet**'e tıklayın.

İki bağlayıcıyı (Microsoft.SystemCenter.Advisor.DataConnector ve Advisor Connector) silmek için, aşağıdaki PowerShell betiğini bilgisayarınıza kaydedin ve aşağıdaki örnekleri kullanarak yürütün:

```
    .\OM2012_DeleteConnectors.ps1 "Advisor Connector" <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 "Microsoft.SystemCenter.Advisor.DataConnector" <ManagementServerName>
```

> [!NOTE]
> Bu betiği çalıştırdığınız bilgisayar bir yönetim sunucusu değilse, yönetim grubunuzun sürümüne bağlı olarak bu bilgisayarda Operations Manager komut kabuğunun yüklü olması gerekir.
>
>

```powershell
    param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

Gelecekte yönetim grubunuzu bir Log Analytics çalışma alanına yeniden bağlamayı planlıyorsanız, yönetim `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` paketi dosyasını yeniden içe aktarmanız gerekir. Ortamınıza dağıtılan System Center Operations Manager sürümüne bağlı olarak bu dosyayı aşağıdaki konumda bulabilirsiniz:

* System Center 2016 - Operations Manager ve üstü için kaynak medyada `\ManagementPacks` klasörünün altında.
* Yönetim grubunuza uygulanan en son güncelleştirme dağıtımından. Operations Manager 2012 için, `%ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` kaynak klasör ve 2012 R2 `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`için, içinde yer alır.

## <a name="next-steps"></a>Sonraki adımlar

İşlev selavar etmek ve veri toplamak [için, Çözüm Galerisi'nden Azure Monitörü Ekle çözümlerine](../../azure-monitor/insights/solutions.md)bakın.
