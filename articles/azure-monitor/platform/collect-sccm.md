---
title: Configuration Manager'ı Azure Monitörüne Bağlayın | Microsoft Dokümanlar
description: Bu makalede, Configuration Manager'ı Azure Monitor'da çalışma alanına bağlama ve verileri çözümlemeye başlama adımları gösterilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/28/2019
ms.openlocfilehash: 3140c0de6fbe090e3d040202cd581c455f03b6d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655265"
---
# <a name="connect-configuration-manager-to-azure-monitor"></a>Configuration Manager'ı Azure Monitörüne Bağlayın
Microsoft Endpoint Configuration Manager ortamınızı Azure Monitor'a bağlayarak cihaz toplama verilerini senkronize edebilir ve bu koleksiyonlara Azure Monitor ve Azure Otomasyonu'nda başvuruyapabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

Azure Monitör, Configuration Manager geçerli dal, sürüm 1606 ve üzeri destekler.

>[!NOTE]
>Configuration Manager'ı Log Analytics çalışma alanına bağlama özelliği isteğe bağlıdır ve varsayılan olarak etkinleştirilemez. Kullanmadan önce bu özelliği etkinleştirmeniz gerekir. Daha fazla bilgi için, bkz. [Enable optional features from updates](https://docs.microsoft.com/configmgr/core/servers/manage/install-in-console-updates#bkmk_options).

## <a name="configuration-overview"></a>Yapılandırmaya genel bakış

Aşağıdaki adımlar, Azure Monitor ile Configuration Manager tümleştirmesini yapılandırma adımlarını özetleyin.  

1. Azure Etkin Dizin'de Configuration Manager'ı Bir Web Uygulaması ve/veya Web API uygulaması olarak kaydedin ve Azure Active Directory'den kayıttan istemci kimliği ve istemci gizli anahtarına sahip olduğunuzdan emin olun. Bu adımı nasıl gerçekleştirebileceğinizhakkında ayrıntılı bilgi için [kaynaklara erişebilen Active Directory uygulaması ve hizmet ilkesi oluşturmak](../../active-directory/develop/howto-create-service-principal-portal.md) için Portalı Kullan'a bakın.

2. Azure Etkin Dizin'de, [Azure Monitor'a erişim izni yle Configuration Manager'ı (kayıtlı web uygulaması) hibe edin.](#grant-configuration-manager-with-permissions-to-log-analytics)

3. Configuration Manager'da **Azure Hizmetleri** sihirbazını kullanarak bağlantı ekleyin.

