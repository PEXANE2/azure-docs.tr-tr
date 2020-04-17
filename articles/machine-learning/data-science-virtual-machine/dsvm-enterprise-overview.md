---
title: Ekip analitiği ve AI ortamı
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi VM'yi kurumsal ekip ortamında dağıtma desenleri.
keywords: derin öğrenme, Yapay Bilgi, veri bilimi araçları, veri bilimi sanal makine, jeouzamsal analitik, ekip veri bilimi süreci
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 05/08/2018
ms.openlocfilehash: 1f3a24afb2238c2448cb19b3889467a1b1819724
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460554"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Data Science Sanal Makine tabanlı ekip analizi ve Yapay AI ortamı 
[Data Science Virtual Machine](overview.md) (DSVM), yapay zeka (AI) ve veri analitiği için önceden oluşturulmuş yazılımlarla Azure platformunda zengin bir ortam sağlar.

Geleneksel olarak, DSVM bireysel bir analitik masaüstü olarak kullanılmıştır. Bireysel veri bilimciler bu paylaşılan, önceden oluşturulmuş analiz ortamıyla üretkenlik kazanırlar. Büyük analiz ekipleri veri bilimcileri ve AI geliştiricileri için ortamlar planlarken, yinelenen temalardan biri geliştirme ve deneme için paylaşılan bir analiz altyapısıdır. Bu altyapı, veri bilimi ve analiz ekipleri arasında işbirliğini ve tutarlılığı da kolaylaştıran kurumsal BT ilkeleri doğrultusunda yönetilir.

Paylaşılan bir altyapı, analitik ortamın BT kullanımını daha iyi sağlar. Bazı kuruluşlar, ekip tabanlı veri bilimi/analitik altyapısını *analitik sandbox*olarak adlandırıyor. Verileri hızla anlamak için veri bilimcilerin çeşitli veri varlıklarına erişmesini sağlar. Bu sanal ortam aynı zamanda veri bilimcilerin deney yürütmelerine, hipotezleri doğrulamalarına ve üretim ortamını etkilemeden tahmine dayalı modeller oluşturmalarına yardımcı olur.

DSVM Azure altyapı düzeyinde çalıştığından, BT yöneticileri DSVM'yi işletmenin BT ilkelerine uygun olarak çalışacak şekilde kolayca yapılandırabilir. DSVM, çeşitli paylaşım mimarilerini uygulamada tam esneklik sunarken, aynı zamanda kurumsal veri varlıklarına kontrollü bir şekilde erişim sağlar.

Bu bölümde, DSVM'yi ekip tabanlı veri bilimi altyapısı olarak dağıtmak için kullanabileceğiniz bazı desenler ve yönergeler açıklanabilir. Bu desenlerin yapı taşları hizmet olarak Azure altyapısından (IaaS) geldiğinden, tüm Azure VM'leri için geçerlidir. Bu makale serisi, bu standart Azure altyapı özelliklerini DSVM'ye uygulamaya odaklanır.

Kurumsal ekip analizi ortamının temel yapı taşları şunlardır:

* [DSVM'lerin otomatik olarak ölçeklendirildik havuzu](dsvm-pools.md)
* [Havuzdaki DSVM'lerden herhangi birinden ortak kimlik ve çalışma alanına erişim](dsvm-common-identity.md)
* [Veri kaynaklarına güvenli erişim](dsvm-secure-access-keys.md)


Bu seri, önceki konuların her biri için kılavuz ve işaretçiler sağlar. Büyük kurumsal yapılandırmalarda DSVM'leri dağıtmak için gereken tüm hususları ve gereksinimleri kapsamaz. İşletmenizde DSVM örneklerini uygularken kullanabileceğiniz diğer bazı Azure kaynakları şunlardır:

* [Ağ güvenliği](https://docs.microsoft.com/azure/security/fundamentals/network-security)
* [İzleme](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) ve [yönetim](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Günlük kaydı ve denetim](https://docs.microsoft.com/azure/security/fundamentals/log-audit)
* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [İlke ayarı ve uygulama](../../governance/policy/overview.md)
* [Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
* [Şifreleme](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption-overview.md)
* [Veri bulma ve yönetişim](https://docs.microsoft.com/azure/data-catalog/)

Son [olarak, Azure Mimari Merkezi,](https://docs.microsoft.com/azure/architecture/) bulut tabanlı analiz altyapınızı oluşturmak ve yönetmek için ayrıntılı bir uçten uca mimari ve modeller sağlar.