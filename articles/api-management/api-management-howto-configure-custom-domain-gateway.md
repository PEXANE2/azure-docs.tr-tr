---
title: Kendi barındırılan Azure API Yönetimi ağ geçidiniz için özel bir etki alanı adı yapılandırma | Microsoft Dokümanlar
description: Bu konu, kendi barındırılan Azure API Yönetimi ağ geçidi için özel bir etki alanı adı yapılandırma adımlarını açıklar.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: 1f2184c7c62887a98a76877528b167d173c3d75b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335940"
---
# <a name="configure-a-custom-domain-name"></a>Özel bir etki alanı adı yapılandırma

Kendi kendine [barındırılan](self-hosted-gateway-overview.md) bir Azure API Yönetimi ağ geçidi ni sağladığınızı ve ana bilgisayar adı atanmadığını ve IP adresine başvurması gerekir. Bu makalede, varolan özel bir DNS adının (ana bilgisayar adı olarak da adlandırılır) kendi barındırılan ağ geçidinin nasıl eşlenebildiğini gösterir.

> [!NOTE]
> Kendi kendine barındırılan ağ geçidi özelliği önizlemededir. Önizleme sırasında, kendi kendine barındırılan ağ geçidi ek ücret ödemeden yalnızca Geliştirici ve Premium katmanlarında kullanılabilir. Geliştirici katmanı, kendi kendine barındırılan tek bir ağ geçidi dağıtımıyla sınırlıdır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunları yapmalısınız:

-   Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Bir API Yönetimi örneği. Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)
- Kendi kendine barındırılan bir ağ geçidi. Daha fazla bilgi için, [kendi kendine barındırılan ağ geçidini nasıl sağlayacağız](api-management-howto-provision-self-hosted-gateway.md)
-   Size veya kuruluşunuza ait özel bir etki alanı adı. Bu konu, özel bir etki alanı adı satın almak için nasıl talimatlar sağlamaz.
-   Özel alan adını kendi barındırılan ağ geçidinin IP adresiyle eşleyen bir DNS sunucusunda barındırılan bir DNS kaydı. Bu konu, bir DNS kaydının nasıl barındırılalacılaşacağına ilişkin yönergeler sağlamaz.
-   Ortak ve özel anahtara sahip geçerli bir sertifikanız olmalıdır (. PFX). Konu veya konu alternatif adı (SAN) etki alanı adı eşleşmiştir (bu TLS üzerinden URL'leri güvenli bir şekilde ortaya çıkarmak için API Yönetimi örneğini sağlar).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-custom-domain-certificate-to-your-api-management-service"></a>API Yönetimi hizmetinize özel etki alanı sertifikası ekleme

1. **Güvenlik**Altından **Sertifikalar'ı** seçin.
2. **+ Ekle** öğesini seçin.
3. **Kimlik** alanına sertifika için bir kaynak adı girin.
4. Sertifikayı içeren dosyayı seçin (. PFX) **Sertifika** alanını veya bitişik klasör simgesini seçerek.
5. Sertifikanın parolasını **Parola** alanına girin.
6. API Yönetimi hizmetinize sertifika eklemek için **Oluştur'u** seçin.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name-for-your-self-hosted-gateway"></a>Kendi barındırılan ağ geçidiniz için özel bir etki alanı adı ayarlamak için Azure portalını kullanın

1. **Ayarlar'ın**altından **Ağ Geçitleri'ni** seçin.
2. Etki alanı adını yapılandırmak istediğiniz kendi kendine barındırılan ağ geçidini seçin.
3. **Ayarlar**altında **Ana Bilgisayar Adlarını** seçin.
4. Seç **+ Ekle**
5. **Ad** alanına ana bilgisayar adı için kaynak adı girin.
6. **Hostname** alanına alan adı girin.
7. **Sertifika** açılır tarihinden bir sertifika seçin.
8. Bu ağ geçidi aracılığıyla açığa çıkan API'lerden herhangi biri istemci sertifikası kimlik doğrulamasını kullanıyorsa, Müşteri Sertifikası Kimlik Doğrulaması'nı **görüşün** onay kutusunu seçin.
    > [!WARNING]
    > Bu ayar, ağ geçidi için yapılandırılan tüm etki alanı adları tarafından paylaşılır.
9. Özel etki alanı adını seçili kendi barındırılan ağ geçidine atamak için **Ekle'yi** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmetinizi yükseltin ve ölçeklendirin](upgrade-and-scale.md)
