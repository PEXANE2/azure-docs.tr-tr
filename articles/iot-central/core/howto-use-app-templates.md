---
title: Azure IoT Central uygulamasını dışarı aktarma | Microsoft Docs
description: Bir çözüm Yöneticisi olarak, bir uygulama şablonunu yeniden kullanabilmek için dışarı aktarmak istiyorum.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: e05256d26da2c061559bcc8e50e9ecce6d8425a0
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248218"
---
# <a name="export-your-application"></a>Uygulamanızı dışarı aktarma

Bu makalede, bir IoT Central uygulamasının yeniden kullanabilmek için bir çözüm Yöneticisi olarak nasıl dışarı aktarılacağı açıklanmaktadır.

İki seçeneğiniz vardır:

- Yalnızca uygulamanızın yinelenen bir kopyasını oluşturmanız gerekiyorsa uygulamanızın bir kopyasını oluşturabilirsiniz.
- Birden çok kopya oluşturmayı planlıyorsanız uygulamanızdan bir uygulama şablonu oluşturabilirsiniz.

## <a name="copy-your-application"></a>Uygulamanızı kopyalayın

Herhangi bir uygulamanın bir kopyasını, eksi herhangi bir cihaz örneğini, cihaz veri geçmişini ve Kullanıcı verilerini oluşturabilirsiniz. Kopya, için faturalandırılacağımız standart bir fiyatlandırma planı kullanır. Bir uygulamayı kopyalayarak ücretsiz fiyatlandırma planı kullanan bir uygulama oluşturamazsınız.

**Kopyala**’yı seçin. İletişim kutusunda, yeni uygulamanın ayrıntılarını girin. Sonra devam etmek istediğinizi onaylamak için **Kopyala** ' yı seçin. Formdaki alanlar hakkında daha fazla bilgi edinmek için bkz. [uygulama oluşturma](quick-deploy-iot-central.md) hızlı başlangıcı.

![Uygulama ayarları sayfası](media/howto-use-app-templates/appcopy2.png)

Uygulama kopyalama işlemi başarılı olduktan sonra, bağlantısını kullanarak yeni uygulamaya gidebilirsiniz.

![Uygulama ayarları sayfası](media/howto-use-app-templates/appcopy3a.png)

Bir uygulamayı kopyalamak kuralların ve e-posta eyleminin tanımını da kopyalar. Flow ve Logic Apps gibi bazı eylemler kural KIMLIĞI aracılığıyla belirli kurallara bağlıdır. Bir kural farklı bir uygulamaya kopyalandığında kendi kural KIMLIĞINI alır. Bu durumda, kullanıcıların yeni bir eylem oluşturması ve yeni kuralı onunla ilişkilendirilmesi gerekir. Genel olarak, yeni uygulamada güncel olduklarından emin olmak için kuralları ve eylemleri denetlemeniz iyi bir fikirdir.

> [!WARNING]
> Bir pano belirli cihazlarla ilgili bilgileri görüntüleyen kutucuklar içeriyorsa, bu kutucuklar **istenen kaynağı** yeni uygulamada bulamadı. Yeni uygulamanızdaki cihazlarla ilgili bilgileri göstermek için bu kutucukları yeniden yapılandırmanız gerekir.

## <a name="create-an-application-template"></a>Uygulama şablonu oluşturma

Bir Azure IoT Central uygulaması oluşturduğunuzda, yerleşik bir örnek şablonlar seçiminiz vardır. Ayrıca, mevcut IoT Central uygulamalarından kendi uygulama şablonlarınızı da oluşturabilirsiniz. Ardından, yeni uygulamalar oluştururken kendi uygulama şablonlarınızı kullanabilirsiniz.

Bir uygulama şablonu oluşturduğunuzda, var olan uygulamanızdan aşağıdaki öğeleri içerir:

- Pano düzeni ve tanımladığınız tüm kutucuklar dahil olmak üzere varsayılan uygulama panosu.
- Ölçümler, ayarlar, özellikler, komutlar ve Pano dahil cihaz şablonları.
- Kuralın. Tüm kural tanımları dahildir. Ancak, e-posta eylemleri hariç eylemler dahil değildir.
- Koşulları ve panoları dahil cihaz kümeleri.

> [!WARNING]
> Bir pano belirli cihazlarla ilgili bilgileri görüntüleyen kutucuklar içeriyorsa, bu kutucuklar **istenen kaynağı** yeni uygulamada bulamadı. Yeni uygulamanızdaki cihazlarla ilgili bilgileri göstermek için bu kutucukları yeniden yapılandırmanız gerekir.

Bir uygulama şablonu oluşturduğunuzda, bu, aşağıdaki öğeleri içermez:

- Cihazlar
- Kullanıcılar
- İş tanımları
- Sürekli veri dışa aktarma tanımları

Bu öğeleri bir uygulama şablonundan oluşturulan tüm uygulamalara el ile ekleyin.

Mevcut bir IoT Central uygulamasından uygulama şablonu oluşturmak için:

1. Uygulamanızdaki **Yönetim** bölümüne gidin.
1. **Uygulama şablonu dışarı aktar**' ı seçin.
1. **Uygulama şablonu dışarı aktarma** sayfasında, şablonunuz için bir ad ve açıklama girin.
1. Uygulama şablonunu oluşturmak için **dışarı aktar** düğmesini seçin. Artık, bir kişinin şablondan yeni bir uygulama oluşturmasını sağlayan **paylaşılabilir bağlantıyı** kopyalayabilirsiniz:

![Uygulama şablonu oluşturma](media/howto-use-app-templates/create-template.png)

### <a name="use-an-application-template"></a>Uygulama şablonu kullanma

Yeni bir IoT Central uygulaması oluşturmak üzere bir uygulama şablonu kullanmak için önceden oluşturulmuş **paylaşılabilir bir bağlantıya**ihtiyacınız vardır. **Paylaşılabilir bağlantıyı** tarayıcınızın adres çubuğuna yapıştırın. **Uygulama oluştur** sayfası, özel uygulama şablonunuz seçili olarak görüntülenir:

![Şablondan uygulama oluşturma](media/howto-use-app-templates/create-app.png)

Fiyatlandırma planınızı seçin ve formdaki diğer alanları doldurun. Sonra uygulama şablonundan yeni bir IoT Central uygulaması oluşturmak için **Oluştur** ' u seçin.

### <a name="manage-application-templates"></a>Uygulama şablonlarını yönetme

**Uygulama şablonu dışarı aktarma** sayfasında, uygulama şablonunu silebilir veya güncelleştirebilirsiniz.

Bir uygulama şablonunu silerseniz, yeni uygulamalar oluşturmak için daha önce oluşturulmuş paylaşılabilir bağlantısını artık kullanamazsınız.

Uygulama şablonunuzu güncelleştirmek için, **uygulama şablonu dışarı aktarma** sayfasındaki şablon adını veya açıklamasını değiştirin. Ardından **dışarı aktar** düğmesini tekrar seçin. Bu eylem, yeni bir **paylaşılabilir bağlantı** oluşturur ve önceki **paylaşılabilir bağlantı** URL 'lerini geçersiz kılar.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama şablonlarını nasıl kullanacağınızı öğrendiğinize göre, bir sonraki adım, [bir IoT Central uygulamasına bağlı cihazların genel durumunu nasıl izleyeceğinizi](howto-monitor-application-health.md) öğrenmektedir.
