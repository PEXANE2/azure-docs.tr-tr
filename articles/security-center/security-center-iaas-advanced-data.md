---
title: Azure Güvenlik Merkezi 'nde IaaS için gelişmiş veri güvenliği | Microsoft Docs
description: " Azure Güvenlik Merkezi 'nde IaaS için gelişmiş veri güvenliğini nasıl etkinleştirebileceğinizi öğrenin. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: memildin
ms.openlocfilehash: 287da68617a9527bc398df577cf8d10773fa8557
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202180"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-public-preview"></a>Azure sanal makineler 'de SQL sunucuları için gelişmiş veri güvenliği (Genel Önizleme)
Azure sanal makineler 'deki SQL sunucuları için gelişmiş veri güvenliği, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir pakettir. Şu anda (genel önizlemede), ortaya çıkabilecek ve olası veritabanı güvenlik açıklarını azaltmaya ve veritabanınıza yönelik bir tehdit oluşturabilecek anormal etkinlikleri algılamakla ilgili işlevsellik içerir. 

Azure VM 'Leri için bu güvenlik teklifi SQL Server 'lar, [Azure SQL veritabanı gelişmiş veri güvenlik paketinde](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)kullanılan temel teknolojiyi temel alır.


## <a name="overview"></a>Genel Bakış

Gelişmiş veri güvenliği, güvenlik açığı değerlendirmesinden ve Gelişmiş tehdit korumasından oluşan gelişmiş bir SQL güvenlik özellikleri kümesi sağlar.

* [Güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) , olası veritabanı güvenlik açıklarını düzeltebileceğiniz, izleyebileceğiniz ve bu sorunları gidermenize yardımcı olabilecek bir hizmeti kolayca yapılandırabilir. Güvenlik durumlarınızın görünürlüğünü sağlar ve güvenlik sorunlarını giderme ve veritabanınızın Fortifications geliştirilmesine yönelik adımları içerir.
* [Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) , SQL Server 'a erişmeye veya yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı sürekli şüpheli etkinlikler için izler ve anormal veritabanı erişim desenlerinde eyleme dayalı güvenlik uyarıları sağlar. Bu uyarılar, tehdidi araştırmak ve azaltmak için şüpheli etkinlik ayrıntılarını ve önerilen eylemleri sağlar.

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Azure VM 'lerinde SQL için gelişmiş veri güvenliği ile çalışmaya başlama

Aşağıdaki adımlar, Azure VM 'Leri genel önizleme üzerinde SQL için gelişmiş veri güvenliği ile çalışmaya başlamanızı ister.

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>Azure VM 'lerde SQL için gelişmiş veri güvenliğini ayarlama

**Başlamadan önce**: Çözümlenmekte olan güvenlik günlüklerini depolamak için bir Log Analytics çalışma alanı gerekir. Bir tane yoksa, [Azure portal Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)bölümünde açıklandığı gibi kolayca bir tane oluşturabilirsiniz.

