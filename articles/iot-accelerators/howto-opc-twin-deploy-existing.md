---
title: OPC İkiz modülü varolan bir Azure projesine nasıl dağıtılır | Microsoft Dokümanlar
description: Bu makalede, OPC Twin'in varolan bir projeye nasıl dağıtılancayakadar dır. Dağıtım hatalarını nasıl giderebileceğinizi de öğrenebilirsiniz.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: b971ec13c71ccfd7d28ae6987593d09201b9b764
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73824113"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>OPC Twin'i varolan bir projeye dağıtma

OPC Twin modülü IoT Edge üzerinde çalışır ve OPC Twin ve Registry hizmetlerine çeşitli kenar hizmetleri sağlar.

OPC Twin microservice, opc Twin IoT Edge modülü aracılığıyla fabrika operatörleri ve OPC UA sunucu cihazları arasındaki iletişimi kolaylaştırır. Mikro hizmet, REST API'si aracılığıyla OPC UA hizmetlerini (Gözat, Oku, Yazma ve Yürüt) ortaya çıkarır. 

OPC UA cihaz kayıt microservice kayıtlı OPC UA uygulamalarına ve uç noktalarına erişim sağlar. Operatörler ve yöneticiler yeni OPC UA uygulamalarını kaydedebilir ve kaydedebilir ve son noktaları da dahil olmak üzere varolan uygulamalara göz atabilir. Kayıt defteri hizmeti, uygulama ve uç nokta yönetimine ek olarak, kayıtlı OPC Twin IoT Edge modüllerini de kataloglar. Hizmet API'si, sunucu bulma (tarama hizmetleri) başlatma veya durdurma veya OPC Twin microservice kullanılarak erişilebilen yeni uç nokta ikizlerini etkinleştirme gibi kenar modülü işlevselliğini kontrol etmenizi sağlar.

Modülün temeli Denetçi kimliğidir. Denetçi, ilgili OPC UA kayıt defteri API'si kullanılarak etkinleştirilen OPC UA sunucu uç noktalarına karşılık gelen uç nokta ikizini yönetir. Bu uç nokta ikizleri, bulutta çalışan OPC Twin microservice'den alınan OPC UA microSERVICE'i, yönetilen bitiş noktasına özel güvenli bir kanal üzerinden gönderilen OPC UA ikili iletilerine çevirir. Denetçi ayrıca, bu olayların OPC UA kayıt defterinde güncelleştirmelere neden olduğu işleme için OPC UA aygıtına aygıt bulma olayları gönderen keşif hizmetleri de sağlar.  Bu makalede, OPC Twin modüllerini varolan bir projeye nasıl dağıtabileceğiniz gösterilmektedir.

