---
title: Sanal makine değişikliklerini izleyin - Azure Event Grid & Logic Apps
description: Azure Olay Izgara ve Mantık Uygulamalarını kullanarak sanal makinelerde (Sanal Makinelerde) değişiklikleri denetleyin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.date: 10/11/2019
ms.openlocfilehash: f5aac7fe63b2afc997ff69e5d976c755440c1bea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75982562"
---
# <a name="tutorial-monitor-virtual-machine-changes-by-using-azure-event-grid-and-logic-apps"></a>Öğretici: Azure Olay Izgara ve Mantık Uygulamalarını kullanarak sanal makine değişikliklerini izleyin

Azure kaynaklarında veya üçüncü taraf kaynaklarında meydana gelen belirli olayları izlemek ve yanıtlamak için, en az kod kullanan bir [mantık uygulaması](../logic-apps/logic-apps-overview.md) oluşturarak görevleri iş akışı olarak otomatikleştirebilir ve çalıştırabilirsiniz. Bu kaynaklar, olayları [Bir Azure olay tablosunda](../event-grid/overview.md)yayımlayabilir. Olay kılavuzu da bu olayları uç nokta olarak kuyruk, web kancası veya [olay hub’ları](../event-hubs/event-hubs-what-is-event-hubs.md) olan abonelere gönderir. Bir abone olarak, mantık uygulamanız görevleri gerçekleştirmek için otomatik iş akışları çalıştırmadan önce olay ızgarasından bu olayları bekleyebilir.

Örneğin, yayımcıların Azure Event Grid hizmeti üzerinden abonelere gönderebileceği bazı olaylar şunlardır:

* Kaynağı oluşturun, okuyun, güncelleştirin veya silin. Örneğin, Azure aboneliğinizde ücretlendirmeye neden olabilecek ve faturanızı etkileyebilecek değişiklikleri izleyebilirsiniz.

* Bir Azure aboneliğine kişi ekleyin veya kaldırın.

* Uygulamanız belirli bir eylemi gerçekleştirir.

* Bir kuyrukta yeni bir ileti görüntülenir.

Bu öğretici, sanal bir makinedeki değişiklikleri izleyen ve bu değişikliklerle ilgili e-postalar gönderen bir mantık uygulaması oluşturur. Bir Azure kaynağı için bir mantıksal uygulama oluşturduğunuzda, bu kaynaktan olayların bir mantık kılavuzu üzerinden mantıksal uygulamaya akışı yapılır. Öğretici size bu mantıksal uygulamayı oluşturma sürecinde yardımcı olur:

![Genel bakış - Olay kılavuzu ve mantıksal uygulama ile sanal makineyi izleme](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir olay kılavuzundan olayları izleyen bir mantıksal uygulama oluşturun.
> * Özellikle sanal makine değişikliklerini izleyen bir koşul ekleyin.
> * Sanal makineniz değiştiğinde e-posta gönderin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Office 365 Outlook, Outlook.com veya Gmail gibi bildirimler göndermek için Logic Apps tarafından desteklenen bir e-posta sağlayıcısından gelen e-posta hesabı. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](/connectors/).

  Bu öğretici, bir Office 365 Outlook hesabı kullanır. Farklı bir e-posta hesabı kullanırsanız genel adımlar aynı kalır, ancak kullanıcı arabiriminiz biraz farklı görünebilir.

