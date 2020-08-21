---
title: Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne (Önizleme) geçiş
titleSuffix: Azure Network Watcher
description: Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne (Önizleme) geçiş yapmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: ddf6e326df876229d32ef15983f76879836f1fca
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88701868"
---
# <a name="migrate-to-connection-monitor-preview-from-connection-monitor"></a>Bağlantı Izleyicisi 'nden bağlantı Izleyicisi 'ne (Önizleme) geçiş

Mevcut bağlantı izleyicilerini yeni ve geliştirilmiş bağlantı Izleyicisine (Önizleme) tek tıklamayla ve sıfır kapalı kalma süresiyle geçirebilirsiniz. Avantajlar hakkında daha fazla bilgi edinmek için [Bağlantı İzleyicisi 'ni okuyabilirsiniz (Önizleme)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview)

## <a name="key-points-to-note"></a>Önemli noktaları göz önünde

* Aracılar ve güvenlik duvarı ayarları olduğu gibi çalışır (dokunma gerekmez) 
* Mevcut bağlantı izleyicileri bağlantı Izleyici (Önizleme)-> test grubu > test biçimine eşlenir. Kullanıcılar, bağlantı Izleyicisinde değişiklik yapmak ve Azure Resource Manager aracılığıyla göndermek için yeni bağlantı Izleyicisinin özelliklerini görüntüleyip değiştirmek üzere *Düzenle* ' ye tıklayabilir. 
* Ağ Izleyicisi uzantısına sahip Azure sanal makineleri, verileri çalışma alanına ve ölçümlere gönderir. Bağlantı Izleyicileri, eski ölçümleri Durdur (ProbesFailedPercent ve AverageRoundtripMs) yerine yeni ölçümler (ChecksFailedPercent (Preview) ve Roundroundtimems (Önizleme)) aracılığıyla verilerin kullanılabilmesini sağlayacak 
* Verileri izleme
    * Uyarılar – geçişin bir parçası olarak yeni ölçümlere geçirilecektir
    * Panolar ve tümleştirmeler – ölçüm kümesini el ile düzenlemeniz gerekir. 
    
## <a name="prerequisites"></a>Ön koşullar

Özel çalışma alanı kullanıyorsanız, Log Analytics çalışma alanının aboneliğinde ve bölgesinde ağ izleyicisinin etkinleştirildiğinden emin olun 

## <a name="steps-to-migrate-from-connection-monitor-to-connection-monitor-preview"></a>Bağlantı izleyiciden bağlantı Izleyicisine geçiş adımları (Önizleme)

1. Bağlantı izleyicilerini daha yeni çözüme geçirmek için "bağlantı Izleyicisi" ne tıklayın, "bağlantı Izleyicilerini geçir" e gidin.

    ![Bağlantı izleyicilerine bağlantı Izleyicisi önizlemesine geçişi gösteren ekran görüntüsü](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Abonelik ve bağlantı izleyicileri seçip "Seçileni geçir" seçeneğine tıklayın. Bir tek tıklama var olan bağlantı izleyicilerini bağlantı Izleyicisine geçir (Önizleme) 
1. Bağlantı İzleyicisi özelliklerini özelleştirebilir, varsayılan çalışma alanını değiştirebilir, şablonu indirebilir ve geçişin durumunu kontrol edebilirsiniz. 
1. Geçiş başladıktan sonra, aşağıdaki değişiklikler gerçekleşir: 
    1. Kaynak değişikliklerini daha yeni bağlantı izleyicisinde Azure Resource Manager
        1. Bağlantı izleyicisinin adı, bölgesi ve aboneliği değişmeden kalır. Bu nedenle, kaynak KIMLIĞI üzerinde hiçbir etkisi yoktur.
        1. Özelleştirilmediği takdirde, bağlantı izleyicisinin bölgesinde ve aboneliğinde varsayılan bir Log Analytics çalışma alanı oluşturulur. Bu çalışma alanı, izleme verilerinin depolanacağı yerdir. Test sonucu verileri de ölçümler ' de depolanır.
        1. Her test, * defaultTestGroup * adlı bir test grubuna geçirilir
        1.  Kaynak ve hedef uç noktaları oluşturulan test grubunda oluşturulur ve kullanılır. Varsayılan adlar *Defaultsourceendpoint* ve *defaultdestinationendpoint*
        1. Hedef bağlantı noktası ve yoklama aralığı, *defaulttestconfiguration*adlı test yapılandırmasına taşınır. Bağlantı noktası değerlerine bağlı olarak protokol ayarlanır. Başarı eşikleri ve diğer isteğe bağlı özellikler boş bırakılır.
    1. Ölçüm uyarıları bağlantı Izleyicisi (Önizleme) ölçüm uyarılarına geçirilir. Ölçümler farklıdır <link to metric section in the doc> , bu nedenle değişiklik
    1. Geçirilen bağlantı izleyicileri eski Bağlantı İzleyicisi çözümünde gösterilmez, artık yalnızca bağlantı Izleyicilerinde (Önizleme) kullanıma sunulacaktır
    1. SıEM sistemleriyle Power BI, Grafana ve tümleştirmeler gibi tüm dış tümleştirmeler, Kullanıcı tarafından doğrudan geçirilmesi gerekecektir. Bu, kullanıcının kurulumunu geçirmek için gerçekleştirmesi gereken tek el ile yapılan adımdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ performansı İzleyicisi Bağlantı İzleyicisi 'ne (Önizleme) geçiş yapmayı](migrate-to-connection-monitor-preview-from-network-performance-monitor.md) öğrenin
* [Azure Portal kullanarak bağlantı İzleyicisi (Önizleme) oluşturmayı](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal) öğrenin
