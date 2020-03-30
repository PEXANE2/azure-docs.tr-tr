---
title: Azure Günlük Analizinde BT Hizmet Yönetimi Bağlayıcısı | Microsoft Dokümanlar
description: Bu makalede, BT Hizmet Yönetimi Bağlayıcısı'na (ITSMC) genel bir bakış ve Azure Log Analytics'teki ITSM iş öğelerini merkezi olarak izlemek ve yönetmek ve sorunları hızla çözmek için bu çözümün nasıl kullanılacağı hakkında bilgiler sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.openlocfilehash: 50bab4c26046059b993c19a030a8f840ae336ef2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274547"
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı'nı kullanarak Azure'u ITSM araçlarına bağlayın

![BT Hizmet Yönetimi Bağlayıcısı simgesi](media/itsmc-overview/itsmc-symbol.png)

BT Hizmet Yönetimi Bağlayıcısı (ITSMC), Azure'u ve desteklenen bir BT Hizmet Yönetimi (ITSM) ürün/hizmetini bağlamanızı sağlar.

Log Analytics ve Azure Monitor gibi Azure hizmetleri, Azure ve Azure olmayan kaynaklarınızla ilgili sorunları algılamak, analiz etmek ve sorun giderme araçları sağlar. Ancak, bir sorunla ilgili iş öğeleri genellikle bir ITSM ürün/hizmetinde bulunmaktadır. ITSM bağlayıcısı, sorunları daha hızlı çözmenize yardımcı olmak için Azure ve ITSM araçları arasında çift yönlü bir bağlantı sağlar.

ITSMC aşağıdaki ITSM araçlarıyla bağlantıları destekler:

-   ServiceNow
-   Sistem Merkezi Servis Müdürü
-   Provance
-   Cherwell

ITSMC ile şunları yapabilirsiniz:

-  Azure uyarılarınızı (metrik uyarılar, Etkinlik Günlüğü uyarıları ve Log Analytics uyarıları) temel alan ITSM aracında iş öğeleri oluşturun.
-  İsteğe bağlı olarak, olaylarınızı senkronize edebilir ve istek verilerinizi ITSM aracınızdan bir Azure Log Analytics çalışma alanıyla değiştirebilirsiniz.

Yasal koşullar [ve gizlilik politikası](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9)hakkında daha fazla bilgi edinin.

ITSM Konektörünü aşağıdaki adımları kullanarak kullanmaya başlayabilirsiniz:

