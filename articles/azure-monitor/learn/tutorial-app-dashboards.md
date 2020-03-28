---
title: Azure Application Insights’ta özel panolar oluşturma | Microsoft Docs
description: Azure Application Insights’ı kullanarak özel KPI panoları oluşturma öğreticisi.
ms.subservice: application-insights
ms.topic: tutorial
author: lgayhardt
ms.author: lagayhar
ms.date: 07/3/2019
ms.custom: mvc
ms.openlocfilehash: caf8b1899cad95ade6297e78e8f2cf35939ef189
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77661643"
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Azure Application Insights’ı kullanarak özel KPI panoları oluşturma

Azure portalında, her biri farklı kaynak gruplarında ve aboneliklerde yer alan çeşitli Azure kaynaklarındaki verilerin görselleştirildiği kutucular içeren birden çok pano oluşturabilirsiniz.  Uygulamanızın durumunu ve performansını tam olarak gösteren özel panolar oluşturmak için, Azure Application Insights’daki farklı grafikleri ve görünümleri sabitleyebilirsiniz. Bu öğretici, Azure Application Insights’taki çeşitli veri ve görselleştirme türlerini içeren özel bir pano oluşturma konusunda size rehberlik etmektedir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure’da özel bir pano oluşturma
> * Kutucuk Galerisinden kutucuk ekleme
> * Application Insights’taki standart ölçümleri panoya ekleme
> * Özel bir Application Insights ölçüm grafiğini panoya ekleme
> * Günlükler (Analytics) sorgusunun sonuçlarını panoya ekleme



## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- Azure’a .NET uygulaması dağıtma ve [Application Insights SDK’sını etkinleştirme](../../azure-monitor/app/asp-net.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma
Tek bir pano çeşitli uygulamalardan, kaynak gruplarından ve aboneliklerden gelen kaynakları içerebilir.  Uygulamanız için yeni bir pano oluşturarak öğreticiyi kullanamaya başlayın.  

1. Pano bölmesine Yeni **pano'yu**seçin.

   ![Yeni pano](media/tutorial-app-dashboards/1newdashboard.png)

1. Pano için bir ad yazın.
1. Panonuza ekleyebileceğiniz kutucuk çeşitleri için **Kutucuk Galerisi**’ne göz atın.  Galeriden kutucuk eklemenin yanı sıra, grafikleri ve diğer görünümleri doğrudan Application Insights'tan panoya sabitleyebilirsiniz.
1. **Markdown** kutucuğunu bulun ve panonuza sürükleyin.  Bu döşeme, panonuza açıklayıcı metin eklemek için ideal olan işaretleme biçiminde biçimlendirilmiş metin eklemenize olanak tanır.
1. Metni kutucuğun özelliklerine ekleyin ve pano tuvalinizde yeniden boyutlandırın.
    
    ![Markdown kutucuğunu düzenleme](media/tutorial-app-dashboards/2dashboard-text.png)

1. Döşeme özelleştirme modundan çıkmak için ekranın üst kısmında **özelleştirme bitti'yi** tıklatın.

## <a name="add-health-overview"></a>Sistem durumuna genel bakış ekleme
Statik metiniçeren bir pano çok ilginç değildir, bu nedenle uygulamanız hakkında bilgi göstermek için uygulama istatistiklerinden bir döşeme ekleyin.  Kutucuk Galerisi’nden Application Insights kutucukları ekleyebilir veya bunları Application Insights ekranlarından doğrudan sabitleyebilirsiniz.  Bu, bildiğiniz grafikleri ve görünümleri panonuza sabitlemeden önce yapılandırmanıza olanak tanır.  İlk olarak uygulamanız için sistem durumuna standart genel bakışı ekleyin.  Bu işlem yapılandırma gerektirmez ve panoda çok az özelleştirme yapmaya izin verir.


