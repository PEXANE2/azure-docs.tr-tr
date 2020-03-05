---
author: tomarcher
ms.service: jenkins
ms.topic: include
ms.date: 03/03/2020
ms.author: tarcher
ms.openlocfilehash: 2468dc72881755a2990e8ddf8112d7fe27f64f4d
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274882"
---
## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği
* Bilgisayarınızın komut satırından (Bash kabuğu veya [PuTTY](https://www.putty.org/) gibi) SSH’ye erişin.

[!INCLUDE [quickstarts-free-trial-note](quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Çözüm şablonundan Jenkins VM’si oluşturma
Jenkins, tek bir Jenkins yüklemesinin çok sayıda projeyi barındırması veya derleme ya da test için gereken farklı ortamları sağlamak için Jenkins sunucusunun işi bir veya daha fazla aracıya devrettiği bir modeli destekler. Bu bölümdeki adımlar Azure'da Jenkins sunucusu yükleme ve yapılandırma aşamasında size yol gösterir.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-jenkins"></a>Jenkins’e bağlanma

Web tarayıcınızda sanal makinenize (örneğin, `http://jenkins2517454.eastus.cloudapp.azure.com/`) gidin. Jenkins konsoluna güvenli olmayan HTTP üzerinden erişilemeyeceğinden Jenkins konsoluna bilgisayarınızdan SSH tüneli kullanarak güvenli bir şekilde erişmek için yönergeler bu sayfada sağlanmıştır.

![Jenkins’in kilidini açma](./media/jenkins-install-solution-template-steps/jenkins-ssh-instructions.png)

Komut satırından `ssh` komutunu kullanarak tüneli ayarlayın. `username` değerini çözüm şablonundan sanal makineyi ayarlarken seçtiğiniz sanal makine yönetici kullanıcısının adıyla değiştirin.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Tüneli başlattıktan sonra, yerel makinenizde `http://localhost:8080/` adresine gidin. 

İlk parolayı Jenkins VM’ye SSH üzerinden bağlanırken bir komut satırında aşağıdaki komutu çalıştırarak alın.

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Bu ilk yönetici parolasıyla Jenkins panosunun kilidini ilk kez açın.

![Jenkins’in kilidini açma](./media/jenkins-install-solution-template-steps/jenkins-unlock.png)

Sonraki sayfada **Önerilen eklentileri yükle**’yi seçin ve Jenkins panosuna erişmek için kullanılacak bir Jenkins yönetici kullanıcısı oluşturun.

![Jenkins hazır!](./media/jenkins-install-solution-template-steps/jenkins-welcome.png)

Jenkins sunucusu artık kod oluşturmak için hazırdır.

## <a name="create-your-first-job"></a>İlk işinizi oluşturma

Jenkins konsolundan **Yeni iş oluştur**’u seçin, ardından işi **mySampleApp** olarak adlandırıp **Serbest tarzda proje**seçeneğini belirleyin ve **Tamam**’ı seçin.

![Yeni bir iş oluşturma](./media/jenkins-install-solution-template-steps/jenkins-new-job.png) 

**Kaynak Kod Yönetimi** sekmesini seçin, **Git**’i etkinleştirin ve **Depo URL'si** alanına aşağıdaki URL'yi girin :`https://github.com/spring-guides/gs-spring-boot.git`

![Git deposunu tanımlayın](./media/jenkins-install-solution-template-steps/jenkins-job-git-configuration.png) 

**Yapı** sekmesini ve ardından **Yapı ekleme adımı**’nı ve **Gradle betiğini başlat**’ı seçin. **Gradle sarmalayıcıyı kullan**’ı seçin, ardından `complete`Sarmalayıcı konumu **’na**  değerini, `build`Görevler**için** değerini girin.

![Derlemek için Gradle sarmalayıcıyı kullanma](./media/jenkins-install-solution-template-steps/jenkins-job-gradle-config.png) 

**Advanced**'i (Gelişmiş) seçin ve `complete`Root Build script **(Kök Derleme betiği) alanına** değerini girin. **Kaydet**’i seçin.

![Gradle sarmalayıcısı derleme adımında Gelişmiş ayarları belirleme](./media/jenkins-install-solution-template-steps/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Kodu oluşturma

Kodu derlemek ve örnek uygulamayı paketlemek için **Şimdi Derle**’yi seçin. Yapınız tamamlandığında proje için **Çalışma alanı** bağlantısını seçin.

![Derlemeden JAR dosyasını almak için çalışma alanına göz atma](./media/jenkins-install-solution-template-steps/jenkins-access-workspace.png) 

Yapınızın başarılı olduğunu doğrulamak için `complete/build/libs` konumuna gidin ve `gs-spring-boot-0.1.0.jar` dosyasının olduğundan emin olun. Jenkins sunucunuz artık Azure’da projelerinizi derlemeye hazırdır.

## <a name="troubleshooting-the-jenkins-solution-template"></a>Jenkins çözüm şablonunu sorunlarını giderme

Jenkins çözüm şablonuyla ilgili hatalarla karşılaşırsanız [Jenkins GitHub deposunda](https://github.com/azure/jenkins/issues) sorun bildirin.

## <a name="next-steps"></a>Sonraki Adımlar

> [!div class="nextstepaction"]
> [Jenkins aracıları olarak Azure VM'leri ekleme](/azure/jenkins-azure-vm-agents)