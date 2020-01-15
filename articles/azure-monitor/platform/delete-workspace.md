---
title: Azure Log Analytics çalışma alanını silme ve kurtarma | Microsoft Docs
description: Kişisel abonelikte bir tane oluşturduysanız veya çalışma alanı modelinizi yeniden yapılandırmak için Log Analytics çalışma alanınızı silmeyi öğrenin.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/14/2020
ms.openlocfilehash: 739f97e912a33402aa7482e59dd78f5aeb005772
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75944435"
---
# <a name="delete-and-restore-azure-log-analytics-workspace"></a>Azure Log Analytics çalışma alanını silme ve geri yükleme

Bu makalede, Azure Log Analytics çalışma alanı geçici silme kavramı ve silinen çalışma alanının nasıl kurtarılacağı açıklanmaktadır. 

## <a name="considerations-when-deleting-a-workspace"></a>Çalışma alanı silinirken dikkat edilecek noktalar

Bir Log Analytics çalışma alanını sildiğinizde, silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu, veri ve bağlı aracılarını 14 gün içinde içeren çalışma alanının kurtarılmasına izin veren bir geçici silme işlemi yapılır. Geçici silme süresinden sonra, çalışma alanı kaynağı ve verileri kurtarılamaz – verileri kalıcı olarak silinmek üzere kuyruğa alınır ve 30 gün içinde tamamen temizlenir. Çalışma alanı adı ' yayımlandı ' ve bunu yeni bir çalışma alanı oluşturmak için kullanabilirsiniz.

> [!NOTE]
> Geçici silme davranışı kapatılamaz. Silme işleminde bir ' zorla ' etiketi kullanılırken geçici silme işlemini geçersiz kılmak için kısa bir seçenek ekleyeceğiz.

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

### <a name="azure-portal"></a>Azure Portal

1. Oturum açmak için [Azure Portal](https://portal.azure.com)gidin. 
2. Azure portalda **Tüm hizmetler**’i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını**seçin.
3. Log Analytics çalışma alanları listesinde bir çalışma alanı seçin ve orta bölmenin en üstündeki **Sil** ' e tıklayın.
   ![seçim alanı Özellikler bölmesinden](media/delete-workspace/log-analytics-delete-workspace.png)
4. Çalışma alanını silme işlemini onaylamanızı isteyen onay iletisi penceresi göründüğünde **Evet**' e tıklayın.
   ![çalışma alanının silinmesini onaylayın](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

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
