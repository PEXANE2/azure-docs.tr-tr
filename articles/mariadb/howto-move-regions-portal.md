---
title: Azure bölgelerini taşıma-Azure portal-MariaDB için Azure veritabanı
description: MariaDB sunucusu için bir Azure veritabanını bir Azure bölgesinden diğerine, okuma çoğaltması ve Azure portal kullanarak taşıyın.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 06/29/2020
ms.openlocfilehash: abb692f71a3ed69c6779b6141c9098dc94c75c4f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569104"
---
# <a name="move-an-azure-database-for-mariadb-server-to-another-region-by-using-the-azure-portal"></a>MariaDB sunucusu için Azure veritabanı 'nı Azure portal kullanarak başka bir bölgeye taşıma

MariaDB sunucusu için mevcut bir Azure veritabanını bir bölgeden diğerine taşımaya yönelik çeşitli senaryolar vardır. Örneğin, olağanüstü durum kurtarma planlamanızın bir parçası olarak bir üretim sunucusunu başka bir bölgeye taşımak isteyebilirsiniz.

Farklı bir bölgeye taşıma işlemini gerçekleştirmek için MariaDB [bölgeler arası okuma çoğaltması](concepts-read-replicas.md#cross-region-replication) Için Azure veritabanı 'nı kullanabilirsiniz. Bunu yapmak için ilk olarak hedef bölgede bir okuma çoğaltması oluşturun. Ardından, okuma ve yazma trafiğini kabul eden tek başına bir sunucu haline getirmek için okuma çoğaltması sunucusuna çoğaltmayı durdurun. 

> [!NOTE]
> Bu makale, sunucunuzu farklı bir bölgeye taşımaya odaklanır. Sunucunuzu farklı bir kaynak grubuna veya aboneliğine taşımak istiyorsanız [taşıma](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription) makalesine başvurun. 

## <a name="prerequisites"></a>Ön koşullar

- Çoğaltma oku özelliği yalnızca Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında bulunan MariaDB sunucuları için Azure veritabanı 'nda kullanılabilir. Kaynak sunucunun bu fiyatlandırma katmanlarından birinde olduğundan emin olun.

- MariaDB kaynak sunucusu için Azure veritabanınızın, geçiş yapmak istediğiniz Azure bölgesinde olduğundan emin olun.

## <a name="prepare-to-move"></a>Taşımaya hazırlanma

Azure portal kullanarak hedef bölgede çapraz bölge okuma çoğaltması sunucusu oluşturmak için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Kaynak sunucu olarak kullanmak istediğiniz MariaDB sunucusu için mevcut Azure veritabanını seçin. Bu eylem **genel bakış** sayfasını açar.
1. **Ayarlar**' ın altında, menüden **çoğaltma** ' yı seçin.
1. **Çoğaltma ekle**' yi seçin.
1. Çoğaltma sunucusu için bir ad girin.
1. Çoğaltma sunucusunun konumunu seçin. Varsayılan konum, ana sunucu ile aynıdır. Çoğaltmanın dağıtılmasını istediğiniz hedef konumu seçtiğinizden emin olun.
1. Çoğaltmanın oluşturulmasını onaylamak için **Tamam ' ı** seçin. Çoğaltma oluşturma sırasında, veriler kaynak sunucudan çoğaltmaya kopyalanır. Oluşturma zamanı, kaynak sunucunun boyutuyla orantılı olarak son birkaç dakika veya daha fazla olabilir.

>[!NOTE]
> Bir çoğaltma oluşturduğunuzda, ana sunucunun VNet hizmet uç noktalarını aktarılmaz. Bu kuralların çoğaltma için bağımsız olarak ayarlanması gerekir.

## <a name="move"></a>Taşı

> [!IMPORTANT]
> Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.
> Bir okuma çoğaltmasında çoğaltmayı durdurmadan önce, çoğaltmanın gerekli tüm verilere sahip olduğundan emin olun.

Çoğaltma sunucusuna çoğaltma durdurulduğunda, tek başına sunucu haline gelmesine neden olur. Azure portal çoğaltmaya çoğaltmayı durdurmak için aşağıdaki adımları kullanın:

1. Çoğaltma oluşturulduktan sonra, MariaDB kaynak sunucusu için Azure veritabanınızı bulun ve seçin. 
1. **Ayarlar**' ın altında, menüden **çoğaltma** ' yı seçin.
1. Çoğaltma sunucusunu seçin.
1. **Çoğaltmayı durdur**' u seçin.
1. **Tamam**' a tıklayarak çoğaltmayı durdurmak istediğinizi onaylayın.

## <a name="clean-up-source-server"></a>Kaynak sunucuyu temizle

MariaDB sunucusu için kaynak Azure veritabanını silmek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları kullanın:

1. Çoğaltma oluşturulduktan sonra, MariaDB kaynak sunucusu için Azure veritabanınızı bulun ve seçin.
1. **Genel bakış** penceresinde **Sil**' i seçin.
1. Silmek istediğinizi onaylamak için kaynak sunucunun adını yazın.
1. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir MariaDB sunucusu için Azure veritabanını Azure portal kullanarak bir bölgeden diğerine taşımış sonra gereksiz kaynak kaynaklarını temizlüksiniz. 

- [Okuma çoğaltmaları](concepts-read-replicas.md) hakkında daha fazla bilgi edinin
- [Azure Portal okuma çoğaltmalarını yönetme](howto-read-replicas-portal.md) hakkında daha fazla bilgi edinin
- [İş sürekliliği](concepts-business-continuity.md) seçenekleri hakkında daha fazla bilgi edinin
