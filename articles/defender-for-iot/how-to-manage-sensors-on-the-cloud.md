---
title: IoT portalı için Defender 'da algılayıcılar ve abonelikler ekleme ve yönetme
description: IoT portalı için Defender 'da sensöri ekleme, görüntüleme ve yönetme hakkında bilgi edinin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 2/18/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 63b3b450e289b40aa9acbfb0d5170e8eb57f9e58
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101733270"
---
# <a name="onboard-and-manage-sensors-and-subscriptions-in-the-defender-for-iot-portal"></a>IoT portalı için Defender 'da algılayıcılar ve abonelikler ekleme ve yönetme

Bu makalede, [IoT portalı Için Defender](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started)'da algılayıcıları ekleme, görüntüleme ve yönetme işlemlerinin nasıl yapılacağı açıklanır.

## <a name="onboard-sensors"></a>Algılayıcı ekleme

IoT için Azure Defender ile kaydederek bir algılayıcı ekleme ve bir algılayıcı etkinleştirme dosyası indirme.

### <a name="register-the-sensor"></a>Algılayıcıyı Kaydet

Kaydolmak için:

1. [IoT portalındaki Defender](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started)'Daki **hoş geldiniz** sayfasına gidin.
1. **Yerleşik algılayıcı** seçin.
1. Bir algılayıcı adı oluşturun. Adının bir parçası olarak yüklediğiniz sensör IP adresini dahil etmenizi veya kolayca tanımlanabilir bir ad kullanmanızı öneririz. Bu, [IoT portalındaki Azure Defender](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) 'daki kayıt adı ve algılayıcı konsolunda görüntülenecek olan dağıtılan algılayıcının IP 'si ile ilgili daha kolay izleme ve tutarlı adlandırma sağlar.
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

[IoT Portal Için Defender](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started)'da, eklendi sensörleri hakkındaki temel bilgileri görüntüleyebilirsiniz.

1. **Siteleri ve Algılayıcılar ' ı** seçin.
1. Gereken algılayıcı ve tehdit bilgileri bilgilerini bulmak için filtre ve arama araçlarını kullanın.

- Kaç sensör eklendi
- Buluta bağlı ve yerel olarak yönetilen sensörların sayısı
- Bir eklendi algılayıcısı ile ilişkili Merkez
- Ekleme sırasında sensöre atanan ad, sensörle ilişkilendirilen bölge veya etiketlerle eklenen diğer açıklayıcı bilgiler gibi bir algılayıcı hakkında daha fazla bilgi eklendi

## <a name="manage-onboarded-sensors"></a>Eklendi sensörleri yönetme

