---
title: Genel Ağ Erişimini Reddet - Azure portalı - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanınız için Azure portalı kullanarak Genel Ağ Erişimini Reddet'i nasıl yapılandıracaklarını öğrenin
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 87b6033e486e9009573436628d0183c8a022aced
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79375245"
---
# <a name="deny-public-network-access-in-azure-database-for-mariadb-using-azure-portal"></a>Azure portalını kullanarak MariaDB için Azure Veritabanında Genel Ağ Erişimini Reddetme

Bu makalede, MariaDB sunucusu için bir Azure Veritabanını tüm genel yapılandırmaları reddetmek ve ağ güvenliğini daha da artırmak için yalnızca özel uç noktalar üzerinden bağlantılara izin verecek şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:

* [MariaDB için Bir Azure Veritabanı](quickstart-create-MariaDB-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Genel Ağ Erişimini Reddet'i Ayarla

MariaDB sunucusu Nungenel Ağ Erişimini Reddet'i ayarlamak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/)MariaDB sunucusu için mevcut Azure Veritabanınızı seçin.

1. MariaDB sunucu sayfasında, **Ayarlar**altında bağlantı güvenliği yapılandırma sayfasını açmak için **Bağlantı güvenliğini** tıklatın.

1. Ortak Ağ Erişimini Reddet'te, MariaDB sunucunuz için genel erişimi reddetmek için **Evet'i** seçin.

    ![MariaDB Ağ erişimini reddet için Azure Veritabanı](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenlik ayarının başarıyla etkinleştirildiğini onaylar.

    ![MariaDB Ağ erişim başarısını reddet için Azure Veritabanı](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-metric.md)hakkında bilgi edinin.