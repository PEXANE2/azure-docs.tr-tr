---
title: Genel Ağ Erişimini Reddet - Azure portalı - PostgreSQL için Azure Veritabanı - Tek sunucu
description: PostgreSQL Tek sunucu için Azure Veritabanınız için Azure portalı kullanarak Genel Ağ Erişimini Reddet'i nasıl yapılandırabilirsiniz öğrenin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 4dff2321414721dbd415b468e59aea0ab4b3acee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79375128"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>Azure portalını kullanarak PostgreSQL Tek sunucu için Azure Veritabanında Genel Ağ Erişimini Reddetme

Bu makalede, postgreSQL Tek sunucu için bir Azure Veritabanını tüm genel yapılandırmaları reddetmek ve ağ güvenliğini daha da artırmak için yalnızca özel uç noktalar üzerinden bağlantılara izin verecek şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:

* [PostgreSQL Tek sunucu için Azure Veritabanı](quickstart-create-PostgreSQL Single server-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Genel Ağ Erişimini Reddet'i Ayarla

PostgreSQL Tek sunucu Reddet Public Network Access ayarlamak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/)PostgreSQL Tek sunucu için mevcut Azure Veritabanınızı seçin.

1. PostgreSQL Tek sunucu sayfasında, **Ayarlar**altında bağlantı güvenliği yapılandırma sayfasını açmak için **Bağlantı güvenliği'ni** tıklatın.

1. **Genel Ağ Erişimini Reddet'te,** PostgreSQL Tek sunucunuz için genel erişimi reddetmek için **Evet'i** seçin.

    ![PostgreSQL Tek sunucu Ağ erişimini reddet için Azure Veritabanı](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenlik ayarının başarıyla etkinleştirildiğini onaylar.

    ![PostgreSQL Tek sunucu ağ erişim başarısını reddet için Azure Veritabanı](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-on-metric.md)hakkında bilgi edinin.