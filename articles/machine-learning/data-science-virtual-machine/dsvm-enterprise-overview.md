---
title: Team Analytics ve AI ortamı
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi VM'si kurumsal bir ekip ortamında dağıtmaya yönelik desenler.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz, ekip veri bilimi işlemi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 05/08/2018
ms.openlocfilehash: 03fdbf6979db3249d1322a3025b48de81b953ae1
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82856221"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Veri Bilimi Sanal Makinesi tabanlı ekip Analizi ve AI ortamı 
[Veri bilimi sanal makinesi](overview.md) (dsvm), yapay zeka (AI) ve veri analizi için önceden oluşturulmuş yazılımlarla Azure platformunda zengin bir ortam sağlar.

Geleneksel olarak, DSVM tek bir analiz masaüstü olarak kullanılmıştır. Bireysel veri bilimcileri, bu paylaşılan, önceden oluşturulmuş analiz ortamıyla üretkenlik elde edebilir. Büyük analiz takımları, veri bilimcilerinin ve AI geliştiricilerinin ortamlarını planlarken, yinelenen temalardan biri, geliştirme ve deneme için paylaşılan bir analiz altyapısıdır. Bu altyapı, kurumsal BT ilkeleriyle birlikte yönetilir ve bu da veri bilimi ve analiz ekipleri genelinde işbirliğini ve tutarlılığı kolaylaştırır.

Paylaşılan bir altyapı, analiz ortamının daha iyi BT kullanımını mümkün kılar. Bazı kuruluşlar ekip tabanlı veri bilimi/analiz altyapısını bir *analiz korumalı alanı*olarak çağırır. Veri bilimcilerinin verileri hızla anlamak için çeşitli veri varlıklarına erişmesine olanak sağlar. Bu korumalı alan ortamı Ayrıca, veri bilimcilerinin üretim ortamını etkilemeden denemeleri çalıştırmasına, hipotezleri doğrulamasına ve tahmine dayalı modeller oluşturmasına yardımcı olur.

DSVM, Azure altyapı düzeyinde çalıştığından, BT yöneticileri DSVM 'yi kuruluşun BT ilkeleriyle uyumlu olarak çalışacak şekilde kolayca yapılandırabilir. DSVM, çeşitli paylaşım mimarları uygulamak için, aynı zamanda şirket veri varlıklarına denetimli bir şekilde erişim sağlarken tam esneklik sunar.

Bu bölümde, DSVM 'yi takım tabanlı bir veri bilimi altyapısı olarak dağıtmak için kullanabileceğiniz bazı desenler ve yönergeler açıklanmaktadır. Bu desenlerin yapı taşları Azure hizmet olarak altyapı (IaaS) ' den geldiği için, tüm Azure sanal makineleri için geçerlidir. Bu makale serisi, bu standart Azure altyapı özelliklerini DSVM 'ye uygulamaya odaklanır.

Kurumsal ekip Analizi ortamının anahtar yapı taşları şunları içerir:

* [DSVMs tarafından boyutlandırılmış bir havuz](dsvm-pools.md)
* [Havuzdaki DSVMs 'lerden herhangi birinden bir çalışma alanına ortak kimlik ve erişim](dsvm-common-identity.md)
* [Veri kaynaklarına güvenli erişim](dsvm-secure-access-keys.md)


Bu seri, yukarıdaki konuların her biri için rehberlik ve işaretçiler sağlar. Büyük ölçekli kurumsal yapılandırmalarda DSVMs dağıtmaya yönelik tüm konuları ve gereksinimleri kapsamaz. Kuruluşunuzda DSVM örnekleri uygulanırken kullanabileceğiniz bazı diğer Azure kaynakları aşağıda verilmiştir:

* [Ağ güvenliği](https://docs.microsoft.com/azure/security/fundamentals/network-security)
* [İzleme](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) ve [Yönetim](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Günlük kaydı ve denetim](https://docs.microsoft.com/azure/security/fundamentals/log-audit)
* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [İlke ayarı ve zorlaması](../../governance/policy/overview.md)
* [Korunma](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
* [Şifreleme](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption-overview)
* [Veri bulma ve idare](https://docs.microsoft.com/azure/data-catalog/)

Son olarak [Azure mimari merkezi](https://docs.microsoft.com/azure/architecture/) , bulut tabanlı analiz altyapınızı oluşturmak ve yönetmek için ayrıntılı bir uçtan uca mimari ve modeller sağlar.