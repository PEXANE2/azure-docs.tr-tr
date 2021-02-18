---
title: Azure Log Analytics çalışma alanını silme ve kurtarma | Microsoft Docs
description: Kişisel abonelikte bir tane oluşturduysanız veya çalışma alanı modelinizi yeniden yapılandırmak için Log Analytics çalışma alanınızı silmeyi öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/20/2020
ms.openlocfilehash: 5bb072d0ea710e35b4f741836aed143e962a326e
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100622256"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Azure Log Analytics çalışma alanını silme ve kurtarma

Bu makalede, Azure Log Analytics çalışma alanı geçici silme kavramı ve silinen çalışma alanının nasıl kurtarılacağı açıklanmaktadır.

## <a name="considerations-when-deleting-a-workspace"></a>Çalışma alanı silinirken dikkat edilecek noktalar

Bir Log Analytics çalışma alanını sildiğinizde, silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu, veri ve bağlı aracılarını 14 gün içinde içeren çalışma alanının kurtarılmasına izin veren bir geçici silme işlemi yapılır. Geçici silme süresinden sonra, çalışma alanı kaynağı ve verileri kurtarılamaz ve 30 gün içinde tamamen temizlenmek üzere sıraya alınır. Çalışma alanı adı ' yayımlandı ' ve bunu yeni bir çalışma alanı oluşturmak için kullanabilirsiniz.

