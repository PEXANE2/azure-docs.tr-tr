---
title: Azure FarmBeats'i yükleme
description: Bu makalede, Azure aboneliğinizde Azure FarmBeats nasıl yüklenir
author: usha-rathnavel
ms.topic: article
ms.date: 1/17/2020
ms.author: atinb
ms.openlocfilehash: 0761db6b73c6fcfeb1ef6fda729a68c9644bbc72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479568"
---
# <a name="install-azure-farmbeats"></a>Azure FarmBeats'i yükleme

Bu makalede, Azure aboneliğinizde Azure FarmBeats nasıl yüklenir.

Azure FarmBeats, Azure Marketi'nde kullanılabilen bir işletmeden işletmeye bir tekliftir. Tarım veri kümelerinin sağlayıcılar arasında toplanmasını ve işlem yapılabilir öngörüler oluşturmasını sağlar. Azure FarmBeats bunu, erimiş veri kümelerine dayalı yapay zeka (AI) veya makine öğrenimi (ML) modelleri oluşturmanıza olanak sağlayarak yapar. Azure FarmBeats'in iki ana bileşeni şunlardır:

- **Datahub**: Farklı sağlayıcılar arasında çeşitli tarım veri kümelerinin biraraya getirilmesini, normalleştirilmesini ve bağlamsallaştırılmasını sağlayan bir API katmanı.

- **Hızlandırıcı**: Datahub'ın üzerine inşa edilmiş web uygulaması. Model geliştirme ve görselleştirmenizi hızlandırArak başlatır. Hızlandırıcı, yutulan sensör verilerinin grafik olarak görselleştirilmesini ve model çıktısının harita olarak görselleştirilmesini göstermek için Azure FarmBeats API'lerini kullanır.

## <a name="general-information"></a>Genel bilgiler

### <a name="components-installed"></a>Bileşenler yüklü

Azure FarmBeats'i yüklediğinizde, Azure aboneliğinizde aşağıdaki kaynaklar sağlanmaktadır:

| Azure Kaynakları Yüklendi  | Azure FarmBeats bileşeni  |
|---------|---------|
| Application Insights   |      Datahub & Hızlandırıcı      |
| App Service     |     Datahub & Hızlandırıcı     |
| App Service Planı   | Datahub & Hızlandırıcı  |
| API Bağlantısı    |  Datahub       |
| Redis için Azure Önbelleği       | Datahub      |
| Azure Cosmos DB   |  Datahub       |
| Azure Data Factory V2       |     Datahub & Hızlandırıcı      |
| Azure Batch hesabı    | Datahub   |
| Azure Key Vault |  Datahub & Hızlandırıcı        |
| Azure Haritalar Hesabı       |     Hızlandırıcı    |
| Olay Hub Ad Alanı    |     Datahub      |
| Logic App      |  Datahub       |
| Depolama Hesabı      |     Datahub & Hızlandırıcı      |
| Time Series Insights     |    Datahub    |

### <a name="costs-incurred"></a>Tahakkuk eden maliyetler

Azure FarmBeats'in maliyeti, temel Azure hizmetlerinin maliyetinin toplamıdır. Azure hizmetleri için fiyatlandırma bilgileri [Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator)kullanılarak hesaplanabilir. Toplam yüklemenin fiili maliyeti kullanıma bağlı olarak değişir. İki bileşen için sabit durum maliyeti:

- Datahub - günde 10 DOLARDAN az
- Hızlandırıcı - günde 2 DOLARDAN az

### <a name="regions-supported"></a>Desteklenen bölgeler

Şu anda Azure FarmBeats aşağıdaki bölgelerdeki genel bulut ortamlarında desteklenir:

- Doğu Avustralya
- Orta ABD
- Doğu ABD
- Doğu ABD 2
- Batı ABD
- Batı ABD 2
- Kuzey Avrupa
- Batı Avrupa
- Doğu Asya
- Güneydoğu Asya

### <a name="time-taken"></a>Alınan süre

Hazırlama ve yükleme de dahil olmak üzere Azure FarmBeats'in tüm kurulumu bir saatten az sürer.

## <a name="prerequisites"></a>Ön koşullar

Azure FarmBeats'in gerçek yüklemesini başlatmadan önce aşağıdaki adımları tamamlamanız gerekir:

### <a name="verify-permissions"></a>İzinleri Doğrula

Azure FarmBeats'i yüklemek için Azure kiracısında aşağıdaki izinlere ihtiyacınız vardır:

- Kiracı - AAD uygulama yaratıcısı
- Abonelik - Sahibi
- FarmBeats'in kurulduğu Kaynak Grubu - Sahibi

