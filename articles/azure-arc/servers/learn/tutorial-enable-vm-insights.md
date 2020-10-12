---
title: Öğretici-VM'ler için Azure İzleyici bir karma makineyi Izleme
description: Azure Izleyici 'de karma makineden verileri nasıl toplayacağınızı ve analiz edeceğinizi öğrenin.
ms.topic: tutorial
ms.date: 09/23/2020
ms.openlocfilehash: 97ab390570f434295a5aa836ef994640f6dc14f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91335424"
---
# <a name="tutorial-monitor-a-hybrid-machine-with-azure-monitor-for-vms"></a>Öğretici: VM'ler için Azure İzleyici bir karma makineyi Izleme

[Azure izleyici](../overview.md) , ayrıntılı analiz ve bağıntı için doğrudan karma sanal makinelerinizden bir Log Analytics çalışma alanına veri toplayabilir. Genellikle bu durum, yapılandırma yönetimi standartlarınızı takip eden bir betik, el ile veya otomatik yöntemi kullanarak makineye [Log Analytics aracısını](../../../azure-monitor/platform/agents-overview.md#log-analytics-agent) yüklemeyi kuyruğa alır. Yay özellikli sunucular yakın zamanda Windows ve Linux için Log Analytics ve bağımlılık Aracısı [VM uzantılarını](../manage-vm-extensions.md) yüklemeye yönelik destek sunarak Azure Izleyici 'nin Azure dışı VM 'lerinize veri toplamasını olanaklı bir şekilde sunmuştur.

Bu öğreticide, basit bir adım kümesini izleyerek VM'ler için Azure İzleyici etkinleştirerek Linux veya Windows sanal makinelerinizdeki verileri nasıl yapılandıracağınızı ve toplayacağınızı ve bu deneyim daha kısa bir süre sürer.  

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* VM Uzantısı işlevselliği yalnızca [Desteklenen bölgeler](../overview.md#supported-regions)listesinde kullanılabilir.

* Etkinleştirmiş olduğunuz VM işletim sisteminin VM'ler için Azure İzleyici tarafından desteklendiğinden emin olmak için [desteklenen işletim sistemleri](../../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın.

* [Log Analytics aracısına genel bakış](../../../azure-monitor/platform/log-analytics-agent.md#network-requirements)bölümünde sunulan Log Analytics aracısına yönelik güvenlik duvarı gereksinimlerini gözden geçirin. VM'ler için Azure İzleyici Map bağımlılık Aracısı herhangi bir veri iletmez ve güvenlik duvarları veya bağlantı noktalarında değişiklik gerektirmez.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="enable-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici etkinleştir

1. **Tüm hizmetler**' e tıklayarak ve ardından **makineler-Azure yay**' i arayıp seçerek Azure yay hizmetini Azure Portal başlatın.

    :::image type="content" source="./media/quick-enable-hybrid-vm/search-machines.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

1. **Makineler-Azure yay** sayfasında [hızlı başlangıç](quick-enable-hybrid-vm.md) makalesinde oluşturduğunuz bağlı makineyi seçin.

1. **İzleme** bölümünün altındaki sol bölmeden **Öngörüler** ' i seçin ve ardından öğesini **etkinleştirin**.

    :::image type="content" source="./media/tutorial-enable-vm-insights/insights-option.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

1. Azure Izleyici **öngörüleri ekleme** sayfasında, bir çalışma alanı oluşturmanız istenir. Bu öğreticide, zaten varsa, var olan bir Log Analytics çalışma alanı seçmenizi önermiyoruz. Kayıtlı bağlı makineniz ile aynı bölgede benzersiz ada sahip bir çalışma alanı olan varsayılanı seçin. Bu çalışma alanı oluşturulur ve sizin için yapılandırılır.

    :::image type="content" source="./media/tutorial-enable-vm-insights/enable-vm-insights.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

1. Yapılandırma gerçekleştirilirken durum iletileri alırsınız. Bu işlem, uzantıların bağlı makinenize yüklendiği birkaç dakika sürer.

    :::image type="content" source="./media/tutorial-enable-vm-insights/onboard-vminsights-vm-portal-status.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

    Bu tamamlandığında, makinenin başarıyla eklendi olduğunu ve öngörüleri başarıyla dağıtıldığını belirten bir ileti alırsınız.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin

Dağıtım ve yapılandırma tamamlandıktan sonra **Öngörüler**' i seçin ve ardından **performans** sekmesini seçin. Performans sekmesinde, sanal makinenizin Konuk işletim sisteminden toplanan bir grup performans sayacını gösterir. Daha fazla sayaç görüntülemek için aşağı kaydırın ve Log Analytics VM uzantısının makinede yüklendiği zamandan itibaren başlayan ortalama ve yüzdebirlik değeri görüntülemek için fareyi grafiğin üzerine taşıyın.

:::image type="content" source="./media/tutorial-enable-vm-insights/insights-performance-charts.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

Sanal makinede çalışan işlemlerin ve bağımlılıklarını gösteren haritalar özelliğini açmak için **eşle** ' yi seçin. Henüz açık değilse Özellik bölmesini açmak için **Özellikler** ' i seçin.

:::image type="content" source="./media/tutorial-enable-vm-insights/insights-map.png" alt-text="Tüm hizmetlerde yay etkin sunucular için arama yapın" border="false":::

Sanal makineniz için süreçler ' ı genişletin. Ayrıntılarını görüntülemek ve bağımlılıklarını vurgulamak için işlemlerden birini seçin.

Sanal makinenizi yeniden seçin ve ardından **günlük olayları**' nı seçin. Sanal makinenin Log Analytics çalışma alanında depolanan tabloların listesini görürsünüz. Bu liste, bir Windows veya Linux sanal makinesi kullanıyor olmanıza bağlı olarak farklı olacaktır. **Olay** tablosunu seçin. **Olay** tablosu, Windows olay günlüğü 'ndeki tüm olayları içerir. Log Analytics, toplanan olay günlüğü girişlerini almak için basit bir sorgu ile açılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Azure İzleyici'ye genel bakış](../../../azure-monitor/overview.md)