1. Ana ekranda **Application Insights** kaynağınızı seçin.
2. Genel **Bakış** bölmesinde, görüntülediğiniz](media/tutorial-app-dashboards/pushpin.png) son panoya döşemeyi eklemek için pin simgesi ![simgesini tıklatın.  
 
3. Sağ üstte, döşemenizin panonuza sabitlenmiş olduğuna dair bir bildirim görüntülenir. Panonuza dönmek veya pano bölmesini kullanmak için bildirimde **panoya Sabitlenmiş'i** tıklatın.
4. Bu döşeme artık panonuza eklenir. Döşemenin konumunu değiştirmek için **Edit'i** seçin. Tıklatın ve konuma sürükleyin ve sonra **Özelleştirme Bitti'yi**tıklatın. Artık panonuz yararlı bilgiler içeren bir kutucuğa sahip olur.

    ![Zaman çizelgesine genel bakış içeren pano](media/tutorial-app-dashboards/4dashboard-edit.png)

## <a name="add-custom-metric-chart"></a>Özel ölçüm grafiği ekleme
**Ölçümler** paneli, isteğe bağlı filtreler ve gruplandırma ile zaman içinde Application Insights tarafından toplanan bir ölçüm grafiği oluşturmanıza olanak tanır.  Application Insights’taki her öğe gibi bu grafiği de panoya ekleyebilirsiniz.  Bu işlem öncelikle birkaç basit özelleştirme gerektirir.

1. Ana ekranda **Uygulama Öngörüleri** kaynağınızı seçin.
1. **Ölçümler**’i seçin.  
2. Boş bir grafik oluşturulur ve bir ölçüm eklemeniz istenir.  Grafiğe bir ölçüm ve isteğe bağlı olarak bir filtre ve gruplandırma ekleyin.  Aşağıdaki örnekte başarı ölçütüne göre gruplandırılmış sunucu isteklerinin sayısı gösterilmektedir.  Bu, başarılı ve başarısız isteklerin sürekli bir görünümünü sunar.

    ![Ölçüm ekleme](media/tutorial-app-dashboards/metrics.png)

4. Sağdaki **panoya Pin'i** seçin. Bu, görünümü üzerinde çalıştığınız en son panoya ekler.

3.  Sağ üstte, döşemenizin panonuza sabitlenmiş olduğuna dair bir bildirim görüntülenir. Panonuza dönmek veya pano bıçağını kullanmak için bildirimde **panoya Sabitlenmiş'i** tıklatın.

4. Bu döşeme artık panonuza eklenir. Döşemenin konumunu değiştirmek için **Edit'i** seçin. Tıklatın ve konuma sürükleyin ve sonra **Özelleştirme Bitti'yi**tıklatın.

## <a name="add-logs-analytics-query"></a>Günlükekle (Analytics) sorgusu
Azure Application Insights Logs (Analytics), toplanan tüm Uygulamaları Öngörüler'i analiz etmenizi sağlayan zengin bir sorgu dili sağlar. Grafikler ve diğer görünümler gibi, günlük sorgusunun çıktısını panonuza ekleyebilirsiniz.

Azure Uygulamaları Öngörügünlükleri (Analytics) ayrı bir hizmet olduğundan, günlük sorgusu eklemek için panonuzu paylaşmanız gerekir. Bir Azure panosu paylaştığınızda, onu diğer kullanıcılar ve kaynaklar için kullanılabilir hale getirebilecek bir Azure kaynağı olarak yayımlarsınız.  

1. Pano ekranının üst kısmındaki **Paylaş** seçeneğine tıklayın.

    ![Panoyu yayımlama](media/tutorial-app-dashboards/8dashboard-share.png)

2. Panoyu paylaşmak için **Pano adını** aynı tutun ve **Abonelik Adı**’nı.  **Yayımla**’ta tıklayın.  Pano artık diğer hizmetler ve abonelikler tarafından kullanılabilir.  İsteğe bağlı olarak panoya erişebilecek belirli kullanıcıları tanımlayabilirsiniz.
1. Ana ekranda **Uygulama Öngörüleri** kaynağınızı seçin.
2. Günlükler (Analytics) portalını açmak için izleme altında solda **Günlükler (Analytics)** seçeneğini tıklayın.
3. En çok istekte bulunulan 10 sayfayı ve bunlara ait istek sayılarını döndüren şu sorguyu yazın:

    ``` Kusto
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10
    ```

4. Sorgunun sonuçlarını doğrulamak için **Çalıştır'ı** tıklatın.
5. Pin simgesine tıklayın ![raptiye](media/tutorial-app-dashboards/pushpin.png) ve panonunuzun adını seçin. Bu seçeneğin, son panonun kullanıldığı önceki adımlardan farklı olarak bir pano seçmenize sahip olmasının nedeni, Günlükler (Analytics) konsolunun ayrı bir hizmet olması ve kullanılabilir tüm paylaşılan panolardan seçim yapması dır.

5. Panoya geri dönmeden önce, başka bir sorgu ekleyin, ancak bu kez bir panüste günlük sorgusunu görselleştirmenin farklı yollarını görmek için grafik olarak işleyin. En çok istisnayı içeren ilk 10 işlemi özetleyen aşağıdaki sorguyu kullanın.

    ``` Kusto
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10
    ```

6. **Grafik** seçeneğini belirleyin ve ardından çıktıyı görselleştirmek için seçimi **Halka** olarak değiştirin.

    ![Günlükleri (Analytics) grafiği](media/tutorial-app-dashboards/11querychart.png)

6. Pin simgesine tıklayın ![raptiye](media/tutorial-app-dashboards/pushpin.png) grafiği panonuza sabitlemek için sağ üstte ve bu kez panonuza dönmek için bağlantıyı seçin.
4. Sorguların sonuçları seçtiğini biçimde panonuza eklenir.  Tıklatıp her biri konuma sürükleyin ve sonra **Özelleştirme bitti'yi**tıklatın.
5. Kalem simgesini seçin ![Kalem simgesi](media/tutorial-app-dashboards/pencil.png) her başlık onları açıklayıcı bir başlık vermek.

5. Uygulama Öngörüleri'nden çeşitli grafikler ve görseller içeren değişikliklerinizi panonuzda yeniden yayınlamak için **Paylaş'ı** seçin.


## <a name="next-steps"></a>Sonraki adımlar
Artık nasıl özel pano oluşturulacağını öğrendiniz. Bir örnek olay içeren diğer Application Insights belgelerine de göz atabilirsiniz.

> [!div class="nextstepaction"]
> [Derin tanılama](../../azure-monitor/app/devops.md)
