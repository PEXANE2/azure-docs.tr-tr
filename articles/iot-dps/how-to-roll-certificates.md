---
title: Azure IoT Hub Cihaz Sağlama Hizmetinde X.509 sertifikaları rulosu
description: X.509 sertifikaları, Cihaz Sağlama Hizmeti (DPS) örneğinizle nasıl yuvarlar
author: wesmc7777
ms.author: wesmc
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 4d5ddb229cd6a41235990437bc0f8db08e3381ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974896"
---
# <a name="how-to-roll-x509-device-certificates"></a>X.509 cihaz sertifikaları nasıl yuvarlenir?

IoT çözümünüzün yaşam döngüsü sırasında sertifikaları yuvarlamanız gerekir. Sertifikaların yuvarlanma temel nedenlerinden ikisi bir güvenlik ihlali ve sertifika nın son kullanma tarihleri olacaktır. 

Rolling sertifikaları, bir ihlal durumunda sisteminizin güvenliğini sağlamaya yardımcı olmak için en iyi güvenlik uygulamasıdır. [Assume Breach Metodolojisinin](https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)bir parçası olarak, Microsoft önleyici önlemlerle birlikte reaktif güvenlik süreçlerinin yerinde olması gereğini savunur. Cihaz sertifikalarınızın kaydedilmesi bu güvenlik işlemlerinin bir parçası olarak eklenmelidir. Sertifikalarınızı alma sıklığınız, çözümünüzün güvenlik gereksinimlerine bağlıdır. Son derece hassas verileri içeren çözümlere sahip müşteriler sertifikalarını her gün yuvarlayabilirken, diğerleri sertifikalarını birkaç yılda bir yuvarlayabilir.

Haddeleme cihazı sertifikaları, aygıtta ve IoT hub'ında depolanan sertifikanın güncelleştirilmesini içerir. Daha sonra, aygıt, Aygıt Sağlama Hizmeti ile normal [otomatik sağlama](concepts-auto-provisioning.md) kullanarak IoT hub'ı ile kendisini yeniden sağlayabilir.


## <a name="obtain-new-certificates"></a>Yeni sertifikalar edinin

IoT aygıtlarınız için yeni sertifikalar almanın birçok yolu vardır. Bunlar arasında aygıt fabrikasından sertifika almak, kendi sertifikalarınızı oluşturmak ve üçüncü bir tarafın sizin için sertifika oluşturmayı yönetmesi yer alır. 

