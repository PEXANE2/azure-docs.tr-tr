---
title: Azure Otomasyonu 'Nu devre dışı bırakma çözüm sırasında VM 'Leri Başlat/Durdur
description: Bu makalede, Azure sanal makineleriniz için Azure Otomasyonu başlatma/durdurma VM çözümünün nasıl etkinleştirileceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: d47daa29c65f847fdeb33b9e24a892ac1f31b52a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82096971"
---
# <a name="enable-azure-automation-startstop-vms-solution"></a>Azure Otomasyonu başlatma/durdurma VM çözümünü etkinleştirme

**VM'leri çalışma saatleri dışında Başlat/Durdur** çözümünü yeni veya var olan bir Otomasyon hesabına ve bağlantılı Log Analytics çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin. Ekleme işlemini tamamladıktan sonra, çözümü özelleştirmek için değişkenleri yapılandırın.

>[!NOTE]
>Bu çözümü klasik VM 'lerle birlikte kullanmak için, varsayılan olarak oluşturulmamış bir klasik Farklı Çalıştır hesabına ihtiyacınız vardır. Klasik farklı çalıştır hesabı oluşturma yönergeleri için bkz. [Klasik farklı çalıştır hesabı oluşturma](automation-create-standalone-account.md#create-a-classic-run-as-account).
>

## <a name="enable-solution"></a>Çözümü etkinleştirme