[AAD uygulama adımını oluşturmak](#create-an-aad-application) için ilk iki izin gereklidir. Gerekirse, AAD uygulamasını oluşturmak için uygun izinlere sahip birini alabilirsiniz.

FarmBeats'i pazardan yükleyen kişinin FarmBeats'in yüklendiği Kaynak Grubu'nun sahibi olması gerekir. Abonelik sahipleri için kaynak grubu oluşturulduğunda bu otomatik olarak gerçekleşir. Diğerleri için lütfen Kaynak Grubu'nu önceden oluşturun ve Abonelik sahibinden sizi Kaynak Grubu'nun sahibi yapmasını isteyin.

Azure portalındaki erişim [izinlerinizi, role dayalı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/check-access)yönergelerini izleyerek doğrulayabilirsiniz.

### <a name="decide-subscription-and-region"></a>Abonelme ve Bölgeye Karar Verin

Azure abonelik kimliğine ve Azure FarmBeats'i yüklemek istediğiniz bölgeye ihtiyacınız vardır. [Bölgeler desteklenen](#regions-supported) bölümünde listelenen bölgelerden birini seçin.

**Azure Abonelik Kimliği** ve Azure **Bölgesi'ni**not edin.

### <a name="create-an-aad-application"></a>AAD uygulaması oluşturma

Azure FarmBeats için Azure Active Directory uygulaması oluşturulması ve kaydedilmesi gerekir. AAD oluşturma komut dosyasını başarıyla çalıştırmak için aşağıdaki izinlere gerek vardır:

- Kiracı - AAD uygulama yaratıcısı
- Abonelik - Sahibi

PowerShell ortamını kullanarak bulut kabuğu örneğinde aşağıdaki adımları çalıştırın. İlk kez kullanan kullanıcılardan bir abonelik seçmeleri ve bir depolama hesabı oluşturmaları istenir. Kurulum alıbını söylendiği gibi tamamlayın.

1. [AAD uygulama jeneratörkomut dosyasını](https://aka.ms/FarmBeatsAADScript) indirin

    ```azurepowershell-interactive
        wget -q https://aka.ms/FarmBeatsAADScript -O ./create_aad_script.ps1
    ```

2. Varsayılan olarak, dosya ev dizininize indirilir. Dizine gidin.

    ```azurepowershell-interactive
        cd
    ```

3. AAD komut dosyasını çalıştırma

    ```azurepowershell-interactive
        ./create_aad_script.ps1
    ```

4. Komut dosyası aşağıdaki üç girişi ister:

    - **FarmBeats Web Sitesi Adı**: Bu FarmBeats web uygulaması için benzersiz URL önekidir. Önek zaten alınmışsa, komut dosyası hata çıkarır. Kurulduktan sonra FarmBeats dağıtımınıza https://\<farmbeats-web sitesi adı>.azurewebsites.net adresinden erişebilirsiniz ve\<swagger API'leri farmbeats-web sitesi adı>-api.azurewebsites.net https:// olacaktır.

    - **Azure giriş kimliği**: FarmBeats yöneticisi olarak eklenmesini istediğiniz kullanıcı için Azure giriş kimliği sağlayın. Bu kullanıcı daha sonra diğer kullanıcılara FarmBeats web uygulaması erişim izni verebilir. Giriş kimliği genellikle formun john.doe@domain.com. Azure UPN de desteklenir.

    - **Abonelik Kimliği**: Azure FarmBeats'i yüklemek istediğiniz abonelik kimliğidir

5. AAD komut dosyasının çalıştırılması yaklaşık 2 dakika sürer ve ekrandaki değerlerin yanı sıra aynı dizindeki bir json dosyasına çıktı verir. Komut dosyasını başka biri çalıştırmışsa, bu çıktıyı sizinle paylaşmalarını isteyin.

### <a name="create-sentinel-account"></a>Sentinel hesabı oluşturma

Azure FarmBeats kurulumunuz, Avrupa Uzay Ajansı'nın [Sentinel-2](https://scihub.copernicus.eu/) uydu görevinden çiftliğiniz için uydu görüntüleri almanızı sağlar. Bu kurulumu yapılandırmak için bir Sentinel hesabı gerekir.

Sentinel ile ücretsiz hesap oluşturmak için aşağıdaki adımları izleyin:

1. Resmi [kayıt](https://aka.ms/SentinelRegistration) sayfasına gidin.
2. Gerekli ayrıntıları (ad, soyad, kullanıcı adı, parola ve e-posta kimliği) sağlayın ve formu doldurun.
3. Kayıtlı e-posta kimliğine doğrulama bağlantısı gönderilir. E-postada sağlanan bağlantıyı seçin ve doğrulamayı tamamlayın.

Kayıt işleminiz tamamlandı. Doğrulama tamamlandıktan sonra **Sentinel Kullanıcı Adınızı** ve Sentinel **Şifrenizi**not alın.

## <a name="install"></a>Yükleme

FarmBeats'i yüklemeye hazırsınız. Yüklemeyi başlatmak için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın. Sağ üst köşede hesabınızı seçin ve Azure FarmBeats'i yüklemek istediğiniz Azure AD kiracısına geçin.

2. Portal daki Azure Marketi'ne gidin ve Markette **Azure FarmBeats'i** arayın.

3. Azure FarmBeats'e genel bakış içeren yeni bir pencere görüntülenir. **Oluştur'u**seçin.

4. Yeni bir pencere görüntülenir. Azure FarmBeats'i yüklemek istediğiniz doğru aboneliği, kaynak grubunu ve konumu seçerek kaydolma işlemini tamamlayın.

5. **FarmBeats Hizmet Uyarıları** bölümünde Azure FarmBeats ile ilgili herhangi bir hizmet uyarısı alması gereken e-posta adresini sağlayın. **Bağımlılıklar** Sekmesine geçmek için sayfanın altındaki **İleri'yi** seçin.

    ![Temel Bilgiler Sekmesi](./media/install-azure-farmbeats/create-azure-farmbeats-basics.png)

6. [AAD komut dosyasının](#create-an-aad-application) çıktısından aad uygulama bölümündeki girişlere tek tek girişleri kopyalayın.

7. Sentinel [Hesabı](#create-sentinel-account) kullanıcı adını ve parolasını Sentinel Hesabı bölümüne girin. Gözden Geçir + **Oluştur** sekmesine taşımak için **İleri'yi** seçin.

    ![Bağımlılıklar Sekmesi](./media/install-azure-farmbeats/create-azure-farmbeats-dependencies.png)

8. Girilen ayrıntılar doğrulandıktan sonra **Tamam'ı**seçin. Kullanım Koşulları sayfası görüntülenir. Koşulları gözden geçirin ve yüklemeyi başlatmak için **Oluştur'u** seçin. Yükleme ilerlemesini takip edebilirsiniz sayfaya yönlendirilir.

Yükleme tamamlandıktan sonra, yükleme sırasında sağladığınız web sitesi adına giderek yüklemeyi doğrulayabilir ve\<FarmBeats portalını kullanmaya başlayabilirsiniz: farmbeats-web sitesi adı>.azurewebsites.net https://. Çiftlikler oluşturmak için bir seçenek ile FarmBeats kullanıcı arabirimini görmelisiniz.

**Datahub** https://\<FarmBeats-web sitesi-adı>-api.azurewebsites.net/swagger bulunabilir. Burada farklı FarmBeats API nesneleri göreceksiniz ve API'lerde REST işlemleri gerçekleştirmek.

## <a name="upgrade"></a>Yükseltme

FarmBeats'i en son sürüme yükseltmek için PowerShell ortamını kullanarak bulut kabuğu örneğinde aşağıdaki adımları çalıştırın. Kullanıcının FarmBeats'in yüklü olduğu aboneliğin sahibi olması gerekir.

İlk kez kullanan kullanıcılardan bir abonelik seçmeleri ve bir depolama hesabı oluşturmaları istenir. Kurulum alıbını söylendiği gibi tamamlayın.

1. Yükseltme [komut dosyasını indirin](https://aka.ms/FarmBeatsUpgradeScript)

    ```azurepowershell-interactive
        wget –q https://aka.ms/FarmBeatsUpgradeScript -O ./upgrade-farmbeats.ps1
    ```

2. Varsayılan olarak, dosya ev dizininize indirilir. Dizine gidin.

    ```azurepowershell-interactive
        cd
    ```

3. Yükseltme komut dosyasını çalıştırma

    ```azurepowershell-interactive
        ./upgrade-farmbeats.ps1 -InputFilePath [Path to input.json file]
    ```

input.json dosyasına giden yol isteğe bağlıdır. Belirtilmemişse, komut dosyası gerekli tüm girişleri ister. Yükseltme yaklaşık 30 dakika içinde bitmelidir.

## <a name="uninstall"></a>Kaldırma

Azure FarmBeats Datahub veya Accelerator'ı kaldırmak için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açın ve bu bileşenlerin yüklü olduğu **kaynak gruplarını silin.**

2. Azure FarmBeats yüklemesine bağlı **Azure AD uygulamasını silmek** & Azure Etkin Dizini'ne gidin.

## <a name="next-steps"></a>Sonraki adımlar

Azure aboneliğinizde Azure FarmBeats'i nasıl yükleyeceğimiz öğrenildi. Şimdi, Azure FarmBeats örneğinize nasıl [kullanıcı ekleyeceğinizi](manage-users-in-azure-farmbeats.md#manage-users) öğrenin.
