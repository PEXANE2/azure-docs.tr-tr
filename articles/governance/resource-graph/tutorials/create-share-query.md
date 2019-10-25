---
title: Azure portal bir sorgu oluşturma ve paylaşma
description: Bu öğreticide, bir kaynak grafiği sorgusu oluşturup Azure portal başkalarıyla paylaşma hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: resource-graph
ms.openlocfilehash: 10f93f34923fb2399a4b2053167576ba004ae467
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72821686"
---
# <a name="tutorial-create-and-share-an-azure-resource-graph-query-in-azure-portal"></a>Öğretici: Azure portal Azure Kaynak Grafiği sorgusu oluşturma ve paylaşma

Azure Kaynak Grafiği Gezgini, kaynak grafik sorgularınızı Azure portal hemen kaydetmenizi sağlar. _Özel_ ve _paylaşılan_iki tür sorgu vardır. _Özel_ bir sorgu Azure Portal ayarlarınıza kaydedilir, ancak _paylaşılan_ sorgu rol tabanlı erişim denetimleriyle (RBAC) yönetilebilecek ve kaynak kilitleri ile korunabilen bir kaynak yöneticisi kaynaktır.

Azure portal sorguları kaydetme, sık kullandığınız veya yaygın olarak kullanılan sorguları bulmak için harcanan süreyi kaydeder. Sorguları paylaşırken takımınızın tutarlı ve yinelenebilir olmasını sağlayabilirsiniz. Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

> [!div class="checklist"]
> - _Özel_ sorgu oluşturma ve silme
> - _Paylaşılan_ sorgu oluşturma
> - _Paylaşılan_ sorguları bulma
> - _Paylaşılan_ bir sorguyu silme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-and-delete-a-private-query"></a>Özel sorgu oluşturma ve silme

_Özel_ sorgular yalnızca erişilebilir veya onları oluşturan hesap tarafından görülebilir. Bir hesabın Azure portal ayarlarına kaydedildiğinden, bunlar yalnızca Azure portal içinden oluşturulabilir, kullanılabilir ve silinebilir. _Özel_ bir sorgu kaynak yöneticisi kaynak değildir. Aşağıdaki adımları izleyerek yeni bir _özel_ sorgu oluşturun:

1. Portal menüsünde, ' tüm hizmetler ' ' i seçin veya tüm sayfaların en üstündeki Azure Arama kutusunu kullanın.
   ' Kaynak grafiği Gezgini ' ni arayıp seçin.

