---
title: Öğretici Windows için C modülü geliştirmek - Azure IoT Edge | Microsoft Dokümanlar
description: Bu öğretici, C koduna sahip bir IoT Edge modülü oluşturmanızı ve bunu IoT Edge çalıştıran bir Windows aygıtına nasıl dağıtabileceğinizi gösterir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 05/28/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 09d039801107a44df4f3bf3745a1e074e6d708b8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76760973"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-windows-devices"></a>Öğretici: Windows aygıtları için c ioT edge modülü geliştirin

C kodunu geliştirmek ve Azure IoT Edge çalıştıran bir Windows aygıtına dağıtmak için Visual Studio'yı kullanın.

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * C SDK'yı temel alan bir IoT Edge modülü oluşturmak için Visual Studio'yu kullanın.
> * Bir Docker resmi oluşturmak ve kayıt defterinize yayınlamak için Visual Studio ve Docker'ı kullanın.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğretici, **Visual Studio 2019** kullanarak **C'de** bir modülün nasıl geliştirilmeye ve bir **Windows cihazına**nasıl dağıtılanın gösteriş olduğunu göstermektedir. Linux aygıtları için modüller geliştiriyorsanız, bunun yerine [Linux aygıtları için bir C IoT Edge modülü geliştirin.](tutorial-c-module.md)

C modülleri geliştirme ve Windows aygıtlarına dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın:

