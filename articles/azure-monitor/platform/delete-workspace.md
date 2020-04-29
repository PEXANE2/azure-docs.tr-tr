---
title: Azure Log Analytics çalışma alanını silme ve kurtarma | Microsoft Docs
description: Kişisel abonelikte bir tane oluşturduysanız veya çalışma alanı modelinizi yeniden yapılandırmak için Log Analytics çalışma alanınızı silmeyi öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/14/2020
ms.openlocfilehash: 1dceb3db4572ecdaf504745dba1099a5eccead43
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80395780"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Azure Log Analytics çalışma alanını silme ve kurtarma

Bu makalede, Azure Log Analytics çalışma alanı geçici silme kavramı ve silinen çalışma alanının nasıl kurtarılacağı açıklanmaktadır. 

## <a name="considerations-when-deleting-a-workspace"></a>Çalışma alanı silinirken dikkat edilecek noktalar

Bir Log Analytics çalışma alanını sildiğinizde, silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu, veri ve bağlı aracılarını 14 gün içinde içeren çalışma alanının kurtarılmasına izin veren bir geçici silme işlemi yapılır. Geçici silme süresinden sonra, çalışma alanı kaynağı ve verileri kurtarılamaz – verileri kalıcı olarak silinmek üzere kuyruğa alınır ve 30 gün içinde tamamen temizlenir. Çalışma alanı adı ' yayımlandı ' ve bunu yeni bir çalışma alanı oluşturmak için kullanabilirsiniz.

