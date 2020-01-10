---
title: Azure geçişi projeleri sorunlarını giderme
description: Azure geçişi projelerini oluşturma ve yönetme ile ilgili sorunları gidermenize yardımcı olur.
ms.topic: troubleshooting
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.date: 01/01/2020
ms.openlocfilehash: 3b8c2f6ec33965317d2aaa23a36b6becff11a54a
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75725734"
---
# <a name="troubleshoot-azure-migrate-projects"></a>Azure geçişi projeleri sorunlarını giderme

Bu makale, [Azure geçişi](migrate-services-overview.md) projelerini oluştururken ve yönetirken sorunları gidermenize yardımcı olur.

## <a name="how-to-add-new-project"></a>Yeni proje nasıl eklenir?

Bir abonelikte birden çok Azure geçişi projesi oluşturabilirsiniz. İlk kez bir proje oluşturmayı veya [ek projeler eklemeyi](create-manage-projects.md#create-additional-projects) [öğrenin](how-to-add-tool-first-time.md) .

## <a name="what-azure-permissions-are-needed"></a>Hangi Azure izinleri gerekir?

Azure geçişi projesi oluşturmak için abonelikte katkıda bulunan veya sahip izinlerinizin olması gerekir.

## <a name="cant-find-a-project"></a>Proje bulunamıyor

Mevcut bir Azure geçişi projesini bulmak, Azure geçişi 'nin güncel veya eski sürümünü kullanıyor olmanıza bağlı olarak değişir. [İzleyin](create-manage-projects.md#find-a-project).


## <a name="cant-find-a-geography"></a>Coğrafya bulunamıyor

[Desteklenen coğrafi](migrate-support-matrix.md#supported-geographies)bölgelerde bir Azure geçişi projesi oluşturabilirsiniz. Proje Coğrafya bulunan makine meta verilerini depolamak için kullanıldığını unutmayın. Diğer konumlardaki makineleri de değerlendirebilir veya geçirebilirsiniz.

## <a name="what-are-vm-limits"></a>VM sınırları nelerdir?

Tek bir projede en fazla 35.000 VMware VM veya en fazla 35.000 Hyper-V VM 'Leri değerlendirebilirsiniz. Bir proje, değerlendirme sınırlarına kadar hem VMware VM 'lerini hem de Hyper-V sanal makinelerini içerebilir.

## <a name="can-i-upgrade-old-project"></a>Eski projeyi yükseltebilir miyim?

Azure geçişi 'nin önceki sürümündeki projeler güncelleştirilemiyor. [Yeni bir proje oluşturmanız](how-to-add-tool-first-time.md)ve buna araçlar eklemeniz gerekir.

## <a name="cant-create-a-project"></a>Proje oluşturulamıyor

Bir proje oluşturmayı denerseniz ve bir dağıtım hatasıyla karşılaşırsanız:

- Geçici bir hata olması durumunda projeyi tekrar oluşturmayı deneyin. **Dağıtımlar**' da yeniden denemek için **yeniden Dağıt** ' a tıklayın.
- Abonelikte katkıda bulunan veya sahip izninizin olduğunu denetleyin.
- Yeni eklenen bir coğrafya ile dağıtım yapıyorsanız kısa bir süre bekleyip yeniden deneyin.
- "Istekler Kullanıcı kimlik üst bilgilerini içermesi gerekir" hatasını alırsanız, bu, kuruluşun Azure Active Directory (Azure AD) kiracısına erişiminizin olmadığını gösterebilir. Bu durumda:
    - Bir Azure AD kiracısına ilk kez eklenişinizde kiracıya katılması için bir e-posta davetiyesi alırsınız.
    - Kiracıya eklenecek daveti kabul edin.
    - E-postayı göremiyorsanız kiracıya erişimi olan bir kullanıcıyla iletişim kurun ve [daveti](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users) size yeniden göndermesini isteyin.
    - Davet e-postasını aldıktan sonra açın ve daveti kabul etmek için bağlantıyı seçin. Sonra, Azure portal oturumunuzu kapatın ve yeniden oturum açın. (tarayıcının yenilenmesi çalışmaz.) Daha sonra geçiş projesi oluşturmaya başlayabilirsiniz.

## <a name="how-do-i-delete-a-project"></a>Projeyi Nasıl yaparım? silme

Bir projeyi silmek için [Bu yönergeleri izleyin](create-manage-projects.md#delete-a-project) . Bir projeyi sildiğinizde, projenin ve projede bulunan makineler hakkındaki meta verilerin silindiğini unutmayın.

## <a name="added-tools-dont-show"></a>Eklenen araçlar gösterilmez

Doğru projenin seçili olduğundan emin olun. Azure geçişi Hub > **sunucularında** veya **veritabanlarında**, ekranın sağ üst köşesindeki **projeyi geçir (değiştir)** ' in yanındaki **Değiştir** ' e tıklayın. Doğru aboneliği ve proje adını seçin > **Tamam**' ı seçin. Sayfa, seçili projenin eklenen araçlarıyla yenilenir.

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi projelerine [değerlendirme](how-to-assess.md) veya [geçiş](how-to-migrate.md) araçları ekleyin.