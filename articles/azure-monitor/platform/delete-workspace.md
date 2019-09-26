---
title: Azure Log Analytics çalışma alanını silme ve geri yükleme | Microsoft Docs
description: Kişisel abonelikte bir tane oluşturduysanız veya çalışma alanı modelinizi yeniden yapılandırmak için Log Analytics çalışma alanınızı silmeyi öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.openlocfilehash: f8dcab1a7a46d518b752e48f9886b60a37d8ec4c
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299535"
---
# <a name="delete-and-restore-azure-log-analytics-workspace"></a>Azure Log Analytics çalışma alanını silme ve geri yükleme
Bu makalede, Azure Log Analytics çalışma alanı geçici silme kavramı ve silinen çalışma alanının nasıl kurtarılacağı açıklanmaktadır. 

## <a name="considerations-when-deleting-a-workspace"></a>Çalışma alanı silinirken dikkat edilecek noktalar
Bir Log Analytics çalışma alanını sildiğinizde, silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu, veri ve bağlı aracılarını 14 gün içinde içeren çalışma alanının kurtarılmasına izin veren bir geçici silme işlemi yapılır. Geçici silme süresinden sonra, çalışma alanı ve verileri kurtarılamaz ve 30 gün içinde kalıcı olarak silinmek üzere sıraya alınır.

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
> Otomasyon hesabı gibi yüklü çözümler ve bağlı hizmetler, silme sırasında çalışma alanından kalıcı olarak kaldırılır ve kurtarılamaz. Çalışma alanını önceki işlevselliğine getirmek için kurtarma işleminden sonra bunların yeniden yapılandırılması gerekir. 

[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete)veya [Azure Portal](https://portal.azure.com)kullanarak bir çalışma alanını silebilirsiniz.

### <a name="delete-workspace-in-azure-portal"></a>Azure portal çalışma alanını sil
1. Oturum açmak için [Azure Portal](https://portal.azure.com)gidin. 
2. Azure portalda **Tüm hizmetler**’i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını**seçin.
3. Log Analytics çalışma alanları listesinde bir çalışma alanı seçin ve orta bölmenin en üstündeki **Sil** ' e tıklayın.
   ![Çalışma alanı Özellikler bölmesinden silme seçeneği](media/delete-workspace/log-analytics-delete-workspace.png)
4. Çalışma alanını silme işlemini onaylamanızı isteyen onay iletisi penceresi göründüğünde **Evet**' e tıklayın.
   ![Çalışma alanının Silinmesini Onayla](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

## <a name="recover-workspace"></a>Çalışma alanını kurtar
Çalışma alanının geçici silme işleminden önce ilişkilendirildiği abonelik ve kaynak grubu için katkıda bulunan izinleriniz varsa, verileri, yapılandırması ve bağlı aracıları da dahil olmak üzere geçici silme döneminde kurtarabilirsiniz. Geçici silme süresinden sonra, çalışma alanı kurtarılamaz ve kalıcı silme için atanmaz.

Desteklenen oluşturma yöntemlerinden herhangi birini kullanarak çalışma alanını yeniden oluşturarak kurtarabilirsiniz: PowerShell, Azure CLı veya Azure portal, bu özellikler silinen çalışma alanının ayrıntılarıyla doldurulduğu sürece aşağıdakiler de dahil olmak üzere:
1.  Abonelik Kimliği
2.  Kaynak grubu adı
3.  Çalışma alanı adı
4.  Bölge

> [!NOTE]
> Silinen çalışma alanlarının adları, geçici silme döneminde korunur ve yeni bir çalışma alanı oluşturulurken kullanılamaz. Çalışma alanı adları *serbest* bırakılır ve geçici silme döneminin süresi dolduktan sonra yeni çalışma alanı oluşturmak için kullanılabilir.
