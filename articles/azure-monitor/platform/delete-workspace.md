---
title: Azure Günlük Analizi çalışma alanını silme ve kurtarma | Microsoft Dokümanlar
description: Kişisel bir abonelikte bir tane oluşturduysanız veya çalışma alanı modelinizi yeniden yapılandırıyorsanız, Günlük Analizi çalışma alanınızı nasıl sileydiğinizi öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/14/2020
ms.openlocfilehash: 1dceb3db4572ecdaf504745dba1099a5eccead43
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395780"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Azure Günlük Analizi çalışma alanını silme ve kurtarma

Bu makalede, Azure Log Analytics çalışma alanı yumuşak silme kavramı ve silinen çalışma alanının nasıl kurtarılacağA çalışılabilenler açıklanmaktadır. 

## <a name="considerations-when-deleting-a-workspace"></a>Çalışma alanını silerken dikkat edilmesi gerekenler

Bir Log Analytics çalışma alanını sildiğinizde, silinme nin kazara veya kasıtlı olup olmadığını, verileri ve bağlı aracıları da dahil olmak üzere çalışma alanının kurtarılmasına olanak sağlamak için bir yumuşak silme işlemi gerçekleştirilir. Yumuşak silme döneminden sonra, çalışma alanı kaynağı ve verileri kurtarılamaz – verileri kalıcı silme için sıraya alınır ve 30 gün içinde tamamen temizlenir. Çalışma alanı adı 'serbest' ve yeni bir çalışma alanı oluşturmak için kullanabilirsiniz.