| C | Visual Studio Code | Visual Studio 2017/2019 |
| -- | ------------------ | ------------------ |
| **Windows AMD64** |  | ![Visual Studio'da WinAMD64 için C modülleri geliştirin](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce, Windows kapsayıcı geliştirme için geliştirme ortamınızı ayarlamak için önceki öğreticiden geçmiş olmalıydınız: [Windows aygıtları için IoT Edge modülleri geliştirin.](tutorial-develop-for-windows.md) Bu öğreticitamamladıktan sonra, yerde aşağıdaki ön koşullar almalısınız:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart.md)bir Windows aygıtı.
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Visual Studio 2019,](https://docs.microsoft.com/visualstudio/install/install-visual-studio) [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) uzantısı ile yapılandırıldı.
* [Docker Desktop,](https://docs.docker.com/docker-for-windows/install/) Windows kapsayıcılarını çalıştırmak için yapılandırıldı.
* Windows x64 için Azure IoT C SDK'yı vcpkg ile yükleyin:

   ```powershell
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   .\vcpkg install azure-iot-sdk-c:x64-windows
   .\vcpkg --triplet x64-windows integrate install
   ```

> [!TIP]
> Visual Studio 2017 kullanıyorsanız (sürüm 15.7 veya üzeri), lütfen Visual Studio pazar yerinden VS 2017 için [Azure IoT Edge Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) indirin ve yükleyin

## <a name="create-a-module-project"></a>Modül projesi oluşturma

Aşağıdaki adımlar, Visual Studio ve Azure IoT Edge Tools uzantısını kullanarak C SDK'yı temel alan bir IoT Edge modül projesi oluşturur. Bir proje şablonu oluşturulduktan sonra, modülün bildirilen özelliklerine göre iletileri filtreleyebilmeleri için yeni kod ekleyin.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir C çözüm şablonu oluşturun.

1. Visual Studio 2019'u başlatın ve **Yeni Proje Oluştur'u**seçin.

2. **IoT Edge'i** arayın ve **Azure IoT Edge (Windows amd64)** projesini seçin. **İleri**'ye tıklayın.

   ![Yeni bir Azure IoT Edge projesi oluşturun](./media/tutorial-c-module-windows/new-project.png)

3. **CTutorialApp**gibi açıklayıcı bir şey için proje ve çözüm rename . Projeyi oluşturmak için **Oluştur'u** tıklatın.

   ![Yeni bir Azure IoT Edge projesini yapılandırma](./media/tutorial-c-module-windows/configure-project.png)

4. Projenizi aşağıdaki değerlerle yapılandırın:

   | Alan | Değer |
   | ----- | ----- |
   | Şablon seçin | **C Modülünü**seçin. |
   | Modül proje adı | Modülünüze **CModule** adını verin. |
   | Docker görüntü deposu | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz modül proje ad değerinden önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son resim deposu kayıt defteri \<adı\>.azurecr.io/cmodule gibi görünüyor. |

   ![Projenizi hedef aygıt, modül türü ve konteyner kayıt defteri için yapılandırın](./media/tutorial-c-module-windows/add-application-and-module.png)

5. Projeyi oluşturmak için **Ekle'yi** seçin.

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Dağıtım bildirimi, kapsayıcı kayıt defterinizin kimlik bilgilerini IoT Edge çalışma zamanıile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. Azure konteyner kayıt defterinizin **Erişim tuşları** bölümündeki kimlik bilgilerini kullanın.

1. Visual Studio çözüm gezgininde **deployment.template.json** dosyasını açın.

2. İstenilen $edgeAgent özelliklerinde **kayıt kimlik bilgileri** özelliğini bulun. Projeyi oluştururken sağladığınız bilgilerden kayıt defteri adresinizin otomatik olarak doldurulması gerekir. Kullanıcı adı ve parola alanları değişken adlarını içermelidir. Örnek:

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "$CONTAINER_REGISTRY_USERNAME_<registry name>",
       "password": "$CONTAINER_REGISTRY_PASSWORD_<registry name>",
       "address": "<registry name>.azurecr.io"
     }
   }
   ```

3. Modül çözümünde **.env** dosyasını açın. (Çözüm Gezgini'nde varsayılan olarak gizlenir, bu nedenle görüntülemek için **Tüm Dosyaları Göster** düğmesini seçmeniz gerekebilir.) .env dosyası, deployment.template.json dosyasında gördüğünüz kullanıcı adı ve parola değişkenlerini içermelidir.

4. Azure kapsayıcı kayıt defterinizden **Kullanıcı Adı** ve **Parola** değerlerini ekleyin.

5. Değişikliklerinizi .env dosyasına kaydedin.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Varsayılan modül kodu, giriş kuyruğundaki iletileri alır ve bunları bir çıkış kuyruğu boyunca ileterek geçirir. Modülün iletileri IoT Hub'ına göndermeden önce kenarda işleyebilmeleri için bazı ek kodlar ekleyelim. Modülü, her iletideki sıcaklık verilerini analiz edecek şekilde güncelleştirin ve iletiyi yalnızca sıcaklık belirli bir eşiği aşarsa IoT Hub'a gönderir.

1. Bu senaryoda sensörden alınan veriler JSON biçimindedir. JSON biçimindeki iletileri filtreleme amacıyla C için bir JSON kitaplığını içeri aktarın. Bu öğreticide Parson kullanılmıştır.

   1. [Parson GitHub deposunu indirin.](https://github.com/kgabis/parson) **Parson.c** ve **parson.h** dosyalarını **CModule** projesine kopyalayın.

   2. Visual Studio'da CModule proje klasöründen **CMakeLists.txt** dosyasını açın. Dosyanın en üstünde Parson dosyalarını **my_parson** adlı bir kitaplık olarak içeri aktarın.

      ```txt
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. CMakeLists.txt dosyasının `my_parson` **target_link_libraries** bölümündeki kitaplıklar listesine ekleyin.

   4. **CMakeLists.txt** dosyasını kaydedin.

   5. **CModule** > **main.c'yi**açın . Ekleme ifadeleri listesinin en altında, JSON desteği `parson.h` için eklemek için yeni bir tane ekleyin:

      ```c
      #include "parson.h"
      ```