Sertifikalar, kök CA sertifikasından [yaprak sertifikasına](concepts-security.md#end-entity-leaf-certificate)kadar bir güven zinciri oluşturmak için birbirleri tarafından imzalanır. İmza sertifikası, güven zincirinin sonundaki yaprak sertifikasını imzalamak için kullanılan sertifikadır. İmza sertifikası kök CA sertifikası veya güven zincirinde bir ara sertifika olabilir. Daha fazla bilgi için [X.509 sertifikalarına](concepts-security.md#x509-certificates)bakın.
 
İmza sertifikası almanın iki farklı yolu vardır. Üretim sistemleri için önerilen ilk yol, bir kök sertifika yetkilisinden (CA) imza sertifikası satın almaktır. Bu şekilde güvenliği güvenilir bir kaynağa zincirler. 

İkinci yol OpenSSL gibi bir araç kullanarak kendi X.509 sertifikaları oluşturmaktır. Bu yaklaşım X.509 sertifikalarını test etmek için harikadır, ancak güvenlik çevresinde birkaç garanti sağlar. Kendi CA sağlayıcınız olarak davranmaya hazır değilseniz, bu yaklaşımı yalnızca sınama amaçlı kullanmanızı öneririz.
 

## <a name="roll-the-certificate-on-the-device"></a>Sertifikayı aygıta yuvarla

Aygıttaki sertifikalar her zaman donanım güvenlik modülü [(HSM)](concepts-device.md#hardware-security-module)gibi güvenli bir yerde depolanmalıdır. Aygıt sertifikalarını yuvarlama şekliniz, aygıtlarda nasıl oluşturulduklarına ve aygıtlara nasıl yüklendiklerine bağlıdır. 

Sertifikalarınızı üçüncü bir taraftan aldıysanız, sertifikalarını nasıl yuvarladıklarını araştırmalısınız. İşlem, onlarla olan düzenlemenize dahil edilebilir veya sundukları ayrı bir hizmet olabilir. 

Kendi aygıt sertifikalarınızı yönetiyorsanız, sertifikaları güncelleştirmek için kendi ardışık alanınızı oluşturmanız gerekir. Hem eski hem de yeni yaprak sertifikalarının aynı ortak ada (CN) sahip olduğundan emin olun. Aynı CN'ye sahip olarak, aygıt yinelenen bir kayıt kaydı oluşturmadan kendisini yeniden sağlayabilir. 


## <a name="roll-the-certificate-in-the-iot-hub"></a>Sertifikayı IoT hub'ına yuvarlayın

Aygıt sertifikası el ile bir IoT hub'ına eklenebilir. Sertifika, aygıt sağlama hizmeti örneği kullanılarak da otomatikhale getirilebilir. Bu makalede, otomatik sağlamayı desteklemek için bir Aygıt Sağlama hizmeti örneğinin kullanıldığını varsayıyoruz.

Bir aygıt başlangıçta otomatik sağlama yoluyla sağlandığında, önyükleme yapılır ve sağlama hizmetiyle bağlantı kurarak hizmete başlar. Sağlama hizmeti, aygıtın yaprak sertifikasını kimlik bilgisi olarak kullanarak bir IoT hub'ında aygıt kimliği oluşturmadan önce kimlik denetimi gerçekleştirerek yanıt verir. Sağlama hizmeti daha sonra aygıta atandığını söyler ve aygıt kimlik doğrulaması yapmak ve IoT hub'ına bağlanmak için yaprak sertifikasını kullanır. 

Aygıta yeni bir yaprak sertifikası yüklendikten sonra, bağlanmak için yeni bir sertifika kullandığından Artık IoT hub'ına bağlanamaz. IoT hub'ı yalnızca eski sertifikaya sahip aygıtı tanır. Aygıtın bağlantı denemesinin sonucu "yetkisiz" bir bağlantı hatası olacaktır. Bu hatayı gidermek için, aygıtın yeni yaprak sertifikasını hesaba katmak için aygıtın kayıt girişini güncelleştirmeniz gerekir. Daha sonra sağlama hizmeti, aygıt yeniden sağlandığında Gerektiğinde IoT Hub aygıt kayıt defteri bilgilerini güncelleştirebilir. 

Bu bağlantı hatasının olası bir özel durumu, sağlama hizmetinde aygıtınız için bir [Kayıt Grubu](concepts-service.md#enrollment-group) oluşturduğunuz bir senaryo olacaktır. Bu durumda, aygıtın güven sertifikası zincirindeki kök veya ara sertifikaları yuvarlamıyorsanız, yeni sertifika kayıt grubunda tanımlanan güven zincirinin bir parçasıysa aygıt tanınır. Bu senaryo bir güvenlik ihlaline tepki olarak ortaya çıkarsa, en azından gruptaki ihlal edildiği düşünülen belirli aygıt sertifikalarını kara listeye almalısınız. Daha fazla bilgi için, [bir kayıt grubundaki belirli aygıtları Kara Liste'ye](https://docs.microsoft.com/azure/iot-dps/how-to-revoke-device-access-portal#blacklist-specific-devices-in-an-enrollment-group)bakın.

Haddelenmiş sertifikalar için kayıt girişlerini **güncelleştirme, kayıtları Yönet** sayfasında gerçekleştirilir. Bu sayfaya erişmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın ve cihazınızın kayıt girişine sahip IoT Hub Aygıt Sağlama Hizmeti örneğine gidin.

2. **Kayıtları yönetme**'ye tıklayın.

    ![Kayıtları yönetme](./media/how-to-roll-certificates/manage-enrollments-portal.png)


Kayıt girişini güncelleştirmeyi nasıl kaldıracağınız, tek tek kayıtları mı yoksa grup kayıtlarını mı kullandığınıza bağlıdır. Ayrıca, önerilen yordamlar, bir güvenlik ihlali nedeniyle sertifikaları yuvarladığınıza veya sertifikanın süresinin dolmasına bağlı olarak değişir. Aşağıdaki bölümlerde bu güncelleştirmelerin nasıl işleyeceğiniaçıklanmıştır.


## <a name="individual-enrollments-and-security-breaches"></a>Bireysel kayıtlar ve güvenlik ihlalleri

Bir güvenlik ihlaline yanıt olarak sertifikaları kaydediyorsanız, geçerli sertifikayı hemen silen aşağıdaki yaklaşımı kullanmanız gerekir:

1. **Bireysel Kayıtlar'ı**tıklatın ve listedeki kayıt kimliği girişini tıklatın. 

2. Geçerli **sertifikayı sil** düğmesini tıklatın ve ardından, kayıt girişi için yüklenecek yeni sertifikayı seçmek için klasör simgesini tıklatın. Bittiğinde **Kaydet'i** tıklatın.

    Her ikisi de tehlikeye girerse, birincil ve ikincil sertifika için bu adımlar tamamlanmalıdır.

    ![Tek tek kayıtları yönetme](./media/how-to-roll-certificates/manage-individual-enrollments-portal.png)

3. İmtihal edilen sertifika sağlama hizmetinden kaldırıldıktan sonra, sertifika, ioT hub'ına aygıt bağlantısı yapmak için, aygıt kaydı olduğu sürece kullanılabilir. Bu iki yolu ele alabilirsiniz: 

    İlk yol, IoT hub'ınıza el ile gitmek ve gizliliği ihlal edilen sertifikayla ilişkili aygıt kaydını derhal kaldırmaktır. Daha sonra aygıt güncelleştirilmiş bir sertifikayla yeniden hüküm verdiğinde, yeni bir aygıt kaydı oluşturulur.     

    ![IoT hub cihaz kaydını kaldırma](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    İkinci yol, aygıtı aynı IoT hub'ına yeniden sağlamak için yeniden sağlama desteği kullanmaktır. Bu yaklaşım, IoT hub'ındaki aygıt kaydı sertifikasını değiştirmek için kullanılabilir. Daha fazla bilgi için [aygıtları yeniden sağlama konusuna](how-to-reprovision.md)bakın.

## <a name="individual-enrollments-and-certificate-expiration"></a>Bireysel kayıtlar ve sertifika süresi nin dolması

Sertifika son kullanma gün geçmişlerini işlemek için sertifikaları yuvarlanıyorsanız, sağlama girişiminde bulunmayı deneyen aygıtların kapalı kalma süresini azaltmak için ikincil sertifika yapılandırmasını aşağıdaki gibi kullanmanız gerekir.

İkincil sertifika da sona ermek üzere yken ve yuvarlanması gerektiğinde, birincil yapılandırmayı kullanmaya dönebilirsiniz. Birincil ve ikincil sertifikalar arasında bu şekilde döndürülme, sağlama girişiminde bulunan aygıtların kapalı kalma süresini azaltır.


1. **Bireysel Kayıtlar'ı**tıklatın ve listedeki kayıt kimliği girişini tıklatın. 

2. **İkincil Sertifika'yı** tıklatın ve ardından, kayıt girişi için yüklenecek yeni sertifikayı seçmek için klasör simgesini tıklatın. **Kaydet**'e tıklayın.

    ![İkincil sertifikayı kullanarak tek tek kayıtları yönetme](./media/how-to-roll-certificates/manage-individual-enrollments-secondary-portal.png)

3. Birincil sertifikanın süresi dolduğunda, **geçerli sertifikayı sil** düğmesini tıklatarak geri gelin ve birincil sertifikayı silin.

## <a name="enrollment-groups-and-security-breaches"></a>Kayıt grupları ve güvenlik ihlalleri

Bir grup kaydını bir güvenlik ihlaline yanıt olarak güncelleştirmek için, geçerli ca kökünü veya ara sertifikayı hemen silecek aşağıdaki yaklaşımlardan birini kullanmanız gerekir.

#### <a name="update-compromised-root-ca-certificates"></a>Tehlikeye atılmış kök CA sertifikalarını güncelleştirme

1. Cihaz Sağlama hizmeti örneğiniz için **Sertifikalar** sekmesini tıklatın.

2. Listedeki gizliliği ihlal edilen sertifikayı tıklatın ve sonra **Sil** düğmesini tıklatın. Sertifika adını girerek silmeyi onaylayın ve **Tamam'ı**tıklatın. Tüm tehlikeye atılmış sertifikalar için bu işlemi yineleyin.

    ![Kök CA sertifikasını silme](./media/how-to-roll-certificates/delete-root-cert.png)

3. Yeni kök CA sertifikaları eklemek ve doğrulamak için [Doğrulanmış CA sertifikalarını yapılandır'da](how-to-verify-certificates.md) özetlenen adımları izleyin.

4. Cihaz Sağlama hizmeti örneğiniz için **kayıtları Yönet** sekmesini ve **Kayıt Grupları** listesini tıklatın. Listedeki kayıt grubu adınızı tıklatın.

5. **CA Sertifikası'nı**tıklatın ve yeni kök CA sertifikanızı seçin. Daha sonra **Kaydet**'e tıklayın. 

    ![Yeni kök CA sertifikasını seçin](./media/how-to-roll-certificates/select-new-root-cert.png)

6. İmtihal edilen sertifika sağlama hizmetinden kaldırıldıktan sonra, sertifika, aygıt kayıtları orada bulunduğu sürece IoT hub'ına aygıt bağlantısı yapmak için kullanılabilir. Bu iki yolu ele alabilirsiniz: 

    İlk yol, IoT hub'ınıza el ile gitmek ve gizliliği ihlal edilen sertifikayla ilişkili aygıt kaydını derhal kaldırmaktır. Daha sonra, aygıtlarınız güncelleştirilmiş sertifikalarla yeniden sağlandığında, her biri için yeni bir aygıt kaydı oluşturulur.     

    ![IoT hub cihaz kaydını kaldırma](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    İkinci yol, aygıtlarınızı aynı IoT hub'ına yeniden sağlamak için yeniden sağlama desteği kullanmak olacaktır. Bu yaklaşım, IoT hub'ındaki aygıt kayıtları için sertifikaları değiştirmek için kullanılabilir. Daha fazla bilgi için [aygıtları yeniden sağlama konusuna](how-to-reprovision.md)bakın.



#### <a name="update-compromised-intermediate-certificates"></a>Tehlikeye atılmış ara sertifikaları güncelleştirme

1. **Kayıt Grupları'nı**tıklatın ve ardından listedeki grup adını tıklatın. 

2. **Ara Sertifika'yı**tıklatın ve **geçerli sertifikayı sil.** Kayıt grubu için yüklenecek yeni ara sertifikaya gitmek için klasör simgesini tıklatın. Bittiğinde **Kaydet'i** tıklatın. Her ikisi de tehlikeye girerse, bu adımlar hem birincil hem de ikincil sertifika için tamamlanmalıdır.

    Bu yeni ara sertifika, zaten sağlama hizmetine eklenmiş doğrulanmış bir kök CA sertifikası tarafından imzalanmalıdır. Daha fazla bilgi için [X.509 sertifikalarına](concepts-security.md#x509-certificates)bakın.

    ![Tek tek kayıtları yönetme](./media/how-to-roll-certificates/enrollment-group-delete-intermediate-cert.png)


3. İmtihal edilen sertifika sağlama hizmetinden kaldırıldıktan sonra, sertifika, aygıt kayıtları orada bulunduğu sürece IoT hub'ına aygıt bağlantısı yapmak için kullanılabilir. Bu iki yolu ele alabilirsiniz: 

    İlk yol, IoT hub'ınıza el ile gitmek ve gizliliği ihlal edilen sertifikayla ilişkili aygıt kaydını derhal kaldırmaktır. Daha sonra, aygıtlarınız güncelleştirilmiş sertifikalarla yeniden sağlandığında, her biri için yeni bir aygıt kaydı oluşturulur.     

    ![IoT hub cihaz kaydını kaldırma](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    İkinci yol, aygıtlarınızı aynı IoT hub'ına yeniden sağlamak için yeniden sağlama desteği kullanmak olacaktır. Bu yaklaşım, IoT hub'ındaki aygıt kayıtları için sertifikaları değiştirmek için kullanılabilir. Daha fazla bilgi için [aygıtları yeniden sağlama konusuna](how-to-reprovision.md)bakın.


## <a name="enrollment-groups-and-certificate-expiration"></a>Kayıt grupları ve sertifika süresi nin dolması

Sertifika son kullanma gün geçmişlerini işlemek için sertifikaları yuvarlanıyorsanız, sağlamaya çalışan aygıtlar için kapalı kalma süresi olmamasını sağlamak için ikincil sertifika yapılandırmasını aşağıdaki gibi kullanmanız gerekir.

İkincil sertifika da sona ermek üzere yken ve yuvarlanması gerektiğinde, birincil yapılandırmayı kullanmaya dönebilirsiniz. Birincil ve ikincil sertifikalar arasında bu şekilde döndürülme, sağlamaya çalışan aygıtlar için hiçbir kapalı kalma süresi sağlamaz. 

#### <a name="update-expiring-root-ca-certificates"></a>Süresi dolan kök CA sertifikalarını güncelleştirme

1. Yeni kök CA sertifikaları eklemek ve doğrulamak için [Doğrulanmış CA sertifikalarını yapılandır'da](how-to-verify-certificates.md) özetlenen adımları izleyin.

2. Cihaz Sağlama hizmeti örneğiniz için **kayıtları Yönet** sekmesini ve **Kayıt Grupları** listesini tıklatın. Listedeki kayıt grubu adınızı tıklatın.

3. **CA Sertifikası'nı**tıklatın ve **İkincil Sertifika** yapılandırması altında yeni kök CA sertifikanızı seçin. Daha sonra **Kaydet**'e tıklayın. 

    ![Yeni kök CA sertifikasını seçin](./media/how-to-roll-certificates/select-new-root-secondary-cert.png)

4. Birincil sertifikanın süresi dolduğunda, Aygıt Sağlama hizmeti örneğiniz için **Sertifikalar** sekmesini tıklatın. Listede süresi dolan sertifikayı tıklatın ve sonra **Sil** düğmesini tıklatın. Sertifika adını girerek silmeyi onaylayın ve **Tamam'ı**tıklatın.

    ![Kök CA sertifikasını silme](./media/how-to-roll-certificates/delete-root-cert.png)



#### <a name="update-expiring-intermediate-certificates"></a>Süresi dolan ara sertifikaları güncelleştirme


1. **Kayıt Grupları'nı**tıklatın ve listedeki grup adını tıklatın. 

2. **İkincil Sertifika'yı** tıklatın ve ardından, kayıt girişi için yüklenecek yeni sertifikayı seçmek için klasör simgesini tıklatın. **Kaydet**'e tıklayın.

    Bu yeni ara sertifika, zaten sağlama hizmetine eklenmiş doğrulanmış bir kök CA sertifikası tarafından imzalanmalıdır. Daha fazla bilgi için [X.509 sertifikalarına](concepts-security.md#x509-certificates)bakın.

   ![İkincil sertifikayı kullanarak tek tek kayıtları yönetme](./media/how-to-roll-certificates/manage-enrollment-group-secondary-portal.png)

3. Birincil sertifikanın süresi dolduğunda, **geçerli sertifikayı sil** düğmesini tıklatarak geri gelin ve birincil sertifikayı silin.


## <a name="reprovision-the-device"></a>Aygıtı yeniden sağlama

Sertifika hem aygıtta hem de Aygıt Sağlama Hizmeti'nde yuvarlandıktan sonra, aygıt Aygıt Sağlama hizmetiyle iletişime geçerek kendisini yeniden sağlayabilir. 

Aygıtları yeniden sağlamanın kolay bir yolu, aygıtın IoT hub'ına bağlanmaya çalışırken "yetkisiz" bir hata alması durumunda, sağlama hizmetiyle iletişim kurmasını programlamaktır.

Başka bir yol, hem eski hem de yeni sertifikaların kısa bir çakışma için geçerli olması ve IoT hub'ını kullanarak aygıtlara ioT Hub bağlantı bilgilerini güncelleştirmek için sağlama hizmeti aracılığıyla yeniden kaydolmalarını sağlayan bir komut göndermektir. Her aygıt komutları farklı şekilde işleyebildiğinden, komut çağrıldığında ne yapacağınızı bilmek için aygıtınızı programlamanız gerekir. IoT Hub üzerinden cihazınızı kontrol etmenin birkaç yolu vardır ve işlemi başlatmak için [doğrudan yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) veya [işler](../iot-hub/iot-hub-devguide-jobs.md) kullanmanızı öneririz.

Yeniden sağlama tamamlandıktan sonra, aygıtlar yeni sertifikalarını kullanarak IoT Hub'a bağlanabilecek.


## <a name="blacklist-certificates"></a>Kara liste sertifikaları

Bir güvenlik ihlaline yanıt olarak, bir aygıt sertifikasını kara listeye almanız gerekebilir. Aygıt sertifikasını kara listeye almak için hedef aygıtın/sertifikanın kayıt girişini devre dışı bırakın. Daha fazla bilgi için, [Yönetme'nin kaydını](how-to-revoke-device-access-portal.md) bırakma makalesinde kara liste aygıtları'na bakın.

Bir sertifika devre dışı bırakılmış bir kayıt girişinin parçası olarak dahil edildikten sonra, başka bir kayıt girişinin parçası olarak etkinleştirilmiş olsa bile, bu sertifikaları kullanarak bir IoT hub'ına kaydolmaya yönelik tüm girişimler başarısız olur.
 



## <a name="next-steps"></a>Sonraki adımlar

- Aygıt Sağlama Hizmeti'ndeki X.509 sertifikaları hakkında daha fazla bilgi edinmek için [Güvenlik](concepts-security.md) 
- Azure IoT Hub Aygıt Sağlama Hizmeti ile X.509 CA sertifikaları için sahip olma kanıtı nın nasıl yapılacağını öğrenmek için [sertifikaları nasıl doğrulayabilirsiniz](how-to-verify-certificates.md)
- Bir kayıt grubu oluşturmak için portalı nasıl kullanacağınızı öğrenmek için [bkz.](how-to-manage-enrollments.md)










