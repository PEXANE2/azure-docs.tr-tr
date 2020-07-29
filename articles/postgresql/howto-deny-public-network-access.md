---
title: Ortak ağ erişimini reddetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı tek sunuculu Azure portal kullanarak genel ağ erişimini reddetme hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: 01a256e17b1101782eaee9bebd85f5e7093773d3
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287783"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>Azure portal kullanarak PostgreSQL için Azure veritabanı 'nda genel ağ erişimini reddetme

Bu makalede, bir PostgreSQL için Azure veritabanı 'nı tüm genel yapılandırmaların reddedecek ve yalnızca özel uç noktalar aracılığıyla ağ güvenliğini artırmak üzere yalnızca bağlantılara izin verecek şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

* [PostgreSQL Için Azure veritabanı tek sunucu](quickstart-create-server-database-portal.md)

## <a name="set-deny-public-network-access"></a>Ortak ağ erişimini reddet ayarla

PostgreSQL için tek sunuculu ortak ağ erişimini reddet ' i ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), mevcut PostgreSQL Için Azure veritabanı tek sunucu ' yı seçin.

1. PostgreSQL tek sunucusu sayfasında, **Ayarlar**altında bağlantı **güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **Ortak ağ erişimini engelle**' de, PostgreSQL tek sunucunuz için genel erişimi Reddet ' i etkinleştirmek için **Evet** ' i seçin.

    ![PostgreSQL için Azure veritabanı tek sunucu ağ erişimini reddet](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    ![PostgreSQL için Azure veritabanı tek sunuculu ağ erişimini reddetme başarılı](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-on-metric.md)hakkında bilgi edinin.
