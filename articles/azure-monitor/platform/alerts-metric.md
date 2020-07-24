---
title: Azure Izleyici kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme
description: Azure portal veya CLı kullanarak ölçüm uyarı kuralları oluşturma, görüntüleme ve yönetme hakkında bilgi edinin.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 03/13/2020
ms.subservice: alerts
ms.openlocfilehash: b535cda8fe7d98748eb0657a599a61ed1f8549d2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87045367"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Azure İzleyici'yi kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme

Azure Izleyici 'de ölçüm uyarıları, ölçümlerinizin bir eşiğin kesiştiği durumlarda bildirim almanın bir yolunu sağlar. Ölçüm uyarıları bir dizi çok boyutlu platform ölçümleri, özel ölçümler, Application Insights standart ve özel ölçümleri üzerinde çalışır. Bu makalede, Azure portal ve Azure CLı aracılığıyla ölçüm uyarı kuralları oluşturma, görüntüleme ve yönetme hakkında açıklama göndereceğiz. Ayrıca, [ayrı bir makalede](alerts-metric-create-templates.md)açıklanan Azure Resource Manager şablonlarını kullanarak da ölçüm uyarı kuralları oluşturabilirsiniz.

Ölçüm uyarılarının [ölçüm uyarılarından genel bakışta](alerts-metric-overview.md)nasıl çalıştığı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="create-with-azure-portal"></a>Azure portalı ile oluşturma

Aşağıdaki yordamda Azure portal bir ölçüm uyarısı kuralının nasıl oluşturulacağı açıklanmaktadır:

