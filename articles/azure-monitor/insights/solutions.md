---
title: Azure Izleyici 'de çözümleri izleme | Microsoft Docs
description: Azure Izleyici 'de izleme çözümleri, belirli bir sorun alanı etrafında ölçüm sağlayan bir mantık, görselleştirme ve veri alma kuralları koleksiyonudur.  Bu makale, izleme çözümlerini yükleme ve kullanma hakkında bilgi sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/12/2020
ms.openlocfilehash: 2da00b44be7018bef80e466231efb75a8eb99754
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87081551"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Azure Izleyici 'de çözümleri izleme

İzleme çözümleri, belirli bir uygulama veya hizmet işleminin ek analizini sağlamak için Azure 'daki hizmetlerden yararlanır. Bu makalede, Azure 'daki çözümleri izleme hakkında kısa bir genel bakış ve bunları kullanma ve yükleme hakkındaki ayrıntılar sunulmaktadır. Azure Izleyici 'ye, kullandığınız tüm uygulama ve hizmetler için izleme çözümleri ekleyebilirsiniz. Bunlar genellikle ücretsiz olarak kullanılabilir ancak kullanım ücretlerini çağırabilecek veri toplar.

## <a name="use-monitoring-solutions"></a>İzleme çözümlerini kullanma

Çalışma alanında yüklü her bir çözümün kutucuğunu görüntülemek için Azure Izleyici 'de **genel bakış** sayfasını açın.

