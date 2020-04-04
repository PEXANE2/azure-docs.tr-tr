---
title: Azure Monitör ile karma Windows bilgisayarından veri toplama
description: Bu hızlı başlangıçta, Azure dışında çalışan Windows bilgisayarları için Log Analytics aracısını nasıl dağıtabileceğinizi ve Azure Monitör Günlükleri ile veri toplamayı nasıl etkinleştireceğinizi öğreneceksiniz.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 08/22/2019
ms.author: bwren
ms.custom: mvc, seo-javascript-october2019
ms.openlocfilehash: 6435535ee14450c590a79ea68c8855c29df8bd9a
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637877"
---
# <a name="collect-data-from-a-windows-computer-in-a-hybrid-environment-with-azure-monitor"></a>Azure Monitor ile karma ortamda bir Windows bilgisayarından veri toplama

[Azure Monitor,](../overview.md) ortamınızdaki fiziksel veya sanal Windows bilgisayarlarınızdan doğrudan veri toplayıp ayrıntılı analiz ve korelasyon için Log Analytics çalışma alanına aktarabilir. Log [Analytics aracısının](../platform/log-analytics-agent.md) yüklenmesi, Azure Monitor'un veri merkezinden veya başka bir bulut ortamından veri toplamasına olanak tanır. Bu hızlı başlangıçta birkaç kolay adımda Windows bilgisayarınızı nasıl yapılandırabileceğiniz ve veri toplayabileceğiniz gösterilmektedir. Azure Windows SANAL MM'leri hakkında bilgi [için](../../azure-monitor/learn/quick-collect-azurevm.md)bkz.  

Desteklenen yapılandırmayı anlamak için [bkz.](../../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems) [Network firewall configuration](../../azure-monitor/platform/log-analytics-agent.md#network-requirements)
 
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

1. Azure portalında **Tüm hizmetler'i**seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.

    ![Azure portal](media/quick-collect-azurevm/azure-portal-log-analytics-workspaces.png)<br>  

2. **Oluştur'u**seçin ve ardından aşağıdaki öğeler için seçenekleri seçin:

   * *Varsayılan LAWorkspace*gibi yeni **Log Analytics çalışma alanı**için bir ad sağlayın.  
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak Grubu** için, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
   * VM’lerinizin dağıtıldığı **Konum**’u seçin.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan’dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir EA kaydına bağlı aboneliğe yönelik çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).
  
        ![Log Analytics kaynak bıçağı oluşturun](media/quick-collect-azurevm/create-log-analytics-workspace-azure-portal.png) 

3. **Log Analytics çalışma alanı** bölmesi üzerinde gerekli bilgileri sağladıktan sonra **Tamam'ı**seçin.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 


## <a name="get-the-workspace-id-and-key"></a>Çalışma alanı kimliğini ve anahtarı nı alın

Windows için Log Analytics aracısını (Microsoft İzleme Aracısı (MMA) olarak da adlandırılır) yüklemeden önce, Log Analytics çalışma alanınız için çalışma alanı kimliğine ve anahtarına ihtiyacınız vardır. Kurulum sihirbazı aracıyı düzgün bir şekilde yapılandırmak ve Azure Monitor ile iletişim kurabilmesini sağlamak için bu bilgilere ihtiyaç duyar.  

1. Azure portalının sol üst köşesinde **Tüm hizmetler'i**seçin. Arama kutusuna, **Günlük Analizi'ni**girin. Siz yazarken, girişinize göre liste filtreler. **Günlük Analizi çalışma alanlarını**seçin.

2. Günlük Analizi çalışma alanları listenizde, daha önce oluşturduğunuz çalışma alanını seçin. (Buna **DefaultLAWorkspace**adını vermiş olabilirsiniz.)

3. **Gelişmiş ayarları**seçin:

    ![Log Analytics gelişmiş ayarları](media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png)
  
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Windows Sunucuları**’nı seçin.

5. Değerleri **Çalışma Alanı Kimliği** ve Birincil **Anahtar'ın**sağında kopyalayın. En sevdiğiniz editöre yapıştırın.

## <a name="install-the-agent-for-windows"></a>Windows için aracıyı yükleme

Aşağıdaki adımlar, Azure ve Azure Kamu'da Günlük Analitiği aracısını yükler ve yapılandırır. Aracıyı bilgisayarınıza yüklemek için Microsoft İzleme Aracısı Kurulumu programını kullanırsınız.

1. Önceki adım kümesinden devam eden **Windows Sunucular** sayfasında, indirmek istediğiniz Windows Agent sürümünü **karşıdan yükleyin'** i seçin. Windows işletim sisteminizin işlemci mimarisi için uygun sürümü seçin.

2. Aracıyı bilgisayarınıza yüklemek için Kurulum'u çalıştırın.

3. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.

4. **Lisans Koşulları** sayfasında, lisansı okuyun ve sonra **Kabul Ediyorum**’u seçin.

5. **Hedef Klasör** sayfasında, varsayılan yükleme klasörünü değiştirin veya aynı şekilde bırakın ve daha sonra **İleri**’yi seçin.

6. Aracı **Kurulum Seçenekleri** sayfasında aracıyı Azure Günlük Analizi'ne bağlayın ve **ardından İleri'yi**seçin.

