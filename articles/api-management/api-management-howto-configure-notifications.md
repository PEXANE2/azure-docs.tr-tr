---
title: Bildirimleri ve e-posta şablonlarını yapılandırma
titleSuffix: Azure API Management
description: Azure API Management bildirimleri ve e-posta şablonlarını yapılandırmayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79244075"
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Azure API Management’te bildirimleri ve e-posta şablonlarını yapılandırma

API Management belirli olaylara yönelik bildirimleri yapılandırma ve bir API Management örneğinin yöneticileri ve geliştiricileri ile iletişim kurmak için kullanılan e-posta şablonlarını yapılandırma olanağı sağlar. Bu makalede, kullanılabilir olaylar için bildirimlerin nasıl yapılandırılacağı gösterilir ve bu olaylar için kullanılan e-posta şablonlarını yapılandırmaya genel bir bakış sağlanır.

## <a name="prerequisites"></a>Ön koşullar

Bir API Management hizmet örneğiniz yoksa, aşağıdaki hızlı başlangıcı izleyin: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="configure-notifications"></a><a name="publisher-notifications"> </a>Bildirimleri yapılandırma

1.  **API Management** örneğinizi seçin.
2.  Kullanılabilir bildirimleri görüntülemek için **Bildirimler** ' e tıklayın.

    ![Yayımcı bildirimleri][api-management-publisher-notifications]

    Aşağıdaki olaylar listesi bildirimler için yapılandırılabilir.

    -   **Abonelik istekleri (onay gerektiren)** -belirtilen e-posta alıcıları ve kullanıcılar onay gerektiren API ürünleri için abonelik istekleri hakkında e-posta bildirimleri alır.
    -   **Yeni abonelikler** -belirtilen e-posta alıcıları ve KULLANıCıLAR yeni API ürün abonelikleri hakkında e-posta bildirimleri alacaktır.
    -   **Uygulama Galerisi istekleri** -belirtilen e-posta alıcıları ve kullanıcılar, uygulama galerisine yeni uygulamalar gönderildiğinde e-posta bildirimleri alır.
    -   **Gizli** -belirtilen e-posta alıcıları ve kullanıcılar, geliştiricilere gönderilen tüm e-postaların e-posta tarafından gizli kopyalarını alırlar.
    -   **Yeni sorun veya açıklama** -Geliştirici Portalında yeni bir sorun veya yorum gönderildiğinde, belirtilen e-posta alıcıları ve kullanıcılar e-posta bildirimleri alır.
    -   **Hesap Iletisini kapat** -belirtilen e-posta alıcıları ve kullanıcılar bir hesap kapatıldığında e-posta bildirimleri alır.
    -   **Abonelik kotası sınırına yaklaşırken** , abonelik kullanımı kullanım kotasına yaklaşdığında aşağıdaki e-posta alıcıları ve kullanıcılar e-posta bildirimleri alır.

        > [!NOTE]
        > Bildirimler yalnızca [abonelik ilkesiyle kota](api-management-access-restriction-policies.md#SetUsageQuota) tarafından tetiklenir. [Anahtar ilkesine göre kota](api-management-access-restriction-policies.md#SetUsageQuotaByKey) bildirim oluşturmaz.

    Her olay için e-posta adresi metin kutusunu kullanarak e-posta alıcıları belirtebilir veya listeden Kullanıcı seçebilirsiniz.

3.  Bildirilecek e-posta adreslerini belirtmek için, e-posta adresi metin kutusuna bunları girin. Birden çok e-posta adresiniz varsa, bunları virgül kullanarak ayırın.

    ![Bildirim alıcıları][api-management-email-addresses]

4.  **Ekle**’ye basın.

## <a name="configure-notification-templates"></a><a name="email-templates"> </a>Bildirim şablonlarını yapılandırma

API Management, hizmeti yönetme ve kullanma konusunda gönderilen e-posta iletileri için bildirim şablonları sağlar. Aşağıdaki e-posta şablonları verilmiştir.

-   Uygulama Galerisi gönderimi onaylandı
-   Geliştirici Farewell harfi
-   Geliştirici kota sınırı bildirimle yaklaşılıyor
-   Kullanıcıyı davet et
-   Bir soruna yeni açıklama eklendi
-   Yeni sorun alındı
-   Yeni abonelik etkinleştirildi
-   Abonelik yenilendi onayı
-   Abonelik isteği reddederse
-   Abonelik isteği alındı

Bu şablonlar istenen şekilde değiştirilebilir.

API Management örneğiniz için e-posta şablonlarını görüntülemek ve yapılandırmak için, **Bildirimler şablonlar**' a tıklayın.

![E-posta şablonları][api-management-email-templates]

Her e-posta şablonunun bir konusu düz metin ve HTML biçiminde bir gövde tanımı vardır. Her öğe istenildiği gibi özelleştirilebilir.

![E-posta şablonu Düzenleyicisi][api-management-email-template]

**Parametreler** listesi, e-posta gönderildiğinde belirlenen değerin yerine, konuya veya gövdeye eklendiğinde parametre listesini içerir. Bir parametre eklemek için, imleci istediğiniz yere yerleştirip parametre adının solundaki oka tıklayın.

> [!NOTE]
> Bir testi önizlemede veya gönderirken parametreler gerçek değerlerle değiştirilmez.

E-posta şablonunda yapılan değişiklikleri kaydetmek için **Kaydet**' e tıklayın veya değişiklikleri iptal etmek için **at**' a tıklayın.

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
