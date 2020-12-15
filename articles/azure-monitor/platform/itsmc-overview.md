---
title: Log Analytics BT Hizmet Yönetimi Bağlayıcısı
description: Bu makalede, BT Hizmet Yönetimi Bağlayıcısı (ıSMC) hakkında genel bir bakış ve bu iş öğelerini Log Analytics izlemek ve yönetmek ve sorunları hızlı bir şekilde çözmek için kullanma hakkında bilgiler sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.custom: references_regions
ms.openlocfilehash: 3d4e5ad0b24b7163072d7e3110a523dad9608923
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97507380"
---
# <a name="connect-azure-to-itsm-tools-by-using-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı kullanarak Azure 'ı ıTSM araçlarına bağlama

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

BT Hizmet Yönetimi Bağlayıcısı (ITSMC), Azure 'un desteklenen bir BT hizmet yönetimi (ıTSM) ürününe veya hizmetine bağlanmasını sağlar.

Azure Log Analytics ve Azure Izleyici gibi Azure Hizmetleri, Azure ve Azure dışı kaynaklarla ilgili sorunları tespit etmek, çözümlemek ve sorunlarını gidermek için araçlar sağlar. Ancak bir sorunla ilgili iş öğeleri genellikle ıTSM ürünü veya hizmetinde bulunur. ISMC, sorunları daha hızlı çözmenize yardımcı olmak için Azure ile ıTSM araçları arasında çift yönlü bir bağlantı sağlar.

ISMC aşağıdaki ıTSM araçlarıyla bağlantıları destekler:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

   >[!NOTE]
