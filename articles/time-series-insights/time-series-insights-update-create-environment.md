---
title: 'Öğretici: Önizleme ortamı ayarlama - Azure Time Series Insights | Microsoft Dokümanlar'
description: 'Öğretici: Azure Zaman Serisi Öngörüleri Önizlemesinde nasıl bir ortam ayarlayabileceğinizi öğrenin.'
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 04/02/2020
ms.custom: seodec18
ms.openlocfilehash: 3ccb9c7aff6eb59c4883bc3218e205fb7877e86e
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618366"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Öğretici: Azure Zaman Serisi Öngörüleri Önizleme ortamını ayarlama

Bu öğretici, azure zaman serisi öngörüleri önizleme seçimleri oluşarak size yol açırıyor. *pay-as-you-go*

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Azure Zaman Serisi Öngörüleri Önizleme ortamı oluşturun.
> * Azure Zaman Serisi Öngörüler Önizleme ortamını bir IoT Hub'ına bağlayın.
> * Verileri Azure Zaman Serisi Öngörüleri Önizleme ortamına aktarmak için bir çözüm hızlandırıcı örneği çalıştırın.
> * Veriler üzerinde temel analiz yapın.
> * Zaman Serisi Model türünü ve hiyerarşisini tanımlayın ve örneklerinizle ilişkilendirin.

