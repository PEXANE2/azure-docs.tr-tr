---
title: Azure Analiz Hizmetleri modelleri için Logic Apps ile yenileyin | Microsoft Dokümanlar
description: Bu makalede, Azure Mantık Uygulamaları kullanarak Azure Analiz Hizmetleri için eşzamanlı yenilemenin nasıl kodlanır.
author: chrislound
ms.service: analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: chlound
ms.openlocfilehash: 78bc629598c0635b7760285d0507b7a85a4ab551
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126927"
---
# <a name="refresh-with-logic-apps"></a>Logic Apps ile yenileme

Logic Apps ve REST çağrılarını kullanarak, sorgu ölçeklendirmesi için salt okunur yinelemelerin senkronizeleştirilmesi de dahil olmak üzere Azure Analizi tabular modellerinizde otomatik veri yenileme işlemleri gerçekleştirebilirsiniz.

Azure Analiz Hizmetleri ile REST API'lerini kullanma hakkında daha fazla bilgi edinmek için [REST API'si ile Eşzamanlı yenile'ye](analysis-services-async-refresh.md)bakın.

## <a name="authentication"></a>Kimlik doğrulaması

Tüm aramaların geçerli bir Azure Etkin Dizini (OAuth 2) belirteciyle kimlik doğrulaması yapılmalıdır.  Bu makaledeki örnekler, Azure Çözümleme Hizmetleri'nin kimliğini doğrulamak için bir Hizmet Sorumlusu (SPN) kullanır. Daha fazla bilgi için Azure [portalLarını kullanarak bir hizmet sorumlusu oluştur'a](../active-directory/develop/howto-create-service-principal-portal.md)bakın.

## <a name="design-the-logic-app"></a>Mantık uygulamasını tasarla