4. Configuration Manager servis bağlantı noktası site sistem rolünü çalıştıran bilgisayara [Windows için Log Analytics aracısını indirin ve yükleyin.](#download-and-install-the-agent) Aracı, Configuration Manager verilerini Azure Monitor'daki Log Analytics çalışma alanına gönderir.

5. Azure Monitor'da, [koleksiyonlarını Configuration Manager'dan](#import-collections) bilgisayar grupları olarak aktarın.

6. Azure Monitor'da Configuration Manager'daki verileri [bilgisayar grupları](computer-groups.md)olarak görüntüleyin.

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>Log Analytics izinleri ile Configuration Manager'ı hibe edin

Aşağıdaki yordamda, Log Analytics çalışma alanınızdaki *Katılımcı* rolünü Configuration Manager için daha önce oluşturduğunuz AD uygulamasına ve hizmet yöneticisine verirsiniz. Zaten bir çalışma alanınız yoksa, devam etmeden önce [Azure Monitörü'nde bir çalışma alanı oluşturma'ya](../../azure-monitor/learn/quick-create-workspace.md) bakın. Bu, Configuration Manager'ın günlük analizi çalışma alanınızın kimliğini doğrulayıp bunlara bağlanmasına olanak tanır.  

> [!NOTE]
> Configuration Manager için Log Analytics çalışma alanında izinler belirtmeniz gerekir. Aksi takdirde, Configuration Manager'da yapılandırma sihirbazını kullandığınızda bir hata iletisi alırsınız.
>

1. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.

2. Günlük Analizi çalışma alanları listenizde, değiştirilecek çalışma alanını seçin.

3. Sol bölmeden Access **denetimi (IAM)** seçeneğini belirleyin.

4. Access denetimi (IAM) sayfasında rol **atamaekle'yi** tıklatın ve **rol atama ekle** bölmesi görüntülenir.

5. Rol **atama ekle** bölmesinde, **Rol** açılır listesinin altında **Katılımcı** rolünü seçin.  

6. Açılan listeye **erişim atama** altında, DAHA önce AD'de oluşturulan Configuration Manager uygulamasını seçin ve ardından **Tamam'ı**tıklatın.  

## <a name="download-and-install-the-agent"></a>Aracıyı indirin ve kurun

Configuration Manager hizmet bağlantı noktası sitesi sistem rolünü barındıran bilgisayara Windows için Log Analytics aracısını yüklemek için kullanılabilir yöntemleri anlamak için [Windows bilgisayarlarını Azure'da Azure Monitörüne bağlayın](agent-windows.md) makalesini gözden geçirin.  

## <a name="connect-configuration-manager-to-log-analytics-workspace"></a>Configuration Manager'ı Log Analytics çalışma alanına bağlayın

>[!NOTE]
> Log Analytics bağlantısı eklemek için Configuration Manager ortamınızın çevrimiçi mod için yapılandırılmış bir [servis bağlantı noktası](https://docs.microsoft.com/configmgr/core/servers/deploy/configure/about-the-service-connection-point) olması gerekir.

> [!NOTE]
> Hiyerarşinizdeki en üst düzey siteyi Azure Monitor'a bağlamanız gerekir. Bağımsız bir birincil siteyi Azure Monitor'a bağlar ve ardından ortamınıza merkezi bir yönetim sitesi eklerseniz, yeni hiyerarşi içindeki bağlantıyı silmeniz ve yeniden oluşturmanız gerekir.

1. Configuration Manager'ın **Yönetim** çalışma alanında **Buluthizmetleri'ni** seçin ve ardından **Azure Hizmetleri'ni**seçin. 

2. **Azure Hizmetleri'ni** sağ tıklatın ve ardından **Azure Hizmetlerini Yapılandır'ı**seçin. **Azure Hizmetlerini Yapılandırış** sayfası görüntülenir. 
   
3. **Genel** ekranda, aşağıdaki eylemleri yaptığınızı ve her öğe için ayrıntılara sahip olduğunuzu onaylayın ve **ardından İleri'yi**seçin.

4. Azure Hizmetleri Sihirbazı'nın Azure Hizmetleri sayfasında:

    1. Configuration Manager'da nesne için bir **Ad** belirtin.
    2. Hizmeti tanımlamanıza yardımcı olmak için isteğe bağlı bir **Açıklama** belirtin.
    3. Azure hizmeti **OMS Bağlayıcısı'nı**seçin.

    >[!NOTE]
    >OMS artık Azure Monitor'un bir özelliği olan Log Analytics olarak anılıyor.

5. Azure Hizmetleri Sihirbazı'nın Azure uygulama özellikleri sayfasına devam etmek için **İleri'yi** seçin.

6. Azure Hizmetleri Sihirbazı'nın **Uygulama** sayfasında, önce listeden Azure ortamını seçin ve sonra **İçe Aktar'ı**tıklatın.

7. Uygulamaları **İçe Aktar** sayfasında aşağıdaki bilgileri belirtin:

    1. Uygulama için **Azure AD Kiracı Adını** belirtin.

    2. Azure **AD Kiracı Kimliği** için Azure AD kiracısını belirtin. Bu bilgileri Azure Etkin Dizin **Özellikleri** sayfasında bulabilirsiniz. 

    3. Uygulama **Adı** için uygulama adını belirtin.

    4. Daha önce oluşturulan Azure AD uygulamasının Uygulama Kimliği olan **İstemci Kimliği**için belirtin.

    5. Oluşturulan Azure AD uygulamasının Istemci gizli anahtarı **olan Gizli anahtar**için belirtin.

    6. **Anahtarınızın**son kullanma tarihi olan Gizli Anahtar Son Kullanma Tarihi için belirtin.

    7. Daha önce oluşturulan Azure AD uygulamasının **App ID URI ,App ID URI**için belirtin.

    8. **Doğrula'yı** seçin ve sağda sonuçlar **Başarıyla doğrulanmış göstermelidir!**.

8. **Yapılandırma** sayfasında, **Azure aboneliklerini, Azure**kaynak **grubunu**ve İşlem Yönetimi Paketi çalışma **alanı** alanlarının Azure AD uygulamasının kaynak grubunda yeterli izine sahip olduğunu gösteren önceden doldurulan bilgileri doğrulamak için bilgileri gözden geçirin. Alanlar boşsa, başvurunuzun gerekli haklara sahip olmadığını gösterir. Toplamak ve çalışma alanına iletmek için aygıt koleksiyonlarını seçin ve sonra **Ekle'yi**seçin.

9. **Ayarları onayla** sayfasındaki seçenekleri gözden geçirin ve bağlantıyı oluşturmaya ve yapılandırmaya başlamak için **İleri'yi** seçin.

10. Yapılandırma tamamlandığında, **Tamamlama** sayfası görüntülenir. **Kapat**'ı seçin. 

Configuration Manager'ı Azure Monitor'a bağladıktan sonra koleksiyonek veya kaldırabilir ve bağlantının özelliklerini görüntüleyebilirsiniz.

## <a name="update-log-analytics-workspace-connection-properties"></a>Günlük Analizi çalışma alanı bağlantı özelliklerini güncelleştirme

Parola veya istemci gizli anahtarının süresi doluyorsa veya kaybolursa, Log Analytics bağlantı özelliklerini el ile güncelleştirmeniz gerekir.

1. Configuration Manager'ın **Yönetim** çalışma alanında **Bulut Hizmetleri'ni** seçin ve **ardından OMS Bağlantı Özellikleri** sayfasını açmak için **OMS Bağlayıcısı'nı** seçin.
2. Bu sayfada, **Kiracı,** **İstemci Kimliği**, **İstemci gizli anahtar sona erme tarihini**görüntülemek için **Azure Etkin Dizin** sekmesini tıklatın. Süresi **dolmuşsa İstemcinizin gizli anahtarını** **doğrulayın.**

## <a name="import-collections"></a>İthalat koleksiyonları

Configuration Manager'a bir Log Analytics bağlantısı ekledikten ve Configuration Manager servis bağlantı noktası site sistem rolünü çalıştıran bilgisayara aracıyı yükledikten sonra, bir sonraki adım Azure'da Configuration Manager'dan tahsilat almaktır Bilgisayar grupları olarak izleyin.

Hiyerarşinizden aygıt koleksiyonlarını almak için ilk yapılandırmayı tamamladıktan sonra, üyelik bilgilerini güncel tutmak için her 3 saatte bir koleksiyon bilgileri alınır. İstediğiniz zaman bunu devre dışı bırakmayı seçebilirsiniz.

1. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.
2. Günlük Analizi çalışma alanları listenizde, kayıtlı olan çalışma alanı Yapılandırma Yöneticisi'ni seçin.  
3. **Gelişmiş ayarlar**’ı seçin.
4. **Bilgisayar Grupları'nı** seçin ve ardından **SCCM'yi**seçin.  
5. **Yapılandırma Yöneticisi toplama üyeliklerini içe aktar'ı** seçin ve sonra **Kaydet'i**tıklatın.  
   
    ![Bilgisayar Grupları - SCCM sekmesi](./media/collect-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Configuration Manager'dan verileri görüntüleme

Configuration Manager'a bir Log Analytics bağlantısı ekledikten ve Configuration Manager servis bağlantı noktası site sistemini çalıştıran bilgisayara aracıyı yükledikten sonra, aracıdan gelen veriler Azure Monitor'daki Log Analytics çalışma alanına gönderilir. Azure Monitor'da Configuration Manager koleksiyonlarınız [bilgisayar grupları](../../azure-monitor/platform/computer-groups.md)olarak görünür. Grupları **Ayarlar\Bilgisayar Grupları**altında **Configuration Manager** sayfasından görüntüleyebilirsiniz.

Koleksiyonlar alındıktan sonra, koleksiyon üyelikleri olan kaç bilgisayarın algılandığını görebilirsiniz. Ayrıca, içe aktarılan koleksiyon sayısını da görebilirsiniz.

![Bilgisayar Grupları - SCCM sekmesi](./media/collect-sccm/sccm-computer-groups02.png)

Birini tıklattığınızda, günlük sorgu düzenleyicisi, tüm içe aktarılan grupları veya her gruba ait tüm bilgisayarları görüntüleyerek açılır. [Günlük Arama'yı](../../azure-monitor/log-query/log-query-overview.md)kullanarak, toplama üyelik verilerini daha ayrıntılı olarak analiz edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Configuration Manager verileriniz hakkında ayrıntılı bilgileri görüntülemek için [Günlük Arama'yı](../../azure-monitor/log-query/log-query-overview.md) kullanın.
