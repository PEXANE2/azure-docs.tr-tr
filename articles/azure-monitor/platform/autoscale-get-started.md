---
title: Azure'da otomatik ölçeklendirmeye başlayın
description: Azure'da ayarlanan kaynak Web Uygulamanızı, Bulut Hizmetinizi, Sanal Makinenizi veya Sanal Makine Ölçeğinizi nasıl ölçeklendireceklerini öğrenin.
ms.topic: conceptual
ms.date: 07/07/2017
ms.subservice: autoscale
ms.openlocfilehash: 0a40496e4d496d0062c6ee7a6ab935c1ad9b35a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75396343"
---
# <a name="get-started-with-autoscale-in-azure"></a>Azure'da otomatik ölçeklendirmeyi kullanmaya başlama
Bu makalede, Microsoft Azure portalında kaynağınız için Otomatik Ölçek ayarlarınızı nasıl ayarlayabilirsiniz.

Azure Monitör otomatik ölçeklendirmesi yalnızca [Sanal Makine Ölçeği Kümeleri,](https://azure.microsoft.com/services/virtual-machine-scale-sets/) [Bulut Hizmetleri,](https://azure.microsoft.com/services/cloud-services/) [Uygulama Hizmeti - Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/)ve [API Yönetimi hizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)için geçerlidir.

## <a name="discover-the-autoscale-settings-in-your-subscription"></a>Aboneliğinizdeki Otomatik Ölçek ayarlarını keşfedin
Azure Monitörü'nde Otomatik Ölçek'in geçerli olduğu tüm kaynakları keşfedebilirsiniz. Adım adım gözden geçirme için aşağıdaki adımları kullanın:

1. Azure [portalını açın.][1]
1. Sol bölmedeki Azure Monitör simgesini tıklatın.
  ![Azure Monitörünü Aç][2]
1. Otomatik Ölçeklendirme'nin geçerli olduğu tüm kaynakları ve geçerli Otomatik Ölçek durumuyla birlikte görüntülemek için **Otomatik** Ölçek'i tıklatın.
  ![Azure Monitör'de Otomatik Ölçeklendirmeyi Keşfedin][3]

Belirli bir kaynak grubundaki kaynakları, belirli kaynak türlerini veya belirli bir kaynağı seçmek için listenin kapsamını genişletmek için en üstteki filtre bölmesini kullanabilirsiniz.

Her kaynak için geçerli örnek sayısını ve Otomatik Ölçek durumunu bulacaksınız. Otomatik ölçek durumu şu olabilir:

- **Yapılandırılmamış**: Bu kaynak için henüz Otomatik Ölçeklendirme'yi etkinleştirmiş değil.
- **Etkin**: Bu kaynak için Otomatik Ölçeklendirme'yi etkinleştirdin.
- **Devre Dışı :** Bu kaynak için Otomatik Ölçek'i devre dışı bıraktınız.

## <a name="create-your-first-autoscale-setting"></a>İlk Otomatik Ölçek ayarınızı oluşturun

Şimdi ilk Otomatik Ölçek ayarınızı oluşturmak için adım adım basit bir gözden geçirme den geçelim.

1. Azure Monitor'da **Otomatik Ölçeklendirme** bıçağını açın ve ölçeklendirmek istediğiniz bir kaynak seçin. (Aşağıdaki adımlar, bir web uygulamasıyla ilişkili bir Uygulama Hizmeti planı kullanır. [Azure'da ilk ASP.NET web uygulamanızı 5 dakikada oluşturabilirsiniz.][4])
1. Geçerli örnek sayısının 1 olduğunu unutmayın. **Otomatik ölçeklendir'i etkinleştir'i**tıklatın.
  ![Yeni web uygulaması için ölçek ayarı][5]
1. Ölçek ayarı için bir ad sağlayın ve ardından **kural ekle'yi**tıklatın. Sağ tarafta bağlam bölmesi olarak açılan ölçek kuralı seçeneklerine dikkat edin. Varsayılan olarak, kaynağın CPU yüzdesi yüzde 70'i aşarsa, örnek sayınızı 1 ölçeklendirme seçeneği belirler. Varsayılan değerlerinde bırakın ve **Ekle'yi**tıklatın.
  ![Bir web uygulaması için ölçek ayarı oluşturma][6]
