---
title: Azure portal IoT Central Yönet | Microsoft Docs
description: Bu makalede, Azure portal IoT Central uygulamalarınızın nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 10/02/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 1e206d9a1b14fc8ff1b92e4eb7217824d81212cf
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990186"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Azure portal IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamaları oluşturup yönetmek yerine uygulamalarınızı yönetmek için [Azure Portal](https://portal.azure.com) kullanabilirsiniz.

## <a name="create-iot-central-applications"></a>IoT Central uygulamaları oluşturma

Bir uygulama oluşturmak için [Azure Portal](https://ms.portal.azure.com) gidin ve soldaki ana bölmede **kaynak oluştur** ' u seçin.

![Yönetim Portalı: gezinti menüsü](media/howto-manage-iot-central-from-portal/image0.png)

Arama çubuğuna **IoT Central**yazın.

![Yönetim Portalı: arama](media/howto-manage-iot-central-from-portal/image0a1.png)

Arama sonuçlarındaki **uygulama satırı IoT Central** öğesini seçin.

![Yönetim Portalı: arama sonuçları](media/howto-manage-iot-central-from-portal/image0b1.png)

Şimdi **Oluştur**' u seçin.

![Yönetim Portalı: IoT Central kaynağı](media/howto-manage-iot-central-from-portal/image0c1.png)

Formdaki tüm alanları doldurur. Bu form, [Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesinde uygulama oluşturmak için doldurduğunuz forma benzer. Daha fazla bilgi için [IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcı bölümüne bakın.

Uygulama şablonu olarak **eski uygulama** ' yı seçerek genel kullanıma sunulan özelliklerle IoT Central uygulama oluşturabilirsiniz, diğer tüm uygulama şablonları genel önizleme özelliklerini kullanır.

![IoT Central form oluştur](media/howto-manage-iot-central-from-portal/image6a.png)

**Konum** , uygulamanızı oluşturmak istediğiniz [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) ' dır. Genellikle en iyi performansı elde etmek için cihazlarınıza fiziksel olarak en yakın konumu seçmeniz gerekir. Azure IoT Central Şu anda **Birleşik Devletler**, **Avustralya**, **Asya Pasifik**veya **Avrupa**'da sunulmaktadır.  Bir konum seçtikten sonra, uygulamanızı daha sonra farklı bir konuma taşıyamazsınız.

> [!NOTE]
> Önizleme uygulaması şablonları Şu anda yalnızca **Avrupa** ve **Birleşik Devletler** konumlarda kullanılabilir.


Tüm alanları doldurduktan sonra **Oluştur**' u seçin.

## <a name="manage-existing-iot-central-applications"></a>Mevcut IoT Central uygulamalarını yönetme

Zaten bir Azure IoT Central uygulamanız varsa, bunu silebilir veya Azure portal farklı bir aboneliğe veya kaynak grubuna taşıyabilirsiniz.

> [!NOTE]
> Aboneliğiniz ile ilişkilendirilmediğinden, Azure portal ücretsiz fiyatlandırma planında oluşturulan uygulamaları göremezsiniz.

Başlamak için sol taraftaki ana bölmedeki **tüm kaynaklar** ' ı seçin. Kaynak listenizde bulmak için uygulamanızın adını yazmak üzere arama kutusunu kullanın. Ardından, yönetmek istediğiniz IoT Central uygulamayı seçin.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image2a.png)

Uygulamaya gitmek için IoT Central uygulama URL 'sini seçin.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image3.png)

Uygulamayı farklı bir kaynak grubuna taşımak için kaynak grubunun yanındaki **Değiştir** ' i seçin. **Kaynakları taşı** sayfasında, bu uygulamayı geçirmek istediğiniz kaynak grubunu seçin.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image4a.png)

Uygulamayı farklı bir aboneliğe taşımak için aboneliğin yanındaki **değişiklik** bağlantısını seçin. Görüntülenen iletişim kutusunda bu uygulamayı geçirmek istediğiniz aboneliği seçin.

![Yönetim Portalı: kaynak yönetimi](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamalarını Azure portal nasıl yönetebileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)