---
title: Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne (Önizleme) geçiş
titleSuffix: Azure Network Watcher
description: Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne (Önizleme) geçiş yapmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: 05b42024529b8d9de3590488ecafbdf83283e007
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441825"
---
# <a name="migrate-to-connection-monitor-preview-from-connection-monitor"></a>Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne (Önizleme) geçiş

Var olan bağlantı izleyicilerini yalnızca birkaç tıklamayla ve sıfır kapalı kalma süresiyle yeni, geliştirilmiş bağlantı Izleyicisine (Önizleme) geçirebilirsiniz. Avantajlar hakkında daha fazla bilgi edinmek için bkz. [Bağlantı İzleyicisi (Önizleme)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview).

## <a name="key-points-to-note"></a>Önemli noktaları göz önünde

Geçiş, aşağıdaki sonuçları üretmenize yardımcı olur:

* Aracılar ve güvenlik duvarı ayarları olduğu gibi çalışır. Değişiklik gerekmiyor. 
* Var olan bağlantı izleyicileri, test grubu > test biçimi > bağlantı Izleyicisi (Önizleme) ile eşleştirilir. **Düzenle**' yi seçerek, yeni bağlantı izleyicisinin özelliklerini görüntüleyip değiştirebilir, bağlantı izleyicisinde değişiklik yapmak için bir şablon indirebilir ve Azure Resource Manager aracılığıyla gönderebilirsiniz. 
* Ağ Izleyicisi uzantısına sahip Azure sanal makineleri, hem çalışma alanına hem de ölçümlere veri gönderir. Bağlantı Izleyicisi, verileri eski ölçümler (ProbesFailedPercent ve AverageRoundtripMs) yerine yeni ölçümler (ChecksFailedPercent [Preview] ve Roundroundtimems [Preview]) üzerinden kullanılabilir hale getirir. 
* Veri izleme:
   * **Uyarılar**: otomatik olarak yeni ölçümlere geçirilir.
   * **Panolar ve tümleştirmeler**: ölçüm kümesinin el ile düzenlenmesine gerek vardır. 
    
## <a name="prerequisites"></a>Ön koşullar

Özel bir çalışma alanı kullanıyorsanız, aboneliğinizde ve Log Analytics çalışma alanınızın bölgesinde ağ Izleyicisi 'nin etkinleştirildiğinden emin olun. 

## <a name="migrate-the-connection-monitors"></a>Bağlantı izleyicilerini geçirme

1. Eski bağlantı izleyicilerini yeni bir sürümüne geçirmek için **Bağlantı İzleyicisi**' ni seçin ve ardından **bağlantı izleyicilerini geçir**' i seçin.

    ![Bağlantı izleyicilerinin bağlantı izleyicilerine geçişini gösteren ekran görüntüsü (Önizleme).](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Aboneliğinizi ve geçirmek istediğiniz bağlantı izleyicilerini seçin ve sonra da **Seçileni geçir**' i seçin. 

Yalnızca birkaç tıklamayla, var olan bağlantı izleyicilerini bağlantı Izleyicisi 'ne (Önizleme) geçirdiniz. 

Artık bağlantı Izleyicisi (Önizleme) özelliklerini özelleştirebilir, varsayılan çalışma alanını değiştirebilir, şablonları indirebilir ve geçiş durumunu kontrol edebilirsiniz. 

Geçiş başladıktan sonra aşağıdaki değişiklikler gerçekleşir: 
* Azure Resource Manager kaynağı daha yeni bağlantı izleyicisinde değişir.
    * Bağlantı izleyicisinin adı, bölgesi ve aboneliği değişmeden kalır. Kaynak KIMLIĞI etkilenmemiştir.
    * Bağlantı İzleyicisi özelleştirilmediği takdirde, abonelikte ve bağlantı izleyicisinin bölgesinde varsayılan bir Log Analytics çalışma alanı oluşturulur. Bu çalışma alanı, izleme verilerinin depolandığı yerdir. Test sonucu verileri de ölçümlerde depolanır.
    * Her test, *defaulttestgroup*adlı bir test grubuna geçirilir.
    * Kaynak ve hedef uç noktaları yeni test grubunda oluşturulur ve kullanılır. Varsayılan adlar *Defaultsourceendpoint* ve *defaultdestinationendpoint*' dir.
    * Hedef bağlantı noktası ve yoklama aralığı, *Defaulttestconfiguration*adlı bir test yapılandırmasına taşınır. Protokol, bağlantı noktası değerlerine göre ayarlanır. Başarı eşikleri ve diğer isteğe bağlı özellikler boş bırakılır.
* Ölçüm uyarıları, bağlantı Izleyicisi (Önizleme) ölçüm uyarılarına geçirilir. Ölçümler farklıdır, bu nedenle değişiklik. Daha fazla bilgi için bkz. [Bağlantı İzleyicisi Ile ağ bağlantısı izleme (Önizleme)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#metrics-in-azure-monitor).
* Geçirilen bağlantı izleyicileri artık eski Bağlantı İzleyicisi çözümü olarak gösterilmez. Artık yalnızca bağlantı Izleyicisinde (Önizleme) kullanılabilir.
* Power BI ve Grafana 'deki panolar ve güvenlik bilgileri ve olay yönetimi (SıEM) sistemleriyle tümleştirmeler gibi dış tümleştirmeler, el ile geçirilmesi gerekir. Bu, kurulumunuzu geçirmek için yapmanız gereken tek el ile gerçekleştirilen adımdır.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı Izleyicisi (Önizleme) hakkında daha fazla bilgi için bkz.:
* [Ağ Performansı İzleyicisi bağlantı Izleyicisine geçiş (Önizleme)](migrate-to-connection-monitor-preview-from-network-performance-monitor.md)
* [Azure portal kullanarak bağlantı Izleyicisi (Önizleme) oluşturun](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
