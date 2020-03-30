---
title: Azure Monitör'ü kullanarak B2B iletilerini izleme
description: Azure Monitor günlüklerini ayarlayarak ve Azure Logic Apps için tanılama verileri toplayarak AS2, X12 ve EDIFACT iletilerini sorun giderme
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: e9ba5a516293eb72a715dc9d0df7db4d5a4ea3c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76907987"
---
# <a name="set-up-azure-monitor-logs-and-collect-diagnostics-data-for-b2b-messages-in-azure-logic-apps"></a>Azure Logic Apps'ta Azure Monitor günlüklerini ayarlayın ve B2B iletileri için tanılama verileri toplayın

Entegrasyon hesabınızda ticaret ortakları arasında B2B iletişimini kurduktan sonra, bu ortaklar AS2, X12 ve EDIFACT gibi protokolleri kullanarak mesaj alışverişinde bulunabilirler. Bu iletişimin beklediğiniz gibi çalışıp çalışmadığını kontrol etmek için, tümleştirme hesabınız için [Azure Monitor günlükleri](../azure-monitor/platform/data-platform-logs.md) ayarlayabilirsiniz. [Azure Monitör,](../azure-monitor/overview.md) kullanılabilirliklerini ve performanslarını daha kolay koruyabilmeniz için bulut ve şirket içi ortamlarınızı izlemenize yardımcı olur. Azure Monitor günlüklerini kullanarak, bir [Günlük Analizi çalışma alanında](../azure-monitor/platform/resource-logs-collect-workspace.md)tetikleyici olaylar, olayları çalıştırma ve eylem olayları gibi çalışma zamanı verileri ve olaylar la ilgili verileri kaydedebilir ve depolayabilirsiniz. İletiler için, günlük bilgileri de toplar:

* İleti sayısı ve durumu
* Bildirimler durumu
* İletiler ve bildirimler arasındaki korelasyonlar
* Hatalar için ayrıntılı hata açıklamaları

Azure Monitor, bu bilgileri bulmanıza ve gözden geçirmenize yardımcı olmak için [günlük sorguları](../azure-monitor/log-query/log-query-overview.md) oluşturmanıza olanak tanır. Bu tanılama verilerini Azure Depolama ve Azure Etkinlik Hub'ları gibi diğer Azure hizmetleriyle de [kullanabilirsiniz.](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data)