> [!NOTE]
> Geçici silme davranışını geçersiz kılmak ve çalışma alanınızı kalıcı olarak silmek istiyorsanız, [kalıcı çalışma alanı silme](#permanent-workspace-delete)bölümündeki adımları izleyin.

Hizmet işleminizi olumsuz yönde etkileyebilecek önemli veriler ve yapılandırma olabileceğinden, bir çalışma alanını sildiğinizde dikkatli olmak istersiniz. Şu gibi Log Analytics verilerini depolayan aracıları, çözümleri ve diğer Azure hizmetlerini ve kaynakları inceleyin:

* Yönetim çözümleri
* Azure Otomasyonu
* Windows ve Linux sanal makinelerinde çalışan aracılar
* Ortamınızdaki Windows ve Linux bilgisayarları üzerinde çalışan aracılar
* System Center Operations Manager

Geçici silme işlemi çalışma alanı kaynağını siler ve ilişkili kullanıcıların izni bozulur. Kullanıcılar başka çalışma alanlarıyla ilişkiliyse, bu diğer çalışma alanlarıyla Log Analytics kullanmaya devam edebilirler.

## <a name="soft-delete-behavior"></a>Geçici silme davranışı

Çalışma alanı silme işlemi, çalışma alanı Kaynak Yöneticisi kaynağını kaldırır, ancak yapılandırma ve veriler 14 gün boyunca tutulur ve çalışma alanının silindiği görünümü verir. Çalışma alanına rapor verecek şekilde yapılandırılan tüm aracılar ve System Center Operations Manager yönetim grupları, geçici silme dönemi boyunca yalnız bırakılmış durumda kalır. Bu hizmet, silinen çalışma alanının verileri ve bağlı kaynakları dahil olmak üzere, silme işlemini geri alarak kurtarmak için bir mekanizma sağlar.

> [!NOTE] 
> Azure Otomasyonu hesabınız gibi yüklü çözümler ve bağlı hizmetler, silme sırasında çalışma alanından kalıcı olarak kaldırılır ve kurtarılamaz. Çalışma alanını önceden yapılandırılmış durumuna getirmek için kurtarma işleminden sonra bunların yeniden yapılandırılması gerekir.

Bir çalışma alanını [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [REST API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete)veya [Azure Portal](https://portal.azure.com)kullanarak silebilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. Oturum açmak için [Azure Portal](https://portal.azure.com)gidin. 
2. Azure portal, **tüm hizmetler**' i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını**seçin.
3. Log Analytics çalışma alanları listesinde bir çalışma alanı seçin ve orta bölmenin en üstündeki **Sil** ' e tıklayın.
   ![Çalışma alanı Özellikler bölmesinden silme seçeneği](media/delete-workspace/log-analytics-delete-workspace.png)
4. Çalışma alanını silme işlemini onaylamanızı isteyen onay iletisi penceresi göründüğünde **Evet**' e tıklayın.
   ![Çalışma alanının Silinmesini Onayla](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

### <a name="troubleshooting"></a>Sorun giderme

Log Analytics çalışma alanını silmek için ' Log Analytics katkıda bulunan ' izinlerine sahip olmanız gerekir.<br>
Bir hata iletisi alırsanız '*Bu çalışma alanı adı zaten kullanımda*' bir çalışma alanı oluştururken şu şekilde olabilir:
* Çalışma alanı adı, kuruluşunuzdaki birisi veya diğer müşteri tarafından kullanılabilir değil.
* Çalışma alanı son 14 gün içinde silindi ve bu ad, geçici silme dönemi için ayrılmış olarak tutuldu. Geçici silme işlemini geçersiz kılmak ve çalışma alanınızı hemen silmek ve aynı ada sahip yeni bir çalışma alanı oluşturmak için, önce çalışma alanını kurtarmak ve kalıcı silme gerçekleştirmek için şu adımları izleyin:<br>
   1. Çalışma alanınızı [kurtarın](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#recover-workspace) .
   2. Çalışma alanınızı [kalıcı olarak silin](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#permanent-workspace-delete) .
   3. Aynı çalışma alanı adını kullanarak yeni bir çalışma alanı oluşturun.


## <a name="permanent-workspace-delete"></a>Kalıcı çalışma alanı silme
Geçici silme yöntemi, aynı ayarlar ve çalışma alanı adıyla bir dağıtımı tekrarlamanız gereken geliştirme ve test gibi bazı senaryolara uygun olmayabilir. Bu gibi durumlarda, çalışma alanınızı kalıcı olarak silebilir ve geçici silme dönemini "geçersiz kılabilirsiniz". Kalıcı çalışma alanı silme işlemi çalışma alanı adını serbest bırakır ve aynı adı kullanarak yeni bir çalışma alanı oluşturabilirsiniz.


> [!IMPORTANT]
> Kalıcı çalışma alanı silme işlemini, geri döndürülemez bu yana dikkatli bir şekilde kullanın, çalışma alanınızı ve verilerini kurtaramayacağız.

Kalıcı çalışma alanı silme, şu anda REST API aracılığıyla gerçekleştirilebilir.

> [!NOTE]
> Herhangi bir API isteği, istek üst bilgisinde bir taşıyıcı yetkilendirme belirteci içermelidir.
>
> Belirteci şunları kullanarak edinebilirsiniz:
> - [Uygulama kayıtları](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens)
> - Tarayıcıda geliştirici konsolunu (F12) kullanarak Azure portal gidin. **Batch** , **istek üstbilgileri**altındaki kimlik doğrulama dizesinin örneklerinden birine bakın. Bu, model *yetkilendirmesi: taşıyıcı <token> *içinde olacaktır. Örnekte gösterildiği gibi bunu kopyalayın ve API çağra ekleyin.
> - Azure REST belgeleri sitesine gidin. herhangi bir API üzerinde **deneyin** , taşıyıcı belirtecini KOPYALAYıN ve API çağra ekleyin.
Çalışma alanınızı kalıcı olarak silmek için, [çalışma alanları-Rest]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) API çağrısını bir zorlama etiketiyle birlikte kullanın:
>
> ```rst
> DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview&force=true
> Authorization: Bearer eyJ0eXAiOiJKV1Qi….
> ```
WHERE ' eyJ0eXAiOiJKV1Qi... ' tam yetkilendirme belirtecini temsil eder.

## <a name="recover-workspace"></a>Çalışma alanını kurtar

Çalışma alanının geçici silme işleminden önce ilişkilendirildiği abonelik ve kaynak grubu için katkıda bulunan izinleriniz varsa, verileri, yapılandırması ve bağlı aracıları da dahil olmak üzere geçici silme döneminde kurtarabilirsiniz. Geçici silme süresinden sonra, çalışma alanı kurtarılamaz ve kalıcı silme için atanmaz. Silinen çalışma alanlarının adları, geçici silme dönemi sırasında korunur ve yeni bir çalışma alanı oluşturulmaya çalışılırken kullanılamaz.  

Aşağıdaki özellikler silinen çalışma alanı ayrıntılarıyla doldurulduğu sürece, bir çalışma alanını yeniden oluşturarak kurtarabilirsiniz: [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/New-AzOperationalInsightsWorkspace) veya [REST API]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate) .

* Abonelik Kimliği
* Kaynak grubu adı
* Çalışma alanı adı
* Bölge

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

Çalışma alanı ve tüm verileri kurtarma işleminden sonra geri getirilir. Çözüm ve bağlı hizmetler silindiği zaman çalışma alanından kalıcı olarak kaldırılmıştır ve çalışma alanını daha önce yapılandırılmış durumuna getirmek için yeniden yapılandırılması gerekir. İlişkili çözümler yeniden yüklenene ve şemaları çalışma alanına eklenene kadar, bazı veriler, çalışma alanı kurtarmasından sonra sorgu için kullanılamayabilir.

> [!NOTE]
> * [Azure Portal](https://portal.azure.com)çalışma alanı kurtarma desteklenmiyor. 
> * Geçici silme dönemi sırasında çalışma alanının yeniden oluşturulması, bu çalışma alanı adının zaten kullanımda olduğunu belirten bir bildirim sağlar. 
> 
