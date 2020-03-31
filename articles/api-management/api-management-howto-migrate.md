---
title: Azure API Yönetimi bölgeler e göre nasıl geçirilir?
description: Bir API Yönetimi örneğini bir bölgeden diğerine nasıl geçirteceklerini öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: 3294a7b2112e9527041ef343f4452aedb7a2a272
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073473"
---
# <a name="how-to-migrate-azure-api-management-across-regions"></a>Azure API Yönetimi bölgeler e göre nasıl geçirilir?
API Yönetimi örneklerini bir Azure bölgesinden diğerine geçirmek için [yedekleme ve geri yükleme](api-management-howto-disaster-recovery-backup-restore.md) özelliğini kullanabilirsiniz. Kaynak ve hedef bölgelerde aynı API Yönetimi fiyatlandırma katmanını seçmelisiniz. 

> [!NOTE]
> Yedekleme ve geri yükleme, farklı bulut türleri arasında geçiş yaparken çalışmaz. Bunun için kaynağı [şablon olarak](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-portal#export-resource-groups-to-templates)dışa aktarmanız gerekir. Ardından, hedef Azure bölgesi için dışa aktarılan şablonu uyarlayın ve kaynağı yeniden oluşturun. 

## <a name="option-1-use-a-different-api-management-instance-name"></a>Seçenek 1: Farklı bir API Yönetimi örnek adı kullanma

1. Hedef bölgede, kaynak API Yönetimi örneğiyle aynı fiyatlandırma katmanına sahip yeni bir API Yönetimi örneği oluşturun. Yeni örnek farklı bir ada sahip olmalıdır. 
1. Varolan API Yönetimi örneğini bir depolama hesabına yedekleme.
1. Adım 2'de oluşturulan yedeklemeyi, Adım 1'deki yeni bölgede oluşturulan yeni API Yönetimi örneğine geri yükleyin.
1. Kaynak bölge API Yönetimi örneğini gösteren özel bir etki alanınız varsa, yeni API Yönetimi örneğini işaret etmek için özel etki alanı CNAME'yi güncelleştirin. 


## <a name="option-2-use-the-same-api-management-instance-name"></a>Seçenek 2: Aynı API Yönetimi örnek adını kullanma

> [!NOTE]
> Bu seçenek, geçiş sırasında kapalı kalma süresine neden olur.

1. Kaynak bölgedeki API Yönetimi örneğini bir depolama hesabına yedekle.
1. Kaynak bölgedeki API Yönetimini silin. 
1. Hedef bölgede, kaynak bölgedekiyle aynı ada sahip yeni bir API Yönetimi örneği oluşturun.
1. Adım 1'de oluşturulan yedeklemeyi hedef bölgedeki yeni API Yönetimi örneğine geri yükleyin.  


## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar
* Yedekleme ve geri yükleme özelliği hakkında daha fazla bilgi için [olağanüstü durum kurtarmanın nasıl uygulanacağını](api-management-howto-disaster-recovery-backup-restore.md)görün.
* Azure kaynaklarının geçişi hakkında bilgi için [Azure bölgeler arası geçiş kılavuzuna](https://github.com/Azure/Azure-Migration-Guidance)bakın.