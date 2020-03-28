---
title: 'Öğretici: Azure portalında sorguları yönetme'
description: Bu öğreticide, bir Kaynak Grafiği Sorgusu oluşturur ve yeni sorguyu Azure portalındaki diğer kişilerle paylaşırsınız.
ms.date: 11/21/2019
ms.topic: tutorial
ms.openlocfilehash: 00cb3f95112804c81beb6bce6fc35891e6197e60
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74303951"
---
# <a name="tutorial-create-and-share-an-azure-resource-graph-query-in-the-azure-portal"></a>Öğretici: Azure portalında bir Azure Kaynak Grafiği sorgusu oluşturma ve paylaşma

Azure Kaynak Grafiği Gezgini, Kaynak Grafiği sorgularınızı doğrudan Azure portalına kaydetmenizi sağlar. İki tür sorgu vardır: _Özel_ ve _Paylaşılan._ Azure portal ayarlarınızda Özel sorgu kaydedilir. Paylaşılan sorgu, rol tabanlı erişim denetimleri (RBAC) ile yönetilebilen ve kaynak kilitleriyle korunan bir Kaynak Yöneticisi kaynağıdır. Her iki sorgu türü de istirahatte şifrelenir.

Azure portalındaki sorguları kaydederek, aksi takdirde sık kullandığınız veya sık kullanılan sorgularınızı arayarak harcayabileceğiniz zamandan tasarruf elabilirsiniz. Sorguları paylaştığınızda, tekrarlama yoluyla takımınızın tutarlılık ve verimlilik hedeflerini gerçekleştirmelerine yardımcı olursunuz.

Bu öğreticide, aşağıdaki görevleri tamamlayasınız:

> [!div class="checklist"]
> - Özel sorgu oluşturma ve silme
> - Paylaşılan sorgu oluşturma
> - Paylaşılan sorguları keşfedin
> - Paylaşılan sorguy

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-and-delete-a-private-query"></a>Özel sorgu oluşturma ve silme

Özel sorgulara yalnızca bunları oluşturan hesap erişebilir ve görünür. Bir hesabın Azure portal ayarlarına kaydedildiklerinde, yalnızca Azure portalının içinden oluşturulabilir, kullanılabilir ve silinebilir. Özel sorgu kaynak yöneticisi kaynağı değildir. Yeni bir Özel sorgu oluşturmak için aşağıdaki adımları izleyin:

1. Portal menüsünden **Tüm hizmetler'i** seçin veya tüm sayfaların üst kısmındaki Azure arama kutusunu kullanın. **Kaynak Grafiği Gezgini'ni**arayın ve ardından seçin.

