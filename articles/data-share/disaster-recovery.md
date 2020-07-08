---
title: Azure veri paylaşımında olağanüstü durum kurtarma
description: Azure veri paylaşımında olağanüstü durum kurtarma
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: a736e3ddfcf785f9ce27140eed58374a0732c1f1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75483186"
---
# <a name="disaster-recovery-for-azure-data-share"></a>Azure veri paylaşımında olağanüstü durum kurtarma

Bu makalede, Azure veri paylaşımında bir olağanüstü durum kurtarma ortamının nasıl yapılandırılacağını adım adım inceleyeceğiz. Azure veri merkezine kesintiler nadir bir durumdur, ancak en son birkaç dakika ile saat arasında bir süre olabilir. Veri merkezi kesintileri, veri sağlayıcısı tarafından paylaşılmakta olan verilere bağlı ortamlarda kesintiye neden olabilir. Bu makalede açıklanan adımları izleyerek, veri sağlayıcıları veri paylaşımınızı barındıran birincil bölge için bir veri merkezi kesintisi durumunda veri tüketicileriyle veri paylaşmaya devam edebilir. 

## <a name="achieving-business-continuity-for-azure-data-share"></a>Azure veri paylaşımında iş sürekliliği elde etmek

Veri merkezi kesintisi için hazırlanmak üzere, veri sağlayıcısı ikincil bir bölgede sağlanan bir veri paylaşma ortamına sahip olabilir. Veri merkezi kesintisinden oluşması durumunda sorunsuz bir yük devretme gerçekleştiğinden, alınabilecek ölçüler vardır. 

Veri sağlayıcıları ek bir bölgede ikincil Azure veri paylaşma kaynakları sağlayabilir. Bu veri paylaşma kaynakları, birincil veri paylaşma ortamında mevcut olan veri kümelerini içerecek şekilde yapılandırılabilir. Veri tüketicileri, DR ortamını yapılandırırken veri paylaşımında eklenebilir veya daha sonraki bir zamanda içine eklenir (ör. el ile yük devretme adımlarının bir parçası olarak).

Veri tüketicileri, DR amaçları için sağlanan ikincil bir ortamda etkin bir Share aboneliğine sahip ise, bir yük devretmenin parçası olarak anlık görüntü zamanlamasını etkinleştirebilirler. Veri tüketicileri DR amaçları için ikincil bir bölgeye abone olmak istemiyor, daha sonraki bir zamanda ikincil veri paylaşımında davet edilebilir. 

Veri tüketicileri, DR amaçları için boşta olan etkin bir paylaşma aboneliğine sahip olabilir veya veri sağlayıcıları el ile yük devretme yordamlarının bir parçası olarak bunları daha sonraki bir zamanda ekleyebilirler. 

## <a name="related-information"></a>İlgili bilgiler

- [İş sürekliliği ve olağanüstü durum kurtarma](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
- [BCDR stratejinize yüksek düzeyde kullanılabilirlik ekleme](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/build-high-availability-into-your-bcdr-strategy)

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.




