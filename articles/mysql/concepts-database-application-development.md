---
title: Uygulama geliştirme-MySQL için Azure veritabanı
description: Bir geliştiricinin MySQL için Azure veritabanı 'na bağlanmak üzere uygulama kodu yazarken izlenmesi gereken tasarım konularını tanıtır
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 02ce6b00b6555f849d162b9f3b381c0ab358d712
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79532850"
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'na uygulama geliştirmeye genel bakış 
Bu makalede, bir geliştiricinin MySQL için Azure veritabanı 'na bağlanmak üzere uygulama kodu yazarken izlenmesi gereken tasarım konuları ele alınmaktadır. 

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, veritabanı oluşturma ve çalışma ekranı ve MySQL. exe kullanarak bağlanma ve sorgulama işlemlerinin nasıl yapılacağını gösteren bir öğretici için bkz. [MySQL için Ilk Azure veritabanı veritabanınızı tasarlama](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Şu adreste kod örneklerinin bağlantılarını bulabilirsiniz: [MySQL Için Azure veritabanı 'na bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="tools"></a>Araçlar
MySQL için Azure veritabanı MySQL Community sürümünü kullanarak MySQL. exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)gibi MySQL veya MySQL gibi MySQL ortak yönetim araçlarıyla uyumludur. Veritabanı hizmetiyle etkileşim kurmak için Azure portal, Azure CLı ve REST API 'Lerini de kullanabilirsiniz.

## <a name="resource-limitations"></a>Kaynak sınırlamaları
MySQL için Azure veritabanı, iki farklı mekanizma kullanarak bir sunucu için kullanılabilir kaynakları yönetir: 
- Kaynakları Idare edin.
- Limitlerin zorlanması.

## <a name="security"></a>Güvenlik
MySQL için Azure veritabanı, erişimi sınırlandırma, verileri koruma, kullanıcıları ve rolleri yapılandırma ve bir MySQL veritabanında etkinlikleri izleme için kaynaklar sağlar.

## <a name="authentication"></a>Kimlik Doğrulaması
MySQL için Azure veritabanı, kullanıcıların ve oturum açmaların sunucu kimlik doğrulamasını destekler.

## <a name="resiliency"></a>Dayanıklılık
Bir MySQL veritabanına bağlanırken geçici bir hata oluştuğunda, kodunuzun çağrıyı yeniden denemesi gerekir. Yeniden deneme mantığının, birden çok istemcisini aynı anda yeniden denemeye yönelik SQL veritabanı 'nı daha fazla bir şekilde kapatması için yeniden deneme mantığını kullanmanızı öneririz.

- Kod örnekleri: yeniden deneme mantığını gösteren kod örnekleri Için, istediğiniz dilin örneklerine bakın: [MySQL Için Azure veritabanı 'na bağlanmak için kullanılan bağlantı kitaplıkları](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Bağlantıları yönetme
Veritabanı bağlantıları sınırlı bir kaynaktır; bu nedenle, daha iyi performans elde etmek için MySQL veritabanınıza erişirken bağlantıların daha kolay kullanılmasını öneririz.
- Bağlantı havuzu veya kalıcı bağlantılar kullanarak veritabanına erişin.
- Kısa bağlantı ömrü kullanarak veritabanına erişin. 
- Uygulamanızda yeniden deneme mantığını kullanarak, eşzamanlı bağlantılardan kaynaklanan hataların sonucunu yakalamak için bağlantı girişimi izin verilen üst sınıra ulaştı. Yeniden dene mantığındaki kısa bir gecikme ayarlayın ve ek bağlantı denemesinden önce rastgele bir süre bekleyin.