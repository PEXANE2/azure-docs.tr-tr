---
title: Azure Güvenlik Merkezi'nde IaaS için Gelişmiş Veri Güvenliği | Microsoft Dokümanlar
description: " Azure Güvenlik Merkezi'nde IaaS için gelişmiş veri güvenliğini nasıl etkinleştirmenizi öğrenin. "
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
ms.date: 11/11/2019
ms.author: memildin
ms.openlocfilehash: a2970ea3f5ad360deaedd7efc82154cd3bc50337
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282737"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Azure Sanal Makinelerdeki SQL sunucuları için gelişmiş veri güvenliği (Önizleme)
Azure Sanal Makinelerdeki SQL Sunucuları için gelişmiş veri güvenliği, gelişmiş SQL güvenlik özellikleri için birleşik bir pakettir. Bu önizleme özelliği, olası veritabanı güvenlik açıklarını tanımlamak ve hafifletmek ve veritabanınıza yönelik tehditleri gösterebilecek anormal etkinlikleri algılamak için işlevsellik içerir. 

Azure VMs SQL sunucuları için bu güvenlik teklifi, [Azure SQL Veritabanı Gelişmiş Veri Güvenliği paketinde](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)kullanılan temel teknolojiyi temel almaktadır.


## <a name="overview"></a>Genel Bakış

Gelişmiş veri güvenliği, Güvenlik Açığı değerlendirmesi ve Gelişmiş Tehdit Koruması'ndan oluşan bir dizi gelişmiş SQL güvenlik özelliği sağlar.

* [Güvenlik açığı değerlendirmesi,](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) olası veritabanı güvenlik açıklarını keşfedebilen, izleyebilen ve düzeltmenize yardımcı olabilecek kolay bir hizmeti yapılandırmaktır. Güvenlik durumunuzda görünürlük sağlar ve güvenlik sorunlarını gidermek ve veritabanı tahkimatLarınızı geliştirmek için adımları içerir.
* [Gelişmiş Tehdit Koruması,](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) SQL sunucunuza erişmek veya bundan yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı şüpheli etkinlikler için sürekli olarak izler ve anormal veritabanı erişim desenleri üzerinde eylem odaklı güvenlik uyarıları sağlar. Bu uyarılar, şüpheli etkinlik ayrıntılarını ve tehdidi araştırmak ve azaltmak için önerilen eylemleri sağlar.

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Azure VM'lerde SQL için Gelişmiş veri güvenliği ile başlayın

Aşağıdaki adımlar, Azure VM Genel Önizleme'de SQL için Gelişmiş Veri Güvenliği'ni başlatmanızı alır.

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>Azure VM'lerde SQL için gelişmiş veri güvenliği ayarlama

Sanal Makinelerdeki SQL Sunucuları için gelişmiş veri güvenliğini abonelik/çalışma alanı düzeyinde etkinleştirin:
 
1. Güvenlik Merkezi'nin kenar çubuğundan **Fiyatlandırma & ayarları** sayfasını açın.

1. Azure VM'lerde SQL için Gelişmiş Veri Güvenliği'ni etkinleştirmek istediğiniz abonelik veya çalışma alanını seçin.

