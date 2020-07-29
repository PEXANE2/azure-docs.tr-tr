---
title: Azure Data Box Gateway cihazınızı izleme | Microsoft Docs
description: Azure Data Box Gateway izlemek için Azure portal ve yerel Web Kullanıcı arabiriminin nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: how-to
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: e352c2d8141f90c4dc721fe4fc0e717dfad7b88b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84339051"
---
# <a name="monitor-your-azure-data-box-gateway"></a>Azure Data Box Gateway izleyin

Bu makalede Azure Data Box Gateway nasıl izleneceği açıklanır. Cihazınızı izlemek için Azure portal veya yerel Web Kullanıcı arabirimi ' ni kullanabilirsiniz. Cihaz olaylarını görüntülemek, uyarıları yapılandırmak ve yönetmek ve ölçümleri görüntülemek için Azure portal kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Cihaz olaylarını ve ilgili uyarıları görüntüleme
> * Cihazınız için kapasite ve işlem ölçümlerini görüntüleme
> * Uyarıları yapılandırma ve yönetme

## <a name="view-device-events"></a>Cihaz olaylarını görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-metrics"></a>Ölçümleri görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

### <a name="metrics-on-your-device"></a>Cihazınızdaki ölçümler

Bu bölümde, cihazınızdaki izleme ölçümleri açıklanmaktadır. Ölçümler şu şekilde olabilir:

* Kapasite ölçümleri. Kapasite ölçümleri, cihazın kapasitesiyle ilgilidir.

* İşlem ölçümleri. İşlem ölçümleri, Azure depolama 'ya yönelik okuma ve yazma işlemleriyle ilgilidir.

Ölçümlerin tam listesi aşağıdaki tabloda gösterilmiştir:

|Kapasite ölçümleri                     |Açıklama  |
|-------------------------------------|-------------|
|**Kullanılabilir kapasite**               | Cihaza yazılabilen verilerin boyutunu ifade eder. Diğer bir deyişle, bu, cihazda kullanılabilir hale getirilebilir kapasitedir. <br></br>Hem cihazda hem de bulutta bir kopyası olan dosyaların yerel kopyasını silerek cihaz kapasitesini serbest bırakabilirsiniz.        |
|**Toplam kapasite**                   | Verilerin yazılacağı cihazdaki toplam bayt sayısını ifade eder. Bu, yerel önbelleğin toplam boyutu olarak da adlandırılır. <br></br> Artık bir veri diski ekleyerek var olan bir sanal cihazın kapasitesini artırabilirsiniz. VM için hiper yönetici yönetimi aracılığıyla bir veri diski ekleyin ve ardından sanal makineyi yeniden başlatın. Ağ Geçidi cihazının yerel depolama havuzu, yeni eklenen veri diskine uyum sağlayacak şekilde genişletilir. <br></br>Daha fazla bilgi için, [Hyper-V sanal makinesi için sabit sürücü ekleme](https://www.youtube.com/watch?v=EWdqUw9tTe4)bölümüne gidin. |

|İşlem ölçümleri              | Açıklama         |
|-------------------------------------|---------|
|**Karşıya yüklenen bulut baytları (cihaz)**    | Cihazınızdaki tüm paylaşımlar genelinde karşıya yüklenen tüm baytların toplamı        |
|**Karşıya yüklenen bulut baytları (paylaşma)**     | Her bir paylaşılan bayt karşıya yüklendi. Bu yük şunlardan biri olabilir: <br></br> Ort, (paylaşım başına karşıya yüklenen baytların toplamı/paylaşım sayısı),  <br></br>En fazla, bir paylaşımdan karşıya yüklenen en fazla bayt sayısıdır <br></br>En az, bir paylaşımdan karşıya yüklenen bayt sayısı alt sınırı      |
|**Bulut indirme verimlilik (paylaşma)**| Her bir paylaşıma indirilen bayt. Bu yük şunlardan biri olabilir: <br></br> Ortalama, (okuma veya bir paylaşıma/paylaşım sayısına indirilen tüm baytların toplamı) <br></br> En fazla, bir paylaşımdan indirilen en fazla bayt sayısıdır<br></br> ve bir paylaşımdan indirilen en az bayt sayısı olan dk  |
|**Bulut okuma performansı**            | Cihazınızdaki tüm paylaşımlar genelinde buluttan okunan tüm baytların toplamı     |
|**Bulut karşıya yükleme performansı**          | Cihazınızdaki tüm paylaşımlar genelinde buluta yazılan tüm baytların toplamı     |
|**Bulut karşıya yükleme üretilen işi (paylaşma)**  | Bir paylaşımdan buluta yazılan tüm baytların toplamı, paylaşım başına ortalama, en fazla ve en az      |
|**Okuma performansı (ağ)**           | Buluttan okunan tüm baytlar için sistem ağı aktarım hızını içerir. Bu görünüm, paylaşımlarla sınırlı olmayan verileri içerebilir. <br></br>Bölmek, cihazdaki tüm ağ bağdaştırıcılarının trafiğini gösterir. Bu, bağlı veya etkin olmayan bağdaştırıcılar içerir.      |
|**Yazma işleme (ağ)**       | Buluta yazılan tüm baytlar için sistem ağı aktarım hızını içerir. Bu görünüm, paylaşımlarla sınırlı olmayan verileri içerebilir. <br></br>Bölmek, cihazdaki tüm ağ bağdaştırıcılarının trafiğini gösterir. Bu, bağlı veya etkin olmayan bağdaştırıcılar içerir.          |

## <a name="manage-alerts"></a>Uyarıları yönetme

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Bant genişliğini yönetmeyi](data-box-gateway-manage-bandwidth-schedules.md) öğrenin.
