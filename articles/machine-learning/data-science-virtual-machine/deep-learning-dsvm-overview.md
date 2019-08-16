---
title: Derin öğrenme sanal makinesine giriş-Azure | Microsoft Docs
description: Derin öğrenme sanal makineleri için anahtar analizi senaryoları ve bileşenleri.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi araçları, Linux veri bilimi
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/16/2018
ms.author: vijetaj
ms.openlocfilehash: 8541713219eb8daf880605089fe49fc585836bdc
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534943"
---
# <a name="introduction-to-the-deep-learning-virtual-machine"></a>Derin öğrenme sanal makinesine giriş

> [!NOTE]
> Tüm derin öğrenme VM (DLVM) araçları artık [veri bilimi VM'si (DSVM)](dsvm-tools-overview.md)olarak katlanmıştır. DLCı VM ve DSVM artık aynı. Lütfen DLVM yerine bir DSVM oluşturmayı düşünün.

## <a name="why-deep-learning-virtual-machine"></a>Derin öğrenme sanal makinesi neden? 

Gittikçe artan, derin öğrenme algoritmaları/derin sinir ağları birçok makine öğrenimi sorununa yönelik popüler yöntemlerin biri haline geliyor. Özellikle, görüntü, metin, ses/video anlama gibi makine bilişsel görevler, gelişmiş derin sinir ağ mimarilerine sahip bazı belirli etki alanlarında insan bilişsel düzeylerine yaklaştığı ve modelleri eğitemek için büyük veri kümesine erişim sağlar. Derin öğrenme, bu büyük veri kümeleriyle modelleri eğitmek için büyük miktarda hesaplama gücü gerektirir. Grafik Işleme birimlerinin (GPU 'Lar) bulutunu ve kullanılabilirliğiyle, karmaşık kapsamlı sinir mimarilerin oluşturulması ve bunları buluttaki güçlü bilgi işlem altyapısında büyük bir veri kümesinde eğmek mümkün oluyor.  [Veri bilimi sanal makinesi](overview.md) , veri hazırlığı, makine öğrenimi ve derin öğrenme için zengin bir araç ve örnek kümesi sağlamıştır. Ancak kullanıcıların karşılaştığı güçlüklerden biri, derinlemesine öğrenme ve ayrıca GPU tabanlı sanal makine örneklerini daha kolay bir şekilde sağlamak gibi belirli senaryolar için araç ve örnekleri bulabilmektedir. Bu derin öğrenme sanal makinesi (DLVM) bu zorlukları ele alır. 

## <a name="what-is-deep-learning-virtual-machine"></a>Derin öğrenme sanal makinesi nedir? 
Derin öğrenme sanal makinesi, derinlemesine öğrenme modellerini eğitmek için GPU tabanlı VM örneklerinin kullanımını daha basit hale getirmek için [veri bilimi sanal makinesi](overview.md) (dsvm) özel olarak yapılandırılmış bir varyantıdır. Windows 2016 ve Ubuntu Veri Bilimi Sanal Makinesi desteklenir.  Aynı çekirdek VM görüntülerini (ve dolayısıyla tüm zengin araç takımını) DSVM olarak paylaşır, ancak derin öğrenimi daha kolay hale getirmek için yapılandırılır. Ayrıca, birçok gerçek yaşam AI senaryosu için büyük ölçüde geçerli olan görüntü ve metin anlama için uçtan uca örnekler sunuyoruz. Derin öğrenme sanal makinesi Ayrıca, sanal makinedeki araçların ve örneklerin kataloğunu ortaya çıkararak DSVM 'de zengin araç ve örnek kümesini daha kolay bulunabilir hale getirmek için de çalışır. Derin öğrenme sanal makinesi, araçların yanı sıra görüntü ve metin verileri elde etmek için birçok popüler derin öğrenme çerçevesi sağlar. Araçların kapsamlı bir listesi için [veri bilimi sanal makinesi genel bakış sayfasına](overview.md#whats-included-in-the-data-science-vm)başvurabilirsiniz. 

## <a name="next-steps"></a>Sonraki Adımlar

Derin öğrenme sanal makinesini aşağıdaki adımlarla kullanmaya başlayın:

* [Windows Veri Bilimi Sanal Makinesi sağlama](provision-vm.md)
* [Ubuntu Veri Bilimi Sanal Makinesi sağlama](dsvm-ubuntu-intro.md)

