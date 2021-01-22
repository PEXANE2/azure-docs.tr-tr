---
title: Windows 'da Linux için Azure IoT Edge yüklemesi | Microsoft Docs
description: Windows cihazlarındaki yükleme yönergelerini Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/20/2021
ms.author: v-tcassi
monikerRange: =iotedge-2018-06
ms.openlocfilehash: e603e99c1aa9c49d20bdb281bbfd96131ae2efdd
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98663630"
---
# <a name="install-and-provision-azure-iot-edge-for-linux-on-a-windows-device-preview"></a>Windows cihazında Linux için Azure IoT Edge yükleyip sağlama (Önizleme)

Azure IoT Edge çalışma zamanı, bir cihazı IoT Edge cihazına dönüştürür. Çalışma zamanı, bılgısayar sınıfındaki cihazlara endüstriyel sunuculara dağıtılabilir. Bir cihaz IoT Edge çalışma zamanıyla yapılandırıldığında, buluttan cihaza iş mantığını dağıtmaya başlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

Windows üzerinde Linux için Azure IoT Edge, Linux sanal makinelerini kullanarak Windows cihazlarda Azure IoT Edge kullanmanıza olanak tanır. Azure IoT Edge Linux sürümü ve ile dağıtılan tüm Linux modülleri sanal makinede çalışır. Buradan, Windows Uygulamaları ve kodu ve IoT Edge çalışma zamanı ve modülleri birbirleriyle serbestçe etkileşim kurabilir.

Bu makalede, bir Windows cihazında IoT Edge ayarlama adımları listelenir. Bu adımlar, Windows cihazınızda çalıştırmak üzere IoT Edge çalışma zamanını içeren bir Linux sanal makinesini dağıtır ve ardından cihazı IoT Hub cihaz kimliğiyle sağlar.

