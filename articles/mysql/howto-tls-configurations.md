---
title: TLS yapılandırması-Azure portal-MySQL için Azure veritabanı
description: MySQL için Azure veritabanınız için Azure portal kullanarak TLS yapılandırması ayarlamayı öğrenin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 495fea21c4fca12cd1ec6c1ea45105178d72fe1b
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79375297"
---
# <a name="configuring-tls-settings-in-azure-database-for-mysql-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda TLS ayarlarını yapılandırma

Bu makalede, bir MySQL için Azure veritabanı sunucusunu, daha düşük TLS sürümüne sahip tüm bağlantıları ve bu sayede ağ güvenliğini artırmasını sağlamak üzere nasıl yapılandırabileceğinizi açıklar.

Müşteriler artık, MySQL için Azure veritabanı 'na bağlanmak üzere TLS sürümü zorlama özelliğine sahiptir. Müşteriler artık veritabanı sunucuları için en düşük TLS sürümünü ayarlamayı tercih etmektedir. Örneğin, bu en düşük TLS sürümünü 1,0 olarak ayarlamak, TLS 1.0, 1.1 ve 1,2 kullanarak bağlanan istemcilere izin vermeyeceğiniz anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1,2 kullanarak bağlanan istemcilere izin vermek ve TLS 1,0 ve TLS 1,1 ile gelen tüm bağlantıları reddedilecek anlamına gelir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda tamamlanması gerekir:

* [MySQL Için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için TLS yapılandırması ayarlama

MySQL Server en düşük TLS sürümünü ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), var olan MySQL Için Azure veritabanı sunucunuzu seçin.

1. MySQL sunucusu sayfasında, **Ayarlar**altında **bağlantı güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **En düşük TLS**sürümü ' nde, MySQL sunucunuz için TLS 1,2 ' den düşük olan bağlantıları reddetmek için **1,2** ' ı seçin.

    ![MySQL için Azure veritabanı TLS yapılandırması](./media/howto-tls-configurations/setting-tls-value.png)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    ![MySQL için Azure veritabanı TLS yapılandırması başarılı](./media/howto-tls-configurations/setting-tls-value-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-on-metric.md)hakkında bilgi edinin.