>[!TIP]
> [IoT çözüm hızlandırıcıları, özel IoT](https://www.azureiotsolutions.com/Accelerators) çözümlerinin geliştirilmesini hızlandırmak için kullanabileceğiniz kurumsal dereceli önceden yapılandırılmış çözümler sağlar.

Zaten bir [Azure aboneliğiniz](https://azure.microsoft.com/free/) yoksa ücretsiz bir Azure aboneliği için kaydolun.

## <a name="prerequisites"></a>Ön koşullar

* En azından Azure aboneliği için **Katılımcı** rolüne sahip olmalısınız. Daha fazla bilgi [için, rol tabanlı erişim denetimi ve Azure portalını kullanarak erişimi yönet'i](../role-based-access-control/role-assignments-portal.md)okuyun.

## <a name="create-a-device-simulation"></a>Cihaz benzetimi oluşturma

Bu bölümde, bir Azure IoT Hub örneğine veri gönderen üç benzetimli aygıt oluşturursunuz.

1. [Azure IoT çözüm hızlandırıcıları sayfasına](https://www.azureiotsolutions.com/Accelerators)gidin. Sayfa, önceden oluşturulmuş birkaç örnek görüntüler. Azure hesabınızı kullanarak oturum açın. Ardından Aygıt **Simülasyonu'nun 'u**seçin.

   [![Azure IoT çözüm hızlandırıcıları sayfası.](media/v2-update-provision/iot-solution-accelerators-landing-page.png)](media/v2-update-provision/iot-solution-accelerators-landing-page.png#lightbox)

1. Sonraki **sayfada, Şimdi Deneyin'i**seçin. Ardından, **Aygıt Simülasyonu Oluştur çözüm** sayfasına gerekli parametreleri girin.

   Parametre|Açıklama
   ---|---
   **Dağıtım adı** | Bu benzersiz değer, yeni bir kaynak grubu oluşturmak için kullanılır. Listelenen Azure kaynakları oluşturulur ve kaynak grubuna atanır.
   **Azure aboneliği** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı aboneliği belirtin.
   **Dağıtım seçenekleri** | Bu öğreticiye özgü yeni bir IoT hub'ı oluşturmak için **Yeni IoT Hub'ı sağlama'yı** seçin.
   **Azure konumu** | Önceki bölümde Time Series Insights ortamınızı oluşturmak için kullanılan aynı bölgeyi belirtin.

   İşi nizi bitirdiğinde, çözümün Azure kaynaklarını sağlamak için **Oluştur'u** seçin. Bu işlemin tamamlanması 20 dakika kadar sürebilir.

   [![Cihaz simülasyon çözümlerini sağlama.](media/v2-update-provision/iot-solution-accelerators-configuration.png)](media/v2-update-provision/iot-solution-accelerators-configuration.png#lightbox)

1. Sağlama tamamlandıktan sonra, dağıtım durumunun **Provisioning'den** **Hazır'a**taşındığını bildiren iki bildirim görüntülenir. 

   >[!IMPORTANT]
   > Çözüm hızlandırıcınızı henüz girmeyin! Daha sonra geri döneceğiniz için bu web sayfasını açık tutun.

   [![Cihaz simülasyon çözüm sağlamaları tamamlandı.](media/v2-update-provision/iot-solution-accelerator-ready.png)](media/v2-update-provision/iot-solution-accelerator-ready.png#lightbox)

1. Şimdi, Azure portalında yeni oluşturulan kaynakları inceleyin. Kaynak **grupları** sayfasında, son adımda sağlanan **Çözüm adı** kullanılarak yeni bir kaynak grubunun oluşturulduğuna dikkat edin. Aygıt simülasyonu için oluşturulan kaynaklara dikkat edin.

   [![Cihaz simülasyon kaynakları.](media/v2-update-provision/tsi-device-sim-solution-resources.png)](media/v2-update-provision/tsi-device-sim-solution-resources.png#lightbox)

## <a name="create-a-preview-payg-environment"></a>Önizleme PAYG ortamı oluşturma

Bu bölümde, Azure Zaman Serisi Öngörüleri Önizleme ortamının nasıl oluşturulup [Azure portalını](https://portal.azure.com/)kullanarak IoT Çözüm Hızlandırıcısı tarafından oluşturulan IoT hub'ına nasıl bağlanılabildiğini açıklar.

1. Azure abonelik hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın. 
1. Üstteki menüden **+ Kaynak oluştur**'u seçin. 
1. Nesnelerin **İnterneti** kategorisini seçin ve ardından **Zaman Serisi Öngörüleri'ni**seçin. 

   [![Zaman Serisi Öngörüler ortamı kaynağını seçin.](media/v2-update-provision/tsi-create-new-environment.png)](media/v2-update-provision/tsi-create-new-environment.png#lightbox)

1. Create **Time Series Insights ortamı** bölmesinde, **Temel Bilgiler** sekmesinde aşağıdaki parametreleri ayarlayın:

    | Parametre | Eylem |
    | --- | ---|
    | **Ortam adı** | Azure Zaman Serisi Öngörüleri Önizleme ortamı için benzersiz bir ad girin. |
    | **Abonelik** | Azure Zaman Serisi Öngörüleri Önizleme ortamını oluşturmak istediğiniz aboneliğe girin. En iyi yöntem, aygıt simülatörü tarafından oluşturulan IoT kaynaklarının geri kalanıyla aynı aboneliği kullanmaktır. |
    | **Kaynak grubu** | Varolan bir kaynak grubu seçin veya Azure Zaman Serisi Öngörüleri Önizleme ortamı kaynağı için yeni bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarına yönelik bir kapsayıcıdır. En iyi yöntem, aygıt simülatörü tarafından oluşturulan diğer IoT kaynaklarıyla aynı kaynak grubunu kullanmaktır. |
    | **Konum** | Azure Zaman Serisi Öngörüleri Önizleme ortamınız için bir veri merkezi bölgesi seçin. Ek gecikmeyi önlemek için, azure zaman serisi öngörüleri önizleme ortamınızı aygıt simülatörü tarafından oluşturulan IoT hub'ınızla aynı bölgede oluşturmak en iyisidir. |
    | **Katmanı** |  **PAYG** 'yi seçin (*giderseniz ödeyin).* Bu, Azure Zaman Serisi Öngörüleri Önizleme ürünü için SKU'dur. |
    | **Özellik adı** | Zaman serisi örneğini benzersiz olarak tanımlayan bir değer girin. **Özellik Kimliği** kutusuna girdiğiniz değer daha sonra değiştirilemez. Bu öğretici için ***iothub-connection-device-id***girin. Time Series ID hakkında daha fazla bilgi edinmek [için, Zaman Serisi Kimliği seçmek için en iyi uygulamaları](./time-series-insights-update-how-to-id.md)okuyun. |
    | **Depolama hesabı adı** | Yeni bir depolama hesabı için genel olarak benzersiz bir ad girin.|
    |**Sıcak mağazayı etkinleştirme**|Sıcak mağazayı etkinleştirmek için **Evet'i** seçin. Daha sonra geri gelebilir ve bu ayarı etkinleştirebilirsiniz. |
    |**Veri saklama (gün içinde)**|Varsayılan 7 gün seçeneğini seçin. |

    **Sonraki seçin: Olay Kaynağı**.

   [![Yeni Zaman Serisi Öngörüler ortam yapılandırması.](media/v2-update-provision/tsi-environment-configuration.png)](media/v2-update-provision/tsi-environment-configuration.png#lightbox)

   [![Ortam için Zaman Serisi Kimliğini yapılandırın.](media/v2-update-provision/tsi-time-series-id-selection.png)](media/v2-update-provision/tsi-time-series-id-selection.png#lightbox)

1. Olay **Kaynağı** sekmesinde aşağıdaki parametreleri ayarlayın:

   | Parametre | Eylem |
   | --- | --- |
   | **Olay kaynağı mı oluşturuyorsun?** | **Evet'i**seçin.|
   | **Adı** | Olay kaynağı adı için benzersiz bir değer girin. |
   | **Kaynak türü** | **IoT Hub'ı**seçin. |
   | **Hub seçin** | **Varolan seç'i**seçin. |
   | **Abonelik** | Aygıt simülatörü için kullandığınız aboneliği seçin. |
   | **IoT Hub adı** | Aygıt simülatörü için oluşturduğunuz IoT hub adını seçin. |
   | **IoT Hub erişim ilkesi** | **iothubowner**seçin. |
   | **IoT Hub tüketici grubu** | **Yeni'yi**seçin, benzersiz bir ad girin ve sonra **+ Ekle'yi**seçin. Tüketici grubu, Azure Zaman Serisi Öngörüleri Önizlemesinde benzersiz bir değer olmalıdır. |
   | **Zaman damgası özelliği** | Bu değer, gelen telemetri verilerinizdeki **Zaman Damgası** özelliğini tanımlamak için kullanılır. Bu öğretici için, bu kutuyu boş bırakın. Bu simülatör, Time Series Insights'ın varsayılan olarak kullandığı IoT Hub'dan gelen zaman damgasını kullanır. |

   **Gözden Geçir + Oluştur'u**seçin.

   [![Oluşturulan IoT hub'ını olay kaynağı olarak yapılandırın.](media/v2-update-provision/tsi-configure-event-source.png)](media/v2-update-provision/tsi-configure-event-source.png#lightbox)

1. **Oluştur'u**seçin.

    [![Oluştur düğmesiyle gözden geçir + sayfa oluştur.](media/v2-update-provision/tsi-environment-confirmation.png)](media/v2-update-provision/tsi-environment-confirmation.png#lightbox)

    Dağıtımınızın durumunu gözden geçirebilirsiniz:

    [![Dağıtımın tamamolduğuna dair bildirim.](media/v2-update-provision/tsi-deployment-notification.png)](media/v2-update-provision/tsi-deployment-notification.png#lightbox)

1. Azure aboneliğinin sahibiyseniz varsayılan olarak Azure Zaman Serisi Öngörüleri Önizleme ortamınıza erişebilirsiniz. Erişiminiz olduğunu doğrulayın:

   1. Kaynak grubunuzu arayın ve ardından yeni oluşturulan Azure Zaman Serisi Öngörüleri Önizleme ortamınızı seçin. 

      [![Ortamınızı seçin ve görüntüleyin.](media/v2-update-provision/verify-tsi-resource-in-group.png)](media/v2-update-provision/verify-tsi-resource-in-group.png#lightbox)

   1. Azure Zaman Serisi Öngörüleri Önizleme sayfasında **Veri Erişim İlkeleri'ni**seçin:

      [![Veri erişim ilkelerini doğrulayın.](media/v2-update-provision/tsi-data-access-panel.png)](media/v2-update-provision/tsi-data-access-panel.png#lightbox)

   1. Kimlik bilgilerinizin listeli olduğunu doğrulayın:

      Kimlik bilgileriniz listelenmemişse, Ekle'yi seçerek ve kimlik bilgilerinizi arayarak ortama erişme izni vermelisiniz. İzinleri ayarlama hakkında daha fazla bilgi edinmek için [Veri erişimini Ver'i](./time-series-insights-data-access.md)okuyun.

## <a name="stream-data"></a>Veri akışı yapma

Zaman Serisi Öngörüleri ortamınızı dağıttığınıza göre, analiz için veri akışına başlayın.

1. [Çözüm hızlandırıcıları panonuza](https://www.azureiotsolutions.com/Accelerators#dashboard) geri dönün. Gerekirse bu eğitimde kullandığınız Azure hesabını kullanarak yeniden oturum açın. "Aygıt Çözümünüzü" seçin ve ardından dağıtılan çözümünüzü başlatmak için **çözüm hızlandırıcınıza gidin.**

   [![Çözüm hızlandırıcılar panosu.](media/v2-update-provision/iot-solution-accelerator-ready.png)](media/v2-update-provision/iot-solution-accelerator-ready.png#lightbox)

1. Cihaz simülasyonu web uygulaması, web uygulamasına Oturum Açma'yı ve **profil izninizi okumanızı** istenerek başlar. Bu izin, uygulamanın işleyişini desteklemek için gerekli kullanıcı profili bilgilerini almak için uygulama sağlar.

   [![Cihaz simülasyonu web uygulama onayı.](media/v2-update-provision/sawa-signin-consent.png)](media/v2-update-provision/sawa-signin-consent.png#lightbox)

1. Seçin **+ Yeni simülasyon**. Simülasyon kurulum sayfası **yüklendikten** sonra gerekli parametreleri girin.

    | Parametre | Eylem |
    | --- | --- |
    | **Adı** | Simülatör için benzersiz bir ad girin. |
    | **Açıklama** | Bir tanım girin. |
    | **Benzetim süresi** | **Süresiz Olarak Çalıştırılaayarlayın.** |
    | **Cihaz modeli** | + **Aygıt türü ekle'yi** tıklatın <br />**Adı**: **Asansör**girin . <br />**Tutar**: **3**girin . <br /> Kalan varsayılan değerleri bırakın |
    | **Hedef IoT Hub'ı** | Önceden **temin edilmiş IoT Hub'ı kullanacak**şekilde ayarlayın. |

    [![Parametreleri yapılandırın ve başlatın.](media/v2-update-provision/tsi-launch-solution-accelerator.png)](media/v2-update-provision/tsi-launch-solution-accelerator.png#lightbox)

    **Simülasyonu Başlat'ı**seçin.

    Aygıt simülasyon panosunda **Etkin aygıtlar** ve **Toplam iletiler** görüntülenir.

    [![Azure IoT simülasyon panosu.](media/v2-update-provision/tsi-see-active-devices-and-messages.png)](media/v2-update-provision/tsi-see-active-devices-and-messages.png#lightbox)

## <a name="analyze-data"></a>Verileri çözümleme

Bu bölümde, [Azure Zaman Serisi Öngörüleri Önizleme gezginini](./time-series-insights-update-explorer.md)kullanarak zaman serisi verilerinizde temel analizler gerçekleştirebilirsiniz.

1. [Azure portalındaki](https://portal.azure.com/)kaynak sayfasından URL'yi seçerek Azure Zaman Serisi Öngörüleri Önizleme gezgininize gidin.

    [![Zaman Serisi Öngörüler Önizleme explorer URL..](media/v2-update-provision/tsi-select-explorer-url.png)](media/v2-update-provision/tsi-select-explorer-url.png#lightbox)

1. Zaman Serisi Öngörüleri gezgininde, ekranın üst kısmını kapsayan bir çubuk görüntülenir. Bu sizin kullanılabilirlik seçici. En az iki 2 m seçili olduğundan emin olun ve gerekirse seçici tutamaçları sağa ve sola doğru seçip sürükleyerek zaman dilimini genişletin.

1. **Zaman Serisi Örnekleri** sol tarafta görüntülenir.

    [![Ebeveynsiz örneklerin listesi.](media/v2-update-provision/tsi-explorer-unparented-instances.png)](media/v2-update-provision/tsi-explorer-unparented-instances.png#lightbox)

1. İlk kez seri örneğini seçin. Ardından, **sıcaklığı göster'i**seçin.

    [![Ortalama sıcaklığı göstermek için menü komutu ile seçili zaman serisi örneği.](media/v2-update-provision/select-instance-and-temperature.png)](media/v2-update-provision/select-instance-and-temperature.png#lightbox)

    Bir zaman serisi grafiği görüntülenir. **Aralığı** **30'lara değiştirin.**

1. Önceki adımı, bu grafikte gösterildiği gibi üçünü de görüntülemek için diğer iki zaman serisi örneğiyle birlikte yineleyin:

    [![Tüm zamanların serisi için grafik.](media/v2-update-provision/tsi-explorer-add-three-instances.png)](media/v2-update-provision/tsi-explorer-add-three-instances.png#lightbox)

1. Sağ üst köşedeki zaman aralığı seçiciyi seçin. Burada milisaniyeye kadar belirli başlangıç ve bitiş saatlerini seçebilir veya Son **30 dakika**gibi önceden yapılandırılmış seçenekler arasından seçim yapabilirsiniz. Varsayılan saat dilimini de değiştirebilirsiniz.

    [![Zaman aralığını son 30 dakikaya ayarlayın.](media/v2-update-provision/tsi-explorer-thirty-minute-time-range.png)](media/v2-update-provision/tsi-explorer-thirty-minute-time-range.png#lightbox)

    Çözüm hızlandırıcının **Son 30 dakikadaki** ilerlemesi artık Time Series Insights gezgininde görüntülenir.

## <a name="define-and-apply-a-model"></a>Bir modeli tanımlama ve uygulama

Bu bölümde, verilerinizi yapılandırmak için bir model uygularsınız. Modeli tamamlamak için türleri, hiyerarşileri ve örnekleri tanımlarsınız. Veri modelleme hakkında daha fazla bilgi edinmek için [Time Series Model'i](./time-series-insights-update-tsm.md)okuyun.

1. Gezginde **Model** sekmesini seçin:

   [![Gezginde Model sekmesini görüntüleyin.](media/v2-update-provision/tsi-select-model-view.png)](media/v2-update-provision/tsi-select-model-view.png#lightbox)

   **Türler** sekmesinde **+ Ekle'yi**seçin.

1. Aşağıdaki parametreleri girin:

    | Parametre | Eylem |
    | --- | ---|
    | **Adı** | **Asansöre** Girin |
    | **Açıklama** | Girin **Bu Asansör için bir tür tanımı** |

1. Ardından, **Değişkenler** sekmesini seçin. 

   **+ Değişken Ekle'yi** seçin ve Asansör türünün ilk değişkeni için aşağıdaki değerleri doldurun. Toplamda üç değişken yazarsınız.

    | Parametre | Eylem |
    | --- | --- |
    | **Adı** | **Avg Sıcaklık**girin. |
    | **Tür** | **Sayısal'ı** seçin |
    | **Değer** | Önceden ayarlanmış seçin: **Sıcaklık seçin (Çift)**. <br /> Not: Azure Zaman Serisi Öngörüleri Önizlemesi olayları almaya başladıktan sonra **Değer'in** otomatik olarak doldurulması birkaç dakika sürebilir.|
    | **Toplama İşlemi** | **Gelişmiş Seçenekleri**Genişletin. <br /> **AVG'yi**seçin. |

    **Uygula**’yı seçin. Sonra, **+ Yeniden Değişken Ekle** ve aşağıdaki değerleri ayarlayın:

    | Parametre | Eylem |
    | --- | --- |
    | **Adı** | **Avg Titreşim**girin. |
    | **Tür** | **Sayısal'ı** seçin |
    | **Değer** | Önceden ayarlanmış seçin: **Titreşimi seçin (Çift)**. <br /> Not: Azure Zaman Serisi Öngörüleri Önizlemesi olayları almaya başladıktan sonra **Değer'in** otomatik olarak doldurulması birkaç dakika sürebilir.|
    | **Toplama İşlemi** | **Gelişmiş Seçenekleri**Genişletin. <br /> **AVG'yi**seçin. |

    **Uygula**’yı seçin. Sonra, **+ Tekrar Değişken Ekle** ve üçüncü ve son değişken için aşağıdaki değerleri ayarlayın:

    | Parametre | Eylem |
    | --- | --- |
    | **Adı** | **Katı**girin. |
    | **Tür** | **Kategorik'i** seçin |
    | **Değer** | Önceden ayarlanmış seçin: **Kat (Çift)** seçin. <br /> Not: Azure Zaman Serisi Öngörüleri Önizlemesi olayları almaya başladıktan sonra **Değer'in** otomatik olarak doldurulması birkaç dakika sürebilir.|
    | **Kategoriler** | <span style="text-decoration: underline">Etiket </span>   -  <span style="text-decoration: underline">Değerleri</span> <br /> Alt: 1,2,3,4 <br /> Orta: 5,6,7,8,9 <br /> Üst: 10,11,12,13,14,15 |
    | **Varsayılan Kategori** | **Bilinmeyeni** Girin |

    [![Tür değişkenleri ekleyin.](media/v2-update-provision/tsi-add-type-variables.png)](media/v2-update-provision/tsi-add-type-variables.png#lightbox)

    **Uygula**’yı seçin.

1. **Kaydet'i**seçin. Üç değişken oluşturulur ve görüntülenir.

    [![Türü ekledikten sonra Model görünümünde gözden geçirin.](media/v2-update-provision/tsi-add-type-and-view.png)](media/v2-update-provision/tsi-add-type-and-view.png#lightbox)

1. **Hiyerarşiler** sekmesini seçin. Daha sonra **+ Ekle'yi**seçin.
   
   **Hiyerarşiyi Edit** bölmesinde aşağıdaki parametreleri ayarlayın:

   | Parametre | Eylem |
   | --- | ---|
   | **Adı** | **Konum Hiyerarşisi'ni**girin. |
   |**Düzeyler**| Birinci düzeyin adı olarak **Ülkeye** girin <br> Select **+ Ekle Düzeyi** <br> İkinci seviye için **Şehir'e** girin, ardından **+ Seviye Ekle'yi** seçin <br> Üçüncü ve son seviyenin adı olarak **Binaya** girin |

   **Kaydet'i**seçin.

   [![Yeni hiyerarşinizi Model görünümünde görüntüleyin.](media/v2-update-provision/tsi-add-hierarchy-and-view.png)](media/v2-update-provision/tsi-add-hierarchy-and-view.png#lightbox)

1. **Örneklere**gidin. En sağdaki **Eylemler** altında ve aşağıdaki değerlerle ilk örneği düzeltmek için kalem simgesini seçin:

    | Parametre | Eylem |
    | --- | --- |
    | **Tür** | **Asansör'ü**seçin. |
    | **Adı** | **Asansör 1'e** girin|
    | **Açıklama** | **Asansör 1 Için Örnek** Girin |

    Örnek **Alanlara** gidin ve aşağıdaki değerleri girin:

    | Parametre | Eylem |
    | --- | --- |
    | **Hiyerarşiler** | **Konum Hiyerarşisi'ni** seçin |
    | **Ülke** | **ABD'ye** girin |
    | **Şehir** | **Seattle'a** girin |
    | **Oluşturma** | **Uzay İğnesi** Girin |

    **Kaydet'i**seçin.

1. Aşağıdaki değerleri kullanırken önceki adımı diğer iki örnekle yineleyin:

    **Asansör 2 için:**

    | Parametre | Eylem |
    | --- | --- |
    | **Tür** | **Asansör'ü**seçin. |
    | **Adı** | **Asansör 2'ye** girin|
    | **Açıklama** | **Asansör 2 Için Örnek** Girin |
    | **Hiyerarşiler** | **Konum Hiyerarşisi'ni** seçin |
    | **Ülke** | **ABD'ye** girin |
    | **Şehir** | **Seattle'a** girin |
    | **Oluşturma** | **Pasifik Bilim Merkezi** girin |

    **Asansör 3 için:**

    | Parametre | Eylem |
    | --- | --- |
    | **Tür** | **Asansör'ü**seçin. |
    | **Adı** | **Asansör 3'e** girin|
    | **Açıklama** | **Asansör 3 Için Örnek** Girin |
    | **Hiyerarşiler** | **Konum Hiyerarşisi'ni** seçin |
    | **Ülke** | **ABD'ye** girin |
    | **Şehir** | **New York'a** girin |
    | **Oluşturma** | **Empire State Binasıgirin** |

    [![Güncelleştirilmiş örnekleri görüntüleyin.](media/v2-update-provision/iot-solution-accelerator-instances.png)](media/v2-update-provision/iot-solution-accelerator-instances.png#lightbox)

1. Grafik bölmesini görüntülemek için **Çözümle** sekmesine gidin. **Konum Hiyerarşisi**altında, zaman serisi örneklerini görüntülemek için tüm hiyerarşi düzeylerini genişletin:

    [![Grafik görünümünde tüm hiyerarşileri görüntüleyin.](media/v2-update-provision/iot-solution-accelerator-view-hierarchies.png)](media/v2-update-provision/iot-solution-accelerator-view-hierarchies.png#lightbox)

1. **Pasifik Bilim Merkezi**altında, Zaman Serisi Örnek Asansör **2**seçin ve sonra Ortalama **Sıcaklık göster**seçin.

1. Aynı durumda, **Asansör 2**, **Göster Floor**seçin.

    Kategorik değişkeninizle asansörün üst, alt ve orta katlarda ne kadar zaman geçirdiğini belirleyebilirsiniz.

    [![Asansör 2'yi hiyerarşi ve verilerle görselleştirin.](media/v2-update-provision/iot-solution-accelerator-elevator-two.png)](media/v2-update-provision/iot-solution-accelerator-elevator-two.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladığınızda, oluşturduğunuz kaynakları temizleyin:

1. [Azure portalındaki](https://portal.azure.com)sol menüden **Tüm kaynakları**seçin, Azure Zaman Serisi Öngörüleri kaynak grubunuzu bulun.
1. **Sil'i** seçerek tüm kaynak grubunu (ve içindeki tüm kaynakları) silin veya her kaynağı tek tek kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:  

* Bir aygıt simülasyon hızlandırıcısı oluşturun ve kullanın.
* Bir Azure Zaman Serisi Öngörüleri Önizleme PAYG ortamı oluşturun.
* Azure Zaman Serisi Öngörüleri Önizleme ortamını bir iot hub'ına bağlayın.
* Azure Zaman Serisi Öngörüler Önizleme ortamına veri akışı sağlamak için bir çözüm hızlandırıcı örneği çalıştırın.
* Verilerin temel analizini gerçekleştirin.
* Bir Zaman Serisi Model türünü ve hiyerarşisini tanımlayın ve bunları örnekleriniz ile ilişkilendirin.

Artık kendi Azure Zaman Serisi Öngörüleri Önizleme ortamınızı nasıl oluşturacağınız konusunda bildiğinize göre, Azure Zaman Serisi Öngörüleri'ndeki temel kavramlar hakkında daha fazla bilgi edinin.

Azure Time Series Insights depolama yapılandırması hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Zaman Serisi Öngörüleri Önizleme depolama ve giriş](./time-series-insights-update-storage-ingress.md)

Time Series Modelleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Time Series Öngörüleri Önizleme veri modellemesi](./time-series-insights-update-tsm.md)

Çevrenizi Power BI'ye bağlayın hakkında daha fazla bilgi edinin

> [!div class="nextstepaction"]
> [Power BI'de Time Series Insights'tan verileri görselleştirin](./how-to-connect-power-bi.md)
