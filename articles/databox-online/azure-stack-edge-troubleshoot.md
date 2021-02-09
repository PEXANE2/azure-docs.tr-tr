---
title: Azure Stack Edge Pro sorunlarını gidermek için Azure portal kullanma | Microsoft Docs
description: Azure Stack Edge Pro 'unuzda sorunları nasıl giderebileceğinizi öğrenin. Tanılama çalıştırabilir, destek için bilgi toplayabilir ve günlükleri kullanarak sorun giderme yapabilirsiniz.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 02/05/2021
ms.author: alkohli
ms.openlocfilehash: ff6d1f46299875354a72a6a93a828df7960169ad
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99831555"
---
# <a name="troubleshoot-your-azure-stack-edge-pro-issues"></a>Azure Stack Edge Pro sorunlarınızı giderme

Bu makalede Azure Stack Edge Pro ile ilgili sorunların nasıl giderileceği açıklanmaktadır. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Tanılama çalıştırma
> * Destek paketi toplama
> * Sorun gidermek için günlükleri kullanma
> * IoT Edge hatalarında sorun giderme

## <a name="run-diagnostics"></a>Tanılama çalıştırma

Cihaz hatalarını tanılamak ve gidermek için tanılama testlerini çalıştırabilirsiniz. Tanılama testlerini çalıştırmak için cihazınızın yerel web arabiriminde aşağıdaki adımları gerçekleştirin.

1. Yerel web arabiriminde **Sorun giderme > Tanılama testleri** sayfasına gidin. Çalıştırmak istediğiniz testi seçin ve **Test Çalıştır**' ı seçin. Bunu yaptığınızda ağ, cihaz, web proxy, saat veya bulut ayarlarınızdaki sorunların tanılanmasına yönelik testler çalıştırılır. Cihazın testleri çalıştırdığı bildirilir.

    ![Testleri seçin](media/azure-stack-edge-troubleshoot/run-diag-1.png)

2. Testler tamamlandıktan sonra sonuçlar görüntülenir.

    ![Test sonuçlarını gözden geçirme](media/azure-stack-edge-troubleshoot/run-diag-2.png)

    Test başarısız olursa önerilen eylem URL'si gösterilir. Önerilen eylemi görüntülemek için URL 'YI seçin.

    ![Başarısız testler için uyarıları gözden geçirin](media/azure-stack-edge-troubleshoot/run-diag-3.png)

## <a name="collect-support-package"></a>Destek paketi toplama

Günlük paketi, Microsoft Desteğinin cihaz sorunlarını giderme konusunda yardımcı olabilmesi için ihtiyaç duyabileceği günlüklerin tamamını içerir. Günlük paketi oluşturmak için yerel web arabirimini kullanabilirsiniz.

Destek paketi toplamak için aşağıdaki adımları gerçekleştirin.

1. Yerel web arabiriminde **Sorun giderme > Destek** sayfasına gidin. **Destek paketi oluştur**' u seçin. Sistem, destek paketini toplamaya başlar. Paketin toplanması birkaç dakika sürebilir.

    ![Kullanıcı ekle'ye tıklayın](media/azure-stack-edge-troubleshoot/collect-logs-1.png)

2. Destek paketi oluşturulduktan sonra **destek paketini indir**' i seçin. Seçtiğiniz yola .zip uzantılı bir paket indirilir. Paketi açıp sistem günlüğü dosyalarını görüntüleyebilirsiniz.

    ![Kullanıcı Ekle 2 ' ye tıklayın](media/azure-stack-edge-troubleshoot/collect-logs-2.png)

## <a name="use-logs-to-troubleshoot"></a>Sorun gidermek için günlükleri kullanma

Yükleme ve yenileme işlemlerinde karşılaşılan hatalar ilgili hata dosyalarına yazılır.

1. Hata dosyalarını görüntülemek için paylaşımınıza gidin ve içeriği görüntülemek için seçin. 

      ![Paylaşılan içeriğe bağlanma ve içeriği görüntüleme](media/azure-stack-edge-troubleshoot/troubleshoot-logs-1.png)

2. _Microsoft Azure Stack Edge Pro klasörünü_ seçin. Bu klasör iki alt klasör içerir:

    * Karşıya yükleme hatalarının bulunduğu Upload klasörü.
    * Yenileme sırasında karşılaşılan hataların bulunduğu Refresh klasörü.

    Yenileme günlük dosyası örneği burada gösterilmiştir.

    ```xml
    <root container="test1" machine="VM15BS020663" timestamp="03/18/2019 00:11:10" />
    <file item="test.txt" local="False" remote="True" error="16001" />
    <summary runtime="00:00:00.0945320" errors="1" creates="2" deletes="0" insync="3" replaces="0" pending="9" />
    ```

3. Bu dosyada bir hata gördüğünüzde (örnekte vurgulanan), bu örnekte 16001, hata kodunu göz önünde bulabilirsiniz. Hata kodunun açıklamasını aşağıdaki hata başvurusu bölümünde bulabilirsiniz.

    [!INCLUDE [data-box-edge-edge-upload-error-reference](../../includes/data-box-edge-gateway-upload-error-reference.md)]

## <a name="troubleshoot-iot-edge-errors"></a>IoT Edge hatalarında sorun giderme

[!INCLUDE [Troubleshoot IoT Edge runtime](../../includes/azure-stack-edge-iot-troubleshoot-compute.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Bu sürümdeki bilinen sorunlar](../databox-gateway/data-box-gateway-release-notes.md) hakkında daha fazla bilgi edinin.