---
title: Azure Otomasyonu 'nda VM 'niz için güncelleştirmeleri ve düzeltme eklerini yönetme
description: Bu makalede, Azure ve Azure olmayan sanal makinelerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Güncelleştirme Yönetimi nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 07/28/2020
ms.custom: mvc
ms.openlocfilehash: e4f630931e3cd79a46a539955b45d72e5b573271
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450860"
---
# <a name="manage-updates-and-patches-for-your-vms"></a>VM 'niz için güncelleştirmeleri ve düzeltme eklerini yönetme

Azure Otomasyonu Güncelleştirme Yönetimi Yazılım güncelleştirmeleri, Azure ve hibrit bulutundaki makinelere yazılım güncelleştirmelerini izleme ve uygulamanın karmaşık görevi yönetimine yardımcı olabilecek bir dizi araç ve kaynak sağlar. İşlemsel verimliliği sürdürmek, güvenlik sorunlarının üstesinden gelmek ve artırılmış siber güvenlik tehditlerine karşı riskleri azaltmak için etkili bir yazılım güncelleştirme yönetimi işlemi gereklidir. Ancak, teknolojinin değişen yapısı ve yeni güvenlik tehditlerinin sürekli ortaya çıkması nedeniyle, etkili yazılım güncelleştirme yönetimi tutarlı ve sürekli bir ilgi gerektirir.

> [!NOTE]
> Güncelleştirme Yönetimi, birinci taraf güncelleştirmelerin dağıtımını ve bunların önceden indirilmesini destekler. Bu destek, güncelleştirilmekte olan sistemlerde değişiklik yapılmasını gerektirir. Bu ayarları sistemlerinizde nasıl yapılandıracağınızı öğrenmek için bkz. [Azure Automation güncelleştirme yönetimi için Windows Update ayarlarını yapılandırma](update-mgmt-configure-wuagent.md) .

Sanal makinelerinize yönelik güncelleştirmeleri yönetmeyi denemeden önce, aşağıdaki yöntemlerden birini kullanarak Güncelleştirme Yönetimi üzerinde etkin olduğunuzdan emin olun:

* [Otomasyon hesabından Güncelleştirme Yönetimi’ni etkinleştirme](update-mgmt-enable-automation-account.md)
* [Azure portal göz atarak Güncelleştirme Yönetimi etkinleştirin](update-mgmt-enable-portal.md)
* [Runbook’tan Güncelleştirme Yönetimi’ni etkinleştirme](update-mgmt-enable-runbook.md)
* [Azure VM’den Güncelleştirme Yönetimi’ni etkinleştirme](update-mgmt-enable-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Dağıtımın kapsamını sınırlandırma

Güncelleştirme Yönetimi, güncelleştirmeleri almak için bilgisayarları hedeflemek üzere çalışma alanı içinde bir kapsam yapılandırması kullanır. Daha fazla bilgi için bkz. [sınır güncelleştirme yönetimi dağıtım kapsamı](update-mgmt-scope-configuration.md).

## <a name="compliance-assessment"></a>Uyumluluk değerlendirmesi

Makinelerinize yazılım güncelleştirmeleri dağıtmadan önce, etkinleştirilmiş makineler için güncelleştirme uyumluluğu değerlendirmesi sonuçlarını gözden geçirin. Her yazılım güncelleştirmesi için, uyumluluk durumu kaydedilir ve değerlendirme tamamlandıktan sonra Azure Izleyici günlüklerine toplu olarak toplanır ve iletilir.

Bir Windows makinesinde, uyumluluk taraması varsayılan olarak her 12 saatte bir çalıştırılır. Zamanlanan taramaya ek olarak, güncelleştirme uyumluluğuna yönelik tarama, güncelleştirme yüklemesinden önce ve güncelleştirme yüklemesinden sonra, Windows için Log Analytics aracısının 15 dakika içinde başlatılır. Ayrıca, doğru yönetilmesini engelleyen sorunlardan kaçınmak için [Windows Update istemcisinin güncelleştirme yönetimi yapılandırma](update-mgmt-configure-wuagent.md) önerilerini gözden geçirmeniz önemlidir.

Bir Linux makinesi için, uyumluluk taraması her saat varsayılan olarak gerçekleştirilir. Linux için Log Analytics Aracısı yeniden başlatılırsa, 15 dakika içinde bir uyumluluk taraması başlatılır.

Uyumluluk sonuçları, değerlendirilen her makine için Güncelleştirme Yönetimi olarak sunulur. Yönetim için etkinleştirilen yeni bir makine için panonun güncelleştirilmiş verileri görüntülemesi 30 dakika kadar sürebilir.

Uyumluluk sonuçlarının nasıl görüntüleneceğini öğrenmek için [yazılım güncelleştirmelerini izleyin](update-mgmt-view-update-assessments.md) ' i gözden geçirin.

## <a name="deploy-updates"></a>Güncelleştirmeleri dağıt

Uyumluluk sonuçlarını inceledikten sonra yazılım güncelleştirme dağıtım aşaması, yazılım güncelleştirmelerini dağıtma işlemidir. Güncelleştirmeleri yüklemek için, sürüm zamanlamanız ve hizmet pencerenize göre hizalanan bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

Güncelleştirme dağıtımını zamanlamayı öğrenmek için [yazılım güncelleştirmelerini dağıtma](update-mgmt-deploy-updates.md) konusunu gözden geçirin.

## <a name="review-update-deployments"></a>Güncelleştirme dağıtımlarını gözden geçirme

Dağıtım tamamlandıktan sonra, makineye veya hedef gruba göre güncelleştirme dağıtımının başarısını belirleme işlemini gözden geçirin. Dağıtım durumunu nasıl izleyebileceğinizi öğrenmek için bkz. [dağıtım durumunu gözden geçirme](update-mgmt-deploy-updates.md#check-deployment-status) .

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım sonuçlarını güncelleştirme hakkında sizi bilgilendirmek üzere uyarılar oluşturmayı öğrenmek için bkz. [güncelleştirme yönetimi uyarı oluşturma](update-mgmt-configure-alerts.md).

* Güncelleştirme değerlendirmelerini, dağıtımları ve diğer ilgili yönetim görevlerini çözümlemek için [Azure izleyici günlüklerini sorgulayabilirsiniz](update-mgmt-query-logs.md) . Başlamanıza yardımcı olmak için önceden tanımlanmış sorgular içerir.