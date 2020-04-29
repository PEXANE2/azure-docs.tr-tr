---
title: Ortak ağ erişimini reddetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı tek sunuculu Azure portal kullanarak genel ağ erişimini reddetme hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 4dff2321414721dbd415b468e59aea0ab4b3acee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79375128"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>Azure portal kullanarak PostgreSQL için Azure veritabanı 'nda genel ağ erişimini reddetme

Bu makalede, bir PostgreSQL için Azure veritabanı 'nı tüm genel yapılandırmaların reddedecek ve yalnızca özel uç noktalar aracılığıyla ağ güvenliğini artırmak üzere yalnızca bağlantılara izin verecek şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

* [PostgreSQL Için Azure veritabanı tek sunucu](quickstart-create-PostgreSQL Single server-server-database-using-azure-portal.md)

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