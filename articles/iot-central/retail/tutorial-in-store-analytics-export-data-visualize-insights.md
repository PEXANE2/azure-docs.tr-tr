---
title: Öğretici - Azure IoT Central'da veri aktarın ve öngörüleri görselleştirin
description: Bu eğitimde, IoT Central'dan nasıl veri dışa aktarılamayı öğrenin ve Power BI panosundaki öngörüleri görselleştirin.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.custom:
- iot-storeAnalytics-checkout
- iot-p0-scenario
ms.author: dobett
author: dominicbetts
ms.date: 11/12/2019
ms.openlocfilehash: 6062e8a74af4bb0a19d02ccf9a4c50da0cc4a7c5
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000108"
---
# <a name="tutorial-export-data-from-azure-iot-central-and-visualize-insights-in-power-bi"></a>Öğretici: Azure IoT Central'dan veri aktarın ve Power BI'deki öngörüleri görselleştirin



Önceki iki öğreticide, **mağaza içi analiz - ödeme** uygulama şablonu kullanarak bir IoT Central uygulaması oluşturdunuz ve özelleştirmişsiniz. Bu eğitimde, IoT Central uygulamanızı cihazlardan toplanan telemetriyi dışa aktarmak için yapılandırırsınız. Daha sonra, telemetriden türetilen öngörüleri görselleştirmek için mağaza yöneticisi için özel bir pano oluşturmak için Power BI'yi kullanırsınız.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Telemetriyi bir olay merkezine aktarmak için bir IoT Central uygulamasını yapılandırın.
> * Etkinlik merkezinden Power BI akış veri kümesine veri göndermek için Logic Apps'ı kullanın.
> * Akış veri kümesindeki verileri görselleştirmek için bir Power BI panosu oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Önceki iki öğreticiyi tamamlamak için [Azure IoT Central'da mağaza içi bir analiz uygulaması oluşturun](./tutorial-in-store-analytics-create-app.md) ve operatör [panosunu özelleştirin ve Azure IoT Central'daki cihazları yönetin.](./tutorial-in-store-analytics-customize-dashboard.md)
* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
* Power BI hesabı. Power BI hesabınız yoksa, başlamadan önce ücretsiz power [BI Pro denemesi](https://app.powerbi.com/signupredirect?pbi_source=web) için kaydolun.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Etkinlik hub'ınızı ve mantık uygulamanızı oluşturmadan önce, bunları yönetmek için bir kaynak grubu oluşturmanız gerekir. Kaynak **grubu, mağaza içi analizinizle** aynı konumda olmalıdır - checkout IoT Central uygulaması. Kaynak grubu oluşturmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezintide **Kaynak gruplarını**seçin. Ardından **Ekle**'yi seçin.
1. **Abonelik**için, IoT Central uygulamanızı oluşturmak için kullandığınız Azure aboneliğinin adını seçin.
1. Kaynak **grup** adı için _perakende mağaza analizi_*girin.
1. **Bölge**için, IoT Central uygulaması için seçtiğiniz bölgeyi seçin.
1. **Gözden Geçir + Oluştur'u**seçin.
1. Gözden **Geçir + Oluştur** sayfasında **Oluştur'u**seçin.

Artık aboneliğinizde perakende **mağaza analizi** adlı bir kaynak grubunuz var.

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Perakende izleme uygulamasını telemetri dışa aktarmak için yapılandırmadan önce, dışa aktarılan verileri almak için bir olay merkezi oluşturmanız gerekir. Aşağıdaki adımlar, etkinlik hub'ınızı nasıl oluşturabileceğinizi gösterir:

1. Azure portalında, ekranın sol üst kısmında **bir kaynak oluştur'u** seçin.
1. Marketi Ara'da, _Olay Hub'larını_girin ve enter **tuşuna** **basın.**
1. Olay **Hub'ları** sayfasında **Oluştur'u**seçin.
1. Ad **Alanı Oluştur** sayfasında aşağıdaki adımları izleyin:
    * _Adınız-perakende-mağaza-analizi_gibi ad alanı için benzersiz bir ad girin. Sistem, bu adın kullanılabilir olup olmadığını denetler.
    * **Temel** fiyatlandırma katmanını seçin.
    * IoT Central uygulamanızı oluşturmak için kullandığınız **Aboneliği** seçin.
    * Perakende **mağaza analizi** kaynak grubunu seçin.
    * IoT Central uygulamanız için kullandığınız konumu seçin.
    * **Oluştur'u**seçin. Sistemin kaynakları sağlaması için birkaç dakika beklemeniz gerekebilir.
1. Portalda, **perakende mağaza analizi** kaynak grubuna gidin. Dağıtımın tamamlanmasını bekleyin. Dağıtım durumunu güncelleştirmek için **Yenile'yi** seçmeniz gerekebilir. **Ayrıca Bildirimler'de**olay merkezi ad alanı oluşturma durumunu da denetleyebilirsiniz.
1. Perakende **mağaza çözümleme** kaynak **grubunda, Olay Hub'ları Ad Alanı'nı**seçin. Portalda **Olay Hub'larınızın Ad Alanı'nın** ana sayfasını görürsünüz.

Artık bir **Olay Hub'ınız**var, IoT Central uygulamanızla birlikte kullanabileceğiniz bir **Olay Hub'ı** oluşturabilirsiniz:

1. Portaldaki **Olay Hub'larınızın Ad Alanı'nın** ana sayfasında **+ Etkinlik Hub'ını**seçin.
1. Olay **Hub'ı Oluştur** sayfasında, ad olarak _mağaza-telemetri_ girin ve sonra **Oluştur'u**seçin.

Artık IoT Central uygulamanızdan veri dışa aktarma yapılandırırken kullanabileceğiniz bir etkinlik hub'ınız var:

![Olay hub'ı](./media/tutorial-in-store-analytics-visualize-insights/event-hub.png)

## <a name="configure-data-export"></a>Veri dışa aktarma yapılandırma

Artık bir etkinlik hub'ınız var, bağlı cihazlardan telemetri ihraç etmek için **mağaza içi analizinizi - ödeme** uygulamanızı yapılandırabilirsiniz. Aşağıdaki adımlar, dışa aktarmayı nasıl yapılandırabileceğinizi gösterir:

1. Mağaza içi **analizleriniz - checkout** IoT Central uygulamanızda oturum açın.
1. Sol bölmede **Veri dışa aktarma'yı** seçin.
1. **Yeni > Azure Etkinlik Hub'larını**seçin.
1. _Telemetri dışa aktarmayı_ **Görüntü Adı**olarak girin.
1. Olay **Hub'larınızı seçin.**
1. **Mağaza-telemetri** olay merkezini seçin.
1. **Dışa aktarma** bölümünde Verilerdeki **Aygıtları** ve **Aygıt Şablonlarını** kapatın.
1. **Kaydet'i**seçin.

Veri dışa aktarma nın etkinlik merkezinize telemetri göndermeye başlaması birkaç dakika sürebilir. **Veri dışa aktarım** sayfasında dışa aktarma durumunu görebilirsiniz:

![Sürekli veri dışa aktarma yapılandırması](./media/tutorial-in-store-analytics-visualize-insights/export-configuration.png)

## <a name="create-the-power-bi-datasets"></a>Power BI veri kümelerini oluşturma

Power BI panonuz, perakende izleme uygulamanızdan elde edilen verileri görüntüler. Bu çözümde, Power BI panosunun veri kaynağı olarak Power BI akış veri kümelerini kullanırsınız. Bu bölümde, mantık uygulamasının olay merkezinden veri aktarabilmesi için akış veri kümelerinin şemasını tanımlarsınız. Aşağıdaki adımlar, çevre sensörleri için iki akış veri kümesi ve doluluk sensörü için bir akış veri kümesi oluşturmanızı gösterir:

1. **Power BI** hesabınızda oturum açın.
1. **Çalışma Alanları'nı**seçin ve ardından **çalışma alanı oluştur'u**seçin.
1. Çalışma **alanı oluşturma** sayfasında, _Çalışma_ Alanı **adı**olarak mağaza içi analiz - ödeme girin.
1. **Mağaza içi analize Hoş Geldiniz - ödeme çalışma alanı** sayfasına gidin ve **Atla'yı**seçin.
1. Çalışma alanı sayfasında, **akış > oluştur veri kümesini**seçin.
1. Yeni **akış veri kümesi** **sayfasında, API'yi**seçin ve sonra **İleri'yi**seçin.
1. **Dataset adı**olarak _Bölge 1 sensörünü_ girin.
1. Aşağıdaki tabloya **akıştan** üç Değer girin:

    | Değer adı  | Değer türü |
    | ----------- | ---------- |
    | Zaman damgası   | DateTime   |
    | Nem oranı    | Sayı     |
    | Sıcaklık | Sayı     |

1. **Geçmiş veri çözümlemesi** değiştirin.
1. **Oluştur'u** ve ardından **Bitti'yi**seçin.
1. **Zone 1 sensörü** akış veri kümesiyle aynı şema ve ayarlara sahip Zone 2 **sensörü** adlı başka bir akış veri kümesi oluşturun.

Artık iki akış veri kümeniz var. Mantık uygulaması, telemetriyi **mağaza içi analizinize** bağlı iki çevresel sensörden bu iki veri kümesine yönlendirecektir:

![Bölge veri kümeleri](./media/tutorial-in-store-analytics-visualize-insights/dataset-1.png)

Bu çözüm, Power BI'de veri akışına filtre uygulamak mümkün olmadığından, her sensör için bir akış veri kümesi kullanır.

Ayrıca doluluk telemetrisi için bir akış veri kümesine de ihtiyacınız var:

1. Çalışma alanı sayfasında, **akış > oluştur veri kümesini**seçin.
1. Yeni **akış veri kümesi** **sayfasında, API'yi**seçin ve sonra **İleri'yi**seçin.
1. **Dataset adı**olarak _Doluluk sensörünü_ girin.
1. Aşağıdaki tabloya **akıştan** beş Değer girin:

    | Değer adı     | Değer türü |
    | -------------- | ---------- |
    | Zaman damgası      | DateTime   |
    | Sıra Uzunluğu 1 | Sayı     |
    | Sıra Uzunluğu 2 | Sayı     |
    | Çalışma Süresi 1   | Sayı     |
    | Çalışma Süresi 2   | Sayı     |

1. **Geçmiş veri çözümlemesi** değiştirin.
1. **Oluştur'u** ve ardından **Bitti'yi**seçin.

Artık simüle edilmiş doluluk sensöründen değerleri depolayan üçüncü bir akış veri setine sahipsiniz. Bu sensör, mağazadaki iki ödeme sırasında ki sıra uzunluğunu ve müşterilerin bu kuyruklarda ne kadar süre beklediğini bildirir:

![Doluluk veri seti](./media/tutorial-in-store-analytics-visualize-insights/dataset-2.png)

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

Bu çözümde, mantık uygulaması olay hub'ından telemetriyi okur, verileri ayrıştirır ve sonra oluşturduğunuz Power BI akış veri kümelerine gönderir.

Mantık uygulamasını oluşturmadan önce, Azure IoT Central öğreticisinde [mağaza içi analiz uygulaması oluşturun'da](./tutorial-in-store-analytics-create-app.md) IoT Central uygulamanıza bağladığınız iki RuuviTag sensörünün aygıt dizinlerine ihtiyacınız vardır:

1. Mağaza içi **analizleriniz - checkout** IoT Central uygulamanızda oturum açın.
1. Sol bölmedeki **Aygıtlar'ı** seçin. Sonra **RuuviTag**seçin.
1. **Aygıt adlarını**not edin. Aşağıdaki ekran görüntüsünde, disler **f5dcf4ac32e8** ve **e29ffc8d5326:**

    ![Cihaz T.C.](./media/tutorial-in-store-analytics-visualize-insights/device-ids.png)

Aşağıdaki adımlar, Azure portalında mantık uygulamasını nasıl oluşturabileceğinizi gösterir:

1. [Azure portalında](https://portal.azure.com) oturum açın ve ekranın sol üst kısmında **bir kaynak oluştur'u** seçin.
1. **Marketi Ara'da**, _Mantık Uygulaması'na_girin ve sonra **Enter**tuşuna basın.
1. Mantık **Uygulaması** sayfasında **Oluştur'u**seçin.
1. Mantık **Uygulaması** oluşturma sayfasında:
    * _Adınız-perakende-mağaza-analizi_gibi mantık uygulamanız için benzersiz bir ad girin.
    * IoT Central uygulamanızı oluşturmak için kullandığınız **Aboneliği** seçin.
    * Perakende **mağaza analizi** kaynak grubunu seçin.
    * IoT Central uygulamanız için kullandığınız konumu seçin.
    * **Oluştur'u**seçin. Sistemin kaynakları sağlaması için birkaç dakika beklemeniz gerekebilir.
1. Azure portalında yeni mantık uygulamanıza gidin.
1. Logic **Apps Designer** sayfasında aşağı kaydırın ve **Boş Mantık Uygulaması'nı**seçin.
1. **Arama bağlayıcıları ve tetikleyicileri,** _Olay Hub'ları_girin.
1. **Tetikleyiciler'de,** **Olaylar Olay Hub'ında kullanılabilir olduğunda'yı**seçin.
1. **Bağlantı adı**olarak _Mağaza telemetrisini_ girin ve **Olay Hub'larınızı**seçin Ad Alanı'nı seçin.
1. **RootManageSharedAccess** ilkesini seçin ve **Oluştur'u**seçin.
1. Olay **Hub eyleminde ne zaman olaylar kullanılabilir:**
    * **Olay Hub adı,** **mağaza-telemetri**seçin.
    * **İçerik türünde,** **uygulama/json'u**seçin.
    * **Aralığı** üçe, **Frekansı** saniyeye ayarlama
1. Mantık uygulamanızı kaydetmek için **Kaydet'i** seçin.

Mantık uygulama tasarımınıza mantık eklemek için **Kod görünümü'nü**seçin:

1. Aşağıdaki `"actions": {},` JSON ile değiştirin. İki yer `[YOUR RUUVITAG DEVICE ID 1]` tutucuyu `[YOUR RUUVITAG DEVICE ID 2]` ve iki RuuviTag aygıtınızdan belirttiğiniz notlarla değiştirin:

    ```json
    "actions": {
        "Initialize_Device_ID_variable": {
            "inputs": {
                "variables": [
                    {
                        "name": "DeviceID",
                        "type": "String"
                    }
                ]
            },
            "runAfter": {},
            "type": "InitializeVariable"
        },
        "Initialize_Interface_ID_variable": {
            "inputs": {
                "variables": [
                    {
                        "name": "InterfaceID",
                        "type": "String",
                        "value": "Other"
                    }
                ]
            },
            "runAfter": {
                "Initialize_Device_ID_variable": [
                    "Succeeded"
                ]
            },
            "type": "InitializeVariable"
        },
        "Parse_Properties": {
            "inputs": {
                "content": "@triggerBody()?['Properties']",
                "schema": {
                    "properties": {
                        "iothub-connection-auth-generation-id": {
                            "type": "string"
                        },
                        "iothub-connection-auth-method": {
                            "type": "string"
                        },
                        "iothub-connection-device-id": {
                            "type": "string"
                        },
                        "iothub-enqueuedtime": {
                            "type": "string"
                        },
                        "iothub-interface-name": {
                            "type": "string"
                        },
                        "iothub-message-source": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "integer"
                        }
                    },
                    "type": "object"
                }
            },
            "runAfter": {
                "Initialize_Interface_ID_variable": [
                    "Succeeded"
                ]
            },
            "type": "ParseJson"
        },
        "Parse_Telemetry": {
            "inputs": {
                "content": "@triggerBody()?['ContentData']",
                "schema": {
                    "properties": {
                        "DwellTime1": {
                            "type": "number"
                        },
                        "DwellTime2": {
                            "type": "number"
                        },
                        "count1": {
                            "type": "number"
                        },
                        "count2": {
                            "type": "number"
                        },
                        "humidity": {
                            "type": "number"
                        },
                        "temperature": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "runAfter": {
                "Initialize_Interface_ID_variable": [
                    "Succeeded"
                ]
            },
            "type": "ParseJson"
        },
        "Set_Device_ID_variable": {
            "inputs": {
                "name": "DeviceID",
                "value": "@body('Parse_Properties')?['iothub-connection-device-id']"
            },
            "runAfter": {
                "Parse_Properties": [
                    "Succeeded"
                ]
            },
            "type": "SetVariable"
        },
        "Set_Interface_ID_variable": {
            "inputs": {
                "name": "InterfaceID",
                "value": "@body('Parse_Properties')?['iothub-interface-name']"
            },
            "runAfter": {
                "Set_Device_ID_variable": [
                    "Succeeded"
                ]
            },
            "type": "SetVariable"
        },
        "Switch_by_DeviceID": {
            "cases": {
                "Occupancy": {
                    "actions": {
                        "Switch_by_InterfaceID": {
                            "cases": {
                                "Dwell_Time_interface": {
                                    "actions": {},
                                    "case": "RS40_Occupancy_Sensor_v2_1l0"
                                },
                                "People_Count_interface": {
                                    "actions": {},
                                    "case": "RS40_Occupancy_Sensor_iv"
                                }
                            },
                            "default": {
                                "actions": {}
                            },
                            "expression": "@variables('InterfaceID')",
                            "runAfter": {},
                            "type": "Switch"
                        }
                    },
                    "case": "Occupancy"
                },
                "Zone 2 environment": {
                    "actions": {},
                    "case": "[YOUR RUUVITAG DEVICE ID 2]"
                },
                "Zone_1_environment": {
                    "actions": {},
                    "case": "[YOUR RUUVITAG DEVICE ID 1]"
                }
            },
            "default": {
                "actions": {}
            },
            "expression": "@variables('DeviceID')",
            "runAfter": {
                "Parse_Telemetry": [
                    "Succeeded"
                ],
                "Set_Interface_ID_variable": [
                    "Succeeded"
                ]
            },
            "type": "Switch"
        }
    },
    ```

1. Eklediğiniz mantığın görsel sürümünü görmek için **Kaydet'i** seçin ve ardından **Designer'ı** seçin:

    ![Mantık uygulaması tasarımı](./media/tutorial-in-store-analytics-visualize-insights/logic-app.png)

1. Eylemi genişletmek için **DeviceID'ye göre geçiş** seçeneğini belirleyin. Ardından **Bölge 1 ortamını**seçin ve **eylem ekle'yi**seçin.
1. **Arama bağlayıcıları ve eylemlerinde,** Power **BI**girin ve sonra **Enter**tuşuna basın.
1. Veri **kümesi (önizleme) eylemine satır ekle'yi** seçin.
1. **Oturum Aç'ı** seçin ve Power BI hesabınızda oturum açma istemlerini izleyin.
1. Oturum açma işlemi tamamlandıktan sonra, **veri kümesi eylemine ekle satırlarında:**
    * Çalışma alanı olarak **mağaza içi analiz - ödeme** yi seçin.
    * Veri kümesi olarak **Bölge 1 sensörünü** seçin.
    * Tablo olarak **RealTimeData'yı** seçin.
    * **Yeni parametre ekle'yi** seçin ve ardından Zaman **Damgası,** **Nem**ve **Sıcaklık** alanlarını seçin.
    * Zaman **Damgası** alanını seçin ve ardından **Dinamik içerik** listesinden **x-opt-enqueuedtime'ı** seçin.
    * **Nem** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını görün'u** seçin. Sonra **nem**seçin.
    * **Sıcaklık** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını gör'üni** seçin. Sonra **sıcaklık**seçin.
    * Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin. **Bölge 1 ortam** eylemi aşağıdaki ekran ![görüntüsüne benzer: Bölge 1 ortamı](./media/tutorial-in-store-analytics-visualize-insights/zone-1-action.png)
1. Bölge **2 ortam** eylemini seçin ve **eylem ekle'yi**seçin.
1. **Arama bağlayıcıları ve eylemlerinde,** Power **BI**girin ve sonra **Enter**tuşuna basın.
1. Veri **kümesi (önizleme) eylemine satır ekle'yi** seçin.
1. Veri **kümesi 2 eylemine satır ekle:**
    * Çalışma alanı olarak **mağaza içi analiz - ödeme** yi seçin.
    * Veri kümesi olarak **Bölge 2 sensörünü** seçin.
    * Tablo olarak **RealTimeData'yı** seçin.
    * **Yeni parametre ekle'yi** seçin ve ardından Zaman **Damgası,** **Nem**ve **Sıcaklık** alanlarını seçin.
    * Zaman **Damgası** alanını seçin ve ardından **Dinamik içerik** listesinden **x-opt-enqueuedtime'ı** seçin.
    * **Nem** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını görün'u** seçin. Sonra **nem**seçin.
    * **Sıcaklık** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını gör'üni** seçin. Sonra **sıcaklık**seçin.
    Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.  **Bölge 2 ortam** eylemi aşağıdaki ekran ![görüntüsüne benzer: Bölge 2 ortamı](./media/tutorial-in-store-analytics-visualize-insights/zone-2-action.png)
1. **Doluluk** eylemini seçin ve ardından **Arabirim Kimliği** eylemine göre geçiş seçeneğini belirleyin.
1. **Dwell Time arabirimi** eylemini seçin ve **eylem ekle'yi**seçin.
1. **Arama bağlayıcıları ve eylemlerinde,** Power **BI**girin ve sonra **Enter**tuşuna basın.
1. Veri **kümesi (önizleme) eylemine satır ekle'yi** seçin.
1. Veri **kümesi eylemine satır ekle:**
    * Çalışma alanı olarak **mağaza içi analiz - ödeme** yi seçin.
    * Veri seti olarak **Doluluk Sensörü'nü** seçin.
    * Tablo olarak **RealTimeData'yı** seçin.
    * **Yeni parametre ekle'yi** seçin ve ardından Zaman **Damgası,** **Dwell Time 1**ve **Dwell Time 2** alanlarını seçin.
    * Zaman **Damgası** alanını seçin ve ardından **Dinamik içerik** listesinden **x-opt-enqueuedtime'ı** seçin.
    * **Dwell Time 1** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını görün'u** seçin. Sonra **DwellTime1**seçin.
    * **Dwell Time 2** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını görün'u** seçin. Sonra **DwellTime2**seçin.
    * Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin. **Dwell Time arabirimi** eylemi aşağıdaki ![ekran görüntüsüne benzer: Doluluk eylemi](./media/tutorial-in-store-analytics-visualize-insights/occupancy-action-1.png)
1. Kişiler **Sayısı arabirimi** eylemini seçin ve **eylem ekle'yi**seçin.
1. **Arama bağlayıcıları ve eylemlerinde,** Power **BI**girin ve sonra **Enter**tuşuna basın.
1. Veri **kümesi (önizleme) eylemine satır ekle'yi** seçin.
1. Veri **kümesi eylemine satır ekle:**
    * Çalışma alanı olarak **mağaza içi analiz - ödeme** yi seçin.
    * Veri seti olarak **Doluluk Sensörü'nü** seçin.
    * Tablo olarak **RealTimeData'yı** seçin.
    * **Yeni parametre ekle'yi** seçin ve ardından Zaman **Damgası,** **Sıra Uzunluğu 1**ve Sıra **Uzunluğu 2** alanlarını seçin.
    * Zaman **Damgası** alanını seçin ve ardından **Dinamik içerik** listesinden **x-opt-enqueuedtime'ı** seçin.
    * Sıra **Uzunluğu 1** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını gör'üni** seçin. Sonra **count1'i**seçin.
    * Sıra **Uzunluğu 2** alanını seçin ve ardından **Parse Telemetri'nin**yanında **daha fazlasını gör'üni** seçin. Sonra **count2'yi**seçin.
    * Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin. **Kişiler Sayısı arabirimi** eylemi aşağıdaki ![ekran görüntüsüne benzer: Doluluk eylemi](./media/tutorial-in-store-analytics-visualize-insights/occupancy-action-2.png)

Mantık uygulaması otomatik olarak çalışır. Her çalıştırmanın durumunu görmek için Azure portalındaki mantık uygulamasıiçin **Genel Bakış** sayfasına gidin:

## <a name="create-a-power-bi-dashboard"></a>Power BI panosu oluşturma

Şimdi etkinlik merkeziniz aracılığıyla IoT Central uygulamanızdan akan telemetri var. Daha sonra mantık uygulamanız olay merkezi iletilerini ayrışdırır ve bunları Power BI akış veri kümesine ekler. Şimdi, telemetri görselleştirmek için bir Güç BI panosu oluşturabilirsiniz:

1. **Power BI** hesabınızda oturum açın.
1. **Mağaza içi analiz> Çalışma Alanları'nı**seçin - ödeme yapın.
1. **> Oluştur Pano'u**seçin.
1. Pano adı olarak **Mağaza analitiğini** girin ve **Oluştur'u**seçin.

### <a name="add-line-charts"></a>Satır grafikleri ekleme

İki çevre sensöründen gelen sıcaklığı ve nemi göstermek için dört satır grafik kutucukekleyin. Kutucukları oluşturmak için aşağıdaki tablodaki bilgileri kullanın. Her döşeme eklemek için, seçerek başlayın **... (Daha fazla seçenek) > Kiremit ekle.** **Özel Akış Verilerini**seçin ve ardından **Sonraki'ni**seçin:

| Ayar | Grafik #1 | Grafik #2 | Grafik #3 | Grafik #4 |
| ------- | -------- | -------- | -------- | -------- |
| Veri kümesi | Bölge 1 sensörü | Bölge 1 sensörü | Bölge 2 sensörü | Bölge 2 sensörü |
| Görselleştirme türü | Çizgi grafik | Çizgi grafik | Çizgi grafik | Çizgi grafik |
| Eksen | Zaman damgası | Zaman damgası | Zaman damgası | Zaman damgası |
| Değerler | Sıcaklık | Nem oranı | Sıcaklık | Nem oranı |
| Zaman penceresi | 60 dakika | 60 dakika | 60 dakika | 60 dakika |
| Başlık | Sıcaklık (1 saat) | Nem oranı (1 saat) | Sıcaklık (1 saat) | Nem oranı (1 saat) |
| Alt başlık | Bölge 1 | Bölge 1 | Bölge 2 | Bölge 2 |

Aşağıdaki ekran görüntüsü ilk grafiğin ayarlarını gösterir:

![Çizgi grafik ayarları](./media/tutorial-in-store-analytics-visualize-insights/line-chart.png)

### <a name="add-cards-to-show-environmental-data"></a>Çevresel verileri göstermek için kart ekleme

İki çevre sensöründen en son sıcaklık ve nem değerlerini göstermek için dört kart kutucağı ekleyin. Kutucukları oluşturmak için aşağıdaki tablodaki bilgileri kullanın. Her döşeme eklemek için, seçerek başlayın **... (Daha fazla seçenek) > Kiremit ekle.** **Özel Akış Verilerini**seçin ve ardından **Sonraki'ni**seçin:

| Ayar | Kart #1 | Kart #2 | Kart #3 | Kart #4 |
| ------- | ------- | ------- | ------- | ------- |
| Veri kümesi | Bölge 1 sensörü | Bölge 1 sensörü | Bölge 2 sensörü | Bölge 2 sensörü |
| Görselleştirme türü | Kart | Kart | Kart | Kart |
| Alanlar | Sıcaklık | Nem oranı | Sıcaklık | Nem oranı |
| Başlık | Sıcaklık (F) | Nem (%) | Sıcaklık (F) | Nem (%) |
| Alt başlık | Bölge 1 | Bölge 1 | Bölge 2 | Bölge 2 |

Aşağıdaki ekran görüntüsü ilk kartın ayarlarını gösterir:

![Kart ayarları](./media/tutorial-in-store-analytics-visualize-insights/card-settings.png)

### <a name="add-tiles-to-show-checkout-occupancy-data"></a>Ödeme doluluk verilerini göstermek için kutucuk ekleme

Mağazadaki iki ödeme için kuyruk uzunluğunu ve çalışma süresini göstermek için dört kart döşemesi ekleyin. Kutucukları oluşturmak için aşağıdaki tablodaki bilgileri kullanın. Her döşeme eklemek için, seçerek başlayın **... (Daha fazla seçenek) > Kiremit ekle.** **Özel Akış Verilerini**seçin ve ardından **Sonraki'ni**seçin:

| Ayar | Kart #1 | Kart #2 | Kart #3 | Kart #4 |
| ------- | ------- | ------- | ------- | ------- |
| Veri kümesi | Doluluk sensörü | Doluluk sensörü | Doluluk sensörü | Doluluk sensörü |
| Görselleştirme türü | Kümelenmiş sütun grafik | Kümelenmiş sütun grafik | Ölçer | Ölçer |
| Eksen    | Zaman damgası | Zaman damgası | Yok | Yok |
| Değer | Çalışma Süresi 1 | Çalışma Süresi 2 | Sıra Uzunluğu 1 | Sıra Uzunluğu 2 |
| Zaman penceresi | 60 dakika | 60 dakika |  Yok | Yok |
| Başlık | Çalışma Zamanı | Çalışma Zamanı | Sırası Uzunluğu | Sırası Uzunluğu |
| Alt başlık | Ödeme 1 | Ödeme 2 | Ödeme 1 | Ödeme 2 |

Panonuzdaki kutucukları aşağıdaki ekran görüntüsüne benzer şekilde yeniden boyutlandırın ve yeniden düzenleyin:

![Power BI panosu](./media/tutorial-in-store-analytics-visualize-insights/pbi-dashboard.png)

Panoyu daha da özelleştirmek için bazı ek grafik kaynakları ekleyebilirsiniz:

![Power BI panosu](./media/tutorial-in-store-analytics-visualize-insights/pbi-dashboard-graphics.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Central uygulamanızı bitirdiyseniz, uygulamada oturum unu atarak ve **İdare** bölümündeki **Uygulama Ayarları** sayfasına gezinerek uygulamayı silebilirsiniz.

Uygulamayı tutmak ancak bununla ilişkili maliyetleri azaltmak istiyorsanız, etkinlik merkezinize telemetri gönderen veri dışa aktarmayı devre dışı tutun.

Azure portalındaki etkinlik hub'ını ve mantık **uygulamasını, perakende mağaza analizi**adı verilen kaynak grubunu silerek silebilirsiniz.

Çalışma alanı için Power BI ayarları sayfasından çalışma alanını silerek Power BI veri kümelerinizi ve panonuzu silebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bu üç öğretici, **mağaza içi analiz - checkout** IoT Central uygulama şablonu kullanan uçtan uca bir çözüm göstermiştir. Aygıtları uygulamaya bağladınız, aygıtları izlemek için IoT Central'ı kullandınız ve aygıt telemetrisinden gelen istatistikleri görüntülemek için bir pano oluşturmak için Power BI'yi kullandınız. Önerilen bir sonraki adım, diğer IoT Merkezi uygulama şablonlarından birini keşfetmektir:

> [!div class="nextstepaction"]
> * [IoT Central ile enerji sektörü çözümleri oluşturma](../energy/overview-iot-central-energy.md)
> * [IoT Central ile kamu çözümleri oluşturma](../government/overview-iot-central-government.md)
> * [IoT Central ile sağlık çözümleri oluşturma](../healthcare/overview-iot-central-healthcare.md)