> [!NOTE]
> Yumuşak silme davranışını geçersiz kılmak ve çalışma alanınızı kalıcı olarak silmek istiyorsanız, [Kalıcı çalışma alanı silme](#permanent-workspace-delete)adımlarını izleyin.

Hizmet işleminizi olumsuz etkileyebilecek önemli veriler ve yapılandırma olabileceğinden, bir çalışma alanını sildiğinizde dikkatli olmak istersiniz. Verilerini Log Analytics'te depolayan hangi aracıların, çözümlerin ve diğer Azure hizmetlerinin ve kaynaklarının bunlar la ilgili olarak gözden geçirilmesi:

* Yönetim çözümleri
* Azure Otomasyonu
* Windows ve Linux sanal makinelerinde çalışan aracılar
* Ortamınızda Windows ve Linux bilgisayarlarda çalışan aracılar
* System Center Operations Manager

Yumuşak silme işlemi çalışma alanı kaynağını siler ve ilişkili kullanıcıların izni kesilir. Kullanıcılar diğer çalışma alanlarıyla ilişkiliyse, günlük analitiği kullanmaya devam edebilirler.

## <a name="soft-delete-behavior"></a>Yumuşak silme davranışı

Çalışma alanı silme işlemi çalışma alanı Kaynak Yöneticisi kaynağını kaldırır, ancak yapılandırması ve verileri çalışma alanının silindiği görünümünü sağlarken 14 gün boyunca saklanır. Çalışma alanına rapor vermek üzere yapılandırılan tüm aracılar ve Sistem Merkezi Operasyon Yöneticisi yönetim grupları, yumuşak silme döneminde yetim durumda kalır. Hizmet ayrıca, verileri ve bağlı kaynakları da dahil olmak üzere silinen çalışma alanını kurtarmak için bir mekanizma sağlar ve aslında silme işlemini geri alar.

> [!NOTE] 
> Azure Otomasyon hesabınız gibi yüklü çözümler ve bağlantılı hizmetler silme zamanında çalışma alanından kalıcı olarak kaldırılır ve kurtarılamaz. Bunlar, çalışma alanını daha önce yapılandırılmış durumuna getirmek için kurtarma işleminden sonra yeniden yapılandırılmalıdır.

[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [REST API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete)kullanarak veya [Azure portalında](https://portal.azure.com)bir çalışma alanını silebilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. Oturum açabilmek için [Azure portalına](https://portal.azure.com)gidin. 
2. Azure portalında **Tüm hizmetler'i**seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.
3. Günlük Analizi çalışma alanları listesinde bir çalışma alanı seçin ve ardından orta bölmenin üst kısmından **Sil'i** tıklatın.
   ![Çalışma Alanı özellikleri bölmesinden silme seçeneği](media/delete-workspace/log-analytics-delete-workspace.png)
4. Onay iletisi penceresi çalışma alanının silinmesini onaylamanızı istediğinde **Evet'i**tıklatın.
   ![Çalışma alanının silinmesini onaylama](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

### <a name="troubleshooting"></a>Sorun giderme

Log Analytics çalışma alanını silmek için 'Log Analytics Katılımcısı' izinlerine sahip olmalısınız.<br>
Bir hata iletisi alırsanız '*Bu çalışma alanı adı zaten kullanılıyor*' bir çalışma alanı oluştururken, o zamandan beri olabilir:
* Çalışma alanı adı kullanılamıyor ve kuruluşunuzdaki bir kişi veya başka bir müşteri tarafından kullanılıyor.
* Çalışma alanı son 14 gün içinde silinmiş ve adı yumuşak silme dönemi için ayrılmış olarak tutulmuştur. Yumuşak silmeyi geçersiz kılmak ve çalışma alanınızı hemen silmek ve aynı ada sahip yeni bir çalışma alanı oluşturmak için, önce çalışma alanını kurtarmak ve kalıcı silme gerçekleştirmek için aşağıdaki adımları izleyin:<br>
   1. Çalışma alanınızı [kurtarın.](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#recover-workspace)
   2. Çalışma alanınızı [kalıcı olarak silin.](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#permanent-workspace-delete)
   3. Aynı çalışma alanı adını kullanarak yeni bir çalışma alanı oluşturun.


## <a name="permanent-workspace-delete"></a>Kalıcı çalışma alanı silme
Yumuşak silme yöntemi, aynı ayarlar ve çalışma alanı adı ile bir dağıtımı yinelemeniz gereken geliştirme ve sınama gibi bazı senaryolara sığmayabilir. Bu gibi durumlarda çalışma alanınızı kalıcı olarak silebilir ve yumuşak silme süresini "geçersiz kılabilirsiniz". Kalıcı çalışma alanı silme işlemi çalışma alanı adını serbest bırakır ve aynı adı kullanarak yeni bir çalışma alanı oluşturabilirsiniz.


> [!IMPORTANT]
> Geri döndürülemez olduğundan kalıcı çalışma alanı silme işlemini dikkatli kullanın ve çalışma alanınızı ve verilerini kurtaramazsınız.

Kalıcı çalışma alanı silme şu anda REST API üzerinden gerçekleştirilebilir.

> [!NOTE]
> Herhangi bir API isteği, istek üstbilgisine bir Taşıyıcı yetkilendirme belirteci içermelidir.
>
> Belirteci aşağıdakileri kullanarak edinebilirsiniz:
> - [Uygulama kayıtları](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens)
> - Tarayıcıda geliştiricinin konsolunu (F12) kullanarak Azure portalına gidin. **İstek Üstbilgilerinin**altındaki kimlik doğrulama dizesi için toplu iş lerden birine **bakın.** Bu desen yetkilendirme *olacak: Taşıyıcı <token> *. Örneklerde gösterildiği gibi api çağrınıza bunu kopyalayın ve ekleyin.
> - Azure REST dokümantasyon sitesine gidin. herhangi bir API'de **deneyin,** Taşıyıcı belirteci kopyalayın ve API çağrınıza ekleyin.
Çalışma alanınızı kalıcı olarak silmek için Çalışma Alanlarını kullanın - REST API çağrısını kuvvet etiketiyle [silin:]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete)
>
> ```rst
> DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview&force=true
> Authorization: Bearer eyJ0eXAiOiJKV1Qi….
> ```
Nerede 'eyJ0eXAiOiJKV1Qi ...' tam yetkilendirme belirteci temsil eder.

## <a name="recover-workspace"></a>Çalışma alanını kurtarma

Çalışma alanının yumuşak silme işleminden önce ilişkili olduğu abonelik ve kaynak grubuna Katkıda Bulunan İzinler varsa, verileri, yapılandırması ve bağlı aracıları da dahil olmak üzere yumuşak silme döneminde kurtarabilirsiniz. Yumuşak silme döneminden sonra, çalışma alanı kurtarılamaz ve kalıcı silme için atanır. Silinen çalışma alanlarının adları yumuşak silme döneminde korunur ve yeni bir çalışma alanı oluşturmaya çalışırken kullanılamaz.  

Aşağıdaki özellikler silinen çalışma alanı ayrıntılarıyla doldurulur sayılsa [da,](https://docs.microsoft.com/powershell/module/az.operationalinsights/New-AzOperationalInsightsWorkspace) aşağıdaki çalışma alanı oluşturma yöntemlerini kullanarak bir çalışma alanını yeniden oluşturarak kurtarabilirsiniz: PowerShell veya [REST API:]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate)

* Abonelik Kimliği
* Kaynak Grubu adı
* Çalışma alanı adı
* Bölge

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

Çalışma alanı ve tüm verileri kurtarma işleminden sonra geri getirilir. Çözümler ve bağlantılı hizmetler silindiğinde çalışma alanından kalıcı olarak kaldırıldı ve bunlar çalışma alanını daha önce yapılandırılmış durumuna getirmek için yeniden yapılandırılmalıdır. İlişkili çözümler yeniden yüklenene ve şemaları çalışma alanına eklenene kadar bazı veriler çalışma alanı kurtarmadan sonra sorgu için kullanılamayabilir.

> [!NOTE]
> * Çalışma alanı kurtarma Sıtkı [Portalı'nda](https://portal.azure.com)desteklenmez. 
> * Yumuşak silme döneminde bir çalışma alanını yeniden oluşturmak, bu çalışma alanı adının zaten kullanımda olduğunu gösterir. 
> 
