---
title: Windows Yönetim Merkezi sunucularını Azure Güvenlik Merkezi ile koruma
description: Bu makalede, Azure Güvenlik Merkezi 'nin Windows Yönetim Merkezi ile nasıl tümleştirileceği açıklanmaktadır.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: memildin
ms.openlocfilehash: 36f519ce41ccfbfb48ca696ed2a61c6131a75998
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906336"
---
# <a name="protect-windows-admin-center-resources-with-security-center"></a>Güvenlik Merkezi ile Windows Yönetim Merkezi kaynaklarını koruma

Windows Yönetim Merkezi, Windows sunucularınız için bir yönetim aracıdır. Sistem yöneticilerinin en sık kullanılan yönetim araçlarının çoğunluğuna erişebilmeleri için tek bir konumdur. Windows Yönetim Merkezi 'nin içinden, şirket içi sunucularınızı doğrudan Azure Güvenlik Merkezi 'ne ekleyebilirsiniz. Daha sonra, güvenlik önerilerinizi ve uyarılarınızı bir Özet olarak doğrudan Windows Yönetim Merkezi deneyimi ' nde görüntüleyebilirsiniz.

> [!NOTE]
> Windows Yönetim Merkezi tümleştirmesini etkinleştirmek için Azure aboneliğiniz ve ilişkili Log Analytics çalışma alanının her ikisi de Azure Defender 'ın etkinleştirilmiş olması gerekir.
> Daha önce bunu abonelikte ve çalışma alanında kullanmadıysanız, Azure Defender ilk 30 gün boyunca ücretsizdir. Daha fazla bilgi için bkz. [fiyatlandırma bilgileri sayfası](security-center-pricing.md).
>

Windows Yönetim Merkezi 'nden Azure Güvenlik Merkezi 'ne bir sunucu başarıyla eklendi, şunları yapabilirsiniz:

* Windows Yönetim Merkezi 'nde Güvenlik Merkezi uzantısının içindeki güvenlik uyarılarını ve önerilerini görüntüleme
* Güvenlik duruşunu görüntüleyin ve Azure portal (veya bir API aracılığıyla) Güvenlik Merkezi 'nde Windows Yönetim Merkezi tarafından yönetilen sunucularınızın ek ayrıntılı bilgilerini alın

Bu iki aracı birleştirerek Güvenlik Merkezi, kaynak: Windows Yönetim Merkezi 'nde yönetilen şirket içi sunucuları, VM 'lerinizi ve ek PaaS iş yüklerinizi koruyan tüm güvenlik bilgilerinizi görüntülemek için tek bir cam bölmesidir.

## <a name="onboard-windows-admin-center-managed-servers-into-security-center"></a>Windows Yönetim Merkezi tarafından yönetilen sunucuları güvenlik merkezi 'ne ekleme

1. Windows Yönetim Merkezi 'nden sunucularınızdan birini seçin ve **Araçlar** bölmesinde Azure Güvenlik Merkezi uzantısını seçin:

    ![Windows Yönetim Merkezi 'nde Azure Güvenlik Merkezi uzantısı](./media/windows-admin-center-integration/onboarding-from-wac.png)

    > [!NOTE]
    > Sunucu zaten güvenlik merkezi eklendi, kurulum penceresi görünmez.

1. **Azure 'Da oturum aç**' a tıklayın ve kurulumu yapın.
    ![Windows Yönetim Merkezi uzantısını Azure Güvenlik Merkezi 'ne ekleme](./media/windows-admin-center-integration/onboarding-from-wac-welcome.png)

1. Sunucunuzu Güvenlik Merkezi 'ne bağlamak için yönergeleri izleyin. Gerekli ayrıntıları girdikten ve onaylandıktan sonra, güvenlik merkezi aşağıdakilerin tümünün doğru olduğundan emin olmak için gerekli yapılandırma değişikliklerini yapar:
    * Bir Azure Gateway kaydedilir.
    * Sunucuda, raporlamak için bir çalışma alanı ve ilişkili bir abonelik vardır.
    * Güvenlik Merkezi 'nin Log Analytics çözümü çalışma alanında etkinleştirilmiştir. Bu çözüm, bu çalışma alanına rapor veren *Tüm* sunucular ve sanal makineler Için Azure Defender 'ın özelliklerini sağlar.
    * Sunucular için Azure Defender, abonelikte etkinleştirilmiştir.
    * Log Analytics Aracısı sunucuya yüklenir ve seçilen çalışma alanına raporlamak için yapılandırılır. Sunucu zaten başka bir çalışma alanına bildirirse, yeni seçilen çalışma alanına da rapor vermek için yapılandırılır.

    > [!NOTE]
    > Önerilerin görünmesi için ekleme biraz zaman alabilir. Aslında, sunucu etkinliklerinize bağlı olarak *herhangi bir* uyarı almazsınız. Uyarılarınızın doğru şekilde çalıştığını sınamak için test uyarıları oluşturmak üzere [Uyarı doğrulama yordamındaki](security-center-alert-validation.md)yönergeleri izleyin.


## <a name="view-security-recommendations-and-alerts-in-windows-admin-center"></a>Windows Yönetim Merkezi 'nde güvenlik önerilerini ve uyarılarını görüntüleme

Eklendi bir kez, uyarı ve önerilerinizi doğrudan Windows Yönetim Merkezi 'nin Azure Güvenlik Merkezi alanında görüntüleyebilirsiniz. Azure portal görüntülemek için bir öneri veya uyarıya tıklayın. Burada, daha fazla bilgi edinirsiniz ve sorunları nasıl düzelteceğinizi öğreneceksiniz.

[![Windows Yönetim Merkezi 'nde görülen Güvenlik Merkezi önerileri ve uyarıları](media/windows-admin-center-integration/asc-recommendations-and-alerts-in-wac.png)](media/windows-admin-center-integration/asc-recommendations-and-alerts-in-wac.png#lightbox)

## <a name="view-security-recommendations-and-alerts-for-windows-admin-center-managed-servers-in-security-center"></a>Güvenlik Merkezi 'nde Windows Yönetim Merkezi tarafından yönetilen sunucular için güvenlik önerilerini ve uyarılarını görüntüleme
Azure Güvenlik Merkezi 'nden:

* Tüm Windows Yönetim Merkezi sunucularınızın güvenlik önerilerini görüntülemek için [varlık envanterini](asset-inventory.md) açın ve araştırmak istediğiniz makine türüne filtre uygulayın. **VM 'ler ve bilgisayarlar** sekmesini seçin.

* Tüm Windows Yönetim Merkezi sunucularınızın güvenlik uyarılarını görüntülemek için **güvenlik uyarılarını**açın. **Filtre** ' ye tıklayın ve **yalnızca** "Azure dışı" ' ın seçili olduğundan emin olun:

    :::image type="content" source="./media/windows-admin-center-integration/filtering-alerts-by-environment.png" alt-text="Windows Yönetim Merkezi tarafından yönetilen sunucular için güvenlik uyarılarını filtreleme" lightbox="./media/windows-admin-center-integration/filtering-alerts-by-environment.png":::