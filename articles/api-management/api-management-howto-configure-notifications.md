---
title: Bildirimleri ve e-posta şablonlarını yapılandırma
titleSuffix: Azure API Management
description: Azure API Yönetimi'nde bildirimleri ve e-posta şablonlarını nasıl yapılandırılamayı öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2020
ms.author: apimpm
ms.openlocfilehash: 786a9e26003a7afb98307e0bd7fae94c42a2f00d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244075"
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma

API Yönetimi, bildirimleri belirli olaylar için yapılandırma ve bir API Yönetimi örneğinin yöneticileri ve geliştiricileri ile iletişim kurmak için kullanılan e-posta şablonlarını yapılandırma olanağı sağlar. Bu makalede, kullanılabilir olaylar için bildirimlerin nasıl yapılandırılakullanılacağı gösterilmektedir ve bu olaylar için kullanılan e-posta şablonlarını yapılandırmaya genel bir bakış sağlar.

## <a name="prerequisites"></a>Ön koşullar

Bir API Yönetimi hizmet örneğiniz yoksa, aşağıdaki hızlı başlatmayı tamamlayın: [Azure API Yönetimi örneği oluşturun.](get-started-create-service-instance.md)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="configure-notifications"></a><a name="publisher-notifications"> </a>Bildirimleri yapılandırma

1.  **API YÖNETİmİ** örneğini seçin.
2.  Kullanılabilir bildirimleri görüntülemek için **Bildirimler'i** tıklatın.

    ![Yayımcı bildirimleri][api-management-publisher-notifications]

    Aşağıdaki olaylar listesi bildirimler için yapılandırılabilir.

    -   **Abonelik istekleri (onay gerektiren)** - Belirtilen e-posta alıcıları ve kullanıcılar, onay gerektiren API ürünleri için abonelik istekleri hakkında e-posta bildirimleri alır.
    -   **Yeni abonelikler** - Belirtilen e-posta alıcıları ve kullanıcılar yeni API ürün abonelikleri hakkında e-posta bildirimleri alır.
    -   **Uygulama galerisi istekleri** - Belirtilen e-posta alıcıları ve kullanıcılar, yeni başvurular uygulama galerisine gönderildiğinde e-posta bildirimleri alır.
    -   **BCC** - Belirtilen e-posta alıcıları ve kullanıcılar geliştiricilere gönderilen tüm e-postaların e-posta kör karbon kopyalarını alırsınız.
    -   **Yeni sorun veya yorum** - Geliştirici portalına yeni bir sorun veya yorum gönderildiğinde belirtilen e-posta alıcıları ve kullanıcılar e-posta bildirimleri alır.
    -   **Hesap iletisini kapat** - Belirtilen e-posta alıcıları ve kullanıcılar, bir hesap kapatıldığında e-posta bildirimleri alır.
    -   **Yaklaşan abonelik kotası sınırı** - Abonelik kullanımı kullanım kotasına yaklaştığında aşağıdaki e-posta alıcıları ve kullanıcılar e-posta bildirimleri alır.

        > [!NOTE]
        > Bildirimler [yalnızca abonelik](api-management-access-restriction-policies.md#SetUsageQuota) ilkesi tarafından kota tarafından tetiklenir. [Anahtar ilkesine göre kota](api-management-access-restriction-policies.md#SetUsageQuotaByKey) bildirimler oluşturmaz.

    Her olay için, e-posta adresi metin kutusunu kullanarak e-posta alıcıları belirtebilir veya bir listeden kullanıcıları seçebilirsiniz.

3.  Bildirilecek e-posta adreslerini belirtmek için bunları e-posta adresi metin kutusuna girin. Birden çok e-posta adresiniz varsa, virgül kullanarak ayırın.

    ![Bildirim alıcıları][api-management-email-addresses]

4.  **Ekle**’ye basın.

## <a name="configure-notification-templates"></a><a name="email-templates"> </a>Bildirim şablonlarını yapılandırma

API Yönetimi, hizmeti yönetme ve kullanma süresince gönderilen e-posta iletileri için bildirim şablonları sağlar. Aşağıdaki e-posta şablonları sağlanır.

-   Başvuru galerisi gönderimi onaylandı
-   Geliştirici veda mektubu
-   Geliştirici kota sınırı bildirimi yaklaşıyor
-   Kullanıcıyı davet et
-   Bir soruna yeni yorum eklendi
-   Yeni sayı alındı
-   Yeni abonelik etkinleştirildi
-   Abonelik yenilenen onay
-   Abonelik isteği reddediyor
-   Abonelik isteği alındı

Bu şablonlar istenenildiği gibi değiştirilebilir.

API Yönetimi örneğiniz için e-posta şablonlarını görüntülemek ve yapılandırmak için **Bildirimler şablonlarını**tıklatın.

![E-posta şablonları][api-management-email-templates]

Her e-posta şablonunda düz metinde bir konu ve HTML biçiminde bir gövde tanımı vardır. Her öğe islenebildiği gibi özelleştirilebilir.

![E-posta şablonu düzenleyicisi][api-management-email-template]

**Parametreler** listesi, konu veya gövdeye eklendiğinde, e-posta gönderildiğinde belirlenen değerin değiştirileceğinigösteren parametrelerin bir listesini içerir. Bir parametre eklemek için imleci parametrenin gitmesini istediğiniz yere yerleştirin ve parametre adının solundaki oku tıklatın.

> [!NOTE]
> Parametreler, bir testi önizleme veya gönderirken gerçek değerlerle değiştirilmez.

Değişiklikleri e-posta şablonuna kaydetmek için **Kaydet'i**tıklatın veya değişiklikleri iptal etmek için **At'ı**tıklatın.

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png
[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png
[configure publisher notifications]: #publisher-notifications
[configure email templates]: #email-templates
[how to create and use groups]: api-management-howto-create-groups.md
[how to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[get started with azure api management]: get-started-create-service-instance.md
[create an api management service instance]: get-started-create-service-instance.md
