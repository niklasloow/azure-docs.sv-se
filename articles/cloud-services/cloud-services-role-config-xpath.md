---
title: Cloud Services-lathund för roll konfiguration | Microsoft Docs
description: De olika XPath-inställningar som du kan använda i konfigurationen för Cloud Service-rollen för att Visa inställningar som en miljö variabel.
services: cloud-services
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 04/19/2017
ms.author: tagore
ms.openlocfilehash: 380b0be4e4e4b19d16cb611b0b472294339f2199
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "75386093"
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Exponera roll konfigurations inställningar som en miljö variabel med XPath
I moln tjänstens eller webb rollens tjänst definitions fil kan du Visa konfigurations värden för körnings miljön som miljövariabler. Följande XPath-värden stöds (som motsvarar API-värden).

Dessa XPath-värden är också tillgängliga via biblioteket [Microsoft. windowsazure. ServiceRuntime](/previous-versions/azure/reference/ee773173(v=azure.100)) . 

## <a name="app-running-in-emulator"></a>App som körs i emulatorn
Anger att appen körs i emulatorn.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = " /RoleEnvironment/Deployment/@emulated " |
| Kod |var x = RoleEnvironment. IsEmulated; |

## <a name="deployment-id"></a>Distributions-ID
Hämtar distributions-ID för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = " /RoleEnvironment/Deployment/@id " |
| Kod |var deploymentId = RoleEnvironment. DeploymentId; |

## <a name="role-id"></a>Roll-ID
Hämtar det aktuella roll-ID: t för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = " /RoleEnvironment/CurrentInstance/@id " |
| Kod |var-ID = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Uppdatera domän
Hämtar uppdaterings domänen för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = " /RoleEnvironment/CurrentInstance/@updateDomain " |
| Kod |var UD = RoleEnvironment. CurrentRoleInstance. UpdateDomain; |

## <a name="fault-domain"></a>Feldomän
Hämtar fel domänen för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = " /RoleEnvironment/CurrentInstance/@faultDomain " |
| Kod |var fd = RoleEnvironment. CurrentRoleInstance. Faulydomain; |

## <a name="role-name"></a>Rollnamn
Hämtar roll namnet för instanserna.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = " /RoleEnvironment/CurrentInstance/@roleName " |
| Kod |var Rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Konfigurations inställning
Hämtar värdet för den angivna konfigurations inställningen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [ @name = ' Setting1 '] /@value " |
| Kod |var inställning = RoleEnvironment. GetConfigurationSettingValue ("Setting1"); |

## <a name="local-storage-path"></a>Lokal lagrings Sök väg
Hämtar den lokala lagrings Sök vägen för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/LocalResources/LocalResource [ @name = ' LocalStore1 '] /@path " |
| Kod |var localResourcePath = RoleEnvironment. GetLocalResource ("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Lokal lagrings storlek
Hämtar storleken på den lokala lagringen för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/LocalResources/LocalResource [ @name = ' LocalStore1 '] /@sizeInMB " |
| Kod |var localResourceSizeInMB = RoleEnvironment. GetLocalResource ("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Slut punkts protokoll
Hämtar slut punkts protokollet för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/Endpoints/Endpoint [ @name = ' Endpoint1 '] /@protocol " |
| Kod |var prot = RoleEnvironment. CurrentRoleInstance. InstanceEndpoints ["Endpoint1"]. Protokollhanterare |

## <a name="endpoint-ip"></a>Slut punkts-IP
Hämtar den angivna slut punktens IP-adress.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/Endpoints/Endpoint [ @name = ' Endpoint1 '] /@address " |
| Kod |var adress = RoleEnvironment. CurrentRoleInstance. InstanceEndpoints ["Endpoint1"]. IPEndpoint. address |

## <a name="endpoint-port"></a>Slut punkts port
Hämtar slut punkts porten för instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/Endpoints/Endpoint [ @name = ' Endpoint1 '] /@port " |
| Kod |var port = RoleEnvironment. CurrentRoleInstance. InstanceEndpoints ["Endpoint1"]. IPEndpoint. port; |

## <a name="example"></a>Exempel
Här är ett exempel på en arbets roll som skapar en start aktivitet med en miljö variabel med namnet `TestIsEmulated` set till [ @emulated XPath-värdet](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Nästa steg
Läs mer om filen [ServiceConfiguration. cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Skapa ett paket med [servicepack. cspkg](cloud-services-model-and-package.md#servicepackagecspkg) .

Aktivera [fjärr skrivbord](cloud-services-role-enable-remote-desktop-new-portal.md) för en roll.




