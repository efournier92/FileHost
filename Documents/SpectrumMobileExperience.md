# The Spectrum Mobile Experience

## Contents
* [Overview](#overview)
* [User Experience](#user-experience)
* [Supported Mobile Platforms](#supported-mobile-platforms)
* [Server-Side Mobile Logic](#server-side-mobile-logic)
  - [MultiPage.ascx.cs](#multipageascxcs)
  - [ProductAdmin.ascx.cs](#productadminascxcs)
  - [SpectrumProvider.cs](#spectrumprovidercs)
  - [TwoStepRegistrationSection.cs](#twostepregistrationsectioncs)
  - [RegistrationDisplayMode.cs](#registrationdisplaymodecs)
  - [RegistrationMultiPurpose.ascx.cs](#registrationmultipurposeascxcs)
  - [PchUniNav.ascx.cs](#pchuninavascxcs)
  - [PackageChecker.cs](#packagecheckercs)
  - [PresentationShell.cs](#presentationshellcs)
  - [SLP.ascx.cs](#slpascxcs)
  - [PackageUploader.cs](#packageuploadercs)
  - [ShellParser.cs](#shellparsercs)
  - [ProgressBarEdit.ascx.cs](#progressbareditascxcs)
  - [SLPEdit.ascx.cs](#slpeditascxcs)
  - [TargetedProductsEdit.ascx.cs](#targetedproductseditascxcs)
  - [SequenceManager.cs](#sequencemanagercs)
  - [ShowMoreProducts.cs](#showmoreproductscs)
  - [CacheKeyHelper.cs](#cachekeyhelpercs)
  - [IsMobile Declarations](#ismobile-declarations)
* [Client-Side Mobile Logic](#client-side-mobile-logic)
  - [Multipage.js](#multipagejs)
  - [MultiPageMobile.js](#multipagemobilejs)
  - [Tabs.js](#tabsjs)
  - [Spectrum.js](#spectrumjs)
  - [sequenceController.js](#sequencecontrollerjs)
  - [SLP.js](#slpjs)
  - [SpectrumDeviceManager.js](#spectrumdevicemanagerjs)
  - [FusionProductsTab.js](#fusionproductstabjs)
  - [showMoreLikeThis.js](#showmorelikethisjs)
  - [TargetedProducts.js](#targetedproductsjs)
  - [Replay.js](#replayjs)
  - [EoProductDevice.js](#eoproductdevicejs)
  - [Cart.js](#cartjs)
  - [AbandondedCart.js](#abandondedcartjs)
  - [allAdUnitsHelper.js](#alladunitshelperjs)
  - [ProductLightboxDisplay.js](#productlightboxdisplayjs)
  - [EventTrackerProvider.js](#eventtrackerproviderjs)
  - [MultiAnalyticsProvider.js](#multianalyticsproviderjs)
  - [SpectrumAnalytics.js](#spectrumanalyticsjs)
* [Style Sheets](#style-sheets)
* [Suggested Improvements](#suggested-improvements)

## Overview
**Goal:** _Understand mobile experience on Spectrum with focus on how it differs from desktop experience functionally and document findings._

The purpose of this document is to compare and contrast the mobile experience on the Spectrum platform, with emphasis on how it differs from the desktop experience. 

Although the Spectrum application provides the same basic functionality when viewed on desktop and mobile, the end-user experiences the platform differently when viewed from a mobile device. Likewise, the logic used to render the mobile platform differs from that used on desktop.

This document is meant to serve as a reference for Spectrum developers, by pinpointing UI elements & code snippets where our rendering logic differs between desktop & mobile, while annotating each snippet.

Based on these findings, the document will conclude with suggesting how our desktop/mobile rendering logic could be simplified in the future.

## User Experience
_When it comes to Spectrum user experience, there exist a number of conspicuous differences between desktop & mobile. Below is a list of some of the most conspicuous differences._

### _In desktop, admins can configure the following ad units:_

### **`Large`**

  ![Large Ad Unit](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Adunit_Large.png)

### **`SuperFeature`**

  ![SuperFeature Ad Unit](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Adunit_SuperFeature.png)

### **`Outlet`**

  ![Outlet Ad Unit](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Adunit_Outlet.png)

### **`Coupon`**

  ![Coupon Ad Unit](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Adunit_Coupon.png)

### _In mobile, all ad units are rendered as `mobileLarge`_

  ![MobileLarge Ad Unit](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Adunit_MobileLarge.png)

### _Tabs are rendered differently between desktop & mobile_

  * In desktop each time the user clicks the 'Continue' button, they are redirected to a new tab, as indicated by the 'Page 1', 'Page 2', etc. tabs at the top & bottom of each page. 
  
  ![Desktop Tabs](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Tabs_Desktop.png)

  * In mobile, clicking 'Continue' appends more products to the user's current view, thus eliminating the tab concept from our mobile experience.

  ![Mobile Tabs](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Tabs_Mobile.png)

### _The `Place Order` progress bar appears differently between desktop & mobile_

### **Desktop**

  ![Desktop Progress Bar](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Progress_Desktop.png)

### **Mobile**

  ![Mobile Progress Bar](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Progress_Mobile.png)

### _The navigation bar menu bar appears differently between desktop & mobile_

### **Desktop**

  ![Desktop Navigation Bar](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/NavBar_Desktop.png)

### **Mobile**

  ![Mobile Navigation Bar](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/NavBar_Mobile_01.png)

### _The mobile experience also offer a sidebar, with additional functionality_

  ![Mobile Sidebar](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/NavBar_Mobile_02.png)

### _Spectrum registration packages contain seperate styling for desktop & mobile_

### **Desktop**

  ![Desktop Registration](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Reg_Desktop.png)

### **Mobile**

  ![Mobile Registration](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/Reg_Mobile.png)

### _`Show More Like This` is rendered differently between desktop & mobile_

### Desktop

  ![Desktop SMLT](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/SMLT_Desktop.png)

### Mobile

  ![Mobile SMLT](https://github.com/efournier92/filehost/raw/master/SpectrumMobile/SMLT_Mobile.png)

## Supported Mobile Platforms
* iOs
* Android
* Symbian
* WindowsPhone
* RIM
* Bada
* webOs

## Server-Side Mobile Logic

### MultiPage.ascx.cs
#### **`MultiPage` contains some of the most important mobile rendering logic.**
#### _Load `MultiPageMobile.js`, if user's device is mobile_
```csharp
if (IsMobileApplicable())
{
    RegisterScriptInclude(
      "Spectrum/scripts/ProductAdmin/Runtime/MultiPageMobile.js", BundleDeviceType.Mobile
    );
}
```
#### _Load MultiPage styling_
```csharp
public string GetMultipageCss()
{
  if (IsMobileApplicable())
    return "mobileMultipage";
} 
```
#### _Special logic, only relevant to mobile users_
```csharp
public bool IsMobileDevice()
{
  if (_hasMadeMobileCall)
    return _isMobile;
  var pageLoader = (PageLoader)HttpContext.Current.Items["PageLoader_Instance"];

  _isMobile = pageLoader.IsMobile;
  _hasMadeMobileCall = true;
  return _isMobile;
}
```
#### _Load mobile shell, if user is on a mobile device_
```csharp
public bool IsMobileShellActive()
{
  var container = ComponentContainer;
  if (container == null) return false;

  var pageLoader = container.PageLoader;
  if (pageLoader == null) return false;

  return pageLoader.IsMobileShell;
} 
```

### MultipageConstants.cs
#### _Defaults`DefaultMobileProductsPerPage` to -1_
  * `public const int DefaultMobileProductsPerPage = -1;`

### ProductAdmin.ascx.cs
#### _Assigns orderMode, based on device type_
```csharp
var orderMode = (PageLoaderInstance.IsMobile && (dto.MobileOrderMode != null)) ?
dto.MobileOrderMode.Value : dto.OrderMode.Value; 
```

### ProductAdminService.cs
#### _Retrieve relevant `MobileUiElements` from database_
```csharp
dto.MobileUiElementsExtJsTree = 
  ExtJsConversionHelper.ConvertUiElementsToExtJsFormat(
    dto.MobileUiElements, 
    _uiElementExtJsTreeConverter);
var continueShopping = isMobile 
  ? dto.MobileContinueShopping 
  : dto.DesktopContinueShopping;
dto.MobileUiElements = ConvertExtJsTreeToUiElements(dto.MobileUiElementsExtJsTree);
if (dto.MobileUiElements != null && dto.MobileUiElements.Count > 0)
```

### SpectrumProvider.cs
#### _Add mobile shell reference to `SpectrumProvider` params_
```csharp
cmd.Parameters.AddWithValue("@MobileShellHTML", ps.MobileShellHtml);
```
#### _Update mobile shell component id's in shell html_
```csharp
if (!String.IsNullOrEmpty(presentationShell.MobileShellHtml))
{
  presentationShell.MobileShellHtml = UpdateShellComponentIds(
    presentationShell.MobileShellHtml, presentationShell.ShellComponents
  );

  this.UpdateShell(presentationShell);

  return Convert.ToInt32(m_OutParam.Value);
}
```
#### _Cleanup `mobileShell` HTML, for final presentation_
```csharp
if (!String.IsNullOrEmpty(shell.MobileShellHtml))
{
  var finalMobileShellHtml = CleanupHelper.CleanupHtml(cleanupCommands, shell.MobileShellHtml);
  shell.MobileShellHtml = finalMobileShellHtml;
}
```

### ResetPassword.aspx.cs
#### _Mobile decision logic for `ResetPassword` feature_
```csharp
public bool IsMobile { get; set; }
```
```csharp
ResetPasswordContol.IsMobile = isMobileDevice;
PchUniNav.Visible = isMobileDevice;
```
```csharp
protected override bool IsMobile()
{
  var deviceType = GetDeviceTypeFromUserAgent();
  return deviceType == "MOBILE";
}
```

### RegistrationDisplayMode.cs
#### _Set dislpay mode, for use with registration calls_
```csharp
public static readonly RegistrationDisplayMode Mobile = Parse("Mobile");
```

### SignIn.aspx.cs
#### _Load mobile assets for `SignIn` feature_
```csharp
private void RenderScriptsAndStylesheets()
{
  RegisterCss("Spectrum/css/UniNav/Mobile/sso_pages_mobile.css", BundleDeviceType.Mobile);
  RegisterScriptIncludeAtBottom(
    "Spectrum/scripts/PathSession.js", BundleDeviceType.Any, BundleType.GlobalJs
  );
}
```
```csharp
RegisterScriptVariable("spectrumDeviceType", "MOBILE", true);
```

### EditInfo.aspx.cs
#### _Set device type to `MOBILE`_
```csharp
RegisterScriptVariable("spectrumDeviceType", "MOBILE", true); 
```
```csharp
private void RenderScriptsAndStylesheets()
{
  RegisterCss("Spectrum/css/UniNav/Mobile/sso_pages_mobile.css", BundleDeviceType.Mobile);
}
```

### RegistrationMultiPurpose.ascx.cs
#### _Load registration page, based on device type_
```csharp
var isTwoStep = !((string.IsNullOrEmpty(registrationFields.RegistrationType)
  || registrationFields.RegistrationType == "Default")
  || (regDisplayMode != RegistrationDisplayMode.Inline
  && regDisplayMode != RegistrationDisplayMode.Mobile));
```

### Promos.ascx.cs
#### _Load specially styled promos, if user is on a mobile device_
```csharp
if (this.DeviceType == SpectrumDeviceAnalyzerConstants.DeviceTypes.Mobile)
{
  return "tabPromoMobile";
} 
```

### MyAccountPageBase.cs
#### _Assign mobile details for `MyAccount` page_
```csharp
MyAccountPageBase.cs
if (IsMobile() && !deviceType.HasFlag(BundleDeviceType.Mobile)) return;
```

### PchUniNav.ascx.cs
#### _Mobile decision logic for `PchUniNav` module_
```csharp
Page_Load(isMobile?)
if (isMobilePresentInEqpCookie)
{
  if (Convert.ToBoolean(eqpCookieContents["mob"]))
    return true;
}
```
#### _Load special styling, if mobile device_
```csharp
if (isMobile)
{
  _scriptContainer.RegisterCss("Spectrum/css/UniNav/Mobile/uninav_mobile.css");
}
```

### SubcomponentProperties.cs
#### _Sets mobile property codes to string values_
```csharp
public const string MobileTabsPropertyCode = "PADMN-MobTabs";
public const string MobileOrderModePropertyCode = "PADMN-MobOrdMde";
public const string MobileProductsPerPage = "PADMN-MobPrdPg";
public const string NoOfMobileProductsPropertyCode = "NumMobPrdt";
public const string MobileContinueShoppingPropertyCode = "MobContShopping";
public const string MobileStartTabPropertyCode = "MobStartTab";
public const string MobileHideAdditionalTabsPropertyCode = "MobHideAdtnlTab"; 
```

### PackageChecker.cs
#### _Mobile shell decision logic_
```csharp
public bool IsMobileShell(string shellname)
{
  if (string.IsNullOrEmpty(shellname))
    return false;
  return shellname.StartsWith(MobileShellPrefix);
}
```
```csharp
public PresentationShell GetEditableShell(string name)
{
  if (IsMobileShell(name))
    name = name.Substring(2);
  return GetShellByName(name);
}
```
```csharp
return IsMobileShell(name) ? shell.MobileShellHtml : shell.ShellHTML;
```

### PresentationShell.cs
#### _Declare `MobileShellHtml` as gettable/settable object_
```csharp
public string MobileShellHtml { get; set; }
```
```csharp
MobileShellHtml = ps.MobileShellHtml;
```

### SLP.ascx.cs
#### _Constant, to determine if mobile shell should be fetched_
```csharp
public override Control GetAdditionalRequiredResources(bool isMobileShell)
```

### PackageUploader.cs
#### _Check if shell is for mobile, based on given `shellpath`_
```csharp
private bool IsMobileShell(string shellpath)
{
  var filename = GetShellName(shellpath);
  return filename != null 
    && filename.StartsWith(
      SpectrumPackageConstants.MobileShellPrefix, StringComparison.InvariantCultureIgnoreCase
    );
}
```

### ShellParser.cs
#### _Fetch HTML for relevant mobile shell_
```csharp
var mobileShellHtml = ""; 
mobileShellHtml = GetShellContents(
  physicalPath, SpectrumPackageConstants.MobileShellPrefix + filename
);
responseMessage.MobilePresentationShell.ShellHTML = 
  responseMessage.MobilePresentationShell.MobileShellHtml;
```

### SequencesService.cs
#### _Mobile sequence connections are maintained separately from desktop sequences_
```csharp
MobileConnections = ToConnectionRecords(sequence.SequenceConnections,
                    SpectrumDeviceAnalyzerConstants.DeviceTypes.Mobile),
                    public ConnectionRecord[] MobileConnections { get; set; }
```

### AssetEdit.ascx.cs
#### _Handle loading mobile assets_
```csharp
if (mobileAsset.HasFile)
{
  if (deviceMap != null && deviceMap.ContainsKey("MOBILE"))
    deviceMap["MOBILE"].FileName = mobileAsset.FileName;
  else
    deviceMap.Add("MOBILE", new DeviceTypeFilenamePair {FileName = mobileAsset.FileName});
}
lblMobileFileName.Text = Model.Asset.DeviceTypeFilenameMap.ContainsKey("MOBILE")
mobileAsset.Enabled 
```

### ShowMoreFusionProduct.ascx.cs
#### _Adjust SMLT logic to for mobile device view_
```csharp
txtMobileShowMoreProducts.Text = showMoreProducts.NoOfMobileProducts.ToString();
  NoOfMobileProducts =
  !string.IsNullOrEmpty(txtMobileShowMoreProducts.Text)
    ? Convert.ToInt32(txtMobileShowMoreProducts.Text)
    : 0
```

### PchDevices.ascx.cs
#### _Switch `pageLoader` logic for mobile view_
```csharp
private bool IsMobile()
{
  var pageLoader = (PageLoader)HttpContext.Current.Items["PageLoader_Instance"];
  return pageLoader.IsMobile;
}
DeviceTypeByDeviceAnalyzer()
```

### SlpDeviceValidator.cs
```csharp
if (validateForDevice == "MOBILE")
```

### DiscountPriceSetting.ascx.cs
```csharp
const string mobileStr = "_mobile";
```

### ExpressCheckoutEdit.ascx.cs
#### _Configure `ExpressCheckout` button for mobile device view_
```csharp
cbButtonsMobileEnabled.Checked = Model.ButtonConfig.IsMobileEnabled; 
cbPopupMobileEnabled.Checked = Model.PopUpConfig.IsMobileEnabled;
var isDeviceTypeSelectedForButtonConfig = 
  (cbButtonsDesktopEnabled.Checked 
    || cbButtonsTabletEnabled.Checked
    || cbButtonsMobileEnabled.Checked);
IsMobileEnabled = cbButtonsMobileEnabled.Checked
```

### FusionProductsTabEdit.ascx.cs
#### _Validate the admin has configured the FPT for at least one view mode_
```csharp
if (!cbDesktopEnabled.Checked && !cbMobileEnabled.Checked && !cbTabletEnabled.Checked)
{
  errors.Add("Please select at least one display platform");
}
```

### ProgressBarEdit.ascx.cs
#### _Admin configuration effecting mobile progress bar_
```csharp
cbButtonsMobileEnabled.Checked = Model.Config.IsMobileEnabled;
var isDeviceTypeSelectedForButtonConfig = 
  (
    cbButtonsDesktopEnabled.Checked 
    || cbButtonsTabletEnabled.Checked 
    || cbButtonsMobileEnabled.Checked
  );
```

### SLPEdit.ascx.cs
#### _Admin SLP configuration related to mobile content_
```csharp
if(!string.IsNullOrEmpty(Model.MobileImageUrl))
  txtMobileSlpUrl.Text = Model.MobileImageUrl;
  devicemodel.MobileImageUrl = txtMobileSlpUrl.Text;
if(!string.IsNullOrEmpty(Model.MobileImageUrl))
  txtMobileSlpUrl.Text = Model.MobileImageUrl;
if (string.IsNullOrEmpty(txtSLPUrl.Text) && string.IsNullOrEmpty(txtMobileSlpUrl.Text)) 
  errors.Add("Neither desktop nor mobile image url supplied");
```

### TargetedProductsEdit.ascx.cs
#### _Admins can set different number of targeted products for mobile users_
```csharp
txtNoOfProductsMobile.Text = Convert.ToString(DefaultMobileProductCount); 
NumberOfMobileProducts = int.Parse(txtNoOfProductsMobile.Text)
txtNoOfProductsMobile.Text = (Model.ProductCount == null) 
  ? Convert.ToString(DefaultMobileProductCount)
  : Convert.ToString(Model.ProductCount.NumberOfMobileProducts);
var isMobileNoOfProductsMissing = (String.IsNullOrEmpty(txtNoOfProductsMobile.Text));
```

### SequenceManager.cs
#### _Admins can set differently for mobile users_
```csharp
if (deviceType == SpectrumDeviceAnalyzerConstants.DeviceTypes.Mobile)
{
  sequence = clonedSequences.FirstOrDefault();
}
```

### ShowMoreProducts.cs
#### _Sets number of products to show in mobile, from admin config_
```csharp
public int NoOfMobileProducts { get; set; }
```

### SpectrumDeviceAnalyzerConstants.cs
#### _Declare Mobile as a string constant, for use throughout server-side logic_
```csharp
public const string Mobile = "MOBILE";
```

### AppSettingKeys.cs
#### _Contains keys related to two mobile-related processes_
  * `MobileOptimizationOn`
  * `EnableMobileShell`

### CampaignTypeConstants.cs
#### _Contains keys related to two mobile-related processes_
  * `public const string PchComMobileClick1 = "PCH.com - Mobile Click 1";`
  * `public const string PchComMobileClick1 = "PCH.com - Mobile Portal";`

### CacheKeyHelper.cs
#### _Specifies whether or not mobile shell is enabled_
```csharp
var enableMobileShell = Convert.ToBoolean(
  ConfigurationManagerGateway.GetAppSetting(AppSettingKeys.EnableMobileShell)
);
```

### `IsMobile` Declarations
#### _The following files contain `public bool IsMobile { get; set; }`_
  * CreatePassword.ascx.cs
  * EditInfo.ascx.cs
  * N1Message.ascx.cs
  * N2Message1.ascx.cs
  * ProductAdminServiceProxy.aspx.cs
  * PchComponent.ascx.cs
  * DeviceRuntimeViewBase.cs
  * IDeviceRuntimeView.cs
  * IUniNavControl.cs
  * SlpLightboxes.ascx.cs

## Client-Side Mobile Logic

### Multipage.js
#### _References several functions for mobile decision logic_ 
  * `this.Settings.IsMobileApplicable()`
  * `MultiPage.prototype.OnNavClicked()`
  * `WireMobileAddToCartButton()`
  * `_multipage.Tabs.HideLoadingIcon()`
  * `HandleMobileCartActions`

#### _MultipageFactory_
```javascript
Create: function (isMobileApplicable) {
    this.CreateCommonComponents();
    isMobileApplicable ? this.CreateMobile() : this.CreateDesktop();
}
```

### MultiPageMobile.js
#### _Contains functions specific to mobile `MultiPage` features_
  * `TabsMobile`
  * `DisplayTabByIndex`
  * `GetWoaDeviceInteractionStatus`
  * `HideLoadingIcon`
  * `InitShowMoreProductsSpectrum`
  * `SmltClickHandler`
  * `ShowOrHideAdUnits`

#### _Contains function specific to the mobile navigation bar_
  * `OnClick`
  * `Display`
  * `HideContinueLink`
  * `ShowContinueLink`
  * `ShowContinueShopping`
  * `ShowCheckoutNow`
  * `HideContinueShopping`
  * `HideCheckoutNow`

#### _Fetch `ProductCount`, based on device type_
```javascript
ProductCount: SpectrumUtils.IsMobile() 
  ? this.ShowMoreProductsSpectrumSettings.NoOfMobileProducts
  : this.ShowMoreProductsSpectrumSettings.NoOfDesktopProducts
```

### Tabs.js
#### _Contains logic to hide promo image in mobile, and `MobileAddToCartButtons()` function_
```javascript
if (SpectrumUtils.IsMobile()) {
  this.HidePromoImage(tabIndex);
  return;
}
```

### Spectrum.js
#### _Return optimized images from Akamai for mobile users_
```javascript
if (self.IsMobile() && mpSettings.IsMobileOptimizationOn) {
  newImageUrl = imageUrl 
    + (self.HasQueryStringsParams(imageUrl) ? "&" : "?") 
    + self.StringFormat("output-quality={0}&output-format={1}",
    mpSettings.AkamaiOutputQuality,
    mpSettings.AkamaiOutputFormat);
  // If there is an offerType then pass the new image url with total 3 parameters
  if (offerType)
      newImageUrl += this.StringFormat("&resize={0}", (offerType == "MERCH") 
        ? mpSettings.AkamaiMerchResize 
        : mpSettings.AkamaiMagResize);

  return newImageUrl;
}
```

### sequenceController.js
#### _Client-side sequence controller logic_
```javascript
ctrl.desktopDiagService = ctrl.mobileDiagService = null;
ctrl.mobileDiagService.LoadDiagram(seq);
ctrl.mobileDiagService.LayoutDiagram();
diagramSerialized.MobileConnections = this.mobileDiagService.SerializeDiagram();
this.addMobileDiagram 
$scope.isMobileTabEnabled = true;
```

### SLP.js
#### _Returns images sized specificlly for mobile experience_ 
```javascript
window.SpectrumUtils.IsMobile()
```
```javascript
var imageUrl = window.SpectrumUtils.IsMobile() ? slp.MobileImageUrl : slp.ImageUrl;
```

### SpectrumDeviceManager.js
#### _Replaces devices for mobile experience_
```javascript
transformDevicePlacementForMobile(stepDevicePlacement)
```

### FusionProductsTab.js
#### _Uses the following functions to transform Spectrum for mobile_
  * `SpectrumUtils.IsMobile()`
  * `transformAdUnitByDeviceType()`
  * `adUnitJQuery.append(showMoreTmpl)`

### showMoreLikeThis.js
#### _SMLT requires a separate HTML template for mobile_
```javascript
if (SpectrumUtils.IsMobile()) {
  smltConfig = { 'openTopMargin': '130px',
                  'closeTopMargin': '0px', 
                  'openNorProdsMargin':'20px' }; 
  adUnitObj = $("<div  isrendered='false'  smltKey=\"" + placementId 
              + "_" + offerGroup.Id + "\" id=\"smlt_" + smltRequestType 
              + "OfferFeatureDiv_" + (x + 1) + "\" class=\" " 
              + getApplicableClass(applicableAdUnit) + " mainAdWrap sm-ad" 
              + (x) + "\"subGroupSource=\"smlt\" groupSource=\"" 
              + smltGroups[x].GroupSource + "\"" + elementId 
              + "\" GroupId=\"" + smltGroups[x].Id + "\">" 
              + "<div id=\"subAdWrapDiv\" class=\"subAdWrap " 
              + adUnits_getOfferType(smltGroups[x].Offers[0]) 
              + "\" runat=\"server\"></div>" + "</div>");
}
```

### Replay.js
#### _Uses the following functions to transform Spectrum app for mobile devices_
  * `SpectrumUtils.IsMobile()`
  * `WireMobileCartNotifications()`
  * `WireDesktopCartNotifications()`

### EoProductDevice.js
#### _Contains controls for the following mobile-specific features_
  * Mobile `addToCart` button
  * Mobile notifications
  * Mobile product description

### Cart.js
#### _Maintains a separate cart device for mobile_
```javascript
SpectrumUtils.IsMobile
if (!SpectrumUtils.IsMobile()) return _tab.CurrentTabIndex;
var isMobile = window.spectrumDeviceType === "MOBILE";
```

### AbandondedCart.js
#### _Use mobile template, if user is on a mobile device_
```javascript
this.IsMobile ? return mobile template
```

### allAdUnitsHelper.js
#### _Contains logic that transforms all adUnits into `mobileAdUnit`_

### ProductLightboxDisplay.js
#### _Resize mobile text content in `ProductLightbox`_
```javascript
if (isMobile) {
  truncateMobileBodyCopy(product);
  adUnitContainer = $('#adUnitModalMobile');
}
```

### EventTrackerProvider.js
```javascript
window.SpectrumUtils.IsMobile()
```

#### MultiAnalyticsProvider.js
```javascript
window.SpectrumUtils.IsMobile()
```

### SpectrumAnalytics.js
#### _We indicate to analytics whether or not user is on a mobile device_
```javascript
metricsData.IsMobile = window.SpectrumUtils.IsMobile();
```

## Style Sheets

### CSS classes are defined separately for desktop & mobile, via the following files
  * `ProductLightboxDisplay.css`
  * `showMoreLikeThis.css`
  * `AbandonedCart.css`
  * `ExpressCheckout.css`
  * `showMoreLikeThis.css`
  * `ProgressBar.css`
  * `Replay.css`
  * `SLP.css`
  * `TargetedProducts.css`
  * `mpCart.css`
  * `mpPagination.css`
  * `styles.css`

### Additionally `bootstrap.css` helps us optimize for mobile view
* media queries are used throughout our style sheets
  - ex. `@media (min-width: 768px)`

## Suggested Improvements
1. As can be observed from the above code snippets, when it comes to mobile rendering logic, Spectrum contains a fair amount of repetitive code. In keeping with the DRY principle of object oriented design, we could use inheritance to minimize such repetition. For example: classes for page elements in files like `EditInfo.aspx.cs` and `CreatePassword.ascx.cs` could inherit from an even deeper base class, which defines `public bool IsMobile { get; set; }`, as well as some global utility methods. If we were to introduce a tech debt item to seek out and consolidate repetitive mobile logic, we would discover an abundance of instances where inheritance would be beneficial. This would ultimately make our logic easier to follow moving forward.

2. Similarly, in order to increase the readability of our logic, we could benefit from gathering the code snippets featured above, and consolidating them into one CS file for the server-side, and one JS file for the client-side. On the server, we could call this file `MobileLogic.aspx.cs`, and all Spectrum page rendering calls could evaluate said logic. If the user is on a mobile device most, if not all, the transformation logic to serve a mobile page could be performed via this single file. This would make related defects simpler to debug. We could create a similar file called `MobileLogic.js` on the client-side, where we would gather the above JS snippets, and source all our front-end desktop/mobile decision logic from there directly.

3. Since Spectrum has been in development for almost a decade, we now have numerous tools at our disposal that were not around at the project's inception. In the past few years, several tools have been released that simplify the process of building responsive web pages. Although we've already integrated tools such as AngularJS & Bootstrap into Spectrum, we're not yet using them to their full capacity. If we were to redesign the Spectrum runtime with AngularJS or React, we could greatly reduce our effort when it comes to building responsive webpages, by making use of built-in framework features. Likewise, we could be using Bootstrap media queries more frequently on the Spectrum client-side, such that our markup would transform not only on the binary desktop/mobile condition, but would scale correctly for intermediary screen sizes, like tablets. Although it would be a tremendous undertaking to redesign Spectrum in this way, this would be an excellent consideration when it comes time to build the Nova runtime platform.

4. Finally, a lot has changed in terms of UI development best practices since the Spectrum project began. Now, mobile-first development is strongly encouraged, and the challenges we face in rendering Spectrum mobile elements serve as a testament to why mobile-first is a good idea. It's too late to redesign the Spectrum UI from the bottom up in this way, but I suggest we alter our development process to focus on mobile-first, when it comes to building the Nova runtime. We need to focus more on simplicity of design, adding elements sparingly, so they can scale smoothly and look appealing on screens of any size. It's also important that we consider our users' fingers, rather than only their mouse & cursor. Therefore, we need to be concious of spacing clickable elements appropriately. Since an increasing proportion of our users will be on touchscreen devices in the future, we need to start designing our interface with those users in mind. If we're able to design a smooth & responsive mobile UI, then the desktop portion will subsequently be simpler to build. Since the Nova admin platform makes heavy use of AngularJS, it would make sense to utilize the same when integrating Nova's runtime. That way, we could use AngularJS tools, in conjunction with Bootstrap media queries, to provide our users with responsive pages, while avoiding the complexities of writing explicit rendering logic, like we do in Spectrum.

