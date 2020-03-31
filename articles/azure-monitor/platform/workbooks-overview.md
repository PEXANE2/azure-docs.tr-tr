---
title: Azure İzleyici Çalışma Kitapları’na Genel Bakış
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
author: mrbullwinkle
manager: carmonm
services: azure-monitor
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 29e675f3ae35df9211f58d45ad8450566d67a588
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658243"
---
# <a name="azure-monitor-workbooks"></a>Azure Monitör Çalışma Kitapları

Çalışma kitapları, Veri analizi ve Azure portalı içinde zengin görsel raporlar oluşturulması için esnek bir tuval sağlar. Azure'un her yerinden birden fazla veri kaynağına girmenize ve bunları birleşik etkileşimli deneyimlerde birleştirmenize olanak tanır. 

## <a name="data-sources"></a>Veri kaynakları

Çalışma kitapları Azure içindeki birden çok kaynaktan gelen verileri sorgulayabilir. Çalışma kitaplarının yazarları, temel bileşenlerin kullanılabilirliği, performansı, kullanımı ve genel durumu hakkında öngörüler sağlamak için bu verileri dönüştürebilir. Örneğin, yüksek CPU veya düşük bellek örneklerini tanımlamak için sanal makinelerdeki performans günlüklerini çözümlemek ve sonuçları etkileşimli bir raporda ızgara olarak görüntülemek.
  
Ama çalışma kitaplarının gerçek gücü, birbirinden farklı kaynaklardan gelen verileri tek bir rapor içinde birleştirebilme yeteneğidir. Bu, bileşik kaynak görünümlerinin oluşturulmasına veya kaynaklar arasında birleşerek daha zengin veri ve öngörüler oluşturmanın imkansız olmasını sağlar.

Çalışma kitapları şu anda aşağıdaki veri kaynaklarıyla uyumludur:

* [Günlükler](workbooks-data-sources.md#logs)
* [Ölçümler](workbooks-data-sources.md#metrics)
* [Azure Kaynak Grafiği](workbooks-data-sources.md#azure-resource-graph)
* [Uyarılar (Önizleme)](workbooks-data-sources.md#alerts-preview)
* [İş Yükü Durumu (Önizleme)](workbooks-data-sources.md#workload-health-preview)
* [Azure Kaynak Durumu (Önizleme)](workbooks-data-sources.md#azure-resource-health)
* [Azure Veri Gezgini (Önizleme)](workbooks-data-sources.md#azure-data-explorer-preview)

## <a name="visualizations"></a>Görsel öğeler

Çalışma kitapları, verilerinizi görselleştirmek için zengin bir yetenek kümesi sağlar. Her görselleştirme türünün ayrıntılı örnekleri için aşağıdaki örnek bağlantılara başvurabilirsiniz:

* [Metin](workbooks-visualizations.md#text)
* [Grafikler](workbooks-visualizations.md#charts)
* [Kılavuzlar](workbooks-visualizations.md#grids)
* [Fayans](workbooks-visualizations.md#tiles)
* [Ağaç](workbooks-visualizations.md#trees)
* [Grafik](workbooks-visualizations.md#graphs)

![Örnek çalışma kitabı görselleştirmeleri](./media/workbooks-overview/visualizations.png)

## <a name="getting-started"></a>Başlarken

Çalışma kitapları deneyimini keşfetmek için önce Azure Monitör hizmetine gidin. Bu, Azure portalındaki arama kutusuna **Monitör** yazarak yapılabilir.

Ardından **Çalışma Kitapları 'nı (önizleme)** seçin.

![Çalışma Kitapları önizleme düğmesinin ekran görüntüsü kırmızı bir kutuda vurgulanır](./media/workbooks-overview/workbooks-preview.png)

### <a name="gallery"></a>Galeri

Bu, sizi çalışma kitapları galerisine götürür:

![Azure Monitor çalışma kitapları galeri görünümü ekran görüntüsü](./media/workbooks-overview/gallery.png)

### <a name="workbooks-versus-workbook-templates"></a>Çalışma kitapları ve çalışma kitabı şablonları

Yeşil renkte bir _çalışma kitabı_ ve mor renkte bir dizi çalışma _kitabı şablonu_ görebilirsiniz. Şablonlar, birden çok kullanıcı ve takım tarafından esnek yeniden kullanım için tasarlanmış küratörlü raporlar olarak hizmet verir. Şablonaçmak, şablonun içeriğiyle dolu geçici bir çalışma kitabı oluşturur. 

Şablon tabanlı çalışma kitabının parametrelerini ayarlayabilir ve iş arkadaşlarınızın gelecekteki raporlama deneyimini bozma korkusu olmadan analiz gerçekleştirebilirsiniz. Bir şablon açarsanız, bazı ayarlamalar yaparsanız ve sonra şablonu çalışma kitabı olarak kaydedeceğiniz kaydet simgesini seçerseniz, özgün şablona dokunulmadan yeşil olarak gösterilir. 

Kaputun altında, şablonlar kaydedilmiş çalışma kitaplarından da farklıdır. Çalışma kitabının kaydedilmesi ilişkili bir Azure Kaynak Yöneticisi kaynağı oluştururken, şablonu açarken oluşturulan geçici çalışma kitabının bununla ilişkili benzersiz bir kaynağı yoktur. Çalışma kitaplarında erişim denetiminin nasıl yönetildiği hakkında daha fazla bilgi edinmek için [çalışma kitapları erişim denetimi makalesine başvurun.](workbooks-access-control.md)

### <a name="exploring-a-workbook-template"></a>Çalışma kitabı şablonu keşfetme

Varsayılan uygulama çalışma kitabı şablonlarından birini görmek için Uygulama Hatası Çözümlemesi'ni seçin. **Application Failure Analysis**

![Uygulama hatası analizi şablonunun ekran görüntüsü](./media/workbooks-overview/failure-analysis.png)

Daha önce belirtildiği gibi, şablonu açmak, etkileşimde olabildiğiniz geçici bir çalışma kitabı oluşturur. Varsayılan olarak, çalışma kitabı yalnızca özgün şablon yazarı tarafından oluşturulan amaçlanan çözümleme deneyimine ait bilgileri görüntüleyen okuma modunda açılır.

Bu özel çalışma kitabında, deneyim etkileşimlidir. Aboneliği, hedeflenen uygulamaları ve görüntülemek istediğiniz verilerin zaman aralığını ayarlayabilirsiniz. Bu seçimleri yaptıktan sonra HTTP İstekleri'nin ızgarası da etkileşimlidir ve tek bir satır seçilirken, raporun alt kısmındaki iki grafikte hangi verilerin işlendiği değiştirilir.

### <a name="editing-mode"></a>Düzenleme modu

Bu çalışma kitabı şablonunun nasıl bir araya getirilmiş olduğunu **Edit**anlamak için Düzenle'yi seçerek düzenleme moduna değiştirmeniz gerekir. 

![Uygulama hatası analizi şablonunun ekran görüntüsü](./media/workbooks-overview/edit.png)

Düzenleme moduna geçtikten sonra, çalışma kitabınızın her bir yönüyle karşılık gelen sağa karşılık gelen bir dizi **Düzenle** kutusu fark edeceksiniz.

![Edit düğmesinin ekran görüntüsü](./media/workbooks-overview/edit-mode.png)

İstek verilerinin ızgarası altında hemen edit düğmesini seçersek, çalışma kitabımızın bu bölümünün Bir Application Insights kaynağından gelen verilere karşı bir Kusto sorgusundan oluştuğunu görebiliriz.

![Altta yatan Kusto sorgusunun ekran görüntüsü](./media/workbooks-overview/kusto.png)

Sağdaki diğer **Edit** düğmelerini tıklatmak, işaretleme tabanlı [metin kutuları,](workbooks-visualizations.md#text) [parametre seçimi](workbooks-parameters.md) kullanıcı arası öğeleri ve diğer [grafik/görselleştirme türleri](workbooks-visualizations.md)gibi çalışma kitaplarını oluşturan bir dizi temel bileşeni ortaya çıkarır. 

Önceden oluşturulmuş şablonları edit modunda keşfetmek ve bunları gereksinimlerinize uyacak şekilde değiştirmek ve kendi özel çalışma kitabınızı kaydetmek, Azure Monitor çalışma kitaplarıyla nelerin mümkün olduğunu öğrenmeye başlamak için mükemmel bir yoldur.

## <a name="pinning-visualizations"></a>Sabitleme Görselleştirmeleri

Çalışma kitabı pin modundayken bu öğelerin üzerindeki pin düğmesi kullanılarak veya çalışma kitabı yazarı, bu öğenin pin simgesini görünür hale getirmek için ayarları etkinleştirmişse, çalışma kitabındaki metin, sorgu ve metrik adımlar sabitlenebilir. 

Pin moduna erişmek için düzenleme moduna girmek için **Düzenle'yi** tıklatın ve üst çubuktaki mavi pin simgesini seçin. Daha sonra, ilgili her çalışma kitabı bölümünün ekranınızın sağ tarafındaki *Edit* kutusunun üzerinde tek bir pin simgesi görünür.

![Sabitleme deneyimi](./media/workbooks-overview/pin-experience.png)

> [!NOTE]
> Çalışma kitabının durumu pin sırasında kaydedilir ve panoya sabitlenmiş çalışma kitapları, temel çalışma kitabı değiştirildiğinde güncelleştirilmez. Sabitlenmiş bir çalışma kitabı bölümünü güncelleştirmek için, bu bölümü silmeniz ve yeniden sabitlemeniz gerekir.

## <a name="dashboard-time-ranges"></a>Pano zaman aralıkları

Sabitlenmiş öğe *Zaman Aralığı* parametresi kullanacak şekilde yapılandırılırsa, sabitlenmiş çalışma kitabı sorgu bölümleri panodaki zaman aralığına saygı gösterir. Gösterge tablosunun zaman aralığı değeri, zaman aralığı parametresinin değeri olarak kullanılır ve pano zaman aralığındaki herhangi bir değişiklik sabitlenmiş öğenin güncellenebilmesine neden olur. Sabitlenmiş bir parça panonun zaman aralığını kullanıyorsa, zaman aralığı değiştiğinde gösterge tablosunun zaman aralığını göstermek için sabitlenmiş parça güncelleştirmesinin altyazısını görürsünüz. 

Ayrıca, zaman aralığı parametresi kullanarak sabitlenmiş çalışma kitabı parçaları, gösterge panelinin zaman aralığıtarafından belirlenen bir hızda otomatik olarak yenilenir. Sorgu en son çalıştırılda sabitlenmiş bölümün alt başlığında görünür.

Sabitlenmiş bir adımda açıkça ayarlanmış bir zaman aralığı varsa (zaman aralığı parametresi kullanmıyorsa), pano ayarlarına bakılmaksızın bu zaman aralığı her zaman pano için kullanılır. Sabitlenmiş bölümün alt yazısı panodaki zaman aralığını göstermez ve sorgu panoda otomatik olarak yenilenmez. Altyazı, sorgunun en son ne zaman yürütüldeceğini gösterir.

> [!NOTE]
> *Birleştirme* veri kaynağını kullanan sorgular, panolara sabitlenirken şu anda desteklenmez.

## <a name="sharing-workbook-templates"></a>Çalışma kitabı şablonlarını paylaşma

Kendi çalışma kitabı şablonlarınızı oluşturmaya başladığınızda, şablonları daha geniş bir toplulukla paylaşmak isteyebilirsiniz. Daha fazla bilgi edinmek ve varsayılan Azure Monitor galerisi görünümünün parçası olmayan diğer şablonları keşfetmek için [GitHub depomuzu](https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/README.md)ziyaret edin. Varolan çalışma kitaplarına göz atmak için GitHub'daki [Çalışma Kitabı kitaplığını](https://github.com/microsoft/Application-Insights-Workbooks/tree/master/Workbooks) ziyaret edin.

## <a name="next-step"></a>Sonraki adım

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