1. Azure Kaynak Grafiği Gezgini sayfasındaki ' sorgu 1 ' sekmesinde aşağıdaki sorguyu girin. Bu sorgu hakkında daha fazla bilgi için bkz. [OS türüne göre örnek sayısı sanal makineleri](../samples/starter.md#count-virtual-machines-by-os-type).
   Sorgu sonuçlarını tek alt bölmede görmek için **Sorguyu Çalıştır** ' ı seçin.

   ```kusto
   Resources
   | where type =~ 'Microsoft.Compute/virtualMachines'
   | summarize count() by tostring(properties.storageProfile.osDisk.osType)
   ```

1. **Kaydet** veya **farklı kaydet**' i SEÇIN, _adı_ ' OS tarafından VM sayısı ' olarak girin, _türü_ ' özel sorgu ' olarak bırakın ve _Kaydet sorgu_ bölmesinin en altında **Kaydet** ' i seçin. Sekmenin başlığı ' sorgu 1 ' iken ' VM 'ye göre VM sayısı ' olarak değişir.

1. Azure portal ' de Azure Kaynak Grafiği Gezgini ' nden gidip bu konuma geri dönün. Kaydedilen Sorgu artık görüntülenmiyor ve ' sorgu 1 ' sekmesi döndü.

1. **Sorgu aç**' ı seçin. _Türün_ ' Private Query ' olup olmadığını denetleyin. ' OS tarafından VM 'Ler tarafından kaydedilen ' sayısı artık _sorgu adı_ listesinde görünür. Kaydedilen sorgunun başlık bağlantısını seçin ve bu sorgular adıyla yeni bir sekmeye yüklenir.

   > [!NOTE]
   > Kaydedilen bir sorgu açık olduğunda ve sekme bu _adı_gösteriyorsa, **Kaydet** düğmesi bunu yapılan değişikliklerle günceller. Yeni kaydedilmiş bir sorgu oluşturmak için **farklı kaydet** ' i kullanın ve yeni bir kayıtlı sorgu gibi adımları izleyin.

1. Kayıtlı sorguyu silmek için **bir sorguyu yeniden aç** ' ı seçin ve bu _türün_ ' özel sorgu ' olduğunu denetleyin. ' İşletim sistemine göre VM sayısı ' sorgusunun satırındaki çöp kutusu simgesini seçin. Onay iletişim kutusunda, sorguyu silme işlemini gerçekleştirmek için **Evet** ' i seçin. Sonra _sorgu aç_ bölmesini kapatın.

## <a name="create-a-shared-query"></a>Paylaşılan sorgu oluşturma

_Özel_ bir sorgunun aksine, _paylaşılan_ bir sorgu Kaynak Yöneticisi kaynağıdır. Bu olgu, sorgunun bir kaynak grubuna kaydedildiği, RBAC ile yönetilebilecek ve denetlenebileceği, hatta kaynak kilitleri ile korunabilecek anlamına gelir. Kaynak olarak, uygun izinlere sahip herkes bunu görebilir ve kullanabilir. Aşağıdaki adımları izleyerek yeni bir _paylaşılan_ sorgu oluşturun:

1. Portal menüsünde, ' tüm hizmetler ' ' i seçin veya tüm sayfaların en üstündeki Azure Arama kutusunu kullanın.
   ' Kaynak grafiği Gezgini ' ni arayıp seçin.

1. Azure Kaynak Grafiği Gezgini sayfasındaki ' sorgu 1 ' sekmesinde aşağıdaki sorguyu girin. Bu sorgu hakkında daha fazla bilgi için bkz. [OS türüne göre örnek sayısı sanal makineleri](../samples/starter.md#count-virtual-machines-by-os-type).
   Sorgu sonuçlarını alt bölmede görmek için **Sorguyu Çalıştır** ' ı seçin.

   ```kusto
   Resources
   | where type =~ 'Microsoft.Compute/virtualMachines'
   | summarize count() by tostring(properties.storageProfile.osDisk.osType)
   ```

1. **Kaydet** veya **farklı kaydet**' i seçin.

   ![Kaydet düğmesini kullanarak yeni sorguyu kaydetme](../media/create-share-query/save-shared-query-buttons.png)

1. _Sorguyu Kaydet_ bölmesinde _adı_ ' OS tarafından VM 'leri say ' olarak girin, _türü_ ' paylaşılan sorgu ' olarak değiştirin, _açıklamayı_ işletim sistemi türüne göre sanal makine sayısı ' olarak ayarlayın ve sorgu kaynağının oluşturulduğu _aboneliği_ seçin. ' Kaynak-grafik-sorgu kaynak grubu ' na Yayımla ' onay kutusunu ve _kaynak grubu konumunu_ ' (US) Orta Batı ABD ' olarak ayarlanmış olarak bırakın. Sonra Kaydet _sorgu_ bölmesinin en altında bulunan **Kaydet** ' i seçin. Sekmenin başlığı ' sorgu 1 ' iken ' VM 'ye göre VM sayısı ' olarak değişir. ' Kaynak-grafik-sorgular ' kaynak grubu ilk kez kullanıldığında, kaynak grubu oluşturulduktan sonra kaydetme işlemi daha uzun sürer.

   ![Yeni sorguyu paylaşılan sorgu olarak kaydet](../media/create-share-query/save-shared-query-window.png)

   > [!NOTE]
   > İsterseniz, paylaşılan sorguyu kaydetmek için var olan bir kaynak grubunun adını sağlamak üzere denetimini kaldırın. Sorgular için varsayılan adlandırılmış kaynak grubunun kullanılması, _paylaşılan_ sorguları bulmayı kolaylaştırır. Ayrıca, bu kaynak grubunun amacını daha da belirgin hale getirir. Bununla birlikte, mevcut izinlere bağlı olarak, var olan bir kaynak grubunun seçilmesi güvenlik nedenleriyle yapılabilir.

1. Azure portal ' de Azure Kaynak Grafiği Gezgini ' nden gidip bu konuma geri dönün. Kaydedilen Sorgu artık görüntülenmiyor ve ' sorgu 1 ' sekmesi döndü.

1. **Sorgu aç**' ı seçin. Bu _türün_ ' Shared Query ' olup olmadığını ve sorguyu kaydettiğiniz yerdeki _abonelik_ ve _kaynak grubu_ ile eşleşen bir bileşim olduğunu denetleyin. ' OS tarafından VM 'Ler tarafından kaydedilen ' sayısı artık _sorgu adı_ listesinde görünür. Kaydedilen sorgunun başlık bağlantısını seçin ve bu sorgular adıyla yeni bir sekmeye yüklenir. _Paylaşılan_ bir sorgu olarak, başlığı paylaşılan olarak belirten başlığın yanındaki sekmede bir simge görüntüler.

   ![Paylaşılan sorguyu simge ile göster](../media/create-share-query/show-saved-shared-query.png)

   > [!NOTE]
   > Kaydedilen bir sorgu açık olduğunda ve sekme bu _adı_gösteriyorsa, **Kaydet** düğmesi bunu yapılan değişikliklerle günceller. Yeni kaydedilmiş bir sorgu oluşturmak için **farklı kaydet** ' i kullanın ve yeni bir kayıtlı sorgu gibi adımları izleyin.

## <a name="discover-shared-queries"></a>Paylaşılan sorguları bulma

_Paylaşılan_ bir sorgu kaynak yöneticisi bir kaynaktır, bunları bulmanın birkaç yolu vardır:

- Kaynak Grafiği Gezgini 'nden **sorgu aç** ' ı seçin ve _türü_ ' paylaşılan sorgu ' olarak ayarlayın
- Kaynak Grafiği sorgulama portalı sayfası
- Kaydedildiği kaynak grubu
- Kaynak grafiğine yönelik bir sorgu ile

### <a name="view-resource-graph-queries"></a>Kaynak Grafiği sorgularını görüntüle

Azure portal, kaynak grafiği sorguları sayfası, oturum açan hesabın erişebileceği _paylaşılan_ sorguları görüntüler. Bu sayfa, kaynak grafiği sorgusunun adına, aboneliğe, kaynak grubuna ve diğer özelliklerine göre filtrelemeye olanak tanır. Kaynak Grafiği sorguları Ayrıca bu arabirim kullanılarak etiketlenebilir, aktarılabilir ve silinebilir.

Sorgulardan birini seçmek kaynak grafiği sorgu sayfasını açar. Diğer Kaynak Yöneticisi kaynaklar gibi bu sayfada, etkinlik günlüğü, erişim denetimi ve etiketleriyle birlikte etkileşimli bir genel bakış sunulmaktadır. Kaynak kilidi, doğrudan bu sayfadan de uygulanabilir.

' Tüm hizmetler ' ' i seçerek veya tüm sayfaların en üstündeki Azure Arama kutusunu kullanarak portal menüsünden Kaynak Grafiği sorguları sayfasına ulaşın. ' Kaynak grafiği Gezgini ' ni arayıp seçin.

### <a name="list-resource-groups-resources"></a>Kaynak grupları kaynaklarını listeleme

Kaynak Grafiği sorgusu, kaynak grubunun parçası olan diğer kaynakların yanı sıra listelenir.
Kaynak Grafiği sorgusunun seçilmesi, bu sorgunun sayfasını açar. Üç nokta veya sağ tıklama seçenekleri, kaynak Graph sorgu sayfasıyla aynı şekilde çalışır.

### <a name="query-resource-graph"></a>Sorgu kaynağı grafiği

Kaynak grafik sorguları, Kaynak Yöneticisi bir kaynak olarak kaynak grafik sorgusu ile bulunabilir.
Aşağıdaki kaynak Graph sorgu `Microsoft.ResourceGraph/queries`türe göre kısıtlar ve sonra yalnızca adı, değiştirilme süresini ve sorgunun kendisini listelemek için `project` kullanır:

```kusto
Resources
| where type == "microsoft.resourcegraph/queries"
| project name, properties.timeModified, properties.query
```

## <a name="delete-a-shared-query"></a>Paylaşılan bir sorguyu silme

_Paylaşılan_ bir sorgu artık gerekmiyorsa, silin. _Paylaşılan_ bir sorgunun silinmesi, gerçek kaynak yöneticisi kaynağını kaldırır. Sonuçlar grafiğinin şimdi bir hata mesajı görüntülemesi için sabitlenmiş panolar. Bu hata iletisi görüntülendiğinde panonuzu temizlemek için **panodan kaldır** düğmesini kullanın.

_Paylaşılan_ bir sorgu aşağıdaki arabirimlerden silinebilir:
- Kaynak Grafiği sorguları sayfası
- Kaynak Grafiği sorgu sayfası
- Kaynak grafik Gezgini bir sorgu sayfası aç
- Kaynak grupları sayfası

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi tamamladığınızda, artık istemediğiniz _özel_ ve _paylaşılan_ sorguları silin.

## <a name="next-steps"></a>Sonraki adımlar

- İlk sorgunuzu [Azure Portal](../first-query-portal.md) ile çalıştırma
- [Sorgu dili](../concepts/query-language.md) hakkında daha fazla bilgi edinme
- [Kaynakları keşfetmeyi](../concepts/explore-resources.md) öğrenin
- Bkz. [başlangıç sorguları](../samples/starter.md) örnekleri
- Bkz. [Gelişmiş sorgular](../samples/advanced.md) örnekleri
- [UserVoice](https://feedback.azure.com/forums/915958-azure-governance) ile ilgili geri bildirim gönderme