1.  [ITSM Konektör Çözümünü Ekleyin](#adding-the-it-service-management-connector-solution)
2.  ITSM bağlantısı oluşturma
3.  [Bağlantıyı kullanma](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>BT Hizmet Yönetimi Konektör Çözümü Ekleme

Bağlantı oluşturabilmeniz için ITSM Bağlayıcı Çözümü eklemeniz gerekir.

1. Azure portalında **+ Yeni** simgesini tıklatın.

   ![Azure yeni kaynak](media/itsmc-overview/azure-add-new-resource.png)

2. Markette **BT Hizmet Yönetimi Bağlayıcısı'nı** arayın ve **Oluştur'u**tıklatın.

   ![ITSMC çözümü ekleyin](media/itsmc-overview/add-itsmc-solution.png)

3. **OMS Çalışma Alanı** bölümünde, çözümü yüklemek istediğiniz Azure Günlük Analizi çalışma alanını seçin.
   >[!NOTE]
   > * Microsoft Operations Management Suite'ten (OMS) Azure Monitor'a devam eden geçişin bir parçası olarak, OMS Çalışma Alanları artık Log Analytics çalışma alanları olarak adlandırılır.
   > * ITSM Konektörü yalnızca aşağıdaki bölgelerdeki Log Analytics çalışma alanlarına kurulabilir: Doğu ABD, Batı US2, Güney Orta ABD, Batı Orta ABD, Orta Kanada, Batı Avrupa, Güney İngiltere, Güneydoğu Asya, Doğu Japonya, Orta Hindistan, Güneydoğu Avustralya.

4. **OMS Çalışma Alanı Ayarları** bölümünde, çözüm kaynağını oluşturmak istediğiniz Kaynak Grubu'nu seçin.

   ![ITSMC çalışma alanı](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Microsoft Operations Management Suite'ten (OMS) Azure Monitor'a devam eden geçişin bir parçası olarak, OMS Çalışma Alanları artık Log Analytics çalışma alanları olarak adlandırılır.

5. **Oluştur'u**tıklatın.

Çözüm kaynağı dağıtıldığında, pencerenin sağ üst kısmında bir bildirim görüntülenir.


## <a name="creating-an-itsm--connection"></a>ITSM bağlantısı oluşturma

Çözümü yükledikten sonra bir bağlantı oluşturabilirsiniz.

Bağlantı oluşturmak için ITSM aracınızı ITSM Bağlayıcı sı'ndan bağlantıya izin verecek şekilde hazırlamanız gerekir.  

Bağlandığınız ITSM ürününe bağlı olarak aşağıdaki adımları kullanın:

- [Sistem Merkezi Servis Yöneticisi (SCSM)](../../azure-monitor/platform/itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](../../azure-monitor/platform/itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](../../azure-monitor/platform/itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](../../azure-monitor/platform/itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

ITSM araçlarınızı hazırladıktan sonra bağlantı oluşturmak için aşağıdaki adımları izleyin:

1. Tüm **Kaynaklara**gidin, **ServiceDesk(YourWorkspaceName)** arayın.
2. Sol bölmedeki **WORKSPACE DATA SOURCES** altında **ITSM Bağlantıları'nı**tıklatın.
   ![ITSM bağlantıları](media/itsmc-overview/itsm-connections.png)

   Bu sayfa bağlantıların listesini görüntüler.
3. **Bağlantı Ekle**'ye tıklayın.

   ![ITSM bağlantısı ekle](media/itsmc-overview/add-new-itsm-connection.png)

4. [ITSMC bağlantısını ITSM ürün/hizmet makalenizle yapılandırmada](../../azure-monitor/platform/itsmc-connections.md)açıklandığı gibi bağlantı ayarlarını belirtin.

   > [!NOTE]
   >
   > Varsayılan olarak ITSMC, bağlantının yapılandırma verilerini her 24 saatte bir yeniler. Yaptığınız tüm güncellemeler veya şablon güncellemeleri için bağlantınızın verilerini anında yenilemek için bağlantınızın bıçağındaki **Eşitle** düğmesini tıklatın.

   ![Bağlantı yenileme](media/itsmc-overview/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Çözümü kullanma
   ITSM Bağlayıcısı çözümünü kullanarak Azure uyarılarından, Log Analytics uyarılarından ve Log Analytics günlük kayıtlarından iş öğeleri oluşturabilirsiniz.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarılarından ITSM iş öğeleri oluşturma

ITSM bağlantınızı oluşturduktan sonra, **Eylem Gruplarında** **ITSM Eylemi'ni** kullanarak Azure uyarılarına dayalı ITSM aracınızda iş öğesi(ler) oluşturabilirsiniz.

Eylem Grupları, Azure Uyarılarınız için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. Azure portalında metrik uyarılara, Etkinlik Günlüğü uyarılarına ve Azure Günlüğü Analizi uyarılarına sahip Eylem Grupları'nı kullanabilirsiniz.

Aşağıdaki yordamı kullanın:

1. Azure **portalında, Monitör'ü**tıklatın.
2. Sol **bölmede, Eylem gruplarını**tıklatın. **Eylem grubu ekle** penceresi görüntülenir.

    ![Eylem Grupları](media/itsmc-overview/action-groups.png)

3. Eylem grubunuz için **Ad** ve **Kısa Ad** sağlayın. Eylem **grubunuzu** oluşturmak istediğiniz Kaynak Grubu ve **Abonelik'i** seçin.

    ![Eylem Grupları detayı](media/itsmc-overview/action-groups-details.png)

4. Eylemler listesinde, **Eylem Türü**için açılan menüden **ITSM'yi** seçin. Eylem için bir **Ad** sağlayın ve **ayrıntıları edit'i**tıklatın.
5. Log Analytics çalışma alanınızın bulunduğu **Abonelik'i** seçin. **Bağlantı** adını (ITSM Bağlayıcı adınız) ve ardından Çalışma Alanı adınızı seçin. Örneğin, "MyITSMMConnector(MyWorkspace)."

    ![ITSM Eylem ayrıntıları](media/itsmc-overview/itsm-action-details.png)

6. Açılan menüden **İş Öğesi** türünü seçin.
   Varolan bir şablonkullanmayı veya ITSM ürününüzün gerektirdiği alanları doldurmayı seçin.
7. **Tamam**'a tıklayın.

Azure uyarı kuralı oluştururken/düzenlerken, ITSM Eylemi olan bir Eylem grubu kullanın. Uyarı tetiklendiğinde, ITSM aracında iş öğesi oluşturulur/güncelleştirilir.

> [!NOTE]
>
> ITSM Action fiyatlandırması hakkında daha fazla bilgi için Eylem Grupları için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın.


## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Olayı görselleştirin ve analiz edin ve istek verilerini değiştirin

Bir bağlantı kurarken yapılandırmanıza bağlı olarak, ITSM konektörü 120 güne kadar Olay ve Değişiklik isteği verilerini eşitleyebilir. Bu verilerin günlük kayıt şeması [sonraki bölümde](#additional-information)sağlanmıştır.

Olay ve değişiklik isteği verileri çözümdeki ITSM Bağlayıcı panosu kullanılarak görüntülenebilir.

![Günlük Analizi ekranı](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Pano ayrıca, bağlantılarla ilgili sorunları çözümlemek için başlangıç noktası olarak kullanılabilecek bağlayıcı durumu hakkında da bilgi sağlar.

Ayrıca, etkilenen bilgisayarlara karşı senkronize edilen olayları Hizmet Haritası çözümünde de görselleştirebilirsiniz.

Service Map, Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak keşfeder ve hizmetler arasındaki iletişimi eşler. Sunucularınızı düşündüğünüz şekilde (kritik hizmetler sunan birbirine bağlı sistemler halinde) görüntülemenize olanak tanır. Hizmet Eşlemesi, bir aracının yüklenmesi dışında yapılandırma gerektirmeyen TCP bağlantılı bir mimari üzerinde bulunan sunucular, süreçler ve bağlantı noktaları arasındaki bağlantıları gösterir. [Daha fazla bilgi edinin](../../azure-monitor/insights/service-map.md).

Hizmet Haritası çözümünü kullanıyorsanız, ITSM çözümlerinde oluşturulan servis masası öğelerini aşağıdaki örnekte gösterildiği gibi görüntüleyebilirsiniz:

![Günlük Analizi ekranı](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

Daha fazla bilgi: [Hizmet Haritası](../../azure-monitor/insights/service-map.md)


## <a name="additional-information"></a>Ek bilgiler

### <a name="data-synced-from-itsm-product"></a>ITSM ürününden senkronize edilen veriler
Olaylar ve değişiklik istekleri, bağlantının yapılandırmasına bağlı olarak ITSM ürününden Log Analytics çalışma alanınızla senkronize edilir.

Aşağıdaki bilgiler ITSMC tarafından toplanan veri örneklerini gösterir:

> [!NOTE]
>
> Log Analytics'e aktarılan iş öğesi türüne bağlı **olarak, ServiceDesk_CL** aşağıdaki alanları içerir:

**İş öğesi:** **Olaylar**  
ServiceDeskWorkItemType_s="Olay"

**Alanlar**

- ServiceDeskConnectionName
- Servis Masası Kimliği
- Durum
- Aciliyet
- Etki
- Öncelik
- Önem Yükseltme
- Oluşturan
- Tarafından Çözüldü
- Tarafından Kapatıldı
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


**Çalışma öğesi:** **İstekleri değiştir**

ServiceDeskWorkItemType_s="ChangeRequest"

**Alanlar**
- ServiceDeskConnectionName
- Servis Masası Kimliği
- Oluşturan
- Tarafından Kapatıldı
- Kaynak
- Atanan
- Başlık
- Tür
- Kategori
- Durum
- Önem Yükseltme
- Çakışma Durumu
- Aciliyet
- Öncelik
- Risk
- Etki
- Atanan
- Oluşturulduğu Tarihi
- Kapatılma Tarihi
- Son Değişiklik Tarihi
- İstenen Tarih
- Planlanan Başlangıç Tarihi
- Planlanan Bitiş Tarihi
- İş Başlangıç Tarihi
- İş Bitiş Tarihi
- Açıklama
- Bilgisayar

## <a name="output-data-for-a-servicenow-incident"></a>ServiceNow olayı için çıktı verileri

| Log Analytics alanı | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| IncidentState_s | Durum |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Tarafından açılan |
| ResolvedBy_s | Tarafından çözüldü|
| ClosedBy_s  | Tarafından kapatıldı |
| Source_s| İlgili kişi türü |
| AssignedTo_s | Atanan  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Açıldı |
| ClosedDate_t| kapalı|
| ResolvedDate_t|Çözümlendi|
| Bilgisayar  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>ServiceNow değişiklik isteği için çıktı verileri

| Log Analytics | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | Tarafından talep edilen |
| ClosedBy_s | Tarafından kapatıldı |
| AssignedTo_s | Atanan  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Durum|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Risk|
| Impact_s| Etki|
| RequestedDate_t  | Tarihe göre istenilen |
| ClosedDate_t | Kapalı tarih |
| PlannedStartDate_t  |     Planlanan başlangıç tarihi |
| PlannedEndDate_t  |   Planlanan bitiş tarihi |
| WorkStartDate_t  | Gerçek başlangıç tarihi |
| WorkEndDate_t | Gerçek bitiş tarihi|
| Description_s | Açıklama |
| Bilgisayar  | Yapılandırma Öğesi |


## <a name="troubleshoot-itsm-connections"></a>ITSM bağlantılarını giderme
1. Bağlantı bağlantı iletisini **kaydetme hatası** yla bağlı kaynağın UI'sinden başarısız olursa, aşağıdaki adımları izleyin:
   - ServiceNow, Cherwell ve Provance bağlantıları için,  
   - bağlantıların her biri için kullanıcı adını, parolayı, istemci kimliğini ve istemci sırrını doğru girdiğinizi garanti edin.  
   - bağlantıyı yapmak için ilgili ITSM ürününde yeterli ayrıcalıklara sahip olup olmadığını kontrol edin.  
   - Servis Yöneticisi bağlantıları için,  
   - Web uygulamasının başarıyla dağıtıldığından ve karma bağlantı oluşturulduğundan emin olun. Bağlantının şirket içi Servis Yöneticisi makinesiyle başarıyla kurulduğunu doğrulamak için, [karma bağlantı](../../azure-monitor/platform/itsmc-connections.md#configure-the-hybrid-connection)yapmak için belgelerde ayrıntılı olarak belirtildiği gibi Web uygulaması URL'sini ziyaret edin.  

2. ServiceNow'daki veriler Log Analytics ile senkronize edilmiyorsa, ServiceNow örneğinin uyumadığını sağlayın. ServiceNow Dev Örnekleri bazen uzun bir süre boşta yken uykuya dalıyor. Aksi takdirde, sorunu bildirin.
3. Log Analytics yangın uyarısı veriyorsa ancak ITSM ürünveya yapılandırma öğeleri oluşturulmuyorsa veya iş öğelerine veya diğer genel bilgilere bağlı değilse, aşağıdaki yerlere bakın:
   -  ITSMC: Çözüm bağlantıların/iş öğelerinin/bilgisayarların vb. bir özetini gösterir. İlgili sorguyla **Arama'da Oturum Açma'ya** götüren **Bağlayıcı Durumunu**gösteren döşemeyi tıklatın. Daha fazla bilgi için HATA olarak LogType_S ile günlük kayıtları bak.
   - **Günlük Arama** sayfası: sorgu `*`ServiceDeskLog_CL`*`kullanarak doğrudan hataları/ilgili bilgileri görüntüleyin.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Sorun Giderme Servis Yöneticisi Web Uygulaması dağıtımı
1.  Web uygulaması dağıtımıyla ilgili herhangi bir sorun olması durumunda, kaynak oluşturmak/dağıtmak için belirtilen abonelikte yeterli izine sahip olduğunuzu sağlayın.
2.  [Komut dosyasını](itsmc-service-manager-script.md)çalıştırdığınızda **"Nesne başvurusu nesnesi örneği olarak ayarlanmaz"** hatası alırsanız, **Kullanıcı Yapılandırması** bölümü altında geçerli değerler girdiğinizi sağlayın.
3.  Servis veri günü rölesi ad alanı oluşturamazsanız, gerekli kaynak sağlayıcısının aboneye kaydolduğundan emin olun. Kaydedilmemişse, Azure portalından servis veri günü rölesi ad alanı oluşturun. Azure [portalından karma bağlantı oluştururken](../../azure-monitor/platform/itsmc-connections.md#configure-the-hybrid-connection) de oluşturabilirsiniz.


## <a name="contact-us"></a>Bizimle iletişim kurun

BT Hizmet Yönetimi Konektörü ile ilgili herhangi [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com)bir sorunuz veya geri bildirim için bize ulaşın.

## <a name="next-steps"></a>Sonraki adımlar
[ITSM ürünlerini/hizmetlerini BT Hizmet Yönetimi Konektörüne ekleyin.](../../azure-monitor/platform/itsmc-connections.md)
