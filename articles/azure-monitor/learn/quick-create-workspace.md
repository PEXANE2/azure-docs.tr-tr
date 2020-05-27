---
title: Azure portalında bir Log Analytics çalışma alanı oluşturun | Microsoft Docs
description: Azure portal bulut ve şirket içi ortamlarınızdaki yönetim çözümlerini ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/26/2020
ms.openlocfilehash: e6ef8fb7dfd98a509b0b89e049c70169dafbe134
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860571"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portal Log Analytics çalışma alanı oluşturma
Azure portal kullanarak Log Analytics çalışma alanı oluşturmak için **Log Analytics çalışma alanları** menüsünü kullanın. Log Analytics çalışma alanı, Azure Izleyici günlük verileri için benzersiz bir ortamdır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolamak üzere yapılandırılır. Aşağıdaki kaynaklardan veri toplamayı planlıyorsanız bir Log Analytics çalışma alanı gerekir:

* Aboneliğinizdeki Azure kaynakları
* System Center Operations Manager tarafından izlenen şirket içi bilgisayarlar
* Configuration Manager cihaz koleksiyonları 
* Azure depolama 'dan tanılama veya günlük verileri

Ortamınızdaki Azure VM 'Leri ve Windows veya Linux VM 'Leri gibi diğer kaynaklar için aşağıdaki konulara bakın:

*  [Azure sanal makinelerinden veri toplama](../learn/quick-collect-azurevm.md) 
*  [Karma Linux bilgisayarından veri topla](../learn/quick-collect-linux-computer.md)
*  [Karma Windows bilgisayarından veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını**seçin.

    ![Azure portal](media/quick-create-workspace/azure-portal-01.png)
  
2. **Ekle**' ye tıklayın ve ardından aşağıdaki öğeler için Seçenekler ' i seçin:

   * Yeni **Log Analytics çalışma alanı**Için *defaultlaworkspace*gibi bir ad sağlayın. Bu ad tüm Azure Izleyici abonelikleri genelinde genel olarak benzersiz olmalıdır.
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak grubu**için, zaten kurulumu veya yeni bir kaynak grubu oluşturmayı seçin.  
   * Kullanılabilir bir **konum**seçin.  Daha fazla bilgi için, [' de hangi bölgelerin kullanılabildiği Log Analytics](https://azure.microsoft.com/regions/services/) ve **bir ürün Için aramadan** Azure izleyici araması ' nı inceleyin.  
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan 'dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir Kurumsal Anlaşma (EA) kaydına bağlı olan abonelik için bir çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Belirli katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Log Analytics kaynak dikey penceresi oluşturma](media/quick-create-workspace/create-loganalytics-workspace-02.png)  

3. **Log Analytics Çalışma Alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="troubleshooting"></a>Sorun giderme
Son 14 gün içinde silinen bir çalışma alanı oluşturduğunuzda ve [geçici silme durumunda](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#soft-delete-behavior)işlem, çalışma alanı yapılandırmanıza bağlı olarak farklı bir sonuca sahip olabilir:
1. Silinen çalışma alanında aynı çalışma alanı adı, kaynak grubu, abonelik ve bölge sağlarsanız, çalışma alanınız veri, yapılandırma ve bağlı aracılar dahil kurtarılacak.
2. Aynı çalışma alanı adını, ancak farklı kaynak grubunu, aboneliği veya bölgeyi kullanırsanız, *Bu çalışma alanı adının zaten kullanımda olduğunu bir hata alırsınız. Lütfen başka bir tane deneyin*. Geçici silme işlemini geçersiz kılmak ve çalışma alanınızı kalıcı olarak silmek ve aynı ada sahip yeni bir çalışma alanı oluşturmak için, önce çalışma alanını kurtarmak ve kalıcı silme gerçekleştirmek için şu adımları izleyin:
   - Çalışma alanınızı [kurtarın](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#recover-workspace)
   - Çalışma alanınızı [kalıcı olarak silme](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#permanent-workspace-delete)
   - Aynı çalışma alanı adını kullanarak yeni bir çalışma alanı oluştur

## <a name="next-steps"></a>Sonraki adımlar
Artık kullanılabilir bir çalışma alanınız olduğuna göre, izleme telemetri koleksiyonunu yapılandırabilir, bu verileri çözümlemek için günlük aramalarını çalıştırabilir ve ek veri ve analitik Öngörüler sağlamak için bir yönetim çözümü ekleyebilirsiniz. 

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için, bkz. [Log Analytics Kullanım Için Azure hizmet günlükleri ve ölçümleri toplama](../platform/collect-azure-metrics-logs.md).  
* Operations Manager yönetim grubunuzu raporlayan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için [veri kaynağı olarak System Center Operations Manager ekleyin](../platform/om-agents.md) . 
* Hiyerarşideki koleksiyonların üyesi olan bilgisayarları içeri aktarmak için [Configuration Manager](../platform/collect-sccm.md) bağlanın.  
* Kullanılabilir [izleme çözümlerini](../insights/solutions.md) ve çalışma alanınızdan bir çözümün nasıl ekleneceğini ve kaldırılacağını gözden geçirin.
