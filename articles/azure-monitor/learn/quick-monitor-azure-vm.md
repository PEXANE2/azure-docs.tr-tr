---
title: Azure Monitörü ile Azure kaynağını izleme
description: Azure Monitörü'nde bir Azure kaynağı için nasıl veri toplayıp analiz edeceğiz öğrenin.
ms.service: azure-monitor
ms. subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 03/10/2020
ms.openlocfilehash: 12616fbc80468d35ebb8b0bc5cd12bfd5871e788
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79503660"
---
# <a name="quickstart-monitor-an-azure-virtual-machine-with-azure-monitor"></a>Hızlı başlatma: Azure Monitor ile Azure sanal makinesini izleme
[Azure Monitor,](../overview.md) oluşturuldukları anda Azure sanal makinelerinden veri toplamaya başlar. Bu hızlı başlangıçta, bir Azure VM için otomatik olarak toplanan verileri ve Azure portalında nasıl görüntüleyebilirsiniz kısa bir yol alırsınız. Daha sonra [VM'leriniz için Azure Monitor'u](../insights/vminsights-overview.md) etkinleştirirsiniz ve bu da VM'deki aracıların süreçler ve bunların bağımlılıkları da dahil olmak üzere konuk işletim sisteminden veri toplamasına ve analiz etmesini sağlar.

Bu hızlı başlangıçta mevcut bir Azure sanal makinenizin olduğu varsayılmaktadır. Değilse, bir [Windows VM](../../virtual-machines/windows/quick-create-portal.md) oluşturabilir veya VM hızlı başlatmalarımızı takiben bir [Linux VM](../../virtual-machines/linux/quick-create-cli.md) oluşturabilirsiniz.

Azure kaynaklarından toplanan izleme verilerinin daha ayrıntılı açıklamaları için Azure [Sanal makinelerini Azure Monitor ile izleme](../insights/monitor-vm-azure.md)bölümüne bakın.


## <a name="complete-the-monitor-an-azure-resource-quickstart"></a>Azure kaynağını hızlı başlat'ı izleyin'i tamamlayın.
Aboneliğinizdeki bir VM'nin genel bakış sayfasını, etkinlik günlüğünü ve ölçümlerini görüntülemek için [Azure Monitörü ile](quick-monitor-azure-resource.md) Azure kaynağını izleyin'i tamamlayın. Azure VM'ler diğer Azure kaynaklarıyla aynı izleme verilerini toplar, ancak bu yalnızca ana bilgisayar VM içindir. Bu hızlı başlatmanın geri kalanı konuk işletim sistemini ve iş yüklerini izlemeye odaklanacaktır.


## <a name="enable-azure-monitor-for-vms"></a>VM'ler için Azure Monitörünü etkinleştirme
Ana bilgisayar VM için ölçümler ve etkinlik günlükleri toplanırken, konuk işletim sisteminden ve iş yüklerinden izleme verilerini toplamak ve analiz etmek için bir aracıya ve bazı yapılandırmaya ihtiyacınız vardır. Sanal Makineler için Azure Monitor bu aracıları yükler ve sanal makinelerinizi izlemek için ek güçlü özellikler sağlar.

1. Sanal makinenizin menüsüne gidin.
2. **Genel Bakış** sayfasındaki döşemeden **Öngörülere Git'i** tıklatın veya **İzleme** menüsünden **Öngörüler'e** tıklayın.

    ![Genel bakış sayfası](media/quick-monitor-azure-vm/overview-insights.png)

3. Sanal makine için Sanal Sanal Aygıtlar için Azure Monitörü henüz etkinleştirildiyse, **Etkinleştir'i**tıklatın. 

    ![Öngörüleri etkinleştirme](media/quick-monitor-azure-vm/enable-insights.png)

4. Sanal makine zaten bir Log Analytics çalışma alanına bağlı değilse, varolan bir çalışma alanı seçmeniz veya yeni bir çalışma alanı oluşturmanız istenir. Sanal makinenizle aynı bölgede benzersiz bir ada sahip bir çalışma alanı olan varsayılanı seçin.

    ![Çalışma alanını seçme](media/quick-monitor-azure-vm/select-workspace.png)

