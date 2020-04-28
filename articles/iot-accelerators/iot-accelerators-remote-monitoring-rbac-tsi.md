---
title: Uzaktan Izleme veri erişimi denetimi-Azure | Microsoft Docs
description: Bu makalede, uzaktan Izleme Çözüm Hızlandırıcısı 'nda Time Series Insights telemetri Gezgini için erişim denetimlerini nasıl yapılandırabileceğiniz hakkında bilgi sağlanmaktadır
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 9d5d572c3e32e3645e65ba8d6fc28b567b3c1e9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "65827177"
---
# <a name="configure-access-controls-for-the-time-series-insights-telemetry-explorer"></a>Time Series Insights telemetri Gezgini için erişim denetimlerini yapılandırma

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında Time Series Insights Gezgini için erişim denetimlerinin nasıl yapılandırılacağı hakkında bilgi sağlanır. Çözüm hızlandırıcısının kullanıcılarının Time Series Insights gezgin 'e erişmesine izin vermek için, her kullanıcı verilerine erişim izni vermeniz gerekir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir Kullanıcı sorumlusu için veri erişimi sağlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Time Series Insights ortamınızı bulun. **Arama** kutusuna **zaman serisini** yazın. Arama sonuçlarında **zaman serisi ortamını** seçin. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

4. **Veri erişim ilkeleri**' ni seçin ve **+ Ekle**' yi seçin.
    ![Zaman Serisi Görüşleri kaynağını yönetme - ortam](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access1.png)

5. **Kullanıcı Seç**' i seçin.  Eklemek istediğiniz kullanıcıyı bulmak için Kullanıcı adını veya e-posta adresini arayın. Seçimi onaylamak için **Seç** ' e tıklayın. 

    ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access2.png)

6. **Rol Seç**' i seçin. Kullanıcı için uygun erişim rolünü seçin:
   - Kullanıcının başvuru verilerini değiştirmesine ve kaydedilmiş sorguları ve perspektifleri ortamın diğer kullanıcılarıyla paylaşmasına izin vermek istiyorsanız **katılımcı** ' ı seçin. 
   - Aksi takdirde, ortamda Kullanıcı sorgulama verilerine izin vermek ve kişisel (paylaşılmayan) sorguları ortamda kaydetmek için **okuyucu** ' yı seçin.

     Rol seçimini onaylamak için **Tamam ' ı** seçin.

     ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access3.png)

7. **Kullanıcı rolü Seç** sayfasında **Tamam ' ı** seçin.

    ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access4.png)

8. **Veri erişim ilkeleri** sayfası, her kullanıcı için kullanıcıları ve rolleri listeler.

    ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uzaktan Izleme Çözüm Hızlandırıcısı 'nda Time Series Insights Gezgini için erişim denetimlerinin nasıl verildiğini öğrendiniz.

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan Izleme çözümünü özelleştirme hakkında daha fazla bilgi için bkz. [bir mikro hizmeti özelleştirme ve yeniden dağıtma](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->