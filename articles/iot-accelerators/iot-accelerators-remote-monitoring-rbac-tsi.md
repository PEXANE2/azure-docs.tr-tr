---
title: Uzaktan İzleme veri erişim kontrolü - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcısındaki Time Series Insights telemetri gezgini için erişim denetimlerini nasıl yapılandırabileceğiniz hakkında bilgi
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 9d5d572c3e32e3645e65ba8d6fc28b567b3c1e9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65827177"
---
# <a name="configure-access-controls-for-the-time-series-insights-telemetry-explorer"></a>Zaman Serisi Öngörüleri telemetri gezgini için erişim denetimlerini yapılandırma

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısındaki Zaman Serisi Öngörüler kaşifi için erişim denetimlerinin nasıl yapılandırılacakıdır hakkında bilgi verilmektedir. Çözüm hızlandırıcıkullanıcılarının Time Series Insights gezginine erişmesine izin vermek için her kullanıcıya veri erişimi vermeniz gerekir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir kullanıcı ilkesi için veri erişimi sağlamak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Time Series Insights ortamınızı bulun. **Arama** kutusuna **Time Serisi** yazın. Arama sonuçlarında **Zaman Serisi Ortamı'nı** seçin. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

4. **Veri Erişim İlkeleri'ni**seçin, ardından **+ Ekle'yi**seçin.
    ![Zaman Serisi Görüşleri kaynağını yönetme - ortam](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access1.png)

5. **Kullanıcı seç'i**seçin.  Eklemek istediğiniz kullanıcıyı bulmak için kullanıcı adını veya e-posta adresini arayın. Seçimi onaylamak için **Seç'i** tıklatın. 

    ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access2.png)

6. **Rolü Seç'i**seçin. Kullanıcı için uygun erişim rolünü seçin:
   - Kullanıcının başvuru verilerini değiştirmesine ve kaydedilen sorguları ve perspektifleri ortamın diğer kullanıcılarıyla paylaşmasına izin vermek istiyorsanız **Katılımcı'yı** seçin. 
   - Aksi takdirde, kullanıcı nın ortamdaki sorgu verilerine izin vermek ve ortamdaki kişisel (paylaşılmayan) sorguları kaydetmek için **Reader'ı** seçin.

     Rol seçimini onaylamak için **Tamam'ı** seçin.

     ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access3.png)

7. Kullanıcı Rolü **Seç** sayfasında **Tamam'ı** seçin.

    ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access4.png)

8. **Veri Erişim İlkeleri** sayfası, her kullanıcı için kullanıcıları ve rolü(ler) listeler.

    ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısındaki Zaman Serisi Öngörüler kaşifi için erişim denetimlerinin nasıl verildiğini öğrendiniz.

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için [bkz.](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan İzleme çözümünü özelleştirme hakkında daha fazla bilgi için, [bir microservice'i Özelleştir ve yeniden dağıtma](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->