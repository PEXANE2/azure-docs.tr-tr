---
title: ARM ile uygulamaları tahmin edilebilir şekilde dağıtma
description: Azure Kaynak Yönetimi şablonlarını ve PowerShell komut dosyalarını kullanarak birden çok Azure Uygulama Hizmeti uygulamasını tek bir birim olarak ve öngörülebilir bir şekilde nasıl dağıtılacak öğrenin.
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.topic: article
ms.date: 01/06/2016
ms.custom: seodec18
ms.openlocfilehash: 62d0bf776b2d0c97d95b992ed6a1fd2a356e467a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75967392"
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Azure'da tahmin edilebilir şekilde mikro hizmetler sağlama ve dağıtma
Bu öğretici, [Azure Uygulama Hizmeti'ndeki](https://azure.microsoft.com/services/app-service/) [mikro hizmetlerden](https://en.wikipedia.org/wiki/Microservices) oluşan bir uygulamanın tek bir birim olarak ve JSON kaynak grubu şablonlarını ve PowerShell komut dosyası oluşturmayı kullanarak öngörülebilir bir şekilde nasıl sağlanıp dağıtılabildiğini gösterir. 

Yüksek oranda ayrılmış mikro hizmetlerden oluşan yüksek ölçekli uygulamaların sağlanması ve dağıtılması, tekrarlanabilirlik ve öngörülebilirlik başarı için çok önemlidir. [Azure Uygulama Hizmeti,](https://azure.microsoft.com/services/app-service/) web uygulamaları, mobil arka uçlar ve API uygulamaları içeren mikro hizmetler oluşturmanıza olanak tanır. [Azure Kaynak Yöneticisi,](../azure-resource-manager/management/overview.md) veritabanı ve kaynak denetimi ayarları gibi kaynak bağımlılıklarıyla birlikte tüm mikro hizmetleri birim olarak yönetmenize olanak tanır. Şimdi, json şablonları ve basit PowerShell komut dosyası kullanarak böyle bir uygulama dağıtabilirsiniz. 

## <a name="what-you-will-do"></a>Ne yapacaksınız
Öğreticide, aşağıdakileri içeren bir uygulama dağıtırsınız:

* İki Uygulama Hizmeti uygulaması (yani iki mikro hizmet)
* Arka uç SQL Veritabanı
* Uygulama ayarları, bağlantı dizeleri ve kaynak denetimi
* Uygulama öngörüleri, uyarılar, otomatik ölçekleme ayarları

## <a name="tools-you-will-use"></a>Kullanacağınız araçlar
Bu eğitimde, aşağıdaki araçları kullanacaksınız. Araçlar üzerinde kapsamlı bir tartışma olmadığı için, uçtan uca senaryoya bağlı kalacağım ve her birine kısa bir giriş yapacağım ve bu konuda daha fazla bilgi bulabileceğiniz bir yer. 

### <a name="azure-resource-manager-templates-json"></a>Azure Kaynak Yöneticisi şablonları (JSON)
Örneğin Azure Uygulama Hizmeti'nde her uygulama oluşturduğunuzda, Azure Kaynak Yöneticisi bileşen kaynaklarına sahip tüm kaynak grubunu oluşturmak için bir JSON şablonu kullanır. [Azure](/azure/marketplace) Marketi'nden karmaşık bir şablon veritabanını, depolama hesaplarını, Uygulama Hizmeti planını, uygulamanın kendisini, uyarı kurallarını, uygulama ayarlarını, otomatik ölçek ayarlarını ve daha fazlasını içerebilir ve tüm bu şablonlar PowerShell aracılığıyla kullanılabilir. Azure Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için [bkz.](../azure-resource-manager/templates/template-syntax.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Visual Studio için Azure SDK 2.6
En yeni SDK, JSON düzenleyicisinde Kaynak Yöneticisi şablon desteğinde iyileştirmeler içerir. Bunu, değişiklik için sıfırdan bir kaynak grubu şablonu oluşturmak veya varolan bir JSON şablonu (indirilen galeri şablonu gibi) açmak, parametreler dosyasını doldurmak ve hatta kaynak grubunu doğrudan bir Azure Kaynağından dağıtmak için kullanabilirsiniz Grup çözümü.

Daha fazla bilgi için [Visual Studio için Azure SDK 2.6'ya](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/)bakın.

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 veya sonrası
0.8.0 sürümünden başlayarak Azure PowerShell yüklemesi, Azure modülüne ek olarak Azure Kaynak Yöneticisi modüllerini de içerir. Bu yeni modül, kaynak gruplarının dağıtımını komut dosyasına göre yapmanızı sağlar.

Daha fazla bilgi için azure [kaynak yöneticisiyle Azure PowerShell kullanma](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Manager
Bu [önizleme aracı,](https://resources.azure.com) aboneliğinizdeki tüm kaynak gruplarının ve tek tek kaynakların JSON tanımlarını keşfetmenizi sağlar. Araçta, kaynağın JSON tanımlarını düzenleyebilir, kaynakların tüm hiyerarşisini silebilir ve yeni kaynaklar oluşturabilirsiniz.  Bu araçta kolayca bulunan bilgiler şablon yazarlığı için çok yararlıdır, çünkü belirli bir kaynak türü, doğru değerler vb. için hangi özellikleri ayarlamanız gerektiğini gösterir. Kaynak grubunuzu [Azure Portalı'nda](https://portal.azure.com/)bile oluşturabilir, ardından kaynak grubunu baştan çıkarmanıza yardımcı olmak için explorer aracındaki JSON tanımlarını inceleyebilirsiniz.

### <a name="deploy-to-azure-button"></a>Azure düğmesine dağıt
Kaynak denetimi için GitHub kullanıyorsanız, README'nize [Azure'a Dağıt düğmesini](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) koyabilirsiniz. Azure'a anahtar teslimi dağıtım UI sağlayan MD. Bunu herhangi bir basit uygulama için yapabilirsiniz, ancak depo köküne bir azuredeploy.json dosyası koyarak tüm kaynak grubunu dağıtmayı etkinleştirmek için bunu genişletebilirsiniz. Kaynak grubu şablonu içeren bu JSON dosyası, kaynak grubunu oluşturmak için Azure'a Dağıt düğmesi tarafından kullanılır. Örneğin, bu öğreticide kullanacağınız [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) örneğine bakın.

## <a name="get-the-sample-resource-group-template"></a>Örnek kaynak grubu şablonuna alın
Şimdi hemen başlayalım.

1. [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) Uygulama Hizmeti örneğine gidin.
2. readme.md'da **Azure'a Dağıt'ı**tıklatın.
3. [Dağıt-azure](https://deploy.azure.com) sitesine götürülür ve dağıtım parametrelerini girdiniz istenir. Alanların çoğunun depo adı ve sizin için bazı rasgele dizeleri ile doldurulur dikkat edin. İsterseniz tüm alanları değiştirebilirsiniz, ancak girmeniz gereken tek şey SQL Server yönetim girişi ve paroladır, ardından **İleri'yi**tıklatın.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Ardından, dağıtım işlemini başlatmak için **Dağıt'ı** tıklatın. İşlem tamamlandıktan sonra, dağıtılan http://todoappuygulamaya göz atmak için *XXXX*.azurewebsites.net bağlantısını tıklatın. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   Uygulamalar yeni başladığı için ilk göz attığınızda Kullanıcı Arabirimi biraz yavaş olur, ancak bunun tamamen işlevsel bir uygulama olduğuna kendinizi ikna edin.
5. Dağıt sayfasında, Azure Portalı'ndaki yeni uygulamayı görmek için **Yönet** bağlantısını tıklatın.
6. **Essentials** açılır düşüşünde kaynak grubu bağlantısını tıklatın. Uygulamanın **Harici Proje**kapsamında GitHub deposuna zaten bağlı olduğunu da unutmayın. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Kaynak grubu bıçak, kaynak grubunda zaten iki uygulama ve bir SQL Veritabanı olduğunu unutmayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Birkaç dakika içinde gördüğünüz her şey, Azure Kaynak Yöneticisi'nde otomatik bir düzenleme yle ayarlanan tüm bileşenler, bağımlılıklar, ayarlar, veritabanı ve sürekli yayımlama ile tam olarak dağıtılan iki mikro hizmet uygulamasıdır. Tüm bunlar iki şey tarafından yapıldı:

* Azure'a Dağıt düğmesi
* repo kökünde azuredeploy.json

Bu aynı uygulamayı onlarca, yüzlerce veya binlerce kez dağıtabilir ve her seferinde aynı yapılandırmaya sahip olabilirsiniz. Bu yaklaşımın tekrarlanabilirliği ve öngörülebilirliği, yüksek ölçekli uygulamaları kolaylıkla ve güvenle dağıtmanıza olanak tanır.

## <a name="examine-or-edit-azuredeployjson"></a>AZUREDEPLOY'ı inceleyin (veya edin). Json
Şimdi GitHub deposunun nasıl kurulduğuna bakalım. Azure .NET SDK'daki JSON düzenleyicisini kullanacaksınız, bu nedenle [Azure .NET SDK 2.6'yı](https://azure.microsoft.com/downloads/)henüz yüklemediyseniz, şimdi yapın.

1. En sevdiğiniz git aracını kullanarak [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) deposunu klonla. Aşağıdaki ekran görüntüsünde, Visual Studio 2013'teki Team Explorer'da bunu yapıyorum.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Depo kökünden Visual Studio'da azuredeploy.json'ı açın. JSON Anahat bölmesini görmüyorsanız, Azure .NET SDK'yı yüklemeniz gerekir.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

JSON biçiminin her ayrıntısını açıklamayacağım, ancak [Daha Fazla Kaynak](#resources) bölümünde kaynak grubu şablon dilini öğrenmek için bağlantılar vardır. Burada, size uygulama dağıtımı için kendi özel şablonunuzu oluşturmanıza yardımcı olabilecek ilginç özellikleri göstereceğim.

### <a name="parameters"></a>Parametreler
Bu parametrelerin çoğunun **Azure'a Dağıt** düğmesinin girişinizi istediği şey olduğunu görmek için parametreler bölümüne bir göz atın. Azure'a **Dağıt** düğmesinin arkasındaki site, azuredeploy.json'da tanımlanan parametreleri kullanarak giriş UI'sini doldurur. Bu parametreler kaynak adları, özellik değerleri vb. gibi kaynak tanımları boyunca kullanılır.

### <a name="resources"></a>Kaynaklar
Kaynak düğümünde, bir SQL Server örneği, bir Uygulama Hizmeti planı ve iki uygulama da dahil olmak üzere 4 üst düzey kaynağın tanımlandığını görebilirsiniz. 

#### <a name="app-service-plan"></a>App Service planı
JSON basit bir kök düzeyi kaynak ile başlayalım. JSON Anahat'ta, ilgili JSON kodunu vurgulamak için **[hostingPlanName]** adlı Uygulama Hizmeti planını tıklatın. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Öğenin `type` bir Uygulama Hizmeti planı için dize belirtir unutmayın (uzun, çok uzun zaman önce bir sunucu çiftliği olarak adlandırılır) ve diğer öğeler ve özellikler JSON dosyasında tanımlanan parametreleri kullanarak doldurulur ve bu kaynak herhangi bir iç içe kaynak yok.

> [!NOTE]
> Ayrıca, değerin `apiVersion` Azure'a REST API'nin hangi sürümüyle JSON kaynak tanımını kullanacağını söylediğini ve kaynağın içinde nasıl biçimlendirilmesi gerektiğini etkileyebileceğini de `{}`unutmayın. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Ardından, JSON Anahattında **SQLServer** adlı SQL Server kaynağına tıklayın.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Vurgulanan JSON kodu hakkında aşağıdakileri not edin:

* Parametrelerin kullanımı, oluşturulan kaynakların adlandırılmasını ve yapılandırılmasını, onları birbiriyle tutarlı hale getirecek şekilde yapılandırmasını sağlar.
* SQLServer kaynağının iç içe geçen iki kaynağı `type`vardır, her birinin değeri farklıdır.
* Veritabanı ve güvenlik `“resources”: […]`duvarı kurallarının tanımlandığı iç içe kullanılan `dependsOn` kaynaklar, kök düzeyindeki SQLServer kaynağının kaynak kimliğini belirten bir öğeye sahiptir. Bu, Azure Kaynak Yöneticisi'ne "bu kaynağı oluşturmadan önce, başka bir kaynağın zaten var olması gerektiğini; ve bu diğer kaynak şablonda tanımlanmışsa, o zaman ilk olarak bunu oluşturun".
  
  > [!NOTE]
  > İşlevin `resourceId()` nasıl kullanılacağı hakkında ayrıntılı bilgi için [Azure Kaynak Yöneticisi Şablon İşlevlerini](../azure-resource-manager/templates/template-functions-resource.md#resourceid)görün.
  > 
  > 
* Öğenin `dependsOn` etkisi, Azure Kaynak Yöneticisi'nin hangi kaynakların paralel olarak oluşturulabileceğini ve hangi kaynakların sırayla oluşturulması gerektiğini bilmesidir. 

#### <a name="app-service-app"></a>App Service uygulaması
Şimdi, daha karmaşık olan gerçek uygulamalara geçelim. JSON kodunu vurgulamak için JSON Anahattı'ndaki [değişkenler('apiSiteName')] uygulamasını tıklatın. İşlerin giderek ilginçleştiğini fark edeceksiniz. Bu amaçla, özellikleri tek tek konuşacağız:

##### <a name="root-resource"></a>Kök kaynağı
Uygulama iki farklı kaynağa bağlıdır. Bu, Azure Kaynak Yöneticisi'nin uygulamayı yalnızca hem Uygulama Hizmeti planı hem de SQL Server örneği oluşturulduktan sonra oluşturacağı anlamına gelir.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Uygulama ayarları
Uygulama ayarları iç içe bir kaynak olarak da tanımlanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

`properties` Için öğe, `config/appsettings`biçimde `"<name>" : "<value>"`iki uygulama ayarı var.

* `PROJECT`Azure dağıtımına çok projeli Visual Studio çözümünde hangi projeyi kullanacağını söyleyen bir [KUDU ayarıdır.](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) Kaynak denetiminin nasıl yapılandırıldığı daha sonra göstereceğim, ancak ToDoApp kodu çok projeli visual studio çözümünde olduğu için bu ayara ihtiyacımız var.
* `clientUrl`yalnızca uygulama kodunun kullandığı bir uygulama ayarıdır.

##### <a name="connection-strings"></a>Bağlantı dizeleri
Bağlantı dizeleri iç içe bir kaynak olarak da tanımlanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

Öğe `properties` için `config/connectionstrings`eleman, her bağlantı dizesi de bir ad olarak `"<name>" : {"value": "…", "type": "…"}`tanımlanır:değer çifti, belirli biçimi ile . `type` Öğe için olası değerler `MySql` `SQLServer`, `SQLAzure`, `Custom`, ve .

> [!TIP]
> Bağlantı dize türlerinin kesin bir listesi için Azure PowerShell: \[Enum::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType") adresinde aşağıdaki komutu çalıştırın
> 
> 

##### <a name="source-control"></a>Kaynak denetimi
Kaynak denetim ayarları iç içe bir kaynak olarak da tanımlanır. Azure Kaynak Yöneticisi bu kaynağı sürekli yayımlamayı `IsManualIntegration` yapılandırmak (daha sonra uyarıya bakın) ve ayrıca JSON dosyasının işlenmesi sırasında uygulama kodunun dağıtımını otomatik olarak başlatmak için kullanır.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`ve `branch` oldukça sezgisel olmalı ve Git deposu ve şubenin adını yayınlamak için işaret etmelidir. Yine, bu giriş parametreleri ile tanımlanır. 

Uygulama `dependsOn` kaynağının kendisine ek olarak, `sourcecontrols/web` aynı zamanda bağlı `config/appsettings` ve `config/connectionstrings`öğede not. Bunun nedeni, `sourcecontrols/web` yapılandırıldıktan sonra Azure dağıtım işleminin uygulama kodunu otomatik olarak dağıtmaya, oluşturmaya ve başlatmaya çalışmasıdır. Bu nedenle, bu bağımlılık eklemek, uygulama kodu çalıştırılmadan önce uygulamanın gerekli uygulama ayarlarına ve bağlantı dizelerine erişebilmesini sağlamanıza yardımcı olur. 

> [!NOTE]
> Ayrıca `IsManualIntegration` not olarak `true`ayarlanır . Bu özellik, GitHub deposuna gerçekten sahip olmadığınız ve bu nedenle [Azure'a ToDoApp'tan](https://github.com/azure-appservice-samples/ToDoApp) sürekli yayımlama (yani Azure'a otomatik depo güncelleştirmeleri itme) yapılandırma izni veremediği için bu öğreticide gereklidir. Belirtilen depo için `false` varsayılan değeri yalnızca azure [portalında](https://portal.azure.com/) sahibinin GitHub kimlik bilgilerini daha önce yapılandırmışsanız kullanabilirsiniz. Başka bir deyişle, daha önce Azure [Portalı'ndaki](https://portal.azure.com/) herhangi bir uygulama için Kullanıcı kimlik bilgilerinizi kullanarak GitHub veya BitBucket'a kaynak denetimi ayarladıysanız, Azure kimlik bilgilerini hatırlar ve gelecekte GitHub veya BitBucket'tan herhangi bir uygulamayı dağıttığınızda bunları kullanır. Ancak, bunu daha önce yapmadıysanız, Azure Kaynak Yöneticisi uygulamanın kaynak denetim ayarlarını yapılandırmaya çalıştığında JSON şablonunun dağıtımı, depo sahibinin kimlik bilgileriyle GitHub veya BitBucket'ta oturum açamadığından başarısız olur.
> 
> 

## <a name="compare-the-json-template-with-deployed-resource-group"></a>JSON şablonu dağıtılan kaynak grubuyla karşılaştırma
Burada, [Azure Portalı'ndaki](https://portal.azure.com/)tüm uygulama bıçaklarını inceleyebilirsiniz, ancak daha fazla olmasa da aynı derecede yararlı başka bir araç daha vardır. Azure [Kaynak Gezgini](https://resources.azure.com) önizleme aracına gidin ve bu da size aboneliklerinizdeki tüm kaynak gruplarının Azure arka ucunda gerçekte mevcut oldukları gibi JSON gösterimi sağlar. Kaynak grubunun Azure'daki JSON hiyerarşisinin, oluşturmak için kullanılan şablon dosyasındaki hiyerarşiyle nasıl karşılık geldiğini de görebilirsiniz.

Örneğin, [Azure Kaynak Gezgini](https://resources.azure.com) aracına gittiğimde ve gezgindeki düğümleri genişletdiğimde, kaynak grubunu ve ilgili kaynak türleri altında toplanan kök düzeyindekaynakları görebiliyorum.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Bir uygulamaya ayrıntıya inerseniz, aşağıdaki ekran görüntüsüne benzer uygulama yapılandırma ayrıntılarını görebilirsiniz:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Yine, iç içe olan kaynakların JSON şablon dosyanızdakine çok benzer bir hiyerarşiye sahip olması ve uygulama ayarlarını, bağlantı dizelerini vb. JSON bölmesine düzgün bir şekilde yansıtTığını görmeniz gerekir. Buradaki ayarların olmaması JSON dosyanızla ilgili bir soruna işaret edebilir ve JSON şablon dosyanızı sorun gidermenize yardımcı olabilir.

## <a name="deploy-the-resource-group-template-yourself"></a>Kaynak grubu şablonunu kendiniz dağıtın
**Azure'a Dağıt** düğmesi harikadır, ancak azuredeploy.json'daki kaynak grubu şablonuna yalnızca Azuredeploy.json'a daha önce GitHub'a bastıysanız dağıtmanızı sağlar. Azure .NET SDK ayrıca herhangi bir JSON şablon dosyasını doğrudan yerel makinenizden dağıtmanız için araçlar sağlar. Bunu yapmak için aşağıdaki adımları izleyin:

1. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**tıklatın.
2. **Visual C#** > **Cloud** > **Azure Kaynak Grubu'na**tıklayın, ardından **Tamam'ı**tıklatın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. **Azure Şablonu Seç'te**Boş **Şablon'u** seçin ve **Tamam'ı**tıklatın.
4. Azuredeploy.json'ı yeni projenizin **Şablon** klasörüne sürükleyin.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. Solution Explorer'dan, kopyalanan azuredeploy.json'u açın.
6. Gösterinin hatırına, **Kaynak Ekle'yi**tıklatarak JSON dosyamıza bazı standart Application Insight kaynakları ekleyelim. JSON dosyasını dağıtmak istiyorsanız, dağıtım adımlarına atlayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. **Web Uygulamaları için Uygulama Öngörüleri'ni**seçin, ardından varolan bir Uygulama Hizmeti planının ve uygulamasının seçildiğinden emin olun ve ardından **Ekle'yi**tıklatın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   Artık kaynağa ve ne yaptığına bağlı olarak, App Service planına veya uygulamaya bağımlı olan birkaç yeni kaynak görebilirsiniz. Bu kaynaklar varolan tanımlarına göre etkinleştirmez ve siz bunu değiştiresiniz.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. JSON Anahat'ta, JSON kodunu vurgulamak için **appInsights AutoScale'e** tıklayın. Bu, Uygulama Hizmeti planınızın ölçekleme ayarıdır.
9. Vurgulanan JSON kodunda, `location` özellikleri `enabled` ve özelliklerini bulun ve aşağıda gösterildiği gibi ayarlayın.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. JSON Anahat'ta, JSON kodunu vurgulamak için **CPUHigh appInsights'ı** tıklatın. Bu bir uyarı.
11. Özellikleri `location` ve `isEnabled` özelliklerini bulun ve aşağıda gösterildiği gibi ayarlayın. Diğer üç uyarılar (mor ampuller) için aynı şeyi yapın.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. Artık harekete geçmeye hazırsınız. Projeyi sağ tıklatın ve**Yeni Dağıtım Dağıt'ı** **Deploy** > seçin.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Daha önce yapmadıysanız Azure hesabınıza giriş yapın.
14. Aboneliğinizde varolan bir kaynak grubu seçin veya yeni bir tane oluşturun, **azuredeploy.json'u**seçin ve ardından **Parametreleri İyile'yi**tıklatın.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    Artık şablon dosyasında tanımlanan tüm parametreleri güzel bir tabloda edinebilirsiniz. Varsayılanları tanımlayan parametreler zaten varsayılan değerlere sahip olur ve izin verilen değerlerin listesini tanımlayan parametreler açılır bırakma olarak gösterilir.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Tüm boş parametreleri doldurun ve **repoUrl'de** [ToDoApp için GitHub repo adresini](https://github.com/azure-appservice-samples/ToDoApp.git) kullanın. Ardından **Kaydet'i**tıklatın.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Otomatik ölçekleme **Standart** katmanda veya daha yüksek bir özelliktir ve plan düzeyindeuyarılar **Temel** katmanda veya daha yüksek bir alanda sunulan özelliklerdir, tüm yeni App Insights kaynaklarınızın aydınolduğunu görmek için **sku** parametresini **Standart** veya **Premium** olarak ayarlamanız gerekir.
    > 
    > 
16. **Dağıt'ı**tıklatın. **Parolaları Kaydet'i**seçtiyseniz, parola **parametre**dosyasına düz metin olarak kaydedilir. Aksi takdirde, dağıtım işlemi sırasında veritabanı parolasını giriniz.

Bu kadar! JSON dağıtılmış uygulamanıza eklenen yeni uyarıları ve otomatik ölçek ayarlarını görmek için [Azure Portalı'na](https://portal.azure.com/) ve [Azure Kaynak Gezgini](https://resources.azure.com) aracına gitmeniz gerekiyor.

Bu bölümdeki adımlarınız esas olarak aşağıdakileri gerçekleştirilmiştir:

1. Şablon dosyasını hazırladı
2. Şablon dosyasıyla gitmek için parametre dosyası oluşturuldu
3. Parametre dosyasıyla şablon dosyasını dağıttı

Son adım powershell cmdlet ile kolayca yapılır. Visual Studio'nun uygulamanızı dağıttığında ne yaptığını görmek için Scripts\Deploy-AzureResourceGroup.ps1'i açın. Orada bir sürü kod var, ama ben sadece parametre dosyası ile şablon dosyası dağıtmak için gereken tüm ilgili kodu vurgulamak için gidiyorum.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Son cmdlet, `New-AzureResourceGroup`, aslında eylem gerçekleştiren biridir. Tüm bunlar, araç lama nın yardımıyla bulut uygulamanızı tahmin edilebileceği gibi dağıtmanın nispeten kolay olduğunu size göstermelidir. Cmdlet'i aynı parametre dosyasıyla aynı şablonda çalıştırdığınızda, aynı sonucu elde edeyim.

## <a name="summary"></a>Özet
DevOps'de tekrarlanabilirlik ve öngörülebilirlik, mikro hizmetlerden oluşan yüksek ölçekli bir uygulamanın başarılı bir şekilde dağıtılmasının anahtarlarıdır. Bu öğreticide, Azure Kaynak Yöneticisi şablonu kullanarak tek bir kaynak grubu olarak Azure'a iki mikro hizmet uygulaması dağıttınız. Umarım, Azure'daki uygulamanızı bir şablona dönüştürmeye başlamak için ihtiyacınız olan bilgiyi size vermiştir ve bunu tahmin edilebilir bir şekilde sağlayabilir ve dağıtabilirsiniz. 

<a name="resources"></a>

## <a name="more-resources"></a>Diğer kaynaklar
* [Azure Kaynak Yöneticisi Şablon Dili](../azure-resource-manager/templates/template-syntax.md)
* [Azure Kaynak Yöneticisi Şablonları Yazma](../azure-resource-manager/templates/template-syntax.md)
* [Azure Kaynak Yöneticisi Şablon Fonksiyonları](../azure-resource-manager/templates/template-functions.md)
* [Azure Kaynak Yöneticisi şablonuyla bir uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Azure'da Kaynak Grubu Dağıtımları Sorun Giderme](../azure-resource-manager/templates/common-deployment-errors.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede dağıtılan kaynak türleri için JSON sözdizimi ve özellikleri hakkında bilgi edinmek için bkz:

* [Microsoft.Sql/sunucular](/azure/templates/microsoft.sql/servers)
* [Microsoft.Sql/servers/veritabanları](/azure/templates/microsoft.sql/servers/databases)
* [Microsoft.Sql/servers/firewallRules](/azure/templates/microsoft.sql/servers/firewallrules)
* [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)
* [Microsoft.Web/siteler](/azure/templates/microsoft.web/sites)
* [Microsoft.Web/siteler/yuvalar](/azure/templates/microsoft.web/sites/slots)
* [Microsoft.Insights/autoscalesettings](/azure/templates/microsoft.insights/autoscalesettings)