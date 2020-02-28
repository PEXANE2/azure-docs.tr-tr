---
title: Azure portalında Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure portalında, Bulut ve şirket içi ortamlarından yönetim çözümleri ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2019
ms.openlocfilehash: 3c2e9d5634916c3713b7e3380c0496611d8f60a6
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77656288"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portalında Log Analytics çalışma alanı oluşturma
Azure portal kullanarak Log Analytics çalışma alanı oluşturmak için **Log Analytics çalışma alanları** menüsünü kullanın. Log Analytics çalışma alanı, Azure Izleyici günlük verileri için benzersiz bir ortamdır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolamak üzere yapılandırılır. Aşağıdaki kaynaklardan veri toplamayı planlıyorsanız bir Log Analytics çalışma alanı gerekir:

* Aboneliğinizdeki Azure kaynakları
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen
* Configuration Manager cihaz koleksiyonları 
* Azure depolama biriminden Tanılama veya günlük verileri

Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

*  [Azure sanal makinelerinden veri toplama](../learn/quick-collect-azurevm.md) 
*  [Karma Linux bilgisayarından veri topla](../learn/quick-collect-linux-computer.md)
*  [Karma Windows bilgisayarından veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics çalışma alanlarını**seçin.

    ![Azure portalı](media/quick-create-workspace/azure-portal-01.png)
  
2. **Ekle**' ye tıklayın ve ardından aşağıdaki öğeler için Seçenekler ' i seçin:

   * Yeni **Log Analytics çalışma alanı** için *DefaultLAWorkspace* gibi bir ad sağlayın. Bu ad tüm Azure Izleyici abonelikleri genelinde genel olarak benzersiz olmalıdır.
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak grubu**için, zaten kurulumu veya yeni bir kaynak grubu oluşturmayı seçin.  
   * Kullanılabilir bir **konum**seçin.  Daha fazla bilgi için, [' de hangi bölgelerin kullanılabildiği Log Analytics](https://azure.microsoft.com/regions/services/) ve **bir ürün Için aramadan** Azure izleyici araması ' nı inceleyin.  
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  Mevcut bir Kurumsal Anlaşma (EA) kaydına bağlı aboneliğe veya 2 Nisan'dan önce oluşturulmuş mevcut bir abonelik için bir çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Belirli katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Log Analytics kaynak dikey penceresi oluşturma](media/quick-create-workspace/create-loganalytics-workspace-02.png)  

3. **Log Analytics Çalışma Alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin. 

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için, bkz. [Log Analytics Kullanım Için Azure hizmet günlükleri ve ölçümleri toplama](../platform/collect-azure-metrics-logs.md).  
* Operations Manager yönetim grubunuzu raporlayan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için [veri kaynağı olarak System Center Operations Manager ekleyin](../platform/om-agents.md) . 
* Hiyerarşideki koleksiyonların üyesi olan bilgisayarları içeri aktarmak için [Configuration Manager](../platform/collect-sccm.md) bağlanın.  
* Kullanılabilir [izleme çözümlerini](../insights/solutions.md) ve çalışma alanınızdan bir çözümün nasıl ekleneceğini ve kaldırılacağını gözden geçirin.
