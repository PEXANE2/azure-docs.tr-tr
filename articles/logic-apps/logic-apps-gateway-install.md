---
title: Şirket içi veri ağ geçidini yükleme
description: Azure Logic Apps'tan şirket içi verilere erişmeden önce şirket içi veri ağ geçidini indirin ve yükleyin
services: logic-apps
ms.suite: integration
ms.reviewer: arthii, logicappspm
ms.topic: article
ms.date: 12/05/2019
ms.openlocfilehash: 797cd82327d68003d4e5f007d1f16e9534092ac0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283998"
---
# <a name="install-on-premises-data-gateway-for-azure-logic-apps"></a>Azure Logic Apps için şirket içi veri ağ geçidi yükleme

[Azure Logic Apps'tan şirket içi veri kaynaklarına bağlanmadan](../logic-apps/logic-apps-gateway-connection.md) [önce, şirket içi veri ağ geçidini](https://aka.ms/on-premises-data-gateway-installer) yerel bir bilgisayara indirin ve yükleyin. Ağ geçidi, şirket içi veri kaynakları ile mantık uygulamalarınız arasında hızlı veri aktarımı ve şifreleme sağlayan bir köprü olarak çalışır. Power BI, Power Automate, Power Apps ve Azure Analiz Hizmetleri gibi diğer bulut hizmetleriyle aynı ağ geçidi yüklemesini kullanabilirsiniz. Bu hizmetlerle ağ geçidinin nasıl kullanılacağı hakkında bilgi için aşağıdaki makalelere bakın:

* [Microsoft Power Şirket içi veri ağ geçidini otomatikleştir](/power-automate/gateway-reference)
* [Microsoft Power BI şirket içi veri ağ geçidi](/power-bi/service-gateway-onprem)
* [Microsoft Power Apps şirket içi veri ağ geçidi](/powerapps/maker/canvas-apps/gateway-reference)
* [Azure Analiz Hizmetleri şirket içi veri ağ geçidi](../analysis-services/analysis-services-gateway.md)

Bu makalede, Azure Logic Apps'tan şirket içi veri kaynaklarına erişebilmeniz için şirket içi veri ağ geçidinizi nasıl karşıdan yükleyebileceğiniz, yükleyebileceğiniz ve ayarlaabileceğiniz gösterilmektedir. Bu konuda veri [ağ geçidinin nasıl çalıştığı](#gateway-cloud-service) hakkında daha fazla bilgi edinebilirsiniz. Ağ geçidi hakkında daha fazla bilgi için, şirket [içi ağ geçidi nedir?](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem)

<a name="requirements"></a>

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabı ve aboneliği Aboneliği olan bir Azure hesabınız yoksa, [ücretsiz bir Azure hesabı için kaydolun.](https://azure.microsoft.com/free/)

  * Azure hesabınız tek bir [Azure Etkin Dizin (Azure AD) kiracısına veya dizinine](../active-directory/fundamentals/active-directory-whatis.md#terminology)ait olmalıdır. Ağ geçidini yerel bilgisayarınızda yüklemek ve yönetmek için aynı Azure hesabını kullanmanız gerekir.

  * Ağ geçidi yüklemesi sırasında, ağ geçidi yüklemenizi Azure hesabınıza ve yalnızca bu hesaba bağlayan Azure hesabınızla oturum açarsınız. Daha sonra, Azure portalında, ağ geçidi yüklemenizi kaydeden ve talep eden bir Azure ağ geçidi kaynağı oluştururken aynı Azure hesabını ve Azure AD kiracısını kullanmanız gerekir. Azure Logic Apps'ta şirket içi tetikleyiciler ve eylemler, şirket içi veri kaynaklarına bağlanmak için ağ geçidi kaynağını kullanır.

    > [!NOTE]
    > Yalnızca bir ağ geçidi yüklemesini ve bir Azure ağ geçidi kaynağını birbirine bağlayabilirsiniz. Aynı ağ geçidi yüklemesini birden çok Azure hesabına veya Azure ağ geçidi kaynaklarına bağlayamamaktadır. Ancak, bir Azure hesabı birden çok ağ geçidi yüklemesine ve Azure ağ geçidi kaynaklarına bağlanabilir. Şirket içi bir tetikleyici veya eylemde, çeşitli Azure aboneliklerinizden seçim yapabilir ve ardından ilişkili bir ağ geçidi kaynağı seçebilirsiniz.

  * Kuruluş *hesabı* olarak da bilinen ve 'ye benzer `username@contoso.com`bir iş hesabı veya okul hesabıyla oturum açmanız gerekir. Azure B2B (konuk) hesaplarını veya kişisel Microsoft hesaplarını @hotmail.com @outlook.comkullanamazsınız.

    > [!TIP]
    > Office 365 teklifine kaydolduysanız ve iş e-posta adresinizi vermediyseniz, adresiniz şu şekilde `username@domain.onmicrosoft.com`görünebilir. Hesabınız bir Azure Etkin Dizini'nde (Azure AD) kiracıda depolanır. Çoğu durumda, Azure REKLAM hesabınızdaki Kullanıcı Adı (UPN), e-posta adresinizle aynıdır.
    >
    > Microsoft hesabına bağlı bir [Visual Studio Standart aboneliği](https://visualstudio.microsoft.com/vs/pricing/) ni kullanmak için önce Azure [AD'de bir kiracı oluşturun](../active-directory/develop/quickstart-create-new-tenant.md) veya varsayılan dizini kullanın. Dizine parolası olan bir kullanıcı ekleyin ve ardından bu kullanıcıya Azure aboneliğinize erişim hakkı verin. Daha sonra bu kullanıcı adı ve parola ile ağ geçidi yükleme sırasında oturum açabilirsiniz.

* Yerel bilgisayarınız için gereksinimler şunlardır:

  **Minimum gereksinimler**

  *  .NET Framework 4.7.2
  * Windows 7 veya Windows Server 2008 R2 64 bit sürümü (ya da sonraki sürümler)

  **Önerilen gereksinimler**

  * 8 çekirdekli Işlemci
  * 8 GB bellek
  * Windows Server 2012 R2 veya sonrası 64 bit sürümü
  * Biriktirme için katı hal sürücü (SSD) depolama

  > [!NOTE]
  > Ağ geçidi Windows Server Core'u desteklemez.

* **İlgili hususlar**

  * Şirket içi veri ağ geçidini etki alanı denetleyicisini değil, yalnızca yerel bir bilgisayara yükleyin. Ağ geçidini veri kaynağınızla aynı bilgisayara yüklemeniz gerekmez. Tüm veri kaynaklarınız için yalnızca bir ağ geçidine ihtiyacınız varsa, bu nedenle her veri kaynağı için ağ geçidini yüklemeniz gerekmez.

    > [!TIP]
    > Gecikme yi en aza indirmek için, izinleriniz olduğunu varsayarak ağ geçidini veri kaynağınıza veya aynı bilgisayara mümkün olduğunca yakın olarak yükleyebilirsiniz.

  * Ağ üzerinde, internete bağlı, her zaman açık olan ve uyku moduna gitmeyen bir bilgisayara ağ geçidini yükleyin. Aksi takdirde, ağ geçidi çalıştırılmaz ve kablosuz ağ üzerinden performans zarar görebilir.

  * Windows kimlik doğrulamasını kullanmayı planlıyorsanız, ağ geçidini veri kaynaklarınızile aynı Etkin Dizin ortamının üyesi olan bir bilgisayara yüklediğinizden emin olun.

  * Ağ geçidi yüklemeniz için seçtiğiniz bölge, mantık uygulamanız için azure ağ geçidi kaynağını daha sonra oluşturduğunuzda seçmeniz gereken konumla aynıdır. Varsayılan olarak, bu bölge Azure hesabınızı yöneten Azure AD kiracınızla aynı konumdur. Ancak, ağ geçidi yükleme sırasında konumu değiştirebilirsiniz.

  * Ağ geçidi yüklemenizi en son sürüme güncelliyorsanız, daha temiz bir deneyim için önce geçerli ağ geçidinizi kaldırın.

  * Ağ geçidinin iki modu vardır: standart mod ve yalnızca Power BI için geçerli olan kişisel mod. Aynı bilgisayarda aynı modda birden fazla ağ geçidi çalıştıramaz.

  * Azure Logic Apps ağ geçidi üzerinden okuma ve yazma işlemlerini destekler. Ancak, bu [işlemlerin yük boyutu nda sınırlamaları](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations)vardır.

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Veri ağ geçidi yükleme

1. Ağ [geçidi yükleyicisini yerel bir bilgisayarda indirin ve çalıştırın.](https://aka.ms/on-premises-data-gateway-installer)

1. Minimum gereksinimleri gözden geçirin, varsayılan yükleme yolunu tutun, kullanım koşullarını kabul edin ve sonra **Yükle'yi**seçin.

   ![Gereksinimleri gözden geçirin ve kullanım koşullarını kabul edin](./media/logic-apps-gateway-install/review-and-accept-terms-of-use.png)

1. Ağ geçidi başarıyla yüklendikten sonra Azure hesabınızın e-posta adresini girin ve ardından **Oturum Aç'ı**seçin:

   ![İş veya okul hesabıyla oturum açın](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

   Ağ geçidi yüklemeniz yalnızca bir Azure hesabına bağlanabilir.

1. Bu >  **bilgisayarda yeni bir ağ geçidi kaydol****Sonraki'yi**seçin. Bu adım ağ geçidi yüklemenizi [ağ geçidi bulut hizmetine](#gateway-cloud-service)kaydeder.

   ![Ağ geçidini yerel bilgisayarda kaydetme](./media/logic-apps-gateway-install/register-gateway-local-computer.png)

1. Ağ geçidi yüklemeniz için bu bilgileri sağlayın:

   * Azure AD kiracınızda benzersiz bir ağ geçidi adı
   * Kullanmak istediğiniz en az sekiz karaktere sahip olması gereken kurtarma anahtarı
   * Kurtarma anahtarınızın onayı

   ![Ağ geçidi yüklemesi için bilgi sağlama](./media/logic-apps-gateway-install/gateway-name-recovery-key.png)

   > [!IMPORTANT]
   > Kurtarma anahtarınızı güvenli bir yerde kaydedin ve saklayın. Konumu değiştirmek, taşımak, kurtarmak veya bir ağ geçidi yüklemesini devralmak istiyorsanız bu anahtara ihtiyacınız var.

   [Yüksek kullanılabilirlik senaryoları](#high-availability)için ek ağ geçitleri yüklediğinizde seçtiğiniz **varolan bir ağ geçidi kümesine ekleme**seçeneğini not edin.

1. Ağ geçidi bulut hizmeti ve ağ geçidi yüklemeniz tarafından kullanılan [Azure Hizmet Veri Servisi](https://azure.microsoft.com/services/service-bus/) için bölgeyi denetleyin. Varsayılan olarak, bu bölge Azure hesabınız için Azure AD kiracıile aynı konumdur.

   ![Ağ geçidi hizmeti ve servis veri neşrisi için bölgeyi onaylama](./media/logic-apps-gateway-install/confirm-gateway-region.png)

1. Varsayılan bölgeyi kabul etmek için **Yapıl'ı**seçin. Ancak, varsayılan bölge size en yakın bölge değilse, bölgeyi değiştirebilirsiniz.

   *Ağ geçidi yüklemeniz için neden bölgeyi değiştirin?*

   Örneğin, gecikme süresini azaltmak için ağ geçidinizin bölgesini mantık uygulamanızla aynı bölgeye değiştirebilirsiniz. Veya şirket içi veri kaynağınıza en yakın bölgeyi seçebilirsiniz. *Azure'daki ağ geçidi kaynağınız* ve mantık uygulamanız farklı konumlara sahip olabilir.

   1. Geçerli bölgenin **yanında, Bölge Değiştir'i**seçin.

      ![Geçerli ağ geçidi bölgesini değiştirme](./media/logic-apps-gateway-install/change-gateway-service-region.png)

   1. Bir sonraki **sayfada, Bölge Seç** listesini açın, istediğiniz bölgeyi seçin ve **Bitti'yi**seçin.

      ![Ağ geçidi hizmeti için başka bir bölge seçin](./media/logic-apps-gateway-install/select-region-gateway-install.png)

1. Son onay penceresindeki bilgileri gözden geçirin. Bu örnek, Logic Apps, Power BI, Power Apps ve Power Automate için aynı hesabı kullanır, böylece ağ geçidi tüm bu hizmetler için kullanılabilir. Hazır olduğunuzda **Kapat'ı**seçin.

   ![Veri ağ geçidi bilgilerini onaylama](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

1. Şimdi [ağ geçidi yüklemeniz için Azure kaynağını oluşturun.](../logic-apps/logic-apps-gateway-connection.md)

## <a name="check-or-adjust-communication-settings"></a>İletişim ayarlarını denetleme veya ayarlama

Şirket içi veri ağ geçidi, bulut bağlantısı için [Azure Hizmet Veri Servisi'ne](../service-bus-messaging/service-bus-messaging-overview.md) bağlıdır ve ağ geçidinin ilişkili Azure bölgesine karşılık gelen giden bağlantıları kurar. Çalışma ortamınız, internete erişmek için trafiğin proxy veya güvenlik duvarından geçmesini gerektiriyorsa, bu kısıtlama şirket içi veri ağ geçidinin ağ geçidi bulut hizmetine ve Azure Hizmet Veri Yolunda'na bağlanmasını engelleyebilir. Ağ geçidinde ayarlayabileceğiniz birkaç iletişim ayarı vardır. Daha fazla bilgi için şu konulara bakın:

* [Şirket içi veri ağ geçidi için iletişim ayarlarını ayarlama](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication)
* [Şirket içi veri ağ geçidi için ara sunucu ayarlarını yapılandırma](https://docs.microsoft.com/data-integration/gateway/service-gateway-proxy)

<a name="high-availability"></a>

## <a name="high-availability-support"></a>Yüksek kullanılabilirlik desteği

Şirket içi veri erişimi için tek bir hata noktasını önlemek için, her biri farklı bir bilgisayarda birden çok ağ geçidi yüklemesine (yalnızca standart mod) sahip olabilir ve bunları küme veya grup olarak ayarlayabilirsiniz. Bu şekilde, birincil ağ geçidi kullanılamıyorsa, veri istekleri ikinci ağ geçidine yönlendirilir ve böyle devam edin. Bir bilgisayara yalnızca bir standart ağ geçidi yükleyebildiğinizden, kümedeki her ek ağ geçidini farklı bir bilgisayara yüklemeniz gerekir. Şirket içi veri ağ geçidiyle çalışan tüm bağlayıcılar yüksek kullanılabilirliği destekler.

* Birincil ağ geçidi ve bu yükleme için kurtarma anahtarıyla aynı Azure hesabına sahip en az bir ağ geçidi yüklemeniz olması gerekir.

* Birincil ağ geçidiniz Kasım 2017 veya daha sonraki ağ geçidi güncelleştirmesini çalıştırıyor olmalıdır.

Birincil ağ geçidinizi ayarladıktan sonra, başka bir ağ geçidi yüklemeye gittiğinizde, **varolan bir ağ geçidi kümesine**ekle'yi, yüklediğiniz ilk ağ geçidi olan birincil ağ geçidini seçin ve bu ağ geçidi için kurtarma anahtarını sağlayın. Daha fazla bilgi [için, şirket içi veri ağ geçidi için Yüksek kullanılabilirlik kümelerine](https://docs.microsoft.com/data-integration/gateway/service-gateway-install#add-another-gateway-to-create-a-cluster)bakın.

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>Konumu değiştirme, geçiş, geri yükleme veya varolan ağ geçidini devralma

Ağ geçidinizin konumunu değiştirmeniz, ağ geçidi yüklemenizi yeni bir bilgisayara taşımanız, bozuk bir ağ geçidikurtarmanız veya varolan bir ağ geçidiiçin sahiplenme niz gerekiyorsa, ağ geçidi yükleme sırasında sağlanan kurtarma anahtarına ihtiyacınız vardır.

1. Varolan ağ geçidine sahip bilgisayarda ağ geçidi yükleyicisini çalıştırın. En son ağ geçidi yükleyiciniz yoksa, [en son ağ geçidi sürümünü indirin.](https://aka.ms/on-premises-data-gateway-installer)

   > [!NOTE]
   > Özgün ağ geçidi yüklemesi olan bilgisayardaki ağ geçidini geri yüklemeden önce, önce bu bilgisayardaki ağ geçidini kaldırmanız gerekir. Bu eylem, özgün ağ geçidini keser.
   > Herhangi bir bulut hizmeti için bir ağ geçidi kümesini kaldırır veya silerseniz, bu kümeyi geri yükleyemezsiniz.

1. Yükleyici açıldıktan sonra, ağ geçidini yüklemek için kullanılan aynı Azure hesabıyla oturum açın.

1. **Varolan bir ağ geçidini** > Geçir, geri yükle veya devralma yı**seçin, örneğin:**

   !["Varolan bir ağ geçidini geçir, geri yükle veya devralma" seçeneğini belirleyin](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

1. Kullanılabilir kümeler ve ağ geçitleri arasından seçim yapmak ve örneğin seçili ağ geçidinin kurtarma anahtarını girin:

   ![Ağ geçidini seçin ve kurtarma anahtarı sağlayın](./media/logic-apps-gateway-install/select-existing-gateway.png)

1. Bölgeyi değiştirmek için **Bölge Değiştir'i**seçin ve yeni bölgeyi seçin.

1. Hazır olduğunuzda, görevinizi tamamlayabilmeniz için **Yapıya'yı** seçin.

## <a name="tenant-level-administration"></a>Kiracı düzeyinde yönetim

Bir Azure AD kiracısındaki tüm şirket içi veri ağ geçitlerinde görünürlük elde etmek için, bu kiracıdaki küresel yöneticiler [Power Platform Yöneticisi merkezinde](https://powerplatform.microsoft.com) kiracı yönetici olarak oturum açabilir ve Veri Ağ **Geçitleri** seçeneğini seçebilir. Daha fazla bilgi için, [şirket içi veri ağ geçidi için Kiracı düzeyinde yönetime](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin)bakın.

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Ağ geçidini yeniden başlatma

Varsayılan olarak, yerel bilgisayarınızdaki ağ geçidi yüklemesi "Şirket içi veri ağ geçidi hizmeti" adlı bir Windows hizmet hesabı olarak çalışır. Ancak, ağ geçidi `NT SERVICE\PBIEgwService` yüklemesi "Giriş" hesap kimlik bilgileri için adı kullanır ve "Hizmet olarak oturum aç" izinleri vardır.

> [!NOTE]
> Windows hizmet hesabınız, şirket içi veri kaynaklarına bağlanmak için kullanılan hesaptan ve bulut hizmetlerinde oturum açtığınızda kullandığınız Azure hesabından farklıdır.

Diğer Windows hizmetleri gibi, ağ geçidini çeşitli şekillerde başlatabilir ve durdurabilirsiniz. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/data-integration/gateway/service-gateway-restart)

<a name="gateway-cloud-service"></a>

## <a name="how-the-gateway-works"></a>Ağ geçidi nasıl çalışır?

Kuruluşunuzdaki kullanıcılar, zaten yetkili erişime sahip oldukları şirket içi verilere erişebilir. Ancak, bu kullanıcıların şirket içi veri kaynağınıza bağlanabilmeleri için şirket içi bir veri ağ geçidi yüklemeniz ve kurmanız gerekir. Genellikle, yönetici bir ağ geçidi yükleyen ve kuran kişidir. Bu eylemler, Sunucu Yöneticisi izinleri veya şirket içi sunucularınız hakkında özel bilgi gerektirebilir.

Ağ geçidi, sahne arkası iletişimin daha hızlı ve daha güvenli hale ilmesine yardımcı olur. Bu iletişim, buluttaki bir kullanıcı, ağ geçidi bulut hizmeti ve şirket içi veri kaynağınız arasında akar. Ağ geçidi bulut hizmeti, veri kaynağı kimlik bilgilerinizi ve ağ geçidi bilgilerinizi şifreler ve saklar. Hizmet ayrıca sorguları ve bunların sonuçlarını kullanıcı, ağ geçidi ve şirket içi veri kaynağınız arasında da yönlendirir.

Ağ geçidi güvenlik duvarlarıyla çalışır ve yalnızca giden bağlantıları kullanır. Tüm trafik ağ geçidi aracısından güvenli giden trafik olarak kaynaklanır. Ağ geçidi, şirket içi kaynaklardan gelen verileri Azure [Hizmet Veri Hizmeti Veri Yolunda](../service-bus-messaging/service-bus-messaging-overview.md)n içinşifrelenmiş kanallardan aktarıyor. Bu servis veri veri si ağ geçidi ve arama hizmeti arasında bir kanal oluşturur, ancak herhangi bir veri depolamaz. Ağ geçidinden geçen tüm veriler şifrelenir.

![Şirket içi veri ağ geçidi için mimari](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

> [!NOTE]
> Bulut hizmetine bağlı olarak, ağ geçidi için bir veri kaynağı ayarlamanız gerekebilir.

Bu adımlar, şirket içi bir veri kaynağına bağlı bir öğeyle etkileşimkurduğunuzda neler olduğunu açıklar:

1. Bulut hizmeti, veri kaynağının şifrelenmiş kimlik bilgileriyle birlikte bir sorgu oluşturur. Hizmet daha sonra sorguyu ve kimlik bilgilerini işlenmek üzere ağ geçidi sırasına gönderir.

1. Ağ geçidi bulut hizmeti sorguyu analiz eder ve isteği Azure Hizmet Veri Servisi'ne iter.

1. Azure Hizmet Veri Servisi bekleyen istekleri ağ geçidine gönderir.

1. Ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgilerine sahip bir veya daha fazla veri kaynağına bağlanır.

1. Ağ geçidi, sorguyu çalıştırmak için veri kaynağına gönderir.

1. Sonuçlar veri kaynağından ağ geçidine, ardından ağ geçidi bulut hizmetine gönderilir. Ağ geçidi bulut hizmeti daha sonra sonuçları kullanır.

### <a name="authentication-to-on-premises-data-sources"></a>Şirket içi veri kaynaklarına yönelik kimlik doğrulaması

Depolanmış bir kimlik bilgisi ağ geçidinden şirket içi veri kaynaklarına bağlanmak için kullanılır. Ağ geçidi, kullanıcıdan bağımsız olarak bağlanmak için depolanan kimlik bilgilerini kullanır. Power BI'de DirectQuery ve LiveConnect for Analysis Services gibi belirli hizmetler için kimlik doğrulama istisnaları olabilir.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)

Microsoft bulut hizmetleri, kullanıcıların kimliğini doğrulamak için [Azure AD'yi](../active-directory/fundamentals/active-directory-whatis.md) kullanır. Azure AD kiracısı kullanıcı adları ve güvenlik grupları içerir. Genellikle, oturum açmak için kullandığınız e-posta adresi, hesabınız için Kullanıcı Adı (UPN) ile aynıdır.

### <a name="what-is-my-upn"></a>UPN'im nedir?

Etki alanı yöneticisi değilseniz, UPN'nizi bilmiyor olabilirsiniz. Hesabınız için UPN'yi bulmak `whoami /upn` için iş istasyonunuzdaki komutu çalıştırın. Sonuç bir e-posta adresi gibi görünse de, sonuç yerel etki alanı hesabınız için UPN'dir.

### <a name="synchronize-an-on-premises-active-directory-with-azure-ad"></a>Bir şirket içi Active Directory hesabını Azure AD ile eşitleme

Şirket içi Etkin Dizin hesaplarınız ve Azure REKLAM hesaplarınız için UPN aynı olmalıdır. Bu nedenle, şirket içi Her Active Directory hesabının Azure REKLAM hesabınızla eşleştiğinden emin olun. Bulut hizmetleri yalnızca Azure AD'deki hesapları bilir. Bu nedenle, şirket içi Active Directory'nize bir hesap eklemenize gerek yoktur. Hesap Azure AD'de yoksa, bu hesabı kullanamazsınız.

Şirket içi Etkin Dizin hesaplarınızı Azure AD ile eşleştirmenin yolları aşağıda verilmiştir.

* Azure AD'ye hesapları el ile ekleyin.

  Azure portalında veya Microsoft 365 yönetici merkezinde bir hesap oluşturun. Şirket içi Active Directory hesabıiçin hesap adının UPN ile eşleştiğinden emin olun.

* Azure Active Directory Connect aracını kullanarak yerel hesapları Azure AD kiracınıza senkronize edin.

  Azure AD Connect aracı dizin eşitleme ve kimlik doğrulama kurulumu için seçenekler sağlar. Bu seçenekler parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon içerir. Kiracı yönetici veya yerel etki alanı yöneticisi değilseniz, Azure AD Connect kurulumu için BT yöneticinize başvurun. Azure AD Connect, Azure AD UPN'nizin yerel Active Directory UPN'inizle eşleşmesini sağlar. Bu eşleştirme, Power BI veya tek oturum açma (SSO) özelliklerine sahip Analiz Hizmetleri canlı bağlantılarını kullanıyorsanız yardımcı olur.

  > [!NOTE]
  > Hesapları Azure AD Connect aracıyla senkronize etmek, Azure AD kiracınızda yeni hesaplar oluşturur.

<a name="faq"></a>

## <a name="faq-and-troubleshooting"></a>SSS ve sorun giderme

Daha fazla bilgi için şu konulara bakın:

* [Şirket içi veri ağ geçidi hakkında SSS](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem-faq)
* [Şirket içi veri ağ geçidini sorun giderme](https://docs.microsoft.com/data-integration/gateway/service-gateway-tshoot)
* [Ağ geçidi performansını izleme ve en iyi duruma getirme](https://docs.microsoft.com/data-integration/gateway/service-gateway-performance)

## <a name="next-steps"></a>Sonraki adımlar

* [Mantık uygulamalarından şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md)
* [Kurumsal tümleştirme özellikleri](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Azure Logic Apps için Bağlayıcılar](../connectors/apis-list.md)
