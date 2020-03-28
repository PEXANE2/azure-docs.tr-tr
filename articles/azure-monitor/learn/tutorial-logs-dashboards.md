---
title: Azure Log Analytics verilerinin panolarını oluşturma ve paylaşma | Microsoft Docs
description: Bu öğretici, Log Analytics panolarının kaydedilen tüm günlük sorgularınızı nasıl görselleştirebileceğini anlamanıza yardımcı olur ve ortamınızı görüntülemek için tek bir lens sağlar.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 06/19/2019
ms.custom: mvc
ms.openlocfilehash: 76ba79561df4a75004369d24c4c6af82de9b1cfc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77661541"
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Log Analytics verilerinin panolarını oluşturma ve paylaşma

Günlük Analizi panoları, kaydedilen günlük sorgularınızın tümünün görselleştirilmesini sağlayarak kuruluştaki BT operasyonel verilerini bulma, ilişkilendirme ve paylaşma olanağı sağlar.  Bu öğretici, BT işlemleri destek ekibiniz tarafından erişilecek paylaşılan bir panodesteklemek için kullanılacak bir günlük sorgusu oluşturmayı kapsar.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure portalında paylaşılan bir pano oluşturma
> * Performans günlüğü sorgusunda görselleştirme 
> * Paylaşılan panoya günlük sorgusu ekleme 
> * Paylaşılan bir panodaki kutucuğu özelleştirme

Bu öğreticideki örneği tamamlamak için [Log Analytics çalışma alanına bağlı](quick-collect-azurevm.md) mevcut bir sanal makinenizin olması gerekir.  
 
## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç. 

## <a name="create-a-shared-dashboard"></a>Paylaşılan pano oluşturma
Varsayılan [panonuzu](../../azure-portal/azure-portal-dashboards.md)açmak için **Pano'yu** seçin. Panonuz aşağıdaki örnekten farklı görünecektir.

![Azure portalı panosu](media/tutorial-logs-dashboards/log-analytics-portal-dashboard.png)

Burada tüm Azure kaynaklarınız arasında Azure Log Analytics telemetri verileri gibi BT için en önemli olan işlem verilerini bir araya getirebilirsiniz.  Bir günlük sorgusunun görselleştirilmesine geçmeden önce bir pano oluşturup paylaşalım.  Daha sonra, bir çizgi grafiği olarak işleyecek olan örnek performans günlüğü sorgumuza odaklanıp panoya ekleyebiliriz.  

Bir pano oluşturmak için geçerli pano adının yanındaki **Yeni pano** düğmesini seçin.

![Azure portalında yeni pano oluşturma](media/tutorial-logs-dashboards/log-analytics-create-dashboard-01.png)

Bu işlem yeni, boş ve özel bir pano oluşturur ve panonuzu adlandırıp kutucuklar ekleyebileceğiniz ya da yeniden düzenleyebileceğiniz özelleştirme modunu açar. Pano adını edin ve bu öğretici için *Örnek Pano* belirtin ve ardından **Özelleştirme bitti'yi**seçin.<br><br> ![Özelleştirilmiş Azure panosunu kaydetme](media/tutorial-logs-dashboards/log-analytics-create-dashboard-02.png)

Bir pano oluşturulduğunda varsayılan olarak gizlidir, yani onu yalnızca siz görebilirsiniz. Panoyu başkalarının görebilmesi için, diğer pano komutlarıyla birlikte görünen **Paylaş** düğmesini kullanın.

![Azure portalında yeni pano paylaşma](media/tutorial-logs-dashboards/log-analytics-share-dashboard.png) 

Panonuzun yayımlanması için bir abonelik ve kaynak grubu seçmeniz istenir. Kolaylık olması için portalın yayımlama deneyimi, bir kaynak grubuna panoları yerleştirdiğiniz **panolar** adlı bir modelde size kılavuzluk eder.  Seçili olan aboneliği doğrulayın ve sonra **Yayımla**’ya tıklayın.  Panoda gösterilen bilgilere erişim, [Azure Kaynak Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md) ile denetlenir.   

## <a name="visualize-a-log-query"></a>Günlük sorgusunun görselleştirilmesi
[Log Analytics,](../log-query/get-started-portal.md) günlük sorguları ve sonuçlarıyla çalışmak için kullanılan özel bir portaldır. Birden fazla satırda sorgu düzenleme, seçerek kod yürütme, bağlama duyarlı IntelliSense ve Akıllı Analiz özellikleri mevcuttur. Bu öğreticide, grafik biçiminde bir performans görünümü oluşturmak, gelecekteki bir sorguya kaydetmek ve daha önce oluşturulan paylaşılan panoya sabitlemek için Log Analytics'i kullanacaksınız.

Azure Monitor menüsündeki **Günlükler'i** seçerek Günlük Analitiği'ni açın. Yeni bir boş sorguyla başlar.

![Giriş sayfası](media/tutorial-logs-dashboards/homepage.png)

Bilgisayar ve TimeGenerated tarafından gruplanan ve görsel bir grafikte görüntülenen, hem Windows hem de Linux bilgisayarları için işlemci kullanım kayıtlarını döndürmek için aşağıdaki sorguyu girin. Sorguyu çalıştırmak ve ortaya çıkan grafiği görüntülemek için **Çalıştır'ı** tıklatın.

```Kusto
Perf 
| where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" 
| summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1hr), Computer 
| render timechart
```

Sayfanın üst kısmından **Kaydet** düğmesini seçerek sorguyu kaydedin.

![Sorguyı kaydet](media/tutorial-logs-dashboards/save-query.png)

**Sorguyu Kaydet** denetim panelinde, *Azure VM'ler - İşlemci Kullanımı* gibi bir ad ve *Panolar* gibi bir kategori sağlayın ve ardından **Kaydet'i**tıklatın.  Bu şekilde, kullanabileceğiniz ve değiştirebileceğiniz ortak sorgulardan oluşan bir kitaplık oluşturabilirsiniz.  Son olarak, bunu sayfanın sağ üst köşesinden **panoya Pin'i** seçip pano adını seçerek daha önce oluşturulan paylaşılan panoya sabitleyin.

Artık panoya sabitlenmiş bir sorgumuz olduğuna göre, genel bir başlığının ve altında bir yorumun olduğunu fark edeceksiniz.

![Azure panosu örneği](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-01.png)

 Bu panoya, görüntüleyenler tarafından kolayca anlaşılacak, anlamlı bir ad vermeliyiz.  Döşemenin başlığını ve altyazısını özelleştirmek için edit düğmesini tıklatın ve ardından **Güncelleştir'i**tıklatın.  Değişiklikleri yayımlamanızı veya atmanızı isteyen bir başlık görüntülenir.  **Kopyayı Kaydet'i**tıklatın.  

![Örnek pano yapılandırması tamamlandı](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Azure portalında bir pano oluşturmayı ve ona bir günlük sorgusu eklemeyi öğrendiniz.  Günlük sorgusu sonuçlarına göre uygulayabileceğiniz farklı yanıtları öğrenmek için bir sonraki öğreticiye ilerleyin.  

> [!div class="nextstepaction"]
> [Log Analytics Uyarıları ile etkinliklere yanıt verin](tutorial-response.md)
