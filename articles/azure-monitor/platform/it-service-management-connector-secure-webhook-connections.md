---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma
description: Bu makalede, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de güvenli dışarı aktarma ile ıSM ürünlerinizi/hizmetlerinizi nasıl bağlayabileceğinizi gösterir.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: 85ff3bed2a648f852c311fefa8513622c2a48285
ms.sourcegitcommit: 051908e18ce42b3b5d09822f8cfcac094e1f93c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94376545"
---
# <a name="connect-azure-to-itsm-tools-by-using-secure-export"></a>Güvenli dışarı aktarma kullanarak Azure 'dan ıTSM araçlarına bağlanma

Bu makalede, güvenli dışarı aktarma kullanarak BT hizmet yönetimi (ıTSM) ürün veya hizmeti arasındaki bağlantının nasıl yapılandırılacağı gösterilir.

Güvenli dışarı aktarma [BT hizmet yönetimi Bağlayıcısı (ıSMC)](./itsmc-overview.md)güncelleştirilmiş bir sürümüdür. Her iki sürüm de Azure Izleyici uyarı gönderdiğinde bir ITSM aracında iş öğeleri oluşturmanızı sağlar. Bu işlevsellik ölçüm, günlük ve etkinlik günlüğü uyarılarını içerir.

ISMC Kullanıcı adı ve parola kimlik bilgilerini kullanıyor. Güvenli dışarı aktarmanın Azure Active Directory (Azure AD) kullandığından daha güçlü kimlik doğrulaması vardır. Azure AD, Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir. Kullanıcıların oturum açıp iç veya dış kaynaklara erişmesine yardımcı olur. ITSM ile Azure AD 'nin kullanılması, dış sisteme gönderilen Azure uyarılarını (Azure AD uygulama KIMLIĞI aracılığıyla) belirlemenize yardımcı olur.

> [!NOTE]
> Güvenli dışarı aktarma kullanarak Azure 'dan ıTSM araçlarına bağlanma özelliği önizlemededir.

## <a name="secure-export-architecture"></a>Güvenli dışarı aktarma mimarisi

Güvenli dışarı aktarma mimarisi aşağıdaki yeni özellikleri tanıtır:

* **Yeni eylem grubu** : uyarılar, ITSM eylem grubu yerine güvenli Web kancası eylem grubu aracılığıyla ITSM aracına gönderilir.
* **Azure AD kimlik doğrulaması** : kimlik doğrulaması, Kullanıcı adı/parola kimlik bilgileri yerıne Azure AD aracılığıyla yapılır.

## <a name="secure-export-data-flow"></a>Güvenli dışarı aktarma veri akışı

Güvenli dışarı aktarma veri akışı adımları şunlardır:

1. Azure Izleyici, güvenli dışarı aktarma kullanmak üzere yapılandırılmış bir uyarı gönderir.
2. Uyarı yükü, ıTSM aracına güvenli bir Web kancası eylemi tarafından gönderilir.
3. Uyarının ıTSM aracını girme yetkisi varsa, ıTSM uygulaması Azure AD ile kontrol eder.
4. Uyarı yetkilendirilirse, uygulama:
   
   1. ITSM aracında bir iş öğesi (örneğin, bir olay) oluşturur.
   2. Yapılandırma öğesinin (CI) KIMLIĞINI müşteri yönetim veritabanına (CMDB) bağlar.

