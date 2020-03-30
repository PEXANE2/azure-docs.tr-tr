---
title: Azure Blob Depolama'ya bağlanın
description: Azure Mantıksal Uygulamaları'nı kullanarak Azure depolama hesaplarında blob oluşturma ve yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/21/2020
tags: connectors
ms.openlocfilehash: eb943bfe36be10d1e95d569a5c1bf48563e909c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247364"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-by-using-azure-logic-apps"></a>Azure Mantıksal Uygulamaları kullanarak Azure Blob Depolama'da blob oluşturma ve yönetme

Bu makalede, Azure Blob Depolama konektörüne sahip bir mantık uygulaması nın içinden Azure depolama hesabınızda blob olarak depolanan dosyalara nasıl erişebileceğinizi ve bunları nasıl yönetebileceğiniz gösterilmektedir. Bu şekilde, dosyalarınızı yönetmek için görevleri ve iş akışlarını otomatikleştiren mantık uygulamaları oluşturabilirsiniz. Örneğin, depolama hesabınızda dosyalar oluşturan, dosyaları alan, güncelleştiren ve silen mantıksal uygulamalar oluşturabilirsiniz.

Azure web sitesinde güncelleştirilen bir aracınız olduğunu varsayalım. hangi mantık uygulaması için tetikleyici olarak görür. Bu olay gerçekleştiğinde, mantık uygulamanızın blob depolama kapsayıcınızdaki bazı dosyayı güncelleştirmesini sağlayabilirsiniz, bu da mantık uygulamanızda bir eylemdir.

Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bağlayıcıya özel teknik bilgiler için [Azure Blob Depolama bağlayıcısı başvurusuna](https://docs.microsoft.com/connectors/azureblobconnector/)bakın.

> [!IMPORTANT]
> Mantıksal uygulamalar, her ikisi de aynı bölgedeyse, güvenlik duvarlarının arkasındaki depolama hesaplarına doğrudan erişemez. Geçici çözüm olarak, farklı bölgelerdeki mantıksal uygulamalarınıza ve depolama hesabınıza sahip olabilirsiniz. Azure Logic Apps'tan güvenlik duvarlarının arkasındaki depolama hesaplarına erişimi etkinleştirme hakkında daha fazla bilgi için, bu konunun ilerleyen bölümlerinde [güvenlik duvarlarının arkasındaki Access depolama hesaplarına](#storage-firewalls) bakın.

<a name="blob-storage-limits"></a>

## <a name="limits"></a>Sınırlar

* Varsayılan olarak, Azure Blob Depolama eylemleri *50 MB veya daha küçük*dosyaları okuyabilir veya yazabilir. Azure Blob Depolama eylemleri, 50 MB'dan büyük ancak 1024 MB'a kadar olan dosyaları işlemek için [ileti ödenemasını](../logic-apps/logic-apps-handle-large-messages.md)destekler. **Get blob içerik** eylem örtülü olarak ötme kullanır.

* Azure Blob Depolama tetikleyicileri ötmeyi desteklemez. Dosya içeriği isteğinde bulunurken, yalnızca 50 MB veya daha küçük dosyaları seçin. 50 MB'dan büyük dosyaları almak için aşağıdaki deseni izleyin:

  * **Bir blob eklendiğinde veya değiştirildiğinde (yalnızca özellikler)** gibi dosya özelliklerini döndüren bir Azure Blob Depolama tetikleyicisi kullanın.

  * Azure Blob Depolama Alanı ile tetiği izleyin Dosyanın tamamını okuyan ve dolaylı olarak yığınlama kullanan **blob içerik** eylemini alın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Azure [depolama hesabı ve depolama kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)

* Azure blob depolama hesabınıza erişmeniz gereken mantık uygulaması. Mantıksal uygulamanızı Azure Blob Depolama tetikleyicisi ile başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)ihtiyacınız var.

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Blob depolama tetikleyicisi ekleme

Azure Logic Apps'ta, her mantık uygulaması, belirli bir olay olduğunda veya belirli bir koşul yerine getirildiğinde ateş eden bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her ateşleninher yerinde, Logic Apps motoru bir mantık uygulaması örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

