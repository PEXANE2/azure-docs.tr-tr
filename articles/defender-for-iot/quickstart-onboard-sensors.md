---
title: "Hızlı başlangıç: IoT için Azure Defender 'daki sensörlerden katılın"
description: IoT dağıtımı için Azure Defender algılayıcılarının nasıl ekleneceğini öğrenin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2020
ms.author: rkarlin
ms.openlocfilehash: dafa2f231581ee7e7fa7a59a9c9320f6f2a67038
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357693"
---
# <a name="quickstart-deploy-and-onboard-a-sensor"></a>Hızlı başlangıç: bir algılayıcı dağıtın ve ekleyin

Bu makalede, algılayıcı dağıtım sürecine genel bir bakış sunulmaktadır. Algılayıcılar, IoT portalı için Azure Defender eklendi olmalıdır.

Bu işlem önceden yapılandırılmış bir algılayıcı almak veya bir sertifikalı algılayıcı gereci almak ve algılayıcı yazılımını kendiniz yüklemek istiyor.

Sertifikalı bir algılayıcı gereci ile çalışıyorsanız, başlamadan önce [IoT donanım belirtimleri Kılavuzu Için Azure Defender](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) ' ı gözden geçirmeniz önerilir.


> [!NOTE]
> IoT şirket içi yönetim konsolunun Azure Defender 'ın eklendi olması gerekmez.


Ekleme sensörleri şunları yapmanızı sağlar:

|||
|------ | ----------- |
| **Bir algılayıcı adı tanımlayın** | Ekleme yaptığınız sensöri adlandırın ve bir IoT Hub veya abonelikle ilişkilendirin.<br /><br />Daha fazla bilgi için bkz. **sensörlerden katılın** .|
|**Abonelik ve yürütülen cihaz sayısını seçin**|Abonelik kapsamındaki cihaz sayısını ve aboneliği seçin. Sayıyı 1000 artışlarla girin.|
| **Bir algılayıcı yönetim modu tanımlama** | Bir algılayıcı tarafından algılanan varlık, uyarı ve diğer bilgilerin nerede görüntülendiğini tanımlayın. Bu, tanımladığınız **algılayıcı yönetim moduna** göre belirlenir.<br /><br />**Yerel olarak yönetilen mod** : algılayıcı tarafından algılanan bilgiler algılayıcı konsolunda görüntülenir. Algılayıcı kendisine bağlıysa, algılama bilgileri de şirket içi yönetim konsoluyla paylaşılır.<br /><br />**Bulut yönetilen modu** : algılayıcı tarafından algılanan bilgiler algılayıcı konsolunda görüntülenir. Ayrıca, uyarı bilgileri bir IoT Hub aracılığıyla dağıtılır ve Azure Sentinel gibi diğer Azure hizmetleriyle de paylaşılabilir.<br />Daha fazla bilgi için bkz. **algılayıcısı** ekleme. |
| **Algılayıcı etkinleştirme dosyası alma** | **Yerel olarak yönetilen** sensörler için, yetkili algılayıcı etkinleştirme sürelerini yönetmek üzere bir etkinleştirme dosyası kullanılır.<br /><br />**Bulut yönetilen** algılayıcılar için, algılayıcı ve bir IoT Hub arasında bağlantı olarak bir etkinleştirme dosyası kullanılır. Daha fazla bilgi için bkz. **sensörlerden katılın**. |
| **Bir etkinleştirme dosyasını sensöre yükleme** | Etkinleştirme dosyaları kurumsal Sensörlerinizi karşıya yüklememelidir. Etkinleştirme dosyası karşıya yüklenene kadar ağ izleme ve algılayıcı konsol özelliklerine erişim kullanılamayacak. Daha fazla bilgi için bkz. **algılayıcı etkinleştirme dosyalarını karşıya yükleme**. |
| **Etkinleştirmeden önce algılayıcı ağ parametrelerini Güncelleştir** | Algılayıcı yüklemesi sırasında tanımlanan parametreleri güncelleştirin. Daha fazla bilgi için bkz. **algılayıcı etkinleştirme dosyalarını karşıya yükleme**.|


**Bir algılayıcı dağıtmak için:**

1. Azure portal IoT için Azure Defender ' a gidin.

2. Kullanmaya başlayın ' ı seçin **.**

3. **Ağınızı bulma** bölümünde **Ayarla** ' yı seçin.

   ![IoT için Azure Defender ağ görünümünüzü bulma](media/updates/image5.png)

