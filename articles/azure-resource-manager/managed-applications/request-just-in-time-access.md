---
title: Tam zamanında erişim isteme
description: Azure Yönetilen Uygulamalar yayıncılarının yönetilen bir uygulamaya tam zamanında erişim isteğinde bulunmalarını açıklar.
author: MSEvanhi
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 7f475774828bcaecd471e13de994b156041323ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651390"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>Azure Yönetilen Uygulamalar için tam zamanında erişim etkinleştirme ve istekte

Yönetilen uygulamanızın tüketicileri, yönetilen kaynak grubuna kalıcı erişim izni vermekte isteksiz olabilir. Yönetici uygulamasının yayımcısı olarak, tüketicilerin yönetilen kaynaklara tam olarak ne zaman erişmeniz gerektiğini bilmelerini tercih edebilirsiniz. Azure Yönetilen Uygulamalar, tüketicilere yönetilen kaynaklara erişim hakkı verme konusunda daha fazla denetim sağlamak için, şu anda önizlemede olan tam zamanında (JIT) erişim adı verilen bir özellik sağlar.

JIT erişimi, sorun giderme veya bakım için yönetilen bir uygulamanın kaynaklarına yüksek erişim isteğinde bulunmanızı sağlar. Kaynaklara her zaman salt okunur erişiminiz vardır, ancak belirli bir süre için daha fazla erişime sahip olabilirsiniz.

Erişim izni için iş akışı:

1. Pazara yönetilen bir uygulama ekler ve JIT erişiminin kullanılabilir olduğunu belirtirsiniz.

1. Dağıtım sırasında, tüketici yönetilen uygulamanın bu örneği için JIT erişimi sağlar.

1. Dağıtımdan sonra, tüketici JIT erişimi ayarlarını değiştirebilir.

1. Yönetilen kaynakları sorun gidermeniz veya güncelleştirmeniz gerektiğinde erişim isteği gönderirsiniz.

1. Tüketici isteğinizi onaylar.

Bu makalede, yayıncılarJIT erişimi ne etkinleştirmek ve istekleri göndermek için gerçekleştirdiği eylemler üzerinde duruluyor. JIT erişim isteklerini onaylama hakkında bilgi edinmek için Azure [Yönetilen Uygulamalarda tam zamanında erişimi onayla' ya](approve-just-in-time-access.md)bakın.

## <a name="add-jit-access-step-to-ui"></a>UI'ye JIT erişim adımı ekleme

CreateUiDefinition.json dosyanız, tüketicilerin JIT erişimini etkinleştirmenize olanak tanıyan bir adım eklemeniz dışında, kalıcı erişim için oluşturduğunuz UI dosyasına tam olarak benzer. Azure Marketi'nde yönetilen ilk uygulama teklifiniyayımlama hakkında daha fazla bilgi edinmek için [Market'te Azure Yönetilen Uygulamalar'a](publish-marketplace-app.md)bakın.

Teklifiniz için JIT özelliğini desteklemek için CreateUiDefinition.json dosyanıza aşağıdaki içeriği ekleyin:

"Adımlar"da:

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```
 
"Çıktılar"da:

```json
"jitAccessPolicy": "[steps('jitConfiguration').jitConfigurationControl]"
```

> [!NOTE]
> JIT erişimi önizlemede. JIT yapılandırması için şema gelecekteki yinelemelerde değişebilir.

## <a name="enable-jit-access"></a>JIT erişimini etkinleştirme

Pazarda teklifinizi tanımlarken, JIT erişimini etkinleştirdiğinizden emin olun.

1. Bulut Ortağı [yayımlama portalında](https://cloudpartner.azure.com)oturum açın.

1. Yönetilen uygulamanızı pazarda yayınlamak için değerler sağlayın. **JIT Erişimini Etkinleştirmek** için **Evet'i** mi seçtin?

   ![Tam zamanında erişimi etkinleştirme](./media/request-just-in-time-access/marketplace-enable.png)

UI'nize bir JIT yapılandırma adımı eklediniz ve pazar sunan da JIT erişimini etkinleştirdin. Tüketiciler yönetilen uygulamanızı dağıttıklarında, [kendi örnekleri için JIT erişimini açabilirler.](approve-just-in-time-access.md#enable-during-deployment)

## <a name="request-access"></a>Erişim izni isteme

Tüketicinin yönetilen kaynaklarına erişmeniz gerektiğinde, belirli bir rol, zaman ve süre için bir istek gönderirsiniz. Tüketici daha sonra isteği onaylamalıdır.

JIT erişim isteği göndermek için:

1. Erişmeniz gereken yönetilen uygulama için **JIT Access'i** seçin.

1. **Uygun Roller'i**seçin ve istediğiniz rol için EYLEM sütununda **Etkinleştir'i** seçin.

   ![Erişim isteğini etkinleştirme](./media/request-just-in-time-access/send-request.png)

1. Rolü **Etkinleştir** formunda, rolünüzün etkin olması için bir başlangıç zamanı ve süresi seçin. İsteği göndermek için **Etkinleştir'i** seçin.

   ![Erişimi etkinleştirme](./media/request-just-in-time-access/activate-access.png) 

1. Yeni JIT isteğinin tüketiciye başarıyla gönderildiğini görmek için bildirimleri görüntüleyin.

   ![Bildirim](./media/request-just-in-time-access/in-progress.png)

   Şimdi, tüketicinin [isteğinizi onaylamasını beklemeniz](approve-just-in-time-access.md#approve-requests)gerekir.

1. Yönetilen bir uygulama için tüm JIT isteklerinin durumunu görüntülemek için **JIT Access** ve **Request History'yi**seçin.

   ![Durumu görüntüleme](./media/request-just-in-time-access/view-status.png)

## <a name="known-issues"></a>Bilinen sorunlar

JIT erişimi isteyen hesabın asıl kimliği, yönetilen uygulama tanımına açıkça eklenmelidir. Hesap yalnızca pakette belirtilen bir grup aracılığıyla dahil edilemez. Bu sınırlama gelecekteki bir sürümde düzeltilecektir.

## <a name="next-steps"></a>Sonraki adımlar

JIT erişimi isteklerini onaylama hakkında bilgi edinmek için Azure [Yönetilen Uygulamalarda tam zamanında erişimi onayla' ya](approve-just-in-time-access.md)bakın.
