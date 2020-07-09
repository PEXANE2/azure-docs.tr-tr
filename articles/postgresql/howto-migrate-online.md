---
title: PostgreSQL için Azure veritabanı 'na en az kapalı kalma süresi-tek sunucu
description: Bu makalede, Azure veritabanı geçiş hizmeti kullanılarak bir PostgreSQL veritabanının Azure veritabanı 'na PostgreSQL için Azure veritabanı 'na en az kapalı kalma süresi nasıl gerçekleştireceğiniz açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 0b7c6392fbd795a078e9ec8f61281d95cf6363bc
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86112400"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'na en az kapalı kalma süresi-tek sunucu
[Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) için yeni sunulan **sürekli eşitleme özelliğini** kullanarak PostgreSQL için Azure veritabanı 'na geçiş işlemini en az kapalı kalma süresine sahip olabilirsiniz. Bu işlevsellik, uygulama tarafından tahakkuk eden kesinti süresini sınırlandırır.

## <a name="overview"></a>Genel Bakış
Azure DMS, şirket içinde PostgreSQL için Azure veritabanı 'na ilk kez yük gerçekleştirir ve ardından uygulama çalışmaya devam ederken yeni işlemleri sürekli olarak Azure 'a eşitler. Veriler hedef Azure tarafında tamamlandıktan sonra, uygulamayı kısa bir süre (en düşük kapalı kalma süresi) durdurur, son veri toplu iş için bekleyin (uygulamanın yeni bir trafiği almak için etkin hale gelene kadar uygulamayı durdurmanız gerekir) ve ardından Bağlantı dizenizi Azure 'a işaret etmek üzere güncelleştirin. İşiniz bittiğinde, uygulamanız Azure 'da canlı olacaktır!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL uygulamalarının PostgreSQL için Azure veritabanı 'na nasıl geçirileceğini gösteren bir demo içeren [Microsoft Azure ile video uygulaması Modernleştirmesini](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102)görüntüleyin.
- [DMS kullanarak PostgreSQL for PostgreSQL Için Azure veritabanı 'Na geçiş](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online)öğreticisine bakın.
