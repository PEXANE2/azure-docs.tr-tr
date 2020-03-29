---
title: Bağlı Fabrika çözümlerini özelleştirin - Azure | Microsoft Dokümanlar
description: Bağlı Fabrika çözüm hızlandırıcısının davranışını nasıl özelleştirin.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: csharp
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: 6062f8b3992732e0e0f9bbdae9549e69c393f4ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67080481"
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Bağlı Fabrika çözümünüzün OPC UA sunucularınızdaki verileri nasıl görüntülenebildiğini özelleştirme

Bağlı Fabrika çözümü, çözüme bağlı OPC UA sunucularından gelen verileri toplar ve görüntüler. Çözümünüzde OPC UA sunucularına göz atabilir ve komutgönderebilirsiniz. OPC UA hakkında daha fazla bilgi için bkz. [Bağlı Fabrika SSS](iot-accelerators-faq-cf.md).

Çözümdeki toplu verilere örnek olarak, fabrika, hat ve istasyon düzeylerindeki panoda görüntüleyebileceğiniz Genel Ekipman Verimliliği (OEE) ve Anahtar Performans Göstergeleri (KP'ler) verilebilir. Aşağıdaki ekran görüntüsü, **Münih** fabrikasında **üretim hattı 1,** **Montaj** istasyonu için OEE ve KPI değerlerini gösterir:

![Çözümdeki OEE ve KPI değerleri örneği][img-oee-kpi]

Çözüm, *İstasyon*adı verilen OPC UA sunucularından belirli veri öğelerinden ayrıntılı bilgileri görüntülemenizi sağlar. Aşağıdaki ekran görüntüsü, belirli bir istasyondan üretilen öğe sayısının çizimlerini gösterir:

![Üretilen ürün sayısının çizimleri][img-manufactured-items]

Grafiklerden birini tıklattığınızda, Zaman Serisi Öngörüleri 'ni (TSI) kullanarak verileri daha fazla keşfedebilirsiniz:

![Time Series Insights'ı kullanarak verileri keşfedin][img-tsi]

Bu makalede açıklanır:

- Verilerin çözümdeki çeşitli görünümler için nasıl kullanılabilir hale getirilir.
- Çözümün verileri görüntüleme şeklini nasıl özelleştirebileceğiniz.

## <a name="data-sources"></a>Veri kaynakları

Bağlı Fabrika çözümü, çözüme bağlı OPC UA sunucularından gelen verileri görüntüler. Varsayılan yükleme, fabrika simülasyonu çalıştıran birkaç OPC UA sunucusu içerir. Çözümünüze [ağ geçidi][lnk-connect-cf] üzerinden bağlanan kendi OPC UA sunucularınızı ekleyebilirsiniz.

Bağlı bir OPC UA sunucusunun panodaki çözümünüze gönderebileceği veri öğelerine göz atabilirsiniz:

1. **OPC UA sunucu** görünümüne gitmek için **Tarayıcı'yı** seçin:

    ![OPC UA sunucu görünümü seç'e gidin][img-select-server]

1. Bir sunucu seçin ve **Bağlan'ı**tıklatın. Güvenlik uyarısı göründüğünde **Devam et'i** tıklatın.

    > [!NOTE]
    > Bu uyarı, her sunucu için yalnızca bir kez görünür ve çözüm panosu ile sunucu arasında bir güven ilişkisi kurar.

1. Artık sunucunun çözüme gönderebileceği veri öğelerine göz atabilirsiniz. Çözüme gönderilen öğelerin onay işareti vardır:

    ![Yayımlanmış öğeler][img-published]

1. Çözümde *Yöneticiyseniz,* Bağlı Fabrika çözümünde kullanılabilir hale getirmek için bir veri öğesi yayımlamayı seçebilirsiniz. Yönetici olarak, OPC UA sunucusundaki veri öğelerinin ve arama yöntemlerinin değerini de değiştirebilirsiniz.

## <a name="map-the-data"></a>Verileri haritala

Bağlı Fabrika çözümü, yayınlanan veri öğelerini OPC UA sunucusundan çözümdeki çeşitli görünümlere eşler ve toplar. Bağlı Fabrika çözümü, çözümü sağlarken Azure hesabınıza dağıtılır. Visual Studio Connected Factory çözümündeki bir JSON dosyası bu haritalama bilgilerini depolar. Bağlı Fabrika Visual Studio çözümünde bu JSON yapılandırma dosyasını görüntüleyebilir ve değiştirebilirsiniz. Değişiklik yaptıktan sonra çözümü yeniden dağıtabilirsiniz.

Yapılandırma dosyasını şu şekilde kullanabilirsiniz:

- Mevcut simüle edilmiş fabrikaları, üretim hatlarını ve istasyonları edin.
- Çözüme bağlandığınız gerçek OPC UA sunucularından gelen verileri haritalayın.

Belirli gereksinimlerinizi karşılamak için verileri eşleme ve toplama hakkında daha fazla bilgi için [Bağlı Fabrika çözüm hızlandırıcısını nasıl yapılandırılabilirsiniz. ](iot-accelerators-connected-factory-configure.md)

## <a name="deploy-the-changes"></a>Değişiklikleri dağıtma

**ContosoTopologyDescription.json** dosyasında değişiklik yapmayı bitirdikten sonra, Bağlı Fabrika çözümlerini Azure hesabınıza yeniden dağıtmanız gerekir.

**Azure-iot'a bağlı fabrika** deposu, çözümü yeniden oluşturmak ve dağıtmak için kullanabileceğiniz bir **build.ps1** PowerShell komut dosyası içerir.

## <a name="next-steps"></a>Sonraki Adımlar

Aşağıdaki makaleleri okuyarak Bağlı Fabrika çözüm hızlandırıcısı hakkında daha fazla bilgi edinin:

* [azureiotsolutions.com sitesindeki izinler][lnk-permissions]
* [Bağlı Fabrika hakkında SSS](iot-accelerators-faq-cf.md)
* [SSS][lnk-faq]


[img-oee-kpi]: ./media/iot-accelerators-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-accelerators-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-accelerators-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-accelerators-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-accelerators-connected-factory-customize/published.png


[lnk-permissions]: iot-accelerators-permissions.md
[lnk-faq]: iot-accelerators-faq.md