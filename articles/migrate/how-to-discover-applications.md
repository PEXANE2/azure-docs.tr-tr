---
title: Azure geçişi ile şirket içi sunuculardaki uygulamaları, rolleri ve özellikleri bulma
description: Azure geçişi sunucu değerlendirmesi ile şirket içi sunuculardaki uygulamaları, rolleri ve özellikleri bulmayı öğrenin.
ms.topic: article
ms.date: 06/10/2020
ms.openlocfilehash: 535c8ae8c2d6e5d9d175e663a58d47dc76aa0529
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118656"
---
# <a name="discover-machine-apps-roles-and-features"></a>Makine uygulamalarını, rolleri ve özellikleri bul

Bu makalede, Azure geçişi: Sunucu değerlendirmesi kullanılarak şirket içi sunuculardaki uygulamaların, rollerin ve özelliklerin nasıl keşfedileceğini açıklanmaktadır.

Şirket içi makinelerde çalışan uygulamaların, rollerin ve özelliklerin envanterini bulmak, iş yükleriniz için Azure 'a geçiş yolunu belirlemenize ve uyarlamanıza yardımcı olur. App-Discovery, VM Konuk kimlik bilgilerini kullanarak bulma işlemi gerçekleştirmek için Azure geçişi gereci kullanır. Uygulama keşfi aracısız. VM 'Lere hiçbir şey yüklenmez.

> [!NOTE]
> Uygulama bulma Şu anda yalnızca VMware VM 'Leri için önizleme aşamasındadır ve yalnızca bulma ile sınırlıdır. Henüz uygulama tabanlı değerlendirme sunmuyoruz. 


## <a name="before-you-start"></a>Başlamadan önce

- Şunları yapmayı unutmayın:
    - Bir Azure geçişi projesi [oluşturdu](how-to-add-tool-first-time.md) .
    - Azure geçişi: Sunucu değerlendirmesi aracı bir projeye [eklendi](how-to-assess.md) .
- [Uygulama bulma desteğini ve gereksinimlerini](migrate-support-matrix-vmware.md#vmware-requirements)gözden geçirin.
- Uygulama bulma 'yı çalıştırdığınız VM 'Lerin PowerShell sürüm 2,0 veya üzeri yüklü olduğundan ve VMware araçlarının (10.2.0 ' den sonraki sürümler) yüklü olduğundan emin olun.
- Azure geçişi gerecini dağıtmaya yönelik [gereksinimleri](migrate-appliance.md) kontrol edin.


## <a name="deploy-the-azure-migrate-appliance"></a>Azure geçişi gereci dağıtma

1. Azure geçişi gerecini dağıtmaya yönelik gereksinimleri [gözden geçirin](migrate-appliance.md#appliance---vmware) .
2. Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu bulutlarında](migrate-appliance.md#government-cloud-urls)erişmesi gereken Azure URL 'lerini gözden geçirin.
3. Bulma ve değerlendirme sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---vmware) .
4. Gereç [için bağlantı noktası](migrate-support-matrix-vmware.md#port-access-requirements) erişim gereksinimleri.
5. Bulmayı başlatmak için [Azure geçişi gereci dağıtın](how-to-set-up-appliance-vmware.md) . Gereci dağıtmak için bir OVA şablonunu indirip bir VMware VM 'si olarak oluşturmak üzere VMware 'ye içeri aktarabilirsiniz. Gereci yapılandırıp Azure geçişi ile kaydedersiniz.
6. Gereci dağıtırken, sürekli bulmayı başlatmak için aşağıdakileri belirtirsiniz:
    - Bağlanmak istediğiniz vCenter Server adı.
    - VCenter Server Bağlanılacak gereç için oluşturduğunuz kimlik bilgileri.
    - Windows/Linux VM 'lerine bağlanmak için gereç için oluşturduğunuz hesap kimlik bilgileri.

Gereç dağıtıldıktan ve kimlik bilgilerini sağladıysanız, Gereç VM meta verilerinin ve performans verilerinin sürekli olarak keşfedilmesi ve uygulamaların, özelliklerin ve rollerin keşfedilmesi ile birlikte başlatılır.  Uygulama bulmanın süresi, sahip olduğunuz VM sayısına bağlıdır. Genellikle uygulama keşfi 500 VM 'Ler için bir saat sürer.

## <a name="verify-permissions"></a>İzinleri doğrulama

Bulma ve değerlendirme için [vCenter Server salt okunurdur bir hesap oluşturdunuz](tutorial-prepare-vmware.md#set-up-permissions-for-assessment) . Salt okuma hesabının, **sanal makineler**  >  **Konuk işlemleri**için etkin olması gerekir ve bu, uygulama bulma için VM ile etkileşime geçebilir.

### <a name="add-the-user-account-to-the-appliance"></a>Kullanıcı hesabını gereç 'e ekleme

Kullanıcı hesabını aşağıdaki şekilde ekleyin:

1. Gereç Yönetimi uygulamasını açın. 
2. **VCenter ayrıntıları sağla** paneline gidin.
3. **VM 'lerde uygulama ve bağımlılıkları keşfet**bölümünde **kimlik bilgileri ekle** ' ye tıklayın.
3. **İşletim sistemini**seçin, hesap için bir kolay ad ve **Kullanıcı adı** / **parolasını** girin
6. **Kaydet**’e tıklayın.
7. **Kaydet ve bulmayı Başlat**' a tıklayın.

    ![VM Kullanıcı hesabı ekle](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)


## <a name="review-and-export-the-inventory"></a>Envanteri gözden geçirin ve dışa aktarın

Bulma sona erdikten sonra, uygulama bulma için kimlik bilgileri sağladıysanız, Azure portal uygulama envanterini gözden geçirebilir ve dışarı aktarabilirsiniz.

1. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular** sayfasını açmak için, görünen sayıya tıklayın.

    > [!NOTE]
    > Bu aşamada, değerlendirmek istediğiniz makineler arasında bağımlılıkları görselleştirmek için isteğe bağlı olarak, bulunan makineler için bağımlılık analizini de ayarlayabilirsiniz. Bağımlılık analizi hakkında [daha fazla bilgi edinin](concepts-dependency-visualization.md) .

2. **Bulunan uygulamalarda**, görünen sayıya tıklayın.
3. **Uygulama envanteri**' nde, bulunan uygulamaları, rolleri ve özellikleri inceleyebilirsiniz.
4. Envanteri dışarı aktarmak için **bulunan sunucularda**, **uygulama envanterini dışarı aktar**' a tıklayın.

Uygulama envanteri, Excel biçiminde verilir ve indirilir. **Uygulama envanteri** sayfasında tüm makinelerde bulunan tüm uygulamalar görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

- Bulunan sunucular için [bir değerlendirme oluşturun](how-to-create-assessment.md) .
- [Azure geçişi: veritabanı değerlendirmesi](/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017)kullanarak SQL Server veritabanlarını değerlendirin.
