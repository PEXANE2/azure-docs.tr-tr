---
title: Azure Laboratuvar Hizmetleri'nde laboratuvar hesaplarını yapılandırma | Microsoft Dokümanlar
description: Bu makalede, Azure Lab Hizmetleri'nde bir laboratuvar hesabının nasıl oluşturulan, tüm laboratuvar hesaplarını nasıl görüntüleneniz veya silinme niz açıklanmaktadır.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: fa9dba62b3b58687ec6a2bfc29e8722f7016b679
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284310"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'ndeki laboratuvar hesaplarını yapılandırma 
Azure Laboratuvar Hizmetleri'nde, laboratuvar hesabı sınıf laboratuarları gibi yönetilen laboratuvar türleri için bir kapsayıcıdır. Yönetici, Azure Laboratuvar Hizmetleri ile bir laboratuvar hesabı oluşturur ve hesapta laboratuvar oluşturabilen laboratuvar sahiplerine erişim sağlar. 

Bu makalede, aşağıdaki görevlerin nasıl gerçekleştirililecekleri gösterilmektedir: 

- Laboratuvardaki VM'ler için bir adres aralığı belirtin
- Bağlantıyı kesmede VM'lerin otomatik olarak kapatılmasını yapılandırma

## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Laboratuvardaki VM'ler için bir adres aralığı belirtin
Aşağıdaki yordam, laboratuardaki VM'ler için bir adres aralığı belirtmek için adımlar vardır. Daha önce belirttiğiniz aralığı güncellerseniz, değiştirilen adres aralığı yalnızca değişiklik yapıldıktan sonra oluşturulan VM'ler için geçerlidir. 

Aşağıda, aklınızda bulundurmanız gereken adres aralığını belirtirken bazı kısıtlamalar verilmelidir. 

- Önek 23'ten küçük veya eşit olmalıdır. 
- Bir sanal ağ laboratuvar hesabına bakılırsa, sağlanan adres aralığı, eşlenen sanal ağdaki adres aralığıyla çakışamaz.

1. Laboratuvar **Hesabı** sayfasında, sol menüdeki **Labs ayarlarını** seçin.
2. Adres **aralığı** alanı için, laboratuvarda oluşturulacak VM'lerin adres aralığını belirtin. Adres aralığı sınıfsız etki alanları arası yönlendirme (CIDR) gösteriminde olmalıdır (örnek: 10.20.0.0/23). Bu adres aralığında laboratuvardaki sanal makineler oluşturulacaktır.
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Adres aralığını yapılandırma](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)


## <a name="automatic-shutdown-of-vms-on-disconnect"></a>Bağlantıyı kesmede VM'lerin otomatik olarak kapatılması
Uzak bir masaüstü bağlantısı kesildikten sonra Windows lab VM'lerinin (şablon veya öğrenci) otomatik olarak kapatılmasını etkinleştirebilir veya devre dışı kullanabilirsiniz. Ayrıca, otomatik olarak kapanmadan önce VM'lerin kullanıcının yeniden bağlanmasını ne kadar beklemesi gerektiğini de belirtebilirsiniz.

![Laboratuvar hesabında otomatik kapatma ayarı](../media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

Bu ayar, laboratuvar hesabında oluşturulan tüm laboratuarlar için geçerlidir. Bir laboratuvar sahibi bu ayarı laboratuvar düzeyinde geçersiz kılabilir. Laboratuvar hesabındaki bu ayardaki değişiklik yalnızca değişiklik yapıldıktan sonra oluşturulan laboratuarları etkiler.

Bir laboratuvar sahibinin bu ayarı laboratuvar düzeyinde nasıl yapılandırabileceğini öğrenmek için [bu makaleye](how-to-enable-shutdown-disconnect.md) bakın

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucunun laboratuvar konumunu seçmesine izin verme](allow-lab-creator-pick-lab-location.md)
- [Laboratuvarağınızı bir eş sanal ağıyla bağlayın](how-to-connect-peer-virtual-network.md)
- [Paylaşılan bir resim galerisini laboratuvara ekleme](how-to-attach-detach-shared-image-gallery.md)
- [Kullanıcıyı laboratuvar sahibi olarak ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