![ITSM aracının Azure A, Azure uyarıları ve bir eylem grubuyla nasıl iletişim kuracağını gösteren diyagram.](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="benefits-of-secure-export"></a>Güvenli dışarı aktarma avantajları

Tümleştirmenin başlıca avantajları şunlardır:

* **Daha iyi kimlik doğrulaması** : Azure AD, genellikle ısmc 'da oluşan zaman aşımları olmadan daha güvenli kimlik doğrulaması sağlar.
* **ITSM aracında çözümlenen uyarılar** : ölçüm uyarıları "tetiklenir" ve "çözümlendi" durumlarını uygular. Koşul karşılandığında, uyarı durumu "tetiklenir" olur. Koşul artık karşılanmazsa, uyarı durumu "çözüldü" olur. ISMC 'da, uyarılar otomatik olarak çözümlenemiyor. Güvenli dışarı aktarma sayesinde, çözümlenen durum ıTSM aracına akar ve bu nedenle otomatik olarak güncelleştirilir.
* **[Ortak uyarı şeması](./alerts-common-schema.md)** : ısmc 'da, uyarı yükünün şeması, uyarı türüne göre farklılık gösterir. Güvenli dışarı aktarma bölümünde tüm uyarı türleri için ortak bir şema vardır. Bu ortak şema tüm uyarı türleri için CI 'yi içerir. Tüm Uyarı türleri CI 'yi CMDB ile bağlayabilecektir.

ITSM Bağlayıcısı aracını şu adımlarla kullanmaya başlayın:

1. Uygulamanızı Azure AD'ye kaydetme.
2. Güvenli bir Web kancası eylem grubu oluşturun.
3. İş ortağı ortamınızı yapılandırın. 

Güvenli dışarı aktarma aşağıdaki ıTSM araçlarıyla bağlantıları destekler:
* [ServiceNow](https://docs.microsoft.com/azure/azure-monitor/platform/it-service-management-connector-secure-webhook-connections#connect-servicenow-to-azure-monitor)
* [BMC Helix](https://docs.microsoft.com/azure/azure-monitor/platform/it-service-management-connector-secure-webhook-connections#connect-bmc-helix-to-azure-monitor)

## <a name="register-with-azure-active-directory"></a>Azure Active Directory Kaydet

Uygulamayı Azure AD 'ye kaydetmek için şu adımları izleyin:

1. [Microsoft Identity platformu ile uygulama kaydetme](../../active-directory/develop/quickstart-register-app.md)bölümündeki adımları izleyin.
2. Azure AD 'de **uygulamayı kullanıma** sunma ' yı seçin.
3. **Uygulama kimliği URI 'si** için **Ayarla** ' yı seçin.

   [![Uygulamanın U R I 'yi ayarlama seçeneğinin ekran görüntüsü.](media/it-service-management-connector-secure-webhook-connections/azure-ad.png)](media/it-service-management-connector-secure-webhook-connections/azure-ad-expand.png#lightbox)
4. **Kaydet** ’i seçin.

## <a name="create-a-secure-webhook-action-group"></a>Güvenli Web kancası eylem grubu oluşturma

Uygulamanız Azure AD 'ye kaydedildikten sonra, işlem gruplarındaki güvenli Web kancası eylemini kullanarak ıTSM aracınız üzerinde Azure uyarılarını temel alan iş öğeleri oluşturabilirsiniz.

Eylem grupları, Azure uyarıları için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. İşlem gruplarını, ölçüm uyarıları, etkinlik günlüğü uyarıları ve Azure portal Azure Log Analytics uyarıları ile birlikte kullanabilirsiniz.
Eylem grupları hakkında daha fazla bilgi edinmek için [Azure Portal eylem grupları oluşturma ve yönetme](./action-groups.md)konusuna bakın.

Bir eyleme Web kancası eklemek için güvenli Web kancası için aşağıdaki yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com/), **izleme** ' yi arayıp seçin. **İzleyici** bölmesi tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.
2. **Uyarıları**  >  **Yönet eylemler** ' i seçin.
3. [Eylem grubu Ekle](./action-groups.md#create-an-action-group-by-using-the-azure-portal)' yi seçin ve alanları girin.
4. **Eylem grubu adı** kutusuna bir ad girin ve **kısa ad** kutusuna bir ad girin. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.
5. **Güvenli Web kancasını** seçin.
6. Şu ayrıntıları seçin:
   1. Kaydettiğiniz Azure Active Directory örneğinin nesne KIMLIĞINI seçin.
   2. URI için, [ITSM aracı ortamından](https://docs.microsoft.com/azure/azure-monitor/platform/it-service-management-connector-secure-webhook-connections#configure-the-partner-environment)kopyaladığınız Web kancası URL 'sini yapıştırın.
   3. **Ortak uyarı şemasını** **Evet** olarak ayarlayın. 

   Aşağıdaki görüntüde örnek bir güvenli Web kancası eyleminin yapılandırması gösterilmektedir:

   ![Güvenli Web kancası eylemini gösteren ekran görüntüsü.](media/it-service-management-connector-secure-webhook-connections/secure-webhook.png)

## <a name="configure-the-itsm-tool-environment"></a>ITSM aracı ortamını yapılandırma

Yapılandırma 2 adım içerir:
1. Güvenli dışarı aktarma tanımının URI 'sini alın.
2. ITSM aracının akışına göre tanımlar.


### <a name="connect-servicenow-to-azure-monitor"></a>ServiceNow 'ı Azure Izleyici 'ye bağlama

Aşağıdaki bölümlerde, Azure 'da ServiceNow ürününüzü bağlama ve güvenli dışarı aktarma işlemlerinin nasıl yapılacağı hakkında ayrıntılı bilgi sağlanmaktadır.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları karşılatığınızdan emin olun:

* Azure AD kaydedilir.
* ServiceNow olay yönetimi-ıTOM 'un desteklenen sürümüne sahipsiniz (sürüm Orlando veya üzeri).

### <a name="configure-the-servicenow-connection"></a>ServiceNow bağlantısını yapılandırma

1. bağlantı https://(örnek adı). Service-Now. com/API/sn_em_connector/em/inbound_event? Source = azuremonitor, güvenli dışarı aktarma tanımının URI 'sini kullanın.

2. Sürüme göre yönergeleri izleyin:
   * [Paris](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/concept/azure-integration.html)
   * [Tarihleri arasında Orlando](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/concept/azure-integration.html)

### <a name="connect-bmc-helix-to-azure-monitor"></a>BMC Helix 'i Azure Izleyici 'ye bağlama

Aşağıdaki bölümlerde, BMC Helix ürününüzü bağlama ve Azure 'da güvenli dışarı aktarma konularında ayrıntılar sağlanmaktadır.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları karşılatığınızdan emin olun:

* Azure AD kaydedilir.
* Desteklenen BMC Helix Multi-Cloud Service Management sürümünüz (sürüm 19,08 veya üzeri) vardır.

### <a name="configure-the-bmc-helix-connection"></a>BMC Helix bağlantısını yapılandırma

1. güvenli dışarı aktarma için URI 'yi almak üzere BMC Helix ortamında aşağıdaki yordamı kullanın:

   1. Integration Studio 'da oturum açın.
   2. **Azure uyarıları akışından olay oluştur** ' a yönelik arama yapın.
   3. Web kancası URL 'sini kopyalayın.
   
   ![Integration Studio 'daki Web kancası U R L 'nin ekran görüntüsü.](media/it-service-management-connector-secure-webhook-connections/bmc-url.png)
   
2. Sürüme göre yönergeleri izleyin:
   * [Sürüm 20,02 Için Azure izleyici ile önceden oluşturulmuş tümleştirmeyi etkinleştirme](https://docs.bmc.com/docs/multicloud/enabling-prebuilt-integration-with-azure-monitor-879728195.html).
   * [Sürüm 19,11 Için Azure izleyici ile önceden oluşturulmuş tümleştirmeyi etkinleştirme](https://docs.bmc.com/docs/multicloudprevious/enabling-prebuilt-integration-with-azure-monitor-904157623.html).

3. BMC Helix 'teki bağlantının yapılandırmasının bir parçası olarak, tümleştirme BMC örneğine gidin ve şu yönergeleri izleyin:

   1. **Katalog** ' u seçin.
   2. **Azure uyarıları** ' nı seçin.
   3. **Bağlayıcılar** ' ı seçin.
   4. **Yapılandırma** ' yı seçin.
   5. **Yeni bağlantı yapılandırması Ekle** ' yi seçin.
   6. Yapılandırma bölümüne ilişkin bilgileri girin:
      - **Ad** : kendinizinkini oluşturun.
      - **Yetkilendirme türü** : **yok**
      - **Açıklama** : kendinizinkini oluşturun.
      - **Site** : **bulut**
      - **Örnek sayısı** : **2** , varsayılan değer.
      - **Denetle** : kullanımı etkinleştirmek için varsayılan olarak seçilidir.
      - Azure kiracı KIMLIĞI ve Azure uygulama KIMLIĞI, daha önce tanımladığınız uygulamadan alınır.

![BMC yapılandırmasını gösteren ekran görüntüsü.](media/it-service-management-connector-secure-webhook-connections/bmc-configuration.png)




## <a name="next-steps"></a>Sonraki adımlar

* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-overview.md)
