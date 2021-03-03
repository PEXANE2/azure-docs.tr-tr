---
title: Azure Percept Studio içinde sesli yardım uygulamasını yapılandırma
description: Azure Percept Studio içinde sesli yardım uygulamasını yapılandırma
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: 593d2ccadf814cb5af27a4bb738f28132f39ab7d
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663869"
---
# <a name="managing-your-voice-assistant"></a>Ses yardımcınızı yönetme

Bu makalede, [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)'daki ses Yardımcısı uygulamanızın anahtar sözcüğünün ve komutlarının nasıl yapılandırılacağı açıklanır. Anahtar kelimenizi Portal yerine IoT Hub içinde yapılandırmaya yönelik rehberlik için lütfen bu [nasıl yapılır makalesine](./how-to-configure-voice-assistant.md)bakın.

Henüz bir ses Yardımcısı uygulaması oluşturmadıysanız, lütfen bkz. [Azure Percept Studio ve Azure Percept Audio ile kod No-Code Voice Yardımcısı oluşturma](./tutorial-no-code-speech.md).

## <a name="keyword-configuration"></a>Anahtar sözcük yapılandırması

Anahtar sözcüğü, bir sesli yardımcıyı etkinleştirmek için kullanılan bir kelime veya kısa tümceciktir. Örneğin, "Hey Cortana", Cortana yardımcısı için anahtar sözcüktür. Ses etkinleştirme, kullanıcılarınızın yalnızca anahtar sözcüğünü konuşarak tamamen ücretsiz olarak sorunsuz bir şekilde etkileşim kurmaya başlamasını sağlar. Ürününüz anahtar sözcüğü sürekli dinlediğinden, Kullanıcı verilerinin mümkün olduğunca özel kalmasını sağlamak için bir algılama gerçekleşene kadar tüm sesler yerel olarak işlenir.

### <a name="configuration-within-the-voice-assistant-demo-window"></a>Ses Yardımcısı tanıtım penceresi içinde yapılandırma

1. Demo sayfasında **özel anahtar sözcük** ' ın yanındaki **Değiştir** ' e tıklayın.

    :::image type="content" source="./media/manage-voice-assistant/hospitality-demo.png" alt-text="Görüntüyle.":::

    Demo sayfası açık değilse, cihaz sayfasına gidin (aşağıya bakın) ve tanıtım 'e erişmek için, **Eylemler** bölümünde **Ses yardımcınızı test et** ' e tıklayın.

1. Kullanılabilir anahtar sözcüklerden birini seçin ve değişiklikleri uygulamak için **Kaydet** ' e tıklayın.

1. Azure Percept ses cihazı üzerindeki üç LED ışıkları, yapılandırma tamamlandığında ve ses yardımcınız kullanıma hazırlandığında parlak mavi (yanıp sönen) olarak değişir.

### <a name="configuration-within-the-device-page"></a>Cihaz sayfası içinde yapılandırma

1. [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)'nun Genel Bakış sayfasında sol menü bölmesinde **cihazlar** ' a tıklayın.

    :::image type="content" source="./media/manage-voice-assistant/portal-overview-devices.png" alt-text="Görüntüyle.":::

1. Ses Yardımcısı uygulamanızın dağıtıldığı aygıtı seçin.

1. **Konuşma** sekmesini açın.

    :::image type="content" source="./media/manage-voice-assistant/device-page.png" alt-text="Görüntüyle.":::

1. **Anahtar sözcüğünün** yanındaki **Değiştir** ' e tıklayın.

    :::image type="content" source="./media/manage-voice-assistant/change-keyword-device.png" alt-text="Görüntüyle.":::

1. Kullanılabilir anahtar sözcüklerden birini seçin ve değişiklikleri uygulamak için **Kaydet** ' e tıklayın.

1. Azure Percept ses cihazı üzerindeki üç LED ışıkları, yapılandırma tamamlandığında ve ses yardımcınız kullanıma hazırlandığında parlak mavi (yanıp sönen) olarak değişir.

## <a name="create-a-custom-keyword"></a>Özel anahtar sözcük oluşturma

[Konuşma Studio](https://speech.microsoft.com/)ile, sesli yardımcınız için özel bir anahtar sözcük oluşturabilirsiniz. Temel bir özel anahtar sözcük modelinin eğitilmesi 30 dakikaya kadar sürer.

Özel anahtar sözcük oluşturma yönergeleri için [konuşma Studio belgelerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) izleyin. Yapılandırıldıktan sonra yeni anahtar kelimedir, Ses Yardımcısı uygulamanızla birlikte kullanılmak üzere Noel Cruz portalında proje için kullanılabilir olacaktır.

## <a name="commands-configuration"></a>Komutlar yapılandırması

Özel komutlar, sesli ilk etkileşim deneyimleri için iyileştirilmiş zengin sesli uygulama uygulamaları oluşturmayı kolaylaştırır. Özel komutlar, görev tamamlama veya komut ve denetim senaryoları için idealdir.

### <a name="configuration-within-the-voice-assistant-demo-window"></a>Ses Yardımcısı tanıtım penceresi içinde yapılandırma

1. Demo sayfasında **özel komut** ' ın yanındaki **Değiştir** ' e tıklayın. Demo sayfası açık değilse, cihaz sayfasına gidin (aşağıya bakın) ve tanıtım 'e erişmek için, **Eylemler** bölümünde **Ses yardımcınızı test et** ' e tıklayın.

1. Kullanılabilir özel komutlardan birini seçin ve değişiklikleri uygulamak için **Kaydet** ' e tıklayın.

### <a name="configuration-within-the-device-page"></a>Cihaz sayfası içinde yapılandırma

1. [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)'nun Genel Bakış sayfasında sol menü bölmesinde **cihazlar** ' a tıklayın.

1. Ses Yardımcısı uygulamanızın dağıtıldığı aygıtı seçin.

1. **Konuşma** sekmesini açın.

1. **Komutun** yanındaki **Değiştir** ' e tıklayın.

1. Kullanılabilir özel komutlardan birini seçin ve değişiklikleri uygulamak için **Kaydet** ' e tıklayın.

## <a name="create-custom-commands"></a>Özel komutlar oluşturma

[Konuşma Studio](https://speech.microsoft.com/)ile, ses yardımcınızın yürütülmesi için özel komutlar oluşturabilirsiniz.

Özel komutlar oluşturma yönergeleri için [konuşma Studio belgelerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-custom-commands-application) izleyin. Bir kez yapılandırıldıktan sonra, yeni komutlarınız, sesli yardım uygulamanızla birlikte kullanılmak üzere Azure Percept Studio 'da kullanıma sunulacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bir ses Yardımcısı uygulaması oluşturduktan sonra Azure Percept DK ile [kod içermeyen bir Vision çözümü](./tutorial-nocode-vision.md) geliştirmeyi deneyin.