---
title: Azure Stack Edge sorunlarını gidermek için Azure portal kullanın | Microsoft Docs
description: Azure Stack Kenarlarınızın sorunlarını nasıl giderebileceğinizi öğrenin. Tanılama çalıştırabilir, destek için bilgi toplayabilir ve günlükleri kullanarak sorun giderme yapabilirsiniz.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 03/15/2019
ms.author: alkohli
ms.openlocfilehash: b80b47a1be67a9771d138c43178d50156a6f1a95
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87926442"
---
# <a name="troubleshoot-your-azure-stack-edge-issues"></a>Azure Stack Edge sorunlarından sorun giderme

Bu makalede Azure Stack Kenarlarınızın sorunlarını giderme işleminin nasıl giderileceği açıklanmaktadır. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Tanılama çalıştırma
> * Destek paketi toplama
> * Sorun gidermek için günlükleri kullanma


## <a name="run-diagnostics"></a>Tanılama çalıştırma

Cihaz hatalarını tanılamak ve gidermek için tanılama testlerini çalıştırabilirsiniz. Tanılama testlerini çalıştırmak için cihazınızın yerel web arabiriminde aşağıdaki adımları gerçekleştirin.

1. Yerel web arabiriminde **Sorun giderme > Tanılama testleri** sayfasına gidin. Çalıştırmak istediğiniz testi seçip **Testi çalıştır** öğesine tıklayın. Bunu yaptığınızda ağ, cihaz, web proxy, saat veya bulut ayarlarınızdaki sorunların tanılanmasına yönelik testler çalıştırılır. Cihazın testleri çalıştırdığı bildirilir.

    ![Testleri seçin](media/azure-stack-edge-troubleshoot/run-diag-1.png)
 
2. Testler tamamlandıktan sonra sonuçlar görüntülenir. 

    ![Test sonuçlarını gözden geçirme](media/azure-stack-edge-troubleshoot/run-diag-2.png)

    Test başarısız olursa önerilen eylem URL'si gösterilir. URL'ye tıklayarak önerilen eylemi görüntüleyebilirsiniz.
 
    ![Başarısız testler için uyarıları gözden geçirin](media/azure-stack-edge-troubleshoot/run-diag-3.png)


## <a name="collect-support-package"></a>Destek paketi toplama

Günlük paketi, Microsoft Desteğinin cihaz sorunlarını giderme konusunda yardımcı olabilmesi için ihtiyaç duyabileceği günlüklerin tamamını içerir. Günlük paketi oluşturmak için yerel web arabirimini kullanabilirsiniz.

Destek paketi toplamak için aşağıdaki adımları gerçekleştirin. 

1. Yerel web arabiriminde **Sorun giderme > Destek** sayfasına gidin. **Destek paketi oluştur** öğesine tıklayın. Sistem, destek paketini toplamaya başlar. Paketin toplanması birkaç dakika sürebilir.

    ![Kullanıcı ekle'ye tıklayın](media/azure-stack-edge-troubleshoot/collect-logs-1.png)
 
2. Destek paketi oluşturulduktan sonra **Destek paketini indir**'e tıklayın. Seçtiğiniz yola .zip uzantılı bir paket indirilir. Paketi açıp sistem günlüğü dosyalarını görüntüleyebilirsiniz.

    ![Kullanıcı ekle'ye tıklayın](media/azure-stack-edge-troubleshoot/collect-logs-2.png)

## <a name="use-logs-to-troubleshoot"></a>Sorun gidermek için günlükleri kullanma

Yükleme ve yenileme işlemlerinde karşılaşılan hatalar ilgili hata dosyalarına yazılır.

1. Hata dosyalarını görüntülemek için paylaşımınıza gidin ve içeriği görüntülemek için paylaşıma tıklayın. 

      ![Paylaşılan içeriğe bağlanma ve içeriği görüntüleme](media/azure-stack-edge-troubleshoot/troubleshoot-logs-1.png)

2. _Microsoft Azure Stack Edge klasörüne_tıklayın. Bu klasör iki alt klasör içerir:

    - Karşıya yükleme hatalarının bulunduğu Upload klasörü.
    - Yenileme sırasında karşılaşılan hataların bulunduğu Refresh klasörü.

    Yenileme günlük dosyası örneği burada gösterilmiştir.

    ```
    <root container="test1" machine="VM15BS020663" timestamp="03/18/2019 00:11:10" />
    <file item="test.txt" local="False" remote="True" error="16001" />
    <summary runtime="00:00:00.0945320" errors="1" creates="2" deletes="0" insync="3" replaces="0" pending="9" />
    ``` 

3. Bu dosyada bir hata gördüğünüzde (örnekte vurgulanmıştır) hata kodunu yazın (burada 16001). Hata kodunun açıklamasını aşağıdaki hata başvurusu bölümünde bulabilirsiniz.

    [!INCLUDE [data-box-edge-edge-upload-error-reference](../../includes/data-box-edge-gateway-upload-error-reference.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [Bu sürümdeki bilinen sorunlar](data-box-gateway-release-notes.md) hakkında daha fazla bilgi edinin.
