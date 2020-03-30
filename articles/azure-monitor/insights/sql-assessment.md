---
title: Azure Monitor ile SQL Server ortamınızı optimize edin | Microsoft Dokümanlar
description: Azure Monitor ile, ortamlarınızın risk ve sistem durumunu düzenli aralıklarla değerlendirmek için SQL Sistem Durumu Denetimi çözümünü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/28/2019
ms.openlocfilehash: ceaed0800df01bf2c44fee13d98b01b6e726200d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77662493"
---
# <a name="optimize-your-sql-environment-with-the-sql-server-health-check-solution-in-azure-monitor"></a>Azure Monitor'daki SQL Server Health Check çözümüyle SQL ortamınızı optimize edin

![SQL Sistem Durumu Denetimi simgesi](./media/sql-assessment/sql-assessment-symbol.png)

Sunucu ortamlarınızın risk ve sistem durumunu düzenli aralıklarla değerlendirmek için SQL Sistem Durumu Denetimi çözümünü kullanabilirsiniz. Bu makalede, olası sorunlar için düzeltici eylemlerde bulunabilir, böylece çözüm yüklemenize yardımcı olacaktır.

Bu çözüm, dağıtılmış sunucu altyapınıza özgü öncelikle öneriler listesi sağlar. Öneriler, riski hızlı bir şekilde anlamanıza ve düzeltici eylemde bulunmanıza yardımcı olan altı odak alanında sınıflandırılır.

Yapılan öneriler, Microsoft mühendislerinin binlerce müşteri ziyaretinden edindikleri bilgi ve deneyime dayanmaktadır. Her öneri, bir sorunun sizin için neden önemli olabileceği ve önerilen değişikliklerin nasıl uygulanacağı hakkında rehberlik sağlar.

Kuruluşunuz için en önemli olan odak alanlarını seçebilir ve risksiz ve sağlıklı bir ortam çalıştırma yolundaki ilerlemenizi izleyebilirsiniz.

Çözümü ekledikten ve bir değerlendirme tamamlandıktan sonra, odak alanlarına ait özet bilgiler ortamınızdaki altyapı için **SQL Sistem Durumu Denetimi** panosunda gösterilir. Aşağıdaki bölümlerde, SQL Server altyapınız için önerilen eylemleri görüntüleyip sonra da yapabileceğiniz **SQL Sistem Durumu Denetimi** panosundaki bilgilerin nasıl kullanılacağı açıklanmıştır.

![SQL Health Check döşemesi görüntüsü](./media/sql-assessment/sql-healthcheck-summary-tile.png)

