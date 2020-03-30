---
title: Key Vault sertifikasını kullanarak özel etki alanı ekleyemezsiniz
titleSuffix: Azure API Management
description: Önemli bir kasa sertifikası kullanarak Azure API Yönetimi'ne özel bir etki alanı ekleyemezsiniz sorunu nasıl giderebileceğinizi öğrenin.
services: api-management
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/19/2019
ms.author: tehnoonr
ms.openlocfilehash: a09c15466a4a9f62b2696b087cb7ab23cc767379
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430588"
---
# <a name="failed-to-update-api-management-service-hostnames"></a>API Yönetimi hizmeti ana bilgisayar adlarını güncelleştirmek için başarısız oldu

Bu makalede, Azure API Yönetimi hizmeti için özel bir etki alanı eklediğinizde karşılaşabileceğiniz "API Yönetimi hizmet ana bilgisayar adlarını güncelleştirmede başarısız oldu" hatası açıklanmaktadır. Bu makalede, sorunu çözmenize yardımcı olacak sorun giderme adımları sağlar.

## <a name="symptoms"></a>Belirtiler

Azure Key Vault'tan bir sertifika kullanarak API Yönetimi hizmetiniz için özel bir etki alanı eklemeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

- API Yönetimi hizmeti ana bilgisayar adlarını güncelleştirmek için başarısız oldu. Kaynak İsteğihttps://vaultname.vault.azure.net/secrets/secretname/?api-version=7.0' ' StatusCode ile başarısız oldu: RequestId için yasak: . Özel durum iletisi: İşlem geçersiz bir durum kodu 'Yasak' döndürür.

## <a name="cause"></a>Nedeni

API Yönetimi hizmeti, özel etki alanı için kullanmaya çalıştığınız anahtar kasasına erişme iznine sahip değildir.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için şu adımları izleyin:

1. [Azure portalına](Https://portal.azure.com)gidin, API Yönetimi örneğini seçin ve ardından **Yönetilen kimlikleri**seçin. Azure Etkin **Dizinile Kaydol** seçeneğinin **Evet**olarak ayarlandıklarına emin olun. 
    ![Azure Active Director'a Kaydol](./media/api-management-troubleshoot-cannot-add-custom-domain/register-with-aad.png)
1. Azure **portalında, Key vaults** hizmetini açın ve özel etki alanı için kullanmaya çalıştığınız anahtar kasasını seçin.
1. **Access ilkelerini**seçin ve API Yönetimi hizmet örneğinin adıyla eşleşen bir hizmet ilkesi olup olmadığını denetleyin. Varsa, hizmet ilkesini seçin ve **Gizli izinler**altında listelenen **Izin Al** olduğundan emin olun.  
    ![Hizmet ilkesi için erişim ilkesi ekleme](./media/api-management-troubleshoot-cannot-add-custom-domain/access-policy.png)
1. API Yönetimi hizmeti listede yoksa, **erişim ilkesi ekle'yi**seçin ve ardından aşağıdaki erişim ilkesini oluşturun:
    - **Şablondan Yapılandırma**: Yok
    - **Anapara seçin**: API Yönetimi hizmetinin adını arayın ve listeden seçin
    - **Anahtar izinler**: Yok
    - **Gizli izinler**: Get
    - **Sertifika izinleri**: Yok
1. Erişim ilkesini oluşturmak için **Tamam'ı** seçin.
1. Değişiklikleri kaydetmek için **Kaydet'i** seçin.

Sorunun çözülüp çözülmediğini denetleyin. Bunu yapmak için, Anahtar Kasa sertifikasını kullanarak API Yönetimi hizmetinde özel etki alanı oluşturmaya çalışın.

## <a name="next-steps"></a>Sonraki adımlar
API Yönetimi hizmeti hakkında daha fazla bilgi edinin:

- API Yönetimi hakkında daha fazla [video](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.
* Arka uç hizmetinizi güvence altına almanın diğer yolları için [Karşılıklı Sertifika kimlik doğrulaması'na](api-management-howto-mutual-certificates.md)bakın.

* [BIR API Yönetimi hizmeti örneği oluşturun.](get-started-create-service-instance.md)
* [İlk API'nizi yönetin.](import-and-publish.md)