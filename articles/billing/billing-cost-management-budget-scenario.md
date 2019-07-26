---
title: Azure Faturalandırma ve maliyet yönetimi bütçe senaryosu | Microsoft Docs
description: Azure Otomasyonu 'nu kullanarak VM 'Leri belirli bütçe eşiklerine göre kapatma hakkında bilgi edinin.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
editor: ''
tags: billing
ms.assetid: db93f546-6b56-4b51-960d-1a5bf0274fc8
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 03/13/2019
ms.author: banders
ms.openlocfilehash: 37f129526cb184a2eeee9e36028e8f00b5bbc247
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443469"
---
# <a name="manage-costs-with-azure-budgets"></a>Azure Budgets ile maliyetleri yönetme

Maliyet denetimi, buluttaki yatırımınızın değerini en üst düzeye çıkarmak için önemli bir bileşendir. Maliyet görünürlüğü, raporlama ve maliyet tabanlı Orchestration 'un devam eden iş işlemleri için kritik öneme sahip çeşitli senaryolar vardır. [Azure maliyet yönetimi API 'leri](https://docs.microsoft.com/rest/api/consumption/) , bu senaryoların her birini desteklemek Için bir API kümesi sağlar. API 'Ler, kullanım ayrıntılarını sağlar ve ayrıntılı örnek düzeyi maliyetlerinizi görüntülemenize olanak tanır.

Bütçeler genellikle maliyet denetiminin bir parçası olarak kullanılır. Bütçeler Azure 'da kapsamı değiştirilebilir. Örneğin, abonelik, kaynak grupları veya bir kaynak koleksiyonu temelinde bütçe görünümünüzü daraltabilirsiniz. Bütçe eşiğine ulaşıldığında, e-posta yoluyla bildirim almak için bütçeleri API 'SI kullanmanın yanı sıra, bir bütçe olayının sonucu olarak düzenlenmiş bir eylem kümesi tetiklemek için [Azure izleyici eylem gruplarını](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) kullanabilirsiniz.

Kritik olmayan bir iş yükü çalıştıran bir müşteriye yönelik ortak bir bütçe senaryosu, bir bütçeye göre yönetmek istediklerinde ve ayrıca aylık faturaya bakarken öngörülebilir bir maliyete ulaşmak istediklerinde gerçekleşebilir. Bu senaryo, Azure ortamının bir parçası olan kaynakların bazı maliyet tabanlı düzenlemesini gerektirir. Bu senaryoda, abonelik için $1000 aylık bir bütçesi ayarlanır. Ayrıca, bildirim eşikleri birkaç düzenleme tetiklenecek şekilde ayarlanır. Bu senaryo, kaynak grubundaki tüm VM 'Leri **Isteğe bağlı**olarak durduran% 80 maliyet eşiğine sahip olur. Ardından,% 100 maliyet eşiğine göre tüm VM örnekleri durdurulacak.
Bu senaryoyu yapılandırmak için, Bu öğreticinin her bir bölümünde belirtilen adımları izleyerek aşağıdaki eylemleri tamamlayacaksınız.

Bu öğreticide yer alan bu eylemler şunları yapmanıza olanak sağlar:

- Web kancalarını kullanarak VM 'Leri durdurmak için bir Azure Otomasyonu runbook 'U oluşturun.
- Bütçe eşiği değerine göre tetiklenecek bir Azure mantıksal uygulaması oluşturun ve Runbook 'u doğru parametrelerle çağırın.
- Bütçe eşiği karşılandığında Azure Logic App 'i tetiklemek için yapılandırılacak bir Azure Izleyici eylem grubu oluşturun.
- İstenen eşiklerle Azure bütçesini oluşturun ve eylem grubuna bağlayın.

## <a name="create-an-azure-automation-runbook"></a>Azure Otomasyonu runbook 'U oluşturma

[Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) , kaynak yönetimi görevlerinizin çoğunu betikten ve bu görevleri Zamanlanmış ya da isteğe bağlı olarak çalıştırmanıza olanak tanıyan bir hizmettir. Bu senaryonun bir parçası olarak, VM 'Leri durdurmak için kullanılacak bir [Azure Otomasyonu runbook 'u](https://docs.microsoft.com/azure/automation/automation-runbook-types) oluşturacaksınız. Bu senaryoyu oluşturmak için [galerideki](https://docs.microsoft.com/azure/automation/automation-runbook-gallery) [Azure v2 VM 'lerini durdur](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafik runbook 'unu kullanacaksınız. Bu runbook 'u Azure hesabınıza aktararak ve yayımlayarak, bir bütçe eşiğine ulaşıldığında VM 'Leri durdurabilirsiniz.

### <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com/) oturum açın.
2. Azure 'un sol üst köşesinde bulunan **kaynak oluştur** düğmesine tıklayın.
3. **Yönetim Araçları** > **Otomasyonu**' nu seçin.
   > [!NOTE]
   > Azure hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)oluşturabilirsiniz.