> [!NOTE]
> Dağıtım ayrıntıları ve yönergeleri hakkında daha fazla bilgi için GitHub [deposuna](https://github.com/Azure/azure-iiot-opc-twin-module)bakın.

## <a name="prerequisites"></a>Ön koşullar

PowerShell ve [AzureRM PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) uzantılarının yüklü olduğundan emin olun. Bunu zaten yapmadıysanız, bu GitHub deposunu klonla. PowerShell'de aşağıdaki komutları çalıştırın:

```powershell
git clone --recursive https://github.com/Azure/azure-iiot-components.git
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Azure'a endüstriyel IoT hizmetlerini dağıtma

1. PowerShell oturumunuzda çalıştırın:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy.cmd
    ```

2. Dağıtımın kaynak grubuna bir ad ve web sitesine bir ad atamak için istemleri izleyin.   Komut dosyası, azure aboneliğinizdeki kaynak grubuna mikro hizmetleri ve Azure platform bağımlılıklarını dağıtır.  Komut dosyası, OAUTH tabanlı kimlik doğrulamasını desteklemek için Azure Etkin Dizininiz (AAD) kiracınızda bir Uygulama da kaydeder.  Dağıtım birkaç dakika sürer.  Çözüm başarıyla dağıtıldıktan sonra göreceğiniz şeye bir örnek:

   ![Endüstriyel IoT OPC Twin mevcut projeye dağıtım](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   Çıktı, ortak bitiş noktasının URL'sini içerir. 

3. Komut dosyası başarıyla tamamlandıktan sonra, dosyayı `.env` kaydetmek isteyip istemediğinizi seçin.  Konsol gibi `.env` araçları kullanarak bulut bitiş noktasına bağlanmak veya geliştirme ve hata ayıklama için modülleri dağıtmak istiyorsanız ortam dosyasına ihtiyacınız vardır.

## <a name="troubleshooting-deployment-failures"></a>Sorun giderme dağıtım hataları

### <a name="resource-group-name"></a>Kaynak grubu adı

Kısa ve basit bir kaynak grubu adı kullandığınızdan emin olun.  Ad, kaynak adlandırma gereksinimlerine uyması gerektiği gibi kaynakları adlandırmak için de kullanılır.  

### <a name="website-name-already-in-use"></a>Zaten kullanılmakta olan web sitesi adı

Web sitesinin adının zaten kullanımda olması mümkündür.  Bu hatayla karşılaştıysanız, farklı bir uygulama adı kullanmanız gerekir.

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory (AAD) kaydı

Dağıtım komut dosyası, Azure Etkin Dizini'nde iki AAD uygulamasını kaydetmeye çalışır.  Seçili AAD kiracısı üzerindeki haklarınıza bağlı olarak, dağıtım başarısız olabilir. İki seçenek vardır:

1. Kiracılar listesinden bir AAD kiracıseçtiyseniz, komut dosyasını yeniden başlatın ve listeden farklı bir tane seçin.
2. Alternatif olarak, başka bir abonelikte özel bir AAD kiracısı dağıtın, komut dosyasını yeniden başlatın ve kullanmak üzere seçin.

> [!WARNING]
> Kimlik Doğrulama olmadan asla devam edin.  Bunu yapmayı seçerseniz, herkes OPC İkiz uç noktalarınıza Internet'ten kimlik doğrulaması sıza erişebilir.   Lastikleri tekmelemek için her zaman ["yerel" dağıtım seçeneğini](howto-opc-twin-deploy-dependencies.md) seçebilirsiniz.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Hepsi bir e-bir endüstriyel IoT hizmetleri demosu dağıtma

Sadece hizmetler ve bağımlılıklar yerine hepsi birer bir demo da dağıtabilirsiniz.  Bir demodaki tüm üç OPC UA sunucusu, OPC Twin modülü, tüm mikro hizmetler ve örnek bir Web Uygulaması içerir.  Bu gösteri amaçlıdır.

1. Deponun bir klonuna sahip olduğunuzdan emin olun (yukarıya bakın). Deponun kökünde bir PowerShell istemi açın ve çalıştırın:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy -type demo
    ```

2. Kaynak grubuna yeni bir ad ve web sitesine bir ad atamak için istemleri izleyin.  Komut dosyası başarıyla dağıtıldıktan sonra web uygulaması bitiş noktasının URL'sini görüntüler.

## <a name="deployment-script-options"></a>Dağıtım komut dosyası seçenekleri

Komut dosyası aşağıdaki parametreleri alır:

```powershell
-type
```

Dağıtım türü (vm, yerel, demo)

```powershell
-resourceGroupName
```

Varolan veya yeni bir kaynak grubunun adı olabilir.

```powershell
-subscriptionId
```

İsteğe bağlı olarak, kaynakların dağıtılacayacağı abonelik kimliği.

```powershell
-subscriptionName
```

Ya da abonelik adı.

```powershell
-resourceGroupLocation
```

İsteğe bağlı, bir kaynak grubu konumu. Belirtilirse, bu konumda yeni bir kaynak grubu oluşturmaya çalışır.

```powershell
-aadApplicationName
```

AAD uygulaması nın altında kayıt olunması gereken bir ad.

```powershell
-tenantId
```

AAD kiracı kullanmak.

```powershell
-credentials
```

## <a name="next-steps"></a>Sonraki adımlar

OPC Twin'i varolan bir projeye nasıl dağıtabileceğinizi öğrendiğiniz için, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [OPC UA İstemci ve OPC UA PLC güvenli iletişim](howto-opc-vault-secure.md)
