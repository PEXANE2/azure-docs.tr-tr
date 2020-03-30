---
title: include dosyası
description: include dosyası
documentationcenter: partner-center-commercial-marketplace
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
ms.date: 08/13/2019
ms.author: dsindona
ms.custom: include file
ms.openlocfilehash: f2446a924ceed37c51779efc9d9e94c0252a2067
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80321732"
---
**Test sürüşü** sekmesi, müşterilerin teklifinizi satın almayı taahhüt etmeden önce denemelerini sağlayacak bir gösteri (veya "test sürüşü") ayarlamanızı sağlar. Makalede daha fazla bilgi Test [Drive nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive) Teklifiniz için artık bir test sürüşü sağlamak istemiyorsanız, **Teklif kurulum** sayfasına dönün ve **Test sürücüsüne etkin**olmayı etkinleştir'i denetle'nin artmasını sağlayın.

### <a name="technical-configuration"></a>Teknik yapılandırma
Aşağıdaki test sürücüleri, her biri kendi teknik yapılandırma gereksinimlerine sahip kullanılabilir.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Mantık uygulaması](#technical-configuration-for-logic-app-test-drive)
- [Güç BI](#technical-configuration-not-required-for-power-bi-test-drives) (Teknik yapılandırma gerekli değildir)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Azure Kaynak Yöneticisi test sürüşü için teknik yapılandırma

Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır. Azure Kaynak Yöneticisi [test sürücüsü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)ayarlama hakkında daha fazla bilgi edinin.

- **Bölgeler** (gerekli): Şu anda test sürüşünüzün kullanılabildiği 26 Azure destekli bölge bulunmaktadır. Genellikle, test sürüşünüzü en iyi performans için en yakın bölgeyi seçebilmeleri için en fazla müşteri sayısını tahmin ettiğiniz bölgelerde kullanılabilir hale getirmek istersiniz. Aboneliğinizin seçtiğiniz bölgelerin her birinde gereken tüm kaynakları dağıtmasına izin verildiğinden emin olmanız gerekir.

- **Örnekler**: Teklifinizin mevcut olduğu bölge sayısıyla çarpılarak bulunacak tür (sıcak veya soğuk) ve kullanılabilir örnek sayısını seçin.

**Hot**: Bu tür bir örnek dağıtılır ve seçili bölge başına erişim beklenilir. Müşteriler dağıtım beklemek yerine test sürücüsünün *Sıcak* örneklerine anında erişebilir. Bunun amacı, bu örneklerin Azure aboneliğinizde her zaman çalışıyor olmasıdır, bu nedenle daha büyük bir çalışma süresi maliyetine neden olurlar. Çoğu müşteri tam dağıtımları beklemek istemediğinden, en az bir *Sıcak* örneğinin olması önerilir ve bu da *Hot* instance yoksa müşteri kullanımında bir düşüşe neden olur.

**Soğuk**: Bu tür bir örnek, bölge başına dağıtılabilir toplam örnek sayısını temsil eder. Soğuk durumlar, bir müşteri test sürüşü istediğinde tüm Test Sürücüsü Kaynak Yöneticisi şablonuna dağıtılmasını gerektirir, bu nedenle *Soğuk* örneklerin yüklenmesi *Sıcak* örneklerden çok daha yavaştır. Bunun yanı sıra, yalnızca test sürüşü süresi için ödeme niz olması, her zaman Sıcak *örneğinde* olduğu gibi Azure aboneliğinizde *çalışmamasıdır.*

- **Test sürüşü Azure Kaynak Yöneticisi şablonu**: Azure Kaynak Yöneticisi şablonunuzu içeren .zip'i yükleyin.  Azure portalını kullanarak Azure [Kaynak Yöneticisi şablonları oluştur ve dağıtıla](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)başlıklı hızlı başlangıç makalesinde Bir Azure Kaynak Yöneticisi şablonu oluşturma hakkında daha fazla bilgi edinin.

- **Test sürüşü süresi** (gerekli): Test Sürüşünün etkin kalacağı süreyi saat sayısı olarak girin. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer. Bu süre yalnızca tam saat sayısına göre ayarlanabilir (örn. "2" saat, "1,5" geçerli değildir).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Dynamics 365 test sürüşü için teknik yapılandırma

Microsoft, bu tür bir test sürücüsükullanarak hizmet sağlama ve dağıtımını barındırarak ve sürdürerek bir test sürücüsü ayarlamanın karmaşıklığını kaldırabilir. Bu tür barındırılan test sürüşü yapılandırması, test sürücüsünün Bir İş Merkezi, Müşteri Etkileşimi veya Operasyonlar hedef kitlesini hedefleyip hedeflemediğine bakılmaksızın aynıdır.

- **Maksimum eşzamanlı test sürücüleri** (gerekli): Test sürüşünüzü aynı anda kullanabilecek maksimum müşteri sayısını ayarlayın. Her eşzamanlı kullanıcı test sürüşü etkinken bir Dynamics 365 lisansı tüketir, bu nedenle maksimum sınır kümesini desteklemek için yeterli lisansa sahip olduğunuzdan emin olmanız gerekir. Önerilen değer 3-5.

- **Test sürüşü süresi** (gerekli): Saat sayısını tanımlayarak Test Sürüşünün etkin kalacağı süreyi girin. Bu kadar saatten sonra oturum sona erer ve artık lisanslarınızdan birini tüketmeyecek. Teklifinizin karmaşıklığına bağlı olarak 2-24 saat lik bir değer öneriyoruz. Bu süre yalnızca tam saat sayısına göre ayarlanabilir (örn. "2" saat, "1,5" geçerli değildir).  Kullanıcı, zamanı biterse ve test sürücüsüne yeniden erişmek isterse yeni bir oturum isteyebilir.

- **Örnek URL** (gerekli): Müşterinin test sürüşüne başlayacağı URL. Genellikle Dynamics 365 örneğinizin URL'si uygulamanızı örnek veriler yüklü `https://testdrive.crm.dynamics.com`olarak çalıştırıyor (örn. ).

- **Örnek Web API URL'si** (gerekli): Microsoft 365 hesabınızda oturum açarak ve **Ayarlar** \&gt'de gezinerek Dynamics 365 örneğiniz için Web API URL'sini alın; **Özelleştirme** \&gt; **Geliştirici Kaynakları** \&gt; **Örnek Web API (Hizmet Kökü URL)**, burada bulunan `https://testdrive.crm.dynamics.com/api/data/v9.0`URL'yi kopyalayın (örn. ).

- **Rol adı** (gerekli): Özel Dynamics 365 test sürüşünüzde tanımladığınız güvenlik rol adını sağlayın. Bu, test sürüşü sırasında kullanıcıya atanır (örn. test sürüşü rolü).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Logic uygulaması test sürüşü için teknik yapılandırma

Herhangi bir özel ürün, çeşitli karmaşık çözüm mimarilerini kapsayan bu test sürücüsü dağıtım şablonu türünü kullanmalıdır. Logic App test sürücüleri ayarlama hakkında daha fazla bilgi için GitHub'da [Operasyonlar](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [Müşteri Katılımı'nı](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) ziyaret edin.

- **Bölge** (gerekli, tek seçimaçılır liste): Şu anda test sürüşünüzün kullanılabildiği 26 Azure destekli bölge bulunmaktadır. Mantık uygulamanızın kaynakları seçtiğiniz bölgede dağıtılır. Mantık Uygulamanızda belirli bir bölgede depolanan özel kaynaklar varsa, o bölgenin burada seçildiğinden emin olun. Bunu yapmanın en iyi yolu, Logic App'inizi portaldaki Azure aboneliğinizde yerel olarak tam olarak dağıtmak ve bu seçimi yapmadan önce doğru şekilde çalıştığını doğrulamaktır.

- **Maksimum eşzamanlı test sürücüleri** (gerekli): Test sürüşünüzü aynı anda kullanabilecek maksimum müşteri sayısını ayarlayın. Bu test sürücüleri zaten dağıtıldı ve müşterilerin dağıtım beklemeden bunlara anında erişmesini sağladı.

- **Test sürüşü süresi** (gerekli): Test Sürüşünün etkin kalacağı süreyi saat sayısı olarak girin. Bu süre sona erdikten sonra test sürüşü otomatik olarak sona erer.

- **Azure kaynak grubu adı** (gerekli): Logic App test sürücünüzün kaydedildiği [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) adını girin.

- **Azure mantık uygulaması adı** (gerekli): Test sürücüsünu kullanıcıya atayan Mantık uygulamasının adını girin. Bu Mantık uygulaması yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

- **Deprovision logic app name** (gerekli): Müşteri tamamlandıktan sonra test sürücüsünün hükmünü bozan Mantık uygulamasının adını girin. Bu Mantık uygulaması yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Power BI test sürücüleri için teknik yapılandırma gerekli değildir

Etkileşimli power bi görselgöstermek isteyen ürünler, test sürüşü olarak özel olarak oluşturulmuş bir panopaylaşmak için gömülü bir bağlantı kullanabilir, başka teknik yapılandırma gerekmez. [Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) şablon uyrama uygulamalarını ayarlama hakkında daha fazla bilgi edinin.

### <a name="deployment-subscription-details"></a>Dağıtım abonelik ayrıntıları

Test Sürüşü'ni sizin adınıza dağıtmak için lütfen ayrı, benzersiz bir Azure Aboneliği oluşturun ve sağlayın. (Power BI test sürücüleri için gerekli değildir).

- **Azure abonelik kimliği** (Azure Kaynak Yöneticisi ve Mantık uygulamaları için gereklidir): Kaynak kullanımı raporlaması ve faturalandırma için Azure hesap hizmetlerinize erişim izni vermek için abonelik kimliğigirin girin. Zaten test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmenizi öneririz. [Azure portalında](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **Abonelikler** sekmesinde gezinerek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçmek abonelik kimliğinizi görüntüler (örn. "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **Azure AD kiracı kimliği** (gerekli): Azure Etkin Dizini (AD) [kiracı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu kimliği bulmak için [Azure portalında](https://portal.azure.com/)oturum açın, sol menüdeki Etkin Dizin sekmesini seçin, **Özellikler'i** seçin ve listelenen **Dizin Kimliği** numarasını arayın (örn. 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, alan adınızın URL'sini kullanarak kuruluşunuzun [https://www.whatismytenantid.com](https://www.whatismytenantid.com)kiracı kimliğine de bakabilirsiniz: .

- **Azure AD kiracı adı** (Dinamik 365 için gereklidir): Azure Etkin Dizini (AD) adınızı girin. Bu adı bulmak için, sağ üst köşedeki [Azure portalında](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınız altında listelenir.

- **Azure AD uygulama kimliği** (gerekli): Azure Etkin Dizini (AD) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu kimliği bulmak için [Azure portalında](https://portal.azure.com/)oturum açın, sol menüdeki Active Directory sekmesini seçin, **Uygulama kayıtlarını**seçin, ardından listelenen Uygulama **Kimliği** numarasını arayın (örn. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulaması istemcisi gizli** (gerekli): Azure AD uygulama [istemcinizi gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)girin. Bu değeri bulmak için [Azure portalında](https://portal.azure.com/)oturum açın. Sol menüdeki **Azure Active Directory** sekmesini seçin, **Uygulama kayıtlarını**seçin ve ardından test sürüşü uygulamanızı seçin. Sonra, **Sertifikalar ve sırlar**seçin, **Yeni istemci gizli**seçin, bir açıklama girin, Bitiş **Dolması**Altında **Asla** seçin, sonra **Ekle**seçin . Değeri kopyaladığından emin olun. (Bunu yapmadan önce sayfadan uzağa gitmez, aksi takdirde değere erişemezsin.)

Bir sonraki bölüme geçmeden önce **Kaydet'i** unutmayın!

### <a name="test-drive-listings-optional"></a>Test sürüşü listeleri (isteğe bağlı)

**Test sürüşü** sekmesinin altında bulunan **Test Sürüşü listeleri** seçeneği, test sürücünüzün mevcut olduğu dilleri (ve pazarları) görüntüler ve şu anda İngilizce (ABD) kullanılabilir tek konumdur. Ayrıca, bu sayfa dile özgü girişin durumunu ve eklenme tarihi/saati görüntüler. Her dil/pazar için test sürüşü ayrıntılarını (açıklama, kullanım kılavuzu, videolar, vb.) tanımlamanız gerekir.

- **Açıklama** (gerekli): Test sürüşünüzü, gösterilecek leri, kullanıcının deney yapacağı hedefleri, araştırılacak özellikleri ve kullanıcının teklifinizi alıp almayacağını belirlemesine yardımcı olacak ilgili bilgileri açıklayın. Bu alana en fazla 3.000 metin karakteri girilebilir. 

- **Erişim bilgileri** (Azure Kaynak Yöneticisi ve Mantık test sürücüleri için gereklidir): Bu test sürüşüne erişmek ve bunları kullanmak için müşterinin bilmesi gerekenleri açıklayın. Teklifinizi ve test sürüşü boyunca özelliklere erişmek için müşterinin tam olarak bilmesi gerekenleri kullanmak için bir senaryoda yürüyün. Bu alana en fazla 10.000 karakter metin girilebilir.

- **Kullanım Kılavuzu** (gerekli): Test sürüşü deneyiminizi derinlemesine inceler. Kullanım Kılavuzu, müşterinin test sürüşünü deneyimlemekten elde etmesini tam olarak istediğiniz şeyi kapsamalı ve sahip olabilecekleri sorular için bir referans görevi göremelidir. Dosya PDF formatında olmalı ve yükledikten sonra (en fazla 255 karakter) adlandırılmalıdır.

- **Videolar: Video ekleme** (isteğe bağlı): Videolar YouTube veya Vimeo'ya yüklenebilir ve burada bir bağlantı ve küçük resim görüntüsüyle (533 x 324 piksel) başvurulabilir, böylece bir müşteri, teklifinizin özelliklerini başarıyla nasıl kullanacağınız ve avantajlarını vurgulayan senaryoları nasıl başarılı bir şekilde anlayacağınızı da dahil olmak üzere test sürüşünü daha iyi anlamalarına yardımcı olmak için bir bilgi yürüyüşünü görüntüleyebilir.
  - **Ad** (gerekli)
  - **URL (yalnızca YouTube veya Vimeo)** (gerekli)
  - **Küçük resim (533 x 324px)**: Resim dosyası PNG formatında olmalıdır.

Bu alanları tamamladıktan sonra **Kaydet'i** seçin.
