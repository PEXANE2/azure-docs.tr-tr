---
title: Azure Log Analytics BT Hizmet Yönetimi Bağlayıcısı | Microsoft Docs
description: Bu makalede, Azure Log Analytics 'deki ITSM iş öğelerini merkezi olarak izlemek ve yönetmek ve sorunları hızlı bir şekilde çözümlemek ve yönetmek için bu çözümün nasıl kullanılacağına dair bir genel bakış sunulmaktadır BT Hizmet Yönetimi Bağlayıcısı (ıSMC)
services: log-analytics
documentationcenter: ''
author: jyothirmaisuri
manager: riyazp
editor: ''
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: v-jysur
ms.openlocfilehash: 6ae1792d9ef3f07ef7258451be79b587692a1694
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809449"
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Azure 'ı BT Hizmet Yönetimi Bağlayıcısı kullanarak ıTSSM araçlarına bağlama

![BT Hizmet Yönetimi Bağlayıcısı simgesi](media/itsmc-overview/itsmc-symbol.png)

BT Hizmet Yönetimi Bağlayıcısı (ıSMC), Azure ve desteklenen bir BT hizmet yönetimi (ıTSM) ürünü/hizmeti 'ne bağlanmanızı sağlar.

Log Analytics ve Azure Izleyici gibi Azure Hizmetleri, Azure ve Azure dışı kaynaklarla ilgili sorunları tespit etmek, çözümlemek ve sorunlarını gidermek için araçlar sağlar. Ancak, bir sorunla ilgili iş öğeleri genellikle ıTSM ürünü/hizmetinde bulunur. ITSM Bağlayıcısı, sorunları daha hızlı çözmenize yardımcı olmak için Azure ile ıTSM araçları arasında çift yönlü bir bağlantı sağlar.

ISMC aşağıdaki ıTSM araçlarıyla bağlantıları destekler:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

ISMC ile şunları yapabilirsiniz:

-  ITSM aracında Azure uyarılarınıza (ölçüm uyarıları, etkinlik günlüğü uyarıları ve Log Analytics Uyarıları) göre iş öğeleri oluşturun.
-  İsteğe bağlı olarak, ıTSM aracınızdan olay ve değişiklik isteği verilerini Azure Log Analytics çalışma alanına eşitleyebilirsiniz.

[Yasal koşullar ve Gizlilik ilkesi](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9)hakkında daha fazla bilgi edinin.

Aşağıdaki adımları kullanarak ITSM Bağlayıcısı kullanmaya başlayabilirsiniz:

