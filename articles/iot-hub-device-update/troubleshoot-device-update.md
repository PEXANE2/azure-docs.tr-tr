---
title: Azure IoT Hub sorunları için yaygın cihaz güncelleştirmesi sorunlarını giderme | Microsoft Docs
description: Bu belgede IoT Hub cihaz güncelleştirmesiyle ilgili olabilecek birçok sorunu gidermenize yardımcı olacak ipuçları ve püf noktaları listesi sağlanmaktadır.
author: lichris
ms.author: lichris
ms.date: 2/17/2021
ms.topic: troubleshooting
ms.service: iot-hub-device-update
ms.openlocfilehash: 3c1f60b214397b1f97e0157b5beca32d504102d6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102030639"
---
# <a name="device-update-for-iot-hub-troubleshooting-guide"></a>IoT Hub sorun giderme kılavuzu için cihaz güncelleştirmesi

Bu belgede, bazı yaygın soruların ve cihaz güncelleştirme kullanıcılarının bildirdiği sorunlar listelenmektedir. Cihaz güncelleştirmesi genel önizlemeye ilerledikçe, bu sorun giderme kılavuzu yeni sorular ve çözümlerle düzenli aralıklarla güncelleştirilir. Bu sorun giderme kılavuzunda görünmeyen bir sorunla karşılaşırsanız, durumunuzu belgelemek için [iletişim Microsoft desteği](#contact) bölümüne bakın.

## <a name="importing-updates"></a><a name="import"></a>Güncelleştirmeler içeri aktarılıyor

### <a name="q-im-having-trouble-connecting-my-device-update-instance-to-my-iot-hub-instance"></a>S: cihazımın güncelleştirme örneğinden IoT Hub örneğine bağlanırken sorun yaşıyorum.
_Lütfen IoT Hub ileti yollarınızın [Cihaz Güncelleştirme Kaynakları](./device-update-resources.md) belgelerine göre doğru yapılandırıldığından emin olun._

### <a name="q-im-encountering-a-role-related-error-error-message-in-azure-portal-or-a-403-api-error"></a>S: rolle ilgili bir hatayla karşılaştım (Azure portal veya 403 API hatası).
_Erişim izinleriniz doğru şekilde yapılandırılmamış olabilir. Lütfen [cihaz güncelleştirme erişim denetimi](./device-update-control-access.md) belgelerine göre erişim izinlerini doğru yapılandırdığınızdan emin olun._

### <a name="q-im-encountering-a-500-type-error-when-importing-content-to-the-device-update-service"></a>S: cihaz güncelleştirme hizmetine içerik aktarırken 500 türünde bir hatayla karşılaştım.
_500 aralığındaki bir hata kodu, cihaz güncelleştirme hizmeti ile ilgili bir sorun olduğunu gösteriyor olabilir. Lütfen 5 dakika bekleyin ve sonra yeniden deneyin. Aynı hata devam ederse, Microsoft ile bir destek isteği sağlamak için lütfen [iletişim Microsoft desteği](#contact) bölümündeki yönergeleri izleyin._

### <a name="q-im-encountering-an-error-code-when-importing-content-and-would-like-to-parse-it"></a>S: içeriği içeri aktarırken bir hata kodu ile karşılaştım ve ayrıştırmak için.
_Hata kodlarını ayrıştırma hakkında bilgi için lütfen [cihaz güncelleştirme hata kodları](./device-update-error-codes.md) belgelerine bakın._

## <a name="device-failures"></a><a name="device-failure"></a>Cihaz arızaları

### <a name="q-how-can-i-ensure-my-device-is-connected-to-device-update-for-iot-hub"></a>S: cihazımın IoT Hub için cihaz güncelleştirmesine bağlı olduğundan nasıl emin olabilirim?
_Azure portal uyumluluk görünümündeki "Gruplandırılmamış" cihazlar bölümünde görünür olup olmadığını denetleyerek cihazınızın cihaz güncelleştirmesine bağlı olduğunu doğrulayabilirsiniz._

### <a name="q-one-or-more-of-my-devices-is-failing-to-update"></a>S: Cihazlarım bir veya daha fazla güncelleştiremedi.
_Bir cihaz güncelleştirme hatasının birçok olası temel nedeni vardır. Lütfen cihazın şu şekilde olduğunu doğrulayın: 1) cihaz güncelleştirme örneğinize bağlı olan, 2) IoT Hub örneğine bağlı ve 3) teslim Iyileştirme (DO) hizmeti çalışıyor. Cihazınız için üçü de geçerliyse, Microsoft ile bir destek isteği sağlamak için lütfen [iletişim Microsoft desteği](#contact) bölümündeki yönergeleri izleyin._

## <a name="deploying-an-update"></a><a name="deploy"></a> Güncelleştirme dağıtma

### <a name="q-ive-deployed-an-update-to-my-devices-but-the-compliance-status-says-it-isnt-on-the-latest-update-what-should-i-do"></a>S: Cihazlarım için bir güncelleştirme dağıttım, ancak uyumluluk durumu en son güncelleştirmede olmadığını söylüyor. Ne yapmalıyım?
_Cihazın uyumluluk durumunun yenilenmesi 5 dakika sürebilir. Lütfen bekleyin, sonra tekrar denetleyin._
### <a name="q-my-devices-deployment-status-shows-incompatible-what-should-i-do"></a>S: cihazımın dağıtım durumu uyumsuz olarak gösteriliyor, ne yapmam gerekir?
_Hedeflenen bir cihazın üretici ve model özellikleri, cihaz IoT Hub bağlandıktan sonra değiştirilmiş olabilir, böylece cihaz artık geçerli dağıtımın güncelleştirme içeriğiyle uyumsuz olarak değerlendirilir._

_Cihazınızın cihaz güncelleştirme hizmetine rapor ettiği üretici ve modeli görmek için [ADU Core arabirimini](./device-update-plug-and-play.md) denetleyin ve dağıtılan güncelleştirme içeriğinin [içeri aktarma bildiriminde](./import-concepts.md) belirttiğiniz üretici ve modelle eşleştiğinden emin olun. [Cihaz güncelleştirme yapılandırma dosyasını](./device-update-configuration-file.md)kullanarak, belirli bir cihaz için bu özellikleri değiştirebilirsiniz._

### <a name="q-i-see-my-deployment-is-in-active-stage-but-none-of-my-devices-are-in-progress-with-the-update-what-should-i-do"></a>S: dağıtımımın "etkin" aşamada olduğunu görmem, ancak cihazımın hiçbiri güncelleştirme ile "devam ediyor". Ne yapmalıyım?
_Dağıtım başlangıç tarihinin gelecekte ayarlı olmadığından emin olun. Yeni bir dağıtım oluşturduğunuzda, açıkça değiştirmediğiniz sürece dağıtım başlangıç tarihi bir sonraki güne karşı koruma olarak ayarlanır. Dağıtım başlangıç tarihinin gelmesini bekleyebilir ya da devam eden dağıtımı iptal edebilir ve istenen başlangıç tarihiyle yeni bir dağıtım oluşturabilirsiniz._

### <a name="q-im-trying-to-group-my-devices-but-i-dont-see-the-tag-in-the-drop-down-when-creating-a-group"></a>S: cihazlarımı gruplamak istiyorum, ancak bir grup oluştururken açılan kutuda etiketi görmüyorum.
_[Cihaz Güncelleştirme Kaynakları](./device-update-resources.md) belgelerine göre IoT Hub ileti yollarını doğru şekilde yapılandırdığınızdan emin olun. Yolu yapılandırdıktan sonra cihazınızı yeniden etiketlemelisiniz._

_Başka bir kök neden, cihazınızı IoT Hub cihaz güncelleştirmesine bağlamadan önce etiketi uygulamış olabilir. Cihazınızın cihaz güncelleştirmesine zaten bağlı olduğundan emin olun. Uyumluluk görünümündeki "Gruplandırılmamış" cihazlar altında olup olmadığını denetleyerek, cihazınızın IoT Hub cihaz güncelleştirmesine bağlandığını doğrulayabilirsiniz. Geçici olarak farklı bir değerin etiketini ekleyin ve ardından cihaz bağlandıktan sonra amaçlanan etiketinizi yeniden ekleyin._

_Cihaz sağlama hizmeti 'ni (DPS) kullanıyorsanız, cihazlarınızın cihaz oluşturma işlemi sırasında sağlandıktan sonra değil, bunları etiketledikten emin olun. Cihazı cihaz oluşturma adımı sırasında zaten etiketlediyseniz, sağlandıktan sonra cihazınızı farklı bir değerle geçici olarak etiketledikten sonra amaçlanan etiketinizi yeniden eklemeniz gerekir._

### <a name="q-my-deployment-completed-successfully-but-some-devices-failed-to-update"></a>S: dağıtımım başarıyla tamamlandı, ancak bazı cihazlar güncelleştirilemedi.
_Bunun nedeni, başarısız cihazlarda istemci tarafı hatası olabilir. Lütfen bu sorun giderme kılavuzunun cihaz arızaları bölümüne bakın._

### <a name="q-i-encountered-an-error-in-the-ux-when-trying-to-initiate-a-deployment"></a>S: bir dağıtım başlatılmaya çalışılırken UX içinde bir hatayla karşılaştım.
_Bunun nedeni bir hizmet/UX hatası veya bir API izinleri sorunu olabilir. Microsoft ile destek isteği sağlamak için lütfen [iletişim Microsoft desteği](#contact) bölümündeki yönergeleri izleyin._

### <a name="q-i-started-a-deployment-but-it-isnt-reaching-an-end-state"></a>S: bir dağıtım başlatıyorum, ancak bitiş durumuna ulaşmıyor.
_Bunun nedeni bir hizmet performansı sorunu, bir hizmet hatası veya bir istemci hatası olabilir. Lütfen dağıtımınızı 10 dakika sonra yeniden deneyin. Aynı sorunla karşılaşırsanız lütfen cihaz günlüklerinizi çekin ve bu sorun giderme kılavuzunun cihaz arızaları bölümüne bakın. Aynı sorun devam ederse, Microsoft ile bir destek isteği sağlamak için lütfen [iletişim Microsoft desteği](#contact) bölümündeki yönergeleri izleyin._

## <a name="downloading-updates-onto-devices"></a><a name="download"></a> Güncelleştirmeleri cihazlara indirme

### <a name="q-how-do-i-resume-a-download-when-a-device-has-reconnected-after-a-period-of-disconnection"></a>S: Nasıl yaparım? bir cihaz bir bağlantı kesildikten sonra yeniden bağlandığında indirmeyi sürdürür mi?
_İndirme işlemi, 24 saatlik bir süre içinde bağlantı geri yüklendiğinde kendi kendine sürdürülecek. 24 saat sonra, indirme işleminin Kullanıcı tarafından yeniden başlatılması gerekir._
## <a name="using-microsoft-connected-cache-mcc"></a><a name="mcc"></a> Microsoft bağlı önbelleği (MCC) kullanma

### <a name="q-i-am-encountering-an-issue-when-attempting-to-deploy-the-mcc-module-on-my-iot-edge-device"></a>S: IoT Edge cihazmda MCC modülünü dağıtmaya çalışırken bir sorunla karşılaştım.
_Edge modüllerini IoT Edge cihazlara dağıtmaya yönelik [IoT Edge belgelerine]() bakın. ' A giderek, MCC modülünün IoT Edge cihazınızda başarıyla çalışıp çalışmadığını kontrol edebilirsiniz http://localhost:5100/Summary ._
### <a name="q-one-of-my-iot-devices-is-attempting-to-download-an-update-through-mcc-but-is-failing"></a>S: IoT Cihazlarım bir güncelleştirmeyi MCC aracılığıyla indirmeye çalışıyor ancak başarısız oluyor.
_IoT cihazının MCC 'e bağlanmada başarısız olmasına neden olabilecek çeşitli sorunlar vardır. Sorunu tanılamak için lütfen başarısız olan cihazdan DO Client ve NGINX günlüklerini toplayın (istemci günlüklerinin toplanması hakkında yönergeler için bkz. [iletişim Microsoft desteği](#contact) bölümüne bakın)._

_Kullanmakta olduğu URL 'ye izin verilmediğinden, cihazınız, Internet 'ten içerikleri, MCC modülüne geçiremiyor olabilir. Bunu öğrenmek için, Azure portal IoT Edge ortam değişkenlerinizi denetlemeniz gerekecektir._
## <a name="contacting-microsoft-support"></a><a name="contact"></a> Microsoft Desteği bağlanılıyor

Yukarıdaki SSS 'ler kullanılarak çözümlenemeyen sorunlarla karşılaşırsanız, Azure portal arabirimi aracılığıyla Microsoft Desteği bir destek isteği oluşturabilirsiniz. Sorununuzu hangi kategoriye ait olduğuna bağlı olarak, sorununuzu araştırmak Microsoft Desteği yardımcı olması için ek veri toplayıp paylaşmanız istenebilir. 

Her bir veri türünü nasıl toplayacağınız hakkında yönergeler için lütfen aşağıya bakın. API 'nin yük yanıtında ek bilgileri denetlemek için [GetDevices]() ' i kullanabilirsiniz.

Ayrıca, aşağıdaki bilgiler, sorununuzun kök nedenini daraltmak için yararlı olabilir:
* Güncelleştirmeye çalıştığınız cihaz türü (Azure Percept, IoT Edge Gateway, diğer)
* Kullandığınız cihaz güncelleştirme istemci türü (görüntü tabanlı, paket tabanlı, simülatör)
* Cihazınızın çalıştırdığı işletim sistemi
* Cihazınızın mimarisine ilişkin ayrıntılar
* Cihaz güncelleştirmesini bir cihazı daha önce güncelleştirmek için başarıyla kullandıysanız

Yukarıdaki bilgilerden birini kullanıyorsanız, lütfen sorunun açıklamasına ekleyin.

### <a name="collecting-client-logs"></a>İstemci günlüklerini toplama

* Raspberry PI cihazında, burada iki günlük kümesi bulunur:

    ```markdown
    /adu/logs
    ```

    ```markdown
    /var/cache/do-client-lite/log
    ```

* Paketlenmiş istemci için Günlükler burada bulunur:

    ```markdown
    /var/log/adu
    ```

    ```markdown
    /var/cache/do-client-lite/log
    ```

* Simülatör için Günlükler şurada bulunur:

    ```markdown
    /tmp/aduc-logs
    ```

### <a name="error-codes"></a>Hata kodları
Güncelleştirme alma, cihaz hatası veya güncelleştirme dağıtma ile ilgili bir sorun bildirirken hata kodları sağlamanız istenebilir.

Hata kodları [Aducoreınterface](./device-update-plug-and-play.md) arabirimine bakılarak elde edilebilir. Kendi kendine tanılama ve sorun giderme için hata kodlarını ayrıştırma hakkında daha fazla bilgi için lütfen [cihaz güncelleştirme hata kodları](./device-update-error-codes.md) belgelerine bakın.

### <a name="trace-id"></a>İzleme KIMLIĞI
Bir güncelleştirmeyi içeri aktarmaya veya dağıtmaya ilişkin bir sorun bildirirken bir izleme KIMLIĞI sağlamanız istenebilir.

Belirli bir kullanıcı eyleminin izleme KIMLIĞI API yanıtı içinde veya Azure portal Kullanıcı arabiriminin Içeri aktarma geçmişi bölümünde bulunabilir. 

Şu anda, dağıtım eylemlerine yönelik izleme kimliklerine yalnızca API yanıtı üzerinden erişilebilir.

### <a name="deployment-id"></a>Dağıtım KIMLIĞI
Güncelleştirme dağıtımıyla ilgili bir sorun bildirirken bir dağıtım KIMLIĞI sağlamanız istenebilir.

Dağıtım KIMLIĞI, bir dağıtımı başlatmak için API çağrılırken Kullanıcı tarafından oluşturulur.

Şu anda, Azure portal kullanıcı arabiriminden başlatılan dağıtımlar için dağıtım kimlikleri otomatik olarak oluşturulur ve kullanıcıya verilmez.

### <a name="iot-hub-instance-name"></a>IoT Hub örneği adı
Cihaz hatalarıyla ilgili bir sorun bildirirken veya bir güncelleştirme dağıtımı yaparken IoT Hub örneğinizin adını belirtmeniz istenebilir.

IoT Hub adı, ilk sağlandığı Kullanıcı tarafından seçilir.

### <a name="device-update-account-name"></a>Cihaz güncelleştirme hesabı adı
Güncelleştirme alma, cihaz arızaları veya güncelleştirme dağıtma ile ilgili bir sorun bildirirken cihaz güncelleştirme hesabınızın adını sağlamanız istenebilir.

Cihaz güncelleştirme hesabı adı, ilk olarak hizmete kaydolurken Kullanıcı tarafından seçilir. [Cihaz Güncelleştirme Kaynakları](./device-update-resources.md) belgelerinde daha fazla bilgi bulunabilir.

### <a name="device-update-instance-name"></a>Cihaz güncelleştirme örneği adı
Güncelleştirme alma, cihaz arızaları veya güncelleştirme dağıtma ile ilgili bir sorun bildirirken cihaz güncelleştirme örneğinin adını sağlamanız istenebilir.

Cihaz güncelleştirme örneği adı, ilk sağlandıysa Kullanıcı tarafından seçilir. [Cihaz Güncelleştirme Kaynakları](./device-update-resources.md) belgelerinde daha fazla bilgi bulunabilir.

### <a name="device-id"></a>Cihaz Kimliği
Cihaz hatalarıyla ilgili bir sorun bildirirken veya bir güncelleştirme dağıtımı yaptığınızda bir cihaz KIMLIĞI sağlamanız istenebilir.

Cihaz KIMLIĞI, cihaz ilk sağlandığında müşteri tarafından tanımlanır. Cihazın cihaz Ikizi de alınabilir.

### <a name="update-id"></a>Güncelleştirme KIMLIĞI
Güncelleştirme dağıtımıyla ilgili bir sorun bildirirken bir güncelleştirme KIMLIĞI sağlamanız istenebilir.

Güncelleştirme KIMLIĞI, bir dağıtım başlatılırken müşteri tarafından tanımlanır.

### <a name="nginx-logs"></a>NGINX günlükleri
Microsoft bağlı önbelleğiyle ilgili bir sorun bildirirken NGINX günlükleri sağlamanız istenebilir.

### <a name="adu-conftxt"></a>ADU-conf.txt
Güncelleştirme dağıtımıyla ilgili bir sorun bildirirken cihaz güncelleştirme yapılandırma dosyasını ("adu-conf.txt") sağlamanız istenebilir.

Yapılandırma dosyası isteğe bağlıdır ve [Cihaz Güncelleştirme yapılandırması](./device-update-configuration-file.md) belgelerindeki yönergeleri izleyerek Kullanıcı tarafından oluşturulur.

### <a name="import-manifest"></a>İçeri aktarma bildirimi
Bir güncelleştirmeyi içeri aktarmaya veya dağıtmaya ilişkin bir sorun bildirirken içeri aktarma bildirim dosyanızı sağlamanız istenebilir.

İçeri aktarma bildirimi, güncelleştirme içeriği cihaz güncelleştirme hizmetine aktarılırken müşteri tarafından oluşturulan bir dosyadır.

**[Sonraki adım: cihaz güncelleştirme hata kodları hakkında daha fazla bilgi edinin](.\device-update-error-codes.md)**