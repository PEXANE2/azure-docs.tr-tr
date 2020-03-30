---
title: Bağlı Fabrika panosunu kullanma - Azure | Microsoft Dokümanlar
description: Bu makalede, endüstriyel IoT aygıtlarınızı izlemek ve yönetmek için Bağlı Fabrika panosunun özelliklerinin nasıl kullanılacağı açıklanmaktadır.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: dobett
ms.openlocfilehash: b53177d578768428665891704269e63bd8edb09e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820169"
---
# <a name="use-features-in-the-connected-factory-solution-accelerator-dashboard"></a>Bağlı Fabrika çözüm hızlandırıcı panosundaki özellikleri kullanma

[Endüstriyel IoT aygıtlarımın hızlı başlatılmasını yönetmek için bulut tabanlı bir çözüm dağıt,](quickstart-connected-factory-deploy.md) panoda nasıl gezindiğinizi ve alarmlara nasıl yanıt verilebildiğinizi gösterdi. Bu nasıl yapılabilir kılavuzu, endüstriyel IoT aygıtlarınızı izlemek ve yönetmek için kullanabileceğiniz bazı ek pano özelliklerini gösterir.

## <a name="apply-filters"></a>Filtreleri uygulama

Panoda görüntülenen bilgileri **Fabrika Konumları** panelinde veya **Alarmlar** panelinde filtreleyebilirsiniz:

1. Fabrika konumları veya alarmlar panelinde kullanılabilir filtrelerin listesini görüntülemek için **huni** simgesine tıklayın.

