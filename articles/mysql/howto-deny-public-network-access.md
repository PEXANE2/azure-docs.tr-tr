---
title: Genel Ağ Erişimini Reddet - Azure portalı - MySQL için Azure Veritabanı
description: MySQL için Azure Veritabanınız için Azure portalı kullanarak Genel Ağ Erişimini Reddet'i nasıl yapılandıracaklarını öğrenin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: b5f93f3a3583900810ca75f925c6a88df9102652
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79374959"
---
# <a name="deny-public-network-access-in-azure-database-for-mysql-using-azure-portal"></a>Azure portalını kullanarak MySQL için Azure Veritabanı'nda Genel Ağ Erişimini Reddetme

Bu makalede, MySQL sunucusu için bir Azure Veritabanını tüm genel yapılandırmaları reddalacak şekilde nasıl yapılandırabileceğiniz ve ağ güvenliğini daha da artırmak için yalnızca özel uç noktalar üzerinden bağlantılara izin vermek üzere nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:

* [MySQL için Azure Veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Genel Ağ Erişimini Reddet'i Ayarla

MySQL sunucusu Deny Public Network Access'i ayarlamak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com/)MySQL sunucusu için mevcut Azure Veritabanınızı seçin.

1. MySQL sunucu sayfasında, **Ayarlar**altında bağlantı güvenliği yapılandırma sayfasını açmak için **Bağlantı güvenliği'ni** tıklatın.

1. **Genel Ağ Erişimini Reddet'te,** MySQL sunucunuz için genel erişimi reddetmek için **Evet'i** seçin.

    ![MySQL Ağ erişimini reddet için Azure Veritabanı](./media/howto-deny-public-network-access/setting-deny-public-network-access.PNG)

1. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

1. Bildirim, bağlantı güvenlik ayarının başarıyla etkinleştirildiğini onaylar.

    ![MySQL Ağ erişim başarısını reddet için Azure Veritabanı](./media/howto-deny-public-network-access/setting-deny-public-network-access-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-on-metric.md)hakkında bilgi edinin.