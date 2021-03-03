---
title: Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne geçiş
titleSuffix: Azure Network Watcher
description: Bağlantı İzleyicisi 'nden bağlantı Izleyicisi 'ne geçiş yapmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: d4ab5361d245ad1ee10d43184cc0a2d65fed2054
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101730040"
---
# <a name="migrate-to-connection-monitor-from-connection-monitor-classic"></a>Bağlantı izleyiciden bağlantı Izleyicisine geçiş (klasik)

> [!IMPORTANT]
> 1 Temmuz 2021 tarihinden itibaren bağlantı Izleyicisine (klasik) yeni bağlantı izleyicileri ekleyemeyeceksiniz, ancak 1 2021 Temmuz 'dan önce oluşturulan mevcut bağlantı izleyicilerini kullanmaya devam edebilirsiniz. Geçerli iş yüklerinizde hizmet kesintisini en aza indirmek için, [bağlantı izleyicisinden (klasik)](migrate-to-connection-monitor-from-connection-monitor-classic.md)  Azure Ağ İzleyicisi 'Nde 29 Şubat 2024 ' den önceki yeni bağlantı izleyicisine geçiş yapın.

Mevcut bağlantı izleyicilerini yeni, geliştirilmiş bağlantı Izleyicilerine yalnızca birkaç tıklamayla ve sıfır kapalı kalma süresiyle geçirebilirsiniz. Avantajlar hakkında daha fazla bilgi edinmek için bkz. [Bağlantı İzleyicisi](./connection-monitor-overview.md).

## <a name="key-points-to-note"></a>Önemli noktaları göz önünde

Geçiş, aşağıdaki sonuçları üretmenize yardımcı olur:

* Aracılar ve güvenlik duvarı ayarları olduğu gibi çalışır. Değişiklik gerekmiyor. 
* Mevcut bağlantı izleyicileri > test grubu > test biçiminde bağlantı Izleyicisine eşlenir. **Düzenle**' yi seçerek, yeni bağlantı izleyicisinin özelliklerini görüntüleyip değiştirebilir, bağlantı izleyicisinde değişiklik yapmak için bir şablon indirebilir ve Azure Resource Manager aracılığıyla gönderebilirsiniz. 
* Ağ Izleyicisi uzantısına sahip Azure sanal makineleri, hem çalışma alanına hem de ölçümlere veri gönderir. Bağlantı Izleyicisi, verileri eski ölçümler (ProbesFailedPercent ve AverageRoundtripMs) yerine yeni ölçümler (ChecksFailedPercent ve Roundüçlü MS) üzerinden kullanılabilir hale getirir. Eski ölçümler, Probesfailedyüzde > ChecksFailedPercent ve AverageRoundtripMs-> Roundilerlik olarak yeni ölçümlere geçirilecektir.
* Veri izleme:
   * **Uyarılar**: otomatik olarak yeni ölçümlere geçirilir.
   * **Panolar ve tümleştirmeler**: ölçüm kümesinin el ile düzenlenmesine gerek vardır. 
    
## <a name="prerequisites"></a>Önkoşullar

Özel bir çalışma alanı kullanıyorsanız, aboneliğinizde ve Log Analytics çalışma alanınızın bölgesinde ağ Izleyicisi 'nin etkinleştirildiğinden emin olun. 

## <a name="migrate-the-connection-monitors"></a>Bağlantı izleyicilerini geçirme

1. Eski bağlantı izleyicilerini yeni bir sürümüne geçirmek için **Bağlantı İzleyicisi**' ni seçin ve ardından **bağlantı izleyicilerini geçir**' i seçin.

    ![Bağlantı izleyicilerinin bağlantı izleyicilerine geçişini gösteren ekran görüntüsü.](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Aboneliğinizi ve geçirmek istediğiniz bağlantı izleyicilerini seçin ve sonra da **Seçileni geçir**' i seçin. 

Yalnızca birkaç tıklamayla, var olan bağlantı izleyicilerini bağlantı Izleyicisi 'ne geçirdiniz. Cm 'den (klasik) cm 'ye geçirildiğinde, izleyiciyi CM (klasik) altında göremezsiniz.

Artık bağlantı Izleyici özelliklerini özelleştirebilir, varsayılan çalışma alanını değiştirebilir, şablonları indirebilir ve geçiş durumunu kontrol edebilirsiniz. 

Geçiş başladıktan sonra aşağıdaki değişiklikler gerçekleşir: 
* Azure Resource Manager kaynağı daha yeni bağlantı izleyicisinde değişir.
    * Bağlantı izleyicisinin adı, bölgesi ve aboneliği değişmeden kalır. Kaynak KIMLIĞI etkilenmemiştir.
    * Bağlantı İzleyicisi özelleştirilmediği takdirde, abonelikte ve bağlantı izleyicisinin bölgesinde varsayılan bir Log Analytics çalışma alanı oluşturulur. Bu çalışma alanı, izleme verilerinin depolandığı yerdir. Test sonucu verileri de ölçümlerde depolanır.
    * Her test, *defaulttestgroup* adlı bir test grubuna geçirilir.
    * Kaynak ve hedef uç noktaları yeni test grubunda oluşturulur ve kullanılır. Varsayılan adlar *Defaultsourceendpoint* ve *defaultdestinationendpoint*' dir.
    * Hedef bağlantı noktası ve yoklama aralığı, *Defaulttestconfiguration* adlı bir test yapılandırmasına taşınır. Protokol, bağlantı noktası değerlerine göre ayarlanır. Başarı eşikleri ve diğer isteğe bağlı özellikler boş bırakılır.
* Ölçüm uyarıları, bağlantı Izleyicisi ölçüm uyarılarına geçirilir. Ölçümler farklıdır, bu nedenle değişiklik. Daha fazla bilgi için bkz. [Bağlantı İzleyicisi Ile ağ bağlantısı izleme](./connection-monitor-overview.md#metrics-in-azure-monitor).
* Geçirilen bağlantı izleyicileri artık eski Bağlantı İzleyicisi çözümü olarak gösterilmez. Artık yalnızca bağlantı Izleyicisinde kullanıma sunulmuştur.
* Power BI ve Grafana 'deki panolar ve güvenlik bilgileri ve olay yönetimi (SıEM) sistemleriyle tümleştirmeler gibi dış tümleştirmeler, el ile geçirilmesi gerekir. Bu, kurulumunuzu geçirmek için yapmanız gereken tek el ile gerçekleştirilen adımdır.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı Izleyicisi hakkında daha fazla bilgi edinmek için bkz.:
* [Ağ Performansı İzleyicisi bağlantı Izleyicisine geçir](./migrate-to-connection-monitor-from-network-performance-monitor.md)
* [Azure portal kullanarak bağlantı Izleyicisi oluşturma](./connection-monitor-create-using-portal.md)
