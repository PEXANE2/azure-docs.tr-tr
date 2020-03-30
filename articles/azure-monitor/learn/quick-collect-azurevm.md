---
title: Azure Monitörü ile Azure sanal makinesinden veri toplama | Microsoft Dokümanlar
description: Log Analytics Aracısı VM Uzantısını etkinleştirmeyi ve Log Analytics ile Azure VM’lerinizden veri toplamayı etkinleştirmeyi öğrenin.
ms.subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 08/19/2019
ms.custom: mvc, sec-javascript-october2019
ms.openlocfilehash: 2e63b2ca0ee7246009068e6a2875795a14aeddb3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240341"
---
# <a name="collect-data-from-an-azure-virtual-machine-with-azure-monitor"></a>Azure Monitor ile Azure sanal makinesinden veri toplama

[Azure Monitor,](../overview.md) ayrıntılı analiz ve korelasyon için doğrudan Azure sanal makinelerinizden bir Log Analytics çalışma alanına veri toplayabilir. [Windows](../../virtual-machines/extensions/oms-windows.md) ve [Linux](../../virtual-machines/extensions/oms-linux.md) için Log Analytics VM uzantısının yüklenmesi, Azure Monitor'un Azure VM'lerinizden veri toplamasına olanak tanır. Bu hızlı başlangıç, VM uzantısını birkaç kolay adımla kullanarak Azure Linux veya Windows SANAL'larınızdan nasıl veri yapılandırabileceğinizi ve topladığınızı gösterir.  
 
Bu hızlı başlangıçta mevcut bir Azure sanal makinenizin olduğu varsayılmaktadır. Yoksa VM hızlı başlangıçlarımızı izleyerek bir [Windows VM](../../virtual-machines/windows/quick-create-portal.md) veya bir [Linux VM](../../virtual-machines/linux/quick-create-cli.md) oluşturabilirsiniz.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

1. Azure portalında **Tüm hizmetler'i**seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.

    ![Azure portalında](media/quick-collect-azurevm/azure-portal-log-analytics-workspaces.png)<br>  

2. **Oluştur'u**seçin ve ardından aşağıdaki öğeler için seçenekleri seçin:

   * *Varsayılan LAWorkspace*gibi yeni **Log Analytics çalışma alanı**için bir ad sağlayın.  
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak Grubu** için, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
   * VM’lerinizin dağıtıldığı **Konum**’u seçin.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan’dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir EA kaydına bağlı aboneliğe yönelik çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).
  
        ![Log Analytics kaynak bıçağı oluşturun](media/quick-collect-azurevm/create-log-analytics-workspace-azure-portal.png) 

3. **Log Analytics çalışma alanı** bölmesi üzerinde gerekli bilgileri sağladıktan sonra **Tamam'ı**seçin.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="enable-the-log-analytics-vm-extension"></a>Log Analytics VM Uzantısını etkinleştirme

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Zaten Azure’da dağıtılan Windows ve Linux sanal makineler için, Log Analytics aracısını Log Analytics VM Uzantısı ile yüklersiniz. Uzantıyı kullanmak yükleme işlemini kolaylaştırır ve aracıyı belirttiğiniz Log Analytics çalışma alanına veri göndermek üzere otomatik olarak yapılandırır. Aracı, en son özelliklere ve düzeltmelere sahip olduğunuzdan emin olmak için daha yeni bir sürüm piyasaya sürüldüğünde de otomatik olarak yükseltilir. İşleme devam etmeden önce, VM'nin çalıştığını doğrulayın aksi takdirde işlem başarılı bir şekilde tamamlanacak şekilde başarısız olur.  

>[!NOTE]
>Linux için Log Analytics aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz. 

1. Azure portalında, sol üst köşede bulunan **Tüm hizmetleri** seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.

2. Log Analytics çalışma alanlarınızın listesinde, daha önceden oluşturduğunuz *DefaultLAWorkspace* çalışma alanını seçin.

3. Sol menüde, Çalışma Alanı Veri Kaynakları altında **Sanal makineleri**seçin.  

4. **Sanal makine** listesinden aracıyı yüklemek istediğiniz bir sanal makine seçin. VM için **Log Analytics bağlantı durumunun****Bağlı değil** olduğuna dikkat edin.

