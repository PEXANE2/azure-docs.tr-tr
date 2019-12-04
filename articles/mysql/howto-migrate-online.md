---
title: En az kapalı kalma süresi geçişi-MySQL için Azure veritabanı
description: Bu makalede, Azure veritabanı geçiş hizmeti kullanılarak MySQL için Azure veritabanı 'na bir MySQL veritabanının en düşük kapalı kalma süresinin nasıl gerçekleştirileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 9f166323cc72cd22cc4dd28babdfd056100a32e0
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74774152"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'na en düşük kesinti süresi geçişi
[Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) için yeni sunulan **sürekli eşitleme özelliğini** kullanarak, MySQL Için Azure veritabanı 'na MySQL geçişleri gerçekleştirebilirsiniz. Bu işlevsellik, uygulama tarafından tahakkuk eden kesinti süresini sınırlandırır.

## <a name="overview"></a>Genel Bakış
Azure DMS, şirket içi şirketinizin ilk yükünü MySQL için Azure veritabanı 'na uygular ve uygulama çalışmaya devam ederken yeni işlemleri sürekli olarak Azure 'a eşitler. Veriler hedef Azure tarafında tamamlandıktan sonra, uygulamayı kısa bir süre (en düşük kapalı kalma süresi) durdurur, son veri toplu iş için bekleyin (uygulama, yeni trafik almak için etkin hale gelene kadar uygulamayı durdurmanız gerekir) hedefte ve ardından Bağlantı dizenizi Azure 'a işaret etmek üzere güncelleştirin. İşiniz bittiğinde, uygulamanız Azure 'da canlı olacaktır!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- MySQL uygulamalarının MySQL için Azure veritabanı 'na nasıl geçirileceğini gösteren bir demo içeren [MySQL/PostgreSQL uygulamalarını Azure yönetilen hizmetine kolayca geçirin](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201).
- [DMS kullanarak MySQL Için MySQL for MySQL 'e Azure veritabanı 'Na geçiş](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)öğreticisine bakın.
