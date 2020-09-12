---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma
description: Bu makalede, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için ITSM ürünlerinizi/hizmetlerinizi Azure Izleyici 'de güvenli dışarı aktarma ile bağlama hakkında bilgi sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: 160054e7e98dc2cb06c2c7daf325536766963daa
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89568651"
---
# <a name="connect-azure-to-itsm-tools-using-secure-export"></a>Azure 'ı güvenli dışarı aktarma kullanarak ıTSM araçlarına bağlama

Bu makalede, güvenli dışarı aktarma kullanarak ıTSM ürününüz/hizmetiniz arasındaki bağlantının nasıl yapılandırılacağı hakkında bilgi sağlanır.

Güvenli dışarı aktarma ve güncelleştirilmiş [ısmc](./itsmc-overview.md) (BT hizmet yönetimi) sürümü. Her iki sürüm de Azure Izleyici uyarıları başlatıldığında bir ıTSM aracında iş öğeleri oluşturmanızı sağlar. Bu işlevsellik ölçüm, günlük ve etkinlik günlüğü uyarılarını içerir.

[Ismc](./itsmc-overview.md) Kullanıcı ve parola kimlik bilgilerini kullandığından, güvenli dışarı aktarmanın Azure Active Directory (Azure AD) kullandığından daha güçlü kimlik doğrulaması vardır. Azure Active Directory (Azure AD), Microsoft 'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir. Kullanıcıların oturum açıp iç veya dış kaynaklara erişmesine yardımcı olur. ITSM ile Azure AD 'nin kullanılması, dış sisteme gönderilen Azure uyarılarını (Azure AD uygulama KIMLIĞI kullanılarak) belirlemenize yardımcı olur.

> [!NOTE]
> Güvenli dışarı aktarma kullanarak Azure 'dan ıTSM araçlarına bağlanma önizleme aşamasındadır

## <a name="secure-export-architecture"></a>Güvenli dışarı aktarma mimarisi

Güvenli dışarı aktarma mimarisi aşağıdaki yeni özellikleri tanıtır:

* **Yeni eylem grubu** -uyarılar, güvenli Web kancası eylem grubu kullanılarak ITSM aracına gönderilir (ısmc ' de kullanılarak ıTSSM eylem grubu yerine).
* **Azure AD kimlik doğrulaması** -kimlik doğrulama, Kullanıcı/parola kimlik bilgileri yerıne Azure ad kullanılarak gerçekleştirilir.

## <a name="secure-export-data-flow"></a>Güvenli dışarı aktarma veri akışı

Güvenli dışarı aktarma veri akışı adımları şunlardır:

1) Azure Izleyici 'de güvenli dışarı aktarma kullanmak üzere yapılandırılan bir uyarı
2) Uyarı yükü, ıTSM aracına güvenli bir Web kancası eylemi tarafından gönderilir.
3) Uyarının ıTSM aracını girme yetkisi varsa, ıTSM uygulaması Azure AD ile kontrol eder.
4) Uyarı uygulamayı yetkilendirirse:
    1) ITSM aracında bir iş öğesi (ör. olay) oluşturur.
    2) Yapılandırma öğesi (CI) KIMLIĞINI müşteri yönetim veritabanına (CMDB) bağlar.
![ITSM diyagramı](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="connection-with-bmc-helix"></a>BMC Helix ile bağlantı

Güvenli dışarı aktarma, BMC Helix destekler. Tümleştirmenin bazı avantajları şunlardır:

* **Daha iyi kimlik doğrulaması** : Azure AD, genellikle ısmc 'da oluşan zaman aşımları olmadan daha güvenli kimlik doğrulaması sağlar.
* **ITSM aracında çözümlenen uyarılar** – ölçüm uyarıları "tetiklenir" ve "çözümlendi" durumunu uygular. Koşul karşılandığında, uyarı durumu "tetiklenir" olur. Koşul artık karşılanmazsa, uyarı durumu "çözümlendi" olur. ISMC 'da, uyarılar otomatik olarak çözümlenemedi. Güvenli dışarı aktarma sayesinde, çözümlenen durum ıTSM aracına akar ve bu nedenle otomatik olarak güncelleştirilir.
* **[Ortak şema izin verir](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema)** : ısmc 'da, uyarı yükünün şeması, uyarı türüne göre farklıydı. Güvenli dışarı aktarma bölümünde tüm uyarı türleri için ortak bir şema yaptık. Bu yeni ortak şema tüm uyarı türleri için CI 'yi içerir. Bu, tüm uyarı türleri için CI 'yı CMDB ile bağlayabilecektir.

Aşağıdaki adımlarla ITSM Bağlayıcısı kullanmaya başlayın:

1. Uygulamanızı Azure Active Directory kaydedin.
2. Güvenli bir Web kancası eylem grubu oluşturun.
3. İş ortağı ortamınızı yapılandırın.

## <a name="register-with-azure-active-directory"></a>Azure Active Directory Kaydet

Azure AD uygulamasını Azure Active Directory kaydetmek için şu adımları izleyin

1) [Azure AD oluşturma](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)
2) Azure Active Directory "uygulamayı kullanıma sunma" seçeneğini belirleyin
3) Uygulama KIMLIĞI URI 'SI [ ![ Azure AD](media/it-service-management-connector-secure-webhook-connections/azure-ad.png)](media/it-service-management-connector-secure-webhook-connections/azure-ad-expand.png#lightbox) üzerinde ayarla ' yı seçin
4) Kaydet’e tıklayın.

