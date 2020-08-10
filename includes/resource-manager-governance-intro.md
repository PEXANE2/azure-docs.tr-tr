---
title: dosya dahil etme
description: dosya dahil etme
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: d1e7fa1ed1649508f0d4923db8817d17ad556ca1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67188301"
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

Bu makalede, idare uygulamak için üstlendiğiniz görevlere odaklanılır. Kavramlarla ilgili daha geniş kapsamlı bir tartışma için bkz. [Azure'da İdare](../articles/security/governance-in-azure.md). 
