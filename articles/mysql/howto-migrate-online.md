---
title: En az kapalı kalma süresi geçişi-MySQL için Azure veritabanı
description: Bu makalede, Azure veritabanı geçiş hizmeti kullanılarak MySQL için Azure veritabanı 'na bir MySQL veritabanının en düşük kapalı kalma süresinin nasıl gerçekleştirileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: 978312c6238315cf836cf7c26a2ee63e452d3b85
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90884603"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'na en düşük kesinti süresi geçişi
[!INCLUDE[applies-to-single-flexible-server](includes/applies-to-single-flexible-server.md)]

[Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) için yeni sunulan **sürekli eşitleme özelliğini** kullanarak, MySQL Için Azure veritabanı 'na MySQL geçişleri gerçekleştirebilirsiniz. Bu işlevsellik, uygulama tarafından tahakkuk eden kesinti süresini sınırlandırır.

## <a name="overview"></a>Genel Bakış
Azure DMS, şirket içi şirketinizin ilk yükünü MySQL için Azure veritabanı 'na uygular ve uygulama çalışmaya devam ederken yeni işlemleri sürekli olarak Azure 'a eşitler. Veriler hedef Azure tarafında tamamlandıktan sonra, uygulamayı kısa bir süre (en düşük kapalı kalma süresi) durdurur, son veri toplu iş için bekleyin (uygulamanın yeni bir trafiği almak için etkin hale gelene kadar uygulamayı durdurmanız gerekir) ve ardından Bağlantı dizenizi Azure 'a işaret etmek üzere güncelleştirin. İşiniz bittiğinde, uygulamanız Azure 'da canlı olacaktır!

:::image type="content" source="./media/howto-migrate-online/ContinuousSync.png" alt-text="Azure veritabanı geçiş hizmeti ile sürekli eşitleme":::

## <a name="next-steps"></a>Sonraki adımlar
- MySQL uygulamalarının MySQL için Azure veritabanı 'na nasıl geçirileceğini gösteren bir demo içeren [MySQL/PostgreSQL uygulamalarını Azure yönetilen hizmetine kolayca geçirin](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201).
- [DMS kullanarak MySQL Için MySQL for MySQL 'e Azure veritabanı 'Na geçiş](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)öğreticisine bakın.
