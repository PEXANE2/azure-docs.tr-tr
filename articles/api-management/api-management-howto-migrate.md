---
title: Bölgeler arasında Azure API Management geçirme
description: Bir API Management örneğini bir bölgeden diğerine geçirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: 831eacf8197e90d7ae6b752ee11898c9667b5b6a
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70036550"
---
# <a name="how-to-migrate-azure-api-management-across-regions"></a>Bölgeler arasında Azure API Management geçirme
API Management örneklerini bir Azure bölgesinden diğerine geçirmek için [yedekleme ve geri yükleme](api-management-howto-disaster-recovery-backup-restore.md) özelliğini kullanabilirsiniz. Kaynak ve hedef bölgelerde aynı API Management fiyatlandırma katmanını seçmeniz gerekir. 

> [!NOTE]
> Yedekleme ve geri yükleme farklı bulut türleri arasında geçiş yaparken çalışmaz. Bu şekilde, kaynağı [bir şablon olarak](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-portal#export-resource-groups-to-templates)dışarı aktarmanız gerekir. Ardından, hedef Azure bölgesi için, aktarılmış şablonu uyarlayın ve kaynağı yeniden oluşturun. 

## <a name="option-1-use-a-different-api-management-instance-name"></a>Seçenek 1: Farklı bir API Management örnek adı kullanın

1. Hedef bölgede, kaynak API Management örneğiyle aynı fiyatlandırma katmanına sahip yeni bir API Management örneği oluşturun. Yeni örnek farklı bir ada sahip olmalıdır. 
1. Mevcut API Management örneğini bir depolama hesabına yedekleyin.
1. Adım 2 ' de oluşturulan yedeği, 1. adımdaki yeni bölgede oluşturulan yeni API Management örneğine geri yükleyin.
1. Kaynak bölge API Management örneğine işaret eden özel bir etki alanınız varsa, özel etki alanı CNAME ' i yeni API Management örneğini işaret etmek üzere güncelleştirin. 


## <a name="option-2-use-the-same-api-management-instance-name"></a>Seçenek 2: Aynı API Management örnek adını kullan

> [!NOTE]
> Bu seçenek, geçiş sırasında kapalı kalma süresine neden olur.

1. Kaynak bölgedeki API Management örneğini bir depolama hesabına yedekleyin.
1. Kaynak bölgedeki API Management silin. 
1. Hedef bölgede, kaynak bölgeyle aynı ada sahip yeni bir API Management örneği oluşturun.
1. Adım 1 ' de oluşturulan yedeği hedef bölgedeki yeni API Management örneğine geri yükleyin.  


## <a name="next-steps"> </a>Sonraki adımlar
* Yedekleme ve geri yükleme özelliği hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarmayı uygulama](api-management-howto-disaster-recovery-backup-restore.md).
* Geçiş Azure kaynakları hakkında daha fazla bilgi için bkz. [Azure çapraz bölge geçiş kılavuzu](https://github.com/Azure/Azure-Migration-Guidance).