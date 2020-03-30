---
title: Azure IoT Merkezi uygulamasını dışa aktarma | Microsoft Dokümanlar
description: Çözüm yöneticisi olarak, yeniden kullanabilmek için bir uygulama şablonu dışa aktarmak istiyorum.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f50c7e8dcb33fd2ed95829286aaf815926d9fb3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157662"
---
# <a name="export-your-application"></a>Uygulamanızı dışarı aktarma



Bu makalede, bir çözüm yöneticisi olarak, yeniden kullanabilmek için bir IoT Merkezi uygulamasını nasıl dışa aktarılabilmek için açıklanmaktadır.

İki seçeneğiniz vardır:

- Uygulamanızın yinelenen bir kopyasını oluşturmanız gerekiyorsa uygulamanızın bir kopyasını oluşturabilirsiniz.
- Birden çok kopya oluşturmayı planlıyorsanız, uygulamanızdan bir uygulama şablonu oluşturabilirsiniz.

## <a name="copy-your-application"></a>Başvurunuzu kopyalayın

Herhangi bir uygulamanın kopyasını oluşturabilirsiniz, herhangi bir aygıt örnekleri, aygıt veri geçmişi ve kullanıcı verileri hariç. Kopya, faturalandırılacağınız standart bir fiyatlandırma planı kullanır. Bir uygulamayı kopyalayarak ücretsiz fiyatlandırma planını kullanan bir uygulama oluşturamazsınız.

**Kopyala**’yı seçin. İletişim kutusuna, yeni uygulamanın ayrıntılarını girin. Ardından **Copy** devam etmek istediğinizi doğrulamak için Kopyala'yı seçin. Formdaki alanlar hakkında daha fazla bilgi edinmek için uygulama hızlı [başlat'ı oluştur'a](quick-deploy-iot-central.md) bakın.

![Uygulama ayarları sayfası](media/howto-use-app-templates/appcopy2.png)

Uygulama kopyalama işlemi başarılı olduktan sonra, bağlantıyı kullanarak yeni uygulamaya gidebilirsiniz.

![Uygulama ayarları sayfası](media/howto-use-app-templates/appcopy3a.png)

Bir uygulamayı kopyalamak, kuralların ve e-posta eyleminin tanımını da kopyalar. Akış ve Mantık Uygulamaları gibi bazı eylemler, Kural Kimliği aracılığıyla belirli kurallara bağlıdır. Bir kural farklı bir uygulamaya kopyalandığında, kendi Kural Kimliğini alır. Bu durumda, kullanıcıların yeni bir eylem oluşturması ve ardından yeni kuralı onunla ilişkilendirmeleri gerekir. Genel olarak, yeni uygulamada güncel olduğundan emin olmak için kuralları ve eylemleri kontrol etmek iyi bir fikirdir.

> [!WARNING]
> Pano, belirli aygıtlar hakkında bilgi görüntüleyen kutucuklar içeriyorsa, bu kutucuklar **istenen kaynağın** yeni uygulamada bulunmadığını gösterir. Bu kutucukları yeni uygulamanızdaki aygıtlar hakkında bilgi görüntülemek için yeniden yapılandırmanız gerekir.

## <a name="create-an-application-template"></a>Uygulama şablonu oluşturma

Bir Azure IoT Merkezi uygulaması oluşturduğunuzda, yerleşik örnek şablon seçeneğiniz vardır. Ayrıca, mevcut IoT Merkezi uygulamalarından kendi uygulama şablonlarınızı oluşturabilirsiniz. Daha sonra yeni uygulamalar oluştururken kendi uygulama şablonlarınızı kullanabilirsiniz.

Bir uygulama şablonu oluşturduğunuzda, varolan uygulamanızdan aşağıdaki öğeleri içerir:

- Pano düzeni ve tanımladığınız tüm döşemeler dahil olmak üzere varsayılan uygulama panosu.
- Ölçümler, ayarlar, özellikler, komutlar ve pano dahil olmak üzere aygıt şablonları.
- Kural -ları. Tüm kural tanımları dahildir. Ancak, e-posta eylemleri dışında eylemler dahil değildir.
- Durumları ve panoları da dahil olmak üzere cihaz kümeleri.

> [!WARNING]
> Pano, belirli aygıtlar hakkında bilgi görüntüleyen kutucuklar içeriyorsa, bu kutucuklar **istenen kaynağın** yeni uygulamada bulunmadığını gösterir. Bu kutucukları yeni uygulamanızdaki aygıtlar hakkında bilgi görüntülemek için yeniden yapılandırmanız gerekir.

Bir uygulama şablonu oluşturduğunuzda, aşağıdaki öğeleri içermez:

- Cihazlar
- Kullanıcılar
- İş tanımları
- Sürekli veri dışa aktarma tanımları

Bu öğeleri bir uygulama şablonundan oluşturulan uygulamalara el ile ekleyin.

Varolan bir IoT Merkezi uygulamasından bir uygulama şablonu oluşturmak için:

1. Başvurunuzdaki **İdare** bölümüne gidin.
1. **Uygulama Şablonu Dışa Aktarma'yı**seçin.
1. Uygulama **Şablonu Dışa Aktarma** sayfasında şablonunuzun adını ve açıklamasını girin.
1. Uygulama şablonu oluşturmak için **Dışa** Aktarma düğmesini seçin. Artık birinin şablondan yeni bir uygulama oluşturmasına olanak tanıyan **Paylaşılabilir Bağlantıyı** kopyalayabilirsiniz:

![Uygulama şablonu oluşturma](media/howto-use-app-templates/create-template.png)

### <a name="use-an-application-template"></a>Uygulama şablonu kullanma

Yeni bir IoT Merkezi uygulaması oluşturmak için bir uygulama şablonu kullanmak için, önceden oluşturulmuş bir **Paylaşılabilir Bağlantı**gerekir. **Paylaşılabilir Bağlantıyı** tarayıcınızın adres çubuğuna yapıştırın. **Uygulama sayfası oluştur,** özel uygulama şablonunuzun seçili olduğunu gösterir:

![Şablondan uygulama oluşturma](media/howto-use-app-templates/create-app.png)

Fiyatlandırma planınızı seçin ve formdaki diğer alanları doldurun. Ardından, uygulama şablonundan yeni bir IoT Merkezi uygulaması oluşturmak için **Oluştur'u** seçin.

### <a name="manage-application-templates"></a>Uygulama şablonlarını yönetme

Uygulama **Şablonu Dışa Aktarma** sayfasında, uygulama şablonu silebilir veya güncelleyebilirsiniz.

Bir uygulama şablonu silerseniz, yeni uygulamalar oluşturmak için daha önce oluşturulmuş paylaşılabilir bağlantıyı artık kullanamazsınız.

Uygulama şablonunuzu güncelleştirmek **için, Uygulama Şablonu Dışa Aktarma** sayfasındaşablon adını veya açıklamasını değiştirin. Ardından **Dışa** Aktar düğmesini yeniden seçin. Bu eylem yeni bir **Paylaşılabilir bağlantı** oluşturur ve önceki **Paylaşılabilir bağlantı** URL'lerini geçersiz kılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama şablonlarını kullanmayı öğrendiğiniz için önerilen bir sonraki adım, [Azure portalından IoT Central'ı](howto-manage-iot-central-from-portal.md) nasıl yöneteceğimi öğrenmektir
