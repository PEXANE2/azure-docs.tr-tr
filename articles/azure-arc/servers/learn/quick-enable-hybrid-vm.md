---
title: Azure Arc etkin sunucularıyla karma makineyi bağlama
description: Azure Arc etkin sunucularıyla karma makinenizi bağlamayı ve kaydetmeyi öğrenin.
ms.topic: quickstart
ms.date: 12/15/2020
ms.openlocfilehash: c52b8d1f7098a7a2a88a9770a3b768b7fea31775
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101646835"
---
# <a name="quickstart-connect-hybrid-machines-with-azure-arc-enabled-servers"></a>Hızlı başlangıç: Azure Arc etkin sunucularıyla karma makineleri bağlama

[Azure yay özellikli sunucular](../overview.md) , şirket içi, kenar ve çok yüksek ortamlarda barındırılan Windows ve Linux makinelerinizi yönetmenizi ve yönetmenizi sağlar. Bu hızlı başlangıçta, bağlı makine aracısını, Arc etkin sunucularıyla yönetim için Azure dışında barındırılan Windows veya Linux makinenizde dağıtıp yapılandıracaksınız.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Yay özellikli sunucular dağıtımı karma bağlı makine Aracısı, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerine sahip olmanızı gerektirir. Linux 'ta, kök hesabı ve Windows 'ta yerel Yöneticiler grubunun üyesi olan bir hesapla.

* Başlamadan önce, aracı [önkoşullarını](../agent-overview.md#prerequisites) gözden geçirdiğinizden emin olun ve aşağıdakileri doğrulayın:

    * Hedef makineniz desteklenen bir [işletim sistemi](../agent-overview.md#supported-operating-systems)çalıştırıyor.

    * Hesabınıza [gerekli Azure rollerine](../agent-overview.md#required-permissions)atama verildi.

    * Makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya ara sunucu üzerinden bağlanıyorsa, [listelenen](../agent-overview.md#networking-configuration) URL 'lerin engellenmediğinden emin olun.

    * Azure Arc etkin sunucuları yalnızca [burada](../overview.md#supported-regions)belirtilen bölgeleri destekler.

> [!WARNING]
> Linux hostname veya Windows bilgisayar adı, ad içindeki ayrılmış sözcüklerden veya ticari markalara ait olamaz, aksi halde bağlı makineyi Azure 'a kaydetmeye çalışmak başarısız olur. Ayrılmış sözcüklerin listesi için bkz. [ayrılmış kaynak adı hatalarını çözümleme](../../../azure-resource-manager/templates/error-reserved-resource-name.md) .

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

## <a name="register-azure-resource-providers"></a>Azure Kaynak sağlayıcılarını kaydetme

Azure Arc etkin sunucuları, bu hizmeti kullanabilmeniz için aboneliğinizde aşağıdaki Azure Kaynak sağlayıcılarına bağımlıdır:

* Microsoft. HybridCompute
* Microsoft. GuestConfiguration

Aşağıdaki komutları kullanarak bunları kaydedin:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

## <a name="generate-installation-script"></a>Yükleme betiği oluştur

Azure Arc ile indirme, yükleme ve bağlantı kurma işlemlerini otomatik hale getirmeye yönelik betik, Azure portal kullanılabilir. İşlemi gerçekleştirmek için aşağıdakileri yapın:

1. **Tüm hizmetler**' e tıklayıp sunucular ' ı arayıp seçin **-Azure arc**' ı Azure Portal Azure yay hizmetini başlatın.

    :::image type="content" source="./media/quick-enable-hybrid-vm/search-machines.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

1. **Sunucular-Azure yay** sayfasında sol üst köşedeki **Ekle** ' yi seçin.

1. **Bir yöntem seçin** sayfasında, **etkileşimli betiği kullanarak sunucu Ekle** ' yi seçin ve ardından **betik oluştur**' u seçin.

1. **Betik oluştur** sayfasında, makinenin Azure 'da yönetilmesini istediğiniz aboneliği ve kaynak grubunu seçin. Makine meta verilerinin depolanacağı Azure konumunu seçin. Bu konum, kaynak grubunun konumuyla aynı veya farklı olabilir.

1. **Önkoşullar** sayfasında, bilgileri gözden geçirin ve ardından **İleri: kaynak ayrıntıları**' nı seçin.

1. **Kaynak ayrıntıları** sayfasında, aşağıdakileri sağlayın:

    1. **Kaynak grubu** açılan listesinde, makinenin yönetilecek kaynak grubunu seçin.
    1. **Bölge** açılan listesinde, sunucu meta verilerini depolamak için Azure bölgesini seçin.
    1. **İşletim sistemi** açılan listesinde, betiğin üzerinde çalışmak üzere yapılandırıldığı işletim sistemini seçin.
    1. Makine Internet 'e bağlanmak için bir proxy sunucusu üzerinden iletişim kurduklarında, proxy sunucusu IP adresini veya makinenin proxy sunucusuyla iletişim kurmak için kullanacağı adı ve bağlantı noktası numarasını belirtin. Değeri biçiminde girin `http://<proxyURL>:<proxyport>` .
    1. **Sonraki: Etiketler**' i seçin.

1. **Etiketler** sayfasında, önerilen varsayılan **fiziksel konum etiketlerini** gözden geçirin ve bir değer girin veya standartlarınızı desteklemek için bir veya daha fazla **özel etiket** belirtin.

1. **İleri ' yi seçin: betiği indir ve Çalıştır**.

1. **Betiği indir ve Çalıştır** sayfasında, Özet bilgilerini gözden geçirin ve ardından **İndir**' i seçin. Hala değişiklik yapmanız gerekiyorsa, **önceki**' yi seçin.

## <a name="install-the-agent-using-the-script"></a>Betiği kullanarak aracıyı yükler

### <a name="windows-agent"></a>Windows aracısı

1. Sunucusunda oturum açın.

1. Yükseltilmiş bir 64 bit PowerShell komut istemi açın.

1. Betiği kopyaladığınız klasöre veya paylaşıma değiştirin ve betiği çalıştırarak sunucuda yürütün `./OnboardingScript.ps1` .

### <a name="linux-agent"></a>Linux Aracısı

1. Linux aracısını doğrudan Azure ile iletişim kurabilen hedef makineye yüklemek için şu komutu çalıştırın:

    ```bash
    bash ~/Install_linux_azcmagent.sh
    ```

    * Hedef makine bir ara sunucu üzerinden iletişim kuruyorsa, aşağıdaki komutu çalıştırın:

        ```bash
        bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
        ```

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. Makinenizde [Azure Portal](https://aka.ms/hybridmachineportal)görüntüleyin.

:::image type="content" source="./media/quick-enable-hybrid-vm/enabled-machine.png" alt-text="Başarılı bir makine bağlantısı" border="false":::

## <a name="next-steps"></a>Sonraki adımlar

Linux veya Windows hibrit makinenizi etkinleştirmiş ve hizmete başarıyla bağlandığınıza göre, Azure Ilkesini Azure 'da uyumluluğu anlamak üzere etkinleştirmeye hazırsınız.

Log Analytics aracısının yüklü olmadığı Azure Arc etkin sunucuların etkin makinesini nasıl tanımlayacağınızı öğrenmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [Uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma](tutorial-assign-policy-portal.md)
