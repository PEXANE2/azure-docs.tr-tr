---
title: Test sertifikaları oluşturma-Azure IoT Edge | Microsoft Docs
description: Test sertifikaları oluşturun ve üretim dağıtımına hazırlanmak için bunları Azure IoT Edge cihaza yüklemeyi öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3ea3eb696362565413a468951f203ef0a08436e3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75482276"
---
# <a name="create-demo-certificates-to-test-iot-edge-device-features"></a>IoT Edge cihaz özelliklerini sınamak için tanıtım sertifikaları oluşturma

IoT Edge cihazlar, çalışma zamanı, modüller ve tüm aşağı akış cihazları arasında güvenli iletişim için sertifikalar gerektirir.
Gerekli sertifikaları oluşturmak için bir sertifika yetkiliniz yoksa, test ortamınızda IoT Edge özellikleri denemek için tanıtım sertifikalarını kullanabilirsiniz.
Bu makalede, test için IoT Edge sağladığı sertifika oluşturma betiklerinin işlevleri açıklanmaktadır. 

Bu sertifikaların süreleri 30 gün içinde doluyor ve herhangi bir üretim senaryosunda kullanılmamalıdır. 

Herhangi bir makinede sertifika oluşturabilir ve sonra bunları IoT Edge cihazınıza kopyalayabilirsiniz.
IoT Edge cihazınızın kendisi üzerinde oluşturmak yerine, sertifikaların oluşturulması için birincil makinenizin kullanılması daha kolay.
Birincil makinenizi kullanarak betikleri bir kez ayarlayabilir ve ardından birden çok cihaz için sertifika oluşturmak üzere işlemi yineleyebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Git yüklü bir geliştirme makinesi. 

## <a name="set-up-scripts"></a>Betikleri ayarlama

