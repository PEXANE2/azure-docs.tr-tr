---
title: Azure Monitor ile Sistem Merkezi Operasyon Yöneticisi'ni Değerlendirin
description: Düzenli aralıklarla ortamlarınızın risk ve sağlığını değerlendirmek için Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü çözümünü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2018
ms.openlocfilehash: 94251dfa2d9fa732912ed20d825e64f542d79188
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055421"
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-health-check-preview-solution"></a>System Center Operations Manager Sistem Durumu Denetimi (Önizleme) çözümü ile ortamınızı iyileştirme

![Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü sembolü](./media/scom-assessment/scom-assessment-symbol.png)

Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü çözümünü düzenli aralıklarla Sistem Merkezi Operasyon Yöneticisi yönetim grubunuzun risk ve sağlığını değerlendirmek için kullanabilirsiniz. Bu makale, olası sorunlar için düzeltici eylemlerde bulunmak için çözümü yüklemenize, yapılandırmanıza ve kullanmanıza yardımcı olur.

Bu çözüm, dağıtılmış sunucu altyapınıza özgü öncelikle öneriler listesi sağlar. Öneriler, riski hızlı bir şekilde anlamanıza ve düzeltici eylemde bulunmanıza yardımcı olan dört odak alanına göre sınıflandırılır.

Yapılan öneriler, Microsoft mühendislerinin binlerce müşteri ziyaretinden edindikleri bilgi ve deneyime dayanmaktadır. Her öneri, bir sorunun sizin için neden önemli olabileceği ve önerilen değişikliklerin nasıl uygulanacağı hakkında rehberlik sağlar.

Kuruluşunuz için en önemli olan odak alanlarını seçebilir ve risksiz ve sağlıklı bir ortam çalıştırma yolundaki ilerlemenizi izleyebilirsiniz.

Çözümü ekledikten ve bir değerlendirme yapıldıktan sonra, odak alanlarına ait özet bilgiler altyapınız için **Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü** panosunda gösterilir. Aşağıdaki bölümlerde, Operasyon Yöneticisi ortamınızı görüntüleyip ardından önerilen eylemleri gerçekleştirebileceğiniz **Sistem Merkezi Operasyon Yöneticisi Sağlık Denetimi** panosundaki bilgilerin nasıl kullanılacağı açıklanmıştır.

![Sistem Merkezi Operasyon Yöneticisi çözüm döşemesi](./media/scom-assessment/log-analytics-scom-healthcheck-tile.png)

![Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü panosu](./media/scom-assessment/log-analytics-scom-healthcheck-dashboard-01.png)

## <a name="installing-and-configuring-the-solution"></a>Çözümü yükleme ve yapılandırma

Çözüm, Microsoft System Center 2012 Operations Manager Service Pack 1, Microsoft System Center 2012 R2 Operations Manager, Microsoft System Center 2016 Operations Manager, Microsoft System Center 2016 Operations Manager ve Microsoft System ile çalışır Merkez Operasyon Müdürü 1807. Her yönetim sunucusuna .NET Framework 4.6.2'nin desteklenen bir sürümü yüklenmelidir.

Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

- Günlük Analizi'nde Sistem Durumu Denetimi çözümlerini kullanamadan önce çözümü yüklemiş olmalısınız. Çözümü Azure [marketinden](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview)yükleyin.

- Çözüm çalışma alanına ekledikten sonra, panodaki **Sistem Merkezi Operasyon Yöneticisi Sistem Denetimi** Denetimi döşemesi ek bir yapılandırma gerekli ileti görüntüler. Döşemeye tıklayın ve sayfada belirtilen yapılandırma adımlarını izleyin

  ![Sistem Merkezi Operasyon Yöneticisi pano döşemesi](./media/scom-assessment/scom-configrequired-tile.png)

> [!NOTE]
> Sistem Merkezi Operasyon Yöneticisi yapılandırması, Log Analytics'te çözümün yapılandırma sayfasında belirtilen adımları izleyerek bir komut dosyası kullanılarak yapılabilir.

 Değerlendirmeyi Operations Manager Operations konsolu aracılığıyla yapılandırmak için aşağıdaki sırayla aşağıdaki adımları gerçekleştirin:
