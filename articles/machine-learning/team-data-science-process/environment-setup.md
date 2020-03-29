---
title: Azure'da veri bilimi ortamları ayarlama - Ekip Veri Bilimi Süreci
description: Ekip Veri Bilimi İşlemi'nde kullanılmak üzere Azure'da veri bilimi ortamları ayarlayın.
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
ms.openlocfilehash: 6d1185e13397b3e466bd1e8d609a46f2f68bf390
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063949"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Team Data Science Process içinde kullanım için veri bilimi ortamlarını ayarlama
Ekip Veri Bilimi Süreci, verilerin depolanması, işlenmesi ve analizi için çeşitli veri bilimi ortamlarını kullanır. Bunlar arasında Azure Blob Depolama, çeşitli Azure sanal makineleri türleri, HDInsight (Hadoop) kümeleri ve Azure Machine Learning çalışma alanları yer alıyor. Hangi ortamın kullanılacağına ilişkin karar, modellenecek verinin türüne ve miktarına ve buluttaki bu verilerin hedef hedefine bağlıdır. 

* Bu kararı verirken göz önünde bulundurulması gereken sorular hakkında rehberlik için Azure [Machine Learning Data Science Environment planı'na](plan-your-environment.md)bakın. 
* Gelişmiş analitik yaparken karşılaşabileceğiniz bazı senaryoların kataloğu [için, Ekip Veri Bilimi Süreci senaryolarına](plan-sample-scenarios.md) bakın

Aşağıdaki makaleler, Team Data Science Process tarafından kullanılan çeşitli veri bilimi ortamlarını nasıl ayarlayacaklarını açıklayınız.

* [Azure depolama hesabı](../../storage/common/storage-account-create.md)
* [HDInsight (Hadoop) kümesi](customize-hadoop-cluster.md)
* [Azure Machine Learning Studio (klasik) çalışma alanı](../studio/create-workspace.md)

**Microsoft Data Science Virtual Machine (DSVM)** Azure sanal makine (VM) görüntüsü olarak da kullanılabilir. Bu VM önceden yüklenmiş ve yaygın veri analitiği ve makine öğrenimi için kullanılan birkaç popüler araçlar ile yapılandırılmıştır. DSVM hem Windows'da hem de Linux'ta kullanılabilir. Daha fazla bilgi için, [Linux ve Windows için bulut tabanlı Veri Bilimi Sanal Makineye Giriş](../data-science-virtual-machine/overview.md)'e bakın.

Nasıl oluşturulacak öğrenin:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
