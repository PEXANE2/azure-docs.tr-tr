---
title: Azure için Avere vFXT hakkında ek bağlantılar
description: Azure için Avere vFXT hakkında ek bilgilere bağlantılar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 78e0500057baf407f1d26afe29d9df1e525cbf3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153777"
---
# <a name="additional-documentation"></a>Diğer belgeler

Bu makalede, Avere Denetim Masası yönetim arayüzü ve ilgili konular hakkında ek belgelere bağlantılar vardır.

## <a name="avere-cluster-documentation"></a>Avere küme belgeleri

Ek Avere küme belgeleri web sitesinde <https://azure.github.io/Avere/>bulunabilir. Bu belgeler, kümenin yeteneklerini ve ayarlarını nasıl yapılandırabileceğinizi anlamanıza yardımcı olabilir.

* [FXT Küme Oluşturma Kılavuzu](<https://azure.github.io/Avere/#fxt_cluster>) fiziksel donanım düğümlerinden oluşan kümeler için tasarlanmıştır, ancak belgedeki bazı bilgiler vFXT kümeleri için de geçerlidir. Özellikle, yeni vFXT küme yöneticileri bu bölümleri okuma yararlanabilir:
  * [Destek ve İzleme Ayarlarını özelleştirmek,](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_support.html#config-support>) destek yükleme ayarlarını nasıl özelleştirişleri ve uzaktan izlemeyi nasıl etkinleştireceklerini açıklar.
  * [VServers ve Global Namespace yapılandırma](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_vserver.html#config-vserver>) istemciye bakan bir ad alanı oluşturma hakkında bilgi vardır.
  * [Avere kümesi için DNS'yi yapılandırmak,](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_network.html#dns-overview>) round-robin DNS'nin nasıl yapılandırılabildiğini açıklar.
  * [Arka uç Depolama](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) belgeleri ekleme nasıl çekirdek filers eklemek için.

* [Küme Yapılandırma Kılavuzu,](<https://azure.github.io/Avere/#operations>) bir Avere kümesi için ayarların ve seçeneklerin tam bir başvurusudur. VFXT kümesi bu seçeneklerin bir alt kümesini kullanır, ancak aynı yapılandırma sayfalarının çoğu geçerlidir.

* [Pano Kılavuzu,](<https://azure.github.io/Avere/#operations>) Avere Denetim Masası'nın küme izleme özelliklerinin nasıl kullanılacağını açıklar.

## <a name="vfxt-creation-and-management-documentation"></a>vFXT oluşturma ve yönetim dokümantasyonu

Komut dosyası tabanlı bulut kümesi oluşturma ve yönetim yardımcı programı olan vfxt.py kullanmak için tam bir kılavuz GitHub: [vfxt.py ile bulut küme yönetimi](https://github.com/Azure/AvereSDK/blob/master/docs/README.md)nde sağlanır.
