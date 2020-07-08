---
title: TLS yapılandırması-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı tek sunuculu Azure portal kullanarak TLS yapılandırması ayarlamayı öğrenin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: ed5d46bbb5a520ee28013210ba44045ad59c2cc2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84705719"
---
# <a name="configuring-tls-settings-in-azure-database-for-postgresql-single---server-using-azure-portal"></a>Azure portal kullanarak PostgreSQL için Azure veritabanı 'nda TLS ayarlarını yapılandırma

Bu makalede, bir PostgreSQL için Azure veritabanı 'nın bağlantılara izin verilen en düşük TLS sürümünü zorlamak ve daha düşük TLS sürümüne sahip tüm bağlantıları reddetmek için nasıl yapılandırabileceğiniz açıklanmaktadır.

PostgreSQL için Azure veritabanı 'na bağlanmak üzere TLS sürümü uygulayabilirsiniz. Müşteriler artık veritabanı sunucuları için en düşük TLS sürümünü ayarlamayı tercih etmektedir. Örneğin, minimum TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Bunun yerine, minimum TLS sürümünün 1.2 + olarak ayarlanması, yalnızca TLS 1,2 kullanarak istemcilerden gelen bağlantılara izin vertiğinizi ve TLS 1,1 1,0 ile tüm bağlantıların reddedildiğini kabul edilmekte olduğunu gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

* [PostgreSQL Için Azure veritabanı](quickstart-create-server-database-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı için TLS yapılandırmasını ayarlama-tek sunucu

PostgreSQL en düşük TLS sürümünü ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), PostgreSQL Için mevcut Azure veritabanınızı seçin.

1.  PostgreSQL için Azure veritabanı-tek sunucu sayfasında, **Ayarlar**altında bağlantı **güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **En düşük TLS**sürümü ' nde, PostgreSQL tek sunucunuz için TLS 1,2 ' den düşük olan bağlantıları reddetmek için **1,2** ' ı seçin.

    ![PostgreSQL için Azure veritabanı tek sunuculu TLS yapılandırması](./media/howto-tls-configurations/setting-tls-value.png)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    ![PostgreSQL için Azure veritabanı-tek sunuculu TLS yapılandırması başarılı](./media/howto-tls-configurations/setting-tls-value-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarı oluşturma](howto-alert-on-metric.md) hakkında bilgi edinin