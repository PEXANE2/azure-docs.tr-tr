---
title: Ağ Performansı İzleyicisi 'den bağlantı Izleyicisi 'ne (Önizleme) geçiş
titleSuffix: Azure Network Watcher
description: Ağ Performansı İzleyicisi 'den bağlantı Izleyicisi 'ne (Önizleme) geçiş yapmayı öğrenin.
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
ms.openlocfilehash: 69dbb1dd4017c5acf9c195f5104741caee38c2b7
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88701867"
---
# <a name="migrate-to-connection-monitor-preview-from-network-performance-monitor"></a>Ağ Performansı İzleyicisi 'den bağlantı Izleyicisi 'ne (Önizleme) geçiş

Ağ Performansı İzleyicisi testlerini tek tıklamayla yeni ve geliştirilmiş bağlantı Izleyicisine (Önizleme) ve sıfır kapalı kalma süresiyle geçirebilirsiniz. Avantajlar hakkında daha fazla bilgi edinmek için [Bağlantı İzleyicisi 'ni okuyabilirsiniz (Önizleme)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview)

>[!NOTE]
> Yalnızca hizmet bağlantı Izleyicisi 'ndeki testler bağlantı Izleyicisi 'ne (Önizleme) geçirilebilir.
>

## <a name="key-points-to-note"></a>Önemli noktaları göz önünde

* Şirket içi aracılar ve güvenlik duvarı ayarları olduğu gibi çalışacaktır. Değişiklik gerekmiyor. Azure sanal makinelerinde yüklü Log Analytics aracıların ağ Izleyicisi uzantısıyla değiştirilmeleri gerekir
* Mevcut testler, bağlantı Izleyicisi (Önizleme)-> test grubu > test biçimine eşlenir. Kullanıcılar, bağlantı Izleyicisinde değişiklik yapmak ve Azure Resource Manager aracılığıyla göndermek için yeni bağlantı Izleyicisinin özelliklerini görüntüleyip değiştirmek üzere *Düzenle* ' ye tıklayabilir.
* Aracılar – Log Analytics çalışma alanına ve ölçümlere veri gönderir.
* Verileri izleme
    * Log Analytics veriler – geçiş öncesi tüm veriler, NPM 'nin NetworkMonitoring tablosunda yapılandırıldığı çalışma alanında olmaya devam eder. Geçiş sonrası, veriler NetworkMonitoring tablosuna ve ConnectionMonitor_CL tablosuna aynı çalışma alanında gider. Testler NPM 'den devre dışı bırakıldıktan sonra, veriler yalnızca ConnectionMonitor_CL tablosunda depolanır
    * Günlük tabanlı uyarılar, panolar ve tümleştirmeler: yeni tablo ConnectionMonitor_CL göre sorguları el ile düzenlemeniz gerekir. Ayrıca, bu bağlantıyı kullanarak ölçümlere uyarıları yeniden oluşturabilirsiniz. Geçiş 'nin bir parçası olarak, ağ Izleme tablosu 'ndaki günlük tabanlı uyarıları ölçüm tabanlı uyarılara otomatik olarak geçirebilme özelliği yakında kullanıma sunulacaktır
    
## <a name="prerequisites"></a>Ön koşullar

*   Log Analytics çalışma alanının aboneliğinde ve bölgesinde ağ izleyicisinin etkinleştirildiğinden emin olun
*   Log Analytics aracıları yüklü Azure sanal makinelerinin ağ Izleyicisi uzantısıyla etkinleştirilmesi gerekir

## <a name="steps-to-migrate-tests-from-network-performance-monitor-to-connection-monitor-preview"></a>Ağ Performansı İzleyicisi testleri bağlantı Izleyicisine geçirme adımları (Önizleme)

1.  "Bağlantı Izleyicisi" ne tıklayın, testleri bağlantı Izleyicisine geçirmek için "NPM 'den testleri geçir" e gidin (Önizleme)

    ![Test NPM 'den bağlantı Izleyicisi önizlemesine geçiş testi gösteren ekran görüntüsü](./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png)
    
1.  Geçirmek istediğiniz abonelik, çalışma alanı ve NPM özelliğini seçin. Şu anda yalnızca hizmet bağlantısı Izleyiciden testleri geçirebilirsiniz.  
1.  Testleri geçirmek için "Içeri aktar" a tıklayın
1.  Geçiş başladıktan sonra, aşağıdaki değişiklikler gerçekleşir: 
    1. Yeni bir bağlantı İzleyicisi kaynağı oluşturuldu
        1. Her bölge ve abonelik için bir bağlantı İzleyicisi oluşturulur. Şirket içi aracılarla testler için yeni bağlantı izleyici adı <workspaceName> _"Şirket içi" biçimindedir. Azure aracılarıyla testler için yeni bağlantı izleyici adı <workspaceName> _<Azure_region_name biçimindedir>
        1. İzleme verileri artık NPM 'nin etkinleştirildiği Log Analytics çalışma alanında depolanır ve Connectionmonitor_CL tablo adlı yeni bir tablo. 
        1. Test adı, test grubu adına ileriye doğru yürütülür. Test açıklaması geçirilmeyecektir.
        1. Kaynak ve hedef uç noktaları oluşturulan test grubunda oluşturulur ve kullanılır. Şirket içi aracılar için uç noktalar <workspaceName> _"Endpoint"_ biçiminde adlandırılır <FQDN of on-premises machine> . Azure için, geçiş testleri aracıları içeriyorsa, aracıları etkinleştirmeniz ve yeniden geçirmeniz gerekecektir.
        1. Hedef bağlantı noktası ve yoklama aralığı test yapılandırmasına taşınır, yani "TC"_ <testname> "ve" TC "_ <testname> _" appeşikler ". Bağlantı noktası değerlerine bağlı olarak protokol ayarlanır. Başarı eşikleri ve diğer isteğe bağlı özellikler boş bırakılır.
    1. NPM devre dışı değil. Bu nedenle geçirilmiş sınamalar, verileri NetworkMonitoring tablosuna ve ConnectionMonitor_CL tablosuna göndermeye devam eder. Bu adım mevcut günlük tabanlı uyarıların ve tümleştirmelerin etkilenmemesini sağlar. Ağ Izleme tablosundaki günlük tabanlı uyarıları, geçişin bir parçası olarak otomatik olarak ölçüm tabanlı uyarılara geçirme, yakında kullanıma sunulacaktır.
    1. Yeni oluşturulan Bağlantı İzleyicisi bağlantı Izleyicisinde görünür olacak (Önizleme)
1.  Geçiş sonrası, NPM 'deki testleri el ile devre dışı bırakmanız gerekir. Siz bunu yapana kadar ücretlendirilmeye devam edersiniz. NPM 'yi devre dışı bırakırken, ConnectionMonitor_CL tablo üzerinde uyarılarınızı yeniden oluşturduğunuzdan veya ölçümler kullandığınızdan emin olun. Ayrıca, Power BI, Grafana ve SıEM sistemleriyle tümleştirmeler gibi tüm dış tümleştirmelerin ConnectionMonitor_CL tabloya geçirilmesi gerekdiğinden emin olun


## <a name="next-steps"></a>Sonraki adımlar

* [Bağlantı İzleyicisi 'Nden Bağlantı İzleyicisi 'ne geçiş yapmayı öğrenin (Önizleme)](migrate-to-connection-monitor-preview-from-connection-monitor.md)
* [Azure Portal kullanarak bağlantı İzleyicisi (Önizleme) oluşturmayı](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal) öğrenin
