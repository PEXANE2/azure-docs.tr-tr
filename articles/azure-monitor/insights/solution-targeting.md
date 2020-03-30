---
title: Azure Monitör'de izleme çözümlerini hedefleme | Microsoft Dokümanlar
description: Hedefleme izleme çözümleri, izleme çözümlerini belirli bir aracı kümesiyle sınırlamanızı sağlar.  Bu makalede, kapsam yapılandırması oluşturmak ve bir çözüme uygulamak nasıl açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/27/2017
ms.openlocfilehash: dd3279db67fb45aee43cf1b0ef1bebf49433eef4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663241"
---
# <a name="targeting-monitoring-solutions-in-azure-monitor-preview"></a>Azure Monitör'de izleme çözümlerini hedefleme (Önizleme)
Aboneliğinize bir izleme çözümü eklediğinizde, bu çözüm varsayılan olarak Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracıları için otomatik olarak dağıtılır.  Maliyetlerinizi yönetmek ve belirli bir aracı kümesiyle sınırlayarak bir çözüm için toplanan veri miktarını sınırlamak isteyebilirsiniz.  Bu makalede, çözümlerinize kapsam uygulamanızı sağlayan bir özellik olan Çözüm Hedeflemesi'nin nasıl kullanılacağı açıklanmaktadır. **Solution Targeting**

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="how-to-target-a-solution"></a>Bir çözüm nasıl hedefilir?
Aşağıdaki bölümlerde açıklandığı gibi bir çözümü hedeflemek için üç adım vardır. 


### <a name="1-create-a-computer-group"></a>1. Bir bilgisayar grubu oluşturma
Azure Monitor'da bir [bilgisayar grubu](../platform/computer-groups.md) oluşturarak bir kapsama dahil etmek istediğiniz bilgisayarları belirtirsiniz.  Bilgisayar grubu bir günlük sorgusuna dayalı olabilir veya Active Directory veya WSUS grupları gibi diğer kaynaklardan içe aktarılabilir. [Aşağıda açıklandığı](#solutions-and-agents-that-cant-be-targeted)gibi, yalnızca Azure Monitor'a doğrudan bağlı bilgisayarlar kapsama dahil edilecektir.

Çalışma alanınızda bilgisayar grubunu oluşturduktan sonra, bir veya daha fazla çözüme uygulanabilecek bir kapsam yapılandırmasına eklersiniz.
 
 
### <a name="2-create-a-scope-configuration"></a>2. Kapsam yapılandırması oluşturma
 **Kapsam Yapılandırması** bir veya daha fazla bilgisayar grubu içerir ve bir veya daha fazla çözüme uygulanabilir. 
 
 Aşağıdaki işlemi kullanarak bir kapsam yapılandırması oluşturun.  

 1. Azure **portalında, Analytics'i günlüğe kaydedin** ve çalışma alanınızı seçin.
 2. Çalışma alanı altındaki çalışma alanı **özelliklerinde Veri Kaynakları** **Kapsam Yapılandırmaları'nı**seçin.
 3. Yeni bir kapsam yapılandırması oluşturmak için **Ekle'yi** tıklatın.
 4. Kapsam yapılandırması için bir **Ad** yazın.
 5. **Bilgisayar Gruplarını Seç'i**tıklatın.
 6. Oluşturduğunuz bilgisayar grubunu ve yapılandırmaya eklemek için isteğe bağlı olarak diğer grupları seçin.  **Seç'i**tıklatın.  
 6. Kapsam yapılandırmasını oluşturmak için **Tamam'ı** tıklatın. 


### <a name="3-apply-the-scope-configuration-to-a-solution"></a>3. Kapsam yapılandırmasını bir çözüme uygulayın.
Kapsam yapılandırmasına sahip olduktan sonra, bunu bir veya daha fazla çözüme uygulayabilirsiniz.  Tek bir kapsam yapılandırması birden çok çözümle kullanılabilse de, her çözüm yalnızca bir kapsam yapılandırması kullanabilir.

Aşağıdaki işlemi kullanarak bir kapsam yapılandırması uygulayın.  

 1. Azure **portalında, Analytics'i günlüğe kaydedin** ve çalışma alanınızı seçin.
 2. Çalışma alanı nın özelliklerinde **Solutions'ı**seçin.
 3. Kapsamına almak istediğiniz çözüme tıklayın.
 4. **Çalışma Alanı Veri Kaynakları** altında çözüm özelliklerinde Çözüm **Hedefleme**seçin.  Seçenek kullanılamıyorsa, [bu çözüm hedeflenemez.](#solutions-and-agents-that-cant-be-targeted)
 5. **Kapsam yapılandırması ekle'yi**tıklatın.  Bu çözüme zaten uygulanan bir yapılandırmanız varsa, bu seçenek kullanılamaz.  Başka bir yapılandırma eklemeden önce varolan yapılandırmayı kaldırmanız gerekir.
 6. Oluşturduğunuz kapsam yapılandırmasını tıklatın.
 7. **Başarılı**olduğunu gösterdiğinden emin olmak için yapılandırmanın **durumunu** izleyin.  Durum bir hata gösteriyorsa, yapılandırmanın sağındaki elipsi tıklatın ve değişiklik yapmak için **kapsamı edit'i** seçin.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Hedeflenilemeyecek çözümler ve aracılar
Çözüm hedeflemesi ile kullanılameyecek aracılar ve çözümler için ölçütler aşağıda verilmiştir.

- Çözüm hedeflemeyalnızca aracılara dağıtılan çözümler için geçerlidir.
- Çözüm hedefleme yalnızca Microsoft tarafından sağlanan çözümler için geçerlidir.  Kendiniz veya iş [ortakları tarafından oluşturulan](solutions-creating.md)çözümler için geçerli değildir.
- Yalnızca doğrudan Azure Monitor'a bağlanan aracılara filtre uygulayabilirsiniz.  Çözümler, kapsam yapılandırmasına dahil edilip edilmeseler de bağlı bir Operasyon Yöneticisi yönetim grubunun parçası olan aracılara otomatik olarak dağıtılır.

### <a name="exceptions"></a>Özel durumlar
Çözüm hedefleme, belirtilen kriterlere uygun olsa lar bile aşağıdaki çözümlerle kullanılamaz.

- Ajan Sağlık Değerlendirmesi

## <a name="next-steps"></a>Sonraki adımlar
- [Çalışma alanınıza Azure Log Analytics izleme çözümleri ekle'de](solutions.md)ortamınızda yüklenebilecek çözümler de dahil olmak üzere izleme çözümleri hakkında daha fazla bilgi edinin.
- [Azure Monitor günlük sorgularında Bilgisayar gruplarında](../platform/computer-groups.md)bilgisayar grupları oluşturma hakkında daha fazla bilgi edinin.