1. Şimdi ilk ölçek kuralınızı oluşturdunuz. UX'nin en iyi uygulamaları önerdiğini ve "En az bir ölçeğin kural altında olması önerilir" dediğini unutmayın. Bunu yapmak için:

    a. **Kural ekle**'ye tıklayın.

    b. **Operatör'ün** **'den az'** olarak ayarla

    c. **Eşik** **20'ye**ayarlayın.

    d. **İşlemi** **Sayısı'nı Azaltacak**şekilde ayarlayın

   Cpu kullanımına göre ölçeklendirilebilen/ölçeklendirilebilen bir ölçek ayarını şimdi almalısınız.
   ![CPU'ya dayalı ölçek][8]
1. **Kaydet**'e tıklayın.

Tebrikler! Şimdi, web uygulamanızı CPU kullanımına göre otomatik ölçeklendirmek için ilk ölçek ayarınızı başarıyla oluşturdunuz.

> [!NOTE]
> Sanal makine ölçeği kümesi veya bulut hizmeti rolüyle başlamak için aynı adımlar geçerlidir.

## <a name="other-considerations"></a>Diğer konular
### <a name="scale-based-on-a-schedule"></a>Zamanlamaya dayalı ölçeklendirme
CPU'ya dayalı ölçekle ek olarak, ölçeğinizi haftanın belirli günlerinde farklı ayarlayabilirsiniz.

1. **Ölçek koşulu ekle'yi**tıklatın.
1. Ölçek modunu ve kuralları ayarlama varsayılan koşulla aynıdır.
1. Zamanlama için **belirli günleri yinele'yi** seçin.
1. Ölçek koşulunun uygulanması gereken günleri ve başlangıç/bitiş saatini seçin.

![Zamanlamaya göre ölçek koşulu][9]
### <a name="scale-differently-on-specific-dates"></a>Belirli tarihlerde farklı ölçeklendirme
CPU'ya dayalı ölçekle ek olarak, ölçeğinizi belirli tarihler için farklı şekilde ayarlayabilirsiniz.

1. **Ölçek koşulu ekle'yi**tıklatın.
1. Ölçek modunu ve kuralları ayarlama varsayılan koşulla aynıdır.
1. Zamanlamanın **başlangıç/bitiş tarihlerini belirt'i** seçin.
1. Ölçek koşulunun ne zaman uygulanması gerektiğini başlangıç/bitiş tarihlerini ve başlangıç/bitiş saatini seçin.

![Tarihleri temel alan ölçek koşulu][10]

### <a name="view-the-scale-history-of-your-resource"></a>Kaynağınızın ölçek geçmişini görüntüleme
Kaynağınız yukarı veya aşağı ölçeklendirildiğinde, etkinlik günlüğünde bir olay günlüğe kaydedilir. **Run geçmişi** sekmesine geçerek kaynağınızın son 24 saatteki ölçek geçmişini görüntüleyebilirsiniz.

![Çalıştırma geçmişi][11]

Ölçek geçmişini (90 güne kadar) görüntülemek istiyorsanız, **daha fazla ayrıntıyı görmek için burayı tıklatın'ı**seçin. Etkinlik günlüğü açılır ve kaynağınız ve kategoriniz için otomatik ölçek önceden seçilir.

### <a name="view-the-scale-definition-of-your-resource"></a>Kaynağınızın ölçek tanımını görüntüleme
Otomatik ölçek bir Azure Kaynak Yöneticisi kaynağıdır. **JSON** sekmesine geçerek JSON'daki ölçek tanımını görüntüleyebilirsiniz.

![Ölçek tanımı][12]

Gerekirse JSON'da doğrudan değişiklik yapabilirsiniz. Bu değişiklikler, siz kaydedildikten sonra yansıtılır.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Otomatik ölçeklendirmeyi devre dışı bırakıp örneklerinizi el ile ölçeklendirin
Geçerli ölçek ayarınızı devre dışı bırakıp kaynağınızı el ile ölçeklendirmek istediğiniz zamanlar olabilir.

Üstteki **otomatik ölçeklendirmeyi devre dışı** aç düğmesini tıklatın.
![Otomatik Ölçeği Devre Dışı][13]

> [!NOTE]
> Bu seçenek yapılandırmanızı devre dışı katıyor. Ancak, Otomatik Ölçeklendirme'yi yeniden etkinleştirdikten sonra geri alabilirsiniz.

Artık ölçeklendirmek istediğiniz örnek sayısını el ile ayarlayabilirsiniz.

![Manuel ölçek ayarlama][14]

Otomatik **Ölçeklendir'i Etkinleştir'i** ve ardından **Kaydet'i**tıklatarak her zaman Otomatik Ölçeklendirme'ye dönebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Aboneliğinizdeki tüm Otomatik Ölçeklendirme motor işlemlerini izlemek için Etkinlik Günlüğü Uyarısı oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Aboneliğinizdeki tüm başarısız Otomatik Ölçeklendirme ölçeklendirme/ölçeklendirme işlemlerini izlemek için Etkinlik Günlüğü Uyarısı oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/autoscale-get-started/azure-monitor-launch.png
[3]: ./media/autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-dotnet
[5]: ./media/autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/autoscale-get-started/scale-in-recommendation.png
[8]: ./media/autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/autoscale-get-started/scale-condition-schedule.png
[10]: ./media/autoscale-get-started/scale-condition-dates.png
[11]: ./media/autoscale-get-started/scale-history.png
[12]: ./media/autoscale-get-started/scale-definition-json.png
[13]: ./media/autoscale-get-started/disable-autoscale.png
[14]: ./media/autoscale-get-started/set-manualscale.png

