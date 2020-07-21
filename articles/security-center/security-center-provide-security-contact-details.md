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
ms.openlocfilehash: 661d3845365778f7ef23cdd05b81b98c3bf84259
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519295"
---
# <a name="set-up-email-notifications-for-security-alerts"></a>Güvenlik uyarıları için e-posta bildirimleri ayarlama 

Kuruluşunuzdaki doğru kişilerin ortamınızdaki güvenlik uyarıları hakkında bildirim aldığından emin olmak için e-posta **bildirimleri** ayarları sayfasına e-posta adreslerini girin.

Bildirimlerinizi ayarlarken, e-postaları belirli kişilere veya bir abonelik için belirli bir RBAC rolüne sahip herkese gönderilecek şekilde yapılandırabilirsiniz. 

Uyarı fatıg ' y i önlemek için, güvenlik merkezi giden e-posta hacminin miktarını sınırlandırır. Güvenlik Merkezi, her abonelik için şunu gönderir:

- **yüksek öneme sahip** uyarılar için günde en fazla **dört** e-posta
- **Orta önem derecesine** sahip uyarılar için günde en fazla **iki** e-posta
- **düşük önem derecesi** uyarıları için günde en fazla **bir** e-posta

## <a name="availability"></a>Kullanılabilirlik

- Yayın durumu: **genel olarak kullanılabilir**
- Gerekli roller: **Güvenlik Yöneticisi** veya **abonelik sahibi** 
- Bulutlar: ✔ ticari bulutlar ✔ US Gov (kısmi) ✘ National/Sovereign (Çin gov, diğer gov)


## <a name="set-up-email-notifications-for-alerts"></a>Uyarılar için e-posta bildirimleri ayarlama<a name="email"></a>

Kişilere veya belirli RBAC rollerine sahip tüm kullanıcılara e-posta bildirimleri gönderebilirsiniz.

1. Güvenlik Merkezi 'nin **fiyatlandırma & ayarları** alanından, ilgili abonelik ve **e-posta bildirimleri**' ni seçin.

1. Bildirimleriniz için alıcıları tanımlayın:

    - Açılan listeden, kullanılabilir roller ' i seçin.
    - Ve/veya virgülle ayrılmış belirli e-posta adreslerini girin. Girebilmeniz gereken e-posta adresi sayısı için bir sınır yoktur.

1. Güvenlik iletişim bilgilerini aboneliğinize uygulamak için **Kaydet**' i seçin.


## <a name="see-also"></a>Ayrıca bkz.
Güvenlik uyarıları hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Güvenlik uyarıları-bir başvuru kılavuzu](alerts-reference.md) -Azure Güvenlik Merkezi 'Nin tehdit koruması modülünde görebileceğiniz güvenlik uyarılarını öğrenin
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) --güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin