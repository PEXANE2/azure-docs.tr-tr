---
title: Azure Monitörünü Kullanarak Metrik Uyarıları Oluşturma, Görüntüleme ve Yönetme
description: Metrik uyarı kuralları oluşturmak, görüntülemek ve yönetmek için Azure portalLarını veya CLI'yi nasıl kullanacağınızı öğrenin.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 03/13/2020
ms.subservice: alerts
ms.openlocfilehash: cefccd08ea66638f08f00e280fe2704444a7f916
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369395"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Azure İzleyici'yi kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme

Azure Monitörü'ndeki metrik uyarılar, ölçümlerinizden biri bir eşiği geçtiğinde bilgilendirilmenin bir yolunu sağlar. Ölçüm uyarıları bir dizi çok boyutlu platform ölçümleri, özel ölçümler, Application Insights standart ve özel ölçümleri üzerinde çalışır. Bu makalede, Azure portalı ve Azure CLI aracılığıyla metrik uyarı kurallarının nasıl oluşturulacağını, görüntülenebildiğini ve yönetileceğini açıklayacağız. [Ayrıca, ayrı bir makalede](alerts-metric-create-templates.md)açıklanan Azure Kaynak Yöneticisi şablonlarını kullanarak metrik uyarı kuralları da oluşturabilirsiniz.

[Metrik uyarılara genel bakıştan](alerts-metric-overview.md)metrik uyarıların nasıl çalıştığı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="create-with-azure-portal"></a>Azure portalı ile oluşturma

Aşağıdaki yordam, Azure portalında metrik uyarı kuralının nasıl oluşturulacağını açıklar:

1. [Azure portalında,](https://portal.azure.com) **Monitör'e**tıklayın. Monitör bıçağı tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

2. **Uyarılar'ı** tıklatın ve **+Yeni uyarı kuralını**tıklatın.

    > [!TIP]
    > Çoğu kaynak bıçakları da **İzleme**altında kendi kaynak menüsünde **Uyarılar** var , siz de oradan uyarılar oluşturabilirsiniz.

3. Yüklenen bağlam bölmesinde **hedefi seç'i**tıklatın, uyarmak istediğiniz hedef kaynağı seçin. İzlemek istediğiniz kaynağı bulmak için **Abonelik** ve **Kaynak türü** açılır düşüşlerini kullanın. Kaynağınızı bulmak için arama çubuğunu da kullanabilirsiniz.

4. Seçili kaynağın üzerinde uyarı oluşturabileceğiniz ölçümleri varsa, sağ alttaki **Kullanılabilir sinyaller** ölçümleri içerir. Bu [makalede](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported)metrik uyarılar için desteklenen kaynak türlerinin tam listesini görüntüleyebilirsiniz.

5. Hedef bir kaynak seçtikten sonra **Ekle koşulunu**tıklatın.

6. Kaynak için desteklenen sinyallerin listesini görürsünüz, üzerinde uyarı oluşturmak istediğiniz metriyi seçin.

7. Son altı saat için metrik için bir grafik görürsünüz. Metrik için daha uzun geçmişi görmek için seçmek için **Grafik dönemi** açılır düşüşünü kullanın.

8. Ölçümün boyutları varsa, sunulan bir boyut tablosu görürsünüz. Boyut başına bir veya daha fazla değer seçin.
    - Görüntülenen boyut değerleri, son üç güne ait metrik verilere dayanır.
    - Aradığınız boyut değeri görüntülenmiyorsa, özel bir değer eklemek için "+" seçeneğini tıklayın.
    - Boyutlardan herhangi biri için de **seçim \* ** yapabilirsiniz. **Select, \* ** bir boyut için seçimi dinamik olarak tüm geçerli ve gelecekteki değerlere ölçeklendirecek.

    Metrik uyarı kuralı seçilen tüm değer kombinasyonları için durumu değerlendirir. [Çok boyutlu ölçümlerde uyarının nasıl çalıştığı hakkında daha fazla bilgi edinin.](alerts-metric-overview.md)

9. **Eşik** türünü, **Operatör'ünü**ve **Toplama türünü**seçin. Bu, metrik uyarı kuralının değerlendireceği mantığı belirler.
    - **Statik** eşik kullanıyorsanız, **Eşik değeri**tanımlamaya devam edin. Metrik grafik, makul bir eşiğin ne olabileceğini belirlemeye yardımcı olabilir.
    - **Dinamik** bir eşik kullanıyorsanız, Eşik **duyarlılığını**tanımlamaya devam edin. Metrik grafik, son verilere dayalı olarak hesaplanan eşikleri görüntüler. [Dinamik Eşikler durum türü ve duyarlılık seçenekleri hakkında daha fazla bilgi edinin.](alerts-dynamic-thresholds.md)

10. İsteğe bağlı olarak, Toplama **tanecikliliğini** ve **değerlendirme sıklığını**ayarlayarak durumu hassaslaştırın. 

11. **Bitti**’ye tıklayın.

12. Karmaşık bir uyarı kuralını izlemek istiyorsanız, isteğe bağlı olarak başka bir ölçüt ekleyin. Şu anda kullanıcılar tek bir ölçüt olarak Dinamik Eşikler ölçütleri ile uyarı kuralları olabilir.

13. **Uyarı kural adı,** **Açıklama**ve **Önem**derecesi gibi **Uyarı ayrıntılarını** doldurun.

14. Varolan bir eylem grubu seçerek veya yeni bir eylem grubu oluşturarak bir eylem grubu uyarıya ekleyin.

15. Metrik uyarı kuralını kaydetmek için **Bitti'yi** tıklatın.

> [!NOTE]
> Portal aracılığıyla oluşturulan metrik uyarı kuralları, hedef kaynakla aynı kaynak grubunda oluşturulur.

## <a name="view-and-manage-with-azure-portal"></a>Azure portalı ile görüntüleme ve yönetme

Uyarılar altında Kuralları Yönet'i kullanarak metrik uyarı kurallarını görüntüleyebilir ve yönetebilirsiniz. Aşağıdaki yordam, metrik uyarı kurallarınızı nasıl görüntülediğinizi ve bunlardan birini nasıl değerlendirdiğinizi gösterir.

1. Azure portalında, **Monitör'e** gidin

2. **Uyarılar** ve **Yönetme kurallarına** tıklayın

3. Yönet **kuralları** bladeinde, abonelikler arasında tüm uyarı kurallarınızı görüntüleyebilirsiniz. **Kaynak grubu, Kaynak** **türü**ve **Kaynak'ı**kullanarak kuralları daha fazla filtreleyebilirsiniz. Yalnızca metrik uyarıları görmek istiyorsanız, **Ölçümler** olarak Sinyal türünü seçin.

    > [!TIP]
    > Yönet **kuralları** bıçak larında birden çok uyarı kuralı seçebilir ve bunları etkinleştirebilir/devre dışı bırakabilirsiniz. Bu, belirli hedef kaynakların bakım altına alınması gerektiğinde yararlı olabilir

4. Yeniden görüntülemek istediğiniz metrik uyarı kuralının adını tıklatın

5. Edit Kuralı'nda, yönetmek istediğiniz **Uyarı ölçütlerini** tıklatın. Metrik, eşik koşulunu ve diğer alanları gerektiği gibi değiştirebilirsiniz

    > [!NOTE]
    > Metrik uyarı oluşturulduktan sonra **Hedef kaynağı** ve Uyarı **Kural Adı'nı** ayarlayamaz.

6. Yapılarınızı kaydetmek için **Bitti'yi** tıklatın.

## <a name="with-azure-cli"></a>Azure CLI ile

Önceki bölümlerde Azure portalını kullanarak metrik uyarı kurallarının nasıl oluşturulacağını, görüntülenebildiğini ve yönetilenanlatılır. Bu bölümde, platformlar arası [Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanarak aynı şeyin nasıl yapılacağını açıklayacağız. Azure CLI'yi kullanmaya başlamanın en hızlı yolu [Azure Bulut BulutU Bulutu'ndan](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)geçer. Bu makale için Cloud Shell'i kullanacağız.

1. Azure portalına gidin, **Cloud Shell'e**tıklayın.

2. Komut isteminde, komut ve ``--help`` nasıl kullanılacağı hakkında daha fazla bilgi edinmek için seçenekli komutları kullanabilirsiniz. Örneğin, aşağıdaki komut, metrik uyarıları oluşturmak, görüntülemek ve yönetmek için kullanılabilen komutların listesini gösterir

    ```azurecli
    az monitor metrics alert --help
    ```

3. Bir VM'deki ortalama Yüzde CPU'su 90'dan büyükse izleyen basit bir metrik uyarı kuralı oluşturabilirsiniz

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. Kaynak grubundaki tüm metrik uyarıları aşağıdaki komutu kullanarak görüntüleyebilirsiniz

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. Kuralın adını veya kaynak kimliğini kullanarak belirli bir metrik uyarı kuralının ayrıntılarını görebilirsiniz.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. Aşağıdaki komutu kullanarak bir metrik uyarı kuralını devre dışı kullanabilirsiniz.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. Aşağıdaki komutu kullanarak bir metrik uyarı kuralını silebilirsiniz.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kaynak Yöneticisi Şablonlarını kullanarak metrik uyarılar oluşturun.](../../azure-monitor/platform/alerts-metric-create-templates.md)
- [Metrik uyarıların nasıl çalıştığını anlayın.](alerts-metric-overview.md)
- [Dinamik Eşikler koşuluna sahip metrik uyarıların nasıl çalıştığını anlayın.](alerts-dynamic-thresholds.md)
- [Metrik uyarılar için web kancası şemasını anlama](../../azure-monitor/platform/alerts-metric-near-real-time.md#payload-schema)

