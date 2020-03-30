---
title: Windows Yönetici Merkezi Azure Güvenlik Merkezi ile nasıl entegre olun? Microsoft Dokümanlar
description: Bu makalede, Azure Güvenlik Merkezi'nin Windows Yönetici Merkezi ile nasıl tümleştirilen
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 5467794bf246fab4ff7ded9c445dbeee0c4093b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139632"
---
# <a name="integrate-azure-security-center-with-windows-admin-center"></a>Azure Güvenlik Merkezi'ni Windows Yönetici Merkezi ile tümleştirin

Windows Yönetici Merkezi, Windows sunucularınız için bir yönetim aracıdır. Sistem yöneticilerinin en sık kullanılan yönetici araçlarının çoğuna erişebildiği tek bir konumdur. Windows Yönetici Merkezi içinden, ön hazırlık sunucularınızı doğrudan Azure Güvenlik Merkezi'ne taşıyabilirsiniz. Daha sonra güvenlik önerilerinizin ve uyarılarınızın bir özetini doğrudan Windows Yönetici Merkezi deneyiminde görüntüleyebilirsiniz.

> [!NOTE]
> Azure aboneliğiniz in ve ilişkili Log Analytics çalışma alanının, Windows Yönetici Merkezi tümleştirmesini etkinleştirmek için Güvenlik Merkezi'nin standart katmanının etkinleştirilmiş olması gerekir.
> Abonelik ve çalışma alanında daha önce kullanmadıysanız, standart katman ilk 30 gün ücretsizdir. Daha fazla bilgi için [fiyatlandırma bilgileri sayfasına](security-center-pricing.md)bakın.
>

Windows Yönetici Merkezi'nden Azure Güvenlik Merkezi'ne bir sunucuya başarılı bir şekilde bindiğinizde şunları yapabilirsiniz:

* Windows Yönetici Merkezi'ndeki Güvenlik Merkezi uzantısı içinde güvenlik uyarılarını ve önerileri görüntüleme
* Güvenlik duruşunu görüntüleyin ve Azure portalı ndaki Güvenlik Merkezi'ndeki Windows Yönetici Merkezi yönetilen sunucularınızın ek ayrıntılı bilgilerini alın (veya bir API aracılığıyla)

Güvenlik Merkezi, bu iki aracı birleştirerek, kaynak ne olursa olsun tüm güvenlik bilgilerinizi görüntülemek için tek cam bölmeniz olur: Windows Yönetici Merkezinizin ön lisans sunucularında yönetilmesi, VM'leriniz ve ek PaaS iş yüklerinin korunması.

## <a name="onboarding-windows-admin-center-managed-servers-into-security-center"></a>Windows Yönetici Merkezi'nde yönetilen sunucuları Güvenlik Merkezi'ne

1. Windows Yönetici Merkezi'nden sunucularınızdan birini seçin ve **Araçlar** bölmesinde Azure Güvenlik Merkezi uzantısını seçin:

    ![Windows Yönetici Merkezi'nde Azure Güvenlik Merkezi uzantısı](./media/windows-admin-center-integration/onboarding-from-wac.png)

    > [!NOTE]
    > Sunucu zaten Güvenlik Merkezi'ne bağlıysa, kurulum penceresi görünmez.

1. **Azure'da Oturum Aç'ı**tıklatın ve ayarlayın.
    ![Azure Güvenlik Merkezi'ne Windows Yönetici Merkezi uzantısı onboarding](./media/windows-admin-center-integration/onboarding-from-wac-welcome.png)

1. Sunucunuzu Güvenlik Merkezi'ne bağlamak için yönergeleri izleyin. Gerekli ayrıntıları girdikten ve onaylandıktan sonra, Güvenlik Merkezi aşağıdakilerin hepsinin doğru olduğundan emin olmak için gerekli yapılandırma değişikliklerini yapar:
    * Azure Ağ Geçidi kayıtlıdır.
    * Sunucunun rapor edilen bir çalışma alanı ve ilişkili bir abonelik vardır.
    * Security Center'ın standart katman Log Analytics çözümü çalışma alanında etkinleştirilir. Bu çözüm, bu çalışma alanına rapor veren *tüm* sunucular ve sanal makineler için Security Center'ın Standart katman özelliklerini sağlar.
    * Güvenlik Merkezi'nin Sanal Makine için standart katman fiyatlandırması abonelikte etkinleştirilir.
    * Microsoft İzleme Aracısı (MMA) sunucuya yüklenir ve seçili çalışma alanına rapor vermek üzere yapılandırılır. Sunucu zaten başka bir çalışma alanına rapor veriyorsa, yeni seçilen çalışma alanına da rapor vermek üzere yapılandırılmıştır.

    > [!NOTE]
    > Önerilerin görünmesi uçağa bindikten sonra biraz zaman alabilir. Aslında, sunucu etkinliğinize bağlı olarak herhangi *bir* uyarı alamayabilirsiniz. Uyarılarınızın doğru çalıştığını test etmek için test uyarıları oluşturmak için [uyarı doğrulama yordamındaki](security-center-alert-validation.md)yönergeleri izleyin.


## <a name="viewing-security-recommendations-and-alerts-in-windows-admin-center"></a>Windows Yönetici Merkezi'nde güvenlik önerilerini ve uyarılarını görüntüleme

Uçağa bindikten sonra uyarılarınızı ve önerilerinizi doğrudan Windows Yönetici Merkezi'nin Azure Güvenlik Merkezi alanında görüntüleyebilirsiniz. Azure portalında görüntülemek için bir öneri veya uyarıyı tıklatın. Burada ek bilgiler alır ve sorunları nasıl düzelteceğinizi öğrenirsiniz.

[![Windows Yönetici Merkezi'nde görüldüğü gibi Güvenlik Merkezi önerileri ve uyarıları](media/windows-admin-center-integration/asc-recommendations-and-alerts-in-wac.png)](media/windows-admin-center-integration/asc-recommendations-and-alerts-in-wac.png#lightbox)

## <a name="viewing-security-recommendations-and-alerts-for-windows-admin-center-managed-servers-in-security-center"></a>Güvenlik Merkezi'nde Windows Yönetici Merkezi yönetilen sunucular için güvenlik önerilerini ve uyarılarını görüntüleme
Azure Güvenlik Merkezi'nden:

* Tüm Windows Yönetici Merkezi sunucularınız için güvenlik önerilerini görüntülemek **için, Bilgi & Uygulamaları'nı** açın ve **VM'ler ve Bilgisayarlar** sekmesini tıklatın.

    [![Windows Yönetici Merkezi yönetilen sunucular için güvenlik önerilerini görüntüleme](media/windows-admin-center-integration/viewing-recommendations-wac.png)](media/windows-admin-center-integration/viewing-recommendations-wac.png#lightbox)

* Tüm Windows Yönetici Merkezi sunucularınız için güvenlik uyarılarını görüntülemek için **Güvenlik uyarılarını**açın. **Filtre'yi** tıklatın ve **yalnızca** "Azure'suz" seçili olduğundan emin olun:

    ![Windows Yönetici Merkezi yönetilen sunucular için filtre güvenlik uyarıları](./media/windows-admin-center-integration/filtering-alerts-to-non-azure.png)

    [![Windows Yönetici Merkezi yönetilen sunucular için güvenlik uyarılarını görüntüleme](media/windows-admin-center-integration/viewing-alerts-wac.png)](media/windows-admin-center-integration/viewing-alerts-wac.png#lightbox)