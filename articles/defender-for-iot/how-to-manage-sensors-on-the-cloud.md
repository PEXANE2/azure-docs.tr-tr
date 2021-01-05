---
title: IoT portalı için Defender 'daki algılayıcı ekleme ve yönetme
description: IoT portalı için Defender 'da sensöri ekleme, görüntüleme ve yönetme hakkında bilgi edinin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/27/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: a763d8b65049cd9f301379c2c038a1d799114653
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97841891"
---
# <a name="onboard-and-manage-sensors-in-the-defender-for-iot-portal"></a>IoT portalı için Defender 'daki algılayıcı ekleme ve yönetme

Bu makalede, IoT portalı için Defender 'da algılayıcıları ekleme, görüntüleme ve yönetme işlemlerinin nasıl yapılacağı açıklanır.

## <a name="onboard-sensors"></a>Algılayıcı ekleme

IoT için Azure Defender ile kaydederek bir algılayıcı ekleme ve bir algılayıcı etkinleştirme dosyası indirme.

### <a name="register-the-sensor"></a>Algılayıcıyı Kaydet

Kaydolmak için:

1. IoT portalındaki Defender 'daki **hoş geldiniz** sayfasına gidin.
1. **Yerleşik algılayıcı** seçin.
1. Bir algılayıcı adı oluşturun. Adının bir parçası olarak yüklediğiniz sensör IP adresini dahil etmenizi veya kolayca tanımlanabilir bir ad kullanmanızı öneririz. Bu, IoT portalındaki Azure Defender 'daki kayıt adı ve algılayıcı konsolunda görüntülenecek olan dağıtılan algılayıcının IP 'si ile ilgili daha kolay izleme ve tutarlı adlandırma sağlar.
1. Algılayıcıyı bir Azure aboneliğiyle ilişkilendirin.
1. **Buluta bağlı** geçişi kullanarak bir algılayıcı yönetimi modu seçin. Geçiş açık ise, algılayıcı buluta bağlanır. Geçiş kapalıysa, algılayıcı yerel olarak yönetilir.

   - **Buluta bağlı algılayıcılar**: algılayıcıyı algılayan bilgiler algılayıcı konsolunda görüntülenir. Uyarı bilgileri bir IoT Hub 'ı aracılığıyla dağıtılır ve Azure Sentinel gibi diğer Azure hizmetleriyle paylaşılabilir.

   - **Yerel olarak yönetilen sensörler**: algılayıcıyı algılayan bilgiler algılayıcı konsolunda görüntülenir. Bir AIR-gapped ağında çalışıyorsanız ve birden çok yerel olarak yönetilen algılayıcı tarafından algılanan tüm bilgilerin birleştirilmiş bir görünümünü istiyorsanız, şirket içi yönetim konsoluyla çalışın.

   Buluta bağlı sensörler için, ekleme sırasında tanımlanan ad, algılayıcı konsolunda görünen addır. Bu adı konsolundan doğrudan değiştiremezsiniz. Yerel olarak yönetilen sensörler için, ekleme sırasında uygulanan AD Azure 'da depolanır ve bu, algılayıcı konsolunda güncelleştirilebilen olabilir.