GitHub 'daki IoT Edge deposu, tanıtım sertifikaları oluşturmak için kullanabileceğiniz sertifika oluşturma betikleri içerir.
Bu bölüm, Windows veya Linux 'ta, betikleri bilgisayarınızda çalışacak şekilde hazırlamaya ilişkin yönergeler sağlar.
Linux makineniz varsa [Linux üzerinde ayarlamaya](#set-up-on-linux)devam edin.

### <a name="set-up-on-windows"></a>Windows üzerinde ayarlama

Bir Windows cihazında tanıtım sertifikaları oluşturmak için, OpenSSL yüklemeniz ve ardından oluşturma komut dosyalarını kopyalamanız ve bunları PowerShell 'de yerel olarak çalışacak şekilde ayarlamanız gerekir. 

#### <a name="install-openssl"></a>OpenSSL yükleyin

OpenSSL için Windows sertifikalarını oluşturmak için kullanmakta olduğunuz makineye yükleyin.
Windows cihazınızda OpenSSL zaten yüklüyse, bu adımı atlayabilirsiniz, ancak PATH ortam değişkeninizdeki OpenSSL. exe ' nin kullanılabilir olduğundan emin olabilirsiniz. 

OpenSSL 'nin yüklenmesi için aşağıdaki seçenekler de dahil olmak üzere çeşitli yollar vardır:

* **Daha kolay:** [Üçüncü taraf OpenSSL ikililerini](https://wiki.openssl.org/index.php/Binaries)indirin ve yükleyin, örneğin, [SourceForge üzerinde OpenSSL](https://sourceforge.net/projects/openssl/). Tam yolunu openssl.exe, PATH ortam değişkenine ekleyin. 
   
* **Önerilen:** OpenSSL kaynak kodunu indirebilir ve ikili dosyaları makinenizde sizin tarafınızdan veya aracılığıyla derleme [vcpkg](https://github.com/Microsoft/vcpkg). Aşağıda listelenen yönergeleri vcpkg kaynak kodu, derleme ve yükleme OpenSSL kolay adımda Windows makinenize indirmek için kullanın.

   1. Vcpkg yüklemek istediğiniz bir dizine gidin. İndirme ve yükleme yönergelerini [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Vcpkg yüklendikten sonra, Windows x64 için OpenSSL paketini yüklemek üzere bir PowerShell isteminden aşağıdaki komutu çalıştırın. Yükleme işleminin genellikle yaklaşık 5 dakika sürer.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```
   3. Ekleme `<vcpkg path>\installed\x64-windows\tools\openssl` , PATH ortam değişkenine böylece openssl.exe dosya çağırma için kullanılabilir.

#### <a name="prepare-scripts-in-powershell"></a>PowerShell 'de betikleri hazırlama

Git deposu Azure IoT Edge test sertifikaları oluşturmak için kullanabileceğiniz betikleri içerir.
Bu bölümde, IoT Edge depoyu klonlayın ve betikleri yürütün. 

1. Yönetici modunda bir PowerShell penceresi açın. 

2. Tanıtım sertifikaları oluşturmak için betikler içeren IoT Edge Git deposunu kopyalayın. Kullanım `git clone` komutu veya [ZIP indir](https://github.com/Azure/iotedge/archive/master.zip). 

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Çalışmak istediğiniz dizine gidin. Bu makalenin tamamında, bu dizini *\<WRKDIR >* çağıracağız. Tüm sertifikalar ve anahtarlar bu çalışma dizininde oluşturulacak.

4. Kopyalanan deponun yapılandırma ve betik dosyalarını çalışma dizininize kopyalayın. 

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Depoyu ZIP olarak indirdiyseniz, klasör adı `iotedge-master` ve yolun geri kalanı aynı olur. 

5. PowerShell komut dosyalarını çalıştırmak etkinleştirin.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Betikler tarafından kullanılan işlevleri PowerShell 'in genel ad alanına taşıyın.
   
   ```powershell
   . .\ca-certs.ps1
   ```

   PowerShell penceresi, bu komut dosyası tarafından oluşturulan sertifikaların yalnızca test amaçlı olduğunu ve üretim senaryolarında kullanılmamalıdır bir uyarı görüntüler.

8. OpenSSL 'nin doğru şekilde yüklendiğini doğrulayın ve var olan sertifikalarla ad çakışmaları olmadığından emin olun. Sorun varsa, betik çıktısı bunların sisteminizde nasıl düzeltileceğini betimlemelidir.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="set-up-on-linux"></a>Linux üzerinde ayarlama

Bir Windows cihazında tanıtım sertifikaları oluşturmak için, oluşturma komut dosyalarını kopyalamanız ve Bash 'te yerel olarak çalışacak şekilde ayarlamanız gerekir. 

1. Tanıtım sertifikaları oluşturmak için betikler içeren IoT Edge Git deposunu kopyalayın.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Çalışmak istediğiniz dizine gidin. *\<WRKDIR >* olarak makale genelinde bu dizine başvuracağız. Tüm sertifika ve anahtar dosyaları bu dizinde oluşturulacaktır.
  
3. Yapılandırma ve betik dosyalarını kopyalanan IoT Edge deposundan çalışma dizininize kopyalayın.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

<!--
4. Configure OpenSSL to generate certificates using the provided script. 

   ```bash
   chmod 700 certGen.sh 
   ```
-->

## <a name="create-root-ca-certificate"></a>Kök CA sertifikası oluştur

Kök CA sertifikası, IoT Edge senaryonun test edilmesine yönelik tüm diğer tanıtım sertifikalarını oluşturmak için kullanılır.
Birden çok IoT Edge veya aşağı akış cihazı için tanıtım sertifikaları oluşturmak üzere aynı kök CA sertifikasını kullanmaya devam edebilirsiniz. 

Çalışma klasörünüzde bir kök CA sertifikanız zaten varsa yeni bir tane oluşturun.
Yeni kök CA sertifikası eskilerinin üzerine yazacak ve eskilerinin yaptığı tüm aşağı akış sertifikaları çalışmayı durduracaktır.
Birden çok kök CA sertifikası isterseniz, bunları ayrı klasörlerde yönetdiğinizden emin olun. 

Bu bölümdeki adımlara geçmeden önce, tanıtım sertifikası oluşturma betiklerine sahip bir çalışma dizini hazırlamak için [betikleri ayarlama](#set-up-scripts) bölümündeki adımları izleyin. 

### <a name="windows"></a>Windows

1. Sertifika oluşturma betiklerini yerleştirdiğiniz çalışma dizinine gidin.

1. Kök CA sertifikasını oluşturun ve bir ara sertifikayı imzalayıp imzalayın. Sertifikalar çalışma dizininize yerleştirilir.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur, ancak makaleler **kök CA sertifikasını**sorarken aşağıdaki dosyayı kullanın:
   
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

### <a name="linux"></a>Linux

1. Sertifika oluşturma betiklerini yerleştirdiğiniz çalışma dizinine gidin.

1. Kök CA sertifikasını ve bir ara sertifikayı oluşturun. 

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur, ancak makaleler **kök CA sertifikasını**sorarken aşağıdaki dosyayı kullanın:
   
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

## <a name="create-iot-edge-device-ca-certificates"></a>IoT Edge cihaz CA sertifikaları oluşturma

Üretime giden her IoT Edge cihaz, config. YAML dosyasından başvurulan bir cihaz CA sertifikasına ihtiyaç duyuyor. Cihaz CA sertifikası, cihazda çalışan modüller için sertifika oluşturmaktan sorumludur. Ayrıca, IoT Edge cihaz, aşağı akış cihazlarına bağlanırken kimliğini doğrular. 

Bu bölümdeki adımlara geçmeden önce, [betikleri ayarlama](#set-up-scripts) ve [kök CA sertifikası oluşturma](#create-root-ca-certificate) bölümlerindeki adımları izleyin. 


### <a name="windows"></a>Windows

1. Sertifika oluşturma betikleri ve kök CA sertifikası olan çalışma dizinine gidin.

2. Aşağıdaki komutla IoT Edge cihaz CA sertifikasını ve özel anahtarı oluşturun. CA sertifikası için bir ad sağlayın (örneğin, çıkış dosyalarını adlandırmak için kullanılan **Myedgedeviceca**). 

   ```powershell
   New-CACertsEdgeDevice "MyEdgeDeviceCA"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur. Aşağıdaki sertifika ve anahtar çiftinin IoT Edge bir cihaza kopyalanması ve config. YAML dosyasında başvurulması gerekir:
   
   * `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`

Bu betiklerin geçirildiği ağ geçidi cihaz adı config. YAML içindeki "hostname" parametresiyle aynı olmamalıdır. Betikler, bir kullanıcının her iki yerde de aynı adı kullanarak IoT Edge ayarlaması durumunda ad çarpışmasını engellemek için ağ geçidi cihaz adına ". ca" dizesi ekleyerek herhangi bir sorunu önlemenize yardımcı olur. Ancak, aynı adı kullanmaktan kaçınmak iyi bir uygulamadır.


### <a name="linux"></a>Linux

1. Sertifika oluşturma betikleri ve kök CA sertifikası olan çalışma dizinine gidin.

2. Aşağıdaki komutla IoT Edge cihaz CA sertifikasını ve özel anahtarı oluşturun. CA sertifikası için bir ad sağlayın (örneğin, çıkış dosyalarını adlandırmak için kullanılan **Myedgedeviceca**). 

   ```bash
   ./certGen.sh create_edge_device_certificate "MyEdgeDeviceCA"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur. Aşağıdaki sertifika ve anahtar çiftinin IoT Edge bir cihaza kopyalanması ve config. YAML dosyasında başvurulması gerekir:
   
   * `<WRKDIR>/certs/iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-MyEdgeDeviceCA.key.pem`

Bu betiklerin geçirildiği ağ geçidi cihaz adı config. YAML içindeki "hostname" parametresiyle aynı olmamalıdır. Betikler, bir kullanıcının her iki yerde de aynı adı kullanarak IoT Edge ayarlaması durumunda ad çarpışmasını engellemek için ağ geçidi cihaz adına ". ca" dizesi ekleyerek herhangi bir sorunu önlemenize yardımcı olur. Ancak, aynı adı kullanmaktan kaçınmak iyi bir uygulamadır.


## <a name="create-x509-certs-for-downstream-devices"></a>Aşağı akış cihazları için X. 509.440 sertifikaları oluşturma

Bir ağ geçidi senaryosu için bir aşağı akış IoT cihazı ayarlıyorsanız, X. 509.440 kimlik doğrulaması için tanıtım sertifikaları oluşturabilirsiniz.
X. 509.440 sertifikalarını kullanarak IoT cihazının kimlik doğrulamasının iki yolu vardır: otomatik olarak imzalanan sertifikalar kullanma veya sertifika yetkilisi (CA) imzalı sertifikaları kullanma. X. 509.440 otomatik imzalı kimlik doğrulaması için bazen parmak izi kimlik doğrulaması olarak da adlandırılan, IoT cihazınıza yerleştirilecek yeni sertifikalar oluşturmanız gerekir.
Bu sertifikaların kimlik doğrulaması için IoT Hub paylaştığınız bir parmak izi vardır.
X. 509.440 sertifika yetkilisi (CA) imzalı kimlik doğrulaması için, IoT cihazınız için sertifikaları imzalamak üzere kullandığınız IoT Hub kayıtlı bir kök CA sertifikasına ihtiyacınız vardır.
Kök CA sertifikası veya ara sertifikalarının herhangi birinin verdiği bir sertifikayı kullanan tüm cihazların kimlik doğrulamasına izin verilir.

Sertifika oluşturma betikleri, bu kimlik doğrulama senaryolarından birini test etmek için tanıtım sertifikaları almanıza yardımcı olabilir.

Bu bölümdeki adımlara geçmeden önce, [betikleri ayarlama](#set-up-scripts) ve [kök CA sertifikası oluşturma](#create-root-ca-certificate) bölümlerindeki adımları izleyin. 

### <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

Otomatik olarak imzalanan sertifikalarla bir IoT cihazının kimliğini doğruladığınızda, çözümünüz için kök CA sertifikasını temel alan cihaz sertifikaları oluşturmanız gerekir.
Daha sonra, IoT Hub sağlamak için sertifikalardan onaltılık bir "parmak izi" elde edersiniz.
IoT cihazınız, IoT Hub kimlik doğrulaması yapabilmesi için cihaz sertifikalarının bir kopyasına de ihtiyaç duyuyor. 

#### <a name="windows"></a>Windows

1. Sertifika oluşturma betikleri ve kök CA sertifikası olan çalışma dizinine gidin.

2. Aşağı akış cihazı için iki sertifika (birincil ve ikincil) oluşturun. Kullanımı kolay bir adlandırma kuralı, IoT cihazının adı ve ardından birincil veya ikincil etiket ile sertifikalar oluşturmaktır. Örneğin:

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur. Aşağıdaki sertifika ve anahtar çiftlerinin, aşağı akış IoT cihazına kopyalanması ve IoT Hub bağlanan uygulamalarda başvurulmaları gerekir:
   
   * `<WRKDIR>\certs\iot-device-<device name>-primary-full-chain.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary-full-chain.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-primary.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-primary.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary.cert.pfx`
   * `<WRKDIR>\private\iot-device-<device name>-primary.key.pem`
   * `<WRKDIR>\private\iot-device-<device name>-secondary.key.pem`

3. Her bir sertifikadan SHA1 parmak izini (IoT Hub bağlamlarda parmak izi olarak adlandırılır) alın. Parmak izi, 40 onaltılık bir karakter dizesidir. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki OpenSSL komutunu kullanın: 

   ```PowerShell
   openssl x509 -in <WRKDIR>\certs\iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Birincil sertifika ve ikincil sertifika için bir kez olmak üzere bu komutu iki kez çalıştırın. Otomatik olarak imzalanan X. 509.440 sertifikalarını kullanarak yeni bir IoT cihazını kaydettiğinizde her iki sertifika için de parmak izleri sağlarsınız. 

#### <a name="linux"></a>Linux

1. Sertifika oluşturma betikleri ve kök CA sertifikası olan çalışma dizinine gidin.

2. Aşağı akış cihazı için iki sertifika (birincil ve ikincil) oluşturun. Kullanımı kolay bir adlandırma kuralı, IoT cihazının adı ve ardından birincil veya ikincil etiket ile sertifikalar oluşturmaktır. Örneğin:

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur. Aşağıdaki sertifika ve anahtar çiftlerinin, aşağı akış IoT cihazına kopyalanması ve IoT Hub bağlanan uygulamalarda başvurulmaları gerekir:
   
   * `<WRKDIR>/certs/iot-device-<device name>-primary-full-chain.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary-full-chain.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-primary.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-primary.cert.pfx`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary.cert.pfx`
   * `<WRKDIR>/private/iot-device-<device name>-primary.key.pem`
   * `<WRKDIR>/private/iot-device-<device name>-secondary.key.pem`

3. Her bir sertifikadan SHA1 parmak izini (IoT Hub bağlamlarda parmak izi olarak adlandırılır) alın. Parmak izi, 40 onaltılık bir karakter dizesidir. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki OpenSSL komutunu kullanın: 

   ```bash
   openssl x509 -in <WRKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Otomatik olarak imzalanan X. 509.440 sertifikalarını kullanarak yeni bir IoT cihazını kaydettiğinizde hem birincil hem de ikincil parmak izini sağlarsınız. 

### <a name="ca-signed-certificates"></a>CA-imzalı sertifikalar

Otomatik olarak imzalanan sertifikalarla bir IoT cihazının kimliğini doğruladığınızda, çözümünüzün IoT Hub için kök CA sertifikasını yüklemeniz gerekir.
Daha sonra, kök CA sertifikasına sahip olduğunuz IoT Hub kanıtlamak için bir doğrulama gerçekleştirirsiniz.
Son olarak, IoT Hub ile kimlik doğrulaması yapabilmesi için IoT cihazınıza koymak üzere cihaz sertifikaları oluşturmak üzere aynı kök CA sertifikasını kullanırsınız. 

Bu bölümdeki sertifikalar, [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md)adımlarındaki adımlara yöneliktir.

#### <a name="windows"></a>Windows

1. Kök CA sertifika dosyasını çalışma dizininizden `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`, IoT Hub 'ınıza yükleyin. 

2. Bu kök CA sertifikasına sahip olduğunuzu doğrulamak için Azure portal belirtilen kodu kullanın. 

   ```PowerShell
   New-CACertsVerificationCert "<verification code>"
   ```

3. Aşağı akış cihazınız için bir sertifika zinciri oluşturun. IoT Hub ' de cihazın kayıtlı olduğu cihaz KIMLIĞINI kullanın. 

   ```PowerShell
   New-CACertsDevice "<device id>"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur. Aşağıdaki sertifika ve anahtar çiftlerinin, aşağı akış IoT cihazına kopyalanması ve IoT Hub bağlanan uygulamalarda başvurulmaları gerekir:

   * `<WRKDIR>\certs\iot-device-<device id>.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>-full-chain.cert.pem`   
   * `<WRKDIR>\private\iot-device-<device id>.key.pem`

#### <a name="linux"></a>Linux

1. Kök CA sertifika dosyasını çalışma dizininizden `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`, IoT Hub 'ınıza yükleyin. 

2. Bu kök CA sertifikasına sahip olduğunuzu doğrulamak için Azure portal belirtilen kodu kullanın. 

   ```bash
   ./certGen.sh create_verification_certificate "<verification code>"
   ```

3. Aşağı akış cihazınız için bir sertifika zinciri oluşturun. IoT Hub ' de cihazın kayıtlı olduğu cihaz KIMLIĞINI kullanın. 

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

   Bu betik komutu birkaç sertifika ve anahtar dosyası oluşturur. Aşağıdaki sertifika ve anahtar çiftlerinin, aşağı akış IoT cihazına kopyalanması ve IoT Hub bağlanan uygulamalarda başvurulmaları gerekir:

   * `<WRKDIR>/certs/iot-device-<device id>.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device id>.cert.pfx`
   * `<WRKDIR>/certs/iot-device-<device id>-full-chain.cert.pem`   
   * `<WRKDIR>/private/iot-device-<device id>.key.pem`

