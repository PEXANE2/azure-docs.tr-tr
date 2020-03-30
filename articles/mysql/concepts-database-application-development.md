---
title: Uygulama geliştirme - MySQL için Azure Veritabanı
description: MySQL için Azure Veritabanı'na bağlanmak için uygulama kodu yazarken bir geliştiricinin izlemesi gereken tasarım hususlarını sunar
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 02ce6b00b6555f849d162b9f3b381c0ab358d712
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532850"
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>MySQL için Azure Veritabanı için uygulama geliştirme genel bakış 
Bu makalede, MySQL için Azure Veritabanı'na bağlanmak için uygulama kodu yazarken bir geliştiricinin izlemesi gereken tasarım konuları anlatılmaktadır. 

> [!TIP]
> Sunucu oluşturmayı, sunucu tabanlı bir güvenlik duvarı oluşturmayı, sunucu özelliklerini görüntülemeyi, veritabanı oluşturmayı ve workbench ve mysql.exe'yi kullanarak bağlanma ve sorgu oluşturmayı gösteren bir öğretici [için](tutorial-design-database-using-portal.md) bkz.

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Kod örneklerine bağlantılar şu anda bulabilirsiniz: [MySQL için Azure Veritabanına bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="tools"></a>Araçlar
MySQL için Azure Veritabanı, Workbench veya mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)ve diğerleri gibi MySQL ortak yönetim araçlarıyla uyumlu MySQL topluluk sürümünü kullanır. Veritabanı hizmetiyle etkileşim kurmak için Azure portalını, Azure CLI'yi ve REST API'lerini de kullanabilirsiniz.

## <a name="resource-limitations"></a>Kaynak sınırlamaları
MySQL için Azure Veritabanı, bir sunucunun kullanabileceği kaynakları iki farklı mekanizma kullanarak yönetir: 
- Kaynak Yönetimi.
- Limitlerin Uygulanması.

## <a name="security"></a>Güvenlik
MySQL için Azure Veritabanı, erişimi sınırlamak, verileri korumak, kullanıcıları ve rolleri yapılandırmak ve etkinlikleri MySQL veritabanında izlemek için kaynaklar sağlar.

## <a name="authentication"></a>Kimlik doğrulaması
MySQL için Azure Veritabanı, kullanıcıların ve oturum açmaların sunucu kimlik doğrulamasını destekler.

## <a name="resiliency"></a>Dayanıklılık
MySQL veritabanına bağlanırken geçici bir hata oluştuğunda, kodunuz aramayı yeniden denemelidir. Yeniden deneme mantığının, SQL veritabanını aynı anda yeniden denemeden önce yeniden denememesi için geri metod unu kullanmanızı öneririz.

- Kod örnekleri: Yeniden deneme mantığını gösteren kod örnekleri için bkz: [MySQL için Azure Veritabanı'na bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Bağlantıları yönetme
Veritabanı bağlantıları sınırlı bir kaynaktır, bu nedenle daha iyi performans elde etmek için MySQL veritabanınıza erişirken bağlantıların mantıklı bir şekilde kullanılmasını öneririz.
- Bağlantı havuzu veya kalıcı bağlantıları kullanarak veritabanına erişin.
- Kısa bağlantı ömrü kullanarak veritabanına erişin. 
- Eşzamanlı bağlantılardan kaynaklanan hataları yakalamak için bağlantı girişimi noktasında uygulamanızda yeniden deneme mantığını kullanın. Yeniden deneme mantığında, kısa bir gecikme ayarlayın ve ek bağlantı denemeden önce rasgele bir süre bekleyin.