## <a name="create-a-secure-webhook-action-group"></a>Güvenli Web kancası eylem grubu oluşturma

Azure AD 'nizi kaydettikten sonra, Işlem gruplarındaki güvenli Web kancası eylemini kullanarak ıTSM aracınız üzerinde Azure uyarılarını temel alan iş öğeleri oluşturabilirsiniz.
Eylem grupları, Azure uyarılarınız için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. Işlem gruplarını, ölçüm uyarıları, etkinlik günlüğü uyarıları ve Azure portal Azure Log Analytics uyarıları ile birlikte kullanabilirsiniz.
Eylem grupları hakkında daha fazla bilgi edinmek için [Azure Portal eylem grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)konusuna bakın.
Aşağıdaki yordamı kullanın:

BMC Helix ortamında:

1. Integration Studio 'da oturum açın.
2. Azure uyarıları akışından olay oluştur ' a yönelik arama yapın.
3. Web kancası URL 'sini kopyalayın.
![BMC URL 'SI](media/it-service-management-connector-secure-webhook-connections/bmc-url.png)

Bir eyleme Web kancası eklemek için güvenli bir Web kancası için yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com/), **izleme**' yi arayıp seçin. **İzleyici** bölmesi tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.
2. **Uyarılar**'ı ve ardından **Eylemleri yönet**'i seçin.
3. [Eylem grubu Ekle](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal)' yi seçin ve alanları girin.
4. **Eylem grubu adı** kutusuna bir ad girin ve **kısa ad** kutusuna bir ad girin. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.
5. **Güvenli Web kancasını** seçin
6. Ayrıntıları Düzenle ' yi seçin. Aşağıdaki görüntüde örnek bir güvenli Web kancası eylemi gösterilmektedir:
    1. Kaydettiğiniz Azure Active Directory doğru nesne KIMLIĞINI seçin
    2. URI alanına "BMC Helix ortamından kopyaladığınız Web kancası URL 'sini yapıştırın
    3. **Ortak uyarı şemasını** **Evet**olarak ayarlayın. 
7. Aşağıdaki görüntüde örnek bir güvenli Web kancası eylem yapılandırması gösterilmektedir: ![ Güvenli Web kancası](media/it-service-management-connector-secure-webhook-connections/secure-webhook.png)

## <a name="configure-partner-environment"></a>İş ortağı ortamını yapılandırma

### <a name="connect-bmc-helix-to-azure-monitor"></a>BMC Helix 'i Azure Izleyici 'ye bağlama

Aşağıdaki bölümde, BMC Helix ürününüzü bağlama ve Azure 'da güvenli dışarı aktarma hakkında ayrıntılar verilmektedir.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

* Azure AD kaydedilir.
* Desteklenen bir BMC Helix Multi-Cloud Service Management sürümü: 20,02 sürümü veya üzeri

BMC Helix bağlantısını yapılandırmak için:

1) [Sürüm 20,2 için Azure Izleyici ile önceden oluşturulmuş tümleştirmeyi etkinleştirme](https://docs.bmc.com/docs/multicloud/enabling-prebuilt-integration-with-azure-monitor-879728195.html)

2) BMC Helix 'teki bağlantının yapılandırmasının bir parçası olarak, tümleştirme BMC örneğine gidin ve yönergeleri izleyin:

1. **Katalog** Seç
2. **Azure uyarılarını** seçin
3. **Bağlayıcıları** seçin
4. **Yapılandırma** Seç
5. **Yeni bağlantı yapılandırması Ekle** 'yi seçin
6. Yapılandırma bölümünün bilgilerini girin.
    1. **Ad** -kendinizinkini oluşturun
    2. **Yetkilendirme türü** -yok
    3. **Açıklama**-kendinizinkini oluşturma
    4. **Site**-bulut
    5. **Örnek sayısı** -2 – varsayılan değer
    6. **Çek** -varsayılan olarak seçilidir ve kullanımı etkinleştirir
    7. Azure kiracı KIMLIĞI, Azure uygulama KIMLIĞI, "oluşturulan Azure Active Directory" adımında tanımlanan uygulamadan alınır.
![BMC yapılandırması](media/it-service-management-connector-secure-webhook-connections/bmc-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