>[!NOTE]
>Windows üzerinde Linux için IoT Edge [genel önizlemede](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

* Geçerli aboneliği olan bir Azure hesabı. [Azure aboneliğiniz](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* Azure 'da ücretsiz veya standart bir katman [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) .

* Aşağıdaki en düşük sistem gereksinimlerine sahip bir Windows cihazı:

  * Windows 10 sürüm 1809 veya üzeri; derleme 17763 veya üzeri
  * Professional, Enterprise veya Server sürümleri
  * En düşük RAM: 4 GB (8 GB önerilir)
  * Minimum depolama alanı: 10 GB

* Windows Yönetim Merkezi için Azure IoT Edge uzantısı yüklü Windows Yönetim Merkezi Insider derlemesi 'ne erişim:  <!-- The link below needs the language localization to work; otherwise broken -->
   1. [Windows Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)' i ziyaret edin.

   1. Önizlemeler açılan menüsünde **Windows Yönetim Merkezi önizleme-derleme 2012**' i seçin ve **Onayla**' yı seçin.

      ![Kullanılabilir önizlemelerde açılan menüden Windows Yönetim Merkezi önizlemesi-derleme 2012 ' ı seçin.](./media/how-to-install-iot-edge-on-windows/select-windows-admin-center-preview-build.png)

   1. **Dil Seç** açılan menüsünde, **İngilizce**' yi seçin ve **Onayla**' yı seçin.

   1. *WindowsAdminCenterPreview2012.msi* Indirmek Için **Şimdi İndir** ' i seçin.

   1. *WindowsAdminCenterPreview2012.msi* çalıştırın ve Windows Yönetim Merkezi 'ni yüklemek için Install Wizard istemlerini izleyin. Yüklendikten sonra Windows Yönetim Merkezi 'ni açın.

   1. Windows Yönetim Merkezi 'nin ilk sürümünde kullanılacak bir sertifika seçmeniz istenir. Sertifikanız olarak **Windows Yönetim Merkezi istemcisi** ' ni seçin.

   1. Azure IoT Edge uzantısının yüklenmesi zaman alabilir. Windows Yönetim Merkezi panosunun sağ üst köşesindeki dişli simgesini seçin.

      ![Ayarlara erişmek için panonun sağ üst köşesindeki dişli simgesini seçin.](./media/how-to-install-iot-edge-on-windows/select-gear-icon.png)

   1. **Ayarlar** menüsünde, **ağ geçidi** altında, **Uzantılar**' ı seçin.

   1. **Akışlar** sekmesini seçin ve **Ekle**' yi seçin.

   1. https://aka.ms/wac-insiders-feedMetin kutusuna girin ve **Ekle**' yi seçin.

   1. Akış eklendikten sonra **kullanılabilir uzantılar** sekmesine gidin. Uzantılar listesini güncelleştirmek biraz zaman alabilir.

   1. **Kullanılabilir uzantılar** sekmesinde, uzantılar listesinde **Azure IoT Edge** bulun. Bunu seçin ve uzantı listesinin üzerindeki **Install** Prompt ' ı seçin.

   1. Yükleme tamamlandıktan sonra **, yüklü Uzantılar sekmesinde yüklü** uzantılar listesinde Azure IoT Edge görmeniz gerekir.

## <a name="choose-your-provisioning-method"></a>Sağlama yönteminizi seçin

Windows üzerinde Linux için Azure IoT Edge aşağıdaki sağlama yöntemlerini destekler:

* IoT Edge cihazınızın bağlantı dizesini kullanarak el ile sağlama. Bu yöntemi kullanmak için cihazınızı kaydedin ve [IoT Hub IoT Edge cihazı kaydetme](how-to-register-device.md)bölümündeki adımları kullanarak bir bağlantı dizesi alın.
  * X. 509.440 otomatik olarak imzalanan sertifikalar, Windows üzerinde Linux için IoT Edge tarafından şu anda desteklenmediğinden simetrik anahtar kimlik doğrulaması seçeneğini belirleyin.
* Cihaz sağlama hizmeti (DPS) ve simetrik anahtarlar kullanılarak otomatik sağlama. [DPS ve simetrik anahtarlarla IoT Edge cihaz oluşturma ve sağlama](how-to-auto-provision-symmetric-keys.md)hakkında daha fazla bilgi edinin.
* DPS ve X. 509.440 sertifikaları kullanılarak otomatik sağlama. [DPS ve X. 509.440 sertifikalarına sahip bir IoT Edge cihaz oluşturma ve sağlama](how-to-auto-provision-x509-certs.md)hakkında daha fazla bilgi edinin.

El ile sağlama, birkaç cihaz kullanmaya başlamak daha kolay. Cihaz sağlama hizmeti, birçok cihaz sağlamak için yararlıdır.

Cihazınızı veya cihazlarınızı sağlamak için DPS yöntemlerinden birini kullanmayı planlıyorsanız, bir DPS örneği oluşturmak, DPS örneğinizi IoT Hub bağlamak ve bir DPS kaydı oluşturmak için yukarıda bağlanılan ilgili makaledeki adımları izleyin. Tek bir cihaz için tek bir *kayıt* veya bir cihaz grubu için bir *Grup kaydı* oluşturabilirsiniz. Kayıt türleri hakkında daha fazla bilgi için [Azure IoT Hub cihaz sağlama hizmeti kavramlarını](https://docs.microsoft.com/azure/iot-dps/concepts-service#enrollment)ziyaret edin.

## <a name="create-a-new-deployment"></a>Yeni bir dağıtım oluşturun

Hedef cihazınızda Windows üzerinde Linux için Azure IoT Edge dağıtımınızı oluşturun.

# <a name="windows-admin-center"></a>[Windows Yönetim Merkezi](#tab/windowsadmincenter)

Windows Yönetim Merkezi Başlangıç sayfasında, bağlantı listesi altında, Windows yönetici merkezini çalıştırdığınız BILGISAYARı temsil eden bir yerel ana bilgisayar bağlantısı görürsünüz. Yönettiğiniz tüm ek sunucular, bilgisayarlar veya kümeler burada da görünür.

Windows Yönetim Merkezi 'ni kullanarak yerel cihazınızdaki veya uzaktan yönetilen cihazlarınızda Windows üzerinde Linux için Azure IoT Edge yüklemek ve yönetmek üzere kullanabilirsiniz. Bu kılavuzda, yerel ana bilgisayar bağlantısı Windows üzerinde Linux için Azure IoT Edge dağıtımı için hedef cihaz olarak görev yapar.

Yerel cihazınız yerine uzak bir hedef cihaza dağıtmak istiyorsanız ve listede istediğiniz hedef cihazınızı görmüyorsanız, [cihazınızı eklemek için yönergeleri izleyin.](https://docs.microsoft.com/windows-server/manage/windows-admin-center/use/get-started#connecting-to-managed-nodes-and-clusters)

   ![Hedef cihaz listelenen ilk Windows Yönetim Merkezi panosu](./media/how-to-install-iot-edge-on-windows/windows-admin-center-initial-dashboard.png)

1. **Ekle**’yi seçin.

1. **Kaynak Ekle veya oluştur** bölmesinde **Azure IoT Edge** kutucuğunu bulun. Bir cihazda Windows üzerinde Linux için Azure IoT Edge yeni bir örneğini yüklemek için **Yeni oluştur** ' u seçin.

   Bir cihazda çalışan Windows üzerinde Linux için IoT Edge zaten varsa, **Ekle** ' yi seçerek var olan IoT Edge cihazına bağlanabilir ve Windows Yönetim Merkezi ile yönetebilirsiniz.

   ![Windows Yönetim Merkezi 'nde Azure IoT Edge kutucukta yeni oluştur seçeneğini belirleyin](./media/how-to-install-iot-edge-on-windows/resource-creation-tiles.png)

1. **Windows dağıtım üzerinde Linux için Azure IoT Edge oluşturma** bölmesi açılır. 1 üzerinde **. Başlarken** sekmesi, hedef cihazınızın en düşük gereksinimleri karşıladığını doğrulayın ve **İleri**' yi seçin.

1. Lisans koşullarını gözden geçirin, **kabul ediyorum**' u Işaretleyin ve **İleri**' yi seçin.

1. Tercihinize bağlı olarak, **Isteğe bağlı tanılama verilerini** açıp kapatabilirsiniz.

1. **İleri ' yi seçin: dağıt**.

   ![İsteğe bağlı tanılama verilerini tercihiniz ile değiştirdikten sonra Ileri: dağıt düğmesini seçin](./media/how-to-install-iot-edge-on-windows/select-next-deploy.png)

1. **2. Dağıtım** sekmesi, **hedef cihaz Seç** altında, listelenen cihaza tıklayarak, en düşük gereksinimleri karşıladığını doğrulayın. Durumu desteklendikten sonra, **İleri**' yi seçin.

   ![Desteklendiğini doğrulamak için cihazınızı seçin](./media/how-to-install-iot-edge-on-windows/evaluate-supported-device.png)

1. **2,2 ayarları** sekmesindeki varsayılan ayarları kabul edin.

1. **2,3 dağıtım** sekmesinde, dağıtımın ilerlemesini izleyebilirsiniz. Tam işlem, Windows paketinde Linux için Azure IoT Edge indirmeyi, paketi yüklemeyi, ana bilgisayar cihazını yapılandırmayı ve Linux sanal makinesini ayarlamayı içerir. Bu işlemin tamamlanması birkaç dakika sürebilir. Başarılı bir dağıtım aşağıda verilmiştir.

   ![Başarılı bir dağıtımda her bir adım yeşil onay işaretiyle ve ' tamamlandı ' etiketiyle gösterilir](./media/how-to-install-iot-edge-on-windows/successful-deployment.png)

Dağıtımınız tamamlandıktan sonra cihazınızı sağlamaya hazırlanın. **İleri ' yi seçin:** 3 ' e geçmek için Bağlan **.** Azure IoT Edge cihaz sağlamayı işleyen Bağlan sekmesi.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Henüz yoksa, Windows üzerinde Linux için IoT Edge 'i hedef cihazınıza yükleyebilirsiniz.

> [!NOTE]
> Aşağıdaki PowerShell işlemi, Windows 'da Linux için Azure IoT Edge Yerel bir konak dağıtımını oluşturmayı özetler. PowerShell kullanarak uzak hedef cihaza bir dağıtım oluşturmak için uzak bir cihazla bağlantı kurmak ve bu komutları bu cihazda uzaktan çalıştırmak için [uzak PowerShell](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote) 'i kullanabilirsiniz.

1. Yükseltilmiş bir PowerShell oturumunda, Windows 'da Linux için IoT Edge indirmek üzere aşağıdaki komutlardan her birini çalıştırın.

   ```azurepowershell-interactive
   $msiPath = $([io.Path]::Combine($env:TEMP, 'AzureIoTEdge.msi'))
   $ProgressPreference = 'SilentlyContinue'
   Invoke-WebRequest "https://aka.ms/AzureEdgeForLinuxOnWindowsMSI" -OutFile $msiPath
   ```

1. Cihazınıza Windows üzerinde Linux için IoT Edge yüklersiniz.

   ```azurepowershell-interactive
   Start-Process -Wait msiexec -ArgumentList "/i","$([io.Path]::Combine($env:TEMP, 'AzureIoTEdge.msi'))","/qn"
   ```

   > [!NOTE]
   > Yukarıdaki yükleme komutuna ıNSTALLDIR = "<FULLY_QUALIFIED_PATH>" ve VHDXDIR = "<FULLY_QUALIFIED_PATH>" parametrelerini ekleyerek Windows yüklemesinde ve VHDX dizinlerinde Linux için özel IoT Edge belirtebilirsiniz.

1. Dağıtımın başarıyla çalışması için, hedef cihazda Yürütme ilkesini, zaten yoksa olarak ayarlamanız gerekir `AllSigned` . Şu komutu kullanarak yükseltilmiş bir PowerShell isteminde geçerli yürütme ilkesini kontrol edebilirsiniz:

   ```azurepowershell-interactive
   Get-ExecutionPolicy -List
   ```

   Öğesinin yürütme ilkesi `local machine` değilse `AllSigned` , şunu kullanarak yürütme ilkesini ayarlayabilirsiniz:

   ```azurepowershell-interactive
   Set-ExecutionPolicy - ExecutionPolicy AllSigned -Force
   ```

1. Windows dağıtımında Linux için IoT Edge oluşturun.

   ```azurepowershell-interactive
   Deploy-Eflow
   ```

   <!-- Most likely temporary until cmdlet is fully documented -->
   > [!NOTE]
   > Bu komutu parametresiz veya isteğe bağlı olarak parametrelerle özelleştirmeniz için özelleştirebilirsiniz. Parametreleri ve bunların anlamını görmek için AzureEFLOW. psm1 PowerShell modülünü inceleyin (bkz. C:\Program Files\WindowsPowerShell\Modules\AzureEFLOW).

1. Lisans koşullarını kabul etmek için ' Y ' girin.

1. Tercihinize bağlı olarak, **Isteğe bağlı tanılama verilerini** açıp kapatmak Için ' O ' veya ' R ' girin. Başarılı bir dağıtım aşağıda verilmiştir.

   ![Başarılı bir dağıtım, iletilerin sonunda ' Deployment başarılı ' olarak söylecektir](./media/how-to-install-iot-edge-on-windows/successful-powershell-deployment.png)

Dağıtımınız tamamlandıktan sonra cihazınızı sağlamaya hazırlanın.

---

Cihazınızı sağlamak için, seçtiğiniz sağlama yönteminizin bölümüne gitmek üzere aşağıdaki bağlantıları takip edebilirsiniz:

* [Seçenek 1: IoT Edge cihazınızın bağlantı dizesini kullanarak el Ile sağlama](#option-1-provisioning-manually-using-the-connection-string)
* [Seçenek 2: cihaz sağlama hizmeti (DPS) ve simetrik anahtarlar kullanılarak otomatik sağlama](#option-2-provisioning-via-dps-using-symmetric-keys)
* [Seçenek 3: DPS ve X. 509.440 sertifikaları kullanılarak otomatik sağlama](#option-3-provisioning-via-dps-using-x509-certificates)

## <a name="provision-your-device"></a>Cihazınızı sağlama

Cihazınızı sağlamak için bir yöntem seçin ve ilgili bölümdeki yönergeleri izleyin. Cihazlarınızı sağlamak için Windows Yönetim merkezini veya yükseltilmiş bir PowerShell oturumu kullanabilirsiniz.

### <a name="option-1-provisioning-manually-using-the-connection-string"></a>Seçenek 1: bağlantı dizesini kullanarak el ile sağlama

Bu bölümde, Azure IoT Edge cihazınızın bağlantı dizesini kullanarak cihazınızın el ile sağlanması ele alınmaktadır.

# <a name="windows-admin-center"></a>[Windows Yönetim Merkezi](#tab/windowsadmincenter)

1. **Azure IoT Edge cihaz sağlama** bölmesinde, sağlama yöntemi açılır listesinden **bağlantı dizesi (el ile)** öğesini seçin.

1. [Azure Portal](https://ms.portal.azure.com/), IoT Hub **IoT Edge** sekmesine gidin.

1. Cihazınızın cihaz KIMLIĞINE tıklayın. **Birincil bağlantı dizesi** alanını kopyalayın.

1. Windows Yönetim Merkezi 'ndeki cihaz bağlantı dizesi alanına yapıştırın. Ardından, **Seçili yöntemle sağlamayı** seçin.

   ![Cihazınızın bağlantı dizesini yapıştırdıktan sonra seçili yöntemle sağlamayı seçin](./media/how-to-install-iot-edge-on-windows/provisioning-with-selected-method-connection-string.png)

1. Sağlama tamamlandıktan sonra **son**' u seçin. Ana panoya geri yönlendirilirsiniz. Şimdi, türü olan yeni bir cihaz listelendiğini görmeniz gerekir `IoT Edge Devices` . Bağlanılacak IoT Edge cihazı seçebilirsiniz. **Genel bakış** sayfasında, cihazınızın **IoT Edge modül listesini** ve **IoT Edge durumunu** görüntüleyebilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. [Azure Portal](https://ms.portal.azure.com/), IoT Hub **IoT Edge** sekmesine gidin.

1. Cihazınızın cihaz KIMLIĞINE tıklayın. **Birincil bağlantı dizesi** alanını kopyalayın.

1. Aşağıdaki komutta yer tutucu metnin üzerine yapıştırın ve hedef cihazınızda yükseltilmiş bir PowerShell oturumunda çalıştırın.

   ```azurepowershell-interactive
   Provision-EflowVm -provisioningType manual -devConnString "<CONNECTION_STRING_HERE>"
   ```

---

### <a name="option-2-provisioning-via-dps-using-symmetric-keys"></a>Seçenek 2: simetrik anahtarlar kullanarak DPS aracılığıyla sağlama

Bu bölümde, DPS ve simetrik anahtarlar kullanılarak cihazınızın otomatik olarak sağlanması ele alınmaktadır.

# <a name="windows-admin-center"></a>[Windows Yönetim Merkezi](#tab/windowsadmincenter)

1. **Azure IoT Edge cihaz sağlama** bölmesinde, sağlama yöntemi açılır listesinden **SIMETRIK anahtar (DPS)** öğesini seçin.

1. [Azure Portal](https://ms.portal.azure.com/), DPS örneğinize gidin.

1. **Genel bakış** sekmesinde, **kimlik kapsamı** değerini kopyalayın. Windows Yönetim Merkezi 'ndeki kapsam KIMLIĞI alanına yapıştırın.

1. Azure portal kayıtları **Yönet** sekmesinde, oluşturduğunuz kaydı seçin. Kayıt ayrıntılarında **birincil anahtar** değerini kopyalayın. Windows Yönetim merkezinde simetrik anahtar alanına yapıştırın.

1. Windows Yönetim Merkezi 'ndeki kayıt KIMLIĞI alanında cihazınızın kayıt KIMLIĞI ' ni belirtin.

1. **Seçili yöntemle sağlamayı** seçin.

   ![Simetrik anahtar sağlama için gerekli alanları doldurduktan sonra seçili yöntemle sağlamayı seçin](./media/how-to-install-iot-edge-on-windows/provisioning-with-selected-method-symmetric-key.png)

1. Sağlama tamamlandıktan sonra **son**' u seçin. Ana panoya geri yönlendirilirsiniz. Şimdi, türü olan yeni bir cihaz listelendiğini görmeniz gerekir `IoT Edge Devices` . Bağlanılacak IoT Edge cihazı seçebilirsiniz. **Genel bakış** sayfasında, cihazınızın **IoT Edge modül listesini** ve **IoT Edge durumunu** görüntüleyebilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. Aşağıdaki komutu bir metin düzenleyicisine kopyalayın. Yer tutucu metnini, bilgilerinizi ayrıntılı şekilde değiştirin.

   ```azurepowershell-interactive
   Provision-EflowVm -provisioningType symmetric -scopeId <ID_SCOPE_HERE> -registrationId <REGISTRATION_ID_HERE> -symmKey <PRIMARY_KEY_HERE>
   ```

1. [Azure Portal](https://ms.portal.azure.com/), DPS örneğinize gidin.

1. **Genel bakış** sekmesinde, **kimlik kapsamı** değerini kopyalayın. Komutuna uygun yer tutucu metnin üzerine yapıştırın.

1. Azure portal kayıtları **Yönet** sekmesinde, oluşturduğunuz kaydı seçin. Kayıt ayrıntılarında **birincil anahtar** değerini kopyalayın. Komutuna uygun yer tutucu metnin üzerine yapıştırın.

1. Komutta uygun yer tutucu metnin yerine geçecek olan cihazın kayıt KIMLIĞINI belirtin.

1. Komutu hedef cihazda yükseltilmiş bir PowerShell oturumunda çalıştırın.

---

### <a name="option-3-provisioning-via-dps-using-x509-certificates"></a>Seçenek 3: X. 509.440 sertifikalarını kullanarak DPS aracılığıyla sağlama

Bu bölümde, DPS ve X. 509.440 sertifikaları kullanılarak cihazınızın otomatik olarak sağlanması ele alınmaktadır.

# <a name="windows-admin-center"></a>[Windows Yönetim Merkezi](#tab/windowsadmincenter)

1. **Azure IoT Edge cihaz sağlama** bölmesinde, sağlama yöntemi açılır listesinden **X. 509.952 sertifikası (DPS)** öğesini seçin.

1. [Azure Portal](https://ms.portal.azure.com/), DPS örneğinize gidin.

1. **Genel bakış** sekmesinde, **kimlik kapsamı** değerini kopyalayın. Windows Yönetim Merkezi 'ndeki kapsam KIMLIĞI alanına yapıştırın.

1. Windows Yönetim Merkezi 'ndeki kayıt KIMLIĞI alanında cihazınızın kayıt KIMLIĞI ' ni belirtin.

1. Sertifikanızı ve özel anahtar dosyalarınızı karşıya yükleyin.

1. **Seçili yöntemle sağlamayı** seçin.

   ![X. 509.440 sertifikası sağlama için gerekli alanları doldurduktan sonra seçili yöntemle sağlamayı seçin](./media/how-to-install-iot-edge-on-windows/provisioning-with-selected-method-x509-certs.png)

1. Sağlama tamamlandıktan sonra **son**' u seçin. Ana panoya geri yönlendirilirsiniz. Şimdi, türü olan yeni bir cihaz listelendiğini görmeniz gerekir `IoT Edge Devices` . Bağlanılacak IoT Edge cihazı seçebilirsiniz. **Genel bakış** sayfasında, cihazınızın **IoT Edge modül listesini** ve **IoT Edge durumunu** görüntüleyebilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

1. Aşağıdaki komutu bir metin düzenleyicisine kopyalayın. Yer tutucu metnini, bilgilerinizi ayrıntılı şekilde değiştirin.

   ```azurepowershell-interactive
   Provision-EflowVm -provisioningType x509 -scopeId <ID_SCOPE_HERE> -registrationId <REGISTRATION_ID_HERE> -identityCertLocWin <ABSOLUTE_CERT_SOURCE_PATH_ON_WINDOWS_MACHINE> -identityPkLocWin <ABSOLUTE_PRIVATE_KEY_SOURCE_PATH_ON_WINDOWS_MACHINE> -identityCertLocWin <ABSOLUTE_CERT_DEST_PATH_ON_LINUX_MACHINE -identityPkLocVm <ABSOLUTE_PRIVATE_KEY_DEST_PATH_ON_LINUX_MACHINE>
   ```

1. [Azure Portal](https://ms.portal.azure.com/), DPS örneğinize gidin.

1. **Genel bakış** sekmesinde, **kimlik kapsamı** değerini kopyalayın. Komutuna uygun yer tutucu metnin üzerine yapıştırın.

1. Komutta uygun yer tutucu metnin yerine geçecek olan cihazın kayıt KIMLIĞINI belirtin.

1. Uygun yer tutucu metnini, sertifika dosyanızın mutlak kaynak yoluyla değiştirin.

1. Uygun yer tutucu metnini, özel anahtar dosyanızın mutlak kaynak yoluyla değiştirin.

1. Komutu hedef cihazda yükseltilmiş bir PowerShell oturumunda çalıştırın.

---

## <a name="verify-successful-configuration"></a>Başarılı yapılandırmayı doğrula

Windows üzerinde Linux için IoT Edge IoT Edge cihazınıza başarıyla yüklenip yapılandırıldığını doğrulayın.

1. Bağlanmak için Windows Yönetim Merkezi 'ndeki bağlı cihazlar listesinden IoT Edge cihazınızı seçin.
1. Cihaza Genel Bakış sayfasında cihazla ilgili bazı bilgiler görüntülenir:

    1. **IoT Edge modül listesi** bölümünde, cihazdaki çalışan modüller gösterilmektedir. IoT Edge hizmeti ilk kez başladığında yalnızca **Edgeagent** modülünü çalışır duruma gelmelidir. EdgeAgent modülü varsayılan olarak çalışır ve cihazınıza dağıttığınız ek modüllerin yüklenmesini ve başlamasını sağlar.
    1. **IoT Edge durumu** bölümü, hizmet durumunu gösterir ve raporlama **etkin (çalışıyor)** olmalıdır.

1. IoT Edge hizmetinde sorun gidermeniz gerekiyorsa, sanal makineye SSH (Secure Shell) için cihaz sayfasındaki **komut kabuğu** aracını kullanın ve Linux komutlarını çalıştırın.

    1. Hizmetle ilgili sorunları gidermeniz gerekirse hizmet günlüklerini alın.

       ```bash
       journalctl -u iotedge
       ```

    2. `check`Aygıtın yapılandırma ve bağlantı durumunu doğrulamak için aracını kullanın.

       ```bash
       sudo iotedge check
       ```

## <a name="next-steps"></a>Sonraki adımlar

Modülleri cihazınıza dağıtmayı öğrenmek için [IoT Edge modüllerini dağıtmaya](how-to-deploy-modules-portal.md) devam edin.