5. Uzantıları etkinleştirildiğinde ve aracılar sanal makinenize yüklendiğinden onboarding birkaç dakika sürer. Tamamlandığında, öngörülerin başarıyla dağıtıldığına dair bir ileti alırsınız. VM'ler için Azure Monitör'ü açmak için **Azure Monitör'ü** tıklatın.

    ![Azure Monitörünü Aç](media/quick-monitor-azure-vm/azure-monitor.png)

6. VM'nizi aboneliğinizde bulunan diğer VM'lerle birlikte görürsünüz. Aboneliğinizde yerleşik olmayan sanal makineleri görüntülemek **istiyorsanız, izlenmez** sekmesini seçin.

    ![Kullanmaya başlayın](media/quick-monitor-azure-vm/get-started.png)


## <a name="configure-workspace"></a>Çalışma alanını yapılandırma
Yeni bir Log Analytics çalışma alanı oluşturduğunuzda, günlükleri toplamak için yapılandırılması gerekir. Yapılandırma, yapılandırma ona bağlanan sanal makinelere gönderildiğinden, bu yapılandırmanın yalnızca bir kez gerçekleştirilmesi gerekir.

1. **Çalışma Alanı yapılandırması'nı** seçin ve ardından çalışma alanınızı seçin.

2. **Gelişmiş ayarları** seçin

    ![Log Analytics Gelişmiş Ayarlar](media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png)

### <a name="data-collection-from-windows-vm"></a>Windows VM’den veri toplama


2. **Veri**’yi seçin ve ardından **Windows Olay Günlükleri**’ni seçin.

3. Günlüğün adını yazarak bir olay günlüğü ekleyin.  **Sistem** yazın ve sonra **+** artı işaretini seçin.

4. Tabloda, **Hata** ve **Uyarı** önem derecelerini işaretleyin.

5. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

### <a name="data-collection-from-linux-vm"></a>Linux VM’den veri toplama

1. **Syslog**’u seçin.  

2. Günlüğün adını yazarak bir olay günlüğü ekleyin.  **Syslog** yazın ve sonra **+** artı işaretini seçin.  

3. Tabloda, önem leri **Bilgi,** **Bildirim** ve **Hata Ayıklama**seçin. 

4. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin

7. Sanal makinenize tıklayın ve ardından **Performans** sekmesini seçin. Bu, VM'nizin konuk işletim sisteminden toplanan belirli bir performans sayacı grubunu gösterir. Daha fazla sayaç görüntülemek için aşağı kaydırın ve farklı zamanlarda ortalama ve yüzdelik ortalamaları görüntülemek için fareyi bir grafiğin üzerinde hareket ettirin.

    ![Performans](media/quick-monitor-azure-vm/performance.png)

9. Sanal makinede çalışan işlemleri ve bunların bağımlılıklarını gösteren haritalar özelliğini açmak için **Harita'yı** seçin. Zaten açık değilse özellik bölmesini açmak için **Özellikler'i** seçin.

    ![Eşleme](media/quick-monitor-azure-vm/map.png)

11. Sanal makinenizin proseslerini genişletin. Ayrıntılarını görüntülemek ve bağımlılıklarını vurgulamak için işlemlerden birini seçin.

    ![İşlemler](media/quick-monitor-azure-vm/processes.png)

12. Sanal makinenizi yeniden seçin ve ardından **Günlük Olayları'nı**seçin. 

    ![Günlük olayları](media/quick-monitor-azure-vm/log-events.png)

13. Sanal makine için Log Analytics çalışma alanında depolanan tabloların listesini görürsünüz. Bu liste, Windows veya Linux sanal makine kullanıyor sanız bağlı olarak farklı olacaktır. **Etkinlik** tablosunu tıklatın. Bu, Windows olay günlüğündeki tüm olayları içerir. Log Analytics, olay günlüğü girişlerini almak için basit bir sorguyla açılır.

    ![Log Analytics](media/quick-monitor-azure-vm/log-analytics.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, sanal bir makine için Sanal Makineler için Azure Monitor'u etkinleştirdin ve konuk işletim sistemi için etkinlikleri toplamak için Log Analytics çalışma alanını yapılandırıldınız. Verileri görüntüleme ve analiz etmeyi öğrenmek için, öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](../../azure-monitor/learn/tutorial-viewdata.md)
