---
title: Azure Analiz Hizmetleri yüksek kullanılabilirlik | Microsoft Dokümanlar
description: Bu makalede, Azure Analiz Hizmetleri'nin hizmet kesintisi sırasında nasıl yüksek kullanılabilirlik sağladığı açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 78a6d41b638d79111a58830f0cb0d5190ea0796c
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408677"
---
# <a name="analysis-services-high-availability"></a>Analiz Hizmetleri yüksek kullanılabilirlik

Bu makalede, Azure Çözümleme Hizmetleri sunucuları için yüksek kullanılabilirlik sağlanması açıklanmaktadır. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Bir hizmet kesintisi sırasında yüksek kullanılabilirlik sağlanması

Nadir olmakla birlikte, bir Azure veri merkezinin kesintisi olabilir. Bir kesinti meydana geldiğinde, birkaç dakika veya saatlerce sürebilir bir iş kesintisi neden olur. Yüksek kullanılabilirlik genellikle sunucu artıklığı ile elde edilir. Azure Çözümleme Hizmetleri ile, bir veya daha fazla bölgede ek, ikincil sunucular oluşturarak fazlalık elde edebilirsiniz. Gereksiz sunucular oluştururken, bu sunucularda veri ve meta verilerin çevrimdışı olan bir bölgedeki sunucuyla eşitlendiğinden emin olmak için şunları yapabilirsiniz:

* Modelleri diğer bölgelerdeki gereksiz sunuculara dağıtın. Bu yöntem, hem birincil sunucunuzdaki hem de gereksiz sunucularda verileri paralel olarak işlemeyi ve tüm sunucuların eşitlenmede olduğundan güvence verilmeden gerekir.

* Birincil sunucunuzdan [yedek](analysis-services-backup.md) veritabanları ve yedek li sunucularda geri yükleyin. Örneğin, azure depolama için gecelik yedeklemeleri otomatikleştirebilir ve diğer bölgelerdeki diğer gereksiz sunuculara geri yükleyebilirsiniz. 

Her iki durumda da, birincil sunucunuzda bir kesinti yaşanıyorsa, farklı bir bölgesel veri merkezinde sunucuya bağlanmak için raporlama istemcilerinin bağlantı dizelerini değiştirmeniz gerekir. Bu değişiklik son çare olarak kabul edilmelidir ve sadece bir felaket bölgesel veri merkezi kesintisi oluşursa. Birincil sunucunuzu barındıran bir veri merkezi kesintisinin, tüm istemcilerin bağlantılarını güncellemeden önce tekrar çevrimiçi duruma gelmesi daha olasıdır. 

Raporlama istemcileri üzerindeki bağlantı dizelerini değiştirmek zorunda kalmamak için birincil sunucunuz için bir sunucu [adı](analysis-services-server-alias.md) oluşturabilirsiniz. Birincil sunucu çökerse, diğer adı başka bir bölgedeki gereksiz bir sunucuya işaret etmek için değiştirebilirsiniz. Birincil sunucuda bir uç nokta durumu denetimi kodlayarak sunucu adına takma adı otomatikleştirebilirsiniz. Sistem durumu denetimi başarısız olursa, aynı bitiş noktası başka bir bölgedeki gereksiz bir sunucuya yönlendirilebilir. 

## <a name="related-information"></a>İlgili bilgiler

[Yedekleme ve geri yükleme](analysis-services-backup.md)   
[Azure Analiz Hizmetlerini Yönetme](analysis-services-manage.md)   
[Diğer ad sunucu adları](analysis-services-server-alias.md) 

