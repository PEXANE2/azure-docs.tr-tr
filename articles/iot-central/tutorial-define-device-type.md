---
title: Azure IoT Central yeni bir cihaz türü tanımlayın | Microsoft Docs
description: Bu öğreticide, Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama, Oluşturucu olarak gösterilir. Türü için telemetri, durum, özellik ve ayarları tanımlarsınız.
author: dominicbetts
ms.author: dobett
ms.date: 06/07/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: a3faa76c1506664a075648edc7d57fbba542b011
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960540"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu öğreticide, bir Oluşturucu olarak, Microsoft Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlamak için bir cihaz şablonunun nasıl kullanılacağı gösterilmektedir. Cihaz şablonu, cihaz türü için telemetri, durum, özellik ve ayarları tanımlar.

Gerçek bir cihazı bağlanmadan önce uygulamanızı test etmeniz için IoT Central, onu oluştururken cihaz şablonundan sanal bir cihaz oluşturur.

Bu öğreticide, **bağlı bir AIR conditioner** cihaz şablonu oluşturacaksınız. Bağlı bir AIR klimaları cihazı:

* Sıcaklık ve nem gibi telemetri gönderir.
* , Açık veya kapalı olsun gibi bir durum bildirir.
* Üretici yazılımı sürümü ve seri numarası gibi cihaz özelliklerine sahiptir.
* Hedef sıcaklık gibi ayarlara sahiptir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir cihaz şablonu oluşturun
> * Cihazınıza telemetri ekleyin
> * Sanal Telemetriyi görüntüle
> * Olay ölçümünü tanımla
> * Benzetimli olayları görüntüle
> * Durum ölçümünü tanımla
> * Benzetimli durumu görüntüle
> * Ayarları ve özellikleri kullanma
> * Komutları kullanma
> * Sanal cihazınızı panoda görüntüleme

## <a name="prerequisites"></a>Prerequisites

