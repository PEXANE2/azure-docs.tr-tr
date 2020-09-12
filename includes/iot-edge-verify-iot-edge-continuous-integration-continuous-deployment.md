---
author: v-tcassi
ms.service: iot-edge
ms.topic: include
ms.date: 08/26/2020
ms.author: v-tcassi
ms.openlocfilehash: c7be10e14daf3ee769e86d51f648cc6b656a416a
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303730"
---
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Derleme ve yayın işlem hatları ile IoT Edge CI/CD 'yi doğrulama

Bir yapı işini tetiklemek için, kaynak kodu deposuna bir kayıt gönderebilir veya el ile tetikleyebilirsiniz. Bu bölümde, onun çalıştığını test etmek için CI/CD işlem hattını el ile tetiklersiniz. Sonra dağıtımın başarılı olduğunu doğrulayın.

1. Sol bölme menüsünde, işlem **hatları** ' nı seçin ve bu makalenin başlangıcında oluşturduğunuz derleme işlem hattını açın.

2. Yapı işlem hattınızda derleme işini, sağ üst köşedeki işlem hattını **Çalıştır** düğmesini seçerek tetikleyebilirsiniz.

    ![Ardışık düzen Çalıştır düğmesini kullanarak derleme işlem hattınızı el ile tetikleme](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/manual-trigger.png)

3. Çalıştırma işlem **hattı** ayarlarını gözden geçirin. Sonra **Çalıştır**' ı seçin.

    ![Çalıştırma işlem hattı seçeneklerini belirtin ve Çalıştır 'ı seçin](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/run-pipeline-settings.png)

4. Çalıştırmanın ilerlemesini izlemek için **Aracı işi 1** ' i seçin. İşi seçerek iş çıkışının günlüklerini gözden geçirebilirsiniz. 

    ![İşin günlük çıkışını gözden geçirin](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/view-job-run.png)

5. Derleme işlem hattı başarıyla tamamlanırsa, **geliştirme** aşamasına bir yayın tetikler. Başarılı **geliştirme** sürümü, IoT Edge cihazlarını hedeflemek için IoT Edge dağıtımı oluşturur.

    ![Geliştirme sürümü](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/pending-approval.png)

6. Yayın günlüklerini görmek için **geliştirme** aşaması ' na tıklayın.

    ![Yayın günlükleri](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/release-logs.png)