4. Hesap bilgilerinizi girin. **Azure farklı çalıştır hesabı oluştur**Için, Azure 'da kimlik doğrulamasını basitleştirmek üzere gereken ayarları otomatik olarak etkinleştirmek için **Evet** ' i seçin.
5. İşlemi tamamladığınızda Otomasyon hesabı dağıtımını başlatmak için **Oluştur**'a tıklayın.

### <a name="import-the-stop-azure-v2-vms-runbook"></a>Azure v2 VM 'lerini durdur runbook 'u içeri aktarma

[Azure Otomasyonu runbook](https://docs.microsoft.com/azure/automation/automation-runbook-types)'u kullanarak Galeriden [Azure v2 VM 'lerini durdur](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafik runbook 'unu içeri aktarın.

1.  Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com/) oturum açın.
2.  **Tüm hizmetler** > **Otomasyon hesapları**' nı seçerek Otomasyon hesabınızı açın. Ardından Otomasyon hesabınızı seçin.
3.  **Işlem Otomasyonu** bölümünde **runbook 'lar Galerisi** ' ne tıklayın.
4.  **Galeri kaynağını** **betik merkezine** ayarlayın ve **Tamam**' ı seçin.
5.  Azure portal içindeki [Azure v2 VM 'Lerini durdur](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) Galeri öğesini bulun ve seçin.
6.  İçeri Aktar **düğmesine tıklayarak** **içe aktarma** dikey penceresini görüntüleyin ve **Tamam**' ı seçin. Runbook 'a genel bakış dikey penceresi görüntülenir.
7.  Runbook içeri aktarma işlemini tamamladıktan sonra grafik runbook düzenleyicisini ve yayımlama seçeneğini göstermek için **Düzenle** ' yi seçin.

    ![Azure-grafik runbook 'unu düzenleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-01.png)
8.  Runbook 'u yayımlamak için **Yayımla** düğmesine tıklayın ve istendiğinde **Evet** ' i seçin. Bir runbook yayımladığınızda, varolan yayımlanmış bir sürümü taslak sürümü ile geçersiz kılarsınız. Bu durumda, runbook 'u oluşturduğunuz için yayımlanmış sürümünüz yok.

    Runbook yayımlama hakkında daha fazla bilgi için bkz. [grafik runbook 'U oluşturma](https://docs.microsoft.com/azure/automation/automation-first-runbook-graphical).

## <a name="create-webhooks-for-the-runbook"></a>Runbook için Web kancaları oluşturma

[Azure v2 VM 'Lerini durdur](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-ARM-VMs-1ba96d5b) grafik runbook 'unu kullanarak, tek bir http Isteği aracılığıyla Azure Otomasyonu 'nda runbook 'u başlatmak Için Iki Web kancası oluşturacaksınız. İlk Web kancası, runbook 'u kaynak grubu adı olan% 80 bütçe eşiğinden bir parametre olarak çağırır ve isteğe bağlı VM 'Lerin durdurulmasına izin verir. Ardından, ikinci Web kancası runbook 'u bir parametre olmadan (% 100) çağırır, bu da kalan tüm VM örneklerini durdurur.

1. [Azure Portal](https://portal.azure.com/)runbook **'lar** sayfasında, runbook 'un genel bakış dikey penceresini görüntüleyen **StopAzureV2Vm** runbook ' a tıklayın.
2. **Web kancası Ekle** dikey penceresini açmak için sayfanın üst kısmındaki **Web kancası** ' na tıklayın.
3. Yeni Web kancası **Oluştur '** a tıklayarak **Yeni Web kancası oluştur** dikey penceresini açın.
4. Web kancasının **adını** **isteğe bağlı**olarak ayarlayın. **Enabled** özelliği **Yes**olmalıdır. **Süre sonu** değerinin değiştirilmesi gerekmez. Web kancası özellikleri hakkında daha fazla bilgi için bkz. [Web kancasının ayrıntıları](https://docs.microsoft.com/azure/automation/automation-webhooks#details-of-a-webhook).
5. URL değerinin yanında, Web kancasının URL 'sini kopyalamak için Kopyala simgesine tıklayın.
   > [!IMPORTANT]
   > **Isteğe bağlı** olarak adlandırılan Web KANCASıNıN URL 'sini güvenli bir yere kaydedin. URL 'YI Bu öğreticinin ilerleyen kısımlarında kullanacaksınız. Güvenlik nedenleriyle, Web kancasını oluşturduktan sonra URL 'YI görüntüleyemez veya alamazsınız.
6. Yeni Web kancasını oluşturmak için **Tamam** ' ı tıklatın.
7. Runbook 'un parametre değerlerini görüntülemek için **parametreleri Yapılandır ve ayarları Çalıştır** ' a tıklayın.
   > [!NOTE]
   > Runbook 'un zorunlu parametreleri varsa, hiçbir değer sağlanmadığı müddetçe Web kancasını oluşturabileceksiniz.
8. Web kancası parametre değerlerini kabul etmek için **Tamam** ' ı tıklatın.
9. Web kancasını oluşturmak için **Oluştur** ' a tıklayın.
10. Sonra, **Tamam**adlı ikinci bir Web kancası oluşturmak için yukarıdaki adımları izleyin.
    > [!IMPORTANT]
    > Bu öğreticide daha sonra kullanmak üzere her iki Web kancası URL 'sini kaydettiğinizden emin olun. Güvenlik nedenleriyle, Web kancasını oluşturduktan sonra URL 'YI görüntüleyemez veya alamazsınız.

Şimdi, kaydettiğiniz URL 'Ler kullanılarak her biri kullanılabilir olan iki Web kancaya sahip olmanız gerekir.

![Web kancaları-Isteğe bağlı ve tam](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-02.png)

Artık Azure Otomasyonu kurulumu ile işiniz bitti. Web kancalarını basit bir Postman testiyle test edebilir ve Web kancasının çalışıp çalışmadığını doğrulayabilirsiniz. Daha sonra, düzenleme için mantıksal uygulamayı oluşturmanız gerekir.

## <a name="create-an-azure-logic-app-for-orchestration"></a>Düzenleme için bir Azure mantıksal uygulaması oluşturma

Logic Apps, uygulamaları, verileri, sistemleri ve Hizmetleri kuruluşlar veya kuruluşlar arasında tümleştirebilmeniz için işlemleri iş akışları olarak oluşturmanıza, zamanlamanıza ve otomatikleştirmenize yardımcı olur. Bu senaryoda, oluşturduğunuz [mantıksal uygulama](https://docs.microsoft.com/azure/logic-apps/) yalnızca oluşturduğunuz Otomasyon Web kancasını çağırtığından çok daha fazlasını yapacaktır.

Bütçeler, belirtilen bir eşik karşılandığında bildirim tetiklemek üzere ayarlanabilir. ' De bildirim almak için birden çok eşik sağlayabilirsiniz ve mantıksal uygulama, eşiğe ulaşılması temelinde farklı eylemler gerçekleştirmenize olanak tanır. Bu örnekte, birkaç bildirim alacağınız bir senaryo ayarlayacaksınız. ilk bildirim, bütçenin% 80 ' inin ulaştığı ve ikinci bildirimin ise bütçenin% 100 ' i ne zaman ulaşılırsa yapılır. Mantıksal uygulama, kaynak grubundaki tüm VM 'Leri kapatmak için kullanılacaktır. İlk olarak,% 80 ' de **Isteğe bağlı** eşiğe ulaşılmayacak, ardından abonelikteki tüm VM 'lerin kapatılabileceği ikinci eşiğe ulaşılacaktır.

Logic Apps, HTTP tetikleyicisi için örnek bir şema sağlamanıza olanak tanır, ancak **Content-Type** üst bilgisini ayarlamanızı gerektirir. Eylem grubunun Web kancası için özel üst bilgileri olmadığından, yükü ayrı bir adımda ayrıştırmalısınız. **Parse** eylemini kullanacaksınız ve örnek bir yük ile sağlayacaksınız.

### <a name="create-the-logic-app"></a>Mantıksal uygulama oluşturma

Mantıksal uygulama çeşitli eylemler gerçekleştirir. Aşağıdaki liste, mantıksal uygulamanın gerçekleştireceği yüksek düzeyli bir eylemler kümesi sağlar:
- Bir HTTP isteğinin ne zaman alındığını tanır
- Geçirilen eşik değerini öğrenmek için geçirilen JSON verilerini ayrıştırın
- Eşik tutarının% 80 ' e veya daha büyük bir değere sahip olup olmadığını denetlemek için koşullu bir ifade kullanın, ancak% 100 ' den büyük veya buna eşit değildir.
    - Bu eşik miktarına ulaşıldığında, **Isteğe bağlı**olarak adlandırılan Web kancasını kullanarak BIR http gönderisi gönderin. Bu eylem, "Isteğe bağlı" grubundaki VM 'Leri kapatacak.
- Eşik tutarının, bütçe değerinin% 100 ' e ulaşıldığını veya aşmadığını denetlemek için koşullu bir ifade kullanın.
    - Eşik miktarına ulaşıldığında, **tamamlandı**adlı Web kancasını kullanarak BIR http gönderisi gönderin. Bu eylem, kalan tüm VM 'Leri kapatacak.

Yukarıdaki adımları gerçekleştirecek mantıksal uygulamayı oluşturmak için aşağıdaki adımlar gereklidir:

1.  [Azure Portal](https://portal.azure.com/), **kaynak** > tümleştirmemantıksal > **uygulaması**oluştur ' u seçin.

    ![Azure-mantıksal uygulama kaynağını seçin](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03.png)
2.  **Mantıksal uygulama oluştur** dikey penceresinde, mantıksal uygulamanızı oluşturmak için gereken ayrıntıları sağlayın, **panoya sabitle**' yi seçin ve **Oluştur**' a tıklayın.

    ![Azure-mantıksal uygulama oluşturma](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-03a.png)

Azure mantıksal Uygulamanızı dağıttıktan sonra, **Logic Apps Tasarımcısı** açılır ve bir tanıtım videosu ile sık kullanılan tetikleyicilerle bir dikey pencere gösterir.

### <a name="add-a-trigger"></a>Tetikleyici ekleyin

Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur. Eylemler, tetikleyiciden sonra gerçekleşen tüm adımlardır.

1.  **Logic Apps Designer** dikey penceresindeki **Şablonlar** ' ın altında **boş mantıksal uygulama**' yı seçin.
2.  **Logic Apps tasarımcı** arama kutusuna "http isteği" girerek bir [TETIKLEYICI](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts) ekleyerek bir **http isteği alındığında adlandırılmış istek**tetikleyicisi ' ni bulun ve seçin.

    ![Azure-Logic app-http tetikleyicisi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-04.png)
3.  **Yeni adım** > **Eylem Ekle**' yi seçin.

    ![Azure-yeni adım-eylem ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-05.png)
4.  **Veri işlemleri-JSON ayrıştırma** [eylemini](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview#logic-app-concepts)bulmak ve seçmek için **LOGIC Apps Tasarımcısı** arama kutusunda "JSON 'ı ayrıştır" ifadesini arayın.

    ![Azure-Logic App-JSON ayrıştırma eylemi ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-06.png)
5.  JSON yükünün ayrıştırma için **içerik** adı olarak "yük" girin veya dinamik Içerikten "Body" etiketini kullanın.
6.  **JSON 'ı Ayrıştır** kutusunda **şema oluşturmak Için örnek yükü kullan** seçeneğini belirleyin.

    ![Azure-Logic App-şema oluşturmak için örnek JSON verilerini kullanın](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-07.png)
7.  Aşağıdaki JSON örnek yükünü metin kutusuna yapıştırın:`{"schemaId":"AIP Budget Notification","data":{"SubscriptionName":"CCM - Microsoft Azure Enterprise - 1","SubscriptionId":"<GUID>","SpendingAmount":"100","BudgetStartDate":"6/1/2018","Budget":"50","Unit":"USD","BudgetCreator":"email@contoso.com","BudgetName":"BudgetName","BudgetType":"Cost","ResourceGroup":"","NotificationThresholdAmount":"0.8"}}`

    Metin kutusu aşağıdaki gibi görünür:

    ![Azure-Logic app-örnek JSON yükü](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-08.png)
8.  **Bitti**’ye tıklayın.

### <a name="add-the-first-conditional-action"></a>İlk koşullu eylemi ekleyin

Eşik tutarının% 80 ' e veya daha büyük bir değere sahip olup olmadığını denetlemek için koşullu bir ifade kullanın, ancak% 100 ' den büyük veya buna eşit değildir. Bu eşik miktarına ulaşıldığında, **Isteğe bağlı**olarak adlandırılan Web kancasını kullanarak BIR http gönderisi gönderin. Bu eylem, **Isteğe bağlı** gruptaki VM 'leri kapatacak.

1.  **Yeni adım** > **Koşul Ekle**' yi seçin.

    ![Azure-Logic App-koşul ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-09.png)
2.  **Koşul** kutusunda, kullanılabilir değerlerin listesini göstermek için **bir değer seçin** ' i içeren TextBox ' ı tıklatın.

    ![Azure-Logic App-koşul kutusu](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-10.png)

3.  Listenin en üstünde bulunan **ifadeye** tıklayın ve ifade düzenleyicisine aşağıdaki ifadeyi girin:`float()`

    ![Azure-Logic App-float ifadesi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-11.png)

4.  **Dinamik içerik**' i seçin, imleci parantez içine () yerleştirin ve Tamam ifadesini doldurmak Için listeden **Notificationthresholdadmount** öğesini seçin.

    İfade aşağıdaki gibi olacaktır:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`

5.  İfadeyi ayarlamak için **Tamam ' ı** seçin.
6.  **Koşulun**açılan kutusundan **büyüktür veya eşittir** ' i seçin.
7.  Koşul girin `.8`kutusunda **bir değer seçin** .

    ![Azure-Logic App-bir değer içeren float ifadesi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-12.png)

8.  Koşulun ek bir parçasını eklemek için koşul kutusu içinde**satır ekle** ' ye tıklayın.  > 
9.  **Koşul** kutusunda, **bir değer seçin**içeren metin kutusuna tıklayın.
10. Listenin en üstünde bulunan **ifadeye** tıklayın ve ifade düzenleyicisine aşağıdaki ifadeyi girin:`float()`
11. **Dinamik içerik**' i seçin, imleci parantez içine () yerleştirin ve Tamam ifadesini doldurmak Için listeden **Notificationthresholdadmount** öğesini seçin.
12. İfadeyi ayarlamak için **Tamam ' ı** seçin.
13. **Koşulun**açılan kutusundan **küçüktür** ' i seçin.
14. Koşul girin `1`kutusunda **bir değer seçin** .

    ![Azure-Logic App-bir değer içeren float ifadesi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-13.png)

15. **True ise** kutusunda **Eylem Ekle**' yi seçin. İsteğe bağlı VM 'Leri kapatacak bir HTTP POST eylemi ekleyeceksiniz.

    ![Azure-Logic App-eylem ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-14.png)

16. Http eylemini aramak için **http girin ve** **http – http** eylemini seçin.

    ![Azure-Logic App-HTTP eylemi ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-15.png)

17. **Yöntem** değeri için **gönderi** ' ı seçin.
18. Bu öğreticide daha önce oluşturduğunuz **Isteğe bağlı** Web kancası URL 'sini **URI** değeri olarak girin.

    ![Azure-Logic App-HTTP eylem URI 'SI](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-16.png)

19. **Eğer true ise** kutusunda **Eylem Ekle** ' yi seçin. Alıcıya isteğe bağlı VM 'Lerin kapatıldığını bildiren bir e-posta gönderecek e-posta eylemi ekleyeceksiniz.
20. "E-posta gönder" ifadesini arayın ve kullandığınız e-posta hizmetine bağlı olarak bir *e-posta gönder* eylemi seçin.

    ![Azure-Logic App-e-posta gönder eylemi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-17.png)

    Kişisel Microsoft hesapları için **Outlook.com** girişini seçin. Azure iş veya okul hesapları için **Office 365 Outlook** girişini seçin. Önceden bir bağlantınız yoksa e-posta hesabınızda oturum açmanız istenir. Logic Apps, e-posta hesabınıza bir bağlantı oluşturur.

    Mantıksal uygulamanın e-posta bilgilerinizin erişimine izin vermeniz gerekir.

    ![Azure-Logic App-erişim bildirimi](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-18.png)

21. Alıcıya isteğe bağlı VM 'Lerin kapatıldığını bildiren e-posta için **Kime**, **Konu**ve **gövde** metni ekleyin. Konu ve gövde alanlarını doldurmak için **BudgetName** ve **notificationthresdynamic** içeriğini kullanın.

    ![Azure-Logic App-e-posta ayrıntıları](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-19.png)

### <a name="add-the-second-conditional-action"></a>İkinci koşullu eylemi ekleyin

Eşik tutarının, bütçe değerinin% 100 ' e ulaşıldığını veya aşmadığını denetlemek için koşullu bir ifade kullanın. Eşik miktarına ulaşıldığında, **tamamlandı**adlı Web kancasını kullanarak BIR http gönderisi gönderin. Bu eylem, kalan tüm VM 'Leri kapatacak.

1.  **Yeni adım** > **Koşul Ekle**' yi seçin.

    ![Azure-Logic App-eylem Ekle](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-20.png)

2.  **Koşul** kutusunda, kullanılabilir değerlerin listesini göstermek için **bir değer seçin** ' i içeren TextBox ' ı tıklatın.
3.  Listenin en üstünde bulunan **ifadeye** tıklayın ve ifade düzenleyicisine aşağıdaki ifadeyi girin:`float()`
4.  **Dinamik içerik**' i seçin, imleci parantez içine () yerleştirin ve Tamam ifadesini doldurmak Için listeden **Notificationthresholdadmount** öğesini seçin.

    İfade aşağıdaki gibi olacaktır:<br>
    `float(body('Parse_JSON')?['data']?['NotificationThresholdAmount'])`

5.  İfadeyi ayarlamak için **Tamam ' ı** seçin.
6.  **Koşulun**açılan kutusundan **büyüktür veya eşittir** ' i seçin.
7.  Koşul `1`girin **kutusunda bir değer seçin** .

    ![Azure-Logic App-koşul değeri ayarla](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-21.png)

8.  **True ise** kutusunda **Eylem Ekle**' yi seçin. Kalan tüm VM 'Leri kapatacak bir HTTP POST eylemi ekleyeceksiniz.

    ![Azure-Logic App-eylem ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-22.png)

9.  Http eylemini aramak için **http girin ve** **http – http** eylemini seçin.
10. **Yöntem** değeri için **gönderi** ' ı seçin.
11. Bu öğreticide daha önce oluşturduğunuz **tamamlanmış** adlı Web kancası URL 'sini **URI** değeri olarak girin.

    ![Azure-Logic App-eylem ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-23.png)

12. **Eğer true ise** kutusunda **Eylem Ekle** ' yi seçin. Alıcıya kalan VM 'Lerin kapatıldığını bildiren bir e-posta gönderecek e-posta eylemi ekleyeceksiniz.
13. "E-posta gönder" ifadesini arayın ve kullandığınız e-posta hizmetine bağlı olarak bir *e-posta gönder* eylemi seçin.
14. Alıcıya isteğe bağlı VM 'Lerin kapatıldığını bildiren e-posta için **Kime**, **Konu**ve **gövde** metni ekleyin. Konu ve gövde alanlarını doldurmak için **BudgetName** ve **notificationthresdynamic** içeriğini kullanın.

    ![Azure-Logic App-e-posta gönder ayrıntıları](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-24.png)

15. **Mantıksal uygulama Tasarımcısı** dikey penceresinin en üstündeki **Kaydet** ' e tıklayın.

### <a name="logic-app-summary"></a>Mantıksal uygulama Özeti

Mantıksal uygulamanız tamamlandığında bu şekilde görünür. Eşik tabanlı herhangi bir düzenlemeye ihtiyaç duymakta olan çoğu senaryoda, doğrudan **izleyici** 'den otomasyon betiğini çağırabilir ve **Logic App** adımını atlayabilirsiniz.

   ![Azure-Logic App-tam görünüm](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-25.png)

Mantıksal uygulamanızı kaydettiğinizde, çağrabileceksiniz bir URL oluşturulmuştur. Bu URL 'YI Bu öğreticinin sonraki bölümünde kullanacaksınız.

## <a name="create-an-azure-monitor-action-group"></a>Azure Izleyici eylem grubu oluşturma

Bir eylem grubu, tanımladığınız bir bildirim tercihleri koleksiyonudur. Bir uyarı tetiklendiğinde, belirli bir eylem grubu uyarıyı bilgilendirerek alabilir. Azure uyarısı, belirli koşullara göre bir bildirim oluşturur ve işlem yapmak için fırsat sağlar. Bir uyarı, ölçümler ve günlükler dahil olmak üzere birden fazla kaynaktaki verileri kullanabilir.

İşlem grupları, bütçenize tümleştirilecek tek uç noktadır. Bildirimleri bir dizi kanalda ayarlayabilirsiniz, ancak bu senaryo için bu öğreticide daha önce oluşturduğunuz mantıksal uygulamaya odaklanırsınız.

### <a name="create-an-action-group-in-azure-monitor"></a>Azure Izleyici 'de bir eylem grubu oluşturma

Eylem grubunu oluşturduğunuzda, bu öğreticide daha önce oluşturduğunuz mantıksal uygulamayı işaret edersiniz.

1.  [Azure Portal](https://portal.azure.com/)oturumunuz açık değilse, oturum açın ve **tüm hizmetler** > **İzleyicisi**' ni seçin.
2.  **Ayarlar** bölümünde **Eylemler grupları** ' nı seçin.
3.  **Eylem grupları** dikey penceresinden **eylem grubu Ekle** ' yi seçin.
4.  Aşağıdaki öğeleri ekleyin ve doğrulayın:
    - Eylem grubu adı
    - Kısa ad
    - Subscription
    - Resource group

    ![Azure-Logic App-eylem grubu ekleme](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-26.png)

5.  **Eylem grubu Ekle** bölmesinde bir logicapp eylemi ekleyin. İşlem **Bütçe-BudgetLA**olarak adlandırın. **Mantıksal uygulama** bölmesinde, **aboneliği** ve **kaynak grubunu**seçin. Ardından, bu öğreticide daha önce oluşturduğunuz **mantıksal uygulamayı** seçin.
6.  Mantıksal uygulamayı ayarlamak için **Tamam** ' ı tıklatın. Ardından Eylem grubunu oluşturmak için **eylem grubu Ekle** bölmesinde **Tamam** ' ı seçin.

Bütçenize etkin bir şekilde yönetmek için gereken tüm destekleyici bileşenleri ile işiniz bitti. Şimdi tüm yapmanız gereken, bütçeyi oluşturup oluşturduğunuz eylem grubunu kullanacak şekilde yapılandırmaktır.

## <a name="create-the-azure-budget"></a>Azure bütçesini oluşturma

Maliyet yönetimi 'ndeki [Bütçe özelliğini](../cost-management/tutorial-acm-create-budgets.md) kullanarak Azure Portal bir bütçe oluşturabilirsiniz. Ya da REST API 'leri, PowerShell cmdlet 'lerini kullanarak bir bütçe oluşturabilir veya CLı kullanabilirsiniz. Aşağıdaki yordam REST API kullanır. REST API çağrılmadan önce, bir yetkilendirme belirtecine ihtiyacınız olacaktır. Bir yetkilendirme belirteci oluşturmak için [Armclient](https://github.com/projectkudu/ARMClient) projesini kullanabilirsiniz. **Armclient** , Azure Resource Manager kimlik doğrulamasını ve API 'leri çağırmak için bir belirteç almanızı sağlar.

### <a name="create-an-authentication-token"></a>Kimlik doğrulama belirteci oluşturma

1.  GitHub 'da [Armclient](https://github.com/projectkudu/ARMClient) projesine gidin.
2.  Yerel bir kopya almak için depoyu kopyalayın.
3.  Projeyi Visual Studio 'da açın ve oluşturun.
4.  Yapı başarılı olduktan sonra, yürütülebilir dosyanın *\Bin\Debug* klasöründe olması gerekir.
5.  ARMClient 'ı çalıştırın. Bir komut istemi açın ve proje kökünden *\Bin\Debug* klasörüne gidin.
6.  Oturum açmak ve kimlik doğrulaması yapmak için komut istemine aşağıdaki komutu girin:<br>
    `ARMClient login prod`
7.  Çıktısından **abonelik GUID 'ini** kopyalayın.
8.  Panonuza bir yetkilendirme belirteci kopyalamak için komut istemine aşağıdaki komutu girin, ancak kopyalanmış abonelik KIMLIĞINI yukarıdaki adımdan kullandığınızdan emin olun: <br>
    `ARMClient token <subscription GUID from previous step>`

    Yukarıdaki adımı tamamladıktan sonra şunları görürsünüz:<br>
    **Belirteç panoya başarıyla kopyalandı.**
9.  Bu öğreticinin sonraki bölümündeki adımlarda kullanılacak belirteci kaydedin.

### <a name="create-the-budget"></a>Bütçeyi oluşturma

Ardından, **Postman** 'ı Azure tüketim REST API 'lerini çağırarak bir bütçe oluşturacak şekilde yapılandıracaksınız. Postman, bir API geliştirme ortamıdır. Ortam ve koleksiyon dosyalarını Postman içine aktaracaksınız. Koleksiyon, Azure tüketim REST API 'Lerini çağıran HTTP isteklerinin gruplandırılmış tanımlarını içerir. Ortam dosyası, koleksiyon tarafından kullanılan değişkenleri içerir.

1.  REST API 'Leri yürütmek için [Postman Rest istemcisini](https://www.getpostman.com/) indirip açın.
2.  Postman 'da yeni bir istek oluşturun.

    ![Postman-yeni bir istek oluştur](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-27.png)

3.  Yeni isteği bir koleksiyon olarak kaydedin, böylece yeni istek üzerinde hiçbir şey yapmaz.

    ![Postman-yeni isteği Kaydet](./media/billing-cost-management-budget-scenario/billing-cost-management-budget-scenario-28.png)

4.  İsteği bir `Get` `Put` eyleme değiştirin.
5.  Bu öğreticinin önceki bölümünde kullandığınız `{subscriptionId}` **abonelik kimliğiyle** değiştirerek aşağıdaki URL 'yi değiştirin. Ayrıca, URL 'YI "Samplebütçe" değerini `{budgetName}`içerecek şekilde değiştirin:`https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2018-03-31`
6.  Postman içindeki **üstbilgiler** sekmesini seçin.
7.  "Authorization" adlı yeni bir **anahtar** ekleyin.
8.  **Değeri** , son bölümün sonundaki ArmClient kullanılarak oluşturulan belirtece ayarlayın.
9.  Postman içinde **gövde** sekmesini seçin.
10. **Ham** düğme seçeneğini belirleyin.
11. Metin kutusunda, aşağıdaki örnek bütçe tanımına yapıştırın, ancak **SubscriptionID**, **budgetname**ve **actiongroupname** parametrelerini abonelik kimliğiniz, bütçeniz için benzersiz bir ad ve eylem grubu adı ile değiştirmelisiniz hem URL hem de istek gövdesinde oluşturduğunuz:

    ```
        {
            "properties": {
                "category": "Cost",
                "amount": 100.00,
                "timeGrain": "Monthly",
                "timePeriod": {
                "startDate": "2018-06-01T00:00:00Z",
                "endDate": "2018-10-31T00:00:00Z"
                },
                "filters": {
                },
            "notifications": {
                "Actual_GreaterThan_80_Percent": {
                    "enabled": true,
                    "operator": "GreaterThan",
                    "threshold": 80,
                    "contactEmails": [
                    ],
                    "contactRoles": [
                    ],
                    "contactGroups": [
                    "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}
                    ]
                },
            "Actual_EqualTo_100_Percent": {
                    "operator": "EqualTo",
                    "threshold": 100,
                    "contactGroups": [
                "/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/microsoft.insights/actionGroups/{actiongroupname}"
                    ]
                }
            }
        }
    ```
12. İsteği göndermek için **Gönder** ' e basın.

Artık, [Bütçe API](https://docs.microsoft.com/rest/api/consumption/budgets)'sini çağırmak için ihtiyacınız olan tüm parçalara sahipsiniz. Bütçeler API başvurusu, aşağıdakiler de dahil olmak üzere belirli istekler hakkında ek ayrıntılara sahiptir:
    - **budgetName** -birden çok bütçe desteklenir.  Bütçe adları benzersiz olmalıdır.
    - **Kategori** - **Maliyet** veya **kullanım**olmalıdır. API hem maliyet hem de kullanım bütçelerini destekler.
    - **zaman dilimi** -aylık, üç aylık veya yıllık bütçe. Süre sonunda miktar sıfırlanır.
    - **Filtreler** -filtreler, bütçeyi seçili kapsam içindeki belirli bir kaynak kümesiyle daraltmanıza olanak tanır. Örneğin, bir filtre, abonelik düzeyindeki bir bütçe için kaynak grupları koleksiyonu olabilir.
    - **Bildirimler** – bildirim ayrıntılarını ve eşiklerini belirler. Birden çok eşik ayarlayabilir ve bildirim almak için bir e-posta adresi veya bir eylem grubu sağlayabilirsiniz.

## <a name="summary"></a>Özet

Bu öğreticiyi izleyerek şunları öğrendiniz:
- VM 'Leri durdurmak için bir Azure Otomasyonu runbook 'U oluşturma.
- Bütçe eşik değerlerine göre tetiklenen bir Azure mantıksal uygulaması oluşturma ve ilgili runbook 'u doğru parametrelerle çağırma.
- Bütçe eşiği karşılandığında Azure Logic App 'i tetiklemek için yapılandırılmış bir Azure Izleyici eylem grubu oluşturma.
- Azure bütçesini istenen eşiklerle oluşturun ve eylem grubuna bağlayın.

Artık aboneliğiniz için, yapılandırılmış bütçe eşiklerinize ulaştığınızda sanal makinelerinizi kapatacak tam işlevli bir bütçeniz vardır.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Faturalandırma senaryoları hakkında daha fazla bilgi için bkz. [faturalandırma ve maliyet yönetimi otomasyon senaryoları](billing-cost-management-automation-scenarios.md).
