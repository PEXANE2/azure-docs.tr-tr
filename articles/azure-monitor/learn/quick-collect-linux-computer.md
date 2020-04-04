---
title: 'Hızlı başlatma: Azure Monitor ile karma bir Linux Bilgisayarından veri toplama'
description: Bu hızlı başlangıçta, Azure dışında çalışan Linux bilgisayarları için Log Analytics aracısını nasıl dağıtabileceğinizi ve Azure Monitör Günlükleri ile veri toplamayı nasıl etkinleştireceğinizi öğreneceksiniz.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 12/24/2019
ms.author: bwren
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 1da862a7994d7db5ee8c9f090119e14f19c6b13a
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637885"
---
# <a name="quickstart-collect-data-from-a-linux-computer-in-a-hybrid-environment-with-azure-monitor"></a>Hızlı başlatma: Azure Monitor ile karma bir ortamda bir Linux bilgisayarından veri toplama

[Azure Monitor,](../overview.md) ortamınızdaki fiziksel veya sanal Linux bilgisayarlarınızdan doğrudan veri toplayabilir ve ayrıntılı analiz ve korelasyon için Log Analytics çalışma alanına girebilir. Log [Analytics aracısının](../platform/log-analytics-agent.md) yüklenmesi, Azure Monitor'un veri merkezinden veya başka bir bulut ortamından veri toplamasına olanak tanır. Bu hızlı başlangıç, birkaç kolay adımla Linux sunucunuzdan nasıl veri yapılandırabileceğinizi ve topladığınızı gösterir. Azure Linux SANAL M'leri hakkında bilgi [için](../../azure-monitor/learn/quick-collect-azurevm.md)bkz.  

Desteklenen yapılandırmayı anlamak için [bkz.](../../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) [Network firewall configuration](../../azure-monitor/platform/log-analytics-agent.md#network-requirements)
 
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

1. Azure portalında **Tüm hizmetler'i**seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.

    ![Azure portalında Log Analytics çalışma alanını bulma](media/quick-collect-azurevm/azure-portal-log-analytics-workspaces.png)<br>  

2. **Oluştur'u**seçin ve ardından aşağıdaki öğeler için seçenekleri seçin:

   * *Varsayılan LAWorkspace*gibi yeni **Log Analytics çalışma alanı**için bir ad sağlayın.  
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak Grubu** için, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
   * VM’lerinizin dağıtıldığı **Konum**’u seçin.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan’dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir EA kaydına bağlı aboneliğe yönelik çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).
  
        ![Azure portalında Günlük Analizi çalışma alanı oluşturma](media/quick-collect-azurevm/create-log-analytics-workspace-azure-portal.png) 

3. **Log Analytics çalışma alanı** bölmesi üzerinde gerekli bilgileri sağladıktan sonra **Tamam'ı**seçin.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma

Linux için Log Analytics aracısını yüklemeden önce, Log Analytics çalışma alanınızın kimliği ve anahtarına ihtiyacınız olacak. Bu bilgiler aracıyı düzgün yapılandırmak ve Azure Monitor ile başarılı bir şekilde iletişim kurabilmesini sağlamak için aracı yıkıcı komut dosyası tarafından gereklidir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Azure portalının sol üst köşesinde **Tüm hizmetler'i**seçin. Arama kutusuna, **Günlük Analizi'ni**girin. Siz yazarken, girişinize göre liste filtreler. **Günlük Analizi çalışma alanlarını**seçin.

2. Günlük Analizi çalışma alanları listenizde, daha önce oluşturduğunuz çalışma alanını seçin. (Buna **DefaultLAWorkspace**adını vermiş olabilirsiniz.)

3. **Gelişmiş ayarları**seçin:

    ![Azure portalında Günlük Analitiği için Gelişmiş Ayarlar menüsü](media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png) 
 
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Linux Sunucuları**’nı seçin.

5. **Çalışma Alanı Kimliği** ve **Birincil Anahtar**’ın sağındaki değer. Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

## <a name="install-the-agent-for-linux"></a>Linux için aracıyı yükleme

Aşağıdaki adımlar Azure’da ve Azure Kamu bulutunda Log Analytics için aracıyı yapılandırır.  

>[!NOTE]
>Linux için Log Analytics aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz.  

