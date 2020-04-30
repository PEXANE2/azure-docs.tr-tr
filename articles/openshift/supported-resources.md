---
title: Azure Red Hat OpenShift 3,11 için desteklenen kaynaklar
description: Microsoft Azure Red Hat OpenShift tarafından desteklenen Azure bölgelerini ve sanal makine boyutlarını anlayın.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 38203cede755d776ba9142ad16e1bea1fd1637a6
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82203684"
---
# <a name="azure-red-hat-openshift-resources"></a>Azure Red Hat OpenShift kaynakları

Bu konu, Microsoft Azure Red Hat OpenShift 3,11 hizmeti tarafından desteklenen Azure bölgelerini ve sanal makine boyutlarını listeler.

## <a name="azure-regions"></a>Azure bölgeleri

Azure Red Hat OpenShift kümelerini dağıtabileceğiniz bölgelerin geçerli bir listesi için [bölgeye göre kullanılabilir ürünlere](https://azure.microsoft.com/global-infrastructure/services/?products=openshift&regions=all) bakın.

## <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Azure Red Hat OpenShift kümenizdeki işlem düğümleri için belirtebileceğiniz desteklenen sanal makine boyutları aşağıda verilmiştir.

> [!Important]
> Her VM 'nin iliştirilebilecek farklı sayıda sürücüsü vardır. Bu, bellek veya CPU boyutu kadar hemen açık olmayabilir.
> Tüm bölgelerde tüm VM boyutları kullanılamaz. API belirttiğiniz boyutu desteklese de, boyut belirttiğiniz bölgede yoksa bir hata alabilirsiniz.
> Daha fazla bilgi için bkz. [bölge başına desteklenen VM boyutlarının geçerli listesi](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) .

## <a name="compute-node-sizes"></a>İşlem düğüm boyutları

Aşağıdaki işlem düğümü boyutları Azure Red Hat Openshıft REST API tarafından desteklenir:

|Boyut|Sanal işlemci|RAM|
|-|-|-|
|Standart D4s v3|4|16 GB|
|Standart D8s v3|8|32 GB|
|Standart D16s v3|16|64 GB|
|Standart D32s v3|32|128 GB|
|-|-|-|
|Standart E4s v3|4|32 GB|
|Standart E8s v3|8|64 GB|
|Standart E16s v3|16|128 GB|
|Standart E32s v3|32|256 GB|
|-|-|-|
|Standart F8s v2|8|16 GB|
|Standart F16s v2|16|32 GB|
|Standart F32s v2|32|64 GB|

## <a name="master-node-sizes"></a>Ana düğüm boyutları

Aşağıdaki ana/altyapı düğüm boyutları, Azure Red Hat Openshıft REST API tarafından desteklenir:

|Boyut|Sanal işlemci|RAM|
|-|-|-|
|Standart D4s v3|4|16 GB|
|Standart D8s v3|8|32 GB|
|Standart D16s v3|16|64 GB|
|Standart D32s v3|32|128 GB|

## <a name="next-steps"></a>Sonraki adımlar

[Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md) öğreticisini deneyin.