2. **Main.c** dosyasında, iletilerin yanında `temperatureThreshold` genel bir değişken ekleyinReceivedByInput1Queue değişkeni. Bu değişken, IoT Hub'a veri gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```c
    static double temperatureThreshold = 25;
    ```

3. Main.c'deki `CreateMessageInstance` işlevi bulun. İç if else deyimini birkaç işlev satırı ekleyen aşağıdaki kodla değiştirin:

   ```c
   if ((messageInstance->messageHandle = IoTHubMessage_Clone(message)) == NULL)
   {
       free(messageInstance);
       messageInstance = NULL;
   }
   else
   {
       messageInstance->messageTrackingId = messagesReceivedByInput1Queue;
       MAP_HANDLE propMap = IoTHubMessage_Properties(messageInstance->messageHandle);
       if (Map_AddOrUpdate(propMap, "MessageType", "Alert") != MAP_OK)
       {
          printf("ERROR: Map_AddOrUpdate Failed!\r\n");
       }
   }
   ```

   Else deyimindeki yeni kod satırları, iletiyi uyarı olarak etiketleyen iletiye yeni bir özellik ekler. Bu kod tüm iletileri uyarı olarak etiketler, çünkü yalnızca yüksek sıcaklıkları bildirmeleri durumunda IoT Hub'a ileti gönderen işlevsellik ekleriz.

4. İşlevi `InputQueue1Callback` bulun ve tüm işlevi aşağıdaki kodla değiştirin. Bu işlev gerçek bir mesajlaşma filtresi uygular. İleti alındığı zaman, bildirilen sıcaklığın eşiği aşıp aşmadığını denetler. Evet ise, iletiyi çıkış kuyruğuna iletmektedir. Değilse, o zaman iletiyi yoksayar.

    ```c
    static unsigned char *bytearray_to_str(const unsigned char *buffer, size_t len)
    {
        unsigned char *ret = (unsigned char *)malloc(len + 1);
        memcpy(ret, buffer, len);
        ret[len] = '\0';
        return ret;
    }

    static IOTHUBMESSAGE_DISPOSITION_RESULT InputQueue1Callback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
        IOTHUBMESSAGE_DISPOSITION_RESULT result;
        IOTHUB_CLIENT_RESULT clientResult;
        IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle = (IOTHUB_MODULE_CLIENT_LL_HANDLE)userContextCallback;

        unsigned const char* messageBody;
        size_t contentSize;

        if (IoTHubMessage_GetByteArray(message, &messageBody, &contentSize) == IOTHUB_MESSAGE_OK)
        {
            messageBody = bytearray_to_str(messageBody, contentSize);
        } else
        {
            messageBody = "<null>";
        }

        printf("Received Message [%zu]\r\n Data: [%s]\r\n",
                messagesReceivedByInput1Queue, messageBody);

        // Check if the message reports temperatures higher than the threshold
        JSON_Value *root_value = json_parse_string(messageBody);
        JSON_Object *root_object = json_value_get_object(root_value);
        double temperature;

        // If temperature exceeds threshold, send to output1
        if (json_object_dotget_value(root_object, "machine.temperature") != NULL && (temperature = json_object_dotget_number(root_object, "machine.temperature")) > temperatureThreshold)
        {
            printf("Machine temperature %f exceeds threshold %f\r\n", temperature, temperatureThreshold);
            // This message should be sent to next stop in the pipeline, namely "output1".  What happens at "outpu1" is determined
            // by the configuration of the Edge routing table setup.
            MESSAGE_INSTANCE *messageInstance = CreateMessageInstance(message);
            if (NULL == messageInstance)
            {
                result = IOTHUBMESSAGE_ABANDONED;
            }
            else
            {
                printf("Sending message (%zu) to the next stage in pipeline\n", messagesReceivedByInput1Queue);

                clientResult = IoTHubModuleClient_LL_SendEventToOutputAsync(iotHubModuleClientHandle, messageInstance->messageHandle, "output1", SendConfirmationCallback, (void *)messageInstance);
                if (clientResult != IOTHUB_CLIENT_OK)
                {
                    IoTHubMessage_Destroy(messageInstance->messageHandle);
                    free(messageInstance);
                    printf("IoTHubModuleClient_LL_SendEventToOutputAsync failed on sending msg#=%zu, err=%d\n", messagesReceivedByInput1Queue, clientResult);
                    result = IOTHUBMESSAGE_ABANDONED;
                }
                else
                {
                    result = IOTHUBMESSAGE_ACCEPTED;
                }
            }
        }
        // If message does not exceed threshold, do not forward
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

5. `moduleTwinCallback` işlevi ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```c
    static void moduleTwinCallback(DEVICE_TWIN_UPDATE_STATE update_state, const unsigned char* payLoad, size_t size, void* userContextCallback)
    {
        printf("\r\nTwin callback called with (state=%s, size=%zu):\r\n%s\r\n",
            MU_ENUM_TO_STRING(DEVICE_TWIN_UPDATE_STATE, update_state), size, payLoad);
        JSON_Value *root_value = json_parse_string(payLoad);
        JSON_Object *root_object = json_value_get_object(root_value);
        if (json_object_dotget_value(root_object, "desired.TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_dotget_number(root_object, "desired.TemperatureThreshold");
        }
        if (json_object_get_value(root_object, "TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_get_number(root_object, "TemperatureThreshold");
        }
    }
    ```

6. `SetupCallbacksForModule` İşlevi bulun. Modül ikizinin güncelleştirilip güncelleştirilmediğini kontrol etmek için başka **bir** deyim ekleyen aşağıdaki kodla işlevi değiştirin.

   ```c
   static int SetupCallbacksForModule(IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle)
   {
       int ret;

       if (IoTHubModuleClient_LL_SetInputMessageCallback(iotHubModuleClientHandle, "input1", InputQueue1Callback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetInputMessageCallback(\"input1\")..........FAILED!\r\n");
           ret = MU_FAILURE;
       }
       else if (IoTHubModuleClient_LL_SetModuleTwinCallback(iotHubModuleClientHandle, moduleTwinCallback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetModuleTwinCallback(default)..........FAILED!\r\n");
           ret = MU_FAILURE;
       }
       else
       {
           ret = 0;
       }

       return ret;
   }
   ```

7. main.c dosyasını kaydedin.

8. **deployment.template.json** dosyasını açın.

9. Dağıtım bildirimine CModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin:

   ```json
   "CModule": {
       "properties.desired":{
           "TemperatureThreshold":25
       }
   }
   ```

   ![Dağıtım şablonuna CModule ikizi ekleme](./media/tutorial-c-module-windows/module-twin.png)

10. **deployment.template.json** dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Modülünüzü oluşturun ve itin

Önceki bölümde, bildirilen makine sıcaklığının kabul edilebilir eşiğin altında olduğu iletileri filtrelemek için bir IoT Edge çözümü oluşturdunuz ve **CModule'e** kod eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. Geliştirme makinenizde Docker'da oturum açabilmek için aşağıdaki komutu kullanın. Azure konteyner kayıt defterinizden kullanıcı adı, parola ve oturum açma sunucusuyla oturum açın. Bu değerleri Azure portalındaki kayıt defterinizin **Erişim tuşları** bölümünden alabilirsiniz.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   'nin kullanılmasını öneren bir güvenlik `--password-stdin`uyarısı alabilirsiniz. Bu en iyi uygulama üretim senaryoları için önerilir, ancak bu öğretici kapsamı dışındadır. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvurusuna bakın.

2. Visual Studio çözüm gezgininde, oluşturmak istediğiniz proje adını sağ tıklatın. Varsayılan adı **AzureIotEdgeApp1** ve bir Windows modülü oluşturuyorsanız, uzantısı **Windows.Amd64**olmalıdır.

3. **IoT Edge Modülleri Oluştur ve İtme'yi**seçin.

   Yapı ve itme komutu üç işlem başlatır. İlk olarak, **config** adlı çözümde, dağıtım şablonundaki bilgilerin ve diğer çözüm dosyalarının dışında tam dağıtım bildirimini tutan yeni bir klasör oluşturur. İkinci olarak, `docker build` hedef mimariniz için uygun dockerfiledayalı kapsayıcı görüntü oluşturmak için çalışır. Daha sonra, `docker push` görüntü deposunu konteyner kayıt defterinize itmek için çalışır.

