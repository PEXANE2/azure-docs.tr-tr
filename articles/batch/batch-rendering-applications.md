---
title: Uygulamaları oluşturma - Azure Toplu İş
description: Azure Toplu İşi ile herhangi bir render uygulamasını kullanmak mümkündür. Ancak, Azure Marketi VM görüntüleri önceden yüklenmiş yaygın uygulamalarla kullanılabilir.
services: batch
ms.service: batch
author: LauraBrenner
ms.author: labrenne
ms.date: 09/19/2019
ms.topic: conceptual
ms.openlocfilehash: 77672534b2aad993a44e9b637fbed58df8610e97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022996"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>VM görüntülerinin işlenmesi için önceden yüklenmiş uygulamalar

Azure Toplu İşi ile herhangi bir render uygulamasını kullanmak mümkündür. Ancak, Azure Marketi VM görüntüleri önceden yüklenmiş yaygın uygulamalarla kullanılabilir.

Uygun olduğu durumlarda, önceden yüklenmiş işleme uygulamaları için kullanım başına ödeme lisansı kullanılabilir. Toplu İşlem havuzu oluşturulduğunda, gerekli uygulamalar belirtilebilir ve hem VM hem de uygulamalar dakika başına faturalandırılır. Uygulama fiyatları Azure [Toplu Toplu fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering)listelenir.

Bazı uygulamalar yalnızca Windows'u destekler, ancak çoğu hem Windows'da hem de Linux'ta desteklenir.

## <a name="applications-on-centos-7-rendering-images"></a>CentOS 7 render görüntüleri uygulamaları

Aşağıdaki liste CentOS 7.6, sürüm 1.1.6 render görüntüleri için geçerlidir.

* Autodesk Maya I/O 2017 Güncelleştirme 5 (cut 201708032230)
* Autodesk Maya I/O 2018 Güncelleme 2 (cut 201711281015)
* Autodesk Maya I/O 2019 Güncelleme 1
* Maya için Autodesk Arnold 2017 (Arnold sürüm 5.3.1.1) MtoA-3.2.1.1-2017
* Maya için Autodesk Arnold 2018 (Arnold sürüm 5.3.1.1) MtoA-3.2.1.1-2018
* Maya için Autodesk Arnold 2019 (Arnold sürüm 5.3.1.1) MtoA-3.2.1.1-2019
* Chaos Group V-Ray for Maya 2017 (sürüm 3.60.04)
* Chaos Group V-Ray for Maya 2018 (sürüm 3.60.04)
* Blender (2.68)
* Blender (2.8)

## <a name="applications-on-latest-windows-server-2016-rendering-images"></a>En son Windows Server 2016 render görüntüleri uygulamaları

Aşağıdaki liste Windows Server 2016, sürüm 1.3.8 render görüntüleri için geçerlidir.

* Autodesk Maya I/O 2017 Güncelleştirme 5 (sürüm 17.4.5459)
* Autodesk Maya I/O 2018 Güncelleme 6 (sürüm 18.4.0.7622)
* Autodesk Maya I/O 2019
* Autodesk 3ds Max I/O 2018 Güncelleştirme 4 (sürüm 20.4.0.4254)
* Autodesk 3ds Max I/O 2019 Güncelleme 1 (sürüm 21.2.0.2219)
* Autodesk 3ds Max I/O 2020 Güncelleme 2
* Maya için Autodesk Arnold 2017 (Arnold sürüm 5.3.0.2) MtoA-3.2.0.2-2017
* Maya için Autodesk Arnold 2018 (Arnold sürüm 5.3.0.2) MtoA-3.2.0.2-2018
* Maya için Autodesk Arnold 2019 (Arnold sürüm 5.3.0.2) MtoA-3.2.0.2-2019
* Autodesk Arnold 3ds Max 2018 (Arnold sürüm 5.3.0.2)(sürüm 1.2.926) için
* Autodesk Arnold 3ds Max 2019 (Arnold sürüm 5.3.0.2)(sürüm 1.2.926) için
* Autodesk Arnold 3ds Max 2020 (Arnold sürümü 5.3.0.2) (sürüm 1.2.926) için
* Maya 2017 için Kaos Grubu V-Ray (sürüm 4.12.01)
* Maya 2018 için Kaos Grubu V-Ray (sürüm 4.12.01)
* Maya 2019 için Kaos Grubu V-Ray (sürüm 4.04.03)
* 3ds Max 2018 için Kaos Grubu V-Ray (sürüm 4.20.01)
* Chaos Group V-Ray 3ds Max 2019 için (sürüm 4.20.01)
* Kaos Grubu V-Ray 3ds Max 2020 (sürüm 4.20.01) için
* Blender (2.79)
* Blender (2.80)
* AZ 10

> [!IMPORTANT]
> [Azure Toplu Iş Uzantısı şablonlarının](https://github.com/Azure/batch-extension-templates)dışında Maya ile `vrayses.exe` V-Ray çalıştırmak için, işleme çalıştırmadan önce başlayın. Vrayses.exe'yi şablonların dışında başlatmak için aşağıdaki `%MAYA_2017%\vray\bin\vrayses.exe"`komutu kullanabilirsiniz.
>
> Örneğin, GitHub'daki Maya [ve V-Ray şablonunun](https://github.com/Azure/batch-extension-templates/blob/master/templates/maya/render-vray-windows/pool.template.json) başlangıç görevine bakın.

## <a name="applications-on-previous-windows-server-2016-rendering-images"></a>Önceki Windows Server 2016'daki görüntüler oluşturma uygulamaları

Aşağıdaki liste Windows Server 2016, sürüm 1.3.7 render görüntüleri için geçerlidir.

* Autodesk Maya I/O 2017 Güncelleştirme 5 (sürüm 17.4.5459)
* Autodesk Maya I/O 2018 Güncelleme 4 (sürüm 18.4.0.7622)
* Autodesk 3ds Max I/O 2019 Güncelleme 1 (sürüm 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Güncelleştirme 4 (sürüm 20.4.0.4254)
* Maya için Autodesk Arnold 2017 (Arnold sürüm 5.2.0.1) MtoA-3.1.0.1-2017
* Maya için Autodesk Arnold 2018 (Arnold sürüm 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold 3ds Max 2018 (Arnold sürüm 5.0.2.4)(sürüm 1.2.926) için
* Autodesk Arnold 3ds Max 2019 (Arnold sürümü 5.0.2.4)(sürüm 1.2.926) için
* Maya 2018 için Kaos Grubu V-Ray (sürüm 3.52.03)
* Chaos Group V-Ray for 3ds Max 2018 (sürüm 3.60.02)
* Maya 2019 için Kaos Grubu V-Ray (sürüm 3.52.03)
* Chaos Group V-Ray 3ds Max 2019 için (sürüm 4.10.01)
* Blender (2.79)

> [!NOTE]
> Chaos Group V-Ray 3ds Max 2019 (sürüm 4.10.01) için V-ray kırılma değişiklikleri tanıttı. Önceki sürümü (sürüm 3.60.02) kullanmak için Windows Server 2016, sürüm 1.3.2 render düğümlerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Görüntüleme VM görüntülerini kullanmak için, bir havuz oluşturulduğunda havuz yapılandırmasında belirtilmesi gerekir; işlemek [için Toplu havuz özelliklerine](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools)bakın.
