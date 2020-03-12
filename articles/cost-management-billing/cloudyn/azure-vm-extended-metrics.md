---
title: Azure sanal makineleri için genişletilmiş ölçüm ekleme | Microsoft Docs
description: Bu makale, Azure VM'leriniz için genişletilmiş tanılama ölçümlerini etkinleştirmenize ve yapılandırmanıza yardımcı olur.
keywords: ''
author: bandersmsft
ms.reviewer: vitavor
ms.author: banders
ms.date: 01/24/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.custom: seodec18
ms.openlocfilehash: 84a9d6aa6203b8a518b0e33bed0ec2707c4389a1
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79082923"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Azure sanal makineleri için genişletilmiş ölçüm ekleme

Cloudyn, Azure VM'lerin kaynakları hakkında ayrıntılı bilgiler sunmak için Azure ölçüm verilerini kullanır. Cloudyn, rapor oluşturmak için performans sayacı olarak da bilinen ölçüm verilerini kullanır. Ancak Cloudyn, konuk VM'lerden tüm Azure ölçüm verilerini otomatik olarak alamaz. Ölçüm toplama işlevini etkinleştirmeniz gerekir. Bu makale, Azure VM'leriniz için ek tanılama ölçümlerini etkinleştirmenize ve yapılandırmanıza yardımcı olur.

Ölçüm toplamayı etkinleştirdikten sonra şunları yapabilirsiniz:

- VM'leriniz bellek, disk ve CPU sınırlarına yaklaştığında bildirim alma.
- Kullanım eğilimlerini ve anomalilerini tespit etme.
- Kullanıma göre boyutlandırarak maliyetlerinizi kontrol altına alma.
- Cloudyn'den maliyet açısından etkili boyut iyileştirme önerileri alma.

Örneğin Azure VM'lerinizin CPU ve Bellek yüzdesini izlemek isteyebilirsiniz. Azure ölçümleri, _CPU Yüzdesi_ ve _\Bellek\% Kullanımdaki Kaydedilmiş Baytlar_ değerlerini verir.

> [!NOTE]
> Genişletilmiş ölçüm veri koleksiyonu yalnızca Azure konuk seviyesinde izleme için desteklenir. Cloudyn, [Log Analytics aracısıyla](../../azure-monitor/platform/agents-overview.md) uyumlu değildir. 

## <a name="determine-whether-extended-metrics-are-enabled"></a>Genişletilmiş ölçümlerin etkin olup olmadığını belirleme

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. **Sanal makineler**'in altında bir VM seçtikten sonra **İzleme** bölümünde **Ölçümler**'i seçin. Kullanılabilir ölçümlerin listesi gösterilir.
3. Ölçümlerden birkaç tanesini seçtiğinizde veriler grafik biçiminde görüntülenir.  
    ![Örnek ölçüm - ana bilgisayar CPU yüzdesi](./media/azure-vm-extended-metrics/metric01.png)

Yukarıdaki örnekte ana bilgisayarlarınız için sınırlı bir standart ölçüm kümesi mevcuttur ancak bellek ölçümleri yoktur. Bellek ölçümleri, genişletilmiş ölçümlerin bir parçasıdır. Bu durum, VM'de genişletilmiş ölçümlerin etkin olmadığını gösterir. Genişletilmiş ölçümleri etkinleştirmek için bazı ek adımlar gerçekleştirmeniz gerekir. Aşağıdaki bilgiler bu konuda size yol gösterecektir.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Genişletilmiş ölçümleri Azure portalında etkinleştirme

Standart ölçümler ana bilgisayar ölçümleridir. Örnek olarak _CPU yüzdesi_ verilebilir. Konuk VM'ler için temel ölçümler de mevcuttur ve bunlar da genişletilmiş ölçüm olarak adlandırılır. Genişletilmiş ölçümlere örnek olarak _\Bellek\% Kullanımdaki Kaydedilmiş Baytlar_ ve _\Bellek\Kullanılabilir Baytlar_ verilebilir.

Genişletilmiş ölçümleri etkinleştirmek oldukça kolaydır. Her VM için konuk düzeyinde izlemeyi etkinleştirmeniz gerekir. Konuk düzeyinde izlemeyi etkinleştirdiğinizde VM'ye Azure tanılama aracısı yüklenir. Varsayılan olarak, temel bir genişletilmiş ölçüm kümesi eklenir. Aşağıdaki işlem, klasik ve normal VM'ler için aynıdır ve hem Windows hem de Linux VM'ler için geçerlidir.

