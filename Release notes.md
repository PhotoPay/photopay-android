# Release notes

## 9.0.0

### API changes
- We have added back `BarcodeRecognizer` and `RegexParser` to the PhotoPay SDK.

#### Recognizer changes
- Added `lastDateOfTheStandingOrder` result to `CzechiaQrCodeRecognizer.Result`
- Added `url` result to `HungaryQrCodeRecognizer.Result`

### Breaking changes

#### Minimum supported SDK version
- Minimum supported SDK version has been updated from 19 to 21. This means that the devices that have an Android version lower than Android 5.0 (Lollipop) will no longer support PhotoPay SDK - all of the devices with Android 5.0 and above are still supported.

#### Removed support for x86 architecture
- Devices that are based on the Intel x86 architecture, rather than ARMv7, are no longer supported. x86 and x86_64 architectures are used on very few devices today with most of them being manufactured before 2015, and only a few after that (e.g. Asus Zenfone 4). According to the Device catalog on Google Play Console, these devices make up about 1% of all Android devices (223 out of 22074 devices that have an API level of 21 and above support this architecture).


## 8.1.0.

### Major API changes
- Updates for `CroatiaPdf417PaymentRecognizer` and `CroatiaQrCodePaymentRecognizer` to enable conversion from HRK to EUR.
	-  We've added new result members: `amountHRK`, `amountEUR` and`conversionToEurPerformed`.
	-  We've added new setting member called `conversionRate` with the default value of 7,53540.
		-  We will make conversion to EUR only in the case when the currency in the barcode is HRK; `conversionToEurPerformed` will be `true` in that case, and `false` otherwise. Conversion from EUR to HRK will never be performed.
- We have removed HUB1 payment slip support from the `CroatiaPdf417PaymentRecognizer` and `CroatiaQrCodePaymentRecognizer` where these result members are no longer available:
	- `amount`,
	- `currency`,
	- `payerAccountNumber`,
	- `payerBankCode`,
	- `payerIban`,
	- `payerReferenceModel`,
	- `payerReference`.
- Support for the HUB1 payment slip is also removed for the `CroatiaSlipRecognizer`.

### Visual identity update
- We’ve updated Microblink logo and colors


## 8.0.0

