---
title: Azure Otomasyonu Güncelleştirme Yönetimi dağıtım kapsamını sınırlandırma
description: Bu makalede, Güncelleştirme Yönetimi dağıtımının kapsamını sınırlamak için kapsam yapılandırmalarının nasıl kullanılacağı açıklanır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 72065b388f348da1d268f875a10d5b13d2f8cf3b
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117394"
---
# <a name="limit-update-management-deployment-scope"></a>Güncelleştirme Yönetimi dağıtım kapsamını sınırla

Bu makalede, sanal makinelerinize güncelleştirmeler ve düzeltme ekleri dağıtmak için [güncelleştirme yönetimi](automation-update-management.md) özelliği kullanılırken kapsam yapılandırmalarının nasıl çalıştığı açıklanır. Daha fazla bilgi için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting). 

## <a name="about-scope-configurations"></a>Kapsam konfigürasyonları hakkında

Kapsam yapılandırması, Güncelleştirme Yönetimi kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama (sorgu) grubudur. Kapsam yapılandırması, etkinleştirmek üzere bilgisayarları hedeflemek için Log Analytics çalışma alanında kullanılır. Güncelleştirme Yönetimi güncelleştirmeleri almak üzere bir bilgisayar eklediğinizde, bilgisayar çalışma alanındaki kayıtlı bir aramaya de eklenir.

## <a name="set-the-scope-limit"></a>Kapsam sınırını ayarla

Güncelleştirme Yönetimi dağıtımınızın kapsamını sınırlandırmak için:

1. Otomasyon hesabınızda **ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. **Çalışma alanına git ' e**tıklayın.

3. **Çalışma alanı veri kaynakları**altında **kapsam yapılandırma (Önizleme)** öğesini seçin.

4. Kapsam yapılandırmasının sağ tarafındaki üç noktayı seçin `MicrosoftDefaultScopeConfig-Updates` ve **Düzenle**' ye tıklayın. 

5. Düzen bölmesinde **bilgisayar grupları seç**' i genişletin. Bilgisayar grupları bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir. Güncelleştirme Yönetimi tarafından kullanılan kayıtlı arama:

    |Name     |Kategori  |Diğer ad  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

6. Grubu doldurmak için kullanılan sorguyu görüntülemek ve düzenlemek için kayıtlı aramayı seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

    ![Kayıtlı aramalar](media/automation-scope-configurations-update-management/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Özellik hatalarını gidermek için bkz. [sorun giderme güncelleştirme yönetimi sorunları](troubleshoot/update-management.md).
* Windows Update Aracısı hatalarını gidermek için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux Güncelleştirme Aracısı hatalarını gidermek için bkz. [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).