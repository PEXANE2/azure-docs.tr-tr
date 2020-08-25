---
title: Sanal makinelerin olağanüstü durum kurtarmasını tamamlanma
description: Bu makalede, Azure VMware çözümü kullanılarak sanal makinelerin olağanüstü durum kurtarmasının nasıl tamamlanacağı gösterilmektedir
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: 76a417b9ba00c4c0e6e958e5a04d19aecfe24563
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752273"
---
# <a name="complete-a-disaster-recovery-of-virtual-machines-using-azure-vmware-solution"></a>Azure VMware çözümünü kullanarak sanal makinelerin olağanüstü durum kurtarmasını tamamlanma

Bu makale, VMware karma bulut uzantısı (HCX) çözümü ile sanal makinelerinizin olağanüstü durum kurtarma işlemini tamamlamaya ve kurtarma veya hedef site olarak bir Azure VMware Çözüm özel bulutu kullanmaya yönelik süreci içerir.

VMware HCX, çoğaltma ilkelerinde ince denetim ve ayrıntı düzeyi sağlayan çeşitli işlemler sağlar. Kullanılabilir Işlemler şunları içerir:

- Ters: olağanüstü bir durum meydana geldi. Ters çevirme, site B 'nin, korunan VM 'nin artık yaşadığı kaynak site ve site için olmasına yardımcı olur.

- Duraklat – seçili sanal makine ile ilişkili geçerli çoğaltma ilkesini duraklatın.

- Resume-seçili sanal makine ile ilişkili geçerli çoğaltma ilkesini duraklatın.

- Kaldır-Seçilen sanal makineyle ilişkili geçerli çoğaltma ilkesini kaldırın.

- Şimdi Eşitle – sınırlı eşitleme kaynak sanal makinesini korunan VM 'ye kalmadı.

Bu kılavuzda aşağıdaki çoğaltma senaryoları ele alınmıştır:

- Bir VM 'yi veya bir VM grubunu koruyun.

- Bir sanal makinenin veya bir VM grubunun bir test kurtarmasına ilişkin bir sınama testi yapın.

- Bir VM 'yi veya bir VM grubunu kurtarın.

- Bir VM 'nin veya bir VM grubunun korumasını tersine çevirin.

## <a name="protect-virtual-machines"></a>Sanal makineleri koruma

Kaynak sitede **vSphere istemcisinde** oturum açın ve **HCX eklentisine**erişin.

:::image type="content" source="./media/disaster-recovery/hcx-vsphere.png" alt-text="VSphere içinde HCX seçeneği" border="true":::

**Olağanüstü durum kurtarma** alanını girin ve **VM 'leri koru**' ya tıklayın.

:::image type="content" source="./media/disaster-recovery/protect-virtual-machine.png" alt-text="VM 'leri koru seçeneğini belirleyin" border="true":::

Açılan pencerede kaynağı ve uzak siteleri seçin, bu durumda uzak site Azure VMware Çözüm özel bulutu olmalıdır.

:::image type="content" source="./media/disaster-recovery/protect-virtual-machines.png" alt-text="VM 'Leri koruma penceresi" border="true":::

Gerekirse, varsayılan çoğaltma seçeneklerini belirleyin:

- **Sıkıştırmayı etkinleştir:** Düşük aktarım hızı senaryoları için önerilir.

- **Quiescence etkinleştir:** Uzak siteyle tutarlı bir kopya eşitlendiğinden emin olmak için sanal makineyi duraklatır.

- **Hedef depolama alanı:** Korunan VM 'ler için uzak veri deposunu seçin. Azure VMware Çözüm özel bulutu 'nda bu seçim vSAN veri deposu olmalıdır.

- **Işlem kapsayıcısı:** Uzak vSphere kümesi veya kaynak havuzu.

- **Hedef klasör:** Uzak hedef klasörü, bu ayar isteğe bağlıdır ve hiçbir klasör seçilmezse VM 'ler doğrudan seçilen kümenin altında ilerlecektir.

- **RPO:** Bu değer, kaynak sanal makine ile korunan sanal makine arasındaki eşitleme aralığıdır ve 5 dakikadan ila 24 saat arasında bir süre olabilir.

