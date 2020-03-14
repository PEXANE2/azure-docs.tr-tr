---
title: TLS yapılandırması-Azure portal-MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanınız için Azure portal kullanarak TLS yapılandırması ayarlamayı öğrenin
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 063fc82131a66012c7329f590c0fdf2ce4d4da79
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79375011"
---
# <a name="configuring-tls-settings-in-azure-database-for-mariadb-using-azure-portal"></a>Azure portal kullanarak MariaDB için Azure veritabanı 'nda TLS ayarlarını yapılandırma

Bu makalede, bir MariaDB sunucusu için Azure veritabanı 'nın, daha düşük TLS sürümüne sahip tüm bağlantıları ve bu sayede ağ güvenliğini artırmasını sağlamak üzere bağlantılarının nasıl yapılandırılacağı açıklanır.

Müşteriler artık, veritabanı sunucuları için en düşük TLS sürümünü ayarlayarak MariaDB için Azure veritabanı 'na bağlanmak üzere TLS sürümünü zorunlu kılabilir. Örneğin, minimum TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1,2 kullanarak istemcilerden gelen bağlantılara izin vertiğinizi ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedildiğini belirtir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda tamamlanması gerekir:

* [MariaDB Için Azure veritabanı](quickstart-create-mariaDB-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı için TLS yapılandırması ayarlama

MariaDB sunucusu en düşük TLS sürümünü ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), var olan MariaDB sunucusu Için Azure veritabanınızı seçin.

1. MariaDB sunucusu sayfasında, **Ayarlar**altında **bağlantı güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **En düşük TLS sürümü**' nde, MariaDB sunucunuz için TLS 1,2 ' den düşük olan bağlantıları reddetmek için **1,2** ' ı seçin.

    ![MariaDB için Azure veritabanı TLS yapılandırması](./media/howto-tls-configurations/tls-configurations.png)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    ![MariaDB için Azure veritabanı TLS yapılandırması başarılı](./media/howto-tls-configurations/tls-configurations-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-metric.md)hakkında bilgi edinin.
