---
title: Azure Percept DK 'yi ayarlama
description: Geliştirme setinizi Azure 'a bağlama ve ilk AI modelinizi dağıtma
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/15/2021
ms.custom: template-quickstart
ms.openlocfilehash: 3540204d66bb589c567514f92a9a8acb2159e343
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101665646"
---
# <a name="set-up-your-azure-percept-dk-and-deploy-your-first-ai-model"></a>Azure Percept DK 'yi ayarlama ve ilk AI modelinizi dağıtma

Cihazınızı Azure 'a bağlamak ve ilk AI modelinizi dağıtmak için Azure Percept DK kurulum deneyimini kullanarak Azure Percept DK ve Azure Percept Studio ile çalışmaya başlayın. Azure hesabınızın Azure Percept Studio ile uyumlu olduğunu doğruladıktan sonra, Azure Percept DK 'nin Edge AI kanıtı kavramlarını oluşturmak üzere yapılandırıldığından emin olmak için kurulum deneyimini tamamlayabilirsiniz.

Bu hızlı başlangıç herhangi bir sorunla karşılaşırsanız, olası çözümler için [sorun giderme](./troubleshoot-dev-kit.md) kılavuzuna bakın.

## <a name="prerequisites"></a>Önkoşullar

- Azure Percept DK.
- Wi-Fi özelliğine ve bir Web tarayıcısına sahip bir Windows, Linux veya OS X tabanlı ana bilgisayar.
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Azure hesabının abonelikte "sahip" veya "katkıda bulunan" rolü olmalıdır. Azure rol tanımları hakkında daha fazla bilgi edinin

### <a name="prerequisite-check"></a>Önkoşul denetimi

Azure hesabınızın abonelikte bir "sahip" veya "katkıda bulunan" olup olmadığını doğrulamak için aşağıdaki adımları izleyin.

1. Azure portal gidin ve Azure Percept Studio ile kullanmayı düşündüğünüz Azure hesabıyla oturum açın. 

    > [!NOTE]
    > Birden çok Azure hesabınız varsa, tarayıcınız kimlik bilgilerini başka bir hesaptan önbelleğe alabilir. Doğru hesapta oturum açtığınızdan emin olmak hakkında daha fazla bilgi için bkz. sorun giderme kılavuzu.

1. Ekranınızın sol üst köşesinden ana menüyü genişletin ve "abonelikler" e tıklayın veya giriş sayfasındaki simgeler menüsünden "abonelikler" i seçin. 
    <!---
    :::image type="content" source="./media/quickstart-percept-dk-setup/prereq-01-subscription.png" alt-text="supscription icon in Azure portal.":::
    --->
1. Listeden aboneliğinizi seçin. Aboneliğinizi listede görmüyorsanız doğru Azure hesabıyla oturum açtığınızdan emin olun. 
    <!---
    :::image type="content" source="./media/quickstart-percept-dk-setup/prereq-02-sub-list.png" alt-text="supscription list in Azure portal.":::
    --->
Yeni bir abonelik oluşturmak isterseniz, [aşağıdaki adımları](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)izleyin.

1. Abonelik menüsünden "erişim denetimi (ıAM)" öğesini seçin
1. "Erişimimi görüntüle" düğmesine tıklayın
1. Rolü denetleyin
    - "Okuyucu" rolünü gösteriyorsa veya rolleri görme izniniz olmadığını belirten bir ileti alırsanız, hesap rolünüzü yükseltilmiş hale getirmek için kuruluşunuzdaki gerekli süreci izlemeniz gerekir.
    - Rolü "Owner" veya "katkıda bulunan" olarak gösteriyorsa, hesabınız Azure Percept Studio ile çalışır. 

## <a name="launch-the-azure-percept-dk-setup-experience"></a>Azure Percept DK kurulum deneyimini başlatın