- **Anlık görüntü aralığı:** Anlık görüntüler arasındaki Aralık.

- **Anlık görüntü sayısı:** Yapılandırılan anlık görüntü aralığı içindeki toplam anlık görüntü sayısı.

:::image type="content" source="./media/disaster-recovery/protect-virtual-machine-options.png" alt-text="sanal makine seçeneklerini koruma" border="true":::

Listeden bir veya daha fazla sanal makine seçin ve sanal makineyi çoğaltma seçeneklerini gerektiği şekilde yapılandırın.

Varsayılan olarak, sanal makineler varsayılan çoğaltma seçeneklerinde yapılandırılan genel ayarlar Ilkesini alır. Seçili VM 'deki her bir ağ arabirimi için, uzak **ağ bağlantı noktası grubunu** yapılandırın ve **son** ' u seçerek koruma sürecini başlatın.

:::image type="content" source="./media/disaster-recovery/network-interface-options.png" alt-text="Ağ arabirimi seçenekleri" border="true":::

Aşağıdaki görüntüde görüldüğü gibi, aynı olağanüstü durum kurtarma alanındaki seçili sanal makinelerin her biri için işlemi izleyebilirsiniz.

:::image type="content" source="./media/disaster-recovery/protect-monitor-progress.png" alt-text="korumanın ilerlemesini izleme" border="true":::

VM korunduktan sonra, **anlık** görüntüler sekmesinde farklı anlık görüntüler görülebilir.

:::image type="content" source="./media/disaster-recovery/list-of-snapshots.png" alt-text="anlık görüntülerin listesi" border="true":::

Sarı üçgen, anlık görüntüler ve sanal bir sınama kurtarma işleminde sınanmamış anlamına gelir.

Kapalı olan ve çalıştıran bir VM arasında önemli farklar vardır.
Önceki ekran görüntüsünde, sanal makine üzerinde güç için eşitleme işlemi gösterilmektedir. VM 'nin tam bir kopyası olan ilk anlık görüntüyü tamamlayana kadar eşitleme işlemini başlattı ve sonra yapılandırılan aralıkta bir sonraki işlemleri tamamlar.

Kapalı bir sanal makine için, bir kopyayı eşitler ve ardından VM etkin değil olarak görünür ve koruma işlemi tamamlandı olarak gösterilir.

Sanal makine açık olduğunda, uzak siteye eşitleme işlemini başlatır.

## <a name="complete-a-test-recover-of-virtual-machines"></a>Sanal makinelerin bir test kurtarmalarını doldurun

Azure VMware çözümü özel bulutu olan uzak sitede **vSphere istemcisinde** oturum açın. **HCX eklentisi**Içinde, olağanüstü durum kurtarma alanında, işlemler menüsünü göstermek için HERHANGI bir VM 'deki dikey üç nokta simgesini seçin. **Test kurtarma VM 'yi**seçin.

:::image type="content" source="./media/disaster-recovery/test-recover-virtual-machine.png" alt-text="Test kurtarma VM 'sini seçin" border="true":::

Yeni pencerede, test seçeneklerini belirleyin. Sanal makinenin farklı durumlarını test etmek için kullanmak istediğiniz anlık görüntüyü seçin.

:::image type="content" source="./media/disaster-recovery/choose-snapshot.png" alt-text="bir anlık görüntü seçin ve test 'i tıklatın" border="true":::

**Teste**tıkladıktan sonra kurtarma işlemi başlar.

Sınama kurtarma işlemi tamamlandığında, yeni VM Azure VMware çözümü özel bulut vCenter ' da denetlenebilir.

:::image type="content" source="./media/disaster-recovery/verify-test-recovery.png" alt-text="kurtarma işlemini denetle" border="true":::

Son olarak, VM üzerinde test yapıldıktan veya üzerinde çalışan herhangi bir uygulamayla, test örneğini silmek için bir temizleme işlemi yapın.

:::image type="content" source="./media/disaster-recovery/cleanup-test-instance.png" alt-text="test örneğini temizle" border="true":::

