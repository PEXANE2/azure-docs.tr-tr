---
title: Azure Percept bilinen sorunlar
description: Azure Percept bilinen sorunlar ve bunların geçici çözümleri hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: reference
ms.date: 03/03/2021
ms.openlocfilehash: a04e53c8444a01bc42f3ce71393fc842f3419e74
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102193539"
---
# <a name="known-issues"></a>Bilinen sorunlar

Bu sorunlardan biriyle karşılaşırsanız, bir hatayı açmak gerekli değildir. Geçici çözümlerden herhangi biriyle ilgili sorun yaşıyorsanız lütfen bir sorun açın.

|Alan|Sorun açıklaması|Geçici çözüm|
|-------|---------|---------|
| Taslak deneyimi | Cihazın Wi-Fi yapılandırılmadığı müddetçe taslak deneyimi tamamlayamıyor (Azure oturum açma başarısız olur). | 1. SSH ile cihaz erişim noktası (10.1.1.1) <br> 2. cihaz Ethernet IP adresini tanımla ve Kopyala <br> 3. kopyalanmış Ethernet IP tabanlı URL 'YI kullanarak taslak deneyimle bağlantı |
| Taslak deneyimi | Şirket içi deneyim sırasında EULA 'daki bağlantılara tıklanması bazen yeni bir Web sayfası açmaz. | Bağlantıyı kopyalayın ve ayrı bir tarayıcı penceresinde açın. |
| Taslak deneyimi | Bir mobil Wi-Fi etkin noktaya bağlanıldığında taslak deneyimle çalışılamıyor. | Cihazınızı doğrudan SoftAP 'a, bir Wi-Fi ağa veya Ethernet üzerinden bir ağa bağlayın. |
| Wi-Fi/SoftAP | SoftAP bazen bağlantıyı kesebilir veya kaybolur. | Araştırıyoruz.  Cihazın yeniden başlatılması genellikle geri alınacaktır. |
| Wi-Fi | Wi-Fi SoftAP açık ve kapalı olarak geçiş yapan donanım düğmesi bazen çalışmaz. | Düğmeye basmayı veya cihazı yeniden başlatmayı denemeye devam edin. |
| Wi-Fi | Kullanıcılar Wi-Fi bağlandıktan sonra bir ileti görebilir ve bu Wi-Fi ağ daha eski bir güvenlik standardı kullanır. " | Devkit 'in Hotspot/SoftAP, WEP şifreleme algoritmasını kullanır. |
| Wi-Fi | Windows 10 PC 'den SoftAP 'a şu hata iletisiyle bağlantı kurulamadı: <br> "Bu ağa bağlanılamıyor" | Hem devkit hem de bilgisayarı yeniden başlatın. |
| Cihaz güncelleştirme | Kapsayıcılar bir OTA güncelleştirmesinden sonra çalışmaz. | Cihaza SSH ekleyin ve IoT Edge kapsayıcısını bu komutla yeniden başlatın `systemctl restart iotedge` . Bu, tüm kapsayıcıları yeniden başlatacak. |
| Cihaz güncelleştirme | Kullanıcılar, başarılı olsa bile güncelleştirmenin başarısız olduğunu belirten bir ileti alabilir. | IoT Hub cihaz için cihaz Ikizi gezinerek, cihazın güncelleştirildiğini onaylayın. Bu, ilk güncelleştirmeden sonra düzeltilir. |
| Cihaz güncelleştirme | Kullanıcılar, ilk güncelleştirmesinden sonra Wi-Fi bağlantı ayarlarını kaybedebilir. | Wi-Fi bağlantısını kurmak için güncelleştirdikten sonra taslak deneyimini çalıştırın. Bu, ilk güncelleştirmeden sonra düzeltilir. |
| Cihaz güncelleştirme | OTA güncelleştirmesini gerçekleştirdikten sonra, kullanıcılar daha önce oluşturulan kullanıcı hesaplarını kullanarak SSH aracılığıyla oturum açabilirler ve yeni SSH kullanıcıları taslak deneyimi aracılığıyla oluşturulamaz. Bu sorun, aşağıdaki önceden yüklenmiş görüntü sürümlerinden OTA güncelleştirmeleri gerçekleştiren sistemleri etkiler: 2020.110.114.105 ve 2020.109.101.105. | Kullanıcı profillerinizi kurtarmak için, OTA güncelleştirmesinden sonra şu adımları uygulayın: <br> Kullanıcı adı olarak "root" kullanarak [devkit 'e SSH](./how-to-ssh-into-percept-dk.md) . SSH "root" Kullanıcı oturum açma bilgilerini taslak deneyimle devre dışı bırakırsanız, yeniden etkinleştirmeniz gerekir. Başarıyla bağlandıktan sonra bu komutu Çalıştır: <br> ```mkdir -p /var/custom-configs/home; chmod 755 /var/custom-configs/home``` <br> Önceki Kullanıcı giriş verilerini kurtarmak için aşağıdaki komutu çalıştırın: <br> ```mkdir -p /tmp/prev-rootfs && mount /dev/mmcblk0p3 /tmp/prev-rootfs && [ ! -L /tmp/prev-rootfs/home ] && cp -a /tmp/prev-rootfs/home/* /var/custom-configs/home/. && echo "User home migrated!"; umount /tmp/prev-rootfs``` |
| Cihaz güncelleştirme | OTA güncelleştirmesini gerçekleştirdikten sonra, güncelleştirme grupları kaybedilir. | [Bu yönergeleri](https://docs.microsoft.com/azure/azure-percept/how-to-update-over-the-air#create-a-device-update-group)izleyerek cihazın etiketini güncelleştirin. |
| Geliştirme araçları paketi yükleyicisi | Docker sistemde düzgün çalışmıyorsa isteğe bağlı Caffe yüklemesi başarısız olabilir. | Docker 'ın yüklü olduğundan ve çalıştığından emin olun ve ardından Caffe yüklemesini yeniden deneyin. |
| Geliştirme araçları paketi yükleyicisi | İsteğe bağlı CUDA yüklemesi uyumsuz sistemlerde başarısız olur. | Yükleyiciyi çalıştırmadan önce CUDA ile sistem uyumluluğunu doğrulayın. |
| Docker, ağ, IoT Edge | İç ağınız 172. x. x. x kullanıyorsa Docker Kapsayıcıları Edge 'e bağlanamaz. | Aşağıdaki gibi,/etc/Docker/daemon.jsdosyasına özel bir bip bölümü ekleyin: `{    "bip": "192.168.168.1/24"}` |
|Azure Percept Studio | Azure Percept Studio 'daki "akışı görüntüleme" bağlantıları, cihazın Web akışını gösteren yeni bir pencere açmayın. | 1. [Azure Portal](https://portal.azure.com) açın ve **IoT Hub**' ı seçin. <br> 2. cihazınızın bağlı olduğu IoT Hub tıklayın. <br> 3. IoT Hub sayfanızda **otomatik cihaz yönetimi** altında **IoT Edge** seçin. <br> 4. listeden cihazınızı seçin. <br> 5. cihaz sayfanızın en üstündeki **modülleri ayarla** ' yı seçin. <br> 6. modülünü silmek için **Hostipmodule** ' un yanındaki trakıcan simgesine tıklayın. <br> 7. eylemi onaylamak için, **gözden geçir + oluştur** ' a ve ardından **Oluştur**' a tıklayın. <br> 8. [Azure Percept Studio 'yu](https://go.microsoft.com/fwlink/?linkid=2135819) açın ve sol taraftaki menü panelinde **cihazlar** ' a tıklayın. <br> 9. listeden cihazınızı seçin. <br> 10. **vizyon** sekmesinde **cihaz akışınızı görüntüleyin**' e tıklayın. Cihazınız, barındırma tipl modülünün yeni bir sürümünü indirir ve cihazınızın web akışıyla bir tarayıcı sekmesi açar. |