Entegrasyon hesabınız için günlük oluşturmayı ayarlamak için [Azure portalına Logic Apps B2B çözümlerini yükleyin.](#install-b2b-solution) Bu çözüm, B2B ileti olayları için toplu bilgi sağlar. Ardından, bu bilgiler için günlüğe kaydetme ve sorgu oluşturmayı etkinleştirmek için [Azure Monitor günlüklerini](#set-up-resource-logs)ayarlayın.

Bu makalede, tümleştirme hesabınız için Azure Monitor günlüğe kaydetmenasıl etkinleştirilir gösterilmektedir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

* Log Analytics çalışma alanı. Log Analytics çalışma alanınız yoksa, [Log Analytics çalışma alanı oluşturmayı](../azure-monitor/learn/quick-create-workspace.md)öğrenin.

* Azure Monitor günlüğe kaydetme ile ayarlanmış ve bu bilgileri bir Log Analytics çalışma alanına gönderen bir mantık uygulaması. [Mantık uygulamanız için Azure Monitor günlüklerini nasıl ayarlayizleyeceğinizi](../logic-apps/monitor-logic-apps.md)öğrenin.

* Mantık uygulamanıza bağlı bir entegrasyon hesabı. [Entegrasyon hesabınızı mantık uygulamanıza nasıl bağlayacaklarınıöğrenin.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

<a name="install-b2b-solution"></a>

## <a name="install-logic-apps-b2b-solution"></a>Logic Apps B2B çözümyükleme

Azure Monitor günlükleri mantık uygulamanız için B2B mesajlarını izlemeden önce Log Analytics çalışma alanınıza **Logic Apps B2B** çözümünü ekleyin.

1. Azure [portalının](https://portal.azure.com)arama kutusuna `log analytics workspaces`girin ve ardından **Günlük Analizi çalışma alanlarını**seçin.

   !["Günlük Analitiği çalışma alanlarını" seçin](./media/monitor-b2b-messages-log-analytics/find-select-log-analytics-workspaces.png)

1. **Günlük Analizi çalışma alanları**altında çalışma alanınızı seçin.

   ![Günlük Analizi çalışma alanınızı seçin](./media/monitor-b2b-messages-log-analytics/select-log-analytics-workspace.png)

1. Genel Bakış bölmesinde, **Log Analytics** > ile başla nın altında**izleme çözümlerini yapılandırın,** **Çözümleri Görüntüle'yi**seçin.

   ![Genel Bakış bölmesinde "Çözümleri görüntüle" seçeneğini belirleyin](./media/monitor-b2b-messages-log-analytics/log-analytics-workspace.png)

1. Genel Bakış bölmesine **Ekle'yi**seçin.

   ![Genel bakış bölmesine yeni çözüm ekleyin](./media/monitor-b2b-messages-log-analytics/add-logic-apps-management-solution.png)

1. **Market** açıldıktan sonra, arama kutusuna girve `logic apps b2b`Mantık Uygulamaları **B2B'yi**seçin.

   ![Market'ten "Logic Apps Management" seçeneğini belirleyin](./media/monitor-b2b-messages-log-analytics/select-logic-apps-b2b-solution.png)

1. Çözüm açıklama bölmesine, **Oluştur'u**seçin.

   !["Logic Apps B2B" çözümü eklemek için "Oluştur" seçeneğini belirleyin](./media/monitor-b2b-messages-log-analytics/create-logic-apps-b2b-solution.png)

1. Çözümü yüklemek istediğiniz Log Analytics çalışma alanını gözden geçirin ve onaylayın ve yeniden **Oluştur'u** seçin.

   !["Mantık Uygulamaları B2B" için "Oluştur"u seçin](./media/monitor-b2b-messages-log-analytics/confirm-log-analytics-workspace.png)

   Azure, çözümü Günlük Analizi çalışma alanınızı içeren Azure kaynak grubuna dağıttıktan sonra, çözüm çalışma alanınızın özet bölmesinde görünür. B2B iletileri işlendiğinde, bu bölmedeki ileti sayısı güncelleştirilir.

   ![Çalışma alanı özet bölmesi](./media/monitor-b2b-messages-log-analytics/b2b-overview-messages-summary.png)

<a name="set-up-resource-logs"></a>

## <a name="set-up-azure-monitor-logs"></a>Azure Monitör günlüklerini ayarlama

Azure Monitor günlüğe kaydetmeyi doğrudan entegrasyon hesabınızdan etkinleştirebilirsiniz.

1. Azure [portalında](https://portal.azure.com)entegrasyon hesabınızı bulun ve seçin.

   ![Entegrasyon hesabınızı bulun ve seçin](./media/monitor-b2b-messages-log-analytics/find-integration-account.png)

1. Entegrasyon hesabınızın menüsünde, **İzleme**altında **Tanılama ayarlarını**seçin. **Tanıayar ayarı ekle'yi**seçin.

   !["İzleme" altında "Tanılama ayarları" seçeneğini belirleyin](./media/monitor-b2b-messages-log-analytics/monitor-diagnostics-settings.png)

1. Ayarı oluşturmak için aşağıdaki adımları izleyin:

   1. Ayar için bir ad sağlayın.

   1. **Günlük Analitiğine Gönder'i**seçin.

   1. **Abonelik**için, Günlük Analizi çalışma alanınızla ilişkili Azure aboneliğini seçin.

   1. **Günlük Analizi Çalışma Alanı**için, kullanmak istediğiniz çalışma alanını seçin.

   1. **Günlük**altında, kaydetmek istediğiniz etkinlik kategorisini belirten **IntegrationAccountTrackingEvents** kategorisini seçin.

   1. İşiniz bittiğinde **Kaydet**'i seçin.

   Örnek: 

   ![Tanılama verilerini toplamak için Azure Monitor günlüklerini ayarlama](./media/monitor-b2b-messages-log-analytics/send-diagnostics-data-log-analytics-workspace.png)

<a name="view-message-status"></a>

## <a name="view-message-status"></a>İleti durumunu görüntüleme

Mantık uygulamanız çalıştırdıktan sonra, Log Analytics çalışma alanınızda bu iletilerle ilgili durumu ve verileri görüntüleyebilirsiniz.

1. Azure [portalı](https://portal.azure.com) arama kutusunda, Log Analytics çalışma alanınızı bulun ve açın.

1. Çalışma alanınızın menüsünde, **Çalışma Alanı özeti** > **Logic Apps B2B'yi**seçin.

   ![Çalışma alanı özet bölmesi](./media/monitor-b2b-messages-log-analytics/b2b-overview-messages-summary.png)

   > [!NOTE]
   > Logic Apps B2B döşemesi bir çalıştırmadan sonra sonuçları hemen göstermiyorsa, Yeniden denemeden önce **Yenile'yi** seçmeyi deneyin veya kısa bir süre bekleyin.

   Varsayılan olarak, **Logic Apps B2B** döşemesi verileri tek bir güne göre gösterir. Veri kapsamını farklı bir aralıkla değiştirmek için sayfanın üst kısmındaki kapsam denetimini seçin:

   ![Değiştirme aralığı](./media/monitor-b2b-messages-log-analytics/change-summary-interval.png)

1. İleti durumu panosu göründükten sonra, belirli bir ileti türü için verileri tek bir güne göre gösteren daha fazla ayrıntı görüntüleyebilirsiniz. **AS2**, **X12**veya **EDIFACT**için döşemeyi seçin.

   ![İletilerin durumlarını görüntüleme](./media/monitor-b2b-messages-log-analytics/workspace-summary-b2b-messages.png)

   Seçtiğiniz döşeme için iletilistesi görüntülenir. Örneğin, bir AS2 ileti listesi şu şekilde görünebilir:

   ![AS2 iletilerinin durumları ve ayrıntıları](./media/monitor-b2b-messages-log-analytics/as2-message-results-list.png)

   Her ileti türüözellikleri hakkında daha fazla bilgi edinmek için aşağıdaki ileti özelliği açıklamalarına bakın:

   * [AS2 ileti özellikleri](#as2-message-properties)
   * [X12 ileti özellikleri](#x12-message-properties)
   * [EDIFACT ileti özellikleri](#EDIFACT-message-properties)

<!--
1. To view or export the inputs and outputs for specific messages, select those messages, and select **Download**. When you're prompted, save the .zip file to your local computer, and then extract that file.

   The extracted folder includes a folder for each selected message. If you set up acknowledgements, the message folder also includes files with acknowledgement details. Each message folder has at least these files:
   
   * Human-readable files with the input payload and output payload details
   * Encoded files with the inputs and outputs

   For each message type, you can find the folder and file name formats here:

   * [AS2 folder and file name formats](#as2-folder-file-names)
   * [X12 folder and file name formats](#x12-folder-file-names)
   * [EDIFACT folder and file name formats](#edifact-folder-file-names)

   ![Download message files](./media/monitor-b2b-messages-log-analytics/download-messages.png)

1. To view all actions that have the same run ID, on the **Log Search** page, select a message from the message list.

   You can sort these actions by column, or search for specific results.

   * To search results with prebuilt queries, select **Favorites**.

   * Learn [how to build queries by adding filters](../logic-apps/create-monitoring-tracking-queries.md). Or learn more about [how to find data with log searches in Azure Monitor logs](../log-analytics/log-analytics-log-searches.md).

   * To change query in the search box, update the query with the columns and values that you want to use as filters.
-->

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>AS2, X12 ve EDIFACT iletileri için özellik açıklamaları ve ad biçimleri

Her ileti türü için, indirilen ileti dosyalarının özellik açıklamaları ve ad biçimleri aşağıda veda edilir.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>AS2 ileti özelliği açıklamaları

Burada her AS2 iletisi için özellik açıklamaları ver.

| Özellik | Açıklama |
|----------|-------------|
| **Gönderen** | **Alma Ayarları'nda**belirtilen konuk ortak veya AS2 sözleşmesi için **Ayarlar Gönder'de** belirtilen ana bilgisayar ortağı |
| **Alıcı** | **Alma Ayarları'nda**belirtilen ana bilgisayar ortağı veya AS2 sözleşmesi için **Ayarlar Gönder'de** belirtilen konuk ortak |
| **Logic App** | AS2 eylemlerinin ayarlandığı mantık uygulaması |
| **Durum** | AS2 ileti durumu <br>Başarı = Geçerli bir AS2 iletisi aldı veya gönderdi. MDN ayarlı değil. <br>Başarı = Geçerli bir AS2 iletisi aldı veya gönderdi. MDN ayarlanır ve alınır veya MDN gönderilir. <br>Başarısız = Geçersiz bir AS2 iletisi alındı. MDN ayarlı değil. <br>Beklemede = Geçerli bir AS2 iletisi alındı veya gönderildi. MDN ayarlanır ve MDN bekleniyor. |
| **Ack** | MDN ileti durumu <br>Kabul = Pozitif bir MDN aldı veya gönderdi. <br>Beklemede = MDN almayı veya göndermeyi bekliyor. <br>Reddedilen = Negatif bir MDN aldı veya gönderdi. <br>Gerekli Değildir = MDN sözleşmede ayarlanmaz. |
| **Yön** | AS2 ileti yönü |
| **İzleme Kimliği** | Bir mantık uygulamasındaki tüm tetikleyicileri ve eylemleri ilişkilendiren kimlik |
| **İleti Kimliği** | AS2 ileti üstbilgilerinden AS2 ileti kimliği |
| **Zaman damgası** | AS2 eyleminin iletiyi işlediği saat |
|||

<!--
<a name="as2-folder-file-names"></a>

### AS2 name formats for downloaded message files

Here are the name formats for each downloaded AS2 message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_outputs.txt |
|||
-->

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 ileti özelliği açıklamaları

Burada her X12 iletisi için özellik açıklamaları ver.

| Özellik | Açıklama |
|----------|-------------|
| **Gönderen** | **Alma Ayarları'nda**belirtilen konuk ortak veya X12 anlaşması için **Ayarlar Gönder'de** belirtilen ana bilgisayar ortağı |
| **Alıcı** | **Alma Ayarları'nda**belirtilen ana bilgisayar ortağı veya X12 sözleşmesi için **Ayarlar Gönder'de** belirtilen konuk ortak |
| **Logic App** | X12 eylemlerinin ayarlandığı mantık uygulaması |
| **Durum** | X12 ileti durumu <br>Başarı = Geçerli bir X12 iletisi aldı veya gönderdi. İşlevsel bir ack ayarlı değil. <br>Başarı = Geçerli bir X12 iletisi aldı veya gönderdi. Fonksiyonel ack ayarlanır ve alınır veya işlevsel bir ack gönderilir. <br>Başarısız = Geçersiz bir X12 iletisi alındı veya gönderildi. <br>Bekleyen = Geçerli bir X12 iletisi alındı veya gönderildi. Fonksiyonel ack ayarlanır ve fonksiyonel bir ack bekleniyor. |
| **Ack** | Fonksiyonel Ack (997) durumu <br>Kabul = Alınan veya olumlu bir fonksiyonel ack gönderdi. <br>Reddedilen = Alınan veya gönderilen negatif işlevsel ack. <br>Bekleyen = Fonksiyonel bir ack bekliyor ama alınmadı. <br>Bekleyen = İşlevsel bir ack oluşturuldu, ancak iş ortağına gönderemiyorum. <br>Not Required = Fonksiyonel ack kurulmadı. |
| **Yön** | X12 ileti yönü |
| **İzleme Kimliği** | Bir mantık uygulamasındaki tüm tetikleyicileri ve eylemleri ilişkilendiren kimlik |
| **Msg Tipi** | EDI X12 ileti türü |
| **ıcn** | X12 iletisinin Değişim Kontrol Numarası |
| **TSCN** | X12 iletisinin Hareket Kümesi Kontrol Numarası |
| **Zaman damgası** | X12 eyleminin iletiyi işlediği saat |
|||

<!--
<a name="x12-folder-file-names"></a>

### X12 name formats for downloaded message files

Here are the name formats for each downloaded X12 message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_outputs.txt |
|||
-->

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT ileti özelliği açıklamaları

Burada her EDIFACT iletisi için özellik açıklamaları vardır.

| Özellik | Açıklama |
|----------|-------------|
| **Gönderen** | **Alma Ayarları'nda**belirtilen konuk ortak veya EDIFACT anlaşması için **Ayarlar Gönder'de** belirtilen ana bilgisayar ortağı |
| **Alıcı** | **Alma Ayarları'nda**belirtilen ana bilgisayar ortağı veya EDIFACT anlaşması için **Ayarlar Gönder'de** belirtilen konuk ortak |
| **Logic App** | EDIFACT eylemlerinin ayarlandığı mantık uygulaması |
| **Durum** | EDIFACT ileti durumu <br>Başarı = Geçerli bir EDIFACT iletisi aldı veya gönderdi. İşlevsel bir ack ayarlı değil. <br>Başarı = Geçerli bir EDIFACT iletisi aldı veya gönderdi. Fonksiyonel ack ayarlanır ve alınır veya işlevsel bir ack gönderilir. <br>Başarısız = Geçersiz bir EDIFACT iletisi alındı veya gönderildi <br>Bekleyen = Geçerli bir EDIFACT iletisi alındı veya gönderildi. Fonksiyonel ack ayarlanır ve fonksiyonel bir ack bekleniyor. |
| **Ack** | Fonksiyonel Ack (CONTRL) durumu <br>Kabul = Alınan veya olumlu bir fonksiyonel ack gönderdi. <br>Reddedilen = Alınan veya gönderilen negatif işlevsel ack. <br>Bekleyen = Fonksiyonel bir ack bekliyor ama alınmadı. <br>Bekleyen = İşlevsel bir ack oluşturuldu, ancak iş ortağına gönderemiyorum. <br>Not Required = Fonksiyonel Ack kurulmadı. |
| **Yön** | EDIFACT ileti yönü |
| **İzleme Kimliği** | Bir mantık uygulamasındaki tüm tetikleyicileri ve eylemleri ilişkilendiren kimlik |
| **Msg Tipi** | EDIFACT ileti türü |
| **ıcn** | EDIFACT iletisi için Değişim Kontrol Numarası |
| **TSCN** | EDIFACT iletisi için Hareket Kümesi Kontrol Numarası |
| **Zaman damgası** | EDIFACT eyleminin iletiyi işlediği zaman |
|||

<!--
<a name="edifact-folder-file-names"></a>

### EDIFACT name formats for downloaded message files

Here are the name formats for each downloaded EDIFACT message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_outputs.txt |
|||
-->

## <a name="next-steps"></a>Sonraki adımlar

* [İzleyici ve izleme sorguları oluşturma](../logic-apps/create-monitoring-tracking-queries.md)