Bu öğreticiyi tamamlayabilmeniz için bir Azure IoT Central uygulamasına ihtiyacınız vardır. [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcı ' nı tamamladıysanız, hızlı başlangıçta oluşturduğunuz uygulamayı yeniden kullanabilirsiniz. Aksi takdirde, boş bir Azure IoT Central uygulaması oluşturmak için aşağıdaki adımları izleyin:

1. [Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesine gidin.

2. Azure aboneliğinize erişmek için kullandığınız e-posta adresini ve parolayı girin:

    ![Kuruluş hesabınızı girin](./media/tutorial-define-device-type/sign-in.png)

3. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni uygulama**' yı seçin:

    ![Azure IoT Central uygulama Yöneticisi sayfası](./media/tutorial-define-device-type/iotcentralhome.png)

4. Yeni bir Azure IoT Central uygulaması oluşturmak için:
    
   * **Deneme sürümü**seçin. Deneme uygulaması oluşturmak için bir Azure aboneliğine ihtiyacınız yoktur.
    
      Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [uygulama oluşturma hızlı](quick-deploy-iot-central.md)başlangıcı.
    
   * **Özel uygulama**seçin.
    
   * İsteğe bağlı olarak, **contoso AIR conditioners**gibi kolay bir uygulama adı seçebilirsiniz. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL önekini, daha kolay bir şekilde değiştirebilirsiniz.
    
   * **Oluştur**' u seçin.

     ![Azure IoT Central uygulama oluştur sayfası](./media/tutorial-define-device-type/iotcentralcreate.png)

     Daha fazla bilgi için [uygulama oluşturma hızlı](quick-deploy-iot-central.md)başlangıcı bölümüne bakın.

## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Bir Oluşturucu olarak, uygulamanızda cihaz şablonları oluşturabilir ve düzenleyebilirsiniz. Bir cihaz şablonu oluşturduğunuzda, Azure IoT Central şablondan sanal bir cihaz oluşturur. Sanal cihaz, gerçek bir cihaza bağlanmadan önce uygulamanızın davranışını test etmenizi sağlayan telemetri üretir.

Uygulamanıza yeni bir cihaz şablonu eklemek için **cihaz şablonları** sayfasına gitmeniz gerekir. Bunu yapmak için sol gezinti menüsünde **cihaz şablonları** ' nı seçin.

![Cihaz şablonları sayfası](./media/tutorial-define-device-type/devicetemplates.png)

## <a name="add-a-device-template"></a>Cihaz şablonu ekleme

Aşağıdaki adımlarda, uygulamanıza sıcaklık telemetri gönderen cihazlar için yeni bir **bağlı hava** cihazı cihaz şablonu oluşturma yöntemi gösterilmektedir:

1. **Cihaz şablonları** sayfasında **+ Yeni**' yi seçin:

    ![Cihaz şablonları sayfası, cihaz şablonu oluştur](./media/tutorial-define-device-type/newtemplate.png)

2. Sayfa, aralarından seçim yapabileceğiniz şablonları gösterir.

    ![Cihaz şablonu kitaplığı](./media/tutorial-define-device-type/devicetemplatelibrary.png)

3. **Özel**' i seçin, **bağlı hava koşullayıcı** ' i cihaz şablonunuzun adı olarak girin ve ardından **Oluştur**' u seçin. Ayrıca, cihaz Gezgininde işleçlere görünür olan cihazınızın bir görüntüsünü de yükleyebilirsiniz:

    ![Özel cihaz](./media/tutorial-define-device-type/createcustomdevice.png)

4. **Bağlı hava** cihazı cihaz şablonunda, Telemetriyi tanımladığınız **ölçümler** sekmesinde olduğunuzdan emin olun. Tanımladığınız her cihaz şablonunun sizin için ayrı sekmeleri vardır:

   * Cihaz tarafından gönderilen telemetri, olay ve durum gibi _ölçümleri_belirtin.

   * Cihazı denetlemek için kullanılan _ayarları_ tanımlayın.

   * Cihaz meta verileri olan _özellikleri_ tanımlayın.

   * Doğrudan cihazda çalıştırılacak _komutları_ tanımlayın.

   * Cihazla ilişkili _kuralları_ tanımlayın.

   * Operatörlerinizin cihaz _panosunu_ özelleştirin.

     ![Hava koşullayıcı ölçümleri](./media/tutorial-define-device-type/airconmeasurements.png)

     > [!NOTE]
     > Cihaz şablonunun adını değiştirmek için sayfanın üst kısmındaki şablon adını seçin.

5. Sıcaklık telemetri ölçümünü eklemek için **+ yeni ölçüm**' i seçin. Ardından ölçüm türü olarak **telemetri** ' i seçin:

    ![Bağlı hava koşullayıcı ölçümleri](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Bir cihaz şablonu için tanımladığınız her telemetri türü aşağıdakiler gibi [yapılandırma seçeneklerini](howto-set-up-template.md) içerir:

   * Görüntüleme seçenekleri.

   * Telemetri ayrıntıları.

   * Simülasyon parametreleri.

     **Sıcaklık** telemetrinizi yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

     | Ayar              | Değer         |
     | -------------------- | -----------   |
     | Görünen ad         | Yüksel   |
     | Alan adı           | yüksel   |
     | Birim                | F             |
     | Min.                  | 60            |
     | Maks.                  | 110           |
     | Ondalık basamaklar       | 0             |

     Telemetri ekranı için bir renk de seçebilirsiniz. Telemetri tanımını kaydetmek için **Kaydet**' i seçin:

     ![Sıcaklık simülasyonu yapılandırma](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Daha kısa bir süre sonra **ölçümler** sekmesi, sanal cihaz koşullarınızdan gelen sıcaklık telemetrinin bir grafiğini gösterir. Görünürlüğü, toplamayı yönetmek veya telemetri tanımını düzenlemek için denetimleri kullanın:
 
    > [!NOTE]
    > Telemetri için, **Ortalama** varsayılan toplama olarak ayarlanır. 

    ![Sıcaklık simülasyonu görüntüle](./media/tutorial-define-device-type/viewsimulation.png)

8. Grafiği **çizgi**, **yığın**ve **düzenleme zaman aralığı** denetimlerini kullanarak da özelleştirebilirsiniz:

    ![Grafiği özelleştirme](./media/tutorial-define-device-type/customizechart.png)

## <a name="add-an-event-measurement"></a>Olay ölçümü ekleme

Bir hata veya bileşen hatası gibi bir olay olduğunda cihazın gönderdiği zaman içindeki verileri tanımlamak için olayları kullanın. Azure IoT Central, gerçek bir cihaza bağlanmadan önce uygulamanızın davranışını sınamanızı sağlamak üzere cihaz olaylarının benzetimini yapabilir. **Ölçüm** görünümündeki cihaz şablonunuz için olay ölçümlerini tanımlayın.

1. **Fan motoru hata** olayı ölçümünü eklemek Için **+ yeni ölçüm**' i seçin. Ardından ölçüm türü olarak **olay** ' ı seçin:

    ![Bağlı hava koşullayıcı ölçümleri](./media/tutorial-define-device-type/eventnew.png)

2. Bir cihaz şablonu için tanımladığınız her olay türü, aşağıdakiler gibi [yapılandırma seçeneklerini](howto-set-up-template.md) içerir:

   * Görünen ad.

   * Alan adı.

   * İnin.

     **Fan motoru hata** olaylarınızı yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

     | Ayar              | Değer             |
     | -------------------- | -----------       |
     | Görünen ad         | Fan Motoru hatası   |
     | Alan adı           | fanotohata       |
     | Önem Derecesi             | Hata             |

     Olay tanımını kaydetmek için **Kaydet**' i seçin:

     ![Olay ölçümünü yapılandırma](./media/tutorial-define-device-type/eventconfiguration.png)

3. Kısa bir süre sonra **ölçümler** sekmesi, sanal olarak bağlı hava koşullayıcı cihazınızdan rastgele oluşturulan olayların bir grafiğini gösterir. Görünürlüğü yönetmek veya olay tanımını düzenlemek için denetimleri kullanın:

    ![Olay simülasyonu görüntüle](./media/tutorial-define-device-type/eventview.png)

1. Olayla ilgili ek ayrıntıları görmek için grafikteki olayı seçin:

    ![Olay ayrıntılarını görüntüle](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-a-state-measurement"></a>Durum ölçümü tanımlama

Durumu kullanarak cihazın veya bileşenin durumunu bir süre içinde tanımlayabilir ve görselleştirebilirsiniz. Azure IoT Central, gerçek bir cihaza bağlanmadan önce uygulamanızın davranışını sınamanızı sağlamak üzere cihaz durumunun benzetimini yapabilir. **Ölçüm** görünümündeki cihaz yazmanız için durum ölçümleri tanımlarsınız.

1. **Fan modu** durum ölçümü eklemek Için **+ yeni ölçüm**' i seçin. Ardından ölçüm türü olarak **durum** ' u seçin:

    ![Bağlı Hava durumu ölçümleri](./media/tutorial-define-device-type/statenew.png)

2. Bir cihaz şablonu için tanımladığınız her durum türü, aşağıdakiler gibi [yapılandırma seçeneklerini](howto-set-up-template.md) içerir:

   * Görünen ad.

   * Alan adı.

   * İsteğe bağlı görüntü etiketleri içeren değerler.

   * Her bir değerin rengi.

     **Fan modu** durumunu yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

     | Ayar              | Değer             |
     | -------------------- | -----------       |
     | Görünen ad         | Fan modu          |
     | Alan adı           | fanmodu           |
     | Değer                | 1\.                 |
     | Görüntüleme etiketi        | İşletim         |
     | Değer                | 0                 |
     | Görüntüleme etiketi        | Durduruldu           |

     Durum ölçümü tanımını kaydetmek için **Kaydet**' i seçin:

     ![Durum ölçümünü yapılandırma](./media/tutorial-define-device-type/stateconfiguration.png)

3. Daha kısa bir süre sonra **ölçüler** sekmesi, sanal olarak bağlı hava koşullayıcı cihazınızdan rastgele oluşturulan durumların bir grafiğini gösterir. Görünürlüğü yönetmek veya durum tanımını düzenlemek için denetimleri kullanın:

    ![Görünüm durumu simülasyonu](./media/tutorial-define-device-type/stateview.png)

4. Cihaz tarafından küçük bir süre içinde gönderilen çok fazla veri noktası varsa, durum ölçümü farklı bir görselle gösterilir. Bu süre içindeki tüm veri noktalarını kronolojik sırada görüntülemek için grafiği seçin. Ayrıca, ölçümleri daha ayrıntılı olarak görmek için zaman aralığını da daraltabilirsiniz.

## <a name="settings-properties-and-commands"></a>Ayarlar, Özellikler ve komutlar

Ayarlar, Özellikler ve komutlar bir cihaz şablonunda tanımlanan ve tek bir cihazla ilişkili farklı değerlerdir:

* Uygulamanızdaki bir cihaza yapılandırma verileri göndermek için _ayarları_ kullanın. Örneğin, bir operatör cihazın telemetri aralığını iki saniyeden beş saniyeye değiştirmek için bir ayar kullanabilir. Bir işleci bir ayarı değiştirdiğinde, cihaz bir bildirim ile yanıt verinceye kadar bu ayar Kullanıcı arabiriminde bekliyor olarak işaretlenir.

* Aygıtınızla ilişkili meta verileri tanımlamak için _özellikleri_ kullanırsınız. Özelliklerin iki kategorisi vardır:
    
  * Uygulamanızda aygıtınızla ilgili bilgileri kaydetmek için _uygulama özelliklerini_ kullanırsınız. Örneğin, uygulama özelliklerini bir cihazın konumunu ve en son hizmet tarihini kaydetmek için kullanabilirsiniz. Bu özellikler uygulamada depolanır ve cihazla eşitlenmez. Bir işleç, özelliklere değerler atayabilir.

  * Cihazın uygulamanıza özellik değerleri göndermesini sağlamak için _cihaz özelliklerini_ kullanırsınız. Bu özellikler yalnızca cihaz tarafından değiştirilebilir. Bir operatör için, cihaz özellikleri salt okunurdur. Bağlı bir AIR 'in bu senaryosunda, bellenim sürümü ve cihaz seri numarası cihaz tarafından raporlanan cihaz özellikleridir.
    
    Daha fazla bilgi için, bkz. bir cihaz şablonu ayarlama hakkında nasıl yapılır kılavuzundaki [Özellikler](howto-set-up-template.md#properties) .

* Uygulamanızı uygulamanızdan uzaktan yönetmek için _komutları_ kullanırsınız. Cihazları denetlemek için cihazdaki komutları buluttan doğrudan çalıştırabilirsiniz. Örneğin, bir operatör cihazı anında yeniden önyüklemek için yeniden başlatma gibi komutları çalıştırabilir.

## <a name="use-settings"></a>Ayarları kullan

Bir işlece bir cihaza yapılandırma verileri göndermesini sağlamak için *ayarları* kullanırsınız. Bu bölümde, bağlantılı hava **koşullayıcı** cihaz şablonunuza, bir işlecin bağlantılı hava koşullayıcı hedef sıcaklığını ayarlamaya olanak sağlayan bir ayar eklersiniz.

1. **Bağlı hava koşullayıcı** cihaz şablonunuz için **Ayarlar** sekmesine gidin.

2. Sayılar veya metin gibi farklı türlerde ayarlar oluşturabilirsiniz. Cihazınıza sayı ayarı eklemek için **sayı** ' yı seçin.

3. **Küme sıcaklık** ayarınızı yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

    | Alan                | Değer           |
    | -------------------- | -----------     |
    | Görünen ad         | Sıcaklık ayarla |
    | Alan adı           | Setsıcaklık  |
    | Ölçü birimi      | F               |
    | Ondalık basamaklar       | 1\.               |
    | En küçük değer        | 20              |
    | En büyük değer        | 200             |
    | İlk değer        | 80              |
    | Açıklama          | Hava koşullayıcı için hedef sıcaklığını ayarlama |

    Sonra **Kaydet**' i seçin:

    ![Sıcaklık ayarla ayarını Yapılandır](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Cihaz bir ayar değişikliğini yaptığı zaman, ayarın durumu **eşitlenmiş**olarak değişir.

4. Ayarlar kutucuklarını taşıyarak ve yeniden boyutlandırarak **ayarlar sekmesinin yerleşimini** özelleştirebilirsiniz:

    ![Ayarlar yerleşimini Özelleştir](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Özellikleri kullanma

Uygulama *özelliklerini* kullanarak, cihazınızda aygıtınızla ilgili bilgileri saklayabilirsiniz. Bu bölümde, cihazın konumunu ve son hizmet tarihini depolamak için **bağlı hava koşullayıcı** cihaz şablonunuza uygulama özelliklerini eklersiniz. Bu özellikler uygulamada düzenlenebilir. Cihaz ayrıca, uygulamada salt okunan seri numarası ve bellenim sürümü gibi özellikleri de raporlar.

1. **Bağlı hava koşullayıcı** cihaz şablonunuz için **Özellikler** sekmesine gidin.

1. Sayılar veya metin gibi farklı türlerin cihaz özelliklerini oluşturabilirsiniz. Cihaz şablonunuza bir konum özelliği eklemek için **konum**' u seçin. Konum özelliğini yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

    | Alan                | Değer                |
    | -------------------- | -------------------- |
    | Görünen ad         | Konum             |
    | Alan adı           | konum             |
    | İlk değer        | Seattle, WA          |
    | Açıklama          | Cihaz konumu      |

    Diğer alanları varsayılan değerleriyle bırakın.

    ![Cihaz özelliklerini yapılandırma](./media/tutorial-define-device-type/configureproperties.png)

    **Kaydet**' i seçin.

1. Cihaz şablonunuza son bir hizmet tarihi özelliği eklemek için **Tarih**' i seçin.

1. Son hizmet tarihi özelliğini yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen ad         | Son hizmet tarihi       |
    | Alan adı           | serviceDate             |
    | İlk değer        | 1/1/2019                |
    | Açıklama          | Son hizmet verilen           |

    ![Cihaz özelliklerini yapılandırma](./media/tutorial-define-device-type/configureproperties2.png)

    **Kaydet**' i seçin.

1. Özellik kutucuklarını taşıyarak ve yeniden boyutlandırarak **Özellikler** sekmesinin yerleşimini özelleştirebilirsiniz.

1. Cihaz şablonunuza bellenim sürümü gibi bir cihaz özelliği eklemek için **cihaz özelliği**' ni seçin.

1. Üretici yazılımı sürümünüzü yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen ad         | Bellenim sürümü        |
    | Alan adı           | firmwareVersion         |
    | Veri Türü            | metin                    |
    | Açıklama          | AIR klimaları 'ın bellenim sürümü |

    ![Bellenim sürümünü Yapılandır](./media/tutorial-define-device-type/configureproperties3.png)

    **Kaydet**' i seçin.

1. Cihaz şablonunuza seri numarası gibi bir cihaz özelliği eklemek için **cihaz özelliği**' ni seçin.

1. Seri numarasını yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

    | Alan                | Değer                   |
    | -------------------- | ----------------------- |
    | Görünen ad         | Seri numarası           |
    | Alan adı           | Number            |
    | Veri Türü            | metin                    |
    | Açıklama          | Hava koşullayıcı seri numarası  |

    ![Seri numarasını Yapılandır](./media/tutorial-define-device-type/configureproperties4.png)

    **Kaydet**' i seçin.

    > [!NOTE]
    > Cihaz özelliği cihazdan uygulamaya gönderilir. Üretici yazılımı sürümü ve seri numarası değerleri, gerçek cihazınız IoT Central bağlantı kurduğunda güncelleştirilecek.

## <a name="use-commands"></a>Komutları kullanma

Bir işlecin komutları doğrudan cihazda çalıştırmasını sağlamak için _komutları_ kullanırsınız. Bu bölümde, bağlantılı hava **koşullayıcı** cihaz şablonunuza, bir işlecin bağlantılı hava koşullayıcı üzerinde belirli bir iletiyi yankısını sağlayan bir komut eklersiniz.

1. Şablonu düzenlemek için **bağlantılı hava koşullayıcı** cihaz şablonunuzun **Komutlar** sekmesine gidin.

1. Cihazınıza bir komut eklemek ve yeni komutunuz yapılandırmaya başlamak için **+ yeni komutunu** seçin.

1. Yeni komutunuz yapılandırmak için aşağıdaki tabloda bulunan bilgileri kullanın:

    | Alan                | Değer           |
    | -------------------- | -----------     |
    | Görünen ad         | Echo komutu    |
    | Alan adı           | echo            |
    | Varsayılan zaman aşımı      | 30              |
    | Veri Türü         | metin            |
    | Açıklama          | Cihaz komutu  |  

    **Giriş alanları**için **+ ' i** seçerek komuta ek girişler ekleyebilirsiniz.

    ![Ayar eklemeye hazırlanma](./media/tutorial-define-device-type/commandsecho1.png)

     **Kaydet**' i seçin.

1. Komut kutucuklarını taşıyarak ve yeniden boyutlandırarak **Komutlar** sekmesinin yerleşimini özelleştirebilirsiniz.

## <a name="view-your-simulated-device"></a>Sanal cihazınızı görüntüleme

Artık **bağlı hava koşullayıcı** cihaz şablonunuzu tanımladınız, kendi **panosunu** , tanımladığınız ölçümleri, ayarları ve özellikleri içerecek şekilde özelleştirebilirsiniz. Sonra panoyu bir operatör olarak önizleyebilirsiniz:

1. **Bağlı hava koşullayıcı** cihaz şablonunuz için **Pano** sekmesini seçin.

1. **Pano**üzerine bileşen eklemek Için **çizgi grafik** ' i seçin.

1. Aşağıdaki tablodaki bilgileri kullanarak **çizgi grafik** bileşenini yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Başlık        | Yüksel |
    | Zaman aralığı   | Son 30 dakika |
    | Ölçüler     | Sıcaklık ( **sıcaklık**yanındaki **görünürlüğü** seçin) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/linechartsettings.png)

    Sonra **Kaydet**' i seçin.

1. Aşağıdaki tablodaki bilgileri kullanarak **olay geçmişi** bileşenini seçin:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Başlık        | Fan Motoru etkinlikleri |
    | Zaman aralığı   | Son 30 dakika |
    | Ölçüler     | Fan Motoru hatası ( **Fan Motoru hatasının**yanındaki **görünürlüğü** seçin) |

    ![Olay grafiği ayarları](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Sonra **Kaydet**' i seçin.

1. **Durum geçmişi** bileşenini aşağıdaki tablodaki bilgileri kullanarak yapılandırın:

    | Ayar      | Değer       |
    | ------------ | ----------- |
    | Başlık        | Fan modu |
    | Zaman aralığı   | Son 30 dakika |
    | Ölçüler | Fan modu ( **fan modunun**yanındaki **görünürlüğü** seçin) |

    ![Çizgi grafik ayarları](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Sonra **Kaydet**' i seçin.

1. Panoya cihaz ayarları ve özellikler eklemek için **Ayarlar ve Özellikler**' i seçin. Panoda görmek istediğiniz ayarları veya özellikleri eklemek için **Ekle/Kaldır** ' ı seçin.

1. Aşağıdaki tablodaki bilgileri kullanarak **Ayarlar ve Özellikler** bileşenini yapılandırın:

    | Ayar                 | Değer         |
    | ----------------------- | ------------- |
    | Başlık                   | Cihaz özellikleri |
    | Ayarlar ve Özellikler | Sıcaklık ayarla<br/>Seri numarası<br/>Bellenim sürümü |

    **Ayarlar ve Özellikler** sayfalarında daha önce tanımladığınız ayarlar ve Özellikler **kullanılabilir sütunlarda**gösterilmektedir.

    ![Sıcaklık özelliği ayarlarını ayarla](./media/tutorial-define-device-type/propertysettings4.png)

    Sonra **Kaydet**' i seçin.

1. Artık panodaki bağlantılı hava koşullarınız için sanal verileri görebilirsiniz. Pano için kutucukları ve düzeni düzenleyebilirsiniz:

    ![Panoyu görüntüle](./media/tutorial-define-device-type/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Yeni bir cihaz şablonu oluşturun
> * Cihazınıza telemetri ekleyin
> * Sanal Telemetriyi görüntüle
> * Cihaz olaylarını tanımlama
> * Benzetimli olayları görüntüle
> * Durumlarınızı tanımlama
> * Benzetimli durumu görüntüle
> * Ayarları ve özellikleri kullanma
> * Komutları kullanma
> * Sanal cihazınızı panoda görüntüleme

Azure IoT Central uygulamanızda bir cihaz şablonu tanımladığınıza göre, önerilen sonraki adımlar aşağıda verilmiştir:

* [Cihazınız için kuralları ve eylemleri yapılandırma](tutorial-configure-rules.md)
* [İşlecin görünümlerini özelleştirme](tutorial-customize-operator.md)