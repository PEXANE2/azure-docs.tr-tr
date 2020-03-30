---
title: Azure Red Hat OpenShift için desteklenen kaynaklar
description: Microsoft Azure Red Hat OpenShift tarafından hangi Azure bölgelerinin ve sanal makine boyutlarının desteklenerek desteklendirilmelerini anlayın.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 1b794852390141d31a3218a1a434c21133db914c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243672"
---
# <a name="azure-red-hat-openshift-resources"></a>Azure Red Hat OpenShift kaynakları

Bu konu, Microsoft Azure Red Hat OpenShift hizmeti tarafından desteklenen Azure bölgelerini ve sanal makine boyutlarını listeler.

## <a name="azure-regions"></a>Azure bölgeleri

Azure Kırmızı Şapka OpenShift kümelerini dağıtabileceğiniz bölgelerin geçerli bir listesi için [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=openshift&regions=all) bakın.

## <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Azure Red Hat OpenShift kümenizdeki bilgi işlem düğümleri için belirtebileceğiniz desteklenen sanal makine boyutları aşağıda veda edilmiştir.

> [!Important]
> Her VM'nin ekilebilen farklı sayıda sürücüsü vardır. Bu bellek veya CPU boyutu kadar hemen açık olmayabilir.
> Tüm VM boyutları tüm bölgelerde kullanılamaz. API belirttiğiniz boyutu desteklese bile, boyut belirttiğiniz bölgede kullanılamıyorsa bir hata alabilirsiniz.
> Daha fazla bilgi için [bölge başına desteklenen VM boyutlarının geçerli listesine](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) bakın.

## <a name="compute-node-sizes"></a>Hesap düğümü boyutları

Aşağıdaki işlem düğümü boyutları Azure Red Hat OpenShift REST API tarafından desteklenir:

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

Aşağıdaki ana / altyapı düğümü boyutları Azure Red Hat OpenShift REST API tarafından desteklenir:

|Boyut|Sanal işlemci|RAM|
|-|-|-|
|Standart D4s v3|4|16 GB|
|Standart D8s v3|8|32 GB|
|Standart D16s v3|16|64 GB|
|Standart D32s v3|32|128 GB|

## <a name="next-steps"></a>Sonraki adımlar

Azure [Kırmızı Şapka OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisini deneyin.