Sensörlerden ilgili yönetim görevleri için, [IoT portalı Için Defender 'ı](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) kullanın.

Eklendi sensörleri **siteler ve algılayıcılar** sayfasında görüntülenebilir. Ayrıca, bu sayfadan algılayıcı bilgilerini düzenleyebilirsiniz.

### <a name="export-sensor-details"></a>Algılayıcı ayrıntılarını dışarı aktar

Eklendi algılayıcı bilgilerini dışarı aktarmak için, **siteler ve algılayıcılar** sayfasının en üstündeki **dışarı aktar** simgesini seçin.

### <a name="edit-sensor-zone-details"></a>Algılayıcı bölgesi ayrıntılarını Düzenle

Algılayıcı adını ve bölgesini düzenlemek için **siteler ve algılayıcılar** düzenleme seçeneklerini kullanın.

Düzenlemek için:

1. Düzenlemek istediğiniz algılayıcı için **üç nokta** (**...**) simgesini seçin.
1. **Düzenle**'yi seçin.
1. Algılayıcı bölgesini güncelleştirin veya yeni bir bölge oluşturun.

### <a name="delete-a-sensor"></a>Algılayıcıyı silme

Buluta bağlı bir algılayıcı silerseniz, bilgiler IoT Hub 'ına gönderilmez. Artık bunlarla çalışmadığınızda, yerel olarak bağlı sensörlerden silin.

Bir algılayıcıyı silmek için:

1. Silmek istediğiniz algılayıcı için üç nokta (**...**) simgesini seçin.
1. Silme işlemini onaylayın.

### <a name="reactivate-a-sensor"></a>Algılayıcıyı yeniden etkinleştirme 

Şunları yapmak istediğiniz için sensörinizi yeniden etkinleştirmeniz gerekebilir:

- **Yerel olarak yönetilen mod yerine buluta bağlı modda çalışın**: yeniden etkinleştirmeyi tamamladıktan sonra, algılayıcı algılamaları sensörde görüntülenir ve yeni algılanan uyarı bilgileri IoT Hub 'ı aracılığıyla gönderilir. Bu bilgiler, Azure Sentinel gibi diğer Azure hizmetleriyle paylaşılabilir.

- **Bulut bağlantılı mod yerine yerel olarak yönetilen modda çalışın**: yeniden etkinleştirmeyi tamamladıktan sonra, algılayıcı algılama bilgileri yalnızca sensörde görüntülenir.

- **Algılayıcıyı yeni bir IoT Hub 'ı Ile ilişkilendirin**: bunu yapmak için, algılayıcıyı yeni bir hub ile yeniden kaydedin ve ardından yeni bir etkinleştirme dosyası indirin.

Bir algılayıcıyı yeniden etkinleştirmek için:

1. [IoT portalı Için Defender](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started)'daki **siteler ve algılayıcılar** sayfasına gidin.

2. Yeni bir etkinleştirme dosyası yüklemek istediğiniz algılayıcıyı seçin.

3. Algılayıcıyı silin.

4. Başlangıç sayfasından **bir algılayıcı** Ekle ' ye tıklayarak algılayıcıyı yeni modda veya yeni bir IoT Hub 'ı ile yeniden ekleyin.

5. Etkinleştirme dosyasını indirin.

1. IoT algılayıcısı konsolunda Defender 'da oturum açın.

7. Algılayıcı konsolunda, **sistem ayarları** ' nı seçin ve yeniden **etkinleştirme**' yi seçin.

   :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/reactivate.png" alt-text="Algılayıcıyı yeniden etkinleştirmek için etkinleştirme dosyanızı karşıya yükleyin.":::

8. **Karşıya yükle** ' yi seçin ve ekleme algılayıcısı sayfasından kaydettiğiniz dosyayı seçin.

9. **Etkinleştir**' i seçin.

## <a name="offboard-a-subscription"></a>Abonelik çıkarma

Abonelikler aylık olarak yönetilir. Bir aboneliği boşaltdığınızda, ayın sonuna kadar bu abonelik için faturalandırılırsınız. 

Aboneliği boşaltmadan önce abonelikle ilişkili tüm algılayıcıları kaldırın. Algılayıcıyı silme hakkında daha fazla bilgi için bkz. [algılayıcı silme](#delete-a-sensor). 

Bir aboneliği bırakmak için:

1. **Fiyatlandırma** sayfasına gidin.
1. Aboneliği seçin ve ardından **Sil** simgesini seçin :::image type="icon" source="media/how-to-manage-sensors-on-the-cloud/delete-icon.png" border="false"::: .
1. Onay açılan penceresinde, abonelikle ilişkili tüm algılayıcıları sildiğini onaylamak için onay kutusunu işaretleyin.

    :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/offboard-popup.png" alt-text="Onay kutusunu seçin ve sensöriz için kapalı Pano ' yı seçin.":::

1. **Kapalı panosunu** seçin. 

Şirket içi ortam etkilenmez, ancak ilgili verilerin şirket içi yönetim konsoluna akmasını engellemek için algılayıcıyı şirket içi ortamdan kaldırmanız veya algılayıcıyı başka bir aboneliğe yeniden atamanız gerekir. 

## <a name="see-also"></a>Ayrıca bkz.

[Algılayıcınızı etkinleştirme ve ayarlama](how-to-activate-and-set-up-your-sensor.md)
