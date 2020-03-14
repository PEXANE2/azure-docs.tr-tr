---
title: TLS yapılandırması-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı tek sunuculu Azure portal kullanarak TLS yapılandırması ayarlamayı öğrenin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 049b30689c86881ce89d4fbcd54175ee1ee5497e
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79375115"
---
# <a name="configuring-tls-settings-in-azure-database-for-postgresql---single-server-using-azure-portal"></a>PostgreSQL için Azure veritabanı 'nda TLS ayarlarını yapılandırma-Azure portal kullanarak tek sunucu

Bu makalede, bir PostgreSQL için Azure veritabanı-tek sunucu, tüm gelen bağlantılar için en düşük TLS sürümünü zorlamak üzere nasıl yapılandırılabileceğinizi açıklar. Bu, ağ güvenliğinin artırılmasına yardımcı olur.

Müşteriler artık, PostgreSQL için Azure veritabanı 'na bağlanmak üzere TLS sürümünü zorunlu kılabilir-tek sunucu. Müşteriler artık veritabanı sunucuları için en düşük TLS sürümünü ayarlamayı tercih etmektedir. Örneğin, minimum TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1,2 kullanarak istemcilerden gelen bağlantılara izin vertiğinizi ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedildiğini belirtir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda tamamlanması gerekir:

* [PostgreSQL Için Azure veritabanı](quickstart-create-server-database-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı için TLS yapılandırmasını ayarlama-tek sunucu

PostgreSQL tek sunuculu en düşük TLS sürümünü ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), mevcut PostgreSQL Için Azure veritabanı-tek sunucu ' yı seçin.

1.  PostgreSQL için Azure veritabanı-tek sunucu sayfasında, **Ayarlar**altında bağlantı **güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **En düşük TLS**sürümü ' nde, PostgreSQL tek sunucunuz için TLS 1,2 ' den düşük olan bağlantıları reddetmek için **1,2** ' ı seçin.

    ![PostgreSQL için Azure veritabanı tek sunuculu TLS yapılandırması](./media/howto-tls-configurations/setting-tls-value.png)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    ![PostgreSQL için Azure veritabanı-tek sunuculu TLS yapılandırması başarılı](./media/howto-tls-configurations/setting-tls-value-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ölçümler üzerinde uyarılar oluşturma](howto-alert-on-metric.md)hakkında bilgi edinin.
