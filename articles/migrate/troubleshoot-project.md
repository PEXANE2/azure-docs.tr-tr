---
title: Azure Geçişi projeleri sorunlarını giderme
description: Azure Geçiş projeleri oluşturma ve yönetme yle ilgili sorunları gidermenize yardımcı olur.
ms.topic: troubleshooting
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.date: 01/01/2020
ms.openlocfilehash: 3b8c2f6ec33965317d2aaa23a36b6becff11a54a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75725734"
---
# <a name="troubleshoot-azure-migrate-projects"></a>Azure Geçişi projeleri sorunlarını giderme

Bu makale, [Azure Geçiş](migrate-services-overview.md) projeleri oluştururken ve yönetirken sorunları gidermenize yardımcı olur.

## <a name="how-to-add-new-project"></a>Yeni proje nasıl eklenir?

Bir abonelikte birden çok Azure Geçiş projesiniz olabilir. İlk kez bir proje oluşturmayı veya [ek](create-manage-projects.md#create-additional-projects) projeler [eklemeyi öğrenin.](how-to-add-tool-first-time.md)

## <a name="what-azure-permissions-are-needed"></a>Hangi Azure izinleri gereklidir?

Bir Azure Geçiş projesi oluşturmak için abonelikte Katılımcı veya Sahip izinlerine ihtiyacınız vardır.

## <a name="cant-find-a-project"></a>Proje bulamıyorum

Varolan bir Azure Geçiş projesi bulmak, Azure Geçiş'in geçerli veya eski sürümünü kullanıp kullanmadığınıza bağlıdır. [Takip edin.](create-manage-projects.md#find-a-project)


## <a name="cant-find-a-geography"></a>Coğrafya bulamıyorum.

[Desteklenen coğrafyalarda](migrate-support-matrix.md#supported-geographies)bir Azure Geçiş projesi oluşturabilirsiniz. Proje coğrafyasının keşfedilen makine meta verilerini depolamak için kullanıldığını unutmayın. Makineleri başka konumlara da değerlendirebilir veya geçirebilirsiniz.

## <a name="what-are-vm-limits"></a>VM limitleri nelerdir?

Tek bir projede 35.000 VM'ye kadar VM'yi veya 35.000'e kadar Hyper-V VM'yi değerlendirebilirsiniz. Bir proje, değerlendirme sınırlarına kadar hem VMware VM'leri hem de Hyper-V VM'leri içerebilir.

## <a name="can-i-upgrade-old-project"></a>Eski projeyi yükseltebilir miyim?

Azure Geçiş önceki sürümündeki projeler güncelleştirilemez. [Yeni bir proje oluşturmanız](how-to-add-tool-first-time.md)ve buna araçlar eklemeniz gerekir.

## <a name="cant-create-a-project"></a>Proje oluşturamıyorum

Bir proje oluşturmaya çalışır ve bir dağıtım hatasıyla karşılaşırsanız:

- Geçici bir hata olması durumunda projeyi yeniden oluşturmaya çalışın. **Dağıtımlarda,** yeniden denemek için **Yeniden dağıt'ı** tıklatın.
- Abonelikte Katılımcı veya Sahip İzni olup olup yok.
- Yeni eklenen bir coğrafyada dağıtım yapıyorsunuz, kısa bir süre bekleyin ve yeniden deneyin.
- "İstekler kullanıcı kimliği üstbilgileri içermelidir" hatasını alırsanız, bu, kuruluşun Azure Etkin Dizin (Azure AD) kiracısına erişiminiz olmadığını gösterebilir. Bu durumda:
    - Bir Azure REKLAM kiracısına ilk kez eklendiğinde, kiracıya katılmak için bir e-posta daveti alırsınız.
    - Kiracıya eklenmek üzere daveti kabul edin.
    - E-postayı göremiyorsanız, kiracıya erişimi olan bir kullanıcıyla iletişime geçin ve daveti size [yeniden göndermelerini](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users) isteyin.
    - Davet e-postasını aldıktan sonra açın ve daveti kabul etmek için bağlantıyı seçin. Ardından, Azure portalından çıkış ve yeniden oturum açın. (tarayıcıyı yenilemek çalışmaz.) Daha sonra geçiş projesini oluşturmaya başlayabilirsiniz.

## <a name="how-do-i-delete-a-project"></a>Projeyi nasıl silerim?

Bir projeyi silmek için [bu yönergeleri izleyin.](create-manage-projects.md#delete-a-project) Bir projeyi sildiğinizde, hem proje nin hem de projedeki keşfedilen makinelerle ilgili meta verilerin silindiğini unutmayın.

## <a name="added-tools-dont-show"></a>Eklenen araçlar görünmüyor

Doğru projeyi seçtiğinizden emin olun. **Azure** Geçir hub> **Sunucular'da**veya Veritabanlarında, ekranın sağ üst köşesindeki **Taşıt Projesi'nin (Değiştir)** yanındaki **Değiştir'i** tıklatın. **Ok**> doğru abonelik ve proje adını seçin. Sayfa, seçili projenin eklenen araçlarıyla yenilenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Geçiş projelerine [değerlendirme](how-to-assess.md) veya [geçiş](how-to-migrate.md) araçları ekleyin.