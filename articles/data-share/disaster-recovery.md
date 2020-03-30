---
title: Azure Veri Paylaşımı için olağanüstü durum kurtarma
description: Azure Veri Paylaşımı için olağanüstü durum kurtarma
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: a736e3ddfcf785f9ce27140eed58374a0732c1f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75483186"
---
# <a name="disaster-recovery-for-azure-data-share"></a>Azure Veri Paylaşımı için olağanüstü durum kurtarma

Bu makalede, Azure Veri Paylaşımı için olağanüstü durum kurtarma ortamının nasıl yapılandırılabildiğini gözden geçireceğiz. Azure veri merkezi kesintileri nadirdir, ancak birkaç dakika ile saat arasında herhangi bir yerde sürebilir. Veri Merkezi kesintileri, veri sağlayıcısı tarafından paylaşılan verilere dayanan ortamlarda kesintiye neden olabilir. Bu makalede ayrıntılı olarak belirtilen adımları izleyerek, veri sağlayıcıları veri paylaşımınızı barındıran birincil bölge için bir veri merkezi kesintisi durumunda verileri veri tüketicileri ile paylaşmaya devam edebilir. 

## <a name="achieving-business-continuity-for-azure-data-share"></a>Azure Veri Paylaşımı için iş sürekliliği elde etme

Veri merkezi kesintisi için hazırlıklı olmak için, veri sağlayıcısı ikincil bir bölgede sağlanan bir veri paylaşım ortamı na sahip olabilir. Bir veri merkezi kesintisi meydana gelmesi durumunda sorunsuz bir başarısızlık sağlayacak alınabilir önlemler vardır. 

Veri sağlayıcıları, ikincil Azure Veri Paylaşımı kaynaklarını ek bir bölgede sağlayabilir. Bu Veri Paylaşımı kaynakları, birincil veri paylaşım ortamında bulunan veri kümelerini içerecek şekilde yapılandırılabilir. Veri tüketicileri, DR ortamını yapılandırırken veri payına eklenebilir veya daha sonraki bir zamanda eklenebilir (örn. el ile başarısız adımların bir parçası olarak).

Veri tüketicileri DR amaçları için sağlanan ikincil bir ortamda etkin bir hisse aboneliğine sahipse, anlık görüntü zamanlamasını bir hatanın parçası olarak etkinleştirebilirler. Veri tüketicileri DR amacıyla ikincil bir bölgeye abone olmak istemiyorsa, daha sonraki bir zamanda ikincil veri paylaşımına davet edilebilirler. 

Veri tüketicileri, DR amaçları için boşta olan etkin bir paylaşım aboneliğine sahip olabilir veya veri sağlayıcıları bunları manuel hata yordamlarının bir parçası olarak daha sonraki bir zamanda ekleyebilir. 

## <a name="related-information"></a>İlgili bilgiler

- [İş Sürekliliği ve Olağanüstü Durum Kurtarma](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
- [BCDR stratejinize yüksek düzeyde kullanılabilirlik ekleme](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/build-high-availability-into-your-bcdr-strategy)

## <a name="next-steps"></a>Sonraki adımlar

Veri paylaşmaya nasıl başlayacağınızı öğrenmek [için, veri öğreticinizi paylaşmaya devam edin.](share-your-data.md)




