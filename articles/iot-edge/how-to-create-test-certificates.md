---
title: Test sertifikaları oluşturma - Azure IoT Edge | Microsoft Dokümanlar
description: Test sertifikaları oluşturun ve üretim dağıtımına hazırlanmak için bunları bir Azure IoT Edge aygıtına nasıl yükleyeceğimiz öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/14/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 364846f6cef196f6cefa7872af48f262b387db4f
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393813"
---
# <a name="create-demo-certificates-to-test-iot-edge-device-features"></a>IoT Edge cihaz özelliklerini test etmek için demo sertifikaları oluşturun

IoT Edge aygıtları, çalışma zamanı, modüller ve alt akış aygıtları arasında güvenli iletişim için sertifikalar gerektirir.
Gerekli sertifikaları oluşturmak için bir sertifika yetkiniz yoksa, test ortamınızda IoT Edge özelliklerini denemek için demo sertifikalarını kullanabilirsiniz.
Bu makalede, IoT Edge'in sınama sağladığı sertifika oluşturma komut dosyalarının işlevselliği açıklanmaktadır.

Bu sertifikaların süresi 30 gün içinde doluyor ve herhangi bir üretim senaryosunda kullanılmamalıdır.

Sertifikaları herhangi bir makinede oluşturabilir ve bunları IoT Edge cihazınıza kopyalayabilirsiniz.
Sertifikaları oluşturmak için birincil makinenizi kullanmak, bunları IoT Edge aygıtınızda oluşturmaktan daha kolaydır.
Birincil makinenizi kullanarak komut dosyalarını bir kez ayarlayabilir ve ardından birden çok aygıt için sertifika oluşturmak için işlemi yineleyebilirsiniz.

IoT Edge senaryonuzu test etmek için demo sertifikaları oluşturmak için aşağıdaki adımları izleyin:

