---
title: Azure Logic Apps için Bağlayıcılar
description: Yerleşik, yönetilen, şirket içi, tümleştirme hesabı, Ise ve kurumsal bağlayıcılar gibi Azure Mantık Uygulamaları için konektörlerle iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 03/05/2020
ms.openlocfilehash: 3010f3c99a5b214c2503f890321cbb73427e3c20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247338"
---
# <a name="connectors-for-azure-logic-apps"></a>Azure Logic Apps için Bağlayıcılar

Bağlayıcılar; Azure Logic Apps’ten diğer uygulamalar, hizmetler, sistemler, protokoller ve platformlar arasındaki olaylara, verilere ve eylemlere hızlı erişilmesini sağlar. Mantıksal uygulamalarınızdaki bağlayıcıları kullanarak, oluşturduğunuz ve zaten sahip olduğunuz verilerle görevler gerçekleştirmek için bulut ve şirket içi uygulamalarınızın yeteneklerini genişletebilirsiniz.

Logic Apps [yüzlerce bağlayıcı](https://docs.microsoft.com/connectors)sunarken, bu makalede, binlerce uygulama ve veri ve bilgi işlemek için milyonlarca yürütme tarafından başarıyla kullanılan popüler ve *daha yaygın olarak kullanılan* bağlayıcılar açıklanmaktadır. Tetikleyiciler, eylemler ve sınırlar gibi bağlayıcıların tam listesini ve her bağlayıcının başvuru bilgilerini bulmak için [Bağlayıcılar'a genel bakış](https://docs.microsoft.com/connectors)altında bağlayıcı başvuru sayfalarını gözden geçirin. Ayrıca, [tetikleyiciler ve eylemler](#triggers-actions)hakkında daha fazla bilgi edinin, [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md), ve Logic [Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/).

> [!TIP]
> Bağlayıcısı olmayan bir hizmet veya API ile tümleştirmek için, hizmeti doğrudan HTTP gibi bir protokol üzerinden arayabilir veya özel bir [bağlayıcı](#custom)oluşturabilirsiniz.

## <a name="connector-types"></a>Konektör türleri

Konektörler yerleşik tetikleyiciler ve eylemler veya yönetilen bağlayıcılar olarak kullanılabilir.

<a name="built-in"></a>

* [**Yerleşik**](#built-ins): Yerleşik tetikleyiciler ve eylemler Azure Logic Apps'a "yerel" dir ve bu görevleri mantıksal uygulamalarınız için gerçekleştirmenize yardımcı olur:

  * Özel ve gelişmiş zamanlamalarla çalıştırın.

  * Mantık uygulamanızın iş akışını( örneğin döngüler ve koşullar) düzenleyin ve kontrol edin ve değişkenler ve veri işlemleriyle çalışın.

  * Diğer uç noktalarla iletişim kurun.

  * İstekleri alın ve yanıtlayın.

  * Azure işlevlerini, Azure API Uygulamalarını (Web Apps), Azure API Yönetimi ile yönetilen ve yayınlanan kendi API'lerinizi ve istek alabilen iç içe geçmemantığı uygulamalarını arayın.

<a name="managed-connectors"></a>

* [**Yönetilen bağlayıcılar**](#managed-api-connectors): Microsoft tarafından dağıtılan ve yönetilen bu bağlayıcılar, Bulut hizmetlerine, şirket içi sistemlere veya Office 365, Azure Blob Depolama, SQL Server, Dynamics, Salesforce, SharePoint ve daha fazlası dahil olmak üzere her ikisine erişmek için tetikleyiciler ve eylemler sağlar. Bazı bağlayıcılar özellikle işletmelerarası (B2B) iletişim senaryolarını destekler ve mantık uygulamanızla bağlantılı bir [entegrasyon hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) gerektirir. Belirli bağlayıcıları kullanmadan önce Azure Logic Apps tarafından yönetilen bağlantılar oluşturmanız gerekebilir.

  Örneğin, Microsoft BizTalk Server kullanıyorsanız, mantıksal uygulamalarınız [BizTalk Server'A şirket içi konektörü](#on-premises-connectors)kullanarak BizTalk Server'ınıza bağlanabilir ve onlarla iletişim kurabilir. Daha sonra [entegrasyon hesabı bağlayıcılarını](#integration-account-connectors)kullanarak mantık uygulamalarınızda BizTalk benzeri işlemleri genişletebilir veya gerçekleştirebilirsiniz.

  Konektörler Standart veya Kurumsal olarak sınıflandırılır. [Kurumsal konektörler](#enterprise-connectors) ek bir ücret karşılığında SAP, IBM MQ ve IBM 3270 gibi kurumsal sistemlere erişim sağlar. Bağlayıcının Standart mı yoksa Kurumsal mı olduğunu belirlemek için, [Bağlayıcılar'a genel bakış](https://docs.microsoft.com/connectors)altında her bağlayıcının referans sayfasındaki teknik ayrıntılara bakın.

  Bazı bağlayıcılar birden çok kategoriyi geçebilse de, bu kategorileri kullanarak bağlayıcıları da tanımlayabilirsiniz. Örneğin, SAP bir Kurumsal bağlayıcı ve şirket içi bağlayıcıdır:

  |   |   |
  |---|---|
  | [**Yönetilen konektörler**](#managed-api-connectors) | Azure Blob Depolama, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online ve daha birçok hizmeti kullanan mantık uygulamaları oluşturun. |
  | [**Şirket içi bağlayıcılar**](#on-premises-connectors) | [Şirket içi veri ağ geçidini][gateway-doc]yükledikten ve kurduktan sonra, bu bağlayıcılar mantık uygulamalarınızın SQL Server, SharePoint Server, Oracle DB, dosya paylaşımları ve diğerleri gibi şirket içi sistemlere erişmelerine yardımcı olur. |
  | [**Tümleştirme hesabı bağlayıcıları**](#integration-account-connectors) | Bir tümleştirme hesabı oluşturduğunuzda ve ödeme yaptığınızda, bu bağlayıcılar XML'i dönüştürür ve doğrular, düz dosyaları kodlar ve kodlar ve AS2, EDIFACT ve X12 protokolleriyle işletmeler arası (B2B) iletileri işler. |
  |||

<a name="integration-service-environment"></a>

### <a name="connect-from-an-integration-service-environment"></a>Entegrasyon hizmeti ortamından bağlanma

Bir Azure sanal ağındaki kaynaklara doğrudan erişmeye ihtiyaç duyan mantıksal uygulamalar için, mantık uygulamalarınızı özel kaynaklarda oluşturabileceğiniz, dağıtabileceğiniz ve çalıştırabileceğiniz yalıtılmış bir [tümleştirme hizmeti ortamı (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturabilirsiniz. Logic App Designer'da, Bir İmKB'de mantık uygulamaları için kullanmak istediğiniz konektörlere göz attığınızda, yerleşik tetikleyicilerde ve eylemlerde **CORE** etiketi görünürken, **ISE** etiketi bazı bağlayıcılarda görünür:

* **CORE**: Bu etikete sahip yerleşik tetikleyiciler ve eylemler, örneğin mantık uygulamalarınızla aynı İmKB'de çalışır:

  ![Örnek ISE konektörü](./media/apis-list/example-core-connector.png)

* **Ise**: Bu etikete sahip yönetilen konektörler, örneğin, mantık uygulamalarınız ile aynı İmKB'de çalışır:

  ![Örnek ISE konektörü](./media/apis-list/example-ise-connector.png)

  Bir Azure sanal ağına bağlı bir şirket içi sisteminiz varsa, Bir İmKB, mantık uygulamalarınızın [şirket içi veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md)olmadan bu sisteme doğrudan erişmesini sağlar. Bunun yerine, varsa bu sistemin **ISE** konektörünü, bir HTTP eylemini veya özel bir [bağlayıcıyı](#custom)kullanabilirsiniz. **İmKB** konektörleri olmayan şirket içi sistemler için şirket içi veri ağ geçidi kullanın. Kullanılabilir Ise konektörlerini incelemek için [Ise konektörlerine](#ise-connectors)bakın.

* Kullanmaya devam edebilirsiniz **CORE** veya **ISE** etiketi olmayan diğer tüm bağlayıcılar, örneğin, küresel, çok kiracılı Logic Apps hizmetinde çalışır:

  ![Örnek çok kiracılı bağlayıcı](./media/apis-list/example-multi-tenant-connector.png)

Bir İmKB'de çalışan mantıksal uygulamalar ve bunların bağlayıcıları, bu bağlayıcılar nerede çalışırsa çalıştırın, tüketim tabanlı fiyatlandırma planına göre sabit bir fiyatlandırma planı izler. Daha fazla bilgi için şu sayfalara bakın:

* [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)
* [Logic Apps fiyatlandırma detayları](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Azure Logic Apps'tan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="built-ins"></a>

## <a name="built-in"></a>Yerleşik

Logic Apps, zamanlama tabanlı iş akışları oluşturabilmeniz, mantık uygulamalarınızın diğer uygulamalar ve hizmetlerle iletişim kurmasına, mantık uygulamalarınız aracılığıyla iş akışını kontrol etmesine ve verileri yönetebilmeniz veya değiştirebilmeniz için yerleşik tetikleyiciler ve eylemler sağlar.

|   |   |   |   |
|---|---|---|---|
| [![API][schedule-icon]<br>simgesi**Zamanlama**][schedule-doc] | - [ **Yinelenen** tetikleyici][schedule-recurrence-doc]ile temelden gelişmiş zamanlamalara kadar belirli bir yinelemede bir mantık uygulaması çalıştırın. <p>- [ **Sürgülü Pencere** tetikleyicisi][schedule-sliding-window-doc]ile verileri sürekli parçalar halinde işlemesi gereken bir mantık uygulaması çalıştırın. <p>- [ **Gecikme** eylemi][schedule-delay-doc]ile mantık uygulamanızı belirli bir süre duraklatın. <p>- Mantık uygulamanızı belirtilen tarih ve saate kadar duraklatın, [ **gecikme ile eyleme kadar.** ][schedule-delay-until-doc] | [![API][batch-icon]<br>simgesi**Toplu İşlem**][batch-doc] | - **Toplu Iş iletileri** tetikleyicisi ile toplu olarak iletileri işleme. <p>- **Toplu eyleme ileti gönder** ile varolan toplu iş tetikleyicileri olan mantık uygulamalarını arayın. |
| [![API][http-icon]<br>simgesi**HTTP**][http-doc] | HTTP için tetikleyiciler ve eylemlerle HTTP veya HTTPS uç noktalarını arayın. Diğer HTTP yerleşik tetikleyiciler ve eylemler [http + Swagger][http-swagger-doc] ve [HTTP + Webhook][http-webhook-doc]içerir. | [![API][http-request-icon]<br>simgesi**İstek**][http-request-doc] | - Mantık uygulamanızı diğer uygulamalardan veya hizmetlerden çağrılabilir hale getirin, Olay Ağı kaynak olaylarını tetikleyin veya **İstek** tetikleyicisiyle Azure Güvenlik Merkezi uyarılarına verilen yanıtları tetikleyin. <p>- **Yanıt** eylemiyle bir uygulamaya veya hizmete yanıt gönderin. |
| [![API][azure-api-management-icon]<br>simgesi**Azure <br>API Yönetimi**][azure-api-management-doc] | Azure API Yönetimi ile yönettiğiniz ve yayımladığunuzda kendi API'leriniz tarafından tanımlanan tetikleyicileri ve eylemleri arayın. | [![API][azure-app-services-icon]<br>simgesi**Azure Uygulama <br>Hizmetleri**][azure-app-services-doc] | Azure Uygulama Hizmeti'nde barındırılan Azure API Uygulamalarını veya Web Uygulamalarını arayın. Bu uygulamalar tarafından tanımlanan tetikleyiciler ve eylemler, Swagger dahil edildiğinde diğer birinci sınıf tetikleyiciler ve eylemler gibi görünür.|
| [![API][azure-logic-apps-icon]<br>simgesi**Azure Mantık <br>Uygulamaları**][nested-logic-app-doc] | **İstek** tetikleyicisiyle başlayan diğer mantık uygulamalarını arayın. |
|||||

### <a name="run-code-from-logic-apps"></a>Mantık uygulamalarından kod çalıştırma

Logic Apps, mantık uygulamanızın iş akışında kendi kodunuzu çalıştırmak için yerleşik eylemler sağlar:

|   |   |   |   |
|---|---|---|---|
| [![API][azure-functions-icon]<br>simgesi**Azure Fonksiyonları**][azure-functions-doc] | Mantık uygulamalarınızdan özel kod parçacıkları (C# veya Node.js) çalıştıran Azure işlevlerini arayın. | [![API][inline-code-icon]<br>simgesi**Satır Kodu**][azure-functions-doc] | Mantık uygulamalarınızdan JavaScript kod parçacıkları ekleyin ve çalıştırın. |
|||||

### <a name="control-workflow"></a>İş akışını kontrol edin

Logic Apps, mantık uygulamanızın iş akışındaki eylemleri yapılandırmak ve denetlemek için yerleşik eylemler sağlar:

|   |   |   |   |
|---|---|---|---|
| [![Yerleşik Simge][condition-icon]<br>**Durumu**][condition-doc] | Bir koşulu değerlendirin ve koşulun doğru veya yanlış olup olmadığına bağlı olarak farklı eylemler çalıştırın. | [![Her biri][for-each-icon]<br>**için** yerleşik simge][for-each-doc] | Bir dizideki her öğede aynı eylemleri gerçekleştirin. |
| [![Yerleşik Simge][scope-icon]<br>**Kapsamı**][scope-doc] | Kapsamdaki eylemleri tamamladıktan sonra kendi durumunu alan *kapsamlara*grubu eylemler. | [![Dahili Simge][switch-icon]<br>**Anahtarı**][switch-doc] | Eylemleri, varsayılan servis talebi dışında benzersiz değerler atanan *servis taleplerine*gruplandırma. Yalnızca atanan değeri bir ifade, nesne veya belirteç sonucuyla eşleşen bu durumu çalıştırın. Eşleşme yoksa, varsayılan servis talebinin çalıştırın. |
| [![Yerleşik Simge][terminate-icon]<br>**Sonlandırma**][terminate-doc] | Etkin olarak çalışan bir mantık uygulaması iş akışını durdurun. | [![Dahili Simge][until-icon]<br>**Kadar**][until-doc] | Belirtilen koşul doğru olana veya bazı durum değiştirilene kadar eylemleri yineleyin. |
|||||

### <a name="manage-or-manipulate-data"></a>Verileri yönetme veya işleme

Logic Apps, veri çıktıları ve biçimleriyle çalışmak için yerleşik eylemler sağlar:

|   |   |
|---|---|
| [![Yerleşik Simge][data-operations-icon]<br>**Veri İşlemleri**][data-operations-doc] | İşlemleri verilerle gerçekleştirin: <p>- **Beste**: Çeşitli türleri ile birden fazla girdiden tek bir çıkış oluşturun. <br>- **CSV tablosu oluşturma**: JSON nesneleri olan bir diziden virgülle ayrılmış değer (CSV) tablosu oluşturun. <br>- **HTML tablosu oluşturma**: JSON nesneleri olan bir diziden bir HTML tablosu oluşturun. <br>- **Filtre dizisi**: Ölçütlerinizi karşılayan başka bir dizideki öğelerden bir dizi oluşturun. <br>- **Join**: Bir dizideki tüm öğelerden bir dize oluşturun ve bu öğeleri belirtilen sınırlandırıcıile ayırın. <br>- **Ayrıştırıcı JSON**: İş akışınızda bu özellikleri kullanabilmeniz için JSON içeriğindeki özelliklerden ve onların değerlerinden kullanıcı dostu belirteçler oluşturun. <br>- **Select**: Başka bir dizideki öğeleri veya değerleri dönüştürerek ve bu öğeleri belirtilen özelliklere eşleyerek JSON nesnelerine sahip bir dizi oluşturun. |
| ![Dahili Simge][date-time-icon]<br>**Tarih Saati** | Zaman damgaları ile işlemleri gerçekleştirin: <p>- **Zamana ekle**: Belirtilen birim sayısını bir zaman damgasına ekleyin. <br>- **Saat dilimini dönüştür**: Zaman damgasını kaynak saat diliminden hedef saat dilimine dönüştürün. <br>- **Geçerli saat**: Geçerli zaman damgasını dize olarak döndürün. <br>- **Gelecek zamanı al**: Geçerli zaman damgasını ve belirtilen zaman birimlerini döndürün. <br>- **Geçmiş zamanı al**: Geçerli zaman damgasını eksi belirtilen zaman birimlerini döndürün. <br>- **Zaman çıkarma**: Zaman damgasından birkaç zaman birimi çıkar. |
| [![Yerleşik Simge][variables-icon]<br>**Değişkenleri**][variables-doc] | Değişkenlerle işlemleri gerçekleştirin: <p>- **Dizi değişkenine ek**: Bir değişken tarafından depolanan bir dizideki son öğe olarak bir değer ekleyin. <br>- **String değişkenine ek**: Bir değişken tarafından depolanan bir dizedeki son karakter olarak bir değer ekleyin. <br>- **Decrement değişkeni**: Değişkeni sabit bir değerle azaltın. <br>- **Artış değişkeni**: Değişkeni sabit bir değerle arttırır. <br>- **Initialize değişken**: Bir değişken oluşturun ve veri türünü ve başlangıç değerini bildirin. <br>- **Değişkeni Ayarla**: Varolan bir değişkene farklı bir değer atayın. |
|  |  |

<a name="managed-api-connectors"></a>

## <a name="managed-connectors"></a>Yönetilen konektörler

Logic Apps, görevleri, süreçleri ve iş akışlarını bu hizmetler veya sistemlerle otomatikleştirmek için bu popüler Standart bağlayıcıları sağlar:

|   |   |   |   |
|---|---|---|---|
| [![API][azure-service-bus-icon]<br>simgesi**Azure Hizmet Veri Servisi**][azure-service-bus-doc] | Logic Apps’te en yaygın kullanılan bağlayıcı ile zaman uyumsuz iletileri, oturumları ve konu aboneliklerini yönetin. | [![API][sql-server-icon]<br>simgesi**SQL Server**][sql-server-doc] | Kayıtları yönetebilmek, depolanan yordamları çalıştırabilmek veya sorgular gerçekleştirebilmek için SQL Server'ınıza şirket içinde veya buluttaki bir Azure SQL Veritabanı'na bağlanın. |
| [![API][azure-blob-storage-icon]<br>simgesi**Azure<br>Blob Depolama**][azure-blob-storage-doc] | Blob içeriği oluşturup yönetebilmek için depolama hesabınıza bağlanın. | [![API][office-365-outlook-icon]<br>simgesi**Office<br>365 Outlook**][office-365-outlook-doc] | E-postaları, görevleri, takvim etkinliklerini ve toplantıları, kişileri, istekleri ve daha fazlasını oluşturup yönetebilmek için Office 365 e-posta hesabınıza bağlanın. |
| [![API][sftp-ssh-icon]<br>simgesi**SFTP-SSH**][sftp-ssh-doc] | Dosya ve klasörlerinizle çalışabilmeniz için SSH kullanarak internetten erişebileceğiniz SFTP sunucularına bağlanın. | [![API][sharepoint-online-icon]<br>simgesi**SharePoint<br>Online**][sharepoint-online-doc] | Dosyaları, ekleri, klasörleri ve daha fazlasını yönetebilmek için SharePoint Online'a bağlanın. | 
| [![API][dynamics-365-icon]<br>simgesi**Dynamics<br> 365**][dynamics-365-doc] | Kayıtları, öğeleri ve daha fazlasını oluşturabilmek ve yönetmek için Dynamics 365 hesabınıza bağlanın. | [![API][azure-queues-icon]<br>simgesi** <br>Azure Kuyrukları**][azure-queues-doc] | Kuyruklar ve iletiler oluşturup yönetebilmeniz için Azure Depolama hesabınıza bağlanın |
| [![API][ftp-icon]<br>simgesi**FTP**][ftp-doc] | Dosya ve klasörlerinizle çalışabilmeniz için internetten erişebileceğiniz FTP sunucularına bağlanın. | [![API][file-system-icon]<br>simgesi**Dosya <br>Sistemi**][file-system-doc] | Dosyaları oluşturabilmek ve yönetebilmek için şirket içi dosya paylaşımınıza bağlanın. |
| [![API][azure-event-hubs-icon]<br>simgesi**Azure Etkinlik Hub'ları**][azure-event-hubs-doc] | Olayları bir Olay Hub'ı aracılığıyla tüketin ve yayımlayın. Örneğin, Olay Hub'ları ile mantık uygulamanızdan çıktı alın ve bu çıktıyı gerçek zamanlı bir analiz sağlayıcısına gönderin. | [![API][azure-event-grid-icon]<br>simgesi**Azure Olay**<br>**Izgara**][azure-event-grid-doc] | Örneğin, Azure kaynakları veya üçüncü taraf kaynakları değiştiğinde, bir Olay Izgarası tarafından yayınlanan olayları izleyin. |
| [![API][salesforce-icon]<br>simgesi**Salesforce**][salesforce-doc] | Kayıtlar, işler, nesneler ve daha fazlası gibi öğeler oluşturup yönetebilmeniz için Salesforce hesabınıza bağlanın. | [![API][twitter-icon]<br>simgesi**Twitter**][twitter-doc] | Tweetleri, takipçileri, zaman çizelgenizi ve daha fazlasını yönetebilmek için Twitter hesabınıza bağlanın. Tweetlerinizi SQL, Excel veya SharePoint'e kaydedin. |
|||||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Şirket içi bağlayıcılar

Burada, Logic Apps'ın şirket içi sistemlerdeki verilere ve kaynaklara erişmek için sağladığı yaygın olarak kullanılan bazı Standart bağlayıcılar verilmiştir. Şirket içi bir sisteme bağlantı oluşturabilmek için önce şirket [içi bir veri ağ geçidini indirmeniz, yüklemeniz ve kurmanız][gateway-doc]gerekir. Bu ağ geçidi, gerekli ağ altyapısını kurmak zorunda kalmadan güvenli bir iletişim kanalı sağlar.

|   |   |   |   |   |
|---|---|---|---|---|
| [![API][biztalk-server-icon]<br>simgesi**BizTalk** <br> **Server**][biztalk-server-doc] | [![API][file-system-icon]<br>simgesi**Dosya <br>Sistemi**][file-system-doc] | [![API][ibm-db2-icon]<br>simgesi**IBM DB2**][ibm-db2-doc] | [![API][ibm-informix-icon]<br>simgesi**IBM** <br> **Informix**][ibm-informix-doc] | [![API][mysql-icon]<br>simgesi**MySQL**][mysql-doc] |
| [![API][oracle-db-icon]<br>simgesi**Oracle DB**][oracle-db-doc] | [![API][postgre-sql-icon]<br>simgesi**PostgreSQL**][postgre-sql-doc] | [![API][sharepoint-server-icon]<br>simgesi**SharePoint <br>Server**][sharepoint-server-doc] | [![API][sql-server-icon]<br>simgesi**SQL <br>Server**][sql-server-doc] | [![API][teradata-icon]<br>simgesi**Teradata**][teradata-doc] |
|||||

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları

Logic Apps, Azure'daki Kurumsal Tümleştirme Paketi (EIP) aracılığıyla kullanılabilen bir [entegrasyon hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)oluşturduğunuzda ve ödeme yaptığınızda, mantıksal uygulamalarınızla işletmeden işletmeye (B2B) çözümler oluşturmak için Standart bağlayıcılar sağlar. Bu hesapla, ticaret ortakları, anlaşmalar, haritalar, şemalar, sertifikalar ve benzeri B2B eserleri oluşturabilir ve depolayabilirsiniz. Bu yapıları kullanmak için mantık uygulamalarınızı tümleştirme hesabınızla ilişkilendirin. Şu anda BizTalk Server kullanıyorsanız, bu bağlayıcılar zaten tanıdık görünebilir.

|   |   |   |   |
|---|---|---|---|
| [![API][as2-icon]<br>simgesi**AS2 <br>çözme**][as2-doc] | [![API][as2-icon]<br>simgesi**AS2 <br>kodlama**][as2-doc] | [![API][edifact-icon]<br>simgesi**EDIFACT <br>çözme**][edifact-decode-doc] | [![API][edifact-icon]<br>simgesi**EDIFACT <br>kodlama**][edifact-encode-doc] |
| [![API][flat-file-decode-icon]<br>simgesi**Düz dosya <br>çözme**][flat-file-decode-doc] | [![API][flat-file-encode-icon]<br>simgesi**Düz dosya <br>kodlama**][flat-file-encode-doc] | [![API][integration-account-icon]<br>simgesi**Tümleştirme <br>hesabı**][integration-account-doc] | [![API][liquid-icon]<br>simgesi**Sıvı** <br> **dönüştürür**][json-liquid-transform-doc] |
| [![API][x12-icon]<br>simgesi**X12 <br>çözme**][x12-decode-doc] | [![API][x12-icon]<br>simgesi**X12 <br>kodlama**][x12-encode-doc] | [![API][xml-transform-icon]<br>simgesi**XML** <br> **dönüştürür**][xml-transform-doc] | [![API][xml-validate-icon]<br>simgesi**XML <br>doğrulama**][xml-validate-doc] |  
|||||

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Logic Apps, SAP ve IBM MQ gibi kurumsal sistemlere erişmek için bu Kurumsal bağlayıcıları sağlar:

|   |   |   |
|---|---|---|
| [![API][ibm-3270-icon]<br>simgesi**IBM 3270**][ibm-3270-doc] | [![API][ibm-mq-icon]<br>simgesi**IBM MQ**][ibm-mq-doc] | [![API][sap-icon]<br>simgesi**SAP**][sap-connector-doc] |
||||

<a name="ise-connectors"></a>

## <a name="ise-connectors"></a>İmKB konektörleri

Oluşturduğunuz ve çalıştırdığınız mantıksal uygulamalar için yalıtılmış bir [tümleştirme hizmet ortamı (Ise)](#integration-service-environment)için Logic App **Designer, CORE** etiketini kullanarak İmKB'nizde çalışan yerleşik tetikleyicileri ve eylemleri tanımlar. İmKB'de çalışan yönetilen bağlayıcılar **ISE** etiketini görüntülerken, küresel, çok kiracılı Logic Apps hizmetinde çalışan bağlayıcılar her iki etiketi de görüntülemez. Bu liste, şu anda ISE sürümleri olan konektörleri gösterir:

|   |   |   |   |   |
|---|---|---|---|---|
[![API][as2-icon]<br>simgesi**AS2**][as2-doc] | [![API][azure-blob-storage-icon]<br>simgesi**Azure<br>Blob Depolama**][azure-blob-storage-doc] | [![API][azure-cosmos-db-icon]<br>simgesi**Azure <br> Cosmos DB**][azure-cosmos-db-doc] | [![API][azure-event-hubs-icon]<br>simgesi**Azure Etkinlik <br>Hub'ları**][azure-event-hubs-doc] | [![API][azure-file-storage-icon]<br>simgesi**Azure Dosya<br>Depolama**][azure-file-storage-doc] |
| [![API][azure-service-bus-icon]<br>simgesi**Azure Hizmet <br>Veri Servisi**][azure-service-bus-doc] | [![API][azure-sql-data-warehouse-icon]<br>simgesi**Azure <br>SQL Veri Ambarı**][azure-sql-data-warehouse-doc] | [![API][azure-table-storage-icon]<br>simgesi**Azure Tablo <br>Depolama**][azure-table-storage-doc] | [![API][azure-queues-icon]<br>simgesi** <br>Azure Kuyrukları**][azure-queues-doc] | [![API][edifact-icon]<br>simgesi**EDIFACT**][edifact-doc] |
| [![API][file-system-icon]<br>simgesi**Dosya <br>Sistemi**][file-system-doc] | [![API][ftp-icon]<br>simgesi**FTP**][ftp-doc] | [![API][ibm-3270-icon]<br>simgesi**IBM 3270**][ibm-3270-doc] | [![API][ibm-db2-icon]<br>simgesi**IBM DB2**][ibm-db2-doc] | [![API][ibm-mq-icon]<br>simgesi**IBM MQ**][ibm-mq-doc] |
| [![API][sap-icon]<br>simgesi**SAP**][sap-connector-doc] | [![API][sftp-ssh-icon]<br>simgesi**SFTP-SSH**][sftp-ssh-doc] | [![API][smtp-icon]<br>simgesi**SMTP**][smtp-doc] | [![API][sql-server-icon]<br>simgesi**SQL <br>Server**][sql-server-doc] | [![API][x12-icon]<br>simgesi**X12**][x12-doc] |
||||||

Daha fazla bilgi için şu konulara bakın:

* [Azure Logic Apps'tan Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md)
* [Azure Logic Apps'tan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="triggers-actions"></a>

## <a name="triggers-and-action-types"></a>Tetikleyiciler ve eylem türleri

Bağlayıcılar *tetikleyiciler,* *eylemler*veya her ikisini de sağlayabilir. *Tetikleyici,* genellikle tetikleyiciyi çalıştıran ve mantık uygulamanızı çalıştırmaya başlayan olayı belirten herhangi bir mantık uygulamasının ilk adımıdır. Örneğin, FTP bağlayıcısının mantık uygulamanızı başlatan bir tetikleyicisi vardır ve "bir dosya eklendiğinde veya değiştirildiğinde". Bazı tetikleyiciler belirtilen olayı veya verileri düzenli olarak denetler ve belirtilen olayı veya verileri algıladıklarında ateş eder. Diğer tetikleyiciler bekler, ancak belirli bir olay gerçekleştiğinde veya yeni veriler kullanılabilir olduğunda anında ateş eder. Tetikleyiciler, gerekli verileri mantık uygulamanıza da aktarıyor. Mantık uygulamanız iş akışı boyunca bu verileri okuyabilir ve kullanabilir. Örneğin, Twitter bağlayıcısının "Yeni bir tweet yayınlandığında" tetikleyicisi vardır ve bu da tweet'in içeriğini mantık uygulamanızın iş akışına aktarabilir.

Tetikleyici yangınlarından sonra Azure Logic Apps, mantık uygulamanızın bir örneğini oluşturur ve mantık uygulamanızın iş akışındaki *eylemleri* çalıştırmaya başlar. Eylemler, mantık uygulamanızın iş akışında tetikleyiciyi izleyen ve görevleri gerçekleştiren adımlardır. Örneğin, bir SQL veritabanından müşteri verilerini alan ve bu verileri daha sonraki eylemlerde işleyen bir mantık uygulaması oluşturabilirsiniz.

Azure Logic Apps'ın sağladığı genel tetikleyici türleri şunlardır:

* *Yineleme tetikleyicisi*: Bu tetikleyici belirli bir zamanlamada çalışır ve belirli bir hizmet veya sistemle sıkı bir şekilde ilişkili değildir.

* *Yoklama tetikleyicisi*: Bu tetikleyici, belirli bir hizmeti veya sistemi belirtilen zamanlamaya göre düzenli olarak yoklar, yeni verileri veya belirli bir olayın olup olmadığını denetler. Yeni veriler varsa veya belirli bir olay meydana geldiyse, tetikleyici mantık uygulamanızın yeni bir örneğini oluşturur ve çalıştırAbilir ve bu örnek artık girdi olarak geçirilen verileri kullanabilir.

* *İtme tetikleyicisi*: Bu tetikleyici yeni verileri veya bir olayın gerçekleşmesini bekler ve dinler. Yeni veriler kullanılabilir olduğunda veya olay gerçekleştiğinde, tetikleyici mantık uygulamanızın yeni örneğini oluşturur ve çalıştırZ ve artık giriş olarak geçirilen verileri kullanabilir.

<a name="connections"></a>

## <a name="connector-configuration"></a>Konektör yapılandırması

Her bağlayıcının tetikleyicileri ve eylemleri yapılandırmanız için kendi özelliklerini sağlar. Birçok bağlayıcı, mantık uygulamanızda *tetikleyici* veya eylem kullanmadan önce önce hedef hizmete veya sisteme bağlantı oluşturmanızı ve kimlik doğrulama kimlik bilgilerini veya diğer yapılandırma ayrıntılarını sağlamanızı da gerektirir. Örneğin, verilere erişmek veya sizin adınıza yayınlamak için bir Twitter hesabına bağlantı izni vermelisiniz.

Azure Active Directory (Azure AD) OAuth kullanan bağlayıcılar için, bağlantı oluşturmak, erişim belirteçlerinizin [şifrelendiği](../security/fundamentals/encryption-overview.md) ve bir Azure gizli deposunda güvenli bir şekilde depolandığı Office 365, Salesforce veya GitHub gibi hizmette oturum açma anlamına gelir. FTP ve SQL gibi diğer bağlayıcılar, sunucu adresi, kullanıcı adı ve parola gibi yapılandırma ayrıntılarına sahip bir bağlantı gerektirir. Bu bağlantı yapılandırma ayrıntıları da şifrelenir ve güvenli bir şekilde saklanır. [Azure'da şifreleme](../security/fundamentals/encryption-overview.md)hakkında daha fazla bilgi edinin.

Bağlantılar, bu hizmetin veya sistemin izin verdiği sürece hedef hizmete veya sisteme erişebilir. Office 365 ve Dynamics gibi Azure AD OAuth bağlantılarını kullanan hizmetlerde Azure Mantık Uygulamaları, erişim belirteçlerini süresiz olarak yeniler. Diğer hizmetlerin, Azure Logic Apps'ın bir belirteci yenilemeden ne kadar süre yle kullanabileceğiyle ilgili sınırlamaları olabilir. Genellikle, bazı eylemler parolanızı değiştirme gibi tüm erişim belirteçlerini geçersiz kLar.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Özel API'ler ve konektörler

Özel kod çalıştıran veya bağlayıcı olarak kullanılamayan API'leri aramak için, [özel API Uygulamaları oluşturarak](../logic-apps/logic-apps-create-api-app.md)Logic Apps platformünü genişletebilirsiniz. Ayrıca, bu API'leri Azure aboneliğinizdeki herhangi *bir* mantık uygulaması için kullanılabilir hale getiren TÜM REST veya SOAP tabanlı API'ler için [özel bağlayıcılar da oluşturabilirsiniz.](../logic-apps/custom-connector-overview.md) Azure'da herkesin kullanabileceği özel API Uygulamaları veya bağlayıcıları herkese açık hale getirmek [için Microsoft sertifikası için bağlayıcılar gönderebilirsiniz.](../logic-apps/custom-connector-submit-certification.md)

> [!NOTE]
> [Bir tümleştirme hizmeti ortamında (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) dağıttığınız ve çalıştırdığınız mantıksal uygulamalar, bir Azure sanal ağındaki kaynaklara doğrudan erişebilir. Şirket içi veri ağ geçidi gerektiren özel bağlayıcılarınız varsa ve bu bağlayıcıları Bir İmKB dışında oluşturduysanız, İmKB'deki mantıksal uygulamalar da bu bağlayıcıları kullanabilir.
>
> İmKB içinde oluşturulan özel konektörler şirket içi veri ağ geçidiyle çalışmaz. Ancak, bu bağlayıcılar, İmKB'yi barındıran bir Azure sanal ağına bağlı şirket içi veri kaynaklarına doğrudan erişebilir. Bu nedenle, Bir İmKB'deki mantık uygulamaları büyük olasılıkla bu kaynaklarla iletişim kurarken veri ağ geçidine ihtiyaç duymaz.
>
> ISİs oluşturma hakkında daha fazla bilgi için azure [mantık uygulamalarından Azure sanal ağlarına bağlan'a](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Tam [bağlayıcı listesini](https://docs.microsoft.com/connectors) görüntüleme
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Mantık uygulamaları için özel bağlayıcılar oluşturun](https://docs.microsoft.com/connectors/custom-connectors/)
* [Mantıksal uygulamalar için özel API’ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Şirket içi veri ağ geçidiyle mantıksal uygulamalardan şirket içi veri kaynaklarına bağlanın"

<!--Built-in doc links-->
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "API'lerinizi yönetmek ve yayımlamanız için bir Azure API Yönetimi hizmeti örneği oluşturun"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Mantıksal uygulamaları Azure İşlevleri ile tümleştirin"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "İletileri gruplar halinde veya toplu olarak işleme"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Bir koşulu değerlendirin ve koşulun doğru mu yanlış mı olduğuna bağlı olarak farklı eylemler çalıştırın"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Dizideki her öğede aynı eylemleri gerçekleştirme"
[http-doc]: ./connectors-native-http.md "Mantık uygulamalarınızdan HTTP veya HTTPS uç noktalarını arayın"
[http-request-doc]: ./connectors-native-reqres.md "Mantık uygulamalarınızda HTTP isteklerini alın"
[http-response-doc]: ./connectors-native-reqres.md "Mantık uygulamalarınızdan http isteklerine yanıt verme"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Mantık uygulamalarınızdan REST uç noktalarını arayın"
[http-webhook-doc]: ./connectors-native-webhook.md "HTTP veya HTTPS uç noktalarından belirli olayları bekleyin"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Mantıksal uygulamaları iç içe geçmiş iş akışları ile tümleştirin"
[query-doc]: ../logic-apps/logic-apps-perform-data-operations.md#filter-array-action "Sorgu eylemi ile dizileri seçip filtreleyin"
[schedule-doc]: ../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md "Zamanlamaya dayalı mantık uygulamalarını çalıştırma"
[schedule-delay-doc]: ./connectors-native-delay.md "Sonraki eylemi çalıştırmayı geciktirme"
[schedule-delay-until-doc]: ./connectors-native-delay.md "Sonraki eylemi çalıştırmayı geciktirme"
[schedule-recurrence-doc]:  ./connectors-native-recurrence.md "Mantık uygulamalarını yinelenen bir zamanlamada çalıştırma"
[schedule-sliding-window-doc]: ./connectors-native-sliding-window.md "Bitişik parçalar halinde verileri işlemesi gereken mantık uygulamalarını çalıştırma"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Grup bitiş çalışma eylemleri sonra kendi durumunu almak gruplar halinde eylemleri düzenlemek"
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Eylemleri, benzersiz değerler atanmış servis talepleri ne göre düzenleyin. Yalnızca değeri bir ifade, nesne veya belirteç sonucuyla eşleşen servis değerini çalıştırın. Eşleşme yoksa, varsayılan servis talebi"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Mantık uygulamanız için etkin olarak çalışan bir iş akışını durdurma veya iptal etme"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Belirtilen koşul doğru olana veya bazı durum değiştirilene kadar eylemleri yineleme"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Dizileri filtreleme veya CSV ve HTML tabloları oluşturma gibi veri işlemleri gerçekleştirin"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Dize veya dizi değişkenine başlatma, kümeleme, artış, decrement ve ekleme gibi değişkenlerle işlemler gerçekleştirin"

<!--Managed connector doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-cosmos-db-doc]: https://docs.microsoft.com/connectors/documentdb/ "Belgelere ve depolanan yordamlara erişebilmeniz için Azure Cosmos DB'ye bağlanın"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Örneğin, Azure kaynakları veya üçüncü taraf kaynakları değiştiğinde, Bir Olay Izgarası tarafından yayınlanan olayları izleme"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Mantıksal uygulamalar ve Etkinlik Hub'ları arasında etkinlik alabilmeniz ve gönderebilmeniz için Azure Etkinlik Hub'larına bağlanın"
[azure-file-storage-doc]: https://docs.microsoft.com/connectors/azurefile/ "Dosyaları oluşturabilmek, güncellenebilmeniz, alabilmeniz ve silebileceğiniz için Azure Depolama hesabınıza bağlanın"
[azure-queues-doc]: https://docs.microsoft.com/connectors/azurequeues/ "Kuyruklar ve iletiler oluşturup yönetebilmeniz için Azure Depolama hesabınıza bağlanın"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Servis Veri Hizmeti Sıraları ve Konularından mesaj gönderin ve Servis Veri Hizmeti Sırası ve Aboneliklerinden ileti alın"
[azure-sql-data-warehouse-doc]: https://docs.microsoft.com/connectors/sqldw/ "Verilerinizi görüntüleyebilmeniz için Azure SQL Veri Ambarı'na bağlanın"
[azure-table-storage-doc]: https://docs.microsoft.com/connectors/azuretables/ "Tablo oluşturabilmek, güncellenebilmeniz ve sorgulayabilmeniz için Azure Depolama hesabınıza bağlanın ve daha fazlasını yapabilirsiniz"
[biztalk-server-doc]: https://docs.microsoft.com/connectors/biztalk/ "BizTalk tabanlı uygulamaları Azure Logic Apps ile yan yana çalıştırabilmeniz için BizTalk Sunucunuza bağlanın"
[box-doc]: ./connectors-create-api-box.md "Kutu'ya bağlan. Dosyalarınızı yükleyin, alın, silin, listeleyin ve daha fazlası"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Dropbox'a bağlan. Dosyalarınızı yükleyin, alın, silin, listeleyin ve daha fazlası"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "CRM Online verileriyle çalışabilmeniz için Dynamics CRM Online'a bağlanın"
[facebook-doc]: ./connectors-create-api-facebook.md "Facebook'a bağlanın. Zaman çizelgesine gönderme, sayfa akışı ve daha fazlası"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Şirket içi dosya sistemine bağlanın"
[ftp-doc]: ./connectors-create-api-ftp.md "Dosyaları karşıya yükleme, alma, silme ve diğer FTP görevleri için bir FTP / FTPS sunucusuna bağlanın"
[github-doc]: ./connectors-create-api-github.md "GitHub’a bağlanın ve sorunları izleyin"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Google Takvim'e bağlanır ve takvimi yönetebilir"
[google-drive-doc]: ./connectors-create-api-googledrive.md "Verilerinizle çalışabilmeniz için GoogleDrive'a bağlanın"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Sayfalarınızı değiştirebilmeniz için Google Sayfalar'a bağlanın"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmeniz için Google Görevler'e bağlanır"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "IBM ana bilgisayarlarındaki 3270 uygulamaya bağlanma"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2'ye bağlanın. Bir satırı güncelleştirme, bir tablo alma ve daha fazlası"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix'e bağlanın. Bir satırı okuma, tabloları listele ve daha fazlası"
[ibm-mq-doc]: ./connectors-create-api-mq.md "İleti göndermek ve almak için IBM MQ'ya şirket içinde veya Azure'da bağlanın"
[instagram-doc]: ./connectors-create-api-instagram.md "Instagram'a bağlanın. Olayları tetikleme veya harekete geçme"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "MailChimp hesabınıza bağlanın. Postaları yönetme ve otomatikleştirme"
[mandrill-doc]: ./connectors-create-api-mandrill.md "İletişim için Mandrill’e bağlanın"
[mysql-doc]: https://docs.microsoft.com/connectors/mysql/ "Verileri okuyup yazabilmeniz için şirket içi MySQL veritabanınıza bağlanın"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "E-posta gönderip alabilmeniz, takviminizi ve kişilerinizi yönetebilmeniz ve daha fazlası için Office 365 hesabınıza bağlanın"
[office-365-users-doc]: ./connectors-create-api-office365-users.md
[onedrive-doc]: ./connectors-create-api-onedrive.md "Dosyaları yükleyebilmeniz, silebileceğiniz, listeleyebilmeniz ve daha fazlasını yapabilmek için kişisel Microsoft OneDrive'ınıza bağlanın"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Dosyalarınızı yükleyebilmeniz, silebileceğiniz, listeleyebilmeniz ve daha fazlasını yapabilmek için işletmeniz Microsoft OneDrive'a bağlanın"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Satır ekleme, ekleme, silme ve daha fazlasını yapabilmeniz için bir Oracle veritabanına bağlanın"
[outlook.com-doc]: ./connectors-create-api-outlook.md "E-postalarınızı, takvimlerinizi, kişilerinizi ve daha fazlasını yönetebilmeniz için Outlook posta kutunuza bağlanın"
[postgre-sql-doc]: https://docs.microsoft.com/connectors/postgresql/ "Tablolardaki verileri okuyabilmeniz için PostgreSQL veritabanınıza bağlanın"
[project-online-doc]: ./connectors-create-api-projectonline.md "Projelerinizi, görevlerinizi, kaynaklarınızı ve daha fazlasını yönetebilmeniz için Microsoft Project Online'a bağlanın"
[rss-doc]: ./connectors-create-api-rss.md "Özet akışı öğelerini yayımlama ve alma, yeni bir öğe RSS akışında yayımlandığında işlemleri tetikleme"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın. Hesapları, müşteri adaylarını, fırsatları ve daha fazlasını yönetme"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Şirket içi SAP sistemine bağlanın"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "SendGrid'e bağlanın. E-posta gönderme ve alıcı listelerini yönetme"
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "SSH kullanarak SFTP hesabınıza bağlanın. Dosyaları yükleme, alma, silme ve daha fazlası"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "SharePoint şirket içi sunucusuna bağlanın. Belgeleri yönetme, öğeleri listele ve daha fazlasını"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "SharePoint Online'a bağlanın. Belgeleri yönetme, öğeleri listele ve daha fazlasını"
[slack-doc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtp-doc]: ./connectors-create-api-smtp.md "Bir SMTP sunucusuna bağlanın ve ekleri içeren e-posta gönderin"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Azure SQL Veritabanı'na veya SQL Server'a bağlanın. SQL veritabanı tablosunda girişleri oluşturma, güncelleştirme, alma ve silme"
[teradata-doc]: https://docs.microsoft.com/connectors/teradata/ "Tablolardaki verileri okumak için Teradata veritabanınıza bağlanın"
[trello-doc]: ./connectors-create-api-trello.md "Trello'ya bağlan. Projelerinizi yönetin ve herhangi biriyle her şeyi düzenleyin"
[twilio-doc]: ./connectors-create-api-twilio.md "Twilio'ya bağlan. İleti gönderme ve alma, kullanılabilir numaraları alma, gelen telefon numaralarını yönetme ve daha fazlası"
[twitter-doc]: ./connectors-create-api-twitter.md "Twitter'a bağlanın. Zaman çizelgeleri, tweet'ler ve daha fazlasını alın"
[yammer-doc]: ./connectors-create-api-yammer.md "Yammer'a bağlan. İleti gönderme, yeni iletiler alma ve daha fazlası"
[youtube-doc]: ./connectors-create-api-youtube.md "YouTube'a bağlanın. Videolarınızı ve kanallarınızı yönetme"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "AS2 protokolünü kullanan iletileri kodlama ve çözme"
[edifact-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "EDIFACT protokolünü kullanan iletileri kodlama ve çözme"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "EDIFACT protokolünü kullanan iletileri çözme"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "EDIFACT protokolünü kullanan iletileri kodlama"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Kurumsal tümleştirme düz dosya hakkında bilgi edinin"
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Kurumsal tümleştirme düz dosya hakkında bilgi edinin"
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Tümleştirme hesabı yapıları için meta verileri yönetme"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Sıvı şablonlarla JSON'u dönüştürün"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "X12 protokolünü kullanan iletileri kodlama ve çözme"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "X12 protokolünü kullanan iletileri çözme"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "X12 protokolünü kullanan iletileri kodlama"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "XML iletilerini dönüştürme"
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "XML iletilerini doğrulama"

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
[azure-ml-icon]: ./media/apis-list/azure-ml.png
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