1. [Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü için Run'ı Hesap Olarak Ayarlama](#operations-manager-run-as-accounts-for-log-analytics)  
2. Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü kuralını yapılandırın

## <a name="system-center-operations-manager-health-check-data-collection-details"></a>System Center Operations Manager Sistem Durumu Denetimi veri toplama ayrıntıları

Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü çözümü aşağıdaki kaynaklardan veri toplar:

* Kayıt Defteri
* Windows Yönetim Araçları (WMI)
* Olay günlüğü
* Dosya verileri
* PowerShell ve SQL sorgularını kullanarak doğrudan Operations Manager'dan, belirttiğiniz bir yönetim sunucusundan.  

Veriler yönetim sunucusunda toplanır ve her yedi günde bir Log Analytics'e iletilir.  

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Log Analytics için Operations Manager farklı çalıştır hesapları

Log Analytics, katma değerli hizmetler sağlamak için iş yükleri için yönetim paketleri üzerine inşa eder. Her iş yükü, yönetim paketlerini etki alanı kullanıcı hesabı gibi farklı bir güvenlik bağlamında çalıştırmak için iş yüküne özgü ayrıcalıklar gerektirir. Bir Operasyon Yöneticisi Run As hesabını ayrıcalıklı kimlik bilgileriyle yapılandırın. Daha fazla bilgi için, Operasyon Yöneticisi belgelerinde [Run As hesabının nasıl oluşturulabildiğini](https://technet.microsoft.com/library/hh321655(v=sc.12).aspx) görün.

Sistem Merkezi Operasyon Yöneticisi Sağlık Denetimi için Operasyon Yöneticisi Çalıştır'ı hesap olarak ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account"></a>Farklı Çalıştır hesabını ayarlama

Run As hesabı devam etmeden önce aşağıdaki gereksinimleri karşılamalıdır:

* Herhangi bir Operations Manager rolünü destekleyen tüm sunucularda yerel Yöneticiler grubunun üyesi olan bir etki alanı kullanıcı hesabı - Yönetim sunucusu, SQL Server operasyonel barındırma, veri ambarı ve ACS veritabanı, Raporlama, Web konsolu ve Ağ Geçidi sunucusu.
* Değerlendirilmekte olan yönetim grubu için Operasyon Yöneticisi Yönetici Rolü
* Hesabın SQL sysadmin hakları yoksa, işlem yöneticisi veritabanlarından birini veya tamamını barındıran her SQL Server örneğinde hesaba parçalı izinler vermek için [komut dosyasını](#sql-script-to-grant-granular-permissions-to-the-run-as-account) çalıştırın.

1. Operations Manager Console'da **Yönetim** gezinti düğmesini seçin.
2. **Yapılandırma Olarak Çalıştır**altında, **Hesaplar'ı**tıklatın.
3. Hesap **Olarak Çalıştır** Sihirbazı'nda, **Giriş** sayfasında **İleri'yi**tıklatın.
4. Genel **Özellikler** sayfasında, **Hesap Olarak Çalıştır türünde Windows'u seçin:** liste. **Windows**
5. **Görüntü Adı** metin kutusuna bir ekran adı yazın ve isteğe bağlı olarak **Açıklama** kutusuna bir açıklama yazın ve ardından **İleri'yi**tıklatın.
6. Dağıtım **Güvenliği** sayfasında **Daha güvenli'yi**seçin.
7. **Oluştur'u**tıklatın.  

Artık Çalıştır Hesabı oluşturulduğuna göre, yönetim grubundaki yönetim sunucularını hedeflemesi ve önceden tanımlanmış bir Run As profiliyle ilişkilendirilmesi gerekir, böylece iş akışları kimlik bilgilerini kullanarak çalışır.  

1. **Çalıştır Yapılandırması**altında, **Hesaplar**, sonuç bölmesinde, daha önce oluşturduğunuz hesabı çift tıklatın.
2. **Dağıtım** sekmesinde, **Seçili bilgisayarlar** için **Ekle** kutusunu tıklatın ve hesabı dağıtmak için yönetim sunucusunu ekleyin.  Değişikliklerinizi kaydetmek için Iki kez **Tamam'ı** tıklatın.
3. **Yapılandırma Olarak Çalıştır** **altında, Profiller'i**tıklatın.
4. *SCOM Değerlendirme Profilini*Ara .
5. Profil adı olmalıdır: *Microsoft System Center Operations Manager Health Check Run As Profile*.
6. Sağ tıklayın ve özelliklerini güncelleştirin ve daha önce oluşturduğunuz en son oluşturulan Run As Hesabı ekleyin.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>Farklı Çalıştır hesabına ayrıntılı izinler vermek için SQL betiği

Operasyonel, veri ambarı ve ACS veritabanını barındıran Operations Manager tarafından kullanılan SQL Server örneğinde Run As hesabına gerekli izinleri vermek için aşağıdaki SQL komut dosyasını çalıştırın.

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```

### <a name="configure-the-health-check-rule"></a>Sistem durumu denetimi kuralını yapılandırma

System Center Operations Manager Health Check solution'ın yönetim paketi, *Microsoft System Center Operations Manager Run Health Check Rule*adlı bir kural içerir. Bu kural, sistem durumu denetimini çalıştırmakla yükümlüdür. Kuralı etkinleştirmek ve sıklığı yapılandırmak için aşağıdaki yordamları kullanın.

Varsayılan olarak, Microsoft System Center Operations Manager Run Health Check Rule devre dışı bırakılır. Sistem durumu denetimini çalıştırmak için, kuralı bir yönetim sunucusunda etkinleştirmeniz gerekir. Aşağıdaki adımları kullanın:

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Belirli bir yönetim sunucusu için kuralı etkinleştirme

1. Operations Manager Operations konsolunun **Yazma** çalışma alanında, **Kurallar** bölmesinde Microsoft System Center Operations Manager Run Health *Check Rule* kuralını arayın.
2. Arama sonuçlarında, *Tür: Yönetim Sunucusu'* nu içeren metni seçin.
3. Kuralı sağ tıklatın ve sonra sınıfın belirli bir **nesnesi** > için Geçersiz Kılmaları'nı**tıklatın: Yönetim Sunucusu**.
4.  Kullanılabilir yönetim sunucuları listesinde, kuralın çalışması gereken yönetim sunucusunu seçin.  Bu, Run As hesabını ilişkilendirmek için daha önce yapılandırdığınız yönetim sunucusu olmalıdır.
5.  **Etkin** parametre değeri için geçersiz kılma değerini **True** olarak değiştirdiğinden emin olun.<br><br> ![parametreyi geçersiz kılma](./media/scom-assessment/rule.png)

    Bu pencerede yken, sonraki yordamı kullanarak çalıştırma sıklığını yapılandırın.

#### <a name="configure-the-run-frequency"></a>Çalıştırma sıklığını yapılandırma

Değerlendirme, varsayılan olarak her 10.080 dakika (veya yedi gün) çalışacak şekilde yapılandırılır. Değeri en az 1440 dakika (veya bir gün) değeri geçersiz kılabilirsiniz. Değer, ardışık değerlendirme çalıştırmaları arasında gereken minimum zaman aralığını temsil eder. Aralığı geçersiz kılmak için aşağıdaki adımları kullanın.

1. Operations Manager konsolunun **Yazma** çalışma alanında, **Kurallar** bölümünde Microsoft System Center Operations Manager Run Health *Check Rule* kuralını arayın.
2. Arama sonuçlarında, *Tür: Yönetim Sunucusu'* nu içeren metni seçin.
3. Kuralı sağ tıklatın ve sonra sınıfın tüm nesneleri için **Kuralı** > Geçersiz**Kılma'yı tıklatın: Yönetim Sunucusu**.
4. **Aralık** parametre değerini istediğiniz aralık değeriyle değiştirin. Aşağıdaki örnekte, değer 1440 dakika (bir gün) olarak ayarlanır.<br><br> ![aralık parametresi](./media/scom-assessment/interval.png)<br>  

    Değer 1440 dakikadan az olarak ayarlanırsa, kural bir günlük aralıkla çalışır. Bu örnekte, kural aralık değerini yoksa ve bir gün frekansta çalışır.


## <a name="understanding-how-recommendations-are-prioritized"></a>Önerilerin önceliklerini belirlemeyi anlama

Yapılan her öneriye, önerinin göreli önemini tanımlayan bir ağırlık değeri verilir. Yalnızca en önemli 10 öneri gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıklar nasıl hesaplanır?

Ağırlıklandırmalar üç temel faktöre dayalı toplam değerlerdir:

- Tanımlanan bir sorunun sorunlara neden olma *olasılığı.* Daha yüksek bir olasılık, öneri için daha büyük bir genel puana eşittir.
- Sorunun kuruluşunuz üzerindeki *etkisi,* bir soruna neden oluyorsa. Daha yüksek bir etki, tavsiye için daha büyük bir genel puana eşittir.
- Tavsiyeyi uygulamak için gereken *çaba.* Daha yüksek bir çaba, tavsiye için daha küçük bir genel puana eşittir.

Her öneriiçin ağırlıklandırma, her odak alanı için kullanılabilir toplam puanın yüzdesi olarak ifade edilir. Örneğin, Kullanılabilirlik ve İş Sürekliliği odak alanındaki bir öneri %5'e sahipse, bu öneriyi uygulamak genel Kullanılabilirlik ve İş Sürekliliği puanınızı %5 artırır.

### <a name="focus-areas"></a>Odak alanları

**Kullanılabilirlik ve İş Sürekliliği** - Bu odak alanı, hizmet kullanılabilirliği, altyapınızın esnekliği ve iş koruması için öneriler gösterir.

**Performans ve Ölçeklenebilirlik** - Bu odak alanı, kuruluşunuzun BT altyapısının büyümesine yardımcı olacak, BT ortamınızın geçerli performans gereksinimlerini karşıladığından ve değişen altyapı gereksinimlerine yanıt verebilmesini sağlayacak öneriler gösterir.

**Yükseltme, Geçiş ve Dağıtım** - Bu odak alanı, SQL Server'ı varolan altyapınıza yükseltmenize, geçiş inizi ve dağıtmanıza yardımcı olacak öneriler gösterir.

**İşlemler ve İzleme** - Bu odak alanı, BT operasyonlarınızı kolaylaştırmaya, önleyici bakımı uygulamaya ve performansı en üst düzeye çıkarmaya yardımcı olacak öneriler gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Her odak alanında %100 puanı mı hedeflemeniz gerekiyor?

Mutlaka. Öneriler, Microsoft mühendislerinin binlerce müşteri ziyaretinde edindikleri bilgi ve deneyimlere dayanmaktadır. Ancak, iki sunucu altyapısı aynı değildir ve belirli öneriler sizin için az çok alakalı olabilir. Örneğin, sanal makineleriniz Internet'e açık değilse, bazı güvenlik önerileri daha az alakalı olabilir. Bazı kullanılabilirlik önerileri, düşük öncelikli geçici veri toplama ve raporlama sağlayan hizmetler için daha az alakalı olabilir. Olgun bir işletme için önemli olan sorunlar, yeni kurulan bir işletme için daha az önemli olabilir. Önceliklerinizin hangi odak alanları olduğunu belirlemek ve puanlarınızın zaman içinde nasıl değiştiğine bakmak isteyebilirsiniz.

Her öneri, neden önemli olduğu hakkında rehberlik içerir. BT hizmetlerinizin doğası ve kuruluşunuzun iş gereksinimleri göz önüne alındığında, önerinin uygulanmasının sizin için uygun olup olmadığını değerlendirmek için bu kılavuzu kullanın.

## <a name="use-health-check-focus-area-recommendations"></a>Sistem durumu denetimi odak alanı önerilerini kullanma

Log Analytics'te sistem durumu denetimi çözümlerini kullanamadan önce çözümü yüklü yormuş olmalısınız. Çözümleri yükleme hakkında daha fazla bilgi için [bkz.](../../azure-monitor/insights/solutions.md) Yüklendikten sonra, Azure portalındaki çalışma alanınız için **Genel Bakış** sayfasındaKi Sistem Merkezi İşlem yöneticisi Sistem Yöneticisi Sistem Durumu Denetimi döşemesini kullanarak önerilerin özetini görüntüleyebilirsiniz.

Altyapınız için özet uyumluluk değerlendirmelerini görüntüleyin ve ardından ayrıntılı önerilere sahip.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için önerileri görüntülemek ve düzeltici eylemde bulunmak için
1. Azure portalına giriş [https://portal.azure.com](https://portal.azure.com)yapın.
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Günlük Analizi abonelikleri bölmesinde, bir çalışma alanı seçin ve ardından **Çalışma Alanı özeti** menü öğesini tıklatın.  
4. Genel **Bakış** sayfasında, **Sistem Merkezi Operasyon Yöneticisi Sağlık Denetimi** döşemesini tıklatın.
5. Sistem **Merkezi Operasyon Yöneticisi Sağlık Denetimi** sayfasında, odak alanı bıçaklarından birinde özet bilgileri gözden geçirin ve ardından bu odak alanı için önerileri görüntülemek için bir tanesini tıklatın.
6. Odak alanı sayfalarından herhangi birinde, ortamınız için yapılan öncelikli önerileri görüntüleyebilirsiniz. Önerinin neden yapıldığıyla ilgili ayrıntıları görüntülemek için **Etkilenen Nesneler** altında bir öneriyi tıklatın.<br><br> ![odak alanı](./media/scom-assessment/log-analytics-scom-healthcheck-dashboard-02.png)<br>
7. **Önerilen Eylemler'de**önerilen düzeltici eylemleri alabilirsiniz. Öğe ele alındığında, daha sonraki değerlendirmeler önerilen eylemlerin alındığını ve uyumluluk puanınızın artacağını kaydeder. Düzeltilen öğeler **Geçirilen Nesneler**olarak görünür.

## <a name="ignore-recommendations"></a>Önerileri yoksayma

Göz ardı etmek istediğiniz önerileriniz varsa, Log Analytics'in değerlendirme sonuçlarınızda önerilerin görünmesini önlemek için kullandığı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Yoksaymak istediğiniz önerileri tanımlamak için
1. Seçtiğiniz çalışma alanınız için Günlük Analizi çalışma alanı sayfasındaki Azure portalında, **Günlük Arama** menüsü öğesini tıklatın.
2. Ortamınızdaki bilgisayarlarda başarısız olan önerileri listelemek için aşağıdaki sorguyu kullanın.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Çalışma alanınız yeni Log [Analytics sorgu diline](../../azure-monitor/log-query/log-query-overview.md)yükseltildiyse, yukarıdaki sorgu aşağıdakigibi değiştirilir.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Burada Günlük Arama sorgusunu gösteren bir ekran görüntüsü verem:<br><br> ![günlük araması](./media/scom-assessment/scom-log-search.png)<br>

3. Yoksaymak istediğiniz önerileri seçin. Sonraki yordamda RecommendationId için değerleri kullanırsınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>IgnoreRecommendations.txt metin dosyası oluşturmak ve kullanmak için

1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Log Analytics'in ayrı bir satırda yoksaymasını istediğiniz her öneri için her Öneri'yi yapıştırın veya yazın ve ardından dosyayı kaydedip kapatın.
3. Günlüğü Analytics'in önerileri yok saymasını istediğiniz her bilgisayarda dosyayı aşağıdaki klasöre koyun.
4. Operations Manager yönetim sunucusunda - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Önerilerin yoksayDığını doğrulamak için

1. Bir sonraki zamanlanmış değerlendirme çalıştırıldıktan sonra, varsayılan olarak her yedi günde bir, belirtilen öneriler Göz ardı edilir ve sistem durumu denetimi panosunda görünmez.
2. Göz ardı edilen tüm önerileri listelemek için aşağıdaki Günlük Arama sorgularını kullanabilirsiniz.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    >[!NOTE]
    > Çalışma alanınız yeni Log [Analytics sorgu diline](../../azure-monitor/log-query/log-query-overview.md)yükseltildiyse, yukarıdaki sorgu aşağıdakigibi değiştirilir.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Ignore" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Daha sonra yoksayan önerileri görmek istediğinize karar verirseniz, YokSayöneriler.txt dosyalarını kaldırın veya RecommendationsID'leri kaldırabilirsiniz.

## <a name="system-center-operations-manager-health-check-solution-faq"></a>System Center Operations Manager Sistem Durumu Denetimi çözümü SSS

*Günlük Analizi çalışma alanıma Sistem Durumu Denetimi çözümlerini ekledim. Ama önerileri göremiyorum. Neden olmasın?* Çözümü ekledikten sonra, Log Analytics panosundaki önerileri görüntüleyin aşağıdaki adımları kullanın.  

- [Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü için Run'ı Hesap Olarak Ayarlama](#operations-manager-run-as-accounts-for-log-analytics)  
- [Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü kuralını yapılandırın](#configure-the-health-check-rule)


*Denetimin ne sıklıkta çalıştığını yapılandırmanın bir yolu var mı?* Evet. Bkz. [Çalışma sıklığını yapılandırın.](#configure-the-run-frequency)

*System Center Operations Manager Health Check çözümlerini ekledikten sonra başka bir sunucu keşfedilirse, bu durum kontrol edilir mi?* Evet, keşiften sonra, varsayılan olarak her yedi günde bir, o andan itibaren kontrol edilir.

*Veri toplama işleminin adı nedir?* AdvisorAssessment.exe

*AdvisorAssessment.exe süreci nerede çalışır?* AdvisorAssessment.exe, sistem durumu denetimi kuralının etkin olduğu yönetim sunucusunun HealthService işlemi altında çalışır. Bu işlem kullanılarak, tüm ortamınızı bulma uzak veri toplama yoluyla elde edilir.

*Veri toplama ne kadar sürer?* Sunucuda veri toplama yaklaşık bir saat sürer. Birçok Operasyon Yöneticisi örneğinin veya veritabanlarının olduğu ortamlarda daha uzun sürebilir.

*Değerlendirme aralığını 1440 dakikadan daha az bir süreye ayarlarsam ne olur?* Değerlendirme günde en fazla bir kez çalışacak şekilde önceden yapılandırılmıştır. Aralık değerini 1440 dakikadan daha küçük bir değere geçersiz kılarsanız, değerlendirme aralık değeri olarak 1440 dakika kullanır.

*Ön koşul hataları olup olmadığını nasıl anlarsınız?* Sağlık denetimi çalıştırDıysa ve sonuçları göremiyorsanız, büyük olasılıkla çek için ön koşullar başarısız olur. Sorguları yürütebilirsiniz: `Operation Solution=SCOMAssessment` `SCOMAssessmentRecommendation FocusArea=Prerequisites` ve başarısız ön koşulları görmek için Günlük Arama'da.

*Ön koşul `Failed to connect to the SQL Instance (….).` hatalarında bir ileti vardır. Sorun nedir?* Data toplayan işlem olan AdvisorAssessment.exe, yönetim sunucusundaki HealthService işlemi altında çalışır. Sistem durumu denetiminin bir parçası olarak, işlem, Operations Manager veritabanının bulunduğu SQL Server'a bağlanmaya çalışır. Bu hata, güvenlik duvarı kuralları SQL Server örneğine bağlantıyı engellediğinde oluşabilir.

*Ne tür veriler toplanır?* Aşağıdaki veri türleri toplanır: - WMI verileri - Kayıt defteri verileri - EventLog verileri - Windows PowerShell, SQL Sorguları ve Dosya bilgi toplayıcısı aracılığıyla İşlem Yöneticisi verileri.

*Neden Bir Run As Hesabı'nı yapılandırmam gerekiyor?* Operations Manager ile çeşitli SQL sorguları çalıştırılır. Bunların çalışması için gerekli izinlere sahip bir Run As Hesabı kullanmanız gerekir. Buna ek olarak, yerel yönetici kimlik bilgileri WMI sorgulamak için gereklidir.

*Neden sadece en iyi 10 önerileri görüntüleniyor?* Size kapsamlı ve ezici bir görev listesi vermek yerine, önce öncelik verilen önerileri ele almaya odaklanmanızı öneririz. Bunları ele aldıktan sonra, ek öneriler kullanılabilir olacaktır. Ayrıntılı listeyi görmek isterseniz, Günlük Arama'yı kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneriyi yok saymanın bir yolu var mı?* Evet, [Yoksay önerilerine](#ignore-recommendations)bakın.


## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı Sistem Merkezi Operasyon Yöneticisi Sağlık Kontrolü verileri ve önerilerinin nasıl analiz edilebildiğini öğrenmek için [günlükleri arayın.](../../azure-monitor/log-query/log-query-overview.md)