1. **VM (Preview) üzerindeki SQL sunucuları** için etkinleştirme seçeneğini geçiş. 

    (Genişletmek için ekran görüntüsünü tıklatın)

    [![Windows Yönetici Merkezi'nde görüldüğü gibi Güvenlik Merkezi önerileri ve uyarıları](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    Seçilen çalışma alanına veya seçili aboneliğin varsayılan çalışma alanına bağlı tüm SQL Sunucularında SQL Sunucuları için Gelişmiş Veri Güvenliği etkinleştirilir.

    >[!NOTE]
    > Çözüm, SQL Server'ın ilk yeniden başlatılmasından sonra tamamen etkin olacaktır. 

Yeni bir çalışma alanı oluşturmak için, [Log Analytics çalışma alanında](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)yönergeleri izleyin.

SQL Server'ın ana bilgisayarını bir çalışma alanına bağlamak için Windows bilgisayarlarındaki yönergeleri [Azure Monitor'a bağlayın.](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)


## <a name="set-up-email-notification-for-security-alerts"></a>Güvenlik uyarıları için e-posta bildirimi ayarlama 

Güvenlik Merkezi uyarıları oluşturulduğunda bir e-posta bildirimi almak için alıcıların listesini ayarlayabilirsiniz. E-posta, Azure Güvenlik Merkezi'ndeki uyarıya ilgili tüm ayrıntıları içeren doğrudan bir bağlantı içerir. 

1. Güvenlik **Merkezi** > **Fiyatlandırma & ayarlarına** gidin ve ilgili aboneliğe tıklayın

    ![Abonelik Ayarları](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. Ayarlar menüsünden **E-posta bildirimlerini**tıklatın. 
1. **E-posta adresi** metin kutusuna, bildirimleri almak için e-posta adreslerini girin. E-posta adreslerini virgülle (,) ayırarak birden fazla e-posta adresi girebilirsiniz.  Örneğin admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com

    ![E-posta Ayarları](./media/security-center-advanced-iaas-data/email-settings.png)

1. **E-posta bildirimi** ayarlarında aşağıdaki seçenekleri ayarlayın:
  
    * **Yüksek önem uyarıları için e-posta bildirimi gönderin**: Tüm uyarılar için e-posta göndermek yerine, yalnızca yüksek önem uyarılarını gönderin.
    * **Ayrıca abonelik sahiplerine e-posta bildirimleri gönderin**: Abonelik sahiplerine de bildirim gönderin.

1. **E-posta bildirimleri** ekranının üst kısmında **Kaydet'i**tıklatın.

  > [!NOTE]
  > Pencereyi kapatmadan önce **Kaydet'i** tıklattığınızdan emin olun, yoksa yeni **E-posta bildirimi** ayarları kaydedilmez.

## <a name="explore-vulnerability-assessment-reports"></a>Güvenlik açığı değerlendirme raporlarını keşfedin

Güvenlik Açığı değerlendirme panosu, değerlendirme sonuçlarınızın tüm veritabanlarınızda genel bir görünümünü sağlar. Veritabanlarının SQL Server sürümüne göre dağılımını, veritabanlarının geçirilmesine karşı başarısızlığın bir özetini ve risk dağılımına göre başarısız denetimlerin genel bir özetini görüntüleyebilirsiniz.

Güvenlik açığı değerlendirme sonuçlarını doğrudan Güvenlik Merkezi'nden görüntüleyebilirsiniz.

1. KAYNAK GÜVENLİğİ HIJYENi altındaki Güvenlik Merkezi'nin kenar çubuğundan **Veri & Depolama'yı**seçin.

1. **VM'lerde SQL veritabanlarınızdaki öneri güvenlik açıklarını seçin (Önizleme) düzeltilmelidir.** Daha fazla bilgi için [Güvenlik Merkezi Önerileri'ne](security-center-recommendations.md)bakın. 

    [![**VM'lerde SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir (Önizleme)** önerisi](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png)](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png#lightbox)

    Bu öneri için ayrıntılı görünüm görüntülenir.

    [![VM'lerde SQL veritabanlarınızdaki **Güvenlik Açıkları için ayrıntılı görünüm düzeltilmelidir (Önizleme)** önerisi](media/security-center-advanced-iaas-data/all-servers-view.png)](media/security-center-advanced-iaas-data/all-servers-view.png#lightbox)

1. Daha fazla ayrıntı için detaya inmek için:

    * Taranmış kaynaklara (veritabanlarına) ve sınanan güvenlik denetimleri listesine genel bakış için, ilgi çekici sunucuyu tıklatın.
    [![SQL sunucusuna göre gruplanan güvenlik açıkları](media/security-center-advanced-iaas-data/single-server-view.png)](media/security-center-advanced-iaas-data/single-server-view.png#lightbox)

    * Belirli bir SQL veritabanı tarafından gruplanan güvenlik açıklarına genel bir bakış için ilgi alanı veritabanını tıklatın.
    [![SQL sunucusuna göre gruplanan güvenlik açıkları](media/security-center-advanced-iaas-data/single-database-view.png)](media/security-center-advanced-iaas-data/single-database-view.png#lightbox)

    Her görünümde, güvenlik denetimleri **Önem Derecesine**göre sıralanır. **Açıklama**içeren bir ayrıntı bölmesini , nasıl **düzeltilir** ve **Etki** veya **Kıyaslama**gibi diğer ilgili bilgileri görmek için belirli bir güvenlik denetimini tıklatın.

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Azure VM uyarılarındaki SQL sunucuları için gelişmiş tehdit koruması
Uyarılar, SQL Sunucularına erişmek veya bundan yararlanmak için alışılmadık ve zararlı olabilecek girişimlerle oluşturulur. Bu olaylar aşağıdaki uyarıları tetikleyebilir:

### <a name="anomalous-access-pattern-alerts-preview"></a>Anormal erişim deseni uyarıları (Önizleme)

* **Olağandışı bir konumdan erişim:** Bu uyarı, birinin alışılmadık bir coğrafi konumdan SQL sunucusuna oturum açtığı SQL sunucusuna erişim deseninde bir değişiklik olduğunda tetiklenir. Olası nedenler:
    * Bir saldırgan veya eski kötü niyetli çalışan, SQL Server'ınıza erişmiştir.
    * Yasal bir kullanıcı SQL Server'ınıza yeni bir konumdan erişmiştir.
* **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Olası nedenler:
    * Ortak saldırı araçlarını kullanarak SQL'inizi ihlal etmeye çalışan bir saldırgan.
    * Yasal bir nüfuz testi.
* **Sıra dışı bir sorumludan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir sorumludan (SQL kullanıcısı) eriştiğinde tetiklenir. Olası nedenler:
    * Bir saldırgan veya eski kötü niyetli çalışan, SQL Server'ınıza erişmiştir. 
    * Yasal bir kullanıcı SQL Server'ınıza yeni bir müdürle erişmiştir.
* **SQL kimlik bilgilerine deneme yanılma saldırısı**: Bu uyarı, farklı kimlik bilgileri kullanılarak sıra dışı sayıda başarısız oturum açma denemesi olduğunda tetiklenir. Olası nedenler:
    * Kaba kuvvet kullanarak SQL'inizi ihlal etmeye çalışan bir saldırgan.
    * Yasal bir nüfuz testi.

### <a name="potential-sql-injection-attacks-supported-in-sql-server-2019"></a>Olası SQL enjeksiyon saldırıları (SQL Server 2019'da desteklenir)

* **SQL enjeksiyonuna karşı güvenlik açığı**: Bir uygulama veritabanında hatalı bir SQL deyimi oluşturduğunda bu uyarı tetiklenir. Bu uyarı, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Olası nedenler:
    * Hatalı SQL deyimini oluşturan uygulama kodunda hata
    * Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
* **Olası SQL ekleme**: Bu uyarı, SQL ekleme işlemine tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma görüldüğünde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.


### <a name="unsafe-command-supported-in-sql-server-2019"></a>Güvenli olmayan komut (SQL Server 2019'da desteklendi)

* **Potansiyel Olarak Güvenli Olmayan Eylem**: Bu uyarı, yüksek ayrıcalıklı ve potansiyel olarak güvenli olmayan bir komut yürütüldüğünde tetiklenir. Olası nedenler:
    * Daha iyi güvenlik duruşu için devre dışı bırakılmaları önerilen komut etkinleştirilir.
    * SQL erişimine yararlanmaya veya ayrıcalıkları yükseltmeye çalışan bir saldırgan.   


## <a name="explore-and-investigate-security-alerts"></a>Güvenlik uyarılarını keşfedin ve araştırın

Veri güvenliği uyarılarınız Güvenlik Merkezi'nin uyarılar sayfasında, kaynağın güvenlik sekmesinde veya uyarı e-postalarında doğrudan bağlantı aracılığıyla kullanılabilir.

1. Uyarıları görüntülemek için:

    * Güvenlik Merkezi'nde - Kenar çubuğundan **Güvenlik uyarılarını** tıklatın ve bir uyarı seçin.
    * Kaynak kapsamında - İlgili kaynak sayfasını açın ve kenar çubuğundan **Güvenlik'i**tıklatın. 

1. Uyarılar, her birinde ayrıntılı düzeltme adımları ve soruşturma bilgileriyle birlikte bağımsız olacak şekilde tasarlanmıştır. Daha geniş bir görünüm için diğer Azure Güvenlik Merkezi ve Azure Sentinel özelliklerini kullanarak daha fazla araştırma yapabilirsiniz:

    * Daha fazla araştırma için SQL Server'ın denetim özelliğini etkinleştirin. Azure Sentinel kullanıcısıysanız, Windows Güvenlik Günlüğü etkinliklerinden Sql denetim günlüklerini Sentinel'e yükleyebilir ve zengin bir araştırma deneyiminin keyfini çıkarabilirsiniz.
    * Güvenlik duruşunuzu iyileştirmek için, güvenlik merkezinin her uyarıda belirtilen ana bilgisayar makinesi için önerilerini kullanın. Bu, gelecekteki saldırıların risklerini azaltacaktır. 


## <a name="next-steps"></a>Sonraki adımlar

İlgili materyaller için aşağıdaki makaleye bakın:

- [Öneriler nasıl düzeltilir?](security-center-remediate-recommendations.md)