5. Sanal makinenizin ayrıntılarında, **Bağlan**’ı seçin. Aracı otomatik olarak yüklenir ve Log Analytics çalışma alanınız için yapılandırılır. Bu işlem birkaç dakika sürer ve bu süre içinde **Durum** **Bağlantıyı**gösterir.

6. Aracıyı yükleyip bağlandıktan sonra, **Log Analytics bağlantı durumu****Bu çalışma alanı** olarak güncelleştirilir.

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama

Azure Monitor, Windows olay günlüklerinden veya Linux Syslog'dan ve daha uzun vadeli analiz ve raporlama için belirttiğiniz performans sayaçlarından olayları toplayabilir ve belirli bir koşul algılandığında harekete geçebilir. Windows sistem günlüğü ve Linux Syslog’dan olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

### <a name="data-collection-from-windows-vm"></a>Windows VM’den veri toplama

1. **Gelişmiş ayarlar**’ı seçin.

    ![Log Analytics Gelişmiş Ayarlar](media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png)

2. **Veri**’yi seçin ve ardından **Windows Olay Günlükleri**’ni seçin.

3. Bir olay günlüğü eklemek için günlüğün adını yazın.  **Sistem** yazın ve sonra **+** artı işaretini seçin.

4. Tabloda, **Hata** ve **Uyarı** önem derecelerini işaretleyin.

5. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

6. Bir Windows bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Windows Performans Verileri**’ni seçin.

7. Yeni bir Log Analytics çalışma alanı için Windows Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir.

    ![Varsayılan Windows performans sayaçları seçildi](media/quick-collect-azurevm/windows-perfcounters-default.png)

    **Seçili performans sayaçlarını ekle'yi**seçin.  Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.
  
8. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

### <a name="data-collection-from-linux-vm"></a>Linux VM’den veri toplama

1. **Syslog**’u seçin.  

2. Bir olay günlüğü eklemek için günlüğün adını yazın.  **Syslog** yazın ve sonra **+** artı işaretini seçin.  

3. Tabloda, önem leri **Bilgi,** **Bildirim** ve **Hata Ayıklama**seçin. 

4. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

5. Bir Linux bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Linux Performans Verileri**’ni seçin. 

6. Yeni bir Log Analytics çalışma alanı için Linux Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir.

    ![Varsayılan Windows performans sayaçları seçildi](media/quick-collect-azurevm/linux-perfcounters-azure-monitor.png)

    **Makinelerime aşağıdaki yapılandırmayı uygula'yı** seçin ve **ardından seçili performans sayaçlarını ekle'yi**seçin.  Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.  

7. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin

Veri toplamayı etkinleştirdiyseniz, şimdi hedef VM’lerden verileri görmek için basit bir günlük araması örneği çalıştıralım.  

1. Seçili çalışma alanında, sol bölmeden **Günlükler'i**seçin.

2. Günlükler sorgu sayfasında, `Perf` sorgu düzenleyicisini yazın ve **Çalıştır'ı**seçin.

    ![Log Analytics günlük araması sorgu örneği](./media/quick-collect-windows-computer/log-analytics-portal-queryexample.png) 

    Örneğin, aşağıdaki resimdeki sorgu 10.000 performans kaydı döndürdü. Sonuçlarınız önemli ölçüde daha az olacaktır.

    ![Log Analytics günlük araması sonucu](media/quick-collect-azurevm/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, Log Analytics çalışma alanını silin. Bunu yapmak için, daha önce oluşturduğunuz Günlük Analizi çalışma alanını seçin ve kaynak sayfasında **Sil'i**seçin.


![Log Analytics kaynağını silme](media/quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Windows veya Linux sanal makinelerinizden işletimsel verileri ve performans verilerini topluyorsunuz ve *ücretsiz* olarak topladığınız verileri kolayca keşfetmeye, analiz etmeye ve verilerde işlem gerçekleştirmeye başlayabilirsiniz.  

Verileri görüntüleme ve analiz etmeyi öğrenmek için, öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](../../azure-monitor/learn/tutorial-viewdata.md)