Bu örnek, bir blob'un özellikleri depolama kapsayıcınıza eklendiğinde veya güncelleştirildiğinde **ne zaman blob eklendiğinde veya değiştirildiğinde (yalnızca özellikler)** tetikleyicisiyle bir mantık uygulaması iş akışını nasıl başlatabileceğinizi gösterir.

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da, Logic App Designer'ı açan boş bir mantık uygulaması oluşturun. Bu örnekte Azure portalı kullanır.

2. Arama kutusuna filtreniz olarak "azure blob" girin. Tetikleyiciler listesinden istediğiniz tetikleyiciyi seçin.

   Bu örnekte şu tetikleyici kullanır: **Bir blob eklendiğinde veya değiştirildiğinde (yalnızca özellikler)**

   ![Azure Blob Depolama tetikleyicisi seçin](./media/connectors-create-api-azureblobstorage/add-azure-blob-storage-trigger.png)

3. Bağlantı ayrıntıları için istenirseniz, [blob depolama bağlantınızı şimdi oluşturun.](#create-connection) Veya bağlantınız zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnekte, izlemek istediğiniz kapsayıcıyı ve klasörü seçin.

   1. **Kapsayıcı** kutusunda klasör simgesini seçin.

   2. Klasör listesinde, sağ açılı braketi **>** (), seçin ve ardından istediğiniz klasörü bulup seçene kadar göz atın.

      ![Tetikleyiciyle kullanılacak depolama klasörünü seçin](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Tetikleyicinin değişiklikleri için klasörü denetlemesini ne sıklıkta istediğinize dair aralığı ve sıklığı seçin.

4. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

5. Şimdi, tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevler için mantık uygulamanıza bir veya daha fazla eylem eklemeye devam edin.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>Blob depolama eylemi ekleme

Azure Logic Apps'ta [eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) iş akışınızda bir tetikleyici veya başka bir eylemi izleyen bir adımdır. Bu örnekte, mantık uygulaması [Yineleme tetikleyicisi](../connectors/connectors-native-recurrence.md)ile başlar.

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın. Bu örnekte Azure portalı kullanır.

2. Mantık Uygulama Tasarımcısı' nda, tetikleyici veya eylem altında **Yeni adımı**seçin.

   ![Mantık uygulaması iş akışına yeni adım ekleme](./media/connectors-create-api-azureblobstorage/add-new-step-logic-app-workflow.png) 

   Varolan adımlar arasında eylem eklemek için farenizi bağlanan okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve eylem **ekle'yi**seçin.

3. Arama kutusuna filtreniz olarak "azure blob" girin. Eylemler listesinden, istediğiniz eylemi seçin.

   Bu örnekşu eylemi kullanır: **Blob içeriği alın**

   ![Azure Blob Depolama eylemini seçin](./media/connectors-create-api-azureblobstorage/add-azure-blob-storage-action.png)

4. Bağlantı ayrıntıları için sizden istenirse, [Azure Blob Depolama bağlantınızı şimdi oluşturun.](#create-connection)
Veya bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

   Bu örnekte, istediğiniz dosyayı seçin.

   1. **Blob** kutusundan klasör simgesini seçin.
  
      ![Eylemle birlikte kullanılacak depolama klasörünü seçin](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Blob'un **kimlik** numarasına göre istediğiniz dosyayı bulun ve seçin. Bu **kimlik** numarasını, daha önce açıklanan blob depolama tetikleyicisi tarafından döndürülen blob meta verilerinde bulabilirsiniz.

5. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.
Mantık uygulamanızı sınamak için, seçili klasörün bir leke içerdiğinden emin olun.

Bu örnek yalnızca bir blob için içeriğini alır. İçeriği görüntülemek için, başka bir bağlayıcı kullanarak blob ile bir dosya oluşturan başka bir eylem ekleyin. Örneğin, blob içeriğini temel alan bir dosya oluşturan bir OneDrive eylemi ekleyin.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Depolama hesabına bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Bağlantıyı oluşturmanız istendiğinde, şu bilgileri sağlayın:

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Bağlantı Adı** | Evet | <*bağlantı adı*> | Bağlantınız için oluşturulacak ad |
   | **Depolama Hesabı** | Evet | <*depolama hesabı*> | Listeden depolama hesabınızı seçin. |
   ||||

   Örnek:

   ![Azure Blob depolama hesabı bağlantısı oluşturma](./media/connectors-create-api-azureblobstorage/create-storage-account-connection.png) 

1. Hazır olduğunuzda **Oluştur'u** seçin

1. Bağlantınızı oluşturduktan sonra [blob depolama tetikleyicisi ekle](#add-trigger) veya [blob depolama eylemi ekle](#add-action)ile devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/azureblobconnector/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

<a name="storage-firewalls"></a>

## <a name="access-storage-accounts-behind-firewalls"></a>Güvenlik duvarlarının arkasındaki depolama hesaplarına erişin

Erişimi [güvenlik duvarı ve güvenlik duvarı kurallarıyla](../storage/common/storage-network-security.md)kısıtlayarak Bir Azure depolama hesabına ağ güvenliği ekleyebilirsiniz. Ancak bu kurulum, Azure ve depolama hesabına erişması gereken diğer Microsoft hizmetleri için bir sorun oluşturur. Veri merkezindeki yerel iletişim dahili IP adreslerini özetler, böylece IP kısıtlamalarıyla güvenlik duvarı kuralları ayarlayamadığınız için. Daha fazla bilgi için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](../storage/common/storage-network-security.md).

Azure Blob Depolama bağlayıcısı veya diğer çözümleri kullanarak Azure Logic Apps'tan güvenlik duvarlarının arkasındaki depolama hesaplarına erişmek için çeşitli seçenekler aşağıda verilmiştir:

* Azure Depolama Blobu bağlayıcısı

  * [Diğer bölgelerdeki depolama hesaplarına erişin](#access-other-regions)
  * [Güvenilir bir sanal ağ üzerinden depolama hesaplarına erişin](#access-trusted-virtual-network)

* Diğer çözümler

  * [Depolama hesaplarına yönetilen kimliklerle güvenilir bir hizmet olarak erişin](#access-trusted-service)
  * [Azure API Yönetimi aracılığıyla depolama hesaplarına erişin](#access-api-management)

<a name="access-other-regions"></a>

### <a name="problems-accessing-storage-accounts-in-the-same-region"></a>Aynı bölgedeki depolama hesaplarına erişim sorunları

Mantıksal uygulamalar, her ikisi de aynı bölgedeyken güvenlik duvarlarının arkasındaki depolama hesaplarına doğrudan erişemez. Geçici çözüm olarak, mantıksal uygulamalarınızı depolama hesabınızdan farklı bir bölgeye koyun ve [bölgenizdeki yönetilen bağlayıcılar için giden IP adreslerine](../logic-apps/logic-apps-limits-and-config.md#outbound)erişim verin.

> [!NOTE]
> Bu çözüm, Azure Table Storage bağlayıcısı ve Azure Sıra Depolama bağlayıcısı için geçerli değildir. Bunun yerine, Tablo Depolama veya Sıra Depolama erişmek için yerleşik HTTP tetikleyici ve eylemleri kullanın.

<a name="access-trusted-virtual-network"></a>

### <a name="access-storage-accounts-through-a-trusted-virtual-network"></a>Güvenilir bir sanal ağ üzerinden depolama hesaplarına erişin

Depolama hesabını yönettiğiniz bir Azure sanal ağına koyabilir ve ardından bu sanal ağı güvenilen sanal ağlar listesine ekleyebilirsiniz. Mantık uygulamanızın güvenilir bir sanal [ağ](../virtual-network/virtual-networks-overview.md)üzerinden depolama hesabına erişebilmeniz için, bu mantık uygulamasını sanal ağdaki kaynaklara bağlanabilen bir [entegrasyon hizmet ortamına (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)dağıtmanız gerekir. Daha sonra, bu İmKB'deki alt ağları güvenilir listeye ekleyebilirsiniz. Blob Depolama konektörü gibi Azure Depolama konektörleri depolama kabına doğrudan erişebilir. Bu kurulum, Bir İmKB'deki hizmet uç noktalarını kullanmakla aynı deneyimdir.

<a name="access-trusted-service"></a>

### <a name="access-storage-accounts-as-a-trusted-service-with-managed-identities"></a>Depolama hesaplarına yönetilen kimliklerle güvenilir bir hizmet olarak erişin

Microsoft'un güvenilen hizmetlere bir güvenlik duvarı aracılığıyla bir depolama hesabına erişmesini sağlamak için, bu hizmetler için bu depolama hesabında bir özel durum ayarlayabilirsiniz. Bu çözüm, güvenlik duvarlarının arkasındaki depolama hesaplarına güvenilir hizmetler olarak erişmek [için kimlik doğrulaması için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/overview.md) destekleyen Azure hizmetlerine izin verir. Özellikle, küresel çok kiracılı Azure'daki bir mantık uygulamasının bu depolama hesaplarına erişebilmesi için, öncelikle mantık uygulamasında [yönetilen kimlik desteğini etkinleştirin.](../logic-apps/create-managed-service-identity.md) Ardından, mantık uygulamanızda HTTP eylemini veya tetikleyicisini kullanır ve [kimlik doğrulama türünü mantık uygulamanızın yönetilen kimliğini kullanacak şekilde ayarlarsınız.](../logic-apps/create-managed-service-identity.md#authenticate-access-with-managed-identity) Bu senaryo için yalnızca HTTP *eylemini* veya tetikleyicisini kullanabilirsiniz.

Özel durum ve yönetilen kimlik desteğini ayarlamak için aşağıdaki genel adımları izleyin:

1. Depolama hesabınızda Ayarlar **altında** **Güvenlik Duvarları'nı ve sanal ağları**seçin. **Erişime izin ver'in**altında, ilgili ayarların görünmesi için **Seçili Ağlar** seçeneğini seçin.

1. **Özel Durumlar**altında, bu **depolama hesabına erişmek için güvenilen Microsoft hizmetlerine İzin ver'i**seçin ve ardından **Kaydet'i**seçin.

   ![Microsoft'un güvendiği hizmetlere izin veren özel durum seçme](./media/connectors-create-api-azureblobstorage/allow-trusted-services-firewall.png)

1. Mantık uygulamanızın ayarlarında, [yönetilen kimlik için destek etkinleştirin.](../logic-apps/create-managed-service-identity.md)

1. Mantık uygulamanızın iş akışında, depolama hesabına veya varlığına erişmek için HTTP eylemini veya tetikleyicisini ekleyin ve ayarlayın.

   > [!IMPORTANT]
   > Giden HTTP eylemi veya Azure Depolama hesaplarına yapılan çağrıları tetiklemek `x-ms-version` için, istek üstbilgisinin depolama hesabında çalıştırmak istediğiniz işlem için özellik ve API sürümünü içerdiğinden emin olun. Daha fazla bilgi için, yönetilen kimlik ve [Azure Depolama hizmetleri için Sürüm'le](https://docs.microsoft.com/rest/api/storageservices/versioning-for-the-azure-storage-services#specifying-service-versions-in-requests)erişim [kimliğine](../logic-apps/create-managed-service-identity.md#authenticate-access-with-managed-identity) yönelik erişim kimliğine bakın.

1. Bu eylemde, kimlik doğrulaması için kullanılacak [yönetilen kimliği seçin.](../logic-apps/create-managed-service-identity.md#authenticate-access-with-managed-identity)

<a name="access-api-management"></a>

### <a name="access-storage-accounts-through-azure-api-management"></a>Azure API Yönetimi aracılığıyla depolama hesaplarına erişin

[API Yönetimi](../api-management/api-management-key-concepts.md)için özel bir katman kullanıyorsanız, API Yönetimi'ni kullanarak ve güvenlik duvarından ikincisinin IP adreslerine izin vererek Depolama API'sinin önüne geçebilirsiniz. Temel olarak, API Yönetimi tarafından kullanılan Azure sanal ağını depolama hesabının güvenlik duvarı ayarına ekleyin. Daha sonra, Azure Depolama API'lerini aramak için API Yönetimi eylemini veya HTTP eylemini kullanabilirsiniz. Ancak, bu seçeneği seçerseniz, kimlik doğrulama işlemini kendiniz işlemeniz gerekir. Daha fazla bilgi için [Basit kurumsal tümleştirme mimarisine](https://aka.ms/aisarch)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
