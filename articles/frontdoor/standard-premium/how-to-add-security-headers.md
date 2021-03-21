---
title: Azure ön kapısı Standart/Premium (Önizleme) kural kümesi ile güvenlik üstbilgilerini yapılandırma
description: Bu makale, güvenlik üstbilgilerini yapılandırmak için kural kümesinin nasıl kullanılacağına ilişkin yönergeler sağlar.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: c08ba57f43969bb2f0ee9c66b6cb4e92879ed258
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101100459"
---
# <a name="configure-security-headers-with-azure-front-door-standardpremium-preview-rule-set"></a>Azure ön kapısı Standart/Premium (Önizleme) kural kümesi ile güvenlik üstbilgilerini yapılandırma

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Bu makalede, HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy veya X-Frame-Options gibi tarayıcı tabanlı güvenlik açıklarını önlemeye yönelik güvenlik üstbilgilerinin nasıl uygulanacağı gösterilmektedir. Güvenlik tabanlı öznitelikler, tanımlama bilgileriyle de tanımlanabilir.

Aşağıdaki örnek, rotadaki yol ile eşleşen tüm gelen isteklere Content-Security-Policy üstbilgisinin nasıl ekleneceğini gösterir. Burada, yalnızca güvenilir sitemizdeki betiklerin **https://apiphany.portal.azure-api.net** uygulamamızda çalışmasına izin veririz.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

* Güvenlik üstbilgilerini yapılandırmak için önce bir ön kapı oluşturmanız gerekir. Daha fazla bilgi için bkz. [Hızlı başlangıç: Front Door oluşturma](create-front-door-portal.md).
* Kural kümesi özelliğini daha önce kullanmadıysanız [bir kural kümesi ayarlamayı](how-to-configure-rule-set.md) inceleyin.

## <a name="add-a-content-security-policy-header-in-azure-portal"></a>Azure portal Içerik-Güvenlik Ilkesi üst bilgisi ekleme

1. Azure ön kapısının Standart/Premium profiline gidin ve ayarlar altında **kural kümesi** ' ni seçin **.**

1. Yeni bir kural kümesi eklemek için **Ekle** ' yi seçin. Kurala bir **ad** verip daha sonra kural Için bir **ad** sağlayın. **Eylem Ekle** ' yi seçin ve **yanıt başlığı**' nı seçin.

1. Bu üst bilgiyi, bu yolun tüm gelen isteklerine yanıt olarak eklemek için **Ekle** işlecini ayarla olarak ayarlayın.

1. Üst bilgi adını ekleyin: **Content-Security-Policy** ve bu üstbilginin kabul etmesi gereken değerleri tanımlayın. Bu senaryoda, *"Script-src ' Self ' https://apiphany.portal.azure-api.net "* öğesini seçtik.

1. Yapılandırmanıza istediğiniz kuralların tümünü ekledikten sonra, kural kümesini bir rota ile ilişkilendirmeyi unutmayın. Kural kümesinin işlem yapmasına izin vermek için bu adım *gereklidir* . 

> [!NOTE]
> Bu senaryoda, kurala [eşleşen koşulları](concept-rule-set-match-conditions.md) ekleyemedik. İlişkili rotada tanımlanan yol ile eşleşen tüm gelen istekler bu kural uygulanmış olacaktır. Bu isteklerin yalnızca bir alt kümesine uygulanmasını istiyorsanız, özel **eşleşme koşullarınızı** bu kurala eklediğinizden emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="deleting-a-rule"></a>Bir kuralı silme

Yukarıdaki adımlarda, kural kümesi ile Content-Security-Policy üst bilgisini yapılandırdınız. Artık bir kural istemiyorsanız kural kümesi adını seçip kuralı Sil ' i seçebilirsiniz. 

### <a name="deleting-a-rule-set"></a>Bir kural kümesini silme

Bir kural kümesini silmek istiyorsanız, silmeden önce tüm rotalardan ilişkiyi kaydettiğinizden emin olun. Bir kural kümesini silme hakkında ayrıntılı yönergeler için, bkz. [kural kümesini yapılandırma](how-to-configure-rule-set.md).

## <a name="next-steps"></a>Sonraki adımlar

Ön kapılarınız için bir Web uygulaması güvenlik duvarını yapılandırma hakkında bilgi edinmek için bkz. [Web uygulaması güvenlik duvarı ve ön kapı](../../web-application-firewall/afds/afds-overview.md).
