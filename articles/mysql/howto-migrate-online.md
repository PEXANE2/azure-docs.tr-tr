---
title: En az kapalı kalma süresi geçişi - MySQL için Azure Veritabanı
description: Bu makalede, Azure Veritabanı Geçiş Hizmeti'ni kullanarak MySQL veritabanının MySQL veritabanıiçin Azure Veritabanı'na en az kesintiyle nasıl gerçekleştirilebildiğini açıklamaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: df818fa4106aec341607d8142b2a672699b8e9d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063354"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanına en az kapalı kalma süresi geçişi
[Azure Veritabanı Geçiş Hizmeti](https://aka.ms/get-dms) (DMS) için yeni tanıtılan sürekli **eşitleme özelliğini** kullanarak MySQL için Azure Veritabanı'na mySQL geçişlerini en az kapalı kalma süresiyle gerçekleştirebilirsiniz. Bu işlevsellik, uygulamatarafından tahakkuk eden kapalı kalma süresini sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, mysql için şirket içi ilk yüklemenizi Azure Veritabanı'na gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemleri sürekli olarak Azure ile eşitler. Veriler hedef Azure tarafında yakaladıktan sonra, uygulamayı kısa bir süre (minimum kapalı kalma süresi) durdurursunuz, son veri toplusunu beklersiniz (uygulama yeni bir trafik almak için etkili bir şekilde kullanılamayana kadar uygulamayı durdurun) hedefte yukarı ve ardından bağlantı dizenizi Azure'a işaret etmek için güncelleştirin. İşinizi bitirdiğinizde, uygulamanız Azure'da canlı olacak!

![Azure Veritabanı Geçiş Hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu [kolayca MySQL/PostgreSQL uygulamalarını MySQL/MySQL](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201)için Azure Veritabanı'na nasıl geçirildirilebildiğini gösteren bir demo içeren Azure yönetilen hizmetine geçirin.
- [DMS kullanarak MySQL çevrimiçi için MySQL Veritabanına Geçir](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)in öğreticisine bakın.
