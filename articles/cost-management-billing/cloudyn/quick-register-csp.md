---
title: Azure’da Cloudyn ile CSP İş Ortağı bilgilerini kullanarak kaydolma
description: Müşterilerini Cloudyn portalına eklemek amacıyla iş ortakları tarafından kullanılan kayıt süreci hakkındaki ayrıntıları öğrenin.
author: bandersmsft
ms.author: banders
ms.date: 10/23/2020
ms.topic: quickstart
ms.custom: seodec18
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.reviewer: benshy
ROBOTS: NOINDEX
ms.openlocfilehash: 95d8a425314ad1c968915085b35ca89aa3f62711
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92543330"
---
# <a name="register-with-the-csp-partner-program-and-view-cost-data"></a>CSP İş Ortağı programına kaydolun ve maliyetle ilgili verileri görüntüleyin

CSP iş ortağı ve kayıtlı Cloudyn kullanıcısı olarak, Cloudyn’deki bulut harcamalarınızı görüntüleyip analiz edebilirsiniz. [Azure Maliyet Yönetimi, müşterilerini bir Microsoft Müşteri Sözleşmesi'ne eklemiş ve bir Azure Planı satın almış olan doğrudan iş ortakları tarafından yerel olarak kullanılabilir](../costs/get-started-partners.md).

Kaydınız Cloudyn portalına erişim sağlar. Bu hızlı başlangıçta bir Cloudyn deneme aboneliği oluşturmak ve Cloudyn portalında oturum açmak için gereken kayıt işlemleri açıklanmaktadır.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="configure-indirect-csp-access-in-cloudyn"></a>Cloudyn’de dolaylı CSP erişimini yapılandırma

İş Ortağı Merkezi API’sine, varsayılan olarak yalnızca CSP’ler erişebilir. Ancak, doğrudan bir CSP sağlayıcısı, Cloudyn’ndeki varlık gruplarını kullanarak dolaylı CSP müşterileri veya iş ortakları için erişim yapılandırabilir.

Dolaylı CSP müşterileri veya iş ortakları için erişimi etkinleştirmek amacıyla, Cloudyn varlık gruplarını kullanarak dolaylı CSP verilerini gruplandırmak üzere aşağıdaki adımları izleyin. Sonra, varlık gruplarına uygun kullanıcı izinlerini atayacağız.

1. [Varlık oluşturma ](tutorial-user-access.md#create-and-manage-entities) konusundaki bilgilerle varlık grubu oluşturun.
2. [Maliyet Varlıklarına abonelik atama](https://www.youtube.com/watch?v=d9uTWSdoQYo) konusundaki adımları izleyin. Dolaylı CSP müşterisinin hesabını ve önceden oluşturduğunuz varlıkla ilgili Azure aboneliğini ilişkilendirin.
3. Yönetici erişimine sahip bir kullanıcı hesabı oluşturmak için [Yönetici erişimine sahip bir kullanıcı oluşturma](tutorial-user-access.md#create-a-user-with-admin-access) konusuna göz atın. Sonra, kullanıcı hesabının dolaylı hesap için daha önce oluşturduğunuz belirli varlıklara erişimi olduğundan emin olun.

Dolaylı CSP iş ortakları, Cloudyn portalında, kendileri için oluşturduğunuz hesapları kullanarak oturum açar.


[!INCLUDE [cost-management-create-account-view-data](../../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cloudyn’e kaydolmak için CSP bilgilerinizi kullandınız. Ayrıca Cloudyn portalında oturum açarak maliyet verilerini görüntülemeye başladınız. Cloudyn hakkında daha fazla bilgi için Cloudyn öğreticisine geçin.

> [!div class="nextstepaction"]
> [Kullanımı ve maliyetleri gözden geçirme](tutorial-review-usage.md)