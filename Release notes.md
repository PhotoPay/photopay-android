# Release notes

##5.12.0
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