> [!NOTE]
> Geçici silme davranışını geçersiz kılmak ve çalışma alanınızı kalıcı olarak silmek istiyorsanız, [kalıcı çalışma alanı silme](#permanent-workspace-delete)bölümündeki adımları uygulayın.

Hizmet işleminizi olumsuz yönde etkileyebilecek önemli veriler ve yapılandırma olabileceğinden, bir çalışma alanını sildiğinizde dikkatli olmak istersiniz. Hangi aracıların, çözümlerin ve diğer Azure hizmetlerinin verilerini Log Analytics veri depolamalarını gözden geçirin, örneğin:

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

Bir çalışma alanını [PowerShell](/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace), [REST API](/rest/api/loganalytics/workspaces/delete)veya [Azure Portal](https://portal.azure.com)kullanarak silebilirsiniz.

### <a name="azure-portal"></a>Azure portalı

1. [Azure portalında](https://portal.azure.com) oturum açın. 
2. Azure portal, **tüm hizmetler**' i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını** seçin.
3. Log Analytics çalışma alanları listesinde bir çalışma alanı seçin ve orta bölmenin en üstündeki **Sil**  ' e tıklayın.
4. Çalışma alanına geçen hafta boyunca veri alımını gösteren bir onay sayfası görüntülenir. 
5. Daha sonra kurtarma seçeneğini kaldırarak çalışma alanını kalıcı olarak silmek istiyorsanız, **çalışma alanını kalıcı olarak sil** onay kutusunu seçin.
6. Onaylamak için çalışma alanının adını yazın ve ardından **Sil**' e tıklayın.

   ![Çalışma alanının Silinmesini Onayla](media/delete-workspace/workspace-delete.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

## <a name="permanent-workspace-delete"></a>Kalıcı çalışma alanı silme
Geçici silme yöntemi, aynı ayarlar ve çalışma alanı adıyla bir dağıtımı tekrarlamanız gereken geliştirme ve test gibi bazı senaryolara uygun olmayabilir. Bu gibi durumlarda, çalışma alanınızı kalıcı olarak silebilir ve geçici silme dönemini "geçersiz kılabilirsiniz". Kalıcı çalışma alanı silme işlemi çalışma alanı adını serbest bırakır ve aynı adı kullanarak yeni bir çalışma alanı oluşturabilirsiniz.

> [!IMPORTANT]
> Kalıcı çalışma alanı silme işlemini, geri döndürülemez bu yana dikkatli bir şekilde kullanın, çalışma alanınızı ve verilerini kurtaramayacağız.

Azure portal kullanarak bir çalışma alanını kalıcı olarak silmek için, **Sil** düğmesine tıklamadan önce **çalışma alanını kalıcı olarak sil** onay kutusunu seçin.

PowerShell kullanarak bir çalışma alanını kalıcı olarak silmek için, çalışma alanınızı kalıcı olarak silmek üzere '-ForceDelete ' etiketini ekleyin. '-ForceDelete ' seçeneği şu anda az. Operationalınsights 2.3.0 veya üzeri ile kullanılabilir. 

```powershell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name" -ForceDelete
```

## <a name="recover-workspace"></a>Çalışma alanını kurtar
Bir Log Analytics çalışma alanını yanlışlıkla veya isteyerek sildiğinizde, hizmet, çalışma alanını herhangi bir işlem için erişilebilir hale getiren bir geçici silme durumuna koyar. Silinen çalışma alanının adı, geçici silme dönemi sırasında korunur ve yeni bir çalışma alanı oluşturmak için kullanılamaz. Geçici silme süresinden sonra, çalışma alanı kurtarılamaz, kalıcı silme için zamanlanır ve yeni bir çalışma alanı oluşturmak için kullanılabilir.

Veri, yapılandırma ve bağlı aracılar dahil olmak üzere geçici silme döneminde çalışma alanınızı kurtarabilirsiniz. Çalışma alanının geçici silme işleminden önce bulunduğu abonelik ve kaynak grubu için katkıda bulunan izinlerine sahip olmanız gerekir. Çalışma alanı kurtarma işlemi, Log Analytics çalışma alanını, silinen çalışma alanının ayrıntılarıyla birlikte yeniden oluşturularak gerçekleştirilir:

- Abonelik Kimliği
- Kaynak grubu adı
- Çalışma alanı adı
- Region

> [!IMPORTANT]
> Çalışma alanınız kaynak grubu silme işleminin bir parçası olarak silinmişse, önce kaynak grubunu yeniden oluşturmanız gerekir.

### <a name="azure-portal"></a>Azure portalı

1. [Azure portalında](https://portal.azure.com) oturum açın. 
2. Azure portal, **tüm hizmetler**' i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını** seçin. Seçili kapsamda bulunan çalışma alanlarının listesini görürsünüz.
3. En üstteki menüden **kurtar** ' a tıklayarak geçici bir şekilde kurtarılabilen, geçici silme durumunda bulunan çalışma alanları içeren bir sayfa açın.

   ![Menü çubuğunda kurtarma vurgulanmış şekilde Azure portal Log Analytics çalışma alanları ekranının ekran görüntüsü.](media/delete-workspace/recover-menu.png)

4. Çalışma alanını seçin ve bu çalışma alanını kurtarmak için **kurtar** ' a tıklayın.

   ![Bir çalışma alanı vurgulanmış ve kurtar düğmesi seçili olan Azure portal içindeki silinen Log Analytics çalışma alanlarını kurtar iletişim kutusunun ekran görüntüsü.](media/delete-workspace/recover-workspace.png)


### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

Çalışma alanı ve tüm verileri kurtarma işleminden sonra geri getirilir. Çözüm ve bağlı hizmetler silindiği zaman çalışma alanından kalıcı olarak kaldırılmıştır ve çalışma alanını daha önce yapılandırılmış durumuna getirmek için yeniden yapılandırılması gerekir. İlişkili çözümler yeniden yüklenene ve şemaları çalışma alanına eklenene kadar, bazı veriler, çalışma alanı kurtarmasından sonra sorgu için kullanılamayabilir.

## <a name="troubleshooting"></a>Sorun giderme

Bir çalışma alanını silmek için en az *Log Analytics katkıda bulunan* izinlerinizin olması gerekir.

* Silinen çalışma alanının geçici silme durumunda olduğundan ve kurtarılabileceği emin değilseniz, abonelik başına geçici olarak silinen çalışma alanlarının listesini görmek için *Log Analytics çalışma alanları* sayfasında [geri dönüşüm kutusunu aç](#recover-workspace) ' a tıklayın. Kalıcı olarak silinen çalışma alanları listeye dahil edilmez.
* Bir hata iletisi alırsanız, *Bu çalışma alanı adı zaten kullanımda* veya bir çalışma alanı oluştururken *çakışıyor* olabilir:
  * Çalışma alanı adı, kuruluşunuzdaki birisi veya diğer müşteri tarafından kullanılabilir değil.
  * Çalışma alanı son 14 gün içinde silindi ve bu ad, geçici silme dönemi için ayrılmış olarak tutuldu. Geçici silme işlemini geçersiz kılmak ve aynı ada sahip yeni bir çalışma alanı oluşturmak için çalışma alanınızı kalıcı olarak silmek üzere, önce çalışma alanını kurtarmak için aşağıdaki adımları izleyin ve sonra kalıcı silme gerçekleştirin:<br>
    1. Çalışma alanınızı [kurtarın](#recover-workspace) .
    2. Çalışma alanınızı [kalıcı olarak silin](#permanent-workspace-delete) .
    3. Aynı çalışma alanı adını kullanarak yeni bir çalışma alanı oluşturun.
 
      Silme çağrısı arka uçta başarıyla tamamlandıktan sonra, çalışma alanını geri yükleyebilir ve daha önce önerilen yöntemlerden birindeki kalıcı silme işlemini tamamlayabilirsiniz.

* Bir çalışma alanı silinirken *kaynak bulunamadı* 204 yanıt kodu alırsanız, ardışık yeniden deneme işlemleri meydana gelebilir. 204 boş bir yanıt olduğundan, genellikle kaynağın bulunmadığı anlamına gelir; bu nedenle silme işlemi hiçbir şey yapılmadan tamamlanır.
* Kaynak grubunuzu ve çalışma alanınızı silerseniz, [geri dönüşüm kutusu 'Nu aç](#recover-workspace) sayfasında silinen çalışma alanını görebilirsiniz. ancak, kaynak grubu mevcut olmadığından kurtarma işlemi 404 hata koduyla başarısız olur; kaynak grubunuzu yeniden oluşturun ve kurtarmayı yeniden deneyin.