> Çeriyi ve Provance müşterilerinin, tümleştirmede başka bir çözüm olarak, Cherwell ve Provance uç noktasına [Web kancası eylemi](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#webhook) kullanmasını sağlıyoruz.

ISMC ile şunları yapabilirsiniz:

-  ITSM aracınız için Azure uyarılarınıza (ölçüm uyarıları, etkinlik günlüğü uyarıları ve Log Analytics Uyarıları) göre iş öğeleri oluşturun.
-  İsteğe bağlı olarak, ıTSM aracınızdan olay ve değişiklik isteği verilerini Azure Log Analytics çalışma alanına eşitleyebilirsiniz.

Yasal koşullar ve Gizlilik ilkesi hakkında bilgi için bkz. [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Aşağıdaki adımları tamamlayarak ıSMC kullanmaya başlayabilirsiniz:

1.  [ISMC ekleyin.](#add-it-service-management-connector)
2. [BT Hizmet Yönetimi Bağlayıcısı ile ITSM ürünlerine/hizmetlerine bağlanma](https://docs.microsoft.com/azure/azure-monitor/platform/itsmc-connections)
3.  [ITSM bağlantısı oluşturun.](#create-an-itsm-connection)
4.  [Bağlantıyı kullanın.](#use-itsmc)
   
   >[!NOTE]
> Adımları bu sırayla izlemeniz gerekir, aksi takdirde bir hata alırsınız.

##  <a name="add-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı Ekle

Bir bağlantı oluşturabilmeniz için önce ıSMC eklemeniz gerekir.

1. Azure portal **kaynak oluştur**' u seçin:

   ![Kaynak oluştur menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/azure-add-new-resource.png)

2. Azure Marketi 'nde **BT hizmet yönetimi Bağlayıcısı** arayın. **Oluştur**' u seçin.

   ![Azure Marketi 'ndeki Oluştur düğmesini gösteren ekran görüntüsü.](media/itsmc-overview/add-itsmc-solution.png)

3. **OMS çalışma alanı** bölümünde, itsmc ' ı yüklemek istediğiniz Azure Log Analytics çalışma alanını seçin.
   >[!NOTE]
   > * Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS çalışma alanları artık *Log Analytics çalışma alanları* olarak adlandırılır.
   > * ISMC yalnızca şu bölgelerde Log Analytics çalışma alanlarına yüklenebilir: Doğu ABD, Batı ABD 2, Orta Güney ABD, Orta Batı ABD, US Gov Arizona, US Gov Virginia, Kanada Orta, Batı Avrupa, Güney UK, Güneydoğu Asya, Japonya Doğu, Orta Hindistan ve Avustralya Güneydoğu.


4. **Log Analytics çalışma alanı** bölümünde, itsmc kaynağını oluşturmak istediğiniz kaynak grubunu seçin:

   ![Log Analytics çalışma alanı bölümünü gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Microsoft Operations Management Suite (OMS) ile Azure Izleyici arasında devam eden geçişin bir parçası olarak, OMS çalışma alanları artık *Log Analytics çalışma alanları* olarak adlandırılır.

5. **Tamam**’ı seçin.

ISMC kaynağı dağıtıldığında, pencerenin sağ üst köşesinde bir bildirim görüntülenir.


## <a name="create-an-itsm-connection"></a>ITSM bağlantısı oluşturma

ISMC yükledikten sonra bir bağlantı oluşturabilirsiniz.

Bağlantı oluşturmak için, ITSM aracınızdan bağlantı sağlamak üzere ITSM aracınızı yapmanız gerekir.  

Bağlanmakta olduğunuz ıTSM ürününe bağlı olarak, yönergeler için aşağıdaki bağlantılardan birini seçin:

- [System Center Service Manager](./itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](./itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](./itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](./itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

ITSM araçlarınızı önceden tamamladıktan sonra bağlantı oluşturmak için aşağıdaki adımları izleyin:

1. **Tüm kaynaklarda**, **ServiceDesk 'yi arayın (*çalışma alanınızın adı*)**:

   ![Azure portal son kaynakları gösteren ekran görüntüsü.](media/itsmc-overview/itsm-connections.png)

1. Sol bölmedeki **çalışma alanı veri kaynakları** altında **ITSM bağlantıları**' nı seçin:

   ![ITSM bağlantıları menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/add-new-itsm-connection.png)
   Bu sayfada bağlantıların listesi görüntülenir.
1. **Bağlantı ekle**' yi seçin.

4. ITSM [Ürünlerimiz/hizmetlerinizle ıSMC bağlantısını yapılandırma](./itsmc-connections.md)bölümünde açıklandığı gibi bağlantı ayarlarını belirtin.

   > [!NOTE]
   >
   > Varsayılan olarak, ıSMC her 24 saatte bir bağlantının yapılandırma verilerini yeniler. Yaptığınız düzenlemeleri veya şablon güncelleştirmelerini yansıtacak şekilde bağlantınızın verilerini hemen yenilemek için, bağlantınızın dikey penceresindeki **Eşitle** düğmesini seçin:
   >
   > ![Bağlantı dikey penceresinde Eşitle düğmesini gösteren ekran görüntüsü.](media/itsmc-overview/itsmc-connections-refresh.png)


## <a name="use-itsmc"></a>ISMC kullanma
   Azure uyarılarından, Log Analytics uyarılarla ve Log Analytics günlük kayıtlarından iş öğeleri oluşturmak için ıSMC kullanabilirsiniz.

## <a name="template-definitions"></a>Şablon tanımları
   ITSM aracı tarafından tanımlanan şablonları kullanılabilecek iş öğesi türleri vardır.
Şablonları kullanarak, eylem grubunun bir parçası olarak tanımlanan sabit değerlere göre otomatik olarak doldurulacak alanları tanımlayabilirsiniz. ITSM aracında şablonlar tanımlarsınız. Eylem grubu tanımının bir parçası olarak kullanmak istediğiniz şablonu tanımlayabilirsiniz.
      
## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarılarından ıTSM iş öğeleri oluşturma

ITSM bağlantınızı oluşturduktan sonra, Azure uyarılarını temel alan ıTSM aracınız için iş öğeleri oluşturabilirsiniz. İş öğelerini oluşturmak için, eylem gruplarında ıTSM eylemini kullanacaksınız.

Eylem grupları, Azure uyarılarınız için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. İşlem gruplarını, ölçüm uyarıları, etkinlik günlüğü uyarıları ve Azure portal Azure Log Analytics uyarıları ile birlikte kullanabilirsiniz.

> [!NOTE]
> ITSM bağlantısını oluşturduktan sonra eşitleme işleminin tamamlanabilmesi için 30 dakika beklemeniz gerekir.
> 

İş öğeleri oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portal,  **Uyarılar**' ı seçin.
2. Ekranın üst kısmındaki menüde **Yönet eylemler**' i seçin:

    ![Eylemleri Yönet menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/action-groups-selection-big.png)

   **Eylem grubu oluştur** penceresi görüntülenir.

3. Eylem grubunuzu oluşturmak istediğiniz **aboneliği** ve **kaynak grubunu** seçin. Eylem grubunuz için bir **eylem grubu adı** ve **görünen ad** belirtin. **İleri: bildirimler**' i seçin.

    ![Eylem grubu Oluştur penceresini gösteren ekran görüntüsü.](media/itsmc-overview/action-groups-details.png)

4. Bildirim listesinde **İleri: eylemler**' i seçin.
5. Eylemler listesinde, **eylem türü** listesinde **ISM** ' yi seçin. Eylem için bir **ad** girin. **Ayrıntıları Düzenle**' yi temsil eden kalem düğmesini seçin.
6. **Abonelik** listesinde Log Analytics çalışma alanınızın bulunduğu aboneliği seçin. **Bağlantı** listesinde ITSM bağlayıcınızın adını seçin. Bunun ardından çalışma alanınızın adı gelir. Örneğin, MyITSMConnector (çalışma alanım).

7. Bir **Iş öğesi** türü seçin.

8. Sabit değerlerle kullanıma hazır alanları doldurmanız istiyorsanız **özel şablon kullan**' ı seçin. Aksi takdirde, **şablon** listesinde var olan bir [şablonu](#template-definitions) seçin ve şablon alanlarına sabit değerleri girin.

9. **Her yapılandırma öğesi için bireysel çalışma öğeleri oluştur**' u seçerseniz, her yapılandırma öğesinin kendi iş öğesi olur. Her yapılandırma öğesi için bir iş öğesi olacaktır. Oluşturulacak uyarılara göre güncelleştirilir.

    * İş öğesi açılan kutusu "olay" veya "uyarı" bölümünde seçtiğiniz bir durumda: **her bir yapılandırma öğesi için bireysel çalışma öğeleri oluştur** onay kutusunu temizlerseniz, her uyarı yeni bir iş öğesi oluşturur. Yapılandırma öğesi başına birden fazla uyarı olabilir.

       ![ITSM olay penceresini gösteren ekran görüntüsü.](media/itsmc-overview/itsm-action-configuration.png)

    * İş öğesi açılan kutusu "olay" nda seçtiğiniz bir durumda: radyo düğmeleri seçiminde her bir **günlük girişi için bireysel iş öğeleri oluştur** ' u seçerseniz, her uyarı yeni bir iş öğesi oluşturur. Radyo düğmeleri seçiminde **her yapılandırma öğesi için bireysel çalışma öğeleri oluştur** ' u seçerseniz, her yapılandırma öğesinin kendi iş öğesi olur.
   ![ITSM olay penceresini gösteren ekran görüntüsü.](media/itsmc-overview/itsm-action-configuration-event.png)

10. **Tamam**’ı seçin.

Bir Azure uyarı kuralı oluşturduğunuzda veya düzenlediğinizde, bir ıTSM eylemi olan bir eylem grubunu kullanın. Uyarı tetiklendiğinde, çalışma öğesi ıTSM aracında oluşturulur veya güncelleştirilir.

> [!NOTE]
>
>- ITSM eyleminin fiyatlandırması hakkında daha fazla bilgi için, eylem grupları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın.
>
>
>- Uyarı kuralı tanımındaki kısa açıklama alanı, ıTSSM eylemini kullanarak gönderdiğinizde 40 karakterle sınırlıdır.

## <a name="additional-information"></a>Ek bilgiler

### <a name="data-synced-from-your-itsm-product"></a>ITSM ürününüzle eşitlenen veriler
Olaylar ve değişiklik istekleri, bağlantı yapılandırmasına bağlı olarak ıTSM ürününüzle Log Analytics çalışma alanınıza eşitlenir.

Bu bölümde, ıSMC tarafından toplanan verilerin bazı örnekleri gösterilmektedir.

**ServiceDesk_CL** alanlar, Log Analytics içe aktardığınız iş öğesi türüne bağlı olarak değişir. İki iş öğesi türü için bir alan listesi aşağıda verilmiştir:

**İş öğesi:** **Olaylar**  
ServiceDeskWorkItemType_s = "olay"

**Alanlar**

- ServiceDeskConnectionName
- Hizmet Masası KIMLIĞI
- Durum
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
- Oluşturulduğu Tarihi
- Kapatılma Tarihi
- Çözümlenme Tarihi
- Son Değişiklik Tarihi
- Bilgisayar


**İş öğesi:** **değişiklik istekleri**

ServiceDeskWorkItemType_s = "ChangeRequest"

**Alanlar**
- ServiceDeskConnectionName
- Hizmet Masası KIMLIĞI
- Oluşturan
- Kapatan
- Kaynak
- Atanan
- Başlık
- Tür
- Kategori
- Durum
- Önem Yükseltme
- Çakışma durumu
- Aciliyet
- Öncelik
- Risk
- Etki
- Atanan
- Oluşturulduğu Tarihi
- Kapatılma Tarihi
- Son Değişiklik Tarihi
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
| IncidentState_s | Durum |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Açan |
| ResolvedBy_s | Çözümleyen|
| ClosedBy_s  | Kapatan |
| Source_s| Kişi türü |
| AssignedTo_s | Atanan  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Makta |
| ClosedDate_t| kapalı|
| ResolvedDate_t|Çözümlendi|
| Bilgisayar  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>ServiceNow değişiklik isteği için çıkış verileri

| Log Analytics | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | İsteyen |
| ClosedBy_s | Kapatan |
| AssignedTo_s | Atanan  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Durum|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Risk|
| Impact_s| Etki|
| RequestedDate_t  | İstek tarihine göre |
| ClosedDate_t | Kapatma tarihi |
| PlannedStartDate_t  | Planlanan başlangıç tarihi |
| PlannedEndDate_t  | Planlanan bitiş tarihi |
| WorkStartDate_t  | Gerçek başlangıç tarihi |
| WorkEndDate_t | Gerçek bitiş tarihi|
| Description_s | Açıklama |
| Bilgisayar  | Yapılandırma öğesi |

## <a name="contact-us"></a>Bizimle iletişim kurun

BT Hizmet Yönetimi Bağlayıcısı hakkında sorgular veya geri bildiriminiz varsa, adresinden bizimle iletişim kurun [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com) .

## <a name="next-steps"></a>Sonraki adımlar
[BT Hizmet Yönetimi Bağlayıcısı için ıTSM ürünleri/hizmetleri ekleyin](./itsmc-connections.md)