### Major API changes
- The SDK now includes only `PhotoPay` recognizers and `BlinkInput` recognizer. For `BlinkID` recognizers, please import `BlinkID` SDK ([more here](https://github.com/BlinkID/blinkid-android#-sdk-integration)).
- To ensure compatibility with other Microblink SDKs, we have repackaged all classes. The root package has been renamed from `com.microblink` to `com.microblink.photopay`, which is unique to `PhotoPay` SDK.
- Removed support for Android 4.1 - 4.3 - minimum required Android version is now Android 4.4 (API level 19).

### Minor API changes
- Getters for some of the fields have been renamed according to the guidelines we have for all of our recognizers.
	- For example, `getIban` instead of `getIBAN`, `getBic` instead of `getBIC`, `getBlz` instead of `getBLZ`, etc.

### Changes to the PhotoPayRecognizer
- We’ve added support for 3 new fields to HungaryQRCodePaymentRecognizer:
	- `idCode`, `versionNumber`, `characterSet`
- We’ve added 2 new fields to CzQRCodePaymentRecognizer:
	- `paymentSituation`, `orderFrequency`

## 7.14.0

### Back side support added:
- Thailand - ID Card

### Changes to BlinkID(Combined) Recognizer
- Added new result members - `fathersName` and `mothersName` both in BlinkID and BlinkIDCombined Recognizers, as well as in VIZ result

### Improvements
- We can now extract `fathersName` and `mothersName` from Mexico Voter ID Card
- Australian Driving Licenses for New South Wales, Northern Territory, Queensland, Victoria and Western Australia now have the driver license unique card number field extracted as `document_additional_number`

### Changes to BarcodeRecognizer
- We’ve removed support for `aztec` and `dataMatrix` barcode formats from BarcodeRecognizer

### Changes to MRTDRecognizer
- Added `MRTD_TYPE_BORDER_CROSSING_CARD` to MrtdDocumentType enum 

### Fixes
- Fixed rare NullPointerException that happened only on some devices, caused by a lifecycle issue where the camera error callback was called after the view has already been destroyed


## 7.13.0

### Newly supported identity documents:

We’ve added 174 new documents:

#### Europe
- Albania - Driver Card (front only)
- Albania - Professional Driver License (front only)
- Belarus - Driver License (front only, beta)
- Belgium - Minors ID (beta)
- Czechia - Residence Permit
- Finland - Alien ID
- Finland - Residence Permit (beta)
- Georgia - Driver License (front only)
- Greece - Residence Permit
- Ireland - Passport Card
- Ireland - Public Services Card (beta)
- Kosovo - Driver License (front only, beta)
- Latvia - Alien ID
- Luxembourg - ID Card
- Moldova - ID Card (beta)
- North Macedonia - Driver License (front only)
- North Macedonia - ID Card
- Poland - Passport (beta)
- Slovenia - Residence Permit (beta)
- Spain - Alien ID
- UK - Passport (beta)
- North Macedonia - Polycarbonate Passport
- Austria - Paper Passport 
- Belarus - Paper Passport
- Belgium - Paper Passport (beta)
- Bulgaria - Paper Passport
- Estonia - Paper Passport
- France - Paper Passport (beta)
- Georgia - Paper Passport (beta)
- Germany - Paper Passport 
- Greece - Paper Passport
- Hungary- Paper Passport
- Italy - Paper Passport (beta)
- Kosovo - Paper Passport
- Moldova - Paper Passport (beta)
- Poland - Paper Passport
- Portugal - Paper Passport
- Spain - Paper Passport
- Switzerland - Paper Passport
- UK - Paper Passport

#### Middle East and Africa 
- Algeria - Driver License
- Burkina Faso - ID Card
- Cameroon - ID Card
- Democratic Republic Of The Congo - Driver License (front only, beta)
- Egypt - Driver License (beta)
- Ghana - ID Card
- Iraq - ID Card
- Ivory Coast - Driver License (front only, beta)
- Ivory Coast - ID Card
- Lebanon - ID Card (beta)
- Morocco - Driver License
- Mozambique - Driver License (front only, beta)
- Oman - Driver License (beta)
- Rwanda - ID Card (front only)
- Senegal - ID Card
- Tanzania - Driver License (front only, beta)
- Tunisia - Driver License (front only)
- Uganda - Driver License (front only, beta)
- Botswana - ID Card
- Sudan - Polycarbonate Passport
- Algeria - Paper Passport (beta)
- Egypt - Paper Passport (beta)
- Eswatini - Paper Passport 
- Ghana - Paper Passport
- Iran - Paper Passport (beta)
- Iraq - Paper Passport (beta)
- Israel - Paper Passport (beta)
- Jordan - Paper Passport (beta)
- Kenya - Polycarbonate Passport
- Libya - Polycarbonate Passport (beta)
- Morocco - Paper Passport (beta)
- Nigeria - Paper Passport
- Nigeria - Polycarbonate Passport (beta)
- Qatar - ID Card (front only, beta)
- Saudi Arabia - Paper Passport
- Syria - Paper Passport
- Tanzania - ID Card (beta)
- Tanzania - Voter ID (front only, beta)
- Tunisia - Paper Passport
- Turkey - Paper Passport
- Zimbabwe - Paper Passport

#### Latin America & the Caribbean
- Argentina - Alien ID
- Bahamas - ID Card (front only, beta)
- Bolivia - Minors ID
- Jamaica - Driver License
- Mexico - Residence Permit (beta)
- Mexico - Chiapas - Driver License (front only)
- Mexico - Coahuila - Driver License (beta)
- Mexico - Durango - Driver License(front only, beta)
- Mexico - Guerrero-cocula - Driver License (beta)
- Mexico - Guerrero-juchitan - Driver License (beta)
- Mexico - Guerrero-tepecoacuilco - Driver License (front only, beta)
- Mexico - Guerrero-tlacoapa - Driver License (front only, beta)
- Mexico - Hidalgo - Driver License
- Mexico - Mexico - Driver License (beta)
- Mexico - Morelos - Driver License (front only)
- Mexico - Oaxaca - Driver License
- Mexico - Puebla - Driver License (front only, beta)
- Mexico - San Luis Potosi - Driver License (front only)
- Mexico - Sinaloa - Driver License (front only, beta)
- Mexico - Sonora - Driver License (beta)
- Mexico - Tabasco - Driver License (beta)
- Mexico - Yucatan - Driver License (beta)
- Mexico - Zacatecas - Driver License (beta)
- Mexico - Baja California Sur - Driving License (beta)
- Mexico - Campeche - Driving License (beta)
- Mexico - Colima - Driving License (beta)
- Panama - Temporary Residence Permit (beta)
- Peru - Minors ID (beta)
- Trinidad And Tobago - Driver License (front only, beta)
- Trinidad And Tobago - ID Card
- Argentina - Paper Passport
- Brazil - Paper Passport (beta)
- Guatemala - Paper Passport
- Haiti - Paper Passport
- Honduras - Paper Passport (beta)
- Mexico - Paper Passport (beta)
- Mexico - Nayarit - Driving Licence (beta)

#### Oceania
- Australia - South Australia - Proof Of Age Card (front only, beta)
- Australia - Health Insurance Card (front only, beta)
- Australia - Paper Passport

#### Asia
- Armenia - ID Card
- Bangladesh - Driver License (beta)
- Cambodia - Driver License (front only, beta)
- India - Gujarat - Driving Licence (front only, beta)
- India - Karnataka - Driving Licence (front only)
- India - Kerala - Driving Licence (beta)
- India - Madhya Pradesh - Driving Licence (front only, beta)
- India - Punjab - Driving Licence (front only, beta)
- India - Tamil Nadu - Driving Licence (beta)
- Kyrgyzstan - ID Card
- Malaysia - Mypolis (beta)
- Malaysia - Refugee ID (front only)
- Myanmar - Driver License (beta)
- Pakistan - Punjab - Driving Licence (front only)
- Sri Lanka - Driving Licence (front only)
- Thailand - Alien ID (front only)
- Thailand - Driver License (beta)
- Uzbekistan - Driver License (front only, beta)
- Azerbaijan - Polycarbonate Passport (beta)
- Tajikistan - Polycarbonate Passport (beta)
- Bangladesh - Paper Passport
- China - Paper Passport (beta)
- India - Paper Passport
- Indonesia - Paper Passport
- Japan - Paper Passport
- Nepal - Paper Passport
- Pakistan - Paper Passport
- Philippines - Paper Passport
- South Korea - Paper Passport (beta)
- Sri Lanka - Paper Passport
- Uzbekistan - Paper Passport

#### Northern America
- Canada - Tribal ID (beta)
- Canada - Nova Scotia - ID Card (beta)
- Canada - Saskatchewan - ID Card (beta)
- USA - Border Crossing Card (front only)
- USA - Global Entry Card
- USA - Nexus Card (beta)
- USA - Veteran ID (front only)
- USA - Work Permit
- USA - Mississippi - ID Card (beta)
- USA - Montana - ID Card
- USA - New Mexico - ID Card
- USA - Wisconsin - ID Card
- Canada - Citizenship Certificate (beta)
- Canada - Ontario - Health Insurance Card (front only)
- Canada - Quebec - Health Insurance Card (front only, beta)
- USA - Military ID Card 
- USA - Rhode Island - ID Card
- USA - South Carolina - ID Card
- Canada - Paper Passport
- Canada - Weapon Permit (front only, beta)
- USA - Paper Passport (beta)

#### Back side support added:
- Hungary - Residence Permit
- Luxembourg - Residence Permit (no longer beta)
- Mauritius - ID Card
- Colombia - Alien ID (no longer beta)
- Mexico - Baja California - Driver License
- Mexico - Chihuahua - Driver License
- Mexico - Guanajuato - Driver License
- Mexico - Michoacan - Driver License
- Malaysia - MyKid
- Malaysia - MyPR
- Mexico - Puebla - Driving License 
- Singapore - S PASS
- Greece - ID Card
- Democratic Republic of the Congo - Driving Licence
- Mexico - Veracruz - Driving Licence

#### No longer beta:
- Albania - Passport
- Malta - Residence Permit
- Switzerland - Residence Permit
- Bolivia - Driver License
- Chile - Passport
- El Salvador - ID Card
- Peru - ID Card
- Singapore - S Pass
- Finland - Polycarbonate Passport
- Ireland - Polycarbonate Passport
- Kosovo - Driving License 
- Latvia - Polycarbonate Alien Passport
- Latvia - Polycarbonate Passport 
- Poland - Polycarbonate Passport 
- Cameroon - ID Card 
- Ghana - ID Card 
- Iraq - ID Card 
- Tanzania - Driving Licence
- Turkey - Polycarbonate Passport
- Uganda - Driving Licence
- Chile - Driving Licence
- Ecuador - Driving Licence
- Haiti - Driving Licence
- Belarus - Driving Licence
- UK - Polycarbonate Passport
- Bahamas - Driving Licence
- Mexico - Durango - Driving Licence
- Venezuela - ID Card
- USA - Kansas - ID Card

### Changes to the BlinkId(Combined)Recognizer
- You can now retrieve an image of the document owner along with cropped images of the document itself whenever you’re scanning an AAMVA-compliant ID: 

	- Using `BarcodeId` as a `RecognitionMode` lets you scan US driver licenses and IDs that BlinkID can’t read from the Visual Inspection Zone (VIZ) alone. Use it to extract:
		- A face image from the front side
		- Barcode data from the back side
		- Cropped document images of both sides
	- You can disable this `RecognitionMode` by setting `enableBarcodeId` to `false` in the `RecognitionModeFilter`.
- We've improved data extraction through the MRZ:
	- We now allow standard M/F values for gender on Mexican documents (along with localized H/M values)
- We're now converting dates to the Gregorian calendar for:
	- Taiwan documents with Republic of China (ROC) calendar dates
	- Saudi documents with Islamic calendar dates
- We're now auto-filling all ‘partial’ dates found on identity documents (showing year or month-year only):
	- Date of issue will be converted to the first day of the (first) month
		- E.g. '1999' will be converted to '01.01.1999.'
		- E.g. '03.1999.' will be converted to '01.03.1999.'
	- Date of expiry will be converted to the last day of the (last) month
		- E.g. '1999' will be converted to '31.12.1999.'
		- E.g. '03.1999.' will be converted to '31.03.1999.'
- We've added the parameter `maxAllowedMismatchesPerField` to settings. When this is set to a non-zero value, DataMatch will be successful as long as the number of mismatched characters doesn't exceed the specified value.
- We've added the parameter `allowUncertainFrontSideScan` to settings. When this parameter is set to true, the Recognizer will proceed scanning the back side of the document even if the front side scanning result `State` is `Uncertain`.
- We've enabled the return of image and back side data results, even when the `State` is `Uncertain`. Keep in mind that returned images, in this case, might be blurry or low quality.
	- This applies to all image types: full document image, face and signature image.
- We've added two separate fields for the processing status in the Recognizer Result: `frontProcessingStatus` and `backProcessingStatus`. They indicate the status of the last recognition process for each side.
- We’ve renamed the Swaziland country to Eswatini in results and ClassInfo
- Improved result validation
    - `FieldIdentificationFailed` processing status is used to indicate if unexpected fields are present on the document. Those fields are then deleted from the result
- We are filling out COUNTRY and REGION fields in ClassInfo, without the field TYPE of document, when using BarcodeID mode for scanning documents where the Front side is not supported, and back side results are extracted from AAMVA compliant barcodes
    - This applies only if `ClassInfo` isn’t already prepopulated in some other way and when you’re not in `FullRecognition` mode

### `BlinkId` improvements and fixes:
- We added support for the Malaysian NRIC numbers that hold an asterisk (*) character.
- While using `FullRecognitionMode` for scanning unsupported Passports, we are now extracting `ClassInfo` from MRZ 
- Improved quality of fully cropped vertical images
- Better parsing of Bermuda Driving License AAMVA-compliant barcode dates
- Fix for correct calculation of check digit for Saudi Arabia ID Card MRZ
- We are splitting first and last name from the additional name information (e.g., Nom d’ usage, Epouse, Geb. etc.)  into two different results. The additional name info will be a part of the `name_additional_info` field. This applies to the following documents:

	- France
		- ID Card
		- Residence Permit
	- Germany	
		- ID Card
	- Luxembourg
		- ID Card
	- Netherlands
		- Driving License
		- Polycarbonate Passport
		
- We are removing title prefixes (e.g., Mrs., Mr., Ing., etc.) from `full_name`, `first_name` and `last_name` for these documents:
	- Austria
		- Driving License
		- ID Card
	- Czechia 
		- Driving License
	- Germany
		- ID Card
	- Thailand 
		- ID Card
	- UK 
		- Driving License
- We can now extract the date of birth from the document number on the South Korean identity card and from the personal identification number on the driving licence

#### `BlinkId` Anonymization
- We've added anonymization for new documents:
	- Document number on Germany Polycarbonate Passport
	- Document number on Hong Kong Polycarbonate Passport
	- Document number and personal ID number on Singapore Polycarbonate Passport


### `BlinkCard` improvements and fixes:
- We've added a new feature that lets you anonymize extracted results, images or both:
	- Choose which fields you want to mask:
		- Card number
		- Card number prefix
		- CVV
		- Owner
		- IBAN
	- Choose a `BlinkCardAnonymizationMode` for each field:
		- `None`
		- `ImageOnly` - Black boxes will cover chosen data on the extracted image, results are not anonymized
		- `ResultFieldsOnly` - String data is redacted from the result, images are not anonymized
		- `FullResult` - Both images and string data (results) are anonymized
	- You can customize the way you anonymize the card number through `CardNumberAnonymizationSettings`:
		- `prefixDigitsVisible` - Defines how many digits at the beginning of the card number remain visible after anonymization
		- `suffixDigitsVisible` - Defines how many digits at the end of the card number remain visible after anonymization
- We've added support for new horizontal card layouts
- We've added support for vertical payment cards.
- As of this release, BlinkCard supports the Visa Quick Read format (the one where the card number spans through four lines).
- We've changed the threshold for "Camera too far" and "Camera too near" callbacks. From now on, the card needs to be closer to the camera.
- We updated the default BlinkCard scanning screen (`BlinkCardOverlayController` and `BlinkCardActivity`) which is activated when using BlinkCardUISettings. Now the UI supports landscape mode. 
- From now on, BlinkCard reads and extracts the expiry date in MM/YYYY format


### Changes to the `PhotoPayRecognizer`:
- removed the usage of konto_check library which was used for validating German account numbers on non-SEPA payment slips. 
- Removed support for non-SEPA payment slips in `DePaymentRecognizer`
- Removed `ACCOUNT_NUMBER` and `BANK_CODE` fields from `SlipRecognizer`
	- in SEPA payment slips they are part of the IBAN


### ImageCapture
- Added support for including or excluding groups of documents supported by the current license with the `captureModeFilter` setting, which also enables:
	- Scanning of documents with AAMVA compliant barcodes with BarcodeID mode
	- Scanning of all licensed documents in BlinkID FullRecognition mode

### Minor breaking API changes
- `Pdf417Recognizer` has been deprecated - use `BarcodeRecognizer` instead
- We've changed how `RecognizerRunner` processes images. Now it can treat `Image` objects as either video frame or photo frame. Until now, Direct API always processed images as photo frames, that not giving ability to recognizers to use time-redundant information for yield better recognition quality.

### Breaking change

To optimize our scanning process, we decided to remove support for processors which don't have VFPv4 support because it blocked us from implementing those optimizations.
This affects only a small number of older devices with armeabi_v7 CPUs. Those devices are rarely used.

### General fixes
- We’ve fixed camera issues for the following devices: 
	- Motorola Moto G100 - Camera 2 API wasn’t working
	- Realme X50 5G - Problem with Camera 2 API in legacy mode
	- LG K4 - Camera 2 API wasn’t working
	- Galaxy Tab Active 2 - broken aspect ratio


## 7.12.0

### Improvements

- We've improved QR code parsing in our `CzechiaQrCodeRecognizer`:
    - We're now supporting strings containing the null terminator in the middle.
- We've improved parsing Croatian reference numbers:
    - We're now supporting cases where the reference number contains spaces around the `-` delimiter

- **Newly supported identity documents**

    - Saudi Arabia - DL (front)
    - Saudi Arabia - Resident ID (front)

- We've introduced another variant of `resetRecognitionState` method that lets you reset the scanning process in different recognition stages:
- If you’re using our Direct API, call `RecognizerRunner.resetRecognitionState(false)` to clear the scanning cache as well as results gathered in a specific recognition step, i.e. after scanning the back side of a document.
- If you’re using our built-in UI to scan IDs from the camera stream, call `RecognizerRunnerView.resetRecognitionState(false)` to do the same.

### Changes to the BlinkId(Combined)Recognizer:

- We're now able to extract the additional address on Hungary Address Cards
- We've improved data extraction through the MRZ:
    - We now return the document type through `ClassInfo`, regardless of the `RecognitionMode` you're using (`MrzId`, `MrzPassport` or `MrzVisa`).
    - This means you can now use `ClassFilter` to filter these documents by their type.
    - We now return the document number on Nigeria IDs complete with its check digit.
    - We now support Italy Residence Permits with a *CR* document code.
- We've extended the `ClassInfo` structure with helper methods so you can filter documents by country more easily:
    - Use `countryName`, `isoNumericCountryCode`, `isoAlpha2CountryCode` and `isoAlpha3CountryCode` to get the full country names or their representative codes defined by [ISO](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes).
- We've extended the `BarcodeResult` structure with `extendedElements`
    - You can find all data from AAMVA-compliant barcodes under their respective `BarcodeElementKey` in the `BarcodeElements` structure
    - For a full list of keys please see `BarcodeElementKey` enum
- We've added another `ProcessingStatus` called `AwaitingOtherSide`
    - This status is triggered once BlinkID has finished with the first side of a document and expects the other side, too.
- We're now able to extract the date of birth from the CURP field on Mexico Voter IDs
- Direct API:
	- We've added a new recognition mode for recognizing still images of documents that have already been cropped:
		- Set the `scanCroppedDocumentImage` to true when you're feeding BlinkID images of documents that have already been cropped and don't require detection.
		- Keep in mind that this setting won't work on document images that haven't been properly cropped.


### Changes to the IdBarcodeRecognizer:

- We've extended the results with `extendedElements`
    - You can find all data from AAMVA-compliant barcodes under their respective `BarcodeElementKey` in the `BarcodeElements` structure
    - For a full list of keys please see `BarcodeElementKey` enum

### `BlinkCard` improvements and fixes:

- We’ve updated our default UI with new error messages shown if the user doesn’t place the card within the frame.
- We’re no longer hiding the security code on the edit results screen.
- We’ve added a new edit results screen customisation option, you can now change the toolbar icon displaying an arrow by default (`mb_blinkcardEditToolbarNavigationIcon` theme attribute).
- We’ve fixed an edit results screen issue causing user changes to become lost when the app goes to background.

### Deprecated recognizers:

-  We've deprecated `UsdlRecognizer`. Please use `IdBarcodeRecognizer` instead.

### Minor API changes

- We've replaced `Using time-limited license!` warning with `Using trial license!` warning. The warning message is displayed when using a trial license key.
- We’ve renamed `MrtdDocumentType.MRTD_TYPE_IDENITY_CARD` enum member to `MrtdDocumentType.MRTD_TYPE_IDENTITY_CARD`.

### Fixes:

- We've fixed the front-facing camera error on `Oukitel WP8 Pro`.
- We've fixed a rare crash in `BlinkIdOverlayController`, which happened as a result of rapidly launching the scanning process multiple times in a row.

## 7.11.0

### New features:
- We’ve updated SDK's Android **target API level to 30**.
- We’ve translated the complete SDK to the **Serbian** language.
- We’ve  made the SDK safe from **tapjacking**, a form of attack where a user is tricked into tapping something he or she didn’t intend to tap. We did this by adding a new security option that prompts the SDK to discard touches when the activity's window is obscured by another visible window. To activate it, use `UISettings.setFilterTouchesWhenObscured(true)`.
- We’ve added a new feedback message to users, reminding them to keep a document fully visible in case they accidentally cover a part of it with their finger or an object.
    - It appears while scanning the document by using `BlinkIdUISettings` and `BlinkIdOverlayController`:
    - It will display an error message "Keep the document fully visible".
    - This message is displayed only when using `BlinkIdRecognizer` or `BlinkIdCombinedRecognizer` for scanning.
- We’ve introduced a new `FrameRecognitionCallback` callback for the `RecognizerRunnerView`, which is invoked each time a camera frame from a video stream is recognized. Use `RecognizerRunnerView.setFrameRecognitionCallback` method to set the callback.
- We’ve added support for a new Top Up format, for A1 Croatia.

### Newly supported identity documents

- 53 documents added:
	- ALBANIA - DL (front)
	- BELGIUM - RESIDENCE PERMIT (front, back)
	- BOLIVIA - ID (front, back)
	- BOSNIA AND HERZEGOVINA - PASSPORT
	- CAMBODIA - PASSPORT
	- CANADA - RESIDENCE PERMIT (front, back)
	- CANADA - MANITOBA - ID (front)
	- CANADA - ONTARIO - HEALTH INSURANCE CARD (front)
	- CHILE - ALIEN ID (front, back)
	- CHINA - ID (front, back)
	- COLOMBIA - MINORS ID (front, back)
	- CYPRUS - RESIDENCE PERMIT (front, back)
	- CZECHIA - PASSPORT
	- GREECE - ID (front)
	- HAITI - ID (front, back)
	- ITALY - RESIDENCE PERMIT (front, back)
	- LATVIA - DL (front)
	- LATVIA - PASSPORT
	- LITHUANIA - PASSPORT
	- LUXEMBOURG - DL (front)
	- MONTENEGRO - DL (front)
	- MONTENEGRO - ID (front, back)
	- MONTENEGRO - PASSPORT
	- NETHERLANDS - RESIDENCE PERMIT (front, back)
	- NICARAGUA - ID (front, back)
	- NIGERIA - ID (front, back)
	- NORWAY - RESIDENCE PERMIT (front, back)
	- OMAN - RESIDENT ID (front, back)
	- PARAGUAY - DL (front, back)
	- PERU - DL (front, back)
	- PHILIPPINES - SOCIAL SECURITY CARD (front)
	- ROMANIA - PASSPORT
	- RUSSIA - PASSPORT
	- SERBIA - PASSPORT
	- SLOVAKIA - PASSPORT
	- SLOVENIA - PASSPORT
	- SOUTH KOREA - DL (front)
	- SPAIN - RESIDENCE PERMIT (front, back)
	- SWEDEN - RESIDENCE PERMIT (front, back)
	- THAILAND - PASSPORT
	- UKRAINE - DL (front)
	- UKRAINE - PASSPORT
	- USA - ARKANSAS - ID (front, back)
	- USA - CONNECTICUT - ID (front, back)
	- USA - GREEN CARD (front, back)
	- USA - MARYLAND - ID (front, back)
	- USA - MINNESOTA - ID (front, back)
	- USA - NEVADA - ID (front, back)
	- USA - NEW YORK CITY - ID (front, back)
	- USA - TEXAS - WEAPON PERMIT (front)
	- USA - VIRGINIA - ID (front, back)
	- VENEZUELA - DL (front)
	- VENEZUELA - PASSPORT

- Beta support added for 46 documents:
	- ALBANIA - PASSPORT
	- BAHAMAS - DL (front)
	- BERMUDA - DL (front)
	- BOLIVIA - DL (front)
	- CHILE - DL (front)
	- COLOMBIA - ALIEN ID (front)
	- DENMARK - RESIDENCE PERMIT (front, back)
	- DOMINICAN REPUBLIC - DL (front, back)
	- ECUADOR - DL (front)
	- EL SALVADOR - DL (front, back)
	- ESTONIA - RESIDENCE PERMIT (front, back)
	- GUATEMALA - DL (front, back)
	- HAITI - DL (front)
	- HONDURAS - DL (front, back)
	- HONDURAS - ID (front, back)
	- HUNGARY - ADDRESS CARD (front, back)
	- HUNGARY - RESIDENCE PERMIT (front)
	- ICELAND - DL (front)
	- ISRAEL - ID (front, back)
	- JAPAN - DL (front)
	- JORDAN - DL (front)
	- LATVIA - ALIEN PASSPORT
	- LATVIA - RESIDENCE PERMIT (front, back)
	- LUXEMBOURG - RESIDENCE PERMIT (front)
	- MALTA - RESIDENCE PERMIT (front, back)
	- MEXICO - BAJA CALIFORNIA - DL (front)
	- MEXICO - CHIHUAHUA - DL (front)
	- MEXICO - CIUDAD DE MEXICO - DL (front)
	- MEXICO - PROFESSIONAL DL (front)
	- MEXICO - GUANAJUATO - DL (front)
	- MEXICO - MICHOACAN - DL (front)
	- MEXICO - TAMAULIPAS - DL (front, back)
	- MEXICO - VERACRUZ - DL (front, back)
	- PHILIPPINES - TAX ID (front)
	- PHILIPPINES - VOTER ID (front)
	- POLAND - RESIDENCE PERMIT (front, back)
	- PORTUGAL - RESIDENCE PERMIT (front, back)
	- PUERTO RICO - VOTER ID (front)
	- SLOVAKIA - RESIDENCE PERMIT (front, back)
	- SOUTH KOREA - ID (front)
	- SWITZERLAND - RESIDENCE PERMIT (front, back)
	- TAIWAN - TEMPORARY RESIDENCE PERMIT (front)
	- TURKEY - RESIDENCE PERMIT (front)
	- USA - KANSAS - ID (front, back)
	- VENEZUELA - ID (front)
	- VIETNAM - DL (front)

- Added back side support for 7 documents:
	- ARGENTINA - ID
	- ECUADOR - ID
	- FINLAND - ID
	- NIGERIA - DL
	- QATAR - RESIDENCE PERMIT
	- URUGUAY - ID
	- USA - NEW YORK - DL

- 9 documents are no longer beta:
	- BRAZIL - DL
	- CANADA - ALBERTA - ID
	- MALAYSIA - MyKAS
	- MEXICO - NUEVO LEON - DL
	- PANAMA - DL
	- PORTUGAL - DL
	- SAUDI ARABIA - ID
	- SRI LANKA - ID
	- USA - IDAHO - ID


### New features and updates to the `BlinkIdRecognizer` and `BlinkIdCombinedRecognizer`

- We’re now able to read partial MRZ formats (2.5 lines), like the ones found on Switzerland and Liechtenstein DLs.
- We’ve added `documentOptionalAdditionalNumber` to the main part of the result, as well as front and back side VIZ results.
- We’ve expanded the set of possible recognizer states with `StageValid`. This state fixes `BlinkIDCombinedRecognizer` timeout issues, and enables better control of the Combined scanning pipeline. It activates when the first side of a document has been successfully scanned and scanning of the second side is required.

### New features and updates to the `BlinkCard`

- `BlinkCardRecognizer` is a Combined recognizer, which means it's designed for scanning **both sides of a card**. However, if all required data is found on the first side, we do not wait for second side scanning. We can return the result early. A set of required fields is defined through the recognizer's settings.

- "Front side" and "back side" are terms more suited to ID scanning. We start the scanning process with the **side containing the card number**. This makes the UX easier for users with cards where all data is on the back side.

- We've expanded the set of possible recognizer states with **StageValid**. This state is set when first side scanning completes with valid data, and second side scanning is required.

- Available `BlinkCardRecognizer` **settings**:
	- You can toggle mandatory **extraction** of all fields except the PAN.
	- You can enable the **blur filter**. When blur filtering is enabled, blurred frames are discarded. Otherwise, we process the blurred frames but set the blur indicator result member.
	- You can define required **padding** around the detected document. This ensures some empty space exists between the document and the edge of the frame.

- `BlinkCardRecognizer.Result` structure:
    - Contains:
        - The card issuer
        - PAN
        - PAN prefix
        - Expiry date
        - Owner information
        - IBAN
        - CVV
        - Cropped document images
        - Blur indicators for both sides
        - Processing status
	- **Processing status** can be one of:
		- Success - if the process ended successfully and data is valid
		- DetectionFailed - if detection of the document failed
		- ImagePreprocessingFailed - if preprocessing of the image failed
		- StabilityTestFailed - if inconsistent results were detected between different video frames (when video processing, we require at least two frames with consistent data, for image processing this isn't applicable)
		- ScanningWrongSide - if the first side presented in the scanning process does not contain the PAN, or when the user failed to present the second side
		- FieldIdentificationFailed - if we detected a field, but we're unable to parse it (possible glare issues, or a finger covering the field)
		- ImageReturnFailed - failed to return requested images
		- UnsupportedCard - this card layout is currently unsupported.

- We added a new BlinkCard screen that allows users to edit `BlinkCardRecognizer` scan results:
    - This screen allows users to edit scanned data and input data that wasn't scanned.
    - Enable it by calling `BlinkCardUISettings.setEditScreenEnabled(true)`.
    - Configure which fields should be displayed on this screen by using `BlinkCardUISettings.setEditScreenFieldConfiguration()` method.
    - Set your custom theme with `BlinkCardUISettings.setEditScreenTheme()` method.
    - Change default strings by using `BlinkCardUISettings.setEditScreenStrings()`.
    - To get user-edited fields, in your `onActivityResult(int requestCode, int resultCode, Intent data)` method call `BlinkCardEditResultBundle.createFromIntent(data)`.
    - This feature is available only for `BlinkCardRecognizer`.
    - If you are using a custom UI, you can launch edit screen by building intent with the following method `BlinkCardEditActivity.buildIntent()`.

- We updated the default BlinkCard scanning screen (`BlinkCardOverlayController`):
    - Instructions on how to reduce glare will be displayed when the user enables flashlight, you can disable it with `BlinkCardUISettings.setShowGlareWarning(false)`.
    - If the edit screen is enabled, a new button will show up after 5 seconds of unsuccessful scanning to allow the user to go directly to the edit screen.
- We have improved recognition timeout logic when using `BlinkCardRecognizer`.
    - When a credit card has multiple sides to scan, the timeout timer for the second side starts after the second side of the card has been detected. Previously, it has been started immediately after the first side has been scanned.
    - Timeout duration can be configured by using `RecognizerBundle.setNumMsBeforeTimeout`

### Improvements to existing features
- `CroatiaReferenceParser`
    - We’ve improved parsing of Croatian reference strings by adding additional validation of the model and reference fields. You can disable these validations by setting `allowUnverifiedReferences` to `true`.


### Deprecated recognizers

We have deprecated the following recognizers:

- `CroatiaIdFrontRecognizer`, `CzechiaIdFrontRecognizer`, `GermanyDlFrontRecognizer`, `GermanyIdFrontRecognizer`, `GermanyIdOldRecognizer`, `MalaysiaMyKasFrontRecognizer`, `SlovakiaIdFrontRecognizer`, `SwitzerlandDlFrontRecognizer` - **use `BlinkIdCombinedRecognizer` or `BlinkIdRecognizer` instead**

- `CroatiaCombinedRecognizer`, `CroatiaIdBackRecognizer`, `CzechiaCombinedRecognizer`, `CzechiaIdBackRecognizer`, `GermanyCombinedRecognizer`, `GermanyIdBackRecognizer`, `SlovakiaCombinedRecognizer`, `SlovakiaIdBackRecognizer` - **use `BlinkIdCombinedRecognizer` instead**

### Minor API changes:

- We have renamed old `BlinkCardRecognizer` and `BlinkCardEliteRecognizer` recognizers to `LegacyBlinkCardRecognizer` and `LegacyBlinkCardEliteRecognizer`. They are now deprecated.
- Interface `CombinedResult` does not provide the `getDocumentDataMatch()` method anymore, this method is moved to the new interface `CombinedDataMatchResult`.

### Fixes

- We’ve fixed an uncommon bug where you’d get incomplete results upon scanning of the MRZ with the `allowUnparsed` setting enabled.
- SDK does not require permission `android.permission.ACCESS_NETWORK_STATE` to unlock itself anymore, in cases when the license key needs online activation.

## 7.10.0

### New features:

- We added support for data extraction from **Hungary** QR code:
    - New `HungaryQrCodePaymentRecognizer`
    - Support for the Hungary QR code payment standard
- We have improved parsing of **Serbia** Barcode:
    - We improved parsing for the recipient name, sender’s name and address fields

- We translated complete SDK to the following additional languages: **Malay**, **Dutch**, **Hungarian**, **Slovenian**, **Indonesian**, **Arabic(UAE)**, **Romanian**, **Chinese traditional**, **Chinese simplified**, **Thai**, **Hewrew**, **Vietnamese**, **Filipino**.

- We added user feedback when turning ON the flashlight on `BlinkIdOverlayController`:
    - It warns the user to watch out for flashlight glare.
    - It can be disabled by using `BlinkIdUISettings.setShowFlashlightWarning(false)` option.

- New features and updates in BlinkId(Combined)Recognizer:
    - We added `signatureImage` to the result.
    - We enabled extraction of the **date of birth** from the **NRIC** from Malaysian documents.

- `BlinkIdRecognizer` and `BlinkIdCombinedRecognizer` now support new document types from different countries.
    - All supported document types are listed in `documentation/BlinkIDRecognizer.md`. Also check `documentation/BlinkIDRecognizerResult.md` for complete list of fields that we are extracting.

- We added the field `middleName` to `BlinkId(Combined)Recognizer`, `IdBarcodeRecognizer` and `Usdl(Combined)Recognizer` results. This field is extracted from AAMVA standard compliant barcodes, whenever available.

### Improvements to existing features:

- We have improved parsing of **MRZ** formats deviating from the ISO/IEC 7501 standard:
    - Document discriminator was in place of the document number on driver licenses and IDs from:
        - **New York**
        - **Michigan**
        - **Canada**
    - Different check digit calculation for **Mexico** (Consular) ID
    - Recognition of the unofficial `XCT` country code for Northern **Cyprus** ID
    - Recognition of different country codes and check digit calculation on **China** Mainland Travel Permit for Hong Kong and Macao Residents

- We have made some improvements to the **BlinkID(Combined)Recognizer**
   - We improved MRZ data extraction on **Russia Passport**.
   - We added anonymization support for more documents.
    - You can now see `ProcessingStatus` in the results to inspect potential processing errors, such as when barcode detection fails, a mandatory field is missing, etc.
    - You can now also see a more detailed `ImageAnalysisResult` showing you when:
        - Face image is detected
        - MRZ is detected
        - Barcode is detected
    - We added a `RecognitionModeFilter` settings group. You can toggle flags on this object to control the recognition mode of the recognizer:
        - `enableMrzId` lets you scan MRZ on all identity documents except visas and passports.
        - `enableMrzVisa` lets you scan MRZ on visa documents.
        - `enableMrzPassport` lets you scan MRZ on passports.
        - `enablePhotoId` lets you scan photo IDs. Use it to enable or disable document and face image extraction on unsupported documents.
        - `enableFullRecognition` lets you scan all data from our supported documents.
        - Your license key still controls which of the above recognition modes are allowed.
    - We have added a `RecognitionMode` result member describing which recognition mode was used to produce the results.
    -  We are now retrieving sex and nationality fields from the MRZ in cases where those two fields cannot be found in the document’s VIZ. Previously, we only used to do this for dates, name fields and document numbers.
    - We are now preserving the original string (raw data) of the dates we couldn’t parse.

- We have improved the thresholds for card detection feedback messages ("move closer" and "move farther"). This will improve the UX when scanning in landscape mode as the document can now be closer to the camera.

### Deprecated recognizers

We have deprecated the following recognizers:

- `PassportRecognizer`, `VisaRecognizer`, `AustraliaDlFrontRecognizer`, `AustriaDlFrontRecognizer`, `AustriaIdFrontRecognizer`, `BelgiumIdFrontRecognizer`, `BruneiIdFrontRecognizer`, `ColombiaDlFrontRecognizer`, `ColombiaIdFrontRecognizer`, `CyprusIdFrontRecognizer`, `CyprusOldIdFrontRecognizer`, `EgyptIdFrontRecognizer`, `EudlRecognizer`, `HongKongIdFrontRecognizer`, `IndonesiaIdFrontRecognizer`, `IrelandDlFrontRecognizer`, `ItalyDlFrontRecognizer`, `JordanIdFrontRecognizer`, `KuwaitIdFrontRecognizer`, `MalaysiaDlFrontRecognizer`, `MalaysiaIkadFrontRecognizer`, `MalaysiaMyKadFrontRecognizer`, `MalaysiaMyPrFrontRecognizer`, `MalaysiaMyTenteraFrontRecognizer`, `MexicoVoterIdFrontRecognizer`, `MoroccoIdFrontRecognizer`, `NewZealandDlFrontRecognizer`, `PolandIdFrontRecognizer`, `RomaniaIdFrontRecognizer`, `SingaporeDlFrontRecognizer`, `SingaporeIdFrontRecognizer`, `SloveniaIdFrontRecognizer`, `SpainDlFrontRecognizer`, `SwedenDlFrontRecognizer`, `SwitzerlandIdFrontRecognizer`, `UnitedArabEmiratesIdFrontRecognizer`, `UnitedArabEmiratesDlFrontRecognizer ` - **use `BlinkIdCombinedRecognizer` or `BlinkIdRecognizer` instead**

- `DocumentFaceRecognizer`, `MrtdRecognizer` - **use `BlinkIdRecognizer` instead**

- `MrtdCombinedRecognizer`, `AustraliaDlBackRecognizer`, `AustriaCombinedRecognizer`, `AustriaIdBackRecognizer`, `BelgiumCombinedRecognizer`, `BelgiumIdBackRecognizer`, `BruneiIdBackRecognizer`, `ColombiaIdBackRecognizer`, `CyprusIdBackRecognizer`, `CyprusOldIdBackRecognizer`, `JordanCombinedRecognizer`, `JordanIdBackRecognizer`, `KuwaitIdBackRecognizer`, `MalaysiaMyKadBackRecognizer`, `MoroccoIdBackRecognizer`, `NigeriaCombinedRecognizer`, `NigeriaVoterIdBackRecognizer`, `PolandCombinedRecognizer`, `PolandIdBackRecognizer`, `SingaporeCombinedRecognizer`, `SingaporeIdBackRecognizer`, `SloveniaCombinedRecognizer`, `SloveniaIdBackRecognizer`, `SwitzerlandIdBackRecognizer`, `UnitedArabEmiratesIdBackRecognizer` - **use `BlinkIdCombinedRecognizer` instead**

### Major API change

`ScanResultListener` interface now has an additional method called when the scanning cannot continue because of an unrecoverable error. You have to implement `onUnrecoverableError` method.
If you’re using built-in activities, when `onActivityResult` is called with `RESULT_CANCELED` result code, the exception will be available via `ActivityRunner.EXTRA_SCAN_EXCEPTION` intent extra. If the user canceled the scan, the exception would be `null`.

### Minor API Changes

- We have made some changes to the `BlinkIDRecognizer` and `BlinkIDCombinedRecognizer`:
    - We renamed `DocumentImageMoireStatus` to `ImageAnalysisDetectionStatus`
    - We grouped the `conditions` member from the results with the `DriverLicenseDetailedInfo` structure
- We renamed `RecogitionMode` to `RecognitionDebugMode` in `RecognizerBundle`.

### Fixes

- We improved the data match logic for **Guatemala Consular ID** in BlinkId(Combined)Recognizer.
- We fixed race conditions in camera management, which in some cases caused that the camera was unable to resume after it has been paused.
- We fixed an issue in default UI that was causing reticle to reappear while card flip animation was showing.
- We fixed an issue that was causing some methods marked with `@NonNull` to return `null`.
- We made camera permission handling update for Android 11.

## 7.9.0

### New features:

- In `BlinkIdCombinedRecognizer` and `BlinkIdRecognizer` we added:
    - Support for obtaining full document image for IDs with barcodes. Now you can capture document image and extract barcode data with a single scan.
    - Scanning & data extraction for  travel visas and passports.
    - Field validation - we validate if the results from certain fields match predefined character sets.
        - If validation fails, the recognizer's state is `Recognizer.Result.State.Uncertain`.
        - Use `setValidateResultCharacters()`to enable or disable validation.

    - Support for US documents with **vertical** orientations:
        - Alabama DL
        - Arizona DL
        - California DL
        - Colorado DL
        - Connecticut DL
        - Georgia DL
        - Illinois DL
        - Iowa DL
        - Kansas DL
        - Kentucky DL
        - Maryland DL
        - Massachusetts DL
        - Minnesota DL
        - Missouri DL
        - New Jersey DL
        - Ohio DL
        - Pennsylvania DL
        - South Carolina DL
        - Tennessee DL
        - Texas DL
        - Utah DL
        - Washington DL
        - Wisconsin DL

    - Support for new document types:
        - Australia New South Wales - ID Card / Front only / BETA
        - Brazil - Driver License / BETA
        - Brunei - Military ID / BETA
        - Brunei - Residence Permit / BETA
        - Brunei - Temporary Residence Permit / BETA
        - Croatia Health Insurance Card / front side / BETA
        - Ecuador ID / front side
        - El Salvador ID / BETA
        - Ghana - Driver License / Front only
        - Latvia - ID Card
        - Norway - Driving Licence / Front only / BETA
        - Oman - ID Card
        - Saudi Arabia - ID Card / BETA
        - Sri Lanka ID / BETA
        - Sweden - Social Security Card / Front only
        - USA - Social Security Card / BETA
        - Back side supported:
            - Malaysia - MyTentera

    - No longer BETA:
        - Australia Tasmania - Driving Licence
        - Canada British Columbia - ID Card
        - Canada Nova Scotia DL
        - Canada Yukon DL        - Germany - Residence Permit
        - Morocco - ID Card
        - Nigeria - Voter ID
        - Norway DL
        - Singapore - Work Permit
        - USA Alaska - ID Card
        - USA District Of Columbia - Driver License
        - USA Indiana - ID Card
        - USA Kentucky - ID Card

    - Back side support:
        - Kenya ID

    - Barcode scanning on the following documents:
        - Argentina ID
        - Colombia ID
        - Nigeria Voter ID
        - South Africa ID

    - **Result anonymization** - with this option enabled, results are not returned for protected fields on documents listed here. The full document image will also have this data blacked out.
        - Protected fields are:
            - Document number on Hong Kong ID
            - MRZ on Hong Kong passports
            - Personal ID number on Netherlands DL
            - Personal ID number and MRZ on Netherlands ID
            - MRZ on Netherlands passports
            - Document number on Singapore DL, ID, Fin Card, Resident ID
            - Personal ID number on Singapore Employment Pass
            - Document number and personal ID number on Singapore Work Permit
            - MRZ on Singapore passports.
        - By using `setAnonymizationMode` method, you can choose the `AnonymizationMode` : `ImageOnly`, `ResultFieldsOnly`, `FullResult` or `None`.
        - `FullResult` anonymization (both images and data) is set by default.

- We added support for new **MRZ** formats:
    - Guatemala ID
    - Kenya ID

### Improvements for existing features:

- Improvements in `BlinkIdCombinedRecognizer` and `BlinkIdRecognizer`:
    - Documents discarded with the class filter are now reported as not supported
        - `ClassifierCallback.onDocumentSupportStatus(false)` will be called if a documents is filtered out by the `ClassFilter`
    - For Malaysian MyKad we are now returning if a Moire pattern is present on the scanned document (detected or not detected).
        - use `getImageAnalysisResult().getDocumentImageMoireStatus()` in `BlinkIdRecognizer`.
        - use `getFrontImageAnalysisResult().getDocumentImageMoireStatus()` and `getBackImageAnalysisResult().getDocumentImageMoireStatus()` in `BlinkIdCombinedRecognizer `.

- We made changes to the result structure of `BlinkIdCombinedRecognizer` and `BlinkIdRecognizer`:
    - Barcode data is now encapsulated in its own result structure: `BarcodeResult`.
    - Data from all OCR-ed fields, without MRZ data, is now encapsulated in a `VizResult` structure, representing the "Visual Inspection Zone" data. In `BlinkIdCombinedRecognizer`, front side data is available in its own structure (`frontVizResult`), back side data in its own (`backVizResult`), so you can now **access data from each side separately**.
    - The main part of the result, outside these structures, is filled according to these rules:
        - Document number is filled with data from the MRZ, if present.
        - Remaining data is then filled with barcode data.
        - Remaining data is filled from the back side's visual inspection zone (OCR data outside of MRZ).
        - Remaining data is filled from the front side's visual inspection zone.
        - Remaining data is filled with data from the MRZ.

- We added digital signature support to `PassportRecognizer`.
- We updated `IdBarcodeRecognizer.Result` with specific driving license info.
    - Use `getRestrictions()`, `getEndorsements()` and `getVehicleClass()`.
- We updated `UsdlRecognizer.Result` and `IdBarcodeRecognizer.Result` with additional address fields:
    - `street`, `postalCode`, `city` and `jurisdiction`
- We added `isExpired()` method to `BlinkIdRecognizer.Result`, `BlinkIdCombinedRecognizer.Result` and `IdBarcodeRecognizer.Result`.
    - It compares the current time on the device with the date of expiry and checks whether the document has expired or not.
- We enabled usage of combined recognizers through the Direct API:
    - Recognition state is not automatically reset with every image that comes to the recognition through the Direct API. If reset is required, you should call `RecognizerRunner.resetRecognitionState()`

### Minor API changes:

- We renamed `EMailParser` to `EmailParser`.
- We renamed some methods:
    - In `LicensePlatesParser`: `getLicensePlateString` to `getLicensePlate`.
    - In `RegexParser`: `isUsingSieve` to `shouldUseSieve`, `setMustStartWithWhitespace ` to `setStartWithWhitespace `, `isMustStartWithWhitespace ` to `shouldStartWithWhitespace `, `setMustEndWithWhitespace ` to `setEndWithWhitespace `, `isMustEndWithWhitespace ` to `shouldEndWithWhitespace `.
    - In `RawParser`: `isUsingSieve` to `shouldUseSieve`.
- We moved `BlinkIdRecognizer.Result` member `colorStatus` to the result's `imageAnalysisResult` (`frontImageAnalysisResult` and `backImageAnalysisResult` in `BlinkIDCombinedRecognizer.Result`).
- We changed default `IntentDataTransferMode` to `IntentDataTransferMode.PERSISTED_OPTIMISED`. It can be configured by using `MicroblinkSDK.setIntentDataTransferMode`.

### Bug fixes:

- We fixed bug in IBAN parsing which caused that reference number was sometimes read as part of the IBAN number.
- We fixed amount extraction in `SerbiaQrCodePaymentRecognizer` and `SerbiaPdf417PaymentRecognizer`.
- We fixed bug in `BlinkIdCombinedRecognizer` and `BlinkIdRecognizer` which caused that dates on Belgian ID cards are not parsed correctly in cases when month is July.
- We fixed US driver's license address extraction (Oregon, Mississippi, Rhode Island).
- We fixed a bug which caused that, in some cases, camera has not been resumed after the device screen was turned OFF and back ON.

## 7.8.0

### New features:

- We added a new recognizer specialized for scanning and parsing barcodes on various identity cards - `IdBarcodeRecognizer`. Supported document types are:
    - AAMVA compliant (US DL, Canada DL, etc.)
    - Argentina ID
    - Panama ID
    - Colombia ID
    - South Africa ID
    - Nigeria Voter ID and driver license
- We added age verification feature:
    - Now you can more easily obtain the age of the document owner in years and check whether it is above some age limit.
    - Use `age` and `ageLimitStatus` helper methods in `MrzResult`, `BlinkIdRecognizer.Result`, `BlinkIdCombinedRecognizer.Result`, `UsdlRecognizer.Result`, `UsdlCombinedRecognizer.Result`, and `IdBarcodeRecognizer.Result`.
- We added the option to disable Microblink logs in the console output. Use `LoggingSettings.disableMicroblinkLogging()`. Be careful with this option. We need full log outputs from the application for support purposes. In case of having problems with scanning certain items, undesired behavior on the specific device(s), crashes inside SDK or anything unmentioned, we will need a full log from your side. If you disable Microblink logging, you won't be able to provide us this information. Hence support might be limited.

### Improvements:

- We have translated complete SDK to following languages: **Croatian**, **Czech**, **English**, **French**, **German**, **Italian**, **Portuguese**, **Slovak**, and **Spanish**.
- We added support for non-standard (floating point) amounts in QR code
- We enabled reading of ITF barcodes with length 4
- We added support for new document types in `BlinkIdCombinedRecognizer` and `BlinkIdRecognizer`:
    - Australia - Australian Capital Territory - Driving licence / front only
    - Australia - Northern Territory - Driving licence / BETA
    - Australia - Tasmania - Driving licence / front only / BETA
    - Canada - Alberta - ID card / BETA
    - Canada - British Columbia - Driver license / Public services card (Combined)
    - Canada - British Columbia - ID card / BETA
    - Canada - British Columbia - Public services card
    - Canada - New Brunswick - Driving license
    - Canada - Nova Scotia - Driving license / BETA
    - Canada - Yukon - Driving license / BETA
    - Panama - Driving license / front only / BETA
    - Panama - ID card / front only
    - Singapore - Work permit / BETA
    - Taiwan - ID card / front only / BETA
    - USA - Alabama - ID card
    - USA - Alaska - ID card / BETA
    - USA - District Of Columbia - Driver license / BETA
    - USA - Idaho - ID card / BETA
    - USA - Indiana - ID card / BETA
    - USA - Kentucky - ID card / BETA
    - USA - Massachusetts - ID card
    - USA - Oregon - ID card
    - USA - Washington - ID card
    - We added support for back side to:
        - Australia - Western Australia - Driving licence
        - Mexico - Voter ID
        - Netherlands - Driving licence
- Additional improvements in `BlinkIdCombinedRecognizer` and `BlinkIdRecognizer`:
    - When the back side of the document is not fully supported by the `BlinkIdCombinedRecognizer`, we will capture and return the back side image without performing data extraction. You can disable this behaviour by using `BlinkIdCombinedRecognizer.setSkipUnsupportedBack(true)`.
    - We are now returning color status for the scanned document (black and white or color) in the following result fields:
        - `documentImageColorStatus` in `BlinkIdRecognizer.Result`.
        - `documentFrontImageColorStatus` and `documentBackImageColorStatus` in `BlinkIdCombinedRecognizer.Result`.
    - We are now returning `ClassInfo` which holds the following information about the scanned document: `Country`, `Region`, and `Type` of the document. Use  `BlinkIdRecognizer.Result.getClassInfo()` and `BlinkIdCombinedRecognizer.Result.getClassInfo()`.
    - We introduced `ClassFilter` which determines whether a document should be processed or is filtered out, based on its `ClassInfo`. Use  `BlinkIdRecognizer.setClassFilter` and `BlinkIdCombinedRecognizer.setClassFilter` to enable it.
    - To improve the scanning performance, we added additional feedback for users that ensures a detected document is entirely inside the frame. When a document is too close to the edge of the camera frame, we will display an appropriate message to the user in `BlinkIdOverlayController`. You can configure the minimum distance from the edge of the frame by using the `paddingEdge` settings method.
    - We added new recognizer options: `allowUnparsedMrzResults` and `allowUnverifiedMrzResults`
    - We added new result field: `dateOfExpiryPermanent`
- We added new result fields in `MrzResult`, returned by all recognizers which scan MRZ (Machine Readable Zone):
    - `issuerName`
    - `nationalityName`

- Improvements in `BlinkIdOverlayController`:
    - When a document is too close to the edge of the camera frame, we display *`Move the document from the edge`* message.
    - We added better user instructions when barcodes are being scanned in `UsdlCombinedRecognizer`. We display *`Scan the barcode`* message.
- We improved document detection with `DocumentCaptureRecognizer`.
- We are now delivering the complete list of open source dependencies used in the SDK. Please check the `open-source-software-used` directory.

### Minor API changes:

- We removed `RecognizerRunnerView` custom attributes: `mb_initialOrientation` and `mb_aspectMode`. Use `RecognizerRunnerView.setInitialOrientation` and `RecognizerRunnerView.setAspectMode` to configure the attributes in the code.
- All provided scan activities extend `AppCompatActivity`

### Bug fixes:

- We fixed bug in `BlinkIdOverlayController` which caused that `MrtdRecognizer.Result` is cleared after scanning is done and empty result is returned.
- We fixed crash when using Direct API on high resolution `com.microblink.image.Image` from `HighResImageWrapper

## 7.7.0

### Major API changes:

- SDK has been migrated to **AndroidX** dependencies - previous SDK dependency com.android.support:appcompat-v7 has been replaced with  **androidx.appcompat:appcompat**

### New features:

- added support for capturing cropped images (without data extraction) of documents of any format:
    - use `DocumentCaptureRecognizer` and `DocumentCaptureUISettings`
    - `DocumentCaptureUISettings` launches activity that uses `DocumentCaptureOverlayController`, which is designed for taking **high resolution** document images and guides the user through the image capturing process. It can be used only with `DocumentCaptureRecognizer`.
- `BlinkIdRecognizer` and `BlinkIdCombinedRecognizer` now support new document types from different countries, all supported document types are listed [here](https://github.com/PhotoPay/photopay-android/blob/a8ba6b1efd9314212477e4c702fed7f4de53d1fb/documentation/BlinkIDRecognizer.md)
- Updated `BelgiumCombinedRecognizer`:
    - added `nationalRegisterNumber` to result
- added support for reading front and back side of Nigerian Voter ID card - use `NigeriaCombinedRecognizer`
- new options in `BlinkIdUISettings`, `DocumentUISettings`, `DocumentVerificationUISettings` and `BlinkIdOverlaySettings`:
    - option to disable displaying of "Document Not Supported" dialog when `BlinkIdRecognizer` or `BlinkIdCombinedRecognizer` is used in combination with other recognizers - use method `setShowNotSupportedDialog`
    - option to configure back side scanning timeout - use `setBackSideScanningTimeoutMs`
- it is possible to set theme that will be used by activity, launched from the UISettings - use UISettings.setActivityTheme

### Improvements:

- **overall size impact on application reduced for almost 5 MB** when PhotoPay  SDK v7.7 is used, relative to size impact of the previous v7.5
- improved `SwitzerlandSlipRecognizer`:
    - new result member `ocrLineResult` which returns raw OCR line
- improved `BelgiumSlipRecognizer`:
    - better `amount` and `reference` extraction
    - now it is possible to enable/disable reading of free form, unstructured references - use `BelgiumSlipRecognizer.setFreeFormReferenceEnabled`
- improved `BlinkCardRecognizer`:
    - now extracts IBAN from the Payment / Debit card
- added option to anonymize Netherlands MRZ area in document image returned by the `PassportRecognizer` - use `PassportRecognizer.setAnonymizeNetherlandsMrz(true)`
- enabled setting `MrzFilter` on `MrtdCombinedRecognizer`:
    - determines whether document should be processed or it is filtered out, based on its MRZ (Machine Readable Zone)
    - this feature is also available for `MrtdRecognizer`
- added property `localizedName` to `BlinkIdRecognizer.Result`, `BlinkIdCombinedRecognizer.Result` and `HongKongIdFrontRecognizer.Result` (CCC to chinese alphabet conversion for Hong Kong ID)
- enabled digital signing of `BlinkIdCombinedRecognizer.Result`
- improved `VinParser`:
    - added support for Renewal Identification Number (RIN) - DMV California format
- added new fields in `MrzResult`:
    - `sanitizedDocumentCode`
    - `sanitizedDocumentNumber`
- improved `BlinkIdRecognizer` and `BlinkIdCombinedRecognizer`:
    - introduced blur filter that discards blurred frames and prevents reading data from them. This option is enabled by default, it can be disabled by using `BlinkIdRecognizer.setAllowBlurFilter(false)` and `BlinkIdCombinedRecognizer.setAllowBlurFilter(false)`
- improved camera performance on some Samsung devices

### Minor API changes:

- `MrzFilter` now accepts `MrzResult` for filtering, previously filtering has been performed based on the given `MrtdRecognizer` result.
- `RecognizerRunner.getSingletonInstance()` does not throw `FeatureNotSupportedException` anymore
- all scan activity classes are final now
- in combined recognizers results, `documentDataMatch` value is now returned as `DataMatchResult` enum with three possible values: `NotPerformed`,  `Failed` and `Success`
- new API for configuring camera options on `UISettings` -  use `UISettings.setCameraSettings`, which accepts object of `CameraSettings` type


### Bug fixes:

- fixed issue with utf8 encoding in SloveniaQrCodePaymentRecognizer
- fixed problems with aspect ratio of camera preview on Huawei Mate 10
- fixed bug in `BlinkCardRecognizer`:
    - `anonymizeCvv` now works independently of any other anonymization setting
- `BlinkIdCombinedRecognizer` - fixed issue when the front side of a document was returned as a back side

## 7.5.0

### New features:

- added `BlinkIdRecognizer` for scanning front side of ID cards and `BlinkIdCombinedRecognizer` for combined scanning of front and back side of ID cards
    - for now, these recognizers classify and extract data from **87** different classes of **United States driver's licenses and IDs** (front and back side)
    - list of all supported document types can be found [here](https://github.com/PhotoPay/photopay-android/blob/0bfd5f791520af013357c8c686e9fee1c0f5d75a/BlinkIDRecognizer.md)
    - in the upcoming releases, we are planning to add support for more document types from different countries
- completely new UX for scanning ID cards with new scan activity and overlay: `BlinkIdActivity` and `BlinkIdOverlayController`:
    -  best suited for scanning with `BlinkIdRecognizer` and `BlinkIdCombinedRecognizer`
    - other single side and combined document recognizers are also supported
- added support for reading back side of Nigerian Voter ID card - use `NigeriaVoterIdBackRecognizer`
- added support for reading front and back side of Belgium ID - use `BelgiumIdFrontRecognizer`, `BelgiumIdBackRecognizer` and `BelgiumCombinedRecognizer`
- added support for reading all visa documents containing Machine Readable Zone - use `VisaRecognizer`

### Improvements for existing features:

- improved `MrtdRecognizer`:
    - added support for documents with non-binary gender specification (symbol X)
- improved `DocumentFaceRecognizer`:
    - improved scanning time (faster scan)
    - added support for vertical IDs
    - removed the `tryBothOrientations` option (improved scan in all directions is enabled by default)
- improved scanning time (faster scan) for `PassportRecognizer`
- improved `RomaniaIdFrontRecognizer`
    - now extracts `CNP` number
- improved `SloveniaIdFrontRecognizer` and `SloveniaCombinedRecognizer`:
    - return boolean flag which indicates whether **date of expiry** is permanent - use `SloveniaIdFrontRecognizer.Result.isDateOfExpiryPermanent()` and `SloveniaCombinedRecognizer.Result.isDateOfExpiryPermanent()`
- improved `GermanyPassportRecognizer`:
    - better passport classification
- improved `ColombiaIdFrontRecognizer`:
    - support for document number in format 1-3-3
- improved `SlovakiaIdFrontRecognizer`:
    - support for German letters
- Malaysia:
    - `MalaysiaMyTenteraFrontRecognizer` supports 6-digit army number
    - `MalaysiaIkadFrontRecognizer` - better extraction of the following fields (**DeepOCR** support): date of birth, sector, employer, address and date of expiry
- United Arab Emirates:
    - glare detection is disabled by default for `UnitedArabEmiratesIdFrontRecognizer` and `UnitedArabEmiratesIdBackRecognizer`
    - `UnitedArabEmiratesIdBackRecognizer` - optimized detection for black backgrounds

### Minor API changes:

- renamed following recognizers:
    - `CroatiaPdf417Recognizer` to `CroatiaPdf417PaymentRecognizer`
    - `CroatiaQrCodeRecognizer` to `CroatiaQrCodePaymentRecognizer`
    - `SepaQrCodeRecognizer` to `SepaQrCodePaymentRecognizer`
    - `SlovakiaCode128Recognizer` to `SlovakiaCode128PaymentRecognizer`
    - `SlovakiaDataMatrixRecognizer` to `SlovakiaDataMatrixPaymentRecognizer`
    - `SlovakiaQrCodeRecognizer` to `SlovakiaQrCodePaymentRecognizer`
    - `SloveniaQrCodeRecognizer` to `SloveniaQrCodePaymentRecognizer`
    - `SwitzerlandQrCodeRecognizer` to `SwitzerlandQrCodePaymentRecognizer`
    - `UnitedKingdomQrCodeRecognizer` to `UnitedKingdomQrCodePaymentRecognizer`
- overlay controllers are no longer using UI settings, they're now using Overlay settings,  you can create Overlay controller from UI settings by calling method `uiSettings.createOverlayController`
- `PhotopayOverlayController`, `DocumentOverlayController` and `BarcodeOverlayController` are replaced with single overlay controller - `BasicOverlayController`
- moved some classes to new packages

### Removed features:

- removed `WindowedOcrLineUISettings`
- `BarcodeScanActivity` no longer supports results dialog
- verification activities no longer support action bar built in obsolete activity themes

### Bug fixes:

- fixed bug in `SloveniaQrCodePaymentRecognizer` which caused invalid parsing
- all default scan activities correctly set volume to media instead of ring
- all default scan activities now apply secure flag if enabled in ui settings

## 7.4.0

### New features:

- added support for reading all passports with MRZ - use `PassportRecognizer`

### Improvements for existing features:

- added `tryBothOrientations` option to `DocumentFaceRecognizer` which defines whether document will be scanned in both orientations (normal and upside down)
- `GermanyCombinedRecognizer.Result`:
    - added getter for the full MRZ string: `getRawMrzString`
- added support for reading commercial code in two rows for `HongKongIdFrontRecognizer`
- added support for `HongKongIdFrontRecognizer` 2018 version
- improved reading accuracy for the following recognizers (**DeepOCR** support):
`MalaysiaIKadFrontRecognizer`
- improved scanning time of all Malaysian ID front recognizers: `MalaysiaMyKadFrontRecognizer`, `MalaysiaMyKasFrontRecognizer`, `MalaysiaMyPrFrontRecognizer`, `MalaysiaMyTenteraFrontRecognizer`

### Minor API changes:

- Scanning timeout that can be configured by using `RecognizerBundle.setNumMsBeforeTimeout` is by default set to `RecognizerBundle.TIMEOUT_INFINITY`, which means that timeout is disabled by default. Previous default timeout value was 10 seconds.

### Bug fixes:

- `ColombiaIdBackRecognizer` - fixed result strings encoding problem
- DPI options on images are now correctly applied to dewarped image results in `DocumentFaceRecognizer.Result`
- fixed a validation issue for the gender field in `SloveniaCombinedRecognizer`
- fixed scanning bug for devices with problematic camera resolution, which caused that SDK was unable to scan data, known affected devices were: `OnePlus 6T`, `OnePlus 7 Pro` and `Vivo V15`

## 7.3.0

**Important notice on MRTD recognizer in the latest PhotoPay SDK release (v7.3.0)**

Please note that we have significantly improved accuracy for MRZ/MRTD scanning because now we switched to the newest OCR technology based on machine learning.
To be more precise, we measured and compared existing vs. new MRTD scanning. The new OCR system based on machine learning achieves 99.9% accuracy on the character level, which results with a 50% reduction in the error rate in MRZ extraction.

In order to use new *MrtdRecognizer* or *MrtdCombinedRecognizer* or to continue using any additional *Recognizer for scanning any ID with the MRZ (machine readable zone)* within the latest PhotoPay SDK update, you *must* have a new license key. Before updating to the SDK version 7.3.0, please contact your account manager or send an email to support@microblink.com to obtain the *new production license key*.

**Important notes**:

- The MRTD scanning with the older PhotoPay SDK versions (v7.2.0 and below) will continue to work without any problems - until you decide to update.
- If you upgrade to the SDK version 7.3.0 without a new license key scanning of MRTD/MRZ documents will not work.
- Contact us at support@microblink.com to obtain a new license key if you plan to update your app with the latest release.

For any questions, you might have, we stand at your service.

### New features:

- added support for reading front and back side of Brunei Military ID - use `BruneiMilitaryIdFrontRecognizer` and `BruneiMilitaryIdBackRecognizer`
- added support for reading front and back side of Brunei Temporary Residence Permit - use `BruneiTemporaryResidencePermitFrontRecognizer` and `BruneiTemporaryResidencePermitBackRecognizer`
- added new scan activity and overlay: `BlinkCardActivity` and `BlinkCardOverlayController` which are best suited for scanning payment cards with `BlinkCardRecognizer`

### Improvements for existing features:

- improved reading accuracy for all MRZ recognizers
- added option to force overlay orientation for `PhotopayOverlayController` (`PhotopayActivity`), `DocumentOverlayController` (`DocumentScanActivity`) and `BarcodeOverlayController` (`BarcodeScanActivity`) - use `PhotopayUISettings.setForcedOrientation(OverlayOrientation)`, `DocumentUISettings.setForcedOrientation(OverlayOrientation)` and `BarcodeUISettings.setForcedOrientation(OverlayOrientation)`
- enabled reading year-only dates of birth on **Kuwait IDs**
- improved `SingaporeIdBackRecognizer`:
    - better reading of documents with sticker
- improved `MrtdRecognizer`:
    - added `allowSpecialCharacters` option which is required for parsing Malaysian Passport IMM13P MRZ type
- all recognizers now reset their results on shake, except Combined recognizers
- `BlinkCardRecognizer` returns card issuer

### Minor API changes:

- renamed `com.microblink.entities.recognizers.photopay.austria.qr.AustriaQrCodeRecognizer` to `com.microblink.entities.recognizers.photopay.austria.AustriaQrCodePaymentRecognizer` and fields in its `Result`:
    - `IBAN` to `iban`
    - `BIC` to `bic`
    - `referenceNumber` to `reference`
- renamed `com.microblink.entities.recognizers.photopay.germany.qr.GermanyQrCodeRecognizer` to `com.microblink.entities.recognizers.photopay.germany.GermanyQrCodePaymentRecognizer` and updated fields in its `Result`:
    - `authority` is returned as `String`
    - renamed fields:
        - `IBAN` to `iban`
        - `BIC` to `bic`
        - `paymentReference` to `reference`
        - `BLZ` to `bankCode`
    - added fields:
        - `creditorId`
        - `dateOfSignature`
        - `displayData`
        - `formFunction`
        - `formType`
        - `formVersion`
        - `mandateId`
        - `periodicFirstExecutionDate`
        - `periodicLastExecutionDate`
        - `periodicTimeUnit`
        - `periodicTimeUnitRotation`
        - `postingKey`
- renamed `com.microblink.entities.recognizers.photopay.kosovo.code128.KosovoCode128Recognizer` to `com.microblink.entities.recognizers.photopay.kosovo.KosovoCode128PaymentRecognizer` and updated fields in its `Result`:
    - removed `currency` field
    - renamed fields:
        - `payerAccount` to `payerAccountNumber`
        - `referenceNumber` to `reference`
        - `slipID` to `slipId`
- removed `SerbiaIdFrontRecognizer`, `SerbiaIdBackRecognizer` and `SerbiaCombinedRecognizer`
- fields that are **not** deprecated anymore:
    - Sweden DL - reference number
    - Ireland DL - driver number
    - Malaysia iKad - passport number
    - Hong Kong ID - commercial code
- deprecated the following methods in `UsdlRecognizer.Result` and `UsdlCombinedRecognizer.Result`: (they have been replaced with new getters):
    - getField(UsdlKeys)
    - getOptionalElements
- added new getters to following results:
    - `UsdlRecognizer.Result` and `UsdlCombinedRecognizer.Result`:
        - `firstName`
        - `lastName`
        - `fullName`
        - `address`
        - `documentNumber`
        - `sex`
        - `restrictions`
        - `endorsements`
        - `vehicleClass`
        - `dateOfBirth`
        - `dateOfIssue`
        - `dateOfExpiry`
    - `MrzResult`:
       - `sanitizedOpt1`
       - `sanitizedOpt2`
       - `sanitizedNationality`
       - `sanitizedIssuer`
- moved `SwedenDlFrontRecognizer` from package `com.microblink.entities.recognizers.blinkid.sweden.dl` to `com.microblink.entities.recognizers.blinkid.sweden`
- renamed methods in the following recognizers and its results:
    - `CzechiaCombinedRecognizer`:
        - `lastName` to `surname`
        - `firstName` to `givenNames`
        - `identityCardNumber` to `documentNumber`
        - `address` to `permanentStay`
        - `issuingAuthority` to `authority`
        - `personalIdentificationNumber` to `personalNumber`
    - `GermanyCombinedRecognizer`:
        - `lastName` to `surname`
        - `firstName` to `givenNames`
        - `identityCardNumber` to `documentNumber`
        - `issuingAuthority` to `authority`
        - `eyeColor` to `colourOfEyes`
    - `JordanCombinedRecognizer`:
        - `issuer` to `issuedBy`
    - `PolandCombinedRecognizer`:
        - `issuer` to `issuedBy`
    - `RomaniaIdFrontRecognizer`:
       - `lastName` to `surname`
       - `cardNumber` to `documentNumber` from `MrzResult`
       - `parentNames` to `parentName`
       - `nonMRZNationality` to `nationality`
       - `nonMRZSex` to `sex`
       - `validFrom` to `dateOfIssue`
       - `validUntil` to `dateOfExpiry`
       - removed field `idSeries`
       - removed field `cnp`
       - MRZ fields are available through `MrzResult` which can be obtained by using getter `RomaniaIdFrontRecognizer.Result.getMrzResult()`
    - `SlovakiaCombinedRecognizer`:
       - `issuingAuthority` to `issuedBy`
       - `personalIdentificationNumber` to `personalNumber`
    - `SloveniaIdFrontRecognizer`:
       - `lastName` to `surname`
       - `firstName` to `givenNames`
    - `SloveniaIdBackRecognizer`:
       - `authority` to `administrativeUnit`
       - MRZ fields are available through `MrzResult` which can be obtained by using getter `SloveniaIdBackRecognizer.Result.getMrzResult()`
    - `SloveniaCombinedRecognizer`:
       - `lastName` to `surname`
       - `firstName` to `givenNames`
       - `identityCardNumber` to `documentNumber`
       - `citizenship` to `nationality`
       - `issuingAuthority` to `administrativeUnit`
       - `personalIdentificationNumber` to `pin`
- `MrtdRecognizer` and `MrtdCombinedRecognizer` do not return MRZ image any more
- `MrtdComginedRecognizer` does not have glare detection options (it does not detect glare anymore)
- replaced `PaymentCardFrontRecognizer`, `PaymentCardBackRecognizer` and `PaymentCardCombinedRecognizer` with single recognizer - `BlinkCardRecognizer`
- replaced `ElitePaymentCardFrontRecognizer`, `ElitePaymentCardBackRecognizer` and `ElitePaymentCardCombinedRecognizer` with single recognizer - `BlinkCardEliteRecognizer`
- `PolandIdBackRecognizer.Result` does not extend `MRTDResult` any more, it has getter `getMrzResult` for obtaining MRZ results

### Bug fixes:

- fixed bug in `SlovakiaQrCodeRecognizer`
- fixed bug in `IbanParser` for **Romanian IBANs**
- removed incorrect autofocus check that was performed before concrete camera type is chosen
- `MrtdRecognizer`:  result state is now properly invalidated after detection fails
- various other bug fixes and improvements

## 7.2.0

### New features:

- added support for reading front side of Italy Driver's License - use `ItalyDlFrontRecognizer`
- added standalone recognizer for reading front side of Austria Driver's License - use `AustriaDlFrontRecognizer`
- added support for reading front and back side of elite Payment / Debit cards - use `ElitePaymentCardFrontRecognizer`, `ElitePaymentCardBackRecognizer` and `ElitePaymentCardCombinedRecognizer`
- added standalone recognizer for reading front side of German Driver's License - use `GermanyDlFrontRecognizer`
- added support for reading back side of Germany Driver's License (reading of single B10 field - date of issue for B category)  - use `GermanyDlBackRecognizer`
- added support for reading front side of Mexico Voter ID  - use `MexicoVoterIdFrontRecognizer`
- added support for reading front and back side of Brunei ID - use `BruneiIdFrontRecognizer` and `BruneiIdBackRecognizer`
- added support for reading front and back side of Cyprus ID, issued after 2015.  - use `CyprusIdFrontRecognizer` and `CyprusIdBackRecognizer`
- added support for reading front side of Malaysian MyKAS - use `MalaysiaMyKasFrontRecognizer`
- added support for reading front side of Malaysian MyPR - use `MalaysiaMyPrFrontRecognizer`
- added support for reading front and back side of Brunei Residence Permit - use `BruneiResidencePermitFrontRecognizer` and `BruneiResidencePermitBackRecognizer`
- enabled capturing high resolution camera frames:
    - when custom UI integration is performed, use `RecognizerRunnerView.setHighResFrameCaptureEnabled` and `RecognizerRunnerView.captureHighResImage`
    - when using provided scan activities, high resolution full camera frames taken at the moment of successful scan are returned if this option is enabled through `UISettings`. Concrete `UISettings` which implement interface `HighResSuccessFrameCaptureUIOptions` support this feature.
- updated default icons in scan activities/overlays

### Improvements for existing features:
- improved `CzechiaQrCodeRecognizer`:
    - support for default currency
- improved `SwitzerlandSlipRecognizer`
- improved `SerbiaQrCodePaymentRecognizer` and `SerbiaPdf417PaymentRecognizer`:
    - support for new Serbian QR code standard
    - added result members: `identificationCode`, `payerAccountNumber`, `paymentCode`, `merchantCodeCategory`, `oneTimePaymentCode`, `merchantReference` and `rawBarcodeData`
    - renamed result member: accountNumber to recipientAccountNumber
- improved QR code scanning for images
- improved `MalaysiaDlFrontRecognizer`:
    - added support for reading Malaysia Dl for foreigners
- improved `UsdlRecognizer`:
    - added support for reading dates on Nigerian Driver's licenses
- added support for setting full document image extension factors for almost all ID document recognizers, they implement interface `FullDocumentImageExtensionOptions`
- added support for setting the number of stable detections threshold on `DocumentFaceRecognizer` and recognizers which use it internally: `MrtdCombinedRecognizer` and `UsdlCombinedRecognizer` - use `setNumStableDetectionsThreshold(int)`. This can help to avoid returning of blurry images.
- improved `EudlRecognizer`:
    - better reading accuracy for UK Driver's license
- improved reading accuracy for the following recognizers (**DeepOCR** support):
    - `CroatiaIdFrontRecognizer`
    - `CroatiaIdBackRecognizer`
    - `HongKongIdFrontRecognizer`
    - `IndonesiaIdFrontRecognizer`
    - `MalaysiaMyKadFrontRecognizer`
    - `MalaysiaMyKadBackRecognizer`
    - `MalaysiaMyTenteraFrontRecognizer`
    - `MalaysiaDlFrontRecognizer`
    - `NewZealandDlFrontRecognizer`
    - `SingaporeIdFrontRecognizer`
    - `SingaporeIdBackRecognizer`
    - `SingaporeDlFrontRecognizer`
- improved DeepOCR accuracy
- added support for hiding sensitive parts of images returned by payment card recognizers:
    - `PaymentCardFrontRecognizer`: setAnonymizeOwner, setAnonymizeCardNumber
    - `PaymentCardBackRecognizer`: setAnonymizeCvv
    - `PaymentCardCombinedRecognizer`:  setAnonymizeOwner, setAnonymizeCardNumber, setAnonymizeCvv
    - `ElitePaymentCardFrontRecognizer`: setAnonymizeOwner
    - `ElitePaymentCardBackRecognizer`: setAnonymizeCardNumber, setAnonymizeCvv
    - `ElitePaymentCardBackRecognizer`: setAnonymizeOwner,  setAnonymizeCardNumber, setAnonymizeCvv
- `SlovakiaIdFrontRecognizer`: improved reading of `personalNumber` field
- improved image return processor:
    - the processor now estimates detected (dewarped) document image quality and returns the best quality dewarped image from the best quality detection
- `DocumentVerificationActivity` does not extend `AppCompatActivity` any more
- improved `PaymentCard` recognizers:
    - better OCR and data extraction
    - added support for reading payment card numbers in 4x6x4 and 4x6x5 format
- improveed UAE recognizers:
    - glare detection is enabled for all images returned from `UnitedArabEmiratesDlFrontRecognizer`, `UnitedArabEmiratesIdBackRecognizer` and `UnitedArabEmiratesIdFrontRecognizer` recognizers
- improved `MrtdRecognizer`:
    - added option to set extension factors for full document image: use method `setFullDocumentImageExtensionFactors`
    - added option to encode `fullDocumentImage` and `mrzImage` to JPEG and save them to `MrtdRecognizer.Result`: use `setEncodeMrzImage` and `setEncodeFullDocumentImage` to enable encoding
- `RecognizerRunnerView` is lifecycle-aware now, it implements `android.arch.lifecycle.LifecycleObserver` interface

### Minor API changes:

- renamed methods in `SingaporeIdFrontRecognizer` and its `Result`:
    - `bloodType` to `bloodGroup`
- renamed methods in `GermanyIdFrontRecognizer` and its `Result`:
    - `lastName` to `surname`
    - `firstName` to `givenNames`
- renamed `MyTenteraRecognizer` to `MalaysiaMyTenteraFrontRecognizer` and  methods in recognizer and its `Result`:
    - `ownerFullName` -> `fullName`
    - `ownerAddress` -> `fullAddress`
    - `ownerAddressStreet` -> `street`
    - `ownerAddressZipCode` -> `zipcode`
    - `ownerAddressCity` -> `city`
    - `ownerAddressState` -> `ownerState`
    - `ownerBirthDate` -> `birthDate`
    - `ownerSex` -> `sex`
    - `ownerReligion` -> `religion`
    - `nricNumber` -> `nric`
- renamded enum `com.microblink.uisettings.options.ShowOcrResultMode` to `com.microblink.uisettings.options.OcrResultDisplayMode`
- for all `UISettings` classes which support setting of OCR result display mode, renamed method `setShowOcrResultMode` to `setOcrResultDisplayMode`
- renamed `IkadRecognizer` to `MalaysiaIkadFrontRecognizer` and  methods in recognizer and its `Result`:
    - `expiryDate` to `dateOfExpiry `
    - `sex ` to `gender`
- renamed `MyKadFrontRecogniezer ` to `MalaysiaMyKadFrontRecognizer ` and  methods in recognizer and its `Result`:
    - `ownerFullName ` to `fullName `
    - `ownerAddress ` to `fullAddress `
    - `addressStreet ` to `street `
    - `ownerAddressZipCode ` to `zipcode `
    - `ownerAddressCity ` to `city `
    - `ownerAddressState ` to `ownerState `
    - `ownerBirthDate ` to `birthDate `
    - `ownerSex ` to `sex `
    - `ownerReligion ` to `religion `
    - `nricNumber ` to `nric `
- `MalaysiaMyKadFrontRecognizer` does not extract `armyNumber` anymore, use `MalaysiaMyTenteraFrontRecognizer` for scanning `MyTentera`
- removed `sex` and `signatureImage` from `MalaysiaMyKadBackRecognizer `
- `MrtdRecognizer`:
    - method `setSaveImageDPI` which has been used to set DPI for full document and MRZ image is replaced with methods `setFullDocumentImageDpi` and `setMrzImageDpi`
- renamed methods in `SwitzerlandIdBackRecognizer` and its `Result`:
    - `nonMrzDateOfExpiry` to `dateOfExpiry`
    - `nonMrzSex` to `sex`
- renamed methods in `SwitzerlandPassportRecognizer` and its `Result`:
    - `placeOfBirth` to `placeOfOrigin`
    - `nonMrzDateOfBirth` to `dateOfBirth`
    - `nonMrzDateOfExpiry` to `dateOfExpiry`
    - `nonMrzSex` to `sex`
- renamed `GermanyOldIdRecognizer` to `GermanyIdOldRecognizer`
- renamed methods in `CroatiaCombinedRecognizer` and its result:
    - `identityCardNumber` to `documentNumber`
    - `address` to `residence`
    - `issuingAuthority` to `issuedBy`
    - `personalIdentificationNumber` to `oib`
    - `nonResident` to `documentForNonResident`
- removed `mrzImage` from `MrtdCombinedRecognizer` and its result
- renamed methods in `AustraliaDlFrontRecognizer.Result`:
    - `name` to `fullName`
    - `dateOfExpiry` to `licenceExpiry`
- renamed `eyeColour` to `colourOfEyes` in `GermanyIdBackRecognizer.Result`
- deprecated the following recognizers:
    - `SerbiaIdBackRecognizer`
    - `SerbiaIdFrontRecognizer`
    - `SerbiaCombinedRecognizer`
- deprecated the following result fields:
    - `HongKongIdFrontRecognizer.Result`:
        - `commercialCode`
    - `IndonesiaIdFrontRecognizer.Result`:
        - `bloodType`
        - `district`
        - `kelDesa`
        - `rt`
        - `rw`
    - `NewZealandDlFrontRecognizer.Result`:
        - `donorIndicator`
        - `cardVersion`
    - `MalaysiaMyKadBackRecognizer.Result`:
        - `extendedNric`
    - `MexicoVoterIdFrontRecognizer.Result`:
        - `electorKey`
    - `IrelandDlFrontRecognizer.Result`:
        - `driverNumber`
    - `SwedenDlFrontRecognizer.Result`:
        - `referenceNumber`
    - `MalaysiaIkadFrontRecognizer.Result`:
        - `passportNumber`
    - `AustriaIdBackRecognizer.Result`:
        - `principalResidence`
        - `height`
        - `eyeColour`
    - `AustriaPassportRecognizer.Result`:
        - `height`
     - `GermanyIdBackRecognizer.Result`:
        - `colourOfEyes`
        - `height`
    - `SwitzerlandIdBackRecognizer.Result`:
        - `height`
    - `SwitzerlandPassportRecognizer.Result`:
        - `height`
     - `SingaporeIdBackRecognizer.Result`:
        - `bloodGroup`
    - `ColombiaIdBackRecognizer.Result`:
        - `bloodGroup`
    - `SwitzerlandPassportRecognizer.Result`:
        - `height`
    - `PolandIdFrontRecognizer.Result`:
        - `familyName`
        - `parentsGivenNames`
    - `MoroccoIdBackRecognizer.Result`:
        - `fathersName`
        - `mothersName`
    - `RomaniaIdFrontRecognizer.Result`:
        - `parentNames`

### Bug fixes:

- `DocumentFaceRecognizer` now correctly applies DPI settings to returned face and full document images
- various other bug fixes and improvements

## 7.1.1

### Bug fixes:

- fixed camera autofocus problems on Samsung S9/S9+ when optimisation for near scanning is enabled
- fixed autofocus problems in `Field by field` scanning on Huawei P20 pro, Huawei P20 and Huawei P20 lite

## 7.1.0

### New features:

- added support for reading partial dates on all MRTD documents:
    - affects all recognizers which extract data from Machine Readable Zone
- added support for reading front side of Singapore Changi employee ID - use `SingaporeChangiEmployeeIdRecognizer`
- added support for reading back side of Morocco ID - use  `MoroccoIdBackRecognizer`
- added support for reading front side of United Arab Emirates Driver's License - use `UnitedArabEmiratesDlFrontRecognizer`
- added support for reading front side of Spain Driver's License - use `SpainDlFrontRecognizer`
- added support for reading front and back side of Cyprus ID - use `CyprusIdFrontRecognizer` and `CyprusIdBackRecognizer`
- added support for reading front and back side of Kuwait ID - use `KuwaitIdFrontRecognizer` and `KuwaitIdBackRecognizer`
- added support for reading front and back side of Payment / Debit cards - use `PaymentCardFrontRecognizer`, `PaymentCardBackRecognizer` and `PaymentCardCombinedRecognizer`
- added support for reading front side of Ireland Driver's License  - use `IrelandDlFrontRecognizer`
- added support for reading front side of Colombia Driver's License - use `ColombiaDlFrontRecognizer`

### Improvements for existing features:
- improved reading of Croatia Pdf417 payment barcode - `CroatiaPdf417Recognizer`
- improved `SwitzerlandSlipRecognizer`:
    - added getter for `currencyCode` to its Result
- improved `HongKongIdFrontRecognizer`:
    - added support for reading residential status field
- improved `UnitedArabEmiratesIdFrontRecognizer`:
    - better name and nationality extraction
- improved `SingaporeIdBackRecognizer`:
    - added support for reading sticker with new address
- improved `NewZealandDlFrontRecognizer`:
    - better reading of all fields
- improved `SingaporeCombinedRecognizer`:
    - added support for reading sticker with new address from the back side
- `BarcodeScanActivity` by default does not show result dialog after scan
- improved `MrtdCombinedRecognizer`:
    - added option to allow unparsed and unverified MRZ results - use `MrtdCombinedRecognizer.setAllowUnparsedResults` and `MrtdCombinedRecognizer.setAllowUnverifiedResults`
- improved reading of Malaysian Driver's License

### Minor API changes:
- in `CroatiaIdFrontRecognizer` `identityCardNumber` is renamed to `documentNumber`
- `HongKongIdFrontRecognizer.Result.getDateOfBirth()` and `HongKongIdFrontRecognizer.Result.getDateOfIssue()` return `com.microblink.results.date.DateResult` instead of `com.microblink.results.date.Date`
- in `SingaporeIdBackRecognizer.Result` getter `getBloodGroup()` is renamed to `getBloodType()`
- renamed getters in `NewZealandDlFrontRecognizer.Result` renamed getters:
    - `getIssueDate()` to `getDateOfIssue()`
    - `getExpiryDate()` to `getDateOfExpiry()`
    - `getDonorIndicator()` to `isDonorIndicator()`
- renamed methods in `CroatiaIdBackRecognizer` and its `Result`:
    - `address` to `residence`
    - `documentForNonResident` to `isDocumentForNonResident`
    - `issuingAuthority` to `issuedBy`
    - `getDateOfExpiryPermanent` to `isDateOfExpiryPermanent`
    - MRZ fields are available through `MrzResult` which can be obtained by using getter `CroatiaIdBackRecognizer.Result.getMrzResult()`
- renamed method in `SingaporeIdFrontRecognizer` and its `Result`:
    - `cardNumber` to `identityCardNumber`
- renamed method in `SingaporeCombinedRecognizer` and its `Result`:
    - `cardNumber` to `identityCardNumber`
    - `bloodGroup` to `bloodType`
- renamed methos in `MalaysiaDlFrontRecognizer` and its `Result`:
    - `state` to `ownerState`
    - `zipCode` to `zipcode`
- renamed methos in `IndonesiaIdFrontRecognizer` and its `Result`:
    - `validUntil` to `dateOfExpiry`
    - `validUntilPermanent` to `dateOfExpiryPermanent`

### Bug fixes:
- fixed bug which caused that results from the previous scan are cleared when the scan activity is run again and entities which have produced results are not used in the new scan
- various other bug fixes and improvements

## 7.0.0

### New API and license format

- new API, which is not backward compatible. Please check [README](README.md) and updated demo applications for more information, but the gist of it is:
    - `RecognizerView` has been renamed to `RecognizerRunnerView` and `Recognizer` singleton to `RecognizerRunner`
    - `SegmentScanActivity` has been renamed to `FieldByFieldScanActivity`
    - `RandomScanActivity` does not exist anymore
    - previously internal `Recognizer` objects are not internal anymore - instead of having opaque `RecognizerSettings` and `RecognizerResult` objects, you now have stateful `Recognizer` object that contains its `Result` within and mutates it while performing recognition.
        - similarly we now have stateful `Parser` and `Detector` objects
        - introduced new `Processor` object type
        - For more information, see [README](README.md) and updated demo applications
    - added `RecognizerRunnerFragment` with support for various scanning overlays in a manner similar to iOS API. This now allows you to use built-in UI, which was previously strictly available for built-in activities, in form of fragment anywhere within your activity. Full details are given in [README](README.md) and in updated demo applications.
- new licence format, which is not backward compatible. Full details are given in [README](README.md) and in updated applications, but the gist of it is:
    - licence can now be provided with either file, byte array or base64-encoded bytes

### New features:

- Added support for reading front side of Morocco identity documents
- Added support for reading front side of Singapore driver's licences
- Added support for reading front side of Swiss driver's licenses


### Improvements for existing features:

- Slovak payBySquare QR code recognizer now supports returning multiple payment informations with multiple account numbers. This feature was not possible to achieve with the 6.x API.
- Czech payment QR code recognizer now supports returning multiple account numbers, if available within the QR code. This feature was not possible to achieve with the 6.x API.
- improved scanning of United Arab Emirates ID documents
- improved scanning of back side of Colombia ID

### Bug fixes:

- improved recognition of czech and german payment QR codes in case they contained non-ascii characters

## 6.13.0

### New features:

- added support for reading front side of Sweden Driver's License - use `SwedenDLFrontRecognizerSettings`

### Improvements for existing features:

- improved `CroatianIDBackSideRecognizer`:
    - better reading of address field
- improved `GermanIDFrontSideRecognizer`:
    - added support for reading CAN number
- improved `USDLRecognizer`:
    - better parsing of the USDL data fields
- improved `HongKongIDFrontRecognizer`:
    - better reading of document number (check digit validation)
- improved `IKadRecognizer`:
    - added support for iKad IMM_55 document type (foreign students card)
- added option to extend full document image for German recognizers:
`GermanIDFrontSideRecognizer`, `GermanOldIDRecognizer`, `GermanIDBackSideRecognizer`, `GermanIDCombinedRecognizer` and `GermanPassportRecognizer` - use method `setFullDocumentImageExtensionFactors(ImageExtensionFactors)` from the corresponding recognizer settings
- when `DocumentFaceRecognizer` is activated at the same time with another more specific recognizer(s) (e.g. EUDLRecognizer), preference is given to the more specific recognizer which means that it will get a chance to extract additional data from the concrete document type

### Bug fixes:

- fixed reading of non-standard PDF417 barcodes
- fixed reading of IBAN and BIC numbers in payment CzechQRCodeRecognizer

## 6.12.0

### New features:

- added support for reading front side of Egypt ID - use `EgyptIDFrontRecognizerSettings`
- added support for reading front and back side of Jordan ID - use `JordanIDFrontRecognizerSettings`,  `JordanIDBackRecognizerSettings` and `JordanIDCombinedRecognizerSettings`
- added support for reading front side of Hong Kong ID - use `HongKongIDFrontRecognizerSettings`
- added support for reading front side of Malaysian drivers license - use `MalaysianDLFrontRecognizerSettings`
- added support for reading front and back side of Colombian ID - use `ColombiaIDFrontRecognizerSettings` and `ColombiaIDBackRecognizerSettings`
- added support for reading front and back side of United Arab Emirates ID - use `UnitedArabEmiratesIDFrontRecognizerSettings` and `UnitedArabEmiratesIDBackRecognizerSettings`
- added support for reading front side of New Zealand drivers license - use `NewZealandDLFrontRecognizerSettings`
- added support for reading back side of Malaysian MyKad - use `MyKadBackSideRecognizerSettings`
- added support for reading Malaysian MyTentera documents - use `MyTenteraRecognizerSettings`
- added support for reading Malaysian MyTentera documents with MyKad recognizer - use `MyKadFrontSideRecognizerSettings` and enable reading of army number
- added support for setting DPI for full document images returned by `MyKadFrontSideRecognizer`, `MyKadBackSideRecognizer`, `MyTenteraRecognizer` and `IKadRecognizer`:
    - use `setFullDocumentImageDPI` on the corresponding recognizer settings

### Minor API changes:

- renamed `MyKadRecognizerSettings` and `MyKadRecognitionResult` to `MyKadFrontSideRecognizerSettings` and `MyKadFrontSideRecognitionResult` and moved them to `com.microblink.recognizers.blinkid.malaysia.mykad.front` package
- moved `IKadRecognizerSettings` and `IKadRecognitionResult` to `com.microblink.recognizers.blinkid.malaysia.ikad` package

### Improvements for existing features:

- improved `USDLRecognizer`:
    - better parsing of the USDL barcode content
    - fixed extraction of expiry date from magnetic stripe USDL subtype
- improved `VinParser`:
    - better extraction of specific VIN numbers
- improved `SlovakSlipRecognizer`:
    - better reading of payer name and address
- improved `SlovenianSlipRecognizer`:
    - better reading of amount, reference and BIC from Slovenian payment slip
- improved `MRTDRecognizer`:
    - added support for parsing Malaysian Passport IMM13P MRZ type, reading of special characters must be enabled by using `MRTDRecognizerSettings.setAllowSpecialCharacters`
    - enabled reading of MRZ with '-' characters (non-default setting), to enable this use method `MRTDRecognizerSettings.setAllowSpecialCharacters`
    - improved reading of Belgium ID MRZ OPT2 field
    - added support for reading Belgium MRZ with partial date of birth - `MRTDRecognizerSettings.setAllowUnverifiedResults()` must be set to `true`
    - added support for reading Kenya MRZ - `MRTDRecognizerSettings.setAllowUnverifiedResults()` must be set to `true`
- improved `MyKadFrontSideRecognizer` and `MyTenteraRecognizer`:
    - better reading of name field
    - better reading of address field
- when setting DPI for full document image in concrete recognizer settings that has method `setFullDocumentImageDPI`, exception is thrown if DPI value is not in the expected range `[100, 400]`
- improved `AustralianDLFrontSideRecognizer`:
    - improved reading of names and addresses
    - added support for reading first names with more words
- improved `SingaporeIDFrontRecognizer`:
    - tuned ID card data extraction positions
- improved Malaysian `IKadRecognizer`:
    - better reading of date of expiry and employer fields

### Bug fixes:

- fixed a crash in Templating API caused by using a `MultiDetector` with `DetectorRecognizer`
- fixed rare crash in `MRTDRecognizer`

## 6.11.1

### Bug fixes:

- fixed reading of Czech QR codes which contain encoded URLs
- fixed reading of Croatian HUB3 payment PDF417 barcodes:
    - added support for reading barcodes whose encoding is not explicitly defined and encoding is cp1250

## 6.11.0

### New features:

- added support for reading back side of new Australian Driver's licence for state Victoria - use `AustralianDLBackSideRecognizerSettings`
- added support for reading front side of Indonesian ID - use `IndonesianIDFrontRecognizerSettings`
- added support for Malaysian visa with document code TS - use `MRTDRecognizerSettings`
- added support for setting DPI for full document images returned by `MRTDRecognizer`, `AustralianDLFrontSideRecognizer`, `AustralianDLBackSideRecognizer` and `EUDLRecognizer`:
    - use `setFullDocumentImageDPI` on the corresponding recognizer settings

### Improvements for existing features:

- `CroatianSlipRecognizer`: added support for Croatian references with models 64, 26, 35, 40, 69
- improved parsing of payment QR codes
- improved reading of Malaysian MyKad address

## 6.10.0

### New features:
- added support for scanning front side of Swiss ID - use `SwissIDFrontSideRecognizerSettings`
- added support for scanning front and back side of Polish ID - use `PolishIDFrontSideRecognizerSettings`, `PolishIDBackSideRecognizerSettings` and `PolishIDCombinedRecognizerSettings`
- added support for reading front side of new Australian Driver's licence for state Victoria - use `AustralianDLFrontSideRecognizerSettings`
- added support for Swiss payment QR - use `SwissQRCodeRecognizerSettings`
- introduced `MRZFilter`:
    - use  `MRTDRecognizerSettings` to enable it on  `MRTDRecognizer`
    - determines whether document should be processed or it is filtered out, based on its MRZ (Machine Readable Zone)
 - added `QuadWithSizeDetectorResult` which inherits existing `QuadDetectorResult`:
    - it's subclasses are `DocumentDetectorResult` and `MRTDDetectorResult`
    - returns information about physical size (height) in inches of the detected location when physical size is known
- introduced `GlareDetector` which is by default used in all recognizers whose settings implement `GlareDetectorOptions`:
    - when glare is detected, OCR will not be performed on the affected document position to prevent errors in the extracted data
    - if the glare detector is used and obtaining of glare metadata is enabled in `MetadataSettings`, glare status will be reported to `MetadataListener`
    - glare detector can be disabled by using `setDetectGlare(boolean)` method on the recognizer settings
- introduced `MRTDSpecification` and method `setMRTDSpecifications` on `MRTDRecognizerSettings` and `MRTDDetectorSettings`:
    - detection is limited only to document type specified with `MRTDSpecification`
    - when `MRTDSpecifications` are set, results will be returned only for specified MRTD documents
    - `MRTDSpecification` can be created by using `MRTDSpecification.createFromPreset`, available presets are: `MRTD_SPECIFICATION_TD1`, `MRTD_SPECIFICATION_TD2` and `MRTD_SPECIFICATION_TD3`
- new document specification presets in `DocumentSpecificationPreset` enum:  `DOCUMENT_SPECIFICATION_PRESET_ID1_VERTICAL_CARD` and  `DOCUMENT_SPECIFICATION_PRESET_ID2_VERTICAL_CARD` - use `DocumentSpecification.createFromPreset` method to create document specification for detector
- `EUDLRecognizer` can return face image from the driver's license
- warning for time limited license keys when using provided activities, custom UI integration or Direct API:
    - the goal is to prevent unintentional publishing of application to production with the demo license key that will expire
    - warning toast can be disabled by using `EXTRAS_SHOW_TIME_LIMITED_LICENSE_KEY_WARNING` intent extra, `RecognizerView.setLicenseKeyTimeLimitedWarningEnabled` method when custom UI integration is used and `Recognizer.setLicenseKeyTimeLimitedWarningEnabled` method when Direct API is used

### Minor API changes:
- `DocumentSpecification` does not have method `setPhysicalSizeInInches` any more
- `DocumentDetectorResult` does not contain information about screen orientation any more

### Improvements for existing features:
- improved face detection in `DocumentFaceRecognizer`: stable detection is required to prevent returning of blurred images
- improved reading of Malaysian `MyKad` documents:
    - improved reading and parsing of address fields: previously recognizer was unable to read some documents because of the expected address format
- improved reading of Malaysian visas and work permits
- improved reading of issuing authority on Croatian ID back side
- improved parsing of variable symbol on Czech payment slips
- allowed scanning of Czech QR codes which have colon (:) in payment description field
- Italian IBAN parsing now validates the BBAN check digit
- `DutchSlipRecognizerSettings` - added option to enable or disable reading of recipient name. By default, this is turned off and recipient name will not be returned.

### Bug fixes:
- fixed returning valid data for MRZ based recognizers when not all fields outside of the MRZ have been scanned
- fixed crash in `GermanIDBackSideRecognitionResult` caused by ProGuard obfuscation, when it is not declared in ProGuard rules to keep the result constructor

## 6.9.2

### Bug fixes

- fixed amount parsing from German BezahlCode
    - 0.8 is now parsed as 80 cents, not as 8 cents anymore

## 6.9.1

### Minor API changes

- `USDLRecognizerSettings`:
    - removed option to scan 1D Code39 and Code128 barcodes on driver's licenses that contain those barcodes alongside PDF417 barcode

### Improvements for existing features

- improved `CroatianIDBackSideRecognizer`:
    - better extraction of fields on back side of the Croatian ID card
- improved `USDLRecognizer`:
    - added support for new USDL standard
- `KosovoCode128Recognizer` returns reference number for new barcode type

### Bug fixes

- fixed bug in QR code reading

## 6.9.0

### New features:

- added support for scanning Austrian passports - use `AustrianPassportRecognizerSettings` ​
- added support for scanning Swiss passports - use `SwissPassportRecognizerSettings `​
- added support for scanning Mexican Voting cards - use `MRTDRecognizerSettings` ​
- added support for scanning Serbian payment QR codes - use `SerbianPdf417RecognizerSettings` ​
- added support for scanning back side of Swiss ID - use `SwissIDBackSideRecognizerSettings` ​

### Minor API changes:

- `RegexParserSettings` and `RawParserSettings` now work with `AbstractOCREngineOptions`, which is a base class of `BlinkOCREngineOptions`
    - default engine options returned by method `getOcrEngineOptions` for both parser settings return instance of `BlinkOCREngineOptions`
- `BlinkOCRRecognizerSettings` is now deprecated and will be removed in `v7.0.0`
    - use `DetectorRecognizerSettings` to perform scanning of templated documents
    - use `BlinkInputRecognizerSettings` for segment scan or for full-screen OCR
    - until `v7.0.0`, `BlinkOCRRecognizerSettings` will behave as before, however you are encouraged to update your code not to use it anymore
- `DocumentClassifier` interface is moved from `com.microblink.recognizers.blinkocr` to `com.microblink.recognizers.detector` package and `DocumentClassifier.classifyDocument()` now accepts `DetectorRecognitionResult` as parameter for document classification
- `Slovak ID Recognizers`:
    - result getters `getPersonalIdentificationNumber()` and `getIssuingAuthority()` are renamed to `getPersonalNumber()` and `getIssuedBy()`
- Renamed `RomanianIDFrontSideRecognitionResult` element getters for Sex and Nationality outside of the MRZ to `getNonMRZNationality()` and `getNonMRZSex()`

### Improvements for existing features

- improved address parsing on Malaysian iKad documents
    - affects only iKad recognizer (represented by `IKadRecognizerSettings`)
- added support for scanning non-expiring Croatian ID documents
    - affects:
        - Croatian ID front recognizer (represented by `CroatianIDFrontSideRecognizerSettings`) - date of expiry is keyword **TRAJNO**
        - Croatian ID back recognizer (represented by `CroatianIDBackSideRecognizerSettings`) - date of expiry inside MRZ is `991231`
        - Croatian ID combined recognizer (represented by `CroatianIDCombinedRecognizerSettings`)
- improved date parsing:
    - affects date parser and all recognizers which perform date parsing
- added support for reading mirrored QR codes:
    - affects all recognizers that perform QR code scanning
- improved `IKadRecognizer`:
    - added support for long addresses and employer names
- improved `Singapore ID Recognizers`:
    - tuned reading positions
    - more accurate reading of name and blood type fields
- improved `Slovak ID Recognizers`:
    - tuned reading positions of ID elements
    - improved reading precision of address, place of birth, last name and issuing authority
    - added options to disable/enable extraction of certain fields in recognizer settings
- for `Austrian ID Recognizers` added options to disable/enable extraction of certain fields in recognizer settings

### Bug fixes:

- fixed occassional crash in MRTD detection algorithm
    - this affects both _MRTD Recognizer_ (represented by `MRTDRecognizerSettings`) and _MRTD Detector_ (represented by `MRTDDetectorSettings`)
- fixed `SlovakQRCodeRecognizer` (Slovak pay by square):
    - parsing of amounts with less than 2 decimals

## 6.8.1

- added support for slovenian references without prefix
- Czech payment QR code - improved parsing of amounts with less than 2 decimals
- `IKadRecognizer`: fixed returning of full document images as metadata

## 6.8.0

- removed `isItalic` and `isBold` getters from `OcrChar` class
    - they always returned `false`, since OCR engine cannot accurately detect that
- removed `setLineGroupingEnabled` and `isLineGroupingEnabled` from `BlinkOCREngineOptions` because disabling line grouping completely destroyed the OCR accuracy
- fixed crash in TemplatingAPI where classifier interfaces were accidentally removed by ProGuard
- improved `CombinedRecognizers`:
    - better handling of names containing dashes and extra long names
- improved `TopUpParser`:
    - added option to return USSD code without prefix
- by default `MRTDRecognizer` does not return results with incorrect check digits
- bugfix in Croatian ID scanning:
    - ensured that OIB number is not returned for old ID cards, where it does not exist
- added Czech and Slovak translations
- Date fields in recognition results are returned as `com.microblink.results.date.Date` class which represents immutable dates that are consisted of day, month and year
- added `SerbianPdf417Recognizer` which supports scanning Pdf417 barcodes on Serbian payslips
- improved `IbanParser`:
    - improved extraction of IBANs without prefix and introduced `setAlwaysReturnPrefix` option to always return prefix (country code)
    - added support for french IBANs
- added support for new Kosovo code128 payment slips to `KosovoCode128Recognizer`
- enabled reading of Pdf417 barcodes having width/height bar aspect ratio less than 2:1
- Added `VinRecognizer` for scanning VIN (*Vehicle Identification Number*) barcodes
- Added unified `BarcodeRecognizer` for scanning various tipes of barcodes
    - `ZXingRecognizer`, `BarDecoderRecognizer` and `AztecRecognizer` are **deprecated**, `BarcodeRecognizer` should be used for all barcode types that are supported by these recognizers
- `OcrLine.getChars()` method returns `CharWithVariants` array. OCR char is defined by all its parameters (value, font, position, quality, etc.) and for each resulting char it is possible to have multiple variants. For example it is possible to have same char value with different font.
- Fixed bug in SegmentScanActivity:
    - scan results are no longer hidden on shake event

## 6.7.0
- added USDL, MyKad, iKad, EUDL and Singapore ID recognizers
- although new recognizers have been added, we managed to reduce final binary size a bit
- introduced ability to create minimum-size AAR
    - a separate static library distribution now exists which contains a script that you can configure with features you need and it creates a AAR file which only contains features you need - this includes minimum-size native binary and only required assets. The rest (resources and java classes) can be thrown-away by ProGuard.
- `LibPhotoPay` is now fully ProGuard-compatible, i.e. you no longer need to exclude `com.microblink.**` classes in your ProGuard configuration
- removed support for Android 2.3 and Android 4.0 - minimum required Android version is now Android 4.1 (API level 16)
    - devices with Android 4.0 and earlier take [less than 2% of market share](https://developer.android.com/about/dashboards/index.html#Platform) and is very costly to support them
- prefixed custom attributes to avoid name collisions with attributes from other libraries:
    - `CameraViewGroup`: renamed animateRotation to `mb_animateRotation`, animationDuration to `mb_animationDuration`, rotatable to `mb_rotatable`
    - `BaseCameraView`:  renamed initialOrientation to `mb_initialOrientation`, aspectMode to `mb_aspectMode`
- improved `MRTDRecognizer`:
    - WSA (World Goverment of World Citizens) added as valid country code when parsing MRZ
- added `USDLCombinedRecognizer`: scans face image and USDL barcode
- updated German ID recognizers:
    - instead of `GermanIDMRZSideRecognizer`, which was used for scanning front side of the older ID cards and back side of the new ID cards, there are two specialised recognizers:  `GermanIDBackSideRecognizer`  and `GermanOldIDRecognizer`
    - improved scanning accuracy of the `GermanIDFrontSideRecognizer` (name and surname)
    - splitting address from back side of the new German ID (GermanIDBackSideRecognizer) to ZIP code, city, street and house number

- improved Croatian ID recognizers:
    - multiple scans are used for better confidence
- `TopUpParser` improvements
- `DateParser` can parse dates with month names in English (either full or abbreviated), if this option is enabled
- added support for polish IBAN without PL prefix to `IBANParser`
- fixed returning of images inside TemplatingAPI for frames when document was not correctly detected
- introduced combined recognizers:
    - `AustrianIDCombinedRecognizer`: scans front and back side of the Austrian ID
    - `CroatianIDCombinedRecognizer`: scans front and back side of the Croatian ID
    - `CzechIDCombinedRecognizer`: scans front and back side of the Czech ID
    - `MRTDCombinedRecognizer`: scans face image from any type of the document and Machine Readable Zone
    - `SerbianIDCombinedRecognizer`: scans front and back side of the Serbian ID
    - `SingaporeIDCombinedRecognizer`: scans front and back side of the Singapore ID
    - `SlovakIDCombinedRecognizer`: scans front and back side of the Slovak ID
    - `SlovenianIDCombinedRecognizer`: scans front and back side of the Slovenian ID
    Combined recognizers can be used for scanning multiple parts/sides of the document in predefined order. They combine results from individual scans to boost accuracy and merge them into the final result.
- added `VerificationFlowActivity` which is designed for scanning multiple parts/sides of the document by using provided combined recognizers

## 6.6.2

- fixed crash when multiple QR code-based recognizers were used together
- improved `TopUpParser`:
    - added option to enable all prefixes at the same time (generic prefix)
- fixed crash in SlovenianQRCodeRecognizer
- improved `MRTDRecognizer`:
    - better support for arab MRZ
- updated `CroatianIDFrontSideRecognizer`:
    - returning sex as written on front side of a document
- added support for scanning front side of Romanian ID cards

## 6.6.1

- updated `SlovenianQRCodeRecognitionResult`:
    - addresses are divided to street and place, now payer address and recipient address can be obtained as four fields:  payer place, payer street, recipient place and recipient street
    - fixed obtaining of raw results - method getRawResult()
- improved CroReferenceParser
    - references in form HRXX-XXXX... are not returned if model is not valid, for example HR22-2360 is not a valid reference because model 22 does not exist
    - trailing whitespace is removed from result

## 6.6.0

- added PhotoPay support for Slovak code 128 barcode
- added PhotoPay support for Slovenian UPN QR payslips
- added support for extraction of beneficiary name and address on Slovak payslips
- improved PhotoPay support for Slovak Data Matrix codes
- fixed layouting of provided `SegmentScanActivity` and `RandomScanActivity` in multi-window mode
- `MobileCouponsParser` is renamed to `TopUpParser`
- added `SimNumberRecognizer` which scans SIM numbers from barcodes

## 6.5.1

- improved multi-window camera support
- fix layouting of default activities in multi-window mode
- improved recognition of issuing authority on Croatian ID cards

## 6.5.0

- added support for Android 7 multi-window mode
- added support for Slovenian UPN QR code
- workaround for camera bug on OnePlus 3T
- fixed autofocus bug on Huawei Honor 8
- fixed black camera on Motorola Moto Z
- made camera focusing more stable on some devices
    - _stable_ means less "jumpy" when searching for focused image

## 6.4.0

- added support for scanning IBAN from Georgia in Segment Scan
- added support for cancelling ongoing DirectAPI recognition call
- added option to allow unverified results for `MRTDRecognizer`:
    - by using method `setAllowUnverifiedResults` in `MRTDRecognizerSettings`,  it is possible to allow obtaining of results with incorrect check digits
- added Belgian account number check to IBAN parser
- added PhotoPay support for scanning Slovak payslips and Slovak Data Matrix codes

## 6.3.0

- removed `RecognizerView` method `setInitialScanningPaused`. For achieving the same functionality, method `pauseScanning` should be used.
- updated `SepaQRRecognizer` to process image frames in the same way as `ZXingRecognizer`. Now, `slowThoroughScan` is enabled by default.
- improved quality of german ID address recognition
- added support for extracting place of birth on old German IDs

## 6.2.1
- added support for scanning IBANs that contain spaces and dashes
- support for scanning Croatian slips that have no amount, but have currency in amount field

## 6.2.0
- fixed camera management on LG X Cam
- added support for scanning Slovenian IDs
- improved IBAN parser
- improved amount parser
- added support for returning optional data beyond the end of SEPA payment QR code
- added support for drawing MRZ detection result
- _LibRecognizer.aar_ renamed to _LibPhotoPay.aar_
- Czech account number is now separated into first (prefix) and second part. First part is not mandatory on czech payslips.

## 6.1.0
- added support for scanning Beneficiary name in slovenian payslips
- added support for scanning PayerID from slip (if missing in OCR line) in hungarian payslips
- added support for scanning SEPA QR codes
- improved czech code and symbol parsing
- fixed rare NPE in SegmentScanActivity
- fixed problematic camera management on One Plus 3

## 6.0.0
- improved `MobileCouponsParser`, added more options to customise parser settings
- added support for scanning front and back side of Serbian ID cards
- added option to request setting of `FLAG_SECURE` on activity window, using intent extras, for all built-in scan activities
    - this flag can be used to prevent users from taking screenshots of the activity window content and to prevent content from being viewed on non-secure displays
- improved support for Czech segment scan:
    - added parser for Czech variabilni symbol
- added support for Hungarian segment scan:
    - Hungarian account number parser
    - Hungarian payer ID parser
- added support for Slovenian segment scan:
    - Slovenian reference parser
- migrated to libc++ native runtime and used clang from NDKr12b for building the native code
    - this enabled c++14 features which will help us yield much better performance in the future

## 5.12.0
- added `MobileCouponsParser` for reading prepaid codes from mobile phone coupons (Croatia)
- `DateParser` returns result as java `Date` object and as original date `String`
- added method `getSpecificParsedResult` to `TemplatingRecognitionResult` (`BlinkOCRRecognitionResult`) which returns specific parser results, e.g. java `Date` for `DateParser`
- improved `MRTDRecognizer`:
    - Dates from MRZ are parsed to java `Date`
    - `MRTDRecognitionResult` can return dates as java `Date` and as original date `String`
    - more accurate and faster detection of MRZ zone
- fixed crashes on  LG F6 and HTC One SV
- added support for scanning front and back side of Slovak ID cards
- added support for scanning front and back side of German ID cards
- improved support for scanning Croatian ID cards
- fixed crashes on Nexus 6 (Android 7.0)

## 5.11.0
- added support for Alberta (Canada) DL
- enabled reading of longer ITF barcodes
- added support for Malaysian visa
- added support for Malaysian work permits
- improved Sweden parsers: SweGiroParser and SweReferenceParser
- improved Macedonian parsers: MkdAccountParser and MkdReferenceParser

## 5.10.1
- improved CroReferenceParser, reference can be parsed when it is followed by dot character

## 5.10.0
- added support for scanning Czech IDs
- renamed BlinkOCRActivity to SegmentScanActivity
- added RandomScanActivity which is similar to SegmentScanActivity but it does not force the user to scan text segments in the predefined order
- fixed CroAmountParser for amounts < 1,00
- improved autofocus support on SGS6 and SGS7
- improved swedish segment scan

## 5.9.0
- fixed IBAN parser on some slovenian slips
- in templating API recognition data does not need to be valid anymore in classification step
- added support for scanning MRZ of green cards
- support for parsing Vehicle Identification Numbers (VINs)
- support for parsing vehicle licence plates

## 5.8.0
- added support for scanning front and back side of Austrian ID cards
- improved swedish amount parser (used in swedish segment scan)
    - now it is much more robust in cases when text comes into scanning area
- improved support for Croatian ID card scanning

## 5.7.0
- Added option to set the OCR document type in BlinkOCREngineOptions
- Introduced new setting options for generic AmountParser:
    - set parser to Arabic-Indic mode (amounts with Arabic-Indic digits)
    - allow amounts with space separated digit groups (thousands)
    - set ideal (expected) number of digits before decimal point
- Added factory method `createFromPreset` to generic AmountParserSettings that creates the settings from one of the available presets (`GENERIC`, `LARGE_AMOUNT`)
- added support for DetectorRecognizer - a recognizer which can perform detection of various documents
    - see `PhotoPayDetectorDemo` app for example on how detectors can be used
- initial support for Slovak PAY by square QR codes
- initial support for Czech payment QR codes
- default scan activity for Dutch OCR line has been replaced with `ScanFovWithInfo` activity
- Dutch payment slip recognizer now supports scanning of recipient name
- updated `BlinkOCRActivity`, through `ScanConfiguration` it is now possible to make some scan field optional (user can skip it) and set the field size
- fixed IBAN extraction on slovenian payslips containing barcode in IBAN field
- added parser for Czech account number
- added support for scanning front and back side of Croatian ID cards
- added support for scanning Austrian payslips that do not have form ID printed (SEPA format is assumed)
- austrian reference parser now supports alphanumeric payment references
- IBAN parser now accepts whitelist of allowed countries
- Date parser can now be configured with allowed date formats

## 5.6.0
- FailedDetectionMetadata, PointsDetectionMetadata and QuadDetectionMetadata have been replaced with DetectionMetadata which now holds a DetectorResult
    - DetectorResult is more flexible as it allows more different detection types to be added in future
- fixed several possible crashes in camera management
- updated to NDK r11
- fixed autofocus bug on LG devices when metering areas or non-default zoom level were set
- fixed autofocus bug on LG G4 (not related to bug above)
- added new options to BlinkOcrEngineOptions
- added RegexParser which can parse almost any regular expression from OCR result
- fixed user interface problems in provided BlinkOCRActivity on devices with low density screens
- support for parsing PlusGiro numbers in Swedish segment scan
    - existing SweBankGiroParserSettings has been renamed to SweGiroParserSettings
    - SweGiroParser can now parse both BankGiro and PlusGiro
    - two slip code digits are appended to SweGiroParser result
- fixed missing last digit (check digit) in SweReferenceParser result


## 5.5.1
- fixed ANR on Samsung Galaxy S3 Mini version

## 5.5.0
- support for scanning custom camera frames via DirectAPI
- fixed bug on some devices causing it to never start scanning if device was not shaken
- improved payment slip detection on low-end devices (fixed regression introduced in v4.6.0)
- increased OCR engine initialisation speed

## 5.4.1
- fixed hang that occurred in slovenian PhotoPay on some poor quality slips
- improved quality and robustness of slovenian reference number parsing
- improved OCR quality of slovenian slip containing courier condensed font
- reconfigureRecognizers method now throws an error if phone does not have autofocus and at least one of new recognizers require it
- raw resources are now packed as assets
- fixed bug with isScanningPaused which sometimes returned bogus value and caused scanning to work even if initial scanning was set to be paused

## 5.4.0
- added support for parsing payer address from croatian payment slip barcode
- better support for scanning some german payslips
- added support for Montenegro segment scan

## 5.3.0
- added support for scanning RF references in slovenian payslips
- support for detecting on activity flip event with OnActivityFlipListener
    - this listener can inform you when activity flip has happened. This is a event which is not notified by Android OS and happens when activity with sensor orientation changes its orientation from landscape to reverse landscape or vice versa
- fixed crash in RecognizerCompatibility class on ARMv7 without NEON
- added RecognizerCompatibility class to javadoc
- added Sony Xperia L to OpenGL blacklist
- fixed NPE in BarcodeDetailedData

## 5.2.0
- improved performance of conversion of Image object into Bitmap
- fixed crash that could be caused by quickly restarting camera activity
- fixed bug in camera layout in certain aspect ratios of camera view
- fixed bug in segment scan when put on landscape activity
- added support for scanning MRTD with wrong checkdigit on date field
- fixed bug in handling setMeteringAreas
- setMeteringAreas now receives boolean indicating whether set areas should be rotated with device
- added support for specifying camera aspect mode from XML
- support for reading amount in hungarian payslips from slip if not present in OCR line
- support for obtaining payer bank and payer account number from hungarian slips (where available)

## 5.1.1
- fixed `NullPointerException` that could occur sometimes in recognition result parcelisation
- fixed occasional ANR on Nexus 4
    - this bug was introduced with Android 5.0.0 update on Nexus 4, it was not present on KitKat and older Android versions
    - other devices were not affected
- fixed bug on Sony and HTC which made it impossible to select and copy text scanned with `BlinkOcrActivity`

## 5.1.0
- fixed crash that happened when detection image metadata was being sent from native to Java
- fixed random behaviour of some recognizers caused by improperly default-initialized RecognizerSettings objects
- added support fo STURRA QR version 002 in german QR recognizer
- improved performance and accuracy of hungarian slip recognition
- silenced annoying "Point array is null" debug log output
- fixed bug that caused nondeterministic refusal to scan anything (this bug occurred mostly on high-end devices)
- fixed bug in scanning 1D barcodes with ZXing when scanning region was set
    - the bug caused not to honor set region
- fixed wrong icon size on `BlinkOCRActivity` on some devices
- when embedding `RecognizerView` into custom scan activity, you no longer need to take care of whether runtime permissions are set or not. You can now simply pass all lifecycle events to `RecognizerView` and if camera permission is denied, a new callback method `onCameraPermissionDenied` of `CameraEventsListener` will be invoked to give you chance to ask user for permissions
    - please refer to updated demo apps for example of new callback
- **IMPORTANT** - `onScanningDone` callback method does not automatically pause scanning loop anymore. As soon as `onScanningDone` method ends, scanning will be automatically resumed without resetting state
    - if you need to reset state, please call `resetRecognitionState` in your implementation of `onScanningDone`
    - if you need to have scanning paused after `onScanningDone` ends, please call `pauseScanning` in your implementation of `onScanningDone`. Do not forget to call `resumeScanning` to resume scanning after it has been paused.
- `pauseScanning` and `resumeScanning` calls are now counted, i.e. if you call `pauseScanning` twice, you will also need to call `resumeScanning` twice to actually resume scanning
    - this is practical if you show multiple onboarding views over camera and you want the scanning paused while each is shown and you do not know in which order they will be dismissed. Now you can simply call `pauseScanning` on showing the onboarding view and `resumeScanning` on dismissing it, regardless of how many onboarding views you have
    - if you want to show onboarding help first time your scan activity starts, you can call `setInitialScanningPaused(true)` which will ensure that first time camera is started, the scanning will not automatically start - you will need to call `resumeScanning` to start scanning after your onboarding view is dismissed
- added support for `x86_64` architecture

## 5.0.0
- new API which is easier to understand, but is not backward compatible. Please check [README](README.md) and updated demo applications for more information.
- removed support for ARMv7 devices which do not support NEON SIMD
    - this enabled us to increase recognition speed at cost of not supporting old devices like those using NVIDIA Tegra 2
    - you can check [this article](https://microblink.zendesk.com/hc/en-us/articles/206113151-Removing-support-for-devices-without-NEON-SIMD-extensions) for more information about NEON and why we use it
- removed support for ARMv6 devices
- added official support for Android 6.0 and it's runtime camera permissions
    - if using provided activities, the logic behind asking user to give camera permission is handled internally
    - if integrating using custom UI, you are required to ask user to give you permission to use camera. To make this easier, we have provided a _CameraPermissionManager_ class which does all heavylifting code about managing states when asking user for camera permission. Refer to demo apps to see how it is used.
- PhotoPay now depends on appcompat-v7 library, instead of full android-support library.
    - even older versions of PhotoPay required only features from appcompat-v7 so we now decided to make appcompat-v7 as dependency because it is much smaller than full support library and is also default dependency of all new Android apps.
- added support for new STUZZA QR codes used in Austria
    - newly supported version is STUZZA version 2.0 which allows empty BIC field in QR code
- added support for scanning Austrian reference number with SegmentScan
    - supported reference number is 12-digit reference number (Kundendaten) as found on Austrian payment slips
- added support for scanning German reference number with SegmentScan
    - supported reference numbers is 13-digit reference number with ISO 7064 checkdigit and RF-reference numbers
- added support for Bosnian SegmentScan
    - it is now possible to scan account numbers and reference numbers from bills issued in Bosnia and Herzegovina
- completely rewritten JNI layer which now gives much lower overhead in communication between Java and native code
    - in our internal tests, this yielded up to 3 times better performance in OCR intensive tasks
- fixed issue with Nexus 5X upside down camera
- when using DirectAPI, recognized Bitmap is not recycled anymore so it can be reused
- added support for reading white hungarian slips
- improved performance of Slovenian amount extraction

## 4.10.1
- fixed bug in slovenian reference parser
    - now supporting slovenian references with more than 20 characters
    - allow dashes inside slovenian reference number

## 4.10.0
- fixed autofocus issue on devices that do not support continuous autofocus
- improved OCR quality when scanning documents with Machine Readable Zone
- improved support for belgian payslips
    - added support for reading recipient name on belgian payslips
    - improved scanning quality of other elements (IBAN, amount, reference) on belgian payslips
- support for defining camera video resolution preset
    - to define video resolution preset via Intent, use `ScanActivity.EXTRAS_CAMERA_VIDEO_PRESET`
    - to define video resolution preset on `RecognizerView`, use method `setVideoResolutionPreset`

## 4.9.0
- added support for reading `Recipient name` in Hungary
- fixed some bugs in camera2 management
- fixed Nexus 6 auto exposure bug
- `MRTD` recognizer optimized for speed

## 4.8.0
- added support for scanning barcodes
- added support for scanning segments with SegmentScan feature
- added support for scanning ID documents
- fixed race condition causing memory leak or rare crashes
- fixed `NullPointerException` in `BaseCameraView.dispatchTouchEvent`
- fixed bug that caused returning scan result from old video frame
- fixed `NullPointerException` in camera2 management
- fixed rare race condition in gesture recognizer
- fixed segmentation fault on recognizer reconfiguration operation
- fixed freeze when camera was being quickly turned on and off
- ensured `RecognizerView` lifecycle methods are called on UI thread
- ensure `onCameraPreviewStarted` is not called if camera is immediately closed after start before the call should have taken place
- ensure `onScanningDone` is not called after `RecognizerView` has been paused, even if it had result ready just before pausing
- when calling `onDisplayOcrResult` callback, make sure OCR char recognition variants are not sent to Java - this is both slow and not required
- fixed behaviour of method `getParsedAmount` - it now returns correct decimal value
- reorganized integration demo apps
    - `PhotoPayDemo` has been expanded and now also shows how to perform segment scan, ID scan and barcode scan via Intent
    - `PhotoPayDemoCustomUI` has remained as it was - it shows how to perform scanning of Croatian payment slips from within custom scanning activity
    - `PhotoPayDemoCustomSegmentScan` is a new demo app similar to `PhotoPayDemoCustomUI`, except it shows how to perform scanning of segments from within custom scanning activity
    - `PhotoPayDirectAPIDemo` shows how to perform scanning of [Android Bitmaps](https://developer.android.com/reference/android/graphics/Bitmap.html) without using Camera

## 4.7.0
- when scanning Croatian QR code, use slower, but more thorough QR code scanning algorithm (this can be disabled via settings)
- `resumeScanningWithoutStateReset` method does not exist anymore - `resumeScanning` now receives `boolean` parameter defining whether or not internal state should be reset
- support for using DirectAPI while RecognizerView is active

## 4.6.0
- new and improved camera management
    - use of Camera2 API on devices that support it
    - new algorithm for choosing best possible frame to be processed
    - support defining camera metering areas on devices that support it
- added support for ARM64 processor ABI

## 4.5.0
- improved error handling on native library initialization
- device specific lists moved to resource - now it is much easier to add new device to blacklist
- disabled offscreen OpenGL initialization on Samsung Galaxy S4 Mini with Android 4.2.2 because of buggy GPU driver

## 4.4.0
- support for defining camera zoom level on android
- various bugfixes and performance improvements

## 4.3.2
- improved Slovenian PhotoPay

## 4.3.1
- added support for Croatian QRCode with optional data

## 4.3.0
- disabled OpenGL accelerated image processing algorithms on devices that have buggy OpenGL driver (Sony Xperia Z on Android 4.1.2 and Motorola Lex 755 on Android 4.2.2)

## 4.2.2
- using SurfaceView instead of TextureView on all devices for displaying camera feed. TextureView does not work on activities that are not HW-accelerated, as is case on some ICS devices.
- dropped support for Android 2.2 because Android NDK r10d deprecated GCC 4.6 and now default GCC 4.8 produces binaries that are incompatible with Android 2.2 (if required by client, we can compile SDK with old NDK while our codebase is still GCC 4.6-compatible)
- german SDK is still built with old GCC

## 4.2.1
- cleared all occurences of strcpy, strcat, setjmp, longjmp, printf, scanf and similar dangerous functions. Those functions were replaced by safer alternatives.
- NOTE: it is possible that those functions are still in use by 3rd party libraries (iconv, OpenCV, TBB, libJpeg and libPng).

## 4.2.0
- added segment scan option to Croatian PhotoPay
- option is available only on request

## 4.1.1
- added support for library license keys

## 4.1.0
- added direct API that enables direct processing of android bitmaps without the need for camera

## 4.0.0
- completely rewritten API for defining settings and obtaining results which is easier to use; see README for details
- optimized library size - native library is now 2 MB per platform smaller than before
- `PhotoPayView` renamed to `RecognizerView`
- added support for Nvidia Tegra 2 devices, whilst preserving NEON acceleration on other ARMv7 devices
- Android studio and gradle are now recommended - demo apps are now only provided for Android studio, Eclipse is supported only via documentation, see README for details

## 3.4.1
- support for tablets that have inverse landscape natural display orientation (currently only Sprint Optik 2 supported)

## 3.4.0
- support for aspect fill camera mode, check README for instructions

## 3.3.1
- fixed race condition at camera initialization on some Android devices (HTC One M8)

## 3.3.0
- added support for embedding PhotoPay into custom activity
- PhotoPayView is now just a camera view that can be embedded anywhere, you just need to pass activity's lifecycle events to it, see README for more details

## 3.2.1
- added workaround for [known Android bug](https://code.google.com/p/android/issues/detail?id=6271)

## 3.2.0
- updated `RecognizerCompatibility.getRecognizerCompatibilityStatus` method - see README for details
- added support for devices without autofocus (only some recognizers support that - see README for more details)
- fixed freezing on Samsung Galaxy Nexus

## 3.1.2
- fixed segfault in OCR flipping process

## 3.1.1
- support for non-standard fonts in UK OCR line

## 3.1.0
- New detector for Germany. Besides old non-SEPA slips, new SEPA slips are now supported.

## 3.0.7
- initial support for OCR of blurred characters applied to ID scanner

## 3.0.6
- added support for German SEPA payslips
- improvements in German payslip scanner
- Kosovo's Utility ID now no longer returns check digit

## 3.0.5
- fixed freezing on Samsung Galaxy S3
- fixed Croatian Reference Model 11 extractor

## 3.0.4
- Fixed whitespace handling in Austrian scanner
- Fixed SIGSEGV on scanning termination when GBuffer is used on Android

## 3.0.3
- Fixes in Austrian Amount and Bank Account number scanning

## 3.0.2
- Tweaks to OCR engine and text recognition
- Bugfixes in Austrian OCR recognition

## 3.0.1
- fixed camera focusing issue on HTC One V
- new OCR line detector for UK Giro slips

## 3.0.0
- optional support for obtaining multiple scan results from single scan
- API change:
    - `onScanningDone` method in BasePhotoPayActivity now receives a list of scanning results instead of single scanning result
    - this list can have zero, one or more scan results
    - if multiple recognizers are turned on and obtaining of multiple scan results from single image is allowed (can be allowed with method `RecognizerSettings.setAllowMultipleScanResultsOnSingleImage`, by default it is disabled), list can have multiple `RecognitionData` elements, otherwise it will have at most one element (similar behaviour as before)
- new key has been added for retrieving list of recognised objects via intent: `BasePhotoPayActivity.EXTRAS_PAYMENT_DATA_LIST`
    - you can obtain the list with following snippet:

```java
            ArrayList<RecognitionData> dataList = getIntent().getExtras().getParcelableArrayList(BasePhotoPayActivity.EXTRAS_PAYMENT_DATA_LIST);
```
- by default, taking screenshots of camera activity is now allowed (until now it was disabled by default and could be overriden with custom implementation of `onConfigureWindow`)
- added support for x86 devices

## 2.5.1
- fixed model and reference parsing when there was no whitespace between the fields on Croatian payslips

## 2.5.0
- reduced library size per processor architecture
- added ARMv7 architecture (much faster on newer phones than ARMv6)
    - see updated README for information how to exclude ARMv6 or ARMv7 binary from your app and all pros and cons of doing that
- `RecognitionData.RECOGNITIONDATA_TYPE` constants are now enumerated in `StringConstants.RecognitionDataType` interface (see updated README)
- support for reading `Payer name` in Croatia
- support for turning reading of `Payer name` and `Payment description` on or off on Croatian slips
- improved algorithm that uses time redundant OCR information for improving OCR quality
- allowing , besides . as amount decimal separator in austrian QR codes

## 2.4.0
- support for Kosovo OCR line and Code128 barcode scanning
- fixed race condition in focus management
- added support for changing camera activity's background color
- added support for optimizing camera parameters for near slip scanning
- fixed camera orientation bug on Samsung Galaxy Ace GT-S5830i

## 2.3.5
- fixed ProGuard warnings when building in release mode (added `EnclosingMethod` attribute to library ProGuard obfuscation rules and updated ProGuard documentation and demo)
- fixed bug in camera preview resolution chooser: when phone does not support any of the required camera preview resolutions, random resolution was chosen

## 2.3.3
- Implemented QR code Reading for HUB3 standard

## 2.3.2
- Croatian reference model now returns HR prefix if it exists on the payslip
- All strings returned by PhotoPay now have ASCII encoding fallback. This means no more "Invalid UTF-8 string" messages should appear in scanning results.

## 2.3.1
- fixed Austrian STUZZA QR code parsing - now supports both LF and CRLF line endings and does not confuse reference number and payment description

## 2.3.0
- improved croatian reference extraction
- determining croatian refernence status

## 2.2.2
- fixed QR code scanning issues when ECI codes were used inside QR code

## 2.2.1
- Improved OCR for Croatian payslips
- Added new field for Croatian scanning "Reference number insecure"
- Fixed issue with returning completely empty results in some cases when scanning timeout occurs

## 2.2.0
- this version does not use R class for referencing resources from within binary jar - this means that PhotopaySdk library project can now be repackaged

## 2.1.0
- support for having title bar and status bar in camera activity
- support for custom activity window configuration (added overridable method `onConfigureWindow` to `BaseCameraActivity`)

## 2.0.0
- new API for custom camera UI
- support for scanning multiple slips without closing camera activity (in custom camera UI only)
- support for controlling torch
- `net.photopay.ocr.PaymentData` renamed to `net.photopay.recognition.RecognitionData`
- removed deprecated getters from `RecognitionData`
- detection statuses moved from `net.photopay.ocr.PaymentRecognizerDelegate` to `net.photopay.recognition.DetectionStatus`
- new utility method for setting PhotoPay language: `net.photopay.locale.LanguageUtils.setLanguage`
- officially supported PhotoPay languages now listed in `net.photopay.locale.Language` enum
- bugfixes

## 1.8.4
- Improved image processing with smarter threshold calculation
- Payment description scanning in croatian slips now more reliable
- Added scanning of recipient address for barcodes on croatian slips
- Faster frame quality estimation
- New ABBYY version
- Other stability improvements and bugfixes

## 1.8.3
- Major UI refactor
- Viewfinder is now GPU accelerated
- Improved focus management, meaning now frames that are coming to OCR are less blurred which results with faster and
more reliable scanning
- New localization files (Slovenian, French)

## 1.8.2
- new QR code features for Austria
- Hardware accelerated viewfinder
- Focus management bugfix

## 1.8.1
- Slovenian version now scans payment description
- Bugfix for scanning with sudden movements of the phone
- Updated strings
- Support for Austrian payslips with nonstandard amount printed on the bottom
- Support for arbitrary length austrian reference numbers

## 1.8.0
- More advanced algorithm for Dictionary

## 1.7.9
- Added Dictionary checking for tokens returned by Ocr Engine

## 1.7.8
- Improvements to barcode recognition for Croatian payslips
- Added sound on successful scan
- Whitespace fix for text recognition
- More improvements to Sieve boosting algorithm
- Added help screen with a message to check results
- Fix for Austrian amounts in format like ==3.600,--

## 1.7.7
- Sieve algorithm improved, results are not boosted between results
- Belegart recognition in Austria improved, detection algorithm for non SEPA slips improved
- Positioning of some elements fixed, especially on wide screen phones
- In Austria, detection frame is now unmovable.

## 1.7.6
- Multiple recognitions of receiver name in Austrian slips
- Implemented basic Sieve method for video OCR algorithm

## 1.7.5
- Autoupdate functionality made more reliable
- Testing app more robust, serial dispatch queue used for saving of usage data
- PDF417 update for Croatian payslips

## 1.7.4
- Added autoupdate functionality

## 1.7.3
- Reading of Prufziffer and Belegnr. for Austrian slips
- Kundendaten is returned on non-Sepa slips. Otherwise reference number is returned.

## 1.7.2
- Added Croatian PDF417 barcode reader

## 1.7.1
- Implemented Slovenian check digit calculators
- Slovenian recognition updated to new image processing algorithm and processing by regions
- Austrian recognition handles tax slips
- Fixed problem with recognition of Austrian BLZs
- Payment description recognized if reference is not present on Austrian slips

## 1.7.0
- Added polishers which fix some obvious gramatical errors in recognition results
- Additional stability and bug fixes

## 1.6.7
- Status messages made more logical
- Improvements to Austrian recognition, text block exstractor and camera management

## 1.6.5
- Upgraded to new ABBYY version.
- Reduced library size

## 1.6.4
- Javadoc for Sdk project is now available from application projects.
- Added solution to autofocus problem on some phones
- Removed Alert view with partial results from UI - it was unscalable to large amounts of recognition data
- Numerous bugfixes for Austrian recognition

## 1.6.2
- Implemented support for API 7 phones

## 1.6.1
- Fixed bug with rendering context accessed from two threads on first run of photopay

## 1.6.0
- Using new image processing algorithm (A1)
- The whole recognition process now goes field by field
- Tweaked UI (added orientation support, slightly changed help screen)

## 1.5.3
- Cleaned up localization files and added German localization

## 1.5.2
- Faster image preprocessing for QR codes
- Fix for hangs in BIC recognition
- Android focus management changes - now focus request come in regular intervals

## 1.5.0
- QR Code recognition made more robust in combination with OCR
- Tweaked Austrian recognition
- Fixed bug in recognition of empty elements

## 1.4.3
- Added device specific recognition parameters and camera options
- Added options for controlling camera frame resolution via Extras parameter
- Improved recognition confidence with reusing subsequent recognition results
- Accelerometer control changes - now high pass filter to get more accurate measurements

## 1.4.0
- Improved detection algorithm generation with internal tools
- Fixed image deformation problem with GPU dewarping

## 1.3.8
- Fixed bug with ROI in image preprocessing

## 1.3.7
- Updated to new version of Ocr engine
- License file for ocr engine now needed to initialize Ocr engine

## 1.3.6
- Added support for reading QR codes

## 1.3.5
- Using single OpenGL context which additionally speeds up the processing and fixes the hanging bug
- Added arbitrary text block extractor
- Updated ACS algorithm

## 1.3.1
- Further improvements to adaptive contrast stretching
- Detection algorithm allows more flexibility

## 1.3.0
- Added adaptive contrast stretching for improving recognition in bad light conditions
- Improved recognition speed with doing ocr by regions
- Fixed a memory leak with patterns file
- Patterns file now use direct memory allocation in Java
- Improved postprocessing of croatian amounts

## 1.2.7
- Added automatic white balance correction for improving detection
- Small corrections to contrast stretching method
- Rectangle sent to recognition now stretched to allow errors in slip printing
- Improved speed of detection algorithm with non constant number of detection iterations

## 1.2.5
- Fixed a problem with detection of croatian slips
- Added accelerometer management which now start AF when the phone is shaking
- Upgraded focus management and switched to MACRO mode
- Changed interface to PhotoPaySdk - now uses onActivityResult method.

## 1.2.2
- Fixed OpenGL shader linking bug on low video memory phones

## 1.2.1
- Fixed viewfinder lines width on some Android phones
- Alert view for partial recognition now not cancelable
- Added reset of recognition results between consecutive partial recognitions
- Added additional checks fot OpenGL

## 1.2.0
- GPU processing made stable
- Detection algorithm made more robust
- OCR speed increase
- Timer for partial recognition view now starts on first failed recognition
- Unified strings with iPhone app

## 1.2.0
- Image preprocessing moved to GPU

## 1.1.1
- Fixed crashes on partial recognitions and in simulator
- Fixed problem with localization when user closes the app on PhotoPay activity
- Small modifications to text message lifecycle

## 1.1.0
- HUB3 standard support
- Added detection of payment form and perspective correction
- Added UI animation
- Added support for continuous autofocus on Android 2.3+ devices
- Added tap to focus support
- Workaround for Android bug which prevents simultaneous use of auto white balance and auto focus.
- Removed source from Library project :), replaced with jar and javadoc
- Target SDK now 2.3.3, min SDK still 2.1.

## 1.0.0
- Basic functionality
