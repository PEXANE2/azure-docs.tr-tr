---
title: Azure Otomasyonu Güncelleştirme Yönetimi kapsam yapılandırmalarına göre çalışma
description: Bu makalede, Güncelleştirme Yönetimi kullanırken kapsam yapılandırmalarına nasıl çalışacağınızı söyliyoruz.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 23ec49f2d68cf376ef0beb118d8bf69ada7bc0de
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83832036"
---
# <a name="work-with-scope-configurations-for-update-management"></a>Güncelleştirme Yönetimi için kapsam yapılandırmasıyla çalışma

Bu makalede, VM 'lerde [güncelleştirme yönetimi](automation-update-management.md) özelliğini kullanırken kapsam yapılandırmalarına nasıl çalışabileceğinizi açıklanmaktadır. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Kapsam yapılandırmasını denetleme

Güncelleştirme Yönetimi, özelliği etkinleştirmek üzere bilgisayarları hedeflemek için Log Analytics çalışma alanındaki kapsam yapılandırmasını kullanır. Kapsam yapılandırması, özelliğin kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Kapsam yapılandırmalarına erişmek için:

1. Otomasyon hesabınızda **ilgili kaynaklar**altında **çalışma alanı**' nı seçin. 

2. **Çalışma alanı veri kaynakları**altında çalışma alanını seçin ve **kapsam yapılandırması**' nı seçin.

3. Seçilen çalışma alanında Güncelleştirme Yönetimi özelliği henüz etkinleştirilmemişse, `MicrosoftDefaultScopeConfig-Updates` kapsam yapılandırması oluşturulur. 

4. Seçilen çalışma alanı özelliği zaten etkinse, yeniden dağıtılır ve kapsam yapılandırması buna eklenmez. 

5. Kapsam yapılandırmalarının herhangi birinde üç noktayı seçin ve ardından **Düzenle**' ye tıklayın. 

6. Düzen bölmesinde **bilgisayar gruplarını Seç**' i seçin. Bilgisayar grupları bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir.

## <a name="view-a-saved-search"></a>Kayıtlı bir aramayı görüntüleme

Bir bilgisayar Güncelleştirme Yönetimi eklendiğinde, çalışma alanınızda kayıtlı bir aramaya de eklenir. Kayıtlı arama, hedeflenen bilgisayarları içeren bir sorgudur.

1. Log Analytics çalışma alanınıza gidin ve **genel**altında **kaydedilmiş aramalar** ' ı seçin. Güncelleştirme Yönetimi tarafından kullanılan kayıtlı arama:

|Name     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

2. Grubu doldurmak için kullanılan sorguyu görüntülemek için kayıtlı aramayı seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

    ![Kayıtlı aramalar](media/automation-scope-configurations-update-management/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

* Özelliği ile çalışmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
* Özellik hatalarını gidermek için bkz. [sorun giderme güncelleştirme yönetimi sorunları](troubleshoot/update-management.md).
* Windows Update Aracısı hatalarını gidermek için bkz. [Windows Update Aracısı sorunlarını giderme](troubleshoot/update-agent-issues.md).
* Linux Güncelleştirme Aracısı hatalarını gidermek için bkz. [Linux Güncelleştirme Aracısı sorunlarını giderme](troubleshoot/update-agent-issues-linux.md).