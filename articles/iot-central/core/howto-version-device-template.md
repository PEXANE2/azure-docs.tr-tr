---
title: Azure IoT Central uygulamalarınız için cihaz şablonu sürümünü anlama | Microsoft Dokümanlar
description: Yeni sürümler oluşturarak ve canlı bağlı cihazlarınızı etkilemeden cihaz şablonlarınız üzerinde yeniden düzenleme
author: sarahhubbard
ms.author: sahubbar
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: bb77d1a09cb2692765c4c834ce617d13465d4d67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157577"
---
# <a name="create-a-new-device-template-version"></a>Yeni bir aygıt şablonu sürümü oluşturma



Azure IoT Central, IoT Uygulamaları'nın hızlı bir şekilde geliştirilmesine olanak tanır. Aygıt özelliklerini, görünümlerini ve özelleştirmelerini ekleyerek, düzenleyerek veya silerek cihaz şablon tasarımlarınızı hızlı bir şekilde yineleyebilirsiniz. Aygıt şablonunuzu yayımladıktan sonra, aygıt yeteneği modeli modelin yanındaki kilit simgeleriyle **Yayımlanmış** olarak gösterilir. Aygıt yeteneği modelinde değişiklik yapmak için aygıt şablonunun yeni bir sürümünü oluşturmanız gerekir. Bu arada bulut özellikleri, özelleştirmeler ve görünümler, aygıt şablonu sürümü gerek kalmadan herhangi bir zamanda düzenlenebilir. Bu değişikliklerden herhangi birini kaydettikten sonra, işleç için en son değişiklikleri Aygıt Gezgini'nde görüntülemek için kullanılabilir hale getirmek için aygıt şablonuna yayımlayabilirsiniz.

> [!NOTE]
> Aygıt şablonu oluşturma hakkında daha fazla bilgi edinmek için [bkz.](howto-set-up-template.md)

## <a name="add-customizations-to-the-device-template-without-versioning"></a>Sürüm olmadan aygıt şablonuna özelleştirmeekleme

Aygıt yeteneklerinizin belirli öğeleri, aygıt şablonunuzu ve arayüzlerinizi sürüm oluşturmanıza gerek kalmadan düzenlenebilir. Örneğin, bu alanlardan bazıları görüntü adı, anlamsal türü, minimum değer, maksimum değer, ondalık basamaklar, renk, birim, görüntü birimi, yorum ve açıklama içerir. Bu özelleştirmelerden birini eklemek için:

1. **Aygıt Şablonları** sayfasına gidin.
1. Özelleştirmek istediğiniz aygıt şablonuna seçin.
1. **Özelleştir** sekmesini seçin.
1. Cihaz yetenek modelinizde tanımlanan tüm özellikler burada listelenir. Burada oluşturabileceğiniz tüm alanlar, cihazınızŞablonunuzu sürümüne gerek kalmadan cihazınızın her yerinde kaydedilebilir ve kullanılabilir. Yalnızca okunan bir şekilde değiştirmek istediğiniz alanlar varsa, bunları değiştirmek için aygıt şablonunuzu sürümoluşturmanız gerekir. Başka bir değerde yeniden yapmak ve girmek istediğiniz bir alan seçin.
1. **Kaydet**'e tıklayın. Artık bu değerler, başlangıçta aygıt şablonunuzda kaydedilen ve uygulama genelinde kullanılacak olan her şeyi geçersiz kılar.

## <a name="versioning-a-device-template"></a>Aygıt şablonu sürüm oluşturma

Aygıt şablonunuzun yeni bir sürümünü oluşturmak, şablonun aygıt yetenek modelinin düzenlenebileceği taslak bir sürümünü oluşturur. Yayımlanmış arabirimler, tek tek sürümlendirilene kadar yayımlanmamış olarak kalır. Yayımlanmış bir arabirimi değiştirmek için önce yeni bir aygıt şablonu sürümü oluşturmanız gerekir.

Aygıt şablonu yalnızca aygıt şablonunun özelleştirmeler bölümünde düzenlemeyi başaramadığınızda aygıt yeteneği modelinin bir bölümünü düzenlemeye çalıştığınızda sürülmelidir. 

Aygıt şablonu sürümü için:

1. **Aygıt Şablonları** sayfasına gidin.
1. Sürüm oluşturmaya çalıştığınız aygıt şablonunu seçin.
1. Sayfanın üst kısmındaki **Sürüm** düğmesini tıklatın ve şablona yeni bir ad verin. Sizin için düzenlenebilecek yeni bir isim önerdik.
1. **Oluştur'u**tıklatın.
1. Artık aygıt şablonunuzun taslak modunda olduğu görülüyor. Arabirimlerinizin hala kilitli olduğunu ve düzenlenebilmek için tek tek sürümlendirilmeniz gerektiğini görürsünüz. 

### <a name="versioning-an-interface"></a>Arabirimi sürümleme

Arabirimi sürüm oluşturma, zaten oluşturduğunuz arabirim içindeki yetenekleri eklemenize, güncelleştirmenize ve kaldırmanıza olanak tanır. 

Bir arabirimi sürümlemek için:

1. **Aygıt Şablonları** sayfasına gidin.
1. Taslak modunda sahip olduğunuz aygıt şablonunu seçin.
1. Sürümünü ve yeniden biçimdedini yapmak istediğiniz yayımlanmış modda olan arabirimi seçin.
1. Arayüz sayfasının üst kısmındaki **Sürüm** düğmesini tıklatın. 
1. **Oluştur'u**tıklatın.
1. Şimdi arabiriminiz taslak modunda. Varolan özelleştirmeleri ve görünümleri bozmadan arabiriminize özellikler ekleyebilir veya bunları edebilirsiniz. 

> [!NOTE]
> Azure IoT tarafından yayınlanan standart arabirimler sürülenemez veya düzenlenemez. Bu standart arabirimler aygıt sertifikası için kullanılır.

> [!NOTE]
> Arayüz yayımlandıktan sonra, taslak modunda bile bu özelliklerin hiçbirini silemezsiniz. Özellikler yalnızca taslak modunda düzenlenebilir veya arabirime eklenebilir.


## <a name="migrate-a-device-across-device-template-versions"></a>Aygıtı aygıt şablonu sürümlerine geçirme

Aygıt şablonunun birden çok sürümü oluşturabilirsiniz. Zamanla, bu aygıt şablonlarını kullanarak birden çok bağlı aygıtınız olacaktır. Aygıtları aygıt şablonunuzun bir sürümünden diğerine geçirebilirsiniz. Aşağıdaki adımlar, bir aygıtın nasıl geçirilen açıklanmıştır:

1. **Aygıt Gezgini** sayfasına gidin.
1. Başka bir sürüme geçirmek için gereken aygıtı seçin.
1. **Geçir'i**seçin.
1. Aygıtı geçirmek istediğiniz sürüm numarasını içeren aygıt şablonunu seçin ve **Geçir'i**seçin.

![Aygıt nasıl geçirilir?](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda aygıt şablonu sürümlerini kullanmayı öğrendiğinize göre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Telemetri kuralları nasıl oluşturulur?](tutorial-create-telemetry-rules.md)
