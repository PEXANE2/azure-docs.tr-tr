---
title: Avere vFXT küme ayarı - Azure
description: Azure için Avere vFXT'de performansı optimize etmek için özel ayarlara genel bakış
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: df20f050ff87fdb59a3e5cca373098240f8bfbb9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76152944"
---
# <a name="cluster-tuning"></a>Küme ayarlama

Çoğu vFXT kümeleri özelleştirilmiş performans ayarlarından yararlanabilir. Bu ayarlar kümenin belirli iş akışınız, veri kümeniz ve araçlarınızla en iyi şekilde çalışmasına yardımcı olur.

Avere Denetim Masası'ndan bulunmayan özellikleri yapılandırmayı içerebileceğinden, bu özelleştirme bir destek temsilcisinin yardımıyla yapılmalıdır.

Bu bölümde, yapılabilecek bazı özel aparatlar açıklanmaktadır.

## <a name="general-optimizations"></a>Genel optimizasyonlar

Bu değişiklikler, veri kümesi niteliklerine veya iş akışı stiline bağlı olarak önerilebilir.

* İş yükü yazma ağırlıklıysa, yazma önbelleğinin boyutunu varsayılan %20'den artırın.
* Veri kümesi çok sayıda küçük dosya içeriyorsa, küme önbelleğinin dosya sayısı sınırını artırın.
* Çalışma, verileri kopyalamayı veya iki depo arasında taşımayı içeriyorsa, verileri taşımak için kullanılan iş parçacığı sayısını ayarlayın:
  * Hızı artırmak için, kullanılan paralel iş parçacığı sayısını artırabilirsiniz.
  * Arka uç depolama hacmi aşırı yükleniyorsa, kullanılan paralel iş parçacığı sayısını azaltmanız gerekebilir.
* Küme, NFSv4 ALIP'leri kullanan bir çekirdek filer için veri önbelleğe alıyorsa, belirli istemciler için dosya yetkilendirmesini kolaylaştırmak için erişim modunu önbelleğe alın.

## <a name="cloud-nas-or-cloud-gateway-optimizations"></a>Bulut NAS veya bulut ağ geçidi optimizasyonları

Bulut NAS veya ağ geçidi senaryosunda, vFXT kümesi bir bulut kapsayıcısına NAS tarzı erişim sağlar. vFXT kümesi ve bulut depolama arasındaki yüksek veri hızlarından yararlanmak için temsilciniz, önbellekten verileri depolama birimine daha agresif bir şekilde itmek için ayarları değiştirmenizi önerebilir. Örnek:

* Küme ve depolama kapsayıcısı arasındaki TCP bağlantı sayısını artırma

## <a name="cloud-bursting-or-hybrid-wan-optimizations"></a>Bulut patlaması veya hibrit WAN optimizasyonları

Bulut patlaması senaryosunda veya karma depolama WAN optimizasyonu senaryosunda, vFXT kümesi bulut ve şirket içi donanım depolaması arasında tümleştirme sağlar. Bu değişiklikler yararlı olabilir:

* Küme ve çekirdek filer arasında izin verilen TCP bağlantı sayısını artırma
* Uzak çekirdek filer için WAN Optimizasyona ayarını etkinleştirin (Bu ayar, farklı bir Azure bölgesinde uzak bir şirket içi filer veya bulut çekirdek filer için kullanılabilir.)
* TCP soketi arabellek boyutunu artırma<sup>*</sup>
* Gereksiz önbelleğe alınan dosyaları azaltmak için "her zaman iletme" ayarını etkinleştirme<sup>*</sup>

<sup>*</sup>Bu ayarlamalar, iş yükü ve performans gereksinimlerine bağlı olarak tüm sistemler için geçerli olmayabilir.

## <a name="help-optimizing-your-avere-vfxt-for-azure"></a>Azure için Avere vFXT'nizi optimize etmenize yardımcı olun

Bu optimizasyonlar hakkında destek personeliyle iletişim kurmak için [sisteminizden yardım alın'da](avere-vfxt-open-ticket.md)açıklanan yordamı kullanın.
