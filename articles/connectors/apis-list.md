---
title: Azure Logic Apps için Bağlayıcılar
description: Yerleşik, yönetilen, şirket içi, tümleştirme hesabı, ıSE ve kurumsal bağlayıcılar gibi Azure Logic Apps için bağlayıcıları olan iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 06/11/2020
ms.openlocfilehash: 7d631698f7c00a838f28d55b6b26055e686d27db
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977008"
---
# <a name="connectors-for-azure-logic-apps"></a>Azure Logic Apps için Bağlayıcılar

Bağlayıcılar; Azure Logic Apps’ten diğer uygulamalar, hizmetler, sistemler, protokoller ve platformlar arasındaki olaylara, verilere ve eylemlere hızlı erişilmesini sağlar. Mantıksal uygulamalarınızdaki bağlayıcıları kullanarak, oluşturduğunuz ve zaten sahip olduğunuz verilerle görevler gerçekleştirmek için bulut ve şirket içi uygulamalarınızın yeteneklerini genişletebilirsiniz.

Logic Apps [yüzlerce bağlayıcı](https://docs.microsoft.com/connectors)sunurken, bu makalede binlerce uygulama tarafından başarıyla kullanılan *popüler ve daha yaygın kullanılan* bağlayıcılar ve veri ve bilgi işleme için milyonlarca yürütme açıklanmaktadır. Bağlayıcıların tam listesini ve Tetikleyiciler, Eylemler ve sınırlar gibi her bağlayıcının başvuru bilgilerini bulmak için [Bağlayıcılar genel bakış](https://docs.microsoft.com/connectors)altındaki bağlayıcı başvuru sayfalarını gözden geçirin. Ayrıca [Tetikleyiciler ve eylemler](#triggers-actions), [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)ve [Logic Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/)hakkında daha fazla bilgi edinin.

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

  Bağlayıcılar, standart ya da kurumsal olarak sınıflandırılır. [Kurumsal bağlayıcılar](#enterprise-connectors) , ek bir MALIYET için SAP, IBM MQ ve IBM 3270 gibi kurumsal sistemlere erişim sağlar. Bağlayıcının standart veya kurumsal olduğunu anlamak için, [Bağlayıcılar genel bakış](https://docs.microsoft.com/connectors)altındaki her bağlayıcının başvuru sayfasındaki Teknik ayrıntılara bakın.

  Ayrıca, bazı bağlayıcılar birden çok kategori içerebilse de, bu kategorileri kullanarak bağlayıcılar tanımlayabilirsiniz. Örneğin, SAP bir kurumsal bağlayıcı ve şirket içi bağlayıcıdır:

  |   |   |
  |---|---|
  | [**Yönetilen bağlayıcılar**](#managed-api-connectors) | Azure Blob depolama, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online ve çok daha fazlası gibi hizmetleri kullanan mantıksal uygulamalar oluşturun. |
  | [**Şirket içi bağlayıcılar**](#on-premises-connectors) | Şirket [içi veri ağ geçidini][gateway-doc]yükleyip ayarladıktan sonra, bu bağlayıcılar mantıksal uygulamalarınızın SQL Server, SharePoint Server, Oracle DB, dosya paylaşımları ve diğerleri gibi şirket içi sistemlere erişmesine yardımcı olur. |
  | [**Tümleştirme hesabı bağlayıcıları**](#integration-account-connectors) | Bir tümleştirme hesabı oluştururken ve ödeme yaptığınızda, bu bağlayıcılar XML 'yi dönüştürür ve doğrular, düz dosyaları kodlayıp kodunu çözer ve AS2, EDIOLGU ve x12 protokolleriyle işletmeden işletmeye (B2B) iletileri işler. |
  |||

  > [!IMPORTANT]
  > Gmail bağlayıcısını kullanmak istiyorsanız, mantıksal uygulamalarda kısıtlama olmadan yalnızca G-Suite iş hesapları bu bağlayıcıyı kullanabilir. Gmail tüketicisi hesabınız varsa, bu bağlayıcıyı yalnızca belirli Google onaylı hizmetlerle kullanabilirsiniz veya [Gmail Bağlayıcınız ile kimlik doğrulaması için kullanmak üzere bir Google istemci uygulaması oluşturabilirsiniz](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application). Daha fazla bilgi için, bkz. [Azure Logic Apps Google bağlayıcıları Için veri güvenliği ve gizlilik ilkeleri](../connectors/connectors-google-data-security-privacy-policy.md).

<a name="integration-service-environment"></a>

### <a name="connect-from-an-integration-service-environment"></a>Tümleştirme hizmeti ortamından Bağlan

Bir Azure sanal ağındaki kaynaklara doğrudan erişmesi gereken Logic Apps için, ayrılmış kaynaklarda mantıksal uygulamalarınızı oluşturabileceğiniz, dağıtabileceğiniz ve çalıştırabileceğiniz bir yalıtılmış [tümleştirme hizmeti ortamı (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturabilirsiniz. Logic App Designer 'da, bir ıSE 'de Logic Apps için kullanmak istediğiniz bağlayıcılara gözattığınızda, yerleşik Tetikleyiciler ve eylemler üzerinde, **Ise** etiketi bazı bağlayıcılarda göründüğünde bir **çekirdek** etiket görünür:

* **Çekirdek**: Bu etiketle yerleşik Tetikleyiciler ve Eylemler, Logic Apps ile aynı Ise ile çalışır, örneğin:

  ![Örnek ıSE Bağlayıcısı](./media/apis-list/example-core-connector.png)

* **Ise**: Bu etiketle yönetilen bağlayıcılar, Logic Apps ile aynı Ise 'de çalışır, örneğin:

  ![Örnek ıSE Bağlayıcısı](./media/apis-list/example-ise-connector.png)

  Azure sanal ağına bağlı bir şirket içi sisteminiz varsa, bir ıSE, Logic Apps 'in [Şirket içi veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md)olmadan bu sisteme doğrudan erişmesini sağlar. Bunun yerine, varsa, bu sistemin **Ise** bağlayıcısını kullanabilirsiniz, bir http eylemi veya [özel bağlayıcı](#custom). **Ise** bağlayıcıları olmayan şirket içi sistemler için şirket içi veri ağ geçidi kullanın. Kullanılabilir ıSE bağlayıcılarını gözden geçirmek için bkz. [Ise bağlayıcıları](#ise-connectors).

* Kullanmaya devam edebilmeniz için **Core** veya **Ise** etiketi olmayan diğer tüm bağlayıcılar, genel, çok kiracılı Logic Apps hizmetinde çalıştırın, örneğin:

  ![Örnek çok kiracılı bağlayıcı](./media/apis-list/example-multi-tenant-connector.png)

ISE ve bağlayıcılarında çalışan Logic Apps, bu bağlayıcıların çalıştığı durumlar ne olursa olsun, tüketim tabanlı fiyatlandırma planına karşı sabit bir fiyatlandırma planını takip edin. Daha fazla bilgi için şu sayfalara bakın:

* [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)
* [Logic Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="built-ins"></a>

## <a name="built-in"></a>Yerleşik

Logic Apps, zamanlama tabanlı iş akışları oluşturabilmek, mantıksal uygulamalarınızın diğer uygulama ve hizmetlerle iletişim kurmasına, Logic Apps aracılığıyla iş akışını denetlemesine ve verileri yönetmenize veya yönetmenize yardımcı olmak için yerleşik Tetikleyiciler ve eylemler sağlar.

|   |   |   |   |
|---|---|---|---|
| [![API simge ][schedule-icon]<br> **zamanlaması**][schedule-doc] | - [ **Yineleme** tetikleyicisiyle][schedule-recurrence-doc]temel ve gelişmiş zamanlamalara değişen bir mantıksal uygulamayı belirtilen bir yinelenme üzerinde çalıştırın. <p>- [ **Kayan pencere** tetikleyicisiyle][schedule-sliding-window-doc]sürekli öbeklerdeki verileri işlemek için gereken bir mantıksal uygulamayı çalıştırın. <p>- [ **Gecikme** eylemini][schedule-delay-doc]kullanarak mantıksal uygulamanızı belirtilen süre için duraklatın. <p>-Mantıksal uygulamanızı, belirtilen tarih ve saate kadar [ **ertele** ][schedule-delay-until-doc]tamamlanana kadar duraklatın. | [![API simgesi ][batch-icon]<br> **Batch**][batch-doc] | - **Batch iletileri** tetikleyicisiyle toplu olarak ileti işleme. <p>- **Batch 'e Ileti gönder** eylemine sahip mevcut toplu iş tetikleyicilerine sahip Logic Apps 'i çağırın. |
| [![API simgesi ][http-icon]<br> **http**][http-doc] | Http veya HTTPS uç noktalarını Tetikleyiciler ve HTTP için eylemlerle çağırın. Http [+ Swagger][http-swagger-doc] ve [http + Web kancasını][http-webhook-doc]içeren diğer http yerleşik Tetikleyicileri ve eylemleri. | [![API simgesi ][http-request-icon]<br> **isteği**][http-request-doc] | -Mantıksal uygulamanızı diğer uygulamalardan veya hizmetlerden çağrılabilir, Event Grid Kaynak olaylarında tetikleyin veya **istek** tetikleyicisiyle Azure Güvenlik Merkezi uyarılarına yönelik yanıtlara karşı tetikleyin. <p>- **Yanıt** eylemiyle bir uygulamaya veya hizmete yanıt gönderin. |
| [![API simgesi ][azure-api-management-icon]<br> **Azure API <br> yönetimi**][azure-api-management-doc] | Azure API Management ile yönettiğiniz ve yayımladığınız kendi API 'leriniz tarafından tanımlanan Tetikleyicileri ve eylemleri çağırın. | [![API simgesi ][azure-app-services-icon]<br> **Azure uygulama <br> Hizmetleri**][azure-app-services-doc] | Azure App Service barındırılan Azure API Apps veya Web Apps çağırın. Bu uygulamalar tarafından tanımlanan Tetikleyiciler ve Eylemler, Swagger dahil edildiğinde diğer birinci sınıf Tetikleyicileri ve eylemleri gibi görünür.|
| [![API simgesi ][azure-logic-apps-icon]<br> **Azure Logic <br> Apps**][nested-logic-app-doc] | **İstek** tetikleyicisiyle başlayan diğer mantıksal uygulamaları çağırın. |
|||||

### <a name="run-code-from-logic-apps"></a>Logic Apps 'ten kod çalıştırma

Logic Apps, mantıksal uygulamanızın iş akışında kendi kodunuzu çalıştırmaya yönelik yerleşik eylemler sağlar:

|   |   |   |   |
|---|---|---|---|
| [![API simgesi ][azure-functions-icon]<br> **Azure işlevleri**][azure-functions-doc] | Mantıksal uygulamalarınızdan özel kod parçacıkları (C# veya Node.js) çalıştıran Azure işlevlerini çağırın. | [![API simgesi ][inline-code-icon]<br> **satır içi kod**][inline-code-doc] | Mantıksal uygulamalarınızdan JavaScript kod parçacıklarını ekleyin ve çalıştırın. |
|||||

### <a name="control-workflow"></a>Denetim iş akışı

Logic Apps mantıksal uygulamanızın iş akışındaki eylemleri yapılandırmak ve denetlemek için yerleşik eylemler sağlar:

|   |   |   |   |
|---|---|---|---|
| [![Yerleşik simge ][condition-icon]<br> **koşulu**][condition-doc] | Koşulu değerlendirin ve koşulun doğru veya yanlış olduğunu temel alarak farklı eylemleri çalıştırın. | [![][for-each-icon]<br>**Her biri Için** yerleşik simge][for-each-doc] | Bir dizideki her öğe için aynı eylemleri gerçekleştirin. |
| [![Yerleşik simge ][scope-icon]<br> **kapsamı**][scope-doc] | Kapsamdaki eylemlerin çalışmasını bitirdikten sonra kendi durumlarını içeren, eylemleri *kapsamlara*gruplandırın. | [![Yerleşik simge ][switch-icon]<br> **anahtarı**][switch-doc] | Eylemleri, varsayılan durum dışında benzersiz değerler atanmış *durumlarına*göre gruplandırın. Yalnızca atanmış değeri bir ifade, nesne veya belirteçten sonuçla eşleşen bu durumu çalıştırın. Hiçbir eşleşme yoksa, varsayılan durumu çalıştırın. |
| [![Yerleşik simge ][terminate-icon]<br> **Sonlandır**][terminate-doc] | Etkin bir şekilde çalışan mantıksal uygulama iş akışını durdurun. | [![Yerleşik simgeye ][until-icon]<br> **kadar**][until-doc] | Belirtilen koşul doğru olana veya bir durum değiştirilene kadar eylemleri tekrarlayın. |
|||||

### <a name="manage-or-manipulate-data"></a>Verileri yönetme veya işleme

Logic Apps, veri çıktıları ve biçimleri ile çalışmaya yönelik yerleşik eylemler sağlar:

|   |   |
|---|---|
| [![Yerleşik simge ][data-operations-icon]<br> **veri işlemleri**][data-operations-doc] | Verilerle işlemleri gerçekleştirin: <p>- **Oluştur**: çeşitli türlerde birden çok girişe ait tek bir çıktı oluşturun. <br>- **CSV tablosu oluştur**: JSON nesneleriyle bir diziden virgülle ayrılmış değer (CSV) tablosu oluşturun. <br>- **HTML tablosu oluştur**: JSON nesneleriyle BIR diziden HTML tablosu oluşturma. <br>- **Diziyi filtrele**: ölçütlerinizi karşılayan başka bir dizideki öğelerden bir dizi oluşturun. <br>- **Birleştir**: dizideki tüm öğelerden bir dize oluşturun ve bu öğeleri belirtilen sınırlayıcıyla ayırın. <br>- **JSON Ayrıştır**: iş akışınızda bu özellikleri kullanabilmeniz IÇIN, JSON içeriğindeki özelliklerden ve bunların değerlerinden Kullanıcı dostu belirteçler oluşturun. <br>- Şunları **seçin**: öğeleri veya değerleri başka bir dizide dönüştürerek ve bu öğeleri belirtilen ÖZELLIKLERLE eşleyerek JSON nesneleriyle bir dizi oluşturun. |
| ![Yerleşik simge][date-time-icon]<br>**Tarih saat** | Zaman damgalarına sahip işlemleri gerçekleştirin: <p>- **Zamana Ekle**: belirtilen birim sayısını bir zaman damgasına ekleyin. <br>- **Saat dilimini Dönüştür**: kaynak saat dilimindeki bir zaman damgasını hedef saat dilimine dönüştürür. <br>- **Geçerli zaman**: geçerli zaman damgasını bir dize olarak döndürür. <br>- **Gelecek zamanı al**: geçerli zaman damgasını ve belirtilen zaman birimlerini döndürün. <br>- **Son saati al**: geçerli zaman damgasını belirtilen zaman birimleriyle döndürün. <br>- Şu **andan itibaren**: zaman damgasından bir dizi zaman birimi çıkar. |
| [![Yerleşik simge ][variables-icon]<br> **değişkenleri**][variables-doc] | Değişkenlerle işlemleri gerçekleştirin: <p>- **Dizi değişkenine Ekle**: bir değişken tarafından depolanan bir dizideki son öğe olarak bir değer ekleyin. <br>- **Dize değişkenine Ekle**: bir değişken tarafından depolanan bir dizedeki son karakter olarak bir değer ekleyin. <br>- **Değişken azaltma**: bir değişkeni sabit bir değere küçültün. <br>- **Artış değişkeni**: bir değişkeni sabit bir değerle artırın. <br>- **Değişkeni Başlat**: bir değişken oluşturun ve veri türünü ve başlangıç değerini bildirin. <br>- **Set değişkeni**: var olan bir değişkene farklı bir değer atayın. |
|  |  |

<a name="managed-api-connectors"></a>

## <a name="managed-connectors"></a>Yönetilen bağlayıcılar

Logic Apps, bu hizmet veya sistemlerle görevleri, işlemleri ve iş akışlarını otomatikleştirmek için bu popüler standart bağlayıcıları sağlar:

|   |   |   |   |
|---|---|---|---|
| [![API simgesi ][azure-service-bus-icon]<br> **Azure Service Bus**][azure-service-bus-doc] | Logic Apps’te en yaygın kullanılan bağlayıcı ile zaman uyumsuz iletileri, oturumları ve konu aboneliklerini yönetin. | [![API simgesi ][sql-server-icon]<br> **SQL Server**][sql-server-doc] | Kayıtları yönetebilmeniz, saklı yordamları çalıştırabilmeniz veya sorgular gerçekleştirebilmeniz için, şirket içi SQL Server veya buluttaki bir Azure SQL veritabanı 'na bağlanın. |
| [![API simgesi ][azure-blob-storage-icon]<br> **Azure Blob <br> depolama**][azure-blob-storage-doc] | Blob içeriğini oluşturup yönetebilmeniz için depolama hesabınıza bağlanın. | [![API simgesi ][office-365-outlook-icon]<br> **Office 365 <br> Outlook**][office-365-outlook-doc] | E-postalar, görevler, takvim olayları ve toplantılar, kişiler, istekler ve daha fazlasını oluşturabilmeniz ve yönetebilmeniz için Office 365 e-posta hesabınıza bağlanın. |
| [![API simgesi ][sftp-ssh-icon]<br> **SFTP-SSH**][sftp-ssh-doc] | Dosya ve klasörlerle çalışabilmeniz için SSH kullanarak İnternet 'ten erişebileceğiniz SFTP sunucularına bağlanın. | [![API simgesi ][sharepoint-online-icon]<br> **SharePoint <br> Online**][sharepoint-online-doc] | Dosyaları, ekleri, klasörleri ve daha fazlasını yönetebilmeniz için SharePoint Online 'a bağlanın. | 
| [![API simgesi ][dynamics-365-icon]<br> **Dynamics 365 <br> **][dynamics-365-doc] | Kayıtlar, öğeler ve daha fazlasını oluşturabilmeniz ve yönetebilmeniz için Dynamics 365 hesabınıza bağlanın. | [![API simgesi ][azure-queues-icon]<br> **Azure <br> kuyrukları**][azure-queues-doc] | Kuyrukları ve iletileri oluşturabilmeniz ve yönetebilmeniz için Azure depolama hesabınıza bağlanın |
| [![API simgesi ][ftp-icon]<br> **FTP**][ftp-doc] | Dosya ve klasörlerle çalışabilmeniz için internet 'ten erişebileceğiniz FTP sunucularına bağlanın. | [![API simgesi ][file-system-icon]<br> **Dosya <br> sistemi**][file-system-doc] | Dosya oluşturup yönetebilmek için şirket içi dosya paylaşımınıza bağlanın. |
| [![API simgesi ][azure-event-hubs-icon]<br> **Azure Event Hubs**][azure-event-hubs-doc] | Olayları bir olay hub 'ı aracılığıyla tüketin ve yayımlayın. Örneğin, Event Hubs ile mantıksal uygulamanızdan çıkış alın ve ardından bu çıktıyı gerçek zamanlı bir analiz sağlayıcısına gönderin. | [![API simgesi ][azure-event-grid-icon]<br> **Azure olay** <br> **Kılavuzu**][azure-event-grid-doc] | Örneğin, Azure kaynakları veya üçüncü taraf kaynakları değiştiğinde Event Grid tarafından yayımlanan olayları izleyin. |
| [![API simgesi ][salesforce-icon]<br> **Salesforce**][salesforce-doc] | Kayıtlar, işler, nesneler ve daha fazlası gibi öğeleri oluşturup yönetebilmeniz için Salesforce hesabınıza bağlanın. | [![API simgesi ][twitter-icon]<br> **Twitter**][twitter-doc] | Aynı doldurulabilir, izleyicileri, zaman çizelgenizi ve daha fazlasını yönetebilmeniz için Twitter hesabınıza bağlanın. Tasarruf etlerinizi SQL, Excel veya SharePoint 'e kaydedin. |
|||||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Şirket içi bağlayıcılar

Şirket içi sistemlerdeki verilere ve kaynaklara erişmek için Logic Apps sağladığı yaygın olarak kullanılan bazı standart bağlayıcılar aşağıda verilmiştir. Şirket içi bir sisteme bağlantı oluşturabilmeniz için önce şirket [içi veri ağ geçidini indirmeniz, yüklemeniz ve ayarlamanız][gateway-doc]gerekir. Bu ağ geçidi, gerekli ağ altyapısını ayarlamak zorunda kalmadan güvenli bir iletişim kanalı sağlar.

|   |   |   |   |   |
|---|---|---|---|---|
| [![API simgesi ][biztalk-server-icon]<br> **BizTalk** <br> **Server**][biztalk-server-doc] | [![API simgesi ][file-system-icon]<br> **Dosya <br> sistemi**][file-system-doc] | [![API simgesi ][ibm-db2-icon]<br> **IBM DB2**][ibm-db2-doc] | [![API simgesi ][ibm-informix-icon]<br> **IBM** <br> **Informix**][ibm-informix-doc] | [![API simgesi ][mysql-icon]<br> **MySQL**][mysql-doc] |
| [![API simgesi ][oracle-db-icon]<br> **Oracle DB**][oracle-db-doc] | [![API simgesi ][postgre-sql-icon]<br> **PostgreSQL**][postgre-sql-doc] | [![API simgesi ][sharepoint-server-icon]<br> **SharePoint <br> sunucusu**][sharepoint-server-doc] | [![API simgesi ][sql-server-icon]<br> **SQL <br> Server**][sql-server-doc] | [![API simgesi ][teradata-icon]<br> **Teradata**][teradata-doc] |
|||||

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları

Logic Apps, Azure 'da Enterprise Integration Pack (EıP) aracılığıyla kullanılabilen bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)oluştururken ve ödeyerek, Logic Apps ile işletmeler arası (B2B) çözümleri oluşturmaya yönelik standart bağlayıcılar sağlar. Bu hesapla, ticaret ortakları, anlaşmalar, Haritalar, şemalar, sertifikalar vb. gibi B2B yapıtları oluşturabilir ve bunları kaydedebilirsiniz. Bu yapıtları kullanmak için, mantıksal uygulamalarınızı tümleştirme hesabınızla ilişkilendirin. Şu anda BizTalk Server kullanıyorsanız, bu bağlayıcılar zaten tanıdık görünebilir.

|   |   |   |   |
|---|---|---|---|
| [![API simgesi ][as2-icon]<br> **AS2 <br> kod çözme**][as2-doc] | [![API simgesi ][as2-icon]<br> **AS2 <br> kodlaması**][as2-doc] | [![API simgesi ][edifact-icon]<br> **ediolgu <br> kod çözme**][edifact-decode-doc] | [![API simgesi ][edifact-icon]<br> **ediolgu <br> kodlaması**][edifact-encode-doc] |
| [![API simgesi ][flat-file-decode-icon]<br> **düz dosya <br> kodu çözme**][flat-file-decode-doc] | [![API simgesi ][flat-file-encode-icon]<br> **düz dosya <br> kodlaması**][flat-file-encode-doc] | [![API simgesi ][integration-account-icon]<br> **tümleştirme <br> hesabı**][integration-account-doc] | [![API simgesi ][liquid-icon]<br> **likit** <br> **dönüşümler**][json-liquid-transform-doc] |
| [![API simgesi ][x12-icon]<br> **x12 <br> kod çözme**][x12-decode-doc] | [![API simgesi ][x12-icon]<br> **x12 <br> kodlaması**][x12-encode-doc] | [![API simgesi ][xml-transform-icon]<br> **XML** <br> **dönüşümleri**][xml-transform-doc] | [![API simgesi ][xml-validate-icon]<br> **XML <br> doğrulaması**][xml-validate-doc] |  
|||||

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Logic Apps, SAP ve IBM MQ gibi kurumsal sistemlere erişmek için bu kurumsal bağlayıcıları sağlar:

|   |   |   |
|---|---|---|
| [![API simgesi ][ibm-3270-icon]<br> **IBM 3270**][ibm-3270-doc] | [![API simgesi ][ibm-mq-icon]<br> **IBM MQ**][ibm-mq-doc] | [![API simgesi ][sap-icon]<br> **SAP**][sap-connector-doc] |
||||

<a name="ise-connectors"></a>

## <a name="ise-connectors"></a>ISE bağlayıcıları

Yalıtılmış bir [tümleştirme hizmeti ortamı (ISE)](#integration-service-environment)oluşturup çalıştırdığınız mantıksal uygulamalar Için, mantıksal uygulama Tasarımcısı, **çekirdek** etiketi kullanılarak Ise içinde çalışan yerleşik Tetikleyicileri ve eylemleri belirler. ISE 'de çalışan yönetilen bağlayıcılar **Ise** etiketini görüntülerken genel, çok kiracılı Logic Apps hizmetinde çalışan bağlayıcılar her iki etiketi de görüntülemez. Bu liste şu anda ıSE sürümlerinin bulunduğu bağlayıcıları gösterir:

|   |   |   |   |   |
|---|---|---|---|---|
[![API simgesi ][as2-icon]<br> **AS2**][as2-doc] | [![API simgesi ][azure-automation-icon]<br> **Azure <br> Otomasyonu**][azure-automation-doc] | [![API simgesi ][azure-blob-storage-icon]<br> **Azure Blob <br> depolama**][azure-blob-storage-doc] | [![API simgesi ][azure-cosmos-db-icon]<br> **Azure Cosmos <br> DB**][azure-cosmos-db-doc] | [![API simgesi ][azure-event-hubs-icon]<br> **Azure olay <br> hub 'ları**][azure-event-hubs-doc] |
[![API simgesi ][azure-event-grid-icon]<br> **Azure olay <br> Kılavuzu**][azure-event-grid-doc] | [![API simgesi ][azure-file-storage-icon]<br> **Azure dosya <br> depolama**][azure-file-storage-doc] | [![API simgesi ][azure-key-vault-icon]<br> **Azure anahtar <br> Kasası**][azure-key-vault-doc] | [![API simgesi ][azure-monitor-logs-icon]<br> **Azure izleyici <br> günlükleri**][azure-monitor-logs-doc] | [![API simgesi ][azure-service-bus-icon]<br> **Azure Service <br> Bus**][azure-service-bus-doc] |
| [![API simgesi ][azure-sql-data-warehouse-icon]<br> **Azure SQL veri <br> ambarı**][azure-sql-data-warehouse-doc] | [![API simgesi ][azure-table-storage-icon]<br> **Azure Tablo <br> depolama**][azure-table-storage-doc] | [![API simgesi ][azure-queues-icon]<br> **Azure <br> kuyrukları**][azure-queues-doc] | [![API simgesi ][edifact-icon]<br> **ediolgu**][edifact-doc] | [![API simgesi ][file-system-icon]<br> **Dosya <br> sistemi**][file-system-doc] |
| [![API simgesi ][ftp-icon]<br> **FTP**][ftp-doc] | [![API simgesi ][ibm-3270-icon]<br> **IBM 3270**][ibm-3270-doc] | [![API simgesi ][ibm-db2-icon]<br> **IBM DB2**][ibm-db2-doc] | [![API simgesi ][ibm-mq-icon]<br> **IBM MQ**][ibm-mq-doc] | [![API simgesi ][sap-icon]<br> **SAP**][sap-connector-doc] |
| [![API simgesi ][sftp-ssh-icon]<br> **SFTP-SSH**][sftp-ssh-doc] | [![API simgesi ][smtp-icon]<br> **SMTP**][smtp-doc] | [![API simgesi ][sql-server-icon]<br> **SQL <br> Server**][sql-server-doc] | [![API simgesi ][x12-icon]<br> **x12**][x12-doc] |
||||||

Daha fazla bilgi için şu konulara bakın:

* [Azure Logic Apps Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)
* [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="triggers-actions"></a>

## <a name="triggers-and-action-types"></a>Tetikleyiciler ve eylem türleri

Bağlayıcılar, *Tetikleyiciler*, *Eylemler*veya her ikisini de sağlayabilir. *Tetikleyici* , genellikle tetikleyiciyi tetikleyen ve mantıksal uygulamanızı çalıştırmaya başladığı olayı belirten herhangi bir mantıksal uygulamadaki ilk adımdır. Örneğin, FTP Bağlayıcısı, "bir dosya eklendiğinde veya değiştirildiğinde" mantıksal uygulamanızı başlatan bir tetikleyicisine sahiptir. Bazı Tetikleyiciler, belirtilen olay veya verileri düzenli olarak denetler ve ardından belirtilen olay veya verileri algılarsa tetiklenir. Diğer Tetikleyiciler bekler, ancak belirli bir olay gerçekleştiğinde veya yeni veri kullanılabilir olduğunda anında harekete geçecektir. Tetikleyiciler ayrıca mantıksal uygulamanıza gerekli verileri de geçirir. Mantıksal uygulamanız bu verileri iş akışı boyunca okuyabilir ve kullanabilir. Örneğin, Twitter Bağlayıcısı, "yeni bir tweet gönderildiğinde" bir tetikleyicisine sahiptir. Bu, Tweet içeriğini mantıksal uygulamanızın iş akışına geçirir.

Bir tetikleyici etkinleştirildikten sonra, Azure Logic Apps mantıksal uygulamanızın bir örneğini oluşturur ve mantıksal uygulamanızın iş akışındaki *eylemleri* çalıştırmaya başlar. Eylemler, tetikleyiciyi izleyen ve mantıksal uygulamanızın iş akışında görevleri gerçekleştiren adımlardır. Örneğin, bir SQL veritabanından müşteri verilerini alan ve daha sonraki eylemlerdeki verileri işleyen bir mantıksal uygulama oluşturabilirsiniz.

Azure Logic Apps sağladığı genel tetikleyici türleri şunlardır:

* *Yinelenme tetikleyicisi*: Bu tetikleyici belirtilen bir zamanlamaya göre çalışır ve belirli bir hizmet ya da sistemle sıkı bir şekilde ilişkilendirilmez.

* *Yoklama tetikleyicisi*: Bu tetikleyici belirtilen zamanlamaya göre belirli bir hizmeti veya sistemi düzenli olarak yoklar, yeni verileri denetler veya belirli bir olayın olup olmadığını denetler. Yeni veriler varsa veya belirli bir olay meydana gelirse, tetikleyici mantıksal uygulamanızın yeni bir örneğini oluşturur ve çalıştırır. Bu, artık giriş olarak geçirilen verileri kullanabilir.

* *Gönderim tetikleyicisi*: Bu tetikleyici, yeni verileri bekler ve dinler veya bir olayın gerçekleşmesini ister. Yeni veriler kullanılabilir olduğunda veya olay gerçekleştiğinde, tetikleyici mantıksal uygulamanızın yeni bir örneğini oluşturur ve çalıştırır. Bu, artık giriş olarak geçirilen verileri kullanabilir.

<a name="connections"></a>

## <a name="connector-configuration"></a>Bağlayıcı yapılandırması

Her bağlayıcının Tetikleyicileri ve eylemleri, yapılandırmanız için kendi özelliklerini sağlar. Birçok bağlayıcı, mantıksal uygulamanızda bir tetikleyici veya eylem kullanabilmeniz için öncelikle hedef hizmet veya sisteme bir *bağlantı* oluşturmanızı ve kimlik doğrulama kimlik bilgilerini ya da diğer yapılandırma ayrıntılarını sağlamanızı gerektirir. Örneğin, verilere erişim veya sizin adınıza gönderi için Twitter hesabına bir bağlantı yetkilendirmelisiniz.

Azure Active Directory (Azure AD) OAuth kullanan bağlayıcılar için bağlantı oluşturmak, erişim belirtecinizin [şifreli](../security/fundamentals/encryption-overview.md) ve güvenli bir şekilde bir Azure gizli deposunda depolandığı Office 365, Salesforce veya GitHub gibi hizmette oturum açma anlamına gelir. FTP ve SQL gibi diğer bağlayıcılar, sunucu adresi, Kullanıcı adı ve parola gibi yapılandırma ayrıntılarına sahip bir bağlantı gerektirir. Bu bağlantı yapılandırma ayrıntıları da şifrelenir ve güvenli bir şekilde depolanır. [Azure 'da şifreleme](../security/fundamentals/encryption-overview.md)hakkında daha fazla bilgi edinin.

Bağlantılar, hizmet veya sistem izin verdiği sürece hedef hizmete veya sisteme erişebilir. Office 365 ve Dynamics gibi Azure AD OAuth bağlantıları kullanan hizmetler için Azure Logic Apps erişim belirteçlerini süresiz olarak yeniler. Diğer hizmetlerde, Azure Logic Apps yenileme olmadan ne kadar süreyle bir belirteç kullanabileceği hakkında sınırlar bulunabilir. Genellikle, bazı eylemler parolanızı değiştirme gibi tüm erişim belirteçlerini geçersiz kılar.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Özel API 'Ler ve bağlayıcılar

Özel kod çalıştıran veya bağlayıcı olarak kullanılamayan API 'Leri çağırmak için, [özel API Apps oluşturarak](../logic-apps/logic-apps-create-api-app.md)Logic Apps platformunu genişletebilirsiniz. Ayrıca, bu API 'Leri Azure aboneliğinizdeki herhangi bir mantıksal uygulama için kullanılabilir hale getirmek *üzere bekleyen veya* SOAP tabanlı API 'ler için [özel bağlayıcılar oluşturabilirsiniz](../logic-apps/custom-connector-overview.md) . Özel API Apps veya bağlayıcıları herkesin Azure 'da kullanması için genel hale getirmek amacıyla, [Microsoft sertifikası için bağlayıcılar gönderebilirsiniz](../logic-apps/custom-connector-submit-certification.md).

> [!NOTE]
> Bir [tümleştirme hizmeti ortamında (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) dağıttığınız ve çalıştırdığınız Logic Apps, bir Azure sanal ağındaki kaynaklara doğrudan erişebilir. Şirket içi veri ağ geçidini gerektiren özel bağlayıcılarınız varsa ve bu bağlayıcıları bir ıSE dışında oluşturduysanız, bir ıSE içindeki Logic Apps de bu bağlayıcıları kullanabilir.
>
> Bir ıSE içinde oluşturulan özel bağlayıcılar şirket içi veri ağ geçidi ile çalışmaz. Ancak bu bağlayıcılar, ıSE 'yi barındıran bir Azure sanal ağına bağlı şirket içi veri kaynaklarına doğrudan erişebilir. Bu nedenle, bir ıSE içindeki Logic Apps, bu kaynaklarla iletişim kurarken veri ağ geçidine ihtiyaç duymamasından kaynaklanıyor olabilir.
>
> Sesleri oluşturma hakkında daha fazla bilgi için bkz. [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="block-connections"></a>

## <a name="block-creating-connections"></a>Bağlantı oluşturmayı engelle

Kuruluşunuz Azure Logic Apps bağlayıcılarını kullanarak belirli kaynaklara bağlanmasına izin vermezse, [Azure ilkesi](../governance/policy/overview.md)kullanarak mantıksal uygulama iş akışlarında belirli bağlayıcılar için [Bu bağlantıları oluşturma özelliğini engelleyebilirsiniz](../logic-apps/block-connections-connectors.md) . Daha fazla bilgi için bkz. [Azure Logic Apps içindeki belirli bağlayıcılar tarafından oluşturulan bağlantıları engelleyin](../logic-apps/block-connections-connectors.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Tam bağlayıcı listesini](https://docs.microsoft.com/connectors) görüntüleyin
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Mantıksal uygulamalar için özel bağlayıcılar oluşturma](https://docs.microsoft.com/connectors/custom-connectors/)
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
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
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
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
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
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[yammer-icon]: ./media/apis-list/yammer.png
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
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
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
[azure-automation-doc]: https://docs.microsoft.com/connectors/azureautomation/ "Bulut ve şirket içi altyapınız için otomasyon işleri oluşturma ve yönetme"
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-cosmos-db-doc]: https://docs.microsoft.com/connectors/documentdb/ "Belgelere ve saklı yordamlara erişebilmek için Azure Cosmos DB bağlanın"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Örneğin, Azure kaynakları veya üçüncü taraf kaynakları değiştiğinde Event Grid tarafından yayımlanan olayları izleme"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Logic Apps ile Event Hubs arasında olay alabilmeniz ve gönderebilmeniz için Azure Event Hubs 'e bağlanın"
[azure-file-storage-doc]: https://docs.microsoft.com/connectors/azurefile/ "Dosya oluşturabilmek, güncelleştirebilmeniz, alabilmeniz ve silmeniz için Azure depolama hesabınıza bağlanın"
[azure-key-vault-doc]: https://docs.microsoft.com/connectors/keyvault/ "Gizli dizilerinizi ve anahtarlarınızı yönetebilmeniz için Azure Key Vault bağlanın"
[azure-monitor-logs-doc]: https://docs.microsoft.com/connectors/azuremonitorlogs/ "Log Analytics çalışma alanları ve Application Insights bileşenleri arasında Azure Izleyici günlüklerine yönelik sorguları çalıştırma"
[azure-queues-doc]: https://docs.microsoft.com/connectors/azurequeues/ "Kuyrukları ve iletileri oluşturabilmeniz ve yönetebilmeniz için Azure depolama hesabınıza bağlanın"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Service Bus kuyruklardan ve konulardan ileti gönderin ve Service Bus kuyruklardan ve aboneliklerden ileti alın"
[azure-sql-data-warehouse-doc]: https://docs.microsoft.com/connectors/sqldw/ "Verilerinizi görüntüleyebilmeniz için Azure SQL veri ambarı 'na bağlanın"
[azure-table-storage-doc]: https://docs.microsoft.com/connectors/azuretables/ "Tabloları ve daha fazlasını oluşturabilmeniz, güncelleştirebilmeniz ve sorgulamanızı sağlamak için Azure depolama hesabınıza bağlanın"
[biztalk-server-doc]: https://docs.microsoft.com/connectors/biztalk/ "BizTalk tabanlı uygulamaları Azure Logic Apps ile yan yana çalıştırabilmeniz için BizTalk Server bağlanın"
[box-doc]: ./connectors-create-api-box.md "Kutuya Bağlan. Karşıya yükleyin, alın, silin, dosyalarınızı listeleyin ve daha fazlasını yapın"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Dropbox 'a bağlanın. Karşıya yükleyin, alın, silin, dosyalarınızı listeleyin ve daha fazlasını yapın"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "CRM Online verileriyle çalışabilmeniz için Dynamics CRM Online 'a bağlanın"
[facebook-doc]: ./connectors-create-api-facebook.md "Facebook 'a bağlanın. Bir zaman çizelgesinde gönderi yapın, sayfa akışı alın ve daha fazlasını yapın"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Şirket içi dosya sistemine bağlanın"
[ftp-doc]: ./connectors-create-api-ftp.md "Dosyaları karşıya yükleme, alma, silme ve diğer FTP görevleri için bir FTP / FTPS sunucusuna bağlanın"
[github-doc]: ./connectors-create-api-github.md "GitHub’a bağlanın ve sorunları izleyin"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Google takvimine bağlanır ve takvimi yönetebilir"
[google-drive-doc]: ./connectors-create-api-googledrive.md "Verilerinize çalışabilmeniz için GoogleDrive 'e bağlanın"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Sayfalarınızı değiştirebilmeniz için Google sayfalarına bağlanın"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmeniz için Google görevlerine bağlanır"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "IBM ana bilgisayarları üzerinde 3270 uygulamalarına bağlanma"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2 'ye bağlanın. Bir satırı güncelleştirin, tablo alın ve daha fazlasını yapın"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix 'e bağlanın. Bir satırı okuyun, tabloları listeleyin ve daha fazlasını yapın"
[ibm-mq-doc]: ./connectors-create-api-mq.md "İleti göndermek ve almak için IBM MQ şirket içi veya Azure 'a bağlanma"
[instagram-doc]: ./connectors-create-api-instagram.md "Instagram 'a bağlanın. Olayları tetikleme veya üzerinde işlem yapın"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "MailChimp hesabınıza bağlanın. Postaları yönetin ve otomatikleştirin"
[mandrill-doc]: ./connectors-create-api-mandrill.md "İletişim için Mandrill’e bağlanın"
[mysql-doc]: https://docs.microsoft.com/connectors/mysql/ "Verileri okuyabilmeniz ve yazabilmeniz için şirket içi MySQL veritabanınıza bağlanın"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "E-posta gönderip alabilmeniz, takviminizi ve kişilerinizi yönetmek ve daha fazlasını yapmak için Office 365 hesabınıza bağlanın"
[office-365-users-doc]: ./connectors-create-api-office365-users.md
[onedrive-doc]: ./connectors-create-api-onedrive.md "Dosyaları karşıya yükleyebilir, silebilir, listeleyebilir ve daha fazlasını yapmak için kişisel Microsoft OneDrive 'ınıza bağlanın"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Dosyalarınızı karşıya yükleyebilir, silebilir, listeleyebilir ve daha fazlasını yapmak için iş Microsoft OneDrive 'a bağlanın"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Satırları ekleyebileceğiniz, ekleyebileceğiniz, silebilmeniz ve daha fazlasını yapmak için bir Oracle veritabanına bağlanın"
[outlook.com-doc]: ./connectors-create-api-outlook.md "E-postanızı, takvimlerinizi, kişilerinizi ve daha fazlasını yönetebilmeniz için Outlook posta kutunuza bağlanın"
[postgre-sql-doc]: https://docs.microsoft.com/connectors/postgresql/ "Tablolardan verileri okuyabilmeniz için PostgreSQL veritabanınıza bağlanın"
[project-online-doc]: ./connectors-create-api-projectonline.md "Projelerinizi, görevlerinizi, kaynaklarınızı ve daha fazlasını yönetebilmeniz için Microsoft Project Online 'a bağlanın"
[rss-doc]: ./connectors-create-api-rss.md "Bir RSS akışına yeni bir öğe yayımlandığında akış öğelerini yayımlama ve alma, işlemleri tetikleme"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın. Hesapları, müşteri adaylarını, fırsatları ve daha fazlasını yönetin"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Şirket içi SAP sistemine bağlanın"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "SendGrid 'e bağlanın. E-posta gönderme ve alıcı listelerini yönetme"
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "SSH kullanarak SFTP hesabınıza bağlanın. Dosyaları karşıya yükleyin, alın, silin ve daha fazlasını yapın"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "SharePoint şirket içi sunucusuna bağlanın. Belgeleri, liste öğelerini ve daha fazlasını yönetin"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "SharePoint Online 'a bağlanın. Belgeleri, liste öğelerini ve daha fazlasını yönetin"
[slack-doc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtp-doc]: ./connectors-create-api-smtp.md "Bir SMTP sunucusuna bağlanın ve ekleri olan e-posta gönderin"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Azure SQL veritabanı 'na veya SQL Server bağlanın. SQL veritabanı tablosunda giriş oluşturma, güncelleştirme, edinme ve silme"
[teradata-doc]: https://docs.microsoft.com/connectors/teradata/ "Tablolardaki verileri okumak için Teradata veritabanınıza bağlanın"
[trello-doc]: ./connectors-create-api-trello.md "Trello 'a bağlanın. Projelerinizi yönetin ve herkesle her şeyi düzenleyin"
[twilio-doc]: ./connectors-create-api-twilio.md "Twilio 'e bağlanın. İleti gönderin ve alın, kullanılabilir numaraları alın, gelen telefon numaralarını yönetin ve daha fazlasını yapın"
[twitter-doc]: ./connectors-create-api-twitter.md "Twitter 'a bağlanın. Zaman çizelgelerini alın, daha fazla nakil yapın ve daha fazlasını yapın"
[yammer-doc]: ./connectors-create-api-yammer.md "Yammer 'a bağlanın. İleti gönderin, yeni iletiler alın ve daha fazlasını yapın"
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