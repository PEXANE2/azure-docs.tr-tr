---
title: Azure portalında bir Log Analytics çalışma alanı oluşturun | Microsoft Docs
description: Azure portal bulut ve şirket içi ortamlarınızdaki yönetim çözümlerini ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
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
ms.date: 03/12/2019
ms.author: magoedte
ms.openlocfilehash: a9775ce071b6d64d535560a567dcae078900c692
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838722"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portal Log Analytics çalışma alanı oluşturma
Azure portal kullanarak Log Analytics çalışma alanı oluşturmak için **Log Analytics çalışma alanları** menüsünü kullanın. Log Analytics çalışma alanı, Azure Izleyici günlük verileri için benzersiz bir ortamdır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolamak üzere yapılandırılır. Aşağıdaki kaynaklardan veri toplamayı planlıyorsanız bir Log Analytics çalışma alanı gerekir:

* Aboneliğinizdeki Azure kaynakları
* System Center Operations Manager tarafından izlenen şirket içi bilgisayarlar
* System Center Configuration Manager cihaz koleksiyonları 
* Azure depolama 'dan tanılama veya günlük verileri

Ortamınızdaki Azure VM 'Leri ve Windows veya Linux VM 'Leri gibi diğer kaynaklar için aşağıdaki konulara bakın:

*  [Azure sanal makinelerinden veri toplama](../learn/quick-collect-azurevm.md) 
*  [Karma Linux bilgisayarından veri topla](../learn/quick-collect-linux-computer.md)
*  [Karma Windows bilgisayarından veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portal oturum açın
Azure portal [https://portal.azure.com](https://portal.azure.com)' de oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluştur
1. Azure portal, **tüm hizmetler**' e tıklayın. Kaynak listesinde **Log Analytics**yazın. Yazmaya başladığınızda liste, girişinizi temel alarak filtreler. **Log Analytics çalışma alanlarını**seçin.

    ![Azure portal](media/quick-create-workspace/azure-portal-01.png)
  
2. **Ekle**' ye tıklayın ve ardından aşağıdaki öğeler için Seçenekler ' i seçin:

   * Yeni **Log Analytics çalışma alanı**Için *defaultlaworkspace*gibi bir ad sağlayın. Bu ad tüm Azure Izleyici abonelikleri genelinde genel olarak benzersiz olmalıdır.
   * Varsayılan seçilen değer uygun değilse, açılan listeden seçerek bağlantı için bir **abonelik** seçin.
   * **Kaynak grubu**için, zaten kurulumu veya yeni bir kaynak grubu oluşturmayı seçin.  
   * Kullanılabilir bir **konum**seçin.  Daha fazla bilgi için, [' de hangi bölgelerin kullanılabildiği Log Analytics](https://azure.microsoft.com/regions/services/) ve **bir ürün Için aramadan** Azure izleyici araması ' nı inceleyin.  
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni abonelikte bir çalışma alanı oluşturuyorsanız, bu, otomatik olarak *GB başına* fiyatlandırma planı kullanır ve fiyatlandırma katmanını seçme seçeneği kullanılamaz.  2 Nisan 'dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir Kurumsal Anlaşma (EA) kaydına bağlı olan abonelik için bir çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Belirli katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Log Analytics kaynak dikey penceresi oluşturma](media/quick-create-workspace/create-loganalytics-workspace-02.png)  

3. **Log Analytics çalışma alanı** bölmesinde gerekli bilgileri sağladıktan sonra, **Tamam**' a tıklayın.  

Bilgiler doğrulanırken ve çalışma alanı oluşturulduktan sonra, menüdeki **Bildirimler** ' in altında ilerleme durumunu izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Artık kullanılabilir bir çalışma alanınız olduğuna göre, izleme telemetri koleksiyonunu yapılandırabilir, bu verileri çözümlemek için günlük aramalarını çalıştırabilir ve ek veri ve analitik Öngörüler sağlamak için bir yönetim çözümü ekleyebilirsiniz. 

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için, bkz. [Log Analytics Kullanım Için Azure hizmet günlükleri ve ölçümleri toplama](../platform/collect-azure-metrics-logs.md).  
* Operations Manager yönetim grubunuzu raporlayan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için [veri kaynağı olarak System Center Operations Manager ekleyin](../platform/om-agents.md) . 
* Hiyerarşideki koleksiyonların üyesi olan bilgisayarları içeri aktarmak için [Configuration Manager](../platform/collect-sccm.md) bağlanın.  
* Kullanılabilir [izleme çözümlerini](../insights/solutions.md) ve çalışma alanınızdan bir çözümün nasıl ekleneceğini ve kaldırılacağını gözden geçirin.
