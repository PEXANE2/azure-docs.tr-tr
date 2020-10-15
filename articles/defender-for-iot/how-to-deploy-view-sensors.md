---
title: IoT için Azure Defender 'da eklendi sensörleri görüntüleme ve yönetme
description: Bu makalede, IoT için Azure Defender 'daki eklendi sensörleri için yeni etkinleştirme dosyalarını nasıl görüntüleyebileceğiniz ve silebileceği de açıklanmaktadır.
author: rkarlin
ms.author: rkarlin
ms.date: 10/10/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 6aec19d413e1730bc1599f6cbac1c62e9b304ecd
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094526"
---
# <a name="view-and-manage-onboarded-sensors"></a>Eklendi sensörleri görüntüleme ve yönetme

Bu makalede, eklendi sensörlarının nasıl görüntüleneceği ve silineceği ve eklendi sensörleri için yeni etkinleştirme dosyalarının nasıl indirileceği açıklanır.

## <a name="update-sensor-management-mode"></a>Algılayıcı yönetimi modunu Güncelleştir

Sensörizin yönetilip yönetilme modunu güncelleştirmek isteyebilirsiniz. Bunu yaptığınızda, algılayıcı yönetimi sayfasından geçerli algılayıcıyı kaldırmanız ve yeni bir etkinleştirme dosyası yüklemeniz gerekir.

- **Yerel olarak yönetilen mod yerine bulut tarafından yönetilen modda çalışın**: bulut tarafından yönetilen bir algılayıcı için bir etkinleştirme dosyası ile yerel olarak yönetilen sensörle ilgili etkinleştirme dosyasını güncelleştirin. Yeniden etkinleştirmeyi tamamladıktan sonra, algılayıcı algılamaları hem algılayıcı hem de IoT portalı için Azure Defender 'da görüntülenir. Yeniden etkinleştirme dosyası başarıyla karşıya yüklendikten sonra, yeni algılanan uyarı bilgileri Azure 'a gönderilir.

- **Bulut yönetilen modu yerine yerel olarak yönetilen modda çalışın**: bulut tarafından yönetilen bir algılayıcı için etkinleştirme dosyasını yerel olarak yönetilen bir algılayıcı için etkinleştirme dosyasıyla güncelleştirin. Yeniden etkinleştirmeyi tamamladıktan sonra, algılayıcı algılama bilgileri yalnızca sensörde görüntülenir.

- **Algılayıcıyı yeni bir IoT Hub ilişkilendir**: sensörinizi yeni bir IoT Hub ilişkilendirmek isteyebilirsiniz. Bunu yapmak için yeniden kaydedin ve yeni bir etkinleştirme dosyası yükleyin.

**Algılayıcıyı yeniden etkinleştirmek için:**

1. IoT için Azure Defender, **algılayıcı yönetimi** sayfasına gidin.

2. Yeni bir etkinleştirme dosyası yüklemek istediğiniz algılayıcıyı seçin.

3. Silin.

4. **Ekleme** sayfasından yeni modda veya yeni bir IoT Hub algılayıcısı yeniden ekleyin.

5. Etkinleştirme dosyasını **Indirme yükleme** sayfasından indirin.

6. IoT algılayıcı konsolunda Azure Defender ' da oturum açın.

7. Algılayıcı konsolunda, **sistem ayarları** ' nı seçin ve yeniden **etkinleştirme**' yi seçin.

   ![Yeniden etkinleştirme görünümünün ekran görüntüsü](media/updates/image14.png)

8. **Karşıya yükle** ' yi seçin ve kaydettiğiniz dosyayı seçin.

9. **Etkinleştir**' i seçin.
 
## <a name="delete-sensors"></a>Algılayıcıları Sil

Yönetilen bir bulut algılayıcısı silerseniz, bilgiler IoT Hub gönderilmez.

Artık bunlarla çalışmadığınızda yerel olarak yönetilen algılayıcıları silin.

**Algılayıcıları silmek için:**

1. IoT algılayıcısı için Azure Defender **Yönetim** sayfasına gidin.

2. Silmek istediğiniz sensörleri seçin.

3. **Algılayıcıyı Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için, modül yapılandırması için nasıl yapılır kılavuzuna ilerleyin.
> [!div class="nextstepaction"]
> [Modül yapılandırması nasıl yapılır Kılavuzu](./how-to-agent-configuration.md)
