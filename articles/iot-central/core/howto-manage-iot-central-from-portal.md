---
title: Azure portalından IoT Central'ı yönetin | Microsoft Dokümanlar
description: Bu makalede, Azure portalından IoT Merkezi uygulamalarınızın nasıl oluşturulup yönetilen anlatılmaktadır.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: e7efda8efa27044168386e3ebbc557bf7fb74e8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157934"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Azure portalından IoT Central'ı yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamalarını oluşturmak ve yönetmek yerine, uygulamalarınızı yönetmek için Azure [portalını](https://portal.azure.com) kullanabilirsiniz.

## <a name="create-iot-central-applications"></a>IoT Merkezi uygulamaları oluşturma

Bir uygulama oluşturmak için [Azure portalına](https://ms.portal.azure.com) gidin ve **kaynak oluştur'u**seçin.

**Market çubuğunu** Ara'da, *IoT Central*yazın:

![Yönetim portalı: arama](media/howto-manage-iot-central-from-portal/image0a1.png)

Arama sonuçlarında **IoT Merkezi Uygulama** döşemesini seçin:

![Yönetim Portalı: arama sonuçları](media/howto-manage-iot-central-from-portal/image0b1.png)

Şimdi, **Oluştur'u**seçin:

![Yönetim portalı: IoT Merkezi kaynak](media/howto-manage-iot-central-from-portal/image0c1.png)

Formdaki tüm alanları doldurun. Bu form, [Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde uygulama oluşturmak için doldurduğunuz forma benzer. Daha fazla bilgi [için, Bir IoT Merkezi uygulama](quick-deploy-iot-central.md) hızlı başlat Oluştur'a bakın.

![IoT Merkezi form oluşturma](media/howto-manage-iot-central-from-portal/image6a.png)

**Konum,** uygulamanızı oluşturmak istediğiniz [coğrafyadır.](https://azure.microsoft.com/global-infrastructure/geographies/) Genellikle, en iyi performansı elde etmek için fiziksel olarak aygıtlarınıza en yakın konumu seçmeniz gerekir. Azure IoT Central şu anda **Avustralya,** **Asya Pasifik,** **Avrupa,** **Amerika Birleşik Devletleri,** **Birleşik Krallık**ve **Japonya** coğrafyalarında mevcuttur. Bir konum seçtikten sonra, uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.

Tüm alanları doldurduktan sonra **Oluştur'u**seçin.

## <a name="manage-existing-iot-central-applications"></a>Mevcut IoT Merkezi uygulamalarını yönetme

Zaten bir Azure IoT Merkezi uygulamanız varsa uygulamayı silebilir veya Azure portalında farklı bir abonelik veya kaynak grubuna taşıyabilirsiniz.

> [!NOTE]
> Azure portalında ücretsiz fiyatlandırma planında oluşturulan uygulamaları göremezsiniz, çünkü bunlar aboneliğinizle ilişkili değildir.

Başlamak için portaldaki **tüm kaynakları** seçin. **Gizli türleri göster'i** seçin ve bulmak için uygulamanızın adını **Filtre'ye göre** yazmaya başlayın. Ardından yönetmek istediğiniz IoT Central uygulamasını seçin.

Uygulamaya gitmek için **IoT Merkezi Uygulama URL'sini**seçin:

![Yönetim portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image3.png)

Uygulamayı farklı bir kaynak grubuna taşımak için kaynak grubunun yanında **değiştir'i** seçin. Kaynakları **Taşı** sayfasında, bu uygulamayı şu şekilde taşımak istediğiniz kaynak grubunu seçin:

![Yönetim portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image4a.png)

Uygulamayı farklı bir aboneliğe taşımak için aboneliğin yanında **değiştir'i** seçin. Kaynakları **Taşı** sayfasında, bu uygulamayı şu şekilde taşımak istediğiniz aboneliği seçin:

![Yönetim portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure portalından Azure IoT Merkezi uygulamalarını nasıl yönetdiğinizi öğrendiğiniz için, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)