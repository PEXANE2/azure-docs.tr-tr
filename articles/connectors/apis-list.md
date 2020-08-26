---
title: Azure Logic Apps için Bağlayıcılar
description: Yerleşik, yönetilen, şirket içi, tümleştirme hesabı, ıSE ve kurumsal bağlayıcılar gibi Azure Logic Apps için bağlayıcıları olan iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 06/11/2020
ms.openlocfilehash: 1bd3cb1c18d1bac078ac1344f574914dba73d07b
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871573"
---
# <a name="connectors-for-azure-logic-apps"></a>Azure Logic Apps için Bağlayıcılar

Bağlayıcılar; Azure Logic Apps’ten diğer uygulamalar, hizmetler, sistemler, protokoller ve platformlar arasındaki olaylara, verilere ve eylemlere hızlı erişilmesini sağlar. Mantıksal uygulamalarınızdaki bağlayıcıları kullanarak, oluşturduğunuz ve zaten sahip olduğunuz verilerle görevler gerçekleştirmek için bulut ve şirket içi uygulamalarınızın yeteneklerini genişletebilirsiniz.

Logic Apps [yüzlerce bağlayıcı](/connectors)sunurken, bu makalede binlerce uygulama tarafından başarıyla kullanılan *popüler ve daha yaygın kullanılan* bağlayıcılar ve veri ve bilgi işleme için milyonlarca yürütme açıklanmaktadır. Bağlayıcıların tam listesini ve Tetikleyiciler, Eylemler ve sınırlar gibi her bağlayıcının başvuru bilgilerini bulmak için [Bağlayıcılar genel bakış](/connectors)altındaki bağlayıcı başvuru sayfalarını gözden geçirin. Ayrıca [Tetikleyiciler ve eylemler](#triggers-actions), [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)ve [Logic Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/)hakkında daha fazla bilgi edinin.

> [!TIP]
> Bağlayıcısı olmayan bir hizmet veya API ile tümleştirme için, hizmeti doğrudan HTTP gibi bir protokol üzerinden çağırabilir veya [özel bir bağlayıcı](#custom)oluşturabilirsiniz.

## <a name="connector-types"></a>Bağlayıcı türleri

Bağlayıcılar, yerleşik Tetikleyiciler ve eylemler olarak veya yönetilen bağlayıcılar olarak kullanılabilir.

<a name="built-in"></a>

* [**Yerleşik**](#built-ins): yerleşik Tetikleyiciler ve eylemler, Azure Logic Apps için "yerel" ve mantıksal uygulamalarınız için bu görevleri gerçekleştirmenize yardımcı olur:

  * Özel ve gelişmiş zamanlamalarda çalıştırın.

  * Mantıksal uygulamanızın iş akışını düzenleyin ve denetleyin, örneğin döngüler ve koşullar ve ayrıca değişkenlerle ve veri işlemleriyle çalışma.

  * Diğer uç noktalarla iletişim kurun.

  * İstekleri alın ve yanıtlayın.

  * Azure işlevleri, Azure API Apps (Web Apps), Azure API Management ile yönetilen ve yayınlanan API 'lerinizi ve istekleri alabilen iç içe mantık uygulamalarını çağırın.

<a name="managed-connectors"></a>

* [**Yönetilen bağlayıcılar**](#managed-api-connectors): Microsoft tarafından dağıtılan ve yönetilen bu bağlayıcılar, Office 365, Azure Blob depolama, SQL Server, Dynamics, Salesforce, SharePoint ve daha fazlası dahil olmak üzere bulut hizmetlerine, şirket içi sistemlere veya her ikisine de erişmek için Tetikleyiciler ve eylemler sağlar. Bazı bağlayıcılar özellikle işletmeden işletmeye (B2B) iletişim senaryolarını destekler ve mantıksal uygulamanıza bağlı bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) gerektirir. Belirli bağlayıcılar kullanılmadan önce, Azure Logic Apps tarafından yönetilen bağlantıları oluşturmanız gerekebilir.

  Örneğin, Microsoft BizTalk Server kullanıyorsanız, mantıksal uygulamalarınız [BizTalk Server şirket içi bağlayıcıyı](#on-premises-connectors)kullanarak BizTalk Server bağlanabilir ve sizinle iletişim kurabilir. Daha sonra [tümleştirme hesabı bağlayıcıları](#integration-account-connectors)' nı kullanarak mantıksal uygulamalarınızda BizTalk benzeri işlemleri genişletebilir veya gerçekleştirebilirsiniz.

  Bağlayıcılar, standart ya da kurumsal olarak sınıflandırılır. [Kurumsal bağlayıcılar](#enterprise-connectors) , ek bir MALIYET için SAP, IBM MQ ve IBM 3270 gibi kurumsal sistemlere erişim sağlar. Bağlayıcının standart veya kurumsal olduğunu anlamak için, [Bağlayıcılar genel bakış](/connectors)altındaki her bağlayıcının başvuru sayfasındaki Teknik ayrıntılara bakın.

  Ayrıca, bazı bağlayıcılar birden fazla kategoride yer değiştirebilse de, bu kategorileri kullanarak bağlayıcıları tanımlayabilirsiniz. Örneğin, SAP bir kurumsal bağlayıcı ve şirket içi bağlayıcıdır:

  | Kategori | Açıklama |
  |----------|-------------|
  | [**Yönetilen bağlayıcılar**](#managed-api-connectors) | Azure Blob depolama, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online ve çok daha fazlası gibi hizmetleri kullanan mantıksal uygulamalar oluşturun. |
  | [**Şirket içi bağlayıcılar**](#on-premises-connectors) | Şirket [içi veri ağ geçidini][gateway-doc]yükleyip ayarladıktan sonra, bu bağlayıcılar mantıksal uygulamalarınızın SQL Server, SharePoint Server, Oracle DB, dosya paylaşımları ve diğerleri gibi şirket içi sistemlere erişmesine yardımcı olur. |
  | [**Tümleştirme hesabı bağlayıcıları**](#integration-account-connectors) | Bir tümleştirme hesabı oluştururken ve ödeme yaptığınızda, bu bağlayıcılar XML 'yi dönüştürür ve doğrular, düz dosyaları kodlayıp kodunu çözer ve AS2, EDIOLGU ve x12 protokolleriyle işletmeden işletmeye (B2B) iletileri işler. |
  |||

<a name="integration-service-environment"></a>

### <a name="connect-from-an-integration-service-environment-ise"></a>Tümleştirme hizmeti ortamından (ıSE) Bağlan

Bir Azure sanal ağındaki kaynaklara doğrudan erişmesi gereken Logic Apps için, adanmış kaynaklarda mantıksal uygulamalarınızı oluşturabileceğiniz, dağıtabileceğiniz ve çalıştırabileceğiniz bir adanmış [tümleştirme hizmeti ortamı (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturabilirsiniz. Logic App Designer 'da, bir ıSE 'de Logic Apps için kullanmak istediğiniz bağlayıcılara gözattığınızda, yerleşik Tetikleyiciler ve eylemler üzerinde, **Ise** etiketi bazı bağlayıcılarda göründüğünde bir **çekirdek** etiketi görünür.

> [!NOTE]
> ISE ve bağlayıcılarında çalışan Logic Apps, bu bağlayıcıların çalıştığı durumlar ne olursa olsun, tüketim tabanlı fiyatlandırma planına karşı sabit bir fiyatlandırma planını takip edin. Daha fazla bilgi için bkz. [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md) ve [Logic Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/).

| Etiketle | Örnek | Açıklama |
|-------|---------|-------------|
| **ÇEKIRDEĞIN** | ![Örnek çekırdek Bağlayıcısı](./media/apis-list/example-core-connector.png) | Bu etiketle birlikte yerleşik Tetikleyiciler ve Eylemler, Logic Apps ile aynı ıSE 'de çalışır. |
| **ISE** | ![Örnek ıSE Bağlayıcısı](./media/apis-list/example-ise-connector.png) | Bu etikete sahip yönetilen bağlayıcılar, Logic Apps ile aynı ıSE 'de çalışır. Azure sanal ağına bağlı bir şirket içi sisteminiz varsa, bir ıSE, Logic Apps 'in [Şirket içi veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md)olmadan bu sisteme doğrudan erişmesini sağlar. Bunun yerine, varsa, bu sistemin **Ise** bağlayıcısını kullanabilirsiniz, bir http eylemi veya [özel bağlayıcı](#custom). **Ise** bağlayıcıları olmayan şirket içi sistemler için şirket içi veri ağ geçidi kullanın. Kullanılabilir ıSE bağlayıcılarını gözden geçirmek için bkz. [Ise bağlayıcıları](#ise-connectors). |
| Etiket yok | ![Örnek çok kiracılı bağlayıcı](./media/apis-list/example-multi-tenant-connector.png) | **Temel** veya **Ise** etiketi olmayan diğer tüm bağlayıcılar, kullanmaya devam edebilmeniz için genel, çok kiracılı Logic Apps hizmetinde çalıştırın. |
|||

<a name="built-ins"></a>

## <a name="built-in"></a>Yerleşik

Logic Apps, zamanlama tabanlı iş akışları oluşturabilmek, mantıksal uygulamalarınızın diğer uygulama ve hizmetlerle iletişim kurmasına, Logic Apps aracılığıyla iş akışını denetlemesine ve verileri yönetmenize veya yönetmenize yardımcı olmak için yerleşik Tetikleyiciler ve eylemler sağlar.

| Ad | Açıklama |
|------|-------------|
| [![Yerleşik bağlayıcı ][schedule-icon]<br> **zamanlamasını** zamanla][schedule-doc] | - [ **Yineleme** tetikleyicisiyle][schedule-recurrence-doc]temel ve gelişmiş zamanlamalara değişen bir mantıksal uygulamayı belirtilen bir yinelenme üzerinde çalıştırın. <br>- [ **Kayan pencere** tetikleyicisiyle][schedule-sliding-window-doc]sürekli öbeklerdeki verileri işlemek için gereken bir mantıksal uygulamayı çalıştırın. <br>- [ **Gecikme** eylemini][schedule-delay-doc]kullanarak mantıksal uygulamanızı belirtilen süre için duraklatın. <br>-Mantıksal uygulamanızı, belirtilen tarih ve saate kadar [ **ertele** ][schedule-delay-until-doc]tamamlanana kadar duraklatın. |
| [![Batch yerleşik bağlayıcı ][batch-icon]<br> **toplu işi**][batch-doc] | - **Batch iletileri** tetikleyicisiyle toplu olarak ileti işleme. <br>- **Batch 'e Ileti gönder** eylemine sahip mevcut toplu iş tetikleyicilerine sahip Logic Apps 'i çağırın. |
| [![HTTP yerleşik Bağlayıcısı ][http-icon]<br> **http**][http-doc] | HTTP tetikleyicileri ve eylemleriyle HTTP veya HTTPS uç noktalarını çağırma. Http [+ Swagger yerleşik bağlayıcı][http-swagger-doc] ve [http + Web kancasını][http-webhook-doc]içeren diğer http yerleşik Tetikleyicileri ve eylemleri. |
| [![Yerleşik bağlayıcı ][http-request-icon]<br> **isteği** iste][http-request-doc] | -Mantıksal uygulamanızı diğer uygulamalardan veya hizmetlerden çağrılabilir, Event Grid Kaynak olaylarında tetikleyin veya **istek** tetikleyicisiyle Azure Güvenlik Merkezi uyarılarına yönelik yanıtlara karşı tetikleyin. <br>- **Yanıt** eylemiyle bir uygulamaya veya hizmete yanıt gönderin. |
| [![Azure API Management yerleşik bağlayıcı ][azure-api-management-icon]<br> **Azure API <br> yönetimi**][azure-api-management-doc] | Azure API Management ile yönettiğiniz ve yayımladığınız API'leriniz tarafından tanımlanan tetikleyicileri ve eylemleri çağırın. |
| [![Azure Uygulama Hizmetleri yerleşik Bağlayıcısı ][azure-app-services-icon]<br> **Azure uygulama <br> Hizmetleri**][azure-app-services-doc] | Azure App Service üzerinde barındırılan Azure API uygulamalarını veya web uygulamalarını çağırın. Bu uygulamalar tarafından tanımlanan Tetikleyiciler ve Eylemler, Swagger dahil edildiğinde diğer birinci sınıf Tetikleyicileri ve eylemleri gibi görünür. |
| [![Azure Logic Apps yerleşik bağlayıcı ][azure-logic-apps-icon]<br> **Azure Logic <br> Apps**][nested-logic-app-doc] | **İstek** tetikleyicisiyle başlayan diğer mantıksal uygulamaları çağırın. |
|||

### <a name="run-code-from-logic-apps"></a>Logic Apps 'ten kod çalıştırma

Logic Apps, mantıksal uygulamanızın iş akışında kendi kodunuzu çalıştırmaya yönelik yerleşik eylemler sağlar:

| Ad | Açıklama |
|------|-------------|
| [![Azure Işlevleri yerleşik bağlayıcı ][azure-functions-icon]<br> **Azure işlevleri**][azure-functions-doc] | Mantıksal uygulamalarınızdan özel kod parçacıkları (C# veya Node.js) çalıştıran Azure işlevlerini çağırın. |
| [![Satır içi kod yerleşik bağlayıcı ][inline-code-icon]<br> **satır kodu**][inline-code-doc] | Mantıksal uygulamalarınızdan JavaScript kod parçacıklarını ekleyin ve çalıştırın. |
|||

### <a name="control-workflow"></a>Denetim iş akışı

Logic Apps mantıksal uygulamanızın iş akışındaki eylemleri yapılandırmak ve denetlemek için yerleşik eylemler sağlar:

| Ad | Açıklama |
|------|-------------|
| [![Koşul yerleşik eylem ][condition-icon]<br> **koşulu**][condition-doc] | Koşulu değerlendirin ve koşulun doğru veya yanlış olduğunu temel alarak farklı eylemleri çalıştırın. |
| [![][for-each-icon]<br>**Her biri için** her bir yerleşik eylem için][for-each-doc] | Bir dizideki her öğe için aynı eylemleri gerçekleştirin. |
| [![Kapsam yerleşik eylem ][scope-icon]<br> **kapsamı**][scope-doc] | Kapsamdaki eylemlerin çalışmasını bitirdikten sonra kendi durumlarını içeren, eylemleri *kapsamlara*gruplandırın. |
| [![Yerleşik eylem ][switch-icon]<br> **anahtarını** Değiştir][switch-doc] | Eylemleri, varsayılan durum dışında benzersiz değerler atanmış *durumlarına*göre gruplandırın. Yalnızca atanmış değeri bir ifade, nesne veya belirteçten sonuçla eşleşen bu durumu çalıştırın. Hiçbir eşleşme yoksa, varsayılan durumu çalıştırın. |
| [![Yerleşik eylemi Sonlandır ][terminate-icon]<br> **Sonlandır**][terminate-doc] | Etkin bir şekilde çalışan mantıksal uygulama iş akışını durdurun. |
| [![Sürece yerleşik eyleme kadar ][until-icon]<br> **Until**][until-doc] | Belirtilen koşul doğru olana veya bir durum değiştirilene kadar eylemleri tekrarlayın. |
|||

### <a name="manage-or-manipulate-data"></a>Verileri yönetme veya işleme

Logic Apps, veri çıktıları ve biçimleri ile çalışmaya yönelik yerleşik eylemler sağlar:

| Ad | Açıklama |
|------|-------------|
| [![Veri Işlemleri yerleşik eylem ][data-operations-icon]<br> **verileri işlemleri**][data-operations-doc] | Verilerle işlemleri gerçekleştirin: <p>- **Oluştur**: çeşitli türlerde birden çok girişe ait tek bir çıktı oluşturun. <br>- **CSV tablosu oluştur**: JSON nesneleriyle bir diziden virgülle ayrılmış değer (CSV) tablosu oluşturun. <br>- **HTML tablosu oluştur**: JSON nesneleriyle BIR diziden HTML tablosu oluşturma. <br>- **Diziyi filtrele**: ölçütlerinizi karşılayan başka bir dizideki öğelerden bir dizi oluşturun. <br>- **Birleştir**: dizideki tüm öğelerden bir dize oluşturun ve bu öğeleri belirtilen sınırlayıcıyla ayırın. <br>- **JSON Ayrıştır**: iş akışınızda bu özellikleri kullanabilmeniz IÇIN, JSON içeriğindeki özelliklerden ve bunların değerlerinden Kullanıcı dostu belirteçler oluşturun. <br>- Şunları **seçin**: öğeleri veya değerleri başka bir dizide dönüştürerek ve bu öğeleri belirtilen ÖZELLIKLERLE eşleyerek JSON nesneleriyle bir dizi oluşturun. |
| ![Tarih saat yerleşik eylemi][date-time-icon]<br>**Tarih saat** | Zaman damgalarına sahip işlemleri gerçekleştirin: <p>- **Zamana Ekle**: belirtilen birim sayısını bir zaman damgasına ekleyin. <br>- **Saat dilimini Dönüştür**: kaynak saat dilimindeki bir zaman damgasını hedef saat dilimine dönüştürür. <br>- **Geçerli zaman**: geçerli zaman damgasını bir dize olarak döndürür. <br>- **Gelecek zamanı al**: geçerli zaman damgasını ve belirtilen zaman birimlerini döndürün. <br>- **Son saati al**: geçerli zaman damgasını belirtilen zaman birimleriyle döndürün. <br>- Şu **andan itibaren**: zaman damgasından bir dizi zaman birimi çıkar. |
| [![Değişkenler yerleşik eylem ][variables-icon]<br> **değişkenleri**][variables-doc] | Değişkenlerle işlemleri gerçekleştirin: <p>- **Dizi değişkenine Ekle**: bir değişken tarafından depolanan bir dizideki son öğe olarak bir değer ekleyin. <br>- **Dize değişkenine Ekle**: bir değişken tarafından depolanan bir dizedeki son karakter olarak bir değer ekleyin. <br>- **Değişken azaltma**: bir değişkeni sabit bir değere küçültün. <br>- **Artış değişkeni**: bir değişkeni sabit bir değerle artırın. <br>- **Değişkeni Başlat**: bir değişken oluşturun ve veri türünü ve başlangıç değerini bildirin. <br>- **Set değişkeni**: var olan bir değişkene farklı bir değer atayın. |
|||

<a name="managed-api-connectors"></a>

## <a name="managed-connectors"></a>Yönetilen bağlayıcılar

Logic Apps, bu hizmet veya sistemlerle görevleri, işlemleri ve iş akışlarını otomatikleştirmek için bu popüler standart bağlayıcıları sağlar:

| Ad | Açıklama |
|------|-------------|
| [![Azure Service Bus Managed Connector ][azure-service-bus-icon]<br> **Azure Service Bus**][azure-service-bus-doc] | Logic Apps’te en yaygın kullanılan bağlayıcı ile zaman uyumsuz iletileri, oturumları ve konu aboneliklerini yönetin. |
| [![SQL Server Managed Connector ][sql-server-icon]<br> **SQL Server**][sql-server-doc] | Kayıtları yönetebilmeniz, saklı yordamları çalıştırabilmeniz veya sorgular gerçekleştirebilmeniz için, şirket içi SQL Server veya buluttaki bir Azure SQL veritabanı 'na bağlanın. |
| [![Azure Blob depolama yönetilen Bağlayıcısı ][azure-blob-storage-icon]<br> **Azure Blob <br> depolama**][azure-blob-storage-doc] | Blob içeriğini oluşturup yönetebilmeniz için depolama hesabınıza bağlanın. |
| [![Office 365 Outlook Managed Connector ][office-365-outlook-icon]<br> **Office 365 <br> Outlook**][office-365-outlook-doc] | E-postalar, görevler, takvim olayları ve toplantılar, kişiler, istekler ve daha fazlasını oluşturabilmeniz ve yönetebilmeniz için Office 365 e-posta hesabınıza bağlanın. |
| [![SFTP-SSH Managed Connector ][sftp-ssh-icon]<br> **SFTP-SSH**][sftp-ssh-doc] | Dosya ve klasörlerle çalışabilmeniz için SSH kullanarak İnternet 'ten erişebileceğiniz SFTP sunucularına bağlanın. |
| [![SharePoint Online Managed Connector ][sharepoint-online-icon]<br> **SharePoint <br> Online**][sharepoint-online-doc] | Dosyaları, ekleri, klasörleri ve daha fazlasını yönetebilmeniz için SharePoint Online 'a bağlanın. |
| [![Azure kuyrukları yönetilen bağlayıcı ][azure-queues-icon]<br> **Azure <br> kuyrukları**][azure-queues-doc] | Kuyrukları ve iletileri oluşturabilmeniz ve yönetebilmeniz için Azure depolama hesabınıza bağlanın. |
| [![FTP yönetilen Bağlayıcısı ][ftp-icon]<br> **FTP**][ftp-doc] | Dosya ve klasörlerle çalışabilmeniz için internet 'ten erişebileceğiniz FTP sunucularına bağlanın. |
| [![Dosya sistemi tarafından yönetilen bağlayıcı ][file-system-icon]<br> **Dosya <br> sistemi**][file-system-doc] | Dosya oluşturup yönetebilmek için şirket içi dosya paylaşımınıza bağlanın. |
| [![Azure Event Hubs Managed Connector ][azure-event-hubs-icon]<br> **Azure Event Hubs**][azure-event-hubs-doc] | Olayları bir olay hub 'ı aracılığıyla tüketin ve yayımlayın. Örneğin, Event Hubs ile mantıksal uygulamanızdan çıkış alın ve ardından söz konusu çıkışı gerçek zamanlı bir analiz sağlayıcısına gönderin. |
| [![Azure Event Grid yönetilen bağlayıcı ][azure-event-grid-icon]<br> **Azure olay** <br> **Kılavuzu**][azure-event-grid-doc] | Örneğin, Azure kaynakları veya üçüncü taraf kaynakları değiştiğinde Event Grid tarafından yayımlanan olayları izleyin. |
| [![Salesforce yönetilen bağlayıcı ][salesforce-icon]<br> **Salesforce**][salesforce-doc] | Kayıtlar, işler, nesneler ve daha fazlası gibi öğeleri oluşturup yönetebilmeniz için Salesforce hesabınıza bağlanın. |
|||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Şirket içi bağlayıcılar

Şirket içi sistemlerdeki verilere ve kaynaklara erişmek için Logic Apps sağladığı yaygın olarak kullanılan bazı standart bağlayıcılar aşağıda verilmiştir. Şirket içi bir sisteme bağlantı oluşturabilmeniz için önce şirket [içi veri ağ geçidini indirmeniz, yüklemeniz ve ayarlamanız][gateway-doc]gerekir. Bu ağ geçidi, gerekli ağ altyapısını ayarlamak zorunda kalmadan güvenli bir iletişim kanalı sağlar.

:::row:::
    :::column:::
        [![BizTalk Server Bağlayıcısı ][biztalk-server-icon]<br> **BizTalk** <br> **Server**][biztalk-server-doc]
    :::column-end:::
    :::column:::
        [![Dosya sistemi Bağlayıcısı ][file-system-icon]<br> **Dosya <br> sistemi**][file-system-doc]
    :::column-end:::
    :::column:::
        [![DB2 Bağlayıcısı ][ibm-db2-icon]<br> **IBM DB2**][ibm-db2-doc]
    :::column-end:::
    :::column:::
        [![Informix Bağlayıcısı ][ibm-informix-icon]<br> **IBM** <br> **Informix**][ibm-informix-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![MySQL Bağlayıcısı ][mysql-icon]<br> **MySQL**][mysql-doc]
    :::column-end:::
    :::column:::
        [![Oracle DB Bağlayıcısı ][oracle-db-icon]<br> **Oracle DB**][oracle-db-doc]
    :::column-end:::
    :::column:::
        [![PostgreSQL Bağlayıcısı ][postgre-sql-icon]<br> **PostgreSQL**][postgre-sql-doc]
    :::column-end:::
    :::column:::
        [![SharePoint Server Bağlayıcısı ][sharepoint-server-icon]<br> **SharePoint <br> sunucusu**][sharepoint-server-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![SQL Server Bağlayıcısı ][sql-server-icon]<br> **SQL <br> Server**][sql-server-doc]
    :::column-end:::
    :::column:::
        [![Teradata bağlayıcı ][teradata-icon]<br> **Teradata**][teradata-doc]
    :::column-end:::
    :::column:::
        
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları

Logic Apps, Azure 'da Enterprise Integration Pack (EıP) aracılığıyla kullanılabilen bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)oluştururken ve ödeyerek, Logic Apps ile işletmeler arası (B2B) çözümleri oluşturmaya yönelik standart bağlayıcılar sağlar. Bu hesapla, ticaret ortakları, anlaşmalar, Haritalar, şemalar, sertifikalar vb. gibi B2B yapıtları oluşturabilir ve bunları kaydedebilirsiniz. Bu yapıtları kullanmak için, mantıksal uygulamalarınızı tümleştirme hesabınızla ilişkilendirin. Şu anda BizTalk Server kullanıyorsanız, bu bağlayıcılar zaten tanıdık görünebilir.

:::row:::
    :::column:::
        [![AS2 kod çözme eylemi ][as2-icon]<br> **AS2 <br> kod çözme**][as2-doc]
    :::column-end:::
    :::column:::
        [![AS2 Encoding eylemi ][as2-icon]<br> **AS2 <br> kodlama**][as2-doc]
    :::column-end:::
    :::column:::
        [![EDIOLGU kod çözme eylemi ][edifact-icon]<br> **ediolgu <br> kod çözme**][edifact-decode-doc]
    :::column-end:::
    :::column:::
        [![EDIOLGU kodlama eylemi ][edifact-icon]<br> **ediolgu <br> kodlaması**][edifact-encode-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Düz dosya kod çözme eylemi ][flat-file-decode-icon]<br> **düz dosya <br> kodu çözme**][flat-file-decode-doc]
    :::column-end:::
    :::column:::
        [![Düz dosya kodlama eylemi ][flat-file-encode-icon]<br> **düz dosya <br> kodlaması**][flat-file-encode-doc]
    :::column-end:::
    :::column:::
        [![Tümleştirme hesabı eylem ][integration-account-icon]<br> **tümleştirme <br> hesabı**][integration-account-doc]
    :::column-end:::
    :::column:::
        [![Sıvı dönüştürmeleri eylem ][liquid-icon]<br> **sıvı** <br> **dönüştürmeleri**][json-liquid-transform-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![X12 kod çözme eylemi ][x12-icon]<br> **x12 <br> kod çözme**][x12-decode-doc]
    :::column-end:::
    :::column:::
        [![X12 Encoding eylemi ][x12-icon]<br> **x12 <br> kodlama**][x12-encode-doc]
    :::column-end:::
    :::column:::
        [![XML dönüşümler eylem ][xml-transform-icon]<br> **XML** <br> **dönüşümleri**][xml-transform-doc]
    :::column-end:::
    :::column:::
        [![XML doğrulama eylemi ][xml-validate-icon]<br> **XML <br> doğrulaması**][xml-validate-doc]
    :::column-end:::
:::row-end:::

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Logic Apps, SAP ve IBM MQ gibi kurumsal sistemlere erişmek için bu kurumsal bağlayıcıları sağlar:

:::row:::
    :::column:::
        [![IBM 3270 Bağlayıcısı ][ibm-3270-icon]<br> **IBM 3270**][ibm-3270-doc]
    :::column-end:::
    :::column:::
        [![MQ Bağlayıcısı ][ibm-mq-icon]<br> **IBM MQ**][ibm-mq-doc]
    :::column-end:::
    :::column:::
        [![SAP Bağlayıcısı ][sap-icon]<br> **SAP**][sap-connector-doc]
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::

<a name="ise-connectors"></a>

## <a name="ise-connectors"></a>ISE bağlayıcıları

Özel bir [tümleştirme hizmeti ortamında (ISE)](#integration-service-environment)oluşturduğunuz ve çalıştırdığınız mantıksal uygulamalar Için, mantıksal uygulama Tasarımcısı, **çekirdek** etiketi kullanılarak Ise içinde çalışan yerleşik Tetikleyicileri ve eylemleri belirler. ISE 'de çalışan yönetilen bağlayıcılar **Ise** etiketini görüntülerken genel, çok kiracılı Logic Apps hizmetinde çalışan bağlayıcılar her iki etiketi de görüntülemez. Bu liste şu anda ıSE sürümlerinin bulunduğu bağlayıcıları gösterir:

:::row:::
    :::column:::
        [![AS2 ıSE Bağlayıcısı ][as2-icon]<br> **AS2**][as2-doc]
    :::column-end:::
    :::column:::
        [![Azure Otomasyonu ıSE Bağlayıcısı ][azure-automation-icon]<br> **Azure <br> Otomasyonu**][azure-automation-doc]
    :::column-end:::
    :::column:::
        [![Azure Blob Storage ıSE Bağlayıcısı ][azure-blob-storage-icon]<br> **Azure Blob <br> depolama**][azure-blob-storage-doc]
    :::column-end:::
    :::column:::
        [![Azure Cosmos DB ıSE Bağlayıcısı ][azure-cosmos-db-icon]<br> **Azure Cosmos <br> DB**][azure-cosmos-db-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Azure Event Hubs ıSE Bağlayıcısı ][azure-event-hubs-icon]<br> **Azure olay <br> hub 'ları**][azure-event-hubs-doc]
    :::column-end:::
    :::column:::
        [![Azure Event Grid ıSE Bağlayıcısı ][azure-event-grid-icon]<br> **Azure olay <br> Kılavuzu**][azure-event-grid-doc]
    :::column-end:::
    :::column:::
        [![Azure dosya depolama ıSE Bağlayıcısı ][azure-file-storage-icon]<br> **Azure dosya <br> depolama**][azure-file-storage-doc]
    :::column-end:::
    :::column:::
        [![Azure Key Vault ıSE Bağlayıcısı ][azure-key-vault-icon]<br> **Azure anahtar <br> Kasası**][azure-key-vault-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Azure Izleyici günlükleri ıSE Bağlayıcısı ][azure-monitor-logs-icon]<br> **Azure izleyici <br> günlükleri**][azure-monitor-logs-doc]
    :::column-end:::
    :::column:::
        [![Azure Service Bus ıSE Bağlayıcısı ][azure-service-bus-icon]<br> **Azure Service <br> Bus**][azure-service-bus-doc]
    :::column-end:::
    :::column:::
        [![Azure SQL veri ambarı ıSE Bağlayıcısı ][azure-sql-data-warehouse-icon]<br> **Azure SQL veri <br> ambarı**][azure-sql-data-warehouse-doc]
    :::column-end:::
    :::column:::
        [![Azure Tablo depolama ıSE Bağlayıcısı ][azure-table-storage-icon]<br> **Azure Tablo <br> depolama**][azure-table-storage-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Azure Kuyrukları ıSE Bağlayıcısı ][azure-queues-icon]<br> **Azure <br> kuyrukları**][azure-queues-doc]
    :::column-end:::
    :::column:::
        [![EDIOLGU ıSE Bağlayıcısı ][edifact-icon]<br> **ediolgu**][edifact-doc]
    :::column-end:::
    :::column:::
        [![Dosya sistemi ıSE Bağlayıcısı ][file-system-icon]<br> **Dosya <br> sistemi**][file-system-doc]
    :::column-end:::
    :::column:::
        [![FTP ıSE Bağlayıcısı ][ftp-icon]<br> **FTP**][ftp-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![IBM 3270 ıSE Bağlayıcısı ][ibm-3270-icon]<br> **IBM 3270**][ibm-3270-doc]
    :::column-end:::
    :::column:::
        [![DB2 ıSE Bağlayıcısı ][ibm-db2-icon]<br> **IBM DB2**][ibm-db2-doc]
    :::column-end:::
    :::column:::
        [![MQ ıSE Bağlayıcısı ][ibm-mq-icon]<br> **IBM MQ**][ibm-mq-doc]
    :::column-end:::
    :::column:::
        [![SAP ıSE Bağlayıcısı ][sap-icon]<br> **SAP**][sap-connector-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![SFTP-SSH ıSE Bağlayıcısı ][sftp-ssh-icon]<br> **SFTP-SSH**][sftp-ssh-doc]
    :::column-end:::
    :::column:::
        [![SMTP ıSE Bağlayıcısı ][smtp-icon]<br> **SMTP**][smtp-doc]
    :::column-end:::
    :::column:::
        [![SQL Server ıSE Bağlayıcısı ][sql-server-icon]<br> **SQL <br> Server**][sql-server-doc]
    :::column-end:::
    :::column:::
        [![X12 ıSE Bağlayıcısı ][x12-icon]<br> **x12**][x12-doc]
    :::column-end:::
:::row-end:::

Daha fazla bilgi için şu konulara bakın:

* [Azure Logic Apps Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)
* [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="triggers-actions"></a>

## <a name="triggers-and-action-types"></a>Tetikleyiciler ve eylem türleri

Bağlayıcılar, *Tetikleyiciler*, *Eylemler*veya her ikisini de sağlayabilir. *Tetikleyici* , genellikle tetikleyiciyi tetikleyen ve mantıksal uygulamanızı çalıştırmaya başladığı olayı belirten herhangi bir mantıksal uygulamadaki ilk adımdır. Örneğin, FTP Bağlayıcısı, "bir dosya eklendiğinde veya değiştirildiğinde" mantıksal uygulamanızı başlatan bir tetikleyicisine sahiptir. Bazı Tetikleyiciler, belirtilen olay veya verileri düzenli olarak denetler ve ardından belirtilen olay veya verileri algılarsa tetiklenir. Diğer Tetikleyiciler bekler, ancak belirli bir olay gerçekleştiğinde veya yeni veri kullanılabilir olduğunda anında harekete geçecektir. Tetikleyiciler ayrıca mantıksal uygulamanıza gerekli verileri de geçirir. Mantıksal uygulamanız bu verileri iş akışı boyunca okuyabilir ve kullanabilir. Örneğin, Office 365 Outlook Bağlayıcısı, "yeni bir e-posta geldiğinde" bir tetikleyicisine sahiptir. Bu, içeriği bu e-postadaki mantıksal uygulamanızın iş akışına geçirebilir.

Bir tetikleyici etkinleştirildikten sonra, Azure Logic Apps mantıksal uygulamanızın bir örneğini oluşturur ve mantıksal uygulamanızın iş akışındaki *eylemleri* çalıştırmaya başlar. Eylemler, tetikleyiciyi izleyen ve mantıksal uygulamanızın iş akışında görevleri gerçekleştiren adımlardır. Örneğin, bir SQL veritabanından müşteri verilerini alan ve daha sonraki eylemlerdeki verileri işleyen bir mantıksal uygulama oluşturabilirsiniz.

Azure Logic Apps sağladığı genel tetikleyici türleri şunlardır:

* *Yinelenme tetikleyicisi*: Bu tetikleyici belirtilen bir zamanlamaya göre çalışır ve belirli bir hizmet ya da sistemle sıkı bir şekilde ilişkilendirilmez.

* *Yoklama tetikleyicisi*: Bu tetikleyici belirtilen zamanlamaya göre belirli bir hizmeti veya sistemi düzenli olarak yoklar, yeni verileri denetler veya belirli bir olayın olup olmadığını denetler. Yeni veriler varsa veya belirli bir olay meydana gelirse, tetikleyici mantıksal uygulamanızın yeni bir örneğini oluşturur ve çalıştırır. Bu, artık giriş olarak geçirilen verileri kullanabilir.

* *Gönderim tetikleyicisi*: Bu tetikleyici, yeni verileri bekler ve dinler veya bir olayın gerçekleşmesini ister. Yeni veriler kullanılabilir olduğunda veya olay gerçekleştiğinde, tetikleyici mantıksal uygulamanızın yeni bir örneğini oluşturur ve çalıştırır. Bu, artık giriş olarak geçirilen verileri kullanabilir.

<a name="connections"></a>

## <a name="connector-configuration"></a>Bağlayıcı yapılandırması

Her bağlayıcının Tetikleyicileri ve eylemleri, yapılandırmanız için kendi özelliklerini sağlar. Birçok bağlayıcı, mantıksal uygulamanızda bir tetikleyici veya eylem kullanabilmeniz için öncelikle hedef hizmet veya sisteme bir *bağlantı* oluşturmanızı ve kimlik doğrulama kimlik bilgilerini ya da diğer yapılandırma ayrıntılarını sağlamanızı gerektirir. Örneğin, Office 365 Outlook e-posta hesabınızla erişebilmek ve bu hesaba erişebilmeniz için önce bu hesaba bir bağlantı yetkilendirmelisiniz.

Azure Active Directory (Azure AD) OAuth kullanan bağlayıcılar için bağlantı oluşturmak, erişim belirtecinizin [şifreli](../security/fundamentals/encryption-overview.md) ve güvenli bir şekilde bir Azure gizli deposunda depolandığı Office 365, Salesforce veya GitHub gibi hizmette oturum açma anlamına gelir. FTP ve SQL gibi diğer bağlayıcılar, sunucu adresi, Kullanıcı adı ve parola gibi yapılandırma ayrıntılarına sahip bir bağlantı gerektirir. Bu bağlantı yapılandırma ayrıntıları da şifrelenir ve güvenli bir şekilde depolanır. [Azure 'da şifreleme](../security/fundamentals/encryption-overview.md)hakkında daha fazla bilgi edinin.

Bağlantılar, hizmet veya sistem izin verdiği sürece hedef hizmete veya sisteme erişebilir. Office 365 ve Dynamics gibi Azure AD OAuth bağlantıları kullanan hizmetler için Azure Logic Apps erişim belirteçlerini süresiz olarak yeniler. Diğer hizmetlerde, Azure Logic Apps yenileme olmadan ne kadar süreyle bir belirteç kullanabileceği hakkında sınırlar bulunabilir. Genellikle, bazı eylemler parolanızı değiştirme gibi tüm erişim belirteçlerini geçersiz kılar.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Özel API 'Ler ve bağlayıcılar

Özel kod çalıştıran veya bağlayıcı olarak kullanılamayan API 'Leri çağırmak için, [özel API Apps oluşturarak](../logic-apps/logic-apps-create-api-app.md)Logic Apps platformunu genişletebilirsiniz. Ayrıca, bu API 'Leri Azure aboneliğinizdeki herhangi bir mantıksal uygulama için kullanılabilir hale getirmek *üzere bekleyen veya* SOAP tabanlı API 'ler için [özel bağlayıcılar oluşturabilirsiniz](../logic-apps/custom-connector-overview.md) . Özel API Apps veya bağlayıcıları herkesin Azure 'da kullanması için genel hale getirmek amacıyla, [Microsoft sertifikası için bağlayıcılar gönderebilirsiniz](/connectors/custom-connectors/submit-certification).

> [!NOTE]
> Bir [tümleştirme hizmeti ortamında (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) dağıttığınız ve çalıştırdığınız Logic Apps, bir Azure sanal ağındaki kaynaklara doğrudan erişebilir. Şirket içi veri ağ geçidini gerektiren özel bağlayıcılarınız varsa ve bu bağlayıcıları bir ıSE dışında oluşturduysanız, bir ıSE içindeki Logic Apps de bu bağlayıcıları kullanabilir.
>
> Bir ıSE içinde oluşturulan özel bağlayıcılar şirket içi veri ağ geçidi ile çalışmaz. Ancak bu bağlayıcılar, ıSE 'yi barındıran bir Azure sanal ağına bağlı şirket içi veri kaynaklarına doğrudan erişebilir. Bu nedenle, bir ıSE içindeki Logic Apps, bu kaynaklarla iletişim kurarken veri ağ geçidine ihtiyaç duymamasından kaynaklanıyor olabilir.
>
> Sesleri oluşturma hakkında daha fazla bilgi için bkz. [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="block-connections"></a>

## <a name="block-creating-connections"></a>Bağlantı oluşturmayı engelle

Kuruluşunuz Azure Logic Apps bağlayıcılarını kullanarak belirli kaynaklara bağlanmasına izin vermezse, [Azure ilkesi](../governance/policy/overview.md)kullanarak mantıksal uygulama iş akışlarında belirli bağlayıcılar için [Bu bağlantıları oluşturma özelliğini engelleyebilirsiniz](../logic-apps/block-connections-connectors.md) . Daha fazla bilgi için bkz. [Azure Logic Apps içindeki belirli bağlayıcılar tarafından oluşturulan bağlantıları engelleyin](../logic-apps/block-connections-connectors.md).

## <a name="get-ready-for-deployment"></a>Dağıtıma hazırlanın

Bir mantıksal uygulama içinden bağlantı oluşturabilirsiniz, ancak bağlantılar kendi kaynak tanımlarıyla ayrı Azure kaynaklarıdır. Bu bağlantı kaynağı tanımlarını gözden geçirmek için, [mantıksal uygulamanızı Azure 'Dan Visual Studio 'ya indirin](../logic-apps/manage-logic-apps-with-visual-studio.md). Bu, en kolay dağıtım için hazırlamış geçerli bir parametreli mantıksal uygulama şablonu oluşturmanın en kolay yoludur.

## <a name="next-steps"></a>Sonraki adımlar

* [Tam bağlayıcı listesini](/connectors) görüntüleyin
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Mantıksal uygulamalar için özel bağlayıcılar oluşturma](/connectors/custom-connectors/)
* [Mantıksal uygulamalar için özel API’ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[inline-code-icon]: ./media/apis-list/inline-code.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed connector icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-cosmos-db-icon]: ./media/apis-list/azure-cosmos-db.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-devops-icon]: ./media/apis-list/azure-devops.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-file-storage-icon]: ./media/apis-list/azure-file-storage.png
[azure-key-vault-icon]: ./media/apis-list/azure-key-vault.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-monitor-logs-icon]: ./media/apis-list/azure-monitor-logs.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[azure-sql-data-warehouse-icon]: ./media/apis-list/azure-sql-data-warehouse.png
[azure-table-storage-icon]: ./media/apis-list/azure-table-storage.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-ssh-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[twilio-icon]: ./media/apis-list/twilio.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png

<!--Other doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Şirket içi veri ağ geçidiyle mantıksal uygulamalardan şirket içi veri kaynaklarına bağlanın"

<!--Built-in doc links-->
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "API 'lerinizi yönetmek ve yayımlamak için bir Azure API Management hizmet örneği oluşturma"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-api-host-deploy-call.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Mantıksal uygulamaları Azure Işlevleri ile tümleştirme"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Gruplardaki iletileri veya toplu işlem olarak işleme"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Koşulu değerlendirin ve koşulun doğru veya yanlış olduğunu temel alarak farklı eylemleri çalıştırın"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Bir dizideki her öğe için aynı eylemleri gerçekleştirin"
[http-doc]: ./connectors-native-http.md "Mantıksal uygulamalarınızdan HTTP veya HTTPS uç noktalarını çağırma"
[http-request-doc]: ./connectors-native-reqres.md "Mantıksal uygulamalarınızda HTTP istekleri alma"
[http-response-doc]: ./connectors-native-reqres.md "Mantıksal uygulamalarınızdan gelen HTTP isteklerine yanıt verme"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Mantıksal uygulamalarınızdan REST uç noktalarını çağırma"
[http-webhook-doc]: ./connectors-native-webhook.md "HTTP veya HTTPS uç noktalarından gelen belirli olayları bekle"
[inline-code-doc]: ../logic-apps/logic-apps-add-run-inline-code.md "Mantıksal uygulamalarınızdan JavaScript kod parçacıklarını ekleyin ve çalıştırın"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Mantıksal uygulamaları iç içe geçmiş iş akışları ile tümleştirin"
[query-doc]: ../logic-apps/logic-apps-perform-data-operations.md#filter-array-action "Sorgu eylemi ile dizileri seçip filtreleyin"
[schedule-doc]: ../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md "Mantıksal uygulamaları zamanlamaya göre Çalıştır"
[schedule-delay-doc]: ./connectors-native-delay.md "Sonraki eylemi çalıştırmayı geciktir"
[schedule-delay-until-doc]: ./connectors-native-delay.md "Sonraki eylemi çalıştırmayı geciktir"
[schedule-recurrence-doc]:  ./connectors-native-recurrence.md "Mantıksal uygulamaları yinelenen bir zamanlamaya göre çalıştırma"
[schedule-sliding-window-doc]: ./connectors-native-sliding-window.md "Bitişik öbeklerdeki verileri işlemesi gereken Logic Apps 'i çalıştırma"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Gruptaki eylemler çalışmaya başladıktan sonra kendi durumlarını almak için eylemleri gruplar halinde düzenleyin"
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Eylemleri, benzersiz değerler atanmış durumlarda düzenleyin. Yalnızca değeri bir ifade, nesne veya belirteçten sonuçla eşleşen bir durum çalıştırın. Hiçbir eşleşme yoksa, varsayılan durumu çalıştırın"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Mantıksal uygulamanız için etkin olarak çalışan bir iş akışını durdurma veya iptal etme"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Belirtilen koşul doğru olana veya bir durum değiştirilene kadar eylemleri tekrarlayın"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Dizileri filtreleme veya CSV ve HTML tabloları oluşturma gibi veri işlemleri gerçekleştirin"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Başlatma, ayarlama, artırma, azaltma ve dize veya dizi değişkenine ekleme gibi değişkenlerle işlemleri gerçekleştirin"

<!--Managed connector doc links-->
[azure-automation-doc]: /connectors/azureautomation/ "Bulut ve şirket içi altyapınız için otomasyon işleri oluşturma ve yönetme"
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-cosmos-db-doc]: /connectors/documentdb/ "Belgelere ve saklı yordamlara erişebilmek için Azure Cosmos DB bağlanın"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Örneğin, Azure kaynakları veya üçüncü taraf kaynakları değiştiğinde Event Grid tarafından yayımlanan olayları izleme"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Logic Apps ile Event Hubs arasında olay alabilmeniz ve gönderebilmeniz için Azure Event Hubs 'e bağlanın"
[azure-file-storage-doc]: /connectors/azurefile/ "Dosya oluşturabilmek, güncelleştirebilmeniz, alabilmeniz ve silmeniz için Azure depolama hesabınıza bağlanın"
[azure-key-vault-doc]: /connectors/keyvault/ "Gizli dizilerinizi ve anahtarlarınızı yönetebilmeniz için Azure Key Vault bağlanın"
[azure-monitor-logs-doc]: /connectors/azuremonitorlogs/ "Log Analytics çalışma alanları ve Application Insights bileşenleri arasında Azure Izleyici günlüklerine yönelik sorguları çalıştırma"
[azure-queues-doc]: /connectors/azurequeues/ "Kuyrukları ve iletileri oluşturabilmeniz ve yönetebilmeniz için Azure depolama hesabınıza bağlanın"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Service Bus kuyruklardan ve konulardan ileti gönderin ve Service Bus kuyruklardan ve aboneliklerden ileti alın"
[azure-sql-data-warehouse-doc]: /connectors/sqldw/ "Verilerinizi görüntüleyebilmeniz için Azure SQL veri ambarı 'na bağlanın"
[azure-table-storage-doc]: /connectors/azuretables/ "Tabloları ve daha fazlasını oluşturabilmeniz, güncelleştirebilmeniz ve sorgulamanızı sağlamak için Azure depolama hesabınıza bağlanın"
[biztalk-server-doc]: /connectors/biztalk/ "BizTalk tabanlı uygulamaları Azure Logic Apps ile yan yana çalıştırabilmeniz için BizTalk Server bağlanın"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Şirket içi dosya sistemine bağlanın"
[ftp-doc]: ./connectors-create-api-ftp.md "Dosyaları karşıya yükleme, alma, silme ve diğer FTP görevleri için bir FTP / FTPS sunucusuna bağlanın"
[github-doc]: ./connectors-create-api-github.md "GitHub’a bağlanın ve sorunları izleyin"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Google takvimine bağlanır ve takvimi yönetebilir"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Sayfalarınızı değiştirebilmeniz için Google sayfalarına bağlanın"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmeniz için Google görevlerine bağlanır"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "IBM ana bilgisayarları üzerinde 3270 uygulamalarına bağlanma"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2 'ye bağlanın. Bir satırı güncelleştirin, tablo alın ve daha fazlasını yapın"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix 'e bağlanın. Bir satırı okuyun, tabloları listeleyin ve daha fazlasını yapın"
[ibm-mq-doc]: ./connectors-create-api-mq.md "İleti göndermek ve almak için IBM MQ şirket içi veya Azure 'a bağlanma"
[instagram-doc]: ./connectors-create-api-instagram.md "Instagram 'a bağlanın. Olayları tetikleme veya üzerinde işlem yapın"
[mandrill-doc]: ./connectors-create-api-mandrill.md "İletişim için Mandrill’e bağlanın"
[mysql-doc]: /connectors/mysql/ "Verileri okuyabilmeniz ve yazabilmeniz için şirket içi MySQL veritabanınıza bağlanın"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "E-posta gönderip alabilmeniz, takviminizi ve kişilerinizi yönetmek ve daha fazlasını yapmak için Office 365 hesabınıza bağlanın"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Dosyaları karşıya yükleyebilir, silebilir, listeleyebilir ve daha fazlasını yapmak için kişisel Microsoft OneDrive 'ınıza bağlanın"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Dosyalarınızı karşıya yükleyebilir, silebilir, listeleyebilir ve daha fazlasını yapmak için iş Microsoft OneDrive 'a bağlanın"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Satırları ekleyebileceğiniz, ekleyebileceğiniz, silebilmeniz ve daha fazlasını yapmak için bir Oracle veritabanına bağlanın"
[outlook.com-doc]: ./connectors-create-api-outlook.md "E-postanızı, takvimlerinizi, kişilerinizi ve daha fazlasını yönetebilmeniz için Outlook posta kutunuza bağlanın"
[postgre-sql-doc]: /connectors/postgresql/ "Tablolardan verileri okuyabilmeniz için PostgreSQL veritabanınıza bağlanın"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın. Hesapları, müşteri adaylarını, fırsatları ve daha fazlasını yönetin"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Şirket içi SAP sistemine bağlanın"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "SendGrid 'e bağlanın. E-posta gönderme ve alıcı listelerini yönetme"
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "SSH kullanarak SFTP hesabınıza bağlanın. Dosyaları karşıya yükleyin, alın, silin ve daha fazlasını yapın"
[sharepoint-server-doc]: ./connectors-create-api-sharepoint.md "SharePoint şirket içi sunucusuna bağlanın. Belgeleri, liste öğelerini ve daha fazlasını yönetin"
[sharepoint-online-doc]: ./connectors-create-api-sharepoint.md "SharePoint Online 'a bağlanın. Belgeleri, liste öğelerini ve daha fazlasını yönetin"
[slack-doc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtp-doc]: ./connectors-create-api-smtp.md "Bir SMTP sunucusuna bağlanın ve ekleri olan e-posta gönderin"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Azure SQL veritabanı 'na veya SQL Server bağlanın. SQL veritabanı tablosunda giriş oluşturma, güncelleştirme, edinme ve silme"
[teradata-doc]: /connectors/teradata/ "Tablolardaki verileri okumak için Teradata veritabanınıza bağlanın"
[twilio-doc]: ./connectors-create-api-twilio.md "Twilio 'e bağlanın. İleti gönderin ve alın, kullanılabilir numaraları alın, gelen telefon numaralarını yönetin ve daha fazlasını yapın"
[youtube-doc]: ./connectors-create-api-youtube.md "YouTube 'a bağlanın. Videolarınızı ve kanallarınızı yönetin"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "AS2 protokolünü kullanan iletileri kodlama ve kodunu çözme"
[edifact-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "EDIOLGU protokolünü kullanan iletileri kodlama ve kodunu çözme"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "EDIOLGU protokolünü kullanan iletilerin kodunu çözme"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "EDIOLGU protokolünü kullanan iletileri kodla"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise Integration düz dosyası hakkında bilgi edinin"
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise Integration düz dosyası hakkında bilgi edinin"
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Tümleştirme hesabı yapıtları için meta verileri yönetme"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Likit JSON şablonlarıyla dönüştürme"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "X12 protokolünü kullanan iletileri kodlama ve kodunu çözme"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "X12 protokolünü kullanan iletilerin kodunu çözme"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "X12 protokolünü kullanan iletileri kodla"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "XML iletilerini dönüştürme"
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "XML iletilerini doğrula"
