---
title: Azure Otomasyonu Değişiklik İzleme ve envanter dağıtım kapsamını sınırlayın
description: Bu makalede, bir Değişiklik İzleme ve envanter dağıtımının kapsamını sınırlamak için kapsam yapılandırmalarına nasıl çalışılacağı açıklanmaktadır.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 49655d11858086b16099a1864fd4d2dc5988f02a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84117440"
---
# <a name="limit-change-tracking-and-inventory-deployment-scope"></a>Değişiklik İzleme ve envanter dağıtım kapsamını sınırlandırma

Bu makalede, sanal makinelerinizdeki değişiklikleri dağıtmak üzere [değişiklik izleme ve envanter](change-tracking.md) özelliği kullanılırken kapsam yapılandırmalarının nasıl çalıştığı açıklanır. Daha fazla bilgi için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting). 

## <a name="about-scope-configurations"></a>Kapsam konfigürasyonları hakkında

Kapsam yapılandırması, Değişiklik İzleme ve envanterinin kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama (sorgu) grubudur. Kapsam yapılandırması, etkinleştirmek üzere bilgisayarları hedeflemek için Log Analytics çalışma alanında kullanılır. Özellikten değişikliklere bir bilgisayar eklediğinizde, bilgisayar çalışma alanındaki kayıtlı bir aramaya de eklenir.

## <a name="set-the-scope-limit"></a>Kapsam sınırını ayarla

Değişiklik İzleme ve envanter dağıtımınızın kapsamını sınırlandırmak için:

1. Otomasyon hesabınızda **ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. **Çalışma alanına git ' e**tıklayın.

3. **Çalışma alanı veri kaynakları**altında **kapsam yapılandırma (Önizleme)** öğesini seçin.

4. Kapsam yapılandırmasının sağ tarafındaki üç noktayı seçin `MicrosoftDefaultScopeConfig-ChangeTracking` ve **Düzenle**' ye tıklayın. 

5. Düzen bölmesinde **bilgisayar gruplarını Seç**' i seçin. Bilgisayar grupları bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir. Değişiklik İzleme ve envanter tarafından kullanılan kayıtlı arama:

    |Name     |Kategori  |Diğer ad  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  Değişiklik izleme dosyanız       | ChangeTracking__MicrosoftDefaultComputerGroup        |

6. Grubu doldurmak için kullanılan sorguyu görüntülemek ve düzenlemek için kayıtlı aramayı seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

    ![Kayıtlı aramalar](media/automation-scope-configurations-change-tracking/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

* Değişiklik İzleme ve envanterle çalışmak için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Genel özellik sorunlarını gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).