Linux bilgisayarınızın bir ara sunucu üzerinden Log Analytics’le iletişim kurması gerekiyorsa, ara sunucu yapılandırması komut satırında `-p [protocol://][user:password@]proxyhost[:port]` eklenerek belirtilebilir.  *proxyhost* özelliği, ara sunucunun tam etki adı alanı veya IP adresini kabul eder. 

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

1. Linux bilgisayarını Log Analytics çalışma alanına bağlanacak şekilde yapılandırmak için, çalışma alanı kimliğini ve birincil anahtarı daha önce kopyalayan aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    Aşağıdaki komut, `-p` proxy sunucunuz tarafından kimlik doğrulaması gerektiğinde proxy parametresini ve örnek sözdizimini içerir:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Linux bilgisayarını Azure Genel Bulut'taki Log Analytics çalışma alanına bağlayacak şekilde yapılandırmak için, daha önce kopyalanan çalışma alanı kimliğini ve birincil anahtarı sağlayan aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    Aşağıdaki komut, `-p` proxy sunucunuz tarafından kimlik doğrulaması gerektiğinde proxy parametresini ve örnek sözdizimini içerir:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```

3. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama

Azure Monitor, linux syslog'undan olayları ve daha uzun vadeli analiz ve raporlama için belirttiğiniz performans sayaçlarını toplayabilir. Belirli bir koşulu algıladığında da harekete geçebilir. Linux Syslog’dan olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

1. Azure portalının sol alt köşesinde **Daha Fazla hizmet**seçin. Arama kutusuna, **Günlük Analizi'ni**girin. Siz yazarken, girişinize göre liste filtreler. **Günlük Analizi çalışma alanlarını**seçin.

2. **Veri'yi**seçin ve ardından **Syslog'u**seçin.  

3. Günlüğün adını yazarak syslog eklersiniz. **Syslog** girin ve sonra **+** artı işareti seçin.  

4. Tabloda, **Bilgiler**, **Bildirim** ve **Hata Ayıklama** önem derecelerinin işaretini kaldırın. 

5. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

6. Bir Linux bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Linux Performans Verileri**’ni seçin. 

7. Yeni bir Log Analytics çalışma alanı için Linux Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir.

    ![Azure Monitör'de varsayılan Linux performans sayaçları seçildi](media/quick-collect-azurevm/linux-perfcounters-azure-monitor.png)

    **Makinelerime aşağıdaki yapılandırmayı uygula'yı** seçin ve **ardından seçili performans sayaçlarını ekle'yi**seçin. Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.  

8. Yapılandırmayı kaydetmek için sayfanın üst kısmında **Kaydet'i** seçin.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin

Veri toplamayı etkinleştirdiyseniz, şimdi hedef bilgisayardan verileri görmek için basit bir günlük araması örneği çalıştıralım.  

1. Seçili çalışma alanında, sol bölmeden **Günlükler'i**seçin.

2. Günlükler sorgu sayfasında, `Perf` sorgu düzenleyicisini yazın ve **Çalıştır'ı**seçin.
 
    ![Log Analytics günlük arama](media/quick-collect-windows-computer/log-analytics-portal-queryexample.png)

    Örneğin, aşağıdaki resimdeki sorgu 10.000 Performans kaydı döndürdü. Sonuçlarınız önemli ölçüde daha az olacaktır.

    ![Log Analytics günlük araması sonucu](media/quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, aracıyı Linux bilgisayardan kaldırıp Log Analytics çalışma alanını silebilirsiniz.  

Aracıyı kaldırmak için Linux bilgisayarında aşağıdaki komutu çalıştırın. *--temizleme* bağımsız değişkeni, aracıyı ve yapılandırmasını tamamen kaldırır.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

Çalışma alanını silmek için, daha önce oluşturduğunuz Günlük Analizi çalışma alanını seçin ve kaynak sayfasında **Sil'i**seçin.

![Log Analytics kaynağını silme](media/quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi şirket içi Linux bilgisayarınızdan işletimsel verileri ve performans verilerini topluyorsunuz ve *ücretsiz* olarak topladığınız verileri kolayca keşfetmeye, analiz etmeye ve verilerde işlem gerçekleştirmeye başlayabilirsiniz.  

Verileri görüntüleme ve analiz etmeyi öğrenmek için, öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](../../azure-monitor/learn/tutorial-viewdata.md)
