---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 55b545471cbe45fe35e28879270e4340cf9d3834
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026021"
---
Kaynakları Azure'a dağıtırken, hangi tür kaynakların dağıtılacağına, nerede bulunduklarına ve nasıl ayarlanacaklarına karar vermede inanılmaz bir esnekliğe sahip olursunuz. Ancak bu esneklik, kuruluşunuzda izin vermek istediğinizden daha fazla seçeneğe yol açabilir. Kaynakları Azure'a dağıtmayı düşünürken şunları merak ediyor olabilirsiniz:

* Belirli ülkelerde/bölgelerde veri egemenliği için yasal gereksinimleri karşılayacak Nasıl yaparım? mı?
* Maliyeti nasıl kontrol edebilirim?
* Birilerinin istemeden önemli bir sistemi değiştirmemesini nasıl sağlayabilirim?
* Kaynak maliyetlerini nasıl izleyip doğru bir şekilde faturalarım?

Bu makalede bu sorular ele alınır. Daha ayrıntılı belirtmek gerekirse, siz:

> [!div class="checklist"]
> * Kullanıcıların beklenen eylemleri gerçekleştirme izinlerinin olması ancak daha fazla eylemin engellenmesi için kullanıcılara rol atayıp bu rolleri bir kapsama atayın.
> * Aboneliğinizdeki kaynaklara yönelik kurallar belirleyen ilkeler uygulayın.
> * Sisteminiz için kritik olan kaynakları kilitleyin.
> * Kuruluşunuzun anlayacağı değerlere göre izlemek için kaynakları etiketleyin.

Bu makalede, idare uygulamak için üstlendiğiniz görevlere odaklanılır. Kavramlarla ilgili daha geniş kapsamlı bir tartışma için bkz. [Azure'da İdare](../articles/governance/index.yml).