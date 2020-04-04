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
ms.date: 04/02/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/28/2019
ms.openlocfilehash: 855a050afa14144f8963f24398c6b7b3939ef562
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656479"
---
# <a name="change-pricing-tier-of-an-azure-notification-hubs-namespace"></a>Azure bildirim hub'larının ad alanının fiyatlandırma katmanını değiştirme

Bildirim Hub'ları üç katmanda sunulur: **ücretsiz,** **temel**ve **standart.** Bu makalede, bir Azure Bildirim Hub'ları ad alanının fiyatlandırma katmanını nasıl değiştireceğiniz gösterilmektedir.

## <a name="overview"></a>Genel Bakış

Azure Bildirim Hub'larında hub en küçük kaynak/varlıktır. Genellikle tek bir uygulamayla eşler ve uygulama için desteklediğimiz her Platform Bildirim Sistemi (PNS) için bir sertifika tutabilir. Uygulama bir melez veya yerel ve çapraz platform uygulaması olabilir.

**Ad alanı,** bildirim hub'ları topluluğudur. Her ad alanı genellikle ilişkili ve belirli bir amaç için kullanılan hub'lardan oluşur. Örneğin, sırasıyla geliştirme, sınama ve üretim amaçları için üç farklı ad alanınız olabilir.

Bir ad alanını **ücretsiz,** **temel**veya **standart** fiyatlandırma katmanlarıyla ilişkilendirebilirsiniz. Katmanı gereksinimlerinize uygun bir ad alanı için kullanabilirsiniz. Aşağıdaki bölümler, Bildirim Hub'ları ad alanının fiyatlandırma katmanını nasıl değiştireceğinizi gösterir.

## <a name="use-azure-portal"></a>Azure portalı kullanma

Azure portalını kullanırken, ad alanı sayfasında veya hub sayfasında bir ad alanı için fiyatlandırma katmanını değiştirebilirsiniz. Hub sayfasında değiştirdiğinizde, aslında ad alanı düzeyinde değiştirirsiniz. Ad alanı nın fiyatlandırma katmanını ve ad alanındaki tüm hub'ları değiştirir.

### <a name="change-tier-on-the-namespace-page"></a>Ad alanı sayfasında katmanı değiştirme

Aşağıdaki yordam, ad alanı sayfasındaki bir ad alanının fiyatlandırma katmanının nasıl değiştirilebildiğini gösterir. Bir ad alanı için katmanı değiştirdiğinizde, bu ad alanındaki tüm hub'lar için geçerlidir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüdeki **Tüm hizmetleri** seçin.
3. **Nesnelerin İnterneti** bölümünde **Bildirim Hub'ı Ad Alanları'nı** seçin. Metnin yanındaki yıldız işaretini (`*`) seçerseniz, sık **kullanılanlar**altında sol gezinti çubuğuna eklenir. İleriye doğru ad alanları sayfasına daha hızlı erişmenize yardımcı olur. SıK Kullanılanlar'a ekledikten sonra **Bildirim Hub Ad Alanları'nı**seçin.

    ![Tüm hizmetler -> Bildirim Merkezi Ad Boşlukları](./media/change-pricing-tier/all-services-nhub.png)

4. Bildirim **Merkezi Ad Alanları** sayfasında, fiyatlandırma katmanını değiştirmek istediğiniz ad alanını seçin.
5. Ad alanınız için **Bildirim Hub Ad Alanı** sayfasında, **Essentials** bölümünde ad alanı için geçerli fiyatlandırma katmanını görebilirsiniz. Aşağıdaki resimde, ad alanının fiyatlandırma katmanının **Ücretsiz**olduğunu görebilirsiniz.

    ![Ad alanı sayfasındaki geçerli fiyatlandırma katmanı](./media/change-pricing-tier/pricing-tier-before.png)

6. Ad alanınız için **Bildirim Hub Ad Alanı** sayfasında **Yönet** bölümünde **Fiyatlandırma Katmanı'nı** seçin.

    ![Ad alanı sayfasında fiyatlandırma katmanını seçin](./media/change-pricing-tier/namespace-select-pricing-menu.png)

7. Fiyatlandırma katmanını değiştirin ve ardından **Seç** düğmesini tıklatın.
8. **Uyarılarda**katman değişikliği eyleminin durumunu görebilirsiniz.
9. **Genel Bakış** sayfasına geçin. Yeni katmanın **Essentials** **bölümündeki Fiyatlandırma Katmanı** alanı için gösterildiğini doğrulayın.
10. Bu adım isteğe bağlıdır. Ad alanında herhangi bir hub seçin. **Essentials** bölümünde aynı fiyatlandırma katmanını gördüğünüzden onaylayın. Ad alanındaki tüm hub'lar için aynı fiyatlandırma katmanını görmeniz gerekir.

### <a name="change-tier-on-the-hub-page"></a>Hub sayfasında katmanı değiştirme

Aşağıdaki yordam, hub sayfasındaki bir ad alanının fiyatlandırma katmanının nasıl değiştirilebildiğini gösterir. Hub sayfasından başlayarak bu adımları yapmanıza rağmen, ad alanı nın ve ad alanındaki tüm hub'ların fiyatlandırma katmanını gerçekten değiştirirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüdeki **Tüm hizmetleri** seçin.
3. **Nesnelerin İnterneti** bölümünde **Bildirim Hub'larını** seçin.
4. Bildirim **merkezinizi**seçin.
5. Sol menüde **Fiyatlandırma Katmanı'nı** seçin.
6. Fiyatlandırma katmanını değiştirin ve **Seç** düğmesini tıklatın. Bu eylem, hub'ı içeren ad alanı için fiyatlandırma katmanı ayarını değiştirir. Yani, ad alanı sayfasında ve tüm hub sayfalarında yeni fiyatlandırma katmanını görürsünüz.

> [!NOTE]
> Tüm fiyatlandırma katmanı değişiklikleri hemen geçerli olur.

## <a name="use-rest-api"></a>REST API’yi kullanma

Geçerli fiyatlandırma katmanını almak ve güncelleştirmek için aşağıdaki Kaynak Sağlayıcı REST API'lerini kullanabilirsiniz.

### <a name="get-current-pricing-tier-for-a-namespace"></a>Ad alanı için geçerli fiyatlandırma katmanını alın

Geçerli ad alanı katmanını almak için aşağıdaki örnekte gösterildiği gibi bir GET komutu gönderin:

```REST
GET: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
```

### <a name="update-pricing-tier-for-a-namespace"></a>Ad alanı için fiyatlandırma katmanını güncelleştirme

Ad alanı katmanını güncelleştirmek için, aşağıdaki örnekte gösterildiği gibi bir PUT komutu gönderin:

```REST
PUT: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
Body: <NotificationHubPlan xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"><SKU>Standard</SKU></NotificationHubPlan>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu katmanlar ve fiyatlandırma hakkında daha fazla bilgi için [Bildirim Hub'ları](https://azure.microsoft.com/pricing/details/notification-hubs/)fiyatlandırması'na bakın.