> [!IMPORTANT]
> Aşağıdaki örnekler, Azure Çözümleme Hizmetleri güvenlik duvarının devre dışı bırakıldığını varsayar. Güvenlik duvarı etkinse, istek başlatıcısının genel IP adresi Azure Çözümleme Hizmetleri güvenlik duvarında beyaz listeye alınmalıdır. Azure Logic Apps BÖLGE İçİn IP aralıkları hakkında daha fazla bilgi edinmek [için Azure Mantık Uygulamaları için Sınırlar ve yapılandırma bilgilerine](../logic-apps/logic-apps-limits-and-config.md#configuration)bakın.

### <a name="prerequisites"></a>Ön koşullar

#### <a name="create-a-service-principal-spn"></a>Hizmet Sorumlusu Oluşturma (SPN)

Hizmet Sorumlusu oluşturma hakkında bilgi edinmek için Azure [portalını kullanarak bir hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md)'na bakın.

#### <a name="configure-permissions-in-azure-analysis-services"></a>Azure Çözümleme Hizmetlerinde izinleri yapılandırma
 
Oluşturduğunuz Hizmet Yöneticisi'nin sunucuda sunucu yöneticisi izinleri olmalıdır. Daha fazla bilgi için bkz. [sunucu yöneticisi rolüne bir hizmet sorumlusu ekle.](analysis-services-addservprinc-admins.md)

### <a name="configure-the-logic-app"></a>Mantık Uygulamasını Yapılandırma

Bu örnekte, Bir HTTP isteği geldiğinde Mantık Uygulaması tetiklemek için tasarlanmıştır. Bu, Azure Veri Fabrikası gibi bir düzenleme aracının kullanılmasını sağlayarak Azure Çözümleme Hizmetleri modelinin yenilenmesini tetikler.

Bir Mantık Uygulaması oluşturduktan sonra:

1. Mantık Uygulaması tasarımcısında, **bir HTTP isteği alındığı zaman**ilk eylemi seçin.

   ![HTTP alınan etkinliği ekleme](./media/analysis-services-async-refresh-logic-app/1.png)

Bu adım, Mantık Uygulaması kaydedildikten sonra HTTP POST URL ile doldurulur.

2. Yeni bir adım ekleyin ve **HTTP**için arama yapın.  

   ![HTTP etkinliği ekleme](./media/analysis-services-async-refresh-logic-app/9.png)

   ![HTTP etkinliği ekleme](./media/analysis-services-async-refresh-logic-app/10.png)

3. Bu eylemi eklemek için **HTTP'yi** seçin.

   ![HTTP etkinliği ekleme](./media/analysis-services-async-refresh-logic-app/2.png)

HTTP etkinliğini aşağıdaki gibi yapılandırın:

|Özellik  |Değer  |
|---------|---------|
|**Yöntem**     |POST         |
|**URI**     | sunucu*bölgenizi /sunucularınızı/**aas sunucu adınızı*/modellerinizi/ veritabanı adınızı /yenilenizi https://*your database name* <br /> <br /> Örneğin: https:\//westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes|
|**Üst Bilgiler**     |   İçerik Türü, uygulama/json <br /> <br />  ![Üst bilgiler](./media/analysis-services-async-refresh-logic-app/6.png)    |
|**Gövde**     |   İstek gövdesi oluşturma hakkında daha fazla bilgi edinmek için [REST API - POST /refreshes ile Asynchronous yenile'](analysis-services-async-refresh.md#post-refreshes)ye bakın. |
|**Kimlik doğrulaması**     |Aktif Dizin OAuth         |
|**Kiracı**     |Azure Etkin Dizin KiracıKimliğinizi doldurun         |
|**Hedef kitle**     |https://*.asazure.windows.net         |
|**İstemci Kimliği**     |Hizmet Ana Adınızı Girin Müşteri Kimliği         |
|**Kimlik Bilgisi Türü**     |Gizli dizi         |
|**Gizli dizi**     |Hizmet Müdür Adınızı Gizli Girin         |

Örnek:

![Tamamlanan HTTP etkinliği](./media/analysis-services-async-refresh-logic-app/7.png)

Şimdi Mantık Uygulaması test edin.  Mantık Uygulaması tasarımcısında **Çalıştır'ı**tıklatın.

![Mantıksal uygulamayı test etme](./media/analysis-services-async-refresh-logic-app/8.png)

## <a name="consume-the-logic-app-with-azure-data-factory"></a>Azure Veri Fabrikası ile Mantık Uygulamasını Tüketin

Mantık Uygulaması kaydedildikten sonra, **Bir HTTP isteği ne zaman etkinliği alınır** ve şimdi oluşturulan HTTP POST **URL'sini** kopyalayın.  Bu URL, Azure Veri Fabrikası tarafından Mantık Uygulamasını tetiklemek için eşzamanlı arama yapmak için kullanılabilecek URL'dir.

Aşağıda, bu eylemi yapan bir örnek Azure Veri Fabrikası Web Etkinliği verilmiştir.

![Veri Fabrikası Web Etkinliği](./media/analysis-services-async-refresh-logic-app/11.png)

## <a name="use-a-self-contained-logic-app"></a>Bağımsız Bir Mantık Uygulaması Kullanma

Model yenilemesini tetiklemek için Veri Fabrikası gibi bir Düzenleme aracı kullanmayı düşünmüyorsanız, zamanlamaya dayalı olarak yenilemeyi tetiklemek için mantık uygulamasını ayarlayabilirsiniz.

Yukarıdaki örneği kullanarak, ilk etkinliği silin ve **zamanlama** etkinliğiyle değiştirin.

![Etkinlik Programı](./media/analysis-services-async-refresh-logic-app/12.png)

![Etkinlik Programı](./media/analysis-services-async-refresh-logic-app/13.png)

Bu **örnekte Yineleme**kullanılacaktır.

Etkinlik eklendikten sonra, Aralık ve Sıklık'ı yapılandırın, ardından yeni bir parametre ekleyin ve **bu saatlerde**seçin.

![Etkinlik Programı](./media/analysis-services-async-refresh-logic-app/16.png)

Aranan saatleri seçin.

![Etkinlik Programı](./media/analysis-services-async-refresh-logic-app/15.png)

Mantık Uygulamasını Kaydet.

## <a name="next-steps"></a>Sonraki adımlar

[Örnekler](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
