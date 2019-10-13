---
title: Yüksek kullanılabilirlik Azure Analysis Services | Microsoft Docs
description: Yüksek kullanılabilirlik Azure Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 151739c77939ab98b0cdca6d75692267ef0d4385
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72295320"
---
# <a name="analysis-services-high-availability"></a>Yüksek kullanılabilirlik Analysis Services

Bu makalede Azure Analysis Services sunucularının en yüksek kullanılabilirliği açıklanır. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Hizmet kesintisi sırasında yüksek kullanılabilirlik

Nadir bir Azure veri merkezinde kesinti olabilir. Bir kesinti oluştuğunda, en son birkaç dakika veya saat için en son olabilecek iş kesintilerine neden olabilir. Yüksek kullanılabilirlik çoğu zaman sunucu yedekliliği ile elde edilir. Azure Analysis Services, bir veya daha fazla bölgede ek, ikincil sunucular oluşturarak yedeklilik elde edebilirsiniz. Gereksiz sunucular oluştururken, bu sunuculardaki verilerin ve meta verilerin güvence altına almak için, çevrimdışı duruma geçmiş bir bölgedeki sunucu ile eşitlenmiş durumda olabilir:

* Diğer bölgelerdeki gereksiz sunuculara modeller dağıtın. Bu yöntem, hem birincil sunucunuzda hem de yedekli sunucularınızdaki verilerin hem paralel hem de aynı şekilde işlenmesini gerektirir, bu da tüm sunucular eşitlenir.

* Birincil sunucunuzdaki veritabanlarını [yedekleyin](analysis-services-backup.md) ve gereksiz sunuculara geri yükleyin. Örneğin, gecelik yedeklemeleri Azure depolama 'ya otomatikleştirebilir ve diğer bölgelerdeki diğer yedekli sunuculara geri yükleyebilirsiniz. 

Her iki durumda da, birincil sunucunuz bir kesinti yaşıyorsa, raporlama istemcilerindeki bağlantı dizelerini farklı bir bölgesel veri merkezinde sunucuya bağlanacak şekilde değiştirmeniz gerekir. Bu değişiklik, son çare ve yalnızca çok zararlı bir bölgesel veri merkezi kesintisi oluşursa göz önünde bulundurulmalıdır. Tüm istemcilerde bağlantıları güncelleştirebilmeniz için birincil sunucunuzu barındıran bir veri merkezi kesintisi daha olasıdır. 

Raporlama istemcilerinde bağlantı dizelerini değiştirmek zorunda kalmamak için, birincil sunucunuz için bir sunucu [diğer adı](analysis-services-server-alias.md) oluşturabilirsiniz. Birincil sunucu kapalıysa diğer adı başka bir bölgedeki gereksiz sunucuya işaret etmek üzere değiştirebilirsiniz. Birincil sunucuda bir uç nokta sistem durumu denetimi kodlayarak sunucu adı ' nı otomatik hale getirebilirsiniz. Sistem durumu denetimi başarısız olursa, aynı uç nokta başka bir bölgedeki gereksiz sunucuya doğrudan yönlendirebilir. 

## <a name="related-information"></a>İlgili bilgiler

[Yedekleme ve geri yükleme](analysis-services-backup.md)   
[Azure Analysis Services @no__t Yönet](analysis-services-manage.md)-1  
[Diğer ad sunucu adları](analysis-services-server-alias.md) 

