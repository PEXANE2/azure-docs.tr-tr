---
title: Verileri Azure - Team Data Science Process bilimi ortamlarını ayarlama
description: Veri bilimi ortamlarını Team Data Science Process içinde kullanım için Azure üzerinde ayarlayın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d11e1614307d5b6cb267ae72bae6f32adb4b244a
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722229"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Team Data Science Process içinde kullanım için veri bilimi ortamlarını ayarlama
Team Data Science Process, depolama, işleme ve veri analizi için çeşitli veri bilimi ortamlarını kullanır. Bunlar, Azure Blob Depolama, birden fazla Azure sanal makineleri, HDInsight (Hadoop) kümeleri ve Azure Machine Learning çalışma alanlarını içerir. Kullanmak için hangi ortamı modellenmesi için türü ve veri miktarını bağlıdır ve buluttaki veriler için hedef hakkında karar. 

* Bu kararı verirken göz önünde bulundurmanız gereken sorular hakkında yönergeler için bkz. [Azure Machine Learning veri bilimi ortamınızı planlayın](plan-your-environment.md). 
* Gelişmiş analizler yaparken karşılaşabileceğiniz bazı senaryolardan bir katalog için bkz [. Team Data Science Işlemi senaryoları](plan-sample-scenarios.md)

Aşağıdaki makaleler Team Data Science Process tarafından kullanılan çeşitli veri bilimi ortamlarını ayarlama açıklanır.

* [Azure depolama hesabı](../../storage/common/storage-account-create.md)
* [HDInsight (Hadoop) kümesi](customize-hadoop-cluster.md)
* [Azure Machine Learning Studio (klasik) çalışma alanı](../studio/create-workspace.md)

**Microsoft veri bilimi sanal makinesi (DSVM)** Ayrıca bir Azure sanal MAKINESI (VM) görüntüsü olarak da kullanılabilir. Bu VM önceden yüklenmiş ve yapılandırılmış veri analizi ve makine öğrenimi için yaygın olarak kullanılan çeşitli popüler araçlarla. DSVM, hem Windows hem de Linux üzerinde kullanılabilir. Daha fazla bilgi için bkz. [Linux ve Windows için bulut tabanlı veri bilimi sanal makinesi giriş](../data-science-virtual-machine/overview.md).

Oluşturmayı öğrenin:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
