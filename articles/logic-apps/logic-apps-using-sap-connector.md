---
title: SAP sistemlerine bağlanma
description: Azure Logic Apps ile iş akışlarını otomatikleştirerek SAP kaynaklarına erişin ve yönetin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, daviburg, logicappspm
ms.topic: article
ms.date: 05/29/2020
tags: connectors
ms.openlocfilehash: 557e162d9d7f0238d5554c32cb3ae96885877dbe
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220496"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Azure Logic Apps’ten SAP sistemlerine bağlanma

> [!IMPORTANT]
> Önceki SAP uygulama sunucusu ve SAP Ileti sunucusu bağlayıcıları, 29 Şubat 2020 ' de kullanımdan kaldırılmıştır. Geçerli SAP Bağlayıcısı, bu önceki SAP bağlayıcılarını, bağlantı türünü değiştirmek zorunda kalmazsınız, önceki bağlayıcılarla tamamen uyumlu, birçok ek özellik sağlar ve SAP .net bağlayıcı kitaplığı 'nı (SAP NCo) kullanmaya devam eder.
>
> Eski bağlayıcıları kullanan Logic Apps için, kullanımdan kaldırma tarihinden önce lütfen [en son bağlayıcıya geçiş](#migrate) yapın. Aksi takdirde, bu Logic Apps yürütme hatalarıyla karşılaşacaktır ve SAP sisteminize ileti gönderemeyecektir.

Bu makalede SAP bağlayıcısını kullanarak bir mantık uygulamasının içinden şirket içi SAP kaynaklarınıza nasıl erişebileceğiniz gösterilmektedir. Bağlayıcı, şirket içi R/3 ve ECC sistemleri gibi SAP 'nin klasik sürümleriyle birlikte kullanılabilir. Bağlayıcı Ayrıca SAP 'nin şirket içinde veya bulutta barındırıldığından, S/4 HANA gibi daha yeni bir HANA tabanlı SAP sistemleri ile tümleştirmeyi de mümkün. SAP Bağlayıcısı, ara belge (IDoc), Iş uygulaması programlama arabirimi (BAPı) veya uzak Işlev çağrısı (RFC) aracılığıyla SAP NetWeaver tabanlı sistemlere ileti veya veri tümleştirmeyi destekler.

SAP Bağlayıcısı [sap .net bağlayıcı (NCo) kitaplığını](https://support.sap.com/en/product/connectors/msnet.html) kullanır ve şu eylemleri sağlar:

* **SAP 'ye Ileti gönderin**: tRFC üzerinden IDoc GÖNDERIN, RFC üzerinde BAPI işlevlerini ÇAĞıRıN veya SAP sistemlerinde RFC/tRFC çağrısı yapın.

* **SAP 'den bir ileti alındığında**: tRFC üzerinde IDoc alma, TRFC üzerinde BAPI işlevleri ÇAĞıRMA veya SAP sistemlerinde RFC/tRFC çağırma.

* **Şemalar oluşturma**: ıDOC, BAPı veya RFC için SAP yapıtları için şemalar oluşturun.

Bu işlemler için SAP Bağlayıcısı, Kullanıcı adları ve parolalar aracılığıyla temel kimlik doğrulamasını destekler. Bağlayıcı Ayrıca [güvenli ağ iletişimini (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true)destekler. SNC, SAP NetWeaver çoklu oturum açma (SSO) veya harici bir güvenlik ürünü tarafından sunulan ek güvenlik özellikleri için kullanılabilir.

Bu makalede, daha önce açıklanan tümleştirme senaryolarını kapsayan, SAP ile tümleştirilen örnek mantık uygulamalarının nasıl oluşturulacağı gösterilmektedir. Eski SAP bağlayıcılarını kullanan Logic Apps için, bu makalede mantıksal uygulamalarınızın en son SAP bağlayıcısına nasıl geçirileceği gösterilmektedir.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyle birlikte takip etmek için şu öğelere ihtiyacınız vardır:

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* SAP sisteminize erişmek istediğiniz mantıksal uygulama ve mantıksal uygulamanızın iş akışını başlatan bir tetikleyici. Logic Apps 'e yeni başladıysanız bkz. [Azure Logic Apps nedir? nedir?](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: Ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* [SAP uygulama sunucunuz](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) veya [SAP ileti sunucunuz](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm).

* SAP sunucunuza gönderebilmeniz için örnek bir IDoc dosyası gibi ileti içeriği, XML biçiminde olmalıdır ve kullanmak istediğiniz SAP eyleminin ad alanını içermelidir.

* **SAP tetikleyicisinden bir ileti alındığında** kullanmak için, bu kurulum adımlarını da gerçekleştirmeniz gerekir:

  * Bu ayarla SAP Gateway Güvenlik izinlerinizi ayarlayın:

    `"TP=Microsoft.PowerBI.EnterpriseGateway HOST=<gateway-server-IP-address> ACCESS=*"`

  * Access Control listesi (ACL) hatalarını bulmaya yardımcı olan ve varsayılan olarak etkin olmayan SAP Gateway Güvenlik günlüğü 'nü ayarlayın. Aksi takdirde, şu hatayı alırsınız:

    `"Registration of tp Microsoft.PowerBI.EnterpriseGateway from host <host-name> not allowed"`

    Daha fazla bilgi için, [ağ geçidi günlüğünü ayarlama](https://help.sap.com/erp_hcm_ias2_2015_02/helpdata/en/48/b2a710ca1c3079e10000000a42189b/frameset.htm), SAP yardım konusuna bakın.

<a name="multi-tenant"></a>

### <a name="multi-tenant-azure-prerequisites"></a>Çok kiracılı Azure önkoşulları

Bu Önkoşullar, Logic Apps çok kiracılı Azure 'da çalıştığında ve bir [tümleştirme hizmeti ortamında (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)yerel olarak ÇALıŞTıRMAYAN yönetilen SAP bağlayıcısını kullanmak istediğinizde geçerlidir. Aksi halde, Premium düzeyinde bir ıSE kullanıyorsanız ve ıSE 'de yerel olarak çalışan SAP bağlayıcısını kullanmak istiyorsanız, bkz. [Integration Service Environment (ISE) önkoşulları](#sap-ise).

Yönetilen (ıSE) SAP Bağlayıcısı şirket içi [veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md)aracılığıyla ŞIRKET içi SAP sistemleriyle tümleştirilir. Örneğin, ileti gönder senaryolarında bir mantıksal uygulamadan SAP sistemine bir ileti gönderildiğinde, veri ağ geçidi bir RFC istemcisi işlevi görür ve mantıksal uygulamadan alınan istekleri SAP 'ye iletir. Benzer şekilde, ileti alma senaryolarında, veri ağ geçidi SAP 'den gelen istekleri alan ve mantıksal uygulamaya ileten bir RFC sunucusu işlevi görür.

* Şirket [içi veri ağ geçidini yerel bilgisayarınıza indirip yükleyin](../logic-apps/logic-apps-gateway-install.md) . Daha sonra, bu ağ geçidi için Azure portal [bir Azure ağ geçidi kaynağı oluşturun](../logic-apps/logic-apps-gateway-connection.md#create-azure-gateway-resource) . Ağ Geçidi, şirket içi verilere ve kaynaklara güvenli bir şekilde erişmenize yardımcı olur.

  En iyi uygulama olarak, şirket içi veri ağ geçidinin desteklenen bir sürümünü kullandığınızdan emin olun. Microsoft, her ay yeni bir sürüm yayınlar. Şu anda Microsoft son altı sürümü destekliyor. Ağ geçidinizle ilgili bir sorun yaşıyorsanız, sorunu çözmek için güncelleştirmeleri içerebilen [en son sürüme yükseltmeyi](https://aka.ms/on-premises-data-gateway-installer)deneyin.

* [En son SAP istemci kitaplığını](#sap-client-library-prerequisites) şirket içi veri ağ geçidiyle aynı bilgisayara indirip yükleyin.

<a name="sap-ise"></a>

### <a name="integration-service-environment-ise-prerequisites"></a>Tümleştirme hizmeti ortamı (ıSE) önkoşulları

Bu Önkoşullar, mantıksal uygulamalarınız Premium düzeyinde (Geliştirici düzeyinde değil) bir [tümleştirme hizmeti ortamında (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)çalıştırıldığında ve bır Ise 'de yerel olarak çalışan SAP bağlayıcısını kullanmak istediğinizde geçerlidir. ISE, bir Azure sanal ağı tarafından korunan kaynaklara erişim sağlar ve Logic Apps 'in şirket içi veri ağ geçidini kullanmadan şirket içi kaynaklara doğrudan erişmesini sağlayan diğer ıSE-yerel bağlayıcılar sunar.

> [!NOTE]
> SAP ıSE Bağlayıcısı geliştirici düzeyinde bir ıSE içinde görünür olsa da, bağlayıcıyı yüklemeye yönelik girişimler başarılı olmayacaktır.

1. Henüz bir Azure depolama hesabınız ve bir blob Kapsayıcınız yoksa, [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) ya da [Azure Depolama Gezgini](../storage/blobs/storage-quickstart-blobs-storage-explorer.md)kullanarak bu kapsayıcıyı oluşturun.

1. [En son SAP istemci kitaplığını yerel bilgisayarınıza indirip yükleyin](#sap-client-library-prerequisites) . Aşağıdaki derleme dosyalarına sahip olmanız gerekir:

   * libıuılıdecnumber. dll
   * rscp4n. dll
   * sapnco. dll
   * sapnco_utils. dll

1. Bu derlemeleri içeren bir. zip dosyası oluşturun ve bu paketi Azure Storage 'daki blob kapsayıcınıza yükleyin.

1. Azure portal ya da Azure Depolama Gezgini,. zip dosyasını karşıya yüklediğiniz kapsayıcı konumuna gidin.

1. Paylaşılan erişim Imzası (SAS) belirtecini eklediğinizden emin olmak için bu konumun URL 'sini kopyalayın.

   Aksi takdirde SAS belirteci yetkilendirilmez ve SAP ıSE Bağlayıcısı için dağıtım başarısız olur.

1. SAP ıSE bağlayıcısını kullanabilmeniz için, bağlayıcıyı yüklemeniz ve ıSE 'ye dağıtmanız gerekir.

   1. [Azure Portal](https://portal.azure.com), Ise 'nizi bulun ve açın.
   
   1. Ise menüsünde, **yönetilen bağlayıcılar**  >  **Ekle**' yi seçin. Bağlayıcılar listesinden **SAP**bulun ve seçin.
   
   1. **Yeni bir yönetilen bağlayıcı Ekle** bölmesinde, **SAP paketi** kutusuna SAP derlemeleri olan. zip dosyasının URL 'sini yapıştırın. *SAS belirtecini eklediğinizden emin olun.*

   1. İşiniz bittiğinde **Oluştur**’u seçin.

   Daha fazla bilgi için bkz. [Ise bağlayıcıları ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment).

1. SAP örneğiniz ve ıSE farklı sanal ağlardayken, ıSE 'nin sanal ağının SAP örneğinizin sanal ağına bağlanması için [bu ağları da eşdüzey](../virtual-network/tutorial-connect-virtual-networks-portal.md) yapmanız gerekir.

<a name="sap-client-library-prerequisites"></a>

### <a name="sap-client-library-prerequisites"></a>SAP istemci kitaplığı önkoşulları

* [.NET Framework 4,0-Windows 64-bit (x64) ile derlenen Microsoft .NET 3.0.22.0 Için SAP Bağlayıcısı (NCo 3,0)](https://softwaredownloads.sap.com/file/0020000001000932019)en son sürümünü yüklediğinizden emin olun. Önceki sürümler uyumluluk sorunlarına yol açabilir. Daha fazla bilgi için bkz. [SAP istemci Kitaplığı sürümleri](#sap-library-versions).

* Varsayılan olarak, SAP yükleyicisi derleme dosyalarını varsayılan yükleme klasörüne koyar. Senaryonuza bağlı olarak, bu derleme dosyalarını aşağıdaki gibi başka bir konuma kopyalamanız gerekir:

  Bir ıSE içinde çalışan Logic Apps için [Integration Service ortamı önkoşulları](#sap-ise)bölümünde açıklanan adımları izleyin. Çok kiracılı Azure 'da çalışan ve şirket içi veri ağ geçidini kullanan Logic Apps için, derleme dosyalarını varsayılan yükleme klasöründen veri ağ geçidi yükleme klasörüne kopyalayın. Data Gateway ile ilgili sorunlar yaşıyorsanız, aşağıdaki sorunları gözden geçirin:

  * Data Gateway yalnızca 64 bit sistemlerde çalıştığından SAP istemci kitaplığı için 64-bit sürümünü yüklemelisiniz. Aksi takdirde, veri ağ geçidi ana bilgisayar hizmeti 32 bitlik derlemeleri desteklemediğinden "kötü görüntü" hatası alırsınız.

  * SAP bağlantınız "Lütfen hesap bilgilerinizi ve/veya izinlerinizi denetleyin ve yeniden deneyin" hata iletisiyle başarısız olursa, derleme dosyaları yanlış konumda olabilir. Derleme dosyalarını veri ağ geçidi yükleme klasörüne kopyaladığınızdan emin olun.

    Sorun gidermenize yardımcı olması için, derleme dosyalarının doğru konumda olup olmadığını kontrol etmenizi sağlayan [.NET derleme bağlama günlüğü görüntüleyicisini kullanın](https://docs.microsoft.com/dotnet/framework/tools/fuslogvw-exe-assembly-binding-log-viewer). İsteğe bağlı olarak, SAP istemci kitaplığını yüklerken **genel derleme önbelleği kaydı** seçeneğini belirleyebilirsiniz.

<a name="sap-library-versions"></a>

#### <a name="sap-client-library-versions"></a>SAP istemci Kitaplığı sürümleri

Aynı anda birden fazla IDoc iletisi gönderildiğinde eski SAP NCo sürümleri kilitlenebilir. Bu koşul, SAP hedefine gönderilen daha sonraki tüm iletileri engeller ve bu da iletilerin zaman aşımına neden olur.

SAP istemci kitaplığı, .NET Framework, .NET çalışma zamanı ve ağ geçidi arasındaki ilişkiler aşağıda verilmiştir:

* Hem Microsoft SAP bağdaştırıcısı hem de ağ geçidi ana bilgisayar hizmeti 4,5 .NET Framework kullanır.

* .NET Framework 4,0 için SAP NCo, 4.7.1 için .NET Runtime 4,0 kullanan işlemlerle çalışmaktadır.

* .NET Framework 2,0 için SAP NCo, .NET Runtime 3,5 2,0 ' i kullanan işlemlerle ve en son ağ geçidiyle artık çalışmakla birlikte çalışmaktadır.

### <a name="secure-network-communications-prerequisites"></a>Güvenli ağ Iletişimi önkoşulları

Şirket içi veri ağ geçidini, yalnızca çok kiracılı Azure 'da desteklenen isteğe bağlı güvenli ağ Iletişimleri (SNC) ile birlikte kullanıyorsanız, bu ayarları da yapılandırmanız gerekir:

* SNC 'yi çoklu oturum açma (SSO) ile birlikte kullanıyorsanız, Data Gateway 'in SAP kullanıcısına karşı eşlenmiş bir kullanıcı olarak çalıştığından emin olun. Varsayılan hesabı değiştirmek için **hesabı Değiştir**' i seçin ve Kullanıcı kimlik bilgilerini girin.

  ![Veri ağ geçidi hesabını değiştir](./media/logic-apps-using-sap-connector/gateway-account.png)

* SNC 'yi bir dış güvenlik ürünüyle etkinleştirirseniz, SNC kitaplığını veya dosyalarını, veri ağ geçidinin yüklü olduğu aynı bilgisayara kopyalayın. Bazı SNC ürünlerine örnek olarak [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos ve NTLM verilebilir.

Veri ağ geçidi için SNC 'yi etkinleştirme hakkında daha fazla bilgi için bkz. [güvenli ağ Iletişimini etkinleştirme](#secure-network-communications).

<a name="migrate"></a>

## <a name="migrate-to-current-connector"></a>Geçerli bağlayıcıya geçiş

Önceki bir yönetilen (ıSE) SAP bağlayıcısından geçerli yönetilen SAP bağlayıcısına geçiş yapmak için şu adımları izleyin:

1. Şimdiye kadar yapmadıysanız, Şirket [içi veri ağ geçidinizi](https://www.microsoft.com/download/details.aspx?id=53127) en son sürüme sahip olacak şekilde güncelleştirin. Daha fazla bilgi için bkz. [Azure Logic Apps için şirket içi veri ağ geçidi yüklemesi](../logic-apps/logic-apps-gateway-install.md).

1. Eski SAP bağlayıcısını kullanan mantıksal uygulamada, **SAP gönder** eylemini silin.

1. En son SAP bağlayıcısından **SAP 'a Ileti gönder** eylemini ekleyin. Bu eylemi kullanabilmeniz için, SAP sisteminize olan bağlantıyı yeniden oluşturun.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

<a name="add-trigger"></a>

## <a name="send-message-to-sap"></a>SAP 'ye ileti gönder

Bu örnek, bir HTTP isteğiyle tetikleyebileceğiniz bir mantıksal uygulama kullanır. Mantıksal uygulama bir SAP sunucusuna bir IDoc gönderir ve mantıksal uygulamayı çağıran istek sahibine bir yanıt döndürür.

### <a name="add-an-http-request-trigger"></a>HTTP Istek tetikleyicisi ekleme

Azure Logic Apps, her mantıksal uygulama, belirli bir olay gerçekleştiğinde veya belirli bir koşul karşılandığında tetiklenen bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her tetiklendiğinde Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

> [!NOTE]
> Bir mantıksal uygulama SAP 'den IDoc paketleri aldığında, [istek TETIKLEYICISI](https://docs.microsoft.com/azure/connectors/connectors-native-reqres) SAP 'Nin WE60 IDoc belgeleri tarafından oluşturulan "düz" XML şemasını desteklemez. Ancak, mantıksal uygulamalardan SAP *'ye* ileti gönderen senaryolar için "düz" XML şeması desteklenir. İstek tetikleyicisini SAP 'nin IDoc XML 'i ile birlikte kullanabilirsiniz, ancak bu, RFC üzerinde IDoc ile kullanamazsınız. Veya XML 'yi gerekli biçime dönüştürebilirsiniz. 

Bu örnekte, mantıksal uygulamanıza *http post istekleri* gönderebilmeniz için Azure 'da bir uç nokta ile bir mantıksal uygulama oluşturursunuz. Mantıksal uygulamanız bu HTTP isteklerini aldığında, tetikleyici ateşlenir ve iş akışınızda bir sonraki adımı çalıştırır.

1. [Azure Portal](https://portal.azure.com), mantıksal uygulama Tasarımcısı ' nı açan boş bir mantıksal uygulama oluşturun.

1. Arama kutusuna `http request` filtreniz olarak yazın. **Tetikleyiciler** LISTESINDEN **bir http isteği alındığında**öğesini seçin.

   ![HTTP Istek tetikleyicisi Ekle](./media/logic-apps-using-sap-connector/add-http-trigger-logic-app.png)

1. Mantıksal uygulamanız için bir uç nokta URL 'SI oluşturabilmeniz için artık mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Uç nokta URL 'SI Şu anda tetikleyicide görüntülenir, örneğin:

   ![Uç nokta için URL Oluştur](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-an-sap-action"></a>SAP eylemi ekleme

Azure Logic Apps bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) , iş akışınızda bir tetikleyiciyi veya başka bir eylemi izleyen bir adımdır. Mantıksal uygulamanıza henüz bir tetikleyici eklemediyseniz ve bu örneği izlemek istiyorsanız, [Bu bölümde açıklanan tetikleyiciyi ekleyin](#add-trigger).

1. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici altında **yeni adım**' ı seçin.

   ![Mantıksal uygulamaya yeni adım Ekle](./media/logic-apps-using-sap-connector/add-sap-action-logic-app.png)

1. Arama kutusuna `sap` filtreniz olarak yazın. **Eylemler** LISTESINDEN, **SAP 'ye ileti gönder**' i seçin.
  
   !["İletiyi SAP 'a gönder" eylemini seçin](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Ya da **Kurumsal** SEKMESINI seçip SAP eylemini seçebilirsiniz.

   ![Kurumsal sekmesinden "SAP 'a ileti gönder" eylemini seçin](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. Bununla birlikte, bağlantı ayrıntıları istenirse, şirket içi SAP sunucunuza bir bağlantı oluşturabilmek için bilgileri sağlayın.

   1. Bağlantı için bir ad girin.

   1. Veri ağ geçidini kullanıyorsanız, şu adımları izleyin:
   
      1. **Veri ağ geçidi** bölümünde, **abonelik**altında, önce veri ağ geçidi yüklemeniz için Azure Portal oluşturduğunuz veri ağ geçidi kaynağı için Azure aboneliğini seçin.
   
      1. **Bağlantı ağ geçidi**altında Azure 'da veri ağ geçidi kaynağınızı seçin.

   1. Bağlantıyla ilgili bilgi sağlamaya devam edin. **Oturum açma türü** özelliği için, özelliğin **uygulama sunucusu** veya **Grup**olarak ayarlanmış olup olmadığına bağlı olarak adımı izleyin:
   
      * **Uygulama sunucusu**için genellikle isteğe bağlı olarak görüntülenen bu özellikler gereklidir:

        ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * **Grup**için, genellikle isteğe bağlı olarak görüntülenen bu özellikler gereklidir:

        ![SAP ileti sunucusu bağlantısı oluştur](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Varsayılan olarak, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için tanımlayıcı yazma kullanılır. Bu davranış, daha önce sorunları tespit etmenize yardımcı olabilir. **Güvenli yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. [Güvenli yazma seçeneği](#safe-typing)hakkında daha fazla bilgi edinin.

   1. İşiniz bittiğinde **Oluştur**' u seçin.

      Logic Apps, bağlantının düzgün çalıştığından emin olmak için bağlantınızı kurar ve test eder.

1. Şimdi SAP sunucunuzdaki bir eylemi bulun ve seçin.

   1. **SAP eylemi** kutusunda klasör simgesini seçin. Dosya listesinden, kullanmak istediğiniz SAP iletisini bulun ve seçin. Listede gezinmek için okları kullanın.

      Bu örnek, **Orders** türünde bir IDoc seçer.

      ![IDoc eylemini bul ve Seç](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      İstediğiniz eylemi bulamazsanız, el ile bir yol girebilirsiniz, örneğin:

      ![IDoc eyleminin yolunu el ile sağlayın](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > , İfade Düzenleyicisi aracılığıyla **SAP eylemi** değerini sağlayın. Bu şekilde, farklı ileti türleri için aynı eylemi kullanabilirsiniz.

      IDOC işlemleri hakkında daha fazla bilgi için bkz. [IDOC işlemleri Için ileti şemaları](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   1. Dinamik içerik listesinin görünmesi için **giriş iletisi** kutusunun içine tıklayın. Bu listeden, **BIR http isteği alındığında**altında **gövde** alanını seçin.

      Bu adım, HTTP Istek tetikleyicinizden gelen gövde içeriğini içerir ve bu çıktıyı SAP sunucunuza gönderir.

      ![Tetikleyiciden "gövde" özelliğini seçin](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      İşiniz bittiğinde, SAP eyleminiz Şu örneğe benzer şekilde görünür:

      ![SAP eylemini tamamlamayı](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

<a name="add-response"></a>

### <a name="add-an-http-response-action"></a>HTTP yanıt eylemi ekleme

Şimdi mantıksal uygulamanızın iş akışına bir yanıt eylemi ekleyin ve SAP eyleminden çıktıyı dahil edin. Bu şekilde, mantıksal uygulamanız SAP sunucunuzdaki sonuçları özgün istek sahibine döndürür.

1. Mantıksal uygulama Tasarımcısı ' nda, SAP eylemi altında **yeni adım**' ı seçin.

1. Arama kutusuna `response` filtreniz olarak yazın. **Eylemler** listesinden **Yanıt**' ı seçin.

1. Dinamik içerik listesinin görünmesi için **gövde** kutusunun içine tıklayın. Bu listeden, **SAP 'ye Ileti gönder**altında **gövde** alanını seçin.

   ![Tamamlanmış SAP eylemi](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Mantıksal uygulamanızı kaydedin.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Mantıksal uygulamanız zaten etkin değilse, mantıksal uygulama menüsünde **genel bakış**' ı seçin. Araç çubuğunda **Etkinleştir**' i seçin.

1. Tasarımcı araç çubuğunda **Çalıştır**' ı seçin. Bu adım mantıksal uygulamanızı el ile başlatır.

1. HTTP Istek tetikleyicinizdeki URL 'ye bir HTTP POST isteği göndererek mantıksal uygulamanızı tetikleyin.
İsteğiniz ile ileti içeriğinizi ekleyin. İsteği göndermek için [Postman](https://www.getpostman.com/apps)gibi bir araç kullanabilirsiniz.

   Bu makalede, istek XML biçiminde olması gereken bir IDoc dosyası gönderir ve kullanmakta olduğunuz SAP eyleminin ad alanını içermelidir, örneğin:

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. HTTP isteğinizi gönderdikten sonra, mantıksal uygulamanızdan gelen yanıtı bekleyin.

   > [!NOTE]
   > Yanıt için gereken tüm adımlar [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md)içinde tamamlanmazsa mantıksal uygulamanız zaman aşımına uğrayabilir. Bu durum oluşursa, istekler engellenebilir. Sorunları tanılamanıza yardımcı olması için [Logic Apps 'i nasıl denetleyebilir ve izleyebileceğinizi](../logic-apps/monitor-logic-apps.md)öğrenin.

Artık, SAP sunucunuz ile iletişim kurabilen bir mantıksal uygulama oluşturdunuz. Mantıksal uygulamanız için bir SAP bağlantısı ayarladığınıza göre, BAPı ve RFC gibi diğer kullanılabilir SAP eylemlerini inceleyebilirsiniz.

<a name="receive-from-sap"></a>

## <a name="receive-message-from-sap"></a>SAP 'den ileti al

Bu örnek, uygulama SAP sisteminden bir ileti aldığında tetiklenen bir mantıksal uygulama kullanır.

### <a name="add-an-sap-trigger"></a>SAP tetikleyicisi ekleme

1. Azure portal, mantıksal uygulama Tasarımcısı ' nı açan boş bir mantıksal uygulama oluşturun.

1. Arama kutusuna `sap` filtreniz olarak yazın. **Tetikleyiciler** listesinden **SAP 'den bir ileti alındığında**öğesini seçin.

   ![SAP tetikleyicisi Ekle](./media/logic-apps-using-sap-connector/add-sap-trigger-logic-app.png)

   Ya da **Kurumsal** sekmesini seçip tetikleyiciyi seçebilirsiniz:

   ![Kurumsal sekmeden SAP tetikleyicisi ekleme](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. Bununla birlikte, bağlantı ayrıntıları istenirse, şu anda şirket içi SAP sunucunuza bir bağlantı oluşturabilmek için bilgileri sağlayın.

   1. Bağlantı için bir ad girin.

   1. Veri ağ geçidini kullanıyorsanız, şu adımları izleyin:

      1. **Veri ağ geçidi** bölümünde, **abonelik**altında, önce veri ağ geçidi yüklemeniz için Azure Portal oluşturduğunuz veri ağ geçidi kaynağı için Azure aboneliğini seçin.

      1. **Bağlantı ağ geçidi**altında Azure 'da veri ağ geçidi kaynağınızı seçin.

   1. Bağlantıyla ilgili bilgi sağlamaya devam edin. **Oturum açma türü** özelliği için, özelliğin **uygulama sunucusu** veya **Grup**olarak ayarlanmış olup olmadığına bağlı olarak adımı izleyin:

      * **Uygulama sunucusu**için genellikle isteğe bağlı olarak görüntülenen bu özellikler gereklidir:

        ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * **Grup**için, genellikle isteğe bağlı olarak görüntülenen bu özellikler gereklidir:

        ![SAP ileti sunucusu bağlantısı oluştur](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Varsayılan olarak, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için tanımlayıcı yazma kullanılır. Bu davranış, daha önce sorunları tespit etmenize yardımcı olabilir. **Güvenli yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. [Güvenli yazma seçeneği](#safe-typing)hakkında daha fazla bilgi edinin.

   1. İşiniz bittiğinde **Oluştur**' u seçin.

      Logic Apps, bağlantının düzgün çalıştığından emin olmak için bağlantınızı kurar ve test eder.

1. SAP sistem yapılandırmanıza göre [gerekli parametreleri](#parameters) sağlayın.

   İsteğe bağlı olarak bir veya daha fazla SAP eylemi sağlayabilirsiniz. Bu eylem listesi, tetikleyicinin SAP sunucusundan aldığı iletileri belirtir. Boş liste, tetikleyicinin tüm iletileri alacağını belirtir. Listede birden fazla ileti varsa, tetikleyici yalnızca listede belirtilen iletileri alır. SAP sunucusundan gönderilen diğer iletiler reddedilir.

   Dosya seçicisinden bir SAP eylemi seçebilirsiniz:

   ![Logic App 'e SAP eylemi ekleme](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Ya da bir eylemi el ile belirtebilirsiniz:

   ![SAP eylemini el ile girin](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png)

   Bu, tetikleyiciyi birden fazla ileti alacak şekilde ayarlarken eylemin nasıl göründüğünü gösteren bir örnektir.

   ![Birden çok ileti alan tetikleyici örneği](media/logic-apps-using-sap-connector/example-trigger.png)

   SAP eylemi hakkında daha fazla bilgi için bkz. [IDoc Için ileti şemaları işlemler](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

1. SAP sisteminizden ileti almaya başlayabilmeniz için artık mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

Mantıksal uygulamanız artık SAP sisteminizden ileti almaya hazırdır.

> [!NOTE]
> SAP tetikleyicisi bir yoklama tetikleyicisi değildir, ancak bunun yerine Web kancası tabanlı bir tetikleyiciden yapılır. Veri ağ geçidini kullanıyorsanız, tetikleyici veri ağ geçidinden yalnızca bir ileti varken çağrılır, dolayısıyla hiçbir yoklama gerekmez.

<a name="parameters"></a>

#### <a name="parameters"></a>Parametreler

Basit dize ve sayı girişlerinin yanı sıra, SAP Bağlayıcısı aşağıdaki tablo parametrelerini ( `Type=ITAB` girişler) kabul eder:

* Eski SAP sürümleri için hem giriş hem de çıkış tablo yönü parametreleri.
* Daha yeni SAP sürümleri için tablo yönü parametrelerini değiştiren parametreleri değiştirme.
* Hiyerarşik tablo parametreleri

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Mantıksal uygulamanızı tetiklemek için SAP sisteminizden bir ileti gönderin.

1. Mantıksal uygulama menüsünde **genel bakış**' ı seçin. Mantıksal uygulamanız için tüm yeni çalıştırmalar için **çalıştırma geçmişini** gözden geçirin.

1. Tetikleyici çıkışları bölümünde SAP sisteminizden gönderilen iletiyi gösteren en son çalıştırmayı açın.

## <a name="receive-idoc-packets-from-sap"></a>SAP 'den IDoc paketleri alma

SAP 'yi, toplu işler veya IDoc grupları olan [paketlerdeki IDoc 'ları gönderecek](https://help.sap.com/viewer/8f3819b0c24149b5959ab31070b64058/7.4.16/en-US/4ab38886549a6d8ce10000000a42189c.html)şekilde ayarlayabilirsiniz. IDoc paketlerini, SAP bağlayıcısını ve özellikle tetikleyiciyi almak için ek yapılandırma gerekmez. Ancak, bir IDoc paketindeki her öğeyi tetikleme paketini aldıktan sonra işlemek için, paketi tek tek IDoc 'a bölmek için bazı ek adımlar gerekir.

İşte, [ `xpath()` işlevi](./workflow-definition-language-functions-reference.md#xpath)kullanarak bir paketten tek tek IDoc 'ları nasıl ayıklayabileceğiniz gösterilmektedir:

1. Başlamadan önce SAP tetikleyicisine sahip bir mantıksal uygulama gerekir. Bu mantıksal uygulama henüz yoksa, [SAP tetikleyicisiyle bir mantıksal uygulama ayarlamak](#receive-from-sap)için bu konudaki önceki adımları izleyin.

   Örneğin:

   ![Logic App 'e SAP tetikleyicisi ekleme](./media/logic-apps-using-sap-connector/first-step-trigger.png)

1. Mantıksal uygulamanızın SAP 'den aldığı XML IDoc 'ından kök ad alanını alın. Bu ad alanını XML belgesinden ayıklamak için, bir yerel dize değişkeni oluşturan ve bu ad alanını bir ifade kullanarak depolayan bir adım ekleyin `xpath()` :

   `xpath(xml(triggerBody()?['Content']), 'namespace-uri(/*)')`

   ![IDoc 'dan kök ad alanı al](./media/logic-apps-using-sap-connector/get-namespace.png)

1. Tek bir IDoc 'u ayıklamak için, bir dizi değişkeni oluşturan ve IDoc koleksiyonunu başka bir ifade kullanarak depolayan bir adım ekleyin `xpath()` :

   `xpath(xml(triggerBody()?['Content']), '/*[local-name()="Receive"]/*[local-name()="idocData"]')`

   ![Öğelerin dizisini al](./media/logic-apps-using-sap-connector/get-array.png)

   Dizi değişkeni, mantıksal uygulamanız için her bir IDoc 'un koleksiyon üzerinde sıralama yaparak tek tek işlemesini sağlar. Bu örnekte, Logic App her IDoc 'u bir döngü kullanarak bir SFTP sunucusuna aktarır:

   ![SFTP sunucusuna IDoc gönder](./media/logic-apps-using-sap-connector/loop-batch.png)

   Her IDoc 'un, `<Receive></Receive` Bu durumda IDoc 'un aşağı akış uygulamasına veya SFTP sunucusuna göndermeden önce kök ad alanı ile birlikte, dosya içeriğinin bir öğe içinde sarmalanması neden olan kök ad alanını içermesi gerekir.

Yeni bir mantıksal uygulama oluştururken mantıksal uygulama Tasarımcısı ' nda bu şablonu seçerek bu model için hızlı başlangıç şablonunu kullanabilirsiniz.

![Batch mantıksal uygulama şablonu seçin](./media/logic-apps-using-sap-connector/select-batch-logic-app-template.png)

## <a name="generate-schemas-for-artifacts-in-sap"></a>SAP 'de yapılar için şemalar oluşturma

Bu örnek, bir HTTP isteğiyle tetikleyebileceğiniz bir mantıksal uygulama kullanır. SAP eylemi, belirtilen IDoc ve BAPı için şemaları oluşturmak üzere bir SAP sistemine bir istek gönderir. Yanıtta döndürülen şemalar, Azure Resource Manager Bağlayıcısı kullanılarak bir tümleştirme hesabına yüklenir.

### <a name="add-an-http-request-trigger"></a>HTTP Istek tetikleyicisi ekleme

1. Azure portal, mantıksal uygulama Tasarımcısı ' nı açan boş bir mantıksal uygulama oluşturun.

1. Arama kutusuna `http request` filtreniz olarak yazın. **Tetikleyiciler** LISTESINDEN **bir http isteği alındığında**öğesini seçin.

   ![HTTP Istek tetikleyicisi Ekle](./media/logic-apps-using-sap-connector/add-http-trigger-logic-app.png)

1. Mantıksal uygulamanız için bir uç nokta URL 'SI oluşturabilmeniz için artık mantıksal uygulamanızı kaydedin.
Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Uç nokta URL 'SI Şu anda tetikleyicide görüntülenir, örneğin:

   ![Uç nokta için URL Oluştur](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-an-sap-action-to-generate-schemas"></a>Şemaları oluşturmak için bir SAP eylemi ekleyin

1. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici altında **yeni adım**' ı seçin.

   ![Mantıksal uygulamaya yeni adım Ekle](./media/logic-apps-using-sap-connector/add-sap-action-logic-app.png)

1. Arama kutusuna `sap` filtreniz olarak yazın. **Eylemler** listesinden **şemalar oluştur**' u seçin.
  
   ![Logic App 'e "şema üret" eylemi ekleyin](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Ya da **Kurumsal** SEKMESINI seçip SAP eylemini seçebilirsiniz.

   ![Kurumsal sekmesinden SAP gönder eylemini seçin](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. Bununla birlikte, bağlantı ayrıntıları istenirse, şu anda şirket içi SAP sunucunuza bir bağlantı oluşturabilmek için bilgileri sağlayın.

   1. Bağlantı için bir ad girin.

   1. **Veri ağ geçidi** bölümünde, **abonelik**altında, önce veri ağ geçidi yüklemeniz için Azure Portal oluşturduğunuz veri ağ geçidi kaynağı için Azure aboneliğini seçin. 
   
   1. **Bağlantı ağ geçidi**altında Azure 'da veri ağ geçidi kaynağınızı seçin.

   1. Bağlantıyla ilgili bilgi sağlamaya devam edin. **Oturum açma türü** özelliği için, özelliğin **uygulama sunucusu** veya **Grup**olarak ayarlanmış olup olmadığına bağlı olarak adımı izleyin:
   
      * **Uygulama sunucusu**için genellikle isteğe bağlı olarak görüntülenen bu özellikler gereklidir:

        ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * **Grup**için, genellikle isteğe bağlı olarak görüntülenen bu özellikler gereklidir:

        ![SAP ileti sunucusu bağlantısı oluştur](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Varsayılan olarak, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için tanımlayıcı yazma kullanılır. Bu davranış, daha önce sorunları tespit etmenize yardımcı olabilir. **Güvenli yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. [Güvenli yazma seçeneği](#safe-typing)hakkında daha fazla bilgi edinin.

   1. İşiniz bittiğinde **Oluştur**' u seçin.

      Logic Apps, bağlantının düzgün çalıştığından emin olmak için bağlantınızı kurar ve test eder.

1. Şemayı oluşturmak istediğiniz yapıtın yolunu belirtin.

   Dosya seçicisinden SAP eylemini seçebilirsiniz:

   ![SAP eylemini seçin](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Ya da eylemi el ile girebilirsiniz:

   ![SAP eylemini el ile girin](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Birden fazla yapıt için şemalar oluşturmak üzere her yapıt için SAP eylem ayrıntılarını sağlayın, örneğin:

   ![Yeni öğe Ekle ' yi seçin](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![İki öğeyi göster](media/logic-apps-using-sap-connector/schema-generator-example.png)

   SAP eylemi hakkında daha fazla bilgi için bkz. [IDOC işlemleri Için ileti şemaları](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Tasarımcı araç çubuğunda **Çalıştır** ' ı seçerek mantıksal uygulamanız için bir çalıştırma tetikleyin.

1. Çalıştır ' ı açın ve **şemalar oluştur** eyleminin çıkışlarını kontrol edin.

   Çıkışlar, belirtilen ileti listesi için üretilen şemaları gösterir.

### <a name="upload-schemas-to-an-integration-account"></a>Tümleştirme hesabına şemaları yükleme

İsteğe bağlı olarak, oluşturulan şemaları blob, depolama veya tümleştirme hesabı gibi depolarda indirebilir veya saklayabilirsiniz. Tümleştirme hesapları, diğer XML eylemleriyle birinci sınıf bir deneyim sağlar, bu nedenle bu örnek, Azure Resource Manager Bağlayıcısı kullanılarak aynı mantıksal uygulama için nasıl bir tümleştirme hesabına şema yükleneceğini gösterir.

1. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici altında **yeni adım**' ı seçin.

1. Arama kutusuna `Resource Manager` filtreniz olarak yazın. **Kaynak oluştur veya Güncelleştir**' i seçin.

   ![Azure Resource Manager eylemi seçin](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Azure aboneliğiniz, Azure Kaynak grubunuz ve tümleştirme hesabınız dahil olmak üzere eyleme ilişkin ayrıntıları girin. Alanlara SAP belirteçleri eklemek için, bu alanlar için kutulara tıklayın ve görüntülenen dinamik içerik listesinden seçin.

   1. **Yeni parametre Ekle** listesini açın ve **konum** ve **Özellikler** alanlarını seçin.

   1. Bu yeni alanların ayrıntılarını bu örnekte gösterildiği gibi sağlayın.

      ![Azure Resource Manager eylemi için ayrıntıları girin](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   SAP **oluşturma şemaları** eylemi, şemaları bir koleksiyon olarak oluşturur, böylece tasarımcı eyleme her bir döngü **için** otomatik olarak bir ekler. Bu eylemin nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

   !["For each" döngüsüyle Azure Resource Manager eylemi](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)

   > [!NOTE]
   > Şemalar Base64 kodlamalı biçim kullanır. Şemaları bir tümleştirme hesabına yüklemek için, işlevi kullanılarak kodu çözülmüş olmaları gerekir `base64ToString()` . Öğe için kodu gösteren bir örnek aşağıda verilmiştir `"properties"` :
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Tasarımcı araç çubuğunda, mantıksal uygulamanızı el ile tetiklemek için **Çalıştır** ' ı seçin.

1. Başarılı bir çalıştıktan sonra, tümleştirme hesabına gidin ve oluşturulan şemaların mevcut olup olmadığını denetleyin.

<a name="secure-network-communications"></a>

## <a name="enable-secure-network-communications"></a>Güvenli ağ Iletişimini etkinleştir

Başlamadan önce, yalnızca veri ağ geçidini kullandığınızda ve mantıksal uygulamalarınızın çok kiracılı Azure 'da çalıştığı durumlarda uygulanan önceden listelenmiş [önkoşulları](#pre-reqs)karşıladığınızdan emin olun:

* Şirket içi veri ağ geçidinin, SAP sisteminizle aynı ağda olan bir bilgisayarda yüklü olduğundan emin olun.

* Çoklu oturum açma (SSO) için, veri ağ geçidi, SAP kullanıcısına eşlenmiş bir kullanıcı olarak çalışır.

* Ek güvenlik işlevlerini sağlayan SNC kitaplığı, veri ağ geçidiyle aynı makineye yüklenir. Bazı örneklerde [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos ve NTLM vardır.

   SAP sistemine yönelik istekleriniz için SNC 'yi etkinleştirmek üzere SAP bağlantısı 'ndaki **SNC kullan** onay kutusunu seçin ve şu özellikleri sağlayın:

   ![Bağlantıda SAP SNC 'yi yapılandırma](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Özellik | Açıklama |
   |----------| ------------|
   | **SNC kitaplık yolu** | SNC Kitaplık adı veya yolu NCo yükleme konumuna veya mutlak yola göre. Örnekler `sapsnc.dll` veya `.\security\sapsnc.dll` veya `c:\security\sapsnc.dll` . |
   | **SNC SSO 'SU** | SNC üzerinden bağlandığınızda, genellikle çağıranın kimliğini doğrulamak için SNC kimliği kullanılır. Kullanıcı ve parola bilgilerinin çağıranın kimliğini doğrulamak için kullanılabilmesi, ancak hattın hala şifrelenmemesi için başka bir seçenek de geçersiz kılınmalıdır. |
   | **SNC My adı** | Çoğu durumda, bu özellik atlanabilir. Yüklü SNC çözümü genellikle kendi SNC adını bilir. Yalnızca birden çok kimliği destekleyen çözümler için, bu belirli hedef veya sunucu için kullanılacak kimliği belirtmeniz gerekebilir. |
   | **SNC Iş ortağı adı** | Arka uç SNC için ad. |
   | **SNC koruma kalitesi** | Bu belirli hedef veya sunucu için SNC iletişimi için kullanılacak hizmet kalitesi. Varsayılan değer, arka uç sistemi tarafından tanımlanır. En büyük değer, SNC için kullanılan güvenlik ürünü tarafından tanımlanır. |
   |||

   > [!NOTE]
   > Ortam değişkenlerini, veri ağ geçidi ve SNC kitaplığı olan makinede SNC_LIB ve SNC_LIB_64 ayarlama. Ayarlanırsa, bağlayıcı aracılığıyla geçirilen SNC kitaplık değerine göre önceliklidir.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Güvenli yazma

Varsayılan olarak, SAP bağlantınızı oluştururken şemaya göre XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için güçlü yazma kullanılır. Bu davranış, daha önce sorunları tespit etmenize yardımcı olabilir. **Güvenli yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. **Güvenli yazma**' yı SEÇERSENIZ, SAP içindeki Bats türü ve Tims türü, XML eşdeğerleri ve, yerine dize olarak değerlendirilir `xs:date` `xs:time` `xmlns:xs="http://www.w3.org/2001/XMLSchema"` . Güvenli yazma, tüm şema oluşturma davranışlarını, "gönderilen" yükün ve "alınmış" yanıtın ve tetikleyicisinin her ikisi için de ileti gönder ' i etkiler. 

Tanımlayıcı yazma kullanıldığında (**Güvenli yazma** etkin değil), şema, ve Tims türlerini daha basit XML türlerine eşler:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Güçlü yazma kullanarak iletiler gönderdiğinizde, izin ve TIMS yanıtı eşleşen XML türü biçimine uyar:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

**Güvenli yazma** etkin olduğunda, şema yalnızca uzunluk KıSıTLAMALARıNA sahip XML dize alanlarıyla birlikte, ve gibi, şema ve Tims türlerini eşler.

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

İletiler **Güvenli yazma** etkinken gönderildiğinde, aşağıdaki örnekteki gibi görünür:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

### <a name="confirm-transaction-explicitly"></a>İşlemi açıkça Onayla

Logic Apps 'dan SAP 'ye işlem gönderdiğinizde, bu Exchange SAP belgesinde, [Işlem RFC sunucu programlarında](https://help.sap.com/doc/saphelp_nwpi71/7.1/en-US/22/042ad7488911d189490000e829fbbd/content.htm?no_cache=true)açıklandığı gibi iki adımda gerçekleşir. Varsayılan olarak, **SAP gönder** eylemi, işlev aktarımına yönelik adımları ve tek bir çağrıda işlem onayını işler. SAP Bağlayıcısı, bu adımları ayırma seçeneği sunar. Bir IDoc gönderebilir ve işlemi otomatik olarak onaylamanız yerine, açıkça **Işlem kimliği Onayla** eylemini kullanabilirsiniz.

İşlem KIMLIĞI onayını ayırma özelliği, örneğin ağ sorunları gibi nedenlerle hataların gerçekleşebileceği senaryolarda, SAP 'deki işlemleri yinelemek istemediğiniz durumlarda faydalıdır. İşlem KIMLIĞINI ayrı olarak onayladıktan sonra, işlem SAP sisteminizde yalnızca bir kez tamamlanır.

Bu düzenin gösterildiği bir örnek aşağıda verilmiştir:

1. Boş bir mantıksal uygulama oluşturun ve bir HTTP tetikleyicisi ekleyin.

1. SAP bağlayıcısından, **IDoc Send** eylemini ekleyin. SAP sisteminize göndereceğiniz IDoc 'un ayrıntılarını sağlayın.

1. İşlem KIMLIĞINI ayrı bir adımda açıkça onaylamak için, **TID 'Yi Onayla** alanında **Hayır**' ı seçin. İsteğe bağlı **Işlem KIMLIĞI GUID** alanı için, değeri el ile belirtebilir ya da bağlayıcının bu GUID 'yi otomatik olarak oluşturmasını ve bu GUID 'Yi IDoc gönderme eyleminden yanıt olarak döndürmesini sağlayabilirsiniz.

   ![IDOC eylem özelliklerini gönder](./media/logic-apps-using-sap-connector/send-idoc-action-details.png)

1. İşlem KIMLIĞINI açıkça onaylamak için, **Işlem kimliğini onayla** eylemini ekleyin. Dinamik içerik listesinin görünmesi için **Işlem kimliği** kutusunun içine tıklayın. Bu listeden, **IDoc gönder** eyleminden döndürülen **işlem kimliği** değerini seçin.

   ![İşlem KIMLIĞI eylemini Onayla](./media/logic-apps-using-sap-connector/explicit-transaction-id.png)

   Bu adım çalıştıktan sonra, SAP Bağlayıcısı tarafında ve SAP sistem tarafında, geçerli işlem her iki uçta da tamamlanmış olarak işaretlenir.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Yönetilen (ıSE) SAP bağlayıcısının Şu anda bilinen sorunları ve sınırlamaları aşağıda verilmiştir:

* SAP tetikleyicisi, veri ağ geçidi kümelerini desteklemez. Bazı yük devretme durumlarında, SAP sistemiyle iletişim kuran veri ağ geçidi düğümü etkin düğümden farklı olabileceğinden beklenmeyen davranışlara neden olur. Gönderme senaryolarında, veri ağ geçidi kümeleri desteklenir.

* SAP Bağlayıcısı Şu anda SAP yönlendirici dizelerini desteklemiyor. Şirket içi veri ağ geçidi, bağlanmak istediğiniz SAP sistemiyle aynı LAN üzerinde bulunmalıdır.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında, bağlayıcının Swagger dosyasında açıklanan Tetikleyiciler, Eylemler ve sınırlar gibi daha teknik ayrıntılar için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sap/)bakın.

> [!NOTE]
> Bir [tümleştirme hizmeti ortamındaki (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)Logic Apps için, bu bağlayıcının Ise etiketli sürümü bunun yerine [Ise ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Logic Apps 'den [Şirket içi sistemlere bağlanın](../logic-apps/logic-apps-gateway-connection.md) .
* [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)ile diğer ileti işlemlerini doğrulamayı, dönüştürmeyi ve kullanmayı öğrenin.
* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md)hakkında bilgi edinin.
