---
title: Azure Otomasyonu Güncelleştirme Yönetimi dağıtım kapsamını sınırlandırma
description: Bu makalede, Güncelleştirme Yönetimi dağıtımının kapsamını sınırlamak için kapsam yapılandırmalarının nasıl kullanılacağı açıklanır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 8770762fa2d2ae6bc0584d75397829298a62e8c0
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185900"
---
# <a name="limit-update-management-deployment-scope"></a>Güncelleştirme Yönetimi dağıtım kapsamını sınırla

Bu makalede, sanal makinelerinize güncelleştirmeler ve düzeltme ekleri dağıtmak için [güncelleştirme yönetimi](automation-update-management.md) özelliği kullanılırken kapsam yapılandırmalarının nasıl çalıştığı açıklanır. Daha fazla bilgi için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](../azure-monitor/insights/solution-targeting.md). 

## <a name="about-scope-configurations"></a>Kapsam konfigürasyonları hakkında

Kapsam yapılandırması, Güncelleştirme Yönetimi kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama (sorgu) grubudur. Kapsam yapılandırması, etkinleştirmek üzere bilgisayarları hedeflemek için Log Analytics çalışma alanında kullanılır. Güncelleştirme Yönetimi güncelleştirmeleri almak üzere bir bilgisayar eklediğinizde, bilgisayar çalışma alanındaki kayıtlı bir aramaya de eklenir.

## <a name="set-the-scope-limit"></a>Kapsam sınırını ayarla

Güncelleştirme Yönetimi dağıtımınızın kapsamını sınırlandırmak için:

1. Otomasyon hesabınızda **ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. **Çalışma alanına git ' e**tıklayın.

3. **Çalışma alanı veri kaynakları**altında **kapsam yapılandırma (Önizleme)** öğesini seçin.

4. Kapsam yapılandırmasının sağ tarafındaki üç noktayı seçin `MicrosoftDefaultScopeConfig-Updates` ve **Düzenle**' ye tıklayın. 

5. Düzen bölmesinde **bilgisayar grupları seç**' i genişletin. Bilgisayar grupları bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir. Güncelleştirme Yönetimi tarafından kullanılan kayıtlı arama:

    |Ad     |Category  |Diğer ad  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

6. Grubu doldurmak için kullanılan sorguyu görüntülemek ve düzenlemek için kayıtlı aramayı seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

    ![Kayıtlı aramalar](media/automation-scope-configurations-update-management/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Özellik hatalarını gidermek için bkz. [sorun giderme güncelleştirme yönetimi sorunları](troubleshoot/update-management.md).
* Windows Update Aracısı hatalarını gidermek için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux Güncelleştirme Aracısı hatalarını gidermek için bkz. [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).
