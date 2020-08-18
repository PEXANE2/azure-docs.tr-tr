---
title: Azure Güvenlik Merkezi 'nde güvenlik iletişim bilgilerini sağlama | Microsoft Docs
description: Bu belgede, Azure Güvenlik Merkezi 'nde güvenlik iletişim ayrıntılarını sağlama gösterilmektedir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/11/2020
ms.author: memildin
ms.openlocfilehash: 9bbb4ee4a2fd41ca0605d7dd5d8c66c83d475744
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88506489"
---
# <a name="set-up-email-notifications-for-security-alerts"></a>Güvenlik uyarıları için e-posta bildirimleri ayarlama 

Kuruluşunuzdaki doğru kişilerin ortamınızdaki güvenlik uyarıları hakkında bildirim aldığından emin olmak için e-posta **bildirimleri** ayarları sayfasına e-posta adreslerini girin.

Bildirimlerinizi ayarlarken, e-postaları belirli kişilere veya belirli bir Azure rolüne sahip herkese bir abonelik için gönderilecek şekilde yapılandırabilirsiniz. 

Uyarı fatıg ' y i önlemek için, güvenlik merkezi giden e-posta hacminin miktarını sınırlandırır. Güvenlik Merkezi, her abonelik için şunu gönderir:

- **yüksek öneme sahip** uyarılar için günde en fazla **dört** e-posta
- **Orta önem derecesine** sahip uyarılar için günde en fazla **iki** e-posta
- **düşük önem derecesi** uyarıları için günde en fazla **bir** e-posta


:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Güvenlik uyarıları hakkında e-posta alacak kişinin ayrıntılarını yapılandırma." :::

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir|
|Fiyat|Ücretsiz katmanı|
|Gerekli roller ve izinler:|**Güvenlik Yöneticisi**<br>**Abonelik sahibi** |
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) US Gov (kısmi)<br>![Hayır](./media/icons/no-icon.png) Çin gov, diğer gov|
|||


## <a name="set-up-email-notifications-for-alerts"></a>Uyarılar için e-posta bildirimleri ayarlama <a name="email"></a>

Kişilere veya belirli Azure rollerine sahip tüm kullanıcılara e-posta bildirimleri gönderebilirsiniz.

1. Güvenlik Merkezi 'nin **fiyatlandırma & ayarları** alanından, ilgili abonelik ve **e-posta bildirimleri**' ni seçin.

1. Bildirimleriniz için alıcıları tanımlayın:

    - Açılan listeden, kullanılabilir roller ' i seçin.
    - Ve/veya virgülle ayrılmış belirli e-posta adreslerini girin. Girebilmeniz gereken e-posta adresi sayısı için bir sınır yoktur.

1. Güvenlik iletişim bilgilerini aboneliğinize uygulamak için **Kaydet**' i seçin.


## <a name="see-also"></a>Ayrıca bkz.
Güvenlik uyarıları hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Güvenlik uyarıları-bir başvuru kılavuzu](alerts-reference.md) -Azure Güvenlik Merkezi 'Nin tehdit koruması modülünde görebileceğiniz güvenlik uyarılarını öğrenin
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) --güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin
- [Iş akışı Otomasyonu](workflow-automation.md) --özel bildirim mantığı ile uyarılara olan yanıtları otomatikleştirin