---
title: Bulut Dökümünü izlemek için Azure Günlük Analizi Nozulu'nun dağıtım
description: Azure Günlük Analitiği için Cloud Foundry loggregator Nozzle'ı dağıtma konusunda adım adım kılavuz. Bulut Döküm sistemi durumunu ve performans ölçümlerini izlemek için Nozul'u kullanın.
services: virtual-machines-linux
author: ningk
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: bf6691310ec964a1d6293f3a60c151e3d6f8e641
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277354"
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Bulut Döküm sistemi izleme için Azure Günlük Analizi Nozulu'nun dağıtım

[Azure Monitor,](https://azure.microsoft.com/services/log-analytics/) Azure'da bir hizmettir. Bulut ve şirket içi ortamlarınızdan oluşturulan verileri toplamanıza ve analiz ekoltenize yardımcı olur.

Log Analytics Nozzle (Nozul), Bulut Döküm kutusu [loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) firehose'undaki ölçümleri Azure Monitor günlüklerine ileten bir Bulut Dökümhanesi (CF) bileşenidir. Nozul ile CF sistem sistem sistem ve performans ölçümlerinizi birden çok dağıtımda toplayabilir, görüntüleyebilir ve analiz edebilirsiniz.

Bu belgede, Nozul'u CF ortamınıza nasıl dağıtabileceğinizi ve ardından Azure Monitor günlükleri konsolundaki verilere nasıl erişirdiğinizi öğreneceksiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki adımlar Nozul dağıtmak için ön koşullardır.

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Azure'da bir CF veya Pivotal Cloud Foundry ortamı dağıtma

Nozul'u açık kaynak CF dağıtımı veya Pivotal Cloud Foundry (PCF) dağıtımı yla kullanabilirsiniz.

* [Azure’da Cloud Foundry dağıtma](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)

* [Azure'da Önemli Bulut Dökümhane'yi dağıtma](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. Nozul'u dağıtmak için CF komut satırı araçlarını yükleyin

Nozul CF ortamında bir uygulama olarak çalışır. Uygulamayı dağıtmak için CF CLI'ye ihtiyacınız var.

Nozul'un loggregator firehose ve Cloud Controller'a erişim izni ne de olması gerekir. Kullanıcıyı oluşturmak ve yapılandırmak için Kullanıcı Hesabı ve Kimlik Doğrulama (UAA) hizmetine ihtiyacınız vardır.

* [Bulut Döküm CLI yükle](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

* [Cloud Foundry UAA komut satırı istemcisi yükleme](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md)

UAA command-Line istemcisini kurmadan önce RubyGems'in kurulduğundan emin olun.

### <a name="3-create-a-log-analytics-workspace-in-azure"></a>3. Azure'da Günlük Analizi çalışma alanı oluşturma

Log Analytics çalışma alanını el ile veya şablon kullanarak oluşturabilirsiniz. Şablon, Azure Monitor günlükleri konsolu için önceden yapılandırılmış KPI görünümleri ve uyarılarının bir kurulumünü dağıtacaktır. 

#### <a name="to-create-the-workspace-manually"></a>Çalışma alanını el ile oluşturmak için:

1. Azure portalında, Azure Marketi'ndeki hizmetlerin listesini arayın ve ardından Günlük Analizi çalışma alanlarını seçin.
2. **Oluştur'u**seçin ve ardından aşağıdaki öğeler için seçenekleri seçin:

   * **Günlük Analizi çalışma alanı**: Çalışma alanınız için bir ad yazın.
   * **Abonelik**: Birden çok aboneliğiniz varsa, CF dağıtımınızla aynı olan aboneliği seçin.
   * **Kaynak grubu**: Yeni bir kaynak grubu oluşturabilir veya CF dağıtımınızla aynı kaynak grubunu kullanabilirsiniz.
   * **Yer**: Konumu girin.
   * **Fiyatlandırma katmanı**: Tamamlamak için **Tamam'ı** seçin.

Daha fazla bilgi için Azure [Monitor günlüklerini başlatın'](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)a bakın.

#### <a name="to-create-the-log-analytics-workspace-through-the-monitoring-template-from-azure-market-place"></a>Azure pazar yerinden izleme şablonu aracılığıyla Log Analytics çalışma alanını oluşturmak için:

1. Azure portalını açın.
1. Sol üst köşedeki "+" işaretini veya "Kaynak oluştur"u tıklatın.
1. Arama penceresine "Bulut Dökümhanesi" yazın, "Bulut Dökümünü İzleme Çözümü"nü seçin.
1. Cloud Foundry izleme çözümü şablonu ön sayfası yüklenir, şablon bıçağını başlatmak için "Oluştur"a tıklayın.
1. Gerekli parametreleri girin:
    * **Abonelik**: Log Analytics çalışma alanı için genellikle Bulut Dökümkutusu dağıtımında da aynı olan bir Azure aboneliği seçin.
    * **Kaynak grubu**: Varolan bir kaynak grubu seçin veya Log Analytics çalışma alanı için yeni bir grup oluşturun.
    * **Kaynak Grubu Konumu**: Kaynak grubunun konumunu seçin.
    * **OMS_Workspace_Name**: Çalışma alanı yoksa, şablon yeni bir çalışma alanı adı girin.
    * **OMS_Workspace_Region**: Çalışma alanının konumunu seçin.
    * **OMS_Workspace_Pricing_Tier**: Log Analytics çalışma alanı SKU'yu seçin. Başvuru için [fiyatlandırma kılavuzuna](https://azure.microsoft.com/pricing/details/log-analytics/) bakın.
    * **Yasal terimler**: Yasal terimleri tıklatın, ardından yasal terimi kabul etmek için "Oluştur"a tıklayın.
1. Tüm parametreler belirtildikten sonra, şablonu dağıtmak için "Oluştur"u tıklatın. Dağıtım tamamlandığında, durum bildirim sekmesinde gösterilecek.


## <a name="deploy-the-nozzle"></a>Nozulu Dağıt

Nozul'u dağıtmanın birkaç farklı yolu vardır: PCF döşemesi olarak veya CF uygulaması olarak.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>Nozulu PCF Ops Manager döşemesi olarak dağıtın

[PCF için Azure Log Analytics Nozul'u yüklemek ve yapılandırmak için](https://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html)adımları izleyin. Bu basitleştirilmiş bir yaklaşımdır, PCF Ops yöneticisi döşemeotomatik olarak yapılandırAcak ve meme itecektir. 

### <a name="deploy-the-nozzle-manually-as-a-cf-application"></a>Nozul'u CF uygulaması olarak el ile dağıtma

PCF Ops Manager kullanmıyorsanız, Nozul'u bir uygulama olarak dağıtın. Aşağıdaki bölümlerde bu işlem açıklayınız.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>CF CLI aracılığıyla cf dağıtımınızda yönetici olarak oturum açın

Şu komutu çalıştırın:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

"SYSTEM_DOMAIN" CF alan adınızdır. CF dağıtım bildirim dosyanızdaki "SYSTEM_DOMAIN" aramasını yapabilirsiniz. 

"CF_User" CF yönetici adıdır. CF dağıtım bildirim dosyanızdaki adı ve "cf_admin_password" ibaresini arayarak ad ve şifreyi "scim" bölümünde arayarak alabilirsiniz.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>Bir CF kullanıcısı oluşturun ve gerekli ayrıcalıkları tanıyın

Aşağıdaki komutları çalıştırın:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

"SYSTEM_DOMAIN" CF alan adınızdır. CF dağıtım bildirim dosyanızdaki "SYSTEM_DOMAIN" aramasını yapabilirsiniz.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>En son Log Analytics Nozzle yayınını indirin

Şu komutu çalıştırın:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Şimdi geçerli dizininizdeki manifest.yml dosyasında ortam değişkenlerini ayarlayabilirsiniz. Aşağıda Nozul için uygulama bildirimi gösterilmektedir. Değerleri özel Log Analytics çalışma alanı bilgilerinizle değiştirin.

```
OMS_WORKSPACE             : Log Analytics workspace ID: Open your Log Analytics workspace in the Azure portal, select **Advanced settings**, select **Connected Sources**, and select **Windows Servers**.
OMS_KEY                   : OMS key: Open your Log Analytics workspace in the Azure portal, select **Advanced settings**, select **Connected Sources**, and select **Windows Servers**.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to Azure Monitor logs. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to Azure Monitor logs. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to Azure Monitor logs. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to Azure Monitor logs as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to Azure Monitor logs. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>Uygulamayı geliştirme bilgisayarınızdan itin

Oms-log-analytics-firehose-nozul klasörü altında olduğundan emin olun. Şu komutu çalıştırın:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>Nozul yüklemesini doğrulama

### <a name="from-apps-manager-for-pcf"></a>Apps Manager'dan (PCF için)

1. Ops Manager'da oturum açın ve döşemenin yükleme panosunda görüntülendiğinden emin olun.
2. Apps Yöneticisi'nde oturum açın, Nozul için oluşturduğunuz alanın kullanım raporunda listelenmiş olduğundan emin olun ve durumun normal olduğunu onaylayın.

### <a name="from-your-development-computer"></a>Geliştirme bilgisayarınızdan

CF CLI penceresinde şunları yazın:
```
cf apps
```
OMS Nozul uygulamasının çalıştığını unutmayın.

## <a name="view-the-data-in-the-azure-portal"></a>Azure portalındaki verileri görüntüleme

İzleme çözümünü pazar yeri şablonu üzerinden dağıttıysanız, Azure portalına gidin ve çözümü bulun. Çözümü şablonda belirttiğiniz kaynak grubunda bulabilirsiniz. Çözüme tıklayın, "günlük analizi konsoluna" göz atın, önceden yapılandırılmış görünümler en iyi Bulut Döküm sistemi KP'leri, uygulama verileri, uyarılar ve VM sistem ölçümleri ile listelenir. 

Log Analytics çalışma alanını el ile oluşturduysanız, görünümleri ve uyarıları oluşturmak için aşağıdaki adımları izleyin:

### <a name="1-import-the-oms-view"></a>1. OMS görünümünü alma

OMS portalından Tasarımcı**Alma** > **Gözat'ını** **Görüntüle'ye** > göz atın ve omsview dosyalarından birini seçin. Örneğin, *Cloud Foundry.omsview'i*seçin ve görünümü kaydedin. Şimdi **Genel Bakış** sayfasında bir döşeme görüntülenir. Görselleştirilmiş ölçümleri görmek için seçin.

Bu görünümleri özelleştirebilir veya **Tasarımcıyı Görüntüle**aracılığıyla yeni görünümler oluşturabilirsiniz.

*"Cloud Foundry.omsview",* Cloud Foundry OMS görünüm şablonunun önizleme sürümüdür. Bu, tamamen yapılandırılmış varsayılan bir şablondur. Şablon hakkında önerileriniz veya geri bildiriminiz varsa, bunları [sorun bölümüne](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues)gönderin.

### <a name="2-create-alert-rules"></a>2. Uyarı kuralları oluşturma

Uyarıları [oluşturabilir](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts)ve gerektiğinde sorguları ve eşik değerlerini özelleştirebilirsiniz. Aşağıdaki uyarılar önerilir:

| Arama sorgusu                                                                  | Temelalan uyarı oluşturma | Açıklama                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type=CF_ValueMetric_CL Origin_s=bbs Name_s="Domain.cf-apps"                   | Sonuç sayısı < 1   | **bbs. Domain.cf uygulamalar** cf-apps Etki Alanı'nın güncel olup olmadığını gösterir. Bu, Bulut Denetleyicisi'nden gelen CF App isteklerinin bbs ile senkronize edildiği anlamına gelir. LRPsDesired (Diego-istenen AIs) yürütme için. Alınan hiçbir veri, cf-apps Etki Alanı'nın belirtilen zaman penceresinde güncel olmadığı anlamına gelir. |
| Tip=CF_ValueMetric_CL Origin_s=rep Name_s=SağlıksızHücre Value_d>1            | Sonuç sayısı > 0   | Diego hücreleri için 0 sağlıklı, 1 ise sağlıksız anlamına gelir. Belirtilen zaman penceresinde birden çok sağlıksız Diego hücresi algılanırsa uyarıyı ayarlayın. |
| Tip=CF_ValueMetric_CL Origin_s="bosh-hm-forwarder" Name_s="system.healthy" Value_d=0 | Sonuç sayısı > 0 | 1, sistemin sağlıklı, 0'ın ise sistemin sağlıklı olmadığı anlamına gelir. |
| Type=CF_ValueMetric_CL Origin_s=route_emitter Name_s=ConsulDownMode Value_d>0 | Sonuç sayısı > 0   | Konsolos sağlık durumunu periyodik olarak yayır. 0, sistemin sağlıklı olduğu anlamına gelir ve 1 de rota yayımlayıcısının Konsolos'un çöktüklerini algıladığını gösterir. |
| Type=CF_CounterEvent_CL Origin_s=DopplerServer (Name_s="TruncatingBuffer.DropdMessages" veya Name_s="doppler.shedEnvelopes") Delta_d>0 | Sonuç sayısı > 0 | Doppler tarafından arka basınç nedeniyle kasten bırakılan iletilerin delta sayısı. |
| Tür=CF_LogMessage_CL SourceType_s=LGR MessageType_s=ERR                      | Sonuç sayısı > 0   | Loggregator günlük işlemi ile ilgili sorunları belirtmek için **LGR** yalar. Böyle bir soruna örnek olarak, günlük iletisi çıktısının çok yüksek olması dır. |
| Type=CF_ValueMetric_CL Name_s=slowConsumerAlert                               | Sonuç sayısı > 0   | Nozul loggregator yavaş bir tüketici uyarısı aldığında, **yavaş ConsumerAlert** ValueMetric'i Azure Monitor günlüklerine gönderir. |
| Type=CF_CounterEvent_CL Job_s=nozul Name_s=olaylarKayıp Delta_d>0              | Sonuç sayısı > 0   | Kaybolan olayların delta sayısı bir eşiğe ulaşırsa, Bu Nozul çalışan bir sorun olabilir anlamına gelir. |

## <a name="scale"></a>Ölçek

Nozul'u ve loggregator'ı ölçeklendirebilirsiniz.

### <a name="scale-the-nozzle"></a>Nozulu Ölçeklendir

Nozul'un en az iki örneğiyle başlamalısınız. İtfaiye hortumu, iş yükünü Nozul'un tüm örneklerine dağıtır.
Nozul'un yangın hortumundaki veri trafiğine ayak uydurabilmesini sağlamak için **slowConsumerAlert** uyarısını ayarlayın (önceki bölümde "Uyarı kuralları oluşturma" bölümünde listelenmiştir). Uyarılandıktan sonra, ölçekleme gerekip gerekmediğini belirlemek [için yavaş Nozul kılavuzunu](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz) izleyin.
Nozul'u büyütmek için, Örnek numaralarını veya Nozul'un bellek veya disk kaynaklarını artırmak için Apps Yöneticisi'ni veya CF CLI'yi kullanın.

### <a name="scale-the-loggregator"></a>Loggregator ölçeklendirin

Loggregator günlük işlemi ile ilgili sorunları belirtmek için bir **LGR** günlük iletisi gönderir. Loggregator kadar ölçeklendirilmesi gerekip gerekmediğini belirlemek için uyarıiz izleyebilirsiniz.
Loggregator'ı büyütmek için, Doppler arabellek boyutunu artırın veya CF bildirimine ek Doppler sunucu örnekleri ekleyin. Daha fazla bilgi [için loggregator ölçekleme kılavuzuna](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling)bakın.

## <a name="update"></a>Güncelleştirme

Nozul'u daha yeni bir sürümle güncelleştirmek için yeni Nozul sürümünü indirin, önceki "Nozul'u dağıt" bölümündeki adımları izleyin ve uygulamayı yeniden itin.

### <a name="remove-the-nozzle-from-ops-manager"></a>Nozulu Ops Yöneticisi'nden kaldırın

1. Ops Manager'da oturum açın.
2. PCF döşemesi **için Microsoft Azure Log Analytics Nozul'u** bulun.
3. Çöp simgesini seçin ve silme işlemini onaylayın.

### <a name="remove-the-nozzle-from-your-development-computer"></a>Nozulu geliştirme bilgisayarınızdan kaldırma

CF CLI pencerenize şunları yazın:
```
cf delete <App Name> -r
```

Nozul'u kaldırırsanız, OMS portalındaki veriler otomatik olarak kaldırılmaz. Azure Monitor günlükleri bekletme ayarınızın süresi doluyor.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure Log Analytics Nozul açık kaynaklıdır. Sorularınızı ve geri bildirimlerinizi [GitHub bölümüne](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues)gönderin. Azure destek isteği açmak için hizmet kategorisi olarak "Bulut Dökümhanesi çalıştıran Sanal Makine"yi seçin. 

## <a name="next-step"></a>Sonraki adım

PCF2.0'dan, VM performans ölçümleri Sistem Ölçümleri Forwarder tarafından Azure Log Analytics nozula aktarılır ve Log Analytics çalışma alanına entegre edilir. VM performans ölçümleri için artık Log Analytics temsilcisine ihtiyacınız yoktur. Ancak, Syslog bilgilerini toplamak için Log Analytics aracısını kullanmaya devam edebilirsiniz. Log Analytics aracısı CF VM'lerinize Bosh eklentisi olarak yüklenir. 

Ayrıntılar için, [Bulut Döküm kutusu dağıtımınıza Günlük Analizi aracısını dağıtın bölümüne](https://github.com/Azure/oms-agent-for-linux-boshrelease)bakın.
