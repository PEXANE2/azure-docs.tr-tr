---
title: Azure Monitör'ü kullanarak klasik metrik uyarıları oluşturma ve yönetme
description: Klasik metrik uyarı kuralları oluşturmak, görüntülemek ve yönetmek için Azure portalı, CLI veya Powershell'i nasıl kullanacağınızı öğrenin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 09/18/2018
ms.openlocfilehash: e635c243a887690fb1f7a5dcd017c1130d74a747
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114576"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Azure Monitör'ü kullanarak klasik metrik uyarıları oluşturma, görüntüleme ve yönetme

Azure Monitörü'ndeki klasik metrik uyarılar, ölçümlerinizden biri eşiği geçtiğinde bildirim almanın bir yolunu sağlar. Klasik metrik uyarılar, yalnızca boyutsal olmayan ölçümler de uyarmaya olanak tanıyan eski bir işlevdir. Klasik metrik uyarılar üzerinde işlevselliği geliştirmiş Metrik uyarıları adlı mevcut yeni bir işlev vardır. Yeni metrik [uyarılar](../../azure-monitor/platform/alerts-metric-overview.md)işlevselliği hakkında daha fazla bilgi edinebilirsiniz. Bu makalede, Azure portalı, Azure CLI ve Powershell aracılığıyla klasik metrik uyarı kurallarının nasıl oluşturulacağını, görüntülenebildiğini ve yönetileceğini açıklayacağız.

## <a name="with-azure-portal"></a>Azure portalı ile

