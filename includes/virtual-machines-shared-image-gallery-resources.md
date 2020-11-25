---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/04/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 433e909563602a2ef32b7986959b428c9afaf9f4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96016007"
---
| Kaynak | Açıklama|
|----------|------------|
| **Görüntü kaynağı** | Bu, bir görüntü galerisinde **görüntü sürümü** oluşturmak için kullanılabilecek bir kaynaktır. Görüntü kaynağı, başka bir görüntü galerisinde [Genelleştirilmiş veya özelleştirilmiş](../articles/virtual-machines/windows/shared-image-galleries.md#generalized-and-specialized-images), yönetilen bir görüntü, anlık görüntü veya görüntü sürümü olan mevcut BIR Azure VM olabilir. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** , görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimlerin erişimi olduğunu kontrol edersiniz. |
| **Görüntü tanımı** | Görüntü tanımları bir galeri içinde oluşturulur ve görüntü ve bu dosyayı dahili olarak kullanmaya yönelik gereksinimler hakkında bilgi taşır. Bu, görüntünün Windows veya Linux, sürüm notları ve en düşük ve en yüksek bellek gereksinimleri olduğunu içerir. Bu, bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** , galerı kullanılırken VM oluşturmak için kullandığınız şeydir. Ortamınız için gerektiğinde bir görüntünün birden fazla sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, bir sanal makine oluşturmak için bir **görüntü sürümü** kullandığınızda, sanal makine için yeni diskler oluşturmak üzere görüntü sürümü kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |