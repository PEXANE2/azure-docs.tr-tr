---
title: BT Hizmet Yönetimi Bağlayıcısı-Azure Izleyici 'de güvenli dışarı aktarma-Azure yapılandırması
description: Bu makalede, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de güvenli dışarı aktarma özelliğiyle ıSM ürünlerinizi/hizmetlerinizi bağlamak üzere Azure 'un nasıl yapılandırılacağı gösterilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 01/03/2021
ms.openlocfilehash: 5eb58c48acc7974a4379cf1993a73228c99f5e6d
ms.sourcegitcommit: 697638c20ceaf51ec4ebd8f929c719c1e630f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97857582"
---
# <a name="configure-azure-to-connect-itsm-tools-using-secure-export"></a>Güvenli dışarı aktarma kullanarak ıTSM araçlarına bağlanmak için Azure 'ı yapılandırma

Bu makalede, Azure 'un "güvenli dışarı aktarma" kullanmak üzere nasıl yapılandırılacağı hakkında bilgi sağlanır.
"Güvenli dışarı aktarma" kullanmak için şu adımları izleyin:

1. [Uygulamanızı Azure AD'ye kaydetme.](./itsm-connector-secure-webhook-connections-azure-configuration.md#register-with-azure-active-directory)
1. [Hizmet sorumlusunu tanımlayın.](./itsm-connector-secure-webhook-connections-azure-configuration.md#define-service-principal)
1. [Güvenli bir Web kancası eylem grubu oluşturun.](./itsm-connector-secure-webhook-connections-azure-configuration.md#create-a-secure-webhook-action-group)
1. İş ortağı ortamınızı yapılandırın.
    Güvenli dışarı aktarma aşağıdaki ıTSM araçlarıyla bağlantıları destekler:
    * [ServiceNow](./itsmc-secure-webhook-connections-servicenow.md)
    * [BMC Helix](./itsmc-secure-webhook-connections-bmc.md)

## <a name="register-with-azure-active-directory"></a>Azure Active Directory Kaydet

Uygulamayı Azure AD 'ye kaydetmek için şu adımları izleyin:

1. [Microsoft Identity platformu ile uygulama kaydetme](../../active-directory/develop/quickstart-register-app.md)bölümündeki adımları izleyin.
2. Azure AD 'de **uygulamayı kullanıma** sunma ' yı seçin.
3. **Uygulama kimliği URI 'si** için **Ayarla** ' yı seçin.

   [![Uygulamanın U R I 'yi ayarlama seçeneğinin ekran görüntüsü.](media/it-service-management-connector-secure-webhook-connections/azure-ad.png)](media/it-service-management-connector-secure-webhook-connections/azure-ad-expand.png#lightbox)
4. **Kaydet**’i seçin.

## <a name="define-service-principal"></a>Hizmet sorumlusunu tanımlama

Hizmet ile kimlik doğrulaması yapmak için, eylem grup hizmeti 'nin AAD uygulamanızdan kimlik doğrulama belirteçleri alma izni olması gerekir. Bu izinleri vermek için kiracınızdaki eylem grubu hizmeti için bir hizmet sorumlusu oluşturmanız gerekir.
Bu amaçla, bu [PowerShell komutlarını](./action-groups.md#secure-webhook-powershell-script) kullanabilirsiniz. (Kiracı yönetici ayrıcalıkları gerektirir).
İsteğe bağlı bir adım olarak, oluşturulan uygulama bildiriminde uygulama rolünü tanımlayabilir ve bu, erişimi daha da belirli bir role sahip olan belirli uygulamaların ileti gönderebileceği şekilde kısıtlayabilir. Bu rolün daha sonra eylem grubu hizmet sorumlusuna atanması gerekmez. \
Bu adım aynı [PowerShell komutları](./action-groups.md#secure-webhook-powershell-script)aracılığıyla yapılabilir.

## <a name="create-a-secure-webhook-action-group"></a>Güvenli Web kancası eylem grubu oluşturma

Uygulamanız Azure AD 'ye kaydedildikten sonra, işlem gruplarındaki güvenli Web kancası eylemini kullanarak ıTSM aracınız üzerinde Azure uyarılarını temel alan iş öğeleri oluşturabilirsiniz.

Eylem grupları, Azure uyarıları için eylemleri tetiklemenin modüler ve yeniden kullanılabilir bir yolunu sağlar. İşlem gruplarını, ölçüm uyarıları, etkinlik günlüğü uyarıları ve Azure portal Azure Log Analytics uyarıları ile birlikte kullanabilirsiniz.
Eylem grupları hakkında daha fazla bilgi edinmek için [Azure Portal eylem grupları oluşturma ve yönetme](./action-groups.md)konusuna bakın.

Bir eyleme Web kancası eklemek için güvenli Web kancası için aşağıdaki yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com/), **izleme**' yi arayıp seçin. **İzleyici** bölmesi tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.
2. **Uyarıları**  >  **Yönet eylemler**' i seçin.
3. [Eylem grubu Ekle](./action-groups.md#create-an-action-group-by-using-the-azure-portal)' yi seçin ve alanları girin.
4. **Eylem grubu adı** kutusuna bir ad girin ve **kısa ad** kutusuna bir ad girin. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.
5. **Güvenli Web kancasını** seçin.
6. Şu ayrıntıları seçin:
   1. Kaydettiğiniz Azure Active Directory örneğinin nesne KIMLIĞINI seçin.
   2. URI için, [ITSM aracı ortamından](#configure-the-itsm-tool-environment)kopyaladığınız Web kancası URL 'sini yapıştırın.
   3. **Ortak uyarı şemasını** **Evet** olarak ayarlayın. 

   Aşağıdaki görüntüde örnek bir güvenli Web kancası eyleminin yapılandırması gösterilmektedir:

   ![Güvenli Web kancası eylemini gösteren ekran görüntüsü.](media/it-service-management-connector-secure-webhook-connections/secure-webhook.png)

## <a name="configure-the-itsm-tool-environment"></a>ITSM aracı ortamını yapılandırma

Yapılandırma iki adım içerir:

1. Güvenli dışarı aktarma tanımının URI 'sini alın.
2. ITSM aracının akışına göre tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

* [ServiceNow güvenli dışarı aktarma yapılandırması](./itsmc-secure-webhook-connections-servicenow.md)
* [BMC güvenli dışarı aktarma yapılandırması](./itsmc-secure-webhook-connections-bmc.md)
