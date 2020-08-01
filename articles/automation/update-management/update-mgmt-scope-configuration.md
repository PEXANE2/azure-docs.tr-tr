---
title: Azure Otomasyonu Güncelleştirme Yönetimi dağıtım kapsamını sınırlandırma
description: Bu makalede, Güncelleştirme Yönetimi dağıtımının kapsamını sınırlamak için kapsam yapılandırmalarının nasıl kullanılacağı açıklanır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: aafd284122ca61ba2b668186095b88003be2775c
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450375"
---
# <a name="limit-update-management-deployment-scope"></a>Güncelleştirme Yönetimi dağıtım kapsamını sınırla

Bu makalede, sanal makinelerinize güncelleştirmeler ve düzeltme ekleri dağıtmak için [güncelleştirme yönetimi](update-mgmt-overview.md) özelliği kullanılırken kapsam yapılandırmalarının nasıl çalıştığı açıklanır. Daha fazla bilgi için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](../../azure-monitor/insights/solution-targeting.md).

## <a name="about-scope-configurations"></a>Kapsam konfigürasyonları hakkında

Kapsam yapılandırması, Güncelleştirme Yönetimi kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama (sorgu) grubudur. Kapsam yapılandırması, etkinleştirmek üzere bilgisayarları hedeflemek için Log Analytics çalışma alanında kullanılır. Güncelleştirme Yönetimi güncelleştirmeleri almak üzere bir bilgisayar eklediğinizde, bilgisayar çalışma alanındaki kayıtlı bir aramaya de eklenir.

## <a name="set-the-scope-limit"></a>Kapsam sınırını ayarla

Güncelleştirme Yönetimi dağıtımınızın kapsamını sınırlandırmak için:

1. Otomasyon hesabınızda **ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. **Çalışma alanına git**' i seçin.

3. **Çalışma alanı veri kaynakları**altında **kapsam yapılandırma (Önizleme)** öğesini seçin.

4. Kapsam yapılandırmasının sağ tarafındaki üç noktayı seçin `MicrosoftDefaultScopeConfig-Updates` ve **Düzenle**' yi seçin.

5. Düzen bölmesinde **bilgisayar grupları seç**' i genişletin. Bilgisayar grupları bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir. Güncelleştirme Yönetimi tarafından kullanılan kayıtlı arama:

    |Name     |Kategori  |Diğer ad  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

6. Grubu doldurmak için kullanılan sorguyu görüntülemek ve düzenlemek için kayıtlı aramayı seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

    [![Kayıtlı aramalar](./media/update-mgmt-scope-configuration/logsearch.png)](./media/update-mgmt-scope-configuration/logsearch-expanded.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirme değerlendirmelerini, dağıtımları ve diğer ilgili yönetim görevlerini çözümlemek için [Azure izleyici günlüklerini sorgulayabilirsiniz](update-mgmt-query-logs.md) . Başlamanıza yardımcı olmak için önceden tanımlanmış sorgular içerir.
