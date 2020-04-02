---
title: Mesai saatleri dışında başlat/durdur un çözümlemesini etkinleştirin
description: Bu makalede, Azure sanal makineleriniz için Azure Otomasyon Başlat/Durdur VM çözümünüzü niçin etkinleştireceğimiz açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 4f4b0805e2a9247aff881a9e34b0bd1cd4c6ca27
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550415"
---
# <a name="enable-azure-startstop-vms-solution"></a>Azure Başlat/Durdur VM çözümlerini etkinleştirme

Yeni veya varolan Bir Otomasyon hesabına ve bağlantılı Log Analytics çalışma alanına mesai dışı çözüm sırasında Start/Stop VM'leri eklemek için aşağıdaki adımları gerçekleştirin. Onboarding işlemini tamamladıktan sonra, çözümü özelleştirmek için değişkenleri yapılandırın.

>[!NOTE]
>Bu çözümü Klasik VM'lerle kullanmak için varsayılan olarak oluşturulmayan bir Klasik RunAs hesabına ihtiyacınız vardır. Klasik RunAs hesabı oluşturma yönergeleri [için, Klasik Run-As Hesapları'na](automation-create-standalone-account.md#classic-run-as-accounts)bakın.
>

## <a name="enable-solution"></a>Çözümü etkinleştirme

1. Azure [portalında](https://portal.azure.com)oturum açın.

2. **Otomasyon Hesaplarını**arayın ve seçin.

3. Otomasyon **Hesapları** sayfasında, listeden Otomasyon hesabınızı seçin.

4. Otomasyon Hesabından **İlgili Kaynaklar**altında **Başlat/Durdur VM'yi** seçin. Buradan daha fazla **bilgi edinin ve çözümü etkinleştirebilirsiniz.** Zaten dağıtılan bir Başlat/Durdur VM çözümünüz varsa, **çözümü Yönet'e** tıklayarak ve listede bularak bu çözümü seçebilirsiniz.

   ![Otomasyon hesabından etkinleştirme](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Ayrıca, **kaynak oluştur'a**tıklayarak Azure portalının herhangi bir yerinden de oluşturabilirsiniz. Market sayfasında, **Başlat** veya **Başlat/Durdur**gibi bir anahtar kelime yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Alternatif olarak, çözümün tam adından bir veya daha fazla anahtar kelime yazabilir ve ardından Enter tuşuna basabilirsiniz. Arama sonuçlarından **mesai saatleri dışında Başlat/Durdur VM'leri** seçin.

5. Seçilen çözüm için mesai dışı sayfada **Başlat/Durdur VM'lerinde** özet bilgilerini gözden geçirin ve ardından **Oluştur'u**tıklatın.

   ![Azure portal](media/automation-solution-vm-management/azure-portal-01.png)

6. **Çözüm Ekle** sayfası görüntülenir. Otomasyon aboneliğinize aktaramadan önce çözümü yapılandırmanız istenir.

   ![VM Yönetimi Çözüm Ekle sayfası](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

7. Çözüm **Ekle** sayfasında **Çalışma Alanı'nı**seçin. Otomasyon hesabının içinde olduğu aynı Azure aboneliğine bağlı bir Günlük Analizi çalışma alanı seçin. Çalışma alanınız **yoksa, Yeni Çalışma Alanı Oluştur'u**seçin. Günlük **Analitiği çalışma alanı** sayfasında aşağıdaki adımları gerçekleştirin:

   - "ContosoLAWorkspace" gibi yeni **Log Analytics çalışma alanı**için bir ad belirtin.
   - Varsayılan seçili uygun değilse, açılır listeden seçerek bağlantı kuracak bir **Abonelik** seçin.
   - **Kaynak Grubu**için yeni bir kaynak grubu oluşturabilir veya varolan bir tane seçebilirsiniz.
   - Bir **Konum** seçin.
   - Bir **Fiyatlandırma katmanı** seçin. GB **Başına (Bağımsız)** seçeneğini seçin. Azure Monitor günlükleri [fiyatlandırmayı](https://azure.microsoft.com/pricing/details/log-analytics/) güncelledi ve GB Başına katman tek seçenektir.

   > [!NOTE]
   > Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
   >
   > Desteklenen eşleme çiftleri listesi [için, Otomasyon Hesabı ve Günlük Analizi çalışma alanı için Bölge eşleme](how-to/region-mappings.md)bölümüne bakın.

8. **Log Analytics çalışma alanı** sayfasında gerekli bilgileri sağladıktan sonra **Oluştur'u**tıklatın. Bildirimler **altında** ilerlemesini menüden izleyebilirsiniz ve bu da bittiğinde **Çözüm Ekle** sayfasına geri döner.

9. Çözüm **Ekle** sayfasında **Otomasyon hesabını**seçin. Yeni bir Log Analytics çalışma alanı oluşturuyorsanız, bu hesapla ilişkilendirilecek yeni bir Otomasyon hesabı oluşturabilir veya log analytics çalışma alanına zaten bağlı olmayan mevcut bir Otomasyon Hesabı seçebilirsiniz. Varolan bir Otomasyon Hesabı seçin veya **Bir Otomasyon Hesabı Oluştur'u**tıklatın ve Ek **Otomasyon hesabı** sayfasında aşağıdaki bilgileri sağlayın:
 
   - **Ad** alanına Otomasyon hesabının adını girin.

     Diğer tüm seçenekler, seçilen Log Analytics çalışma alanına göre otomatik olarak doldurulur. Bu seçenekler değiştirilemez. Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. **Tamam'ı**tıklattıktan sonra yapılandırma seçenekleri doğrulanır ve Otomasyon hesabı oluşturulur. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

10. Son olarak, **Çözüm Ekle** sayfasında **Yapılandırma'yı**seçin. **Parametreler** sayfası görüntülenir.

    ![Çözüm için parametreler sayfası](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Burada, sizden istenir:
  
   - Hedef **Kaynak Grubu Adlarını**belirtin. Bu değerler, bu çözüm tarafından yönetilecek VM'ler içeren kaynak grubu adlarıdır. Virgül kullanarak birden fazla ad girebilir ve her birini ayırabilirsiniz (değerler büyük/küçük harf duyarlı değildir). Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir. Bu değer **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenlerde depolanır.
  
   - **VM Dışlama Listesini (dize) belirtin.** Bu değer, hedef kaynak grubundan bir veya daha fazla sanal makinenin adıdır. Virgül kullanarak birden fazla ad girebilir ve her birini ayırabilirsiniz (değerler büyük/küçük harf duyarlı değildir). Joker karakter kullanımı desteklenir. Bu değer **External_ExcludeVMNames** değişkeninde depolanır.
  
   - **Zamanlama**seçin. Zamanlamanız için bir tarih ve saat seçin. Seçtiğiniz saatten başlayarak yinelenen bir günlük zamanlama oluşturulur. Farklı bir bölge seçmek kullanılamaz. Çözümü yapılandırdıktan sonra zamanlamayı belirli saat diliminize yapılandırmak için [bkz.](automation-solution-vm-management-config.md#modify-the-startup-and-shutdown-schedules)
  
   - Bir eylem grubundan **E-posta bildirimleri** almak için **Evet** varsayılan değerini kabul edin ve geçerli bir e-posta adresi sağlayın. **Hayır'ı** seçerseniz ancak daha sonraki bir tarihte e-posta bildirimleri almak istediğinize karar verirseniz, virgülle ayrılmış geçerli e-posta adresleriyle oluşturulan [eylem grubunu](../azure-monitor/platform/action-groups.md) güncelleştirebilirsiniz. Ayrıca aşağıdaki uyarı kurallarını etkinleştirmeniz gerekir:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > Hedef Kaynak **Grubu Adları** için **&ast;** varsayılan değer bir . Bu, abonelikteki tüm VM'leri hedefler. Çözümün aboneliğinizdeki tüm VM'leri hedeflemesini istemiyorsanız, bu değerin zamanlamaları etkinleştirmeden önce kaynak grubu adları listesine güncelleştirilmesi gerekir.

11. Çözüm için gereken ilk ayarları yapılandırıldıktan sonra **Parametreler** sayfasını kapatmak için **Tamam'ı** tıklatın ve **Oluştur'u**seçin. 

Tüm ayarlar doğrulandıktan sonra, çözüm aboneliğinize dağıtılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve **bildirimler** altında ilerlemesini menüden izleyebilirsiniz.

> [!NOTE]
> Bir Azure Bulut Çözüm Sağlayıcısı (Azure CSP) aboneliğiniz varsa, dağıtım tamamlandıktan sonra Otomasyon Hesabınızda **Paylaşılan Kaynaklar** altındaki **Değişkenler'e** gidin ve [**External_EnableClassicVMs**](automation-solution-vm-management.md#variables) değişkenini **False**olarak ayarlayın. Bu, çözümün Klasik VM kaynaklarını aramasını durdurur.

## <a name="next-steps"></a>Sonraki adımlar

Artık çözümü etkinleştirdiğinize göre, bunu VM yönetim gereksinimlerinizi destekleyecek şekilde [yapılandırabilirsiniz.](automation-solution-vm-management-config.md)