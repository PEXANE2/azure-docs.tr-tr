---
title: 'Quickstart: İlk portal sorgunuz'
description: Bu hızlı başlangıçta, Azure Kaynak Grafiği Gezgini'ni kullanarak Azure portalından ilk sorgunuzu çalıştırma adımlarını izlersiniz.
ms.date: 11/21/2019
ms.topic: quickstart
ms.openlocfilehash: 5cf355e78ad51e06d7ba27d48dd352f35b4c0740
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74406789"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-resource-graph-explorer"></a>Hızlı başlatma: Azure Kaynak Grafiği Gezgini'ni kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırma

Azure Kaynak Grafiği'nin gücü, Azure Kaynak Grafiği Gezgini aracılığıyla doğrudan Azure portalında kullanılabilir. Kaynak Grafiği Gezgini, Sorgulayabildiğiniz Azure Kaynak Yöneticisi kaynak türleri ve özellikleri hakkında göz atılabilir bilgiler sağlar. Kaynak Grafiği Gezgini ayrıca birden çok sorguyla çalışmak, sonuçları değerlendirmek ve hatta bazı sorguların sonuçlarını Azure panosuna sabitlenebilecek bir grafiğe dönüştürmek için temiz bir arayüz sağlar.

Bu hızlı başlangıcın sonunda, ilk Kaynak Grafiği sorgunuzu çalıştırmak için Azure portalını ve Kaynak Grafiği Gezgini'ni kullanmış ve sonuçları bir panoya sabitlemiş olacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="run-your-first-resource-graph-query"></a>İlk Kaynak Grafiği sorgunuzu çalıştırma

İlk Kaynak Grafiği sorgunuzu çalıştırmak için aşağıdaki adımları izleyen Kaynak Grafiği Gezgini'ni bulmak ve kullanmak için [Azure portalını](https://portal.azure.com) açın:

1. Sol bölmede **Tüm hizmetler**'i seçin. Kaynak Grafiği **Gezgini'ni**arayın ve seçin.

1. Pencerenin **Sorgu 1** bölümünde, sorguyu `Resources | project name, type | limit 5` girin ve **sorguyu çalıştır'ı**seçin.

   > [!NOTE]
   > Bu sorgu örneği gibi `order by`bir sıralama değiştirici sağlamadığından, bu sorgunun birden çok kez çalıştırış, istek başına farklı bir kaynak kümesi verim olasılığı yüksektir.

1. **Sonuçlar** sekmesindeki sorgu yanıtını gözden **Messages** geçirin. Varsa hatalar bu sekme altında görüntülenir.

1. Sorguyu `order by` **Ad** özelliğine `Resources | project name, type | limit 5 | order by name asc`güncelleştirin: . Ardından **sorguyı çalıştır'ı**seçin.

   > [!NOTE]
   > İlk sorguda olduğu gibi, bu sorguyu birden çok kez çalıştırmak, muhtemelen istek başına farklı bir kaynak kümesi sunacaktır. Sorgu komutlarının düzeni önemlidir. Bu örnekte `order by`, `limit`’den sonra gelmektedir. Bu, sorgu sonuçlarını önce sınırlar, sonra düzenler.

1. `order by` Önce **Ad** özelliğine, ardından `limit` ilk beş sonuçiçin `Resources | project name, type | order by name asc | limit 5`sorguyu güncelleştirin: . Ardından **sorguyı çalıştır'ı**seçin.

Son sorgu birkaç kez çalıştırıldığında, ortamınızda hiçbir şeyin değişmediğini varsayarsak, döndürülen sonuçlar tutarlıdır ve beklendiği gibi **Ad** özelliği tarafından sıralanır, ancak yine de ilk beş sonuçla sınırlıdır.

### <a name="schema-browser"></a>Şema tarayıcısı

Şema tarayıcısı Kaynak Grafiği Gezgini'nin sol bölmesinde yer alır. Bu kaynak listesi, her ikisi de Azure Kaynak Grafiği tarafından desteklenen ve erişebildiğiniz bir kiracıda bulunan tüm Azure _kaynakları kaynak türlerini_ gösterir. Kaynak türünü veya alt özellikleri genişletmek, Kaynak Grafiği sorgusu oluşturmak için kullanılabilecek alt özellikleri gösterir.

Kaynak türü yerlerini `where type =="<resource type>"` sorgu kutusuna seçmek. Alt özelliklerden birini seçmek `where <propertyName> == "INSERT_VALUE_HERE"` sorgu kutusuna ekler.
Şema tarayıcısorgularda kullanım özellikleri keşfetmek için harika bir yoldur. _INSERT\_VALUE\_HERE'ı_ kendi değerinizle değiştirdiğinizden emin olun, istenen sonuçları elde etmek için sorguyu koşullarla, işleçlerle ve işlevlerle ayarlayın.

## <a name="create-a-chart-from-the-resource-graph-query"></a>Kaynak Grafiği sorgusundan grafik oluşturma