## <a name="deploy-modules-to-device"></a>Modülleri aygıta dağıtma

Modül projesini IoT Edge aygıtınıza dağıtmak için Visual Studio bulut gezgini ve Azure IoT Edge Araçları uzantısını kullanın. Zaten senaryonuz için hazırlanmış bir dağıtım bildiriminiz var, config klasöründe **deployment.json** dosyası. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IoT Edge cihazınızın çalışır durumda olduğundan emin olun.

1. Visual Studio bulut gezgininde, IoT aygıtları listenizi görmek için kaynakları genişletin.

2. Dağıtımı almak istediğiniz IoT Edge aygıtının adını sağ tıklatın.

3. **Dağıtım Oluştur'u**seçin.

4. Dosya gezgini'nde, çözümünüzün config klasöründe **deployment.windows-amd64** dosyasını seçin.

5. Cihazınızın altında listelenen dağıtılmış modülleri görmek için bulut gezginini yenileyin.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

İletileri IoT Hub'ınıza ulaştıklarında görüntülemek için IoT Edge Tools uzantısını kullanabilirsiniz.

1. Visual Studio bulut gezgininde, IoT Edge cihazınızın adını seçin.

2. **Eylemler** listesinde, **Yerleşik Olay Bitiş Noktası'nı İzlemeye Başla'yı**seçin.

3. IoT Hub'ınıza gelen iletileri görüntüleyin. IoT Edge aygıtının yeni dağıtımını alması ve tüm modülleri başlatması nedeniyle iletilerin gelmesi biraz zaman alabilir. Daha sonra, CModule kodunda yaptığımız değişiklikler, mesaj göndermeden önce makine sıcaklığının 25 dereceye ulaşmasını bekler. Ayrıca, bu sıcaklık eşiğine ulaşan iletilere ileti türü **Uyarısı** da ekler.

   ![IoT Hub'a gelen iletileri görüntüleme](./media/tutorial-c-module-windows/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>Modül ikizini edin

Sıcaklık eşiğini 25 derece ye ayarlamak için CModule modül ikizini kullandık. Modül kodunu güncelleştirmek zorunda kalmadan işlevselliği değiştirmek için modül ikizi kullanabilirsiniz.

1. Visual Studio'da **deployment.windows-amd64.json** dosyasını açın. (Deployment.template dosyası değil. Çözüm gezginindeki config dosyasında dağıtım bildirimini görmüyorsanız, gezgin araç çubuğundaki **tüm dosyaları göster** simgesini seçin.)

2. CModule ikizini bulun ve **sıcaklık** değerini eşik parametresini yeni bir sıcaklığa değiştirin 5 derece ile en son bildirilen sıcaklıktan 10 derece daha yüksek.

3. **Deployment.windows-amd64.json** dosyasını kaydedin.

4. Güncelleştirilmiş dağıtım bildirimini cihazınıza uygulamak için dağıtım adımlarını yeniden izleyin.

5. Gelen aygıttan buluta iletileri izleyin. İletilerin yeni sıcaklık eşiğine ulaşılına kadar durduğunu görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Aksi takdirde, ücretlendirmeleri önlemek için bu makalede kullandığınız yerel yapılandırmaları ve Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [kendi IoT Edge modüllerinizi geliştirme](module-development.md) veya Visual [Studio ile modülgeliştirme](how-to-visual-studio-develop-module.md)hakkında daha fazla bilgi edinebilirsiniz. Simüle sıcaklık modülü de dahil olmak üzere IoT Edge modülleri örnekleri için [IoT Edge modül örneklerine](https://github.com/Azure/iotedge/tree/master/edge-modules) ve [IoT C SDK örneklerine](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples)bakın.

Azure IoT Edge'in, verileri işlemek ve en uçta analiz etmek için Azure bulut hizmetlerini dağıtmanıza nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Fonksiyonlar](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Makine Öğrenme](tutorial-deploy-machine-learning.md)
> [Özel Vizyon Hizmeti](tutorial-deploy-custom-vision.md)