<!---
> [!NOTE]
> Connecting over ethernet? See [this how-to guide](<link needed>) for detailed instructions.
--->
1. Ana bilgisayarınızı doğrudan Dev Kit 'in Wi-Fi erişim noktasına bağlayın. Bu, diğer herhangi bir Wi-Fi ağına bağlanmada olduğu gibi yapılır.
    - **ağ adı**: SCZ-xxxx (burada "xxxx", dev kıt 'in Mac ağ adresinin son dört rakamı)
    - **parola**: Dev Kit Ile gelen hoş geldiniz kartında bulunabilir

    > [!WARNING]
    > Azure Percept DK Wi-Fi erişim noktasına bağlıyken, ana bilgisayarınız Internet bağlantısını geçici olarak kaybedecektir. Etkin video konferansı çağrıları, web akışı veya diğer ağ tabanlı deneyimler, Azure Percept DK Kurulum deneyiminin 3. adımı tamamlanana kadar kesintiye uğracaktır.

1. Geliştirme Seti 'nin Wi-Fi erişim noktasına bağlandıktan sonra, konak cihaz Azure Percept DK kurulum deneyimini yeni bir tarayıcı penceresinde otomatik olarak başlatır. Otomatik olarak açılmadığından, bir tarayıcı penceresi açıp ' ye giderek el ile başlatabilirsiniz [http://10.1.1.1](http://10.1.1.1) . 

1. Azure Percept kurulum deneyimini başlatdığınıza göre, kurulum deneyimine devam etmeye hazırsınız. 

    > [!NOTE]
    > Azure Percept DK kurulum deneyimi Web hizmeti, 30 dakikalık bir süre sonra ve Kurulum deneyiminin tamamlanmasından sonra kapatılacak. Bu durumda, Dev Kit Wi-Fi erişim noktasıyla bağlantı sorunlarını önlemek için Dev Kit 'i yeniden başlatmanız önerilir.

## <a name="complete-the-azure-percept-dk-setup-experience"></a>Azure Percept DK kurulum deneyimini doldurun

1. Kullanmaya başlayın-hoş geldiniz ekranının **Ileri** ' ye tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-01-welcome.png" alt-text="Hoş geldiniz sayfası.":::

1. Wi-Fi ' y e Bağlan-ağ bağlantısı sayfasında, devkit 'nizi Wi-Fi ağınıza bağlamak için **Yeni bir WiFi ağına bağlan ' a** tıklayın.

    Bu Dev Kit 'i daha önce Wi-Fi ağınıza bağladıysanız veya Wi-Fi ağınız aracılığıyla Azure Percept DK kurulum deneyimine zaten bağlandıysanız, **Atla** bağlantısına tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-02-connect-to-wi-fi.png" alt-text="Wi-Fi ' a bağlanın.":::

1. Kullanılabilir bağlantılardan Wi-Fi ağınızı seçin ve bağlayın. (Yerel Wi-Fi parolanızın olması gerekir)

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-03-select-wi-fi.png" alt-text="Wi-Fi ağını seçin."::: 

1. IP adresinizi kopyalama-devkit istediğiniz ağa başarıyla bağlandıktan sonra sayfada görünen **IPv4 adresini** yazın. Bu **IP adresine bu hızlı başlangıç kılavuzunda daha sonra ihtiyacınız olacak**. 

    > [!NOTE]
    > IP adresi, her bir cihaz önyüklemesi ile değişebilir.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-04-success-wi-fi.png" alt-text="IP adresini kopyalayın.":::

1. Lisans sözleşmesini gözden geçirin ve kabul edin-lisans sözleşmesini okuyun, **Lisans sözleşmesini okudum ve kabul** ediyorum (Sözleşmenin en altına gidip gelmelidir) seçeneğini belirleyin ve **İleri**' ye tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-05-eula.png" alt-text="EULA 'Yı kabul edin.":::

1. SSH oturum açma hesabınızı oluşturun-devkit 'e uzaktan erişim için SSH ayarlayın. SSH Kullanıcı adı ve parola oluşturun. 

    > [!NOTE]
    > SSH (Secure Shell), konak cihazla geliştirme seti arasında güvenli iletişim için kullanılan bir ağ protokolüdür. SSH hakkında daha fazla bilgi için [Bu makaleye](https://en.wikipedia.org/wiki/SSH_(Secure_Shell))bakın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-06-ssh-login.png" alt-text="SSH hesabı oluşturun."::: 

1. Dev Kit bağlantı işlemini başlatın-bir sonraki ekranda, Dev Kit 'i Azure IoT Hub bağlama işlemini başlatmak için **Yeni bir cihazla Bağlan ' a** tıklayın. 

    <!---
    Connecting with an existing IoT Edge device connection string? See this [how-to guide](<link needed>) for reference.
    --->
    :::image type="content" source="./media/quickstart-percept-dk-setup/main-07-connect-device.png" alt-text="Azure 'a bağlanın."::: 

1. Cihaz kodunu kopyala-cihaz kodunuzu kopyalamak için **Kopyala** bağlantısına tıklayın. Ardından **Azure 'Da oturum aç**' a tıklayın. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-08-copy-code.png" alt-text="Cihaz kodunu kopyalayın."::: 

1. Cihaz kodunu yapıştırma-yeni bir tarayıcı sekmesi, kopyalanmış Cihaz kodunu soran bir pencere ile açılır. Kodu pencereye yapıştırın ve **İleri**' ye tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-09-enter-code.png" alt-text="Cihaz kodunu girin."::: 

1. Azure 'da oturum aç-sonraki pencere, hızlı başlangıç başlangıcında doğrulamış olduğunuz Azure hesabıyla oturum açmanızı gerektirir. Bu oturum açma kimlik bilgilerini girin ve **İleri**' ye tıklayın. 

    > [!NOTE]
    > Tarayıcınız diğer kimlik bilgilerini otomatik olarak önbelleğe alabilir. Doğru hesapla oturum açmak için çift kontrol edin.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-10-azure-sign-in.png" alt-text="Azure 'da oturum açın.":::
 
1. Kurulum deneyimi tarayıcı sekmesini kapatmayın Bu adımda oturum açtıktan sonra oturum açmış olduğunuz bir ekran görüntülenir. Pencereyi birçok kapatmayı söylemekle birlikte, **herhangi bir Windows 'u kapatmadığımızda**. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-11-sign-in-success.png" alt-text="Oturum açma başarılı."::: 

1. Kurulum deneyimini barındıran tarayıcı sekmesine dönün.
1. IoT Hub seçeneğinizi belirleyin
    - Zaten bir IoT Hub varsa ve bu sayfada listeleniyorsa, bunu seçip **17. adıma geçebilirsiniz**.
    - Bir IoT Hub yoksa veya yeni bir tane oluşturmak istiyorsanız, listenin en altına gidin ve **yeni Azure IoT Hub oluştur ' a** tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-13-iot-hub-select.png" alt-text="Bir IoT Hub seçin."::: 

1. Yeni IoT Hub oluşturun – Bu sayfadaki tüm alanları doldurun. 
    - Azure Percept Studio ile kullanacağınız Azure aboneliğini seçin
    - Var olan bir kaynak grubunu seçin. Bunlardan biri yoksa, "Yeni oluştur" a tıklayın ve istemleri izleyin. 
    - Azure bölgesini seçin. 
    - Yeni IoT Hub bir ad verin
    - Fiyatlandırma katmanını seçin (genellikle abonelikle eşleşir)

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-14-create-iot-hub.png" alt-text="Yeni bir IoT Hub oluşturun."::: 

1. IoT Hub dağıtılması için bekleyin, bu işlem birkaç dakika sürebilir

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-15-iot-hub-deploy.png" alt-text="IoT Hub dağıtın."::: 

1. Geliştirme setinizi kaydetme-dağıtım tamamlandığında, **Kaydet** düğmesine tıklayın

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-16-iot-hub-success.png" alt-text="IoT Hub başarıyla dağıtıldı."::: 

1. Geliştirme setini adlandırın-Dev Kit için bir cihaz adı girin ve **İleri**' ye tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-17-device-name.png" alt-text="Cihazı adlandırın."::: 

1. Varsayılan AI modellerinin indirilmesini bekleyin – bu işlem birkaç dakika sürer

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-18-finalize.png" alt-text="Kurulum sonlandırılıyor."::: 

1. Bkz. Vision AI in Action-devkit Azure IoT Hub başarıyla bağlandı ve varsayılan Vision AI nesne algılama modelini aldı. Azure Percept Vision kamera artık buluta bir bağlantı olmadan nesne algılamayı de daha fazla sınırlama yapabilir. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-2-preview-video-output.png" alt-text="Video çıkışının Önizlemesi ' ne tıklayın."::: 
       
    > [!NOTE]
    > Ana bilgisayarınız için ekleme işlemi ve cihaz WiFi erişimi bağlantısı bu noktada kapanıyor, ancak Dev Kit, internet 'e bağlı kalır.   Ekleme deneyimini bir geliştirme seti yeniden başlatmasıyla yeniden başlatarak, ekleme işlemini geri dönüp cihazı aynı veya farklı bir Azure aboneliğiyle ilişkili farklı bir ıOT hub 'ına yeniden bağlamanıza olanak sağlar.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-0-warning.png" alt-text="Kurulum deneyimi bağlantı kesme uyarısı."::: 

1. Azure Portal 'a devam edin – Azure Percept Studio 'da özel AI modellerinizi oluşturmaya başlamak için kurulum deneyimi penceresine geri dönün ve **Azure Portal** düğmesine tıklayın.

    > [!NOTE]
    > Ana bilgisayarınızın artık WiFi ayarlarınızda Dev Kit erişim noktasına bağlı olmadığını ve artık yerel WiFi 'nize yeniden bağlandığından emin olun.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-20-azure-portal-continue.png" alt-text="Azure Percept Studio 'ya gidin."::: 

## <a name="view-your-device-in-the-azure-percept-studio-and-deploy-common-prebuilt-sample-apps"></a>Cihazınızı Azure Percept Studio 'da görüntüleyin ve ortak önceden oluşturulmuş örnek uygulamalar dağıtın

1. Azure Percept genel bakış sayfasından cihaz listenizi görüntüleyin. Azure Percept genel bakış sayfası, hem başlangıç hem de gelişmiş AI Edge modeli ve çözüm geliştirme için birçok farklı iş akışının erişimine yönelik başlatma noktancıdır

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-01-get-device-list.png" alt-text="Cihaz listenizi görüntüleyin.":::
    
1. Yeni oluşturduğunuz cihazın bağlı olduğunu doğrulayın ve üzerine tıklayın.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-02-select-device.png" alt-text="Cihazınızı seçin.":::

1. Geliştirme Seti kameranızın neleri gördüğünü görmek için cihaz akışınızı görüntüleyin. Varsayılan bir nesne algılama modeli, kutudan dağıtılır ve bir dizi ortak nesneyi algılar.

    > [!NOTE]
    > Bunu yeni bir cihazda ilk kez yaptığınızda sağ üst köşede yeni bir modülün dağıtıldığını belirten bir bildirim alırsınız.  Bu işlem yapıldıktan sonra, kamerayı kamera video akışı ile başlatabilirsiniz. 
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-1-start-video-stream.png" alt-text="Video akışınızı görüntüleyin.":::
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-2-object-detection.png" alt-text="Bkz. nesne algılama.":::

1. Önceden oluşturulmuş örnek AI modellerini keşfet.   Azure Precept Studio, tek bir tıklama ile dağıtılabilecek ortak bir dizi önceden oluşturulmuş örnek içerir.   Bunları görüntülemek ve dağıtmak için "örnek model dağıt" ı seçin.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-04-explore-prebuilt.png" alt-text="Önceden oluşturulmuş modelleri keşfet.":::
    
1. Bağlı cihazınıza önceden oluşturulmuş yeni bir örnek dağıtın. Kitaplıktan bir örnek seçin ve "cihaza dağıt" a tıklayın

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-05-1-select-prebuilt.png" alt-text="Önceden oluşturulmuş ' ı seçin.":::
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-05-2-select-journey.png" alt-text="Bkz. nesne algılama işlemi sürüyor.":::

## <a name="next-steps"></a>Sonraki adımlar

[Önceden oluşturulmuş konuşma modellerini](./tutorial-no-code-speech.md)denemek için, benzer bir akışı izleyebilirsiniz.