* Kendi Azure kaynak grubunda tek başına olan [sanal bir makine.](https://azure.microsoft.com/services/virtual-machines) Bunu zaten yapmadıysanız, [VM öğretici satin](../virtual-machines/windows/quick-create-portal.md)alinyoluyla sanal bir makine oluşturun. Sanal makinenin olayları yayımlaması için, [başka bir işlem yapmanız gerekmez](../event-grid/overview.md).

## <a name="create-blank-logic-app"></a>Boş mantıksal uygulama oluşturma

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Ana Azure menüsünden **kaynak** > **Tümleştirme** > **Mantığı Uygulaması**oluştur'u seçin.

   ![Mantıksal uygulama oluşturma](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

1. **Mantık Uygulaması**altında, mantık uygulaması kaynağı hakkında bilgi sağlar. İşiniz bittiğinde **Oluştur**’u seçin.

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Adı** | Evet | <*mantık-uygulama adı*> | Mantık uygulamanız için benzersiz bir ad sağlayın. |
   | **Abonelik** | Evet | <*Azure abonelik adı*> | Bu öğreticideki tüm hizmetler için aynı Azure aboneliğini seçin. |
   | **Kaynak grubu** | Evet | <*Azure kaynak grubu*> | Bu öğreticideki tüm hizmetler için seçebileceğiniz mantık uygulamanızın Azure kaynak grubu adı. |
   | **Konum** | Evet | <*Azure bölgesi*> | Bu öğreticideki tüm hizmetler için aynı bölgeyi seçin. |
   |||

1. Azure mantık uygulamanızı dağıttıktan sonra, Logic Apps Tasarımcısı giriş videosu ve yaygın olarak kullanılan tetikleyicileri içeren bir sayfa gösterir. Video ve tetikleyicileri kaydırın.

1. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   ![Mantık uygulaması şablonu seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Logic Apps Designer artık mantık uygulamanızı başlatmak için kullanabileceğiniz [*tetikleyicileri*](../logic-apps/logic-apps-overview.md#logic-app-concepts) gösterir. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her çalıştığında, Azure Logic Apps mantık uygulamanızı çalıştıran bir iş akışı örneği oluşturur.

## <a name="add-an-event-grid-trigger"></a>Olay Izgara tetikleyicisi ekleme

Şimdi, sanal makinenizin kaynak grubunu izlemek için kullandığınız Olay Izgara tetikleyicisini ekleyin.

1. Tasarımcıda, arama kutusuna filtreniz olarak girin. `event grid` Tetikleyiciler listesinden kaynak olayı ne **zaman tetikle'yi** seçin.

   ![Şu tetikleyiciyi seçin: "Kaynak olayında"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

1. İstendiğinde, Azure hesap kimlik bilgilerinizle Azure Etkinlik Ağıt'ında oturum açın. Azure aboneliğinizle ilişkili Azure Etkin Dizin kiracısını gösteren **Kiracı** listesinde, örneğin doğru kiracının görünün:

   ![Azure kimlik bilgilerinizle oturum açın](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > @outlook.com veya @hotmail.com gibi kişisel bir Microsoft hesabında oturum açtıysanız, Event Grid tetikleyicisi doğru görüntülenmeyebilir. Geçici çözüm olarak, [Hizmet Sorumlusuyla Bağlan'ı](../active-directory/develop/howto-create-service-principal-portal.md)seçin veya Azure aboneliğiniz (örneğin kullanıcı *adı)*@emailoutlook.onmicrosoft.comile ilişkili Azure Etkin Dizininin bir üyesi olarak kimliğinizi doğrulayın.

1. Şimdi yayıncıdan gelen olaylariçin mantık uygulamaabone. Etkinlik aboneliğiniz hakkındaki ayrıntıları aşağıdaki tabloda açıklandığı gibi sağlayın, örneğin:

   ![Olay aboneliğinin ayrıntılarını sağlayın](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

   | Özellik | Gerekli | Değer | Açıklama |
   | -------- | -------- | ----- | ----------- |
   | **Abonelik** | Evet | <*etkinlik-yayımcı-Azure-abonelik adı*> | *Etkinlik yayımcısıyla*ilişkili Azure aboneliğinin adını seçin. Bu öğretici için, sanal makineniz için Azure abonelik adını seçin. |
   | **Kaynak Türü** | Evet | <*etkinlik-yayımcı-Azure kaynak türü*> | Etkinlik yayımcısı için Azure kaynak türünü seçin. Azure kaynak türleri hakkında daha fazla bilgi için [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md)hakkında bkz. Bu öğretici için `Microsoft.Resources.ResourceGroups` Azure kaynak gruplarını izlemek için değeri seçin. |
   | **Kaynak Adı** |  Evet | <*etkinlik-yayımcı-Azure-kaynak adı*> | Etkinlik yayımcısı için Azure kaynak adını seçin. Bu liste seçtiğiniz kaynak türüne göre değişir. Bu öğretici için, sanal makinenizi içeren Azure kaynak grubunun adını seçin. |
   | **Olay Türü Öğesi** |  Hayır | <*olay türleri*> | Filtrelemek ve olay ızgaranıza göndermek için bir veya daha fazla özel olay türü seçin. Örneğin, kaynakların ne zaman değiştirildiğini veya silindiğini algılamak için isteğe bağlı olarak bu olay türlerini ekleyebilirsiniz: <p><p>- `Microsoft.Resources.ResourceActionSuccess` <br>- `Microsoft.Resources.ResourceDeleteSuccess` <br>- `Microsoft.Resources.ResourceWriteSuccess` <p>Daha fazla bilgi için şu konulara bakın: <p><p>- [Kaynak grupları için Azure Olay Izgara olay şeması](../event-grid/event-schema-resource-groups.md) <br>- [Olay filtrelemeyi anlama](../event-grid/event-filtering.md) <br>- [Olay Izgarası için filtre olayları](../event-grid/how-to-filter-events.md) |
   | İsteğe bağlı özellikler eklemek için **yeni parametre ekle'yi**seçin ve ardından istediğiniz özellikleri seçin. | Hayır | {açıklamalara bakın} | * **Önek Filtresi**: Bu öğretici için bu özelliği boş bırakın. Varsayılan davranış tüm değerlerle eşleşir. Ancak filtre olarak bir ön ek dizesi (örneğin belirli bir kaynak için bir yol ve bir parametre) belirtebilirsiniz. <p>* **Sonek Filtresi**: Bu öğretici için bu özelliği boş bırakın. Varsayılan davranış tüm değerlerle eşleşir. Ancak yalnızca belirli dosya türlerini istediğinizde filtre olarak bir sonek dizesi (örneğin dosya adı uzantısı) belirtebilirsiniz. <p>* **Abonelik Adı**: Bu öğretici için etkinlik aboneliğiniz için benzersiz bir ad verebilirsiniz. |
   |||

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin. Bir eylemin ayrıntılarını mantık uygulamanızda daraltmak ve gizlemek için eylemin başlık çubuğunu seçin.

   ![Mantıksal uygulamanızı kaydetme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Mantıksal uygulamanızı bir olay kılavuzu tetikleyicisiyle kaydettiğinizde, Azure seçtiğiniz kaynağa mantıksal uygulamanız için otomatik olarak bir olay aboneliği oluşturur. Bu nedenle kaynak bir olayı olay kılavuzuna yayımladığında, bu olay kılavuzu otomatik olarak olayı mantıksal uygulamanıza gönderir. Bu olay mantıksal uygulamanızı tetikler ve ardından sonraki adımlarda yapılandıracağınız iş akışının bir örneğini oluşturur ve çalıştırır.

Mantıksal uygulamanız artık canlı ve olay kılavuzundan olayları dinliyor ancak siz eylemleri iş akışına ekleyene kadar herhangi bir işlem yapmayacak.

## <a name="add-a-condition"></a>Koşul ekleme

Mantık uygulamanızın yalnızca belirli bir olay veya işlem gerçekleştiğinde çalışmasını istiyorsanız, `Microsoft.Compute/virtualMachines/write` işlemi denetleyen bir koşul ekleyin. Bu koşul true olduğunda, mantıksal uygulamanız size güncelleştirilen sanal makine hakkında ayrıntıları içeren bir e-posta gönderir.

1. Mantık Uygulama Tasarımcısı'nda, olay ızgara tetikleyicisi altında **Yeni adım'ı**seçin.

   !["Yeni adım" seçeneğini belirleyin](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-new-step-condition.png)

1. Bir **eylem seçin**altında, arama `condition` kutusuna filtreniz olarak girin. Eylemler listesinden **Durum** eylemini seçin.

   ![Koşul ekleme](./media/monitor-virtual-machine-changes-event-grid-logic-app/select-condition.png)

   Logic App Tasarımcısı iş akışınıza koşulun true veya false olmasına bağlı olarak izlenecek eylem yolları dahil boş bir koşul ekler.

   ![Boş bir durum görüntülenir](./media/monitor-virtual-machine-changes-event-grid-logic-app/empty-condition.png)

1. Koşul başlığını ' `If a virtual machine in your resource group has changed`olarak yeniden adlandırın Koşulun başlık çubuğunda elips (**...**) düğmesini seçin ve **Yeniden Adlandır'ı**seçin.

   ![Koşulu yeniden adlandırın](./media/monitor-virtual-machine-changes-event-grid-logic-app/rename-condition.png)

1. Özelliğin `Microsoft.Compute/virtualMachines/write` işlemle eşit `body` olduğu `data` bir nesne için olayı denetleyen bir koşul oluşturun. `operationName` [Event Grid olay şeması](../event-grid/event-schema.md) hakkında daha fazla bilgi edinin.

   1. İlk satırda **Ve** başlığının altındaki sol kutunun içine tıklayın. Görünen dinamik içerik listesinde **İfade'yi**seçin.

      ![İfade düzenleyicisini açmak için "İfade" seçeneğini belirleyin](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-choose-expression.png)

   1. İfade düzenleyicisinde, işlem adını tetikleyiciden döndüren bu ifadeyi girin ve **Tamam'ı**seçin:

      `triggerBody()?['data']['operationName']`

      Örnek:

      ![İşlem adını ayıklamak için ifade girin](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-add-data-operation-name.png)

   1. Ortadaki kutuda **eşittir** işlecini tutun.

   1. Sağ kutuya, izlemeniz gereken özel işlem olan bu değeri girin:

      `Microsoft.Compute/virtualMachines/write`

   Bitmiş durumunuz şimdi şu örnek gibi görünüyor:

   ![İşlemi karşılaştıran tamamlanmış koşul](./media/monitor-virtual-machine-changes-event-grid-logic-app/complete-condition.png)

   Tasarım görünümünden kod görünümüne ve tasarım görünümüne geçerseniz, koşulda belirttiğiniz ifade **data.operationName** belirteciyle çözülür:

   ![Koşulda çözülmüş belirteçler](./media/monitor-virtual-machine-changes-event-grid-logic-app/resolved-condition.png)

1. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-notifications"></a>E-posta bildirimleri gönderme

Şimdi, belirtilen koşul doğru olduğunda bir e-posta alabilmeniz için bir [*eylem*](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. **If true** box koşulunda, **eylem ekle'yi**seçin.

   ![Koşul true olduğunda kullanılacak eylemi ekleyin](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-true-add-action.png)

1. Bir **eylem seçin**altında, arama `send an email` kutusuna filtreniz olarak girin. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Ardından bağlayıcı için "e-posta gönder" eylemini seçin. Örnek:

   * Azure iş veya okul hesabı için Office 365 Outlook bağlayıcısını seçin.

   * Kişisel Microsoft hesapları için Outlook.com bağlayıcısını seçin.

   * Gmail hesapları için Gmail bağlayıcısını seçin.

   Bu öğretici, Office 365 Outlook bağlayıcısıyla devam ediyor. Farklı bir sağlayıcı kullanıyorsanız, adımlar aynı kalır, ancak uI'niz biraz farklı görünebilir.

   !["E-posta gönder" eylemini seçin](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

1. E-posta sağlayıcınız için zaten bir bağlantınız yoksa, kimliğinizi doğrulamanız istendiğinde e-posta hesabınızda oturum açın.

1. Bu başlık için e-posta gönder eylem yeniden adlarını:`Send email when virtual machine updated`

1. Aşağıdaki tabloda belirtildiği gibi e-posta hakkında bilgi sağlayın:

   ![E-posta eylemi hakkında bilgi sağlama](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > İş akışınızdaki önceki adımlardan çıktı seçmek için, dinamik içerik listesinin görünmesi için bir edit kutusunun içini tıklatın veya **dinamik içerik ekle'yi**seçin. Daha fazla sonuç için, listedeki her bölüm için **daha fazlasını görün'u** seçin. Dinamik içerik listesini kapatmak için dinamik içeriği yeniden **ekle'yi** seçin.

   | Özellik | Gerekli | Değer | Açıklama |
   | -------- | -------- | ----- | ----------- |
   | **Hedef** | Evet | <*alıcı\@etki alanı*> | Alıcının e-posta adresi girin. Test için kendi e-posta adresinizi kullanabilirsiniz. |
   | **Konu** | Evet | `Resource updated:` **Konu** | E-posta konusunun içeriğini girin. Bu öğretici için belirtilen metni girin ve etkinliğin **Konu** alanını seçin. Burada, e-postanızın konusu güncelleştirilen kaynağın (sanal makine) adını içerir. |
   | **Gövde** | Evet | `Resource:` **Konu başlığı** <p>`Event type:` **Olay Türü**<p>`Event ID:`**KIMLIK**<p>`Time:`**Etkinlik Zamanı** | E-posta gövdesinin içeriğini girin. Bu öğretici için belirtilen metni girin ve e-postanızın olayı, olay türünü, olay zaman damgasını ve güncelleştirme için olay kimliğini ateşleyen kaynağı içermek için etkinliğin **Konusu,** **Etkinlik Türü,** **Kimliği**ve **Etkinlik Zamanı** alanlarını seçin. Bu öğretici için kaynak, tetikleyicide seçilen Azure kaynak grubudur. <p>İçeriğinize boş satır eklemek için Shift + Enter tuşlarını kullanın. |
   ||||

   > [!NOTE]
   > Bir diziyi temsil eden bir alan seçerseniz, tasarımcı eyleme otomatik olarak diziye başvuran bir **For each** döngüsü ekler. Bu şekilde mantıksal uygulamanız ilgili eylemi dizideki tüm öğeler için gerçekleştirir.

   Şimdi, e-posta eyleminiz bu örnekteki gibi görünebilir:

   ![E-postaya eklenecek çıkışları seçme](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Tamamlanmış mantıksal uygulamanız örnekteki gibi görünebilir:

   ![Tamamlanmış mantıksal uygulama](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

1. Mantıksal uygulamanızı kaydedin. Her eylemin ayrıntılarını mantık uygulamanızda daraltmak ve gizlemek için eylemin başlık çubuğunu seçin.

   Mantıksal uygulamanız artık canlıdır ancak herhangi bir işlem gerçekleştirmeden önce sanal makinenizde yapılan değişiklikleri bekler. Mantıksal uygulamanızı şimdi test etmek için sonraki bölüme geçin.

## <a name="test-your-logic-app-workflow"></a>Mantıksal uygulama iş akışınızı test etme

1. Mantıksal uygulamanızın belirtilen olayları alıp almadığını denetlemek için, sanal makinenizi güncelleştirin.

   Örneğin, Azure portalında sanal makinenizi yeniden boyutlandırabilir veya [VM’nizi Azure PowerShell ile yeniden boyutlandırabilirsiniz](../virtual-machines/windows/resize-vm.md).

   Birkaç dakika sonra bir e-posta almanız gerekir. Örnek:

   ![Sanal makine güncelleştirmesi hakkında e-posta](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

1. Mantık uygulamanızın koşularını incelemek ve mantık uygulamanızın geçmişini tetiklemek için, mantık uygulaması menüsünde **Genel Bakış'ı**seçin. Çalışma yla ilgili daha fazla ayrıntıyı görüntülemek için, bu çalıştırma için satırı seçin.

   ![Mantıksal uygulama çalıştırma geçmişi](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

1. Her bir adımın giriş ve çıkışlarını görüntülemek için gözden geçirmek istediğiniz adımı genişletin. Bu bilgiler mantıksal uygulamanızdaki sorunları tespit etmenize ve gidermenize yardımcı olabilir.

   ![Mantıksal uygulama çalıştırma geçmişi ayrıntıları](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Tebrikler, bir olay kılavuzuyla kaynak olaylarını izleyen ve bu olaylar gerçekleştiğinde size e-posta gönderen bir mantıksal uygulama oluşturdunuz. Ayrıca, süreçleri otomatik hale getiren iş akışlarını ne kadar kolay oluşturabileceğinizi ve sistemler ile bulut hizmetlerini tümleştirmeyi öğrendiniz.

Olay kılavuzları ve mantıksal uygulamalarla diğer yapılandırma değişikliklerini izleyebilirsiniz, örneğin:

* Bir sanal makine rol tabanlı erişim denetimi (RBAC) haklarını alır.
* Değişiklikler bir ağ arabirimi (NIC) üzerindeki bir ağ güvenlik grubunda (NSG) yapılır.
* Bir sanal makine için diskler eklenir veya kaldırılır.
* Bir sanal makine NIC’sine genel bir IP adresi atanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmakta ve eylemler gerçekleştirilmektedir. Bu nedenle öğreticiyi ve testlerinizi tamamladıktan sonra ücret uygulanmasını istemediğiniz kaynakları devre dışı bırakmayı veya silmeyi unutmayın.

* Çalışmanızı silmeden mantıksal uygulamanızı durdurmak için uygulamanızı devre dışı bırakın. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin. Araç çubuğunda **Devre Dışı'** yı seçin.

  ![Mantıksal uygulamanızı kapatma](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

* Mantık uygulamanızı kalıcı olarak silmek için, mantık uygulaması menüsünde **Genel Bakış'ı**seçin. Araç çubuğunda **Sil'i**seçin. Mantık uygulamanızı silmek istediğinizi onaylayın ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Grid ile özel olaylar oluşturma ve yönlendirme](../event-grid/custom-event-quickstart.md)