1. [Portalda,](https://portal.azure.com/)izlemek istediğiniz kaynağı bulun ve sonra seçin.

2. **İzleme** bölümünde **Uyarılar (Klasik)** seçeneğini belirleyin. Metin ve simge farklı kaynaklar için biraz farklı olabilir. **Uyarıları (Klasik)** burada bulamazsanız, **uyarıları Uyarılar** veya Uyarı **Kuralları'nda**bulabilirsiniz.

    ![İzleme](media/alerts-classic-portal/AlertRulesButton.png)

3. Metrik **uyarı ekle (klasik) komutunu** seçin ve sonra alanları doldurun.

    ![Uyarı Ekle](media/alerts-classic-portal/AddAlertOnlyParamsPage.png)

4. Uyarı kuralınızı **adlandırın.** Ardından, bildirim e-postalarında da görünen bir **Açıklama**seçin.

5. İzlemek istediğiniz **Metrik'i** seçin. Ardından, metrik için bir **Koşul** ve **Eşik** değeri seçin. Ayrıca, **Period** uyarı tetiklenmeden önce metrik kuralın karşılanması gereken süreyi de seçin. Örneğin, "Son 5 dakika içinde" dönemini kullanıyorsanız ve uyarınız %80'in üzerinde bir CPU arıyorsa, CPU 5 dakika boyunca sürekli olarak %80'in üzerinde olduğunda uyarı tetikler. İlk tetikleyici oluştuktan sonra, CPU 5 dakika boyunca %80'in altında kaldığında tekrar tetikler. CPU metrik ölçümü her dakika gerçekleşir.

6. **E-posta sahiplerini seçin...** uyarı çıktığında yöneticilerin ve yardımcı yöneticilerin e-posta bildirimleri almasını istiyorsanız.

7. Uyarı çıktığında ek e-posta adreslerine bildirim göndermek istiyorsanız, bunları **Ek Yönetici e-posta(lar)** alanına ekleyin. Aşağıdaki biçimde, yarı kolonlu birden fazla e-postayı ayırın: *contoso.com;email2\@\@contoso.com*

8. Uyarı çıktığında çağrılmasını istiyorsanız **Webhook** alanına geçerli bir URI koyun.

9. Azure Otomasyonu kullanıyorsanız, uyarı çıktığında çalıştırılacak bir runbook seçebilirsiniz.

10. Uyarıyı oluşturmak için **Tamam'ı** seçin.

Birkaç dakika içinde, uyarı etkin dir ve daha önce açıklandığı gibi tetikler.

Bir uyarı oluşturduktan sonra, uyarıyı seçebilir ve aşağıdaki görevlerden birini yapabilirsiniz:

* Bir önceki güne ait metrik eşiği ve gerçek değerleri gösteren bir grafiği görüntüleyin.
* Düzenle veya silin.
* Bu uyarı için bildirimleri geçici olarak durdurmak veya almaya devam etmek istiyorsanız **devre dışı bırakın** veya **etkinleştirin.**

## <a name="with-azure-cli"></a>Azure CLI ile

Önceki bölümlerde Azure portalını kullanarak metrik uyarı kurallarının nasıl oluşturulacağını, görüntülenebildiğini ve yönetilenanlatılıyor. Bu bölümde, platformlar arası [Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanarak aynı şeyin nasıl yapılacağını açıklayacağız. Azure CLI'yi kullanmaya başlamanın en hızlı yolu [Azure Bulut BulutU Bulutu'ndan](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)geçer.

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Kaynak grubundaki tüm klasik metrik uyarı kurallarını alma

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Belirli bir klasik metrik uyarı kuralının ayrıntılarını görün

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Klasik metrik uyarı kuralı oluşturma

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Klasik metrik uyarı kuralını silme

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>PowerShell ile

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu bölümler, PowerShell komutlarının klasik metrik uyarıları nasıl oluşturacağını, görüntülenin ve yönetilenleri gösterir. Makaledeki örnekler, klasik metrik uyarılar için Azure Monitor cmdlets'i nasıl kullanabileceğinizi göstermektedir.

1. Henüz yapmadıysanız, PowerShell'i bilgisayarınızda çalışacak şekilde ayarlayın. Daha fazla bilgi için [PowerShell'in nasıl yüklenir ve yapılandırılın.](/powershell/azure/overview) [Azure Monitor (Insights) Cmdlets](https://docs.microsoft.com/powershell/module/az.applicationinsights)adresindeki Azure Monitor PowerShell cmdlet'lerinin tüm listesini de inceleyebilirsiniz.

2. İlk olarak, Azure aboneliğinizde oturum açın.

    ```powershell
    Connect-AzAccount
    ```

3. Ekranda bir işaret görürsünüz. Hesabınızı oturum aladıktan sonra Kiracı Kimliği ve varsayılan Abonelik Kimliği görüntülenir. Tüm Azure cmdlet'leri varsayılan aboneliğiniz bağlamında çalışır. Erişiminiz olan aboneliklerin listesini görüntülemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzSubscription
    ```

4. Çalışma bağlamınızı farklı bir aboneliğe değiştirmek için aşağıdaki komutu kullanın:

    ```powershell
    Set-AzContext -SubscriptionId <subscriptionid>
    ```

5. Kaynak grubundaki tüm klasik metrik uyarı kurallarını alabilirsiniz:

    ```powershell
    Get-AzAlertRule -ResourceGroup montest
    ```

6. Klasik metrik uyarı kuralının ayrıntılarını görüntüleyebilirsiniz

    ```powershell
    Get-AzAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. Hedef kaynak için ayarlanan tüm uyarı kurallarını alabilirsiniz. Örneğin, vm'de ayarlanan tüm uyarı kuralları.

    ```powershell
    Get-AzAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
    ```

8. Klasik uyarı kuralları artık PowerShell üzerinden oluşturulamaz. Bir uyarı kuralı oluşturmak için yeni ['Add-AzMetricAlertRule'](/powershell/module/az.monitor/add-azmetricalertrule) komutunu kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak Yöneticisi şablonuyla klasik bir metrik uyarı oluşturun.](../../azure-monitor/platform/alerts-enable-template.md)
- [Webhook kullanarak Azure olmayan bir sistemi bildiren klasik bir metrik uyarınız var.](../../azure-monitor/platform/alerts-webhooks.md)
