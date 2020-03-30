---
title: Azure Portalı'nda Günlük Analizi çalışma alanı oluşturun | Microsoft Dokümanlar
description: Azure portalındaki bulut ve şirket içi ortamlarınızdan yönetim çözümleri ve veri toplamayı etkinleştirmek için Bir Log Analytics çalışma alanı oluşturmayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2019
ms.openlocfilehash: 3c2e9d5634916c3713b7e3380c0496611d8f60a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77656288"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portalında Günlük Analizi çalışma alanı oluşturma
Azure portalını kullanarak Bir Günlük Analizi çalışma alanı oluşturmak için **Günlük Analizi çalışma alanları** menüsünü kullanın. Log Analytics çalışma alanı, Azure Monitor günlük verileri için benzersiz bir ortamdır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolayabilmek üzere yapılandırılır. Aşağıdaki kaynaklardan veri toplamak istiyorsanız bir Log Analytics çalışma alanına ihtiyacınız vardır:

* Aboneliğinizdeki Azure kaynakları
* Sistem Merkezi Operasyon Yöneticisi tarafından izlenen şirket içi bilgisayarlar
* Configuration Manager'dan aygıt koleksiyonları 
* Azure depolamadan veri tanılama veya günlük

Ortamınızdaki Azure VM'leri ve Windows veya Linux VM'leri gibi diğer kaynaklar için aşağıdaki konulara bakın:

*  [Azure sanal makinelerinden veri toplama](../learn/quick-collect-azurevm.md) 
*  [Karma Linux bilgisayarından veri toplama](../learn/quick-collect-linux-computer.md)
*  [Karma Windows bilgisayarından veri toplama](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.

    ![Azure portalında](media/quick-create-workspace/azure-portal-01.png)
  
2. **Ekle'yi**tıklatın ve ardından aşağıdaki öğeler için seçenekleri seçin:

   * *Varsayılan LAWorkspace*gibi yeni **Log Analytics çalışma alanı**için bir ad sağlayın. Bu ad, tüm Azure Monitor aboneliklerinde genel olarak benzersiz olmalıdır.
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak Grubu**için, varolan bir kaynak grubunu zaten kurulum da kullanmayı veya yeni bir tane oluşturmayı seçin.  
   * Kullanılabilir bir **Konum**seçin.  Daha fazla bilgi için [Log Analytics'in](https://azure.microsoft.com/regions/services/) hangi bölgelerde kullanılabildiği ne bakın ve **ürün arama** alanından Azure Monitörünü arayın.  
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan'dan önce oluşturulmuş bir abonelik için veya varolan bir Kurumsal Sözleşme (EA) kaydına bağlı abonelik için bir çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Belirli katmanlar hakkında daha fazla bilgi için [Log Analytics Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/log-analytics/)bakın.

        ![Log Analytics kaynak bıçağı oluşturun](media/quick-create-workspace/create-loganalytics-workspace-02.png)  

3. **Log Analytics Çalışma Alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Artık kullanılabilir bir çalışma alanınız olduğuna göre, izleme telemetrisi koleksiyonunu yapılandırabilir, bu verileri çözümlemek için günlük aramaları çalıştırabilir ve ek veriler ve analitik öngörüler sağlamak için bir yönetim çözümü ekleyebilirsiniz. 

* Azure Tanılama veya Azure depolama alanıyla Azure kaynaklarından veri toplamayı etkinleştirmek [için, Log Analytics'te kullanılmak üzere Azure hizmet günlüklerini ve ölçümlerini topla'ya](../platform/collect-azure-metrics-logs.md)bakın.  
* Operasyon Yöneticisi yönetim grubunuzu bildiren aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için [Sistem Merkezi Operasyon Yöneticisi'ni veri kaynağı olarak ekleyin.](../platform/om-agents.md) 
* [Configuration Manager'ı](../platform/collect-sccm.md) hiyerarşideki koleksiyonların üyesi olan bilgisayarları içe aktarmaya bağlayın.  
* Kullanılabilir [izleme çözümlerini](../insights/solutions.md) ve çalışma alanınızdan bir çözümü nasıl ekleyeceğiniz veya kaldırılanızı gözden geçirin.