1.  [ITSM Bağlayıcısı çözümünü ekleyin](#adding-the-it-service-management-connector-solution)
2.  ITSM bağlantısı oluşturma
3.  [Bağlantıyı kullanma](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>BT Hizmet Yönetimi Bağlayıcısı çözümü ekleme

Bir bağlantı oluşturabilmeniz için önce ITSM Bağlayıcısı çözümünü eklemeniz gerekir.

1. Azure portal ' de **+ Yeni** simgesi ' ne tıklayın.

   ![Azure yeni kaynak](media/itsmc-overview/azure-add-new-resource.png)

2. Market 'te **BT hizmet yönetimi Bağlayıcısı** arayın ve **Oluştur**' a tıklayın.

   ![ISMC çözümü Ekle](media/itsmc-overview/add-itsmc-solution.png)

3. **OMS çalışma alanı** bölümünde, çözümü yüklemek istediğiniz Azure Log Analytics çalışma alanını seçin.
   >[!NOTE]
   > * Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.
   > * ITSM Bağlayıcısı yalnızca şu bölgelerde Log Analytics çalışma alanlarına yüklenebilir: Doğu ABD, Batı Avrupa, Güneydoğu Asya, Güneydoğu Avustralya, Orta Batı ABD, Doğu Japonya, Güney UK, Orta Hindistan, Orta Kanada.

4. **OMS çalışma alanı ayarları** bölümünde, çözüm kaynağını oluşturmak istediğiniz ResourceGroup öğesini seçin.

   ![ISMC çalışma alanı](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.

5. **Oluştur**’a tıklayın.

Çözüm kaynağı dağıtıldığında, pencerenin sağ üst kısmında bir bildirim görüntülenir.


## <a name="creating-an-itsm--connection"></a>ITSM bağlantısı oluşturma

Çözümü yükledikten sonra bir bağlantı oluşturabilirsiniz.

Bağlantı oluşturmak için, ITSM Bağlayıcısı çözümüyle bağlantı sağlamak üzere ıTSM aracınızdan Prep yapmanız gerekir.  

Bağlanmakta olduğunuz ıSM ürününe bağlı olarak, aşağıdaki adımları kullanın:

- [System Center Service Manager (SCSM)](../../azure-monitor/platform/itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](../../azure-monitor/platform/itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](../../azure-monitor/platform/itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](../../azure-monitor/platform/itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

ITSM araçlarınızı önceden doldurduktan sonra bağlantı oluşturmak için aşağıdaki adımları izleyin:

1. **Tüm kaynaklara**gidin, **ServiceDesk (yourçalışmaalanıadı)** öğesini arayın.
2. Sol bölmedeki **çalışma alanı VERI kaynakları** altında **ITSM bağlantıları**' na tıklayın.
   ıTSM bağlantıları ![](media/itsmc-overview/itsm-connections.png)

   Bu sayfada bağlantıların listesi görüntülenir.
3. **Bağlantı ekle**' ye tıklayın.

   ![ITSM bağlantısı ekle](media/itsmc-overview/add-new-itsm-connection.png)

4. ITSM [ürünleri/hizmetleri makalelerinizle ısmc bağlantısını yapılandırma](../../azure-monitor/platform/itsmc-connections.md)bölümünde açıklandığı gibi bağlantı ayarlarını belirtin.

   > [!NOTE]
   >
   > Varsayılan olarak, ıSMC her 24 saatte bir bağlantının yapılandırma verilerini yeniler. Yaptığınız herhangi bir düzenleme veya şablon güncelleştirmesi için bağlantınızın verilerini anında yenilemek üzere bağlantınızın dikey penceresindeki **Eşitle** düğmesine tıklayın.

   ![Bağlantı yenileme](media/itsmc-overview/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Çözümü kullanma
   ITSM Bağlayıcısı çözümünü kullanarak Azure uyarılarından iş öğeleri oluşturabilir, Log Analytics uyarılar ve Log Analytics günlük kayıtları oluşturabilirsiniz.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarılarından ıTSM iş öğeleri oluşturma

ITSM bağlantınız oluşturulduktan sonra, **Işlem gruplarındaki** **ıtssm eylemini** kullanarak ITSM aracınız üzerinde Azure uyarılarını temel alan iş öğeleri oluşturabilirsiniz.

Eylem grupları, Azure uyarılarınız için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. Işlem gruplarını, ölçüm uyarıları, etkinlik günlüğü uyarıları ve Azure portal Azure Log Analytics uyarıları ile birlikte kullanabilirsiniz.

Aşağıdaki yordamı kullanın:

1. Azure portal, **İzle**' ye tıklayın.
2. Sol bölmede **eylem grupları**' na tıklayın. **Eylem grubu Ekle** penceresi görüntülenir.

    ![Eylem Grupları](media/itsmc-overview/action-groups.png)

3. Eylem grubunuz için **Name** ve **ShortName** sağlayın. Eylem grubunuzu oluşturmak istediğiniz **kaynak grubunu** ve **aboneliği** seçin.

    ![Eylem grupları ayrıntısı](media/itsmc-overview/action-groups-details.png)

4. Eylemler listesinde, **eylem türü**için açılan menüden **ıtssm** ' ı seçin. Eylem için bir **ad** girin ve **Ayrıntıları Düzenle**' ye tıklayın.
5. Log Analytics çalışma alanınızın bulunduğu **aboneliği** seçin. **Bağlantı** adını (ITSM Bağlayıcısı adınız) ve ardından çalışma alanınızın adını seçin. Örneğin, "MyITSMMConnector (MyWorkspace)".

    ![ITSM eylemi ayrıntıları](media/itsmc-overview/itsm-action-details.png)

6. Açılır menüden **Iş öğesi** türünü seçin.
   Mevcut bir şablonu kullanmayı veya ıTSM ürününüzün gerektirdiği Alanları doldurmayı seçin.
7. **Tamam**’a tıklayın.

Bir Azure uyarı kuralı oluştururken/düzenlenirken, bir ıTSM eylemi olan bir eylem grubu kullanın. Uyarı tetiklendiğinde, çalışma öğesi ıTSM aracında oluşturulur/güncelleştirilir.

> [!NOTE]
>
> ITSM eyleminin fiyatlandırması hakkında bilgi için bkz. eylem grupları için [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/monitor/) .


## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Olay ve değişiklik isteği verilerini görselleştirin ve çözümleyin

ITSM Bağlayıcısı bir bağlantı kurarken yapılandırmanıza bağlı olarak, 120 güne kadar olay ve değişiklik isteği verilerini eşitleyebilir. Bu veriler için günlük kaydı şeması, [sonraki bölümde](#additional-information)verilmiştir.

Olay ve değişiklik isteği verileri, çözümdeki ITSM Bağlayıcısı Panosu kullanılarak görselleştirilebilir.

![Log Analytics ekranı](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Pano, bağlantılarla ilgili sorunları çözümlemek için bir başlangıç noktası olarak kullanılabilecek bağlayıcı durumu hakkında bilgi de sağlar.

Ayrıca, Hizmet Eşlemesi çözümü içinde etkilenen bilgisayarlara karşı eşitlenen olayları da görselleştirebilirsiniz.

Hizmet Eşlemesi Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Bu sayede, önemli hizmetleri sunan bağlantılı sistemler gibi, sunucularınızı düşündüğünüzden görüntülemenize izin verir. Hizmet Eşlemesi, bir aracı yüklemesi dışında hiçbir yapılandırma gerekmeden, herhangi bir TCP bağlantılı mimarideki sunucular, süreçler ve bağlantı noktaları arasındaki bağlantıları gösterir. [Daha fazla bilgi edinin](../../azure-monitor/insights/service-map.md).

Hizmet Eşlemesi çözümünü kullanıyorsanız, ıTSM çözümlerinde oluşturulan hizmet Masası öğelerini aşağıdaki örnekte gösterildiği gibi görüntüleyebilirsiniz:

![Log Analytics ekranı](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

Daha fazla bilgi: [hizmet eşlemesi](../../azure-monitor/insights/service-map.md)


## <a name="additional-information"></a>Ek Bilgi

### <a name="data-synced-from-itsm-product"></a>ITSM ürününden eşitlenen veriler
Olaylar ve değişiklik istekleri, bağlantı yapılandırmasına bağlı olarak ıTSM ürününüzle Log Analytics çalışma alanınıza eşitlenir.

Aşağıdaki bilgiler, ıSMC tarafından toplanan veri örneklerini gösterir:

> [!NOTE]
>
> Log Analytics içeri aktarılan iş öğesi türüne bağlı olarak, **ServiceDesk_CL** aşağıdaki alanları içerir:

**İş öğesi:** **Olaylar**  
ServiceDeskWorkItemType_s = "olay"

**Alanını**

- ServiceDeskConnectionName
- Hizmet Masası KIMLIĞI
- Eyalet
- Aciliyet
- Etki
- Öncelik
- Önem Yükseltme
- Oluşturan
- Çözümleyen
- Kapatan
- Kaynak
- Atanan
- Kategori
- Başlık
- Açıklama
- Oluşturulma tarihi
- Kapatma tarihi
- Çözümlenme tarihi
- Son değiştirilme tarihi
- Bilgisayar


**İş öğesi:** **değişiklik istekleri**

ServiceDeskWorkItemType_s = "ChangeRequest"

**Alanını**
- ServiceDeskConnectionName
- Hizmet Masası KIMLIĞI
- Oluşturan
- Kapatan
- Kaynak
- Atanan
- Başlık
- Tür
- Kategori
- Eyalet
- Önem Yükseltme
- Çakışma durumu
- Aciliyet
- Öncelik
- Risk
- Etki
- Atanan
- Oluşturulma tarihi
- Kapatma tarihi
- Son değiştirilme tarihi
- İstenen Tarih
- Planlanan başlangıç tarihi
- Planlanan bitiş tarihi
- Çalışma başlangıç tarihi
- Çalışma bitiş tarihi
- Açıklama
- Bilgisayar

## <a name="output-data-for-a-servicenow-incident"></a>ServiceNow olayı için çıkış verileri

| Log Analytics alanı | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| IncidentState_s | Eyalet |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Açan |
| ResolvedBy_s | Çözümleyen|
| ClosedBy_s  | Kapatan |
| Source_s| Kişi türü |
| AssignedTo_s | Şuna atandı  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Makta |
| ClosedDate_t| kapandı|
| ResolvedDate_t|Çözülüyor|
| Bilgisayar  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>ServiceNow değişiklik isteği için çıkış verileri

| Log Analytics | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | İsteyen |
| ClosedBy_s | Kapatan |
| AssignedTo_s | Şuna atandı  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Eyalet|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Risk|
| Impact_s| Etki|
| RequestedDate_t  | İstek tarihine göre |
| ClosedDate_t | Kapatma tarihi |
| PlannedStartDate_t  |     Planlanan başlangıç tarihi |
| PlannedEndDate_t  |   Planlanan bitiş tarihi |
| WorkStartDate_t  | Gerçek başlangıç tarihi |
| WorkEndDate_t | Gerçek bitiş tarihi|
| Description_s | Açıklama |
| Bilgisayar  | Yapılandırma öğesi |


## <a name="troubleshoot-itsm-connections"></a>ITSM bağlantılarında sorun giderme
1. Bağlantı bağlanılan kaynağın kullanıcı arabiriminden bağlantı başarısız olursa, bağlantı iletisini **kaydetme hatası** ile aşağıdaki adımları uygulayın:
   - ServiceNow, Cherwell ve Provance bağlantıları için  
   - bağlantıların her biri için Kullanıcı adını, parolayı, istemci KIMLIĞINI ve istemci gizli anahtarını doğru girdiğinizden emin olun.  
   - bağlantıyı kurmak için karşılık gelen ıTSM ürününde yeterli ayrıcalıklara sahip olup olmadığınızı denetleyin.  
   - Service Manager bağlantıları için  
   - Web uygulamasının başarıyla dağıtıldığından ve karma bağlantının oluşturulduğundan emin olun. Bağlantının şirket içi Service Manager makinesiyle başarıyla yapıldığını doğrulamak için, [karma bağlantı](../../azure-monitor/platform/itsmc-connections.md#configure-the-hybrid-connection)oluşturma belgelerinde ayrıntılı olarak Web uygulaması URL 'sini ziyaret edin.  

2. ServiceNow 'daki veriler Log Analytics ile eşitlenmediği için ServiceNow örneğinin uyku modunda olmadığından emin olun. ServiceNow dev örnekleri bazen uzun bir süre boşta kaldığında uyku moduna geçebilir. Aksi takdirde, sorunu bildirin.
3. Log Analytics uyarılar harekete geçse ancak ıTSM ürününde iş öğeleri oluşturulmadıysa veya yapılandırma öğeleri, iş öğelerine ya da diğer genel bilgiler için oluşturulmadıysa veya bağlanmadıysa, aşağıdaki yerlere bakın:
   -  ISMC: çözüm, bağlantıların/iş öğelerinin/bilgisayarların, vb. bir özetini gösterir. **Bağlayıcı durumunu**gösteren kutucuğa tıklayın ve bu, arama ile Ilgili sorgu **günlüğü** için sizi yönlendirir. Daha fazla bilgi için LogType_S ile günlük kayıtlarına bakın.
   - **Günlük arama** sayfası: `*`ServiceDeskLog_CL`*`sorgusunu kullanarak hataları/ilgili bilgileri doğrudan görüntüleyin.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Web uygulaması dağıtımı Service Manager sorunlarını giderme
1.  Web uygulaması dağıtımıyla ilgili herhangi bir sorun olması durumunda, kaynak oluşturmak/dağıtmak için belirtilen abonelikte yeterli izinlere sahip olduğunuzdan emin olun.
2.  [Komut dosyasını](itsmc-service-manager-script.md)çalıştırdığınızda bir **"nesne başvurusu bir nesnenin örneğine ayarlanmadı"** hatası alırsanız, **Kullanıcı Yapılandırması** bölümünde geçerli değerler girdiğinizden emin olun.
3.  Service Bus geçişi ad alanı oluşturmadıysanız, gerekli kaynak sağlayıcının abonelikte kayıtlı olduğundan emin olun. Kayıtlı değilse, Service Bus geçişi ad alanını Azure portal el ile oluşturun. [Karma bağlantıyı Azure Portal oluştururken](../../azure-monitor/platform/itsmc-connections.md#configure-the-hybrid-connection) da oluşturabilirsiniz.


## <a name="contact-us"></a>Bize ulaşın

BT Hizmet Yönetimi Bağlayıcısı tüm sorgular veya geri bildirimler için [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com)' de bizimle iletişim kurun.

## <a name="next-steps"></a>Sonraki adımlar
[BT hizmet yönetimi Bağlayıcısı IÇIN ITSM ürünleri/hizmetleri ekleyin](../../azure-monitor/platform/itsmc-connections.md).