4. Algılayıcı almak için bir seçenek belirleyin.

   ![IoT ağ algılayıcısı görünümü için Azure Defender](media/updates/image6.png)

  - **Önceden yapılandırılmış bir algılayıcı satın alın** : Microsoft ve ok, önceden yapılandırılmış algılayıcılar sağlamak için partnerin. Önceden yapılandırılmış bir algılayıcı satın almak için, şu adreste iletişim oku: <hardware.sales@arrow.com> . Algılayıcı, özelliğinize teslim edilecek. En güncel sürüm yüklendi.

  - **Kendi gerecinizi getir (ISO yüklemesi)** : çözüm sertifikalı gereçler üzerinde çalışır. Sertifikalı gerecinizi satın alırken [IoT Için Azure Defender 'ı donanım belirtimleri kılavuzunu](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) kullanın.

    - **Sürüm Seç** menüsünden bir sürüm seçin.

    - **İndir** ve dosyayı Kaydet ' i seçin. ISO görüntüsünü indirme ve algılayıcı yazılımını yükleme hakkında ayrıntılı bilgi için **IoT Için Azure Defender Yükleme Kılavuzu** 'na bakın.

5. Yazılım sensöre yüklendikten sonra veya önceden yapılandırılmış bir algılayıcı aldıysanız, ağ kurulum görevlerini gerçekleştirin. Ayrıntılar için [Ağ Kurulum kılavuzuna](https://aka.ms/AzureDefenderForIoTNetworkSetup) bakın.

## <a name="onboard-a-sensor"></a>Bir algılayıcı ekleme

Algılayıcılar 'ın IoT portalı için Azure Defender eklendi olması gerekir. Ekleme iki aşamada gerçekleştirilir:

1. Algılayıcıyı IoT portalı için Azure Defender ile kaydedin.

2. Algılayıcı için bir etkinleştirme dosyası indirin. Dosya daha sonra sensöre yüklenecektir.

**Bu sayfaya erişmek için:**

1. **IoT Başlarken** sayfası Için Microsoft Azure Defender ' a gidin.

2. IoT için Azure Defender 'dan **yerleşik olarak 2** . seçeneğini belirleyin.

   ![Fazla pano sayfa görünümünün ekran görüntüsü](media/updates/image7.png)

3. Ekleme **algılayıcısı** sayfası açılır. Algılayıcısı kaydetmek ve etkinleştirme dosyasını indirmek için seçenekler sağlar.

   ![Fazla pano algılayıcısı Sayfa görünümünün ekran görüntüsü](media/quickstart/onboard-sensors.png)

**Algılayıcıyı kaydetmek için:**

1. Bir algılayıcı adı seçin. Bulut tarafından yönetilen sensörler için, burada tanımlanan ad, algılayıcı konsolunda görünen adı uygulanır: Bu ad konsolundan değiştirilemez. Yerel olarak yönetilen sensörler için, burada uygulanan AD Azure 'da depolanır, ancak algılayıcı konsolunda güncelleştirilebilecek. Adının bir parçası olarak yüklediğiniz algılayıcının IP adresini eklemeniz veya kolayca tanımlanabilir bir ad kullanmanız önerilir. Bu işlem, IoT portalındaki Azure Defender 'daki kayıt adı ve algılayıcı konsolunda görünen dağıtılan sensör IP 'si arasında daha kolay izleme ve tutarlı adlandırma sağlar.

2. Abonelik açılan listesinden bir abonelik seçin.
3. **Taahhüt edilen cihazlar** alanına aboneliğin kapsadığı cihaz sayısını girin. Aynı abonelikle ilişkili olan her bir algılayıcı için bu numarayı yeniden girmeniz gerekir.  Örneğin, "A" aboneliği 6000 cihazlarıyla ilişkiliyse, "A" aboneliğiyle ilişkilendirilen her algılayıcı için 6000 girin.   
4. **Bulut bağlantısı** geçişi kullanarak bir algılayıcı yönetimi modu seçin. Geçiş açık ise, algılayıcı **bulutta yönetilir**. Geçiş kapalıysa, algılayıcı **yerel olarak yönetilir**.


| Algılayıcı yönetim modu | Description                                                |
| ---------------------- | ---------------------------------------------------------  |
| **Bulut yönetiliyor**          | Algılayıcı tarafından algılanan bilgiler algılayıcı konsolunda görüntülenir. Ayrıca, uyarı bilgileri bir IoT Hub aracılığıyla dağıtılır ve Azure Sentinel gibi diğer Azure hizmetleriyle de paylaşılabilir.<br /><br />Bu sensör ile ilişkilendirilecek IoT Hub seçin.<br /><br />Bulut tarafından yönetilen sensörlerle bulut tarafından yönetilen bir etkinleştirme dosyası yüklemeniz gerekir.<br /><br />Ayrıntılar için bkz. **algılayıcı etkinleştirme dosyalarını karşıya yükleme** . |
| **Yerel olarak yönetilen**        | Yerel olarak yönetilen sensörlerle algılanan bilgiler algılayıcı konsolunda görüntülenir. Bir AIR-gapped ağında çalışıyorsanız ve birden çok yerel olarak yönetilen algılayıcı tarafından algılanan tüm bilgilerin birleştirilmiş bir görünümünü istiyorsanız, şirket içi yönetim konsoluyla çalışın.<br /><br />*Yerel olarak yönetilen* sensörler bir Azure aboneliğiyle ilişkilendirilir ve etkinleştirme süre sonu döneminin algılayıcısı hakkında yönergeler içerir.<br /><br />Bu sensörle ilişkilendirilecek bir abonelik seçin.<br /><br />Her sensöre yerel olarak yönetilen bir etkinleştirme dosyası yüklemeniz gerekir. Ayrıntılar için bkz. **algılayıcı etkinleştirme dosyalarını karşıya yükleme** . |

5. **Kaydet** ’i seçin.

6. Etkinleştirme dosyasını Indir sayfasında etkinleştirme dosyasını **İndir** ' i seçin.

   ![Ekleme algılayıcısı görünümünün ekran görüntüsü](media/updates/image9.png)

7. Dosyayı kaydedin. Dosya adı şu biçimdedir: `<hub_name>_<sensor_name>` . Algılayıcı adı, yukarıda tanımladığınız adı ifade eder.

8. **Son** ’u seçin.

9. Algılayıcı ekleme işlemini gerçekleştirmek için, IoT için Azure Defender konsolundan dosyayı sensöre yükleyin.
 
## <a name="upload-a-sensor-activation-file"></a>Bir algılayıcı etkinleştirme dosyasını karşıya yükle

Bu makalede, bir etkinleştirme dosyasının sensöre nasıl yükleneceği açıklanır. Algılayıcıyı eklerken dosyayı almış olmanız gerekir.

**Yerel olarak yönetilen etkinleştirme dosyaları**

Yerel olarak yönetilen algılayıcılar bir Azure aboneliğiyle ilişkilendirilir.  Yerel olarak yönetilen sensörlerinizin etkinleştirme dosyası bir sona erme tarihi içerir. Bu tarihten bir ay önce, algılayıcı konsolunun en üstünde bir uyarı iletisi görünür. Uyarı, etkinleştirme dosyasını güncelleştirdikten sonra kalır.

Etkinleştirme dosyasının süresi dolduktan sonra IoT özellikleri için Azure Defender ile çalışmaya devam edebilirsiniz.

**Bulut tarafından yönetilen etkinleştirme dosyaları**

Bulut tarafından yönetilen sensörler bir Azure IoT Hub ile ilişkilidir. Bu algılayıcılar, etkinleştirme dosyası zaman dilimleriyle sınırlı değildir. Bulut tarafından yönetilen sensörlerle ilgili etkinleştirme dosyası IoT Hub bağlantı sağlamak için kullanılır.

IoT Hub 'Ları hakkında daha fazla bilgi için bkz. [IoT Hub](../iot-hub/about-iot-hub.md).

**Etkinleştirme dosyasını karşıya yüklemek için:**

1. Şunları olduğunu doğrulayın:

   - Yükleme sırasında tanımlanan algılayıcı için IP adresi

   - Algılayıcı için Kullanıcı oturum açma kimlik bilgileri gerekli

2. Bu algılayıcı için IP adresini kullanarak tarayıcınızdan IoT için Azure Defender konsoluna erişin.

3. IoT algılayıcı konsolunda Azure Defender ' da oturum açın.

   ![IoT konsolu için Azure Defender oturum açma görünümü](media/updates/image11.png)

4. Başarılı oturum açma işleminden sonra etkinleştirme ekranı açılır. Karşıya yükle ' yi seçin ve kaydettiğiniz etkinleştirme dosyasını seçin.

   ![IoT etkinleştirme görünümü için Azure Defender](media/updates/image12.png)

5. Hüküm ve koşulları onaylayın.

6. **Etkinleştir** ' i seçin. Algılayıcı ağı yapılandırma parametreleri, yazılım yüklemesi sırasında veya önceden yapılandırılmış bir algılayıcı satın alındığında tanımlanmıştır. Aşağıdaki parametreler tanımlandı:
   - IP Adresi
   - DNS  
   - Varsayılan ağ geçidi
   - Alt ağ maskesi
   - Konak adı
 
   Algılayıcıyı etkinleştirmeden önce bu bilgileri güncellemek isteyebilirsiniz çünkü:
   - Tanımlanan önceden yapılandırılmış parametreleri değiştirmeniz gerekir  
   - Ağ parametrelerini yükleme sonrası olarak yeniden yapılandırmak istiyorsanız, sensörinizi etkinleştirmeden önce proxy ayarlarını da tanımlayabilirsiniz.

7. Etkinleştirme iletişim kutusunu, **algılayıcı ağ yapılandırması** bağlantısını seçin.  

   ![Ağ yapılandırması düzenleme görünümünün ekran görüntüsü](media/updates/image13.png)

8. Yükleme sırasında tanımlanan parametreler görüntülenir. Ayrıca bir seçenek proxy 'yi tanımlar. Gerekirse güncelleştirin ve **Kaydet** ' i seçin.
 
 
## <a name="next-steps"></a>Sonraki adımlar

Bu makalede sensörlerden dağıtmayı ve bunları oluşturmayı öğrendiniz. Kullanmaya başlama hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

- [Başlarken](getting-started.md)