Hem Azure hem de Linux konuk düzeyinde izleme için depolama hesabı gerektiğini unutmayın. Konuk düzeyinde izlemeyi etkinleştirdiğinizde var olan bir depolama hesabını seçmezseniz, sizin için bir depolama hesabı oluşturulur.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Mevcut VM'lerde konuk düzeyinde izlemeyi etkinleştirme

1. **Sanal Makineler**'de VM listenizi görüntüleyin ve VM'lerden birini seçin.
2. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.
3. Tanılama ayarları sayfasında **Konuk düzeyinde izlemeyi etkinleştir**'e tıklayın.  
    ![Genel Bakış sayfasındaki Konuk düzeyinde izlemeyi etkinleştir seçeneği](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
4. Birkaç dakika içinde VM'ye Azure tanılama aracısı yüklenir. Temel ölçüm kümesi eklenir. Sayfayı yenileyin. Eklenen performans sayaçları Genel Bakış sekmesinde görünür.
5. İzleme'nin altında **Ölçümler**'i seçin.
6. **Ölçüm Ad Alanı**'nın altındaki ölçümler grafiğinde **Konuk (Klasik)** seçimini yapın.
7. Ölçüm listesinde konuk VM'nin kullanılabilir performans sayaçlarının tümünü görüntüleyebilirsiniz.  
    ![örnek genişletilmiş ölçümler listesi](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Yeni VM'lerde konuk düzeyinde izlemeyi etkinleştirme

Yeni VM oluşturduğunuzda Yönetim sekmesinde **İşletim sistemi konuk tanılaması** ayarını **Açık** olarak belirleyin.

![İşletim sistemi konuk tanılamasını açın](./media/azure-vm-extended-metrics/new-enable-diag.png)

Azure sanal makineleri için genişletilmiş ölçümleri etkinleştirme hakkında daha fazla bilgi için bkz. [Azure Linux aracısını anlama ve kullanma](../../virtual-machines/extensions/agent-linux.md) ve [Azure Sanal Makine Aracısı'na genel bakış](../../virtual-machines/extensions/agent-windows.md).

## <a name="resource-manager-credentials"></a>Resource Manager kimlik bilgileri

Genişletilmiş ölçümleri etkinleştirdikten sonra Cloudyn'in [Resource Manager kimlik bilgilerinize](../../cost-management/activate-subs-accounts.md) erişimi olduğundan emin olun. Cloudyn'in VM'lerinize ait performans verilerini toplaması ve görüntülemesi için kimlik bilgileriniz gereklidir. Bu bilgiler, maliyet iyileştirme önerileri oluşturmak için de kullanılır. Cloudyn, bir örneğin boyutunu küçültme önerilerine uygun bir aday olup olmadığını belirlemek için en az üç günlük performans verilerine ihtiyaç duyar.

## <a name="enable-vm-metrics-with-a-script"></a>VM ölçümlerini betik kullanarak etkinleştirme

VM ölçümlerini Azure PowerShell betikleriyle etkinleştirebilirsiniz. Ölçümleri etkinleştirmek istediğiniz çok sayıda VM varsa betik kullanarak süreci otomatikleştirebilirsiniz. Örnek betikleri GitHub'ın [Azure Enable Diagnostics](https://github.com/Cloudyn/azure-enable-diagnostics) sayfasında bulabilirsiniz.

## <a name="view-azure-performance-metrics"></a>Azure performans ölçümlerini görüntüleme

Cloudyn portalında Azure örneklerinizin performans ölçümlerini görüntülemek için **Varlıklar** > **İşlem** > **Örnek Gezgini** yolunu izleyin. VM örnekleri listesinde bir örnek seçip genişleterek ayrıntılarını görüntüleyebilirsiniz.

![Örnek Gezgini'nde gösterilen örnek bilgiler](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınız için Azure Resource Manager API’si erişimini etkinleştirmediyseniz bkz. [Azure aboneliklerini ve hesaplarını etkinleştirme](../../cost-management/activate-subs-accounts.md).