![SQL Sistem Durumu Kontrol panosunun görüntüsü](./media/sql-assessment/sql-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Ön koşullar

* SQL Sistem Durumu Denetimi çözümü, Microsoft İzleme Aracısı (MMA) yüklü olan her bilgisayarda yüklü olan .NET Framework 4.6.2'nin desteklenen bir sürümünü gerektirir.  MMA aracısı System Center 2016 - Operasyon Yöneticisi ve Operasyon Yöneticisi 2012 R2 ve Azure Monitor tarafından kullanılır.  
* Çözüm, SQL Server sürüm 2012, 2014 ve 2016'yı destekler.
* Azure portalındaki Azure pazar yerinden SQL Sistem Durumu Denetimi çözümlerini eklemek için bir Günlük Analizi çalışma alanı.  Çözümü yüklemek için Azure aboneliğinde yönetici veya katkıda bulunan kişi olmalısınız.

  > [!NOTE]
  > Çözümü ekledikten sonra AdvisorAssessment.exe dosyası aracılı sunuculara eklenir. Yapılandırma verileri okunur ve işlenmek üzere buluttaki Azure Monitörü'ne gönderilir. Alınan verilere mantık uygulanır ve bulut hizmeti verileri kaydeder.
  >
  >

SQL Server sunucularınıza karşı sistem durumu denetimini gerçekleştirmek için, aşağıdaki desteklenen yöntemlerden birini kullanarak Azure Monitor'a bir aracı ve bağlantı gerektirir:

1. Sunucu System Center 2016 - Operations Manager veya Operations Manager 2012 R2 tarafından zaten izlenmiyorsa [Microsoft İzleme Aracısını (MMA)](../../azure-monitor/platform/agent-windows.md) yükleyin.
2. System Center 2016 ile izlenirse - Operations Manager veya Operations Manager 2012 R2 ve yönetim grubu Azure Monitor ile entegre değilse, sunucu veri toplamak ve hizmete iletmek için Log Analytics ile çok sayıda yuvaya sahip olabilir ve yine de Operasyon Müdürü tarafından izlenir.  
3. Aksi takdirde, Operations Manager yönetim grubunuz hizmetle bütünleşmişse, çözüme olanak verdikten sonra iş yeriniz tarafından [yönetilen bilgisayarlar ekle](../../azure-monitor/platform/om-agents.md#connecting-operations-manager-to-azure-monitor) adımları altında nağmeler altında veri toplama için etki alanı denetleyicilerini eklemeniz gerekir.  

SQL Server'ınızdaki bir İşlem Yöneticisi yönetim grubuna rapor veren, veri toplayan, atanan yönetim sunucusuna ileten ve doğrudan bir yönetim sunucusundan Azure Monitor'a gönderilen aracı.  Veriler Operasyon Yöneticisi veritabanlarına yazılmadı.  

SQL Server Operations Manager tarafından izleniyorsa, bir Operasyon Yöneticisi Run Hesabı olarak yapılandırmanız gerekir. Daha fazla bilgi için aşağıda [Azure Monitor için Operations Manager run-as hesaplarına](#operations-manager-run-as-accounts-for-log-analytics) bakın.

## <a name="sql-health-check-data-collection-details"></a>SQL Sistem Durumu Denetimi veri toplama ayrıntıları
SQL Health Check, etkinleştirdiğiniz aracıyı kullanarak aşağıdaki kaynaklardan veri toplar:

* Windows Yönetim Araçları (WMI)
* Kayıt Defteri
* Performans sayaçları
* SQL Server dinamik yönetim görünümü sonuçları

Veriler SQL Server'da toplanır ve her yedi günde bir Log Analytics'e iletilir.

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Log Analytics için Operations Manager farklı çalıştır hesapları
Log Analytics, Log Analytics hizmetine veri toplamak ve göndermek için Operasyon Yöneticisi aracısını ve yönetim grubunu kullanır. Log Analytics, katma değerli hizmetler sağlamak için iş yükleri için yönetim paketleri üzerine inşa edin. Her iş yükü, yönetim paketlerini etki alanı kullanıcı hesabı gibi farklı bir güvenlik bağlamında çalıştırmak için iş yüküne özgü ayrıcalıklar gerektirir. Bir Operasyon Yöneticisi Run As hesabını yapılandırarak kimlik bilgisi sağlamanız gerekir.

İşlemyöneticisi Run'ı SQL Sistem Durumu Denetimi için hesap olarak ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account-for-sql-health-check"></a>SQL Sistem Durumu Denetimi için Farklı Çalıştır hesabını ayarlama
 SQL Server yönetim paketini zaten kullanıyorsanız, bu Çalıştır yapılandırmasını kullanmanız gerekir.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>İşlemler konsolunda SQL Run Hesabı'nı yapılandırmak için
> [!NOTE]
> Yönetim paketindeki varsayılan iş akışları Yerel Sistem hesabının güvenlik bağlamında çalışır. Bir İşlem Yöneticisi yönetim grubuna doğrudan rapor vermek yerine doğrudan hizmete bağlı Microsoft İzleme Aracısı kullanıyorsanız, aşağıdaki 1-5 adımını atlayın ve NT AUTHORITY\SYSTEM olarak belirterek T-SQL veya PowerShell örneğini çalıştırın kullanıcı adı.
>
>

1. Operations Manager'da, Operations konsolunu açın ve ardından **Yönetim'i**tıklatın.
2. **Yapılandırma Olarak Çalıştır** **altında, Profiller'i**tıklatın ve SQL Assessment Run As **Profile'ı**açın.
3. **Farklı Çalıştır Hesapları** sayfasında **Ekle**’ye tıklayın.
4. SQL Server için gereken kimlik bilgilerini içeren bir Windows Çalıştır Hesabı seçin veya oluşturmak için **Yeni'yi** tıklatın.

   > [!NOTE]
   > Hesap Olarak Çalıştır türü Windows olmalıdır. Çalıştır Hesabı, SQL Server Örneklerini barındıran tüm Windows Sunucularında Yerel Yöneticiler grubunun bir parçası olmalıdır.
   >
   >
5. **Kaydet**'e tıklayın.
6. Sistem durumu denetimini gerçekleştirmek için Run As Hesabı için gereken minimum izinleri vermek için her SQL Server örneğinde aşağıdaki T-SQL örneğini değiştirin ve çalıştırın. Ancak, bir Run As Hesabı zaten SQL Server örneklerinde sysadmin sunucu rolünün bir parçası ise bunu yapmanız gerekmez.

```
    ---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```

#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Windows PowerShell kullanarak SQL Run As hesabını yapılandırmak için
Bir PowerShell penceresi açın ve bilgilerinizle güncelledikten sonra aşağıdaki komut dosyasını çalıştırın:

```
    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Önerilerin önceliklerini belirlemeyi anlama
Yapılan her öneriye, önerinin göreli önemini tanımlayan bir ağırlık değeri verilir. Sadece en önemli on öneri gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıklar nasıl hesaplanır?
Ağırlıklandırmalar üç temel faktöre dayalı toplam değerlerdir:

* Tanımlanan bir sorunun sorunlara neden olma *olasılığı.* Daha yüksek bir olasılık, öneri için daha büyük bir genel puana eşittir.
* Sorunun kuruluşunuz üzerindeki *etkisi,* bir soruna neden oluyorsa. Daha yüksek bir etki, tavsiye için daha büyük bir genel puana eşittir.
* Tavsiyeyi uygulamak için gereken *çaba.* Daha yüksek bir çaba, tavsiye için daha küçük bir genel puana eşittir.

Her öneriiçin ağırlıklandırma, her odak alanı için kullanılabilir toplam puanın yüzdesi olarak ifade edilir. Örneğin, Güvenlik ve Uyumluluk odak alanındaki bir öneri %5 puana sahipse, bu öneriyi uygulamak genel Güvenlik ve Uyumluluk puanınızı %5 artırır.

### <a name="focus-areas"></a>Odak alanları
**Güvenlik ve Uyumluluk** - Bu odak alanı, potansiyel güvenlik tehditleri ve ihlalleri, şirket politikaları ve teknik, yasal ve mevzuata uygunluk gereklilikleri için öneriler gösterir.

**Kullanılabilirlik ve İş Sürekliliği** - Bu odak alanı, hizmet kullanılabilirliği, altyapınızın esnekliği ve iş koruması için öneriler gösterir.

**Performans ve Ölçeklenebilirlik** - Bu odak alanı, kuruluşunuzun BT altyapısının büyümesine yardımcı olacak, BT ortamınızın geçerli performans gereksinimlerini karşıladığından ve değişen altyapı gereksinimlerine yanıt verebilmesini sağlayacak öneriler gösterir.

**Yükseltme, Geçiş ve Dağıtım** - Bu odak alanı, SQL Server'ı varolan altyapınıza yükseltmenize, geçiş inizi ve dağıtmanıza yardımcı olacak öneriler gösterir.

**İşlemler ve İzleme** - Bu odak alanı, BT operasyonlarınızı kolaylaştırmaya, önleyici bakımı uygulamaya ve performansı en üst düzeye çıkarmaya yardımcı olacak öneriler gösterir.

**Değişim ve Yapılandırma Yönetimi** - Bu odak alanı, günlük operasyonların korunmasına yardımcı olacak, değişikliklerin altyapınızı olumsuz etkilememesini sağlamaya, değişiklik denetimi yordamlarını oluşturmaya ve sistem yapılandırmalarını izleme ve denetlemeye yardımcı olacak öneriler gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Her odak alanında %100 puanı mı hedeflemeniz gerekiyor?
Mutlaka. Öneriler, Microsoft mühendislerinin binlerce müşteri ziyaretinde edindikleri bilgi ve deneyimlere dayanmaktadır. Ancak, iki sunucu altyapısı aynı değildir ve belirli öneriler sizin için az çok alakalı olabilir. Örneğin, sanal makineleriniz Internet'e açık değilse, bazı güvenlik önerileri daha az alakalı olabilir. Bazı kullanılabilirlik önerileri, düşük öncelikli geçici veri toplama ve raporlama sağlayan hizmetler için daha az alakalı olabilir. Olgun bir işletme için önemli olan sorunlar, yeni kurulan bir işletme için daha az önemli olabilir. Önceliklerinizin hangi odak alanları olduğunu belirlemek ve puanlarınızın zaman içinde nasıl değiştiğine bakmak isteyebilirsiniz.

Her öneri, neden önemli olduğu hakkında rehberlik içerir. BT hizmetlerinizin doğası ve kuruluşunuzun iş gereksinimleri göz önüne alındığında, önerinin uygulanmasının sizin için uygun olup olmadığını değerlendirmek için bu kılavuzu kullanmalısınız.

## <a name="use-health-check-focus-area-recommendations"></a>Sistem Durumu Denetimi odak alanı önerilerini kullanma
Azure Monitor'da bir değerlendirme çözümünü kullanamadan önce çözümün yüklü olması gerekir.  Yüklendikten sonra, Azure portalındaki Azure Monitörü için **Genel Bakış** sayfasındaki SQL Sistem Durumu Denetimi döşemesini kullanarak önerilerin özetini görüntüleyebilirsiniz.

Altyapınız için özet uyumluluk değerlendirmelerini görüntüleyin ve ardından ayrıntılı önerilere sahip.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için önerileri görüntülemek ve düzeltici eylemde bulunmak için
1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **İzleyici** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **İzleyici**'yi seçin.
3. Menünün **Öngörüler** bölümünde Daha **Fazla'yı**seçin.  
4. Genel **Bakış** sayfasında, **SQL Sistem Durumu Denetimi** döşemesini tıklatın.
5. Sistem **Durumu Denetimi** sayfasında, odak alanı bıçaklarından birinde özet bilgileri gözden geçirin ve ardından bu odak alanı yla ilgili önerileri görüntülemek için bir tanesini tıklatın.
6. Odak alanı sayfalarından herhangi birinde, ortamınız için yapılan öncelikli önerileri görüntüleyebilirsiniz. Önerinin neden yapıldığıyla ilgili ayrıntıları görüntülemek için **Etkilenen Nesneler** altında bir öneriyi tıklatın.<br><br> ![SQL Sağlık Kontrolü önerilerinin görüntüsü](./media/sql-assessment/sql-healthcheck-dashboard-02.png)<br>
7. **Önerilen Eylemler'de**önerilen düzeltici eylemleri alabilirsiniz. Öğe ele alındığında, daha sonraki değerlendirmeler önerilen eylemlerin alındığını ve uyumluluk puanınızın artacağını kaydeder. Düzeltilen öğeler **Geçirilen Nesneler**olarak görünür.

## <a name="ignore-recommendations"></a>Önerileri yoksayma
Göz ardı etmek istediğiniz önerileriniz varsa, Azure Monitor'un değerlendirme sonuçlarınızda önerilerin görünmesini önlemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Yoksayacağınız önerileri tanımlamak için
1. Azure Monitörü menüsünde **Günlükler'i**tıklatın.
2. Ortamınızdaki bilgisayarlarda başarısız olan önerileri listelemek için aşağıdaki sorguyu kullanın.

    ```
    SQLAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```
    Burada günlük sorgusunu gösteren bir ekran görüntüsü var:<br><br> ![başarısız öneriler](./media/sql-assessment/sql-assess-failed-recommendations.png)<br>

3. Yoksaymak istediğiniz önerileri seçin. Sonraki yordamda RecommendationId için değerleri kullanırsınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>IgnoreRecommendations.txt metin dosyası oluşturmak ve kullanmak için
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Azure Monitor'un ayrı bir satırda yoksaymasını istediğiniz her öneri için her Öneri'yi yapıştırın veya yazın ve ardından dosyayı kaydedip kapatın.
3. Azure Monitor'un önerileri yok saymasını istediğiniz her bilgisayarda dosyayı aşağıdaki klasöre koyun.
   * Microsoft Monitoring Agent (doğrudan veya Operations Manager üzerinden bağlı) ile bilgisayarlarda - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent
   * Operations Manager yönetim sunucusunda - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server
   * Operations Manager 2016 yönetim sunucusunda - *SystemDrive*:\Program Files\Microsoft System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Önerilerin yoksayDığını doğrulamak için
1. Bir sonraki zamanlanmış değerlendirme çalıştırıldıktan sonra, varsayılan olarak her 7 günde bir, belirtilen öneriler Yoksayıldı olarak işaretlenir ve değerlendirme panosunda görünmez.
2. Göz ardı edilen tüm önerileri listelemek için aşağıdaki Günlük Arama sorgularını kullanabilirsiniz.

    ```
    SQLAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```
3. Daha sonra yoksayan önerileri görmek istediğinize karar verirseniz, YokSayöneriler.txt dosyalarını kaldırın veya RecommendationsID'leri kaldırabilirsiniz.

## <a name="sql-health-check-solution-faq"></a>SQL Sistem Durumu Denetimi çözümü SSS

*SQL Assessment çözümü tarafından hangi denetimler gerçekleştirilir?*

* Aşağıdaki sorgu, şu anda gerçekleştirilen tüm denetimlerin açıklamasını gösterir:

```Kusto
SQLAssessmentRecommendation
| distinct RecommendationId, FocusArea, ActionArea, Recommendation, Description
| sort by FocusArea,ActionArea, Recommendation
```
Sonuçlar daha sonra daha fazla gözden geçirilmek üzere Excel'e aktarılabilir.


*Sağlık kontrolü ne sıklıkta çalışır?*

* Çek her yedi günde bir çalışır.

*Denetimin ne sıklıkta çalıştığını yapılandırmanın bir yolu var mı?*

* Şu anda değil.

*SQL Sistem Durumu Denetimi çözümlerini ekledikten sonra başka bir sunucu keşfedilirse, bu durum kontrol edilir mi?*

* Evet, bir kez o andan itibaren, her yedi günde bir kontrol edilir keşfedilir.

*Bir sunucu kullanımdan kaldırılırsa, sistem durumu denetiminden ne zaman kaldırılır?*

* Bir sunucu 3 hafta boyunca veri göndermezse, kaldırılır.

*Veri toplama işleminin adı nedir?*

* AdvisorAssessment.exe

*Verilerin toplanması ne kadar sürer?*

* Sunucudaki gerçek veri toplama yaklaşık 1 saat sürer. Çok sayıda SQL örneği veya veritabanı olan sunucularda daha uzun sürebilir.

*Ne tür veriler toplanır?*

* Aşağıdaki veri türleri toplanır:
  * WMI
  * Kayıt Defteri
  * Performans sayaçları
  * SQL dinamik yönetim görünümleri (DMV).

*Veriler toplandığında yapılandırmanın bir yolu var mı?*

* Şu anda değil.

*Neden Bir Run As Hesabı'nı yapılandırmam gerekiyor?*

* SQL Server için az sayıda SQL sorgusu çalıştırılır. Bunların çalışması için, SQL'e VIEW SERVER STATE izinleri olan Bir Run Hesabı kullanılmalıdır.  Buna ek olarak, WMI sorgusu için yerel yönetici kimlik bilgileri gereklidir.

*Neden sadece en iyi 10 önerileri görüntüleniyor?*

* Size kapsamlı ezici bir görev listesi vermek yerine, önce öncelik verilen önerileri ele almaya odaklanmanızı öneririz. Bunları ele aldıktan sonra, ek öneriler kullanılabilir olacaktır. Ayrıntılı listeyi görmek isterseniz, Log Analytics günlük aramasını kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneriyi yok saymanın bir yolu var mı?*

* Evet, yukarıdaki [önerileri yoksay](#ignore-recommendations) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı SQL Sistem Durumu Denetimi verilerini ve önerilerini nasıl analiz edeceğiz öğrenmek için [sorguları günlüğe](../log-query/log-query-overview.md) kaydedin.
