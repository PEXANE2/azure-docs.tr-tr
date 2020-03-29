---
title: Bildirim Hub'ları ad alanının fiyatlandırma katmanını değiştirme | Microsoft Dokümanlar
description: Azure Bildirim Hub'ları ad alanının fiyatlandırma katmanını nasıl değiştireceğinizi öğrenin.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/28/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/28/2019
ms.openlocfilehash: 51d0880b7f56d523a01fbc993993b3caf7328134
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76261084"
---
# <a name="change-pricing-tier-of-an-azure-notification-hubs-namespace"></a>Azure bildirim hub'larının ad alanının fiyatlandırma katmanını değiştirme
Bildirim Hub'ları üç katmanda sunulur: **ücretsiz,** **temel**ve **standart.** Bu makalede, bir Azure Bildirim Hub'ları ad alanının fiyatlandırma katmanını nasıl değiştireceğiniz gösterilmektedir. 

## <a name="overview"></a>Genel Bakış
Azure Bildirim Hub'larında **hub** en küçük kaynak/varlıktır. Genellikle tek bir uygulamayla eşler ve uygulama için desteklediğimiz her Platform Bildirim Sistemi için bir sertifika tutabilir. Uygulama bir melez veya yerli ve çapraz platform uygulaması olabilir.

**Ad alanı,** bildirim hub'ları topluluğudur. Her ad alanı genellikle ilişkili ve belirli bir amaç için kullanılan hub'lardan oluşur. Örneğin, sırasıyla geliştirme, test ve üretim amaçları için üç farklı ad alanınız olabilir. 

Ad alanı düzeyinde bir fiyatlandırma katmanı ilişkilendirebilirsiniz. Bildirim Hub'ları üç katmanı destekler: **ücretsiz,** **temel**ve **standart.** Katmanı gereksinimlerinize uygun bir ad alanı için kullanabilirsiniz. Aşağıdaki bölümler, Bildirim Hub'ları ad alanının fiyatlandırma katmanını nasıl değiştireceğinizi gösterir. 

## <a name="use-azure-portal"></a>Azure portalı kullanma 
Azure portalını kullanırken, ad alanı sayfasındaveya hub sayfasında bir ad alanı için fiyatlandırma katmanını değiştirebilirsiniz.  Hub sayfasında değiştirdiğinizde, aslında ad alanı düzeyinde değiştirirsiniz. Ad alanı nın fiyatlandırma katmanını ve ad alanındaki tüm hub'ları değiştirir. 

### <a name="change-tier-on-the-namespace-page"></a>Ad alanı sayfasında katmanı değiştirme
Aşağıdaki yordam, ad alanı sayfasındaki bir ad alanının fiyatlandırma katmanını değiştirmeniz için adımlar verir. Bir ad alanı için katmanı değiştirdiğinizde, ad alanındaki tüm hub'lar için geçerlidir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüdeki **Tüm hizmetleri** seçin. 
3. **Nesnelerin İnterneti** bölümünde **Bildirim Hub'ı Ad Alanları'nı** seçin. Metnin yanındaki`*`yıldız ( ) seçeneğini belirlerseniz, **sık kullanılanlar**altında sol gezinti çubuğuna eklenir. Bir sonraki sefere daha hızlı erişmenize yardımcı olur. SıK Kullanılanlar'a ekledikten sonra **Bildirim Hub Ad Alanları'nı**seçin. 

    ![Tüm hizmetler -> Bildirim Merkezi Ad Boşlukları](./media/change-pricing-tier/all-services-nhub.png)
1. Bildirim **Merkezi Ad Alanları** sayfasında, fiyatlandırma katmanını değiştirmek istediğiniz ad alanını seçin. 
2. Ad alanınız için **Bildirim Hub Ad Alanı** sayfasında, **Essentials** bölümünde ad alanı için geçerli fiyatlandırma katmanını görebilirsiniz. Aşağıdaki resimde, ad alanının fiyatlandırma katmanının **Ücretsiz**olduğunu görebilirsiniz. 

    ![Ad alanı sayfasındaki geçerli fiyatlandırma katmanı](./media/change-pricing-tier/pricing-tier-before.png)
1. Ad alanınız için **Bildirim Hub Ad Alanı** sayfasında **Yönet** bölümünde **Fiyatlandırma Katmanı'nı** seçin. 

    ![Ad alanı sayfasında fiyatlandırma katmanını seçin](./media/change-pricing-tier/namespace-select-pricing-menu.png)
6. Fiyatlandırma katmanınızı değiştirin ve **Seç** düğmesini tıklatın.    
7. Uyarılarda katman değişikliği eyleminin durumunu **görürsünüz.** 
8. **Genel Bakış** sayfasına geçin. Yeni katmanın **Essentials** **bölümündeki Fiyatlandırma Katmanı** alanı için gösterildiğini doğrulayın.     
1. Bu adım isteğe bağlıdır. Ad alanında herhangi bir hub seçin. **Essentials** bölümünde aynı fiyatlandırma katmanını gördüğünüzden onaylayın. Ad alanındaki tüm hub'lar için aynı fiyatlandırma katmanını görmeniz gerekir. 

### <a name="change-tier-on-the-hub-page"></a>Hub sayfasında katmanı değiştirme
Aşağıdaki yordam, hub sayfasındaki bir ad alanının fiyatlandırma katmanını değiştirmeniz için adımlar verir. Hub sayfasından başlayarak bu adımları yapmanıza rağmen, ad alanı nın ve ad alanındaki tüm hub'ların fiyatlandırma katmanını gerçekten değiştirirsiniz. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüdeki **Tüm hizmetleri** seçin.
3. **Nesnelerin İnterneti** bölümünde **Bildirim Hub'larını** seçin. 
4. Bildirim **merkezinizi**seçin. 
5. Sol menüde **Fiyatlandırma Katmanı'nı** seçin. 
6. Fiyatlandırma katmanını değiştirin ve **Seç** düğmesini tıklatın. Bu eylem, hub'ı içeren ad alanı için fiyatlandırma katmanı ayarını değiştirir. Yani, ad alanı sayfasında ve tüm hub sayfalarında yeni fiyatlandırma katmanını görürsünüz. 

## <a name="use-rest-api"></a>REST API’yi kullanma
Geçerli fiyatlandırma katmanını almak ve güncelleştirmek için aşağıdaki Kaynak Sağlayıcı REST API'lerini kullanabilirsiniz. 

### <a name="get-current-pricing-tier-for-a-namespace"></a>Ad alanı için geçerli fiyatlandırma katmanını alın
**Geçerli ad alanı katmanını**almak için aşağıdaki örnekte gösterildiği gibi GET komutu gönderin: 

```REST
GET: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
```

### <a name="update-pricing-tier-for-a-namespace"></a>Ad alanı için fiyatlandırma katmanını güncelleştirme
**Ad alanı katmanını güncelleştirmek**için, aşağıdaki örnekte gösterildiği gibi bir PUT komutu gönderin: 

```REST
PUT: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
Body: <NotificationHubPlan xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"><SKU>Standard</SKU></NotificationHubPlan>
```



## <a name="next-steps"></a>Sonraki adımlar
Bu katmanlar ve fiyatlandırma hakkında daha fazla bilgi için [Bildirim Hub'ları](https://azure.microsoft.com/pricing/details/notification-hubs/)fiyatlandırması'na bakın.