## <a name="recover-virtual-machines"></a>Sanal makineleri kurtarma

Azure VMware çözümü özel bulutu olan uzak sitede **vSphere istemcisinde** oturum açın ve **HCX eklentisine**erişin.

Kurtarma senaryosunda, bu örnek için kullanılan sanal makine grubu.

Listeden kurtarılacak sanal makineyi seçin, **Eylemler** menüsünü açın ve **VM 'leri kurtar**' ı seçin.

:::image type="content" source="./media/disaster-recovery/recover-virtual-machines.png" alt-text="sanal makineleri kurtarma" border="true":::

Her örnek için kurtarma seçeneklerini yapılandırın ve kurtarma işlemini başlatmak için **kurtar** 'a tıklayın.

:::image type="content" source="./media/disaster-recovery/recover-virtual-machines-confirm.png" alt-text="sanal makineleri kurtarma onayı" border="true":::

Kurtarma işlemi tamamlandıktan sonra, yeni VM 'Ler uzak vCenter Server envanterinde görünür.

## <a name="complete-a-reverse-replication-on-virtual-machines"></a>Sanal makinelerde ters çoğaltmayı tamamlar

Azure VMware çözümünüz özel bulutunuzda **vSphere istemcisinde** oturum açın ve **HCX eklentisine**erişin.
Ters çoğaltmaya başlamadan önce kaynak sitedeki özgün sanal makinelerin kapalı olması gerekir. Sanal makineler kapalıyken işlem başarısız olur.

Kaynak siteye geri çoğaltılacak sanal makineleri listeden seçin, **Eylemler** menüsünü açın ve **ters**' yı seçin. Açılır pencerede çoğaltmayı başlatmak için **ters** ' ye tıklayın.

:::image type="content" source="./media/disaster-recovery/reverse-operation-virtual-machines.png" alt-text="Koruma işlemleri altında tersine eylem ' i seçin" border="true":::

Çoğaltma, her bir sanal makinenin Ayrıntılar bölümünde izlenebilir.

:::image type="content" source="./media/disaster-recovery/review-reverse-operation.png" alt-text="ters işlemin sonuçlarını gözden geçirme" border="true":::

## <a name="disaster-recovery-plan-automation"></a>Olağanüstü durum kurtarma planı Otomasyonu

VMware HCX Şu anda bir olağanüstü durum kurtarma planı oluşturmak ve otomatikleştirmek için yerleşik bir mekanizmaya sahip değildir. Bu özellik HCX içinde yok. Ancak, olağanüstü durum kurtarma işleminin API 'Leri de dahil olmak üzere bir dizi REST API 'si sağlar.

API belirtimine URL içindeki HCX Yöneticisi içinde erişilebilir.

Olağanüstü durum kurtarma 'daki aşağıdaki işlemler bu API 'Ler tarafından ele alınmıştır.

- Koruma

- Kurtar

- Sınama kurtar

- Planlı kurtarma

- Reverse

- Sorgu

- Testi Temizleme

- Duraklat

- Sürdür

- Korumayı kaldır

- Yeniden Yapılandırma

JSON 'da kurtarma işlemi yükünün bir örneği aşağıda gösterilmiştir.

```json
[

    {

        "replicationId": "string",

        "needPowerOn": true,

        "instanceId": "string",

        "source": {

            "endpointType": "string",

            "endpointId": "string",

            "endpointName": "string",

            "resourceType": "string",

            "resourceId": "string",

            "resourceName": "string"

        },

        "destination": {

            "endpointType": "string",

            "endpointId": "string",

            "endpointName": "string",

            "resourceType": "string",

            "resourceId": "string",

            "resourceName": "string"

        },

        "placement": [

            {

                "containerType": "string",

                "containerId": "string"

            }

        ],

        "resourceId": "string",

        "forcePowerOff": true,

        "isTest": true,

        "forcePowerOffAfterTimeout": true,

        "isPlanned": true

    }

]
```

Bu API 'Leri kullanarak bir müşteri olağanüstü durum kurtarma planının oluşturulmasını ve yürütülmesini otomatikleştirmek için özel bir mekanizma oluşturabilir.