1. Cihazınızda sertifika oluşturma için [komut dosyaları ayarlayın.](#set-up-scripts)
2. Senaryonuz için diğer tüm sertifikaları imzalamak için kullandığınız [kök CA sertifikasını oluşturun.](#create-root-ca-certificate)
3. Sınamak istediğiniz senaryo için ihtiyacınız olan sertifikaları oluşturun:
   * IoT Hub Aygıt Sağlama Hizmeti ile otomatik sağlamayı test etmek için [IoT Edge aygıt kimlik sertifikaları oluşturun.](#create-iot-edge-device-identity-certificates)
   * Üretim senaryolarını veya ağ geçidi senaryolarını test etmek için [IoT Edge aygıt CA sertifikaları oluşturun.](#create-iot-edge-device-ca-certificates)
   * Ağ geçidi senaryosunda ioT Hub'a doğru doğrulama aygıtlarını test etmek için [aşağı akış aygıt sertifikaları oluşturun.](#create-downstream-device-certificates)

## <a name="prerequisites"></a>Ön koşullar

Git yüklü bir geliştirme makinesi.

## <a name="set-up-scripts"></a>Komut dosyalarını ayarlama

GitHub'daki IoT Edge deposu, demo sertifikaları oluşturmak için kullanabileceğiniz sertifika oluşturma komut dosyalarını içerir.
Bu bölümde, windows veya Linux'ta bilgisayarınızda çalışacak komut dosyalarını hazırlamak için yönergeler verilmektedir.
Bir Linux makinesindeyseniz, [Linux'ta kurulum](#set-up-on-linux)yapmak için ileri ye atlayın.

### <a name="set-up-on-windows"></a>Windows'da ayarlama

Bir Windows aygıtında demo sertifikaları oluşturmak için OpenSSL'yi yüklemeniz ve ardından nesil komut dosyalarını klonlamanız ve PowerShell'de yerel olarak çalışacak şekilde ayarlamanız gerekir.

#### <a name="install-openssl"></a>OpenSSL'yi yükleyin

Sertifikaları oluşturmak için kullandığınız makineye Windows için OpenSSL'yi yükleyin.
Windows cihazınızda zaten OpenSSL yüklüyse, bu adımı atlayabilirsiniz, ancak OPENSSL.exe'nin PATH ortamı değişkeninizde kullanılabilir olduğundan emin olabilirsiniz.

OpenSSL'yi yüklemenin aşağıdaki seçenekler de dahil olmak üzere çeşitli yolları vardır:

* **Daha kolay:** [SourceForge'daki OpenSSL'den](https://sourceforge.net/projects/openssl/) [üçüncü taraf OpenSSL ikililerini](https://wiki.openssl.org/index.php/Binaries)indirin ve kurun. PATH ortamı değişkeninize openssl.exe için tam yol ekleyin.

* **Önerilen:** OpenSSL kaynak kodunu indirin ve ikilileri makinenizde kendiniz veya [vcpkg](https://github.com/Microsoft/vcpkg)üzerinden oluşturun. Aşağıda listelenen talimatlar, kaynak kodu indirmek, derlemek ve kolay adımlarla Windows makinenize OpenSSL yüklemek için vcpkg kullanır.

   1. vcpkg yüklemek istediğiniz bir dizine gidin. [Vcpkg'ı](https://github.com/Microsoft/vcpkg)indirmek ve yüklemek için talimatları izleyin.

   2. VCPKG yüklendikten sonra, Windows x64 için OpenSSL paketini yüklemek için powershell komutunu bir PowerShell komutundan çalıştırın. Yüklemenin tamamlanması genellikle yaklaşık 5 dakika sürer.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```

   3. Openssl.exe dosyasının çağrılması için kullanılabilecek şekilde PATH ortamı değişkenine ekleyin. `<vcpkg path>\installed\x64-windows\tools\openssl`

#### <a name="prepare-scripts-in-powershell"></a>PowerShell'de komut dosyaları hazırlama

Azure IoT Edge git deposu, test sertifikaları oluşturmak için kullanabileceğiniz komut dosyaları içerir.
Bu bölümde, IoT Edge repo'yu kopyalar ve komut dosyalarını çalıştırırsınız.

1. Yönetici modunda bir PowerShell penceresi açın.

2. Demo sertifikaları oluşturmak için komut dosyaları içeren IoT Edge git repo'yu klonla. Komutu `git clone` kullanın veya [ZIP indirin.](https://github.com/Azure/iotedge/archive/master.zip)

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Çalışmak istediğiniz dizine gidin. Bu makale boyunca, bu dizin * \<WRKDIR>* çağıracağız. Tüm sertifikalar ve anahtarlar bu çalışma dizininde oluşturulur.

4. Klonlanmış repo'daki yapılandırma ve komut dosyası dosyalarını çalışma dizininize kopyalayın.

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Repo'yu ZIP olarak indirdiyseniz, klasör adı `iotedge-master` ve yolun geri kalanı aynıdır.

5. Komut dosyalarını çalıştırmak için PowerShell'i etkinleştirin.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

6. Komut dosyalarının kullandığı işlevleri PowerShell'in genel ad alanına taşıyın.

   ```powershell
   . .\ca-certs.ps1
   ```

   PowerShell penceresi, bu komut dosyası tarafından oluşturulan sertifikaların yalnızca sınama amacıyla olduğunu ve üretim senaryolarında kullanılmaması gerektiğine dair bir uyarı görüntüler.

7. OpenSSL'nin doğru yüklendiğini doğrulayın ve varolan sertifikalarla ad çakışması olmayacağından emin olun. Sorun varsa, komut dosyası çıktısı sisteminizde bunları nasıl düzeltilenaçıklanmalıdır.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="set-up-on-linux"></a>Linux'ta kurulumu

Bir Windows aygıtında demo sertifikaları oluşturmak için, nesil komut dosyalarını klonlamanız ve bunları bash'ta yerel olarak çalışacak şekilde ayarlamanız gerekir.

1. Demo sertifikaları oluşturmak için komut dosyaları içeren IoT Edge git repo'yu klonla.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Çalışmak istediğiniz dizine gidin. Biz * \<WRKDIR>* olarak makale boyunca bu dizini atıfta bulunacağız. Tüm sertifika ve anahtar dosyaları bu dizinde oluşturulur.
  
3. Klonlanmış IoT Edge reposundaki config ve komut dosyası dosyalarını çalışma dizininize kopyalayın.

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

## <a name="create-root-ca-certificate"></a>Kök CA sertifikası oluşturma

Kök CA sertifikası, bir IoT Edge senaryosunu sınamak için diğer tüm demo sertifikalarını yapmak için kullanılır.
Birden çok IoT Edge veya downstream aygıtları için demo sertifikaları yapmak için aynı kök CA sertifikasını kullanmaya devam edebilirsiniz.

Çalışma klasörünüzde zaten bir kök CA sertifikası varsa, yeni bir tane oluşturmayın.
Yeni kök CA sertifikası eskinin üzerine yazar ve eskisinden yapılan alt akış sertifikaları çalışmayı durdurur.
Birden çok kök CA sertifikası istiyorsanız, bunları ayrı klasörlerde yönettiğinden emin olun.

Bu bölümdeki adımlara geçmeden önce, demo sertifikası oluşturma komut dosyalarıyla çalışan bir dizini hazırlamak için [komut dosyalarını ayarla](#set-up-scripts) bölümündeki adımları izleyin.

### <a name="windows"></a>Windows

1. Sertifika oluşturma komut dosyalarını yerleştirdiğiniz çalışma dizinine gidin.

1. Kök CA sertifikasını oluşturun ve bir ara sertifika imzalatın. Sertifikaların tümü çalışma dizininizde yer leştirilir.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur, ancak makaleler **kök CA sertifikası**için sorduğunda, aşağıdaki dosyayı kullanın:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

### <a name="linux"></a>Linux

1. Sertifika oluşturma komut dosyalarını yerleştirdiğiniz çalışma dizinine gidin.

1. Kök CA sertifikası ve bir ara sertifika oluşturun.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur, ancak makaleler **kök CA sertifikası**için sorduğunda, aşağıdaki dosyayı kullanın:

   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

## <a name="create-iot-edge-device-ca-certificates"></a>IoT Edge aygıt CA sertifikaları oluşturma

Üretime giden her IoT Edge cihazının config.yaml dosyasından başvurulan bir cihaz CA sertifikasına ihtiyacı vardır.
Aygıt CA sertifikası, aygıtta çalışan modüller için sertifika lar oluşturmaktan sorumludur.
Ayrıca, IoT Edge aygıtı nın akış aşağı aygıtlara bağlanırken kimliğini nasıl doğrulandığıdır.

Aygıt CA sertifikaları, IoT Edge aygıtındaki config.yaml dosyasının **Sertifika** bölümüne gider.

Bu bölümdeki adımlara geçmeden [önce, komut dosyalarını ayarla](#set-up-scripts) ve kök CA sertifikası bölümleri [oluştur'daki](#create-root-ca-certificate) adımları izleyin.

### <a name="windows"></a>Windows

1. Sertifika oluşturma komut dosyaları ve kök CA sertifikası olan çalışma dizinine gidin.

2. Aşağıdaki komutla IoT Edge aygıt CA sertifikasını ve özel anahtarı oluşturun. Ca sertifikası için bir ad sağlayın, örneğin çıktı dosyalarını adlandırmak için kullanılan **MyEdgeDeviceCA.**

   ```powershell
   New-CACertsEdgeDevice "MyEdgeDeviceCA"
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur. Aşağıdaki sertifika ve anahtar çiftinin bir IoT Edge cihazına kopyalanması ve config.yaml dosyasında başvurulması gerekir:

   * `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`

Bu komut dosyalarına geçirilen ağ geçidi aygıtı adı, config.yaml'daki "hostname" parametresi veya Aygıtın IoT Hub'daki kimliğiyle aynı olmamalıdır.
Komut dosyaları, bir kullanıcının her iki yerde de aynı adı kullanarak IoT Edge'i kurması durumunda ad çakışmasını önlemek için ağ geçidi aygıtı adına ".ca" dizesini ekleyerek herhangi bir sorundan kaçınmanıza yardımcı olur.
Ancak, aynı adı kullanmaktan kaçınmak iyi bir uygulamadır.

### <a name="linux"></a>Linux

1. Sertifika oluşturma komut dosyaları ve kök CA sertifikası olan çalışma dizinine gidin.

2. Aşağıdaki komutla IoT Edge aygıt CA sertifikasını ve özel anahtarı oluşturun. Ca sertifikası için bir ad sağlayın, örneğin çıktı dosyalarını adlandırmak için kullanılan **MyEdgeDeviceCA.**

   ```bash
   ./certGen.sh create_edge_device_certificate "MyEdgeDeviceCA"
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur. Aşağıdaki sertifika ve anahtar çiftinin bir IoT Edge cihazına kopyalanması ve config.yaml dosyasında başvurulması gerekir:

   * `<WRKDIR>/certs/iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-MyEdgeDeviceCA.key.pem`

Bu komut dosyalarına geçirilen ağ geçidi aygıtı adı, config.yaml'daki "hostname" parametresi veya Aygıtın IoT Hub'daki kimliğiyle aynı olmamalıdır.
Komut dosyaları, bir kullanıcının her iki yerde de aynı adı kullanarak IoT Edge'i kurması durumunda ad çakışmasını önlemek için ağ geçidi aygıtı adına ".ca" dizesini ekleyerek herhangi bir sorundan kaçınmanıza yardımcı olur.
Ancak, aynı adı kullanmaktan kaçınmak iyi bir uygulamadır.

## <a name="create-iot-edge-device-identity-certificates"></a>IoT Edge aygıt kimlik sertifikaları oluşturma

Aygıt kimlik sertifikaları, [Azure IoT Hub Aygıt Sağlama Hizmeti (DPS)](../iot-dps/index.yml)aracılığıyla IoT Edge aygıtlarını sağlamak için kullanılır.

Aygıt kimlik sertifikaları, IoT Edge aygıtındaki config.yaml dosyasının **Sağlama** bölümüne gider.

Bu bölümdeki adımlara geçmeden [önce, komut dosyalarını ayarla](#set-up-scripts) ve kök CA sertifikası bölümleri [oluştur'daki](#create-root-ca-certificate) adımları izleyin.

### <a name="windows"></a>Windows

Aşağıdaki komutla IoT Edge aygıt kimlik sertifikasını ve özel anahtarı oluşturun:

```powershell
New-CACertsEdgeDeviceIdentity "<name>"
```

Bu komuta geçtiğiniz ad, IoT Hub'daki IoT Edge aygıtının aygıt kimliği olacaktır.

Yeni aygıt kimlik komutu, DPS'de tek tek bir kayıt oluştururken ve IoT Edge çalışma süresini yüklerken kullanacağınız üç dosya da dahil olmak üzere çeşitli sertifika lar ve anahtar dosyalar oluşturur:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>\certs\iot-edge-device-identity-<name>.cert.pem`
* `<WRKDIR>\private\iot-edge-device-identity-<name>.key.pem`

### <a name="linux"></a>Linux

Aşağıdaki komutla IoT Edge aygıt kimlik sertifikasını ve özel anahtarı oluşturun:

```bash
./certGen.sh create_edge_device_identity_certificate "<name>"
```

Bu komuta geçtiğiniz ad, IoT Hub'daki IoT Edge aygıtının aygıt kimliği olacaktır.

Komut dosyası, DPS'de tek tek bir kayıt oluştururken ve IoT Edge çalışma süresini yüklerken kullanacağınız üç dosya da dahil olmak üzere birkaç sertifika ve anahtar dosya oluşturur:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>/certs/iot-edge-device-identity-<name>.cert.pem`
* `<WRKDIR>/private/iot-edge-device-identity-<name>.key.pem`

## <a name="create-downstream-device-certificates"></a>Akış aşağı aygıt sertifikaları oluşturma

Bir ağ geçidi senaryosu için bir aşağı IoT aygıtı kuruyorsanız, X.509 kimlik doğrulaması için demo sertifikaları oluşturabilirsiniz.
X.509 sertifikalarını kullanarak bir IoT aygıtının kimliğini doğrulamanın iki yolu vardır: kendi imzalı sertifikaları kullanmak veya sertifika yetkilisini (CA) imzalı sertifikalar kullanmak.
X.509 kendi imzalı kimlik doğrulaması için, bazen parmak izi kimlik doğrulaması olarak da adlandırılır, IoT aygıtınıza yerleştirmek için yeni sertifikalar oluşturmanız gerekir.
Bu sertifikalarda kimlik doğrulaması için IoT Hub ile paylaştığınız bir parmak izi vardır.
X.509 sertifika yetkilisi (CA) imzalı kimlik doğrulaması için, IoT Hub'Da kayıtlı ve IoT aygıtınızın sertifikalarını imzalamak için kullandığınız bir kök CA sertifikasına ihtiyacınız vardır.
Kök CA sertifikası veya ara sertifikalarından herhangi biri tarafından verilen bir sertifikayı kullanan tüm aygıtların kimlik doğrulaması yapmasına izin verilir.

Sertifika oluşturma komut dosyaları, bu kimlik doğrulama senaryolarından birini test etmek için demo sertifikaları oluşturmanıza yardımcı olabilir.

Bu bölümdeki adımlara geçmeden [önce, komut dosyalarını ayarla](#set-up-scripts) ve kök CA sertifikası bölümleri [oluştur'daki](#create-root-ca-certificate) adımları izleyin.

### <a name="self-signed-certificates"></a>Kendi imzalı sertifikalar

Kendi imzalı sertifikalara sahip bir IoT aygıtını doğruladığınızda, çözümünüz için kök CA sertifikasını temel alan aygıt sertifikaları oluşturmanız gerekir.
Ardından, IoT Hub'a sağlamak için sertifikalardan bir hexadecimal "parmak izi" alırsınız.
IoT aygıtınızın ioT Hub ile kimlik doğrulaması yapabilmesi için aygıt sertifikalarının bir kopyasına da ihtiyacı vardır.

#### <a name="windows"></a>Windows

1. Sertifika oluşturma komut dosyaları ve kök CA sertifikası olan çalışma dizinine gidin.

2. Akış aşağı aygıt için iki sertifika (birincil ve ikincil) oluşturun. Kullanımı kolay bir adlandırma kuralı, sertifikaları IoT aygıtının adı ve ardından birincil veya ikincil etiketle oluşturmaktır. Örneğin:

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur. Aşağıdaki sertifika ve anahtar çiftleri downstream IoT aygıtına kopyalanması ve IoT Hub'a bağlanan uygulamalarda başvurulması gerekir:

   * `<WRKDIR>\certs\iot-device-<device name>-primary-full-chain.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary-full-chain.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-primary.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>-primary.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>-secondary.cert.pfx`
   * `<WRKDIR>\private\iot-device-<device name>-primary.key.pem`
   * `<WRKDIR>\private\iot-device-<device name>-secondary.key.pem`

3. Her sertifikadan SHA1 parmak izini (IoT Hub bağlamlarında parmak izi olarak adlandırılır) alın. Parmak izi 40 hexadecimal karakter dize. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki openssl komutunu kullanın:

   ```PowerShell
   openssl x509 -in <WRKDIR>\certs\iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Bu komutu iki kez, birincil sertifika için bir kez ve ikincil sertifika için bir kez çalıştırın. Kendi imzalı X.509 sertifikalarını kullanarak yeni bir IoT aygıtı kaydettirdiğinizde her iki sertifika için de parmak izi sağlarsınız.

#### <a name="linux"></a>Linux

1. Sertifika oluşturma komut dosyaları ve kök CA sertifikası olan çalışma dizinine gidin.

2. Akış aşağı aygıt için iki sertifika (birincil ve ikincil) oluşturun. Kullanımı kolay bir adlandırma kuralı, sertifikaları IoT aygıtının adı ve ardından birincil veya ikincil etiketle oluşturmaktır. Örneğin:

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur. Aşağıdaki sertifika ve anahtar çiftleri downstream IoT aygıtına kopyalanması ve IoT Hub'a bağlanan uygulamalarda başvurulması gerekir:

   * `<WRKDIR>/certs/iot-device-<device name>-primary-full-chain.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary-full-chain.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-primary.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device name>-primary.cert.pfx`
   * `<WRKDIR>/certs/iot-device-<device name>-secondary.cert.pfx`
   * `<WRKDIR>/private/iot-device-<device name>-primary.key.pem`
   * `<WRKDIR>/private/iot-device-<device name>-secondary.key.pem`

3. Her sertifikadan SHA1 parmak izini (IoT Hub bağlamlarında parmak izi olarak adlandırılır) alın. Parmak izi 40 hexadecimal karakter dize. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki openssl komutunu kullanın:

   ```bash
   openssl x509 -in <WRKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Kendi imzalı X.509 sertifikalarını kullanarak yeni bir IoT aygıtı kaydettirdiğinizde hem birincil hem de ikincil parmak izini sağlarsınız.

### <a name="ca-signed-certificates"></a>CA imzalı sertifikalar

Kendi imzalı sertifikalara sahip bir IoT aygıtının kimliğini doğruladığınızda, çözümünüz için kök CA sertifikasını IoT Hub'a yüklemeniz gerekir.
Ardından, IoT Hub'a kök CA sertifikasına sahip olduğunuzu kanıtlamak için bir doğrulama gerçekleştirirsiniz.
Son olarak, IoT aygıtınıza ioT aygıtınızın kimliğini doğrulamak için aygıt sertifikaları oluşturmak için aynı kök CA sertifikasını kullanırsınız.

Bu bölümdeki sertifikalar, Azure [IoT hub'ınızdaki X.509 güvenliğini ayarlama adımlarına](../iot-hub/iot-hub-security-x509-get-started.md)yöneliktir.

#### <a name="windows"></a>Windows

1. Kök CA sertifika dosyasını çalışma dizininizden `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`IoT hub'ınıza yükleyin.

2. Bu kök CA sertifikasına sahip olduğunuzu doğrulamak için Azure portalında sağlanan kodu kullanın.

   ```PowerShell
   New-CACertsVerificationCert "<verification code>"
   ```

3. Akış aşağı aygıtınız için bir sertifika zinciri oluşturun. Aygıtın IoT Hub'da kayıtlı olduğu cihaz kimliğini kullanın.

   ```PowerShell
   New-CACertsDevice "<device id>"
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur. Aşağıdaki sertifika ve anahtar çiftleri downstream IoT aygıtına kopyalanması ve IoT Hub'a bağlanan uygulamalarda başvurulması gerekir:

   * `<WRKDIR>\certs\iot-device-<device id>.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>-full-chain.cert.pem`  
   * `<WRKDIR>\private\iot-device-<device id>.key.pem`

#### <a name="linux"></a>Linux

1. Kök CA sertifika dosyasını çalışma dizininizden `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`IoT hub'ınıza yükleyin.

2. Bu kök CA sertifikasına sahip olduğunuzu doğrulamak için Azure portalında sağlanan kodu kullanın.

   ```bash
   ./certGen.sh create_verification_certificate "<verification code>"
   ```

3. Akış aşağı aygıtınız için bir sertifika zinciri oluşturun. Aygıtın IoT Hub'da kayıtlı olduğu cihaz kimliğini kullanın.

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

   Bu komut dosyası komutu birkaç sertifika ve anahtar dosyaları oluşturur. Aşağıdaki sertifika ve anahtar çiftleri downstream IoT aygıtına kopyalanması ve IoT Hub'a bağlanan uygulamalarda başvurulması gerekir:

   * `<WRKDIR>/certs/iot-device-<device id>.cert.pem`
   * `<WRKDIR>/certs/iot-device-<device id>.cert.pfx`
   * `<WRKDIR>/certs/iot-device-<device id>-full-chain.cert.pem`  
   * `<WRKDIR>/private/iot-device-<device id>.key.pem`