1. SQL Server 'ı barındıran VM 'yi Log Analytics çalışma alanına bağlayın. Yönergeler için bkz. [Windows bilgisayarlarını Azure izleyici 'ye bağlama](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).

1. Azure Marketi 'nden [SQL gelişmiş veri güvenliği çözümüne](https://ms.portal.azure.com/#create/Microsoft.SQLAdvancedDataSecurity)gidin.
(Aşağıdaki görüntüde gösterildiği gibi Market arama seçeneğini kullanarak bulabilirsiniz.) **SQL gelişmiş veri güvenliği** sayfası açılır.

    ![IaaS için gelişmiş veri güvenliği](./media/security-center-advanced-iaas-data/sql-advanced-data-security.png)

1. **Oluştur**’a tıklayın. Çalışma yerleri görüntülenir.

    ![Gelişmiş veri güvenliği oluşturma](./media/security-center-advanced-iaas-data/sql-advanced-data-create.png)

1. Kullanılacak çalışma alanını seçin ve **Oluştur**' a tıklayın.

   ![Çalışma alanını seçme](./media/security-center-advanced-iaas-data/sql-workspace.png)

1. [VM 'nın SQL Server](https://docs.microsoft.com/sql/database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services?view=sql-server-2017)'ı yeniden başlatın.


## <a name="explore-and-investigate-security-alerts"></a>Güvenlik uyarılarını araştır ve araştır

Geçerli güvenlik uyarılarınızı görüntüleyebilir ve yönetebilirsiniz.

1. **Güvenlik Merkezi** > **güvenlik uyarıları**' na tıklayın ve bir uyarıya tıklayın.

    ![Uyarı bul](./media/security-center-advanced-iaas-data/find-alert.png)

1. **Saldırıya** uğrayan kaynak sütunundan saldırıya uğrayan bir kaynağa tıklayın.

1. Geçerli tehdidi araştırmaya ve gelecekteki tehditleri gidermeye yönelik uyarı ayrıntılarını ve eylemlerini görüntülemek için, **genel bilgiler** sayfasını kaydırın ve **Düzeltme ADıMLARı** bölümünde **araştırma adımları** bağlantısına tıklayın.

    ![Düzeltme Adımları](./media/security-center-advanced-iaas-data/remediation-steps.png)

1. Uyarının tetiklenmesi ile ilişkili günlükleri görüntülemek için **Log Analytics çalışma alanları** ' na gidin ve aşağıdaki adımları uygulayın:

     > [!NOTE]
     > **Log Analytics çalışma alanları** sol menüde görünmezse, **tüm hizmetler**' e tıklayın ve **Log Analytics çalışma alanları**' nı arayın.

    1. Sütunların **fiyatlandırma katmanını** ve çalışma alanı **kimliği** sütunlarını görüntülediğinden emin olun. (**Log Analytics çalışma alanları** > **Sütunları Düzenle**, **fiyatlandırma katmanı** ve çalışma alanı **kimliği**Ekle.)

     ![Sütunları Düzenle](./media/security-center-advanced-iaas-data/edit-columns.png)

    1. Uyarı günlüklerine sahip çalışma alanına tıklayın.

    1. **Genel** menü altında **Günlükler** ' e tıklayın.

    1. **SQLAdvancedThreatProtection** tablosunun yanındaki göz düğmesine tıklayın. Günlükler listelenir.

     ![Günlükleri Görüntüle](./media/security-center-advanced-iaas-data/view-logs.png)

## <a name="set-up-email-notification-for-atp-alerts"></a>ATP uyarıları için e-posta bildirimi ayarlama 

Bir alıcı listesini ASC uyarıları oluşturulduğunda bir e-posta bildirimi alacak şekilde ayarlayabilirsiniz. E-posta, Azure Güvenlik Merkezi 'ndeki tüm ilgili ayrıntıların bulunduğu uyarıya doğrudan bir bağlantı içerir. 

1. **Güvenlik Merkezi** > **fiyatlandırma & ayarlar** ' a gidin ve ilgili aboneliğe tıklayın

    ![Abonelik ayarları](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. **Ayarlar** menüsünde **e-posta bildirimleri**' ne tıklayın. 
1. **E-posta adresi** metin kutusuna bildirimleri alacak e-posta adreslerini girin. E-posta adreslerini virgülle ayırarak birden fazla e-posta adresi girebilirsiniz (,).  admin1@mycompany.comÖrneğinadmin2@mycompany.com,,admin3@mycompany.com

      ![E-posta Ayarları](./media/security-center-advanced-iaas-data/email-settings.png)

1. **E-posta bildirimi** ayarları ' nda, aşağıdaki seçenekleri ayarlayın:
  
    * **Yüksek öneme sahip uyarılar için e-posta bildirimi gönderin**: Tüm uyarılar için e-posta göndermek yerine yalnızca yüksek önem derecesi uyarıları gönderin.
    * **Ayrıca, abonelik sahiplerine e-posta bildirimleri gönderin**:  Abonelik sahiplerine de bildirimler gönderin.

1. **E-posta bildirimleri** ekranının üst kısmında **Kaydet**' e tıklayın.

  > [!NOTE]
  > Pencereyi kapatmadan önce **Kaydet** ' i tıklattığınızdan emin olun veya yeni **e-posta bildirimi** ayarları kaydedilmez.

## <a name="explore-vulnerability-assessment-reports"></a>Güvenlik açığı değerlendirme raporlarını keşfet

Güvenlik açığı değerlendirme panosu, tüm veritabanlarınızdaki değerlendirme sonuçlarınıza genel bir bakış sağlar. Veritabanlarının SQL Server sürüme göre dağıtımını, veritabanlarının başarılı olup olmadığını ve risk dağıtımına göre başarısız olan denetimlerin genel özetini de görüntüleyebilirsiniz.

Güvenlik açığı değerlendirme sonuçlarınızı ve raporlarınızı doğrudan Log Analytics görüntüleyebilirsiniz.

1. Gelişmiş veri güvenliği çözümü ile Log Analytics çalışma alanınıza gidin.
1. **Çözümler** ' e gidin ve **SQL güvenlik açığı değerlendirme** çözümünü seçin.
1. **Özet** bölmesinde, **Özeti görüntüle** ' ye tıklayın ve **SQL güvenlik açığı değerlendirme raporunuzu**seçin.

    ![SQL Değerlendirmesi raporu](./media/security-center-advanced-iaas-data/ads-sql-server-1.png)

    Rapor panosu yüklenir. Güvenlik açığı değerlendirme taramalarının, veritabanlarınızda 7 günde bir kez sabit bir zamanlamaya göre çalıştırıldığı zaman penceresinin en az **son 7 güne** ayarlandığından emin olun.

    ![Son 7 günü ayarla](./media/security-center-advanced-iaas-data/ads-sql-server-2.png)

1. Daha ayrıntılı bilgi edinmek için Pano öğelerinden herhangi birine tıklayın. Örneğin:

   1. Tüm veritabanları genelinde bu denetim sonuçlarının bulunduğu bir Log Analytics tablosunu görüntülemek için, **başarısız denetimler Özeti** bölümünde bir güvenlik açığına tıklayın. Sonuçlara sahip olanlar önce listelenir.

   1. Ardından, güvenlik açığı açıklaması ve etkisi, durumu, ilişkili risk ve bu veritabanındaki gerçek sonuçlar dahil olmak üzere her güvenlik açığıyla ilgili ayrıntıları görüntülemek için öğesine tıklayın. Ayrıca, bu denetimi gerçekleştirmek için çalıştırılan gerçek sorguyu ve bu güvenlik açığını çözümlemek için düzeltme bilgilerini görebilirsiniz.

    ![Çalışma alanını seçme](./media/security-center-advanced-iaas-data/ads-sql-server-3.png)

    ![Çalışma alanını seçme](./media/security-center-advanced-iaas-data/ads-sql-server-4.png)

1. Gereksinimlerinize göre verileri dilimleyip zaratmak için güvenlik açığı değerlendirme sonuçları verilerinizde Log Analytics sorguları çalıştırabilirsiniz.

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Azure VM 'lerinde SQL sunucuları için Gelişmiş tehdit koruması
Uyarılar, SQL Server 'Lar için olağandışı ve olası zararlı saldırılara göre oluşturulur. Bu olaylar aşağıdaki uyarıları tetikleyebilir:

### <a name="anomalous-access-pattern-alerts-supported-in-public-preview"></a>Anormal erişim deseninin uyarıları (genel önizlemede desteklenir)

* **Olağan dışı konumdan erişim:** Bu uyarı, SQL Server 'a erişim modelinde bir değişiklik olduğunda tetiklenir ve bu, birinin SQL Server 'da olağan dışı bir coğrafi konumdan oturum açtığı yerdir. Olası nedenler:
     * Bir saldırgan veya eski bir kötü amaçlı, SQL Server erişti.
     * Yasal bir Kullanıcı yeni bir konumdan SQL Server erişti.
* **Zararlı olabilecek bir uygulamadan erişim**: veritabanına erişmek için potansiyel olarak zararlı bir uygulama kullanıldığında bu uyarı tetiklenir. Olası nedenler:
     * Bir saldırgan ortak saldırı araçlarını kullanarak SQL 'nizi ihlal etmek üzere çalışıyor.
     * Eylemde geçerli bir sızma testi.
* **Bilmediğiniz sorumludan erişim**: Bu uyarı, SQL Server 'a erişim düzeninde bir değişiklik olduğunda tetiklenir ve bu, birinin SQL Server 'da olağan dışı bir sorumlu (SQL kullanıcısı) kullanarak oturum açmıştır. Olası nedenler:
     * Bir saldırgan veya eski bir kötü amaçlı, SQL Server erişti. 
     * Yasal bir Kullanıcı, SQL Server yeni bir sorumlu ile erişmiştir.
* **Deneme YANıLMA SQL kimlik bilgilerini zorla**: Bu uyarı, farklı kimlik bilgileri ile olağan dışı yüksek sayıda başarısız oturum açma işlemi olduğunda tetiklenir. Olası nedenler:
     * Bir saldırgan, deneme yanılma kullanarak SQL 'nizi ihlal etmeye çalışan bir saldırgan.
     * Eylemde geçerli bir sızma testi.

### <a name="potential-sql-injection-attacks-coming"></a>Olası SQL ekleme saldırıları (gelen)

* **SQL ekleme güvenlik açığı**: Bu uyarı, bir uygulama veritabanında hatalı bir SQL açıklaması oluşturduğunda tetiklenir. Bu uyarı, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Olası nedenler:
     * Hatalı SQL deyimini oluşturan uygulama kodunda hata
     * Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
* **OLASı SQL ekleme**: Bu uyarı, SQL ekleme için tanımlanan uygulama güvenlik açığına karşı etkin bir yararlanma gerçekleştiğinde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
