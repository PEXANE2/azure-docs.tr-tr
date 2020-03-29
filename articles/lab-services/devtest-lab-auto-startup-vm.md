---
title: Azure DevTest Labs'da bir VM için otomatik başlatma ayarlarını yapılandırın | Microsoft Dokümanlar
description: Laboratuvarda VM'ler için otomatik başlatma ayarlarını nasıl yapılandırıştırabilirsiniz öğrenin. Bu ayar, laboratuardaki VM'lerin zamanlamada otomatik olarak başlatılmasını sağlar.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/03/2019
ms.author: spelluru
ms.openlocfilehash: 95f810ba16f358c5aabc35e26294cdb3f8c3cca0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74807920"
---
# <a name="auto-startup-lab-virtual-machines"></a>Otomatik başlangıç laboratuvarı sanal makineler  
Azure DevTest Labs, laboratuarınızdaki sanal makineleri bir zamanlamaya göre otomatik olarak başlatılacak ve kapatılmak üzere yapılandırmanıza olanak tanır. Otomatik kapatma ayarlarını yapılandırma hakkında daha fazla bilgi için Azure [DevTest Labs'daki bir laboratuvar için otomatik kapatma ilkelerini yönet'e](devtest-lab-auto-shutdown.md)bakın. 

İlke açıkken tüm VM'lerin dahil edildiği otomatik kapatmanın aksine, otomatik başlatma ilkesi bir laboratuvar kullanıcısının açıkça bir VM seçmesini ve bu zamanlamaya katılmayı seçmesini gerektirir. Bu şekilde, istenmeyen VM'lerin yanlışlıkla otomatik olarak başlatıldığı ve beklenmeyen harcamalara neden olduğu duruma kolayca rastlamayacaksınız.

Bu makalede, bir laboratuvar için otomatik başlatma ilkesini nasıl yapılandırabileceğiniz gösterilmektedir.

## <a name="configure-autostart-settings-for-a-lab"></a>Laboratuvar için otomatik başlatma ayarlarını yapılandırma 
1. Laboratuvarınızın ana sayfasına gidin. 
2. Sol menüde **Yapılandırma ve ilkeler'i** seçin. 

    ![Yapılandırma ve ilkeler menüsü](./media/devtest-lab-auto-startup-vm/configuration-policies-menu.png)
3. Yapılandırma **ve ilkeler** sayfasında aşağıdaki adımları yapın:
    
    1. Bu laboratuvarın otomatik başlatma özelliğini etkinleştirmek **için otomatik başlatma için zamanlanacak sanal makinelere izin ver'i** seçin. **On** 
    2. **Zamanlama başlangıç** alanı için bir başlangıç saati (örneğin: 8:00:00 AM) seçin. 
    3. Kullanılacak bir **saat dilimi** seçin. 
    4. VM'lerin otomatik olarak başlatılması gereken **haftanın günlerini** seçin. 
    5. Ardından, ayarları kaydetmek için araç çubuğunda **Kaydet'i** seçin. 

        ![Otomatik başlatma ayarları](./media/devtest-lab-auto-startup-vm/auto-start-configuration.png)

        > [!IMPORTANT]
        > Bu ilke, laboratuardaki sanal makinelere otomatik olarak otomatik başlatma uygulamaz. Tek tek sanal makineleri **tercih** etmek için sanal makine sayfasına gidin ve bu VM için **otomatik başlatmayı** etkinleştirin.

## <a name="enable-autostart-for-a-vm-in-the-lab"></a>Laboratuvarda bir VM için otomatik başlatmayı etkinleştirme
Aşağıdaki yordam, bir VM'yi laboratuvarın otomatik başlatma ilkesine dönüştürmeniz için adımlar sağlar. 

1. Laboratuvarınızın ana sayfasında **Sanal Makineler listemdeki** **VM'yi** seçin. 

    ![Yapılandırma ve ilkeler menüsü](./media/devtest-lab-auto-startup-vm/select-vm.png)
2. Sanal **makine** sayfasında, sol menüde veya **Zamanlamalar** listesinde **Otomatik Başlat'ı** seçin. 

    ![Otomatik başlat menüsünü seçin](./media/devtest-lab-auto-startup-vm/select-auto-start.png)
3. Otomatik **Başlatma** sayfasında, bu sanal makinenin otomatik başlatma seçeneği **için zamanlanmasına izin ver'i için** **On'u** seçin.

    ![VM için otomatik başlatmayı etkinleştirme](./media/devtest-lab-auto-startup-vm/auto-start-vm.png)
4. Ardından, ayarı kaydetmek için araç çubuğunda **Kaydet'i** seçin. 


## <a name="next-steps"></a>Sonraki adımlar
Bir laboratuvar için yapılandırma otomatik kapatma ilkesi hakkında bilgi edinmek için Azure [DevTest Labs'daki bir laboratuvar için otomatik kapatma ilkelerini yönet'e](devtest-lab-auto-shutdown.md) bakın
