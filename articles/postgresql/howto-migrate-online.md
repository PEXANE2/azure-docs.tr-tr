---
title: PostgreSQL için Azure Veritabanına minimum kapalı kalma süresi geçişi - Tek Sunucu
description: Bu makalede, Bir PostgreSQL veritabanının Azure Veritabanı için Azure Veritabanı'na en az kesintiyle geçişinin Nasıl Yapılacağını, Azure Veritabanı Geçiş Hizmeti'ni kullanarak PostgreSQL - Single Server için Azure Veritabanı'na nasıl yapılacağını açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 93cd390889c023adf1c30a8470e1c2298598439e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65067522"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanına minimum kapalı kalma süresi geçişi - Tek Sunucu
[Azure Veritabanı Geçiş Hizmeti](https://aka.ms/get-dms) (DMS) için yeni tanıtılan sürekli **eşitleme özelliğini** kullanarak PostgreSQL için Azure Veritabanı'na en az kapalı kalma süresiyle PostgreSQL geçişleri gerçekleştirebilirsiniz. Bu işlevsellik, uygulamatarafından tahakkuk eden kapalı kalma süresini sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, şirket içi bir yüklemenizi PostgreSQL için Azure Veritabanı'na gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemleri sürekli olarak Azure ile eşitler. Veriler hedef Azure tarafında yakaladıktan sonra, uygulamayı kısa bir süre (minimum kapalı kalma süresi) durdurursunuz, son veri toplusunu beklersiniz (uygulama yeni bir trafik almak için etkili bir şekilde kullanılamayana kadar uygulamayı durdurun) hedefte yukarı ve ardından bağlantı dizenizi Azure'a işaret etmek için güncelleştirin. İşinizi bitirdiğinizde, uygulamanız Azure'da canlı olacak!

![Azure Veritabanı Geçiş Hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL uygulamalarını PostgreSQL için Azure Veritabanı'na nasıl geçirilen gösteren bir demo içeren Microsoft Azure ile video [Uygulama Modernizasyonunu](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102)görüntüleyin.
- [DMS kullanarak PostgreSQL sonrası çevrimiçi için PostgreSQL'i Azure Veritabanına Geçir](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online)başlıklı öğreticiye bakın.
