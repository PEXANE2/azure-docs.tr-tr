---
title: Azure IoT Central Azure IoT Edge bir cihaz ekleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanıza Azure IoT Edge bir cihaz ekleyin
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: e5d60c77e9bdc0733c12bca891eb6c3e33a1fceb
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74979079"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanıza Azure IoT Edge bir cihaz ekleme

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu öğretici, Azure IoT Central uygulamanıza bir Azure IoT Edge cihazının nasıl ekleneceğini ve yapılandırılacağını gösterir. Bu öğreticide, Azure Marketi 'nden IoT Edge özellikli bir Linux VM seçtik.

Bu öğretici iki bölümden oluşur:

* İlk olarak, bir operatör olarak, bir IoT Edge cihazının bulut ilk sağlamasını yapmayı öğrenirsiniz.
* Daha sonra, bir IoT Edge cihazının "ilk cihaz" sağlamasını yapmayı öğrenirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir IoT Edge cihaz ekleme
> * IoT Edge cihazı, paylaşılan erişim imzası (SAS) anahtarı kullanarak sağlamaya yardımcı olacak şekilde yapılandırın
> * IoT Central panoları ve modül durumunu görüntüleme
> * IoT Edge cihazda çalışan bir modüle Komut gönder
> * IoT Edge cihazda çalışan bir modülün özelliklerini ayarlama

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure IoT Central uygulamanızın olması gerekir. [Azure IoT Central uygulaması oluşturmak için bu hızlı](./quick-deploy-iot-central.md)başlangıcı izleyin.

## <a name="enable-azure-iot-edge-enrollment-group"></a>Azure IoT Edge kayıt grubunu etkinleştir
**Yönetim** sayfasından Azure IoT Edge kayıt grubu için SAS anahtarlarını etkinleştirin.

![Cihaz bağlantısı vurgulanmış şekilde Yönetim sayfasının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/groupenrollment.png)

## <a name="provision-a-cloud-first-azure-iot-edge-device"></a>"Bulut ilk" Azure IoT Edge cihaz sağlama  
Bu bölümde, ortam algılayıcı şablonunu kullanarak yeni bir IoT Edge cihaz oluşturur ve bir cihaz hazırlarsınız. **Cihazlar** > **ortam algılayıcı şablonu**' nu seçin. 

![Ortam algılayıcı şablonu vurgulanmış olarak cihazlar sayfasının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/deviceexplorer.png)

**+ Yeni**' yi seçin ve seçtiğiniz BIR cihaz kimliği ve adı girin. **Oluştur**'u seçin.

![Cihaz KIMLIĞI ve vurgulanmış olarak yeni cihaz oluştur iletişim kutusunun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfdevicecredentials.png)

Cihaz **kayıtlı** moda gider.

![Cihaz durumu vurgulanmış şekilde ortam algılayıcısı şablon sayfasının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfregistered.png)

## <a name="deploy-an-iot-edge-enabled-linux-vm"></a>IoT Edge özellikli bir Linux sanal makinesi dağıtma

> [!NOTE]
> Herhangi bir makineyi veya cihazı kullanmayı seçebilirsiniz. İşletim sistemi Linux veya Windows olabilir.

Bu öğreticide, Azure 'da oluşturulan bir Azure IoT etkin Linux sanal makinesi kullanıyoruz. [Azure Marketi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview)'Nde **Şimdi al**' ı seçin. 

![Azure Marketi 'nin, ŞIMDI al vurgulanarak ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfmarketplace.png)

**Devam**'ı seçin.

![Bu uygulamayı Azure 'da oluştur iletişim kutusunda, devam eden vurgulanan ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfmarketplacecontinue.png)


Azure portal aldınız. **Oluştur**'u seçin.

![Vurgulanan oluştur ile Azure portal ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfubuntu.png)

**Abonelik**' ı seçin, yeni bir kaynak grubu oluşturun ve VM kullanılabilirliği için **Batı ABD 2 (US)** seçin. Ardından Kullanıcı ve parola bilgilerini girin. Bunlar gelecekteki adımlarda gerekli olacaktır, bu nedenle bunları unutmayın. **İncele ve oluştur**’u seçin.

![Çeşitli seçenekler vurgulanmış şekilde, sanal makine ayrıntıları oluşturma sayfasının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfvm.png)

Doğrulamadan sonra **Oluştur**' u seçin.

![Doğrulama başarılı ve vurgulanmış olarak bir sanal makine oluşturma sayfasının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfvmvalidated.png)

Kaynakların oluşturulması birkaç dakika sürer. **Kaynağa Git**' i seçin.

![Kaynak vurgulanacak şekilde dağıtım tamamlama sayfasının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfvmdeploymentcomplete.png)

### <a name="provision-vm-as-an-iot-edge-device"></a>IoT Edge cihaz olarak VM sağlama 

**Destek + sorun giderme**altında **seri konsol**' yi seçin.

![Seri konsol vurgulanmış olan destek ve sorun giderme seçeneklerinin ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfserialconsole.png)

Aşağıdakine benzer bir ekran görürsünüz:

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsole.png)

ENTER tuşuna basın, istendiğinde kullanıcı adını ve parolayı girin ve ardından ENTER tuşuna basın. 

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsolelogin.png)

Bir komutu yönetici olarak (Kullanıcı "kök") çalıştırmak için şunu girin: **sudo su –**

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfsudo.png)

IoT Edge çalışma zamanı sürümünü denetleyin. Bu yazma sırasında geçerli GA sürümü 1.0.8 ' dir.

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsoleversion.png)

VIM düzenleyicisini ya da tercih ediyorsanız nano 'yi kullanın. 

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsolevim.png)

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfvim.png)

IoT Edge config. YAML dosyasını düzenleyin.

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsoleconfig.png)

Aşağı kaydırın ve YAML dosyasının bağlantı dizesi bölümünü not edin. 

**Önce**

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioning.png)

**Sonra** (ESC tuşuna basın ve küçük harf a tuşlarına basarak düzenleyebilirsiniz.)

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioningcomments.png)

YAML dosyasının simetrik anahtar bölümünün açıklamasını kaldırın. 

**Önce**

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmcomments.png)

**Sonra**

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmuncomments.png)

IoT Central gidin. IoT Edge cihazının kapsam KIMLIĞINI, cihaz KIMLIĞINI ve simetrik anahtarını alın.
çeşitli cihaz bağlantısı seçenekleri vurgulanmış şekilde IoT Central ![ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfdeviceconnect.png)

Linux bilgisayarına gidin ve kapsam KIMLIĞINI ve kayıt KIMLIĞINI cihaz KIMLIĞI ve simetrik anahtarla değiştirin.

ESC tuşuna basın ve şunu yazın **: WQ!** . Değişikliklerinizi kaydetmek için ENTER tuşuna basın.

Değişikliklerinizi işlemek için IoT Edge yeniden başlatın ve ENTER tuşuna basın.

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfrestart.png)

**İotedge listesini**yazın. Birkaç dakika sonra dağıtılan üç modül görürsünüz.

![Konsolun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfconsolemodulelist.png)


## <a name="iot-central-device-explorer"></a>Cihaz Gezgini IoT Central 

IoT Central, cihazınız sağlanan duruma getirilir.

![Cihaz durumu vurgulanmış şekilde IoT Central cihaz seçeneklerinin ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfprovisioned.png)

**Modüller** sekmesi IoT Central üzerindeki cihaz ve modülün durumunu gösterir. 

![IoT Central modülleri sekmesinin ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/cfiotcmodulestatus.png)


Bulut özelliklerini, önceki adımlarda oluşturduğunuz cihaz şablonundan bir biçimde görürsünüz. Değerleri girin ve **Kaydet**' i seçin. 

![Linux Edge cihaz formumun ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/deviceinfo.png)

Bir pano kutucuğu biçiminde sunulan bir görünüm aşağıda verilmiştir.

![Linux Edge cihaz panosu kutucuklarının ekran görüntüsü](./media/tutorial-add-edge-as-leaf-device/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Central IoT Edge cihazlarıyla nasıl çalışabileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Saydam ağ geçidini yapılandırma](../../iot-edge/how-to-create-transparent-gateway.md)
