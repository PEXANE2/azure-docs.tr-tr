---
title: Ortak ağ erişimini reddetme-Azure portal-MySQL için Azure veritabanı
description: MySQL için Azure veritabanınız için Azure portal kullanarak genel ağ erişimini reddetme hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: a98ab9ea347ba4d9ec53c80626f97b429e083cb1
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93242390"
---
# <a name="deny-public-network-access-in-azure-database-for-mysql-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda genel ağ erişimini reddetme

Bu makalede, bir MySQL için Azure veritabanı sunucusunu tüm genel yapılandırmaların erişimini engelleyecek şekilde nasıl yapılandırabileceğiniz ve ağ güvenliğini daha da geliştirmek için özel uç noktalar aracılığıyla yalnızca bağlantılara izin veren açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

* [MySQL Için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Ortak ağ erişimini reddet ayarla

MySQL Server 'ın ortak ağ erişimini reddetme Iznini ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), var olan MySQL Için Azure veritabanı sunucunuzu seçin.

1. MySQL sunucusu sayfasında, **Ayarlar** altında **bağlantı güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **Ortak ağ erişimini engelle** ' de, MySQL sunucunuz için genel erişimi Reddet ' i etkinleştirmek için **Evet** ' i seçin.

    :::image type="content" source="./media/howto-deny-public-network-access/setting-deny-public-network-access.PNG" alt-text="MySQL için Azure veritabanı ağ erişimini reddetme":::

1. Değişiklikleri kaydetmek için **Kaydet** ’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    :::image type="content" source="./media/howto-deny-public-network-access/setting-deny-public-network-access-success.png" alt-text="MySQL için Azure veritabanı ağ erişimini reddetme başarısı":::

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-on-metric.md)hakkında bilgi edinin.