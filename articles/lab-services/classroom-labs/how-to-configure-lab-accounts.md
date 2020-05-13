---
title: Laboratuvar hesaplarını Azure Lab Services yapılandırma | Microsoft Docs
description: Bu makalede laboratuvar hesabının nasıl oluşturulacağı, tüm laboratuar hesaplarının nasıl görüntüleneceği veya Azure Lab Services bir laboratuvar hesabının nasıl silineceği açıklanır.
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
ms.openlocfilehash: 14f66701d3a375807829493f866dcb91b131f2e5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83121091"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Azure Lab Services laboratuvar hesaplarını yapılandırma 
Azure Lab Services, laboratuvar hesabı, sınıf laboratuvarları gibi yönetilen laboratuvar türleri için bir kapsayıcıdır. Yönetici Azure Lab Services bir laboratuvar hesabı ayarlar ve hesapta laboratuvarları oluşturabileceğiniz laboratuvar sahiplerine erişim sağlar. 

Bu makalede, aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilmektedir: 

- Laboratuvardaki VM 'Ler için bir adres aralığı belirtin
- Bağlantı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını yapılandırın

## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Laboratuvardaki VM 'Ler için bir adres aralığı belirtin
Aşağıdaki yordamda, laboratuvardaki VM 'Ler için bir adres aralığı belirtme adımları vardır. Daha önce belirttiğiniz aralığı güncelleştirirseniz, değiştirilen adres aralığı yalnızca değişiklik yapıldıktan sonra oluşturulan VM 'Ler için geçerlidir. 

Göz önünde bulundurmanız gereken adres aralığını belirtirken bazı kısıtlamalar aşağıda verilmiştir. 

- Ön ek, 23 ' ten küçük veya buna eşit olmalıdır. 
- Bir sanal ağ, laboratuvar hesabına eşlenirse, belirtilen adres aralığı eşlenen sanal ağın adres aralığıyla çakışamaz.

1. **Laboratuvar hesabı** sayfasında, soldaki menüden **Labs ayarları** ' nı seçin.
2. **Adres aralığı** alanı için, laboratuvarda oluşturulacak VM 'ler için adres aralığını belirtin. Adres aralığı, sınıfsız etki alanları arası yönlendirme (CıDR) gösteriminde olmalıdır (örnek: 10.20.0.0/23). Laboratuvardaki sanal makineler, bu adres aralığında oluşturulacaktır.

    > [!NOTE]
    > Adres aralığı özelliği yalnızca laboratuvar için eş sanal ağ etkinse geçerlidir. 
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Adres aralığını yapılandır](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)


## <a name="automatic-shutdown-of-vms-on-disconnect"></a>Bağlantı kesildiğinde VM 'lerin otomatik olarak kapatması
Uzak Masaüstü bağlantısı kesildikten sonra Windows laboratuvar VM 'lerinin (şablon veya öğrenci) otomatik olarak kapatılmasını etkinleştirebilir veya devre dışı bırakabilirsiniz. Ayrıca, sanal makinelerin otomatik olarak kapanmadan önce kullanıcının yeniden bağlanmasına ne kadar süre beklemesi gerektiğini de belirtebilirsiniz.

![Laboratuvar hesabındaki otomatik kapatılma ayarı](../media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

Bu ayar, laboratuvar hesabında oluşturulan tüm laboratuvarlara uygulanır. Laboratuvar sahibi bu ayarı laboratuvar düzeyinde geçersiz kılabilir. Laboratuvar hesabındaki bu ayarda yapılan değişiklik yalnızca değişiklik yapıldıktan sonra oluşturulan laboratuvarları etkiler.

Laboratuvar sahibi bu ayarı laboratuvar düzeyinde nasıl yapılandırabileceğinizi öğrenmek için, [Bu makaleye](how-to-enable-shutdown-disconnect.md) bakın

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucunun laboratuvar konumunu seçmesine izin verme](allow-lab-creator-pick-lab-location.md)
- [Laboratuvarınızın ağını eş bir sanal ağla bağlama](how-to-connect-peer-virtual-network.md)
- [Laboratuvara paylaşılan görüntü Galerisi iliştirme](how-to-attach-detach-shared-image-gallery.md)
- [Laboratuvar sahibi olarak Kullanıcı ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