1. [Azure portalına](https://ms.portal.azure.com) gidin. **İzleme**araması yapın ve seçin.
1. **Öngörüler** menüsünde, **daha fazla**' yı seçin.
1. Çalışma alanını veya kutucuklar için kullanılan zaman aralığını değiştirmek için ekranın üst kısmındaki açılan kutuları kullanın.
1. Bir çözümün kutucuğuna tıklayarak toplanan verilerin daha ayrıntılı analizini içeren görünümünü açın.

![Genel Bakış](media/solutions/overview.png)

İzleme çözümleri birden çok Azure kaynağı türü içerebilir ve bir çözüme dahil olan tüm kaynakları diğer kaynaklar gibi görüntüleyebilirsiniz. Örneğin, çözüme dahil edilen herhangi bir günlük sorgusu, [sorgu Gezgini](../log-query/get-started-portal.md#load-queries) 'Ndeki **çözüm sorguları** altında listelenmiştir ve [günlük sorgularıyla](../log-query/log-query-overview.md)geçici analiz gerçekleştirirken bu sorguları kullanabilirsiniz.

## <a name="list-installed-monitoring-solutions"></a>Yüklü izleme çözümlerini Listele

### <a name="portal"></a>[Portal](#tab/portal)

Aboneliğinizde yüklü izleme çözümlerini listelemek için aşağıdaki yordamı kullanın.

1. [Azure portalına](https://ms.portal.azure.com) gidin. **Çözümler**arayın ve seçin.
1. Çalışma alanlarınızın tümünde yüklü olan çözümler listelenir. Çözümün adının ardından yüklendiği çalışma alanının adı gelir.
1. Abonelik veya kaynak grubuna göre filtrelemek için ekranın üst kısmındaki açılan kutuları kullanın.

![Tüm çözümleri Listele](media/solutions/list-solutions-all.png)

Bir çözümün adına tıklayarak Özet sayfasını açın. Bu sayfada çözüme dahil edilen görünümler görüntülenir ve çözümün kendisi ve çalışma alanı için farklı seçenekler sunulur. Çözümleri listelemek için Yukarıdaki yordamlardan birini kullanarak bir çözümün Özet sayfasını görüntüleyin ve sonra çözümün adına tıklayın.

![Çözüm özellikleri](media/solutions/solution-properties.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aboneliğinizde yüklü izleme çözümlerini listelemek için [az Monitor Log-Analytics çözüm List](/cli/azure/ext/log-analytics-solution/monitor/log-analytics/solution#ext-log-analytics-solution-az-monitor-log-analytics-solution-list) komutunu kullanın.   Komutu çalıştırmadan önce `list` , [Izleme çözümü yüklerken](#install-a-monitoring-solution)bulunan önkoşulları izleyin.

```azurecli
# List all log-analytics solutions in the current subscription.
az monitor log-analytics solution list

# List all log-analytics solutions for a specific subscription
az monitor log-analytics solution list --subscription MySubscription

# List all log-analytics solutions in a resource group
az monitor log-analytics solution list --resource-group MyResourceGroup
```

* * *

## <a name="install-a-monitoring-solution"></a>İzleme çözümü yüklemesi

### <a name="portal"></a>[Portal](#tab/portal)

Microsoft ve iş ortaklarından izleme çözümlerini [Azure Marketi](https://azuremarketplace.microsoft.com)' nden edinebilirsiniz. Aşağıdaki yordamı kullanarak, kullanılabilir çözümleri arayabilir ve bunları yükleyebilirsiniz. Bir çözüm yüklediğinizde, çözümün yükleneceği ve verilerinin toplanacağı bir [Log Analytics çalışma alanı](../platform/manage-access.md) seçmeniz gerekir.

1. [Aboneliğiniz için çözümler listesinden](#list-installed-monitoring-solutions) **Ekle**' ye tıklayın.
1. Bir çözüme gözatıp arayın. Ayrıca, [Bu arama bağlantısından](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/management-tools?page=1&subcategories=management-solutions)çözümlere da gidebilirsiniz.
1. İstediğiniz izleme çözümünü bulun ve açıklamasını okuyun.
1. Yükleme işlemini başlatmak için **Oluştur** ' a tıklayın.
1. Yükleme işlemi başladığında Log Analytics çalışma alanını belirtmeniz ve çözüm için gerekli tüm yapılandırmaları sağlamanız istenir.

![Çözüm yüklemesi](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Topluluktan bir çözüm yükler

Topluluk üyeleri, yönetim çözümlerini Azure hızlı başlangıç şablonlarına gönderebilir. Bu çözümleri doğrudan yükleyebilir veya daha sonra yüklenmek üzere şablonları indirebilirsiniz.

1. Çalışma alanını ve hesabı bağlamak için [Log Analytics çalışma alanı ve Otomasyon hesabı](#log-analytics-workspace-and-automation-account) ' nda açıklanan süreci izleyin.
2. [Azure hızlı başlangıç şablonlarına](https://azure.microsoft.com/documentation/templates/)gidin.
3. İlgilendiğiniz bir çözüm arayın.
4. Ayrıntılarını görüntülemek için sonuçlardan çözümü seçin.
5. **Azure 'A dağıt** düğmesine tıklayın.
6. Çözümdeki parametrelerin değerlerinin yanı sıra kaynak grubu ve konum gibi bilgileri sağlamanız istenir.
7. Çözümü yüklemek için **satın al** ' a tıklayın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="prepare-your-environment"></a>Ortamınızı hazırlama

1. Azure CLI'yi yükleme

   CLı başvuru komutlarını çalıştırmadan önce [Azure CLI 'yı yüklemeniz](/cli/azure/install-azure-cli) gerekir.  İsterseniz, bu makaledeki adımları tamamlayabilmeniz için Azure Cloud Shell de kullanabilirsiniz.  Azure Cloud Shell, tarayıcınız aracılığıyla kullandığınız etkileşimli bir kabuk ortamıdır.  Aşağıdaki yöntemlerden birini kullanarak Cloud Shell başlatın:

   - Cloud Shell giderek açın[https://shell.azure.com](https://shell.azure.com)

   - [Azure Portal](https://portal.azure.com) sağ üst köşedeki menü çubuğunda bulunan **Cloud Shell** düğmesini seçin

1. Giriş yapın.

   CLı 'nın yerel bir yüklemesini kullanıyorsanız [az Login](/cli/azure/reference-index#az-login) komutunu kullanarak oturum açın.  Kimlik doğrulama işlemini gerçekleştirmek için terminalinizde görünen adımları izleyin.

    ```azurecli
    az login
    ```

1. Uzantıyı yükler `log-analytics`

   Bu `log-analytics` komut, çekirdek Azure CLI 'nın deneysel uzantısıdır. Uzantı başvuruları hakkında daha fazla bilgi için bkz. [Azure CLI ile uzantı kullanımı](/cli/azure/azure-cli-extensions-overview?).

   ```azurecli
   az extension add --name log-analytics
   ```

   Aşağıdaki uyarı beklenmektedir.

   ```output
   The installed extension `log-analytics` is experimental and not covered by customer support.  Please use with discretion.
   ```

### <a name="install-a-solution-with-the-azure-cli"></a>Azure CLı ile bir çözüm yüklemesi

Bir çözüm yüklediğinizde, çözümün yükleneceği ve verilerinin toplanacağı bir [Log Analytics çalışma alanı](/azure/azure-monitor/platform/manage-access) seçmeniz gerekir.  Azure CLı ile, [az Monitor Log-Analytics çalışma alanı](/cli/azure/monitor/log-analytics/workspace) başvuru komutlarını kullanarak çalışma alanlarını yönetirsiniz.  Çalışma alanını ve hesabı bağlamak için [Log Analytics çalışma alanı ve Otomasyon hesabı](#log-analytics-workspace-and-automation-account) ' nda açıklanan süreci izleyin.

İzleme çözümü yüklemek için [az Monitor Log-Analytics çözüm Create](/cli/azure/ext/log-analytics-solution/monitor/log-analytics/solution) ' i kullanın.  Köşeli parantezler içindeki parametreler isteğe bağlıdır.

```azurecli
az monitor log-analytics solution create --name
                                         --plan-product
                                         --plan-publisher
                                         --resource-group
                                         --workspace
                                         [--no-wait]
                                         [--tags]
```

Bu, OMSGallery/Container plan ürünü için bir Log Analytics çözümü oluşturan bir kod örneğidir.

```azurecli
az monitor log-analytics solution create --resource-group MyResourceGroup \
                                         --name Containers({SolutionName}) \
                                         --tags key=value \
                                         --plan-publisher Microsoft  \
                                         --plan-product "OMSGallery/Containers" \
                                         --workspace "/subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/ \
                                           Microsoft.OperationalInsights/workspaces/{WorkspaceName}"
```

* * *

## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı

Tüm izleme çözümleri, çözüm tarafından toplanan verileri depolamak ve günlük aramalarını ve görünümlerini barındırmak için bir [Log Analytics çalışma alanı](../platform/manage-access.md) gerektirir. Bazı çözümler ayrıca runbook 'ları ve ilgili kaynakları içeren bir [Otomasyon hesabı](../../automation/automation-security-overview.md) gerektirir. Çalışma alanı ve hesap aşağıdaki gereksinimlere uymalıdır.

* Her bir çözümün yüklemesi yalnızca bir Log Analytics çalışma alanı ve bir Otomasyon hesabı kullanabilir. Çözümü birden çok çalışma alanına ayrı olarak yükleyebilirsiniz.
* Bir çözüm bir Otomasyon hesabı gerektiriyorsa, Log Analytics çalışma alanı ve Otomasyon hesabı bir diğeri ile bağlantılı olmalıdır. Log Analytics çalışma alanı yalnızca bir Otomasyon hesabına bağlanabilir ve bir Otomasyon hesabı yalnızca bir Log Analytics çalışma alanına bağlanabilir.
* Bağlantı sağlamak için, Log Analytics çalışma alanı ve Otomasyon hesabı aynı abonelikte olmalıdır, ancak aynı bölgeye dağıtılmış farklı kaynak gruplarında bulunabilir. Özel durum, Doğu ABD 2 Doğu ABD Region ve Automation hesabındaki bir çalışma alanıdır.

Azure Marketi aracılığıyla bir çözüm yüklediğinizde, sizden bir çalışma alanı ve Otomasyon hesabı istenir. Bunlar arasındaki bağlantı, henüz bağlanmadıysa oluşturulur.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı arasındaki bağlantıyı doğrulama

Aşağıdaki yordamı kullanarak bir Log Analytics çalışma alanı ve Otomasyon hesabı arasındaki bağlantıyı doğrulayabilirsiniz.

1. Azure portal Otomasyon hesabını seçin.
1. Menünün **Ilgili kaynaklar** bölümüne gidin ve **bağlantılı çalışma alanı**' nı seçin.
1. **Çalışma alanı** bir Otomasyon hesabına bağlanmışsa, Bu sayfa bağlandığı çalışma alanını listeler. Listelenen çalışma alanının adını seçerseniz, bu çalışma alanının genel bakış sayfasına yönlendirilirsiniz.

## <a name="remove-a-monitoring-solution"></a>İzleme çözümünü kaldırma

### <a name="portal"></a>[Portal](#tab/portal)

Portalı kullanarak yüklü bir çözümü kaldırmak için, [yüklü çözümler listesinde](#list-installed-monitoring-solutions)bulun. Çözümün adına tıklayarak Özet sayfasını açın ve ardından **Sil**' e tıklayın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı 'yı kullanarak yüklü bir çözümü kaldırmak için [az Monitor Log-Analytics çözüm Delete](/cli/azure/ext/log-analytics-solution/monitor/log-analytics/solution#ext-log-analytics-solution-az-monitor-log-analytics-solution-delete) komutunu kullanın.

```azurecli
az monitor log-analytics solution delete --name
                                         --resource-group
                                         [--no-wait]
                                         [--yes]
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft 'un izleme çözümlerinin bir listesini](solutions-inventory.md)alın.
* İzleme çözümleri tarafından toplanan verileri analiz etmek için [sorgular oluşturmayı](../log-query/log-query-overview.md) öğrenin.
* [Azure izleyici için tüm Azure CLI komutlarına](/cli/azure/azure-cli-reference-for-monitor)bakın.
