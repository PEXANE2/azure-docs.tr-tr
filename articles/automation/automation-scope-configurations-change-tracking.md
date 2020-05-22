---
title: Azure Otomasyonu Değişiklik İzleme ve envanteri için kapsam yapılandırmasıyla çalışma
description: Bu makalede, Değişiklik İzleme ve envanterini kullanırken kapsam yapılandırmalarına nasıl çalışacağınızı söyliyoruz.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: c30bd8a3bb4fa1085e56dd93c66c016c3612e352
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749140"
---
# <a name="work-with-scope-configurations-for-change-tracking-and-inventory"></a>Değişiklik İzleme ve envanter için kapsam yapılandırmalarına sahip çalışma

Bu makalede, VM 'lerde [güncelleştirme yönetimi](automation-update-management.md) özelliğini etkinleştirirken kapsam yapılandırmalarına nasıl çalışabileceğinizi açıklanmaktadır. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Kapsam yapılandırmasını denetleme

Güncelleştirme Yönetimi, Güncelleştirme Yönetimi etkinleştirmek üzere bilgisayarları hedeflemek için Log Analytics çalışma alanındaki kapsam yapılandırmasını kullanır. Kapsam yapılandırması, özelliğin kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Kapsam yapılandırmalarına erişmek için:

1. Otomasyon hesabınızda **ilgili kaynaklar**altında **çalışma alanı**' nı seçin. 

2. **Çalışma alanı veri kaynakları**altında çalışma alanını seçin ve **kapsam yapılandırması**' nı seçin.

3. Seçilen çalışma alanında Güncelleştirme Yönetimi özelliği henüz etkinleştirilmemişse, `MicrosoftDefaultScopeConfig-ChangeTracking` kapsam yapılandırması oluşturulur. 

4. Seçilen çalışma alanı özelliği zaten etkinse, yeniden dağıtılır ve kapsam yapılandırması buna eklenmez. 

5. Kapsam yapılandırmalarının herhangi birinde üç noktayı seçin ve ardından **Düzenle**' ye tıklayın. 

6. Düzen bölmesinde **bilgisayar gruplarını Seç**' i seçin. Bilgisayar grupları bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir.

## <a name="view-a-saved-search"></a>Kayıtlı bir aramayı görüntüleme

Bir bilgisayar Değişiklik İzleme ve envantere eklendiğinde, çalışma alanınızdaki kayıtlı bir aramaya de eklenir. Kayıtlı arama, hedeflenen bilgisayarları içeren bir sorgudur.

1. Log Analytics çalışma alanınıza gidin ve **genel**altında **kaydedilmiş aramalar** ' ı seçin. Güncelleştirme Yönetimi tarafından kullanılan kayıtlı arama:

    |Name     |Kategori  |Diğer ad  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  Değişiklik izleme dosyanız       | ChangeTracking__MicrosoftDefaultComputerGroup        |

2. Grubu doldurmak için kullanılan sorguyu görüntülemek için kayıtlı aramayı seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

    ![Kayıtlı aramalar](media/automation-scope-configurations-change-tracking/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği hakkında genel bilgi için bkz. [değişiklik izleme ve envantere genel bakış](change-tracking.md).
* Özelliği ile çalışmak için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Özelliği etkinleştirmek üzere bir Otomasyon hesabı kullanmak için bkz. [Otomasyon hesabından değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-auto-acct.md).
* Özelliği etkinleştirmek üzere Azure portal kullanmak için, bkz. [Azure portal değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-browse.md).
* Özelliği etkinleştirmek üzere bir runbook kullanmak için bkz. [runbook 'tan değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-runbook.md).
* Özelliği etkinleştirmek üzere bir Azure VM kullanmak için bkz. [Azure VM 'den değişiklik izleme ve envanteri etkinleştirme](automation-enable-changes-from-vm.md).
* Bir VM 'deki değişiklik izleme ve stokla ilgili sorunları gidermek için bkz. [Azure VM 'de değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md).
* Özellik hatalarını gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).