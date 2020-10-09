---
ms.openlocfilehash: 2349939d4997ddc57d0c0c56a21eeec0357bf0ec
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91829046"
---
Örnek kodu çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Code, **Uzantılar** sekmesini açın (veya CTRL + SHIFT + X tuşlarına basın) ve Azure IoT Hub aratın.
1. Sağ tıklayıp **uzantı ayarları**' nı seçin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="../../../media/run-program/extensions-tab.png" alt-text="Uzantı ayarları&quot;:::
1. &quot;Ayrıntılı Iletiyi göster" i arayın ve etkinleştirin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="../../../media/run-program/show-verbose-message.png" alt-text="Uzantı ayarları&quot;:::
1. &quot;Ayrıntılı Iletiyi göster"
          }
        ]
      }
    }
    ```
     
    * `GraphInstanceActivate`Grafik örneğini ve videonun akışını başlatan öğesine yönelik bir çağrı.
    * `GraphInstanceList`Grafik örneğinin çalışır durumda olduğunu gösteren ikinci bir çağrı.
1. **TERMINAL** penceresindeki çıktı, tarihinde duraklatılır `Press Enter to continue` . Henüz ENTER ' ı seçmeyin. Doğrudan çağrdığınız yöntemler için JSON yanıtı yüklerini görmek üzere yukarı kaydırın.
1. Visual Studio Code **Çıkış** penceresine geçin. IoT Edge modülündeki canlı video analizinin IoT Hub 'ına gönderdiğini iletiler görürsünüz. Bu hızlı başlangıçta aşağıdaki bölümde bu iletiler ele alınmaktadır.
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, **TERMINAL** penceresine dönün ve ENTER ' u seçin. 

    Sonraki çağrı dizisi kaynakları temizler:

    * `GraphInstanceDeactivate`Grafik örneğini devre dışı bırakmak için bir çağrı.
    * `GraphInstanceDelete`Örneği silme çağrısı.
    * İçin bir çağrı `GraphTopologyDelete` , topolojiyi siler.
    * İçin nihai bir çağrı `GraphTopologyList` , listenin boş olduğunu gösterir.