1. Filtreler paneli görüntülenir:

    [![Bağlı Fabrika çözüm hızlandırıcısı filtreleri](./media/iot-accelerators-connected-factory-dashboard/filterpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterpanel-expanded.png#lightbox)

1. Gereksinim duyduğunuz filtreyi seçin ve **Uygula'yı**tıklatın. Filtre alanlarına boş metin yazmak da mümkündür.

1. Filtre daha sonra uygulanır. Ek huni simgesi bir filtrenin uygulandığını gösterir:

    [![Bağlı Fabrika çözüm hızlandırıcı filtresi uygulandı](./media/iot-accelerators-connected-factory-dashboard/filterapplied-inline.png)](./media/iot-accelerators-connected-factory-dashboard/filterapplied-expanded.png#lightbox)

    > [!NOTE]
    > Etkin bir filtre görüntülenen OEE ve KPI değerlerini etkilemez, yalnızca liste içeriğini filtreler.

1. Filtreyi temizlemek için huniyi tıklatın ve filtre panelinde **Temizle'yi** tıklatın.

## <a name="browse-an-opc-ua-server"></a>OPC UA sunucusuna göz atma

Çözüm hızlandırıcısını dağıttığınızda, panodan göz atabileceğiniz bir dizi simüle edilmiş OPC UA sunucusunu otomatik olarak sağlarsınız. Simüle edilmiş sunucular, gerçek sunucuları dağıtmaya gerek kalmadan çözüm hızlandırıcısını denemenizi kolaylaştırır.

1. Pano gezinti çubuğundaki **tarayıcı simgesini** tıklatın:

    [![Bağlı Fabrika çözüm hızlandırıcısı sunucu tarayıcısı](./media/iot-accelerators-connected-factory-dashboard/browser-inline.png)](./media/iot-accelerators-connected-factory-dashboard/browser-expanded.png#lightbox)

1. Çözüm hızlandırıcısında sizin için dağıtılan sunucuları gösteren listeden sunuculardan birini seçin:

    [![Bağlı Fabrika çözüm hızlandırıcı sunucu listesi](./media/iot-accelerators-connected-factory-dashboard/serverlist-inline.png)](./media/iot-accelerators-connected-factory-dashboard/serverlist-expanded.png#lightbox)

1. **Bağlan**’a tıklayın; güvenlik iletişim kutusu görüntülenir. Simülasyon için **Devam**et'i tıklatmak güvenlidir.

1. Sunucu ağacındaki düğümlerden herhangi birini genişletmek için düğüme tıklayın. Telemetri yayımlayan düğümlerin yanlarında bir onay işareti vardır:

    [![Bağlı Fabrika çözüm hızlandırıcısı sunucu ağacı](./media/iot-accelerators-connected-factory-dashboard/servertree-inline.png)](./media/iot-accelerators-connected-factory-dashboard/servertree-expanded.png#lightbox)

1. Düğümü okumak, yazmak, yayımlamak veya çağırmak için bir öğeye sağ tıklayın. Kullanabileceğiniz eylemler, izinlerinize ve düğümün özniteliklerine bağlıdır. Okuma seçeneği belirli bir düğümün değerini gösteren bağlam panelini görüntüler. Yazma seçeneği yeni değer girebileceğiniz bir bağlam paneli görüntüler. Çağırma seçeneği çağrı için parametreleri girebileceğiniz bir düğüm görüntüler.

## <a name="publish-a-node"></a>Düğümü yayımlama

*Sanal OPC UA sunucusuna* göz attığınızda, yeni düğümleri yayımlamayı da seçebilirsiniz. Çözümde bu düğümlerden gelen telemetriyi analiz edebilirsiniz. Bu *simüle edilmiş OPC UA sunucuları,* gerçek aygıtları dağıtmadan çözüm hızlandırıcısını denemeyi kolaylaştırır:

1. OPC UA sunucusu tarayıcı ağacında yayımlamak istediğiniz düğüme göz atın.

1. Düğüme sağ tıklayın. Tıklayın **Yayımla**:

    [![Bağlı Fabrika çözüm hızlandırıcı yayın düğümü](./media/iot-accelerators-connected-factory-dashboard/publishnode-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishnode-expanded.png#lightbox)

1. Yayımlama işleminin başarılı olduğunu bildiren bir bağlam paneli görüntülenir. Düğüm, istasyon düzeyindeki görünümde yanında bir onay işareti yle görünür:

    [![Bağlı Fabrika çözüm hızlandırıcı başarı yayınlamak](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/publishsuccess-expanded.png#lightbox)

## <a name="command-and-control"></a>Komut ve denetim

Bağlı Fabrika, endüstri cihazlarınızı doğrudan buluttan kumanda etmenize ve denetlemenize olanak tanır. Cihaz tarafından oluşturulan alarmları yanıtlamak için bu özelliği kullanabilirsiniz. Örneğin, bir basınç tahliye valfi açmak için cihaza bir komut gönderebilirsiniz. Kullanılabilir komutları, OPC UA sunucuları tarayıcı ağacındaki **StationCommands** düğümünde bulabilirsiniz. Bu senaryoda, Münih’teki üretim hattının montaj istasyonundaki basınç tahliye vanasını açıyorsunuz. Komut ve denetim işlevini kullanmak için çözüm hızlandırıcı dağıtımı için **Yönetici** rolünde olmalısınız:

1. Münih, üretim hattı 0, montaj istasyonu için OPC UA tarayıcı ağacında **StationCommands** düğümüne göz atın.

1. Kullanmak istediğiniz komutu seçin. **OpenPressureReleaseValve** düğümüne sağ tıklayın. **Çağrı'yı**tıklatın :

    [![Bağlı Fabrika çözüm hızlandırıcısı çağrı komutu](./media/iot-accelerators-connected-factory-dashboard/callcommand-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callcommand-expanded.png#lightbox)

1. Hangi yöntemi ve parametre ayrıntılarını arayacağınızı bildiren bir bağlam paneli görünür. **Çağrı'yı**tıklatın :

    [![Bağlı Fabrika çözüm hızlandırıcı çağrı parametreleri](./media/iot-accelerators-connected-factory-dashboard/callpanel-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callpanel-expanded.png#lightbox)

1. Bağlam paneli, yöntem çağrısının başarılı olduğunu bildirecek şekilde güncelleştirilir. Çağrı sonucu güncelleştirilen basınç düğümünün değerini okuyarak çağrının başarılı olduğunu doğrulayabilirsiniz.

    [![Bağlı Fabrika çözüm hızlandırıcısı çağrı başarısı](./media/iot-accelerators-connected-factory-dashboard/callsuccess-inline.png)](./media/iot-accelerators-connected-factory-dashboard/callsuccess-expanded.png#lightbox)

## <a name="behind-the-scenes"></a>Arka planda

Bir çözüm hızlandırıcısını dağıttığınızda, dağıtım işlemi seçtiğiniz Azure aboneliğinde birden çok kaynak oluşturur. Bu kaynakları Azure [portalında](https://portal.azure.com) görüntüleyebilirsiniz. Dağıtım işlemi, çözüm hızlandırıcınız için seçtiğiniz adı temel alan bir ada sahip bir **kaynak grubu** oluşturur:

[![Bağlı Fabrika çözüm hızlandırıcı kaynak grubu](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-inline.png)](./media/iot-accelerators-connected-factory-dashboard/resourcegroup-expanded.png#lightbox)

Her bir kaynağın ayarlarını, kaynak grubundaki kaynaklar listesinde seçerek görüntüleyebilirsiniz.

Ayrıca, [azure-iot'a bağlı fabrika](https://github.com/Azure/azure-iot-connected-factory) GitHub deposunda çözüm hızlandırıcısının kaynak kodunu da görüntüleyebilirsiniz.

Işiniz bittiğinde, [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard) sitesindeki Azure aboneliğinizden çözüm hızlandırıcısını silebilirsiniz. Bu site, çözüm hızlandırıcısını oluşturduğunuzda sağlanan tüm kaynakları kolayca silmenize imkan tanır.

> [!NOTE]
> Çözüm hızlandırıcıyla ilgili her şeyi sildiğinizden emin olmak [için, azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators#dashboard) sitesinde silin. Portalda kaynak grubunu silmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Çalışan bir çözüm hızlandırıcısı dağıttığınıza göre aşağıdaki makaleleri okuyarak IoT çözüm hızlandırıcılarıyla çalışmaya başlayabilirsiniz:

* [Bağlı Fabrika çözüm hızlandırıcısını yapılandırın](iot-accelerators-connected-factory-configure.md)
* [azureiotsolutions.com sitesindeki izinler](iot-accelerators-permissions.md)
