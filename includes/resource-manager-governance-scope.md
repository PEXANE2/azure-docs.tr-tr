---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 14ac8ff54f5627509fe10a90bfdd0a01c07762d9
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027488"
---
Bir öğe oluşturmadan önce kapsam kavramını gözden geçirelim. Azure dört yönetim düzeyi sunar: yönetim grupları, abonelik, kaynak grubu ve kaynak. [Yönetim grupları](../articles/governance/management-groups/overview.md) önizleme sürümündedir. Aşağıdaki resimde bu katmanlara ait bir örnek gösterilir.

![Kapsam](./media/resource-manager-governance-scope/scope-levels.png)

Yönetim ayarlarını bu kapsam düzeylerinden birinde uygularsınız. Seçtiğiniz düzey, ayarın ne kadar yaygın olarak uygulanacağını belirler. Düşük düzeyler, yüksek düzeylerdeki ayarları devralır. Aboneliğe bir ayar uyguladığınızda, bu ayar aboneliğinizdeki tüm kaynak gruplarına ve kaynaklara uygulanır. Kaynak grubunda bir ayar uyguladığınızda, bu ayar kaynak grubu ve tüm kaynaklarına uygulanır. Ancak başka bir kaynak grubunda bu ayar bulunmaz.

Genellikle, önemli ayarları yüksek düzeylerde, projeye özgü gereksinimleri ise düşük düzeylerde uygulamak en mantıklı yoldur. Örneğin, kuruluşunuza ait tüm kaynakların belirli bölgelere dağıtıldığından emin olmak isteyebilirsiniz. Bu gereksinimi gerçekleştirmek için, aboneliğe izin verilen konumları belirten bir ilke uygulayın. Kuruluşunuzda bulunan diğer kullanıcılar yeni kaynak grupları ve kaynaklar eklediğinde, izin verilen konumlar otomatik olarak uygulanır.