1. Bu algılayıcı ve IoT için Azure Defender arasında bir ağ geçidi olarak kullanılacak bir IoT Hub 'ı seçin.
1. Algılayıcı buluta bağlıysa, bir IoT Hub ile ilişkilendirin ve ardından bir site adı ve bölgesi tanımlayın. Açıklayıcı Etiketler de ekleyebilirsiniz. Site adı, bölge ve Etiketler, [siteler ve algılayıcılar sayfasında](#view-onboarded-sensors)açıklayıcı girişlerdir.

### <a name="download-the-sensor-activation-file"></a>Algılayıcı etkinleştirme dosyasını indirin

Algılayıcı etkinleştirme dosyası, sensör yönetim moduyla ilgili yönergeleri içerir. Dağıttığınız her algılayıcı için benzersiz bir etkinleştirme dosyası indirirler. Algılayıcı konsolunda ilk kez oturum açan bir Kullanıcı, etkinleştirme dosyasını sensöre yükler.

Bir etkinleştirme dosyasını indirmek için:

1. **Algılayıcısı** ekleme sayfasında, **etkinleştirme dosyasını indir**' i seçin.
1. Dosyayı ilk kez algılayıcı konsolunda oturum açan kullanıcı için erişilebilir yapın.

## <a name="view-onboarded-sensors"></a>Eklendi sensörleri görüntüle

IoT Portal için Defender 'da, eklendi sensörleri hakkındaki temel bilgileri görüntüleyebilirsiniz. 

1. **Siteleri ve Algılayıcılar ' ı** seçin.
1. **Siteler ve algılayıcılar** sayfasında, ihtiyacınız olan algılayıcı bilgilerini bulmak için filtre ve arama araçları ' nı kullanın.

Kullanılabilir bilgiler şunları içerir:

- Kaç sensör eklendi
- Buluta bağlı ve yerel olarak yönetilen sensörların sayısı
- Bir eklendi algılayıcısı ile ilişkili Merkez
- Ekleme sırasında sensöre atanan ad, sensörle ilişkilendirilen bölge veya etiketlerle eklenen diğer açıklayıcı bilgiler gibi bir algılayıcı hakkında daha fazla bilgi eklendi

## <a name="manage-onboarded-sensors"></a>Eklendi sensörleri yönetme

Sensörlerden ilgili yönetim görevleri için, IoT portalı için Defender 'ı kullanın.

### <a name="export"></a>Dışarı Aktarma

Eklendi algılayıcı bilgilerini dışarı aktarmak için, **siteler ve algılayıcılar** sayfasının en üstündeki **dışarı aktar** simgesini seçin.

### <a name="edit"></a>Düzenle

Site adı, bölge ve Etiketler eklemek ve düzenlemek için **siteler ve sensör** düzenleme araçlarını kullanın.

### <a name="delete"></a>Sil

Buluta bağlı bir algılayıcı silerseniz, bilgiler IoT Hub 'ına gönderilmez. Artık bunlarla çalışmadığınızda, yerel olarak bağlı sensörlerden silin.

Bir algılayıcıyı silmek için:

1. Silmek istediğiniz algılayıcı için üç nokta (**...**) simgesini seçin. 
1. Silme işlemini onaylayın.

### <a name="reactivate"></a>Yeniden etkinleştirme

Sensörizin yönetilip yönetilme modunu güncelleştirmek isteyebilirsiniz. Örnek:

- **Yerel olarak yönetilen mod yerine buluta bağlı modda çalışın**: bunu yapmak için, yerel olarak bağlı sensörizin etkinleştirme dosyasını, buluta bağlı bir algılayıcı için bir etkinleştirme dosyasıyla güncelleştirin. Yeniden etkinleştirmeyi tamamladıktan sonra, algılayıcı algılamaları hem sensör hem de IoT için Defender portalında görüntülenir. Yeniden etkinleştirme dosyası başarıyla karşıya yüklendikten sonra, yeni algılanan uyarı bilgileri Azure 'a gönderilir.

- **Buluta bağlı mod yerine yerel olarak bağlı modda çalışın**: bunu yapmak için, bulut bağlantılı bir algılayıcı için etkinleştirme dosyasını yerel olarak yönetilen bir algılayıcı için etkinleştirme dosyası ile güncelleştirin. Yeniden etkinleştirmeyi tamamladıktan sonra, algılayıcı algılama bilgileri yalnızca sensörde görüntülenir.

- **Algılayıcıyı yeni bir IoT Hub 'ı Ile ilişkilendirin**: bunu yapmak için yeniden kaydedin ve yeni bir etkinleştirme dosyası yükleyin.

Bir algılayıcıyı yeniden etkinleştirmek için:

1. IoT portalı için Defender 'daki **siteler ve algılayıcılar** sayfasına gidin.

2. Yeni bir etkinleştirme dosyası yüklemek istediğiniz algılayıcıyı seçin.

3. Algılayıcıyı silin.

4. Yeni moddaki veya yeni bir IoT Hub ile **ekleme** sayfasından algılayıcısı yeniden ekleyin.

5. Etkinleştirme dosyasını **Indirme yükleme** sayfasından indirin.

6. IoT algılayıcısı konsolunda Defender 'da oturum açın.

7. Algılayıcı konsolunda, **sistem ayarları** ' nı seçin ve yeniden **etkinleştirme**' yi seçin.

   :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/reactivate.png" alt-text="Algılayıcıyı yeniden etkinleştirmek için etkinleştirme dosyanızı karşıya yükleyin.":::

8. **Karşıya yükle** ' yi seçin ve kaydettiğiniz dosyayı seçin.

9. **Etkinleştir**' i seçin. 

## <a name="see-also"></a>Ayrıca bkz.

[Algılayıcıyı etkinleştirme ve ayarlama](how-to-activate-and-set-up-your-sensor.md)