1. Azure Kaynak Grafiği Gezgini sayfasındaki **Sorgu 1** sekmesine aşağıdaki sorguyu girin:

   ```kusto
   Resources
   | where type =~ 'Microsoft.Compute/virtualMachines'
   | summarize count() by tostring(properties.storageProfile.osDisk.osType)
   ```

   Sorgu sonuçlarını alt bölmede görmek için **Sorgu yu** seçin.

   Bu sorgu hakkında daha fazla bilgi için bkz: [Örnekler – Sanal makineleri işletim sistemi türüne göre say.](../samples/starter.md#count-virtual-machines-by-os-type)


1. Olarak Kaydet veya **Kaydet'i** seçin, os'ye göre Sayım **VM'lerini** ad olarak girin, türü **Özel sorgu**olarak bırakın ve ardından **Sorguyu Kaydet** bölmesinin en altında **Kaydet'i** seçin. **Save as** Sekme **başlığı, Sorgu 1'den** **OS'ye göre VM Sayısı'na**dönüşür.

1. Azure portalındaki Azure Kaynak Grafiği Gezgini'nden uzaklaşın ve sonra geri dönün. Kaydedilen sorgunun artık görüntülenmediğini ve **Sorgu 1** sekmesinin döndürüldedikkat edin.

1. **Sorgu aç'ı**seçin. Türün **Özel sorgu**olduğundan emin olun. OS tarafından kaydedilen ad **Count VM'ler** artık **Sorgu Adı** listesinde görünür. Kaydedilen sorgunun başlık bağlantısını seçtiğinizde, bu sorgunun adını içeren yeni bir sekme yüklenir.

   > [!NOTE] 
   > Kaydedilen bir sorgu açık olduğunda ve sekme adını gösterdiğinde, **Kaydet** düğmesini seçerek yapılan değişikliklerle birlikte güncelleştirir. Bu açık sorgudan yeni bir kayıtlı sorgu oluşturmak **için, olarak kaydet'i** seçin ve yepyeni bir sorgu kaydediyormuş gibi devam edin.

1. Kaydedilen sorguyu silmek için, sorguyu yeniden **aç'ı** seçin ve **Tür** alanının **Özel sorgu**olarak ayarlanıp ayarlıştırDığını doğrulayın. Kaydedilen `Count VMs by OS` sorgunun satırında **Sil** (Geri Dönüşüm kutusu simgesi) seçeneğini belirleyin. Onay iletişim kutusunda, sorguyu silmeyi bitirmek için **Evet'i** seçin.
   Ardından, sorgu bölmesini **aç'ı** kapatın.

## <a name="create-a-shared-query"></a>Paylaşılan sorgu oluşturma

Özel sorgunun aksine, Paylaşılan sorgu Kaynak Yöneticisi kaynağıdır. Bu durum, sorgunun bir kaynak grubuna kaydedilmesi, RBAC ile yönetilebileceği ve denetlenebildiği ve hatta kaynak kilitleriyle korunabileceği anlamına gelir. Kaynak olarak, uygun izinlere sahip olan herkes bunu görebilir ve kullanabilir.
Yeni bir Paylaşılan sorgu oluşturmak için aşağıdaki adımları izleyin:

1. Portal menüsünden **Tüm hizmetleri**seçin veya Kaynak Grafiği Gezgini'ni aramak ve **Resource Graph Explorer**seçmek için tüm sayfaların üst kısmındaki Azure arama kutusunu kullanın.

1. Azure Kaynak Grafiği Gezgini sayfasındaki **Sorgu 1** sekmesine aşağıdaki sorguyu girin:

   ```kusto
   Resources
   | where type =~ 'Microsoft.Compute/virtualMachines'
   | summarize count() by tostring(properties.storageProfile.osDisk.osType)
   ```
    
   Sorgu sonuçlarını alt bölmede görmek için **Sorgu yu** seçin.

   Bu sorgu hakkında daha fazla bilgi için bkz: [Örnekler – Sanal makineleri işletim sistemi türüne göre say.](../samples/starter.md#count-virtual-machines-by-os-type)

1. Olarak Kaydet veya **Kaydet'i** seçin. **Save as**

   
   ![Kaydet düğmesini kullanarak yeni sorguyu kaydetme](../media/create-share-query/save-shared-query-buttons.png)

1. **Sorguyu Kaydet** bölmesine, ad için **OS'ye göre Count VM'leri** girin.

1. Türü Paylaşılan **sorguolarak**değiştirin, açıklamayı **işletim sistemi türüne göre sanal makinelerin Sayısı**olarak ayarlayın ve sorgu kaynağının nerede oluşturulacağını belirtmek için **Abonelik'i** ayarlayın.

1. **Yayımla'yı kaynak grafiği sorguları kaynak grubu** onay kutusuna ve **(ABD) Batı Merkezi ABD**olarak ayarlanan Kaynak Grubu **konumuna** bırakın.

1. Sorgu bölmesinin altındaki **Kaydet'i** seçin. **Save** Sekme **başlığı, Sorgu 1'den** **OS'ye göre VM Sayısı'na**dönüşür. Kaynak grafiği **sorguları** kaynak grubu ilk kez kullanıldığında, kaynak grubu oluşturuldukça kaydetme beklenenden uzun sürer.
   
   ![Yeni sorguyu Paylaşılan sorgu olarak kaydetme](../media/create-share-query/save-shared-query-window.png)

   > [!NOTE] 
   > Paylaşılan sorguyu kaydetmek için varolan bir kaynak grubunun adını sağlamak **istiyorsanız, kaynak grafiği sorguları kaynak grubu** onay kutusuna yayımla'yı temizleyebilirsiniz. Sorgular için varsayılan adlandırılmış kaynak grubunun kullanılması Paylaşılan sorguların bulunmasını kolaylaştırır. Ayrıca, bu kaynak grubunun amacını daha belirgin hale getirir. Ancak, varolan izinleri temel alan güvenlik nedenleriyle varolan bir kaynak grubunu seçmeyi seçebilirsiniz.

1. Azure portalındaki Azure Kaynak Grafiği Gezgini'nden uzaklaşın ve sonra geri dönün. Kaydedilen sorgunun artık görüntülenmediğini ve **Sorgu 1** sekmesinin döndürüldedikkat edin.

1. **Sorgu aç'ı**seçin. Türün **Paylaşılan sorgu** olarak ayarlanıp sorguyu kaydettiğiniz **Abonelik** ve **Kaynak grubu** eşleşmesinin birleşimi olarak ayarlandığınızı doğrulayın. OS **öğesine göre** kaydedilen Count VM'ler artık **Sorgu Adı** listesinde görünür. Kaydedilen sorgunun başlık bağlantısını seçerek bu sorgunun adını içeren yeni bir sekmeye yükleyin. Paylaşılan sorgu olarak, başlığın yanındaki sekmede paylaşılan olarak gösteren bir simge görüntüler.

   ![Paylaşılan Sorguyu simgeyle göster](../media/create-share-query/show-saved-shared-query.png)

   > [!NOTE] 
   > Kaydedilen bir sorgu açık olduğunda ve sekme adını gösterdiğinde, **Kaydet** düğmesi yapılan değişikliklerle birlikte güncelleştirir. Yeni bir kayıtlı sorgu oluşturmak için, **olarak kaydet'i** seçin ve yepyeni bir sorgu kaydediyormuş gibi devam edin.

## <a name="discover-shared-queries"></a>Paylaşılan sorguları keşfedin

Paylaşılan sorgu kaynak yöneticisi kaynağı olduğundan, bir tane bulmanın birkaç yolu vardır:

- Kaynak Grafiği Gezgini'nden **sorgu aç'ı** seçin ve türü **Paylaşılan sorguya**ayarlayın.
- Kaynak Grafiği sorguları portal sayfasından.
- Paylaşılan sorgunun kaydedildiği kaynak grubundan.
- Kaynak Grafiği'ne bir sorgu aracılığıyla.

### <a name="view-resource-graph-queries"></a>Kaynak Grafiği sorgularını görüntüle

Azure portalında Kaynak Grafiği sorguları sayfası, oturum açmış hesabın erişediği Paylaşılan sorguları görüntüler. Bu sayfa, Kaynak Grafiği sorgusunun adına, aboneliğine, kaynak grubuna ve diğer özelliklerine göre filtreleme yapılmasını sağlar. Ayrıca, bu arabirimi kullanarak Kaynak Grafiği sorgularını etiketleyebilir, dışa aktarabilir ve silebilirsiniz.

Sorgulardan birini seçmek Kaynak Grafiği sorgu sayfasını açar. Diğer Kaynak Yöneticisi kaynakları gibi, bu sayfa da Etkinlik günlüğü, erişim denetimi ve etiketleri ile birlikte etkileşimli bir genel bakış sunar. Doğrudan bu sayfadan bir kaynak kilidi de uygulayabilirsiniz.

**Tüm hizmetleri** seçerek veya tüm sayfaların üst kısmındaki Azure arama kutusunu kullanarak portal menüsünden Kaynak Grafiği sorguları sayfasına gidin. Kaynak Grafiği **Gezgini'ni**arayın ve seçin.

### <a name="list-resource-groups-resources"></a>Kaynak grupları kaynaklarını listele

Kaynak Grafiği sorgusu, kaynak grubunun parçası olan diğer kaynakların yanında listelenir.
Kaynak Grafiği sorgusunu seçmek, bu sorgu için sayfayı açar. Elipsler ve kısayol menü seçenekleri (sağ tıklatma tarafından tetiklenir) Kaynak Grafiği sorgu sayfasındaki yle aynı şekilde çalışır.

### <a name="query-resource-graph"></a>Kaynak Grafiğini Sorgula

Kaynak Grafiği sorgularını kaynak grafiği sorguları aracılığıyla bulabilirsiniz. Aşağıdaki Kaynak Grafiği sorgusu `Microsoft.ResourceGraph/queries`türüne göre `project` sınırlar ve ardından yalnızca adı, değiştirilen zamanı ve sorgunun kendisini listelemek için kullanır:

```kusto
Resources
| where type == "microsoft.resourcegraph/queries"
| project name, properties.timeModified, properties.query
```

## <a name="delete-a-shared-query"></a>Paylaşılan sorguy

Paylaşılan sorguya artık gerek yoksa, silin. Paylaşılan sorguyu silerek, ilgili Kaynak Yöneticisi kaynağını kaldırırsınız. Sonuç grafiğinin artık bir hata iletisi görüntülemek için sabitlenmiş olduğu panolar. Bu hata iletisi görüntülendiğinde, panonuzu temizlemek için **panodan Kaldır** düğmesini kullanın.

Paylaşılan sorguyu aşağıdaki arabirimler aracılığıyla silebilirsiniz:
- Kaynak Grafiği sorguları sayfası
- Kaynak Grafiği sorgu sayfası
- Kaynak Grafiği Gezgini'nde sorgu sayfası **açma**
- Kaynak grupları sayfası

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi bitirdikten sonra, artık istemiyorsanız oluşturduğunuz Özel ve Paylaşılan sorguları silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Özel ve Paylaşılan sorgular oluşturdunuz. Kaynak grafik dili hakkında daha fazla bilgi edinmek için, sorgu dili ayrıntıları sayfasına devam edin.

> [!div class="nextstepaction"]
> [Sorgu dili hakkında daha fazla bilgi alın](../concepts/query-language.md)