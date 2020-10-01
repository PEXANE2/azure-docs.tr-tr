---
title: Visual Studio Code Otomasyon iş akışları (Önizleme) oluşturma
description: Uygulamaları, verileri, bulut hizmetlerini ve şirket içi sistemleri bütünleştirmek için Visual Studio Code Azure Logic Apps (Önizleme) uzantısıyla durum bilgisiz veya durum bilgisi içeren Otomasyon iş akışları oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: deli, rohitha, vikanand, hongzili, sopai, absaafan, logicappspm
ms.topic: conceptual
ms.date: 09/26/2020
ms.openlocfilehash: cc52358af203bafc87c5f9ac3ae1f237c0c7ae6c
ms.sourcegitcommit: ffa7a269177ea3c9dcefd1dea18ccb6a87c03b70
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91597790"
---
# <a name="create-stateful-or-stateless-workflows-in-visual-studio-code-with-the-azure-logic-apps-preview-extension"></a>Azure Logic Apps (Önizleme) uzantısıyla Visual Studio Code durum bilgisi olan veya durum bilgisiz iş akışları oluşturma

> [!IMPORTANT]
> Bu özellik genel önizlemede, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Uygulamalar, veriler, bulut hizmetleri ve sistemler arasında tümleşen mantıksal uygulama iş akışları oluşturmak için, geliştirme ortamınızda [ *durum* *bilgisiz ve durum bilgisi olmayan* mantıksal uygulama iş akışlarını](#stateful-stateless) derlemek ve yerel olarak çalıştırmak üzere Visual Studio Code ve Azure Logic Apps (Önizleme) uzantısını kullanabilirsiniz.

![Visual Studio Code ve Logic App iş akışını gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-logic-apps-overview.png)

Genel Önizleme uzantısıyla oluşturduğunuz Logic Apps, yeni **mantıksal uygulama (Önizleme)** kaynak türünü kullanır ve yerel ortamınızda [Azure işlevleri](../azure-functions/functions-overview.md) çalışma zamanı tarafından desteklenir. Bu yeni kaynak türü birden çok iş akışı içerebilir ve birden çok işlev içerebilen **işlev uygulaması** kaynak türüne bazı yollarla benzerdir.

Bu arada, Visual Studio Code ve Azure portal oluşturmak ve kullanmak için özgün **Logic Apps** kaynak türü hala vardır. Ancak, özgün kaynak türüne yönelik deneyimler yeni kaynak türünden ayrıdır. Şu anda, hem **Logic Apps** hem de **Logic App (Önizleme)** kaynak türleri Visual Studio Code ve Azure Portal aynı zamanda bulunabilir. Azure aboneliğinizde dağıtılan tüm mantıksal uygulamaları görüntüleyebilir ve bunlara erişebilirsiniz, ancak bunlar görünür ve kendi kategorileriyle ve bölümlerinde ayrı tutulur.

Bu makale, [Bu genel önizleme hakkında](#whats-new)üst düzey bir genel bakış sağlar, **mantıksal uygulama (Önizleme)** kaynak türü hakkında çeşitli yönleri açıklar ve Visual Studio Code kullanarak bu kaynağı nasıl oluşturacağınız aşağıda açıklanmıştır:

* [Durum bilgisiz ve durum bilgisi olmayan](#stateful-stateless) mantıksal uygulamalar birbirinden farklı.

* [Kurulum gereksinimlerini](#prerequisites) karşılayın ve genel önizleme uzantısı için [Visual Studio Code ayarlayın](#set-up) .

* [Bir proje oluşturup bir iş akışı şablonu seçerek](#create-project)yeni **mantıksal uygulama (Önizleme)** iş akışları oluşturma.

* Visual Studio Code ' de yeni mantıksal uygulamalarınızın yerel olarak çalıştırılması ve hata ayıklaması.

* Bu yeni mantıksal uygulamaları doğrudan Visual Studio Code 'dan [Azure 'a](#publish-azure) veya her yerde çalıştırabileceğiniz [bir Docker kapsayıcısına](#deploy-docker) yayınlama. Docker hakkında daha fazla bilgi için bkz. [Docker](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)nedir?

<a name="whats-new"></a>

## <a name="whats-in-this-public-preview"></a>Bu genel önizlemede neler var?

Azure Logic Apps (Önizleme) uzantısı, Visual Studio Code yerel geliştirme deneyiminize birçok geçerli ve ek Logic Apps özelliği getirir, örneğin:

* Hizmet olarak yazılım (SaaS) ve hizmet olarak platform (PaaS) uygulamaları ve hizmetleri ve şirket içi sistemler için bağlayıcılar için, [390 + bağlayıcılarından](/connectors/connector-reference/connector-reference-logicapps-connectors) tümleştirme ve Otomasyon iş akışları için mantıksal uygulamalar oluşturun.

  * Azure Service Bus, Azure Event Hubs ve SQL Server gibi bazı yönetilen bağlayıcılar, yerleşik yerel tetikleyicilere ve HTTP eylemi gibi eylemlere benzer şekilde çalışır.

  * Azure Logic Apps hizmeti bu mantıksal uygulamaların bulut bağlantısı çalışma zamanı uç noktasına istek göndermek için kullanabileceği paylaşılan erişim Imzası (SAS) bağlantı dizeleri oluşturduğundan, her yerde çalışabilen Logic Apps oluşturun ve dağıtın. Logic Apps hizmeti, bu bağlantı dizelerini diğer uygulama ayarlarıyla kaydederek Azure 'a dağıtırken bu değerleri Azure Key Vault kolayca depolayabilirler.

    > [!NOTE]
    > Varsayılan olarak, **mantıksal uygulama (Önizleme)** kaynağında, çalışma zamanında bağlantıların kimliğini doğrulamak için [sistem tarafından atanan yönetilen kimlik](../logic-apps/create-managed-service-identity.md) otomatik olarak etkinleştirilir. Bu kimlik, bağlantı oluştururken kullandığınız kimlik doğrulama kimlik bilgilerinden veya bağlantı dizesinden farklıdır. Bu kimliği devre dışı bırakırsanız, bağlantılar çalışma zamanında çalışmaz.

* Yalnızca bellekte çalışan ve eylemler arasında çalışma geçmişi ve verileri dış depolamada kalıcı hale getirilemediği için daha hızlı bir şekilde çalışacak durum bilgisiz Logic Apps oluşturun. İsteğe bağlı olarak, daha kolay hata ayıklama için çalıştırma geçmişini etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [durum bilgisi içermeyen mantıksal uygulamalar](#stateful-stateless).

* Visual Studio Code geliştirme ortamında mantıksal uygulamalarınızı yerel olarak çalıştırın ve hata ayıklayın.

* Mantıksal uygulamalarınızı, [Azure App Service](../app-service/environment/intro.md) ve [Docker Kapsayıcıları](/dotnet/core/docker/introduction)gibi çeşitli barındırma ortamlarına doğrudan Visual Studio Code yayımlayın ve dağıtın.

> [!NOTE]
> Bilinen güncel sorunlar hakkında daha fazla bilgi için, önizleme uzantısının [bilinen sorunlar GitHub sayfasını](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md)gözden geçirin.

<a name="stateful-stateless"></a>

## <a name="stateful-versus-stateless-logic-apps"></a>Durum bilgisiz ve durum bilgisi olmayan mantıksal uygulamalar

* *Durum Bilgisi Olan*

  Önceki olaylardaki verileri tutmanız, gözden geçirmeniz veya başvuru yapmanız gerektiğinde durum bilgisi içeren Logic Apps oluşturun. Bu Logic Apps, her bir eylem için giriş ve çıktıyı ve bunların iş akışı durumlarını dış depolamada tutar; bu da her çalıştırma tamamlandıktan sonra çalışma ayrıntılarını ve geçmişini gözden geçirmeyi sağlar. Durum bilgisi olan Logic Apps, kesintiler gerçekleştiğinde yüksek dayanıklılık sağlar. Hizmetler ve sistemler geri yüklendikten sonra, kesintiye uğramış mantıksal uygulama çalıştırmalarını kaydedilmiş durumdan yeniden oluşturabilir ve Logic Apps 'i tamamlamak için yeniden çalıştırabilirsiniz. Durum bilgisi olan iş akışları bir yıla kadar çalışmaya devam edebilir.

* *Durum bilgisi olmayan*

  Daha sonra gözden geçirmek üzere dış depolamada bulunan önceki olaylara ait verileri kaydetmeniz, gözden geçirmeniz veya başvuru yapmanız gerekmiyorsa durum bilgisiz Logic Apps oluşturun. Bu Logic Apps, bu bilgileri dış depolamaya aktarmak yerine her eylem için giriş ve çıktıyı ve bunların iş akışı durumlarını saklar. Sonuç olarak, durum bilgisiz Logic Apps genellikle 5 dakikadan daha kısa bir süre, daha hızlı yanıt süreleriyle daha hızlı performans, daha yüksek aktarım hızı ve çalışma maliyetleri azaldığından daha hızlı çalışır. Ancak, kesintiler meydana geliyorsa, kesintiye uğramayan çalıştırmalar otomatik olarak geri yüklenmez, bu sayede çağıranın kesintiye uğratılmaları el ile yeniden göndermelidir. Bu Logic Apps yalnızca zaman uyumlu çalışabilir ve daha kolay hata ayıklama için performans üzerinde bir etkisi olan [çalıştırma geçmişini etkinleştirebilirsiniz](#run-history).

  Durum bilgisiz iş akışları Şu anda yalnızca [yönetilen bağlayıcılar](../connectors/apis-list.md#managed-api-connectors)için eylemleri destekler, tetiklerdir. İş akışınızı başlatmak için [yerleşik isteği, Event Hubs veya Service Bus tetikleyiciyi](../connectors/apis-list.md#built-ins)seçin. Desteklenmeyen Tetikleyiciler, Eylemler ve bağlayıcılar hakkında daha fazla bilgi için bkz. [Desteklenmeyen yetenekler](#unsupported).

İç içe geçmiş mantık uygulamalarının durum bilgisiz ve durum bilgisi olmayan mantıksal uygulamalar arasında farklı davrandığı hakkında bilgi için bkz. [durum bilgisiz ve durum bilgisi olmayan mantıksal uygulamalar](#nested-behavior)

<a name="pricing-model"></a>

## <a name="pricing-model"></a>Fiyatlandırma modeli

Yeni **mantıksal uygulama (Önizleme)** kaynak türünü dağıttığınızda, bir barındırma planı, özellikle de fiyatlandırma modeli olarak kullanılacak [App Service planı veya Premium planını](../azure-functions/functions-scale.md) seçmeniz istenir. App Service planını seçerseniz, ayrıca bir [fiyatlandırma katmanı](../app-service/overview-hosting-plans.md)da seçmeniz istenir. Genel Önizleme sırasında App Service Logic apps çalıştırmak, seçili planın en üstünde *ek* ücret vermez.

Durum bilgisi olan Logic Apps, [dış depolama](../azure-functions/functions-scale.md#storage-account-requirements)kullanır, bu nedenle Azure Storage fiyatlandırma modeli Azure Logic Apps çalışma zamanının gerçekleştirdiği depolama işlemleri için geçerlidir. Örneğin, kuyruklar zamanlama için kullanılır, ancak tablolar ve Bloblar iş akışı durumlarını depolamak için kullanılır.

Bu yeni kaynak türüne uygulanan fiyatlandırma modelleri hakkında daha fazla bilgi için şu konuları gözden geçirin:

* [Azure İşlevleri’ni ölçeklendirme ve barındırma](../azure-functions/functions-scale.md)
* [Azure App Service ölçeği büyütme](../app-service/manage-scale-up.md)
* [Azure Işlevleri fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/functions/)
* [App Service fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/app-service/windows/)
* [Azure depolama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/)

<a name="unsupported"></a>

## <a name="unavailable-or-unsupported-capabilities"></a>Kullanılamayan veya desteklenmeyen yetenekler

Bu genel önizleme için, bu yetenekler kullanılamaz veya desteklenmez:

* Yeni **mantıksal uygulama (Önizleme)** kaynağının oluşturulması MacOS 'ta Şu anda kullanılamıyor.

* Azure bölgelerinin hepsi henüz desteklenmiyor. Şu anda kullanılabilir bölgeler için [bölgeler listesini](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions)kontrol edin.

* İş akışınızı başlatmak için, [istek, http, Event Hubs veya Service Bus tetikleyicisini](../connectors/apis-list.md)kullanın. Şu anda, [Kurumsal bağlayıcılar](../connectors/apis-list.md#enterprise-connectors), Şirket [içi veri ağ geçidi Tetikleyicileri](../connectors/apis-list.md#on-premises-connectors), Web kancası tabanlı tetikleyiciler, kayan pencere tetikleyicisi, [özel bağlayıcılar](../connectors/apis-list.md#custom-apis-and-connectors), tümleştirme hesapları, yapıtları ve [bağlayıcıları](../connectors/apis-list.md#integration-account-connectors) Bu önizlemede desteklenmez. "Azure işlevi çağrısı" özelliği kullanılamıyor, bu nedenle şimdilik Azure işlevi için istek URL 'sini çağırmak üzere HTTP *eylemini* kullanın.

  Durum bilgisi olmayan mantıksal uygulama iş akışları, tetiklemeleri değil yalnızca [yönetilen bağlayıcılar](../connectors/apis-list.md#managed-api-connectors)için eylemleri kullanabilir. Daha önce belirtilen Tetikleyiciler hariç, durum bilgisi olan iş akışları yönetilen bağlayıcılar için hem Tetikleyicileri hem de eylemleri kullanabilir.

* Yeni **mantıksal uygulama (Önizleme)** kaynak türünü yalnızca [Azure 'daki bir Premium veya App Service barındırma planına](#publish-azure) veya [tümleştirme hizmeti ortamları (sesleri)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)değil bir [Docker kapsayıcısına](#deploy-docker)dağıtabilirsiniz. **Tüketim** barındırma planları bu kaynak türünü dağıtmak için desteklenmez ve kullanılabilir değildir.

* Azure portal yeni **mantıksal uygulama (Önizleme)** kaynak türü ile yeni mantıksal uygulamalar oluşturamazsınız. Bu mantıksal uygulamaları yalnızca Visual Studio Code için oluşturabilirsiniz. Ancak, Visual Studio Code Azure 'a bu kaynak türüyle Logic Apps dağıttıktan sonra, [Bu mantıksal uygulamalara yeni iş akışları ekleyebilirsiniz](#add-workflows).

## <a name="prerequisites"></a>Önkoşullar

### <a name="access-and-connectivity"></a>Erişim ve bağlantı

* Gereksinimleri indirebilmeniz, Azure hesabınıza Visual Studio Code bağlanabilmek ve Visual Studio Code Azure 'a, Docker kapsayıcısına veya diğer bir ortama yayımlamanız için internet 'e erişin.

* Bir Azure hesabı ve aboneliği Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Bu makalede aynı örnek mantıksal uygulamayı oluşturmak için, oturum açmak için Microsoft iş veya okul hesabı kullanan bir Office 365 Outlook e-posta hesabınız olması gerekir.

  Outlook.com veya [Gmail](../connectors/connectors-google-data-security-privacy-policy.md)gibi [Azure Logic Apps tarafından desteklenen farklı bir e-posta Bağlayıcısı](/connectors/)kullanmayı tercih ediyorsanız, örneği yine de izleyebilirsiniz ve genel adımlar aynıdır, ancak Kullanıcı arabiriminiz ve seçenekleriniz bazı yollarla farklılık gösterebilir. Örneğin, Outlook.com bağlayıcısını kullanıyorsanız, oturum açmak için kişisel Microsoft hesabı kullanın.

### <a name="storage-requirements"></a>Depolama gereksinimleri

1. [Azure Storage öykünücüsü 5,10](https://go.microsoft.com/fwlink/p/?linkid=717179)'yi indirip yükleyin.

1. Öykünücüyü çalıştırmak için, ücretsiz [SQL Server 2019 Express sürümü](https://go.microsoft.com/fwlink/p/?linkid=866658)gibi BIR yerel SQL DB yüklemeniz gerekir. Daha fazla bilgi için bkz. [geliştirme ve test Için Azure depolama öykünücüsünü kullanma](../storage/common/storage-use-emulator.md).

   > [!IMPORTANT]
   > Mantıksal uygulama iş akışınızı oluşturmak için mantıksal uygulama Tasarımcısı 'nı açmadan önce öykünücüyü başlattığınızdan emin olun. Aksi takdirde, ' a bir ileti alırsınız `Workflow design time could not be started` .
   >
   > ![Çalışan Azure Storage öykünücüsü 'nü gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/start-storage-emulator.png)

### <a name="tools"></a>Araçlar

* [Visual Studio Code 1.30.1 (ocak 2019) veya üzeri bir sürümü](https://code.visualstudio.com/)ücretsizdir. Ayrıca, şu anda yoksa Visual Studio Code için bu ek araçları indirip yükleyin:

  * Visual Studio Code ' deki diğer tüm Azure uzantıları için tek bir genel Azure oturum açma ve abonelik filtreleme deneyimi sunan [Azure Hesap uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

  * [Visual Studio Code uzantısı Için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp), mantıksal uygulamanızı çalıştırmak için F5 işlevinin kullanılmasına olanak sağlar.

  * [Azure Functions Core Tools](../azure-functions/functions-run-local.md), Microsoft ıNSTALLER (MSI) aracılığıyla sürüm [3.0.2931](https://github.com/Azure/azure-functions-core-tools/releases/tag/3.0.2931) veya [2.7.2936](https://github.com/Azure/azure-functions-core-tools/releases/tag/2.7.2936). Bu araçlar, Visual Studio Code ' de çalışan Azure Işlevleri çalışma zamanını destekleyen aynı çalışma zamanının bir sürümünü içerir.

    > [!IMPORTANT]
    > Bu sürümlerden daha önceki bir yüklemeniz varsa, önce bu sürümü kaldırın ya da PATH ortam değişkeninin indirip yüklediğiniz sürümü işaret ettiğini doğrulayın.
    >
    > JavaScript kodunu çalıştırmak için [ **satır içi kod** eylemini](../logic-apps/logic-apps-add-run-inline-code.md) kullanmak istiyorsanız, eylem 2x sürümünü desteklemediğinden Azure işlevleri çalışma zamanı sürümü 3x ' nı kullanmanız gerekir. Ayrıca, bu eylem şu anda Linux işletim sistemlerinde desteklenmemektedir.

  * [Visual Studio Code için Azure Logic Apps (Önizleme) uzantısı](https://go.microsoft.com/fwlink/p/?linkid=2143167). Bu genel önizleme uzantısı, durum bilgisiz ve durum bilgisi olmayan mantıksal uygulamalar oluşturmanıza ve bunları Visual Studio Code yerel olarak çalıştırmanıza olanak sağlar.

    Şu anda, hem özgün **Azure Logic Apps** uzantısının hem de yeni **Azure Logic Apps (önizleme)** uzantısının Visual Studio Code aynı anda yüklü olmasını sağlayabilirsiniz. Visual Studio Code araç çubuğunda Azure simgesini seçerek Azure 'da dağıtılan tüm mantıksal uygulamaları görüntüleyebilirsiniz, ancak her kaynak türü kendi uzantı bölümlerinde, **Logic Apps** ve **Azure Logic Apps (Önizleme)** görünür.

    > [!IMPORTANT]
    > **Azure Logic Apps (özel Önizleme)** uzantısını kullanarak Logic Apps oluşturduysanız, bu Logic Apps genel önizleme uzantısıyla çalışmaz. Ancak, özel önizleme uzantısını kaldırarak, gerekli temizleme işlemini gerçekleştirerek ve genel önizleme uzantısını yükleyerek bu mantıksal uygulamaları geçirebilirsiniz. Daha sonra yeni projenizi Visual Studio Code oluşturup daha önce oluşturduğunuz mantıksal uygulamanın **Workflow. Definition** dosyasını yeni projenize kopyalayabilirsiniz.
    >
    > Bu nedenle, genel önizleme uzantısını yüklemeden önce, önceki sürümleri kaldırdığınızdan ve bu yapıtları sildikten emin olun:
    >
    > * Önceki uzantı paketlerini içeren ve burada yol üzerinde yer alan **Microsoft. Azure. Functions. Extensiondemeti. Iş akışları** klasörü:
    >
    >   * `C:\Users\{userName}\AppData\Local\Temp\Functions\ExtensionBundles`
    >
    >   * `C:\Users\{userName}.azure-functions-core-tools\Functions\ExtensionBundles`
    >
    > * Özel Önizleme uzantısı için [NuGet](/nuget/what-is-nuget) önbelleği olan ve bu yolda bulunan **Microsoft. Azure. iş akışları. WebJobs. Extension** klasörü:
    >
    >   `C:\Users\{userName}\.nuget\packages`

    **Azure Logic Apps (Önizleme)** uzantısını yüklemek için şu adımları izleyin:

    1. Visual Studio Code, sol araç çubuğunda **Uzantılar**' ı seçin.

    1. Uzantılar arama kutusuna girin `azure logic apps preview` . Sonuçlar listesinden **Azure Logic Apps (Önizleme)** **>** **yüklemesi**' ni seçin.

       Yükleme tamamlandıktan sonra, genel önizleme uzantısı **Uzantılar: yüklü** listesinde görünür.

       ![Visual Studio Code yüklü uzantılar listesini "Azure Logic Apps (Önizleme)" uzantısıyla altı çizili olarak gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/azure-logic-apps-extension-installed.png)

* Bu makalede oluşturduğunuz örnek mantıksal uygulamayı test etmek için, örnek Logic App 'teki ilk adım olan Istek tetikleyicisine çağrılar gönderebilen bir araca ihtiyacınız vardır. Böyle bir aracınız yoksa [Postman](https://www.postman.com/downloads/)'ı indirebilir, yükleyebilir ve kullanabilirsiniz.

* Daha kolay tanılama günlüğü ve izleme özelliği için bir [Application Insights](../azure-monitor/app/app-insights-overview.md) kaynağı ekleyebilir ve kullanabilirsiniz. Mantıksal uygulama dağıtımı sırasında veya mantıksal Uygulamanızı dağıttıktan sonra Azure portal bu kaynağı oluşturabilirsiniz.

<a name="set-up"></a>

## <a name="set-up-visual-studio-code"></a>Visual Studio Code'u ayarlama

1. Tüm uzantıların doğru yüklendiğinden emin olmak için Visual Studio Code yeniden yükleyin veya yeniden başlatın.

1. Visual Studio Code, genel önizleme uzantınızın en son güncelleştirmeleri almasını sağlamak için uzantı güncelleştirmelerini otomatik olarak bulup yükleyip yüklemediğine göz atın.

   Bu ayarı denetlemek için şu adımları izleyin:

   1. **Dosya** menüsünde **Tercihler** **>** **Ayarlar**' a gidin.

   1. **Kullanıcı** sekmesinde, **Özellikler** **>** **uzantıları**' na gidin.

   1. **Otomatik denetim güncelleştirmeleri** ve **Otomatik güncelleştirme** 'nin seçili olduğunu onaylayın.

1. Visual Studio Code ' de bu **Azure Logic Apps (Önizleme)** uzantı ayarlarının ayarlandığını etkinleştirin veya denetleyin:

   * **Azure Logic Apps v2: Panel modu**
   * **Azure Logic Apps v2: proje çalışma zamanı**

   1. **Dosya** menüsünde **Tercihler** **>** **Ayarlar**' a gidin.

   1. **Kullanıcı** sekmesinde **>** **Uzantılar** **>** **Azure Logic Apps (Önizleme)** bölümüne gidin.

   1. **Azure Logic Apps v2: Panel modu**altında, **panel modunu etkinleştir** ' in seçili olduğunu onaylayın. **Azure Logic Apps v2: proje çalışma zamanı**altında, daha önce yüklediğiniz [Azure Functions Core Tools sürümüne](#prerequisites) göre sürümü **~ 3** veya **~ 2**olarak ayarlayın.

      > [!IMPORTANT]
      > JavaScript kodunu çalıştırmak için [ **satır içi kod** eylemini](../logic-apps/logic-apps-add-run-inline-code.md) kullanmak istiyorsanız, eylem sürüm 2 ' yi desteklemediğinden, Project Runtime sürüm 3 ' ü kullandığınızdan emin olun. Ayrıca, bu eylem şu anda Linux işletim sistemlerinde desteklenmemektedir.

      !["Azure Logic Apps (Önizleme)" uzantısının Visual Studio Code ayarlarını gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/azure-logic-apps-preview-settings.png)

<a name="connect-azure-account"></a>

## <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

1. Visual Studio Code araç çubuğunda Azure simgesini seçin.

   ![Visual Studio Code araç çubuğunu ve seçili Azure simgesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-azure-icon.png)

1. Azure bölmesinde Azure **: Logic Apps (Önizleme)** altında **Azure 'da oturum aç**' ı seçin. Visual Studio Code kimlik doğrulama sayfası göründüğünde Azure hesabınızla oturum açın.

   ![Azure bölmesi ve Azure oturum açma için Seçili bağlantıyı gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/sign-in-azure-subscription.png)

   Oturum açtıktan sonra Azure bölmesinde Azure hesabınızdaki abonelikler gösterilir. Genel olarak Logic Apps uzantıya sahipseniz, özgün uzantıyı kullanarak oluşturduğunuz özgün Logic Apps kaynaklarını, önizleme uzantısının **Logic Apps (Önizleme** bölümünde değil, yayımlanan uzantının **Logic Apps** bölümünde bulabilirsiniz.
   
   Beklenen abonelikler görünmezse veya bölmenin yalnızca belirli abonelikleri göstermesini istiyorsanız aşağıdaki adımları izleyin:

   1. Abonelikler listesinde, **abonelikleri Seç** düğmesi (filtre simgesi) görünene kadar işaretçinizi ilk aboneliğin yanına taşıyın. Filtre simgesini seçin.

      ![Azure bölmesini ve Seçili filtre simgesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/filter-subscription-list.png)

      Ya da Visual Studio Code durum çubuğunda Azure hesabınızı seçin. 

   1. Başka bir abonelik listesi göründüğünde, istediğiniz abonelikleri seçin ve **Tamam**' ı seçtiğinizden emin olun.

<a name="create-project"></a>

## <a name="create-a-local-project"></a>Yerel proje oluşturma

Mantıksal uygulamanızı oluşturmadan önce, Visual Studio Code mantıksal uygulamanızı yönetebilmeniz ve dağıtabilmeniz için yerel bir proje oluşturun. Temel alınan proje, işlev uygulaması projesi olarak da bilinen bir Azure Işlevleri projesine benzerdir. Ancak, bu proje türleri birbirinden ayrıdır, bu nedenle mantıksal uygulama iş akışları ve işlevleri aynı projede olamaz.

1. Bilgisayarınızda, daha sonra Visual Studio Code oluşturacağınız proje için kullanmak üzere *boş* bir yerel klasör oluşturun.

1. Visual Studio Code, tüm açık klasörleri kapatın.

1. Azure bölmesinde **Azure: Logic Apps (Önizleme)**' nin yanındaki **Yeni proje oluştur** ' u (bir klasör ve şimşek sürgüsü gösteren simge) seçin.

   !["Yeni proje oluştur" seçiliyken Azure bölmesi araç çubuğunu gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/create-new-project-folder.png)

1. Windows Defender güvenlik duvarı, için Visual Studio Code için ağ erişimi vermenizi isterse, `Code.exe` Azure Functions Core Tools için, `func.exe` **Ev veya iş ağım** **>** **erişime izin ver**gibi özel ağlar ' ı seçin.

1. Proje klasörünüzü oluşturduğunuz konuma gidin, bu klasörü seçin ve devam edin.

   ![Yeni oluşturulan proje klasörüyle ve "Seç" düğmesinin seçili olduğu "klasör seç" iletişim kutusunu gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-project-folder.png)

1. Görüntülenen şablonlar listesinden **durum bilgisi olan Iş akışı** veya **durum bilgisiz iş akışı**' nı seçin. Bu örnek, **durum bilgisi olan Iş akışını**seçer.

   !["Durum bilgisi olan Iş akışı" seçiliyken iş akışı şablonları listesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-stateful-stateless-workflow.png)

1. Mantıksal uygulama iş akışınız için bir ad girin ve ENTER tuşuna basın. Bu örnek `example-workflow` , ad olarak kullanır.

   !["Yeni bir durum bilgisi olan Iş akışı (3/4)" kutusu ve "örnek-iş akışı" iş akışı adı olarak gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/name-your-workflow.png)

1. Görüntülenen sonraki listeden **geçerli pencerede aç**' ı seçin.

   !["Geçerli pencerede aç" listesinin seçili olduğunu gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-project-location.png)

   Yeniden yükler Visual Studio Code, Gezgin bölmesini açar ve artık otomatik olarak oluşturulan proje dosyalarını içeren projenizi gösterir. Örneğin, projenin mantıksal uygulama iş akışınızın adını gösteren bir klasörü vardır. Bu klasörün içindeworkflow.jsdosyadaki **workflow.json** , mantıksal uygulama iş akışınızın temel alınan JSON tanımını içerir.

   ![Proje klasörü, iş akışı klasörü ve "workflow.json" dosyası ile gezgin penceresini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/local-project-created.png)

<a name="open-workflow-definition-designer"></a>

## <a name="open-the-workflow-definition-file-in-logic-app-designer"></a>Logic App Designer 'da iş akışı Tanım dosyasını açın

1. Şu komutu çalıştırarak bilgisayarınızda yüklü olan sürümleri kontrol edin:

   `..\Users\{yourUserName}\dotnet --list-sdks`

   .NET Core SDK 5. x varsa, bu sürüm, tasarımcıda mantıksal uygulamanın temel alınan iş akışı tanımını açmanızı önleyebilir. Bu sürümü kaldırmak yerine, projenizin kök konumunda, 3.1.201 ' den daha yeni olan .NET Core Runtime 3. x sürümüne başvuran bir **global.js** oluşturun, örneğin:

   ```json
   {
      "sdk": {
         "version": "3.1.8",
         "rollForward": "disable"
      }
   }
   ```

   Bu **global.js** dosyadaki bir dosyayı, Visual Studio Code içindeki kök konumdaki projenize açıkça eklediğinizden emin olun. Aksi halde tasarımcı açılmaz.

1. Visual Studio Code Windows veya Linux üzerinde çalışıyorsa Azure Storage öykünücüsü ' nin çalıştığından emin olun. Daha fazla bilgi için [önkoşulları](#prerequisites)gözden geçirin.

1. İş akışınız için proje klasörünü genişletin. Dosyanın kısayol menüsünde **workflow.js** açın ve **tasarımcıda aç**' ı seçin.

   !["Tasarımcıda aç" seçiliyken dosyada workflow.jsiçin Gezgin bölmesini ve kısayol penceresini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/open-definition-file-in-designer.png)

   ' In hata iletisini alırsanız `Workflow design time could not be started` , Azure Storage öykünücüsü ' nin çalıştığından emin olun. Aksi takdirde, diğer sorun giderme önerilerini deneyin:

   Visual Studio Code, önizleme uzantısının çıktısını denetleyin.

   1. **Görünüm** menüsünde **Çıkış**' ı seçin.

   1. **Çıktı** başlığı çubuğundaki listeden, önizleme uzantısı için çıktıyı görüntüleyebilmeniz için **Azure Logic Apps** ' ı seçin, örneğin:

      !["Azure Logic Apps" seçiliyken Visual Studio Code çıkış penceresini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/check-outout-window-azure-logic-apps.png)

   1. Çıktıyı gözden geçirin ve bu hata iletisinin görüntülenip görüntülenmeyeceğini denetleyin:

      ```text
      A host error has occurred during startup operation '{operationID}'.
      System.Private.CoreLib: The file 'C:\Users\{userName}\AppData\Local\Temp\Functions\
      ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle.Workflows\1.1.1\bin\
      DurableTask.AzureStorage.dll' already exists.
      Value cannot be null. (Parameter 'provider')
      Application is shutting down...
      Initialization cancellation requested by runtime.
      Stopping host...
      Host shutdown completed.
      ```

      Daha önce tasarımcıyı açmaya çalıştıysanız ve ardından projeyi sonlandırdıysanız veya sildiyseniz bu hata oluşabilir. Bu hatayı çözmek için, bu konumdaki **extensiondemeti** klasörünü silin **. ..\Users \\ {UserName} \Appdata\local\temp\functions\extensiondemeti**ve tasarımcıda **workflow.js** dosyayı açmayı yeniden deneyin.

1. **Azure 'da bağlayıcıları etkinleştir** listesinde, Azure 'un yalnızca Azure hizmetleri için bağlayıcılar değil Azure Portal bulunan tüm yönetilen bağlayıcılar için geçerli olan **Azure 'dan bağlayıcılar kullanın**' ı seçin.

   !["Azure 'da bağlayıcıları etkinleştir" listesinin açık ve "Azure 'dan bağlayıcılar kullan" seçiliyken Gezgin bölmesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/use-connectors-from-azure.png)

1. Kaynak grupları listesinden **Yeni kaynak grubu oluştur**' u seçin.

   ![Kaynak grupları listesi ve "yeni kaynak grubu oluştur" seçiliyken Gezgin bölmesini gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/create-select-resource-group.png)

1. Kaynak grubu için bir ad girin ve ENTER tuşuna basın. Bu örnekte, kullanılır `example-logic-app-preview-rg` .

   ![Gezgin bölmesi ve kaynak grubu adı kutusunu gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/enter-name-for-resource-group.png)

1. Konumlar listesinden, kaynak grubunuzu ve kaynaklarınızı oluşturmak için kullanmak üzere [desteklenen bir Azure bölgesi](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions) bulun ve seçin. Bu örnek **Orta Batı ABD**kullanır.

   > [!IMPORTANT]
   > Tüm bölgeler Şu anda desteklenmemektedir, ancak daha fazla bölge ekleme güncelleştirmeleri devam etmez. Desteklenmeyen bir bölgenin seçilmesi, bağlantı oluşturma gibi sorunlara yol açabilir. Şu anda desteklenen bölgeler için Önizleme uzantısının [bilinen sorunlar GitHub sayfasını](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions)gözden geçirin.

   ![Konum listesi ve "Orta Batı ABD" seçili Gezgin bölmesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-azure-region.png)

   Bu adımı gerçekleştirdikten sonra, Visual Studio Code mantıksal uygulama Tasarımcısı ' nı açar.

   > [!NOTE]
   > Visual Studio Code iş akışı tasarım zamanı API 'sini başlattığında, başlatmanın birkaç saniye sürebileceğini belirten bir ileti görüntülenir. Bu iletiyi yoksayabilir veya Tamam ' **ı**seçebilirsiniz.

   Mantıksal uygulama Tasarımcısı görüntülendikten sonra, tasarımcı üzerinde **bir Işlem seçme** istemi görüntülenir ve varsayılan olarak seçilidir ve **Eylem Ekle** bölmesini gösterir.

   ![Mantıksal uygulama Tasarımcısı 'Nı gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-app-designer.png)

1. Sonra, iş akışınıza [bir tetikleyici ve eylem ekleyin](#add-trigger-actions) .

<a name="add-trigger-actions"></a>

## <a name="add-a-trigger-and-actions"></a>Tetikleyici ve eylem ekleme

Mantıksal uygulama Tasarımcısı 'nı dosyanın kısayol menüsündeki **workflow.js** açtıktan sonra, tasarımcı üzerinde **bir işlem seçme** istemi görüntülenir ve varsayılan olarak seçilidir. Artık, bir tetikleyici ve eylemler ekleyerek iş akışınızı oluşturmaya başlayabilirsiniz.

Bu örnekteki Logic App iş akışı bu tetikleyiciyi ve bu eylemleri kullanır:

* Gelen çağrıları veya istekleri alan ve diğer hizmetlerin veya mantıksal uygulamaların çağırabileceği bir uç nokta oluşturan **BIR http isteği alındığında**yerleşik [istek tetikleyicisi](../connectors/connectors-native-reqres.md).

* [Office 365 Outlook eylemi](../connectors/connectors-create-api-office365-outlook.md), **bir e-posta gönderin**.

* Bir yanıt göndermek ve verileri çağırana geri döndürmek için kullandığınız yerleşik [yanıt eylemi](../connectors/connectors-native-reqres.md).

### <a name="add-the-request-trigger"></a>Istek tetikleyicisini ekleme

1. Tasarımcı 'nın yanında, **bir tetikleyici Ekle** bölmesindeki bir **işlem araması seçin** kutusunda, yerel olarak çalışan bir tetikleyiciyi seçebilmeniz için **yerleşik** ' ın seçildiğinden emin olun.

1. **Bir işlem araması seçin** kutusunda `when a http request` , gırın ve **bir http Isteği alındığında**adlandırılan yerleşik istek tetikleyicisini seçin.

   ![Logic App Designer ve * * "bir HTTP isteği alındığında" tetikleyicisi seçiliyken "bir tetikleyici Ekle" bölmesinin gösterildiği ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/add-request-trigger.png)

   Tetikleyici tasarımcıda göründüğünde, tetikleyicinin özellikler, ayarlar ve diğer eylemleri göstermek için tetikleyicinin Ayrıntılar bölmesi açılır.

   ![Mantıksal uygulama tasarımcısını "HTTP isteği alındığında" tetikleyicisi seçiliyken ve tetikleme ayrıntıları bölmesini açık olarak gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/request-trigger-added-to-designer.png)

   > [!TIP]
   > Ayrıntılar bölmesi görünmezse, tetikleyicinin tasarımcıda seçili olduğundan emin olur.

1. Tasarımcıda bir öğeyi silmeniz gerekiyorsa, şu adımları izleyin:

   1. Tasarımcıda öğenin Ayrıntılar bölmesini sağ tarafa açan öğeyi seçin.

   1. Visual Studio Code penceresini tetikleyici veya eylem adı ' nın yanında, sağ üst köşede üç nokta (**...**) düğmesi görünecek şekilde genişletin. 

   1. Üç nokta (**...**) menüsünü açın ve **Sil**' i seçin. Silmeyi onaylamak için **Tamam**' ı seçin.

      ![Açık ayrıntı bölmesi ve seçili üç nokta düğmesi ve "Sil" seçeneği ile tasarımcıda seçili öğeyi gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/delete-item-from-designer.png)

### <a name="add-the-office-365-outlook-action"></a>Office 365 Outlook eylemini ekleme

1. Tasarımcıda, eklediğiniz tetikleyicinin altında **yeni adım**' ı seçin.

   **Bir Işlem seçme** istemi tasarımcıda görünür ve bir sonraki eylemi seçebilmeniz Için **Eylem Ekle bölmesi** yeniden açılır.

1. **Eylem Ekle** bölmesinde, **bir işlem seçin** arama kutusunun **altında Azure ' u seçerek Azure** 'da dağıtılan bir yönetilen bağlayıcı için bir eylem bulabilir ve seçebilmeniz gerekir.

   Bu örnek, Office 365 Outlook eylemini seçer ve kullanır, **e-posta gönder (v2)**.

   ![Logic App Designer 'ı ve * * Office 365 Outlook "e-posta gönder" eylemini içeren bir eylem Ekle * * bölmesi gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/add-send-email-action.png)

1. Eylem ayrıntıları bölmesinde, e-posta hesabınıza bir bağlantı oluşturabilmek için **oturum aç** ' ı seçin.

   ![Logic App Designer ve * * "oturum aç" seçiliyken bir e-posta gönder (v2) * * bölmesi gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/send-email-action-sign-in.png)

   > [!NOTE]
   > Hata alırsanız, `Failed to create connection...` mantıksal uygulamanız için şu anda desteklenmeyen bir bölge seçmiş olabilirsiniz. Daha fazla bölge eklemek için güncelleştirmeler. Bu sırada, desteklenen bölgeler için Önizleme uzantısının [bilinen sorunlar GitHub sayfasını](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md#available-regions)gözden geçirin.

1. Visual Studio Code, e-posta hesabınıza erişmek için onay isteminde bulunduğunda **Aç**' ı seçin.

   ![Erişime izin vermek için Visual Studio Code isteminin gösterildiği ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-open-external-website.png)

   > [!TIP]
   > Gelecekteki istemlere engel olmak için, **güvenilir etki alanlarını Yapılandır** ' ı seçerek kimlik doğrulama sayfasını güvenilen bir etki alanı olarak ekleyebilirsiniz.

1. Oturum açmak, erişime izin vermek ve Visual Studio Code dönmeye izin vermek için sonraki istemleri izleyin.

   > [!NOTE]
   > İstekleri tamamlamadan önce çok fazla zaman geçerse, kimlik doğrulama işlemi zaman aşımına uğrar ve başarısız olur. Bu durumda, tasarımcıya geri dönüp bağlantıyı oluşturmak için oturum açmayı yeniden deneyin.

1. Azure Logic Apps Preview uzantısı, e-posta hesabınıza erişmek için onay sorar, **Aç**' ı seçin. Erişime izin vermek için sonraki istemi izleyin.

   ![Erişime izin vermek için Önizleme uzantısı isteminin gösterildiği ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/allow-preview-extension-open-uri.png)

   > [!TIP]
   > Gelecekteki istemlerin oluşmasını engellemek için, **Bu uzantı için bir daha sorma**seçeneğini belirleyin.

   Visual Studio Code bağlantınız oluşturulduktan sonra bazı bağlayıcılar `The connection will be valid for {n} days only.` Bu zaman sınırının yalnızca mantıksal uygulamanızı Visual Studio Code yazarken yalnızca süreye uygulanacağını belirten iletiyi gösterir. Dağıtımdan sonra, mantıksal uygulamanız otomatik olarak etkinleştirilmiş [sistem tarafından atanan yönetilen kimliği](../logic-apps/create-managed-service-identity.md)kullanarak çalışma zamanında kimlik doğrulayabildiğinden, bu sınır artık geçerli olmaz. Bu yönetilen kimlik, bağlantı oluştururken kullandığınız kimlik doğrulama kimlik bilgilerinden veya bağlantı dizesinden farklıdır. Bu sistem tarafından atanan yönetilen kimliği devre dışı bırakırsanız, bağlantılar çalışma zamanında çalışmaz.

1. Tasarımcıda **e-posta gönder** eylemi seçili görünmüyorsa, bu eylemi seçin.

1. Eylemin Ayrıntılar bölmesindeki **Parametreler** sekmesinde, eylem için gerekli bilgileri sağlayın, örneğin:

   ![Logic App Designer 'ı Office 365 Outlook "e-posta gönderme" eylemi için ayrıntılarıyla gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/send-email-action-details.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Amaç** | Evet | <*e-posta adresiniz*> | E-posta alıcısı, test amacıyla e-posta adresiniz olabilir. Bu örnek, kurgusal e-postayı kullanır `sophiaowen@fabrikam.com` . |
   | **Konu** | Evet | `An email from your example workflow` | E-posta konusu |
   | **Gövde** | Evet | `Hello from your example workflow!` | E-posta gövdesi içeriği |
   ||||

   > [!NOTE]
   > Ayrıntılar bölmesinde **Ayarlar**, **sonra Çalıştır**veya **statik sonuç** sekmesinde herhangi bir değişiklik yapmak istiyorsanız, sekmeleri değiştirmeden veya odağı tasarımcıya değiştirmeden önce bu değişiklikleri kaydetmek için **bitti** ' yi seçtiğinizden emin olun. Aksi takdirde, Visual Studio Code değişikliklerinizi tutamayacaktır. Daha fazla bilgi için, önizleme uzantısının [bilinen sorunlar GitHub sayfasını](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md)gözden geçirin.

1. Tasarımcıda **Kaydet**' i seçin.

Sonra, Visual Studio Code iş akışınızı yerel olarak çalıştırın ve hata ayıklayın.

<a name="debug-test-locally"></a>

## <a name="run-and-debug-locally"></a>Yerel olarak çalıştır ve hata ayıkla

Mantıksal uygulamanızı test etmek için aşağıdaki adımları izleyerek bir hata ayıklama oturumu başlatın ve Istek tetikleyicisi tarafından oluşturulan uç noktanın URL 'sini bulun. Daha sonra bu uç noktaya bir istek gönderebilmeniz için bu URL 'ye ihtiyacınız vardır.

1. Durum bilgisiz mantıksal uygulama iş akışında daha kolay hata ayıklamanıza yardımcı olmak için [Bu iş akışı için çalıştırma geçmişini etkinleştirebilirsiniz](#run-history).

1. Visual Studio Code araç çubuğunda, **Çalıştır** menüsünü açın ve **hata ayıklamayı Başlat** (F5) seçeneğini belirleyin.

   Hata ayıklama oturumunu gözden geçirebilmeniz için **Terminal** penceresi açılır.

1. Şimdi Istek tetikleyicisinde uç nokta için geri çağırma URL 'sini bulun.

   1. Projenizi görüntüleyebilmeniz için Gezgin bölmesini yeniden açın.

   1. Dosyanın kısayol menüsünde **workflow.js** **genel bakış**' ı seçin.

      !["Genel bakış" seçiliyken dosya workflow.jsiçin Gezgin bölmesini ve kısayol penceresini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/open-workflow-overview.png)

   1. Örnek Istek tetikleyicisi için bu URL 'ye benzer şekilde görünen **geri çağırma URL 'si** değerini bulun:

      `http://localhost:7071/api/<workflow-name>/triggers/manual/invoke?api-version=2020-05-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<shared-access-signature>`

      ![Geri çağırma URL 'SI ile iş akışınızın genel bakış sayfasını gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/find-callback-url.png)

1. Mantıksal uygulama iş akışını tetikleyerek geri çağırma URL 'sini test etmek için, istek oluşturma ve gönderme için [Postman](https://www.postman.com/downloads/) veya tercih ettiğiniz aracı açın.

   Bu örnek Postman kullanılarak devam eder. Daha fazla bilgi için bkz. [Postman Başlarken](https://learning.postman.com/docs/getting-started/introduction/).

   1. Postman araç çubuğunda **Yeni**' yi seçin.

      ![Yeni düğme seçiliyken Postman gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/postman-create-request.png)

   1. **Yeni oluştur** bölmesinde, **yapı taşları**altında **istek**' ı seçin.

   1. İsteği **Kaydet** penceresinde, **istek adı**altında, istek için bir ad sağlayın, örneğin, `Test workflow trigger` .

   1. **Kaydedilecek bir koleksiyon veya klasör seçin**altında **koleksiyon oluştur**' u seçin.

   1. **Tüm koleksiyonlar**' ın altında, isteklerinizi düzenlemek için oluşturulacak koleksiyon için bir ad sağlayın, ENTER tuşuna basın ve ** *koleksiyon adı* > <için Kaydet**' i seçin. Bu örnek `Logic Apps requests` , koleksiyon adı olarak kullanılır.

      İstek tetikleyicisi için geri çağırma URL 'sine bir istek gönderebilmeniz için Postman 'ın istek bölmesi açılır.

      ![Açılan istek bölmesi ile Postman gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/postman-request-pane.png)

   1. Visual Studio Code dön. iş akışının genel bakış sayfasından **geri çağırma URL 'si** özellik değerini kopyalayın.

   1. Postman 'a geri dönün. İstek bölmesinde, şu anda varsayılan istek yöntemi olarak **Al** ' ı gösteren Yöntem listesinin ardından, daha önce Adres kutusuna kopyaladığınız geri arama URL 'sini yapıştırın ve **Gönder**' i seçin.

      ![Gönder düğmesinin seçili olduğu adres kutusunda Postman ve geri arama URL 'sini gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/postman-test-call-back-url.png)

      Örnek mantıksal uygulama iş akışı, bu örneğe benzer şekilde görünen bir e-posta gönderir:

      ![Örnekte açıklandığı gibi Outlook e-postasını gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-app-result-email.png)

1. Visual Studio Code, iş akışınızın genel bakış sayfasına dönün.

   Durum bilgisi olan bir iş akışı oluşturduysanız, gönderdiğiniz istek iş akışını tetikler, Genel Bakış sayfasında iş akışının çalışma durumu ve geçmişi gösterilir. Çalıştırma durumları hakkında daha fazla bilgi için bkz. çalışma [geçmişini inceleme](../logic-apps/monitor-logic-apps.md#review-runs-history).

   ![İş akışınızın genel bakış sayfasını çalışma durumu ve geçmişi ile gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/post-trigger-call.png)

   > [!TIP]
   > Çalıştırma durumu görünmezse, **Yenile**' yi seçerek genel bakış sayfasını yenilemeyi deneyin.

1. Belirli bir çalıştırmada ve adımın giriş ve çıktılarında her adımın durumlarını gözden geçirmek için, bu çalıştırma için üç nokta (**...**) düğmesini seçin ve **çalıştırmayı göster**' i seçin.

   ![İş akışınızın çalışma geçmişi satırını üç nokta düğmesi ve "çalışmayı göster" seçiliyken gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/show-run-history.png)

   Visual Studio Code her eyleme ait çalıştırma durumlarını gösterir.

1. Her adımın giriş ve çıkışlarını gözden geçirmek için, incelemek istediğiniz adımı genişletin. Bu adım için ham giriş ve çıkışları daha fazla gözden geçirmek için **Ham girdileri göster** veya **Ham çıkışları göster**' i seçin.

   ![İş akışındaki her adımın durumunu ve genişletilmiş "e-posta gönder" eyleminde giriş ve çıkışları gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-details.png)

1. Hata ayıklama oturumunu durdurmak için, **Çalıştır** menüsünde, **hata ayıklamayı Durdur** (SHIFT + F5) öğesini seçin.

<a name="return-response"></a>

## <a name="return-a-response-to-the-caller"></a>Çağırana bir yanıt döndürün

Mantıksal uygulamanıza bir istek gönderen çağırana geri yanıt döndürmek için, Istek tetikleyicisiyle başlayan bir iş akışı için yerleşik [Yanıt eylemini](../connectors/connectors-native-reqres.md) kullanabilirsiniz.

1. Mantıksal uygulama Tasarımcısı ' nda, **e-posta gönder** eyleminin altında **yeni adım**' ı seçin.

   **Bir Işlem seçme** istemi tasarımcıda görünür ve bir sonraki eylemi seçebilmeniz Için **Eylem Ekle bölmesi** yeniden açılır.

1. **Eylem Ekle** bölmesinde, **eylem** Ekle arama kutusunda, **yerleşik** ' ın seçili olduğundan emin olun. Arama kutusuna girin `response` ve **Yanıt** eylemini seçin.

   ![Mantıksal uygulama Tasarımcısı 'nın seçili yanıt eylemiyle birlikte gösterdiği ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/add-response-action.png)

   Tasarımcı üzerinde **Yanıt** eylemi göründüğünde, eylemin Ayrıntılar bölmesi otomatik olarak açılır.

   ![Mantıksal uygulama Tasarımcısı 'nı "yanıt" eyleminin Ayrıntılar bölmesiyle açık olan "gövde" özelliği "e-posta gönder" eyleminin "Body" özellik değeri olarak ayarlanan ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/response-action-details.png)

1. **Parametreler** sekmesinde çağırmak istediğiniz işlev için gerekli bilgileri sağlayın.

   Bu örnek, **e-posta gönder** eyleminden çıktı olan **gövde** özelliği değerini döndürür.

   1. Dinamik içerik listesinin görünmesi için **Body** özellik kutusunun içine tıklayın ve yukarıdaki tetikleyiciden ve iş akışındaki eylemlerden kullanılabilir çıkış değerlerini gösterir.

      ![Dinamik içerik listesinin görünmesi için "gövde" özelliğinin içindeki fare işaretçisi içindeki "yanıt" eyleminin Ayrıntılar bölmesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/open-dynamic-content-list.png)

   1. Dinamik içerik listesinde, **e-posta gönder**' ın altında **gövde**' yi seçin.

      ![Açık dinamik içerik listesini gösteren ekran görüntüsü. Listede, "e-posta gönder" üstbilgisinin altında "gövde" çıkış değeri seçilidir.](./media/create-stateful-stateless-workflows-visual-studio-code/select-send-email-action-body-output-value.png)

      İşiniz bittiğinde, yanıt eyleminin **gövde** özelliği artık **e-posta gönder** eyleminin **gövde** çıkış değeri olarak ayarlanır.

      ![İş akışındaki her adımın durumunu ve genişletilmiş "yanıt" eyleminde giriş ve çıkışları gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/response-action-details-body-property.png)

1. Tasarımcıda **Kaydet**' i seçin.

<a name="retest-workflow"></a>

## <a name="retest-your-logic-app"></a>Mantıksal uygulamanızı yeniden test etme

Mantıksal uygulamanızda güncelleştirme yaptıktan sonra, Visual Studio 'da hata ayıklayıcıyı yeniden çalıştırarak ve güncelleştirilmiş mantıksal uygulamanızı tetiklemek için başka bir istek göndererek, [hata ayıklama ve mantıksal uygulamanızı test](#debug-test-locally)etme bölümündeki adımlara benzer bir test çalıştırabilirsiniz.

1. Visual Studio Code araç çubuğunda, **Çalıştır** menüsünü açın ve **hata ayıklamayı Başlat** (F5) seçeneğini belirleyin.

1. İstek oluşturmaya ve göndermeye yönelik Postman veya aracınız içinde, iş akışınızı tetiklemek için başka bir istek gönderin.

1. Durum bilgisi olan bir iş akışı oluşturduysanız, iş akışının genel bakış sayfasında en son çalıştırmanın durumunu kontrol edin. Söz konusu çalıştırmada her adımın durumunu, girişlerini ve çıkışlarını görüntülemek için, bu çalıştırma için üç nokta (**...**) düğmesini seçin ve **çalıştırmayı göster**' i seçin.

   Örneğin, örnek iş akışı yanıt eylemiyle güncelleştirildikten sonra bir çalıştırmanın adım adım durumu aşağıda verilmiştir.

   ![Güncelleştirilmiş iş akışındaki her adımın durumunu ve genişletilmiş "yanıt" eyleminde giriş ve çıkışları gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-details-rerun.png)

1. Hata ayıklama oturumunu durdurmak için, **Çalıştır** menüsünde, **hata ayıklamayı Durdur** (SHIFT + F5) öğesini seçin.

<a name="publish-azure"></a>

## <a name="publish-to-azure"></a>Azure’da Yayımlama

Visual Studio Code, projenizi doğrudan Azure 'a dağıtabilirsiniz. Bu, yeni **mantıksal uygulama (Önizleme)** kaynak türünü kullanarak mantıksal uygulamanızı yayımlar. Azure Işlevlerinde işlev uygulaması kaynağına benzer şekilde, bu yeni kaynak türüne yönelik dağıtım, dağıtım sırasında ayarlayabileceğiniz bir [barındırma planı ve fiyatlandırma katmanı](../app-service/overview-hosting-plans.md)seçmenizi gerektirir. Barındırma planları ve fiyatlandırma hakkında daha fazla bilgi için şu konuları gözden geçirin:

* [Azure App Service ölçeği büyütme](../app-service/manage-scale-up.md)
* [Azure İşlevleri’ni ölçeklendirme ve barındırma](../azure-functions/functions-scale.md)

Mantıksal uygulamanızı, [işlev uygulaması gereksinimlerine benzer şekilde, Azure depolama hesabı](../azure-functions/storage-considerations.md)gibi ek gerekli kaynakları otomatik olarak oluşturan yeni bir kaynak olarak yayımlayabilirsiniz. Ya da mantıksal uygulamanızı daha önce dağıtılan bir **mantıksal uygulama (Önizleme)** kaynağına yayımlayabilirsiniz ve bu mantıksal uygulamanın üzerine yazar.

### <a name="publish-as-a-new-logic-app-preview-resource"></a>Yeni bir mantıksal uygulama (Önizleme) kaynağı olarak Yayımla

1. Visual Studio Code araç çubuğunda Azure simgesini seçin.

1. **Azure: Logic Apps (Önizleme)** bölmesi araç çubuğunda, **mantıksal uygulamaya dağıt**' ı seçin.

   !["Mantıksal uygulamaya dağıt" seçiliyken "Azure: Logic Apps (Önizleme)" bölmesinin ve bölmesinin araç çubuğunun gösterildiği ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/deploy-to-logic-app.png)

1. Visual Studio Code açılan listeden aşağıdaki seçeneklerden birini belirleyin:

   * **Azure 'da yeni mantıksal uygulama (Önizleme) oluşturma** (hızlı)
   * **Azure gelişmiş 'de yeni mantıksal uygulama (Önizleme) oluşturma**
   * Varsa, önceden dağıtılan bir **mantıksal uygulama (Önizleme)** kaynağı

   Bu örnek, **Azure Gelişmiş ' te yeni mantıksal uygulama (Önizleme) oluşturma**ile devam eder.

   !["Azure 'da yeni mantıksal uygulama (Önizleme) oluşturma" seçiliyken "Azure: Logic Apps (Önizleme)" bölmesini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-create-logic-app-options.png)

1. Yeni **mantıksal uygulama (Önizleme)** kaynağını oluşturmak için aşağıdaki adımları izleyin:

   1. **Mantıksal uygulama (Önizleme)** kaynağı için kullanılacak ad olan yeni mantıksal uygulamanız için genel olarak benzersiz bir ad sağlayın. Bu örnekte, kullanılır `example-logic-app-preview` .

      !["Azure: Logic Apps (Önizleme)" bölmesinin ve oluşturulacak yeni mantıksal uygulama için bir ad sağlamak üzere bir istem gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/enter-logic-app-name.png)

   1. Plan veya [**Premium**](../azure-functions/functions-scale.md#premium-plan) [**App Service**](../azure-functions/functions-scale.md#app-service-plan) yeni mantıksal uygulamanız için bir barındırma planı seçin. Bu örnek **App Service planını**seçer.

      !["Azure: Logic Apps (Önizleme)" bölmesinin ve "App Service planı" veya "Premium" seçilecek bir istem gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-hosting-plan.png)

   1. Yeni bir App Service planı oluşturun veya var olan bir planı seçin. Bu örnek **yeni App Service planı oluştur**' a seçer.

      !["Azure: Logic Apps (Önizleme)" bölmesini ve "Yeni App Service planı oluşturma" istemi ve var olan bir App Service planı seçme istemi gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/create-app-service-plan.png)

   1. App Service planınız için bir ad girin ve ardından plan için bir [fiyatlandırma katmanı](../app-service/overview-hosting-plans.md) seçin. Bu örnek **F1 ücretsiz** planını seçer.

      !["Azure: Logic Apps (Önizleme)" bölmesinin ve bir fiyatlandırma katmanını seçmek için bir istem gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/select-pricing-tier.png)

   1. En iyi performans için, dağıtım için projenizle aynı kaynak grubunu bulun ve seçin.

      > [!NOTE]
      > Farklı bir kaynak grubu oluşturabilseniz veya kullanabilseniz de, bu durum performansı etkileyebilir. Farklı bir kaynak grubu oluşturur veya seçer, ancak onay istemi görüntülendikten sonra iptal edilirse, dağıtımınız de iptal edilir.

   1. Durum bilgisi olan iş akışları için **Yeni depolama hesabı** veya mevcut bir depolama hesabı oluştur ' u seçin.

      !["Azure: Logic Apps (Önizleme)" bölmesini gösteren ekran görüntüsü ve bir depolama hesabı oluşturmak veya seçmek için bir istem.](./media/create-stateful-stateless-workflows-visual-studio-code/create-storage-account.png)

   1. Daha kolay tanılama günlüğü ve izleme özelliği için mevcut bir Application Insights kaynağı seçebilirsiniz. Aksi halde, uygulamanızı dağıttıktan sonra **yeni Application Insights kaynak oluştur**' u veya Azure Portal Application Insights ayarlayabilirsiniz ' ı seçebilirsiniz.

      Dağıtmadan önce, `logLevel` nesnesini `logging` projenizin kök düzeyinde var olan **host.js** nesnesine eklediğinizden emin olun ve `Host.Triggers.Workflow` `Information` ' ı ' e ayarlayın, örneğin:

      ```json
      "logLevel": {
         "Host.Triggers.Workflow": "Information"
      },
      ```

      Dosyadaki **host.js** şöyle görünebilir:

      ```json
      {
         "version": "2.0",
         "logging": {
            "applicationInsights": {
               "samplingExcludedTypes": "Request",
               "samplingSettings": {
                  "isEnabled": true
               }
            },
            "logLevel": {
               "Host.Triggers.Workflow": "Information"
            }
         }
      }
      ``` 

   İşiniz bittiğinde Visual Studio Code mantıksal uygulamanızı yayımlamak için gerekli kaynakları oluşturmaya ve dağıtmaya başlar.

1. Dağıtım sürecini gözden geçirmek ve izlemek için, **Görünüm** menüsünde **Çıkış**' ı seçin. Çıkış penceresi araç çubuğu listesinden **Azure Logic Apps**' yi seçin.

   ![Dağıtım ilerleme durumu ve durumları ile birlikte araç çubuğu listesinde seçili olan "Azure Logic Apps" ile çıkış penceresini gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/logic-app-deployment-output-window.png)

   Visual Studio Code mantıksal uygulama iş akışınızı Azure 'a dağıtmaya tamamlandığında bu ileti görüntülenir:

   ![Azure 'a dağıtımın başarıyla tamamlandığını belirten bir ileti gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/deployment-to-azure-completed.png)

   Tebrikler, mantıksal uygulamanız artık Azure 'da canlı ve varsayılan olarak etkinleştirilmiştir.

Daha sonra, bu görevleri nasıl gerçekleştireceğinizi öğrenebilirsiniz:

* Azure portal veya [Visual Studio Code](#find-manage-deployed-workflows-vs-code) [dağıtılan mantıksal uygulamanızı bulun](#find-manage-deployed-workflows-portal) .

* [Durum bilgisiz mantıksal uygulama iş akışlarında çalıştırma geçmişini etkinleştirin](#run-history).

* [Dağıtılan bir **mantıksal uygulama (Önizleme)** kaynağında izlemeyi etkinleştirin](#enable-monitoring).

<a name="find-manage-deployed-workflows-vs-code"></a>

## <a name="find-and-manage-deployed-logic-apps-in-visual-studio-code"></a>Visual Studio Code dağıtılan mantıksal uygulamaları bulun ve yönetin

Visual Studio Code, özgün **Logic Apps** veya **mantıksal uygulama (Önizleme)** kaynak türü olup olmadıkları ve bu mantıksal uygulamaları yönetmenize yardımcı olan görevleri seçmek üzere Azure aboneliğinizde dağıtılan tüm mantıksal uygulamaları görüntüleyebilirsiniz. Ancak, her iki kaynak türüne de erişmek için, Visual Studio Code için hem **Azure Logic Apps** hem de **Azure Logic Apps (Önizleme)** uzantılarına sahip olmanız gerekir.

1. Sol araç çubuğunda Azure simgesini seçin. **Azure: Logic Apps (Önizleme)** bölmesinde, bu abonelik için dağıtılan tüm mantıksal uygulamaları gösteren aboneliğinizi genişletin.

1. Yönetmek istediğiniz mantıksal uygulamayı bulun ve seçin. Mantıksal uygulamanın kısayol menüsünü açın ve gerçekleştirmek istediğiniz görevi seçin.

   Örneğin, dağıtılan mantıksal uygulamanızı durdurma, başlatma, yeniden başlatma veya silme gibi görevleri seçebilirsiniz.

   ![Açılan "Azure Logic Apps (Önizleme)" uzantı bölmesi ve dağıtılan iş akışı ile Visual Studio Code gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/find-deployed-workflow-visual-studio-code.png)

1. Mantıksal uygulamadaki tüm iş akışlarını görüntülemek için mantıksal uygulamanızı genişletin ve sonra **Iş akışları** düğümünü genişletin.

1. Belirli bir iş akışını görüntülemek için, iş akışının kısayol menüsünü açın ve **tasarımcıda aç**' ı seçerek iş akışını salt okunurdur modda açın.

   İş akışını düzenlemek için şu seçeneklere sahipsiniz:

   * Visual Studio Code ' de, mantıksal uygulama tasarımcısında dosya ** üzerinde projenizinworkflow.js** açın, düzenlemelerinizi yapın ve mantıksal uygulamanızı Azure 'a yeniden dağıtın.

   * Azure portal, [mantıksal uygulamanızı bulun ve açın](#find-manage-deployed-workflows-portal). İş akışını bulun, düzenleyin ve kaydedin.

1. Azure portal dağıtılan mantıksal uygulamayı açmak için mantıksal uygulamanın kısayol menüsünü açın ve **portalda aç**' ı seçin.

   Azure portal tarayıcınızda açılır, Visual Studio Code oturumu açtıysanız ve mantıksal uygulamanızı gösteriyorsa, portalda otomatik olarak oturum açar.

   ![Visual Studio Code ' de mantıksal uygulamanızın Azure portal sayfasını gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-workflow-azure-portal.png)

   Ayrıca, Azure portal ayrı olarak oturum açabilir, mantıksal uygulamanızı bulmak için Portal arama kutusunu kullanabilir ve sonra sonuçlar listesinden mantıksal uygulamanızı seçebilirsiniz.

   ![Azure portal ve arama çubuğunu, dağıtılan mantıksal uygulama için arama sonuçlarıyla gösteren ve seçili görüntülenen ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/find-deployed-workflow-azure-portal.png)

<a name="find-manage-deployed-workflows-portal"></a>

## <a name="find-and-manage-deployed-logic-apps-in-the-portal"></a>Portalda dağıtılan Logic Apps bulma ve yönetme

Azure portal, Azure aboneliğinizdeki tüm dağıtılan mantıksal uygulamaları, özgün **Logic Apps** kaynak türü veya **mantıksal uygulama (Önizleme)** kaynak türü olup olmadıkları gibi görüntüleyebilirsiniz. Şu anda her kaynak türü Azure 'da ayrı kategoriler olarak düzenlenir ve yönetilir.

> [!NOTE]
> Genel önizleme için, yalnızca Azure portal dağıtılan **mantıksal uygulama (Önizleme)** kaynaklarını görüntüleyebilir, yeni **mantıksal uygulama (Önizleme)** kaynakları oluşturmamalıdır. Bu mantıksal uygulamaları yalnızca Visual Studio Code için oluşturabilirsiniz. Ancak, bu kaynak türüyle dağıtılan Logic Apps 'e [iş akışları ekleyebilirsiniz](#add-workflows) .

**Mantıksal uygulama (Önizleme)** kaynak türüne sahip mantıksal uygulamaları bulmak için şu adımları izleyin:

1. Azure portal arama kutusuna girin `logic app preview` . Sonuçlar listesi göründüğünde, **Hizmetler**altında **mantıksal uygulama (Önizleme)** öğesini seçin.

   !["Mantıksal uygulama önizlemesi" arama metniyle Azure portal arama kutusunu gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/portal-find-logic-app-preview-resource.png)

1. **Mantıksal uygulama (Önizleme)** bölmesinde, Visual Studio Code dağıttığınız mantıksal uygulamayı bulun ve seçin.

   ![Azure 'da dağıtılan Azure portal ve mantıksal uygulama (Önizleme) kaynaklarını gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/logic-app-preview-resources-pane.png)

   Azure portal, seçilen mantıksal uygulamanın tek tek kaynak sayfasını açar.

   ![Mantıksal uygulama iş akışınızın kaynak sayfasını Azure portal gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-workflow-azure-portal.png)

1. Bu mantıksal uygulamadaki iş akışlarını görüntülemek için mantıksal uygulamanın menüsünde **Iş akışları**' nı seçin.

   **Iş akışları** bölmesi, geçerli mantıksal uygulamadaki tüm iş akışlarını gösterir. Bu örnek, Visual Studio Code oluşturduğunuz iş akışını gösterir.

   !["Iş akışları" bölmesi açık olan "Logic App (Önizleme)" kaynak sayfasını ve dağıtılan iş akışını gösteren ekran görüntüsü](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-logic-app-workflows-pane.png)

1. Bir iş akışını görüntülemek için, iş **akışları** bölmesinde o iş akışını seçin.

   İş akışı bölmesi açılır ve bu iş akışında gerçekleştirebileceğiniz daha fazla bilgi ve görev gösterir.

   Örneğin, iş akışındaki adımları görüntülemek için **Tasarımcı**' yı seçin.

   ![Seçilen iş akışının "genel bakış" bölmesini gösteren ekran görüntüsü, iş akışı menüsü seçili "tasarımcı" komutunu gösterir.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-overview-pane-select-designer.png)

   Mantıksal uygulama Tasarımcısı açılır ve Visual Studio Code içinde derlediniz iş akışını gösterir. Artık Azure portal bu iş akışında değişiklik yapabilirsiniz.

   ![Visual Studio Code 'ten dağıtılan mantıksal uygulama tasarımcısını ve iş akışını gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/opened-workflow-designer.png)

<a name="add-workflows"></a>

## <a name="add-a-workflow-to-deployed-logic-apps"></a>Dağıtılan Logic Apps 'e iş akışı ekleme

Azure portal aracılığıyla, Visual Studio Code dağıttığınız bir **mantıksal uygulama (Önizleme)** kaynağına boş iş akışları ekleyebilir ve bu iş akışlarını Azure Portal oluşturabilirsiniz.

1. [Azure Portal](https://portal.azure.com), dağıtılan **mantıksal uygulama (Önizleme)** kaynağını bulup seçin.

1. Mantıksal uygulamanın menüsünde, **Iş akışları**' nı seçin. **Iş akışları** bölmesinde **Ekle**' yi seçin.

   ![Seçilen mantıksal uygulamanın "Iş akışları" bölmesini ve araç çubuğunu "Ekle" komutu seçiliyken gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/add-new-workflow.png)

1. **Yeni iş akışı** bölmesinde iş akışı için ad belirtin. **Durum bilgisi olan** veya **durum bilgisiz** **>** **Oluştur**seçeneğini belirleyin.

   Azure, **Iş akışları** bölmesinde görüntülenen yeni iş akışınızı dağıttığından, yönetim ve mantıksal uygulama Tasarımcısı veya kod görünümü açma gibi diğer görevleri gerçekleştirmek için bu iş akışını seçin.

   ![Seçili iş akışını yönetim ve gözden geçirme seçenekleriyle gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/view-new-workflow.png)

   Örneğin, yeni bir iş akışı için tasarımcıyı açmak boş bir tuval gösterir. Artık Azure portal bu iş akışını oluşturabilirsiniz.

   ![Mantıksal uygulama tasarımcısını ve boş bir iş akışını gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/opened-blank-workflow-designer.png)

<a name="run-history"></a>

## <a name="run-history-for-stateless-logic-app-workflows"></a>Durum bilgisiz mantıksal uygulama iş akışları için çalışma geçmişi

Durum bilgisi olmayan bir mantıksal uygulama iş akışında daha kolay hata ayıklamak için, bu iş akışı için Visual Studio Code veya Azure portal bir çalıştırma geçmişi etkinleştirebilir ve sonra işiniz bittiğinde çalıştırma geçmişini devre dışı bırakabilirsiniz.

### <a name="for-a-stateless-logic-app-workflow-in-visual-studio-code"></a>Visual Studio Code durum bilgisiz mantıksal uygulama iş akışı için

Üzerinde çalışıyorsanız ve durum bilgisiz Logic App iş akışını Visual Studio Code yerel olarak çalıştırıyorsanız, aşağıdaki adımları izleyin:

1. Projenizde **iş akışı-tasarımsaati** klasörünü bulun ve genişletin. Dosyasında **local.settings.js** bulun ve açın.

1. Özelliği ekleyin `Workflows.{yourWorkflowName}.operationOptions` ve değeri olarak ayarlayın `WithStatelessRunHistory` , örneğin:

   ```json
   {
      "IsEncrypted": false,
      "Values": {
         "AzureWebJobsStorage": "UseDevelopmentStorage=true",
         "FUNCTIONS_WORKER_RUNTIME": "dotnet",
         "Workflows.{yourWorkflowName}.OperationOptions": "WithStatelessRunHistory"
      }
   }
   ```

1. İşiniz bittiğinde çalıştırma geçmişini devre dışı bırakmak için, `Workflows.{yourWorkflowName}.OperationOptions` özelliği ve değerini silin ya da özelliğini olarak ayarlayın `None` .

### <a name="for-a-stateless-logic-app-workflow-in-the-azure-portal"></a>Azure portal durum bilgisiz mantıksal uygulama iş akışı için

Projenizi zaten Azure portal dağıttıysanız, şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **mantıksal uygulama (Önizleme)** kaynağını bulup açın.

1. Mantıksal uygulamanın menüsünde, **Ayarlar**' ın altında **yapılandırma**' yı seçin.

1. **Uygulama ayarları** sekmesinde **Yeni uygulama ayarı**' nı seçin.

1. **Uygulama ayarı Ekle/Düzenle** bölmesinde, **ad** kutusuna bu işlem seçeneği adını girin: 

   `Workflows.{yourWorkflowName}.OperationOptions`

1. **Değer** kutusuna aşağıdaki değeri girin:`WithStatelessRunHistory`

   Örneğin:

   ![Azure portal ve mantıksal uygulama (Önizleme) kaynağını "yapılandırma" > "yeni uygulama ayarı" < "uygulama ayarı Ekle/Düzenle" bölmesi açık ve "Iş akışları" olarak gösteren ekran görüntüsü. {yourWorkflowName}. OperationOptions "seçeneği" WithStatelessRunHistory "olarak ayarlandı.](./media/create-stateful-stateless-workflows-visual-studio-code/stateless-operation-options-run-history.png)

1. İşiniz bittiğinde **Tamam**’ı seçin. **Yapılandırma** bölmesinde **Kaydet**' i seçin.

Dağıtılan mantıksal uygulama (Önizleme) kaynağında izlemeyi etkinleştirmek için sonraki bölüme geçin.

<a name="enable-monitoring"></a>

## <a name="enable-monitoring-for-deployed-logic-app-preview-resources"></a>Dağıtılan mantıksal uygulama (Önizleme) kaynakları için izlemeyi etkinleştir

Dağıtılan bir **mantıksal uygulama (Önizleme)** kaynağında izlemeyi etkinleştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)dağıtılan **mantıksal uygulama (Önizleme)** kaynağını bulup seçin.

1. Bu kaynağın menüsünde, **API**altında **CORS**' yi seçin.

1. **CORS** bölmesinde, **izin verilen**kaynaklar ' ın altında, joker karakter (*) ekleyin.

1. İşiniz bittiğinde, **CORS** araç çubuğundan **Kaydet**' i seçin.

   ![Dağıtılan bir Logic Apps (Önizleme) kaynağıyla Azure portal gösteren ekran görüntüsü. "CORS" kaynak menüsünde "Izin verilen kaynaklar" joker karakter "*" karakterine ayarlanmış yeni bir girdiyle birlikte seçilir.](./media/create-stateful-stateless-workflows-visual-studio-code/enable-run-history-deployed-logic-app.png)

<a name="deploy-docker"></a>

## <a name="deploy-to-docker-container"></a>Docker kapsayıcısına dağıt

[.NET Core komut satırı arabirimi (CLI) aracını](/dotnet/core/tools/)kullanarak projenizi derleyebilir ve sonra yapınızı yayımlayabilirsiniz. Daha sonra, mantıksal uygulama iş akışınızı dağıtmak için hedef olarak bir [Docker kapsayıcısı](/visualstudio/docker/tutorials/docker-tutorial#what-is-a-container) oluşturup kullanabilirsiniz. Daha fazla bilgi için şu konuları gözden geçirin:

* [Kapsayıcılar ve Docker’a Giriş](/dotnet/architecture/microservices/container-docker-introduction/)
* [.NET ve Docker’a Giriş](/dotnet/core/docker/introduction)
* [Docker terimleri](/dotnet/architecture/microservices/container-docker-introduction/docker-terminology)
* [Öğretici: Docker 'ı kullanmaya başlama](/visualstudio/docker/tutorials/docker-tutorial)

1. Projenizi derlemek için bir komut satırı istemi açın ve şu komutu çalıştırın:

   `dotnet build -c release`

   Daha fazla bilgi için [DotNet derleme](/dotnet/core/tools/dotnet-build/) başvurusu sayfasına bakın.

1. Şu komutu çalıştırarak derlemenizi yayımlayın:

   `dotnet publish -c release`

   Daha fazla bilgi için [DotNet Publish](/dotnet/core/tools/dotnet-publish/) başvuru sayfasına bakın.

1. Bir .NET iş akışı için Docker dosyası kullanarak bir Docker kapsayıcısı oluşturun ve şu komutu çalıştırın:

   `docker build --tag local/workflowcontainer .`

   Örneğin, bir durum bilgisi olan mantıksal uygulama dağıtan ve mantıksal uygulamayı Azure portal yayımlamak için kullanılan Azure depolama hesabı için bağlantı dizesini belirten örnek bir Docker dosyası aşağıda verilmiştir. Azure portal depolama hesabının bağlantı dizesini bulmak ve kopyalamak için, [depolama hesabı anahtarlarını Yönet](../storage/common/storage-account-keys-manage.md?tabs=azure-portal#view-account-access-keys)' i gözden geçirin.

   ![Depolama hesabı erişim anahtarları ve bağlantı dizesinin kopyalandığı Azure portal gösteren ekran görüntüsü.](./media/create-stateful-stateless-workflows-visual-studio-code/find-storage-account-connection-string.png)

   Bağlantı dizesi şu örneğe benzer şekilde görünür:

   `DefaultEndpointsProtocol=https;AccountName=fabrikamstorageaccount;AccountKey={access-key};EndpointSuffix=core.windows.net`

   Docker dosyasının biçimi aşağıdadır:


   ```text
   FROM mcr.microsoft.com/azure-functions/dotnet:3.0.14492-appservice

   ENV AzureWebJobsStorage <storage-account-connection-string>
   ENV AZURE_FUNCTIONS_ENVIRONMENT Development
   ENV AzureWebJobsScriptRoot=/home/site/wwwroot
   ENV AzureFunctionsJobHost__Logging__Console__IsEnabled=true
   ENV FUNCTIONS_V2_COMPATIBILITY_MODE=true

   COPY ./bin/Release/netcoreapp3.1/publish/ /home/site/wwwroot
   ```

   Daha fazla bilgi için bkz. [Docker Build](https://docs.docker.com/engine/reference/commandline/build/).

1. Dizeyi daha sonra Visual Studio Code ' de mantıksal uygulamanızı oluşturmak için kullandığınız projedeki dosyalardaki **local.settings.js** ekleyebilmeniz için dizeyi güvenli bir yere kaydedin.

1. Şu komutu kullanarak kapsayıcıyı yerel olarak çalıştırın:

   `docker run -e WEBSITE_HOSTNAME=localhost -p 8080:80 local/workflowcontainer`

   Daha fazla bilgi için bkz. [Docker Run](https://docs.docker.com/engine/reference/commandline/run/).

1. Istek tetikleyicisinin geri çağırma URL 'sini almak için şu isteği gönderin:

   `POST /runtime/webhooks/workflow/api/management/workflows/{workflow-name}/triggers/{trigger-name}/listCallbackUrl?api-version=2019-10-01-edge-preview&code={master-key}`

   <*ana anahtar*> değeri `AzureWebJobsStorage` , bu bölümdeki değeri bulabileceğiniz Azure **-WebJobs-gizlilikler/{Deployment-Name}/host.js**dosyasında için ayarladığınız Azure depolama hesabında tanımlanmıştır:

   ```json
   {
     <...>
     "masterKey": {
        "name": "master",
        "value": "<master-key>",
        "encrypted": false
     },
     <...>
   }
   ```

<a name="nested-behavior"></a>

## <a name="nested-behavior-differences-between-stateful-and-stateless-logic-apps"></a>Durum bilgisiz ve durum bilgisi içermeyen mantıksal uygulamalar arasındaki iç

[Bir mantıksal uygulama iş akışını](../logic-apps/logic-apps-http-endpoint.md) , [Istek](../connectors/connectors-native-reqres.md) tetikleyicisi, [http Web kancası](../connectors/connectors-native-webhook.md) tetikleyicisi veya [apiconnectionwehook türüne](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) sahip yönetilen bağlayıcı Tetikleyicileri kullanarak aynı **Logic App (Önizleme)** kaynağında bulunan DIĞER mantıksal uygulama iş akışlarından çağrılabilir hale getirebilirsiniz ve https isteklerini alabilir.

Ana iş akışı alt iş akışını çağırdığında, iç içe mantıksal uygulama iş akışlarının izleyebildiğiniz davranış desenleri aşağıda verilmiştir:

* Zaman uyumsuz yoklama kriteri

  Üst öğe, ilk çağrısına yanıt beklemez, ancak alt çalışmayı bitirene kadar alt öğenin çalışma geçmişini sürekli olarak denetler. Varsayılan olarak, durum bilgisi olan iş akışları, [istek zaman aşımı sınırlarını](../logic-apps/logic-apps-limits-and-config.md)aşabileceğinden uzun süre çalışan alt iş akışları için ideal olan bu kalıbı izler.

* Zaman uyumlu model ("ateş ve unut")

  Alt öğe, hemen bir yanıt döndürerek çağrıyı onaylar `202 ACCEPTED` ve üst öğe, alt öğeden gelen sonuçları beklemeden bir sonraki eyleme devam eder. Bunun yerine, alt öğe çalışmayı bitirdiğinde sonuçları alır. Yanıt eylemi içermeyen, durum bilgisi olan alt iş akışları her zaman zaman uyumlu düzende takip eder. Alt durum bilgisi olan iş akışlarında, çalışma geçmişi gözden geçirmeniz için kullanılabilir.

  Bu davranışı etkinleştirmek için iş akışının JSON tanımında `operationOptions` özelliğini olarak ayarlayın `DisableAsyncPattern` . Daha fazla bilgi için bkz. [tetikleyici ve eylem türleri-işlem seçenekleri](../logic-apps/logic-apps-workflow-actions-triggers.md#operation-options).

* Tetikleme ve bekleme

  Alt durum bilgisi olmayan bir iş akışı için üst öğe, alt öğeden sonuçları döndüren bir yanıt bekler. Bu model, bir alt iş akışını çağırmak için yerleşik [http tetikleyicisi veya eylemini](../connectors/connectors-native-http.md) kullanmayla benzerdir. Yanıt eylemi içermeyen alt durum bilgisiz iş akışları hemen bir `202 ACCEPTED` yanıt döndürür, ancak üst öğe sonraki eyleme geçmeden önce alt öğenin bitmesini bekler. Bu davranışlar yalnızca alt durum bilgisiz iş akışları için geçerlidir.

Bu tablo, üst ve alt öğenin durum bilgisiz, durumsuz veya karışık iş akışı türleri olup olmadığına bağlı olarak alt iş akışının davranışını belirtir:

| Üst iş akışı | Alt iş akışı | Alt davranış |
|-----------------|----------------|----------------|
| Durum Bilgisi Olan | Durum Bilgisi Olan | Ayar ile zaman uyumsuz veya zaman uyumlu `"operationOptions": "DisableAsyncPattern"` |
| Durum Bilgisi Olan | Durum bilgisi olmayan | Tetikleme ve bekleme |
| Durum bilgisi olmayan | Durum Bilgisi Olan | Zaman Uyumlu |
| Durum bilgisi olmayan | Durum bilgisi olmayan | Tetikleme ve bekleme |
||||

## <a name="limits"></a>Sınırlar

[Azure Logic Apps için mevcut limitlerin](../logic-apps/logic-apps-limits-and-config.md) çoğu bu kaynak türü için aynı olsa da, bu genel önizleme uzantısındaki farklılıklar aşağıda verilmiştir:

* Yönetilen bağlayıcılar: bağlantı başına dakika başına 50 istek

* JavaScript eylemi [Için satır Içi kod eylemi](../logic-apps/logic-apps-add-run-inline-code.md) için bu sınırlar değiştirilmiştir:

  * Kod karakterlerinin sınırı 1.024 karakterden 100.000 karaktere kadar artar.

  * Kodun çalıştırılacağı zaman, beş saniyeden 15 saniyeye kadar artar.

## <a name="next-steps"></a>Sonraki adımlar

Bu genel önizleme uzantısıyla ilgili deneyimleriniz hakkında görüşlerinizi öğrenmek istiyoruz!

* Hatalar veya sorunlar için [sorunlarınızı GitHub ' da oluşturun](https://github.com/Azure/logicapps/issues).
* Sorular, istekler, açıklamalar ve diğer geri bildirimler için [Bu geri bildirim formunu kullanın](https://aka.ms/lafeedback).
