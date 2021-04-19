---
title: Azure portal IoT Central Yönet | Microsoft Docs
description: Bu makalede, Azure portal IoT Central uygulamalarınızın nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.
services: iot-central
ms.service: iot-central
author: vishwam
ms.author: vishwams
ms.date: 04/17/2021
ms.topic: how-to
manager: philmea
ms.openlocfilehash: ed65e85c7428bf59fe770534e97afdd53564086a
ms.sourcegitcommit: 089c2bd1ac4861f43c4b89396d3d056a6eef4913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2021
ms.locfileid: "107601965"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Azure portal IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

IoT Central [Uygulama Yöneticisi](https://apps.azureiotcentral.com/myapps)işlevselliğine benzer IoT Central uygulamalar oluşturmak ve yönetmek için [Azure Portal](https://portal.azure.com) kullanabilirsiniz.

## <a name="create-iot-central-applications"></a>IoT Central uygulamaları oluşturma

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]

Bir uygulama oluşturmak için, Azure portal [IoT Central uygulama oluştur](https://ms.portal.azure.com/#create/Microsoft.IoTCentral) sayfasına gidin ve formu doldurmanız gerekir.

![IoT Central form oluştur](media/howto-manage-iot-central-from-portal/image6a.png)

* **Kaynak adı** , Azure kaynak grubunuzda IoT Central uygulamanız için seçebileceğiniz benzersiz bir addır.

* **Uygulama URL** 'si, uygulamanıza erişmek IÇIN kullanabileceğiniz URL 'dir.

* **Konum** , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Azure IoT Central Şu anda aşağıdaki konumlarda sunulmaktadır:
    * Asya Pasifik
    * Avustralya
    * Avrupa
    * Japonya
    * Birleşik Krallık
    * Birleşik Devletler

  Bir konum seçtikten sonra, uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.

Tüm alanları doldurduktan sonra **Oluştur**' u seçin. Daha fazla bilgi için [IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcı bölümüne bakın.

## <a name="manage-existing-iot-central-applications"></a>Mevcut IoT Central uygulamalarını yönetme

Zaten bir Azure IoT Central uygulamanız varsa, bunu silebilir veya Azure portal farklı bir aboneliğe veya kaynak grubuna taşıyabilirsiniz.

> [!NOTE]
> *Ücretsiz* plan kullanılarak oluşturulan uygulamalar, Azure abonelikleri gerektirmez ve bu nedenle, bunları Azure Portal Azure aboneliğinizdeki listede bulamayamayacağız. IoT Central portalından yalnızca ücretsiz uygulamaları görebilir ve yönetebilirsiniz.

Başlamak için Azure portal üst kısmındaki arama çubuğunda uygulamanızı arayın. Tüm uygulamalarınızı, "IoT Central uygulamaları" arayarak ve hizmeti seçerek de görüntüleyebilirsiniz:

![Seçilen ilk hizmetle "IoT Central uygulamaları" için arama sonuçlarını gösteren ekran görüntüsü.](media/howto-manage-iot-central-from-portal/search-iot-central.png)

Arama sonuçlarında bir uygulama seçtikten sonra Azure portal, genel görünümünü gösterir. **IoT Central uygulama URL 'sini** seçerek gerçek uygulamaya gidebilirsiniz:

!["IoT Central uygulama URL 'SI" vurgulanmış "genel bakış" sayfasını gösteren ekran görüntüsü.](media/howto-manage-iot-central-from-portal/image3.png)

Uygulamayı farklı bir kaynak grubuna taşımak için kaynak grubunun yanındaki **Değiştir** ' i seçin. **Kaynakları taşı** sayfasında, bu uygulamayı taşımak istediğiniz kaynak grubunu seçin:

!["Genel bakış" sayfasını "kaynak grubu (değişiklik)" vurgulanmış şekilde gösteren ekran görüntüsü.](media/howto-manage-iot-central-from-portal/image4a.png)

Uygulamayı farklı bir aboneliğe taşımak için, aboneliğin yanındaki  **Değiştir** ' i seçin. **Kaynakları taşı** sayfasında, bu uygulamayı taşımak istediğiniz aboneliği seçin:

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamalarını Azure portal nasıl yönetebileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)