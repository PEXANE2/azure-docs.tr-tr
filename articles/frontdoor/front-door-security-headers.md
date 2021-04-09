---
title: 'Öğretici: kural altyapısına güvenlik üstbilgileri ekleme-Azure ön kapısı'
description: Bu öğretici, Azure ön kapısından kurallar altyapısı aracılığıyla bir güvenlik üst bilgisi yapılandırmayı öğretir
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: 1727193faa028a1d681f2a74df950afeb9570ec9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91270058"
---
# <a name="tutorial-add-security-headers-with-rules-engine"></a>Öğretici: kural altyapısına güvenlik üst bilgileri ekleme

Bu öğreticide, HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy veya X-Frame-Options gibi tarayıcı tabanlı güvenlik açıklarını önlemeye yönelik güvenlik üstbilgilerinin nasıl uygulanacağı gösterilmektedir. Güvenlik tabanlı öznitelikler, tanımlama bilgileriyle de tanımlanabilir.

Aşağıdaki örnek, kurallarınızın altyapı yapılandırmanızın ilişkilendirildiği yolda tanımlanan yol ile eşleşen tüm gelen isteklere Içerik-güvenlik-Ilke üst bilgisi nasıl ekleneceğini gösterir. Burada, yalnızca güvenilir sitemizdeki betiklerin **https://apiphany.portal.azure-api.net** uygulamamızda çalışmasına izin veririz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Rules Engine içinde Content-Security-Policy yapılandırma.

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticideki adımları tamamlayabilmeniz için öncelikle bir Front Door oluşturmanız gerekir. Daha fazla bilgi için bkz. [Hızlı başlangıç: Front Door oluşturma](quickstart-create-front-door.md).
* Kural altyapısı özelliğini ilk kez kullanıyorsanız, bkz. [bir kural altyapısı ayarlama](front-door-tutorial-rules-engine.md).

## <a name="add-a-content-security-policy-header-in-azure-portal"></a>Azure portal Içerik-Güvenlik Ilkesi üst bilgisi ekleme

1. Yeni kural eklemek için **Ekle**'ye tıklayın. Kurala bir ad verin ve ardından **eylem**  >  **yanıt üst bilgisi** Ekle ' ye tıklayın.

1. Bu üst bilgiyi bu rotaya yönelik tüm gelen isteklere yanıt olarak **eklemek için, bu üstbilgiyi Ekle olarak** ayarlayın.

1. Üst bilgi adını ekleyin: **Content-Security-Policy** ve bu üstbilginin kabul etmesi gereken değerleri tanımlayın. Bu senaryoda, *"Script-src ' Self ' öğesini seçtik https://apiphany.portal.azure-api.net .*

1. Yapılandırmanıza istediğiniz kuralların tümünü ekledikten sonra tercih ettiğiniz rotaya gidip kural motoru yapılandırmanızı yol kuralınız ile ilişkilendirmeyin. Kuralın çalışmasını sağlamak için bu adım gereklidir. 

![Portal örneği](./media/front-door-rules-engine/rules-engine-security-header-example.png)

> [!NOTE]
> Bu senaryoda, kurala [eşleşen koşulları](front-door-rules-engine-match-conditions.md) ekleyemedik. Yol kuralında tanımlanan yol ile eşleşen tüm gelen istekler bu kural uygulanmış olacaktır. Bu isteklerin yalnızca bir alt kümesine uygulanmasını istiyorsanız, özel **eşleşme koşullarınızı** bu kurala eklediğinizden emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yukarıdaki adımlarda, kurallar altyapısıyla güvenlik üst bilgileri yapılandırdınız. Kuralı artık istemiyorsanız, kuralı Sil ' i tıklatarak kaldırabilirsiniz.

:::image type="content" source="./media/front-door-rules-engine/rules-engine-delete-rule.png" alt-text="Kuralı silme":::

## <a name="next-steps"></a>Sonraki adımlar

Ön kapılarınız için bir Web uygulaması güvenlik duvarını yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Web Uygulaması Güvenlik Duvarı ve Front Door](front-door-waf.md)