1. Azure [portalında](https://portal.azure.com)oturum açın.

2. **Otomasyon hesaplarını**arayın ve seçin.

3. Otomasyon hesapları sayfasında, listeden Otomasyon hesabınızı seçin.

4. Otomasyon hesabından **Ilgili kaynaklar**altında **VM Başlat/Durdur** ' u seçin. Buradan **daha fazla bilgi edinin ve çözümü etkinleştirin**. Zaten dağıtılan bir Başlat/Durdur VM çözümünüz varsa, **çözümü Yönet** ' i tıklatarak ve listede bularak bunu seçebilirsiniz.

   ![Otomasyon hesabından etkinleştir](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Ayrıca, **kaynak oluştur ' a**tıklayarak Azure Portal herhangi bir yerden oluşturabilirsiniz. Market sayfasında **Başlat** veya **Başlat/Durdur**gibi bir anahtar sözcük yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Alternatif olarak, çözümün tam adından bir veya daha fazla anahtar sözcük yazabilir ve ardından ENTER tuşuna basabilirsiniz. Arama sonuçlarından **VM'leri çalışma saatleri dışında Başlat/Durdur** seçin.

5. Seçili çözümün **VM'leri çalışma saatleri dışında Başlat/Durdur** sayfasında, Özet bilgilerini gözden geçirin ve ardından **Oluştur**' a tıklayın.

   ![Azure portal](media/automation-solution-vm-management/azure-portal-01.png)

6. Çözüm Ekle sayfası görüntülenir. Çözümü Otomasyon aboneliğinize aktarmadan önce yapılandırmanız istenir.

   ![VM yönetimi çözüm ekleme sayfası](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

7. Çözüm Ekle sayfasında, **çalışma alanı**' nı seçin. Otomasyon hesabının bulunduğu Azure aboneliğine bağlı bir Log Analytics çalışma alanı seçin. Çalışma alanınız yoksa **Yeni çalışma alanı oluştur**' u seçin. Log Analytics çalışma alanı sayfasında, aşağıdaki adımları uygulayın:

   - Yeni Log Analytics çalışma alanı için **Contosolaworkspace**gibi bir ad belirtin.
   - Varsayılan seçilen değer uygun değilse, açılan listeden seçerek bağlantı için bir **abonelik** seçin.
   - **Kaynak grubu**için, yeni bir kaynak grubu oluşturabilir veya var olan bir grup seçebilirsiniz.
   - Bir **Konum** seçin.
   - Bir **Fiyatlandırma katmanı** seçin. **GB başına (tek başına)** seçeneğini belirleyin. Azure Izleyici günlükleri güncelleştirilmiş [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/log-analytics/) sahıptır ve GB başına katman tek seçenektir.

   > [!NOTE]
   > Çözümleri etkinleştirirken, bir Log Analytics çalışma alanı ve Otomasyon hesabı bağlamak için yalnızca belirli bölgeler desteklenir.
   >
   > Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

8. Log Analytics çalışma alanı sayfasında gerekli bilgileri sağladıktan sonra **Oluştur**' a tıklayın. Bu işlemi, tamamlandığında çözüm Ekle sayfasına geri getiren menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

9. Çözüm Ekle sayfasında **Otomasyon hesabı**' nı seçin. Yeni bir Log Analytics çalışma alanı oluşturuyorsanız, onunla ilişkilendirilecek yeni bir Otomasyon hesabı oluşturabilir veya bir Log Analytics çalışma alanına bağlı olmayan var olan bir Otomasyon hesabı seçebilirsiniz. Var olan bir Otomasyon hesabı seçin veya **Otomasyon**hesabı Ekle ' ye tıklayıp Otomasyon hesabı Ekle sayfasında aşağıdaki bilgileri sağlayın:
 
   - **Ad** alanına Otomasyon hesabının adını girin.

     Diğer tüm seçenekler, seçilen Log Analytics çalışma alanına göre otomatik olarak doldurulur. Bu seçenekler değiştirilemez. Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. **Tamam**' a tıkladıktan sonra yapılandırma seçenekleri onaylanır ve Otomasyon hesabı oluşturulur. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

10. Son olarak, çözüm Ekle sayfasında **yapılandırma**' yı seçin. Parametreler sayfası görüntülenir.

    ![Çözüm için parametreler sayfası](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Şunları yapmanız istenir:
  
   - **Hedef ResourceGroup adlarını**belirtin. Bu değerler, bu çözüm tarafından yönetilecek VM 'Leri içeren kaynak grubu adlarıdır. Birden fazla ad girebilir ve her birini virgülle ayırarak ayırabilirsiniz (değerler büyük/küçük harfe duyarlı değildir). Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir. Bu değer **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenlerinde depolanır.
  
   - **VM dışlama listesini (dize)** belirtin. Bu değer, hedef kaynak grubundaki bir veya daha fazla sanal makinenin adıdır. Birden fazla ad girebilir ve her birini virgülle ayırarak ayırabilirsiniz (değerler büyük/küçük harfe duyarlı değildir). Joker karakter kullanımı desteklenir. Bu değer **External_ExcludeVMNames** değişkeninde depolanır.
  
   - Bir **zamanlama**seçin. Zamanlamanız için bir tarih ve saat seçin. Seçtiğiniz zamandan itibaren bir yeniden gerçekleşen günlük zamanlama oluşturulacaktır. Farklı bir bölge seçme kullanılamıyor. Çözümü yapılandırdıktan sonra, zamanlamayı belirli saat diliminize göre yapılandırmak için, bkz. [başlatma ve kapatılma zamanlamasını değiştirme](automation-solution-vm-management-config.md#modify-the-startup-and-shutdown-schedules).
  
   - Bir eylem grubundan **e-posta bildirimleri** almak için **Yes** varsayılan değerini kabul edin ve geçerli bir e-posta adresi sağlayın. **Hayır** ' ı seçerseniz ve e-posta bildirimleri almak istediğiniz sonraki bir tarihte karar verirseniz, geçerli e-posta adresleriyle oluşturulan [Eylem grubunu](../azure-monitor/platform/action-groups.md) virgülle ayırarak güncelleştirebilirsiniz. Aşağıdaki uyarı kurallarını da etkinleştirmeniz gerekir:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > **Hedef ResourceGroup adları** için varsayılan değer bir **&ast;**. Bu, bir abonelikteki tüm VM 'Leri hedefler. Çözümün aboneliğinizdeki tüm VM 'Leri hedeflemesini istemiyorsanız, zamanlamalar etkinleştirilmeden önce bu değerin kaynak grubu adları listesine güncelleştirilmesi gerekir.

11. Çözüm için gereken başlangıç ayarlarını yapılandırdıktan sonra, **Tamam** ' a tıklayarak parametreler sayfasını kapatın ve **Oluştur**' u seçin. 

Tüm ayarlar doğrulandıktan sonra, çözüm aboneliğinize dağıtılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve ilerleme durumunu menüdeki **Bildirimler** ' in altından izleyebilirsiniz.

> [!NOTE]
> Azure bulut çözümü sağlayıcısı (Azure CSP) aboneliğiniz varsa, dağıtım tamamlandıktan sonra Otomasyon hesabınızda, **paylaşılan kaynaklar** altındaki **değişkenlere** gidin ve [External_EnableClassicVMs](automation-solution-vm-management.md#variables) değişkenini **false**olarak ayarlayın. Bu, çözümün klasik VM kaynaklarına bakmasını engeller.

## <a name="next-steps"></a>Sonraki adımlar

Çözümü etkinleştirmiş olduğunuza göre, bunu VM yönetimi gereksinimlerinizi destekleyecek şekilde [yapılandırabilirsiniz](automation-solution-vm-management-config.md) .