1. [Azure Portal](https://portal.azure.com), **izleyici**' ye tıklayın. Izleyici dikey penceresi tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

2. **Uyarılar** ' a ve ardından **+ Yeni uyarı kuralı**' na tıklayın.

    > [!TIP]
    > Çoğu kaynak dikey penceresinde, **izleme**altındaki kaynak menüsünde **da uyarılar bulunur** . Ayrıca, burada da uyarı oluşturabilirsiniz.

3. **Hedef Seç**' e tıklayın, yüklenen bağlam bölmesinde, uyarmak istediğiniz hedef kaynağı seçin. İzlemek istediğiniz kaynağı bulmak için **abonelik** ve **kaynak türü** açılan listelerini kullanın. Kaynağı bulmak için arama çubuğunu da kullanabilirsiniz.

4. Seçili kaynakta, uyarı oluşturabileceğiniz ölçümler varsa, en alt sağ tarafta **bulunan kullanılabilir sinyaller** ölçümler dahil edilir. Bu [makaledeki](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported)ölçüm uyarıları için desteklenen kaynak türlerinin tam listesini görebilirsiniz.

5. Bir hedef kaynak seçtikten sonra **Koşul Ekle**' ye tıklayın.

6. Kaynak için desteklenen sinyallerin bir listesini göreceksiniz, üzerinde uyarı oluşturmak istediğiniz ölçümü seçin.

7. En son altı saat için ölçüm için bir grafik görürsünüz. Ölçüm için daha uzun geçmişi görmeyi seçmek için **grafik dönemi** açılan listesini kullanın.

8. Ölçümün boyutları varsa, bir Boyutlar tablosu görürsünüz. Her boyut için bir veya daha fazla değer seçin.
    - Görüntülenen boyut değerleri, son üç gündeki ölçüm verilerini temel alır.
    - Aradığınız boyut değeri görüntülenmiyorsa, özel bir değer eklemek için "+" düğmesine tıklayın.
    - Boyutlardan herhangi biri için de **seçim \* ** yapabilirsiniz. **Seç \* ** seçimi dinamik olarak bir boyut için geçerli ve gelecekteki değerlere ölçeklendirecektir.

    Ölçüm uyarısı kuralı, seçilen tüm değer birleşimlerinin koşulunu değerlendirir. [Çok boyutlu ölçümlerde uyarı oluşturma hakkında daha fazla bilgi edinin](alerts-metric-overview.md).

9. **Eşik** türü, **işleç**ve **toplama türünü**seçin. Bu, ölçüm uyarı kuralının değerlendileceğini belirten mantığı belirleyecek.
    - **Statik** eşik kullanıyorsanız, bir **eşik değeri**tanımlamaya devam edin. Ölçüm grafiği ne kadar makul bir eşik olabileceğini belirlemenize yardımcı olabilir.
    - **Dinamik** eşik kullanıyorsanız, **eşik duyarlılığını**tanımlamaya devam edin. Ölçüm grafiği, hesaplanan eşikleri son verilere göre görüntüler. [Dinamik eşikler durum türü ve duyarlılık seçenekleri hakkında daha fazla bilgi edinin](alerts-dynamic-thresholds.md).

10. İsteğe bağlı olarak, **toplama ayrıntı** düzeyini ve **değerlendirme sıklığını**ayarlayarak koşulu daraltın. 

11. **Bitti**’ye tıklayın.

12. İsteğe bağlı olarak, karmaşık bir uyarı kuralını izlemek istiyorsanız başka ölçütler ekleyin. Şu anda kullanıcılar, dinamik eşik ölçütlerine sahip uyarı kurallarına tek bir ölçüt olarak sahip olabilir.

13. **Uyarı kuralı adı**, **açıklaması**ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.

14. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu ekleyin.

15. Ölçüm uyarı kuralını kaydetmek için **bitti** ' ye tıklayın.

> [!NOTE]
> Portal üzerinden oluşturulan ölçüm uyarısı kuralları, hedef kaynakla aynı kaynak grubunda oluşturulur.

## <a name="view-and-manage-with-azure-portal"></a>Azure portal ile görüntüleyin ve yönetin

Uyarılar altındaki kuralları Yönet dikey penceresini kullanarak ölçüm uyarı kurallarını görüntüleyebilir ve yönetebilirsiniz. Aşağıdaki yordamda, ölçüm uyarı kurallarınızı nasıl görüntüleyebileceğiniz ve bunlardan birinin nasıl düzenleneceği gösterilmektedir.

1. Azure portal ' de **izleyici** ' ye gidin

2. **Uyarılar** ' a tıklayın ve **kuralları yönetin**

3. **Kuralları Yönet** dikey penceresinde tüm uyarı kurallarınızı abonelikler arasında görüntüleyebilirsiniz. **Kaynak grubu**, **kaynak türü**ve **kaynak**kullanarak kuralları daha fazla filtreleyebilirsiniz. Yalnızca ölçüm uyarılarını görmek istiyorsanız, ölçüm olarak **sinyal türü** ' nü seçin.

    > [!TIP]
    > **Kuralları Yönet** dikey penceresinde birden çok uyarı kuralı seçebilir ve bunları etkinleştirebilir/devre dışı bırakabilirsiniz. Bu, belirli hedef kaynakların bakım altına konulma ihtiyacı olduğunda yararlı olabilir

4. Düzenlemek istediğiniz ölçüm uyarısı kuralının adına tıklayın

5. Düzenle kuralında, düzenlemek istediğiniz **Uyarı ölçütlerine** tıklayın. Ölçüyü, eşik koşulunu ve diğer alanları gerektiği gibi değiştirebilirsiniz

    > [!NOTE]
    > Ölçüm uyarısı oluşturulduktan sonra **hedef kaynak** ve **Uyarı kuralı adını** düzenleyemezsiniz.

6. Düzenlemelerinizi kaydetmek için **bitti** ' ye tıklayın.

## <a name="with-azure-cli"></a>Azure CLI ile

Önceki bölümlerde Azure portal kullanarak ölçüm uyarı kurallarının nasıl oluşturulacağı, görüntüleneceği ve yönetileceği açıklanmaktadır. Bu bölümde, platformlar arası [Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanarak nasıl yapılacağı açıklanır. Azure CLı 'yı kullanmaya başlamanın en hızlı yolu [Azure Cloud Shell](../../cloud-shell/overview.md?view=azure-cli-latest)kullanmaktır. Bu makalede Cloud Shell kullanacağız.

1. Azure portal git **Cloud Shell**' e tıklayın.

2. Komut isteminde komutlar ``--help`` ve kullanma hakkında daha fazla bilgi edinmek için seçeneğini kullanabilirsiniz. Örneğin, aşağıdaki komut, ölçüm uyarılarını oluşturmak, görüntülemek ve yönetmek için kullanabileceğiniz komutların listesini gösterir

    ```azurecli
    az monitor metrics alert --help
    ```

3. Bir VM üzerinde ortalama yüzde CPU 90 ' den büyükse, izleyen bir basit ölçüm uyarı kuralı oluşturabilirsiniz

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. Aşağıdaki komutu kullanarak tüm ölçüm uyarılarını bir kaynak grubunda görüntüleyebilirsiniz

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. Kuralın adını veya kaynak KIMLIĞINI kullanarak belirli bir ölçüm uyarısı kuralının ayrıntılarını görebilirsiniz.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. Aşağıdaki komutu kullanarak bir ölçüm uyarısı kuralını devre dışı bırakabilirsiniz.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. Aşağıdaki komutu kullanarak bir ölçüm uyarısı kuralını silebilirsiniz.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager şablonları kullanarak ölçüm uyarıları oluşturun](../../azure-monitor/platform/alerts-metric-create-templates.md).
- [Ölçüm uyarılarının nasıl çalıştığını anlayın](alerts-metric-overview.md).
- [Dinamik Eşik koşulu ile ölçüm uyarılarının nasıl çalıştığını anlayın](alerts-dynamic-thresholds.md).
- [Ölçüm uyarıları için Web kancası şemasını anlayın](../../azure-monitor/platform/alerts-metric-near-real-time.md#payload-schema)