Yukarıdaki son sorguyu çalıştırdıktan sonra, **Grafikler** sekmesini seçerseniz, "sonuç kümesi pasta grafiği görselleştirmesiyle uyumlu değildir" iletisi alırsınız. Sonuçları listeleyen sorgular bir grafiğe dönüştürülemez, ancak kaynak sayısı sağlayan sorgular yapılabilir. Örnek [sorguyu kullanma - Sanal makineleri işletim sistemi türüne göre say,](./samples/starter.md#count-virtual-machines-by-os-type)Kaynak Grafiği sorgusundan bir görselleştirme oluşturalım.

1. Pencerenin **Sorgu 1** bölümünde, aşağıdaki sorguyu girin ve **sorguyu çalıştır'ı**seçin.

   ```kusto
   Resources
   | where type =~ 'Microsoft.Compute/virtualMachines'
   | summarize count() by tostring(properties.storageProfile.osDisk.osType)
   ```

1. **Sonuçlar** sekmesini seçin ve bu sorguiçin yanıtın sayımlar sağladığını unutmayın.

1. **Grafikler** sekmesini seçin. Şimdi, sorgu görselleştirmelerle sonuçlanır. Kullanılabilir görselleştirme seçeneklerini denemek için türü _Select chart türünden_ _Çubuk grafiğine_ veya _Donut grafiğine_ değiştirin.

## <a name="pin-the-query-visualization-to-a-dashboard"></a>Sorgu görselleştirmesini panoya sabitleme

Görüntülenebilen bir sorgudan sonuçlar aldığınızda, bu veri görselleştirmesi panolarınızdan birine sabitlenebilir. Yukarıdaki sorguyu çalıştırdıktan sonra aşağıdaki adımları izleyin:

1. **Kaydet'i** seçin ve "OS Türüne Göre VM' ler" adını sağlayın. Ardından sağ bölmenin altındaki **Kaydet'i** seçin.

1. Kaydedilmiş olan sorguyu yeniden çalıştırmak için **Sorguyu Çalıştır'ı** seçin.

1. **Grafikler** sekmesinde bir veri görselleştirmesi seçin. Ardından **panoya Sabitle'yi**seçin.

1. Görünen portal bildirimini seçin veya sol bölmeden **Pano'yu** seçin.

Sorgu artık panonuzda sorgu adıyla eşleşen döşeme başlığıyla kullanılabilir. Sorgu sabitlendiğinde kaydedilmemişse, bunun yerine 'Sorgu 1' adı verilmiştir.

Sorgu ve ortaya çıkan veri görselleştirmesi, pano her yükleninher yılında çalışır ve güncellenir ve doğrudan iş akışınızda Azure ortamınıza gerçek zamanlı ve dinamik öngörüler sağlar.

> [!NOTE]
> Bir listeyle sonuçlanan sorgular da panoya sabitlenebilir. Özellik, sorguların veri görselleştirmeleri ile sınırlı değildir.

## <a name="import-example-resource-graph-explorer-dashboards"></a>İçe aktarma örneği Kaynak Grafiği Explorer panoları

Kaynak Grafiği sorgularına örnekler vermek ve Kaynak Grafiği Gezgini'nin Azure portal iş akışınızı geliştirmek için nasıl kullanılabileceğini sağlamak için bu örnek panoları deneyin.

- [Kaynak Grafiği Gezgini - Örnek Pano #1](https://github.com/Azure-Samples/Governance/blob/master/src/resource-graph/portal-dashboards/sample-1/resourcegraphexplorer-sample-1.json)

  [![Örnek Pano #1 için örnek resim](./media/arge-sample1-small.png)](./media/arge-sample1-large.png#lightbox)

- [Kaynak Grafiği Gezgini - Örnek Pano #2](https://github.com/Azure-Samples/Governance/blob/master/src/resource-graph/portal-dashboards/sample-2/resourcegraphexplorer-sample-2.json)

  [![Örnek Pano #2 için örnek resim](./media/arge-sample2-small.png)](./media/arge-sample2-large.png#lightbox)

> [!NOTE]
> Yukarıdaki örnek pano ekran görüntülerindeki sayımlar ve grafikler Azure ortamınıza bağlı olarak değişir.

1. Değerlendirmek istediğiniz örnek panosunu seçin ve indirin.

1. Azure portalında, sol bölmeden **Pano'yu** seçin.

1. **Yükle'yi**seçin, ardından indirilen örnek pano dosyasını bulun ve seçin. Sonra **Aç'ı**seçin.

Alınan pano otomatik olarak görüntülenir. Azure portalınızda artık var olduğundan, gerektiğinde değişiklikleri keşfedebilir ve yapabilir veya ekiplerinizle paylaşmak için örnekten yeni panolar oluşturabilirsiniz. Panolarla çalışma hakkında daha fazla bilgi için Azure [portalında pano oluştur ve paylaş'a](../../azure-portal/azure-portal-dashboards.md)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek Kaynak Grafiği panolarını Azure portal ortamınızdan kaldırmak istiyorsanız, bunu aşağıdaki adımlarla yapabilirsiniz:

1. Sol bölmeden **Pano'yu** seçin.

1. Pano açılır panelinden, silmek istediğiniz örnek Kaynak Grafiği panosunu seçin.

1. Pano üst kısmındaki pano menüsünden **Sil'i** ve onaylamak için **Tamam'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, ilk sorgunuzu çalıştırmak için Azure Kaynak Grafiği Gezgini'ni kullandınız ve Kaynak Grafiği tarafından desteklenen pano örneklerine baktınız. Kaynak grafik dili hakkında daha fazla bilgi edinmek için, sorgu dili ayrıntıları sayfasına devam edin.

> [!div class="nextstepaction"]
> [Sorgu dili hakkında daha fazla bilgi alın](./concepts/query-language.md)