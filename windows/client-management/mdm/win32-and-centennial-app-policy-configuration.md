---
title: Win32 and Desktop Bridge app policy configuration
description: Starting in Windows 10, version 1703, you can import ADMX files and set those ADMX-backed policies for Win32 and Desktop Bridge apps.
ms.author: maricia
ms.date: 05/02/2017
ms.topic: article
ms.prod: w10
ms.technology: windows
author: nickbrower
---

# Win32 and Desktop Bridge app policy configuration
  
## In this section

-   [Overview](#overview)
-   [Ingesting an app ADMX file](#ingesting-an-app-admx-file)
-   [URI format for configuring an app policy](#uri-format-for-configuring-an-app-policy)
-   [ADMX-backed app policy examples](#admx-backed-app-policy-examples)
    - [Enabling an app policy](#enabling-an-app-policy)
    - [Disabling an app policy](#disabling-an-app-policy)
    - [Setting an app policy to not configured](#setting-an-app-policy-to-not-configured)

## <a href="" id="overview"></a>Overview

Starting in Windows 10, version 1703, you can import ADMX files (also called ADMX ingestion) and set those ADMX-backed policies for Win32 and Desktop Bridge apps by using Windows 10 Mobile Device Management (MDM) on desktop SKUs. The ADMX files that define policy information can be ingested to your device by using the Policy CSP URI, `./Device/Vendor/MSFT/Policy/ConfigOperations/ADMXInstall`. The ingested ADMX file is then processed into MDM policies.

When the ADMX policies are imported, the registry keys to which each policy is written are checked so that known system registry keys, or registry keys that are used by existing inbox policies or system components, are not overwritten. This precaution helps to avoid security concerns over opening the entire registry. Currently, the ingested policies are not allowed to write to locations within the **System**, **Software\Microsoft**, and **Software\Policies\Microsoft** keys.


## <a href="" id="ingesting-an-app-admx-file"></a>Ingesting an app ADMX file

The following ADMX file example shows how to ingest a Win32 or Desktop Bridge app ADMX file and set policies from the file. The ADMX file defines eight policies.

**Payload**
```XML
<policyDefinitions revision="1.0" schemaVersion="1.0">
  <categories>
    <category name="ParentCategoryArea"/>
    <category name="Category1">
      <parentCategory ref="ParentCategoryArea" />
    </category>
    <category name="Category2">
      <parentCategory ref="ParentCategoryArea" />
    </category>
    <category name="Category3">
      <parentCategory ref="Category2" />
    </category>
  </categories>
  <policies>
    <policy name="L_PolicyConfigurationMode" class="Machine" displayName="$(string.L_PolicyConfigurationMode)" explainText="$(string.L_ExplainText_ConfigurationMode)" presentation="$(presentation.L_PolicyConfigurationMode)" key="software\policies\contoso\companyApp" valueName="configurationmode">
      <parentCategory ref="Category1" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <enabledValue>
        <decimal value="1" />
      </enabledValue>
      <disabledValue>
        <decimal value="0" />
      </disabledValue>
      <elements>
        <text id="L_ServerAddressInternal_VALUE" key="software\policies\contoso\companyApp" valueName="serveraddressinternal" required="true" />
        <text id="L_ServerAddressExternal_VALUE" key="software\policies\contoso\companyApp" valueName="serveraddressexternal" required="true" />
      </elements>
    </policy>
    <policy name="L_PolicyEnableSIPHighSecurityMode" class="Machine" displayName="$(string.L_PolicyEnableSIPHighSecurityMode)" explainText="$(string.L_ExplainText_EnableSIPHighSecurityMode)" presentation="$(presentation.L_PolicyEnableSIPHighSecurityMode)" key="software\policies\contoso\companyApp" valueName="enablesiphighsecuritymode">
      <parentCategory ref="Category1" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <enabledValue>
        <decimal value="1" />
      </enabledValue>
      <disabledValue>
        <decimal value="0" />
      </disabledValue>
    </policy>
    <policy name="L_PolicySipCompression" class="Machine" displayName="$(string.L_PolicySipCompression)" explainText="$(string.L_ExplainText_SipCompression)" presentation="$(presentation.L_PolicySipCompression)" key="software\policies\contoso\companyApp">
      <parentCategory ref="Category1" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <elements>
        <enum id="L_PolicySipCompression" valueName="sipcompression">
          <item displayName="$(string.L_SipCompressionVal0)">
            <value>
              <decimal value="0" />
            </value>
          </item>
          <item displayName="$(string.L_SipCompressionVal1)">
            <value>
              <decimal value="1" />
            </value>
          </item>
          <item displayName="$(string.L_SipCompressionVal2)">
            <value>
              <decimal value="2" />
            </value>
          </item>
          <item displayName="$(string.L_SipCompressionVal3)">
            <value>
              <decimal value="3" />
            </value>
          </item>
        </enum>
      </elements>
    </policy>
    <policy name="L_PolicyPreventRun" class="Machine" displayName="$(string.L_PolicyPreventRun)" explainText="$(string.L_ExplainText_PreventRun)" presentation="$(presentation.L_PolicyPreventRun)" key="software\policies\contoso\companyApp" valueName="preventrun">
      <parentCategory ref="Category1" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <enabledValue>
        <decimal value="1" />
      </enabledValue>
      <disabledValue>
        <decimal value="0" />
      </disabledValue>
    </policy>
    <policy name="L_PolicyConfiguredServerCheckValues" class="Machine" displayName="$(string.L_PolicyConfiguredServerCheckValues)" explainText="$(string.L_ExplainText_ConfiguredServerCheckValues)" presentation="$(presentation.L_PolicyConfiguredServerCheckValues)" key="software\policies\contoso\companyApp">
      <parentCategory ref="Category2" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <elements>
        <text id="L_ConfiguredServerCheckValues_VALUE" valueName="configuredservercheckvalues" required="true" />
      </elements>
    </policy>
    <policy name="L_PolicySipCompression_1" class="User" displayName="$(string.L_PolicySipCompression)" explainText="$(string.L_ExplainText_SipCompression)" presentation="$(presentation.L_PolicySipCompression_1)" key="software\policies\contoso\companyApp">
      <parentCategory ref="Category2" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <elements>
        <enum id="L_PolicySipCompression" valueName="sipcompression">
          <item displayName="$(string.L_SipCompressionVal0)">
            <value>
              <decimal value="0" />
            </value>
          </item>
          <item displayName="$(string.L_SipCompressionVal1)">
            <value>
              <decimal value="1" />
            </value>
          </item>
          <item displayName="$(string.L_SipCompressionVal2)">
            <value>
              <decimal value="2" />
            </value>
          </item>
          <item displayName="$(string.L_SipCompressionVal3)">
            <value>
              <decimal value="3" />
            </value>
          </item>
        </enum>
      </elements>
    </policy>
    <policy name="L_PolicyPreventRun_1" class="User" displayName="$(string.L_PolicyPreventRun)" explainText="$(string.L_ExplainText_PreventRun)" presentation="$(presentation.L_PolicyPreventRun_1)" key="software\policies\contoso\companyApp" valueName="preventrun">
      <parentCategory ref="Category3" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <enabledValue>
        <decimal value="1" />
      </enabledValue>
      <disabledValue>
        <decimal value="0" />
      </disabledValue>
    </policy>
    <policy name="L_PolicyGalDownloadInitialDelay_1" class="User" displayName="$(string.L_PolicyGalDownloadInitialDelay)" explainText="$(string.L_ExplainText_GalDownloadInitialDelay)" presentation="$(presentation.L_PolicyGalDownloadInitialDelay_1)" key="software\policies\contoso\companyApp">
      <parentCategory ref="Category3" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <elements>
        <decimal id="L_GalDownloadInitialDelay_VALUE" valueName="galdownloadinitialdelay" minValue="0" required="true" />
      </elements>
    </policy>
  </policies>
</policyDefinitions>
```

**Request Syncml**

The ADMX file is escaped and sent in SyncML format through the Policy CSP URI, `./Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/{AppName}/{SettingType}/{FileUid or AdmxFileName}`.
When the ADMX file is imported, the policy states for each new policy are the same as those in a regular MDM policy: Enabled, Disabled, or Not Configured. 

The following example shows an ADMX file in SyncML format:

```XML
<SyncML xmlns="SYNCML:SYNCML1.2">
  <SyncBody>
    <Add>
      <CmdID>102</CmdID>
      <Item>
        <Meta>
          <Format>chr</Format>
          <Type>text/plain</Type>
        </Meta>
        <Target>
          <LocURI>./Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/ContosoCompanyApp/Policy/AppAdmxFile01</LocURI>
        </Target>
        <Data>&lt;policyDefinitions revision=&quot;1.0&quot; schemaVersion=&quot;1.0&quot;&gt;
          &lt;categories&gt;
          &lt;category name=&quot;ParentCategoryArea&quot;/&gt;
          &lt;category name=&quot;Category1&quot;&gt;
          &lt;parentCategory ref=&quot;ParentCategoryArea&quot; /&gt;
          &lt;/category&gt;
          &lt;category name=&quot;Category2&quot;&gt;
          &lt;parentCategory ref=&quot;ParentCategoryArea&quot; /&gt;
          &lt;/category&gt;
          &lt;category name=&quot;Category3&quot;&gt;
          &lt;parentCategory ref=&quot;Category2&quot; /&gt;
          &lt;/category&gt;
          &lt;/categories&gt;
          &lt;policies&gt;
          &lt;policy name=&quot;L_PolicyConfigurationMode&quot; class=&quot;Machine&quot; displayName=&quot;$(string.L_PolicyConfigurationMode)&quot; explainText=&quot;$(string.L_ExplainText_ConfigurationMode)&quot; presentation=&quot;$(presentation.L_PolicyConfigurationMode)&quot; key=&quot;software\policies\contoso\companyApp&quot; valueName=&quot;configurationmode&quot;&gt;
          &lt;parentCategory ref=&quot;Category1&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;enabledValue&gt;
          &lt;decimal value=&quot;1&quot; /&gt;
          &lt;/enabledValue&gt;
          &lt;disabledValue&gt;
          &lt;decimal value=&quot;0&quot; /&gt;
          &lt;/disabledValue&gt;
          &lt;elements&gt;
          &lt;text id=&quot;L_ServerAddressInternal_VALUE&quot; key=&quot;software\policies\contoso\companyApp&quot; valueName=&quot;serveraddressinternal&quot; required=&quot;true&quot; /&gt;
          &lt;text id=&quot;L_ServerAddressExternal_VALUE&quot; key=&quot;software\policies\contoso\companyApp&quot; valueName=&quot;serveraddressexternal&quot; required=&quot;true&quot; /&gt;
          &lt;/elements&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicyEnableSIPHighSecurityMode&quot; class=&quot;Machine&quot; displayName=&quot;$(string.L_PolicyEnableSIPHighSecurityMode)&quot; explainText=&quot;$(string.L_ExplainText_EnableSIPHighSecurityMode)&quot; presentation=&quot;$(presentation.L_PolicyEnableSIPHighSecurityMode)&quot; key=&quot;software\policies\contoso\companyApp&quot; valueName=&quot;enablesiphighsecuritymode&quot;&gt;
          &lt;parentCategory ref=&quot;Category1&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;enabledValue&gt;
          &lt;decimal value=&quot;1&quot; /&gt;
          &lt;/enabledValue&gt;
          &lt;disabledValue&gt;
          &lt;decimal value=&quot;0&quot; /&gt;
          &lt;/disabledValue&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicySipCompression&quot; class=&quot;Machine&quot; displayName=&quot;$(string.L_PolicySipCompression)&quot; explainText=&quot;$(string.L_ExplainText_SipCompression)&quot; presentation=&quot;$(presentation.L_PolicySipCompression)&quot; key=&quot;software\policies\contoso\companyApp&quot;&gt;
          &lt;parentCategory ref=&quot;Category1&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;elements&gt;
          &lt;enum id=&quot;L_PolicySipCompression&quot; valueName=&quot;sipcompression&quot;&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal0)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;0&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal1)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;1&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal2)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;2&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal3)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;3&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;/enum&gt;
          &lt;/elements&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicyPreventRun&quot; class=&quot;Machine&quot; displayName=&quot;$(string.L_PolicyPreventRun)&quot; explainText=&quot;$(string.L_ExplainText_PreventRun)&quot; presentation=&quot;$(presentation.L_PolicyPreventRun)&quot; key=&quot;software\policies\contoso\companyApp&quot; valueName=&quot;preventrun&quot;&gt;
          &lt;parentCategory ref=&quot;Category1&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;enabledValue&gt;
          &lt;decimal value=&quot;1&quot; /&gt;
          &lt;/enabledValue&gt;
          &lt;disabledValue&gt;
          &lt;decimal value=&quot;0&quot; /&gt;
          &lt;/disabledValue&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicyConfiguredServerCheckValues&quot; class=&quot;Machine&quot; displayName=&quot;$(string.L_PolicyConfiguredServerCheckValues)&quot; explainText=&quot;$(string.L_ExplainText_ConfiguredServerCheckValues)&quot; presentation=&quot;$(presentation.L_PolicyConfiguredServerCheckValues)&quot; key=&quot;software\policies\contoso\companyApp&quot;&gt;
          &lt;parentCategory ref=&quot;Category2&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;elements&gt;
          &lt;text id=&quot;L_ConfiguredServerCheckValues_VALUE&quot; valueName=&quot;configuredservercheckvalues&quot; required=&quot;true&quot; /&gt;
          &lt;/elements&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicySipCompression_1&quot; class=&quot;User&quot; displayName=&quot;$(string.L_PolicySipCompression)&quot; explainText=&quot;$(string.L_ExplainText_SipCompression)&quot; presentation=&quot;$(presentation.L_PolicySipCompression_1)&quot; key=&quot;software\policies\contoso\companyApp&quot;&gt;
          &lt;parentCategory ref=&quot;Category2&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;elements&gt;
          &lt;enum id=&quot;L_PolicySipCompression&quot; valueName=&quot;sipcompression&quot;&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal0)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;0&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal1)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;1&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal2)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;2&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;item displayName=&quot;$(string.L_SipCompressionVal3)&quot;&gt;
          &lt;value&gt;
          &lt;decimal value=&quot;3&quot; /&gt;
          &lt;/value&gt;
          &lt;/item&gt;
          &lt;/enum&gt;
          &lt;/elements&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicyPreventRun_1&quot; class=&quot;User&quot; displayName=&quot;$(string.L_PolicyPreventRun)&quot; explainText=&quot;$(string.L_ExplainText_PreventRun)&quot; presentation=&quot;$(presentation.L_PolicyPreventRun_1)&quot; key=&quot;software\policies\contoso\companyApp&quot; valueName=&quot;preventrun&quot;&gt;
          &lt;parentCategory ref=&quot;Category3&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;enabledValue&gt;
          &lt;decimal value=&quot;1&quot; /&gt;
          &lt;/enabledValue&gt;
          &lt;disabledValue&gt;
          &lt;decimal value=&quot;0&quot; /&gt;
          &lt;/disabledValue&gt;
          &lt;/policy&gt;
          &lt;policy name=&quot;L_PolicyGalDownloadInitialDelay_1&quot; class=&quot;User&quot; displayName=&quot;$(string.L_PolicyGalDownloadInitialDelay)&quot; explainText=&quot;$(string.L_ExplainText_GalDownloadInitialDelay)&quot; presentation=&quot;$(presentation.L_PolicyGalDownloadInitialDelay_1)&quot; key=&quot;software\policies\contoso\companyApp&quot;&gt;
          &lt;parentCategory ref=&quot;Category3&quot; /&gt;
          &lt;supportedOn ref=&quot;windows:SUPPORTED_Windows7&quot; /&gt;
          &lt;elements&gt;
          &lt;decimal id=&quot;L_GalDownloadInitialDelay_VALUE&quot; valueName=&quot;galdownloadinitialdelay&quot; minValue=&quot;0&quot; required=&quot;true&quot; /&gt;
          &lt;/elements&gt;
          &lt;/policy&gt;
          &lt;/policies&gt;
          &lt;/policyDefinitions&gt;</Data>
      </Item>
    </Add>
    <Final/>
  </SyncBody>
</SyncML>
```

**Response Syncml**
```XML
<Status><CmdID>2</CmdID><MsgRef>1</MsgRef><CmdRef>102</CmdRef><Cmd>Add</Cmd><Data>200</Data></Status>
```

### <a href="" id="uri-format-for-configuring-an-app-policy"></a>URI format for configuring an app policy 

The following example shows how to derive a Win32 or Desktop Bridge app policy name and policy area name:

```XML
<categories>
    <category name="ParentCategoryArea"/>
    <category name="Category1">
      <parentCategory ref="ParentCategoryArea" />
    </category>
    <category name="Category2">
      <parentCategory ref="ParentCategoryArea" />
    </category>
    <category name="Category3">
      <parentCategory ref="Category2" />
    </category>
  </categories>
<policy name="L_PolicyPreventRun_1" class="User" displayName="$(string.L_PolicyPreventRun)" explainText="$(string.L_ExplainText_PreventRun)" presentation="$(presentation.L_PolicyPreventRun_1)" key="software\policies\contoso\companyApp" valueName="preventrun">
      <parentCategory ref="Category3" />
      <supportedOn ref="windows:SUPPORTED_Windows7" />
      <enabledValue>
        <decimal value="1" />
      </enabledValue>
      <disabledValue>
        <decimal value="0" />
      </disabledValue>
    </policy>
```

As documented in [Policy CSP](policy-configuration-service-provider.md), the URI format to configure a policy via Policy CSP is: 
'./{user or device}/Vendor/MSFT/Policy/Config/{AreaName}/{PolicyName}'.

**User or device policy**

In the policy class, the attribute is defined as "User" and the URI is prefixed with `./user`.
If the attribute value is "Machine", the URI is prefixed with `./device`.
If the attribute value is "Both", the policy can be configured either as a user or a device policy.

The policy {AreaName} format is {AppName}~{SettingType}~{CategoryPathFromAdmx}.
{AppName} and {SettingType} are derived from the URI that is used to import the ADMX file. In this example, the URI is: `./Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/ContosoCompanyApp/Policy/AppAdmxFile01`.

{CategoryPathFromAdmx} is derived by traversing the parentCategory parameter. In this example, {CategoryPathFromAdmx} is ParentCategoryArea~Category2~Category3. Therefore, {AreaName} is ContosoCompanyApp~ Policy~ ParentCategoryArea~Category2~Category3.

Therefore, from the example:
   - Class: User
   - Policy name: L_PolicyPreventRun_1
   - Policy area name: ContosoCompanyApp~Policy~ParentCategoryArea~Category2~Category3
   - URI: `./user/Vendor/MSFT/Policy/Config/ContosoCompanyApp~Policy~ParentCategoryArea~Category2~Category3/L_PolicyPreventRun_1`

## <a href="" id="admx-backed-app-policy-examples"></a>ADMX-backed app policy examples

The following examples describe how to set an ADMX-ingested app policy.

### <a href="" id="enabling-an-app-policy"></a>Enabling an app policy

**Payload**
```XML
<enabled/>
<data id="L_ServerAddressInternal_VALUE" value="TextValue1"/>
<data id="L_ServerAddressExternal_VALUE" value="TextValue2"/>
```

**Request Syncml**
```XML
<SyncML xmlns="SYNCML:SYNCML1.1">
  <SyncBody>
    <Replace>
      <CmdID>103</CmdID>
      <Item>
        <Meta>
          <Format>chr</Format>
          <Type>text/plain</Type>
        </Meta>
        <Target>
          <LocURI>./Device/Vendor/MSFT/Policy/Config/ContosoCompanyApp~ Policy~ParentCategoryArea~Category1/L_PolicyConfigurationMode</LocURI>
        </Target>
        <Data>&lt;enabled/&gt;&lt;data id=&quot;L_ServerAddressInternal_VALUE&quot; value=&quot;TextValue1&quot;/&gt;&lt;data id=&quot;L_ServerAddressExternal_VALUE&quot; value=&quot;TextValue2&quot;/&gt;</Data>
      </Item>
    </Replace>
    <Final/>
  </SyncBody>
</SyncML>
```

**Response SyncML**
```XML
<Status><CmdID>2</CmdID><MsgRef>1</MsgRef><CmdRef>103</CmdRef><Cmd>Replace</Cmd><Data>200</Data></Status>
```

### <a href="" id="disabling-an-app-policy"></a>Disabling an app policy

**Payload**
```XML
<disabled/>
```

**Request SyncML**
```XML
<SyncML xmlns="SYNCML:SYNCML1.1">
  <SyncBody>
    <Replace>
      <CmdID>104</CmdID>
      <Item>
        <Meta>
          <Format>chr</Format>
          <Type>text/plain</Type>
        </Meta>
        <Target>
          <LocURI>./Device/Vendor/MSFT/Policy/Config/ContosoCompanyApp~ Policy~ParentCategoryArea~Category1/L_PolicyConfigurationMode</LocURI>
        </Target>
        <Data>&lt;disabled/&gt;</Data>
      </Item>
    </Replace>
    <Final/>
  </SyncBody>
</SyncML>
```

**Response SyncML**
```XML
<Status><CmdID>2</CmdID><MsgRef>1</MsgRef><CmdRef>104</CmdRef><Cmd>Replace</Cmd><Data>200</Data></Status>
```

### <a href="" id="setting-an-app-policy-to-not-configured"></a>Setting an app policy to not configured

**Payload**

(None)

**Request SyncML**
```XML
<SyncML xmlns="SYNCML:SYNCML1.1">
  <SyncBody>
    <Delete>
      <CmdID>105</CmdID>
      <Item>
        <Target>
          <LocURI>./Device/Vendor/MSFT/Policy/Config/ContosoCompanyApp~ Policy~ParentCategoryArea~Category1/L_PolicyConfigurationMode</LocURI>
        </Target>
      </Item>
    </Delete>
    <Final/>
  </SyncBody>
</SyncML>
```

**Response SyncML**
```XML
<Status><CmdID>2</CmdID><MsgRef>1</MsgRef><CmdRef>105</CmdRef><Cmd>Delete</Cmd><Data>200</Data></Status>
```