7. Azure **Günlük Analizi** sayfasında aşağıdaki adımları tamamlayın:

   1. Daha önce kopyaladığınız **Çalışma Alanı Kimliği** ve Çalışma **Alanı Anahtarına (Birincil Anahtar)** yapıştırın. Bilgisayar Azure Kamu'da bir Günlük Analizi çalışma alanına rapor verirse, **Azure Bulut** listesinde Azure ABD **Hükümeti'ni** seçin.  
   2. Bilgisayarın, Log Analytics hizmetiyle bir ara sunucu üzerinden iletişim kurması gerekiyorsa **Gelişmiş** seçeneğini belirleyip ara sunucunun URL’sini ve bağlantı noktası numarasını sağlayın. Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, proxy sunucusuyla kimlik doğrulaması için kullanıcı adını ve parolayı girin ve **ardından İleri'yi**seçin.  

8. Yapılandırma ayarlarını ekledikten sonra **İleri'yi** seçin:

    ![Microsoft İzleme Aracısı Kurulumu](media/quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)

9. **Yüklemeye Hazır** sayfasında, tercihlerinizi gözden geçirip **Yükle**’yi seçin.

10. **Başarıyla tamamlanan Yapılandırma** sayfasında **Finish'i**seçin.

Yükleme ve kurulum tamamlandığında, Microsoft İzleme Aracısı Denetim Masası'nda görünür. Yapılandırmanızı gözden geçirebilir ve aracının Log Analytics çalışma alanına bağlı olduğunu doğrulayabilirsiniz. Bağlı olduğunuzda, **Azure Log Analytics** sekmesinde, aracı şu iletiyi görüntüler: **Microsoft İzleme Aracısı Microsoft Log Analytics hizmetine başarıyla bağlanmıştır.**<br><br> ![MMA bağlantı durumu](media/quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama

Azure Monitor, windows olay günlüğünden belirttiğiniz olayları ve daha uzun vadeli analiz ve raporlama için performans sayaçlarını toplayabilir. Belirli bir koşulu algıladığında da harekete geçebilir. Windows olay günlüğünden olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

1. Azure portalının sol alt köşesinde **Daha Fazla hizmet**seçin. Arama kutusuna, **Günlük Analizi'ni**girin. Siz yazarken, girişinize göre liste filtreler. **Günlük Analizi çalışma alanlarını**seçin.

2. **Gelişmiş ayarları**seçin:

    ![Log Analytics gelişmiş ayarları](media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png)
 
3. **Veri**’yi seçin ve ardından **Windows Olay Günlükleri**’ni seçin.  

4. Günlüğün adını girerek bir olay günlüğü eklersiniz. **Sisteme**girin ve sonra artı**+** işaretini seçin ( ).  

5. Tabloda **Hata** ve **Uyarı** önemderecelerini seçin.

6. Sayfanın üst kısmında **Kaydet'i** seçin.

7. Windows bilgisayarda performans sayaçları toplanmasını etkinleştirmek için **Windows Performans Sayaçları'nı** seçin.

8. Yeni bir Log Analytics çalışma alanı için Windows performans sayaçlarını ilk yapılandırdığınızda, size birkaç ortak sayaç oluşturma seçeneği verilir. Her seçenek, yanında bir onay kutusu ile listelenir:

    ![Windows performans sayaçları](media/quick-collect-windows-computer/windows-perfcounters-default.png).
    
    **Seçili performans sayaçlarını ekle'yi**seçin. Sayaçlar eklenir ve on saniyelik bir toplama örnek aralığı ile önceden ayarlanır.

9. Sayfanın üst kısmında **Kaydet'i** seçin.

## <a name="view-collected-data"></a>Toplanan verileri görüntüleme

Artık veri toplamayı etkinleştirdiğinize göre, hedef bilgisayardaki bazı verileri görmek için basit bir günlük araması çalıştıralım.  

1. Seçili çalışma alanında, sol bölmeden **Günlükler'i**seçin.

2. Günlükler sorgu sayfasında, `Perf` sorgu düzenleyicisini yazın ve **Çalıştır'ı**seçin.
 
    ![Log Analytics günlük arama](media/quick-collect-windows-computer/log-analytics-portal-queryexample.png)

    Örneğin, bu resimdeki sorgu 10.000 Performans kaydı döndürdü. Sonuçlarınız önemli ölçüde daha az olacaktır.

    ![Log Analytics günlük araması sonucu](media/quick-collect-azurevm/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aracıyı bilgisayarınızdan kaldırabilir ve artık ihtiyacınız yoksa Log Analytics çalışma alanını silebilirsiniz.  

Aracıyı kaldırmak için aşağıdaki adımları tamamlayın:

1. Denetim Masası'nı açın.

2. **Programlar ve Özellikler**'i açın.

3. **Programlar ve Özellikler'de**Microsoft **Monitoring Agent'ı** seçin ve ardından **Kaldır'ı**seçin.

Daha önce oluşturduğunuz Log Analytics çalışma alanını silmek için, onu seçin ve kaynak sayfasında **Sil'i**seçin:

![Günlük Analizi çalışma alanını silme](media/quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık Windows bilgisayarınızdan operasyonel ve performans verileri topladığınıza göre, topladığınız verileri *ücretsiz*olarak keşfetmeye, analiz etmeye ve hareket etmeye kolayca başlayabilirsiniz.  

Verileri görüntülemeyi ve analiz etmeyi öğrenmek için öğreticiye devam edin:

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](tutorial-viewdata.md)
